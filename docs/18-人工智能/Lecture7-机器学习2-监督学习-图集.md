# Lecture7-机器学习2-监督学习-图集

> 主题库：人工智能　｜　源文件：Lecture7-机器学习2-监督学习-图集.pdf　｜　5,597 字符　｜　36 个片段　｜　更新于 2026-11-11
1943 1950 1952 1957 1960 1969 First Weights l Turingtestis Perceptron mathematical Oneofthefirst automaticall Thefirstcost proposed. function. cannot modelofa computer earned. learnXOR. x1 Q3 0  ! boardgame.    ea neuron. x2 .31 .2  3 x  X3 Φ(x)= 1ifx≥θ -∑vx+n-ws E(w)= (−Φ()2 -1otherwse 2∠ MCulloch&Ptts AlanTung AthurSamuel an Rosenblat dorow&Hoff Mnsy Paoert AIWinter(1974-1980) AlWinter(1987-1993) Backpropagation Foundation of  The first convolutional Convolutional neural Almost champion-level & automatic graphical neural network (CNN). networks (CNN) backgammon, using diferentiation. probabilistic models. used forrecognizing reinforcement learning.   Backpropagation handwriting. is ∂y ∂y ∂ 1 popularized. bySeppoLinnainmaaf KunihikoFukushima JudeaPearl Rumelhar Hinton,Williams YannLeCun GeraldTesauro

5 1 1 197 2 N S Stmargni isTis m S i 2 5 todcd 9 M eplebeas Moradesing eelearig magel al Kapaovin anisingadens ispse agec ces imagdase intoduced L M GENE 5 9 8 3 6 5 7        SupportVectorMachines MNISTby DeepBlueby LongShort-Term Memory Deep Boltzman Machine ImageNetby 2012 2013 2014 2016 2018 2019 Generative Adversarial DeepMind Google AI DeepMind First significant Word embeddings Network (GAN) results in deep The first program to beat a Pretrained Grandmaster level become popular. introduced. language models. learning. professional Go player. in Startcraft ll. ALPAGO AlexNetby Word2Vecby GANby AlphaGoby BERTby AlphaStarby zhevsy &Hinton TomasMiolov IanGoodfellow DepMind GoogleAl DpMnd ParisaRashidi, May 2020.CCBY 4.0

回归 k近邻 SVM 决策树监督学习朴素贝叶斯有标签：对于输入数据X能预测Y 逻辑回归 XGBoost Lightgbm 机学习聚类降维算法无标签： 对于输入数据X能发现什么 [监 M算法 … 马尔科夫策方法强化学习序列决策问题

100 path 80- +200 60- XX 150 40- 50 20 0 20 0 15 0 10 -10 -5 0 5 10 5 10 5 15 × 20 0

当损失函数给定时，我们将基于模型训练数据的误差(TrainingError)和测试数据的 [Tl _____ 测试误差的具体定义为： Etest = L(y ，f(xn)) n =1 其中，N为测试数据数量，L(ym，f(xn))是损失函数，yn代表真实标签，f(xn)代表预一f- 般来说，若我们模型学习的效果好，则训练误差和测试误差接近一致。

代表训练集中样本的数量建筑面积总层数楼层实用面积房价代表特征的数量 143.7 31 10 105 1 36200 162.2 31 8 118 37000 1 代表特征/输入变量 199.5 10 10 170 42500 96.5 31 13 74 31200 代表目标变量/输出变量代表训练集中的样本是特征矩阵中的第行，是一个向量。 1 [162.2  代表第个观察样本上图的： x(2） 31 y(2）=37000 代表学习算法的解决方案或函 8 数也称为假设 （hypothesis） 118    代表特征矩阵中第昇行的第昇个特征代表预测的值上图的x (2) =31,x (2 8

Linear Regression Regression 5 Sample 4 [3 [2 -6 -4 -2 0 2 4

h(x）=W0+W1x1+W2x2+ 错误程度，损失函数值越小\，模型就越好。 …+WXn 常用的损失函数包括：0-1损失函数、平方损失函数、绝对损失函数、对数损失函数等 xi) = (h(x(i)−y() (CostFunctio )度量全部样本集的平均误差。常用的代价函数包括均方误差、 要找到一组w(wo，W1，W2，，Wn） 均方根误差、平均绝对误差等。 化函数，最终要优化的函数。 (残差平方最

M 1 MSE = M (y(i) − ν(i) 2 1 m m R MSE (y, y ) = ; ∑(y (i) − y(i)2 . i= 1 =l c − MAE(y,y) 1 M =1 其中，y和_分别表示第i个样本的真实值和预测值，m为样本个数

-3.0 -25 Loss  30 20 10 0 w2 20 -40 -30 -20 -20 -10 0 10 -30 w1 20 30 —40 40

earnngrete 如果学习速率态小则会使收敛过慢

学习率梯度参数更新 αξ ; ∑1 m Wj: = Wj − α· ((h(x(i) − y(i)) · · x(i) L (同步更新wi，(=0,1,,n）

α i+b−1 Wj: = Wj − - α (h(x(k) − y(k): (k)   x j

b=1 (随机梯度下降，SGD) b=m （批量梯度下降，BGD） b=batchsize， 通常是2的指数倍，常见有32,64,128等 (小批量梯度下降，MBGD)

1 n∑( m (x(i) −)2 m i=: 1 M 1 x (i) m. i=1

0.5 -6 -4 -2 0 2 4 6

2  上 1 2 3 4 5 6

0 5 laye 0f 1 2 1 3 4 5 6

10 5  0   -5 -10 50 40 50 30 40 20 30 20 10 10 00

1.5 1.4 1.3 1.1 1.0 × 0.8 −0.1 0.0 0.1 0.2 0.3 0.4 0.5 0.6 0.7 6

0 0.0 - 0 100 200 300 400 500 600 700

3.20 0.15 0.10 3.051 0.00 0 100 200 300 400 500 600 700 iteration

5 8-4 C 2- 1 c -3 -2 -1 0 1 2

−7 -5 - 4 -3 +2 − 1 0 2 z 4 7

2. 1.5- 1.0- 0.5- 0.0- -0.5- -1.0 -2.0 -1.5 -1.0 -0.5 0.0 0.5 1.0 1.5 2.0

Algorithm 1: Adam, our proposed algorithm for stochastic optimization. See section 2 for details, and for a slightly more efficient (but less clear) order of computation. g indicates the elementwise square gt O gt. Good default settings for the tested machine learning problems are α = O.001, = .9, 2 = 0.99gand∈= 10−8 A_loperatonsonvectorsareelemen-wise. Wit and Require: β1, β2 ∈ [0, 1): Exponential decay rates for the moment estimates Require: f (Θ): Stochastic objective function with parameters θ v0 ← 0 (Initialize 2nd moment vector) ta 0 Initializetimestep while θt not converged do t←—t+1 gt ← ∇ θ ft(θt−1) ( Get gradients w.r.t. stochastic objective at timestep t) mt ← β1 · mt−1 + (1 − β1) · gt (Update biased first moment estimate) vt ← β2 · vt−1 + (1 − β2) · gt (Update biased second raw moment estimate) mt ← mt/(1 − βt)(Compute bias-corrected first moment estimate) t ← vt/(1 — βt)(Compute bias-corrected second raw moment estimate) θt ← θt−1 − α · mt/(√νt + ) (Update parameters)

色泽=? 青绿根蒂=? 蜷缩敲声=? 浊响好瓜

输入： 练集D ={(x1,y1),(x2,y2)，..(xm,ym)}； 属性集 A ={a1,a2，… ,ad}. a程函数TreeGenerateD,A 1：生成结点node; 2:ifD中样本全属于同一类别Cthen 4: endif 5：ifA= ORD中样本在A上取值相同then 6： 将node标记为叶结点，其类别标记为D中样本数最多的类；return 8：从A中选择最优划分属性a*； 9: 1 fora的每一个值ado 10： 为node生成一个分支；令D表示D中在a*上取值为a的样本子集； 11： ifD，为空then 12： 将分支结点标记为叶结点，其类别标记为D中样本最多的类；return 13: else 15: endif 16:end for

Gain(D, a) = Ent( D) − M Ent(Dv) [=1

编号色泽根蒂敲声纹理脐部触感好瓜 1 青绿蜷缩浊响清晰凹陷硬滑足是 2 乌黑蜷缩沉闷清晰凹陷硬滑 3 乌黑蜷缩浊响清晰凹陷硬滑 4 青绿蜷缩沉闷清晰凹陷硬滑是  5 浅白蜷缩浊响清晰凹陷硬滑是 6 青绿稍蜷浊响清晰稍凹软粘是 7 乌黑稍蜷浊响稍糊稍凹软粘是 - 8 乌黑稍蜷浊响清晰稍凹硬滑是 9 乌黑稍蜷沉闷稍糊稍凹硬滑否 10 青绿硬挺清脆清晰平坦软粘否 11 浅白硬挺清脆模糊平坦硬滑否 12 浅白蜷缩浊响模糊平坦软粘否 13 青绿稍蜷浊响稍糊凹陷硬滑否 14 浅白稍蜷沉闷稍糊凹陷硬滑否 15 乌黑稍蜷浊响清晰稍凹软粘否 16 浅白蜷缩浊响模糊平坦硬滑否 17. 青绿 ！ 蜷缩沉闷稍糊稍凹硬滑否

Ent(D 1) = - (1 10g2 : 2 + + 0= 10g2 = 1.000 , Ent(D2) = 10 2\ 10g2 s 10g22 = 0.918 , Ent( D3) = - 1 :10g2 +1 10g2 4\ {5) = 0.722 ,

3 Gain(D,色 )= Ent(D)−∑ D D Ent(D ) 6 +\ = 0.998 - 17 ×1.000 + × 0.918 + 17 5 ×0.722 =0.09

Gain(D,根蒂)= 0.143； Gain(D,敲声）=0.141; Gain(D,纹理)=0.381； Gain(D,脐部)=0.289； Gain(D,触感)=0.006.

理晰模糊 {1, 2, 3, 4, 5, 6, 8, 10, 15} {7, 9, 13, 14, 17} {11, 12, 16}

清晰稍糊模糊根蒂=？ 触感=？ 坏瓜蜷缩稍蜷硬挺硬滑/ 软粘好瓜色泽=？ 坏瓜好瓜坏瓜青绿乌黑浅白好瓜触感=？ 好瓜 [硬a [好瓜_ 坏瓜

Gini(D) = ∑ ∑ PkPk′ =1— K-I
