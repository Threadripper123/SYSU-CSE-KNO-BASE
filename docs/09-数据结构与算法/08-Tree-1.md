# 08-Tree-1

> 主题库：数据结构与算法　｜　源文件：08-Tree-1.pdf　｜　11,449 字符　｜　45 个片段　｜　更新于 2026-11-11


Trees  & Binary Trees
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn
School of Computer Science and Engineering
Data Structure & Algorithms

Trees
•TreeSpecifications and terms
•Binary trees
•Traversal of binary trees
•Application: binary trees
Data Structure & Algorithms2

线性结构(Linear structure)
•线性结构：线性结构是一个有序数据元素的集合。
•顺序表
•链表
•栈
•队列
•字符串
•数组
3Data Structure & Algorithms
特征：
1．集合中必存在唯一的一个"第一个元素"；
2．集合中必存在唯一的一个"最后的元素"；
3．除最后元素之外，其它数据元素均有唯一的"后继"；
4．除第一元素之外，其它数据元素均有唯一的"前驱"。
数据结构中线性结构指的是数据元素之间存在着“一对一”
的线性关系的数据结构。
如（a1,a2,a3,.....,an），a1为第一个元素，an为最后一个元
素，此集合即为一个线性结构的集合。

非线性结构(Non-linear structure)
•所谓非线性结构是指，在该结构中至少存在一个数据元素，有两个或
两个以上的直接前驱（或直接后继）元素。相对应于线性结构，非线
性结构的逻辑特征是一个结点元素可能对应多个直接前驱和多个后继。
•树型结构和图型结构就是其中十分重要的非线性结构，可以用来描述
客观世界中广泛存在的层次结构和网状结构的关系，如家族谱、城市
交通等。
•树在计算机领域中也有着广泛的应用，
•在编译程序中，用树来表示源程序的语法结构；
•在数据库系统中，可用树来组织信息；
•在分析算法的行为时，可用树来描述其执行过程等等。
Data Structure & Algorithms4

Trees
Data Structure & Algorithms5
Binary Tree
(二叉树)
Tree
(树)
年龄大
年龄小年龄大
年龄小

Definition
树(Tree)是n(n≧0)个结点的有限集合T，若n=0时称为空树(Empty)，否则：
(1)有且只有一个特殊的称为树的根(Root)结点；
(2)若n>1时，其余的结点被分为m(m>0)个互不相交的子集T
1
, T
2
, T
3
...T
m
，
其中每个子集本身又是一棵树，称其为根的子树(Subtree)。
这是树的递归定义，即用树来定义树，而只有一个结点的树必定仅由根组成，
如图所示。
6Data Structure & Algorithms
A
只有根结点

Concepts of trees
•结点(node)：一个数据元素及其若干指向其子树的分支。
•结点的度(degree)：结点所拥有的子树的个数称为该结点的度。
•树的度：树中各结点度的最大值称为该树的度，下图中为3。
Data Structure & Algorithms7
A
只有根结点

Concepts of trees
•叶子(left)结点：树中度为0的结点称为叶子结点(或终端结点)。
•非叶子结点：度不为0的结点称为非叶子结点(或非终端结点或分支结
点)。
•如图中结点F、G、I、J、K、L、M是叶子结点，而所有其它结点都是
非叶子结点。
8Data Structure & Algorithms
A
只有根结点

Concepts of trees
孩子结点、双亲结点、兄弟结点
•一个结点的子树的根称为该结点的孩子结点(child)或子结点；相应地，该结点
是其孩子结点的双亲结点(parent)或父结点。
•如图中结点B 、C、D是结点A的子结点，而结点A是结点B 、C、D的父结点；类似
地结点E 、F是结点B的子结点，结点B是结点E 、F的父结点。
•同一双亲结点的所有子结点互称为兄弟结点。
•如图中结点B 、C、D是兄弟结点；结点E 、F是兄弟结点。
9Data Structure & Algorithms

Concepts of trees
层次、堂兄弟结点
•规定树中根结点的层次为1，其余结点的层次等于其双亲结点的层次加1。
•若某结点在第l (l≧1) 层，则其子结点在第l+1 层。
•双亲结点在同一层上的所有结点互称为堂兄弟结点(Cousins)。如图中结点E、F、
G、H、I、J。
10Data Structure & Algorithms

Concepts of trees
结点的层次路径、祖先、子孙
•从根结点开始，到达某结点p所经过的所有结点成为结点p的层次路径
(有且只有一条)。
•结点p的层次路径上的所有结点（p除外）称为p的祖先(ancester) 。
•以某一结点为根的子树中的任意结点称为该结点的子孙结点(descent)。
•树的深度(depth)：树中结点的最大层次值，又称为树的高度，如图中
树的高度为4。
•有序树和无序树：对于一棵树，若其中每一个结点的子树（若有）具
有一定的次序，则该树称为有序树，否则称为无序树。
•森林(forest)：是m(m≧0)棵互不相交的树的集合。显然，若将一棵树
的根结点删除，剩余的子树就构成了森林。
11Data Structure & Algorithms

Representations
树的表示形式
(1)倒悬树。是最常用的表示形式，如图(a)。
(2)嵌套集合。是一些集合的集体，对于任何两个集合，或者不相交，或
者一个集合包含另一个集合。图(b)是树的嵌套集合形式。
(3)广义表形式。图(c)是树的广义表形式。
12Data Structure & Algorithms
(A(B(E(K,L),F),C(G(M,N)),D(H,I,J)
(a)倒悬树(b)嵌套集合形式(c)广义表

Trees
•TreeSpecifications and terms
•Binary trees
•Traversal of binary trees
•Application: binary trees
Data Structure & Algorithms13

Binary trees
•二叉树在树结构的应用中起着非常重要的作用，原因在于：
•对二叉树的许多操作算法简单
•任何树都可以转换成二叉树。因此我们重点研究二叉树
Data Structure & Algorithms14

Specifications of Binary Trees
•定义
•二叉树是由n(n≥0)个结点的有限集合构成，此集合或者为空集，或者由一个根结点
及两棵互不相交的左右子树组成，并且左右子树都是二叉树。
•这是一个递归定义。二叉树可以是空集合，根可以有空的左子树或空的右子树。
Data Structure & Algorithms15

Specifications of Binary Trees
•二叉树不是树的特例
•二叉树与无序树不同
•二叉树中，每个结点最多只能有两棵子树，并且有左右之分。二叉树并非
是树的特殊情形，它们是两种不同的数据结构。
•二叉树与度数为2的有序树不同
•在有序树中，虽然一个结点的孩子之间是有左右次序的，但是若该结点只
有一个孩子，就无须区分其左右次序。
•而在二叉树中，即使是一个孩子也有左右之分。
16Data Structure & Algorithms

Specifications of Binary Trees
•二叉树结点的子树要区分左子树和右子树，即使只有一棵子树也要进
行区分，说明它是左子树，还是右子树。这是二叉树与树的最主要的
差别。
•下图列出二叉树的5种基本形态，图(c)和图(d)是不同的两棵二叉树。
Data Structure & Algorithms17
LLRR
二叉树的五种不同形态
(a)(c)(b)(e)(d)

二叉树的相关概念
•满二叉树：在一棵二叉树中，如果所有分支结点都存在左子树和右子树，并且所有叶结
点都在同一层上，这样的一棵二叉树称作满二叉树。
Data Structure & Algorithms18
89
2
54
A
B
D
E
HI
3
76
C
FG
1
89101112131415
2
54
A
B
D
E
HIJK
3
76
C
F
G
LMNO
1
(a) 一棵满二叉树(b) 一棵非满二叉树
图6.2  满二叉树和非满二叉树示意图

二叉树的相关概念
•完全二叉树：一棵深度为k的有n个结点的二叉树，对树中的结点按从上至下、从左到右
的顺序进行编号，如果编号为i(1≤i≤n) 的结点与满二叉树中编号为i的结点在二叉树中的
位置相同，则这棵二叉树称为完全二叉树。
•特点：叶结点只能出现在最下层和次下层，且最下层的叶结点集中在树的左部。
•显然，一棵满二叉树必定是一棵完全二叉树，而完全二叉树未必是满二叉树。如(a)为一
棵完全二叉树，图(b)不是完全二叉树。
Data Structure & Algorithms19
(a) 一棵完全二叉树(b) 一棵非完全二叉树
图6.3  完全二叉树和非完全二叉树示意图

应用：表达式树
Data Structure & Algorithms20

应用：表达式树
Data Structure & Algorithms21

Properties of Binary Trees
性质1：在二叉树的第i层上至多有2
i-1
个结点(i≥1)。
【证明】采用归纳法证明此性质。
⚫当i=1 时，只有一个根结点，2
i-1
=2
0
=1，命题成立。
⚫现在假定多所有的j，1≤j<i,命题成立，即第j层上至多有2
j-1
个结点，
那么可以证明j＝i时命题也成立。由归纳假设可知，第i-1 层上至多
有2
i-2
个结点。
⚫由于二叉树每个结点的度最大为2，故在第i层上最大结点数为第i-1
层上最大结点数的二倍，即2×2
i-2
＝2
i-1
。
⚫命题得到证明。
Data Structure & Algorithms22

Properties of Binary Trees
性质2：深度为k的二叉树至多有2
k
-1个结点(k≥1)。
【证明】设第i层的结点数为x
i 
(1≤i≤k)，深度为k的二叉树的结点数为M，
x
i
最多为2
i-1
，则有：
Data Structure & Algorithms23
1
2
2
x
M
k
1
i
k
1
i
k
1
i
i
−
=

=


=
=
−

Properties of Binary Trees
性质3：对任何一棵二叉树，如果其叶结点数为n
0
，度为2的结点数为n
2
，
则n
0
＝n
2
＋1。
【证明】设n为二叉树的结点总数，n
1
为二叉树中度为1的结点数，则有：
n＝n
0
＋n
1
＋n
2
(1)
⚫在二叉树中，除根结点外，其余结点都有唯一的一个进入分支。设B为二叉
树中的分支数，那么有：
B＝n-1(2)
⚫这些分支是由度为1和度为2的结点发出的，一个度为1的结点发出一个分支，
一个度为2的结点发出两个分支，所以有：
B＝n
1
＋2n
2
(3)
⚫综合(1)、(2)、(3)式可以得到：
n
0
＝n
2
＋1
Data Structure & Algorithms24

Properties of Binary Trees
性质4：具有n个结点的完全二叉树的深度k 为log
2
n+1。
【证明】根据完全二叉树的定义和性质2可知，当一棵完全二叉树的深度
为k、结点个数为n时，有
2
k-1
-1<n≤2
k
-1
即
2
k-1
≤n<2
k
对不等式取对数，有
k-1≤log
2
n<k
由于k是整数，所以有k＝log
2
n+1。
Data Structure & Algorithms25
注：符号x表示不大于x的最大整数。

Properties of Binary Trees
性质5：如果对一棵有n个结点的完全二叉树的结点按层序编号(从第1层到第
log
2
n+1层，每层从左到右)，则对任一结点i(1≤i≤n)，有：
(1)如果i＝1，则结点i无双亲，是二叉树的根；如果i>1，则其双亲是结点i/2。
(2)如果2i>n，则结点i为叶结点，无左孩子；否则，其左孩子是结点2i。
(3)如果2i+1>n，则结点i无右孩子；否则，其右孩子是结点2i+1。
•在此过程中，可以从(2)和(3)推出(1)，所以先证明(2)和(3)。
Data Structure & Algorithms26
(a)i和i+1结点在同一层(b)i和i+1结点不在同一层
完全二叉树中结点i和i+1
注：符号x表示不大于x的最大整数。

Properties of Binary Trees
【证明】
◼对于i＝1，由完全二叉树的定义，其左孩子是结点2，若2>n，即不存在结点2，此
是，结点i无孩子。结点i的右孩子也只能是结点3，若结点3不存在，即3>n，此时
结点i无右孩子。
◼对于i>1，可分为两种情况：
1. 设第j(1≤j≤log
2
n)层的第一个结点的编号为i，由二叉树的性质2和定义知i=2
j-1
。结点i的左孩子必定为的j+1层的第一个结点，
其编号为2
j
=2×2
j-1
=2i。如果2
i
>n，则无左孩子；其右孩子必定为第j+1层的第二个结点，编号为2i+1。若2i+1>n，则无右孩子。
2. 假设第j(1≤j≤log
2
n)层上的某个结点编号为i(2e(j-1)≤i≤2ej-1)，且2i+1<n，其左孩子为2i，右孩子为2i+1，则编号为i+1的
结点时编号为i的结点的右兄弟或堂兄弟。若它有左孩子，则其编号必定为2i+2＝2×(i+1)：若它有右孩子，则其编号必定为
2i+3＝2×(i+1)+1。
◼当i＝1时，就是根，因此无双亲，当i>1时，如果i为左孩子，即2×(i/2)=i，则i/2
是i的双亲；如果i为右孩子，i＝2p+1，i的双亲应为p，p＝(i-1)/2=i/2。证毕。
Data Structure & Algorithms27

Properties of Binary Trees
•性质1：在二叉树的第i层上至多有2
i-1
个结点(i≥1)。
•性质2：深度为k的二叉树至多有2
k
-1个结点(k≥1)。
•性质3：对任何一棵二叉树，如果其叶结点数为n
0
，度为2的结点数为n
2
，则n
0
＝n
2
＋1。
•性质4：具有n个结点的完全二叉树的深度k 为log
2
n+1。
•性质5：如果对一棵有n个结点的完全二叉树的结点按层序编号(从第1层到第log
2
n+1
层，每层从左到右)，则对任一结点i(1≤i≤n)，有：
(1)如果i＝1，则结点i无双亲，是二叉树的根；如果i>1，则其双亲是结点i/2。
(2)如果2i>n，则结点i为叶结点，无左孩子；否则，其左孩子是结点2i。
(3)如果2i+1>n，则结点i无右孩子；否则，其右孩子是结点2i+1。
•结点数量与二叉树深度之间的关系
28Data Structure & Algorithms
某一层上的结点数量关系
深度与结点总数的关系
结点间的
数量关系
完全二叉树中结点数量与深度的关系
上下层结点之间的关系

Exercises
•1、一棵完全二叉树有200个结点，则度为1的结点有（）个，度为0的结点有
（）个，度为2的结点有（）个。
•2、一棵完全二叉树第6层有7个结点，则共有（）个结点？其中度为1的结点有
（）个，度为0的结点有（）个，编号最大的非叶结点是（），编号最小的叶
结点是（）。
29Data Structure & Algorithms

Exercises
•1、一棵完全二叉树有200个结点，则度为1的结点有（）个，度为0的结点有
（）个，度为2的结点有（）个。
•度为1的结点数为(200+1) mod 2 = 1，度为0的结点数为200/2 = 100，度为2的结点数为100-
1=99。
•2、一棵完全二叉树第6层有7个结点，则共有（）个结点？其中度为1的结点有
（）个，度为0的结点有（）个，编号最大的非叶结点是（），编号最小的叶
结点是（）。
•一棵完全二叉树除去最底层结点，则是一棵满二叉树，而一棵5层的满二叉树有2
5
-1=31个
结点，因此共有31+7=38个结点，最底层的结点数是奇数，因此度为1的结点有1个，度为
0的结点数为38/2=19，度为2的结点数为19-1=18，编号最大的非叶结点是38/2=19，编号
最小的叶结点是19+1=20。
30Data Structure & Algorithms

Implementation of Binary Trees
顺序存储结构
•用一维数组存储存放二叉树中的结点。一般是按照二叉树结点从上至
下、从左到右的顺序存储。
•这样结点在存储位置上的前驱后继关系并不一定就是它们在逻辑上的
邻接关系，然而只有通过一些方法确定某结点在逻辑上的前驱结点和
后继结点，这种存储才有意义。
•因此，依据二叉树的性质，完全二叉树和满二叉树采用顺序存储比较
合适，树中结点的序号可以唯一地反映出结点之间的逻辑关系，这样
既能够最大可能地节省存储空间，又可以利用数组元素的下标值确定
结点在二叉树中的位置，以及结点之间的关系。
Data Structure & Algorithms31

Implementation of Binary Trees
•给出的下图所示的完全二叉树的顺序存储示意。
•数组下标0       1      2      3      4       5       6      7      8      9 
•下图是完全二叉树
Data Structure & Algorithms32

Implementation of Binary Trees
•对于一般的二叉树，如果仍按从上至下和从左到右的顺序将树中的结点顺序存储在一维数组中，则数组元
素下标之间的关系不能够反映二叉树中结点之间的逻辑关系，只有增添一些并不存在的空结点，使之成为
一棵完全二叉树的形式，然后再用一维数组顺序存储。
•显然，这种存储对于需增加许多空结点才能将一棵二叉树改造成为一棵完全二叉树的存储时，会造成空间
的大量浪费，不宜用顺序存储结构。
•最坏的情况是右单支树，如下图所示，一棵深度为k的右单支树，只有k个结点，却需分配2
k
-1个存储单元。
Data Structure & Algorithms33
(a) 一棵二叉树(b) 改造后的完全二叉树
(c) 改造后完全二叉树顺序存储状态
一般二叉树及其顺序存储示意图
(a) 一棵右单支二叉树(b) 改造后的右单支树对应的完全二叉树
(c) 单支树改造后完全二叉树的顺序存储状态
右单支二叉树及其顺序存储示意图

Implementation of Binary Trees
链式存储结构
◼用链式结构来指示结点之间的逻辑关系。链表中每个结点由三个域组成，除
了数据域外，还有两个指针域。
Data Structure & Algorithms35









Implementation of Binary Trees
三叉链表存储
◼每个结点由四个域组成，具体结构为：
◼其中，data、lchild以及rchild三个域的意义同二叉链表结构；parent域为指向该结点双亲结点的指
针。这种存储结构既便于查找孩子结点，又便于查找双亲结点；但是，相对于二叉链表存储结构而
言，它增加了空间开销。
◼下图是一棵二叉树的三叉链表示。
Data Structure & Algorithms40
二叉树的三叉链表表示示意图
◼尽管在二叉链表中无法由结点直接找到其双
亲，但由于二叉链表结构灵活，操作方便，
对于一般情况的二叉树，甚至比顺序存储结
构还节省空间。因此，二叉链表是最常用的
二叉树存储方式。

Trees
•TreeSpecifications and terms
•Binary trees
•Traversal of binary trees
•Application: binary trees
Data Structure & Algorithms41

Traversal of Binary Trees
•遍历二叉树
•按照某种顺序访问二叉树中的每个结点，使每个结点被访问一次且仅
被访问一次。
•遍历是二叉树中经常要用到的一种操作。因为在实际应用问题中，常
常需要按一定顺序对二叉树中的每个结点逐个进行访问，查找具有某
一特点的结点，然后对这些满足条件的结点进行处理。
•通过一次完整的遍历，可使二叉树中结点信息由非线性排列变为某种
意义上的线性序列。也就是说，遍历操作使非线性结构线性化。
Data Structure & Algorithms42

Traversal of Binary Trees
•由二叉树的定义可知，一棵由根结点、根结点的左子树和根结点的右子树三部
分组成。因此，只要依次遍历这三部分，就可以遍历整个二叉树。
•若以D、L、R分别表示访问根结点、遍历根结点的左子树、遍历根结点的右子
树，则二叉树的遍历方式有六种：DLR、LDR、LRD、DRL、RDL和RLD。如
果限定先左后右，则只有前三种方式，即
•DLR (称为先序遍历)、LDR (称为中序遍历)、LRD (称为后序遍历)。
Data Structure & Algorithms43

Traversal of Binary Trees
1. 先序遍历(DLR)
◼递归过程为：若二叉树为空，遍历结束。否则，
(1)访问根结点；
(2)先序遍历根结点的左子树；
(3)先序遍历根结点的右子树。
◼先序遍历二叉树的递归算法：
voidPreOrder(BiTreebt)//先序遍历二叉树bt
{if(bt==NULL)return;//递归调用的结束条件
Visite(bt->data);//访问结点的数据域
PreOrder(bt->lchild);//先序递归遍历bt的左子树
PreOrder(bt->rchild);//先序递归遍历bt的右子树
}
对于上图所示的二叉树，按先序遍历所得到的结点序列为：ABDGCEF
Data Structure & Algorithms44
7
2
4
A
B
D
G
3
65
C
EF
1

Traversal of Binary Trees
2. 中序遍历(LDR)
◼递归过程为：若二叉树为空，遍历结束。否则，
(1)中序遍历根结点的左子树；
(2)访问根结点；
(3)中序遍历根结点的右子树。
◼中序遍历二叉树的递归算法：
voidInOrder(BiTreebt)//中序遍历二叉树bt
{if(bt==NULL)return;//递归调用的结束条件
InOrder(bt->lchild);//中序递归遍历bt的左子树
Visite(bt->data);//访问结点的数据域
InOrder(bt->rchild);//中序递归遍历bt的右子树
}
对于上图所示的二叉树，按中序遍历所得到的结点序列为：DGBAECF
Data Structure & Algorithms45
7
2
4
A
B
D
G
3
65
C
EF
1

Traversal of Binary Trees
3. 后序遍历(LRD)
◼递归过程为：若二叉树为空，遍历结束。否则，
(1)后序遍历根结点的左子树；
(2)后序遍历根结点的右子树。
(3)访问根结点；
◼后序遍历二叉树的递归算法：
voidPostOrder(BiTreebt)//后序遍历二叉树bt
{if(bt==NULL)return;//递归调用的结束条件
PostOrder(bt->lchild);//后序递归遍历bt的左子树
PostOrder(bt->rchild);//后序递归遍历bt的右子树
Visite(bt->data);//访问结点的数据域
}
对于上图所示的二叉树，按先序遍历所得到的结点序列为：GDBEFCA
Data Structure & Algorithms46
7
2
4
A
B
D
G
3
65
C
EF
1

Traversal of Binary Trees
4. 层次遍历
◼所谓二叉树的层次遍历，是指从二叉树的第一层(根结点)开始，从上至下逐层
遍历，在同一层中，则按从左到右的顺序对结点逐个访问。
对于图6.3(b)所示的二叉树，按层次遍历所得到的结果序列为：ABCDEFG
Data Structure & Algorithms47
7
2
4
A
B
D
G
3
65
C
EF
1

Traversal of Binary Trees
◼由层次遍历的定义可以推知，在进行层次遍历时，对一层结点访问完后，再
按照它们的访问次序对各个结点的左孩子和右孩子顺序访问，这样一层一层
进行，先遇到的结点先访问，这与队列的操作原则比较吻合。
◼因此，在进行层次遍历时，可设置一个队列结构，遍历从二叉树的根结点开
始，首先将根结点指针入队列，然后从队头取出一个元素，每取一个元素，
执行下面两个操作：
(1)访问该元素所指结点；
(2)若该元素所指结点的左、右孩子结点非空，则将该元素所指结点的左孩子
指针和右孩子指针顺序入队。
◼此过程不断进行，当队列为空时，二叉树的层次遍历结束。
Data Structure & Algorithms48

Traversal of Binary Trees
•在下面的层次遍历算法中，二叉树以二叉链表存放，一维数组Queue[MaxNode]用以实现
队列，变量front和rear分别表示当前对首元素和队尾元素在数组中的位置。
Data Structure & Algorithms49
voidLevelOrder(BiTreebt)//层次遍历二叉树bt
{BiTreeQueue[MaxNode];
intfront,rear;
if(bt==NULL)return;
front=-1;rear=0;queue[rear]=bt;
while(front!=rear)
{front++;
Visit(queue[front]->data);//访问队首结点的数据域
if(queue[front]->lchild!=NULL)//将队首结点的左孩子结点入队列
{rear++;
queue[rear]=queue[front]->lchild;
}
if(queue[front]->rchild!=NULL)//将队首结点的右孩子结点入队列
{rear++;
queue[rear]=queue[front]->rchild;
}
}
}

Exercise
【例】右下图(1)所示的二叉树表达式
(a+b×(c-d)-e/f)
⚫若先序遍历此二叉树，按访问结点的先后
次序将结点排列起来，其先序序列为：
-+a×b-cd/ef
⚫按中序遍历，其中序序列为：
a+b×c-d-e/f
⚫按后序遍历，其后序序列为：
abcd-×+ef/-
⚫用户喜欢中缀形式的算术表达式，
对于计算机，使用后缀易于求值。
Data Structure & Algorithms50
-
+
×
a
/
b
-
dc
f
e
图(1)
