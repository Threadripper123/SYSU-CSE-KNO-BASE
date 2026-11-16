# 用 NVFP4 KV 缓存优化长上下文与大 batch 推理

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　6,624 字符　｜　85 个片段　｜　更新于 2026-11-13
来源: https://developer.nvidia.cn/blog/optimizing-inference-for-long-context-and-large-batch-sizes-with-nvfp4-kv-cache

# https://developer.nvidia.cn/blog/optimizing-inference-for-long-context-and-large-batch-sizes-with-nvfp4-kv-cache

来源: https://developer.nvidia.cn/blog/optimizing-inference-for-long-context-and-large-batch-sizes-with-nvfp4-kv-cache
抓取方式: read_page

DEVELOPER

首页
博客
论坛
论坛 (英文)
文档
下载
培训

搜索

加入

Related Resources

智能体/生成式 AI

English 中文

使用 NVFP4 KV 缓存优化大批次与长上下文推理

2025年 12月 8日

作者： Eduardo Alvarez, Wei-Ming Chen 和 Huizi Mao

点赞

W

W

Email

量化是大规模推理中的关键手段之一。通过降低权重、激活值和KV缓存的精度，可以有效减少内存占用和计算开销，从而显著提升推理吞吐量、降低延迟，并支持更长的上下文长度。

本博客介绍了 NVFP4 KV 缓存量化技术，这是一种专为 NVIDIA Blackwell 架构 GPU 设计的新型 KV 格式，可显著提升推理性能。NVFP4 能将 KV 缓存的显存占用最多降低 50%，有效实现上下文容量翻倍，从而支持更大的批处理规模、更长的序列长度以及更高的缓存命中率。在代码生成、知识问答和长上下文基准测试中，该技术带来的精度损失约为 1%。

在接下来的章节中，我们将探讨这一优化如何为推理工作负载带来显著收益，并进一步强化 NVIDIA 协同设计堆栈的整体优势。

什么是 KV 缓存？

大语言模型（LLM）依赖于自回归机制，逐个生成token，每一步都基于之前所有已生成的token进行预测。这一机制能够充分利用序列的完整上下文信息，正是LLM在自然语言建模任务中表现优异的关键所在。然而，这种生成方式也带来了显著的计算效率问题：每次生成新token时，模型都会重复计算此前所有token对应的注意力投影（即键值张量），造成计算资源的冗余消耗。

下图1展示了使用与不使用KV缓存时注意力计算的简化示意图。由于注意力机制中先前的token无法关注未来的token，其注意力值会被屏蔽，因此所有历史token（包括原始输入序列）对应的键值向量始终保持不变。这意味着在生成每个新token时，若重复计算这些固定的键值向量并重新执行相关的矩阵乘加（MMA）运算，将造成不必要的计算开销，是一种冗余操作。<!–

图1中的GIF展示了键值缓存如何在自回归Transformer中通过自注意力机制减少计算量。上方面板标注为“无KV缓存”，显示在每个新步骤中，模型需重新计算此前所有token的查询、键、值以及完整的注意力输出。下方面板标注为“使用KV缓存”，表明仅需重新计算当前token的查询，而所有历史的键和值则直接从缓存中读取，从而使注意力计算和输出矩阵显著缩小，避免了重复计算。

引入 KV 缓存旨在缓解因反复为先前已处理的每个 token 重新计算键和值向量所带来的计算瓶颈。通过将这些键（K）和值（V）张量缓存一次并在后续注意力计算中直接复用，不仅减少了重复计算，还降低了内存占用和带宽开销。在实际应用中，缓存通常由一个固定大小的内存池支持，如图 2 所示。

图2展示了通过传入的tokens查询KV缓存（即K/V张量的固定内存池）的过程；当缓存命中时，可复用已存储的值以减少计算量，而缓存未命中则可能触发K/V的重新计算，并在达到内存限制时引发数据驱逐。

当缓存池满时，KV 缓存管理器会清除部分旧的上下文内容。若后续请求引用了已被逐出的 Span，系统将发生缓存缺失，不得不重新计算相应的 K/V 张量。因此，实际的性能提升取决于缓存命中率：较高的命中率能够保留预期的计算节省，而较低的命中率则会使模型重新陷入 KV 缓存本应避免的重复计算路径。

在推理过程中，该缓存会在两个不同阶段被填充和使用。在预填充阶段，模型会处理整个输入序列，通过执行大规模且高度并行的矩阵乘积累加（MMA）运算来计算注意力，并将所有输入 token 对应的键和值向量存储到 KV 缓存中。随后，模型进入解码阶段，逐个生成新的 token。在每一步解码中，模型需完成一次完整的前向传播，此时注意力模块从 KV 缓存中读取此前所有 token 的键和值向量，计算当前 token 的键和值向量，并将其追加至缓存中，供后续解码步骤重复使用。

使用 NVFP4 优化 KV 缓存

优化 KV 缓存性能的一个新机遇是通过 NVFP4 与 NVIDIA TensorRT Model Optimizer 实现的。该功能支持将 KV 缓存从原始的 16 位精度量化至 4 位，从而提升效率。

量化 KV 缓存并非新概念，FP8 KV 缓存已在生产环境中得到广泛应用。然而，随着模型规模和推理部署规模的持续扩大，KV 缓存在预填充和解码阶段仍可能引发显著的性能瓶颈。通过量化 KV 缓存，可以有效缓解推理流程中多个组件的压力，从而在计算、内存容量和内存带宽方面带来积极影响。

显存容量: 相比 FP8 KV 缓存，NVFP4 KV 缓存将 KV 缓存的显存占用降低约 50%，从而支持更长的上下文长度、更大的批量大小以及更高的用户并发能力。

显存带宽: 在解码阶段，KV 缓存需频繁进行读写操作，对显存带宽带来较大压力；而更小的 KV 缓存可有效减少显存带宽的消耗。

当前 NVFP4 KV 缓存的实现要求在执行注意力计算和上下文矩阵运算之前，先将值从 NVFP4 反量化为 FP8。新生成 token 的键值向量会被量化为 NVFP4，然后追加到 KV 缓存中（如图 3 所示）。

图 3。KV 缓存驱动的注意力流，展示了量化与去量化在推理过程中的具体位置。

Model Optimizer 中的 quantize API 可用于执行 训练后量化（PTQ） 或 量化感知训练（QAT） 。在 PTQ 或 QAT 过程中，若要启用 NVFP4 KV 缓存，可使用相同的 quantize API，仅需调整量化配置即可。

以下代码片段配置了模型，支持在 FP8 权重和激活的基础上将 KV 缓存量化为 NVFP4。此外，为了充分利用 4 位数学运算的优势，可将模型权重通过将 quant_cfg 替换为 mtq.NVFP4_DEFAULT_CFG 进一步压缩为 NVFP4。

# configure fp8 quantization and fp4 for KV cache
quant_cfg = mtq.FP8_DEFAULT_CFG
quant_cfg["quant_cfg"].update(mtq.NVFP4_KV_CFG["quant_cfg"])

# Define forward loop for calibration with
def forward_loop(model):
for data in calib_set:
model(data)

# Quantize the modelmodel = mtq.quantize(model, quant_cfg, forward_loop)

# Model is ready for Post Training Quantization (PTQ) deployment

# (Optional) Quantization-aware training (QAT)
Train quantized model further for improving accuracy
# adjust training parameters, e.g., lr, schedule, epochs
# HuggingFace and Megatron models supported
train(model, train_loader, optimizer, scheduler,...)

KV 缓存如何影响性能

如上所述，KV缓存通过占用内存避免了对已处理token的重复计算。与当前标准的FP8 KV缓存相比，将KV缓存压缩为NVFP4可使内存开销降低50%，同时将上下文容量提升一倍，从而使模型能够支持更长的推理上下文。这一优化特别有利于需要利用教科书规模数据源或进行深度推理的应用场景，这些场景通常会迅速耗尽传统KV缓存的内存预算。

更高的命中率可节省预填充计算

在预填充阶段，延迟显著受到 KV 缓存中已驻留的请求上下文数量的影响。NVFP4 通过更高的有效缓存命中率改善了这一问题，其 4 位存储占用使设备能够保留约两倍于 FP8 的上下文数据。这不仅减少了缓存驱逐，还能保留更长跨度的已处理 token 信息。当模型可直接检索这些 KV 条目而无需重新计算时，预填充过程的停机时间得以减少，持续输入吞吐量相应提升，从而使首个 token 的生成延迟（TTFT）最多缩短至原来的三分之一。

图4显示，与FP8 KV缓存相比，NVFP4 KV缓存在缓存命中率上提升达20%，同时延迟最高可降低至原来的三分之一，展现出显著的性能优势，尤其在每GPU缓存显存增加的情况下更为明显。分析基于Qwen3-Coder-480B-A35B模型进行。

随着 KV 缓存的增大，其能够容纳更多的 K/V 张量，从而自然提升缓存命中率。这将产生一种稳定效应，即 NVFP4 与 FP8 在延迟和命中率上的差距会逐渐缩小（如上图 4 所示），但该效应在很大程度上取决于具体模型和上下文长度。然而，持续增长且未经优化的 KV 缓存会不断消耗更多的 HBM 资源。NVFP4 能显著提升 KV 缓存的 HBM 利用效率，释放更多内存预算用于模型权重，并结合堆栈中其他协同设计的组件（如 NVLink、内核优化和 Wide Expert Parallelism），进一步增强整体系统的性能与效率优势。

NVFP4 KV 缓存如何影响准确性

我们观察到，在 LiveCodeBench 、 MMLU-PRO 、 MBPP 和 Ruler 64K 等现代大语言模型基准测试中，与 BF16 和 FP8 基准相比，精度损失不到 1%。特别是在 LiveCodeBench 上表现出的近乎一致的性能，表明量化方法有效保留了复杂的多步代码生成能力，而这类任务中微小的数值误差往往会导致语法、编译或逻辑错误。

同样地，Ruler 64K 保持了优异的性能，验证了所提格式在长达 64K-token 序列中进行长上下文推理的稳健性，即便在量化噪声可能累积的情况下依然表现可靠。结果表明，在处理具有挑战性的代码任务和长上下文工作负载时，该格式在不牺牲端到端功能的前提下，显著提升了效率。

图5展示了在Qwen3 480B A35B上对FP16、FP8和NVFP4 KV缓存精度的基准测试结果，表明FP8与NVFP4在编码、知识理解及长上下文任务中的表现与FP16非常接近。

另一个关键见解体现在KV缓存量化中 NVFP4 与 MXFP4 的对比表现。图6展示了BF16、FP8、NVFP4 和 MXFP4 对 MMLU 模型准确率的影响。在测试模型 Llama 3.3 70B 上，我们发现，与 MXFP4 相比，采用NVFP4 进行KV缓存时，模型准确率提升了5%。这一优势源于NVFP4更精细的块级缩放机制，以及更高精度的 E4M3 FP8 缩放因子，二者协同作用，有效降低了反量化过程中的量化误差。

图6对比了FP8、NVFP4和MXFP4的KV缓存格式，结果显示FP8和NVFP4在MMLU精度上显著优于MXFP4。

展望未来

NVFP4 KV 缓存是 NVIDIA 推出的推理堆栈在软硬件协同设计方面的又一实质性进展。随着生态系统的不断完善，该技术可与 NVIDIA Dynamo 中的 KV+ 感知路由和卸载机制相结合，并进一步集成到 NVIDIA TensorRT™ LLM 的 Wide+ EP 架构中，利用大规模专家并行技术，提升大型 MoE 模型部署的资源利用率。

在硬件层面，通过更精细的 KV 缓存优化，可充分释放 NVL72 纵向扩展能力与 NVLink 架构的潜力，从而有效支持多代理推理和长上下文深度推理等高负载任务。结合这些组件的优势，我们能够在保持模型准确性的同时，为规模更大的专家模型、更长的序列处理以及更高的并发请求提供高效服务。

建议利用 Model Optimizer 的代码示例和 notebook 作为自定义量化工作流的基础方案，以启动这些技术的应用。

Kai Xu、Shengliang Xu、Tian Zheng 和 Asma Kuriparamabil Thekkumpate 为本博客所介绍的工程工作做出了重要贡献。

点赞

标签

数据中心/云端 | 智能体/生成式 AI

关于作者

Eduardo Alvarez 是 NVIDIA 的高级技术营销工程师，专注于大规模 AI 推理、工作负载性能优化、TCO 分析和 AI 工程支持。他在 AI 系统工程、模型优化和 GPU 加速计算方面拥有深厚的背景，将前沿研究转化为开发者和企业团队的实用见解。加入 NVIDIA 之前，Eduardo 曾在领先的半导体和能源公司担任技术职务，帮助推动 AI 在各行各业中的应用。他拥有德克萨斯农工大学 (Texas A&M University) 的地球物理工程学位。

查看 Eduardo Alvarez 所有文章

Wei-Ming Chen 是 NVIDIA 深度学习算法和软件团队的高级工程师，专注于高效的深度学习和模型部署。在加入 NVIDIA 之前，他曾是麻省理工学院的博士后研究员，师从宋教授。Wei - Ming 拥有国立台湾大学计算机科学博士学位、硕士学位和学士学位。

查看 Wei-Ming Chen 所有文章

Huizi Mao 是 NVIDIA 深度学习算法和软件团队的技术负责人和高级工程师，领导 TensorRT 模型优化器的整体开发。Huizi 通过收购 OmniML，股份有限公司加入 NVIDIA ，他是该公司的联合创始人和 CTO。他在斯坦福大学获得电气工程博士学位，在清华大学获得学士学位。

查看 Huizi Mao 所有文章

相关文章

在 NVIDIA Blackwell GPU 上使用 FLUX 3 Image Klein 加速图像生成

在 NVIDIA Blackwell GPU 上使用 FLUX 3 Image Klein 加速图像生成

使用 NVIDIA DSX OS 配置、认证和运行 Kubernetes GPU 集群

使用 NVIDIA DSX OS 配置、认证和运行 Kubernetes GPU 集群

NVIDIA 协同设计的五种方式可更大限度地提高 AI 工厂中每个字节的价值

NVIDIA 协同设计的五种方式可更大限度地提高 AI 工厂中每个字节的价值

NVIDIA 如何使用 AI 智能体简化 AI 工厂运营

NVIDIA 如何使用 AI 智能体简化 AI 工厂运营

NVIDIA 工厂运营 Blueprint 如何协调 AI 智能体调查生产问题

NVIDIA 工厂运营 Blueprint 如何协调 AI 智能体调查生产问题

Theme

Light

System

Dark

W

W

Email
