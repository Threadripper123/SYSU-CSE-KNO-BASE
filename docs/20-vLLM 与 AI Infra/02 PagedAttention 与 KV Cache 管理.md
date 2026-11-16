# 02 PagedAttention 与 KV Cache 管理

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　3,029 字符　｜　21 个片段　｜　更新于 2026-11-13
## KV Cache 到底占多少显存
每个 token 的 KV 占用公式：

    每 token 字节数 = 2 × 层数 × KV头数 × head_dim × dtype字节数

系数 2 来自 K 和 V 各存一份。以 Llama-3-70B（80 层、GQA 8 个 KV 头、head_dim 128、FP16）为例：

    2 × 80 × 8 × 128 × 2 B = 327,680 B ≈ 320 KiB / token

也就是说 8K 上下文的一个请求约占 2.5 GiB。80GB 卡扣掉权重（70B FP16 约 140GB，实际要 TP）之后，并发能力完全由 KV 决定。**GQA/MQA 把 KV 头数从 64 降到 8，是长上下文服务最重要的模型侧改动之一**，它直接把 KV 显存降 8 倍。

### 显存预算怎么分
- 权重：参数 × dtype 字节数（+ 可能的量化 scale）。
- 激活与 workspace：与最大 batch、最长序列相关，通常几 GB。
- KV Cache：剩下的全部，由 gpu_memory_utilization（vLLM 默认 0.9）划定上限后交给 block pool。
- CUDA graph / NCCL buffer / 碎片：容易被忽略，多卡时 NCCL 会吃掉数百 MB 到 GB。

## PagedAttention 的设计
传统做法按 max_model_len 为每个请求预留连续显存，实际长度远小于上限，浪费 60%–80%。PagedAttention 改为：

- 把 KV Cache 切成固定大小的 **block**（vLLM 默认 16 个 token 一块）。
- 每个 block 只存若干 token 的 K、V，物理上可以不连续。
- 每个序列维护一张 **block table**（逻辑块号 → 物理块号），相当于进程页表。
- 注意力计算时按 block table 做 gather，kernel 直接支持非连续读取。

### 关键数据结构
| 结构 | 作用 |
|---|---|
| block_table[batch, max_blocks] | 每个请求的逻辑块到物理块映射，随请求长度增长 |
| slot_mapping[total_tokens] | 本 step 每个 token 的 KV 应写入的物理槽位（block_id × block_size + offset） |
| seq_lens / query_start_loc | 每个请求的上下文长度与 query 在拼接张量中的偏移 |
| ref_cnt（每 block） | 引用计数，多序列共享同一物理块时 >1，写时复制 |

### 共享与写时复制
- **并行采样 / beam search**：同一 prompt 的多条分支共享 prompt 部分的物理块，只有分叉后的 token 才新分配块（copy-on-write）。论文中这让采样类负载的 KV 显存接近 1/n。
- **前缀缓存**：不同请求的相同前缀块直接复用，ref_cnt 加一。

## prefix caching（自动前缀缓存）
vLLM 的实现要点：

1. 对每个 **满 block** 计算 hash：hash(本块 token) 与 **前驱块的 hash** 链式组合。因此只有从第 0 块开始完全一致的前缀才能命中。
2. 命中查询在 block pool 的 hash→block 索引里做，命中即 ref_cnt++，跳过对应 prefill 计算。
3. 淘汰用 LRU：空闲块按最近使用时间排序，优先复用最久未用的块；正在被引用的块不能淘汰。
4. 粒度是 block（16 token），所以前缀差 1 个 token 会让该 token 所在块及其后全部失效。

**收益**：多轮对话、few-shot 模板、RAG 中固定的系统提示、agent 反复带同一段 context，命中率高时 TTFT 可降一个数量级。
**代价**：hash 计算与索引维护有 CPU 开销；缓存块会挤占可用 KV，低命中率场景反而降低并发。vLLM V1 默认开启，可用 enable_prefix_caching=False 关闭对比。

## 抢占：recompute 还是 swap
显存不足时必须让出资源。两种做法：

| 方式 | 做法 | 代价 |
|---|---|---|
| Recompute（V1 默认） | 丢弃被抢占请求的全部 KV，之后重跑 prefill | 浪费已算的算力；但省掉 PCIe 传输 |
| Swap | 把 KV 拷到 CPU 内存，恢复时拷回 | 受 PCIe 带宽限制（单请求几 GB 时非常慢） |

高速互连（NVLink）与较强算力下，重算通常更快；这也是 vLLM V1 选择 recompute 的原因。

## 一个常被忽略的坑：块内碎片
即使按块分配，**最后一个未填满的块**仍会浪费平均半块（8 token）。当并发请求很多、上下文都很短时，这部分浪费可观。缓解手段：
- 减小 block_size（如 8）→ 碎片少，但 block table 变大、kernel 寻址开销上升。
- 增大 block_size（如 32/64）→ 大模型长上下文下 kernel 效率更高，碎片更多。
经验：通用负载 16 是平衡点；纯长上下文可试 32；极短请求密集场景可试 8。

## 容量估算的实用算法
1. 算单 token KV 字节数（上面的公式）。
2. 可用 KV 显存 = 总显存 × utilization − 权重 − 激活 − 通信 buffer。
3. 可容纳 token 总数 = 可用 KV 字节 / 单 token 字节。
4. 并发请求数 ≈ 可容纳 token 总数 / 平均上下文长度（含输出）。
5. 再乘一个 0.85–0.9 的安全系数（块内碎片 + 调度预留）。

例：单卡 80GB、权重 16GB、激活 4GB、utilization 0.9 → KV 约 72−20 = 52GB；单 token 320KB → 约 16 万 token；平均请求 4K → 约 40 并发。

## 与其他方案的对比
- **连续分配 + swap（早期 FasterTransformer）**：实现简单，碎片严重。
- **vLLM 分页**：碎片低、可共享，但每次注意力都要查 block table，kernel 稍复杂。
- **SGLang RadixAttention**：用基数树管理前缀，命中粒度更细（token 级前缀树），对多轮对话/分支场景命中率更高；vLLM 的块级 hash 链是简化版。

## 相关原始资料
- PagedAttention 论文（SOSP'23）原始抓取文档
- vLLM 设计文档：Paged Attention、PagedAttention CUDA kernel
- vLLM 自动前缀缓存文档
