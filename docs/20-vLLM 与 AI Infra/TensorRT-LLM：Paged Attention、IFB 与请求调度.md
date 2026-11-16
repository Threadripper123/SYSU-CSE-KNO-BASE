# TensorRT-LLM：Paged Attention、IFB 与请求调度

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　19,986 字符　｜　312 个片段　｜　更新于 2026-11-13
来源: https://nvidia.github.io/TensorRT-LLM/features/paged-attention-ifb-scheduler.html

# https://nvidia.github.io/TensorRT-LLM/features/paged-attention-ifb-scheduler.html

来源: https://nvidia.github.io/TensorRT-LLM/features/paged-attention-ifb-scheduler.html
抓取方式: read_page
实际URL: https://nvidia.github.io/TensorRT-LLM/features/paged-attention-ifb-scheduler.html

Skip to main content

Back to top

Ctrl + K

TensorRT LLM

Choose version

Search
Ctrl + K

System Settings

Light

Dark

Search
Ctrl + K

TensorRT LLM

Choose version

System Settings

Light

Dark

Table of Contents

Getting Started

Overview

Quick Start Guide

Installation
Installation Guide

Build from Source

Container Images

Supported Hardware

Deployment Guide

LLM Examples
Generate text

Generate text asynchronously

Generate text in streaming

Distributed LLM Generation

Generate text with guided decoding

Control generated text using logits processor

Generate text with multiple LoRA adapters

Sparse Attention

Speculative Decoding

KV Cache Compression

KV Cache Connector

KV Cache Connector under VSWA

KV Cache Offloading

Runtime Configuration Examples

Sampling Techniques Showcase

Attention-DP KV Cache Connector

LMCache KV Cache Connector

Run LLM-API with pytorch backend on Slurm

Run trtllm-bench with pytorch backend on Slurm

Run trtllm-serve with pytorch backend on Slurm

VisualGen Examples
API walkthrough

Online Serving Examples
Aiperf Client

Aiperf Client For Multimodal

Curl Chat Client

Curl Chat Client For Multimodal

Curl Completion Client

Curl Responses Client

Deepseek R1 Reasoning Parser

OpenAI Chat Client

OpenAI Chat Client for Multimodal

OpenAI Completion Client

Openai Completion Client For Lora

OpenAI Completion Client with JSON Schema

OpenAI Responses Client

Prometheus Metrics

Dynamo K8s Example

Model Recipes
Deployment Guide for Nemotron v3 (Ultra & Super) on TensorRT LLM - Blackwell & Hopper Hardware

Deployment Guide for GPT-OSS on TensorRT-LLM - Blackwell Hardware

Deployment Guide for Qwen3 on TensorRT LLM - Blackwell & Hopper Hardware

Deployment Guide for Qwen3.8 MoE and Qwen3.5 MoE on TensorRT LLM - Blackwell Hardware

Deployment Guide for Qwen3.8-Flash-Next on TensorRT LLM - Blackwell Hardware

Deployment Guide for Kimi K3 on TensorRT LLM - Blackwell

Deployment Guide for GLM-5 on TensorRT LLM - Blackwell Hardware

Deployment Guide for GLM-5.3-Flash on TensorRT LLM - Blackwell Hardware

Deployment Guide for MiniMax-M3 on TensorRT LLM

CPU Affinity configuration in TensorRT LLM

Models

Supported Models

Visual Generation (Beta)

Adding a New Model

CLI Reference

trtllm-bench

trtllm-eval

trtllm-serve
trtllm-serve

Render endpoints

Run benchmarking with trtllm-serve

API Reference

LLM API Introduction

API Reference
LLM

AsyncLLM

ExecutorMemoryType

RuntimeMemoryStatus

SleepConfig

MultimodalEncoder

CompletionOutput

RequestOutput

GuidedDecodingParams

SamplingParams

KvHintAction

KvHintsEnvelope

KvHintsInput

DisaggregatedParams

ConversationParams

DisaggScheduleStyle

BlockReuseConfig

KvCacheConfig

KVEventsConfig

MambaStateConfig

KvCacheRetentionConfig

CudaGraphConfig

DecodeCudaGraphConfig

EncodeCudaGraphConfig

EncodeExtraInputSpec

MoeConfig

EagleDecodingConfig

Eagle3DecodingConfig

MTPDecodingConfig

SchedulerConfig

CapacitySchedulerPolicy

QuantConfig

QuantAlgo

CalibConfig

RequestError

MpiCommSession

ContextChunkingPolicy

DynamicBatchConfig

CacheTransceiverConfig

NGramDecodingConfig

PARDDecodingConfig

DFlashDecodingConfig

DSparkDecodingConfig

SADecodingConfig

SAEnhancerConfig

UserProvidedDecodingConfig

TorchCompileConfig

DraftTargetDecodingConfig

LlmArgs

TorchLlmArgs

AutoDecodingConfig

AttentionDpConfig

LoRARequest

SaveHiddenStatesDecodingConfig

RocketSparseAttentionConfig

QSASparseAttentionConfig

ReorderRequestPolicyConfig

DeepSeekSparseAttentionConfig

DeepSeekV4SparseAttentionConfig

MiniMaxM3SparseAttentionConfig

SchedulingParams

SkipSoftmaxAttentionConfig

ColdPageQuantizationCompressionConfig

TriAttentionKvCacheCompressionConfig

PrometheusMetricsConfig

PrefillCudaGraphBackend

ThinkingBudgetLogitsProcessor

add_thinking_budget_logits_processor

MultimodalConfig

Features

Feature Combination Matrix

Multi-Head, Multi-Query, and Group-Query Attention

Disaggregated Serving

Sub-agent Routing

Embeddings (Encoder-Only Models)

KV Cache System

KV hint request metadata

KV Cache Compression

Long Sequences

LoRA (Low-Rank Adaptation)

Multimodal Support in TensorRT LLM

Overlap Scheduler

Paged Attention, IFB, and Request Scheduling

Parallelism in TensorRT LLM

Quantization

Sampling

Additional Outputs

Post-Processing Hook

Guided Decoding

Speculative Decoding

Checkpoint Loading

ModelExpress (MX) Checkpoint Loading

Runtime Memory Checkpointing (Prototype)

Ray Orchestrator (Prototype)

Torch Compile & Prefill CUDA Graph

Helix Parallelism

KV Cache Connector

Sparse Attention

Visual Generation (Beta)
CUDA Graphs

Quantized Attention

Sparse Attention

Prefix-Tokenization Cache

Developer Guide

Architecture Overview

Multimodal Request Data

Performance Analysis

TensorRT LLM Benchmarking

MoE Performance Guide
MoE Perf Dashboard

Continuous Integration Overview

Using Dev Containers

LLM API Change Guide

Introduction to KV Cache Transmission

KVCacheManagerV2 Cold-Page Codec Design

KV Cache Compression Development Guide

Sparse Attention Development Guide

Telemetry

Blogs

KV Cache Compression in TensorRT LLM

GVR V2: Self-Sampling and Multi-Thresholding for Faster Exact Top-K

Accelerating Video Generation with GEMM Quantization, Attention Quantization and Skip Softmax Attention in TensorRT-LLM

Evaluating Agentic Serving with Trace Replay and Job-Level Metrics

DeepSeek-V4 on NVIDIA Blackwell: Model-Specific and Agentic-Workload Optimizations in TensorRT LLM

Scaling Video Generation Across NVL72 rack with TensorRT-LLM

MoE as Dense GEMM: Optimizing Low-Latency MoE Inference on NVIDIA Blackwell

Joint Optimization of Agent Applications and TensorRT-LLM

Helix Parallelism in TensorRT-LLM: Scaling Multi-Million-Token Decoding with KV Cache Sharding

Temporal Correlation Meets Sparse Attention: Guess-Verify-Refine Top-K for Blackwell

Tuning CUDA Graph Batch Sizes for Higher Output Throughput

DWDP: Distributed Weight Data Parallelism for High-Performance LLM Inference on NVL72

Optimizing MoE Communication with One-Sided AlltoAll Over NVLink

Sparse Attention in TensorRT LLM

Accelerating Long-Context Inference with Skip Softmax Attention

Optimizing DeepSeek-V3.2 on NVIDIA Blackwell GPUs

Scaling Expert Parallelism in TensorRT LLM (Part 3: Pushing the Performance Boundary)

Inference Time Compute Implementation in TensorRT LLM

Combining Guided Decoding and Speculative Decoding: Making CPU and GPU Cooperate Seamlessly

Running GPT-OSS-120B with Eagle3 Speculative Decoding on GB200/B200 (TensorRT LLM)

ADP Balance Strategy

Running a High Performance GPT-OSS-120B Inference Server with TensorRT LLM

Scaling Expert Parallelism in TensorRT LLM (Part 2: Performance Status and Optimization)

N-Gram Speculative Decoding in TensorRT LLM

How to launch Llama4 Maverick + Eagle3 TensorRT LLM server

Disaggregated Serving in TensorRT LLM

Scaling Expert Parallelism in TensorRT LLM (Part 1: Design and Implementation of Large-scale EP)

Optimizing DeepSeek R1 Throughput on NVIDIA Blackwell GPUs: A Deep Dive for Developers

DeepSeek R1 MTP Implementation and Optimization

Pushing Latency Boundaries: Optimizing DeepSeek-R1 Performance on NVIDIA B200 GPUs

How to get best performance on DeepSeek-R1 in TensorRT LLM

H200 achieves nearly 12,000 tokens/sec on Llama2-13B with TensorRT LLM

New XQA-kernel provides 2.4x more Llama-70B throughput within the same latency budget

H100 has 4.6x A100 Performance in TensorRT LLM, achieving 10,000 tok/s at 100ms to first token

Falcon-180B on a single H200 GPU with INT4 AWQ, and 6.7x faster Llama-70B over A100

Speed up inference with SOTA quantization techniques in TRT-LLM

Quick Links

Releases

Github Code

Roadmap

Migration

Migration Guide: TensorRT Backend Removed

Paged Attention, IFB, and Request Scheduling

Paged Attention, IFB, and Request Scheduling #

In-flight Batching #

TensorRT LLM supports in-flight batching of requests (also known as continuous
batching or iteration-level batching) for higher serving throughput. With this feature,
sequences in the context phase can be processed together with sequences in the
generation phase. The purpose of that technique is to better interleave
requests to reduce latency as well as make better use of the GPUs.
For efficiency reasons (1), the support for inflight batching requires the
input tensors to be packed (no padding).

In the current implementation, the sequences that are going through the
context phase must come before the sequences in the generation phase in the input
tensor. For example, for sequences S0, S1 and S2, if S0 and S2 are in
context phase (and S1 in generation), tokens from S0 and S2 must appear
before the tokens of S1 in the input tensor. The constraint may or may not
be relaxed in a future version.

_(1) Padding sequences in the generation phase that contain a single token to
the length of the maximum input sequence is inefficient use of resources.

max_batch_size, max_seq_len and max_num_tokens #

max_batch_size #

max_batch_size defines the maximum number of requests that the engine can handle.​

It controls the maximum number of requests that can be scheduled at runtime.

Set a sufficiently high max_batch_size when building the engine so that it does not become the bottleneck of the throughput, and use runtime max_batch_size to tune throughput or latency without rebuilding the engine.

max_seq_len #

max_seq_len defines the maximum sequence length of a single request.

Starting from TensorRT LLM v0.11, when --remove_input_padding and --context_fmha are enabled, max_seq_len can replace max_input_len and max_output_len, and is set to max_position_embeddings by default.

Use default max_seq_len (which is max_position_embeddings ), no need to tune it unless you are very sure what max sequence lengths would be on your workloads. If GPU memory is so limited that it cannot support even one request reaching max_seq_len, you need to reduce it.

max_num_tokens #

max_num_tokens defines the maximum number of batched input tokens after padding is removed in each batch.​

max_num_tokens is set to 8192 by default starting from v0.11. It is recommended to tune –max_num_tokens for optimal performance.

The maximum number of tokens will not take effect when input padding is not removed. When input padding is removed, the tokens from different sequences are
packed together and the maximum number of the tokens can be set to a different
(lower) value, which by default is 8192.

There are two aspects that must be considered. Firstly, some input sequences
will be shorter than the maximum input length. Secondly, when in-flight
sequence batching is enabled, requests in context phase will be executed with
requests in generation phase. Those latter requests produce a lot fewer tokens
than max_input_len (at most, beam_width tokens).

max_num_tokens affects workspace buffer sizes to be allocated as well as one of the matrix multiplication dimensions. Hence, using a more realistic value for max_num_tokens allows TensorRT LLM to
allocate more memory to store the KV cache and execute more requests together.
It leads to increased efficiency.

GPUs yield higher utilization with larger matrix multiplications. Hence, increasing max_num_tokens appropriately will be beneficial to performance. At some point, GPU utilization will plateau,
going beyond that saturation point may hurt both first token latency as well as
total end-to-end latency. In summary, one should select reasonably high max_num_tokens for high token throughput/good GPU math utilization but not very high in order to meet SLO TTFT (time to first token) and TPOT (time per output token).

Chunked Context (a.k.a Chunked Prefill) #

The original behavior was to process all context tokens at once. However, this feature splits the context into several chunks. In this way, the
context chunks can be batched with more tokens during the generation phase,
which should increase overall throughput. Chunking contexts also removes
constraints on input length. To enable this feature, the FMHA paged kv-cache also
needs to be enabled. Except for the last chunk, the size of each context chunk needs to be an integer multiple of the kv-cache block size.

KV Cache #

In the generation phase, a common optimization is to provide the MHA kernel
with a cache containing the values of the past K and V elements that have
already been computed. That cache is known as the KV cache. TensorRT LLM uses
that technique to accelerate its generation phase. In TensorRT LLM, there is
one KV cache per Transformer layer, which means that there are as many KV
caches as layers in a model. The current version of TensorRT LLM supports two
different types of KV caches: contiguous and paged KV caches.

Contiguous KV Cache #

The contiguous KV cache is a monolithic tensor. Its shape is:

[ max_batch_size * max_beam_width, 2, num_heads, max_seqlen, hidden_dim_per_head ].

This implementation uses much more memory than needed when sequences are shorter than the maximum sequence length. Even if they approach the limit after generating many output tokens, it may take many steps to reach that point.

Paged KV Cache #

The paged KV cache decomposes the KV cache into blocks that are distributed to
the different requests by a cache manager during processing. That cache manager
keeps track of the sequences, allocates new blocks from a pool, and recycles those blocks when required.
The PyTorch executor integrates the cache managers through
KVCacheManager and
KVCacheManagerV2.
The V1 KVCacheManager wraps the C++ block manager under the
Batch Manager.
See Selecting the KV Cache Manager
for configuration and compatibility details.

The schedulers #

This section visualizes how TensorRT LLM schedules requests based on max-batch size and max-num tokens. The example starts out with a newly initialized engine as well as a few unscheduled requests that have come in. For the sake of this example, toy values are set to max batch size = 4 and max num tokens = 12. Each square block represents a token, and its color represents which request it belongs to.

Now the scheduler takes the first two requests, Request 1 and Request 2, and schedules them to execute the context phase. However, it cannot schedule any more requests because the prompts of the first two requests had 5 tokens each, leaving a budget of 2 tokens due to the max num tokens limit. Since all remaining requests have more than 2 prompt tokens none of them can be scheduled (context chunking can help in this situation, see the paged context attention section below). The tokens are marked with a “C” on them to represent that they are prompt tokens that were processed in the context phase.

Note: The tokens for different requests are shown on different rows simply for visualization purposes and are not representative of actual memory layouts

Now the engine runs an iteration of execution, completing the context phases for both of the scheduled requests. After it is done, the kv-cache of the prompts for both requests have been created and the first token has been generated. Tokens that were generated are marked with “G(n)” - for example a token marked “G1” represents that it is the first token generated for its request.

TRT-LLM prioritizes scheduling requests in generation phase first so the two generated tokens are queued to be processed in the next iteration. Now, since the two previously scheduled requests have entered generation phase and only take up two tokens out of the max num token budget of 12, the scheduler is able to schedule two additional requests, Request 3 and Request 4. It cannot schedule the last request, Request 5, even though there is space for it in the max num tokens budget because of the max batch size limit of 4.

After the next iteration of execution, the second tokens for Requests 1 and 2 have been generated, and the first tokens for Request 3 and 4 have been generated. Let’s say that G2, which was generated for Request 1, is the stop token signifying that Request 1 is completed. In this case the scheduler would evict Request 1 before performing another execution iteration and prepare to return it to the user. This eviction puts the state of the engine below the max batch size limit and allows Request 5 to be scheduled.

Also note that G1, which was generated for Request 2, has been added to the kv-cache for Request 2, illustrating how the kv-cache for a request grows as more tokens are generated.

Overall, the max batch size and max num tokens limits play a key role in determining when requests are executed. Tuning these parameters can significantly impact throughput, as well as how the engine balances previously scheduled requests in the generation phase with new requests in the context phase.

Note: This presents a simplified visualization of the scheduler to highlight how max batch size and max num tokens affect it. The scheduler also considers things like amount of free memory available to be used for kv-cache and has other configurable options that can affect its behavior. See the Runtime Flags of the Additional Options page for more on this.

Revisiting Paged Context Attention and Context Chunking #

Previously we recommended enabling paged context attention even though in our case study it didn’t affect performance significantly. Now that we understand the TensorRT LLM scheduler, we can explain why this is beneficial. In short, we recommend enabling it because it enables context chunking, which allows the context phase of a request to be broken up into pieces and processed over several execution iterations, allowing the engine to provide a more stable balance of context and generation phase execution. Enabling chunking results in lower TTFTs on average across requests but it may increase TTFT for small number of “lucky” requests that would have been processed in a single iteration in absence of chunking.

The visualization of the TensorRT LLM scheduler showed that initially Request 3 couldn’t be scheduled because it would put the scheduler over the max-num tokens limit. However, with context chunking, this is no longer the case, and the first chunk of Request 3 can be scheduled.

This is extremely beneficial for several reasons. First, it eliminates the possibility of requests with large prompts (relative to max num tokens) not being scheduled due to other requests already in-flight. In production workloads, this can mitigate queuing effects and worst case TTFT numbers. Second, it allows for setting smaller values of max num tokens, since you no longer need max num tokens to be at least as large as the longest prompt you want to support. For long-context cases this is extremely important, because setting extremely large values of max-num tokens takes away from memory available to be used as kv-cache. Given that, in the worst-case scenario, chunked context has minimal impact on performance but can significantly benefit it in many situations, NVIDIA recommends that you always enable it.

previous

Overlap Scheduler

next

Parallelism in TensorRT LLM

On this page

In-flight Batching
max_batch_size, max_seq_len and max_num_tokens
max_batch_size

max_seq_len

max_num_tokens

Chunked Context (a.k.a Chunked Prefill)

KV Cache
Contiguous KV Cache

Paged KV Cache

The schedulers

Revisiting Paged Context Attention and Context Chunking

Privacy Policy
|

Your Privacy Choices
|

Terms of Service
|

Accessibility
|

Corporate Policies
|

Product Security
|

Contact

Copyright © 2025, NVidia.

Last updated on November 03, 2026.

This page is generated by TensorRT-LLM commit 9e334fb.
