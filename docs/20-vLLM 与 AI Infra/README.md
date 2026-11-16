# vLLM 与 AI Infra

vLLM/SGLang 推理系统、PagedAttention、连续批处理、KV Cache、量化、投机解码、CUDA/Triton 算子优化、FlashAttention、Tensor Core、GPU 体系结构与性能建模（Roofline）等主题的联网采集资料库。

共 109 篇，合计 2,933,002 字符。

| # | 文档 | 源文件 | 字符数 |
| --- | --- | --- | ---: |
| 1 | [00 索引与导读：这个库怎么用](00%20%E7%B4%A2%E5%BC%95%E4%B8%8E%E5%AF%BC%E8%AF%BB%EF%BC%9A%E8%BF%99%E4%B8%AA%E5%BA%93%E6%80%8E%E4%B9%88%E7%94%A8.md) | （纯文本条目） | 1,900 |
| 2 | [01 vLLM 架构总览：请求的完整生命周期](01%20vLLM%20%E6%9E%B6%E6%9E%84%E6%80%BB%E8%A7%88%EF%BC%9A%E8%AF%B7%E6%B1%82%E7%9A%84%E5%AE%8C%E6%95%B4%E7%94%9F%E5%91%BD%E5%91%A8%E6%9C%9F.md) | （纯文本条目） | 2,395 |
| 3 | [02 PagedAttention 与 KV Cache 管理](02%20PagedAttention%20%E4%B8%8E%20KV%20Cache%20%E7%AE%A1%E7%90%86.md) | （纯文本条目） | 3,029 |
| 4 | [03 推理性能优化技术栈全景](03%20%E6%8E%A8%E7%90%86%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E6%8A%80%E6%9C%AF%E6%A0%88%E5%85%A8%E6%99%AF.md) | （纯文本条目） | 3,049 |
| 5 | [04 GPU 体系结构与 CUDA 执行模型](04%20GPU%20%E4%BD%93%E7%B3%BB%E7%BB%93%E6%9E%84%E4%B8%8E%20CUDA%20%E6%89%A7%E8%A1%8C%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 3,276 |
| 6 | [05 CUDA 算子优化方法论：从朴素实现到 cuBLAS 级](05%20CUDA%20%E7%AE%97%E5%AD%90%E4%BC%98%E5%8C%96%E6%96%B9%E6%B3%95%E8%AE%BA%EF%BC%9A%E4%BB%8E%E6%9C%B4%E7%B4%A0%E5%AE%9E%E7%8E%B0%E5%88%B0%20cuBLAS%20%E7%BA%A7.md) | （纯文本条目） | 2,930 |
| 7 | [06 Attention 算子：FlashAttention 与 PagedAttention 实现要点](06%20Attention%20%E7%AE%97%E5%AD%90%EF%BC%9AFlashAttention%20%E4%B8%8E%20PagedAttention%20%E5%AE%9E%E7%8E%B0%E8%A6%81%E7%82%B9.md) | （纯文本条目） | 2,626 |
| 8 | [07 Triton 算子开发](07%20Triton%20%E7%AE%97%E5%AD%90%E5%BC%80%E5%8F%91.md) | （纯文本条目） | 2,799 |
| 9 | [08 量化与低精度推理](08%20%E9%87%8F%E5%8C%96%E4%B8%8E%E4%BD%8E%E7%B2%BE%E5%BA%A6%E6%8E%A8%E7%90%86.md) | （纯文本条目） | 2,456 |
| 10 | [09 分布式并行与部署形态](09%20%E5%88%86%E5%B8%83%E5%BC%8F%E5%B9%B6%E8%A1%8C%E4%B8%8E%E9%83%A8%E7%BD%B2%E5%BD%A2%E6%80%81.md) | （纯文本条目） | 2,231 |
| 11 | [10 性能建模与 Profiling：怎么算、怎么看](10%20%E6%80%A7%E8%83%BD%E5%BB%BA%E6%A8%A1%E4%B8%8E%20Profiling%EF%BC%9A%E6%80%8E%E4%B9%88%E7%AE%97%E3%80%81%E6%80%8E%E4%B9%88%E7%9C%8B.md) | （纯文本条目） | 3,136 |
| 12 | [11 大模型与 AI 前沿论文导读](11%20%E5%A4%A7%E6%A8%A1%E5%9E%8B%E4%B8%8E%20AI%20%E5%89%8D%E6%B2%BF%E8%AE%BA%E6%96%87%E5%AF%BC%E8%AF%BB.md) | （纯文本条目） | 3,646 |
| 13 | [99 来源清单与抓取限制](99%20%E6%9D%A5%E6%BA%90%E6%B8%85%E5%8D%95%E4%B8%8E%E6%8A%93%E5%8F%96%E9%99%90%E5%88%B6.md) | （纯文本条目） | 10,033 |
| 14 | [99A 原始资料来源索引](99A%20%E5%8E%9F%E5%A7%8B%E8%B5%84%E6%96%99%E6%9D%A5%E6%BA%90%E7%B4%A2%E5%BC%95.md) | （纯文本条目） | 8,369 |
| 15 | [AI Infra 训练栈总览（Infrasys-AI）](AI%20Infra%20%E8%AE%AD%E7%BB%83%E6%A0%88%E6%80%BB%E8%A7%88%EF%BC%88Infrasys-AI%EF%BC%89.md) | （纯文本条目） | 12,941 |
| 16 | [CUDA C++ 最佳实践指南（官方）](CUDA%20C%2B%2B%20%E6%9C%80%E4%BD%B3%E5%AE%9E%E8%B7%B5%E6%8C%87%E5%8D%97%EF%BC%88%E5%AE%98%E6%96%B9%EF%BC%89.md) | （纯文本条目） | 50,233 |
| 17 | [CUDA Tile IR 编程模型](CUDA%20Tile%20IR%20%E7%BC%96%E7%A8%8B%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,011 |
| 18 | [CUDA 入门教程（更新版）](CUDA%20%E5%85%A5%E9%97%A8%E6%95%99%E7%A8%8B%EF%BC%88%E6%9B%B4%E6%96%B0%E7%89%88%EF%BC%89.md) | （纯文本条目） | 11,327 |
| 19 | [CUDA 入门：warp 与线程束](CUDA%20%E5%85%A5%E9%97%A8%EF%BC%9Awarp%20%E4%B8%8E%E7%BA%BF%E7%A8%8B%E6%9D%9F.md) | （纯文本条目） | 4,693 |
| 20 | [CUDA 学习笔记与高频面试题（sgemm/sgemv/warp）](CUDA%20%E5%AD%A6%E4%B9%A0%E7%AC%94%E8%AE%B0%E4%B8%8E%E9%AB%98%E9%A2%91%E9%9D%A2%E8%AF%95%E9%A2%98%EF%BC%88sgemm-sgemv-warp%EF%BC%89.md) | （纯文本条目） | 31,169 |
| 21 | [CUDA 性能优化实战：并行归约算法优化七步](CUDA%20%E6%80%A7%E8%83%BD%E4%BC%98%E5%8C%96%E5%AE%9E%E6%88%98%EF%BC%9A%E5%B9%B6%E8%A1%8C%E5%BD%92%E7%BA%A6%E7%AE%97%E6%B3%95%E4%BC%98%E5%8C%96%E4%B8%83%E6%AD%A5.md) | （纯文本条目） | 14,491 |
| 22 | [CUDA 算子手撕与面试代码集](CUDA%20%E7%AE%97%E5%AD%90%E6%89%8B%E6%92%95%E4%B8%8E%E9%9D%A2%E8%AF%95%E4%BB%A3%E7%A0%81%E9%9B%86.md) | （纯文本条目） | 21,963 |
| 23 | [CUDA 算子手撕与面试指南](CUDA%20%E7%AE%97%E5%AD%90%E6%89%8B%E6%92%95%E4%B8%8E%E9%9D%A2%E8%AF%95%E6%8C%87%E5%8D%97.md) | （纯文本条目） | 38,038 |
| 24 | [CUTLASS Operator API：基础 GEMM 教程](CUTLASS%20Operator%20API%EF%BC%9A%E5%9F%BA%E7%A1%80%20GEMM%20%E6%95%99%E7%A8%8B.md) | （纯文本条目） | 16,177 |
| 25 | [FlashAttention 系列技术详解](FlashAttention%20%E7%B3%BB%E5%88%97%E6%8A%80%E6%9C%AF%E8%AF%A6%E8%A7%A3.md) | （纯文本条目） | 5,634 |
| 26 | [FlashAttention：快速且显存高效的精确注意力（论文 2205.14135）](FlashAttention%EF%BC%9A%E5%BF%AB%E9%80%9F%E4%B8%94%E6%98%BE%E5%AD%98%E9%AB%98%E6%95%88%E7%9A%84%E7%B2%BE%E7%A1%AE%E6%B3%A8%E6%84%8F%E5%8A%9B%EF%BC%88%E8%AE%BA%E6%96%87%202205.14135%EF%BC%89.md) | （纯文本条目） | 50,180 |
| 27 | [FlashInfer：为 LLM 服务加速自注意力算子](FlashInfer%EF%BC%9A%E4%B8%BA%20LLM%20%E6%9C%8D%E5%8A%A1%E5%8A%A0%E9%80%9F%E8%87%AA%E6%B3%A8%E6%84%8F%E5%8A%9B%E7%AE%97%E5%AD%90.md) | （纯文本条目） | 23,030 |
| 28 | [GPU MODE 公开课资料（CUDA/GPU 编程）](GPU%20MODE%20%E5%85%AC%E5%BC%80%E8%AF%BE%E8%B5%84%E6%96%99%EF%BC%88CUDA-GPU%20%E7%BC%96%E7%A8%8B%EF%BC%89.md) | （纯文本条目） | 11,284 |
| 29 | [GPU 性能背景：数学带宽、Roofline 与 kernel 效率（NVIDIA）](GPU%20%E6%80%A7%E8%83%BD%E8%83%8C%E6%99%AF%EF%BC%9A%E6%95%B0%E5%AD%A6%E5%B8%A6%E5%AE%BD%E3%80%81Roofline%20%E4%B8%8E%20kernel%20%E6%95%88%E7%8E%87%EF%BC%88NVIDIA%EF%BC%89.md) | （纯文本条目） | 26,354 |
| 30 | [GPU 术语：内存合并（Memory Coalescing）](GPU%20%E6%9C%AF%E8%AF%AD%EF%BC%9A%E5%86%85%E5%AD%98%E5%90%88%E5%B9%B6%EF%BC%88Memory%20Coalescing%EF%BC%89.md) | （纯文本条目） | 6,790 |
| 31 | [KV Cache：加速 LLM 推理的关键](KV%20Cache%EF%BC%9A%E5%8A%A0%E9%80%9F%20LLM%20%E6%8E%A8%E7%90%86%E7%9A%84%E5%85%B3%E9%94%AE.md) | （纯文本条目） | 3,283 |
| 32 | [Lilian Weng：大型 Transformer 模型推理优化](Lilian%20Weng%EF%BC%9A%E5%A4%A7%E5%9E%8B%20Transformer%20%E6%A8%A1%E5%9E%8B%E6%8E%A8%E7%90%86%E4%BC%98%E5%8C%96.md) | （纯文本条目） | 48,083 |
| 33 | [LLM 推理的 Roofline 模型基础](LLM%20%E6%8E%A8%E7%90%86%E7%9A%84%20Roofline%20%E6%A8%A1%E5%9E%8B%E5%9F%BA%E7%A1%80.md) | （纯文本条目） | 17,850 |
| 34 | [Nsight Compute 计算性能排查指南](Nsight%20Compute%20%E8%AE%A1%E7%AE%97%E6%80%A7%E8%83%BD%E6%8E%92%E6%9F%A5%E6%8C%87%E5%8D%97.md) | （纯文本条目） | 50,057 |
| 35 | [NVIDIA：LLM 推理优化技术全景（Mastering LLM Techniques）](NVIDIA%EF%BC%9ALLM%20%E6%8E%A8%E7%90%86%E4%BC%98%E5%8C%96%E6%8A%80%E6%9C%AF%E5%85%A8%E6%99%AF%EF%BC%88Mastering%20LLM%20Techniques%EF%BC%89.md) | （纯文本条目） | 36,380 |
| 36 | [Orca 论文摘要页（OSDI'22，连续批处理出处）](Orca%20%E8%AE%BA%E6%96%87%E6%91%98%E8%A6%81%E9%A1%B5%EF%BC%88OSDI%2722%EF%BC%8C%E8%BF%9E%E7%BB%AD%E6%89%B9%E5%A4%84%E7%90%86%E5%87%BA%E5%A4%84%EF%BC%89.md) | （纯文本条目） | 6,106 |
| 37 | [PagedAttention 论文：面向大模型服务的高效内存管理（SOSP'23 / 2309.06180）](PagedAttention%20%E8%AE%BA%E6%96%87%EF%BC%9A%E9%9D%A2%E5%90%91%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%9C%8D%E5%8A%A1%E7%9A%84%E9%AB%98%E6%95%88%E5%86%85%E5%AD%98%E7%AE%A1%E7%90%86%EF%BC%88SOSP%2723%20-%202309.06180%EF%BC%89.md) | （纯文本条目） | 50,137 |
| 38 | [SGLang：RadixAttention 与快速可表达的 LLM 推理](SGLang%EF%BC%9ARadixAttention%20%E4%B8%8E%E5%BF%AB%E9%80%9F%E5%8F%AF%E8%A1%A8%E8%BE%BE%E7%9A%84%20LLM%20%E6%8E%A8%E7%90%86.md) | （纯文本条目） | 13,265 |
| 39 | [SGLang：面向结构化语言模型程序的高效执行（论文 2312.07104）](SGLang%EF%BC%9A%E9%9D%A2%E5%90%91%E7%BB%93%E6%9E%84%E5%8C%96%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B%E7%A8%8B%E5%BA%8F%E7%9A%84%E9%AB%98%E6%95%88%E6%89%A7%E8%A1%8C%EF%BC%88%E8%AE%BA%E6%96%87%202312.07104%EF%BC%89.md) | （纯文本条目） | 50,164 |
| 40 | [Taming the Titans：高效 LLM 推理服务综述（2025）](Taming%20the%20Titans%EF%BC%9A%E9%AB%98%E6%95%88%20LLM%20%E6%8E%A8%E7%90%86%E6%9C%8D%E5%8A%A1%E7%BB%BC%E8%BF%B0%EF%BC%882025%EF%BC%89.md) | （纯文本条目） | 50,137 |
| 41 | [tcgen05 MMA 编程指南（CUTLASS）](tcgen05%20MMA%20%E7%BC%96%E7%A8%8B%E6%8C%87%E5%8D%97%EF%BC%88CUTLASS%EF%BC%89.md) | （纯文本条目） | 50,317 |
| 42 | [TensorRT-LLM：Paged Attention、IFB 与请求调度](TensorRT-LLM%EF%BC%9APaged%20Attention%E3%80%81IFB%20%E4%B8%8E%E8%AF%B7%E6%B1%82%E8%B0%83%E5%BA%A6.md) | （纯文本条目） | 19,986 |
| 43 | [Triton 教程：矩阵乘法（Matrix Multiplication）](Triton%20%E6%95%99%E7%A8%8B%EF%BC%9A%E7%9F%A9%E9%98%B5%E4%B9%98%E6%B3%95%EF%BC%88Matrix%20Multiplication%EF%BC%89.md) | （纯文本条目） | 24,456 |
| 44 | [Triton 教程：融合 Softmax](Triton%20%E6%95%99%E7%A8%8B%EF%BC%9A%E8%9E%8D%E5%90%88%20Softmax.md) | （纯文本条目） | 15,247 |
| 45 | [Triton 算子开发指南（Triton Ascend 中文）](Triton%20%E7%AE%97%E5%AD%90%E5%BC%80%E5%8F%91%E6%8C%87%E5%8D%97%EF%BC%88Triton%20Ascend%20%E4%B8%AD%E6%96%87%EF%BC%89.md) | （纯文本条目） | 37,100 |
| 46 | [vLLM Chunked Prefill 文档](vLLM%20Chunked%20Prefill%20%E6%96%87%E6%A1%A3.md) | （纯文本条目） | 8,521 |
| 47 | [vLLM Fused MoE Kernel 特性](vLLM%20Fused%20MoE%20Kernel%20%E7%89%B9%E6%80%A7.md) | （纯文本条目） | 17,482 |
| 48 | [vLLM Fused MoE 模块化 Kernel](vLLM%20Fused%20MoE%20%E6%A8%A1%E5%9D%97%E5%8C%96%20Kernel.md) | （纯文本条目） | 25,210 |
| 49 | [vLLM V1：核心架构的重大升级（Red Hat 解读）](vLLM%20V1%EF%BC%9A%E6%A0%B8%E5%BF%83%E6%9E%B6%E6%9E%84%E7%9A%84%E9%87%8D%E5%A4%A7%E5%8D%87%E7%BA%A7%EF%BC%88Red%20Hat%20%E8%A7%A3%E8%AF%BB%EF%BC%89.md) | （纯文本条目） | 19,375 |
| 50 | [vLLM V1：核心架构重大升级（官方博客）](vLLM%20V1%EF%BC%9A%E6%A0%B8%E5%BF%83%E6%9E%B6%E6%9E%84%E9%87%8D%E5%A4%A7%E5%8D%87%E7%BA%A7%EF%BC%88%E5%AE%98%E6%96%B9%E5%8D%9A%E5%AE%A2%EF%BC%89.md) | （纯文本条目） | 16,283 |
| 51 | [vLLM 性能飞跃部署实践：从推理加速到高效部署](vLLM%20%E6%80%A7%E8%83%BD%E9%A3%9E%E8%B7%83%E9%83%A8%E7%BD%B2%E5%AE%9E%E8%B7%B5%EF%BC%9A%E4%BB%8E%E6%8E%A8%E7%90%86%E5%8A%A0%E9%80%9F%E5%88%B0%E9%AB%98%E6%95%88%E9%83%A8%E7%BD%B2.md) | （纯文本条目） | 10,446 |
| 52 | [vLLM 投机解码（Speculative Decoding）文档](vLLM%20%E6%8A%95%E6%9C%BA%E8%A7%A3%E7%A0%81%EF%BC%88Speculative%20Decoding%EF%BC%89%E6%96%87%E6%A1%A3.md) | （纯文本条目） | 21,873 |
| 53 | [vLLM 教程 01：认识 vLLM](vLLM%20%E6%95%99%E7%A8%8B%2001%EF%BC%9A%E8%AE%A4%E8%AF%86%20vLLM.md) | （纯文本条目） | 11,101 |
| 54 | [vLLM 数据并行部署（Data Parallel）文档](vLLM%20%E6%95%B0%E6%8D%AE%E5%B9%B6%E8%A1%8C%E9%83%A8%E7%BD%B2%EF%BC%88Data%20Parallel%EF%BC%89%E6%96%87%E6%A1%A3.md) | （纯文本条目） | 18,559 |
| 55 | [vLLM 架构分析（博客园）](vLLM%20%E6%9E%B6%E6%9E%84%E5%88%86%E6%9E%90%EF%BC%88%E5%8D%9A%E5%AE%A2%E5%9B%AD%EF%BC%89.md) | （纯文本条目） | 11,230 |
| 56 | [vLLM 流水线并行（Pipeline Parallelism）](vLLM%20%E6%B5%81%E6%B0%B4%E7%BA%BF%E5%B9%B6%E8%A1%8C%EF%BC%88Pipeline%20Parallelism%EF%BC%89.md) | （纯文本条目） | 26,014 |
| 57 | [vLLM 源码解析（二）：调度策略分析](vLLM%20%E6%BA%90%E7%A0%81%E8%A7%A3%E6%9E%90%EF%BC%88%E4%BA%8C%EF%BC%89%EF%BC%9A%E8%B0%83%E5%BA%A6%E7%AD%96%E7%95%A5%E5%88%86%E6%9E%90.md) | （纯文本条目） | 50,223 |
| 58 | [vLLM 自动前缀缓存（APC）文档](vLLM%20%E8%87%AA%E5%8A%A8%E5%89%8D%E7%BC%80%E7%BC%93%E5%AD%98%EF%BC%88APC%EF%BC%89%E6%96%87%E6%A1%A3.md) | （纯文本条目） | 14,185 |
| 59 | [vLLM 设计文档：Paged Attention 原理](vLLM%20%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3%EF%BC%9APaged%20Attention%20%E5%8E%9F%E7%90%86.md) | （纯文本条目） | 28,904 |
| 60 | [vLLM 设计文档：PagedAttention CUDA kernel 实现](vLLM%20%E8%AE%BE%E8%AE%A1%E6%96%87%E6%A1%A3%EF%BC%9APagedAttention%20CUDA%20kernel%20%E5%AE%9E%E7%8E%B0.md) | （纯文本条目） | 27,198 |
| 61 | [vLLM 量化（Quantization）文档总览](vLLM%20%E9%87%8F%E5%8C%96%EF%BC%88Quantization%EF%BC%89%E6%96%87%E6%A1%A3%E6%80%BB%E8%A7%88.md) | （纯文本条目） | 18,026 |
| 62 | [什么是 KV Cache（Key-Value Cache）](%E4%BB%80%E4%B9%88%E6%98%AF%20KV%20Cache%EF%BC%88Key-Value%20Cache%EF%BC%89.md) | （纯文本条目） | 21,197 |
| 63 | [从 KV-Cache 到 PagedAttention：LLM 推理性能细节](%E4%BB%8E%20KV-Cache%20%E5%88%B0%20PagedAttention%EF%BC%9ALLM%20%E6%8E%A8%E7%90%86%E6%80%A7%E8%83%BD%E7%BB%86%E8%8A%82.md) | （纯文本条目） | 7,120 |
| 64 | [从显存瓶颈到推理革命：vLLM 为何成为大模型服务标配](%E4%BB%8E%E6%98%BE%E5%AD%98%E7%93%B6%E9%A2%88%E5%88%B0%E6%8E%A8%E7%90%86%E9%9D%A9%E5%91%BD%EF%BC%9AvLLM%20%E4%B8%BA%E4%BD%95%E6%88%90%E4%B8%BA%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%9C%8D%E5%8A%A1%E6%A0%87%E9%85%8D.md) | （纯文本条目） | 6,248 |
| 65 | [在 CUDA C++ 中为 5090 写出接近峰值的 Flash Attention](%E5%9C%A8%20CUDA%20C%2B%2B%20%E4%B8%AD%E4%B8%BA%205090%20%E5%86%99%E5%87%BA%E6%8E%A5%E8%BF%91%E5%B3%B0%E5%80%BC%E7%9A%84%20Flash%20Attention.md) | （纯文本条目） | 50,140 |
| 66 | [在 NVIDIA CUDA Tile 中把 Flash Attention 调优到峰值性能](%E5%9C%A8%20NVIDIA%20CUDA%20Tile%20%E4%B8%AD%E6%8A%8A%20Flash%20Attention%20%E8%B0%83%E4%BC%98%E5%88%B0%E5%B3%B0%E5%80%BC%E6%80%A7%E8%83%BD.md) | （纯文本条目） | 31,994 |
| 67 | [如何把 CUDA Matmul Kernel 优化到 cuBLAS 级性能](%E5%A6%82%E4%BD%95%E6%8A%8A%20CUDA%20Matmul%20Kernel%20%E4%BC%98%E5%8C%96%E5%88%B0%20cuBLAS%20%E7%BA%A7%E6%80%A7%E8%83%BD.md) | （纯文本条目） | 49,333 |
| 68 | [深入 vLLM：高吞吐 LLM 推理系统解剖（官方博客）](%E6%B7%B1%E5%85%A5%20vLLM%EF%BC%9A%E9%AB%98%E5%90%9E%E5%90%90%20LLM%20%E6%8E%A8%E7%90%86%E7%B3%BB%E7%BB%9F%E8%A7%A3%E5%89%96%EF%BC%88%E5%AE%98%E6%96%B9%E5%8D%9A%E5%AE%A2%EF%BC%89.md) | （纯文本条目） | 50,172 |
| 69 | [用 NVFP4 KV 缓存优化长上下文与大 batch 推理](%E7%94%A8%20NVFP4%20KV%20%E7%BC%93%E5%AD%98%E4%BC%98%E5%8C%96%E9%95%BF%E4%B8%8A%E4%B8%8B%E6%96%87%E4%B8%8E%E5%A4%A7%20batch%20%E6%8E%A8%E7%90%86.md) | （纯文本条目） | 6,624 |
| 70 | [用 NVIDIA CUDA Tile 简化 GPU 编程（Python）](%E7%94%A8%20NVIDIA%20CUDA%20Tile%20%E7%AE%80%E5%8C%96%20GPU%20%E7%BC%96%E7%A8%8B%EF%BC%88Python%EF%BC%89.md) | （纯文本条目） | 13,147 |
| 71 | [知乎·CUDA 算子优化系列(1)：CUDA Reduce](%E7%9F%A5%E4%B9%8E%C2%B7CUDA%20%E7%AE%97%E5%AD%90%E4%BC%98%E5%8C%96%E7%B3%BB%E5%88%97%281%29%EF%BC%9ACUDA%20Reduce.md) | （纯文本条目） | 16,852 |
| 72 | [知乎·CUDA 编程入门极简教程](%E7%9F%A5%E4%B9%8E%C2%B7CUDA%20%E7%BC%96%E7%A8%8B%E5%85%A5%E9%97%A8%E6%9E%81%E7%AE%80%E6%95%99%E7%A8%8B.md) | （纯文本条目） | 13,307 |
| 73 | [知乎·LLM 提速利器：投机推理的原理与常见方案](%E7%9F%A5%E4%B9%8E%C2%B7LLM%20%E6%8F%90%E9%80%9F%E5%88%A9%E5%99%A8%EF%BC%9A%E6%8A%95%E6%9C%BA%E6%8E%A8%E7%90%86%E7%9A%84%E5%8E%9F%E7%90%86%E4%B8%8E%E5%B8%B8%E8%A7%81%E6%96%B9%E6%A1%88.md) | （纯文本条目） | 7,980 |
| 74 | [知乎·图解大模型计算加速系列之：vLLM 核心技术 PagedAttention 原理](%E7%9F%A5%E4%B9%8E%C2%B7%E5%9B%BE%E8%A7%A3%E5%A4%A7%E6%A8%A1%E5%9E%8B%E8%AE%A1%E7%AE%97%E5%8A%A0%E9%80%9F%E7%B3%BB%E5%88%97%E4%B9%8B%EF%BC%9AvLLM%20%E6%A0%B8%E5%BF%83%E6%8A%80%E6%9C%AF%20PagedAttention%20%E5%8E%9F%E7%90%86.md) | （纯文本条目） | 11,742 |
| 75 | [知乎·图解大模型计算加速系列：FlashAttention V1，从硬件到计算逻辑](%E7%9F%A5%E4%B9%8E%C2%B7%E5%9B%BE%E8%A7%A3%E5%A4%A7%E6%A8%A1%E5%9E%8B%E8%AE%A1%E7%AE%97%E5%8A%A0%E9%80%9F%E7%B3%BB%E5%88%97%EF%BC%9AFlashAttention%20V1%EF%BC%8C%E4%BB%8E%E7%A1%AC%E4%BB%B6%E5%88%B0%E8%AE%A1%E7%AE%97%E9%80%BB%E8%BE%91.md) | （纯文本条目） | 26,384 |
| 76 | [知乎·大模型推理与 AI Infra 软硬件栈协同优化](%E7%9F%A5%E4%B9%8E%C2%B7%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%8E%A8%E7%90%86%E4%B8%8E%20AI%20Infra%20%E8%BD%AF%E7%A1%AC%E4%BB%B6%E6%A0%88%E5%8D%8F%E5%90%8C%E4%BC%98%E5%8C%96.md) | （纯文本条目） | 21,625 |
| 77 | [自定义 Triton Kernel 把带宽利用率从 11% 提到 88%](%E8%87%AA%E5%AE%9A%E4%B9%89%20Triton%20Kernel%20%E6%8A%8A%E5%B8%A6%E5%AE%BD%E5%88%A9%E7%94%A8%E7%8E%87%E4%BB%8E%2011%25%20%E6%8F%90%E5%88%B0%2088%25.md) | （纯文本条目） | 12,895 |
| 78 | [解密 vLLM：基于 nano-vllm 源码剖析推理加速之道](%E8%A7%A3%E5%AF%86%20vLLM%EF%BC%9A%E5%9F%BA%E4%BA%8E%20nano-vllm%20%E6%BA%90%E7%A0%81%E5%89%96%E6%9E%90%E6%8E%A8%E7%90%86%E5%8A%A0%E9%80%9F%E4%B9%8B%E9%81%93.md) | （纯文本条目） | 7,473 |
| 79 | [论文·Attention Is All You Need（Transformer 原始论文）](%E8%AE%BA%E6%96%87%C2%B7Attention%20Is%20All%20You%20Need%EF%BC%88Transformer%20%E5%8E%9F%E5%A7%8B%E8%AE%BA%E6%96%87%EF%BC%89.md) | （纯文本条目） | 41,918 |
| 80 | [论文·AWQ：激活感知的权重量化](%E8%AE%BA%E6%96%87%C2%B7AWQ%EF%BC%9A%E6%BF%80%E6%B4%BB%E6%84%9F%E7%9F%A5%E7%9A%84%E6%9D%83%E9%87%8D%E9%87%8F%E5%8C%96.md) | （纯文本条目） | 45,087 |
| 81 | [论文·BitNet b1.58：1-bit 大模型时代](%E8%AE%BA%E6%96%87%C2%B7BitNet%20b1.58%EF%BC%9A1-bit%20%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%97%B6%E4%BB%A3.md) | （纯文本条目） | 23,391 |
| 82 | [论文·Chain-of-Thought 提示：激发大模型推理能力](%E8%AE%BA%E6%96%87%C2%B7Chain-of-Thought%20%E6%8F%90%E7%A4%BA%EF%BC%9A%E6%BF%80%E5%8F%91%E5%A4%A7%E6%A8%A1%E5%9E%8B%E6%8E%A8%E7%90%86%E8%83%BD%E5%8A%9B.md) | （纯文本条目） | 45,087 |
| 83 | [论文·Chameleon：混合模态早期融合基础模型](%E8%AE%BA%E6%96%87%C2%B7Chameleon%EF%BC%9A%E6%B7%B7%E5%90%88%E6%A8%A1%E6%80%81%E6%97%A9%E6%9C%9F%E8%9E%8D%E5%90%88%E5%9F%BA%E7%A1%80%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,087 |
| 84 | [论文·Chinchilla：训练计算最优的大语言模型](%E8%AE%BA%E6%96%87%C2%B7Chinchilla%EF%BC%9A%E8%AE%AD%E7%BB%83%E8%AE%A1%E7%AE%97%E6%9C%80%E4%BC%98%E7%9A%84%E5%A4%A7%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,087 |
| 85 | [论文·DDPM：去噪扩散概率模型](%E8%AE%BA%E6%96%87%C2%B7DDPM%EF%BC%9A%E5%8E%BB%E5%99%AA%E6%89%A9%E6%95%A3%E6%A6%82%E7%8E%87%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,087 |
| 86 | [论文·DeepSeek-R1：用强化学习激励推理能力](%E8%AE%BA%E6%96%87%C2%B7DeepSeek-R1%EF%BC%9A%E7%94%A8%E5%BC%BA%E5%8C%96%E5%AD%A6%E4%B9%A0%E6%BF%80%E5%8A%B1%E6%8E%A8%E7%90%86%E8%83%BD%E5%8A%9B.md) | （纯文本条目） | 45,087 |
| 87 | [论文·DeepSeek-V2：多头潜在注意力（MLA）与 DeepSeekMoE](%E8%AE%BA%E6%96%87%C2%B7DeepSeek-V2%EF%BC%9A%E5%A4%9A%E5%A4%B4%E6%BD%9C%E5%9C%A8%E6%B3%A8%E6%84%8F%E5%8A%9B%EF%BC%88MLA%EF%BC%89%E4%B8%8E%20DeepSeekMoE.md) | （纯文本条目） | 45,087 |
| 88 | [论文·DeepSeek-V3 技术报告（MoE + MLA + FP8 训练）](%E8%AE%BA%E6%96%87%C2%B7DeepSeek-V3%20%E6%8A%80%E6%9C%AF%E6%8A%A5%E5%91%8A%EF%BC%88MoE%20%2B%20MLA%20%2B%20FP8%20%E8%AE%AD%E7%BB%83%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 89 | [论文·DPO：直接偏好优化](%E8%AE%BA%E6%96%87%C2%B7DPO%EF%BC%9A%E7%9B%B4%E6%8E%A5%E5%81%8F%E5%A5%BD%E4%BC%98%E5%8C%96.md) | （纯文本条目） | 45,087 |
| 90 | [论文·EAGLE：投机采样的特征级草稿模型](%E8%AE%BA%E6%96%87%C2%B7EAGLE%EF%BC%9A%E6%8A%95%E6%9C%BA%E9%87%87%E6%A0%B7%E7%9A%84%E7%89%B9%E5%BE%81%E7%BA%A7%E8%8D%89%E7%A8%BF%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,087 |
| 91 | [论文·FlashAttention-2：更好的并行与工作划分](%E8%AE%BA%E6%96%87%C2%B7FlashAttention-2%EF%BC%9A%E6%9B%B4%E5%A5%BD%E7%9A%84%E5%B9%B6%E8%A1%8C%E4%B8%8E%E5%B7%A5%E4%BD%9C%E5%88%92%E5%88%86.md) | （纯文本条目） | 45,087 |
| 92 | [论文·FlashAttention-3：Hopper 上的异步与低精度注意力](%E8%AE%BA%E6%96%87%C2%B7FlashAttention-3%EF%BC%9AHopper%20%E4%B8%8A%E7%9A%84%E5%BC%82%E6%AD%A5%E4%B8%8E%E4%BD%8E%E7%B2%BE%E5%BA%A6%E6%B3%A8%E6%84%8F%E5%8A%9B.md) | （纯文本条目） | 45,087 |
| 93 | [论文·GPT-4 技术报告](%E8%AE%BA%E6%96%87%C2%B7GPT-4%20%E6%8A%80%E6%9C%AF%E6%8A%A5%E5%91%8A.md) | （纯文本条目） | 45,087 |
| 94 | [论文·GPTQ：生成式预训练 Transformer 的量化（4-bit 权重量化）](%E8%AE%BA%E6%96%87%C2%B7GPTQ%EF%BC%9A%E7%94%9F%E6%88%90%E5%BC%8F%E9%A2%84%E8%AE%AD%E7%BB%83%20Transformer%20%E7%9A%84%E9%87%8F%E5%8C%96%EF%BC%884-bit%20%E6%9D%83%E9%87%8D%E9%87%8F%E5%8C%96%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 95 | [论文·Llama 3 Herd of Models（技术报告）](%E8%AE%BA%E6%96%87%C2%B7Llama%203%20Herd%20of%20Models%EF%BC%88%E6%8A%80%E6%9C%AF%E6%8A%A5%E5%91%8A%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 96 | [论文·LoRA：低秩适配（低秩微调）](%E8%AE%BA%E6%96%87%C2%B7LoRA%EF%BC%9A%E4%BD%8E%E7%A7%A9%E9%80%82%E9%85%8D%EF%BC%88%E4%BD%8E%E7%A7%A9%E5%BE%AE%E8%B0%83%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 97 | [论文·Mamba-2：状态空间对偶与高效实现](%E8%AE%BA%E6%96%87%C2%B7Mamba-2%EF%BC%9A%E7%8A%B6%E6%80%81%E7%A9%BA%E9%97%B4%E5%AF%B9%E5%81%B6%E4%B8%8E%E9%AB%98%E6%95%88%E5%AE%9E%E7%8E%B0.md) | （纯文本条目） | 45,087 |
| 98 | [论文·Mamba：线性时间序列建模（选择性状态空间）](%E8%AE%BA%E6%96%87%C2%B7Mamba%EF%BC%9A%E7%BA%BF%E6%80%A7%E6%97%B6%E9%97%B4%E5%BA%8F%E5%88%97%E5%BB%BA%E6%A8%A1%EF%BC%88%E9%80%89%E6%8B%A9%E6%80%A7%E7%8A%B6%E6%80%81%E7%A9%BA%E9%97%B4%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 99 | [论文·Mixtral of Experts（稀疏专家混合）](%E8%AE%BA%E6%96%87%C2%B7Mixtral%20of%20Experts%EF%BC%88%E7%A8%80%E7%96%8F%E4%B8%93%E5%AE%B6%E6%B7%B7%E5%90%88%EF%BC%89.md) | （纯文本条目） | 32,636 |
| 100 | [论文·ReAct：推理与行动协同（LLM Agent 基础）](%E8%AE%BA%E6%96%87%C2%B7ReAct%EF%BC%9A%E6%8E%A8%E7%90%86%E4%B8%8E%E8%A1%8C%E5%8A%A8%E5%8D%8F%E5%90%8C%EF%BC%88LLM%20Agent%20%E5%9F%BA%E7%A1%80%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 101 | [论文·Ring Attention：近乎无限上下文的分块注意力](%E8%AE%BA%E6%96%87%C2%B7Ring%20Attention%EF%BC%9A%E8%BF%91%E4%B9%8E%E6%97%A0%E9%99%90%E4%B8%8A%E4%B8%8B%E6%96%87%E7%9A%84%E5%88%86%E5%9D%97%E6%B3%A8%E6%84%8F%E5%8A%9B.md) | （纯文本条目） | 45,087 |
| 102 | [论文·Scaling Laws for Neural Language Models（OpenAI 缩放律）](%E8%AE%BA%E6%96%87%C2%B7Scaling%20Laws%20for%20Neural%20Language%20Models%EF%BC%88OpenAI%20%E7%BC%A9%E6%94%BE%E5%BE%8B%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 103 | [论文·SmoothQuant：大模型 8-bit 激活量化](%E8%AE%BA%E6%96%87%C2%B7SmoothQuant%EF%BC%9A%E5%A4%A7%E6%A8%A1%E5%9E%8B%208-bit%20%E6%BF%80%E6%B4%BB%E9%87%8F%E5%8C%96.md) | （纯文本条目） | 45,087 |
| 104 | [论文·Titans：用神经长期记忆突破上下文长度限制](%E8%AE%BA%E6%96%87%C2%B7Titans%EF%BC%9A%E7%94%A8%E7%A5%9E%E7%BB%8F%E9%95%BF%E6%9C%9F%E8%AE%B0%E5%BF%86%E7%AA%81%E7%A0%B4%E4%B8%8A%E4%B8%8B%E6%96%87%E9%95%BF%E5%BA%A6%E9%99%90%E5%88%B6.md) | （纯文本条目） | 45,087 |
| 105 | [论文·Toolformer：让语言模型自己学会调用工具](%E8%AE%BA%E6%96%87%C2%B7Toolformer%EF%BC%9A%E8%AE%A9%E8%AF%AD%E8%A8%80%E6%A8%A1%E5%9E%8B%E8%87%AA%E5%B7%B1%E5%AD%A6%E4%BC%9A%E8%B0%83%E7%94%A8%E5%B7%A5%E5%85%B7.md) | （纯文本条目） | 45,087 |
| 106 | [论文·Tree of Thoughts：用树搜索组织思考过程](%E8%AE%BA%E6%96%87%C2%B7Tree%20of%20Thoughts%EF%BC%9A%E7%94%A8%E6%A0%91%E6%90%9C%E7%B4%A2%E7%BB%84%E7%BB%87%E6%80%9D%E8%80%83%E8%BF%87%E7%A8%8B.md) | （纯文本条目） | 45,087 |
| 107 | [论文·ViT：一张图等于 16×16 个词（Vision Transformer）](%E8%AE%BA%E6%96%87%C2%B7ViT%EF%BC%9A%E4%B8%80%E5%BC%A0%E5%9B%BE%E7%AD%89%E4%BA%8E%2016%C3%9716%20%E4%B8%AA%E8%AF%8D%EF%BC%88Vision%20Transformer%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 108 | [论文·投机解码：Transformer 快速推理（Leviathan 等）](%E8%AE%BA%E6%96%87%C2%B7%E6%8A%95%E6%9C%BA%E8%A7%A3%E7%A0%81%EF%BC%9ATransformer%20%E5%BF%AB%E9%80%9F%E6%8E%A8%E7%90%86%EF%BC%88Leviathan%20%E7%AD%89%EF%BC%89.md) | （纯文本条目） | 45,087 |
| 109 | [论文·推理时扩展（test-time compute）优于更大模型](%E8%AE%BA%E6%96%87%C2%B7%E6%8E%A8%E7%90%86%E6%97%B6%E6%89%A9%E5%B1%95%EF%BC%88test-time%20compute%EF%BC%89%E4%BC%98%E4%BA%8E%E6%9B%B4%E5%A4%A7%E6%A8%A1%E5%9E%8B.md) | （纯文本条目） | 45,087 |
