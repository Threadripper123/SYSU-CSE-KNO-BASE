# 02-1-Algorithm Complexity

> 主题库：数据结构与算法　｜　源文件：02-1-Algorithm Complexity.pdf　｜　5,922 字符　｜　31 个片段　｜　更新于 2026-11-11


Algorithm Complexity
Yan Pan
School of Computer Science and Engineering
Data Structure & Algorithms
E-mail: panyan5@mail.sysu.edu.cn

Algorithm summary
•算法的定义：解决特定问题求解步骤的描述，在计算机中表现为指令的
有限序列，并且每条指令表示一个或多个操作。
•5 algorithm specifications:
•Input (输入), Output (输出), 
•Finiteness (有穷性), Definiteness (确定性), Effectiveness (可执行性)。
•A good algorithm: 
•Correctness (正确性), Readability (可读性), 
•Robustness (健壮性), Efficiency (效率)
•Algorithm description: 
•Natural Language, Pipeline Chart, Programming, Pseudo Code.
Data Structure & Algorithms2

Analyzing algorithms
•How to measure the efficiency of the algorithm？
•(Statistics after the event) 事后统计方法
•通过设计好的测试程序和数据，利用计时器对程序的运行时
间进行比较，从而确定算法效率的高低。
•(Analysis & estimation before the event) 事前分析估算方法
•在计算机程序编制前，依据统计方法对算法进行估算。
Data Structure & Algorithms3

Analyzing algorithms
Data Structure & Algorithms4

Analyzing algorithms
•算法的执行时间，是基本（操作）语句重复执行的次数，它是问题规
模的一个函数。我们把这个函数的渐近阶称为该算法的时间复杂度。
Data Structure & Algorithms5

A case
•Sum 1+2+...+100?
Data Structure & Algorithms
1+(n+1)+n+1 = 2n+3
1+1+1 = 3
6

A case
•An extension to aforementioned case:
Data Structure & Algorithms
n
2
 iterations
算法的执行时间随
着n的增加也将远
远多于前面2个算
法。
7

A case
•由前述例子可知，同样的输入规模是 n：
•第一种方法运行的基本语句数量f(n)=n。
•第二种方法则无论n为多少，运行的基本语句数都是常数，
记作f(n)=1
•第三种方法运行的基本语句数量f(n)=n
2
。
Data Structure & Algorithms8

A case
Data Structure & Algorithms9

•Progressive function
•2n+3 operations in Algorithm A, and 3n+1 operations in Algorithm B, 
which is faster？
•在输入规模n没有限制的情况下，只要超过一个数值N，这个函数就总
是大于另一个函数，则称函数是渐进增长的。
•Notice：ignore the addition constant will not affect the algorithm 
complexity。
Analyzing algorithms
Data Structure & Algorithms10

Analyzing algorithms
•4n+8 in Algorithm C，and 2n
2
+1 in Algorithm D
•Notice: The coefficients are not important!
Data Structure & Algorithms11

•2n
2
+3n+1 in Algorithm E, and 2n
3
+3n+1 in Algorithm F
•Notice: the larger highest-order exponent, the faster raising of 
algorithm complexity as n increasing. 
Analyzing algorithms
Data Structure & Algorithms12

Analyzing algorithms
•2n
2
 in Algorithm G, 3n+1 in Algorithm H, and 2n
2
+3n+1 in 
Algorithm I.
•结论：判断一个算法的效率时，函数中的常数和其他次要项
常常可以忽略，而更应该关注主项（最高阶项）的阶数。
Data Structure & Algorithms
随着n的值越来越大，
H已经远小于G和I，
而G和I越来越相近。
13

Time complexity of the algorithm
•定义：
•在进行算法分析时，语句总的执行次数T(n)是关于问题规模n的函数，
进而分析T(n)随n的变化情况，并确定T(n)的数量级。
•算法的时间复杂度，记作：T(n)=O(f(n))。表示随问题规模n的增大，
算法执行时间的增长率和f(n)的增长率相同，称作算法的渐近时间
复杂度，简称为时间复杂度。
•其中f(n)是问题规模n的某个函数。
•用大写O()来体现算法时间复杂度的记法，称之为大O记法。
•O(1)为常数阶，O(n)为线性阶，O(n
2
)为平方阶。
Data Structure & Algorithms14

Time complexity of the algorithm
•“大O记法”：在这种描述中使用的基本参数是n，即问题实例的规模，
把复杂性或运行时间表达为n的函数。
•这里的 ”O” 表示量级 (order)，比如说“二分检索是 O(logn)的”,也就是说它需要
“通过logn量级的步骤去检索一个规模为n的数组”。
•记法 O ( f(n) ) 表示当 n 增大时，运行时间至多将以正比于 f(n) 的速度增长。
•这种渐进估计对算法的理论分析和大致比较是非常有价值的，但在实
践中细节也可能造成差异。
•例如，一个低附加代价的O(n
2
)算法在n较小的情况下可能比一个高附加代价的 
O(nlogn)算法运行得更快。当然，随着n足够大以后，具有较慢上升函数的算法必然
工作得更快。
Data Structure & Algorithms15

Time complexity of the algorithm
•一个算法往往有最好情况、平均情况、最坏情况三种情形，通常最坏情况的效率是我们
最关注的。
•最差效率是指在输入规模为n时，算法在最坏情况下的效率。
•最好效率是指在输入规模为n时，算法在最优情况下的效率。
•平均效率是指在“典型”或“随机”输入的情况下，算法具有的效率。
•算法分析是为了得到近似的执行时间，一般考察的是当问题规模n增加时，运算所需时间
频度T(n)的上界和下界，如图所示。
•其中的O、Ω、Θ表示法，分别表示了算法的效率上限(上界)、下限(下界)和等限(确界)。
Data Structure & Algorithms16

Time complexity of the algorithm
•其中的O、Ω、Θ表示法，分别表示了算法的效率上限（上界）、下限（下界）
和等限（确界），其数学上的具体定义见表，对应曲线如图所示。
Data Structure & Algorithms17

时间复杂性的分析方法
ü 首先求出程序中各语句、各模块的运行时间。
ü 再求整个程序的运行时间。
Data Structure & Algorithms18

Time complexity of the algorithm
•Constant Time (常数阶)
Data Structure & Algorithms
只有执行次数的差异，
跟问题规模n的取值
无关，所以为O(1)时
间复杂度
3次
12次
19

Time complexity of the algorithm
•Linear Time (线性阶)
•Logarithmic Time (对数阶)
Data Structure & Algorithms
循环n次，所以为
O(n)时间复杂度
由2
x
=n，得x=log
2
n，所
以为O(logn)时间复杂度
20

Time complexity of the algorithm
•Quadratic Time (平方阶)
Data Structure & Algorithms
内循环n次，再进行外循环n次，
所以为O(n
2
)时间复杂度
内循环n次，外循环m次，所以
为O(mn)时间复杂度
21

Complicated cases
•Nested Loops:
•Total run times:
•Final time complexity: O(n
2
)
Data Structure & Algorithms
i=0时，内循环执行n次
i=1时，内循环执行n-1次
......
i=n-1时，内循环执行1次
22

Complicated cases
Data Structure & Algorithms
Times: 
Time complexity: O(n
2
)
23

Time complexity of the algorithm
•Common time complexity:
•Time cost (from small to large):
Data Structure & Algorithms24

各种语句和模块分析应遵循的规则
Data Structure & Algorithms25
(1) 赋值语句或读/写语句
     运行时间通常取O (1)，有函数调用的除外，此时要考虑函数的执行时间。
(2) 语句序列
    运行时间由加法规则确定，即该序列中耗时最多的语句的运行时间。
(3) 分支语句
    运行时间由条件测试（通常为O (1)）加上分支中运行时间最长的语句的
运行时间。

各种语句和模块分析应遵循的规则
Data Structure & Algorithms26
(4) 循环语句
Ø 运行时间是对输入数据重复执行n次循环体所耗时间的总和。
Ø 每次重复所耗时间包括两部分：
Ø一是循环体本身的运行时间；
Ø二是计算循环参数、测试循环终止条件和跳回循环头所耗时间。后一部分通常为O (1)。
Ø 通常，将常数因子忽略不计，可以认为上述时间是循环重复次数n和m的乘积，其中m是n次
执行循环体当中时间消耗最多的那一次的运行时间(乘法规则)。
Ø 当遇到多重循环时，要由内层循环向外层逐层分析。因此，当分析外层循环的运行时间是，
内层循环的运行时间应该是已知的此时可以把内层循环看成是外层循环的循环体的一部分。

各种语句和模块分析应遵循的规则
Data Structure & Algorithms27
(5) 函数调用语句
Ø 若程序中只有非递归调用，则从没有函数调用的被调函数开始，计算
所有这种函数的运行时间。然后考虑有函数调用的任意一个函数P，在P
调用的全部函数的运行时间都计算完之后，即可开始计算P 的运行时间。
Ø 若程序中有递归调用，则令每个递归函数对应于一个未知的时间开销
函数T (n)，其中n是该函数参数的大小，之后列出关于T 的递归方程并
求解之。

算法的空间复杂性
•算法的空间复杂度通过计算算法所需的存储空间实现，记
作S(n)=O(f(n))，其中n为问题的规模，f(n)为语句关于n所占
存储空间的函数。
•若算法执行时所需的辅助空间相对于输入数据量而言是个
常数，则称此算法的空间复杂度为O(1)。
Data Structure & Algorithms28

算法的空间复杂性
Ø 一个算法的存储量需求除了存放算法本身所有的指令、常数、变量和
输入数据外，还包括对数据进行操作的工作单元和存储实现计算所需信
息的辅助空间。
Ø 算法的存储量需求与输入的规模、表示方式、算法采用的数据结构、
算法的设计以及输入数据的性质有关。
Ø 算法的执行的不同时刻，其空间需求可能是不同的。
Ø 算法的空间复杂性是指算法在执行过程中的最大存储量需求。
Data Structure & Algorithms29

Summary
•The measurement for algorithm. 
•Time complexity. 
•The time cost in running the 
algorithm as the scale n of the 
problem increasing, also called 
asymptotic time complexity. 
•Space complexity. 
•The storage requirement in 
running the algorithm.
Data Structure & Algorithms30
Time cost (from small to large)

Exercises
•给出下面几个程序段的时间复杂度
Data Structure & Algorithms31
•指出下面算法的时间复杂度，并给出
详细的分析过程
O(log
5
n) 
O(
퐀
)
O(n
2
)
该算法的时间复杂度主要由while循环决定，若n为偶
数，while指令只执行一次，若n为素数，则while指令
执行的次数达到最大值
퐀
-1，因此该算法最好情况的
时间复杂度为O(1)，最坏情况的时间复杂度为O(
퐀
)，
平均时间复杂度为O(
퐀
)。
