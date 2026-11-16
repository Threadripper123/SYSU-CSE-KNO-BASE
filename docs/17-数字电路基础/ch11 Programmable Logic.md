# ch11 Programmable Logic

> 主题库：数字电路基础　｜　源文件：ch11 Programmable Logic.pdf　｜　7,917 字符　｜　43 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Programmable 
Logic
11

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Outline and Key Terms

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
可编程逻辑：SPLD和CPLD
•
SPLD：PAL（一次编程阵列逻辑）和GAL（可重复编程
的通用阵列逻辑），组成：可编程的与阵列+固定的或
阵列，实现SOP表达式
•
CPLD：包含多个可互联的SPLD模块
PAL的基本“与-或”结构图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用PAL实现SOP表达式
熔丝保留来连接相
应输入到与门，否
则烧断，即编程
或门的输入固
定，不可编程

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
GAL：用可重复编程的EEPROM器件取代PAL的熔丝
每个单元的浮栅可编程为有无充负电荷，如果无充电则与地短路，否则与
地断路
•
假设该单元无充电，该行上的其它三
个单元充电
•
则A=0时单元导通接地，与门输入0，
否则与门输入1，即与门输入为A
注意：此图的单元
结构只是示意图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
PAL/GAL的简化结构图
主要部件：与门，或门，输入缓冲器，连接阵列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：PAL实现SOP逻辑表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
PAL/GAL的模块图
Macrocell（宏单元）：由或门及其输出逻辑组成，可配
置为组合或寄存逻辑（含有一个触发器），用于实现组
合或时序电路功能

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
组合逻辑的PAL/GAL的三种宏单元基本类型
•
组合输出：以一个三态门来控制的输出
•
组合输入/输出：在组合输出的基础上增加一个三态门来控制的输入/
输出
•
可编程极性的输出：在组合输入/输出的基础上增加一个异或门来控制
输出的有效电平

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
通用CPLD的基本模块图
主要部件：多个属于SPLD的逻辑阵列模块（LAB），可编
程互联阵列
•
多种CPLD具有不同的参
数：密度，器件技术，
功耗，电压，速度
•
密度：宏单元数，LAB数
•
重复编程器件技术：
EEPROM，SRAM
•
功耗：几到几百mw
•
电压：1.8-5V
•
代表厂家：Altera，
Xilinx等

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
CPLD的主要厂家和参数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Altera Max 7000系列CPLD的基本模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Altera MAX 7000系列CPLD的简化结构图
共享扩展项：每个
宏单元一个，用于
增加乘积项

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用宏单元的共享扩展项来增加乘积项

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
每个x代表已编程
的连接点

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
并行扩展项：把宏单元的输出作为其它宏单元的
或门输入

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
MAX II CPLD的简化模块图
•
LAB包含多个相当于宏单元
的LE（逻辑部件）
•
LAB之间由互联阵列连接
•
IOE（IO部件）提供输入/
输出功能
•
与FPGA架构相似，相当于
低密度的FPGA

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
MAX II CPLD与传统CPLD的区别：
•
实现SOP的方式:前者采用LUT（查找表）逻辑，后者采用基于SPLD
的“与-或”阵列逻辑,LUT由可编程的SRAM存储器构成，原理不同
于“与-或”阵列
•
LAB的互联方式：前者采用行列矩阵，后者采用相对简单的通道

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
PAL（可编程阵列逻辑）与PLA（可编程逻辑阵列）
•
PAL：可编程的与阵列 + 固定的或阵列
•
PLA：可编程的与阵列 + 可编程的或阵列

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用PLA实现SOP逻辑表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
数据选择器的通用符号
（根据编号输入来在输入数据中选择一个输出）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Altera MAX 7000 CPLD的宏单元模块图
•
用四个数据选择器来控制触发器的信号输入
•
用一个数据选择器来控制实现组合或寄存逻辑
•
用一个异或门来控制实现SOP或POS表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
实现SOP逻辑函数的CPLD宏单元配置
（数据通路用红色标出）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
实现寄存逻辑函数的CPLD宏单元配置
（数据通路用红色标出）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
现场可编程逻辑门阵列（FPGA）
•
当前常用的大规模可编程器件，结构上区别于PAL/PLA，密
度远大于CPLD
•
三个基本模块：可配置逻辑模块（CLB），互联模块，输入
/输出模块
•
CLB比CPLD中的LAB或FB简单，但是数量大很多，大规模
FPGA的CLB高达万级以上
•
根据CLB的复杂程度分为细粒度（简单）和粗粒度（复杂）
•
产品系列的不同型号在主要参数上有差异，满足不同应用
的需求，比如：密度、能耗、电压、速度等

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FPGA的基本结构
CLB内部有可编程的局部连接；CLB和I/O模块之间有可编程的
全局行、列连接

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
CLB的基本结构
每个CLB内部包含多个更小的逻辑模块，以局部可编程连接来
互联

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
CLB中的逻辑模块的基本模块图
•
包括两个主要部件：查找表（LUT）和关联逻辑（Associated 
logic）
•
逻辑模块可配置为组合逻辑、寄存逻辑或两者的结合
•
关联逻辑有一个触发器，用于实现寄存逻辑
•
查找表可编程实现SOP表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
查找表实现SOP表达式的基本原理
•
n个输入变量有2^n个最小项，每个最小项用一位存储单元来
编程控制是否有效
•
选择逻辑（Selection logic）相当于译码器，存储单元相当
于和最小项相与的开关变量

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
逻辑模块的运算模式
•
可编程实现四种运算模式：普通，扩展，算术，共享算术
•
普通模式：主要用于实现组合逻辑函数
•
扩展模式：可扩展变量数，实现多达7变量的函数
具有两个查找表的逻辑模
块普通模式的几种配置
•
输入可共享
•
单个LUT的输入最多为6个

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
实现7变量SOP函数的逻辑模块扩展模式示例

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
易失FPGA配置的基本概念
•
编程的存储单元采用SRAM，断电后编程数据消失
•
由芯片从内嵌的非易失存储在加电后加载编程数据，或由主处理器从
外部存储器加载编程数据

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
在FPGA中嵌入内核功能的基本概念
•
硬核功能由厂家在芯片生产时嵌入，不能改写，为常用功能，比如：
微处理器，标准输入/输出接口，数字信号处理器等
•
如果嵌入功能允许编程，则称为软核功能
•
内核功能可节省应用开发成本和时间，优化性能，减少开发错误
•
IP（知识产权）核由厂家拥有，可根据需求调整具体参数
•
平台FPGA包含各种嵌入处理器和内核，可独立实现一个完整的系统，
无需附加外部设备

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
包含嵌入内存和DSP的FPGA模块图示例

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FPGA产品的代表厂家和参数
FPGA产品形成系列，具有多种技术规格，满足丰富应用需求

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SPLD、CPLD和FPGA编程的通用流程
•
被编程的设备称为目标设备，编程过程称为设计流程，由
CAD（计算机辅助设计）软件来提供支持
•
完整流程：设计输入，功能仿真，综合，实现，时序仿真，
设备编程（下载）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SPLD、CPLD和FPGA编程的关键部件
包括：计算机，开发软件，编程逻辑设备，编程硬件（开发
板，连线等
）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
