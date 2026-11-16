# 01 vLLM 架构总览：请求的完整生命周期

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　2,395 字符　｜　7 个片段　｜　更新于 2026-11-13
## 一句话概括
vLLM 的核心贡献不是某个 kernel，而是把**操作系统的虚拟内存思想搬进了 KV Cache 管理**：用固定大小 block 存放 KV、用 block table 做逻辑到物理的映射，从而允许请求间共享物理块、允许非连续存放，把显存碎片从「按最大长度预留」变成「按需分配」。

## 分层结构（V1 之后）
- **入口层**：OpenAI 兼容 HTTP server（api_server），负责协议解析、鉴权、流式 SSE。
- **AsyncLLM**：异步引擎封装，把请求变成 EngineCore 的输入队列消息，并把输出流回推给前端。
- **EngineCore / EngineCoreProc**：真正跑调度与模型执行的核心，V1 中独立进程运行，通过 ZMQ/共享内存与前端的 tokenizer、detokenizer 进程通信。这样做是为了把 Python 侧的调度开销与前端请求处理解耦。
- **Scheduler**：每个 step 决定「这一轮跑哪些请求的哪些 token」。V1 统一了原先 prefill 优先 / decode 优先的多套策略，改为单一调度器 + token budget。
- **KVCacheManager / BlockPool**：管理物理 block 的空闲池、引用计数、前缀缓存命中与驱逐。
- **Worker / Executor**：持有模型权重，执行 forward；单卡是 Worker，多卡由 Executor 组织 TP/PP 进程组。
- **ModelRunner**：准备输入张量（input_ids、positions、slot_mapping、block_tables、seq_lens），调用模型与 attention backend。

## 一次请求的生命周期
1. **入队**：请求到达，tokenize，计算 prompt 长度，加入 waiting 队列。
2. **调度（prefill）**：Scheduler 按 token budget 从 waiting 队列取请求，分配 KV block，生成本轮要算的 token 列表。
3. **执行**：ModelRunner 组装 batch，写 slot_mapping（把每个 token 的 KV 写到对应物理 block 的槽位），跑模型，采样出 token。
4. **调度（decode）**：请求进入 running 队列，每个 step 追加 1 个 token；同时新请求可继续插入（continuous batching）。
5. **抢占（preemption）**：显存不够时，V1 采用 recompute 方式——把被抢占请求的 KV 丢掉，之后重算 prefill（而不是 swap 到 CPU），因为高速互连下重算通常更便宜。
6. **结束**：命中 EOS 或 max_tokens，释放 block（引用计数归零才真正回收），结果 detokenize 后流式返回。

## 为什么要以 block 为单位
- 请求长度未知且差异巨大，按最大长度预留会浪费 60%–80% 显存（PagedAttention 论文的核心观察）。
- 固定大小 block 让**共享**成为可能：同一 prompt 的多个采样、beam search、并行 agent 分支可以指向同一批物理块（写时才复制）。
- 非连续存储让**分配**变成 O(1) 的空闲链表操作，不需要搬迁。

## V1 相对 V0 的关键变化
| 维度 | V0（legacy） | V1 |
|---|---|---|
| 调度 | prefill/decode 两套路径，策略分支多 | 单一 Scheduler，统一 token budget |
| 进程模型 | 单进程内多线程 | EngineCore 独立进程，前后端解耦 |
| 前缀缓存 | 需显式开启 | 默认开启的 automatic prefix caching |
| Chunked prefill | 可选项 | 默认开启 |
| CPU 开销 | 调度与采样占比较高 | zero-overhead scheduling，减少每 step Python 开销 |

## 读源码的入口建议
- 调度：vllm/v1/core/sched/scheduler.py（决定 batch 组成）
- KV：vllm/v1/core/kv_cache_manager.py、block_pool.py（分配、引用计数、驱逐）
- 执行：vllm/v1/worker/gpu_model_runner.py（输入组装、CUDA graph、attention backend 选择）
- 注意力后端：vllm/v1/attention/backends/（flash_attn、flashinfer、triton 等）

## 常见误解
- 「vLLM 快是因为 PagedAttention kernel 快」——不对。分页解决的首先是**显存浪费**，吞吐提升主要来自更大 batch；kernel 只是让分页访问可行。
- 「前缀缓存会自动命中任意公共前缀」——只按 block 边界（16 token）对齐，且依赖 block 级 hash 链，前缀差 1 个 token 就会 miss 到该 block。
- 「chunked prefill 只影响延迟」——它也改变吞吐：把长 prefill 切碎后能与 decode 混批，GPU 利用率更高，但会拉高被插入请求的 ITL。
