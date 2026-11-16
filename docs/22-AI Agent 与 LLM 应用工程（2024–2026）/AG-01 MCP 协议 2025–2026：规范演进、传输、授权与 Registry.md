# AG-01 MCP 协议 2025–2026：规范演进、传输、授权与 Registry

> 主题库：AI Agent 与 LLM 应用工程（2024–2026）　｜　源文件：（纯文本条目）　｜　8,335 字符　｜　31 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

## 一、结论先行

MCP 在两年里完成了三次身份转变：从 Anthropic 2024 年 11 月放出的"工具接入约定"，变成有正式治理、有版本号、有扩展机制的开放协议，再在 **2026-07-28 版规范**里被改造成**无状态、HTTP 原生**的协议。这次改动不是加特性，而是删机制：删掉协议级会话、初始化握手、SSE 断线续传，把"服务端主动问客户端"改成"客户端重放请求"。对正在做 Agent 平台的团队，这是必须重新评估的一次升级。

四个判断：

1. **传输层收敛为两条**：本地用 stdio，远程用 Streamable HTTP。2026-07-28 之后，远程 MCP 服务器就是一个普通 HTTP 工作负载，可以放在无粘性会话的负载均衡后面。
2. **授权走 OAuth 2.1 全套**，但客户端注册机制已换轨：Dynamic Client Registration（DCR）被标记为弃用，推荐 Client ID Metadata Documents（CIMD）。
3. **Registry 仍是 preview**（官方明示可能破坏性变更或数据重置），定位是"元数据中转站"，主要供聚合器消费，不直接面向 host 应用，也不支持私有服务器。
4. **协议已归入 Linux Foundation 体系**（"Model Context Protocol a Series of LF Projects, LLC"），代码与规范 Apache-2.0，文档 CC BY 4.0。

## 二、版本演进：五个规范快照

官方文档按日期固化每个修订版，路径形如 `/specification/2026-07-28/`。可核对的关键节点如下：

| 修订版 | 状态与要点 | 对实现者的影响 |
| - | - | - |
| 2024-11-05 | 首个规范版本，定义 JSON-RPC 消息与 stdio、HTTP+SSE 两条传输 | 现有大量教程仍以它为模板，容易写出过时代码 |
| 2025-03-26 | 引入 Streamable HTTP，替代 HTTP+SSE | 远程服务器开始按"单端点 + POST"实现 |
| 2025-06-18 | 授权与工具输出模型继续细化（本次未逐条复核该版 changelog） | 多数 SDK 的兼容基线 |
| 2025-11-25 | 九项主要变更：OIDC Discovery、icons、增量 scope 同意、CIMD、tasks 实验性支持等 | 客户端注册与异步任务开始有官方答案 |
| 2026-07-28 | 无状态化、删会话、MRTR、扩展机制正式化 | 需要改架构，不只是改字段 |

> 2025-06-18 的具体条目我没有逐条核对，表中只标注其"兼容基线"角色；引用该版细节前建议回查官方 changelog。

## 三、传输：把会话从协议里拿掉

### 3.1 两条传输的分工

- **stdio**：客户端把服务器作为子进程拉起，按行读写 JSON-RPC。适合本地文件、数据库、IDE 插件类能力；凭据从环境变量取，规范明确 **SHOULD NOT** 走 OAuth 流程。
- **Streamable HTTP**：服务器**必须**提供单一 MCP 端点（如 `https://example.com/mcp`）并支持 POST。客户端每次 POST 必须带 `Accept: application/json, text/event-stream`，响应可以是单个 JSON，也可以是 SSE 流。所有请求必须带 `Mcp-Method` 头，`tools/call`、`resources/read`、`prompts/get` 还必须带 `Mcp-Name`（非 ASCII 值用 Base64 哨兵编码）。通知类消息若被接受，服务器返回 `202 Accepted` 且无 body。

安全基线也写进了传输层：服务器**必须**校验 `Origin`，非法来源返回 `403`；本地运行**应**只绑 `127.0.0.1`。

### 3.2 2026-07-28 的破坏性变更

| 维度 | 2025-11-25 及以前 | 2026-07-28 |
| - | - | - |
| 会话 | `Mcp-Session-Id` 头 + 连接级会话 | 删除协议级会话；跨调用状态由服务端签发 handle，当普通工具参数传 |
| 握手 | `initialize` / `notifications/initialized` | 删除；每请求在 `_meta` 带 `protocolVersion` 与 `clientCapabilities` |
| 版本协商 | 初始化时一次定版本 | 每请求协商；另有必实现的 `server/discover` 供预选版本或 stdio 兼容探测 |
| 订阅 | HTTP GET 端点 + `resources/subscribe` | `subscriptions/listen`：一条长活 POST 响应流，按类型订阅并打 `subscriptionId` |
| 服务端发起请求 | `roots/list`、`sampling/createMessage`、`elicitation/create` | MRTR：返回 `resultType: "input_required"` 与 `inputRequests`，客户端重试原请求并带 `inputResponses` |
| 结果形状 | 无统一标记 | 所有结果必带 `resultType`（`complete` / `input_required`），旧服务器缺省按 `complete` 处理 |
| 断流恢复 | `Last-Event-ID` 重投 | 删除；流断即请求丢失，客户端须用新 request ID 重发 |
| 日志级别 | `logging/setLevel` | 每请求 `_meta.logLevel` |
| 心跳 | `ping` | 删除 |

次要但影响工程的点：列表与读取结果必须带 `ttlMs` 与 `cacheScope`（`public`/`private`），配合 `tools/list` 的确定性排序提高 prompt cache 命中率；`_meta` 支持 OpenTelemetry 的 `traceparent`/`tracestate`/`baggage`；错误码分区，`-32020`~`-32099` 归规范所有（如 `HeaderMismatch` `-32020`、`UnsupportedProtocolVersion` `-32022`）。

### 3.3 工程后果

- 无状态意味着**水平扩容不再需要粘性会话**，MCP 服务器可像普通 API 一样部署；代价是每请求都要携带版本与能力，头部体积上升。
- 断流不重投，要求工具调用**幂等**或自带去重键；把长任务交给 Tasks 扩展比"挂着一条流"更稳。
- MRTR 把"服务端反问"变成显式状态机：客户端要能保存原请求参数、在拿到用户输入后原样重放。
- `server/discover` 应作为健康检查/兼容探测的第一跳。

### 3.4 为什么必须无状态

会话是横在 MCP 与云原生之间的一道墙：只要协议要求 `Mcp-Session-Id` 与连接级初始化，服务器就得做粘性路由、会话存储与超时回收，扩缩容时还要迁移或放弃会话。把这些交给协议之外（服务端自己签发 handle、客户端自己保存上下文），MCP 服务器才真正变成“普通 HTTP 工作负载”：可以放在任意副本后面，可以按请求计费，也可以被 API 网关按标准手段缓存和限流。

代价同样明确：每次请求都多带版本、能力与身份字段；服务端不能再“记得”上一次调用的上下文，必须把状态编码进 handle 或参数里。这对工具设计提出新要求——**参数要能自描述，返回值要能独立解释**，否则客户端无法在断流后重放。


## 四、授权：OAuth 2.1，但注册机制换轨

角色划分很清晰：**MCP 服务器 = OAuth 2.1 资源服务器，MCP 客户端 = OAuth 客户端**，授权服务器（AS）可以独立部署。规范基于 OAuth 2.1（draft-ietf-oauth-v2-1-13），并引用 RFC 6750、8414、7591、8707、9728、9207，以及 CIMD 草案与 OIDC Discovery 1.0。

| 要求 | 规范措辞 | 实践含义 |
| - | - | - |
| AS 能力 | **MUST** 实现 OAuth 2.1 | 不支持 OAuth 2.1 的旧 AS 需要升级或加代理 |
| 资源元数据 | MCP 服务器 **MUST** 实现 RFC 9728 PRM | `/.well-known/oauth-protected-resource` 成为发现入口 |
| AS 发现 | AS **MUST** 提供 RFC 8414 或 OIDC Discovery 之一；客户端 **MUST** 两者都支持 | 客户端不能只实现一种发现 |
| 客户端注册 | CIMD **SHOULD**；DCR **已弃用**，仅为向后兼容保留 | 新接入优先用 CIMD 托管客户端元数据 |
| 令牌绑定 | 客户端凭据必须按 issuer 绑定，**不得**跨 AS 复用 | 多租户场景要按 AS 隔离凭据存储 |
| 授权响应 | AS **SHOULD** 带 `iss`，客户端 **MUST** 校验（RFC 9207） | 防混流攻击，校验失败即拒绝兑换 |

stdio 是明确的例外：规范说这类实现 **SHOULD NOT** 走上述 OAuth 流程，而应从环境变量取凭据。这不是漏洞，而是信任边界不同——子进程由宿主应用亲手拉起，权限继承自本机用户，再套一层浏览器授权反而增加攻击面。反过来说，把 stdio 服务器通过端口转发暴露到公网，等于把本地信任边界搬到互联网上，必须自行补认证与来源校验。

对平台方最现实的一条：**私有部署的 MCP 服务器不要指望 Registry 解决分发**，注册与授权是两件事——Registry 只证明命名空间归属，不验证远程 URL 控制权，也不做安全扫描。

## 五、Registry：preview 状态的"元数据黄页"

MCP Registry（`registry.modelcontextprotocol.io`）由 Anthropic、GitHub、PulseMCP、Microsoft 等共建，当前**仍是 preview**，官方明示在 GA 前可能破坏性变更或数据重置。

它存储 `server.json`：唯一名称（反向 DNS，如 `io.github.user/server`）、包位置（npm/PyPI/NuGet/OCI/MCPB）或远程 URL、启动参数、描述与能力。发布用 `mcp-publisher` CLI；npm 包需在 `package.json` 写 `mcpName` 等于注册名；纯远程服务器写 `remotes[{type:"streamable-http",url}]`。schema 地址示例为 `https://static.modelcontextprotocol.io/schemas/2025-12-11/server.schema.json`。

| 维度 | 支持 | 不支持 |
| - | - | - |
| 服务器类型 | 开源、闭源；包分发或公网可访问的远程服务 | 私有网络、私有包 registry 上的服务器 |
| 信任机制 | 命名空间验证（GitHub 账号 / 域名） | 不验证远程 URL 归属，不做安全扫描 |
| 消费方式 | REST API、OpenAPI、分页 cursor、`isLatest` 标记 | 不直接给 host 应用用，不建议自托管官方代码库 |

发布流程有三道容易被忽略的校验：一是命名空间归属（GitHub 账号或域名），二是包与元数据的一致性（npm 包的 `mcpName` 必须等于注册名），三是版本号唯一性（同版本不能重复发布）。远程服务器还多一层约定：Registry 只验证你控制 `name` 里的命名空间，**不验证你控制 `remotes[].url`**，所以远程端点的真实性要由下游聚合器或用户自行判断。

实测 API 返回结构：`GET /v0/servers?limit=2` 给出 `servers[].server` 与 `_meta["io.modelcontextprotocol.registry/official"]`（含 `status`、`publishedAt`、`isLatest`），以及 `metadata.nextCursor`。这意味着下游聚合器可以按游标增量拉取，而不必全量同步。

## 六、扩展与治理：把"实验特性"移出核心

2026-07-28 把实验性 tasks 移出核心，改为官方扩展 `io.modelcontextprotocol/tasks`；同期官方扩展还有 Skills over MCP（通过 MCP 发现和读取 Agent Skills）与 MCP Apps（在对话内渲染图表、表单、视频等交互 UI）。扩展标识格式为 `{vendor}/{name}`，官方用 `io.modelcontextprotocol` 前缀，第三方用自己的反向域名。

扩展有明确的三段生命周期，避免"核心被实验特性拖住"：

| 阶段 | 允许的变更 | 批准方 | 实现者可预期 |
| - | - | - | - |
| Experimental | 任何时间、任何变更，不另行通知 | 扩展维护者 | 无保证，可能消失 |
| Beta | 可有变更，破坏性变更应罕见 | 扩展维护者 | 可以使用，破坏性变更会记录 |
| Stable | 只随核心规范新修订版变更 | 扩展维护者 | 行为在修订版内固定 |

Tasks 扩展的契约值得单独记一笔：客户端用 `tasks/get` 轮询状态，用 `tasks/update` 把用户中途输入送回服务端；阻塞式的 `tasks/result` 与 `tasks/list` 被移除，服务端也可以不经逐请求许可就返回任务句柄。相比“挂住一条流等结果”，这套接口把长任务变成了可持久化、可恢复、可审计的对象——对需要跑几分钟以上的爬取、批处理、代码执行类工具，这是更合适的形状。

治理上，MCP 已是 Linux Foundation 系列项目，采用 Lead Maintainers（BDFL）→ Core Maintainers → Maintainers（WG/SDK）→ Contributors 的层级。官方路线图（2026-08-22 更新）列了四个优先方向：Agentic Messaging Primitives（Tasks/订阅/进度统一生命周期，含 server-initiated events 与 webhooks）、HTTP 原生传输统一（HTTP over stdio、ETag 缓存）、Agent 身份与企业安全（DPoP、Workload Identity Federation、RFC 8693 token exchange）、原语改进（重设计 `tools/call` 返回形状、progressive discovery）。

## 七、落地检查清单

1. 先定协议版本：新项目直接按 2026-07-28 设计；存量按 2025-11-25 灰度，双栈期用 `server/discover` 探测。
2. 远程服务器按无状态服务部署，去掉会话粘性；为所有工具定义幂等键。
3. 权限模型先画清"用户—AS—资源服务器"三方，再选 CIMD 或 DCR；凭据按 issuer 分桶。
4. 长任务用 Tasks 扩展，不要靠长连接硬撑；客户端实现 `tasks/get` 轮询与 `tasks/update` 输入。
5. UI 型能力评估 MCP Apps 的渲染与安全边界；渐进披露（progressive discovery）适合工具数量大的服务器。
6. 发布用 Registry 只当"被发现"的入口，私有服务器自建 registry 并复用其 OpenAPI 形状。

## 来源

- https://modelcontextprotocol.io/specification/2026-07-28.md（访问 2026-11-15）
- https://modelcontextprotocol.io/specification/2026-07-28/changelog.md（访问 2026-11-15）
- https://modelcontextprotocol.io/specification/2025-11-25/changelog.md（访问 2026-11-15）
- https://modelcontextprotocol.io/specification/2026-07-28/basic/transports/streamable-http.md（访问 2026-11-15）
- https://modelcontextprotocol.io/specification/2026-07-28/basic/authorization/index.md（访问 2026-11-15）
- https://modelcontextprotocol.io/registry/about.md、/registry/quickstart.md（访问 2026-11-15）
- https://modelcontextprotocol.io/extensions/overview.md（访问 2026-11-15）
- https://modelcontextprotocol.io/community/governance.md、/development/roadmap.md（访问 2026-11-15）
- https://registry.modelcontextprotocol.io/v0/servers?limit=2（访问 2026-11-15）
- https://pypi.org/pypi/mcp/json、https://registry.npmjs.org/@modelcontextprotocol%2Fsdk/latest（访问 2026-11-15，Python SDK 2.3.0、TS SDK 1.32.2）

## 不确定项

- **2025-06-18 修订版**的具体条目未逐条核对，本文只把它标为兼容基线；引用其细节需回查官方 changelog。
- **2025-03-26 引入 Streamable HTTP** 来自公开版本脉络与规范交叉链接，本次未直接读该版 changelog 原文。
- Registry 的**服务器总量、日活**等规模数据官方未在文档中给出，本文未引用任何数量级。
- SDK 版本为查询时刻的最新值（Python 2.3.0 / TS 1.32.2），两条版本线独立演进，不能按数字大小比较成熟度。
- 2026-07-28 无状态化对**生态迁移进度**的影响缺少公开统计，本文只描述规范要求，不预测采用率。
