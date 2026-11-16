# ch10 Memory and Storage

> 主题库：数字电路基础　｜　源文件：ch10 Memory and Storage.pdf　｜　11,151 字符　｜　70 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Memory and Storage
10

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
计算机系统中的存储器层次
触发器
SRAM
DRAM
硬盘
磁带

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
存储器
•
容量单位：位，字节，半字节，字
•
字：位数非固定，根据需要定义，比如2B，4B，8B
•
多种器件技术：RAM（随机存取存储器），ROM（只读存储器），
Flash（闪存），磁盘，光盘等
•
存储单元通常以阵列形式组织，地址可分为行、列地址

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二维阵列的存储器寻址
（每个单元1位）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
三维阵列的存储器寻址
（每个单元多位共用同一个地址）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二维和三维阵列的存储器模块图
（大容量的存储器通常采用三维阵列）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
存储器的写操作过程

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
存储器的读操作过程

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
RAM的类别
•
SRAM（静态RAM）：单元采用锁存器，无需动态刷新
•
DRAM（动态RAM）：单元采用MOS管和电容，以电容的电量高低
来代表1和0，需动态周期刷新来保持数据
•
特点：SRAM速度高、容量低，DRAM速度低、容量高

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SRAM的存储单元结构
写入1：
•
1位线=高，0位线=低，字线=
高，T3，T4导通
•
T3导通，Q=1，T2导通
•
T4导通，Q=0，T5导通
•
写完成后，字线=低，T5、T2
保持导通，Q=1,Q=0
后续再读时
•
字线=高，T3、T4导通，1位
线=高，0位线=低

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SRAM的基本阵列结构
字线：地址译码
器根据访问的单
元地址译码选中

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
32k×8异步SRAM的逻辑图
•
异步：读写操作没有同步时钟信号控制
•
容量：32k个单元，每单元8位，对应地址线15位，
数据线8位
•
倒三角符号表示三态
输出
•
三态输出方便连接到
与其它器件共用的数
据总线

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
32k×8异步SRAM的基本组织
15位地址：8位行地址，7位列地址

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SRAM读、写周期的典型时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
支持突发存取的同步SRAM模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
突发存取的寻址逻辑

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SRAM作为Cache来弥补DRAM和CPU之间的速度差距
原理：利用代码和数据存取的时间和空间局部性，减少
对DRAM的访问

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
DRAM的MOS单元结构
（电容漏电，需要周期刷新来保持状态）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
DRAM单元的基本读写操作

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
地址复用，分
时用于行、列
地址
1M×1的DRAM简化模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
地址复用的时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
普通读周期的时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
普通写周期的时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
快速页面模式读操作的时序图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapters

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ROM的类别
•
掩膜ROM：数据由厂家在生产时设定
•
PROM：数据可由用户通过特定设备写入

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ROM的单元结构
Row为高时导
通，Column输
出高电平

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
16x8b的ROM阵列
（4位地址对应16单元，每单元8位）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ROM应用示例：二进码到格雷码的转换
原理：设置存储数据来实现标准SOP逻辑表达式

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
256x4b的ROM逻辑符号
A 
0
/
255
：代表地址范围为0-255
三态输出：
当选片信号有效时输出
数据，否则为高阻态

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
256x4b的ROM结构模块图
8位地址：5位行地址，3位列地址
4个3线到8线的译
码器，每个对应1
位数据的列寻址

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
ROM的存取时间
t
a
•
定义1：给定选片有效，从地址有效到输出数据之间的延迟
•
定义2：给定地址有效，从选片有效到输出数据之间的延迟

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
带熔丝的MOS PROM阵列
（通过烧断熔丝来代表0，否则代表1）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
2048x8b的UVEPROM的逻辑符号
•
EPROM：可擦除PROM，两大类：UVEPROM（紫外线可擦除
PROM），EEPROM（电可擦除PROM）
•
读模式：CE/PGM=0,OE=0
•
写模式：CE/PGM=1,OE=1，
V
PP
为高

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
2048x8b的UVEPROM的编程时序图
t
s
：设置时间，
t
h
：保持时间

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Flash（闪存）的单元结构
（浮栅存储的电子多寡分别代表0和1）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Flash单元的删除操作
（在晶体管的源极施加高电平，从浮栅中移除电子）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Flash单元的写操作
•
初始化：通过删除操作把浮栅的电子移除
•
存储0：在控制栅施加高电平，在浮栅存储电子

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Flash单元的读操作
•
0：晶体管截止，无电流；1：晶体管导通，有电流
•
电流很弱，需通过感应电路识别，如果感应电路的
分辨率足够高，一个单元的电荷状态可以再细分，
比如4个级别代表2位数据，8个级别代表3位数据

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
基本的Flash阵列结构

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 11–2   Comparison of types of memories.
各种存储器件技术的对比
•
Flash是目前常用的非易失半导体存储器
•
SRAM和DRAM是目前常用的易失半导体存储器，前者速
度高、容量低，后者速度低、容量高

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
存储器两种扩展方式：字长扩展，字数扩展
（示例：用两片65,536×4的ROM构成一个65,536×8的ROM）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
65,536×4的ROM逻辑符号

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
字长扩展到8位：地址同时连接到2个ROM芯片

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
字长扩展到16位：地址同时连接到4个ROM芯片

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用两片2
m
×n的RAM来构成一个2
m
×2n的RAM，
即字长从n位扩展到2n位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用两片1M×4的SRAM来构成一个1M×8的SRAM

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用两片1M×8的RAM来构成一个2M×8的SRAM，
即字数从1M扩展到2M

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：用512K×4的RAM来构成一个1M×4的RAM
（字数为1M，需要芯片数为1M/512k=2）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
RAM模块的两种封装形式：SIMM和DIMM
•
SIMM：引脚在电路板单面，引脚数：30(8位数据总线），
72(32位数据总线）
•
DIMM：引脚在电路板双面，引脚数： 72、100、144、168，
支持32和64位数据总线

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Chapter

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIFO寄存器：数据先入先出
（注意：与移位寄存器有区别）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIFO串行存储器模块图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIFO寄存器应用示例：数据速率缓冲（变换）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
LIFO寄存器：数据后入先出
（示例：堆栈，数据从栈顶压入和弹出）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
从栈顶压入数据

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
从栈顶弹出数据

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
示例：内存中的64KB堆栈

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
64KB堆栈的压入和弹出操作

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
CCD（电荷耦合器件）
•
以电容存储电荷多寡来代表1和0，无晶体管
•
存储单元以串行通道形式来组织

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
硬盘结构图
时间延迟参数：
寻轨时间，寻扇区时
间，传输时间

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
硬盘的读写磁头
写：极化磁介质；读：感应磁介质的极化状态

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
QIC磁带
（只能顺序读写）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
磁-光（MO）盘的读写原理
（介质高温时可磁化，常温时稳定保持磁化状态）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
CD-ROM磁光盘的读操作
（介质的刻录状态代表0和1）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
