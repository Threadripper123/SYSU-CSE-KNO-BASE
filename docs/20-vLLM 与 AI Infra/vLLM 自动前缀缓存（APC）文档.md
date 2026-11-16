# vLLM 自动前缀缓存（APC）文档

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　14,185 字符　｜　508 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/en/latest/features/automatic_prefix_caching

# https://docs.vllm.ai/en/latest/features/automatic_prefix_caching

来源: https://docs.vllm.ai/en/latest/features/automatic_prefix_caching
抓取方式: read_page

Skip to content

You are viewing the latest developer preview docs. Click here to view docs for the latest stable release.

vLLM

Automatic Prefix Caching

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

Automatic Prefix Caching

Table of contents

Introduction

Enabling APC in vLLM

Hybrid Mamba models

FlashInfer ReplaySSM

Example workloads

Limits

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

Introduction

Enabling APC in vLLM

Hybrid Mamba models

FlashInfer ReplaySSM

Example workloads

Limits

Home

User Guide

Features

Automatic Prefix Caching ¶

Introduction ¶

Automatic Prefix Caching (APC in short) caches the KV cache of existing queries, so that a new query can directly reuse the KV cache if it shares the same prefix with one of the existing queries, allowing the new query to skip the computation of the shared part.

Note

Technical details on how vLLM implements APC can be found here.

Enabling APC in vLLM ¶

Set enable_prefix_caching=True in vLLM engine to enable APC. Here is an example:

examples/features/automatic_prefix_caching/automatic_prefix_caching_offline.py

Hybrid Mamba models ¶

With prefix caching enabled, Mamba state is stored only on the Mamba block grid, so a prefix-cache hit can resume only at a block boundary. --enable-mamba-shared-prefix-checkpoint also stores a checkpoint at the shared-prefix junction, the point where an earlier request with the same prefix stopped. Requests whose shared prefix ends inside a block can then reuse it.

This helps when many requests share a long system prompt and then diverge. It is off by default, and takes effect only when all of the following hold:

prefix caching enabled

EAGLE/MTP speculative decoding on the Mamba group

--prefix-match-unit smaller than the Mamba block size

the model does not use multi-module MTP

vllm serve <hybrid-model> \
--prefix-match-unit 64 \
--enable-mamba-shared-prefix-checkpoint

--prefix-match-unit is required. It sets the granularity at which prefix-cache keys are computed. When unset it defaults to the greatest common divisor of the prefix-cacheable KV cache group block sizes. For hybrid Mamba models that is the block size itself, so no sub-block boundary exists and the flag has no effect.

Choose a value that divides the block size of every prefix-cacheable KV cache group, and that is a multiple of the per-state compression ratio for models that use one, such as sparse MLA. vLLM validates both at startup and names the offending sizes in the error. Read the served block size from the startup log. 64 is a reasonable starting point.

FlashInfer ReplaySSM ¶

FlashInfer ReplaySSM caches recent inputs to reduce full-state writes during Mamba2 decoding. It is disabled by default. Both Model Runners V1 and V2 support prefix caching with ReplaySSM, including MTP speculative decoding.

This example enables ReplaySSM and prefix caching with three MTP draft tokens:

vllm serve nvidia/NVIDIA-Nemotron-3.5-Lightning-30B-A3B-NVFP4 \
--tensor-parallel-size 2 \
--max-model-len 12288 \
--dtype bfloat16 \
--mamba-backend flashinfer \
--mamba-ssm-cache-dtype float16 \
--enable-mamba-cache-stochastic-rounding \
--mamba-cache-philox-rounds 5 \
--use-replayssm \
--replayssm-buffer-len 16 \
--enable-prefix-caching \
--speculative-config '{"method":"mtp","num_speculative_tokens":3}'

--enable-prefix-caching selects Mamba cache mode align. To use ReplaySSM without prefix caching, replace this flag with --no-enable-prefix-caching. This selects mode none. To disable speculative decoding, remove --speculative-config.

FlashInfer ReplaySSM has these limits:

align requires BF16 model inputs for its replay rings. The SSM state dtype is independent. The example uses FP16 state with stochastic rounding.

The logical replay buffer cannot exceed 16 tokens. For speculative decoding, it must contain at least 1 + num_speculative_tokens tokens.

Pipeline parallelism, microbatching, and KV connectors are not supported.

With --mamba-backend triton (for example on ROCm), ReplaySSM also supports speculative decoding with or without prefix caching. It has the same speculative-decoding buffer requirement and parallelism limits, has no 16-token buffer cap, and does not support stochastic rounding.

Prefix reuse still requires a shared prefix with a saved Mamba checkpoint. The matching rules for hybrid Mamba models above also apply.

Example workloads ¶

We describe two example workloads, where APC can provide huge performance benefit:

Long document query, where the user repeatedly queries the same long document (e.g. software manual or annual report) with different queries. In this case, instead of processing the long document again and again, APC allows vLLM to process this long document only once, and all future requests can avoid recomputing this long document by reusing its KV cache. This allows vLLM to serve future requests with much higher throughput and much lower latency.

Multi-round conversation, where the user may chat with the application multiple times in the same chatting session. In this case, instead of processing the whole chatting history again and again, APC allows vLLM to reuse the processing results of the chat history across all future rounds of conversation, allowing vLLM to serve future requests with much higher throughput and much lower latency.

Limits ¶

APC in general does not reduce the performance of vLLM. With that being said, APC only reduces the time of processing the queries (the prefilling phase) and does not reduce the time of generating new tokens (the decoding phase). So APC does not bring performance gain when vLLM spends most of the time generating answers to the queries (e.g. when the length of the answer is long), or new queries do not share the same prefix with any of existing queries (so that the computation cannot be reused).

November 12, 2026

Back to top

Made with

Material for MkDocs
