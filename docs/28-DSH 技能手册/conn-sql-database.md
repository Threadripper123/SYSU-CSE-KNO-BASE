# conn-sql-database

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,932 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-sql-database
description: "数据库连接器。直连 SQLite/MySQL/SQL Server/PostgreSQL 做查询、导出与结构探查。仅在需要读库数据、核对表结构或导出结果时使用。"
---

# 数据库连接器

## 适用场景

- 直接查库拿业务数据做分析，不经过应用层。
- 探查表结构与数据分布，写分析 SQL。
- 导出查询结果到 Excel/CSV。
- 不适用：优化慢查询本身（用 `gh-sql-optimization`）；需要改库表结构（先走变更流程）。

## 前置条件

- 连接信息：主机、端口、库名、账号；SQL Server 常需 `ODBC Driver 17/18`。
- Python 驱动：SQLite 内置 `sqlite3`；MySQL `pip install pymysql`；SQL Server `pip install pyodbc`；PostgreSQL `pip install psycopg2-binary`。
- **默认只读**。任何写操作先确认影响范围与是否有备份。

## 步骤

1. **先探查再查询**：`SELECT TOP 10 * FROM 表`（SQL Server）/ `LIMIT 10`（MySQL/PG），看清字段名、类型与取值样例。
2. 列结构：SQL Server 用 `INFORMATION_SCHEMA.COLUMNS`，MySQL 用 `DESCRIBE 表` 或 `information_schema`。
3. **查询写法**：
   - 只 SELECT 需要的列，不用 `SELECT *`。
   - 大结果集必须加 `TOP`/`LIMIT` 与 `ORDER BY`（无 ORDER BY 的分页结果不稳定）。
   - 聚合先 `GROUP BY` 关键维度，再用 `HAVING` 过滤，不要在大表上做全量拉取再内存汇总。
   - 时间范围条件放 WHERE 里且用可索引的写法（避免对列做函数运算，如 `WHERE YEAR(dt)=2026` 会失去索引）。
4. **Python 连接要点**：
   - SQLite：`sqlite3.connect(path)`，记得 `conn.close()`；并发写会锁库。
   - MySQL：`pymysql.connect(..., charset='utf8mb4')`，中文必须 utf8mb4。
   - SQL Server：`pyodbc.connect('DRIVER={ODBC Driver 18 for SQL Server};SERVER=...;TrustServerCertificate=yes')`。
   - 用 `pd.read_sql(sql, conn, params=[...])`，**参数化传值**，绝不字符串拼接。
5. **导出**：`df.to_excel(path, index=False)` 或 `to_csv(path, encoding='utf-8-sig')`（Excel 打开中文不乱码）。
6. **大表处理**：用 `chunksize` 分块读取，或先聚合再取结果。

## 常见坑

- 中文乱码：MySQL 连接未指定 utf8mb4；CSV 导出未用 `utf-8-sig`。
- SQL Server 证书错误：需 `TrustServerCertificate=yes` 或配好证书。
- 无 ORDER BY 做分页，翻页出现重复或漏行。
- 在 WHERE 里对索引列套函数，导致全表扫描。
- 忘关连接导致连接池耗尽（尤其循环里反复 connect）。
- 直接在生产库跑大批量查询拖垮业务，应加超时或走从库。
- SQL 注入：拼接用户输入进 SQL。
- SQLite 在网盘同步目录下会出现 database is locked。

## 验证

- 查询结果行数与预期量级一致，抽样核对几行原始数据。
- 关键聚合用另一种写法交叉验证（如 SUM 与分组求和对比总额）。
- 连接与游标显式关闭。
- 写操作前明确备份并记录影响行数。

## 参考

- 与 `gh-sql-optimization`（性能）、`gh-data-pipeline`（入库流程）、`gh-office-xlsx-extra`（结果呈现）配合。

## 变更记录

- 2026-11-11: 初始版本，记录三种驱动的连接坑与只读默认原则。
