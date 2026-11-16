# 06-String

> 主题库：数据结构与算法　｜　源文件：06-String.pdf　｜　12,643 字符　｜　57 个片段　｜　更新于 2026-11-11


String
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

String（字符串）
•In computer programming, a string is traditionally a sequence of 
characters, either as a literal constant or as some kind of variable. 
•A string is generally understood as a data type and is often 
implemented as an array of bytes (or words) that stores a sequence of 
elements, typically characters, using some character encoding. 
2Data Structure & Algorithms

Main contents
•Basic concepts of string
•Abstract data type of string
•Storage & implementation of string
•Pattern matching of string
3Data Structure & Algorithms

Basic concepts of string
Data Structure & Algorithms4
•字符串(String)是零个或多个字符组成的有限序列。一般记作
S=“a
1
a
2
a
3
...a
n
”，其中 S 是串名，双引号括起来的字符序列是串值；
a
i
(1≦i≦n)可以是字母、数字或其它字符。
•串中所包含的字符个数称为该串的长度。长度为零的串称为空串
(Empty String)，它不包含任何字符。
•通常将仅由一个或多个空格组成的串称为空白串(Blank String)
•注意：空串和空白串的不同，例如“  ”和“”分别表示长度为1的空
白串和长度为0的空串。

Basic concepts of string
Data Structure & Algorithms5
字符串的表示：
    s= “a
1
a
2
...a
n
”    (n0)
    s1= “”
    s2= “    ”
•s1中没有字符，是一个空串；
•s2中有若干个空白格字符，它的长度不等于0，它是由
空格字符组成的串，一般称此为空格串。

思考
•顺序表与顺序串在存储结构设计上是否有什么不同？
•先回顾一下顺序表的存储结构
•顺序表备用空间的设置，是为了在插入新结点时有扩充的余地，如图所示：
•因为有备用空间的设计，因此串的实际长度是串存储时必须已知的一个参数。可以设计
如下的储方案：
（1）用一个指针指示最后一个字符位置。
（2）直接用一个计数器记录串元素个数。
（3）在串末尾添加串结束标记：在串尾存储一个不会在串中出现的特殊字符作为串的终结符，以
此表示串的结尾。（注：C语言中处理定长串的方法就是这样的，它是用 ’\0’ 来表示串的结束，用
查找串结束标志的方法可以间接求得串的长度）。
6Data Structure & Algorithms

Basic concepts of string
Data Structure & Algorithms7
•字符串中任意连续的字符组成的子序列被称为该字符串的子串。
•包含子串的字符串又被称为该子串的主串。
a = “Welcome to Beijing”
b= “Welcome”
c= “Bei”
d= “Welcome to”
•两个字符串相等：两个字符串的长度相等，并且各个对应的字符也都相同。
•例如，有下列四个字符串a，b，c，d：
a= “program”
b= “Program”
c= “pro”
d= “program  ”

Basic concepts of string
•字符串常量和变量
•String constant (字符串常量)
•例如： "\n"
•String variant (字符串变量)
•例如：a = “program”
         a = “NEW”
8Data Structure & Algorithms

Basic concepts of string
•字符(char) ：组成字符串的基本单位 。
•在C和C＋＋中
•单字节（8 bits）
•采用ASCII码对128个符号（字符集charset）进行编码 
9Data Structure & Algorithms

Standard string in C++
•标准字符串：将C++的<string.h>函数库作为字符串数据类型
的方案。
•例如：char  S[M];
•字符串的结束标记：'\0'
•'\0'是ASCII码中8位BIT全0码，又称为NULL符。
10Data Structure & Algorithms

Standard string in C++
•1. 字符串长函数
   int strlen(char  *s);
•2. 字符串复制
   char *strcpy(char  *s1, char*s2);
•3. 字符串拼接  
   char *strcat(char  *s1, char  *s2);
•4.字符串比较  
   int  strcmp(char *s1, char *s2);
11Data Structure & Algorithms
5．输入和输出函数
6．定位函数  
   char *strchr(char *s, char c);
7．逆定位函数  
   char *strrchr(char  *s, char  c);

Standard string in C++
12Data Structure & Algorithms
•For example, a string s
“The quick brown dog jumps over the lazy fox. ”
•Find the character ‘h’, strchr(s, ‘r’); return 1.
•Inversely find the character ‘h’, strrchr(s, ‘r’); return 
43.

Storage & implementation of string
       字符串是一种特殊的线性表，其存储表示和线性表类似，但又不完全相同。
字符串的存储方式取决于将要对字符串所进行的操作。字符串在计算机中有3种
表示方式：
◆ 定长顺序存储表示：将字符串定义成字符数组，利用字符串名可以直接访问字
符串值。用这种表示方式，字符串的存储空间在编译时确定，其大小不能改变。
◆ 堆分配存储方式：仍然用一组地址连续的存储单元来依次存储字符串中的字符
序列，但字符串的存储空间是在程序运行时根据字符串的实际长度动态分配的。
◆ 块链存储方式：是一种链式存储结构表示。
13Data Structure & Algorithms

Storage & implementation of string
•字符串的定长顺序存储表示
•这种存储结构又称为字符串的顺序存储结构。是用一组连续的存储单
元来存放字符串中的字符序列。所谓定长顺序存储结构，是直接使用
定长的字符数组来定义，数组的上界预先确定。
14Data Structure & Algorithms
定长顺序存储结构定义为：
#define MAX_STRLEN  256
typedef  struct
{  char  str[MAX_STRLEN] ;
int  length;
} StringType ;   

Storage & implementation of string
•字符串的堆分配存储表示
实现方法：系统提供一个空间足够大且地址连续的存储空间(称为“堆”)供字符
串使用。可使用C语言的动态存储分配函数malloc()和free()来管理。
特点：仍然以一组地址连续的存储空间来存储字符串值，但其所需的存储空间是
在程序执行过程中动态分配，故是动态的，变长的。
15Data Structure & Algorithms
字符串的堆式存储结构的类型定义
typedef  struct
{  char *ch;    /*  若非空，按长度分配，
否则为NULL */
int length;      /*  字符串的长度  */
} HString ;

Storage & implementation of string
链式存储表示
l串的链式存储结构和线性表的串的链式存储结构类似，采用单链表来存储串，
结点的构成是：
◆ data域：存放字符，data域可存放的字符个数称为结点的大小；
◆ next域：存放指向下一结点的指针。
l若每个结点仅存放一个字符，则结点的指针域就非常多，造成系统空间浪费，
为节省存储空间，考虑串结构的特殊性，使每个结点存放若干个字符，这种结
构称为块链结构。如下图是块大小为3的串的块链式存储结构示意图。
16Data Structure & Algorithms
a    b   c  e    p   c  g   @   @  ⋀ 
⋯⋯
head
串的块链式存储结构示意图

Storage & implementation of string
在这种存储结构下，结点的分配总是完整的结点为单位，因此，为使一个串能存
放在整数个结点中，在串的末尾填上不属于串值的特殊字符，以表示串的终结。
当一个块(结点)内存放多个字符时，往往会使操作过程变得较为复杂，如在串中
插入或删除字符操作时通常需要在块间移动字符。
17Data Structure & Algorithms
串的块链式存储的类型定义包括：
(1) 块结点的类型定义
#define BLOCK_SIZE  4
typedef  struct  Blstrtype
{  char  data[BLOCK_SIZE] ;
struct  Blstrtype  *next;
}BNODE ;
(2)  块链串的类型定义
typedef  struct
  {  BNODE  head;     /*  头指针  */
      int  Strlen ;        /*  当前长度  */
   } Blstring ;

Pattern matching of string
•子串在主串中的定位称为模式匹配(Pattern Matching)或字符串匹配
(String Matching)。模式匹配成功是指在主串S中能够找到模式串T，
否则，称模式串T在主串S中不存在。
•此运算的应用在非常广泛。
•模式匹配是一个较为复杂的串操作过程。对于串匹配算法的研究，
已经有三十多年的历史，著名的匹配算法有BF算法、KMP算法、
BM算法、BMH算法、RK算法和SUNDAY算法等。计算机科学家
们提出了很多优秀的算法，如Aho-Corasick算法、Wu-Manber
算法等，AC算法就是KMP算法在多模式匹配情况下的扩展，Wu-
Manber基于BM算法思想。
Data Structure & Algorithms18
S：
T：

Naïve pattern matching（exhaustion）
Data Structure & Algorithms19
•在串匹配中，一般将主串称为目标串，子串称之为模式串。
设S为目标串，T为模式串，且不妨设：
S=“s
1
s
2
...s
n-1
”  T=“t
1
...t
m-1
” 
•串的匹配实际上是对于合法的位置1≦i≦n-m依次将目标串
中的子串S[i..i+m-1]和模式串T[1..m-1]进行比较
•若S[i..i+m-1]=T[1..m-1]，则称从位置i开始的匹配成功，
亦称模式T在目标S中出现；
•若S[i..i+m-1]≠T[1..m-1]，则称从位置i开始的匹配失败。

Naïve pattern matching（exhaustion）
Data Structure & Algorithms20
l把模式与目标逐一进行比较（首位置开始），直到碰到不匹配的字符为止
（模式右移一位再次开始匹配）。
l算法可在第一个匹配或是目标的结束处停止。
abacab
abacab
abacaacabcabacabaa
01234587691011121314151617
a
b
acab
abacab
abacab
abacab
............
abacab

Implementation
Data Structure & Algorithms21
#include “String.h”  
#include <assert.h>
int NaiveStrMatching (String T, String P)  { 
int i = 0;// 模式的下标变量
int j = 0;// 目标的下标变量
nt pLen = P.length( );             // 模式的长度
int tLen = T.length( );// 目标的长度
if  (tLen < pLen) // 如果目标比模式短，匹配无法成功
return (-1);          
while ( i < pLen  &&  j < tLen)  // 反复比较对应字符来开始匹配
   if (T[j] == P[i]) 
i++,  j++;
else  { 
 j = j – i + 1; 
i = 0;
}
if ( i > = pLen)
return (j – pLen+1);
else return (-1);
 }

Naïve pattern matching（exhaustion）: efficiency analysis
理解该算法的关键点
当第一次s
k
≠t
j
时：主串要退回到k-j+1的位置，而模式串也
要退回到第一个字符（即j=0的位置）。
比较出现s
k
≠t
j
时：则应该有s
k-1
=t
j-1
，...，s
k-j+1
=t
1
， s
k-j
=t
0
 。
22Data Structure & Algorithms

Data Structure & Algorithms23
•假定目标T的长度为n，模式P长度为m，且 m ≤ n 
•整个算法的最坏时间开销为O(mn)
Naïve pattern matching（exhaustion）: efficiency analysis

Naïve pattern matching: worst case
•模式与目标的每一个长度为m的子串进行比较
Data Structure & Algorithms24
l目标形如a
n
, 模式形如a
m-1
b
l总比较次数：O(n-m+1)
l时间复杂度：O(mn)
AAAAAAAAAAAAAAAAAAAAAA
AAAAB            5次比较
AAAAAAAAAAAAAAAAAAAAAA
  AAAAB   5次比较
AAAAAAAAAAAAAAAAAAAAAA
 AAAAB     5次比较
AAAAAAAAAAAAAAAAAAAAAA
   AAAAB           5次比较
............
AAAAAAAAAAAAAAAAAAAAAA
      AAAAB   5次比较

Naïve pattern matching: best case--Matching
•在目标的前m个位置上找到模式，设 m = 5
Data Structure & Algorithms25
l总比较次数：m
l时间复杂度：O(m)
AAAAAAAAAAAAAAAAAAAAAB
AAAAA            
5次比较 

Naïve pattern matching: best case—Not Matching
•总是在第一个字符上不匹配
Data Structure & Algorithms26
l总比较次数：n-m+1
l时间复杂度：O(n)
AAAAAAAAAAAAAAAAAAAAAH
OOOOH            1次比较
AAAAAAAAAAAAAAAAAAAAAH
  OOOOH   1次比较
AAAAAAAAAAAAAAAAAAAAAH
OOOOH   1次比较
AAAAAAAAAAAAAAAAAAAAAH
  OOOOH   1次比较
............
AAAAAAAAAAAAAAAAAAAAAH
        1次比较        OOOOH

Case: naïve pattern matching
•举个例子，如果给定文本串S “BBC ABCDAB ABCDABCDABDE”，
和模式串P “ABCDABD”。
Data Structure & Algorithms27
参考资料：《从头到尾彻底理解KMP》 

Case: naïve pattern matching
Data Structure & Algorithms28

Case: naïve pattern matching
Data Structure & Algorithms29

Case: naïve pattern matching
Data Structure & Algorithms30

Think deeper for the pattern 
matching of the string
31Data Structure & Algorithms

An improved pattern matching--KMP algorithm
•Knuth-Morris-Pratt 字符串查找算法，简称为 “KMP算法”，常用
于在一个文本串S内查找一个模式串P 的出现位置。
•每当一趟匹配过程出现字符不相等时，主串指示器不用回溯，而是利
用已经得到的“部分匹配”结果，将模式串的指示器向右“滑动”尽
可能远的一段距离后，继续进行比较。
•这个算法由Donald Knuth、James H. Morris、Vaughan Pratt、三
人同时独立发现，后取这3人的姓氏命名此算法。
32Data Structure & Algorithms

An improved pattern matching--KMP algorithm
33Data Structure & Algorithms

Steps of KMP algorithm
KMP的算法流程
① 寻找前缀和后缀最长公共元素长度
② 求next数组
③ 匹配失配的处理
34Data Structure & Algorithms

KMP algorithm
•寻找前缀和后缀最长公共元素长度
•如果给定的模式串是：“ABCDABD”，从左至右遍历整个模式串，其各个
子串的前缀后缀分别如下表格所示：
35Data Structure & Algorithms

KMP algorithm
•寻找前缀和后缀最长公共元素长度
•如果给定的模式串是：“ABCDABD”，则字符串对应的各个前缀后缀的公
共元素的最大长度表为（下简称《最大长度表》）：
36Data Structure & Algorithms

KMP algorithm
•基于《最大长度表》匹配
•因为模式串中首尾可能会有重复的字符，故可得出下述结论：
•给定文本串“BBC ABCDAB ABCDABCDABDE”，和模式
串“ABCDABD”
37Data Structure & Algorithms
失配时，模式串向右移动的位数为：
已匹配字符数 - 失配字符的上一位字符所对应的最大长度值

KMP--基于《最大长度表》匹配
38Data Structure & Algorithms
1. 因为模式串中的字符A跟文本串中的字符B、B、C、空格一
开始就不匹配，所以不必考虑结论，直接将模式串不断的右
移一位即可，直到模式串中的字符A跟文本串的第5个字符A
匹配成功：

KMP--基于《最大长度表》匹配
39Data Structure & Algorithms
•2. 继续往后匹配，当模式串最后一个字符D跟文本串匹配时失配，显而
易见，模式串需要向右移动。但向右移动多少位呢？因为此时已经匹
配的字符数为6个（ABCDAB），然后根据《最大长度表》可得失配字符
D的上一位字符B对应的长度值为2，所以根据之前的结论，可知需要向
右移动6 - 2 = 4 位。

KMP--基于《最大长度表》匹配
40Data Structure & Algorithms
•3. 模式串向右移动4位后，发现C处再度失配，因为此时已
经匹配了2个字符（AB），且上一位字符B对应的最大长度
值为0，所以向右移动：2 - 0 =2 位。

KMP--基于《最大长度表》匹配
•4. A与空格失配，向右移动 1 位。
•5. 继续比较，发现D与C 失配，故向右移动的位数为：已匹配的字符数6
减去上一位字符B对应的最大长度2，即向右移动6 - 2 = 4 位。
41Data Structure & Algorithms

KMP--基于《最大长度表》匹配
•6. 经历第5步后，发现匹配成功，过程结束。
42Data Structure & Algorithms

KMP--根据《最大长度表》求出next数组
•已知字符串“ABCDABD”各个前缀后缀的最大公共元素长度
分别为：
•根据这个表可以得出下述结论：
•失配时，模式串向右移动的位数为：
已匹配字符数 - 失配字符的上一位字符所对应的最大长度值
43Data Structure & Algorithms

KMP--根据《最大长度表》求出next数组
•利用最大长度表和结论进行匹配时，可以发现，当匹配到
一个字符失配时，其实没必要考虑当前失配的字符，每次
失配时，都是使用失配字符的上一位字符对应的最大长度
值。
•如此，便引出了next 数组。    
•给定字符串“ABCDABD”，可求得它的next 数组如下：
44Data Structure & Algorithms

KMP--根据《最大长度表》求出next数组
•把next 数组跟之前求得的最大长度表对比后，不难发现，next 数组相
当于“最大长度值” 整体向右移动一位，然后初始值赋为-1。
•换言之，对于给定的模式串：ABCDABD，它的最大长度表及next 数组
分别如下：
• 根据最大长度表求出了next 数组后，从而有
45Data Structure & Algorithms
失配时，模式串向右移动的位数为：失配字符所在位置 - 失配字符对应的next 值

KMP--根据《最大长度表》求出next数组
•根据《最大长度表》，失配时，模式串向右移动的位数 = 已经匹配的字符数 - 
失配字符的上一位字符的最大长度值。
•根据《next 数组》，失配时，模式串向右移动的位数 = 失配字符的位置 - 失
配字符对应的next 值。
•其中，从0开始计数时，失配字符的位置 = 已经匹配的字符数（失配字符不计
数），而失配字符对应的next 值 = 失配字符的上一位字符的最大长度值。
•因此，无论是基于《最大长度表》的匹配，还是基于next 数组的匹配，两者得
出来的向右移动的位数是一样的。
46Data Structure & Algorithms

KMP--根据《next数组》匹配
1. 最开始匹配时
2. P[1]跟S[5]匹配成功，P[2]跟S[6]也匹配成功, ...，直到当匹配到字符D时
失配（即S[10] != P[6]），由于 j 从0开始计数，故数到失配的字符D时 j 为
6，且字符D对应的next 值为2，所以向右移动的位数为：j - next[j] = 6 - 2 
=4 位
47Data Structure & Algorithms

KMP--根据《next数组》匹配
3. 向右移动4位后，C再次失配，向右移动：j - next[j] = 2 - 0 = 2 位
4. 移动两位之后，A 跟空格不匹配，再次后移1 位
48Data Structure & Algorithms

KMP算法--根据《next数组》匹配
•5. D处失配，向右移动 j - next[j] = 6 - 2 = 4 位
•6. 匹配成功，过程结束。
49Data Structure & Algorithms

Steps of KMP algorithm
① 寻找前缀和后缀最长公共元素长度
② 求next数组
③ 匹配失配的处理
•KMP的算法流程
50Data Structure & Algorithms

KMP算法实现
51Data Structure & Algorithms
1. int KmpSearch(char* s, char* p)  
2. {  
3.     int i = 0;  
4.     int j = 0;  
5.     int sLen = strlen(s);  
6.     int pLen = strlen(p);  
7.     while (i < sLen && j < pLen)  
8.     {  
9.         //①如果j = -1，或者当前字符匹配成功（即S[i] == P[j]），都令i++，j++      
10.         if (j == -1 || s[i] == p[j])  
11.         {  
12.             i++;  
13.             j++;  
14.         }  
15.         else  
16.         {  
17.             //②如果j != -1，且当前字符匹配失败（即S[i] != P[j]），则令 i 不变，j = next[j]      
18.             //next[j]即为j所对应的next值        
19.             j = next[j];  
20.         }  
21.     }  
22.     if (j == pLen)  
23.         return i - j;  
24.     else  
25.         return -1;  
26. }  

KMP算法实现--求得next数组
52Data Structure & Algorithms
1. void GetNext(char* p,int next[])  
2. {  
3.     int pLen = strlen(p);  
4.     next[0] = -1;  
5.     int k = -1;  
6.     int j = 0;  
7.     while (j < pLen - 1)  
8.     {  
9.         //p[k]表示前缀，p[j]表示后缀  
10.         if (k == -1 || p[j] == p[k])   
11.         {  
12.             ++j;  
13.             ++k;  
14.             next[j] = k;  
15.         }  
16.         else   
17.         {  
18.             k = next[k];  
19.         }  
20.     }  
21. }  

用递归的思想来计算next[j]
53Data Structure & Algorithms
A1 跟 A2 完全相同（长度为j）
情况1：A1的下一个字符等于A2的下一个字符
“若T[i+1] == T[j+1]，则next[i+1] = j + 1”
计算next值

用递归的思想来计算next[j]
54Data Structure & Algorithms
情况2：A1的下一个字符不等于A2的下一个字符
若T[i+1] ！= T[j+1]，则令j = next[j]
重新判断T[i+1]是不是等于 T[j+1]
计算next值

KMP算法的时间复杂度分析
•假设现在文本串S匹配到 i 位置，模式串P匹配到 j 位置，发现如果某个字
符匹配成功，模式串首字符的位置保持不动，仅仅是i++、j++；如果匹
配失配，i 不变（即 i 不回溯），模式串会跳过匹配过的next [j]个字符。
55Data Structure & Algorithms

KMP算法的时间复杂度分析
•整个算法最坏的情况是，当模式串首字符位于i - j的位置时才匹配成功，
算法结束。
•如果文本串的长度为n，模式串的长度为m，那么匹配过程的时间复杂度
为O(n)，算上计算next的O(m)时间，KMP的整体时间复杂度为O(m + n)。
56Data Structure & Algorithms

Summary
•根据《最大长度表》，失配时，模式串向右移动的位数 = 已经匹配的字符数 - 
失配字符的上一位字符的最大长度值。
•根据《next 数组》，失配时，模式串向右移动的位数 = 失配字符的位置 - 失
配字符对应的next 值。
•其中，从0开始计数时，失配字符的位置 = 已经匹配的字符数（失配字符不计
数），而失配字符对应的next 值 = 失配字符的上一位字符的最大长度值。
•因此，无论是基于《最大长度表》的匹配，还是基于next 数组的匹配，两者得
出来的向右移动的位数是一样的。
57Data Structure & Algorithms
