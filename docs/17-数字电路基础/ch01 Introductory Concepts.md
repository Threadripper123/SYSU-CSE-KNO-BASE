# ch01 Introductory Concepts

> 主题库：数字电路基础　｜　源文件：ch01 Introductory Concepts.pdf　｜　13,263 字符　｜　66 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Introductory 
Concepts
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
FIGURE 1-1   Graph of an analog quantity (temperature versus time).
连续
模
拟
信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-2   Sampled-value representation (quantization) of the analog quantity in Figure 1–1. Each value 
represented by a dot can be digitized by representing it as a digital code that consists of a series of 1s and 0s.
离散数字信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-3   A basic audio public address system.
模
拟扩
音器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-4   Basic block diagram of a CD player. Only one channel is shown.
数字
CD
播放机

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-5   Logic level ranges of voltage for a digital circuit.
高、低
电
平区
间
分
别
表达二
进
制的
1
、
0
正
逻辑
：高（
1
）低（
0
）
负逻辑
：高（
0
）低（
1
）
CMOS
：
2-3.3v
，
0-
0.8v

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-6   Ideal pulses.
理想（理
论
）的正、
负
脉冲信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-7   Nonideal pulse characteristics.
非理想（真
实
）的脉冲信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-8   Examples of digital waveforms.
周期方波和非周期信号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-9
周期，
频
率，占空比（
Duty cycle
）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-10   Example of a clock waveform synchronized with a waveform representation of a sequence of bits.
以
时钟
同步的二
进
信号序列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-11   Example of a timing diagram.
二
进
信号序列的
时
序
图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-12   Illustration of serial and parallel transfer of binary data. Only the data lines are shown.
二
进
信号的串行和并行
传输
长
距离，低速
短距离，高速

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-13
8
位数据的
传输时间
，
时钟1MHz
（
1
）串行；（
2
）
4
位
总线
；（
3
）
8
位
总线

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-14
正
逻辑
负逻辑
？

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-15   The basic logic functions and symbols.
非：取反
与：所有
输
入
为
真
时输
出真
或：至少一个
为
真
时输
出真

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-16   The NOT function.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-17   The AND function.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-18   The OR function.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-19   The comparison function.
数
值
比
较
器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-20   The addition function.
加法器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-21   An encoder used to encode a calculator keystroke into a binary code for storage or for 
calculation.
编码
器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-23   A decoder used to convert a special binary code into a 7-segment decimal readout.
解
码
器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-23   Illustration of a basic multiplexing/demultiplexing application.
数据
选择
器（多路
选择
器）
数据分配器（分路器）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-24   Example of the operation of a 4-bit serial shift register. Each block represents one storage “cell” 
or flip-flop.
串行移位寄存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-25   Example of the operation of a 4-bit parallel shift register.
并行移位寄存器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-26   Illustration of basic counter operation.
计
数器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-27   Block diagram of a tablet-bottling system.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-28   Cutaway view of one type of fixed-function IC package (dual in-line package) showing the chip 
mounted inside, with connections to input and output pins.
双列直插封装

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-29   Examples of through-hole and surface-mounted devices. The DIP is larger than the SOIC with 
the same number of leads. This particular DIP is approximately 0.785 in. long, and the SOIC is approximately 
0.385 in. long.
小外形集成电路封装

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-30   Examples of SMT package configurations. Parts (e) and (f) show bottom views.
表面组装技术封装
（a）窄间距小外形 （b）带引线的塑料芯片载体 （c）无引脚陶瓷芯片载体
（d）薄型四方扁平封装   （e）层压芯片级封装（f）细间距球栅阵列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-31   Pin numbering for two examples of standard types of IC packages. Top views are shown.
芯片引脚编号标记

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
类别  规模（等效逻辑门
数量）
器件技术
SSI10
TTL，CMOS
MSI10-100
LSI100-10,000
CMOS，NMOS
（高密度，
低功耗）
VLSI   10,000-100,000
ULSI100,000+
集成电路规模分类
三种器件技术：TTL（双极型晶体管），CMOS（场
效应晶体管，MOSFET），BiCMOS（TTL+CMOS）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-32   Typical digital oscilloscope with voltage probe.
Used with permission from Tektronix, Inc.
数字示波器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-35   Block diagram of a digital oscilloscope.
(Photo courtesy of Digilent, Inc.)
数字示波器模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-36   A typical digital oscilloscope front panel. Numbers below screen indicate the values for each 
division on the vertical (voltage) and horizontal (time) scales and can be varied using the vertical and horizontal 
controls on the scope.
Used with permission from Tektronix, Inc.
数字示波器前面板

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-37   Comparison of an untriggered and a triggered waveform on an oscilloscope.
无触发和有触发的波形显示

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-38   Displays of the same waveform having a dc component.
信号耦合

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-40   Probe compensation conditions.
探头补偿

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-41
波形测试例子：波幅、周期

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-42   Typical logic analyzer.
Used with permission from Tektronix, Inc.
逻辑分析仪

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-43   Simplified block diagram of a logic analyzer.
逻辑分析仪模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-44   Two logic analyzer display modes.
逻辑分析仪的波形和列表显示模式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-45   A typical multichannel logic analyzer probe.
Used with permission from Tektronix, Inc.
逻辑分析仪多频道探头

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-46   Typical signal generators.
Used with permission from Tektronix, Inc.
典型信号发生器
波形和函数发生器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-47   Typical DMMs.
Used with permission from (a) B+K Precision
®
; (b) Fluke
数字万用表

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-48   Typical bench-type dc power supply.
Used with permission from Tektronix, Inc.
直流电源

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
逻辑笔

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-50   Programmable logic hierarchy.
可编程逻辑
PLD：可编程逻辑器件
FPGA：现场可编程门阵列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-51   Block diagrams of simple programmable logic devices (SPLDs).
两类SPLD的模块图
PAL：可编程阵列逻辑，可编程1次
GAL：通用阵列逻辑，可重复编程的PAL

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-52   A typical SPLD package, 24-28 pins.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-53   General block diagram of a CPLD.
逻辑阵列模块  可编程连接阵列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-54   Typical CPLD plastic quad flat packages (PQFP).
塑料方块平面封装的复杂编程逻辑器件

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-55   Basic structure of an FPGA.
FPGA的主要模块：逻辑，可编程连接，输入/输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-56   A typical ball-grid array (BGA) package.
球栅阵列封装的FPGA

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-57   Basic setup for programming a PLD or FPGA. Graphic entry of a logic circuit is shown for 
illustration. Text entry such as VHDL can also be used.
(Photo courtesy of Digilent, Inc.)

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 1-58   Basic programmable logic design flow block diagram.
可编程逻辑设计流程
•
设计
•
功能仿真
•
综合优化
•
实现（布局布线）
•
时序仿真
•
下载

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
