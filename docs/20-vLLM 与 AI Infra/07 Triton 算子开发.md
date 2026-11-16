# 07 Triton 算子开发

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　2,799 字符　｜　15 个片段　｜　更新于 2026-11-13
## 它解决什么问题
手写 CUDA 高性能 kernel 要处理 shared memory 分配、bank conflict、warp 划分、异步拷贝、寄存器压力。Triton 把这些交给编译器，你只需要描述**块级（block-level）的数据流**：

    @triton.jit
    def add_kernel(x_ptr, y_ptr, out_ptr, n, BLOCK: tl.constexpr):
        pid = tl.program_id(0)
        offs = pid * BLOCK + tl.arange(0, BLOCK)
        mask = offs < n
        x = tl.load(x_ptr + offs, mask=mask)
        y = tl.load(y_ptr + offs, mask=mask)
        tl.store(out_ptr + offs, x + y, mask=mask)

一个 program 处理一个 block；tl.arange 表达并行维度；mask 处理边界。编译器负责向量化、合并访存、shared memory 布局与流水。

## 与 CUDA / CUTLASS 的定位对比
| 维度 | CUDA C++ | Triton |
|---|---|---|
| 抽象层级 | 线程级 | 块级 |
| 代码量 | 多 | 少（通常 1/3 到 1/10） |
| 峰值性能 | 可到极限 | 常见 80%–95%，极端优化不如手写 |
| 调试 | 成熟工具链 | 较弱，靠 interpreter 模式 |
| 适合 | 极致融合、特殊布局 | 快速迭代、研究型 kernel、融合算子 |

## 三个必读实例（官方教程）
### 1. 融合 Softmax
- 一行一个 program，行内用 tl.max / tl.sum 做归约。
- 行长度超过一个 block 时用两阶段：先求每块的部分 max/sum，再合并。
- 数值稳定：先减行最大值。
- 与朴素实现相比，融合后省掉多次 HBM 往返，带宽受限场景提升明显。

### 2. 矩阵乘法
核心是三层循环的分块映射：

    for k in range(0, K, BLOCK_K):
        a = tl.load(A + ...)   # BLOCK_M × BLOCK_K
        b = tl.load(B + ...)   # BLOCK_K × BLOCK_N
        acc += tl.dot(a, b)    # 用 Tensor Core

关键调优点：
- **BLOCK_M/N/K**：太小 → 复用不足；太大 → 寄存器溢出。
- **group_m（L2 swizzle）**：按 group 重排 program 顺序，提高 L2 命中。
- **num_warps / num_stages**：num_stages 控制软件流水（预取几层），是性能关键。
- **边界处理**：用 mask 或 padding，避免每步都判断。

官方教程的 matmul 在 A100 上通常能到 cuBLAS 的 80%–95%；后续教程再加 persistent kernel、TMA、warp specialization 继续逼近。

### 3. 低内存 Dropout / LayerNorm / RMSNorm
都是「读一次、算归约、写一次」的融合型 kernel，Triton 写起来非常短，也常被 vLLM 等框架采用。

## 实用参数与坑
1. **autotune**：用 @triton.autotune 扫 (BLOCK, num_warps, num_stages)，但要注意首次运行会编译所有配置，冷启动很慢；生产环境应把结果固定下来。
2. **constexpr**：把 BLOCK 之类写成 tl.constexpr，编译器才能做常量折叠与展开。
3. **divisibility hint**：tl.multiple_of / tl.max_contiguous 告诉编译器对齐信息，显著影响向量化。
4. **动态 shape 重编译**：每次 shape 变化都可能触发重新编译；serving 场景要把 shape 分桶或 padding。
5. **调试**：设 TRITON_INTERPRET=1 用 CPU 语义执行，配合 print 定位；性能问题仍要看 ncu。
6. **数值差异**：归约顺序与 cuBLAS 不同，浮点结果会有微小差异，测试要用容差。
7. **编译缓存**：TRITON_CACHE_DIR 可复用，容器里要注意持久化。

## 在 LLM 推理中的实际位置
- **vLLM**：部分归一化、激活、MoE 路由与 fused MoE、部分 attention 后端用 Triton 实现；也有专门的 Triton attention backend。
- **torch.compile**：Inductor 后端把图降成 Triton kernel，是 PyTorch 生态里 Triton 最大的使用者。
- **昇腾/国产卡**：Triton Ascend 把同一套前端映射到 Ascend C，降低了跨硬件移植成本（本库收录了中文开发指南）。
- **FlashInfer / 各类研究 kernel**：大量用 Triton 快速验证注意力变体。

## 什么时候选 Triton
选它：
- 要融合一串 elementwise / 归约算子；
- 要实现论文里的新 attention / MoE 变体，先跑通再谈极致；
- 团队没有 CUDA 专家，但需要可维护的 kernel。

不选它：
- 已经接近硬件极限、需要 warp specialization / TMA 手调；
- 编译器版本不稳定、需要长期维护的生产 kernel；
- 目标硬件没有成熟后端。

## 学习路径
1. 官方 tutorials：vector add → fused softmax → matmul → layernorm → fused attention。
2. 读 vLLM 里的 Triton kernel 源码（rms_norm、fused_moe）。
3. 用 ncu 对比 Triton 与 CUDA 实现的 DRAM 吞吐与 occupancy，理解编译器做了什么。
