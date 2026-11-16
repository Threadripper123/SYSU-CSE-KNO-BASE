# Lecture7-机器学习3-深度学习-图集-矢量

> 主题库：人工智能　｜　源文件：Lecture7-机器学习3-深度学习-图集-矢量.pdf　｜　2,568 字符　｜　78 个片段　｜　更新于 2026-11-13
神经网络 Axon Synapse Synaptictermi Postsynapticc Neurotransmitter + + σ(z) 十 "Neuron”

1  0.98 1  -2 -1 0.12 S 1 0 Sigmoid Function 1 0.5 1+e−²

0.98 :  0.12

C

1  0.98 2 0.86 3 0.62 0.12 -2 0.11 -1 0.83 1 -1 4

x x2f W1 W2 W| b²2 X b1 X b W1 b1 X + W2 a1 + b2 σ( WL al-1 + b

X2 W1 W2 W X b1 X b2 b X Ym x a1 a y f 使用并行计算技术来 x 加速矩阵操作

1 0.73 2 0.72 3 0.51 0 2 0.5 -2 0.12 -1 0.85 1 -1 4 0 0 2

SpECIal structure

cuaeer-left beak" detector

Property 1 一些特征会比整张图片小很多 Property 2 相同特征会出现在不同区域 Property 3 像素二次采样不会改变识别的目标信息 Flatten

Convolution Max Pooling Convolution

1 -1 -1 -1 1  -1 -1 -1 1

-1 1 -1 -1 1 -1 -1 1 -1

1 0 0 0   0 1 0 0 0 0 1 1

1 -1 -1 -1 1 -1 -1 -1 1

3 -1

1 0 0 0 0 0 1 0 0 1 0 0 1 1 0

1 -1 -1 -1 1 -1 -1 -1 1

3 -3

0 0 0 0 1 0 1 0 0 1 0  0 0 1 0 0    00 0 1 0   0 0 0 1 0 0 0 1 0 1 0

3 -1 -3 -1  -3 0 -3

-1 -1 -1 -1 -1 区

-1 -1 -1 -1 -1 -1 - 2 1 特征图 -1 -1 -2 1 -1 0 -4 3

1 0 0  0 1   0 1 0 0 1 0 0 0 1 1 0 0 1 00 0 1 0 0 1 0 0 1 0 0 0 1 0 1 0

-1 1 -1 -1 1 -1 -1 1 -1

1 0 0 0 0 1 1 -1 -1 -1 1 -1 0 1 0 0 1 0 -1 1 -1 -1 1 -1 -1 -1 0 0 1 1 0 0 -1 -1 1 -1 1 -1 -1 -1 -2 1 1 0 0 0 1 0 0 1 0 0 1 0 -1 -1 -2 1 0 0 1 0 1 0 卷积 1 4 3 Image

捲积核1 1 0 0 0 -3 0 3 0 1 0 0 1 -3 -3 0 1 0 0 1 1 0 [3 -2 -2 -1 1 0 0 0 1

3 -1 -3 -1 -3 1 0 -3

1 -1 -1 -1 1 -1 -1 -1 1

-1 1 -1 -1 1 -1 -1 1 -1

Conv Max Pooling

-1  1

0 3

Convolution Max Pooling 小 Convolution 量相同 Max Pooling

cYa 0 3

Iwould like to arrive  on May 31th ticket booking system

示） x1 x2

31th time 31th S x1 x 2

arrive  on May 31th

leave Guangzhou on May 31th

隐藏层输出存 [t在记忆中忆内容可以用作 x1

[2 为“1”，没有偏置 1 1 函数都为线性

[4][1 2] 12 12 6 6 1 1

了小· [41[12[32] 32 32 序列的顺序将会生影响！ 16 16 2 2

al_ a2 a a1 l a arrive  on May 31th

store 1 a2 al X2 Guangzhou 同的

store 1 a2 al X2

xt+1

xt+2

xt

特殊神经元： 4 项输入， [aanputGate 1项输出 Memory Forget 遗忘门 Cell Gate 控制信号

9 ×1 100 ×2 10 1

×1 0 10g Ya 1

0 3 100 + 1 O 10 1

3 0 1 100 + O

3 、0 1. 0  + 0 zoo 1

Input Gate Input Gate Input Gate Bloc g Block

al Copy a Copy a a1 1 a 吾句： arrive  On May 31th

otner 0 1 0

Other 0 1 0

0 1 10

ckpropagation ough time (BPTT) a2 W −η∂L/ ∂w x₁ x 2

0

W : = 1 y1000 = 1 Large Small y1000 W = 1.01 ≈ 20000 ∂L/∂w Learning rate?  W = 0.99 y1000 ≈ 0 small Large W : = 0.01 y1000 ≈0 ∂L/∂w Learning rate?

Y Y Y 1 4 4 1 1 1 W w 1 1 1 1 0 0

Y 1 W 1

FC FC FC  FC  EXT Self-attention

cb relevant? a a a3 a

attention score u1,2 u1,3 α1,4 k2 q query key k k4

On dttenioi scoies i b α1,1 α1,2 α1,3 α1,4 x x q 1 k1 k2 ν3 V4 k3

b a2,1 a2,2 a2,3 a2,4 q1 k1 q2 k2 V q3 k3 v3 k4

q4 k4 a4

q3 k3 v3

q2 1 k2 v2 ca

q k11 [a

KT Q

A
