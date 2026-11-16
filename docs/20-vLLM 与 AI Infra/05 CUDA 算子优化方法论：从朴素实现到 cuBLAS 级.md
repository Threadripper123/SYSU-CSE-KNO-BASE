# 05 CUDA 算子优化方法论：从朴素实现到 cuBLAS 级

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　2,930 字符　｜　12 个片段　｜　更新于 2026-11-13
## 先判断瓶颈，再动手
写任何 kernel 前先算两件事：
1. 这个算子的**算术强度**（FLOPs / 字节）。
2. 目标 GPU 的**拐点算术强度** = 峰值算力 / 峰值带宽。

低于拐点 → 带宽受限，优化目标是减少搬运、提高合并度；高于拐点 → 算力受限，优化目标是提高 Tensor Core 利用率、减少指令开销。搞反方向会白干一整天。

## 通用优化顺序（APOD）
Assess（分析）→ Parallelize（并行化）→ Optimize（优化）→ Deploy（部署）。NVIDIA 官方最佳实践指南的核心结构就是这个，配套工具是 Nsight Systems（时序、谁在等谁）与 Nsight Compute（单 kernel 的 DRAM/Compute 吞吐、stall 原因）。

## 案例：GEMM 的优化阶梯
手写 SGEMM 追 cuBLAS 是最经典的练手路径（siboehm 的 worklog 把它拆成 8 步，最终在 A100/4090 上达到 cuBLAS 的 90% 以上）。每一步解决一个具体瓶颈：

| 阶段 | 做法 | 解决的瓶颈 | 典型现象 |
|---|---|---|---|
| 0 | 每线程算一个 C 元素，直接读 global | — | 极慢，访存完全未合并/重复 |
| 1 | GMEM tiling：block 内复用 A/B 的一小块 | 减少重复读 global | 仍受限于 global 延迟 |
| 2 | SMEM tiling：A/B tile 先搬进 shared memory | global 访问次数降到 1/tile | 需 __syncthreads，bank conflict 出现 |
| 3 | 1D blocktiling：每线程算 TM 个元素（沿一维） | 提高每线程计算量，摊薄访存 | 寄存器压力开始上升 |
| 4 | 2D blocktiling：每线程算 TM×TN 的小块 | 进一步复用寄存器，接近算力上限 | 寄存器可能 spill |
| 5 | 向量化（float4 读 global） | 提高单指令搬运字节数 | 需对齐与尾块处理 |
| 6 | warp tiling + swizzle | 消除 shared memory bank conflict | 接近峰值 |
| 7 | double buffering（cp.async 预取） | 让搬运与计算重叠 | 达到 cuBLAS 90%+ |

**结论**：GEMM 的性能来自「数据复用层次」——寄存器 > 共享内存 > L1 > L2 > HBM。每往下一层，带宽差一个数量级，所以要把能复用的数据尽量往上提。

## 案例：Reduction（归约）
求和/最大值是最基础的通信型 kernel，坑最多：
1. **树形归约**而不是线性累加（O(log n) 步）。
2. **warp 内用 __shfl_down_sync**，不要用 shared memory（省掉同步）。
3. **block 内**：每 warp 先归约，再把每 warp 结果写 shared memory，最后一个 warp 收尾。
4. **避免 bank conflict**：shared 数组按 32 交错或加 padding。
5. **每线程处理多个元素**（grid-stride + 向量化），减少 block 数与启动开销。
6. **注意初始值与数据类型**：FP 归约不要假设结合律无害，长序列累加要用 Kahan 或分块累加保精度。

## 案例：Softmax
- 数值稳定版必须减最大值：exp(x - max)。
- 两遍读（先求 max/sum，再算输出）会多一次 HBM 往返；**online softmax**（FlashAttention 用的技巧）可以在单遍内增量更新 max 与 sum。
- 按行做 softmax 时，一行交给一个 block 或一个 warp，用 warp shuffle 做行内归约；行很长时用「每线程处理多列 + 共享内存交换」。
- 与 mask、scale 融合，避免额外 kernel。

## 案例：LayerNorm / RMSNorm
- 融合成单 kernel：一次读入，算均值方差（或平方和），再写归一化结果。
- 大 hidden size 时用两阶段（block 内 partial reduce + 一次 block 间同步/二次 kernel）。
- RMSNorm 省掉减均值，少一次 reduce，是 LLaMA 系列的默认选择。

## 通用技巧清单
1. **合并访存**：让相邻线程访问相邻地址；必要时用 shared memory 转置。
2. **向量化**：float4 / int4 一次搬 16B。
3. **提高 ILP**：每线程处理多个独立元素，掩盖延迟。
4. **控制寄存器**：__launch_bounds__(threads, minBlocksPerSM)；避免大数组与动态索引导致 spill。
5. **异步流水**：cp.async / TMA + 多 stage buffer，让 global→shared 与计算重叠。
6. **减少同步**：用 warp-level shuffle 替代 shared memory + __syncthreads。
7. **合并 kernel**：elementwise 链（如 residual + norm + dropout）合成一个，省 HBM 往返。
8. **用 CUDA Graph**：固定形状的重复调用（decode）消掉 launch 开销。
9. **profile 而不是猜**：ncu 看 DRAM Throughput、Compute (SM) Throughput、Occupancy、Warp Stall Reasons；如果 DRAM 到 80%+ 就是带宽瓶颈，继续优化计算没意义。

## 什么时候不该自己写
- 通用 GEMM/卷积：直接用 cuBLAS/cuDNN/CUTLASS。
- attention：FlashAttention / FlashInfer 已高度优化。
- 只有在以下情况才值得手写：融合特定算子链、特殊数据布局（paged KV、稀疏）、低精度自定义、以及明确的 profile 证据表明现有实现是瓶颈。

## 学习资源（本库已收录）
- CUDA C++ 最佳实践指南（官方）
- CUDA 入门教程（NVIDIA 博客中文）
- 如何把 CUDA Matmul Kernel 优化到 cuBLAS 级性能（siboehm）
- CUDA 算子手撕与面试代码集 / 面试指南
- CUDA 性能优化实战：并行归约
- GPU MODE 公开课资料
