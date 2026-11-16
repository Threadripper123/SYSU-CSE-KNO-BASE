# ch06 Functions of Combinational Logic

> 主题库：数字电路基础　｜　源文件：ch06 Functions of Combinational Logic.pdf　｜　8,282 字符　｜　53 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Functions of
Combinational Logic
6

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
半加器：输入无进位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
全加器：输入有进位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用两个半加器来实现全加器
练习：根据输入计算输出（和、进位）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用多个全加器来实现多位并行加法器
练习：根据输入计算输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位并行加法器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位并行加法器：
74HC283/74LS283 

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
多个加法器可以级联起来构成一个更大的加法器
2x74HC283构成8位加法器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
6人投票系统
•
每个全加器的3个
输入对应3个人的
投票
•
用两个全加器来接
受6个人的输入
•
两个全加器的结果
用一个4位加法器
相加得到总票数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
串行进位加法器
（结构简单，速度慢）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
进位输出的构成：本地生成进位，低位传播进位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位加法器的本地生成进位和低位传播进位参数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位超前进位加法器的逻辑设计
（比如74LS283，以串行进位方式级联多个芯片来
构成更大的加法器）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
比较器：对输入的两个二进制数A和B进行比较
•
输出三种信号：A=B，A>B，A<B
•
相等的逻辑：各位相等，每位的相等检测用同或门
•
不等的逻辑：高位相等，本位不等，比如：
Y
A>B
=(A1>B1)+(A1=B1)(A0>B0)

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位比较器74HC85/74LS85的引脚图和逻辑符号
•
有3个级联输入信号，来自于低位芯片的输出
用两个74HC85构成
一个8位比较器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
译码器：对特定二进位组合（编码）进行检测
并输出预设结果（1-高有效，0-低有效）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 6–4   Decoding functions and truth table for a 
4-line-to-16 line (1-of-16) decoder with active-LOW 
outputs.
4线-16线（16选1）译码器的真值表和逻辑符号
（输出低有效，可用NAND和NOT门实现）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
16选1译码器 74HC154的引脚图和逻辑符号
•
输出低有效
•
使能输入信号EN
用于芯片级联

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用两片74HC154构成一个5位译码器
用高位信号
来选片

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
译码器的应用示例：控制多个设备的使能端

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
BCD—十进制译码器：根据输入的BCD编码令
相应编号的输出为有效电平

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
BCD—七段译码器：根据输入的BCD编码令相应段
的输出为有效电平，用于七段显示器的控制
逻辑符号
74HC47的引脚图
和逻辑符号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
BCD—七段译码器的整数和小数灭零设计

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
编码器：根据输入信号来输出对应该信号的编码
（十进制BCD普通编码器的逻辑符号、真值表和逻辑图）
高电平有效
9个输入都为低=0
号输入有效

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
优先编码器：根据输入信号来输出对应最高优先级
信号的编码
10线-4线优先编码器74HC147，HPRI代表高编号信号
优先，NC=No internal connection

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
8线-3线优先编码器74LS148
EO  GS
状态
1  1
不工作
0  1
工作，但无输入
1  0
工作，且有输入
0  0
不可能出现

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
优先编码器应用示例：0-9数字键盘
假设：每次至少一个键被按下

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 6–7   Binary representations of BCD bit 
weights.
BCD—二进制转码器
（原理：用加法器求加权和）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二进制—格雷码转换    格雷码—二进制转换

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
数据选择器：根据输入编号选择相应编号的数据输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4选1数据选择器的波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC157：4个2选1数据选择器
共用控制块

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC151：8选1数据选择器
2x74HC151构成16选1数据选择器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
以多路选择器来复用BCD七段显示芯片的设计示例
交替选中A、B

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用多路选择器实现逻辑函数
(2^n选1实现n变量逻辑函数)

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
2^(n-1)选1实现n变量逻辑函数
原理：每根数据线最多有4种输入：{0，1，A
0
，A
0
’}

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
数据分配器：根据输入编号把数据分配到对应编
号的输出线

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC154译码器作为1线—16线的数据分配器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
74HC280：9位奇偶校验位生成器/检验器
•
生成奇校验位：∑Even；生成偶校验位：∑Odd
•
奇检验：∑Even=1，出错；偶检验：∑Odd=1，出错

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
可检错的数据传输系统
应为
∑Odd

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
3线-8线译码器输出的毛刺
原因：输入信号变化存在时延，多个输入同时变化时导
致短暂的错误输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
输出中有毛刺的原因分析

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
以选通信号来消除毛刺
（原理：在选通信号有效期间无输入变化）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
交通灯信号控制系统状态图
状态
状态转
换条件

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
交通灯信号控制系统模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
组合逻辑模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
