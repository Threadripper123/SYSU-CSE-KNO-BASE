# Chap10 图像分割

> 主题库：数字图像处理　｜　源文件：Chap10 图像分割.pdf　｜　14,810 字符　｜　69 个片段　｜　更新于 2026-11-13


第十章. 图像分割
骆伟祺
中山大学计算机学院
Email：luoweiqi@mail.sysu.edu.cn

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
2

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
3

School of  Computer Science and Engineering
1.基础知识
4
令R表示一张图像占据的整个空间区域，图像分割可以视为把R分为
n个子区域R
1
，R
2
，...，R
n
的过程，满足:
(a)∪
௜ୀଵ
௡
ܴ
௜
=ܴ
(b) R
i
,i=1,2,...,n是一个连通集
(c)对于所有i和j，i≠j，R
i
∩R
j
=∅
(d) Q(R¡)= TRUE,i=1,2,...,n【即要求分割后区域的像素满足某些
性质，如灰度值一致】
(e)对于任何邻接区域R
i
和R
j
，Q(R
i
∪Rj)= FALSE【即要求邻接2分割
后区域的对于谓词逻辑Q的意义是不一样的】

School of  Computer Science and Engineering
1.基础知识
5
图像分割一般是基于灰度值的两种基本特性来分割:
1）不连续性（基于边缘的方法，如点检测、线检测和边缘检测）
2）相似性（基于区域的方法，如阈值处理、区域生长、区域分离和聚合）
基于边缘连续
性进行分割
基于区域性
质进行分割

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
6

School of  Computer Science and Engineering
2. 背景知识
7
一阶导数和二阶导数（用差分定义，详见P504-506）适用于检测灰度的局部突
变，需满足条件：
1）一阶导数
需满足a）在恒定灰度区域为零；b）在灰度台阶和斜坡开始处不为零；c）
在灰度斜坡上不为零
2）二阶导数
需满足a）在恒定灰度区域为零；b）在灰度台阶和斜坡开始处不为零；c）
在灰度斜坡上为零
)()()('xfxfxf
x
f



1
)()()()(
2
2
xfxfxfxf
x
f
211





School of  Computer Science and Engineering
2. 背景知识
8
总结：
（1）一阶导数产生较粗边缘；
（2）二阶导数对孤立点、细线
有较强响应；
（3）二阶导数在灰度斜坡或者
灰度台阶过渡处会产生双边缘
响应；
（4）二阶导数的符号可用于确
定边缘的过渡是从亮到暗还是
从暗到亮。
斜坡
边缘
台阶
边缘
中线
灰度
剖面
线
图像
取样剖
面线

School of  Computer Science and Engineering
2. 孤立点的检测
9
点检测应以二阶导数为基础，可以使用拉普拉斯。
令Z(x,y)表示拉普拉斯滤波结果，采用阈值(T>0)处理检测到的点，输出图像
g(x,y)，这一过程表示为：
1   if  Z(  ,  )
(  ,  )
0   otherwise
x y T
g x y




拉普
拉斯核
原图滤波结果
阈值处理结果

School of  Computer Science and Engineering
2. 线检测
10
原图
拉普拉斯滤波结
果（有负值，出
现双边缘效应：
一黑一白）
取绝对值，消
除双边缘，得
到宽边缘
只取正值，消
除双边缘，得
到细边缘

School of  Computer Science and Engineering
2. 线检测
11
拉普拉斯核是各向同性的、特殊方向线检测通常采用如下4种摸板：
如果上述4种模板产生的响应分别为：R
i
,i=1,2,3,4 ，如果 |Ri(x,y)|>
|Rj(x,y)|,并且i≠j，则认为此点与模板i方向的线有关。这里同样采取阈值决定
哪些为边缘像素，需注意处理“双边缘”效应。

School of  Computer Science and Engineering
2. 线检测
12
+45度左上方放大
右下方放大负值置0
阈值化

School of  Computer Science and Engineering
2. 边缘模型
13
根据灰度剖面，边缘模型可分为：
台阶边缘：1像素距离
上的两个灰度级之间出
现一个过渡
斜坡边缘：边缘接近灰
度斜坡剖面线，斜坡的
斜度与边缘的模糊程度
成反比
屋顶边缘：是通过一个
区域的线的模型，边缘
的基底（宽度）由线的
宽度和尖锐度决定
该图像包含三种边缘模型：斜坡边缘
（左下）、台阶边缘（右上）、屋顶
边缘（右下）

School of  Computer Science and Engineering
2. 边缘模型
14
使用一阶导数和二阶导数检测图像中的边缘
一阶导数的幅度用于检测
图像的某个点处是否存在
一个边缘
二阶导数的符号用于确定
一个边缘像素位于边缘的
暗侧还是亮侧
二阶导数的过0点可以
确定粗边缘的中心位置

School of  Computer Science and Engineering
2. 边缘模型
15
带噪边缘模型一阶导数和二阶导数性质：
斜坡边缘模
型
一阶导数二阶导数
ܩ(0,0.12)
微小的噪声对导数的影响是巨大的，
使用导数前，需要进行平滑预处理。
用于边缘检测的三个步骤：
1）平滑处理（降低噪声）
2）检测边缘点（局部运算，提取可能
是边缘的候选点）
3）边缘定位（从候选点中选出组成边
缘的成员点）
ܩ(0,12)
ܩ(0,102)

School of  Computer Science and Engineering
2. 基本边缘检测
16
图像的梯度及其性质：
梯度用于求图像݂中(x,y)位置的边缘幅度和方向，定义为
梯度幅度:
梯度方向:
(x, y)
(x,
(x, y)
f (x, y)[f(x, y)]
(x, y
)
)
y
x
y
f
g
x
grad
f
g
y

   
   
   

   
   
   

   
   
   

   
221/2
(  ,  )  ||   f(x, y) ||(x, y[])(x, y)
xy
M x yg   g        
(  ,  )   arcta
(x, y)
(x, y
n[]
)
y
x
g
x y
g

边缘方向与梯度
向量方向垂直

School of  Computer Science and Engineering
2. 基本边缘检测
17
梯度算子：
使用偏导（差分定义）：
具有对角性能
的二维核算子
(  ,  )
(  ,  )(    1,  )(  ,  )
x
f x y
g x yf x  y f x y
x

        

(  ,  )
(  ,  )(  ,1)(  ,  )
y
f x y
g x yf x y   f x y
y

        

可得
95
()
x
f
g    z z
x

    

86
()
y
f
gz z
y

    


School of  Computer Science and Engineering
2. 基本边缘检测
18
789123
369147
()   ()
()   ()
x
y
f
g    z z z  z z z
x
f
gz z z  z z z
y

         


         

789123
369147
(2)   (2)
(2)   (2)
x
y
f
g    z  z z  z  z z
x
f
gz  z z  z  z z
y

           


           

梯度算子：
使用偏导（差分定义）：
与Sobel相比，Prewitt更简单，但Sobel能更好抑制（平滑）噪声。

School of  Computer Science and Engineering
2. 基本边缘检测
19
Sobel检测
原图
|gx|
|gy|
|gx|2 + |gy|2
总结：Sobel核主要对垂直边
缘和水平边缘表现较强的响应
梯度方向
α
xy
，
恒定灰度区域
内梯度向量方
向相同
先平滑（均值滤波）后边缘检测
原图
|gx|
|gy|
|gx|2 + |gy|2

School of  Computer Science and Engineering
2. 基本边缘检测
20
使用Kirsch罗
盘核W得到
使用Kirsch罗
盘核SW得到
Kirsch罗盘核：用于检测8个罗盘方向的边缘幅度和方向

School of  Computer Science and Engineering
2. 基本边缘检测
21
梯度与阈值处理相结合：
大于等于梯度图像最大值33%的像素显示
为白色，低于该阈值显示为黑色
平滑图像的梯度图像
经阈值处理结果

School of  Computer Science and Engineering
2. 更先进的边缘检测技术
22
Marr-Hildreth边缘检测子
高斯拉普拉斯算子（LoG）：
拉普拉斯算子ߘ
ଶ
是各向同性的二阶微分算子，定义为：
2
2
2
2
2
y
yxf
x
yxf
yxf






),(),(
),(
各向同性（旋转不变）
当݂为二维高斯函数ܩ，标准差为ߪ
LoG三维图形
图像
剖面线
近似核

School of  Computer Science and Engineering
2. 基本边缘检测
23
Marr-Hildreth边缘检测算法：
2）计算第一步得到的图像的拉普拉斯变换；
3）找到步骤2）所得到图像的过零点（见p.523 确定粗边缘的中心位置）。
1）用对下式取样得到݊×݊的高斯低通滤波器对输入图像滤波；
22
(   ,   )[(   ,   ) ](   ,   )[    (   ,   )(   ,   ) ]g x y    G x y   f x yG x y  f x y  
2
22
2
yx
eyxG


),(
݊选择为大于ߪ6的最小奇数
原图
高斯和拉普拉
斯滤波结果
取过零点
取阈值
先做高斯低通，再求二阶导数

School of  Computer Science and Engineering
2. 更先进的边缘检测技术
24
Canny 坎尼边缘检测子（目前介绍方法中最优秀的）
算法步骤：
1）用一个高斯滤波器平滑输入图像；
2）计算梯度幅值图像和角度图像；
3）对梯度幅值图像应用非极大值抑制进行细化边缘；
4）用双阈值处理和连通性分析来检测与连接边缘。
一阶梯度会产生粗边缘
防止梯度算子会放大噪声
22
1
(  ,  )(  ,  )(  ,  )(  ,  )
(  ,  )
(  ,  )   tan
(  ,  )
[]
ssxy
y
x
M x y  f x y  g x y g x y
g x y
x y
g x y


    

(  ,  )(  ,  )(  ,  )
s
f x y G x y f x y

School of  Computer Science and Engineering
2. 更先进的边缘检测技术
25
步骤3非极大值抑制方案
1）寻找最接近α(x,y)的方向d
k
：
2）若ܯ
௦
(ݔ,ݕ)小于d
k
方向上一个或两个邻点处ܯ
௦
的值，则令g
N
(x,y)=0（抑制）;
否则，令g
N
(x,y)=ܯ
௦
(ݔ,ݕ)：
根据边缘法线确定d
1
 
d
2
d
3
d
4
四个方向分别比较的邻居:
水平、-45、垂直、+45
ܯ
௦
(x,y)小于d
k
方向上的两个邻点值
否则

0
( ,  )
,
N
s
g x y
M x y




非极大值抑制图像

School of  Computer Science and Engineering
2. 更先进的边缘检测技术
26
用双阈值进行处理(高阈值T
H
和低阈值T
L
):
high threshold 
(strong edges) 
lowthreshold 
(weak edges) 
hysteresis 
threshold 
步骤4对݃
݊
作双阈值化处理。单阈值处理存在的问题：
1）阈值过低，产生比较多的伪边缘点；
2）阈值过高，会删除真正的边缘点。
HNNH
Tyxgyxg),(),(
LNNL
Tyxgyxg),(),(
),(),(),(yxgyxgyxg
NHNLNL

强边缘（存在间断）
强边缘+弱边缘
弱边缘

School of  Computer Science and Engineering
2. 更先进的边缘检测技术
27
连通性分析连接边缘（用弱边缘补齐强边缘，获得完整边缘）:
1）在g
NH
(x,y)中定位下一个未被访问的边缘像素p;
2）在g
NL
(x,y)中，用8连通连接到p的所有弱像素标记为有效边缘像素;
3）如果g
NH
(x,y)中所有非零标记已经访问过，则跳到步骤4)，否则返回步骤1）;
4）将g
NL
(x,y)中未被标记为有效边缘的像素的所有像素置零。将g
NL
(x,y)中非零
像素附加到g
NH
(x,y)，形成坎尼算子输出的最终图像。

School of  Computer Science and Engineering
2. Marr-Hildreth v.s. Canny 
28
原图
梯度图
坎尼算
法得到
Marr-
Hildreth
算法得
到
原图
梯度图
坎尼算
法得到
Marr-
Hildreth
算法得到

School of  Computer Science and Engineering
2. 连接边缘点
29
边缘连接:两个端点在边缘强度和走向相近的情况下连接，组合为有意义的边缘和
/或区域边界。这里讨论两种连接边缘的基本方法：局部区域处理和全局处理。
局部处理：把邻域内具有相似性质的点连接.如像素(s,t)在(x,y)的邻域内满足：
,),(),(EyxMtsM
Ayxts),(),(
其中,E是幅度阈值,A是角度阈值，满足则可以将(s,t)与(x,y)连接起来。
原图
梯度幅值
水平连接边缘像素
垂直连接
边缘像素
水平U 垂直形态学细化

School of  Computer Science and Engineering
2. 连接边缘点
30
全局处理：可使用霍夫变换
O
b
a
(ܽ
1
,ܾ
1
)
x-y空间的一条直线
对应a-b空间上一点
ݕ
݅
=݅ݔܽ+ܾ
O
(݅ݔ,݅ݕ)
y
x
过x-y空间的一点做
无数条直线
在a-b空间是一条直线
O
y
x
O
b
a
过（ݔ
݅
,݅ݕ）的
所有直线
ܾ=ݕ
݅
−݅ݔܽ

School of  Computer Science and Engineering
2. 连接边缘点
31
O
(݅ݔ,݅ݕ)
y
x
(݆ݔ,݆ݕ)
O
b
a
b’
a’
ܾ=−݅ݔܽ+ݕ
݅
ܾ=−݆ݔܽ+ݕ
݆
统计a-b 参数空间
交点的数目，用阈值
确定图像中点过的直线
一条直线
同时过2点
交点参数
确定直线

School of  Computer Science and Engineering
2. 连接边缘点
32
使用(a,b)参数空间（用于表示直线的斜率和截距）存在的的问题:
1）无界的参数域；
2）垂直线需要无穷大的a。
因而使用直线的法线表示：
正弦曲线
离散化参数空间，
投票选取直线参数
)sin()cos(yx
注：Hough变换检测直线的思想也可以用于检测其他的能用解析式
表示的参数较少的曲线，如圆、椭圆、抛物线等。

School of  Computer Science and Engineering
2. 连接边缘点
33
基于Hough变换的边缘连接方法：
1）根据原图得到二值边缘图像；
2）规定ߠߩ平面中的细分；
3）检查像素高度集中的累加器单元数量；
4）检查选中单元中像素间的关系（针对连续性）。
原图
霍夫参数空间（两处
方框强调了与长垂直
线相关联的点）
对应的
方框所
强调的
点的线
叠加至原图
坎尼边缘图

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
34

School of  Computer Science and Engineering
3. 基础知识
35
设阈值处理模型为
)],(),,(,,[yxfyxpyxTT
其中，݂(ݔ,ݕ)是点(x,y)的灰度级，݌(ݔ,ݕ)是该点的局部性质。
当T取决于݂(ݔ,ݕ)时，阈值是全局的；
当T取决于݂(ݔ,ݕ)和݌(ݔ,ݕ)时，阈值是局部的；
当T取决于位置坐标ݔ和ݕ时，阈值是自适应或动态的。






Tyxf
Tyxf
yxg
),(
),(
),(
0
1












1
21
2
Tyxfc
TyxfTb
Tyxfa
yxg
,,
,,
,,
),(
根据波谷、波峰划分
单阈值
双阈值

School of  Computer Science and Engineering
3. 基础知识
36
图像阈值处理中噪声、光照的影响
噪声污染严重，无法找到
合适的阈值进行分割
被灰度斜坡污染，波峰之
间的深谷破坏到无法分隔

School of  Computer Science and Engineering
3. 基本的全局阈值处理
37
算法步骤：
1）为全局阈值选择一个初始值T(通常选择图像的均值作为初始值)；
2）使用T分割图像，产生两组像素:所有的灰度值均大于T的像素组G
1
以及所有的
灰度值小于等于T的像素组G
2
；
3）分别计算G
1
和G
2
中像素的平均灰度值m
1
,m
2
；
4）令T=(m
1
+m
2
)/2；
5）重复步骤2）、3）、4）直到得到的相邻的T的差值小于预定义的∆T值为止。
初始值T的选择影响迭代速度

School of  Computer Science and Engineering
3. 使用Otsu方法的最优全局阈值处理
38
通过最大化类间方差来优化全局阈值——仅使用图像的直方图
1）让{0、1、2...、ܮ−1}表示M×N图像的强度，n
i
表示具有强度݅的像素数。
归一化后的直方图:
假设我们选择一个阈值݇，将图像分割成两类:强度在[0，݇]范围内的ܥ
1
，强度
在[݇+1，ܮ−1]范围内的ܥ
2
。
2）计算类别ܥ
1
和ܥ
2
的累积和：
3）计算累积均值m(k), ݇=0,1,2,...,ܮ−1：
1
0
,1,0
L
i
iii
i
n
pp   p
MN


   

1
121
01
(  ),(  )1(  )
kL
ii
ii k
P k   p P k   p  P k

 
    
    
0
(  )
k
i
i
m k   ip




School of  Computer Science and Engineering
3. 使用Otsu方法的最优全局阈值处理
39
4）计算全局灰度均值m
G
：
5）计算类间方差ߪ
஻
ଶ
(݇),݇=0,1,2,...,ܮ−1：
6）得到Otsu阈值݇
∗
，即ߪ
஻
ଶ
(݇)最大时的݇值。如果极大值不唯一，那么取对应
各个极大值的各个݇值的平均值来得到݇
∗
：
1
0
L
Gi
i
m   ip




2
2
1
11
[(  )(  )]
(  )
(  )[1(  )]
G
B
m P k m k
k
P k  P k




*2
01
max{   (  )}
k L   B
kk
  

全局阈值
Otsu
原图
直方图

School of  Computer Science and Engineering
3. 使用图像平滑改进全局阈值处理
40
平滑处理显著改
进了阈值分割表
现，但也会引起
边界的轻微失真

School of  Computer Science and Engineering
3. 使用图像平滑改进全局阈值处理
41
待分割区域小到无法
与噪声引起的灰度扩
展相比，平滑处理无
法改进阈值分割表现

School of  Computer Science and Engineering
3. 使用边缘改进全局阈值处理
42
待分割图像݂(ݔ,ݕ)使用满足某些基于梯度和拉普拉斯的简单测度的像素，有助
于加深直方图波峰间的波谷：
1）计算边缘图像，如利用梯度幅度或拉普拉斯绝对值
2）规定一个阈值T；
3）用步骤2）阈值T对边缘图像进行阈值处理，得到一幅二值图像g
T
(x,y)，并用
做一幅模板图像；
4）只用݂(ݔ,ݕ)中对应g
T
(x,y)中1值像素位置的那些像素计算直方图；
5）用步骤4）中的直方图全局地分割݂(ݔ,ݕ)，例如使用Otsu方法。
小区域待分割
的噪声图像
直方图
直方图
采用99.7%阈
值化的梯度幅
度掩码图
原图X 掩码图Otsu

School of  Computer Science and Engineering
3. 使用边缘改进全局阈值处理
43
使用基于拉普拉斯的边缘信息改进全局阈值处理
原图直方图
直方图
Otsu
Otsu阈值
化的
拉普
拉斯
绝对
值掩
码图

School of  Computer Science and Engineering
3. 多阈值处理
44
Otsu方法可以扩展到多阈值
对于K个类ܥ
1
,ܥ
2
,...,ܭܥ,类间方差可归纳为：
其中，，݉
ܩ
是全局均值
阈值的确定可通过使其极大化：

2
2
1
K
Bk  k   G
k
P m m



k
ki
i c
P   p




12
2***2
1210...1121
,   ,...,max,   ,...
()
K
BKk k  k L   BK
k k  kk k k
     

使用两个阈值分
割为三个区域

School of  Computer Science and Engineering
3. 可变阈值处理
45
基于局部图像性质的可变阈值处理：
根据(ݔ,ݕ)的一个邻域的一条或多条规定性质计算阈值。令m
xy
和ߪ
௫௬
是图像݂中以
坐标(x,y)为中心的邻域S
xy
所包含的像素集的标准差和均值，关于局部图像性质
的可变阈值的通用公式可表达为：
ܶ
௫௬
=ܽߪ
௫௬
+ܾ݉
௫௬
式中，a和ܾ是非负常数。分割后的图像g计算为
根据每个像素(x,y)的邻域算出的谓词逻辑，可给可变阈值处理分配更大的权重：
原图
使用双阈
值分割
3x3邻域
的局部标
准差图像
用局部阈
值处理得
到的结果
1   (  ,  )
(  ,  )
0   (  ,  )
xy
xy
f x y T
g x y
f x y T







School of  Computer Science and Engineering
3. 可变阈值处理
46
基于移动平均的可变阈值处理：
在文件图像处理中很有用，一般沿着行、列或Z字形使用。利用移动平均值(平
均强度)，进行阈值化处理
光照污染图像Otsu方法全局分割移动平均局部阈值分割

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
47

School of  Computer Science and Engineering
4. 区域生长
48
区域生长法是一种根据事前定义的准则将像素或子区域聚合成更大区域的过程。
令݂(ݔ,ݕ)是一幅输入图像；令ܵ(ݔ,ݕ)是一个种子阵列，阵列中种子点所在位置的值
是1，其他位置的值是0；令Q是应用到每个位置(x,y)的一个谓词逻辑。基于8连接的
一个基本区域生长算法说明如下：
1）在ܵ(ݔ,ݕ)中找到所有连通分量，把所有这种像素标记为1，其他像素标记为0;
2）如果݂(ݔ,ݕ)满足谓词Q，则形成图像݂
ܳ
(ݔ,ݕ)=1，否则݂
ܳ
(ݔ,ݕ)=0;
3）对于在݂
ܳ
中8连通到种子点的所有1值点，添加到S中的每个种子点，形成图像݃;
4）用不同的标签标记݃的每个连通分量，这就是区域生长得到的分割结果。

School of  Computer Science and Engineering
4. 区域生长
49
原图
直方图初始种子
最终种子(腐蚀)
种子点处值为255
逻辑谓词
255-原图
直方图
双阈值
68 126
单阈值
68
与种子连通区域

School of  Computer Science and Engineering
4. 区域分离与聚合
令R表示整个图像区域，Q是针对区域的一个逻辑谓词。
50
算法步骤：
1）把满足Q(R
i
)=FALSE的任何R
i
区域分离为四个不相交的子象限区域；
2）无法进一步分离时，聚合满足谓词逻辑Q(R
j
∪R
k
)=TRUE的任意两个邻接区域R
j
和R
k
;
3）在无法进一步聚合时停止。

School of  Computer Science and Engineering
4. 区域分离与聚合
谓词逻辑
51
32×32
8×816×16
原图

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
52

School of  Computer Science and Engineering
分水线
汇水盆地
极小值点
5. 背景知识
分水岭以三维(2位置坐标和1灰度（高度）坐标)显示一幅图像（P36，图2.18）
在这种“地形学”的解释中，考虑三种类型的点：
1）一个区域极小值点（可能存在多个，同个区域看成一个）；
2）水滴在该位置，会流向某个极值点。这些点形成对应极小值点的汇水盆地。
3）水滴在该位置，水会等概率地流向不止一个极小值的点。这些点形成分界线/分水线。
53

School of  Computer Science and Engineering
5. 背景知识
洪水的第一阶段。
水覆盖区域与深色背景相对应
洪水的下一个阶段
水已经涨进了左边集水盆地
左边盆里的水溢到了右边盆里
修建了一座短坝以防止水合流
水已经上升到右边汇水盆地。
假设所有汇水盆地极小值点都是与地下水相连，地下水水位不断上涨（像素从min max+1）
54

School of  Computer Science and Engineering
5. 背景知识
水位逐渐上升水坝更长了
建造了新的水坝。
这一过程一直持续到洪水达到最大程度。
最终的水坝与流域线相对应，这是分割的结果。
55

School of  Computer Science and Engineering
5. 构建水坝
洪水泛滥的第n-1步，有两个连通分量
水位上升到第n步，此时形成
一个连通分量，表示为q
用结构元对n-1步的连通单元
执行膨胀，使其满足：
1）能够约束到q中；
2）不能对使得正被膨胀集合
聚合（形成单个连通分量）
的那些点执行膨胀
q中满足上述两条件的点为分隔水坝
C
n-1
(M
i
) 水位为n-1时，
汇水盆地坐标点集与对
应极小值坐标点的集合
56

School of  Computer Science and Engineering
5. 分水岭分割算法思想
算法思想:
1）采用低阈值二值化（找盆底）;
2）逐渐增加阈值（模拟水涨）;
3）初次接触的点为边界（找到分水岭）;
4）“修坝”以继续寻找高位分水岭。
57

School of  Computer Science and Engineering
5. 分水岭分割算法
设M
1
、M
2
...MR为表示图像݃(ݔ,ݕ)的局部最小值点的坐标集合（预先给定），min
和max代表g(x,y)的最小值、最大值。
C(Mi)为M
i
相对应的汇水盆地内的点的坐标的集合。
T[n]表示坐标(s,t)的集合
T[n]={(s,t)|g(s,t)< n}
几何上看，T[n]是g(x,y)中点的坐标集合，集合中的点均位于 g(x,y)=n平面之下
的位置【水涨到n高度后被淹没的位置】。假设T[n]中坐标在g(x,y)=n之下，并被
标记为黑色，其他坐标标记为白色，形成一幅二值图像。
模拟水位增长，n 从整数min+1到max+1不断增加，图像所有地形均会被水漫过。
58

School of  Computer Science and Engineering
5. 分水岭分割算法
令ܥ
݊
(ܯ
݅
)表示汇水盆中与淹没阶段n的极小值M
i
相关联的点的坐标集合：
C
n
(ܯ
݅
)=C(ܯ
݅
)∩T[n]
即(x,y)∈C(ܯ
݅
)且(x,y)∈T[n]时，位置(x,y)有C
n
(ܯ
݅
)=1，否则C
n
(ܯ
݅
)=0
令B表示第n阶段被洪水淹没的汇水盆地的数量，令C[n]表示第n阶段汇水盆地
被水淹的部分的合集:
C[n]=∪
௜ୀଵ
஻
 Cn(ܯ
݅
)
令C[max+1]为所有汇水盆地合集:
C[max+1]=∪
௜ୀଵ
஻`
 C(ܯ
݅
)
可证明：C[n−1]中每个连通分量都恰好包含在T[n]的一个连通分量中。
59

School of  Computer Science and Engineering
5. 分水岭分割算法
算法开始时，令C[min+1]=T[min+1]。通过递归调用，利用C[n−1]计算C[n]
过程如下:
设Q代表T[n]中连通分量的集合，对每个连通分量q∈Q[n]，有三种可能:
1.q ⋂ C[n−1]为空
当遇到一个新的最小值时，符合条件(1)则将q并入C[n−1]→C[n]。
1.q∩C[n−1]包含C[n−1]中的一个连通分量
当q位于某些局部最小值构成的汇水盆地时符合条件(2)，将q并入C[n−1]→C[n]。
1.q∩C[n−1]包含C[n−1]多于一个的连通分量
当遇到全部或部分分割两个或多个汇水盆的山脊时，符合条件(3)，再注水就导
致合并，此时用3×3结构元素膨胀q ⋂ C[n−1]并限制在q内，建筑水坝。
60

School of  Computer Science and Engineering
5. 标记的使用
噪声和梯度的其他局部不规则性通常会导致过度分割。
解决方案:通过引入预处理阶段来限制允许区域的数量，为分割过程提供额外知识。
用于控制过度分割的一种方法依据的是标记这一概念（标记是属于一幅图像的连
通分量）：
1）与对象相关联的为内部标记；
2）与背景相关联的为外部标记。
61

School of  Computer Science and Engineering
5. 标记的使用
平滑后，内部标记显示为浅灰色。
分水岭算法被应用，内部标记是唯一允许的区域最小值。
由此形成的分水岭是外部标志(以白色显示)。
由外部标记定义的每个区域都有一个内部标记和一部分背景。
问题简化为将这些区域中的每一个分割成两个部分:一个对象和一个背景。
选择标记的过程：
1）预处理（如平滑）；
2）定义标记必须满足的一个准则集合（如内部标记：被更高的“海拔”点包围
的区域；区域中形成一个连通分量的点；连通分量中具有相同灰度值的点）。
62

School of  Computer Science and Engineering
1.基础知识
2.点、线和边缘检测
3.阈值处理
4.使用区域生长、区域分离与聚合
5.使用形态学分水岭分割图像
6.分割中运动的使用
第十章:图像分割
63

School of  Computer Science and Engineering
6. 空间域技术
累积差值：
将参考图像R(x,y)与序列中的每个后续图像进行比较。当当前图像中的像素与
参考图像不同时，累积差分图像(ADI)中每个像素的计数器就会增加。当检查
t
k
帧时，累积差分图像给出该像素与参考图像中对应像素的差异次数。三种不
同类型的ADI定义如下：
绝对ADI
正ADI
负ADI
64





其他0
),,(),,(1
),(
Ttyxftyxf
yxd
ji
ij
基本方法：
逐像素地比较ݐ
݅
和ݐ
݆
两帧图像݂(ݔ,ݕ)可以获得相应的差值图像：
其中ܶ是一个非负阈值。

School of  Computer Science and Engineering
6. 空间域技术
ADls表示向东南移动的矩形物体。
绝对ADI同时包含正负ADIs。
正ADI的非零面积表示物体的大小。
正ADI的位置表示物体在参考帧中的位置。
方向和速度可从绝对和负ADIs得到。
65

School of  Computer Science and Engineering
6. 空间域技术
建立参考图像：
根据一组包含一个或多个运动目标的图像来构建参考图像，以描述频繁变化或
者频繁更新的场景：
1）平均序列帧；
2）高斯混淆建模。
将第一张图像视为参考图像。
当一个移动对象在参考帧中移出其位置时，当前帧中的相应背景可以在参考帧中复制。这
由正ADI确定:当移动对象相对于参考帧完全偏移时，正ADI停止增加。
66

School of  Computer Science and Engineering
67
作业：
10.6、10.11、10.25、10.34，10.42，10.47，10.51 

School of  Computer Science and Engineering
68
编程：
1.编写一个全局阈值化程序，该程序使用Otsu方法自动估计阈值，程序的
输出应该是一个二值分割图像。
2.下载下图并使用阈值程序对对象进行分割。
