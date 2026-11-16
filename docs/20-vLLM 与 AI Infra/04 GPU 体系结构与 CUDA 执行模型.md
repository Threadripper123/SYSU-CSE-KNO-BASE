# 04 GPU 体系结构与 CUDA 执行模型

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　3,276 字符　｜　15 个片段　｜　更新于 2026-11-13
## 设计哲学：用并行度换延迟
CPU 用大缓存、乱序执行、分支预测把单线程延迟压到最低；GPU 反过来——用海量线程和切换来**隐藏**延迟。所以 GPU 的性能问题几乎总是「并行度不够」或「访存不合并」，而不是「单条指令慢」。

## 硬件层次
### SM（Streaming Multiprocessor）
一张 A100 有 108 个 SM，H100 SXM 有 132 个。每个 SM：
- 4 个处理块（sub-partition），各有一个 warp scheduler；
- 每个 scheduler 每周期发射 1 条 warp 指令（Volta 之后每 scheduler 有独立 PC）；
- 大量寄存器：A100 每 SM 65536 个 32-bit 寄存器（256KB）；
- 共享内存 / L1：A100 每 SM 192KB 可配置（共享内存最多 164KB），H100 228KB；
- Tensor Core：Volta 起每 SM 8 个（Ampere 4 个更大的第三代）。

### 执行单位
- **warp = 32 个线程**，以 warp 为单位调度，同一 warp 内线程执行同一条指令（SIMT）。
- 分支发散时，不同分支串行执行，掩码屏蔽掉不参与的线程 —— 这是性能杀手之一。
- block 内的 warp 可共享 shared memory 并用 __syncthreads() 同步；block 之间无法同步（除非用 cooperative groups / grid sync）。

### Occupancy（占用率）
occupancy = 驻留 warp 数 / SM 支持的最大 warp 数。它由三件事共同限制：
1. 每线程寄存器数（编译期决定，可用 __launch_bounds__ 约束）；
2. 每 block 的共享内存；
3. 每 block 的线程数与每 SM 的 block 数上限。

occupancy 不是越高越好：高 occupancy 能隐藏访存延迟，但可能因寄存器不足导致 spill（局部内存，落到 L1/L2，性能暴跌）。计算密集 kernel 常常 25%–50% occupancy 反而更快。

## 内存层次与带宽
| 层级 | 容量（A100/H100） | 延迟量级 | 带宽 |
|---|---|---|---|
| 寄存器 | 256KB/SM | 1 cycle | 极高 |
| 共享内存 | 164KB / 228KB per SM | ~20–30 cycle | ~19 TB/s（每 SM） |
| L1 | 与共享内存共享物理空间 | ~30 cycle | — |
| L2 | 40MB / 50MB | ~200 cycle | ~5–7 TB/s |
| HBM | 40/80GB、80GB | ~400–600 cycle | 1.55–2.0 TB/s（A100）、3.35 TB/s（H100） |
| 主机内存（PCIe 4.0 x16） | — | 微秒 | ~25–32 GB/s 单向 |

**关键推论**：从 HBM 读 1 字节的时间，够做几十次浮点运算。所以算术强度低的 kernel（elementwise、decode 阶段的 GEMV）注定是 memory-bound，优化方向是「少搬数据」；算术强度高的（大 GEMM、prefill）才值得优化计算。

## 访存合并（coalescing）
一个 warp 的 32 个线程访问全局内存时，硬件把地址合并成尽量少的 32B sector / 128B cache line 事务：
- **合并**：thread i 访问 base + i × 4B → 一次 128B 事务搞定。
- **未合并**：thread i 访问 base + i × stride（stride 不是 1）× 4B → 需要 32 次事务，带宽利用率可能只有 1/32。
- 矩阵按行存（row-major）时，让**相邻线程读相邻列**；做转置类访问时用 shared memory 做 tile 中转。

## 共享内存 bank conflict
- 共享内存分 32 个 bank，每 bank 宽 4B，地址按 4B 交错映射。
- 同一 warp 内若多个线程访问**同一 bank 的不同地址**，会串行化（bank conflict）。
- 经典解法：padding。例如把 shared[32][32] 改成 shared[32][33]，让按列访问时错开 bank。

## Tensor Core
- 一条 MMA 指令完成一个小矩阵乘加（如 m16n8k16），输入在寄存器/共享内存，累加器通常是 FP32。
- Ampere：mma.sync；Hopper：wgmma（warpgroup，异步，直接从 shared memory 读）；Blackwell：tcgen05（更强调异步与 TMEM）。
- 使用 Tensor Core 的前提是数据布局符合要求（ldmatrix、swizzle），这也是手写高性能 GEMM 最繁琐的部分。
- CUTLASS / cuBLAS / Triton 帮你处理这些细节；自己写要先确认值得。

## 一个 kernel 的性能上限（Roofline）
    可达性能 = min(峰值算力, 峰值带宽 × 算术强度)
    算术强度 = FLOPs / 搬运字节数

例：A100 FP16 Tensor 312 TFLOPS、HBM 2.0 TB/s → 拐点算术强度 ≈ 156 FLOP/B。低于它的 kernel 无论怎么写都受带宽限制。

## 常见性能反模式
1. **未合并的全局访存**（结构体数组 AoS、跨步访问）。
2. **低并行度**：grid 太小，SM 空闲；或每线程工作量太少，启动开销占比高。
3. **分支发散**：warp 内走不同分支。
4. **过多 __syncthreads()**：把并行度切成串行段。
5. **寄存器 spill**：局部数组、过大 unroll、动态索引。
6. **把该放 shared memory 的数据反复从 HBM 读**。
7. **忽略 PCIe**：host↔device 拷贝同步等待，用 pinned memory + 流式重叠。

## CUDA 编程模型的几个实用点
- **grid-stride loop**：让 kernel 适配任意数据规模，同时保持 block 数 = SM 数 × 每 SM block 数。
- **异步拷贝**：cp.async（Ampere）把 global→shared 的搬运与计算重叠；Hopper 的 TMA 进一步把多维拷贝交给硬件。
- **流与图**：多流重叠不同 kernel；CUDA Graph 消除 launch 开销（decode 场景关键）。
- **错误检查**：kernel 异步，必须 cudaDeviceSynchronize 或 cudaGetLastError 才能看到错误。

## 与 LLM 推理的对应关系
- **prefill**：大 GEMM + 长序列 attention → compute-bound，吃 Tensor Core 与 FlashAttention。
- **decode**：GEMV（batch 小）+ 增量 attention → memory-bound，吃权重量化与 KV 量化、更大 batch。
- 这解释了为什么「同一张卡，prefill 吞吐和 decode 吞吐差一个数量级」。
