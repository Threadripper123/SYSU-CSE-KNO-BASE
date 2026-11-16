# Chap6 彩色图像处理

> 主题库：数字图像处理　｜　源文件：Chap6 彩色图像处理.pdf　｜　9,772 字符　｜　50 个片段　｜　更新于 2026-11-13


第六章. 彩色图像处理
骆伟祺
中山大学计算机学院
Email：luoweiqi@mail.sysu.edu.cn
1

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
2

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
3

School of  Computer Science and Engineering
1.彩色基础
4
•光特性是色彩科学的核心。光可以区分为无色光和彩色光。
•无色光属性：只有亮度（表示：黑→灰→白）
•描述彩色光源的质量的三个基本量：
—辐射亮度：从光源流出的总能量，单位为瓦特(W)；
—发光强度：观察者从光源感知的总能量，单位为流明(红外的光强接近零)；
—亮度：主观描绘子，不可测量，体现发光强度的消色概念。
20世纪30年代拍摄的消色光电影
彩色光在电磁波谱中的波长范围是400~700nm，
超出此范围为红外线、紫外线等非可见波谱
太阳光可分解为紫、蓝、绿、黄、橙、
红等可见色谱

School of  Computer Science and Engineering
1.彩色基础
5
人眼锥状体是负责色觉的传感器，约600万~700万个，主要分3个感知类别，
对应于红(65%)、绿(33%)、蓝色(2%)
为标准化，1931年CIE（国际照明委员会）规定：蓝(435.8nm)、绿(546.1nm)、
红(700nm)，理论上存在无穷种三原色方案
人眼中的红、绿和蓝锥
状体对光的吸收曲线

School of  Computer Science and Engineering
1.彩色基础
6
•相加混色模型
—发光色叠加
—三原色：红、绿、蓝
—应用在显示屏
•相减混色模型
—利用滤光原理
（白光减去不需要的彩色）
—三原色：品红、黄、青
—应用在打印、颜料调色
光和颜料的原色及二次色

School of  Computer Science and Engineering
1.彩色基础
7
•如何区分不同颜色
—色调: 感知的主导色，跟主波长相关
—饱和度: 相对纯度，与一种色调混合的白光量
—亮度: 发光强度的消色概念
•色调和饱和度一起称为色度，一种颜色可以由亮度和色度来表征
•确定任一特殊颜色的方法:
—由其三色系数（红色量X，绿色量Y，蓝色量Z）来规定，
三色系数定义为
풙=
푿
푿+풀+풁
, 풚=
풀
푿+풀+풁
, 풛=
풁
푿+풀+풁
且풙+풚+풛=ퟏ
—由CIE色度图来规定
CIE色度图(纯
色在边界)

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
8

School of  Computer Science and Engineering
2.彩色模型
9
•彩色模型是以某种标准方式来规定颜色，本质上规定：
—坐标系
—坐标系内的子空间（模型内每种颜色都可由子空间内包含的点表示）
•常用的彩色模型有：
—RGB（红、绿、蓝）模型（针对彩色显色器和彩色摄像机开发）
—CMY（青、深红、黄）模型、CMYK（青、深红、黄、黑）模型（针对彩色
打印开发）
—HSI（色调、饱和度、亮度）模型（针对人们描述和解释颜色的方式开发，解除了
亮度和色彩信息的联系）

School of  Computer Science and Engineering
2.RGB彩色模型
10
原色
二次色
灰度值变化
24比特RGB彩色立方体及其对应隐藏面
RGB彩色图像传感器采集
成像（彩色显示器成像）
RGB模型（Red, Green, Blue）
—描述图像由3幅分量图像组成，即每种原色一幅分量图像
—对每个像素，每个通道为8比特，三通道共24比特，颜色总类有2
8x3
=16,777,216（全彩色）

School of  Computer Science and Engineering
2.CMY和CMYK彩色模型
11
CMY模型（Cyan, Magenta, Yellow）
—描述从物体反射出来的光（相减混色），二次色（颜料的原色）
—RGB与CMY模型的转换公式：
式中所有彩色值已归一化到[0,1]区间内










−










=










B
G
R
Y
M
C
1
1
1
CMYK模型（Cyan, Magenta, Yellow，K：black）
—C、M、Y组合产生黑色时只能产生模糊的棕色，深色效果较差，故加入黑色K
—CMY与CMYK 模型的相互转换公式（P286）
RGB、CMY、CMYK模型之间的转换简单，适合硬件实现。但都不能很
好的描述人脸实际解析的颜色。

School of  Computer Science and Engineering
2.HSI彩色模型
12
•HSI彩色模型（Hue、Saturation、Intensity）
亮度轴
RGB与HSI彩色模型之间的概念关系
饱和度
色调
夹角：色调
向量长度：饱和度
与亮度轴正交的平面
三角形、圆
形彩色平面
的HSI彩色
模型

School of  Computer Science and Engineering
2.从RGB到HSI的彩色变换
13
色调H分量由夹角求出
已知一幅RGB彩色格式图，每个RGB像素的H、S、I分量可由下式得出
饱和度S分量
亮度I分量

School of  Computer Science and Engineering
2.从HSI到RGB的彩色变换
14
RG扇区
已知HSI值在[0,1]内，求同一区间对应的RGB值，适用的公式取决于H存在的不同扇区:
GB扇区
BR扇区

School of  Computer Science and Engineering
2.操作HSI分量图像
15
RGB图像及其对应的HSI图像分量
H分量图像：灰
度值对应于角度
S分量图像：灰度
值对应于饱和度
I分量图像：灰度值
对应于平均灰度
修改左图中的HSI分量后，转换为最终的
RGB图像

School of  Computer Science and Engineering
2.设备无关彩色模型
16
•L*a*b*模型（CIELAB）
—设备无关的彩色模型，常用于计算机色调调整和彩色校正
—L*a*b*分量：
L∗=116h(Y/YW)−16
a∗=500[h(X/XW)−h(Y/YW)]
b∗=200[h(Y/YW)−h(Z/ZW)]
其中ℎ(푞)ቊ
3
푞,
7.787푞+16/116,
푞>0.008856
푞≤0.008856
, 푋
푤
,
푌푊
,
푍푤是参考白三色激励值
—L:亮度；a的正数代表红色，负端代表绿色；b的正数代表黄色，负端代表蓝色
—L*a*b*的值无法直接显示，需要转换到其它色彩空间
—亮度与颜色信息的分离程度大，在图像压缩与图像处理中应用广

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
17

School of  Computer Science and Engineering
3.假彩色图像处理
18
•根据一定准则对灰度值赋以彩色的处理
—主要应用于可视化和解释单幅图像或序列图像中的灰度事件（人类可以
分辨几千种色调和亮度，只能分辨20多种灰度）

School of  Computer Science and Engineering
3.灰度分层和彩色编码
19
灰度分层技术的图形解释
图像可描述为一个三维函数，分层方法是指平行图像坐标平面放置一些平面进
行“切割”相交的区域，并对平面上下区域赋予不同颜色
灰度分层技术的另一种表示
采用多种颜色进行灰度分层：令[0,L−1]表示灰度级，令l
0
表示黑色，令l
L
−
1
表示白色。假
设与灰度轴垂直的P个平面将其分为P+1个区间I
1
,I
2
,...,I
P
+
1
。
根据图像像素（x，y）的所处的灰度区域赋予伪彩色，规则如下：
若f(x,y)∈Ik，则令f(x,y)=ck
其中c
k
为与第k个灰度区间I
k
相关联的颜色。

School of  Computer Science and Engineering
3.灰度分层和彩色编码
20
甲状腺灰度图像分层编码结果
按照物理性质对焊接图像进行灰度分层及
颜色编码
使用彩色突出降雨量

School of  Computer Science and Engineering
3.灰度到彩色的变换
21
假彩色图像处理功能框图
假彩色图像增强凸显X射线下的爆炸物（左）及
其所采用的变换函数（右）
假彩色增强：对输入像素的灰度执行三个独立的转换（灰度级分段线性函数、
非线性平滑函数），分别将三个结果送进彩色显示器的红、绿和蓝通道，生成
一幅合成彩色图像。

School of  Computer Science and Engineering
3.灰度到彩色的变换
22
假彩色增强：对多幅单色图像组合为一幅彩色合成图像。
以多光谱图像为例，对多幅由不同传感器在不同波段产生的灰度图像进行处理（
结合每个波段的物理特性），并采用附加处理（彩色平衡和空间滤波等技术）
使用多幅灰度级图像的假彩色编码方法
红R、绿G、蓝B、近红外IR分量；
以及由IR、G、B合成图像；R、IR、B合成图像

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
23

School of  Computer Science and Engineering
4.全彩色图像处理基础
24
对灰度图像
的标量处理
对彩色图像
的向量处理
标量框架：分别处理每幅灰度级分量图像（像素值为标量），将处理后的各分量图
像合成一幅彩色图像。
向量框架：直接处理彩色像素，将彩色像素视为向量处理。
c=
푐
푅
푐
퐺
푐
퐵
=
푅
퐺
퐵
令c是RGB彩色空间的任意向量：
对应的空间坐标（x,y）的函数值：
c(x,y)=
푐
푅
(푥,푦)
푐
퐺
(푥,푦)
푐
퐵
(푥,푦)
=
푅(푥,푦)
퐺(푥,푦)
퐵(푥,푦)

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
25

School of  Computer Science and Engineering
5.彩色变换
26
彩色图像及其各个彩色空间分量
彩色变换是在单个彩色模型中处理彩
色图像的各个分量：
푠
푖
=푇
푖
(푟
푖
),푖=1,2,...,푛
式中,
n是分量图像的总数，
r
i
是输入分量图像的灰度值
s
i
是输出分量图像中空间上的对应灰度
T
i
是对r
i
操作产生s
i
的一组变换或颜色映
射函数
全彩色
CMYK空间
RGB空间
CMY空间
HIS空间

School of  Computer Science and Engineering
5.彩色变换
27
对应不同彩色模型图像分量所用到的变换函数
原图亮度降低30%
K=0.7

School of  Computer Science and Engineering
5.补色
28
彩色环
原图
彩色环：首先等距离地放置三原色，其次将二次色等距离地放置在原色之间
在彩色环上，与一种色调直接相对立的另一色调称为补色
补色变
换函数
RGB补色
HSI补色

School of  Computer Science and Engineering
5.彩色分层
29
突出图像中某个特定的彩色范围，将目标（某特定颜色）从周围分离出来：
“切割”彩色图像：把感兴趣区域之外的某些彩色映射为一种不突出的中性色
若感兴趣的颜色被宽度为W、中心在原型（即平均）颜色并具有分量（a
1
,a
2
,...,an）的立
方体包围，则必要的一组变换为
n
i
r
W
a
r
s
i
n
j
j
i
i
,...,
2
,
1
,
]
2
/
|
[|
,
5
.
0
1
=




−
=


其他
n
i
r
R
a
r
s
i
n
j
j
i
,...,
2
,
1
,
)
(
,
5
.
0
1
j
2
0
2
=






−
=

=
其他
式中R
0
是封闭球体半径，（a
1
,a
2
,...,a
n
）是球体中心的分量
用一个球体来规定感兴趣的颜色时，式子变为：
基于假设：在同一色彩空间下，相邻的点具有相近的颜色。

School of  Computer Science and Engineering
5.彩色分层
30
RGB立方体内彩色分层变换
RGB球体内彩色分层变换

School of  Computer Science and Engineering
5.色调和彩色校正
31
图像的色调范围（也称主特性），
是指图像中颜色亮度的一般分布。
高主特性图像中的大部分信息集
中在高亮度上；低主特性图像的
颜色主要集中在低亮度上；中主
特性图像颜色介于两者之间。
平淡彩色图
像色调校正
较亮彩色图
像色调校正
较暗彩色图
像色调校正

School of  Computer Science and Engineering
5.色调和彩色校正
32
色调特性校正完成后，
通常要解决彩色不平衡问题
彩色平衡一幅
CMYK图像

School of  Computer Science and Engineering
5.彩色图像的直方图处理
33
对彩色图像各幅分量进行直方图均衡
化容易产生错误的颜色，
对HSI彩色空间进行处理：均匀地分
布颜色亮度，保持色调不变
HSI彩色空间中的直方图均衡化
原图
RGB通道独立处理
原图
在I
通道
实施
直方
图均
衡
饱和
度调
整

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
34

School of  Computer Science and Engineering
6.彩色图像平滑和锐化
35
•彩色图像平滑
从对灰度图像平滑（标量灰度值）过渡到彩色图像平滑（分量向量）
在RGB彩色图像中，S
xy
是一组坐标，定义了中心（x,y）的一个邻域，这个邻域中RGB分
量向量平均值为
根据向量加法性质有
•彩色图像锐化
一个向量的拉普拉斯也是一个向量，其分量等于输入向量的各个标量分量的拉普拉斯


−
=
xy
S
t
s
t
s
c
K
y
x
)
,
(
)
,
(
1
)
,
(
c




















=






−
xy
xy
xy
Sts
Sts
Sts
tsB
K
tsG
K
tsR
K
yx
),(
),(
),(
),(
1
),(
1
),(
1
),(c
将向量分量视为几幅标量图像，
采用传统灰度级邻域处理，单
独地平滑RGB图像的每个平面













=

)
,
(
)
,
(
)
,
(
)]
,
(
[
2
2
2
2
y
x
B
y
x
G
y
x
R
y
x
c
分别计算每幅分
量图像的拉普拉
斯，可求全彩色
图像的拉普拉斯

School of  Computer Science and Engineering
6.彩色图像平滑和锐化
36
平滑处理每幅RGB图像分量以及HIS 的I分量
拉普拉斯锐化处理每幅RGB图像分量以及HIS 的I分量
两种处理结果的差别

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
37

School of  Computer Science and Engineering
7.HSI空间中的分割
38
如果要根据颜色来分割一幅图像，首先考虑HSI空间，因为颜色在色调图像中可方
便地表示
原图H图
S图
I图
S图二
值化
S图二
值化点
乘H
点乘结果
的直方图
分割结
果（提
取红色
区域）

School of  Computer Science and Engineering
7.RGB空间中的分割
39
令z代表RGB空间中的任意一点，a是分割颜色样本集的平均颜色向量，计算向量的欧式
距离
式中，下标R、G、B表示向量a和z的RGB分量，满足距离D(z,a)≤D
0
的点的轨迹是半径D
0
的一个实心球体（见左下图），距离小于该阈值的为相似区域
D(z,a)=[(z−a)
T
C
−
1
(z−a)]1/2
推广的距离测度式，C是待分割颜色范围内代表性的颜色样本协方差矩
阵，满足D(z,a)≤D
0
的点的轨迹是一个实心的三维椭球体
盒式分割，
计算简单

School of  Computer Science and Engineering
7.RGB vs. HSI 分割
40
原图
RGB分割效果
HSI分割效果

School of  Computer Science and Engineering
7.RGB边缘检测
41
R
3个独立平面形成的合成梯度图（交集）可导致错误的结果!
G
B 
彩色图像
一致
不一致

School of  Computer Science and Engineering
7.RGB边缘检测
42
•Di Zenzo法：不分通道计算梯度的处理方法
令r,g和b是沿RGB彩色空间的R、G、B轴的单位向量，定义向量
根据这些向量的点积，令g
xx
,gyy,gxy为
由此最大变化率方向可由以下角度给出
坐标（x,y）处方向的变化率为
R
G
B
x
x
x
    

=    +
+



u
r
g
b
R
G
B
y
y
y
    

=    +
+



v
r
g
b
2
2
2
T
xx
R
G
B
g
xxx



=    =    =
+
+

u  u    u  u
2
2
2
T
yy
R
G
B
g
yyy



=    =    =
+
+

v  v
v  v
T
xy
R   RG   GB   B
g
x   yx    yx   y
            
=    =    =++
            
u  v    u  v
1
2
1
(  ,   )tan
2
[]
xy
xxyy
g
xy
gg

−
=
−
()
()()
1
2
1
,cos 2,2sin 2,
2
[(    )(    )]
xxyyxxyyxy
F   x  yggggx  ygx  y



=+    +    −+


(  ,   )xy

School of  Computer Science and Engineering
7.RGB边缘检测
43
两种处
理方式
的残差
原图
Di 
Zenzo法
得到的梯
度图
三幅单
独的梯
度图相
加结果
R图
像梯
度分
量
G图
像梯
度分
量
B图
像梯
度分
量

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
44

School of  Computer Science and Engineering
8.彩色图像中的噪声
45
加性高
斯噪声
污染的R
分量图
像
加性高
斯噪声
污染的
G分量
图像
加性高
斯噪声
污染的
B分量
图像
合成的
RGB图
分析彩色图像中的噪声，以及
将图像从一个彩色模型转换到
另一个模型时，噪声如何传递
噪声图
像彩色
模型转
换为HSI
后对应
的分量
图像

School of  Computer Science and Engineering
8.彩色图像中的噪声
46
只有一个RGB通道受到噪声污染时，到HSI的转换会将噪声分布到所有
HSI分量图像上
仅仅绿色平
面被椒盐噪
声污染的
RGB图像
彩色模型转
换后的H分量
彩色模型转换
后的S分量
彩色模型转换
后的I分量

School of  Computer Science and Engineering
1.彩色基础
2.彩色模型
3.假彩色图像处理
4.全彩色图像处理基础
5.彩色变换
6.彩色图像平滑和锐化
7.使用彩色分割图像
8.彩色图像中的噪声
9.彩色图像压缩
第六章:彩色图像处理
47

School of  Computer Science and Engineering
9.彩色图像压缩
48
彩色图像表示所需的比特数比灰度级表示所需的比特数大3~4倍。
压缩可以减小或消除冗余和/或无关数据的处理，在存储和传输彩色图像
时作用非常明显。
原RGB图像
压缩并解压
后的图像

School of  Computer Science and Engineering
49
作业：
6.7，6.14，6.15 

School of  Computer Science and Engineering
50
•方法#1：采用HSI 空间，仅在I空间上作直方图均衡
•方法#2:  采用RGB空间，分别在RGB 三个空间上作直方图均衡
编程：对下图像进行不同方法的直方图均衡、并作对比分析：
