# 什么是 KV Cache（Key-Value Cache）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　21,197 字符　｜　441 个片段　｜　更新于 2026-11-13
来源: https://blog.csdn.net/u013172930/article/details/147387537

# https://blog.csdn.net/u013172930/article/details/147387537

来源: https://blog.csdn.net/u013172930/article/details/147387537
抓取方式: read_page

首页

博客

下载

学习

社区

模型市场

AI搜索

AtomGit

InsCode

技术会议

墨衍
MoGrow

收藏

历史

会员中心

创作中心

什么是 KV Cache（Key-Value Cache）

原创
最新推荐文章于&nbsp;2026-10-10 13:36:06&nbsp;发布
1w 阅读

AI写同款

收录于

LLM

当前文章被收录于：

LLM

105
篇文章

14
人学习

大模型相关的技术博文

订阅专栏
查看详情

当前文章被以下社区和专栏收录：

彬彬侠

码龄13年

墨衍会员
·
AI 创作全网分发

墨衍智能分发
本文由作者通过墨衍一键同步至各平台

1 分发平台
1w 累计阅读
1.1w+ 平均单平台

已同步平台

CSDN
微信公众号
微博
知乎
掘金
百家号
博客园
抖音
小红书

你的文章也可以这样分发
墨衍会员 ¥399 起/年，写一次发全网

我也要 →

1万+粉丝 · 1707篇原创

关注

kv cache 原理、参数量、代码详解
kv cache 一句话来说就是把。训练的时候不需要保存。 推理 解码生成时都是自回归auto - regressive的方式，也就是每次生成一个token，都要依赖之前token的结果。如果没生成一个token的时候乘以W_K,W_V这俩参数矩阵要对所有token都算一遍，代价非常大，所以缓存起来就叫 kv cache 。举个例子，假如prompt=&ldquo;The largest city of China is&rdquo;，输入是6个tokens，返回是"Shang Hai"这两个tokens。

阅读详情

AI摘要

KV Cache 是大模型推理常用优化技术，用于加速自回归生成任务。它通过缓存中间计算结果避免重复计算，降低计算量和延迟。介绍了其工作原理、优缺点，优点是降低计算量、加速推理且易实现，缺点是有内存开销等，还提及优化方法和应用场景。

KV Cache（Key-Value Cache）是大模型（特别是Transformer架构）在推理过程中常用的一种优化技术，主要用于加速自回归生成任务（如文本生成）中的计算效率。它通过缓存中间计算结果（键和值的张量），避免重复计算，从而显著降低推理的计算量和延迟。以下是对KV Cache的详细介绍，包括其背景、原理、实现方式、优缺点以及应用场景。

1. 背景

在Transformer模型（如GPT系列、LLaMA等）中，自回归生成是一个逐 token 生成的过程。每次生成一个新 token，模型需要基于当前输入序列重新计算整个注意力机制（Attention）的键（Key）和值（Value）。具体来说：

Transformer 的注意力机制依赖于查询（Query）、键（Key）和值（Value）之间的交互。
对于自回归任务，输入序列会逐渐增长（每次增加一个新 token），但前缀序列（之前生成的 token）是不变的。
如果每次都重新计算整个序列的键和值，会导致大量重复计算，尤其在长序列生成时，计算开销会显著增加。

KV Cache 的核心思想是： 将之前计算过的键和值缓存下来，仅计算新增 token 的键和值，然后复用缓存结果 ，从而避免重复计算。

2. KV Cache 的工作原理

2.1 Transformer 注意力机制回顾

在 Transformer 的自注意力机制中，对于一个输入序列 X=[x1,x2,...,xt]X = [x_1, x_2,..., x_t] X = [ x 1 ​, x 2 ​,..., x t ​ ] ，每个 token 的表示会通过线性变换生成查询、键和值：

查询： Q=XWQQ = XW_Q Q = X W Q ​
键： K=XWKK = XW_K K = X W K ​
值： V=XWVV = XW_V V = X W V ​

注意力计算公式为：

Attention(Q,K,V)=softmax(QKTdk)V
\text{Attention}(Q, K, V) = \text{softmax}\left(\frac{QK^T}{\sqrt{d_k}}\right)V
Attention ( Q, K, V ) = softmax ( d k ​ ​ Q K T ​ ) V

其中：

QQ Q 、 KK K 、 VV V 是形状为 (t,dk)(t, d_k) ( t, d k ​ ) 的张量（ tt t 为序列长度， dkd_k d k ​ 为每个头的维度）。
QKTQK^T Q K T 的结果是一个 (t,t)(t, t) ( t, t ) 的注意力分数矩阵。

在自回归生成中，模型每次只生成一个新 token，输入序列从 [x1,...,xt][x_1,..., x_t] [ x 1 ​,..., x t ​ ] 扩展到 [x1,...,xt,xt+1][x_1,..., x_t, x_{t+1}] [ x 1 ​,..., x t ​, x t + 1 ​ ] 。如果每次都重新计算整个序列的 KK K 和 VV V ，会非常低效，因为前 tt t 个 token 的 KK K 和 VV V 是完全相同的。

2.2 KV Cache 的作用

KV Cache 的核心是：

缓存每一层的 KK K 和 VV V ：在 Transformer 的每一层（多头注意力机制）中，将计算得到的 KK K 和 VV V 张量存储到内存中。
增量计算 ：当生成新 token 时，仅计算新 token 的 QQ Q 、 KK K 、 VV V ，并将新 token 的 KK K 和 VV V 追加到缓存中。
复用缓存 ：在注意力计算中，使用缓存的 KK K 和 VV V ，结合新 token 的 QQ Q ，计算注意力输出。

2.3 工作流程

假设当前序列为 [x1,x2,...,xt][x_1, x_2,..., x_t] [ x 1 ​, x 2 ​,..., x t ​ ] ，KV Cache 的工作流程如下：

初始化 ：
在生成第一个 token 时，计算整个序列的 QQ Q 、 KK K 、 VV V 。
将 KK K 和 VV V 存储到 KV Cache 中（通常按层和注意力头组织）。

生成新 token ：
输入新 token xt+1x_{t+1} x t + 1 ​ ，计算其 Qt+1Q_{t+1} Q t + 1 ​ 、 Kt+1K_{t+1} K t + 1 ​ 、 Vt+1V_{t+1} V t + 1 ​ 。
从 KV Cache 中读取之前所有 token 的 KK K 和 VV V ，构成 Kcache=[K1,K2,...,Kt]K_{\text{cache}} = [K_1, K_2,..., K_t] K cache ​ = [ K 1 ​, K 2 ​,..., K t ​ ] 和 Vcache=[V1,V2,...,Vt]V_{\text{cache}} = [V_1, V_2,..., V_t] V cache ​ = [ V 1 ​, V 2 ​,..., V t ​ ] 。
将 Kt+1K_{t+1} K t + 1 ​ 和 Vt+1V_{t+1} V t + 1 ​ 追加到 KV Cache。
计算注意力：

Attention(Qt+1,[Kcache,Kt+1],[Vcache,Vt+1])
\text{Attention}(Q_{t+1}, [K_{\text{cache}}, K_{t+1}], [V_{\text{cache}}, V_{t+1}])
Attention ( Q t + 1 ​, [ K cache ​, K t + 1 ​ ], [ V cache ​, V t + 1 ​ ])

重复 ：继续生成下一个 token，重复上述步骤。

2.4 缓存的存储结构

按层和头组织 ：KV Cache 通常为每个 Transformer 层和每个注意力头维护单独的缓存。
张量形状 ：
对于一个序列长度为 tt t 、批大小为 bb b 、注意力头数为 hh h 、头维度为 dkd_k d k ​ ，缓存的 KK K 和 VV V 形状为 (b,h,t,dk)(b, h, t, d_k) ( b, h, t, d k ​ ) 。
随着序列增长， tt t 会不断增加。

存储位置 ：KV Cache 通常存储在 GPU 显存中，以保证快速访问。

3. KV Cache 的优点

显著降低计算量 ：
没有 KV Cache 时，每次生成新 token 需要重新计算整个序列的 KK K 和 VV V ，计算复杂度为 O(t2)O(t^2) O ( t 2 ) （注意力机制的二次复杂度）。
使用 KV Cache 后，仅计算新 token 的 KK K 和 VV V ，计算复杂度降为 O(t)O(t) O ( t ) ，极大地提高了推理效率。

加速推理 ：
尤其在长序列生成任务中（如对话、文章生成），KV Cache 可以大幅减少推理时间。

易于实现 ：
KV Cache 的实现相对简单，只需在推理代码中添加缓存逻辑，无需修改模型结构。

4. KV Cache 的缺点与挑战

内存开销 ：
KV Cache 需要存储每一层、每个注意力头的 KK K 和 VV V ，内存需求随序列长度 tt t 线性增长。
对于长序列或多层大模型，显存占用可能成为瓶颈。例如：
一个 70B 模型，32 层，16 个注意力头，序列长度 2048，头维度 128，单精度浮点（FP16），KV Cache 占用约为：

32×16×2048×128×2×2(bytes)≈8.5GB
32 \times 16 \times 2048 \times 128 \times 2 \times 2 \text{(bytes)} \approx 8.5 \text{GB}
32 × 16 × 2048 × 128 × 2 × 2 (bytes) ≈ 8.5 GB
对于更长序列或更大模型，显存需求会更高。

内存管理复杂性 ：
需要高效管理缓存的分配和释放，尤其在多用户或多轮对话场景下。
如果缓存未及时清理，可能导致显存泄漏。

对序列长度的依赖 ：
当序列长度超过一定限制（受显存限制），需要截断缓存或使用其他优化技术（如窗口注意力）。

不支持某些任务 ：
KV Cache 主要适用于自回归生成任务，对于非自回归任务（如机器翻译的编码-解码模型）效果有限。

5. 优化与扩展

为了解决 KV Cache 的局限性，业界提出了一些优化方法：

量化与压缩 ：
对 KV Cache 应用量化技术（如 INT8 或 4-bit 量化），减少内存占用。
使用稀疏化或低秩分解压缩 KK K 和 VV V 。

滑动窗口注意力 ：
限制 KV Cache 的大小，只保留最近 NN N 个 token 的 KK K 和 VV V ，适用于长序列生成。

多查询注意力（Multi-Query Attention） ：
减少注意力头数，降低 KV Cache 的存储需求。

硬件加速 ：
利用 GPU 或 TPU 的内存管理优化（如显存分片），提高缓存访问效率。

分布式推理 ：
在多 GPU 环境中，将 KV Cache 分片存储到不同设备上。

6. 应用场景

KV Cache 广泛应用于以下场景：

对话系统 ：如 chatbot、虚拟助手，生成长对话时复用历史 token 的计算结果。
文本生成 ：如文章生成、代码补全，处理长序列生成任务。
实时推理 ：在低延迟场景（如在线翻译、语音交互）中加速推理。
大模型部署 ：在生产环境中优化 Transformer 模型的推理性能。

7. 实现示例（伪代码）

以下是一个简化的 KV Cache 实现伪代码（基于 PyTorch）：

class KVCache :
def __init__ ( self, num_layers, num_heads, head_dim ) :
self. cache = {
layer : { 'K' : [ ], 'V' : [ ] }
for layer in range ( num_layers )
}
self. num_heads = num_heads
self. head_dim = head_dim

def update ( self, layer, K, V ) :
"""将新 token 的 K 和 V 追加到缓存"""
self. cache [ layer ] [ 'K' ]. append ( K ) # 形状: (batch, heads, 1, head_dim)
self. cache [ layer ] [ 'V' ]. append ( V ) # 形状: (batch, heads, 1, head_dim)

def get ( self, layer ) :
"""获取缓存的 K 和 V"""
K = torch. cat ( self. cache [ layer ] [ 'K' ], dim = 2 ) # 形状: (batch, heads, seq_len, head_dim)
V = torch. cat ( self. cache [ layer ] [ 'V' ], dim = 2 ) # 形状: (batch, heads, seq_len, head_dim)
return K, V

def attention_with_kv_cache ( Q, K, V, kv_cache, layer ) :
"""使用 KV Cache 的注意力计算"""
# 更新缓存
kv_cache. update ( layer, K, V )
# 获取完整的 K 和 V
K_cached, V_cached = kv_cache. get ( layer )
# 计算注意力
scores = torch. matmul ( Q, K_cached. transpose ( - 1, - 2 ) ) / sqrt ( Q. size ( - 1 ) )
weights = torch. softmax ( scores, dim = - 1 )
output = torch. matmul ( weights, V_cached )
return output

8. 总结

KV Cache 是一种简单而高效的优化技术，通过缓存 Transformer 模型中注意力机制的键和值，显著降低了自回归生成任务的计算开销。它在大模型推理服务中几乎是标配技术，尤其在对话系统、文本生成等场景中应用广泛。然而，KV Cache 也带来了内存管理的挑战，需要结合量化、滑动窗口等技术进一步优化。

标签

#KV Cache
#推理优化
#Transformer
#pytorch
#python

#大模型

版权声明：本文为博主原创文章，遵循 CC 4.0 BY-SA 版权协议，转载请附上原文出处链接和本声明。

大模型 &mdash;&mdash; 推理 优化 &mdash;&mdash; KV Cache
在本文中，我们将详细介绍 KV Cache ，这是一种 大模型 推理 加速的方法。正如其名称所示，该方法通过缓存Attention中的K和V来实现 推理 优化 。

阅读详情

相关推荐

换一换

KV Cache 详解：新手也能理解的 LLM 推理 加速技巧

（Autoregressive）生成的，即每次生成一个 token，然后将其作为输入继续生成下一个 token。：如果序列太长，超过了 KV Cache 的存储限制，就需要丢弃最早的 tokens（类似滑动窗口）。时，每次只会生成一个新 token，然后再把这个 token 作为输入继续生成下一个 token。，只缓存计算过的 K,VK, V，每次只计算新 token 的 K,VK, V，从而提升 推理 效率。：如果输入的 prompt 发生变化，之前的 KV Cache 不能复用，必须重新计算。

Harry的博客

7368

KV Cache 到底是什么？一文讲透 大模型 推理 加速原理

Q KV Token:北京Q_beijingK_beijingV_beijingQ：我想找谁K：我是哪个信息V：我携带什么内容Q = 查询条件K = 索引V = 数据 KV 名称的来源。 KV Cache 本质上是 Transformer Attention 的结果缓存机制，用空间换时间，避免历史 Token 的重复计算。重复计算问题已经算过的 K 和 V不要再算第二次决定模型能力 KV Cache 决定 推理 效率GPU 是 推理 系统的核心。

qq_36863796的博客

1423

Transformer 推理 加速方法 - KV 缓存 ( KV Cache )

可以进行缓存后续复用；在Decoder中 推理 过程中，同样可以缓存计算出来的。在 推理 进程中与训练不同， 推理 进行时上下文输入Encoder后计算出来的。所以这里Decoder每次计算出来一组新的。向量都跟之前向量一起进行缓存，后续也可以重复复用。并进行缓存，后续 推理 都不用再计算了；，但计算Decoder过程中需要之前时刻。减少重复计算，这里注意在输入是。时刻通过输入的一个词计算出来。是固定不变的，对于这里的。计算时，输入仍需要前面。是Encoder，在。

分享机器学习、深度学习、分布式计算、MLSys相关领域知识

6053

KV - Cache 详解

本文探讨了 KV 缓存的概念、应用以及它在自注意力机制中 优化 的特定计算。

发几十块的吧

1万+

Transformer 中的 kv cache 算法说明和代码解释

图解 kv cache

weixin_55982578的博客

485

【人工智能】结合代码通俗讲解 Transformer 推理 性能 优化 技术： KV Cache

KV Cache 是 Transformer 推理 性能 优化 的一项重要工程化技术，各大 推理 框架都已实现并将其进行了封装（例如 transformer s库 generate 函数已经将其封装，用户不需要手动传入past_ key _ value s）并默认开启（config.json文件中use_ cache =True）。本文尝试打开封装分析该技术内部实现，希望对大家有所帮助，文中如有纰漏，欢迎指正。作者：Young。

AI天才研究院

2万+

KV Cache （ Key - Value Cache ）原理和应用

KV Cache 是 Transformer 推理 的基石技术，通过缓存历史K/V向量解决了自回归生成的效率瓶颈。随着模型规模扩大和应用场景复杂化， KV Cache 的 优化 （如压缩、动态管理）仍是提升 大模型 落地能力的关键方向。

jerwey的博客

1204

Vivado Schematic视图实战：从RTL分析到布局布线，如何用它快速定位设计问题（附2019.1版操作）

本文深入解析Vivado Schematic视图在FPGA设计中的关键作用，从RTL分析到布局布线全流程，帮助工程师快速定位设计问题。通过三重视图对比、高效调试四步法和高级技巧，提升问题排查效率，缩短调试时间。文章结合2019.1版操作指南和实战案例，展示如何利用Schematic视图解决典型设计问题。

weixin_30591551的博客

489

lmdeploy量化之 Key - Value ( KV ) Cache 量化

摘要： KV Cache 量化是一种 优化 大模型 推理 显存占用的技术，通过量化 transformer 结构中的 KV 缓存值而非模型参数，有效提升并发性能。LMDeploy支持int8（精度无损）和int4（精度可接受）两种 KV 量化方式，无需校准数据，兼容Volta及以上架构显卡。实验显示，在Llama2 - 7B模型上应用int8/int4 KV 量化可使RPS分别提升30%和40%。使用LMDeploy时只需设置quant_policy参数（4为int4，8为int8），即可轻松应用于离线 推理 和服务部署场景。该技术特别适合

weixin_45526015的博客

352

ArcGIS实战：手把手教你用DEM和灾害点数据，5步搞定地质灾害风险区划图

本文详细介绍了使用ArcGIS进行地质灾害风险评估的完整流程，从DEM和灾害点数据出发，通过五步标准化操作生成专业风险区划图。重点讲解了信息量模型的应用，帮助读者掌握GIS技术在地质灾害易发性评价中的实践方法，提升风险评估效率与准确性。

weixin_30696427的博客

426

AVM环视算法实战：从3D碗型投影到多场景模式切换的实现与 优化

本文深入探讨了AVM环视算法在3D碗型投影和多场景模式切换中的实现与 优化 。通过分析3D碗型投影的核心原理和动态视角调节技术，展示了如何实现从超广角模式到转向模式的无缝切换。文章还分享了性能 优化 实战经验，包括实时性提升和图像拼接质量 优化 ，为开发者提供了宝贵的AVM系统开发指南。

weixin_42573113的博客

171

LLM KV Cache 压缩技术解析：Multi - Head Key - Value 共享方案

随着大语言模型（LLM）在生成任务中的广泛应用， 推理 阶段的内存瓶颈愈发显著。特别是在长序列场景下， Transformer 架构中的 Key - Value （ KV ）缓存成为显存占用的主要来源。本文将深入剖析一种高效的 KV Cache 压缩技术&mdash;&mdash;，从理论推导到生产级实践的实现方案。

zhangzhentiyes的博客

1378

【HSPiP应用指南】从理论到实践：如何利用汉森溶解度参数 优化 配方设计

本文深入解析了汉森溶解度参数（HSP）理论及其在配方设计中的核心价值，并详细介绍了如何利用HSPiP软件工具从理论走向实践。文章通过匹配溶解度参数、计算汉森距离（Ra），系统阐述了如何解决溶剂选择、颜料分散稳定性及扩散行为预测等关键问题，并结合界面气相色谱（IGC）等实验方法提升预测精度，旨在帮助配方师实现配方设计的可视化、可量化与高效 优化 。

weixin_29305313的博客

1649

大模型 推理 kv _ cache 缓存

推理 时保留past_ key _ value 。优点：减少计算量，提高 推理 速度。

weixin_40777649的博客

2023

从ISO 17987到SAE J2602：LIN一致性测试标准全景解读与CANoe实现

本文深入解析了从ISO 17987到SAE J2602的LIN一致性测试标准技术演进，详细对比了两大标准体系的技术差异，并基于Vector CANoe平台演示了如何构建符合最新行业要求的自动化测试方案。文章重点介绍了SAE J2602标准的三大改进点，包括唤醒特性、传输层协议和诊断服务的增强测试要求，并提供了CANoe测试环境配置、测试模块集成及测试案例解析的实战指南。

app77的博客

336

LaTeX参考文献格式apsrev4 - 1，增加文献标题

APS的LaTeX参考文献格式apsrev4 - 1是隐藏标题的，如果想要出现标题，需要更改apsrev4 - 1.bst文件内容

在apsrev4 - 1.bst文件靠后部分，定义不同类型文献格式的地方，将article类型的内容替换为，

FUNCTION {article}
{ output.bibitem
format.authors
booktitle empty$ {
"author" output.check
}{ output } if$
author format.

loading1234567的博客

5032

【文献阅读】 Key, Value, Compress: A Systematic Exploration of KV Cache Compression Techniques

大语言模型（LLMs）在生成文本、图像和视频内容方面展现出卓越的能力。然而，随着上下文长度的增加，注意力机制的计算成本会随着令牌数量的增加而呈二次方增长，这带来了显著的效率挑战。本文对各种键值（ KV ）缓存压缩策略进行了分析，提供了一个全面的分类体系，根据其基本原理和实现技术对这些方法进行分类。此外，我们评估了它们对性能和 推理 延迟的影响，深入了解了它们的有效性。我们的研究结果突出了 KV 缓存压缩所涉及的权衡，以及它对处理长上下文场景的影响，为更高效的大语言模型实现铺平了道路。近年来，大语言模型

Toky_min的博客

1116

《深入浅出强化学习原理入门》学习笔记（三）机器人找金币应用实例

《深入浅出强化学习原理入门》学习笔记（三）MDP应用实例

lxs3213196的博客

3187

KV Cache

KV Cache 是一种 优化 大语言模型 推理 速度的技术，其核心原理是缓存已计算的 Key 和 Value 向量，避免重复计算。通过只计算当前token的Query向量，并复用缓存的 KV 向量，显著减少了计算量。Prefix Cache 进一步 优化 了这一机制，允许复用完全相同的prompt前缀对应的 KV 缓存。但当prompt前缀发生变化时，所有后续生成的Attention结果都会受影响，因此需要重新计算。这提示我们可能通过prompt顺序调整或同义转换来提升缓存复用率。该技术在提升LLM 推理 效率方面具有重要价值。

weixin_45207619的博客

193

YOLO26 网络结构逐模块解析：Backbone/Neck/Head

本文解析了YOLO26目标检测网络的架构设计，重点介绍其在Backbone、Neck和Head三个核心组件的创新 优化 。Backbone采用改进的CSP模块与轻量级注意力机制，提升特征提取效率；Neck使用改进的BiFPN结构实现高效多尺度特征融合；Head采用完全解耦设计简化回归流程。文章详细分析了各模块的技术演进、实现细节和设计思想，通过流程图和代码示例展示了YOLO26的整体架构和关键组件实现。该研究对模型 优化 、定制开发和硬件部署具有重要指导意义。

一键难忘的博客

1248

大中型超市收银系统 ( 可直接运行 )

光步前台收银系统系统功能：商品销售、商品退货、挂单、收银员权限、会员销售、单品取消、整单取消、商品查询、脱机销售、脱机数据上传，基本资料下载，开钱箱、小票打印、收款结算，支持多币种付款方式（现金、储值卡、礼券、微信等）,支持多种促销。运行环境：.net 4.0登陆信息用户名:1001 密码:123456内置商品信息，商品编码可输入00001000 - 00010000,测试效果相关功能键F5 结算 F9挂单 F11商品查询 F8退货 Insert单品取消 Del整单取消 F12开钱箱F2 会员 Ctrl+Z退出系统 Home 锁屏 相关源码待公开

KV Cache 中的 &ldquo;page attention&rdquo; 是什么

KV Cache （ Key - Value Cache ）是 Transformer 模型中用于加速 推理 的技术，通过缓存历史tokens的 Key 和 Value ，避免重复计算。在自注意力机制中，Query（Q）与缓存的 Key 进行点乘，再对缓存的 Value 加权求和，从而提升效率。Page attention则是 KV Cache 的 优化 策略，将 Key 和 Value 按页分块存储，减少无效数据搬运，提高内存访问效率。通过分页索引，模型可以快速定位相关页面，避免全量遍历，进一步加速 推理 。这种分页式存储不仅 优化 了内存和显存利用，还保持了注

ningyanggege的博客

1175

Outlook Express 6 ( OE6 ) for Win7/Win8/Win10

英文版 Outlook Express6.0， 兼容Win7/Win10/WinXP，解压就可用。

Kv cache

KV Cache 是 大模型 推理 加速的关键技术，通过缓存历史token的 Key - Value 矩阵避免重复计算。其工作原理是：在自回归生成过程中，将已计算的K/V矩阵存入缓存，后续生成时直接复用，显著减少计算量。但 KV Cache 会线性增加显存占用，导致长文本生成时出现OOM问题。DeepSeek的MLA技术通过压缩K/V矩阵有效缓解这一问题，将缓存体积大幅缩小，支持更长文本 推理 。 KV Cache 实现了计算效率与显存占用的平衡，是 优化 大模型 推理 性能的重要方案。

weixin_71184753的博客

948

IEM模型，地表粗糙度模拟

利用微波反演土壤水分时候，利用IEM模型可以实现对随机地表的模拟

计算机控制技术及应用

《计算机控制技术及应用》通过总结作者的科研成果和吸收国内外的先进理论、方法和技术，以计算机控制系统的网络化、开放化、智能化和集成化发展趋势为主线索，主要介绍计算机控制系统的基本概念和计算机控制系统的典型形式，计算机控制系统的数学描述与基本分析方法，以及数字PID控制器、最少拍控制系统、达林算法、状态变量设计法等设计方法及其各种改进算法。重点突出计算机控制系统输入输出通道接口的设计方法和典型控制系统的设计案例与实现方法等。

MHA还是GQA？StableLM - 3B - 4E1T的num_ key _ value _heads参数如何帮你节省显存

StableLM - 3B - 4E1T 是 Stability AI 开源的 30 亿参数语言模型，其 `config.json` 中的 **num_ key _ value _heads** 参数决定了模型使用 MHA 还是 GQA 注意力机制，直接影响 推理 时的 KV Cache 显存占用。这篇文章带你快速看懂这个参数，并告诉你如何用它省下显存。💡

## 一句话说清：MHA、GQA、MQA 到底是什么

gitblog_00805的博客

1096

大模型 推理 引擎vLLM ( 0 ) : KV Cache 原理

大模型 推理 引擎vLLM ( 0 ) : KV Cache 原理

C/C++、嵌入式开发、深度学习算法、模型部署与推理优化

973

TurboQuant：LLM 推理 KV Cache 压缩新利器&mdash;&mdash;3比特 Key +2比特 Value 如何实现近无损压缩

TurboQuant 是一款面向 LLM 推理 的 KV Cache 压缩开源工具，它把注意力机制中的 Key 压缩到 3 比特、 Value 压缩到 2 比特，实现近无损的 4.4 倍显存压缩，并原生集成 vLLM 与 Triton 加速内核。🚀

如果你在 大模型 推理 中常遇到"显存不够、上下文装不下、并发上不去"的困境，这篇文章将带你从零看懂 TurboQuant 的工作原理、实测数据和上手步骤。

gitblog_00807的博客

129

上一篇：

什么是 稀疏激活（Sparse Activation） 和 MOE（The Sparsely-Gated Mixture-of-Experts Layer，稀疏门控专家混合层）

下一篇：

什么是 PagedAttention

KV Cache 详解：新手也能理解的 LLM 推理 加速技巧

Harry的博客

03-28

7368

（Autoregressive）生成的，即每次生成一个 token，然后将其作为输入继续生成下一个 token。：如果序列太长，超过了 KV Cache 的存储限制，就需要丢弃最早的 tokens（类似滑动窗口）。时，每次只会生成一个新 token，然后再把这个 token 作为输入继续生成下一个 token。，只缓存计算过的 K,VK, V，每次只计算新 token 的 K,VK, V，从而提升 推理 效率。：如果输入的 prompt 发生变化，之前的 KV Cache 不能复用，必须重新计算。

KV Cache 到底是什么？一文讲透 大模型 推理 加速原理

qq_36863796的博客

06-16

1423

Q KV Token:北京Q_beijingK_beijingV_beijingQ：我想找谁K：我是哪个信息V：我携带什么内容Q = 查询条件K = 索引V = 数据 KV 名称的来源。 KV Cache 本质上是 Transformer Attention 的结果缓存机制，用空间换时间，避免历史 Token 的重复计算。重复计算问题已经算过的 K 和 V不要再算第二次决定模型能力 KV Cache 决定 推理 效率GPU 是 推理 系统的核心。

Transformer 推理 加速方法 - KV 缓存 ( KV Cache )

分享机器学习、深度学习、分布式计算、MLSys相关领域知识

07-12

6053

可以进行缓存后续复用；在Decoder中 推理 过程中，同样可以缓存计算出来的。在 推理 进程中与训练不同， 推理 进行时上下文输入Encoder后计算出来的。所以这里Decoder每次计算出来一组新的。向量都跟之前向量一起进行缓存，后续也可以重复复用。并进行缓存，后续 推理 都不用再计算了；，但计算Decoder过程中需要之前时刻。减少重复计算，这里注意在输入是。时刻通过输入的一个词计算出来。是固定不变的，对于这里的。计算时，输入仍需要前面。是Encoder，在。

KV - Cache 详解

发几十块的吧

09-18

1万+

本文探讨了 KV 缓存的概念、应用以及它在自注意力机制中 优化 的特定计算。

Transformer 中的 kv cache 算法说明和代码解释

weixin_55982578的博客

03-26

485

图解 kv cache

【人工智能】结合代码通俗讲解 Transformer 推理 性能 优化 技术： KV Cache

热门推荐

AI天才研究院

05-26

2万+

KV Cache 是 Transformer 推理 性能 优化 的一项重要工程化技术，各大 推理 框架都已实现并将其进行了封装（例如 transformer s库 generate 函数已经将其封装，用户不需要手动传入past_ key _ value s）并默认开启（config.json文件中use_ cache =True）。本文尝试打开封装分析该技术内部实现，希望对大家有所帮助，文中如有纰漏，欢迎指正。作者：Young。

KV Cache （ Key - Value Cache ）原理和应用

jerwey的博客

07-09

1204

KV Cache 是 Transformer 推理 的基石技术，通过缓存历史K/V向量解决了自回归生成的效率瓶颈。随着模型规模扩大和应用场景复杂化， KV Cache 的 优化 （如压缩、动态管理）仍是提升 大模型 落地能力的关键方向。

Vivado Schematic视图实战：从RTL分析到布局布线，如何用它快速定位设计问题（附2019.1版操作）

weixin_30591551的博客

04-17

489

本文深入解析Vivado Schematic视图在FPGA设计中的关键作用，从RTL分析到布局布线全流程，帮助工程师快速定位设计问题。通过三重视图对比、高效调试四步法和高级技巧，提升问题排查效率，缩短调试时间。文章结合2019.1版操作指南和实战案例，展示如何利用Schematic视图解决典型设计问题。

lmdeploy量化之 Key - Value ( KV ) Cache 量化

weixin_45526015的博客

11-18

352

摘要： KV Cache 量化是一种 优化 大模型 推理 显存占用的技术，通过量化 transformer 结构中的 KV 缓存值而非模型参数，有效提升并发性能。LMDeploy支持int8（精度无损）和int4（精度可接受）两种 KV 量化方式，无需校准数据，兼容Volta及以上架构显卡。实验显示，在Llama2 - 7B模型上应用int8/int4 KV 量化可使RPS分别提升30%和40%。使用LMDeploy时只需设置quant_policy参数（4为int4，8为int8），即可轻松应用于离线 推理 和服务部署场景。该技术特别适合

ArcGIS实战：手把手教你用DEM和灾害点数据，5步搞定地质灾害风险区划图

weixin_30696427的博客

05-20

426

本文详细介绍了使用ArcGIS进行地质灾害风险评估的完整流程，从DEM和灾害点数据出发，通过五步标准化操作生成专业风险区划图。重点讲解了信息量模型的应用，帮助读者掌握GIS技术在地质灾害易发性评价中的实践方法，提升风险评估效率与准确性。

AVM环视算法实战：从3D碗型投影到多场景模式切换的实现与 优化

weixin_42573113的博客

05-18

171

本文深入探讨了AVM环视算法在3D碗型投影和多场景模式切换中的实现与 优化 。通过分析3D碗型投影的核心原理和动态视角调节技术，展示了如何实现从超广角模式到转向模式的无缝切换。文章还分享了性能 优化 实战经验，包括实时性提升和图像拼接质量 优化 ，为开发者提供了宝贵的AVM系统开发指南。

LLM KV Cache 压缩技术解析：Multi - Head Key - Value 共享方案

zhangzhentiyes的博客

04-13

1378

随着大语言模型（LLM）在生成任务中的广泛应用， 推理 阶段的内存瓶颈愈发显著。特别是在长序列场景下， Transformer 架构中的 Key - Value （ KV ）缓存成为显存占用的主要来源。本文将深入剖析一种高效的 KV Cache 压缩技术&mdash;&mdash;，从理论推导到生产级实践的实现方案。

【HSPiP应用指南】从理论到实践：如何利用汉森溶解度参数 优化 配方设计

weixin_29305313的博客

03-07

1649

本文深入解析了汉森溶解度参数（HSP）理论及其在配方设计中的核心价值，并详细介绍了如何利用HSPiP软件工具从理论走向实践。文章通过匹配溶解度参数、计算汉森距离（Ra），系统阐述了如何解决溶剂选择、颜料分散稳定性及扩散行为预测等关键问题，并结合界面气相色谱（IGC）等实验方法提升预测精度，旨在帮助配方师实现配方设计的可视化、可量化与高效 优化 。

大模型 推理 kv _ cache 缓存

weixin_40777649的博客

01-09

2023

推理 时保留past_ key _ value 。优点：减少计算量，提高 推理 速度。

从ISO 17987到SAE J2602：LIN一致性测试标准全景解读与CANoe实现

app77的博客

02-25

336

本文深入解析了从ISO 17987到SAE J2602的LIN一致性测试标准技术演进，详细对比了两大标准体系的技术差异，并基于Vector CANoe平台演示了如何构建符合最新行业要求的自动化测试方案。文章重点介绍了SAE J2602标准的三大改进点，包括唤醒特性、传输层协议和诊断服务的增强测试要求，并提供了CANoe测试环境配置、测试模块集成及测试案例解析的实战指南。

LaTeX参考文献格式apsrev4 - 1，增加文献标题

loading1234567的博客

07-18

5032

APS的LaTeX参考文献格式apsrev4 - 1是隐藏标题的，如果想要出现标题，需要更改apsrev4 - 1.bst文件内容

在apsrev4 - 1.bst文件靠后部分，定义不同类型文献格式的地方，将article类型的内容替换为，

FUNCTION {article}
{ output.bibitem
format.authors
booktitle empty$ {
"author" output.check
}{ output } if$
author format.

【文献阅读】 Key, Value, Compress: A Systematic Exploration of KV Cache Compression Techniques

Toky_min的博客

04-02

1116

大语言模型（LLMs）在生成文本、图像和视频内容方面展现出卓越的能力。然而，随着上下文长度的增加，注意力机制的计算成本会随着令牌数量的增加而呈二次方增长，这带来了显著的效率挑战。本文对各种键值（ KV ）缓存压缩策略进行了分析，提供了一个全面的分类体系，根据其基本原理和实现技术对这些方法进行分类。此外，我们评估了它们对性能和 推理 延迟的影响，深入了解了它们的有效性。我们的研究结果突出了 KV 缓存压缩所涉及的权衡，以及它对处理长上下文场景的影响，为更高效的大语言模型实现铺平了道路。近年来，大语言模型

《深入浅出强化学习原理入门》学习笔记（三）机器人找金币应用实例

lxs3213196的博客

11-24

3187

《深入浅出强化学习原理入门》学习笔记（三）MDP应用实例

KV Cache

weixin_45207619的博客

01-19

193

KV Cache 是一种 优化 大语言模型 推理 速度的技术，其核心原理是缓存已计算的 Key 和 Value 向量，避免重复计算。通过只计算当前token的Query向量，并复用缓存的 KV 向量，显著减少了计算量。Prefix Cache 进一步 优化 了这一机制，允许复用完全相同的prompt前缀对应的 KV 缓存。但当prompt前缀发生变化时，所有后续生成的Attention结果都会受影响，因此需要重新计算。这提示我们可能通过prompt顺序调整或同义转换来提升缓存复用率。该技术在提升LLM 推理 效率方面具有重要价值。

YOLO26 网络结构逐模块解析：Backbone/Neck/Head

一键难忘的博客

03-01

1248

本文解析了YOLO26目标检测网络的架构设计，重点介绍其在Backbone、Neck和Head三个核心组件的创新 优化 。Backbone采用改进的CSP模块与轻量级注意力机制，提升特征提取效率；Neck使用改进的BiFPN结构实现高效多尺度特征融合；Head采用完全解耦设计简化回归流程。文章详细分析了各模块的技术演进、实现细节和设计思想，通过流程图和代码示例展示了YOLO26的整体架构和关键组件实现。该研究对模型 优化 、定制开发和硬件部署具有重要指导意义。

大中型超市收银系统 ( 可直接运行 )

光步前台收银系统系统功能：商品销售、商品退货、挂单、收银员权限、会员销售、单品取消、整单取消、商品查询、脱机销售、脱机数据上传，基本资料下载，开钱箱、小票打印、收款结算，支持多币种付款方式（现金、储值卡、礼券、微信等）,支持多种促销。运行环境：.net 4.0登陆信息用户名:1001 密码:123456内置商品信息，商品编码可输入00001000 - 00010000,测试效果相关功能键F5 结算 F9挂单 F11商品查询 F8退货 Insert单品取消 Del整单取消 F12开钱箱F2 会员 Ctrl+Z退出系统 Home 锁屏 相关源码待公开

KV Cache 中的 &ldquo;page attention&rdquo; 是什么

ningyanggege的博客

05-10

1175

KV Cache （ Key - Value Cache ）是 Transformer 模型中用于加速 推理 的技术，通过缓存历史tokens的 Key 和 Value ，避免重复计算。在自注意力机制中，Query（Q）与缓存的 Key 进行点乘，再对缓存的 Value 加权求和，从而提升效率。Page attention则是 KV Cache 的 优化 策略，将 Key 和 Value 按页分块存储，减少无效数据搬运，提高内存访问效率。通过分页索引，模型可以快速定位相关页面，避免全量遍历，进一步加速 推理 。这种分页式存储不仅 优化 了内存和显存利用，还保持了注

Outlook Express 6 ( OE6 ) for Win7/Win8/Win10

英文版 Outlook Express6.0， 兼容Win7/Win10/WinXP，解压就可用。

Kv cache

weixin_71184753的博客

02-13

948

KV Cache 是 大模型 推理 加速的关键技术，通过缓存历史token的 Key - Value 矩阵避免重复计算。其工作原理是：在自回归生成过程中，将已计算的K/V矩阵存入缓存，后续生成时直接复用，显著减少计算量。但 KV Cache 会线性增加显存占用，导致长文本生成时出现OOM问题。DeepSeek的MLA技术通过压缩K/V矩阵有效缓解这一问题，将缓存体积大幅缩小，支持更长文本 推理 。 KV Cache 实现了计算效率与显存占用的平衡，是 优化 大模型 推理 性能的重要方案。

IEM模型，地表粗糙度模拟

利用微波反演土壤水分时候，利用IEM模型可以实现对随机地表的模拟

计算机控制技术及应用

《计算机控制技术及应用》通过总结作者的科研成果和吸收国内外的先进理论、方法和技术，以计算机控制系统的网络化、开放化、智能化和集成化发展趋势为主线索，主要介绍计算机控制系统的基本概念和计算机控制系统的典型形式，计算机控制系统的数学描述与基本分析方法，以及数字PID控制器、最少拍控制系统、达林算法、状态变量设计法等设计方法及其各种改进算法。重点突出计算机控制系统输入输出通道接口的设计方法和典型控制系统的设计案例与实现方法等。

MHA还是GQA？StableLM - 3B - 4E1T的num_ key _ value _heads参数如何帮你节省显存

gitblog_00805的博客

08-22

1096

StableLM - 3B - 4E1T 是 Stability AI 开源的 30 亿参数语言模型，其 `config.json` 中的 **num_ key _ value _heads** 参数决定了模型使用 MHA 还是 GQA 注意力机制，直接影响 推理 时的 KV Cache 显存占用。这篇文章带你快速看懂这个参数，并告诉你如何用它省下显存。💡

## 一句话说清：MHA、GQA、MQA 到底是什么

大模型 推理 引擎vLLM ( 0 ) : KV Cache 原理

C/C++、嵌入式开发、深度学习算法、模型部署与推理优化

12-22

973

大模型 推理 引擎vLLM ( 0 ) : KV Cache 原理

TurboQuant：LLM 推理 KV Cache 压缩新利器&mdash;&mdash;3比特 Key +2比特 Value 如何实现近无损压缩

最新发布

gitblog_00807的博客

10-10

129

TurboQuant 是一款面向 LLM 推理 的 KV Cache 压缩开源工具，它把注意力机制中的 Key 压缩到 3 比特、 Value 压缩到 2 比特，实现近无损的 4.4 倍显存压缩，并原生集成 vLLM 与 Triton 加速内核。🚀

如果你在 大模型 推理 中常遇到"显存不够、上下文装不下、并发上不去"的困境，这篇文章将带你从零看懂 TurboQuant 的工作原理、实测数据和上手步骤。

目录

展开全部

收起

33

0

40

复制链接

分享到 QQ

分享到新浪微博

扫一扫

打赏

举报

+ 关注

彬彬侠

优质创作者: 人工智能技术领域

码龄13年
1万+粉丝
1707原创

+ 关注
专栏目录
订阅专栏

✦ 双11 狂欢大促 ✦

盲盒赢 定制羽绒服

开启惊喜盲盒，抽取你的专属好礼

✦

立即开盒 ›
中奖列表 ›

— 惊喜奖品抢先看 —

超级年卡50元券
周边70元券
手机支架
一拖三数据线
雨伞
运动毛巾
电动牙刷
VIP月卡
机械键盘
定制羽绒服
超级年卡50元券
周边70元券
手机支架
一拖三数据线
雨伞
运动毛巾
电动牙刷
VIP月卡
机械键盘
定制羽绒服

评论

成就一亿技术人!

拼手气红包 6.0元

还能输入 1000 个字符

插入表情

代码片

HTML/XML

objective-c

Ruby

PHP

C

C++

JavaScript

Python

Java

CSS

SQL

其它

条评论被折叠 查看

被折叠的 条评论
为什么被折叠?

到【灌水乐园】发言

查看更多评论

添加红包

祝福语

请填写红包祝福语或标题

红包数量

个

红包个数最小为10个

红包总金额

元

红包金额最低5元

余额支付

当前余额 3.43 元
前往充值 >

需支付： 10.00 元

取消
确定

成就一亿技术人!

领取后你会自动成为博主和红包主的粉丝
规则

hope_wisdom
发出的红包

打赏作者

彬彬侠

你的鼓励将是我创作的最大动力

¥1
¥2
¥4
¥6
¥10
¥20

扫码支付： ¥1

获取中

扫码支付

您的余额不足，请更换扫码支付或 充值

打赏作者

实付 元

使用余额支付

点击重新获取

扫码支付

钱包余额
0

抵扣说明：

1.余额是钱包充值的虚拟货币，按照1:1的比例进行支付金额的抵扣。
2.余额无法直接购买下载，可以购买VIP、付费专栏及课程。

余额充值
