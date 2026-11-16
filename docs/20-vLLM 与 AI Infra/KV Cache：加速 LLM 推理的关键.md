# KV Cache：加速 LLM 推理的关键

> 主题库：vLLM 与 AI Infra　｜　源文件：（纯文本条目）　｜　3,283 字符　｜　104 个片段　｜　更新于 2026-11-13
来源: https://cnblogs.com/LexLuc/p/18716439

# https://cnblogs.com/LexLuc/p/18716439

来源: https://cnblogs.com/LexLuc/p/18716439
抓取方式: read_page

会员

周边

新闻

博问

闪存

赞助商

Chat2DB

所有博客

当前博客

我的博客
我的园子
账号设置
会员中心

简洁模式...

退出登录

注册
登录

Lex个人随想乡

Attention before pay attention

博客园

首页

新随笔

联系

订阅

管理

KV Cache：加速LLM推理的关键

1. KV 缓存是什么？

KV 缓存（KV Cache）是一种优化大语言模型（LLM, Large Language Models）推理速度的关键技术。最近爆火的DeepSeek，其首创的MLA技术，使得KV Cache降低了93%，在大语言模型的训练和推理上有效降低了对高性能GPU的性能要求（比如原先需要H100才能训练和推理，现在H20就能满足）。

在生成文本时，GPT 这样的模型需要不断计算自注意力（Self-Attention）。但实际上，我们可以缓存 Key 和 Value（K/V）向量，避免重复计算，从而显著加速推理。

先来看一个直观的对比：

不开启 KV 缓存 → 42 秒

使用 KV 缓存 → 9 秒，推理速度提升约 5 倍

为什么 KV 缓存能让推理速度提升这么多？让我们深入解析。

1. 什么是 KV Cache？

大语言模型在生成文本时，模型需要在每一步计算 注意力（Attention） 。标准的 Transformer 计算 自注意力 时，每次都需要重新计算 Key（K） 和 Value（V） 矩阵，并与新的 Query（Q） 进行注意力计算：

\[A_t = \text{softmax} \left( \frac{Q_t K_{\leq t}^T}{\sqrt{d_k}} \right) V_{\leq t}
\]

但是， Key 和 Value 其实是静态的 （不会变化），它们在过去的时间步已经计算过了。因此，我们可以 缓存（Cache）这些 Key/Value ，避免重复计算，提高生成效率。

2. 传统 Transformer 计算的性能瓶颈

标准 Transformer 计算 Attention（没有 KV Cache）

每次生成新 token \(x_t\) 时：

重新计算所有 \(K\) 和 \(V\) （包括所有历史 token）。

计算新的 Query \(Q_t\) 。

计算注意力 \(\text{softmax}(Q_t K_{\leq t}^T) V_{\leq t}\) 。

计算复杂度 ：

每个 Token 计算 Attention 需要 \(O(t d_k)\) （因为每个新 Query 需要和 \(t\) 个 Key 进行点积）。

生成 \(T\) 个 Token 的总复杂度为：
\[O(T^2 d_k)
\]

随着生成长度增加，计算量呈二次增长！

3. KV Cache 的优化

KV Cache 机制

缓存 Key/Value 矩阵 ：

每个 Token 计算 Key/Value 后 存起来 ，而不是在每个时间步重新计算。

只需要存储形状为 $$(t, d_k)$$ 和 $$ (t, d_v) $$ 的矩阵。

下一个 Token 计算时 ：

只需计算新的 Query $ Q_{t+1} $。

直接使用缓存的 Key/Value 进行注意力计算：
\[A_{t+1} = \text{softmax} \left( \frac{Q_{t+1} K_{\leq t}^T}{\sqrt{d_k}} \right) V_{\leq t}
\]

然后只计算 $ K_{t+1} $ 和 $ V_{t+1} $，并追加到缓存中。

计算复杂度优化 ：

由于只计算新的 $ K, V $，每个 token 的计算变为 $ O(t d_k) $。

生成 $ T $ 个 Token 的总复杂度变为：
\[O(T d_k)
\]

从二次复杂度 $ O(T^2 d_k) $ 降到线性复杂度 $ O(T d_k) $ ，极大提升推理速度。

4. KV Cache 如何提升推理速度

（1）减少重复计算

在 没有 KV Cache 的情况下 ：

每生成一个新 token，都需要重新计算所有 Key/Value。

计算量随着 Token 长度增加 呈二次增长 ，导致长文本生成非常慢。

使用 KV Cache 后 ：

只需要计算 当前新 token 的 Key/Value ，并追加到缓存中。

计算量随 Token 长度增加仅为 线性增长 ，显著加速推理。

（2）减少显存（VRAM）占用

标准 Transformer 需要存储完整的 Attention 计算历史，占用大量显存。

KV Cache 仅存储 Key/Value 矩阵 ，相比于存储整个计算图，显存占用大幅降低。

（3）加速 GPU 计算

减少矩阵运算规模 ：

原始 Transformer 需要计算 $$ O(t^2 d_k) $$ 规模的矩阵乘法。

KV Cache 只需要 $ O(t d_k) $，能更好地利用 GPU 并行计算能力。

提升批处理（Batch Processing）性能 ：

由于不需要重复计算过去的 Key/Value，GPU 可以更快地处理多个请求，适用于 大规模文本生成任务 （如 ChatGPT）。

5. KV Cache 在实际推理中的应用

在实际使用中，KV Cache 通过 动态扩展缓存 ，让模型在生成过程中 不断追加 Key/Value，而不是重新计算整个序列 。在优化框架中，通常：

缓存格式 ：

多头注意力情况下，缓存维度为：
\[(\text{batch_size}, \text{num_heads}, \text{seq_len}, d_k)
\]

缓存更新策略 ：

计算 $ Q_t $。

读取之前存储的 Key/Value。

计算 Attention 得到输出。

计算新的 Key/Value 并追加到缓存。

示意流程：

计算 Query $ Q_t $。

直接从 KV Cache 读取 $ K_{\leq t} $ 和 $ V_{\leq t} $。

计算 Attention 权重 $ A_t $。

计算 Attention 输出 $ O_t $。

计算新 Key/Value 并存入 Cache，供下一个 Token 使用。

6. 结论

KV Cache 极大地优化了 GPT 模型的推理速度 ，其主要贡献包括：

减少 Key/Value 计算量 （避免重复计算）。

降低计算复杂度 （从 $ O(T^2 d_k) $ 降到 $ O(T d_k) $）。

减少显存占用 （仅存储 Key/Value，不存储完整计算图）。

加速 GPU 计算 （减少不必要的矩阵计算）。

提升大规模推理能力 （适用于 ChatGPT、文档生成等场景）。

这个优化方案使得 GPT 模型能够 高效地进行长文本生成 ，是现代大语言模型推理的关键技术之一。

这就是 KV 缓存的核心原理，也是 GPT 等 LLM 运行高效的关键。

posted @
2025-02-15 02:18
LexLuc
阅读( 4554 )
评论( 0 )

收藏
举报

刷新页面 返回顶部

博客园
&copy; 2004-2026

浙公网安备 33010602011771号
浙ICP备2021040463号-3
