# ch08 Counters

> 主题库：数字电路基础　｜　源文件：ch08 Counters.pdf　｜　9,521 字符　｜　60 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Counters
8

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
异步2位二进制计数器
忽略了传输延迟

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
异步2位二进制计数器的两种设计

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
异步3位二进制计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
异步触发可能导致多级延迟（最长延迟必须小于
时钟周期）、错误的暂态

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：异步4位二进制计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
异步十进制计数器（异步模10计数器）
•
模：计数序列的状态数
•
4个触发器最多有16个状态，只用了其中10个，称为截断（截尾）序列
部分译码
异步清零
导致毛刺

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：异步模12计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74LS93：异步4位二进制计数器
1位计数器3位计数器
两个信号同
为1异步清零

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：74HC93构成模16和模10计数器
（CTR DIV n代表n个状态的计数器）
“1010”的
部分译码
如何设计模
12计数器？

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步2位二进制计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步3位二进制计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步4位二进制计数器
信号重用

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步BCD十进制计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
原理：设计从现态到次态的翻转逻辑来控制各个触发器的
状态转换
•
Q
1
翻转条件：(Q
0
=1)(Q
3
=0)，J
1
=K
1
=Q
0
Q
3
•
Q
2
翻转条件：(Q
0
=1)(Q
1
=1)，J
2
=K
2
=Q
0
Q
1
•
Q
3
翻转条件：(Q
0
=1)(Q
1
=1)(Q
2
=1)+(Q
0
=1)(Q
3
=1)， J
3
=K
3
=Q
0
Q
1
Q
2
+Q
0
Q
3

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC163：同步4位二进制计数器
•
同步置数，同步清零
•
计数使能条件：ENT和
ENP同为1
•
末端计数TC=15时进位
输出RCO=1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
级联两片74HC163构成同步8位二进制计数器
低4位高4位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
双向计数器（可逆计数器）
计数方向可在计数序列的任一状态改变，可通过控制输
入设为加法或减法计数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步3位双向计数器
UP/DOWN：1—加法计数，0—减法计数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步4位双向计数器时序图
假设：上升沿触发，初态为0000

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC190：同步十进制双向计数器
•
计数：同步
•
使能、置数：异步
•
MAX/MIN=1，RCO=0：
加法计数状态=9，减
法计数状态=0

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
时序逻辑电路的一般模型：状态机
•
输入：(I
0
,I
1
,...,I
m
)；输出：(O
0
,O
1
,...,O
m
)；激励：
(Y
0
,Y
1
,...,Y
p
)；状态：(Q
0
,Q
1
,...,Q
x
)
•
Moore型：输出仅与当前状态相关，O=F(Q)
•
Mealy型：输出与当前状态和输入相关，O=F(I,Q)
•
同步：所有触发器用同一个时钟信号,状态变化同时发生
•
异步：触发器没有统一的时钟信号,状态变化时间有先后

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
3位格雷码计数器的状态图及次态表
现态
次态
转换

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
J-K触发器的转换表
（根据Q
N
和Q
N+1
设置J和K的输入值，设计J和K的逻辑函数）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
根据次态表和触发器转换表作卡诺图来化简触发
器的输入逻辑表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
时序逻辑电路设计步骤
1.准备：画状态图，作次态表、触发器的转换表。
2.化简：作卡诺图，求触发器的输入逻辑表达式。
3.设计：作电路逻辑图。

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用J-K触发器设计任意同步计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用D触发器设计任意同步计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用J-K触发器设计同步3位双向格雷码计数器

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

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
两个级联的异步计数器
目的：级联多个计数器来扩大计数器的模

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
级联的同步计数器
级联信号：计数使能CTEN，末端计数TC
低位片的TC作为高位
片的计数使能信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
多个级联的同步计数器构成递减计数链
•
计数器的模等于各级计数器的模的乘积
•
从低位到高位的TC信号频率等于时钟频率依次除以各级计
数器的模

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
计数器a的模：
计数器b的模：

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：两片74HC190构成模100加法计数器
D/U=0：加法计数；MAX/MIN=9：下一脉冲高位片计数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用4片74HC161设计模为40000的计数器
•
思路：先把4片74HC161级联为一个模65536的计数器，再
改造为模40000的计数器
•
65536-40000=25536（63C0
16
），计数序列：25536
~
65535
•
计数到达65535，最高位片RCO=1，计数器状态设为25536

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
计数器状态的译码
目的：检测计数器的特定状态来输出有效信号，例子：TC
检测110输
出高电平

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：计数器状态2和7的译码电路和时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
计数状态译码的毛刺
•
毛刺：与稳态相反的错误
暂态，持续时间很短（例
如几纳秒）
异步计数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
以选通脉冲来消除译码毛刺
原理：译码器的输入稳定后再译码，假设脉冲高电平期间
足够计数器状态稳定

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
显示“时：分：秒”的数字钟系统

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
同步模60计数器，用于分钟和秒的计数：00
~
59
十位个位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
模12计数器，用于小时的计数
计数序列：(00,)01,02,...,09,10,11,12
小时JK，Q
0000，0
0100，0
0200，0
......
0910，0
1000，1
1100，1
1201，1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
停车场出入控制系统
功能：对出入车辆计数，100个车位满后禁止进入
100时输出1，需要
额外电路

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
并行—串行数据转换系统
原理：以计数器生成0-7的计
数序列作为数据选择器的地
址输入，把一个字节的8位数
据依次选中输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC163同步4位计数器的关联标注逻辑符号
•
共用控制块：上方带凹槽的方框
•
独立元件：下方的四个方框
•
限制符号：CTR DIV 16，表达16个状态（DIV 16）的计数
器（CTR）
•
关联符号：C：控制，M：模式，G：与

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
