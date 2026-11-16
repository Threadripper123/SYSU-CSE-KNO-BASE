# GPU 术语：内存合并（Memory Coalescing）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　6,790 字符　｜　38 个片段　｜　更新于 2026-11-13
来源: https://modal.com/gpu-glossary/perf/memory-coalescing

# https://modal.com/gpu-glossary/perf/memory-coalescing

来源: https://modal.com/gpu-glossary/perf/memory-coalescing
抓取方式: read_page

GPU Glossary
GPU Glossary
Terminal Light green Light

Deploy on GPUs

TABLE OF CONTENTS
Home -
README
Device Hardware -
CUDA (Device Architecture)
Streaming Multiprocessor SM

Core
Special Function Unit SFU

Load/Store Unit LSU

Warp Scheduler
CUDA Core
Tensor Core
Tensor Memory Accelerator TMA

Streaming Multiprocessor Architecture
Texture Processing Cluster TPC

Graphics/GPU Processing Cluster GPC

Register File
L1 Data Cache
Tensor Memory
GPU RAM
Device Software -
CUDA (Programming Model)
Streaming ASSembler SASS

Parallel Thread eXecution PTX

Compute Capability
Thread
Warp
Warpgroup
Cooperative Thread Array
Kernel
Thread Block
Thread Block Grid
Thread Hierarchy
Memory Hierarchy
Registers
Shared Memory
Global Memory
CUDA Tile Programming Model
Host Software -
CUDA (Software Platform)
CUDA C++ (programming language)
NVIDIA GPU Drivers
nvidia.ko
CUDA Driver API
libcuda.so
NVIDIA Management Library NVML

libnvml.so
nvidia-smi
CUDA Runtime API
libcudart.so
CUDA Graphs
NVIDIA CUDA Compiler Driver nvcc

NVIDIA Runtime Compiler
NVIDIA CUDA Profiling Tools Interface CUPTI

NVIDIA Nsight Systems
CUDA Binary Utilities
cuBLAS
cuDNN
CUTLASS
CuTe
CuTe DSL
Performance -
Performance Bottleneck
Roofline Model
Compute-bound
Memory-bound
Arithmetic Intensity
Overhead
Little's Law
Memory Bandwidth
Arithmetic Bandwidth
Latency Hiding
Warp Execution State
Active Cycle
Occupancy
Pipe Utilization
Peak Rate
Issue Efficiency
Streaming Multiprocessor Utilization
Warp Divergence
Scoreboard Stall
Branch Efficiency
Memory Coalescing
Bank Conflict
Register Pressure
Contributors

/perf/memory-coalescing? Something seem wrong?
Or want to contribute?

Click
this button to
let us know on GitHub.

What is Memory Coalescing?

Memory coalescing is a hardware technique to improve the utilization of
memory bandwidth by servicing multiple
logical memory reads in a single physical memory access.

Memory coalescing occurs during accesses of
global memory. For efficient
access of shared memory, see the
article on bank conflict.

In CUDA GPUs,
global memory is backed by the
GPU RAM, built with Dynamic Random
Access Memory (DRAM) technologies like GDDR or HBM. These technologies have high
memory bandwidth but long access latency
(even compared to the peer technology used in CPU RAM, DDR5). DRAM access
latency is limited by the speed at which the small capacitors can charge up
their access lines, which is fundamentally limited by thermal, power, and size
constraints. Due to this high latency, if all logical memory accesses are
serviced as separate physical accesses, the GPU's
memory bandwidth will not be fully
utilized.

Memory coalescing takes advantage of the internals of DRAM technology to enable
full bandwidth utilization for certain access patterns. Each time a DRAM address
is accessed, multiple consecutive addresses are fetched together in parallel in
a single clock. For a bit more detail, see Section 6.1 of
the 4th edition of Programming Massively Parallel Processors;
for comprehensive detail, see Ulrich Drepper's excellent article
What Every Programmer Should Know About Memory.
The access and transfer of these consecutive memory locations is referred to as
a DRAM burst. If multiple concurrent logical accesses are serviced by a single
physical burst, the access is said to be coalesced. Note that a physical
access is part of a memory transaction, terminology you may see elsewhere in
descriptions of memory coalescing.

On CPUs, a similar mapping of bursts onto cache lines improves access
efficiency. As is common in GPU programming, what is automatic cache behavior in
CPUs is here programmer-managed.

That's not as hard as it could be, because DRAM bursts align elegantly with the
single-instruction, multiple thread (SIMT) execution model of
CUDA PTX. That is, in
normal execution all threads in a
warp execute the same instruction at the
same time. That makes it easy for a
CUDA programmer to write
programs with coalesced access and simple for the memory management hardware to
detect accesses that can be coalesced. Typically, a single burst can service 128
bytes – not coincidentally, enough for each of the 32
threads in a
warp to load one 32 bit float.

To demonstrate the performance impact of memory coalescing, let's consider the
following kernel, which reads values
from an array with a variable stride, or spacing between accessed elements.
With increasing stride, the number of DRAM bursts required to service the read
issued by each warp will increase, leading
to more physical accesses per logical access and so to reduced memory
throughput.

cpp
__global__ void strided_read_kernel(const float* __restrict__ in,
float* __restrict__ out,
size_t N, int stride)
{
const size_t t = blockIdx.x * blockDim.x + threadIdx.x;
const size_t T = gridDim.x * (size_t)blockDim.x;

float acc = 0.f;

for (size_t j = (size_t)t * (size_t)stride; j < N; j += (size_t)T * (size_t)stride) {
// across a warp, addresses differ by (stride * sizeof(float))
float v = in[j]; // perfectly coalesced for stride == 1
acc = acc * 1.000000119f + v; // force compiler to keep the load
}

// do one write per thread (negligible vs reads)
if (t < N) out[t] = acc;
}

When we run this kernel through a micro-benchmark on Godbolt (which you can
reproduce here ), we observe the expected
relationship between stride and throughput:

# Device: Tesla T4 (SM 75)
# N = 67108864 floats (256.0 MB), iters = 10
stride GB/s
1 206.0
2 130.5
4 68.8
8 33.8
16 16.8
32 15.2
64 13.6
128 11.2

That is, adding a stride of two cuts the throughput in half, as the number of
DRAM bursts required to service each
warp's request doubles. Doubling the
stride to four again cuts throughput in half once more. The pattern changes once
we hit a 16x reduction in throughput at a stride of 16. Performance degrades
differently from there, presumably due to increasing visibility of other memory
subsystem components and their degraded performance from reduced locality (e.g.
on-device TLB misses).

For more best practices for global memory access, see the post
How to Access Global Memory Efficiently in CUDA C/C++ Kernels
on the NVIDIA Developers blog.

Building on GPUs? We know a thing or two about it.
Modal is an ergonomic Python SDK wrapped around a global GPU fleet. Deploy serverless AI workloads instantly without worrying
about quota requests, driver compatibility issues, or managing
bulky ML dependencies.

Deploy serverless AI workloads instantly without worrying about quota
requests, driver compatibility issues, or managing bulky ML
dependencies.

Deploy on GPUs

Branch Efficiency Bank Conflict
