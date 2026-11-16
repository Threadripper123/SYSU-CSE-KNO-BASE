# 在 NVIDIA CUDA Tile 中把 Flash Attention 调优到峰值性能

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　31,994 字符　｜　396 个片段　｜　更新于 2026-11-13
来源: https://developer.nvidia.com/blog/tuning-flash-attention-for-peak-performance-in-nvidia-cuda-tile

# https://developer.nvidia.com/blog/tuning-flash-attention-for-peak-performance-in-nvidia-cuda-tile

来源: https://developer.nvidia.com/blog/tuning-flash-attention-for-peak-performance-in-nvidia-cuda-tile
抓取方式: read_page

DEVELOPER

Home
Blog
Forums
Docs
Downloads
Training

Join

Technical Blog

Subscribe

Related Resources

Developer Tools & Techniques

English 中文

Tuning Flash Attention for Peak Performance in NVIDIA CUDA Tile

Mar 05, 2026

By Alessandro Morari, Allen Zhao, Ivan Yin and Vishal Mehta

Like

Discuss (0)

L

T

F

R

E

AI-Generated Summary

Flash Attention eliminates the need to materialize the full N×N attention matrix by tiling computation, using online softmax, and fusing operations into a single kernel pass, delivering 2-4x speedups and significant memory savings.
An implementation using cuTile Python demonstrates a production-ready kernel that supports causal masking and grouped-query attention across NVIDIA Ampere, Ada, and Blackwell architectures.
Naively increasing tile size from 64×64 to 256×128 degraded performance by 18-43% due to compute bottlenecks and reduced occupancy, illustrating that tile size and compute efficiency are interdependent.
Applying fast math flags such as flush_to_zero and approximate rounding to exp2 and division operations rescued the larger tiles, matching or exceeding the baseline and adding 10-20% gains for longer sequences.
Splitting the K-loop into unmasked, partially masked, and skipped phases provided the largest single optimization, improving throughput by up to 32% across all sequence lengths by avoiding unnecessary masking work.
An autotuner that benchmarks multiple tile configurations per input shape achieved a final 1.60x-1.66x speedup over the baseline, automatically selecting 64×64 tiles for short sequences and larger tiles for long sequences.

Next Steps
Explore the cuTile Python repository to access the kernel implementation and documentation.
Review the TileGym repository for the completely optimized Flash Attention kernel.
Read the cuTile Python quickstart guide for installation and setup instructions.

Powered by NVIDIA Nemotron. AI-generated content may summarize information incompletely. Verify important information. Learn more

In this post, we dive into one of the most critical workloads in modern AI: Flash Attention, where you’ll learn:

How to implement Flash Attention using NVIDIA cuTile. Walk through the complete code for a production-ready implementation.

The “trap and rescue” optimization journey. This case study shows how naive optimizations (like just increasing tile size) can backfire, and how to fix them.

Advanced techniques like FMA patterns, fast math, loop splitting, and adaptive tiling for maximum performance.

Environment requirements:

CUDA 13.1 or higher

GPU architecture : Compute capability 8.X, 10.X, 11.X, 12.X (NVIDIA Ampere, NVIDIA Ada, NVIDIA Blackwell)

Python : 3.10 or higher

See the quickstart doc for more information on installing cuTile Python.

What is attention?

The attention mechanism is the computational heart of transformer models. Given a sequence of tokens, attention enables each token to “look at” every other token and decide how much to weigh their contributions. Mathematically, for input matrices Query (\(Q\)), Key (\(K\)), and Value (\(V\)), the output is:

\(O = \text{softmax}\left(\frac{QK^T}{\sqrt{d}}\right)V\)

Where:

\(Q \text{ has shape } (N,d),\ N \text{ query tokens, each with dimension } d.\)

\(K \text{ has shape } (N,d),\ N \text{ key tokens.}\)

\(V \text{ has shape } (N,d),\ N \text{ value tokens.}\)

\(\text{The intermediate } QK^{T} \text{ matrix has shape } (N,N), \text{ is a problem.}\)

The memory bandwidth problem

For a sequence length of \(N = 16,384\) (common in modern LLMs), the attention matrix \(QK^{T}\) contains \(N^2 = 268\) million elements. In FP16, that’s 512 MB of intermediate storage per attention head, per batch item.

Standard attention implementations:

Compute the full \(N \times N\) attention matrix and write it to global memory (slow)

Apply softmax row-by-row

Read the matrix back and multiply by \(V\)

This approach is memory-bound as the GPU spends most of its time waiting for data to move between HBM and compute units, rather than computing.

How Flash Attention solves the memory bandwidth problem

Flash Attention (introduced by Dao et al., 2022) is an IO-aware algorithm that never materializes the full \(N \times N\) matrix. Instead, it:

Tiles the computation : Processes \(Q, K, V\) in small blocks that fit in fast on-chip SMEM

Uses online softmax : Computes softmax incrementally without needing the full row

Fuses operations : Combines the matrix multiply and softmax into a single kernel pass

The result is a 2-4x speedup and significant memory savings, enabling longer context lengths.

Figure 1. Tiled Flash Attention computation

Understanding online softmax

The key algorithmic insight of Flash Attention is the online softmax trick. The numerically stable safe softmax requires knowing the maximum value across the entire row before computing:

\(\text{softmax}(x_i) = \frac{e^{x_i – \max(x)}}{\sum_j e^{x_j – \max(x)}}\)

But if we’re processing tiles, we don’t have access to the full row. Online softmax solves this by maintaining running statistics that can be updated incrementally.

The online softmax algorithm

We maintain two running values for each row:

\(m_i\): The maximum value seen so far (for numerical stability)

\(l_i\): The sum of exponentials seen so far (the softmax denominator)

When we process a new tile with values \(x_{new}\):

Update the maximum : \(m_{new} = \max(m_i, \max(x_{new}))\)

Compute correction factor : \(\alpha = e^{m_i – m_{new}}\) (rescales previous work)

Update the sum : \(l_i = l_i \cdot \alpha + \sum e^{x_{new} – m_{new}}\)

Update the accumulator : \(acc = acc \cdot \alpha + P_{new} \cdot V_{tile}\)

\(P_{new}\) is the matrix of the attention weights, and \(V_{tile}\) is the value matrix tile, corresponding to the Key tile of the current iteration. At the end, we normalize: \(O = acc / l_i\)

This enables us to compute an exact softmax without ever storing the full row.

Causal attention and grouped-query attention

Before diving into the implementation, let’s understand two important attention variants used in modern LLMs:

Causal attention

In autoregressive language models like GPT, LLaMA, and Claude, each token can only attend to previous tokens in the sequence, not future ones. This prevents “cheating” during training, where the model looks ahead to predict the next word.

Mathematically, we apply a triangular mask to the attention scores:

\(\text{mask}_{ij} = \begin{cases} 0 & \text{if } i \geq j \text{ (query position ≥ key position)} \ -\infty & \text{if } i < j \text{ (future tokens)} \end{cases}\)

The masked attention becomes:

\(O = \text{softmax}\left(\frac{QK^T}{\sqrt{d}} + \text{mask}\right)V\)

Adding \(-\infty\) to future positions ensures they become zero after softmax, effectively blocking information flow from future tokens.

Figure 2. Causal attention mask for four tokens

With causal masking, roughly half the attention matrix is masked (the upper triangle). We can skip computing these masked tiles entirely, providing a 2x algorithmic speedup. This is crucial for the K-loop splitting optimization.

Grouped-query attention

Standard multi-head attention has separate \(K,V\) matrices for each attention head, leading to high memory usage:

Multi-head attention (MHA) : 32 query heads → 32 K/V heads (1:1 ratio)

Grouped-query attention (GQA) : 32 query heads → 4 K/V heads (8:1 ratio)

Multi-query attention (MQA) : 32 query heads → 1 K/V head (32:1 ratio)

In GQA, multiple query heads share the same K/V heads. For example, with 32 query heads and 4 K/V heads:

Query heads 0-7 use K/V head 0

Query heads 8-15 use K/V head 1

Query heads 16-23 use K/V head 2

Query heads 24-31 use K/V head 3

This reduces K/V cache size by 8x during inference, critical for serving long-context models. Modern LLMs like LlamA 2, Llama 3, Mistral, and Qwen use GQA extensively.

When implementing in Flash Attention, each CUDA block computes attention for one query head, but loads the appropriate shared K/V head:

head_idx = bid_y % num_heads # Which query head (0-31)
kv_head_idx = head_idx // query_group_size # Which K/V head (0-3)

With a query group size of 8, query heads 0-7 all map to kv_head_idx = 0, sharing the same K/V tiles in memory.

Part 1: The flash attention kernel in CUDA Tile

Let’s implement Flash Attention step-by-step. Our baseline uses small 64×64 tiles and straightforward code—correct but not yet optimized.

1. Defining the kernel interface

In cuTile, the @ct.kernel decorator marks a Python function as a GPU kernel. We pass compile-time constants using ct.Constant[T] type annotations:

import math
import cuda.tile as ct

# Type aliases for compile-time constants
ConstInt = ct.Constant[int]
ConstBool = ct.Constant[bool]

# Conversion factor: we use exp2 instead of exp for efficiency
INV_LOG_2 = 1.0 / math.log(2)

@ct.kernel()
def fmha_kernel(
Q, K, V, Out, # Input/output tensors
qk_scale: float, # Scale factor (1/sqrt(d))
input_pos: int, # Position offset for causal masking
TILE_D: ConstInt, # Head dimension (for example, 128)
H: ConstInt, # Number of attention heads
TILE_M: ConstInt, # Tile size for Q dimension (for example, 64)
TILE_N: ConstInt, # Tile size for K/V dimension (for example, 64)
QUERY_GROUP_SIZE: ConstInt,# For Grouped Query Attention (GQA)
CAUSAL: ConstBool, # Whether to apply causal mask
EVEN_K: ConstBool, # Whether K length is divisible by TILE_N
):

2. Block ID mapping

Each CUDA block computes one tile of the output. Using ct.bid, we map the 2D grid to batch/head indices:

# Get block indices
bid_x = ct.bid(0) # Which tile along the sequence dimension
bid_y = ct.bid(1) # Which batch-head combination

# Decode batch and head from flattened index
batch_idx = bid_y // H
head_idx = bid_y % H

# For Grouped Query Attention: multiple Q heads share one K/V head
off_kv_h = head_idx // QUERY_GROUP_SIZE

3. Initializing accumulators

Before the main loop, we initialize the online softmax state and output accumulator:

# Convert scale for base-2 exponential (faster than natural exp)
qk_scale = qk_scale * INV_LOG_2

# Create position indices for this tile
offs_m = bid_x * TILE_M + ct.arange(TILE_M, dtype=ct.int32)
offs_m += input_pos
offs_m = offs_m[:, None] # Shape: [TILE_M, 1]

offs_n_tile = ct.arange(TILE_N, dtype=ct.int32)
offs_n_tile = offs_n_tile[None, :] # Shape: [1, TILE_N]

# Online softmax state (float32 for numerical stability)
m_i = ct.full((TILE_M, 1), -math.inf, dtype=ct.float32) # Running max
l_i = ct.full((TILE_M, 1), 0.0, dtype=ct.float32) # Running sum
acc = ct.full((TILE_M, TILE_D), 0.0, dtype=ct.float32) # Output accumulator

We use float32 for accumulators, even when inputs are float16 to maintain numerical precision during the iterative softmax computation.

4. Loading the query tile

The query tile is loaded once and reused across all K/V iterations:

# Load Q tile: shape [1, 1, TILE_M, TILE_D] -> [TILE_M, TILE_D]
q = ct.load(
Q,
index=(batch_idx, head_idx, bid_x, 0),
shape=(1, 1, TILE_M, TILE_D)
).reshape((TILE_M, TILE_D))

The ct.load function handles boundary conditions automatically when the tile extends past the tensor edge.

5. The main loop over K/V tiles

This is the heart of Flash Attention. We iterate over K/V tiles:

# Calculate loop bounds
m_end = input_pos + (bid_x + 1) * TILE_M
k_seqlen = K.shape[2]

if CAUSAL:
# For causal attention, stop early (future tokens are masked)
Tc = ct.cdiv(min(m_end, k_seqlen), TILE_N)
else:
Tc = ct.cdiv(k_seqlen, TILE_N)

for j in range(0, Tc):
# --- Step A: Load Key tile and compute QK^T ---
k = ct.load(
K,
index=(batch_idx, off_kv_h, 0, j),
shape=(1, 1, TILE_D, TILE_N),
order=(0, 1, 3, 2), # Transpose for correct layout
latency=2 # Hint for memory prefetching
).reshape((TILE_D, TILE_N))

# Matrix multiply: Q @ K^T
qk = ct.full((TILE_M, TILE_N), 0.0, dtype=ct.float32)
qk = ct.mma(q, k, qk) # Uses Tensor Cores automatically

The order=(0,1,3,2) in the parameter tells cuTile load operation to use K transposed, and latency=2 hints that we can tolerate some latency (enabling better pipelining). Then we use the ct.mma=(q, k, k,qk) to perform the cuTile matrix multiply-accumulate.

6. Applying the causal mask

For autoregressive models (GPT, Llama, etc.), each token can only attend to previous tokens:

# --- Step B: Apply causal masking ---
if CAUSAL or not EVEN_K:
offs_n = j * TILE_N + offs_n_tile
mask = ct.full((TILE_M, TILE_N), True, dtype=ct.bool_)

# Boundary mask (for non-divisible sequence lengths)
if not EVEN_K:
mask = mask & (offs_n < k_seqlen)

# Causal mask: query position >= key position
if CAUSAL:
mask = mask & (offs_m >= offs_n)

# Convert to additive mask: True->0, False->-inf
mask = ct.where(mask, 0.0, -math.inf)
qk += mask

Adding -inf to masked positions ensures they become zero after softmax.

7. Online softmax update

Now we update our running softmax statistics:

# --- Step C: Online softmax ---
# Find max in current tile
qk_max = ct.max(qk, axis=-1, keepdims=True)
qk_max_scaled = qk_max * qk_scale

# Update running maximum
m_ij = max(m_i, qk_max_scaled)

# Scale QK scores
qk = qk * qk_scale
qk = qk - m_ij

# Compute attention weights (using exp2 for speed)
p = ct.exp2(qk)

# Update running sum
l_ij = ct.sum(p, axis=-1, keepdims=True)
alpha = ct.exp2(m_i - m_ij) # Correction factor
l_i = l_i * alpha
l_i = l_i + l_ij

# Rescale previous accumulator
acc = acc * alpha

8. Accumulating the output

Finally, we load the Value tile and accumulate:

# --- Step D: Load V and accumulate ---
v = ct.load(
V,
index=(batch_idx, off_kv_h, j, 0),
shape=(1, 1, TILE_N, TILE_D),
latency=4
).reshape((TILE_N, TILE_D))

# Cast attention weights back to input dtype for Tensor Core MMA
p = p.astype(Q.dtype)

# Accumulate: acc += P @ V
acc = ct.mma(p, v, acc)

# Update max for next iteration
m_i = m_ij

9. Final normalization and store

After processing all tiles, we normalize by the total sum and write the result:

# --- Final: Normalize and store ---
acc = ct.truediv(acc, l_i)
acc = acc.reshape((1, 1, TILE_M, TILE_D)).astype(Out.dtype)
ct.store(Out, index=(batch_idx, head_idx, bid_x, 0), tile=acc)

Launching the kernel: Host-side code

Now let’s look at the host-side code that launches the kernel:

import torch
from math import ceil

def tile_fmha(q, k, v, sm_scale=None, is_causal=True):
"""
Launch the Flash Attention kernel.

Args:
q: Query tensor, shape [batch, heads, seq_len, head_dim]
k: Key tensor, shape [batch, kv_heads, seq_len, head_dim]
v: Value tensor, shape [batch, kv_heads, seq_len, head_dim]
sm_scale: Softmax scale (default: 1/sqrt(head_dim))
is_causal: Whether to apply causal masking

Returns:
Output tensor, same shape as q
"""
if sm_scale is None:
sm_scale = 1.0 / math.sqrt(q.size(-1))

batch_size, num_heads, seq_len, head_dim = q.shape
_, num_kv_heads, _, _ = k.shape

# Calculate query group size for GQA
query_group_size = num_heads // num_kv_heads

# Ensure contiguous memory layout
q = q.contiguous()
k = k.contiguous()
v = v.contiguous()

# Allocate output
o = torch.empty_like(q)

# Choose tile sizes (we'll optimize this later!)
TILE_M, TILE_N = 64, 64

# Calculate grid dimensions
grid_x = ceil(seq_len / TILE_M) # Number of tiles along sequence
grid_y = batch_size * num_heads # One block per batch-head pair
grid = (grid_x, grid_y, 1)

# Check if K length is evenly divisible
EVEN_K = (k.shape[2] % TILE_N) == 0

# Launch kernel
ct.launch(
torch.cuda.current_stream(),
grid,
fmha_kernel,
(q, k, v, o, sm_scale, 0, head_dim, num_heads,
TILE_M, TILE_N, query_group_size, is_causal, EVEN_K)
)

return o

This baseline with 64×64 tiles works correctly. But can we make it faster? Let’s find out.

Part 2: The “trap and rescue” optimization journey

We benchmark on the following configuration:

Hardware : NVIDIA B200

Batch : 4, Heads : 32, Head dimension : 128

Attention : Causal, Dtype : FP16

Sequence lengths : 1024, 2048, 4096, 8192, 16384

To interpret each step, we use Nsight Compute with a minimal section set:

LaunchStats

Occupancy

SpeedOfLight

ComputeWorkloadAnalysis

MemoryWorkloadAnalysis

Baseline performance

SeqLen
Throughput (TFLOPS)

1,024
330

2,048
441

4,096
511

8,192
546

16,384
566

Table 1. Baseline performance without any specific optimizations

This is our starting point with 64×64 tiles and no optimizations.

NCU insight (SeqLen=1024, B200) :

Registers/thread: 128

Theoretical/achieved occupancy: 25% / 19.8%

Compute (SM) throughput: 37.8%

Memory throughput: 19.7%

Grid size: 2,048

1. The trap of larger tiles

A common intuition in GPU programming is “bigger tiles = better performance.” Larger tiles:

Amortize memory access overhead.

Improve L2 cache utilization.

Reduce kernel launch overhead per element.

So, let’s increase our tile size from 64×64 to 256×128 :

TILE_M, TILE_N = 256, 128 # Was 64, 64

The expected is better memory bandwidth utilization → faster performance. However, the result in TFLOPS are:

SeqLen
Baseline (64×64)
Larger tiles (256×128)
Performance Degradation

1,024
330
187
-43%

2,048
441
268
-39%

4,096
511
347
-32%

8,192
546
415
-24%

16,384
566
463
-18%

Table 2. Baseline performance compared to performance with larger tile sizes, showing degradation when using larger tile sizes

Performance degraded by 18-43% across all sequence lengths. This is the trap, where large tiles make performance worse.

Why does this happen?

Compute bottleneck : With more elements per tile, inefficient operations (separate mul/add, precise math) become the bottleneck.

Instruction overhead : More work per tile means more instructions before the next memory operation.

Lesson : Tile size and compute efficiency are interdependent. Large tiles only help if the computation is efficient enough to keep up.

NCU insight (SeqLen=1,024, NVIDIA B200) :

Registers/thread jump to 168 (+31%), reducing theoretical occupancy to 18.75%

Achieved occupancy drops to 16.5%

Compute throughput collapses to 17.4% (the trap)

Memory throughput falls to 7.4%

Grid size shrinks to 512 (fewer blocks from larger tiles)

2. The rescue with fast math

One of the bottlenecks is special functions: exp2 (exponential) and truediv (division). By default, these are IEEE-754 precise—highly accurate, but slow.

For deep learning, we can trade a tiny bit of precision for massive speedups:

Before (precise operations):

p = ct.exp2(qk)
alpha = ct.exp2(m_i - m_ij)
acc = ct.truediv(acc, l_i)

After (fast math):

p = ct.exp2(qk, flush_to_zero=True)
alpha = ct.exp2(m_i - m_ij, flush_to_zero=True)
acc = ct.truediv(acc, l_i, flush_to_zero=True, rounding_mode=RMd.APPROX)

What these flags do :

flush_to_zero=True : Denormal numbers (extremely small values near zero) become exactly zero. This avoids slow microcode paths on the GPU.

rounding_mode=RMd.APPROX : Skips iterative refinement after initial hardware approximation.

With fast math, we’ve “rescued” the large tiles, and the results in TFLOPS are:

SeqLen
Larger tiles (trap)
Fast math (rescue)
Improvement

1,024
187
322
+72%

2,048
268
436
+63%

4,096
347
524
+51%

8,192
415
585
+41%

16,384
463
620
+34%

Table 3. Performance improvement when using two fast math optimizations

We now match or exceed the small-tile baseline, with 10-20% gains for longer sequences.

NCU insight (SeqLen=1,024, NVIDIA B200) :

Registers/thread: 168 (unchanged)

Theoretical/achieved occupancy: 18.75% / 16.6% (unchanged)

Compute throughput rebounds to 24.0%

Memory throughput improves to 12.9%

3. K-loop split

For causal attention, we apply a triangular mask: each query can only attend to keys at earlier positions. In our baseline, we check if CAUSAL: mask … on every loop iteration.

But think about it: for a query tile at position 1000, most key tiles (0-900) need no masking at all. Only tiles near the diagonal need the mask. And tiles beyond the query position are completely masked (we can skip them entirely).

Figure 3. Tiled causal attention matrix (8 tiles per side)

The optimization splits the loop into phases:

# Calculate where masking starts being necessary
mask_start = (input_pos + bid_x * TILE_M) // TILE_N
mask_start = min(mask_start, k_seqlen // TILE_N)

# Calculate where to stop (for causal, we exit early)
if CAUSAL:
Tc = ct.cdiv(min(m_end, k_seqlen), TILE_N)
else:
Tc = ct.cdiv(k_seqlen, TILE_N)

for j in range(0, Tc):
# Load K and compute QK...

# ONLY apply masking when necessary
if (CAUSAL or not EVEN_K) and j >= mask_start:
offs_n = j * TILE_N + offs_n_tile
mask = ct.full((TILE_M, TILE_N), True, dtype=ct.bool_)
if not EVEN_K:
mask = mask & (offs_n < k_seqlen)
if CAUSAL:
mask = mask & (offs_m >= offs_n)
mask = ct.where(mask, 0.0, -math.inf)
qk += mask

# Continue with softmax and accumulation...

Why this matters : For a 16K sequence with 256-token tiles:

~50% of tiles are fully unmasked (no branch, no mask computation)

~1 tile per row is partially masked (full logic)

The rest are skipped entirely (early exit)

Result in TFLOPS :

SeqLen
Fast math
Loop split
Improvement

1,024
322
373
+16%

2,048
436
552
+27%

4,096
524
684
+31%

8,192
585
770
+32%

16,384
620
813
+31%

Table 4. Performance improvement when using K-loop split optimization

This is the biggest single optimization —up to 32% speedup across all sequence lengths.

NCU insight (SeqLen=1,024, B200) :

Registers/thread: 168 (unchanged)

Theoretical/achieved occupancy: 18.75% / 16.6% (unchanged)

Memory throughput improves to 14.5% (less wasted work)

Compute throughput remains 24.0% (work is more useful, not necessarily faster per cycle)

4. ProgramId remapping

One subtle optimization is reversing the block order for causal attention. When we process tiles in reverse (bottom-right to top-left), later-launched blocks have less work due to the causal mask. This improves load balancing and reduces tail effects.

Before (standard order):

bid_x = ct.bid(0) # Process tiles 0, 1, 2,...

After (reversed for causal):

if CAUSAL:
bid_x = NUM_M_BLOCKS - 1 - ct.bid(0) # Process tiles N, N-1, N-2,...
else:
bid_x = ct.bid(0)

This small change improves wave scheduling, as blocks complete more uniformly across the GPU.

Result in TFLOPS :

SeqLen
Loop split
Remapping
Improvement

1,024
373
377
+1%

2,048
552
560
+1.5%

4,096
684
696
+1.8%

8,192
770
781
+1.5%

16,384
813
835
+2.6%

Table 5. Performance improvement after remapping the block order of the tiles

A modest but consistent 1-3% gain, especially noticeable at longer sequences where tail effects matter most.

5. Autotuning

We’ve optimized large tiles, but there’s a catch: short sequences still prefer small tiles.

Why? With a 1,024-token sequence and 256-token tiles, we only have 4 tiles. That’s not enough to fully utilize all SMs on a B200. Smaller tiles (64×64) give us 16 tiles, better filling the GPU.

Rather than manually choosing a threshold, we can let cuTile’s autotuner benchmark multiple configurations and cache the best one for each input shape.

The autotuner approach :

def _fmha_autotune_configs():
"""Search space for autotuning.

The autotuner will benchmark these configurations and cache the best one
per input shape (sequence length, batch size, etc.).
"""
gpu_capability = torch.cuda.get_device_capability()

if gpu_capability in [(12, 0), (12, 1)]:
# RTX 50 series (sm120, sm121)
yield SimpleNamespace(TILE_M=64, TILE_N=64, num_ctas=1, occupancy=2)
else:
# B200/GB200 (sm100) - Try multiple tile sizes
# Autotuner will discover:
# - 64x64 is best for short sequences (1024-2048)
# - 128x128 may be best for medium sequences (4096)
# - 256x128 is best for long sequences (8192+)
yield SimpleNamespace(TILE_M=64, TILE_N=64, num_ctas=1, occupancy=2)
yield SimpleNamespace(TILE_M=128, TILE_N=128, num_ctas=1, occupancy=2)
yield SimpleNamespace(TILE_M=256, TILE_N=128, num_ctas=1, occupancy=1)

How to launch with autotuning :

Instead of calling ct.launch directly, use ct_experimental.autotune_launch :

import cuda.tile_experimental as ct_experimental

def autotune_launch_fmha(
stream, q, k, v, o, sm_scale, input_pos,
hidden_size, num_heads, query_group_size, is_causal
):
batch_size, _, q_len, _ = q.shape

def _grid_fn(cfg):
return (math.ceil(q_len / cfg.TILE_M), batch_size * num_heads, 1)

def _args_fn(cfg):
num_m_blocks = math.ceil(q_len / cfg.TILE_M)
even_k = (k.shape[2] % cfg.TILE_N) == 0
return (
q, k, v, o, sm_scale, input_pos,
hidden_size, num_heads, cfg.TILE_M, cfg.TILE_N,
query_group_size, is_causal, even_k, num_m_blocks,
)

ct_experimental.autotune_launch(
stream,
grid_fn=_grid_fn,
kernel=fmha_kernel,
args_fn=_args_fn,
hints_fn=lambda cfg: {"num_ctas": cfg.num_ctas, "occupancy": cfg.occupancy},
search_space=_fmha_autotune_configs,
)

Note: The autotuner API may be subject to change.

The autotuner works intelligently:

First call with seq_len=1024 : Benchmarks all 3 configs, caches best one

First call with seq_len=2048 : Benchmarks all 3 configs, caches best one

Subsequent calls : Uses cached config (zero overhead)

The cache key includes tensor shapes, so different sequence lengths automatically get different optimal configurations.

Result in TFLOPS :

SeqLen
Baseline
Remapping
Autotune
Speedup vs baseline

1,024
330
377
548
1.66x

2,048
441
560
708
1.61x

4,096
511
696
817
1.60x

8,192
546
781
887
1.62x

16,384
566
835
918
1.62x

Table 6. Original baseline compared to step 5 and to step 6 autotuned results

The autotuner discovers that 64×64 tiles are best for sequences ≤2,048, then transitions to larger tiles for longer sequences. This delivers 45% additional performance at short sequences compared to fixed large tiles, while maintaining peak performance at long sequences.

What the autotuner chose (on B200):

SeqLen 1,024: 64×64 tiles (high parallelism)

SeqLen 2,048: 64×64 or 128×128 tiles (balanced)

SeqLen 4,096+: 128×128 or 256×128 tiles (memory efficiency)

We now achieve optimal performance across all sequence lengths without manual tuning.

Summary: The optimization stack

Optimization
Key insight
Impact

Baseline (64×64)
Correct but unoptimized
Baseline

Large tiles (256×128)
TRAP : 18-43% slower!
-18% to -43%

+ Fast math (FTZ, APPROX)
RESCUE : Large tiles now pay off
+34% to +72% from trap

+ K-loop split
Biggest single optimization
+16% to +32%

+ ProgramId remapping
Better load balancing
+1% to +3%

+ Autotuning
Optimal tiles per sequence
+10% to +45%

Table 7. Step-by-step optimization results with performance impacts for each step

Final speedup: 1.60x-1.66x across all sequence lengths.

Getting started

Writing high-performance kernels is rarely about finding one “magic” setting. As we saw with the “trap and rescue”:

Optimizations are interdependent : Large tiles were slower until we fixed the math. You can’t evaluate tile size in isolation.

Math matters : Flags like flush_to_zero and APPROX are critical for unlocking Tensor Core throughput. Precise math is often overkill for deep learning.

Algorithmic wins compound : K-loop splitting gave us the biggest single improvement (up to 32%) by avoiding unnecessary work.

Autotuning beats manual heuristics : cuTile’s autotuner discovers optimal tile sizes per sequence length (64×64 for short sequences, 256×128 for long), delivering 10-45% gains over fixed configurations.

Cumulative effects are multiplicative : The full optimization stack delivers 1.60x-1.66x speedup across all sequence lengths—far more than any single optimization alone.

cuTile enables developers to express these optimizations—tiling, fast math controls, loop splitting, autotune—in clean, readable Python code while generating highly optimized PTX for NVIDIA GPUs.

You can find the completely optimized kernel in the TileGym repository. Happy hacking.

Discuss (0)

Like

Tags

Agentic AI / Generative AI | Data Science | Developer Tools & Techniques | General | CUDA | Advanced Technical | Tutorial | CUDA Tile | cuTile | featured

About the Authors

About Alessandro Morari

Alessandro Morari is an AI systems leader at NVIDIA in the DevTech AI organization. His current focus is on AI-driven GPU kernels and next-generation programming models for accelerated computing. His experience spans the full AI stack, from GPU kernel optimization to AI product leadership. Before NVIDIA, he led the team at IBM Research that shipped the Watson Code Assistant, one of the earliest large-scale generative AI products. He previously worked on system software for the Summit and Sierra supercomputers and created NYU Courant's first course on high-performance machine learning. Morari has authored over 30 publications, holds 15 patents, and earned a Ph.D. in Computer Architecture.

View all posts by Alessandro Morari

About Allen Zhao

Allen Zhao (Wenyi Zhao) is a senior compute architect engineer specializing in cutting-edge AI compiler technologies, including both graph-level and tile-level compilation. His expertise lies in optimizing the execution efficiency of AI models across diverse hardware architectures, especially for GPGPU. He's passionate about translating theoretical compiler advancements into practical, high-impact solutions for the next generation of artificial intelligence. He holds a Master's degree from Shanghai Jiao Tong University.

View all posts by Allen Zhao

About Ivan Yin

Ivan Yin (Wenzhi Yin) is a senior computer architect engineer specializing in GPU compiler engineering and high-performance deep learning. He graduated from Shanghai Jiao Tong University. He has expertise in compiler development for NVIDIA CUDA Tile Programming, where he maps high-level tensor operations to efficient GPU machine code through automated code generation for modern GPU architectures. Beyond compiler engineering, he has experience in high-performance deep learning kernel development and performance tuning.

View all posts by Ivan Yin

About Vishal Mehta

Vishal works as a senior developer technology engineer at NVIDIA, with focus on performance optimization for GPU applications. He has been working in the field of GPU computing for over 10 years. He is keen on teaching CUDA and GPU computing to users and drives the content for the CUDA programming guide. His day-to-day activities involve collaborations with domain scientists and industry experts to improve their workloads on GPUs.

View all posts by Vishal Mehta

Comments

Related posts

Accelerating Mamba-2 with High-Performance Forward Pass Kernels in NVIDIA CUDA Tile

Accelerating Mamba-2 with High-Performance Forward Pass Kernels in NVIDIA CUDA Tile

Making Softmax More Efficient with NVIDIA Blackwell Ultra

Making Softmax More Efficient with NVIDIA Blackwell Ultra

OpenAI Triton on NVIDIA Blackwell Boosts AI Performance and Programmability

OpenAI Triton on NVIDIA Blackwell Boosts AI Performance and Programmability

Next Generation of FlashAttention

Next Generation of FlashAttention

Accelerating Transformers with NVIDIA cuDNN 9

Accelerating Transformers with NVIDIA cuDNN 9

Related posts

GPU-Accelerated Clustering for Financial Instruments at Scale

GPU-Accelerated Clustering for Financial Instruments at Scale

Run Massive-Scale UMAP in Minutes Using Multiple GPUs—Without Losing Accuracy

Run Massive-Scale UMAP in Minutes Using Multiple GPUs—Without Losing Accuracy

Beyond VLAs: How World Action Models Reshape Robot Manipulation

Beyond VLAs: How World Action Models Reshape Robot Manipulation

Generate Trajectories, Reasoning Traces, and Auto-Labels with NVIDIA Alpamayo 2 Super

Generate Trajectories, Reasoning Traces, and Auto-Labels with NVIDIA Alpamayo 2 Super

Run High-Performance Core Math at Scale with NVIDIA nvmath-python

Run High-Performance Core Math at Scale with NVIDIA nvmath-python

Theme

Light

System

Dark

Close

Previous

Next

L

T

F

R

E
