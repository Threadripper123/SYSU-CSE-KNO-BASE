# 12-Sorting

> 主题库：数据结构与算法　｜　源文件：12-Sorting.pdf　｜　13,781 字符　｜　56 个片段　｜　更新于 2026-11-11


Sorting
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Data and Computer Science
Data Structure & Algorithms

什么是排序
•在信息处理过程中，最基本的操作是查找。从查找来说，效率高的方
法之一的是折半查找，折半查找的前提是所有的数据元素(记录)是按关
键字有序的。
•将任一文件中的记录通过某种方法整理成为按(记录)关键字有序排列的
处理过程称为排序。
•排序是数据处理中一种最常用的操作。
2Data Structure & Algorithms

排序的基本概念
•排序(Sorting) ：是将一批(组)任意次序的记录重新排列成按关键字有序的记录序列的过程。
•给定一组记录序列：{R
1
 , R
2 
,..., R
n
}，其相应的关键字序列是{K
1
 , K
2 
,..., K
n
} 。确定1, 2, ... n的一
个排列p
1
 , p
2 
,..., p
n
，使其相应的关键字满足如下非递减(或非递增)关系： K
p
1
≤K
p
2 
≤...≤K
p
n
的序列
{K
p
1 
,K
p
2 
, ...,K
p
n
} 。
•数据表(datalist)：它是待排序数据对象的有限集合。
•关键字(key)：通常数据对象有多个属性域，即多个数据成员组成，其中有一个属性域可用
来区分对象，作为排序依据。该域即为关键字。每个数据表用哪个属性域作为关键字，要
视具体的应用需要而定。即使是同一个表，在解决不同问题的场合也可能取不同的域做关
键字。
3Data Structure & Algorithms

排序的基本概念
•排序算法的稳定性 (Stability)：如果在对象序列中有两个对
象r
[i]
和r
[j]
，它们的关键字 k
[i] 
== k
[j]
，且在排序之前，对象
r
[i]
排在r
[j]
前面。如果在排序之后，对象r
[i]
仍在对象r
[j]
的前
面，则称这个排序方法是稳定的，否则称这个排序方法是
不稳定的
4Data Structure & Algorithms
12, 4, 22, 9, 12*, 33, 2
2, 4, 9, 12, 12*, 22, 33
2, 4, 9, 12*, 12, 22, 33

排序的基本概念
•内排序与外排序:   内排序是指在排序期间数据对象全部存放在内存的排序；外排序是指
在排序期间全部对象个数太多，不能同时存放在内存，必须根据排序过程的要求，不断
在内、外存之间移动的排序。
•排序的时间开销:   排序的时间开销是衡量算法好坏的最重要的标志。排序的时间开销可
用算法执行中的数据比较次数与数据移动（即记录的赋值）次数来衡量。算法运行时间
代价的估算一般都按平均情况进行估算。对于那些受对象关键字序列初始排列及对象个
数影响较大的，需要按最好情况和最坏情况进行估算。
•衡量排序方法的标准
•排序时所需要的平均比较次数
•排序时所需要的平均移动次数
•排序时所需要的平均辅助存储空间
•排序的稳定性
5Data Structure & Algorithms

排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
6Data Structure & Algorithms





插入排序 (Insertion Sort)
•插入排序的基本方法是：每步将一个待排序的对象，按其关键字大小，
插入到前面已经排好序的一组对象的适当位置上，直到对象全部插入
为止。
•基本思想
•当插入第i (i  1) 个对象时，前面的V[0], V[1], ..., v[i-1]已经排好序。
这时，用v[i]的关键字与v[i-1], v[i-2], ...的关键字顺序进行比较，找到
插入位置即将v[i]插入，原来位置上之后的所有对象依次向后顺移。
9Data Structure & Algorithms











































直接插入排序--算法分析
•直接插入排序是一种稳定的排序方法。
31Data Structure & Algorithms

排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
32Data Structure & Algorithms

选择排序(Selection Sort)
• 选择排序的基本思想是：每一趟 (例如第 i 趟，i = 1, ..., n-1) 在后面的 
n-i+1 个待排序对象中选出关键字最小的对象, 作为有序对象序列的第 i 
个对象。待到第 n-1 趟作完，待排序对象只剩下1个，就不用再选了。
•基本步骤为：i从1开始，直到n-1，进行n-1趟排序，第 i 趟的排序过程
为： 在一组对象r[i]～r[n] (i=1,2,...,n-1)中选择具有最小关键字的对象；
并和第 i 个对象进行交换。
33Data Structure & Algorithms











排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
39Data Structure & Algorithms

希尔排序 (Shell Sort)
•希尔排序按其设计者希尔（Donald Shell）的名字命名，该算法于
1959年公布。
•希尔排序方法又称为“缩小增量”排序。
•基本思想
        先将整个待排对象序列按照一定间隔分割成为若干子序列，分
别进行直接插入排序，然后缩小间隔，对整个对象序列重复以上
的划分子序列和分别排序工作，直到最后间隔为1，此时整个对象
序列已 “基本有序”，进行最后一次直接插入排序。
40Data Structure & Algorithms











希尔排序 (Shell Sort)
•算法分析
l对特定的待排序对象序列，可以准确地估算关键字的比较次数和对象
移动次数。
l但想要弄清关键字比较次数和对象移动次数与增量选择之间的依赖关
系，并给出完整的数学分析，还没有人能够做到。
lgap的取法有多种。最初 shell 提出取 gap = n/2，gap = gap/2，直到
gap = 1。后来Knuth 提出取gap = gap/3 +1。还有人提出都取奇数为好，
也有人提出各gap互质为好。
46Data Structure & Algorithms

希尔排序 (Shell Sort)
l步长的选择是希尔排序的重要部分，理论上只要最终步长为1任何步长序列都
可以工作。
lDonald Shell 最初建议步长选择为n/2并且对步长取半直到步长达到1。
l目前已知的最好步长序列由Marcin Ciura设计（1，4，10，23，57，132，301，
701，1750，...），用这样步长序列的希尔排序比插入排序和堆排序都要快，
甚至在小数组中比快速排序还快；但是在涉及大量数据时希尔排序还是比快速
排序慢。
lDonald Knuth利用大量的实验统计资料得出，当 n 很大时，关键字平均比较次
数和对象平均移动次数大约在 n
1.25 
到 1.6n
1.25 
的范围内。这是在利用直接插入排
序作为子序列排序方法的情况下得到的。
47Data Structure & Algorithms

排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
48Data Structure & Algorithms

冒泡排序( Buble Sort )
•冒泡排序是一种交换排序，交换排序的基本思想是两两比较
待排序对象的关键字，如果发生逆序(即排列顺序与排序后
的次序正好相反)，则交换之，直到所有对象都排好序为止。
•冒泡排序的基本方法是：设待排序对象序列中的对象个数为 
n。最多作 n-1 趟排序。在第 i 趟中顺次两两比较r[j-1].Key和
r[j].Key，j = i, i+1, ,  n-i-1。如果发生逆序，则交换r[j-1]和
r[j]。
49Data Structure & Algorithms

冒泡排序 (Bubble Sort)
50Data Structure & Algorithms
第一趟

冒泡排序 (Bubble Sort)
51Data Structure & Algorithms
第二趟
第三趟

冒泡排序 (Bubble Sort)
52Data Structure & Algorithms
第四趟
第五趟

冒泡排序 (Bubble Sort)
•算法实现
53Data Structure & Algorithms
void 
void 
void 
BubbleSort
BubbleSort
BubbleSort
(
(
(
SqList
SqList
SqList
 &L)
 &L)
 &L)
{for (
{for (
{for (
int
int
int
 i=L.length, change=
 i=L.length, change=
 i=L.length, change=
TRUE;i
TRUE;i
TRUE;i
>1 && change; --i)
>1 && change; --i)
>1 && change; --i)
      {
      {
      {
change = FALSE;
change = FALSE;
change = FALSE;
for (
for (
for (
int
int
int
 j=1; j<i; ++j)
 j=1; j<i; ++j)
 j=1; j<i; ++j)
   if (LT(
   if (LT(
   if (LT(
L.r
L.r
L.r
[j+1].
[j+1].
[j+1].
key,L.r
key,L.r
key,L.r
[j].key))
[j].key))
[j].key))
                 {       
                 {       
                 {       
ElemType
ElemType
ElemType
 temp=
 temp=
 temp=
L.r
L.r
L.r
[j];
[j];
[j];
            
            
            
L.r
L.r
L.r
[j]=
[j]=
[j]=
L.r
L.r
L.r
[j+1];
[j+1];
[j+1];
            
            
            
L.r
L.r
L.r
[j+1]=temp;
[j+1]=temp;
[j+1]=temp;
            change = TRUE;
            change = TRUE;
            change = TRUE;
    }
    }
    }
      }
      }
      }
}
}
}
•特点
至少比较1次，至多比较n-1次；
一次确定位置；
可实现部分排序
稳定排序

冒泡排序 (Bubble Sort)
n在对象的初始排列已经按关键字从小到大排好序时，此算法只
执行一趟冒泡，做 n-1 次关键字比较，不移动对象。这是最好
的情形。
n最坏的情形是算法执行了n-1趟冒泡，第 i 趟 (1 i n) 做了 n- i 
次关键字比较，执行了n-i 次对象交换。这样在最坏情形下总的
关键字比较次数和对象移动次数为：
54Data Structure & Algorithms








1
1
1
1
1
2
3
3
1
2
1
n
i
n
i
nninRMN
nninKCN
)()(
)()(

排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
55Data Structure & Algorithms





































































































































排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
122Data Structure & Algorithms

Trees, Lists, and Heaps
Chapter 8  Sorting
If the root of the tree is in position 0 of the list, the left 
and right children of the node in position k are in positions 
2k+1 and 2k+2 of the list, respectively. If these positions are 
beyond the end of the list, then these children do not exist.

Trees, Lists, and Heaps
Definition A heap is a list in which each entry contains a 
key, and, for all positions k in the list, the key at position k is 
at least as large as the keys in positions 2k and 2k+1, 
provided these positions exist in the list.
Remark Many C++ manuals and textbooks refer to the 
area used for dynamic memory as the heap; this use of the 
word .heap. has nothing to do with the present definition.
Heapsort is suitable only for contiguous lists.
Chapter 8  Sorting



Examples of Heaps
126Data Structure & Algorithms
09
09
87
87
78
7845
45
65
6531
3153
23
23
53
17
17
  &&   &&  







Trees, Lists, and Heaps
Chapter 8  Sorting

Trees, Lists, and Heaps
Chapter 8  Sorting





















Analysis of Heapsort
In its worst case for sorting a list of length n, heapsort 
performs 2nlgn + O(n) comparisons of keys, and it performs 
nlgn + O(n) assignments of entries.
Chapter 8  Sorting



思考题
•给定一个有N个entry的list，设计一个算法，
从N个entry中找出最大的k个。假定N很大且
k很小（例如N=100亿，k=30）。

Priority Queues
Definition A priority queue consists of entries, such that 
each entry contains a key called the priority of the entry. A 
priority queue has only two operations other than the usual 
creation, clearing, size, full, and empty operations:
1) Insert an entry
2) Remove the entry having the largest (or smallest) key.
If entries have equal keys, then any entry with the largest 
key may be removed first.
Chapter 8  Sorting

堆排序 (Heap Sort)
堆 vs. 二叉排序树
•从逻辑的角度来讲，堆是一种树形结构，而且是一种特殊的完全二叉
树。此完全二叉树的每个结点对应于序列中的一个关键码，根结点对
应于关键码K
0
，按层次从左至右依次类推。
•说其特殊，主要是因为堆序只是局部有序，即最小堆对应的完全二叉
树中所有内部结点的值均不大于其左右孩子的关键码值，而一个结点
与其兄弟之间没有必然的联系。
•最小堆不像二叉排序树那样实现了关键码的完全排序。相比较而言，
只有当结点之间是父子关系的时候，才可以确定这两个结点关键码的
大小关系。
146Data Structure & Algorithms

排序方法
•Insertion Sort (插入排序)
•Selection Sort (简单选择排序)
•Shell Sort（希尔排序）
•Bubble Sort (冒泡排序)
•Divide-and-Conquer Sort (归并排序、快速排序)
•Heap Sort（堆排序）
•Radix Sort (基数排序)
147Data Structure & Algorithms

Lower Bound for Sorting
•Mergesort and heapsort
–worst-case running time is O(N log N)
•Are there better algorithms?
•Goal: Prove that any sorting algorithm based 
on only comparisons takes (N log N) 
comparisons in the worst case (worse-case 
input) to sort N elements. 

Lower Bound for Sorting
•Suppose we want to sort N distinct elements
•How many possible orderings do we have for 
N elements?
•We can have N! possible orderings (e.g., the 
sorted output for  a,b,c  can be  a b c,  b a c,  a 
c b,  c a b,  c b a,  b c a.)

Lower Bound for Sorting
•Any comparison-based sorting process can be 
represented as a binary decision tree.
–Each node represents a set of possible orderings, 
consistent with all the comparisons that have 
been made
–The tree edges are results of the comparisons

Decision tree for
Algorithm X for sorting
three elements a, b, c

Lower Bound for Sorting
•A different algorithm would have a different decision tree
•Decision tree for Insertion Sort on 3 elements:
There exists an input ordering that corresponds to each root-to-leaf path to arrive at a sorted 
order.  For decision tree of insertion sort, the longest path is O(N
2
).

Lower Bound for Sorting
•The worst-case number of comparisons used by the sorting 
algorithm is equal to the depth of the deepest leaf
–The average number of comparisons used is equal to the average 
depth of the leaves
•A decision tree to sort N elements must have N! leaves
–a binary tree of depth d has at most 2
d
 leaves
     a binary tree with 2
d
 leaves must have depth at least d
     the decision tree with N! leaves must have depth at least log
2
 (N!)
•Therefore, any sorting algorithm based on only comparisons 
between elements requires at least
     log
2
(N!)  comparisons in the worst case.

Lower Bound for Sorting
•Any sorting algorithm based on comparisons 
between elements requires (N log N) 
comparisons.

Linear time sorting
•Can we  do better (linear time algorithm) if the 
input has special structure (e.g., uniformly 
distributed, every number can be represented 
by d digits)? Yes.
•radix sort

基数排序 (Radix Sort)
•基数排序(Radix Sorting) 又称为桶排序或数字排序：按待排
序记录的关键字的组成成分(或“位”)进行排序。
•基数排序和前面的各种内部排序方法完全不同，不需要进
行关键字的比较和记录的移动。
•基数排序是采用“分配”与“收集”的办法，用对多关键
字进行排序的思想实现对单关键字进行排序的方法。
156Data Structure & Algorithms

基数排序 (Radix Sort)
•多关键字排序
•设有n个记录{R
1
, R
2
, ...,R
n
}， 每个记录R
i
的关键字是由若干项(数据项)
组成，即记录R
i
的关键字Key是若干项的集合： {K
i
1
, K
i
2
, ...,K
i
d
}(d>1) 。
•记录{R
1
, R
2
, ...,R
n
}有序的，指的是i, j∈[1,n]，i<j ，若记录的关键字
满足：
{K
i
1
, K
i
2
, ...K
i
d
}<{K
j
1
, K
j
2
, ...K
j
d
}，
即K
i
p 
≤K
j
p 
(p=1, 2, ... d)        
157Data Structure & Algorithms

基数排序 (Radix Sort)
•多关键字排序思想
•先按第一个关键字K
1
进行排序，将记录序列分成若干个子序列，每个
子序列有相同的K
1
值；然后分别对每个子序列按第二个关键字K
2
进行
排序，每个子序列又被分成若干个更小的子序列；如此重复，直到按
最后一个关键字K
d
进行排序。
•最后，将所有的子序列依次联接成一个有序的记录序列，该方法称为
最高位优先(Most Significant Digit first)。
•另一种方法正好相反，排序的顺序是从最低位开始，称为最低位优先
(Least Significant Digit first)。
158Data Structure & Algorithms

基数排序 (Radix Sort)
•链式基数排序
•若记录的关键字由若干确定的部分(又称为 “位”)组成，每一位(部分)
都有确定数目的取值。对这样的记录序列排序的有效方法是基数排序。
•设有n个待排序记录{R
1
, R
2
, ...,R
n
}， (单)关键字是由d位(部分)组成，每
位有r种取值，则关键字R[i].key可以看成一个d元组： R[i].key={K
i
1
, 
K
i
2
, ...,K
i
d
} 。
•基数排序可以采用前面介绍的MSD或LSD方法。
•以下以LSD方法讨论链式基数排序。
159Data Structure & Algorithms

基数排序 (Radix Sort)
•排序思想
160Data Structure & Algorithms
(1)  首先以静态链表存储n个待排序记录，头结点指针指向第一个记录结点； 
(2)  一趟排序的过程是：
①  分配： 按K
d
值的升序顺序，改变记录指针，将链表中的记录结点分
配到r个链表(桶)中，每个链表中所有记录的关键字的最低位(K
d
)的
值都相等，用f[i]、e[i]作为第i个链表的头结点和尾结点；
②  收集：改变所有非空链表的尾结点指针，使其指向下一个非空连表
的第一个结点，从而将r个链表中的记录重新链接成一个链表；
(3)  如此依次按K
d-1
, K
d-2
, ... K
1
分别进行，共进行d趟排序后排序完成。

基数排序 (Radix Sort)
•设有关键字序列为1039, 2121, 3355, 4382, 66, 118的一组记录，采用链式
基数排序的过程如下图所示。
161Data Structure & Algorithms
以LSD方法进行链式基数排序的过程
......

基数排序 (Radix Sort)
162Data Structure & Algorithms
以LSD方法进行链式基数排序的过程
......

基数排序 (Radix Sort)
163Data Structure & Algorithms
以LSD方法进行链式基数排序的过程
...
...

基数排序 (Radix Sort)
164Data Structure & Algorithms
以LSD方法进行链式基数排序的过程
.
.
.
.
.
.

基数排序 (Radix Sort)
•算法分析
165Data Structure & Algorithms
       设有n个待排序记录，关键字位数为 d，每位有 r 种取值。则排序的
趟数是 d；在每一趟中：
◆ 链表初始化的时间复杂度：O(r) ； 
◆ 分配的时间复杂度：O(n) ； 
◆ 分配后收集的时间复杂度：O(r) ； 
        则链式基数排序的时间复杂度为： O(d(n+r))
        在排序过程中使用的辅助空间是：2r 个链表指针， n 个指针域空间，
则空间复杂度为：O(n+r)
        基数排序是稳定的。

各种内部排序的比较
•各种内部排序方法的性能比较如下表
166Data Structure & Algorithms

各种内部排序的比较
•选取排序方法的主要考虑因素
167Data Structure & Algorithms
◆ 待排序的记录数目 n；
◆ 每个记录的大小；
◆ 关键字的结构及其初始状态；
◆ 是否要求排序的稳定性；
◆ 语言工具的特性；
◆ 存储结构的初始条件和要求； 
◆ 时间复杂度、空间复杂度和开发工作的复杂程度的平衡点等。

Exercises
•已知待排序记录（70, 83, 100, 65, 10, 32, 7, 9），分别用插入排序，希尔
排序，冒泡排序和快速排序算法进行排序，写出各趟排序结果。
168Data Structure & Algorithms
初始   (70), 83, 100, 65, 10, 32, 7, 9
      1    (70, 83), 100, 65, 10, 32, 7, 9
      2    (70, 83, 100), 65, 10, 32, 7, 9
      3    (65, 70, 83, 100), 10, 32, 7, 9
      4    (10, 65, 70, 83, 100), 32, 7, 9
      5    (10, 32, 65, 70, 83, 100), 7, 9
      6    (7, 10, 32, 65, 70, 83, 100), 9
      7    (7, 9, 10, 32, 65, 70, 83, 100)
初始     70, 83, 100, 65, 10, 32, 7, 9
1 d=4    10, 32, 7, 9, 70, 83, 100, 65
2 d=2    7, 9, 10, 32, 70, 65, 100, 83
3 d=1    7, 9, 10, 32, 65, 70, 83, 100
插入排序
希尔排序
初始    70, 83, 100, 65, 10, 32, 7, 9
      1     70, 83, 65, 10, 32, 7, 9, 100
      2     70, 65, 10, 32, 7, 9, 83, 100
      3     65, 10, 32, 7, 9, 70, 83, 100
      4     10, 32, 7, 9, 65, 70, 83, 100
      5     10, 7, 9, 32, 65, 70, 83, 100
      6     7, 9, 10, 32, 65, 70, 83, 100
初始    (70), 83, 100, 65, 10, 32, 7, 9
      1     (9, 7, 32, 65, 10), 70, (100, 83)
      2     (7), 9, (32, 65, 10), 70, 83, 100
      3      7, 9, 10, 32, 65, 70, 83, 100 
冒泡排序
快速排序
