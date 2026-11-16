# 08-Tree-2

> 主题库：数据结构与算法　｜　源文件：08-Tree-2.pdf　｜　13,711 字符　｜　53 个片段　｜　更新于 2026-11-11


Trees  & Binary Trees
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn
School of Data and Computer Science
Data Structure & Algorithms

Trees
Nonrecursivetraversal of binary trees
Applicationsofbinary trees
Data Structure & Algorithms2

Traversal of Binary Trees
•二叉树遍历的非递归实现
•前面给出的二叉树先序、中序和后序三种遍历算法都是递归算法。当
给出二叉树的链式存储结构以后，用具有递归功能的程序设计语言很
方便就能实现上述算法。
•然而，递归程序虽然简洁，但执行效率不高。
•为此，我们考虑非递归的实现。
Data Structure & Algorithms3

Traversal of Binary Trees
对二叉树进行先序、中序和后序遍历都是从根结点A开始的，且在遍
历过程中经过结点的路线是一样的，只是访问的时机不同而已。如上图
所示，从根结点左外侧开始，由根结点右外侧结束的曲线，为遍历图的
路线。沿着该路线按▲标记的结点读得的序列为先序序列，按★标记读得
的序列为中序序列，按◎标记读得的序列为后序序列。
Data Structure & Algorithms4

Traversal of Binary Trees
Data Structure & Algorithms5
•遍历路线正是从根结点开始沿左子树深入下去，当深入到最左端，无法再
深入下去时，则返回，再逐一进入刚才深入时遇到结点的右子树，再进行
如此的深入和返回，直到最后从根结点的右子树返回到根结点为止。
•先序遍历是在深入时遇到结点就访问，中序遍历是在从左子树返回时遇到
结点访问，后序遍历是在从右子树返回时遇到结点访问。

Traversal of Binary Trees
Data Structure & Algorithms6
◼在这一过程中，返回结点的顺序与深入结点的顺序相反，即后深入先
返回，正好符合栈结构后进先出的特点。
◼因此，可以用栈来帮助实现这一遍历路线。
遍历图的路线示意图

Traversal of Binary Trees
•在沿左子树深入时，深入一个结点，入栈一个结点。
•若为先序遍历，则在入栈之前访问之，当沿左分支深入不下去时，则
返回，即从堆栈中弹出前面压入的结点；
•若为中序遍历，则此时访问该结点，然后从该结点的右子树继续深入；
•若为后序遍历，则将此结点再次入栈，然后从该结点的右子树继续深
入，与前面类同，仍能深入一个结点入栈一个结点，深入不下去再返
回，直到第二次从栈里弹出该结点，才访问之。
Data Structure & Algorithms7

Traversal of Binary Trees
•利用栈的先序遍历非递归算法
Data Structure & Algorithms8
c
a
b
c
d
e
d
c
c
访问
a
进栈
c
左进
b
访问
b
进栈
d
左进
空
退栈
d
访问
d
左进
空
退栈
c
访问
c
左进
e
访问
e
左进
空
栈空
结束

Traversal of Binary Trees
•利用栈的先序遍历非递归算法
Data Structure & Algorithms9
template <classT> 
void BinaryTree<T>::PreOrder(void (*visit)(BinTreeNode<T> *t))
{   stack<BinTreeNode<T>*> S;
BinTreeNode<T> *p = root; 
S.Push(NULL);
while (p != NULL){
{     visit(p);// 访问结点
if (p->rightChild!= NULL)
S.Push(p->rightChild); // 预留右指针在栈中
if (p->leftChild!= NULL) 
p = p->leftChild;//进左子树
else S.Pop(p);//左子树为空
}
}

Traversal of Binary Trees
•利用栈的中序遍历非递归算法
Data Structure & Algorithms10
a
d
b
a
a
d
a
a
左空
退栈
访问b
左空
退栈
访问d
退栈
访问a
左空
e
c
退栈访问e
cc
右空退栈访问c栈空结束
a
b
c
d
e

Traversal of Binary Trees
•利用栈的中序遍历非递归算法
Data Structure & Algorithms11
template <class T> 
void BinaryTree<T>::InOrder(void (*visit) (BinTreeNode<T> *t))
{   stack<BinTreeNode<T>*> S;
BinTreeNode<T> *p = root;
do
{    while (p != NULL)//遍历指针向左下移动
{S.Push(p); //该子树沿途结点进栈
p = p->leftChild;
}
if (!S.IsEmpty())//栈不空时退栈
{S.Pop(p);  visit (p);//退栈, 访问
p = p->rightChild;//遍历指针进到右子女
}
} while (p != NULL ||!S.IsEmpty());
}

Traversal of Binary Trees
•利用栈的后序遍历非递归算法
◼在后序遍历过程中所用栈的结点定义
template <class T>
structstkNode{
BinTreeNode<T> *ptr;     // 树结点指针
enumtag {L, R}; // 退栈标记
stkNode(BinTreeNode<T> *N = NULL) :
ptr(N), tag(L){ }// 构造函数
};
◼tag = L,  表示从左子树退回还要遍历右子树; 
◼tag = R，表示从右子树退回要访问根结点。
Data Structure & Algorithms12
ptr     tag{L,R}

Traversal of Binary Trees
•利用栈的后序遍历非递归算法
Data Structure & Algorithms13
aL
bL
aL
bR
aL
dL
bR
aL
dR
bR
aL
bR
aLaL
aR
eL
cL
aR
eR
cL
aR
cL
aR
cR
aR
aR
a
b
c
d
e
dbeca

Traversal of Binary Trees
•后序遍历的非递归算法
Data Structure & Algorithms14
voidBinaryTree<T>::PostOrder(void (*visit) (BinTreeNode<T> *t)
{   Stack< stkNode<T> >S;   stkNode<T> w; 
BinTreeNode<T> * p = root;// p是遍历指针
do {
while (p != NULL){
w.ptr= p;  w.tag= L;  S.Push(w);   p = p->leftChild; }
intcontinue1 = 1;// 继续循环标记, 用于R
while (continue1&& !S.IsEmpty()){
S.Pop(w);  p = w.ptr;
switch (w.tag)// 判断栈顶的tag标记
case L:  w.tag=R;  S.Push(w);  continue1 = 0;  
p = p->rightChild;  break;
case R:  visit (p);   break;
}
}
} while (!S.IsEmpty());// 继续遍历其他结点
cout<< endl;
};

Restore the binary tree by the traversal 
•由遍历序列恢复二叉树
•从前面讨论的二叉树的遍历知道，任意一棵二叉树结点的
先序序列和中序序列都是唯一的。
•反过来，若已知结点的先序序列和中序序列，能否确定这
棵二叉树呢？
•这样确定的二叉树是否是唯一的呢？
Data Structure & Algorithms15

Restore the binary tree by the traversal 
•在先序序列中，第一个结点一定是二叉树的根结点。
•另一方面，中序遍历是先遍历左子树，然后访问根结点，最后再遍历右子树。
•根结点在中序序列中必然将中序序列分割成两个子序列，前一个子序列是根结
点的左子树的中序序列，后一个子序列是根结点的右子树的中序序列。
•在先序序列中找到对应的左子序列和右子序列。在先序序列中，左子序列的第
一个结点是左子树的根结点，右子序列的第一个结点是右子树的根结点。这样
就确定了二叉树的三个结点。
•同时，左子树和右子树的根结点又可以分别把左子序列和右子序列划分成两个
子序列，如此递归下去，当取尽先序序列中的结点时，便可以得到一棵二叉树。
Data Structure & Algorithms16

Restore the binary tree by the traversal 
【例】已知一棵二叉树的先序与中序序列分别为ABCDEFGHI和BCAEDGHFI，试恢复该二叉树。
1.首先，由先序序列可知，结点A是二叉树的根结点。
2.其次，根据中序序列，在A之前的所有结点都是根结点左子树的结点，在A之后的所有结点都是根结点
右子树的结点，由此得到图(a)所示的状态。
3.然后，再对左子树进行分解，由先序序列得知B是左子树的根结点，又从中序序列知道，B的左子树为
空，B的右子树只有一个结点C。
4.接着对A的右子树进行分解，得知A的右子树的根结点为D；而结点D把其余结点分成两部分，即左子树
为E，右子树为F、G、H、I，如图(b)所示。接下去的工作就是按上述原则对D的右子树继续分解下去，
最后得到如图(c)的整棵二叉树。
Data Structure & Algorithms17
(a)                                    (b)                                     (c)
一棵二叉树的恢复过程示意

Restore the binary tree by the traversal 
•这是一个递归过程，其思想是：
•先根据先序序列的第一个元素建立根结点；
•然后在中序序列中找到该元素，确定根结点的左、右子树的中序序列；
•再在先序序列中确定左、右子树的先序序列；
•最后由左子树的先序序列与中序序列建立左子树，由右子树的先序序列与中序序列建
立右子树。
Data Structure & Algorithms18
(a)                                    (b)                                     (c)
一棵二叉树的恢复过程示意

Restore the binary tree by the traversal 
•同理，由二叉树的后序序列和中序序列也可唯一地确定一棵二叉树。
•因为，依据后序遍历和中序遍历的定义，后序序列的最后一个结点，就如同先
序序列的第一个结点一样，可将中序序列分成两个子序列，分别为这个结点的
左子树的中序序列和右子树的中序序列。
•再拿出后序序列的倒数第二个结点，并继续分割中序序列，如此递归下去，当
倒着取取尽后序序列中的结点时，便可以得到一棵二叉树。
Data Structure & Algorithms19

Restore the binary tree by the traversal 
20Data Structure & Algorithms
【例】已知二叉树有如下的遍历结果
◼按中序遍历，其中序序列为：
a+b×c-d-e/f
◼按后序遍历，其后序序列为：
abcd-×+ef/-
请根据中序遍历和后序遍历结果重构
二叉树。
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

Restore the binary tree by the traversal 
•另外，如果只知道二叉树的先序序列和后序序列，则不能
唯一地确定一棵二叉树。
21Data Structure & Algorithms
◼先序序列为：
-+a×b-cd/ef
◼后序序列为：
abcd-×+ef/-
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

思考
•在二叉链表中查找结点的前驱后继方便吗?
•二叉树的存储结构中只能找到结点的左、右孩子信息，而不能直接得到结点在任一
序列中的前驱和后继信息，这种信息只有在遍历的动态过程中才能得到。当二叉树
的操作经常要涉及结点的前驱或后继的查找，用二叉链表作为存储结构时查找会很
麻烦，且速度不快。
•采用树的线性存储方式解决结点的前驱或后继的查找，有什么问题?
•顺序表的查找效率比较低，时间复杂度为O(n)，而树的查找属于“范围查找”，查
找效率一般比顺序查找要高。
•二叉链表查找结点的前驱后继，如何简化操作?
•对二叉树以某种遍历顺序进行扫描，在扫描的同时，我们可以通过给每个结点添加
数据项来记录前驱与后继的线索，以达到后续操作中方便查找前驱后继结点的目的。
22Data Structure & Algorithms

不用栈的二叉树遍历的非递归方法
•前面介绍的二叉树的遍历算法可分为两类，
•一类是依据二叉树结构的递归性，采用递归调用的方式来实现；
•另一类则是通过堆栈或队列来辅助实现。
•采用这两类方法对二叉树进行遍历时，递归调用和栈的使用都带来额外空间增
加，递归调用的深度和栈的大小都与二叉树的高度有关。因此，在最坏的情况
下，即二叉树退化为单支树的情况下，递归的深度或栈需要的存储空间等于二
叉树中的结点数。
•还有一类二叉树的遍历算法，就是不用栈也不用递归来实现。利用具有n个结
点的二叉树中的叶结点和一度结点的n＋1个空指针域，来存放线索，然后在这
种具有线索的二叉树上遍历时，就可以不需要栈，也不需要递归了。
Data Structure & Algorithms23

Threaded binary trees 
•二叉树通常存在很多空指针。如下图有10个结点，而带有“Λ”空指针域为11。
•另一方面，如下图进行中序遍历时，得到HDIBJEAFCG序列。可知结点I的前驱
是D，后继是B，结点F的前驱是A，后继是C。
•因此，可以通过遍历来记录结点的前驱和后继结点，从而节省后续访问前驱后
继的时间。
Data Structure & Algorithms24

Threaded binary trees 
线索二叉树的定义
◼按照某种遍历方式对二叉树进行遍历，可以把二叉树中所有结点排列为一个线性序列。
◼在该序列中，除第一个结点外，每个结点有且仅有一个直接前驱结点；除最后一个结点外，每个结
点有且仅有一个直接后继结点。
◼但是，二叉树中每个结点在这个序列中的直接前驱结点和直接后继结点是什么，二叉树的存储结构
中并没有反映出来，只能在对二叉树遍历的动态过程中得到这些信息。
◼为了保留结点在某种遍历序列中直接前驱和直接后继的位置信息，可以利用二叉树的二叉链表存储
结构中的那些空指针域来指示。这些指向直接前驱结点和指向直接后继结点的指针被称为线索
(thread)，加了线索的二叉树称为线索二叉树。
◼线索二叉树将为二叉树的遍历提供许多遍历方式。
Data Structure & Algorithms25

Threaded binary trees 
线索二叉树的结构
◼一个具有n个结点的二叉树若采用二叉链表存储结构，
在2n个指针域中只有n-1个指针域是用来存储结点孩
子的地址，而另外n+1个指针域存放的都是NULL。
◼因此，可以利用某结点空的左指针域(lchild) 指出该
结点在某种遍历序列中的直接前驱结点的存储地址，
利用结点空的右指针域(rchild) 指出该结点在某种遍
历序列中的直接后继结点的存储地址；对于那些非空
的指针域，则仍然存放指向该结点左、右孩子的指针。
这样，就得到了一棵线索二叉树。
◼由于序列可由不同的遍历方法得到，因此，线索树有
先序线索二叉树、中序线索二叉树和后序线索二叉树
三种。把二叉树改造成线索二叉树的过程称为线索化。
Data Structure & Algorithms26

Threaded binary trees 
Data Structure & Algorithms27

Threaded binary trees 
◼区别结点的指针域内存放的是指针还是线索？通常可以采用下面两种方法来
实现。
(1) 为每个结点增设两个标志位域ltag和rtag，令：
0lchild指向结点的左孩子
ltag＝{
1lchild指向结点的前驱结点
0rchild指向结点的右孩子
rtag＝{
1rchild指向结点的后继结点
◼每个标志位令其只占一个bit，这样就只需增加很少的存储空间。这样结点的
结构为：
(2)不改变结点结构，仅在作为线索的地址前加一个负号，即负的地址表示线索，
正的地址表示指针。
Data Structure & Algorithms28
ltaglchilddatarchildrtag

Threaded binary trees 
•这里按第一种方法来介绍线索二叉树的存储。
•为了将二叉树中所有空指针域都利用上，以
及操作便利的需要，在存储线索二叉树时往
往增设一头结点
•头结点的结构与其它线索二叉树的结点结构
一样，只是其数据域不存放信息，其左指针
域指向二叉树的根结点，右指针域指向自己。
•而原二叉树在某序遍历下的第一个结点的前
驱线索和最后一个结点的后继线索都指向该
头结点。
Data Structure & Algorithms29

Threaded binary trees 
在中序线索二叉树上查找任意结点的前驱结点
◼对于中序线索二叉树上的任一结点，寻找其中序的前驱结点，有以下两种情况：
◼如果该结点的左标志为1，那么其左指针域所指向的结点便是它的前驱结点；
◼如果该结点的左标志为0，表明该结点有左孩子，根据中序遍历的定义，它的前驱结
点是以该结点的左孩子为根结点的子树的最右结点，即沿着其左子树的右指针链向
下。查找，当某结点的右标志为1时，它就是所要找的前驱结点。
Data Structure & Algorithms30
中序遍历：
H D I B J E A F C G

Exercise
•已知有二叉树：
(1) 绘出先序前驱线索化示意图
(2) 绘出中序前驱和后继线索化示意图
(3) 绘出后序后继线索化示意图
31Data Structure & Algorithms

Applications of Binary Trees 
32Computer Graphics

Applications: binary trees 
•表达式运算
我们可以把任意一个算数表达式用一棵二叉树表示，图6.15所示为表达式
3x
2
+x-1/x+5的二叉树表示。在表达式二叉树中，每个叶结点都是操作数，每个
非叶结点都是运算符。对于一个非叶结点，它的左、右子树分别是它的两个操作
数。
Data Structure & Algorithms33
图6.15   表达式3x2+x-1/x+5的二叉树表示示意

Applications: binary trees 
◼对该二叉树分别进行先序、中序和后序遍历，可以得到表达式的三种
不同表示形式。
前缀表达式+-+*3*xxx/1x5
中缀表达式3*x*x+x-1/x+5
后缀表达式3xx**x+1x/-5+
◼中缀表达式是经常使用的算术表达式，前缀表达式和后缀表达式分别
称为波兰式和逆波兰式，它们在编译程序中有着非常重要的作用。
Data Structure & Algorithms34

Applications: Huffman tree and coding
◼哈夫曼树（HuffmanTree）是指对于一组带有确定权值的叶结点，构造的具
有最小带权路径长度的二叉树，也称最优二叉树（OptimalBinaryTree）。
◼哈夫曼编码（HuffmanCoding）是一种编码方式，是一种用于无损数据压
缩的熵编码（权编码）算法，是DavidA.Huffman发明的。
Data Structure & Algorithms35

Applications: Huffman tree and coding
◼那么什么是二叉树的带权路径长度呢？
◼设二叉树具有n个带权值的叶结点，那么从根结点到各个叶结点的路径长度与
相应结点权值的乘积之和，称为二叉树的带权路径长度，记为：
◼其中W
k
为第k个叶结点的权值，L
k
为第k个叶结点的路径长度。
36Data Structure & Algorithms
1
n
kk
k
WPLWL
=
=


Applications: Huffman tree and coding
•【例】如图6.16所示的二叉树，它的带权路径长度值
WPL＝2×2＋4×2＋5×2＋3×2＝28。
图6.16  一个带权二叉树
•在给定一组具有确定权值的叶结点，可以构造出不同的带权二叉树。
例如，给出4个叶结点，设其权值分别为1,3,5,7，我们可以构造出形状
不同的多个二叉树。这些形状不同的二叉树的带权路径长度将各不相
同。
Data Structure & Algorithms37
4
5
32

•这五棵树的带权路径长度分别为：
(a) WPL＝1×2＋3×2＋5×2＋7×2＝32       (b) WPL＝1×3＋3×3＋5×2＋7×1＝29
(c) WPL＝1×2＋3×3＋5×3＋7×1＝33       (d) WPL＝7×3＋5×3＋3×2＋1×1＝43 
(e) WPL＝7×1＋5×2＋3×3＋1×3＝29 
Applications: Huffman tree and coding
Data Structure & Algorithms38
(a)                                     (b)                                 (c) 
(d)                                   (e)
具有相同叶结点和不同带权路径长度的二叉树

Applications: Huffman tree and coding
◼由相同权值的一组叶结点所构成的二叉树有不同的形态和不同的带权
路径长度，那么如何找到带权路径长度最小的二叉树呢？
◼根据哈夫曼树的定义，一棵二叉树要使其WPL值最小，必须使权值越
大的叶结点越靠近根结点，而权值越小的叶结点越远离根结点。
◼哈夫曼(Huffman)依据这一特点提出了一种方法。
Data Structure & Algorithms39

Applications: Huffman tree and coding
哈夫曼树的基本思想
(1)由给定的n个权值{W
1
,W
2
,...,W
n
}构造n棵只有一个叶结点的二叉树，从而得到
一个二叉树的集合F＝{T
1
,T
2
,...,T
n
}；
(2)在F中选取根结点的权值最小和次小的两棵二叉树作为左、右子树构造一棵新
的二叉树，这棵新的二叉树根结点的权值为其左、右子树根结点权值之和；
(3)在集合F中删除作为左、右子树的两棵二叉树，并将新建立的二叉树加入到集
合F中；
(4)重复(2)、(3)两步，当F中只剩下一棵二叉树时，这棵二叉树便是所要建立的哈
夫曼树。
Data Structure & Algorithms40

Applications: Huffman tree and coding
•下图给出了前面提到的叶结点权值集合为W＝{1，3，5，7}的哈夫曼树的构造
过程。
•可以计算出其带权路径长度为29，由此可见，对于同一组给定叶结点所构造的
哈夫曼树，树的形状可能不同，但带权路径长度值是相同的，一定是最小的。
Data Structure & Algorithms41
7
5
3
1
7
5
3
1
4
7
5
3
1
4
9
7
5
3
1
4
9
16
第二步
第四步
第一步
第三步
哈夫曼树的建立过程

Applications: Huffman tree and coding
42Data Structure & Algorithms
下图是权值集合W={8, 3, 4, 6, 5, 5}构造Huffman树的过程。
所构造的Huffman树的WPL是：WPL=62+33+43+82+53+53 =79
3455
6
8
第一步
55
6
8
第二步
34
7
68
第三步
34
7
55
10
8
第四步
55
10
6
34
7
13
第六步
1
1
1
1
1
0
0
0
0
0
8
55
10
18
6
34
7
13
31
Huffman树的构造过程
第五步
8
55
10
18
6
34
7
13

Applications: Huffman tree and coding
◼在数据通讯中，经常需要将传送的文字转换成由二进制字符0，1组成
的二进制串，称之为编码。
例：假设要传送的电文为ABACCDA，电文中只含有A, B, C, D四种字符。
字符的四种不同的编码方案
Data Structure & Algorithms43
字符编码
A000
B010
C100
D111
字符编码
A00
B01
C10
D11
字符编码
A0
B110
C10
D111
字符编码
A01
B010
C001
D10
(a)(b)(c)(d)

Applications: Huffman tree and coding
◼传送的电文为ABACCDA
◼若采用表(a)所示的编码，则电文的代码为000010000100100111000，长度为21。
在传送电文时，我们总是希望传送时间尽可能短，这就要求电文代码尽可能短，显然，
这种编码方案产生的电文代码不够短。
◼若采用表(b)所示的编码，则电文的代码为00010010101100，长度为14。在这种编
码方案中，四种字符的编码均为两位，是一种等长编码。如果在编码时考虑字符出现
的频率，让出现频率高的字符采用尽可能短的编码，出现频率低的字符采用稍长的编
码，构造一种不等长编码，则电文的代码就可能更短。
◼若采用表(c)所示的编码时，上述电文的代码为0110010101110，长度仅为13。
◼若采用表(d)所示的编码时，上述电文的代码为01010010010011001，长度仅为17。
Data Structure & Algorithms44
不同的编码方式产生不同的数据存储消耗！！

Applications: Huffman tree and coding
◼哈夫曼树可用于构造使电文的编码总长最短的编码方案。
◼具体做法如下：设需要编码的字符集合为{d
1
,d
2
,...,d
n
}，它们在电文中出现的
次数或频率集合为{w
1
,w
2
,...,w
n
}，以d
1
,d
2
,...,d
n
作为叶结点，w
1
,w
2
,...,w
n
作为
它们的权值，构造一棵哈夫曼树。
◼规定哈夫曼树中的左分支代表0，右分支代表1，则从根结点到每个叶结点，
所经过的路径分支组成的0和1的序列，便为该结点对应字符的编码，我们称
之为哈夫曼编码。
◼在哈夫曼编码树中，树的带权路径长度的含义是各个字符的码长与其出现次
数的乘积之和，也就是电文的代码总长，所以采用哈夫曼树构造的编码是一
种能使电文代码总长最短的不等长编码。
Data Structure & Algorithms45

Applications: Huffman tree and coding
•实现哈夫曼编码的算法可分为两大部分：
(1) 构造哈夫曼树；
(2) 在哈夫曼树上求叶结点的编码。
•求哈夫曼编码，实质上就是在已建立的哈夫曼树中，从叶结点开始，沿结点的双亲链域
回退到根结点，每回退一步，就走过了哈夫曼树的一个分支，从而得到一位哈夫曼码值。
•由于一个字符的哈夫曼编码是从根结点到相应叶结点所经过的路径上各分支所组成的0，
1序列，因此先得到的分支代码为所求编码的低位码，后得到的分支代码为所求编码的高
位码。
Data Structure & Algorithms46

Applications: Huffman tree and coding
•在建立不等长编码时，必须使任何一个字符的编码都不是另一个字符
编码的前缀，这样才能保证译码的唯一性。
•存在前缀编码，称之为具有二义性的译码。
•采用哈夫曼树进行编码，则不会产生上述二义性问题。
•因为，在哈夫曼树中，每个字符结点都是叶结点，它们不可能在根结
点到其它字符结点的路径上，所以一个字符的哈夫曼编码不可能是另
一个字符的哈夫曼编码的前缀，从而保证了译码的非二义性。
Data Structure & Algorithms47

Applications: Huffman tree and coding
例：假设有一个电文字符集中有8个字符，每个字符的使用频率分别为
{0.05, 0.29, 0.07, 0.08, 0.14, 0.23, 0.03, 0.11}，现以此为例设计哈夫曼
编码。
◼为方便计算，将所有字符的频度乘以100，使其转换成整型数值集合，
得到{5, 29, 7, 8, 14, 23, 3, 11}；
◼哈夫曼编码设计如下图。
Data Structure & Algorithms48

Applications: Huffman tree and coding
Data Structure & Algorithms49

Applications: Huffman tree and coding
哈夫曼编码的实现算法
◼首先根据字符的权值构建哈夫曼树，然后从每个叶结点开始不断的向上搜索双亲结点，
直到根点为止，得出字符的哈夫曼编码。
◼编码的存储结构：
typedefstruct//哈夫曼编码的存储结构
{   char bits[N];//保存编码的数组
intstart;  //编码有效起始位置，该位置之后的01串为字符的编码
char ch;     //字符
}  CodeType;
CodeTypecode[N+1]; //字符编码数组，下标为0的单元空出
Data Structure & Algorithms50

Applications: Huffman tree and coding
huffmanCode(HufmTreetree[],CodeTypecode[])
/*利用哈夫曼树求字符的哈夫曼编码*/
{    inti,c,p;
for(i= 1; i<=N; i++)// N次循环，分别得到N个字符的编码
{    code[i].start = N;
c = i;
p = tree[i].parent;// 获取字符的双亲
while (p != 0)// 一直往上层查找，直到根结束
{   code[i].start--;
if ( tree[p].lchild==c )  code[i].bits[code[i].start] = '0';
else  code[i].bits[code[i].start] = '1';
c = p;
p = tree[p].parent;
}
}
}
Data Structure & Algorithms51

Case
•给定集合{15,3,14,2,6,9,16,17}，解答如下问题：
(1)构造相应的哈夫曼树，给出构造过程；
(2)计算它的带权路径长度；
(3)写出它的Huffman编码。
52Data Structure & Algorithms

Case
•计算它的带权路径长度；
WPL = (2 + 3) x 5
+ 6 x 4
+ (9 + 14 + 15) x 3
+ (16 + 17) x 2
= 229
•Huffman编码
15：1106：1001
3：100009：101
14：11116：00
2：1000117：01
53Data Structure & Algorithms
0
0
0
0
0
01
1
1
1
1
1
1
0
