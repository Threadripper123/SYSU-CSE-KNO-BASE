# Chap5 图像复原与重建-图集

> 主题库：数字图像处理　｜　源文件：Chap5 图像复原与重建-图集.pdf　｜　1,179 字符　｜　23 个片段　｜　更新于 2026-11-13
g (xy f(x, y) Degradation g + Restoration f(x, y) filter(s) DEGRADATION RESTORATION

p(z)| p(z) | p(z)| 0.607, K √2πσ 7 Gaussian Rayleigh Erlang (Gamma) 一 0.607 √2πσ a(b − 1)b−1 K = c−1)! e− −(b−1) z − σ zz + σ z a (b − 1)/a z b z a 2

(z)| p(z)| p(z| b Exponential a Uniform Salt-and- Z a b z 0 V 2k-1 Z

2π0 0.607 /2π0 z—σzz+σ Z

p(z)  0.607 言 √ i  Rayleigh 1 1 a  z aT 2

K a elang Gaa k = a(b −1)− e e −(b−1) (b − 1)! (b − 1)/a z

A Exponential Z

b a Uniform a b z

p(z)| 1 − (Ps + Pp) F cYman a P, s pepper 0 V 2k-1 Z

T7I1T 1111111

c

H(u,ν) H(u,ν) H(u, ν) u u u 2 2 2 ab

H (u,ν) H (u,ν) H (u,ν) Ideal u 2 Gaussian u Butterworth ν ν ν cYa

小亮点成像系统H  g(x,y) 8

Di igital Image Processing

Dig l Pecessing igital Digital image Image Processing Processing

Image sne ocessig Processing   [ab

Pocessmg Digital Digital Image Image Processing Processing

Digital mme oCessing

Paoce i 8

Digital Image Processing

Di igital Image Processing

9.约束最小二乘方滤波参数y估计方法（也可以通过多次手工调优） 令 r是y的单调递增函数(前面解知道 F(u，v)是y的函数) 估计参数y，使 l=±a （） 其中，a为给定的精度因数 （a=0最佳）． ll体算法： )给定y值 2)计算 3) 如果(*)满足， 则停止。 否则如果 r|²< z²—a， 增加y； 如果    rl > z +a 减小γ，并回到步骤2.   78 School of Computer Science and Engineering
