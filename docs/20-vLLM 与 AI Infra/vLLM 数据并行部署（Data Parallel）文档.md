# vLLM 数据并行部署（Data Parallel）文档

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　18,559 字符　｜　521 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/en/latest/serving/data_parallel_deployment

Skip to content

You are viewing the latest developer preview docs. Click here to view docs for the latest stable release.

vLLM

Data Parallel Deployment

Initializing search

GitHub

Home

User Guide

Developer Guide

Benchmarking

API Reference

CLI Reference

Community

vLLM

GitHub

Home

User Guide

User Guide

Getting Started

Getting Started

Quickstart

Installation

Installation

GPU

CPU

TPU

Examples

Examples

Applications

Applications

API Server

Chatbot

Rag

Basic

Basic

Offline Inference

Online Serving

Deployment

Deployment

Async LLM Streaming

Helm Charts

LLM Engine Example

Sagemaker-Entrypoint

Disaggregated

Disaggregated

Disaggregated Encoder

Disaggregated Serving

Ec Both Encoder

Disaggregated Prefill V1

Flexkv Connector

KV Load Failure Recovery Test

LMCache Examples

Mooncake Connector

Mooncake Store

Features

Features

Automatic Prefix Caching

Context Extension

Data Parallel

Kv Events

Logging Configuration

Custom Logits Processors

LoRA

Offline Inference with the OpenAI Batch file format

Profiling

Prompt Embed

Reset Kv

Sharded State

Speculative Decoding

Structured reads on DiffusionGemma

Structured Outputs

Tensorize vLLM Model

Torchrun

Generate

Generate

Batched Chat Completions Online

Multimodal

Qwen 1M Offline

Trace Replay Offline

Observability

Observability

Monitoring Dashboards

Metrics

Setup OpenTelemetry POC

Prometheus and Grafana

Pooling

Pooling

Classify

Embed

Plugin

Reward

Score

Token Classify

Token Embed

Ray Serving

Ray Serving

Batch LLM Inference

Elastic Ep

Multi-Node-Serving

Ray Serve Deepseek

Run Cluster

Reasoning

Reasoning

OpenAI Chat Completion Tool Calls With Reasoning

OpenAI Chat Completion With Reasoning

OpenAI Chat Completion With Reasoning Streaming

OpenAI Responses Client

RL

RL

Batch Invariance

Pause Resume

Rdt vLLM Serve

Rdt Weight Source

RLHF Async New APIs

RLHF Http IPC

RLHF Http IPC No vLLM

RLHF Http NCCL

RLHF Http NCCL No vLLM

RLHF IPC Fsdp Ep

RLHF M2N

RLHF NCCL Fsdp Ep

RLHF NCCL Multi Source

RLHF Sharded Rdt Small Ep

RLHF Sparse NCCL

Routed Experts E2E

Skip Loading Weights In Engine Init

Scale Out

Scale Out

Init

Example Mm Serve

Token Generation Client

Speech To Text

Speech To Text

OpenAI

Realtime

Tool Calling

Tool Calling

Chat With Tools Offline

OpenAI Chat Completion Client With Tools

OpenAI Chat Completion Client With Tools Required

OpenAI Chat Completion Client With Tools Xlam

OpenAI Chat Completion Client With Tools Xlam Streaming

OpenAI Responses Client With Mcp Tools

OpenAI Responses Client With Tools

General

General

vLLM V1

Frequently Asked Questions

Production Metrics

Reproducibility

Security

Troubleshooting

Usage Stats Collection

Inference and Serving

Inference and Serving

Offline Inference

Online Serving

Online Serving

Derenderer APIs

Generative Scoring

OpenAI-Compatible Server

Renderer APIs

Speech to Text APIs

Tokens In <> Tokens Out API

Trace Replay

Context Parallel Deployment

Data Parallel Deployment

Data Parallel Deployment

Table of contents

Internal Load Balancing

Hybrid Load Balancing

External Load Balancing

Troubleshooting distributed deployments

Expert Parallel Deployment

Parallelism and Scaling

Integrations

Integrations

Claude Code

Codex

LangChain

LlamaIndex

Deployment

Deployment

Using Docker

Using Kubernetes

Using Nginx

Frameworks

Frameworks

Anyscale

AnythingLLM

AutoGen

BentoML

Cerebrium

Chatbox

Crusoe

Dify

dstack

Haystack

Helm

Hugging Face Inference Endpoints

LiteLLM

Lobe Chat

LWS

Modal

Nebius Serverless AI

Open WebUI

Retrieval-Augmented Generation

RunPod

SkyPilot

Streamlit

NVIDIA Triton

Integrations

Integrations

AIBrix

NVIDIA Dynamo

k8s-aibom

KAITO

KServe

Kthena

KubeAI

KubeRay

Llama Stack

llm-d

llmaz

Production stack

Training

Training

Async Reinforcement Learning

What is Layerwise (Re)loading?

Prompt Token ID Logprobs (Teacher Scoring)

Reinforcement Learning from Human Feedback

Routed Experts Return (MoE Routing Replay / R3)

Sampling Mask (Distribution Replay)

Transformers Reinforcement Learning

Weight Checker

Weight Transfer

Weight Transfer

Base Classes and Custom Engines

IPC Engine

NCCL M2N Engine

ModelExpress Engine

NCCL Engine

Sharded RDT Engine

Configuration

Configuration

Conserving Memory

Engine Arguments

Environment Variables

Model Resolution

Optimization and Tuning

Server Arguments

TPU

Models

Models

Supported Models

Generative Models

Pooling Models

Pooling Models

Classification Usages

Embedding Usages

Reward Usages

Scoring Usages

Specific Model Examples

Token Classification Usages

Token Embedding Usages

Structured Decisions Models

Extensions

Extensions

Loading model weights with fastsafetensors

Loading Model Weights with InstantTensor

Loading models with Run:ai Model Streamer

Loading models with CoreWeave's Tensorizer

Hardware Supported Models

Hardware Supported Models

CPU - Intel® Xeon®

XPU - Intel® GPUs

TPU

Features

Features

Automatic Prefix Caching

Batch Invariance

Context Extension

Cross-Encoder Output Reuse

Custom Arguments

Custom Logits Processors

Disaggregated Encoder

Disaggregated Prefilling (experimental)

CPU EC Connector Usage Guide

Engram: conditional memory via n-gram lookups

IndexCache

Initialized engine snapshots

Interleaved Thinking

KV Offloading Usage Guide

LoRA Adapters

MooncakeConnector Usage Guide

ECMooncakeConnector Usage Guide

MooncakeStoreConnector Usage Guide

MoRIIOConnector Usage Guide

Multimodal Inputs

NixlConnector Compatibility Matrix

NixlConnector Usage Guide

Per-Request Metrics

Preload

Prompt Embedding Inputs

Reasoning Outputs

Sleep Mode

Structured Outputs

Tool Calling

UMBPStoreConnector Usage Guide

Text watermarking

Quantization

Quantization

AutoAWQ

b12x Linear and MoE Backends

BitsAndBytes

FP8 ViT Encoder Attention

GGUF

GPTQModel

Intel Quantization Support

NVIDIA Model Optimizer

Online Quantization

Quantized KV Cache

AMD Quark

TorchAO

LLM Compressor

LLM Compressor

FP8 W8A8

INT4 W4A16

INT8 W4A8

INT8 W8A8

Speculative Decoding

Speculative Decoding

Per-Request Acceptance Metrics

Adaptive Verification

Draft Models

Dynamic Speculative Decoding

EAGLE Draft Models

Hidden State Extraction

FLy Verification

LiLiCorr

MTP (Multi-Token Prediction)

N-Gram Speculation

vLLM-Project/Speculators

Suffix Decoding

Developer Guide

Developer Guide

General

General

Deprecation Policy

Dockerfile

Editing Agent Instructions

Incremental Compilation Workflow

JIT Kernel Warmup

Labels

Profiling vLLM

Releasing vLLM

Vulnerability Management

Model Implementation

Model Implementation

Basic Model

Registering a Model

Unit Testing

Multi-Modal Support

Speech-to-Text (Transcription/Translation) Support

CI

CI

Dependency Hash Pinning

CI Failures

Nightly Builds of vLLM Wheels

Update PyTorch version on vLLM OSS CI/CD

Design Documents

Design Documents

Plugins

Plugins

Endpoint Plugins

IO Processor Plugins

LoRA Resolver Plugins

Plugin System

Architecture Overview

Attention Backend Feature Support

CUDA Graphs

Vision Encoder (ViT) CUDA Graphs

CustomOp

Dual Batch Overlap

How to debug the vLLM-torch.compile integration

Expert substitution (MoNE)

Fused MoE Modular Kernel

Fusion torch.compile passes

HiSparse local KV offload architecture

Integration with Hugging Face

Hybrid KV Cache Manager

Metrics

Multi-Modal Data Processing

Model Runner V2 Design Document

Fused MoE Kernel Features

Python Multiprocessing

NIXL KV Cache Lease Renewal

NIXL push-mode KV transfer

Optimization Levels

Paged Attention

Automatic Prefix Caching

Octave KV: a 3-bit KV cache for ROCm

Zero-Copy Shared-Memory Tensor Arena for MessageQueue

torch.compile integration

torch.compile with Multimodal Encoders

vLLM IR: Functional Intermediate Representation

Benchmarking

Benchmarking

Benchmark CLI

Parameter Sweeps

Performance Dashboard

API Reference

CLI Reference

CLI Reference

vllm

vllm

chat

complete

download-kernels

preload

run-batch

serve

bench

bench

forward

latency

mm-processor

serve

startup

throughput

sweep

sweep

plot

plot_pareto

serve

serve_workload

startup

launch

launch

render

Community

Community

Contact Us

Meetups

Reviewers

Sponsors

Governance

Governance

Collaboration Policy

Committers

Governance Process

Blog

Forum

Slack

Table of contents

Internal Load Balancing

Hybrid Load Balancing

External Load Balancing

Home

User Guide

Inference and Serving

Data Parallel Deployment ¶

vLLM supports Data Parallel deployment, where model weights are replicated across separate instances/GPUs to process independent batches of requests.

This will work with both dense and MoE models.

For MoE models, particularly those like DeepSeek that employ MLA (Multi-head Latent Attention), it can be advantageous to use data parallel for the attention layers and expert or tensor parallel (EP or TP) for the expert layers.

In these cases, the data parallel ranks are not completely independent. Forward passes must be aligned, and expert layers across all ranks are required to synchronize during every forward pass, even when there are fewer requests to be processed than DP ranks.

By default, expert layers form a tensor parallel group of size DP × TP. To use expert parallelism instead, include the --enable-expert-parallel CLI arg (on all nodes in the multi-node case). See Expert Parallel Deployment for details on how attention and expert layers behave differently with EP enabled.

In vLLM, each DP rank is deployed as a separate "core engine" process that communicates with front-end process(es) via ZMQ sockets. Data Parallel attention can be combined with Tensor Parallel attention, in which case each DP engine owns a number of per-GPU worker processes equal to the configured TP size.

For MoE models, when any requests are in progress in any rank, we must ensure that empty "dummy" forward passes are performed in all ranks that don't currently have any requests scheduled. This is handled via a separate DP Coordinator process that communicates with all ranks, and a collective operation performed every N steps to determine when all ranks become idle and can be paused. When TP is used in conjunction with DP, expert layers form a group of size DP × TP (using either tensor parallelism by default, or expert parallelism if --enable-expert-parallel is set).

In all cases, it is beneficial to load-balance requests between DP ranks. For online deployments, this balancing can be optimized by taking into account the state of each DP engine - in particular its currently scheduled and waiting (queued) requests, and KV cache state. Each DP engine has an independent KV cache, and the benefit of prefix caching can be maximized by directing prompts intelligently.

This document focuses on online deployments (with the API server). DP + EP is also supported for offline usage (via the LLM class), for an example see examples/features/data_parallel/data_parallel_offline.py.

There are two distinct modes supported for online deployments - self-contained with internal load balancing, or externally per-rank process deployment and load balancing.

Internal Load Balancing ¶

vLLM supports "self-contained" data parallel deployments that expose a single API endpoint.

It can be configured by simply including e.g. --data-parallel-size=4 in the vllm serve command line arguments. This will require 4 GPUs. It can be combined with tensor parallel, for example --data-parallel-size=4 --tensor-parallel-size=2, which would require 8 GPUs. When sizing DP deployments, remember that --max-num-seqs applies per DP rank.

By contrast, the admission control limits --max-num-queued-reqs and --max-num-queued-tokens apply to the whole server, not per DP rank: each API server process counts in-flight requests and prefill backlog across all DP ranks it routes to. For example, with --data-parallel-size=4 --max-num-seqs=256 --max-num-queued-reqs=256, the deployment rejects new requests once 256 are in-flight in total, even though ranks could jointly run 1024. To keep ranks saturating, size the cap as roughly data-parallel-size * max-num-seqs plus the desired queue depth.

Running a single data parallel deployment across multiple nodes requires a different vllm serve to be run on each node, specifying which DP ranks should run on that node. In this case, there will still be a single HTTP entrypoint - the API server(s) will run only on one node, but it doesn't necessarily need to be co-located with the DP ranks.

This will run DP=4, TP=2 on a single 8-GPU node:

vllm serve $MODEL --data-parallel-size 4 --tensor-parallel-size 2

This will run DP=4 with DP ranks 0 and 1 on the head node and ranks 2 and 3 on the second node:

# Node 0 (with ip address 10.99.48.128)
vllm serve $MODEL --data-parallel-size 4 --data-parallel-size-local 2 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345
# Node 1
vllm serve $MODEL --headless --data-parallel-size 4 --data-parallel-size-local 2 \
--data-parallel-start-rank 2 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345

This will run DP=4 with only the API server on the first node and all engines on the second node:

# Node 0 (with ip address 10.99.48.128)
vllm serve $MODEL --data-parallel-size 4 --data-parallel-size-local 0 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345
# Node 1
vllm serve $MODEL --headless --data-parallel-size 4 --data-parallel-size-local 4 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345

This DP mode can also be used with Ray by specifying --data-parallel-backend=ray :

vllm serve $MODEL --data-parallel-size 4 --data-parallel-size-local 2 \
--data-parallel-backend = ray

There are several notable differences when using Ray:

A single launch command (on any node) is needed to start all local and remote DP ranks, therefore it is more convenient compared to launching on each node

There is no need to specify --data-parallel-address, and the node where the command is run is used as --data-parallel-address

There is no need to specify --data-parallel-rpc-port

When a single DP group requires multiple nodes, e.g. in case a single model replica needs to run on at least two nodes, make sure to set VLLM_RAY_DP_PACK_STRATEGY="span" in which case --data-parallel-size-local is ignored and will be automatically determined

Remote DP ranks will be allocated based on node resources of the Ray cluster

Currently, the internal DP load balancing is done within the API server process(es) and is based on the running and waiting queues in each of the engines. This could be made more sophisticated in future by incorporating KV cache aware logic.

When deploying large DP sizes using this method, the API server process can become a bottleneck. In this case, the orthogonal --api-server-count command line option can be used to scale this out (for example --api-server-count=4 ). This is transparent to users - a single HTTP endpoint / port is still exposed. Note that this API server scale-out is "internal" and still confined to the "head" node.

Hybrid Load Balancing ¶

Hybrid load balancing sits between the internal and external approaches. Each node runs its own API server(s) that only queue requests to the data-parallel engines colocated on that node. An upstream load balancer (for example, an ingress controller or traffic router) spreads user requests across those per-node endpoints.

Enable this mode with --data-parallel-hybrid-lb while still launching every node with the global data-parallel size. The key differences from internal load balancing are:

You must provide --data-parallel-size-local and --data-parallel-start-rank so each node knows which ranks it owns.

Not compatible with --headless since every node exposes an API endpoint.

Scale --api-server-count per node based on the number of local ranks

In this configuration, each node keeps scheduling decisions local, which reduces cross-node traffic and avoids single node bottlenecks at larger DP sizes.

External Load Balancing ¶

For larger scale deployments especially, it can make sense to handle the orchestration and load balancing of data parallel ranks externally.

In this case, it's more convenient to treat each DP rank like a separate vLLM deployment, with its own endpoint, and have an external router balance HTTP requests between them, making use of appropriate real-time telemetry from each server for routing decisions.

This can already be done trivially for non-MoE models, since each deployed server is fully independent. In that case, launch independent vLLM instances without any --data-parallel-* arguments; external DP CLI options are only supported for MoE deployments.

We support an equivalent topology for MoE DP+EP which can be configured via the following CLI arguments.

If DP ranks are co-located (same node / ip address), a default RPC port is used, but a different HTTP server port must be specified for each rank:

# Rank 0
CUDA_VISIBLE_DEVICES = 0 vllm serve $MODEL --data-parallel-size 2 --data-parallel-rank 0 \
--port 8000
# Rank 1
CUDA_VISIBLE_DEVICES = 1 vllm serve $MODEL --data-parallel-size 2 --data-parallel-rank 1 \
--port 8001

For multi-node cases, the address/port of rank 0 must also be specified:

# Rank 0 (with ip address 10.99.48.128)
vllm serve $MODEL --data-parallel-size 2 --data-parallel-rank 0 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345
# Rank 1
vllm serve $MODEL --data-parallel-size 2 --data-parallel-rank 1 \
--data-parallel-address 10.99.48.128 --data-parallel-rpc-port 13345

When using --data-parallel-external-lb with --nnodes, the data parallel rank
can be inferred from --node-rank only when --nnodes is evenly divisible by
--data-parallel-size. If it is not evenly divisible, automatic rank inference
is rejected and --data-parallel-rank must be set explicitly. When multiple
external-LB ranks are co-located on a node, assign each process a disjoint
device set with CUDA_VISIBLE_DEVICES or --device-ids; devices are not
partitioned automatically between processes.

The coordinator process also runs in this scenario, co-located with the DP rank 0 engine.

In the above diagram, each of the dotted boxes corresponds to a separate launch of vllm serve - these could be separate Kubernetes pods, for example.

September 21, 2026

Back to top

Made with

Material for MkDocs
