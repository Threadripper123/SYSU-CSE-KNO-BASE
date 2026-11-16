# vLLM 投机解码（Speculative Decoding）文档

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　21,873 字符　｜　678 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/en/latest/features/speculative_decoding

Skip to content

You are viewing the latest developer preview docs. Click here to view docs for the latest stable release.

vLLM

Speculative Decoding

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

vLLM Speculation Methods

Method Selection at a Glance

Custom Speculators (Experimental)

--speculative-config schema

Common keys

Method-specific keys

N-gram

Suffix decoding

Notes

Speculative Decoding with Pipeline Parallelism

Lossless guarantees of Speculative Decoding

Known Feature Incompatibility

Resources for vLLM contributors

Home

User Guide

Features

Speculative Decoding

Speculative Decoding ¶

This document shows how to use Speculative Decoding with vLLM to reduce inter-token latency under medium-to-low QPS (queries per second), memory-bound workloads.

To train your own draft models for optimized speculative decoding, see vllm-project/speculators for seamless training and integration with vLLM.

vLLM Speculation Methods ¶

vLLM supports a variety of methods of speculative decoding. Model-based methods such as EAGLE, MTP and draft models provide the best latency reduction, while simpler methods such as n-gram and suffix decoding provide modest speedups without increasing workload during peak traffic.

EAGLE

LiLiCorr

Multi-Token Prediction (MTP)

Draft Model

N-Gram

Suffix Decoding

Hidden State Extraction

Custom Speculators (Experimental)

Dynamic Speculative Decoding

Adaptive Verification

FLy Verification (Lossy)

Per-Request Acceptance Metrics

Method Selection at a Glance ¶

Use this qualitative table as a starting point for method selection. Real gains
depend on your model family, traffic pattern, hardware, and sampling settings.

Method

Low QPS (latency focused)

High QPS (throughput focused)

Notes

EAGLE

High gain

Medium to high gain

Strong general-purpose model-based method.

MTP

High gain

Medium to high gain

Best when the target model has native MTP support.

Draft model

High gain

Medium gain

Needs a separate draft model.

N-gram

Low to medium gain

Medium gain

Lightweight and easy to enable.

Suffix decoding

Low to medium gain

Medium gain

No extra draft model; matches against prior responses too.

Dynamic Speculative Decoding

High gain

Higher than base SD method

Useful for RL or workload with fluctuating QPS

Adaptive Verification

High gain

Higher than base SD method

Sizes verification per request from drafter confidence; currently DSpark only.

For reproducible measurements in your environment, use
examples/features/speculative_decoding/spec_decode_offline.py
or the benchmark CLI guide.

Custom Speculators (Experimental) ¶

You can plug in your own speculator by registering it as a speculative method
from a vLLM plugin, and then using it with
"method": "<name>".

A custom speculator consists of:

The speculator itself: a subclass of
vllm.v1.worker.gpu.spec_decode.speculator.BaseSpeculator, typically of
DraftModelSpeculator (for a speculator that runs its own draft model, which
the model runner then loads and allocates draft KV cache for) or
TargetDependentARSpeculator (for EAGLE/MTP-style drafting from the target
model's hidden states). It's constructed with (vllm_config, device).

Its config: a subclass of
vllm.v1.worker.gpu.spec_decode.speculator_config.SpeculatorConfig, which
names the speculator class and describes how it behaves (KV lookahead slots,
whether it needs the target's hidden states, which features it supports,...). It's used in every vLLM process, including those that never build the
speculator, so its module shouldn't import the speculator. A speculator that
behaves like a built-in one should subclass that speculator's config (e.g.
Eagle3SpeculatorConfig ), which also determines how its draft model is
configured from model.

# my_package/config.py
from vllm.v1.worker.gpu.spec_decode.eagle.config import Eagle3SpeculatorConfig

class MySpeculatorConfig ( Eagle3SpeculatorConfig ):
speculator_class = "my_package.speculator.MySpeculator"
# Accept options in `speculator_options` (validated in `apply_options`).
supported_options = frozenset ({ "speculator_options" })

@classmethod
def apply_options ( cls, spec ):
options = spec. speculator_options or {}
if unknown := options. keys () - { "my_option" }:
raise ValueError ( f "Unknown speculator_options: { sorted ( unknown ) } " )

# my_package/__init__.py, registered as a `vllm.general_plugins` entry point
def register ():
from vllm.v1.worker.gpu.spec_decode.speculator_config import (
register_speculative_method,
)

register_speculative_method ( "my_spec", "my_package.config.MySpeculatorConfig" )

vllm serve <target-model> \
--speculative-config '{
"method": "my_spec",
"model": "<draft-model>",
"num_speculative_tokens": 3,
"speculator_options": {"my_option": 1}
}'

The speculator reads its options from
vllm_config.speculative_config.speculator_options. Once set up,
speculative_config.custom_method is the registered name ( "my_spec" ), and
speculative_config.method is the built-in method its config is based on
( "eagle3" ), so vLLM treats it like that built-in speculator.

--speculative-config schema ¶

Use --speculative-config to pass speculative decoding settings as a JSON
object on the CLI:

vllm serve <target-model> \
--speculative-config '{
"method": "draft_model",
"model": "<draft-model>",
"num_speculative_tokens": 5
}'

The same keys are accepted from Python via LLM(..., speculative_config={...}).
The tables below highlight common user-facing keys accepted in this JSON
object; they are not an exhaustive schema reference.
For more details, see the generated engine arguments reference
and the API docs for vllm.config.SpeculativeConfig.

Common keys ¶

These keys are commonly used across speculative decoding setups, though some
only apply to model-based methods such as draft_model, mtp, eagle3, and
dflash.

Key

Type

Default

Allowed values / meaning

method

string

None

Speculation method. Common values include draft_model, ngram, suffix, mtp, eagle3, and dflash. If omitted, vLLM infers the method from the provided configuration when possible.

model

string

None

Draft model, EAGLE head, or auxiliary model identifier. For ngram, ngram_gpu, suffix, and mtp, this can often be omitted.

num_speculative_tokens

integer > 0

None

Number of speculative tokens to propose per step. Required for methods that do not infer it from model metadata.

draft_tensor_parallel_size

integer >= 1

None

Tensor parallel size for the draft model.

draft_decode_context_parallel_size

integer >= 1

None

Decode context parallel size for the draft model: 1 or the target's decode_context_parallel_size. Defaults to the target's, except dflash / dspark drafts, which default to 1 unless they are full-attention MLA.

max_model_len

integer >= 1

None

Maximum context length for the draft model. A draft limit below the target's may also lower the engine's max_model_len.

parallel_drafting

boolean

null

Deprecated and has no effect. dflash and dspark always draft in parallel, and no other method supports it.

rejection_sample_method

string

standard

standard, synthetic, block, or the lossy fly verifier.

draft_sample_method

string

greedy

greedy uses target-only acceptance for random requests; probabilistic retains the full draft distribution for probability-ratio acceptance.

synthetic_acceptance_rates

list[float]

None

Per-position unconditional acceptance rates for synthetic rejection sampling. Each entry in [0, 1]; length must equal num_speculative_tokens; must be non-increasing.

synthetic_acceptance_length

float

None

Target mean acceptance length for synthetic; in [1, num_speculative_tokens + 1]. Mutually exclusive with synthetic_acceptance_rates.

fly_window_size

integer >= 1

min(6, num_speculative_tokens - 1)

Number of subsequent native acceptance decisions checked by FLy. FLy requires at least two speculative tokens, and the window must be smaller than num_speculative_tokens. With synthetic, runs FLy's entropy computation without deferring rejections (see FLy ).

fly_entropy_threshold

float >= 0

0.3

Minimum normalized top-k entropy, in [0, 1], required for FLy to defer a native rejection.

fly_entropy_top_k

integer >= 1

3

Number of largest processed target scores renormalized for FLy's entropy gate, capped at the vocabulary size.

Note

Gemma 4 assistant checkpoints are handled as Gemma 4 MTP speculators, not
as generic draft models. Use "method": "mtp" with the assistant
checkpoint in model, as shown in the MTP guide.

If startup logs show SpeculativeConfig(method='draft_model',...) for a
Gemma 4 assistant checkpoint, the installed vLLM version does not include
Gemma 4 MTP support for that path. Upgrade to a version that includes
Gemma 4 MTP support instead of forcing the assistant checkpoint through
generic draft-model speculative decoding.

Method-specific keys ¶

N-gram ¶

Key

Type

Default

Meaning

prompt_lookup_max

integer >= 1

5 if both lookup bounds are omitted; otherwise mirrors prompt_lookup_min when omitted

Maximum n-gram window size.

prompt_lookup_min

integer >= 1

5 if both lookup bounds are omitted; otherwise mirrors prompt_lookup_max when omitted

Minimum n-gram window size.

Example:

vllm serve <target-model> \
--speculative-config '{
"method": "ngram",
"num_speculative_tokens": 4,
"prompt_lookup_min": 2,
"prompt_lookup_max": 5
}'

Suffix decoding ¶

Key

Type

Default

Meaning

suffix_decoding_max_tree_depth

integer

24

Maximum suffix match length.

suffix_decoding_max_cached_requests

integer

10000

Set 0 to disable the cross-request corpus of finished responses.

suffix_decoding_corpus_tokens

integer

1048576

Token capacity of the GPU corpus of finished responses. Set 0 to match only within each request.

suffix_decoding_max_spec_factor

float

1.0

Deprecated and has no effect.

suffix_decoding_min_token_prob

float

0.1

Deprecated and has no effect.

Example:

vllm serve <target-model> \
--speculative-config '{
"method": "suffix",
"num_speculative_tokens": 8,
"suffix_decoding_max_tree_depth": 24,
"suffix_decoding_corpus_tokens": 1048576
}'

Notes ¶

--speculative-config expects a JSON object on the CLI. In YAML config
files, use a nested mapping instead of an escaped JSON string.

tensor_parallel_size is not a valid key in speculative_config. Use
draft_tensor_parallel_size instead.

Keys such as temperature and top_p are sampling parameters, not
--speculative-config fields.

Internal fields such as target_model_config, draft_model_config,
target_parallel_config, draft_parallel_config, and draft_load_config
are populated by vLLM and are not intended to be set by users.

Speculative Decoding with Pipeline Parallelism ¶

With pipeline parallelism ( pipeline_parallel_size > 1 ), the draft model runs as a whole model on the last pipeline stage, as it would at pipeline_parallel_size=1. A custom VLLM_PP_LAYER_PARTITION applies only to the target model.

Lossless guarantees of Speculative Decoding ¶

In vLLM, speculative decoding aims to enhance inference efficiency while maintaining accuracy. This section addresses the lossless guarantees of
speculative decoding, breaking down the guarantees into three key areas:

Theoretical Losslessness
- Speculative decoding sampling is theoretically lossless up to the precision limits of hardware numerics. Floating-point errors might
cause slight variations in output distributions, as discussed
in Accelerating Large Language Model Decoding with Speculative Sampling

Warning

These guarantees apply to the standard rejection sampler. FLy
deliberately relaxes verification and is not distribution preserving.

Algorithmic Losslessness
- vLLM’s implementation of speculative decoding is algorithmically validated to be lossless. Key validation tests include:

Rejection Sampler Convergence : Ensures that samples from vLLM’s rejection sampler align with the target
distribution. View Test Code

Greedy Sampling Equality : Confirms that greedy sampling with speculative decoding matches greedy sampling
without it. This verifies that vLLM's speculative decoding framework, when integrated with the vLLM forward pass and the vLLM rejection sampler,
provides a lossless guarantee. Almost all of the tests in tests/spec_decode/e2e
verify this property using this assertion implementation

vLLM Logprob Stability
- vLLM does not currently guarantee stable token log probabilities (logprobs). This can result in different outputs for the
same request across runs. For more details, see the FAQ section
titled Can the output of a prompt vary across runs in vLLM? in the FAQs.

While vLLM strives to ensure losslessness in speculative decoding, variations in generated outputs with and without speculative decoding
can occur due to following factors:

Floating-Point Precision : Differences in hardware numerical precision may lead to slight discrepancies in the output distribution.

Batch Size and Numerical Stability : Changes in batch size may cause variations in logprobs and output probabilities, potentially
due to non-deterministic behavior in batched operations or numerical instability.

For mitigation strategies, please refer to the FAQ entry Can the output of a prompt vary across runs in vLLM? in the FAQs.

Known Feature Incompatibility ¶

Pipeline parallelism is not composable with speculative decoding as of vllm<=0.15.0

Speculative decoding with draft models is not supported in vllm<=0.10.0

Resources for vLLM contributors ¶

[vLLM Office Hours #40] Intro to Speculators

A Hacker's Guide to Speculative Decoding in vLLM

What is Lookahead Scheduling in vLLM?

Information on batch expansion

Dynamic speculative decoding

November 13, 2026

Back to top

Made with

Material for MkDocs
