# 08-Tree-3-图集

> 主题库：数据结构与算法　｜　源文件：08-Tree-3-图集.pdf　｜　3,227 字符　｜　49 个片段　｜　更新于 2026-11-11
Bimary Tree Class Specimcalion

intsizeocons voidclearO; intheightOconst; void insert(const Entry &); Binary_tree (const Binary tree<Entry> &original); Binary_tree & operator = (const Binary tree<Entry> &original);

Binary Search Trees Detinitior

Binary Search Trees

Tlhe rimarv Search lree Class

Tree Search mot present

Tree Searcln Fecursiva auxiiary tumctiom

Tree Search Fmbiic mnathol for trae seanch

IBinary Search Trees with the Same Keys Q

Amalvsis of Tree Search

Lnsertion imto a Finary Searcn H ree

Hmsertiom imto a Fimary SearchTree a) Insert e b)Irsertb (cInsert d 2 e (d) Insert f (e) Insert & Insert g (@ Insert c の

M etmod tor Imsertmnom

mraosort Furst savontaae Sesond anvantage

Removal from a binary search tree Delete Casedeletionofaleaf

Removalfrom a binary search tree Deletiex Case omeistabtreeiemmpty

Removal from a binary search tree 1Delete Replace x byw originaliw wispredecessor X *V W W Case: neither subtree empty w has no right child andcanbedeleted Findxsimmediate a models im-onder pnetecessor isu le miahte predecessor: go as far right as 1 ost l li oft l-f sub-tiee possible

Auxiliary Fmmctiom to Fennove Ome l ode

Belam cac Tinary Starcin Troe tor Trdormmatom Fetieran 2 10 14 18 22 26 30 1517 222 2 2

Random Search Trees amd Optimnality

Height balanoe: Avl Irees

二言国手BF(BalanceFactor)定义为该结点的左子树的深度减去它的右子树的深度. 任一结点的平衡因子只能取：、 或；如果树中任意一个结点的平衡因子的绝对值大于，则这棵二树就去平衡，不再A树对于一棵有n个结点的AV树，其言

如果在一棵AVL树中插入一个新结点，就有可能造成失衡，此时必须 ，使之恢复平衡。我们称调整平衡过程为 。 左旋转右旋转先左后右旋转先右后左旋转

左旋转 (a) 插入到c的右子树 (b)言育心高 (c)左旋转结点A是插入后失去平衡的最高层结点，插入后A的右子树的右树偏高，则进行左旋转。

右旋转 B (a) 插入到B的左子树 (b)左子症后言 (c)右旋转结点A是插入后失去平衡的最高层结点，插入后A的左子树的左子树偏高，则进行右旋转。

-1 A A B C 2 B C 插入后右子的左于树 D E h D E 偏高 F G E G h h-1 h-1 h-1 先右旋转 h (a) (b) A 0 D B D A C E C B G E h-1 左旋转 G E h-1 h h h h (c) (d)

A A B  C 插入后 B C 权的有子 D E D O E h 偏高 h I E G F G h h-1 h-1 h-1 h 先左旋转 (b) (a A E C E  A B G B    D F G C D F h-1 右旋转 (c) (d)

· 例， 输入关键码序列为{16，3，7，11,9,26,18,14， 15}，插入和调整过程如下。 16 16 0 -1 6 LR旋转 3 3 16 1 0 -2 2 2  LL旋转 16 11  26

左单旋 -1 右左双旋 -2 1 26 26 18 1 1 1 18) 1 16 26 9 .6 14

-1 左右双旋 11 8 2 3 0 16 20 9 15 26 -1 0 0 14 16) 2

C .. Comvemtioms for A VL Trees lee highar eapal heigntrieltrhipher

Mletnods tor Falance Factors

D mmmny Nletlnodls andl Virtnal Tletlnods

Pablic lmsertion Netnoo

Recmrsive Fumctiom Specimcatioms

Rのtatioms ot am AvI Tree rlght tree  root Rotate 加 +1 Tota1 hedpht = h + 3 Total heiott = h + 2

L0のumbleLlotatiom .1 .1. One of T or Te hae height h.

Deleted Ca1 Hetabt Caso2 cDaletedl Ce

一1 T h一1 一1 Daleted 一2 一一1 一 Deleted o cYa l

[odt : 02 Deletep: An balence factor hr

Rotata a aet clght gronuundl A :

Worst-Case AVL Trees

10 NIL 50 NIL 0 NIL 10 80 NIL NIL NIL NIL NIL NIL NIL NIL NIL NIL 红树

键建值，即表中记录的主键 1 1 35 3 指针存储子节点地址信息据表记录中除主键数 3 5 9 10 13 151 28 29 36 60 75 79 90 99 03 3 d a i 5

定义阶为m的B-树是一棵m一路树，其中： 1.所有树叶在同一层。 2.除了根以外的所有内部结点多有m个空孩，少有m/27个空的孩 。 3.每个内部结点中的键数都比它的空孩数少， 这些键以查找树的式划 4根多有个孩，但如果它不是树叶只有2个孩，或者如果树本分孩子中 。 mn pg u a b c e f hijk

1. 2. ag b: abf g k a 3. d,h,m 4. j f a bd g h k m a b d g h k m 5. 6. e,s,ir: x f f T b d e g h i k m r s  a b d e g h i k m s

c, ntu: c fj r b m x P n t u x

1.删除h,r: 提升s D Js 并从树叶删除 2.删除p: 拉下s， m t 提上 s de p[ ]rux n 2 S

3.删除d： 合成： 合成： fj m r
