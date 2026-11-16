# ch02 Number Systems, Operations, and Codes

> 主题库：数字电路基础　｜　源文件：ch02 Number Systems, Operations, and Codes.pdf　｜　6,787 字符　｜　40 个片段　｜　更新于 2026-11-11


Digital Fundamentals
CHAPTER
ELEVENTH EDITION
Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Number Systems,
Operations, and 
Codes
2

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
十进制：数基，位权

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–1

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-1Illustration of a simple binary counting application.
二进制的计数结果：
0000-1111

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–2   Binary weights.
二进制：数基，位权

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
例子：二进制数转换到十进制数

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
十进制整数到二进制的转换:重复除以2

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
十进制小数到二进制的转换:重复乘以2

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二进制加法

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二进制减法

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二进制乘法二进制除法

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-2Example of inverters used to obtain the 1’s complement of a binary number.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-3Example of obtaining the 2’s complement of a negative binary number.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二进制数的1’s 、2’s complement
•
1’s complement：各位取反
•
2’s complement：1’s complement+1（最低位的1的
左边各位取反，其余保持不变）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
有符号的整数表示
•
1位符号位：0-正数，1-负数
•
三种编码：Sign-magnitude （原码）、1’s 
complement （反码）、2’s complement （补码）
•
正数的三种编码相同，负数的三种编码不同
•
负数的反码=正数的各位取反；负数的补码=正数的各位
取反+1
整数原码反码补码
+2500011001  00011001  00011001
-2510011001  11100110  11100111

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
有符号整数的十进制数值
•
原码：符号位决定正负，所有为1的数值位的权值和
•
反码：正数：所有为1的位的权值和；负数：-(符号位
的权值)+所有为1的位的权值和+1
•
补码：正数：所有为1的位的权值和；负数：-(符号位
的权值)+所有为1的位的权值和
•
n位补码的表达范围：

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
浮点数的表示
•
标准：ANSI/IEEE 754-1985
•
三种精度：32位单精度，64位双精度，80位扩展精度
•
三个字段：符号位，指数（偏置），小数（隐含1）

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
有符号数的算术运算
•
加法：相同符号的两个数相加可能溢出
•
溢出判断：同符号数相加结果的符号位错误
•
减法：转化为：加（-x）的补码
•
乘法：直接加，部分积
•
除法：直接减，部分商

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–3

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
二、八、十和十六进制的转换
•
3、4位二进制分别对应1位八进制、十六进制
•
不同进制的算术运算原理相同，差别在于数基

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-4Getting the 2’s complement of a hexadecimal number, Method 1.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-5Getting the 2’s complement of a hexadecimal number, Method 2.
取反

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-6Getting the 2’s complement of a hexadecimal number, Method 3.
取反

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–4   Octal/binary conversion.
BCD编码有多种，常用的是8421-BCD码，8421对应各位的权

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
BCD加法中的调整

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–6   Four-bit Gray code.
•
格雷码的任意两个相邻编码只有一位不同
•
编码长度不局限于4位

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
格雷码和二进码的转换

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-7A simplified illustration of how the Gray code solves the error problem in shaft position encoders. 
Three bits are shown to illustrate the concept, although most shaft encoders use more than 10 bits to achieve a 
higher resolution.

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
TABLE 2–7   American Standard Code for Information 
Interchange (ASCII).

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
•
奇校验：编码包含
奇数个“1”
•
偶校验：编码包含
偶数个“1”
•
可检测奇数个位错

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
FIGURE 2-8 The CRC process.
循环冗余校验码的生成和检测
第二位错
生成码

Digital Fundamentals, Eleventh Edition
Thomas L. Floyd
Copyright © 2015 by Pearson Education, Inc.
All Rights Reserved
Q&A
