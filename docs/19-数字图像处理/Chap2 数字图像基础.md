# Chap2 数字图像基础

> 主题库：数字图像处理　｜　源文件：Chap2 数字图像基础.pdf　｜　9,453 字符　｜　57 个片段　｜　更新于 2026-11-13


第二章. 数字图像基础
骆伟祺
中山大学计算机学院
Email：luoweiqi@mail.sysu.edu.cn
1

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
2

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
3

School of  Computer Science and Engineering
1.人眼结构
4
物体
光线
角膜
瞳孔
（控制进光量）
虹膜
（控制瞳孔）
晶状体
（聚焦与投影）
神经元
（传递信号到大脑）
视网膜
（光信号转脑电信号）
影像

School of  Computer Science and Engineering
1.人眼结构
5
视网膜中的光感受器：锥状体(Cone) 和杆状体(Rod)
锥状体：每只眼约600~700万个，主要分布于视网膜中央凹部分，有自身的神
经末销，对色彩高度敏感，称为明视觉。
杆状体：每只眼有7500~15000万个，分布面积大且几个杆状体连接到同一个
神经末销，用于捕获图像轮廓，没色觉，对低光照度敏感，称为暗视觉。

School of  Computer Science and Engineering
1. 人眼结构
6
锥状体和杆状体在视网膜上的分布
其中，中央凹直径约1.5mm圆形凹陷，其面积为1.77 mm
2 
。锥状体密度为
150,000个/mm
2
，约有26500个锥状体

School of  Computer Science and Engineering
1.人眼成像
7
晶状体的自适应调节能力
看远处物体，肌肉会迫使晶状体变得扁平，晶状体的聚焦中心向前移动；
看近处物体，则反之。
焦距越小，晶状体的折射能力越强，故近处物体的细节更加容易分辨。

School of  Computer Science and Engineering
1. 亮度适应
8
人类视觉系统能够适应光强等级很宽：从暗阈值到强闪光约有10
10
个量级。
主观亮度(人视觉系统感知的亮度)是进入人眼光强的对数函数

School of  Computer Science and Engineering
1. 马赫带效应图解
9
视觉系统在不同强度区域边界存在
“上冲”、“下冲”效应
感知亮度并非实际灰度的简单函数
实际亮度
感知亮度

School of  Computer Science and Engineering
1. 同时对比
10
所有中心分块的灰度都相同，但随着背景变亮，它们感觉逐渐“变暗”

School of  Computer Science and Engineering
1. 光学错觉
11

School of  Computer Science and Engineering
1. 光学错觉
12
白点v.s.黑点
凹v.s.凸
动v.s. 静人脸v.s.花瓶

School of  Computer Science and Engineering
1.人眼成像v.s. 相机成像
13

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
14

School of  Computer Science and Engineering
2. “七彩”阳光
15
1666年，牛顿发现：当一束白光通过一个玻璃棱镜时，出来的光就不
再是白光，而是一端是紫色另一端是红色的连续色谱。

School of  Computer Science and Engineering
2. 光和电磁波
16
光是电磁波，而电磁波可用波长λ、频率ν、或能量E表示。
λ = c / ν
其中c是光速（2.998x10
8
m/s）
电磁波谱的能量由下式给出
E=hν
h是普克朗常数。能量与频率成正比。
电磁波可以看成是以波长λ传播的正弦波。

School of  Computer Science and Engineering
2. 电磁波谱
17
可见光的波长范围：约400~700nm（纳米）（1nm=10
-9
m）
波谱一端是伽马射线，波长比可见光短几百万倍
波普另一端是无线电波，波长比可见光长几十亿倍
能量
频率
波长

School of  Computer Science and Engineering
2. 单色光
18
单色光：即无颜色的光，其唯一的属性为亮度
0     1     2     3      4     5     6     7     8     9
无颜色图像称为灰度图像（单通道），除第6章彩色图像处理外，灰度图像
处理是本书研究的重点！
彩色图像可用分别用多个单通道的灰度值表示各颜色分量，如RGB通道等。

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
19

School of  Computer Science and Engineering
3. 图像传感器
20
单个数字成像传感器
条状传感器
阵列传感器

School of  Computer Science and Engineering
3. 使用单个、条状传感器获取图像
21

School of  Computer Science and Engineering
3. 使用阵列传感器获取图像
22
数码相机的传感器类型主要决定了相机的成像质量、噪声水平和整体性能。常见的传感器
包括：CCD （Charge-Coupled Device，电荷耦合器件）和CMOS（Complementary 
Metal-Oxide-Semiconductor，互补金属氧化物半导体）。

School of  Computer Science and Engineering
3. 简单的成像模型
23
灰度图像可以用一个二维函数푓(푥,푦)表示，其中푓(푥,푦)正比于物理源的辐射
能量（如电磁波），是一个非负且有限的标量：
0≤ƒ푥,푦<+∞
函数ƒ(x,y)有两个分量来表征：
（1）入射分量푖(푥,푦)：入射到观察场景的光源总量和
（2）反射分量푟(푥,푦)：场景中物体反射光的总量。
ƒ(푥,푦)=푖(푥,푦)푟(푥,푦)
其中0≤푖푥,푦<+∞，0≤푟(푥,푦)≤1
单色图像上任一点的强度就是图像在那一点的灰度级。
反射分量限制在0（全吸收）和1（全反射）之间。

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
24

School of  Computer Science and Engineering
4.图像采样与量化
25
图像可用一个二维函数푓(푥,푦)进行表示
注：1. 该函数的푥坐标（∈[0, a]）、y坐标(∈[0, b])，
2. 函数值0≤ƒ푥,푦<+∞
计算机无法存储与处理连续的数值，需对图像函数的坐标及其函数值进行数
字化（离散且有限化）处理。
✓图像采样：对坐标值进行数字化
✓图像量化：对函数值进行数字化
풇(풙,풚)

School of  Computer Science and Engineering
4.连续图像离散化
26
连续图像
A→B的扫描线
量化
数字化扫描线
采样

School of  Computer Science and Engineering
4. 空间采样
27
二维图像采样：
把푥푦平面分为网格，每一网格的中心坐标用笛卡儿坐标(푥,푦)表示，(x,y) 
属于Z
2 
。最后仅记录这些网格中心点的强度。
连续物理图像
采样结果

School of  Computer Science and Engineering
4. 数字图像表示
28
一幅图像可以定义为一个二维函数푓(푥,푦)，其中(x,y)是平面坐标，在任何
坐标(x,y)处的幅度푓被定义为图像在这一位置的亮度。
矩阵表示
坐标约定

School of  Computer Science and Engineering
4. 坐标索引和线性索引
29
坐标索引：像素的位置由其二维坐标(푥,푦)给出的索引。
线性索引：由一个一维的非负整数串组成，这个非负整数串是通过计算到坐标
(0, 0)的偏移量得到的。扫描方式包括：行扫描、列扫描。
生成列扫描线性索引

School of  Computer Science and Engineering
4. 空间分辨率
30
空间分辨率：图像中可辨别的最小细节。
打印机：单位距离可以分辨的最小线对数目(Dots Per Inch ：DPI)
数字图像：图像大小，即行数╳ 列数(Pixels Per Inch：PPI)
采样间隔越大，像素越少，分辨率越低，
细节越少

School of  Computer Science and Engineering
4. 灰度分辨率
31
灰度分辨率：灰度级中可分辨的最小变化。
对采样点强度值的离散化过程（连续变化区间转换为单个特定编码），
确定图像的灰度等级
物理世界连续强度范围
曝光范围
成像系统动态范围
强度量化
0123...244255
数字图像灰度级别（灰度分辨率）

School of  Computer Science and Engineering
4. 灰度分辨率
32
•灰度分辨率（灰度级数）通常为2的整数次幂：L=2
k
•一般用8bit存一个像素点的灰度值(k=8，2
8
=256级)
•灰度存储一般选择[0,L-1](或者[0,1]）表示
灰度等级越少，图像细节损失越多，伪轮廓信息增加，图像的视觉效果越差，但图像数据量变小
对比：空间分辨率变小导致的图像视觉效果变化

School of  Computer Science and Engineering
4. 图像内插
33
图像内插：用已知数据去估计未知位置的值的过程，主要用于图像放缩、旋转、几
何校正等任务。常见的内插方法包括：
最近邻内插：把原图像中最近邻的灰度赋给了每个新位置，最简单但会造成严重的直边失真
双线性内插：用4个最近邻点去估计给定位置的灰度，可给出比最近邻内插好得多的结果，但
随之而来的是计算量的增加。注意：双线性内插不是一种线性内插方法。
双三次内插：用16个最近邻点，复杂度较高，在保持细节方面比双线性内插相对要好。
最近邻方法
双线性
双三次内插

School of  Computer Science and Engineering
4. 超分辨率研究
34
双线性内插
SRResNet
SRGAN
原始图像
以数据驱动的方式（如GAN，Diffusion）进行插值

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
35

School of  Computer Science and Engineering
5. 像素的相邻像素
36
位于坐标(x, y)的像素p有四个水平和垂直的相邻像素（黑点）。坐标分别为：
(x-1, y), (x+1, y), (x, y-1), (x, y+1)。定义为像素p的4邻域，用N
4
(p)表示。

•

•
•

•

)
,
(
y
x
此外，p有4个对角相邻像素（星号）。坐标为：(x-1, y-1), (x+1, y-1), (x-
1, y+1), (x+1, y+1)。用N
D
(p)表示。
如何定义16邻域？
N
D
(p)和N
4
(p)的并集称为p的8邻域，用N
8
(p)表示

School of  Computer Science and Engineering
5. 邻接
37
设V是邻接的灰度值集合，如二值图像考察值为1的邻接情况时V={1}，对于
灰度图像V可以为{0,1,2,...,255} 的非空子集。
常见的三种邻接性：
✓4邻接：如果点q在N
4
(p)中，并q和p具有V中的数值，则q和p是4邻接的；
✓8邻接：如果点q在N
8
(p)中，并q和p具有V中的数值，则q和p是8邻接的；
✓m邻接（混合邻接）：满足下列条件的任一个，则具有V中数值的p和q是
m连接的。
--q在中N
4
(p)
--q在N
D
(p)中，且集合푁
4
(푝)∩푁
4
(푞)中没有V值的像素。

School of  Computer Science and Engineering
5. 邻接、数字通路
38
混合邻接是可消除8邻接的歧义性，如对二值图像，V={1}。
8邻接
m邻接
从坐标为(x
0
,y
0
)的像素p到坐标为（x
n
,y
n
）的像素q的数字通路（或曲线）定义为
如下的像素序列：(x
0
,y
0
)，(x
1
,y
1
), ..., (x
n
,y
n
)
其中，点(x
i
,y
i
), (x
i-1
,y
i-1
) 在1≤i≤n 均是邻接的（可为4、8、m邻接）。此时，
该通路的长度为n，当(x
0
,y
0
) =(x
n
,y
n
) 时，为闭合通路。
该节中其它的概念（如区域、边界、距离等）请自行学习！

School of  Computer Science and Engineering
1.视觉感知要素
2.光和电磁波谱
3.图像感知与获取
4.图像取样和量化
5.像素间的一些基本关系
6.数字图像处理所用的基本数学工具介绍
第二章:数字图像基础
39

School of  Computer Science and Engineering
6. 对应元素运算和矩阵运算
40
图像=矩阵。两幅图像的运算可以定义为矩阵之间的运算，如
考虑两2 ×2图像
11
12
11
12
21
22
21
22
aabb
and
aabb












定义矩阵乘法
11
12
11
12
11  11
12   21
11  12
12   22
21
22
21
22
21  11
22   21
21  12
22   22
aabbabababab
aabbabababab
++

 
    

=

 
    

++

 
    

定义对应元素的积
11
12
11
12
11  11
12   12
21
22
21
22
21   21
22   22
aabbabab
aabbabab



    

=



    




    

符号：⊗或者⊙

School of  Computer Science and Engineering
6. 线性运算和非线性运算
41
给定两个任意常数a和b,以及两幅任意图像푓
1
(푥,푦)和푓
2
(푥,푦)，若满足
12
12
12
[
(  ,   )
(  ,   )]
[   (  ,   )]
[    (  ,   )]
(  ,   )
(  ,   )
Hafxybfxy
aHfxybHfxy
agxybgxy
+
=+
=+
考虑算子H，该算子对一幅输入图像푓(푥,푦)产生一幅输出图像g푥,푦
[   (  ,   )]
(  ,   )
Hfxygxy
=
则H是线性算子，若不满足则为非线性算子。如求矩阵最大值算子max等。
注：在大部分任务中，通常非线性算子往往优于线性算子。后续章节介绍。

School of  Computer Science and Engineering
6. 算术运算
42
两幅图像푓(푥,푦)和g(푥,푦)之间的算数运算表示为
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
)
,
(
y
x
g
y
x
f
y
x
v
y
x
g
y
x
f
y
x
p
y
x
g
y
x
f
y
x
d
y
x
g
y
x
f
y
x
s

=

=
−
=
+
=
以上算数均为对应像素运算，在푓和g中的对应像素对之间执行。
其中푥=0,1,2,...,푀−1；푦=0,1,2,...,푁−1，M和N对应图像的行数和列数

School of  Computer Science and Engineering
6. 图像加法
43
去噪
注：需满足假设条件p. 50  + 进行图像配准

School of  Computer Science and Engineering
6. 图像相减
44
异常检测
模板图像（正常）活体图像
相减结果增强结果

School of  Computer Science and Engineering
6. 图像相乘
45
图像相乘用
于校正阴影
图像相乘用
于校正模板

School of  Computer Science and Engineering
6. 集合运算
46
集合是不同对象的汇总. 任一个元素a与一集合A，只有以下两种关系
a∈퐴
1）a 属于A 表示为
a∉퐴
2）a 不属于A 表示为
若集合A中的所有元素也是集合B中的元素，则称集合A是集合B的子集，表示为
퐴⊑B
注：集合与集合之间的基本运算（并交补）及其运算律（交换律、结合律、分配率、
德摩根律等）与《概率论》、《离散数学》等相关课程介绍一致

School of  Computer Science and Engineering
6. 集合基本运算-维恩图
47
集合的并
}|{BxAxxBA=或
集合的补
}
|
{
B
x
x
B
c

=
集合的交퐴∩퐵={푥|푥∈퐴且푥∈퐵}
集合的差
}
|
{
A
B
x
A
x
x
B


=
−
且
A
B
A
B
注：自学并了解该节相关部分

School of  Computer Science and Engineering
6. 逻辑运算
48
逻辑运算处理TRUE(通常用1表示)和FALSE(通常用0表示)变量与表达式。考察由
前景(1值)像素和(0值)背景像素组成的二值图像。逻辑“与”、“或”、“非”
运算是功能完备的，它们是构建任何其他逻辑运算符的基础：
逻辑“非”
逻辑“与”
逻辑“或”
注：类似集合的维恩图

School of  Computer Science and Engineering
6. 单像素及邻域变换
49
邻域运算——局部平均单像素运算——灰度反转

School of  Computer Science and Engineering
6. 几何变换
50
几何运算改变图像中像素的空间排列，通过预定义的一组规则来拉伸和收
缩图像。由坐标空间变换和灰度插值两种基本运算组成。
坐标空间变换：
11
12
21
22
'
'
tt
xxx
T
tt
yyy

      
 
==

      
 
      
 

11
12
13
21
22
23
 
 
 
'
'A
1
1
1
00
 
 
 
 
 
1
aaa
xxx
yyaaay

       
 

       
 
==

       
 

       
 
       
 

a
13
=a
23
=0
仿射变换包括：放缩、平移、旋转和剪切，可用齐次坐标表示。
灰度插值：即为空间变换后的像素赋予灰度值，常见ppt 33-34. 

School of  Computer Science and Engineering
6. 仿射变换
51

School of  Computer Science and Engineering
6. 空间运算
52
图像配准用于对齐同一场景的两幅或多幅图像。通过对图像几何变换，使
输出图像与参考图像对齐。
校正几何
形变图像
参考图像几何形变图像
配准后图像
差异图像

School of  Computer Science and Engineering
6. 向量表达与矩阵运算
53
RGB图像中每个像素可组成一个列向量
푧
1
z
2
和z
3
分别是红色、绿色和蓝色
通道中像素的灰度
大小为M×N的RGB图像可以用
MN个3×1的向量表示
所有图像都可以当矩阵或者向量来处理，例如
gHfn
=+
其中，푓是输入图像的푀푁×1向量，푛是表示푀×푁噪声模式的푀푁×1向量，g是处
理后图像的푀푁×1向量，퐻是对输入图像进行线性处理的푀푁×푀푁矩阵

School of  Computer Science and Engineering
6. 图像变换
54
图像变换—不直接在图像空间域对像素进行操作
包括以下的步骤：变换图像，在变换域执行任务，执行反变换，返回空间域


−
=
−
=
=
1
0
1
0
)
,
,
,
(
)
,
(
)
,
(
M
x
N
y
v
u
y
x
r
y
x
f
v
u
T


−
=
−
=
=
1
0
1
0
)
,
,
,
(
))
,
(
(
)
,
(
M
u
N
v
v
u
y
x
s
v
u
T
R
y
x
g
将在第四章详细介绍

School of  Computer Science and Engineering
作业
55
Ex. 2.8、2.14、2.18、2.28
自学
1. Matlab图像处理的一些基本操作
2. Photoshop 图像处理的基本功能
