# Chap4 频率域滤波

> 主题库：数字图像处理　｜　源文件：Chap4 频率域滤波.pdf　｜　32,673 字符　｜　137 个片段　｜　更新于 2026-11-13


第四章. 频率域滤波
骆伟祺
中山大学计算机学院
Email：luoweiqi@mail.sysu.edu.cn
1

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
2

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
3

School of  Computer Science and Engineering
1.傅里叶
4
➢1822年，傅里叶（Fourier）发表的“热传导解析理
论”，提出了利用傅立叶变换来分析热传导现象。
傅里叶1768—1830
法国，数学和物理学家
➢傅里叶变换能使我们从空域（or 时域）与频率域两
个不同的角度去观察信号。有时在空域很难解决的
问题，在频域却变得非常简单。
➢傅里叶变换是十九世纪数学界和工程界最辉煌的成
果之一。它一直是信号处理领域中最完美、应用最
广泛、效果最好的一种分析手段。

School of  Computer Science and Engineering
1.傅里叶级数v.s. 傅里叶变换
5
▪傅里叶级数：满足某些数学条件(如绝对可积等)的实周期函数푓(푡)
（周期为T）可表示为正弦/余弦函数的级数形式。
2
22
(t)= (cos()sin())
n
nn
jt
T
nn
nn
nn
fc ectjt
TT


==
=−=−
=+

/2
2
/2
1
( )
,
0,   1,   2,
T
n
jt
T
n
T
c
f  t e
dt  n
T

−
−
=
=   

其中，系数

+

−
=
du
e
u
F
f
ut
j

2
)
(
)
t
(
▪傅里叶变换：满足某些数学条件（如曲线下的面积有限）的非周期
（或周期）函数푓(푡)可表示为正弦/余弦函数的积分形式。
2
(  )
(t)
j
ut
F  u
f
e
dt

+
−
−
=

且存在逆变换
欧拉公式
复数形式
复数，一般不为实数
傅里叶级数得到离散频谱、而傅里叶变换得到连续频谱。

School of  Computer Science and Engineering
1.一元信号示意图
6
某一复杂函数
时域信号
4个简单的三角基函数
c
1
c
2
c
3
c
4
频域信号
基
系数
注：傅里叶变换基采用正弦和余弦。其它常见的基函数，如DCT，小波等
重构（逆变换）
分解（变换）

School of  Computer Science and Engineering
1.图像空域滤波vs.频率滤波
7
空间滤波：一步一个脚印
频域滤波：牵一发而动全身
频谱滤波
原图
傅里叶变换
原图
傅里叶
逆变换
处理后结果

School of  Computer Science and Engineering
1.小结
8
▪傅里叶变换可以将空域信号转换为正弦、余弦（基函数）的积分形式，
其频谱系数（通常为复数）表示了原函数中包含相应基函数成分多少。
傅里叶变换存在相应的逆变换。
将复杂信号分解为简单信号；为观测复杂信号提供了新视角（频谱）。
▪图像频率域处理：即利用基函数的特性，通过对频谱系数的修改从而达
到对图像去噪、增强、压缩等处理。
在空域不好解决的问题，在频域上可能变得非常容易（性能及时间上）。
▪傅里叶系数（频谱）计算需要用到函数在整个空间上的分布情况。
不同于空域像素的调整，对频谱系数修改会作用于整个空域图像。

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
9

School of  Computer Science and Engineering
2.复数
10
▪复数可以表示为复平面上的一个有序实数对(푥,푦)
复平面
(x, y)
(x, 0)
(0, y)
实轴
虚轴
x
y
O
点表示：푧=（푥,푦）
1.实数可以表示为实轴上的点，它是复数的子集；
2.两复数之间不能比较大小，只有相等或不等关系。
x = Rez(Real part); 
y = Imz(Imaginary part)
z
1
=z
2
iff
1.Rez
1
= Rez
2
2.Imz
1 
= Imz
2
Q: z
1
<z
2
? 

School of  Computer Science and Engineering
➢加法
(x
1
, y
1
)  + (x
2
, y
2
) = (x
1
+x
2
, y
1
+y
2
)   
➢乘法
(x
1
, y
1
) (x
2
, y
2
) = (x
1
x
2
-y
1
y
2
, y
1
x
2
+x
1
y
2
)   
2.复数的基本运算：加法和乘法
1.when y
1
=0, y
2
=0, the above operations reduce to the usual operations of
addition  and multiplication for real numbers.
2. Any complex number z= (x,y) can be written z = (x,0) + (0,y)
3. Let j be the pure imaginary number (0,1), then  
z = x (1, 0) + y (0,1) = x + j y, x & y are real numbers
j
2 
=(0,1) (0,1) =(-1, 0) ➔j
2
=-1
11
减法是加法逆运算
除法是乘法逆运算

School of  Computer Science and Engineering
2.复数的极坐标表示
12
▪非零复数可以用复平面上的极坐标形式表示
(x, y)
x
O
r
θ
(cossin   )zxjyrj=  +  =+
22
|   |
0
r
z
x
y
=  =    +   
复数的模
arctan(   )    2
,
y
k
k
Z
x

=
+

复数的复角
注：1、复角arg(z)是一个集合，各元素间相差2π整数倍；
2、复角主值Arg(z)是一个限制在(-π,+π]的数
引入欧拉公式
||
i
zz  e

=

School of  Computer Science and Engineering
▪欧拉公式
2. 欧拉公式
13
cos
sin
j
ej


=+
2
2
1
2
2
2
2
2
1
0
1
0
1
2
1
2
1
01
1
1
1
1
(   )
(   )
( )
[
( )
]
(2  )!
(2
1)!
(2  )!
(2
1)!
11
(  1)
(  )
[
(  1)
(  )
]
(2  )!
(2
1)!
j
n
n
n
n
n
n
n
n
n
n
n
n
n
n
nn
e
i
i
i
j
i
n
n
n
n
j
nn






+
+
+
+
−
−
−
=
=
=
=
+
+
−−
==
=
+
=
+
−−
=   −
+
−
−





cosθ泰勒展开
sinθ泰勒展开
1
2
3
0
2
2
1
00
1
1
1
1
1
1
...
...
1!
2!
3!
!
!
11
(2  )!
(2
1)!
x
n
n
n
nn
nn
e
x
x
x
x
x
nn
xx
nn
+
=
+
+
+
==
= +    +
+
+  +
+  =
=+
+


泰勒展开
奇偶分开
令푥=푗휃代入

School of  Computer Science and Engineering
▪Product in exponential form
2. 复数乘法的极坐标表示
12
12
1122
12121212
()
1212
(cossin    )(cossin)
(coscossinsin)(sincoscos    sin)
cos()sin()
ii
i
e   eii
i
ie



            
    
+
=++
=−++
=+   ++   =
1212
()
1   2121  2
()()
iii
z zr er er r e
  +
==
11
111
(   )(),0,  1,  2,...
iin
nnn
zr er  en

===   
12
1122
&
ii
zr ezr e

==
14
两复数푧
1
,푧
2

School of  Computer Science and Engineering
2. 复数乘法几何意义
1
2
1
2
()
1   2
1
2
1  2
(
)(
)
j
j
j
z z
r e
r e
r r e


  
+
==
argz
1
z
2
=θ
1 
+θ
2 
+2(n
1
+n
2
)π
=(θ
1 
+2n
1
π)+(θ
2 
+2n
2
π)
=argz
1
+argz
2
arg(z
1
z
2
)=θ
1 
+θ
2 
+2nπ, n=0, ±1, ±2 ...
θ
1 
is one of arguments of z
1
and 
θ
2 
is one of arguments of z
2 
then 
θ
1 
+θ
2 
is one of arguments of z
1
z
2
Here: n
1
and n
2 
are two integers with n
1
+n
2
=n
Q: Argz
1
z
2
=Argz
1
+Argz
2
?
12
1
1
2
2
&,
jj
Ifz
r e
z
r e
then

==
15

School of  Computer Science and Engineering
2. 共轭复数及性质
x
y
O
z(x,y)
z (x,-y)
16
zz=
|   |  |   |zz=
基本性质：
Re, Im
22
z    zz    z
zz
i
+−
==
1
2
1
2
z
z
z
z
+  =  +
1212
zzzz  =  
1212
zzzz−  =  −
11
2
2
2
,0
zz
z
z
z
=
四则运算：
222
()   ()||z zxjyxjyxyZ=  +     −   =  +   =
复数与其共轭复数相乘：

School of  Computer Science and Engineering
实系数形式
2. 傅里叶级数
17
▪傅里叶级数：满足某些数学条件(如绝对可积等) 的实周期函数푓(푡)
（周期为T）可表示为正弦/余弦函数的级数形式。
2
22
(t)= (cos()sin())
n
nn
jt
T
nn
nn
nn
fc ectjt
TT


==
=−=−
=+

/2
2
/2
1
( )
,
0,   1,   2,
T
n
jt
T
n
T
c
f  t e
dt  n
T

−
−
=
=   

其中，系数
复数形式
,,
n
n
n
n
n
c
j
R
R
    

=   +


令
22
(t)
 
[
cos(
)
sin(
)]
n
nn
n
nn
f
t
t
R
TT


=
=−
=
−


0
1
22
(t) [2cos()    2sin()]
nn
n
nn
ftt
TT



=
=   +−

nn
cc
−
=注：
0
1
2
2
2   (
)
2   (
)
{[
cos(
)
sin(
)]   [
cos(
)
sin(
)]}
n
n
n
n
n
n
n
n
n
n
t
t
t
t
T
T
T
T









=
−−
=
−−
=   +
−
+
−

n
n
n
n
          
−−
=
= −
；

School of  Computer Science and Engineering
2.冲激及取样
18
冲激定义（由英国科学家保罗·狄拉克提出，1902－1984）
  
0
)t(




=
=
0
0
t
t
，
，

且
1
=



−
dt
)
t
(

连续情形
满足在0处无穷大，其它取值为0的pdf （概率密度函数）。并非真正意义上的函数。
通过积分取样
)
(
dt
)
t
(
)
(
0
f
t
f
=



−

)
(
dt
)
t
-
t
(
)
(
0
0
t
f
t
f
=



−

0点处
任一点푡
0
处
)()x-x()(
00
xfxf
x
=


−=

(  )    1
x
x

=−
=

易知
(  )  (  )(0)
x
f   xxf

=−
=

离散情形
通过级数取样
0点处
任一点푡
0
处
满足在0处为1，其它取值为0的pmf（概率质量函数）。是一个普通函数。
1
if 
0
()
0
if 
0
x
x
x

=

=




School of  Computer Science and Engineering
2.冲激串
19
连续
离散
单个冲激
( )
(
)
T
k
s
t
t    k   T



=−
=
− 

周期为∆T的冲激串
0
()
tt

−

School of  Computer Science and Engineering
2.一维连续函数傅里叶变换及反变换
20


2
(  )
( )
(t)
j
ut
F  u
f  t
f
e
dt
u


+
−
−
=
=
−    +

，
12
( )
{   (   )}
(  )
j
ut
f  t
F
F  u  e
du
t


+
−
−
=
=
−   +

,
傅里叶逆变换：从频域퐹(푢)转换为空域푓푡
注：퐹(푢)一般是复数！无法在一个二维坐标下显示，可用模|퐹푢|。
在一定条件下，一元连续实函数푓푡，−∞<푡<+∞，可根据如下公
式进行傅里叶正变换
( )(  )f  tF  u
傅里叶变换对

1
(   )( )  ,   ( ){   (   )}Ff  tf  tF  
−
==
相差一个负号

School of  Computer Science and Engineering
2.方波的傅里叶变换
21
方波函数
实数，一般情况是复数

School of  Computer Science and Engineering
2.常数函数的傅里叶变换
22
常数函数푓푥=1的傅里叶变换是一个在휇=0处的冲激函数，证明略
1
푓푥=1
푥
푓(푥)
傅里叶变换
푓푢=훿(푢)
u
00
2
{1}
1
(   )
jt
e
dt


 
+
−
−
=

=
2
()
jt
ted


+
−
−

=
冲激函数的另一种形式的定义：

School of  Computer Science and Engineering
2.冲激的傅里叶变换
23
位于푡−푡
0
处冲激的傅里叶变换
0
2
2
0
(   )()
jt
jt
Ft    t   edt   e




−
−
−
=−=

相当于对此函数在푡=푡
0
处取样
x
y
O
1
几何意义：模为1，复角为-2πμt
0
-2πμt
0
|퐹푢|=1,∀푢∈푅

School of  Computer Science and Engineering
2.冲激串的傅里叶变换
24

22
11
(   )( ){}
1
nn
jtjt
TT
T
nn
n
Sstee
TT
n
TT

  




=−=−

=−

===




=−





周期为1/ ∆T的冲激串
周期为∆T的冲激串的可表示为傅里叶级数形式
22
1
()
nn
nn
j
t
j
t
TT
Tn
nn
s
t
c e
e
T

=
=


=−
=−
==


对该级数（即冲激串）再作傅里叶变换
2
()
()
n
n
jt
T
Tn
n
k
t    k   T
s
t
c e


=



=−
=−
−   =
=


/2
2
/2
1
( )
,
0,   1,   2,
T
n
jt
T
nT
T
c
s
t e
dt  n
T


−


−
=
=   


公式（4.8）
公式（4.9）
/2
22
0
/2
1111
( )( )
T
nn
jtjt
TT
n
T
ct edtt edte
TTTT



−−

−−
====


[-∆T/2, +∆T/2]只有0点非零
取样
先变换再求和

School of  Computer Science and Engineering
2.卷积定理
25
卷积定理#1：空间域两个函数的卷积的傅里叶变换= 频率域中两个
函数的傅里叶变换的乘积
(
)( )
(  )  (t-  )d
f    h   t
f
h
      

−
=

两个连续函数푓(푡)和ℎ(푡)的卷积定义如下：
(
)( )
(  )
(  )
f    h   t
F  u    H  u



(
)( )
(
)(  )
f   h   t
F   H   u

   
卷积定理#2 ：空间域两个函数的乘积的傅里叶变换= 频率域中两个
函数的傅里叶变换的卷积
注：1. 函数的乘积和卷积均满足交换律。
2. 卷积定理是频率域滤波的基础。

School of  Computer Science and Engineering
2.卷积定理证明
26
22
[(
)( )]
[(
)( )]
d
(  )  (
)
d
j
t
j
t
f    h   t
f    h   t   e
t
f
h t
d
e
t



  
+
+    +
−−
−
−    −


=

=
−



  
F
2
(  )[
(
) e
d  ]d
jt
f
h t
t




+
+
−
−
−
=−

交换积分顺序
2
2
(
)
(  )
[
(
)
d(
)]d
j
j
t
f
e
h t
e
t

   


   
+
+
−
−
−
−
−
=
−
−

换元，凑成ℎ函数的傅里叶变换
2
(  )(   )]d
j
feH

   
+
−
−
=

2
(   )
(  )
d
j
H
f
e




+
−
−
=

푓函数的傅里叶变换
(   )   (   )
(   )
(   )
H
F
F
H
   
   
==
交换律
傅里叶变换定义
卷积定义

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
27

School of  Computer Science and Engineering
为什么需要取/采样：计算机无法处理连续、无穷的数据，需
要对连续信号进行取样及量化处理，使之离散化及有限化。
3.采样
例子：考察对连续函数푓(푡), -∞<푡<+∞进行均匀间隔∆푇的采样
利用冲激串函数采样
( )
(x-n
)
T
n
s
t
T



=−
=

28

School of  Computer Science and Engineering
3.采样
29
 
)
(
)
(
)
(
)
(
)
(
~


=
−
=


−
=
=
n
n
T
T
n
t
t
f
t
s
t
f
t
f

)
(
)
(
)
(
T
k
f
dt
T
k
t
t
f
f
k

=

−
=



−

对取样后函数积分得到取样点的值
取样后函数= 푓×冲激串
加权脉冲

School of  Computer Science and Engineering
3.采样后函数的傅里叶变换
30
( )
( )
( )
T
f  t
f  t  s
t

=
取样后函数= 푓(푡)×冲激串
(   ){   ( )( )}(   ) *   (   )(  )   ()
T
Ff  t  stFSFSd             
+

−
===−

相乘形式
卷积定理#2 ：
空间域两个函数的乘积的傅里叶变换= 频率域中两个函数的傅里叶变换的卷积


1
(   )
( )
T
n
n
S
s
t
TT
  
 


=−

=
=
−




已知，脉冲串的傅里叶变换为
1
(   )
(  )   (
)
(  )
n
n
F
F
S
d
F
d
TT

       

 
  
+
+

=−
−
−

=
−
=
−   −





1
()
n
n
Fd
TT
    
+

=−
−

=−   −





表示在τ=μ−
n
∆T
时的一个脉冲（令μ−
n
∆T
−τ=0）
1
()
n
n
F
TT


=−
=−


在脉冲处取样

School of  Computer Science and Engineering
3.采样后函数的傅里叶变换
31
( )
( )
( )
T
f  t
f  t  s
t

=
是离散的（只在冲激处非零）
n
1
(   )(   -)
n
FF
TT


=−
=


n
1
(   )(   -)
n
FF
TT


=−
=


是连续的，由采样前函数푓(푡)
的傅里叶频谱퐹(휇)作周期性为1/∆푇的
周期延拓（平移&叠加）得到
脉冲周期为∆T
例子：若某一函数푓푡的傅里叶变换频谱퐹(휇)如下图所示
带限(band-limited)信号
-μ
max
+μ
max
分析在不同采样周期下的
n
1
(   )
(   -
)
n
FF
TT


=−
=



School of  Computer Science and Engineering
3.采样后函数的傅里叶变换
32
过采样（Over-sampling）
采样间隔ΔT很小（即采
样率1/Δ푇很大）
临界采样（Critically Sampling）
欠采样（Under-sampling）
采样间隔ΔT很大
频谱混叠（aliasing），无法正确重构原信号

School of  Computer Science and Engineering
퐹휇=0,|휇|>휇
max
3.采样定理
33
采样定理：如果一个信号的最高频率为휇
푚푎푥
（即信号是带限的，其频谱在
휇
푚푎푥
之外为零）, 以一个采样率푓
푠
=1/Δ푇对这个信号进行采样，只要满足
max
1
2
s
f
T

=

就可以完全恢复原始信号。
奈奎斯特频率(Nyquist rate)
1. 如果푓
푠
<2휇
푚푎푥
，原始信号中的高频会与低频混叠在一起，重建时会出伪影或失真。
2. 如果信号不是带限的，采样后的信号可能包含高频分量的混叠。

School of  Computer Science and Engineering
3.信号重建
34
信号恢复：采样后信号频谱是采样前信号频谱的周期延拓。因此，可以
通过一个理想低通滤波器对进行滤波，分割出一个采样前信号频谱。
~
(   )
(   )    (   )
F
H
F

   
=
max
max
,
()
0,
T
H
others
      

   −      

=


~
22
( )
(   )
(   )    (   )
j
t
j
t
f  t
F
e
d
H
F
e
d




   

+
+
−
−
==

信号重建：傅里叶逆变换

School of  Computer Science and Engineering
3.混叠
35
混叠指取样后不同信号变得彼此无法区分的现象，或者一个信号“伪
装”成另外一个信号的现象。
相同的采样率对不同连续函数采样
相同的采样后函数

School of  Computer Science and Engineering
3.混叠
36
讨论：如果以低于函数最高频率的两倍的取样，连续带限函数能否被恢复？
（欠取样→混淆，高频分量的信息就会混叠到低频分量中）
现实中混淆不可避免（
如非带限函数），可通
过平滑原函数减少高频
来降低混淆的影响。

School of  Computer Science and Engineering
3.混叠
37
考虑函数sin(πt)，其周期为2s ，即最高频率μ
max
=1/2.理论上，采样率1/∆T>1
时，即∆T <1时就可恢复此信号。
图下图所示，如果∆T大于1，会出现以下结果（蓝点也能拟合出另一个正弦函数，
但是周期与原函数不同）。

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
38

School of  Computer Science and Engineering
离散傅里叶变换DFT
4. 连续傅里叶变换到离散傅里叶变换
39
 
)
(
)
(
)
(
)
(
)
(
~


=
−
=


−
=
=
n
n
T
T
n
t
t
f
t
s
t
f
t
f

푓(푡)是连续函数，对其取样后函数
作傅里叶变换
~
22
(   )
( )
( )
( )   (
) 
n
j
t
j
t
T
n
F
f  t  s
t  e
dt
f  t
t    n  T   e
dt



+
+
=
−−

=−
−
−
=
=
− 


2
( )   (
)
n
jt
n
f  t
t    n  T  e
dt


+
=
−
=−
−
=
− 


交互求和与积分顺序
2
()
n
j
n  T
n
f  n  T  e

=
−
=−
=

冲激函数取样特性
2
n
j
n  T
n
n
fe

=
−
=−
=

푓
푛
是离散函数（以周期为∆푇对푓(푡)进行采样得到）
但存在周期为1/∆푇的连续频谱
假设我们对连续频谱
ത
퐹在휇=0,휇=1/∆푇一个周期内等间距地取푀个样本
1
,0,1,...,1
m
mM
MT
=   =−

1
2/
0
,
0,1, 2,...,
1
M
j
mn  M
mn
n
F
f  e
m
M

−
−
=
=
=
−

得到离散频谱
存在反变换
作业4.15
1
2/
0
1
,0,1, 2,...,1
M
jmn  M
nm
m
fF  enM
M

−
=
==−


School of  Computer Science and Engineering
4. 单变量离散傅里叶变换（DFT）
40
定义离散函数푓(푥)，푥=0,1,...,푀−1的离散傅里叶变换(DFT)公式: 
1
2/
0
(  )
(  )
     
0,1,
,
1
M
j
ux  M
x
F  u
f   x e
u
M

−
−
=
=
=
−

可以证明：M是离散傅里叶正反变换的周期
(  ) 
    (
)
(  ) 
   (
)
F  u
F  u    kM
f   x
f   x    kM
=
+
=    +
、
110
1
1
0
2
−==

−
=
MxeuF
M
xf
M
x
Muxj
,,,     )()(
/


及其反变换IDCT
时域、频域都是离散的，可以在计算机上进行计算。
注：这里假设了信号푓(푥)作周期性延拓，周期为M
而且푒
−
푗2휋푢푥
푀
也是一个周期为푀的函数（푥,푢均为整数）

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
41

School of  Computer Science and Engineering
5. 二维冲激及其取样特性
42
二维连续单位冲激定义
采样特性
if 
0
( ,  )
0
otherwise
tz
tz

==
+

=


&( ,  )1t  z  dtdz

−  −
=

( ,  )  ( ,  )
(0, 0)
f  t  z
t  z  dtdz
f


−  −
=

0
0
0
0
( ,  )  (
,
)
(   ,    )
f  t  z
t    t   z    z   dt dz
f  t   z


−  −
−   −
=

二维离散单位冲激定义
1
if 
0
(  ,   )
0
otherwise
xy
xy

==

=


(  ,   )  (  ,   )(0, 0)
xy
f   x  yx  yf

=−  =−
=

0000
(  ,   )  (,)(   ,)
xy
f   x  yx    x   y    yf   x   y

=−  =−
−    −   =

采样特性

School of  Computer Science and Engineering
43
二维傅里叶变换是一维情形向两个方向的简单扩展. 

+

−
−
=
dt
e
f
u
F
ut
j

2
)
t
(
)
(
vz
ut
z
t
v
u
ut
T
+
=
→
)
,
)(
,
(
5. 二维连续傅里叶变换对


+

−
+

−
+
−
=
dtdz
e
z
t
f
v
u
F
vz
ut
j
)
(
)
,
(
)
,
(

2
二维正变换


+

−
+

−
+
=
dudv
e
v
u
F
z
t
f
vz
t
j
)
(
)
,
(
)
,
(


2
二维逆变换
二维盒式函数
一维正变换

School of  Computer Science and Engineering
5. 二维取样和二维取样定理
44
二维取样（基于二维冲激串）
( ,  )( ,  )( ,  )( ,  )   (,) 
mn
TZ
mn
f  t  zf  t  z  st  zf  t  zt    m  T  z    n  Z
=   =

=−  =−
==−     − 

过取样
欠取样
二维采样定理：如果一个二维带限连续函数在푢和푣两个方向上由以大于该函数最
高频率两倍的取样率取样获得的样本表示，则没有信息丢失（完整恢复）。
max
max
11
22
v
TZ



，
分别在푢,푣方向考察
二维冲激串

School of  Computer Science and Engineering
5. 图像中的混叠
45

School of  Computer Science and Engineering
5. 图像中的混叠
46
考虑水平方向：采样周期为1，即采样率固定为1。
为了无损恢复信号，要求信号的最大频率不超过1/2.
频率：1/16<1/2
频率：1/6<1/2
频率：1/0.95>1/2频率：1/0.48>1/2

School of  Computer Science and Engineering
5. 图像中的混叠
47
原始图像
先缩写至33%, 再利用像素
复制方法恢复到原始大小
先对原图进行模糊处理，再
进行缩小和像素复制
混叠情况严重
模糊可以消除部分高频，
使得混淆情况不严重

School of  Computer Science and Engineering
5. 图像中的混叠
48
模式叠加
莫尔纹：由近似等间隔两个光栅叠加所产生的一种视觉现象
打印产生

School of  Computer Science and Engineering
5. 二维离散傅里叶变换及其反变换
49
设푓(푥,푦)是一幅尺寸为푀×푁的图像, 二维离散傅里叶变换(DFT)及反变
换(IDFT)定义如下
傅里叶谱为：F(u, v) = [R
2
(u, v) + I
2
(u, v)]
1/2

−
=
−
=
+
=
1
0
1
0
2
1
M
u
N
v
NvyMuxj
evuF
MN
yxf
)//(
),(),(

0,1, 2,...,1,0,1, 2,...,1;   (  ,  )uMvMF  u  vC=−   =−
其中，
11
2   (
/
/    )
00
(  ,  )
(  ,   )
MN
j
ux  M   vy  N
xy
F  u  v
f   x  y  e

−−
−+
==
=

DFT：
IDFT：
0,1, 2,...,
1,
0,1, 2,...,
1;   (  ,   )
x
M
y
N
f   x  y
R
=
−   =
−

其中，

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
50

School of  Computer Science and Engineering
6. 空间频率间隔关系、平移特性
51
空间间隔∆푥,∆푦与频率间隔∆푢,∆푣关系：
x
M
u

=

1
y
N
v

=

1
频谱作平移
11
2   (
/
/    )
00
(  ,  )
(  ,   )
MN
j
ux  M   vy  N
xy
F  u  v
f   x  y  e

−−
−+
==
=

00
11
2   ((
)   /
(
)   /    )
00
00
(
,
)
(  ,   )
MN
j
u   u    x  M
v   v
y  N
xy
F  u    u   v    v
f   x  y  e

−−
−
−
+  −
==
−    −   =

00
11
2   (
/
/    )
2   (
/
/    )
00
[   (  ,   )
]
MN
j
u  x  M   v  y  N
j
ux  M   vy  N
xy
f   x  y  e
e


−−
+
−+
==
=

00
2   (//    )
00
(  ,   )(,)
ju  x  M   v  y  N
f   x  y  eF  u    u   v    v
+
    −    −
空间样本间隔、与频率样本间隔及样本数量成反比
频谱平移后作傅里叶反变换，得到的复数模不变
00
2   (
/
/    )
00
(
,
)
(  ,  )
j
u  x  M   v  y  N
f   x    x   y
y
F  u  v e

−+
−    −   
类似地
信号平移后作傅里叶变换，得到的复数模不变
00
2   (
/
/    )
|
|   1
j
u  x  M   v  y  N
e

+
=
注: 复角增加（依赖x&y）
)
2휋(푢
0
Τ
푥푀+푣
0
Τ
푦푁

School of  Computer Science and Engineering
6. 旋转特性
52
00
1
1
1
1
2   (
/
/    )
2   (
/
/    )
0
0
0
0
(   (  ,   )
)
(  ,   )
M
N
M
N
jj
j
ux  M   vy  N
j
ux  M   vy  N
x
y
x
y
f   x  y  e
e
f   x  y  e    e


−   −
−   −
−
+
−
+
=   =
=   =
=


(  ,   )
(  ,  )
f   x  y
F  u  v

假设存在傅里叶变换对
将信号푓(푥,푦)旋转휃
0
角度后作傅里叶变换
00
11
2   (
/
/    )
00
(  ,   )
(  ,  )
MN
jj
j
ux  M   vy  N
xy
e
f   x  y  e
e    F  u  v


−−
−+
==
==

cos   ,sin   ;
cos   ,sin
xryr
uv

         
==
==
利用
相当于频谱也做了相应的旋转
00
(  ,)(   ,)f  rF     +   +
给出极坐标下的傅里叶变换对
(  ,   )(   ,   )f  rF 

School of  Computer Science and Engineering
6. 周期性及中心化处理
53
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
N
k
y
M
k
x
f
N
k
y
x
f
y
M
k
x
f
y
x
f
2
1
2
1
+
+
=
+
=
+
=
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
N
k
v
M
k
F
N
k
v
F
v
M
k
F
v
F
2
1
2
1
+
+
=
+
=
+
=




类似一维信号，二维信息同样假设做了周期性延拓操作，因此
频谱中心化
00
2   (
/
/    )
00
(  ,   )
(
,
)
j
u  x  M   v  y  N
f   x  y  e
F  u    u   v    v

+
    −    −
00
,
22
MN
uv==
代入上式
()
(  ,   )
(  ,   )(  1)
j
x   y
x   y
f   x  y  e
f   x  y

++
=−
(
,
)
22
MN
F  u
v
    −
−
低频集中在中心位置附近，离
中心越远，频率越高。

School of  Computer Science and Engineering
6. 对称性
54
实函数푓(푥,푦)的傅里叶变换是共轭对称的：
(  ,  )
(
,
)
F  u  v
F
u    v
=  −  −
11
2   (//    )
00
(  ,  )(  ,   )
MN
jux  M   vy  N
xy
F  u  vf   x  y  e

−−
−+
==
=

1
1
1
1
2   (
/
/    )
2   (
/
/    )
0
0
0
0
(
,
)
(  ,   )
(  ,   )
M
N
M
N
j
ux  M   vy  N
j
ux  M   vy  N
x
y
x
y
F
u    v
f   x  y  e
f   x  y  e

−   −
−   −
−
−
−
+
=   =
=   =
−  −  =
=


11
2   (//    )
00
(,)(  ,   )
MN
jux  M   vy  N
xy
Fu    vf   x  y  e

−−
+
==
−  −  =

11
2   (//    )
00
(  ,   )(  ,  )
MN
jux  M   vy  N
xy
f   x  y eF  u  v

−−
−+
==
==

푓푥,푦是实数

School of  Computer Science and Engineering
6. 傅里叶谱和相角
55
22
(  ,  )
|    (  ,  ) |
(  ,  )
(  ,  ),   (  ,  )    arctan
(  ,  )
I  u  v
F  u  v
R   u  v
I   u  v
u  v
R u  v


=
+
=


(   ,  )
(  ,  )
(  ,  )
(  ,  )   |    (  ,  ) |
j
u v
F  u  v
R u  v
jI  u  v
F  u  v    e

=
+
=
实函数푓(푥,푦)作傅里叶变换后得到的퐹(푢,푣)一般情况下是复数
傅里叶谱（频谱）：非负实数
相角（相位谱）：实数
(  ,  )(,)F  u  vFu    v=  −  −
共轭对称
|    (  ,  ) |  |    (
,
) |  |    (
,
) |
F  u  v
F
u    v
F
u    v

=   −  −  =   −  −
频谱关于原点对称
(  ,  )(,)u  vu    v= −  −  −
相角关于原点反对称

School of  Computer Science and Engineering
6. 傅里叶谱和相角
56
原图
频谱
中心化
频谱
对数中心
化频谱
11
00
(0, 0)
(  ,   )
MN
xy
F
f   x  y
−−
==
=

直流分量DC，与其他频率
相比该值非常大

School of  Computer Science and Engineering
6. 傅里叶谱和相角
原图
中心矩形
物体平移
物体旋转
57

School of  Computer Science and Engineering
6. 使用傅里叶谱和相角重建图像
58
小孩图像
小孩相角
仅用小孩相角重建
仅用小孩
频谱重建
小孩相角+矩形频谱重建矩形相角+小孩频谱重建
相角更多体现了
图像的形状信息

School of  Computer Science and Engineering
6. 其他一些性质
59
F*表示F的共轭
书中相关内容自行学习

School of  Computer Science and Engineering
6. 二维离散卷积定理
60
二维循环卷积
11
00
(   *  )(  ,   )
(   ,   )  (
,
)
MN
mn
f    h   x  y
f  m n h  x    m  y    n
−−
==
=
−    −

0,1, 2,...,
1;
0,1, 2,...,
1
x
M
y
N
=
−   =
−
二维卷积定理
(   *  )(  ,   )
(
)(  ,  )
f    h   x  y
F  H   u  v

空域上的卷积处理
（第三章内容）
频域上的乘积处理
（本章内容）
构建了“空域卷积”和“频域乘积”的桥梁，是频率域中线性滤波的基础。
1
()(  ,   )(   *    )(  ,  )f  h   x  yF   H   u  v
MN

注：类似一维的情况，原始信号均作了周期性延拓。

School of  Computer Science and Engineering
6. 避免交叠错误
61
对两个周期函数进行卷积，因此卷积本身是周期的。若各周期靠得太近，会
导致交叠错误。
可以证明(Brigham1988 )：假设两个函数푓(푥) 和ℎ(푥), 他们分别由퐴个样本
和퐵各样本组成。若在两个函数中填充0，使得它们长度均为P，当
푃≥A+퐵−1
则可避免交叠错误。
二维场景：设푓(푥,푦)和ℎ(푥,푦)分别为퐴×퐵，퐶×퐷像素图像它们循环
卷积中的交叠错误可以通过对两个函数填充0避免，具体如下
(  ,   ), 0
1& 0
1
(  ,   )
0,
or
p
f   x  y
x
A
y
B
f    x  y
A
x
P   B
y    Q
    −
    −

=

  
  

(  ,   ), 0
1& 0
1
(  ,   )
0,
or
p
h  x  y
x    C
y
D
h   x  y
C
x
P   D
y    Q
    −
    −

=

  
  

1&1PA   CQB    D  +  −  +  −
特别地，当푓、ℎ均为푀×N,  一般将其扩充为푃=2푀,푄=2푁偶数形式

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
62

School of  Computer Science and Engineering
7. 频率域滤波的一般处理流程
63
预处理
输入图像푓(푥,푦)
傅里叶变换
퐹(푢,푣)
퐹
′
푢,푣=퐻(푢,푣)퐹푢,푣
滤波器퐻(푢,푣)构造
逆傅里叶变换
后处理
输出图像g(푥,푦)
e.g.,0扩充、中心化等
e.g.,图像裁剪等
注：对퐹(푢,푣)修改不像空域那么直观，且频域任一点的修改都会影响空域全局。
关键
1
(  ,   )Re    {[    (  ,  )   (  ,  )]}g  x  yalH  u  v  F  u  v
−
=

School of  Computer Science and Engineering
7. 频率域滤波基础知识
64
原图
傅里叶谱
在±45方向有强边缘



=
=
others     1
)2/ ,2/(),(    0
),(
NMvu
vuH
滤波后图
퐹(0,0)是直流分量
反映了图像平均灰度

School of  Computer Science and Engineering
7. 频率域滤波基础知识
65
低通滤波器（提取低频）和高通滤波器（提取高频）
低频：图像中缓慢变化的灰度成分;    高频：灰度的尖锐过渡
原图
低通
高通
(直流项变为0)
高通+常数
（保留直流项）

School of  Computer Science and Engineering
7. 空域填充对频域滤波影响
66
原图
不补零低通
补零低通
1
(  ,   )
Re    {
[    (  ,  )   (  ,  )]}
g  x  y
al
H  u  v  F  u  v
−
=
作低通滤波

School of  Computer Science and Engineering
7. 空域填充对频域滤波影响
67
未填充时图像的周期扩展
0填充后图像的周期扩展
1
(  ,   )
Re    {
[    (  ,  )   (  ,  )]}
g  x  y
al
H  u  v  F  u  v
−
=
(  ,  )
[   (  ,   )]}
F  u  v
f   x  y
=
需要先对原始图像푓作周期性扩充

School of  Computer Science and Engineering
7. 频率域滤波基础知识
68
1
(  ,   )
Re    {
[    (  ,  )   (  ,  )]}
g  x  y
al
H  u  v  F  u  v
−
=
如何构造：直接频域构造or 空域→频域；注：H与F大小需一致
频域低通퐻
ℎ=퐼퐷퐹푇(퐻)
퐻的空域表示
对ℎ填充0得到ℎ′
퐻’=퐷퐹푇(ℎ)
ℎ’的频域表示
空域不连续性
振铃

School of  Computer Science and Engineering
7. 频率域滤波步骤小结
69

School of  Computer Science and Engineering
7. 频率域滤波步骤小结
70
IDFT、去中心化、
取实部
原图
填充0
频域中心化
傅里叶谱
高斯滤波器
滤波后的谱
裁剪

School of  Computer Science and Engineering
7. 空域和频域滤波间的对应关系
71
“空域滤波器ℎ(푥,푦)”和“频域滤波器퐻(푢,푣)”组成傅里叶变换对
这个最基本的联系是由著名的卷积定理建立起来的.
在频域构造一个有意义的滤波器,然后作傅里叶反变换,尽可能在空域建造尽可
能小的滤波模板.
傅里叶变换对
(  ,   )
(  ,  )
h  x  y
H  u  v

在频域进行滤波更为直观,其物理意义（DC、高低频、周期性等）更明确.
在空域用较小的模板进行滤波计算则比较经济.
如果两个滤波器的大小一样,则在频率域进行滤波计算比较方便.

School of  Computer Science and Engineering
7. 高斯滤波
72
以一维高斯低通为例,设高斯滤波器(频率域)为H(u)=Ae
-u
2
/2
2
,其中:
为高函数的标准差.而相关的空间滤波器则为
2
2   2
2
(  )
2
x
h  x
Ae


−
=
高斯函数有两个重要特性:
1）高斯函数作傅里叶变换或反变换都是实高斯函数;
2）当퐻(푢)有较宽的轮廓(大的值)时, ℎ(푥)有较窄的轮廓, 反之亦然.
22   222   2
12
22
12
(  )22
xx
h  xAeBe
  

−−
=−
空域高斯高通滤波：
2
2
2
2
12
/ 2
/ 2
()
uu
H  u
Ae
Be

−−
=−
频域高斯高通滤波：
其中:
12
,AB
•频域滤波器越窄，引起的模糊越大。
•当接近无限宽时, 퐻(푢)趋于常数量, ℎ(푥)则趋于冲激函数【即不作滤波】。
两高斯低通滤波作差（宽-窄）。
常量-高斯（极端，3.7节）

School of  Computer Science and Engineering
73
7. 高斯滤波
频域
空域
低通高通

School of  Computer Science and Engineering
74
7. Sobel 滤波
原图
原图频谱
空域Sobel频域Sobel
空域滤波效果频域滤波效果

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
75

School of  Computer Science and Engineering
8. 理想低通滤波器
理想低通滤波器(Ideal LowPassFilters，ILPF)
0
0
1                (  ,  )
(  ,  )
0               (  ,  )
 
D u  v
D
H  u  v
D u  v
D


=



2
1
2
2
2
2
/
]
)
/
(
)
/
[(
)
,
(
Q
v
P
u
v
u
D
−
+
−
=
P、Q 为填充0后的图
像尺寸；且图像做了
中心化处理
D
0
是一个正常数, 称为滤波器的截止频率(Cutoff Frequency). 
三维透视图
二维图像形式
径向剖面
点(u,v)到中心(P/2,Q/2)距离
76

School of  Computer Science and Engineering
8. 理想低通滤波器
截止频率位置D
0
决定了低通滤波器通过的频率成分所包含的功率, 
以及它们在图像总功率值P
T
中所占的比例.




−
=
−
=
−
=
−
=
=
=
1
0
1
0
2
1
0
1
0
)
,
(
)
,
(
P
u
Q
v
P
u
Q
v
T
v
u
F
v
u
P
P
100
(  ,  ) /
T
uv
P u  v
P


=



注意:
1）理想的低通滤波器无法通过电子元件实现;
2）通过计算机模拟会出现模糊与振铃现象.
푤ℎ푒푟푒퐷(푢,푣)≤퐷
0
总功率：
在D(u,v)内
的功率占比：
77

School of  Computer Science and Engineering
8. 理想低通滤波器
例：图像功率作为距频谱矩形中心距离的函数（由于填充，谱的大小
为图像的4倍）
688 x 688像素图像
频谱图中的圆（半径分别为10、30、60、
160、460，分别包含86.9%、92.8%、
95.1%、97.6%、99.4%能量）
DC+少量低频占据了大比例的频谱能量
78

School of  Computer Science and Engineering
8. 理想低通滤波器
ILPFs with cutoff frequencies set at radii values 10, 30, 60, 160, and 460
79

School of  Computer Science and Engineering
8. 理想低通滤波器
理想低通滤波器的模糊和振铃现象解释：
——滤波函数h(x, y) 和其傅里叶变换H(u, v) 之间的倒数关系(宽窄互置)；
——h(x, y)的中心成分大小和截至频率成反比, 决定了模糊的程度.距原点中
心每单位距离的同心圆的多少和截至频率亦成反比, 决定了振铃的程度.
퐻(푢,푣):半径15
ℎ(푥,푦)
过图像中心剖面
80

School of  Computer Science and Engineering
8. 高斯低通滤波器
高斯低通滤波器(Gaussian LowPassFilter，GLPF)
H(u, v) = e 
-D
2
(u, v)/2D
0
2
其中퐷(푢,푣)和퐷
0
定义如前面类似。
GLPF不会产生振铃效应：
——高斯函数的（反）傅里叶变换仍然为高斯函数
81

School of  Computer Science and Engineering
8. 高斯低通滤波器
GLPFs with cutoff frequencies set at radii values 10, 30, 60, 160, and 460
82

School of  Computer Science and Engineering
8. 巴特沃斯低通滤波器
巴特沃斯(Butterworth)低通滤波器


2
0
1
(  ,  )
1
(  ,  ) /
n
H  u  v
D u  v
D
=
+
当n较高，BLPF逼近ILPF，但其平滑性质使得模糊和振铃较小；
当n较低，逼近GLPF，但模糊程度稍高。
n阶巴特沃思低通滤波器(BLPF)的传递函数定义为
83

School of  Computer Science and Engineering
8. 巴特沃斯低通滤波器
一阶BLPF没有振铃, 二阶、三阶的BLPF振铃很小, 随着阶数的增高振铃会逐渐增大. 
二阶、三阶的BLPF可以较好地在有效的低通滤波和可接受的空间域振铃效应之间折中。
1阶
2阶
5阶
20阶
84

School of  Computer Science and Engineering
8. 巴特沃斯低通滤波器
BLPFs with cutoff frequencies at the radii values 10, 30, 60, 160, and 460 & n=2.25
85

School of  Computer Science and Engineering
8. 低通滤波的其他例子
低分辨率文本字符修复(采用高斯低通滤波器)
86

School of  Computer Science and Engineering
8. 低通滤波的其他例子
面部柔和（采用高斯低通滤波）
87

School of  Computer Science and Engineering
8. 低通滤波的其他例子
去除传感器扫描线（采用高斯低通滤波）
88

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
89

School of  Computer Science and Engineering
9. 频率域锐化滤波器
对低通滤波进行相反操作即可
得到高通滤波器(如3.7节)
(  ,  )    1
(  ,  )
HP
LP
H
u  v
H
u  v
=−
90

School of  Computer Science and Engineering
9. 频率域锐化滤波器
91
(  ,  )    1
(  ,  )
HP
LP
H
u  v
H
u  v
=−
频域高通滤波器
11
(  ,   )
(
(  ,  ))
(1
(  ,  ))
HP
HP
LP
h
x  y
H
u  v
H
u  v
−−
= 
=    −
(  ,   )
(  ,   )
LP
x  y
h
x  y

=−
1
(  ,   )
LP
h
x  y
−
空域高通滤波器
11
(1)((  ,  ))
LP
Hu  v
−−
=    − 

School of  Computer Science and Engineering
9. 频率域锐化滤波器
92
퐷
0
=60
퐷
0
=160
IHPFGHPFBHPF,n=2

School of  Computer Science and Engineering
空域连续情形P119：
2
2
2
2
2
y
f
x
f
f


+


=

9. 频率域中的拉普拉斯
)
(
)
(
)
(
u
F
ju
dx
x
f
d
n
n
n

2

)
,
(
)
(
4
)
,
(
)
,
(
2
v
u
F
v
u
y
y
x
f
x
y
x
f
2
2
2
2
2
2
+
−



+



傅里叶变换
的微分性质
2
2
2
(  ,  )
4
(
)
H  u  v
u
v

= −
+
拉普拉斯
传递函数
2
2
2
2
2
(  ,  )
4
[(
/ 2)
(
/ 2)  ]
4
(  ,  )
H  u  v
u    P
v   Q
D   u  v

= −
−
+  −
= −
中心化处理
93

School of  Computer Science and Engineering
9. 频率域中的拉普拉斯
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
f
c
y
x
f
y
x
g
2

+
=
基于锐化滤波的图像增强（空域）：










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
v
F
v
D
v
F
v
H
v
F
v
H
v
F
y
x
g











2
2
1
1
1
4
1
1
+
=
−
=
−
=
−
−
−
基于锐化滤波的图像增强（频域）：
22
[   (  ,   )]
[   (  ,   )
(  ,   )]
[   (  ,   )]
[
(  ,   )]
g  x  y
f   x  y
c
f   x  y
f   x  y
c
f   x  y




=
+ 
=
+   
(  ,  )
(  ,  )   (  ,  )
F  u  v
cH  u  v  F  u  v
=+
当푐=−1
94

School of  Computer Science and Engineering
9. 频率域中的拉普拉斯
95

School of  Computer Science and Engineering
9. 钝化掩蔽、高提升滤波和高频强调滤波
96
钝化遮蔽（从原图像中减去一副钝化图像）步骤：
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
f
y
x
f
y
x
g
m ask
−
=
)
,
(
y
x
f
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
kg
y
x
f
y
x
g
m ask
+
=
1)获得模糊原图像
2）构造模板= 从原图像减去模糊后的图像
3）将模板与原图像相加
k=1  为钝化掩蔽；
k>1 为高提升滤波；
k<1 则不强调钝化模板的贡献。

School of  Computer Science and Engineering
9. 钝化掩蔽、高提升滤波和高频强调滤波


1
(  ,   )
(  ,   )
[
(   ,  )   (   ,  )]
LP
f   x  y
k   f   x  y
H
v  F
v



−
=
+
−
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
k
y
x
f
y
x
g
m ask

+
=




)
,
(
)]
,
(
[
v
F
v
H
k
LP



−

+
=
−
1
1
1


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
f
y
x
f
k
y
x
f
LP
−
+
=


)]
,
(
)
,
(
[
)]
,
(
[
)]
,
(
[
v
F
v
H
v
F
k
v
F
LP







1
1
1
−
−
−
−
+
=
用低通获得模糊图像
利用傅里叶变换/反变换的线性特点
用高通滤波代替


1
1(   ,  )(   ,  )
HP
k  Hv   Fv
−
=+ 
97

School of  Computer Science and Engineering
9. 钝化掩蔽、高提升滤波和高频强调滤波


),(),(),(vFvHkyxg
HP
+=
−
1
1
高频强调滤
波器




)
,
(
)
,
(
)
,
(
v
F
v
H
k
k
y
x
g
HP




+
=
−
2
1
1
更通用的高频强调滤波：
퐾
1
原始图像成分的贡献
푘
2
控制图像高频的贡献
原始图像信号成分
原始图像的高频信号
98

School of  Computer Science and Engineering
9. 钝化掩蔽、高提升滤波和高频强调滤波
原图
GHPF
GHPF高频强调滤波
GHPF高频强调滤波+直方图增强
99

School of  Computer Science and Engineering
9. 同态滤波处理过程
100
基于照射-反射模型，图像可以表示为：
ƒ(푥,푦)=푖(푥,푦)푟(푥,푦)
入射分量푖(푥,푦)：入射到观察场景的光源总量和；0≤푖푥,푦<+∞，
反射分量푟(푥,푦)：场景中物体反射光的总量；0≤푟(푥,푦)≤1
)
,
(
ln
)
,
(
ln
)
,
(
ln
)
,
(
y
x
r
y
x
i
y
x
f
y
x
z
+
=
=
1）取对数
2）傅里叶变换
(  ,  )
[  (  ,   )]
[ln  (  ,   )]
[ln  (  ,   )]
Z  u  v
z  x  y
i  x  y
r  x  y



=
=
+
(  ,  )
(  ,  )
ir
F  u  v
F  u  v
=+
3）频谱滤波
(  ,  )
(  ,  )[   (  ,  )
(  ,  )]
ir
S  u  v
H  u  v   F  u  v
F  u  v
=+
4）傅里叶逆变换
1
1
1
(  ,   )
[   (  ,  )]
[
]
[
]
'(  ,   )
'(  ,   )
ir
s  x  y
S  u  v
HF
HF
i   x  y
r   x  y



−
−
−
=
=
+
=
+
5）取指数
'(   ,   )'(   ,   )'(   ,   )'(   ,   )
(  ,   )
i   x  yr   x  yi   x  yr   x  y
g  x  yeee
+
==

School of  Computer Science and Engineering
9. 同态滤波分析
(  ,  )
[  (  ,   )]
[ln  (  ,   )]
[ln  (  ,   )]
Z  u  v
z  x  y
i  x  y
r  x  y



=
=
+
(  ,   )
(  ,   )  (  ,   )
f   x  y
i  x  y  r  x  y
=
照射分量푖(푥,푦)通常由慢空间变化表征，近似关联到푙푛푓(푥,푦)的低频；
反射分量푟(푥,푦)通常倾向于突变，近似关联到푙푛푓(푥,푦)的高频。
关键：构造同态滤波器퐻(푢,푣)调整푍(푢,푣)的频谱，以达到调整照射分量和反射分量目的。如
22
0
(   ,  )/
(  ,  )    (
)[1
]
cD    u v    D
H
L
L
H  u  v
e
   

−
=   −
−
+
其中，훾
퐻
>1,表示高频成分的增益因子
훾
퐿
<1,表示低频成分的增益因子
高斯型高通滤波
该滤波器衰减图像的低频成分（光照分量），
增强高频成分（反射分量）
1
衰减
增强
101

School of  Computer Science and Engineering
9. 同态滤波效果
102

School of  Computer Science and Engineering
9. 同态滤波效果
103

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
104

School of  Computer Science and Engineering
10. 带阻滤波器
105

School of  Computer Science and Engineering
10. 陷波滤波器
去除指定区域频率
带有摩尔纹图像
频谱图滤波后效果
106

School of  Computer Science and Engineering
1.背景
2.基本概念
3.取样和取样函数的傅里叶变换
4.单变量的离散傅里叶变换
5.二变量函数的傅里叶变换
6.二维DFT和IDFT的一些性质
7.频率域滤波基础
8.使用低通频率域滤波器平滑图像
9.使用高通滤波器锐化图像
10.选择性滤波
11.快速傅里叶变换
第四章：频率域滤波
107

School of  Computer Science and Engineering
11. 二维DFT的可分离性
可分离性





−
=
−
−
=
−
=
−
−
−
=
−
=
+
−
=
=
=
1
0
2
1
0
1
0
2
2
1
0
1
0
2
1
1
1
M
x
M
ux
j
M
x
N
y
N
vy
j
M
ux
j
M
x
N
y
N
vy
M
ux
j
e
v
x
F
MN
e
y
x
f
e
MN
e
y
x
f
MN
v
u
F
/
/
/
)
/
/
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




交换顺序计算：可逐列或逐行先计算一维的傅里叶变换, 然后对另
外一个方向进行计算。
反变换计算可类似处理.
108
给定x,对푓(푥,푦)作一维DFT
给定v,对푓(푥,v)作一维DFT

School of  Computer Science and Engineering
11. 使用DFT算法计算IDFT
逆变换等式两边取复共轭, 有
 
)
,
(
)
,
(
)
/
/
(


−
=
−
=
+
−
=
1
0
1
0
2
M
x
N
y
N
vy
M
ux
j
e
y
x
f
v
u
F

     ),(),(
)//(

+
=
=
M
u
NvyMuxj
N
v
evuF
MN
yxf
2
0
1
11
*
*
2   (
/
/    )
00
1
(  ,   )
(  ,  )
     
MN
j
ux  M   vy  N
xy
f
x  y
F   u  v e
MN

−−
−+
==
=

傅里叶正变换和逆变换如下：
)
,
(
)
,
(
*
y
x
f
y
x
f
=
对实函数（虚部为0）：
109
F*(u,v)的正DFT
11
*2   (//    )
00
1
(  ,   )(  ,  )     
MN
jux  M   vy  N
xy
f   x  yF   u  v e
MN

−−
−+
==
=


School of  Computer Science and Engineering
11. 快速傅里叶变换（FFT）
110
直接计算上述傅里叶变换（逆变换）时间复杂度：O（MN）
2
 
)
,
(
)
,
(
)
/
/
(


−
=
−
=
+
−
=
1
0
1
0
2
M
x
N
y
N
vy
M
ux
j
e
y
x
f
v
u
F

     
)
,
(
)
,
(
)
/
/
(


+
=
=
M
u
N
vy
M
ux
j
N
v
e
v
u
F
MN
y
x
f

2
0
1
假设一副2048×2048大小的中等大小图像，计算量在：17万亿级
而快速傅里叶变换（FFT）可在푂푀푁푙표푔
2
푀푁完成，即约9200万次
以一维傅里叶变换(二维可分解为2个1维计算)为例，介绍一种快
速的方法：逐次加倍法（Successive doubling method）。

School of  Computer Science and Engineering
11.逐次加倍法
111
基本思想：利用傅里叶变换基底性质，将푀个数据的傅里叶变换转为
2组푀/2个数据的傅里叶变换，此时计算量从M
2
降低为M
2
/2

−
=

−
=
−
−===
1
0
1
0
2
110
M
x
ux
M
M
x
Muxj
MuWxfexfuF,,,,)()()(
/


Mj
M
eW
/2−
=
其中
2/
()
ux
ux
j
ux  M
MM
W
W
e

−
==
考虑푀具有2的幂次方푀=2
푛
的形式, 푛为正整数. 故푀可以表示为2퐾, 
퐾=푀/2也是正整数. 从而


−
=
−
=
+
−
=
++=
=
1
0
1
0
12
2
2
2
12
0
2
122
K
x
K
x
xu
K
xu
K
K
x
ux
K
WxfWxf
WxfuF
)()(
)()(         
 )()(

School of  Computer Science and Engineering
11.逐次加倍法
112
注W
M
=e
-j2/M
，W
2K
2ux
=W
k
ux
，因此


−
=
−
=
+
+
=
1
0
1
0
2
1
2
2
K
x
K
x
u
K
ux
K
ux
K
W
W
x
f
W
x
f
u
F
)
(
)
(
)
(
 
)
(
)
(

−
=
=
1
0
2
K
x
ux
K
even
W
x
f
u
F
 )()(

−
=
+=
1
0
12
K
x
ux
Kodd
WxfuF
对풖=ퟎ,ퟏ,...,푲−ퟏ, 定义
u
Koddeven
WuFuFuF
2
)( )()(+=
11
( 2   )
( 2
1)
22
00
(  )
 
(2  )
(2
1)
KK
u
x
u
x
KK
xx
F  u
f
x W
f
x
W
−−
+
==
=
+
+

偶数部分
奇数部分

School of  Computer Science and Engineering
11.逐次加倍法
u
K
odd
even
W
u
F
u
F
u
F
2
)
(
 
)
(
)
(
+
=
W
K
u+K
= W
K
u
和W
2K
u+K
= -W
2K
u
.                  
W
K
=e
-j2/K
,故对任意的正整数K,有
W
K
u+K
=W
K
u
W
K
K
=W
K
u
e
-j2
=W
K
u
W
2K
u+K
=W
2K
u
W
2K
K 
=W
2K
u
e
-j
=-W
K
u
u
K
odd
even
W
u
F
u
F
K
u
F
2
)
(
 
)
(
)
(
−
=
+
因此
113
从而可计算出F（u），풖=푲,푲+ퟏ,...,ퟐ푲−ퟏ的值
풖=ퟎ,ퟏ,...,푲−ퟏ
 
)
(
)
(

−
=
=
1
0
2
K
x
ux
K
even
W
x
f
u
F
 
)
(
)
(

−
=
+
=
1
0
1
2
K
x
ux
K
odd
W
x
f
u
F
(  )
(
)
even
even
F
u
F
u    K
=+
(  )()
oddodd
FuFu    K=+

School of  Computer Science and Engineering
11.逐次加倍法
M个点作傅里叶变换可以把原始表达式分成前、后两部分来计算.
u
K
odd
even
W
u
F
u
F
u
F
2
)
(
 
)
(
)
(
+
=
u
K
odd
even
W
u
F
u
F
K
u
F
2
)
(
 
)
(
)
(
−
=
+
114
仅需要计算前一半M/2个点上的变换值, 后一半的结果可以由前一半的推导得到，
不需要另外的变换运算.
如此循环推导下去, 直到最后剩下若干组两个点对. 用归纳法可以证明,逐次加倍
法的计算复杂性是푀푙표푔
2
푀（原始DFT的计算复杂度为푀
2
）

School of  Computer Science and Engineering
115
作业：
4.3、4.9、4.15、4.17、4.43、4.47、4.57

School of  Computer Science and Engineering
116
•编程#1：实现二维快速傅里叶变换及逆变换，假设图像长宽均为尺寸为
M=2
P
，P为正整数
•编程#2：结合编程#1，实现4.7.3节的处理过程：对下图（512×512）
进行高斯低通滤波处理，要求显示如图4.35的处理结果。其中，高斯低通
频谱图中能量占总能量约95%。
