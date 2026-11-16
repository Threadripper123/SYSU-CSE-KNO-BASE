# SYS-02 数据库与数据栈 2025–2026：PostgreSQL 18、DuckDB 1.x、Iceberg v3、ClickHouse、pgvector

> 主题库：现代软件与系统（2023–2026）　｜　源文件：（纯文本条目）　｜　7,336 字符　｜　29 个片段　｜　更新于 2026-11-15
> 资料截至 2026-11-15；覆盖 2023-11 至 2026-11，重点 2024–2026。

**一句话结论**：这套栈正在分叉成两条互不替代的路线——PostgreSQL 把分析能力（异步 I/O、skip scan、虚拟生成列、时间约束）持续塞进 OLTP 内核，形成「一个 PG 干完所有事」的引力；湖仓侧则是 DuckDB 2.0 转为 LTS、Iceberg v3 把删除向量与行级血缘写进规范，走向「开放表格式 + 就地计算」。

标题里的「DuckDB 1.x」是 2025 年的基线：到 2026-10-21，DuckDB 已经发布 2.0.0 LTS（代号 Cinnamon Teal）。本文按 2026-11-15 的真实版本写，标题保持任务分配原样。

## 关键变化总表

| 组件 | 2026-11-15 状态 | 关键版本与日期 | 这一轮最重要的变化 |
| --- | --- | --- | --- |
| PostgreSQL | 18.7 是稳定线；19 处于 RC2 | 18.0 于 2025-09-25；18.7 与 19 RC2 于 2026-11-12 | 异步 I/O、`uuidv7()`、虚拟生成列、OAuth |
| PostgreSQL 19 | 候选版本，未 GA | 文档标注「AS OF 2026-10-09」 | `REPACK`、`pg_plan_advice`、`WAIT` |
| DuckDB | 2.0.0 LTS | 2.0.0 于 2026-10-21；2.0.1 计划 2026-11-18 | 每两版一个 LTS、Quack 协议、DuckLake |
| Apache Iceberg | 规范 v3 已写入主规范 | Java 库 1.12.0 | 删除向量、行级血缘、variant/geometry |
| ClickHouse | 26.10 | 2026-10-22 | 旧查询分析器彻底移除、`system.metric_log` 桶化 |
| pgvector | 0.8.7 | 支持 PostgreSQL 13+ | halfvec/sparsevec、HNSW 与 IVFFlat |

## PostgreSQL 18：先换 I/O，再换默认值

PostgreSQL 18 于 2025-09-25 发布（`postgresql.org/docs/18/release-18.html`），官方列出的重点集中在「让 OLTP 数据库具备更多分析能力」：

- 异步 I/O（AIO）子系统，改善顺序扫描、位图堆扫描、`VACUUM` 等操作的吞吐。
- `pg_upgrade` 现在保留优化器统计信息，升级后不必立刻重建统计。
- 支持 skip scan，让多列 B-tree 索引在「前导列没有等值条件」时也能用上。
- `uuidv7()` 直接生成时间有序 UUID，省掉「时间戳 + 随机」的拼装。
- 虚拟生成列（读取时计算）成为生成列的默认形态。
- 支持 OAuth 认证；`INSERT`/`UPDATE`/`DELETE`/`MERGE` 的 `RETURNING` 可同时取 `OLD` 与 `NEW`。
- 时间约束：`PRIMARY KEY`、`UNIQUE`、`FOREIGN KEY` 可以定义在范围类型上。

真正让升级团队头疼的是不兼容变化，它们恰好都在默认值上：

| 变化 | 影响 | 应对 |
| --- | --- | --- |
| `initdb` 默认开启数据校验和 | 新集群默认带校验和 | 需要旧行为时用 `--no-data-checksums` |
| `pg_upgrade` 要求两端校验和设置一致 | 旧集群（无校验和）无法直接升级 | 升级前统一设置，或用上面那个开关重建 |
| 时区缩写优先取会话时区 | 依赖 `timezone_abbreviations` 的查询语义可能变化 | 回归测试里覆盖跨时区报表 |
| MD5 密码认证被弃用 | `CREATE ROLE`/`ALTER ROLE` 会告警 | 尽快迁到 SCRAM；临时可用 `md5_password_warnings = off` |
| `VACUUM`/`ANALYZE` 默认处理继承子表 | 分区表维护行为变化 | 确需旧行为时加 `ONLY` |
| `COPY FROM` 读 CSV 不再把 `\.` 当文件结束符 | 旧 `psql` 客户端做 `copy` 可能出错 | 升级客户端，或改用管道输入 |

## PostgreSQL 19：RC2 里能看到的方向

19 目前是候选版本。官方文档对它的发行日期仍写着「2026-??-??，AS OF 2026-10-09」，并且页面明确提示这是「不受支持的开发版本」——这句话本身就是最好的生产警告。已记录的亮点包括：

- 新命令 `REPACK`，回收磁盘空间并重排表内容；`REPACK (CONCURRENTLY)` 允许在不阻塞读写的情况下重打包。
- `pg_plan_advice` 扩展用于干预优化器决策，`pg_stash_advice` 则把建议按查询自动套用。
- I/O worker 进程数可自动伸缩；外键检查更快；规划与执行继续优化。
- 逻辑复制的门槛降低：`wal_level` 设为 `replica` 时无需重启即可开启逻辑复制；序列值也可逻辑复制。
- 新增 `WAIT` 命令，为只读副本提供「read-your-writes」一致性。
- autovacuum 支持并行处理表索引，并引入打分机制做优先级排序。

19 的不兼容项同样集中在认证与字符串语义：新增 `password_expiration_warning_threshold`（默认提前 7 天告警）、MD5 认证成功后也告警、移除 RADIUS 支持（官方理由是其只支持 UDP、安全性无法修复）、`standard_conforming_strings` 被强制为 `on`。

## DuckDB：2.0 转 LTS，版本策略变成一等公民

DuckDB 2.0.0 LTS「Cinnamon Teal」于 2026-10-21 发布，上一版 1.5.6 是 2026-09-28。更值得记住的是版本政策：从 1.4.0 起，每隔一个版本就是 LTS，社区支持期约为发布后一年；作为过渡，1.4 的支持被延长到 2026-11-17。发布日历显示 2.0.1 计划于 2026-11-18，2.1 排在 2027 年上半年，代号 Islandica。

按官网首页的划分，现在的「Duck Stack」包含三层：DuckDB 本体（进程内分析型数据库）、Quack（DuckDB 的客户端-服务器协议，两端都是完整 DuckDB 实例）、DuckLake（用 SQL 数据库做 catalog 的湖仓格式）；Iceberg 则是一等支持，可以直接读写。

对工程团队的含义很直接：嵌入式分析不必再「导出 CSV 再喂给 Pandas」，可以直接在 Parquet/Iceberg 上跑 SQL；但如果要把它放进长期服务，必须按 LTS 选版本，而不是看到新版就升——2.0 是大版本，行为变化不会限于补丁级别。

## Iceberg v3：删除向量与行级血缘

规范仓库里的 `format/spec.md` 已经把 v3 写成正式章节，v3 相对旧版新增：

- 新数据类型：纳秒精度时间戳（带时区）、`unknown`、`variant`、`geometry`、`geography`。
- 列的默认值支持。
- 分区与排序支持多参数 transform。
- 行级血缘（row lineage）追踪。
- 二进制删除向量（deletion vectors），替代按行扫描删除文件的旧路径。
- 表级加密密钥。

同一份规范里还出现了 v4 章节：自适应元数据树（提升小提交与列更新效率）、数据文件与其删除向量合并为一个条目（去掉两阶段 planning）、元数据中的相对位置、禁止再写 equality deletes、新增 `file` 类型、表约束、写默认值表达式。这些内容说明 v4 正在草案阶段，**不能**当成已发布能力来规划。

## ClickHouse 26.10：把旧分析器送走

ClickHouse 已改用日历式版本号，26.10 于 2026-10-22 发布。这一版最硬的变化是 backward incompatible：旧的那套查询分析（analyzer 之前的那一层）被彻底移除，它自 26.9 起就已不可选（`enable_analyzer` 成为废弃项）。如果集群里存在 26.10 之前的 initiator，会出现两个分析器对结果列命名不同而报 `NOT_FOUND_COLUMN_IN_BLOCK` 的风险；官方建议是升级前在所有节点显式设置 `enable_analyzer = 1`。

配套被移除或失效的开关还有 `use_analyzer_for_mutations`、`parallel_replicas_only_with_analyzer`，以及 `enable_optimize_predicate_expression`（谓词下推改由分析器在查询计划层完成；分布式子查询的 AST 层下推仍由 `allow_push_predicate_ast_for_distributed_subqueries` 控制）。

可观测侧的变化同样值得运维注意：`system.metric_log` 默认改为桶化 schema，所有 profile event 与 current metric 存在一个 `Map(Enum16(...), Int64)` 列里、固定 128 个桶，零值不落盘，读单个指标只碰一个桶；每个指标另有一个 ALIAS 列，`SELECT ProfileEvent_Query FROM system.metric_log` 这类查询不受影响。升级时旧表会被重命名为 `metric_log_0`，想保留旧结构要在配置里加 `<schema_type>wide</schema_type>`。

## pgvector 0.8.7：够用主义

pgvector 的定位是「把向量和业务数据放在同一个 Postgres 里」，而不是做一个独立向量数据库。当前 README 标的是 v0.8.7，支持 PostgreSQL 13 及以上，能力覆盖：精确与近似最近邻、单精度/半精度/二值/稀疏向量、L2/内积/余弦/L1/Hamming/Jaccard 六种距离，以及 HNSW 与 IVFFlat 两类索引。向量规模大时用 quantization 压内存。

| 场景 | 建议 | 理由 | 代价 |
| --- | --- | --- | --- |
| 千万级以内、需要 JOIN 与事务 | pgvector | 与业务表同库，天然 ACID | 索引内存占用随维度上升 |
| 十亿级、写入吞吐优先 | 专用向量库或分片方案 | 单机索引规模受限 | 多一套系统与同步链路 |
| 已有 Iceberg/湖仓、只做离线召回 | DuckDB + 列存扫描 | 免运维、可直接读湖 | 不适合高并发在线查询 |
| 强 SQL 分析与半结构化混合 | PostgreSQL 18/19 | AIO 与 skip scan 改善分析 | 复杂聚合仍不如列存 |

## 版本策略：三种节奏，别混着用

| 项目 | 节奏 | 支持承诺 | 升级建议 |
| --- | --- | --- | --- |
| PostgreSQL | 每年一个大版本，每季度一次补丁 | 每个大版本约 5 年 | 大版本升级走 `pg_upgrade` + 校验和检查 |
| DuckDB | 约每月一版，每两版一个 LTS | LTS 社区支持约 1 年 | 生产只跟 LTS，跳版前跑回归 |
| ClickHouse | 日历月版本（26.10 = 2026 年 10 月） | 按版本线维护 | 混版本集群先统一 analyzer 开关 |
| Iceberg | 规范与 Java 库分开演进 | 规范 v3 已定，库 1.12.0 | 先确认查询引擎支持的能力子集 |

这张表解释了一个常见误解：DuckDB 的发布频率远高于 PostgreSQL，但它的 LTS 承诺只有一年，所以「用最新版」在 DuckDB 上是高风险的默认动作；反过来在 PostgreSQL 上，跟着大版本走反而是官方支持的路径。

## 对数据团队的实际影响

- 单库统一：PostgreSQL 18 的 AIO 与 skip scan 让「报表也放主库」在更多场景下可行，前提是索引设计配合——skip scan 只对多列 B-tree 有效，前导列选择变得更重要。
- 湖仓读取：Iceberg v3 的删除向量会显著降低「边删边读」场景的读放大，但只有当引擎实现了 v3 的删除向量才有收益；仅升级表格式没有意义。
- 本地分析：DuckDB 2.0 的 LTS 定位适合「数据科学工作站」与 CI 中的小规模校验，不适合当作多租户在线服务。
- 向量检索：pgvector 的调参集中在索引侧——HNSW 看 `m` 与 `ef_construction`，IVFFlat 看 `lists`；先定召回率目标，再反推内存预算。
- 可观测：ClickHouse 26.10 的 `system.metric_log` 桶化会改变监控查询的写法习惯，自建看板若直接读旧列，需要改到 ALIAS 列或旧表 `metric_log_0`。

## 常见坑与局限

- **把 19 RC2 当生产版本**：官方文档页面自己就写着「不受支持的版本」。RC2 于 2026-11-12 与 18.7 同日发布，容易看混。
- **升级前不查校验和**：18 默认开启数据校验和，而 `pg_upgrade` 要求两端一致，这是最常见的升级卡点。
- **`\.` 语义变化**：`COPY FROM` 读 CSV 不再把 `\.` 当 EOF，但 `psql` 从标准输入读 CSV 仍当 EOF——新旧客户端混用时表现为「少了几行数据」。
- **只在规范里看到 v3 就以为能用**：Iceberg 的能力取决于引擎实现，规范先行、引擎滞后是常态。
- **ClickHouse 混版本升级**：先统一 `enable_analyzer = 1` 再逐节点升级，否则旧 initiator 会踩到列名不一致。
- **把 DuckDB 的 LTS 理解成「不升级」**：LTS 是支持期承诺，不是行为冻结；大版本之间的行为差异仍需回归。

## 来源

以下页面均在 2026-11-15 实际抓取：

- PostgreSQL 18 发行说明：https://www.postgresql.org/docs/18/release-18.html
- PostgreSQL 19 发行说明（开发版）：https://www.postgresql.org/docs/19/release-19.html
- PostgreSQL 官网版本横幅（18.7 与 19 RC2）：https://www.postgresql.org/
- DuckDB 官网：https://duckdb.org/
- DuckDB 发布日历：https://duckdb.org/release_calendar
- DuckDB 工程博客索引（2.0.0 LTS 日期）：https://duckdb.org/news/
- Iceberg 规范源码：https://raw.githubusercontent.com/apache/iceberg/main/format/spec.md
- Iceberg 规范站点：https://iceberg.apache.org/spec/
- ClickHouse 2026 changelog：https://clickhouse.com/docs/en/changelog
- pgvector README：https://raw.githubusercontent.com/pgvector/pgvector/master/README.md

## 不确定项

- PostgreSQL 19 的 GA 日期：文档尚未给出，只能确认 2026-10-09 之后仍在改、2026-11-12 处于 RC2。
- DuckDB 2.0.0 的具体特性清单：官网博客由前端渲染，`web_fetch` 取不到正文，故本文只引用可验证的版本、日期与 LTS 政策；RSS 在抓取时也被截断，未覆盖 10 月这篇。
- Iceberg v4 的状态：主规范里已有章节，但未找到「已批准」的公告，按草案处理。
- ClickHouse 26.10 之后的 26.11 是否存在：changelog 页面当前只列出 26.10。
- pgvector 是否已发布 0.9.x：README 的安装示例仍指向 `v0.8.7`，未进一步核对仓库 tag。
