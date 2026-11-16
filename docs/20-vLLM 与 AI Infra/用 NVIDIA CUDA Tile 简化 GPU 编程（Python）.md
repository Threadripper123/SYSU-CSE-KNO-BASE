# 用 NVIDIA CUDA Tile 简化 GPU 编程（Python）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　13,147 字符　｜　146 个片段　｜　更新于 2026-11-13
来源: https://developer.nvidia.com/blog/simplify-gpu-programming-with-nvidia-cuda-tile-in-python

# https://developer.nvidia.com/blog/simplify-gpu-programming-with-nvidia-cuda-tile-in-python

来源: https://developer.nvidia.com/blog/simplify-gpu-programming-with-nvidia-cuda-tile-in-python
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

Data Science

English 한국어 中文

Simplify GPU Programming with NVIDIA CUDA Tile in Python

Dec 04, 2025

By Jonathan Bentz and Tony Scudiero

Like

Discuss (4)

L

T

F

R

E

AI-Generated Summary

CUDA Tile programming model introduces tile-based GPU programming that abstracts hardware intricacies like tensor cores while the compiler handles thread partitioning.
cuTile Python expresses this model in Python, enabling developers to write tile kernels using arrays, tiles, kernels, and blocks as primary abstractions.
The vector addition example demonstrates how cuTile Python replaces explicit thread indexing with tile load, compute, and store operations using functions such as ct.bid, ct.load, and ct.store.
NVIDIA Nsight Compute profiles cuTile kernels with a dedicated Tile Statistics report section showing block counts, compiler-chosen block sizes, and other tile-specific metrics.
Running cuTile Python requires a GPU with compute capability 8.x, 10.x, 11.x, or 12.x, NVIDIA Driver R580 or later, CUDA Toolkit 13.1 or later, Python 3.10 or higher, and the cuda-tile package installed via pip.

Next Steps
Watch the What is CUDA Tile? video for an introduction to the programming model.
Explore the cuTile Python documentation to learn the API and development workflow.
Try the sample programs on GitHub to start writing cuTile Python kernels.

Powered by NVIDIA Nemotron. AI-generated content may summarize information incompletely. Verify important information. Learn more

The release of NVIDIA CUDA 13.1 introduces tile-based programming for GPUs, making it one of the most fundamental additions to GPU programming since CUDA was invented. Writing GPU tile kernels enables you to write your algorithm at a higher level than a single-instruction multiple-thread (SIMT) model, while the compiler and runtime handle the partitioning of work onto threads under the covers. Tile kernels also help abstract away special-purpose hardware like tensor cores, and write code that’ll be compatible with future GPU architectures. With the launch of NVIDIA cuTile Python, you can write tile kernels in Python.

What is cuTile Python?

cuTile Python is an expression of the CUDA Tile programming model in Python, built on top of the CUDA Tile IR specification. It enables you to write tile kernels in Python and express GPU kernels using a tile-based model, rather than or in addition to a single instruction, multiple threads (SIMT) model.

SIMT programming requires specifying each GPU thread of execution. In principle, each thread can operate independently and execute a unique code path from any other thread. In practice, to use GPU hardware effectively, it’s typical to program algorithms where each thread performs the same work on separate pieces of data.

SIMT enables maximum flexibility and specificity, but can also require more manual tuning to achieve top performance. The tile model abstracts away some of the HW intricacies. You can focus on your algorithm at a higher level, while the NVIDIA CUDA compiler and runtime handle partitioning your tile algorithm into threads and launching them onto the GPU.

cuTile is a programming model for writing parallel kernels for NVIDIA GPUs. In this model:

Arrays are the primary data structure.

Tiles are subsets of arrays that kernels operate on.

Kernels are functions that are executed in parallel by blocks.

Blocks are subsets of the GPU; operations on tiles are parallelized across each block.

cuTile automates block-level parallelism and asynchrony, memory movement, and other low-level details of GPU programming. It will leverage the advanced capabilities of NVIDIA hardware (such as tensor cores, shared memory, and tensor memory accelerators) without requiring explicit programming. cuTile is portable across different NVIDIA GPU architectures, enabling you to use the latest hardware features without rewriting your code.

Who is cuTile for?

cuTile is for general-purpose data-parallel GPU kernel authoring. Our efforts have been focused on optimizing cuTile for the types of computations typically encountered in AI/ML applications. We’ll continue to evolve cuTile, adding functionality and performance features to expand the range of workloads it can optimize.

You might be asking why you’d use cuTile to write kernels when CUDA C++ or CUDA Python has worked well so far. We talk more about this in another post describing the CUDA tile model. The short answer is that as GPU hardware becomes more complex, we’re providing an abstraction layer at a reasonable level so developers can focus more on algorithms and less on mapping an algorithm to specific hardware.

Writing tile programs enables you to target tensor cores with code compatible with future GPU architectures. Just as Parallel Thread Execution (PTX) provides the virtual Instruction Set Architecture (ISA) that underlies the SIMT model for GPU programming, Tile IR provides the virtual ISA for tile-based programming. It enables higher-level algorithm expression, while the software and hardware transparently map that representation to tensor cores to deliver peak performance.

cuTile Python example

What does cuTile Python code look like? If you’ve learned CUDA C++, you probably encountered the canonical vector addition kernel. Assuming the data has been copied from the host to the device, a vector add kernel in CUDA SIMT looks something like the following, which takes two vectors and adds them together elementwise to produce a third vector. This is one of the simplest CUDA kernels you can write.

__global__ void vecAdd(float* A, float* B, float* C, int vectorLength)
{
/* calculate my thread index */
int workIndex = threadIdx.x + blockIdx.x*blockDim.x;

if(workIndex < vectorLength)
{
/* perform the vector addition */
C[workIndex] = A[workIndex] + B[workIndex];
}
}

In this kernel, each thread’s work is explicitly specified, and the programmer, when launching this kernel, selects the number of blocks and threads for launch.

Now, let’s look at the equivalent code written in cuTile Python. We don’t need to specify what each thread does. We only have to break the data into tiles and specify the mathematical operations for each tile. Everything else is handled for us.

The cuTile Python kernel looks as follows:

import cuda.tile as ct

@ct.kernel
def vector_add(a, b, c, tile_size: ct.Constant[int]):
# Get the 1D pid
pid = ct.bid(0)

# Load input tiles
a_tile = ct.load(a, index=(pid,), shape=(tile_size, ) )
b_tile = ct.load(b, index=(pid,), shape=(tile_size, ) )

# Perform elementwise addition
result = a_tile + b_tile

# Store result
ct.store(c, index=(pid, ), tile=result)

ct.bid(0) is the function that obtains the block ID along the (in this case) zeroth axis. It’s equivalent to how SIMT kernel writers would reference blockIdx.x and threadIdx.x, for example. ct.load() is the function that loads a tile of data, with the requisite index and shape, from device memory. Once data is loaded into tiles, these tiles can be used in computations. When all the computations are complete, ct.store() puts the tiled date back into GPU device memory.

Putting it all together

Now we’ll show how to call this vector_add kernel in Python using a complete Python script that you can try yourself. The following is the complete code, including the kernel and the main function.

"""
Example demonstrating simple vector addition.
Shows how to perform elementwise operations on vectors.
"""

from math import ceil

import cupy as cp
import numpy as np
import cuda.tile as ct

@ct.kernel
def vector_add(a, b, c, tile_size: ct.Constant[int]):
# Get the 1D pid
pid = ct.bid(0)

# Load input tiles
a_tile = ct.load(a, index=(pid,), shape=(tile_size, ) )
b_tile = ct.load(b, index=(pid,), shape=(tile_size, ) )

# Perform elementwise addition
result = a_tile + b_tile

# Store result
ct.store(c, index=(pid, ), tile=result)

def test():
# Create input data
vector_size = 2**12
tile_size = 2**4
grid = (ceil(vector_size / tile_size),1,1)

a = cp.random.uniform(-1, 1, vector_size)
b = cp.random.uniform(-1, 1, vector_size)
c = cp.zeros_like(a)

# Launch kernel
ct.launch(cp.cuda.get_current_stream(),
grid, # 1D grid of processors
vector_add,
(a, b, c, tile_size))

# Copy to host only to compare
a_np = cp.asnumpy(a)
b_np = cp.asnumpy(b)
c_np = cp.asnumpy(c)

# Verify results
expected = a_np + b_np
np.testing.assert_array_almost_equal(c_np, expected)

print("✓ vector_add_example passed!")

if __name__ == "__main__":
test()

Assuming you’ve already installed all the requisite software, including cuTile Python and CuPy, running this code is as simple as invoking Python.

$ python3 VectorAdd_quickstart.py
✓ vector_add_example passed!

Congratulations, you just ran your first cuTile Python program!

Developer tools

cuTile kernels can be profiled with NVIDIA Nsight Compute in the same way as SIMT kernels.

$ ncu -o VecAddProfile --set detailed python3 VectorAdd_quickstart.py

Once you’ve created the profile and opened it with the graphical version of Nsight Compute:

Select the vector_add kernel

Choose the “Details” tab

Expand the “Tile Statistics” report section

You should see an image similar to Figure 1.

Figure 1. Profile generated from Nsight Compute, showing the tile statistics for the vector_add kernel

Notice the Tile Statistics report section includes the number of tile blocks specified, block size (chosen by compiler), and various other tile-specific information.

The source page also supports cuTile kernels and performance metrics at the source-line level, just like CUDA C kernels.

How developers can get cuTile

To run cuTile Python programs, you need the following:

A GPU with compute capability 8.x, 10.x, 11.x or 12.x (in future CUDA releases, we’ll add support for additional GPU architectures)

NVIDIA Driver R580 or later (R590 is required for tile-specific developer tools support)

CUDA Toolkit 13.1 or later

Python version 3.10 or higher

The cuTile Python package: pip install cuda-tile

Get started

Check out a few videos to help you learn more:

Video 1. Getting started with cuTile Python

What is CUDA Tile?

The Future is Tiled

Deep Dive: How to Use cuTile Python

Also, check out the cuTile Python documentation.

You’re now ready to try the sample programs on GitHub and start programming in cuTile Python.

Discuss (4)

Like

Tags

Data Science | Developer Tools & Techniques | General | CUDA | Intermediate Technical | Tutorial | CUDA Tile | cuTile | featured | Python

About the Authors

About Jonathan Bentz

Jonathan Bentz leads the CUDA technical marketing engineering team at NVIDIA, where his team focuses on creating and delivering engaging content and connecting with CUDA developers. Jonathan holds a PhD in Chemistry and a master’s degree in Computer Science from Iowa State University.

View all posts by Jonathan Bentz

About Tony Scudiero

Tony Scudiero is a technical marketing engineer for the CUDA platform. He works to bring CUDA to developers of every type and ability. He has worked with large HPC systems and applications, real-time acoustic simulations (VRWorks Audio), and the Omniverse RTX Renderer during his tenure at NVIDIA.

View all posts by Tony Scudiero

Comments

Related posts

Develop High-Performance GPU Kernels in C++ with NVIDIA CUDA Tile

Develop High-Performance GPU Kernels in C++ with NVIDIA CUDA Tile

CUDA 13.2 Introduces Enhanced CUDA Tile Support and New Python Features

CUDA 13.2 Introduces Enhanced CUDA Tile Support and New Python Features

cuTile.jl Brings NVIDIA CUDA Tile-Based Programming to Julia

cuTile.jl Brings NVIDIA CUDA Tile-Based Programming to Julia

NVIDIA CUDA 13.1 Powers Next-Gen GPU Programming with NVIDIA CUDA Tile and Performance Gains

NVIDIA CUDA 13.1 Powers Next-Gen GPU Programming with NVIDIA CUDA Tile and Performance Gains

Focus on Your Algorithm—NVIDIA CUDA Tile Handles the Hardware

Focus on Your Algorithm—NVIDIA CUDA Tile Handles the Hardware

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
