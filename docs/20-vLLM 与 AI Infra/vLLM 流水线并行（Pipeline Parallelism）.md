# vLLM 流水线并行（Pipeline Parallelism）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　26,014 字符　｜　515 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/projects/ascend/en/latest/user_guide/feature_guide/pipeline_parallel.html

# https://docs.vllm.ai/projects/ascend/en/latest/user_guide/feature_guide/pipeline_parallel.html

来源: https://docs.vllm.ai/projects/ascend/en/latest/user_guide/feature_guide/pipeline_parallel.html
抓取方式: read_page

Skip to content

You are viewing the latest developer preview docs.
Click here
to view docs for the latest stable release (v0.30.0).

report issue

vLLM Ascend

Pipeline Parallelism

Initializing search

vllm-project/vllm-ascend

Home

Getting Started

User Guide

Developer Guide

Community

vLLM Ascend

vllm-project/vllm-ascend

Home

Getting Started

Getting Started

Overview

Quick Start

Installation

Model Tutorials

Model Tutorials

Qwen3-Dense(0.6B/1.7B/4B/8B/14B/32B)

Qwen-VL-Dense(8B/32B)

Qwen3-30B-A3B

Qwen3-235B-A22B

Qwen3-VL-30B-A3B-Instruct

Qwen3-VL-235B-A22B-Instruct

Qwen3-Coder-30B-A3B

Qwen3-Embedding

Qwen3-VL-Embedding

SigLIP2

Step-5-Preview (Experimental)

Qwen3-Reranker

Qwen3-VL-Reranker

Qwen3-Next

Qwen3-Omni-30B-A3B-Thinking

Qwen3.5-27B & Qwen3.6-27B

Qwen3.5-Dense (2B/4B/9B)

Qwen3.5-397B-A17B

Qwen3.6-35B-A3B

Qwen3.8-2.4T-A95B

Qwen3.8-27B

Qwen3.8-Flash-Next

DeepSeek-V3 & 3.1

DeepSeek-V3.2

DeepSeek-V4-Flash

DeepSeek-V4.1-Flash

DeepSeek-V4-Pro

DeepSeek-R1

DeepSeek-OCR-2

Gemma4

GLM-4.x(4.5/4.6/4.7)

GLM-5 & GLM-5.1

GLM-5.2 & GLM-5.3

GLM-5.3-Flash

Kimi-K2-Thinking

Kimi-K2.5

Kimi-K2.6

Kimi-K3

Kimi K3.1 Pro (Experimental)

PaddleOCR-VL

MiniMax-M2

MiniMax-M3

MiniMax-M3.1-Flash

Hunyuan-A13B-Instruct

Hy3-preview

Hy4-preview (Experimental)

Minitron-8B-Base

LLaVA-OneVision-Qwen2-0.5B-OV

gpt-oss-120b

Mixtral-8x7B-Instruct-v0.1

Qwen3-ASR-1.7B

Cohere Transcribe

Qwen2.5-Math-RM-72B

InternVL3.5(38B/241B-A28B)

Dots3 Note

Feature Tutorials

Feature Tutorials

PD Colocated (Mooncake, Multi-Instance)

PD Disaggregation (Mooncake, Single Node)

PD Disaggregation (Mooncake, Multi Node)

Ray

FAQs

User Guide

User Guide

Deployment Guide

Deployment Guide

Using MindIE Motor

Using Volcano Kthena

Features and Models

Features and Models

Supported Models

Supported Features

Feature Matrix

Configuration

Configuration

NPU Device Selection

Environment Variables

Additional Configuration

Runtime Configuration

Score Encoder Cache Manager

Feature Guide

Feature Guide

Graph Mode

CPU Binding

AI QoS

Quantization

Sleep Mode

Runtime Guard

Structured Output

LoRA

Expert Parallelism Load Balancer

NetLoader

RFork

EPD Disaggregation

KV Cache Pool (Ascend Store)

Mooncake Layerwise Adaptation and Optimization

Mooncake Layerwise Hybrid Attention

Layerwise and Sparse KV Cache Offloading Guide

KVPP

KV Cache CPU Offload

Encoder Cache CPU Offload

Preempt Offload Guide

Large Scale EP

UCM Deployment

Fine-grained TP

Speculative Decoding

Context Parallel

Sequence Parallelism

Batch Invariance

LMCache Ascend Deployment

Pipeline Parallelism

Pipeline Parallelism

Table of contents

Quick Start

Before You Begin

Single-Node Deployment

Two-Node Deployment with Multiprocessing

Multi-Node Deployment with Ray

When to Use PP

Model Support for PP

Configure Layer Partitioning

Automatic Partitioning

Custom Partitioning

Tune Stage Balance

Advanced Scenarios

PP with Speculative Decoding

PP in P/D-Disaggregated Serving

Performance Characteristics: PP Compared with TP

Pipeline Bubbles and Workload Effects

Compatibility and Limitations

Troubleshooting

Related Documentation

Dynamic Chunk Pipeline Parallel

Flash Attention

ShortRequestFirst Prefill Scheduling

Batch-Job-Aware Scheduler

vLLM-Ascend for RL

Release Notes

Developer Guide

Developer Guide

Contribution

Contribution

Testing

Doc Writing

Multi-Node Test

Nightly CI Test

E2E CI Test

Dependency Version Management

Design Documents

Design Documents

Persistent Incremental Csrc Build Cache

Patch

CPU Binding

ModelRunner Prepare Inputs

Disaggregated Prefill

Layerwise and Sparse KV Cache Offloading Design

Model Runner V2 EPLB Architecture

ACL Graph

KV Cache Pool Guide

KVPP Design

Non-Contiguous KV Cache

Add Custom ACLNN Op

Context Parallel

Balance Schedule Refactor

Dynamic Chunked Pipeline Parallel

Quantization

NPUGraph Ex

Evaluation

Evaluation

Using EvalScope

Using lm_eval

Using AISBench

Using OpenCompass

Performance and Debug

Performance and Debug

Performance Benchmark

Optimization and Tuning

Service Profiling Guide

msprobe Guide

Community

Community

Governance

Contributors

Issue Workflow Guidelines

Slash Commands

Versioning Policy

User Stories

User Stories

LLaMA-Factory

Table of contents

Quick Start

Before You Begin

Single-Node Deployment

Two-Node Deployment with Multiprocessing

Multi-Node Deployment with Ray

When to Use PP

Model Support for PP

Configure Layer Partitioning

Automatic Partitioning

Custom Partitioning

Tune Stage Balance

Advanced Scenarios

PP with Speculative Decoding

PP in P/D-Disaggregated Serving

Performance Characteristics: PP Compared with TP

Pipeline Bubbles and Workload Effects

Compatibility and Limitations

Troubleshooting

Related Documentation

Pipeline Parallelism &para;

Pipeline Parallelism (PP) partitions a model's Transformer hidden layers into
consecutive pipeline stages. Each stage is executed by one Tensor Parallel (TP)
group, or by one worker when the TP size is 1, and sends intermediate
activations to the next stage. For example, --pipeline-parallel-size 2
divides the hidden layers between two pipeline stages.

Input tokens → PP stage 0 (earlier hidden layers) → intermediate activations →
PP stage 1 (later hidden layers, final norm, LM head, and sampling) → output
tokens.

This guide uses the following terms:

Term

Meaning

PP stage

A logical partition containing a consecutive range of model layers.

PP rank

A worker's stage index within its PP communication group. Workers with the same stage index and different TP ranks jointly execute one stage.

TP group

The workers that apply TP together within one PP stage.

Node

A physical server. One node can contain multiple PP stages or TP groups.

Note

Not every model implementation supports PP. Before deployment, check the
Pipeline Parallel column in Supported Models.

For a first deployment, begin with Quick Start. Before production, review
Configure Layer Partitioning and the compatibility limitations. Use the
advanced scenarios and performance analysis when the deployment needs
additional features or tuning.

Quick Start &para;

The following examples show the minimum options for starting a basic PP
deployment. Review the topology, compatibility, and performance sections
before using the configuration in production.

Before You Begin &para;

Verify the following before starting the service:

The visible NPU count satisfies the selected TP/PP/DP topology.

All nodes use the same vLLM, vLLM Ascend, model weights, and Python environment.

All nodes can access the same model path or equivalent local copies.

For an MP deployment, every node can reach the head node's master address
and port.

HCCL and, for a Ray deployment, the Ray resource view are healthy.

Single-Node Deployment &para;

The following example demonstrates the minimum PP syntax on two NPUs. Use this
topology only when layer partitioning is required:

export ASCEND_RT_VISIBLE_DEVICES = 0,1

vllm serve /path/to/model \
--tensor-parallel-size 1 \
--pipeline-parallel-size 2 \
--trust-remote-code

Increase --tensor-parallel-size when each stage should use a TP group. For
example, TP4 PP2 requires eight visible NPUs.

Two-Node Deployment with Multiprocessing &para;

The multiprocessing (MP) backend can start a PP deployment across two nodes
without a Ray cluster. The following example uses two 8-NPU nodes, keeps TP
inside each node, and places one PP stage on each node. Replace
<HEAD_NODE_IP> with an address that the worker node can reach and
<MASTER_PORT> with an unused TCP port. Use the same values on both nodes.

On the head node, use node rank 0 :

vllm serve /path/to/model \
--distributed-executor-backend mp \
--tensor-parallel-size 8 \
--pipeline-parallel-size 2 \
--nnodes 2 \
--node-rank 0 \
--master-addr <HEAD_NODE_IP> \
--master-port <MASTER_PORT> \
--trust-remote-code

On the worker node, use node rank 1 and add --headless :

vllm serve /path/to/model \
--distributed-executor-backend mp \
--tensor-parallel-size 8 \
--pipeline-parallel-size 2 \
--nnodes 2 \
--node-rank 1 \
--master-addr <HEAD_NODE_IP> \
--master-port <MASTER_PORT> \
--headless \
--trust-remote-code

Only the head node starts the API server. Both nodes must use identical model,
TP, PP, --nnodes, --master-addr, and --master-port values. Assign each
node a unique --node-rank from 0 to --nnodes - 1, and set the required
HCCL and communication environment variables before running either command.

Multi-Node Deployment with Ray &para;

After starting the Ray cluster, run vllm serve on the head node. For two
8-NPU nodes, keep TP inside each node and use PP between the nodes:

vllm serve /path/to/model \
--distributed-executor-backend ray \
--tensor-parallel-size 8 \
--pipeline-parallel-size 2 \
--trust-remote-code

Start Ray only after setting the required communication environment variables
on every node. The complete cluster setup is documented in
Ray Distributed.

When to Use PP &para;

PP is useful in the following situations:

A model does not fit in one TP group and must be divided further by layer.

A multi-node deployment should keep high-frequency TP collectives inside each
node while sending activations between nodes through PP.

A Prefill node in a P/D-disaggregated deployment needs more devices for model
capacity or long-context prefill.

PP also introduces activation transfers between stages and pipeline bubbles.
Use a TP-only deployment as the baseline when the model fits in one node, then
add PP when model capacity or the network topology requires it.

Deployment goal

Recommended starting point

The model fits in one node

Start with TP only and benchmark before adding PP.

The model spans multiple nodes

Use TP within a node and PP across nodes.

Multiple serving replicas are required

Build one TP/PP replica first, then add DP only on a supported model, operator, and network topology.

Long-context Prefill needs PP tuning

Start with PP, then evaluate Dynamic Chunked Pipeline Parallel.

Model Support for PP &para;

A model can run with --pipeline-parallel-size > 1 only when its
implementation exposes the SupportsPP interface (the upstream marker in
vllm.model_executor.models.interfaces ). The interface declares that the
model implementation can:

Split its Transformer decoder layers into per-stage [start_layer, end_layer) ranges.

Skip layers that fall outside its own range.

Send and receive the intermediate activations exchanged at stage boundaries.

Models without this marker reject PP at startup, so checking support is
mechanical: inspect the model class (or one of its parents) for
SupportsPP, or check the Pipeline Parallel column in
Supported Models for the validated
per-model status on Ascend.

Model families with SupportsPP implementations on Ascend:

Model family

Variants with SupportsPP implementations

DeepSeek-V4

Text, multimodal, and MTP-drafter variants (Ascend implementation)

DeepSeek-V4.1

Text and multimodal variants (Ascend implementation)

GLM-5.2

Text variant (Ascend implementation)

MiniMax-M3

Sparse text and multimodal variants (Ascend implementation)

Qwen3.6

Dense text and MoE variants (upstream implementation)

Kimi K3

Text and multimodal variants (upstream implementation with an Ascend adapter)

All other models use their upstream implementation, and when that
implementation inherits SupportsPP, PP behaves the same way on Ascend.
Note that speculative-decoding drafters are never partitioned across PP
stages even when they carry the SupportsPP marker; they run on the last
stage as described in PP with Speculative Decoding.

Configure Layer Partitioning &para;

Layer partitioning determines which range of target-model hidden layers each PP
stage executes. If the target model has N hidden layers and the PP size is
P, the stage selected by PP rank i uses a half-open interval:

PP rank i: [start_layer_i, end_layer_i)

Only the target model's Transformer hidden layers participate in this
partitioning.

Automatic Partitioning &para;

By default, vLLM distributes hidden layers as evenly as possible. When the layer
count is not divisible by the PP size, vLLM assigns the remaining layers
starting from the second-to-last stage and proceeding toward lower PP ranks.
This avoids adding more hidden layers to the last stage, which normally also
runs the final norm, LM head, logits, and sampling. With more than two stages,
small remainders are placed on middle stages to reduce pressure on the input
and output stages.

For a target model with 61 hidden layers:

PP2: [31, 30]
PP rank 0: [0, 31)
PP rank 1: [31, 61)

PP4: [15, 15, 16, 15]
PP rank 0: [0, 15)
PP rank 1: [15, 30)
PP rank 2: [30, 46)
PP rank 3: [46, 61)

Use automatic partitioning first. Customize it only when measurements show a
stage-level memory or latency imbalance.

Custom Partitioning &para;

Set VLLM_PP_LAYER_PARTITION to the number of target hidden layers executed by
each PP stage, in PP-rank order:

export VLLM_PP_LAYER_PARTITION = "32,29"

vllm serve /path/to/61-layer-model \
--tensor-parallel-size 8 \
--pipeline-parallel-size 2 \
--trust-remote-code

In this example:

PP rank 0: [0, 32)
PP rank 1: [32, 61)

The partition must satisfy all of the following rules:

Use one positive integer per PP rank, separated by commas.

The number of entries must equal --pipeline-parallel-size.

The sum must equal the target model's hidden-layer count.

Do not include embeddings, the final norm, the LM head, or draft-model layers.

Use the same value for every worker in one service instance.

On Ray clusters, set the value on every node before starting Ray.

Restart the service after changing the value.

An invalid entry count or layer sum causes startup to fail.

Tune Stage Balance &para;

Stage balance is workload-dependent. The last stage often needs extra memory
and compute for the final norm, LM head, logits, sampling, and possibly a local
speculative-decoding drafter.

Use the following tuning loop:

Start with automatic partitioning.

Record peak memory and step latency for the workers in every PP stage under
a representative workload.

If the last stage is the bottleneck, move one target hidden layer at a time
from the last stage to a preceding stage with a lower PP rank, for example
from [31,30] to [32,29].

Repeat the accuracy and performance test after every change.

Moving too many layers can merely transfer the bottleneck to another stage, so
do not select a partition using memory capacity alone.

Advanced Scenarios &para;

PP with Speculative Decoding &para;

VLLM_PP_LAYER_PARTITION partitions only the target model. Draft-model layers
are not included in its layer sum, and their weight naming and layer layout
depend on the specific speculative-decoding implementation. On both model
runners the drafter is loaded on the last PP stage rather than partitioned
across PP stages. Therefore, the last stage can contain:

Its target-model hidden layers.

The target model's output-side modules.

The local drafter and its supporting modules.

If the last rank is constrained by memory or latency, reduce its target hidden
layers with a custom partition. draft_tensor_parallel_size controls only the
draft model's TP size; it does not define a draft-model PP partition.

The following matrix lists the features that can be stacked with PP. Support
describes each individual feature combination with PP and remains subject to
the model and hardware requirements in the relevant feature guide.

Feature stacked with PP

Support

Graph mode (ACL graph)

Supported with FULL_DECODE_ONLY and PIECEWISE; FULL_DECODE_ONLY is validated with MTP stacks.

MTP

Supported.

EAGLE3

Supported only for MiniMax-M3 sparse models; the upstream restriction is lifted for these architectures.

DFlash

Not supported.

DSpark

Supported only for DeepSeek-V4 and GLM-5.2 MoE-DSA models. The drafter taps intermediate target layers, and the tapped hidden states are relayed across PP stages.

PCP (Prefill Context Parallel)

Supported.

DCP (Decode Context Parallel)

Supported; use MooncakeConnectorV2 for KV transfer.

LayerSplit (KVPP)

Supported with eager or PIECEWISE execution; full graph modes cannot be combined with KVPP. An intermittent output-mismatch issue in the e2e test is under investigation. See the KVPP guide.

KV offloading (Sparse KV Cache Offload)

Supported with the layerwise connector.

Prefix caching

Supported.

Pooling

Supported.

KV Cache Pool

Supported.

Lmhead TP

Supported.

MLAPO

Supported.

Multimodal inputs (mm)

Supported.

Multistream MoE

Supported.

Shared Expert DP

Supported.

Quantization

Supported for all quantization methods available for the model and hardware, including W4A4, W4A8, and W8A8.

Weight NZ

Supported.

P/D disaggregation

Supported with PP on the Prefill node only; the Decode node must use PP1.

Async scheduling

Supported.

Sequence parallelism

Supported.

Notes for speculative decoding with PP:

DSpark drafts from taps on intermediate target layers
( dspark_target_layer_ids ). Taps that fall on an earlier PP stage are
relayed to the drafter on the last stage automatically. When customizing
VLLM_PP_LAYER_PARTITION, keep the tap list inside the target model so the
drafter's input width stays valid.

Speculative-decoding support varies by model, method, and model runner. See
Speculative Decoding and the relevant model tutorial
before enabling it with PP.

PP in P/D-Disaggregated Serving &para;

PP is commonly enabled on the Prefill node, while the Decode node remains
PP1. When using MooncakeConnectorV1, both the Prefill and Decode
configurations must describe the actual Prefill topology.

Setting

Prefill node

Decode node

--pipeline-parallel-size

Actual Prefill PP size, for example 2.

Currently 1.

prefill.pp_size

Actual Prefill PP size.

The same Prefill PP size.

prefill.pp_layer_partition

Custom Prefill partition, if used.

The same custom Prefill partition.

decode.pp_size

1

1

VLLM_PP_LAYER_PARTITION

Set when the Prefill node uses a custom partition.

Do not copy the Prefill value to a PP1 Decode process.

For a 61-layer Prefill model using TP8 PP2 and the custom partition
[32,29], include the following topology in kv_connector_extra_config on
both sides:

{
"prefill" : {
"dp_size" : 1,
"tp_size" : 8,
"pp_size" : 2,
"pp_layer_partition" : "32,29"
},
"decode" : {
"dp_size" : 1,
"tp_size" : 1,
"pp_size" : 1
}
}

The Prefill process must also export:

export VLLM_PP_LAYER_PARTITION = "32,29"

If automatic partitioning is used, omit pp_layer_partition on both sides.
The current Mooncake connector requires the Prefill TP size to be greater than
or equal to the Decode TP size and to be an integer multiple of it.

For complete P/D startup commands, see
PD Disaggregation with Mooncake.
For long-context Prefill optimization, see
Dynamic Chunked Pipeline Parallel.

Performance Characteristics: PP Compared with TP &para;

PP and TP distribute model execution in different ways, so neither strategy is
universally faster. TP partitions supported operations within Transformer
layers. TP workers participate in every layer and use collective communication
to combine partial results, while components that are not partitioned can
remain replicated. PP assigns consecutive layers to different stages and
transfers intermediate tensors between adjacent stages. In a hybrid topology,
TP communication remains inside each PP stage, while PP communication connects
the stages.

The primary performance advantage of PP over a large cross-node TP group is
communication locality, rather than faster computation within an individual
layer.

Aspect

Tensor Parallelism

Pipeline Parallelism

Performance implication

Model partition

Every TP rank participates in every layer and holds either a shard or a replica of each component.

Each PP stage executes only its assigned layers; TP can additionally shard those layers.

PP can extend model capacity without creating a very large TP group.

Communication

Collective operations can occur inside many Transformer layers.

Intermediate tensors are transferred between adjacent PP stages.

Keeping TP inside each node usually reduces the frequency of cross-node communication.

Single-request latency

All TP workers contribute to the same layer concurrently.

A request traverses the PP stages in sequence.

TP usually favors latency-sensitive, low-concurrency workloads when the TP interconnect is fast.

Steady-state throughput

A large TP group can lose efficiency to collective latency and smaller per-rank matrix operations.

Concurrent scheduler batches formed from independent requests or Prefill chunks can keep different stages occupied.

PP can improve throughput when the pipeline has enough work and the stages are balanced.

Memory

Weights are sharded within each layer; some tensors can remain replicated.

Weights and KV cache are divided by layer, while edge stages also hold input-side or output-side modules.

Both reduce per-rank memory, but equal parallel sizes do not guarantee equal peak memory.

Scaling constraints

The usable TP size can be limited by attention heads, KV heads, hidden dimensions, expert routing, or supported kernels.

The model must support PP and have partitionable layers.

PP is useful when increasing TP is invalid or no longer efficient.

Main bottleneck

A slow collective delays every rank in the TP group.

The slowest stage and its boundary transfer limit the pipeline.

TP depends on collective efficiency; PP depends on stage balance and activation-transfer efficiency.

Pipeline Bubbles and Workload Effects &para;

PP introduces idle time while work enters and leaves the pipeline, and whenever
one stage takes longer than the others. This idle time is commonly called a
pipeline bubble. Equal layer counts do not guarantee equal stage latency: the
first and last stages can also execute embeddings, the final norm, LM head,
logits processing, sampling, or a speculative-decoding drafter.

Workload characteristic

Expected behavior

One request or low-concurrency Decode

There is little independent work to occupy multiple stages. Stage traversal and communication commonly increase latency relative to an efficient node-local TP baseline. Against a TP group that spans nodes, measure both configurations because cross-node collective latency can reverse the result.

High concurrency with continuous batching

Concurrent scheduler batches can occupy different stages, reducing the relative pipeline bubble and improving throughput.

Long-context Prefill

More computation per chunk can amortize PP communication, and PP can provide the memory capacity required by long contexts.

Variable prompt lengths

Batch and chunk durations vary, so a fixed-length benchmark can hide stage imbalance.

Unbalanced layer partition

The slowest stage makes the other stages wait and limits steady-state throughput.

PP is therefore more likely to outperform cross-node TP when TP can remain
inside each node, the stages can be balanced by measured latency and memory,
and the workload provides enough concurrent requests or Prefill chunks to keep
the pipeline occupied. TP remains the preferred baseline when the model fits in
one node, low-concurrency latency is the primary objective, or the stages cannot
be balanced.

For long-context Prefill workloads, consider
Dynamic Chunked Pipeline Parallel, which
adjusts chunk sizes using measured execution time to reduce stage idle time.

Compatibility and Limitations &para;

Xlite graph mode is not compatible with PP.

Sparse KV Cache Offload supports PP with the layerwise connector.

MooncakeConnectorV1 currently requires the Decode-side PP size to be 1.

For cross-node MoE deployments over RoCE, PP and DP cannot currently be
enabled together because the MoeDistributeDispatch communication path does
not support this combined topology.

Feature combinations remain model-specific. Check the relevant feature guide
before production deployment.

Troubleshooting &para;

Symptom

Checks and actions

Startup reports insufficient resources

Confirm that visible NPUs satisfy the selected DP/PP/TP topology and that Ray sees every expected NPU.

Startup rejects VLLM_PP_LAYER_PARTITION

Check that the entry count equals the PP size and that the sum equals the target hidden-layer count.

One PP rank runs out of memory

Check whether it is the last stage or hosts a local drafter. Measure all ranks, then move one target layer at a time.

The first request hangs

Retry with smaller sequence and batch limits. Use --enforce-eager to determine whether graph capture is involved, and verify inter-rank communication.

An MP worker cannot join the head node

Verify that both nodes use the same master address, master port, node count, model, and parallel sizes; that each node rank is unique; and that the master port is reachable. The worker command must include --headless.

A Ray deployment hangs or misses ranks

Verify identical environments, model paths, communication variables, and Ray resources on every node.

A cross-node RoCE MoE deployment fails or hangs after enabling PP and DP

Keep either the PP size or the DP size at 1; the current MoeDistributeDispatch path does not support this combined topology.

P/D handshake times out

Compare the P/D connector topology, prefill.pp_size, optional prefill.pp_layer_partition, and the available kv_port range.

Related Documentation &para;

Supported Models

Ray Distributed

Dynamic Chunked Pipeline Parallel

Speculative Decoding

PD Disaggregation with Mooncake

Performance Benchmark

Service Profiling Guide

November 9, 2026

Back to top
