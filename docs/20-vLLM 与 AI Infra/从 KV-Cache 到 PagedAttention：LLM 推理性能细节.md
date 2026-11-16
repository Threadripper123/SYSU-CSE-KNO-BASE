# 从 KV-Cache 到 PagedAttention：LLM 推理性能细节

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　7,120 字符　｜　114 个片段　｜　更新于 2026-11-13
来源: https://cnblogs.com/sio2zyh/p/18978864

会员

周边

新闻

博问

闪存

赞助商

Chat2DB

所有博客

当前博客

我的博客
我的园子
账号设置
会员中心

简洁模式...

退出登录

注册
登录

sio2zyh

博客园

首页

新随笔

联系

订阅

管理

从KV-Cache到PagedAttention，揭秘LLM推理性能的全部细节

终极解析：从KV-Cache到PagedAttention，深入vLLM代码揭秘性能黑魔法

当我们与ChatGPT、Gemini等大型语言模型（LLM）流畅对话时，我们惊叹于其生成文本的速度和连贯性。但这顺滑体验的背后，是一系列精巧的工程技术在默默支撑，它们解决了巨大的计算和内存挑战。其中， KV-Cache 和 PagedAttention 是两个最核心、最具革命性的优化技术。

这篇文章将作为一份详尽的指南，不仅解释“是什么”和“为什么”，更将带你深入vLLM的源代码，看看这些顶尖的理念是如何被转化为高效、健壮的工程实现的。我们将从基础概念出发，用直观的比喻层层递进，最终为你呈现一幅完整、清晰的LLM推理优化技术全景图。

一、基础加速器：KV-Cache的“速记本”原理与局限

1. 问题的根源：自回归生成的“重复劳动”

LLM生成文本的过程是“自回归”的，即一个词一个词地吐出。为了生成下一个词，模型必须回顾并理解它前面说过的所有内容。这个过程依赖于模型的核心组件—— 注意力机制（Attention Mechanism） 。

在注意力机制中，每个输入到模型的词（Token）都会被映射成三个关键的向量：

Query (Q) : 当前词为了更好地理解上下文而发出的一个“提问”或“查询信号”。它在问：“为了生成下一个词，我应该关注历史内容中的哪些部分？”

Key (K) : 历史序列中每个词给自己贴上的“索引标签”或“关键字”。它响应Query的提问，表示“我这里有这样的信息，你可以通过这个Key来找到我”。

Value (V) : 这是每个词实际携带的“信息”或“内涵”。一旦Query通过Key找到了相关的历史词，它就会提取这些词的Value来汇集信息。

瓶颈在哪里？ 想象一下生成第100个词。模型需要用第100个词的Q，去和前面99个词的K进行匹配计算。如果从头开始，就意味着要重新计算这99个词的K和V向量。当生成第101个词时，又要重新计算前面100个词的K和V……这其中包含了海量的重复计算，效率极低。

2. KV-Cache的诞生：避免重复，记住历史

为了解决这个问题， KV-Cache 应运而生。它的思想简单而高效：

计算过一次的Key和Value，就没必要再算第二遍。把它们缓存（Cache）起来！

具体来说，模型每处理一个词，就将其计算出的K和V向量存入GPU显存中的一块专用区域。当生成下一个词时，模型只需为这个新词计算K和V，然后从缓存中直接读取所有历史词的K和V，一起送入注意力模块即可。

📖 的比喻：一本胶装的速记本

让我们用一个比喻来深化理解：

没有KV-Cache ：AI是一位记忆力很差的作者，每写一个新字，都必须从头到尾把之前写的所有内容重新阅读并消化一遍。

有了KV-Cache ：我们给了他一本 “胶装的速记本” 。每写一个字，他就把这个字的核心要点（K和V）记在速记本上。当要写下一个字时，他不再需要重读原文，只需快速翻阅速记本，就能立刻掌握上下文，效率大大提升。

3. “速记本”的致命缺陷：内存碎片化

这本“速记本”虽然好用，但它的“胶装”特性（即要求 连续的物理内存 ）带来了灾难性的内存管理问题：

内部碎片（Internal Fragmentation） ：为了保证有足够空间，系统通常会申请一块非常大的连续内存（比如能容纳2048个词的KV Cache）。如果一个用户的请求很短（比如只有100个词），那么预留的剩下1948个词的预留空间就被彻底浪费了。这就像为了写一首短诗，却预留了一整本厚厚的笔记本，后面的空白页谁也用不了。

外部碎片（External Fragmentation） ：当大量不同长度的请求并发处理时，显存被切割成许多不连续的小块。当一个需要较长连续内存的新请求到来时，即使总的剩余显存还很多，系统也可能找不到一块足够大的 连续 空间来满足它。这就像你的书桌上散落着许多小纸片，虽然纸张总面积很大，但你却找不到一张足够大的白纸来画一幅完整的画。

据vLLM论文统计，这种原始的KV-Cache管理方式，内存浪费可高达60%-80%。

二、内存管理的革命：PagedAttention的“活页夹”系统

为了根治内存碎片问题，由伯克利大学的研究者提出的 PagedAttention 闪亮登场。它的核心灵感，直接来源于现代计算机操作系统沿用数十年的经典技术—— 虚拟内存 和 分页（Paging） 。

PagedAttention的哲学是： 为什么物理内存非要连续？

1. 核心机制：分块、建表、映射

PagedAttention将KV-Cache的管理机制进行了彻底的重构：

分块 (Blocking) ：它不再将整个KV-Cache视为一个整体，而是将其在逻辑上切分成许多个固定大小的 块（Block） 。每个Block可以存储固定数量Token的Key和Value（例如，在vLLM的实现中，一个Block可以存16个Token）。这些Block就是我们的“活页纸”。

块表 (Block Table) ：这是PagedAttention的“大脑”。系统为每一个请求（Sequence）都维护一个独立的“块表”。这个表就像活页夹的目录，它的作用是记录该请求的每一个 逻辑Block ，分别存储在物理显存中的哪个 物理Block 上。它建立了一个从“逻辑页码”到“物理页存放位置”的映射。

物理内存管理器 (Block Manager) ：这是一个中央化的“图书管理员”，它负责管理GPU显存中所有可用的物理Block。当一个请求需要新的空间（生成新Token）时，调度器会向Block Manager申请一张新的“活页纸”（物理Block），然后将这张纸的存放地址登记到对应请求的“目录”（Block Table）中。

通过这套机制，物理上零散的内存空间被高效地组织和利用起来，从根本上消除了内外碎片问题，将内存利用率提升到了惊人的96%。

三、从代码看智慧：深入vLLM的工程实现

理论的优雅最终需要通过代码来实现。让我们深入 vllm 项目的源码，看看这些设计是如何落地的。

1. 缓存初始化与容量预估

在 vllm/worker/worker.py 中， initialize_cache 函数揭示了系统启动时的核心准备工作：

def initialize_cache(self, num_gpu_blocks: int, num_cpu_blocks) -> None:
"""Initialize the KV cache by invoking the underlying worker.
"""
# NOTE: This is logged in the executor because there can be >1 workers.
logger.info("# %s blocks: %d, # CPU blocks: %d",
vllm.platforms.current_platform.device_name,
num_gpu_blocks, num_cpu_blocks)
max_concurrency = (num_gpu_blocks * self.cache_config.block_size /
self.model_config.max_model_len)
logger.info("Maximum concurrency for %s tokens per request: %.2fx",
self.model_config.max_model_len, max_concurrency)

self.cache_config.num_gpu_blocks = num_gpu_blocks
self.cache_config.num_cpu_blocks = num_cpu_blocks

self.collective_rpc("initialize_cache",
args=(num_gpu_blocks, num_cpu_blocks))

这段代码告诉我们：

分层缓存 ：vLLM不仅管理GPU上的KV Cache（ num_gpu_blocks ），还管理CPU上的缓存（ num_cpu_blocks ）。这为我们后面要讲的**Swapping（交换）**机制提供了基础。

容量意识 ：在启动时，系统会根据可用的GPU Block总数、每个Block的大小（ block_size ）以及模型支持的最大长度（ max_model_len ），估算出理论上的 最大并发度（Maximum Concurrency） 。这体现了系统对自身资源和处理能力的精确掌控。

分层缓存 ： PagedAttention 会将 KV 缓存分割为固定大小的块（默认是16个token/块），内存按块分配而非连续预留。同时物理块可分散存储，逻辑上通过映射表维护连续性。所以为了能存储 kv 缓存，所以需要预留一定量的 GPU 显存。 vLLM 的计算方法是：可用的显存等于总 GPU 显存减去模型权重的大小、中间激活大小和缓冲区（默认为总内存的 10%）。模型大小已知，但中间激活大小未知，即推理过程中中间激活占用的最大内存， vLLM 通过运行虚拟数据然后分析内存消耗来确定此数字。虚拟数据的大小由配置中的参数决定，默认情况下设置为模型支持的最大上下文长度。

2. 用“规格说明书”定义一切：KVCacheSpec

vLLM设计的精髓之一，在于其高度的模块化和可扩展性，这在 vllm/kv_cache_interface.py 中体现得淋漓尽致。系统定义了一系列 Spec （Specification，规格说明）类，它们像一份份蓝图，精确描述了不同类型模型层所需要的KV Cache的“形状”和“大小”。

# vllm/kv_cache_interface.py

@dataclass
class KVCacheSpec:
"""定义KV Cache格式的基类"""
block_size: int # 一个Block里有多少个Token

@property
def page_size_bytes(self) -> int:
"""一个Block（页面）占多少字节"""
raise NotImplementedError

#... 还有 AttentionSpec, MambaSpec 等等

灵活支持多样的模型架构

现代LLM架构百花齐放，vLLM通过不同的 Spec 子类来优雅地支持它们：

FullAttentionSpec : 用于标准的全局注意力模型。

SlidingWindowSpec : 专门用于像Mistral这样的滑动窗口注意力模型。它需要额外记录 sliding_window 的大小，并且 max_memory_usage_bytes 的计算方式也完全不同，因为它只需要为窗口内的Token分配缓存。

MambaSpec : Mamba是与Transformer不同的架构，它的状态缓存（SSM state）和K/V对完全不同。vLLM通过一个独立的 MambaSpec 来描述其独特的形状和大小。

这种设计的好处是 极致的灵活性 。当未来出现新的模型架构时，开发者只需要定义一个新的 Spec 子类，并实现其计算内存占用的方法，就能无缝地集成到vLLM的内存管理体系中，而无需改动核心的调度和内存分配逻辑。

3. “组”的概念：KVCacheGroupSpec

一个模型的所有层不一定使用同一种注意力机制。vLLM引入了 KVCacheGroupSpec 的概念，将使用相同KV Cache规格的层 分组管理 。

@dataclass
class KVCacheGroupSpec:
"""代表共享同一个块表（Block Table）的模型层分组"""
layer_names: list[str]
kv_cache_spec: KVCacheSpec

这意味着，在一个混合模型中，所有“全局注意力层”可能属于一个Group，共享一套内存管理策略；而所有“滑动窗口注意力层”属于另一个Group。这进一步提升了内存管理的精细度和效率。

四、深入引擎：PagedAttention的实现魔法

有了代码层面的理解，我们再来回顾PagedAttention的核心机制，会发现一切都豁然开朗。

1. 定制化的Attention核（CUDA Kernel）：聪明的读者

标准的Attention计算库是为连续内存设计的。为了让GPU能在这些由“块表”管理的零散Block上高效工作，PagedAttention必须实现一个定制化的底层计算核。

这个定制Kernel的工作流程如下：

接收块表 ：除了接收常规的Query向量，该Kernel还会额外接收当前请求的 块表 作为输入。

间接寻址 ：当Kernel需要计算Query和某个历史Token的Attention时，它首先根据Token在序列中的位置计算出它属于哪个 逻辑Block 。

查询物理地址 ：接着，它查询块表，找到这个逻辑Block对应的 物理Block 在显存中的真实地址。

数据获取 (Gather) ：最后，从该物理地址中精准地抓取所需的Key和Value向量进行计算。

2. 写时复制 (Copy-on-Write)：终极内存共享方案

这是实现高吞吐量的王牌。考虑一个常见场景： 并行采样（Parallel Sampling） ，即用同一个Prompt（前缀）生成多个不同的续写。

传统方法 ：每个续写都需要独立存储一份完整的KV-Cache，包括完全相同的前缀部分。如果Prompt很长，并行度很高，内存会瞬间爆炸。

PagedAttention的优雅之道 ：

共享前缀 ：所有从同一个Prompt生成的序列，它们的块表在初始阶段会指向 同一组 存储着Prompt KV-Cache的物理Block。系统会通过**引用计数（Reference Count）**来追踪有多少个序列正在共享这些Block。

按需分配 ：当某个序列开始生成与其它序列不同的Token时，系统并不会复制整个历史。它只会 新分配一个Block 来存储这个新的、独有的KV对，并只更新该序列自己的块表。

零冗余 ：共享的前缀部分在内存中永远只存一份，直到没有任何序列引用它时才会被回收。

这个机制将并行任务的内存成本从“乘法”关系变成了“加法”关系，极大地节省了显存。

3. 灵活的请求调度：高级交通指挥

基于Block的内存管理，也为上层的请求调度器赋予了前所未有的灵活性。

高效抢占与交换 (Preemption & Swapping) ：当高优先级的请求到来但显存不足时，调度器可以立刻暂停一个低优先级的请求。由于其KV-Cache是由独立的Block组成的，系统可以非常快速地将这些Block 交换（Swap）到我们之前在 initialize_cache 看到的CPU缓存 中，从而瞬间释放出宝贵的GPU显存。这个过程远比操作一个巨大的、连续的KV-Cache要轻量和快速得多。

结论

让我们再次总结这段从朴素到精密的进化之旅：

KV-Cache 是一个基础但关键的加速技术。它通过 缓存 解决了自回归生成中的重复计算问题，是所有现代LLM推理的必备组件。但其原始的 连续内存 分配方式，如同“胶装书”，造成了严重的内存浪费。

PagedAttention 是一场内存管理的革命。它借鉴操作系统的 分页 思想，将KV-Cache打散成由 块表 管理的 Block ，如同“活页夹”，从根本上解决了内存碎片问题。

vLLM的工程实践 则向我们展示了如何通过 高度抽象的规格定义（Spec） 、 分层缓存 和 分组管理 等软件工程思想，将这一革命性理念打造成一个健壮、高效、可扩展的顶级推理框架。

理解了从KV-Cache到PagedAttention，再到vLLM的具体实现，我们不仅能明白LLM高效推理的核心原理，更能体会到理论思想与卓越工程实践结合所能释放出的巨大威力。正是这些在底层默默付出的“英雄”，才让我们能够享受到今天如此流畅和强大的AI体验。

posted @
2025-07-11 15:15
SIo_2
阅读( 1489 )
评论( 0 )

收藏
举报

刷新页面 返回顶部

公告

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3
