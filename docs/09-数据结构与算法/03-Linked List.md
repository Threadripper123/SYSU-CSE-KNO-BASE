# 03-Linked List

> 主题库：数据结构与算法　｜　源文件：03-Linked List.pdf　｜　7,260 字符　｜　38 个片段　｜　更新于 2026-11-11


Linked List
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

Overview
•线性表（List）
•Definition and operations
•Implementation
•Applications
Data Structure & Algorithms2
SequenceLinked

Two types of physical structure
Data Structure & Algorithms3
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

Implementation
•Sequence List (Array) 
•线性表的顺序存储结构，指的是用一段地址连续的存储
单元依次存储线性表的数据元素
•Linked List
•线性表的链式存储结构，指的是用一组任意的存储单元
存储线性表的数据元素，这组存储单元可以是连续的，
也可以是不连续的
Data Structure & Algorithms4

Linked list
Data Structure & Algorithms5
DataPoint
•通过指针把它的一串存储结点链接成一个链 
•存储结点由两部分组成： 
 数据字段 +  指针字段（后继地址） 

Expression of linked list
Data Structure & Algorithms6
带头指针的单链表
带头结点的单链表
空链表

思考
•单链表设置表头结点的目的是什么？
•头结点在链表中并不是必须的，仅仅是为了操作上的方便。
•它具有两方面的优点：
•由于开始结点的位置被存放在头结点的指针域中，所以在链表的第一个位置上
的操作就和在表的其他位置上操作一致，无须进行特殊处理；
•无论链表是否为空，其头指针都是指向头结点的非空指针（空表中头结点的指
针域空），因此空表和非空表的处理也就统一了。
7Data Structure & Algorithms

















Variations of Linked Lists
•Doubly linked lists
–Each node points to not only successor but the 
predecessor
–There are two NULL: at the first and last nodes in 
the list
–Advantage: given a node, it is easy to visit its 
predecessor. Convenient to traverse lists backwards
A
Head
B

C


Double Linked Lists

Double Linked Lists



Double Linked Lists

Double Linked Lists

Double Linked Lists

Why do we use double linked lists?
•Time complexity
–We focus on the function “set_position”
–Single linked list: O(n), on average n/2
–Double linked list: O(n), on average ?
//给定一个有n个节点的双向链表L
List L;
For (k = 1; k<=10000000;k++)
{
    r = int(rand()*n) //产生一个1到n之间的随机整数    
    L.retrieve( r, &entry) //获取第r个节点的值
    ........
}

Why do we use double linked lists?
•Double linked list: O(n), on average n/3
平均情况下的复杂度:设当前位置(current_pos)的值是i，下一步要定位到位
置j（1<=I,j<=n）
（1） avg=
1
퐀∗퐀
 
퐀=1
퐀
 
퐀=1
퐀
|퐀−퐀|
（2） avg=
2
퐀∗퐀
 
퐀=1
퐀
퐀∗(퐀−퐀)
= 
2
퐀∗퐀
(n*  
퐀=1
퐀
퐀
 -   
퐀=1
퐀
퐀
*i)
利用1+2+...+n = n(n+1)/2
以及1^2+2^2+......+n^2 
= n(n+1)(2n+1)/6
化简后可得avg = n/3 +O(1)
I   \  j1234......n
10123n-1
21012n-2
32101n-3
43210n-4
...
nn-1n-2n-3n-40

Variations of Linked Lists
•Circular linked lists
–The last node points to the first node of the list
–How do we know when we have finished traversing 
the list? (Tip: check if the pointer of the current 
node is equal to the head.)
A
Head
BC

Circular Linked List 
•用循环链表求解约瑟夫问题
n约瑟夫问题的提法
   n 个人围成一个圆圈，首先第 1 个人从 1 开始一个
人一个人顺时针报数,  报到第 m 个人，令其出列。
然后再从下一 个人开始，从 1 顺时针报数，报到
第 m 个人，再令其出列，...，如此下去,  直到圆圈
中只剩一个人为止。此人即为优胜者。
26Data Structure & Algorithms

Insert at doubly circular Linked list
Data Structure & Algorithms
27
p→prior = current;             (1)
p→next = current→next;   (2)
current→next = p;              (3)
p→next→prior = p;            (4)
P

Delete at doubly circular Linked list
Data Structure & Algorithms
28
current→next→prior = current→prior;        
current→prior→next = current→next;

Array versus Linked Lists
•Linked lists are more complex to code and manage 
than arrays, but they have some distinct advantages.
–Dynamic: a linked list can easily grow and shrink in size.
•We don’t need to know how many nodes will be in the list. 
They are created in memory as needed.
•In contrast, the size of a C++ array is fixed at compilation 
time.
•Vector can grow automatically, but expensive.
–Easy and fast insertions and deletions
•To insert or delete an element in an array, we need to 
copy to temporary variables to make room for new 
elements or close the gap caused by deleted elements.
•With a linked list, no need to move other nodes. Only 
need to reset some pointers.

Merge two ordered linked lists
•设有两个有序的单链表，它们的头指针分别是La和Lb，将
它们合并为以Lc为头指针的有序链表。合并前的示意图如
图所示。
30Data Structure & Algorithms

Merge two ordered linked lists
•合并了值为-7，-2的结点后示意图如图2所示。
•算法中pa ，pb分别是待考察的两个链表的当前结点，pc是
合并过程中合并的链表的最后一个结点。
31Data Structure & Algorithms

Merge two ordered linked lists
•算法描述
32Data Structure & Algorithms
LNode  *Merge_LinkList(LNode *La， LNode *Lb)
      /*  合并以La, Lb为头结点的两个有序单链表   */
{    LNode *Lc,  *pa ,  *pb ,  *pc, *ptr ;
Lc=La ;  pc=La  ;    pa=La->next ;  pb=Lb->next  ;
 while (pa!=NULL&& pb!=NULL)
 {  if  (pa->data < pb->data)
    {   pc->next=pa ;  pc=pa ;   pa=pa->next  ;   }
/*  将pa所指的结点合并，pa指向下一个结点  */
if  (pa->data > pb->data)
    {   pc->next=pb ;  pc=pb ;   pb=pb->next  ;   }
/*  将pa所指的结点合并，pa指向下一个结点  */

Merge two ordered linked lists
33Data Structure & Algorithms
if  (pa->data == pb->data)
    {   pc->next=pa ;  pc=pa ;   pa=pa->next  ; 
         ptr=pb ; pb=pb->next ; free(ptr) ;   }
/*  将pa所指的结点合并，pb所指结点删除  */
}
 if  (pa!=NULL)  pc->next=pa ;
else   pc->next=pb ;     /*将剩余的结点链上*/
free(Lb) ;
return(Lc) ;
}
算法分析
        若La ，Lb两个链表的长度分别是m，n，则链表合并的时间复杂度
为O(m+n) 。

Time Complexity for Operations in Linked Lists
•对一个结点操作，必先找到它，即用一个指针指向它
•找单链表中任一结点，都必须从第一个点开始： 
Data Structure & Algorithms34
o单链表的时间复杂度：O(n)
n定位： O(n)
n插入： O(n) + O(1)
n删除： O(n) + O(1)
n合并： O(n+m)
p = head;
while (没有到达)
     p = p->next;

Sequence List vs. Linked List
Data Structure & Algorithms35
•程序设计语言方面的因素：在不支持指针的概念和指针类型的程序设计语言中，可以采
用静态链表（即利用游标实现）的方法来模拟动态存储结构。对于问题规模不大的问题，
采用静态链表来实现更加方便。

Sequence List vs. Linked List
•应用场合的选择 
•顺序表不适用的场合
•经常插入删除时，不宜使用顺序表  
•线性表的最大长度也是一个重要因素 
•链表不适用的场合 
•当读操作比插入删除操作频率大时，不应选择链表
•当指针的存储开销，和整个结点内容所占空间相比其比例较大时，
应该慎重选择
Data Structure & Algorithms36

Exercises
•选择题：在一个单链表中，已知 q 是 p 的前驱结点，若在 q 和 p 之间插入
结点s，则执行（   ）
•A. s->next = p->next;  p->next = s;
•B. p->next = s->next;  s->next = p;
•C. q->next = s;  s->next = p;
•D. p->next = s;  s->next = q;
•填空题：在非空双向循环链表中，在结点 q 的前面插入结点 p 的过程如下：
•p->prior =  q->prior; 
•q->prior->next = p;
•p->next = q;
•__（补上代码）__
37Data Structure & Algorithms
C
q->prior = p;

Exercises
•例子：设计一个算法删除单链表L中第一个值为x的节点。
•思路：如图所示，用pre, p遍历整个单链表，pre指向*p的前驱结点，p用于查找
第一个值为x的结点。当找到后将*p结点删除，返回1；否则返回0。对应算法
如下。
38Data Structure & Algorithms

Exercises
•例子：设计一个算法，删除单链表L含两个或两个以上的数据结点中，第一个值为x的结点
的前驱结点。
•思路：如图所示，在遍历时用p指向值为x的结点，用pre指向*p结点的前驱结点，用prepre
指向*pre节点的前驱结点。若第一个数据结点的值为x或未找到值为x的结点，返回0；否则
删除*pre结点，并返回1。对应的算法实现如下：
39Data Structure & Algorithms
查找
删除

Exercises
•例子：在带头结点的单链表中，删除所有值为x的结点，并释放其空间。假设值为x的结
点不唯一，试编写算法实现以上的操作。
•思路1：用p从头到尾扫描单链表，pre指向*p结点的前驱。若p所指结点的值为x，则删除，
并让p移向下一个结点，否则让pre, p指针同步后移一个结点。
40Data Structure & Algorithms
本算法是在无序单链表中删除满足某种条件的
所有结点，这里的条件是结点的值为x。
实际上，这个条件是可以任意指定的，只要修
改if条件即可，比如，要求删除介于mink和
maxk之间的所有结点，则只需将if语句修改为：
if(p->data > mink && p->data < maxk)。

Exercises
•例子：在带头结点的单链表中，删除所有值为x的结点，并释放其空间。假设值为x的结
点不唯一，试编写算法实现以上的操作。
•思路2：采用尾插法建立单链表。用p指针扫描L的所有结点 ，当其值不为x时，将其链接
到L之后，否则将其释放。
41Data Structure & Algorithms
两种算法皆扫描一遍链表，
时间复杂度为O(n)
空间复杂度为O(1)

Exercises
•例子：试编写算法将带有头结点的单链表就地逆置，所谓“就地”是指辅助空间
复杂度为O(1)。
•思路1：将头结点摘下，然后从第一结点开始，依次前插入到头结点的后面（头
插法建立单链表），直到最后一个结点为止，则实现了链表的逆置，如图所示。
42Data Structure & Algorithms

Exercises
•例子：试编写算法将带有头结点的单链表就地逆置，所谓“就地”是指辅助空间复杂度为O(1)。
•思路2：假设 pre, p 和 r 指向3个相邻的结点，如图所示。
•假设经过若干操作，*pre之前的结点的指针都已经调整完毕，它们的next都指向其原前驱结点。现在
令*p结点的next域指向*pre结点，注意到一旦调整指针的指向后，*p的后继结点的链接就断开了，为
此需要用r来指向原*p的后继结点。
•处理时需要注意两点：
•一是在处理第一个结点时，应将其next域置为NULL，而不是指向头结点（因为它将作为新表的尾结点）；
•二是在处理完最后一个结点后，需要将头结点的指针指向它。
43Data Structure & Algorithms

Exercises
•例子：试编写算法将带有头结点的单链表就地逆置，所谓“就地”是指辅助空
间复杂度为O(1)。
•上述两个算法思路的时间复杂度为O(n)，空间复杂度为O(1)。
44Data Structure & Algorithms
