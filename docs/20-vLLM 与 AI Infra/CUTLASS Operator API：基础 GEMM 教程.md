# CUTLASS Operator API：基础 GEMM 教程

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　16,177 字符　｜　262 个片段　｜　更新于 2026-11-13
来源: https://docs.nvidia.com/cutlass/4.7.0/media/docs/operators/tutorials/000_gemm.html

# https://docs.nvidia.com/cutlass/4.7.0/media/docs/operators/tutorials/000_gemm.html

来源: https://docs.nvidia.com/cutlass/4.7.0/media/docs/operators/tutorials/000_gemm.html
抓取方式: read_page

Skip to main content

Back to top

Ctrl + K

NVIDIA CUTLASS Documentation

Choose version

Search
Ctrl + K

Search
Ctrl + K

NVIDIA CUTLASS Documentation

Choose version

Table of Contents

Changelog

CUTLASS Python

Overview

Quick Start Guide

DSL Programming Model
Introduction

Code Generation

Control Flow

JIT Argument Generation

JIT Argument: Layouts

Struct-like JIT Arguments

JIT Caching

JIT Compilation Options

Guides
Educational Notebooks

MMA Programming Guides
Warp-Level MMA Instructions Programming Guide

Warpgroup MMA Programming Guide

tcgen05 MMA Programming Guide

Integration with Frameworks

Debugging

In-Kernel Event Tracing (IKET) Profiling

Autotuning with the DSL

Ahead-of-Time (AOT) Compilation

TVM FFI Compilation

API Reference
Basic Data Types

Primitives (experimental)

CUDA (Jittable)

CuTe
Core

Runtime

Math

GPU Operations
nvgpu

nvgpu.common

nvgpu.warp

nvgpu.warpgroup

nvgpu.cpasync

nvgpu.tcgen05

arch

Naming Conventions

Pipeline

Task Scheduling (experimental)
Introduction

Resources

Pipeline Types

Schedules

Scheduling Patterns

Validation

Reading the Printed Output

Allocators

Pipeline Groups

Programmatic Dependent Launch

API Reference
task_scheduling

resources

task

task_manager

schedule_builder

memory

pipeline_group

pipeline

enums

Utilities
Hopper (SM90)

Blackwell (SM100)

Talks and Presentations

Limitations

Deprecation Policy

FAQs

CUTLASS Operator API

CUTLASS Operator API

Tutorials
Core concepts and basic GEMM

GEMM with fused epilogue

Bringing your own kernel

Host latency best practices

Fake tensors for compilation

Grouped GEMM with contiguous offset

Block-scaled GEMM (MXFP8)

API reference
Operator interface

Arguments and Operands

Kernel discovery

Metadata

Miscellaneous

CUTLASS C++

Overview

Getting Started
Quickstart

IDE Setup

Build
Building on Windows with Visual Studio

Building with Clang as host compiler

Functionality

Terminology

Fundamental Types

Programming Guidelines

GEMM Heuristics

Efficient GEMM in CUDA

Synchronization primitives

CUTLASS Profiler

GEMM Performance Measurement Guidelines

Dependent Kernel Launch

Blackwell Specific
Blackwell SM100 GEMMs

Blackwell Cluster Launch Control

CuTe
00_quickstart

01_layout

02_layout_algebra

03_tensor

04_algorithms

0t_mma_atom

0x_gemm_tutorial

0y_predication

0z_tma_tensors

CUTLASS 3.x
Design

GEMM Backwards Compatibility

GEMM API

CUTLASS 2.x
Layouts and Tensors

GEMM API

Tile Iterator Concepts

Utilities

Code Organization

Grouped Kernel Schedulers

CUTLASS Convolution

Reference

Software License Agreement

Tutorials

Basic GEMM using CUTLASS Operator API

Note

This page is a rendered version of 000_gemm.ipynb
available on GitHub.

Basic GEMM using CUTLASS Operator API #

CUTLASS Operator API serves to integrate kernels written using CUTLASS Python DSLs into your code. It provides a kernel-agnostic interface to discover, compile, and run CUTLASS Python kernels, and a registry of kernels exposed via that interface.

This notebook walks through a minimal GEMM (Generalized Matrix-Matrix Multiplication) example, and introduces the core concepts of the API.

[1]:

import torch

import cutlass

import cutlass.operators as ops

if not ( status := ops. utils. device. device_or_env_supports ( "80" )):
print (
f "This notebook requires a GPU with compute capability >= 80. \n { status. error } "
)
import sys

sys. exit ( 0 )

Running your first Operator #

While DSLs focus on kernel authoring, CUTLASS Operator API focuses on ease of managing and integrating those kernels into libraries that use CUTLASS.

It views kernels as end-to-end operators that execute an operation (like GEMM, MoE GEMM). We will specify a basic GEMM operation, find Operator s that support it, and comple and run one of them.

Setting up the operation arguments #

CUTLASS Operator API has first-class support for PyTorch and other DLPack-compatible tensors. We start by creating torch tensors that will be operands to a matrix multiplication.

[2]:

M, N, K, L = 128, 256, 64, 2
ab_type = torch. float16
out_type = torch. float16
acc_type = torch. float32

A = torch. randint ( - 1, 2, ( L, M, K ), device = "cuda", dtype = ab_type )
B = torch. randint ( - 1, 2, ( L, K, N ), device = "cuda", dtype = ab_type )
out = torch. empty (( L, M, N ), device = "cuda", dtype = out_type )

reference = ( A @ B ). to ( out. dtype )

We then create a GemmArguments object. This object specifies:

what logical operation do we want to perform (a GEMM)

on which operands we want to perform that operation ( A, B, out as declared above)

[3]:

args = ops. GemmArguments ( A = A, B = B, out = out, accumulator_type = acc_type )

Operator discovery #

We now need to find operators that can perform the operation we expressed in args.

The simplest way to do so is to use get_operators(args). The library has a pre-registered set of many operators. A subset of these support GEMM, and a further subset support the tensor dtypes, layouts, target arch, etc. we want. With get_operators, we query all Operator s that can functionally support (accept and give correct result for) our args.

Any of the returned operators will be functionally equivalent, but they may have different design or performance characteristics. We arbitrarily pick the first of the returned operators to execute here

[4]:

operators = ops. get_operators ( args, target_sm = "80" )
assert operators, "No operators found for the given arguments!"
operator = operators [ 0 ]

Run the operator #

Running the operator is as simple as operator.run(args).

This implicitly JIT-compiles the operator, and launches it on the GPU device using our given arguments.

[5]:

operator. run ( args )

torch. testing. assert_close ( out, reference )

Understanding the core interfaces #

1. RuntimeArguments / GemmArguments #

RuntimeArguments describes the operation that we want to perform, and all the runtime information we wish to pass to an operator. This includes primary runtime operands to the operation, but also any custom epilogue fusions and runtime performance options.

We provide builtin subtypes of RuntimeArguments for common operations (e.g. GEMM, Grouped GEMM).

For instance, GemmArguments is a type of RuntimeArguments :

@dataclass
class GemmArguments ( RuntimeArguments ):
A : Operand
B : Operand
out : Operand
accumulator_type : NumericLike

GemmArguments conveys:

We want to perform a GEMM operation ( out = A @ B )

We want to perform it for operands in A, B, out, with intermediate results stored as accumulator_type. In our case, the operands are individual tensors but later tutorials explore other operands.

We can optionally set a custom epilogue that is fused on top of the base GEMM. Some operators also support some runtime performance controls which can be specified here. These will be discussed in detail in other tutorials.

It is an operator-agnostic way to specify the desired functionality – we are declaring that this is the operation we would like to perform, irrespective of which operator will be chosen to perform it.

RuntimeArguments can be constructed from any tensor-like object. This includes torch.Tensor, cute.Tensor, or any other DLPack-compatible tensors.

2. Operator Discovery #

There are several operators available in CUTLASS DSLs that are registered with, and discoverable via, the CUTLASS Operator API.

This includes operators for various operations. For the same operation, there are different implementations that use different algorithms, optimizations, or architecture features. Even within the same implementation, there are several parametrized instances or configurations of it with different combinations of operand types, layouts, tile sizes, etc.

In the previous step, we used GemmArguments to specify our desired GEMM in an operator-agnostic way. Now we find specific operators that can fulfill that functionality.

[6]:

# get_operators() fetches all operators when called without args
all_operators = ops. get_operators ()
print ( f "A total of { len ( all_operators ) } Operator instances are available." )

# we can limit the search to operators supporting given args
operators = ops. get_operators ( args )
print ( f "Of these, { len ( operators ) } support the given arguments." )

# we can further limit the search to a specific GPU arch (here, we use current device)
target_sm = ops. utils. device. device_or_env_target_sm ()
operators = ops. get_operators ( args, target_sm = target_sm )
print ( f "Of these, { len ( operators ) } support the given GPU architecture." )

operator = operators [ 0 ]
print ( f "Picked operator with name: { operator. metadata. operator_name } " )

A total of 184434 Operator instances are available.
Of these, 572 support the given arguments.
Of these, 548 support the given GPU architecture.
Picked operator with name: cutedsl.PersistentDenseGemmOperator_sm100_ttt_AFloat16_BFloat16_outFloat16_accFloat32_2cta_cluster2x1x1_tile128x32x64_tma_store

3. Operator execution #

Once we have selected an operator, we are now ready to execute it. We previously showed the simplest way to do this is operator.run(args).

This method does the following:

verify that the Operator supports the given args

JIT-compile the Operator

launch the compiled Operator function

Users can do these steps individually for more control:

Verify that the Operator supports the given args #

operator.supports(args) checks if the Operator supports the given args * this is relevant if the operator was not picked just for these args

[7]:

supported = operator. supports ( args )
assert supported

If the arguments are not supported by this Operator, supports returns a Status object explaining the error.

[8]:

unsupported_args = ops. GemmArguments (
A = A. to ( torch. bfloat16 ), B = B, out = out, accumulator_type = acc_type
)
if not ( status := operator. supports ( unsupported_args )):
print ( status. error )

assert not status

Operand `A` is unsupported: Expected element type Float16, got BFloat16

JIT compiling the operator #

operator.compile(args) compiles the operator, and returns a CompiledArtifact

This compiled artifact is a lightweight wrapper over the result of compiling an Operator (e.g., via cute.compile() ).

For just-in-time compilation, we can use the compiled artifact straightaway. In the future, we will support optionally serializing it for ahead-of-time compilation and deserialized in a different context.

[9]:

compiled_artifact = operator. compile ( args )

Launching the compiled Operator function #

operator.run(args) launches the compiled Operator function. The next example uses: * the precompiled artifact * a custom stream to launch to * bypasses the supports check already performed above ( assume_supported_args=True ).

[10]:

# zero the output to avoid testing stale output
out. zero_ ()

operator. run (
args,
compiled_artifact,
stream = torch. cuda. Stream (),
assume_supported_args = True,
)
torch. testing. assert_close ( out, reference )

Passing in a precompiled operator is critical to achieving good performance because it avoids JIT compiling the operator on each invocation. JIT compilation always occurs when a precompiled operator is not provided in the call to operator.run().

Workspace Buffers #

Some operators may also require a device “workspace”. This is an additional buffer needed by some operators for book-keeping, temporary results, etc. Its size can be queried using operator.get_workspace_size(args). Most operators will have a workspace size of 0. If an operator does have a non-zero workspace size, an additional buffer of at least that size must be provided. Without it, the Operator behavior is undefined.

[11]:

workspace_size = operator. get_workspace_size ( args )
workspace = torch. empty ( workspace_size. size_bytes, device = "cuda", dtype = torch. int8 )

out. zero_ ()
operator. run ( args, compiled_artifact, stream = torch. cuda. Stream (), workspace = workspace )
torch. testing. assert_close ( out, reference )

Advanced: Filtering on Metadata #

Using RuntimeArguments to search for supporting operators is a convenient way to discover operators: users directly specify their desired functionality, and get_operators() finds the supporting operators. It covers all logical operands of any operation, as well as (in later examples) epilogue fusions, and performance controls.

However, there may be cases where users want more advanced ways to query operators. These could be:

when the desired properties may not be expressed in runtime controls

the simplest scenario may be if you’re searching searching for an Operator with a specific name, a specific class, etc.

searching for operator’s static properties such as tile size, cluster size, etc.

when the RuntimeArguments are not available or you want to generate & pre-compile a broader set of operators

For such cases, we provide a more advanced filtering based on OperatorMetadata

OperatorMetadata captures a wide variety of properties of a Operator.

These are properties of an Operator’s functional support (like operand types, layouts, alignments), as well as architectural/design choices & performance characteristics (like tilze size, scheduling characteristics).

Different operators may use different sub-classes of metadata.operands, metadata.design, metadata.epilogue for flexibility, which can also identify their characteristics.

@dataclass
class OperatorMetadata :
operator_name : str
operator_class : type [ Operator ]
supported_targets : list [ TargetSm ]
operands : OperandsMetadata
design : DesignMetadata | None = None
epilogue : EpilogueMetadata | None = None

Every unique Operator instance can be distinguished by its metadata. It can be used in filtering for operators in addition to the RuntimeArguments, by providing a custom metadata_filter.

Here, we get all operators that support args, and have metadata.design of type Sm100DesignMetadata.

[12]:

operators = ops. get_operators (
args,
metadata_filter = lambda metadata : isinstance (
metadata. design, ops. metadata. Sm100DesignMetadata
),
)
print ( f "Found { len ( operators ) } operators which support args & have Sm100DesignMetadata" )

Found 544 operators which support args & have Sm100DesignMetadata

We can construct more advanced filters by leveraging duck-typing. Additionally, we can get all the operators that match our filter, rather than supporting a fully-defined set of arguments. This could be useful to pre-generate large set of operators not targeted to any one problem.

[13]:

def a_more_complex_filter ( metadata : ops. OperatorMetadata ) -> bool :
"""Find all GEMM operators that support Float16 A and 2-CTA MMA."""
# Only look at GEMM operators
if not isinstance ( metadata. operands, ops. metadata. GemmOperandsMetadata ):
return False
# Only look at operators with A-type F16
if metadata. operands. A. dtype!= cutlass. Float16 :
return False
# Only look at operators with tile_shape[0] == 128
if getattr ( metadata. design, "tile_shape", [ None ])[ 0 ]!= 128 :
return False
return True

# Look ma, no args! Fetch all operators that match the filter,
# instead of supporting a complete set of args
operators = ops. get_operators (
args = None,
metadata_filter = a_more_complex_filter,
)
print ( f "Found { len ( operators ) } matching operators" )

Found 16736 matching operators

previous

Tutorials

next

Custom epilogue fusions for GEMMs

On this page

Running your first Operator
Setting up the operation arguments

Operator discovery
Run the operator

Understanding the core interfaces
1. RuntimeArguments / GemmArguments

2. Operator Discovery

3. Operator execution
Verify that the Operator supports the given args

JIT compiling the operator

Launching the compiled Operator function

Workspace Buffers

Advanced: Filtering on Metadata

Copyright © 2025-2026, NVIDIA Corporation.

Last updated on Aug 06, 2026.
