# 论文·Mamba-2：状态空间对偶与高效实现

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　241 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2405.21060
抓取: https://ar5iv.labs.arxiv.org/html/2405.21060

Transformers are SSMs: Generalized Models and Efficient Algorithms
Through Structured State Space Duality

Tri Dao

Thanks: Alphabetical by last name.

  
Albert Gu † † footnotemark:

Affiliation: tri@tridao.me, agu@cs.cmu.edu

Abstract

While Transformers have been the main architecture behind deep learning’s
success in language modeling, state-space models (SSMs) such as Mamba have recently been
shown to match or outperform Transformers at small to medium scale.
We show that these families of models are actually quite closely related,
and develop a rich framework of theoretical connections between SSMs and variants of
attention,
connected through various decompositions of a well-studied class of structured semiseparable matrices.
Our state space duality (SSD) framework
allows us to design a new architecture ( Mamba-2 ) whose core layer is an a refinement of Mamba’s selective SSM
that is 2-8 × \times faster, while continuing to be competitive with
Transformers on language modeling.

1 Introduction

Transformers, in particular decoder-only models (e.g. GPT ( Brown et al. 2020 ), Llama ( Touvron et al. 2023 ) ) which process input sequences in a causal fashion, are one of the main drivers of modern deep learning’s success.
Numerous approaches attempt to approximate the core attention layer to address its efficiency issues ( Tay et al. 2022 ), such as scaling quadratically in sequence length during training and requiring a cache of size linear in sequence length during autoregressive generation.
In parallel, a class of alternative sequence models, structured state-space models (SSMs), have emerged with linear scaling in sequence length during training and constant state size during generation.
They show strong performance on long-range tasks (e.g. S4 ( Gu et al. 2022 ) ) and recently matched or beat Transformers on language modeling (e.g. Mamba ( Gu & Dao 2023 ) ) at small to moderate scale.
However, the development of SSMs have appeared disjoint from the community’s collective effort to improve Transformers, such as understanding them theoretically as well as optimizing them on modern hardware.
As a result, it is more difficult to understand and experiment with SSMs compared to Transformers, and it remains challenging to train SSMs as efficiently as Transformers from both an algorithmic and systems perspective.

Our main goal is to develop a rich body of theoretical connections between structured SSMs and variants of attention.
This will allow us to transfer algorithmic and systems optimizations originally developed for Transformers to SSMs, towards the goal of building foundation models that perform better than Transformers while scaling more efficiently in sequence length.
A milestone contribution in this direction was the Linear Attention (LA) framework ( Katharopoulos et al. 2020 ),
which derived a connection between autoregressive attention and linear RNNs
by showing the equivalence between ‘‘dual forms’’ of quadratic kernelized attention and a particular linear recurrence.
This duality allows new capabilities such as the ability to have both efficient parallelizable training and efficient autoregressive inference.
In the same spirit, this paper provides multiple viewpoints connecting linear-complexity SSMs with quadratic-complexity forms to combine the strengths of SSMs and attention. 1 1
1

Technically speaking, these connections only relate to certain flavors of attention; the title of this paper is an homage to Katharopoulos et al. 2020 which first showed that “Transformers are RNNs”.

Figure 1 :
( Structured State-Space Duality.)
This paper fleshes out the relationship between state space models and attention through the bridge of structured matrices.

State Space Duality.

Our framework connecting structured SSMs and variants of attention, which we call structured state space duality (SSD),
is made through the abstractions of structured matrices :
matrices with subquadratic parameters and multiplication complexity.
We develop two broad frameworks for representing sequence models, one as matrix transformations and one as tensor contractions, which each reveal different perspectives of the duality.
Our technical contributions include:

•

We show an equivalence between state space models and a well-studied family of structured matrices called semiseparable matrices ( Section 3 ).
This connection is at the heart our framework, revealing new properties and algorithms for SSMs. A central message of this paper is that different methods of computing state space models can be reframed as various matrix multiplication algorithms on structured matrices.

•

We significantly improve the theory of linear attention ( Katharopoulos et al. 2020 ).
We first provide an incisive proof of its recurrent form through the language of tensor contractions, and then generalize it to a new family of structured masked attention (SMA) ( Section 4 ).

•

We connect SSMs and SMA, showing that they have a large intersection that are duals of each other, possessing both SSM-like linear and attention-like quadratic forms ( Section 5 ).
We also prove that any kernel attention method possessing a fast recurrent form must be an SSM.

Beyond its intrinsic theoretical value, our framework opens up a broad set of directions for understanding and improving sequence models.

Efficient Algorithms.

First and most importantly, our framework exposes new efficient and easily-implementable algorithms for computing SSMs ( Section 6 ).
We introduce a new SSD algorithm, based on block decompositions of semiseparable matrices, that takes advantage of both the linear SSM recurrence and quadratic dual form, obtaining optimal tradeoffs on all main efficiency axes (e.g. training and inference compute, memory usage, and ability to leverage matrix multiplication units on modern hardware).
A dedicated implementation of SSD is 2 − 8 × 2-8\times faster than the optimized selective scan implementation of Mamba, while simultaneously allowing for much larger recurrent state sizes ( 8 × 8\times the size of Mamba or even higher, with minimal slowdown).
SSD is highly competitive with optimized implementations of softmax attention (FlashAttention-2 ( Dao 2024 ) ), crossing over at sequence length 2K and 6 × \times faster at sequence length 16K.

Architecture Design.

One major obstacle to adopting new architectures such as SSMs is the ecosystem tailored to Transformers, such as hardware-efficient optimization and parallelism techniques for large-scale training.
Our framework allows using established conventions and techniques for attention to build a vocabulary of architecture design choices for SSMs, and further improve them ( Section 7 ).
For example, we introduce the analog of heads from multi-head attention (MHA) to SSMs.
We show that the Mamba architecture is a multi-input SSM (MIS) that turns out to be analogous to multi-value attention (MVA), and compare other variants of Mamba with different head structures.

We also use these ideas to make slight modifications to the Mamba block, which allows tensor parallelism to be implemented (e.g. in the style of Megatron ( Shoeybi et al. 2019 ) ).
The main ideas include introducing grouped-value attention (GVA) head structure, and moving all data-dependent projections to occur in parallel at the beginning of the block.

The combination of the modified parallel Mamba block, together with using SSD as the inner SSM layer, results in the Mamba-2 architecture.
We investigate Chinchilla scaling laws for Mamba-2 in the same setting as Mamba, finding that it Pareto dominates Mamba and Transformer++ in both perplexity and wall-clock time.
We additionally train a family of Mamba-2 models at varying sizes on the Pile, showing that it matches or outperforms Mamba and open source Transformers on standard downstream evaluations.
For example, Mamba-2 with 2.7B parameters trained on 300B tokens on the Pile outperforms Mamba-2.8B, Pythia-2.8B and even Pythia-6.9B trained on the same dataset.

Systems Optimizations.

The SSD framework connects SSMs and Transformers, allowing us to leverage a rich body of work on systems optimizations developed for Transformers ( Section 8 ).

•

For example, Tensor Parallelism (TP) is an important model parallelism technique to train large Transformer models by splitting each layer across GPUs on the same node.
We design Mamba-2 to be TP-friendly, reducing the number of synchronization point per block by half.

•

For very long sequences whose activations do not fit on one device, sequence parallelism has been developed for the attention blocks.
We describe how to train SSMs in general and Mamba-2 in particular with sequence parallelism, by passing the recurrent states between devices.

•

For finetuning with examples of different lengths, for best efficiency, Transformer requires sophisticated techniques to remove padding tokens and perform attention on variable length sequences.
We show how Mamba-2 can be trained with variable sequence lengths efficiently, requiring no padding tokens.

Section 9 empirically validates Mamba-2 on language modeling, training efficiency, and a difficult multi-query associative recall task ( Arora et al. 2024a ).
Finally, in Section 10, we provide an extended related work and discuss potential research directions opened up by our framework.

Model code and pre-trained checkpoints are open-sourced at https://github.com/state-spaces/mamba.

2 Background and Overview

2.1 Structured State Space Models

Structured state space sequence models (S4) are a recent class of sequence models for deep learning that are broadly related to RNNs, CNNs,
and classical state space models.
They are inspired by a particular continuous system ( 1 )
that maps a 1-dimensional sequence x ∈ ℝ 𝚃 ↦ y ∈ ℝ 𝚃 x\in\mathbb{R}^{\mathtt{T}}\mapsto y\in\mathbb{R}^{\mathtt{T}} through an implicit latent state h ∈ ℝ ( 𝚃, 𝙽 ) h\in\mathbb{R}^{\mathtt{(T,N)}}.

A general discrete form of structured SSMs takes the form of equation ( 1 ).

h t \displaystyle h_{t}

= A ​ h t − 1 + B ​ x t \displaystyle=Ah_{t-1}+Bx_{t}

(1a)

y t \displaystyle y_{t}

= C ⊤ ​ h t \displaystyle=C^{\top}h_{t}

(1b)

h t \displaystyle h_{t}

= A t ​ h t − 1 + B t ​ x t \displaystyle=A_{t}h_{t-1}+B_{t}x_{t}

(2a)

y t \displaystyle y_{t}

= C t ⊤ ​ h t \displaystyle=C_{t}^{\top}h_{t}

(2b)

where A ∈ ℝ ( 𝙽, 𝙽 ), B ∈ ℝ ( 𝙽, 𝟷 ), C ∈ ℝ ( 𝙽, 𝟷 ) A\in\mathbb{R}^{\mathtt{(N,N)}},B\in\mathbb{R}^{\mathtt{(N,1)}},C\in\mathbb{R}^{\mathtt{(N,1)}}.
Structured SSMs are so named because the A A matrix controlling the temporal dynamics must be structured in order to compute this sequence-to-sequence transformation efficiently enough to be used in deep neural networks.
The original structures introduced were diagonal plus low-rank (DPLR) ( Gu et al. 2022 ) and diagonal ( Gupta et al. 2022; Gu et al. 2022a; Smith et al. 2023 ), which remains the most popular structure.

In this work, we use the term state space model (SSM) to refer to structured SSMs.
There are many flavors of such SSMs, with deep ties to several major paradigms of neural sequence models such as continuous-time, recurrent, and convolutional models ( Gu et al. 2021 ).

We provide a brief overview below, and refer to prior work for more context and details ( Gu 2023; Gu & Dao 2023 ).

Continuous-time Models.

The original structured SSMs originated as continuous-time maps on functions x ⁡ ( t ) ∈ ℝ ↦ y ⁡ ( t ) ∈ ℝ x(t)\in\mathbb{R}\mapsto y(t)\in\mathbb{R}, rather than operating directly on sequences. In the continuous-time perspective, in equation ( 1a ) the matrices ( A, B ) (A,B) are not directly learned but generated from underlying
parameters ( A ̊, B ̊ ) (\mathring{A},\mathring{B}), along with a parameterized step size Δ \Delta.
The “continuous parameters” ( Δ, A ̊, B ̊ ) (\Delta,\mathring{A},\mathring{B}) are converted to “discrete parameters” ( A, B ) (A,B) through fixed formulas A = f A ​ ( Δ, A ̊ ) A=f_{A}(\Delta,\mathring{A}) and B = f B ​ ( Δ, B ̊ ) B=f_{B}(\Delta,\mathring{B}),
where the pair ( f A, f B ) (f_{A},f_{B}) is called a discretization rule.

Remark 1.

While our main models adopt the same parameterization and discretization step as prior work (see Gu & Dao 2023 for details), for simplifying exposition and notation we omit it in the rest of this paper.
We note that prior work on structured SSMs referred to the continuous parameters ( A ̊, B ̊ ) (\mathring{A},\mathring{B}) and discrete parameters ( A, B ) (A,B) as
( A, B ) (A,B) and ( A ¯, B ¯ ) (\bar{A},\bar{B}) instead; we have changed notation to simplify the presentation and focus directly on the discrete parameters, which govern the main SSM recurrence.

Recurrent Models.

Equations ( 1 ) and ( 2 ) take the form of a recurrence which is linear in its input x x.
Structured SSMs can therefore be viewed as types of recurrent neural networks (RNNs), where the linearity endows them with additional properties and
allows them to avoid the sequential computation of traditional RNNs.
Conversely, despite this simplification, SSMs are still fully expressive as sequence transformations (in the sense of universal approximation) ( Kaul 2020; Orvieto et al. 2023; Wang & Xue 2023 ).

Convolutional Models.

When the SSM’s dynamics are constant through time as in equation ( 1 ), the model is called linear time-invariant (LTI).
In this case, they are equivalent to convolutions.
Thus, SSMs can also be viewed as types of CNNs, but where (i) the convolution kernels are implicitly parameterized through the SSM parameters ( A, B, C ) (A,B,C) and
(ii) the convolution kernels are generally global instead of local.
Conversely, through classical signal processing theory all sufficiently well-behaved convolutions can be represented as SSMs.

Commonly, previous LTI SSMs would use the convolutional mode for efficient parallelizable training (where the whole input sequence is seen ahead of time), and switched into recurrent mode ( 1 ) for efficient autoregressive inference (where the inputs are seen one step at a time).

Selective State Space Models.

The form ( 2 ) where the parameters ( A, B, C ) (A,B,C) can also vary in time was introduced in Mamba as the selective SSM.
Compared to the standard LTI formulation ( 1 ), this model can selectively choose to focus on or ignore inputs at every timestep.
It was shown to perform much better than LTI SSMs on information-dense data such as language,
especially as its state size 𝙽 \mathtt{N} increases allowing for more information capacity.
However, it can only be computed in recurrent instead of convolutional mode, and requires a careful hardware-aware implementation to be efficient.
Even so, it is still less efficient than hardware-friendly models such as CNNs and Transformers because it does not leverage matrix multiplication units, which modern accelerators such as GPUs and TPUs are specialized for.

While time-invariant SSMs are closely related to continuous, recurrent, and convolutional sequence models,
they are not directly related to attention.
In this paper, we show a deeper relationship between selective SSMs and attention,
and use it to significantly improve the training speed of SSMs while simultaneously allowing for much larger state sizes 𝙽 \mathtt{N}.

Structured SSMs as Sequence Transformations.

Definition 2.1.

We use the term sequence transformation to refer to a parameterized map on sequences Y = f θ ​ ( X ) Y=f_{\theta}(X) where X, Y ∈ ℝ ( 𝚃, 𝙿 ) X,Y\in\mathbb{R}^{\mathtt{(T,P)}} and θ \theta is an arbitrary collection of parameters.
𝚃 \mathtt{T} represents the sequence or time axis; subscripts index into the first dimension, e.g. X t, Y t ∈ ℝ 𝙿 X_{t},Y_{t}\in\mathbb{R}^{\mathtt{P}}.

Sequence transformations (e.g. SSMs, or self-attention) are the cornerstone of deep sequence models, where they are incorporated into neural network architectures (e.g. Transformers).
The SSM in ( 1 ) or ( 2 ) is a sequence transformation with 𝙿 = 1 \mathtt{P}=1; it can be generalized to 𝙿 > 1 \mathtt{P}>1 by simply broadcasting across this dimension (in other words, viewing the input as 𝙿 \mathtt{P} independent sequences and applying the SSM to each).
One can think of 𝙿 \mathtt{P} as a head dimension, which we will elaborate on in Section 7.

Definition 2.2.

We define the SSM operator
𝖲𝖲𝖬 ( A, B, C ) = 𝖲𝖲𝖬 ( A 0 : T, B 0 : T, C 0 : T ) \mathsf{SSM}(A,B,C)=\mathsf{SSM}(A_{0:T},B_{0:T},C_{0:T}) as the sequence transformation X ∈ ℝ ( 𝚃, 𝙿 ) ↦ Y ∈ ℝ ( 𝚃, 𝙿 ) X\in\mathbb{R}^{\mathtt{(T,P)}}\mapsto Y\in\mathbb{R}^{\mathtt{(T,P)}}
defined by equation ( 2 ).

In SSMs, the 𝙽 \mathtt{N} dimension is a free parameter called the state size or state dimension.
We also call it the state expansion factor, because it expands the size of the input/output by a factor of N N, with implications for the computational efficiency of these models.

Finally, we remark that many types of sequence transformations, such as attention, can be represented as a single matrix multiplication across the sequence dimension.

Definition 2.3.

We call a sequence transformation Y = f θ ​ ( X ) Y=f_{\theta}(X) a matrix transformation if it can be written in the form Y = M θ ​ X Y=M_{\theta}X where M M is a matrix depending on the parameters θ \theta.
We identify the sequence transformation with the matrix M M, and often drop the dependence on θ \theta when clear from context.

2.2 Attention

Attention broadly refers to a type of computation that assigns scores to every pair of positions in a sequence, allowing each element to “attend” to the rest.
By far the most common and important variant of attention is softmax self-attention, which can be defined as

Y = softmax ( Q K ⊤ ) ⋅ V \displaystyle Y=\operatorname*{softmax}(QK^{\top})\cdot V

for Q, K, V ∈ ℝ ( 𝚃, 𝙿 ) Q,K,V\in\mathbb{R}^{\mathtt{(T,P)}}.
The mechanism of pairwise comparisons (induced by materializing Q ​ K ⊤ QK^{\top} ) leads to the characteristic quadratic training cost of attention.

Many variants of attention have been proposed, but all share the underlying core of these attention scores, with various approximations ( Tay et al. 2022 ).
The most important variant for this work is linear attention ( Katharopoulos et al. 2020 ).
Roughly speaking, this family of methods drops the softmax by folding it into a kernel feature map, and uses associativity of matrix multiplication to rewrite
( Q ​ K ⊤ ) ⋅ V = Q ⋅ ( K ⊤ ​ V ) (QK^{\top})\cdot V=Q\cdot(K^{\top}V).
Moreover, in the important case of causal (autoregressive) attention,
they show that when the causal mask is incorporated into the left-hand side as
( L ∘ Q ​ K ⊤ ) ⋅ V (L\circ QK^{\top})\cdot V, where L L is the lower-triangular 1’s matrix,
then the right-hand side can be expanded as a recurrence.
Several recent and concurrent works
such as RetNet ( Sun et al. 2023 ) and GateLoop ( Katsch 2023 ) strengthen this to more general forms of L L ( Section 10 ).
In this work, our formulation of structured masked attention will strongly generalize these ideas.

2.3 Structured Matrices

General matrices M ∈ ℝ ( 𝚃, 𝚃 ) M\in\mathbb{R}^{\mathtt{(T,T)}} require 𝚃 2 \mathtt{T}^{2} parameters to represent and O ⁡ ( 𝚃 2 ) O(\mathtt{T}^{2}) time to perform basic operations such as matrix-vector multiplication.
Structured matrices are those that

(i)

can be represented in subquadratic (ideally linear) parameters through a compressed representation, and

(ii)

have fast algorithms (most importantly matrix multiplication) by operating directly on this compressed representation.

Perhaps the most canonical families of structured matrices are sparse and low-rank matrices.
However, there exist many other families, such as Toeplitz, Cauchy, Vandermonde, and butterfly matrices, which have all been used in machine learning for efficient models ( Thomas et al. 2018; Dao et al. 2019; Gu et al. 2022a; Fu et al. 2024 ).
Structured matrices are a powerful abstraction for efficient representations and algorithms.
In this work, we will show that SSMs are equivalent to another class of structured matrices that have not previously been used in deep learning, and use this connection to derive efficient methods and algorithms.

2.4 Overview: Structured State Space Duality

While this paper develops a much richer framework of connections between SSMs, attention, and structured matrices, we provide a brief summary of the main method, which is actually quite self-contained and simple algorithmically.

Recurrent (Linear) Form.

The state space dual (SSD) layer can be defined as a special case of the selective SSM ( 2 ).
The standard computation of an SSM as a recurrence (or parallel scan) can be applied, which has linear complexity in sequence length.
Compared to the version used in Mamba, SSD has two minor differences:

•

The structure on A A is further simplified from diagonal to scalar times identity structure.
Each A t A_{t} can also be identified with just a scalar in this case.

•

We use a larger head dimension 𝙿 \mathtt{P}, compared to 𝙿 = 1 \mathtt{P}=1 used in Mamba. Typically 𝙿 = { 64,128 } \mathtt{P}=\{64,128\} is chosen which is similar to conventions for modern Transformers.

Compared to the original selective SSM, these changes can be viewed as slightly decreasing the expressive power in return for significant training efficiency improvements.
In particular, our new algorithms will allow the use of matrix multiplication units on modern accelerators.

Dual (Quadratic) Form.

The dual form of SSD is a quadratic computation closely related to attention, defined as

( L ∘ Q ​ K ⊤ ) ⋅ V L i ​ j = { a i × ⋯ × a j + 1 i ≥ j 0 i < j \displaystyle(L\circ QK^{\top})\cdot V\qquad L_{ij}=\begin{cases}a_{i}\times\dots\times a_{j+1}&i\geq j\\
0&i<j\end{cases}

where a i a_{i} are input-dependent scalars bounded in [ 0, 1 ] [0,1].

Compared to standard softmax attention, there are two main differences

•

The softmax is dropped.

•

The attention matrix is multiplied elementwise-wise by an additional mask matrix L L.

Both of these changes can be viewed as addressing problems in vanilla attention.
For example, the softmax has been recently observed to cause problems in attention scores, such as the “attention sink” phenomenon ( Xiao et al. 2024; Darcet et al. 2024 ).
More importantly, the mask matrix L L can be viewed as replacing the heuristic positional embeddings of Transformers with a different data-dependent positional mask that controls how much information is transfered across time.

More broadly, this form is an instance of our structured masked attention generalization of linear attention, defined in Section 4.

Matrix Form and SSD Algorithm.

The various forms of SSD are connected through a unified matrix representation,
by showing that SSMs have a matrix transformation form
Y = M ​ X Y=MX for a matrix M θ ∈ ℝ ( 𝚃, 𝚃 ) M_{\theta}\in\mathbb{R}^{\mathtt{(T,T)}} that depends on θ = ( A, B, C ) \theta=(A,B,C).
In particular, the dual form of SSD is equivalent to naive (quadratic-time) multiplication by the matrix M M,
and the recurrent form is a particular efficient (linear-time) algorithm that leverages the structure in M M.

Going beyond these, any algorithm for multiplication by M M can be applied.
Our proposed hardware-efficient SSD algorithm ( Section 6 ) is a new structured matrix multiplication method that involves block decompositions of M M, which obtains better efficiency tradeoffs than either the pure linear or quadratic forms.
It is relatively simple and easy-to-implement compared to general selective SSMs ( Gu & Dao 2023 );
Section 6.2 provides a complete implementation in a few lines of code.

Figure 1 provides a simple roadmap of the relationships between the concepts presented in this paper.

2.5 Notation

Throughout this paper, we prefer using precise notation that can be mapped to code.

Matrices and Vectors.

We generally use lower case to denote vectors (i.e. tensors with a single axis)
and upper case to denote matrices (i.e. tensors with more than one axes).
We do not bold matrices in this work.
Sometimes, if a matrix is tied or repeated along one axis (and hence can also be viewed as a vector),
we may use either upper or lower case for it. 2 2
2

In this work, this happens only with the A A parameter of SSMs.
⋅ \cdot denotes scalar or matrix multiplication while ∘ \circ denotes Hadamard (elementwise) multiplication.

Indexing.

We use Python-style indexing, e.g. i : j i:j refers to the range ( i, i + 1, …, j − 1 ) (i,i+1,\dots,j-1) when i < j i<j and ( i, i − 1, …, j + 1 ) (i,i-1,\dots,j+1) when i > j i>j.
For example, for any symbol v v we let v j : i v_{j:i} for j ≥ i j\geq i denote the sequence ( v j, …, v i + 1 ) (v_{j},\dots,v_{i+1}).
[ i ] [i] is equivalent to 0 : i = ( 0, …, i − 1 ) 0:i=(0,\dots,i-1).
For shorthand, we also let v j : i × v_{j:i}^{\times} denote the product v j × ⋯ × v i + 1 v_{j}\times\dots\times v_{i+1}. 3 3
3

In some contexts, it is always clear that the notation a i : j a_{i:j} or A i : j A_{i:j} means a i : j × a_{i:j}^{\times}, and the superscript is omitted.

Dimensions.

To distinguish from matrices and tensors, we often use capital letters in typewriter fonts (e.g. OPEN 𝙳, 𝙽, 𝚃 ) \mathtt{D},\mathtt{N},\mathtt{T}) to denote dimensions and tensor shapes.
Instead of the traditional notation M ∈ ℝ T × T M\in\mathbb{R}^{T\times T} we frequently use M ∈ ℝ ( 𝚃, 𝚃 ) M\in\mathbb{R}^{\mathtt{(T,T)}} to reflect tensor shapes in code.

Tensor Contractions.

We will heavily rely on tensor contraction or einsum notation both for clarity and as a central tool in stating and proving our results.
We assume the reader to be familiar with this notation,
which is commonly used in modern tensor libraries such as numpy.
For example, we can use 𝖼𝗈𝗇𝗍𝗋𝖺𝖼𝗍 ( 𝙼𝙽, 𝙽𝙺 → 𝙼𝙺 ) \mathsf{contract}(\mathtt{MN,NK\to MK}) to denote the matrix-matrix multiplication operator, and in our notation 𝖼𝗈𝗇𝗍𝗋𝖺𝖼𝗍 ( 𝙼𝙽, 𝙽𝙺 → 𝙼𝙺 ) ( X, Y ) \mathsf{contract}(\mathtt{MN,NK\to MK})(X,Y) (which is equivalent to X ⋅ Y X\cdot Y ) can be translated to code as 𝚗𝚞𝚖𝚙𝚢. 𝚎𝚒𝚗𝚜𝚞𝚖 ( ′ 𝚖𝚗, 𝚗𝚔 → 𝚖𝚔 ′, 𝚇, 𝚈 ) \mathtt{numpy.einsum(^{\prime}mn,nk\to mk^{\prime},X,Y)}.

A large glossary of notation is included in Appendix A.

3 State Space Models are Structured Matrices

This section explores different perspectives of the state space model as a sequence transformation, and outlines properties and algorithms of such maps.
The main results of this section are about the equivalence between state space models and a family of structured matrices called semiseparable matrices,
which imply new efficiency results ( Theorems 3.5 and 3.7 ).

3.1 The Matrix Transformation Form of State Space Models

Recall that our definition of an SSM is defined as a parameterized map
defined through ( 2 ).
Our theoretical framework starts by simply writing this transformation as a matrix multiplication mapping the vectors x ∈ ℝ 𝚃 ↦ y ∈ ℝ 𝚃 x\in\mathbb{R}^{\mathtt{T}}\mapsto y\in\mathbb{R}^{\mathtt{T}}.

By definition, h 0 = B 0 ​ x 0 h_{0}=B_{0}x_{0}.
By induction,

h t \displaystyle h_{t}

= A t ​ … ​ A 1 ​ B 0 ​ x 0 + A t ​ … ​ A 2 ​ B 1 ​ x 1 + ⋯ + A t ​ A t − 1 ​ B t − 2 ​ x t − 2 + A t ​ B t − 1 ​ x t − 1 + B t ​ x t \displaystyle=A_{t}\dots A_{1}B_{0}x_{0}+A_{t}\dots A_{2}B_{1}x_{1}+\dots+A_{t}A_{t-1}B_{t-2}x_{t-2}+A_{t}B_{t-1}x_{t-1}+B_{t}x_{t}

= ∑ s = 0 t A t : s × B s x s. \displaystyle=\sum_{s=0}^{t}A_{t:s}^{\times}B_{s}x_{s}.

Multiplying by C t C_{t} to produce y t y_{t} and vectorizing the equation over t ∈ [ 𝚃 ] t\in[\mathtt{T}],
we derive the matrix transformation form of SSMs.

y t \displaystyle y_{t}

= ∑ s = 0 t C t ⊤ A t : s × B s x s \displaystyle=\sum_{s=0}^{t}C_{t}^{\top}A_{t:s}^{\times}B_{s}x_{s}

(3)

y \displaystyle y

= 𝖲𝖲𝖬 ⁡ ( A, B, C ) ​ ( x ) = M ​ x \displaystyle=\mathsf{SSM}(A,B,C)(x)=Mx

M j ​ i \displaystyle M_{ji}

≔ C j ⊤ A j ⋯ A i + 1 B i \displaystyle\coloneqq C_{j}^{\top}A_{j}\cdots A_{i+1}B_{i}

3.2 Semiseparable Matrices

M M in equation ( 3 ) is a particular representation of a class of matrices known as semiseparable matrices.
Semiseparable matrices are a fundamental matrix structure.
We first define these matrices and their properties.

Definition 3.1.

A (lower triangular) matrix M M is 𝙽 \mathtt{N} -semiseparable if every submatrix contained in the lower triangular portion (i.e. on or below the diagonal) has rank at most 𝙽 \mathtt{N}.
We call 𝙽 \mathtt{N} the order or rank of the semiseparable matrix.

Definition 3.1, and other forms of related “separable” structure (e.g. quasiseparable matrices and other definitions of semiseparable matrices) are sometimes called structured rank matrices (or rank-structured matrices) because they are characterized by rank conditions on their submatrices.
Semiseparable matrices have many structured representations including the hierarchical semiseparable (HSS),
sequential semiseparable (SSS), and Bruhat forms ( Pernet & Storjohann 2018 ).
We will primarily use the SSS form.

3.2.1 The Sequentially Semiseparable (SSS) Representation

Definition 3.2.

A lower triangular matrix M ∈ ℝ ( 𝚃, 𝚃 ) M\in\mathbb{R}^{\mathtt{(T,T)}} has a 𝙽 \mathtt{N} - sequentially semiseparable (SSS) representation if it can be written in the form

M j ​ i = C j ⊤ A j ⋯ A i + 1 B i M_{ji}=C_{j}^{\top}A_{j}\cdots A_{i+1}B_{i}

(4)

for vectors B 0, …, B 𝚃 − 1, C 0, …, C 𝚃 − 1 ∈ ℝ 𝙽 B_{0},\dots,B_{\mathtt{T}-1},C_{0},\dots,C_{\mathtt{T}-1}\in\mathbb{R}^{\mathtt{N}}
and matrices A 0, …, A 𝚃 − 1 ∈ ℝ ( 𝙽, 𝙽 ) A_{0},\dots,A_{\mathtt{T}-1}\in\mathbb{R}^{\mathtt{(N,N)}}.

We define the operator 𝖲𝖲𝖲 \mathsf{SSS} so that M = 𝖲𝖲𝖲 ( A 0 : 𝚃, B 0 : 𝚃, C 0 : 𝚃 ) M=\mathsf{SSS}(A_{0:\mathtt{T}},B_{0:\mathtt{T}},C_{0:\mathtt{T}}).

A fundamental result of semiseparable matrices is that they are exactly equivalent to matrices with SSS representations.
One direction can be deduced with a simple constructive proof.

Lemma 3.3.

An 𝙽 \mathtt{N} -SSS matrix M M with representation ( 4 ) is 𝙽 \mathtt{N} -semiseparable.

Proof.

Consider any off-diagonal block M j : j ′, i ′ : i M_{j:j^{\prime},i^{\prime}:i} where j ′ > j ≥ i > i ′ j^{\prime}>j\geq i>i^{\prime}.
This has an explicit rank- 𝙽 \mathtt{N} factorization as

[ C j ⊤ A j : i ′ × B i ′ … C j ⊤ A j : i − 1 × B i − 1 C j ′ − 1 ⊤ A j ′ − 1 : i ′ × B i ′ … C j ′ − 1 ⊤ A j ′ − 1 : i − 1 × B i − 1 ] = [ C j ⊤ A j : j × C j ′ − 1 ⊤ A j ′ − 1 : j × ] A j : i − 1 × [ A i − 1 : i ′ × B i ′ ⋯ A i − 1 : i − 1 × B i − 1 ]. \begin{bmatrix}C_{j}^{\top}A_{j:i^{\prime}}^{\times}B_{i^{\prime}}&\dots&C_{j}^{\top}A_{j:i-1}^{\times}B_{i-1}\\
\vdots&&\vdots\\
C_{j^{\prime}-1}^{\top}A_{j^{\prime}-1:i^{\prime}}^{\times}B_{i^{\prime}}&\dots&C_{j^{\prime}-1}^{\top}A_{j^{\prime}-1:i-1}^{\times}B_{i-1}\\
\end{bmatrix}=\begin{bmatrix}C_{j}^{\top}A_{j:j}^{\times}\\
\vdots\\
C_{j^{\prime}-1}^{\top}A_{j^{\prime}-1:j}^{\times}\end{bmatrix}A_{j:i-1}^{\times}\begin{bmatrix}A_{i-1:i^{\prime}}^{\times}B_{i^{\prime}}&\cdots&A_{i-1:i-1}^{\times}B_{i-1}\end{bmatrix}.

(5)

∎

Equation ( 5 ) will be used extensively in deriving our fast algorithms for sequence models.
The other direction is well-established in the literature on semiseparable matrices.

Proposition 3.4.

Every 𝙽 \mathtt{N} -semiseparable matrix has a 𝙽 \mathtt{N} -SSS representation.

Furthermore, note that although Definition 3.2 involves O ⁡ ( 𝙽 2 ​ 𝚃 ) O(\mathtt{N}^{2}\mathtt{T}) parameters for the representation (in particular to store the A A matrices),
it can actually be compressed down to O ⁡ ( 𝙽𝚃 ) O(\mathtt{NT}) parameters, which is asymptotically tight ( Pernet et al. 2023 ).
Therefore in the rest of this paper we will conflate the structured matrix class ( Definition 3.1 ) and a particular representation of it ( Definition 3.2 ); we will always use this representation instead of other candidates.
In turn we will use 𝙽 \mathtt{N} -SS to refer to an 𝙽 \mathtt{N} -semiseparable matrix in SSS form.

Semiseparable matrices are a fundamental matrix structure and have many important properties.
They are deeply related to recurrences at large, and can be defined by multiple characterizations (e.g. Definitions 3.1 and 3.2 ) which reveal different connections and efficient algorithms for them.
We mention some of their other properties in Section C.1.

Remark 2.

The notion of semiseparability is very broad and many similar but subtlely different definitions appear in the literature;
our definitions may differ slightly from other conventions.
First, because we are primarily concerned with causal or autoregressive settings in this paper,
we have restricted the definition of semiseparability to the triangular case;
Definition 3.1 more formally might be called ( 𝙽, 0 ) (\mathtt{N},0) -semiseparability by some authors.
Some authors may also instead refer to it as a form of quasiseparability ( Eidelman & Gohberg 1999; Pernet 2016 ).
See Vandebril et al. 2005 for a brief survey.

3.2.2 1-Semiseparable Matrices: the Scalar SSM Recurrence

We will single out the special case of 1 1 -SS matrices.
Note that in this case, the C j C_{j} and B i B_{i} are scalars, and can be factored out of the SSS representation ( 4 )
(we also use lower-case to emphasize that the parameters are scalars in this case)

𝖲𝖲𝖲 ( a, b, c ) = 𝖽𝗂𝖺𝗀 ( c ) ⋅ M ⋅ 𝖽𝗂𝖺𝗀 ( b ) where M j ​ i = a j : i ×. \displaystyle\mathsf{SSS}(a,b,c)=\mathsf{diag}(c)\cdot M\cdot\mathsf{diag}(b)\qquad\text{where}\qquad M_{ji}=a_{j:i}^{\times}.

Since diagonal matrices are easy to handle (e.g. multiplication by a diagonal matrix is the same as elementwise scalar multiplication),
we can ignore these terms.
Thus our basic representation of a 1-SS matrix is M j ​ i = a j : i M_{ji}=a_{j:i} or

M = 𝟣 𝖲 𝖲 ( a 0 : T ) ≔ [ 1 a 1 1 a 2 ​ a 1 a 2 1 ⋱ ⋱ a T − 1 ​ … ​ a 1 a T − 1 ​ … ​ a 2 … a T − 1 1 ]. M=\mathsf{1SS}(a_{0:T})\coloneqq\begin{bmatrix}1&\\
a_{1}&1&\\
a_{2}a_{1}&a_{2}&1\\
\vdots&\vdots&\ddots&\ddots\\
a_{T-1}\dots a_{1}&a_{T-1}\dots a_{2}&\dots&a_{T-1}&1\\
\end{bmatrix}.

(6)

The importance of 1-SS matrices lies in their equivalence to the minimal form of a scalar recurrence – the case of a degenerate SSM with state dimension 𝙽 = 1 \mathtt{N}=1 and no ( B, C ) (B,C) projections.
Note that multiplication y = M ​ x y=Mx can be computed by the recurrence

y t \displaystyle y_{t}

= a t : 0 x 0 + ⋯ + a t : t x t \displaystyle=a_{t:0}x_{0}+\dots+a_{t:t}x_{t}

(7)

= a t ( a t − 1 : 0 x 0 + ⋯ + a t − 1 : t − 1 x t − 1 ) + a t : t x t \displaystyle=a_{t}\left(a_{t-1:0}x_{0}+\dots+a_{t-1:t-1}x_{t-1}\right)+a_{t:t}x_{t}

= a t ​ y t − 1 + x t. \displaystyle=a_{t}y_{t-1}+x_{t}.

We thus also refer to matrix multiplication by 1 1 -SS matrices as the scalar SSM recurrence or the cumprodsum (cumulative product sum; a generalization of cumulative product and cumulative sum) operator.
As the fundamental form of recurrence, multiplication by 1-SS matrices is important
as a building block for our main algorithms.

We emphasize that one of the central themes of this paper is that many algorithms on sequence models can be reduced to structured matrix multiplication algorithms.
1-SS matrices exemplify this connection: there are many fast algorithms for computing the primitive scalar recurrence or cumprodsum operator,
and all of them turn out to be equivalent to different structured factorization of 1-SS matrices.
We dedicate Appendix B to these algorithms for 1-SS matrix multiplication.

3.3 State Space Models are Semiseparable Matrices

Recall that our definition of an SSM is defined as a parameterized map
defined through Definition 2.1.
The connection between SSMs and semiseparable matrices follows from simply writing this transformation as a matrix multiplication mapping the vectors x ↦ y ∈ ℝ 𝚃 x\mapsto y\in\mathbb{R}^{\mathtt{T}}.

Equation ( 3 ) directly establishes the link between state space models and the sequentially semiseparable representation, which in turn are equivalent to semiseparable matrices in general ( Lemmas 3.3 and 3.4 ).

Theorem 3.5.

The state space model transformation y = 𝖲𝖲𝖬 ​ ( A, B, C ) ​ ( x ) y=\mathsf{SSM}(A,B,C)(x) with state size 𝙽 \mathtt{N} is identical to matrix multiplication by an 𝙽 \mathtt{N} -SS matrix in sequentially semiseparable representation y = 𝖲𝖲𝖲 ⁡ ( A, B, C ) ⋅ x y=\mathsf{SSS}(A,B,C)\cdot x.

In other words the sequence transformation operator 𝖲𝖲𝖬 \mathsf{SSM} ( Definition 2.2 ) coincides with the matrix construction operator 𝖲𝖲𝖲 \mathsf{SSS} ( Definition 3.2 ),
and we use them interchangeably (or sometimes 𝖲𝖲 \mathsf{SS} as shorthand).
Furthermore—by a twist of fate—structured state space models and sequentially semiseparable matrices have the same acronyms, underscoring their equivalence!
Conveniently we can use any of these acronyms SSM (state space model or semiseparable matrix), SSS (structured state space or sequentially semiseparable), or SS (state space or semiseparable) interchangeably to unambiguously refer to either concept.
However, we will generally use the convention that SSM refers to state space model, SS refers to semiseparable, and SSS refers to sequentially semiseparable.

Figure 2 illustrates the sequence transformation perspective of state space models as semiseparable matrices.

Figure 2 :
( State Space Models are Semiseparable Matrices.)
As sequence transformations, state space models can be represented as a matrix transformation M ∈ ℝ ( 𝚃, 𝚃 ) M\in\mathbb{R}^{\mathtt{(T,T)}} acting on the sequence dimension 𝚃 \mathtt{T},
sharing the same matrix for each channel in a head ( Left ).
This matrix is a semiseparable matrix ( Right ), which is a rank-structured matrix where every submatrix contained on-and-below the diagonal ( Blue ) has rank at most 𝙽 \mathtt{N},
equal to the SSM’s state dimension.

3.4 Computing State Space Models through Structured Matrix Algorithms

The reason Theorem 3.5 is important is that it will allow us to reduce the problem of efficient computation of SSMs (and other sequence models) into efficient algorithms for structured matrix multiplication.
We briefly provide an overview and defer our main new algorithm to Section 6, after showing the equivalence of SSMs to other sequence models in
Sections 4 and 5.

As previously defined, semiseparable matrices (i.e. rank-structured matrices) are a classical type of structured matrix:

(i)

They have compressed representations such as the SSS form which has only O ⁡ ( 𝚃 ) O(\mathtt{T}) instead of O ⁡ ( 𝚃 2 ) O(\mathtt{T}^{2}) parameters.

(ii)

They have fast algorithms operating directly on the compressed representation.

Furthermore, the parameterization and matrix multiplication cost can be tight in the semiseparable order.

Proposition 3.6 ( Pernet et al. 2023 ).

An 𝙽 \mathtt{N} -SS matrix of size 𝚃 \mathtt{T} can be represented in O ⁡ ( 𝙽𝚃 ) O(\mathtt{NT}) parameters and has matrix-vector multiplication in time and space O ⁡ ( 𝙽𝚃 ) O(\mathtt{NT}).

For example, 1-SS matrices illustrate the essence of this connection.
The matrix M = 𝟣 ​ 𝖲 ​ 𝖲 ​ ( a ) M=\mathsf{1SS}(a) is defined by exactly 𝚃 − 1 \mathtt{T}-1 parameters a 0 : 𝚃 − 1 = a 1, …, a 𝚃 − 1 a_{0:\mathtt{T}-1}=a_{1},\dots,a_{\mathtt{T}-1},
and can be computed in O ⁡ ( 𝚃 ) O(\mathtt{T}) time by following the scalar recurrence ( 7 ).

3.4.1 The Linear (Recurrent) Mode

Proposition 3.6 can be easily seen in the case of diagonal structured SSMs (S4D ( Gu et al. 2022a ) ),
simply by leveraging the state space model formulation ( 2 ) and unrolling the recurrence.
We provide the formal tensor-contraction algorithm in ( 8 ), where the dimension 𝚂 \mathtt{S} is equal to 𝚃 \mathtt{T} 4 4
4

A different symbol is required for the contraction notation..

Z \displaystyle Z

= 𝖼𝗈𝗇𝗍𝗋𝖺𝖼𝗍 ( 𝚂𝙿, 𝚂𝙽 → 𝚂𝙿𝙽 ) ( X, B ) \displaystyle=\mathsf{contract}(\mathtt{SP},\mathtt{SN}\to\mathtt{SPN})(X,B)

( 𝚂, 𝙿, 𝙽 ) \displaystyle\mathtt{(S,P,N)}

(8a)

H \displaystyle H

= 𝖼𝗈𝗇𝗍𝗋𝖺𝖼𝗍 ( 𝚃𝚂𝙽, 𝚂𝙿𝙽 → 𝚃𝙿𝙽 ) ( L, Z ) \displaystyle=\mathsf{contract}(\mathtt{TSN},\mathtt{SPN}\to\mathtt{TPN})(L,Z)

( 𝚃, 𝙿, 𝙽 ) \displaystyle\mathtt{(T,P,N)}

(8b)

Y \displaystyle Y

= 𝖼𝗈𝗇𝗍𝗋𝖺𝖼𝗍 ( 𝚃𝙽, 𝚃𝙿𝙽 → 𝚃𝙿 ) ( C, H ) \displaystyle=\mathsf{contract}(\mathtt{TN},\mathtt{TPN}\to\mathtt{TP})(C,H)

( 𝚃, 𝙿 ) \displaystyle\mathtt{(T,P)}

(8c)

Here, L ∈ ℝ ( 𝚃, 𝚃 ) L\in\mathbb{R}^{(\mathtt{T},\mathtt{T})} is defined as 𝟣 ​ 𝖲 ​ 𝖲 ​ ( A ) \mathsf{1SS}(A), or in other words L 0 : 𝚃, 0 : 𝚃 = 𝟣 𝖲 𝖲 ( A 0 : 𝚃 ) L_{0:\mathtt{T},0:\mathtt{T}}=\mathsf{1SS}(A_{0:\mathtt{T}}) for i ∈ [ 𝙽 ] i\in[\mathtt{N}].
This algorithm involves three steps corresponding to ( 2 ):

(i)

expanding the input X X by the input matrix B B ( 8a ),

(ii)

unrolling independent scalar SSM recurrences ( 8b ), and

(iii)

contracting the hidden state H H by the output matrix C C ( 8c ).

Note that we have used the equivalence between scalar SSMs and 1-SS matrices in step ( 8b ).

Remark 3.

We note that ( 8 ) is a special case of the Mamba (S6) model.
however, a naive implementation is slow because of the expanded tensors Z Z and H H of size ( 𝚃, 𝙿, 𝙽 ) \mathtt{(T,P,N)};
Gu & Dao 2023 introduced a hardware-aware implementation to avoid materializing these tensors.

Surprisingly, Theorem 3.5 and Proposition 3.6 immediately imply that all SSMs have the same asymptotic efficiency as algorithm ( 8 ).

Theorem 3.7.

Any state space model ( Definition 2.2 ) of state size 𝙽 \mathtt{N} on sequence length 𝚃 \mathtt{T} can be computed in time O ⁡ ( 𝚃𝙽 ) O(\mathtt{TN}) (not accounting for potential preprocessing).

We note that this result is new to the structured SSM literature.
In particular, given dense unstructured A t A_{t} matrices, the total representation alone seems to be of size O ⁡ ( 𝚃𝙽 2 ) O(\mathtt{TN}^{2}).
Thus Theorem 3.7 states the non-trivial result that with a pre-processing step, even an unstructured SSM can be computed optimally efficiently,
with upper bound matching the lower bound O ⁡ ( 𝚃𝙽 ) O(\mathtt{TN}) given by the size of B B and C C.

Remark 4.

Theorem 3.7 is perhaps not too surprising in light of the fact that almost all dense matrices over ℝ ( 𝙽, 𝙽 ) \mathbb{R}^{\mathtt{(N,N)}} are diagonalizable over ℂ \mathbb{C},
leading to the result that almost all dense real SSMs are equivalent to a diagonal complex SSM.
This fact underlies the reason why diagonal SSMs are the most popular form of structured SSM ( Gupta et al. 2022; Gu et al. 2022a; Smith et al. 2023 ).
However, Theorem 3.7 implies the much stronger result for all real SSMs (not just the diagonalizable ones), as well as dense SSMs over other fields (including ℂ \mathbb{C} itself).

In practice, efficiently computable SSMs still require additional structure on A A,
particularly to avoid the expensive preprocessing step (which both has order 𝙽 \mathtt{N} extra FLOPs and involves hardware-inefficient operations such as singular value decompositions).
These structures are the focus of past work on structured SSMs (e.g. S4(D) and Mamba) as well as our new algorithms.
In particular, when slightly stronger structure is imposed on A A, we will design very hardware-efficient algorithms through block decompositions of the SSM matrix M = 𝖲𝖲𝖲 ⁡ ( A, B, C ) M=\mathsf{SSS}(A,B,C) in Section 6.

3.4.2 The Quadratic (Naive) Mode

We note that there is another way to compute an SSM exposed by our new matrix point of view.
A naive computation of the matrix SSM representation ( 3 ) involves simply materializing the sequence transformation matrix M = 𝖲𝖲𝖲 ⁡ ( A, B, C ) M=\mathsf{SSS}(A,B,C).
This is a ( 𝚃, 𝚃 ) \mathtt{(T,T)} matrix, and therefore this naive algorithm will scale quadratically in sequence length.
However, when the sequence length 𝚃 \mathtt{T} is short, this can actually be more efficient than the linear algorithm due to constant factors and the hardware-friendliness of the computation pattern (e.g. leveraging matrix-matrix multiplications).
In fact, for a particular case of structured SSMs, this looks very similar to a quadratic attention computation ( Section 5 ).

3.4.3 Summary

Many sequence models are explicitly motivated or defined as matrix sequence transformations –
most notably Transformers, where the matrix mixer is the attention matrix.
On the other hand, RNNs and SSMs have not previously been described in this way.
By providing an explicit matrix transformation form of state space models,
we reveal new ways of understanding and using them.
From a computational perspective, any method of computing the forward pass of a state space model can be
viewed as a matrix multiplication algorithm on semiseparable matrices.
The semiseparable matrix perspective provides one lens into state space duality (SSD),
where the dual modes respectively refer to a linear-time semiseparable matrix multiplication algorithm and quadratic-time naive matrix multiplication.

Moreover, leveraging the rich structure of semiseparable matrices can lea
