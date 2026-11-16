# vLLM Fused MoE Kernel 特性

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　17,482 字符　｜　685 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/en/latest/design/moe_kernel_features

Skip to content

You are viewing the latest developer preview docs. Click here to view docs for the latest stable release.

vLLM

Fused MoE Kernel Features

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

Fused MoE Kernel Features

Table of contents

Fused MoE Modular All2All backends

Fused Experts Kernels

Modular Kernel "families"

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

Fused MoE Modular All2All backends

Fused Experts Kernels

Modular Kernel "families"

Home

Developer Guide

Design Documents

Fused MoE Kernel Features ¶

The purpose of this document is to provide an overview of the various MoE kernels (both modular and non-modular) so it will be easier to select an appropriate set of kernels for any particular situation. This includes information about the all2all backends used by modular kernels.

Fused MoE Modular All2All backends ¶

There are a number of all2all communication backends that are used to implement expert parallelism (EP) for the MoERunner layer. The different FusedMoEPrepareAndFinalizeModular subclasses provide an interface for each all2all backend.

The following table describes the relevant features of each backend, i.e. activation format, supported quantization schemes and async support.

The output activation format (standard or batched) corresponds to the output of the prepare step of the FusedMoEPrepareAndFinalizeModular subclass, and the finalize step requires the same format. All the backend prepare methods expect activations in the standard format and all the finalize methods return activations in standard format. More details on the formats can be found in the Fused MoE Modular Kernel document.

The quantization types and formats enumerate which quantization schemes are supported by each FusedMoEPrepareAndFinalizeModular class. The quantization can happen before or after the dispatch based on the format the all2all backend supports, e.g. deepep_high_throughput supports only block-quantized fp8 format. Any other format will result in dispatching in higher precision and quantizing afterwards. The output of the prepare step for each backend is the quantized type. The finalize step generally requires the same input type as the original activations, e.g. if the original input is bfloat16 and the quantization scheme is fp8 with per-tensor scales, prepare will return fp8/per-tensor scale activations and finalize will take bfloat16 activations. See the diagrams in Fused MoE Modular Kernel for more details on the types and formats of activations at each step of the MoE process. If no quantization type is specified, the kernel operates on float16 and/or bfloat16.

Async backends support the use of DBO (Dual Batch Overlap) and shared expert overlap (where shared experts are computed during the combine step).

Certain models require the topk weights to be applied to the input activations rather than the output activations when topk==1, e.g. Llama. For modular kernels, this feature is supported by the FusedMoEPrepareAndFinalizeModular subclass. For non-modular kernels, it is up to the experts function to deal with this flag.

Unless otherwise specified, backends are controlled via the --all2all-backend command-line argument (or the all2all_backend parameter in ParallelConfig ). All backends except flashinfer only work with EP+DP or EP+TP. Flashinfer can work with EP or DP without EP.

Backend

Output act. format

Quant. types

Quant. format

Async

Apply Weight On Input

Subclass

naive

standard

all 1

G,A,T

N

6

MoERunner

deepep_high_throughput

standard

fp8

G(128),A,T 2

Y

Y

DeepEPHTPrepareAndFinalize

deepep_low_latency

batched

fp8

G(128),A,T 3

Y

Y

DeepEPLLPrepareAndFinalize

flashinfer_nvlink_two_sided

standard

nvfp4,fp8

G,A,T

N

N

FlashInferNVLinkTwoSidedPrepareAndFinalize

flashinfer_nvlink_one_sided

standard

nvfp4,bf16,mxfp8,fp8

G,A,T

N

N

FlashInferNVLinkOneSidedPrepareAndFinalize

passthrough

standard

nvfp4,mxfp4

G

N

Y

PassThroughPrepareAndFinalize

Table key

All types: mxfp4, nvfp4, int4, int8, fp8

A,T quantization occurs after dispatch.

All quantization happens after dispatch.

Controlled by --moe-backend ( flashinfer_cutlass or flashinfer_trtllm )

This is a no-op dispatcher that can be used to pair with any modular experts to produce a modular kernel that runs without dispatch or combine. These cannot be selected via environment variable. These are generally use for testing or adapting an expert subclass to the fused_experts API.

This depends on the experts implementation.

G - Grouped

G(N) - Grouped w/block size N

A - Per activation token

T - Per tensor

Modular kernels are supported by the following FusedMoEMethodBase classes.

ModelOptFp8MoEMethod

Fp8MoEMethod

CompressedTensorsW4A4Nvfp4MoEMethod

CompressedTensorsW8A8Fp8MoEMethod

GptOssMxfp4MoEMethod

UnquantizedFusedMoEMethod

Fused Experts Kernels ¶

There are a number of MoE experts kernel implementations for different quantization types and architectures. Most follow the general API of the base Triton fused_experts function. Many have modular kernel adapters, so they can be used with compatible all2all backends. This table lists each experts kernel and its particular properties.

Each kernel must be provided with one of the supported input activation formats. Some flavors of kernels support both standard and batched formats through different entry points, e.g. TritonExperts and BatchedTritonExperts. Batched format kernels are currently only needed for matching with certain all2all backends, e.g. DeepEPLLPrepareAndFinalize.

Similar to the backend kernels, each experts kernel only supports certain quantization formats. For non-modular experts, the activations will be in the original type and quantized internally by the kernel. Modular experts will expect the activations to already be in the quantized format. Both types of experts will yield outputs in the original activation type.

Each experts kernel supports one or more activation functions, e.g. silu or gelu, which are applied to the intermediate results.

As with the backends, some experts support applying topk weights on the input activations. The entries in the column in this table only apply to the non-modular experts.

Most experts flavors include an equivalent modular interface which will be a subclass of FusedMoEExpertsModular.

To be used with a particular FusedMoEPrepareAndFinalizeModular subclass, MoE kernels must have compatible activation formats, quantization types and quantization formats.

Kernel

Input act. format

Quant. types

Quant. format

Activation function

Apply Weight On Input

Modular

Source

triton

standard

all 1

G,A,T

silu, gelu,
swigluoai,
silu_no_mul,
gelu_no_mul

Y

Y

fused_experts,
TritonExperts

triton (batched)

batched

all 1

G,A,T

silu, gelu

6

Y

BatchedTritonExperts

deep gemm

standard,
batched

fp8

G(128),A,T

silu, gelu

6

Y

DeepGemmExperts,
BatchedDeepGemmExperts

cutlass_fp4

standard,
batched

nvfp4

A,T

silu

Y

Y

CutlassExpertsFp4

cutlass_fp8

standard,
batched

fp8

A,T

silu, gelu

Y

Y

CutlassExpertsFp8,
CutlasBatchedExpertsFp8

flashinfer

standard

nvfp4,
fp8

T

5

N

Y

FlashInferExperts

gpt oss triton

standard

N/A

N/A

5

Y

Y

triton_kernel_fused_experts,
OAITritonExperts

marlin

standard,
batched

3 / N/A

3 / N/A

silu,
swigluoai

Y

Y

fused_marlin_moe,
MarlinExperts,
BatchedMarlinExperts

trtllm

standard

mxfp4,
nvfp4

G(16),G(32)

7

N

Y

TrtLlmMxfp4ExpertsMonolithic,
TrtLlmMxfp4ExpertsModular,
TrtLlmNvFp4ExpertsMonolithic,
TrtLlmNvfp4ExpertsModular

hpc

standard

fp8

G(128),T

silu

Y

Y

HPCExperts

rocm aiter moe

standard

none,
mxfp4,
fp8

G(32),G(128),A,T

silu, gelu,
swigluoai,
situ

Y

Y

rocm_aiter_fused_experts,
AiterUnquantizedExperts,
AiterFp8BlockExperts,
AiterFp8TensorExperts,
AiterFp8ChannelExperts,
AiterMxfp4Experts

cpu_moe

standard

N/A

N/A

silu, gelu,
gelu_tanh,
swigluoai

Y

N

X86CPUUnquantizedExperts,
ArmCPUUnquantizedExperts,
CPUUnquantizedExperts

naive batched 4

batched

int8,
fp8

G,A,T

silu, gelu

6

Y

NaiveBatchedExperts

Table key

All types: mxfp4, nvfp4, int4, int8, fp8

A dispatcher wrapper around triton and deep gemm experts. Will select based on type + shape + quantization params

uint4, uint8, fp8, fp4

This is a naive implementation of experts that supports batched format. Mainly used for testing.

The activation parameter is ignored and SwiGlu is used by default instead.

Only handled by or supported when used with modular kernels.

Supported activations depend on the quantization format and FlashInfer version. NVFP4 supports swiglustep.

Modular Kernel "families" ¶

The following table shows "families" of modular kernels that are intended to work together. There are some combinations which may work but have not yet been tested, e.g. flashinfer with other fp8 experts.

backend

FusedMoEPrepareAndFinalizeModular subclasses

FusedMoEExpertsModular subclasses

deepep_high_throughput

DeepEPHTPrepareAndFinalize

DeepGemmExperts,
TritonExperts,
TritonOrDeepGemmExperts,
CutlassExpertsFp8,
MarlinExperts

deepep_low_latency

DeepEPLLPrepareAndFinalize

BatchedDeepGemmExperts,
BatchedTritonExperts,
CutlassBatchedExpertsFp8,
BatchedMarlinExperts

flashinfer

FlashInferCutlassMoEPrepareAndFinalize

FlashInferExperts

November 3, 2026

Back to top

Made with

Material for MkDocs
