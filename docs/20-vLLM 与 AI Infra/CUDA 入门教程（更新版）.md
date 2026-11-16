# CUDA 入门教程（更新版）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　11,327 字符　｜　173 个片段　｜　更新于 2026-11-13
来源: https://developer.nvidia.cn/blog/even-easier-introduction-cuda-2

# https://developer.nvidia.cn/blog/even-easier-introduction-cuda-2

来源: https://developer.nvidia.cn/blog/even-easier-introduction-cuda-2
抓取方式: read_page

DEVELOPER

首页
博客
论坛
论坛 (英文)
文档
下载
培训

搜索

加入

Related Resources

数据科学

English 한국어 中文

CUDA 入门教程：更简单的介绍 (更新版)

2025年 5月 2日

作者： Mark Harris

点赞

W

W

Email

注意：本博文最初发布于 2017 年 1 月 25 日，但已进行编辑以反映新的更新。

本文非常简单地介绍了 CUDA，这是 NVIDIA 的热门并行计算平台和编程模型。我在 2013 年写过一篇文章，名为“ An Easy Introduction to CUDA ”，多年来一直备受欢迎。但是，CUDA 编程变得更加简单，GPU 也变得更快了，所以现在是时候更新 (甚至更轻松) 介绍了。

CUDA C++ 只是使用 CUDA 创建大规模并行应用程序的多种方法之一。它允许您使用功能强大的 C++ 编程语言来开发由 GPU 上运行的数千个并行线程加速的高性能算法。许多开发者都以这种方式加速了需要大量计算和带宽的应用程序，包括支持人工智能持续革命 (即 Deep Learning ) 的库和框架。

您听说过 CUDA，并且有兴趣学习如何在自己的应用中使用 CUDA。如果您是 C++ 程序员，这篇博文应该会为您提供一个良好的开端。为此，您需要一台配备支持 CUDA 的 GPU ( Windows、WSL 或 64 位 Linux，任何 NVIDIA GPU 都应配备) 的计算机，或配备 GPU 的云实例 ( AWS、Azure、Google Colab 和其他云服务提供商均配备此类 GPU ) 。您还需要安装免费的 CUDA 工具包 。

我们开始吧！

从简单开始

我们将从一个简单的 C++ 程序开始，该程序可添加两个数组的元素，每个数组包含一百万个元素。

#include <iostream>
#include <math.h>

// function to add the elements of two arrays
void add(int n, float *x, float *y)
{
for (int i = 0; i < n; i++)
y[i] = x[i] + y[i];
}

int main(void)
{
int N = 1<<20; // 1M elements

float *x = new float[N];
float *y = new float[N];

// initialize x and y arrays on the host
for (int i = 0; i < N; i++) {
x[i] = 1.0f;
y[i] = 2.0f;
}

// Run kernel on 1M elements on the CPU
add(N, x, y);

// Check for errors (all values should be 3.0f)
float maxError = 0.0f;
for (int i = 0; i < N; i++)
maxError = fmax(maxError, fabs(y[i]-3.0f));
std::cout << "Max error: " << maxError << std::endl;

// Free memory
delete [] x;
delete [] y;

return 0;
}

首先，编译并运行此 C++ 程序。将上述代码放入文件并另存为 add.cpp，然后使用 C++ 编译器进行编译。我使用的是 Linux，所以我使用的是 g++，但您可以在 Windows 上使用 MSVC (或在 WSL 上使用 g++) 。

> g++ add.cpp -o add

然后运行：

>./add
Max error: 0.000000

(在 Windows 上，您可能希望为可执行文件 add.exe 命名并使用.\add 运行。)

与预期一样，它会打印求和中没有错误，然后退出。现在，我想在 GPU 的多个核心上 (并行) 运行这种计算。迈出第一步其实非常简单。

首先，我只需将 add 函数转换为 GPU 可以运行的函数，即 CUDA 中的 kernel 函数。为此，我所要做的就是将说明符 __global__ 添加到函数中，以告知 CUDA C++ 编译器这是一个在 GPU 上运行的函数，可以从 CPU 代码中调用。

// Kernel function to add the elements of two arrays
__global__
void add(int n, float *sum, float *x, float *y)
{
for (int i = 0; i < n; i++)
sum[i] = x[i] + y[i];
}

此 __global__ function 称为 CUDA 内核 ，在 GPU 上运行。 在 GPU 上运行的代码通常称为设备代码 ，而 在 CPU 上运行的代码则是主机代码 。

CUDA 中的显存分配

要在 GPU 上进行计算，我需要分配 GPU 可访问的内存。CUDA 中的 Unified Memory 通过为系统中的所有 GPU 和 CPU 提供可访问的单一内存空间来简化这一过程。要在 Unified Memory 中分配数据，请调用 cudaMallocManaged() ，它会返回可从主机 (CPU) 代码或设备 (GPU) 代码访问的指针。要释放数据，只需将指针传递给 cudaFree() 即可。

我只需要将上面代码中对 new 的调用替换为对 cudaMallocManaged() 的调用，并将对 delete [] 的调用替换为对 cudaFree 的调用。

// Allocate Unified Memory -- accessible from CPU or GPU
float *x, *y, *sum;
cudaMallocManaged(&x, N*sizeof(float));
cudaMallocManaged(&y, N*sizeof(float));...

// Free memory
cudaFree(x);
cudaFree(y);

最后，我需要启动 add() 内核，以在 GPU 上调用它。CUDA 核函数启动使用三重角度括号语法 <<< >>> 指定。我只需将其添加到参数列表之前的 add 调用中。

add<<<1, 1>>>(N, sum, x, y);

简单！我很快就会详细介绍角括号内的内容；现在您只需要知道这一行启动一个 GPU 线程来运行 add() 。

还有一件事：我需要 CPU 等到 kernel 完成后再访问结果 (因为 CUDA kernel 启动不会阻塞调用 CPU 线程) 。为此，我只需调用 cudaDeviceSynchronize() ，然后再在 CPU 上进行最后一次错误检查。

以下是完整代码：

#include <iostream>
#include <math.h>

// Kernel function to add the elements of two arrays
__global__
void add(int n, float *x, float *y)
{
for (int i = 0; i < n; i++)
y[i] = x[i] + y[i];
}

int main(void)
{
int N = 1<<20;
float *x, *y;

// Allocate Unified Memory – accessible from CPU or GPU
cudaMallocManaged(&x, N*sizeof(float));
cudaMallocManaged(&y, N*sizeof(float));

// initialize x and y arrays on the host
for (int i = 0; i < N; i++) {
x[i] = 1.0f;
y[i] = 2.0f;
}

// Run kernel on 1M elements on the GPU
add<<<1, 1>>>(N, x, y);

// Wait for GPU to finish before accessing on host
cudaDeviceSynchronize();

// Check for errors (all values should be 3.0f)
float maxError = 0.0f;
for (int i = 0; i < N; i++) {
maxError = fmax(maxError, fabs(y[i]-3.0f));
}
std::cout << "Max error: " << maxError << std::endl;

// Free memory
cudaFree(x);
cudaFree(y);
return 0;
}

CUDA 文件的文件扩展名为.cu 。将此代码保存在名为 add.cu 的文件中，并使用 CUDA C++ 编译器 nvcc 进行编译。

> nvcc add.cu -o add_cuda
>./add_cuda
Max error: 0.000000

这只是第一步，因为正如书面所述，此 kernel 仅适用于单个线程，因为运行此 kernel 的每个线程都将对整个数组执行添加操作。此外，由于多个并行线程会读写相同的位置，因此存在 race condition。

注意：在 Windows 上，您需要确保在 Microsoft Visual Studio 的项目配置属性中将 Platform 设置为 x64。

分析它！

要了解内核的运行时间，一个很好的方法是使用 NSight Systems CLI `nsys` 运行内核。我们可以在命令行中输入 nsys profile -t cuda --stats=true./add_cuda 。但是，这会生成详细的统计数据，在本文中，我们只想了解内核运行所需的时间。因此，我编写了一个名为 nsys_easy 的包装器脚本，该脚本仅生成我们需要的输出，并阻止 nsys 生成会使源目录混乱的中间文件。 该脚本可在 GitHub 上获取。 只需下载 nsys_easy ，并将其放在 PATH (甚至当前目录) 中的某个位置即可。(请注意，我从输出中删除了一些统计数据，以便更好地适应本网站的宽度。)

> nsys_easy./add_cuda
Max error: 0
Generating '/tmp/nsys-report-bb25.qdstrm'
[1/1] [========================100%] nsys_easy.nsys-rep
Generated:
/home/nfs/mharris/src/even_easier/nsys_easy.nsys-rep
Generating SQLite file nsys_easy.sqlite from nsys_easy.nsys-rep
Processing 1259 events: [======================================100%]
Processing [nsys_easy.sqlite] with [cuda_gpu_sum.py]...

** CUDA GPU Summary (Kernels/MemOps) (cuda_gpu_sum):

Time (%) Total Time (ns) Count Category Operation
-------- --------------- ----- ----------- --------------------------
98.5 75,403,544 1 CUDA_KERNEL add(int, float *, float *)
1.0 768,480 48 MEMORY_OPER [memcpy Unified H2D]
0.5 352,787 24 MEMORY_OPER [memcpy Unified D2D]

CUDA GPU Summary 表显示了对 add 的一次调用。在 NVIDIA T4 GPU 上大约需要 75 毫秒。让我们通过并行来提高它的速度。

获取线程

现在，您已使用一个线程运行核函数来执行一些计算，如何使其并行？密钥采用 CUDA 的 <<<1, 1>>> 语法。这称为执行配置，它会告知 CUDA 运行时在 GPU 上启动要使用的并行线程数。这里有两个参数，我们先来更改第二个参数：线程块中的线程数。CUDA GPU 使用线程块 (大小为 32 的倍数) 运行内核；256 个线程可合理选择。

add<<<1, 256>>>(N, x, y);

如果我仅在运行代码时进行此更改，它将为每个线程执行一次计算，而不是将计算分散到并行线程。为此，我需要修改内核。CUDA C++ 提供关键字，允许内核获取正在运行的线程的索引。具体而言， threadIdx.x 包含其块内当前线程的索引， blockDim.x 包含块中的线程数。我只需修改循环，即可使用并行线程在数组中大步移动。

__global__
void add(int n, float *x, float *y)
{
int index = threadIdx.x;
int stride = blockDim.x;
for (int i = index; i < n; i += stride)
y[i] = x[i] + y[i];
}

add 函数没有太大变化。事实上，将 index 设置为 0 并将 stride 设置为 1 会使其在语义上与第一个版本相同。

将文件另存为 add_block.cu ，然后再次编译并在 nvprof 中运行。在本文的剩余部分中，我将展示输出中的相关行。

Time (%) Time (ns) Count Category Operation
-------- --------- ----- ----------- --------------------------
79.0 4,221,011 1 CUDA_KERNEL add(int, float *, float *)

这是一个很大的加速 (75ms down to 4ms)，但并不奇怪，因为执行从一个线程扩展到 256 个线程。让我们继续提升性能。

突破限制

CUDA GPU 有许多并行处理器，分为 Streaming Multiprocessors (SM) 。每个 SM 可以运行多个并发线程块，但每个线程块都在单个 SM 上运行。例如，基于 Turing GPU 架构的 NVIDIA T4 GPU 具有 40 个 SM 和 2560 个 CUDA 核心，每个 SM 可支持多达 1024 个活动线程。为了充分利用所有这些线程，我应该启动具有多个线程块的 kernel。

现在，您可能已经猜到执行配置的第一个参数用于指定线程块的数量。并行线程块共同构成了所谓的 grid 。由于要处理 N 元素，且每个线程块有 256 个线程，因此我只需计算线程块的数量即可获得至少 N 个线程。我只需将 N 除以块大小 (如果 N 不是 tg_ 44 的倍数，请注意进行四舍五入) 。

int blockSize = 256;
int numBlocks = (N + blockSize - 1) / blockSize;
add<<<numBlocks, blockSize>>>(N, x, y);

图 1。CUDA 内核中的 Grid、Block 和 Thread 索引 (一维) 。

我还需要更新 kernel 代码，以将整个线程块网格考虑在内。CUDA 提供 gridDim.x (包含网格中的块数量) 和 tg_ 48 (包含网格中当前线程块的索引) 。图 1 说明了使用 blockDim.x 、 gridDim.x 和 tg_ 51 在 CUDA 中索引为数组 (一维) 的方法。其思路是，每个线程通过计算其块开头的偏移量 (块索引乘以块大小： blockIdx.x * blockDim.x ) 并在块 (tg_ 53) 中添加线程索引来获取其索引。代码 blockIdx.x * blockDim.x + threadIdx.x 是惯用的 CUDA。

__global__
void add(int n, float *x, float *y)
{
int index = blockIdx.x * blockDim.x + threadIdx.x;
int stride = blockDim.x * gridDim.x;
for (int i = index; i < n; i += stride)
y[i] = x[i] + y[i];
}

更新后的内核还将 stride 设置为网格中的线程总数 ( blockDim.x * gridDim.x ) 。CUDA 内核中的这类循环通常称为 grid-stride 循环 。

将文件另存为 add_grid.cu ，然后再次编译并在 nvprof 中运行。

Time (%) Time (ns) Count Category Operation
-------- --------- ----- ----------- ----------------------------
79.6 4,514,384 1 CUDA_KERNEL add(int, float *, float *)

这很有趣。这种变化并没有带来加速，而且可能会略有减缓。这是为什么？如果将计算增加 40 倍 (SM 数量) 并不会缩短总时间，那么计算就不是瓶颈。

统一内存预取

如果我们从分析器中查看完整的 summary table，就会发现瓶颈：

Time (%) Time (ns) Count Category Operation
-------- --------- ------ ----------- --------------------------
79.6 4,514,384 1 CUDA_KERNEL add(int, float *, float *)
14.2 807,245 64 MEMORY_OPER [CUDA memcpy Unified H2D]
6.2 353,201 24 MEMORY_OPER [CUDA memcpy Unified D2H]

在这里，我们可以看到有 64 次主机到设备 (H2D) 和 24 次设备到主机 (D2H) “统一” memcpy 运算。但代码中没有明确的 memcpy 调用。CUDA 中的 Unified Memory 是虚拟内存。单个虚拟内存页面可能驻留在系统中任何设备 (GPU 或 CPU) 的内存中，并且这些页面会按需迁移。此程序首先在 for 循环中初始化 CPU 上的数组，然后启动由 GPU 读取和写入数组的内核。由于内核运行时内存页均为 CPU 驻留，因此存在多个分页错误，并且硬件会在发生错误时将分页迁移到 GPU 显存。这会导致内存瓶颈，这也是我们没有看到加速的原因。

迁移成本高昂，因为 page faults 是单独发生的，并且 GPU 线程在等待 page migration 时停止。因为我知道内核需要哪些内存 (x 和 y 数组)，所以我可以使用 prefetching 来确保数据在内核需要之前位于 GPU 上。我在启动 kernel 之前使用 cudaMemPrefetchAsync() 函数来执行此操作：

// Prefetch the x and y arrays to the GPU
cudaMemPrefetchAsync(x, N*sizeof(float), 0, 0);
cudaMemPrefetchAsync(y, N*sizeof(float), 0, 0);

使用分析器运行此过程会产生以下输出。现在，kernel 只需不到 50 微秒！

Time (%) Time (ns) Count Category Operation
-------- --------- ----- ----------- --------------------------
63.2 690,043 4 MEMORY_OPER [CUDA memcpy Unified H2D]
32.4 353,647 24 MEMORY_OPER [CUDA memcpy Unified D2H]
4.4 47,520 1 CUDA_KERNEL add(int, float *, float *)

总结

一次预取数组的所有页面比单个页面错误快得多。请注意，此更改可使所有版本的 add 程序受益，因此，让我们将其添加到所有三个版本中，并在分析器中再次运行它们。这是一个汇总表。

版本

时间

加速与。单线程

带宽

单线程

91811206 纳秒

1x

137 MB/ 秒

单块 ( 256 个线程)

2049，034 纳秒

45 倍

6 GB/ 秒

多个块

47520 纳秒

1932 倍

265 GB/ 秒

数据进入内存后，从单个块到多个块的加速与 GPU 上的 SM 数量 (40) 成正比。

如您所见，我们可以在 GPU 上实现非常高的带宽。加法内核具有很高的带宽限制 ( 265 GB/s 是 T4 峰值带宽 ( 320 GB/s) 的 80% 以上) ，但 GPU 在密集矩阵线性代数、 深度学习 、图像和信号处理、物理模拟等高度计算受限的计算方面也表现出色。

练习

为了让您继续前进，您可以自己尝试以下几点。请在下面的评论区发布您的体验。

浏览 CUDA 工具包文档 。如果您尚未安装 CUDA，请查看 快速入门指南 和安装指南。然后浏览 编程指南 和 最佳实践指南 。我们还提供各种架构的调优指南。

在内核中试验 printf () 。尝试为部分或全部线程打印 threadIdx.x 和 blockIdx.x 的值。它们是否按顺序打印？为什么或者为什么不呢？

打印内核中 threadIdx.y 或 threadIdx.z (或 blockIdx.y ) 的值。(同样适用于 blockDim 和 gridDim )。它们为何存在？如何让它们接受非 0 (dim 为 1) 的值？

从哪里开始？

我希望这篇文章能激发您对 CUDA 的兴趣，并希望您有兴趣了解更多信息并在自己的计算中应用 CUDA C++。如果您有任何疑问或意见，请随时使用下面的评论部分与我们联系。

您可以继续阅读一系列较早的介绍性博文：

如何在 CUDA C++ 中实现 Performance Metrics

如何在 CUDA C++ 中查询设备属性并处理错误

如何在 CUDA C++ 中优化数据传输

如何在 CUDA C++ 中重叠数据传输

如何在 CUDA C++ 中高效访问 Global Memory

在 CUDA C++ 中使用 Shared Memory

CUDA C++ 中的高效矩阵转置

CUDA C++ 中的 Finite Difference Methods，第 1 部分

CUDA C++ 中的 Finite Difference Methods，第 2 部分

借助 CUDA 在一个周末加速 Ray Tracing

此外，还有一系列反映上述内容的 CUDA Fortran 帖子，首先是 An Easy Introduction to CUDA Fortran 。

NVIDIA Developer 博客 上有关于 CUDA C++ 和其他 GPU 计算主题的大量其他内容，请四处看看！

如果您喜欢这篇博文并想了解更多信息， NVIDIA DLI 提供了一些深度 CUDA 编程课程。

如果您是新手，请参阅 现代 CUDA C++ 中的加速计算入门 ，其中提供专用 GPU 资源、更复杂的编程环境、 NVIDIA Nsight Systems 视觉分析器的使用、数十次交互式练习、详细演示、8 小时以上的材料，以及获得 DLI 能力证书的能力。

有关 Python 程序员，请参阅 使用 CUDA Python 的加速计算基础知识 。

有关更多中级和高级 CUDA 编程材料，请参阅 NVIDIA DLI 自定进度目录 的 Accelerated Computing 部分。

点赞

标签

开发工具与技巧 | 数据科学 | 模型/库/框架 | 模拟/建模/设计

关于作者

Mark Harris 是 NVIDIA 杰出的工程师，致力于 RAPIDS 。 Mark 拥有超过 20 年的 GPUs 软件开发经验，从图形和游戏到基于物理的模拟，到并行算法和高性能计算。当他还是北卡罗来纳大学的博士生时，他意识到了一种新生的趋势，并为此创造了一个名字： GPGPU （图形处理单元上的通用计算）。

查看 Mark Harris 所有文章

相关文章

量子经典超级计算机的编程

量子经典超级计算机的编程

用标准语言并行性开发加速代码

用标准语言并行性开发加速代码

CUDA 的更简单介绍

CUDA 的更简单介绍

相关文章

使用 NVIDIA Cosmos 构建用于交互式仿真的世界模型

使用 NVIDIA Cosmos 构建用于交互式仿真的世界模型

为机器学习原子间相互作用势解锁 Tensor Core 性能

为机器学习原子间相互作用势解锁 Tensor Core 性能

使用 AI 智能体教会像 Microdock 这样的机器人在模拟环境中行走

使用 AI 智能体教会像 Microdock 这样的机器人在模拟环境中行走

使用奖励引导推理时间搜索，使用 RFD4-Proteina 设计蛋白质

使用奖励引导推理时间搜索，使用 RFD4-Proteina 设计蛋白质

使用代理辅助工作流开发 AI Factory 数字孪生

使用代理辅助工作流开发 AI Factory 数字孪生

Theme

Light

System

Dark

W

W

Email
