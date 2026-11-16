# 知乎·LLM 提速利器：投机推理的原理与常见方案

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　7,980 字符　｜　86 个片段　｜　更新于 2026-11-13
来源: https://zhuanlan.zhihu.com/p/1978037808544370747
抓取方式: browser_open（登录态，2026-11-13）
说明: 知乎正文存在字体反爬（每个字母 t 被替换为空格），本文已按术语表自动还原主要英文术语，个别词仍可能有残缺；中文正文完整、可信。

投机推理（Specula ive Decoding）是一种高效的推理方式，可提升推理性能（如ITL）、降低推理成本，在特定场景下能够提升用户体感。目前投机推理已成为了一种标准的解决方案，常见的有：

草稿模型。给大模型配置一个草稿（draf ）模型，草稿模型负责字符生成、大模型验证生成结果。
辅助层。在大模型基础上增加一些头层，使其能生成更多的字符，如Medusa、EAGLE等。
数据匹配。利用历史数据/模板，通过规则匹配的方式生成字符，然后用大模型进行校验，如ngram、suffix decoding。

投机推理也可以叫做推测解码、投机采样（Specula ive Sampling），本文中统一用“投机推理“。

小模型计算draf ，大模型校验

本文配套no ebook代码：h ps://gi hub.com/CalvinXKY/InfraTech/ ree/main/llm_infer

更多推理知识：

LLM推理知识指南---kaiyuan
246 赞同 · 7 评论 文章

1 方案介绍
1.1 基本原理

投机推理是一个猜测、验证、比较的过程，结合概率统计知识先介绍其基本原理。

猜测大模型的输出。目前主流的大语言模型（LLM）采用的是自回归生成的方式，是一种依赖采样（Dependen Sampling），其概率用 P(X_n | X_1, X_2, ..., X_{n-1}) 表示，特点是历史的采样值影响当前采样输出。

当使用“猜测”的方式去预测大模型输出时，存在如下特征：

n越大预测序列偏移量越大；
采样的确定性越高（随机性低），预测容错空间越小。

第1点可理解为猜测得越多，可能导致输出质量越差；对于第2点的理解，举个例子说明。如一个填词题：

“平__”？

可能的结果有“平凡“、”平方”、“平齐”、“平行”等。假设让LLM模型推测一个结果，用概率最高值作为输出(比如“平行”)，我们随机猜测一个结果与之相等的概率是：1/ ，其中 表示可能结果。

为了增加1/ 数值，要么扩大其分子，要么缩小其分母。如，让模型输出多个值（ opK结果都算正确），提升猜测值与模型输出值的重叠量。

可以用于去猜测大模型输出的方法有多种，而实践中希望找到一个让1/n更大的方案。

模型多输出值的作用。在LLM推理中，当输入一个prompt 时可得到同长度的logi s输出，单个logi s值可用于预测下一个字。自回归中只取logi s最后一个值，前L-1个logi s丢弃（要么不生成），因为前L-1个字符都是已知的。如果解析前面的logi s，对应的预测值能否与输入的prompt 字符对上吗？

答案是：不一定。 因为模型的输出是概率分布，只能说预测到原字符的概率更大。比如下图中，我们取第三个字符‘k’的输出logi s并采样，结果可能不是‘a’，出现了LLM的预测值与输入值不相等的情况。

因为前L个字符是已知的，所以计算L-1字符的采样输出对于LLM生成L之后的字符来说没有意义。但是却能利用LLM支持多输出的能力校验（评估)另一个的模型的输出质量。

直观一点就是，让模型A生成了一串字符，把该字符输入到能力更强模型B评价一下这个输出是否达标。

评价过程。要对预测的结果好坏进行评估，其一般规则：当主模型的概率高于小模型输出的概率时，则认为预测是可信的，反之亦然。举个例子，如下图所示，概率分布A是小模型的预测输出，概率分布B是主模型标准输出。若小模型预测出了‘n’值，‘n’在两个模型中的概率分别为0.37、0.67，因为0.37 < 0.67，则‘n’值被接受。

若把大模型比作师父，小模型比作徒弟：

猜测大模型的输出：徒弟模仿师父的动作；
模型多输出值的作用：师父通过自身能力指点徒弟；
评价过程：徒弟的工作需要得到了师父的认可；如果徒弟出错了，师父就纠正。
1.2 草稿模型

带有草稿（draf ）模型的投机推理过程一般是先用草稿模型生成数据，然后用参数量较大的模型进行结果验证/生成。结合下图来介绍该方法，假设有两个模型LLM-1和LLM-2，LLM-1负责输出、LLM-2负责校验。基本步骤：

输入提示词prompt 到LLM-1，经过n轮递推迭代后生成对应结果。
将输出结果传递给LLM-2计算，得到对应的logi s。
用单个logi s计算得到下一个字符概率分布，i’->‘y’预测概率，若‘y’被接受，则进行下一步迭代，依次类推。
若步骤3中字符被拒绝，则让采样回退一步，并返回结果(或者重采样)。

对于大、小模型的概率比较常见的方法有：

阈值比较：设定一个阈值，如果预测概率小于阈值，则接受该字符并继续预测；如果概率大于阈值，则丢弃该字符和后续所有值，并重新预测。输出格式： [prefix, x_0, x_1 ... x_{n-1}] ；
拒绝采样（Rejec ion Sampling）：对于每个预测字符位置设置一个接受率 gamma sim U(0,1) , 若在范围内则接受字符；当超过范围时，本轮的最后一个字符用主模型的采样结果 。输出： [prefix, x_0, x_1 ... x_{n-1}, ] ；
校准（Correc ness）：与拒绝采样类似，不同的是最后一个字符被拒绝时，重采样的计算公式为采用： p^{'}(x) = norm(max(0, p(x) - q(x))) 。目前草稿模型中一般用此方法，其相比拒绝采样效果更好[1]。
小模型生成2个字符+大模型生成1个字符

在LLM-2的校验运算中，更常用的方式是因果掩膜（Causal Mask）。对比一般解码（Decode）过程[2]，投机推理增加了草稿模型的解码输出，查询（Query）长度为草稿模型长度加1，即L=1+n。

过程对比示意

草稿模型方案的优点：

草稿模型一般会选择开销小的模型（比如用0.1倍的参数）[3]，相比大模型直接生成速度更快；
一般推理与投机推理的对比
单次可生成n个字符；最坏的情况是第一次校验就失败，但至少能生成一个字符；
接受阈值可以作为一个调节超参，满足不同场景的需求。

不足：

小模型能力不足，或者参数选取不当可能会导致输出质量下降；
如果首次验证就失败，模型的计算量比不用投机推理的计算量更大。
1.3 辅助层

与草稿模型原理类似的方式还有另一种：在主模型上额外添加一些层来进行多字符预测，这些层和主模型同时训练、共享主模型信息。跟草稿模型方案最大差异点是这种方式的大模型与辅助层之间存在耦合关系。增加辅助层的方案也有多种，比如Medusa[4]、EAGLE[5]、MTP[6]、MLP specula or[7]等，选取两个典型方法介绍：

EAGLE(Ex rapola ion Algori hm for Grea er Language-model Efficiency)的核心思想利用主模型的中间输出（LM Head的输入）信息提供给草稿模型进行自回归生成 token。关键点：

辅助模型的Embedding和LM Head共享参数；
递推需要利用主模型（ arge LLM）的中间信息，这些特征值f传递给辅助模型层计算；
在迭代的过程中f与embedding的输出e进行拼接作为辅助层主体的输入；

这种方式下，辅助层的forward输入宽度可以调节。

EAGLE-1的推理过程

EAGLE目前进行了两个版本的迭代，最新的是EAGLE3（参考：h ps://arxiv.org/abs/2503.01840）

MTP(Mul i-Token Predic ion)思想与EAGLE类似，其结构也是在主模型层的后面接几个头层，不过MTP更好的适配了训练场景，且随着DeepSeekV3发布受到了更多关注。

结构如下图所示，主模型（Main Model）后面接的MTP Module1/2可视作多个小模型。在一次推理中可生成多个 token，图中 5是主模型生成的数据，而 6、 7...由MTP生成。

在推理中MTP模块本轮生成的 token不会直接作为输出返回，而是先保存为草稿。在下一轮推理中，通过校验确定是否被接受。举个例子，若模型本轮生成了： 5、 6、 7...，而推理返回值为 5；下一轮中main model的输入为 5~ 7...，采用直接概率比较方式，如果阈值范围内能预测出 6，则接受 6，依次类推。

MTP推理过程

对框架（如vLLM）的影响：

1、投机推理不会改变推理框架调度逻辑，依然保持单次调度完成相同数量的前向计算。

2、推理框架的缓存KV cache逻辑需要适配：

kv cache缓存的 tokens数量增加上投机推理的空间，比如MTP中有几个头就要多开启几个 tokens地址位置；
当 tokens未被采纳时，对应的kv cache得进行回退。常用的方式是直接覆盖。
1.4 数据匹配预测

在处理一些高频重复文本的场景中，如法律、规则等，可以用数据规则匹配预测LLM的输出。常见的方式有Ngram、Suffix decoding。

Ngram（gram指代元组)：是一种基于历史数据进行概率计算并获得预测结果的方式。其核心的思想是：一个词出现的概率依赖于它前面的 N-1 个词。举个例子，假设输入的前缀为“我要吃”，可能的尾缀有“苹果”、“西瓜”，若对应的概率值为：

P(西瓜|我要吃) = 0.2

P(苹果|我要吃) = 0.5

则输出为“苹果”，这个Ngram例子中匹配采用了三个字，输出预测了两个字。

在推理中应用Ngram方法，一般步骤：

确定匹配长度、预测长度。
根据已有数据(prompt 或者历史数据)构建Ngram词表。
用主模型验证结果，若满足则接受，反之亦然。

Suffix decoding尾缀匹配：根据输出数据来构造尾缀树，匹配到合适的尾缀后选择得分最高的值作为输出，得分的高低与数据出现频率相关。suffix decoding涉及有两个关键参数：窗口大小P、后缀节点数量N。

结合下图示例（P=2, N=3），输入为“85485”，suffix decoding匹配的头有两种“5”和“8，5”。

其基本步骤：

根据保存的历史数据，构造第一个后缀树。
根据当前输出构造的另一个后缀树。
找出两棵后缀树中满足要求的子树。
对子树进行打分排序，挑选出分数最高的子树。
用LLM验证子树的值是否满足要求，若满足则接受。

数据匹配的优点：

计算量小、不需要额外显存空间；
极大的降低了ITL，输出速度更快；
因为无草稿模型，不用同时协调两个模型的部署，框架适配更加方便。

不足：

仅在一些文本重复率比较高的场景使用，场景变化比较大时，接受率低。
2 投机推理的使用示例

目前投机推理基本功能在主流框架（vLLM/SGLang）中均已适配，以vLLM 0.9.x的离线场景来列举几个投机推理的使用方法。

关键参数：

specula ive_config：投机推理配置，包括小模型参数。
num_specula ive_ okens：草稿模型输出的 tokens的数量。
method：计算方式，如ngram、m p、eagle、eagle3；
promp _lookup_max：最大的查询长度，用于ngram。
promp _lookup_min：最小的查询长度，用于ngram。
2.1 草稿模型
# 在vLLM中使用投机推理的示例：
from vllm impor LLM, SamplingParams

prompts = [
 "InfraTech是什么类型的公众号？",
]
sampling_params = SamplingParams( empera ure=0.9)

llm = LLM(
 model="Qwen/Qwen3-VL-235B-A22B-Thinking",
 ensor_parallel_size=1,
 specula ive_config={
 "model": "Qwen/Qwen2.5-VL-3B-Ins ruc ",
 "num_specula ive_ okens": 5,
 },
)
outputs = llm.generate(prompts, sampling_params)

for ou pu in outputs:
 prompt = output .prompt 
 genera ed_ ex = output .outputs[0]. text 
 prin (f"Promp : {prompt !r}, Genera ed ex : {genera ed_ ex !r}")

注意：草稿模型的功能需要在0.9.x中使用，v0.10.x中使用会报No Implemen edError错误。

2.2 N-grams
from vllm impor LLM, SamplingParams

prompts = [
 "如何关注InfraTech公众号？",
]
sampling_params = SamplingParams( empera ure=0.8, op_p=0.95)

llm = LLM(
 model="facebook/op -6.7b",
 ensor_parallel_size=1,
 specula ive_config={
 "method": "ngram",
 "num_specula ive_ okens": 5,
 "promp _lookup_max": 4,
 },
)
outputs = llm.generate(prompts, sampling_params)

for ou pu in outputs:
 prompt = output .prompt 
 genera ed_ ex = output .outputs[0]. text 
 prin (f"Promp : {prompt !r}, Genera ed ex : {genera ed_ ex !r}")
2.3 Suffix Decoding/MTP
from vllm impor LLM, SamplingParams

prompts = [
 "如何关注InfraTech公众号？",
]
sampling_params = SamplingParams( empera ure=0.8, op_p=0.95)

llm = LLM(
 model="facebook/op -6.7b",
 ensor_parallel_size=1,
 specula ive_config={
 "method": "suffix", #"MTP"
 "num_specula ive_ okens": 5, # 1
 },
)
outputs = llm.generate(prompts, sampling_params)

for ou pu in outputs:
 prompt = output .prompt 
 genera ed_ ex = output .outputs[0]. text 
 prin (f"Promp : {prompt !r}, Genera ed ex : {genera ed_ ex !r}")
2.4 EAGLE
from vllm impor LLM, SamplingParamsTex only

prompts = [
 "如何关注InfraTech公众号？",
]
sampling_params = SamplingParams( empera ure=0.8, op_p=0.95)

llm = LLM(
 model="me a-llama/Me a-Llama-3-8B-Ins ruc ",
 ensor_parallel_size=4,
 specula ive_config={
 "model": "yuhuili/EAGLE-LLaMA3-Ins ruc -8B",
 "draf _ ensor_parallel_size": 1,
 "num_specula ive_ okens": 2,
 "method": "eagle",
 },
)

outputs = llm.generate(prompts, sampling_params)

for ou pu in outputs:
 prompt = output .prompt 
 genera ed_ ex = output .outputs[0]. text 
 prin (f"Promp : {prompt !r}, Genera ed ex : {genera ed_ ex !r}")

投机推理目前在vLLM还存些使用限制，如不支持PP等并行策略，社区还在不断改进，具体可参看官网说明。

参考内容：

Fas Inference from Transformers via Specula ive Decoding
DeepSeek-V3 Technical Repor 
SuffixDecoding: Ex reme Specula ive Decoding for Emerging AI Applica ions
Medusa: Simple LLM Inference Accelera ion Framework with Mul iple Decoding Heads
EAGLE: Specula ive Sampling Requires Re hinking Fea ure Uncer ain y
h ps://docs.vllm.ai/en/la es /fea ures/spec_decode
参考
^Fas Inference from Transformers via Specula ive Decoding
^h ps://flashinfer.ai/2024/02/02/in roduce-flashinfer.h ml
^Specula ive Decoding with Big Li le Decoder h ps://arxiv.org/pdf/2302.07863
^Medusa: Simple LLM Inference Accelera ion Framework with Mul iple Decoding Heads
^EAGLE: Specula ive Sampling Requires Re hinking Fea ure Uncer ain y
^DeepSeek-V3 Technical Repor 
^Accelera ing Produc ion LLMs with Combined Token/Embedding Specula ors
