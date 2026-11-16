# FM-04 多模态、长上下文与 Agentic 能力：原生图像/视频、百万上下文、computer use、tool calling

> 主题库：前沿大模型（2023–2026）　｜　源文件：（纯文本条目）　｜　7,542 字符　｜　26 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

## 一句话结论

到 2026 年 11 月，"百万上下文"与"原生多模态"已经从卖点变成入场券，真正的分水岭变成两件更朴素的事：**同一代模型内部的模态分档**，和**服务端保真度**。DeepSeek 把 Vision 只留给 `deepseek-flash`，旗舰 `deepseek-v4-pro` 明确标注不支持；Artificial Analysis 实测的 103 个 serverless endpoint 里有 24 个在准确率上统计显著低于自托管参考。上下文长度写在规格表里，能不能用满写在计价器和供应商的推理栈里。

## 关键变化表

表 1：能力迁移的三个断面（按官方页面与公开论文可查的事实归纳）

| 维度 | 2023 年底 | 2025 年 | 2026 年 11 月 |
|---|---|---|---|
| 上下文 | 32K–128K 为主 | 128K 普及，1M 试点 | 1M 成旗舰规格（DeepSeek 1M、Claude Opus 系列产品页写 1M） |
| 最大输出 | 4K–8K | 32K–64K | 384K（DeepSeek 两档均为 384K） |
| 图像输入 | 外挂视觉模型/OCR 拼接 | 原生多模态 | 原生，但**同代分档**：flash 有、pro 无 |
| 视频/音频 | 基本缺席 | 试点 | Gemini Omni 1.2 Flash 文本/图像/视频同价输入 |
| 执行能力 | function calling | computer use 试点 | computer use 进入主评测（OSWorld 2.1 85% 以上） |
| 计费口径 | 按 token | 按 token + 缓存 | 按 token / 按任务（Cost per Task）双账本 |

表 2：可核实的官方规格对照（2026-11-15）

| 模型 | 上下文 | 最大输出 | 视觉 | 依据 |
|---|---|---|---|---|
| `deepseek-flash`（V4.1-Flash-1017） | 1M | 384K | 支持 | DeepSeek 定价页 |
| `deepseek-v4-pro`（V4-Pro-0813） | 1M | 384K | **不支持** | 同上 |
| Claude Opus 5.5 | 1M（产品页描述） | 页内未明示 | 视觉 + computer use | Anthropic 产品页 |
| Gemini 4 Carbon | 官方只写"长解码生成最高 1M tokens" | 未明示 | 原生多模态 | Google 定价页 |
| GPT-6.1 Sol | 未取到数值 | 未取到 | 编码 / computer use / 专业工作 | OpenAI 文章页副标题 |

## 一、长上下文：标称容量与有效容量是两件事

厂商标称的是窗口大小，用户需要的是"在窗口里还能答对"。这一层差距在 2025 年就被量化过：NoLiMa 论文测了 13 个声称支持至少 128K 上下文的模型，在 32K 长度上，有 11 个模型的成绩跌破自身短上下文强基线的 50%。原因不神秘——经典 needle-in-a-haystack 测试里问题和材料存在字面重合，模型可以靠字符串匹配蒙对；一旦去除字面重合、要求推断潜在关联，长上下文的成绩就塌。

2026 年的评测把这条经验产品化了。Artificial Analysis 的 AA-LCR-25 每题喂约 100k 输入 token 做长上下文召回，并且把重复次数从 10 次提到 25 次，才把 95% 置信区间从 ±4.7 压到 ±2.6 分——换句话说，在此之前，很多"长上下文能力差异"落在噪声里。同一份 2026-11-09 的报告还给出另一组数字：在其评测的 103 个 serverless endpoint 中，24 个的准确率统计显著低于自托管参考实现，原因包括量化权重、自定义 kernel、输出长度截断。你买到的 1M 窗口，可能只是别人的 200K 行为。

## 二、多模态：从"能看"到"能查"

视觉能力的评测在 2026 年明显偏向"读取密度高的专业材料"，而不是识别猫狗。Anthropic 在 Fable 5.2 页面自报的 Chartography（无工具视觉图表识别）一组可作参照：Fable 5.2 73.4%、Gemini 4 Carbon 72.5%、GPT-6 Astra 71.0%、Opus 5.5 64.4%、Fable 5.1 44.8%。两个观察值得记住：一是同代内小版本迭代（Fable 5.1 → 5.2）可以把这项指标拉高 28.6 个百分点，说明它仍在快速爬坡而非饱和；二是跨厂商的差距比想象中窄，头部四家落在 64%–74% 的区间里。

另一侧是模态的"下沉"。DeepSeek 在 2026 年的官方规格里把 Vision 放在便宜档 `deepseek-flash`，旗舰 `deepseek-v4-pro` 不支持视觉；Google 则反向操作，Gemini Omni 1.2 Flash 把文本、图像、视频输入统一到 $1.50/1M token 一档。两种策略指向同一个现实：多模态不再是旗舰的专属能力，而是产品定位函数——要推理纯度就砍模态，要吞吐就走便宜档。

生成侧也在同步演进。OpenAI 的 Images 2.6 于 2026-11-04 发布，官方副标题强调"更快生成、对多轮编辑更强的控制"；Google 的 Nano Banana 系列已迭代到 2.1。图像生成与图像理解正在变成两条独立产品线，共用同一个多模态底座。

## 三、Agentic：评测对象从"模型"变成"系统"

computer use 的分数已经进入厂商互相对照的主表。Anthropic 公布的 OSWorld 2.1（离线集，部分）结果：Fable 5.2 89.3%、Opus 5.5 87.2%、Gemini 4 Carbon 81.5%、GPT-6 Astra 79.5%、Fable 5.1 83.5%。终端与研究型任务同样被纳入：Terminal-Bench-Science 0.1 上 Fable 5.2 74.6%（Mythos 5.2 75.3%）、GPT-6 Astra 68.1%、Gemini 4 Carbon 64.8%、Opus 5.5 58.7%。软件工程侧则是 FrontierSWE v2：Fable 5.2 69.9%、GPT-6 Astra 65.5%、Opus 5.5 62.3%。

这里要提醒一句口径问题：这些数字全部来自 Anthropic 自己的发布页，属于"厂商自报"，且注明"最高可用 effort"。第三方复现的版本往往不同——Artificial Analysis 的 Coding Agent Index 测的是"模型 + 厂商自家 agent"的组合，Muse Ember 在 Muse Code 里拿 46 分，Claude Haiku 5.5 在 Claude Code 里 37 分，GPT-6 Luna 在 Codex 里 31 分。分数差里混着模型能力、脚手架质量、以及工具链成熟度，不能直接读成模型排名。

## 四、结构变化：上下文、模态、执行三者开始互相定价

把上面三节拼起来，会看到一个不太直观的结论：长上下文、多模态、agentic 三件事在成本结构上是耦合的。图像和视频输入按 token 折算后体积放大，长上下文让每次调用的输入 token 线性增长，而 agent 的多轮特性又让"重复前缀"成为成本主项。于是定价机制开始承担能力开关的角色：

- 缓存命中价成为真正的上下文价格。DeepSeek 的 cache hit 与 cache miss 相差 50 倍（$0.003 对 $0.15，谷时）；Claude Opus 5.5 的缓存读 $0.20/1M 对标准输入 $4.00/1M，是 20 倍。
- 峰谷与批处理让"什么时候跑"变成一等公民。DeepSeek 谷时价格是峰时的一半（峰时定义为 UTC 01:00–04:00 与 06:00–10:00 的工作日）；Google 的 batch 档与标准档是 50% 关系。
- 长 prompt 不再线性定价。Claude Haiku 5.5 在 prompt 超过 100K token 后，输入从 $0.10 跳到 $0.50、输出从 $0.50 跳到 $2.50。


## 五、一个可复算的成本算例

把"上下文长度 + 缓存"换算成钱，比背单价更有用。假设一个 agent 任务每次调用输入 200K token、连续 10 轮，输入总量 2M token，输出忽略不计，分别按三家的官方价（谷时、标准档）计算：

表 3：2M 输入 token 的成本随缓存命中率变化（USD，2026-11-15 官方价）

| 缓存命中率 | DeepSeek `deepseek-flash` | Gemini 4 Carbon | Claude Opus 5.5 |
|---|---|---|---|
| 0% | 0.30 | 4.00 | 8.00 |
| 80% | 0.065 | 0.96 | 1.92 |
| 95% | 0.021 | 0.39 | 0.78 |

算式很朴素：`成本 = 命中token × 缓存价 + 未命中token × 输入价`。以 Opus 5.5、80% 命中为例，1.6M × $0.20/1M + 0.4M × $4.00/1M = $1.92。三种命中率之间是 4–10 倍的差距，比换模型（同代内通常 2–4 倍）更大。这也解释了为什么 2026 年的 agent 框架普遍把"稳定前缀"当成一等设计目标：缓存命中率不是一个优化项，而是成本模型的主变量。

## 对开发者的实际影响

1. **仍然要做分块与检索。** 1M 窗口解决的是"少一次检索"，不是"不需要检索"。NoLiMa 那组数字说明，在 32K 之后有效召回就可能腰斩；把整本手册塞进去，代价是 token 账单和准确率同时上升。
2. **把"缓存友好"写进架构。** 系统提示、工具定义、代码库索引放在前缀，人类输入放在后缀；否则每次调用都按 miss 计价，成本差 20–50 倍。
3. **模态按任务选档，不要按旗舰选。** 纯文本推理任务用不支持视觉的旗舰反而更便宜；读图表、读扫描件、跑 computer use 才值得付多模态溢价。
4. **端到端测自己的任务分布。** 厂商自报的 OSWorld、Terminal-Bench 分数是能力上限，你的任务分布和工具链决定下限；AA 的 endpoint 数据说明，同一模型换供应商就可能掉几个点。


## 六、音频与实时：被低估的第三模态

多模态讨论常止于图像，但 2026 年的官方模型族已经把音频单列：Google 的模型导航里，Gemini Audio、Veo（带音频视频）、Lyria（音乐）各自成条；DeepSeek 则把 TTS 与 ASR 作为独立计费 API（TTS $0.45/万字符、限时 5 折 $0.225；ASR $0.116/小时、限时 $0.058）。这类 API 的计价单位不是 token，而是字符数或音频小时，意味着"多模态成本"根本不在同一张表里——把语音接进 agent 时，预算模型必须按模态分别建，而不是折算成 token。


## 七、接口形态：能力要靠协议落地

能不能把上面这些能力接进系统，最后取决于协议面。DeepSeek 的做法很务实：同一套模型同时提供 OpenAI 兼容格式与 Anthropic 兼容格式（`https://api.deepseek.com` 与 `https://api.deepseek.com/anthropic`），并提供 Responses API、Tool Calls、JSON 输出、Chat Prefix Completion（Beta）与 FIM（Beta，仅非思考模式）。对迁移方来说，这等于把"换模型"的成本压到改一个 base URL；对 agent 框架来说，前缀补全和 FIM 这类偏"编辑"的接口，在多轮代码修改里比纯对话接口更省 token。

并发配额是另一条容易被忽略的硬约束：DeepSeek 官方标注 `deepseek-flash` 2500 并发、`deepseek-v4-pro` 500 并发。多模态与长上下文任务往往单请求耗时更长，同样的并发数下吞吐会明显下降，这也是很多团队最终把视觉任务放到便宜档、把推理任务放到旗舰档的现实原因。

## 常见坑与局限

- **标称上下文 ≠ 有效上下文。** 规格页写 1M，供应商可能截断输出长度、限制单次请求字节数，或在长上下文下切换量化路径。
- **视觉能力分档被忽略。** DeepSeek 同代两档一有一无，选错型号会在生产环境直接报错，而不是降级。
- **thinking token 计入输出。** Google 在 Gemini 定价页明确写"output price (including thinking tokens)"；按旧口径估算预算会系统性偏低。
- **只支持隐式缓存的模型**（如 Gemini 4 Carbon）无法手动创建缓存，缓存收益取决于请求模式，不能像显式缓存那样预先布局。
- **厂商自报表的横向可比性有限。** 同一张表里的对手分数由发布方代跑，effort 档位、safeguards 开关、工具使用都可能不一致；Mythos 5.2 与 Fable 5.2 是同一底层模型，仅因安全防护不同就在 CyScenarioBench 上出现 76.3% 对不适用防护的差异。

## 来源

- DeepSeek《Models & Pricing》：https://api-docs.deepseek.com/quick_start/pricing （访问 2026-11-15）
- Anthropic《Claude Mythos》产品页（含 Fable 5.2 与 Mythos 5.2 基准表）：https://www.anthropic.com/claude/mythos （访问 2026-11-15）
- Anthropic《Claude Opus》产品页：https://www.anthropic.com/claude/opus （访问 2026-11-15）
- Anthropic Newsroom（发布日期）：https://www.anthropic.com/news （访问 2026-11-15）
- Google《Gemini Developer API Pricing》：https://ai.google.dev/gemini-api/docs/pricing （访问 2026-11-15）
- Google DeepMind 模型族页：https://deepmind.google/models/gemini/ （访问 2026-11-15）
- OpenAI News RSS 与《Introducing GPT-6.1 Sol》文章页：https://openai.com/news/rss.xml 、https://openai.com/index/introducing-gpt-6-1-sol/ （访问 2026-11-15）
- OpenAI《Introducing Images 2.6》：https://openai.com/index/introducing-images-2-6/ （访问 2026-11-15）
- Artificial Analysis《Endpoint Accuracy Index v1.1》：https://artificialanalysis.ai/articles/endpoint-accuracy-index-v1-1 （访问 2026-11-15）
- Artificial Analysis 首页方法论与变更日志：https://artificialanalysis.ai/ （访问 2026-11-15）
- NoLiMa 论文（ICML 2025）：https://arxiv.org/abs/2502.05167 （访问 2026-11-15）

## 不确定项

- **GPT-6.1 Sol 的上下文窗口与输入模态未取到官方数值。** OpenAI 文章页正文由客户端渲染，web_fetch 只拿到标题与副标题；官方定价页（openai.com/api/pricing、platform.openai.com/docs/pricing）分别返回 JS 空壳与 HTTP 403。
- **Claude Opus 5.5 的 1M 上下文来自产品页描述句**，页面正文未按型号逐条列出窗口规格，且该页开头仍残留旧版本（Opus 4.7）文案，规格可能滞后。
- **Gemini 4 Carbon 的"1M"是长解码生成上限，不等于上下文窗口。** Google 的 models 文档页为纯客户端渲染，未能取到 context window 字段。
- **Anthropic 基准表中的"Elo"一行（1785/1669/1800/1569/1611）未标注来源**，本表未将其当作 LMArena 分数使用。
- **LMArena/Arena 的实时榜单未能读取**（lmarena.ai 返回 HTTP 403，存档页超时），因此本文不含该榜分数。
