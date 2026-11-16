# 06 Attention 算子：FlashAttention 与 PagedAttention 实现要点

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　2,626 字符　｜　16 个片段　｜　更新于 2026-11-13
# 06 Attention 算子实现要点

## 朴素 attention 为什么慢
标准实现要物化三个中间矩阵并把它们写回 HBM：

    S = Q K^T        (N × N)
    P = softmax(S)   (N × N)
    O = P V          (N × d)

以 N = 8192、d = 128、FP16 为例，单个 head 的 S 就是 8192×8192×2B = 128MB。写一遍、读两遍，HBM 流量是纯算力需求的几十倍。对长上下文，瓶颈完全在显存带宽而不是 FLOPs。

## FlashAttention 的核心：分块 + online softmax
不物化 N×N，而是：

1. 把 Q 切成长度为 Br 的块留在 SRAM（或寄存器）里。
2. 沿序列维迭代 K/V 块（Bc）。
3. 每处理一块，增量更新三个量：
   - 当前最大 m（running max）
   - 当前指数和 l（running sum）
   - 当前输出累加 O
4. 用新的 m 对已有的 O 与 l 做重缩放（乘以 exp(m_old − m_new)）。

这样只需 O(N) 额外显存，且 HBM 只读写 O(N·d) 的数据，**数学上与原式完全等价**（是精确注意力，不是近似）。

### 数值细节
- 必须减最大值防溢出；online 版本要求重缩放能被证明等价，这是 FlashAttention 的关键推导。
- 累加用 FP32，输入输出 BF16/FP16。
- causal mask 在块级处理：完全在 mask 外的块直接跳过（省一半 FLOPs）。

### 版本演进
| 版本 | 关键改动 |
|---|---|
| v1 | 分块 + online softmax，只在 batch/head 维度并行 |
| v2 | 减少非 matmul FLOPs；沿序列维也并行；更好的 warp 划分，A100 上 2× 左右 |
| v3 | Hopper：TMA + wgmma 异步流水，FP8 支持，接近理论峰值 |
| FlashDecoding | 长上下文 decode 时沿 KV 维 split 并行，解决 batch 小、序列长时并行度不足 |

## 服务场景的两个特殊形态
### 1. Paged / 分页 KV
vLLM 的 KV 存放在不连续的 block 里，attention kernel 必须：
- 输入 block_table、seq_len，按块迭代读取 K/V；
- prefill 时把新算的 K/V 通过 slot_mapping 分散写到物理槽位；
- 支持 GQA（多个 query head 共享一组 KV head），通常用 packed layout 把 KV head 复制成 query head 数以避免 gather。

vLLM 文档里「Paged Attention」设计页就是这个 kernel 的图解；实现有 FlashAttention、FlashInfer、Triton 等多个后端。

### 2. Cascade / 共享前缀
当大批请求共享同一前缀（系统提示、RAG 文档），可以只对共享部分算一次 attention，再与各自的部分合并（FlashInfer 的 cascade 机制）。这与前缀缓存互补：缓存省计算，cascade 省重复读 KV。

## 三种 attention 的瓶颈画像
| 场景 | 形态 | 瓶颈 | 优化重点 |
|---|---|---|---|
| Prefill（长 prompt） | 大 Q 块 × 长 KV | 算力（Tensor Core） | 提高 MMA 占比、causal 块跳过、FP8 |
| Decode（单 token） | 1 × 长 KV | 显存带宽（读整个 KV） | KV 量化、GQA、split-KV 并行、更大 batch |
| 投机解码验证 | k 个 token 并行验证 | 算力 + 带宽混合 | 批量 mask、树形 attention |

**关键数字感**：decode 一个 token 要把整个 KV 读一遍。7B 模型 32K 上下文、FP16 KV 约 4GB —— 单请求 ¬decode 的理论下限就是 4GB / 3.35TB/s ≈ 1.2ms（H100）。这就是为什么长上下文 decode 主要靠 KV 量化和更大 batch 摊薄。

## 精度与稳定性
- BF16 比 FP16 动态范围大，长序列 attention 更稳；FP16 需要更小心 scaling。
- FP8 attention 需要 per-tile 或 per-head 的 scale，且在 softmax 前的 logits 上要做 clamp。
- 与训练不同，推理的 attention 只需前向，但 KV cache 的量化误差会随上下文长度累积，长上下文场景要评估。

## 实现检查清单（自研或改 kernel 时）
1. 是否避免物化 N×N？没有就先去学 FlashAttention。
2. causal mask 是否按块跳过？
3. GQA 是否做了 head 打包，避免每步 gather？
4. KV 是否按 block 访问，block table 的加载是否走 shared memory？
5. 是否用了 cp.async / TMA 做 K/V 预取与计算重叠？
6. decode 时是否 split-KV 以提高并行度？
7. 数值：累加是否 FP32？online softmax 的重缩放是否无溢出？

## 本库相关原始资料
- FlashAttention 论文（2205.14135）
- 在 NVIDIA CUDA Tile 中把 Flash Attention 调优到峰值性能
- 在 CUDA C++ 中为 5090 写出接近峰值的 Flash Attention
- FlashInfer 介绍、vLLM PagedAttention kernel 设计文档
- FlashAttention 系列技术详解（中文）
