# 07-Array

> 主题库：数据结构与算法　｜　源文件：07-Array.pdf　｜　19,440 字符　｜　93 个片段　｜　更新于 2026-11-11


Array
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

数组和广义表 
•数组的定义
•数组的顺序表示
•矩阵的压缩存储（特殊矩阵，稀疏矩阵）
•广义表的定义
•广义表的存储结构
Data Structure & Algorithms2

数组与向量
•解析几何中“向量”是指既有大小又有方向的量（又称为矢量）；线性代数中“向
量”是指n个实数组成的有序数组。
•经常有人把一维数组也称为“向量”，二维数组称为“矩阵”。
•计算机的出现，最初就是解决数学计算问题的，所以里面会有很多概念来自数学，
例如函数、向量。一维数组与“向量”对应，二维数组与“矩阵”对应。
3Data Structure & Algorithms

数组与广义表
•数组是一种人们非常熟悉的数据结构，几乎所有的程序设计语言都支持这种数
据结构，或将这种数据结构设定为语言的固有类型。
•数组这种数据结构可以看成是线性表的推广。 
•科学计算中涉及到大量的矩阵问题，在程序设计语言中一般都采用数组来存储，
被描述成一个二维数组。但当矩阵规模很大且具有特殊结构 (对角矩阵、三角
矩阵、对称矩阵、稀疏矩阵等)，为减少程序的时间和空间需求，采用自定义的
描述方式。 
•广义表是另一种推广形式的线性表，是一种灵活的数据结构，在许多方面有广
泛的应用。
Data Structure & Algorithms4

数组的定义
•数组是一组偶对(下标值，数据元素值)的集合。在数组中，对于一组有
意义的下标，都存在一个与其对应的值。一维数组对应着一个下标值，
二维数组对应着两个下标值，如此类推。
•数组是由n(n>1)个具有相同数据类型的数据元素a
1
，a
2
，
...
，a
n
组成的
有序序列，且该序列必须存储在一块地址连续的存储单元中。
◆ 数组中的数据元素具有相同数据类型。
◆ 数组是一种随机存取结构，给定一组下标，就可以访问与其对应的
数据元素。
◆ 数组中的数据元素个数是固定的。
5Data Structure & Algorithms

数组的定义
•抽象数据类型
•由上述定义知，n维数组中有b
1
b
2 

 
... 
 
b
n
个数据元素，每个数据元素都
受到n维关系的约束。
6Data Structure & Algorithms
ADT Array{
数据对象：j
i
= 0,1,...,b
i
-1 , 1,2, ...,n ; 
D = { a
j
1
j
2
...
j
n
 | n>0称为数组的维数，b
i
是数组第 i 维的长度，j
i 
是
数组元素第 i 维的下标，a
j
1
j
2
...
j
n
∈ElemSet }
数据关系：R = {R
1
, R
2
, ..., R
n
}
R
i
={<a
j
1
j
2 
...
j
i
...
j
n
 , a
j
1
j
2 
...
j
i+1
...
j
n
>|0≦j
k
≦b
k
-1，1≦k≦n且k≠i，0≦j
i
≦b
i
-2， 
a
j
1
j
2 
...
j
i+1
...
j
n
∈D }            
基本操作： ...... 
} ADT Array

数组的定义
7Data Structure & Algorithms
(a) 矩阵表示形式
(b) 行向量的一维数组形式
(c)列向量的一维数组形式
•二维数组图例形式

数组的定义
Data Structure & Algorithms8













nmmm
n
n
nm
aaa
aaa
aaa
A
,2,1,
,22221
,11211
...
............
...
...
数组的特点：
•元素数目固定
•下标有界
数组的操作：
•根据下标进行读写

数组的顺序表示和实现
•数组一般不做插入和删除操作，也就是说，数组一旦建立，结构中的元素个数和
元素间的关系就不再发生变化。因此，一般都是采用顺序存储的方法来表示数组。
•问题：计算机的内存结构是一维(线性)地址结构，对于多维数组，将其存放(映射)
到内存一维结构时，有个次序约定问题。
•即必须按某种次序将数组元素排成一列序列，然后将这个线性序列存放到内存中。
•二维数组是最简单的多维数组，以此为例说明多维数组存放 (映射) 到内存一维
结构时的次序约定问题。
Data Structure & Algorithms9

数组的顺序表示和实现
Data Structure & Algorithms10
通常有两种顺序存储方式：
(1) 行优先顺序(Row Major Order) ——将数组元素按行排列，第i+1个行向量紧接在
第 i 个行向量后面。以二维数组为例，按行优先顺序存储的线性序列为：   
     a
11
, a
12
,  ...,  a
1n
,  a
21
,  a
22
,  ...,  a
2n
,  ...,  a
m1
,  a
m2
,  ...,  a
mn
        
   在C/C++语言中，数组就是按行优先顺序存储的。
(2) 列优先顺序(Column Major Order) ——将数组元素按列向量排列，第 j+1 个列向
量紧接在第 j 个列向量之后，A的 m×n 个元素按列优先顺序存储的线性序列为：
     a
11
,  a
21
,  ...,  a
m1
,  a
12
,  a
22
,  ...,  a
m2
,  ...,  a
n1
,  a
n2
,  ...,  a
nm
   在FORTRAN语言中，数组就是按列优先顺序存储的。

数组的顺序表示和实现
Data Structure & Algorithms11













nmmm
n
n
nm
aaa
aaa
aaa
A
,2,1,
,22221
,11211
...
............
...
...
第1行                     第2行                                第m行
行优先顺序 (Row Major Order) 

数组的顺序表示和实现
•以上规则可以推广到多维数组的情况：
•行优先顺序可规定为，先排最右的下标，从右到左，最后排最左下标；
•列优先顺序与此相反，先排最左下标，从左向右，最后排最右下标。
•按上述两种方式顺序存储的序组，只要知道基地址（即开始结点的存放
地址），维数和每维的上、下界，以及每个数组元素所占用的单元数，
就可以将数组元素的存放地址表示为其下标的线性函数。
Data Structure & Algorithms12

Data Structure & Algorithms13
•设有二维数组A = (a
ij
)
mn
，若每个元素占用的存储单元数为 l (个)， 
•LOC[a
11
]表示元素a
11
的首地址，即数组的首地址。
•以“行优先顺序”存储
第1行中的每个元素对应的(首)地址是：
      LOC[a
1j
] = LOC[a
11
] + (j-1)  l        j = 1, 2,
 
..., n
第2行中的每个元素对应的(首)地址是：
       LOC[a
2j
] = LOC[a
11
] + n  l + (j - 1)  l      j = 1, 2,
 
..., n
... ... ...
第m行中的每个元素对应的(首)地址是：
LOC[a
mj
] = LOC[a
11
] + (m - 1)  n  l + (j - 1)  l     j = 1, 2,
 
..., n     
数组的顺序表示和实现

数组的顺序表示和实现
Data Structure & Algorithms14













nmmm
n
n
nm
aaa
aaa
aaa
A
,2,1,
,22221
,11211
...
............
...
...
第1行                     第2行                                第m行
行优先顺序 (Row Major Order) 

数组的顺序表示和实现
由此可知，二维数组中任一元素 a
ij 
的(首)地址是：
LOC[a
ij
] = LOC[a
11
] + [(i - 1)  n + (j - 1)]  l         (1)
  i = 1, 2,
 
..., m    j = 1, 2,
 
..., n
根据(1)式，对于三维数组A = (a
ijk
)
mnp
，若每个元素占用的存储单元数为 l (个)，
LOC[a
111
]表示元素a
111
的首地址，即数组的首地址。以“行优先顺序”存储在内存中。
三维数组中任一元素 a
ijk 
的(首)地址是：
LOC(a
ijk
) = LOC[a
111
] + [(i-1)  n  p + (j-1)  p + (k-1)]  l   (2)
15Data Structure & Algorithms

数组的顺序表示和实现
Data Structure & Algorithms16
        推而广之，对n维数组A=(a
j
1
j
2
...
j
n
) ，若每个元素占用的存储单元数为 l 
(个)，LOC[a
11 ...1
]表示元素a
11 ...1
的首地址。则以 “行优先顺序” 存储在内
存中。
n维数组中任一元素 a
j
1
j
2
...
j
n 
的(首)地址是：
     LOC[a
j
1
j
2
...
j
n
] = LOC[a
11 ...1
] + [(b
2 
 ...  b
n
)  (j
1 
- 1)
                           + (b
3 
 ...  b
n
)  (j
2 
- 1) + ... 
                           + b
n 
 (j
n-1 
- 1) + (j
n 
- 1)]  l    

数组的顺序表示和实现
•以“列优先顺序”存储
第1列中的每个元素对应的(首)地址是：
      LOC[a
i1
] = LOC[a
11
] + (i - 1)  l        i = 1, 2,
 
..., m
第2列中的每个元素对应的(首)地址是：
       LOC[a
i2
] = LOC[a
11
] + m  l + (i-1)  l     i = 1, 2,
 
..., m
... ... ...
第n列中的每个元素对应的(首)地址是：
LOC[a
in
] = LOC[a
11
] + (n - 1)  m  l + (i - 1)  l     i = 1, 2,
 
..., m 
        由此可知，二维数组中任一元素a
ij
的(首)地址是：
LOC[a
ij
] = LOC[a
11
] + [(j - 1)  m + (i - 1)]  l          
i = 1, 2,
 
..., m    j = 1, 2,
 
..., n
17Data Structure & Algorithms

数组的顺序表示和实现
•假设二维数组A的规模为6X8，每个元素用相邻的6个字节存储，存储器按字
节编址。已知A的起始存储位置（基地址）为1000，元素a
00
为起始，计算：
(1) 数组A的体积（即存储量）
6 x 8 x 6 = 288
(2) 数组A的最后一个元素a
57
的第一个字节的地址
1000 + 288 – 6 = 1282
(3) 按行存储时，元素a
14
的第一个字节的地址
1000 + (1 x 8 + 4) x 6 = 1072
(4) 按列存储时，元素a
47
的第一个字节的地址
1000 + (7 x 6 + 4) x 6 = 1276
18Data Structure & Algorithms

Exercise
•设数组a[1...60，1...70]的基地址为2048，每个元素占2个存储单元，若
以列序为主序顺序存储，求元素a[32, 58]的存储地址。
•注：a[1...60，1...70]是数组的一种表示方法，表示行优先的二维数组，
行下标范围为1到60，列下标范围为1到70。
•解：
•数组行数m=60-1+1=60；列数n=70-1+1=70；
•行下标 i=32；列下标 j=58；
•元素长度 L=2；
•列优先元素求址公式Loc(a
ij
) = Loc(a
11
) +[ (j-1) * m + (i-1) ] * L
•得：LOC(a[32, 58]) = 2048 + [ (58-1) * 60 + (32-1) ] * 2 = 8950
19Data Structure & Algorithms

Exercise
•设数组a[1...60，1...70]的基地址为2048，每个元素占2个存储单元，若
以列序为主序顺序存储，已知元素a[32, 58]的存储地址为8950。
•若数组是a[0...59，0...69]，结果是否仍为8950？
•讨论：
•因为i、j均从0开始，求址公式为Loc(a
ij
) =Loc(a
00
) + [j * m + i] * L（如图）；
•则有：LOC(a[32, 58]) = 2048 + [ 58 * 60 + 32 ] * 2 = 9072
20Data Structure & Algorithms

Exercise
•二维数组M的成员是6个字符（每个字符占一个存储单元）组成
的字符串，行下标 i 的范围从0到8，列下标 j 的范围从0到9。若
M按行优先方式存储时，元素M[8][5]的起始地址与当M按列优先
方式存储时的哪个元素的起始地址一致？    
•M[4][9]
•分析：按行优先方式存储时，
    M[8][5]的存储地址为M + (8 x 10 + 5) x 6 = M + 510，
    M[i][j]按列优先存储时的地址为M + (i + j x 9) x 6，当 i = 4, j = 9 时，
其值为M + 510。
21Data Structure & Algorithms

数据压缩
•数据压缩是指在不丢失信息的前提下，缩减数据量以减少存储空间，
提高其传输、存储和处理效率的一种技术方法。
•数据压缩按照一定的算法对数据进行重新组织，减少数据的冗余和存
储的空间。从信息的角度来看，压缩就是去除掉信息中的冗余，即去
除掉确定的或可推知的信息，而保留不确定的信息。
•数据压缩包括有损压缩和无损压缩。
•若压缩后的数据可恢复完整的原始数据，这种方式称为无损数据压缩；
•若为了实现更高的压缩率允许一定程度的数据损失，这种方式称为有损数
据压缩。
22Data Structure & Algorithms

矩阵的压缩存储
•在科学与工程计算问题中，矩阵是一种常用的数学对象，在高级
语言编制程序时，简单而又自然的方法，就是将一个矩阵描述为
一个二维数组。
•矩阵在这种存储表示之下，可以对其元素进行随机存取，各种矩
阵运算也非常简单，并且存储的密度为1。
Data Structure & Algorithms23














mnmm
n
n
aaa
aaa
aaa
...
............
...
...
21
22221
11211
一个m×n的矩阵。

矩阵的压缩存储
对于高阶矩阵，若其中非零元素呈某种规律分布或者矩阵中有大量的零
元素，若仍然用常规方法存储，可能存储重复的非零元素或零元素，将
造成存储空间的大量浪费。对这类矩阵进行压缩存储：
◆ 多个相同的非零元素只分配一个存储空间；
◆ 零元素不分配空间。
24Data Structure & Algorithms

矩阵的压缩存储
•特殊矩阵
•所谓特殊矩阵就是指非零元素或零元素的分布具有一定规
律的矩阵。
•常见的这类矩阵有：对称矩阵、下（上）三角矩阵、对角
线矩阵等等。
Data Structure & Algorithms25

矩阵的压缩存储--对称矩阵
•对称矩阵 
若一个n阶方阵A=(a
ij
)
nn
中的元素满足性质：
a
ij 
= a
ji
   1≦i, j≦n且i≠j
则称A为对称矩阵，如下图所示。
26Data Structure & Algorithms
对称矩阵示例
1   5   1   3   7
3   0   2   5   1
7   0   6   1   3
5   0   8   0   0
1   8   9   2   6
A=
 a
11
 a
21   
a
22
 a
31   
a
32   
a
33
... ... ...  ...
 a
n1   
a
n2   
...      a
nn
A=

矩阵的压缩存储--对称矩阵
对称矩阵中的元素关于主对角线对称，因此，让每一对对称元素a
ij
和
a
ji
(i≠j)分配一个存储空间，则n
2
个元素压缩存储到n(n+1)/2个存储空间，
能节约近一半的存储空间。
不失一般性，假设按“行优先顺序”存储下三角形(包括对角线)中的元素。
设用一维数组(向量)sa[0...n(n+1)/2]存储n阶对称矩阵，如下图所示。为
了便于访问，必须找出矩阵A中的元素的下标值（i,j）和向量sa[k]的下标
值k之间的对应关系。
27Data Structure & Algorithms
sa
 a
11   
a
21    
a
22     
a
31     
a
32    
a
33       
...
    
a
n1 
 
  
a
n2       
...  
  
a
nn
K     1       2     3      4         ...            n(n-1)/2   ...   n(n+1)/2
对称矩阵的压缩存储示例

矩阵的压缩存储--对称矩阵
若i≧j：a
i j
在下三角形中，直接保存在sa中。a
i j
之前的i-1行共有元素个数：
1 + 2 + ... + (i-1) = i  (i-1) / 2
而在第i行上，a
i j
之前恰有j-1个元素，因此，元素a
i j
保存在向量sa中时的
下标值k之间的对应关系是：
k = i  (i - 1) / 2 + j - 1          i≧j
若i<j：则a
ij
是在上三角矩阵中。因为a
ij
=a
ji
，在向量sa中保存的是a
ji 
。依
上述分析可得：
k = j  (j - 1) / 2 + i - 1         i<j
对称矩阵元素a
i j
保存在向量sa中时的下标值k与（i,j）之间的对应关系是：   
28Data Structure & Algorithms
i(i-1)/2+j-1        当 i≧j 时
j(j-1)/2+i-1        当 i<j 时
k =
1≦i, j≦ n        

矩阵的压缩存储--对称矩阵
根据上述的下标对应关系，对于矩阵中的任意元素a
ij
，均可
在一维数组sa中唯一确定其位置k；反之，对所有：
k=1, 2, ..., n(n+1)/2，
都能确定sa[k]中的元素在矩阵中的位置(i,j)。
称sa[0...n(n+1)/2]为n阶对称矩阵A的压缩存储。
29Data Structure & Algorithms
sa
 a
11   
a
21    
a
22     
a
31     
a
32    
a
33       
...
    
a
n1 
 
  
a
n2       
...  
  
a
nn
K     1       2     3      4         ...            n(n-1)/2   ...   n(n+1)/2
对称矩阵的压缩存储示例

矩阵的压缩存储--三角矩阵
•以主对角线划分，三角矩阵有上三角和下三角两种。
上三角矩阵的下三角（不包括主对角线）中的元素均为常数c 
(一般为0)。下三角矩阵正好相反，它的主对角线上方均为常
数，如下图所示。
30Data Structure & Algorithms
a
11
   a
12
  ...  a
1n
 c     a
22
  ...  a
2n
 c       c  ...   a
nn
  ...    ...    ...
a
11
     c    ...  c
a
21
    a
22
  ...  c
a
n1
    a
n2
  ...  a
nn
 ...    ...    ...
三角矩阵示例
(b)   下三角矩阵示例(a)   上三角矩阵示例

矩阵的压缩存储--三角矩阵
•三角矩阵中的重复元素c可共享一个存储空间，其余的元素正好有
n(n+1)/2个，因此，三角矩阵可压缩存储到向量sa[0...n(n+1)/2]中，其
中c存放在向量的第1个分量中。
•上三角矩阵元素a
i j
保存在向量sa中时的下标值k与(i,j)之间的对应关系是：
•下三角矩阵元素a
i j
保存在向量sa中时的下标值k与(i,j)之间的对应关系是：
31Data Structure & Algorithms
i(i-1)/2+j-1       当i≧j时
n(n+1)/2           当i<j时
K =
1≦i, j≦ n
i(i-1)/2+j-1        当i≦j时
n(n+1)/2            当i>j时
K =
1≦i, j≦n

矩阵的压缩存储--对角矩阵
矩阵中，除了主对角线和主对角线上或下方若干条对角线上的元素之外，
其余元素皆为零。即所有的非零元素集中在以主对角线为了中心的带状
区域中，如图所示。
32Data Structure & Algorithms
a
11
   a
12    
0  ....  0
a
21
   a
22   
a
23
    0 ....  0
0      a
32   
a
33
   a
34
   0 ....  0
   ...    ...     ...    ....  
0   ....     0     0         a
n n-1
    a
n n
0   ....     0   a
n-1 n-2
   a
n-1 n-1
  a
n-1 n
A=
三对角矩阵示例

矩阵的压缩存储--对角矩阵
•如上图三对角矩阵，非零元素仅出现在主对角(a
i i
,1≦i≦n)上、主对角线上
的那条对角线(a
i i+1
,1≦i≦n-1) 、主对角线下的那条对角线上(a
i+1 i
,1≦i≦n-1)。
显然，当| i-j |>1时，元素a
ij
=0。
•由此可知，一个k对角矩阵(k为奇数) A是满足下述条件： 当| i-j |>(k-1)/2时， 
a
i j 
= 0 
33Data Structure & Algorithms

矩阵的压缩存储--对角矩阵
•对角矩阵可按行优先顺序或对角线顺序，将其压缩存储到一个向量中，
并且也能找到每个非零元素和向量下标的对应关系。
•仍然以三对角矩阵为例讨论。
   当i=1，j=1、2，或i=n， j=n-1、n 或
      1<i<n-1, j=i-1、i、i+1的元素a
ij
外，其余元素都是0。
•对这种矩阵，当以按“行优先顺序”存储时， 第1行和第n行是各有2
个非零元素，其余每行的非零元素都要是3个，则需存储的元素个数为
3n-2。
34Data Structure & Algorithms
sa
 a
11    
a
12    
a
21     
a
22     
a
23    
a
32   
a
33     
a
34       
...
     
a
n n-1   
 
 
a
nn
K     1      2      3      4      5      6     7      8      ...   3n-3  3n-2
三对角矩阵的压缩存储示例

矩阵的压缩存储--对角矩阵
•如前图所示三对角矩阵的压缩存储形式。数组sa中的元素sa[k]与三对角
矩阵中的元素a
ij
存在一一对应关系，在a
ij
之前有i-1行，共有3(i-1)-1个
非零元素，在第i行，有j-i+2个非零元素，这样，非零元素a
ij
的地址为：
         LOC[a
i j
] = LOC[a
11
] + [3  (i-1) - 1 + (j - i + 2)]  l 
                         =LOC[a
11
] + (2  i + j - 2)  l
•上例中，a
34
对应着sa[8] ,  k = 2  3 + 4 - 2 = 8，称sa[1...3n-2]是n阶三
对角矩阵A的压缩存储。
•上述各种特殊矩阵，其非零元素的分布都是有规律的，因此总能找到
一种方法将它们压缩存储到一个向量中，并且一般都能找到矩阵中的
元素与该向量的对应关系，通过这个关系，仍能对矩阵的元素进行随
机存取。 
35Data Structure & Algorithms

稀疏矩阵
•在科学与工程领域，求解线性模型时经常出现大型的稀疏矩阵。如数值法求解
偏微分方程时通常也会出现稀疏矩阵。
•再如，手写签名的扫描图像是一幅黑白图像，若以位图方式存储，则白色像素
远远多于黑色部分的像素，就可以将其按稀疏矩阵编码，仅保存其少数像素的
行列信息，如图所示。
•在使用计算机存储和操作稀疏矩阵时，经常需要修改标准算法以利用矩阵的稀
疏结构。由于其自身的稀疏特性，通过压缩可以大大节省稀疏矩阵的内存代价。
更为重要的是，由于过大的尺寸，标准的算法经常无法操作这些稀疏矩阵。
36Data Structure & Algorithms

•稀疏矩阵(Sparse Matrix)：对于稀疏矩阵，目前还没有一个确切的定义。
设矩阵A是一个nm的矩阵中有s个非零元素，设 δ=s/(nm)，称δ为稀
疏因子，如果某一矩阵的稀疏因子δ满足δ≦0.05时称为稀疏矩阵。
稀疏矩阵
Data Structure & Algorithms37
0    12    9    0    0    0    0    0
0     0    0    0    0    0    0    0
-3    0    0    0    0    0    0    4
0     0    24   0    0    2    0    0
0    18    0    0    0    0    0    0
0     0    0    0    0    0   -7    0
A=
0     0    0   -6    0    0    0    0
稀疏矩阵示例

稀疏矩阵
•三元组表示法
•矩阵中的每个元素都是由行序号和列序号唯一确定的。因此，我们需
要用三项内容表示稀疏矩阵中的每个非零元素，即形式为： (i,j,value)
•其中，i表示行序号，j表示列序号，value表示非零元素的值，通常将它
称为三元组。
Data Structure & Algorithms38
(1,2,12),  (1,3,9),
(3,1,-3),   (3,8,4), 
(4,3,24),  (5,2,18), 
(6,7,-7),   (7,4,-6)  

稀疏矩阵
•三元组顺序表
•若以行序为主序，稀疏矩阵中所有非0元素的三元组，就可以得构成该
稀疏矩阵的一个三元组顺序表。相应的数据结构定义如下：
39Data Structure & Algorithms
三元组结点定义    
#define MAX_SIZE 101
typedef int elemtype ;
typedef struct
{      int   row ;     /*  行下标  */
int  col ;        /*  列下标  */
elemtype value;      /*  元素值  */
} Triple ;
三元组顺序表定义    
typedef struct 
{      int  rn ;         /*   行数   */
int  cn ;         /*   列数   */
int  tn ;         /*    非0元素个数   */
Triple   data[MAX_SIZE] ; 
} TMatrix ; 

稀疏矩阵
40Data Structure & Algorithms

稀疏矩阵
•矩阵的运算包括矩阵的转置、矩阵求逆、矩阵的加减、矩阵的乘除等。
在此，先讨论在这种压缩存储结构下的求矩阵的转置的运算。
•一个mn的矩阵A，它的转置B是一个nm的矩阵，且b[i][j]=a[j][i]，
0≦i≦n，0≦j≦m，即B的行是A的列，B的列是A的行。
•设稀疏矩阵A是按行优先顺序压缩存储在三元组表a.data中，若仅仅是
简单地交换a.data中i和j的内容，得到三元组表b.data，b.data将是一个
按列优先顺序存储的稀疏矩阵B，要得到按行优先顺序存储的b.data，
就必须重新排列三元组表b.data中元素的顺序。
41Data Structure & Algorithms

稀疏矩阵的转置
求转置矩阵的基本算法思想是：
① 将矩阵的行、列下标值交换。即将三元组表中的行、列位置值i 、j相
互交换； 
② 重排三元组表中元素的顺序。即交换后仍然是按行优先顺序排序的。
算法思想：按稀疏矩阵A的三元组表a.data中的列次序依次找到相应的三
元组存入b.data中。 
    每转置矩阵的一个三元组，需从头至尾扫描整个三元组表a.data 。找
到之后自然就成为按行优先的转置矩阵的压缩存储表示。
42Data Structure & Algorithms

稀疏矩阵的转置
43Data Structure & Algorithms

稀疏矩阵的转置
•转置矩阵的算法如下：
44Data Structure & Algorithms
void TransMatrix(TMatrix a , TMatrix b)
{   int p , q , col ;
b.rn=a.cn ;  b.cn=a.rn ;  b.tn=a.tn ;
/*    置三元组表b.data的行、列数和非0元素个数 */
if  (b.tn==0)    printf(“ The Matrix A=0\n” );
else
{   q=0;
for  (col=1; col<=a.cn ; col++)
      /*   每循环一次找到转置后的一个三元组  */
     for  (p=0 ;p<a.tn ; p++)
      /*   循环次数是非0元素个数   */
     if  (a.data[p].col==col)
        {  b.data[q].row=a.data[p].col ;
            b.data[q].col=a.data[p].row ; 
            b.data[q].value=a.data[p].value; 
             q++ ;
         }
}
}
算法分析：
本算法主要的工作是在p和col的两
个循环中完成的，故算法的时间复
杂度为O(cn x tn)，即矩阵的列数和
非0元素的个数的乘积成正比。

稀疏矩阵的快速转置
•快速转置的算法
算法思想：直接按照稀疏矩阵A的三元组表a.data的次序依次顺序转换，
并将转换后的三元组放置于三元组表b.data的恰当位置。
前提：若能预先确定原矩阵A中每一列的(即B中每一行)第一个非0元素在
b.data中应有的位置，则在作转置时就可直接放在b.data中恰当的位置。
因此，应先求得A中每一列的非0元素个数。
附设两个辅助向量num[ ]和cpot[ ] 。
◆ num[col]：统计A中第col列中非0元素的个数；
◆  cpot[col] ：指示A中第一个非0元素在b.data中的恰当位置。
45Data Structure & Algorithms

稀疏矩阵的快速转置
•显然有位置对应关系：
46Data Structure & Algorithms
cpot[1]=1
cpot[col]=cpot[col-1]+num[col-1]       2≦col≦a.cn
7

十字链表
对于稀疏矩阵，当非0元素的个数和位置在操作过程中变化较大时，采用
链式存储结构表示比三元组的线性表更方便。
矩阵中非0元素的结点所含的域有：行、列、值、行指针 (指向同一行的
下一个非0元)、列指针(指向同一列的下一个非0元)。其次，十字交叉链
表还有一个头结点，结点的结构如图5-10所示。
47Data Structure & Algorithms
十字链表结点结构
row   col   value
 down      right
  rn     cn     tn
 down      right
(a)   结点结构(b)   头结点结构

十字链表
•由定义知，稀疏矩阵中同一行的非0元素的由right指针域
链接成一个行链表， 由down指针域链接成一个列链表。
则每个非0元素既是某个行链表中的一个结点，同时又是某
个列链表中的一个结点，所有的非0元素构成一个十字交叉
的链表，称为十字链表。
•此外，还可用两个一维数组分别存储行链表的头指针和列
链表的头指针。
48Data Structure & Algorithms

十字链表
十字链表结点的描述如下：
49Data Structure & Algorithms
typedef struct  Clnode  
{   int  row , col ;   /*  行号和列号  */     
elemtype value ;    /*  元素值  */
struct  Clnode  *down , *right ;
} OLNode ;   /*  非0元素结点  */
typedef struct  Clnode  
{   int   rn;        /*  矩阵的行数  */     
int   cn;        /*  矩阵的列数  */
int   tn;        /*  非0元素总数  */
OLNode *rhead ;  
OLNode *chead ; 
} CrossList ;

分析
•特殊矩阵和稀疏矩阵哪一种压缩存储后失去随机存取的功能？为什么？
•特殊矩阵指值相同的元素或零元素在矩阵中的分布有一定规律。因此可以对非零元
素分配单元（对值相同元素只分配一个单元），将非零元素存储在向量中，元素的
下标i和j和该元素在向量中的下标有一定规律，可以用简单公式表示，仍具有随机存
取功能。
•稀疏矩阵式指非零元素和矩阵容量相比很小（t << mxn），且分布没有规律。用十
字链表作存储结构自然失去了随机存取功能。
•即使用三元组表的顺序存储结构，存取下标为i和j的元素时，要扫描三元组表。下标
不同的元素，存取时间也不同。最好情况下存取时间为O(1)，最差情况下是O(n)，
因此也失去了随机存取的功能。
50Data Structure & Algorithms

Exercises
•设矩阵A为
    
2
0
0
4
0
0
3
0
0
3
0
0
4
0
0
0
 
(1) 若将A视为对称矩阵，画出对其进行压缩存储的存储表示(行序方式)，并讨论
如何存取A中的元素a
ij 
(0≤i, j<4)。
(2) 若将A视为稀疏矩阵，画出A的十字链表存储结构。
51Data Structure & Algorithms

Applications: Array Representation
•矩阵：图像(Image)的表示与显示
52Data Structure & Algorithms

Applications: Array Representation
•Image Processing
53Data Structure & Algorithms
•Examples：Reverse color
•R, G, B channels
•Color2Gray
•Gray = a R + b G + c B
•Gray = R*0.299 + G*0.587 + B*0.114
•Add noise
•...

Applications: Array Decomposition
54Data Structure & Algorithms
•Matrix Decomposition
•LU Decomposition
•QR Decomposition
•Cholesky Decomposition
•Jordan Decomposition
•Spectral Decomposition
•Singular Value Decomposition (SVD)
•Low-Rank Decomposition

Applications: Array Decomposition
55Data Structure & Algorithms
•Singular values of flowers image
Rank-1Rank-5Rank-20Rank-80Rank-150True Image

Applications: Array Decomposition
56Data Structure & Algorithms
•Low Rank Matrix
•高维数据往往具有低维结构
•Visual data exhibit low-dimensional structures due to rich local regularities, 
global symmetries, repetitive patterns, or redundant sampling.

Applications: Array Decomposition
•Image Recovery
57Data Structure & Algorithms

Applications: Array Transformation
•Space Motion by Array Transformation
58Data Structure & Algorithms

Applications: Matrix Multiplication
•2022年10月5日，Nature刊出DeepMind团队最新的研究成果《Discovering faster matrix multiplication 
algorithms with reinforcement learning》。DeepMind 提出了 AlphaTensor，并表示它是第一个可用于为矩
阵乘法等基本任务发现新颖、高效且可证明正确的算法的人工智能系统。
•简单来说，使用 AlphaTensor 能够发现新算法。这项研究揭示了 50 年来在数学领域一个悬而未决的问题，
即找到两个矩阵相乘最快方法。
59Data Structure & Algorithms
AlphaTensor 目标是找到正确的矩阵乘法算法。游戏状态是一个由数字组成的立方数组（灰色
表示 0，蓝色表示 1，绿色表示 - 1），它代表了要完成的剩余工作。
举例而言，如果学校里教的传统算法可以使用 100 次乘法完成 4x5 与 5x5 矩阵相乘，通过人类的聪明才智（Strassen Algorithm）可以
将这一数字降至 80 次。与之相比，AlphaTensor 发现的算法只需使用 76 次乘法即可完成相同的运算，如下图所示。
标准矩阵乘法

数组和广义表 
•数组的定义
•数组的顺序表示
•矩阵的压缩存储（特殊矩阵，稀疏矩阵）
•广义表的定义
•广义表的存储结构
Data Structure & Algorithms60

广义表
•广义表是线性表的推广和扩充，在人工智能领域中应用十分广泛。
•在前面的内容中，我们把线性表定义为n(n≧0 )个元素a
1
, a
2 
,..., a
n
的有穷
序列，该序列中的所有元素具有相同的数据类型且只能是原子项(Atom)。
所谓原子项可以是一个数或一个结构，是指结构上不可再分的。若放
松对元素的这种限制，容许它们具有其自身结构，就产生了广义表的
概念。
•广义表(Lists，又称为列表 )：是由n(n ≧0)个元素组成的有穷序列： 
LS=(a
1
，a
2
，...，a
n
)
•其中a
i
或者是原子项，或者是一个广义表。LS是广义表的名字，n为它
的长度。若a
i
是广义表，则称为LS的子表。
61Data Structure & Algorithms

广义表的定义
•线性表的推广，表中的元素可以是表。
•LS = ( a1, a2, ..., an )
•A = ()
•B = ( e )
•C = ( a, ( b, c, d ) )
•D = ( A, B, C )  
•E = ( a, E )
Data Structure & Algorithms62
习惯上：原子用小写字母，
                  子表用大写字母。

广义表的定义
Data Structure & Algorithms63
广义表的长度：元素的数目。
广义表的表头：非空广义表中第一个元素。
广义表的表尾：除表头元素之外，其余元素构成的表。
广义表的深度：广义表中括号的重数。
A=()                       0                                      1     
B=(e)                     1          e          ()              1
C=(a,(b,c,d))         2          a      ((b,c,d))       2
D=(A,B,C)             3         ()       (B,C)           3
E=(a,E)                  2         a        (E)             无穷大
长度       表头        表尾           深度

广义表的定义
64Data Structure & Algorithms
广  义  表
表长
n表深h
A=()00
B=(e)11
C=(a,(b,c,d))22
D=(A,B,C)33
广义表及其示例
a
b
e
c
d
A
B
C
D
广义表的图形表示

广义表的定义
广义表的重要结论：
•广义表的元素可以是原子，也可以是子表，子表的元素又可以是子
表， ...。即广义表是一个多层次的结构。
•广义表可以被其它广义表所共享，也可以共享其它广义表。广义表共
享其它广义表时通过表名引用。
•广义表本身可以是一个递归表。
•根据对表头、表尾的定义，任何一个非空广义表的表头可以是原子，
也可以是子表， 而表尾必定是广义表。
65Data Structure & Algorithms

广义表的存储结构
•由于广义表中的数据元素具有不同的结构，通常用链式存储结构表示，
每个数据元素用一个结点表示。因此，广义表中就有两类结点：
•一类是表结点，用来表示广义表项，由标志域，表头指针域，表尾指针
域组成;
•另一类是原子结点，用来表示原子项，由标志域，原子的值域组成。
•只要广义表非空，都是由表头和表尾组成，即一个确定的表头和表尾
就唯一确定一个广义表。
Data Structure & Algorithms66
  标志tag=0        原子的值    标志tag=1        表头指针hp       表尾指针tp 
广义表的链表结点结构示意图
(b)  表结点(a)  原子结点

广义表的存储结构
•相应的数据结构定义如下：
typedef struct GLNode
{  int   tag ;     /*  标志域，为1：表结点;为0 ：原子结点  */
union
{  elemtype value;     /* 原子结点的值域  */
struct
    {  struct GLNode  *hp , *tp ;
     } ptr ;   /*  ptr和atom两成员共用  */
} Gdata ; 
} GLNode ;      /* 广义表结点类型  */
67Data Structure & Algorithms

广义表的存储结构
例： 对A=()，B=(e)，C=(a, (b, c, d) )，D=(A, B, C)，E=(a, E)的广
义表的存储结构如图所示。
68Data Structure & Algorithms

广义表的存储结构
•对于上述存储结构，有如下几个特点：
(1) 若广义表为空，表头指针为空；否则，表头指针总是指向一个表
结点，其中hp指向广义表的表头结点(或为原子结点，或为表结点) ，
tp指向广义表的表尾(表尾为空时，指针为空，否则必为表结点)。
(2)  这种结构求广义表的长度、深度、表头、表尾的操作十分方便。
(3)  表结点太多，造成空间浪费。
69Data Structure & Algorithms

Summary
•数组的定义
•数组的顺序表示
•矩阵的压缩存储（特殊矩阵，稀疏矩阵）
•广义表的定义
•广义表的存储结构
Data Structure & Algorithms70
