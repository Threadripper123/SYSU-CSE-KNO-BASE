# 论文·AWQ：激活感知的权重量化

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　45,087 字符　｜　526 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/abs/2306.00978
抓取: https://ar5iv.labs.arxiv.org/html/2306.00978

marginparsep has been altered.
topmargin has been altered.
marginparwidth has been altered.
marginparpush has been altered.

The page layout violates the ICML style.

Please do not change the page layout, or include packages like geometry,
savetrees, or fullpage, which change it for you.

We’re not able to reliably undo arbitrary changes to the style. Please remove
the offending package(s), or layout-changing commands and try again.

AWQ: A ctivation-aware W eight Q uantization for
On-Device LLM Compression and Acceleration

Ji Lin   *  1 
Jiaming Tang   *  1  2 
Haotian Tang   †  1 
Shang Yang   †  1 
Wei-Ming Chen   3 
Wei-Chen Wang   1 
Guangxuan Xiao   1 
Xingyu Dang   1  4 
Chuang Gan   5  6 
Song Han   1  3 

https://github.com/mit-han-lab/llm-awq

† † footnotetext: * : Algorithm co-lead, † : system co-lead.
1 MIT
2 Shanghai Jiao Tong University
3 NVIDIA
4 Tsinghua University
5 MIT-IBM Watson AI Lab
6 UMass Amherst.
Correspondence to: Song Han <songhan@mit.edu>.

Proceedings of the
7 t ​ h \mathit{7}^{th} MLSys Conference,
Santa Clara, CA, USA, 2024. Best Paper Award. Copyright 2024 by the author(s).
Abstract

Large language models (LLMs) have transformed numerous AI applications. On-device LLM is becoming increasingly important: running LLMs locally on edge devices can reduce the cloud computing cost and protect users’ privacy. However, the astronomical model size and the limited hardware resource pose significant deployment challenges. We propose Activation-aware Weight Quantization (AWQ), a hardware-friendly approach for LLM low-bit weight-only quantization. AWQ finds that not all weights in an LLM are equally important. Protecting only 1% salient weights can greatly reduce quantization error. To identify salient weight channels, we should refer to the activation distribution, not weights.
To avoid the hardware-inefficient mix-precision quantization, we mathematically derive that scaling up the salient channels can reduce the quantization error.
AWQ employs an equivalent transformation to scale the salient weight channels to protect them. The scale is determined by collecting the activation statistics offline.
AWQ does not rely on any backpropagation or reconstruction, so it generalizes to different domains and modalities without overfitting the calibration set. AWQ outperforms existing work on various language modeling and domain-specific benchmarks (coding and math). Thanks to better generalization, it achieves excellent quantization performance for instruction-tuned LMs and, for the first time, multi-modal LMs.
Alongside AWQ, we implement TinyChat, an efficient and flexible inference framework tailored for 4-bit on-device LLM/VLMs. With kernel fusion and platform-aware weight packing, TinyChat offers more than 3 × \times speedup over the Huggingface FP16 implementation on both desktop and mobile GPUs. It also democratizes the deployment of the 70B Llama-2 model on mobile GPUs.

1 Introduction

Deploying large language models (LLMs) directly on edge devices is crucial. On-device usage eliminates delays caused by sending data to a cloud server and enables LLMs to operate offline, which is beneficial for real-time applications like virtual assistants, chatbots, and autonomous vehicles. The operational costs associated with maintaining and scaling centralized cloud infrastructure can also be reduced. On-device LLM also enhances data security by keeping sensitive information local, reducing the chance of data breaches. LLMs, grounded in transformer-based architectures Vaswani et al. 2017, have gathered significant attention for their impressive performance across diverse benchmarks Brown et al. 2020; Zhang et al. 2022; Touvron et al. 2023a; Scao et al. 2022. However, the large model size leads to the high serving costs. For example, GPT-3 has 175B parameters, which is 350GB in FP16, while the latest B200 GPU only has 192GB memory, let alone edge devices.

Figure 1: We introduce AWQ, a versatile weight quantization method for LLM. To implement AWQ, we developed TinyChat to deploy 4-bit quantized LLMs into various edge platforms, achieving a 3-4 × \times performance boost compared to FP16. Notably, we’ve also manufactured a TinyChat computer, powered by TinyChat, which contains an NVIDIA Jetson Orin Nano with only 8GB of memory and 15W power consumption. Demo: https://youtu.be/z91a8DrfgEw.

Low-bit weight quantization for LLMs can significantly reduce the memory footprint of on-device LLM inference but is hard.
Quantization-aware training (QAT) is not efficient due to the high training cost, while post-training quantization (PTQ) suffers from large accuracy degradation under a low-bit setting. The closest work is GPTQ Frantar et al. 2022, which uses second-order information to perform error compensation.
However, it may overfit the calibration set during reconstruction, distorting the learned features on out-of-distribution domains (Figure 8 ), which is problematic since LLMs are generalist models.

In this paper, we propose Activation-aware Weight Quantization (AWQ), a hardware-friendly low-bit weight-only quantization method for LLMs. Our method is based on the observation that weights are not equally important for LLMs’ performance. There is a small fraction (0.1%-1%) of salient weights; skipping the quantization of these salient weights will significantly reduce the quantization loss (Table 1 ). To find the salient weight channels, the insight is that we should refer to the activation distribution instead of the weight distribution, despite we are doing weight-only quantization: weight channels corresponding to larger activation magnitudes are more salient since they process more important features.
To avoid the hardware-inefficient mixed-precision implementation, we analyze the error from weight quantization and derive that scaling up the salient channels can reduce their relative quantization error (Equation 2 ). Following the intuition, we designed a per-channel scaling method to automatically search for the optimal scaling that minimizes the quantization error under full-weight quantization.
AWQ does not rely on any backpropagation or reconstruction, so it can well preserve LLMs’ generalization ability on various domains and modalities without overfitting to the calibration set.

To implement AWQ, we designed TinyChat, an efficient inference framework to convert theoretical memory savings from 4-bit LLM to measured speedup. Our framework significantly speeds up linear layers through on-the-fly dequantization. We also take advantage of efficient 4-bit weight packing and kernel fusion to minimize the inference overhead ( e.g., intermediate DRAM access and kernel launch overhead), such that we can better realize the speed up from quantizing the weights to 4-bit, despite the computer is byte-aligned.

Experiments show that AWQ outperforms existing work on various tasks
for different model families ( e.g., LLaMA Touvron et al. 2023a, OPT Zhang et al. 2022 ) and model sizes. Thanks to better generalization, it also achieves good quantization performance for instruction-tuned LMs ( e.g., Vicuna) and, for the first time, multi-modal LMs (OpenFlamingo Awadalla et al. 2023 ).
TinyChat further translates the ∼ \sim 4 × \times lower memory footprint to measured speedup. On desktop, laptop and mobile GPUs, we consistently observe a 3.2-3.3 × \times average speedup compared to the FP16 implementation by Huggingface across a diverse spectrum of LLMs. Furthermore, it facilitates effortless deployment of the Llama-2-70B model on a single NVIDIA Jetson Orin with 64GB of memory. It also democratizes 13 billion parameter LLM at an interactive pace of 30 tokens/second on a laptop RTX 4070 GPU with only 8GB of memory. AWQ has been widely adopted by industry and open-source community: HuggingFace Transformers, NVIDIA TensorRT-LLM, Microsfot DirectML, Google Vertex AI, Intel Neural Compressor, Amazon Sagemaker, AMD, FastChat, vLLM, LMDeploy, and enables Falcon-180B deployable on a single H200 GPU.

Figure 2: We observe that we can find 1% of the salient weights in LLMs based on the activation distribution (middle). Keeping the salient weights in FP16 can significantly improve the quantized performance (PPL from 43.2 (left) to 13.0 (middle)), but the mixed-precision format is not hardware-efficient. We follow the activation-awareness principle and propose AWQ (right). AWQ performs per-channel scaling to protect the salient weights and reduce quantization error. We measure the perplexity of OPT-6.7B under INT3-g128 quantization.

2 Related Work

Model quantization methods.

Quantization reduces the bit-precision of deep learning models Han et al. 2016; Jacob et al. 2018; Nagel et al. 2019; Wang et al. 2019; Nagel et al. 2020; Lin et al. 2020, which helps to reduce the model size and accelerate inference. Quantization techniques generally fall into two categories: quantization-aware training (QAT, which relies on backpropagation to update the quantized weights) Bengio et al. 2013; Gholami et al. 2021; Nagel et al. 2021; Choi et al. 2018 and post-training quantization Jacob et al. 2018; Nagel et al. 2019; Nagel et al. 2020 (PTQ, usually training-free). The QAT methods cannot easily scale up to large models like LLMs. Therefore, people usually use PTQ methods to quantize LLMs.

Quantization of LLMs.

People study two settings for LLM quantization: (1) W8A8 quantization, where both activation and weights are quantized to INT8 Dettmers et al. 2022; Xiao et al. 2022; Yao et al. 2022; Wei et al. 2022a; Wei et al. 2023; (2) Low-bit weight-only quantization ( e.g., W4A16), where only weights are quantized into low-bit integers Frantar et al. 2022; Dettmers & Zettlemoyer 2022; Sheng et al. 2023; Park et al. 2022. We focus on the second setting in this work since it not only reduces the hardware barrier (requiring a smaller memory size) but also speeds up the token generation (remedies memory-bound workload). Apart from the vanilla round-to-nearest baseline (RTN), GPTQ Frantar et al. 2022 is the closest to our work. However, the reconstruction process of GPTQ leads to an over-fitting issue to the calibration set and may not preserve the generalist abilities of LLMs for other modalities and domains. It also requires a reordering trick to work for some models ( e.g., LLaMA-7B Touvron et al. 2023a and OPT-66B Zhang et al. 2022 ). Apart from quantiztion methods designed for general-purporse hardware, SpAtten Wang et al. 2020 designs a progressive approach to gradually increase the number of bits used in softmax calculation.

System support for low-bit quantized LLMs.

Low-bit quantized LLMs have been a popular setting to reduce inference costs. There are some system supports to achieve a practical speed-up. GPTQ Frantar et al. 2022 provides INT3 kernels for OPT models and GPTQ-for-LLaMA extends kernel support for INT4 reordered quantization with the help of Triton Tillet et al. 2019. FlexGen Sheng et al. 2023, llama.cpp * *
*

https://github.com/ggerganov/llama.cpp and exllama † †
†

https://github.com/turboderp/exllama perform group-wise INT4 quantization to reduce I/O costs and offloading. FasterTransformer implements FP16 × \times INT4 GEMM for weight-only per-tensor quantization but does not support group quantization. LUT-GEMM Park et al. 2022 performs bitwise computation on GPU CUDA cores with the help of lookup tables. Our concurrent work, MLC-LLM MLC-Team 2023 offers strong results on multiple edge CPU and GPU platforms thanks to the powerful TVM Chen et al. 2018; Feng et al. 2023 backend.

3 AWQ: Activation-aware Weight Quantization

Quantization maps a floating-point number into lower-bit integers. It is an effective method to reduce the model size and inference costs of LLMs Dettmers et al. 2022; Frantar et al. 2022; Yao et al. 2022; Xiao et al. 2022. In this section, we first propose a weight-only quantization method to improve accuracy without training/regression by protecting more “important” weights. And then develop a data-driven method to search for the optimal scaling that reduces quantization errors (Figure 2 ).

3.1 Improving LLM Quantization by Preserving 1% Salient Weights

PPL ↓ \downarrow

FP16

RTN

FP16% (based on act.)

FP16% (based on W)

FP16% (random)

(w3-g128)

0.1%

1%

3%

0.1%

1%

3%

0.1%

1%

3%

OPT-1.3B

14.62

119.00

25.03

16.91

16.68

108.71

98.55

98.08

119.76

109.38

61.49

OPT-6.7B

10.86

23.54

11.58

11.39

11.36

23.41

22.37

22.45

23.54

24.23

24.22

OPT-13B

10.13

46.04

10.51

10.43

10.42

46.07

48.96

54.49

44.87

42.00

39.71

Table 1: Keeping a small fraction of weights (0.1%-1%) in FP16 significantly improves the performance of the quantized models over round-to-nearest (RTN). It is only effective when we select the important weights in FP16 by looking at activation distribution instead of weight distribution. We highlight results with a decent perplexity in green.
We used INT3 quantization with a group size of 128 and measured the WikiText perplexity ( ↓ \downarrow ).

We observe that the weights of LLMs are not equally important : there is a small fraction of salient weights that are much more important for LLMs’ performance compared to others. Skipping the quantization of these salient weights can help bridge the performance degradation due to the quantization loss without any training or regression (Figure 2 (b)).
To verify the idea, we benchmark the performance of quantized LLMs when skipping part of the weight channels in Table 1. We measured the performance of INT3 quantized models while keeping some ratios of weight channels in FP16. A widely used method to determine the importance of weights is to look at its magnitude or L 2 L_{2} -norm Han et al. 2015; Frankle & Carbin 2018. But we find skipping the weight channels with large norm ( i.e., FP16% (based on W)) does not significantly improve the quantized performance, leading to a similar marginal improvement as random selection.
Interestingly, selecting weights based on activation magnitude can significantly improve the performance despite keeping only 0.1%-1% of channels in FP16.
We hypothesize that the input features with larger magnitudes are generally more important. Keeping the corresponding weights in FP16 can preserve those features, which contributes to better model performance.

Limitations:
Despite keeping 0.1% of weights in FP16 can improve the quantized performance without a noticeable increase in model size (measured in total bits), such a mixed-precision data type will make the system implementation difficult. We need to come up with a method to protect the important weights without actually keeping them as FP16.

3.2 Protecting Salient Weights by Activation-aware Scaling

We propose an alternative method to reduce the quantization error of the salient weight by per-channel scaling, which does not suffer from the hardware inefficiency issue.

Analyzing the quantization error.

OPT-6.7B

s = 1 s=1

s = 1.25 s=1.25

s = 1.5 s=1.5

s = 2 s=2

s = 4 s=4

proportion of Δ ′ ≠ Δ \Delta^{{}^{\prime}}\neq\Delta

0%

2.8%

4.4%

8.2%

21.2%

average Δ ′ / Δ \Delta^{{}^{\prime}}/\Delta

1

1.005

1.013

1.038

1.213

average Δ ′ Δ ⋅ 1 s \frac{\Delta^{{}^{\prime}}}{\Delta}\cdot\frac{1}{s}

1

0.804

0.676

0.519

0.303

Wiki-2 PPL

23.54

12.87

12.48

11.92

12.36

Table 2: Statistics when multiplying the 1% salient channels by s > 1 s>1. Scaling up the salient channels significantly improves the perplexity (23.54 to 11.92). As s s goes larger, the percentage of changed Δ \Delta increases, and the error reduction rate for salient channels also increases. However, the best perplexity is achieved at s = 2 s=2, since further increasing s s will increase the quantization error for non-salient channels.

We start by analyzing the error from weight-only quantization.
Consider a group/block of weight 𝐰 \mathbf{w}; the linear operation can be written as y = 𝐰𝐱 y=\mathbf{w}\mathbf{x}, and the quantized counterpart is y = Q ⁡ ( 𝐰 ) ​ 𝐱 y=Q(\mathbf{w})\mathbf{x}. Specifically, the quantization function is defined as:

Q ⁡ ( 𝐰 ) = Δ ⋅ Round ​ ( 𝐰 Δ ), Δ = max ⁡ ( | 𝐰 | ) 2 N − 1, Q(\mathbf{w})=\Delta\cdot\text{Round}(\frac{\mathbf{w}}{\Delta}),\quad\Delta=\frac{\max(|\mathbf{w}|)}{2^{N-1}},

(1)

where N N is the number of quantization bits, and Δ \Delta is the quantization scaler determined by the absolute maximum value. Now consider a weight element w ∈ 𝐰 w\in\mathbf{w}, if we multiply w w with s > 1 s>1 and the inversely scale x x, we will have Q ​ ( w ⋅ s ) ​ ( x / s ) Q(w\cdot s)(x/s), which is:

Q ( w ⋅ s ) ⋅ x s = Δ ′ ⋅ Round ( w ​ s Δ ′ ) ⋅ x ⋅ 1 s, Q(w\cdot s)\cdot\frac{x}{s}=\Delta^{{}^{\prime}}\cdot\text{Round}(\frac{ws}{\Delta^{{}^{\prime}}})\cdot x\cdot\frac{1}{s},

(2)

where Δ ′ \Delta^{{}^{\prime}} is the new quantization scaler after applying s s. We empirically find that: (1) The expected error from Round ​ ( ⋅ ) \text{Round}(\cdot) (denoted as RoundErr ​ ( ⋅ ) \texttt{RoundErr}(\cdot) ) does not change: since the round function maps a floating-point number to an integer, the error is roughly uniformly distributed from [0,0.5], resulting in an average error of 0.25 0.25; i.e., RoundErr ​ ( ⋅ ) ∼ 0.25 \texttt{RoundErr}(\cdot)\sim 0.25. (2) Scaling up a single element w w usually does not change the maximum value from the group 𝐰 \mathbf{w}. Therefore we have Δ ′ ≈ Δ \Delta^{{}^{\prime}}\approx\Delta;
(3) As Δ \Delta and x x are represented in FP16, they have no quantization error. Consequently, the quantization error from equation 1 and 2 can be expressed as

Err ​ ( Q ​ ( w ) ​ x ) \displaystyle\texttt{Err}(Q(w)x)

= Δ ⋅ RoundErr ​ ( w Δ ) ⋅ x \displaystyle=\Delta\cdot\texttt{RoundErr}(\frac{w}{\Delta})\cdot x

(3)

Err ​ ( Q ​ ( w ⋅ s ) ​ ( x s ) ) \displaystyle\texttt{Err}(Q(w\cdot s)(\frac{x}{s}))

= Δ ′ ⋅ RoundErr ( w ​ s Δ ′ ) ⋅ x ⋅ 1 s \displaystyle=\Delta^{{}^{\prime}}\cdot\texttt{RoundErr}(\frac{ws}{\Delta^{{}^{\prime}}})\cdot x\cdot\frac{1}{s}

The ratio of the new error to the original error is Δ ′ Δ ⋅ 1 s \frac{\Delta^{{}^{\prime}}}{\Delta}\cdot\frac{1}{s}. Given Δ ′ ≈ Δ \Delta^{{}^{\prime}}\approx\Delta and s > 1 s>1, the relative error is smaller for the salient weight w w.

To verify the idea, we multiply the 1% salient channels with s > 1 s>1 for the OPT-6.7B model, and measure the change in Δ \Delta for each group in Table 2. We find that scaling up the salient channels is quite effective: the perplexity improves from 23.54 for s = 1 s=1 (simply RTN) to 11.92 for s = 2 s=2.
As s s goes larger, the percentage of changed Δ \Delta generally gets larger, but the percentage is still quite small for s < 2 s<2 (less than 5%); the relative error for the salient channels continues to go smaller as s s increases. Nonetheless, the best PPL actually appears at s = 2 s=2. This is because if we use a very large s s, it will increase the relative error for the non-salient channels when Δ \Delta increases (the error of non-salient channels will be amplified by Δ ′ Δ \frac{\Delta^{{}^{\prime}}}{\Delta}, and the ratio is larger than 1 for 21.2% of the channels under s = 4 s=4 ), which can damage the model’s overall accuracy. Therefore, we need to also consider the error from non-salient channels when protecting salient ones.

OPT (PPL ↓ \downarrow )

1.3B

2.7B

6.7B

13B

30B

FP16

14.62

12.47

10.86

10.13

9.56

RTN

119.47

298.00

23.54

46.04

18.80

1% FP16

16.91

13.69

11.39

10.43

9.85

s = 2 s=2

18.63

14.94

11.92

10.80

10.32

AWQ

16.32

13.58

11.39

10.56

9.77

Table 3: AWQ protects salient weights and reduces quantization error by using a scaling-based method. It consistently outperforms Round-to-nearest quantization (RTN) and achieves comparable performance as mixed-precision (1% FP16) while being more hardware-friendly. We use 3-bit quantization with group size 128.

Figure 3: Bottleneck analysis for Llama-2-7B on NVIDIA RTX 4090. Left : In on-device LLM applications, generation stage is much slower than the context stage. Middle : The generation stage is memory bound and has low arithmetic intensity. W4A16 quantization can effectively improve the arithmetic intensity by 4 × \times. Right : The amount of weight access is orders of magnitude larger than the amount of activation access. Thus, weight-only quantization is more effective for on-device LLMs.

Searching to scale.
To consider both salient and non-salient weights, we choose to automatically search for an optimal (per input channel) scaling factor that minimizes the output difference after quantization for a certain layer. Formally, we want to optimize the following objective:

𝐬 ∗ \displaystyle\mathbf{s}^{*}

= arg ⁡ min 𝐬 ⁡ ℒ ⁡ ( 𝐬 ) \displaystyle=\mathop{\arg\min}_{\mathbf{s}}\mathcal{L}(\mathbf{s})

(4)

ℒ ( 𝐬 ) = ∥ Q ( 𝐖 ⋅ di \displaystyle\mathcal{L}(\mathbf{s})=\lVert Q(\mathbf{W}\cdot\text{di}

ag ( 𝐬 ) ) ( diag ( 𝐬 ) − 𝟏 ⋅ 𝐗 ) − 𝐖𝐗 ∥ \displaystyle\text{ag}(\mathbf{s}))(\mathbf{\text{diag}(s)^{-1}}\cdot\mathbf{X})-\mathbf{W}\mathbf{X}\rVert

Here Q Q means the weight quantization function ( e.g., INT3/INT4 quantization with group size 128), 𝐖 \mathbf{W} is the original weights in FP16, and 𝐗 \mathbf{X} is the input features cached from a small calibration set (we take a small calibration set from he pre-training dataset in order not to overfit to a specific task). 𝐬 \mathbf{s} is a per-(input) channel scaling factor; for 𝐬 − 𝟏 ⋅ 𝐗 \mathbf{s^{-1}}\cdot\mathbf{X}, it can usually be fused into the previous operator Wei et al. 2022b; Xiao et al. 2022.
Since the quantization function is not differentiable, we are not able to directly optimize the problem with vanilla backpropagation. There are some techniques relying on approximated gradients Bengio et al. 2013; Esser et al. 2019, which we found still suffers from unstable convergence.

To make the process more stable, we define a search space for the optimal scale by analyzing the factors that will affect the choice of scaling factor. As shown in the last section, the saliency of weight channels is actually determined by the activation scale (thus “activation-awareness”). Therefore, we simply use a very simple search space:

𝐬 = 𝐬 𝐗 α, α ∗ = arg ⁡ min α ⁡ ℒ ⁡ ( 𝐬 𝐗 α ) \mathbf{s}=\mathbf{s_{X}}^{\alpha},\quad\alpha^{*}=\mathop{\arg\min}_{\alpha}\mathcal{L}(\mathbf{s_{X}}^{\alpha})

(5)

𝐬 𝐗 \mathbf{s_{X}} is the average magnitude of activation (per-channel), and we use a single hyper-parameter α \alpha to balance between the protection of salient and non-salient channels. We can find the best α \alpha by a fast grid search over the interval of [ 0, 1 ] [0,1] ( 0 0 means we do not scale; 1 1 corresponds to the most aggressive scaling in our search space). We further apply weight clipping to minimize the MSE error of quantization. We provide an ablation study on OPT models under INT3-g128 quantization in Table 5; AWQ consistently outperforms round-to-nearest quantization (RTN) and achieves comparable performance as mixed-precision (1% FP16) while being more hardware-friendly.

Advantages.
Our method does not rely on any regression Frantar et al. 2022 or backpropagation, which is required by many quantization-aware training methods. It has minimal reliance on the calibration set since we only measure the average magnitude per channel, thus preventing over-fitting (Figure 8 ). Therefore, our method requires fewer data for the quantization process and can preserve LLMs’ knowledge outside of the calibration set’s distribution.
See Section 5.3 for more details.

4 TinyChat: Mapping AWQ onto Edge Platforms

AWQ can substantially reduce the size of LLMs. However, converting the theoretical memory savings from W4A16 (4-bit weight, 16-bit activation) quantization into measured speedup is non-trivial. Alternative W8A8 quantization methods, such as SmoothQuant Xiao et al. 2022, maintain the same data precision for both storage and computation. This allows the dequantization procedure to be seamlessly integrated into the computation kernel’s epilogue. On the other hand, W4A16 quantization employs different data types for memory access and computation. As a result, its dequantization must be incorporated into the primary computation loop for optimal performance, posing implementation challenges. To tackle this, we introduce TinyChat: a nimble system for AWQ model inference. It boasts a PyTorch frontend and a backend harnessing device-specific instruction sets (e.g., CUDA/PTX, Neon, AVX).

4.1 Why AWQ Helps Accelerate On-Device LLMs

Figure 4: SIMD-aware weight packing for ARM NEON with 128-bit SIMD units. Original weights are reordered and packed to align with the bit width so that the weights can be unpacked into bytes at runtime using AND and shift bitwise operations with a 128-bit mask.

To understand the acceleration opportunities in quantized LLMs on the edge, we start by profiling the latency breakdown of LLaMA-7B Touvron et al. 2023a model on an RTX 4090 GPU. We adopt an inference batch size of 1, catering for edge use cases, and implement the model in FP16 with NVIDIA FasterTransformer.

Context vs generation latency.

As in Figure 3 (a), it takes 310 ms to generate 20 tokens, while summarizing a prompt with 200 tokens only takes 10 ms. Consequently, the generation phase is substantially slower than the context stage, particularly for on-device interactive applications.

Generation stage is memory-bound.

To accelerate the generation phase, we conduct a roofline analysis in Figure 3 (b). The 4090 GPU has a peak computation throughput of 165 TFLOPS and a memory bandwidth of 1TB/s. Therefore, any workload with arithmetic intensity (the ratio of FLOPs to memory access) less than 165 is memory bounded on 4090 GPUs. Notably, when executed in FP16, the generation stage for on-device LLMs has arithmetic intensity ≈ \approx 1. This underscores the memory-bound nature of the workload. Since the FLOPs of a given model is fixed, the only way to improve the peak performance is to reduce the total amount of memory traffic. AWQ reduces the weight memory by four times.

Weight access dominates memory traffic.

We therefore further break down the memory access for weight and activation in Figure 3 (c). Clearly, weight access dominates the memory traffic for on-device LLMs. Quantizing the model weights to 4 bit integers will approximately increase the arithmetic intensity to 4 FLOPs/Byte, leading to a 4TFLOPS peak performance in Figure 3 (b). Since weight-only quantization leads to a lower bit width for weights (and thus higher theoretical performance upper bound), it is natural for AWQ to follow this setting for on-device LLM applications.

4.2 Deploy AWQ with TinyChat

To this end, we demonstrated that 4-bit weight quantization could lead to a 4 × \times theoretical peak performance. We further design TinyChat to realize this speedup. On GPUs, we only focus on implementing essential components, including attention, layer normalization, and linear projection kernels. The flexible frontend allows easy customization and fast support for new models. TinyChat with 4-bit AWQ achieves more than 3 × \times speedup compared with the Huggingface FP16 implementation across different families of LLMs on GPUs. On CPUs, we lower the entire computation graph to C++ to minimize overhead.

On-the-fly weight dequantization.

For quantized layers, as the hardware does not provide multiplication instructions between INT4 and FP16, we need to dequantize the integers to FP16 before performing matrix computation. We avoid writing dequantized weights into DRAM by fusing dequantization kernels with the matrix multplication kernel. Note that such fusion is adopted for both matrix-matrix (MM) and matrix-vector (MV) product kernels.

SIMD-aware weight packing.

On-the-fly weight dequantization reduces intermediate DRAM access, but remains expensive. For instance, dequantizing a single 4-bit weight involves 1 shift, 1 bitwise AND, and 1 FMA scaling operations, while the dequantized weight undergoes only 1 FMA computation. This process is particularly costly on CPUs with SIMD architecture that favor vectorized instructions. To mitigate this, we suggest platform-specific weight packing tailored to the bitwidth of a device’s SIMD units. Figure 4 demonstrates our strategy for ARM CPUs with 128-bit SIMD registers offering up to 1.2 × \times speedup. Here, each register holds 32 4-bit weights, sequenced as w 0, w 16, w 1, w 17, …, w 15, w 31 w_{0},w_{16},w_{1},w_{17},...,w_{15},w_{31}. This approach requires just three SIMD instructions to unpack all 32 weights, as opposed to 3 scalar instructions per weight in a conventional packing ( w 0, w 1, …, w 31 w_{0},w_{1},...,w_{31} ). Generally, for 2 n 2^{n} -bit SIMD registers, adjacent weights will have indices off by 1 / 8 × 2 n 1/8\times 2^{n}, since each register can hold 1 / 8 × 2 n 1/8\times 2^{n} 8-bit integers. On GPUs, we found it more efficient to pack each 8 weights into w { 0, 2, 4, 6, 1, 3, 5, 7 } w_{\{0,2,4,6,1,3,5,7\}} following Kim et al. 2022.

Kernel fusion.

We also extensively apply kernel fusion to optimize on-device LLM inference. For layer normalization, we fuse all operators ( e.g. multiplication, division and square root) into a single kernel. For attention layers, we fuse QKV projections into a single kernel, and also perform on-the-fly positional embedding calculation. We also pre-allocate KV caches and perform cache updates within the attention kernel. Kernel fusion is particularly useful for models with inefficient forward pass implementations, such as Falcon Penedo et al. 2023 and StarCoder Li et al. 2023c. Notably, the computation time for each FP16 kernel is in the order of 0.01ms on the 4090 GPU, comparable to the GPU kernel launch overhead. Hence, reducing number of kernel calls through kernel fusion leads to direct speedups.

5 Experiments

PPL ↓ \downarrow

Llama-2   

LLaMA   

7B

13B

70B

7B

13B

30B

65B

FP16

-

5.47

4.88

3.32

5.68

5.09

4.10

3.53

INT3
g128

RTN

6.66

5.52

3.98

7.01

5.88

4.88

4.24

GPTQ

6.43

5.48

3.88

8.81

5.66

4.88

4.17

GPTQ-R

6.42

5.41

3.86

6.53

5.64

4.74

4.21

AWQ

6.24

5.32

3.74

6.35

5.52

4.61

3.95

INT4
g128

RTN

5.73

4.98

3.46

5.96

5.25

4.23

3.67

GPTQ

5.69

4.98

3.42

6.22

5.23

4.24

3.66

GPTQ-R

5.63

4.99

3.43

5.83

5.20

4.22

3.66

AWQ

5.60

4.97

3.41

5.78

5.19

4.21

3.62

Table 4: AWQ improves over round-to-nearest quantization (RTN) for different model sizes and different bit-precisions. It consistently achieves better perplexity than GPTQ (w/ and w/o reordering) on LLaMA & Llama-2 models.

Wikitext2 PPL ↓ \downarrow

Mixtral-8x7B

Mistral-7B

FP16

5.94

4.14

INT4-g128

6.05

4.30

INT3-g128

6.52

4.83

Table 5: AWQ quantization results on Mistral-7B-Instruct-v0.2 Jiang et al. 2023 and Mixtral-8x7B-Instruct-v0.1 model Jiang et al. 2024. The PPL result on wikitext shows that AWQ can achieve superior quantization performance on different model architectures including LLMs with GQA and Mixture-of-Experts (MoE) models.

5.1 Settings

Quantization.

We focus on weight-only grouped quantization in this work. As shown in previous work Dettmers & Zettlemoyer 2022; Frantar et al. 2022, grouped quantization is always helpful for improving performance/model size trade-off. We used a group size of 128 throughout the work, except otherwise specified. We focus on INT4/INT3 quantization since they are able to mostly preserve the LLMs’ performance Dettmers & Zettlemoyer 2022. For AWQ, we used a small calibration set from the Pile Gao et al. 2020 dataset in order not to overfit to a specific downstream domain. We used a grid size of 20 to search for the optimal α \alpha in Equation 5.

Models.

We benchmarked our method on LLaMA Touvron et al. 2023a and OPT Zhang et al. 2022 families. There are other open LLMs like BLOOM Scao et al. 2022, but they are generally worse in quality, so we do not include them in our study. We further benchmark an instruction-tuned model Vicuna Chiang et al. 2023 and visual language models OpenFlamingo-9B Awadalla et al. 2023 and LLaVA-13B Liu et al. 2023a to demonstrate the generability of our method.

Evaluations.

Following previous literature Dettmers et al. 2022; Xiao et al. 2022; Frantar et al. 2022; Dettmers & Zettlemoyer 2022; Yao et al. 2022, we mainly profiled the quantized models on language modeling tasks (perplexity evaluation on WikiText-2 Merity et al. 2016 ) since perplexity can stably reflect the LLM’s performance Dettmers & Zettlemoyer 2022.

Baselines.

Our primary baseline is vanilla round-to-nearest quantization (RTN). It is actually quite strong when using a small group size like 128 Frantar et al. 2022; Dettmers & Zettlemoyer 2022. We also compare with a state-of-the-art method GPTQ Frantar et al. 2022 for LLM weight quantization. For GPTQ, we also compare with an updated version that uses a “reorder” trick (denoted as GPTQ-Reorder or GPTQ-R). Other techniques like ZeroQuant Yao et al. 2022, AdaRound Nagel et al. 2020, and BRECQ Li et al. 2021 rely on backpropagation to update the quantized weights, which may not easily scale up to large model sizes; they also do not outperform GPTQ Frantar et al. 2022, thus not included for study.

5.2 Evaluation

Figure 5: Comparing INT3-g128 quantized Vicuna models with FP16 counterparts under GPT-4 evaluation protocol Chiang et al. 2023. More winning cases (in
blue ) indicate better performance. AWQ consistently improves the quantized performance compared to RTN and GPTQ Frantar et al. 2022, showing generalization to instruction-tuned models.

COCO (CIDEr ↑ \uparrow )

0-shot

4-shot

8-shot

16-shot

32-shot

Δ \Delta (32-shot)

FP16

-

63.73

72.18

76.95

79.74

81.70

-

INT4
g128

RTN

60.24

68.07

72.46

74.09

77.13

-4.57

GPTQ

59.72

67.68

72.53

74.98

74.98

-6.72

AWQ

62.57

71.02

74.75

78.23

80.53

-1.17

INT3
g128

RTN

46.07

55.13

60.46

63.21

64.79

-16.91

GPTQ

29.84

50.77

56.55

60.54

64.77

-16.93

AWQ

56.33

64.73

68.79

72.86

74.47

-7.23

Table 6: Quantization results of a visual language model OpenFlamingo-9B Awadalla et al. 2023 on COCO Captioning datasets. Activation-aware Weight Quantization outperforms existing methods under zero-shot and various few-shot settings, demonstrating the generability to different modalities and in-context learning workloads. Activation-aware Weight Quantization reduces the quantization degradation (32-shot) from 4.57 to 1.17 under INT4-g128, providing 4 × \times model size reduction with negligible performance loss.

Model (Accuracy ↑ \uparrow )

VQAv2

GQA

VizWiz

SQA-I

VQA-T

POPE

MME

MMB

SEED

llava-bench

MM-Vet

VILA-7B

80.3

63.1

59.6

68.0

62.6

86.3

1489.4

69.8

61.7

75.2

35.1

VILA-7B-AWQ

80.1

63.0

57.8

68.0

61.9

85.3

1486.3

68.8

61.3

75.8

35.9

VILA-13B

80.5

63.6

63.1

70.5

64.0

86.3

1553.6

73.8

62.8

78.3

42.6

VILA-13B-AWQ

80.4

63.6

63.0

71.2

63.5

87.0

1552.9

73.6

62.2

77.6

42.0

Table 7: INT4-g128 results of VILA-7B and VILA-13B Lin et al. 2024 on 11 visual-language benchmarks. AWQ consistently shows lossless performance on all benchmarks. Benchmark names are abbreviated due to space limits. VQA-v2 Goyal et al. 2017; GQA Hudson & Manning 2019; VisWiz Gurari et al. 2018; SQA I {}^{\text{I}} : ScienceQA-IMG Lu et al. 2022; VQA T {}^{\text{T}} : TextVQA Singh et al. 2019; POPE Li et al. 2023d; MME Fu et al. 2023; MMB: MMBench Liu et al. 2023b; MMB CN {}^{\text{CN}} : MMBench-Chinese Liu et al. 2023b; SEED: SEED-Bench Li et al. 2023a; LLaVA W {}^{\text{W}} : LLaVA-Bench (In-the-Wild) Liu et al. 2023a; MM-Vet Yu et al. 2023.

Results on LLaMA models.

We focus on LLaMA models (LLaMA Touvron et al. 2023a and Llama-2 Touvron et al. 2023b ) due to their superior performance compared to other open-source LLMs Zhang et al. 2022; Scao et al. 2022; it is also the foundation of many popular open-source models Taori et al. 2023; Chiang et al. 2023. We evaluate the perplexity before and after quantization in Table 4.
AWQ consistently outperforms round-to-nearest (RTN) and GPTQ Frantar et al. 2022 (w/ and w/o reordering) across different model scales (7B-70B) and generations.

Results on Mistral / Mixtral models.

We also evaluated AWQ on the Mistral and Mixtral models, which are among the most popular open-source LLMs and Mixture-of-Experts (MoE) models, respectively Jiang et al. 2023; Jiang et al. 2024. The results indicate that AWQ achieves superior performance on both the Mistral and Mixtral models. This demonstrates that AWQ is effective across various model architectures.

Figure 6: Visual reasoning examples from LLaVA-13B model Liu et al. 2023a. AWQ improves over the round-to-nearest (RTN) baseline, providing more reasonable answers. We color the text to show the correct or wrong responses.

Figure 7: Qualitative results of quantized OpenFlamingo-9B Awadalla et al. 2023 on COCO captioning dataset (4-shot, INT4-g128 quantization). Our method significantly improves the captioning quality compared to the round-to-nearest (RTN) baseline. We color the text to show the correct or wrong captions.

Quantization of instruction-tuned models.

Instruction tuning can significantly improve the models’ performance and usability
Wei et al. 2021; Sanh et al. 2021; Ouyang et al. 2022; Chung et al. 2022. It has become an essential procedure before model deployment. We further benchmark our method’s performance on a popular instruction-tuned model Vicuna Chiang et al. 2023 in Figure 5. We used the GPT-4 score to evaluate the quantized models’ performance against the FP16 counterpart on 80 sample questions Chiang et al. 2023.
We compare the responses with both orders (quantized-FP16, FP16-quantized) to get rid of the ordering effect (we found GPT-4 tends to increase the rating of the first input), leading to 160 trials.
AWQ consistently improves the INT3-g128 quantized Vicuna models over RTN and GPTQ under both scales (7B and 13B), demonstrating the generability to instruction-tuned models.

MBPP (7B)

pass@1

pass@10

FP16

38.53

49.77

RTN

37.51

48.49

GPTQ

31.97

44.75

AWQ

40.64

49.25

GSM8K

7B

13B

70B

FP16

13.87

26.16

56.41

RTN

11.07

21.23

53.98

GPTQ

12.13

24.26

56.03

AWQ

13.57

25.25

56.40

Table 8: INT4-g128 quantization results of CodeLlama-7b-Instruct-hf on MBPP dataset and Llama-2 (7B/13B/70B) on GSM8K dataset. AWQ outperforms existing methods on programming and math datasets, demonstrating the generability to different scenarios and evaluation settings. Notably, AWQ under the INT4-g128 configuration demonstrates comparable performance to the original FP16 model across both datasets.

Quantization of multi-modal language models.

Large multi-modal models (LMMs) or visual language models (VLMs) are LLMs augmented with vision inputs Alayrac et al. 2022; Li et al. 2023b; Koh et al. 2023; Driess et al. 2023; Zhang et al. 2023; Liu et al. 2023a. Such models are able to perform text generation conditioned on image/video inputs. Since our method does not have the overfitting issue to the calibration set, it can be directly applied to VLMs to provide accurate and efficient quantization.
We perform experiments with the OpenFlamingo-9B model Awadalla et al. 2023 (an open-source reproduction of Alayrac et al. 2022 ) on COCO captioning Chen et al. 2015 dataset (Table 6 ). We measured the average performance of 5k samples under different few-shot settings. We only quantize the language part of the model since it dominates the model size.
AWQ outperforms existing methods under zero-shot and various few-shot settings, demonstrating the generability to different modalities and in-context learning workloads. It reduces the quantization degradation (32-shot) from 4.57 to 1.17 under INT4-g128, providing 4 × \times model size reduction with negligible performance loss.
To further demonstrate the generability of AWQ, we also evaluated AWQ on one of the SoTA multi-image visual language models: VILA. The result in Table 7 shows that AWQ achieves lossless quantization performance on 11 visual-language benchmarks.
We further provide some qualitative captioning results in Figure 7 to show our advantage over RTN.
Our method provides a push-the-button solution for LMM/VLM quantization. It is the first study of VLM low-bit quantization to the best of our knowledge.

Figure 8: Left: AWQ needs a much smaller calibration set to reach a good quantized performance. It can achieve better perplexity using 10 × \times smaller calibration set compared to GPTQ. Right: Our method is more robust to the calibration set distribution. Overall, using the same calibration and evaluation distribution works the best (PubMed-PubMed, Enron-Enron). But when using a different calibration distribution (PubMed-Enron, Enron-PubMed), AWQ only increases the perplexity by 0.5-0.6, while GPTQ has 2.3-4.9 worse perplexity. All experiments are done with the OPT-6.7B model under INT3-g128 quantization.

Figure 9: TinyChat provides a turn-key solution to transform the theoretical memory footprint reduction into a quantifiable speedup. As a result, TinyChat is up to 3.9 × \times and 3.5 × \times faster than the FP16 implementation from Huggingface on 4090 (desktop GPU) and Orin (mobile GPU), respectively. AWQ also democratizes Llama-2-13B deployment on laptop GPUs (4070) with merely 8GB memory.

Figure 10: TinyChat offers 1.2-3.0 × \times speedup over existing systems when running 4-bit quantized Llama models on NVIDIA Jetson Orin. It also supports a diverse range of general-purpose and coding-specific LLMs with at least 2.6 × \times speedup over AutoGPTQ, which also supports all these workloads. Moreover, TinyChat seamlessly operates on Raspberry Pi and enables the deployment of LLMs with up to 7 billion parameters on extremely resource-constrained IoT devices.

OPT (Wiki PPL ↓ \downarrow )

1.3B

2.7B

6.7B

13B

30B

FP16

14.62

12.47

10.86

10.13

9.56

RTN

10476

193210

7622

17564

8170

GPTQ

46.67

28.15

16.65

16.74

11.75

AWQ +GPTQ

35.71

25.70

15.71

13.25

11.38

Table 9: Our method is orthogonal to GPTQ: it further closes the performance gap under extreme low-bit quantization (INT2-g64) when combined with GPTQ. Results are WikiText-2 perplexity of OPT models.

Visual reasoning results.

We further provide some qualitative visual reasoning examples of the LLaVA-13B Liu et al. 2023a model in Figure 6. AWQ improves the responses compared to round-to-nearest (RTN) for INT4-g128 quantization, leading to more reasonable answers. In this first example, the AWQ model can understand the meme as it resembles the Earth when looking from space, while RTN produces wrong descriptions (marked in red ).

Results on programming and math tasks

To further evaluate the performance of AWQ on tasks involving complex generations, we also tested AWQ on MBPP Austin et al. 2021 and GSM8K Cobbe et al. 2021. MBPP Austin et al. 2021 consists of around 1,000 Python programming problems, designed to be solvable by entry level programmers, covering programming fundamentals, standard library functionality, etc. GSM8K Cobbe et al. 2021 was created to support the task of question answering on basic mathematical problems that require multi-step reasoning. We quantize CodeLlama-7b-Instruct-hf and Llama-2 to INT4-g128 and perform experiments on programming and math datasets (Table 8 ). AWQ outperforms existing methods on both datasets, demonstrating the generability to complex generation. AWQ under the INT4-g128 configuration demonstrates comparable
performance to the original FP16 model on both datasets.

Extreme low-bit quantization.

We further quantize LLM to INT2 to accommodate limited device memory (Table 9 ). RTN completely fails, and AWQ brings significant perplexity improvement on top of GPTQ.Our method is orthogonal to GPTQ.
We can combine our method with GPTQ to further improve the INT2 quantization performance, making it a more practical setting.

5.3 Data Efficiency and Generalization

Better data-efficiency for the calibration set.

Our method requires a smaller calibration set since we do not rely on regression/backpropagation; we only measure the average activation scale from the calibration set, which is data-efficient.
To demonstrate the idea, we compare the perplexity of the OPT-6.7B model with INT3-g128 quantization in Figure 8 (a). AWQ needs a much smaller calibration to reach a good quantized performance; it can achieve better perplexity using 10 × \times smaller calibration set compared to GPTQ (16 sequences v.s. 192 sequences).

Robust to the calibration set distributions.

Our method is less sensitive to the calibration set distribution since we only measure the average activation scale from the calibration set, which is more generalizable across different dataset distributions. We further benchmarked the effect of the different calibration set distributions in Figure 8 (b). We took
