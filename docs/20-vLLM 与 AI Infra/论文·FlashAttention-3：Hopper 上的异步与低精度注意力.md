# 论文·FlashAttention-3：Hopper 上的异步与低精度注意力

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　342 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2407.08608
抓取: https://ar5iv.labs.arxiv.org/html/2407.08608

FlashAttention-3:
Fast and Accurate Attention with Asynchrony and Low-precision

Jay Shah

Thanks: Equal contribution

  
Ganesh Bikshandi † † footnotemark:

  
Ying Zhang

  
Vijay Thakkar

  
Pradeep Ramani

  
Tri Dao

Affiliation: {jayhshah,ganesh}@colfax-intl.com,
yingz@meta.com, {vithakkar,prraman}@nvidia.com, tri@tridao.me

Abstract

Attention, as a core layer of the ubiquitous Transformer architecture, is the bottleneck for large language models and long-context applications.
FlashAttention elaborated an approach to speed up attention on GPUs through minimizing memory reads/writes.
However, it has yet to take advantage of new capabilities present in recent
hardware, with FlashAttention-2 achieving only 35% utilization on the H100 GPU.
We develop three main techniques to speed up attention on Hopper GPUs: exploiting
asynchrony of the Tensor Cores and TMA to (1) overlap overall computation and data movement via warp-specialization and (2) interleave block-wise matmul and softmax operations, and (3)
block quantization and incoherent processing that leverages hardware support for FP8 low-precision.
We demonstrate that our method, FlashAttention-3, achieves speedup on H100 GPUs
by 1.5-2.0 × \times with FP16 reaching up to 740 TFLOPs/s (75% utilization), and with FP8 reaching close to 1.2
PFLOPs/s.
We validate that FP8 FlashAttention-3 achieves 2.6 × \times lower numerical error than a baseline
FP8 attention.

1 Introduction

For the Transformer architecture [ 59 ], the attention mechanism constitutes the primary computational bottleneck, since computing the self-attention scores of queries and keys has quadratic scaling in the sequence length.
Scaling attention to longer context will unlock new capabilities (modeling and
reasoning over multiple long
documents [ 24, 50, 43 ] and files in
large codebases [ 48, 30 ] ), new modalities (high-resolution
images [ 11 ], audio [ 23 ], video [ 25 ] ), and new applications (user interaction with long history [ 53 ],
agent workflow with long horizon [ 62 ] ).
This has generated significant interest in making attention faster in the long-context regime, including
by approximation [ 27, 14, 56 ] and software optimization
( [ 45, 17, 29 ] ), or even alternative
architectures [ 42, 55, 22 ].

In this work, we build on the work of Dao et al. 2022 on developing exact-attention algorithms that integrate knowledge of the GPU’s execution model and hardware characteristics into their high-level design.
In [ 17 ], Dao et al. introduced FlashAttention, a novel tiling strategy for parallelizing attention that eliminates intermediate reads/writes to slow global memory through fusing all of the attention operations into a single GPU kernel.
Dao 2023 restructured the algorithm as FlashAttention-2 to also parallelize over the sequence length dimension and perform the inner loop of the forward pass over blocks of the key and value matrices, thus improving the occupancy and distribution of work on the GPU.
However, we observe that FlashAttention-2 nonetheless achieves poor utilization on newer GPUs relative to optimized matrix-multiplication (GEMM) kernels, such as 35% vs. 80-90% on the Hopper H100 GPU.
Partially, this may be attributed to implementation-level differences, such as not using Hopper-specific instructions in place of Ampere ones when targeting the Tensor Cores.
Several work such as ThunkerKitten [ 52 ] and cuDNN 9 [ 39 ] has shown that
with Hopper-specific instructions and tile-based abstractions, one can speedup
attention computation and simplify the implementation.

More fundamentally, FlashAttention-2 ’s algorithm adheres to a simplified synchronous model and
makes no explicit use of asynchrony and low-precision in its design.
Asynchrony is a result of hardware specialization to accelerate the most important
operations in a ML workload: specific hardware
units performing matrix multiplication (Tensor Cores) or memory loading
(Tensor Memory Accelerator – TMA), separate from the rest of the CUDA cores performing logic, integer, and floating
point computation.
Low precision such as FP8 in Hopper and FP4 in Blackwell, continuing the trend
of FP16 (Pascal in 2017) and BF16 (Ampere in 2020), is a proven technique to get
double or quadruple throughput for the same power and chip area.
We review the capabilities afforded by Hopper in these directions in Section 2.2.
The technical challenge is to redesign FlashAttention-2 to make use of these hardware
features: asynchrony requires overlapping computation between matmul and softmax
even though one depends on the output of the other, and low-precision requires
care to minimize quantization error, especially in the case of outlier features
in LLMs [ 20, 54 ].

To this end, we propose FlashAttention-3, which contributes and synthesizes three new ideas to further improve performance on newer GPU architectures: 1 1
1

We describe our results in the context of NVIDIA’s Hopper architecture.
However, our algorithm is operative for any GPU architecture with sufficiently robust asynchronous execution and low-precision capabilities.

1.

Producer-Consumer asynchrony: We define a warp-specialized software pipelining scheme that exploits the asynchronous execution of data movement and Tensor Cores by splitting producers and consumers of data into separate warps, thereby extending the algorithm’s ability to hide memory and instruction issue latencies.

2.

Hiding softmax under asynchronous block-wise GEMMs: We overlap the comparatively low-throughput non-GEMM operations involved in softmax, such as floating point multiply-add and exponential, with the asynchronous WGMMA instructions for GEMM.
As part of this, we rework the FlashAttention-2 algorithm to circumvent certain sequential dependencies between softmax and the GEMMs.
For example, in the 2-stage version of our algorithm, while softmax executes on one block of the scores matrix, WGMMA executes in the asynchronous proxy to compute the next block.

3.

Hardware-accelerated low-precision GEMM: We adapt the forward pass algorithm to allow for targeting the FP8 Tensor Cores for GEMM, nearly doubling the measured TFLOPs/s.
This requires bridging the different layout conformance requirements of WGMMA in terms of how blocks of FP32 accumulator and FP8 operand matrices are assumed to be laid out in memory.
We use the techniques of block quantization and incoherent processing to mitigate the loss of accuracy that results from moving to FP8 precision.

To validate our method empirically, we benchmark FlashAttention-3 on the H100 SXM5 GPU over
a range of parameters and show that (1) FP16 achieves 1.5-2.0 × \times speedup over
FlashAttention-2 in the forward pass (reaching up to 740 TFLOPs/s) and 1.5-1.75 × \times in the backward pass,
(2) FP8 achieves close to 1.2 PFLOPs/s, and
(3) for large sequence length, FP16 outperforms and FP8 is
competitive 2 2
2

More precisely, for head dimension 64 FlashAttention-3 FP8 is ahead,
while for head dimensions 128 and 256 it is at par for those cases
without causal masking and behind with causal masking.
with a state-of-the-art implementation of attention from NVIDIA’s cuDNN library.
We also validate that FP16 FlashAttention-3 yields the same numerical error as FlashAttention-2 and is
better than the standard attention implementation as intermediate results (e.g.,
softmax rescaling) are kept in FP32.
Moreover, FP8 FlashAttention-3 with block quantization and incoherent processing is 2.6 × \times more accurate than standard
attention with per-tensor quantization in cases with outlier features.

We open-source FlashAttention-3 with a permissive license 3 3
3

FlashAttention-3 is available at https://github.com/Dao-AILab/flash-attention and plan
to integrate it with
PyTorch and Hugging Face libraries to benefit the largest number of researchers
and developers.

2 Background: Multi-Head Attention and GPU Characteristics

2.1 Multi-Head Attention

Let 𝐐, 𝐊, 𝐕 ∈ ℝ N × d \mathbf{Q},\mathbf{K},\mathbf{V}\in\mathbb{R}^{N\times d} be the query, key and value input sequences associated to a single head, where N N is the sequence length and d d is the head dimension. Then the attention output 𝐎 \mathbf{O} is computed as:

𝐒 = α ​ 𝐐𝐊 ⊤ ∈ ℝ N × N, 𝐏 = softmax ⁡ ( 𝐒 ) ∈ ℝ N × N, 𝐎 = 𝐏𝐕 ∈ ℝ N × d, \mathbf{S}=\alpha\mathbf{Q}\mathbf{K}^{\top}\in\mathbb{R}^{N\times N},\quad\mathbf{P}=\mathrm{softmax}(\mathbf{S})\in\mathbb{R}^{N\times N},\quad\mathbf{O}=\mathbf{P}\mathbf{V}\in\mathbb{R}^{N\times d},

where softmax \mathrm{softmax} is applied row-wise and one typically sets α = 1 / d \alpha=1/\sqrt{d} as the scaling factor.
In practice, we subtract rowmax ⁡ ( 𝐒 ) \mathrm{rowmax}(\mathbf{S}) from 𝐒 \mathbf{S} to prevent numerical instability with the exponential function.
For multi-head attention (MHA), each head has its own set of query, key and value projections, and this computation parallelizes across multiple heads and batches to produce the full output tensor.

Now let ϕ \phi be a scalar loss function and let 𝐝 ⁡ ( − ) = ∂ ϕ / ∂ ( − ) \mathbf{d}(-)=\partial\phi/\partial(-) be notation for the gradient.
Given the output gradient 𝐝𝐎 ∈ ℝ N × d \mathbf{dO}\in\mathbb{R}^{N\times d}, we compute 𝐝𝐐 \mathbf{dQ}, 𝐝𝐊 \mathbf{dK}, and 𝐝𝐕 \mathbf{dV} according to the chain rule as follows:

𝐝𝐕 \displaystyle\mathbf{dV}

= 𝐏 ⊤ ​ 𝐝𝐎 ∈ ℝ N × d \displaystyle=\mathbf{P}^{\top}\mathbf{dO}\in\mathbb{R}^{N\times d}

𝐝𝐏 \displaystyle\mathbf{dP}

= 𝐝𝐎𝐕 ⊤ ∈ ℝ N × N \displaystyle=\mathbf{dO}\mathbf{V}^{\top}\in\mathbb{R}^{N\times N}

𝐝𝐒 \displaystyle\mathbf{dS}

= dsoftmax ⁡ ( 𝐝𝐏 ) ∈ ℝ N × N \displaystyle=\mathrm{dsoftmax}(\mathbf{dP})\in\mathbb{R}^{N\times N}

𝐝𝐐 \displaystyle\mathbf{dQ}

= α ​ 𝐝𝐒𝐊 ∈ ℝ N × d \displaystyle=\alpha\mathbf{dS}\mathbf{K}\in\mathbb{R}^{N\times d}

𝐝𝐊 \displaystyle\mathbf{dK}

= α ​ 𝐝𝐒 ⊤ ​ 𝐐 ∈ ℝ N × d, \displaystyle=\alpha\mathbf{dS}^{\top}\mathbf{Q}\in\mathbb{R}^{N\times d},

Here, we have that 𝐝 ​ s = ( diag ⁡ ( p ) − p ​ p ⊤ ) ​ 𝐝 ​ p \mathbf{d}s=(\mathrm{diag}(p)-pp^{\top})\mathbf{d}p for p = softmax ⁡ ( s ) p=\mathrm{softmax}(s) as a function of a vector s s, and we write dsoftmax ⁡ ( 𝐝𝐏 ) \mathrm{dsoftmax}(\mathbf{dP}) for this formula applied row-wise.
Finally, this computation again parallelizes across the number of heads and batches for the backward pass of MHA.

2.2 GPU hardware characteristics and execution model

We describe the aspects of the GPU’s execution model relevant for FlashAttention-3, with a focus on the NVIDIA Hopper architecture as a concrete instantiation of this model.

Memory hierarchy:

The GPU’s memories are organized as a hierarchy of data locales, with capacity inversely related to bandwidth ( Table 1 ) 4 4
4

Luo et al. 2024 reports shared memory bandwidth of 128 bytes per clock cycle per SM, and we multiply that by 132 SMs and the boost clock of 1830 MHz..
Global memory (GMEM), also known as HBM, is the off-chip DRAM accessible to all streaming multiprocessors (SMs).
Data from GMEM gets transparently cached into an on-chip L2 cache.
Next, each SM contains a small on-chip, programmer-managed highly banked cache called shared memory (SMEM).
Lastly, there is the register file within each SM.

Thread hierarchy:

The GPU’s programming model is organized around logical groupings of execution units called threads.
From the finest to coarsest level, the thread hierarchy is comprised of threads, warps (32 threads), warpgroups (4 contiguous warps), threadblocks (i.e., cooperative thread arrays or CTAs), threadblock clusters (in Hopper), and grids.

These two hierarchies are closely interlinked.
Threads in the same CTA are co-scheduled on the same SM, and CTAs in the same cluster are co-scheduled on the same GPC.
SMEM is directly addressable by all threads within a CTA, whereas each thread has at most 256 registers (RMEM) private to itself.

Table 1: Thread-Memory hierarchy for the NVIDIA Hopper H100 SXM5 GPU.

Hardware Level

Parallel Agent

Data Locale

Capacity @ Bandwidth

Chip

Grid

GMEM

80 GiB @ 3.35 TB/s

GPC

Threadblock Clusters

L2

50 MiB @ 12 TB/s

SM

Threadblock (CTA)

SMEM

228 KiB per SM, 31TB/s per GPU

Thread

Thread

RMEM

256 KiB per SM

Asynchrony and warp-specialization:

GPUs are throughput processors that rely on concurrency and asynchrony to hide memory and execution latencies.
For async memory copy between GMEM and SMEM, Hopper has the Tensor Memory Accelerator (TMA) as a dedicated hardware unit [ 38, §7.29].
Furthermore, unlike prior architectures such as Ampere, the Tensor Core of Hopper, exposed via the warpgroup-wide WGMMA instruction [ 40, §9.7.14], is also asynchronous and can source its inputs directly from shared memory.

Hardware support for asynchrony allows for warp-specialized kernels, where the warps of a CTA are divided into producer or consumer roles that only ever issue either data movement or computation.
Generically, this improves the compiler’s ability to generate optimal instruction schedules [ 4 ].
In addition, Hopper supports the dynamic reallocation of registers between warpgroups via setmaxnreg [ 40, §9.7.17.1], so those warps doing MMAs can obtain a larger share of RMEM than those just issuing TMA (for which only a single thread is needed).

Low-precision number formats:

Modern GPUs have specialized hardware units for accelerating low-precision computation.
For example, the WGMMA instruction can target the FP8 Tensor Cores on Hopper to deliver 2x the throughput per SM when compared to FP16 or BF16.

However, correctly invoking FP8 WGMMA entails understanding the layout constraints on its operands.
Given a GEMM call to multiply A × B ⊤ A\times B^{\top} for an M × K M\times K -matrix A A and an N × K N\times K -matrix B B, we say that the A A or B B operand is mn-major if it is contiguous in the outer M M or N N dimension, and k-major if is instead contiguous in the inner K K -dimension.
Then for FP16 WGMMA, both mn-major and k-major input operands are accepted for operands in SMEM, but for FP8 WGMMA, only the k-major format is supported.
Moreover, in situations such as attention where one wants to fuse back-to-back GEMMs in a single kernel, clashing FP32 accumulator and FP8 operand layouts pose an obstacle to invoking dependent FP8 WGMMAs.

In the context of attention, these layout restrictions entail certain modifications to the design of an FP8 algorithm, which we describe in Section 3.3.

2.3 Standard Attention and Flash Attention

Following Dao et al. 2022, we let standard attention denote an implementation of attention on the GPU that materializes the intermediate matrices 𝐒 \mathbf{S} and 𝐏 \mathbf{P} to HBM. The main idea of FlashAttention was to leverage a local version of the softmax reduction to avoid these expensive intermediate reads/writes and fuse attention into a single kernel. Local softmax corresponds to lines 18 - 19 of the consumer mainloop in Algorithm 1 together with the rescalings of blocks of 𝐎 \mathbf{O}. The simple derivation that this procedure indeed computes 𝐎 \mathbf{O} can be found in [ 15, §2.3.1].

3 FlashAttention-3: Algorithm

In this section, we describe the FlashAttention-3 algorithm. For simplicity, we focus on the
forward pass, with the backward pass algorithm described in Section B.1. We first indicate how to integrate warp-specialization with a circular SMEM buffer into the base algorithm of FlashAttention-2. We then explain how to exploit asynchrony of WGMMA to define an overlapped GEMM-softmax 2-stage pipeline. Finally, we describe the modifications needed for FP8, both in terms of layout conformance and accuracy via block quantization and incoherent processing.

3.1 Producer-Consumer asynchrony through warp-specialization and
pingpong scheduling

Warp-specialization

As with FlashAttention-2, the forward pass of FlashAttention-3 is embarrassingly parallel in the batch size, number of heads, and query sequence length.
Thus, it will suffice to give a CTA-level view of the algorithm, which operates on a tile 𝐐 i \mathbf{Q}_{i} of the query matrix to compute the corresponding tile 𝐎 i \mathbf{O}_{i} of the output.
To simplify the description, we first give the warp-specialization scheme with a circular SMEM buffer that does not have in addition the GEMM-softmax overlapping.
Let d d be the head dimension, N N the sequence length, and fix a query block size B r B_{r} to divide 𝐐 \mathbf{Q} into T r = ⌈ N B r ⌉ T_{r}=\lceil\frac{N}{B_{r}}\rceil blocks 𝐐 1,.., 𝐐 T r \mathbf{Q}_{1},..,\mathbf{Q}_{T_{r}}.

Algorithm 1 FlashAttention-3 forward pass without intra-consumer overlapping – CTA view

0:

 Matrices 𝐐 i ∈ ℝ B r × d \mathbf{Q}_{i}\in\mathbb{R}^{B_{r}\times d} and 𝐊, 𝐕 ∈ ℝ N × d \mathbf{K},\mathbf{V}\in\mathbb{R}^{N\times d} in HBM, key block size B c B_{c} with T c = ⌈ N B c ⌉ T_{c}=\lceil\frac{N}{B_{c}}\rceil.

1:

 Initialize pipeline object to manage barrier synchronization with s s -stage circular SMEM buffer.

2:

  if in producer warpgroup then

3:

  Deallocate predetermined number of registers.

4:

  Issue load 𝐐 i \mathbf{Q}_{i} from HBM to shared memory.

5:

  Upon completion, commit to notify consumer of the load of 𝐐 i \mathbf{Q}_{i}.

6:

   for 0 ≤ j < T c 0\leq j<T_{c} do

7:

   Wait for the ( j % ​ s ) (j\,\%\,s) th stage of the buffer to be consumed.

8:

   Issue loads of 𝐊 j, 𝐕 j \mathbf{K}_{j},\mathbf{V}_{j} from HBM to shared memory at the ( j % ​ s ) (j\,\%\,s) th stage of the buffer.

9:

   Upon completion, commit to notify consumers of the loads of 𝐊 j, 𝐕 j \mathbf{K}_{j},\mathbf{V}_{j}.

10:

   end for

11:

  else

12:

  Reallocate predetermined number of registers as function of number of consumer warps.

13:

  On-chip, initialize 𝐎 i = ( 0 ) ∈ ℝ B r × d \mathbf{O}_{i}=(0)\in\mathbb{R}^{B_{r}\times d} and ℓ i, m i = ( 0 ), ( − ∞ ) ∈ ℝ B r \ell_{i},m_{i}=(0),(-\infty)\in\mathbb{R}^{B_{r}}.

14:

  Wait for 𝐐 i \mathbf{Q}_{i} to be loaded in shared memory.

15:

   for 0 ≤ j < T c 0\leq j<T_{c} do

16:

   Wait for 𝐊 j \mathbf{K}_{j} to be loaded in shared memory.

17:

   Compute 𝐒 i ( j ) = 𝐐 i ​ 𝐊 j T \mathbf{S}_{i}^{(j)}=\mathbf{Q}_{i}\mathbf{K}_{j}^{T} (SS-GEMM). Commit and wait.

18:

   Store m i old = m i m_{i}^{\mathrm{old}}=m_{i} and compute m i = max ⁡ ( m i old, rowmax ⁡ ( 𝐒 i ( j ) ) ) m_{i}=\mathrm{max}(m_{i}^{\mathrm{old}},\mathrm{rowmax}(\mathbf{S}_{i}^{(j)})).

19:

   Compute 𝐏 ~ i ( j ) = exp ⁡ ( 𝐒 i ( j ) − m i ) \widetilde{\mathbf{P}}_{i}^{(j)}=\mathrm{exp}(\mathbf{S}_{i}^{(j)}-m_{i}) and ℓ i = exp ⁡ ( m i old − m i ) ​ ℓ i + rowsum ⁡ ( 𝐏 ~ i ( j ) ) \ell_{i}=\mathrm{exp}(m_{i}^{\mathrm{old}}-m_{i})\ell_{i}+\mathrm{rowsum}(\widetilde{\mathbf{P}}_{i}^{(j)}).

20:

   Wait for 𝐕 j \mathbf{V}_{j} to be loaded in shared memory.

21:

   Compute 𝐎 i = diag ​ ( exp ⁡ ( m i old − m i ) ) − 1 ​ 𝐎 i + 𝐏 ~ i ( j ) ​ 𝐕 j \mathbf{O}_{i}=\mathrm{diag}(\mathrm{exp}(m_{i}^{\mathrm{old}}-m_{i}))^{-1}\mathbf{O}_{i}+\widetilde{\mathbf{P}}_{i}^{(j)}\mathbf{V}_{j} (RS-GEMM). Commit and wait.

22:

   Release the ( j % ​ s ) (j\,\%\,s) th stage of the buffer for the producer.

23:

   end for

24:

  Compute 𝐎 i = diag ​ ( ℓ i ) − 1 ​ 𝐎 i \mathbf{O}_{i}=\mathrm{diag}(\ell_{i})^{-1}\mathbf{O}_{i} and L i = m i + log ⁡ ( ℓ i ) L_{i}=m_{i}+\log(\ell_{i}).

25:

  Write 𝐎 i \mathbf{O}_{i} and L i L_{i} to HBM as the i i th block of 𝐎 \mathbf{O} and L L.

26:

  end if

For our implementation of Algorithm 1 on Hopper, we use setmaxnreg for (de)allocations, TMA for loads of 𝐐 i \mathbf{Q}_{i} and { 𝐊 j, 𝐕 j } 0 ≤ j < T c \{\mathbf{K}_{j},\mathbf{V}_{j}\}_{0\leq j<T_{c}}, and WGMMA to execute the GEMMs in the consumer mainloop, with the SS or RS prefix indicating whether the first operand is sourced from shared memory or register file.
For interpreting the execution flow of Algorithm 1, note that issuing TMA loads does not stall on the completion of other loads due to asynchrony.
Moreover, in the producer mainloop, no waits will be issued for the first s s iterations as the buffer gets filled.

Pingpong scheduling

The asynchronous nature of WGMMA and TMA, along with warp-specialization, opens
up the opportunity to overlap the softmax computation of one warpgroup with the GEMM of
another warpgroup.
To motivate this, notice that non-matmul operations have much lower throughput
than matmul operations on modern hardware accelerators.
As an example, the H100 SXM5 GPU has 989 TFLOPS of FP16 matmul but only 3.9
TFLOPS of special functions such as exponential 5 5
5

The CUDA programming
guide specifies that 16 operations of special functions can be performed per
streaming multiprocessor (SM) per clock cycle. We multiply 16 by 132 SMs and
1830 MHz clock speed to get 3.9 TFLOPS of special functions. (necessary for softmax).
For the attention forward pass in FP16 with head dimension 128, there are 512x more matmul FLOPS
compared to exponential operations, but the exponential has 256x lower
throughput, so exponential can take 50% of the cycle compared to matmul.
The situation is even worse with FP8, where the matmul throughput doubles but
the exponential throughput stays the same.

Since the exponential is performed by a separate hardware unit (the multi-function
unit), ideally we’d want the exponential calculation to be scheduled when the
Tensor Cores are performing the matmul.
To do so, we use synchronization barriers ( bar.sync instructions) to
force the GEMMs (GEMM1 – 𝐏𝐕 \mathbf{P}\mathbf{V} of one iteration, and GEMM0 – 𝐐𝐊 ⊤ \mathbf{Q}\mathbf{K}^{\top}
of the next iteration) of warpgroup 1 to be scheduled before the GEMMs of
warpgroup 2.
As a result, the softmax of warpgroup 1 will be scheduled while warpgroup 2 is
performing its GEMMs. Then the roles swap, with warpgroup 2 doing softmax while
warpgroup 1 doing GEMMs (hence, “pingpong” scheduling).
This is illustrated in Fig. 1.
Though in practice the pingpong scheduling is not as clean as depicted in the
figure, we generally find this to improve performance (e.g., from 570 TFLOPS to
620-640 TFLOPS for FP16 forward with head dimension 128 and sequence length 8192).

Figure 1 : Pingpong scheduling for 2 warpgroups to overlap softmax and GEMMs: the softmax of one warpgroup
should be scheduled when the GEMMs of another warpgroup are running. The
same color denotes the same iteration.

Attention variants

For multi-query attention [ 51 ] and grouped query
attention [ 3 ], we follow the approach in FlashAttention-2 and adjust the
tensor indexing to avoid duplicating 𝐊 \mathbf{K} and 𝐕 \mathbf{V} in HBM.

3.2 Intra-warpgroup overlapping GEMMs and softmax

Even within one warpgroup, we can overlap some instructions in the softmax with
some instructions in the GEMMs. We describe one technique to do so.

In the attention algorithm, operations within the inner loop (main loop) have sequential dependencies that impede parallelization within a single iteration.
For example, (local) softmax (lines 18 to 19 ) relies on the output 𝐒 i ( j ) \mathbf{S}_{i}^{(j)} of the first GEMM, while the second GEMM takes its result 𝐏 ~ i ( j ) \widetilde{\mathbf{P}}_{i}^{(j)} as an operand.
Indeed, the wait statements in lines 17 and 21 of Algorithm 1 serialize the execution of softmax and GEMMs.
However, we can break these dependencies by pipelining across iterations through additional buffers in registers.
Pursuing this idea, we propose the following two-stage 6 6
6

Note that the number of stages of the overlapping scheme is bounded by, but need not equal, the number s s of stages in the circular SMEM buffer. GEMM-softmax pipelining algorithm:

Figure 2 : 2-stage WGMMA-softmax pipelining

Algorithm 2 FlashAttention-3 consumer warpgroup forward pass

0:

 Matrices 𝐐 i ∈ ℝ B r × d \mathbf{Q}_{i}\in\mathbb{R}^{B_{r}\times d} and 𝐊, 𝐕 ∈ ℝ N × d \mathbf{K},\mathbf{V}\in\mathbb{R}^{N\times d} in HBM, key block size B c B_{c} with T c = ⌈ N B c ⌉ T_{c}=\lceil\frac{N}{B_{c}}\rceil.

1:

 Reallocate predetermined number of registers as function of number of consumer warps.

2:

 On-chip, initialize 𝐎 i = ( 0 ) ∈ ℝ B r × d \mathbf{O}_{i}=(0)\in\mathbb{R}^{B_{r}\times d} and ℓ i, m i = ( 0 ), ( − ∞ ) ∈ ℝ B r \ell_{i},m_{i}=(0),(-\infty)\in\mathbb{R}^{B_{r}}.

3:

 Wait for 𝐐 i \mathbf{Q}_{i} and 𝐊 0 \mathbf{K}_{0} to be loaded in shared memory.

4:

 Compute 𝐒 cur = 𝐐 i ​ 𝐊 0 T \mathbf{S}_{\mathrm{cur}}=\mathbf{Q}_{i}\mathbf{K}_{0}^{T} using WGMMA. Commit and wait.

5:

 Release the 0 0 th stage of the buffer for 𝐊 \mathbf{K}.

6:

 Compute m i m_{i}, 𝐏 ~ cur \tilde{\mathbf{P}}_{\mathrm{cur}} and ℓ i \ell_{i} based on 𝐒 cur \mathbf{S}_{\mathrm{cur}}, and rescale 𝐎 i \mathbf{O}_{i}.

7:

  for 1 ≤ j < T c − 1 1\leq j<T_{c}-1 do

8:

  Wait for 𝐊 j \mathbf{K}_{j} to be loaded in shared memory.

9:

  Compute 𝐒 next = 𝐐 i ​ 𝐊 j T \mathbf{S}_{\mathrm{next}}=\mathbf{Q}_{i}\mathbf{K}_{j}^{T} using WGMMA. Commit but do not wait.

10:

  Wait for 𝐕 j − 1 \mathbf{V}_{j-1} to be loaded in shared memory.

11:

  Compute 𝐎 i = 𝐎 i + 𝐏 ~ cur ​ 𝐕 j − 1 \mathbf{O}_{i}=\mathbf{O}_{i}+\tilde{\mathbf{P}}_{\mathrm{cur}}\mathbf{V}_{j-1} using WGMMA. Commit but do not wait.

12:

  Wait for the WGMMA 𝐐 i ​ 𝐊 j T \mathbf{Q}_{i}\mathbf{K}_{j}^{T}.

13:

  Compute m i m_{i}, 𝐏 ~ next \tilde{\mathbf{P}}_{\mathrm{next}} and ℓ i \ell_{i} based on 𝐒 next \mathbf{S}_{\mathrm{next}}.

14:

  Wait for the WGMMA 𝐏 ~ cur ​ 𝐕 j − 1 \tilde{\mathbf{P}}_{\mathrm{cur}}\mathbf{V}_{j-1} and then rescale 𝐎 i \mathbf{O}_{i}

15:

  Release the ( j % ​ s ) (j\,\%\,s) th, resp. ( j − 1 % ​ s ) (j-1\,\%\,s) th stage of the buffer for 𝐊 \mathbf{K}, resp. 𝐕 \mathbf{V}.

16:

  Copy 𝐒 next \mathbf{S}_{\mathrm{next}} to 𝐒 cur \mathbf{S}_{\mathrm{cur}}.

17:

  end for

18:

 Wait for 𝐕 T c − 1 \mathbf{V}_{T_{c}-1} to be loaded in shared memory.

19:

 Compute
𝐎 i = 𝐎 i + 𝐏 ~ last ​ 𝐕 T c − 1 \mathbf{O}_{i}=\mathbf{O}_{i}+\tilde{\mathbf{P}}_{\mathrm{last}}\mathbf{V}_{T_{c}-1} using WGMMA. Commit and wait.

20:

 Epilogue:
Rescale 𝐎 i \mathbf{O}_{i} based on m i m_{i}.
Compute L i L_{i} based on m i m_{i} and ℓ i \ell_{i}.
Write 𝐎 i \mathbf{O}_{i} and L i L_{i} to HBM as the i i -th block of 𝐎 \mathbf{O} and L L.

Algorithm 2 functions as a replacement for the consumer path of Algorithm 1 to comprise the complete FlashAttention-3 algorithm for FP16 precision. At a high-level, we use WGMMA as a metonym for asynchronous GEMM. Within the mainloop (lines 8 to 16 ), the second WGMMA operation of iteration j j (line 11 ) is overlapped with softmax operations from iteration j + 1 j+1 (line 13 ).

While the pipelined structure illustrated above offers theoretical performance gains, there are several practical aspects to consider:

Compiler reordering

The pseudocode represents an idealized execution order but the compiler (NVCC) often rearranges instructions for optimization.
This can disrupt the carefully crafted WGMMA and non-WGMMA operation pipelining sequence, potentially leading to unexpected behavior or diminished performance gains. An analysis of the SASS code shows that the compiler generates overlapped code as expected (Section B.2 ).

Register pressure

To maintain optimal performance, register spilling should be minimized.
However, the 2-stage pipeline requires additional registers to store intermediate results and maintain context between stages.
Specifically, an extra 𝐒 next \mathbf{S}_{\mathrm{next}} must be kept in registers, leading to extra register usage of size B r × B c × sizeof ​ ( float ) B_{r}\times B_{c}\times\text{sizeof}(\text{float}) per threadblock.
This increased register demand may conflict with using larger block sizes (another common optimization), which is also register-hungry.
In practice, trade-offs should be made based on profiling results.

3-stage pipelining

Extending the 2-stage algorithm described above, we propose a 3-stage variant
that would further overlap the second WGMMA with softmax.
While this approach offers the potential for even higher Tensor Core utilization,
it requires even more registers due to an additional stage in the pipeline,
making the trade-off between tile size and pipeline depth more difficult to balance.
A detailed description of the 3-stage algorithm and its evaluation results can be found in Section B.3.

3.3 Low-precision with FP8

T0 {d0, d1} T1 {d0, d1} T0 {d4, d5} T1 {d4, d5} T2 {d0, d1} T3 {d0, d1} T2 {d4, d5} T3 {d4, d5} T0 {d2, d3} T1 {d2, d3} T0 {d6, d7} T1 {d6, d7} T2 {d2, d3} T3 {d2, d3} T2 {d6, d7} T3 {d6, d7}
Figure 3 : FP32 accumulator register WGMMA layout – rows 0 and 8, threads 0-3, entries 0-7.

T0 {a0, a1} T0 {a2, a3} T1 {a0, a1} T1 {a2, a3} T2 {a0, a1} T2 {a2, a3} T3 {a0, a1} T3 {a2, a3} T0 {a4, a5} T0 {a6, a7} T1 {a4, a5} T1 {a6, a7} T2 {a4, a5} T2 {a6, a7} T3 {a4, a5} T3 {a6, a7}
Figure 4 : FP8 operand A register WGMMA layout – rows 0 and 8, threads 0-3, entries 0-7.

Efficiency: layout transformations.
Computing the forward pass of FlashAttention-3 in FP8 precision poses additional challenges not encountered for FP16 in terms of layout conformance.

First, we note that the input tensors 𝐐 \mathbf{Q}, 𝐊 \mathbf{K}, and 𝐕 \mathbf{V}
are typically given as contiguous in the head dimension,
while to satisfy the k-major constraint on FP8 WGMMA for the second GEMM we need 𝐕 \mathbf{V},
or rather the tiles of 𝐕 \mathbf{V} loaded into SMEM, to be contiguous in the sequence length dimension.
Since the TMA load itself cannot change the contiguous dimension, we then need to either
(1) transpose 𝐕 \mathbf{V} in GMEM as a pre-processing step, or
(2) do an in-kernel transpose of tiles of 𝐕 \mathbf{V} after loading them into SMEM.
To implement option (1), we can either
(1a) fuse the transpose to the epilogue of a preceding step such as the rotary embedding, or
(1b) call a standalone pre-processing transpose
kernel 7 7
7

An optimized transpose kernel will achieve speed near the bandwidth of the device [ 46 ].
to exchange the strides of the sequence length and head dimensions.
However, (1a) is difficult to integrate into a standard library,
and (1b) is too wasteful in a memory-bound situation such as inference.

Instead, for FP8 FlashAttention-3 we opt for option (2).
For the in-kernel transpose, we take advantage of the LDSM ( ldmatrix )
and STSM ( stmatrix ) instructions,
which involve a warp of threads collectively loading SMEM to RMEM
and storing RMEM to SMEM at a granularity of 128 bytes. 8 8
8

In the PTX documentation, LDSM/STSM are described as copying 8 × 8 8\times 8 matrices with 16-bit entries [ 40, §9.7.13.4.15-16],
but we can pack 8-bit entries two at a time to use LDSM/STSM in the context of FP8 precision.
However, the transpose versions of LDSM/STSM cannot split packed 8-bit entries,
which necessitates certain register movements in between LDSM and STSM to actually perform a tile-wise transpose; we omit the details.
The LDSM/STSM instructions are both register efficient,
allowing us to execute them in the producer warpgroup,
and capable of transposing layouts when doing memory copy.
Moreover, after the first iteration we can arrange
for the transpose of the next 𝐕 \mathbf{V} tile
to be executed in the shadow of the two WGMMAs
that involve the preceding 𝐕 \mathbf{V} and current 𝐊 \mathbf{K} tile.

Second, we observe that unlike with FP16,
the memory layout of the FP32 accumulator of an FP8 WGMMA is different
from that assumed for its operand A when held in registers.
We depict fragments of these two layouts in Fig. 3 and Fig. 4,
where the entries are held in registers per thread in the listed order.
By using byte permute instructions,
we can then transform the first WGMMA’s accumulator into a format suitable for the second WGMMA,
and compatibly with the layout of the 𝐕 \mathbf{V} tile produced by the in-kernel transpose. Specifically, with reference to Fig. 3, we change the order in sequence to

{ d0 d1 d4 d5 d2 d3 d6 d7 }, \{\verb|d0 d1 d4 d5 d2 d3 d6 d7|\},

and this register permutation is then replicated over every 8 bytes. In terms of the logical shape of the 𝐏 \mathbf{P} tile, this manuever permutes its columns (e.g., columns 0189 0189 now become the first four columns). For WGMMA to then compute the correct output tile, we can correspondingly arrange for the in-kernel transpose to write out a matching row permutation of the 𝐕 \mathbf{V} tile. 9 9
9

This additional freedom afforded by doing the in-kernel transpose eliminates having to use shuffle instructions to change register ownership across threads, which we previously described in [ 7 ].

Accuracy: block quantization and incoherent processing.
With FP8 (e4m3) format, one only uses 3 bits to store the mantissa and 4 bits
for the exponent.
This results in higher numerical error than FP16/BF16.
Moreover, large models typically have outlier values [ 20, 54 ] that are much larger in magnitude than most other values,
making quantization difficult.
One typically use per-tensor scaling [ 37 ] by keeping one scalar per tensor (e.g., one
for 𝐐 \mathbf{Q}, for 𝐊 \mathbf{K}, and for 𝐕 \mathbf{V} ).
To reduce the numerical error of attention in FP8, we employ two techniques:

1.

Block quantization : we keep one scalar per block, so that
for each of 𝐐 \mathbf{Q}, 𝐊 \mathbf{K}, 𝐕 \mathbf{V} we split the tensor into blocks of
size B r × d B_{r}\times d or B c × d B_{c}\times d and quantize them separately.
This quantization can be fused with an operation right before attention (e.g.,
rotary embedding) with no additional slow down (since rotary embedding is
memory-bandwidth bound).
As the FlashAttention-3 algorithm naturally operates on blocks, we can scale each block of
𝐒 \mathbf{S} to account for this block quantization at no computation cost.

2.

Incoherent processing : to even out outliers, we multiply
𝐐 \mathbf{Q} and 𝐊 \mathbf{K} with a random orthogonal matrix 𝐌 \mathbf{M} before quantizing to
FP8. Since 𝐌 \mathbf{M} is orthogonal, 𝐌𝐌 ⊤ = I \mathbf{M}\mathbf{M}^{\top}=I and so ( 𝐐𝐌 ) ​ ( 𝐊𝐌 ) ⊤ = 𝐐𝐊 ⊤ (\mathbf{Q}\mathbf{M})(\mathbf{K}\mathbf{M})^{\top}=\mathbf{Q}\mathbf{K}^{\top}, i.e., multiplying both 𝐐 \mathbf{Q} and 𝐊 \mathbf{K} with
𝐌 \mathbf{M} does not change the attention output.
This serves to “spread out” the outliers since each entry of 𝐐𝐌 \mathbf{Q}\mathbf{M}
or 𝐊𝐌 \mathbf{K}\mathbf{M} is a random sum of entries of 𝐐 \mathbf{Q} or 𝐊 \mathbf{K}, thus reducing
quantization error.
In practice, we follow Chee et al. 2024 and Tseng et al. 2024 and choose 𝐌 \mathbf{M} to be the product of random diagonal matrices of ± 1 \pm 1 and a Hadamard matrix, which can be multiplied in O ⁡ ( d ​ log ⁡ d ) O(d\log d) instead of
O ⁡ ( d 2 ) O(d^{2}), and can also be fused with the rotary embedding at no extra computation cost.

We validate that these two techniques reduces numerical error by up to 2.6 × \times in Section 4.3.

4 Empirical Validation

We use the primitives from CUTLASS [ 57 ] such as WGMMA
and TMA abstractions to implement FlashAttention-3 and evaluate its efficiency and accuracy.

•

Benchmarking attention.
We measure the runtime of FlashAttention-3 across different sequence lengths and
compare it to a standard implementation in PyTorch,
FlashAttention-2, FlashAttention-2 in Triton (which uses H100-specific instructions),
as well as a vendor’s implementation of FlashAttention-2 optimized for H100 GPUs
from cuDNN.
We confirm that FlashAttention-3 is up to 2.0 × \times faster than FlashAttention-2 and 1.5 × \times
faster than FlashAttention-2 in Triton.
FlashAttention-3 reaches up to 740 TFLOPs/s, 75% of the theoretical maximum TFLOPs/s
on H100 GPUs.

•

Ablation study. We confirm that our algorithmic improvements with warp-specialization and GEMM-softmax pipelining contribute to the speedup of FlashAttention-3.

•

Accuracy of FP8 attention. We validate that block quantization and incoherent processing reduces the numerical error of FP8 FlashAttention-3 by 2.6 × \times.

4.1 Benchmarking Attention

We measure the runtime of different attention methods on an H100 80GB SXM5 GPU
for different settings (without / with causal mask, head dimension 64 or 128) for FP16 inputs.
We report the results
in Fig. 5 and Fig. 6, showing that FlashAttention-3 is around 1.5-2.0 × \times faster
than FlashAttention-2 in the forward pass and 1.5-1.75 × \times faster in
the backward pass.
Compared to a standard attention implementation, FlashAttention-3 can be up
to 3-16 × \times faster.
For medium and long sequences (1k and above), FlashAttention-3 even surpasses the speed of a vendor’s library (cuDNN – closed source) that has been optimized for H100 GPUs.

Benchmark settings:

We vary the sequence length as 512, 1k, …, 16k, and set
batch size so that the total number of tokens is 16k.
We set the hidden dimension to 2048, and head dimension to be either 64, 128, or 256
(i.e., 32 heads, 16 heads, or 8 heads).
To calculate the FLOPs of the forward pass, we use:

4 ⋅ seqlen 2 ⋅ head dimension ⋅ number of heads. 4\cdot\text{seqlen}^{2}\cdot\text{head dimension}\cdot\text{number of heads}.

With causal masking, we divide this number by 2 to account for the fact that
approximately only half of the entries are calculated.
To get the FLOPs of the backward pass, we multiply the forward pass FLOPs by 2.5
(since there are 2 matmuls in the forward pass and 5 matmuls in the backward
pass, due to recomputation).

(a) Forward, without causal mask, head dim 64

(b) Forward, with causal mask, head dim 64

(c) Forward, without causal mask, head dim 128

(d) Forward, with causal mask, head dim 128

(e) Forward, without causal mask, head dim 256

(f) Forward, with causal mask, head dim 256

Figure 5 : Attention forward speed (FP16/BF16) on H100 GPU

(a) Backward, without causal mask, head dim 64

(b) Backward, without causal mask, head dim 128

Figure 6 : Attention backward speed (FP16/BF16) on H100 GPU

We also measure the runtime for FP8 for the forward pass under similar settings.
We report the results for headdim 256 in Fig. 7 and give the full results in Section C.2.

(a) Forward, without causal mask, head dim 256

(b) Forward, with causal mask, head dim 256

Figure 7 : Attention forward speed (FP8) on H100 GPU

4.2 Ablation Study: 2-Stage Pipelining Experiments

We ablate both the 2-stage WGMMA-softmax pipelining and warp-specialization for non-causal FP16 FlashAttention-3 with fixed parameters { batch, seqlen, nheads, hdim } = { 4, 8448, 16, 128 } \{\text{batch},\text{seqlen},\text{nheads},\text{hdim}\}=\{4,8448,16,128\}.
The result in Table 2 confirms that our algorithmic improvements (asynchrony with warp-specialization and overlapping between GEMM and softmax) lead to significant speedup, from 570 to 661 TFLOPs.

Table 2 : Pipelining ablation measurements

Configuration

Time

TFLOPs/s

FlashAttention-3

3.538 ms

661

No GEMM-Softmax Pipelining, Warp-Specialization

4.021 ms

582

GEMM-Softmax Pipelining, No Warp-Specialization

4.105 ms

570

4.3 Numerical Error Validation

As there has been interest in the numerical error [ 21 ] of
FlashAttention, we compare FlashAttention-2, FlashAttention-3, and a standard implementation of attention against a
reference implementation in FP64.
To simulate outlier features and activations in LLMs [ 20, 54 ], we generate the entries of 𝐐, 𝐊, 𝐕 \mathbf{Q},\mathbf{K},\mathbf{V} with the following
distribution:

𝒩 ⁡ ( 0, 1 ) + 𝒩 ⁡ ( 0,100 ) ⋅ Bernoulli ⁡ ( 0.001 ). \mathcal{N}(0,1)+\mathcal{N}(0,100)\cdot\mathrm{Bernoulli}(0.001).

That is, each entry is normally distributed with zero mean and standard
deviation 1, but for 0.1% of entries we add an independent term that’s normally distributed
with standard deviation 10.
We then measure the root mean squared error (RMSE) in Table 3.
In FP16, both FlashAttention-2 and FlashAttention-3 achieves 1.7 × \times lower RMSE compared to the standard implementation since intermediate results (softmax) are kept in FP32.
The baseline attention in FP8 uses per-tensor scaling, with matmul accumulator in FP32 and intermediate softmax results kept in FP16.
Thanks to block quantization and incoherent processing, FlashAttention-3 in FP8 is 2.6 × \times more accurate than this baseline.

Table 3 : Numerical error comparisons in FP16 and FP8 (e4m3).

Method

Baseline FP16

FlashAttention-2 FP16

FlashAttention-3 FP16

RMSE

3.2e-4

1.9e-4

1.9e-4

Method

Baseline FP8

FlashAttention-3 FP8

No block quant

No incoherent processing

RMSE

2.4e-2

9.1e-3

9.3e-3

2.4e-2

5 Dicussion, Limitations, Conclusion

With FlashAttention-3, we have demonstrated that new programming techniques and hardware
features such as asynchrony and low-precision can have a dramatic impact on the
efficiency and accuracy of attention.
We are able to speed up attention by 1.5-2.0 × \times times compared to FlashAttention-2, and
reduce FP8 numerical error by 2.6 × \times compared to standard per-tensor quantization.
Some limitations of our work that we hope to address in the future include: optimizing for
LLM inference, integrating a persistent kernel design into the FP8 kernel, 10 10
10

For our benchmarks, FP16 FlashAttention-3 has a persistent kernel and load balancing strategy, while FP8 FlashAttention-3 does not. This partly explains why FP8 FlashAttention-3 does not perform as well for small sequence length and causal masking compared to the FP8 cuDNN kernels. and understanding the effects of low-precision attention in
large-scale training.
Though we have focused on Hopper GPUs in this work, we expect that the
techniques developed here will apply to other hardware accelerators.
We hope that a faster and more accurate primitive such as attention will unlock
new applications in long-context tasks.

Acknowledgments

We are grateful to the NVIDIA CUTLASS team (especially Haicheng Wu, Aniket
Shivam, and Cris Cecka) for helping us understand Hopper’s
programming model and for their library, which provides clean and powerful building blocks for the implementation of FlashAttention-3.
We thank the cuDNN team for the idea of in-kernel transpose for FP8.
The idea of overlapping GEMMs and softmax was inspired by insightful
conversations with Christopher Ré, Benjamin Spector, Aniket Shivam, and Markus Hoehnerbach.
The pingpong scheduling is adapted from the warp-specialized pingpong GEMM
implementation in CUTLASS.
We appreciate Driss Guessous for integrating FlashAttention to PyTorch.
FlashAttention-3 has benefited from helpful discussions with Horace He on different attention
variants, with Hao Liu and Phil Wang on distributed attention, and with Daniel
Haziza and Chris De Sa on quantization.
We thank Meta, Together AI, and Princeton Language and Intelligence (PLI) for compute support.

References

Abdelfattah et al. [2016]

Ahmad Abdelfattah, Azzam Haidar, Stanimire Tomov, and Jack Dongarra.

Performance, design, and autotuning of batched gemm for gpus.

pages 21–38, 06 2016.

ISBN 978-3-319-41320-4.

doi: 10.1007/978-3-319-41321-1_2.

AI21 [2024]

AI21.

Introducing jamba: Ai21’s groundbreaking ssm-transformer model.

AI21 blog, 2024.

Ainslie et al. [2023]

Joshua Ainslie, James Lee-Thorp, Michiel de Jong, Yury Zemlyanskiy, Federico
Lebrón, and Sumit Sanghai.

Gqa: Training generalized multi-query transformer models from
multi-head checkpoints.

arXiv preprint arXiv:2305.13245, 2023.

Bauer et al. [2011]

Michael Bauer, Henry Cook, and Brucek Khailany.

CudaDMA: Optimizing GPU Memory Bandwidth via Warp
Specialization.

In Proceedings of 2011 International Conference for High
Performance Computing, Networking, Storage and Analysis, SC ’11, New York,
NY, USA, 2011. Association for Computing Machinery.

ISBN 9781450307710.

doi: 10.1145/2063384.2063400.

URL https://doi.org/10.1145/2063384.2063400.

Beck et al. [2024]

Maximilian Beck, Korbinian Pöppel, Markus Spanring, Andreas Auer,
Oleksandra Prudnikova, Michael Kopp, Günter Klambauer, Johannes
Brandstetter, and Sepp Hochreiter.

xlstm: Extended long short-term memory.

arXiv preprint arXiv:2405.04517, 2024.

Beltagy et al. [2020]

Iz Beltagy, Matthew E Peters, and Arman Cohan.

Longformer: The long-document transformer.

arXiv preprint arXiv:2004.05150, 2020.

Bikshandi and Shah [2024]

Ganesh Bikshandi and Jay Shah.

Delivering 1 PFLOP/s of Performance with FP8
FlashAttention-2, 2024.

URL
https://research.colfax-intl.com/adding-fp8-to-flashattention/.

Brandon et al. [2023]

William Brandon, Aniruddha Nrusimha, Kevin Qian, Zachary Ankner, Tian Jin,
Zhiye Song, and Jonathan Ragan-Kelley.

Striped attention: Faster ring attention for causal transformers.

arXiv preprint arXiv:2311.09431, 2023.

Chee et al. [2024]

Jerry Chee, Yaohui Cai, Volodymyr Kuleshov, and Christopher M De Sa.

Quip: 2-bit quantization of large language models with guarantees.

Advances in Neural Information Processing Systems, 36, 2024.

Chen et al. [2021]

Beidi Chen, Tri Dao, Eric Winsor, Zhao Song, Atri Rudra, and Christopher Ré.

Scatterbrain: Unifying sparse and low-rank attention.

In Advances in Neural Information Processing Systems
(N
