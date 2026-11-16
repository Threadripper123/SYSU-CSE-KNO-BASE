# AG-04 Computer-use 与浏览器 Agent：Anthropic Computer Use、OpenAI Operator、browser-use、基准

> 主题库：AI Agent 与 LLM 应用工程（2024–2026）　｜　源文件：（纯文本条目）　｜　7,516 字符　｜　26 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

# AG-04 Computer-use 与浏览器 Agent：Anthropic Computer Use、OpenAI Operator、browser-use、基准

## 一句话结论

Computer-use 用两年时间走完了「实验室能力 → 产品入口」的路，但评测分数与生产可用性之间的鸿沟至今没有闭合：2024 年 OSWorld 上人类完成率 72.36%、最强模型只有 12.24%；到 2026 年，浏览器代操作已经是 Claude、ChatGPT 这类产品的常规入口，而工程上真正稳的方案仍是「浏览器语义层动作 + 像素级兜底」的混合体，纯截图点击只在没有 DOM 可用的死角里才划算。

## 关键变化表

| 时间 | 事件 | 对工程实践的含义 |
| --- | --- | --- |
| 2023-07 | WebArena 论文（arXiv 2307.13854），4 个自建站点域、812 个任务 | 第一个可复现的「真网站」agent 基准；代价是站点是仿真的 |
| 2024-04 | OSWorld（arXiv 2404.07972），369 个真实桌面任务，跨 Ubuntu/Windows/macOS | 把评测从「网页」推到「整台电脑」，含文件 I/O 与跨应用流程 |
| 2024-10-22 | Anthropic 发布 computer use public beta，Claude 3.5 Sonnet 为首发模型 | 动作空间定型为「截图 + 坐标点击 + 键盘」；同日 API、Bedrock、Vertex 可用 |
| 2025-01-23 | OpenAI 推出 Operator（research preview，Pro 用户、仅美国），由 CUA 驱动 | 把「代你上网」做成消费级产品；但产品形态的寿命短于底层能力 |
| 2025-07-28 | OSWorld-Verified 发布，约 10 人团队处理 300+ 条社区反馈 | 基准开始「维护」而非「一次性发布」；AWS 并行把评测压进 1 小时 |
| 2025 下半年 | browser-use 从 0.x 的 DOM 抽取转向 1.0 的「模型写代码 + CDP」 | 动作空间从固定动词表变成图灵完备程序，表达力与不可控性同时上升 |
| 2026 | 浏览器操作成为模型定位的一部分（Claude Haiku 5.5 明确点名 browser use） | 选型问题从「要不要用 computer-use」变成「哪一层动作接口」 |

## 一、能力起点：把「看屏幕、动鼠标」做成 API

2024 年 10 月 22 日，Anthropic 在发布升级版 Claude 3.5 Sonnet 的同时，把 computer use 作为 public beta 开放。官方对它的定性非常克制：这是第一个提供该能力的前沿模型，但仍属实验性，官方原话是「at times cumbersome and error-prone」，开放的目的是尽早拿到开发者反馈。当天即可在 Anthropic API、Amazon Bedrock 和 Google Cloud Vertex AI 上调用，早期探索者包括 Asana、Canva、Cognition、DoorDash、Replit 和 The Browser Company，做的任务动辄几十步、上百步。Replit 的用法很有代表性：用它在 Replit Agent 生成应用的过程中做 UI 走查，判断应用「长出来」之后能不能用。

这个设计的工程后果，比它的演示效果重要得多。动作空间被压成三件事：拿一张截图、在坐标上点击或拖拽、敲键盘。这带来三个无法绕开的成本：

1. **每步一张图**。上下文里堆的是图像 token，步数一多，成本和延迟都是线性上涨，且历史截图很快吃满窗口。
2. **坐标系必须对齐**。模型看到的是缩放后的图，点击要落回真实屏幕坐标，DPI 缩放、窗口边框、多显示器任何一处不一致，点偏就是必然。
3. **动作不可回滚**。点错菜单、误提交表单，没有事务语义，只能靠「先截图再判断」补。

## 二、OpenAI 的路线：把「代你上网」做成产品

2025 年 1 月 23 日，OpenAI 上线 Operator，定位是 research preview，只对美国的 Pro 用户开放，底层是 Computer-Using Agent（CUA）——一个用「看屏幕 + 操作虚拟浏览器」来完成任务的模型。它把 computer-use 从 API 能力包装成了消费产品：用户给一句自然语言，agent 在自己的浏览器里跑。

但这条产品线本身的寿命值得记一笔。到 2026 年再打开 Operator 的发布页，页面顶部已经改成「这篇发布介绍的是 Operator；当前体验请看 Workspace agents / ChatGPT Work」。也就是说，Operator 作为独立产品已经退场，被并入更大的 agent 产品面。这对做工程选型的人是个直接教训：**把业务绑在某个具体 agent 产品上，迁移成本远高于绑在工具层或协议层**。同样是「让模型操作浏览器」，绑在 MCP 工具、Playwright 或 CDP 上的代码，换个模型、换个厂商都还能跑；绑在某个产品的会话 API 上，产品一改版就要重写。

## 三、开源侧：browser-use 的两次范式转移

browser-use 是这三年里最能说明「动作接口怎么设计」的开源项目：Python 与 TypeScript 双实现、MIT 许可。它的 1.0 版本做了一次彻底的范式切换，README 写得很直白——现在的形态是「模型、一个 REPL、CDP」，模型直接写 Python 单元格，在持久 REPL 里调用 browser-harness 的辅助函数和原生 Chrome DevTools Protocol，运行在 Pydantic AI 之上。

| 维度 | 0.x 形态（DOM 抽取 + 动作表） | 1.0 形态（代码 + CDP） |
| --- | --- | --- |
| 模型输出 | 结构化动作（点击元素编号、输入文本） | 一段可执行的 Python 代码 |
| 表达力 | 受限于预定义动词表 | 图灵完备，可写循环、条件、断言、批量操作 |
| 失败模式 | 选错元素、点错序号 | 写错程序、选错选择器、死循环 |
| 可回归性 | 较强，动作可录制重放 | 较弱，每轮代码都可能不同 |
| 调试方式 | 看动作序列 | 看 REPL 输出与异常栈 |

这次切换的代价常被低估：动作表简单但封闭，代码强大但把「agent 出错」变成了「程序出错」，而程序出错是可以静默的——循环跑完了，结果却是错的。工程上要补的是断言与验收，而不是更多的重试。截至 2026 年，README 建议精确 pin 版本（`browser-use==1.0.5`），要求 Python 3.11 以上；云端浏览器报价 0.06 美元/浏览器小时，含 stealth、验证码处理与住宅代理。这类「开源库 + 托管浏览器」的组合，实际上把最难的部分（指纹、代理、验证码）挪到了基础设施层。

## 四、基准：三个数字，和它们的失效边界

| 基准 | 规模 | 当年人类/最强模型 | 关键局限 |
| --- | --- | --- | --- |
| WebArena | 812 个任务、4 个域（电商、社交论坛、协作开发、CMS） | 人类 78.24%，最好 GPT-4 agent 14.41% | 站点自建、可复现，但与真实互联网的对抗性脱节 |
| OSWorld | 369 个真实电脑任务，Ubuntu/Windows/macOS | 人类 72.36%，最好模型 12.24% | 真机 VM，真实但慢、脆、维护成本高 |
| τ-bench | 零售/航空等域的对话任务，LLM 扮演用户 | gpt-4o 成功率低于 50%，retail 域 pass^8 低于 25% | 考的是「按策略办事」，不是「操作界面」 |

WebArena 的 14.41% 与 78.24% 之间那条沟，三年里被填了一部分，但填的方式值得警惕：很多提升来自更强的视觉定位与更长的可回滚轨迹，而不是「理解了网页」。OSWorld 的 12.24% 更能说明问题——论文把失败归因于 GUI grounding 与操作知识，这两项恰恰是纯像素方案最难补的。2025 年 7 月 28 日的 OSWorld-Verified 则暴露了另一面：一个基准跑了 15 个月后，需要专门团队花约两个月、处理 300 多条社区反馈才能「重新可信」，说明这类基准的噪声本身就有几十个百分点的量级。

实际读数是：**基准分数适合做能力下限的筛子，不适合做上线判据**。同一个模型在基准里 40%、在你自己那套后台里可能是 5%，差别来自登录态、内部组件库、异步加载和风控。

## 五、2026 年的产品化现状

到 2026 年，浏览器代操作已经不是「要不要做」的问题，而是产品导航里的一个格子。Anthropic 一侧，`claude.com` 的产品面里出现了 Claude in Chrome、Claude Cowork、Managed Agents 等入口；2026 年 10 月 7 日发布的 Claude Haiku 5.5 在官方描述里直接写明面向「summarization、subagents 和 browser use」这类高并发场景——把 browser use 与 subagent 并列，等于承认它是一条独立负载。官方参考实现 anthropic-quickstarts 的 computer-use-demo 也已把默认模型切到 `claude-opus-4-8` 并用上 adaptive thinking，同时保留 Opus 4.5、Sonnet 4.5、Haiku 4.5 等旧模型的 extended thinking 路径。

更值得注意的是那份 README 里列的「生产化清单」，它基本就是这几年踩坑的总结：显式工具定义、图像尺寸与裁剪、prompt caching、服务端 compaction、批量工具调用、沙箱 shell、轨迹记录。这七项里只有一项与模型有关，其余全是工程。

## 六、三条路线的取舍

| 路线 | 代表 | 适用 | 不适用 |
| --- | --- | --- | --- |
| 像素级（截图 + 坐标） | Anthropic computer use 原始形态 | 无 DOM 的桌面应用、canvas、远程桌面 | 高频重复任务、需要精确断言 |
| 语义级（DOM/可访问性树） | Playwright、浏览器 MCP 工具 | 自家或结构稳定的站点、需要稳定重放 | 强反爬、复杂 iframe、动态混淆 |
| 代码即动作 | browser-use 1.0、CDP 直连 | 需要循环/条件/批量的复杂流程 | 要求强可回归、审计严格的场景 |

实践里最稳的是分层：能用语义选择器就用语义选择器，语义层拿不到时才降级到截图点击，并把「降级」本身记进日志——它是你系统里最脆弱的一环，出问题要能一眼看到。

## 七、对团队的实际影响

- **选型先选动作层，再选模型**。动作层决定可观测性和可回归性，模型只决定成功率。把动作层做成可替换的接口，换模型的成本就从「重写」降到「换参数」。
- **成本按步算，不按次算**。一次任务几十步、每步一张图，是这类 agent 成本结构的基本盘；prompt caching、图像裁剪、历史截图淘汰是必做项，不是优化项。
- **安全不能靠提示词**。Anthropic 自己在 README 里写明：网页或图片里的指令可能覆盖用户指令。这意味着必须做域名 allowlist、敏感操作人工确认、以及把 agent 放进最小权限的容器或虚拟机。
- **验收要写断言**。无论走哪条路线，最后一步都应该是「读回状态并断言」，而不是「模型说完成了」。

## 八、安全边界：为什么这类 agent 天生难防注入

普通聊天应用里，注入的载体是用户输入；computer-use agent 里，注入的载体是**它正在看的那块屏幕**。模型为了完成任务必须读取页面文本、图片、PDF、邮件正文，而这些内容和系统指令走的是同一个上下文通道，没有天然的信任边界。Anthropic 在参考实现的 README 里写得很直接：在某些情况下 Claude 会执行它在内容里找到的指令，即使这与用户指令冲突；网页上的说明或图片里的文字都可能覆盖用户指令或让模型出错。

官方给出的四条缓解措施，本质上都是「缩小爆炸半径」而不是「消除注入」：

1. 用专用虚拟机或容器，给最小权限，避免直接的系统攻击或误操作；
2. 不让模型接触敏感数据（例如账号登录信息），避免信息被窃取；
3. 互联网访问限制在域名 allowlist 内，降低恶意内容的暴露面；
4. 对可能产生真实后果的动作、以及需要明确同意的动作（接受 cookie、执行金融交易、同意服务条款），要求人工确认。

这四条对应到工程上，就是三个必须落地的机制：**出站域名白名单**、**高危动作的人机确认闸门**、**凭据与浏览上下文隔离**。缺任何一条，注入就从「模型答错」升级为「以你的身份做了一件事」。这也是为什么很多团队最后把 computer-use 限制在「读多写少」的任务上——读错可以纠正，写错不可逆。

## 九、常见坑

1. **DPI 与缩放错位**：容器里 1x、宿主机 1.5x，点击永远差十几像素。统一在容器内截图、在容器内点击。
2. **等待用固定 sleep**：页面慢 200ms 就失败。改成等待具体条件（元素可见、网络空闲）。
3. **iframe / Shadow DOM / canvas**：语义层直接失效，很多人在这里误判为「模型太笨」，其实是可见性都没解决。
4. **验证码与登录态**：托管浏览器能解决一部分，自建必须自己解决会话持久化。
5. **重试不幂等**：提交订单类的动作重试两次就是两单。重试前必须先读状态。
6. **把基准当 SLA**：见第四节，基准与你的后台不是同一个分布。

## 来源

- Anthropic，《Introducing computer use, a new Claude 3.5 Sonnet, and Claude 3.5 Haiku》，2024-10-22：https://www.anthropic.com/news/3-5-models-and-computer-use（访问 2026-11-15）
- Anthropic 官方参考实现 computer-use-demo README：https://github.com/anthropics/anthropic-quickstarts/tree/main/computer-use-demo（访问 2026-11-15）
- OpenAI，《Introducing Operator》，2025-01-23：https://openai.com/index/introducing-operator/（访问 2026-11-15）
- OpenAI，《Computer-Using Agent》，2025-01-23：https://openai.com/index/computer-using-agent/（访问 2026-11-15）
- browser-use 仓库 README：https://github.com/browser-use/browser-use（访问 2026-11-15）
- WebArena 论文：https://arxiv.org/abs/2307.13854（访问 2026-11-15）；项目页 https://webarena.dev/
- OSWorld 论文：https://arxiv.org/abs/2404.07972（访问 2026-11-15）
- OSWorld-Verified 发布说明，2025-07-28：https://xlang.ai/blog/osworld-verified（访问 2026-11-15）
- Anthropic Newsroom：https://www.anthropic.com/news（访问 2026-11-15，用于 2026 年产品与模型时间线）

## 不确定项

- OpenAI 在 Operator 之后推出的统一 agent 产品（ChatGPT agent）的确切上线日期，以及底层 CUA 模型在 API 中的正式型号名，本次未能在 OpenAI 一手页确认（页面为 JS 渲染，抓取只得到标题与导航）。文中因此只写「已由 Workspace agents / ChatGPT Work 取代」，不写具体日期。
- Google 的 Gemini Computer Use 相关文档（`ai.google.dev` 路径）本次抓取失败，故未纳入对比；这不代表该能力不存在。
- WebArena 的 14.41% 与 OSWorld 的 12.24% 均为论文发表时的最好成绩，不是 2026 年的最新 SOTA；当前分数需查各自 leaderboard，本文未取。
- browser-use 的版本迭代极快，`1.0.5` 只是 README 在 2026-11-15 时点的推荐 pin 版本，后续可能变化。
- Claude in Chrome 等产品能力的具体开放范围与地区限制未核实，文中仅作为产品面存在性提及。
