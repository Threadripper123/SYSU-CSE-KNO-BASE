# Taming the Titans：高效 LLM 推理服务综述（2025）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　50,137 字符　｜　374 个片段　｜　更新于 2026-11-13
来源: https://arxiv.org/html/2504.19720v1

# https://arxiv.org/html/2504.19720v1

来源: https://arxiv.org/html/2504.19720v1
抓取方式: read_page

Report GitHub Issue

×

Title:

Content selection saved. Describe the issue below:

Description:

Submit without GitHub
Submit in GitHub

arXiv is now an independent nonprofit!
Learn more
&times;

Back to arXiv

Why HTML?

Report Issue

Back to Abstract

Download PDF

Abstract

1 Introduction

2 Background

2.1 Transformer-based LLM

2.2 Inference

2.3 Evaluation

3 LLM Inference Serving in Instance

3.1 Model Placement

Model Parallelism.

Offloading.

3.2 Request Scheduling

Inter-Request Scheduling

Intra-Request Scheduling

3.3 Decoding Length Prediction

Exact Length Prediction.

Range-Based Classification.

Relative Ranking Prediction.

3.4 KV Cache Optimization

Memory Management.

Reuse Strategies.

Compression Techniques.

3.5 PD Disaggregation

4 LLM Inference Serving in Cluster

4.1 Cluster Optimization

Architecture and Optimization for Heterogeneous Resources.

Service-Aware Scheduling.

4.2 Load Balancing

Heuristic Algorithm.

Dynamic Scheduling.

Intelligent Predictive Scheduling.

4.3 Cloud-Based LLM Serving

Deployment and Computing Effective.

Cooperation with Edge Device.

5 Emerging Scenarios

5.1 Long Context

Parallel Processing.

Attention Computation.

KV Cache Management.

5.2 RAG

Workflow Scheduling.

Storage Optimization.

5.3 MoE

Expert Placement.

Expert Load Balancing.

All-to-All Communication.

5.4 LoRA

5.5 Speculative Decoding

5.6 Augmented LLMs

5.7 Test-Time Reasoning

6 Miscellaneous Areas

6.1 Hardware

6.2 Privacy

6.3 Simulator

6.4 Fairness

6.5 Energy

7 Future Works

8 Conclusion

References

License: CC BY 4.0

arXiv:2504.19720v1 [cs.CL] 28 Apr 2025

Taming the Titans: A Survey of Efficient LLM Inference Serving

Ranran Zhen

Affiliation: Soochow University

Email: xiaqingrong@huawei.com

  
Juntao Li
† † thanks: Corresponding author
Affiliation: Soochow University

Email: duanxinyu@huawei.com

  
Yixin Ji

Affiliation: Soochow University

Email: wangzhefeng@huawei.com

  
Zhenlin Yang

Affiliation: Soochow University

Email: huaibaoxing@huawei.com

  
Tong Liu

Affiliation: Soochow University

  
Qingrong Xia

Affiliation: Huawei Cloud {zenrran,jiyixin169}@gmail.com {ljt,minzhang}@suda.edu.cn

  
Xinyu Duan

Affiliation: Huawei Cloud {zenrran,jiyixin169}@gmail.com {ljt,minzhang}@suda.edu.cn

  
Zhefeng Wang

Affiliation: Huawei Cloud {zenrran,jiyixin169}@gmail.com {ljt,minzhang}@suda.edu.cn

  
Baoxing Huai

Affiliation: Huawei Cloud {zenrran,jiyixin169}@gmail.com {ljt,minzhang}@suda.edu.cn

  
Min Zhang

Affiliation: Soochow University

Abstract

Large Language Models (LLMs) for Generative AI have achieved remarkable progress, evolving into sophisticated and versatile tools widely adopted across various domains and applications.
However, the substantial memory overhead caused by their vast number of parameters, combined with the high computational demands of the attention mechanism, poses significant challenges in achieving low latency and high throughput for LLM inference services.
Recent advancements, driven by groundbreaking research, have significantly accelerated progress in this field.
This paper provides a comprehensive survey of these methods, covering fundamental instance-level approaches, in-depth cluster-level strategies, emerging scenario directions, and other miscellaneous but important areas.
At the instance level, we review model placement, request scheduling, decoding length prediction, storage management, and the disaggregation paradigm.
At the cluster level, we explore GPU cluster deployment, multi-instance load balancing, and cloud service solutions.
For emerging scenarios, we organize the discussion around specific tasks, modules, and auxiliary methods.
To ensure a holistic overview, we also highlight several niche yet critical areas.
Finally, we outline potential research directions to further advance the field of LLM inference serving 1 1
1

https://github.com/zenrran4nlp/Awesome-LLM-Inference-Serving.

1 Introduction

With the rapid evolution of open-source Large Language Models (LLMs), weekly updates to model architectures and capabilities have become the norm in recent years.
The surging demand for these models is evident from Huggingface download statistics, which range from hundreds of thousands for models like Mistral-Small-24B-Instruct-2501 Mistral (2025), phi-4 Abdin et al. (2024), and Llama-3.3-70B-Instruct Grattafiori et al. (2024) to millions for DeepSeek-V3 DeepSeek-AI et al. (2024) and DeepSeek-R1 DeepSeek-AI et al. (2025) over recent months.
However, when deploying these models, their large-scale parameters and attention mechanisms impose substantial demands on memory and computational resources, presenting significant obstacles to achieving the desired low latency and high throughput in processing requests.
These challenges have spurred extensive research across multiple domains of inference serving optimization to meet Service Level Objectives (SLOs).

This paper presents a systematic survey of LLM inference serving methods, organized hierarchically from instance-level optimizations and cluster-scale strategies to emerging scenarios and miscellaneous areas, as illustrated in Figure 1.

Instance-Level optimization (§ 3 ) begins with model placement (§ 3.1 ), essential for distributing parameters across devices when single-GPU memory is insufficient.
Subsequent request scheduling (§ 3.2 ) prioritizes batched processing through decoding length prediction (§ 3.3 ), where shorter requests are prioritized to reduce overall latency.
Dynamic batch management then governs request insertion/eviction during iterative processing.
While KV cache (§ 3.4 ) mitigates redundant computation, challenges persist in storage efficiency, reuse strategies, and compression.
Due to the distinction between the prefill and decoding phases, the disaggregated architecture (§ 3.5 ) was introduced, facilitating the optimization of each phase.

Figure 1: Overview of the paper, detailing Instance, Cluster, Emerging Scenarios, and Miscellaneous Areas. 𝐑 \mathbf{R} represents a request. In Inter-request scheduling, two requests, 𝐑𝟏 \mathbf{R1} (10 toks) and 𝐑𝟐 \mathbf{R2} (2 toks), arrive simultaneously. Ignoring the prefill process, if 𝐑𝟏 \mathbf{R1} is processed first, its generation rate is 1 tok/s, and 𝐑𝟐 \mathbf{R2} ’s rate is 0.2 tok/s. Reversing the order gives 𝐑𝟐 \mathbf{R2} a rate of 1 tok/s and 𝐑𝟏 \mathbf{R1} 0.9 tok/s. The default decoding speed is 1 token/s.

Cluster-Level optimization focuses on deployment strategies (§ 4 ), particularly cost-effective GPU cluster configurations with heterogeneous hardware, as well as service-oriented cluster scheduling (§ 4.1 ).
Scalability introduces load balancing challenges (§ 4.2 ) to prevent resource underutilization or overload across distributed instances.
When local hardware infrastructure is inadequate to fulfill deployment requirements, cloud-based solutions (§ 4.3 ) are necessary to address dynamic LLM serving demands.

Emerging Scenarios (§ 5 ) include advanced tasks such as Long Context processing (§ 5.1 ), as well as techniques like Retrieval-Augmented Generation (RAG) (§ 5.2 ), Mixture of Experts (MoE) (§ 5.3 ), Low-Rank Adaptation (LoRA) (§ 5.4 ), Speculative Decoding (§ 5.5 ), Augmented LLMs (§ 5.6 ), and Test-Time Reasoning (§ 5.7 ), all of which require adaptability to address evolving demands.

Lastly, we also provide a detailed overview of Miscellaneous Areas (§ 6 ) that are niche but critical, covering Hardware (§ 6.1 ), Privacy (§ 6.2 ), Simulator (§ 6.3 ), Fairness (§ 6.4 ), and Energy (§ 6.5 ), aiming to foster more holistic progress in the field.

Prior surveys Miao et al. (2023); Yuan et al. (2024); Zhou et al. (2024); Li et al. (2024a) have laid important groundwork but face limitations in depth, breadth, or timeliness given the field’s rapid progress.
Our work addresses these gaps through a systematic, fine-grained taxonomy of cutting-edge methods, complemented by forward-looking research directions.
Finally, we adopt a forward-looking perspective and highlight several promising directions for future research.

2 Background

This section provides an overview of LLM fundamentals, aimed at enhancing the understanding of inference serving, along with the relevant evaluation metrics.

Metric

Definition

Key Focus

TTFT

Latency from input to first token.

Critical for real-time apps (e.g., chatbots).

TBT

Time interval between consecutive tokens.

Reflects step-by-step responsiveness.

TPOT

Average time per token during decoding.

Measures token generation efficiency.

Throughput

Tokens generated per second across all requests.

Evaluates system capacity under high load.

Capacity

Maximum throughput while meeting SLOs.

Represents system’s upper performance limit.

Normalized Latency

Total execution time divided by token count.

Holistic view of system efficiency.

Percentile Metrics

Latency distribution (e.g., P50, P90, P99).

Evaluates stability and performance bounds.

Table 1: LLM Inference Service Evaluation Metrics.

2.1 Transformer-based LLM

The LLM is primarily constructed on the foundation of the vanilla Transformer architecture, with a particular emphasis on its decoding component.
The architecture is composed of multiple layers, primarily consisting of two key components: Multi-Head Self-Attention (MHA) and Feedforward Network (FFN), complemented by the LayerNorm operation.

The input representation 𝐗 = { 𝐱 1, …, 𝐱 n } \mathbf{X}=\{\mathbf{x}_{1},\dots,\mathbf{x}_{n}\} of the model is initially processed by tokenizing the user input and incorporating positional information.
Subsequently, it is transformed through three learnable weight matrices, denoted as 𝐖 Q \mathbf{W}^{Q}, 𝐖 K \mathbf{W}^{K}, and 𝐖 V \mathbf{W}^{V}, to obtain the corresponding query ( 𝐐 \mathbf{Q} ), key ( 𝐊 \mathbf{K} ), and value ( 𝐕 \mathbf{V} ) vectors which are utilized as inputs for the subsequent MHA:

MHA ​ ( 𝐐, 𝐖, 𝐕 ) = Softmax ​ ( 𝐐𝐊 T 𝐝 k ) ​ 𝐕 𝐐 = 𝐗𝐖 Q; 𝐊 = 𝐗𝐖 K; 𝐕 = 𝐗𝐖 V \begin{split}\text{MHA}(\mathbf{Q},\mathbf{W},\mathbf{V})&=\text{Softmax}(\frac{\mathbf{Q}\mathbf{K}^{T}}{\sqrt{\mathbf{d}_{k}}})\mathbf{V}\\
\mathbf{Q}=\mathbf{X}\mathbf{W}^{Q};\penalty\ \mathbf{K}&=\mathbf{X}\mathbf{W}^{K};\penalty\ \mathbf{V}=\mathbf{X}\mathbf{W}^{V}\end{split}

(1)

where 𝐝 k \mathbf{d}_{k} denotes the dimensionality of each attention head.
It is evident that this constitutes the most time-consuming component, with a time complexity of 𝒪 ⁡ ( n 2 ) \mathcal{O}(n^{2}).
The model processes 𝐦 \mathbf{m} heads separately and concatenates the results:

𝐎 = Concat ​ ( 𝐇 1, 𝐇 2, …, 𝐇 m ) ​ 𝐖 O 𝐇 i = MHA ​ ( 𝐐 i, 𝐖 i, 𝐕 i ) \begin{split}\mathbf{O}&=\text{Concat}(\mathbf{H}_{1},\mathbf{H}_{2},\dots,\mathbf{H}_{m})\mathbf{W}^{O}\\
\mathbf{H}_{i}&=\text{MHA}(\mathbf{Q}_{i},\mathbf{W}_{i},\mathbf{V}_{i})\end{split}

(2)

The FFN applies two linear transformations to its input, which is first processed by LayerNorm and residual connection:

FNN ​ ( 𝐱 ) = max ​ ( 0, 𝐱𝐖 1 + 𝐛 1 ) ​ 𝐖 2 + 𝐛 2 \begin{split}\text{FNN}(\mathbf{x})&=\text{max}(0,\mathbf{x}\mathbf{W}_{1}+\mathbf{b}_{1})\mathbf{W}_{2}+\mathbf{b}_{2}\end{split}

(3)

Figure 2: Illustration of the LLM Inference process.

2.2 Inference

LLM inference involves two phases: prefill and decoding, as illustrated in Figure 2.
In prefill, the model processes the entire input in a compute-bound forward pass to produce the first token, while caching 𝐊 \mathbf{K} and 𝐕 \mathbf{V} ( KV cache ) to avoid recomputation.
During decoding, tokens are generated sequentially using KV cache (gray blocks), which reduces the time complexity to 𝒪 ⁡ ( n ) \mathcal{O}(n) at the cost of increased memory overhead.
For each new token 𝐗 n ​ e ​ w \mathbf{X}_{new}, the corresponding 𝐐 n ​ e ​ w \mathbf{Q}_{new}, 𝐊 n ​ e ​ w \mathbf{K}_{new}, and 𝐕 n ​ e ​ w \mathbf{V}_{new} are computed, ensuring efficient generation and terminate at the [EOS] token.

2.3 Evaluation

These are the conventional metrics Agarwal et al. (2023); Zhong et al. (2024); Qin et al. (2024); Yu et al. (2022) listed in Table 1.
In addition, Goodput Zhong et al. (2024), or “effective throughput”, measures the maximum request rate that meets SLOs.
Etalon Agrawal et al. (2024a) is used to evaluate fluency to maintain smooth output during real-time interactions and its maximum output rate while preserving a certain level of fluency.

{forest}

Figure 3: Taxonomy of Instance-Level approaches for LLM inference serving.

3 LLM Inference Serving in Instance

This section covers deployment, scheduling, decoding length prediction, memory management, and innovative architecture, as shown in Figure 3.

3.1 Model Placement

Due to the large number of parameters in LLMs, which exceed a single GPU’s capacity, distributing them across multiple GPUs or offloading them to CPUs has become a common practice.

Model Parallelism.

This section focuses on two core parallelism strategies: pipeline parallelism and tensor parallelism.
Pipeline parallelism (e.g., GPipe Huang et al. (2019), PipeDream Harlap et al. (2018), and Megatron-LM Narayanan et al. (2021) ) distributes distinct model layers across multiple devices, enabling concurrent processing of sequential data to accelerate training/inference.
Tensor parallelism, as implemented in frameworks like Megatron-LM Shoeybi et al. (2020), splits individual operations or layers (e.g., matrix multiplications) into smaller sub-tensors computed in parallel across devices, enhancing computational efficiency and enabling larger model dimensions.

Beyond these, supplementary techniques address specialized fields:
Sequential parallelism Korthikanti et al. (2022) partitions LayerNorm and Dropout activations along the sequence dimension for long-context tasks.
Context parallelism NVIDIA (2024) extends this by splitting all layers along the sequence dimension.
Expert parallelism Fedus et al. (2022) allocates sparse MoE components across GPUs, optimizing memory usage for sparse LLMs. More details can be seen in § 5.3.

Offloading.

When computational resources are limited, a trade-off between GPU and CPU utilization becomes necessary.
Techniques such as ZeRO-Offload Ren et al. (2021), DeepSpeed-Inference Aminabadi et al. (2022), and FlexGen Sheng et al. (2023) address this challenge by storing the majority of a model’s weights in memory or storage devices and loading only the required portions into GPU memory on demand.
PowerInfer’s GPU-CPU hybrid engine Song et al. (2024) preloads hot neurons on the GPU for speed and computes cold neurons on the CPU, cutting GPU memory needs and data transfers.
TwinPilots Yu et al. (2024) proposes a novel computing paradigm that integrates the twin computing engines, GPU and CPU, with the hierarchical memory architecture, including both GPU and CPU memory, within an asymmetric multiprocessing framework.
Park and Egger (2024) propose a technique for efficient resource utilization through dynamic, fine-tuned workload allocation.

3.2 Request Scheduling

For instance, request scheduling directly impacts latency optimization. Here, we review relevant algorithms from both inter-request and intra-request scheduling perspectives.

Inter-Request Scheduling

This part examines the prioritization of request batches during high volumes, focusing on execution order. Current LLM solutions Yu et al. (2022); Stojkovic et al. (2024) and mainstream approaches Qin et al. (2024) mainly use First-Come-First-Served (FCFS), which has limitations. For instance, prioritizing an early, lengthy request over a shorter one can delay the latter, increasing latency (a head-of-line blocking issue Kaffes et al. (2019) ).
Prioritizing shorter requests can help both meet their SLOs.

Advances in decoding length prediction (§ 3.3 ) have led to various scheduling optimizations.
FastServe Wu et al. (2024b) introduces Skip-Join Multi-Level Feedback Queue (MLFQ) scheduler, prioritizing high-priority requests and elevating long-waiting ones, while preempting long-running tasks to accelerate shorter requests.
Fu et al. (2024c) approximate Shortest Job First (SJF) by prioritizing requests with shorter predicted decoding times.
Shahout et al. (2024b) enhance Shortest Remaining Time First (SRTF) by dynamically predicting remaining decoding lengths and introducing a preemption ratio to avoid excessive preemption of long requests. However, this approach requires invoking the length prediction model in nearly every iteration, making the associated overhead a critical concern.
Prophet Saereesitthipitak et al. (2024) employs a Prefill-Decoding (PD) separated architecture, applying SJF in the prefill phase and Skip-Join MLFQ in decoding.
INFERMAX Kim et al. (2024) demonstrates that strategic preemption, guided by inference cost models, reduces GPU costs compared to non-preemptive methods.
In contrast, BatchLLM Zheng et al. (2024b) prioritizes processing requests with global sharing.

Intra-Request Scheduling

This segment explores scheduling within concurrent request batches, aiming to improve parallel decoding efficiency by addressing variability in request arrival, completion times, and output lengths. Orca Yu et al. (2022) introduces iteration-level scheduling, allowing dynamic addition and removal of requests per iteration, offering more flexibility than inter-request scheduling.
The Dynamic SplitFuse Holmes et al. (2024) and the chunked-prefills Agrawal et al. (2024c) partition the prefill stage into smaller segments, merging them with the decoding phase to reduce delays from long prompts and avoid pausing decoding during prefilling.
Similar to prior methods, slice-level scheduling (SCLS) Cheng et al. (2024b) ensures precise control over service time and memory usage by dividing the maximum generation length into fixed-length slices and processing them sequentially.

3.3 Decoding Length Prediction

The uncertainty in generation length makes request scheduling challenging. Recent work on predicting lengths can be categorized into three main areas.

Exact Length Prediction.

This approach predicts exact token counts. Cheng et al. (2024a) link task types to lengths using BERT embeddings and random forest regression, while Hu et al. (2024b) use a small OPT. Qiu et al. (2024b) show simpler regression models work under computational constraints.

Range-Based Classification.

These methods classify requests into length bins.
Zheng et al. (2024a) use supervised fine-tuning to train a model capable of predicting decoding length based on a given prompt.
Jin et al. (2023) and Jain et al. (2024) build DistilBERT classifiers for length categories, while Stojkovic et al. (2024) use short/medium/long bins. μ \mu -Serve ( Qiu et al., 2024a ) processes BERT’s CLS through an FFN, fine-tuned with five percentile groups.
TRAIL ( Shahout et al., 2024b ) uses lightweight classifiers on token embeddings for real-time performance, similar to Lin et al. (2024d).

Relative Ranking Prediction.

This paradigm predicts relative relationships between requests. Qiu et al. (2024b) compare regression, classification, and pairwise methods, finding each suited to specific data-model pairs.
Fu et al. (2024c) predict relative relationships within the same batch using only input requests, enhancing robustness, and reducing overfitting.

Overall, Relative Ranking Prediction is more intuitive as it only requires determining the order of requests within a batch. However, if some requests carry over to the next batch, their rankings must be recalculated, introducing additional overhead. In contrast, the other two methods do not encounter this issue.

Other distinct approaches also exist.
SkipPredict Shahout and Mitzenmacher (2024) uses a “cheap prediction” to classify tasks as short or long, prioritizing the short ones, while long tasks undergo more accurate “expensive predictions” later.
BatchLLM Zheng et al. (2024b) predicts decoding length based on pre-analysis of the input prompt and statistical patterns.
Instead of predicting the length, Imai et al. (2024) predict inference latency using the Roofline-Driven method.

3.4 KV Cache Optimization

While KV cache reduces inference time complexity from quadratic to linear, it introduces critical challenges in memory management, computational reuse, and compression efficiency.
Optimizations for specialized field storage are discussed in § 5.

Memory Management.

Lossless Storage Techniques
Kwon et al. (2023) introduce PagedAttention and vLLM to address memory fragmentation via OS-inspired paging, achieving near-zero space waste.
Lin et al. (2024a) propose DistAttention for distributed KV cache processing, which enables the handling of longer contexts.
FastDecode ( He and Zhai, 2024 ) offloads cache to CPU memory through distributed processing, while LayerKV Xiong et al. (2024) uses hierarchical allocation and offloading with layer-wise.
KunServe Cheng et al. (2024c) frees space for cache by removing model parameters, compensating via a pipeline mechanism from other instances, and SYMPHONY ( Agarwal et al., 2024 ) dynamically migrates caches using multi-turn interaction patterns.
InstCache ( Zou et al., 2024 ) enhances responsiveness through LLM-driven instruction prediction.

Approximation Methods
PQCache Zhang et al. (2024a) leverages the low-overhead Product Quantization, widely employed in embedding retrieval, by partitioning embeddings into sub-embeddings and applying clustering to reduce computational overhead.
InfiniGen Lee et al. (2024) is a dynamic cache management framework, reducing data transfer overhead and enhancing performance via intelligent prefetching of key KV cache entries.

Reuse Strategies.

Lossless Reuse
PagedAttention ( Kwon et al., 2023 ) enables multi-request cache sharing through page-level management. Radix tree-based systems ( Hu et al., 2024a; Srivatsa et al., 2024 ) implement global prefix sharing with dynamic node deletion. CachedAttention ( Gao et al., 2024 ) minimizes redundant computation in dialogues through cross-turn cache reuse.

Semantic-aware Reuse
GPTCache ( Bang, 2023 ) uses semantic similarity to cache and reuse LLM outputs,
while SCALM ( Li et al., 2024c ) clusters queries to uncover meaningful semantic patterns.

Dimension

Lossless

Semantic-Aware

Matching

Exact

Semantic similarity

Requests

Fixed patterns, repeats

Diverse, open-ended

Consistency

✓✓✓

✓✓

Overhead

✓

✓✓

Table 2: Comparison of KV cache reuse strategies.

Comparing lossless with semantic-aware matching (Table 2 ), the former is ideal for exact-template inputs (e.g., legal, medical, or code generation), while the latter is better suited for open-domain conversations.

Compression Techniques.

To minimize inference performance impact, weight and cache compression techniques specific to tensor quantization and compact representations are used, balancing performance and efficiency ( Wang et al., 2024b ).

Quantization-based Compression
This method reduces memory by shifting from high-bit to low-bit precision.
FlexGen Sheng et al. (2023) uses Group-wise Quantization to compress KV cache to 4-bit without extra I/O costs.
Kivi Zirui Liu et al. (2024) suggests per-channel/token quantization for cache, grouping elements along these dimensions.
MiniCache Liu et al. (2024a) compresses the cache across layers by exploiting the high similarity of KV cache states between adjacent layers.
AWQ Lin et al. (2024c) highlights that quantizing non-salient weights reduces quantization loss.
Atom Zhao et al. (2024b) employs mixed-precision, fine-grained group/dynamic activation/cache quantization.
QServe Lin et al. (2024e) quantizes LLMs to W4A8KV4 precision through algorithm-system co-design, improving GPU deployment efficiency.

Compact Encoding Architectures
It is also desirable to use smaller matrix representations instead of the previous heavy matrix.
CacheGen Liu et al. (2024c) employs a custom tensor encoder to compress KV cache into compact bitstreams, saving bandwidth with minimal decoding overhead.

3.5 PD Disaggregation

PD disaggregation tackles LLM inference’s computational disparity by separating the prefill (context encoding), which is computation-bound, from the decoding (token generation), which is memory-bound, into distinct environments, allowing for specialized optimization.

DistServe Zhong et al. (2024) optimizes resource allocation and parallelism for each phase, minimizing communication overhead by strategic placement based on bandwidth.
Splitwise Patel et al. (2024b) explores homogeneous and heterogeneous device designs to optimize cost, throughput, and power.
DéjàVu Strati et al. (2024) resolves pipeline bubbles caused by bimodal latency, GPU overprovisioning, and slow recovery through microbatch swapping and state replication.
Mooncake Qin et al. (2024) employs a KVCache-centric disaggregated architecture, leveraging idle CPU, DRAM, and SSD resources for distributed KVCache storage, with early rejection under high loads to reduce waste.
TetriInfer Hu et al. (2024b) uses a two-level scheduling algorithm with resource prediction to avoid decoding hotspots.
P/D-Serve Jin et al. (2024b) tackles LLM deployment challenges via fine-grained prefill/decode organization, dynamic adjustments, on-demand request allocation, and efficient cache transmission.

{forest}

Figure 4: Taxonomy of Cluster-Level strategies for LLM inference serving.

4 LLM Inference Serving in Cluster

This section focuses on cluster-level deployment and scheduling, as well as cloud-based cluster serving, as detailed in Figure 4.

4.1 Cluster Optimization

Internal optimizations for homogeneous devices require more machines as parameter scale increases, while heterogeneous machines are preferred for their flexibility, efficiency, and cost-effectiveness Mei et al. (2024). External optimizations, like service-oriented cluster scheduling, further enhance internal optimizations.

Architecture and Optimization for Heterogeneous Resources.

Jayaram Subramanya et al. (2023) propose a joint optimization framework for adaptive task allocation across GPU types and batch sizes, demonstrating significant throughput improvements over static configurations.
Helix Mei et al. (2024) models the execution of LLM services on heterogeneous GPUs and networks as a maximum flow problem in a directed weighted graph, where nodes represent GPU instances and edges encode GPU and network heterogeneity through capacity constraints.
LLM-PQ Zhao et al. (2024a) advocates an adaptive quantization and phase-aware partition scheme tailored for heterogeneous GPU clusters.
HexGen Jiang et al. (2024c) supports asymmetric parallel execution on GPUs with different computing capabilities.
Splitwise Patel et al. (2024b), DistServe Zhong et al. (2024) and HEXGEN-2 Jiang et al. (2024d) optimize computation on heterogeneous disaggregated architectures, with the latter focusing on LLM serving via constraint-based scheduling and graph-based resource optimization.
Hisaharo et al. (2024) integrate advanced interconnect technology, high-bandwidth memory, and energy-efficient power management.

Service-Aware Scheduling.

DynamoLLM Stojkovic et al. (2024) optimizes service clusters by adjusting instances, parallelization, and GPU frequencies based on input/output lengths.
Splitwise Patel et al. (2024b) proposes cluster-level scheduling across prefill and decoding on separate devices.

4.2 Load Balancing

Cluster-level load balancing optimizes request distribution to prevent node overload or underutilization, improving throughput and service quality.
While most frameworks Yu et al. (2022); Kwon et al. (2023) rely on traditional methods like Round Robin and Random Deepspeed (2023), recent advances in heuristic, dynamic, and predictive scheduling provide more sophisticated solutions.

Heuristic Algorithm.

SCLS Cheng et al. (2024b) employs a max-min algorithm Radunovic and Le Boudec (2007) to balance the workloads.
It assigns the batch with the longest estimated serving time to the instance with the lowest score, where the score represents the total serve time of all batches in the instance’s queue.
SAL Kossmann et al. (2024) quantifies the load on two key factors: (1) the number of queued prefill tokens and (2) the available memory. This ensures that requests are dispatched to the server with the lowest load, addressing scenarios where delays occur due to either a full token batch or insufficient memory.

Dynamic Scheduling.

Llumnix Sun et al. (2024) dynamically reschedules requests across model instances during runtime to handle request heterogeneity and unpredictability.
It uses real-time migration to transfer requests and memory states, enabling mid-operation migration to the least loaded instance based on real-time load growth.

Intelligent Predictive Scheduling.

Jain et al. (2024) propose a reinforcement learning-based router that models request routing as a Markov Decision Process, aiming to derive an optimal policy for maximizing discounted rewards. It integrates response length prediction, workload impact estimation, and reinforcement learning.

4.3 Cloud-Based LLM Serving

If local LLM deployment lacks resources, cloud services offer a more economical alternative, with recent research focusing on optimizing cloud deployment and edge collaboration for efficiency.

Deployment and Computing Effective.

To reduce LLM deployment costs, spot instances are used despite preemption risks.
SpotServe Miao et al. (2024b) mitigates this with dynamic reparallelization, parameter reuse, and stateful inference recovery.
ServerlessLLM Fu et al. (2024a) tackles serverless cold start latency via optimized checkpoints, live migration, and locality-aware scheduling.
Mélange Griggs et al. (2024) optimizes GPU allocation based on request patterns, lowering costs.
POLCA Patel et al. (2024a) boosts efficiency through power management, while Imai et al. (2024) predict inference latency to enhance cluster management.
Borzunov et al. (2023) propose a way to integrate idle resources through geodistributed devices connected via the internet.

Cooperation with Edge Device.

To meet SLOs amid cloud latency and bandwidth limits, edge computing offers solutions.
EdgeShard Zhang et al. (2024b) leverages collaboration between distributed edge devices and cloud servers.
PreLLM Yang et al. (2024b) uses a multi-armed bandit framework for personalized scheduling.
Hao et al. (2024) integrate small edge models with cloud LLM to address memory constraints, while He et al. (2024) apply deep reinforcement learning for efficient, latency-aware inference offloading.

{forest}

Figure 5: Taxonomy of Emerging Scenarios for LLM inference serving.

5 Emerging Scenarios

This section introduces efficient LLM serving for various tasks, model architectures, and emerging research areas, as shown in Figure 5.

5.1 Long Context

As LLMs evolve, context lengths have expanded significantly, reaching hundreds of thousands or even millions of tokens moonshot (2023). This growth presents both opportunities and challenges for distributed deployment, computation, and storage, especially in parallel processing, attention computation, and KV cache management.

Parallel Processing.

Loongserve Wu et al. (2024a) enhances this with elastic sequence parallelism for efficient long-context LLM serving.

Attention Computation.

The attention mechanism encounters significant challenges in parallel processing and resource management.
RingAttention Liu et al. (2023) uses blockwise self-attention and FFN computation to distribute long sequences across devices, overlapping KV communication with attention.
StripedAttention Brandon et al. (2023), an extension of RingAttention, addresses imbalances from causal attention’s triangular structure.
DistAttention Lin et al. (2024a) subdivides attention across GPUs, avoiding cache transfer during decoding and enabling partitioning for arbitrary sequence lengths with minimal data transfer.
InstInfer Pan et al. (2024b) offloads attention and data to Computational Storage Drives, reducing KV transfer overheads significantly.

KV Cache Management.

Efficient storage for growing KV cache is crucial for generating new tokens.
Infinite-LLM Lin et al. (2024a) manages dynamic LLM contexts by scheduling cache at the cluster level, balancing resources, and maximizing throughput.
InfiniGen Lee et al. (2024) optimizes cache management in CPU memory for offloading-based systems.
Marconi Pan et al. (2024a) introduces tailored admission and eviction policies for hybrid models, using experimental and theoretical analysis to show that personalized cache sizing per layer reduces memory usage significantly.

5.2 RAG

RAG enables LLMs to retrieve external knowledge for responses, but the diversity and complexity of processing pose challenges in optimizing latency and KV cache storage for large retrieval contexts.

Workflow Scheduling.

Several recent innovations have focused on improving the efficiency, flexibility, and optimization of RAG workflows.
PipeRAG Jiang et al. (2024b) improves efficiency via pipeline parallelism, flexible retrieval intervals, and performance-driven quality adjustment.
Teola Tan et al. (2024) models LLM workflows as data flow nodes (e.g., Embedding, Indexing, Searching) for precise execution control.
RaLMSpec Zhang et al. (2024d) employs speculative retrieval with batched verification to reduce serving overhead.
RAGServe Ray et al. (2024) schedules queries and adjusts RAG configurations (e.g., text chunks, synthesis methods) to balance quality and latency.

Storage Optimization.

Efficient storage management is critical for RAG systems, particularly in handling large-scale KV caches. Recent studies include RAGCache Jin et al. (2024a), which employs knowledge trees and dynamic speculative pipelining to reduce redundancy.
SparseRAG Zhu et al. (2024) manages cache efficiently with prefilling and selective decoding, focusing on relevant tokens.
CacheBlend Yao et al. (2024) reuses cache and selects tokens based on a fixed percentage to recompute KV values for partial updates, enhancing efficiency and reducing latency.
In contrast to CacheBlend, EPIC Hu et al. (2024c) introduces position-independent context caching via static sparse computation, recomputing only a small number of tokens at the beginning of each block.

5.3 MoE

MoE models, known for parameter sparsity, excel in LLMs (e.g., DeepSeek-V3 DeepSeek-AI et al. (2024), Mixtral 8x7B Jiang et al. (2024a) ). Key inference latency challenges include expert parallelism, load balancing, and All-to-All communication (Figure 6 ), with Liu et al. (2024b) offering a comprehensive optimization survey.

Figure 6: This figure illustrates a MoE architecture, highlighting expert placement, All-to-All communication (left), and load balancing (right). On the right, high-traffic Expert 𝐦 \mathbf{m} and low-traffic Expert 𝐧 \mathbf{n} are shown. For example, two strategies are presented: replicating m to a new GPU or offloading 𝐧 \mathbf{n} to free space for 𝐦 \mathbf{m}.

Expert Placement.

Tutel Hwang et al. (2023) introduces switchable parallelism and dynamic pipelining without extra overhead, while DeepSpeed-MoE Rajbhandari et al. (2022) combines expert parallelism He et al. (2021); Lepikhin et al. (2020) with expert-slicing.

Expert Load Balancing.

Imbalanced token distribution causes device underutilization.
Expert Buffering Huang et al. (2023) allocates active experts to GPUs and others to CPUs, pairing high- and low-load experts using historical data.
Brainstorm Cui et al. (2023) dynamically assigns GPU units based on load, while Lynx Gupta et al. (2024) adaptively reduces active experts.
ExpertChoice Zhou et al. (2022) selects top-k tokens per expert, rather than the reverse.
High-load experts in DeepSeek-V3 DeepSeek-AI et al. (2024) are identified using deployment statistics and periodically duplicated to optimize performance.

All-to-All Communication.

Expert processing involves all-to-all exchanges for token dispatch and output gathering.
Tutel Hwang et al. (2023) uses a 2D hierarchical All-to-All algorithm, Aurora Li et al. (2024b) optimizes token transmission order during All-to-All exchanges, and
Lina Li et al. (2023) prioritizes All-to-All operations over concurrent All-Reduce whenever feasible, leveraging tensor partitioning to improve performance.

5.4 LoRA

LoRA Hu et al. (2021); Chen et al. (2024); Dettmers et al. (2023) adapts LLMs to various tasks with small, trainable adapters.
CaraServe Li et al. (2024e) enables GPU-efficient, cold-start-free, SLO-aware serving via model multiplexing, CPU-GPU coordination, and rank-aware scheduling.
dLoRA Wu et al. (2024c) dynamically merges and unmerges adapters with the base model, and migrates requests and adapters across worker replicas.

5.5 Speculative Decoding

Speculative decoding Xia et al. (2024); Wang et al. (2024a) speeds up inference by generating draft tokens with smaller LLMs and verifying them in parallel with target LLM, reducing latency and costs without quality loss.
SpecInfer Miao et al. (2024a) uses tree-based speculative inference for faster distributed and single-GPU offloading inference.

5.6 Augmented LLMs

LLMs increasingly integrate with external tools like APIs and Agents.
APISERVE Abhyankar et al. (2024) dynamically manages GPU resources for external APIs, while LAMPS Shahout et al. (2024a) leverages predicting memory usage.
Parrot Lin et al. (2024b) optimizes scheduling by identifying request dependencies and commonalities, particularly in Agent scenarios, using Semantic Variables to tag each request. While Parrot is a pioneering approach to addressing agent-related challenges, it has significant limitations and has yet to achieve widespread adoption, necessitating further exploration by future researchers.

5.7 Test-Time Reasoning

Inference-time algorithms OpenAI (2024) enhance the reasoning ability Ji et al. (2025) of LLMs but achieve this by generating a large number of tokens, which can strain computational resources.
Dynasor Fu et al. (2024b) introduces the Certaindex metric to dynamically track a model’s reasoning progress, adjust computational resources based on task difficulty, and proactively terminate unpromising requests.
Damani et al. (2024) optimizes resource allocation (e.g., different LLMs, computing budgets) by using a built-in reward model to assess the marginal benefit of additional computation for each request.

{forest}

Figure 7: Taxonomy of Miscellaneous Areas for LLM inference serving.

6 Miscellaneous Areas

Other niche but important directions (Figure 7 ) are also advancing LLM inference toward a more comprehensive and far-reaching future.

6.1 Hardware

Recent advancements in optimizing LLM inference have focused on improving efficiency, speed, and resource utilization in various hardware techniques.

Peng et al. (2024) propose a mixed-precision, multi-level caching system (HBM, DRAM, SSDs) and a model modularization algorithm to enable LLM inference on resource-constrained, outdated hardware.
Wu et al. (2024d) explore inference service solutions on Intel GPUs.
LLM-Pilot Łazuka et al. (2024) benchmarks LLM inference across GPUs and recommends the most cost-effective GPU for unseen LLMs.
GenZ Bambhaniya et al. (2024) is an analytical tool for studying the relationship between LLM inference performance and various hardware platform design parameters.
Li et al. (2024d) present Transformer-Lite, an innovative inference engine optimized for mobile GPUs, designed to enhance the efficiency and inference speed of LLM deployment on mobile devices.
LLMS Yin et al. (2024) is an innovative system on mobile devices that, under stringent memory constraints, implements fine-grained, chunk-based KV cache compression and a globally optimized swapping mechanism to decouple applications from LLM memory management, thereby minimizing the overhead of context switching.
Xu et al. (2024) utilize on-device Neural Processing Unit (NPU) offloading to enhance NPU offloading efficiency and reduce prefill latency.

6.2 Privacy

Protecting user conversation content in LLMs from potential leakage is an important issue.
Yang et al. (2024a) adopt weight permutation to shuffle KV pairs, preventing attackers from reconstructing the entire context.
Zhang et al. (2024c) quantify the trade-off between privacy protection and utility loss, pointing out that privacy protection mechanisms (such as randomization) can reduce privacy leakage but will introduce utility loss.
MARILL Rathee et al. (2024) achieves substantial reductions in the costly operations required for secure inference within multi-party computation by optimizing the architecture of LLMs during the fine-tuning phase.

6.3 Simulator

Considering the diversity of computing devices and their associated high costs, a comprehensive simulator is indispensable for conducting trials in virtual environments.
Agrawal et al. (2024b) introduce Vidur, a scalable, high-fidelity simulation framework for evaluating LLM performance under various deployment configurations, alongside Vidur-Search, a tool for optimizing deployments to meet performance constraints and reduce costs.
The Helix system Mei et al. (2024), featuring an event-based simulator, enables accurate simulation of LLM inference in heterogeneous GPU clusters by adjusting factors like network conditions, machine heterogeneity, and cluster scale, providing rapid and cost-effective deployment evaluations.

6.4 Fairness

In LLM inference services, request frequency limits are typically imposed on each client (e.g., user or application) to ensure fair resource allocation.
These limits prevent excessive requests from monopolizing resources and degrading service quality for others.
However, they may also result in underutilized resources.
Sheng et al. (2024) propose a novel fairness definition, based on a cost function considering input and output tokens.
Additionally, a new scheduling algorithm, the Virtual Token Counter (VTC), introduces fair scheduling through a continuous batching mechanism.

6.5 Energy

Given the substantial power demands of LLM computations, optimizing energy usage is a critical challenge that must be addressed.
Nguyen et al. (2024) investigate the carbon emissions of LLMs from operational and embodied perspectives, aiming to promote sustainable LLM services.
Researchers analyzed the performance and energy consumption of the LLaMA model across varying parameter scales and batch sizes, incorporating the carbon intensity of different power grid regions.
This study provides insights into the environmental impact of LLMs and explores opportunities to optimize sustainable LLM systems.

7 Future Works

Given the rapid evolution of LLM inference services, we present several recommendations for future research.

•

Scheduling with Dependency Constraints : User requests are considered complete only when all dependency-ordered sub-requests are finished. This approach is especially relevant for multi-LLM collaboration and agent-based systems.

•

Large Multimodal Model Service : These models are currently deployed as monolithic systems. However, challenges such as the imbalance between text and image inputs, as well as the discrepancy in their encoding times, present significant opportunities for optimization.

•

Intelligent LLM Inference Service : Utilizing the capabilities of smaller LLMs to optimize the deployment, scheduling, and storage management of larger LLMs.

•

Safety and Privacy : As most services rely on cloud computing, it is essential to prevent cache leaks and ensure that any leaked data cannot be used to reconstruct user conversations.

We hope that these suggestions will provide valuable insights for advancing future research.

8 Conclusion

The primary challenge in LLM inference serving stems from the significant memory requirements caused by the scale of parameters and the computational load associated with attention mechanisms.
This paper presents a thorough and hierarchical review of methods, encompassing approaches from basic instance-level to more advanced cluster-level techniques, as well as a variety of emerging scenarios.
Additionally, we explore small yet significant areas and suggest potential directions for future research.
We hope this work provides valuable insights for ongoing research in this crucial field.

References

Abdin et al. (2024)

Marah Abdin, Jyoti Aneja, Harkirat Behl, Sébastien Bubeck, Ronen Eldan, Suriya Gunasekar, Michael Harrison, Russell J. Hewett, Mojan Javaheripi, Piero Kauffmann, James R. Lee, Yin Tat Lee, Yuanzhi Li, Weishung Liu, Caio C. T. Mendes, Anh Nguyen, Eric Price, Gustavo de Rosa, Olli Saarikivi, Adil Salim, Shital Shah, Xin Wang, Rachel Ward, Yue Wu, Dingli Yu, Cyril Zhang, and Yi Zhang. 2024.

Phi-4 technical report.

Preprint, arXiv:2412.08905.

Abhyankar et al. (2024)

Reyna Abhyankar, Zijian He, Vikranth Srivatsa, Hao Zhang, and Yiying Zhang. 2024.

Infercept: Efficient intercept support for augmented large language model inference.

Preprint, arXiv:2402.01869.

Agarwal et al. (2023)

Megha Agarwal, Asfandyar Qureshi, Nikhil Sardana, Linden Li, Julian Quevedo, and Daya Khudia. 2023.

Llm inference performance engineering: Best practices.

Agarwal et al. (2024)

Saurabh Agarwal, Anyong Mao, Aditya Akella, and Shivaram Venkataraman. 2024.

Symphony: Improving memory management for llm inference workloads.

Preprint, arXiv:2412.16434.

Agrawal et al. (2024a)

Amey Agrawal, Anmol Agarwal, Nitin Kedia, Jayashree Mohan, Souvik Kundu, Nipun Kwatra, Ramachandran Ramjee, and Alexey Tumanov. 2024a.

Etalon: Holistic performance evaluation framework for llm inference systems.

Preprint, arXiv:2407.07000.

Agrawal et al. (2024b)

Amey Agrawal, Nitin Kedia, Jayashree Mohan, Ashish Panwar, Nipun Kwatra, Bhargav Gulavani, Ramachandran Ramjee, and Alexey Tumanov. 2024b.

Vidur: A large-scale simulation framework for llm inference.

Preprint, arXiv:2405.05465.

Agrawal et al. (2024c)

Amey Agrawal, Nitin Kedia, Ashish Panwar, Jayashree Mohan, Nipun Kwatra, Bhargav Gulavani, Alexey Tumanov, and Ramachandran Ramjee. 2024c.

Taming Throughput-Latency tradeoff in LLM inference with Sarathi-Serve.

In 18th USENIX Symposium on Operating Systems Design and Implementation (OSDI 24), pages 117–134, Santa Clara, CA. USENIX Association.

Aminabadi et al. (2022)

Reza Yazdani Aminabadi, Samyam Rajbhandari, Minjia Zhang, Ammar Ahmad Awan, Cheng Li, Du Li, Elton Zheng, Jeff Rasley, Shaden Smith, Olatunji Ruwase, and Yuxiong He. 2022.

Deepspeed inference: Enabling efficient inference of transformer models at unprecedented scale.

Preprint, arXiv:2207.00032.

Bambhaniya et al. (2024)

Abhimanyu Bambhaniya, Ritik Raj, Geonhwa Jeong, Souvik Kundu, Sudarshan Srinivasan, Midhilesh Elavazhagan, Madhu Kumar, and Tushar Krishna. 2024.

Demystifying platform requirements for diverse llm inference use cases.

Preprint, arXiv:2406.01698.

Bang (2023)

Fu Bang. 2023.

GPTCache: An open-source semantic cache for LLM applications enabling faster answers and cost savings.

In Proceedings of the 3rd Workshop for Natural Language Processing Open Source Software (NLP-OSS 2023), pages 212–218, Singapore. Association for Computational Linguistics.

Borzunov et al. (2023)

Alexander Borzunov, Max Ryabinin, Artem Chumachenko, Dmitry Baranchuk, Tim Dettmers, Younes Belkada, Pavel Samygin, and Colin Raffel. 2023.

Distributed inference and fine-tuning of large language models over the internet.

Preprint, arXiv:2312.08361.

Brandon et al. (2023)

William Brandon, Aniruddha Nrusimha, Kevin Qian, Zachary Ankner, Tian Jin, Zhiye Song, and Jonathan Ragan-Kelley. 2023.

Striped attention: Faster ring attention for causal transformers.

Preprint, arXiv:2311.09431.

Chen et al. (2024)

Yukang Chen, Shengju Qian, Haotian Tang, Xin Lai, Zhijian Liu, Song Han, and Jiaya Jia. 2024.

Longlora: Efficient fine-tuning of long-context large language models.

Preprint, arXiv:2309.12307.

Cheng et al. (2024a)

Ke Cheng, Wen Hu, Zhi Wang, Peng Du, Jianguo Li, and Sheng Zhang. 2024a.

Enabling efficient batch serving for lmaas via generation length prediction.

arXiv preprint arXiv:2406.04785.

Cheng et al. (2024b)

Ke Cheng, Wen Hu, Zhi Wang, Hongen Peng, Jianguo Li, and Sheng Zhang. 2024b.

Slice-level scheduling for high throughput and load balanced llm serving.

Preprint, arXiv:2406.13511.

Cheng et al. (2024c)

Rongxin Cheng, Yifan Peng, Yuxin Lai, Xingda Wei, Rong Chen, and Haibo Chen. 2024c.

Kunserve: Elastic and efficient large language model serving with parameter-centric memory management.

Preprint, arXiv:2412.18169.

Cui et al. (2023)

Weihao Cui, Zhenhua Han, Lingji Ouyang, Yichuan Wang, Ningxin Zheng, Lingxiao Ma, Yuqing Yang, Fan Yang, Jilong Xue, Lili Qiu, Lidong Zhou, Quan Chen, Haisheng Tan, and Minyi Guo. 2023.

Optimizing dynamic neural networks with brainstorm.

In 17th USENIX Symposium on Operating Systems Design and Implementation (OSDI 23), pages 797–815, Boston, MA. USENIX Association.

Damani et al. (2024)

Mehul Damani, Idan Shenfeld, Andi Peng, Andreea Bobu, and Jacob Andreas. 2024.

Learning how hard to think: Input-adaptive allocation of lm computation.

Preprint, arXiv:2410.04707.

DeepSeek-AI et al. (2025)

DeepSeek-AI, Daya Guo, Dejian Yang, Haowei Zhang, Junxiao Song, Ruoyu Zhang, Runxin Xu, Qihao Zhu, Shirong Ma, Peiyi Wang, Xiao Bi, Xiaokang Zhang, Xingkai Yu, Yu Wu, Z. F. Wu, Zhibin Gou, Zhihong Shao, Zhuoshu Li, Ziyi Gao, Aixin Liu, Bing Xue, Bingxuan Wang, Bochao Wu, Bei Feng, Chengda Lu, Chenggang Zhao, Chengqi Deng, Chenyu Zhang, Chong Ruan, Damai Dai, Deli Chen, Dongjie Ji, Erhang Li, Fangyun Lin, Fucong Dai, Fuli Luo, Guangbo Hao, Guanting Chen, Guowei Li, H. Zhang, Han Bao, Hanwei Xu, Haocheng Wang, Honghui Ding, Huajian Xin, Huazuo Gao, Hui Qu, Hui Li, Jianzhong Guo, Jiashi Li, Jiawei Wang, Jingchang Chen, Jingyang Yuan, Junjie Qiu, Junlong Li, J. L. Cai, Jiaqi Ni, Jian Liang, Ji
