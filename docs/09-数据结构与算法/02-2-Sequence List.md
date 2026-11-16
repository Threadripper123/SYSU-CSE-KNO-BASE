# 02-2-Sequence List

> 主题库：数据结构与算法　｜　源文件：02-2-Sequence List.pdf　｜　5,187 字符　｜　27 个片段　｜　更新于 2026-11-11


Lists
Yan Pan
School of Computer Science and Engineering
Data Structure & Algorithms
E-mail: panyan5@mail.sysu.edu.cn

Two types of physical structures
（1）Sequence：
ü Storage cell with continuous 
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
2Data Structure & Algorithms

Two types of physical structures
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
（2） Linked：
ü Storage cell
ü Logical relationship is 
described by point
3Data Structure & Algorithms

Overview
•线性表（List）
•Definition and operations
•Implementation
•Applications
SequenceLinked
4Data Structure & Algorithms

Definition
•线性表（List）
•由结点集퐀，以及定义在结点集퐀上的线性关系퐀所组成
的线性结构，这些结点称为线性表的元素。
•零个或多个数据元素的有限序列。
      A list is defined as a sequence of  n(n>0) A
1
, ...，A
n
.
             L = (A
1
, A
2
, ..., A
i-1
, A
i
, A
i+1
, ..., A
n
)
L = (‘a’, ’b’, ..., ’x’, ’y’, ’z’)
L = (‘good’, ’student’, ’work’)
5Data Structure & Algorithms

Mathematical Definition
•用数序语言来描述，即有：
•若将线性表记为(a
1
,..., a
i-1
, a
i
, a
i+1
, ..., a
n
)，则表中a
i-1
领先于a
i 
， 
a
i
先于a
i+1 
，称a
i-1
是a
i
的直接前驱元素， a
i+1
是a
i
的直接后继
元素。
•当i=1, 2, ..., n-1时， a
i
有且仅有一个直接后继；当i=2, 3, ..., n
时， a
i
有且仅有一个直接前驱。
a
1
a
2
a
i-1
a
i
a
i+1
a
n
......
6Data Structure & Algorithms

Characteristics of Lists
•线性表（N , r）
（a）结点集N中有一个唯一的开始结点，它没有前驱，但有一个唯
一的后继；
（b）对于有限集N, 它存在一个唯一的终止结点，该结点有一个唯一
的前驱而没有后继；
（c）其它的结点皆称为内部结点，每一个内部结点既有一个唯一的
前驱，也有一个唯一的后继；
（d）线性表所包含的结点个数称为线性表的长度，它是线性表的一
个重要参数；长度为 0 的线性表称为空表；
7Data Structure & Algorithms

Examples of Lists
8Data Structure & Algorithms
•哪个是列表结构？
系统开始菜单？垃圾邮件清单？搜索结果？

•In a complicated list, a data element may consist of some data 
items.
A List of Data Elements
NameIDGenderAgeHealth State
Peter790631Male18Good
Lena790632Female20General
John790633Male21Good
Tom790634Male17Not Good
.....................................
9Data Structure & Algorithms

Operations
•Insert an item in some position;
•Delete an item in some position;
•Locate some item;
•Check if it is empty;
•Check its size;
•Print all items in the list, etc.
10Data Structure & Algorithms

Implementations
•Sequence List (Array) 
•线性表的顺序存储结构，指的是用一段地址连续的存储
单元依次存储线性表的数据元素
•Linked List
•线性表的链式存储结构，指的是用一组任意的存储单元
存储线性表的数据元素，这组存储单元可以是连续的，
也可以是不连续的
11Data Structure & Algorithms

•Array implementation
Sequence List
LOC(a
i
) = LOC(a
0
) + i x L       1<=i<=n
a
0
a
1
a
i
a
n-1
...
...
a
0
a
1
a
i
a
n-1
...
...
0
1
n-
1
Loc(a
0
)
Loc(a
0
) + L
iLoc(a
0
) + i x L
Loc(a
0
) + (n-1) x 
L
12Data Structure & Algorithms
逻辑上相邻的元素，在物理位置上也相邻。

private:
   int count;
  List_entry entry[max_size];

Insert element e at location i
i = 3, e =‘P’
14Data Structure & Algorithms

Delete element at location i
i = 3 
15Data Structure & Algorithms







Time Complexity for Insert and Delete
•移动次数依赖于表长n和插入/删除的位置
•Insertion：
•最坏：移动次数为n
•最好:  移动次数为0
•平均：等概率情况下，平均移动次数n/2 
•Deletion：
•最坏：移动次数为n-1
•最好:   移动次数为0
•平均：等概率情况下，平均移动次数(n-1)/2
19Data Structure & Algorithms

Pros and Cons
1）优点
•顺序表的结构简单
•顺序表的存储效率高，是紧凑结构，无须为表示节点间的逻辑关系
而增加额外的存储空间
•顺序表是一个直接存取结构（随机存取结构）
2）缺点
•在顺序表中进行插入和删除操作时，需要移动数据元素，算法效率
较低。
•对长度变化较大的线性表，或者要预先分配较大空间或者要经常扩
充线性表，给操作带来不方便。
20Data Structure & Algorithms

Pros and Cons
•随机存取
•能用常量时间访问存储区中的任何元素，这样的存储结构就是可随
机存取结构。如数组结构，只要确定其下标，即可对对应元素进行
访问，无论这个下标在数组的什么位置。
•所谓常量访问时间，即得到数组中任意元素地址的时间是一样的，
时间复杂度为O(1)。
21Data Structure & Algorithms

Exercises
•例子一：设计一个高效的算法，将顺序表的所有元素逆置，要求算法
的空间复杂度为O(1)。
•思路：扫描顺序表L的前半部分元素，对于元素L.data[i] (0≤i<L.length/2)，
将其余后半部分对应元素L.data[L.length-i-1]进行交换。
22Data Structure & Algorithms

Exercises
•例子二：长度为n的顺序表L，编写一个时间复杂度为O(n)，空间复杂度为O(1)
的算法，该算法删除线性表中所有值为 x 的数据元素。
•思路1：用k记录顺序表L中不等于x的元素个数（即需要保存的元素个数），一
边扫描 L 一边统计k，并将不等于x的元素向前放置k位置上，最后修改L的长度。
23Data Structure & Algorithms

Exercises
•例子二：长度为n的顺序表L，编写一个时间复杂度为O(n)，空间复杂度为O(1)
的算法，该算法删除线性表中所有值为 x 的数据元素。
•思路2：用k记录顺序表L中等于x的元素个数，一边扫描L一边统计k，并将不等
于x的元素前移k个位置，最后修改L的长度。
24Data Structure & Algorithms

Exercises
•例子二：长度为n的顺序表L，编写一个时间复杂度为O(n)，空间复杂度为O(1)
的算法，该算法删除线性表中所有值为 x 的数据元素。
•思路3：本例子还可以考虑设头、尾两个指针（i=1, j=n），从两端向中间移动，
凡遇到最左端值x的元素时，直接将最右端值非x的元素左移至值为x的数据元素
位置，直到两指针相遇。
•但这种方法会改变原表中元素的相对位置。
25Data Structure & Algorithms

Exercises
•例子三：从有序顺序表中删除其值在给定值s与t之间（要求s<t）的所有元素，如果s或t不
合理或者顺序表为空，则显示出错信息结束运行。
•注意本例子与上一个例子之间的区别，因为是有序表，所以删除的元素必然是相连的整体。
•思路：先寻找值大于等于s的第一个元素（第一个删除的元素），然后寻找值>t的第一个元
素（最后一个删除的元素的下一个元素），要将这段元素删除，则只需将后面的元素前移。
26Data Structure & Algorithms

Exercises
•例子四：设计一个算法从顺序表中删除重复的元素，并使剩余元素间的相对次序保持不变。
•思路：对于顺序表L，用i从1开始遍历其元素，设L.data[0..j] (j的初值为0)中没有重复的元素。
检测L.data[i] (j<i<L.length)，若L.data[i]和L.data[0..j]中任何一个元素都不相同，则将L.data[i]存
入L.data[j+1]中，如图所示。 
27Data Structure & Algorithms

Exercises
28Data Structure & Algorithms
•例子五：设计一个算法从有序顺序表中删除重复的元素，并使剩余元素间的相对次
序保持不变。
•思路：在有序顺序表L中，所有重复的元素应该是相邻存放的，用k保存不重复出现
的元素个数，先将不重复的有序区看成是L.data[0..0]，置e=L.data[0]，用i从1开始遍
历L的所有元素：当：L.data[i]≠e时，将它放在L.data[k]中，k增1，置e=L.data[i]，最后
将L的length置为k 。对应算法如下： 
•本实现是一个高效算法，其采用建表
的思路，时间复杂度为O(n)，空间复杂
度为O(1)。如果不采用该方案，而是每
次遇到重复的元素，都通过移动其后
所有元素来删除它，这样算法的时间
复杂度会变成O(n
2
)。

Summary
•线性表（List）
•由结点集N，以及定义在结点集N上的线性关系r所组成
的线性结构，这些结点称为线性表的元素。
•零个或多个数据元素的有限序列。
•顺序表（Sequence List） 
•线性表的顺序存取结构，指的是用一段地址连续的存储
单元依次存储线性表的数据元素。
      
29Data Structure & Algorithms
a
1
a
2
a
i-1
a
i
a
i+1
a
n
......
