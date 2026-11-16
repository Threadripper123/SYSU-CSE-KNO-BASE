# 01-2-Introduction

> 主题库：数据结构与算法　｜　源文件：01-2-Introduction.pdf　｜　8,912 字符　｜　46 个片段　｜　更新于 2026-11-11


Introduction
Yan Pan  (潘炎)
School of Computer Science and Engineering
Data Structure & Algorithm
E-mail: panyan5@mail.sysu.edu.cn

Before this course
Pre-courses:
Introduction to 
   Computer Science
Programming Languages
e.g., C/C++
Mathematical Courses
......
Data Structure & Algorithm2

A Simple Case
084
2
xx
a
acbb
x
a
acbb
x
2
4
2
2
4
1
2
2




Main()
{ int a, b, c;
   float x1, x2;
   a = 1;
   b = 4;
   c = -8;
   x1=(-b+sqrt(b*b-4*a*c)/(2*a);
   x2=(-b-sqrt(b*b-4*a*c)/(2*a);
}
Data Structure & Algorithm3

Why to Study Data Structures and Algorithms? 
除了数值计算问题，计
算机还要面对什么样的
数据和问题？
Data Structure & Algorithm4

Case 1: Document Editing 
Data Structure & Algorithm5

Case 2: Tables 
Data Structure & Algorithm6

Case 3: Trees -- Eight-Queens Puzzle
......
........
........
..................
Data Structure & Algorithm7

Case 4: Graphs
C
4
, C
5
, C
6
Database TheoryC
7
C
2
,
 
C
4
Computer NetworkC
6
C
3
, C
4
Data StructureC
5
C
1
, C
2
ProgrammingC
4
C
1
Discrete Math.C
3
None
Intro. to ComputerC
2
None
Advanced Math.C
1
Pre.CourseID
C
1
C
2
C
3
C
4
C
6
C
5
C
7
Data Structure & Algorithm8

Case 5: Design for the Metro System 
Data Structure & Algorithm9

Case 6: Search Engines
Data Structure & Algorithm10

Our main concerns in solving problems
√  What is the data in the problem?
 √  How to define and describe the relationships between data?
√  How to design the algorithms to solve the problem? 
√  How to analyze the algorithm?
e.g. running time, storage
Data Structure & Algorithm11

Our main concerns in solving problems
Characteristic of computer applications:
•Data with complex relationships
•Numerical and Non-numerical 
Data Structure & Algorithm12

Analyzing Algorithms
In many problems, 
writing a working program is not good enough. 
If we need to run the program on a large dataset, then the running time 
becomes an issue. 
In this course, we will study how to estimate the running time of a program 
for large input and, more importantly, how to compare the running time of 
two programs without actually implementing them. 
Data Structure & Algorithm13

Course purposes
The main topics are abstraction, design and implementations in 
solving problems.
Abstraction: To analyze the requirements of problem and the 
logical structures of data;
Design: To design the physical structures of data and algorithms, 
to analyze the algorithm’s time complexity;
Implementations: to implement the algorithms by using a 
specific computer language, e.g., C/C++.
Data Structure & Algorithm14

Definitions/Terms
•数据结构课程是一门研究非数值计算的程序设计问题中的
操作对象，以及它们之间的关系和操作等相关问题的学科。
•数据 (Data)：是描述客观事物的符号，是计算机中可以操作
的对象，是能被计算机识别，并输入给计算机处理的符号
集合。
•数值型数据
•文本、音频、视频、图片等
Data Structure & Algorithm15

Definitions/Terms
Case 1: 
084
2
xx
b
acbb
x
b
acbb
x
2
4
2
2
4
1
2
2




Main()
{ int a,b,c;
   float x1,x2;
   a=1;
   b=4;
   c=-8;
   x1=(-b+sqrt(b*b-4*a*c)/(2*b);
   x2=(-b-sqrt(b*b-4*a*c)/(2*b);
}
Data Structure & Algorithm16

Definitions/Terms
•数据元素（Data Elements）：是组成数据的、有一定意义的基本
单位，在计算机中通常作为整体进行处理。也被称作是记录。
•人，牛，羊，马等...
•数据项（Data Item）：一个数据元素可以由若干个数据项组成。
•人可以有眼、耳、口、鼻、手、脚等数据项，数据项是数据不可分割
的最小单位。
•数据对象（Data Object）：是性质相同的数据元素的集合，是数
据的子集。
•性质相同的例子，人都有姓名、生日和性别等相同的数据项。
Data Structure & Algorithm17

Definitions/Terms
•Structure: an intuitional comprehension is a type of relationship.
•数据结构：是相互之间存在一种或多种特定关系的数据元素
的集合。
•In computer science, a data structure is a particular way of 
organizing data in a computer so that it can be used efficiently.
Data Structure & Algorithm18

Case: Graph
C
4
, C
5
, C
6
Database TheoryC
7
C
2
,
 
C
4
Computer NetworkC
6
C
3
, C
4
Data StructureC
5
C
1
, C
2
ProgrammingC
4
C
1
Discrete Math.C
3
None
Intro. to ComputerC
2
None
Advanced Math.C
1
Pre.CourseID
C
1
C
2
C
3
C
4
C
6
C
5
C
7
Data Structure & Algorithm19

Logical Structure vs Physical Structure 
•The Data Structure can be classified into two types.
•Logical Structure (逻辑结构) 
•Physical Structure (存储结构) 
Data Structure & Algorithm20

Logical Structure 
•逻辑结构：
•反映了我们对数据含义的解释，它可以用一组数据以及这些数据之
间的关系表示。数据的逻辑关系是从具体问题抽象出来的数据模型，
与数据元素本身的形式、内容无关，与数据的存储方式也无关。
•퐀是数据集合，퐀是定义在集合퐀上的一组上的二元关系集合퐀×퐀，
用它描述结点数据之间的逻辑关系。
•主要关注的逻辑结构一共有4种。
Data Structure & Algorithm21

Types of logical structure
(1) Set (集合)：数据元素之间就是
  “属于同一个集合”。
(2) List (线性结构)：数据元素之间存在着
一对一的线性关系。
(3) Tree (树结构)：数据元素之间存在
着一对多的层次关系。
(4) Graph (图结构)：数据元素之间存在着
多对多的任意关系。
Data Structure & Algorithm22

•存储结构：
•又称为物理结构，是数据及其逻辑结构在计算机中的表示方式，指
数据该如何在计算机中存放，实质上是内存单元分配，在具体实现
时用计算机语言中的数据类型（Data Type）来描述。
Physical structure
Data Structure & Algorithm23

Two types of physical structure
（1）Sequential Structure：
üStorage cell with continuous 
location address 
üLogical relationship is 
described by the function of 
storage location
...
bat
cat
eat
...
Start
Case:（bat, cat, eat）
Data Structure & Algorithm24

Two types of physical structure
Case：(bat, cat, eat）
0200
0208
0300
0325
...
...
...
...
  bat
0200
  cat
0325
  eat
  ∧
（2） Linked Structure：
ü Storage cell
ü Logical relationship is 
described by the pointer
Data Structure & Algorithm25

Physical structure
•存储数据，应该存些什么？怎样存？
•第一，不仅仅要保存数据本身的值，还要保存数据间的关系，这样才能把相关问题
的所有信息都存储完整。保存数据间联系的目的，是可以通过这种联系找到与之相
连的元素；
•第二，我们在计算机中存储数据的目的是为了对它们进行处理，如果存进机器了但
要用时找不到，数据的存储就失去了意义，这里的“找到”的含义，一个是能找到
需要的数据元素，另一个是能找到与之相联系的数据元素
Data Structure & Algorithm26

Logical Structure vs Physical Structure 
•Relationship between logical structure and physical structure
•数据的逻辑结构属于用户视图，是面向问题的，反映了数据内部的构
成方式，数据的逻辑结构独立于计算机；
•数据的物理存储结构的实现要用计算机语言中的数据类型来描述，因
而是依赖于具体的计算机语言的，是面向计算机的。
•一种数据的逻辑结构可以用多种存储结构来存储，而采用不同的存储
结构，其数据处理的效率往往是不同的。 
Data Structure & Algorithm27

数据的操作/运算
•数据的运算有两个方面的含义，运算的定义与运算的实现：
•运算的定义，取决于数据的逻辑结构，知道了问题中的数据及数据间的联系，我们
就可以设计相应的数据处理方法。
•一般常见的运算操作有以下这些：
•初始化：对存储结构设置初始值，或者申请存储空间。
•判空：判断存储空间是否为未存放有效值的状态
•查找：判断是否包含指定元素
•遍历：按某种次序访问所有元素，每个元素只被访问一次
•插入：增加指定元素
•删除：移去指定元素
•......
Data Structure & Algorithm28

Definition of ADT
•(Abstract Data Type, ADT) is a mathematical model for data types where a data 
type is defined by its behavior (semantics) from the point of view of a user of the 
data, specifically in terms of possible values, possible operations on data of this 
type, and the behavior of these operations. 
•抽象数据类型是指一个数学模型及其定义在该模型上的一组操作。
•抽象数据类型的定义仅取决于它的一组逻辑特性，而与其在计算机内部如何表
示和实现无关。
ADT  Name
    Describe of data；
    Operations;
END ADT
Data Structure & Algorithm29

Definition of ADT
Operation description：
    Name（parameters list）
    Input：      data used to input；
    Output：   data used to output；
    Pre-condition： if the condition can not be satisfied, the 
                               operation may be not correct. 
    Post-condition：The status after the operation be executed 。
Data Structure & Algorithm30

Case: Big integer
ADT Bigint
  DATA
      n: 0..2
512
-1;
  Operations
      Addone
         Pre-condition： n+1<2
512
;
         Post-condition：n=n+1;
Subone
   Pre-condition：n>0;
   Post-condition：n=n-1;
Mult(x,y)
   Input：(x:Bigint)；
   Outpit:  (y: Bigint)；
   Pre-condition：n*x.n<2
512
;
   Post-condition：y.n=n*x.n;
End ADT
Data Structure & Algorithm31

Case: Triple
Data Structure & Algorithm32

C++ Classes
•C++ has emerged as the leading systems programming 
language. In addition to fixing many of the syntactic flaws of C, 
C++ provides direct constructs (the class and template) to 
implement generic data structures as abstract data types.
ADT
·逻辑结构
·操作集合
数据结构
·存储结构
·算法设计
类class
·成员变量
·成员函数
(a)问题的抽象描述     (b) ADT的设计       (c) ADT的计算机实现
Data Structure & Algorithm33

A case：逐步求精的设计过程
Data Structure & Algorithm34

A case：逐步求精的设计过程
Data Structure & Algorithm35

A case：逐步求精的设计过程
例题：设学校有足够的课室供学生考试使用。学校开设的课程名单和学生选读课
程的名单都已经分别存放在名为csfile和scsfile的文件中。规定每一个学生同一天
至多参加一门课程的考试，希望编排一个考试时间表，尽量缩短考试的周期。编
写程序，完成考试的分组情况。
（哪一科目在哪一天、哪个课室考试由人工安排）。
Data Structure & Algorithm36

A case：逐步求精的设计过程
•Analysis
Input data：
csfile 文件中一个数据表示一门功课，每个数据
由五个字符构成，前两位是字母，第三位是数
字 1..4，第四位是数据0或1，第五位是数字0..9。 
文件中任何数据都不相同，数据之间以空白字
符分隔。
scsfile文件中每个记录表示一个学生选学的课
程，每个记录由一个学号和所选的课程名构成，
中间以空白分隔。
Data Structure & Algorithm37

A case：逐步求精的设计过程
Output：
   将可以同时进行考试的科目分组输出，使得同一组里的任
何两门功课都没有同一位学生同时选读。每组之间以一行
“*“分隔。
Data Structure & Algorithm38

A case：逐步求精的设计过程
•Design
Data Structure & Algorithm39

A case：逐步求精的设计过程
Data Structure & Algorithm40

A case：逐步求精的设计过程
Data Structure & Algorithm41

A case：逐步求精的设计过程
Data Structure & Algorithm42

Summary
•数据结构：是相互之间存在一种或多种特定关系的数据元
素的集合。
•Logical Structure vs Physical Structure 
•数据的逻辑结构属于用户视图，是面向问题的，反映了
数据内部的构成方式；
•数据的物理存储结构属于具体实现的视图，是面向计算
机的。
Data Structure & Algorithm43

Summary
•(Abstract Data Type, ADT) is a mathematical model for data types where a data 
type is defined by its behavior (semantics) from the point of view of a user of the 
data, specifically in terms of possible values, possible operations on data of this 
type, and the behavior of these operations. 
•抽象数据类型是指一个数学模型及其定义在该模型上的一组操作。
•抽象数据类型的定义仅取决于它的一组逻辑特性，而与其在计算机内部如何表
示和实现无关。
ADT  Name
    Describe of data；
    Operations;
END ADT
Data Structure & Algorithm44
