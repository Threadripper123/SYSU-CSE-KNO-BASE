# Lecture5-不确定性知识表示和推理

> 主题库：人工智能　｜　源文件：Lecture5-不确定性知识表示和推理.pdf　｜　15,942 字符　｜　116 个片段　｜　更新于 2026-11-11


第5章不确定性知识表示
和推理
概率图模型
课件来源：中山大学刘咏梅教授；多伦多大学SheilaMcIlraith教授；
浙江大学吴飞教授；海军工程大学贲可荣教授；ChrisBishop等

不确定知识表示和推理
背景
概率论与图论基础
概率图模型的表示
概率图模型的推理
概率图模型的学习

背景
人工智能系统的表示与推理过程实际上就是一种思维过程。其中，推理是从
已知事实出发，通过运用相关的知识逐步推出某个结论的过程。人们常常对
原因和结果的推理很感兴趣。比如，我感冒症状减轻了，是不是因为服用了维
生素C片导致的？但是，由于事件都带有随机性，导致看似直截了当的问题，
却不容易回答。例如，我可能仅仅喝白开水，感冒也会自己消失。
◼已知事实（证据），用以指出推理的出发点及推理时应使用的知识；
◼知识是推理得以向前推进，并逐步达到最终目标的依据。
按照所用知识的确定性，可以分为确定性和不确定性两种类别。
◼确定性推理是建立在经典逻辑基础上的，经典逻辑的基础之一就是集合论。这在
很多实际情况中是很难做到的，如高、矮、胖、瘦就很难精确地分开；
◼不确定性推理就是从不确定性初始证据出发，通过运用不确定性的知识，最终推
出具有一定程度的不确定性但却是合理或者近乎合理的结论的思维过程。

背景
常识（commonsense）具有不确定性。
◼一个常识可能有众多的例外，一个常识可能是一种尚无理论
依据或者缺乏充分验证的经验。
常识往往对环境有极强的依存性。
◼“鸟是会飞的”
把指示确定性程度的数据附加到推理规则，并由此研究不
确定强度的表示和计算问题。
处理数据的不精确和知识的不确定所需要的一些工具和方
法，包括：
◼基于Bayes理论的概率推理
◼基于可信度的确定性理论
◼基于信任测度函数的证据理论
◼基于模糊集合论的模糊推理等

背景
不确定知识表示和推理方法
◼确定性理论
该理论由Shortliffe提出，并于1976年首次在血液病诊断
专家系统MYCIN中得到了成功应用。
在确定性理论中，不确定性是用可信度来表示的。
◼证据理论
用于处理不确定性、不精确以及间或不准确的信息。
引入了信任函数来度量不确定性，引用似然函数来处理
由于“不知道”引起的不确定性。
◼模糊逻辑和模糊推理
模糊集合论是1965年由Zadeh提出的，随后，他又将模糊
集合论应用于近似或模糊推理，形成了可能性理论。
模糊逻辑可以看作是多值逻辑的扩展。模糊推理是在一
组可能不精确的前提下推出一个可能不精确的结论。

背景
不确定知识表示与推理是人工智能的核心模块之一，其理论基
础包括概率论和可能性理论等。
◼概率论处理的是由随机性引起的不确定性；
◼可能性理论处理的是由模糊性引起的不确定性。
李德毅院士在统一主观认知和客观现象中的随机性和模糊性方
面提出了不确定性人工智能的研究问题。不确定性人工智能认
为，随机性和模糊性常常是联系在一起的，在人类思维和智能行
为中难以区分并独立存在，研究不确定性需要研究随机性和模
糊性之间的关联性。
本讲主要以概率论为基础，介绍概率图模型的表示、推理和学习
离等内容。

背景
概率图模型
◼概率论与图论结合的产物，为统计推理和学习提供了一个统一的灵活框架；
◼概率图模型用节点表示变量，节点之间的边表示局部变量间的概率依赖关系。系统的
联合概率分布表示为局部变量分布的连乘积，该表示框架不仅避免了对复杂系统的
联合概率分布直接进行建模，而且易于引入先验知识。
概率图模型统一了目前广泛应用的许多统计模型和方法。
◼马尔科夫随机场（MRF）、条件随机场（CRF）
◼隐马尔科夫模型（HMM）、多元高斯模型
◼卡尔曼滤波、粒子滤波、变分推理等
JudeaPearl（朱迪亚·佩尔），贝叶斯网络之父，加州大学洛杉矶分校计算机
科学学院教授、认知系统实验室主任。2011年，因人工智能概率方法和因果
推理算法获得图灵奖。

背景
概率图模型例子

背景
概率图模型发展历程
◼历史上，曾经有来自不同学科的学者尝试使用图的形式表示高维分布的
变量之间的依赖关系；
◼在人工智能领域，概率方法始于构造专家系统的早期尝试
◼在20世纪80年代末，在贝叶斯网络和一般的概率图模型中的推理取得
重要进展。1988年，Pearl提出了信念传播（BeliefPropagation，BP）算
法，把全局的概率推理过程转变为局部变量间的消息传递，从而大大降
低了推理的复杂度
◼BP算法引起了国际上学者的广泛关注，掀起了新一轮的研究热潮。
◼如今，概率图模型的推理和学习已经广泛应用于机器学习、计算机视觉
、自然语言处理、医学图像处理、计算神经学、生物信息学等研究领域，
成为人工智能研究中不可或缺的一门技术。

背景
概率图模型表示
概率图模型推理
概率图模型学习
概率图模型研究内容

背景
概率图模型应用——图像视觉分析
图像分割
立体视觉
图像去噪
姿态估计

背景
概率图模型应用——医学诊断
McLachlan S, Dube K, Hitman G A, et al. Bayesian networks in healthcare: Distribution by medical condition[J]. 
Artificial intelligence in medicine, 2020, 107: 101912.

背景
概率图模型应用——计算神经学
Ott T, Stoop R. The neurodynamics of belief propagation on binary markov random fields[J]. Advances in neural 
information processing systems, 2006, 19.
◼在计算神经学领域，研究表明，大脑具有表示和处理不确定信息的
能力。大量的生理和心理实验发现，大脑的认知处理过程是一个概
率推理过程。
◼Ott和Stoop建立了二值马尔科夫随机场信念传播算法和神经动力学
模型的联系，证明了连续Hopfield网络的方程可以用BP算法的消息
传递迭代方程得到。因此，马尔科夫随机场中的BP算法可以由神经
元实现，每个神经元对应于MRF的一个节点，神经元之间的突触连
接对应于节点之间的依赖关系。
◼现有人工智能技术的两种主流“大脑”
一种是支持人工神经网络的深度学习加速器，基于研究“电脑”的计算机科学
，让计算机运行机器学习算法；
另一种是支持脉冲神经网络的神经形态芯片，基于研究“人脑”的神经科学，
无限模拟人来大脑。

不确定知识表示和推理
背景
概率论与图论基础
概率图模型的表示
概率图模型的推理
概率图模型的学习

概率论与图论基础
频率论学派
◼事件的概率是当我们无限次重复试验时，事件发生次数的比值
◼投掷硬币、掷骰子等
贝叶斯学派
◼将事件的概率视为一种主观置信度
◼我认为明天下雨的概率是30%；他认为明天下雨的概率是80%
ThomasBayes（约1701-1761），英国数学家。约1701年出生于伦敦，1742年成为英
国皇家学会会员。他首先将归纳推理法用于概率论基础理论，并创立了贝叶斯统计理
论，对于统计决策函数、统计推断、统计的估算等做出了贡献。

概率论与图论基础
条件概率
◼假设变量A表示一个事件的集合，且Pr(A)>0
◼在变量A发生的前提下，另一个事件的集合（用变量B表示）
发生的概率记为条件概率Pr(B|A)
假设B覆盖了整个事件集合空间的30%，且覆盖了A的80%，
那么Pr(B) = 30%，且Pr(B|A) = 80%
如果Pr(B|A) = Pr(B)，则B和A独立
如果Pr(B|A,C) = Pr(B|A)，则给定A的前提下，B和C条件独立

概率论与图论基础
乘法/链式法则
联合概率Pr(A,B)=Pr(A)Pr(B|A)=Pr(B,A)=Pr(B)Pr(A|B)
Pr(A,B
1
,B
2
,B
3
)=Pr(A)Pr(B
1
|A)Pr(B
2
|A,B
1
)Pr(B
3
|A,B
1
,B
2
)

概率论与图论基础
加法法则
Pr(A)=Pr(A,B)+Pr(A,B
c
)
=Pr(B)Pr(A|B)+Pr(B
c
)Pr(A|B
c
)
1
1
(   )
(   ,   )
(   ,    )
(   |
)
(    )
n
i
B
i
n
ii
i
Pr  A
Pr  A B
Pr  A B
Pr  A  B  Pr  B
=
=
==
=


•贝叶斯定理
(   ,   )
(    |   )
()
(   )(    |   )
()
(   )(    |   )
(   ,   )(   ,)
c
Pr  A B
Pr  B  A
Pr  A
Pr  B  Pr  A  B
Pr  A
Pr  B  Pr  A  B
Pr  A BPr  A B
=
=
=
+

概率论与图论基础
加法法则
Pr(A)=Pr(A,B)+Pr(A,B
c
)
=Pr(B)Pr(A|B)+Pr(B
c
)Pr(A|B
c
)
1
1
(   )
(   ,   )
(   ,    )
(   |
)
(    )
n
i
B
i
n
ii
i
Pr  A
Pr  A B
Pr  A B
Pr  A  B  Pr  B
=
=
==
=


•贝叶斯定理
(   ,   )
(    |   )
()
(   )(    |   )
()
(   )(    |   )
(   ,   )(   ,)
c
Pr  A B
Pr  B  A
Pr  A
Pr  B  Pr  A  B
Pr  A
Pr  B  Pr  A  B
Pr  A BPr  A B
=
=
=
+
后验概率
先验概率
标准化常量
似然度

概率论与图论基础
假设有一盒骰子，里面有4面的（点数为1、2、3、4），6
面的、8面的、12面的、20面的均匀骰子各1个。如果我
随机从盒子中选一个骰子，投掷它得到了点数5。那
么我选中的骰子为4面、6面、8面、12面、20面的概率
各是多少？

概率论与图论基础
假设有一盒骰子，里面有4面的（点数为1、2、3、4），6
面的、8面的、12面的、20面的均匀骰子各1个。如果我
随机从盒子中选一个骰子，投掷它得到了点数5。那
么我选中的骰子为4面、6面、8面、12面、20面的概率
各是多少？
(
=4)
(
=5 |
=4)
0.2   0
(
=4 |
=5)
(
=5)
(
=5)
1
0.2
(
=6)
(
=5 |
=6)
6
(
=6 |
=5)
(
=5)
(
=5)
...
Pr
Pr
Pr
Pr
Pr
Pr
Pr
Pr
Pr
Pr

==

==
骰
子
点
数
骰
子
骰
子
点
数
点
数
点
数
骰
子
点
数
骰
子
骰
子
点
数
点
数
点
数

概率论与图论基础
假设有一盒骰子，里面有4面的（点数为1、2、3、4），6
面的、8面的、12面的、20面的均匀骰子各1个。如果我
随机从盒子中选一个骰子，投掷它得到了点数5。那
么我选中的骰子为4面、6面、8面、12面、20面的概率
各是多少？

概率论与图论基础
假设有一盒骰子，里面有4面的（点数为1、2、3、4），6
面的、8面的、12面的、20面的均匀骰子各1个。如果我
随机从盒子中选一个骰子，投掷它得到了点数5。那
么我选中的骰子为4面、6面、8面、12面、20面的概率
各是多少？
置信度发生改变！

概率论与图论基础

概率论与图论基础

概率论与图论基础
三个概念
◼联合概率分布
◼边缘概率分布
◼最大后验概率状态

概率论与图论基础
图论基础

不确定知识表示和推理
背景
概率论与图论基础
概率图模型的表示
概率图模型的推理
概率图模型的学习

三类概率图模型
有向图模型（Directedgraphs）
◼如，贝叶斯网络、隐马尔科夫模型、卡尔曼滤波
无向图模型（Undirectedgraphs）
◼如，马尔科夫随机场、条件随机场
因子图模型（Factorgraphs）

贝叶斯网络定义
贝叶斯网络（BayesianNetwork，简称BN）是不确定知识表示与推理的一种
有效方法，它由一个有向无环图（DirectedAcyclicGraph,DAG）和一系列
条件概率表（衡量了上述关系的强度）组成。
1986年，JudeaPearl提出贝叶斯网络，用于描述不确定知识表示与推理问
题。贝叶斯网络让机器可以回答问题——给出一个从非洲回来的发烧且
身体疼痛的病人，最有可能的解释是疟疾。
令G 为定义在{X
1，
X
2，...,
X
N
}上的一个贝叶斯网络，其联合概率分布可以
表示为各个节点的条件概率分布的乘积：
有向无环图指的是一个无回路的有向图，即从图中任意一个节
点出发经过任意条边，均无法回到该节点。有向无环图刻画了
图中所有节点之间的依赖关系。

贝叶斯网络定义
示例：

贝叶斯网络定义
示例：

贝叶斯网络定义
示例：

贝叶斯网络定义
示例：

贝叶斯网络定义
联合概率为什么可以表示为局部条件概率
的乘积？

贝叶斯网络的表示
盘式记法（plate notation）
◼贝叶斯网络的一种简洁的表示方法，其将相互独立的、
由相同机制生成的多个变量放在一个方框（盘）内，并在
方框中标出类似变量重复出现的个数。此外，方框可以
嵌套，且通常用阴影标注出可观察到的变量。
◼示例：

贝叶斯网络的表示
一元语言模型：
一元混合语言模型：

=
=
N
n
n
w
p
p
1
)
(
)
(
w


=
=
z
N
n
n
zwpzpp
1
)|()()(w
假设文本中每个词都和其他词独立，和它的
上下文无关。通过一元语言模型，我们可以
计算一个序列的概率，从而判断该序列是否
符合自然语言的语法和语义规则。
假设给定文本主题（z）的
前提下，该文本中的每个
词都和其他词条件独立。
其中，z为隐变量。

贝叶斯网络的表示
概率潜在语义分析：
潜在狄利克雷分配：

=
z
n
n
d
z
p
z
w
p
d
p
w
d
p
)
|
(
)
|
(
)
(
)
,
(
1
(    |   ,   )
(   |   )
(
|   )   (
|    ,   )
n
N
k
n
n
n
nz
p
p
p  z
p w   z
d
 
 

   
=

=




w
11
(    |   ,   )(|   )(|)   (|,   )
d
dn
N
M
k
ddnddndnd
dnz
p Dpp  zp wzd      
==

=





贝叶斯网络的构建
给定变量{X
1
,...,X
n
}，构建一个贝叶斯网络的步骤如下：
◼步骤1：在某种变量顺序下，对所有变量的联合概率应用链式法则
Pr(X
1
,...,X
n
) = Pr(X
n
|X
1
,...,X
n-1
)Pr(X
n-1
|X
1
,...,X
n-2
)...Pr(X
1
)
◼步骤2：对于每个变量X
i
，考虑该变量的条件集合X
1
,...,X
i-1
，采用如下方
法递归地判断条件集合中的每个变量X
j
是否可以删除：如果给定其余
变量的集合，X
i
和X
j
是条件独立的，则将X
j
从X
i
的条件集合中删除。经过
这一步骤，可以得到下式
Pr(X
1
,...,X
n
) = Pr(X
n
|Par(X
n
))Pr(X
n-1
|Par(X
n-1
))...Pr(X
1
)
◼步骤3：基于上述公式，构建一个有向无环图。其中，对于每个用节点表
示的变量X
i
，其父节点为Par(X
i
)中的变量集合。
◼步骤4：为每个家庭（即变量及其父节点集合）确定条件概率表的取值。

贝叶斯网络的构建
对于疟疾（M）、流感（F）、感冒（C）和疼痛（A）这四
个变量，假设我们知道疟疾（M）、流感（F）和感冒（
C）都会导致疼痛（A），那么在构建贝叶斯网络时，
我们可以按照下述顺序完成步骤1：Pr(M,F,C,A)=
Pr(A|M,F,C)Pr(C|M,F)Pr(F|M)Pr(M)
由于M、F和C都会导致A，所以变量A的条件集合不
能删减。此外，我们通常认为这三种疾病（即M、F
和C）的发生是独立的，因此，Pr(C|M,F)=Pr(C)，且
Pr(F|M)=Pr(F)。据此构建的贝叶斯网络的有向无
环图如下所示：

贝叶斯网络的构建
Pr(M,F,C,A)=Pr(A|M,F,C)Pr(C|M,F)Pr(F|M)Pr(M)
该贝叶斯网络的参数个数为：2
3
+1+1+1=11
M
True
F
True
C
True
MFC
TrueTrueTrue
TrueTrueFalse
TrueFalseTrue
TrueFalseFalse
FalseTrueTrue
FalseTrueFalse
FalseFalseTrue
FalseFalseFalse

贝叶斯网络的构建
对于上述例子，如果我们按照另一种顺序完成步骤1：
Pr(M,F,C,A)=Pr(M|F,C,A)Pr(F|C,A)Pr(C|A)Pr(A)
由于每个变量的条件集合都不能删减，据此构建的贝叶斯
网络的有向无环图为：
该贝叶斯网络的参数个数为：
2
3
+2
2
+2+1=15
与前面构建的贝叶斯网络相比，更为复杂
由此可见因果关系等先验知识在其中的作用

贝叶斯网络的构建
在分类等特定的任务中，为了能够从有限的训练样本中
进行标签预测，有时也会忽略因果关系，人为假定一些
条件独立性。
基于“属性条件独立性假设”的朴素贝叶斯分类模型是
通过这种方式构建的一种特殊结构的贝叶斯网络，它在
强（朴素）独立性假设的条件下运用贝叶斯定理来计算
每个类别的条件概率。
虽然朴素贝叶斯分类模型的条件独立性假设太强，但在
实际应用中，该模型在很多任务上也能得到很好的结果
，并且模型简单，可以有效防止过拟合。

贝叶斯网络的构建
年
龄
(A)
收入
(I)
学生
(S)?
信用等
级
(C)?
是否
买电脑
(B)?
<=30highnofairno
<=30highnoexcellentno
31...40highnofairyes
>40mediumnofairyes
>40lowyesfairyes
>40lowyesexcellentno
31...40lowyesexcellentyes
<=30mediumnofairno
<=30lowyesfairyes
>40mediumyesfairyes
<=30mediumyesexcellentyes
31...40mediumnoexcellentyes
31...40highyesfairyes
>40mediumnoexcellentno
<=30mediumyesfair?
后验概率Pr(B=yes|A<=30,
I=medium,S=yes,C=fair)=?

贝叶斯网络的构建
后验概率Pr(B=yes|A<=30,
I=medium,S=yes,C=fair)=?
先验概率Pr(B = yes) = 9/14 ≈ 
0.64
似然度Pr(A <= 30, I = medium,
S = yes, C = fair | B = yes) = 0
先验概率Pr(B = no) = 5/14 ≈ 0.36
似然度Pr(A <= 30, I = medium,
S = yes, C = fair | B = no) = 0
需要更大规模的训练数据！
年
龄
(A)
收入
(I)
学生
(S)?
信用等
级
(C)?
是否
买电脑
(B)?
<=30highnofairno
<=30highnoexcellentno
31...40highnofairyes
>40mediumnofairyes
>40lowyesfairyes
>40lowyesexcellentno
31...40lowyesexcellentyes
<=30mediumnofairno
<=30lowyesfairyes
>40mediumyesfairyes
<=30mediumyesexcellentyes
31...40mediumnoexcellentyes
31...40highyesfairyes
>40mediumnoexcellentno
<=30mediumyesfair?

贝叶斯网络的构建
年
龄
(A)
收入
(I)
学生
(S)?
信用等
级
(C)?
是否
买电脑
(B)?
<=30highnofairno
<=30highnoexcellentno
31...40highnofairyes
>40mediumnofairyes
>40lowyesfairyes
>40lowyesexcellentno
31...40lowyesexcellentyes
<=30mediumnofairno
<=30lowyesfairyes
>40mediumyesfairyes
<=30mediumyesexcellentyes
31...40mediumnoexcellentyes
31...40highyesfairyes
>40mediumnoexcellentno
<=30mediumyesfair?
假设Pr(A,I,S,C|B) = 
Pr(A|B)Pr(I|B)Pr(S|B)Pr(C|B) ，
即给定B的条件下，A、I、S、
C相互独立朴素贝叶斯分类
该贝叶斯网络的参数个数为
：1 + 2*2 + 2*2 + 2 + 2 = 13
个
无上述假设的网络参数个数
：3*3*2*2*2 -1 = 71个

贝叶斯网络的构建
年
龄
(A)
收入
(I)
学生
(S)?
信用等
级
(C)?
是否
买电脑
(B)?
<=30highnofairno
<=30highnoexcellentno
31...40highnofairyes
>40mediumnofairyes
>40lowyesfairyes
>40lowyesexcellentno
31...40lowyesexcellentyes
<=30mediumnofairno
<=30lowyesfairyes
>40mediumyesfairyes
<=30mediumyesexcellentyes
31...40mediumnoexcellentyes
31...40highyesfairyes
>40mediumnoexcellentno
<=30mediumyesfair?
后验概率Pr(B=yes|A<=30,
I=medium,S=yes,C=fair)=?
先验概率Pr(B = yes) = 9/14 ≈ 
0.64
似然度Pr(A <= 30, I = medium,
S = yes, C = fair | B = yes) =
Pr(A <= 30 | B = yes)*
Pr(I = medium | B = yes)*
Pr(S = yes | B = yes)*
Pr(C = fair | B = yes) =
(2/9) * (4/9) * (6/9) * (6/9)
归一化后，Pr(B=yes|A<=30,
I=medium,S=yes,C=fair)=
0.8买电脑的置信度上升！

条件独立
Fork （tail-to-tail）

条件独立
Fork （tail-to-tail）

条件独立
Chain (head-to-tail)

条件独立
Collider (head-to-head)

条件独立
Collider (head-to-head)

贝叶斯网络中的D-分离
对于一个有向无环图，D-分离（D-Separation）是一种
用来判断其变量是否条件独立的图形化方法。
在基于贝叶斯网络的不确定性知识推理中，采用D-
分离方法可以简化概率计算，提高运行速度。
示例：
◼小偷（Burglar）会引发警报（Alarm）
◼地震（Earthquake）会引发警报（Alarm）
◼警报（Alarm）会引起邻居约翰打电话（JohnCalls）
◼警报（Alarm）会引起邻居玛丽打电话（MaryCalls）
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络中的D-分离
示例：

贝叶斯网络中的D-分离
间接的因果作用：

贝叶斯网络中的D-分离
间接的因果作用：

贝叶斯网络中的D-分离
间接的因果作用：

贝叶斯网络中的D-分离
间接的因果作用：

贝叶斯网络中的D-分离
间接的证据作用：

贝叶斯网络中的D-分离
间接的证据作用：

贝叶斯网络中的D-分离
间接的证据作用：

贝叶斯网络中的D-分离
间接的证据作用：

贝叶斯网络中的D-分离
间接的因果/证据作用（Chain）：给定A的前提
下，M与B条件独立。
Pr(m|a) = Pr(m|a,b) = 0.7

贝叶斯网络中的D-分离
共同的原因：

贝叶斯网络中的D-分离
共同的原因：

贝叶斯网络中的D-分离
共同的原因：

贝叶斯网络中的D-分离
共同的原因(Fork)：给定A的前提下，J与M条件
独立。
Pr(m|a) = Pr(m|a,j) = 0.7

贝叶斯网络中的D-分离
共同的原因(Fork)：未给定A的前提下，J与M不
独立。
Pr(m) = 0.011736
Pr(m|j) = 0.039972

贝叶斯网络中的D-分离
共同的作用：

贝叶斯网络中的D-分离
共同的作用：

贝叶斯网络中的D-分离
共同的作用：

贝叶斯网络中的D-分离
共同的作用（Collider）：给定A的前提下，B与
E不是条件独立的。
Pr(b|a) = 0.373551
Pr(b|a,e) = 0.003268

贝叶斯网络中的D-分离
共同的作用（Collider）：给定A的前提下，B与
E不是条件独立的。
Pr(b|a) = 0.373551
Pr(b|a,e) = 0.003268
解释消除

贝叶斯网络中的D-分离
共同的作用（Collider）：未给定A的前提下，
B与E是独立的。
Pr(b) =Pr(b|e) = 0.001

贝叶斯网络中的D-分离
D-分离（Directionalseparation，D-separation）可用于判断任意两个节
点的相关性和独立性。若存在一条路径将这两个节点（直接）连通，则称
这两个节点是有向连接（d-connected）的，即这两个节点是相关的；若不
存在这样的路径将这两个节点连通，则这两个节点不是有向连接的，则
称这两个节点是有向分离的（d-separated），即这两个节点相互独立。
定义：路径푝被限定集푍阻塞（block）当且仅当：
◼路径푝含有链结构퐴→퐵→퐶或分连结构퐴←퐵→퐶且中间节点퐵在푍中，或者
◼路径푝含有汇连结构퐴→퐵←퐶且汇连节点퐵及其后代都不在푍中。
定义：若푍阻塞了节点푋和节点푌之间的每一条路径，则称给定푍时，푋和푌
是D-分离，即给定푍时，푋和푌条件独立。

贝叶斯网络中的D-分离

贝叶斯网络中的D-分离

贝叶斯网络中的独立性
引理：父节点已知时，该节点与其所有非后代的节点（non-
descendants）满足D-separated.
定理：父节点已知时，该节点与其所有非后代的节点（non-
descendants）条件独立。

贝叶斯网络具体例子
单一：NaiveBayes（朴素贝叶斯）
混合：GMM（混合高斯模型）
加入时间维度
MarkovChain、GaussianProcess（无限维高斯分布）
连续：GaussianBayesianNetwork
混合模型与时间结合起来：动态模型
HMM（离散）、KarmanFilter（连续[高斯]、线性）、ParticleFilter（非高斯、
非线性）
总的来说，用两句话总结这些模型：从单一到混合、从有限到无限
两个角度：空间（随机变量的取值从离散到连续）、时间（加入时间维度，从某
一时刻延长到无限时间）

贝叶斯网络具体例子

不确定知识表示和推理
背景
概率论与图论基础
概率图模型的表示
概率图模型的推理
概率图模型的学习

推理（Inference）
推理问题是概率图模型的核心问题
推理相当于模型求解
学习过程也蕴含推理过程
推理复杂度高，精确推理是NP难问题

推理（Inference）

变量消元算法（VE）
变量消除（VariableElimination，VE）基本思想：通过从联合概率分
别逐步消除变量，来求解边缘概率

变量消元算法（VE）
RMF（链状图模型）：求解节点边缘概率

变量消元算法（VE）
RMF（链状图模型）：求解节点边缘概率

变量消元算法（VE）
RMF（链状图模型）：求解节点边缘概率

变量消元算法（VE）
RMF（链状图模型）：求解节点边缘概率

变量消元算法（VE）
RMF（链状图模型）：求解节点边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率

变量消元算法（VE）
贝叶斯网络：求解节点J的边缘概率
其他消除顺序

变量消元算法（VE）

贝叶斯网络的推理


变量消元算法（VE）
VE过程理解

变量消元算法（VE）
VE过程理解

信念传播算法（BP）
信念传播算法（BeliefPropagation，BP）：在树状图模型上能收敛，
且能达到精确推理。但在一般图模型上，BP为近似推理算法。

信念传播算法（BP）

信念传播算法（BP）

不确定知识表示和推理
背景
概率论与图论基础
概率图模型的表示
概率图模型的推理
概率图模型的学习

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.
•示例1：
•某超市销售的糖果有2种口味（cherry和lime）
•假设尝到c颗cherry口味的和l颗lime口味的

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例1：
•某超市销售的糖果有2种口味（cherry和lime）
•假设尝到c颗cherry口味的和l颗lime口味的
记上述观察到的数据为d
()
(   |)(1)
log(   |)loglog(1)
log(   |)(1)
(1)0  ()
cl
Pr d  h
Pr d  hcl
dPr d  hdcl
clc   c   l





   

=−
=+−
=   −   −
−   −  =    =    +
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例2：
•某超市销售的糖果有2种口味（cherry和lime），
包装纸的颜色分为绿色和红色
•糖果的口味决定了包装纸的颜色
•假设尝到c颗cherry口味的（其中g
c
颗为绿色包装
纸，r
c
颗为红色包装纸），以及l颗lime口味的
（其中g
l
颗为绿色包装纸，r
l
颗为红色包装纸）
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例2：
•某超市销售的糖果有2种口味（cherry和lime），
包装纸的颜色分为绿色和红色
•糖果的口味决定了包装纸的颜色
•假设尝到c颗cherry口味的（其中g
c
颗为绿色包装
纸，r
c
颗为红色包装纸），以及l颗lime口味的
（其中g
l
颗为绿色包装纸，r
l
颗为红色包装纸）
12
,   ,1122
11
1
22
2
(   |)(1)   (1)(1)
(1)0  ()
(1)0  ()
(1)0  ()
ccll
rgrg
cl
ccccc
lllll
Pr d  h
clc   c   l
rgr    rg
rgr    rg
  
          



=−−−
−   −  =    =    +
−−   =     =+
−−   =     =+
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例3：
•不可观测的k值随机变量C
•可观测的二值属性X
1
至X
4
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例3：
•不可观测的k值随机变量C
•可观测的二值属性X
1
至X
4
假设k=3。期望步：随机初始化条
件概率表，通过贝叶斯定理，可以
计算每行数据的后验概率，例如
Pr(C=1|X
1
=t,X
2
=f,X
3
=t,X
4
=
t)、Pr(C=2|X
1
=t,X
2
=f,X
3
=t,
X
4
=t)、Pr(C=3|X
1
=t,X
2
=f,X
3
=t,X
4
=t)，假设分别为0.407、
0.121、0.472，由此可以得到完备
化的数据实例。
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的参数学习
条件概率表的学习方法：
◼极大似然
◼期望-最大化
•示例3：
•不可观测的k值随机变量C
•可观测的二值属性X
1
至X
4
假设k=3。最大化步：通过期望步得
到的完备化数据实例，采用极大似然
法，重新计算条件概率表。
S.Russell,P.Norvig.ArtificialIntelligence:AModernApproach.3rdedition,PrenticeHall,2009.

贝叶斯网络的学习
在给定一个数据样本集合的前提下，寻找一个与训练样本集
匹配最好的网络结构，被称为贝叶斯网络的结构学习。
搜索最优的网络结构是一个NP难问题，从数据中学习贝叶斯
网络的结构一直是研究的热点。
[1]C.K.Chow,C.N.Liu.Approximatingdiscreteprobabilitydistributionswithdependencetrees.IEEETransactions
onInformationTheory,14(3):462-467,1968.
[2]N.Wermuth,S.L.Lauritzen.Graphicalandrecursivemodelsforcontingencytables.Biometrika,72:537-552,
1983.
[3]G.Rebane,J.Pearl.Therecoveryofcausalpolytreesfromstatisticaldata.UAI,222-228,1987.
[4]G.F.Cooper,E.Herskovits.Abayesianmethodfortheinductionofprobabilisticnetworksfromdata.Machine
Learning,9:309-347,1992.
[5]X.Zheng,etal.DAGswithnotears:Continuousoptimizationforstructurelearning.NeurIPS,9492-9503,2018.
[6]S.Lachapelle,etal.Gradient-basedneuralDAGlearning.ICLR,2020.
[7]Y.Bengio,etal.Ameta-transferobjectiveforlearningtodisentanglecausalmechanisms.ICLR,2020.
[8]Y.Luo,J.Peng,J.Ma.Whencausalinferencemeetsdeeplearning.NatureMachineIntelligence,2:426-427,2020.

贝叶斯网络的学习

概率图模型参考资料
Textbooks:
◼Daphne Kollerand NirFriedman, Probabilistic Graphical Models
◼M. I. Jordan, An Introduction to Probabilistic Graphical Models
