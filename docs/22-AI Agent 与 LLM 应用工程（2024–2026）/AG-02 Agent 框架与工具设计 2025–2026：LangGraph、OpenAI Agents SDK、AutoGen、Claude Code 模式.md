# AG-02 Agent 框架与工具设计 2025–2026：LangGraph、OpenAI Agents SDK、AutoGen、Claude Code 模式

> 主题库：AI Agent 与 LLM 应用工程（2024–2026）　｜　源文件：（纯文本条目）　｜　7,029 字符　｜　33 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

## 一、结论先行

到 2026 年，Agent 框架之争已经不是"谁的原语更优雅"，而是三层能力是否齐备：**编排运行时**（状态、持久化、中断恢复）、**harness**（计划、子代理、文件系统、上下文压缩、权限）、**评测与可观测**（trace、数据集、回归）。三家主流玩家的答案高度相似，差别在抽象层级和交付形态。

三个判断：

1. **LangGraph 定位为"低层运行时"**，明确不规定 prompt 与架构；LangChain 提供代理抽象，Deep Agents 提供 harness，LangSmith 负责观测、评测与部署——产品线是分层的，选型时别拿 LangGraph 跟"开箱即用 Agent"比。
2. **OpenAI Agents SDK 用极简原语 + 沙箱补齐工程面**：Agents、Handoffs、Guardrails、Sessions、Tracing 五个概念，加上 Sandbox agents 与 Skills/Compaction 能力，仍停留在 0.x（0.24.0），API 稳定性要打折。
3. **AutoGen 已归档、不再维护**，微软把路线交给 Microsoft Agent Framework（1.26.0，Production/Stable）。这是 2026 年最容易踩的坑：大量中文教程仍在教 AutoGen。

Claude Code 则代表另一种范式：不对外承诺"框架"，而是把 agentic loop 打磨成一个可扩展的 harness——CLAUDE.md、Skills、Subagents、Hooks、MCP、Plugins 各自解决一类问题，版本已到 2.1.325。

## 二、四者对照

| 维度 | LangGraph | OpenAI Agents SDK | AutoGen / MAF | Claude Code |
| - | - | - | - | - |
| 最新版本（2026-11-15 查询） | PyPI 1.2.22；npm 1.7.4 | PyPI 0.24.0 | AutoGen 0.7.5（归档）；MAF 1.26.0 | 2.1.325（2026-11-13） |
| 定位 | 低层编排运行时 | 轻量多代理 SDK | 研究型多代理 → 企业 SDK | 编码 agent harness |
| 核心原语 | StateGraph、节点/边、checkpointer、store | Agent、Handoff、Guardrail、Session、Trace | AgentChat、Core 事件运行时、Workflow | 工具循环 + 扩展面 |
| 状态与恢复 | 检查点按 thread 存，可 time travel | Sessions（SQLite/Redis/Mongo/Dapr/加密） | AgentSession / checkpoint | 会话、/rewind、prompt cache |
| 多代理 | 子图、Command/Send 路由 | handoffs、agents-as-tools | GroupChat / Magentic / 图工作流 | subagents、dynamic workflows |
| 沙箱 | 无内置，自行接 | Sandbox agents（文件/Shell/内存） | 依赖宿主 | 沙箱 Bash、worktree 隔离 |

> 版本号来自 PyPI/npm 注册表查询；AutoGen 与 MAF 是两代产品，不能按版本号直接比较。

## 三、LangGraph：把"状态机"做成一等公民

LangGraph 的最小模型是：共享状态（state）+ 节点（node）+ 边（edge），编译成图后执行。官方示例用 `StateGraph(MessagesState)`，从 `START` 连到节点再到 `END`；它同时允许确定性的手写节点与模型驱动的节点混在同一个图里。

真正决定生产可用性的是持久化。LangGraph 把它拆成两个互补系统：

- **Checkpointer**：按 thread 保存图状态快照，用于会话连续、human-in-the-loop、time travel 与故障恢复。`thread_id` 就是游标——复用即续跑，换新即空状态。
- **Store**：保存图状态之外的键值数据，跨 thread 共享，用于用户偏好、长期事实。

中断机制是 `interrupt()`：节点里调用它，传入任意可 JSON 序列化的值，框架落盘并无限期等待；恢复时用 `Command` 重新调用图，传入值成为 `interrupt()` 的返回值。它比"在节点前后设静态断点"灵活，因为可以写在任意位置、按业务条件触发。

这套设计的代价是**心智负担**：状态 schema、reducer、边条件、检查点后端都要自己设计。LangGraph 也因此适合"流程有明确阶段、需要审计与恢复"的场景（审批、长任务、多步工具链），而不是"问一句答一句"的聊天。

图 API 还有三个容易低估的开关。**reducer** 决定同一状态字段被多个节点并发写时怎么合并，写错会静默丢数据；**条件边**把路由逻辑从节点里抽出来，便于测试；**Send** 允许一次 fan-out 出多个并行分支，再由 reducer 汇总。子图则让一个团队整体作为一个节点被父图调度，这是把多代理系统拆成可复用模块的关键。

落地时还要选检查点后端：官方示例用内存版（InMemorySaver、InMemoryStore）把流程跑通，生产必须换持久化实现；若用 LangSmith Agent Server，文档明确持久化由服务端托管，不必自己配 checkpointer 与 store。另一个常见误区是把 store 当数据库——它存的是应用自定义的键值数据，适合偏好与事实，不适合堆放大量原文，原文仍应留在自己的检索系统里。拿内存版上生产，重启即丢全部会话，这是最常见的低级事故。

## 四、OpenAI Agents SDK：少原语，多工程

官方把自己的设计原则写得很直白：功能要够用，但原语要少到能快速学会。五个原语里，值得展开的是两个：

- **Handoffs**：把控制权交给另一个 agent，带意图分类、参数传递与返回值；与之并列的是 agents-as-tools（把子 agent 当工具调用）。前者适合"换人接手"，后者适合"调用专家"。
- **Guardrails**：与 agent 执行并行跑的校验器，输入侧快速失败、输出侧做合规检查。注意它校验的是"这一轮"，不是替代权限系统。

2026 年的新增能力集中在"把 agent 放进真实环境"：**Sandbox agents** 在隔离工作区里跑，支持 manifest 定义文件、选择 sandbox client、可恢复会话；能力面拆成 Filesystem、Shell、Memory、Skills、Compaction 五类。Sessions 后端也从内存扩到 SQLAlchemy、Redis、MongoDB、Dapr 与加密存储。

一个容易忽略的细节是上下文分层：`RunContextWrapper` 里的本地上下文（用户 ID、logger、数据获取器）**不会发给模型**，只供工具与回调使用；模型看到的是另一套消息。把"业务依赖"和"提示词内容"混在一起，是很多团队上下文爆炸的根源。

Tracing 是这套 SDK 的隐藏卖点：每个 agent 步骤、工具调用、handoff 都生成 span，可发到 OpenAI 的 trace 面板，也可接第三方 processor。它的价值不在看日志，而在于**把一次运行变成可回归的样本**——线上失败可以直接拿 trace 重放，改完 prompt 再跑同一批。没有这层，多代理系统的调试会退化成猜。

## 五、AutoGen 的退场与 MAF 的接棒

AutoGen 官方文档页顶部现在是一条明确警告：**项目已归档，不再由微软维护**，不提供缺陷修复、安全修复、新功能与支持；并建议新用户直接使用 Microsoft Agent Framework，老用户走迁移指南。PyPI 上 `autogen-agentchat` 停在 0.7.5（2025-09-30），也印证了这一点。

MAF 把能力分成四块：Agents（单 agent，接 Foundry、Azure OpenAI、OpenAI、Anthropic、Ollama 等）、**Harness Agent**（面向长任务的"带电池"agent：计划与待办、上下文压缩、文件访问与记忆、don't-ask-again 工具审批、可观测）、Workflows（函数式与图式工作流）、Integrations（模型、工具、上下文提供者、中间件、评测、UI）。Go 版本处于 public preview，要求 Go 1.26，且声明式 agent、CodeAct、函数式工作流尚未实现。

对存量系统，迁移的真实成本不在 API 名称，而在三处语义差异：GroupChat 的发言调度、human-in-the-loop 的请求-响应模型、检查点与恢复的粒度。微软的迁移指南按"单 agent → 多 agent → HITL → checkpoint → 可观测"逐段对照，值得先读再动手。

## 六、Claude Code：harness 而非框架

Claude Code 官方把 agentic loop 拆成三段：**收集上下文、采取行动、验证结果**，三段反复交织，人可以随时插话。它把"模型 + 工具 + 上下文管理"这一层称为 **agentic harness**——这个词在 2026 年比"agent 框架"更准确。

内置工具分五类：文件操作、搜索、执行、Web、代码智能（需 LSP 插件）。真正的设计工作在扩展面：

| 扩展 | 作用 | 何时用 |
| - | - | - |
| CLAUDE.md | 每次会话自动加载的持久上下文 | 项目约定、"总是这样做"的规则 |
| Skills | Markdown 写的知识/流程，可按需加载或 `/命令` 调用 | 可复用流程、参考文档、部署清单 |
| Subagents | 独立上下文里跑自己的循环，只回摘要 | 上下文隔离、并行调研、专职工人 |
| Hooks | 生命周期事件触发脚本/HTTP/MCP/子 agent | 每次编辑后跑 lint、提交前校验 |
| MCP | 接外部服务与工具 | 数据库、Slack、浏览器控制 |
| Plugins | 把 skills/hooks/subagents/MCP 打包分发 | 跨仓库复用、团队分发 |

两个工程细节值得借鉴：一是**上下文成本被显式管理**——prompt caching 自动做，`/compact` 有明确代价，CLAUDE.md 的修改不会中途生效；二是**隔离用 worktree 与沙箱**，而不是靠提示词约束。这说明"权限与隔离"应当由 harness 提供，而不是指望模型自觉。

上下文管理可以归纳为三个杠杆：**缓存**（prompt caching，降低重复前缀的成本与延迟）、**压缩**（`/compact` 或自动摘要，把长历史折成要点，代价是丢细节）、**隔离**（subagent 在自己的上下文里跑，只回摘要）。三者不是替代关系：缓存省钱、压缩省窗口、隔离省注意力。多数团队只做压缩，结果关键约束被摘要掉，表现成 agent 忘了要求。

## 七、工具设计的六条硬规则

无论用哪个框架，工具层最容易出问题。以下六条是上述文档共同指向的结论：

1. **错误要可被模型读懂**。MCP 2025-11-25 明确建议把输入校验错误作为"工具执行错误"返回，而不是协议错误，目的就是让模型能自我纠正。堆栈跟踪对模型无用，字段名 + 期望类型才有用。
2. **schema 要窄**。参数越少、枚举越明确，模型选错的概率越低；自由文本参数应有长度与格式约束。
3. **返回值要自解释**。无状态协议下客户端可能重放请求，返回结果不能依赖"上一次调用"的隐含状态。
4. **权限与隔离放在 harness**。Claude Code 用沙箱、worktree、审批规则解决；OpenAI 用 Sandbox agents 与 guardrails 解决。提示词里的"不要删库"不是安全机制。
5. **渐进披露优于全量注入**。工具数量大时，先给目录再按需展开（MCP 路线图里的 progressive discovery、Claude Code 的 Skills 都是这个思路）。
6. **能用代码执行就别堆工具调用**。把多步工具编排压成一段可执行代码，往返次数与 token 都更省；代价是要有沙箱与审计。

补充一句范围说明：本文只覆盖这四条主线。CrewAI、MetaGPT、Semantic Kernel 等同样活跃，但它们在 2025–2026 的演进更多是补工程面而非改变设计范式，不另开篇幅。

## 八、选型建议

- 流程有明确阶段、要求可恢复与可审计 → LangGraph（配 LangSmith）。
- 团队要最快跑通多代理、且接受 0.x 的 API 波动 → OpenAI Agents SDK。
- 已在微软栈、需要企业治理与 .NET/Go → MAF；不要新上 AutoGen。
- 要做"编码/运维类长任务 agent"，或想抄一套成熟的 harness 设计 → 研究 Claude Code 的扩展面与上下文管理。

## 来源

- https://docs.langchain.com/oss/python/langgraph/overview.md、/persistence.md、/interrupts.md（访问 2026-11-15）
- https://openai.github.io/openai-agents-python/、/context/、/release/（访问 2026-11-15）
- https://microsoft.github.io/autogen/stable/（访问 2026-11-15，归档声明）
- https://learn.microsoft.com/en-us/agent-framework/overview/agent-framework-overview（访问 2026-11-15）
- https://learn.microsoft.com/en-us/agent-framework/migration-guide/from-autogen/（访问 2026-11-15）
- https://code.claude.com/docs/en/how-claude-code-works.md、/features-overview.md、/changelog.md（访问 2026-11-15）
- https://pypi.org/pypi/langgraph/json、/openai-agents/json、/autogen-agentchat/json、/agent-framework/json（访问 2026-11-15）
- https://registry.npmjs.org/@langchain%2Flanggraph/latest（访问 2026-11-15）

## 不确定项

- OpenAI Agents SDK 文档的 changelog 页只列到 0.22.0，而 PyPI 最新为 0.24.0，中间两个版本的细节未核对。
- LangGraph 的 Python 1.2.22 与 npm 1.7.4 是两条独立版本线，本文不据此判断功能差异。
- MAF 的 GA 时间、许可与具体支持矩阵未在本次核对的页面中给出；"Production/Stable" 仅来自 PyPI 分类器。
- AutoGen 归档的具体日期未在文档页标注，只确认"截至 2026-11 已归档"。
- Claude Code 的版本号 2.1.325 是文档 changelog 首条，发布日期 2026-11-13；其功能清单会快速变化，本文只取结构性事实。
