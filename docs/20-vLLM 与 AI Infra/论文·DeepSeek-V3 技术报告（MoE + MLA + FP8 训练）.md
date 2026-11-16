# 论文·DeepSeek-V3 技术报告（MoE + MLA + FP8 训练）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　269 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2412.19437
抓取: https://ar5iv.labs.arxiv.org/html/2412.19437

\reportnumber
001

DeepSeek-V3 Technical Report

DeepSeek-AI

research@deepseek.com

Abstract

We present DeepSeek-V3, a strong Mixture-of-Experts (MoE) language model with 671B total parameters with 37B activated for each token.
To achieve efficient inference and cost-effective training, DeepSeek-V3 adopts Multi-head Latent Attention (MLA) and DeepSeekMoE architectures, which were thoroughly validated in DeepSeek-V2.
Furthermore, DeepSeek-V3 pioneers an auxiliary-loss-free strategy for load balancing and sets a multi-token prediction training objective for stronger performance.
We pre-train DeepSeek-V3 on 14.8 trillion diverse and high-quality tokens, followed by Supervised Fine-Tuning and Reinforcement Learning stages to fully harness its capabilities.
Comprehensive evaluations reveal that DeepSeek-V3 outperforms other open-source models and achieves performance comparable to leading closed-source models.
Despite its excellent performance, DeepSeek-V3 requires only 2.788M H800 GPU hours for its full training.
In addition, its training process is remarkably stable.
Throughout the entire training process, we did not experience any irrecoverable loss spikes or perform any rollbacks.
The model checkpoints are available at https://github.com/deepseek-ai/DeepSeek-V3.

Figure 1:
Benchmark performance of DeepSeek-V3 and its counterparts.

Contents

1 Introduction

2 Architecture

2.1 Basic Architecture

2.1.1 Multi-Head Latent Attention

2.1.2 DeepSeekMoE with Auxiliary-Loss-Free Load Balancing

2.2 Multi-Token Prediction

3 Infrastructures

3.1 Compute Clusters

3.2 Training Framework

3.2.1 DualPipe and Computation-Communication Overlap

3.2.2 Efficient Implementation of Cross-Node All-to-All Communication

3.2.3 Extremely Memory Saving with Minimal Overhead

3.3 FP8 Training

3.3.1 Mixed Precision Framework

3.3.2 Improved Precision from Quantization and Multiplication

3.3.3 Low-Precision Storage and Communication

3.4 Inference and Deployment

3.4.1 Prefilling

3.4.2 Decoding

3.5 Suggestions on Hardware Design

3.5.1 Communication Hardware

3.5.2 Compute Hardware

4 Pre-Training

4.1 Data Construction

4.2 Hyper-Parameters

4.3 Long Context Extension

4.4 Evaluations

4.4.1 Evaluation Benchmarks

4.4.2 Evaluation Results

4.5 Discussion

4.5.1 Ablation Studies for Multi-Token Prediction

4.5.2 Ablation Studies for the Auxiliary-Loss-Free Balancing Strategy

4.5.3 Batch-Wise Load Balance VS. Sequence-Wise Load Balance

5 Post-Training

5.1 Supervised Fine-Tuning

5.2 Reinforcement Learning

5.2.1 Reward Model

5.2.2 Group Relative Policy Optimization

5.3 Evaluations

5.3.1 Evaluation Settings

5.3.2 Standard Evaluation

5.3.3 Open-Ended Evaluation

5.3.4 DeepSeek-V3 as a Generative Reward Model

5.4 Discussion

5.4.1 Distillation from DeepSeek-R1

5.4.2 Self-Rewarding

5.4.3 Multi-Token Prediction Evaluation

6 Conclusion, Limitations, and Future Directions

References

A Contributions and Acknowledgments

B Ablation Studies for Low-Precision Training

B.1 FP8 v.s. BF16 Training

B.2 Discussion About Block-Wise Quantization

C Expert Specialization Patterns of the 16B Aux-Loss-Based and Aux-Loss-Free Models

1 Introduction

In recent years, Large Language Models (LLMs) have been undergoing rapid iteration and evolution ( OpenAI 2024a; Anthropic 2024; Google 2024 ), progressively diminishing the gap towards Artificial General Intelligence (AGI).
Beyond closed-source models, open-source models, including DeepSeek series ( DeepSeek-AI 2024b; DeepSeek-AI 2024c; Guo et al. 2024; DeepSeek-AI 2024a ), LLaMA series ( Touvron et al. 2023a; Touvron et al. 2023b; AI@Meta 2024a; AI@Meta 2024b ), Qwen series ( Qwen 2023; Qwen 2024a; Qwen 2024b ), and Mistral series ( Jiang et al. 2023; Mistral 2024 ), are also making significant strides, endeavoring to close the gap with their closed-source counterparts.
To further push the boundaries of open-source model capabilities, we scale up our models and introduce DeepSeek-V3, a large Mixture-of-Experts (MoE) model with 671B parameters, of which 37B are activated for each token.

With a forward-looking perspective, we consistently strive for strong model performance and economical costs.
Therefore, in terms of architecture, DeepSeek-V3 still adopts Multi-head Latent Attention (MLA) ( DeepSeek-AI 2024c ) for efficient inference and DeepSeekMoE ( Dai et al. 2024 ) for cost-effective training.
These two architectures have been validated in DeepSeek-V2 ( DeepSeek-AI 2024c ), demonstrating their capability to maintain robust model performance while achieving efficient training and inference.
Beyond the basic architecture, we implement two additional strategies to further enhance the model capabilities.
Firstly, DeepSeek-V3 pioneers an auxiliary-loss-free strategy ( Wang et al. 2024a ) for load balancing, with the aim of minimizing the adverse impact on model performance that arises from the effort to encourage load balancing.
Secondly, DeepSeek-V3 employs a multi-token prediction training objective, which we have observed to enhance the overall performance on evaluation benchmarks.

In order to achieve efficient training, we support the FP8 mixed precision training and implement comprehensive optimizations for the training framework.
Low-precision training has emerged as a promising solution for efficient training ( Kalamkar et al. 2019; Narang et al. 2017; Peng et al. 2023b; Dettmers et al. 2022 ), its evolution being closely tied to advancements in hardware capabilities ( Micikevicius et al. 2022; Luo et al. 2024; Rouhani et al. 2023a ).
In this work, we introduce an FP8 mixed precision training framework and, for the first time, validate its effectiveness on an extremely large-scale model.
Through the support for FP8 computation and storage, we achieve both accelerated training and reduced GPU memory usage.
As for the training framework, we design the DualPipe algorithm for efficient pipeline parallelism, which has fewer pipeline bubbles and hides most of the communication during training through computation-communication overlap.
This overlap ensures that, as the model further scales up, as long as we maintain a constant computation-to-communication ratio, we can still employ fine-grained experts across nodes while achieving a near-zero all-to-all communication overhead.
In addition, we also develop efficient cross-node all-to-all communication kernels to fully utilize InfiniBand (IB) and NVLink bandwidths.
Furthermore, we meticulously optimize the memory footprint, making it possible to train DeepSeek-V3 without using costly tensor parallelism.
Combining these efforts, we achieve high training efficiency.

During pre-training, we train DeepSeek-V3 on 14.8T high-quality and diverse tokens.
The pre-training process is remarkably stable.
Throughout the entire training process, we did not encounter any irrecoverable loss spikes or have to roll back.
Next, we conduct a two-stage context length extension for DeepSeek-V3. In the first stage, the maximum context length is extended to 32K, and in the second stage, it is further extended to 128K.
Following this, we conduct post-training, including Supervised Fine-Tuning (SFT) and Reinforcement Learning (RL) on the base model of DeepSeek-V3, to align it with human preferences and further unlock its potential.
During the post-training stage, we distill the reasoning capability from the DeepSeek-R1 series of models, and meanwhile carefully maintain the balance between model accuracy and generation length.

We evaluate DeepSeek-V3 on a comprehensive array of benchmarks.
Despite its economical training costs, comprehensive evaluations reveal that DeepSeek-V3-Base has emerged as the strongest open-source base model currently available, especially in code and math.
Its chat version also outperforms other open-source models and achieves performance comparable to leading closed-source models, including GPT-4o and Claude-3.5-Sonnet, on a series of standard and open-ended benchmarks.

Training Costs

Pre-Training

Context Extension

Post-Training

Total

in H800 GPU Hours

2664K

119K

5K

2788K

in USD

$5.328M

$0.238M

$0.01M

$5.576M

Table 1:
Training costs of DeepSeek-V3, assuming the rental price of H800 is $2 per GPU hour.

Lastly, we emphasize again the economical training costs of DeepSeek-V3, summarized in Table 1, achieved through our optimized co-design of algorithms, frameworks, and hardware.
During the pre-training stage, training DeepSeek-V3 on each trillion tokens requires only 180K H800 GPU hours, i.e., 3.7 days on our cluster with 2048 H800 GPUs.
Consequently, our pre-training stage is completed in less than two months and costs 2664K GPU hours.
Combined with 119K GPU hours for the context length extension and 5K GPU hours for post-training, DeepSeek-V3 costs only 2.788M GPU hours for its full training.
Assuming the rental price of the H800 GPU is $2 per GPU hour, our total training costs amount to only $5.576M.
Note that the aforementioned costs include only the official training of DeepSeek-V3, excluding the costs associated with prior research and ablation experiments on architectures, algorithms, or data.

Our main contribution includes:

Architecture: Innovative Load Balancing Strategy and Training Objective

•

On top of the efficient architecture of DeepSeek-V2, we pioneer an auxiliary-loss-free strategy for load balancing, which minimizes the performance degradation that arises from encouraging load balancing.

•

We investigate a Multi-Token Prediction (MTP) objective and prove it beneficial to model performance.
It can also be used for speculative decoding for inference acceleration.

Pre-Training: Towards Ultimate Training Efficiency

•

We design an FP8 mixed precision training framework and, for the first time, validate the feasibility and effectiveness of FP8 training on an extremely large-scale model.

•

Through the co-design of algorithms, frameworks, and hardware, we overcome the communication bottleneck in cross-node MoE training, achieving near-full computation-communication overlap.
This significantly enhances our training efficiency and reduces the training costs, enabling us to further scale up the model size without additional overhead.

•

At an economical cost of only 2.664M H800 GPU hours, we complete the pre-training of DeepSeek-V3 on 14.8T tokens, producing the currently strongest open-source base model.
The subsequent training stages after pre-training require only 0.1M GPU hours.

Post-Training: Knowledge Distillation from DeepSeek-R1

•

We introduce an innovative methodology to distill reasoning capabilities from the long-Chain-of-Thought (CoT) model, specifically from one of the DeepSeek R1 series models, into standard LLMs, particularly DeepSeek-V3.
Our pipeline elegantly incorporates the verification and reflection patterns of R1 into DeepSeek-V3 and notably improves its reasoning performance.
Meanwhile, we also maintain control over the output style and length of DeepSeek-V3.

Summary of Core Evaluation Results

•

Knowledge :
(1)
On educational benchmarks such as MMLU, MMLU-Pro, and GPQA, DeepSeek-V3 outperforms all other open-source models, achieving 88.5 on MMLU, 75.9 on MMLU-Pro, and 59.1 on GPQA.
Its performance is comparable to leading closed-source models like GPT-4o and Claude-Sonnet-3.5, narrowing the gap between open-source and closed-source models in this domain.
(2)
For factuality benchmarks, DeepSeek-V3 demonstrates superior performance among open-source models on both SimpleQA and Chinese SimpleQA.
While it trails behind GPT-4o and Claude-Sonnet-3.5 in English factual knowledge (SimpleQA), it surpasses these models in Chinese factual knowledge (Chinese SimpleQA), highlighting its strength in Chinese factual knowledge.

•

Code, Math, and Reasoning :
(1)
DeepSeek-V3 achieves state-of-the-art performance on math-related benchmarks among all non-long-CoT open-source and closed-source models.
Notably, it even outperforms o1-preview on specific benchmarks, such as MATH-500, demonstrating its robust mathematical reasoning capabilities.
(2)
On coding-related tasks, DeepSeek-V3 emerges as the top-performing model for coding competition benchmarks, such as LiveCodeBench, solidifying its position as the leading model in this domain.
For engineering-related tasks, while DeepSeek-V3 performs slightly below Claude-Sonnet-3.5, it still outpaces all other models by a significant margin, demonstrating its competitiveness across diverse technical benchmarks.

In the remainder of this paper, we first present a detailed exposition of our DeepSeek-V3 model architecture (Section 2 ).
Subsequently, we introduce our infrastructures, encompassing our compute clusters, the training framework, the support for FP8 training, the inference deployment strategy, and our suggestions on future hardware design.
Next, we describe our pre-training process, including the construction of training data, hyper-parameter settings, long-context extension techniques, the associated evaluations, as well as some discussions (Section 4 ).
Thereafter, we discuss our efforts on post-training, which include Supervised Fine-Tuning (SFT), Reinforcement Learning (RL), the corresponding evaluations, and discussions (Section 5 ).
Lastly, we conclude this work, discuss existing limitations of DeepSeek-V3, and propose potential directions for future research (Section 6 ).

2 Architecture

We first introduce the basic architecture of DeepSeek-V3, featured by Multi-head Latent Attention (MLA) ( DeepSeek-AI 2024c ) for efficient inference and DeepSeekMoE ( Dai et al. 2024 ) for economical training.
Then, we present a Multi-Token Prediction (MTP) training objective, which we have observed to enhance the overall performance on evaluation benchmarks.
For other minor details not explicitly mentioned, DeepSeek-V3 adheres to the settings of DeepSeek-V2 ( DeepSeek-AI 2024c ).

Figure 2:
Illustration of the basic architecture of DeepSeek-V3.
Following DeepSeek-V2, we adopt MLA and DeepSeekMoE for efficient inference and economical training.

2.1 Basic Architecture

The basic architecture of DeepSeek-V3 is still within the Transformer ( Vaswani et al. 2017 ) framework.
For efficient inference and economical training, DeepSeek-V3 also adopts MLA and DeepSeekMoE, which have been thoroughly validated by DeepSeek-V2.
Compared with DeepSeek-V2, an exception is that we additionally introduce an auxiliary-loss-free load balancing strategy ( Wang et al. 2024a ) for DeepSeekMoE to mitigate the performance degradation induced by the effort to ensure load balance.
Figure 2 illustrates the basic architecture of DeepSeek-V3, and we will briefly review the details of MLA and DeepSeekMoE in this section.

2.1.1 Multi-Head Latent Attention

For attention, DeepSeek-V3 adopts the MLA architecture.
Let d d denote the embedding dimension, n h n_{h} denote the number of attention heads, d h d_{h} denote the dimension per head, and 𝐡 t ∈ ℝ d \mathbf{h}_{t}\in\mathbb{R}^{d} denote the attention input for the t t -th token at a given attention layer.
The core of MLA is the low-rank joint compression for attention keys and values to reduce Key-Value (KV) cache during inference:

𝐜 t K ​ V \displaystyle\boxed{\color[rgb]{0,0,1}\mathbf{c}_{t}^{KV}}

= W D ​ K ​ V ​ 𝐡 t, \displaystyle=W^{DKV}\mathbf{h}_{t},

(1)

[ 𝐤 t, 1 C; 𝐤 t, 2 C; …; 𝐤 t, n h C ] = 𝐤 t C \displaystyle[\mathbf{k}_{t,1}^{C};\mathbf{k}_{t,2}^{C};...;\mathbf{k}_{t,n_{h}}^{C}]=\mathbf{k}_{t}^{C}

= W U ​ K ​ 𝐜 t K ​ V, \displaystyle=W^{UK}\mathbf{c}_{t}^{KV},

(2)

𝐤 t R \displaystyle\boxed{\color[rgb]{0,0,1}\mathbf{k}_{t}^{R}}

= RoPE ⁡ ( W K ​ R ​ 𝐡 t ), \displaystyle=\operatorname{RoPE}({W^{KR}}\mathbf{h}_{t}),

(3)

𝐤 t, i \displaystyle\mathbf{k}_{t,i}

= [ 𝐤 t, i C; 𝐤 t R ], \displaystyle=[\mathbf{k}_{t,i}^{C};\mathbf{k}_{t}^{R}],

(4)

[ 𝐯 t, 1 C; 𝐯 t, 2 C; …; 𝐯 t, n h C ] = 𝐯 t C \displaystyle[\mathbf{v}_{t,1}^{C};\mathbf{v}_{t,2}^{C};...;\mathbf{v}_{t,n_{h}}^{C}]=\mathbf{v}_{t}^{C}

= W U ​ V ​ 𝐜 t K ​ V, \displaystyle=W^{UV}\mathbf{c}_{t}^{KV},

(5)

where 𝐜 t K ​ V ∈ ℝ d c \mathbf{c}_{t}^{KV}\in\mathbb{R}^{d_{c}} is the compressed latent vector for keys and values;
d c ( ≪ d h ​ n h ) d_{c}(\ll d_{h}n_{h}) indicates the KV compression dimension;
W D ​ K ​ V ∈ ℝ d c × d W^{DKV}\in\mathbb{R}^{d_{c}\times d} denotes the down-projection matrix;
W U ​ K, W U ​ V ∈ ℝ d h ​ n h × d c W^{UK},W^{UV}\in\mathbb{R}^{d_{h}n_{h}\times d_{c}} are the up-projection matrices for keys and values, respectively;
W K ​ R ∈ ℝ d h R × d W^{KR}\in\mathbb{R}^{d_{h}^{R}\times d} is the matrix used to produce the decoupled key that carries Rotary Positional Embedding (RoPE) ( Su et al. 2024 );
RoPE ⁡ ( ⋅ ) \operatorname{RoPE}(\cdot) denotes the operation that applies RoPE matrices;
and [ ⋅; ⋅ ] [\cdot;\cdot] denotes concatenation.
Note that for MLA, only the blue-boxed vectors (i.e., 𝐜 t K ​ V \color[rgb]{0,0,1}\mathbf{c}_{t}^{KV} and 𝐤 t R \color[rgb]{0,0,1}\mathbf{k}_{t}^{R} ) need to be cached during generation, which results in significantly reduced KV cache while maintaining performance comparable to standard Multi-Head Attention (MHA) ( Vaswani et al. 2017 ).

For the attention queries, we also perform a low-rank compression, which can reduce the activation memory during training:

𝐜 t Q \displaystyle\mathbf{c}_{t}^{Q}

= W D ​ Q ​ 𝐡 t, \displaystyle=W^{DQ}\mathbf{h}_{t},

(6)

[ 𝐪 t, 1 C; 𝐪 t, 2 C; …; 𝐪 t, n h C ] = 𝐪 t C \displaystyle[\mathbf{q}_{t,1}^{C};\mathbf{q}_{t,2}^{C};...;\mathbf{q}_{t,n_{h}}^{C}]=\mathbf{q}_{t}^{C}

= W U ​ Q ​ 𝐜 t Q, \displaystyle=W^{UQ}\mathbf{c}_{t}^{Q},

(7)

[ 𝐪 t, 1 R; 𝐪 t, 2 R; …; 𝐪 t, n h R ] = 𝐪 t R \displaystyle[\mathbf{q}_{t,1}^{R};\mathbf{q}_{t,2}^{R};...;\mathbf{q}_{t,n_{h}}^{R}]=\mathbf{q}_{t}^{R}

= RoPE ⁡ ( W Q ​ R ​ 𝐜 t Q ), \displaystyle=\operatorname{RoPE}({W^{QR}}\mathbf{c}_{t}^{Q}),

(8)

𝐪 t, i \displaystyle\mathbf{q}_{t,i}

= [ 𝐪 t, i C; 𝐪 t, i R ], \displaystyle=[\mathbf{q}_{t,i}^{C};\mathbf{q}_{t,i}^{R}],

(9)

where 𝐜 t Q ∈ ℝ d c ′ \mathbf{c}_{t}^{Q}\in\mathbb{R}^{d_{c}^{\prime}} is the compressed latent vector for queries;
d c ′ ( ≪ d h ​ n h ) d_{c}^{\prime}(\ll d_{h}n_{h}) denotes the query compression dimension;
W D ​ Q ∈ ℝ d c ′ × d, W U ​ Q ∈ ℝ d h ​ n h × d c ′ W^{DQ}\in\mathbb{R}^{d_{c}^{\prime}\times d},W^{UQ}\in\mathbb{R}^{d_{h}n_{h}\times d_{c}^{\prime}} are the down-projection and up-projection matrices for queries, respectively;
and W Q ​ R ∈ ℝ d h R ​ n h × d c ′ W^{QR}\in\mathbb{R}^{d_{h}^{R}n_{h}\times d_{c}^{\prime}} is the matrix to produce the decoupled queries that carry RoPE.

Ultimately, the attention queries ( 𝐪 t, i \mathbf{q}_{t,i} ), keys ( 𝐤 j, i \mathbf{k}_{j,i} ), and values ( 𝐯 j, i C \mathbf{v}_{j,i}^{C} ) are combined to yield the final attention output 𝐮 t \mathbf{u}_{t} :

𝐨 t, i \displaystyle\mathbf{o}_{t,i}

= ∑ j = 1 t Softmax j ⁡ ( 𝐪 t, i T ​ 𝐤 j, i d h + d h R ) ​ 𝐯 j, i C, \displaystyle=\sum_{j=1}^{t}\operatorname{Softmax}_{j}(\frac{\mathbf{q}_{t,i}^{T}\mathbf{k}_{j,i}}{\sqrt{d_{h}+d_{h}^{R}}})\mathbf{v}_{j,i}^{C},

(10)

𝐮 t \displaystyle\mathbf{u}_{t}

= W O ​ [ 𝐨 t, 1; 𝐨 t, 2; …; 𝐨 t, n h ], \displaystyle=W^{O}[\mathbf{o}_{t,1};\mathbf{o}_{t,2};...;\mathbf{o}_{t,n_{h}}],

(11)

where W O ∈ ℝ d × d h ​ n h W^{O}\in\mathbb{R}^{d\times d_{h}n_{h}} denotes the output projection matrix.

2.1.2 DeepSeekMoE with Auxiliary-Loss-Free Load Balancing

Basic Architecture of DeepSeekMoE.

For Feed-Forward Networks (FFNs), DeepSeek-V3 employs the DeepSeekMoE architecture ( Dai et al. 2024 ).
Compared with traditional MoE architectures like GShard ( Lepikhin et al. 2021 ), DeepSeekMoE uses finer-grained experts and isolates some experts as shared ones.
Let 𝐮 t \mathbf{u}_{t} denote the FFN input of the t t -th token, we compute the FFN output 𝐡 t ′ \mathbf{h}_{t}^{\prime} as follows:

𝐡 t ′ \displaystyle\mathbf{h}_{t}^{\prime}

= 𝐮 t + ∑ i = 1 N s FFN i ( s ) ⁡ ( 𝐮 t ) + ∑ i = 1 N r g i, t ​ FFN i ( r ) ⁡ ( 𝐮 t ), \displaystyle=\mathbf{u}_{t}+\sum_{i=1}^{N_{s}}{\operatorname{FFN}^{(s)}_{i}\left(\mathbf{u}_{t}\right)}+\sum_{i=1}^{N_{r}}{g_{i,t}\operatorname{FFN}^{(r)}_{i}\left(\mathbf{u}_{t}\right)},

(12)

g i, t \displaystyle g_{i,t}

= g i, t ′ ∑ j = 1 N r g j, t ′, \displaystyle=\frac{g^{\prime}_{i,t}}{\sum_{j=1}^{N_{r}}g^{\prime}_{j,t}},

(13)

g i, t ′ \displaystyle g^{\prime}_{i,t}

= { s i, t, s i, t ∈ Topk ⁡ ( { s j, t | 1 ⩽ j ⩽ N r }, K r ), 0, otherwise, \displaystyle=\begin{cases}s_{i,t},&s_{i,t}\in\operatorname{Topk}(\{s_{j,t}|1\leqslant j\leqslant N_{r}\},K_{r}),\\
0,&\text{otherwise},\end{cases}

(14)

s i, t \displaystyle s_{i,t}

= Sigmoid ⁡ ( 𝐮 t T ​ 𝐞 i ), \displaystyle=\operatorname{Sigmoid}\left({\mathbf{u}_{t}}^{T}\mathbf{e}_{i}\right),

(15)

where N s N_{s} and N r N_{r} denote the numbers of shared experts and routed experts, respectively;
FFN i ( s ) ⁡ ( ⋅ ) \operatorname{FFN}^{(s)}_{i}(\cdot) and FFN i ( r ) ⁡ ( ⋅ ) \operatorname{FFN}^{(r)}_{i}(\cdot) denote the i i -th shared expert and the i i -th routed expert, respectively;
K r K_{r} denotes the number of activated routed experts;
g i, t g_{i,t} is the gating value for the i i -th expert;
s i, t s_{i,t} is the token-to-expert affinity;
𝐞 i \mathbf{e}_{i} is the centroid vector of the i i -th routed expert;
and Topk ⁡ ( ⋅, K ) \operatorname{Topk}(\cdot,K) denotes the set comprising K K highest scores among the affinity scores calculated for the t t -th token and all routed experts.
Slightly different from DeepSeek-V2, DeepSeek-V3 uses the sigmoid function to compute the affinity scores, and applies a normalization among all selected affinity scores to produce the gating values.

Auxiliary-Loss-Free Load Balancing.

For MoE models, an unbalanced expert load will lead to routing collapse ( Shazeer et al. 2017 ) and diminish computational efficiency in scenarios with expert parallelism.
Conventional solutions usually rely on the auxiliary loss ( Fedus et al. 2021; Lepikhin et al. 2021 ) to avoid unbalanced load.
However, too large an auxiliary loss will impair the model performance ( Wang et al. 2024a ).
To achieve a better trade-off between load balance and model performance, we pioneer an auxiliary-loss-free load balancing strategy ( Wang et al. 2024a ) to ensure load balance.
To be specific, we introduce a bias term b i b_{i} for each expert and add it to the corresponding affinity scores s i, t s_{i,t} to determine the top-K routing:

g i, t ′ \displaystyle g^{\prime}_{i,t}

= { s i, t, s i, t + b i ∈ Topk ⁡ ( { s j, t + b j | 1 ⩽ j ⩽ N r }, K r ), 0, otherwise. \displaystyle=\begin{cases}s_{i,t},&s_{i,t}+b_{i}\in\operatorname{Topk}(\{s_{j,t}+b_{j}|1\leqslant j\leqslant N_{r}\},K_{r}),\\
0,&\text{otherwise}.\end{cases}

(16)

Note that the bias term is only used for routing.
The gating value, which will be multiplied with the FFN output, is still derived from the original affinity score s i, t s_{i,t}.
During training, we keep monitoring the expert load on the whole batch of each training step.
At the end of each step, we will decrease the bias term by γ \gamma if its corresponding expert is overloaded, and increase it by γ \gamma if its corresponding expert is underloaded, where γ \gamma is a hyper-parameter called bias update speed.
Through the dynamic adjustment, DeepSeek-V3 keeps balanced expert load during training, and achieves better performance than models that encourage load balance through pure auxiliary losses.

Complementary Sequence-Wise Auxiliary Loss.

Although DeepSeek-V3 mainly relies on the auxiliary-loss-free strategy for load balance, to prevent extreme imbalance within any single sequence, we also employ a complementary sequence-wise balance loss:

ℒ Bal \displaystyle\mathcal{L}_{\mathrm{Bal}}

= α ​ ∑ i = 1 N r f i ​ P i, \displaystyle=\alpha\sum_{i=1}^{N_{r}}{f_{i}P_{i}},

(17)

f i = N r K r ​ T ​ ∑ t = 1 T 𝟙 \displaystyle f_{i}=\frac{N_{r}}{K_{r}T}\sum_{t=1}^{T}\mathds{1}

( s i, t ∈ Topk ⁡ ( { s j, t | 1 ⩽ j ⩽ N r }, K r ) ), \displaystyle\left(s_{i,t}\in\operatorname{Topk}(\{s_{j,t}|1\leqslant j\leqslant N_{r}\},K_{r})\right),

(18)

s i, t ′ \displaystyle s^{\prime}_{i,t}

= s i, t ∑ j = 1 N r s j, t, \displaystyle=\frac{s_{i,t}}{\sum_{j=1}^{N_{r}}s_{j,t}},

(19)

P i \displaystyle P_{i}

= 1 T ​ ∑ t = 1 T s i, t ′, \displaystyle=\frac{1}{T}\sum_{t=1}^{T}{s^{\prime}_{i,t}},

(20)

where the balance factor α \alpha is a hyper-parameter, which will be assigned an extremely small value for DeepSeek-V3;
𝟙 ​ ( ⋅ ) \mathds{1}(\cdot) denotes the indicator function;
and T T denotes the number of tokens in a sequence.
The sequence-wise balance loss encourages the expert load on each sequence to be balanced.

Node-Limited Routing.

Like the device-limited routing used by DeepSeek-V2, DeepSeek-V3 also uses a restricted routing mechanism to limit communication costs during training.
In short, we ensure that each token will be sent to at most M M nodes, which are selected according to the sum of the highest K r M \frac{K_{r}}{M} affinity scores of the experts distributed on each node.
Under this constraint, our MoE training framework can nearly achieve full computation-communication overlap.

No Token-Dropping.

Due to the effective load balancing strategy, DeepSeek-V3 keeps a good load balance during its full training.
Therefore, DeepSeek-V3 does not drop any tokens during training.
In addition, we also implement specific deployment strategies to ensure inference load balance, so DeepSeek-V3 also does not drop tokens during inference.

Figure 3:
Illustration of our Multi-Token Prediction (MTP) implementation.
We keep the complete causal chain for the prediction of each token at each depth.

2.2 Multi-Token Prediction

Inspired by Gloeckle et al. 2024, we investigate and set a Multi-Token Prediction (MTP) objective for DeepSeek-V3, which extends the prediction scope to multiple future tokens at each position.
On the one hand, an MTP objective densifies the training signals and may improve data efficiency.
On the other hand, MTP may enable the model to pre-plan its representations for better prediction of future tokens.
Figure 3 illustrates our implementation of MTP.
Different from Gloeckle et al. 2024, which parallelly predicts D D additional tokens using independent output heads, we sequentially predict additional tokens and keep the complete causal chain at each prediction depth.
We introduce the details of our MTP implementation in this section.

MTP Modules.

To be specific, our MTP implementation uses D D sequential modules to predict D D additional tokens.
The k k -th MTP module consists of a shared embedding layer Emb ⁡ ( ⋅ ) \operatorname{Emb}(\cdot), a shared output head OutHead ⁡ ( ⋅ ) \operatorname{OutHead}(\cdot), a Transformer block TRM k ⁡ ( ⋅ ) \operatorname{TRM}_{k}(\cdot), and a projection matrix M k ∈ ℝ d × 2 ​ d M_{k}\in\mathbb{R}^{d\times 2d}.
For the i i -th input token t i t_{i}, at the k k -th prediction depth, we first combine the representation of the i i -th token at the ( k − 1 ) (k-1) -th depth 𝐡 i k − 1 ∈ ℝ d \mathbf{h}_{i}^{k-1}\in\mathbb{R}^{d} and the embedding of the ( i + k ) (i+k) -th token E ​ m ​ b ​ ( t i + k ) ∈ ℝ d Emb(t_{i+k})\in\mathbb{R}^{d} with the linear projection:

𝐡 i ′ k = M k ​ [ RMSNorm ⁡ ( 𝐡 i k − 1 ); RMSNorm ⁡ ( Emb ⁡ ( t i + k ) ) ], \mathbf{h}_{i}^{\prime k}=M_{k}[\operatorname{RMSNorm}(\mathbf{h}_{i}^{k-1});\operatorname{RMSNorm}(\operatorname{Emb}(t_{i+k}))],

(21)

where [ ⋅; ⋅ ] [\cdot;\cdot] denotes concatenation.
Especially, when k = 1 k=1, 𝐡 i k − 1 \mathbf{h}_{i}^{k-1} refers to the representation given by the main model.
Note that for each MTP module, its embedding layer is shared with the main model.
The combined 𝐡 i ′ k \mathbf{h}_{i}^{\prime k} serves as the input of the Transformer block at the k k -th depth to produce the output representation at the current depth 𝐡 i k \mathbf{h}_{i}^{k} :

𝐡 1 : T − k k = TRM k ( 𝐡 1 : T − k ′ k ), \mathbf{h}_{1:T-k}^{k}=\operatorname{TRM}_{k}(\mathbf{h}_{1:T-k}^{\prime k}),

(22)

where T T represents the input sequence length and i:j denotes the slicing operation (inclusive of both the left and right boundaries).
Finally, taking 𝐡 i k \mathbf{h}_{i}^{k} as the input, the shared output head will compute the probability distribution for the k k -th additional prediction token P i + 1 + k k ∈ ℝ V P_{i+1+k}^{k}\in\mathbb{R}^{V}, where V V is the vocabulary size:

P i + k + 1 k = OutHead ⁡ ( 𝐡 i k ). P_{i+k+1}^{k}=\operatorname{OutHead}(\mathbf{h}_{i}^{k}).

(23)

The output head OutHead ⁡ ( ⋅ ) \operatorname{OutHead}(\cdot) linearly maps the representation to logits and subsequently applies the Softmax ⁡ ( ⋅ ) \operatorname{Softmax}(\cdot) function to compute the prediction probabilities of the k k -th additional token.
Also, for each MTP module, its output head is shared with the main model.
Our principle of maintaining the causal chain of predictions is similar to that of EAGLE ( Li et al. 2024b ), but its primary objective is speculative decoding ( Xia et al. 2023; Leviathan et al. 2023 ), whereas we utilize MTP to improve training.

MTP Training Objective.

For each prediction depth, we compute a cross-entropy loss ℒ MTP k \mathcal{L}_{\text{MTP}}^{k} :

ℒ MTP k = CrossEntropy ( P 2 + k : T + 1 k, t 2 + k : T + 1 ) = − 1 T ∑ i = 2 + k T + 1 log P i k [ t i ], \mathcal{L}_{\text{MTP}}^{k}=\operatorname{CrossEntropy}(P_{2+k:T+1}^{k},t_{2+k:T+1})=-\frac{1}{T}\sum_{i=2+k}^{T+1}\log P_{i}^{k}[t_{i}],

(24)

where T T denotes the input sequence length, t i t_{i} denotes the ground-truth token at the i i -th position, and P i k ​ [ t i ] P_{i}^{k}[t_{i}] denotes the corresponding prediction probability of t i t_{i}, given by the k k -th MTP module.
Finally, we compute the average of the MTP losses across all depths and multiply it by a weighting factor λ \lambda to obtain the overall MTP loss ℒ MTP \mathcal{L}_{\text{MTP}}, which serves as an additional training objective for DeepSeek-V3:

ℒ MTP = λ D ​ ∑ k = 1 D ℒ MTP k. \mathcal{L}_{\text{MTP}}=\frac{\lambda}{D}\sum_{k=1}^{D}\mathcal{L}_{\text{MTP}}^{k}.

(25)

MTP in Inference.

Our MTP strategy mainly aims to improve the performance of the main model, so during inference, we can directly discard the MTP modules and the main model can function independently and normally.
Additionally, we can also repurpose these MTP modules for speculative decoding to further improve the generation latency.

3 Infrastructures

3.1 Compute Clusters

DeepSeek-V3 is trained on a cluster equipped with 2048 NVIDIA H800 GPUs.
Each node in the H800 cluster contains 8 GPUs connected by NVLink and NVSwitch within nodes.
Across different nodes, InfiniBand (IB) interconnects are utilized to facilitate communications.

3.2 Training Framework

The training of DeepSeek-V3 is supported by the HAI-LLM framework, an efficient and lightweight training framework crafted by our engineers from the ground up.
On the whole, DeepSeek-V3 applies 16-way Pipeline Parallelism (PP) ( Qi et al. 2023a ), 64-way Expert Parallelism (EP) ( Lepikhin et al. 2021 ) spanning 8 nodes, and ZeRO-1 Data Parallelism (DP) ( Rajbhandari et al. 2020 ).

In order to facilitate efficient training of DeepSeek-V3, we implement meticulous engineering optimizations.
Firstly, we design the DualPipe algorithm for efficient pipeline parallelism.
Compared with existing PP methods, DualPipe has fewer pipeline bubbles.
More importantly, it overlaps the computation and communication phases across forward and backward processes, thereby addressing the challenge of heavy communication overhead introduced by cross-node expert parallelism.
Secondly, we develop efficient cross-node all-to-all communication kernels to fully utilize IB and NVLink bandwidths and conserve Streaming Multiprocessors (SMs) dedicated to communication.
Finally, we meticulously optimize the memory footprint during training, thereby enabling us to train DeepSeek-V3 without using costly Tensor Parallelism (TP).

3.2.1 DualPipe and Computation-Communication Overlap

Figure 4:
Overlapping strategy for a pair of individual forward and backward chunks (the boundaries of the transformer blocks are not aligned).
Orange denotes forward, green denotes "backward for input", blue denotes "backward for weights", purple denotes PP communication, and red denotes barriers.
Both all-to-all and PP communication can be fully hidden.

For DeepSeek-V3, the communication overhead introduced by cross-node expert parallelism results in an inefficient computation-to-communication ratio of approximately 1:1.
To tackle this challenge, we design an innovative pipeline parallelism algorithm called DualPipe, which not only accelerates model training by effectively overlapping forward and backward computation-communication phases, but also reduces the pipeline bubbles.

The key idea of DualPipe is to overlap the computation and communication within a pair of individual forward and backward chunks.
To be specific, we divide each chunk into four components: attention, all-to-all dispatch, MLP, and all-to-all combine.
Specially, for a backward chunk, both attention and MLP are further split into two parts, backward for input and backward for weights, like in ZeroBubble ( Qi et al. 2023b ).
In addition, we have a PP communication component.
As illustrated in Figure 4, for a pair of forward and backward chunks, we rearrange these components and manually adjust the ratio of GPU SMs dedicated to communication versus computation.
In this overlapping strategy, we can ensure that both all-to-all and PP communication can be fully hidden during execution.
Given the efficient overlapping strategy, the full DualPipe scheduling is illustrated in Figure 5.
It employs a bidirectional pipeline scheduling, which feeds micro-batches from both ends of the pipeline simultaneously and a significant portion of communications can be fully overlapped.
This overlap also ensures that, as the model further scales up, as long as we maintain a constant computation-to-communication ratio, we can still employ fine-grained experts across nodes while achieving a near-zero all-to-all communication overhead.

Figure 5:
Example DualPipe scheduling for 8 PP ranks and 20 micro-batches in two directions.
The micro-batches in the reverse direction are symmetric to those in the forward direction, so we omit their batch ID for illustration simplicity.
Two cells enclosed by a shared black border have mutually overlapped computation and communication.

In addition, even in more general scenarios without a heavy communication burden, DualPipe still exhibits efficiency advantages.
In Table 2, we summarize the pipeline bubbles and memory usage across different PP methods.
As shown in the table, compared with ZB1P ( Qi et al. 2023b ) and 1F1B ( Harlap et al. 2018 ), DualPipe significantly reduces the pipeline bubbles while only increasing the peak activation memory by 1 P ​ P \frac{1}{PP} times.
Although DualPipe requires keeping two copies of the model parameters, this does not significantly increase the memory consumption since we use a large EP size during training.
Compared with Chimera ( Li and Hoefler 2021 ), DualPipe only requires that the pipeline stages and micro-batches be divisible by 2, without requiring micro-batches to be divisible by pipeline stages.
In addition, for DualPipe, neither the bubbles nor activation memory will increase as the number of micro-batches grows.

   Method

   Bubble

   Parameter

   Activation

  1F1B

   ( P ​ P − 1 ) ​ ( F + B ) (PP-1)(F+B)

   1 × 1\times

   P ​ P PP

  ZB1P

   ( P ​ P − 1 ) ​ ( F + B − 2 ​ W ) (PP-1)(F+B-2W)

   1 × 1\times

   P ​ P PP

  DualPipe (Ours)

   ( P ​ P 2 − 1 ) ​ ( F & B + B − 3 ​ W ) (\frac{PP}{2}-1)(F\&B+B-3W)

   2 × 2\times

   P ​ P + 1 PP+1

Table 2:
Comparison of pipeline bubbles and memory usage across different pipeline parallel methods. F F denotes the execution time of a forward chunk, B B denotes the execution time of a full backward chunk, W W denotes the execution time of a "backward for weights" chunk, and F & B F\&B denotes the execution time of two mutually overlapped forward and backward chunks.

3.2.2 Efficient Implementation of Cross-Node All-to-All Communication

In order to ensure sufficient computational performance for DualPipe, we customize efficient cross-node all-to-all communication kernels (including dispatching and combining) to conserve the number of SMs dedicated to communication.
The implementation of the kernels is co-designed with the MoE gating algorithm and the network topology of our cluster.
To be specific, in our cluster, cross-node GPUs are fully interconnected with IB, and intra-node communications are handled via NVLink.
NVLink offers a bandwidth of 160 GB/s, roughly 3.2 times that of IB (50 GB/s).
To effectively leverage the different bandwidths of IB and NVLink, we limit each token to be dispatched to at most 4 nodes, thereby reducing IB traffic.
For each token, when its routing decision is made, it will first be transmitted via IB to the GPUs with the same in-node index on its target nodes.
Once it reaches the target nodes, we will endeavor to ensure that it is instantaneously forwarded via NVLink to specific GPUs that host their target experts, without being blocked by subsequently arriving tokens.
In this way, communications via IB and NVLink are fully overlapped, and each token can efficiently select an average of 3.2 experts per node without incurring additional overhead from NVLink.
This implies that, although DeepSeek-V3 selects only 8 routed experts in practice, it can scale up this number to a maximum of 13 experts (4 nodes × \times 3.2 experts/node) while preserving the same communication cost.
Overall, under such a communication strategy, only 20 SMs are sufficient to fully utilize the bandwidths of IB and NVLink.

In detail, we employ the warp specialization technique ( Bauer et al. 2014 ) and partition 20 SMs into 10 communication channels.
During the dispatching process, (1) IB sending, (2) IB-to-NVLink forwarding, and (3) NVLink receiving are handled by respective warps.
The number of warps allocated to each communication task is dynamically adjusted according to the actual workload across all SMs.
Similarly, during the combining process, (1) NVLink sending, (2) NVLink-to-IB forwarding and accumulation, and (3) IB receiving and accumulation are also handled by dynamically adjusted warps.
In addition, both dispatching and combining kernels overlap with the computation stream, so we also consider their impact on other SM computation kernels.
Specifically, we employ customized PTX (Parallel Thread Execution) instructions and auto-tune the communication chunk size, which significantly reduces the use of the L2 cache and the interference to other SMs.

3.2.3 Extremely Memory Saving with Minimal Overhead

In order to reduce the memory footprint during training, we employ the following techniques.

Recomputation of RMSNorm and MLA Up-Projection.

We recompute all RMSNorm operations and MLA up-projections during back-propagation, thereby eliminating the need to persistently store their output activations.
With a minor overhead, this strategy significantly reduces memory requirements for storing activations.

Exponential Moving Average in CPU.

During training, we preserve the Exponential Moving Average (EMA) of the model parameters for early estimation of the model performance after learning rate decay.
The EMA parameters are stored in CPU memory and are updated asynchronously after each training step.
This method allows us to maintain EMA parameters without incurring additional memory or time overhead.

Shared Embedding and Output Head for Multi-Token Prediction.

With the DualPipe strategy, we deploy the shallowest layers (including the embedding layer) and deepest layers (including the output head) of the model on the same PP rank.
This arrangement enables the physical sharing of parameters and gradients, of the shared embedding and output head, between the MTP module and the main model.
This physical sharing mechanism further enhances our memory efficiency.

3.3 FP8 Training

Figure 6:
The overall mixed precision framework with FP8 data format. For clarification, only the Linear operator is illustrated.

Inspired by recent advances in low-precision training ( Peng et al. 2023b; Dettmers et al. 2022; Noune et al. 2022 ), we propose a fine-grained mixed precision framework utilizing the FP8 data format for training DeepSeek-V3.
While low-precision training holds great promise, it is often limited by the presence of outliers in activations, weights, and gradients ( Sun et al. 2024; He et al.; Fishman et al. 2024 ).
Although significant progress has been made in inference quantization ( Xiao et al. 2023; Frantar et al. 2022 ), there are relatively few studies demonstrating successful application of low-precision techniques in large-scale language model pre-training ( Fishman et al. 2024 ).
To address this challenge and effectively extend the dynamic range of the FP8 format, we introduce a fine-grained quantization strategy: tile-wise grouping with 1 × N c 1\times N_{c} elements or block-wise grouping with N c × N c N_{c}\times N_{c} elements.
The associated dequantization overhead is largely mitigated under our increased-precision accumulation process, a critical aspect for achieving accurate FP8 General Matrix Multiplication (GEMM).
Moreover, to further reduce memory and communication overhead in MoE training, we cache and dispatch activations in FP8, while storing low-precision optimizer states in BF16.
We validate the proposed FP8 mixed precision framework on two model scales similar to DeepSeek-V2-Lite and DeepSeek-V2, training for approximately 1 trillion tokens (see more details in Appendix B.1 ).
Notably, compared with the BF16 baseline, the relative loss error of our FP8-training model remains consistently below 0.25%, a level well within the acceptable range of training randomness.

3.3.1 Mixed Precision Framework

Building upon widely adopted techniques in low-precision training ( Kalamkar et al. 2019; Narang et al. 2017 ), we propose a mixed precision framework for FP8 training.
In this framework, most compute-density operations are conducted in FP8, while a few key operations are strategically maintained in their original data formats to balance training efficiency and numerical stability.
The overall framework is illustrated in Figure 6.

Firstly, in order to accelerate model training, the majority of core computation kernels, i.e., GEMM operations, are implemented in FP8 precision.
These GEMM operations accept FP8 tensors as inputs and produce outputs in BF16 or FP32.
As depicted in Figure 6, all three GEMMs associated with the Linear operator, namely Fprop (forward pass), Dgrad (activation backward pass), and Wgrad (weight backward pass), are executed in FP8.
This design theoretically doubles the computational speed compared with the original BF16 method.
Additionally, the FP8 Wgrad GEMM allows activations to be stored in FP8 for use in the backward pass.
This significantly reduces memory consumption.

Despite the efficiency advantage of the FP8 format, certain operators still require a higher precision due to their sensitivity to low-precision computations.
Besides, some low-cost operators can also utilize a higher precision with a negligible overhead to the overall training cost.
For this reason, after careful investigations, we maintain the original precision (e.g., BF16 or FP32) for the following components: the embedding module, the output head, MoE gating modules, normalization operators, and attention operators.
These targeted retentions of high precision ensure stable training dynamics for DeepSeek-V3.
To further guarantee numerical stability, we store the master weights, weight gradients, and optimizer states in higher precision.
While these high-precision components incur some memory overheads, their impact can be minimized through efficient sharding across multiple DP ranks in our distributed training system.

Figure 7:
(a) We propose a fine-grained quantization method to mitigate quantization errors caused by feature outliers; for illustration simplicity, only Fprop is illustrated.
(b) In conjunction with our quantization strategy, we improve the FP8 GEMM precision by promoting to CUDA Cores at an interval of N C = 128 N_{C}=128 elements MMA for the high-precision accumulation.

3.3.2 Improved Precision from Quantization and Multiplication

Based on our mixed precision FP8 framework, we introduce several strategies to enhance low-precision training accuracy, focusing on both the quantization method and the multiplication process.

Fine-Grained Quantization.

In low-precision training frameworks, overflows and underflows are common challenges due to the limited dynamic range of the FP8 format, which is constrained by its reduced exponent bits.
As a standard practice, the
