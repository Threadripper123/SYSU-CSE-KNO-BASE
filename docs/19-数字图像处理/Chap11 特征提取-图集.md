# Chap11 特征提取-图集

> 主题库：数字图像处理　｜　源文件：Chap11 特征提取-图集.pdf　｜　3,900 字符　｜　30 个片段　｜　更新于 2026-11-13
111 1 0 11 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1

1 2 3 1 2 0 4 0 5 7 3 6

2          2       2 3 4 3

0 ↑1 3 (2)1 01 0 3 3 2 2 2 0 3 3 3 1 3 3 0 3 0 ↓3 2 2 左转90% 0 0 (3)2 1 21 0 0 3 3 3 入 331 3 3 0 3 0 2

0.6 0.5 0.4 0.12 0.7 、 1 0.3 / 0.20 0.8、 0.2 0.9、 -0.1 0.21 0.11 1 0 Line segment -1 0 -0.11 -0.9 -0.31 -0.1 -0.30 -0.12 -0.8 −0.2 -0.28 −0.21 -0.7 -0.3 -0.6 -0.5 -0.4 -0.28 -0.22 -0.24 abcde FlGuRE1l.6(a) Anopencurve.(b) A straight-line segment (c) Traversingthecurveusing circumferencestodeter mine slope changes; the dotis the origin (starting point).(d) Range of slope changes in the open interval (-1,1) (the arrowin the center ofthe chart indicates direction of travel).There can be ten subintervals between the slope numbers shown.(e) Resulting coded curve showing its corresponding numerical sequence of slope changes.(Cour- c

Direction of travel

θ θ —A— A— r(θ) r(θ) √2A A 0 π 2π π 0 π 2π 2 4 0 θ

S S

0 0 0 0 1 1 1 1 1 1 0 10 11 1 1 10 1 0 1 1 1 0 00 0 0 0 0 0 0 1 0

B  Curve n  A 50 2.3770 B 50 2.5132 C 50 1.6285

ab FIGURE11.15 (a) Fundus image from aprematurely born baby withROP. (b)Tortuosityof vesselsA,B,andC. Paofessorrneso Bribiesca,IIMAS- UNAM, Mexico.)

Order4 Order6 Chaincode:0321 003221 Difference:3333 303303 Shapeno.:3333 033033 Order8 Chaincode:0 0 3 32211 03 0 32211 0 00 32221 Difference:30 30 3030 3313 3030 30033003 Shapeno.:03030303 03033133 00330033

Chaincode:00 0030032232221211 Difference: 3 0 0 0 3 1 0 3 3 0 1 3 0 0 3 1 3 0 Shapeno:000310330130031303

jy →X 01 Real axis

ca e f g h FGURE 11.19(a) Boundary of ahuman chromosome (2868 points).(b)-(h)Boundaries reconstructed using 1434, 286,144,72,36,18,and8Fourierdescriptors,respectively.These numbers are approximately50%,10%,5%,2.5%, 1.25 %,0.63%, and 0.28 % of 2868,respectively. Images (b)-(h) are shown asnegatives to make the boundaries easier to see.

Transformation Beoundar Fourier Desecriptor Identity s(k) a(u) R tati n s,(k)= s(k)eiθ a()= a(u)jθ Translation s()=s()+ ∆xy a()= a()+ ∆xyδ(u) Scaling s()= αs(k) as(u)= αa(u) Starting p int p()= s(k − 0) ap()= a(u)e−12k

√A A 5q3\π 2π 4 4 2 4

c2 = a2 −b2 b Focus Focus a—

Majoraxis Binary e2 λ λ1 2 e1λ1ande2λ2arethe  Centroid of the covariance matrix of of region the coordinates of the region

Regionno. Ratioof lightsper (from top) region to total lights 0.204 0.640 0.049 0.107

区域编号区域灯光与总 （从上到下） 灯光之比 $ 0.204  0.640 3 0.049 4 0.107

Vertex -Face -Hole Edge

纹理均值标准差 R(归一化) 三阶矩均匀性熵平滑 82.64 11.79 0.002 -0.105 0.026 5.434 粗糙 143.56 74.63 0.079 -0.151 0.005 7.783 规则 99.72 33.73 0.017 0.750 0.013 6.674

1 2 3 4 5 6 7 8 1→120 00110 D7532 20 0 0 01100 51 61 25 30 1 0 10000 8 8 6 812 4 0 0101000 4 3 4 5 5 1 5 2 0 1 0 1 0 0 0 8 7 8 762 613 0 0 0 0 0 1  7 8 6 2 62 7 0 00 0 11 0 2 81 0 0 00 2 21 Imagef Co-occurrencematrixG

描述子解释公式最大概率度量G的最强响应， 值域是[0,1] maxPi i.j 个像素在整个图像上与其相邻像素有多相关的测度， 值域是相关 (i-m)j-m)pi [1，−1]，1对应于完全正相关，-1对应于完全负相关。标准差为零  σ,≠0,σ≠0 σ,σ。  时， 这个测度无定义 i=1J=1 个像素在整个图像上与其相邻像素之间的灰度对比度的测度，值 KK 对比度 ∑(ij)p 域是从0到（G为常数时）到(K−1)2 i=1j=1 均匀性 ∑p K 均性的个测度，值域为 。恒定图像的均性 (也称能量） =1j=1 同质性 G中对角分布的元素的空间接近度的测度，值域是[0，1]，G是对角   Pi 阵时同质性为最大值，即1 i=j=1 1+i− 熵 G中元素的随机性的测度。 当所有P 均为0时， 熵是0； 当P 均 - ilog2Pi K 匀分布时，嫡取最大值，因此最大值为2log₂K i=1j=1

归一化共生矩阵最大概率相关对比度均匀性同质性熵  G1/n 0.00006 -0.0005 10838 0.00002 0.0366 15.75 G2/n2 0.01500 0.9650 00570 0.01230 0.0824 06.43 G3/n3 0.06860 0.8798 03156 0.00480 0.2048 13.58

5.0 3.0 2.0 1.0 0 1.6 0 50 100   150 200 )   250    300 0 20 40 60 80100120140160180

缩放0.5后旋转45后旋转90后矩不变量原图像平移后的图像镜像图像的图像的图像的图像  2.8662 2.8662 2.8664 2.8662 2.8661 2.8662  7.1265 7.1265 7.1257 7.1265 7.1266 7.1265 Φ3 10.4109 10.4109 10.4047 10.4109 10.4115 10.4109 4 10.3742 10.3742 10.3719 10.3742 10.3742 10.3742  21.3674 21.3674 21.3924 21.3674 21.3663 21.3674  13.9417 13.9417 13.9383 13.9417 13.9417 13.9417  -20.7809 -20.7809 -20.7724 20.7809 -20.7813 -20.7809

λ1 λ2 λ3 λ4 λ5 λ6 10344 2966 1401 203 94 31

d an x= pt S cYaaaa
