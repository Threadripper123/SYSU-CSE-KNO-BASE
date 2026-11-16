# Lecture7-机器学习3-深度学习

> 主题库：人工智能　｜　源文件：Lecture7-机器学习3-深度学习.pdf　｜　21,947 字符　｜　149 个片段　｜　更新于 2026-11-11


人工智能
机器学习—深度学习
[Based on Hung-yiLee Deep Learning Lecture]

when you think of the brain...
人脑中的神经网络是一个非常复杂的组织，由很多具有适应
性的简单同构单元组成的广泛并行互连的网络，它的组织能
够模拟生物神经系统对真实世界所作出的交互反应
这一简单的结构就是神经元
成人的大脑中估计有1000亿个神经元
人脑神经网络

神经元－引入
⚫1904年生物学家已经知晓神经元的组成结构
⚫一个神经元通常具有多个树突，主要用来接受传入信息；
而轴突只有一条，轴突尾端有许多轴突末梢可以给其他多个
神经元传递信息。轴突末梢跟其他神经元的树突产生连接，
从而传递信号。这个连接的位置在生物学上叫做“突触”。
神经元结构图

神经元－引入
⚫1943年，心理学家McCulloch和数学家Pitts参考了生物神
经元的结构，发表了抽象的神经元模型MP
Warren McCullochWalter Pitts
McCulloch, W.S. and W. Pitts. (1943), “A logical Calculus of the Ideas Immanent in 
Nervous Activity.” Bulletin of Mathematical Biophysics, 5(4): 115-133. 

神经元－结构
•类比生物结构：输入可以类比为神经元的树突，而输出可以类比为神
经元的轴突，计算则可以类比为细胞核。神经元模型是一个包含输入，
输出与计算功能的模型。
•下图是一个典型的神经元模型：包含有3个输入，1个输出，以及2个
计算功能
•注意中间的箭头线。这些线称为“连接”。每个上有一个“权值”
神经元模型

神经元－结构
将神经元图中的所有变量用符号表示，并且写出输出的计算公式
z是在输入和权值的线性加权和叠加了一个函数g的值
在MP模型里，函数g是Sgn函数，也就是迁跃函数
binary-threshold function。当输入大于0时，输出1，否则输出0

神经元－结构
•对神经元模型的图进行扩展，将sum函数与sgn函数合并到一个圆圈里，
代表神经元的内部计算，把输入a与输出z写到连接线的左上方，便于后
面画复杂的网络
•神经元可以看作一个计算与存储单元。计算是神经元对其的输入进行计
算功能。存储是神经元会暂存计算结果，并传递到下一层。
•用“神经元”组成网络以后，描述网络中的某个“神经元”时，可以用
“单元”（unit）来指代。由于神经网络的表现形式是一个有向图，
也会用“节点”（node）来表达同样意思

神经元－效果
•神经元模型的使用可以这样理解：
•数据样本有四个属性，其中3个属性已知，1个属性未知。通过3个已知属
性预测未知属性
•使用神经元的公式进行计算。3个已知属性的值是a1，a2，a3，未知属
性的值是z
•已知的属性称为特征，未知属性称为目标。假设特征与目标之间是线
性关系，并且我们已经得到表示这个关系的权值w1，w2，w3。那么，
可以通过神经元模型预测新样本的目标

神经元－影响
⚫1943年发布的MP模型，虽然简单，但已经建立了神经网络大厦的地基。
但是，MP模型中，权重的值都是预先设置的，因此不能学习。
⚫1949年心理学家Hebb提出了Hebb学习率，认为人脑神经细胞的突
触（也就是连接）上的强度上可以变化的。于是计算科学家们开始考虑用
调整权值的方法来让机器学习。这为后面的学习算法奠定了基础。
⚫尽管神经元模型与Hebb学习律都已诞生，但限于当时的计算机能力，直
到接近10年后，第一个真正意义的神经网络才诞生。
Donald OldingHebb

单层神经网络（感知器）
•1958年，计算科学家Rosenblatt提出了由两层神经元组成的神经网
络。他给它起了一个名字——“感知器”（Perceptron，有的文献
翻译成“感知机”）
•感知器是当时首个可以学习的人工神经网络。Rosenblatt现场演示了
其学习识别简单图像的过程，在当时的社会引起了轰动
•人们认为已经发现了智能的奥秘，许多学者和科研机构纷纷投入到神
经网络的研究中。美国军方大力资助了神经网络的研究，并认为神经
网络比“原子弹工程”更重要。这段时间直到1969年才结束，这个
时期可以看作神经网络的第一次高潮。
Rosenblat与感知器

感知器－结构
•在原来MP模型的“输入”位置添加神经元节点，标志其为“输入单元”
•在“感知器”中，有两个层次，分别是输入层和输出层。输入层里的“输入单元”
只负责传输数据，不做计算。输出层里的“输出单元”则需要对前面一层的输
入进行计算
•把需要计算的层次称之为“计算层”，并把拥有一个计算层的网络称之为“单层
神经网络”。有一些文献会按照网络拥有的层数来命名，例如把“感知器”称为
两层神经网络
•假如预测目标不是一个值，而是一个向量。可以在输出层再增加一个“输出单
元”
单层神经网络
这两个公式是线性代数方程组，可以用矩阵乘法
来表达这两个公式
输入的变量是[a
1
，a
2
，a
3
]
T
（代表由a
1
，a
2
，
a
3
组成的列向量），用向量a来表示
方程的左边是[z
1
，z
2
]
T
，用向量z来表示
系数则是矩阵W（2行3列的矩阵，排列形式与公式中的
一样）
输出公式可以改写成：g(W*a) =z
这个公式就是神经网络中从前一层计算后一层的运算过程

感知器－训练法则
与神经元模型不同，感知器中的权值是通过训练得到
感知机训练法则：
t  :  是当前训练样例的目标输出
o :  是感知机的输出
η:  是一个正的常数称为学习速率（0.1）
xi : 训练样例

感知器－效果
•感知器类似一个逻辑回归模型，可以做线性分类任务。可以把感知机当做n维空间中
的决策超平面，对于超平面一侧的实例感知器输出1，对于另一侧的实例输出0
•我们可以用决策分界来形象的表达分类的效果。决策分界就是在二维的数据平面中划
出一条直线，当数据的维度是3维的时候，就是划出一个平面，当数据的维度是n维时，
就是划出一个n-1维的超平面。下图显示了在二维平面中划出决策分界的效果，也就是
感知器的分类效果。
单层神经网络（决策分界）

感知器－影响
•感知器只能做简单的线性分类任务。但是当时的人们热情太过于高涨，并没
有人清醒的认识到这点Minsky在1969年出版了一本叫《Perceptron》的书，
里面用详细的数学证明了感知器的弱点，尤其是单层感知器对XOR（异或）
这样的简单分类任务都无法解决
•Minsky认为，如果将计算层增加到两层，没有有效的学习算法（受限于感知
机训练法则）。所以，他认为研究更深层的网络是没有价值的由于Minsky的
巨大影响力以及书中呈现的悲观态度，让很多学者和实验室纷纷放弃了神经
网络的研究。神经网络的研究陷入了冰河期。这个时期又被称为“AI 
winter”接近10年以后，对于两层神经网络的研究才带来神经网络的复苏
Marvin Minsky

两层神经网络（多层感知器）
•单层神经网络无法解决异或问题。但是当增加一个计算层以后，两层
神经网络不仅可以解决异或问题，而且具有非常好的非线性分类效果。
不过两层神经网络的计算是一个问题，没有一个较好的解法
•1986年，Rumelhar和Hinton等人提出了反向传播
（Backpropagation，BP）算法，解决了两层神经网络所需要的
复杂计算量问题，从而带动了业界使用两层神经网络研究的热潮
•这时候的Hinton还很年轻，30年以后，正是他重新定义了神经网络，
带来了神经网络复苏
David Rumelhart
Geoffery Hinton

一个简单的神经网络：多层感知机
•一 个 经 典 的 神 经 网 络 包 含 三 个
层次：红色的是输入层，绿色
的是输出层，紫色的是中间层
（也叫隐藏层）
•输入层与输出层的节点数往
往是固定的，中间层则可以
自由指定
•结构图里的关键不是圆圈
（代表“神经元”），而是
连接线。每个连接线对应一
个不同的权重
神经网络结构图

多层感知器－结构
•假设预测目标是一个向量，只需要在“输出层”再增加节点
•使用向量和矩阵来表示层次中的变量。a
(1)
，a
(2)
，z是网
络中传输的向量数据。W
(1)
和W
(2)
是网络的矩阵参数
•矩阵运算的简洁，也不会受节点数增多影响
g(W
(1)
*a
(1)
) =a
(2)
g(W
(2)
*a
(2)
) =z;
两层神经网络（向量形式）

多层感知器－结构
•一般还会引入偏置单元，他与后一层的所有节点都有连接，我们设这些
参数值为向量b，称之为偏置。如下图。
•偏置节点很好认，因为其没有输入（前一层中没有箭头指向它）。有些
神经网络的结构图中会把偏置节点明显画出来，有些不会。一般情况下，
都不会明确画出偏置节点
两层神经网络（考虑偏置节点）

多层感知器－结构
⚫考虑了偏置后的神经网络的矩阵运算：
g(W
(1)
*a
(1)
+b
(1)
) =a
(2)
;
g(W
(2)
*a
(2)
+b
(2)
) =z;
⚫两层神经网络不再使用Sgn函数作为函数g，而是使用平滑
函数sigmoid作为函数g。函数g也称作激活函数
（activefunction）。
⚫神经网络的本质就是通过矩阵参数W与激活函数g来拟合特
征与目标之间的真实函数关系

多层感知器－结构
⚫考虑了偏置后的神经网络的矩阵运算：
g(W
(1)
*a
(1)
+b
(1)
) =a
(2)
;
g(W
(2)
*a
(2)
+b
(2)
) =z;
⚫两层神经网络不再使用Sgn函数作为函数g，而是使用平滑
函数sigmoid作 为 函 数g。函数g也 称 作 激 活 函 数
（activefunction）。
⚫神经网络的本质就是通过矩阵参数W与激活函数g来拟合特
征与目标之间的真实函数关系

多层感知器－效果
•理论证明，两层神经网络可以无限逼近任意连续函数。面
对复杂的非线性分类任务，两层（带一个隐藏层）神经网
络可以分类的很好
•例子
•红色的线与蓝色的线代表数据
•而红色区域和蓝色区域代表由神经网络
•划开的区域，两者的分界线就是决策分界
两层神经网络（决策分界）
•这个两层神经网络的决策分界是非常平滑的曲
线，而且分类的很好。
•Q：单层网络只能做线性分类任务。而两层神
经网络中的后一层也是线性分类层，应该只能
做线性分类任务。为什么两个线性分类任务结
合就可以做非线性分类任务？

多层感知器－效果
⚫我们可以把输出层的决策分界单独拿出来看一下。就是下图。
⚫输出层的决策分界仍然是直线。关键是，从输入层到隐藏层时，数据
发生了空间变换。也就是说，两层神经网络中，隐藏层对原始的数据进
行了一个空间变换，使其可以被线性分类，然后输出层的决策分界划出
了一个线性分类分界线，对其进行分类
两层神经网络（空间变换）

多层感知器－效果
Neural Network Transformation Visualization

多层感知器－效果
Neural Network Decision Boundary Visualization

多层感知器－效果
https://colab.research.google.com/github/murphyka/ml_colabs/blob/main/Simple_MLP_Visuali
zation.ipynb#scrollTo=HpMdcCE7NoB0
units_per_layer=32
num_layers=2
activation_fn='tanh'
units_per_layer=64
num_layers=2
activation_fn='tanh'

多层感知器－效果

多层感知器－效果
•两层神经网络可以做非线性分类的关键--隐藏层
•矩阵公式中矩阵和向量相乘以及非线性函数sigmod，本质上就是对
向量的坐标空间进行一个变换。因此，隐藏层的作用就是使得数据的
原始坐标空间从线性不可分，转换成了线性可分
•两层神经网络通过两层的线性模型模拟了数据内真实的非线性函数。
因此，多层的神经网络的本质就是复杂函数拟合
•设计一个神经网络时，输入层的节点数需要与特征的维度匹配，输出
层的节点数要与目标的维度匹配。如何决定这个自由层的节点数呢？
一般根据经验来设置
•GridSearch（网格搜索）：预先设定几个可选值，通过切换这几个
值来看整个模型的预测效果，选择效果最好的值作为最终选择

Recap:神经网络
()
z
+
(
)
z

+
()
z
+
(
)
z

+
“Neuron”
神经网络
网络参数휃: “神经元”中的权重(weights)和偏置(biases)

Recap: 神经网络
()
z
z
(
)
z
e
z
−
+
=
1
1

Sigmoid Function
1
-1
1
-2
1
-1
1
0
4
-2
0.98
0.12

Recap: 神经网络
휎
2
y
1
y
1
-2
1
-1
1
0
4
-2
0.98
0.12
1
−1
1−2
−11
+
1
0
0.98
0.12
=
1
-1
4
−2
矩阵操作

Recap: 神经网络
1
-2
1
-1
1
0
4
-2
0.98
0.12
2
-1
-1
-2
3
-1
4
-1
0.86
0.11
0.62
0.83
0
0
-2
2
1
-1

Recap: 神经网络
1
x
2
x
......
N
x
......
......
......
......
......
......
......
y
1
y
2
y
M
W
1
W
2
W
L
b
2
b
L
xa
1
a
2
y
b
1
W
1
x
+
휎
b
2
W
2
a
1
+
휎
b
L
W
L
+
휎
a
L-1
b
1

Recap: 神经网络
=휎
휎
1
x
2
x
......
N
x
......
......
......
......
......
......
......
y
1
y
2
y
M
W
1
W
2
W
L
b
2
b
L
xa
1
a
2
y
y
=푓
x
b
1
W
1
x
+
휎
b
2
W
2
+
b
L
W
L
+
...
b
1
...
使用并行计算技术来
加速矩阵操作

Recap: 神经网络
......
......
......
......
......
......
......
......
y
1
y
2
y
M
K
x
Output 
Layer
Hidden Layers
Input 
Layer
x
1
x
2
x
特征提取器取代特征工程
= 多类别
分类器
Softmax
输出层作为多类别分类器

Recap: 神经网络
1
-2
1
-1
1
0
0.73
0.5
2
-1
-1
-2
3
-1
4
-1
0.72
0.12
0.51
0.85
0
0
-2
2
푓
0
0
=
0.51
0.85
푓
1
−1
=
0.62
0.83
0
0
相当于函数功能
输入一个向量，输出另一个向量
网络结构相当于定义了函数集空间

多层神经网络训练-BP算法
策略梯度

多层神经网络训练-BP算法
链式法则

多层神经网络训练-BP算法
BP算法

多层神经网络训练-BP算法

多层神经网络训练-BP算法
BP算法-前向传递

多层神经网络训练-BP算法
BP算法-前向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法-反向传递

多层神经网络训练-BP算法
BP算法总结

多层感知器－冰河期
90年 代 中 期，由Vapnik等人发明的SVM（SupportVector
Machines，支持向量机）算法诞生，很快就在若干个方面体现出了
对比神经网络的优势：无需调参；高效；全局最优解。基于以上种种
理由，SVM迅速打败了神经网络算法成为主流。
神经网络的研究再次陷入了冰河期。当时，只要你的论文中包含神经
网络相关的字眼，非常容易被会议和期刊拒收，研究界那时对神经网
络的不待见可想而知。
Vladimir Vapnik

深度学习
在被人摒弃的10年中，有几个学者仍然在坚持研究，包括加拿大多伦
多大学的GeofferyHinton教授
2006年，Hinton在《Science》和相关期刊上发表了论文，首次提出
了“深度信念网络”的概念
与传统的训练方式不同，“深度信念网络”有一个“预训练”
（pre-training）的过程，这可以方便的让神经网络中的权值找到一
个接近最优解的值，之后再使用“微调”(fine-tuning)技术来对整
个网络进行优化训练。这两个技术的运用大幅度减少了训练多层神经
网络的时间。他给多层神经网络相关的学习方法赋予了一个新名词--
“深度学习”（深度学习是多个算法的总称）

深度学习
参数越多的模型复杂度越高
–优点：“容量”(capacity)越大，能完成更复杂的任务
–缺点：训练效率低，数据量少时易陷入过拟合
缺点解决方案
–云计算，大数据时代下，计算能力大幅提升，可缓解训练低效性
–训练数据大幅增加，降低过拟合风险
以深度学习为代表的复杂模型开始受人关注

深度学习
很快，深 度 学 习 在 语 音 识 别 领 域 暂 露 头 角。接着，2012年，
Hinton与他的学生在ImageNet竞赛中，用多层的卷积神经网络成
功地对包含一千类别的一百万张图片进行了训练，取得了分类错误
率15%的好成绩，这个成绩比第二名高了近11个百分点，充分证
明了多层神经网络识别效果的优越性
在这之后，关于深度神经网络的研究与应用不断涌现。
Geoffery Hinton
GeofferyHinton
1986，BP算法

深度学习
更强的函数模拟能力是由于随着层数的增加，整个网络的参数就越多。而神经
网络其实本质就是模拟特征与目标之间的真实关系函数的方法，更多的参数意
味着其模拟的函数可以更加的复杂，可以有更多的容量（capcity）去拟合真
正的关系（强悍的非线性函数拟合）
在参数数量一样的情况下，更深的网络往往具有比浅层的网络更好的识别效率
–2012年起，每年获得ImageNet冠军的深度神经网络的层数逐年增加
–2015年最好的方法GoogleNet是一个多达22层的神经网络
–2016拿到最好成绩的MSRA团队的方法使用的更是一个152层的网络！
–2016商汤团队1207层的网络

深度学习
Deep = Many hidden layers
8 layers
19 layers
22 layers
AlexNet(2012)
VGG (2014)
GoogleNet(2014)
16.4%
7.3%
6.7%
http://cs231n.stanford.e
du/slides/winter1516_le
cture8.pdf

深度学习
AlexNet
(2012)
VGG 
(2014)
GoogleNet
(2014)
152 layers
3.57%
Residual Net 
(2015)
Canton Tower
(2009)
112 layers
16.4%
7.3%
6.7%
Deep = Many hidden layers
Special 
structure
Ref: 
https://www.youtube.com/watch?v=
dxB6299gpvI

历史回顾
神经网络发展历程：曲折荡漾，既有被人捧上天的时刻，也有
摔落在街头无人问津的时段，中间经历了数次大起大落
从单层神经网络（感知器）开始，到包含一个隐藏层的两层神
经网络，再到多层的深度神经网络，一共有三次兴起过程
三起三落的神经网络

历史回顾
神经网络为什么能这么火热？简而言之，就是其学习效果的强大。随着
神经网络的发展，其表示性能越来越强
从单层神经网络，到两层神经网络，再到多层神经网络，下图说明了，
随着网络层数的增加，以及激活函数的调整，神经网络所能拟合的决策
分界平面的能力
表示能力不断增强

历史回顾
神经网络发展外在原因：更强的计算性能，更多的数据，以及更好的训
练方法。只有满足这些条件时，神经网络的函数拟合能力才能得已体现
Hinton在2006年的论文里说道的
“...providedthatcomputerswerefastenough,datasetswere
bigenough,andtheinitialweightswerecloseenoughtoagood
solution.Allthreeconditionsarenowsatisfied.”

深度学习
⚫卷积神经网络(Convolution Neural Network, CNN)
⚫循环神经网络(Recurrent Neural Network, RNN)
⚫深度学习的一些研究问题

卷积神经网络(CNN)
为什么CNN 适用于图像
一些局部特征要比整张图片小得多
神经网络不必看完整张图片来识别
“beak”detector

卷积神经网络(CNN)
为什么CNN 适用于图像
相同特征会出现在不同区域
“upper-left 
beak”detector
“middle beak”
detector
它们可以使用
相同的参数
作用几乎相同

卷积神经网络(CNN)
对像素进行二次采样不会改变识别对象信息
subsampling
bird
bird
可以使用二次采样使图片更小
网络可以使用更少的参数来处理图片

卷积神经网络(CNN)
Fully Connected 
Feedforward network
cat dog ......
Convolution
Max Pooling
Convolution
Max Pooling
Flatten
可重复
多次

卷积神经网络(CNN)
Convolution
Max Pooling
Convolution
Max Pooling
Flatten
可重复
多次
➢一些特征会比整张图片
小很多
➢相同特征会出现在不同
区域
➢像素二次采样不会改变
识别的目标信息
Property 1
Property 2
Property 3

卷积神经网络(CNN)
Fully Connected 
Feedforward network
cat dog ......
Convolution
Max Pooling
Convolution
Max Pooling
Flatten
可重复
多次

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
1-1-1
-11-1
-1-11
卷积核1
-11-1
-11-1
-11-1
卷积核2
......
需要学习的网络参数
Matrix
Matrix
每个卷积核可以
识别区域特征(3 x 3). 
Property 1

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
1-1-1
-11-1
-1-11
卷积核1
3-1
stride(步长)=1

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
1-1-1
-11-1
-1-11
卷积核1
3-3
If stride(步长)=2
在下文继续使用stride=1

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
1-1-1
-11-1
-1-11
卷积核1
3-1-3-1
-310-3
-3-301
3
-2-2-1
stride=1
Property 2
相同特征出现在不同区域

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
3-1-3-1
-310-3
-3-301
3-2-2-1
-11-1
-11-1
-11-1
卷积核2
-1-1-1-1
-1-1-21
-1-1-21
-10-43
对每个卷积核做
相同的运算
stride=1
4 x 4 image
特征图

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
100001
010010
001100
100010
010010
001010
100001
010010
001100
100010
010010
001010
1-1-1
-11-1
-1-11
卷积核1
-11-1
-11-1
-11-1
卷积核2
1-1-1
-11-1
-1-11
1-1-1
-11-1
-1-11
-11-1
-11-1
-11-1
-11-1
-11-1
-11-1
RGB 彩色图像

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
image
卷积
-11-1
-11-1
-11-1
1-1-1
-11-1
-1-11
1
x
2
x
......
36
x
......
100001
010010
001100
100010
010010
001010
全连接
卷积
v.s.
全连接

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
1-1-1
-11-1
-1-11
卷积核1
1:
2:
3:
...
7:
8:
9:
...
13:
14:
15:
...
一次只对应9维
输入向量，不用
考虑全部输入
4:
10:
16:
1
0
0
0
0
1
0
0
0
0
1
1
3
更少的参数！

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
1-1-1
-11-1
-1-11
卷积核1
1:
2:
3:
...
7:
8:
9:
...
13:
14:
15:
...
4:
10:
16:
1
0
0
0
0
1
0
0
0
0
1
1
3
-1
共享权重
6 x 6 image
减少更多的参数!

卷积神经网络(CNN)
Fully Connected 
Feedforward network
cat dog ......
Convolution
Max Pooling
Convolution
Max Pooling
Flatten
可以重
复多次

卷积神经网络(CNN)
3-1-3-1
-310-3
-3-301
3-2-2-1
-11-1
-11-1
-11-1
卷积核2
-1-1-1-1
-1-1-21
-1-1-21
-10-43
1-1-1
-11-1
-1-11
卷积核1
Max Pooling (最大池化)

卷积神经网络(CNN)
100001
010010
001100
100010
010010
001010
6 x 6 image
30
13
-1
1
3
0
2 x 2 image
每个卷积核都
会生成一个通
道(channel)
更少的特征
信息
Conv
Max
Pooling
Max Pooling (最大池化)

卷积神经网络(CNN)
Convolution
Max Pooling
Convolution
Max Pooling
可重复
多次
A new image
通道数和卷积核数量相同
比原始图像更小
30
13
-1
1
3
0

卷积神经网络(CNN)
Fully Connected 
Feedforward network
cat dog ......
Convolution
Max Pooling
Convolution
Max Pooling
Flatten
A new image
A new image

卷积神经网络(CNN)
30
13
-11
3
0
Flatten
3
0
1
3
-1
1
0
3
全连接前向网络

循环神经网络(RNN)
为什么使用RNN？
考虑下面的Slot Filling(槽填充)问题：
I would like to arrive Guangzhou on May 31
th
.
ticket booking system
Destination:
Time of arrival:
Guangzhou
May 31
th
Slot
在订票系统中设置几个槽位（Slot），希望算法能够将关键词‘Guangzhou’ 放入目的地
（Destination）槽位，将May 和31th 放入到达时间（Time of Arrival）槽位。

循环神经网络(RNN)
1
x
2
x
2
y
1
y
Guangzhou
输入: 一个单词
(每个单词用一个向量表示)
使用全连接神经网络解决?

循环神经网络(RNN)
每个维度对应一个单词
对应维度的单词为1，其他
位置为0
词典= {apple, bag, cat, dog, elephant}
apple = [ 1   0   0   0   0]
bag    = [ 0   1   0   0   0]
cat    = [ 0   0   1   0   0]
dog   = [ 0   0   0   1   0]
elephant   = [ 0   0   0   0   1]
向量和词典大小相同
One-hot Encoding
如何用向量表示单词?

循环神经网络(RNN)
1
x
2
x
2
y
1
y
Guangzhou
输入: 一个单词
(每个单词用一个向量表示)
使用全连接神经网络解决?
destination
time of 
departure
输出: 
分类标签的概率分布

循环神经网络(RNN)
1
x
2
x
2
y
1
y
Guangzhou
arrive  Guangzhou   on       May       31
th
other
other
dest
time
time
leave  Guangzhouon     May     31
th
place of departure
神经网络需要
记忆功能！
destination
time of 
departure
存在什么问题?

循环神经网络(RNN)
1
x
2
x
2
y
1
y
1
a
2
a
记忆内容可以用作
额外输入信息
隐藏层输出存
储在记忆中
store

循环神经网络(RNN)
1
x
2
x
2
y
1
y
1
a
2
a
store
所有权重为“1”，没有偏置
所有激活函数都为线性
输入序列:
1
1
1
1
2
2
......
1
1
0
0
给定初
始化值
22
4
4
输出序列:
4
4

循环神经网络(RNN)
1
x
2
x
2
y
1
y
1
a
2
a
store
输入序列:
1
1
1
1
2
2
......
1
1
2
2
66
12
12
输出序列:
4
4
12
12

循环神经网络(RNN)
1
x
2
x
2
y
1
y
1
a
2
a
store
输入序列:
1
1
1
1
2
2
......
2
2
6
6
1616
32
32
输出序列:
4
4
12
12
32
32
改变输入序列的顺序将会
对输出产生影响！

循环神经网络(RNN)
storestore
x
1
x
2
x
3
y
1
y
2
y
3
a
1
a
1
a
2
a
2
a
3
相同的网络结构多次使用
arrive  Guangzhouon     May     31
th
“arrive” 对应每个
类别的概率
“Guangzhou” 对应
每个类别的概率
“on” 对应每个
类别的概率

循环神经网络(RNN)
store
x
1
x
2
y
1
y
2
a
1
a
1
a
2
......
......
......
store
x
1
x
2
y
1
y
2
a
1
a
1
a
2
......
......
......
leave
Guangzhou
“leave” 对应每
个类别的概率
“Guangzhou” 对
应类别概率
“arrive” 对应每
个类别的概率
“Guangzhou”对
应类别概率
arrive
Guangzhou
概率分布不同
记忆中存储的信息是不同的

循环神经网络(RNN)
当然，循环结构可以构造得很深...
......
......
x
t
x
t+1
x
t+2
......
......
y
t
......
......
y
t+1
......
y
t+2
......
......

循环神经网络(RNN) ——LSTM
Memory
Cell
Input Gate
Output Gate
输入门
控制信号
输出门
控制信号
Forget 
Gate
遗忘门
控制信号
LSTM
特殊神经元:
4 项输入, 
1 项输出
长短期记忆网络(Long Short-term Memory)

循环神经网络(RNN) ——LSTM
푧
푧
푖
푧
푓
푧
표
푔푧
푓푧
푖
multiply
multiply
通常使用sigmoid 激活函数
限制在0 到1 之间
模拟门的开关
c
푐
′
=푔푧푓푧
푖
+푐푓푧
푓
ℎ푐
′
푓푧
표
푎
=ℎ푐
′
푓푧
표
푔푧푓푧
푖
푐
′
푓푧
푓
푐푓푧
푓
푐

循环神经网络(RNN) ——LSTM
1
0
0
0
푥
1
푥
2
푥
3
푦
3
1
0
0
2
0
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
6
1
0
0
1
0
1
6
当x
2
= 1, 把x
1
的值累加到记忆中
当x
3
= 1, 输出记忆的值
00337
77
0
6
当x
2
= -1, 重置记忆
LSTM 示例

循环神经网络(RNN) ——LSTM
푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
x
1
x
2
x
3
x
1
x
2
x
3
x
1
x
2
x
3
x
1
x
2
x
3
y
1
0
0
100
0
0
1
1
1
1
0
-10
0
0
100
-10
0
0
10
100
0

푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
310
3
1
0
3
1
0
3
1
0
y
1
0
0
100
0
0
1
1
1
1
0
-10
0
0
100
-10
0
0
10
100
3
≈1
3
≈1
0
3
3
≈0
≈0

푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
410
4
1
0
4
1
0
4
1
0
y
1
1
1
1
4
≈1
4
≈1
3
7
7
≈0
≈0
1
0
0
100
0
0
0
-10
0
0
100
-10
0
0
10
100

푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
200
2
0
0
2
0
0
2
0
0
y
1
1
1
1
2
≈0
0
≈1
7
7
≈0
≈0
1
0
0
100
0
0
0
-10
0
0
100
-10
0
0
10
100

푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
101
1
0
1
1
0
1
1
0
1
y
1
1
1
1
1
≈0
0
≈1
7
7
≈1
≈7
1
0
0
100
0
0
0
-10
0
0
100
-10
0
0
10
100

푥
1
푥
2
푥
3
푦
3
1
0
0
4
1
0
0
2
0
0
0
1
0
1
7
3
-1
0
0
+
+
+
+
3-10
3
-1
0
3
-1
0
3
-1
0
y
1
1
1
1
3
≈0
0
≈0
7
0
≈0
≈0
0
1
0
0
100
0
0
0
-10
0
0
100
-10
0
0
10
100

循环神经网络(RNN) ——LSTM
x
1
x
2
Input
网络结构:
➢用LSTM 替换原始神经元
푎
1
푎
2
...
...
푧
1
푧
2

循环神经网络(RNN) ——LSTM
x
1
x
2
+
+
+
+
+
+
+
+
Input
푎
1
푎
2
4 倍参数量

循环神经网络(RNN) ——LSTM
c
t-1
......
vector
x
t
zz
i
z
f
z
o
4 vectors

循环神经网络(RNN)
copycopy
x
1
x
2
x
3
y
1
y
2
y
3
W
i
a
1
a
1
a
2
a
2
a
3
arrive  Guangzhouon     May31
th
训练语句：
学
习
目
标
other
other
dest
1
0
0
1
0
0
1
0
0
other
dest
other
...
...
...
...
...
...
time
time

循环神经网络(RNN) 
学习过程
Backpropagation 
through time (BPTT)
푤←푤−휂휕퐿∕휕푤
1
x
2
x
2
y
1
y
1
a
2
a
copy
푤

循环神经网络(RNN) 
但是...
语言模型的真实案例
Lucky
sometimes
Total Loss
Epoch
基于RNN 的网络并不总是容易学习的

循环神经网络(RNN) 
误差表面相当粗糙
w
1
w
2
Cost
误差表面经常相当平坦
或者相当陡峭.
Clipping 
[Razvan Pascanu, ICML’13]
Total Loss

循环神经网络(RNN)
Why? 
1
1
y
1
0
1
w
y
2
0
1
w
y
3
0
1
w
y
1000
......
푤=1
푤=1.01
푦
1000
=1
푦
1000
≈20000
푤=0.99
푤=0.01
푦
1000
≈0
푦
1000
≈0
11
11
Large
Τ
휕퐿휕푤
Small 
Learning rate?
small 
Τ
휕퐿휕푤
Large 
Learning rate?
Toy Example
=w
999

深度学习研究的子问题
•Attention
•Transformer
•BERT
•VAE
•GAN
•Attack
•Explainability
•.....

Attention 
FCFCFCFC
Self-attention
with 
context

Attention 
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
relevant?
훼
Find the relevant vectors in a sequence 
풃
ퟏ

Attention 
풒
ퟏ
풌
ퟐ
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
풒
ퟏ
=푊
푞
풂
ퟏ
풌
ퟑ
풌
ퟒ
풌
ퟐ
=푊
푘
풂
ퟐ
풌
ퟑ
=푊
푘
풂
ퟑ
풌
ퟒ
=푊
푘
풂
ퟒ
훼
1,2
훼
1,3
훼
1,4
훼
1,2
=풒
ퟏ
∙풌
ퟐ
query
key
attention score
훼
1,3
=풒
ퟏ
∙풌
ퟑ
훼
1,4
=풒
ퟏ
∙풌
ퟒ

Attention 
훼
1,1
′
훼
1,2
′
훼
1,3
′
훼
1,4
′
풌
ퟏ
풒
ퟏ
풗
ퟏ
풌
ퟐ
풗
ퟐ
풌
ퟑ
풗
ퟑ
풌
ퟒ
풗
ퟒ
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
풃
ퟏ
풃
ퟏ
=෍
푖
훼
1,푖
′
풗
풊
Extract information based 
on attention scores
풗
ퟏ
=푊
푣
풂
ퟏ
풗
ퟐ
=푊
푣
풂
ퟐ
풗
ퟑ
=푊
푣
풂
ퟑ
풗
ퟒ
=푊
푣
풂
ퟒ

Attention 
풗
ퟏ
풌
ퟏ
풒
ퟏ
풗
ퟐ
풌
ퟐ
풒
ퟐ
풗
ퟑ
풌
ퟑ
풗
ퟒ
풌
ퟒ
훼
2,1
′
훼
2,2
′
훼
2,3
′
훼
2,4
′
풃
ퟐ
풃
ퟐ
=෍
푖
훼
2,푖
′
풗
풊
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
풒
ퟒ
풒
ퟑ

Attention 
Self-attention
풗
ퟏ
풌
ퟏ
풒
ퟏ
풗
ퟐ
풌
ퟐ
풒
ퟐ
풗
ퟑ
풌
ퟑ
풒
ퟑ
풗
ퟒ
풌
ퟒ
풒
ퟒ
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
풒
풊
=푊
푞
풂
풊
풌
풊
=푊
푘
풂
풊
풗
풊
=푊
푣
풂
풊
풒
ퟏ
풒
ퟐ
풒
ퟑ
풒
ퟒ
=
푊
푞
풂
ퟏ
풂
ퟐ
풂
ퟑ
풂
ퟒ
=
푊
푘
=
푊
푣
풂
ퟏ
풂
ퟐ
풂
ퟑ
풂
ퟒ
풂
ퟏ
풂
ퟐ
풂
ퟑ
풂
ퟒ
풗
ퟏ
풗
ퟑ
풗
ퟒ
풗
ퟐ
풌
ퟏ
풌
ퟑ
풌
ퟒ
풌
ퟐ
I
I
I
푄
퐾
푉

Attention 
Self-attention
=
푊
푞
=
푊
푘
=
푊
푣
Q
K
V
퐾
푇
Q
A
A′
A′
V
=
I
O
=
I
I
Attention Matrix
Parameters 
to be learned 

Attention 
Multi-head Self-attention
풌
풊
풗
풊
풂
풊
풒
풊
(2 heads as example)
풒
풊,ퟐ
풒
풊,ퟏ
풌
풊,ퟐ
풌
풊,ퟏ
풗
풊,ퟐ
풗
풊,ퟏ
풌
풋
풗
풋
풂
풋
풒
풋
풒
풋,ퟐ
풒
풋,ퟏ
풌
풋,ퟐ
풌
풋,ퟏ
풗
풋,ퟐ
풗
풋,ퟏ
풃
풊,ퟏ
풒
풊
=푊
푞
풂
풊
풒
풊,ퟏ
=푊
푞,1
풒
풊
풒
풊,ퟐ
=푊
푞,2
풒
풊
Different types of relevance 

Transformer
Speech
Recognition
N
T
Input a sequence, output a sequence 
Machine
Translation
N’
N
人工智能
machine 
learning
Sequence-to-sequence (Seq2seq)
Speech
Translation
人工智能
machine 
learning
你好吗
Language without text

Transformer
Seq2seq 
Encoder
Decoder
output sequence 
https://arxiv.org/abs/1706.03762
Sequence to Sequence Learning with 
Neural Networks
https://arxiv.org/abs/1409.3215
Transformer 

Transformer
Encoder
풉
ퟒ
풉
ퟑ
풉
ퟐ
풉
ퟏ
풙
ퟒ
풙
ퟑ
풙
ퟐ
풙
ퟏ
Transformer’s Encoder 
You can use RNNor CNN.
Encoder

Transformer
Decoder
Decoder
Encoder
START
智
0.0
人
0.8
工
0.0
能
0.1
............
Size V
(common 
characters)
softmax
distribution
(人工智能)
max
(Speech Recognition as example)
人
(special token)

Transformer
工
max
智
max
能
Encoder
START
(人工智能)
max
人
人
工
智
Decoder
max

Transformer
Encoder
Decoder

Attention (Transformer)
풗
ퟏ
풌
ퟏ
풒
ퟏ
풗
ퟐ
풌
ퟐ
풒
ퟐ
풗
ퟑ
풌
ퟑ
풗
ퟒ
풌
ퟒ
Self-attention
훼
2,1
′
훼
2,2
′
훼
2,3
′
훼
2,4
′
풃
ퟐ
풂
ퟒ
풂
ퟑ
풂
ퟐ
풂
ퟏ
풒
ퟒ
풒
ퟑ
Why masked?
Masked Self-attention
Consider how does decoder work

Transformer
Cross 
attention

Transformer
Encoder
<BOS>
minimize cross entropy 
人
人
工
智
工智能
Label: 人工智能
能
<EOS>
Decoder
Ground Truth
Teacher Forcing

BERT (Bidirectional Encoder Representations from Transformers)
Source of image: https://huaban.com/pins/1714071707/
ELMO 
(94M)
BERT 
(340M)
GPT-2 
(1542M)
The models become larger 
and larger ...

BERT (Bidirectional Encoder Representations from Transformers)
Megatron (8B)
GPT-2
TuringNLG (17B)
The models become larger 
and larger ...
GPT-3 is 10times larger than 
Turing NLG.

BERT (Bidirectional Encoder Representations from Transformers)
Self-supervised Learning
Supervised 
푥
푦
label
Model
ො푦
푥
푥′
푥′′
Model
Self-
supervised 
푦

BERT
Masking Input
BERT
中
MASK
Random
(special 
token)
https://arxiv.org/abs/1810.04805
山
大
学
Transformer 
Encoder
Linear
学
0.1
山
0.7
中
0.1
大
0.1
............
(all characters)
=
=
or
Randomly masking 
some tokens
一、天、大、小...
softmax

BERT
Masking Input
BERT
中
MASK
Random
(special 
token)
https://arxiv.org/abs/1810.04805
灣
大
学
Transformer 
Encoder
Linear
=
=
or
Randomly masking 
some tokens
一、天、大、小...
softmax
山
Ground 
truth 
minimize cross 
entropy 

BERT
Next Sentence Prediction 
BERT
[SEP]
Yes/No
[CLS]
Linear
Robustly optimized BERT approach 
(RoBERTa)
w
1
w
2
Sentence 1
w
3
w
4
w
5
Sentence 2
•This approach is not helpful.
•SOP: Sentence order prediction
Used in ALBERT 
https://arxiv.org/abs/1909.11942

BERT
•Masked token prediction
•Nextsentence prediction 
BERT
Self-supervised 
Learning
Model for 
Task 1
Downstream Tasks 
Model for 
Task 2
Model for 
Task 3
•The tasks we care 
•We have a little bit labeled data.
Fine-tune
Pre-train

BERT
BERT and its Family 
GLUE scores
Source of image: https://arxiv.org/abs/1905.00537

VAE
Auto-encoder
As close as possible
NN
Encoder
NN
Decoder
vector
Embedding, Representation, Code
low dim (bottleneck)
old feature 
New feature for downstream tasks 
(reconstruction)
high dim
Dimension reduction

VAE
Why Auto-encoder? 
NN
Encoder
NN
Decoder
As close as possible
(reconstruction)
3 x 3
3 x 3
2 dim
Find pattern, and 
then compress

VAE
De-noising Auto-encoder
As close as possible
NN
Encoder
NN
Decoder
vector
Vincent, Pascal, et al. "Extracting and composing robust features 
with denoising autoencoders."ICML, 2008.
Add noises

深度学习的可解释性
分类器
这是“猫”
Local Explanation 
Global Explanation 
你认为“猫”应该是什么样子？
为什么你认为这张图片是猫？
因为...

深度学习的可解释性
为什么我们需要可解释的机器学习？
用机器来协助判断简历
具体能力？还是性别？
用机器来协助判断犯人是否可以假释
具体证据？还是肤色？
金融相关的决策常常依法需要提供理由
为什麽拒绝了某個人的贷款？
模型诊断：到底机器学到了什么
不能只看正确率？想想神马汉斯的故事

深度学习的可解释性
机器学习可解释性的目标≠完全清楚机器学习模型
的工作机理
•人脑也是一个黑盒模型!
•人类不相信深度学习因为它是黑盒，但他们却相信其他人类的
决策！

深度学习的可解释性
可解释性v.s. 功能强大
➢某些模型本质上是可解释的
•例如，线性模型(可以从权重知道特征的重要性)
•但是，这种模型功能并不强大.
➢深度神经网络很难解释
•深度神经网络是黑盒
•但它功能远强大于线性模型...
因为深度网络是黑盒，
所以我们不用它。
= 削足适履
Let’s make deep network interpretable. 

深度学习的可解释性
Local Explanation:
Explain the Decision
Questions: Why do you think this image 
is a cat?

深度学习的可解释性
Basic Idea
Object 푥
푥
1
,⋯,푥
푛
,⋯,푥
푁
Image: pixel, segment, etc.
Text: a word
Components:
我们想知道每个部分对于决策的重要程度
Idea: 移除或改变该部分的值，观察决策结果是否变化
决策变化较大
该部分很重要

深度学习的可解释性
푥
1
,⋯,푥
푛
,⋯,푥
푁
푥
1
,⋯,푥
푛
+∆푥,⋯,푥
푁
푦
푘
: the prob of the predicted class 
of the model
푦
푘
+∆푦푦
푘
|
∆푦
∆푥
|
|
휕푦
푘
휕푥
푛
|
特征图

Endless to explore ...
Thanks
