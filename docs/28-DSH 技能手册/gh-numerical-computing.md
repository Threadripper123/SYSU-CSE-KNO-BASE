# gh-numerical-computing

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,314 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: gh-numerical-computing
description: "数值计算与数据分析。用 NumPy/Pandas/SciPy 做数据处理、统计建模与数值模拟并保证可复现。仅在需要处理数据、跑数值实验或核对计算结果时使用。"
---

# 数值计算与数据分析

## 适用场景

- 大量数据清洗、聚合、透视，输出可信结论。
- 数值模拟：ODE 求解、蒙特卡洛、优化与曲线拟合。
- 验证解析结论、估计误差与收敛性。
- 不适用：纯符号推导（用 `gh-math-symbolic`）；报表排版（用表格类技能）。

## 前置条件

- `pip install numpy pandas scipy matplotlib`；本机 Python 3.12.10。
- 数据源确认：路径、编码（中文 CSV 常是 GBK）、缺失值表示方式。

## 步骤

1. **先看数据再写逻辑**：`df.head()`、`df.dtypes`、`df.describe()`、缺失率。不要跳过这步直接编码。
2. 读取时显式参数：`pd.read_csv(path, encoding='utf-8-sig', na_values=[...], dtype={...})`；日期列用 `parse_dates`。
3. 向量化优先：用布尔索引与 `groupby` 代替逐行 `apply`；确需逐行时先看数据规模是否可接受。
4. 数值模拟三要素：随机种子固定、迭代次数与收敛判断写明、结果给置信区间而不是单点值。
5. 优化与拟合：`scipy.optimize.curve_fit` 要给合理初值并检查 `pcov`；不收敛时先检查量纲与参数边界。
6. 误差检查：与解析解或小规模暴力计算对照；改变步长/样本量看结果是否稳定。
7. 输出：关键中间表与图保存到文件，报告里给出命令与参数，保证可复现。

## 常见坑

- 链式赋值 `df[df.a>1]['b']=0` 静默不生效，改用 `.loc`。
- 除法遇到 0 产生 inf 污染整列统计：先 `replace([inf,-inf], nan)`。
- 浮点比较用 `==`，应改用 `np.isclose`。
- 内存爆掉：先 `dtype` 降精度（float64→float32）、分块读取 `chunksize`。
- 用 `random` 而非 `numpy.random.Generator`，可复现性无法保证。
- 中文列名与 matplotlib 字体缺失导致乱码，需指定中文字体或输出 ASCII 轴标签。

## 验证

- 报告 N、参数、随机种子、软件版本。
- 至少一个独立对照（解析解/小样本/另一步长）。
- 结论中的数字能由给的脚本重跑得到。

## 参考

- 与 `gh-python-performance`（大规模数据处理提速）配合。
- 与 `gh-experimental-design`（对照与重复设计）配合。

## 变更记录

- 2026-11-11: 初始版本，确立可复现三要素。
