# INDEX-技能库总索引

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　7,848 字符　｜　37 个片段　｜　更新于 2026-11-11
# DSH 技能库索引

> 位置：`C:\Users\CCW\.dsh\skills`　|　更新：2026-11-11　|　**52 个技能目录**（gh- 37 / conn- 8 / 其他 7）

本文件是技能库总目录。新增技能后**同步更新本表**；技能正文以各 `SKILL.md` 的 frontmatter 为准，本表只做路由。

---

## 一、命名与结构约定

| 前缀 | 含义 | 数量 |
| --- | --- | --- |
| `gh-` | 领域专家（方法论 + 排错经验） | 37 |
| `conn-` | 连接器（外部系统接入用法） | 8 |
| 无前缀 | 专用任务技能 | 7 |

```
<skill-name>/
  SKILL.md        # 必需；frontmatter 含 name 与 description
  references/     # 可选；长参考资料
  assets/         # 可选；模板与示例
  UPSTREAM.md     # 可选；上游来源与同步说明
```

**硬约束**：`name` 必须与目录名完全一致且为 ASCII kebab-case；`description` 写"什么时候用"而非实现细节；正文中文，name 英文。

---

## 二、计算机方向

### 2.1 工程专家

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-api-design` | REST/GraphQL 接口与契约设计审查 | 设计接口、定错误码与分页 |
| `gh-architecture-patterns` | 系统架构、模块拆分、单体改造、依赖循环 | 系统设计、重构分层 |
| `gh-sql-optimization` | 慢查询、索引、N+1、响应瓶颈 | 查询变慢、库表评审 |
| `gh-python-performance` | Python CPU/内存/IO/并发优化 | 服务变慢、批处理提速 |
| `gh-code-review` | 分层审查 + 阻塞/建议/疑问三档意见 | 审 PR、把关合并、提交前自查 |
| `gh-testing-strategy` | 测试金字塔、从改动反推用例 | 补测试、定位失败用例 |
| `gh-devops-delivery` | CI 流水线、镜像分层、发布回滚、可观测性 | 搭发布、写 Dockerfile、线上复盘 |
| `gh-security-review` | 注入/鉴权/密钥/依赖漏洞审查与加固 | 上线前安检、排查漏洞 |
| `gh-data-pipeline` | ETL、增量同步、数据质量校验 | 搭管道、同步数据、治脏数据 |

### 2.2 连接器

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `conn-mcp-builder` | 封装 MCP server（FastMCP / Node SDK） | 给 agent 加工具、集成第三方 |
| `conn-github-cli` | `gh` CLI 与 REST API 查仓库/PR/CI | 查 GitHub 数据、批量处理 PR |
| `conn-web-automation` | Playwright 驱动真实浏览器 | 抓动态页、登录态操作、E2E |
| `conn-sql-database` | 直连 SQLite/MySQL/SQL Server/PG | 读库数据、导结果、探结构 |
| `conn-n8n-workflow` | n8n 节点编排、表达式、错误分支 | 搭 n8n 流程、排查报错 |
| `conn-webhook-receiver` | 收 Webhook（签名+去重）与推送通知 | 外部触发本机、结果通知 |
| `conn-platform-messaging` | IMAP/SMTP、群机器人、云文档云盘 | 读写邮箱、群通知、办公对接 |
| `conn-market-data-api` | AkShare/Tushare/yfinance 金融数据 | 拉行情、财务、宏观数据 |

---

## 三、数学方向

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-math-symbolic` | SymPy 符号求解、微积分、线代、公式转代码 | 要解析解、验证推导、出 LaTeX |
| `gh-numerical-computing` | NumPy/Pandas/SciPy 数据处理与模拟 | 清洗数据、数值实验、核对结果 |
| `gh-operations-research` | 线性/整数规划、排程、库存模型 | 资源分配、排班排产、路径优化 |
| `gh-experimental-design` | 对照、随机化、重复、因素设计 | 设计实验方案 |
| `gh-experiment-report` | 依据真实数据写实验报告 | 出报告、排公式图表 |

**链路**：符号推导 → 数值验证 → 实验设计 → 成文。

---

## 四、自动化工作流方向

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-automation-workflow` | 状态外置、幂等、失败分类、定时化 | 批处理、定时任务、多步编排 |
| `conn-n8n-workflow` | 可视化跨系统流程编排 | 需要图形化流程 |
| `conn-webhook-receiver` | 事件入口与出口 | 外部触发、结果推送 |
| `conn-web-automation` | 网页侧自动化动作 | 流程含网页操作 |
| `gh-data-pipeline` | 数据流转与质量门禁 | 流程含数据同步 |
| `gh-devops-delivery` | 服务化部署与定时 CI | 流程长期跑在服务器 |

**典型组合**：Webhook 收事件 → `gh-automation-workflow` 幂等处理 → `conn-web-automation`/`conn-sql-database` 取数与操作 → `conn-webhook-receiver` 推送结果。

---

## 五、经管方向（经济与管理）

### 5.1 战略与商业

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-business-analysis` | 市场规模 TAM/SAM/SOM、竞争格局、单位经济 | 评估商机、写行业研究 |
| `gh-strategy-frameworks` | SWOT/五力/价值链/商业模式画布 | 战略取舍、业务诊断 |
| `gh-product-management` | 需求分析、RICE 优先级、PRD、路线图 | 定义功能、写需求文档 |
| `gh-project-management` | WBS、关键路径、缓冲、风险登记 | 排期、跟进度、处理延期 |

### 5.2 运营与职能

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-marketing-analytics` | 漏斗、留存同期群、渠道归因、ROI | 分析用户、评估投放 |
| `gh-supply-chain` | 需求预测、安全库存、补货策略、供应商评估 | 管库存、优化采购 |
| `gh-accounting-tax` | 三表编制、成本核算、增值税/所得税测算 | 做账、编报表、算税负 |
| `gh-econ-analysis` | 弹性测算、DID/RDD、宏观数据解读 | 经济分析、政策评估 |

---

## 六、金融方向

| 技能 | 用途 | 何时加载 |
| --- | --- | --- |
| `gh-financial-modeling` | 三表联动、DCF、相对估值、敏感性 | 财务预测、估值测算 |
| `gh-investment-analysis` | 基本面、估值判断、组合构建、绩效归因 | 研究标的、检视持仓 |
| `gh-market-data-quant` | 行情清洗、因子、无偏回测、稳健性检验 | 验证策略、跑量化 |
| `gh-risk-management` | VaR/ES、敞口拆解、压力测试、限额 | 量化风险、评估极端损失 |
| `conn-market-data-api` | AkShare/Tushare/yfinance 数据拉取 | 取行情与财务数据 |

> 金融类技能输出为分析框架与量化参考，**不构成投资建议**；真实资金决策需持牌专业人士复核。会计税务以现行法规与主管税务机关口径为准。

---

## 七、写作与办公方向

| 技能 | 用途 |
| --- | --- |
| `gh-biz-writing` | 周报、方案、汇报材料（结论先行） |
| `gh-research-report` | 行业研究、尽调、深度报告 |
| `gh-reading-report` | 阅读报告与文献综述 |
| `gh-data-visualization` | 图表选型与可视化叙事 |
| `gh-office-docx-extra` | Word 文档制作与校对 |
| `gh-office-xlsx-extra` | Excel/CSV 分析、公式、样式与图表 |
| `gh-office-pptx-extra` | PPT 制作与模板套用 |
| `gh-office-pdf-extra` | PDF 文字表格提取、拆分合并、表单 |
| `cnlab-report-docx` | 按模板生成中文实验报告并转 PDF |

---

## 八、常见任务路由（速查）

| 你说 | 加载 |
| --- | --- |
| "review 这个改动" | `gh-code-review` |
| "要写哪些测试" | `gh-testing-strategy` |
| "配 CI / 怎么回滚" | `gh-devops-delivery` |
| "这个接口有安全问题吗" | `gh-security-review` |
| "封装成工具给 AI 用" | `conn-mcp-builder` |
| "查仓库 PR / CI 挂了没" | `conn-github-cli` |
| "抓网站数据 / 自动登录" | `conn-web-automation` |
| "查下这个库里的数据" | `conn-sql-database` |
| "搭个 n8n 流程" | `conn-n8n-workflow` |
| "别人调我的接口触发任务" | `conn-webhook-receiver` |
| "读邮箱 / 发群通知" | `conn-platform-messaging` |
| **"算这个市场规模"** | `gh-business-analysis` |
| **"该不该做这个业务"** | `gh-strategy-frameworks` |
| **"写个 PRD"** | `gh-product-management` |
| **"排个项目计划"** | `gh-project-management` |
| **"分析投放 ROI"** | `gh-marketing-analytics` |
| **"库存怎么定补货"** | `gh-supply-chain` |
| **"算下税负 / 编报表"** | `gh-accounting-tax` |
| **"评估这个政策效果"** | `gh-econ-analysis` |
| **"搭个财务模型 / 估值"** | `gh-financial-modeling` |
| **"这个股票值不值得买"** | `gh-investment-analysis` |
| **"回测这个策略"** | `gh-market-data-quant` |
| **"算 VaR / 做压力测试"** | `gh-risk-management` |
| **"拉一下股价和财报"** | `conn-market-data-api` |
| "解这个方程 / 推这个积分" | `gh-math-symbolic` |
| "处理这份数据" | `gh-numerical-computing` |
| "排班怎么最优" | `gh-operations-research` |
| "每天自动跑一遍" | `gh-automation-workflow` |
| "做张图 / 画看板" | `gh-data-visualization` |
| "写周报 / 汇报材料" | `gh-biz-writing` |
| "写份行业报告" | `gh-research-report` |

---

## 九、跨方向组合链路

- **投资决策**：`conn-market-data-api` 取数 → `gh-investment-analysis` 基本面 → `gh-financial-modeling` 估值 → `gh-risk-management` 下行 → `gh-research-report` 成文。
- **立项论证**：`gh-business-analysis` 市场 → `gh-strategy-frameworks` 战略 → `gh-financial-modeling` 财务 → `gh-project-management` 排期 → `gh-biz-writing` 汇报。
- **增长优化**：`gh-marketing-analytics` 找断点 → `gh-experimental-design` 做 A/B → `gh-numerical-computing` 显著性 → `gh-data-visualization` 呈现。
- **数据产品**：`gh-data-pipeline` 建管道 → `gh-sql-optimization` 提速 → `gh-data-visualization` 看板 → `gh-automation-workflow` 定时刷新。
- **系统交付**：`gh-architecture-patterns` 设计 → `gh-api-design` 契约 → `gh-code-review`/`gh-security-review` 把关 → `gh-devops-delivery` 发布。

---

## 十、其他专用技能

| 技能 | 用途 |
| --- | --- |
| `gh-web-search` | 无 API 密钥的 DuckDuckGo 搜索，超时可回退 GitHub 公共 API |
| `gh-internet-research` | 互联网资料检索与证据核验 |
| `skill-evolution` | 任务收尾时把可复用经验沉淀为新技能或升级已有技能 |
| `dsh-plugin-install-troubleshoot` | Windows 上给 DSH 装插件的排错（镜像、allowBuilds、bundles 闸门） |
| `diagnose-windows-sandbox-acl` | 排查并修复 Windows 沙箱 ACL 导致的意外拒绝 |
| `pyinstaller-tk-exe-verify` | 验证 PyInstaller + tkinter 打包产物、从外部驱动 Tk 程序 |
| `astrbot-qq-image-recovery` | AstrBot + NapCat 的 QQ 图片 HTTP400 修复 |
| `yuanbao-desktop-dialogue` | 通过 UIAutomation 与元宝桌面端对话并取回真实回答 |
| `kdocs` | 金山文档 / WPS 云文档操作（插件自带） |

> 插件自带的 `docx`、`office`、`univer-*` 系列不在本目录内，由插件提供，不列入统计。

---

## 十一、维护

**新增技能**：建目录 → 写 `SKILL.md`（`name` = 目录名）→ 更新本索引 → 运行校验。

**校验脚本**：

```powershell
$root = "$env:USERPROFILE\.dsh\skills"
Get-ChildItem $root -Directory | ForEach-Object {
  $f = Join-Path $_.FullName 'SKILL.md'
  if (-not (Test-Path $f)) { "NO_SKILL_MD: $($_.Name)"; return }
  $name = ((Get-Content $f -TotalCount 5) | Where-Object { $_ -match '^name:' }) -replace '^name:\s*', ''
  if ($name.Trim() -ne $_.Name) { "MISMATCH: dir=$($_.Name) name=$($name.Trim())" }
}
```

**上游同步**：源自社区的技能在目录写 `UPSTREAM.md` 记录 repo / 路径 / license / 抓取日期。

**已知环境限制**（2026-11-11 实测）：

- `raw.githubusercontent.com`、`cdn.jsdelivr.net` 超时 → 抓 GitHub 文件走 contents API 再 base64 解码。
- `registry.npmmirror.com`（313ms）远快于官方源（2.2s），装包优先用镜像。
- `dsh` CLI 不在 PATH。
- Python 3.12.10 已装：numpy 2.2.6、pandas 3.0.3、scipy 1.15.3、sympy 1.14.0、scikit-learn 1.9.0、openpyxl 3.1.5。
- **未装**（相关技能已写明安装步骤）：matplotlib、statsmodels、pulp、akshare、tushare、yfinance。
