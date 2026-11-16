# conn-market-data-api

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,705 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: conn-market-data-api
description: "金融数据接口连接器。用 AkShare/Tushare/yfinance 获取股票、基金、期货、宏观与财务数据。仅在需要拉取金融行情或经济数据时使用。"
---

# 金融数据接口连接器

## 适用场景

- 拉 A 股/港股/美股行情、财务指标、指数成分。
- 取宏观经济数据（CPI、M2、社融、利率）。
- 取基金净值、期货持仓、可转债数据。
- 不适用：需要交易所实时 Level-2 行情（商业数据源）；纯统计建模（用 `gh-numerical-computing`）。

## 前置条件

- 本机三者**均未安装**，按需装：
  - `pip install akshare` —— 免费，无需 token，接口多但字段名与结构随上游网站变动，**版本升级易破坏**。
  - `pip install tushare` —— 需注册 token，稳定但积分制限制频次。
  - `pip install yfinance` —— 海外市场，需外网可达；本机 raw.githubusercontent 已超时，海外网络稳定性需先验证。
- 装完先小样本试拉一次再写批量逻辑。

## 步骤

1. **先确认接口签名**：AkShare 接口名随版本变化，用 `dir(akshare)` 或文档确认当前版本可用函数，不要照抄旧教程。
2. **小样本验证**：先拉单只票、短区间，打印 `df.head()` 与 `df.dtypes`，确认列名与单位。
3. **批量拉取加节流**：每次调用间隔 0.5–1 秒，失败重试 3 次并指数退避；AkShare 底层抓网页，高频会被限流或封 IP。
4. **缓存到本地**：拉到的数据落 Parquet/CSV（按日期分区），后续分析读本地。避免每次分析重复请求。
5. **Tushare 用法**：`pro = ts.pro_api(token)`，注意接口有调用频次上限与积分门槛；token 从环境变量读 `TUSHARE_TOKEN`。
6. **yfinance 用法**：`yf.download(symbols, start, end, auto_adjust=True)`，`auto_adjust=True` 直接给复权价，避免自己算复权出错。
7. **字段标准化**：不同源列名不同（`trade_date`/`日期`/`Date`），落库前统一为英文 snake_case + 明确单位（元/万元/亿元）。
8. 使用时配合 `gh-market-data-quant` 的清洗与回测流程。

## 常见坑

- AkShare 接口在版本升级后改名或返回结构变化，脚本静默返回空 DataFrame；必须校验 `df.empty` 并报错。
- 直接拿去回测却忘记复权，出现虚假跳空收益。
- 高频请求被封 IP，表现为连接超时或返回空，容易误判为网络问题。
- Tushare 积分不足时接口直接报权限错误，不是数据不存在。
- 金额单位不统一（有的函数返回万元，有的返回元），直接相加得出错误结论。
- 交易日历缺失：非交易日返回空，需要与交易日历对齐而不是简单按自然日重采样。
- token 写进代码提交到仓库。

## 验证

- 先拉一只票验证列名与数值合理（价格量级、日期范围）。
- 空数据、非交易日、代码不存在三种情况都有明确报错路径。
- 批量拉取记录成功率与失败清单。

## 参考

- 与 `gh-market-data-quant`（回测）、`gh-financial-modeling`（财务数据）、`gh-investment-analysis`（基本面研究）配合。
- 本机网络：registry.npmmirror.com 快（313ms），装包优先用 `-i` 指定镜像。

## 变更记录

- 2026-11-11: 初始版本，记录 AkShare 版本漂移与单位不统一的坑。
