# CUDA 算子手撕与面试指南

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　38,038 字符　｜　513 个片段　｜　更新于 2026-11-13
来源: https://blog.csdn.net/weixin_44286126/article/details/144476582

# CUDA算子手撕与面试指南（CSDN 备份记录）

来源: https://blog.csdn.net/weixin_44286126/article/details/144476582

首页

博客

下载

学习

社区

模型市场

AI搜索

AtomGit

InsCode

技术会议

墨衍
MoGrow

收藏

历史

会员中心

创作中心

CUDA算子手撕与面试指南

原创
2024-12-23 11:17:18
4.2k 阅读

AI写同款

收录于

CUDA

当前文章被收录于：

CUDA

1
篇文章

1
人学习

订阅专栏
查看详情

当前文章被以下社区和专栏收录：

迈克柯里喵

码龄8年

墨衍会员
·
AI 创作全网分发

墨衍智能分发
本文由作者通过墨衍一键同步至各平台

1 分发平台
4.2k 累计阅读
4.3k 平均单平台

已同步平台

CSDN
微信公众号
微博
知乎
掘金
百家号
博客园
抖音
小红书

你的文章也可以这样分发
墨衍会员 ¥399 起/年，写一次发全网

我也要 →

153粉丝 · 27篇原创

关注

CUDA 高性能计 算 面试 问题总结
线程块内的线程可以通过共享内存进行数据交换和协同计 算 ，通常在一个线程块内的线程之间的通信效率更高。全局内存（Global Memory）：全局内存是 GPU 内存中速度较低的一种，用于存储全局数据，可以被所有线程块和线程访问。共享内存（Shared Memory）：共享内存是位于每个线程块内部的内存，用于线程块内的线程之间共享数据。寄存器（Registers）：寄存器是 GPU 内存中速度最快的一种，用于存储每个线程的局部变量和临时数据。所有线程束中的线程执行相同的指令，但可以在不同的数据上操作。

阅读详情

AI摘要

博主秋招找高性能计算岗位，分享CUDA算子手撕代码和知识点。项目汇总面试高频CUDA算子题目及优化策略，涵盖常见算子及优化版本。介绍了elementwise、reduce、transpose等算子的实现与优化，如向量化访存、折半归约等，助求职者备战面试。

引言

最近秋招落幕，期间一直在找高性能计算（HPC）相关岗位，整理了一些 CUDA算子手撕的代码和知识点 分享给大家。

项目地址 ： https://github.com/Tongkaio/CUDA_Kernel_Samples

如果觉得本项目对你有帮助，欢迎给项目点个 ⭐ 哦 ~

项目介绍

本项目是 CUDA 算子开发基础与面试指南 ：

汇总了面试高频的 CUDA 算子题目和优化策略，包含面试高频算子的编写示例
项目从算子 naive 实现到优化版本均包含完整代码，便于调试与性能分析
每个算子附有相关的 GPU 知识点，帮助求职者高效备战 CUDA 编程面试。

目前覆盖以下 CUDA 常见算子及其优化版本：

文件夹
描述
内容

example
一些简单的例子
/

elementwise
数组对应元素计算
add

gemv
矩阵乘向量
sgemv

reduce
归约计算优化
sum, max, softmax, softmax_matrix

sgemm
矩阵乘优化
naive, blocktile, threadtile, …

transpose
矩阵转置优化
naive, 优化访存并解决bank conflict

算子手撕说明

面试时不会提供 CUDA 运行环境，也不会要求完整写出可以运行的代码，通常只需要写出 CUDA 算子函数（大部分情况只需要写这个），block_size，grid_size 和函数调用。

在此列出一些宏，后面会用到：

// 1. 向上取整
# define CEIL ( a, b ) ( ( a + b - 1 ) / ( b ) )

// 2. FLOAT4，用于向量化访存，以下两种都可以
// c写法
# define FLOAT4 ( value ) * ( float4 * ) ( & ( value ) )

// c++写法
# define FLOAT4 ( value ) ( reinterpret_cast < float4 * > ( & ( value ) ) [ 0 ] )

本文剩余篇幅从这一角度出发，展示必要的代码，以供参考和练习。

elementwise

考察频率 ：低

算子描述 ：elementwise 是最简单的 一类算子 ，其指的是对数据进行逐元素操作，例如将两个等长的数组对应元素相加（ add ）。另外在深度学习中，激活函数会对输入数据的每个元素求对应激活值，故激活函数也算在 elementwise 范围内。

算子主要分两种写法：

naive：每个线程负责一个元素的运算
使用 float4 等向量化访存方式：只对大规模数据有加速效果，需要注意， 要在 grid 上除以 4 ，而不是在 block 上除以 4，否则会降低SM的占用率（Occupancy，实际运行的线程数 / SM 理论最大线程数），可以参考👉 grid_size 和 block_size 选择 （block_size 不小于SM上最大支持的线程数/最大同时执行的block数量，且得是SM上最大支持的线程数的约数，才有可能达到100% Occupancy）。向量化存取的好处在于可以减少访存指令的数量，单位时间内读取的数据量变多，增大访存带宽。

源码文件夹 ： elementwise

add

源码： add.cu

naive版

// block_size，grid_size 和函数调用
int block_size = 1024;
int grid_size = CEIL ( N, block_size );
elementwise_add << < grid_size, block_size >> > ( a, b, c, N );

// 函数定义
__global__ void elementwise_add ( float * a, float * b, float * c, int N ) {
int idx = blockDim. x * blockIdx. x + threadIdx. x;
if ( idx < N ) {
c [ idx ] = a [ idx ] + b [ idx ];
}
}

使用向量化访存

使用向量化访存进行优化，需要注意， 要在 grid 上除以 4 ：

int block_size = 1024;
int grid_size = CEIL ( CEIL ( N, 4 ), block_size ); // 注：在grid维度除以4
elementwise_add << < grid_size, block_size >> > ( a, b, c, N );

__global__ void elementwise_add_float4 ( float * a, float * b, float * c, int N ) {
int idx = ( blockDim. x * blockIdx. x + threadIdx. x ) * 4;

if ( idx < N ) {
float4 tmp_a = FLOAT4 ( a [ idx ] );
float4 tmp_b = FLOAT4 ( b [ idx ] );
float4 tmp_c;
tmp_c. x = tmp_a. x + tmp_b. x;
tmp_c. y = tmp_a. y + tmp_b. y;
tmp_c. z = tmp_a. z + tmp_b. z;
tmp_c. w = tmp_a. w + tmp_b. w;
FLOAT4 ( c [ idx ] ) = tmp_c;
}
}

以下算子的 block_size, grid_size, 函数调用与 add 的写法相同, 不再重复写出。

sigmoid

σ

(

x

)

=

1

1

+

e

−

x

\sigma(x) = \frac{1}{1 + e^{-x}}

σ ( x ) = 1 + e − x 1 ​

__global__ void sigmoid ( float * x, float * y, int N ) {
int idx = blockIdx. x * blockDim. x + threadIdx. x;
if ( idx < N ) y [ idx ] = 1.0f / ( 1.0f + expf ( - x [ idx ] ) );
}

// float4
__global__ void sigmoid_float4 ( float * x, float * y, int N ) {
int idx = ( blockDim. x * blockIdx. x + threadIdx. x ) * 4;
if ( idx < N ) {
float4 tmp_x = FLOAT4 ( x [ idx ] );
float4 tmp_y;
tmp_y. x = 1.0f / ( 1.0f + expf ( - tmp_x. x ) );
tmp_y. y = 1.0f / ( 1.0f + expf ( - tmp_x. y ) );
tmp_y. z = 1.0f / ( 1.0f + expf ( - tmp_x. z ) );
tmp_y. w = 1.0f / ( 1.0f + expf ( - tmp_x. w ) );
FLOAT4 ( y [ idx ] ) = tmp_y;
}
}

relu

ReLU

(

x

)

=

max

⁡

(

0,

x

)

\text{ReLU}(x) = \max(0, x)

ReLU ( x ) = max ( 0, x )

__global__ void relu ( float * x, float * y, int N ) {
int idx = blockDim. x * blockIdx. x + threadIdx. x;
if ( idx < N ) y [ idx ] = fmaxf ( 0.0f, x [ idx ] );
}

// float4
__global__ void relu_float4 ( float * x, float * y, int N ) {
int idx = ( blockDim. x * blockIdx. x + threadIdx. x ) * 4;
if ( idx < N ) {
float4 tmp_x = FLOAT4 ( x [ idx ] );
float4 tmp_y;
tmp_y. x = fmaxf ( 0.0f, tmp_x. x );
tmp_y. y = fmaxf ( 0.0f, tmp_x. y );
tmp_y. z = fmaxf ( 0.0f, tmp_x. z );
tmp_y. w = fmaxf ( 0.0f, tmp_x. w );
FLOAT4 ( y [ idx ] ) = tmp_y;
}
}

reduce

考察频率 ：高

算子描述 ：reduce 是一种聚合操作，通常用于将一个多元素的数据结构（如数组或张量）通过某种规则归约为一个更小的数据结构（通常是单个值或更小的数组）。它广泛应用于数据处理、并行计算以及深度学习中。例如对数组进行求和 (sum)，求均值 (mean)，求最大值 (max)，还有求 softmax。其中， sum 和 softmax 的考察频率最高 。

源码文件夹 ： reduce

sum

源码： sum.cu

naive版

每个线程通过原子函数 atomicAdd ，往同一个全局内存里面写数据，原子函数会导致线程变成序列化，丧失并行性，算子性能大大降低，不能滥用：

dim3 block_size ( BLOCK_SIZE ); // BLOCK_SIZE 是通过宏定义的某个数字
dim3 grid_size ( CIEL ( N, BLOCK_SIZE ) );
reduce_v1 << < grid_size, block_size >> > ( d_x, d_y, N );

__global__ void reduce_v1 ( const float * input, float * output, int N ) {
int idx = blockDim. x * blockIdx. x + threadIdx. x;
if ( idx < N ) atomicAdd ( output, input [ idx ] );
}

折半归约

在block内进行折半归约，一个block归约一部分，先搬到自己 block 内的 shared_memory 下，然后归约到首元素。

这种方法的缺点是 BLOCK_SIZE 必须是 2 的幂次，否则折半操作时会计算出错，导致误差很大。而且每次迭代折半时必须使用 __syncthreads() 进行同步，会强制所有线程在同步点等待，直到线程块中的其他线程也到达。会导致性能下降。

dim3 block_size ( BLOCK_SIZE ); // BLOCK_SIZE 是通过宏定义的某个数字
dim3 grid_size ( CIEL ( N, BLOCK_SIZE ) );
reduce_v2 << < grid_size, block_size >> > ( d_x, d_y, N );

__global__ void reduce_v2 ( const float * input, float * output, int N ) {
int tid = threadIdx. x;
int idx = blockDim. x * blockIdx. x + threadIdx. x;
__shared__ float input_s [ BLOCK_SIZE ];

// 1. 搬运和线程数量(blockDim.x)相等的数据，到当前block的共享内存中
input_s [ tid ] = ( idx < N )? input [ idx ] : 0.0f;
__syncthreads ( );

// 2. 用1/2, 1/4, 1/8...的线程进行折半归约
for ( int offset = blockDim. x >> 1; offset > 0; offset >>= 1 ) {
if ( tid < offset ) { // 2.折半归约
input_s [ tid ] += input_s [ tid + offset ];
}
__syncthreads ( );
}

// 3. 每个block的第一个线程将计算结果累加到输出中
if ( tid == 0 ) atomicAdd ( output, input_s [ 0 ] );
}

warp shuffle（推荐写法）

在 warp 内进行折半归约，其优势在于，一个 warp 内的线程是同步的，相比于以 block 为单位进行折半，以 warp 为单位进行每次折半时不需要 __syncthreads() ，并行性更高。

BLOCK_SIZE需要是32的整数倍，否则产生线程数不足32的warp，可能会导致访问到无效数据。

使用 CUDA 提供的 warp shuffle 操作 ，有以下函数可以用：

__shfl_sync() ：拷贝来自任意laneId(0~31)线程里的值
__shf_xor_sync() ：拷贝来自一个计算出来的laneId(0~31)线程里的值
__shfl_up_sync() ：拷贝来自有一定偏移量laneId更小的线程里的值
__sync_down_sync() ：拷贝来自有一定偏移量laneId更大的线程里的值

其中 __shf_xor_sync() 和 __sync_down_sync() 使用频率较高。

dim3 block_size ( BLOCK_SIZE );
dim3 grid_size ( CIEL ( N, BLOCK_SIZE ) );
reduce_v3 << < grid_size, block_size >> > ( d_x, d_y, N )

__global__ void reduce_v3 ( float * d_x, float * d_y, const int N ) {
__shared__ float s_y [ 32 ]; // 仅需要32个，因为一个block最多1024个线程，最多1024/32=32个warp

int idx = blockDim. x * blockIdx. x + threadIdx. x;
int warpId = threadIdx. x / warpSize; // 当前线程属于哪个warp
int laneId = threadIdx. x % warpSize; // 当前线程是warp中的第几个线程

float val = ( idx < N )? d_x [ idx ] : 0.0f; // 搬运d_x[idx]到当前线程的寄存器中
# pragma unroll
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset ); // 在一个warp里折半归约
}

if ( laneId == 0 ) s_y [ warpId ] = val; // 每个warp里的第一个线程，负责将数据存储到shared mem中
__syncthreads ( );

if ( warpId == 0 ) { // 使用每个block中的第一个warp对s_y进行最后的归约
int warpNum = blockDim. x / warpSize; // 每个block中的warp数量
val = ( laneId < warpNum )? s_y [ laneId ] : 0.0f;
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset );
}
if ( laneId == 0 ) atomicAdd ( d_y, val ); // 使用此warp中的第一个线程，将结果累加到输出
}
}

warp shuffle + float4

在 warp shuffle 上进一步优化，搬运数据时使用 float4：

# define FLOAT4 ( value ) ( float4 * ) ( & ( value ) ) [ 0 ]
dim3 block_size ( BLOCK_SIZE );
dim3 grid_size ( CEIL ( CIEL ( N, BLOCK_SIZE ), 4 ) ); // 这里要除以4
reduce_v3 << < grid_size, block_size >> > ( d_x, d_y, N )

__global__ void reduce_v4 ( float * d_x, float * d_y, const int N ) {
__shared__ float s_y [ 32 ];
int idx = ( blockDim. x * blockIdx. x + threadIdx. x ) * 4; // 这里要乘以4
int warpId = threadIdx. x / warpSize; // 当前线程位于第几个warp
int laneId = threadIdx. x % warpSize; // 当前线程是warp中的第几个线程
float val = 0.0f;
if ( idx < N ) {
float4 tmp_x = FLOAT4 ( d_x [ idx ] );
val += tmp_x. x;
val += tmp_x. y;
val += tmp_x. z;
val += tmp_x. w;
}
# pragma unroll
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset );
}

if ( laneId == 0 ) s_y [ warpId ] = val;
__syncthreads ( );

if ( warpId == 0 ) {
int warpNum = blockDim. x / warpSize;
val = ( laneId < warpNum )? s_y [ laneId ] : 0.0f;
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset );
}
if ( landId == 0 ) atomicAdd ( d_y, val );
}
}

SoftMax

Softmax 的 CPU 和 CUDA 写法均是高频考察。面试时有可能会让任选一种写法进行书写，此时自己可以量力而行。

源码： softmax.cu

Softmax公式如下：

Softmax

(

x

i

)

=

e

x

i

∑

j

=

1

N

e

x

j

\text{Softmax}(x_i) = \frac{e^{x_i}}{\sum_{j=1}^{N} e^{x_j}}

Softmax ( x i ​ ) = ∑ j = 1 N ​ e x j ​ e x i ​ ​

一般为了避免溢出，需要减去最大值，所以通常采用下面这个公式：

Softmax

(

x

i

)

=

e

x

i

−

M

∑

j

=

1

N

(

e

x

j

−

M

)

\text{Softmax}(x_i) = \frac{e^{x_i-M}}{\sum_{j=1}^{N} (e^{x_j-M})}

Softmax ( x i ​ ) = ∑ j = 1 N ​ ( e x j ​ − M ) e x i ​ − M ​

其中

M

M

M 是输入向量的最大值。

CPU 写法

void softmax ( float * input, float * output, int N ) {
int M = * ( std :: max_element ( input, input + N ) );
float div = 0;
for ( int i = 0; i < N; i ++ ) {
output [ i ] = std :: exp ( input [ i ] - M );
div += output [ i ];
}
for ( int i = 0; i < N; i ++ ) {
output [ i ] /= div;
}
}

CUDA写法

最直接的思路是将 Softmax 计算过程拆分为多个归约算子，只要会写归约，那么 Softmax 就能写。

这种写法的优点是比较简单，虽然代码比较多，但基本都是采用归约的写法，几个算子的逻辑上差异不大。缺点是算子效率比较低。 这里建议学习 softmax_matrix 的写法！

思路：

核函数1：归约求最值 max_val
核函数2：归约求和 sum
核函数3：计算每个元素减去 max_val 除以 sum。

__device__ static float atomicMax ( float * address, float val ) {
int * address_as_i = ( int * ) address;
int old = * address_as_i;
int assumed;
do {
assumed = old;
old = atomicCAS ( address_as_i, assumed, __float_as_int ( fmaxf ( val, __int_as_float ( assumed ) ) ) );
} while ( assumed!= old );
return __int_as_float ( old );
}

__global__ void max_kernel ( float * input, float * max_val, int N ) {
__shared__ float s_mem [ 32 ];
int idx = blockDim. x * blockIdx. x + threadIdx. x;
int warpId = threadIdx. x / warpSize;
int laneId = threadIdx. x % warpSize;

float val = ( idx < N )? input [ idx ] : ( - FLT_MAX );
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val = fmaxf ( val, __shfl_down_sync ( 0xFFFFFFFF, val, offset ) );
}
if ( laneId == 0 ) s_mem [ warpId ] = val;
__syncthreads ( );

if ( warpId == 0 ) {
int warpNum = blockDim. x / warpSize;
val = ( laneId < warpNum )? s_mem [ laneId ] : ( - FLT_MAX );
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val = fmaxf ( val, __shfl_down_sync ( 0xFFFFFFFF, val, offset ) );
}
if ( laneId == 0 ) atomicMax ( max_val, val );
}
}

__global__ void sum_kernel ( float * input, float * sum, float * max_val, int N ) {
__shared__ float s_mem [ 32 ];
int idx = blockDim. x * blockIdx. x + threadIdx. x;
int warpId = threadIdx. x / warpSize;
int laneId = threadIdx. x % warpSize;

float val = ( idx < N )? expf ( input [ idx ] - * max_val ) : 0.0f;
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset );
}
if ( laneId == 0 ) s_mem [ warpId ] = val;
__syncthreads ( );

if ( warpId == 0 ) {
int warpNum = blockDim. x / warpSize;
val = ( laneId < warpNum )? s_mem [ laneId ] : 0.0f;
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
val += __shfl_down_sync ( 0xFFFFFFFF, val, offset );
}
if ( laneId == 0 ) atomicAdd ( sum, val );
}
}

__global__ void softmax_kernel ( float * input, float * output, float * sum, float * max_val, int N ) {
int idx = blockDim. x * blockIdx. x + threadIdx. x;
if ( idx < N ) output [ idx ] = expf ( input [ idx ] - * max_val ) / ( * sum );
}

// 初始化相关变量
//...
// 调用
int block_size = 256;
int grid_size = CEIL ( N, block_size );
max_kernel << < gird_size, block_size >> > ( input, max_val, N );
sum_kernel << < gird_size, block_size >> > ( input, sum, max_val, N );
softmax_kernel << < gird_size, block_size >> > ( input, output, sum, max_val, N );

transpose

考察频率 ：中

算子描述 ：指的是矩阵转置，其中会涉及到 GPU 全局内存的高效访问、bank conflict 知识点。

如何优化全局内存的访问：

尽量合并访问 ，即连续的线程读取连续的内存，且尽量让访问的全局内存的首地址是32字节（一次数据传输处理的数据量）的倍数（cudaMalloc分配的至少是256字节整数倍）；
如果不能同时合并读取和写入，则应该 尽量做到合并写入 ，因为编译器如果能判断一个全局内存变量在核函数内是只可读的，会自动调用 __ldg() 读取全局内存，从而对数据进行缓存，缓解非合并访问带来的影响，但这只对读取有效，写入则没有类似的函数。另外，对于开普勒架构和麦克斯韦架构，需要显式的使用 __ldg() 函数，例如 B[ny * N + nx] = __ldg(&A[nx * N + ny]) 。

源码文件夹 ： transpose

naive：

__global__ void transpose ( float * input, float * output, int M, int N ) {
// input的row和col
int row = blockDim. y * blockIdx. y + threadIdx. y;
int col = blockDim. x * blockIdx. x + threadIdx. x;

if ( row < M && col < N ) {
output [ col * M + row ] = input [ row * N + col ];
}
}

仅合并写入：

__global__ void transpose ( float * input, float * output, int M, int N ) {
// output的row和col
int row = blockDim. y * blockIdx. y + threadIdx. y;
int col = blockDim. x * blockIdx. x + threadIdx. x;

if ( row < N && col < M ) {
output [ row * M + col ] = __ldg ( & input [ col * N + row ] ); // 合并写入，读取使用__ldg进行缓存
}
}

使用共享内存中转，同时合并读取和写入（ 推荐 ）：

// 输入矩阵是M行N列，输出矩阵是N行M列
dim3 block ( 32, 32 );
dim3 grid ( CEIL ( N, 32 ), CEIL ( M, 32 ) );
transpose < 32 > << < grid, block >> > ( input, output, M, N );

template < const int BLOCK_SIZE >
__global__ void transpose ( float * input, float * output, int M, int N ) {
__shared__ float s_mem [ BLOCK_SIZE ] [ BLOCK_SIZE + 1 ]; // 避免bank conflict
int bx = blockIdx. x * BLOCK_SIZE;
int by = blockIdx. y * BLOCK_SIZE;

int x1 = bx + threadIdx. x;
int y1 = by + threadIdx. y;
if ( x1 < N && y1 < M ) {
s_mem [ threadIdx. y ] [ threadIdx. x ] = input [ y1 * N + x1 ];
}
__syncthreads ( );

int x2 = by + threadIdx. x;
int y2 = bx + threadIdx. y;
if ( x2 < M && y2 < N ) {
output [ y2 * M + x2 ] = s_mem [ threadIdx. x ] [ threadIdx. y ]; // padding后，不存在bank conflict
}
}

sgemm

考察频率 ：中

算子描述 ：指的是矩阵乘。矩阵乘是 CUDA 学习时的经典案例，涉及多种 CUDA 编程中的常用优化技巧。建议阅读 CUDA SGEMM 优化 。但手撕时难度往往较大，建议优先掌握最简单的 naive 版本以及 block_tile 版本。掌握 block_tile 版本后，只需要加一些代码就可以优化为 thread_tile 版本，故也可以考虑掌握。其余的更高效的优化版本，个人认为了解其原理即可，不必强求面试时手写。

源码文件夹 ：[sgemm]( CUDA_Kernel_Samples/sgemm at master · Tongkaio/CUDA_Kernel_Samples )

naive 版

// C(MxN) = A(MxK) * B(KxN) 行优先
// 每个线程处理一个输出矩阵中的元素

// 假设 M N K 已经赋值
const int BLOCK_SIZE = 32;
dim3 block ( BLOCK_SIZE, BLOCK_SIZE );
dim3 grid ( CEIL ( N, BLOCK_SIZE ), CEIL ( M, BLOCK_SIZE ) );
sgemm << < grid, block >> > ( d_A, d_B, d_C, M, N, K );

__global__ void sgemm ( float * A, float * B, float * C, int M, int N, int K ) {
int col = blockDim. x * blockIdx. x + threadIdx. x;
int row = blockDim. y * blockIdx. y + threadIdx. y;
if ( row >= M || col >= N ) return;

float accum = 0.0f;
for ( int i = 0; i < K; i ++ ) {
accum += A [ row * K + i ] * B [ i * N + col ];
}

C [ row * N + col ] = accum;
}

block_tile 版本

还是一个线程计算一个输出矩阵中的元素，但是用 shared mem 做缓存，重复从 shared mem 中读取，而不是从 global mem，虽然读取次数没变少，但是 shared mem 比 global mem 读取速度快：

# define BLOCK_SIZE 32

dim3 block ( BLOCK_SIZE, BLOCK_SIZE );
dim3 grid ( CEIL ( N, BLOCK_SIZE ), CEIL ( M, BLOCK_SIZE ) );
sgemm << < grid, block >> > ( d_A, d_B, d_C, M, N, K );

__global__ void sgemm ( float * A, float * B, float * C, int M, int N, int K ) {
int idx = blockDim. x * blockIdx. x + threadIdx. x;
int idy = blockDim. y * blockIdx. y + threadIdx. y;
if ( idx >= M || idy >= N ) return;

int bx = blockIdx. x;
int by = blockIdx. y;
int tx = threadIdx. x;
int ty = threadIdx. y;

const int BM = BLOCK_SIZE;
const int BN = BLOCK_SIZE;
const int BK = BLOCK_SIZE;
__shared__ float As [ BM * BK ];
__shared__ float Bs [ BK * BN ];

// 初始化block tile起始位置
A = & A [ ( by * BM ) * K ];
B = & B [ bx * BN ];
C = & C [ ( by * BM ) * N + bx * BN ];

float accum = 0.0f;
for ( int k = 0; k < K; k += BK ) {
// 搬运 global ==> shared
As [ ty * BK + tx ] = A [ ty * K + tx ];
Bs [ ty * BN + tx ] = B [ ty * N + tx ];
__syncthreads ( );
A = A + BK;
B = B + BK * N;
for ( int i = 0; i < BK; i ++ ) {
accum += As [ ty * BK + i ] * Bs [ i * BN + tx ];
}
__syncthreads ( );
}

C [ ty * N + tx ] = accum;
}

thread_tile

一个线程承担更多的计算，更加高效：

dim3 block ( 256 );
dim3 grid ( CEIL ( N, 128 ), CEIL ( M, 128 ) );
sgemm < 128, 128, 8, 8, 8 > << < grid, block >> > ( A, B, C, M, N, K );

template < const int BM,
const int BN,
const int BK,
const int TM,
const int TN >
__global__ void sgemm ( float * A, float * B, float * C, int M, int N, int K ) {
int bx = blockIdx. x;
int by = blockIdy. y;

int block_row_thread = BN / TN; // block中一行的thread数量
int block_col_thread = BM / TM; // block中一列的thread数量
int thread_num = block_row_thread * block_col_thread; // block中thread总量

int tx = ( threadIdx. x % block_row_thread ) * TN; // threadtile左上角x坐标
int ty = ( threadIdx. x / block_row_thread ) * TM; // threadtile左上角y坐标

__shared__ float As [ BM * BK ];
__shared__ float Bs [ BK * BN ];

A = & A [ by * BM * K ];
B = & B [ bx * BN ];
C = & C [ by * BM * N + bx * BN ];

int a_tile_row = threadIdx. x / BK;
int a_tile_col = threadIdx. x % BK;
int a_tile_stride = thread_num / BK; // BM/(BM/(thread_num/BK)) = thread_num/BK = stride

int b_tile_row = threadIdx. x / BN;
int b_tile_col = threadIdx. x % BN;
int b_tile_stride = thread_num / BN;

float accum [ TM ] [ TN ] = { 0.0f };
for ( int k = 0; k < K; k += BK ) {
for ( int i = 0; i < BM; i += a_tile_stride ) {
As [ ( a_tile_row + i ) * BK + a_tile_col ] = A [ ( a_tile_row + i ) * K + a_tile_col ];
}
for ( int i = 0; i < BK; i += b_tile_stride ) {
Bs [ ( b_tile_row + i ) * BN + b_tile_col ] = B [ ( b_tile_row + i ) * N + b_tile_col ];
}
__syncthreads ( );

A += BK;
B += BK * N;

for ( int row = 0; row < TM; row ++ ) {
for ( int col = 0; col < TN; col ++ ) {
for ( int i = 0; i < BK; i ++ ) {
accum [ row ] [ col ] += As [ ( ty + row ) * BK + i ] * Bs [ i * BN + ( tx + col ) ];
}
}
}
__syncthreads ( );
}
for ( int row = 0; row < TM; row ++ ) {
for ( int col = 0; col < TN; col ++ ) {
C [ ( ty + row ) * N + ( tx + col ) ] = accum [ row ] [ col ];
}
}
}

gemv

考察频率 ：低

算子描述 ：求一个矩阵乘以一个向量，方法是每个block中有一个warp，每个warp负责一行的计算。虽然面试考察频率不大但，推荐学习并了解。因为 gemv 中使用一个 warp 负责一行的计算方式，可以拓展到对一个矩阵按行求归约（ 面试时有概率会考察二维矩阵的按行求归约，而不只是一维数组 ）

源码文件夹 ： gemv

gemv

// 行数: M = 1024
// 列数: K = 32
// block数量和行数相同: grid_size = M
// 每个block里一个warp: block_size = 32
sgemv << < grid_size, block_size >> > ( A, x, y, M, K );
__global__ void sgemv ( float * A, float * x, float * y, int M, int K ) {
int laneId = threadIdx. x % warpSize;
int row = blockIdx. x; // 0~M-1
if ( row >= M ) return;

float res = 0.0f;
int kIteration = CEIL ( K, warpSize ); // 每个线程需要负责计算的数据个数

for ( int i = 0; i < kIteration; i ++ ) {
int col = i * warpSize + laneId;
res += ( col < K )? A [ row * K + col ] * x [ col ] : 0.0f;
}

for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
res += __shfl_down_sync ( 0xFFFFFFFF, res, offset );
}

if ( laneId == 0 ) y [ row ] = res;
}

拓展应用

了解了 gemv 后，按照同样的思路，我们可以写出对 MxN 的矩阵，每一行求 softmax。M = 1 时，问题变为对一个长度为 N 的数组求 softmax。

softmax_matrix

源码： softmax_matrix.cu

对一个 MxN 的矩阵，每一行求 softmax，思路同样是每个 warp 处理一行，用这个 warp 对一行进行求和、求最值，计算结果存入共享内存，然后每个元素求 softmax：

__global__ void softmax_kernel ( float * input, float * output, int M, int N ) {
__shared__ float s_max_val;
__shared__ float s_sum;
int laneId = threadIdx. x % warpSize;
// 当前行
int row = blockIdx. x;
if ( row >= M ) return;

int iteration = CEIL ( N, warpSize ); // 每个线程负责计算的数据个数

// 求每一行最大值
float max_val = - FLT_MAX;
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
max_val = ( col < N )? fmaxf ( max_val, input [ row * N + col ] ) : max_val;
}
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
max_val = fmaxf ( max_val, __shfl_down_sync ( 0xFFFFFFFF, max_val, offset ) );
}
if ( laneId == 0 ) s_max_val = max_val; // 最大值汇总到第一个线程，第一个线程将它搬运到s_mem

// 求每一行的和，且要减去最大值
float sum = 0.0f;
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
sum += ( col < N )? expf ( input [ row * N + col ] - s_max_val ) : 0.0f;
}
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
sum += __shfl_down_sync ( 0xFFFFFFFF, sum, offset );
}
if ( laneId == 0 ) s_sum = sum; // sum值汇总到第一个线程，第一个线程将它搬运到s_mem

// 计算每一行的softmax
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
if ( col < N ) output [ row * N + col ] = expf ( input [ row * N + col ] - s_max_val ) / s_sum;
}
}

改用 __shfl_xor_sync 后，每个线程的寄存器的 max_val 和 sum 都是最终的结果，就不用写到共享内存再读取了：

dim3 block ( 32 );
dim3 grid ( M );

__global__ void softmax_kernel ( float * input, float * output, int M, int N ) {
int laneId = threadIdx. x % warpSize;
// 当前行
int row = blockIdx. x;
if ( row >= M ) return;

int iteration = CEIL ( N, warpSize ); // 每个线程负责计算的数据个数

// 求每一行最大值
float max_val = - FLT_MAX;
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
max_val = ( col < N )? fmaxf ( max_val, input [ row * N + col ] ) : max_val;
}
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
max_val = fmaxf ( max_val, __shfl_xor_sync ( 0xFFFFFFFF, max_val, offset ) );
}

// 求每一行的和，且要减去最大值
float sum = 0.0f;
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
sum += ( col < N )? expf ( input [ row * N + col ] - max_val ) : 0.0f;
}
for ( int offset = warpSize >> 1; offset > 0; offset >>= 1 ) {
sum += __shfl_xor_sync ( 0xFFFFFFFF, sum, offset );
}

// 计算每一行的softmax
for ( int i = 0; i < iteration; i ++ ) {
int col = i * warpSize + laneId;
if ( col < N ) output [ row * N + col ] = expf ( input [ row * N + col ] - max_val ) / sum;
}
}

进一步地， 当行数 M = 1，问题退化为对一个长度为 N 的数组进行归约求和 。可以自行编写。

标签

#面试
#职场和发展
#c++
#gpu算力
#求职招聘

#性能优化
#算法

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

cuda 面试 准备（一），架构调试
这个错误往往会伴随fatal error LNK1169: 找到一个或多个多重定义的符号出现。在此吐槽一下英某达链接器的程序猿&mdash;&mdash;只能链接多个lib和一个.cu文件。并建议开发者们摒弃原有的多文件编程方法，函数和类声明即实现：传统多文件编程技巧&mdash;&mdash;头文件声明函数，源文件实现函数 cuda 正确的多文件编程方法，声明即实现。

阅读详情

相关推荐

换一换

【面经笔记】 CUDA

http://blog.csdn.net/junparadox/article/details/50540602
http://www.cnblogs.com/muchen/p/6306792.html
与 CUDA 相关的几个概念：thread，block，grid，warp，sp，sm。
sp: 小核（流处理器）最基本的处理单元，streaming processor 最后具体的指令和任务都

xiaxzhou的博客

6861

CUDA 编程 面试 高频30题

CUDA （Compute Unified Device Architecture）是由NVIDIA开发的一种并行计 算 平台和应用程序接口模型。它允许开发者利用NVIDIA GPU 进行通用计 算 任务，而不仅仅是图形渲染。 CUDA 提供了一个编程模型，使得开发者可以通过C、 C++ 或Fortran等语言编写程序，并在支持 CUDA 的 GPU 上运行这些程序以获得显著的性能提升。Warp是 GPU 执行的基本单位，通常包含32个线程。Warp内的线程以单指令多线程（SIMT）方式执行相同的指令流。

m0_37755875的博客

2590

PyTorch 手 撕 练习：从基础到工程实战

深度学习框架PyTorch作为当前主流的机器学习工具，其核心价值在于动态计 算 图和自动微分系统。通过张量运 算 构建计 算 图，autograd机制自动计 算 梯度，这种设计既保留了灵活性又保证了性能。理解底层实现原理对模型调试和 性能优化 至关重要，比如 手 动实现反向传播可以深入理解梯度流动，而自定义 CUDA 算子 开发则能提升计 算 效率。HappyTorch项目通过 手 撕 ResNet、Transformer等经典模型，帮助开发者掌握PyTorch工程实践中的关键技巧，如混合精度训练、内存优化等高频技术难点，特别适合需要突破PyTo

weixin_30516429的博客

254

CUDA 面试 题库（含答案）一

涵盖： CUDA 历史概述、 GPU 和CPU的区别以及各自的优点和应用、并行计 算 模型：SIMD、MIMD、SPMD等、NVIDIA的 GPU 硬件架构，包括流多处理器（SM）、线程处理核心（ CUDA cores）、全局内存、共享内存等、线程、线程块、网格以及它们在 GPU 上的执行方式。3、请详细描述NVIDIA的 GPU 硬件架构，包括流多处理器（SM）、线程处理核心（ CUDA cores）、全局内存、共享内存等，并解释其在并行计 算 中的角色和作用。10、在NVIDIA的 GPU 架构中，流处理器（SM）的主要功能是什么？

tarzanlee666的博客

1638

【scp】Windows 下scp报错 bad permissions

Windows 下使用 SSH 连接报 Bad owner or permissions on C:\\Users\\Administrator/.ssh/config 错误问题解决

C:\Users\Administrator>scp -P 66666 D:\1111.pdf zzd@58.213.45.xx:/home/zzd/
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING:...

走在搬砖的路上！

4155

基于STM32的HC-SR04超声波模块测距（新 手 级）

本人是嵌入式新 手 ，学习了STM32的部分内容，最近想找些项目做，然后顺便学习了一下HC-SR04超声波模块，学习过程中也参考了一些大佬的文章。本文记录了自己的学习方法和学习过程，同时也希望能和各位大佬交流讨论。

weixin_63758495的博客

2206

CUDA _Kernel_Samples完全 指南 ：从零基础到掌握高频 面试 算子

CUDA _Kernel_Samples是一份全面的 CUDA 算子 手 撕 与 面试 指南 ，汇总了 面试 高频的 CUDA 算子 题目和优化策略，包含从naive实现到优化版本的完整代码，帮助开发者高效备战 CUDA 编程 面试 。本文将带你从零基础开始，逐步掌握各类高频考察的 CUDA 算子 实现 与 优化技巧。

## 🚀 为什么选择 CUDA _Kernel_Samples？

对于 CUDA 开发者和深度学习工程师来说，掌握 算子 优化是提

gitblog_00051的博客

1万+

CCS环境下ADC采样波形的实时可视化调试技巧

本文详细介绍了在CCS环境下实现ADC采样波形实时可视化的调试技巧。通过数组缓存优化、多通道同步显示和波形测量工具的高级用法，帮助开发者高效调试嵌入式系统中的ADC采样问题，提升信号分析的准确性和效率。

weixin_33712987的博客

419

CUDA 面试 必备：GEMV 算子 实现 与 矩阵按行归约技巧

在 CUDA 编程 面试 中，掌握GEMV（General Matrix-Vector multiplication） 算子 的高效实现和矩阵按行归约技巧是区分初级 与 高级开发者的关键。本文将为你揭秘这两个核心技术的实现原理 与 优化策略，帮助你在 面试 中脱颖而出！🚀

## 为什么GEMV 算子 和矩阵归约如此重要？

GEMV（矩阵-向量乘法）是深度学习、科学计 算 和高性能计 算 中的基础操作。无论是神经网络的前向传播还

gitblog_00100的博客

1000

LLCE、PFE模块，二层交换/桥接的通用学习转发机制

NXP 文档里给出的正式全称如下：面向 CAN/CAN-FD、LIN、FlexRay 的&ldquo;低延迟通信引擎&rdquo;，用多核 M0 固件做报文收发、过滤、路由，把主 CPU 中断负荷降到几乎 0。&ldquo;数据包转发引擎&rdquo;，负责以太网帧的分类、修改、转发、VLAN/NAT/IPsec 等 offload，支持 2 Gbit/s 线速且主机负载 &asymp;0。两者都跑各自固件，通过内部 AXI 桥可互连，实现&ldquo;CAN&rarr;ETH&rdquo;零主核参 与 的硬件级网关功能。是它们都集成在。

qq_25461777的博客

1305

rk3588 Android HDMI IN热插拔解决

1、公司在使用 别的厂商的板卡遇到一个问题，开机我们的app自启就会闪退，后来定位发现是camera 的open出错了，这个问题的出现是因为没有插HDMI IN输入的问题导致的,所以需要对HDMI IN的热插拔进行检测，后面我把这个问题也顺利解决了。下面是我的尝试和使用的解决办法。3、话不多说，我用的是读取系统文件实现的，但是这个方法，需要有status文件的系统读权限，可以修改设备下的/vendor/etc/init/hw/init.rk3588.rc文件修改的权限，这个问题交给自己或者厂家协商解决。

qq_54089476的博客

1920

CUDA 面试 高频考点：Softmax 算子 从CPU到 GPU 的完整实现 指南

在深度学习模型训练和推理中，Softmax函数是神经网络输出层的核心组件之一，而 CUDA 面试 中Softmax 算子 的实现更是高频考点！🚀 本文将带你深入理解Softmax 算子 的工作原理，并展示如何从CPU版本逐步优化到高性能的 GPU 实现。

## 什么是Softmax 算子 ？🤔

Softmax函数将任意实数向量转换为概率分布，广泛应用于分类任务的神经网络中。其数学公式为：

```
softma

gitblog_00609的博客

1090

MOS管关断时尖峰电压的5种实用抑制方案（附实测数据对比）

本文针对MOS管关断时产生的尖峰电压问题，提供了五种经过实验室实测验证的实用抑制方案。文章从尖峰电压的成因分析入 手 ，详细阐述了优化PCB布局、设计RC缓冲电路、选用TVS二极管钳位、调节驱动电路以及使用磁珠吸收高频振荡等方法的原理、实施步骤 与 实测数据对比，为硬件工程师解决开关电源和电机驱动中的过压应 力 与 EMI问题提供了系统性的实战 指南 。

1243

AI Infra 面试 常考 手 撕 算子

本文整理 面试 AI Infra 面试 常考的 算子 实现，目标是在白板或在线编辑器里用最少的代码写出的版本。示例默认输入是行主序的float数组；省略错误检查和模板化 dtype， 面试 时先把并行映射 与 同步关系说清楚，再补充向量化、半精度和融合。

m0_73213542的博客

247

使用Intel编译器编译OpenFOAM

本文总结了使用INTEL编译器编译OpenFOAM的过程

姜蜉蝣的博客

1550

国科大学习资料--矩阵论（叶世伟）-期末考试题整理3

国科大学习资料&ndash;矩阵论-期末考试题整理3
国科大学习资料&ndash;矩阵论-期末考试题整理3
国科大学习资料&ndash;矩阵论-期末考试题整理3

无名何许人的博客

1405

CUDA _Kernel_Samples进阶：复杂 算子 组合 与 实际应用案例

CUDA _Kernel_Samples是一套面向深度学习和高性能计 算 的 CUDA 算子 手 撕 与 面试 指南 ，通过丰富的实例帮助开发者掌握 GPU 编程核心技术。本文将深入探讨如何组合复杂 算子 解决实际问题，揭示从基础实现到 性能优化 的完整路径，为你的 CUDA 开发技能带来质的飞跃。

## 核心 算子 性能优化 策略

### 矩阵转置：共享内存优化技术
矩阵转置是深度学习预处理中的基础操作，其性能瓶颈主要源于全局内存访问

gitblog_00051的博客

646

win10 跳过U8环境检测的IE WebControls.rar

win10 安装u8的时候 提示IE WebControls 不通过，这个工具可以欺骗u8的检查。 跳过U8环境检测的IE WebControls。提示导入失败后，重新检测通过。

大模型开发核心 算子 CUDA 实现全解析：从基础到优化， 面试 高频考点一网打尽！

文章详细介绍了大模型开发中的9个核心 算子 (Softmax、GEMM、Reduce、LayerNorm、RmsNorm、Sigmoid、Silu、Gemv和Scan)的 CUDA 实现方法，包含基础版本 与 优化版本(如向量化访问、共享内存、warp级优化等)。这些实现不仅有助于理解大模型底层原理，更是 面试 高频考点。作者还分享了成功辅导学员从 C++ 转行大模型的经验，并提供完整GitHub代码链接供实践学习。

2401_85379281的博客

848

RU DOS版、EFI版32bit/64bit BIOS常用工具

DOS版、EFI版的RU工具，可用来查看IO、MEM、PCI、SMBIOS等资源。亲测在EFI SHELL下可用。

Statistics and Machine Learning Toolbox

Statistics and Machine Learning Toolbox（带书签），MATLAB统计学习工具

CUDA _Kernel_Samples核心 算子 解析：Elementwise 与 Reduce优化技巧

想要掌握 CUDA 编程 面试 的核心技巧吗？本文将为你深入解析 CUDA _Kernel_Samples项目中最重要的两个 算子 &mdash;&mdash;Elementwise和Reduce，并提供实用的优化策略。无论你是 CUDA 新 手 还是希望提升性能的开发者，这篇文章都将帮助你理解 GPU 并行计 算 的核心优化方法。

## 🚀 为什么Elementwise和Reduce如此重要？

在 CUDA 编程 面试 中，Elementwise和Re

gitblog_00661的博客

817

visio安装包

Microsoft Office Visio 是一款专业的制图软件。利用经过更新的形状、协作工具和数据链接图表，创建专业级图表以简化复杂的信息。

华为AI软件岗 面试 攻略：从 C++ 、 算子 开发到系统设计

人工智能落地不仅依赖 算法 创新，更取决于软件工程的稳定性 与 性能。在AI岗位中，真正的价值往往体现在把模型转换为高吞吐、低延迟的生产级系统，而这要求开发者扎实掌握 C++ 内存管理、并发编程、 算子 开发 与 模型推理优化等底层技术。理解NPU架构 与 昇腾计 算 基础，熟悉量化、 算子 融合和分布式训练原理，是提升AI工程能 力 的关键路径。从机器学习基础到系统设计，从机试 手 写代码到项目深挖，每一个环节都在考察候选人的实际工程思维 与 问题拆解能 力 。华为AI软件岗的 面试 挑战正是这一行业趋势的缩影，其核心不是背诵 算法 公式，而是验证你是否能将A

weixin_34417200的博客

462

CUDA 算子 开发（LLM方向）常见的一些术语

硬件层：SM/Tensor Core/Shared Memory是LLM 算子 优化的核心硬件资源，需围绕&ldquo;提升Tensor Core利用率、减少全局内存访存&rdquo;发 力 ；编程层：Kernel函数、Thread/Block划分、__shared__变量是 手 写 算子 的核心语法，需对齐Warp、避免Warp分化；优化层：分块计 算 、内存复用、 算子 融合是LLM 算子 （如Attention、Softmax）的核心优化 手 段；分析层：吞吐量、显存占用、精度误差是验证 算子 效果的核心指标，需用Nsight Compute等工具分析。

qq_53773901的博客

478

如何快速上 手 CUDA 编程？ CUDA _Kernel_Samples项目实战教程

想要快速掌握 CUDA 编程的终极 指南 吗？无论你是 CUDA 编程新 手 ，还是准备 GPU 开发 面试 的 求职 者，这篇完整教程将带你从零开始，通过 CUDA _Kernel_Samples项目实战，快速掌握 CUDA 编程的核心技巧和 面试 高频 算子 。 CUDA 编程作为 GPU 并行计 算 的基石，在现代AI、科学计 算 和高性能计 算 领域发挥着至关重要的作用。

## 🚀 为什么选择 CUDA _Kernel_Samples项目？

CUDA

gitblog_00878的博客

993

Warp Shuffle技术详解： CUDA 归约 算子 的高效实现方法

想要在 CUDA 面试 中脱颖而出吗？掌握Warp Shuffle技术是你必须掌握的终极技能！🚀 作为 CUDA 编程中的高效并行计 算 利器，Warp Shuffle技术能显著提升归约 算子 的性能，是每个 CUDA 开发者都应该掌握的黄金技巧。

CUDA 归约 算子 是并行计 算 中的核心操作，广泛应用于求和、求最大值、求平均值等场景。在 CUDA _Kernel_Samples项目中，我们看到了从基础到高级的多种归约实现方

gitblog_01148的博客

972

CUDA 共享内存 与 寄存器优化：提升 算子 性能的关键技巧

CUDA 共享内存 与 寄存器优化是提升 GPU 算子 性能的核心技术，能够显著降低内存访问延迟并充分利用硬件资源。本文将通过实用技巧和可视化示例，帮助开发者掌握这两项关键优化方法，让你的 CUDA 程序性能提升2-5倍！

## 一、共享内存： GPU 中的高速缓存池 🚀

共享内存是 GPU 线程块内共享的高速存储器，其访问速度比全局内存快100倍以上。合理使用共享内存是优化内存访问模式的关键。

### 1.1

gitblog_00405的博客

318

算子 开发一面复盘： CUDA 访存、 算子 融合 与 性能优化 的实战拷问

GPU 性能优化 是深度学习推理落地中的关键一环， 算子 开发工程师的核心职责就是让每个kernel逼近硬件理论峰值。理解 GPU 内存模型、合并访存 与 bank conflict，是写出高性能 CUDA kernel的基础；而 算子 融合 与 INT8量化，则是降低launch开销和中间张量带宽税的主流工程 手 段。在实际 面试 中， 面试 官关注的往往不是你调用了多少 算子 库API，而是能否量化每一次优化带来的收益来源&mdash;&mdash;比如把延迟下降拆解为kernel启动次数减少和访存流量降低，并对照理论带宽判断瓶颈类型。矩阵转置、共享内存tiling、

weixin_29050271的博客

118

CUDA _Kernel_Samples项目结构解析：从示例到实战的学习路径

想要快速掌握 CUDA 编程并备战 面试 吗？ CUDA _Kernel_Samples项目为你提供了一条从基础到进阶的完整学习路径！这个项目汇集了 面试 高频的 CUDA 算子 题目和优化策略，帮助 CUDA 新 手 和普通用户系统性地掌握 GPU 编程核心技能。无论你是 CUDA 初学者还是希望提升优化能 力 的开发者，这个项目都能为你提供宝贵的实战经验。

## 📚 项目概述： CUDA 算子 手 撕 与 面试 指南

CUDA _Kernel_

gitblog_00808的博客

453

上一篇：

【TensorRT】Yolov5-DeepSORT 目标跟踪

下一篇：

KMP 字符串匹配详解

【面经笔记】 CUDA

xiaxzhou的博客

08-01

6861

http://blog.csdn.net/junparadox/article/details/50540602
http://www.cnblogs.com/muchen/p/6306792.html
与 CUDA 相关的几个概念：thread，block，grid，warp，sp，sm。
sp: 小核（流处理器）最基本的处理单元，streaming processor 最后具体的指令和任务都

CUDA 编程 面试 高频30题

m0_37755875的博客

03-21

2590

CUDA （Compute Unified Device Architecture）是由NVIDIA开发的一种并行计 算 平台和应用程序接口模型。它允许开发者利用NVIDIA GPU 进行通用计 算 任务，而不仅仅是图形渲染。 CUDA 提供了一个编程模型，使得开发者可以通过C、 C++ 或Fortran等语言编写程序，并在支持 CUDA 的 GPU 上运行这些程序以获得显著的性能提升。Warp是 GPU 执行的基本单位，通常包含32个线程。Warp内的线程以单指令多线程（SIMT）方式执行相同的指令流。

PyTorch 手 撕 练习：从基础到工程实战

weixin_30516429的博客

10-14

254

深度学习框架PyTorch作为当前主流的机器学习工具，其核心价值在于动态计 算 图和自动微分系统。通过张量运 算 构建计 算 图，autograd机制自动计 算 梯度，这种设计既保留了灵活性又保证了性能。理解底层实现原理对模型调试和 性能优化 至关重要，比如 手 动实现反向传播可以深入理解梯度流动，而自定义 CUDA 算子 开发则能提升计 算 效率。HappyTorch项目通过 手 撕 ResNet、Transformer等经典模型，帮助开发者掌握PyTorch工程实践中的关键技巧，如混合精度训练、内存优化等高频技术难点，特别适合需要突破PyTo

CUDA 面试 题库（含答案）一

tarzanlee666的博客

07-24

1638

涵盖： CUDA 历史概述、 GPU 和CPU的区别以及各自的优点和应用、并行计 算 模型：SIMD、MIMD、SPMD等、NVIDIA的 GPU 硬件架构，包括流多处理器（SM）、线程处理核心（ CUDA cores）、全局内存、共享内存等、线程、线程块、网格以及它们在 GPU 上的执行方式。3、请详细描述NVIDIA的 GPU 硬件架构，包括流多处理器（SM）、线程处理核心（ CUDA cores）、全局内存、共享内存等，并解释其在并行计 算 中的角色和作用。10、在NVIDIA的 GPU 架构中，流处理器（SM）的主要功能是什么？

【scp】Windows 下scp报错 bad permissions

走在搬砖的路上！

03-11

4155

Windows 下使用 SSH 连接报 Bad owner or permissions on C:\\Users\\Administrator/.ssh/config 错误问题解决

C:\Users\Administrator>scp -P 66666 D:\1111.pdf zzd@58.213.45.xx:/home/zzd/
@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@@
@ WARNING:...

基于STM32的HC-SR04超声波模块测距（新 手 级）

weixin_63758495的博客

05-25

2206

本人是嵌入式新 手 ，学习了STM32的部分内容，最近想找些项目做，然后顺便学习了一下HC-SR04超声波模块，学习过程中也参考了一些大佬的文章。本文记录了自己的学习方法和学习过程，同时也希望能和各位大佬交流讨论。

CUDA _Kernel_Samples完全 指南 ：从零基础到掌握高频 面试 算子

热门推荐

gitblog_00051的博客

07-01

1万+

CUDA _Kernel_Samples是一份全面的 CUDA 算子 手 撕 与 面试 指南 ，汇总了 面试 高频的 CUDA 算子 题目和优化策略，包含从naive实现到优化版本的完整代码，帮助开发者高效备战 CUDA 编程 面试 。本文将带你从零基础开始，逐步掌握各类高频考察的 CUDA 算子 实现 与 优化技巧。

## 🚀 为什么选择 CUDA _Kernel_Samples？

对于 CUDA 开发者和深度学习工程师来说，掌握 算子 优化是提

CCS环境下ADC采样波形的实时可视化调试技巧

weixin_33712987的博客

04-05

419

本文详细介绍了在CCS环境下实现ADC采样波形实时可视化的调试技巧。通过数组缓存优化、多通道同步显示和波形测量工具的高级用法，帮助开发者高效调试嵌入式系统中的ADC采样问题，提升信号分析的准确性和效率。

CUDA 面试 必备：GEMV 算子 实现 与 矩阵按行归约技巧

gitblog_00100的博客

07-01

1000

在 CUDA 编程 面试 中，掌握GEMV（General Matrix-Vector multiplication） 算子 的高效实现和矩阵按行归约技巧是区分初级 与 高级开发者的关键。本文将为你揭秘这两个核心技术的实现原理 与 优化策略，帮助你在 面试 中脱颖而出！🚀

## 为什么GEMV 算子 和矩阵归约如此重要？

GEMV（矩阵-向量乘法）是深度学习、科学计 算 和高性能计 算 中的基础操作。无论是神经网络的前向传播还

LLCE、PFE模块，二层交换/桥接的通用学习转发机制

qq_25461777的博客

12-25

1305

NXP 文档里给出的正式全称如下：面向 CAN/CAN-FD、LIN、FlexRay 的&ldquo;低延迟通信引擎&rdquo;，用多核 M0 固件做报文收发、过滤、路由，把主 CPU 中断负荷降到几乎 0。&ldquo;数据包转发引擎&rdquo;，负责以太网帧的分类、修改、转发、VLAN/NAT/IPsec 等 offload，支持 2 Gbit/s 线速且主机负载 &asymp;0。两者都跑各自固件，通过内部 AXI 桥可互连，实现&ldquo;CAN&rarr;ETH&rdquo;零主核参 与 的硬件级网关功能。是它们都集成在。

rk3588 Android HDMI IN热插拔解决

qq_54089476的博客

07-05

1920

1、公司在使用 别的厂商的板卡遇到一个问题，开机我们的app自启就会闪退，后来定位发现是camera 的open出错了，这个问题的出现是因为没有插HDMI IN输入的问题导致的,所以需要对HDMI IN的热插拔进行检测，后面我把这个问题也顺利解决了。下面是我的尝试和使用的解决办法。3、话不多说，我用的是读取系统文件实现的，但是这个方法，需要有status文件的系统读权限，可以修改设备下的/vendor/etc/init/hw/init.rk3588.rc文件修改的权限，这个问题交给自己或者厂家协商解决。

CUDA 面试 高频考点：Softmax 算子 从CPU到 GPU 的完整实现 指南

gitblog_00609的博客

07-01

1090

在深度学习模型训练和推理中，Softmax函数是神经网络输出层的核心组件之一，而 CUDA 面试 中Softmax 算子 的实现更是高频考点！🚀 本文将带你深入理解Softmax 算子 的工作原理，并展示如何从CPU版本逐步优化到高性能的 GPU 实现。

## 什么是Softmax 算子 ？🤔

Softmax函数将任意实数向量转换为概率分布，广泛应用于分类任务的神经网络中。其数学公式为：

```
softma

MOS管关断时尖峰电压的5种实用抑制方案（附实测数据对比）

02-25

1243

本文针对MOS管关断时产生的尖峰电压问题，提供了五种经过实验室实测验证的实用抑制方案。文章从尖峰电压的成因分析入 手 ，详细阐述了优化PCB布局、设计RC缓冲电路、选用TVS二极管钳位、调节驱动电路以及使用磁珠吸收高频振荡等方法的原理、实施步骤 与 实测数据对比，为硬件工程师解决开关电源和电机驱动中的过压应 力 与 EMI问题提供了系统性的实战 指南 。

AI Infra 面试 常考 手 撕 算子

最新发布

m0_73213542的博客

10-28

247

本文整理 面试 AI Infra 面试 常考的 算子 实现，目标是在白板或在线编辑器里用最少的代码写出的版本。示例默认输入是行主序的float数组；省略错误检查和模板化 dtype， 面试 时先把并行映射 与 同步关系说清楚，再补充向量化、半精度和融合。

使用Intel编译器编译OpenFOAM

姜蜉蝣的博客

07-14

1550

本文总结了使用INTEL编译器编译OpenFOAM的过程

国科大学习资料--矩阵论（叶世伟）-期末考试题整理3

无名何许人的博客

01-16

1405

国科大学习资料&ndash;矩阵论-期末考试题整理3
国科大学习资料&ndash;矩阵论-期末考试题整理3
国科大学习资料&ndash;矩阵论-期末考试题整理3

CUDA _Kernel_Samples进阶：复杂 算子 组合 与 实际应用案例

gitblog_00051的博客

07-01

646

CUDA _Kernel_Samples是一套面向深度学习和高性能计 算 的 CUDA 算子 手 撕 与 面试 指南 ，通过丰富的实例帮助开发者掌握 GPU 编程核心技术。本文将深入探讨如何组合复杂 算子 解决实际问题，揭示从基础实现到 性能优化 的完整路径，为你的 CUDA 开发技能带来质的飞跃。

## 核心 算子 性能优化 策略

### 矩阵转置：共享内存优化技术
矩阵转置是深度学习预处理中的基础操作，其性能瓶颈主要源于全局内存访问

win10 跳过U8环境检测的IE WebControls.rar

win10 安装u8的时候 提示IE WebControls 不通过，这个工具可以欺骗u8的检查。 跳过U8环境检测的IE WebControls。提示导入失败后，重新检测通过。

大模型开发核心 算子 CUDA 实现全解析：从基础到优化， 面试 高频考点一网打尽！

2401_85379281的博客

12-17

848

文章详细介绍了大模型开发中的9个核心 算子 (Softmax、GEMM、Reduce、LayerNorm、RmsNorm、Sigmoid、Silu、Gemv和Scan)的 CUDA 实现方法，包含基础版本 与 优化版本(如向量化访问、共享内存、warp级优化等)。这些实现不仅有助于理解大模型底层原理，更是 面试 高频考点。作者还分享了成功辅导学员从 C++ 转行大模型的经验，并提供完整GitHub代码链接供实践学习。

RU DOS版、EFI版32bit/64bit BIOS常用工具

DOS版、EFI版的RU工具，可用来查看IO、MEM、PCI、SMBIOS等资源。亲测在EFI SHELL下可用。

Statistics and Machine Learning Toolbox

Statistics and Machine Learning Toolbox（带书签），MATLAB统计学习工具

CUDA _Kernel_Samples核心 算子 解析：Elementwise 与 Reduce优化技巧

gitblog_00661的博客

07-01

817

想要掌握 CUDA 编程 面试 的核心技巧吗？本文将为你深入解析 CUDA _Kernel_Samples项目中最重要的两个 算子 &mdash;&mdash;Elementwise和Reduce，并提供实用的优化策略。无论你是 CUDA 新 手 还是希望提升性能的开发者，这篇文章都将帮助你理解 GPU 并行计 算 的核心优化方法。

## 🚀 为什么Elementwise和Reduce如此重要？

在 CUDA 编程 面试 中，Elementwise和Re

visio安装包

Microsoft Office Visio 是一款专业的制图软件。利用经过更新的形状、协作工具和数据链接图表，创建专业级图表以简化复杂的信息。

华为AI软件岗 面试 攻略：从 C++ 、 算子 开发到系统设计

weixin_34417200的博客

08-31

462

人工智能落地不仅依赖 算法 创新，更取决于软件工程的稳定性 与 性能。在AI岗位中，真正的价值往往体现在把模型转换为高吞吐、低延迟的生产级系统，而这要求开发者扎实掌握 C++ 内存管理、并发编程、 算子 开发 与 模型推理优化等底层技术。理解NPU架构 与 昇腾计 算 基础，熟悉量化、 算子 融合和分布式训练原理，是提升AI工程能 力 的关键路径。从机器学习基础到系统设计，从机试 手 写代码到项目深挖，每一个环节都在考察候选人的实际工程思维 与 问题拆解能 力 。华为AI软件岗的 面试 挑战正是这一行业趋势的缩影，其核心不是背诵 算法 公式，而是验证你是否能将A

CUDA 算子 开发（LLM方向）常见的一些术语

qq_53773901的博客

03-19

478

硬件层：SM/Tensor Core/Shared Memory是LLM 算子 优化的核心硬件资源，需围绕&ldquo;提升Tensor Core利用率、减少全局内存访存&rdquo;发 力 ；编程层：Kernel函数、Thread/Block划分、__shared__变量是 手 写 算子 的核心语法，需对齐Warp、避免Warp分化；优化层：分块计 算 、内存复用、 算子 融合是LLM 算子 （如Attention、Softmax）的核心优化 手 段；分析层：吞吐量、显存占用、精度误差是验证 算子 效果的核心指标，需用Nsight Compute等工具分析。

如何快速上 手 CUDA 编程？ CUDA _Kernel_Samples项目实战教程

gitblog_00878的博客

07-01

993

想要快速掌握 CUDA 编程的终极 指南 吗？无论你是 CUDA 编程新 手 ，还是准备 GPU 开发 面试 的 求职 者，这篇完整教程将带你从零开始，通过 CUDA _Kernel_Samples项目实战，快速掌握 CUDA 编程的核心技巧和 面试 高频 算子 。 CUDA 编程作为 GPU 并行计 算 的基石，在现代AI、科学计 算 和高性能计 算 领域发挥着至关重要的作用。

## 🚀 为什么选择 CUDA _Kernel_Samples项目？

CUDA

Warp Shuffle技术详解： CUDA 归约 算子 的高效实现方法

gitblog_01148的博客

07-01

972

想要在 CUDA 面试 中脱颖而出吗？掌握Warp Shuffle技术是你必须掌握的终极技能！🚀 作为 CUDA 编程中的高效并行计 算 利器，Warp Shuffle技术能显著提升归约 算子 的性能，是每个 CUDA 开发者都应该掌握的黄金技巧。

CUDA 归约 算子 是并行计 算 中的核心操作，广泛应用于求和、求最大值、求平均值等场景。在 CUDA _Kernel_Samples项目中，我们看到了从基础到高级的多种归约实现方

CUDA 共享内存 与 寄存器优化：提升 算子 性能的关键技巧

gitblog_00405的博客

07-01

318

CUDA 共享内存 与 寄存器优化是提升 GPU 算子 性能的核心技术，能够显著降低内存访问延迟并充分利用硬件资源。本文将通过实用技巧和可视化示例，帮助开发者掌握这两项关键优化方法，让你的 CUDA 程序性能提升2-5倍！

## 一、共享内存： GPU 中的高速缓存池 🚀

共享内存是 GPU 线程块内共享的高速存储器，其访问速度比全局内存快100倍以上。合理使用共享内存是优化内存访问模式的关键。

### 1.1

算子 开发一面复盘： CUDA 访存、 算子 融合 与 性能优化 的实战拷问

weixin_29050271的博客

10-12

118

GPU 性能优化 是深度学习推理落地中的关键一环， 算子 开发工程师的核心职责就是让每个kernel逼近硬件理论峰值。理解 GPU 内存模型、合并访存 与 bank conflict，是写出高性能 CUDA kernel的基础；而 算子 融合 与 INT8量化，则是降低launch开销和中间张量带宽税的主流工程 手 段。在实际 面试 中， 面试 官关注的往往不是你调用了多少 算子 库API，而是能否量化每一次优化带来的收益来源&mdash;&mdash;比如把延迟下降拆解为kernel启动次数减少和访存流量降低，并对照理论带宽判断瓶颈类型。矩阵转置、共享内存tiling、

CUDA _Kernel_Samples项目结构解析：从示例到实战的学习路径

gitblog_00808的博客

07-01

453

想要快速掌握 CUDA 编程并备战 面试 吗？ CUDA _Kernel_Samples项目为你提供了一条从基础到进阶的完整学习路径！这个项目汇集了 面试 高频的 CUDA 算子 题目和优化策略，帮助 CUDA 新 手 和普通用户系统性地掌握 GPU 编程核心技能。无论你是 CUDA 初学者还是希望提升优化能 力 的开发者，这个项目都能为你提供宝贵的实战经验。

## 📚 项目概述： CUDA 算子 手 撕 与 面试 指南

CUDA _Kernel_

目录

展开全部

收起

33

0

52

复制链接

分享到 QQ

分享到新浪微博

扫一扫

举报

+ 关注

迈克柯里喵

码龄8年
153粉丝
27原创

+ 关注
专栏目录
订阅专栏

✦ 双11 狂欢大促 ✦

盲盒赢 定制羽绒服

开启惊喜盲盒，抽取你的专属好礼

✦

立即开盒 ›
中奖列表 ›

— 惊喜奖品抢先看 —

超级年卡50元券
周边70元券
手机支架
一拖三数据线
雨伞
运动毛巾
电动牙刷
VIP月卡
机械键盘
定制羽绒服
超级年卡50元券
周边70元券
手机支架
一拖三数据线
雨伞
运动毛巾
电动牙刷
VIP月卡
机械键盘
定制羽绒服

评论

成就一亿技术人!

拼手气红包 6.0元

还能输入 1000 个字符

插入表情

代码片

HTML/XML

objective-c

Ruby

PHP

C

C++

JavaScript

Python

Java

CSS

SQL

其它

条评论被折叠 查看

被折叠的 条评论
为什么被折叠?

到【灌水乐园】发言

查看更多评论

添加红包

祝福语

请填写红包祝福语或标题

红包数量

个

红包个数最小为10个

红包总金额

元

红包金额最低5元

余额支付

当前余额 3.43 元
前往充值 >

需支付： 10.00 元

取消
确定

成就一亿技术人!

领取后你会自动成为博主和红包主的粉丝
规则

hope_wisdom
发出的红包

实付 元

使用余额支付

点击重新获取

扫码支付

钱包余额
0

抵扣说明：

1.余额是钱包充值的虚拟货币，按照1:1的比例进行支付金额的抵扣。
2.余额无法直接购买下载，可以购买VIP、付费专栏及课程。

余额充值
