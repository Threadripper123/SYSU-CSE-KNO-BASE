# 论文·DeepSeek-V2：多头潜在注意力（MLA）与 DeepSeekMoE

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　483 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2405.04434
抓取: https://ar5iv.labs.arxiv.org/html/2405.04434

\reportnumber
001

DeepSeek-V2: A Strong, Economical, and Efficient Mixture-of-Experts Language Model

DeepSeek-AI

research@deepseek.com

Abstract

We present DeepSeek-V2, a strong Mixture-of-Experts (MoE) language model characterized by economical training and efficient inference.
It comprises 236B total parameters, of which 21B are activated for each token, and supports a context length of 128K tokens.
DeepSeek-V2 adopts innovative architectures including Multi-head Latent Attention (MLA) and DeepSeekMoE.
MLA guarantees efficient inference through significantly compressing the Key-Value (KV) cache into a latent vector, while DeepSeekMoE enables training strong models at an economical cost through sparse computation.
Compared with DeepSeek 67B, DeepSeek-V2 achieves significantly stronger performance, and meanwhile saves 42.5% of training costs, reduces the KV cache by 93.3%, and boosts the maximum generation throughput to 5.76 times.
We pretrain DeepSeek-V2 on a high-quality and multi-source corpus consisting of 8.1T tokens, and further perform Supervised Fine-Tuning (SFT) and Reinforcement Learning (RL) to fully unlock its potential.
Evaluation results show that, even with only 21B activated parameters, DeepSeek-V2 and its chat versions still achieve top-tier performance among open-source models.
The model checkpoints are available at https://github.com/deepseek-ai/DeepSeek-V2.

Figure 1:
(a) MMLU accuracy vs. activated parameters, among different open-source models.
(b) Training costs and inference efficiency of DeepSeek 67B (Dense) and DeepSeek-V2.

Contents

1 Introduction

2 Architecture

2.1 Multi-Head Latent Attention: Boosting Inference Efficiency

2.1.1 Preliminaries: Standard Multi-Head Attention

2.1.2 Low-Rank Key-Value Joint Compression

2.1.3 Decoupled Rotary Position Embedding

2.1.4 Comparison of Key-Value Cache

2.2 DeepSeekMoE: Training Strong Models at Economical Costs

2.2.1 Basic Architecture

2.2.2 Device-Limited Routing

2.2.3 Auxiliary Loss for Load Balance

2.2.4 Token-Dropping Strategy

3 Pre-Training

3.1 Experimental Setups

3.1.1 Data Construction

3.1.2 Hyper-Parameters

3.1.3 Infrastructures

3.1.4 Long Context Extension

3.2 Evaluations

3.2.1 Evaluation Benchmarks

3.2.2 Evaluation Results

3.2.3 Training and Inference Efficiency

4 Alignment

4.1 Supervised Fine-Tuning

4.2 Reinforcement Learning

4.3 Evaluation Results

4.4 Discussion

5 Conclusion, Limitation, and Future Work

References

A Contributions and Acknowledgments

B DeepSeek-V2-Lite: A 16B Model Equipped with MLA and DeepSeekMoE

B.1 Model Description

B.2 Performance Evaluation

C Full Formulas of MLA

D Ablation of Attention Mechanisms

D.1 Ablation of MHA, GQA, and MQA

D.2 Comparison Between MLA and MHA

E Discussion About Pre-Training Data Debiasing

F Additional Evaluations on Math and Code

G Evaluation Formats

1 Introduction

In the past few years, Large Language Models (LLMs) ( OpenAI 2022; OpenAI 2023; Anthropic 2023; Google 2023 ) have undergone rapid development, offering a glimpse into the dawn of Artificial General Intelligence (AGI).
In general, the intelligence of an LLM tends to improve as the number of parameters increases, allowing it to exhibit emergent capabilities across various tasks ( Wei et al. 2022 ).
However, the improvement comes at the cost of larger computing resources for training and a potential decrease in inference throughput.
These constraints present significant challenges that impede the widespread adoption and utilization of LLMs.
In order to tackle this problem, we introduce DeepSeek-V2, a strong open-source Mixture-of-Experts (MoE) language model, characterized by economical training and efficient inference through an innovative Transformer architecture.
It is equipped with a total of 236B parameters, of which 21B are activated for each token, and supports a context length of 128K tokens.

We optimize the attention modules and Feed-Forward Networks (FFNs) within the Transformer framework ( Vaswani et al. 2017 ) with our proposed Multi-head Latent Attention (MLA) and DeepSeekMoE.
(1)
In the context of attention mechanisms, the Key-Value (KV) cache of the Multi-Head Attention (MHA) ( Vaswani et al. 2017 ) poses a significant obstacle to the inference efficiency of LLMs.
Various approaches have been explored to address this issue, including Grouped-Query Attention (GQA) ( Ainslie et al. 2023 ) and Multi-Query Attention (MQA) ( Shazeer 2019 ).
However, these methods often compromise performance in their attempt to reduce the KV cache.
In order to achieve the best of both worlds, we introduce MLA, an attention mechanism equipped with low-rank key-value joint compression.
Empirically, MLA achieves superior performance compared with MHA, and meanwhile significantly reduces the KV cache during inference, thus boosting the inference efficiency.
(2)
For Feed-Forward Networks (FFNs), we follow the DeepSeekMoE architecture ( Dai et al. 2024 ), which adopts fine-grained expert segmentation and shared expert isolation for higher potential in expert specialization.
The DeepSeekMoE architecture demonstrates great advantages compared with conventional MoE architectures like GShard ( Lepikhin et al. 2021 ), enabling us to train strong models at an economical cost.
As we employ expert parallelism during training, we also devise supplementary mechanisms to control communication overheads and ensure load balance.
By combining these two techniques, DeepSeek-V2 features strong performance (Figure 1 ), economical training costs, and efficient inference throughput (Figure 1 ), simultaneously.

Figure 2:
Illustration of the architecture of DeepSeek-V2.
MLA ensures efficient inference by significantly reducing the KV cache for generation, and DeepSeekMoE enables training strong models at an economical cost through the sparse architecture.

We construct a high-quality and multi-source pre-training corpus consisting of 8.1T tokens.
Compared with the corpus used in DeepSeek 67B (our previous release) ( DeepSeek-AI 2024 ), this corpus features an extended amount of data, especially Chinese data, and higher data quality.
We first pretrain DeepSeek-V2 on the full pre-training corpus.
Then, we collect 1.5M conversational sessions, which encompass various domains such as math, code, writing, reasoning, safety, and more, to perform Supervised Fine-Tuning (SFT) for DeepSeek-V2 Chat (SFT).
Finally, we follow DeepSeekMath ( Shao et al. 2024 ) to employ Group Relative Policy Optimization (GRPO) to further align the model with human preference and produce DeepSeek-V2 Chat (RL).

We evaluate DeepSeek-V2 on a wide range of benchmarks in English and Chinese, and compare it with representative open-source models.
Evaluation results show that even with only 21B activated parameters, DeepSeek-V2 still achieves top-tier performance among open-source models and becomes the strongest open-source MoE language model.
Figure 1 highlights that, on MMLU, DeepSeek-V2 achieves top-ranking performance with only a small number of activated parameters.
In addition, as shown in Figure 1, compared with DeepSeek 67B, DeepSeek-V2 saves 42.5% of training costs, reduces the KV cache by 93.3%, and boosts the maximum generation throughput to 5.76 times.
We also evaluate DeepSeek-V2 Chat (SFT) and DeepSeek-V2 Chat (RL) on open-ended benchmarks.
Notably, DeepSeek-V2 Chat (RL) achieves 38.9 length-controlled win rate on AlpacaEval 2.0 ( Dubois et al. 2024 ), 8.97 overall score on MT-Bench ( Zheng et al. 2023 ), and 7.91 overall score on AlignBench ( Liu et al. 2023 ).
The English open-ended conversation evaluations demonstrate that DeepSeek-V2 Chat (RL) has top-tier performance among open-source chat models.
In addition, the evaluation on AlignBench indicates that in Chinese, DeepSeek-V2 Chat (RL) outperforms all of open-source models, and even beats most of closed-source models.

In order to facilitate further research and development on MLA and DeepSeekMoE, we also release DeepSeek-V2-Lite, a smaller model equipped with MLA and DeepSeekMoE, for the open-source community.
It has a total of 15.7B parameters, where 2.4B are activated for each token.
Detailed descriptions about DeepSeek-V2-Lite can be found in Appendix B.

In the rest of this paper, we first provide a detailed description of the model architecture of DeepSeek-V2 (Section 2 ).
Subsequently, we introduce our pre-training endeavors, including the training data construction, hyper-parameter settings, infrastructures, long context extension, and the evaluation of model performance and efficiency (Section 3 ).
Following this, we demonstrate our efforts in alignment, encompassing Supervised Fine-Tuning (SFT), Reinforcement Learning (RL), the evaluation results, and other discussion (Section 4 ).
Finally, we summarize the conclusion, deliberate on the current limitations of DeepSeek-V2, and outline our future work (Section 5 ).

2 Architecture

By and large, DeepSeek-V2 is still in the Transformer architecture ( Vaswani et al. 2017 ), where each Transformer block consists of an attention module and a Feed-Forward Network (FFN).
However, for both the attention module and the FFN, we design and employ innovative architectures.
For attention, we design MLA, which utilizes low-rank key-value joint compression to eliminate the bottleneck of inference-time key-value cache, thus supporting efficient inference.
For FFNs, we adopt the DeepSeekMoE architecture ( Dai et al. 2024 ), a high-performance MoE architecture that enables training strong models at an economical cost.
An illustration of the architecture of DeepSeek-V2 is presented in Figure 2, and we will introduce the details of MLA and DeepSeekMoE in this section.
For other tiny details (e.g., layer normalization and the activation function in FFNs), unless specifically stated, DeepSeek-V2 follows the settings of DeepSeek 67B ( DeepSeek-AI 2024 ).

2.1 Multi-Head Latent Attention: Boosting Inference Efficiency

Conventional Transformer models usually adopts Multi-Head Attention (MHA) ( Vaswani et al. 2017 ), but during generation, its heavy Key-Value (KV) cache will become the bottleneck that limit the inference efficiency.
In order to reduce the KV cache, Multi-Query Attention (MQA) ( Shazeer 2019 ) and Grouped-Query Attention (GQA) ( Ainslie et al. 2023 ) are proposed.
They require a smaller magnitude of KV cache, but their performance does not match MHA (we provide the ablation of MHA, GQA and MQA in Appendix D.1 ).

For DeepSeek-V2, we design an innovative attention mechanism called Multi-head Latent Attention (MLA).
Equipped with low-rank key-value joint compression, MLA achieves better performance than MHA, but requires a significantly smaller amount of KV cache.
We introduce its architecture in the following, and also provide a comparison between MLA and MHA in Appendix D.2.

2.1.1 Preliminaries: Standard Multi-Head Attention

We first introduce the standard MHA mechanism as background.
Let d d be the embedding dimension, n h n_{h} be the number of attention heads, d h d_{h} be the dimension per head, and 𝐡 t ∈ ℝ d \mathbf{h}_{t}\in\mathbb{R}^{d} be the attention input of the t t -th token at an attention layer.
Standard MHA first produces 𝐪 t, 𝐤 t, 𝐯 t ∈ ℝ d h ​ n h \mathbf{q}_{t},\mathbf{k}_{t},\mathbf{v}_{t}\in\mathbb{R}^{d_{h}n_{h}} through three matrices W Q, W K, W V ∈ ℝ d h ​ n h × d W^{Q},W^{K},W^{V}\in\mathbb{R}^{d_{h}n_{h}\times d}, respectively:

𝐪 t \displaystyle\mathbf{q}_{t}

= W Q ​ 𝐡 t, \displaystyle=W^{Q}\mathbf{h}_{t},

(1)

𝐤 t \displaystyle\mathbf{k}_{t}

= W K ​ 𝐡 t, \displaystyle=W^{K}\mathbf{h}_{t},

(2)

𝐯 t \displaystyle\mathbf{v}_{t}

= W V ​ 𝐡 t, \displaystyle=W^{V}\mathbf{h}_{t},

(3)

Then, 𝐪 t, 𝐤 t, 𝐯 t \mathbf{q}_{t},\mathbf{k}_{t},\mathbf{v}_{t} will be sliced into n h n_{h} heads for the multi-head attention computation:

[ 𝐪 t, 1; \displaystyle[\mathbf{q}_{t,1};

𝐪 t, 2; …; 𝐪 t, n h ] = 𝐪 t, \displaystyle\mathbf{q}_{t,2};...;\mathbf{q}_{t,n_{h}}]=\mathbf{q}_{t},

(4)

[ 𝐤 t, 1; \displaystyle[\mathbf{k}_{t,1};

𝐤 t, 2; …; 𝐤 t, n h ] = 𝐤 t, \displaystyle\mathbf{k}_{t,2};...;\mathbf{k}_{t,n_{h}}]=\mathbf{k}_{t},

(5)

[ 𝐯 t, 1; \displaystyle[\mathbf{v}_{t,1};

𝐯 t, 2; …; 𝐯 t, n h ] = 𝐯 t, \displaystyle\mathbf{v}_{t,2};...;\mathbf{v}_{t,n_{h}}]=\mathbf{v}_{t},

(6)

𝐨 t, i \displaystyle\mathbf{o}_{t,i}

= ∑ j = 1 t Softmax j ⁡ ( 𝐪 t, i T ​ 𝐤 j, i d h ) ​ 𝐯 j, i, \displaystyle=\sum_{j=1}^{t}\operatorname{Softmax}_{j}(\frac{\mathbf{q}_{t,i}^{T}\mathbf{k}_{j,i}}{\sqrt{d_{h}}})\mathbf{v}_{j,i},

(7)

𝐮 t \displaystyle\mathbf{u}_{t}

= W O ​ [ 𝐨 t, 1; 𝐨 t, 2; …; 𝐨 t, n h ], \displaystyle=W^{O}[\mathbf{o}_{t,1};\mathbf{o}_{t,2};...;\mathbf{o}_{t,n_{h}}],

(8)

where 𝐪 t, i, 𝐤 t, i, 𝐯 t, i ∈ ℝ d h \mathbf{q}_{t,i},\mathbf{k}_{t,i},\mathbf{v}_{t,i}\in\mathbb{R}^{d_{h}} denote the query, key, and value of the i i -th attention head, respectively;
W O ∈ ℝ d × d h ​ n h W^{O}\in\mathbb{R}^{d\times d_{h}n_{h}} denotes the output projection matrix.
During inference, all keys and values need to be cached to accelerate inference, so MHA needs to cache 2 ​ n h ​ d h ​ l 2n_{h}d_{h}l elements for each token.
In model deployment, this heavy KV cache is a large bottleneck that limits the maximum batch size and sequence length.

Figure 3:
Simplified illustration of Multi-Head Attention (MHA), Grouped-Query Attention (GQA), Multi-Query Attention (MQA), and Multi-head Latent Attention (MLA).
Through jointly compressing the keys and values into a latent vector, MLA significantly reduces the KV cache during inference.

2.1.2 Low-Rank Key-Value Joint Compression

The core of MLA is the low-rank joint compression for keys and values to reduce KV cache:

𝐜 t K ​ V \displaystyle\mathbf{c}_{t}^{KV}

= W D ​ K ​ V ​ 𝐡 t, \displaystyle=W^{DKV}\mathbf{h}_{t},

(9)

𝐤 t C \displaystyle\mathbf{k}_{t}^{C}

= W U ​ K ​ 𝐜 t K ​ V, \displaystyle=W^{UK}\mathbf{c}_{t}^{KV},

(10)

𝐯 t C \displaystyle\mathbf{v}_{t}^{C}

= W U ​ V ​ 𝐜 t K ​ V, \displaystyle=W^{UV}\mathbf{c}_{t}^{KV},

(11)

where 𝐜 t K ​ V ∈ ℝ d c \mathbf{c}_{t}^{KV}\in\mathbb{R}^{d_{c}} is the compressed latent vector for keys and values;
d c ( ≪ d h ​ n h ) d_{c}(\ll d_{h}n_{h}) denotes the KV compression dimension;
W D ​ K ​ V ∈ ℝ d c × d W^{DKV}\in\mathbb{R}^{d_{c}\times d} is the down-projection matrix;
and W U ​ K, W U ​ V ∈ ℝ d h ​ n h × d c W^{UK},W^{UV}\in\mathbb{R}^{d_{h}n_{h}\times d_{c}} are the up-projection matrices for keys and values, respectively.
During inference, MLA only needs to cache 𝐜 t K ​ V \mathbf{c}_{t}^{KV}, so its KV cache has only d c ​ l d_{c}l elements, where l l denotes the number of layers.
In addition, during inference, since W U ​ K W^{UK} can be absorbed into W Q W^{Q}, and W U ​ V W^{UV} can be absorbed into W O W^{O}, we even do not need to compute keys and values out for attention.
Figure 3 intuitively illustrates how the KV joint compression in MLA reduces the KV cache.

Moreover, in order to reduce the activation memory during training, we also perform low-rank compression for the queries, even if it cannot reduce the KV cache:

𝐜 t Q \displaystyle\mathbf{c}_{t}^{Q}

= W D ​ Q ​ 𝐡 t, \displaystyle=W^{DQ}\mathbf{h}_{t},

(12)

𝐪 t C \displaystyle\mathbf{q}_{t}^{C}

= W U ​ Q ​ 𝐜 t Q, \displaystyle=W^{UQ}\mathbf{c}_{t}^{Q},

(13)

where 𝐜 t Q ∈ ℝ d c ′ \mathbf{c}_{t}^{Q}\in\mathbb{R}^{d_{c}^{\prime}} is the compressed latent vector for queries;
d c ′ ( ≪ d h ​ n h ) d_{c}^{\prime}(\ll d_{h}n_{h}) denotes the query compression dimension;
and W D ​ Q ∈ ℝ d c ′ × d, W U ​ Q ∈ ℝ d h ​ n h × d c ′ W^{DQ}\in\mathbb{R}^{d_{c}^{\prime}\times d},W^{UQ}\in\mathbb{R}^{d_{h}n_{h}\times d_{c}^{\prime}} are the down-projection and up-projection matrices for queries, respectively.

2.1.3 Decoupled Rotary Position Embedding

Following DeepSeek 67B ( DeepSeek-AI 2024 ), we intend to use the Rotary Position Embedding (RoPE) ( Su et al. 2024 ) for DeepSeek-V2.
However, RoPE is incompatible with low-rank KV compression.
To be specific, RoPE is position-sensitive for both keys and queries.
If we apply RoPE for the keys 𝐤 t C \mathbf{k}_{t}^{C}, W U ​ K W^{UK} in Equation 10 will be coupled with a position-sensitive RoPE matrix.
In this way, W U ​ K W^{UK} cannot be absorbed into W Q W^{Q} any more during inference, since a RoPE matrix related to the currently generating token will lie between W Q W^{Q} and W U ​ K W^{UK} and matrix multiplication does not obey a commutative law.
As a result, we must recompute the keys for all the prefix tokens during inference, which will significantly hinder the inference efficiency.

As a solution, we propose the decoupled RoPE strategy that uses additional multi-head queries 𝐪 t, i R ∈ ℝ d h R \mathbf{q}_{t,i}^{R}\in\mathbb{R}^{d_{h}^{R}} and a shared key 𝐤 t R ∈ ℝ d h R \mathbf{k}_{t}^{R}\in\mathbb{R}^{d_{h}^{R}} to carry RoPE, where d h R d_{h}^{R} denotes the per-head dimension of the decoupled queries and key.
Equipped with the decoupled RoPE strategy, MLA performs the following computation:

[ 𝐪 t, 1 R; 𝐪 t, 2 R; …; 𝐪 t, n h R ] = 𝐪 t R \displaystyle[\mathbf{q}_{t,1}^{R};\mathbf{q}_{t,2}^{R};...;\mathbf{q}_{t,n_{h}}^{R}]=\mathbf{q}_{t}^{R}

= RoPE ⁡ ( W Q ​ R ​ 𝐜 t Q ), \displaystyle=\operatorname{RoPE}({W^{QR}}\mathbf{c}_{t}^{Q}),

(14)

𝐤 t R \displaystyle\mathbf{k}_{t}^{R}

= RoPE ⁡ ( W K ​ R ​ 𝐡 t ), \displaystyle=\operatorname{RoPE}({W^{KR}}\mathbf{h}_{t}),

(15)

𝐪 t, i \displaystyle\mathbf{q}_{t,i}

= [ 𝐪 t, i C; 𝐪 t, i R ], \displaystyle=[\mathbf{q}_{t,i}^{C};\mathbf{q}_{t,i}^{R}],

(16)

𝐤 t, i \displaystyle\mathbf{k}_{t,i}

= [ 𝐤 t, i C; 𝐤 t R ], \displaystyle=[\mathbf{k}_{t,i}^{C};\mathbf{k}_{t}^{R}],

(17)

𝐨 t, i \displaystyle\mathbf{o}_{t,i}

= ∑ j = 1 t Softmax j ⁡ ( 𝐪 t, i T ​ 𝐤 j, i d h + d h R ) ​ 𝐯 j, i C, \displaystyle=\sum_{j=1}^{t}\operatorname{Softmax}_{j}(\frac{\mathbf{q}_{t,i}^{T}\mathbf{k}_{j,i}}{\sqrt{d_{h}+d_{h}^{R}}})\mathbf{v}_{j,i}^{C},

(18)

𝐮 t \displaystyle\mathbf{u}_{t}

= W O ​ [ 𝐨 t, 1; 𝐨 t, 2; …; 𝐨 t, n h ], \displaystyle=W^{O}[\mathbf{o}_{t,1};\mathbf{o}_{t,2};...;\mathbf{o}_{t,n_{h}}],

(19)

where W Q ​ R ∈ ℝ d h R ​ n h × d c ′ W^{QR}\in\mathbb{R}^{d_{h}^{R}n_{h}\times d_{c}^{\prime}} and W K ​ R ∈ ℝ d h R × d W^{KR}\in\mathbb{R}^{d_{h}^{R}\times d} are matrices to produce the decouples queries and key, respectively;
RoPE ⁡ ( ⋅ ) \operatorname{RoPE}(\cdot) denotes the operation that applies RoPE matrices;
and [ ⋅; ⋅ ] [\cdot;\cdot] denotes the concatenation operation.
During inference, the decoupled key should also be cached.
Therefore, DeepSeek-V2 requires a total KV cache containing ( d c + d h R ) ​ l (d_{c}+d_{h}^{R})l elements.

In order to demonstrate the complete computation process of MLA, we also organize and provide its full formulas in Appendix C.

Attention Mechanism

KV Cache per Token (# Element)

Capability

Multi-Head Attention (MHA)

2 ​ n h ​ d h ​ l 2n_{h}d_{h}l

Strong

Grouped-Query Attention (GQA)

2 ​ n g ​ d h ​ l 2n_{g}d_{h}l

Moderate

Multi-Query Attention (MQA)

2 ​ d h ​ l 2d_{h}l

Weak

MLA (Ours)

( d c + d h R ) ​ l ≈ 9 2 ​ d h ​ l \penalty\ \penalty\ \penalty\ \penalty\ \penalty\ \penalty\ \penalty\ \penalty\ (d_{c}+d_{h}^{R})l\approx\frac{9}{2}d_{h}l

Stronger

Table 1:
Comparison of the KV cache per token among different attention mechanisms.
n h n_{h} denotes the number of attention heads,
d h d_{h} denotes the dimension per attention head,
l l denotes the number of layers,
n g n_{g} denotes the number of groups in GQA,
and d c d_{c} and d h R d_{h}^{R} denote the KV compression dimension and the per-head dimension of the decoupled queries and key in MLA, respectively.
The amount of KV cache is measured by the number of elements, regardless of the storage precision.
For DeepSeek-V2, d c d_{c} is set to 4 ​ d h 4d_{h} and d h R d_{h}^{R} is set to d h 2 \frac{d_{h}}{2}.
So, its KV cache is equal to GQA with only 2.25 groups, but its performance is stronger than MHA.

2.1.4 Comparison of Key-Value Cache

We demonstrate a comparison of the KV cache per token among different attention mechanisms in Table 1.
MLA requires only a small amount of KV cache, equal to GQA with only 2.25 groups, but can achieve stronger performance than MHA.

2.2 DeepSeekMoE: Training Strong Models at Economical Costs

2.2.1 Basic Architecture

For FFNs, we employ the DeepSeekMoE architecture ( Dai et al. 2024 ).
DeepSeekMoE has two key ideas: segmenting experts into finer granularity for higher expert specialization and more accurate knowledge acquisition, and isolating some shared experts for mitigating knowledge redundancy among routed experts.
With the same number of activated and total expert parameters, DeepSeekMoE can outperform conventional MoE architectures like GShard ( Lepikhin et al. 2021 ) by a large margin.

Let 𝐮 t \mathbf{u}_{t} be the FFN input of the t t -th token, we compute the FFN output 𝐡 t ′ \mathbf{h}_{t}^{\prime} as follows:

𝐡 t ′ \displaystyle\mathbf{h}_{t}^{\prime}

= 𝐮 t + ∑ i = 1 N s FFN i ( s ) ⁡ ( 𝐮 t ) + ∑ i = 1 N r g i, t ​ FFN i ( r ) ⁡ ( 𝐮 t ), \displaystyle=\mathbf{u}_{t}+\sum_{i=1}^{N_{s}}{\operatorname{FFN}^{(s)}_{i}\left(\mathbf{u}_{t}\right)}+\sum_{i=1}^{N_{r}}{g_{i,t}\operatorname{FFN}^{(r)}_{i}\left(\mathbf{u}_{t}\right)},

(20)

g i, t \displaystyle g_{i,t}

= { s i, t, s i, t ∈ Topk ⁡ ( { s j, t | 1 ⩽ j ⩽ N r }, K r ), 0, otherwise, \displaystyle=\begin{cases}s_{i,t},&s_{i,t}\in\operatorname{Topk}(\{s_{j,t}|1\leqslant j\leqslant N_{r}\},K_{r}),\\
0,&\text{otherwise},\end{cases}

(21)

s i, t \displaystyle s_{i,t}

= Softmax i ⁡ ( 𝐮 t T ​ 𝐞 i ), \displaystyle=\operatorname{Softmax}_{i}\left({\mathbf{u}_{t}}^{T}\mathbf{e}_{i}\right),

(22)

where N s N_{s} and N r N_{r} denote the numbers of shared experts and routed experts, respectively;
FFN i ( s ) ⁡ ( ⋅ ) \operatorname{FFN}^{(s)}_{i}(\cdot) and FFN i ( r ) ⁡ ( ⋅ ) \operatorname{FFN}^{(r)}_{i}(\cdot) denote the i i -th shared expert and the i i -th routed expert, respectively;
K r K_{r} denotes the number of activated routed experts;
g i, t g_{i,t} is the gate value for the i i -th expert;
s i, t s_{i,t} is the token-to-expert affinity;
𝐞 i \mathbf{e}_{i} is the centroid of the i i -th routed expert in this layer;
and Topk ⁡ ( ⋅, K ) \operatorname{Topk}(\cdot,K) denotes the set comprising K K highest scores among the affinity scores calculated for the t t -th token and all routed experts.

2.2.2 Device-Limited Routing

We design a device-limited routing mechanism to bound MoE-related communication costs.
When expert parallelism is employed, the routed experts will be distributed across multiple devices.
For each token, its MoE-related communication frequency is proportional to the number of devices covered by its target experts.
Due to the fine-grained expert segmentation in DeepSeekMoE, the number of activated experts can be large, so the MoE-related communication will be more costly if we apply expert parallelism.

For DeepSeek-V2, beyond the naive top-K selection of routed experts, we additionally ensure that the target experts of each token will be distributed on at most M M devices.
To be specific, for each token, we first select M M devices that have experts with the highest affinity scores in them.
Then, we perform top-K selection among experts on these M M devices.
In practice, we find that when M ⩾ 3 M\geqslant 3, the device-limited routing can achieve a good performance roughly aligned with the unrestricted top-K routing.

2.2.3 Auxiliary Loss for Load Balance

We take the load balance into consideration for automatically learned routing strategies.
Firstly, unbalanced load will raise the risk of routing collapse ( Shazeer et al. 2017 ), preventing some experts being fully trained and utilized.
Secondly, when expert parallelism is employed, unbalanced load will diminish computation efficiency.
During the training of DeepSeek-V2, we design three kinds of auxiliary losses, for controlling expert-level load balance ( ℒ ExpBal \mathcal{L}_{\mathrm{ExpBal}} ), device-level load balance ( ℒ DevBal \mathcal{L}_{\mathrm{DevBal}} ), and communication balance ( ℒ CommBal \mathcal{L}_{\mathrm{CommBal}} ), respectively.

Expert-Level Balance Loss.

We use an expert-level balance loss ( Fedus et al. 2021; Lepikhin et al. 2021 ) to mitigate the risk of routing collapse:

ℒ ExpBal \displaystyle\mathcal{L}_{\mathrm{ExpBal}}

= α 1 ​ ∑ i = 1 N r f i ​ P i, \displaystyle=\alpha_{1}\sum_{i=1}^{N_{r}}{f_{i}P_{i}},

(23)

f i \displaystyle f_{i}

= N r K r ​ T ​ ∑ t = 1 T 𝟙 ​ ( Token t selects Expert i ), \displaystyle=\frac{N_{r}}{K_{r}T}\sum_{t=1}^{T}{\mathds{1}(\text{Token $t$ selects Expert $i$})},

(24)

P i \displaystyle P_{i}

= 1 T ​ ∑ t = 1 T s i, t, \displaystyle=\frac{1}{T}\sum_{t=1}^{T}{s_{i,t}},

(25)

where α 1 \alpha_{1} is a hyper-parameter called expert-level balance factor;
𝟙 ​ ( ⋅ ) \mathds{1}(\cdot) denotes the indicator function;
and T T denotes the number of tokens in a sequence.

Device-Level Balance Loss.

In addition to the expert-level balance loss, we additionally design a device-level balance loss to ensure balanced computation across different devices.
In the training process of DeepSeek-V2, we partition all routed experts into D D groups { ℰ 1, ℰ 2, …, ℰ D } \{\mathcal{E}_{1},\mathcal{E}_{2},...,\mathcal{E}_{D}\}, and deploy each group on a single device.
The device-level balance loss is computed as follows:

ℒ DevBal \displaystyle\mathcal{L}_{\mathrm{DevBal}}

= α 2 ​ ∑ i = 1 D f i ′ ​ P i ′, \displaystyle=\alpha_{2}\sum_{i=1}^{D}{f_{i}^{\prime}P_{i}^{\prime}},

(26)

f i ′ \displaystyle f_{i}^{\prime}

= 1 | ℰ i | ​ ∑ j ∈ ℰ i f j, \displaystyle=\frac{1}{|\mathcal{E}_{i}|}\sum_{j\in\mathcal{E}_{i}}{f_{j}},

(27)

P i ′ \displaystyle P_{i}^{\prime}

= ∑ j ∈ ℰ i P j, \displaystyle=\sum_{j\in\mathcal{E}_{i}}{P_{j}},

(28)

where α 2 \alpha_{2} is a hyper-parameter called device-level balance factor.

Communication Balance Loss.

Finally, we introduce a communication balance loss to ensure that the communication of each device is balanced.
Although the device-limited routing mechanism guarantees that the sending communication of each device is bounded, if a certain device receives more tokens than other devices, the practical communication efficiency will also be affected.
In order to mitigate this issue, we design a communication balance loss as follows:

ℒ CommBal \displaystyle\mathcal{L}_{\mathrm{CommBal}}

= α 3 ​ ∑ i = 1 D f i ′′ ​ P i ′′, \displaystyle=\alpha_{3}\sum_{i=1}^{D}{f_{i}^{\prime\prime}P_{i}^{\prime\prime}},

(29)

f i ′′ \displaystyle f_{i}^{\prime\prime}

= D M ​ T ​ ∑ t = 1 T 𝟙 ​ ( Token t is sent to Device i ), \displaystyle=\frac{D}{MT}\sum_{t=1}^{T}{\mathds{1}(\text{Token $t$ is sent to Device $i$})},

(30)

P i ′′ \displaystyle P_{i}^{\prime\prime}

= ∑ j ∈ ℰ i P j, \displaystyle=\sum_{j\in\mathcal{E}_{i}}{P_{j}},

(31)

where α 3 \alpha_{3} is a hyper-parameter called communication balance factor.
The device-limited routing mechanism operates on the principle of ensuring that each device transmits at most M ​ T MT hidden states to other devices.
Simultaneously, the communication balance loss is employed to encourage each device to receive around M ​ T MT hidden states from other devices.
The communication balance loss guarantees a balanced exchange of information among devices, promoting efficient communications.

2.2.4 Token-Dropping Strategy

While balance losses aim to encourage a balanced load, it is important to acknowledge that they cannot guarantee a strict load balance.
In order to further mitigate the computation wastage caused by unbalanced load, we introduce a device-level token-dropping strategy during training.
This approach first computes the average computational budget for each device, which means that the capacity factor for each device is equivalent to 1.0.
Then, inspired by Riquelme et al. 2021, we drop tokens with the lowest affinity scores on each device until reaching the computational budget.
In addition, we ensure that the tokens belonging to approximately 10% of the training sequences will never be dropped.
In this way, we can flexibly decide whether to drop tokens during inference according to the efficiency requirements, and always ensure consistency between training and inference.

3 Pre-Training

3.1 Experimental Setups

3.1.1 Data Construction

While maintaining the same data processing stages as for DeepSeek 67B ( DeepSeek-AI 2024 ), we extend the amount of data and elevate the data quality.
In order to enlarge our pre-training corpus, we explore the potential of the internet data and optimize our cleaning processes, thus recovering a large amount of mistakenly deleted data.
Moreover, we incorporate more Chinese data, aiming to better leverage the corpus available on the Chinese internet.
In addition to the amount of data, we also focus on the data quality.
We enrich our pre-training corpus with high-quality data from various sources, and meanwhile improve the quality-based filtering algorithm.
The improved algorithm ensures that a large amount of non-beneficial data will be removed, while the valuable data will be mostly retained.
In addition, we filter out the contentious content from our pre-training corpus to mitigate the data bias introduced from specific regional cultures.
A detailed discussion about the influence of this filtering strategy is presented in Appendix E.

We adopt the same tokenizer as used in DeepSeek 67B, which is built based on the Byte-level Byte-Pair Encoding (BBPE) algorithm and has a vocabulary size of 100K.
Our tokenized pre-training corpus contains 8.1T tokens, where Chinese tokens are approximately 12% more than English ones.

3.1.2 Hyper-Parameters

Model Hyper-Parameters.

We set the number of Transformer layers to 60 and the hidden dimension to 5120.
All learnable parameters are randomly initialized with a standard deviation of 0.006.
In MLA, we set the number of attention heads n h n_{h} to 128 and the per-head dimension d h d_{h} to 128.
The KV compression dimension d c d_{c} is set to 512, and the query compression dimension d c ′ d_{c}^{\prime} is set to 1536.
For the decoupled queries and key, we set the per-head dimension d h R d_{h}^{R} to 64.
Following Dai et al. 2024, we substitute all FFNs except for the first layer with MoE layers.
Each MoE layer consists of 2 shared experts and 160 routed experts, where the intermediate hidden dimension of each expert is 1536.
Among the routed experts, 6 experts will be activated for each token.
In addition, the low-rank compression and fine-grained expert segmentation will impact the output scale of a layer.
Therefore, in practice, we employ additional RMS Norm layers after the compressed latent vectors, and multiply additional scaling factors at the width bottlenecks (i.e., the compressed latent vectors and the intermediate hidden states of routed experts) to ensure stable training.
Under this configuration, DeepSeek-V2 comprises 236B total parameters, of which 21B are activated for each token.

Training Hyper-Parameters.

We employ the AdamW optimizer ( Loshchilov and Hutter 2017 ) with hyper-parameters set to β 1 = 0.9 \beta_{1}=0.9, β 2 = 0.95 \beta_{2}=0.95, and weight ​ _ ​ decay = 0.1 \mathrm{weight\_decay}=0.1.
The learning rate is scheduled using a warmup-and-step-decay strategy ( DeepSeek-AI 2024 ).
Initially, the learning rate linearly increases from 0 to the maximum value during the first 2K steps.
Subsequently, the learning rate is multiplied by 0.316 after
training about 60% of tokens, and again by 0.316 after training about 90% of tokens.
The maximum learning rate is set to 2.4 × 10 − 4 2.4\times 10^{-4}, and the gradient clipping norm is set to 1.0.
We also use a batch size scheduling strategy, where the batch size is gradually increased from 2304 to 9216 in the training of the first 225B tokens, and then keeps 9216 in the remaining training.
We set the maximum sequence length to 4K, and train DeepSeek-V2 on 8.1T tokens.
We leverage pipeline parallelism to deploy different layers of a model on different devices, and for each layer, the routed experts will be uniformly deployed on 8 devices ( D = 8 D=8 ).
As for the device-limited routing, each token will be sent to at most 3 devices ( M = 3 M=3 ).
As for balance losses, we set α 1 \alpha_{1} to 0.003, α 2 \alpha_{2} to 0.05, and α 3 \alpha_{3} to 0.02.
We employ the token-dropping strategy during training for acceleration, but do not drop any tokens for evaluation.

3.1.3 Infrastructures

DeepSeek-V2 is trained based on the HAI-LLM framework ( High-flyer 2023 ), an efficient and light-weight training framework developed internally by our engineers.
It employs a 16-way zero-bubble pipeline parallelism ( Qi et al. 2023 ), an 8-way expert parallelism ( Lepikhin et al. 2021 ), and ZeRO-1 data parallelism ( Rajbhandari et al. 2020 ).
Given that DeepSeek-V2 has relatively few activated parameters, and a portion of the operators are recomputed to save activation memory, it can be trained without the necessity of tensor parallelism, thereby decreasing the communication overhead.
Moreover, in order to further improve the training efficiency, we overlap the computation of shared experts with the expert parallel all-to-all communication.
We also customize faster CUDA kernels for communications, routing algorithms, and fused linear computations across different experts.
In addition, MLA is also optimized based on an improved version of FlashAttention-2 ( Dao 2023 ).

We conduct all experiments on a cluster equipped with NVIDIA H800 GPUs.
Each node in the H800 cluster contains 8 GPUs connected using NVLink and NVSwitch within nodes.
Across nodes, InfiniBand interconnects are utilized to facilitate communications.

Figure 4:
Evaluation results on the “Needle In A Haystack” (NIAH) tests.
DeepSeek-V2 performs well across all context window lengths up to 128K.

3.1.4 Long Context Extension

After the initial pre-training of DeepSeek-V2, we employ YaRN ( Peng et al. 2023 ) to extend the default context window length from 4K to 128K.
YaRN was specifically applied to the decoupled shared key 𝐤 t R \mathbf{k}^{R}_{t} as it is responsible for carrying RoPE ( Su et al. 2024 ).
For YaRN, we set the scale s s to 40, α \alpha to 1, β \beta to 32, and the target maximum context length to 160K.
Under these settings, we can expect the model to respond well for a context length of 128K.
Slightly diverging from original YaRN, due to our distinct attention mechanism, we adjust the length scaling factor to modulate the attention entropy.
The factor t \sqrt{t} is computed as t = 0.0707 ​ ln ⁡ s + 1 \sqrt{t}=0.0707\ln{s}+1, aiming at minimizing the perplexity.

We additionally train the model for 1000 steps, with a sequence length of 32K and a batch size of 576 sequences.
Although the training is conducted solely at the sequence length of 32K, the model still demonstrates robust performance when being evaluated at a context length of 128K.
As shown in Figure 4, the results on the “Needle In A Haystack” (NIAH) tests indicate that DeepSeek-V2 performs well across all context window lengths up to 128K.

3.2 Evaluations

3.2.1 Evaluation Benchmarks

DeepSeek-V2 is pretrained on a bilingual corpus, so we evaluate it on a series of benchmarks in English and Chinese.
Our evaluation is based on our internal evaluation framework integrated in our HAI-LLM framework.
Included benchmarks are categorized and listed as follows, where underlined benchmarks are in Chinese:

Multi-subject multiple-choice datasets include MMLU ( Hendrycks et al. 2020 ), C-Eval ( Huang et al. 2023 ), and CMMLU ( Li et al. 2023 ).

Language understanding and reasoning datasets include HellaSwag ( Zellers et al. 2019 ), PIQA ( Bisk et al. 2020 ), ARC ( Clark et al. 2018 ), and BigBench Hard (BBH) ( Suzgun et al. 2022 ).

Closed-book question answering datasets include TriviaQA ( Joshi et al. 2017 ) and NaturalQuestions ( Kwiatkowski et al. 2019 ).

Reading comprehension datasets include RACE Lai et al. 2017, DROP ( Dua et al. 2019 ), C3 ( Sun et al. 2019 ), and CMRC ( Cui et al. 2019 ).

Reference disambiguation datasets include WinoGrande Sakaguchi et al. 2019 and CLUEWSC ( Xu et al. 2020 ).

Language modeling datasets include Pile ( Gao et al. 2020 ).

Chinese understanding and culture datasets include CHID ( Zheng et al. 2019 ) and CCPM ( Li et al. 2021 ).

Math datasets include GSM8K ( Cobbe et al. 2021 ), MATH ( Hendrycks et al. 2021 ), and CMath ( Wei et al. 2023 ).

Code datasets include HumanEval ( Chen et al. 2021 ), MBPP ( Austin et al. 2021 ), and CRUXEval ( Gu et al. 2024 ).

Standardized exams include AGIEval ( Zhong et al. 2023 ).
Note that AGIEval includes both English and Chinese subsets.

Following our previous work ( DeepSeek-AI 2024 ), we adopt perplexity-based evaluation for datasets including HellaSwag, PIQA, WinoGrande, RACE-Middle, RACE-High, MMLU, ARC-Easy, ARC-Challenge, CHID, C-Eval, CMMLU, C3, and CCPM, and adopt generation-based evaluation for TriviaQA, NaturalQuestions, DROP, MATH, GSM8K, HumanEval, MBPP, CRUXEval, BBH, AGIEval, CLUEWSC, CMRC, and CMath.
In addition, we perform language-modeling-based evaluation for Pile-test and use Bits-Per-Byte (BPB) as the metric to guarantee fair comparison among models with different tokenizers.

For an intuitive overview of these benchmarks, we additionally provide our evaluation formats for each benchmark in Appendix G.

3.2.2 Evaluation Results

Benchmark (Metric)

# Shots

DeepSeek

Qwen1.5

Mixtral

LLaMA 3

DeepSeek-V2

67B

72B

8x22B

70B

Architecture

-

Dense

Dense

MoE

Dense

MoE

# Activated Params

-

67B

72B

39B

70B

21B

# Total Params

-

67B

72B

141B

70B

236B

English

Pile-test (BPB)

-

0.642

0.637

0.623

0.602

0.606

BBH (EM)

3-shot

68.7

59.9

78.9

81.0

78.9

MMLU (Acc.)

5-shot

71.3

77.2

77.6

78.9

78.5

DROP (F1)

3-shot

69.7

71.5

80.4

82.5

80.1

ARC-Easy (Acc.)

25-shot

95.3

97.1

97.3

97.9

97.6

ARC-Challenge (Acc.)

25-shot

86.4

92.8

91.2

93.3

92.4

HellaSwag (Acc.)

10-shot

86.3

85.8

86.6

87.9

84.2

PIQA (Acc.)

0-shot

83.6

83.3

83.6

85.0

83.7

WinoGrande (Acc.)

5-shot

84.9

82.4

83.7

85.7

84.9

RACE-Middle (Acc.)

5-shot

69.9

63.4

73.3

73.3

73.1

RACE-High (Acc.)

5-shot

50.7

47.0

56.7

57.9

52.7

TriviaQA (EM)

5-shot

78.9

73.1

82.1

81.6

79.9

NaturalQuestions (EM)

5-shot

36.6

35.6

39.6

40.2

38.7

AGIEval (Acc.)

0-shot

41.3

64.4

43.4

49.8

51.2

Code

HumanEval (Pass@1)

0-shot

45.1

43.9

53.1

48.2

48.8

MBPP (Pass@1)

3-shot

57.4

53.6

64.2

68.6

66.6

CRUXEval-I (Acc.)

2-shot

42.5

44.3

52.4

49.4

52.8

CRUXEval-O (Acc.)

2-shot

41.0

42.3

52.8

54.3

49.8

Math

GSM8K (EM)

8-shot

63.4

77.9

80.3

83.0

79.2

MATH (EM)

4-shot

18.7

41.4

42.5

42.2

43.6

CMath (EM)

3-shot

63.0

77.8

72.3

73.9

78.7

Chinese

CLUEWSC (EM)

5-shot

81.0

80.5

77.5

78.3

82.2

C-Eval (Acc.)

5-shot

66.1

83.7

59.6

67.5

81.7

CMMLU (Acc.)

5-shot

70.8

84.3

60.0

69.3

84.0

CMRC (EM)

1-shot

73.4

66.6

73.1

73.3

77.5

C3 (Acc.)

0-shot

75.3

78.2

71.4

74.0

77.4

CHID (Acc.)

0-shot

92.1

-

57.0

83.2

92.7

CCPM (Acc.)

0-shot

88.5

88.1

61.0

68.1

93.1

Table 2:
Comparison among DeepSeek-V2 and other representative open-source models.
All models are evaluated in our internal framework and share the same evaluation setting.
Bold denotes the best and underline denotes the second-best.
Scores with a gap smaller than 0.3 are regarded as at the same level.
With only 21B activated parameters, DeepSeek-V2 achieves top-tier performance among open-source models.

In Table 2, we compare DeepSeek-V2 with several representative open-source models, including DeepSeek 67B ( DeepSeek-AI 2024 ) (our previous release), Qwen1.5 72B ( Bai et al. 2023 ), LLaMA3 70B ( AI@Meta 2024 ), and Mixtral 8x22B ( Mistral 2024 ).
We evaluate all these models with our internal evaluation framework, and ensure that they share the same evaluation setting.
Overall, with only 21B activated parameters, DeepSeek-V2 significantly outperforms DeepSeek 67B on almost all benchmarks, and achieves top-tier performance among open-source models.

Further, we elaborately compare DeepSeek-V2 with its open-source counterparts one by one.
(1)
Compared with Qwen1.5 72B, another model that supports both Chinese and English, DeepSeek-V2 demonstrates overwhelming advantages on the majority of English, code, and math benchmarks.
As for Chinese benchmarks, Qwen1.5 72B shows better performance on multi-subject multiple-choice tasks while DeepSeek-V2 is comparable or better on others.
Note that for the CHID benchmark, the tokenizer of Qwen1.5 72B will encounter errors in our evaluation framework, so we leave the CHID score blank for Qwen1.5 72B.
(2)
Compared with Mixtral 8x22B, DeepSeek-V2 achieves comparable or better English performance, except for TriviaQA, NaturalQuestions, and HellaSwag, which are closely related to English commonsense knowledge.
Notably, DeepSeek-V2 outperforms Mixtral 8x22B on MMLU.
On code and math benchmarks, DeepSeek-V2 demonstrates comparable performance with Mixtral 8x22B.
Since Mixtral 8x22B is not specifically trained on Chinese data, its Chinese capability lags far behind DeepSeek-V2.
(3)
Compared with LLaMA3 70B, DeepSeek-V2 is trained on fewer than a quarter of English tokens.
Therefore, we acknowledge that DeepSeek-V2 still has a slight gap in basic English capabilities with LLaMA3 70B.
However, even with much fewer training tokens and activated parameters, DeepSeek-V2 still demonstrates comparable code and math capability with LLaMA3 70B.
Also, as a bilingual language model, DeepSeek-V2 outperforms LLaMA3 70B overwhelmingly on Chinese benchmarks.

Finally, it is worth mentioning that certain prior studies ( Hu et al. 2024 ) incorporate SFT data during the pre-training stage, whereas DeepSeek-V2 has never been exposed to SFT data during pre-training.

3.2.3 Training and Inference Efficiency

Training Costs.

Since DeepSeek-V2 activates fewer parameters for each token and requires fewer FLOPs than DeepSeek 67B, training DeepSeek-V2 will be more economical than training DeepSeek 67B theoretically.
Although training an MoE model will introduce additional communication overheads, through our operator and communication optimizations, the training for DeepSeek-V2 can attain a relatively high Model FLOPs Utilization (MFU).
During our practical training on the H800 cluster, for training on each trillion tokens, DeepSeek 67B requires 300.6K GPU hours, while DeepSeek-V2 needs only 172.8K GPU hours, i.e., sparse DeepSeek-V2 can save 42.5% training costs compared with dense DeepSeek 67B.

Inference Efficiency.

In order to efficiently deploy DeepSeek-V2 for service, we first convert its parameters into the precision of FP8.
In addition, we also perform KV cache quantization ( Hooper et al. 2024; Zhao et al. 2023 ) for DeepSeek-V2 to further compress each element in its KV cache into 6 bits on average.
Benefiting from MLA and these optimizations, actually deployed DeepSeek-V2 requires significantly less KV cache than DeepSeek 67B, and thus can serve a much larger batch size.
We evaluate the generation throughput of DeepSeek-V2 based on the prompt and generation length distribution from the actually deployed DeepSeek 67B service.
On a single node with 8 H800 GPUs, DeepSeek-V2 achieves a generation throughput exceeding 50K tokens per second, which is 5.76 times the maximum generation throughput of DeepSeek 67B.
In addition, the prompt input throughput of DeepSeek-V2 exceeds 100K tokens per second.

4 Alignment

4.1 Supervised Fine-Tuning

Building upon our prior research ( DeepSeek-AI 2024 ), we curate our instruction tuning datasets to include 1.5M instances, comprising 1.2M instances for helpfulness and 0.3M instances for safety.
In comparison to the initial version, we improve the data quality to mitigate hallucinatory responses and enhance writing proficiency.
We fine-tune DeepSeek-V2 with 2 epochs, and the learning rate is set to 5 × 10 − 6 5\times 10^{-6}.
For the evaluation of DeepSeek-V2 Chat (SFT), we mainly include generation-based benchmarks, except for several representative multiple-choice tasks (MMLU and ARC).
We also conduct an instruction-following evaluation (IFEval) ( Zhou et al. 2023 ) for DeepSeek-V2 Chat (SFT), using prompt-level loose accuracy as the metric.
Moreover, we employ LiveCodeBench ( Jain et al. 2024 ) questions from September 1st, 2023 to April 1st, 2024 to evaluate chat models.
In addition to the standard benchmarks, we further evaluate our model on open-ended conversation benchmarks including MT-Bench ( Zheng et al. 2023 ), AlpacaEval 2.0 ( Dubois et al. 2024 ), and AlignBench ( Liu et al. 2023 ).
For comparison, we also evaluate Qwen1.5 72B Chat, LLaMA-3-70B Instruct, and Mistral-8x22B Instruct in our evaluation framework and settings.
As for DeepSeek 67B Chat, w
