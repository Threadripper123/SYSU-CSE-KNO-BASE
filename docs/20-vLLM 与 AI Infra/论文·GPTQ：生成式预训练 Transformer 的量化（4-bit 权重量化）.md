# 论文·GPTQ：生成式预训练 Transformer 的量化（4-bit 权重量化）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　461 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2210.17323
抓取: https://ar5iv.labs.arxiv.org/html/2210.17323

GPTQ: Accurate Post-Training Quantization
for Generative Pre-trained Transformers

Elias Frantar

Thanks: Corresponding author: elias.frantar@ist.ac.at

Affiliation: IST Austria

  
Saleh Ashkboos

Affiliation: ETH Zurich

  
Torsten Hoefler

Affiliation: ETH Zurich

  
Dan Alistarh

Affiliation: IST Austria & NeuralMagic

Abstract

Generative Pre-trained Transformer models, known as GPT or OPT, set themselves apart through breakthrough performance across complex language modelling tasks, but also by their extremely high computational and storage costs. Specifically, due to their massive size, even inference for large, highly-accurate GPT models may require multiple performant GPUs, which limits the usability of such models. While there is emerging work on relieving this pressure via model compression, the applicability and performance of existing compression techniques is limited by the scale and complexity of GPT models. In this paper, we address this challenge, and propose GPTQ, a new one-shot weight quantization method based on approximate second-order information, that is both highly-accurate and highly-efficient. Specifically, GPTQ can quantize GPT models with 175 billion parameters in approximately four GPU hours, reducing the bitwidth down to 3 or 4 bits per weight, with negligible accuracy degradation relative to the uncompressed baseline. Our method more than doubles the compression gains relative to previously-proposed one-shot quantization methods, preserving accuracy, allowing us for the first time to execute an 175 billion-parameter model inside a single GPU for generative inference.
Moreover, we also show that our method can still provide reasonable accuracy in the extreme quantization regime,
in which weights are quantized to 2-bit or even ternary quantization levels.
We show experimentally that these improvements can be leveraged for end-to-end inference speedups over FP16, of around 3.25x when using high-end GPUs (NVIDIA A100) and 4.5x when using more cost-effective ones (NVIDIA A6000). The implementation is available at https://github.com/IST-DASLab/gptq.

1 Introduction

Pre-trained generative models from the Transformer ( Vaswani et al. 2017 ) family, commonly known as GPT or OPT ( Radford et al. 2019; Brown et al. 2020; Zhang et al. 2022 ), have shown breakthrough performance for complex language modelling tasks, leading to massive academic and practical interest. One major obstacle to their usability is computational and storage cost, which ranks among the highest for known models. For instance, the best-performing model variants, e.g. GPT3-175B, have in the order of 175 billion parameters and require tens-to-hundreds of GPU years to train ( Zhang et al. 2022 ). Even the simpler task of inferencing over a pre-trained model, which is our focus in this paper, is highly challenging: for instance, the parameters of GPT3-175B occupy 326GB (counting in multiples of 1024) of memory when stored in a compact float16 format. This exceeds the capacity of even the highest-end single GPUs, and thus inference must be performed using more complex and expensive setups, such as multi-GPU deployments.

Although a standard approach to eliminating these overheads is model compression, e.g. ( Hoefler et al. 2021; Gholami et al. 2021 ), surprisingly little is known about compressing such models for inference.
One reason is that more complex methods for low-bitwidth quantization or model pruning usually require model retraining, which is extremely expensive for billion-parameter models.
Alternatively, post-training methods ( Nagel et al. 2020; Wang et al. 2020; Hubara et al. 2020; Nahshan et al. 2021 ), which compress the model in one shot, without retraining, would be very appealing.
Unfortunately, the more accurate variants of such methods ( Li et al. 2021; Hubara et al. 2021; Frantar et al. 2022 ) are complex and challenging to scale to billions of parameters ( Yao et al. 2022 ).
To date, only basic variants of round-to-nearest quantization ( Yao et al. 2022; Dettmers et al. 2022 ) have been applied at the scale of GPT-175B; while this works well for low compression targets, e.g., 8-bit weights, they fail to preserve accuracy at higher rates. It therefore remains open whether one-shot post-training quantization to higher compression rates is generally-feasible.

Figure 1: Quantizing OPT models to 4 and BLOOM models to 3 bit precision, comparing GPTQ with the FP16 baseline and round-to-nearest (RTN) ( Yao et al. 2022; Dettmers et al. 2022 ).

Contribution.

In this paper, we present a new post-training quantization method, called GPTQ, 1 1
1

This merges the name of the OPT model family with the abbreviation for post-training quantization (PTQ). which is efficient enough to execute on models with hundreds of billions of parameters in at most a few hours,
and precise enough to compress such models to 3 or 4 bits per parameter without significant loss of accuracy.
For illustration, GPTQ can quantize the largest publicly-available models, OPT-175B and BLOOM-176B, in approximately four GPU hours, with minimal increase in perplexity, known to be a very stringent accuracy metric.

Further, we show that our model can also provide robust results in the extreme quantization regime, in which models are quantized to 2 bits per component, or even ternary values.
On the practical side, we develop an execution harness which allows us to execute the resulting compressed models efficiently for generative tasks.
Specifically, we are able to run the compressed OPT-175B model for the first time on a single NVIDIA A100 GPU, or using only two more cost-effective NVIDIA A6000 GPUs. We also implement bespoke GPU kernels which are able to leverage compression for faster memory loading,
resulting in speedups of ≈ 3.25 × \approx 3.25\times when using A100 GPUs, and 4.5 × 4.5\times when using A6000 GPUs.

To our knowledge, we are the first to show that extremely accurate language models with hundreds of billions of parameters can be quantized to 3-4 bits/component: prior post-training methods only remain accurate at 8 bits ( Yao et al. 2022; Dettmers et al. 2022 ), while prior training-based techniques have only tackled models that are smaller by one to two orders of magnitude ( Wu et al. 2022 ). This high degree of compression may appear natural, as these networks are overparametrized; yet, as we discuss in our detailed analysis of results, compression induces non-trivial tradeoffs between the accuracy of the language modeling (perplexity), bit-width, and the size of the original model.

We hope that our work will stimulate further research in this area, and can be a further step towards making these models available to a wider audience.
In terms of limitations, our method currently does not provide speedups for the actual multiplications, due to the lack of hardware support for mixed-precision operands (e.g. FP16 x INT4) on mainstream architectures. Moreover, our current results do not include activation quantization, as they are not a significant bottleneck in our target scenarios; however, this can be supported using orthogonal techniques ( Yao et al. 2022 ).

2 Related Work

Quantization methods fall broadly into two categories: quantization during training, and post-training methods. The former quantize models during typically extensive retraining and/or finetuning, using some approximate differentiation mechanism for the rounding operation ( Gholami et al. 2021; Nagel et al. 2021 ). By contrast, post-training (“one-shot”) methods quantize a pretrained model using modest resources, typically a few thousand data samples and a few hours of computation. Post-training approaches are particularly interesting for massive models, for which full model training or even finetuning can be expensive.
We focus on this scenario here.

Post-training Quantization.

Most post-training methods have focused on vision models.
Usually, accurate methods operate by quantizing either individual layers, or small blocks of consecutive layers. (See Section 3 for more details.)
The AdaRound method ( Nagel et al. 2020 ) computes a data-dependent rounding by annealing a penalty term, which encourages weights to move towards grid points corresponding to quantization levels. BitSplit ( Wang et al. 2020 ) constructs quantized values bit-by-bit using a squared error objective on the residual error, while AdaQuant ( Hubara et al. 2021 ) performs direct optimization based on straight-through estimates.
BRECQ ( Li et al. 2021 ) introduces Fisher information into the objective, and optimizes layers within a single residual block jointly.
Finally, Optimal Brain Quantization (OBQ) ( Frantar et al. 2022 ) generalizes the classic Optimal Brain Surgeon (OBS) second-order weight pruning framework ( Hassibi et al. 1993; Singh & Alistarh 2020; Frantar et al. 2021 ) to apply to quantization.
OBQ quantizes weights one-by-one, in order of quantization error, always adjusting the remaining weights.
While these approaches can produce good results for models up to ≈ 100 \approx 100 million parameters in a few GPU hours, scaling them to networks orders of magnitude larger is challenging.

Large-model Quantization.

With the recent open-source releases of language models like BLOOM ( Laurençon et al. 2022 ) or OPT-175B ( Zhang et al. 2022 ), researchers have started to develop affordable methods for compressing such giant networks for inference.
While all existing works—ZeroQuant ( Yao et al. 2022 ), LLM.int8() ( Dettmers et al. 2022 ), and nuQmm ( Park et al. 2022 ) — carefully select quantization granularity, e.g., vector-wise, they ultimately just round weights to the nearest (RTN) quantization level, in order to maintain acceptable runtimes for very large models.
ZeroQuant further proposes layer-wise knowledge distillation, similar to AdaQuant, but the largest model it can apply this approach to has only 1.3 billion parameters. At this scale, ZeroQuant already takes ≈ 3 \approx 3 hours of compute; GPTQ quantizes models 100 × \times larger in ≈ 4 \approx 4 hours.
LLM.int8() observes that activation outliers in a few feature dimensions break the quantization of larger models, and proposes to fix this problem by keeping those dimensions in higher precision. Lastly, nuQmm develops efficient GPU kernels for a specific binary-coding based quantization scheme.

Relative to this line of work, we show that a significantly more complex and accurate quantizer can be implemented efficiently at large model scale.
Specifically, GPTQ more than doubles the amount of compression relative to these prior techniques, at similar accuracy.

3 Background

Layer-Wise Quantization.

At a high level, our method follows the structure of state-of-the-art post-training quantization methods ( Nagel et al. 2020; Wang et al. 2020; Hubara et al. 2021; Frantar et al. 2022 ), by performing quantization layer-by-layer, solving a corresponding reconstruction problem for each layer.
Concretely, let 𝐖 ℓ \mathbf{W_{\ell}} be the weights corresponding to a linear layer ℓ \ell and let 𝐗 ℓ \mathbf{X}_{\ell} denote the layer input corresponding to a small set of m m data points running through the network.
Then, the objective is to find a matrix of quantized weights 𝐖 ^ \mathbf{\widehat{W}} which minimizes the squared error, relative to the full precision layer output. Formally, this can be restated as

argmin 𝐖 ^ ​ ‖ 𝐖𝐗 − 𝐖 ^ ​ 𝐗 ‖ 2 2. \text{argmin}_{\mathbf{\widehat{W}}}\,||\mathbf{W}\mathbf{X}-\mathbf{\widehat{W}}\mathbf{X}||_{2}^{2}.

(1)

Further, similar to ( Nagel et al. 2020; Li et al. 2021; Frantar et al. 2022 ), we assume that the quantization grid for 𝐖 ^ \mathbf{\widehat{W}} is fixed before the process,
and that individual weights can move freely as in ( Hubara et al. 2021; Frantar et al. 2022 ).

Optimal Brain Quantization.

Our approach builds on the recently-proposed Optimal Brain Quanization (OBQ) method ( Frantar et al. 2022 ) for solving the layer-wise quantization problem defined above,
to which we perform a series of major modifications, which allow it to scale to large language models, providing more than three orders of magnitude computational speedup.
To aid understanding, we first briefly summarize the original OBQ method.

The OBQ method starts from the observation that Equation ( 1 ) can be written as the sum of the squared errors, over each row of 𝐖 \mathbf{W}.
Then, OBQ handles each row 𝐰 \mathbf{w} independently, quantizing one weight at a time while always updating all not-yet-quantized weights, in order to compensate for the error incurred by quantizing a single weight.
Since the corresponding objective is a quadratic, whose Hessian is 𝐇 F = 2 ​ 𝐗 F ​ 𝐗 F ⊤ \mathbf{H}_{F}=2\mathbf{X}_{F}\mathbf{X}_{F}^{\top}, where F F denotes the set of remaining full-precision weights, the greedy-optimal weight to quantize next, which we denote by w q w_{q}, and the corresponding optimal update of all weights in F F, denoted by 𝜹 F \bm{\delta}_{F}, are given by the following formulas, where quant ​ ( w ) \text{quant}(w) rounds w w to the nearest value on the quantization grid:

w q = argmin w q ( quant ​ ( w q ) − w q ) 2 [ 𝐇 F − 1 ] q ​ q, 𝜹 F = − w q − quant ​ ( w q ) [ 𝐇 F − 1 ] q ​ q ⋅ ( 𝐇 F − 1 ) :, q. w_{q}=\text{argmin}_{w_{q}}\,\frac{(\text{quant}(w_{q})-w_{q})^{2}}{[\mathbf{H}_{F}^{-1}]_{qq}},\quad\bm{\delta}_{F}=-\frac{w_{q}-\text{quant}(w_{q})}{[\mathbf{H}_{F}^{-1}]_{qq}}\cdot(\mathbf{H}_{F}^{-1})_{:,q}.

(2)

OBQ quantizes weights iteratively using these two equations, until all the weights of 𝐰 \mathbf{w} are quantized.
This is done efficiently, avoiding expensive full recomputations of 𝐇 − 1 \mathbf{H}^{-1}, by removing the q q th row and column of 𝐇 \mathbf{H}, which is necessary after quantizing w q w_{q}, directly in the inverse via one step of Gaussian elimination. Namely, the updated inverse is given by the formula

𝐇 − q − 1 = ( 𝐇 − 1 − 1 [ 𝐇 − 1 ] q ​ q 𝐇 :, q − 1 𝐇 q, : − 1 ) − p. \mathbf{H}_{-q}^{-1}=\Big(\mathbf{H}^{-1}-\frac{1}{[\mathbf{H}^{-1}]_{qq}}\mathbf{H}^{-1}_{:,q}\mathbf{H}^{-1}_{q,:}\Big)_{-p}.

(3)

This method comes with a vectorized implementation, handling multiple rows of 𝐖 \mathbf{W} in parallel.
Eventually, the algorithm can achieve reasonable runtimes on medium-sized models: for instance, it can fully quantize the ResNet-50 model (25M parameters) in ≈ 1 \approx 1 hour on a single GPU, which is roughly in line with other post-training methods achieving state-of-the-art accuracy ( Frantar et al. 2022 ).
However, the fact that OBQ’s runtime for a d row × d col d_{\text{row}}\times d_{\text{col}} matrix 𝐖 \mathbf{W} has cubic input dependency O ⁡ ( d row ⋅ d col 3 ) O(d_{\text{row}}\cdot d_{\text{col}}^{3}) means that applying it to models with billions of parameters is extremely expensive.

4 The GPTQ Algorithm

Step 1: Arbitrary Order Insight.

As explained in the previous section, OBQ quantizes weights in greedy order, i.e. it always picks the weight which currently incurs the least additional quantization error. Interestingly, we find that, while this quite natural strategy does indeed seem to perform very well, its improvement over quantizing the weights in arbitrary order is generally small, in particular on large, heavily-parametrized layers.
Most likely, this is because the slightly lower number of quantized weights with large individual error is balanced out by those weights being quantized towards the end of the process, when only few other unquantized weights that can be adjusted for compensation remain. As we will now discuss, this insight that any fixed order may perform well, especially on large models, has interesting ramifications.

Figure 2: GPTQ quantization procedure. Blocks of consecutive columns (bolded) are quantized at a given step, using the inverse Hessian information stored in the Cholesky decomposition, and the remaining weights (blue) are updated at the end of the step. The quantization procedure is applied recursively inside each block: the white middle column is currently being quantized.

The original OBQ method quantizes rows of 𝐖 \mathbf{W} independently, in a specific order defined by the corresponding errors.
By contrast, we will aim to quantize the weights of all rows in the same order, and will show that this typically yields results with a final squared error that is similar to the original solutions. As a consequence, the set of unquantized weights F F and similarly 𝐇 F − 1 \mathbf{H}_{F}^{-1} is always the same for all rows (see Figure 2 for an illustration).
In more detail, the latter is due to the fact that 𝐇 F \mathbf{H}_{F} depends only on the layer inputs 𝐗 F \mathbf{X}_{F}, which are the same for all rows, and not on any weights. Therefore, we have to perform the update of 𝐇 F − 1 \mathbf{H}_{F}^{-1} given by Equation ( 3 ) only d col d_{\text{col}} times, once per column, rather than d row ⋅ d col d_{\text{row}}\cdot d_{\text{col}} times, once per weight. This reduces the overall runtime from O ⁡ ( d row ⋅ d col 3 ) O(d_{\text{row}}\cdot d_{\text{col}}^{3}) to O ⁡ ( max ​ { d row ⋅ d col 2, d col 3 } ) O(\text{max}\,\{d_{\text{row}}\cdot d_{\text{col}}^{2},d_{\text{col}}^{3}\}), i.e., by a factor of min ​ { d row, d col } \text{min}\,\{d_{\text{row}},d_{\text{col}}\}.
For larger models, this difference consists of several orders of magnitude.
However, before this algorithm can actually be applied to very large models in practice, two additional major problems need to be addressed.

Step 2: Lazy Batch-Updates.

First, a direct implementation of the scheme described previously will not be fast in practice, because the algorithm has a relatively low compute-to-memory-access ratio. For example, Equation ( 3 ) needs to update all elements of a potentially huge matrix using just a few FLOPs for each entry. Such operations cannot properly utilize the massive compute capabilities of modern GPUs, and will be bottlenecked by the significantly lower memory bandwidth.

Fortunately, this problem can be resolved by the following observation: The final rounding decisions for column i i are only affected by updates performed on this very column, and so updates to later columns are irrelevant at this point in the process. This makes it possible to “lazily batch” updates together, thus achieving much better GPU utilization. Concretely, we apply the algorithm to B = 128 B=128 columns at a time, keeping updates contained to those columns and the corresponding B × B B\times B block of 𝐇 − 1 \mathbf{H}^{-1} (see also Figure 2 ). Only once a block has been fully processed, we perform global updates of the entire 𝐇 − 1 \mathbf{H}^{-1} and 𝐖 \mathbf{W} matrices using the multi-weight versions of Equations ( 2 ) and ( 3 ) given below, with Q Q denoting a set of indices, and 𝐇 − Q − 1 \mathbf{H}_{-Q}^{-1} denoting the inverse matrix with the corresponding rows and columns removed:

𝜹 F \displaystyle\bm{\delta}_{F}

= − ( 𝐰 Q − quant ( 𝐰 Q ) ) ( [ 𝐇 F − 1 ] Q ​ Q ) − 1 ( 𝐇 F − 1 ) :, Q, \displaystyle=-(\mathbf{w}_{Q}-\text{quant}(\mathbf{w}_{Q}))([\mathbf{H}_{F}^{-1}]_{QQ})^{-1}(\mathbf{H}_{F}^{-1})_{:,Q},

(4)

𝐇 − Q − 1 \displaystyle\mathbf{H}_{-Q}^{-1}

= ( 𝐇 − 1 − 𝐇 :, Q − 1 ( [ 𝐇 − 1 ] Q ​ Q ) − 1 𝐇 Q, : − 1 ) − Q. \displaystyle=\Big(\mathbf{H}^{-1}-\mathbf{H}^{-1}_{:,Q}([\mathbf{H}^{-1}]_{QQ})^{-1}\mathbf{H}^{-1}_{Q,:}\Big)_{-Q}.

(5)

Although this strategy does not reduce the theoretical amount of compute, it effectively addresses the memory-throughput bottleneck. This provides an order of magnitude speedup for very large models in practice, making it a critical component of our algorithm.

Step 3: Cholesky Reformulation.

The final technical issue we have to address is given by numerical inaccuracies, which can become a major problem at the scale of existing models, especially when combined with the block updates discussed in the previous step.
Specifically, it can occur that the matrix 𝐇 F − 1 \mathbf{H}_{F}^{-1} becomes indefinite, which we notice can cause the algorithm to aggressively update the remaining weights in incorrect directions, resulting in an arbitrarily-bad quantization of the corresponding layer.
In practice, we observed that the probability of this happening increases with model size: concretely, it almost certainly occurs for at least a few layers on models that are larger than a few billion parameters.
The main issue appears to be the repeated applications of Equation ( 5 ), which accumulate various numerical errors, especially through the additional matrix inversion.

For smaller models, applying dampening, that is adding a small constant λ \lambda (we always choose 1% of the average diagonal value) to the diagonal elements of 𝐇 \mathbf{H} appears to be sufficient to avoid numerical issues. However, larger models require a more robust and general approach.

To address this, we begin by noting that the only information required from 𝐇 F q − 1 \mathbf{H}_{F_{q}}^{-1}, where F q F_{q} denotes the set of unquantized weights when quantizing weight q q, is row q q, or more precisely, the elements in this row starting with the diagonal. The consequence is that we could precompute all of these rows using a more numerically-stable method without any significant increase in memory consumption.
Indeed, the row removal via ( 3 ) for our symmetric 𝐇 − 1 \mathbf{H}^{-1} essentially corresponds to taking a Cholesky decomposition, except for the minor difference that the latter divides row q q by ( [ 𝐇 F q − 1 ] q ​ q ) 1 / 2 ([\mathbf{H}^{-1}_{F_{q}}]_{qq})^{1/2}. Hence, we can leverage state-of-the-art Cholesky kernels to compute all information we will need from 𝐇 − 1 \mathbf{H}^{-1} upfront. In combination with mild dampening, the resulting method is robust enough to execute on huge models without issues. As a bonus, using a well-optimized Cholesky kernel also yields further speedup. We detail all small changes necessary for the Cholesky version of the algorithm next.

The Full Algorithm.

Finally, we present the full pseudocode for GPTQ in Algorithm 1, including the optimizations discussed above.

Algorithm 1 Quantize 𝐖 \mathbf{W} given inverse Hessian 𝐇 − 1 = ( 2 ​ 𝐗𝐗 ⊤ + λ ​ 𝐈 ) − 1 \mathbf{H}^{-1}=(2\mathbf{X}\mathbf{X}^{\top}+\lambda\mathbf{I})^{-1} and blocksize B B.

  𝐐 ← 𝟎 d row × d col \mathbf{Q}\leftarrow\mathbf{0}_{d_{\text{row}}\times d_{\text{col}}} // quantized output

  𝐄 ← 𝟎 d row × B \mathbf{E}\leftarrow\mathbf{0}_{d_{\text{row}}\times B} // block quantization errors

  𝐇 − 1 ← Cholesky ​ ( 𝐇 − 1 ) ⊤ \mathbf{H}^{-1}\leftarrow\text{Cholesky}(\mathbf{H}^{-1})^{\top} // Hessian inverse information

  for i = 0, B, 2 ​ B, … i=0,B,2B,\dots do

   for j = i, …, i + B − 1 j=i,\dots,i+B-1 do

    𝐐 :, j ← quant ( 𝐖 :, j ) \mathbf{Q}_{:,j}\leftarrow\text{quant}(\mathbf{W}_{:,j}) // quantize column

    𝐄 :, j − i ← ( 𝐖 :, j − 𝐐 :, j ) / [ 𝐇 − 1 ] j ​ j \mathbf{E}_{:,j-i}\leftarrow(\mathbf{W}_{:,j}-\mathbf{Q}_{:,j})\,/\,[\mathbf{H}^{-1}]_{jj} // quantization error

    𝐖 :, j : ( i + B ) ← 𝐖 :, j : ( i + B ) − 𝐄 :, j − i ⋅ 𝐇 − 1 j, j : ( i + B ) \mathbf{W}_{:,j:(i+B)}\leftarrow\mathbf{W}_{:,j:(i+B)}-\mathbf{E}_{:,j-i}\cdot\mathbf{H}^{-1}_{j,j:(i+B)} // update weights in block

   end for

   𝐖 :, ( i + B ) : ← 𝐖 :, ( i + B ) : − 𝐄 ⋅ 𝐇 − 1 i : ( i + B ), ( i + B ) : \mathbf{W}_{:,(i+B):}\leftarrow\mathbf{W}_{:,(i+B):}-\mathbf{E}\cdot\mathbf{H}^{-1}_{i:(i+B),(i+B):} // update all remaining weights

  end for

5 Experimental Validation

Overview.

We begin our experiments by validating the accuracy of GPTQ relative to other accurate-but-expensive quantizers, on smaller models, for which these methods provide reasonable runtimes.
Next, we examine GPTQ’s runtime scaling for very large models.
Then, we present 3- and 4-bit quantization results for the entire BLOOM and OPT model families, evaluated via perplexity on challenging language generation tasks.
In addition, we show that our
method is also stable for 2-bit quantization when the granularity is reduced to small blocks of consecutive weights.
To complement this perplexity analysis, we also evaluate the resulting quantized models on a series of standard zero-shot tasks.
Finally, we focus on the two largest (and interesting) openly-available models, Bloom-176B and OPT-175B, where we perform a detailed evaluation on several tasks.
For these models, we also present practical improvements, namely reducing the number of GPUs required for inference as well as end-to-end speedups for generative tasks.

Setup.

We implemented GPTQ in PyTorch ( Paszke et al. 2019 ) and worked with the HuggingFace integrations of the BLOOM ( Laurençon et al. 2022 ) and OPT ( Zhang et al. 2022 ) model families. We quantized all models (including the 175 billion parameter variants) using a single NVIDIA A100 GPU with 80GB of memory.
Our entire GPTQ calibration data consists of 128 random 2048 token segments from the C4 dataset ( Raffel et al. 2020 ), i.e., excerpts from randomly crawled websites, which represents generic text data.
We emphasize that this means that GPTQ does not see any task-specific data, and our results thus remain actually “zero-shot”.
We perform standard uniform per-row asymmetric quantization on the min-max grid, similar to Dettmers et al. 2022. Additional evaluation details can be found in Appendix A.2.1.

To ensure that the entire compression procedure can be performed with significantly less GPU memory than what would be required to run the full precision model, some care must be taken. Specifically, we always load one Transformer block, consisting of 6 layers, at a time into GPU memory and then accumulate the layer-Hessians and perform quantization. Finally, the current block inputs are sent through the fully quantized block again to produce the new inputs for the quantization of the next block. Hence, the quantization process operates not on the layer inputs in the full precision model but on the actual layer inputs in the already partially quantized one. We find that this brings noticeable improvements at negligible extra cost.

Baselines.

Our primary baseline, denoted by RTN, consists of rounding all weights to the nearest quantized value on exactly the same asymmetric per-row grid that is also used for GPTQ, meaning that it corresponds precisely to the state-of-the-art weight quantization of LLM.int8(). This is currently the method of choice in all works on quantization of very large language models ( Dettmers et al. 2022; Yao et al. 2022; Park et al. 2022 ) : its runtime scales well to networks with many billions of parameters, as it simply performs direct rounding. As we will also discuss further, more accurate methods, such as AdaRound ( Nagel et al. 2020 ) or BRECQ ( Li et al. 2021 ), are currently too slow for models with many billions of parameters, the main focus of this work. Nevertheless, we also show that GPTQ is competitive with such methods for small models, while scaling to huge ones like OPT-175B as well.

Quantizing Small Models.

As a first ablation study, we compare GPTQ’s performance relative to state-of-the-art post-training quantization (PTQ) methods, on ResNet18 and ResNet50, which are standard PTQ benchmarks, in the same setup as ( Frantar et al. 2022 ).
As can be seen in Table 2, GPTQ performs on par at 4-bit, and slightly worse than the most accurate methods at 3-bit. At the same time, it significantly outperforms AdaQuant, the fastest amongst prior PTQ methods.
Further, we compare against the full greedy OBQ method on two smaller language models: BERT-base ( Devlin et al. 2019 ) and OPT-125M.
The results are shown in Appendix Table 8.
At 4 bits, both methods perform similarly, and for 3 bits, GPTQ surprisingly performs slightly better.
We suspect that this is because some of the additional heuristics used by OBQ, such as early outlier rounding, might require careful adjustments for optimal performance on non-vision models.
Overall, GPTQ appears to be competitive with state-of-the-art post-training methods for smaller models, while taking only < 1 <1 minute rather than ≈ 1 \approx 1 hour.
This enables scaling to much larger models.

Method

RN18 – 69.76 %

RN50 – 76.13%

4bit

3bit

4bit

3bit

AdaRound

69.34

68.37

75.84

75.14

AdaQuant

68.12

59.21

74.68

64.98

BRECQ

69.37

68.47

75.88

75.32

OBQ

69.56

68.69

75.72

75.24

GPTQ

69.37

67.88

75.71

74.87

Table 1: Comparison with state-of-the-art post-training methods for vision models.

OPT

13B

30B

66B

175B

Runtime

20.9m

44.9m

1.6h

4.2h

BLOOM

1.7B

3B

7.1B

176B

Runtime

2.9m

5.2m

10.0m

3.8h

Table 2: GPTQ runtime for full quantization of the 4 largest OPT and BLOOM models.

Runtime.

Next we measure the full model quantization time (on a single NVIDIA A100 GPU) via GPTQ; the results are shown in Table 2. As can be seen, GPTQ quantizes 1-3 billion parameter models in a matter of minutes and 175B ones in a few hours. For reference, the straight-through based method ZeroQuant-LKD ( Yao et al. 2022 ) reports a 3 hour runtime (on the same hardware) for a 1.3B model, which would linearly extrapolate to several hundred hours (a few weeks) for 175B models. Adaptive rounding-based methods typically employ a lot more SGD steps and would thus be even more expensive ( Nagel et al. 2020; Li et al. 2021 ).

Language Generation.

We begin our large-scale study by compressing the entire OPT and BLOOM model families to 3- and 4-bit.
We then evaluate those models on several language tasks including WikiText2 ( Merity et al. 2016 ) (see Figure 1 as well as Tables 3 and 4 ), Penn Treebank (PTB) ( Marcus et al. 1994 ) and C4 ( Raffel et al. 2020 ) (both in Appendix A.3 ).
We focus on these perplexity-based tasks, as they are known to be particularly sensitive to model quantization ( Yao et al. 2022 ).
On OPT models, GPTQ clearly outperforms RTN, by significant margins.
For example, GPTQ loses only 0.03 perplexity at 4-bit on the 175B model, while RTN drops 2.2 points, performing worse than the 10 × 10\times smaller full-precision 13B model.
At 3-bit, RTN collapses completely, while GPTQ can still maintain reasonable perplexity, in particular for larger models.
BLOOM shows a similar pattern: the gaps between methods are however usually a bit smaller, indicating that this model family might be easier to quantize.
One interesting trend (see also Figure 1 ) is that larger models generally (with the exception of OPT-66B 2 2
2

Upon closer inspection of the OPT-66B model, it appears that this is correlated with the fact that this trained model has a significant fraction of dead units in the early layers, which may make it harder to compress. ) appear easier to quantize.
This is good news for practical applications, as these are the cases where compression is also the most necessary.

OPT

Bits

125M

350M

1.3B

2.7B

6.7B

13B

30B

66B

175B

full

16

27.65

22.00

14.63

12.47

10.86

10.13

9.56

9.34

8.34

RTN

4

37.28

25.94

48.17

16.92

12.10

11.32

10.98

110

10.54

GPTQ

4

31.12

24.24

15.47

12.87

11.39

10.31

9.63

9.55

8.37

RTN

3

1.3e3

64.57

1.3e4

1.6e4

5.8e3

3.4e3

1.6e3

6.1e3

7.3e3

GPTQ

3

53.85

33.79

20.97

16.88

14.86

11.61

10.27

14.16

8.68

Table 3: OPT perplexity results on WikiText2.

BLOOM

Bits

560M

1.1B

1.7B

3B

7.1B

176B

full

16

22.42

17.69

15.39

13.48

11.37

8.11

RTN

4

25.90

22.00

16.97

14.76

12.10

8.37

GPTQ

4

24.03

19.05

16.48

14.20

11.73

8.21

RTN

3

57.08

50.19

63.59

39.36

17.38

571

GPTQ

3

32.31

25.08

21.11

17.40

13.47

8.64

Table 4: BLOOM perplexity results for WikiText2.

175 Billion Parameter Models.

We now examine BLOOM-176B and OPT-175B, the largest dense openly-available models.
Table 5 summarizes results across Wikitext-2, PTB, C4.
We observe that, at 4 bits, GPTQ models reach only ≤ 0.25 \leq 0.25 lower perplexity than the full-precision versions, with a large gap to RTN results on OPT-175B.
At 3-bit, RTN collapses, while GPTQ is still able to maintain good performance on most tasks, losing only 0.3 − 0.6 0.3-0.6 points for more than 5 × 5\times compression.
We note that GPTQ’s accuracy can be further improved via finer-granularity grouping ( Park et al. 2022 ) : group-size 1024 ( ≈ \approx 0.02 extra bits) improves perplexities by about 0.2 0.2 on average and group-size 128 ( ≈ \approx 0.15 extra bits) by another 0.1 0.1, which is only 0.1 − 0.3 0.1-0.3 off from the uncompressed accuracy. We note that grouping interacts very well with GPTQ, as the group parameters can be
determined during the quantization process of each layer, always using the most current updated weights.

Method

Bits

OPT-175B

BLOOM-176B

Wiki2

PTB

C4

LAMB. ↑ \uparrow

Wiki2

PTB

C4

LAMB. ↑ \uparrow

Baseline

16

8.34

12.01

10.13

75.59

8.11

14.59

11.71

67.40

RTN

4

10.54

14.22

11.61

71.34

8.37

15.00

12.04

66.70

GPTQ

4

8.37

12.26

10.28

76.80

8.21

14.75

11.81

67.71

RTN

3

7.3e3

8.0e3

4.6e3

0

571.

107.

598.

0.17

GPTQ

3

8.68

12.68

10.67

76.19

8.64

15.57

12.27

65.10

GPTQ

3/g1024

8.45

12.48

10.47

77.39

8.35

15.01

11.98

67.47

GPTQ

3/g128

8.45

12.37

10.36

76.42

8.26

14.89

11.85

67.86

Table 5: Results summary for OPT-175B and BLOOM-176B. “g1024” and “g128” denote results with groupings of size 1024 and 128, respectively.

Practical Speedups.

Finally, we study practical applications.
As an interesting use-case, we focus on the OPT-175B model: quantized to 3 bits, this model takes approximately 63GB of memory, including the embeddings and the output layer, which are kept in full FP16 precision.
Additionally, storing the complete history of keys and values for all layers, a common optimization for generation tasks, consumes another ≈ 9 \approx 9 GB for the maximum of 2048 tokens. Hence, we can actually fit the entire quantized model into a single 80GB A100 GPU, which can be executed by dynamically dequantizing layers as they are required during inference (the model would not fully fit using 4 bits).
For reference, standard FP16 execution requires 5x80GB GPUs, and the state-of-the-art 8bit LLM.int8() quantizer ( Dettmers et al. 2022 ) requires 3 such GPUs.

Next, we consider language generation, one of the most appealing applications of these models, with the goal of latency reduction.
Unlike LLM.int8(), which reduces memory costs but has the same runtime as the FP16 baseline, we show that our quantized models can achieve significant speedups for this application.
For language generation, the model processes and outputs one token at-a-time, which for OPT-175B can easily take a few 100s of milliseconds per token.
Increasing the speed at which the user receives generated results is challenging, as compute is dominated by matrix-vector products.
Unlike matrix-matrix products, these are primarily limited by memory bandwidth.
We address this problem by developing a quantized-matrix full-precision-vector product kernel which performs a matrix vector product by dynamically dequantizing weights when needed. Most notably, this does not require any activation quantization.
While dequantization consumes extra compute, the kernel has to access a lot less memory, leading to significant speedups, as shown in Table 6. We note that almost all of the speedup is due to our kernels, as communication costs are negligible in our standard HuggingFace-accelerate-like setting (see Appendix A.2.2 for details).

GPU

FP16

3bit

Speedup

GPU reduction

A6000 – 48GB

589ms

130ms

4.53 × 4.53\times

8 → 2 8\rightarrow 2

A100 – 80GB

230ms

71ms

3.24 × 3.24\times

5 → 1 5\rightarrow 1

Table 6: Average per-token latency (batch size 1) when generating sequences of length 128.

For example, using our kernels, the 3-bit OPT-175B model obtained via GPTQ running on a single A100 is about 3.25 × \mathbf{3.25\bm{\times}} faster than the FP16 version (running on 5 GPUs) in terms of average time per token.
More accessible GPUs, such as the NVIDIA A6000, have much lower memory bandwidth, so this strategy is even more effective: executing the 3-bit OPT-175B model on 2x A6000 GPUs reduces latency from 589 milliseconds for FP16 inference (on 8 GPUs) to 130 milliseconds, a 4.5 × \mathbf{4.5\bm{\times}} latency reduction.

Figure 3: The accuracy of OPT and BLOOM models post-GPTQ, measured on LAMBADA.

Zero-Shot Tasks.

While our focus is on language generation, we also evaluate the performance of quantized models on some popular zero-shot tasks, namely LAMBADA ( Paperno et al. 2016 ), ARC (Easy and Challenge) ( Boratko et al. 2018 ) and PIQA ( Tata & Patel 2003 ).
Figure 3 visualizes model performance on LAMBADA (and see also “Lamb.” results in Table 5 ).
We observe similar behavior as before: the outliers are that 1) quantization appears “easier” across the whole spectrum of models at 4-bit, where even RTN performs relatively well, and 2) at 3-bit, RTN breaks down, while GPTQ still provides good accuracy. We provide additional results in Appendix A.4.

Additional Tricks.

While our experiments so far have focused exclusively on vanilla row-wise quantization, we want to emphasize that GPTQ is compatible with essentially any choice of quantization grid. For example, it is easily combined with standard grouping ( Alistarh et al. 2017; Park et al. 2022 ), i.e. applying independent quantization to groups of g g consecutive weights. As shown in the last rows of Table 5, this can bring noticeable extra accuracy for the largest models at 3-bit. Further, as visualized in Figure 4, it significantly reduces the accuracy losses for medium sized models at 4-bit precision.

Model

FP16

g128

g64

g32

3-bit

OPT-175B

8.34

9.58

9.18

8.94

8.68

BLOOM

8.11

9.55

9.17

8.83

8.64

Table 7: 2-bit GPTQ quantization results with varying group-sizes; perplexity on WikiText2.

Figure 4: GPTQ at 4-bit with different group-sizes on medium sized OPT models.

Extreme Quantization.

Lastly, grouping also makes it possible to achieve reasonable performance for extreme quantization, to around 2-bits per component on average. Table 7 shows results on WikiText2 when quantizing the biggest models to 2-bit with varying group-sizes. At ≈ 2.2 \approx 2.2 bit (group-size 128; using FP16 scale and 2-bit zero point per group) the perplexity increase is already less than 1.5 points, while dropping to 0.6 - 0.7 at ≈ 2.6 \approx 2.6 bit (group-size 32), which is only slightly worse than vanilla 3-bit and might be interesting for practical kernel implementations. Further, if we reduce group size to 8, we can apply ternary (-1, 0, +1) quantization, which achieves 9.20 WikiText2 PPL on OPT-175B, a less than 1 point drop.
While this leads to worse compression on average relative to the 2-bit numbers above, this pattern could be efficiently implemented on custom hardware such as FPGAs.
In summary, these results are an encouraging first step towards pushing highly-accurate one-shot compression of very large language models, even lower than 3 bits per value on average.

6 Summary and Limitations

We have presented GPTQ, an approximate second-order method for quantizing truly large language models.
GPTQ can accurately compress some of the largest publicly-available models down to 3 and 4 bits, which leads to significant usability improvements, and to end-to-end speedups, at low accuracy loss.
We hope that our method will make these models accessible to more researchers and practitioners.
At the same time, we emphasize some significant limitations:
On the technical side, our method obtains speedups from reduced memory movement, and does not lead to computational reductions.
In addition, our study focuses on generative tasks, and does not consider activation quantization.
These are natural directions for future work, and we believe this can be achieved with carefully-designed GPU kernels and existing techniques ( Yao et al. 2022; Wu et al. 2022 ).

Acknowledgments

Elias Frantar and Dan Alistarh gratefully acknowledge funding from the European Research Council (ERC) under the European Union’s Horizon 2020 programme (grant agreement No. 805223 ScaleML), as well as experimental support from Eldar Kurtic, and from the IST Austria IT department, in particular Stefano Elefante, Andrei Hornoiu, and Alois Schloegl.
The work of Saleh Ashkboos and Torsten Hoefler was supported by the PASC DaCeMI project, received EuroHPC-JU funding under grant MAELSTROM, No. 955513. We thank the Swiss National Supercomputing Center (CSCS) for supporting us with compute infrastructure.

7 Ethics Statement

Our work introduces a general method for compressing large language models (LLMs) via quantization, with little-to-no accuracy loss in terms of standard accuracy metrics such as perplexity.
Our method is task-agnostic, as it only uses a tiny amount of randomly-chosen data for calibration.
We therefore do not foresee any significant ethical implications arising directly from the technical details of our method.
However, one possible consideration is that our study focused on “leading accuracy” metrics that are standard in the literature, such as perplexity, which is essentially standard in the literature ( Dettmers et al. 2022; Yao et al. 2022 ).
We believe a thorough study of the impact of compression upon secondary measures, and in particular bias effects ( Bender et al. 2021 ) is warranted, and may be rendered easier through our work.
At the same time, our work makes inference on extremely large language models more accessible, for better or for worse.
We believe that, in time, such tools will become much easier to use and deploy, making the need to understand their power and limitations even more stringent.

8 Reproducibility Statement

In the Supplementary Materials, we provide code to reproduce all experiments in this paper. More specifically, this includes:

•

Compressing all models from the OPT and BLOOM model families to 2/3/4 bits.

•

Evaluating perplexity of the quantized models.

•

Our 3-bit CUDA kernel together with compressed inference benchmarking features.

•

Code for the ZeroShot experiments.

•

A README file providing sample commands and information on how to run all scripts.

References

Alistarh et al. (2017)

Dan Alistarh, Demjan Grubic, Jerry Li, Ryota Tomioka, and Milan Vojnovic.

QSGD: Randomized quantization for communication-efficient
stochastic gradient descent.

In Conference on Neural Information Processing Systems
(NeurIPS), 2017.

Bender et al. (2021)

Emily M Bender, Timnit Gebru, Angelina McMillan-Major, and Shmargaret
Shmitchell.

On the dangers of stochastic parrots: Can language models be too big?

In 2021 ACM Conference on Fairness, Accountability, and
Transparency, 2021.

Boratko et al. (2018)

Michael Boratko, Harshit Padigela, Divyendra Mikkilineni, Pritish Yuvraj,
Rajarshi Das, Andrew McCallum, Maria Chang, Achille Fokoue-Nkoutche, Pavan
Kapanipathi, Nicholas Mattei, et al.

A systematic classification of knowledge, reasoning, and context
within the ARC dataset.

arXiv preprint arXiv:1806.00358, 2018.

Brown et al. (2020)

Tom Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared D Kaplan, Prafulla
Dhariwal, Arvind Neelakantan, Pranav Shyam, Girish Sastry, Amanda Askell,
et al.

Language models are few-shot learners.

In Conference on Neural Information Processing Systems
(NeurIPS), 2020.

Dao et al. (2022)

Tri Dao, Daniel Y Fu, Stefano Ermon, Atri Rudra, and Christopher Ré.

FlashAttention: Fast and memory-efficient exact attention with
io-awareness.

arXiv preprint arXiv:2205.14135, 2022.

Dettmers et al. (2022)

Tim Dettmers, Mike Lewis, Younes Belkada, and Luke Zettlemoyer.

LLM.int8(): 8-bit matrix multiplication for transformers at scale.

arXiv preprint arXiv:2208.07339, 2022.

Devlin et al. (2019)

Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova.

BERT: Pre-training of deep bidirectional transformers for language
understanding.

In North American Chapter of the Association for Computational
Linguistics (NAACL), 2019.

Frantar et al. (2021)

Elias Frantar, Eldar Kurtic, and Dan Alistarh.

M-FAC: Efficient matrix-free approximations of second-order
information.

In Conference on Neural Information Processing Systems
(NeurIPS), 2021.

Frantar et al. (2022)

Elias Frantar, Sidak Pal Singh, and Dan Alistarh.

Optimal Brain Compression: A framework for accurate post-training
quantization and pruning.

arXiv preprint arXiv:2208.11580, 2022.

Accepted to NeurIPS 2022, to appear.

Gholami et al. (2021)

Amir Gholami, Sehoon Kim, Zhen Dong, Zhewei Yao, Michael W Mahoney, and Kurt
Keutzer.

A survey of quantization methods for efficient neural network
inference.

arXiv preprint arXiv:2103.13630, 2021.

Hassibi et al. (1993)

Babak Hassibi, David G Stork, and Gregory J Wolff.

Optimal brain surgeon and general network pruning.

In IEEE International Conference on Neural Networks, 1993.

Hoefler et al. (2021)

Torsten Hoefler, Dan Alistar
