# ch04 Boolean Algebra and Logic Simplification

> 主题库：数字电路基础　｜　源文件：ch04 Boolean Algebra and Logic Simplification.pdf　｜　7,836 字符　｜　52 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Boolean Algebra 
and Logic 
Simplification
4

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
布尔加法、乘法
术语：
•
原变量A，反变量A’，A或A’统称变量
•
和项：变量的和，A+B，A+B’，所有变量值为0时等于0
•
积项：变量的积，AB，AB’，所有变量值为1时等于1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
布尔代数基本定理
•
交换律：A+B=B+A，AB=BA
•
结合律：A+(B+C)=(A+B)+C，A(BC)=(AB)C
•
分配律：A(B+C)=AB+AC

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 4–1   Basic rules of Boolean algebra.
布尔代数基本运算公式及其电路应用

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
TABLE 4–2Rule 10: A + AB = A.

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
德·摩根定理的两种形式
•
变量积的补等于变量补的和
•
变量和的补等于变量补的积

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
代入定理和德·摩根定理的运用例子

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
运用德·摩根定理来从异或推导出同或

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
从电路写出对应的逻辑表达式，有助于理解电
路的逻辑功能以及化简

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
化简电路设计尽量减少逻辑门：变量数，项数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 4-21

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
SOP：sum of product，积之和，与-或逻辑
POS：product of sum，和之积，或-与逻辑
逻辑表达式的域：表达式包含的逻辑变量的集合

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
AB+BCD+AC=(AB+BCD+AC)’’
=((AB)’(BCD)’(AC))’
两次取反相互抵消

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
标准SOP：每个积项包含该表达式的域的所有变量，
任何积项为1则等于1，最小项之和
•
标准POS：每个和项包含该表达式的域的所有变量，
任何和项为0则等于0，最大项之积

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
转换为标准SOP的方法：如果一个积项缺少变量A，
则把该积项乘以 (A+A’)，因为A+A’=1
•
积项的二进表示（编号）：该积项等于1的变量的取
值，比如以下乘积项的二进值为1010

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
转换为标准POS的方法：如果一个和项缺少变量A，
则把该和项加以 (AA’)，因为AA’=0
•
和项的二进表示（编号）：该和项等于0的变量的取
值，比如以下和项的二进值为0101

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
标准SOP转换为POS的方法
1.写出SOP表达式中所有积项的二进值
2.写出步骤1结果中缺失的积项的二进值，根据
每个二进值写出其对应和项
3.原理：A=A’’

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
标准POS转换为SOP的方法
1.写出POS表达式中所有和项的二进值
2.写出步骤1结果中缺失的和项的二进值，根据
每个二进值写出其对应积项
3.原理：A=A’’

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 4–6
标准SOP的真值表

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 4–7
标准POS的真值表

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
从真值表写出SOP或POS

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
3变量和4变量的卡诺图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
位置相邻=逻辑相邻
逻辑相邻：只有一个变量不同
位置相邻：左右，上下

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
给定SOP，所包含的最小项的对应格子标“1”

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
在卡诺图中标出标准SOP

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
在卡诺图中标出非标准SOP

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
相邻项的分组化简：每组的项数是2的n次方

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
最简SOP
①项数最少（圈最少）；②每项的变量最少（圈最大）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用卡诺图化简表达式
根据表达式作图，注意标出变量次序，然后求最简分组

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
表达式、真值表和卡诺图的对应关系
（相关知识：最小项和最大项的编号定义）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
利用无关项来化简的例子
四位BCD编码只用了0000-1001，假设6个没用的编码不允许
在输入出现，则它们不会影响译码输出，属于无关项

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
利用无关项来化简七段发光二极管的a段逻辑

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
POS表达式的卡诺图

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
用卡诺图来化简POS

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
SOP、POS的卡诺图化简

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
五变量的卡诺图化简

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
VHDL标准：IEEE Std. 1076-1993
VHDL程序的两个关键部分：

实体：逻辑函数的输入输出定义

结构体：逻辑函数的内部操作
VHDL设计例子：2输入与门

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
VHDL设计例子：4输入与-或门

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
