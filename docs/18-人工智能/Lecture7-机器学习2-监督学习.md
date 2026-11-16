# Lecture7-机器学习2-监督学习

> 主题库：人工智能　｜　源文件：Lecture7-机器学习2-监督学习.pdf　｜　27,886 字符　｜　174 个片段　｜　更新于 2026-11-11


人工智能
机器学习—监督学习

机器学习发展史

机器学习发展史

机器学习类型

机器学习背景知识
•数学基础
✓高等数学：导数、微分、泰勒公式...
✓线性代数：向量、矩阵...
✓概率论与数理统计：概率基本公式、常见分布、期望、协方差...
•Python基础
✓环境安装：Anaconda、JupyterNotebook、Pycharm、VSCode
✓Python基础
✓Python库：numpy、pandas、scipy、matplotlib、scikit-learn...

机器学习背景知识
•数学基础
✓高等数学：导数、微分、泰勒公式...
✓线性代数：向量、矩阵...
✓概率论与数理统计：概率基本公式、常见分布、期望、协方差...
•Python基础
✓环境安装：Anaconda、JupyterNotebook、Pycharm、VSCode
✓Python基础
✓Python库：numpy、pandas、scipy、matplotlib、scikit-learn...

模型—representation
机器学习首先要考虑使用什么样的模型。
模型的类别，大致有两种：一是概率模型(Probabilistic Model)和非概率模型(Non-
ProbabilisticModel)。
决策树、朴素贝叶斯、隐马尔科夫模型、高斯混合模型属于概率模型。
感知机、支持向量机、KNN、AdaBoost、K-means以及神经网络均属于非概率模型。
直观理解：拟合数据的分布函数
假设数据的特征是，
我们需要找到一个函数能表示所有数据：
例如，猫的特征是X=(10, 20, 30),
如何学习这些权重?
度量预测的和真实的之间的差异，即计算loss值，通过使得loss最小化使得差异最
小化
12
(   ,    ,...,
)
n
X
x   x
x
=
1   1
2    2
y
...
nn
WX
w x
w x
w x
=    =    +    +  +
1   1
2    2
3   3
1
2
3
0
y
10
20
30
1
WX
w x
w x
w x
w
w
w

=    =    +
+    =    +    +    =


12
(    ,
,...,
)
n
W
w  w
w
=
y
y

损失函数loss—evaluation
1. 0-1损失函数(0-1Loss Function)
2.平方损失函数(QuadraticLossFunction)
3.绝对损失函数(AbsoluteLossFunction)
4.对数损失函数(LogarithmicLossFunction)

损失函数
根据上述损失函数模型，我们可知，损失函数值越小，模型性能越好。给
定一个数据集，我们将训练数据集的平均损失称为经验风险。基于经验风
险最小化原则，可构建全局损失函数求解最优化问题：
当样本数量足够大时，根据大数定理，经验风险会近似于模型的期望风险。
此时，经验风险最小化能确保有好的学习性能。然而，当样本数量不足时，
单单利用经验风险最小化可能会导致“过拟合”的问题。为此，我们再
原有基础上加上用于控制模型复杂度的正则项(Regularizer)，得到结构
最小化准则。

优化算法-optimization
如何使得loss最小化？
算法指的是模型学习中的具体计算方法。一般来说，基于参数模型构建的
统计学习问题都为最优化问题，它们都具有显式的解析解。
现有的优化方法主要有：梯度下降法、牛顿法、拟牛顿法、ADAM等等。
梯度下降法：一阶导数为0的点，不断下降寻找极小值

模型评估

机器学习的开发流程
机器学习
深度学习
数据搜集数据清洗特征工程数据建模

分类和回归
人工智能课程

分类和回归
符号定义

分类和回归
•线性分类和回归是一种通过属性的线性组合来进行预测的线性模
型，其目的是找到一条直线或者一个平面或者更高维的超平面，使
得预测值与真实值之间的误差最小化。

分类和回归

分类和回归

分类和回归
梯度下降
学习率
훼
步长

分类和回归
•批量梯度下降（BatchGradientDescent,BGD）
•梯度下降的每一步中，都用到了所有的训练样本
•随机梯度下降（StochasticGradientDescent,SGD）
•梯度下降的每一步中，用到一个样本，在每一次计算之后便更新参
数，而不需要首先将所有的训练集求和
•小批量梯度下降（Mini-BatchGradientDescent,MBGD）
•梯度下降的每一步中，用到了一定批量的训练样本

分类和回归
•归一化：数据归一化的目的是使
得各特征对目标变量的影响一
致，会将特征数据进行伸缩变
化，所以数据归一化是会改变特
征数据分布的。
•将数据映射到[0,1]区间
•Z-Score：数据标准化为了不
同特征之间具备可比性，经过
标准化变换之后的特征数据分
布没有发生改变。
•就是当数据特征取值范围或单位
差异较大时，最好是做一下标准
化处理
•处理后的数据均值为0，方差为1
数据标准化和归一化

分类和回归
푤
1
푤
2
梯度
푤
1
푤
2
梯度
•提升模型精度：不同维度之间的
特征在数值上有一定比较性，可
以大大提高分类器的准确性
•加速模型收敛：最优解的寻优过
程明显会变得平缓，更容易正确
的收敛到最优解
数据标准化和归一化

逻辑回归（分类问题）
휎(푧)代表一个常用的逻辑函数（logistic function）为푆形函数
（Sigmoid function)
1
(  )
(  )
,
1
T
z
z
g  z
z
w  x    b
e

−
=
=
=    +
+
(  )
(  )(1
(  ))
z
z
z




=−
✓휎(푧)大于等于0.5时，预测y=1
✓휎푧小于0.5时，预测y=0

逻辑回归（分类问题）
逻辑回归求解
✓假设一个二分类模型：
✓逻辑回归模型的假设是：
✓逻辑函数（logistic function）公式为
✓损失函数
1
(   |   ;   )    (  (  ))  (1
(  ))
yy
p  y   x  w
h  x
h  x
−
=−
(  )
(
)
(  )
T
h  x
g  w  x
g  z
==
1
(  ),    (  )(  )(1(  ))
1
z
g  zg   zg  zg  z
e
−

==−
+
ˆ
ˆ
ˆ
L(  ,   )
log(  )   (1
) log(1
)
y  y
y
y
y
y
= −
−  −
−
p(y=1 | x ; w)=h(x)
p(y=0 | x ; w)=1-h(x)

逻辑回归（分类问题）
逻辑回归求解
✓损失函数
✓梯度下降
(  )
(  )
(  )
(  )
1
11
ˆ
ˆ
(   )
(   )
(
log(
)    (1
) log(1
))
m
i
i
i
i
i
J  w
l  w
y
y
y
y
mm
=
= −
= −
+  −
−

()
(  )
(  )
(  )
(  )
1
ˆ
(
)
,
1
i
i
i
i
T
i
z
y
z
z
w  x
b
e

−
=
=
=
+
+
()
:
jj
w
w
w
w
J

−

=

(  )
(  )
(  )
1
1
ˆ
(   )
(
)
m
i
i
i
j
i
j
J  w
y
y
x
wm
=

=−



Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML ——Model Bias
푦=푏+෍
푗=1
56
푤
푗
푥
푗
푦=푏+푤푥
1
푦=푏+෍
푖
푐
푖
푠푖푔푚표푖푑푏
푖
+෍
푗
푤
푖푗
푥
푗
More 
features
Deep Learning 
(more neurons, layers)
푓
∗
풙
푓
휽
ퟐ
풙
푓
휽
ퟏ
풙
푓
휽
∗
풙
too small ...
Model Bias 
small loss
find a needle in a haystack ...
... but there is no needle 
푦=푓
휽
풙
•The model is too simple
•Solution: redesign your model to make it more flexible 

Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML ——Optimization
Optimization Issue
•Large loss not always imply model bias. There is 
another possibility ...
퐿
퐿휽
∗
휽
휽
∗
large
푓
∗
풙
푓
휽
ퟐ
풙
푓
휽
ퟏ
풙
푓
휽
∗
풙
Aneedle is in a haystack ...
... Just cannot find it.
푦=푓
휽
풙

Framework of ML ——Optimization
푓
∗
풙
푓
휽
ퟐ
풙
푓
휽
ퟏ
풙
푓
휽
∗
풙
Aneedle is in a haystack ...
... Just cannot find 
it.
푦=푓
휽
풙
푓
∗
풙
푓
휽
ퟐ
풙
푓
휽
ퟏ
풙
푓
휽
∗
풙
too small ...
small loss
find a needle in a haystack ...
... but there is no needle 
Optimization 
Issue
Model Bias 
Which one???

Framework of ML ——Optimization
Model Bias v.s. Optimization Issue
•Gaining the insights from comparison 
Testing Data
Overfitting?
Training Data
Optimization issue
Ref: http://arxiv.org/abs/1512.03385

Framework of ML ——Optimization
Optimization Issue
•Gaining the insights from comparison 
•Start from shallower networks (or other 
models), which are easier to optimize. 
•If deeper networks do not obtain smaller loss 
on training data,  then there is optimization 
issue. 
•Solution: More powerful optimization 
technology 
1 layer2 layer3 layer4 layer5 layer
2017 –
2020
0.28k0.18k0.14k0.10k0.34k

Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML ——Overfitting
•Small loss on training data, large loss on testing data. 
Why?
An extreme 
example
Training 
data:
풙
ퟏ
,ො푦
1
,풙
ퟐ
,ො푦
2
,...,풙
푵
,ො푦
푁
푓풙=ቊ
ො푦
푖
푟푎푛푑표푚
∃풙
풊
=풙
표푡ℎ푒푟푤푖푠푒
This function obtains zero training loss, but large 
testing loss.
Less than 
useless ...

Framework of ML ——Overfitting
푥
푦
푥
푦
푥
푦
“freestyle”
Real data distribution 
(not observable)
Training data
Testing data
Flexible 
model
Large loss

Framework of ML ——Overfitting
푥
푦
푥
푦
Flexible 
model
More training 
data
Data augmentation

Framework of ML ——Overfitting
푥
푦
푥
푦
Real data distribution 
(not observable)
Training data
Testing data
constrained 
model
푦=푎+푏푥+푐푥
2

Framework of ML ——Overfitting
푥
푦
푥
푦
푥
푦
Real data distribution 
(not observable)
Training data
Testing data
constrained 
model
푦=푎+푏푥+푐푥
2

Framework of ML ——Overfitting
푥
푦
•Less features
•Early stopping  
•Regularization
•Dropout 
•Less parameters, sharing parameters 
CNN
Fully-
connected
푥
푦
constrained 
model
푦=푎+푏푥+푐푥
2

Framework of ML ——Overfitting
푥
푦
푥
푦
푥
푦
Real data distribution 
(not observable)
Training data
Testing data
constrain 
too much
푦=푎+푏푥
Back to model 
bias ...

Framework of ML ——Overfitting
Bias-Complexity Trade-off
loss
Model becomes complex 
(e.g. more features, more parameters)
Training loss
Testing loss
select this 
one

Framework of ML ——Overfitting
Training SetTesting Set
Testing Set
public
private
Training 
Set
Validation
set
Model 1
Model 2
Model 3
mse= 0.9
mse= 0.7
mse= 0.5
mse> 0.5
mse> 0.5
Using the results of public testing 
data to selectyour model
You are making public set 
better than private set.
Not recommend
How to split?
Cross Validation

Framework of ML ——Overfitting
N-fold Cross Validation
Training Set
TrainTrainVal
TrainValTrain
ValTrainTrain
Model 1
Model 2Model 3
mse= 0.4
mse= 
0.5
mse= 0.3
mse= 0.4
mse= 0.5
mse= 0.6
mse= 0.2
mse= 0.4
mse= 0.3
Avg mse
= 0.4
Avg mse
= 0.5
Avg mse
= 0.3
Testing Set
Testing Set
public
private

Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML
loss on training data
largesmall
model 
bias
optimization 
make your 
model 
complex
More powerful 
optimization 
technology 
loss on testing data
overfittingmismatch
small
large
trade-off
General 
Guide
Transfer 
learning
make your model simpler
more training data
data augmentation

Framework of ML ——Mismatch
•Your training and testing data have different 
distributions. 
Training Data
Testing Data
Simply increasing the training data will not 
help.
Be aware of how data is 
generated. 

Small Gradient
local minima
Optimization Fails because ......
updates
trainin
g
loss
Not small 
enough
gradient is close 
to zero
saddle point
critical point
Which one?
No way to go
escape

Small Gradient
Saddle Point v.s. Local Minima 
When you have lots of parameters, perhaps local minima is 
rare?
Saddle point in 
higher dimension?

Small Gradient
Loss
The value of a network parameter w
Very slow at the 
plateau
Stuck at local minima
휕퐿∕휕푤
=0
Stuck at saddle point
휕퐿∕휕푤
=0
휕퐿∕휕푤
≈0
Gradient Descent

Small Gradient ——Tips1: Batch
Small Batch v.s. Large Batch
Batch size = N (Full batch)
See all 
examples
See all 
examples
See only one 
example
Update after seeing all 
the 20 examples
Update 20 times in an epoch
Update for each example
Consider 20 examples (N=20)
Batch size = 1
Long time for cooldown, 
but powerful
Short time for cooldown, 
but noisy

Small Gradient ——Tips1: Batch
Small Batch v.s. Large Batch
SmallLarge
Speed for one 
update 
(no parallel)
FasterSlower
Speed for one 
update 
(with parallel)
Same
Same (not too 
large)
Time for one epoch SlowerFaster
Gradient NoisyStable
OptimizationBetterWorse
Generalization BetterWorse
Batch size is a hyperparameter you have to decide.

Small Gradient ——Tips2: Momentum
Loss
The value of a network parameter w
Consider the physical world ...
How about put this phenomenon 
in gradient descent?

Small Gradient ——Tips2: Momentum
Movement = 
Negative of 휕퐿∕휕푤+ Last Movement 
Gradient Descent + Momentum
loss
휕퐿∕휕푤= 0
Negative of 휕퐿∕휕푤
Last Movement
Real Movement

Optimizer
•Error surface is rugged: Adaptive Learning Rate
•People believe training stuck because the 
parameters are around a critical point ...
loss
norm of 
gradient
53

Optimizer ——RMS
Root Mean Square
휎
푖
0
=품
푖
ퟎ
2
휎
푖
1
=
1
2
품
푖
ퟎ
2
+품
푖
ퟏ
2
휎
푖
푡
=
1
푡+1
෍
푖=0
푡
품
푖
풕
2
휎
푖
2
=
1
3
품
푖
ퟎ
2
+품
푖
ퟏ
2
+품
푖
ퟐ
2
휽
푖
ퟏ
←휽
푖
ퟎ
−
휂
휎
푖
0
품
푖
ퟎ
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
휽
푖
ퟐ
←휽
푖
ퟏ
−
휂
휎
푖
1
품
푖
ퟏ
휽
푖
ퟑ
←휽
푖
ퟐ
−
휂
휎
푖
2
품
푖
ퟐ
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
......
=품
푖
ퟎ

Optimizer ——RMS
휎
푖
푡
=
1
푡+1
෍
푖=0
푡
품
푖
풕
2
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
휽
1
휽
2
품
1
풕−ퟏ
품
1
풕
품
2
풕−ퟏ
품
2
풕
smaller
휎
1
푡
larger
휎
2
푡
larger step
smaller 
step
Used in Adagrad

Optimizer
Learning rate adapts dynamically
푤
1
푤
2
Error Surface can be very complex.
Larger Learning 
Rate
Smaller 
Learning Rate

Optimizer ——RMSProp
휎
푖
0
=품
푖
ퟎ
2
휎
푖
1
=훼휎
푖
0
2
+1−훼품
푖
ퟏ
2
휎
푖
푡
=훼휎
푖
푡−1
2
+1−훼품
푖
풕
2
휎
푖
2
=훼휎
푖
1
2
+1−훼품
푖
ퟐ
2
휽
푖
ퟏ
←휽
푖
0
−
휂
휎
푖
0
품
푖
ퟎ
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
휽
푖
ퟐ
←휽
푖
1
−
휂
휎
푖
1
품
푖
ퟏ
휽
푖
3
←휽
푖
2
−
휂
휎
푖
2
품
푖
ퟐ
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
......
0<훼<1

Optimizer ——RMSProp
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
휎
푖
푡
품
푖
풕
increase 휎
푖
푡
decrease 
휎
푖
푡
smaller 
step
larger step
휎
푖
푡
=훼휎
푖
푡−1
2
+1−훼품
푖
풕
2
0<훼<1
The recent gradient has larger 
influence, and the past gradients have 
less influence.
small 휎
푖
푡
larger step
품
푖
ퟏ
품
푖
ퟐ
...
...
품
푖
풕−ퟏ

Optimizer ——Adam
Adam:RMSProp+ Momentum 
for momentum
for RMSprop
Originalpaper: https://arxiv.org/pdf/1412.6980.pdf

Optimizer ——Summary
휽
푖
풕+ퟏ
←휽
푖
풕
−
휂
푡
휎
푖
푡
풎
푖
풕
Learning rate 
scheduling 
root mean square of the gradients
Momentum: weighted sum of 
the previous gradients 
휽
푖
풕+ퟏ
←휽
푖
풕
−휂품
푖
풕
(Vanilla) Gradient Descent 
Various Improvements
Consider 
direction
only magnitude 

决策树模型
人工智能课程
[1]周志华. 机器学习[M]. 清华大学出版社, 2016.

决策树模型
每个“内部结点”对应于某个属性上的“测试”(test)
每个分支对应于该测试的一种可能结果（即该属性的某个取值）
每个“叶结点”对应于一个“预测结果”
决策树基于“树”结构进行决策
➢学习过程：通过对训练样本的分析来
确定“划分属性”（即内部结点所对
应的属性）
➢预测过程：将测试示例从根结点开始，
沿着划分属性所构成的“判定测试序
列”下行，直到叶结点
西瓜问题的一颗决策树

决策树简史
第一个决策树算法：CLS (Concept Learning System)
[E. B. Hunt, J. Marin, and P. T. Stone’s book “Experiments in Induction” published by 
Academic Press in 1966]
使决策树受到关注、成为机器学习主流技术的算法：ID3
[J. R. Quinlan’s paper in a book “Expert Systems in the Micro Electronic Age” edited by 
D. Michie, published by Edinburgh University Press in 1979]
最常用的决策树算法：C4.5
[J. R. Quinlan’s book “C4.5: Programs for Machine Learning” published by Morgan 
Kaufmann in 1993]
可以用于回归任务的决策树算法：CART ( Classification and Regression Tree )[L. 
Breiman, J. H. Friedman, R. A. Olshen, and C. J. Stone’s book “Classification and 
Regression Trees” published by Wadsworth in 1984]
基于决策树的最强大算法：RF ( Random Forest )L. Breiman’sMLJ’01 paper 
“Random Forest”]

基本流程
策略：“分而治之”( divide-and-conquer )
自根至叶的递归过程；
在每个中间结点寻找一个“划分”( split or test ) 属性。
三种停止条件：
1.当前结点包含的样本全属于同一类别，无需划分;
2.当前属性集为空, 或是所有样本在所有属性上取值相同，无法划分;
3.当前结点包含的样本集合为空，不能划分。

基本算法
递归返回，
情形(1)
利用当前结点的后验分布
将父结点的样本分布作为
当前结点的先验分布
递归返回，
情形(2)
递归返回，
情形(3)
决策树算法的核心

信息增益（information gain ）
信息熵( entropy ) 是度量样本集合“纯度”最常用的一种指标
假定当前样本集合퐷中第k 类样本所占的比例为푝
푘
，则퐷的信息熵定义为：
(
)
2
k1
(   )
log
kk
Ent  D
p
p
=
=−

y
퐸푛푡퐷的值越小，则퐷的纯度越高
约定：如果푝=0，则푝푙표푔
2
푝=0
퐸푛푡퐷的最小值：0，此时퐷只有一类；
最大值：푙표푔
2
푦，此时퐷每个样本都是一类。
()
()
22
k  1k  1
2
k1
2
1
(   )log=log
1
log=log
kkk
k
k
k
Ent  Dppp
p
p
p
==
=

=−









yy
y
y
证明：

信息增益
离散属性a 的取值：푎
1
,푎
2
,...,푎
푉
Dv: D 中在a 上取值= av的样本集合
以属性a 对数据集D 进行划分所获得的信息增益为：
划分前的信息熵
划分后的信息熵
第v 个分支的权重
样本越多越重要

一个例子
D
Pr ob
17
8
17
9
否
是
西瓜数据集2.0
퐸푛푡퐷=−
8
17
푙표푔
2
8
17
−
9
17
푙표푔
2
9
17
=0.998

一个例子（续）
以属性“色泽”为例，其对应的个数据子集分别为퐷
1
（色泽= 青绿），퐷
2
（色泽= 乌黑），퐷
3
（色泽= 浅白）
子集퐷
1
包含编号为1,4,6,10,13,17的6 个样例，其中正例占푝
1
=
3
6
，反例占
푝
2
=
3
6
，퐷
2
、퐷
3
同理，3 个结点的信息熵为：
属性“色泽”的信息增益为：

一个例子（续）
类似的，其他属性的信息增益为：
显然，属性“纹理”的信息增益最大，其被选为划分属性：

一个例子（续）
对每个分支结点做进一步划分，最终得到决策树：

增益率（gain ratio）
信息增益：对可取值数目较多的属性有所偏好；、
有明显弱点，例如：考虑将“编号”作为一个属性
增益率：
其中
属性a 的可能取值数目越多(即V 越大)，则IV(a) 的值通常就越大
启发式：先从候选划分属性中找出信息增益高于平均水平的，再从中选取增益率
最高的。
缺点是:对取值数目少的属
性有偏好

基尼指数（giniindex）
反映了从퐷中随机抽取两个样例，其类别
标记不一致的概率
Gini(퐷) 越小，数据集퐷的纯度越高
属性a 的基尼指数：
在候选属性集合中，选取那个使划分后基尼指数最小的属性

划分选择vs 剪枝
研究表明: 划分选择的各种准则虽然对决策树的尺寸有较大影响，但对泛化
性能的影响很有限；
例如信息增益与基尼指数产生的结果，仅在约2% 的情况下不同；
剪枝方法和程度对决策树泛化性能的影响更为显著
在数据带噪时甚至可能将泛化性能提升25%
剪枝（pruning）是决策树对付“过拟合”的主要手段！

剪枝
为了尽可能正确分类训练样本，有可能造成分支过多→ 过拟合
可通过主动去掉一些分支来降低过拟合的风险
基本策略：
a)预剪枝(pre-pruning): 提前终止某些分支的生长
b)后剪枝(post-pruning): 生成一棵完全树，再“回头”剪枝
剪枝过程中需评估剪枝前后决策树的优劣
现在我们假定使用“留出法”

数据集
西瓜数据集2.0划分出的训练集（双线上部）与验证集（双线下部）
训练集
验证集

剪枝

预剪枝—不划分
验证集精度
“脐部=?” 划分前: ３／７＝42.9%

预剪枝—划分后
验证集精度：“脐部=?” 
划分前: ３／７＝42.9%
划分前: ５／７＝71.4%
结论：
保留划分，
不剪枝

预剪枝（续）
验
证
集
结点①：若划分，根据结点②,③,④的训练样例，将这3个结点分别标记为“好瓜”、“好
瓜”、“坏瓜”。此时，验证集中编号为4,5,8,11,12的样例被划分正确。
验证集精度为
5
7
×100%=71.4%
对结点②，③，④分
别进行剪枝判断，节点
②，③都禁止划分，结
点④本身为叶子结点。
最终得到仅有一层划分
的决策树，称为“决策
树桩”( decision stump )

后剪枝

后剪枝（续）
首先考虑结点⑥，若将其替换为叶结点，根据落在其上的训练样例7,15将其
标记为“好瓜”，测得验证集精度提高至57.1%，于是决定剪枝
验证集精度
剪枝前: 3/7=42.9%
剪枝后: 57.1%
后剪枝决策: 剪枝

后剪枝（续）
首先考虑结点⑤，若将其替换为叶结点，根据落在其上的训练样例6,7,15将其
标记为“好瓜”，测得验证集精度提高至57.1%，可以不剪枝；
对结点②，若将其替换为叶结点，根据落在其上的训练样例{1,2,3,14}，将其标
记为“好瓜”，测得验证集精度提升至71.4%，决定剪枝；
对结点③和①，先后替换为叶节点，其验证集精度未提升，则不剪枝。

预剪枝vs 后剪枝
时间开销：
•预剪枝：训练时间开销降低，测试时间开销降低
•后剪枝：训练时间开销增加，测试时间开销降低
过/ 欠拟合风险：
•预剪枝：过拟合风险降低，欠拟合风险增加
•后剪枝：过拟合风险降低，欠拟合风险基本不变
泛化性能：后剪枝通常优于预剪枝

支持向量机
人工智能课程

桌子上放了两种颜色的球，用一根棍分开它们
要求：即便再放更多球之后，仍然能将它们分开
分类

在桌上放了更多的球，让一个球站错了阵营
只是稍微调整一下棍子
分类

SVM就是试图把棍放在最佳位置，好让在棍的两
边有尽可能大的间隙
SVM

新的挑战：这次的球更乱了！
遇到棘手的情况（非线性分类），像武侠片中大侠
一样，桌子一拍，球飞到空中。然后使用trick绝
招（SVM工具箱）抓起一张纸，飞到了两种球的
中间
SVM

球叫做「data」（数据源）
棍子叫做「classifier」（分类器）
最大间隙trick 叫做「optimization」（最优化）
拍桌子叫做「kernelling」（建立核函数）
那张纸叫做「hyperplane」（超平面）
SVM

1995，由Corinna Cortes及Vladimir Vapanik等人发明的SVM（Support 
Vector Machines，支持向量机）算法诞生，迅速打败了神经网络算法成为
主流。
Vladimir Vapnik：he has an h-index of 121 and, overall, his 
publications have been cited close to 230,000 times.
Corinna Cortes                 Vladimir Vapnik 
SVM

92
什么是SVM
➢全名：Support Vector Machine（支持向量机）
➢支持向量：支撑平面上把两类类别划分开来的超平面的
向量点
➢机：一个算法
➢基于统计学习理论的一种机器学习方法。简单的说
，就是将数据单元表示在多维空间中，然后对这个
空间做划分的算法。

93
SVM在解决小样本、非线性、及高维模式识别中
有很多优势
–Vapnik:《Statistical Learning Theory》
SVM是建立在统计学习理论的VC维和结构风险最
小原理基础上，根据有限的样本信息在模型的复杂
性及学习能力之间寻求折衷，以获得最好的泛化能
力
–风险：近似模型与问题真实解之间的误差
什么是SVM

94
VC维：对函数类的一种度量，可以简单的理解为问题的复杂程度，VC
维越高，一个问题就越复杂
VC=Vapnik–Chervonenkisdimension
An Intuition using poly data fitting:
VC维，结构风险

95
结构风险=经验风险+置信风险
经验风险푅
푒푚푝
푤：分类器在training data上的分类的结果与真实结
果之间的差值
最小化经验风险：过拟合
–样本上100%的准确，但是VC维很高
–推广能力差
–样本只是九牛一毛
–经验风险要能逼近真实风险
VC维，结构风险

96
置信风险：在多大程度上可以信任分类器在未知样本上分类的结果
–样本数量：样本数量越大，学习结果越有可能正确，置信风险越
小
–分类函数的VC维，显然VC维越大，推广能力越差，置信风险会变
大
评价分类器好坏：经验风险+置信风险
–经验风险：在给定样本上的误差
–置信风险：在多大程度上可以信任分类器在未知样本上分类的结
果
VC维，结构风险

97
评价分类器好坏：经验风险+置信风险
–经验风险：在给定样本上的误差
–置信风险：在多大程度上可以信任分类器在未知样本上分类的结
果
VC维，结构风险

98
VC维，结构风险

99
泛化误差界：置信风险是没有办法精确计算的，因此只能给出一个估
计的区间，也使得整个误差只能计算上界，而无法计算准确的值（所
以叫做泛化误差界，而不叫泛化误差）。
푅푤≤푅
푒푚푝
푤+휙푛/ℎ
푅푤：真实风险；푅
푒푚푝
푤：经验风险；휙푛/ℎ：置信风险
对于泛化误差的估计，可简单用交叉验证的方法来实际测试。
我们目标从经验风险最小化变为了寻求经验风险与置信风险的和最小
，即结构风险最小
SVM : 最小化结构风险（不再只是优化经验风险）
泛化误差界

100
SVM擅长应付样本数据线性不可分的情况
◆松弛变量“slack variable” 
◆核函数“kernel function”
高维模式识别
◆SVM擅长处理高维数据#samples<#dimension
◆用到的样本信息少（支持向量）
什么是SVM
松弛变量核函数

101
线性分类器(感知机)：最简单也很有效的分类器形式
线性分类器
线性可分的：一个线性函数能够将样本完全正确的分开
非线性可分的：一个线性函数不能够将样本完全正确的分开
超平面（Hyperplane）：平面中的直线、空间中的平面之推广
For 1-D?  2-D? 3-D? 4-D?  The hyperplanes?

102
◼Logistic regression 逻辑回归分类模型
例如一个线性函数，푔푥=푤푥+푏
取阈值0，当有一个样本xi需要判别的时候，若g(xi)>0，就
判别为类别C1，若g(xi)<0，则判别为类别C0
푓푥=푠푔푛푔푥=ቊ
+1푤푥+푏>0
−1푤푥+푏<0
最简单的线性分类器
•在n维空间中，x及w都代表n维向量
•g(x)=0就是中间那条分类直线（or 面or...）的表达式，即wx+b=0，我
们也把这个函数叫做分类面。
分类面的选择可以很多.....

103
One Possible Solution
B
1
线性分类器

104
Another possible solution
B
2
线性分类器

105
Other possible solutions
B
2
线性分类器

106
Which one is better? B1 or B2?
How do you define better?
B
1
B
2
线性分类器
⚫B2分割线与最近的数据点
只有很小的间隔，如果测
试数据有一些噪声的话可
能就会被B2错误分类(即对
噪声敏感、泛化能力弱)。
⚫B1以较大间隔将它们分开
，这样就能容忍测试数据
的一些噪声而正确分类，
是一个泛化能力不错的分
类器。

107
间隔
A分为类别X的信心高于C: 
如何量化把一个样本点分为某一类的置信度？
离分类面越远越好，g(x)越大越好！
푔(푥_A)=푤푥_A+푏>푤푥_C+푏=푔(푥_C)
푔(푥_A)> 푔(푥_C)
푔(푥)= 푤푥+푏

108
◼函数间隔（Functional Margin）：
෢
훾
(푖)
=푦
푖
∗푤푥
푖
+푏=|푔푥
푖
|
线性分类器
某个样本属于正类别，wxi+b>0，而yi为+1；
若属于负类别，那么wxi+b<0，而yi为-1；
yi(wxi+b)总是大于0的，定义
෢
훾
(푖)
=|wxi+b|=|g(x)| 
越大的间隔值（
෢
훾
(푖)
、或是|g(x)|），代表分类的信心越大
◼Logistic regression中，wx+b= 2wx+ 2b=0 代表同一曲线，取决于
wx+b的正负，而跟其2wx+ 2b大小无关
◼Fuctionalmargin中，将w、b换成2w、2b将能增大间隔
෢
훾
(푖)
→2
෢
훾
(푖)
,  
此时最大化间隔没有意义，一般地，会对
훾
(푖)
=
1
푤
푔푥
푖

109
几何间隔（Geometric margins）：
Replace (푤,푏)with (
푤
푤
,
푏
푤
)
几何间隔就是点到超平面的欧氏距离
线性分类器：几何间隔
훾
(푖)
=
1
푤
푔푥
푖
훾
(푖)
=푦
푖
푤
푤
푥
푖
+
푏
푤

110
几何间隔（Geometric margins）：
线性分类器：几何间隔
❑푤与푤푥+푏=0方向垂直
푤푥+푏=0
훾
(푖)
=푦
푖
푤
푤
푥
푖
+
푏
푤
훾=min
푖=1,...,푚
훾
(푖)
❑给定集合S={푥
푖
,푦
푖
;푖=1...푚},
定义

利用三角形的面积计算公式可得
S = (1/2)ab = (1/2)hc
故三角形些边上的高为h = ab/c
线性分类器：几何间隔

线性分类器：几何间隔
Ax+By+C=0

113
点到超平面的欧氏距离（几何间隔Geometric margins）
훾
(푖)
=
|푤푥
푖
+푏|
푤
=
1
푤
푔푥
푖
线性分类器：几何间隔

114
线性分类器
几何间隔
H是分类面，而H1和H2是平行于H，且过离H最近的两类样本的直线，
H1与H，H2与H之间的距离就是几何间隔。

115
线性可分，有超平面
(向量化表示)将数据集分开，使得一侧是“+1”
类，另一侧是“-1类”
线性分类器：例子

116
虽然写法不一样，但是只代表一条直线
能不能想个办法，把这条直线统一一下？
找离直线最近的点，来改进直线方程
都除以4.3
都除以2.15
线性分类器：例子

117
虽然写法不一样，但是只代表一条直线
能不能想个办法，把这条直线统一一下？
找离直线最近的点，来改进直线方程
线性分类器：例子
VS
未用信息：y
(i)
左下方为负数，右上方为正数
左下方为正数，右上方为负数

118
统一化w和b后，两边离线的最近的样本点使得w
T
x+b=1
及w
T
x+b=-1，此时两线之间几何间隔为
线性分类器：例子
푚푎푥
2
푤

119
统一化w和b后，两边离线的最近的样本点使得w
T
x+b=1
及w
T
x+b=-1，此时两线之间几何间隔为
线性分类器：例子
푚푎푥
2
푤

120
Find hyperplane maximizesthe margin => B1 is better than B2
B
1
B
2
b
11
b
12
b
21
b
22
margin
线性分类器
푚푎푥
2
푤

121
线性分类器－求解
目标函数：
푚푎푥
2
푤
=푚푖푛푤= 푚푖푛
1
2
푤
2
直接来解这个求最小值问题，当||w||=0时得到了目标函数的最小值
无论给什么样的数据，都是这个解, 反映在图中，就是H1与H2两条直线间的距
离无限大。所有的样本点（无论正样本还是负样本）都跑到H1和H2中间
原因：只考虑了目标，而没有加入约束条件（在求解过程中必须满足的条件）
，样本点必须在H1或H2的某一侧（或者至少在H1和H2上），而不能跑到两
者中间
如何解决？

122
线性分类器－求解
约束条件：
푦
푖
푤푥
푖
+푏≥1푖=1,2,...,m
转化为和0比较：
푦
푖
푤푥
푖
+푏−1≥0푖=1,2,...,m
带约束的最小值问题：
min                
1
2
푤
2
subject to      푦
푖
푤푥
푖
+푏−1≥0푖=1,2,...,m

123
线性分类器－求解
凸二次规划
min                
1
2
푤
2
subject to      푦
푖
푤푥
푖
+푏−1≥0푖=1,2,...,m
自变量就是w，而目标函数是w的二次函数，所有的约束条件都是w的线
性函数。故称之为二次规划(Quadratic Programming，QP)，且是一
个凸二次规划。
凸二次规划特点：有解（全局最优解）

124
线性分类器－求解
凸二次规划
min                
1
2
푤
2
（1）
subject to      푦
푖
푤푥
푖
+푏−1≥0푖=1,2,...,m
我们称上式（1）所述问题为原始问题(primal problem), 可以应用拉格
朗日乘子法构造拉格朗日函数(Lagrange function)再通过求解其对偶问
题(dual problem)得到原始问题的最优解。转换的原因在于：
⚫对偶问题更易求解，由下文知对偶问题只需优化一个变量且约束条件
更简单；
⚫能更加自然地引入核函数，进而推广到非线性问题。

125
线性分类器－求解
◼经典优化问题
min                                   푓푤
subject to              푔
푖
푤≤0푖=1,2,...,푘
ℎ
푗
푤=0푗=1,2,...,푙
푘+푙个约束条件，其中푘个是不等式约束，푙个等式约束。
푤为自变量，求푓푤的最小值
不是在整个空间找푤，而是在约束条件所划定的一个有限空间里找푤

126
Lagrangian
min                                   푓푤
subject to              푔
푖
푤≤0푖=1,2,...,푘
ℎ
푖
푤=0푖=1,2,...,푙

127
Lagrangian

128
Lagrangian
转换成原问题等价形式：
原问题Primal Problem对偶问题Dual Problem
原问题
对偶问题

129
Lagrangian
原问题Primal Problem对偶问题Dual Problem
某些条件下：d*=p*，此时其解满足

130
In our case

131
In our case
分别对w b 求导：
代入（8) 得到：
Find the α’s that maximize W(α) subject to the constraints, then we can go back and find
the optimal w’s and b’s as a function of the α’s. 求解包括例如SMO算法, ALM方法等等....

132
In our case
w取得最优解时：
푤=
σ
푖=1
푛
푎
푖
푦
푖
푥
푖
=푎
1
푦
1
푥
1
+푎
2
푦
2
푥
2
+...+푎
푚
푦
푚
푥
푚
注意：
◼这些拉格朗日乘子中，至少存在一个푎
i
>0, （若不存在，则푤=0，
2
푤
=∞，
显然不行）

133
In our case
注意：
◼这些拉格朗日乘子中，至少存在一个푎
i
>0, （若不存在，则푤=0，
2
푤
=∞，
显然不行）再根据KKT条件。
所以至少存在一个j，使得푦
푗
푤푥
푗
+푏−1=0，于是可以求得最优푏
푏=
1
푦
푗
−푤푥
푗
=푦
푗
−푤푥
푗
=
σ
푖=1
푛
푎
푖
푦
푖
푥
푗
푥
푖
至此，所以我们就求得了整个线性可分SVM的解。求得的分离超平面为:

134
In our case
求得的分离超平面为:
分类的决策函数就是

135
In our case
◼不等于0的拉格朗日乘子后面所乘的样本点，其实都落在最大间隔边界上H1和
H2上，也正是这部分样本（而不需要全部样本）唯一的确定了分类函数
◼严格的说，这些样本的一部分就可以确定。确定一条直线，只需要两个点就可以
，即便有三五个都落在上面，我们也不是全都需要。这部分真正需要的样本点，
就叫做支持向量。

136
近似线性可分
样本点往往是成千上万的。如果新增了一个样本点，映射到空间后，其位置是
如黄色方块这样的
这样一个单独的样本，使得问题变成了线性不可分的。这样的问题叫做“近似
线性可分”的问题

137
近似线性可分：软间隔
这个点更有可能是一个错误的点，即噪声，仍可以用原来的分类器进行
分类，不会对效果造成影响。然后程序并不能区分一个点是不是噪声。
硬间隔分类法：原本的优化问题的表达式中，要考虑所有的样本点（
不能忽略某一个，因为程序它怎么知道该忽略哪一个呢？）
–寻找正负类之间的最大几何间隔
–而几何间隔本身代表的是距离，是非负的，像上面这种有噪声的
情况会使得整个问题无解
–这种解法叫“硬间隔”分类法：硬性的要求所有样本点都满足和
分类平面间的距离必须大于某个值
缺点：硬间隔的分类法其结果容易受少数点的控制，这是很危险的
解决：允许一些点到分类平面的距离不满足原先的要求

138
软间隔：松弛变量
我们原先对样本点的要求是：
푦
푖
푤푥
푖
+푏≥1푖=1,2,...,푙
离分类面最近的样本点函数间隔也要比1大。如果要引入容错性，就给1这个硬
性的阈值加一个松弛变量，即允许：
푦
푖
푤푥
푖
+푏≥1−휁
푖
푖=1,2,...,푙휁
푖
≥0
因为松弛变量是非负的，因此最终的结果是要求间隔可以比1小。但是当某些
点出现这种间隔比1小的情况时，（这些点也叫离群点），意味着我们放弃了
对这些点的精确分类，而这对我们的分类器来说是种损失。
但是放弃这些点也带来了好处，那就是使分类面不必向这些点的方向移动，因
而可以得到更大的几何间隔（在低维空间看来，分类边界也更平滑）。显然我
们必须权衡这种损失和好处。好处很明显，我们得到的分类间隔越大，好处就
越多。

139
松弛变量
min                
1
2
푤
2
subject to      푦
푖
푤푥
푖
+푏−1≥0푖=1,2,...,푙
优化问题：
衡量损失的两种方法：
෍
푖=1
푙
휁
푖
2
෍
푖=1
푙
휁
푖
把损失加到目标函数里：
引入惩罚因子（cost, C）
min                
1
2
푤
2
+퐶
σ
푖=1
푙
휁
푖
subject to      푦
푖
푤푥
푖
+푏≥1−휁
푖
휁
푖
≥0,푖=1,...,푙
二阶软间隔分类器vs 一阶软间隔分类器

140
松弛变量
并非所有的样本点都有一个松弛变量与其对应,只有“离群点”才有，
或者也可以这么看，所有没离群的点松弛变量都等于0
松弛变量的值实际上标示出了对应的点到底离群有多远，值越大，点
就越远
惩罚因子C决定了你有多重视离群点带来的损失
–当所有离群点的松弛变量的和一定时，C越大，对目标函数的损失
也越大，非常不愿意放弃这些离群点，
–C定为无限大，这样只要稍有一个点离群，目标函数的值马上变成
无限大，马上让问题变成无解，这就退化成了硬间隔问题
惩罚因子C不是一个变量，整个优化问题在解的时候，C是一个你必须
事先指定的值
尽管加了松弛变量这么一说，但这个优化问题仍然是一个优化问题，
解它的过程比起原始的硬间隔问题来说，没有任何更加特殊的地方

141
软间隔问题求解

142
软间隔问题求解
对W b  휻求解

143
软间隔问题求解
对휶求解

144
软间隔问题求解
求解W 与b

145
软间隔问题求解

146
不同的惩罚参数C的影响

147
松弛变量
改进
C所起的作用：表征你有多么重视离群点，C越大越重视，越不想丢掉它们
我们可以给每一个离群点都使用不同的C，这时就意味着你对每个样本的重视程
度都不一样，有些样本丢了也就丢了，错了也就错了，这些就给一个比较小的C
；而有些样本很重要，决不能分类错误，就给一个很大的C。

148
松弛变量
偏斜/不平衡问题：
样本的偏斜(unbalanced)问题：指的是参与分类的两个类别（也可以
指多个类别）样本数量差异很大。比如说正类有10000个样本，而负
类只给了100个
方形的点是负类。H，H1，H2是根据给的样本
算出来的分类面
由于负类的样本很少很少，所以有一些本来是负
类的样本点没有提供，比如图中两个灰色的方形
点
如果这两个点有提供的话，那算出来的分类面应
该是H’，H2’他们和之前的结果有出入
上负类给的样本点越多，就越容易出现在灰色点
附近的点，结果也就越接近于真实的分类面
由于偏斜现象的存在，使得数量多的正类可以把
分类面向负类的方向“推”，因而影响了结果的
准确性

149
松弛变量
正类之所以可以“欺负”负类，其实并不是因为负类样本少，真实的原因是负
类的样本分布的不够广（没扩充到负类本应该有的区域）
比如给政治类和体育类文章做分类，政治类文章很多，而体育类只有几篇关于
篮球的文章，分类会明显偏向于政治类，如果要给体育类文章增加样本，但增
加的样本仍然全都是关于篮球的，虽然体育类文章在数量上可以达到与政治类
一样多，但过于集中了，结果仍会偏向于政治类！所以给C+和C-确定比例更好
的方法应该是衡量他们分布的程度
比如：计算在空间中占据了多大的体积，例如给负类找一个超球（高维空间里
的球），它包含所有负类样本，再给正类找一个，比较两个球的半径，从而大
致确定分布的情况。显然半径大的分布就比较广，就给小一点的惩罚因子
但是，有的类别样本确实很集中，这不是提供的样本数量多少的问题，而是类
别本身的特征（就是某些话题涉及的面很窄，例如计算机类的文章就明显不如
文化类的文章那么广泛），这个时候即便超球的半径差异很大，也不应该赋予
两个类别不同的惩罚因子

150
松弛变量
解决：
对付数据集偏斜问题的方法之一就是在惩罚因子上作文章，把目标函数中
因松弛变量而损失的部分变成：
퐶
+
෍
푖=1
푝
휁
푖
+퐶
−
෍
푗=푝+1
푝+푞
휁
푗
其中i=1...p都是正样本，j=p+1...p+q都是负样本
如何确定C+，C-?
参数调优
使用两类样本数的比来算
比如C+设为5，C-就设为500（因为正负样本比例为10000：100）

151
线性不可分时：核函数

152
线性不可分
如果样本线性不可分：
Positive ClassNegative Class

153
线性不可分
Positive ClassNegative Class
通过点在这条曲线的上方还是下方就可以判断点所属的类别

154
线性不可分
这条二次曲线的表达式可以写为：
푔푥=푐
0
+푐
1
푥+푐
2
푥
2
但它并非线性函数，新建一个向量y和a：
푦=
푦
1
푦
2
푦
3
=
1
푥
푥
2
푎=
푎
1
푎
2
푎
3
=
푐
0
푐
1
푐
2
푔푥转化为푓푦=푎,푦
即푔푥=푓푦=푎푦
在任意维度的空间中，这种形式的函数都是一个线性函数（只不过其中的a和y都是多
维向量）
原来在低维空间中一个线性不可分的问题，映射到高维空间后，变成了线性可分的
最初解决线性不可分问题的基本思路：向高维空间转化，使其变得线性可分

155
线性不可分
文本分类示例：
一个文本分类问题的原始空间是1000维的，即每个要被分类的文档被
表示为一个1000维的向量。在这个维度上问题是线性不可分的。现在
我们有一个2000维空间里的线性函数：
푓푥′=푤
′
,푥′+푏
w’和x’都是2000维向量，w’是定值，x’是向量
输出一个1000维的向量x，分类过程即：
I.把x变换为2000维的向量x’
II.求x’与w’的内积
III.把内积值与b相加
IV.看结果大于阈值还是小于阈值，即得到分类结果。

156
线性不可分
我们其实只关心那个高维空间里内积的值，那个值算出来了，分类
结果就算出来了
从理论上说，x’是经由x变换来的，因此广义上可以把它叫做x的函
数（有一个x，就确定了一个x’），而w’是常量，它是一个低维空
间里的常量w经过变换得到的，所以给了一个w 和x的值，就有一个
确定的f(x’)值与其对应
是否能有这样一种函数K(w,x),他接受低维空间的输入值，却能算出
高维空间的内积值<w’,x’>？
即有这样的函数，当给了一个低维空间的输入x以后
푔푥=퐾푤,푥+푏
푓푥′=푤
′
,푥′+푏
这两个函数的计算结果就完全一样，我们也就用不着费力找那个映射关系，直
接拿低维的输入往g(x)里面代就可以了

157
线性不可分: 引入核函数
线性分类器푓푥′=
σ
푖=1
푛
푎
푖
푦
푖
푥′
푖
,푥′+푏
现在这个就是高维空间里的线性函数，我们就可以用一个低维空间里的函数（
再一次的，这个低维空间里的函数就不再是线性的）来代替：
푔푥=෍
푖=1
푛
푎
푖
푦
푖
퐾푥
푖
,푥+푏
f(x’) 和g(x)里的a，y，b全都是一样的
尽管给的问题是线性不可分的，但是硬当它是线性问题来求解
求解过程中，凡是要求内积的时候就用你选定的核函数来算
这样求出来的a再和选定的核函数一组合，就得到分类器

上述过程中....K其实就是核函数
K的引入是为了解决维数爆炸
–对一个二维空间做映射，假如新空间是原始空间所有1阶及2阶段组合，可以得到
五个维度（푥
1,
푥
2,
푥
1
푥
2,
푥
1
2
,푥
2
2
）
–三维空间的话就会得到19维的新空间（푥
1,
푥
2,
푥
3,
푥
1
푥
2,
푥
1
푥
3,
푥
2
푥
3,
푥
1
2
,푥
2
2
,푥
3
2
,
푥
1
푥
2
2
,푥
1
푥
3
2
,푥
2
푥
3
2
,푥
1
2
푥
2,
푥
1
2
푥
3,
푥
2
2
푥
3,
푥
1
3
,푥
2
3
,푥
3
3
,푥
1
푥
2
푥
3
）
–这个数目是呈指数爆炸的
映射到高维空间中，然后再根据内积的公式进行计算
直接在低维中计算，不显式地写出映射结果
解决了维数爆炸的问题，
而上面的函数为核函数
（휼
ퟏ
,휼
ퟐ
,휼
ퟏ
휼
ퟐ
,휼
ퟏ
ퟐ
,휼
ퟐ
ퟐ
）（훏
ퟏ
,훏
ퟐ
,훏
ퟏ
훏
ퟐ
,훏
ퟏ
ퟐ
,훏
ퟐ
ퟐ
）
（ퟐ휼
ퟏ
,ퟐ휼
ퟐ
,ퟐ휼
ퟏ
휼
ퟐ
,휼
ퟏ
ퟐ
,휼
ퟐ
ퟐ
）（ퟐ훏
ퟏ
,ퟐ훏
ퟐ
,ퟐ훏
ퟏ
훏
ퟐ
,훏
ퟏ
ퟐ
,훏
ퟐ
ퟐ
）

核函数
核函数的意义：
一个是映射到高维空间中，然后再根据内积的公式进行计算，计算复杂度高
而另一个则直接在原来的低维空间中进行计算，而不需要显式地写出映射后的结果，
计算复杂度低
把这里的计算两个向量在隐式映射过后的空间中的内积的函数K（x，z）叫做核函数
(Kernel Function)
核函数能简化映射空间中的内积运算

核函数
几个常见的核函数：

核函数+软间隔SVM
⚫原内积运算就用自己选定的核函数来替换

非线性SVM

163
核函数选择
⚫如针对具体问题在多种核函数中进行选择？
–对核函数的选择，现在还缺乏指导原则
–实验的观察结果表明，某些问题用某些核函数效果很好，用另
一些就比较差
–一般来讲，径向基（高斯）核函数不会出太大偏差
–http://yann.lecun.com/exdb/mnist/
⚫如果使用核函数向高维空间映射后，问题仍然是线性不可分的，怎
么办？
–结合松弛变量这一概念来解决。

SVM用于多分类
一次性考虑所有样本，并求解一个多目标函数的优化问题，一次性得到多个分类面，即
多个超平面把空间划分为多个区域，每个区域对应一个类别？
就像这样：
从前面可以看出，SVM是一个典型的二分类器，如何由二分类器得到多分类器？
这个方法是不可行的，一次性求解的方法计算量实在太大，大到无法实用的地步。

SVM用于多分类
“一类对其余”方法：就是每次仍然解一个两类分类的问题。比如我们有
5个类别，第一次就把类别1的样本定为正样本，其余2，3，4，5的样本
合起来定为负样本，这样得到一个两类分类器，它能够指出一篇文章是还
是不是第1类的；第二次我们把类别2 的样本定为正样本，把1，3，4，5
的样本合起来定为负样本，得到一个分类器，如此下去，我们可以得到5
个这样的两类分类器（总是和类别的数目一致）。
优点：优化问题的规模比较小，分类速度比较快。
缺点：
◆分类重叠现象：每个分类器都说某个样本属于它的类。
◆不可分类现象：每个分类器都说某个样本不属于它的类
◆偏斜问题：数据分类中常见问题，即数据类别对应样本数差别较大

SVM用于多分类
每次选一个类的样本作正类样本，而负类样本则变成只选一个类（
避免偏斜）。算出这样一些分类器：第一个只回答“是第1类还是
第2类”，第二个只回答“是第1类还是第3类”，第三个只回答“
是第1类还是第4类”，如果有k个类别，则总的两类分类器数目为
k(k-1)/2
虽然分类器的数目多了，但是相应的训练速度快
假设一个文章有5个类别1，2，3，4，5，则有10个分类器：
–12，13，14，15
–23，24，25
–34，35
–45

SVM用于多分类
真正分类时，把一篇文章输入给所有分类器，第一
个分类器会投票说它是“1”或者“2”，第二个
会说它是“1”或者“3”，让每一个都投上自己
的一票，最后统计票数，如果类别“1”得票最多
，就判这篇文章属于第1类
会有分类重叠的现象，但不会有不可分类现象，因
为不可能所有类别的票数都是0
对于大类别数的情况，比如有1000个类别，则要
调用的分类器数目会达到500,000个

SVM用于多分类
“一对一”方法：在对一篇文章进行分类之前，先按照下面图的样子
来组织分类器（一个有向无环图，也叫DAG SVM）
1VS 5
2 VS 5
1VS 4
3 VS 5
2 VS 4
1VS 3
4 VS 53 VS 4
3542
2 VS 31 VS 2
1
2，3，4，51，2，3，4
2，3，4
3，4，51，2，3

SVM用于多分类
只调用了4个分类器（如果类别数是k，则只调用k-1个
），分类速度快，且没有分类重叠和不可分类现象
缺点：如果最一开始的分类器回答错误（明明是类别1
的文章，它说成了5），那么后面的分类器是无论如何
也无法纠正它的错误的（因为后面的分类器压根没有出
现“1”这个类别标签），其实对下面每一层的分类器
都存在这种错误向下累积的现象
DAG的累积上限，是有定论的，有理论证明。而一对其
余和一对一方法中，尽管每一个两类分类器的泛化误差
限是知道的，但是合起来做多类分类的时候，无法知道
误差上界，意味着准确率低到0也是有可能的

SVM用于多分类
DAG方法改善：希望根节点少犯错误为好，因此
参与第一次分类的两个类别，最好是差别特别大，
大到以至于不太可能把他们分错
或者总取在两类分类中正确率最高的那个分类器作
根节点，或者我们让两类分类器在分类的时候，不
光输出类别的标签，还输出一个类似“置信度”的
值，当它对自己的结果不太自信的时候，我们就不
光按照它的输出走，把它旁边的那条路也走一走，
等等

最近邻算法
人工智能课程

KNN（最近邻算法）
k-nearest neighboursclassifier：
其中: 
훷
푋,푘
(푞)：返回训练集X中距离q最近的k个
样本
푔⋅：返回（训练）样本的标签
푚푎푗⋅：返回众数

Thank you
人工智能课程
