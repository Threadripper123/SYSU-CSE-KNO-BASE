# ch12 Signal Interfacing and Processing

> 主题库：数字电路基础　｜　源文件：ch12 Signal Interfacing and Processing.pdf　｜　6,871 字符　｜　39 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Signal Interfacing
and Processing
12

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Outline and Key Terms

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
数字信号处理系统的基本模块图
•
模数信号转换：低通防失真滤波器，采样和保持电路，ADC
•
数字信号处理：DSP，实现各种数字信号的计算功能
•
数模信号转换：DAC，低通重建滤波器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ADC（模数转换）的基本概念
•
AD转换过程包括模拟信号的采样、保持和量化编码
•
(Nyquist-Shannon）采样定理：f
sample
>2f
a(max),
其中的f
a(max)
称为Nyquist（奈奎斯特）频率

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
采样原理的直观例子：球的运动轨迹
采样频率越高，样本与原信号的误差越小，越能反映信号
的真实情况

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
采样过程中的高频信号过滤
采样过程包含滤波操作，采用低通滤波器来把高于采样频率一
半的信号过滤，以满足采样定理的要求，即f
sample
>2f
a(max)

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
模拟信号的采样保持和量化编码
•
信号采样后需要保持一定时间来转换为数字编码，因此采样信号波形为
（离散的）阶梯状
•
对采样后保持的信号强度进行量化并输出数字编码

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：对采样保持的模拟信号的4级量化
（灰色部分是原始信号）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：根据4级量化值来重建的信号波形

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：对采样保持的模拟信号的16级量化
（灰色部分是原始信号）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：根据16级量化值来重建的信号波形
对给定的信号强度范围，量化级数越多，误差越少

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
运算放大器的基本概念
•
线性放大器，有两个输入和一个输出，特点：电压增益高，
输入阻抗高，输出阻抗低
•
是ADC和DAC的常用器件，作为反相放大器或者电压比较器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
并联比较型（Flash）ADC的原理
•
以多个等值电阻构成分压电路，生成参考电压，以多个电压比较器把输
入电压与所有参考电压同时比较，结果输入优先编码器来输出编码
•
优点：速度高；缺点：n位编码需要2^n个电阻和2^n-1个比较器，硬件
复杂度太高，只适用于小规模的ADC

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：3位并联比较型ADC的波形图，V
REF
=8V

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
双积分型（Dual-Slope）ADC的基本电路图
•
转换过程包含充电和放电两个阶段，电阻R在充电和放电阶段分别与V
in
和V
REF
连通
•
电容C的充电时间固定，充电结束后，C的电压与输入电压成比例
•
放电时长与C的电压成比例，通过计数器来测量放电时间，放电结束后
计数脉冲被屏蔽，计数结果作为V
in
的二进编码输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
双积分型ADC的工作原理：充电
•
电容C充电时的电流I与V
in
成比例
•
计数器的n是一个预设的参数，用于固定计数时间长度

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
双积分型ADC的工作原理：放电
•
V
REF
固定，电容C的放电时间与-V成比例
•
计数器的计数结果是电容C放电时间的编码，通过分辨率可
换算为V
in
的测量结果

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
双积分型ADC的工作原理：输出转换结果

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
逐次渐近型ADC的基本原理
•
快速ADC硬件太复杂，双积分型ADC太慢，逐次渐近型（Successive 
Approximation）ADC的性能和成本处于两者之间，是常用的ADC电路，
转换时间固定，与输入电压无关
•
逐次渐近方法采用二分查找的方式，从编码的高位到低位依次折半生成
参考电压来与输入电压比较，根据比较结果来逐位生成编码

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：4位逐次渐近型ADC的工作过程

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
逐次渐近型ADC芯片：ADC0804
输入：（V
in+
-V
in-
），分辨率：8位，转换时间：100us

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
计数型（Sigma-Delta）ADC基本概念
•
以单位时间内“1”的数量来表示电压大小
•
例如：4096：最大电压，2048：0V，0：最小电压

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
计数型ADC的基本模块图
•
对输入电压逐步比较，1-bit量化器输出包含比较结果的二
进位流
•
计数器对二进位流所包含的“1”计数，转换结束后把计数
结果写入锁存器作为编码输出

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ADC芯片测试方法及三种典型错误

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ADC芯片错误示例
输出缺失编码0011，
对应第三个比较器

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
权电阻网络DAC的原理
•
电阻值从低位到高位依次折半，对应电流形成2进权值，电
阻越大权值越小
•
缺点：电阻值跨度大，难以大幅增加位数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：4位权电阻网络DAC的波形图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
倒T形电阻网络（R/2R Ladder) DAC的原理
只用R和2R两种电阻，克服权电阻网络DAC的缺点

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
4位倒T形电阻网络DAC的等效电路
根据戴维南（Thevenin）定理构造各输入为“1”时的等效电
路，从高位到低位的输入所生成的流经R
f
的电流构成二进权值

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
等效电路参数的计算

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
DAC芯片的测试及典型错误
DAC的典型性能参数
•
分辨率：输出值间隔个数的倒数，n位输出则为1/(2^n-1)
•
精度：实际输出和期望输出的误差，常用最大输出电压的
百分比来衡量
•
线性度：常用实际输出与理想线性输出之间的误差来衡量
•
单调性：从低到高变化过程中，输出单调上升
•
建立时间：从施加输入信号开始到输出稳定在最后值的LSB
的一半以内所需的时间

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
示例：4位DAC芯片的检测
输入0010、0011的输
出错误，对应输入编
码的2^1权值位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
