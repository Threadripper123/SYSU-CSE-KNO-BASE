# 论文·Titans：用神经长期记忆突破上下文长度限制

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　151 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2501.00663
抓取: https://ar5iv.labs.arxiv.org/html/2501.00663

Titans: Learning to Memorize at Test Time

Ali Behrouz

  
Peilin Zhong

  
Vahab Mirrokni

Affiliation: {alibehrouz, peilinz, mirrokni}@google.com

Abstract

Over more than a decade there has been an extensive research effort of how effectively utilize recurrent models and attentions. While recurrent models aim to compress the data into a fixed-size memory (called hidden state), attention allows attending to the entire context window, capturing the direct dependencies of all tokens. This more accurate modeling of dependencies, however, comes with a quadratic cost, limiting the model to a fixed-length context. We present a new neural long-term memory module that learns to memorize historical context and helps an attention to attend to the current context while utilizing long past information. We show that this neural memory has the advantage of a fast parallelizable training while maintaining a fast inference. From a memory perspective, we argue that attention due to its limited context but accurate dependency modeling performs as a short-term memory, while neural memory due to its ability to memorize the data, acts as a long-term, more persistent, memory. Based on these two modules, we introduce a new family of architectures, called Titans, and present three variants to address how one can effectively incorporate memory into this architecture. Our experimental results on language modeling, common-sense reasoning, genomics, and time series tasks show that Titans are more effective than Transformers and recent modern linear recurrent models. They further can effectively scale to larger than 2M context window size with higher accuracy in needle-in-haystack tasks compared to baselines.

1 Introduction

“The true art of memory is the art of attention!"

— Samuel Johnson, 1787

T ransformers, pure attention-based architectures ( Vaswani et al. 2017 ), have been firmly established as state-of-the-art models in sequence modeling, mainly due to their in-context learning and ability to learn at scale ( Kaplan et al. 2020 ). The primary building blocks of Transformers–attention modules—function as associative memory blocks ( Bietti et al. 2024 ), where they learn to store key-value associations and retrieve them by computing pairwise similarity between queries (i.e., search signals) and keys (i.e., contexts). Accordingly, by design, the output of a Transformer is exclusively conditioned on the direct dependencies of tokens in the current context window. This accurate modeling of dependencies, however, comes with quadratic time and memory complexity in terms of the context length. In complex real-world tasks (e.g., language modeling ( Liu et al. 2024a ), video understanding ( Wu et al. 2019 ), long-term time series forecasting ( Zhou et al. 2021 ) ), the context window can become extremely large, making the applicability of Transformers challenging in these downstream tasks.

To overcome the scalability issue of Transformers, recent studies aim to design different variants of linear Transformers ( Katharopoulos et al. 2020; Kacham et al. 2024; Yang et al. 2024b ), where softmax is replaced by a kernel function in the attention ( see § 2.1 for details ), resulting in a significant drop in memory consumption. Despite efficiency and the ability to scale to longer context, linear Transformers do not show competitive performance compared to Transformers as the kernel trick makes the model a linear recurrent network, in which the data is compressed into a matrix-valued states ( Katharopoulos et al. 2020 ). This, however, brings a contradictory fact about linear recurrent (or linear Transformers) models: On one hand, we use these linear models to enhance scalability and efficiency (linear vs. quadratic complexity), whose advantages is appeared for very long context; On the other hand, a very long context cannot be properly compressed in a small vector-valued or matrix-valued states ( Wang 2024 ).

Furthermore, beyond efficiency, most existing architectures–ranging from Hopfield Networks ( Hopfield 1982 ) to LSTMs ( Schmidhuber & Hochreiter 1997 ) and Transformers ( Vaswani et al. 2017 ) –face challenges when dealing with generalization, length extrapolation, and/or reasoning ( Anil et al. 2022; Qin et al. 2024 ), all of which are inseparable parts of many hard real-world tasks. Although these architectures draw inspiration from the human brain, each of which are missing: (1) a crucial component for learning process—such as short-term memory, long-term memory, meta-memory, attending to current context, etc. ( Cowan 2008 ); (2) how these components are interconnected systems that can operate independently; and/or (3) the ability to actively learn from data and memorize the abstraction of past history. We argue that in an effective learning paradigm, similar to human brain, there are distinct yet interconnected modules, each of which is responsible for a component crucial to the learning process.

Memory Perspective

Memory is a fundamental mental process and is an inseparable component of human learning ( Terry 2017 ). Without a properly functioning memory system, humans and animals would be restricted to basic reflexes and stereotyped behaviors. Accordingly, memory has been the inspiration for many seminal research in machine learning literature; e.g., Hopfield Networks ( Hopfield 1982 ), LSTMs ( Schmidhuber & Hochreiter 1997 ), and Transformers ( Vaswani et al. 2017 ).

Taking inspiration from the common definitions of memory and learning in neuropsychology literature ( Okano et al. 2000 ), most existing architectures consider memory as a neural update caused by an input, and define learning as a process for acquiring effective and useful memory, given an objective. In this perspective, Recurrent Neural Networks (RNNs) ( Williams & Zipser 1989 ) can be defined as models with a vector-valued memory module ℳ \mathcal{M} (also called hidden state) with two main steps: Given a new input x t x_{t} at time t t, the model (1) updates the memory using a function f ⁡ ( ℳ t − 1, x t ) f(\mathcal{M}_{t-1},x_{t}) (with compression); and (2) retrieves the corresponding memory of input using a function g ⁡ ( ℳ t, x t ) g(\mathcal{M}_{t},x_{t}) ( see § 2.1 for details ). Similarly, Transformers can be seen as architectures with a growing memory and two similar steps. That is, the pair of key and value matrices acts as the model’s memory, and the model: (1) updates the memory by appending the key and value to the memory (without compression), and (2) retrieves query vectors’ corresponding memory by finding the similarity of query and key vectors, which is then used to weight the value vectors for the output.

This perspective, can help us better understand existing paradigms, their critical differences, and design more effective architectures. For example, the main difference between Transformers ( Vaswani et al. 2017 ) and linear Transformers ( Katharopoulos et al. 2020 ) is the memory structure as well as the memory updating step, in which linear Transformers compress the historical data into a fixed-size matrix-valued memory while Transformers keep all historical data (within the context length) without any compression. While both linear Transformers and linear RNNs (including state space models) compress the information in memory update step, the critical difference lies in the structure of the memory, where linear RNNs (vs. linear Transformers) use a vector-valued memory (vs. matrix-valued memory). Therefore, this perspective motivates us to ask: (Q1) What constitute a good structure for the memory?
(Q2) What is a proper memory update mechanism? and
(Q3) What is a good memory retrieval process?

Revisiting our understanding of human memory, it is neither a unitary process nor it serves a single function ( Cowan 2008 ). In fact, memory is a confederation of systems–e.g., short-term, working, and long-term memory–each serving a different function with different neural structures, and each capable of operating independently ( Willingham 1997 ). This fact motivates us to ask: (Q4) How to design an efficient architecture that incorporates different interconnected memory modules. Finally, storing a memory is a neural process that requires to encode and store the abstraction of the past. It can be over-simplification to assume a single vector or a matrix, whose parameters are encoding the data in a linear manner, are enough for storing long-term history. (Q5) Is a deep memory module needed to effectively store/remember long past?

Contributions and Roadmap

In this paper, we aim to answer the above five questions by designing a long-term neural memory module, that can efficiently and effectively learn to memorize at test time. Building upon its design, we discuss how it can be incorporated into an architecture.

Neural Memory (§ 3 ).
We present a (deep) neural long-term memory that (as a meta in-context model) learns how to memorize/store the data into its parameters at test time. Inspired by human long-term memory system ( Mandler 2014 ), we design this memory module so an event that violates the expectations (being surprising) is more memorable. To this end, we measure the surprise of an input with the gradient of the neural network with respect to the input in associative memory loss ( see § 3.1 for details ). To better handle the limited memory, we present a decaying mechanism that consider the proportion of memory size and the amount of data surprise, resulting in better memory management. We show that this decay mechanism is in fact the generalization of forgetting mechanism in modern recurrent models ( Dao & Gu 2024; Yang et al. 2024a; Gu & Dao 2024 ). Interestingly, we find that this mechanism is equivalent to optimizing a meta neural network with mini-batch gradient descent, momentum, and weight decay. Building upon tensorizing mini-batch gradient descent to use more matmul operations ( Sun et al. 2024 ), we present a fast and parallelizable algorithm to train our deep neural long-term memory.

Titans Architectures (§ 4 ).
After designing the long-term neural memory, an important remaining question is how to effectively and efficiently incorporate memory into a deep learning architecture. We present Titans, a family of deep models that consists of three hyper-heads: (1) Core: this module consists of the short-term memory, and is responsible for the main flow of processing the data (we use attention with limited window size); (2) Long-term Memory: this branch is our neural long-term memory module that is responsible to store/remember long past; (3) Persistent Memory: this is a set of learnable but date-independent parameters that encodes the knowledge about a task. Finally, as a proof of concept, we present three variants of Titans, in which we incorporate memory as: (i) a context, (ii) a layer, and (iii) a gated branch.

Experimental Results (§ 5 ).
We perform experimental evaluations on language modeling, commonsense reasoning, recall-intensive, needle in haystack, time series forecasting, and DNA modeling tasks. We observe that our Titan architecture outperforms all modern recurrent models as well as their hybrid variants (combining with sliding-window attention) across a comprehensive set of benchmarks. Furthermore, Titans outperforms Transformers with the same context window, and show competitive performance with Transformers that use the entire context. This results are achieved while, contrary to Transformers, Titans scale to larger than 2M context window size.

2 Preliminaries

I n this section, we discuss the notation and some background concepts that we use though the paper. We let x ∈ ℝ N × d in x\in\mathbb{R}^{N\times d_{\text{in}}} be the input, ℳ \mathcal{M} be a neural network (neural memory module), 𝐐, 𝐊, 𝐕 \mathbf{Q},\mathbf{K},\mathbf{V} be the query, key and value of the attention mechanism, and 𝐌 \mathbf{M} be the attention mask. When segmenting the sequence, we use S ( i ) \texttt{S}^{(i)} to refer to the i i -th segment. Through the paper, we abuse the notation and use subscripts to refer to a specific element of a matrix, vector, or segments. For example, we let S j ( i ) \texttt{S}^{(i)}_{j} be the j j -th token in the i i -th segment. The only exception is subscripts with t t, which we reserved to index recurrence over time, or the state of a neural network at time t t. Given a neural network 𝒩 \mathcal{N} and a data sample x x, we use 𝒩 ⁡ ( x ) \mathcal{N}(x) (resp. 𝒩 ∗ ​ ( x ) \mathcal{N}^{*}(x) ) to refer to the forward pass with (resp. without) weight adjustment. Also, we abuse the notation and use 𝒩 ( k ) \mathcal{N}^{(k)} to refer to the k k -th layer of the neural network. In the following, we first, discuss the backgrounds for attention and its efficient variants followed by a review of modern linear RNNs. Finally, we discuss a memory perspective of these architectures that motivates us to design Titans.

2.1 Backgrounds

Attention.
Transformers ( Vaswani et al. 2017 ) as the de facto backbone for many deep learning models are based on attention mechanism. Given input x ∈ ℝ N × d in x\in\mathbb{R}^{N\times d_{\text{in}}}, causal attention computes output 𝐲 ∈ ℝ N × d in \mathbf{y}\in\mathbb{R}^{N\times d_{\text{in}}} based on softmax over input dependent key, value, and query matrices:

𝐐 = x ​ 𝐖 𝐐, 𝐊 = x ​ 𝐖 𝐊, 𝐕 = x ​ 𝐖 𝐕, \displaystyle\mathbf{Q}=x\mathbf{W}_{\mathbf{Q}},\qquad\mathbf{K}=x\mathbf{W}_{\mathbf{K}},\qquad\mathbf{V}=x\mathbf{W}_{\mathbf{V}},

(1)

𝐲 i = ∑ j = 1 i exp ⁡ ( 𝐐 i ⊤ ​ 𝐊 j / d in ) ​ 𝐕 j ∑ ℓ = 1 i exp ⁡ ( 𝐐 i ⊤ ​ 𝐊 ℓ / d in ), \displaystyle\mathbf{y}_{i}=\sum_{j=1}^{i}\frac{\exp\left(\mathbf{Q}_{i}^{\top}\mathbf{K}_{j}/\sqrt{d_{\text{in}}}\right)\mathbf{V}_{j}}{\sum_{\ell=1}^{i}\exp\left(\mathbf{Q}_{i}^{\top}\mathbf{K}_{\ell}/\sqrt{d_{\text{in}}}\right)},

(2)

where 𝐖 𝐐, 𝐖 𝐊, \mathbf{W}_{\mathbf{Q}},\mathbf{W}_{\mathbf{K}}, and 𝐖 𝐕 ∈ ℝ d in × d in \mathbf{W}_{\mathbf{V}}\in\mathbb{R}^{d_{\text{in}}\times d_{\text{in}}} are learnable parameters. Despite the power and effectiveness in recall, transformers need at least N × d N\times d operators to calculate the output, resulting in larger memory consumption and lower-throughput for longer sequences.

Efficient Attentions.
To improve the memory consumption and throughput of softmax attention for longer sequences, various studies focused on I/O aware implementations of attention ( Dao et al. 2022; Dao 2024 ), designing more efficient attention mechanisms by sparsifying the attention matrix ( Choromanski et al. 2021; Dai et al. 2019; Chen et al. 2021 ), approximating the softmax ( Arora et al. 2024 ), or developing kernel-based (linear) attentions ( Kacham et al. 2024; Schlag et al. 2021; Yang et al. 2024b; Aksenov et al. 2024 ). In this part, we focus on the later, i.e., linear attentions, where the softmax in standard attention is replaced with an alternative kernel function ϕ (.,. ) \phi(.,.), such that ϕ ⁡ ( x, y ) = ϕ ⁡ ( x ) ​ ϕ ​ ( y ) \phi(x,y)=\phi(x)\phi(y). Accordingly, the attention can be written as:

𝐲 i = ∑ j = 1 i ϕ ⁡ ( Q i ⊤ ​ K j ) ∑ ℓ = 1 i ϕ ⁡ ( Q i ⊤ ​ K ℓ ) ​ V j = ∑ j = 1 i ϕ ​ ( Q i ) ⊤ ​ ϕ ​ ( K j ) ∑ ℓ = 1 i ϕ ​ ( Q i ) ⊤ ​ ϕ ​ ( K ℓ ) ​ V j = ϕ ​ ( Q i ) ⊤ ​ ∑ j = 1 i ϕ ⁡ ( K j ) ​ V j ϕ ​ ( Q i ) ⊤ ​ ∑ ℓ = 1 i ϕ ⁡ ( K ℓ ), \displaystyle\mathbf{y}_{i}=\sum_{j=1}^{i}\frac{\phi(Q_{i}^{\top}K_{j})}{\sum_{\ell=1}^{i}\phi(Q_{i}^{\top}K_{\ell})}\>V_{j}=\sum_{j=1}^{i}\frac{\phi(Q_{i})^{\top}\phi(K_{j})}{\sum_{\ell=1}^{i}\phi(Q_{i})^{\top}\phi(K_{\ell})}\>V_{j}=\frac{\phi(Q_{i})^{\top}\sum_{j=1}^{i}\phi(K_{j})V_{j}}{\phi(Q_{i})^{\top}\sum_{\ell=1}^{i}\phi(K_{\ell})},

(3)

resulting in a higher-throughput as terms ∑ j = 1 i ϕ ⁡ ( K j ) \sum_{j=1}^{i}\phi(K_{j}) and ∑ ℓ = 1 i ϕ ⁡ ( K ℓ ) \sum_{\ell=1}^{i}\phi(K_{\ell}) are re-using in each step. When choosing the kernel as identity matrix ( Sun et al. 2023 ), the above formulation can also be written in a recurrent format:

ℳ t = ℳ t − 1 + K t ⊤ ​ V t, \displaystyle\mathcal{M}_{t}=\mathcal{M}_{t-1}+K_{t}^{\top}V_{t}\>,

(4)

𝐲 t = Q t ​ ℳ t, \displaystyle\mathbf{y}_{t}=Q_{t}\mathcal{M}_{t}\>,

(5)

which allows efficient inference for linear attentions.

Modern Linear Models and Their Memory Perspective.
As discussed earlier, one can define learning as a process for acquiring effective and useful memory. Building upon this, one can see the hidden state of Recurrent Neural Networks (RNNs) as a memory unit, which the model aims to compress the information into. Accordingly, in a general form of recurrent neural network, the hidden state can be treated as a memory unit and the recurrence process can be split into the read and write operations in the memory unit. That is, we let x ∈ ℝ N × d in x\in\mathbb{R}^{N\times d_{\text{in}}} be the input, ℳ ∈ ℝ d \mathcal{M}\in\mathbb{R}^{d} is the memory unit, and 𝐲 ∈ ℝ d in \mathbf{y}\in\mathbb{R}^{d_{\text{in}}} is the output, then the general form of the recurrent neural network is defined as:

ℳ t = f ⁡ ( ℳ t − 1, x t ), \displaystyle\qquad\qquad\qquad\qquad\mathcal{M}_{t}=f(\mathcal{M}_{t-1},x_{t}),\qquad\qquad

Write Operation

(6)

𝐲 t = g ⁡ ( ℳ t, x t ), \displaystyle\qquad\qquad\qquad\qquad\mathbf{y}_{t}=g(\mathcal{M}_{t},x_{t}),\qquad\qquad

Read Operation

(7)

where f (.,. ) f(.,.) is the read and g (.,. ) g(.,.) is the write corresponding functions. Note that here the subscript of ℳ t \mathcal{M}_{t} shows the state of the memory at time t t.

In this perspective, the recurrence formula of linear Transformers (see Equation 4 ) is equivalent to additively compress and write keys and values, ( K t, V t ) (K_{t},V_{t}), into a matrix-valued memory unit ℳ t \mathcal{M}_{t}. Therefore, when dealing with long context data, this additive nature of the process results in memory overflow, significantly damaging the performance of the model. To address this, studies have focused on two promising directions: (1) Adding forget mechanism: several studies have presented adaptive (data-dependent) forgetting gate mechanisms for linear models, where it can erase the memory when it is needed. As examples of such models, we refer to GLA ( Yang et al. 2024b ), LRU ( Orvieto et al. 2023 ), Griffin ( De et al. 2024 ), xLSTM ( Beck et al. 2024 ), and Mamba2 ( Dao & Gu 2024 ), which the later is also connected to the discretized version of traditional state space models ( Gu & Dao 2024 ).(2) Improving the write operation: To overcome the additive nature of memory write operation in traditional recurrent models, Widrow & Hoff 1988 presented Delta Rule, in which before adding a memory (i.e., a pair of key and value), the model first removes its past value. To enhance the parallelizable training and scaling, Yang et al. 2024c present a fast paralellizable algorithm. Finally, very recently, Yang et al. 2024a improved the DeltaNets by adding a forget gate.

Memory Modules. Memory has always been one of the core parts of the neural network designs ( Schmidhuber 1992; Schmidhuber & Hochreiter 1997; Graves et al. 2014; Zhang et al. 2024 ). The idea of seeing linear layers as the key-value (associative) memory system backs to fast weight programs, in which dynamic fast programs are incorporated into recurrent neural networks to serve as writable memory ( Schmidhuber 1992 ). The two learning rules of Hebbian ( Hebb 2005 ) and delta ( Prados & Kak 1989 ) are the most popular learning rules for fast weight programs, which have been extensively explored in various studies ( Munkhdalai & Yu 2017; Schmidhuber 1992; Munkhdalai et al. 2019; Schlag et al. 2021; Irie et al. 2021; Yang et al. 2024c; Yang et al. 2024a ). All these models, however, are based on momentary surprise, missing the token flow in the sequences (see Section 3.1 ), and most of them lacks a forgetting gate, resulting in a poor memory management.

We further discuss the connection of our architectures with recent models in Appendix C. Additional related work are discussed in Appendix A.

3 Learning to Memorize at Test Time

T o overcome the lack of long-term memory and to enable the model to learn, forget, and retrieve information, in this section, we present a neural long-term memory module, which is a meta models that learns to memorize at test time. In Section 3.1, we first discuss the motivation and the design of the neural memory. In Section 3.2, we discuss how our architecture design can benefit from a fast and parallelizable training. Finally, in Section 3.3, we augment our architecture using persistent memory module, in which we use learnable but data-independent parameters to learn meta information about the task.

3.1 Long-term Memory

To design a neural long-term memory module, we need a model that can encode the abstraction of the past history into its parameters. An example of this can be LLMs that are shown to be memorizing their training data ( Staab et al. 2024; Schwarzschild et al. 2024; Leybzon & Kervadec 2024 ). Therefore, a simple idea is to train a neural network and expect it to memorize its training data. Memorization, however, has almost always been known as an undesirable phenomena in neural networks as it limits the model generalization ( Bayat et al. 2024 ), causes privacy concerns ( Staab et al. 2024 ), and so results in poor performance at test time. Moreover, the memorization of the training data might not be helpful at test time, in which the data might be out-of-distribution. We argue that, we need an online meta-model that learns how to memorize/forget the data at test time. In this setup, the model is learning a function that is capable of memorization, but it is not overfitting to the training data, resulting in a better generalization at test time.

Learning Process and Surprise Metric.
The key idea to train a long-term memory is to treat its training as an online learning problem, in which we aim to compress the past information x 1, …, x t − 1 x_{1},\dots,x_{t-1} into the parameters of our long-term neural memory module ℳ t \mathcal{M}_{t}. As discussed earlier, an event that violates the expectations (i.e., is surprising) is more memorable for humans ( Mandler 2014 ). Inspired by this, a simple definition of surprise for a model can be its gradient with respect to the input. The larger the gradient is, the more different the input data is from the past data. Accordingly, using this surprise score, we can update the memory as:

ℳ t = ℳ t − 1 − θ t ​ ∇ ℓ ​ ( ℳ t − 1, x t ) ⏟ Surprise. \displaystyle\mathcal{M}_{t}=\mathcal{M}_{t-1}-\theta_{t}\>\underset{\text{Surprise}}{\underbrace{{\color[rgb]{0.1641,0.2891,0.4023}\nabla\ell(\mathcal{M}_{t-1};x_{t})}}}.

(8)

This surprise metric, however, can result in missing important information that comes after a big surprising moment. That is, the gradient can become extremely small after several surprising steps, leading to stocking in a flat area (i.e., local minima), and missing information about some parts of the sequence. From the human memory perspective, an event might not consistently surprise us through a long-period of time although it is memorable. The reason is that the initial moment is surprising enough to get our attention through a long time frame, leading to memorizing the entire time frame. To improve the above surprise metric ( Equation 8 ), we break the surprise metric into (1) past surprise, which measures the surprise amount of a very recent past; and (2) momentary surprise, which measures the surprise of incoming data:

ℳ t = ℳ t − 1 + S t, \displaystyle\mathcal{M}_{t}=\mathcal{M}_{t-1}+{\color[rgb]{0.1641,0.2891,0.4023}S_{t}},

(9)

S t = η t ​ S t − 1 ⏟ Past Surprise − θ t ​ ∇ ℓ ​ ( M t − 1, x t ) ⏟ Momentary Surprise. \displaystyle S_{t}=\eta_{t}\underset{\text{Past Surprise}}{\underbrace{{\color[rgb]{0.1641,0.2891,0.4023}S_{t-1}}}}-\theta_{t}\>\underset{\text{Momentary Surprise}}{\underbrace{{\color[rgb]{0.1641,0.2891,0.4023}\nabla\ell\left(M_{t-1};x_{t}\right)}}}.

(10)

Interestingly, this formulation is similar to gradient descent with momentum, where S t S_{t} is the momentum element. Therefore, the momentum here act as a memory of surprise across time (sequence length). In this formulation, the term η t \eta_{t} is a data-dependent surprise decay (a function of x t x_{t} ), controlling how surprise decays over time, and the term θ t \theta_{t} is controlling how much of momentary surprise should be incorporated into the final surprise metric in a data-dependent manner. This data-dependency is particularly important in this design: While surprise of previous tokens might be needed to affect the surprise of the next token, it is mostly valid if all tokens are relevant and are in the same context. Accordingly, a data-dependent η \eta can control if memory needs to: (1) ignore the last surprise by setting η t → 0 \eta_{t}\rightarrow 0 (possibly due to the change of context), or (2) fully incorporate the last surprise by setting η t → 1 \eta_{t}\rightarrow 1 (possibly as the token is highly relevant to its recent past tokens).

Objective.
Our above surprise metric is based on a loss function ℓ (.;. ) \ell(.;.), which is the objective that our memory is learning to act as it at test time. That is, our memory module is a meta model that learns a function based on the loss function ℓ (.;. ) \ell(.;.). In this work, we focus on associative memory, in which we aim to store the past data as the pairs of keys and values. Given x t x_{t}, similar to Transformers ( Vaswani et al. 2017 ), we use two linear layers to project x t x_{t} into a key and value:

𝐤 t = x t ​ W K, 𝐯 t = x t ​ W V, \displaystyle\mathbf{k}_{t}=x_{t}W_{K},\qquad\qquad\mathbf{v}_{t}=x_{t}W_{V},

(11)

where W K W_{K} and W V ∈ ℝ d in × d in W_{V}\in\mathbb{R}^{d_{\text{in}}\times d_{\text{in}}}. Next, we expect our memory module to learn the associations between keys and values. To this end, we define the loss as follows:

ℓ ⁡ ( ℳ t − 1, x t ) = ‖ ℳ t − 1 ​ ( 𝐤 t ) − 𝐯 t ‖ 2 2 \displaystyle\ell(\mathcal{M}_{t-1};x_{t})=\left\|\mathcal{M}_{t-1}\left(\mathbf{k}_{t}\right)-\mathbf{v}_{t}\right\|_{2}^{2}

(12)

By optimizing the above loss function in the inner-loop of our meta model (memory), the model learns how to memorize the mapping between keys and values at test time. Note that, similar to meta-learning models ( Zintgraf et al. 2019; Nichol 2018 ), training of the memory is in the inner-loop, and so parameters W K W_{K} and W V W_{V} are hyperparameters in the above loss function. Accordingly, in the inner loop, we optimize ℳ \mathcal{M} ’s weights, while in the outer-loop, we optimize other parameters of the entire architecture.

Forgetting Mechanism.
When dealing with very large sequences (e.g., millions of tokens), it is crucial to manage which past information should be forgotten–even with a deep or a very large matrix-valued memory. To this end, we use an adaptive forgetting mechanism that allows the memory to forget the information that is not needed anymore, resulting in better managing the memory’s limited capacity. That is, given the next token x t x_{t}, we modify the update rule as:

ℳ t = ( 1 − α t ) ​ ℳ t − 1 + S t, \displaystyle\mathcal{M}_{t}=(1-\alpha_{t})\mathcal{M}_{t-1}+{\color[rgb]{0.1641,0.2891,0.4023}S_{t}},

(13)

S t = η t S t − 1 − θ t ∇ ℓ ( M t − 1; x t ), \displaystyle S_{t}=\eta_{t}{{\color[rgb]{0.1641,0.2891,0.4023}S_{t-1}}}-\theta_{t}\>{{\color[rgb]{0.1641,0.2891,0.4023}\nabla\ell\left(M_{t-1};x_{t}\right)}},

(14)

where α t ∈ [ 0, 1 ] \alpha_{t}\in[0,1] is the gating mechanism that flexibly controls the memory; i.e., decides how much information should be forgotten. For example, it can update the memory without affecting the past abstraction by letting α t → 0 \alpha_{t}\rightarrow 0, and can clear the entire memory by letting α t → 1 \alpha_{t}\rightarrow 1. Later in this section, we show that this weight decay mechanism is closely related to the gating mechanism in modern RNNs ( Dao & Gu 2024; Orvieto et al. 2023 ).

Memory Architecture.
In this paper, we focus on simple MLPs with L ℳ ≥ 1 L_{\mathcal{M}}\geq 1 layers as the architecture of our long-term memory. The main reason behind this choice is that we want to focus on better motivating the design of the long-term memory and ways that it can be incorporated into an architecture. However, our formulation and architectural design opens a new research direction to design neural architectures that are more effective and efficient in memorization of data. Recently, there has been a promising line of work to design such architectures ( Zhang et al. 2024; Cetin et al. 2024; Berges et al. 2024 ), which incorporating them into our framework (i.e., replacing simple MLPs with such architectures) can be an interesting future work.

When using vector-valued or matrix-valued memory ( Yang et al. 2024b; Orvieto et al. 2023; De et al. 2024 ), the memory module is compressing the past data and fit it into a line. That is, from the meta learning or online learning perspective ( Sun et al. 2024 ), using a matrix-valued memory ℳ = W ∈ ℝ d in × d in \mathcal{M}=W\in\mathbb{R}^{d_{\text{in}}\times d_{\text{in}}} is equivalent to optimize ℓ ⁡ ( W t − 1, x t ) = ‖ W t − 1 ​ 𝐤 t − 𝐯 t ‖ 2 2 \ell(W_{t-1};x_{t})=\left\|W_{t-1}\mathbf{k}_{t}-\mathbf{v}_{t}\right\|_{2}^{2}, which is an online linear regression objective and so the optimal solution assumes the underlying dependency of historical data is linear. On the other hand, we argue that deep memory modules (i.e., L ℳ ≥ 2 L_{\mathcal{M}}\geq 2 ). Aligning with the theoretical results that MLPs with at least two layers are strictly more expressive than linear models ( Hornik et al. 1989 ), in Section 5.5, we show that deep memory modules are more effective in practice.

Retrieving a Memory.
In the above, we discuss how one can design and train a long-term memory module that learns to memorize at test time. A key remaining question is: How one can retrieve information from the memory? We simply use the forward pass without weight update (i.e., inference) to retrieve a memory correspond to a query. Formally, given an input x t x_{t}, we use a linear layer W Q W_{Q} to project the input, i.e., 𝐪 t = x t ​ W Q \mathbf{q}_{t}=x_{t}W_{Q} and retrieve the corresponding (or useful) information from the memory y t {y}_{t} by:

y t = ℳ ∗ ​ ( 𝐪 t ). \displaystyle y_{t}=\mathcal{M}^{*}(\mathbf{q}_{t}).

(15)

Figure 1 : The illustration of how the training of neural memory can be done in parallel and using matmul s.

3.2 How to Parallelize the Long-term Memory Training

As discussed above, the design of our long-term memory module is equivalent to training a meta model by optimizing associative memory loss function ℓ ⁡ ( ℳ t − 1, x t ) = ‖ ℳ t − 1 ​ ( 𝐤 t ) − 𝐯 t ‖ 2 2 \ell(\mathcal{M}_{t-1};x_{t})=\left\|\mathcal{M}_{t-1}\left(\mathbf{k}_{t}\right)-\mathbf{v}_{t}\right\|_{2}^{2} using gradient descent with momentum and weight decay. Therefore, in theory, the training of long-term memory module requires 𝒪 ⁡ ( N ) \mathcal{O}\left(N\right) FLOPs, where N N is the sequence length. However, in practice, we need to parallelize the training process and to fully take advantage of hardware accelerators (e.g., TPUs, GPUs), we need to tensorize the process and use more matmul s.

Next, we show that calculating the weights in the inner loop with mini-batch gradient descent, data-dependent learning rate, and weight decay can be reformulated so that it uses only matmul s and sum. We build upon the work of Sun et al. 2024 that shows forward pass of a model optimizing with the mini-batch gradient descent (with constant learning rate) can be calculated using matmul s. We can split the sequence into chunks of size b ≥ 1 b\geq 1, and write the mini-batch gradient descent as:

ℳ t = ( 1 − α t ) ℳ t − 1 − θ t ∇ ℓ ( ℳ t − 1; x t ) = β t ℳ 0 − ∑ i = 1 t θ i β t β i ∇ ℓ ( ℳ t ′; x i ), \displaystyle\mathcal{M}_{t}=(1-\alpha_{t})\mathcal{M}_{t-1}-\theta_{t}\nabla\ell(\mathcal{M}_{t-1};x_{t})=\beta_{t}\mathcal{M}_{0}-\sum_{i=1}^{t}\theta_{i}\frac{\beta_{t}}{\beta_{i}}\nabla\ell(\mathcal{M}_{t^{\prime}};x_{i}),

(16)

where t ′ = t − mod ​ ( t, b ) t^{\prime}=t-\texttt{mod}(t,b), and β i = ∏ j = 1 i ( 1 − α j ) \beta_{i}=\prod_{j=1}^{i}(1-\alpha_{j}). For the sake of simplicity, we focus on the first chunk, i.e., t = b t=b and so t ′ = 0 t^{\prime}=0. Also, we explain the process for the case that ℳ t = W t \mathcal{M}_{t}=W_{t} is linear. The process for MLPs with N p ≥ 2 N_{p}\geq 2 is similar. Using our loss function, we have:

∇ ℓ ( W 0; x t ) = ( W 0 x t − x t ) x t ⊤ ⇒ ∑ i = 1 b θ i β b β i ∇ ℓ ( W 0; x i ) = Θ b 𝐁 b ( W 0 X − X ) X ⊤, \displaystyle\nabla\ell(W_{0};x_{t})=(W_{0}x_{t}-x_{t})x_{t}^{\top}\Rightarrow\sum_{i=1}^{b}\theta_{i}\frac{\beta_{b}}{\beta_{i}}\nabla\ell(W_{0};x_{i})=\Theta_{b}\mathbf{B}_{b}(W_{0}X-X)X^{\top},

(17)

where Θ b = diag ​ ( [ θ 1 θ 2 … θ b ] ) \Theta_{b}=\texttt{diag}\left(\begin{bmatrix}\theta_{1}&\theta_{2}&\dots&\theta_{b}\end{bmatrix}\right) and 𝐁 b \mathbf{B}_{b} is defined analogously on β b β i \frac{\beta_{b}}{\beta_{i}} s. Note that, we do not need to store all Θ k ​ b \Theta_{kb} and 𝐁 k ​ b \mathbf{B}_{kb} for k = 1, …, N / b k=1,\dots,N/b, instead, we store these matrices for each chunk, resulting in using less memory. Next, we extend this representation so we can also incorporate the momentum term. In a chunk wise gradient descent with momentum, if we look at the momentum term, we have:

S t = η t ​ S t − 1 − θ t ​ u t, \displaystyle S_{t}=\eta_{t}{S_{t-1}}-\theta_{t}\>u_{t},

(18)

where u t = ∇ ℓ ​ ( M t ′, x t ) u_{t}=\nabla\ell\left(M_{t^{\prime}};x_{t}\right). Note that, we can compute all u t u_{t} at the same time, and so Equation 18 is a linear recurrence with u t u_{t} as an input, S t S_{t} as the hidden state, and η t \eta_{t} as input-dependent transition value. Accordingly, we can use parallel associative scan ( Smith et al. 2023 ) to calculate S t S_{t} s in this chunk.

Parameters as the Function of Chunks.
Instead of making parameters like α t, θ t \alpha_{t},\theta_{t}, and η t \eta_{t} input-dependent (i.e., a function of token x t x_{t} ), we can make them functions of their chunk. Despite losing expressive power, this formulation can help to make the training even faster. In this case, we are using the same value for each of α \alpha, θ \theta, and η \eta in each chunk. Accordingly, in Equation 17, we can store Θ \Theta using a single scaler. Similarly we can make Equation 18 faster. That is, when η \eta and θ \theta are learnable but time-invariant inside each chunk, this equation becomes a linear time-invariant system (LTI), which can be computed by a global convolution ( Gu et al. 2022 ). In our experiments, we make these parameters as the functions of tokens. However, such simplifications (i.e., as the function of chunks) can be the interest of future work to training larger models in more efficient manner.

3.3 Persistent Memory

Our long-term memory can also be seen as a contextual memory, meaning that the output is fully depend on the context. Therefore, in addition to our long-term memory, we also use a set of learnable but input-independent parameters to act as task-related memory. This type of memory has been referred to as persistent or meta-memory in the literature ( Sukhbaatar et al. 2019; Dong et al. 2024a ). Given N p ≥ 1 N_{p}\geq 1, we use learnable parameters P = [ p 1 p 2 … p N p ] P=\begin{bmatrix}p_{1}&p_{2}&\dots&p_{N_{p}}\end{bmatrix} and append it to the start of our sequence: i.e., given a context window size of N N, we modify the input as:

x new = [ p 1 p 2 … p N p ] | | x, \displaystyle x_{\text{new}}=\begin{bmatrix}p_{1}&p_{2}&\dots&p_{N_{p}}\end{bmatrix}||\>\>x,

(19)

where | ⁣ | || is concatenation. Next, we discuss the motivation of persistent memory from three perspective:

Memory Perspective.
As discussed earlier, our neural long-term memory is a contextual memory, in which all parameters are input-dependent. An effective memory system, however, also needs input-independent parameters to store the abstraction of the task knowledge. That is, mastering a task requires the memorization of the knowledge that how the task can be done, and these parameters are responsible for storing such knowledge.

Feedforward Network Perspective.
In the Transformer architectures, there are fully connected layers after the attention module, which are shown to be similar to attention weights but with data-independent parameters. That is, Sukhbaatar et al. 2019 showed that replacing the ReLU in fully connected layers with Softmax can results in an attention-like weights, in which weights are data-independent:

F ​ F ​ N ​ ( x ) = W V ​ Softmax ​ ( W K ​ x ). \displaystyle FFN(x)=W_{V}\>\texttt{Softmax}\left(W_{K}x\right).

(20)

In fact, W K W_{K} and W V W_{V} are acting similar to K K and V V matrices in attention module when they are input-independent. The persistent memory weights are expected to have the same functionality, meaning that using them in the first part of the sequence leads to having input-independent attention weights ( Sukhbaatar et al. 2019 ).

Technical Perspective.
Attention with causal mask has implicit bias toward initial tokens in the sequence, and so attention weights are almost always highly active for initial tokens, resulting in performance damage. From the technical perspective, these learnable parameters at the start of the sequence can mitigate such effect by redistributing the attention weights more effectively ( Xiao et al. 2024; Han et al. 2024 ).

Figure 2 : Memory as a Context (MAC) Architecture. This architecture includes three branches of (1) core, (2) contextual (long-term) memory, and (3) persistent memory. The core branch concatenates the corresponding long-term and persistent memories with the input sequence. Next, attention performs on the sequence and decides what part of the information should store in the long-term memory. At the test time, parameters corresponds to contextual memory are still learning, parameters corresponds to the core branch are responsible for in-context learning, and parameters of persistent memory are responsible to store the knowledge about tasks and so are fixed.

4 How to Incorporate Memory?

A n important question that remained unanswered is: How one can effectively and efficiently incorporate the designed neural memory into a deep learning architecture? As discussed earlier, from a memory perspective, the pair of K and V matrices in transformers can be interpreted as an associative memory block. Due to their accurate modeling of dependencies and so their limited context window, we interpret them as short-term memory modules, attending to the current context window size. On the other hand, our neural memory with the ability to continuously learn from data and store it in its weights can play the role of a a long-term memory. In this section, we aim to answer the above question by proposing three different variants of Titans. Later in our experiments, we show that each of these variants has its own advantages/disadvantages and also can show a trade-off between the efficiency and effectiveness in very long-contexts.

4.1 Memory as a Context

In the first architecture design (see Figure 2 ), we treat the memory as a context to the current information. That is, given a long sequence x ∈ ℝ N × d in x\in\mathbb{R}^{N\times d_{\text{in}}}, we first chunk the sequence into fixed-size segments S ( i ) \texttt{S}^{(i)} for i = 1, …, N / C i=1,\dots,N/C. Given the incoming segment S ( t ) \texttt{S}^{(t)}, we consider it as the current context and its past segment as the historical information. Therefore, let ℳ t − 1 \mathcal{M}_{t-1} be the state of long-term memory before segment S ( t ) \texttt{S}^{(t)}, we use the input context as the query to the memory ℳ t − 1 \mathcal{M}_{t-1} to retrieve the corresponding information from the long-term memory. That is, we retrieve the past information that corresponds to S ( t ) \texttt{S}^{(t)} as:

h t = ℳ t − 1 ∗ ​ ( 𝐪 t ), \displaystyle h_{t}=\mathcal{M}^{*}_{t-1}(\mathbf{q}_{t}),

(21)

where 𝐪 t = S ( t ) ​ W Q \mathbf{q}_{t}=\texttt{S}^{(t)}W_{Q}. Next, we use this historical information along with our persistent memory parameters as the input sequence to the attention module:

S ~ ( t ) = [ p 1 p 2 … p N p ] ​ ‖ h t ‖ ​ S ( t ), \displaystyle\tilde{\texttt{S}}^{(t)}=\begin{bmatrix}p_{1}&p_{2}&\dots&p_{N_{p}}\end{bmatrix}\>\>||\>\>h_{t}\>\>||\>\>\texttt{S}^{(t)},

(22)

y t = Attn ​ ( S ~ ( t ) ). \displaystyle y_{t}=\texttt{Attn}\left(\tilde{\texttt{S}}^{(t)}\right).

(23)

The structure of the attention map over the entire sequence is shown in 3(a). We then use y t y_{t} to update the long-term memory module for the next segment and the final output:

ℳ t = ℳ t − 1 ​ ( y t ), \displaystyle\mathcal{M}_{t}=\mathcal{M}_{t-1}\left(y_{t}\right),

(24)

o t = y t ⊗ ℳ t ∗ ​ ( y t ). \displaystyle o_{t}=y_{t}\otimes\mathcal{M}_{t}^{*}\left(y_{t}\right).

(25)

Note that, in the above, we are updating the weight of ℳ t − 1 \mathcal{M}_{t-1} through forward pass.

(a) Memory as a Context (MAC). We segment the sequence and use full causal attention in each window. Again, the first N p N_{p} tokens are persistent memory and the next N l N_{l} are long-term memory tokens

(b) Memory as Gating (MAG). We use sliding window attention (SWA) as a short-term memory and our neural memory module as a long-term memory, combining by a gating.

Figure 3 : Attention masks for different variants of Titans.

This architecture has two key advantages: (1) Attention by having both historical and current context, has the ability to decides whether given the current data, the long-term memory information is needed. (2) The attention module helps the long-term memory to store only useful information from the current context. That is, not all tokens in each segment are useful and memorizing all of them can result in memory overflow. Therefore, attention is helping the memory to understand which information is useful, better managing the memory capacity. (3) At test time: (i) persistent memory parameters are fixed as they encodes the knowledge about the task, which should not be changed; (ii) the attention module weights are in-context learner; and (iii) the long-term memory module is still learning (memorizing) the information at test time. That is, we update the weights of the neural memory even at test time as weights are encoding the abstraction of long past.

Figure 4 : Memory as a Gate (MAG) Architecture. This architecture, similarly, has the three branches of (1) core, (2) contextual memory, and (3) persistent memory. It, however, incorporates only persistent memory into the context and combine memory with the core branch using a gating mechanism. At test time, the behavior is the same as Figure 2.

4.2 Gated Memory

In the next variant (see Figure 4 ), in one branch, we directly use the input data to update the long-term memory, and in the second branch, we use a sliding window attention (SWA):

x ~ = [ p 1 p 2 … p N p ] | | x, \displaystyle\tilde{x}=\begin{bmatrix}p_{1}&p_{2}&\dots&p_{N_{p}}\end{bmatrix}\>\>||\>\>x,

(26)

y = SW-Attn ∗ ​ ( x ~ ), \displaystyle y=\texttt{SW-Attn}^{*}\left(\tilde{x}\right),

(27)

o = y ⊗ ℳ ⁡ ( x ~ ), \displaystyle o=y\otimes\mathcal{M}(\tilde{x}),

(28)

where SW-Attn ∗ \texttt{SW-Attn}^{*} is sliding window attention with prefix (see 3(b) ). Note that, contrary to the previous design, we are not segmenting the input data. Also, we abuse the notation and use ℳ ⁡ ( x ) \mathcal{M}(x) to refer to the final output of the memory after all recursion over the tokens of the sequence. In the above equation, ⊗ \otimes can be any non-linear gating. In our experiments, we normalize the outputs y y and ℳ ⁡ ( x ~ ) \mathcal{M}(\tilde{x}) using learnable vector-valued weights, followed by a non-linearity σ (. ) \sigma(.).

The overall attention mask of this design is shown in 3(b). In this design, sliding window attention is act as a precise short-term memory, while the neural memory module is acting as a fading memory for the model. This architecture design can also be seen as a multi-head architecture where the structure of heads are different ( Dong et al. 2024a ).

4.3 Memory as a Layer

The last variant uses the neural Memory As a Layer (MAL) of a deep neural network (see Figure 5 ). This architecture design is more common in the literature, where the hybrid models stack recurrent models with full or sliding window attentions. Given input x x, we have:

x ~ = [ p 1 p 2 … p N p ] | | x, \displaystyle\tilde{x}=\begin{bmatrix}p_{1}&p_{2}&\dots&p_{N_{p}}\end{bmatrix}\>\>||\>\>x,

(29)

y = ℳ ⁡ ( x ~ ), \displaystyle y=\mathcal{M}(\tilde{x}),

(30)

o = SW-Attn ​ ( y ), \displaystyle o=\texttt{SW-Attn}\left(y\right),

(31)

where SW-Attn is sliding window attention. The main drawback of this design is that the power of th
