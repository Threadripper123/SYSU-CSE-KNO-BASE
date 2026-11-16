# 11-1-Search1

> 主题库：数据结构与算法　｜　源文件：11-1-Search1.pdf　｜　12,901 字符　｜　53 个片段　｜　更新于 2026-11-11


Search
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Data and Computer Science
Data Structure & Algorithms

Search (查找，搜索)
•静态查找结构
•动态查找结构
•散列
•可扩充散列
Data Structure & Algorithms2

回顾字符串匹配
•子串在主串中的定位称为模式匹配(Pattern Matching)或串匹配(String 
Matching)。模式匹配成功是指在主串S中能够找到模式串T，否则，称
模式串T在主串S中不存在。
3Data Structure & Algorithms
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

搜索引擎
•通过关键词检索词条
4Data Structure & Algorithms

Search
•数据的组织和查找是大多数应用程序的核心，而查找是所
有数据处理中最基本、最常用的操作。特别当查找的对象
是一个庞大数量的数据集合中的元素时，查找的方法和效
率就显得格外重要。
•主要讨论顺序表、有序表、树和哈希表查找的各种实现方
法，以及相应查找方法在等概率情况下的平均查找长度。
Data Structure & Algorithms5

查找的概念
查找表(Search Table)：相同类型的数据元素(对象)组成的集合，每个元素
通常由若干数据项构成。
关键字(Key，码)：数据元素中某个(或几个)数据项的值，它可以标识一
个数据元素。若关键字能唯一标识一个数据元素，则关键字称为主关键
字；将能标识若干个数据元素的关键字称为次关键字。
查找/检索(Searching)：根据给定的K值，在查找表中确定一个关键字等于
给定值的记录或数据元素。
◆  查找表中存在满足条件的记录：查找成功；结果：所查到的记录信
息或记录在查找表中的位置。
◆ 查找表中不存在满足条件的记录：查找失败。
Data Structure & Algorithms6

查找的概念
查找有两种基本形式：静态查找和动态查找。
      静态查找(Static Search)：在查找时只对数据元素进行查询或检索，查
找表称为静态查找表。
      动态查找(Dynamic Search)：在实施查找的同时，插入查找表中不存在
的记录，或从查找表中删除已存在的某个记录，查找表称为动态查找表。
       查找的对象是查找表，采用何种查找方法，首先取决于查找表的组织。
查找表是记录的集合，而集合中的元素之间是一种完全松散的关系，因
此，查找表是一种非常灵活的数据结构，可以用多种方式来存储。
Data Structure & Algorithms7

查找的概念
•根据存储结构的不同，查找方法可分为三大类：
①  顺序表和链表的查找：将给定的K值与查找表中记录的关键字逐个
进行比较， 找到要查找的记录；
②  散列表的查找：根据给定的K值直接访问查找表， 从而找到要查
找的记录；
③  索引查找表的查找：首先根据索引确定待查找记录所在的块 ，然
后再从块中找到要查找的记录。
Data Structure & Algorithms8

查找方法评价指标
•查找过程中主要操作是关键字的比较，查找过程中关键字的平均比较
次数(平均查找长度ASL：Average Search Length)作为衡量一个查找算法
效率高低的标准。ASL定义为：
Data Structure & Algorithms9
ASL是存储结构中对
象总数n的函数
P
i
为检索第i个元素
的概率
C
i
为找到第i 个元素所
需的关键码值与给定值
的比较次数
1
n
ii
i
ASLPC




查找方法评价指标
•平均检索长度的例子
假设线性表为（a, b, c）检索a、b、c的概率分别为0.4、0.1、0.5
q顺序检索算法的平均检索长度为0.4×1+0.1×2+0.5×3 = 2.1
q即平均需要2.1次给定值与表中关键码值的比较才能找到待查元素
Data Structure & Algorithms10

静态查找--顺序查找(Sequential Search)
查找思想
        从表的一端开始逐个将记录的关键字和给定K值进行比较，若某个记
录的关键字和给定K值相等，查找成功；否则，若扫描完整个表，仍然没
有找到相应的记录，则查找失败。顺序表的类型定义如下：
Data Structure & Algorithms11

Sequential Search
•Algorithm: the simplest way to do search;
•Analysis: How much work need to be done to search a 
key;
•Test: empirical testing on sample data.  
Input: a
1
,a
2
,...,a
n
Output: location of the item if search is successful, null otherwise.
Algorithm:
     for each item a
i
 in the list:
           if that item has the desired value, 
                   stop the search and return the item's location.
     return null. 

Analysis
How to estimate the amount of work that 
a search method based on comparison 
will do?
•The total work is reflected by the 
number of comparisons of keys that the 
algorithm makes. 
•We compute both successful cases and 
unsuccessful cases. 

How many comparisons of keys does 
sequential search make when it is applied to 
a list of n entries?
Assuming that the value being sought occurs once 
in the list, and all orderings of the list are equally 
likely. 

Sequential search
PThe method is simple;
PEfficient for short list;
PThe list don’t have to be ordered;
Disaster for long list to be searched;
There are more efficient methods for ordered lists. 

静态查找--折半查找(Binary Search)
折半查找又称为二分查找，是一种效率较高的查找方法。
前提条件：查找表中的所有记录是按关键字有序(升序或降
序) 。
查找过程中，先确定待查找记录在表中的范围，然后逐步缩
小范围(每次将待查记录所在区间缩小一半)，直到找到或找
不到记录为止。
Data Structure & Algorithms16

静态查找--折半查找(Binary Search)
•查找思想（其中一种实现）
用Low、High和Mid表示待查找区间的下界、上界和中间位置
指针，初值为Low=1，High=n。
(1)   取中间位置Mid：Mid=(Low+High)/2 ；
(2)   比较中间位置记录的关键字与给定的K值：
①  相等： 查找成功；
②  大于：待查记录在区间的前半段，修改上界指针： 
High=Mid-1，转(1) ；
③  小于：待查记录在区间的后半段，修改下界指针：
Low=Mid+1，转(1) ；
直到越界(Low>High)，查找失败。
Data Structure & Algorithms17

静态查找--折半查找(Binary Search)
•关键字18  low=1  high=9 
Data Structure & Algorithms18
35
1 2 3 4 5 6 7 8 9 
 
15   22  51 60 88 93 
low
high
1817
第二次：l=1, , mid=2; array[2]=17<18
第三次：, h=4, mid=3; 

静态查找--折半查找(Binary Search)
Data Structure & Algorithms19
•查找成功示例

静态查找--折半查找(Binary Search)
•查找不成功示例
Data Structure & Algorithms20

静态查找--折半查找(Binary Search)
•算法分析
①  查找时每经过一次比较，查找范围就缩
小一半，该过程可用一棵二叉树表示：
◆ 根结点就是第一次进行比较的中间
位置的记录；
◆ 排在中间位置前面的作为左子树的
结点；
◆ 排在中间位置后面的作为右子树的
结点； 
•对各子树来说都是相同的。这样所得到的
二叉树称为比较树(Comparison Tree)。
Data Structure & Algorithms21

Assumptions
•Assume class Record and class Key;
•Every Record is associated to a key (of 
type Key);
–A key member;
–a conversion operation: operator Key() 
const;
•Key objects can be compared with the 
standard operators == , != , <, >, <= , >= .
•Records can be compared to each other or 
to keys by first converting to their 
associated keys. 
•Define a type Error_code:
enum Error_code{success, fail, not_present};

 Ordered Lists
Ordered lists as an ADT
All the operations on lists apply except insert and 
replace.
We can implement a class of ordered lists by using 
inheritance

Binary Search
Simple? However, 
•The method dates back to 1946, but the first correct 
implementation appeared in 1962;
•Many professional programmers fail to code it correctly.

Making Implementation Correct
To make a correct implementation, we must
§ State exactly what our variables designate;
§ State precisely what conditions must be true 
before and after each iteration of the loop;
§ make sure the loop will terminate properly.
We will
The invariant



The forgetful Version
• Forget the possibility that the key target might be found quickly 
and continue to subdivide the list until what remains has length 1.
• The recursive version takes the range as parameters.



Correctness of the Algorithm
What do we need to verify or prove 
that our algorithm is correct?
1.The target key can be found if it is 
present in the list and it is the first 
occurrence.
2.The algorithm always terminates.



Termination
The recursion continue when top > bottom. So 
we prove that the size of the  interval 
[bottom, top] strictly decreases.
1.When bottom < top, we have  
      bottom <= mid < top
2.  The recursive calls reduce the size of the 
intervals either to 
   top – (mid+1) or mid – bottom
Both of which are strictly less then top – 
bottom.
So the recursion terminates.   

Use of  recursive_binary_1
If we are searching in a whole list, we can 
adjust the above function to  the following 
search function:
Error_code run_recursive_binary_1(const Ordered_list 
&the_list,  const Key &target, int &position)
{
   return recursive_binary_1(the list, target, 
                                        0,  the_list.size() –1, position);
}

Nonrecursive Version
Because both of the two recursions in the function 
recursive_binary_1 are tail recursion, it is easy to 
eliminate them and make an iterative version.



Recursive 
version







Binary search in STL
#include <algorithm> 
 bool binary_search( forward_iterator start, forward_iterator end, const TYPE& 
val ); 
As both random_access_iterator and bidirectional_iterator are refinement of 
forward_iterator, so binary_search can be used to search in both vectors and 
lists.
However, if one wants to get the position of the record, one needs 
lower_bound(upper_bound).
•Notice that binary_search uses the default comparison ‘<‘ to decide if two 
values are equivalent. How about using a relation other than ‘<‘, for example, 
‘>’? 
•Binary_search has another version that allows you provide your own order 
relation. 

Comparison Trees
    The comparison tree of an algorithm is 
obtained by tracing the action of the 
algorithm.
p A vertex (circle) represents a comparison of keys. 
Inside the circle we put the index of the key against 
which we are comparing the target key.
p Branches (lines) drawn down from the circle 
represent the possible outcomes of the comparison. 
p Leaves (squares): When the algorithm terminates, we 
put either F (for failure) or the location where the target 
is found at the end of the appropriate branch.

p The external path length of a tree is the sum of the 
number of branches traversed in going from the root 
once to every leaf in the tree.
p The internal path length is defined to be the sum, over 
all vertices that are not leaves, of the number of 
branches from the root to the vertex.
Comparison Trees

Comparison tree 
for sequential 
search on a list of n 
elements.
 n comparisons to search 
nth element and for 
unsuccessful search 

Comparison tree for binary search— the first version
Five comparisons at most for 
successful and unsuccessful 
searches
The comparison tree for a list of 10 
elements.

Analysing binary search
•Search terminates at leaves, half are successful and half are 
unsuccessful;
•All leaves are on the two adjacent levels;
•Worst case: the number of comparisons is the height of the 
tree;
•Average case?

Comparison Count for binary_search_1
p  The total number of comparisons (for successful 
and unsuccessful) is the external path length of the 
tree, that is, 
          (45)+(64)+(45)+(64) = 88

Comparison Count for binary_search_1
p Half the leaves correspond to successful searches 
and half to unsuccessful searches, so the average 
number of comparisons for both successful and 
unsuccessful searches is
               44/10 = 4.4   (for n = 10) 

Comparison tree for binary search— the second version
pEvery vertex that is not a leaf terminates some successful 
search and the leaves correspond to unsuccessful 
searches.
pOnly one comparison is done at the vertex at which the 
search succeeds in finding the target, other vertex two 
comparison.

Top of the comparison tree, recursive binary_search_2

Comparison Count for binary_search_2
p The total number of successful comparisons  is the 
internal path length of the tree, that is, 

Comparison Count for binary_search_2
p The average number of comparisons  for  a 
successful search to be
p For an unsuccessful search by binary_search_2, we 
need the external path length of the tree. This is
pThe average number of comparisons for an 
unsuccessful search to be

2-trees
A 2-tree is a tree where every vertex except the leaves 
has exactly two children.
Lemma. The number of vertices on each level of a 2-tree 
is at most twice the number on the immediately above. 
Hence, in a 2-tree, the number of vertices on level t is at 
most 2
t
 for t0.
Lemma. If a 2-tree has k vertices on level t, then t  lg k. 

Some Notations
  lg x = log 
2
 x    
  ln x = log 
e 
x
  The floor of x:    x, which is the largest integer less than or 
equal x.
   The ceiling of x: x, which is the smallest integer greater 
than or equal to x.
For integer n, we have
   n/2 +  n/2 = n
   (n-1)/2 <= n/2  <= n/2
  n/2 <= n/2  <= (n+1)/2

Analysis of binary_search_1
Some facts:
§ The last step is a check with the target, hence 
successful and unsuccessful searches terminate 
at leaves, and there are 2n leaves;
§ All the leaves are on the same level or on two 
adjacent levels (exercise);
§ let t be the maximum level, then t = lg 2n, 
this is because
 
 2n<=2
t
 and 2n>2
(t-1)
 for 2-trees,  
hence 
     t >= lg 2n  and t < lg 2n +1;
the number of leaves is less 
than the number of nodes at 
level t and  greater than the 
number of nodes at level t-1

§ The maximum number of key comparisons is     
t = lg 2n, approximately lg n +1;
§The average number of comparisons is about    
lg n +1 (compute the external path length and 
then the average of comparisons);

Lower Bounds
Binary search does far fewer comparisons 
than sequential search. 
•Question: Is there any algorithm that will in 
the worst case and the average case , be able 
to find its target using significant fewer 
comparisons of keys than binary search?
•The answer is NO! As long as the algorithm 
is based on comparisons. 
This can be proven by examine the 
comparison tree.

• The height of our tree will equal the number of 
comparisons that the algorithm does in its worst case;
• The external path length of the tree divided by the 
number of possible outcomes equal to the average 
number of comparisons done by the algorithm. 
an internal node has 
one comparison
Leaves represent 
search results.
A comparison tree based on key comparison

For a 2-tree with n leaves, the minimum value for its height and 
external path length occur when all the leaves are on the same 
level or on two adjacent levels.
r>s+1
E(T’)=E(T)-2r+(r-1)-s+2(s+1)<E(T)

Observations on 2-trees
Lemma. Let T be a 2-tree with k leaves. Then the height h of T 
satisfies h  dlg ke and the external path length E(T) satisfies E(T) 
k lg k. The minimum values for h and E(T) occur when all the 
leaves of T are on the same level or on two adjacent levels.
Lemma. Suppose that an algorithm uses comparisons of keys to 
search for a target in a list. If there are k possible outcomes, then 
the algorithm must make at least lg k comparisons
of keys in its worst case and at least lg k in its average case.
Lemma. binary_search_1 is optimal in the class of all algorithms 
that search an ordered list by making comparisons of keys. In 
both the average and worst cases, binary_search_1 achieves the 
optimal bound.
