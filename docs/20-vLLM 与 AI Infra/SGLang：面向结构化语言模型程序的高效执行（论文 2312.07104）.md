# SGLang：面向结构化语言模型程序的高效执行（论文 2312.07104）

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　50,164 字符　｜　313 个片段　｜　更新于 2026-11-13
来源: https://ar5iv.labs.arxiv.org/html/2312.07104

# https://ar5iv.labs.arxiv.org/html/2312.07104

来源: https://ar5iv.labs.arxiv.org/html/2312.07104
抓取方式: read_page

SGLang: Efficient Execution of
Structured Language Model Programs

Lianmin Zheng   Liangsheng Yin   Zhiqiang Xie   Chuyue Sun   Jeff Huang

Thanks: Equal contribution.

Affiliation: Stanford University

Affiliation: Stanford University

Affiliation: UC Berkeley

Affiliation: Shanghai Jiao Tong University

Affiliation: Texas A&M University

  
Cody Hao Yu   Shiyi Cao   Christos Kozyrakis   Ion Stoica   Joseph E. Gonzalez

Affiliation: Stanford University

Affiliation: UC Berkeley

Affiliation: UC Berkeley

Affiliation: UC Berkeley

Affiliation: Independent Researcher

  
Clark Barrett   Ying Sheng

Affiliation: Stanford University

Abstract

Large language models (LLMs) are increasingly used for complex tasks that require multiple generation calls, advanced prompting techniques, control flow, and structured inputs/outputs. However, efficient systems are lacking for programming and executing these applications. We introduce SGLang, a system for efficient execution of complex language model programs. SGLang consists of a frontend language and a runtime. The frontend simplifies programming with primitives for generation and parallelism control. The runtime accelerates execution with novel optimizations like RadixAttention for KV cache reuse and compressed finite state machines for faster structured output decoding. Experiments show that SGLang achieves up to 6.4 × 6.4\times higher throughput compared to state-of-the-art inference systems on various large language and multi-modal models on tasks including agent control, logical reasoning, few-shot learning benchmarks, JSON decoding, retrieval-augmented generation pipelines, and multi-turn chat. The code is publicly available at https://github.com/sgl-project/sglang.

1 Introduction

Recent increases in the capabilities of LLMs have broadened their utility, enabling them to tackle a wider range of general tasks and act as autonomous agents openai2023gpt4; bubeck2023sparks; Park2023GenerativeAgents; wang2023voyager; sumers2023cognitive.
In such applications, LLMs engage in multi-round planning, reasoning, and interaction with external environments.
This is accomplished through tool usage schick2023toolformer; patil2023gorilla, multiple input modalities team2023gemini; alayrac2022flamingo, and a wide range of prompting techniques liu2023prompting, like few-shot learning brown2020language,
self-consistency wang2022self, skeleton-of-thought ning2024skeletonofthought, and tree-of-thought yao2023tree.
All of these new use cases require multiple, often dependent, LLM generation calls, showing a trend of using multi-call structures to complete complex tasks yao2022react; kim2023llm.

The emergence of these patterns signifies a shift in our interaction with LLMs, moving from simple chatting to a more sophisticated form of programmatic usage of LLMs, which means using a program to schedule and control the generation processes of LLMs. We refer to these programs as "Language Model Programs" (LM Programs) beurer2023lmql; khattab2023dspy.
The advanced prompting techniques and agentic workflow mentioned above fall within the scope of LM programs.
There are two common properties of LM programs:
(1) LM programs typically contain multiple LLM calls interspersed with control flow. This is needed to complete complex tasks and improve overall quality.
(2) LM programs receive structured inputs and produce structured outputs. This is needed to enable the composition of LM programs and to integrate LM programs into existing software systems.

Despite the widespread use of LM programs, current systems for expressing and executing them remain inefficient. We identify two primary challenges associated with the efficient use of LM programs:
First, programming LM programs is tedious and difficult due to the non-deterministic nature of LLMs.
Developing an LM program often requires extensive string manipulation, experimental tuning of prompts, brittle output parsing,
handling multiple input modalities,
and implementing parallelism mechanisms.
This complexity significantly reduces the readability of even simple programs ( Sec. 2 ).

Secondly and importantly, executing LM programs is inefficient due to redundant computation and memory usage.
State-of-the-art inference engines (e.g., vLLM kwon2023vllm, TGI tgi, and TensorRT-LLM nvidia_tensorrt_llm ), have been optimized to reduce latency and improve throughput without direct knowledge of the workload.
This makes these systems general and robust but also results in significant inefficiencies for any given workload.
A prominent example is the reuse of the Key-Value (KV) cache ( Sec. 3 ). The KV cache consists of reusable intermediate tensors that are essential for generative inference.
During typical batch executions of LM programs, numerous opportunities exist to reuse the KV cache across multiple different LLM calls that share a common prefix.
However, current systems lack effective mechanisms to facilitate this reuse, resulting in unnecessary computations and wasted memory.
Another example is constrained decoding for structured outputs (e.g., JSON mode), where the output of LLMs is restricted to follow specific grammatical rules defined by a regular expression ( Sec. 4 ).
Under these constraints, multiple tokens can often be decoded once. However, existing systems only decode one token at a time, leading to suboptimal decoding speeds.

Figure 1 : System architecture: An interpreter executes language primitives with optimized runtime.

To address these challenges, we present SGLang, a S tructured G eneration Lang uage for LLMs.
The core idea is to systematically exploit the multi-call structure in LM programs for efficient execution.
As shown in Fig. 1, it has two parts: a front-end language and a back-end runtime.
The front-end simplifies the programming of LM programs, and the runtime accelerates their execution.
The two parts can work together for better performance but can also function independently.

We introduce SGLang as a domain-specific language embedded in Python. It provides primitives for generation (e.g., extend, gen, select ) and parallelism control (e.g., fork, join ). SGLang is compatible with Python’s control flow and libraries, so users can develop advanced prompting workflows easily with native Python syntax. We provide an interpreter and a compiler for SGLang. The interpreter manages the prompt state as a stream and submits primitive operations to the stream for asynchronous execution, ensuring proper control over synchronization and intra-program parallelism.
Additionally, SGLang program can be traced and compiled for more optimizations.

On the runtime side, we propose several novel optimizations to accelerate the execution of SGLang programs. The first technique, RadixAttention, enables the automatic reuse of the KV cache across multiple generation calls. In existing inference engines, the KV cache of a request is discarded after processing is completed, preventing the KV cache from being reused across multiple calls and significantly slowing down the execution. Instead, our system maintains an LRU cache of the KV cache for all requests within a radix tree. This approach manages the KV cache as a traditional cache and uses a radix tree for efficient matching, insertion, and eviction. It allows the runtime to handle various reuse patterns with a cache-aware scheduling policy efficiently.
The second technique is a compressed finite state machine, which enables faster constrained decoding for structured outputs. Existing systems follow the constraints only for the next token by masking probabilities of disallowed tokens, making them able to decode only one token at a time. Instead, our system analyzes the constraints and builds a compressed finite-state machine to represent the constraint. This approach compresses a multi-token path into a single-step path whenever possible, allowing the decoding of multiple tokens at once to achieve faster decoding speed.
Lastly, SGLang also supports API-only models like OpenAI’s GPT-4, and we introduce the third technique, API speculative execution, to optimize multi-call programs for API-only models.

Using SGLang, we implemented various LLM applications, including agent control, logical reasoning, few-shot learning benchmarks, JSON decoding, retrieval-augmented generation pipelines, multi-turn chat, and multi-modality processing. We tested the performance on models including Llama-7B/70B touvron2023llama2, Mistral-8x7B jiang2024mixtral, LLaVA-v1.5-7B (image) liu2024llavanext, and LLaVA-NeXT-34B (video) zhang2024llavanextvideo on NVIDIA A10G and A100 GPUs. Experimental results show that SGLang achieves up to 6.4 × 6.4\times higher throughput across a wide range of workloads, models, and hardware setups, compared to existing programming and inference systems, including Guidance guidance, vLLM kwon2023vllm, and LMQL beurer2023lmql.

2 Programming Model

This section introduces the SGLang programming model with a running example, describes its language primitives and execution modes, and outlines runtime optimization opportunities.
This programming model can simplify tedious operations in multi-call workflows (e.g., string manipulation, API calling, constraint specification, parallelism) by providing flexible and composable primitives.

Figure 2 : The implementation of a multi-dimensional essay judge in SGLang utilizes the branch-solve-merge prompting technique saha2023branch. Primitives provided by SGLang are shown in red.

A running example.
The language is a domain-specific language embedded in Python.
Fig. 2 shows a program that evaluates an essay about an image using the branch-solve-merge prompting method saha2023branch.
The function multi_dimensional_judge takes three arguments: s, path, and essay.
s manages the prompt state, path is the image file path, and essay is the essay text.
New strings and SGLang primitives can be appended to the state s for execution using the += operator.
First, the function adds the image and essay to the prompt.
It then checks if the essay is related to the image using select, storing the result in s["related"].
If related, the prompt is forked into three copies for parallel evaluation from different dimensions, using gen to store results in f["judgment"].
Next, it merges the judgments, generates a summary, and assigns a letter grade.
Finally, it returns the results in JSON format, following a schema defined by a regular expression constraint regex.
SGLang greatly simplifies this program, as an equivalent program using an OpenAI API-like interface would take 2.1 × 2.1\times as many lines of code due to manual string manipulation and parallelism control.

Language primitives.
SGLang provides primitives for controlling prompt state, generation, and parallelism. They can be used together with Python syntax and libraries.
Here are the primitives:
“ gen ” calls a model to generate and stores the results in a variable with the name specified in its first argument. It supports a “ regex ” argument to constrain the output to follow a grammar defined by a regular expression (e.g., a JSON schema).
“ select ” calls a model to choose the highest probability option from a list.
The operator “ += ” or “ extend ” appends a string to the prompt.
The operator “ [variable_name] ” fetches the results of a generation.
“ fork ” creates parallel forks of the prompt state.
“ join ” rejoins the prompt state.
“ image ” and “ video ” take in image and video inputs.

Execution modes.
The simplest way to execute an SGLang program is through an interpreter, where a prompt is treated as an asynchronous stream. Primitives like extend, gen, and select are submitted to the stream for asynchronous execution. These non-blocking calls allow Python code to continue running without waiting for the generation to finish. This is similar to launching CUDA kernels asynchronously. Each prompt is managed by a stream executor in a background thread, enabling intra-program parallelism. Fetching generation results will block until they are ready, ensuring correct synchronization.
Alternatively, SGLang programs can be compiled as computational graphs and executed with a graph executor, allowing for more optimizations. This paper uses interpreter mode by default and discusses compiler mode results in Appendix D.
SGLang supports open-weight models with its own SGLang Runtime (SRT), as well as API models such as OpenAI and Anthropic models.

Comparison.
Programming systems for LLMs can be classified as high-level (e.g., LangChain, DSPy) and low-level (e.g., LMQL, Guidance, SGLang).
High-level systems provide predefined or auto-generated prompts, such as DSPy’s prompt optimizer.
Low-level systems typically do not alter prompts but allow direct manipulation of prompts and primitives.
SGLang is a low-level system similar to LMQL and Guidance. Table 1 compares their features.
SGLang focuses more on runtime efficiency and comes with its own co-designed runtime, allowing for novel optimizations introduced later.
High-level languages (e.g., DSPy) can be compiled to low-level languages (e.g., SGLang).
We demonstrate the integration of SGLang as a backend in DSPy for better runtime efficiency in Sec. 6.

Runtime optimizations.
Fig. 2 shows three runtime optimization opportunities: KV cache reuse, fast constrained decoding, API speculative execution. We will discuss them in the following sections.

Table 1 : Comparison among LMQL, Guidance, and SGLang.

System

Syntax

Language Primitives

Runtime Backends

LMQL

Custom

extend, gen, select

HF Transformers, llama.cpp, OpenAI

Guidance

Python

extend, gen, select, image

HF Transformers, llama.cpp, OpenAI

SGLang

Python

extend, gen, select, image, video, fork, join

SGLang Runtime (SRT), OpenAI

3 Efficient KV Cache Reuse with RadixAttention

SGLang programs can chain multiple generation calls and create parallel copies with the " fork " primitive.
Additionally, different program instances often share some common parts (e.g., system prompts). These scenarios create many shared prompt prefixes during execution, leading to numerous opportunities for reusing the KV cache.
During LLM inference, the KV cache stores intermediate tensors from the forward pass, reused for decoding future tokens. They are named after key-value pairs in the self-attention mechanism vaswani2017attention. KV cache computation depends only on prefix tokens. Therefore, requests with the same prompt prefix can reuse the KV cache, reducing redundant computation and memory usage. More background and some examples are provided in Appendix A.

Given the KV cache reuse opportunity, a key challenge in optimizing SGLang programs is reusing the KV cache across multiple calls and instances.
While some systems explore certain KV cache reuse cases kwon2023vllm; ye2024chunkattention; juravsky2024hydragen; gim2023prompt, they often need manual configurations and cannot handle all reuse patterns (e.g., dynamic tree structures). Consequently, most state-of-the-art inference systems recompute the KV cache for each request. We will discuss their limitations and our differences in Sec. 7.

This section introduces RadixAttention, a novel technique for automatic and systematic KV cache reuse during runtime. Unlike existing systems that discard the KV cache after a generation request finishes, our system retains the cache for prompts and generation results in a radix tree, enabling efficient prefix search, reuse, insertion, and eviction. We implement an LRU eviction policy and a cache-aware scheduling policy to enhance the cache hit rate. RadixAttention is compatible with techniques like continuous batching yu2022orca, paged attention kwon2023vllm, and tensor parallelism shoeybi2019megatron.
In addition, it introduces only negligible memory and time overhead when there is no cache hit.

RadixAttention.
A radix tree is a data structure that serves as a space-efficient alternative to a classical trie (prefix tree). Unlike typical trees, the edges of a radix tree can be labeled not just with single elements but also with sequences of elements of varying lengths, significantly enhancing efficiency.
In our system, we utilize a radix tree to manage a mapping between sequences of tokens, and their corresponding KV cache tensors. These KV cache tensors are stored in a non-contiguous, paged layout, where the size of each page is equivalent to one token.
Because GPU memory is quickly filled by the KV cahce, we introduce a simple LRU eviction policy that evicts the least recently used leaf first. By evicting leaves first, we enable the re-use of their common ancestors until those ancestors become leaves and are also evicted.

In the continuous batching setting, we cannot evict nodes used by the currently running batch. Therefore, each node maintains a reference counter indicating how many running requests are using it. A node is evictable if its reference counter is zero. Note that we do not preallocate a fixed-size memory pool as a cache. Instead, we let the cached tokens and the currently running requests share the same memory pool. Therefore, the system dynamically allocates memory for cache and running requests. When enough waiting requests run, the system will evict all cached tokens in favor of a larger batch size.
Fig. 3 shows how the radix tree is maintained for several incoming requests.
The frontend interpreter sends full prompts to the runtime, and the runtime performs prefix matching and reuse.
The tree structure is stored on the CPU with negligible maintenance overhead.
During the execution of the fork primitive, the frontend sends the prefix first as a hint, ensuring the prefix is correctly inserted into the tree. It then sends the remaining prompts. This "Frontend Hint" simplifies runtime scheduling and matching, exemplifying the benefits of frontend-runtime co-design.

Figure 3 :
Examples of RadixAttention operations with an LRU eviction policy, illustrated across nine time points.
The figure demonstrates the dynamic evolution of the radix tree in response to various requests. These requests include two chat sessions, a batch of few-shot learning inquiries, and a self-consistency sampling.
Each tree edge carries a label denoting a substring or a sequence of tokens. The nodes are color-coded to reflect different states: green for newly added nodes, blue for cached nodes accessed during the time point, and red for nodes that have been evicted.
In step (1), the radix tree is initially empty.
In step (2), the server processes an incoming user message "Hello" and responds with the LLM output "Hi". The system prompt "You are a helpful assistant", the user message "Hello!", and the LLM reply "Hi!" are consolidated into the tree as a single edge linked to a new node.
In step (3), a new prompt arrives and the server finds the prefix of the prompt (i.e., the first turn of the conversation) in the radix tree and reuses its KV cache. The new turn is appended to the tree as a new node.
In step (4), a new chat session begins. The node “b” from (3) is split into two nodes to allow the two chat sessions to share the system prompt.
In step (5), the second chat session continues. However, due to the memory limit, node "c" from (4) must be evicted. The new turn is appended after node "d" in (4).
In step (6), the server receives a few-shot learning query, processes it, and inserts it into the tree. The root node is split because the new query does not share any prefix with existing nodes.
In step (7), the server receives a batch of additional few-shot learning queries. These queries share the same set of few-shot examples, so we split node ’e’ from (6) to enable sharing.
In step (8), the server receives a new message from the first chat session. It evicts all nodes from the second chat session (node "g" and "h") as they are least recently used.
In step (9), the server receives a request to sample more answers for the questions in node "j" from (8), likely for self-consistency prompting. To make space for these requests, we evict node "i", "k", and "l" in (8).

Cache-aware scheduling.
We define the cache hit rate as number of cached prompt tokens number of prompt tokens \frac{\text{number of cached prompt tokens}}{\text{number of prompt tokens}}.
When there are many requests in the waiting queue, the order in which they are executed can significantly impact the cache hit rate. For example, if the request scheduler frequently switches between different, unrelated requests, it can lead to cache thrashing and a low hit rate.
We design a cache-aware scheduling algorithm to increase the cache hit rate.
In the batch-processing setting we
sort the requests by matched prefix length and prioritize requests with longer matched prefixes instead of using a first-come, first-served schedule. Alg. 1 (Appendix) shows the pseudo-code for cache-aware scheduling with contiguous batching. The algorithm uses longest-shared-prefix-first order.
In more latency-sensitive settings we may still be able to tolerate limited batch re-ordering to improve cache reuse.
Additionally, we prove the following theorem for optimal scheduling in the offline case.
1 1
1

In practice, the computation is not the same as what is described in the proof of Theorem 3.1 because the unpredictable number of output tokens can cause the recomputation of the KV cache.

Theorem 3.1.

For a batch of requests, we can achieve an optimal cache hit rate by visiting the radix tree of the requests in the depth-first search order, with a cache size ≥ \geq the maximum request length. The longest-shared-prefix-first order is equivalent to a depth-first search order.

The proof is in Sec. A.3 (Appendix).
In the online case, the DFS order will be disrupted, but our schedule still approximates the DFS behavior on the augmented part of the full radix tree, as described in Sec. A.3.
While greedy cache-aware scheduling can achieve high throughput, it can lead to starvation. We leave its integration with other fair scheduling methods sheng2023fairness as future work.

Distributed Cases. RadixAttention can be extended to multiple GPUs. For tensor parallelism, each GPU maintains a sharded KV cache. There is no need for additional synchronization because the tree operations are the same.
Data parallelism with multiple workers is discussed in Sec. A.4 (Appendix).

4 Efficient Constrained Decoding with Compressed Finite State Machine

In LM programs, users often want to constrain the model’s output to follow specific formats, such as JSON schemas. This can improve controllability and robustness, and make the output easier to parse. SGLang offers a regex argument to enforce such constraints using regular expressions, which are expressive enough for many practical scenarios.
Existing systems support this by converting a regular expression into a finite state machine (FSM) willard2023efficient. During decoding, they maintain the current FSM state, retrieve allowed tokens from the next states, and set the probability of invalid tokens to zero, decoding token by token. This token-by-token approach, however, is inefficient when there are opportunities to decode multiple tokens at once.
For example, the constant sequence {"summary": " in Fig. 2 spans multiple tokens in the normal decoding process as shown in Fig. 4 (c), requiring multiple decoding stages, even though there is only one valid next token when decoding it. Therefore, the whole sequence can be decoded in a single step (i.e., forward pass).
However, existing systems can only decode one token at a time because the lack of integration between the FSM and the model runner in existing systems prevents multi-token processing, resulting in slow decoding.

Figure 4 : The decoding process of normal and compressed FSMs (the underscore _ means a space).

SGLang overcomes this limitation by creating a fast constrained decoding runtime with a compressed FSM. This runtime analyzes the FSM and compresses adjacent singular-transition edges in the FSM into single edges as demonstrated in Fig. 4 (b), allowing it to recognize when multiple tokens can be decoded together. In Fig. 4 (d), multiple tokens on the compressed transition edge can be decoded in one forward pass, which greatly accelerates the decoding process. It is also general and applicable to all regular expressions. More details on the background and implementation are in Appendix B.

5 Efficient Endpoint Calling with API Speculative Execution

The previous sections introduced optimizations for open-weight models, which require modifications to the model inference process. Additionally, SGLang works with API-access-only models, such as OpenAI’s GPT-4. However, for these models, we can only call a black-box API endpoint.

This section introduces a new optimization for black-box API models that accelerates execution and reduces the API cost of multi-call SGLang programs using speculative execution. For example, a program may ask the model to generate a description of a character with a multi-call pattern: s += context + "name:" + gen("name", stop="\n") + "job:" + gen("job", stop="\n"). Naively, the two gen primitives correspond to two API calls, meaning that the user needs to pay for the input token fee on the context twice.
In SGLang, we can enable speculative execution on the first call and let it continue the generation of a few more tokens by ignoring the stop condition.
The interpreter keeps the additional generation outputs and matches and reuses them with later primitives.
In certain cases, with careful prompt engineering, the model can correctly match the template with high accuracy, saving us the latency and input costs of one API call.

6 Evaluation

We evaluate the performance of SGLang across diverse LLM workloads.
Subsequently, we conduct ablation studies and case studies to demonstrate the effectiveness of specific components.
SGLang is implemented in PyTorch paszke2019pytorch with custom CUDA kernels from FlashInfer flashinfer and Triton tillet2019triton.

6.1 Setup

Models.
We test dense Llama-2 models touvron2023llama2, sparse mixture of experts Mixtral models jiang2024mixtral, multi-modal LLaVA image liu2023improved and video models zhang2024llavanextvideo, and API model OpenAI’s GPT-3.5.
For open-weight models, the number of parameters ranges from 7 billion to 70 billion, and we use float16 precision.

Hardware.
We run most experiments on AWS EC2 G5 instances, which are equipped with NVIDIA A10G GPUs (24GB). We run 7B models on a single A10G GPU and larger models on multiple A10G GPUs with tensor parallelism shoeybi2019megatron.
We run some additional experiments on A100G (80GB) GPUs.

Baselines.
We compare SGLang against both high-level programming systems with their respective languages and default runtimes, as well as low-level inference engines with standard OpenAI-like Completion APIs.
Unless otherwise stated, we do not turn on optimizations that will change the computation results so that all systems compute the same results.
The baselines include:

•

Guidance guidance, a language for controlling LLMs. We use Guidance v0.1.8 with llama.cpp backend.

•

vLLM kwon2023vllm, a high-throughput inference engine. We use vLLM v0.2.5 and its default API server 2 2
2

RadixAttention has been partially integrated as an optional experimental feature into the latest version of vLLM; therefore, we used an earlier version for comparison..

•

LMQL beurer2023lmql, a query language. We use LMQL v0.7.3 with Hugging Face Transformers backend.

Workloads. We test the following: 5-shot MMLU hendrycks2020measuring and 20-shot HellaSwag zellers2019hellaswag benchmarks.
We decode one token for MMLU and use primitive select to select the answer with the highest probability for HellaSwag.
For the ReAct agent yao2022react and generative agents Park2023GenerativeAgents, we extract the traces from the original papers and replay them.
We use the Tree-of-thought yao2023tree for the GSM-8K problems and
Skeleton-of-thought ning2024skeletonofthought for tip generation.
We use LLM judges with the branch-solve-merge saha2023branch technique;
JSON decoding with a schema specified by a regular expression;
Multi-turn chat with 4 turns, where the input of each turn is randomly sampled between 256-512 tokens. Multi-turn chat (short) means short output (4-8 tokens) and multi-turn chat (long) means long output (256-512 tokens); DSPy retrieval-augmented generation (RAG) pipeline khattab2023dspy in its official example.

Metrics.
We report two performance metrics: throughput and latency. For throughput, we run a sufficiently large batch of program instances to compute the maximum throughput, comparing the number of program instances executed per second (programs per second, p/s). For latency, we execute a single program at a time without batching and report the average latency for multiple instances.

Figure 5 : Normalized throughput on Llama-7B models. Higher is better.

Figure 6 : Normalized latency on Llama-7B models. Lower is better.

6.2 End-to-End Performance

Results on open-weight models. The latency and throughput results are shown in Fig. 5 and Fig. 6. SGLang improves throughput by up to 6.4 × 6.4\times and reduces latency by up to 3.7 × 3.7\times.
These improvements result from KV cache reuse, the exploitation of parallelism within a single program, and faster constrained decoding. Next, we explain the reasons for the speedup in each benchmark.

On MMLU, SGLang can reuse the KV cache of the 5-shot examples with RadixAttention. RadixAttention benefits both throughput and latency. RadixAttention reduces total memory usage by sharing the KV cache, allowing for a larger batch size to improve maximum throughput. RadixAttention also reduces the computation of prefill, thus decreasing the first token latency. On HellaSwag, SGLang reuses the KV cache of both few-shot examples and the common question prefix for multiple choices, resulting in two-level sharing.
For the ReAct and generative agents, SGLang reuses the KV cache of the agent template and previous calls. On Tree-of-thought and Skeleton-of-thought, SGLang parallelizes the generation calls within a single program and reuses the KV cache as much as possible. On JSON decoding, SGLang accelerates decoding by decoding multiple tokens at once with a compressed finite state machine. In multi-turn chat, SGLang reuses the KV cache of the chat history. The speedup is more noticeable for short outputs because KV cache reuse mostly helps reduce the prefix time. For long outputs, because there is not much sharing between different chat sessions and the decoding time dominates, there is almost no speedup.
In the DSPy RAG pipeline, SGLang reuses the KV cache of the common context example.
On these benchmarks, the cache hit rate ranges from 50% to 99%.
Fig. 13 (Appendix) lists the achieved and optimal cache hit rates for all of them, showing that our cache-aware scheduling approaches 96% of the optimal hit rate on average.

We exclude LMQL and Guidance from some of the last five benchmarks due to slow performance and missing functionalities. LMQL’s issues stem from slow token-level processing and an unoptimized backend, while Guidance lacks batching and parallelism support.

Figure 7 : Normalized throughput on Mixtral-8x7B models with tensor parallelism. Higher is better.

Results on larger models with tensor parallelism.
We run larger models, Mixtral-8x7B and Llama-70B, with tensor parallelism on the same set of benchmarks and report the results in Fig. 7 and Fig. 12 (Appendix). The speedup on larger models shows a trend similar to that observed on smaller models, indicating that our optimization generalizes well to larger models.
We omit Guidance and LMQL here because they lack efficient implementations of tensor parallelism.

Results on multi-modal models.
SGLang has native support for multi-modal models with the image and video primitives. The optimizations in this paper are compatible with multi-modal models. For RadixAttention, we compute the hash of the input images and use it as the key in the radix tree, allowing us to reuse the KV cache of the image tokens from the same image.
We run LLaVA-v1.5-7B (image) on llava-bench-in-the-wild and LLaVA-NeXT-34B (video) on ActivityNet. Because these models are not well supported by other baseline systems, we use the model authors’ original implementation in Hugging Face Transformers as the baseline.
As shown in Table 2, SGLang provides throughput up to 6 × 6\times higher on these benchmarks. In llava-bench-in-the-wild, there are multiple questions about the same image, and SGLang runtime reuses the KV cache in this case.

Table 2 : Throughput comparison on multi-modal LLaVA image and video models.

Model

LLaVA-v1.5-7B (image)

LLaVA-NeXT-34B (video)

Author’s original implementation

0.18 image/s

0.02 frame/s

SGLang

1.15 image/s

0.10 frame/s

Production deployment.
SGLang has been deployed in Chatbot Arena chiang2024chatbot to serve open-weight models. Due to low traffic for some models, only one SGLang worker serves each. After one month, we observed a 52.4% RadixAttention cache hit rate for LLaVA-Next-34B liu2024llavanext and 74.1% for Vicuna-33B vicuna2023. Cache hits come from common system messages, frequently reused example images, and multi-turn chat histories. This reduces first-token latency by an average of 1.7 × 1.7\times for Vicuna-33B.

Results on API models. We test a prompt that extracts three fields from a Wikipedia page using OpenAI’s GPT-3.5 model. By using few-shot prompting, the accuracy of API speculative execution is high, and it reduces the cost of input tokens by about threefold due to the extraction of three fields.

Figure 8 : (a)(b) Cache hit rate ablation study. (c) RadixAttention ablation study.

6.3 Ablation Study

Cache hit rate vs. latency/throughput.
Fig. 8 (a)(b) shows the relationship between cache hit rate and performance metrics (first token latency, total latency, batch size, and throughput) on the tree-of-thought benchmark. The figure is obtained by partially disabling matched tokens at runtime. It shows that a higher cache hit rate leads to a larger batch size, higher throughput, and lower latency.

Effectiveness of RadixAttention.
We test the effectiveness of RadixAttention and its components on several representative benchmarks. As shown in Fig. 8 (c), "No Cache" means not using any cache, "No Tree-Structure" means using a simple table-based cache instead of a tree-structured cache, "FCFS Schedule" means using a first-come-first-serve policy instead of our cache-aware scheduling, "Random Schedule" means using a random order to schedule requests, "No Frontend Parallelism" means disabling parallelism in the interpreter, "No Frontend Hint" means disabling sending the fork hints from the interpreters, and "Full optimizations" means we turn on all optimizations. The experimental results show that each of these components is required to achieve the best performance. Disabling parallelism and hints from the frontend interpreter also results in suboptimal runtime performance, highlighting the importance of co-designing the frontend language and runtime.

Overhead of RadixAttention.
We test the overhead of RadixAttention on a benchmark without any KV cache reuse opportunities. The benchmark measures throughput on the ShareGPT dataset.
It takes 74.3 seconds to run 100 requests; however, the time used for managing the RadixAttention data structures is only 0.2 seconds, which is a negligible overhead of less than 0.3%. This is because the complexity of tree operations is linear and small. Thus, we can turn on RadixAttention by default.

Effectiveness of the compressed finite state machine.
We test the effectiveness of the compressed finite state machine and its components on the JSON decoding benchmark.
Experimental results show that the compressed finite state machine increases the throughput by 1.6 × 1.6\times because it can decode multiple tokens at once. In addition, we need to preprocess the state machine and reuse it for a batch of requests. Otherwise, redoing the preprocessing for each request makes the throughput 2.4 × 2.4\times lower.

7 Related Work

Various works have explored the reuse of the KV cache, and many of them are concurrent with our work. Uniquely, our RadixAttention first proposes treating the KV cache as a tree-based LRU cache. It is the first solution that supports multi-level sharing, cache-aware scheduling, frontend-runtime co-scheduling, and distributed cases.
vLLM kwon2023vllm and ChunkedAttention ye2024chunkattention explore some simple reuse cases (e.g., system prompt sharing) but do not cover multi-level tree-structured sharing or LRU caching. PromptCache gim2023prompt proposes the modular reuse of the KV cache beyond the prefix but can impact accuracy by up to a 43% drop. HydraGen juravsky2024hydragen, FlashInfer flashinfer, and ChunkedAttention ye2024chunkattention focus on CUDA kernel optimizations and do not include the concept of an LRU cache. API Serve abhyankar2024apiserve and LLM-SQL liu2024optimizing study KV cache reuse for specific applications such as interleaving with external API calls and relational databases, but they do not have our radix tree or cache-aware scheduling.

Several LLM programming and agent frameworks exist, such as Guidance guidance, LMQL beurer2023lmql, DSPy khattab2023dspy, LangChain langchain, AutoGen wu2023autogen, and LLM Compiler kim2023llm. Guidance and LMQL are most similar to SGLang, and we compare them in Sec. 2. Our innovation lies in novel runtime optimizations for accelerating the proposed programming model. SGLang is compatible with other frameworks and can accelerate them (e.g., the DSPy example in our evaluation). Additionally, SGLang is compatible with many other common inference optimizations yu2022orca; pope2023efficiently; aminabadi2022deepspeed; kwon2023vllm; flashinfer; dao2022flashattention; lin2023awq; hooper2024kvquant; kang2024gear; liu2024kivi; liu2024scissorhands; ge2023model.

8 Future Directions and Conclusion

Future directions.
Despite the progress made with SGLang, several limitations remain that reveal promising directions for future research. These include extending SGLang to support additional output modalities, adapting RadixAttention to operate across multiple levels of the memory hierarchy (e.g., DRAM, Disk) sheng2023flexgen, enabling fuzzy semantic matching within RadixAttention, providing higher-level primitives atop SGLang,
fixing starvation in cache-aware scheduling sheng2023fairness,
and enhancing the SGLang compiler to perform advanced static optimizations such as scheduling and memory planning.

Conclusion.
We introduce SGLang, a framework for efficient programming and executing structured language model programs. SGLang significantly improves the throughput and latency of complex LM programs through novel optimizations like RadixAttention, compressed finite state machines, and a language interpreter. It is a valuable tool for developing advanced prompting techniques and agent workflows. The source code is publicly available.

Acknowledgement

This project is supported by the Stanford Center for Automated Reasoning and gifts from Astronomer, Google, IBM, Intel, Lacework, Microsoft, Mohamed Bin Zayed University of Artificial Intelligence, Nexla, Samsung SDS, Uber, and VMware.
Lianmin Zheng is supported by a Meta Ph.D. Fellowship.
We thank Yuanhan Zhang and Bo Li for the LLaVA-NeXT (video) support.

References

[1]

Reyna Abhyankar, Zijian He, Vikranth Srivatsa, Hao Zhang, and Yiying Zhang.

Apiserve: Efficient api support for large-language model inferencing.

arXiv preprint arXiv:2402.01869, 2024.

[2]

Jean-Baptiste Alayrac, Jeff Donahue, Pauline Luc, Antoine Miech, Iain Barr, Yana Hasson, Karel Lenc, Arthur Mensch, Katherine Millican, Malcolm Reynolds, et al.

Flamingo: a visual language model for few-shot learning.

Advances in Neural Information Processing Systems, 35:23716–23736, 2022.

[3]

Reza Yazdani Aminabadi, Samyam Rajbhandari, Ammar Ahmad Awan, Cheng Li, Du Li, Elton Zheng, Olatunji Ruwase, Shaden Smith, Minjia Zhang, Jeff Rasley, et al.

Deepspeed-inference: enabling efficient inference of transformer models at unprecedented scale.

In SC22: International Conference for High Performance Computing, Networking, Storage and Analysis, pages 1–15. IEEE, 2022.

[4]

Luca Beurer-Kellner, Marc Fischer, and Martin Vechev.

Prompting is programming: A query language for large language models.

Proceedings of the ACM on Programming Languages, 7(PLDI):1946–1969, 2023.

[5]

Tom Brown, Benjamin Mann, Nick Ryder, Melanie Subbiah, Jared D Kaplan, Prafulla Dhariwal, Arvind Neelakantan, Pranav Shyam, Girish Sastry, Amanda Askell, et al.

Language models are few-shot learners.

Advances in neural information processing systems, 33:1877–1901, 2020.

[6]

Sébastien Bubeck, Varun Chandrasekaran, Ronen Eldan, Johannes Gehrke, Eric Horvitz, Ece Kamar, Peter Lee, Yin Tat Lee, Yuanzhi Li, Scott Lundberg, et al.

Sparks of artificial general intelligence: Early experiments with gpt-4.

arXiv preprint arXiv:2303.12712, 2023.

[7]

Wei-Lin Chiang, Zhuohan Li, Zi Lin, Ying Sheng, Zhanghao Wu, Hao Zhang, Lianmin Zheng, Siyuan Zhuang, Yonghao Zhuang, Joseph E. Gonzalez, Ion Stoica, and Eric P. Xing.

Vicuna: An open-source chatbot impressing gpt-4 with 90%* chatgpt quality, March 2023.

[8]

Wei-Lin Chiang, Lianmin Zheng, Ying Sheng, Anastasios Nikolas Angelopoulos, Tianle Li, Dacheng Li, Hao Zhang, Banghua Zhu, Michael Jordan, Joseph E Gonzalez, et al.

Chatbot arena: An open platform for evaluating llms by human preference.

arXiv preprint arXiv:2403.04132, 2024.

[9]

Aakanksha Chowdhery, Sharan Narang, Jacob Devlin, Maarten Bosma, Gaurav Mishra, Adam Roberts, Paul Barham, Hyung Won Chung, Charles Sutton, Sebastian Gehrmann, et al.

Palm: Scaling language modeling with pathways.

arXiv preprint arXiv:2204.02311, 2022.

[10]

Tri Dao, Dan Fu, Stefano Ermon, Atri Rudra, and Christopher Ré.

Flashattention: Fast and memory-efficient exact attention with io-awareness.

Advances in Neural Information Processing Systems, 35:16344–16359, 2022.

[11]

Suyu Ge, Yunan Zhang, Liyuan Liu, Minjia Zhang, Jiawei Han, and Jianfeng Gao.

Model tells you what to discard: Adaptive kv cache compression for llms.

In The Twelfth International Conference on Learning Representations, 2023.

[12]

In Gim, Guojun Chen, Seung-seob Lee, Nikhil Sarda, Anurag Khandelwal, and Lin Zhong.

Prompt cache: Modular attention reuse for low-latency inference.

arXiv preprint arXiv:2311.04934, 2023.

[13]

Guidance AI.

A guidance language for controlling large language models.

https://github.com/guidance-ai/guidance.

Accessed: 2023-11.

[14]

Dan Hendrycks, Collin Burns, Steven Basart, Andy Zou, Mantas Mazeika, Dawn Song, and Jacob Steinhardt.

Measuring massive multitask language understanding.

In International Conference on Learning Representations, 2020.

[15]

Coleman Hooper, Sehoon Kim, Hiva Mohammadzadeh, Michael W Mahoney, Yakun Sophia Shao, Kurt Keutzer, and Amir Gholami.

Kvquant: Towards 10 million context length llm inference with kv cache quantization.

arXiv preprint arXiv:2401.18079, 2024.

[16]

Hugging Face.

Text generation inference.

https://github.com/huggingface/text-generation-inference.

Accessed: 2023-11.

[17]

Albert Q Jiang, Alexandre Sablayrolles, Antoine Roux, Arthur Mensch, Blanche Savary, Chris Bamford, Devendra Singh Chaplot, Diego de las Casas, Emma Bou Hanna, Florian Bressand, et al.

Mixtral of experts.

arXiv preprint arXiv:2401.04088, 2024.

[18]

Jordan Juravsky, Bradley Brown, Ryan Ehrlich, Daniel Y Fu, Christopher Ré, and Azalia Mirhoseini.

Hydragen: High-throughput llm inference with shared prefixes.

arXiv preprint arXiv:2402.05099, 2024.

[19]

Hao Kang, Qingru Zhang, Souvik Kundu, Geonhwa Jeong, Zaoxing Liu, Tushar Krishna, and Tuo Zhao.

Gear: An efficient kv cache compression recipefor near-lossless generative inference of llm.

arXiv preprint arXiv:2403.05527, 2024.

[20]

Omar Khattab, Arnav Singhvi, Paridhi Maheshwari, Zhiyuan Zhang, Keshav Santhanam, Sri Vardhamanan, Saiful Haq, Ashutosh Sharma, Thomas T. Joshi, Hanna Moazam, Heather Miller, Matei Zaharia, and Christopher Potts.

Dspy: Compiling declarative language model calls into self-improving pipelines.

arXiv preprint arXiv:2310.03714, 2023.

[21]

Sehoon Kim, Suhong Moon, Ryan Tabrizi, Nicholas Lee, Michael W Mahoney, Kurt Keutzer, and Amir Gholami.

An llm compiler for parallel function calling.

arXiv preprint arXiv:2312.04511, 2023.

[22]

Michael Kuchnik, Virginia Smith, and George Amvrosiadis.

Validating large language models with relm.

Proceedings of Machine Learning and Systems, 5, 2023.

[23]

Woosuk Kwon, Zhuohan Li, Siyuan Zhuang, Ying Sheng, Lianmin Zheng, Cody Hao Yu, Joseph Gonzalez, Hao Zhang, and Ion Stoica.

Efficient memory management for large language model serving with pagedattention.

In Proceedings of the 29th Symposium on Operating Systems Principles, pages 611–626, 2023.

[24]

LangChain AI.

Langchain.

https://github.com/langchain-ai/langchain.

Accessed: 2023-11.

[25]

Yujia Li, David Choi, Junyoung Chung, Nate Kushman, Julian Schrittwieser, Rémi Leblond, Tom Eccles, James Keeling, Felix Gimeno, Agustin Dal Lago, et al.

Competition-level code generation with alphacode.

Science, 378(6624):1092–1097, 2022.

[26]

Ji Lin, Jiaming Tang, Haotian Tang, Shang Yang, Xingyu Dang, and Song Han.

Awq: Activation-aware weight quantization for llm compression and acceleration.

arXiv preprint arXiv:2306.00978, 2023.

[27]

Haotian Liu, Chunyuan Li, Yuheng Li, and Yong Jae Lee.

Improved baselines with visual instruction tuning.

arXiv preprint arXiv:2310.03744, 2023.

[28]

Haotian Liu, Chunyuan Li, Yuheng Li, Bo Li, Yuanhan Zhang, Sheng Shen, and Yong Jae Lee.

Llava-next: Improved reasoning, ocr, and world knowledge, January 2024.

[29]

Shu Liu, Asim Biswal, Audrey Cheng, Xiangxi Mo, Shiyi Cao, Joseph E Gonzalez, Ion Stoica, and Matei Zaharia.

Optimizing llm queries in relational workloads.

arXiv preprint arXiv:2403.05821, 2024.

[30]

Xiaoxia Liu, Jingyi Wang, Jun Sun, Xiaohan Yuan, Guoliang Dong, Peng Di, Wenhai Wang, and Dongxia Wang.

Prompting frameworks for large language models: A survey.

arXiv preprint arXiv:2311.12785, 2023.

[31]

Zichang Liu, Aditya Desai, Fangshuo Liao, Weitao Wang, Victor Xie, Zhaozhuo Xu, Anastasios Kyrillidis, and Anshumali Shrivastava.

Scissorhands: Exploiting the persistence of importance hypothesis for llm kv cache compression at test time.

Advances in Neural Information Processing Systems, 36, 2024.

[32]

Zirui Liu, Jiayi Yuan, Hongye Jin, Shaochen Zhong, Zhaozhuo Xu, Vladimir Braverman, Beidi Chen, and Xia Hu.

Kivi: A tuning-free asymmetric 2bit quantization for kv cache.

arXiv preprint arXiv:2402.02750, 2024.

[33]

Xuefei Ning, Zinan Lin, Zixuan Zhou, Zifu Wang, Huazhong Yang, and Yu Wang.

Skeleton-of-thought: Prompting LLMs for efficient parallel generation.

In The Twelfth International Conference on Learning Representations, 2024.

[34]

NVIDIA.

Tensorrt-llm.

https://github.com/NVIDIA/TensorRT-LLM.

Accessed: 2023-11.

[35]

OpenAI.

Gpt-4 technical report, 2023.

[36]

Joon Sung Park, Joseph C. O’Brien, Carrie J. Cai, Meredith Ringel Morris, Percy Liang, and Michael S. Bernstein.

Generative agents: Interactive simulacra of human behavior.

In In the 36th Annual ACM Symposium on User Interface Software and Technology (UIST ’23), UIST ’23, New York, NY, USA, 2023. Association for Computing Machinery.

[37]

Adam Paszke, Sam Gross, Francisco Massa, Adam Lerer, James Bradbury, Gregory Chanan, Trevor Killeen, Zeming Lin, Natalia Gimelshein, Luca Antiga, et al.

Pytorch: An imperative style, high-performance deep learning library.

Advances in neural information processing systems, 32, 2019.

[38]

Shishir G. Patil, Tianjun Zhang, Xin Wang, and Joseph E. Gonzalez.

Gorilla: Large language model connected with massive apis.

arXiv preprint arXiv:2305.15334, 2023.

[39]

Reiner Pope, Sholto Douglas, Aakanksha Chowdhery, Jacob Devlin, James Bradbury, Jonathan Heek, Kefan Xiao, Shivani Agrawal, and Jeff Dean.

Efficiently scaling transformer inference.

Proceedings of Machine Learning and Systems, 5, 2023.

[40]

Swarnadeep Saha, Omer Levy, Asli Celikyilmaz, Mohit Bansal, Jason Weston, and Xian Li.

Branch-solve-merge improves large language model evaluation and generation.

arXiv preprint arXiv:2310.15123, 2023.

[41]

Timo Schick, Jane Dwivedi-Yu, Roberto Dessì, Roberta Raileanu, Maria Lomeli, Luke Zettlemoyer, Nicola Cancedda, and Thomas Scialom.

Toolformer: Language models can teach themselves to use tools.

arXiv preprint arXiv:2302.04761, 2023.

[42]

Ying Sheng, Shiyi Cao, Dacheng Li, Banghua Zhu, Zhuohan Li, Danyang Zhuo, Joseph E Gonzalez, and Ion Stoica.

Fairness in serving large language models.

arXiv preprint arXiv:2401.00588, 2023.

[43]

Ying Sheng, Lianmin Zheng, Binhang Yuan, Zhuohan Li, Max Ryabinin, Beidi Chen, Percy Liang, Christopher Ré, Ion Stoica, and Ce Zhang.

Flexgen: high-throughput generative inference of large language models with a single gpu.

In International Conference on Machine Learning, pages 31094–31116. PMLR, 2023.

[44]

Mohammad Shoeybi, Mostofa Patwary, Raul Puri, Patrick LeGresley, Jared Casper, and Bryan Catanzaro.

Megatron-lm: Training multi-billion parameter language models using model parallelism.

arXiv preprint arXiv:1909.08053, 2019.

[45]

Vikranth Srivatsa, Zijian He, Reyna Abhyankar, Dongming Li, and Yiying Zhang.

Preble: Efficient distributed prompt scheduling for llm serving.

2024.

[46]

Theodore Sumers, Shunyu Yao, Karthik Narasimhan, and Thomas Griffiths.

Cognitive architectures for language agents.

Transactions on Machine Learning Research, 2023.

[47]

Gemini Team, Rohan Anil, Sebastian Borgeaud, Yonghui Wu, Jean-Bapt
