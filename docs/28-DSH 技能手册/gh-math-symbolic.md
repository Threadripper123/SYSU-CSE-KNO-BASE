# gh-math-symbolic

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,527 字符　｜　8 个片段　｜　更新于 2026-11-11
---
name: gh-math-symbolic
description: "符号计算与公式推导。用 SymPy 做符号求解、化简、微积分、线性代数与公式验证。仅在做数学推导、把公式变成可执行代码或核对解析解时使用。"
---

# 符号计算与公式推导

## 适用场景

- 需要解析解：积分、微分方程、方程组求根、级数展开。
- 把手推公式转成可运行代码并验证等价性。
- 化简长表达式、提取系数、生成 LaTeX 供文档使用。
- 不适用：大规模数值计算（用 numpy/scipy），或只需要近似曲线拟合。

## 前置条件

- 已装 SymPy（`pip install sympy`）；本机 Python 3.12.10 可用。
- 明确符号定义域：实数、正数、整数；域不声明会让化简卡住或给出偏窄的结果。

## 步骤

1. **先声明符号与假设**：`x = symbols('x', real=True, positive=True)`。假设类型写错会导致 `simplify` 给出错误结论。
2. 写出目标表达式，先 `sp.simplify` / `sp.factor` / `sp.expand` 看形态，再决定后续策略。
3. 按目标选函数：
   - 求导/积分：`diff`、`integrate`（定积分给区间，未收敛会挂起，必要时设 `meijerg=False`）。
   - 解方程：`solve`（代数）、`dsolve`（微分方程）、`nsolve`（数值根，需给初值）。
   - 矩阵：`Matrix(...).eigenvals()` / `.inv()` / `.det()`。
   - 极限与级数：`limit`、`series`。
4. **验证而不是相信**：把结果代回原式，用 `simplify(lhs - rhs) == 0` 或 `sp.simplify(expr.subs(sol)) == 0` 断言；数值抽查若干点比较原式与结果。
5. 输出 LaTeX：`sp.latex(expr)`，直接粘进 Markdown 或论文。
6. **转为可执行代码**：`sp.lambdify` 生成 numpy 函数，比 `eval` 字符串快且安全；向量化用 `modules='numpy'`。

## 常见坑

- 未声明假设时 `sqrt(x**2)` 不化简为 `x`，误以为算错。
- `integrate` 对复杂被积函数可能长时间无返回：设超时或改用数值积分做对照，不要无限等。
- 浮点数进入符号表达式会污染精确性：全程用 `Rational(1,3)` 而不是 `1/3`。
- `solve` 返回的解集可能含重根或参数化形式，逐项代入验证而不是只看长度。
- `lambdify` 生成的函数对数组输入存在广播问题，标量公式要先测试形状。
- sympy 很慢：N 次循环里不要调用 `simplify`，先 `lambdify` 再算。

## 验证

- 每个推导结果都附一条代回校验（符号或数值）。
- 给出至少 3 个随机点的数值一致性对照。
- 若结果依赖假设，明确写出假设条件。

## 参考

- 上游：k-dense-ai/scientific-agent-skills 的 sympy（MIT，56.8k stars）。
- 与 `gh-numerical-computing`（数值验证）、`gh-experiment-report`（公式排版）配合。

## 变更记录

- 2026-11-11: 初始版本，强调假设声明与代回验证。
