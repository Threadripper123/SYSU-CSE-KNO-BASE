# FM-05 评测与排行榜：MMLU-Pro、GPQA Diamond、AIME、SWE-bench Verified、ARC-AGI-2、LiveBench、LMArena、Artificial Analysis

> 主题库：前沿大模型（2023–2026）　｜　源文件：（纯文本条目）　｜　7,457 字符　｜　26 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

## 一句话结论

2026 年评测的核心矛盾不是"缺少指标"，而是**指标口径互不可比**：老的单选题基准（MMLU、GPQA）已经接近饱和并被训练数据覆盖，新的基准转向效率约束与代理任务（ARC-AGI-2、Terminal-Bench、AA-LCR-25），而厂商自报表与第三方复现之间的差异，已经大到足以改变排名结论。读榜的第一动作应该是读脚注，而不是读分数。

## 关键变化表

表 1：八个主流基准/榜单的档案（按公开论文与官方页整理）

| 基准 | 起点 | 规模与结构 | 计分方式 | 主要失效模式 |
|---|---|---|---|---|
| MMLU-Pro | 2024（NeurIPS D&B Spotlight） | 选项从 4 扩到 10，剔除简单题 | 准确率 | 仍是知识型，可被训练数据覆盖 |
| GPQA Diamond | 2023-11 | 448 道生物/物理/化学选择题 | 准确率 | 高分端饱和；专家 65%、非专家 34% |
| AIME | 年度竞赛 | 每年两套、每套 15 题，答案 000–999 | 准确率（常取多次采样） | 题量小、年度难度波动，方差大 |
| SWE-bench Verified | 2024-08-13 | 500 题人工确认子集（母集 2,294 题 / 12 仓库） | 解决率 % | 环境与脚手架影响大，判定依赖测试 |
| ARC-AGI-2 | 2025 | 1,000 训练 / 120 公开 / 120 半私有 / 120 私有 | 准确率 + 成本 | 半私有集可能经 API 暴露 |
| LiveBench | 2024 | 持续更新的新题，覆盖数学/编码/推理/语言/指令/数据分析 | 客观 ground truth | 更新节奏与覆盖面 |
| LMArena（现 Arena） | 2023-04-24 | 匿名两两对战、用户投票 | Elo/BT 类排名 | 风格偏好、版本差异争议 |
| Artificial Analysis | 2023 起 | 复合指数 + Cost per Task | 指数与置信区间 | 私有基准不可外部复现 |

表 2：同一个模型，三种口径可以给出相反结论（2026-11，均为 max/最高 effort）

| 模型 | FrontierCode 2.0 | Terminal-Bench 4.0 | Coding Agent Index（端到端） |
|---|---|---|---|
| Muse Ember (max) | 27.2% | 40.9% | 46（在 Muse Code 中） |
| Claude Haiku 5.5 (max) | 34.9% | 32.8% | 37（在 Claude Code 中） |
| GPT-6 Luna (max) | 未取到 | 未取到 | 31（在 Codex 中） |

这张表是本文最重要的表：Muse Ember 在 FrontierCode 2.0 上落后 7.7 个百分点，却在 Terminal-Bench 4.0 上领先 8.1 个百分点，端到端的 agent 指数还高出 9 分。同一批模型、同一周的数据，结论取决于你问哪个问题。

## 一、老基准为什么失效

MMLU-Pro 的论文给出了最直白的量化：把选项从四个扩到十个、剔除琐碎题之后，准确率相对 MMLU 下降 16%–33%；更关键的是稳定性——在 24 种提示风格下，模型分数的波动从 MMLU 的 4%–5% 降到约 2%。换句话说，MMLU 那代基准里，有相当一部分"能力差异"其实是提示敏感度。

GPQA 的设计初衷是"Google-proof"：448 道由生物、物理、化学领域专家撰写的高难题，博士级专家本人只有 65% 正确率（剔除事后确认有误的题后为 74%），而高水平非专家在可以自由上网、平均每题花 30 分钟以上的条件下只有 34%。论文发布时最强的 GPT-4 基线是 39%。这组数字说明 GPQA 的分辨力集中在"人类专家以下、模型之上"的区间；一旦头部模型逼近或超过专家水平，它就从能力测试退化为噪声测试。

## 二、SWE-bench：从 2,294 到 500

SWE-bench 是目前最成功的"真实任务"基准：2,294 个来自 12 个主流 Python 仓库的真实 GitHub issue 与对应 PR，模型要改代码让测试通过，常常需要跨函数、跨文件协调。它 2024 年发表于 ICLR，随后暴露出两个问题——部分题目描述不清或参考答案本身有误，以及不同 scaffold 之间不可比。2024-08-13，SWE-bench 官方与 OpenAI Preparedness 合作发布 **SWE-bench Verified**：由真实软件工程师确认可解的 500 题子集。今天绝大多数"SWE-bench 分数"默认指这个 500 题子集，而不是母集。

这也是一个典型的口径陷阱：模型分数里混着 scaffold（mini-SWE-agent、各家自研 agent）、重试次数、以及是否允许联网。比较两个模型的 SWE-bench 分数前，先确认它们用的是同一套 harness。

## 三、ARC-AGI-2：把"效率"写进基准

ARC-AGI-2 的结构值得完整记住：训练集 1,000 题（公开、未校准，用于训练系统）；公开评测集 120 题、半私有评测集 120 题、私有评测集 120 题，后三者都经过校准，要求每个任务至少被两名人类在 pass@2 内解决。校准过程包括 400 多名参与者的受控人类测试与 2025 年初圣地亚哥的现场研究，官方称三个评测集之间的分数可比（预期差异小于 1 个百分点）。

它明确设定了"效率"这条第二轴：官方页写着"对数线性扩展不足以攻破 ARC-AGI-2"，需要新的测试时适应算法或新系统，并给出 85% 准确率的人类参照目标。ARC Prize 2026 已经结束，官方公告结果将在 2026-12-04 公布——也就是说，本文写作时 2026 赛季的名次尚未发布，任何"2026 ARC Prize 冠军"的说法都不可信。

## 四、抗污染与置信区间：LiveBench 与 AA

LiveBench 的定位是"抗污染、限污染"：题目来自近期数学竞赛、arXiv 论文、新闻与数据集，按月更新；答案用客观 ground truth 自动判定，避开 LLM 裁判与人工众包的偏差；同时包含 Big-Bench Hard、AMPS、IFEval 等旧基准的"更难、抗污染"版本。论文评测范围从 0.5B 到 405B 的开源模型与多家闭源模型。

Artificial Analysis 走的是另一条路：不追求单一榜单，而是把多个评测合成指数，并把不确定性显式写出来。其 Endpoint Accuracy Index v1.1（2026-11-09）由三项等权组成——BFCL-500（工具调用，500 题 × 3 次重复）、HLE-250（科学推理，250 题 × 10 次重复）、AA-LCR-25（长上下文召回，每题约 100k 输入 token，25 题 × 25 次重复）。把 AA-LCR-25 的重复次数从 10 提到 25 之后，95% 置信区间从 ±4.7 收窄到 ±2.6，综合指数区间从 ±5.5 收窄到 ±4.3。同一份报告的核心结论是：在其评测的 103 个 serverless endpoint 中，有 24 个的准确率统计显著低于自托管参考实现。

## 五、LMArena：人类偏好榜的位置与争议

LMArena（2026-01-28 起改名 Arena）是这个领域最特殊的存在：它不测能力，测偏好。用户对两个匿名模型的回答投票，胜率汇总成 Elo 类排名。它 2023-04-24 上线，源自 UC Berkeley 的项目，2024 年 6 月加入图像、2026 年 1 月加入视频；2025 年 4 月独立成公司，5 月完成 1 亿美元种子轮、估值 6 亿美元，2026-01-06 完成 1.5 亿美元 A 轮、投后约 17 亿美元，2026-10-08 又完成 2 亿美元 B 轮、估值 31 亿美元。

争议同样真实。2025 年 4 月，Llama 4 Maverick 在榜上击败 GPT-4o 与 Gemini 2.0 Flash，但榜上版本与公开发布版本存在差异，LMArena 随后更新了政策；公开资料也记录了研究者对该榜方法论的批评。它适合回答"用户更喜欢哪个回答"，不适合回答"哪个模型更能修 bug"。

## 六、怎么读一张榜

表 3：读榜前的六项检查

| 检查项 | 要问的问题 | 典型陷阱 |
|---|---|---|
| 口径 | 是否带工具、是否带 scaffold、effort 档位 | Xhigh 与 Medium 混排 |
| 安全开关 | safeguards 是否开启 | 同一底层模型因防护不同而分数不同 |
| 不确定性 | 是否给出置信区间、重复次数 | 单次采样当结论 |
| 成本 | 分数对应的每任务成本 | 高 5 分贵 10 倍 |
| 时效 | 数据测量窗口 | 旧快照当现状 |
| 可复现 | 权重/服务是否公开 | 私有基准无法外部验证 |

一个具体的例子能说明"安全开关"这项：Anthropic 明确写 Fable 5.2 与 Mythos 5.2 是同一个底层模型，差别只在防护策略；在 CyScenarioBench 上，Mythos 5.2 是 76.3%，而 Opus 5.5 在"无防护"条件下是 67.6%。同一个分数背后可能是三个不同的系统配置。


## 七、复现为什么贵：一笔算得出来的账

第三方复现稀缺，不只是意愿问题，是算术问题。以 Artificial Analysis 的 Endpoint Accuracy Index 为例，AA-LCR-25 是 25 题 × 25 次重复、每题约 100k 输入 token，单个模型单轮就是 62.5M 输入 token；按 Gemini 4 Carbon 的 $2.00/1M 计算，仅这一项约 125 美元，换成 Claude Opus 5.5 的 $4.00/1M 则约 250 美元。再加 BFCL-500（500 题 × 3 次）与 HLE-250（250 题 × 10 次），以及为压置信区间而必须的多次重复，单模型审计的账单就进入数百美元量级；乘上 103 个 endpoint，才得到那份报告里的"24 个显著低于参考"。

这笔账解释了三个现象：第一，公开榜单的更新频率受成本约束，不可能像模型发布一样快；第二，小团队只能做抽样复现，抽样就意味着结论有误差带；第三，厂商自报之所以长期存在，是因为它是唯一"免费"的全量数据——代价是口径由发布方定义。

## 对研究者与开发者的实际影响

1. **为你的任务选基准，而不是为榜单选基准。** 做代码修复看 SWE-bench Verified 与 FrontierSWE v2；做端到端代理看 Terminal-Bench 与 Coding Agent Index；做长文档看 AA-LCR-25 与 NoLiMa 式的去字面匹配测试。
2. **把"每任务成本"纳入主指标。** Artificial Analysis 的 Cost per Task 显示，同为 36 分，Muse Ember (max) 是 $0.14、Claude Haiku 5.5 (max) 是 $0.41，接近 3 倍；DeepSeek V4.1 Flash 1017 (max) 是 33 分、$0.60。按 token 单价排序会得到完全不同的结论。
3. **复现优先于相信。** 厂商自报表里的对手分数通常由发布方代跑；第三方榜单又有自己的私有基准。最可靠的做法是拿 50–200 条自己的样本做小规模对照，先看排序是否稳定，再看绝对分数。
4. **注意饱和速度。** GPQA、MMLU-Pro 这类基准的头部区间已经很挤，1–2 个百分点的差距往往落在置信区间内；把预算花在长尾任务上比抠榜首更有价值。

## 常见坑与局限

- **把 AIME 单次分数当能力。** 每套只有 15 题，年度难度波动大，头部模型常见 90% 以上的成绩，一题之差就是 6.7 个百分点。
- **忽略半私有集的暴露风险。** ARC-AGI-2 官方说明半私有集"可能通过 API 暴露给有限第三方"，只有私有集才完全未暴露；引用 ARC 分数时要看用的是哪一套。
- **跨 harness 比较 SWE-bench。** 不同 agent 框架、不同重试策略的分数不可直接比。
- **把 LMArena 排名当能力排名。** 它测的是偏好，且存在版本一致性与风格偏差的公开争议。
- **把"最新"当"最好"。** 榜单快照有时间戳，模型也可能在发布后被静默更新（例如推理栈调整），旧分数会失真。

## 来源

- MMLU-Pro 论文：https://arxiv.org/abs/2406.01574 （访问 2026-11-15）
- GPQA 论文：https://arxiv.org/abs/2311.12022 （访问 2026-11-15）
- SWE-bench 论文：https://arxiv.org/abs/2310.06770 （访问 2026-11-15）
- SWE-bench 官方仓库 README（含 Verified 500 题与 2024-08-13 发布记录）：https://raw.githubusercontent.com/SWE-bench/SWE-bench/main/README.md （访问 2026-11-15）
- SWE-bench 官方榜单页：https://www.swebench.com/ （访问 2026-11-15）
- ARC Prize《ARC-AGI-2》基准页（数据集结构、校准方法、85% 目标）：https://arcprize.org/arc-agi/2/ （访问 2026-11-15）
- ARC Prize 榜单页：https://arcprize.org/leaderboard （访问 2026-11-15）
- LiveBench 论文：https://arxiv.org/abs/2406.19314 （访问 2026-11-15）
- NoLiMa 论文：https://arxiv.org/abs/2502.05167 （访问 2026-11-15）
- Artificial Analysis《Endpoint Accuracy Index v1.1》：https://artificialanalysis.ai/articles/endpoint-accuracy-index-v1-1 （访问 2026-11-15）
- Artificial Analysis《Meta's Muse Ember is the cheapest model scoring 36 on the Intelligence Index》：https://artificialanalysis.ai/articles/meta-s-muse-ember-is-the-cheapest-model-scoring-36-on-the-intelligence-index （访问 2026-11-15）
- Anthropic《Claude Mythos》产品页基准表：https://www.anthropic.com/claude/mythos （访问 2026-11-15）
- Wikipedia《LMArena》条目（沿革、融资、争议；第三方来源）：https://en.wikipedia.org/wiki/LMArena （访问 2026-11-15）

## 不确定项

- **LMArena/Arena 的实时榜单未读取到**：lmarena.ai 直接返回 HTTP 403，web.archive.org 存档请求超时。本文关于该榜的叙述来自 Wikipedia 条目，属第三方整理，未与官方榜核对。
- **SWE-bench 当前榜单的具体分数未取到**：官方页数据由前端 JS 加载，抓到的 2.39 MB HTML 中不含模型名列；因此本文只引用 Verified 的规模与发布日期，不引用最新排名。
- **LiveBench 的实时榜单未取到**：livebench.ai 返回 889 字节的空壳页；本文只引用论文层面的设计与覆盖范围。
- **AIME 的具体年份题面与官方规则未在本次抓取中核对**，"每年两套、每套 15 题、答案 000–999"为通行常识，非本次一手抓取。
- **GPT-6.1 Sol / GPT-6 Astra 的 GPQA、AIME 等单项分数未取到**：OpenAI 文章页正文客户端渲染，定价页返回 403；Anthropic 表中出现的 GPT-6 Astra 分数由 Anthropic 代跑，属厂商自报。
- **"ARC Prize 2026 冠军"当前不存在可引用结果**：官方明确结果于 2026-12-04 公布。
