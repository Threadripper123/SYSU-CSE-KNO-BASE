# vLLM 设计文档：PagedAttention CUDA kernel 实现

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　27,198 字符　｜　624 个片段　｜　更新于 2026-11-13
来源: https://docs.vllm.ai/en/latest/design/kernel/paged_attention.html

# https://docs.vllm.ai/en/latest/design/kernel/paged_attention.html

来源: https://docs.vllm.ai/en/latest/design/kernel/paged_attention.html
抓取方式: read_page
实际URL: https://docs.vllm.ai/en/latest/design/kernel/paged_attention.html

Skip to content

You are viewing the latest developer preview docs. Click here to view docs for the latest stable release.

vLLM

Contributing to vLLM

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

Job Board

License

Developing

Linting

Documentation

Testing

Issues

Pull Requests & Code Reviews

DCO and Signed-off-by

AI Assisted Contributions

PR Title and Classification

Code Quality

Documenting User-Visible Changes

Adding or Changing Kernels

Notes for Large Changes

What to Expect for the Reviews

Pull Request Limits and Escalation

Thank You

Home

Developer Guide

Contributing to vLLM ¶

Thank you for your interest in contributing to vLLM! Our community is open to everyone and welcomes all kinds of contributions, no matter how small or large. There are several ways you can contribute to the project:

Identify and report any issues or bugs.

Request or add support for a new model.

Suggest or implement new features.

Improve documentation or contribute a how-to guide.

We also believe in the power of community support; thus, answering queries, offering PR reviews, and assisting others are also highly regarded and beneficial contributions.

Finally, one of the most impactful ways to support us is by raising awareness about vLLM. Talk about it in your blog posts and highlight how it's driving your incredible projects. Express your support on social media if you're using vLLM, or simply offer your appreciation by starring our repository!

Job Board ¶

Unsure on where to start? Check out the following links for tasks to work on:

Good first issues
Selected onboarding tasks

New model requests
Models with multi-modal capabilities

License ¶

See LICENSE.

Developing ¶

The first step of contributing to vLLM is to clone the GitHub repository:

git clone https://github.com/vllm-project/vllm.git
cd vllm

Then, configure your Python virtual environment.

It's recommended to use uv, a very fast Python environment manager, to create and manage Python environments. Please follow the documentation to install uv. After installing uv, you can create a new Python environment using the following commands:

uv venv --python 3.12 --seed --managed-python
source.venv/bin/activate

If you are only developing vLLM's Python code, install vLLM using:

VLLM_USE_PRECOMPILED = 1 uv pip install -e.
vllm download-kernels

To rebuild only the Rust frontend binary:./tools/build_rust.sh # release build./tools/build_rust.sh --debug # faster build for development

If you are developing vLLM's Python and CUDA/C++ code, install PyTorch first:

uv pip install torch torchvision torchaudio --extra-index-url https://download.pytorch.org/whl/cu129

Then install the necessary build dependencies from requirements/build/cuda.txt, skipping torch as it was installed in the previous step:

grep -v '^torch==' requirements/build/cuda.txt | uv pip install -r -

Finally install vLLM using:

uv pip install -e. --no-build-isolation

For more details about installing from source and installing for other hardware, check out the installation instructions for your hardware and head to the "Build wheel from source" section.

For an optimized workflow when iterating on C++/CUDA kernels, see the Incremental Compilation Workflow for recommendations.
For JIT kernel warmup conventions, see JIT Kernel Warmup.

Tip

vLLM is compatible with Python versions 3.11 to 3.14. However, vLLM's default Dockerfile ships with Python 3.12 and tests in CI (except mypy ) are run with Python 3.12.

Therefore, we recommend developing with Python 3.12 to minimise the chance of your local environment clashing with our CI environment.

Linting ¶

vLLM uses pre-commit to lint and format the codebase. See https://pre-commit.com/#usage if pre-commit is new to you. Setting up pre-commit is as easy as:

uv pip install pre-commit> = 4.5.1
pre-commit install

vLLM's pre-commit hooks will now run automatically every time you commit.

For package archive hashes, compiled requirements, and GitHub Actions commit
pins, see Dependency Hash Pinning.

When running ShellCheck or markdownlint directly, or configuring an editor
integration, use the configuration files in tools/pre_commit/ :
--rcfile=tools/pre_commit/.shellcheckrc for ShellCheck and
--config tools/pre_commit/.markdownlint.yaml for markdownlint-cli2.

Tips

You can manually run the pre-commit hooks using:

pre-commit run # runs on staged files
pre-commit run -a # runs on all files (short for --all-files)

Hook revisions in.pre-commit-config.yaml are pinned to commit SHAs. To bump them, use:

pre-commit autoupdate --freeze # bumps all hooks
pre-commit autoupdate --freeze --repo <url> # bumps one hook

Documentation ¶

MkDocs is a fast, simple and downright gorgeous static site generator that's geared towards building project documentation. Documentation source files are written in Markdown, and configured with a single YAML configuration file, mkdocs.yaml.

Get started with:

uv pip install -r requirements/docs.txt

Tip

Ensure that your Python version is compatible with the plugins
(e.g., mkdocs-awesome-nav requires Python 3.10+)

MkDocs comes with a built-in dev-server that lets you preview your documentation as you work on it.
From the root of the repository, run the following. Note that by default, the dev-server will run on 127.0.0.1:8000. Should you want to run the dev-server on a different port, use -a like this mkdocs serve -a localhost:8090.

mkdocs serve # with API ref (~10 minutes)
API_AUTONAV_EXCLUDE = vllm mkdocs serve # API ref off (~15 seconds)

Once you see a Serving on http://<address>:<port>/ line in the logs, the live preview is ready!
Open that address in your browser to see it — http://127.0.0.1:8000/ by default, or whichever host/port you passed to -a.

On pull requests, Read the Docs builds a documentation preview only if the PR changes files that affect the docs (e.g. docs/, examples/ or mkdocs.yaml ) and the pre-commit checks have not failed.
Changes to vllm/ alone do not trigger a build, so if your PR changes the API or CLI reference, add the build-docs label to force one.

For additional features and advanced configurations, refer to the:

MkDocs documentation

Material for MkDocs documentation (the MkDocs theme we use)

Testing ¶

vLLM uses pytest to test the codebase.

# Install the test dependencies used in CI (CUDA only)
uv pip install -r requirements/common.txt -r requirements/dev.txt --torch-backend = auto

# Install some common test dependencies (hardware agnostic)
uv pip install pytest pytest-asyncio

# Run all tests
pytest tests/

# Run tests for a single test file with detailed output
pytest -s -v tests/test_logger.py

Install python3-dev if Python.h is missing

If any of the above commands fails with Python.h: No such file or directory, install
python3-dev with sudo apt install python3-dev.

Warnings

Currently, the repository is not fully checked by mypy.

Currently, not all unit tests pass when run on CPU platforms. If you don't have access to a GPU
platform to run unit tests locally, rely on the continuous integration system to run the tests for
now.

Issues ¶

If you encounter a bug or have a feature request, please search existing issues first to see if it has already been reported. If not, please file a new issue, providing as much relevant information as possible.

Important

If you discover a security vulnerability, please follow the instructions here.

Pull Requests & Code Reviews ¶

Thank you for your contribution to vLLM! Before submitting the pull request,
please ensure the PR meets the following criteria. This helps vLLM maintain the
code quality and improve the efficiency of the review process.

DCO and Signed-off-by ¶

When contributing changes to this project, you must agree to the DCO.
Commits must include a Signed-off-by: header which certifies agreement with
the terms of the DCO.

Using -s with git commit will automatically add this header.

Tip

You can enable automatic sign-off via your IDE:

PyCharm : Click on the Show Commit Options icon to the right of the Commit and Push... button in the Commit window.
It will bring up a git window where you can modify the Author and enable Sign-off commit.

VSCode : Open the Settings editor
and enable the Git: Always Sign Off ( git.alwaysSignOff ) field.

AI Assisted Contributions ¶

Before making an AI assisted contribution, you must:

Be involved : Do not submit "pure agent" PRs. The human submitter is responsible for reviewing all changed lines, validating behavior end-to-end, and running relevant tests.

Ensure significance : Avoid one-off "busywork" PRs (single typo, isolated style cleanup, one mutable default fix, etc.). Bundle mechanical cleanups into a clear, systematic scope.

When AI tools provide non-trivial assistance in generating or modifying code, you must:

Review thoroughly : You remain responsible for all code you submit. Review and understand AI-generated code with the same care as code you write manually.

Disclose in PR : Always mention when a pull request includes AI-generated code. Add a note in the PR description.

Mark commits : Add attribution using commit trailers such as Co-authored-by: (other projects use Assisted-by: or Generated-by: ). For example:

Your commit message here

Co-authored-by: GitHub Copilot
Co-authored-by: Claude
Co-authored-by: gemini-code-assist
Signed-off-by: Your Name < [email protected] >

AI-assisted code must meet all quality standards: proper testing, documentation, adherence to style guides, and thorough review. Attribution helps reviewers evaluate contributions in context and maintains legal clarity for the project.

PR Title and Classification ¶

PR titles must start with one or more bracketed tags, followed by a space and a
concise description, for example, [Bugfix][Scheduler] Fix priority handling.
Any descriptive tag is accepted. The following well-known tags improve
consistency and discoverability; use one of them when it fits, or choose a more
specific tag.

Type tags describe the nature of the change:

[Bugfix], [Feature], [Perf], and [Refactor] for code changes.

[CI] or [CI/Build], [Test], and [Doc] or [Docs] for project
infrastructure, tests, and documentation.

[Misc] when another type tag does not fit.

Scope tags identify the affected area:

[Model], [Frontend], [Rust Frontend], [Core], and [Kernel] for major
areas of the codebase.

[Model Runner V2] or [MRV2], [Attention], [Multimodal],
[Quantization], [MoE], [Spec Decode], [LoRA], [KV Connector], and
[KV Offload] for frequently changed subsystems.

[ROCm], [XPU], [CPU], or [Hardware][Vendor] for hardware-specific
changes. Replace Vendor with the vendor name, for example,
[Hardware][AMD].

Type and scope tags can be combined, and multiple tags from either category can
be stacked without spaces. For example, [Bugfix][KV Offload] Fix block
accounting or [Perf][ROCm][Kernel] Optimize fused MoE.

Code Quality ¶

The PR needs to meet the following code quality standards:

We adhere to Google Python style guide and Google C++ style guide.

Pass all linter checks.

The code needs to be well-documented to ensure future contributors can easily
understand the code.

Include sufficient tests to ensure the project stays correct and robust. This
includes both unit tests and integration tests.

Document user-visible changes in the same PR; see
Documenting User-Visible Changes.

Documenting User-Visible Changes ¶

If a PR changes what users can run, configure, or observe, update the rendered
docs in the same PR; code comments, tests, and PR descriptions alone are not
found by users or their agents. CLI arguments, config fields, and environment
variables are documented in their docstring, help= text, or
environment_variables comment, which generate the reference pages. Everything
else goes on its page under docs/ (feature, model, hardware, deprecation),
linked from its index. Lead with what it is, how to enable it, and the default,
using the names users search for.

To check, ask a fresh agent a user question (e.g. "What does VLLM_FOO do?")
three times, limited in turn to web search, GitHub, and a local checkout (before
merge: the docs preview and the PR branch). If it cannot find and quote the
docs, fix the docs.

Adding or Changing Kernels ¶

When actively developing or modifying kernels, using the Incremental Compilation Workflow is highly recommended for faster build times.
Each custom kernel needs a schema and one or more implementations to be registered with PyTorch.

Make sure custom ops are registered following PyTorch guidelines:
Custom C++ and CUDA Operators
and The Custom Operators Manual.

Custom operations that return Tensors require meta-functions.
Meta-functions should be implemented and registered in Python so that dynamic
dims can be handled automatically. See above documents for a description of
meta-functions.

Use torch.library.opcheck()
to test the function registration and meta-function for any registered ops.
See tests/kernels for examples.

When changing the C++ signature of an existing op, the schema must be updated
to reflect the changes.

If a new custom type is needed, see the following document:
Custom Class Support in PT2.

Notes for Large Changes ¶

Please keep the changes as concise as possible. For major architectural changes
(>500 LOC excluding kernel/data/config/test), we would expect a GitHub issue
(RFC) discussing the technical design and justification. Otherwise, we will tag
it with rfc-required and might not go through the PR.

What to Expect for the Reviews ¶

The goal of the vLLM team is to be a transparent reviewing machine. We would
like to make the review process transparent and efficient and make sure no
contributor feels confused or frustrated. However, the vLLM team is small, so we
need to prioritize some PRs over others. Here is what you can expect from the
review process:

After the PR is submitted, the PR will be assigned to a reviewer. Every
reviewer will pick up the PRs based on their expertise and availability.
Official reviews can be performed by any of the
reviewers or
committers; committers perform the final
merge. To find someone to review your PR, see the reviewers' and
committers' areas of expertise or ask in the #pr-reviews channel on
Slack.

After the PR is assigned, the reviewer will provide status updates every 2-3
days. If the PR is not reviewed within 7 days, please feel free to ping the
reviewer or the vLLM team.

After the review, the reviewer will put an action-required label on the PR
if there are changes required. The contributor should address the comments and
ping the reviewer to re-review the PR.

Please respond to all comments within a reasonable time frame. Address each
piece of reviewer feedback with a change or an explanation. If a comment
isn't clear or you disagree with a suggestion, feel free to ask for
clarification or discuss the suggestion. If you and a reviewer can't reach
agreement, a committer can help decide.

Note that not all CI checks will be executed due to limited computational
resources. Reviewers with write access and configured trusted contributors
can comment /ci run for upstream CI or /amd-ci run for AMD CI only when
CI signals are needed before a PR is ready. After the PR is approved or has
the ready label, the PR author can use /ci run, /ci retry, /ci cancel,
or the corresponding /amd-ci variants. New commits do not start upstream
CI automatically.

Before creating a Buildkite build, /ci run and /amd-ci run, including
all and nightly, check the requested PR commit against its target branch
in the upstream repository. Your branch must contain every commit currently
on that branch: zero commits behind. This also applies to PRs targeting
release branches or another PR's branch.

If the PR is behind, the bot merges the target branch into your PR branch
(like the Update branch button) and runs CI on the resulting merge commit.
Pull the branch before pushing more changes. This needs Allow edits by
maintainers enabled on fork PRs. If the merge conflicts or is not allowed,
the bot reports the count without creating a new build; merge or rebase onto
the latest target branch yourself, then rerun the command.

PR authors without write access cannot start another full run (including
--allow-stale ) on a commit that already has a CI build. Push new changes,
use /ci retry to rerun failed jobs, or ask a reviewer with write access.

To run CI on an outdated branch at your own risk, append --allow-stale to
any run command, for example /ci run --allow-stale or
/amd-ci run all --allow-stale. The same authorization requirements apply.
The bot reports the lag and warns that outdated CI configuration may cause
failures. Before merging, merge or rebase onto the latest target branch and
rerun CI without --allow-stale on the latest PR commit.

A PR targeting main that changes docker/Dockerfile.rocm_base or
tools/vllm-rocm/therock_wheels.py fails pre-commit until a maintainer
with write access comments /ci rocm cache, which copies the ROCm base image
the PR's CI built into rocm/vllm-ci so main reuses it after the merge, or
/ci rocm nocache to skip that. Then re-run the failed pre-commit check.

Retrying or cancelling builds does not check branch freshness.

Apart from that merge, these commands do not modify your branch or enforce
merge requirements. Direct Buildkite launches are outside this workflow.

Maintainers must configure required Buildkite status checks on the latest PR
commit and enable GitHub's
Require branches to be up to date before merging
protection on the relevant target branches to enforce the merge policy. Select
statuses that confirm the required tests completed, using Buildkite as their
expected source. An aggregate status that reports "passed and blocked" does not
by itself guarantee those tests ran; verify the pipeline's required and optional
blocked steps before choosing statuses or changing blocked-build reporting.
The CI command workflow does not configure these repository or pipeline settings.

Pull Request Limits and Escalation ¶

vLLM uses GitHub's pull request limit
for contributors without write access. The current cap is 6 open PRs. If this
blocks well-intentioned critical work, contact a committer to request bypass
list access.

If you need an expedited review for an important contribution, please email us
at:

[email protected]

Using a verifiable company or university email, include:

your production or research use case

the problem you encountered

how your contribution addresses it

Thank You ¶

Finally, thank you for taking the time to read these guidelines and for your interest in contributing to vLLM.
All of your contributions help make vLLM a great tool and community for everyone!

November 5, 2026

Back to top

Made with

Material for MkDocs
