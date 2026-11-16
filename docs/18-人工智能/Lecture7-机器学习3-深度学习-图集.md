# Lecture7-机器学习3-深度学习-图集

> 主题库：人工智能　｜　源文件：Lecture7-机器学习3-深度学习-图集.pdf　｜　3,381 字符　｜　45 个片段　｜　更新于 2026-11-11
SCIENCEphotOLIBRARY

树突轴突末梢细胞核轴突

Figure 2. McCulloch-Pitts model of a neuron.

输入1 权值1 权值2 输2 ↓ 求和非线性函数 → 输出权值3 输入3

a2 Sum Sgn a3 z=g(a₁*W₁+a2*W2+ a₃*W₃3)

a2 W2 a2 W2 sum sen f as W3 a3 W3

Perceptron   5

22 _ 2,3 z₁=g(a₁*W11+Q2*W1,2+a₃*W1,3) Z2= g(α1*W2,1+a2*W2,2+C₃*W2,3)

y=Sgn(w·x) sgn(y) = = { , if y > 0  other wise

Y WO XO W1 W2 Wn X1 X2 Xn

Wi ← Wi + ∆ wi ∆wi = η(t − 0)xi

00

1,1) 0 (1,1) 0 (1,1) 1   ? AND XOR   輸出0 輸出1

隐藏层输入层输出层

w(1) a(2) w{2} Z

w(1) (2) a W(2) z

0.5  0 -0.5 -1 -1 -0.5 0 0.5 1

0.5 -0.5 -1 -0.5 0 0.5 1

大

d1 do

100 0.8 0.75 0.6 2 0.50   0.25 04 0.00 -0.25 - 0.2 -0.50  O.O −0.75  -1.00 -0.2 −1.00 −0.75 −0.50 −0.25 0.00 0.25 0.50 0.75 1.00 -0.5 0.0 0.5 10 15 20

Input 2 x 64tanh

Input 1 x 32 tanh 2 x 32tanh 3 x 32 tanh 4 x 32 tanh 5 x 32 tanh 6 x 32 tanh 7 x 32 tanh 8x32 tanh 9x32 tanh 10x 32 tanh 11 x32 tanh 12 x 32 tanh 13 x 32 tanh 14 x 32 tanh 15 x 32 tanh

Stimulus Nucleus Presynaptic cell Axon hillock Cell body Axon Synapse Synaptic terminals Postsynaptic cell Neurotransmitter

1 0.5 0

Network parameters θ = {W1,W2,, b1,b2,. } Starting θ0 θ1 θ2 Parameters ∇L(θ) Compute ∇L(θ ) θ = θ0 − η∇L(θ ) ∂L(θ)/∂w1 ∂L(θ)/∂w2 Compute ∇L(θ1) θ2 = θ − η∇L(θ1) ∂L(@)/ob1 Millions of param eters ... . oL(o)/ab₂ To compute the gradients efficiently, We use backpropagation.

Case 1 y = g(x) : ) z = h(y) dz dz dy ∆x → ∆y → ∆z dx dy dx CASEZ x = g() y = h() z = (x, ) ∆s· dz ∂z dx ∂z dy Δz ds ∂x ds ∂y ds ∆y

N ∂L(θ) N L(θ) = ∑ cn(θ) ∑ ∂cn(θ)  =  ∂w ∂w n=1 n=1 y1 x1

W1 x1- .Y1 W2  Z=x1W1+x2W2+b XZ- YZ Forward pass: ∂C ∂z ∂C Compute ∂z/∂w for all parameters ∂w ∂w∂z Backwardpass: (Chain rule) Compute ∂C /∂z for all activation function inputs z

Compute ∂z/aw for all parameters W1 Z x1· S- →Y1 [W2 Z = x 1W 1 +x2W2 + b x2- → y2 ∂z/∂w1 =? x1 The value of the input ∂z/∂w2 =? x2 connected by the weight

1 0.98 2 0.86 3  0.12 -2 0.11 -1  [1 -1 ∂z ∂z ∂z  = 0.12  = 0.11  ∂w ∂w ∂w

Compute ∂ C /∂z for all activation function inputs z X1· W2 W4 x2 ∂C ∂a∂C ∂z ∂z∂a σ(z)

1 0.6 σ(z) 0.4 E σ′(z) 0.2 上 o -6 -4 -2 0 2 4 6

∂c ∂a∂c ∂c ∂z''ac ∂z" ∂c + (Chainrule) ∂z ∂z ∂a ∂a ∂aí . ∂z ∂a ∂z 3 4 it' 'skno n

Compute ∂C /∂z for all activation function inputs z x1 ∂c T ∂c ∂z ∂z W2 4 x2- ∂C ∂c ∂c ∂cl ∂z" ∂z W3 ∂z W4 ∂z′

Compute ∂C /∂z for all activation function inputs z x1 W1 z W3 ∂C →y1 1 ∂C ∂z ∂z'' W2 4 x2- →Y2 AC Case 1. Output Layer ∂z'' ∂c ∂y1 c ∂c ∂c ∂yz ∂c Done! ∂z'' ∂z''∂y1 ∂z" ∂z ∂y2

Compute ∂C /∂z for all activation function inputs z ∂C ∂C ∂z ∂za W6 Z 1 26 ∂c ac ∂z ∂zb

Compute ∂C /az for all activation function inputs z Case 2.Not Output Layer Compute ∂C/az recursively [a ∂C ∂C ∂z' '∂z aUntil we reach the AC AC ∂Zb

Compute ∂C /∂z for all activation function inputs z ∂c ac ac ∂z1 ∂z3 ∂z5 Z1 Z5 X1 S Z3 Y 1 x2· y2 Z2 Z4 Z6 ∂c ac ∂c ∂z2 ∂z4 ∂Z6

Com pute ∂ C /az for all activation function inputs z ∂c ac ∂c ∂Z1 ∂Z3 ∂Z5 Z1 Z3 Z5 x1 Y1 x2 +y2 Z2 Z4 Z6 ∂C ∂C ∂C ∂Z2 ∂Z4 ∂Z6

Forward Pass Backward Pass ∂z ∂c ∂c x ∂w ∂z ∂w forallw

SURFO点T VECTOR MACHNES

多层神经网络两层神经网络单层挣经网络 2012 CNN 2020 挣经元第三次 Winter？ 兴 1986 2006 BP DBN 第二次 1982 兴 1995 Hopfield SVM 1958 Perceptron 1969 —次 “Atwinter” 1943 MP 1949 兴运诞生 Hebb 1940 1950 1960 1970 1980 1990 2000 2010 2020

单层神经网络两层神经网络多层神经网络网络结构：  sgn sigmoid ReLu 激活函数：  异或问题： 复问：

单层神经网络两层神经网络多层神经网络 (60-70) (85-95) (2010-) 计算能力： 晶体管 CPU 集群或GPU 这据 ： 1-10 1K-10K 1M-100M 法学习算法 BP算法 Pre-training,
