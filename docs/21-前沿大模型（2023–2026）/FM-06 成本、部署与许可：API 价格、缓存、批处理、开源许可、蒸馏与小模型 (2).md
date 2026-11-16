# FM-06 成本、部署与许可：API 价格、缓存、批处理、开源许可、蒸馏与小模型

> 主题库：前沿大模型（2023–2026）　｜　源文件：（纯文本条目）　｜　8,526 字符　｜　33 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

## 一句话结论

到 2026 年 11 月，模型成本已经分裂成三张互不通用的账本：**按 token 的价目表、按任务的实测成本、按许可证的隐性成本**。名义单价对预算的解释力持续下降——缓存把同一份输入的价格拉开 20–50 倍，峰谷与批处理再砍一半，而第三方实测的"每任务成本"排序和"每百万 token 单价"排序经常不一致。与此同时，"开放权重"从 Apache-2.0 的默认选项分裂成 MIT、Apache-2.0 与自定义许可三档，"开源"这个词越来越需要看许可证正文。

## 关键变化表

表 1：官方 API 单价（USD / 1M tokens，2026-11-15 抓取）

| 模型 | 输入 | 缓存读 | 输出 | 关键限制 |
|---|---|---|---|---|
| `deepseek-flash` 谷时 | 0.15 | 0.003 | 0.60 | 谷时=峰时一半 |
| `deepseek-flash` 峰时 | 0.30 | 0.006 | 1.20 | 峰时 UTC 01–04、06–10 工作日 |
| `deepseek-v4-pro` 谷时 | 0.66 | 0.022 | 1.98 | 不支持视觉 |
| `deepseek-v4-pro` 峰时 | 1.32 | 0.044 | 3.96 | 并发上限 500 |
| Claude Haiku 5.5（≤100K prompt） | 0.10 | — | 0.50 | 超 100K 后跳档 |
| Claude Haiku 5.5（>100K prompt） | 0.50 | — | 2.50 | 5 倍跳档 |
| Claude Opus 5.5 | 4.00 | 0.20 | 20.00 | Fast mode 8/40；美国境内 1.1x |
| Claude Mythos 5.2 | 8.00 | — | 40.00 | 需审核；Fast 16/80 |
| Gemini 4 Carbon / Argon | 2.00 | 0.10 | 10.00 | 2026-12-01 起 4.00/0.20/20.00 |
| Gemini 4 Hydrogen | 1.50 | 0.075 | 7.50 | 有免费额度 |
| Gemini 3.8 Flash | 0.75 | 0.075 | 3.75 | 优惠价至 2026-12-31 |

表 2：折扣机制横向对照

| 机制 | DeepSeek | Anthropic | Google | 实际效果 |
|---|---|---|---|---|
| 缓存命中 | 0.003 vs 0.15（50 倍） | 0.20 vs 4.00（20 倍） | 0.10 vs 2.00（20 倍） | 长前缀与多轮 agent 的决定性变量 |
| 批处理 | 无（以峰谷替代） | 省 50% | 省 50%（Carbon 1.00/5.00） | 离线任务直接减半 |
| 时段 | 谷价=峰价一半 | 无 | 无 | 可调度性带来 2 倍价差 |
| 长 prompt | 无阶梯 | Haiku 超 100K 跳 5 倍 | 隐式缓存，无显式缓存（Carbon） | 长上下文不是线性定价 |
| 上游工具 | Search / Fetch $0.75/千次；OCR $0.225/千页 | 未在页内列 | Grounding $14/千次（5000 次/月免费共享） | 工具费可与推理费同量级 |

表 3：开放权重模型的许可实测（Hugging Face API，2026-11-15）

| 仓库 | 许可字段 | 许可名 | 创建时间 | 下载量 |
|---|---|---|---|---|
| `openai/gpt-oss-120b` | apache-2.0 | — | 2025-08-04 | 2,418,465 |
| `deepseek-ai/DeepSeek-V4.1-Flash` | mit | — | 2026-09-10 | 1,694,181 |
| `zai-org/GLM-5.3` | other | glm-5.3 | 2026-08-25 | 1,761,619 |
| `moonshotai/Kimi-K3` | other | kimi-k3 | 2026-06-13 | 783,375 |

## 一、名义单价正在失去解释力

先看一组同一周、同一评测框架下的数字。Artificial Analysis 的 Intelligence Index 与 Cost per Task（2026-11-09）：

- Muse Ember (max)：36 分，$0.14/任务；xhigh 34 分，$0.12/任务
- Claude Haiku 5.5 (max)：36 分，$0.41/任务
- DeepSeek V4.1 Flash 1017 (max)：33 分，$0.60/任务
- GLM-5.3-Flash：28 分，$0.58/任务
- Muse Wonder (max)：44 分，$6.95/任务

Muse Ember 与 Claude Haiku 5.5 的输入/输出单价完全相同（$0.10/$0.50），但每任务成本差近 3 倍。原因被 AA 写得很清楚：缓存输入是这批评测里成本的大头，Muse Ember 的缓存价低 60%；同时它的输出 token 用量约为 Haiku 5.5 的三分之一。也就是说，**同一个价目表，可以算出三种成本**——取决于你的 prompt 复用率与输出长度分布。

这正是"按 token 采购"和"按任务采购"分道扬镳的地方。前者适合做容量规划，后者适合做产品定价；拿错账本，预算会差一个数量级。

## 二、缓存：真正的上下文价格

三家都把缓存做成了定价结构，而不是优化技巧。DeepSeek 的 cache hit 与 miss 是 50 倍关系（$0.003 对 $0.15，谷时）；Claude Opus 5.5 的缓存读 $0.20 对标准输入 $4.00，是 20 倍；Gemini 4 Carbon 是 $0.10 对 $2.00，同样是 20 倍。Anthropic 在 Haiku 页面明确写"prompt caching 最高省 90%"。

但缓存的可用性并不对等。Google 在 Gemini 4 Carbon 的价目里注明"implicit caching only（不支持显式缓存）"——意味着开发者不能手动创建缓存条目，收益取决于请求模式是否自然重复；而 DeepSeek 与 Anthropic 都允许显式指定缓存前缀。对 agent 系统来说，这是一个架构级差异：显式缓存可以把系统提示、工具 schema、代码库索引固定成前缀，隐式缓存只能靠"碰巧重复"。

还有一个容易忽略的点：便宜的是缓存「读」这一侧，写入侧通常单独计价或加价（Anthropic 对缓存写入另计；Gemini 4 Carbon 只有隐式缓存，没有显式写入价）。预算模型只写"缓存省 20 倍"而不区分读写，会在首次填充时低估成本。

## 三、时段与批处理：把"什么时候跑"变成设计变量

DeepSeek 是唯一把峰谷写进价目表的主流厂商：峰时定义为 UTC 01:00–04:00 与 06:00–10:00 的周一至周五（排除中国法定节假日），其余时段（含周末与节假日全天）为谷时，谷价正好是峰价的一半。对可延迟的任务（批量摘要、离线评测、数据清洗），把调度窗口移到谷时等于直接打五折，且不需要改代码。

批处理是另一条路：Google 的 Gemini 4 Carbon batch 档是 $1.00/$5.00（标准档的一半），Anthropic 也在 Haiku 页面标注 batch 省 50%。DeepSeek 没有 batch 档，用峰谷替代。三者不能叠加：不存在"谷时 + batch"的四折。

## 四、长 prompt 与涨价日期：定价是时间的函数

两条时间相关的规则必须在架构里写死：

1. **长 prompt 阶梯。** Claude Haiku 5.5 在 prompt 超过 100K token 后，输入从 $0.10 跳到 $0.50、输出从 $0.50 跳到 $2.50。也就是说，同一份 120K 的文档，成本不是 100K 文档的 1.2 倍，而是 6 倍。
2. **限时价与涨价日。** Gemini 4 Carbon/Argon 现价 $2.00/$10.00 仅到 2026-11-30，12-01 起变为 $4.00/$20.00，缓存从 $0.10 变 $0.20；Gemini 3.8 Flash 的 $0.75/$3.75 是"至 2026-12-31"的优惠价。DeepSeek 的 TTS/ASR 也标着"限时 5 折"。

这意味着任何超过两个月的预算模型都必须带一个"价格生效日期"字段，否则会在涨价日静默超支。

## 五、部署保真度：你买的不是模型，是推理栈

Artificial Analysis 的 Endpoint Accuracy Index v1.1 给出了这个领域最不客气的一组数：在其评测的 103 个 serverless endpoint 中，24 个的准确率统计显著低于自托管参考实现。参考实现是各家官方权重、官方推荐精度，跑在 NVIDIA B200/B300/GB200 上的 vLLM 或 SGLang。

差距的来源被明确列出：量化权重、自定义 kernel、调优过的推理栈、输出长度上限，以及偶发的 bug。对采购方来说，这改变了比价方式——同一模型在不同供应商处的"有效能力"不同，便宜 30% 的 endpoint 可能在长上下文或工具调用上掉几个点。合理的做法是把 endpoint 当作独立变量做 A/B，而不是把模型名当作质量保证。

## 六、开放权重与许可：三档而不是一档

表 3 的四行数据说明，"开源模型"已经不是一个许可类别：

- **Apache-2.0**（`openai/gpt-oss-120b`）：最宽松，允许商用、修改、再分发，只需保留声明。
- **MIT**（`deepseek-ai/DeepSeek-V4.1-Flash`）：与 Apache-2.0 接近，义务更少。
- **自定义许可**（`zai-org/GLM-5.3` 的 `glm-5.3`、`moonshotai/Kimi-K3` 的 `kimi-k3`）：Hugging Face 的许可字段是 `other`，实际条款需要逐条读，通常包含使用范围、署名、商业规模或再分发限制。

下载量也提供了一个侧面信号：gpt-oss-120b 累计 241 万次、GLM-5.3 176 万次、DeepSeek-V4.1-Flash 169 万次、Kimi-K3 78 万次。开放权重已经不是"研究玩具"，而是有真实部署规模的分发渠道。此外，Google 的 Gemma 4 也在其开放模型序列中，AA 的开放权重覆盖名单还包括 GLM-5.3、Kimi K3、gpt-oss-120b、Muse Spark Open 与 DeepSeek V4.1 Flash 1017。

## 七、蒸馏：开放权重时代的成本外溢

蒸馏是这套经济学的阴影面。用开放权重模型的输出训练自己的小模型，成本远低于从头训练；而对闭源厂商来说，这等于把自己的能力以极低边际成本转移出去。OpenAI 在 2026 年发布了题为《disrupting a coordinated model distillation campaign》的文章（可从其新闻 RSS 索引到该 slug），说明"协调式模型蒸馏"已经升级为需要公开应对的问题。

本文没有读到该文正文，因此不引用其细节；但从定价结构可以推出一个结论：**MIT/Apache-2.0 许可 + 低推理价 = 极低的蒸馏成本**。这也解释了为什么部分厂商在许可上收紧（自定义许可），而另一些厂商继续放宽——前者把护城河放在权重上，后者把护城河放在服务、数据与生态上。

## 八、小模型经济学：36 分那条线

2026 年的一个明显趋势是"够用线"下移。AA 的数据里，36 分对应 $0.14/任务（Muse Ember）到 $0.41/任务（Claude Haiku 5.5）；再往下，Gemini 3.8 Flash 这类 $0.75/$3.75 的型号承担高频、低难度的流量。对产品团队，这意味着三层路由几乎是标配：便宜档做分类/抽取/路由，中档做主力生成，旗舰只处理长尾难题。

但"够用线"不等于"全能"。同一份 AA 评测里，Muse Ember 的 AA-Omniscience 准确率只有 21.1%，明显低于 Claude Haiku 5.5 的 34.7%；它靠"拒答 64%"把幻觉率压到 17.4%。换句话说，低成本模型的知识面窄，但它在不知道时会闭嘴——这对客服、检索增强类应用是可接受的权衡，对开放式研究助手则不是。

## 对开发者的实际影响

1. **预算模型要写成三段式**：`成本 = 输入 token × (命中率 × 缓存价 + (1-命中率) × 输入价) + 输出 token × 输出价 + 工具调用次数 × 单价`。少写任何一段都会系统性低估。
2. **把缓存命中率当一级 KPI。** 20–50 倍的价差意味着，命中率从 50% 提到 90% 的收益，通常大于换一家更便宜的供应商。
3. **可延迟任务全部进批处理或谷时。** 50% 的折扣不需要改代码，只需要改调度。
4. **采购前做 endpoint A/B。** 同一模型不同供应商的准确率差异是统计显著的，价格差要除以能力差再比较。
5. **许可证按"义务"审查，而不是按"开源"标签。** Apache-2.0/MIT 与自定义许可在再分发、商业规模、署名上的义务完全不同。

## 常见坑与局限

- **忘记 thinking token 计入输出。** Google 在 Gemini 定价页明确写"output price (including thinking tokens)"；推理档位越高，账单越偏离"输出长度"的直觉。
- **把图像/视频按"张"估算。** 这些输入最终折算成 token，分辨率与时长直接放大账单；Gemini Omni 1.2 Flash 的输入价是按 text/image/video 统一 $1.50 计，但视频的 token 量取决于帧数与分辨率。
- **忽略免费额度的共享池。** Gemini 的 Grounding 是"5,000 次/月、Gemini 3 及更新模型共享"，超量后 $14/千次且有每日 1,500 次上限。
- **把限时价写进长期合同。** 2026-12-01 与 2026-12-31 两个日期之后，Gemini 部分型号的实际单价翻倍。
- **混淆"开放权重"与"OSI 认可的开源"。** 自定义许可通常不满足 OSI 定义，商用前必须读条款。
- **忽略并发上限。** DeepSeek 的 2500（flash）/500（pro）并发是硬约束，高峰期排队会改变"每任务成本"的实际值。

## 来源

- DeepSeek《Models & Pricing》（模型、上下文、峰谷价、工具与语音计价、并发上限）：https://api-docs.deepseek.com/quick_start/pricing （访问 2026-11-15）
- Anthropic《Claude Opus》产品页（Opus 5.5 价格、缓存读价、Fast mode、美国境内 1.1x）：https://www.anthropic.com/claude/opus （访问 2026-11-15）
- Anthropic《Claude Haiku》产品页（Haiku 5.5 两档价格、缓存 90%、batch 50%）：https://www.anthropic.com/claude/haiku （访问 2026-11-15）
- Anthropic《Claude Mythos》产品页（Mythos 5.2 价格与 Fast mode）：https://www.anthropic.com/claude/mythos （访问 2026-11-15）
- Google《Gemini Developer API Pricing》（Gemini 4 Carbon/Argon/Hydrogen、3.8 Flash、Omni、batch、grounding）：https://ai.google.dev/gemini-api/docs/pricing （访问 2026-11-15）
- Artificial Analysis《Meta's Muse Ember is the cheapest model scoring 36 on the Intelligence Index》（每任务成本、缓存占比、输出 token 用量）：https://artificialanalysis.ai/articles/meta-s-muse-ember-is-the-cheapest-model-scoring-36-on-the-intelligence-index （访问 2026-11-15）
- Artificial Analysis《Endpoint Accuracy Index v1.1》（103 个 endpoint、24 个低于参考、自托管参考栈）：https://artificialanalysis.ai/articles/endpoint-accuracy-index-v1-1 （访问 2026-11-15）
- Hugging Face API：https://huggingface.co/api/models/openai/gpt-oss-120b 、https://huggingface.co/api/models/deepseek-ai/DeepSeek-V4.1-Flash 、https://huggingface.co/api/models/zai-org/GLM-5.3 、https://huggingface.co/api/models/moonshotai/Kimi-K3 （访问 2026-11-15）
- OpenAI News RSS（《disrupting a coordinated model distillation campaign》等条目索引）：https://openai.com/news/rss.xml （访问 2026-11-15）

## 不确定项

- **OpenAI 的官方 API 价目表未取到。** openai.com/api/pricing 返回 JS 空壳，platform.openai.com/docs/pricing 返回 HTTP 403，文章页正文由客户端渲染。因此本文不含 GPT-6 系列单价，仅有"GPT-6.1 Sol 定价为 Astra 标准 API 输入/输出价格的五分之一"这一句官方副标题。
- **Anthropic 的缓存写价、Gemini 各型号的免费额度细则未逐条核实**；本文只使用页面上明确写出的数字。
- **GLM-5.3 与 Kimi-K3 的自定义许可具体条款未读**（只核到 Hugging Face 的 `license: other` 与许可名），商用前需查阅原文。
- **"每任务成本"依赖 AA 的私有任务集与 effort 设定**，不可直接外推到其他任务分布；它适合做相对排序，不适合做绝对预算。
- **Gemini 4 系列的上下文窗口与 thinking token 计费比例未取到**（models 文档页为客户端渲染）。
- **蒸馏文章正文未读**，本文对该议题只做结构性判断，不引用具体案例与数字。
