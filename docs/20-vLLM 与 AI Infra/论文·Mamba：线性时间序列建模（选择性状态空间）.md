# 论文·Mamba：线性时间序列建模（选择性状态空间）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　273 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2312.00752
抓取: https://ar5iv.labs.arxiv.org/html/2312.00752

Mamba: Linear-Time Sequence Modeling with Selective State Spaces

Albert Gu

Thanks: Alphabetical by first name.

  
Tri Dao † † footnotemark:

Affiliation: agu@cs.cmu.edu, tri@tridao.me

Abstract

Foundation models, now powering most of the exciting applications in deep learning, are almost universally based on the Transformer architecture and its core attention module.
Many subquadratic-time architectures such as linear attention, gated convolution and recurrent models, and structured state space models (SSMs) have been developed to address Transformers’ computational inefficiency on long sequences, but they have not performed as well as attention on important modalities such as language.
We identify that a key weakness of such models is their inability to perform content-based reasoning, and make several improvements.
First, simply letting the SSM parameters be functions of the input addresses their weakness with discrete modalities, allowing the model to selectively propagate or forget information along the sequence length dimension depending on the current token.
Second, even though this change prevents the use of efficient convolutions, we design a hardware-aware parallel algorithm in recurrent mode.
We integrate these selective SSMs into a simplified end-to-end neural network architecture without attention or even MLP blocks ( Mamba ).
Mamba enjoys fast inference (5 × \times higher throughput than Transformers) and linear scaling in sequence length, and its performance improves on real data up to million-length sequences.
As a general sequence model backbone, Mamba achieves state-of-the-art performance across several modalities such as language, audio, and genomics. On language modeling, our Mamba-3B model outperforms Transformers of the same size and matches Transformers twice its size, both in pretraining and downstream evaluation.

1 Introduction

Foundation models (FMs), or large models pretrained on massive data then adapted for downstream tasks, have emerged as an effective paradigm in modern machine learning.
The backbone of these FMs are often
sequence models, operating on arbitrary sequences of inputs from a wide variety of domains such as language, images, speech, audio, time series, and genomics
( Sutskever et al. 2014; Dosovitskiy et al. 2020; Oord et al. 2016; Brown et al. 2020; Ismail et al. 2019; Poli et al. 2023 ).
While this concept is agnostic to a particular choice of model architecture,
modern FMs are predominantly based on a single type of sequence model: the Transformer ( Vaswani et al. 2017 ) and its core attention layer ( Bahdanau et al. 2015 )
The efficacy of self-attention is attributed to its ability to route information densely within a context window, allowing it to model complex data.
However, this property brings fundamental drawbacks: an inability to model anything outside of a finite window,
and quadratic scaling with respect to the window length.
An enormous body of research has appeared on more efficient variants of attention to overcome these drawbacks ( Tay et al. 2022 ),
but often at the expense of the very properties that makes it effective.
As of yet, none of these variants have been shown to be empirically effective at scale across domains.

Recently, structured state space sequence models (SSMs) ( Gu et al. 2021; Gu et al. 2022 ) have emerged as a promising class of architectures for sequence modeling.
These models can be interpreted as a combination of recurrent neural networks (RNNs) and convolutional neural networks (CNNs), with inspiration from classical state space models ( Kalman 1960 ).
This class of models can be computed very efficiently as either a recurrence or convolution, with linear or near-linear scaling in sequence length.
Additionally, they have principled mechanisms for modeling long-range dependencies ( Gu et al. 2020 ) in certain data modalities, and have dominated benchmarks such as the Long Range Arena ( Tay et al. 2021 ).
Many flavors of SSMs ( Gu et al. 2022; Gupta et al. 2022; Gu et al. 2022a; Li et al. 2023; Ma et al. 2023; Smith et al. 2023; Orvieto et al. 2023 )
have been successful in domains involving continuous signal data such as audio and vision ( Goel et al. 2022; Saon et al. 2023; Nguyen et al. 2022 ).
However, they have been less effective at modeling discrete and information-dense data such as text.

We propose a new class of selective state space models,
that improves on prior work on several axes to achieve the modeling power of Transformers while scaling linearly in sequence length.

Selection Mechanism.

First, we identify a key limitation of prior models: the ability to efficiently select data in an input-dependent manner (i.e. focus on or ignore particular inputs).
Building on intuition based on important synthetic tasks such as selective copy and induction heads, we design a simple selection mechanism by parameterizing the SSM parameters based on the input.
This allows the model to filter out irrelevant information and remember relevant information indefinitely.

Hardware-aware Algorithm.

This simple change poses a technical challenge for the computation of the model; in fact, all prior SSMs models must be time- and input-invariant in order to be computationally efficient.
We overcome this with a hardware-aware algorithm that computes the model recurrently with a scan instead of convolution, but does not materialize the expanded state in order to avoid IO access between different levels of the GPU memory hierarchy.
The resulting implementation is faster than previous methods both in theory (scaling linearly in sequence length, compared to pseudo-linear for all convolution-based SSMs) and on modern hardware (up to 3 × \times faster on A100 GPUs).

Architecture.

We simplify prior deep sequence model architectures by combining the design of prior SSM architectures ( Dao et al. 2023 ) with the MLP block of Transformers into a single block, leading to a simple and homogenous architecture design ( Mamba ) incorporating selective state spaces.

Selective SSMs, and by extension the Mamba architecture, are fully recurrent models with key properties that make them suitable as the backbone of general foundation models operating on sequences.

(i) High quality: selectivity brings strong performance on dense modalities such as language and genomics.

(ii) Fast training and inference: computation and memory scales linearly in sequence length during training, and unrolling the model autoregressively during inference requires only constant time per step since it does not require a cache of previous elements.

(iii) Long context: the quality and efficiency together yield performance improvements on real data up to sequence length 1M.

We empirically validate Mamba’s potential as a general sequence FM backbone, in both pretraining quality and domain-specific task performance, on several types of modalities and settings:

•

Synthetics. On important synthetic tasks such as copying and induction heads that have been proposed as being key to large language models, Mamba not only solves them easily but can extrapolate solutions indefinitely long ( > > 1M tokens).

•

Audio and Genomics. Mamba out-performs prior state-of-the-art models such as SaShiMi, Hyena, and Transformers on modeling audio waveforms and DNA sequences, both in pretraining quality and downstream metrics (e.g. reducing FID on a challenging speech generation dataset by more than half). In both settings, its performance improves with longer context up to million-length sequences.

•

Language Modeling. Mamba is the first linear-time sequence model that truly achieves Transformer-quality performance, both in pretraining perplexity and downstream evaluations.
With scaling laws up to 1B parameters, we show that Mamba exceeds the performance of a large range of baselines, including very strong modern Transformer training recipes based on LLaMa ( Touvron et al. 2023 ).
Our Mamba language model has 5 × \times generation throughput compared to Transformers of similar size, and Mamba-3B’s quality matches that of Transformers twice its size (e.g. 4 points higher avg. on common sense reasoning compared to Pythia-3B and even exceeding Pythia-7B).

Model code and pre-trained checkpoints are open-sourced at https://github.com/state-spaces/mamba.

Figure 1 :
( Overview.)
Structured SSMs independently map each channel (e.g. D = 5 D=5 ) of an input x x to output y y through a higher dimensional latent state h h (e.g. N = 4 N=4 ).
Prior SSMs avoid materializing this large effective state ( D ​ N DN, times batch size B B and sequence length L L ) through clever alternate computation paths requiring
time-invariance: the ( Δ, 𝑨, 𝑩, 𝑪 ) (\Delta,\bm{A},\bm{B},\bm{C}) parameters are constant across time.
Our selection mechanism adds back input-dependent dynamics, which
also requires a careful hardware-aware algorithm to only materialize the expanded states in more efficient levels of the GPU memory hierarchy.

2 State Space Models

Structured state space sequence models (S4) are a recent class of sequence models for deep learning that are broadly related to RNNs, and CNNs,
and classical state space models.
They are inspired by a particular continuous system ( 1 )
that maps a 1-dimensional function or sequence x ⁡ ( t ) ∈ ℝ ↦ y ⁡ ( t ) ∈ ℝ x(t)\in\mathbb{R}\mapsto y(t)\in\mathbb{R} through an implicit latent state h ⁡ ( t ) ∈ ℝ N h(t)\in\mathbb{R}^{N}.

Concretely,
S4 models are defined with four parameters ( Δ, 𝑨, 𝑩, 𝑪 ) (\Delta,\bm{A},\bm{B},\bm{C}), which define a sequence-to-sequence transformation in two stages.

h ′ ​ ( t ) \displaystyle h^{\prime}(t)

= 𝑨 ​ h ​ ( t ) + 𝑩 ​ x ​ ( t ) \displaystyle=\bm{A}h(t)+\bm{B}x(t)

(1a)

y ⁡ ( t ) \displaystyle y(t)

= 𝑪 ​ h ​ ( t ) \displaystyle=\bm{C}h(t)

(1b)

h t \displaystyle h_{t}

= 𝑨 ¯ ​ h t − 1 + 𝑩 ¯ ​ x t \displaystyle=\overline{\bm{A}}h_{t-1}+\overline{\bm{B}}x_{t}

(2a)

y t \displaystyle y_{t}

= 𝑪 ​ h t \displaystyle=\bm{C}h_{t}

(2b)

𝑲 ¯ \displaystyle\bm{\overline{K}}

= ( 𝑪 ​ 𝑩 ¯, 𝑪 ​ 𝑨 ¯ ​ 𝑩 ¯, …, 𝑪 ​ 𝑨 ¯ k ​ 𝑩 ¯, … ) \displaystyle=(\bm{C}\bm{\overline{B}},\bm{C}\bm{\overline{A}}\bm{\overline{B}},\dots,\bm{C}\bm{\overline{A}}^{k}\bm{\overline{B}},\dots)

(3a)

y \displaystyle y

= x ∗ 𝑲 ¯ \displaystyle=x\ast\bm{\overline{K}}

(3b)

Discretization.

The first stage transforms the “continuous parameters” ( Δ, 𝑨, 𝑩 ) (\Delta,\bm{A},\bm{B}) to “discrete parameters” ( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}) through fixed formulas 𝑨 ¯ = f A ​ ( Δ, 𝑨 ) \overline{\bm{A}}=f_{A}(\Delta,\bm{A}) and 𝑩 ¯ = f B ​ ( Δ, 𝑨, 𝑩 ) \overline{\bm{B}}=f_{B}(\Delta,\bm{A},\bm{B}),
where the pair ( f A, f B ) (f_{A},f_{B}) is called a discretization rule.

Various rules can be used such as the zero-order hold (ZOH) defined in equation ( 4 ).

𝑨 ¯ = exp ⁡ ( Δ ​ 𝑨 ) 𝑩 ¯ = ( Δ ​ 𝑨 ) − 1 ​ ( exp ⁡ ( Δ ​ 𝑨 ) − 𝑰 ) ⋅ Δ ​ 𝑩 \overline{\bm{A}}=\exp(\Delta\bm{A})\qquad\overline{\bm{B}}=(\Delta\bm{A})^{-1}(\exp(\Delta\bm{A})-\bm{I})\cdot\Delta\bm{B}

(4)

Discretization has deep connections to continuous-time systems which can endow them with additional properties such as resolution invariance ( Nguyen et al. 2022 ) and automatically ensuring that the model is properly normalized ( Gu et al. 2023; Orvieto et al. 2023 ).
It also has connections to gating mechanisms of RNNs ( Tallec & Ollivier 2018; Gu et al. 2020a ) which we will revisit in Section 3.5.
However, from a mechanical point of view discretization can simply be viewed as the first step of the computation graph in the forward pass of an SSM.

Alternate flavors of SSMs can bypass the discretization step and parameterize ( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}) directly instead ( Zhang et al. 2023 ), which may be easier to reason about.

Computation.

After the parameters have been transformed from ( Δ, 𝑨, 𝑩, 𝑪 ) ↦ ( 𝑨 ¯, 𝑩 ¯, 𝑪 ) (\Delta,\bm{A},\bm{B},\bm{C})\mapsto(\overline{\bm{A}},\overline{\bm{B}},\bm{C}),
the model can be computed in two ways, either as a linear recurrence ( 2 ) or a global convolution ( 3 ).

Commonly, the model uses the convolutional mode ( 3 ) for efficient parallelizable training (where the whole input sequence is seen ahead of time), and switched into recurrent mode ( 2 ) for efficient autoregressive inference (where the inputs are seen one timestep at a time).

Linear Time Invariance (LTI).

An important property of equations ( 1 ) to ( 3 ) is that the model’s dynamics are constant through time.
In other words ( Δ, 𝑨, 𝑩, 𝑪 ) (\Delta,\bm{A},\bm{B},\bm{C}), and consequently ( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}) as well, are fixed for all time-steps.
This property is called linear time invariance (LTI), which is deeply connected to recurrence and convolutions.
Informally, we think of LTI SSMs as being equivalent to any linear recurrence ( 2a ) or convolution ( 3b ),
and use LTI as an umbrella term for these classes of models.

Thus far, all structured SSMs have been LTI (e.g. computed as convolutions) because of fundamental efficiency constraints, discussed in Section 3.3.
However, a core insight of this work is that LTI models have fundamental limitations in modeling certain types of data,
and our technical contributions involve removing the LTI constraint while overcoming the efficiency bottlenecks.

Structure and Dimensions.

Finally, we note that structured SSMs are so named because computing them efficiently also requires imposing structure on the 𝑨 \bm{A} matrix.
The most popular form of structure is diagonal ( Gupta et al. 2022; Gu et al. 2022a; Smith et al. 2023 ), which we also use.

In this case, the 𝑨 ∈ ℝ N × N, 𝑩 ∈ ℝ N × 1, 𝑪 ∈ ℝ 1 × N \bm{A}\in\mathbb{R}^{N\times N},\bm{B}\in\mathbb{R}^{N\times 1},\bm{C}\in\mathbb{R}^{1\times N} matrices can all be represented by N N numbers.
To operate over an input sequence x x of batch size B B and length L L with D D channels,
the SSM is applied independently to each channel.
Note that in this case, the total hidden state has dimension D ​ N DN per input, and computing it over the sequence length requires O ⁡ ( B ​ L ​ D ​ N ) O(BLDN) time and memory; this is the root of the fundamental efficiency bottleneck addressed in Section 3.3.

General State Space Models.

We note that the term state space model has a very broad meaning which simply represents the notion of any recurrent process with a latent state.
It has been used to refer to many disparate concepts in different disciplines,
including Markov decision processes (MDP) (reinforcement learning ( Hafner et al. 2020 ) ),
dynamic causal modeling (DCM) (computational neuroscience ( Friston et al. 2003 ) ),
Kalman filters (controls ( Kalman 1960 ) ),
hidden Markov models (HMM) and linear dynamical systems (LDS) (machine learning),
and recurrent (and sometimes convolutional) models at large (deep learning).

Throughout this entire paper we use the term “SSM” to refer exclusively to the class of structured SSMs or S4 models ( Gu et al. 2022; Gupta et al. 2022; Gu et al. 2022a; Ma et al. 2023; Smith et al. 2023; Hasani et al. 2023 ) and use these terms interchangeably.
For convenience we may also include derivatives of such models, such as those focusing on either the linear-recurrence or global-convolution viewpoints ( Orvieto et al. 2023; Li et al. 2023; Poli et al. 2023 ), and clarify nuances when necessary.

SSM Architectures.

SSMs are standalone sequence transformations that can be incorporated into end-to-end neural network architectures.

(We also sometimes call SSM architectures SSNNs, which are to SSM layers as CNNs are to linear convolution layers.)

We discuss some of the most well-known SSM architectures, many of which will also serve as our primary baselines.

•

Linear attention ( Katharopoulos et al. 2020 ) is an approximation of self-attention involving a recurrence which can be viewed as a degenerate linear SSM.

•

H3 ( Dao et al. 2023 ) generalized this recurrence to use S4; it can be viewed as an architecture with an SSM sandwiched by two gated connections ( Figure 3 ).
H3 also inserts a standard local convolution, which they frame as a shift-SSM, before the main SSM layer.

•

Hyena ( Poli et al. 2023 ) uses the same architecture as H3 but replaces the S4 layer with an MLP-parameterized global convolution ( Romero et al. 2021 ).

•

RetNet ( Sun et al. 2023 ) adds an additional gate to the architecture and uses a simpler SSM, allowing an alternative parallelizable computation path, using a variant of multi-head attention (MHA) instead of convolutions.

•

RWKV ( Peng et al. 2023 ) is a recent RNN designed for language modeling based on another linear attention approximation, the attention-free Transformer ( Zhai et al. 2021 ). Its main “WKV” mechanism involves LTI recurrences and can be viewed as the ratio of two SSMs.

Other closely related SSMs and architectures are discussed further in an extended related work ( Appendix B ).
We highlight in particular S5 ( Smith et al. 2023 ), QRNN ( Bradbury et al. 2016 ),
and SRU ( Lei et al. 2017 ),
which we view as the most closely related methods to our core selective SSM.

3 Selective State Space Models

We motivate our selection mechanism using intuition from synthetic tasks ( Section 3.1 ), then explain how to incorporate this mechanism into state space models ( Section 3.2 ).
The resulting time-varying SSMs cannot use convolutions, presenting a technical challenge of how to compute them efficiently.
We overcome this with a hardware-aware algorithm that exploits the memory hierarchy on modern hardware ( Section 3.3 ).
We then describe a simple SSM architecture without attention or even MLP blocks ( Section 3.4 ).
Finally, we discuss some additional properties of selection mechanisms ( Section 3.5 ).

3.1 Motivation: Selection as a Means of Compression

We argue that a fundamental problem of sequence modeling is compressing context into a smaller state.
In fact, we can view the tradeoffs of popular sequence models from this point of view.
For example, attention is both effective and inefficient because it explicitly does not compress context at all.
This can be seen from the fact that autoregressive inference requires explicitly storing the entire context (i.e. the KV cache), which directly causes the slow linear-time inference and quadratic-time training of Transformers.
On the other hand, recurrent models are efficient because they have a finite state, implying constant-time inference and linear-time training.
However, their effectiveness is limited by how well this state has compressed the context.

To understand this principle,
we focus on two running examples of synthetic tasks ( Figure 2 ).

•

The Selective Copying task modifies the popular Copying task ( Arjovsky et al. 2016 ) by varying the position of the tokens to memorize. It requires content-aware reasoning to be able to memorize the relevant tokens ( colored ) and filter out the irrelevant ones ( white ).

•

The Induction Heads task is a well-known mechanism hypothesized to explain the majority of in-context learning abilities of LLMs ( Olsson et al. 2022 ). It requires context-aware reasoning to know when to produce the correct output in the appropriate context ( black ).

These tasks reveal the failure mode of LTI models. From the recurrent view, their constant dynamics (e.g. the ( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}) transitions in ( 2 )) cannot let them select the correct information from their context, or affect the hidden state passed along the sequence in an input-dependent way.
From the convolutional view, it is known that global convolutions can solve the vanilla Copying task ( Romero et al. 2021 ) because it only requires time-awareness, but that they have difficulty with the Selective Copying task because of lack of content-awareness ( Figure 2 ).
More concretely, the spacing between inputs-to-outputs is varying and cannot be modeled by static convolution kernels.

In summary, the efficiency vs. effectiveness tradeoff of sequence models is characterized by how well they compress their state:
efficient models must have a small state, while effective models must have a state that contains all necessary information from the context.
In turn, we propose that a fundamental principle for building sequence models is
selectivity : or the context-aware ability to focus on or filter out inputs into a sequential state.
In particular, a selection mechanism controls how information propagates or interacts along the sequence dimension (see Section 3.5 for more discussion).

Figure 2 :
( Left ) The standard version of the Copying task involves constant spacing between input and output elements and is easily solved by time-invariant models such as linear recurrences and global convolutions.
( Right Top ) The Selective Copying task has random spacing in between inputs and requires time-varying models that can selectively remember or ignore inputs depending on their content.
( Right Bottom ) The Induction Heads task is an example of associative recall that requires retrieving an answer based on context, a key ability for LLMs.

3.2 Improving SSMs with Selection

One method of incorporating a selection mechanism into models is by letting their parameters that affect interactions along the sequence (e.g. the recurrent dynamics of an RNN or the convolution kernel of a CNN) be input-dependent.

Algorithms 1 and 2 illustrates the main selection mechanism that we use. The main difference is simply making several parameters Δ, 𝑩, 𝑪 \Delta,\bm{B},\bm{C} functions of the input, along with the associated changes to tensor shapes throughout.
In particular, we highlight that these parameters now have a length dimension L L,
meaning that the model has changed from time-invariant to time-varying.
(Note that shape annotations were described in Section 2.)
This loses the equivalence to convolutions ( 3 ) with implications for its efficiency, discussed next.

We specifically choose s B ​ ( x ) = 𝖫𝗂𝗇𝖾𝖺𝗋 N ​ ( x ) s_{B}(x)=\mathsf{Linear}_{N}(x), s C ​ ( x ) = 𝖫𝗂𝗇𝖾𝖺𝗋 N ​ ( x ) s_{C}(x)=\mathsf{Linear}_{N}(x),
s Δ ​ ( x ) = 𝖡𝗋𝗈𝖺𝖽𝖼𝖺𝗌𝗍 D ​ ( 𝖫𝗂𝗇𝖾𝖺𝗋 1 ​ ( x ) ) s_{\Delta}(x)=\mathsf{Broadcast}_{D}(\mathsf{Linear}_{1}(x)), and τ Δ = 𝗌𝗈𝖿𝗍𝗉𝗅𝗎𝗌 \tau_{\Delta}=\mathsf{softplus},
where 𝖫𝗂𝗇𝖾𝖺𝗋 d \mathsf{Linear}_{d} is a parameterized projection to dimension d d.
The choice of s Δ s_{\Delta} and τ Δ \tau_{\Delta} is due to a connection to RNN gating mechanisms explained in Section 3.5.

Algorithm 1 SSM (S4)

1:

x : ( 𝙱, 𝙻, 𝙳 ) x:\mathtt{(B,L,D)}

2:

y : ( 𝙱, 𝙻, 𝙳 ) y:\mathtt{(B,L,D)}

3:

𝑨 : ( 𝙳, 𝙽 ) ← 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 \bm{A}:\mathtt{(D,N)}\leftarrow\mathsf{Parameter}
⊳ \triangleright Represents structured N × N N\times N matrix

4:

𝑩 : ( 𝙳, 𝙽 ) ← 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 \bm{B}:\mathtt{(D,N)}\leftarrow\mathsf{Parameter}

5:

𝑪 : ( 𝙳, 𝙽 ) ← 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 \bm{C}:\mathtt{(D,N)}\leftarrow\mathsf{Parameter}

6:

Δ : ( 𝙳 ) ← τ Δ ​ ( 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 ) \Delta:\mathtt{(D)}\leftarrow\tau_{\Delta}(\mathsf{Parameter})

7:

𝑨 ¯, 𝑩 ¯ : ( 𝙳, 𝙽 ) ← 𝖽𝗂𝗌𝖼𝗋𝖾𝗍𝗂𝗓𝖾 ⁡ ( Δ, 𝑨, 𝑩 ) \overline{\bm{A}},\overline{\bm{B}}:\mathtt{(D,N)}\leftarrow\mathsf{discretize}(\Delta,\bm{A},\bm{B})

8:

y ← 𝖲𝖲𝖬 ​ ( 𝑨 ¯, 𝑩 ¯, 𝑪 ) ​ ( x ) y\leftarrow\mathsf{SSM}(\overline{\bm{A}},\overline{\bm{B}},\bm{C})(x)
⊳ \triangleright Time-invariant: recurrence or convolution

9:

return y y

Algorithm 2 SSM + Selection (S6)

1:

x : ( 𝙱, 𝙻, 𝙳 ) x:\mathtt{(B,L,D)}

2:

y : ( 𝙱, 𝙻, 𝙳 ) y:\mathtt{(B,L,D)}

3:

𝑨 : ( 𝙳, 𝙽 ) ← 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 \bm{A}:\mathtt{(D,N)}\leftarrow\mathsf{Parameter}
⊳ \triangleright Represents structured N × N N\times N matrix

4:

𝑩 : ( 𝙱, 𝙻, 𝙽 ) ← s B ​ ( x ) \bm{B}:{\color[rgb]{0.72,0,0}\mathtt{(B,L,N)}}\leftarrow{\color[rgb]{0.72,0,0}s_{B}(x)}

5:

𝑪 : ( 𝙱, 𝙻, 𝙽 ) ← s C ​ ( x ) \bm{C}:{\color[rgb]{0.72,0,0}\mathtt{(B,L,N)}}\leftarrow{\color[rgb]{0.72,0,0}s_{C}(x)}

6:

Δ : ( 𝙱, 𝙻, 𝙳 ) ← τ Δ ​ ( 𝖯𝖺𝗋𝖺𝗆𝖾𝗍𝖾𝗋 + s Δ ​ ( x ) ) \Delta:{\color[rgb]{0.72,0,0}\mathtt{(B,L,D)}}\leftarrow\tau_{\Delta}(\mathsf{Parameter}{\color[rgb]{0.72,0,0}+s_{\Delta}(x)})

7:

𝑨 ¯, 𝑩 ¯ : ( 𝙱, 𝙻, 𝙳, 𝙽 ) ← 𝖽𝗂𝗌𝖼𝗋𝖾𝗍𝗂𝗓𝖾 ⁡ ( Δ, 𝑨, 𝑩 ) \overline{\bm{A}},\overline{\bm{B}}:{\color[rgb]{0.72,0,0}\mathtt{(B,L,D,N)}}\leftarrow\mathsf{discretize}(\Delta,\bm{A},\bm{B})

8:

y ← 𝖲𝖲𝖬 ​ ( 𝑨 ¯, 𝑩 ¯, 𝑪 ) ​ ( x ) y\leftarrow\mathsf{SSM}(\overline{\bm{A}},\overline{\bm{B}},\bm{C})(x)
⊳ \triangleright Time-varying : recurrence ( scan ) only

9:

return y y

3.3 Efficient Implementation of Selective SSMs

Hardware-friendly primitives such as convolutions ( Krizhevsky et al. 2012 ) and attention ( Bahdanau et al. 2015; Vaswani et al. 2017 ) enjoy widespread application.
Here we aim to make selective SSMs efficient on modern hardware (GPUs) as well.
The selection mechanism is quite natural, and earlier works attempted to incorporate special cases of selection, such as letting Δ \Delta vary over time in recurrent SSMs ( Gu et al. 2020 ).
However,

as previously mentioned a core limitation in the usage of SSMs is their computational efficiency,

which was why S4 and all derivatives used LTI (non-selective) models, most commonly in the form of global convolutions.

3.3.1 Motivation of Prior Models

We first revisit this motivation and overview our approach to overcome limitations of prior methods.

•

At a high level, recurrent models such as SSMs always balance a tradeoff between expressivity and speed: as discussed in Section 3.1, models with larger hidden state dimension should be more effective but slower. Thus we want to maximize hidden state dimension without paying speed and memory costs.

•

Note that the recurrent mode is more flexible than the convolution mode, since the latter ( 3 ) is derived from expanding the former ( 2 ) ( Gu et al. 2021; Gu et al. 2022 ).
However, this would require computing and materializing the latent state h h with shape ( 𝙱, 𝙻, 𝙳, 𝙽 ) \mathtt{(B,L,D,N)}, which is much larger (by a factor of N N, the SSM state dimension) than the input x x and output y y of shape ( 𝙱, 𝙻, 𝙳 ) \mathtt{(B,L,D)}.
Thus the more efficient convolution mode was introduced which could bypass the state computation and materializes a convolution kernel ( 3a ) of size only ( 𝙱, 𝙻, 𝙳 ) \mathtt{(B,L,D)}.

•

Prior LTI state space models leverage the dual recurrent-convolutional forms to increase the effective state dimension by a factor of N N ( ≈ 10 − 100 \approx 10-100 ), much larger than traditional RNNs, without efficiency penalties.

3.3.2 Overview of Selective Scan: Hardware-Aware State Expansion

The selection mechanism is designed to overcome the limitations of LTI models;
at the same time, we therefore need to revisit the computation problem of SSMs.
We address this with three classical techniques: kernel fusion, parallel scan, and recomputation.
We make two main observations:

•

The naive recurrent computation uses O ⁡ ( B ​ L ​ D ​ N ) O(BLDN) FLOPs while the convolutional computation uses O ⁡ ( B ​ L ​ D ​ log ⁡ ( L ) ) O(BLD\log(L)) FLOPs, and the former has a lower constant factor.
Thus for long sequences and not-too-large state dimension N N, the recurrent mode can actually use fewer FLOPs.

•

The two challenges are the sequential nature of recurrence, and the large memory usage.
To address the latter, just like the convolutional mode, we can attempt to not actually materialize the full state h h.

The main idea is to leverage properties of modern accelerators (GPUs) to materialize the state h h only in more efficient levels of the memory hierarchy.
In particular, most operations (except matrix multiplication)
are bounded by memory bandwidth ( Williams et al. 2009; Ivanov et al. 2021; Dao et al. 2022 ).
This includes our scan operation,
and we use
kernel fusion to reduce the amount of memory IOs, leading to a significant speedup
compared to a standard implementation.

Concretely, instead of preparing the scan input
( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}) of size ( 𝙱, 𝙻, 𝙳, 𝙽 ) \mathtt{(B,L,D,N)} in GPU HBM
(high-bandwidth memory),
we load the SSM parameters ( Δ, 𝑨, 𝑩, 𝑪 ) (\Delta,\bm{A},\bm{B},\bm{C}) directly from slow HBM to fast SRAM,
perform the discretization and recurrence in SRAM,
and then write the final outputs of size ( 𝙱, 𝙻, 𝙳 ) (\mathtt{B,L,D}) back to HBM.

To avoid the sequential recurrence, we observe that
despite not being linear it can still be parallelized with a
work-efficient parallel scan algorithm ( Blelloch 1990; Martin & Cundy 2018; Smith et al. 2023 ).

Finally, we must also avoid saving the intermediate states, which are necessary for backpropagation.
We carefully apply the classic technique of recomputation to reduce the memory requirements: the intermediate states are not stored but recomputed in the backward pass when the inputs are loaded from HBM to SRAM.
As a result, the fused selective scan layer has the same memory requirements as an optimized
transformer implementation with FlashAttention.

Details of the fused kernel and recomputation are in Appendix D.

The full Selective SSM layer and algorithm is illustrated in Figure 1.

3.4 A Simplified SSM Architecture

As with structured SSMs, selective SSMs are standalone sequence transformations that can be flexibly incorporated into neural networks.
The H3 architecture is the basis for the most well-known SSM architectures ( Section 2 ), which are generally comprised of a block inspired by linear attention interleaved with an MLP (multi-layer perceptron) block.
We simplify this architecture by combining these two components into one, which is stacked homogenously ( Figure 3 ).
This is inspired by the gated attention unit (GAU) ( Hua et al. 2022 ), which did something similar for attention.

This architecture involves expanding the model dimension D D by a controllable expansion factor E E.
For each block, most of the parameters ( 3 ​ E ​ D 2 3ED^{2} ) are in the linear projections
( 2 ​ E ​ D 2 2ED^{2} for input projections, E ​ D 2 ED^{2} for output projection)
while the inner SSM contributes less.

The number of SSM parameters (projections for Δ, 𝑩, 𝑪 \Delta,\bm{B},\bm{C}, and the matrix 𝑨 \bm{A} ) are much smaller in comparison.

We repeat this block, interleaved with standard normalization and residual connections, to form the Mamba architecture.
We always fix to E = 2 E=2 in our experiments and use two stacks of the block to match the 12 ​ D 2 12D^{2} parameters of a Transformer’s interleaved MHA (multi-head attention) and MLP blocks.

We use the SiLU / Swish activation function ( Hendrycks & Gimpel 2016; Ramachandran et al. 2017 ),
motivated so that the Gated MLP becomes the popular “SwiGLU” variant ( Dauphin et al. 2017; Shazeer 2020; Chowdhery et al. 2023; Touvron et al. 2023 ).

Finally, we additionally use an optional normalization layer (we choose LayerNorm ( Ba et al. 2016a ) ),
motivated by RetNet’s usage of a normalization layer in a similar location ( Sun et al. 2023 ).

Figure 3 :
( Architecture.) Our simplified block design combines the H3 block, which is the basis of most SSM architectures, with the ubiquitous MLP block of modern neural networks. Instead of interleaving these two blocks, we simply repeat the Mamba block homogenously.
Compared to the H3 block, Mamba replaces the first multiplicative gate with an activation function.
Compared to the MLP block, Mamba adds an SSM to the main branch.
For σ \sigma we use the SiLU / Swish activation ( Hendrycks & Gimpel 2016; Ramachandran et al. 2017 ).

3.5 Properties of Selection Mechanisms

The selection mechanism is a broader concept that can be applied in different ways,
such as to more traditional RNNs or CNNs, to different parameters (e.g. 𝑨 \bm{A} in Algorithm 2 ), or using different transformations s ⁡ ( x ) s(x).

3.5.1 Connection to Gating Mechanisms

We highlight the most important connection: the classical gating mechanism of RNNs is an instance of our selection mechanism for SSMs.
We note that the connection between RNN gating and the discretization of continuous-time systems is well established ( Funahashi & Nakamura 1993; Tallec & Ollivier 2018 ).
In fact, Theorem 1 is an improvement of Gu et al. 2021 generalizing to the ZOH discretization and input-dependent gates (proof in Appendix C ).
More broadly, Δ \Delta in SSMs can be seen to play a generalized role of the RNN gating mechanism.
In line with prior work, we adopt the view that discretization of SSMs is the principled foundation of heuristic gating mechanisms.

Theorem 1.

When N = 1, 𝐀 = − 1, 𝐁 = 1, s Δ = 𝖫𝗂𝗇𝖾𝖺𝗋 ⁡ ( x ) N=1,\bm{A}=-1,\bm{B}=1,s_{\Delta}=\mathsf{Linear}(x), and τ Δ = 𝗌𝗈𝖿𝗍𝗉𝗅𝗎𝗌 \tau_{\Delta}=\mathsf{softplus},
then the selective SSM recurrence ( Algorithm 2 ) takes the form

g t \displaystyle g_{t}

= σ ⁡ ( 𝖫𝗂𝗇𝖾𝖺𝗋 ⁡ ( x t ) ) \displaystyle=\sigma(\mathsf{Linear}(x_{t}))

(5)

h t \displaystyle h_{t}

= ( 1 − g t ) ​ h t − 1 + g t ​ x t. \displaystyle=(1-g_{t})h_{t-1}+g_{t}x_{t}.

As mentioned in Section 3.2, our specific choices of s Δ, τ Δ s_{\Delta},\tau_{\Delta} is from this connection.
In particular, note that if a given input x t x_{t} should be completely ignored (as necessary in the synthetic tasks),
all D D channels should ignore it, and so we project the input down to 1 1 dimension before repeating/broadcasting with Δ \Delta.

3.5.2 Interpretation of Selection Mechanisms

We elaborate on three particular mechanistic effects of selection.

Variable Spacing.

Selectivity allows filtering out irrelevant noise tokens that may occur between inputs of interest.
This is exemplified by the Selective Copying task, but occurs ubiquitously in common data modalities, particularly for discrete data – for example the presence of language fillers such as “um”.
This property arises because the model can mechanistically filter out any particular input x t x_{t}, for example in the gated RNN case ( Theorem 1 ) when g t → 0 g_{t}\to 0.

Filtering Context.

It has been empirically observed that many sequence models do not improve with longer context ( Shi et al. 2023 ), despite the principle that more context should lead to strictly better performance. An explanation is that many sequence models cannot effectively ignore irrelevant context when necessary; an intuitive example are global convolutions (and general LTI models).
On the other hand, selective models can simply reset their state at any time to remove extraneous history,
and thus their performance in principle improves monotonicly with context length (e.g. Section 4.3.2 ).

Boundary Resetting.

In settings where multiple independent sequences are stitched together, Transformers can keep them separate by instantiating a particular attention mask,
while LTI models will bleed information between the sequences.
Selective SSMs can also reset their state at boundaries (e.g. Δ t → ∞ \Delta_{t}\to\infty, or Theorem 1 when g t → 1 g_{t}\to 1 ).
These settings may occur artificially (e.g. packing documents together to improve hardware utilization)
or naturally (e.g. episode boundaries in reinforcement learning ( Lu et al. 2023 ) ).

Additionally, we elaborate on effects of each selective parameter.

Interpretation of Δ \Delta.

In general, Δ \Delta controls the balance between how much to focus or ignore the current input x t x_{t}.
It generalizes RNN gates (e.g. g t g_{t} in Theorem 1 ):
mechanically, a large Δ \Delta resets the state h h and focuses on the current input x x,
while a small Δ \Delta persists the state and ignores the current input.
SSMs ( 1 )-( 2 ) can be interpreted as a continuous system discretized by a timestep Δ \Delta,
and in this context the intuition is that large Δ → ∞ \Delta\to\infty represents the system focusing on the current input for longer (thus “selecting” it and forgetting its current state)
while a small Δ → 0 \Delta\to 0 represents a transient input that is ignored.

Interpretation of 𝑨 \bm{A}.

We remark that while the 𝑨 \bm{A} parameter could also be selective, it ultimately affects the model only through its interaction with Δ \Delta via 𝑨 ¯ = exp ⁡ ( Δ ​ 𝑨 ) \overline{\bm{A}}=\exp(\Delta\bm{A}) (the discretization ( 4 )).
Thus selectivity in Δ \Delta is enough to ensure selectivity in ( 𝑨 ¯, 𝑩 ¯ ) (\overline{\bm{A}},\overline{\bm{B}}), and is the main source of improvement.
We hypothesize that making 𝑨 \bm{A} selective in addition to (or instead of) Δ \Delta would have similar performance,
and leave it out for simplicity.

Interpretation of 𝑩 \bm{B} and 𝑪 \bm{C}.

As discussed in Section 3.1, the most important property of selectivity is filtering out irrelevant information
so that a sequence model’s context can be compressed into an efficient state.
In an SSM, modifying 𝑩 \bm{B} and 𝑪 \bm{C} to be selective allows finer-grained control over whether to let an input x t x_{t} into the state h t h_{t}, or the state into the output y t y_{t}.
These can be interpreted as allowing the model to modulate the recurrent dynamics based on content (input) and context (hidden states) respectively.

3.6 Additional Model Details

Real vs. Complex.

Most prior SSMs use complex numbers in their state h h, which is necessary for strong performance on many tasks in perceptual modalities ( Gu et al. 2022 ).
However, it has been empirically observed that completely real-valued SSMs seem to work fine, and possibly even better, in some settings ( Ma et al. 2023 ).
We use real values as the default, which work well for all but one of our tasks;
we hypothesize that the complex-real tradeoff is related to the continuous-discrete spectrum in data modalities, where complex numbers are helpful for continuous modalities (e.g. audio, video) but not discrete (e.g. text, DNA).

Initialization.

Most prior SSMs also suggest special initializations, particularly in the complex-valued case, which can help in several settings such as low-data regimes.
Our default initialization for the complex case is S4D-Lin
and for the real case is S4D-Real ( Gu et al. 2022a ),
which is based on the HIPPO theory ( Gu et al. 2020 ).
These define the n n -th element of 𝑨 \bm{A} as − 1 / 2 + n i -1/2+ni
and − ( n + 1 ) -(n+1) respectively.
However, we expect many initializations to work fine, particularly in the large-data and real-valued SSM regimes;
some ablations are considered in Section 4.6.

Parameterization of Δ \Delta.

We defined the selective adjustment to Δ \Delta as s Δ ​ ( x ) = 𝖡𝗋𝗈𝖺𝖽𝖼𝖺𝗌𝗍 D ​ ( 𝖫𝗂𝗇𝖾𝖺𝗋 1 ​ ( x ) ) s_{\Delta}(x)=\mathsf{Broadcast}_{D}(\mathsf{Linear}_{1}(x)),
which was motivated by the mechanics of Δ \Delta ( Section 3.5 ).
We observe that it can be generalized
from dimension 1 1 to a larger dimension 𝚁 \mathtt{R}.
We set this to be a small fraction of 𝙳 \mathtt{D},
which uses a negligible number of parameters compared to the main Linear projections in the block.
We additionally note that the broadcasting operation can instead
be viewed as another Linear projection, initialized to a specific pattern of 1 1 ’s and 0 0 ’s;
if this projection is trainable, this leads to the alternative s Δ ​ ( x ) = 𝖫𝗂𝗇𝖾𝖺𝗋 D ​ ( 𝖫𝗂𝗇𝖾𝖺𝗋 R ​ ( x ) ) s_{\Delta}(x)=\mathsf{Linear}_{D}(\mathsf{Linear}_{R}(x)), which can be viewed as a low-rank projection.

In our experiments, the Δ \Delta parameter (which can be viewed as a bias term) is initialized to τ Δ − 1 ​ ( 𝖴𝗇𝗂𝖿𝗈𝗋𝗆 ⁡ ( [ 0.001, 0.1 ] ) ) \tau_{\Delta}^{-1}(\mathsf{Uniform}([0.001,0.1])),
following prior work on SSMs ( Gu et al. 2023 ).

Remark 3.1.

For brevity in our experimental results, we sometimes abbreviate selective SSMs as S6 models, because they are S4 models with a selection mechanism and computed with a scan.

4 Empirical Evaluation

In Section 4.1 we test Mamba’s ability to solve the two synthetic tasks motivated in Section 3.1.
We then evaluate on three domains, each evaluated on autoregressive pretraining as well as downstream tasks.

•

Section 4.2 : language model pretraining (scaling laws), and zero-shot downstream evaluation.

•

Section 4.3 : DNA sequence pretraining, and fine-tuning on a long-sequence classification task.

•

Section 4.4 : audio waveform pretraining, and the quality of autoregressively generated speech clips.

Finally, Section 4.5 shows Mamba’s computational efficiency at both training and inference time,
and Section 4.6 ablates various components of the architecture and selective SSMs.

4.1 Synthetic Tasks

Full experiment details for these tasks including task details and training protocol are in Section E.1.

4.1.1 Selective Copying

The Copying task is one of the most well-studied synthetic tasks for sequence modeling,
originally designed to test the memorization abilities of recurrent models.
As discussed in Section 3.1, LTI SSMs (linear recurrences and global convolutions)
can easily solve this task by only keeping track of time instead of reasoning about the data;
for example, by constructing a convolution kernel of exactly the right length ( Figure 2 ).
This was explicitly validated in earlier work on global convolutions ( Romero et al. 2021 ).
The Selective Copying task prevents this shortcut by randomizing the spacing between tokens.
Note that this task has been introduced before as the Denoising task ( Jing et al. 2019 ).

Note that many previous works argue that adding architecture gating (multiplicative interactions) can endow models with “data-dependence” and solve related tasks ( Dao et al. 2023; Poli et al. 2023 ).
However, we find this explanation insufficient intuitively because such gating does not interact along the sequence axis, and cannot affect the spacing between tokens.
In particular architecture gating is not an instance of a selection mechanism ( Appendix A ).

Figure 5 confirms that gated architectures such as H3 and Mamba only partially improve performance,
while the selection mechanism (modifying S4 to S6) easily solves this task, particularly when combined with these more powerful architectures.

4.1.2 Induction Heads

Induction heads ( Olsson et al. 2022 ) is a simple task from the mechanistic interpretability lens ( Elhage et al. 2021 ) that is surprisingly predictive of the in-context learning ability of LLMs. It requires models to perform associative recall and copy: for example, if the model has seen a bigram such as “Harry Potter” in the sequence, then the next time “Harry” appears in the same sequence, the model should be able to predict “Potter” by copying from history.

Dataset.

We train a 2-layer model on the induction heads task at sequence length 256 256, with a vocab size of 16 16,
which is comparable to prior work on this task ( Dao et al. 2023 ) but with longer sequences.
We additionally investigate generalization and extrapolation abilities by evaluating on a range of sequence lengths from 2 6 = 64 2^{6}=64 up to 2 20 = 1048576 2^{20}=1048576 at test time.

Models.

Following established work on induction heads, we use 2 layer models, which allows attention to mechanistically solve the induction heads task ( Olsson et al. 2022 ).
We test both multi-head attention (8 heads, with various positional encodings) and SSM variants.
We use a model dimension D D of 64 64 for Mamba and 128 128 for the other models.

Results.

Figure 5 shows that
Mamba—or more precisely, its selective SSM layer—has the ability to solve the task perfectly because of its ability to selectively remember the relevant token while ignoring everything else in between.
It generalizes perfectly to million-length sequences, or 4000 × 4000\times longer than it saw during training,
while no other method goes beyond 2 × 2\times.

Out of positional encoding variants for attention models, xPos (which was designed for length extrapolation) is slightly better than the others;
also note that all attention models were only tested up to sequence length 2 14 = 16384 2^{14}=16384 due to memory limitations.
Out of other SSMs, H3 and Hyena are similar, contrary to the findings in Poli et al. 2023.

Model

Arch.

Layer

Acc.

S4

No gate

S4

18.3

-

No gate

S6

97.0

H3

H3

S4

57.0

Hyena

H3

Hyena

30.1

-

H3

S6

99.7

-

Mamba

S4

56.4

-

Mamba

Hyena

28.4

Mamba

Mamba

S6

99.8

Figure 4:
( Selective Copying.)
Accuracy for combinations of architectures and inner sequence layers.

Figure 5 :
( Induction Heads.)
Models are trained on sequence length 2 8 = 256 2^{8}=256, and tested on increasing sequence lengths of 2 6 = 64 2^{6}=64 up to 2 20 = 1048576 2^{20}=1048576.
Full numbers in Table 3.

4.2 Language Modeling

We evaluate the Mamba architecture on standard autoregressive language modeling against other architectures, on both pretraining metrics (perplexity) and zero-shot evaluations.
We set the model sizes (depth and width) to mirror GPT3 specifications. We use the Pile dataset ( Gao et al. 2020 ), and follow the training recipe described in Brown et al. 2020.
Al
