# CUDA C++ 最佳实践指南（官方）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　50,233 字符　｜　310 个片段　｜　更新于 2026-11-13
来源: https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html

# https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html

来源: https://docs.nvidia.com/cuda/cuda-c-best-practices-guide/index.html
抓取方式: read_page

1. Overview

2. Preface
2.1. Who Should Read This Guide?

2.2. Assess, Parallelize, Optimize, Deploy
2.2.1. Assess

2.2.2. Parallelize

2.2.3. Optimize

2.2.4. Deploy

2.3. Recommendations and Best Practices

2.4. Assessing Your Application

3. Heterogeneous Computing
3.1. Differences between Host and Device

3.2. What Runs on a CUDA-Enabled Device?

4. Application Profiling

4.1. Profile
4.1.1. Creating the Profile

4.1.2. Identifying Hotspots

4.1.3. Understanding Scaling
4.1.3.1. Strong Scaling and Amdahl’s Law

4.1.3.2. Weak Scaling and Gustafson’s Law

4.1.3.3. Applying Strong and Weak Scaling

5. Parallelizing Your Application

6. Getting Started
6.1. Parallel Libraries

6.2. Parallelizing Compilers

6.3. Coding to Expose Parallelism

7. Getting the Right Answer

7.1. Verification
7.1.1. Reference Comparison

7.1.2. Unit Testing

7.2. Debugging

7.3. Numerical Accuracy and Precision
7.3.1. Single vs. Double Precision

7.3.2. Floating Point Math Is Not Associative

7.3.3. IEEE 754 Compliance

7.3.4. x86 80-bit Computations

8. Optimizing CUDA Applications

9. Performance Metrics

9.1. Timing
9.1.1. Using CPU Timers

9.1.2. Using CUDA GPU Timers

9.2. Bandwidth
9.2.1. Theoretical Bandwidth Calculation

9.2.2. Effective Bandwidth Calculation

9.2.3. Throughput Reported by Visual Profiler

10. Memory Optimizations

10.1. Data Transfer Between Host and Device
10.1.1. Pinned Memory

10.1.2. Asynchronous and Overlapping Transfers with Computation

10.1.3. Zero Copy

10.1.4. Unified Virtual Addressing

10.2. Device Memory Spaces

10.2.1. Coalesced Access to Global Memory
10.2.1.1. A Simple Access Pattern

10.2.1.2. A Sequential but Misaligned Access Pattern

10.2.1.3. Effects of Misaligned Accesses

10.2.1.4. Strided Accesses

10.2.2. L2 Cache
10.2.2.1. L2 Cache Access Window

10.2.2.2. Tuning the Access Window Hit-Ratio

10.2.3. Shared Memory
10.2.3.1. Shared Memory and Memory Banks

10.2.3.2. Shared Memory in Matrix Multiplication (C=AB)

10.2.3.3. Shared Memory in Matrix Multiplication (C=AAT)

10.2.3.4. Asynchronous Copy from Global Memory to Shared Memory

10.2.4. Local Memory

10.2.5. Texture Memory
10.2.5.1. Additional Texture Capabilities

10.2.6. Constant Memory

10.2.7. Registers
10.2.7.1. Register Pressure

10.3. Allocation

10.4. NUMA Best Practices

11. Execution Configuration Optimizations

11.1. Occupancy
11.1.1. Calculating Occupancy

11.2. Hiding Register Dependencies

11.3. Thread and Block Heuristics

11.4. Effects of Shared Memory

11.5. Concurrent Kernel Execution

11.6. Multiple contexts

12. Instruction Optimization

12.1. Arithmetic Instructions
12.1.1. Throughput of Native Arithmetic Instructions

12.1.2. Control Flow Instructions

12.1.3. Synchronization Instruction

12.1.4. Division Modulo Operations

12.1.5. Loop Counters Signed vs. Unsigned

12.1.6. Reciprocal Square Root

12.1.7. Other Arithmetic Instructions

12.1.8. Exponentiation With Small Fractional Arguments

12.1.9. Math Libraries

12.1.10. Precision-related Compiler Flags

12.2. Memory Instructions

13. Control Flow
13.1. Branching and Divergence

13.2. Branch Predication

14. Deploying CUDA Applications

15. Understanding the Programming Environment
15.1. CUDA Compute Capability

15.2. Additional Hardware Data

15.3. Which Compute Capability Target

15.4. CUDA Runtime

16. CUDA Compatibility Developer’s Guide
16.1. CUDA Toolkit Versioning

16.2. Source Compatibility

16.3. Binary Compatibility
16.3.1. CUDA Binary (cubin) Compatibility

16.4. CUDA Compatibility Across Minor Releases

16.4.1. Existing CUDA Applications within Minor Versions of CUDA
16.4.1.1. Handling New CUDA Features and Driver APIs

16.4.1.2. Using PTX

16.4.1.3. Dynamic Code Generation

16.4.1.4. Recommendations for building a minor-version compatible library

16.4.1.5. Recommendations for taking advantage of minor version compatibility in your application

17. Preparing for Deployment
17.1. Testing for CUDA Availability

17.2. Error Handling

17.3. Building for Maximum Compatibility

17.4. Distributing the CUDA Runtime and Libraries

17.4.1. CUDA Toolkit Library Redistribution
17.4.1.1. Which Files to Redistribute

17.4.1.2. Where to Install Redistributed CUDA Libraries

18. Deployment Infrastructure Tools

18.1. Nvidia-SMI
18.1.1. Queryable state

18.1.2. Modifiable state

18.2. NVML

18.3. Cluster Management Tools

18.4. Compiler JIT Cache Management Tools

18.5. CUDA_VISIBLE_DEVICES

19. Recommendations and Best Practices
19.1. Overall Performance Optimization Strategies

20. nvcc Compiler Switches
20.1. nvcc

21. Notices
21.1. Notice

21.2. OpenCL

21.3. Trademarks

CUDA C++ Best Practices Guide

»

CUDA Best Practices Guide

v13.5 |

PDF

|

Archive

CUDA C++ Best Practices Guide

1. Overview 

The CUDA C++ Best Practices Guide provides practical guidelines for writing high-performance CUDA applications. It covers optimization strategies across memory usage, parallel execution, and instruction-level efficiency. The guide helps developers identify performance bottlenecks, leverage GPU architecture effectively, and apply profiling tools to fine-tune applications. It’s an essential resource for maximizing throughput and achieving scalable, efficient CUDA programs.

2. Preface 

This Best Practices Guide is a manual to help developers obtain the best performance from NVIDIA ® CUDA ® GPUs. It presents established parallelization and optimization techniques and explains coding metaphors and idioms that can greatly simplify programming for CUDA-capable GPU architectures.

While the contents can be used as a reference manual, you should be aware that some topics are revisited in different contexts as various programming and configuration topics are explored. As a result, it is recommended that first-time readers proceed through the guide sequentially. This approach will greatly improve your understanding of effective programming practices and enable you to better use the guide for reference later.

2.1. Who Should Read This Guide? 

The discussions in this guide all use the C++ programming language, so you should be comfortable reading C++ code.

This guide refers to and relies on several other documents that you should have at your disposal for reference, all of which are available at no cost from the CUDA website https://docs.nvidia.com/cuda/. The following documents are especially important resources:

CUDA Installation Guide

CUDA Programming Guide

CUDA Toolkit Reference Manual

In particular, the optimization section of this guide assumes that you have already successfully downloaded and installed the CUDA Toolkit (if not, please refer to the relevant CUDA Installation Guide for your platform) and that you have a basic familiarity with the CUDA C++ programming language and environment (if not, please refer to the CUDA Programming Guide).

2.2. Assess, Parallelize, Optimize, Deploy 

This guide introduces the Assess, Parallelize, Optimize, Deploy(APOD) design cycle for applications with the goal of helping application developers to rapidly identify the portions of their code that would most readily benefit from GPU acceleration, rapidly realize that benefit, and begin leveraging the resulting speedups in production as early as possible.

APOD is a cyclical process: initial speedups can be achieved, tested, and deployed with only minimal initial investment of time, at which point the cycle can begin again by identifying further optimization opportunities, seeing additional speedups, and then deploying the even faster versions of the application into production.

2.2.1. Assess 

For an existing project, the first step is to assess the application to locate the parts of the code that are responsible for the bulk of the execution time. Armed with this knowledge, the developer can evaluate these bottlenecks for parallelization and start to investigate GPU acceleration.

By understanding the end-user’s requirements and constraints and by applying Amdahl’s and Gustafson’s laws, the developer can determine the upper bound of performance improvement from acceleration of the identified portions of the application.

2.2.2. Parallelize 

Having identified the hotspots and having done the basic exercises to set goals and expectations, the developer needs to parallelize the code. Depending on the original code, this can be as simple as calling into an existing GPU-optimized library such as cuBLAS, cuFFT, or Thrust, or it could be as simple as adding a few preprocessor directives as hints to a parallelizing compiler.

On the other hand, some applications’ designs will require some amount of refactoring to expose their inherent parallelism. As even CPU architectures will require exposing parallelism in order to improve or simply maintain the performance of sequential applications, the CUDA family of parallel programming languages (CUDA C++, CUDA Fortran, etc.) aims to make the expression of this parallelism as simple as possible, while simultaneously enabling operation on CUDA-capable GPUs designed for maximum parallel throughput.

2.2.3. Optimize 

After each round of application parallelization is complete, the developer can move to optimizing the implementation to improve performance. Since there are many possible optimizations that can be considered, having a good understanding of the needs of the application can help to make the process as smooth as possible. However, as with APOD as a whole, program optimization is an iterative process (identify an opportunity for optimization, apply and test the optimization, verify the speedup achieved, and repeat), meaning that it is not necessary for a programmer to spend large amounts of time memorizing the bulk of all possible optimization strategies prior to seeing good speedups. Instead, strategies can be applied incrementally as they are learned.

Optimizations can be applied at various levels, from overlapping data transfers with computation all the way down to fine-tuning floating-point operation sequences. The available profiling tools are invaluable for guiding this process, as they can help suggest a next-best course of action for the developer’s optimization efforts and provide references into the relevant portions of the optimization section of this guide.

2.2.4. Deploy 

Having completed the GPU acceleration of one or more components of the application it is possible to compare the outcome with the original expectation. Recall that the initial assess step allowed the developer to determine an upper bound for the potential speedup attainable by accelerating given hotspots.

Before tackling other hotspots to improve the total speedup, the developer should consider taking the partially parallelized implementation and carry it through to production. This is important for a number of reasons; for example, it allows the user to profit from their investment as early as possible (the speedup may be partial but is still valuable), and it minimizes risk for the developer and the user by providing an evolutionary rather than revolutionary set of changes to the application.

2.3. Recommendations and Best Practices 

Throughout this guide, specific recommendations are made regarding the design and implementation of CUDA C++ code. These recommendations are categorized by priority, which is a blend of the effect of the recommendation and its scope. Actions that present substantial improvements for most CUDA applications have the highest priority, while small optimizations that affect only very specific situations are given a lower priority.

Before implementing lower priority recommendations, it is good practice to make sure all higher priority recommendations that are relevant have already been applied. This approach will tend to provide the best results for the time invested and will avoid the trap of premature optimization.

The criteria of benefit and scope for establishing priority will vary depending on the nature of the program. In this guide, they represent a typical case. Your code might reflect different priority factors. Regardless of this possibility, it is good practice to verify that no higher-priority recommendations have been overlooked before undertaking lower-priority items.

Note

Code samples throughout the guide omit error checking for conciseness. Production code should, however, systematically check the error code returned by each API call and check for failures in kernel launches by calling cudaGetLastError().

2.4. Assessing Your Application 

From supercomputers to mobile phones, modern processors increasingly rely on parallelism to provide performance. The core computational unit, which includes control, arithmetic, registers and typically some cache, is replicated some number of times and connected to memory via a network. As a result, all modern processors require parallel code in order to achieve good utilization of their computational power.

While processors are evolving to expose more fine-grained parallelism to the programmer, many existing applications have evolved either as serial codes or as coarse-grained parallel codes (for example, where the data is decomposed into regions processed in parallel, with sub-regions shared using MPI). In order to profit from any modern processor architecture, GPUs included, the first steps are to assess the application to identify the hotspots, determine whether they can be parallelized, and understand the relevant workloads both now and in the future.

3. Heterogeneous Computing 

CUDA programming involves running code on two different platforms concurrently: a host system with one or more CPUs and one or more CUDA-enabled NVIDIA GPU devices.

While NVIDIA GPUs are frequently associated with graphics, they are also powerful arithmetic engines capable of running thousands of lightweight threads in parallel. This capability makes them well suited to computations that can leverage parallel execution.

However, the device is based on a distinctly different design from the host system, and it’s important to understand those differences and how they determine the performance of CUDA applications in order to use CUDA effectively.

3.1. Differences between Host and Device 

The primary differences are in threading model and in separate physical memories:

Threading resources

Execution pipelines on host systems can support a limited number of concurrent threads. For example, servers that have two 32 core processors can run only 64 threads concurrently (or small multiple of that if the CPUs support simultaneous multithreading). By comparison, the smallest executable unit of parallelism on a CUDA device comprises 32 threads (termed a warp of threads). Modern NVIDIA GPUs can support up to 2048 active threads concurrently per multiprocessor (see Features and Specifications of the CUDA Programming Guide) On GPUs with 80 multiprocessors, this leads to more than 160,000 concurrently active threads.

Threads

Threads on a CPU are generally heavyweight entities. The operating system must swap threads on and off CPU execution channels to provide multithreading capability. Context switches (when two threads are swapped) are therefore slow and expensive. By comparison, threads on GPUs are extremely lightweight. In a typical system, thousands of threads are queued up for work (in warps of 32 threads each). If the GPU must wait on one warp of threads, it simply begins executing work on another. Because separate registers are allocated to all active threads, no swapping of registers or other state need occur when switching among GPU threads. Resources stay allocated to each thread until it completes its execution. In short, CPU cores are designed to minimize latency for a small number of threads at a time each, whereas GPUs are designed to handle a large number of concurrent, lightweight threads in order to maximize throughput.

RAM

The host system and the device each have their own distinct attached physical memories 1. As the host and device memories are separated, items in the host memory must occasionally be communicated between device memory and host memory as described in What Runs on a CUDA-Enabled Device?.

These are the primary hardware differences between CPU hosts and GPU devices with respect to parallel programming. Other differences are discussed as they arise elsewhere in this document. Applications composed with these differences in mind can treat the host and device together as a cohesive heterogeneous system wherein each processing unit is leveraged to do the kind of work it does best: sequential work on the host and parallel work on the device.

3.2. What Runs on a CUDA-Enabled Device? 

The following issues should be considered when determining what parts of an application to run on the device:

The device is ideally suited for computations that can be run on numerous data elements simultaneously in parallel. This typically involves arithmetic on large data sets (such as matrices) where the same operation can be performed across thousands, if not millions, of elements at the same time. This is a requirement for good performance on CUDA: the software must use a large number (generally thousands or tens of thousands) of concurrent threads. The support for running numerous threads in parallel derives from CUDA’s use of a lightweight threading model described above.

To use CUDA, data values must be transferred from the host to the device. These transfers are costly in terms of performance and should be minimized. (See Data Transfer Between Host and Device.) This cost has several ramifications:

The complexity of operations should justify the cost of moving data to and from the device. Code that transfers data for brief use by a small number of threads will see little or no performance benefit. The ideal scenario is one in which many threads perform a substantial amount of work.

For example, transferring two matrices to the device to perform a matrix addition and then transferring the results back to the host will not realize much performance benefit. The issue here is the number of operations performed per data element transferred. For the preceding procedure, assuming matrices of size NxN, there are N 2 operations (additions) and 3N 2 elements transferred, so the ratio of operations to elements transferred is 1:3 or O(1). Performance benefits can be more readily achieved when this ratio is higher. For example, a matrix multiplication of the same matrices requires N 3 operations (multiply-add), so the ratio of operations to elements transferred is O(N), in which case the larger the matrix the greater the performance benefit. The types of operations are an additional factor, as additions have different complexity profiles than, for example, trigonometric functions. It is important to include the overhead of transferring data to and from the device in determining whether operations should be performed on the host or on the device.

Data should be kept on the device as long as possible. Because transfers should be minimized, programs that run multiple kernels on the same data should favor leaving the data on the device between kernel calls, rather than transferring intermediate results to the host and then sending them back to the device for subsequent calculations. So, in the previous example, had the two matrices to be added already been on the device as a result of some previous calculation, or if the results of the addition would be used in some subsequent calculation, the matrix addition should be performed locally on the device. This approach should be used even if one of the steps in a sequence of calculations could be performed faster on the host. Even a relatively slow kernel may be advantageous if it avoids one or more transfers between host and device memory. Data Transfer Between Host and Device provides further details, including the measurements of bandwidth between the host and the device versus within the device proper.

For best performance, there should be some coherence in memory access by adjacent threads running on the device. Certain memory access patterns enable the hardware to coalesce groups of reads or writes of multiple data items into one operation. Data that cannot be laid out so as to enable coalescing, or that doesn’t have enough locality to use the L1 or texture caches effectively, will tend to see lesser speedups when used in computations on GPUs. A noteworthy exception to this are completely random memory access patterns. In general, they should be avoided, because compared to peak capabilities any architecture processes these memory access patterns at a low efficiency. However, compared to cache based architectures, like CPUs, latency hiding architectures, like GPUs, tend to cope better with completely random memory access patterns.

1

On Systems on a Chip with integrated GPUs, such as NVIDIA® Tegra®, host and device memory are physically the same, but there is still a logical distinction between host and device memory. See the Application Note on CUDA for Tegra for details.

4. Application Profiling 

4.1. Profile 

Many codes accomplish a significant portion of the work with a relatively small amount of code. Using a profiler, the developer can identify such hotspots and start to compile a list of candidates for parallelization.

4.1.1. Creating the Profile 

There are many possible approaches to profiling the code, but in all cases the objective is the same: to identify the function or functions in which the application is spending most of its execution time.

Note

High Priority: To maximize developer productivity, profile the application to determine hotspots and bottlenecks.

The most important consideration with any profiling activity is to ensure that the workload is realistic - i.e., that information gained from the test and decisions based upon that information are relevant to real data. Using unrealistic workloads can lead to sub-optimal results and wasted effort both by causing developers to optimize for unrealistic problem sizes and by causing developers to concentrate on the wrong functions.

There are a number of tools that can be used to generate the profile. The following example is based on gprof, which is an open-source profiler for Linux platforms from the GNU Binutils collection.

$ gcc -O2 -g -pg myprog.c
$ gprof./a.out > profile.txt
Each sample counts as 0.01 seconds.
% cumulative self self total
time seconds seconds calls ms/call ms/call name
33.34 0.02 0.02 7208 0.00 0.00 genTimeStep
16.67 0.03 0.01 240 0.04 0.12 calcStats
16.67 0.04 0.01 8 1.25 1.25 calcSummaryData
16.67 0.05 0.01 7 1.43 1.43 write
16.67 0.06 0.01 mcount
0.00 0.06 0.00 236 0.00 0.00 tzset
0.00 0.06 0.00 192 0.00 0.00 tolower
0.00 0.06 0.00 47 0.00 0.00 strlen
0.00 0.06 0.00 45 0.00 0.00 strchr
0.00 0.06 0.00 1 0.00 50.00 main
0.00 0.06 0.00 1 0.00 0.00 memcpy
0.00 0.06 0.00 1 0.00 10.11 print
0.00 0.06 0.00 1 0.00 0.00 profil
0.00 0.06 0.00 1 0.00 50.00 report

4.1.2. Identifying Hotspots 

In the example above, we can clearly see that the function genTimeStep() takes one-third of the total running time of the application. This should be our first candidate function for parallelization. Understanding Scaling discusses the potential benefit we might expect from such parallelization.

It is worth noting that several of the other functions in the above example also take up a significant portion of the overall running time, such as calcStats() and calcSummaryData(). Parallelizing these functions as well should increase our speedup potential. However, since APOD is a cyclical process, we might opt to parallelize these functions in a subsequent APOD pass, thereby limiting the scope of our work in any given pass to a smaller set of incremental changes.

4.1.3. Understanding Scaling 

The amount of performance benefit an application will realize by running on CUDA depends entirely on the extent to which it can be parallelized. Code that cannot be sufficiently parallelized should run on the host, unless doing so would result in excessive transfers between the host and the device.

Note

High Priority: To get the maximum benefit from CUDA, focus first on finding ways to parallelize sequential code.

By understanding how applications can scale it is possible to set expectations and plan an incremental parallelization strategy. Strong Scaling and Amdahl’s Law describes strong scaling, which allows us to set an upper bound for the speedup with a fixed problem size. Weak Scaling and Gustafson’s Law describes weak scaling, where the speedup is attained by growing the problem size. In many applications, a combination of strong and weak scaling is desirable.

4.1.3.1. Strong Scaling and Amdahl’s Law 

Strong scaling is a measure of how, for a fixed overall problem size, the time to solution decreases as more processors are added to a system. An application that exhibits linear strong scaling has a speedup equal to the number of processors used.

Strong scaling is usually equated with Amdahl’s Law, which specifies the maximum speedup that can be expected by parallelizing portions of a serial program. Essentially, it states that the maximum speedup S of a program is:

\(S = \frac{1}{(1 - P) + \frac{P}{N}}\)

Here P is the fraction of the total serial execution time taken by the portion of code that can be parallelized and N is the number of processors over which the parallel portion of the code runs.

The larger N is(that is, the greater the number of processors), the smaller the P/N fraction. It can be simpler to view N as a very large number, which essentially transforms the equation into \(S = 1/(1 - P)\). Now, if 3/4 of the running time of a sequential program is parallelized, the maximum speedup over serial code is 1 / (1 - 3/4) = 4.

In reality, most applications do not exhibit perfectly linear strong scaling, even if they do exhibit some degree of strong scaling. For most purposes, the key point is that the larger the parallelizable portion P is, the greater the potential speedup. Conversely, if P is a small number (meaning that the application is not substantially parallelizable), increasing the number of processors N does little to improve performance. Therefore, to get the largest speedup for a fixed problem size, it is worthwhile to spend effort on increasing P, maximizing the amount of code that can be parallelized.

4.1.3.2. Weak Scaling and Gustafson’s Law 

Weak scaling is a measure of how the time to solution changes as more processors are added to a system with a fixed problem size per processor; i.e., where the overall problem size increases as the number of processors is increased.

Weak scaling is often equated with Gustafson’s Law, which states that in practice, the problem size scales with the number of processors. Because of this, the maximum speedup S of a program is:

\(S = N + (1 - P)(1 - N)\)

Here P is the fraction of the total serial execution time taken by the portion of code that can be parallelized and N is the number of processors over which the parallel portion of the code runs.

Another way of looking at Gustafson’s Law is that it is not the problem size that remains constant as we scale up the system but rather the execution time. Note that Gustafson’s Law assumes that the ratio of serial to parallel execution remains constant, reflecting additional cost in setting up and handling the larger problem.

4.1.3.3. Applying Strong and Weak Scaling 

Understanding which type of scaling is most applicable to an application is an important part of estimating speedup. For some applications the problem size will remain constant and hence only strong scaling is applicable. An example would be modeling how two molecules interact with each other, where the molecule sizes are fixed.

For other applications, the problem size will grow to fill the available processors. Examples include modeling fluids or structures as meshes or grids and some Monte Carlo simulations, where increasing the problem size provides increased accuracy.

Having understood the application profile, the developer should understand how the problem size would change if the computational performance changes and then apply either Amdahl’s or Gustafson’s Law to determine an upper bound for the speedup.

5. Parallelizing Your Application 

Having identified the hotspots and having done the basic exercises to set goals and expectations, the developer needs to parallelize the code. Depending on the original code, this can be as simple as calling into an existing GPU-optimized library such as cuBLAS, cuFFT, or Thrust, or it could be as simple as adding a few preprocessor directives as hints to a parallelizing compiler.

On the other hand, some applications’ designs will require some amount of refactoring to expose their inherent parallelism. As even CPU architectures require exposing this parallelism in order to improve or simply maintain the performance of sequential applications, the CUDA family of parallel programming languages (CUDA C++, CUDA Fortran, etc.) aims to make the expression of this parallelism as simple as possible, while simultaneously enabling operation on CUDA-capable GPUs designed for maximum parallel throughput.

6. Getting Started 

There are several key strategies for parallelizing sequential code. While the details of how to apply these strategies to a particular application is a complex and problem-specific topic, the general themes listed here apply regardless of whether we are parallelizing code to run on multicore CPUs or for use on CUDA GPUs.

6.1. Parallel Libraries 

The most straightforward approach to parallelizing an application is to leverage existing libraries that take advantage of parallel architectures on our behalf. The CUDA Toolkit includes a number of such libraries that have been fine-tuned for NVIDIA CUDA GPUs, such as cuBLAS, cuFFT, and so on.

The key here is that libraries are most useful when they match well with the needs of the application. Applications already using other BLAS libraries can often quite easily switch to cuBLAS, for example, whereas applications that do little to no linear algebra will have little use for cuBLAS. The same goes for other CUDA Toolkit libraries: cuFFT has an interface similar to that of FFTW, etc.

Also of note is the Thrust library, which is a parallel C++ template library similar to the C++ Standard Template Library. Thrust provides a rich collection of data parallel primitives such as scan, sort, and reduce, which can be composed together to implement complex algorithms with concise, readable source code. By describing your computation in terms of these high-level abstractions you provide Thrust with the freedom to select the most efficient implementation automatically. As a result, Thrust can be utilized in rapid prototyping of CUDA applications, where programmer productivity matters most, as well as in production, where robustness and absolute performance are crucial.

6.2. Parallelizing Compilers 

Another common approach to parallelization of sequential codes is to make use of parallelizing compilers. Often this means the use of directives-based approaches, where the programmer uses a pragma or other similar notation to provide hints to the compiler about where parallelism can be found without needing to modify or adapt the underlying code itself. By exposing parallelism to the compiler, directives allow the compiler to do the detailed work of mapping the computation onto the parallel architecture.

The OpenACC standard provides a set of compiler directives to specify loops and regions of code in standard C, C++ and Fortran that should be offloaded from a host CPU to an attached accelerator such as a CUDA GPU. The details of managing the accelerator device are handled implicitly by an OpenACC-enabled compiler and runtime.

See http://www.openacc.org/ for details.

6.3. Coding to Expose Parallelism 

For applications that need additional functionality or performance beyond what existing parallel libraries or parallelizing compilers can provide, parallel programming languages such as CUDA C++ that integrate seamlessly with existing sequential code are essential.

Once we have located a hotspot in our application’s profile assessment and determined that custom code is the best approach, we can use CUDA C++ to expose the parallelism in that portion of our code as a CUDA kernel. We can then launch this kernel onto the GPU and retrieve the results without requiring major rewrites to the rest of our application.

This approach is most straightforward when the majority of the total running time of our application is spent in a few relatively isolated portions of the code. More difficult to parallelize are applications with a very flat profile - i.e., applications where the time spent is spread out relatively evenly across a wide portion of the code base. For the latter variety of application, some degree of code refactoring to expose the inherent parallelism in the application might be necessary, but keep in mind that this refactoring work will tend to benefit all future architectures, CPU and GPU alike, so it is well worth the effort should it become necessary.

7. Getting the Right Answer 

Obtaining the right answer is clearly the principal goal of all computation. On parallel systems, it is possible to run into difficulties not typically found in traditional serial-oriented programming. These include threading issues, unexpected values due to the way floating-point values are computed, and challenges arising from differences in the way CPU and GPU processors operate. This chapter examines issues that can affect the correctness of returned data and points to appropriate solutions.

7.1. Verification 

7.1.1. Reference Comparison 

A key aspect of correctness verification for modifications to any existing program is to establish some mechanism whereby previous known-good reference outputs from representative inputs can be compared to new results. After each change is made, ensure that the results match using whatever criteria apply to the particular algorithm. Some will expect bitwise identical results, which is not always possible, especially where floating-point arithmetic is concerned; see Numerical Accuracy and Precision regarding numerical accuracy. For other algorithms, implementations may be considered correct if they match the reference within some small epsilon.

Note that the process used for validating numerical results can easily be extended to validate performance results as well. We want to ensure that each change we make is correct and that it improves performance (and by how much). Checking these things frequently as an integral part of our cyclical APOD process will help ensure that we achieve the desired results as rapidly as possible.

7.1.2. Unit Testing 

A useful counterpart to the reference comparisons described above is to structure the code itself in such a way that is readily verifiable at the unit level. For example, we can write our CUDA kernels as a collection of many short __device__ functions rather than one large monolithic __global__ function; each device function can be tested independently before hooking them all together.

For example, many kernels have complex addressing logic for accessing memory in addition to their actual computation. If we validate our addressing logic separately prior to introducing the bulk of the computation, then this will simplify any later debugging efforts. (Note that the CUDA compiler considers any device code that does not contribute to a write to global memory as dead code subject to elimination, so we must at least write something out to global memory as a result of our addressing logic in order to successfully apply this strategy.)

Going a step further, if most functions are defined as __host__ __device__ rather than just __device__ functions, then these functions can be tested on both the CPU and the GPU, thereby increasing our confidence that the function is correct and that there will not be any unexpected differences in the results. If there are differences, then those differences will be seen early and can be understood in the context of a simple function.

As a useful side effect, this strategy will allow us a means to reduce code duplication should we wish to include both CPU and GPU execution paths in our application: if the bulk of the work of our CUDA kernels is done in __host__ __device__ functions, we can easily call those functions from both the host code and the device code without duplication.

7.2. Debugging 

CUDA-GDB is a port of the GNU Debugger that runs on Linux and Mac; see: https://developer.nvidia.com/cuda-gdb.

The NVIDIA Nsight Visual Studio Edition is available as a free plugin for Microsoft Visual Studio; see: https://developer.nvidia.com/nsight-visual-studio-edition.

Several third-party debuggers support CUDA debugging as well; see: https://developer.nvidia.com/debugging-solutions for more details.

7.3. Numerical Accuracy and Precision 

Incorrect or unexpected results arise principally from issues of floating-point accuracy due to the way floating-point values are computed and stored. The following sections explain the principal items of interest. Other peculiarities of floating-point arithmetic are presented in Features and Technical Specifications of the CUDA Programming Guide as well as in a whitepaper and accompanying webinar on floating-point precision and performance available from https://developer.nvidia.com/content/precision-performance-floating-point-and-ieee-754-compliance-nvidia-gpus.

7.3.1. Single vs. Double Precision 

Devices of CUDA Compute Capability 1.3 and higher provide native support for double-precision floating-point values (that is, values 64 bits wide). Results obtained using double-precision arithmetic will frequently differ from the same operation performed via single-precision arithmetic due to the greater precision of the former and due to rounding issues. Therefore, it is important to be sure to compare values of like precision and to express the results within a certain tolerance rather than expecting them to be exact.

7.3.2. Floating Point Math Is Not Associative 

Each floating-point arithmetic operation involves a certain amount of rounding. Consequently, the order in which arithmetic operations are performed is important. If A, B, and C are floating-point values, (A+B)+C is not guaranteed to equal A+(B+C) as it is in symbolic math. When you parallelize computations, you potentially change the order of operations and therefore the parallel results might not match sequential results. This limitation is not specific to CUDA, but an inherent part of parallel computation on floating-point values.

7.3.3. IEEE 754 Compliance 

All CUDA compute devices follow the IEEE 754 standard for binary floating-point representation, with some small exceptions. These exceptions, which are detailed in Features and Technical Specifications of the CUDA Programming Guide, can lead to results that differ from IEEE 754 values computed on the host system.

One of the key differences is the fused multiply-add (FMA) instruction, which combines multiply-add operations into a single instruction execution. Its result will often differ slightly from results obtained by doing the two operations separately.

7.3.4. x86 80-bit Computations 

x86 processors can use an 80-bit double extended precision math when performing floating-point calculations. The results of these calculations can frequently differ from pure 64-bit operations performed on the CUDA device. To get a closer match between values, set the x86 host processor to use regular double or single precision (64 bits and 32 bits, respectively). This is done with the FLDCW x86 assembly instruction or the equivalent operating system API.

8. Optimizing CUDA Applications 

After each round of application parallelization is complete, the developer can move to optimizing the implementation to improve performance. Since there are many possible optimizations that can be considered, having a good understanding of the needs of the application can help to make the process as smooth as possible. However, as with APOD as a whole, program optimization is an iterative process (identify an opportunity for optimization, apply and test the optimization, verify the speedup achieved, and repeat), meaning that it is not necessary for a programmer to spend large amounts of time memorizing the bulk of all possible optimization strategies prior to seeing good speedups. Instead, strategies can be applied incrementally as they are learned.

Optimizations can be applied at various levels, from overlapping data transfers with computation all the way down to fine-tuning floating-point operation sequences. The available profiling tools are invaluable for guiding this process, as they can help suggest a next-best course of action for the developer’s optimization efforts and provide references into the relevant portions of the optimization section of this guide.

9. Performance Metrics 

When attempting to optimize CUDA code, it pays to know how to measure performance accurately and to understand the role that bandwidth plays in performance measurement. This chapter discusses how to correctly measure performance using CPU timers and CUDA events. It then explores how bandwidth affects performance metrics and how to mitigate some of the challenges it poses.

9.1. Timing 

CUDA calls and kernel executions can be timed using either CPU or GPU timers. This section examines the functionality, advantages, and pitfalls of both approaches.

9.1.1. Using CPU Timers 

Any CPU timer can be used to measure the elapsed time of a CUDA call or kernel execution. The details of various CPU timing approaches are outside the scope of this document, but developers should always be aware of the resolution their timing calls provide.

When using CPU timers, it is critical to remember that many CUDA API functions are asynchronous; that is, they return control back to the calling CPU thread prior to completing their work. All kernel launches are asynchronous, as are memory-copy functions with the Async suffix on their names. Therefore, to accurately measure the elapsed time for a particular call or sequence of CUDA calls, it is necessary to synchronize the CPU thread with the GPU by calling cudaDeviceSynchronize() immediately before starting and stopping the CPU timer. cudaDeviceSynchronize() blocks the calling CPU thread until all CUDA calls previously issued by the thread are completed.

Although it is also possible to synchronize the CPU thread with a particular stream or event on the GPU, these synchronization functions are not suitable for timing code in streams other than the default stream. cudaStreamSynchronize() blocks the CPU thread until all CUDA calls previously issued into the given stream have completed. cudaEventSynchronize() blocks until a given event in a particular stream has been recorded by the GPU. Because the driver may interleave execution of CUDA calls from other non-default streams, calls in other streams may be included in the timing.

Because the default stream, stream 0, exhibits serializing behavior for work on the device (an operation in the default stream can begin only after all preceding calls in any stream have completed; and no subsequent operation in any stream can begin until it finishes), these functions can be used reliably for timing in the default stream.

Be aware that CPU-to-GPU synchronization points such as those mentioned in this section imply a stall in the GPU’s processing pipeline and should thus be used sparingly to minimize their performance impact.

9.1.2. Using CUDA GPU Timers 

The CUDA event API provides calls that create and destroy events, record events (including a timestamp), and convert timestamp differences into a floating-point value in milliseconds. How to time code using CUDA events illustrates their use.

How to time code using CUDA events

cudaEvent_t start, stop;
float time;

cudaEventCreate ( & start );
cudaEventCreate ( & stop );

cudaEventRecord ( start, 0 );
kernel <<< grid, threads >>> ( d_odata, d_idata, size_x, size_y,
NUM_REPS );
cudaEventRecord ( stop, 0 );
cudaEventSynchronize ( stop );

cudaEventElapsedTime ( & time, start, stop );
cudaEventDestroy ( start );
cudaEventDestroy ( stop );

Here cudaEventRecord() is used to place the start and stop events into the default stream, stream 0. The device will record a timestamp for the event when it reaches that event in the stream. The cudaEventElapsedTime() function returns the time elapsed between the recording of the start and stop events. This value is expressed in milliseconds and has a resolution of approximately half a microsecond. Like the other calls in this listing, their specific operation, parameters, and return values are described in the CUDA Toolkit Reference Manual. Note that the timings are measured on the GPU clock, so the timing resolution is operating-system-independent.

9.2. Bandwidth 

Bandwidth - the rate at which data can be transferred - is one of the most important gating factors for performance. Almost all changes to code should be made in the context of how they affect bandwidth. As described in Memory Optimizations of this guide, bandwidth can be dramatically affected by the choice of memory in which data is stored, how the data is laid out and the order in which it is accessed, as well as other factors.

To measure performance accurately, it is useful to calculate theoretical and effective bandwidth. When the latter is much lower than the former, design or implementation details are likely to reduce bandwidth, and it should be the primary goal of subsequent optimization efforts to increase it.

Note

High Priority: Use the effective bandwidth of your computation as a metric when measuring performance and optimization benefits.

9.2.1. Theoretical Bandwidth Calculation 

Theoretical bandwidth can be calculated using hardware specifications available in the product literature. For example, the NVIDIA Tesla V100 uses HBM2 (double data rate) RAM with a memory clock rate of 877 MHz and a 4096-bit-wide memory interface.

Using these data items, the peak theoretical memory bandwidth of the NVIDIA Tesla V100 is 898 GB/s:

\(\left. \left( 0.877 \times 10^{9} \right. \times (4096/8) \times 2 \right) \div 10^{9} = 898\text{GB/s}\)

In this calculation, the memory clock rate is converted into Hz, multiplied by the interface width (divided by 8, to convert bits to bytes) and multiplied by 2 due to the double data rate. Finally, this product is divided by 10 9 to convert the result to GB/s.

Note

Some calculations use 1024 3 instead of 10 9 for the final calculation. In such a case, the bandwidth would be 836.4 GiB/s. It is important to use the same divisor when calculating theoretical and effective bandwidth so that the comparison is valid.

Note

On GPUs with GDDR memory with ECC enabled the available DRAM is reduced by 6.25% to allow for the storage of ECC bits. Fetching ECC bits for each memory transaction also reduced the effective bandwidth by approximately 20% compared to the same GPU with ECC disabled, though the exact impact of ECC on bandwidth can be higher and depends on the memory access pattern. HBM2 memories, on the other hand, provide dedicated ECC resources, allowing overhead-free ECC protection. 2

9.2.2. Effective Bandwidth Calculation 

Effective bandwidth is calculated by timing specific program activities and by knowing how data is accessed by the program. To do so, use this equation:

\(\text{Effective\ bandwidth} = \left( {\left( B_{r} + B_{w} \right) \div 10^{9}} \right) \div \text{time}\)

Here, the effective bandwidth is in units of GB/s, B r is the number of bytes read per kernel, B w is the number of bytes written per kernel, and time is given in seconds.

For example, to compute the effective bandwidth of a 2048 x 2048 matrix copy, the following formula could be used:

\(\text{Effective\ bandwidth} = \left( {\left( 2048^{2} \times 4 \times 2 \right) \div 10^{9}} \right) \div \text{time}\)

The number of elements is multiplied by the size of each element (4 bytes for a float), multiplied by 2 (because of the read and write), divided by 10 9 (or 1,024 3 ) to obtain GB of memory transferred. This number is divided by the time in seconds to obtain GB/s.

9.2.3. Throughput Reported by Visual Profiler 

For devices with compute capability of 2.0 or greater, the Visual Profiler can be used to collect several different memory throughput measures. The following throughput metrics can be displayed in the Details or Detail Graphs view:

Requested Global Load Throughput

Requested Global Store Throughput

Global Load Throughput

Global Store Throughput

DRAM Read Throughput

DRAM Write Throughput

The Requested Global Load Throughput and Requested Global Store Throughput values indicate the global memory throughput requested by the kernel and therefore correspond to the effective bandwidth obtained by the calculation shown under Effective Bandwidth Calculation.

Because the minimum memory transaction size is larger than most word sizes, the actual memory throughput required for a kernel can include the transfer of data not used by the kernel. For global memory accesses, this actual throughput is reported by the Global Load Throughput and Global Store Throughput values.

It’s important to note that both numbers are useful. The actual memory throughput shows how close the code is to the hardware limit, and a comparison of the effective or requested bandwidth to the actual bandwidth presents a good estimate of how much bandwidth is wasted by suboptimal coalescing of memory accesses (see Coalesced Access to Global Memory ). For global memory accesses, this comparison of requested memory bandwidth to actual me
