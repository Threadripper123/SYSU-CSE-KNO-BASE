# ch09 Shift Registers

> 主题库：数字电路基础　｜　源文件：ch09 Shift Registers.pdf　｜　5,293 字符　｜　33 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Shift Registers
9

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
寄存器：通常由触发器构成
•
两个基本功能：数据存储和移动
•
参数：容量（阶段），移动方式（左右、串并）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位移位寄存器示例：
左/右移位方向和串/并输入输出方式的不同组合

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
串入/串出移位寄存器电路和逻辑符号
8位移位寄存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
串入/串出移位寄存器的移位过程

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
5位串行输入/串行输出移位寄存器波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
串入/并出移位寄存器电路和逻辑符号
初始状态：1111

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
8位串入/并出移位寄存器74HC164逻辑符号及波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位并入/串出移位寄存器电路、逻辑符号和
波形图
初始状态：1010
同步置数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
8位并入/串出移位寄存器74HC165逻辑符号及波形图
异步置数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位并入/并出（并行存取）寄存器电路逻辑图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位并行存取移位寄存器74HC195的逻辑符号和波形图
J和K增加应用灵活
度，通常连在一起
作为数据输入端

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位双向串入并出移位寄存器电路及波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位双向通用移位寄存器74HC194的逻辑符号和波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
n位扭环计数器的电路逻辑图，模数=2n

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位扭环计数器的计数序列及波形图
注意：初始状态为全0

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
5位扭环计数器的计数序列及波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
10位环形计数器的电路逻辑图，模式=位数
•
模数是相同位数的扭环计数器的一半
•
如果Q
0
-Q
9
只有一个为1，其余为0，状态由1所在的位置
决定，无需译码
•
初始状态可以根据要求来设置

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
10位环形计数器的计数序列及波形图
初始状态：
1010000000
初始状态：
1000000000

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
移位寄存器应用示例：延时电路
原理：脉冲输入寄存器，每拍移动一位，经过n拍后输出，n位
寄存器延迟n拍

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
延时电路示例：8位共延迟8拍，每拍1/500k=2us，延
时2*8=16us

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
移位寄存器应用示例：以74HC195构成环形计数器
方法：Q
3
作为输入，Q
0
~
Q
3
初始状态为：1000

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
移位寄存器应用示例：数据的串行-并行转换电路
数据的串行输入格式：0开始，8位数据，1结束

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
数据的串行-并行转换波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
通用异步收发器（UART）：数据的串-并、并-串转换
长距离的数据传输
通常采用串行方式
用移位寄存器实
现并-串转换

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
移位寄存器应用示例：键盘编码电路
•
键按下导致第一个单稳
态触发器生成一个脉冲
•
第一个脉冲变低后，CLK
被屏蔽，移位寄存器停
止移位，行、列扫描停
止，行、列编码器的输
入停止变化，同时第二
个单稳态触发器生成一
个脉冲
•
第二个脉冲把行、列编
码器的输出（即按键的
编码）写入寄存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
8位移位寄存器74HC164的关联标注逻辑符号
•
共用控制块：上方带凹槽的方框
•
独立元件：下方的8个方框
•
限制符号：SRG 8，表达8位移位寄存器
•
关联符号：C：控制，R：清零（Reset），箭号：移位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位双向移位寄存器74HC194的关联标注逻辑符号
•
共用控制块：上方带凹槽的方框
•
独立元件：下方的4个方框
•
限制符号：SRG 4，表达4位移位寄存器
•
关联符号：C：控制，M：模式，R：清零（Reset），箭号
：移位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
