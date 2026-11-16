# ca-lec02-perf-v1

> 主题库：并行计算机体系结构　｜　源文件：ca-lec02-perf-v1.pdf　｜　37,399 字符　｜　162 个片段　｜　更新于 2026-11-11


计算机体系结构
主讲教师：胡 淼
中山大学 计算机学院
2024 年 秋季

•第 1 章 绪论
•第 2 章 基准评测集
•第 3 章 高性能计算机的体系结构
•第 4 章 高性能处理器的并行计算技术
•第 5 章 高性能计算机的存储层次
•第 6 章 高性能计算机的互连网络
•第 7 章 异构计算体系结构
•第 8 章 领域专用体系结构
教学内容

第 2 章 基准评测集

掌握：CPU性能；加速比；Amdahl定律；
熟悉：Gustafson 定律；并行算
法（程序）的设计步骤；
了解：Sun-Ni 定律；可扩展性评
测标准；基准评测集。
教学目的

课程内容
•Computer Architecture, A 
Quantitative Approach (6th Ed.), 
2017
–Chapter 1. Fundamentals of Quantitative 
Design and Analysis
•并行计算机体系结构 (第2版), 2021
–第二章 性能评测

提  纲
•（一）计算机性能
•（二）基准评测集
•（三）如何提高高性能

（一）计算机性能

计算机性能
•计算机性能
–通常指机器的速度，是程序执行时间的倒数
•程序执行时间
–Wall-clock time (response time, or elapsed time)
–指用户的响应时间 (访问磁盘和访问存储器的时间，
CPU时间， I/O时间以及操作系统的开销)
•CPU时间
–表示CPU的工作时间，不包括I/O等待时间和运行
其它任务的时间
8

The Processor Performance Equation
9
CPU time for a program can then be expressed two ways:
clock cycles per instruction (CPI):

The Processor Performance Equation
•Different instruction types having different 
CPIs
•Overall CPI
10

•It is difficult to change one parameter in 
complete isolation from others  
–Clock cycle time: Hardware technology and 
organization
–CPI: Organization and instruction set architecture
–Instruction count: Instruction set architecture and 
compiler technology
The Processor Performance Equation
11

12
硬件/软件影响什么如何影响
算法程序的执
行指令数
算法决定源程序执行指令的数目，好的算法可以大
幅度减少运算的次数
编程语言程序的执
行指令数
编程语言可能对执行指令数产生巨大的影响，比如
解释执行、即时编译或者原生编译的三类语言完成
同样的功能所需要的指令数可能有数量级的差异
编译器和
库
程序的执
行指令数、
CPI
编译器和库决定了源程序到计算机指令的翻译过程，
编译程序的效率既影响到程序的执行指令数又影响
到CPI，如Intel的ICC编译器编出来的程序，效率可
比GCC高30%，其能充分利用向量化指令和针对处
理器结构的优化
指令系统
结构
程序的执
行指令数、
CPI和时
钟频率
指令系统结构影响到CPU性能的3个方面，因为它
影响到完成某个功能所需的指令数、每条指令的周
期数，以及处理器的时钟频率
微体系结
构
CPI和时
钟频率
微体系结构的改进可以降低CPI，也可以细分流水
线来提高频率
物理设计时钟频率物理设计和电路的进步可以降低时钟周期，从而提
高时钟频率
工艺时钟频率工艺的进步使得晶体管变快从而提高时钟频率

CPU性能公式
13
•MIPS（Million Instructions Per Second）
–为了简化对于不同计算机的横向比较，一个常用的
计算机性能评价指标是每秒百万次指令数MIPS
     MIPS = I
N
  /（T
E
×10
6
）= R
C
 /（CPI×10
6
）
     其中，T
E
表示程序执行时间，R
C
 表示时钟速率，
是T
C
 的倒数
     有时还使用相对MIPS
Rel
这一标准
             MIPS
Rel
 =（T 
Ref
 / T
V
）× MIPS
Ref
   
MIPS指标忽略了不同指令系统之间的差异

计算机性能
•FLOPS（Floating-point Operations Per Second）
–由于 MIPS 指标的缺陷，来自美国劳伦斯·利弗莫尔国家实验
室的弗兰克·H·麦克马洪（Frank H. McMahon）提出了更通
用的计算机性能瓶颈指标，即每秒浮点运算次数 FLOPS
                     MFLOPS = I
FN
 /（T
E
×10
6
） 
         其中， I
FN
 表示程序中的浮点运算次数
       一般认为在标量计算机中执行一次浮点运算平均需要3条指令，
故有1MFLOPS ≈ 3MIPS
14

FLOPS
•为了方便表示，基于 FLOPS，延伸出来一系列扩展指标，
包括：
–每秒百万次浮点运算次数（Mega Floating-point Operations 
Per Second，MFLOPS，megaFLOPS，MFlop/s）
–每秒十亿次浮点运算次数（Giga Floating-point Operations Per 
Second，GFLOPS，gigaFLOPS，GFlop/s）
–每秒万亿次浮点运算次数（Tera Floating-point Operations Per 
Second，TFLOPS，teraFLOPS，TFlop/s）
–每秒千万亿次浮点运算次数（Peta Floating-point Operations 
Per Second，PFLOPS，petaFLOPS，PFlop/s）
–每秒百亿亿次浮点运算次数（Exa Floating-point Operations 
Per Second，EFLOPS，exaFLOPS，EFlop/s）
15

计算机性能
16
名  称符   号含    意单 位
机器规模
P
处理器的数目
/
时钟速率
f
时钟周期长度的倒数
GHZ
工作负载
W
计算操作的数目
Gflop
顺序执行时间
T
s 
(or T
1
)
程序在单处理机上的运行时间
s（秒）
并行执行时间
T
p 
(or T 
n
)程序在并行机（规模为n）上的
运行时间
s（秒）
速度
R 
n 
= W/T 
n
每秒十亿次浮点运算
Gflop/s
加速比
S
n
=T
1
/T
n
衡量并行机有多快
/
效率
E
n
 = S
 n
/n
衡量并行机的效率
/
峰值速度
R 
peak 
= n R
’
 
peak
所有处理器峰值速度之积，R
’
peak
为一个处理器的峰值速度
Gflop/s
利用率
U =R 
n
/R 
peak
可达速度与峰值速度之比无量纲
通信延迟
t
o
传送0-字节或单字的时间
μs（微秒）
渐近带宽
r
∞
传送长消息通信速率
MB/s

计算机性能评测的意义
•计算机性能评测的意义
–提高计算机的使用效率
–减少用户购机盲目性，降低投资风险
–改进系统结构设计，提高机器的性能
–促进软/硬件结合，合理功能划分
–优化 “结构-算法-应用”的最佳组合
–提供客观、公正的评价计算机的标准
17

计算机性能
1.机器级性能评测：
–CPU和存储器的某些基本性能指标；并行和通信开销分析；计
算机的可用性与好用性以及机器成本、价格与性/价比 
2.算法级性能评测：
–加速比、效率、可扩展性
3.程序级性能评测：
–Benchmark, 包括基本测试、数学库测试和并行测试程序等
18

机器级性能评测
19
•CPU的某些基本性能指标 
–工作负载
•执行时间 
•浮点运算数 
•指令数目 
–并行执行时间 T
comput
 为计算时间，T
paro 
为并行开销
时间，T
comm
为相互通信时间 
            T
n
 = T
comput
 + T
paro
+ T
comm 

机器级性能评测
20
•存储器的某些基本性能指标 
–存储器层次结构
–存储器带宽
寄存器
1级
高速缓存
2级
高速缓存
主存
磁盘
远程
存储器
C<2KB
L=0周期
B=1-32GB/S
4-256KB
0-2周期
1-16GB/S
64KB-4MB
2-10周期
1-4GB/S
16MB-16GB
10-100周期
 0.4-2GB/S
1-100GB
100K-1M周期
1-16MB/S
1-100GB
100-100K周期
1-300MB/S

机器级性能评测
21
•机器的成本、价格与性/价比
–机器的性能/价格比 Performance/Cost Ratio：
系指用单位代价（通常以百万美元表示）所
获取的性能（通常以MIPS或GFLOPS表示） 
–利用率（Utilization）：可达到的速度与峰值
速度之比 

算法级性能评测
22
•加速比性能定律
–并行系统的加速比是指对于一个给定的应用，并行算法
（或并行程序）的执行速度相对于串行算法（或串行程
序）的执行速度加快了多少倍
–Amdahl 定律
–Gustafson 定律
–Sun-Ni 定律
•可扩展性评测标准
–等效率度量标准
–等速度度量标准
–平均延迟度量标准

算法级性能评测
23
•加速比性能定律
–并行系统的加速比是指对于一个给定的应用，并行算法
（或并行程序）的执行速度相对于串行算法（或串行程
序）的执行速度加快了多少倍
–Amdahl 定律
–Gustafson 定律
–Sun-Ni 定律
•可扩展性评测标准
–等效率度量标准
–等速度度量标准
–平均延迟度量标准

•Speedup of computer X relative to 
computer Y
–X is n times as fast as Y
Speedup
24

•一个并行程序的加速比（Speedup）：
–T
p
：并行执行时间
–T
s
：串行执行时间
并行加速比
25
퐀=
퐀
퐀
퐀
퐀

•加速前：
•在 3 核的计算机：
•对于 p 核的高性能计算机：
线性加速比
퐀
串行
퐀
并行
=퐀
串行
 / 3
퐀
并行
=
퐀
串行
퐀
此时，并行程序拥有 线性加速比（p）
26

•效率（Efficiency）：
–加速比 S 与线性加速比 p 的比值
效率
퐀=
퐀
퐀
=
퐀
퐀
퐀
퐀
퐀
=
퐀
퐀
퐀⋅퐀
퐀
27

Amdahl定律
•Amdahl 定律
–1967 年，作为 IBM360 系列机的主要设计者，
美国计算机科学家吉恩·阿姆达尔（Gene Amdahl）
提出了 Amdahl 定律
28

Amdahl定律
•Amdahl 定律
–Amdahl 定律给出了 固定负载 条件下程序并行化
效率提升的理论上界
–在并行计算领域，Amdahl 定律常用于预测使用
多个处理器后的理论加速比上界
29

Amdahl定律
30
•P：处理器数
•W：问题规模（计算负载、工作负载，给定问题的总
计算量）
–W
s
：应用程序中的串行分量，f 是串行分量比例（f = W
s
/W）
–W
P
：应用程序中可并行化部分，1-f 为并行分量比例
–W
s
 + W
p
 = W
•T
s 
= T
1 
：串行执行时间，T
p
：并行执行时间
•S：加速比，E：效率
•出发点：
–固定不变的计算负载
–固定的计算负载分布在多个处理器上
–增加处理器加快执行速度，从而达到了加速的目的

Amdahl定律
31
•固定负载的加速公式：
•W
s
+ W
p 
可相应地表示为  [f  + (1 - f )]W
pWW
WW
S
ps
ps
/


)1(1
1
)1(






pf
p
p
f
f
ff
S

Amdahl定律
32
•固定负载的加速公式：
•p → ∞ 时，上式极限为：     S = 1 / f           
)1(1
1
)1(






pf
p
p
f
f
ff
S
加速比是有上限的

Amdahl定律
33

•除非一段串行程序的 所有部分 都可以 并行化，
否则可以达到的加速比是 非常有限 的
–不论使用多少核
•并行程序的加速比与串行执行所需要的时间无关，
只与 可并行部分的长度W
p 
和 并行系数 P 有关
Amdahl定律
34

Amdahl定律
35
•实际并行加速不仅受限于程序的，也受到
并行程序运行时额外开销的影响，令W
o
为额外开销 
•p → ∞ ？                                           
WpWpf
p
W
p
fW
fW
W
W
p
W
W
WW
S
o
oo
P
S
PS
/)1(1
)1(










Amdahl定律
36

Amdahl定律
•Amdahl 定律是适用于固定负载情况下的加
速比模型，它的基本出发点是：
–(1) 在科学计算领域，有很多问题由于规模较大
导致无法满足实时性要求，即问题求解的时间
开销是个关键因素，而问题的规模也就是负载
是固定不变的
–(2) 由于计算负载是固定不变的，因此增加参与
计算的处理器数量能够提高程序的执行速度，
达到加速的目的
37

Gustafson 定律
–Amdahl 定律：当处理器核数
增加到无穷大时，加速比会趋
近于一个上限
–但 Gustafson（古斯塔夫森）
在 1988 年进行的实际实验，
在一个拥有 1024 个处理器的
计算机，分别获得了
1021x/1020x/1016x 的加速比
38
Gustafson 得到了与 Amdahl 不一样的结论！

Gustafson 定律
39
•分析：
–对于很多大型计算，精度要求很高，即在此类应用中精度是个
关键因素，而计算时间是固定不变的。此时为了提高精度，必
须加大计算量，相应地亦必须增多处理器数才能维持时间不变
–除非学术研究，在实际应用中没有必要固定工作负载而计算程
序运行在不同数目的处理器上，增多处理器必须相应地增大问
题规模才有实际意义
•Gustafson 加速定律：
 （放大问题规模的加速公式）
•并行开销 W
o 
：
ps
ps
ps
ps
WW
pWW
pWpW
pWW
S






/
'
 p- p-f -p f  p -f p  f S' )1()1()1(

WW
fpf
WWW
pWW
S
ooPS
PS
/1
1
'







程序中顺序部分的百分比f
(c)
S'
1024
=1024-1023f
0%1%2%3%4%
1024x
1014x1004x
993x
983x
加
速
比
S
 
'
处理器数P
工
作
负
载
W
  (a)
W
1
W
1
W
1
W
1
W
1
W
1
W
p
W
p
W
p
W
p
W
p
W
p
1
23
45
6
 T
p
T
1
T
1
T
1
T
1
T
1
T
1
执
行
时
间
T
处理器数P
  (b)
1
23
45
6
 T
p
 T
p
 T
p
 T
p
 T
p
Gustafson 定律
40

Sun-Ni 定律
•基本思想（存储受限的加速定律）：
–只要存储空间许可，应尽量增大问题规模以产生更好和更精确的解
（此时可能使执行时间略有增加）
–假定在单节点上使用了全部存储容量 M 并在相应于 W 的时间内求解
之，此时工作负载W= f W + (1-f )W 
–在 p 个节点的并行系统上，能够求解较大规模的问题是因为存储容量
可增加到 pM。令因子 G(p) 反映存储容量增加到 p 倍时并行工作负载
的增加量，所以扩大后的工作负载 W = f W + (1-f )G(p)W
•存储受限的加速公式 ： 
•并行开销 W
o
:


pWpGffW
WpGffW
S
/1
1
''





ppGff
pGff
/1
1







WWppGff
pGff
WpWpGffW
pWGffW
S
oo
//1
1
/1
1
''






41

Sun-Ni 定律
•G(p) = 1 时就是Amdahl加速定律；
•G(p) = p 变为 f + p(1-f)，就是Gustafson加速定律
•G(p) > p 时，相应于计算机负载比存储要求增加得
快，此时 Sun-Ni 加速均比 Amdahl 加速和 
Gustafson 加速高
T
1
T
p
执
行
时
间
T
处理器数P
  (b)
T
1
T
1
T
1
T
1
T
1
T
p
T
p
T
p
T
p
T
p
处理器数P
工
作
负
载
W
  (a)
W
1
W
p
W
p
W
p
W
p
W
p
W
p
W
1
W
1
W
1
W
1
W
1
1
23
45
6
1
23
45
6
42

加速比讨论
–参考的加速经验公式： P/log P ≤ S ≤ P 
–线性加速比：
•很少通信开销的矩阵相加、内积运算等
–P/log P 的加速比：
•分治类的应用问题
–类似二叉树，树的同级可并行执行，但向根逐级推进时，并行度将逐渐减少
–通信密集类的应用问题 ：
•S = 1 / C(P)，其中，C(P) 是 P 个处理器的某一通信函数（线性/对数） 
–超线性加速 
•并行搜索算法 (提前取消无谓的搜索分支)
–绝对加速：最佳并行算法与串行算法相比
–相对加速：同一算法在单机和并行机的运行时间之比
43

算法级性能评测
44
•加速比性能定律
–并行系统的加速比是指对于一个给定的应用，并行算法
（或并行程序）的执行速度相对于串行算法（或串行程
序）的执行速度加快了多少倍
–Amdahl 定律
–Gustafson定律
–Sun Ni定律
•可扩展性评测标准
–等效率度量标准
–等速度度量标准
–平均延迟度量标准

可扩展性评测标准
•并行计算的可扩展性（Scalability）
–可扩展性最简朴的含意是在确定的应用背景下，计算机系统（或算法或程序
等）性能随处理器数的增加而按比例提高的能力
•影响加速比的因素：处理器数 p 与 问题规模 W
–求解问题中的
–并行处理所引起的额外开销（通信、等待、竞争、冗余操作和同步等）
–加大的处理器数超过了算法中的并发程度
•增加问题的规模有利于提高加速的因素：
–较大的问题规模可提供较高的并发度
–额外开销的增加可能慢于有效计算的增加
–算法中的串行分量比例不是固定不变的（串行部分所占的比例随着问题规模
的增大而缩小）
•增加处理器数会增大额外开销和降低处理器利用率，所以对于一个特定
的并行系统（算法或程序），它们能否有效利用不断增加的处理器的能
力应是受限的，而度量这种能力就是可扩展性这一指标
45

可扩展性评测标准
•可扩展性：调整什么 和 按什么比例调整
–并行计算要调整的是处理器数 p 和问题规模 W
–两者可按不同比例进行调整，此比例关系（可能是线性的，多项式的或指数
的等）就反映了可扩展的程度
•并行算法和体系结构
•可扩展性研究的主要目的：
–确定解决某类问题用何种并行算法与何种并行体系结构的组合，可以有效地
利用大量的处理器
–对于运行于某种体系结构的并行机上的某种算法当移植到大规模处理机上后
运行的性能
–对固定的问题规模，确定在某类并行机上最优的处理器数与可获得的最大的
加速比
–用于指导改进并行算法和并行机体系结构，以使并行算法尽可能地充分利用
可扩充的大量处理器
•目前无一个公认的、标准的和被普遍接受的严格定义和评判它的标准
46

等效率度量标准
47
•令 t
i
e 
 和 t
i
o
 分别是并行系统上第 i 个处理器的 有用计算时间 和
额外开销时间（包括通信、同步和空闲等待时间等）
•T
p
 是 p 个处理器系统上并行算法的运行时间，对于任意 i 显然
有 T
p
 = t
i
e 
+t
i
o
 ，且 T
e
+ T
o
= pT
p
  
•问题的规模 W 为最佳串行算法所完成的计算量 W = T
e
  
•如果问题规模 W 保持不变，处理器数 p 增加，开销 T
o 
增大，
效率 E 下降。为了维持一定的效率 E（0与1之间），当处理数 
p 增大时，需要相应地增大问题规模 W 的值。由此定义函数 
f
E
(p) 为问题规模 W 随处理器数 p 变化的函数，为等效率函数
（iso-efficiency function）（Kumar1987）                                                                                    




1
0
p
i
i
ee
tT




1
0
p
i
i
oo
tT
W
T
p
T
T
p
p
TT
T
T
T
S
o
e
ooe
e
p
e






11
W
T
T
T
P
S
E
o
e
o




1
1
1
1

等效率度量标准
48
•曲线 1 表示算法具有很好的扩展性；曲线 2 表示算法
是可扩展的；曲线 3 表示算法是不可扩展的
•优点：简单可定量计算的、少量的参数计算等效率函数 
•缺点：可能 T
o 
无法计算出
处理器数 P
工
作
负
载
W
曲线3
曲线2
曲线1

等效率度量标准
49
•T
o 
可能难以计算得到
–例如：非局部访问的读写时间、进程调度时间、存储竞争时
间以及高速缓存一致性操作时间等都是难以准确计算的
•因此，使用解析计算的方法来度量可扩展性不应被视为
唯一的方法
•Sun Xianhe 和 Zhang Xiaodong 两位学者于 1994 年分
别提出了以试验测试为主要手段的两种评测可扩展性的
标准
–等速度度量标准（iso-speed）
–平均延迟度量标准（average latency）

有关可扩展性评测标准的讨论
•等效率度量标准
–在保持效率 E 不变的前提下，研究问题规模 W 如何随处理
器个数 p 而变化
•等速度度量标准
–在保持平均速度不变的前提下，研究处理器个数 p 增多时应
该相应的增加多少工作量 W
•平均延迟度量标准
–在保持效率 E 不变的前提下，用平均延迟的比值来标志随处
理器个数 p 增加需要增加的工作量 W
三种度量可扩展性的标准是彼此等效的
50

有关可扩展性评测标准的讨论
•三种评价可扩展性的标准的基本出发点都是抓住了影响算法
可扩展性的基本参数 T
o
，区别在于：
–等效率指标是采用解析计算的方法得到 T
o
–等速度指标是将 T
o 
隐含在所测量的执行时间中
–而平均延迟指标则是保持效率为恒值时，通过调节 W 与 p 来测
量并行和串行执行时间，最终通过平均延迟反映出 T
o
51

可扩展性研究的主要目的
–（1）探索算法和并行架构的组合
确定解决某类问题使用何种并行算法和并行架构的组合，
更有利于利用大量的硬件资源
–（2）预测算法性能
根据某个算法在特定架构下的小规模处理器上的性能，预
测该算法移植到较大规模的处理器上后算法的运行性能
–（3）计算最大加速比
探索在固定的问题规模下，确定利用的处理器数量及其能
获得的最大加速比
–（4）指导算法改进和并行架构设计
根据拓展性指标，指导开发、研究人员改进并行算法或者
系统架构，以提高处理器的利用效率
52

•速度
–系统组件的速度（如处理器的时钟频率）
•并行性
–系统的并发度，同时能执行的操作数
•效率
–系统的利用率
–常用度量：持续浮点性能与理论峰值性能的比值
HPC架构的关键特性
53

•峰值性能 指的是超级计算机理论上在其硬件资源的支
持下能够获得的最大性能值
•峰值性能分为 理论峰值性能 和 实测峰值性能
–其中，理论浮点峰值性能是指计算机理论上能达到的每秒钟
完成浮点计算最大次数，它由 CPU 的主频、CPU 每个时钟
周期执行浮点运算的次数和系统总 CPU 核数共同决定
–理论峰值性能 = CPU 时钟频率 × CPU 每个时钟周期执行
的浮点运算次数×CPU 数量
峰值计算性能
54

–理论峰值性能适用于通用的硬件体系结构，但
忽略了内存架构及带宽、I/O 性能、缓存一致
性等因素的影响，从而无法真正反映系统的真
实峰值性能
–为此许多组织试图用一些标准的程序来测试计
算机的运算速度，用于评价计算机的实际峰值
运算能力
峰值计算性能
55

•功率
–处理器主频与功率相关
–冷却系统
•可靠性
–硬件和软件故障
–Checkpoint机制
•可编程性
–涉及系统的易用性和生产率
HPC架构的关键特性
56

（二）基准评测集

•性能基准评测就是从基准测试程序和测试
规范的角度评价和预测系统的性能 
•基准评测：
–可以帮助机构确定所需采购的超级计算机
–可以指导制造商高性能计算机系统的设计方向
–是探索 HPC 趋势的重要历史记录 
基准评测介绍
58

•The best choice of benchmarks to measure 
performance is real applications
•Examples:
–Kernels: small, key pieces of real applications.  (e.g. 
matrix multiply)
–Toy programs: 100-line programs from beginning 
programming assignments (e.g., Quicksort)
–Synthetic benchmarks: fake programs invented to try to 
match the profile and behavior of real applications (e.g., 
Dhrystone）
–benchmark suites: collections of benchmark applications  
(e.g., SPEC06fp, TPC-C)
Benchmarks
59
D
i
s
c
r
e
d
i
t
e
d
 
t
o
d
a
y
!

商用基准测试程序

•Transaction-processing (TP) benchmarks 
–measure the ability of a system to handle 
transactions that consist of database accesses and 
updates
•Transaction Processing Council (TPC): create 
realistic and fair benchmarks for TP
–The first TPC benchmark, TPC-A, was published in 
1985
–TPC-C simulates a complex query environment
–TPC-H models ad hoc decision support
–TPC-DI benchmark, a new data integration (DI) task 
–TPC-E is an online transaction processing (OLTP) 
workload that simulates a brokerage firm’s customer 
accounts
商用基准测试程序
61

商用基准测试程序
62
•TPC (Transaction Processing Performance Council) 
–TPC-A和TPC-B于1995年6月废弃
–TPC-C是目前最流行的在线事务处理（On Line Transcation 
Processing, OLTP）商用基准测试程序，能模拟一个大公司的整个销售
环境 （仓库、区域、用户、定单、各种事务的比例、响应时间），
TPC-C用于测试事务处理系统的性能与价格/性能比；
–性能结果 tpmC：描述了系统在执行Payment、Order-status、Delivery、
Stock-Level这四种交易的同时，每分钟可以处理多少个New-Order交易
•所有交易的响应时间必须满足TPC-C测试规范的要求
• 流量指标值越大越好
–和价格/性能结果＄/tpmC：结果越小越好 

商用基准测试程序
63
•TPC (Transaction Processing Performance Council) 
–各厂商的TPC-C测试结果都按TPC组织规定的两种形式发布：
•测试结果概要 (Executive Summary)
–测试结果概要中描述了主要的测试指标、测试环境示意图以及完整的系统配置与报价
•详细测试报告 (Full Disclosure Report)
–详细测试报告中除了包含上述内容外，还详细说明了整个测试环境的设置与测试过程 
–TPC-C允许被测系统放大和缩小，但终端数和数据库规模也必须按比
例调整 

SPEC 测试程序

SPEC和SPEC CPU测试程序
•SPEC是由计算机厂商、系统集成商、大学、研究机构、
咨询等多家公司组成的非营利性组织
–System Performance Evaluation Cooperative（网址: www.spec.org）
–随CPU性能提高已发展了6轮：1989、1992、1995、2000、2006、2017
–SPEC CPU2000：12个定点程序，14个浮点程序
–SPEC CPU2006：12个定点程序，17个浮点程序
–SPEC CPU2017：10个定点，14个浮点程序，分为Rate（多道程序）和
Speed（多线程）
•SPEC CPU系列测试程序集合，主要关注CPU的性能
–SPEC CPU中的SPECratio（单进程）分值主要反映PC系统性能
–衡量服务器的吞吐率通常采用SPECrate，在多CPU执行SPEC CPU的多份
拷贝，并且把CPU的时间转换为比率（rate），即SPECrate
65

66
SPEC CPU2000测试程序套件

67
SPEC CPU2006测试程序套件
SPEC CPU2000 Floating Point Benchmarks
410.bwavesFortranFluid Dynamics 
416.gamessFortranQuantum Chemistry 
433.milcCPhysics: Quantum Chromodynamics
434.zeusmpFortranPhysics / CFD
435.gromacsC/FortranBiochemistry/Molecular Dynamics 
436.cactusADMC/FortranPhysics / General Relativity 
437.leslie3dFortranFluid Dynamics 
444.namdC++Biology / Molecular Dynamics 
447.dealIIC++Finite Element Analysis 
450.soplexC++Linear Programming, Optimization 
453.povrayC++Image Ray-tracing 
454.calculixC/FortranStructural Mechanics 
459.GemsFDTDFortranComputational Electromagnetics 
465.tontoFortranQuantum Chemistry 
470.lbmCFluid Dynamics 
481.wrfC/FortranWeather Prediction 
482.sphinx3CSpeech recognition 
SPEC CPU2000 Integer Benchmarks
400.perlbenchCPERL Programming Language 
401.bzip2CCompression 
403.gccCC Compiler 
429.mcfCCombinatorial Optimization 
445.gobmkCArtificial Intelligence: go
456.hmmerCSearch Gene Sequence
458.sjengC
Artificial Intelligence: 
chess
462.libquantumCPhysics: Quantum Computing 
464.h264refCVideo Compression
471.omnetppC++Discrete Event Simulation 
473.astarC++Path-finding Algorithms 
483.xalancbmkC++XML Processing 

SPEC CPUint 2017测试程序套件
SPECrate 2017
定点程序
SPECspeed 2017
定点程序
语言描述
500.perlbench_r600.perlbench_sC
文本处理，Perl解释器
502.gcc_r602.gcc_sC
编译，GNU C编译器
505.mcf_r605.mcf_sC
组合和优化，求解车辆调度问题
520.omnetpp_r620.omnetpp_sC++
计算机网络，离散事件模拟
523.xalancbmk_r623.xalancbmk_sC++
通过XSLT将XML转换为HTML
525.x264_r625.x264_sC
视频压缩，x264视频编解码
531.deepsjeng_r631.deepsjeng_sC++
人工智能，下棋程序，alpha-beta树搜索
541.leela_r641.leela_sC++
人工智能，下棋程序（go），蒙特卡洛树搜索
548.exchange2_r648.exchange2_sFortran
人工智能，9x9数独，递归方式求解
557.xz_r657.xz_sC
压缩和解压缩，xz压缩程序
两个子测试集：
rate: 多个拷贝多核运行
speed: 单个拷贝，但代码针对openMP自动并行化做了修改 

SPEC CPUfp 2017测试程序套件
SPECrate 2017
浮点程序
SPECspeed 
2017
浮点程序
语言描述
503.bwaves_r603.bwaves_sFortran
计算流体动力学，爆炸建模
507.cactuBSSN_
r
607.cactuBSSN
_s
C++, C, 
Fortran
广义相对论和数值相对论，求解真空中的爱因斯坦方程
508.namd_r C++
结构生物学，模拟大规模的生物分子系统
510.parest_r C++
分子医学成像，光学层析成像问题的有限元求解器
511.povray_r C++, C
计算机可视化，光线追踪应用POV-Ray
519.lbm_r619.lbm_sC
流体动力学
521.wrf_r621.wrf_sFortran, C
天气预报建模，基于新一代中尺度数值天气预报系统WRF
526.blender_r C++, C
三维渲染和动画，基于开源的三维制作套件Blender
527.cam4_r627.cam4_sFortran, C
大气环流建模，地球系统模型CESM中的大气建模部分
 628.pop2_sFortran, C
大规模海洋建模（气候层面），地球系统模型CESM中的海洋
建模部分
538.imagick_r638.imagick_sC
图像处理软件包ImageMagick中convert部分
544.nab_r644.nab_sC
分子动力学，基于生命科学计算领域中分子建模应用NAB（核
酸构建器）
549.fotonik3d_r649.fotonik3d_
s
Fortran
计算电磁学，利用时域有限差分方法计算光子波导的透射系数
554.roms_r654.roms_sFortran
区域海洋建模，基于区域海洋建模系统ROMS

•Summarize performance results of the suite in a unique 
number
–(1) arithmetic means of execution times of programs in the suite
–(2) the weighted arithmetic mean 
–(3) normalize execution times to a reference computer 
•SPECRatio 
–A ratio by dividing the time on the reference computer by the 
time on the computer being rated
–For example, suppose that the SPECRatio of computer A on a 
benchmark is 1.25 times as fast as computer B
Summarizing Performance Results
70

SPEC CPU分值计算
•SPECratio是被测机器执行时间除以参考机器执行时间
–SPECratio
A
 = Execution time
reference
 / Execution time
A
•综合测试结果是取SPECratio的几何平均值
71

SPEC CPUint2006的IPC值
72
•Intel Xeon（2.66GHz）的 SPEC CPU2006 定点分值
•IPC 在 0.38 到 2 之间（5个小于1.0，四个大于 1.5）
•SPEC CPU 分值是一个相对比值，IPC 高分值不一定高

龙芯 3A5000 处理器性能分析实例
（SPEC CPU数据）

性能分析和测试实例
•分析基于 GS464V 的龙芯 3A5000 处理器的性能特性，并和 AMD 的
Zen1 以及 Intel 的 Skylake 处理器进行对比
•3A5000：GS464V，主频 2.5 GHz，内存 DDR4 3200，16MB LLC
•比较时其它处理器主频均设置为 2.5 GHz，内存为 DDR4 3200 或者 2400
74

基本参数和微结构参数
厂商
LoongsonAMDIntel
处理器型号
3A5000r3 1200i3 9100f
上市时间
202120172019
工艺
12nm14nm14nm
指令集
LoongArchX86-64X86-64
微结构型号
GS464VZen1SkyLake
频率
2.5GHz2.5GHz2.5GHz
内存类型和频率
DDR4 3200MHzDDR4 3200MHz
 
DDR4 2400MHz
Cache层次
64KB L1 ICache
64KB L1 DCache
256KB L2 Cache
16MB L3 Cache
64KB L1 ICache
32KB L1 DCache
512KB L2 Cache
8MB L3 Cache
32KB L1 ICache
32KB L1 DCache
256KB L2 Cache
6MB L3 Cache
核心队列和重命名
寄存器数量
128项ROB，64项load队
列，48项store队列，32
项分支队列，32项定点、
32项浮点和32项访存保留
站，128项定点和128项浮
点重命名寄存器
192项ROB，72项load队列，
44项store队列，84项定点
和96项浮点保留站，168项
定点和160项浮点重命名寄
存器
224项ROB，72项load队
列，56项store队列，97
项统一保留站，180项定
点和168项浮点重命名寄
存器
功能部件数4个定点，2个访存部件，
2个256位的浮点乘加
点
4个定点部件，2个访存部件，
4个128位浮点部件（其中2
个FMA/FMUL，2个FADD）
4个定点、3个访存、3个
256位浮点乘加

SPEC CPU2006的分值对比
•3A5000和Zen1、Skylake的SPEC CPU2006分值对比
76
SPEC CPU2006
loongson 
3A5000
zen1 r3-
1200
skylake i3 
9100f
400.perlbench29.131.435.3
401.bzip217.318.519.2
403.gcc23.930.838.1
429.mcf27.127.637
445.gobmk25.620.121.7
456.hmmer39.246.254.7
458.sjeng22.417.822.5
462.libquantum78.8141123
464.h264ref3845.151
471.omnetpp18.117.221.2
473.astar19.315.615.8
483.xalancbmk28.725.936.9
SPECint200627.8729.0533.47
SPEC CPU2006
loongson 
3A5000
zen1 r3-
1200
skylake i3 
9100f
410.bwaves54.499.185.2
416.gamess22.928.430.2
433.milc1838.832.3
434.zeusmp255156.2
435.gromacs15.325.823.5
436.cactusADM84.8135228
437.leslie3d36.248.262.8
444.namd20.721.523.8
447.dealII39.848.746.7
450.soplex28.534.639.8
453.povray39.134.241.5
454.calculix17.629.230.1
459.GemsFDTD35.265.156.7
465.tonto28.425.237.2
470.lbm28.867.574.6
481.wrf36.553.965
482.sphinx334.140.648.4
SPECfp200630.2943.8548.34

SPEC CPU测试数据的初步分析
•通过SPEC CPU分值了解整体的性能概况
–3A5000和Zen1同频定点性能基本相当（差4%），浮点差距
比较明显(约44%)
–skylake整体表现最好
–结合微结构参数，可推测浮点的性能差距与功能部件数和队列
大小有关系
•通过分析具体程序的行为寻找优化方向
–不同程序性能对比有较大差异
–SPEC 2006中462.libquantum, Zen1比3A5000快78.9%：经
分析和自动向量化有关，关闭自动向量化差距缩小到16.5%，
提示3A5000的编译器自动向量化有待改进
–深入分析应结合微结构参数和进一步的细节数据进行，可通过
仿真平台验证推测

动态指令数目对比(SPECint2006)
•456.hmmer 3A5000指令数显著多于另外两款
处理器，其性能差距也相对较大
78
SPEC CPU2006loongson 3A5000zen1 r3-1200
Skylake i3 
9100f
zen1/3A5000skylake/3A5000
400.perlbench2,120,6681,893,3151,875,82589.3%88.5%
401.bzip22,598,4562,230,2102,105,91785.8%81.0%
403.gcc918,503720,416637,26278.4%69.4%
429.mcf270,116273,830279,889101.4%103.6%
445.gobmk1,447,7581,364,9251,349,34694.3%93.2%
456.hmmer1,725,9411,293,2361,140,94474.9%66.1%
458.sjeng2,448,0772,256,0352,259,54492.2%92.3%
462.libquantum1,287,3451,354,3491,355,304105.2%105.3%
464.h264ref3,906,0223,057,4432,574,50378.3%65.9%
471.omnetpp455,925515,436535,127113.1%117.4%
473.astar804,288862,191855,804107.2%106.4%
483.xalancbmk845,446857,273858,951101.4%101.6%
SPECint_200618,828,54416,678,65915,828,41588.6%84.1%

动态指令数目对比（SPECfp2006）
•436.cactusADM skylake指令数只有另两款不到一半
–它自动采用256位向量，Zen采用128位
79
SPECint_200618,828,54416,678,65915,828,41588.6%84.1%
410.bwaves867,049643,946563,80574.3%65.0%
416.gamess6,326,7954,818,9004,737,21576.2%74.9%
433.milc495,564573,026562,614115.6%113.5%
434.zeusmp1,255,758901,519682,46071.8%54.3%
435.gromacs1,523,9191,606,5881,621,933105.4%106.4%
436.cactusADM1,059,689913,079460,97686.2%43.5%
437.leslie3d586,968663,348350,522113.0%59.7%
444.namd1,602,8451,630,1031,627,678101.7%101.5%
447.dealII1,078,3541,048,5601,029,96997.2%95.5%
450.soplex588,538562,087535,32095.5%91.0%
453.povray715,602816,008838,778114.0%117.2%
454.calculix1,941,7781,615,7461,473,99783.2%75.9%
459.GemsFDTD773,507832,552471,723107.6%61.0%
465.tonto2,028,7922,212,7991,674,174109.1%82.5%
470.lbm877,312817,999821,75493.2%93.7%
481.wrf1,437,760953,405794,09566.3%55.2%
482.sphinx32,229,0291,850,5501,867,71483.0%83.8%
SPECfp_200625,389,25922,460,21320,114,72688.5%79.2%

IPC的对比（SPEC CPU2006）
•3A5000定点IPC略高于Zen1，略低于Skylake;
浮点低于另两者
80
SPEC 
CPU2006
loongson 
3A5000
zen1 
r3-1200
skylake 
i3 
9100f
zen1/3A50
00
skylake/3A5
000
400.perlbench2.542.452.7296.5%107.1%
401.bzip21.861.731.6893.0%90.3%
403.gcc1.11.11.22100.0%110.9%
429.mcf0.320.330.45103.1%140.6%
445.gobmk1.421.051.1273.9%78.9%
456.hmmer2.972.572.6886.5%90.2%
458.sjeng1.811.331.6873.5%92.8%
462.libquantu
m
1.421.671.88117.6%132.4%
464.h264ref2.692.52.3892.9%88.5%
471.omnetpp0.540.570.73105.6%135.2%
473.astar0.890.790.7788.8%86.5%
483.xalancbm
k
1.41.31.8492.9%131.4%
SPECint_rate
2006
1.341.241.4092.9%105.0%
410.bwaves1.421.91.41133.8%99.3%
416.gamess2.972.822.9294.9%98.3%
433.milc0.40.970.81242.5%202.5%
434.zeusmp1.392.011.68144.6%120.9%
435.gromacs1.312.322.13177.1%162.6%
436.cactusADM1.791.721.4496.1%80.4%
437.leslie3d0.911.380.93151.6%102.2%
444.namd1.661.751.94105.4%116.9%
447.dealII1.521.791.7117.8%111.8%
450.soplex0.840.941.02111.9%121.4%
453.povray2.062.122.6102.9%126.2%
454.calculix1.662.32.16138.6%130.1%
459.GemsFDTD0.881.270.82144.3%93.2%
465.tonto2.332.272.5397.4%108.6%
470.lbm0.741.621.79218.9%241.9%
481.wrf1.671.951.73116.8%103.6%
482.sphinx31.521.551.85102.0%121.7%
SPECfp_rate200
6
1.341.731.62129.7%121.0%

分支误预测率对比(SPEC 
CPU2006)
•整体分支误预测率均已较低，3A5000还存在一定提升空间
81
SPEC CPU2006
loongson 
3A5000
zen1 r3-
1200
skylake 
i3 9100f
zen1/3A5000skylake/3A5000
400.perlbench1.18%1.10%0.71%0.93 0.60 
401.bzip25.38%5.08%4.94%0.94 0.92 
403.gcc1.52%1.31%1.03%0.86 0.68 
429.mcf5.62%3.57%3.86%0.64 0.69 
445.gobmk8.29%9.80%8.56%1.18 1.03 
456.hmmer1.12%0.66%0.65%0.59 0.58 
458.sjeng4.47%5.89%4.17%1.32 0.93 
462.libquantum0.79%0.21%0.09%0.27 0.11 
464.h264ref2.09%1.64%1.69%0.78 0.81 
471.omnetpp2.53%1.65%1.83%0.65 0.72 
473.astar13.60%12.03%12.75%0.88 0.94 
483.xalancbmk0.43%0.51%0.34%1.19 0.79 
SPECint_20062.48%1.97%1.64%0.79 0.66 
410.bwaves0.09%0.25%0.15%2.78 1.67 
416.gamess0.94%1.07%0.69%1.14 0.73 
433.milc6.58%0.40%0.23%0.06 0.03 
434.zeusmp1.38%0.95%0.12%0.69 0.09 
435.gromacs7.04%6.19%6.11%0.88 0.87 
436.cactusAD
M
0.33%1.49%0.17%4.52 0.52 
437.leslie3d0.33%1.66%0.22%5.03 0.67 
444.namd4.66%4.38%4.52%0.94 0.97 
447.dealII2.48%2.31%2.06%0.93 0.83 
450.soplex5.51%4.22%4.40%0.77 0.80 
453.povray1.86%1.30%0.56%0.70 0.30 
454.calculix3.23%2.80%3.02%0.87 0.93 
459.GemsFDT
D
0.29%0.40%0.10%1.38 0.34 
465.tonto1.20%0.93%0.91%0.78 0.76 
470.lbm0.46%0.45%0.38%0.98 0.83 
481.wrf1.08%0.51%0.24%0.47 0.22 
482.sphinx32.35%1.80%1.88%0.77 0.80 
SPECfp_20061.30%1.24%0.65%0.95 0.50 

STREAM 带宽的对比
STREAM四核
(openMP)
3A5000Zen1Skylake
Copy23860.939896.426983.3
Scale22347.325073.319110.9
Add19323.029768.521516.0
Triad21043.829146.821490.7
3A5000和Zen1、Skylake的STREAM带宽对比（MB/s）

功能部件的操作延迟
•相关数据需要进一步分析
83
lat_ops延迟（ns）
loongson 
3A5000
zen1 r3-1200
skylake i3 
9100f
integer bit0.270.290.27
integer add000
integer mul0.020.021.24
integer div6.218.0310.86
integer mod3.605.6111.32
int64 bit0.270.290.27
int64 add000
int64 mul0.020.021.22
int64 div7.6010.6317.10
int64 mod3.345.3416.55
float add2.001.201.60
float mul2.001.201.61
float div10.824.044.58
double add2.001.201.60
double mul2.001.601.60
double div9.225.255.78
float bogomflops5.041.531.20
double bogomflops9.621.811.61

HPC 基准评测集

•计算性能评测集：Linpack，HPCG，
Graph500
•IO性能评测集：MDTest，IOR，IO500
•网络性能评测集：IMB，OSU Benchmark
•能耗评测集：Green 500
•应用评测集：Miniapplication、戈登·贝尔奖
常见HPC基准评测分类
85

Linpack
86
•杰克·唐加拉（Jack Dongarra，1950年7月
18日－）美国计算机科学家，田纳西大学
电气工程与计算机科学系特聘教授，美国
国家工程院院士，英国皇家学会外籍院士
•2021年，杰克·唐加拉因开创性的概念和方
法获得了图灵奖，这些概念和方法导致了
改变世界的计算。他的算法和软件被认为
推动了高性能计算的发展，并在人工智能
的计算科学到计算机图形学的许多领域产
生了重大影响
杰克·唐加拉
（Jack Dongarra）

•Linpack 是度图灵奖得主美国计算机科学家杰克·唐加拉
（Jack Dongarra）等几位学者于 1979 年开发的线性系统
软件库（Linear system package，Linpack）
•Linpack 设计之初的目标是集成常用的线性方程组求解程
序（例如最小平方问题、稠密矩阵运算）至一个通用的线
性代数软件库，支持求解大部分常见的线性代数问题
•Linpack 评测基准在 HPC 计算性能评测领域发挥了强大的
作用
Linpack
87

•用来度量系统的浮点计算能力
•Linpack性能是指求解双精度线性代数方
程组时所达到的实际性能
–包括Linpack100, Linpack1000, HPL
–Linpack100 和 Linpack1000 分别求解规模为 
100 阶和 1000 阶的线性代数方程组
–HPL是针对现代的高性能计算机的评测基准
•评测指标：每秒浮点运算次数（flops） 
Linpack
88

•Linpack100 使用 Fortran 中的 Linpack 软件求解规模为 100 阶
的稠密线性代数方程组
•为了运行该程序，需要提供一个称为 SECOND 的计时函数，
报告已经过去的 CPU 时间
•运行此基准评测的基本规则是不能对 Fortran 代码进行任何更
改，甚至不允许更改注释，只允许采用编译优化选项进行优化
Linpack100
89
http://www.netlib.org/benchmark/Linpackd

•Linpack1000 要求求解规模为 1000 阶的线性代数方程组，在
达到指定的精度要求下，可以在不改变计算量的前提下做算法
和代码上做优化
•运行此基准评测的基本规则要宽松一些，可以指定希望求解的
任何线性方程，用任何语言实现
•必须满足要求是必须计算出一个解决方案，并且该解决方案必
须返回一个达到规定精度的结果
Linpack1000
90
http://www.netlib.org/benchmark/1000d

•HPL 具体为求一个 n 维的线性方程组的解：
퐀퐀=퐀;퐀∈ℝ
퐀×퐀
;퐀,퐀∈ℝ
퐀
•对n×(n+1)的系数矩阵 [퐀 퐀] 进行LU分解：
 
퐀,퐀
 
= 
 
퐀찀
 
,퐀 
•方程组的解 퐀转化为求解三角矩阵 찀作为系数矩
阵的线性方程组 찀퐀=퐀而得到
HPL 是国际超级计算系统 TOP500 的重要依据
HPL
91

HPL
92
如图(a)所示，矩阵 퐀 首先划分为 퐀
퐀
 = 4 × 4 个数据块，之后所
有 퐀
퐀
 个数据块映射到数目为 퐀 × 퐀 = 2 × 2 的进程阵，如图(b)
所示
•HPL 测试的目标是尽可能地使得高性能计算机对矩阵 퐀 的不同部分并行
地做出更新，这就需要对矩阵进行分块
–퐀 为最高 GFLOPS 值的矩阵规模、퐀
퐀
 为求解矩阵的分块大小
–퐀为处理器网格中水平方向处理器个数、퐀 为处理器网格中垂直方向处理器
个数

Top500排行榜 （2024年6月）
93
HPL 是国际超级计算系统 TOP500 的重要依据

LINPACK
•Linpack 用户手册可以从 http://www.netlib.org/lapack/lug/ 
下载得到
•Linpack100 测试程序可以从
http://www.netlib.org/benchmark/linpackd 下载得到
•Linpack1000 测试程序可以从
http://www.netlib.org/benchmark/1000d 下载得到
•HPL 采用 C 语言进行编写的测试程序可以从
http://www.netlib.org/benchmark/hpl 下载得到
94

HPCG
95
•实际应用中存在大量用偏微分方程建模，
稀疏计算和不规则的访存模式，无法用
Linpack衡量
•HPCG(高性能共轭梯度法)
–作为Linpack的补充，是求解稀疏矩阵方程组
的一种迭代算法

•HPCG（高性能共轭梯度，High-Performance Conjugate 
Gradient）基准测试程序于 2000 年首次发布，代码采用 C++ 
语言编写
•HPCG 首次发布于 2013 年 11 月举办的世界超算会议（SC 
2013）
•HPCG 基准评测方法目的是适应目前大部分应用程序所采用的
计算和数据访问模式, 最终促进计算机性能的整体提升
HPCG
96

•HPCG模拟三维热力学运动问题，从而转
化为求解离散的三维偏微分方程模型问题
–使用局部对称高斯·塞德尔（Local 
Symmetric Gauss-Seidel, LSGS）预条件子
的预处理共轭梯度法
–主要数据为对称正定稀疏矩阵
HPCG
97

•三维热力学运动问题转化为求解离散的三维偏微分方程模型问题步骤：
–网格划分：将热力学问题转化为离散的三维空间网格。根据实际情况，可以
选择均匀网格或非均匀网格
–离散化：根据具体的热力学方程，将偏微分方程转化为差分方程。例如，可
以使用有限差分法或有限元法来离散化方程
–边界条件：根据实际问题，在离散的网格上设置正确的边界条件。这些条件
用于描述问题的物理特性
–代数方程组：将离散化的方程整理为矩阵形式，并构建一个稀疏线性方程组。
方程组的未知数通常是网格点上的温度或其他物理量
–解方程组：使用HPCG或其他合适的求解器，通过迭代方法（如共轭梯度法）
求解离散方程组。这涉及到矩阵向量乘法和共轭梯度法的实现
–并行计算：为了提高性能，利用并行计算技术（如MPI或OpenMP）将计算任
务分配给多个处理器或多个计算节点
三维热力学运动问题转化
98

•局部对称高斯-塞德尔（LSGS）预条件子是一种用于预处理共轭梯度法（Conjugate Gradient, 
CG）的方法。该方法通过将原始线性系统的系数矩阵分解为局部块矩阵，并使用高斯-塞德尔迭代
来逐块求解。LSGS预条件子在求解稀疏线性系统时能够有效地加速CG方法的收敛速度。
•以下是LSGS预处理共轭梯度法的基本步骤：
–定义原始线性系统：将原始线性系统表示为Ax=b的形式，其中A是对称正定的稀疏矩阵，x是解向量，
b是右侧向量。
–分解系数矩阵：将系数矩阵A分解为块对角矩阵形式。例如，将A分解为D - L - U的形式，其中D是主
对角线矩阵，L是严格下三角矩阵，U是严格上三角矩阵。
–进行预处理：采用高斯-塞德尔迭代来对分解后的系数矩阵进行预处理。高斯-塞德尔迭代通过使用前
一步求解得到的分量来更新当前分量，直到达到所需的精度。对于块矩阵来说，每个块的更新是独立
进行的。
–执行共轭梯度法迭代：使用预处理后的分解系数矩阵（块对角矩阵）和预处理后的右侧向量进行共轭
梯度法迭代求解。
•LSGS预条件子的主要优点是它能够利用块矩阵的局部性质和高斯-塞德尔迭代的迭代更新来加速共
轭梯度法的收敛。然而，LSGS预条件子也有一些限制，如对块矩阵的分解和求解过程的复杂性，
以及在某些情况下可能会受到块矩阵形状和特征值分布的影响。
LSGS 预条件子
99

•在HPCG基准测试程序中，稀疏线性方程组是通过配置文件中的参数来构建
的。常见的参数设置及其含义：
–网格维度：通过nx, ny, nz参数设置三维网格的大小
•这决定了方程组中未知数的个数
–网格划分：通过px, py, pz参数设置三维网格的划分
•这定义了在并行计算中网格划分的方式
–通信模式：通过cg_iter, symmetry, overlap, global_mpi等参数设置通信模式
•这些参数决定了通信方式、是否引入对称性等
–矩阵类型：通过exec_set参数设置矩阵类型，如laplacian或elasticity
•这决定了稀疏矩阵的结构
–非零元素密度：通过toc参数设置非零元素密度
•这决定矩阵的稀疏程度
•通过这些参数，HPCG基准测试程序会根据矩阵类型和非零元素密度生成一
个稀疏线性方程组，其中未知数是网格点上的温度或其他物理量
HPCG
100

•HPCG基准测试可以提供以下指标：
–HPCG得分：作为一个综合评估指标，HPCG基准测试提
供一个HPCG得分来度量计算机的整体性能
•这个得分反映了计算机在求解稀疏线性系统方面的速度和效率
–迭代次数：HPCG基准测试可以提供求解稀疏线性系统所
需的迭代次数
•通过迭代次数，可以了解到计算机的计算能力和收敛速度
–内存带宽：HPCG基准测试是一个内存密集型的应用程序，
通过测试可以了解计算机在高速缓存和主存之间的数据传
输效率
HPCG
101

•HPCG 在利用差分方程迭代的过程中，采用
了全局通信和邻近进程数据的通信方式以及向
量更新、点乘、稀疏矩阵向量乘法和局部三角
求解器等计算模式，覆盖了高性能计算领域中
常见的计算操作
•对于未来计算机系统，可以通过异步通信以及
能够降低通信延迟的相关技术来改进 HPCG 
的性能
HPCG
102

•需要注意的是，HPCG要求：
–首先，问题规模所需占用的内存大小至少占系统总内存的
四分之一以上
•这主要是由于问题规模太小会导致数据访存开销以及访存不规
则不能体现
–其次，官方还禁止用户改变预处理器的设置，保证系统测
试的合理性
•禁止包括修改数据在内存中的存储长度等修改数据的存储格式
的操作，以及禁止通过修改算法来减少通信开销的操作
HPCG
103

104
HPCG 弥补了 Linpack 存在的不足，促使计算机系统的设计不
仅仅只专注于计算能力的发展，还要向访存、通信这些方面发展

•美国桑迪亚国家实验室（英语：Sandia 
National Laboratories，SNL）的理查
德·墨菲（Richard Murphy）等多名研究人
员在 2010 年举办的国际超算会议上提出
了 Graph500 评测基准
•不同于 Linpack 与 HPCG基准评测集，
Graph500 旨在评测高性能计算机对复杂
数据的处理性能，它侧重于系统的通信子
系统，而不再专注于计算性能
•在大数据信息时代下，图遍历是一种典型
的数据密集型应用，也是大数据科学的研
究重点，Graph500 基准评测是在图遍历
的基础上设计出来的
Graph500
105
理查德·墨菲
Richard Murphy
美国桑迪亚国家实验室
Sandia National Laboratories

Graph500
106
•Graph500用来衡量计算机在处理数据密集型应用
的能力
–利用图遍历中广度优先搜索（BFS）算法或单源点最短
路径（SSSP）算法
•Graph500评测流程：
图数据生成               
图遍历验证
         结果计算与输出

Graph500
107
•Graph500 基准评测首先会构建一个大的图并且遍历这个图，记录构建
表与遍历表所需要的时间
•Graph500 基准评测程序按照问题的规模划分为 toy、mini、small、
medium、large 和 huge 六个等级，这些也被称为等级 10-15，其中等
级 10 是 toy 等级，等级 15 是 huge 等级

Graph500
108
•图遍历过程采用 BFS 或者是 
SSSP 作为核心搜索程序，随机
选择一个根节点作为源点进行遍
历并检查保存的结果与原来的生
成图是否匹配
–程序随机生成 64个不同的源点，
因此这个遍历过程循环执行 64 
次（如果节点数少于 64 个，则
执行小于 64 次的循环遍历），
并进行计时

Graph500
109
•为了比较各种体系结构、编程模型、语言以及框架的性能，
Graph500 的性能指标通常定义为：每秒遍历的边数
（Traversed Edges Per Second，TEPS）
–TEPS 由生成图的边数和核心搜索程序算法的耗时比值得到
–在访问搜索根源点之前立即开始对搜索时间计时，将输出结果写入
内存后，停止计时

•按照重要性的顺序，Graph500 基准评测的目标是：
–公平遵守基准规范的意图
–给定机器的最大问题大小
–给定问题大小的最短执行时间
•其他次要目标还包括：
–最小代码量（不包括验证码）
–最少的开发时间
–最大的可维护性
–最大的可扩展性
Graph500
110

111

112

113

114

•MDTEST：MDTEST是一个广泛使用的文件和目录创建、访问
和删除的基准测试工具。
•IOR：IOR (Interoperability of I/O Ribbons)是一个设计用于测
量并行文件系统和存储系统I/O性能的基准测试工具。
•IO500：IO500是一个综合性的基准测试套件，旨在评估高性能
计算环境下文件系统和存储系统的性能。
评估文件系统和存储性能
115

MDTest
116
•MDTest 用于评估文件系统的元数据性能的
基准评测
–该程序通过在一组机器（通常是 HPC 集群中
的计算节点）上并行创建、统计和删除目录树
和文件树来评测 IO 性能
• 评测指标：每秒操作数（OP/秒）
MDTest 测试程序可以通过https://sourceforge.net/projects/mdtest/下载得到

IOR
117
•IOR可用于使用多种 IO 接口 (例如 POSIX, 
MPI-IO, HDF5 等) 和访问模式来测试并行
文件系统的性能
–通过接收参数，在客户端上产生特定的工作负
载从而测试系统的 IO 性能并输出评测结果
–评测结果中带宽是通过传输的数据量除以停止
时间戳与开始时间的差值得到

IO500
118
–由于测试方法、工具、参数甚至测试步骤的先
后顺序不同，不同厂商发布的 IO 性能测试结
果具有很大的差异性
–IO500可以对高性能存储系统进行标准的测试
和比较

IO500
119
•从评测方法上，IO500 主要分为两组评测：
–第一组评测的是高性能计算机 I/O 的理想性能，通过大文件的读
写等方式来测评计算机存储系统的性能上限
•理想性能评测包括两类，即 IOR easy 和 MDTest easy
•理想性能评测能够有效地激励世界各大存储厂商不断地提升系统的极限
性能
–第二组评测的是高性能计算机 I/O 的极限性能，通过海量小文件
的读写来测评计算机存储系统的性能极限
•极限性能评测包括两类，即 IOR hard 和MDTest hard
•IO500 最终分数是IOR分数和MDTest分数的几何平均值

120

IMB
121
•用于评估HPC集群在不同消息粒度下节点间点对点、
全局通信的效率
–点对点通信
•评测测试的是两个进程间的消息传递
–包括了 Ping-Pong 和 Ping-Ping 测试
–并行通信
•评测测试的是全局负载下消息的收发效率
–包括 Sendrecv 和 Exchange测试
–群体通信
•评测测试一对多或者是多对一的消息传递

OSU Benchmark
122
•由Ohio State University提供
–程序生成不同规模的数据并执行各种不同模式
的MPI通信，测试各种通信模式的延迟和带宽
•延迟测试采用前面提到的 Ping-Pong 的方式
•带宽测试分为点对点通信和组通信两种形式

Green500
123
•Green500 提供高性能计算机的能耗排名
–评测指标：使用 PPW（performance per watt）
每瓦特性能作为其指标来对能源效率进行排名
•单位为 GFLOPS Per Watt

•直接测量方法：
–直接测量方法通过使用功率计或电流传感器直接测量超级计算机的电力输入。
功率计可以接入电源线路来测量电流和电压，并计算功率。这种方法可以提
供较准确的功率测量结果，但需要考虑电流和电源的稳定性。
•间接测量方法：
–间接测量方法通过测量超级计算机的其他物理参数来间接估计功率。例如，
可以使用温度传感器测量系统的温度，并根据热传导定律和热功率模型推导
出功率。
•硬件监控方法：
–硬件监控方法使用超级计算机中的硬件监控功能来获取功耗数据。
•软件监控方法：
–软件监控方法使用特定的性能监测工具和软件来估计功耗。这些工具可以通
过监测处理器利用率、内存访问模式和其他负载信息来推断功耗。
功率测量方法
124

功率测量方法
125
    超级计算机上单个单元的功率测量图
•GFLOPS 퐀ఀ퐀 퐀⠀퐀퐀=
퐀
퐀퐀퐀
(퐀堀 GFLOPS)
퐀(퐀
퐀퐀퐀
)(퐀堀 퐀⠀퐀퐀)
•퐀
(
퐀
퐀퐀퐀
)
=퐀∙퐀
퐀퐀퐀퐀
 (퐀
퐀퐀퐀
)

–任何功率测量方法都有一定的误差范围，并考虑到测量不确定性。
–考虑到超级计算机的整体功耗，包括计算节点、网络和存储等各
部分的能耗。
–定期进行功率测量，以监控和评估超级计算机的能源效率，并在
必要时进行调整和优化。
•总之，超级计算机的功率测量需要综合考虑硬件和软件监控方
法，以获得更准确的数据，并用于评估和优化能源效率。
功率测量方法
126

127

Miniapplication
128
•评估超级计算机对于 动态应用程序的性能
–是真实应用程序的更小版本
–既足够复杂，又足够小，可以探索内存、网络、加速器和处理器元素的参数
和交互空间
•Mantevo 集包含大量应用领域的 开源 Miniapplication
–MiniAMR：用于探索自适应网格细化和动态执行的小型应用程序
–MiniFE：有限元代码的小型应用程序，提供了对多核节点上进行计算的支持
–MiniGhost：有限差分小型应用程序，可在均匀三维域上实现有限差分
–MiniMD：基于分子动力学工作负载的小型应用程序
–Cloverleaf：使用显式的二阶精确方法来求解可压缩欧拉方程
–TeaLeaf：求解线性热传导方程工作负载的小型应用程序

•戈登贝尔奖（ACM Gordon Bell Prize）：超算界诺贝尔奖
–美国计算机协会设立于1987年，每年颁发
–是一种超级电脑应用软件设计奖，奖金象征性1万美元，由
Gordon Bell提供
–通常会由当年前500排行名列前茅的超级电脑系统之上所跑的的应
用软件获得
•奖项通常分为：
–最高性能奖 (Peak Performance)
–最高性价比奖(Price/Performance)
–特别奖 (Special Achievement)
戈登贝尔奖
129

•戈登·贝尔（Gordon Bell）
–1934年8月19日出生于美国密苏里州的柯克斯维尔
–1956年，获MIT电子工程学士学位
–1957年，获MIT电子工程硕士学位
–1960年-1983年，在DEC任副总裁，负责研发
–1983年7月，合伙创办核心(Encore)计算机公司
–1986年，全美科学基金会(NSF)计算机及信息科学和工程
助理主任
–1991年-1995年，担任微软公司顾问
–1995年8月-至今，微软湾区研究中心高级研究员
•作为DEC的技术灵魂，构思、设计和主持开发的超级
计算机PDP-4，PDP-5，PDP-6，PDP-8，PDP-10
及PDP-11
Gordon Bell
130
戈登·贝尔
（Gordon Bell）

•2016年中国第一次获得戈登贝尔奖：
–神威·太湖之光超级电脑上的“全球大气非静力云分辨模拟”应用
软件得奖，使用了超过一千万个核来完成一次气候的数值仿真
–从2007年开始相关研究，到2011年冲奖团队正式成立，到2012年
第一次尝试冲奖，再到2016年正式获奖
中国第一次获得戈登贝尔奖
131
http://www.cas.cn/xzfc/201909/t20190925_4716653.shtml

•2020年11月该奖项颁给了一支由中美科学家组成的研究团队
–因“结合分子建模、机器学习和高性能计算相关方法，将具有从头算精度的
分子动力学模拟的极限提升至1亿个原子规模”获奖
–获奖的8人团队中，有7张中国面孔
–该团队将这一工作在美国超算Summit机器上全机运行，模拟分别实现了双精
度91PFlops、混合单精度162PFlops和混合半精度275PFlops的峰值性能
2020年戈登贝尔奖
132

–2021年11月18日下午于美国密苏里州圣路易斯
举行的全球超级计算大会（SC21）上，国际计
算机协会（ACM）将2021年度“戈登贝尔奖”
授予中国超算应用团队
–这是我国联合科研团队基于新一代神威超级计
算机的应用“超大规模量子随机电路实时模拟”
（SWQSIM）而获此殊荣
2021年戈登贝尔奖
133

（三）如何提高高性能

•Using parallelism is one of the most important methods 
for improving performance
•Level of the system:
–multiple processors and multiple storage devices can be used. 
–The workload of handling requests can then be spread among 
the processors and storage devices
•Level of individual processor. 
–pipelining: the best-known example of ILP
•Level of detailed digital design. 
–E.g., set-associative caches use multiple banks of memory that 
are typically searched in parallel to find a desired item
Take Advantage of Parallelism
135

•The principle of locality: 
–programs tend to reuse data and instructions they have 
used recently. 
•An implication of locality 
–we can predict with reasonable accuracy what instructions 
and data a program will use in the near future 
–based on its accesses in the recent past. 
•Two different types of locality have been observed. 
–Temporal locality: recently accessed items are likely to be 
accessed soon. 
–Spatial locality: items whose addresses are near one 
another tend to be referenced close together in time.
Principle of Locality
136

•The most important and pervasive principle of 
computer design is to focus on the common 
case: 
–in making a design trade-off, favor the frequent case 
over the infrequent case.
–This principle applies when determining how to spend 
resources
•The common case is often simpler and can be 
done faster than the infrequent case. 
–E.g., overflow is rare, so first optimize the more 
common case of no overflow
Focus on the Common Case
137

•任务划分（Partitioning）
–将整个计算分解为一些小的任务，其目的是尽量开拓并行执行的机会
•通信（Communication）分析
–分析确定诸任务执行中所需交换的数据和协调诸任务的执行，由此可检测    
上述任务划分的合理性
•任务组合（Agglomeration）
–按性能要求和实现的代价来考察前两阶段的结果，必要时可将一些小的任务
组合成更大的任务以提高性能或减少通信开销
•处理器映射（Mapping）
–将每个任务分配到一个处理器上，其目的是最小化全局执行时间和通信成本
以及最大化处理器的利用率
并行算法（程序）的设计步骤
138

并行算法（程序）的设计步骤
139

•域分解（Domain Decomposition）也叫数据划分
–划分对象是数据
•算法（或程序）的输入数据、计算的输出数据、或者算法所产生的中间结果
–步骤：
•首先，分解与问题相关的数据，使这些小的数据片尽可能大致相等
•其次，将每个计算关联到它所操作的数据上
–由此将产生一系列的任务，每个任务包括一些数据及其上的操作
–当一个操作可能需要别的任务中的数据时，就会产生通信要求
任务划分
140

•功能分解（Functional Decomposition）也称计
算划分
–关注于被执行的计算上，而不是计算所需的数据上
–如果所做的计算划分是成功的，再继续研究计算所需
的数据，如果这些数据基本上不相交，就意味着划分
的成功
–大多数并行算法采用域分解，但功能分解有时能揭示
问题的内在结构
任务划分
141

•一个并行程序通常同时存在数据和功能并行的机会
–功能并行的并行度通常比较有限，并且不会随着问题规模的扩
大而增加；不同的函数所涉及的数据集的大小可能差异很大，
因此也难于实现负载均衡
–数据并行则一般具有较好的可扩展性，也易于实现负载均衡
–现有的绝大多数大规模的并行程序属于数据并行应用，但功能
分解有时能提示问题的内在结构展示出优化的机遇
任务划分
142

•合理性检查
–（1）所划分的任务数是否高于目标机上处理器数目1-2个量级？
•若不是，在后面的设计步骤中将缺少灵活性
–（2）划分是否避免了冗余的计算和存储要求？
•若不是，则产生的算法对大型问题可能不是可扩展的
–（3）各任务的尺寸是否大致相当？
•若不是，则分配处理器时很难做到负载均衡
–（4）划分的任务数是否与问题尺寸成比例？
•理想情况下，问题尺寸的增加应引起任务数的增加而不是任务尺寸的增加
•若不是这样，算法可能不能求解更大的问题，尽管有更多的处理器
–（5）是否采用了几种不同的划分法？
•多考虑几种选择可以提高灵活性。同时既要考虑域分解又要考虑功能分解
任务划分
143

通信分析
144
•局部/全局通信：较少的几个近邻或与很多别的任务通信
–局部通信：每个任务只和较少的几个近邻通信
–全局通信：每个任务与很多其他任务通信
•结构化/非结构化通信：通信图 规整结构（如树、网格等）或任意图
–结构化：规整结构，如树、网格等
–非结构化：通信网络可能是任意图，如稀疏矩阵-向量乘（消息传递编程困难）
•静态/动态通信：不随时间改变或可变的且由运行时所计算的数据决定
–静态：通信伙伴身份不随时间改变，如矩阵相乘
–动态：通信伙伴的身份可能由运行时数据决定且是可变的，如15-
puzzle问题，消息传递编程较困难

通信分析
145
•同步/异步通信：接收方和发送方协同操作或无需协同
•One-way/two-way:
–Two-way: 通信存在生产者-消费者关系，如读写
–One-way: 通信只需要一方发起并完成，如只读

任务组合
146
•粒度控制 
–大量细粒度任务有可能增加通信代价和任务创
建代价
–表-容效应（Surface-Volume Effect）
•一个任务通信需求比例于它所操作的子域的表面积，
而计算需求却比例于子域的容积
–体积不变的前提下，增加计算操作深度（又称
冗余计算），能够减少子域表面积，进而减少
通信量

任务组合
147
•组合判据
–通过组合增加粒度是否减少了通讯成本？
–用重复计算换取通信代价是否已权衡了其得益？
–组合后是否保持了灵活性和可扩放性？
–组合的任务数是否与问题尺寸成比例? 
–组合后的各个任务是否保持了类似的计算和通
讯代价？
–有没有减少并行执行的机会？

处理器映射
148
•处理器映射策略：指定任务到哪个处理器上去执行，其主要目
标是减少算法的总执行时间，策略有二：
–把那些可并发执行的任务放在不同的处理器上以增强并行度
–把那些需频繁通信的任务置于同一个处理器上以提高局部性
•负载均衡：使得所有处理器完成等量的任务 
–减少同步等待的时间，这包括等待其它进程结束运行的时间和串
行执行的代码部分（包括临界区代码和因数据相关造成的串行执
行）
–通常采用的一种策略是在任务分配中，先集中目标使负载尽量均
衡，然后再对任务分配进行调整，使得交互尽量少

任务分配与调度
149
•静态调度：任务到进程的算术映射
–静态地为每个处理器分配连续的循环迭代（要求处理器计算能力同构）
–轮转（将第 i 个循环迭代分配给第 i mod P 个处理器）
•动态调度：动态调度技术可以取得较好的负载均衡效果（但开销较大）
–基本自调度SS（Self Scheduling）：每个处理器空闲时从全局队列取一个任务
–块自调度BSS（Block Self Scheduling）：每次取k个任务（块）
–指导自调度GSS（Guided Self Scheduling） ：每次取剩余任务的 1/P
–因子分解调度FS（Factoring Scheduling）：Ci=Ri/2P，每阶段所有处理器任务大小相等
–梯形自调度TSS（Trapezoid Self Scheduling） ：连续的块之间的差距固定不变
–安全自调度SSS（Safe Self Scheduling）：任务分配使得累计执行时间刚刚超过平均负载
–亲和性调度AS（Affinity Scheduling）：分布式任务队列（本地调度+远程调度）
–自适应耦合调度AAS（Adapt Affinity Scheduling）：初始分配不平衡、计算能力异构
•重载、轻载、常载

•计算机性能
–性能指标
–Amdahl 定律
–Gustafson 定律
–可扩展性评测标准
•基准评测集
•计算机性能提升
–并行算法（程序）的设计步骤
本章小结

•Dongarra J J, Luszczek P, Petitet A. The linpack benchmark: past, present and future. 
Concurrency and Computation: practice and experience, 2003, 15(9):803–820.
•Dongarra J J, Moler C B, Bunch J R, et al. LINPACK users’ guide. SIAM, 1979.
•Dongarra J, Heroux M A, Luszczek P. High-performance conjugate-gradient benchmark: A 
new metric for ranking high-performance computing systems. The International Journal of 
High Performance Computing Applications, 2016, 30(1):3–10.
•Murphy R C, Wheeler K B, Barrett B W, et al. Introducing the graph 500. Cray Users Group 
(CUG), 2010, 19:45–74.
•Kunkel J, Lofstead G F, Bent J. The virtual institute for i/o and the io-500. Technical report, 
Sandia National Lab.(SNL-NM), Albuquerque, NM (United States), 2017.
•Bell G, Bailey D H, Dongarra J, et al. A look back on 30 years of the gordon bell prize. The 
International Journal of High Performance Computing Applications, 2017, 31(6):469–484.
参考文献
