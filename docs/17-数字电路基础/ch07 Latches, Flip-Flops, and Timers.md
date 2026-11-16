# ch07 Latches, Flip-Flops, and Timers

> 主题库：数字电路基础　｜　源文件：ch07 Latches, Flip-Flops, and Timers.pdf　｜　9,633 字符　｜　63 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Latches, Flip-Flops,
and Timers
7

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
S-R锁存器
S：置位，R:复位，两者不允许同时有效，两者都
无效时保持Q（和Q’）的状态不变
高有效输入低有效输入

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
非法输入，
禁止出现！
S-R锁存器的四种情况：
置位(置一）、复位（清零）、保持和无效

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
S-R锁存器逻辑符号和波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
S-R锁存器的应用示例：开关抖动消除电路
假设：开关抖动幅度很小，不会反复触及两个接触点
原理：输入同为1时Q保持不变

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC279A：4个低输入有效S-R锁存器
（置一信号有效=S1’+S2’）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
电平触发S-R锁存器
（
EN有效时允许Q的状态根据输入改变，否则Q保持）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
电平触发D锁存器
（EN有效时允许Q的状态设为D，否则Q保持）
R=S'
S

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC75：4个电平触发D锁存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
边沿触发的S-R、D和J-K触发器
（S-R触发器是后两者的基础，但是不以芯片形式出现
）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
上升沿触发S-R触发器的操作：置一、清零、保持

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
上升沿触发S-R触发器的逻辑设计
•
用脉冲转换检测器的输出取代电平触发S-R锁存器的EN
•
上升沿

很窄的正脉冲

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
上升沿触发S-R触发器的状态转换过程：置一、清零

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
D触发器的逻辑符号、真值表和波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K（Jack Kilby）触发器的逻辑设计
原理：在S-R触发器上增加两个反馈信号，令{J=1、K=1}
的输入对应状态翻转功能
S
R

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K触发器的功能和真值表

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K触发器的状态转换过程
①置一：J=1，K=0；②清零：J=0，K=1；③翻转：J=1，K=1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K触发器波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
带异步置一和清零输入的J-K触发器
（异步：立即置一或清零，无需与时钟同步）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K触发器设置为T触发器的波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74AHC74：双D触发器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC112：双下降沿触发J-K触发器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
时钟-输出延迟（传输延迟）：输出Q相对于时钟信号的
延迟
置一-输出、清零-输出延迟:输出Q相对于置一或清零信
号的延迟

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
建立时间：数据在时钟信号之前稳定不变的最小时长
保持时间：数据在时钟信号之后稳定不变的最小时长

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
最大时钟频率：触发器可靠工作的最高时钟频率
•
（最小）脉冲宽度：时钟（高、低时长），置一，清零
•
功耗：P=Vcc × Icc
假设单触发器功耗：
10个触发器的总功耗和5V电源的电流强度：

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
四类D触发器的典型参数值
CMOS静态功耗

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
触发器应用示例：寄存器
原理：多个D触发器并行存储多位数据
同步时序

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
触发器应用示例：分频器
原理：多个触发器级联，各个触发器的状态分别作为分频
输出信号，频率依次减半
异步时序

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
触发器应用示例：计数器
原理：多个触发器级联，各个触发器的状态组成计数编码

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
单稳态触发器
•
两个状态：稳态、暂稳态
•
在外界触发脉冲作用后，从稳态翻转到暂稳态并维持一段时间，然后
自动返回稳态，暂稳态的停留时间取决于电路本身参数，与触发脉冲
宽度和幅度无关
•
应用：脉冲整形、延时、定时

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
不可重复触发单稳态触发器工作波形
可重复触发单稳态触发器工作波形

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74121：不可重复触发单稳态触发器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74121脉冲宽度的三种设置方法
•
默认，外接电容，外接电阻和电容
•
默认脉冲宽度：30ns，内部电阻：2kΩ
•
脉冲宽度（R=R
INT
或R
EXT
）： R：kΩ ，C
EXT
：pF，t
w
：ns

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74LS122：可重复触发单稳态触发器
•
默认脉冲宽度：45ns，内部电阻：10kΩ
•
脉冲宽度：

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
t
w
=0.7RC=0.7*39*3.3*1E6=90.09E6 ns=90.09 ms

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
应用示例：用3个74LS122串接构成顺序计时电路，
每个触发脉冲依次在Q1、Q2、Q3输出一个脉冲

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
555定时器是多用途的数字—模拟混合集成电路，
可构成单稳态或多谐振荡器
1/3Vcc
2/3Vcc
1：Q1导通
0：Q1截止

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
555定时器构成单稳态触发器
R
1
：kΩ ，C
1
：pF，t
w
：ns

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
555定时器构成单稳态触发器：初始状态
通常为高，
负脉冲触发
通常为低，
触发后变高

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
555定时器构成单稳态触发器：触发进入暂稳态,C1开始充电

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
555定时器构成单稳态触发器：C1充电到2/3Vcc,回到稳态
教材P392第一行
的1/3Vcc写错

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
以施密特触发器构成多谐振荡器
（UTP：正向阈值电压，LTP：负向阈值电压）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
以555定时器构成
多谐振荡器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
频率计算公式及函数图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
R
1
< R
2 
时占空比小于50%，C
1
充电路径：R
1
、D
1
，
C
1
放电路径：R
2
、Q
1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
多个信号之间的变化延迟可能导致错误暂态

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
修改设计来避免错误暂态

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
实验示例：用74HC00实现电平触发SR锁存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
交通灯信号控制系统模块图
（已实现：组合逻辑，未实现：计时、时序逻辑）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
4s和25s计时器用74121或者555定时器
•
系统时钟用555定时器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
