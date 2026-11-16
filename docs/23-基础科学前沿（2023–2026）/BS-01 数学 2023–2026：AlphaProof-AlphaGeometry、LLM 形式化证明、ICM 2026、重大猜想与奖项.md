# BS-01 数学 2023–2026：AlphaProof/AlphaGeometry、LLM 形式化证明、ICM 2026、重大猜想与奖项

> 主题库：基础科学前沿（2023–2026）　｜　源文件：（纯文本条目）　｜　7,628 字符　｜　27 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# BS-01 数学 2023–2026：AlphaProof/AlphaGeometry、LLM 形式化证明、ICM 2026、重大猜想与奖项

一句话结论：这三年真正的分水岭不是“AI 会做奥数题”，而是**AI 的证明开始同时接受形式化验证器与竞赛官方评分体系的双重检验**；与此同时，ICM 2026 把菲尔兹奖授予包括三维 Kakeya 问题解决者 Hong Wang 在内的四人，说明硬核数学仍按自己的时间尺度推进。

## 关键变化一览

| 时间 | 事件 | 证据等级 | 关键数字 |
|---|---|---|---|
| 2024-01-17 | AlphaGeometry 论文发表于《Nature》 | 已发表（Nature 625, 476–482） | 30 道最新奥赛几何题解出 25 道 |
| 2024-07-25 | AlphaProof + AlphaGeometry 2 参加 IMO 2024 | 官方评分（Gowers、Myers） | 4/6 题，28/42 分，银牌线 29 分 |
| 2025-02-05 | AlphaGeometry 2 技术报告 | 预印本（arXiv:2502.03544） | 2000–2024 几何题解出率 84%，前代 54% |
| 2025-02-24 | Wang–Zahl 三维 Kakeya 证明 | 预印本（arXiv:2502.17655） | 任意 R³ Kakeya 集的 Minkowski 与 Hausdorff 维数均为 3 |
| 2025-07-21 | Gemini Deep Think 参加 IMO 2025 | 官方认证（IMO 协调员评分） | 5/6 题，35/42 分，金牌 |
| 2025-11-12 | AlphaProof 方法论文发表于《Nature》 | 已发表 | 形式化 + 强化学习路线获同行评议 |
| 2026-07-23–30 | ICM 2026（费城） | 官方 | 4 枚菲尔兹奖、Abacus/Chern/Gauss 各 1 |
| 2026-07-10–21 | IMO 2026（上海） | 官方赛程 | 未检索到 AI 系统获官方评分的记录 |

## 一、从“几何专用”到“通用形式化”

## 〇、2023 年的起点：形式化数学的“基础设施期”

把时间拨回 2023 年，LLM 与定理证明的结合还处在两个平行世界：一边是 Lean 4 与 Mathlib 社区，靠人工把本科到研究生的数学搬进证明助手，进展稳定但缓慢；另一边是 GPT-4 级别的通用模型，能写像模像样的证明草稿，却经常在关键一步胡编。真正缺的不是“生成文本的能力”，而是**可验证性**——一个证明要么通过编译器，要么不通过，没有中间地带。2023 年形成的这套“生成—编译—反馈”闭环，是后来 AlphaProof、DeepSeek-Prover、Goedel-Prover 一系列工作的共同底座。`MiniF2F`（244 题）、`PutnamBench`（658 题）这类基准也是在这个阶段固化成事实标准的。

AlphaGeometry 的意义在于绕开了“人类证明难以转写成机器可验证格式”这一数据瓶颈：它用语言模型在数百万级合成定理与证明上从零训练，再引导符号演绎引擎 DDAR 搜索。2024 年 1 月的《Nature》论文给出的硬指标是：在 30 道最新奥赛几何题上解出 25 道，接近人类金牌选手在同类题上的表现。这不是“通用数学能力”，而是把一个子领域的数据问题工程化解决了。

AlphaProof 走的是另一条路：把题目先人工转写成 Lean，再用类似 AlphaZero 的强化学习在形式化证明空间里搜索。IMO 2024 上，这套组合解出 4/6 题：AlphaProof 拿下三道代数/数论题，其中包括全场只有 5 名选手做出的 P6；AlphaGeometry 2 在收到形式化题目后 **19 秒**内解出几何题 P4；两道组合题未解。总分 28/42，比当年金牌线 29 分低 1 分，属于“银牌顶端”。代价是：题目需要专家转写，且计算耗时两到三天。

2025 年 2 月的 AlphaGeometry 2 技术报告把几何能力推到“超过平均金牌选手”：题目语言覆盖率从 66% 提升到 88%，2000–2024 年全部 IMO 几何题解出率 84%（前代 54%）。方法上的关键不是更大模型，而是 Gemini 架构、知识共享搜索（让多棵搜索树互通）以及符号引擎的工程优化。

## 二、IMO 2025：自然语言端到端与“官方认证”的分水岭

2025 年 7 月，Google DeepMind 的 Gemini Deep Think 在 IMO 2025 上解出 5/6 题、拿到 35/42 分，达到金牌线。比分数更重要的是三点：

1. **自然语言端到端**：不再需要专家把题目转写成 Lean 或几何语言，模型直接读题、写证明；
2. **官方认证**：DeepMind 是“首届”由 IMO 协调员按评学生同样的标准评分并认证的 AI 参赛方之一；
3. **时间约束**：在 4.5 小时赛制内完成，而不是像 2024 年那样跑两三天。

同一时期 OpenAI 也宣称其模型达到金牌水平，但围绕“是否经 IMO 官方评分”出现过争议。这件事给后续所有“AI 做奥数”的新闻定了一个判别标准：**自报分数、第三方复评、IMO 官方认证是三档不同证据**，不能混写。

| 年份 | 系统 | 分数 | 题数 | 评分方式 | 备注 |
|---|---|---|---|---|---|
| 2024 | AlphaProof + AlphaGeometry 2 | 28/42 | 4/6 | 数学家按 IMO 规则评分 | 需人工形式化，算力 2–3 天 |
| 2025 | Gemini Deep Think | 35/42 | 5/6 | IMO 协调员官方认证 | 自然语言，4.5 小时 |
| 2026 | 未检索到公开参赛系统 | — | — | — | 截至 2026-11-15，IMO 官网与 DeepMind 博客均未见 2026 年 AI 成绩 |

## 三、2026：开源复现、Agent 化与“苦涩教训”

2025 年下半年到 2026 年，形式化证明的竞争从“谁能做出一个 demo”转向“谁能把整条流水线开源、可复现、可规模化”。

- **DeepSeek-Prover-V2**（2025-04，arXiv:2504.21801）：671B 模型，用递归子目标分解 + 强化学习，`MiniF2F-test` 通过率 **88.9%**，`PutnamBench` 658 题中解出 49 题，并发布 325 题的 `ProverBench`。
- **Goedel-Prover-V2**（2025-08，arXiv:2508.03613）：8B 模型在 `MiniF2F` 上 `pass@32` 达 84.6%，32B 标准模式 88.1%、自纠错模式 90.4%，`PutnamBench` 在 `pass@184` 下解出 86 题。
- **Kimina-Prover Preview**（2025-04，arXiv:2504.11354）：基于 Qwen2.5-72B，`MiniF2F` `pass@8192` 达 80.7%，强调“形式化推理模式”与采样效率。
- **2026 年的新方向**：`NanoProof`（2026-10，arXiv:2610.11605）主打“训练数据、抽取工具、训练管线、权重全部开放”的可复现 Lean 4 证明器；`h1`（2026-10，arXiv:2610.35962）则以“苦涩教训”为题，主张减少人类设计的规划/分解工作流，把性能压到规模化强化学习上。这两篇都是预印本，结论应视为作者自报。

一个容易被忽略的数字是**评估口径**：`pass@8192` 与 `pass@1` 差距巨大，`MiniF2F-test` 只有 244 题，且公开训练集与测试集存在污染风险。看到“90% 形式化证明通过率”时，先问三个问题：pass@k 的 k 是多少、测试集是否公开、证明是否由独立 Lean 编译验证。

| 系统 | 基准 | 成绩 | 口径 |
|---|---|---|---|
| DeepSeek-Prover-V2-671B | `MiniF2F-test` | 88.9% | 通过率 |
| Goedel-Prover-V2-32B | `MiniF2F` | 88.1%（标准）/ 90.4%（自纠错） | `pass@32` |
| Goedel-Prover-V2-8B | `MiniF2F` | 84.6% | `pass@32` |
| Kimina-Prover Preview | `MiniF2F` | 80.7% | `pass@8192` |
| AlphaGeometry 2 | IMO 2000–2024 几何 | 84% | 全量几何题 |

## 四、ICM 2026：四个菲尔兹奖与三维 Kakeya

ICM 2026 于 2026 年 7 月 23–30 日在费城宾夕法尼亚会议中心举行。官网公布的 2026 年菲尔兹奖得主为：

- **Yu Deng（邓煜，芝加哥大学）**：偏微分方程，包括从硬球动力学严格推导稀薄气体 Boltzmann 方程、从非线性色散系统推导波动动理学方程，以及非线性 Schrödinger 动力学的概率方法。
- **John Pardon（石溪大学）**：辛几何，包括虚拟基本闭链的新方法、Liouville 流形的 Fukaya 范畴与全纯曲线计数，以及三维流形群作用与纽结理论。
- **Jacob Tsimerman（多伦多大学）**：o-minimality 技术在算术与复代数几何中的扩展，包括 Griffiths 猜想（周期映射像的代数性）的证明。
- **Hong Wang（王虹，纽约大学/IHES）**：调和分析与几何测度论，包括平面波方程局部光滑猜想的多尺度与 decoupling 方法，以及在 Fourier restriction、Falconer 距离集、平面 Furstenberg 集与**三维 Kakeya 问题**上的重大进展。

同届还颁出：Abacus Medal 给 Shayan Oveis Gharan（华盛顿大学，算法与多项式几何、TSP 近似、MCMC 与谱独立性）；Chern Medal 给 Graeme Segal（拓扑、数学物理、表示论）；Gauss Prize 给 Yurii Nesterov（优化理论与算法）。IMO 官网 2026-07-24 的新闻标题是“前 IMO 选手跻身 2026 年菲尔兹奖得主”，把竞赛数学与前沿研究连成了一条线。

三维 Kakeya 猜想是这一周期最硬的结果之一。Wang 与 Joshua Zahl 在 2025-02-24 的预印本（arXiv:2502.17655）中证明：R³ 中任何 Kakeya 集的 Minkowski 维数与 Hausdorff 维数都等于 3。Kakeya 猜想问的是“让单位线段在每一点都转过所有方向，所需集合能有多小”，二维情形 1971 年已解决，三维悬置五十余年。该工作目前是预印本，同行评议状态需以期刊版本为准；ICM 的颁奖词把它列为 Wang 获奖理由之一，属于官方对其重要性的背书，但不等于该论文已完成同行评议。

另一个值得记录的进展是几何 Langlands 猜想：2024 年起，Gaitsgory 等作者以系列预印本形式给出证明框架，篇幅达数百页。本文未逐篇核对全文，只把它列为“预印本层面的重大进展”，不当作已定论。

## 五、对不同读者的实际影响

**对竞赛与人才识别**：IMO 2026 官网专门发新闻指出，多位 2026 年菲尔兹奖得主曾是 IMO 选手。这条信号比任何 AI 成绩都更值得注意：竞赛训练与前沿研究之间确实存在通道，但通道的尽头不是“会解题”，而是“能提出问题并坚持数年”。AI 把解题这一端的成本打下来之后，竞赛的选拔价值会更多落在长期潜力而非短期分数上。

**对数学研究者**：Lean 4 + Mathlib 正从“个人爱好”变成协作基础设施。AI 目前最现实的价值不是“替你想定理”，而是证明搜索、引理检索、反例生成、把论文证明形式化到可验证。真正稀缺的能力仍是“提出好问题”和“判断什么值得证”。

**对学生与教师**：IMO 题不再是“AI 不可能”的护城河。2024 年还需要专家转写、跑几天；2025 年已经能在 4.5 小时内用自然语言拿金牌。教学重点会更快转向证明写作、问题提出与对 AI 输出的审查。

**对工程与产品**：可复现的开源证明器（NanoProof、Goedel-Prover 系列）降低了“verifier-in-the-loop”的门槛；但把它接进生产系统时，必须把 Lean 编译成功当作唯一可信信号，而不是模型自报“证明完成”。

## 六、常见坑与局限

1. **把“IMO 金牌”等同于“做数学研究”**：IMO 题有已知答案、限时、范围窄；研究数学需要选题、构造理论、忍受长期不确定。
2. **混淆证据等级**：自报分数、第三方复评、IMO 官方认证、期刊同行评议是四件事。本文表格已逐项标注。
3. **忽略形式化税**：自然语言到 Lean 的转写仍可能出错，2024 年的成绩依赖人工转写；2025 年虽端到端，但证明正确性仍要看最终判定。
4. **基准污染**：`MiniF2F` 题量小、流传广，高通过率不等于泛化。
5. **2026 年 AI 参赛情况不明**：截至本文检索，IMO 2026 官网只发布人类成绩，DeepMind 也未发布 2026 年 IMO 博文；任何“2026 年 AI 再夺金”的说法目前都缺乏一手来源。

## 来源

- ICM 2026 官网（含 2026 年各奖项获奖词）：https://www.icm2026.org/ （访问 2026-11-15）
- IMO 官网（IMO 2026 上海、IMO 2027 布达佩斯、2026-07-24 新闻）：https://www.imo-official.org/ （访问 2026-11-15）
- DeepMind：AI achieves silver-medal standard solving IMO problems（2024-07-25，2025-11-12 更新）：https://deepmind.google/discover/blog/ai-solves-imo-problems-at-silver-medal-level/
- DeepMind：Gemini Deep Think 获 IMO 2025 金牌（2025-07-21）：https://deepmind.google/discover/blog/advanced-version-of-gemini-with-deep-think-officially-achieves-gold-medal-standard-at-the-international-mathematical-olympiad/
- Trinh et al., Solving olympiad geometry without human demonstrations, Nature 625, 476–482 (2024)：https://www.nature.com/articles/s41586-023-06747-5
- AlphaGeometry2 技术报告：https://arxiv.org/abs/2502.03544 （全文经 https://ar5iv.labs.arxiv.org/html/2502.03544 阅读）
- Wang & Zahl, Volume estimates for unions of convex sets, and the Kakeya set conjecture in three dimensions：https://arxiv.org/abs/2502.17655
- DeepSeek-Prover-V2：https://arxiv.org/abs/2504.21801
- Goedel-Prover：https://arxiv.org/abs/2502.07640 ；Goedel-Prover-V2：https://arxiv.org/abs/2508.03613
- Kimina-Prover Preview：https://arxiv.org/abs/2504.11354
- NanoProof：https://arxiv.org/abs/2610.11605 ；h1：https://arxiv.org/abs/2610.35962

## 不确定项

- **IMO 2026 是否有 AI 系统参赛/获官方评分**：未检索到一手来源。IMO 官网只列人类赛程与成绩；DeepMind 博客未见 2026 年 IMO 文章。本文按“无公开记录”处理，不推断为“没有参赛”。
- **AlphaProof 的《Nature》论文标题与 DOI**：DeepMind 博客注明 2025-11-12 发表方法论文，但本文未打开该论文页面核对标题与页码。
- **三维 Kakeya 论文的同行评议状态**：arXiv:2502.17655 为 2025-02-24 预印本，是否已在期刊正式发表未核实。
- **几何 Langlands 系列预印本的作者名单、篇数与评审状态**：未逐篇核对，正文只作“预印本层面的进展”处理。
- **Abel Prize 2024/2025**：abelprize.no 在本环境不可达，本文未写入，以免使用未核实信息。
