# ch4.2 方差-图集-视觉描述

> 主题库：概率论与数理统计　｜　源文件：ch4.2 方差-图集-视觉描述.md　｜　653 字符　｜　8 个片段　｜　更新于 2026-11-13
## ch4.2 方差-图集 第 1 页
这是一张课程讲义中的插图，图中包含了一个关于正态分布的数学公式和推导过程。图中显示了随机变量X服从正态分布N(μ, σ^2)，其概率密度函数为：

\[ f(x) = \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}}, (-\infty < x < +\infty) \]

图中还给出了随机变量X的期望值E(X) = μ，方差D(X)的计算公式：

\[ D(X) = E[X - E(X)]^2 = \int_{-\infty}^{+\infty} (x - \mu)^2 \cdot \frac{1}{\sqrt{2\pi}\sigma} e^{-\frac{(x-\mu)^2}{2\sigma^2}} dx \]

通过变换变量t = (x - μ) / σ，公式简化为：

\[ D(X) = \frac{\sigma^2}{\sqrt{2\pi}} \int_{-\infty}^{+\infty} t^2 e^{-\frac{t^2}{2}} dt \]

进一步计算得到：

\[ D(X) = \frac{\sigma^2}{\sqrt{2\pi}} \left[ t(-e^{-\frac{t^2}{2}}) \right]_{-\infty}^{+\infty} + \int_{-\infty
