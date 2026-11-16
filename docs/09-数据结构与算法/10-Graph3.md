# 10-Graph3

> 主题库：数据结构与算法　｜　源文件：10-Graph3.pdf　｜　21,274 字符　｜　71 个片段　｜　更新于 2026-11-11


Graph
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Data and Computer Science
Data Structure & Algorithms

Graph
•最小生成树
•最短路径 
Data Structure & Algorithms2

图的连通性问题
n连通图与连通分量   在无向图中, 若从顶点v
1
到顶点v
2
有路径, 则称顶点
v
1
与v
2
是连通的。
n如果图中任意一对顶点都是连通的, 则称此图是连通图。
n非连通图的极大连通子图叫做连通分量。
Data Structure & Algorithms3
(a) 非连通的无向图G3                         （b） 非连通无向图G3的连通分量
非连通无向图的连通分量示意图

图的连通性问题
n对于有向图G = <V，E>，若G中任意两个顶点v
i
和v
j
(v
i
≠v
j
)，都有一条从
v
i
到v
j
的有向路径，同时还有一条从v
j
到v
i
的有向路径，则称有向图G是
强连通图。
n有向图强连通的极大子图称为该有向图的强连通分支或者强连通分量。 
4Data Structure & Algorithms
有向图G
2
的两个强连通分量

图的连通性问题
•一个连通图的生成树是含有该连通图全部顶点的一个极小连通子图。
•若连通图G的顶点个数为n，则G的生成树的边数为n-1。但是有n-1条边
的图不一定是生成树。
•如果无向图G的一个生成树 G′上添加一条边，则G′中一定有环，因为依
附于这条边的两个顶点有另一条路径。相反，如果G′的边数小于n-1，
则G′一定不连通。
5Data Structure & Algorithms
v
0
v
2
v
3
v
1
v
4
v
0
v
2
v
1
v
3
(a)  无向图G
1
的生成树
(b)  有向图G
2
的生成树
v
0
v
2
v
3
v
1
v
4
v
0
v
2
v
1
v
3
(a)  无向图G
1
(b)  有向图G
2

图的连通性问题
•无向图的连通分量与生成树
   对于无向图，对其进行遍历时：
◆ 若是连通图：仅需从图中任一顶点出发，就能访问图
中的所有顶点；
◆ 若是非连通图：需从图中多个顶点出发。每次从一个
新顶点出发所访问的顶点集序列恰好是各个连通分量的
顶点集；
Data Structure & Algorithms6

图的连通性问题
        如下图所示的无向图是非连通图，按图中给定的邻接表进行深度优
先搜索遍历，2次调用DFS所得到的顶点访问序列集是： { v1 ,v3 ,v2} 和
{ v4 ,v5 }
Data Structure & Algorithms7
(a)  无向图G
v
1
v
2
v
3
v
4
v
5
(b)  G的邻接链表
0
1
2
3
4
MAX_VEX-1
v
1
       
v
2
v
3
       
v
4
┇  ┇ 
v
5
       
2
1   ⋀
2
0   ⋀
0
1   ⋀
4   ⋀
3   ⋀
    无向图及深度优先生成森林
(c)  深度优先生成森林
v
1
v
2
v
3
v
4
v
5

图的连通性问题
 (1) 若G=(V,E)是无向连通图， 顶点集和边集分别是V(G) ，
E(G) 。若从G中任意点出发遍历时， E(G)被分成两个互不相
交的集合：
T(G) ：遍历过程中所经过的边的集合；
B(G) ：遍历过程中未经过的边的集合；
    显然： E(G)=T(G)∪B(G) ，T(G)∩B(G)=Ø
Data Structure & Algorithms8

图的连通性问题
 (2)  若G=(V,E)是无向非连通图，对图进行遍历时得到若干个连通分量的顶点集：
V
1
(G) ,V
2
(G) ,...,V
n
(G)和相应所经过的边集：T
1
(G) ,T
2
(G) , ...,T
n
(G) 。
    则对应的顶点集和边集的二元组：G
i
=(V
i
(G),T
i
(G)) (1≦i≦n) 是对应分量的生成
树，所有这些生成树构成了原来非连通图的生成森林。
说明：当给定无向图要求画出其对应的生成树或生成森林时，必须先给出相应的
邻接表，然后才能根据邻接表画出其对应的生成树或生成森林。
Data Structure & Algorithms9

图的连通性问题
•图的生成树和生成森林算法
•对图的深度优先搜索遍历DFS算法稍作修改，就可得到构造图的DFS生
成树算法。 
•在算法中，树的存储结构采用孩子-兄弟表示法。
•首先，从某个顶点V出发，建立一个树结点。
•然后再分别以V的邻接点为起始点，建立相应的子生成树，并将其作为V
结点的子树链接到V结点上。
•显然，算法是一个递归算法。
Data Structure & Algorithms10

图的生成树和生成森林算法实现
Data Structure & Algorithms11
template<class Type> 
void DFS_Forest ( Graph<Type>& G, 
Tree <Type> &T ) {
   TreeNode <Type> * rt, * subT;
   int n = G.NumberOfVertices( );
   static int *visited = new int[n];   //访问数组
   for ( int i = 0; i < n; i++ ) visited [i] = 0;
   for ( i = 0; i < n; i++ )        // 遍历所有顶点
      if ( !visited[i] ) {               // 顶点 i 未访问过
    if ( T.IsEmpty ( ) )              // 原为空森林,建根
       subT = rt = T.BuildRoot( G.GetValue(i) );
       // 顶点 i 的值成为根 rt 的值 
    else subT = T.InsertRightSibling
                             (subT, G.GetValue(i));
             // 顶点 i 的值成为 subT 右兄弟的值
       DFS_Tree ( G, T, subT, i, visited );
             //从顶点 i 出发深度优先遍历, 建子树 
     }
}
template<class Type> 
void DFS_Tree ( Graph<Type>& G, Tree<Type>&T,     
      TreeNode <Type>* rt, int v,  int visited [ ] ) {
  TreeNode<Type> * p;
  visited[v] = 1;       // 顶点v作访问过标志
  int w = G.GetFirstNeighbor (v); // 取顶点v的第一个邻接点w
  int FirstChild = 1; // 第一个未访问子女应是v的左子女 
  while ( w != -1 ) {       // 邻接顶点w存在
      if ( ! visited [w] ) {   // w未访问过, 将成为v的子女
         if ( FirstChild ) {
            p = T.InsertLeftChild ( rt, G.GetValue(w) );
           //p插入为rt的左子女
            FirstChild = 0;   //建右兄弟
       }
      else p = T.InsertRightSibling( p, G.GetValue(w) );
      // p 插入为 p 的左子女
        DFS_Tree ( G, T, p, w, visited );
             // 递归建立 w 的以 p 为根的子树
        }   // 邻接顶点 w 处理完
        w = G.GetNextNeighbor ( v, w ); // 取v的下一个邻接点 w
    }  // 回到 while 判邻接顶点 w 存在
} 

有向图的强连通分量
•对于有向图，在其每一个强连通分量中，任何两个顶点都是可达的。 
VG，与V可相互到达的所有顶点就是包含V的强连通分量的所有顶点。
•设从V可到达 (以V为起点的所有有向路径的终点)的顶点集合为T
1
(G)，
而到达V (以V为终点的所有有向路径的起点) 的顶点集合为T
2
(G)，则包
含V的强连通分量的顶点集合是： T
1
(G)∩T
2
(G) 。
Data Structure & Algorithms12

有向图的强连通分量
•求有向图G的强连通分量的基本步骤是：
(1) 对G进行深度优先遍历，生成G的深度优先生成森林T。
(2)  对森林T的顶点按中序遍历顺序进行编号。
(3)  改变G中每一条弧的方向，构成一个新的有向图G’。
13Data Structure & Algorithms
d
a
c
f
e
b
(a)  有向图G
6
5
4
3
2
1
d
a
c
f
e
b
(b)  执行步骤(1)和(2)
利用深度优先搜索求有向图的强连通分量
a
c
d
f
e
b
(c)  执行步骤(3)

有向图的强连通分量
(4)  按(2)中标出的顶点编号，从编号最大的顶点开始对G’进行深度优先搜索，得到一棵深度
优先生成树。若一次完整的搜索过程没有遍历G’的所有顶点，则从未访问的顶点中选择一
个编号最大的顶点，由它开始再进行深度优先搜索，并得到另一棵深度优先生成树。在该
步骤中，每一次深度优先搜索所得到的生成树中的顶点就是G的一个强连通分量的所有顶点。 
(5)  重复步骤(4) ，直到G’中的所有顶点都被访问。如下图是求一棵有向树的强连通分量过程。
Data Structure & Algorithms14
d
a
c
f
e
b
(a)  有向图G
6
5
4
3
2
1
d
a
c
f
e
b
(b)  执行步骤(1)和(2)
a
c
d
f
e
b
(c)  执行步骤(3)
a
d
c
b
e
f
(d)  执行步骤(4)和(5)
利用深度优先搜索求有向图的强连通分量

有向图的强连通分量
•在算法实现时，建立一个数组in_order[n]存放深度优先生成森林的中序遍历序列。对每个顶点v，在调
用DFS函数结束时，将顶点依次存放在数组in_order[n]中。图采用十字链表作为存储结构最合适。
•算法实现： int in_order[MAX_VEX] ;
Data Structure & Algorithms15
void  DFS(OLGraph *G , int v)  //  按弧的正向搜索
{  ArcNode  *p ;
   Count=0 ;
   Visited[v]=TRUE ;
   for  (p=G->xlist[v].firstout ; p!=NULL ; p=p->tlink)
   if  (!Visited[p->headvex])  
   DFS(G , p->headvex) ;
   in_order[count++]=v ;
} 
void  Rev_DFS(OLGraph *G , int v) 
{  ArcNode  *p ;
    Visited[v]=TRUE ;
    printf(“%d” , v) ;     /*  输出顶点  */
    for  (p=G->xlist[v].firstin ; p!=NULL ; p=p->hlink)
    if  (!Visited[p->tailvex]) 
    Rev_DFS(G , p->tailvex) ;
}    /*  对图G按弧的逆向进行搜索  */
void  Connected_DG(OLGraph *G)
{   int  k=1, v, j ; 
    for (v=0; v<G->vexnum; v++)  
          Visited[v]=FALSE ;
    for (v=0; v<G->vexnum; v++)    /*  对图G正向遍历  */
         if (!Visited[v])  DFS(G,v) ;     
    for (v=0; v<G->vexnum; v++)  
          Visited[v]=FALSE ;
    for (j=G->vexnum-1; j>=0; j--)    /*  对图G逆向遍历  */
    {  v=in_order[j] ;
       if (!Visited[v])
       {  printf(“\n第%d个连通分量顶点: ”, k++) ;
          Rev_DFS(G, v) ;
       }
     }
}
备注：中序遍历森林其实指的就是后根遍历森林中的每一棵树。

生成树问题
16Data Structure & Algorithms
   图 G’=(V, T(G)) 是 G 的极小连
通子图，且 G’ 是一棵树。G’ 称
为图G的一棵生成树。
    从任意点出发按DFS算法得到
生成树 G’ 称为深度优先生成树；
   按BFS算法得到的 G’ 称为广度
优先生成树。

最小生成树
•如果连通图是一个带权图，则其生成树中的边也带权，生成树中
所有边的权值之和称为生成树的代价。
•最小生成树(Minimum Spanning Tree) ：带权连通图中代价最小
的生成树称为最小生成树。
•最小生成树在实际中具有重要用途，如设计通信网。
•设图的顶点表示城市，边表示两个城市之间的通信线路，边的权值表示建
造通信线路的费用。
•n个城市之间最多可以建n(n-1)/2条线路，如何选择其中的n-1条，使总的
建造费用最低?
Data Structure & Algorithms17

最小生成树
构造最小生成树的算法有许多，基本原则是：
◆ 尽可能选取权值最小的边，但不能构成回路；
◆ 选择n-1条边构成最小生成树。
以上的基本原则是基于MST的如下性质：
◆ 设G=(V，E)是一个带权连通图，U是顶点集V的一个非空子集。
◆ 若u∈U ，v∈V-U，且(u, v)是U中顶点到V-U中顶点之间权值
最小的边，则必存在一棵包含边(u, v)的最小生成树。
Data Structure & Algorithms18

最小生成树
•证明：用反证法证明。
•设图G的任何一棵最小生成树都不包含边(u,v)。
•设T是G的一棵生成树，则T是连通的，从u到v必有一条路径(u,...,v)。
•当将边(u,v)加入到T中时就构成了回路，则路径(u, ...,v)中必有一条边
(u’,v’) ，满足u’∈U ，v’∈V-U 。
•删去边(u’,v’) 便可消除回路，同时得到另一棵生成树T’。
•由于(u,v)是U中顶点到V-U中顶点之间权值最小的边，故(u,v)的权值不
会高于(u’,v’)的权值，T’的代价也不会高于T， T’是包含(u,v) 的一棵最
小生成树，与假设矛盾。
Data Structure & Algorithms19

最小生成树
•最小生成树类定义
Data Structure & Algorithms20
class MSTEdgeNode {   //生成树边结点类
friend class MinSpanTree;
private:
    int tail, head;  //生成树各边的两顶点
    float cost;        //生成树各边的权值
public:
    MSTEdgeNode ( )   //构造函数
        : tail ( -1 ), head ( -1 ), cost ( 0 ) { }
}; 
class MinSpanTree {
protected:
    MSTEdgeNode * edgevalue;
    int MaxSize, n;
public:
    MinSpanTree ( int sz = NumVertices-1 ) 
      : MaxSize(sz), n (0) {
        edgevalue = new MSTEdgeNode[MaxSize];
    }
    int Insert ( MSTEdgeNode& item );
}

最小生成树的各种算法
•Kruskal算法是一种用来寻找最小生成树的算法，该算法在1956年由约瑟夫·伯纳德·克鲁斯
卡尔（Joseph Bernard Kruskal）提出。
•Prim算法是图论中的一种算法，可在加权连通图里搜索最小生成树。该算法于1930年由
捷克数学家沃伊捷赫·亚尔尼克发现，并在1957年由美国计算机科学家罗伯特·普里姆独立
发现；1959年，艾兹格·迪科斯彻再次发现了该算法。因此，在某些场合，普里姆算法又
被称为DJP算法、亚尔尼克算法或普里姆-亚尔尼克算法。
•对于这一问题的其他算法包括反向删除算法和 BORůVKA 算法，这几种算法都是贪心算法
的应用。与BORůVKA算法不同的地方是，Kruskal算法在图中存在相同权值的边时也有效。
21Data Structure & Algorithms

最小生成树--克鲁斯卡尔 (Kruskal) 算法
Data Structure & Algorithms22
n克鲁斯卡尔算法的基本思想：
        设有一个有 n 个顶点的连通网络 N = { V, E }, 最初先构造一个
只有 n 个顶点, 没有边的非连通图 T = { V,  },  图中每个顶点自成
一个连通分量。
        当在 E 中选到一条具有最小权值的边时, 若该边的两个顶点落
在不同的连通分量上，则将此边加入到 T 中; 
        否则将此边舍去，重新选择一条权值最小的边。
        如此重复下去, 直到所有顶点在同一个连通分量上为止。

最小生成树--克鲁斯卡尔 (Kruskal) 算法
Data Structure & Algorithms23
n算法的框架：利用最小堆(Min Heap)和并查集(Disjoint Sets)来实现克鲁
斯卡尔算法。
n首先, 利用最小堆来存放E中的所有的边, 堆中每个结点的格式为
n在构造最小生成树过程中,  利用不相交集的运算检查依附一条边的两顶
点tail、head 是否在同一连通分量 (即不相交集的同一个子集合) 上,  是
则舍去这条边；否则将此边加入 T, 同时将这两个顶点放在同一个连通
分量上。

最小生成树--克鲁斯卡尔 (Kruskal) 算法
•随着各边逐步加入到最小生成树的边集合中,   各连通分量也在逐步合
并,   直到形成一个连通分量为止。
Data Structure & Algorithms24
应用克鲁斯卡尔算法构造最小生成树的过程
10
5
0
4
6
1
3
2
28
10
25
14
24
22
16
18
12
5
0
4
6
1
3
25
0
4
6
1
3
2
原图                       (a)                       (b)

最小生成树--克鲁斯卡尔 (Kruskal) 算法
Data Structure & Algorithms25
          
(0,5,10) 选中   
(2,3,12) 选中
(1,6,14) 选中   
(1,2,16) 选中   
(3,6,18) 舍弃
(3,4,22) 选中   
(4,6,24) 舍弃   
(4,5,25) 选中

最小生成树--克鲁斯卡尔 (Kruskal) 算法
Data Structure & Algorithms26
void Kruskal ( Graph<string>& G, MinSpanTree& T) 
{
    MSTEdgeNode  ed;            //边结点辅助单元
    int n = G.NumberOfVertices( );     //顶点数
    int e = G.NumberOfEdges( );         //边数
    MinHeap <MSTEdgeNode> H(e); //最小堆
    UFSets F(n);                                   //并查集
    for ( int u = 0; u < n; u++ )
       for ( int v = u +1; v < n; v++ )
              if ( G.GetWeight(u, v) != MaxValue ) {    
     ed.tail = u;  ed.head = v;       //插入堆
                           ed.cost = GetWeight(u, v);  H.Insert (ed);  } 
    int count = 1;     //最小生成树加入边数计数
    while ( count < n ) {   
         H.RemoveMin (ed);  //从堆中退出一条边
         u = F.Find (ed.tail);  v = F.Find (ed.head);
if ( u != v ) {       //两端不在同一连通分量
    F.Union (u, v);   //合并
             T.Insert (ed);       //该边存入最小生成树 
             count++;
         }
    }
}
          (0,5,10) 选中   (2,3,12) 选中
 (1,6,14) 选中   (1,2,16) 选中   (3,6,18) 舍弃
 (3,4,22) 选中   (4,6,24) 舍弃   (4,5,25) 选中

最小生成树--克鲁斯卡尔 (Kruskal) 算法
•思考：Kruskal算法实现的关键问题是什么?
•Kruskal算法的关键包含以下两点：
•如何判断边e所连接的两个结点u和v不在同一棵树上?
•如何把两个结点u和v所在的树合并为一棵树?
•两个顶点是否属于同一棵树，关键点应该是找到这棵树的标志与顶点的关系，
树的一个明显的标志应该是树的根结点，两个顶点如果同属于一棵树，则它们
应该有共同的根结点。
•所以解决第一个问题的办法，可以是分别查找结点u和v的根结点，如果它们的
根结点相同，则认为它们在同一棵树上。
•解决第二个问题的办法，是把结点u和v的根结点分别作为父、子结点而连接在
一起。这样就把两个分离的树组成了同一棵树。
27Data Structure & Algorithms

补充：并查集
•树如何合并的问题，属于子集归并的并查集问题。
•并查集上的子集合并运算，即要合并两个元素所属的子集。
•首先要确定两个元素所属子集所对应的树的根结点，然后将其中一棵树的根结
点链作另一个棵树的子树即可。为减免在合并过程中出现畸形树（近似单链
树），通常将成员较少的子集对应的树，作为成员较多的子集对应的树的子树。
•并查集（Disjoint Sets）是一种树型的数据结构，用于处理一些不相交集合的合
并及查询问题。常常在使用中以森林来表示。集就是让每个元素构成一个单元
素的集合，也就是按一定顺序将属于同一组的元素所在的集合合并。并查集的
主要操作有：
（1）初始化：把每个点所在集合初始化为其自身。
（2）查找：查找元素所在的集合，即根结点。
（3）合并：将两个元素所在的集合合并为一个集合。通常来说，合并之前应先判断两
个元素是否属于同一集合，这可用查找操作来实现。
28Data Structure & Algorithms

最小生成树--普里姆(Prim)算法
n普里姆算法的基本思想：
n从连通网络 N = { V, E }中的某一顶点 u
0 
出发, 选择与它关联
的具有最小权值的边 ( u
0
, v ), 将其顶点加入到生成树顶点集
合U中。
n以后每一步从一个顶点在 U 中, 而另一个顶点不在 U 中的各
条边中选择权值最小的边(u, v), 把它的顶点加入到集合 U 中。
如此继续下去, 直到网络中的所有顶点都加入到生成树顶点集
合 U 中为止。
n采用邻接矩阵作为图的存储表示。
Data Structure & Algorithms29

最小生成树--普里姆(Prim)算法
n在构造过程中，还设置了两个辅助数组：
u lowcost[ ]   存放生成树顶点集合内顶点到生成树外各顶点的各边上
的当前最小权值；
u nearvex[ ]  记录生成树顶点集合外各顶点距离集合内哪个顶点最近
(即权值最小)。
n例子
Data Structure & Algorithms30

最小生成树--普里姆(Prim)算法
n若选择从顶点0出发，即u
0
 = 0，则两个辅助数组的初始状态为：
n然后反复做以下工作：
u在 lowcost [ ]中选择 nearvex[i]  -1 && lowcost[i]最小的边, 用 v 标
记它。则选中的权值最小的边为(nearvex[v], v), 相应的权值为 
lowcost[v]。 
Data Structure & Algorithms31

最小生成树--普里姆(Prim)算法
u将 nearvex[v] 改为-1,  表示它已加入生成树顶点集合。
u将边 (nearvex[v], v, lowcost[v])  加入生成树的边集合。
u取 lowcost[i] = min{ lowcost[i], Edge[v][i] }，即用生成树顶点集合外各顶点 i 到刚加入该
集合的新顶点 v 的距离 Edge[v][i] 与原来它们到生成树顶点集合中顶点的最短距离 
lowcost[i] 做比较,   取距离近的作为这些集合外顶点到生成树顶点集合内顶点的最短距离。
u如果生成树顶点集合外顶点 i 到刚加入该集合的新顶点 v 的距离比原来它到生成树顶点
集合中顶点的最短距离还要近，则修改nearvex[i] : nearvex[i] = v。表示生成树外顶点i到
生成树内顶点v当前距离最近。
Data Structure & Algorithms32

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms33

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms34

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms35

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms36

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms37

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms38

最小生成树--普里姆(Prim)算法
Data Structure & Algorithms39
void Prim(Graph<string>& G, MinSpanTree&T) {
    int i, j, n = G.NumberOfVertices( );   //顶点数
    float * lowcost = new float[n];
    int * nearvex = new int[n];
    for ( i = 1; i < n; i++ ) {
        lowcost[i] = G.GetWeight(0, i); //顶点0到各边代价
        nearvex[i] = 0;  //及最短带权路径
    }
    nearvex[0] = -1;        //加到生成树顶点集合   
     MSTEdgeNode e;     //最小生成树结点单元
    for ( i = 1; i < n; i++ ) {
     //循环n-1次, 加入n-1条边
       float min = MaxValue;  int v = 0;
       for ( j = 0; j < n; j++ )
           if ( nearvex[j] != -1 && lowcost[j] < min )
       { v = j;  min = lowcost[j]; }
        //求生成树外顶点到生成树内顶点具有最
        //小权值的边, v是当前具最小权值的边 
 if ( v ) {     //v=0表示再也找不到要求顶点
           e.tail = nearvex[v];   e.head = v;
           e.cost = lowcost[v];   
           T.Insert (e);          //选出的边加入生成树
           nearvex[v] = -1;  //该边加入生成树标记
           for ( j = 1; j < n; j++ )
               if ( nearvex[j] != -1 &&   
    G.GetWeight(v, j) < lowcost[j] ) {    
                  lowcost[j] = G.GetWeight(v, j); 
                  nearvex[j] = v;
              }
        }
    }     //循环n-1次, 加入n-1条边
}
 

普里姆(Prim)算法 VS.克鲁斯卡尔 (Kruskal) 算法
n分析普里姆算法, 设连通网络有 n 个顶点，则该算法的时间复杂度为 
O(n
2
)，它适用于边稠密的网络，与边的数目无关。
n克鲁斯卡尔算法主要针对边展开，边数少时效率会很高，所以对于边
稀疏的图有优势。
n注意：当各边有相同权值时，由于选择的随意性，产生的生成树可能
不惟一。
Data Structure & Algorithms40

Exercise
•有如下无向图，解答下列问题：
•（1）从顶点A出发，分别给出深度优先生成树和广度优先生成树；
•（2）根据Prim算法和Kruskal算法，绘制出最小生成树过程。
41Data Structure & Algorithms

Exercise
•（1） 从顶点A出发，分别给出深度优先生成树和广度优先生成树；
•（2）根据Prim算法和Kruskal算法，绘制出最小生成树过程。
42Data Structure & Algorithms

最短路径问题
43Data Structure & Algorithms
小明计划去一些城市旅行。有些城市之间有公路，有些城市之间则没有，如下图。
为了节约经费，以及旅程交通上便利，小明期望在动身之前，知道到达各个城市
的最短旅程。

最短路径
•若用带权图表示交通网，图中顶点表示地点，边代表两地之间有直接
道路，边上的权值表示路程(或所花费用或时间) 。从一个地方到另一个
地方的路径长度表示该路径上各边的权值之和。问题：
◆ 两地之间是否有通路?
◆ 在有多条通路的情况下，哪条最短?
•考虑到交通网的有向性，直接讨论的是带权有向图的最短路径问题，
但解决问题的算法也适用于无向图。
•将一个路径的起始顶点称为源点，最后一个顶点称为终点。
Data Structure & Algorithms44

最短路径
•Edsger Wybe Dijkstra (埃德斯加·狄克斯特拉)
——最先察觉“goto有害”的计算机科学大师
•成功地设计并实现了在有障碍物的两个地点之间找出一条最短
路径的高效算法，这个算法被命名为“狄克斯特拉算法”，解
决了机器人学中的一个十分关键的问题，即运动路径规划问题，
至今仍被广泛应用，被认为是利用“贪心法”(greedy method)
设计算法的一个成功范例。
45Data Structure & Algorithms

单源点最短路径
•对于给定的有向图G=(V，E)及单个源点V
s
，求V
s
到G的其余各顶点的最
短路径。
•针对单源点的最短路径问题，Dijkstra提出了一种按路径长度递增次序
产生最短路径的算法，即狄克斯特拉 (Dijkstra)算法（迪杰斯特拉）。
•该算法是由荷兰计算机科学家狄克斯特拉于1959 年提出的，因此又叫
狄克斯特拉算法。
•迪杰斯特拉算法主要特点是以起始点为中心向外层层扩展，直到扩展
到终点为止。
Data Structure & Algorithms46

单源点最短路径
1 基本思想
    从图的给定源点到其它各个顶点之间客观上应存在一条最短路径，在
这组最短路径中，按其长度的递增次序，依次求出到不同顶点的最短路
径和路径长度。
    按长度递增的次序生成各顶点的最短路径，即先求出长度最小的一条
最短路径，然后求出长度第二小的最短路径，依此类推，直到求出长度
最长的最短路径。
47Data Structure & Algorithms

单源点最短路径
Data Structure & Algorithms48
0
1
2
3
4
5
20
30
60
65
15
20
10
80
40
35
70
带权有向图及其邻接矩阵
∞  20   60   ∞  10  65
∞ ∞   30   70  ∞ ∞
∞  ∞   ∞   40  ∞ ∞
∞ ∞  ∞  ∞   35  ∞
∞ ∞   ∞  ∞  ∞ 20
∞ ∞   15   80  ∞ ∞

单源点最短路径
2 算法思想说明
        设给定源点为V
s
，S为已求得最短路径的终点集，开始时令S={V
s
} 。当求得
第一条最短路径(V
s 
，V
i
)后，S为{V
s
，V
i
} 。根据以下结论可求下一条最短路径。
设下一条最短路径终点为V
j 
，则V
j
只有：
◆  源点到终点有直接的弧<V
s
，V
j
>；
◆ 从V
s 
出发到V
j 
的这条最短路径所经过的所有中间顶点必定在S中。即只有
这条最短路径的最后一条弧才是从S内某个顶点连接到S外的顶点V
j
 。
若定义一个数组dist[n]，其每个dist[i]分量保存从V
s 
出发中间只经过集合S中的顶
点而到达V
i
的所有路径中长度最小的路径长度值，则下一条最短路径的终点V
j
必
定是不在S中且值最小的顶点，即： 
dist[i]=Min{ dist[k]| V
k
∈V-S }
利用上述公式就可以依次找出下一条最短路径。
Data Structure & Algorithms49

单源点最短路径
•关于Dijkstra算法思想的另一种说明
n把图中所有顶点分成两组
n第一组包括已确定最短路径的顶点；
n第二组包括尚未确定最短路径的顶点；
n按最短路径长度递增的顺序逐个把第二组的顶点加到第一组中去
n直至从s出发可以到达的所有顶点都包括进第一组中。
n在这个过程中，总保持从s到第一组各顶点的最短路径长度都不大于从s到
第二组的任何顶点的最短路径长度，而且，每个顶点都对应一个距离值：
n第一组的顶点对应的距离值就是从s到该顶点的最短路径长度；
n第二组的顶点对应的距离值是从s到该顶点的只包括第一组的顶点为中
间顶点的最短路径长度。
Data Structure & Algorithms50

单源点最短路径
3  算法步骤
Data Structure & Algorithms51
① 令S={Vs} ，用带权的邻接矩阵表示有向图，对图中每个顶点Vi按以下原则置
初值：
② 选择一个顶点V
j 
，使得：
                        dist[j]=Min{ dist[k]| V
k
∈V-S }
   V
j
就是求得的下一条最短路径终点，将V
j 
并入到S中，即S=S∪{V
j
} 。
③ 对V-S中的每个顶点V
k 
，修改dist[k]，方法是：
     若dist[j]+W
jk
<dist[k]，则修改为：
               dist[k]=dist[j]+W
jk 
(V
k
∈V-S )
④ 重复②，③，直到S=V为止。
每个dist[i]分量保存从Vs 出发中间只
经过集合S中的顶点而到达Vi的所有
路径中长度最小的路径长度值，

单源点最短路径
Data Structure & Algorithms52
0
1
2
3
4
5
20
30
60
65
15
20
10
80
40
35
70
带权有向图及其邻接矩阵
∞  20   60   ∞  10  65
∞ ∞   30   70  ∞ ∞
∞  ∞   ∞   40  ∞ ∞
∞ ∞  ∞  ∞   35  ∞
∞ ∞   ∞  ∞  ∞ 20
∞ ∞   15   80  ∞ ∞

单源点最短路径
Data Structure & Algorithms53
求最短路径时数组dist (路径长度)和pre (经过结点)的各分量的变化情况
已求得最短路径的终点集

单源点最短路径
3  算法步骤
Data Structure & Algorithms54
① 令S={Vs} ，用带权的邻接矩阵表示有向图，对图中每个顶点Vi按以下原则置
初值：
② 选择一个顶点V
j 
，使得：
                        dist[j]=Min{ dist[k]| V
k
∈V-S }
   V
j
就是求得的下一条最短路径终点，将V
j 
并入到S中，即S=S∪{V
j
} 。
③ 对V-S中的每个顶点V
k 
，修改dist[k]，方法是：
     若dist[j]+W
jk
<dist[k]，则修改为：
               dist[k]=dist[j]+W
jk 
(V
k
∈V-S )
④ 重复②，③，直到S=V为止。
每个dist[i]分量保存从Vs 出发中间只
经过集合S中的顶点而到达Vi的所有
路径中长度最小的路径长度值，

单源点最短路径
4 算法实现
        用带权的邻接矩阵表示有向图， 对Prim算法略加改动就成了Dijkstra算法，
将Prim算法中求每个顶点V
k
的lowcost值用dist[k]代替即可。
◆  设数组pre[n]保存从V
s
到其它顶点的最短路径。若pre[i]=k，表示从V
s 
到V
i
的最短路径中，V
i
的前一个顶点是V
k
，即最短路径序列是(V
s 
, ..., V
k  
, V
i
) 。 
◆ 设数组final[n]，标识一个顶点是否已加入S中。
Data Structure & Algorithms55

单源点最短路径
Data Structure & Algorithms56
BOOLEAN  final[MAX_VEX] ;
int  pre[MAX_VEX] , dist[MAX_VEX] ;
void Dijkstra_path (AdjGraph *G, int v)
           /*   从图G中的顶点v出发到其余各顶
点的最短路径    */
{  int j, k, m, min ; 
for ( j=0; j<G->vexnum; j++)
{   pre[j]=v ;  final[j]=FALSE ;
dist[j]=G->adj[v][j] ;
}    /*  各数组的初始化  */
/*  设置S={v}  */
dist[v]=0 ; final[v]=TRUE ; 
/*  其余n-1个顶点  */
for ( j=0; j<G->vexnum-1; j++) 
{   m=0 ;
while (final[m])  m++;   /*  找不在S中的顶点
vk */
min=INFINITY ; 
for ( k=0; k<G->vexnum; k++)
    {  if  (!final[k]&&dist[m]<min)
            {   min=dist[k] ;  m=k ; }
    }     /* 求出当前最小的dist[k]值  */
final[m]=TRUE ;   /*  将第k个顶点并入S中  */
for ( j=0; j<G->vexnum; j++)
    { if (!final[j]&&(dist[m]+G-
>adj[m][j]<dist[j]))
          {   dist[j]=dist[m]+G->adj[m][j] ; 
               pre[j]=m ;  
          }
     }     /* 修改dist和pre数组的值  */ 
   }      /* 找到最短路径  */
}

单源点最短路径
•算法分析
•从算法设计的角度来说，Dijkstra算法为贪心算法。
•当图中各边上的权值均相等时，图的广度优先遍历算法可以用来求
解单源点的最短路径问题。
•Dijkstra算法应用于有向图，而最小生成树算法是针对无向图而言的，
若把给定的无向图视为有向完全图，则可以利用Dijkstra算法产生其
最小生成树。
57Data Structure & Algorithms

单源点最短路径
•Dijkstra算法一般只适用于有向图的边上的权值，均大于或等于0的情况，而对
于带负权值的有向图，虽然能够执行，但会产生错误的结果。
•为什么Djkstra算法的限定条件为非负权值？
•当所有边权都为正时，由于不会存在一个距离更短的没扩展过的点u，所以v0到这个u点的距离
一旦确认就不会再被改变；若存在负权边，则会在扩展时产生更短的距离，有可能就破坏了已
经更新的点的距离不会改变的性质。
•所以只有当所有边权都为非负时，才能保证算法的正确性，故用Dijkstra求最短路的图不能有负
权边。
58Data Structure & Algorithms
Visual Dijkstra - Simple visual graph editor, with 
built-in step-by-step Dijkstra's algorithm
https://github.com/Samuele458/visual-dijkstra 

Case
•有带权有向图如图所示，给出用Dijkstra算法产生
的最短路径的最小生成树，设顶点A为源点。
59Data Structure & Algorithms
确定顶点A到B, C, E之间存
在路径
确定到达顶点B的最短路径
长度
更新路径确定到达顶点C的最短路径
长度
更新路径确定到达顶点D的最短路径
长度
更新路径确定到达顶点E的最短路径
长度

每一对顶点间的最短路径
•1962年，图灵奖获得者、斯坦福大学计算机科学系教授Robert W.Floyd（罗伯
特·弗洛伊德）将“多源点之间最短路径算法”发表在“Communications of the 
ACM”上，同年Stephen Warshall（史蒂芬·沃舍尔）也独立发表了这个算法，所
以称Floyd-Warshall算法，一般简称为Floyd算法。
•Floyd算法的时间复杂度仍是O(n
3
) ， 但算法形式更为简明，步骤更为简单，数
据结构仍然是基于图的邻接矩阵。
•注：用Dijkstra算法也可以求得有向图G=(V, E)中每一对顶点间的最短路径。方
法是：每次以一个不同的顶点为源点重复Dijkstra算法便可求得每一对顶点间的
最短路径，时间复杂度是O(n
3
) 。 
Data Structure & Algorithms60

每一对顶点间的最短路径
•1 Floyd算法思想
设顶点集S(初值为空)，用数组A的每个元素A[i][j]保存从V
i
只经过S中的
顶点到达V
j
的最短路径长度，其思想是：
① 初始时令S={ } ， A[i][j]的赋初值方式是：
Data Structure & Algorithms61
② 将图中一个顶点V
k 
加入到S中，修改A[i][j]的值，修改方法是：
A[i][j]=Min{A[i][j] , (A[i][k]+A[k][j]) }
原因： 从V
j
只经过S中的顶点(V
k
)到达V
j
的路径长度可能比原来不
经过V
k
的路径更短。
③ 重复②，直到G的所有顶点都加入到S中为止。

每一对顶点间的最短路径
2 Floyd算法实现
◆  定义二维数组Path[n][n](n为图的顶点数) ，元素Path[i][j]保存从V
i
到V
j
的最
短路径所经过的顶点。
◆ 若Path[i][j]=k：从V
i
到V
j 
经过V
k 
，最短路径序列是(V
i 
, ..., V
k 
, ..., V
j
) ，则
路径子序列：(V
i 
, ..., V
k
)和(V
k 
, ..., V
j
)一定是从V
i
到V
k
和从V
k
到V
j 
的最短路
径。从而可以根据Path[i][k]和Path[k][j]的值再找到该路径上所经过的其它顶
点，...依此类推。
◆ 初始化为Path[i][j] = -1，表示从V
i
到V
j 
不经过任何(S中的中间)顶点。当某
个顶点V
k
加入到S中后使A[i][j]变小时，令Path[i][j]=k。
Data Structure & Algorithms62
带权有向图及其邻接矩阵
 0     2     8
∞  0    4
 5   ∞    0
V
1
4
8
2
V
2
V
0
5

每一对顶点间的最短路径
Data Structure & Algorithms63
根据上述过程中Path[i][j]数组，得出：
V0到V1 ：最短路径是{ 0, 1 } ，    路径长度是 2 ；
V0到V2 ：最短路径是{ 0, 1, 2 } ，路径长度是 6 ；
V1到V0 ：最短路径是{ 1, 2, 0 } ，路径长度是 9 ；
V1到V2 ：最短路径是{ 1, 2 } ，    路径长度是 4 ；
V2到V0 ：最短路径是{ 2, 0 } ，    路径长度是 5 ；
V2到V1 ：最短路径是{ 2, 0, 1 } ，路径长度是 7 ；
V
1
4
8
2
V
2
V
0
5

每一对顶点间的最短路径
Data Structure & Algorithms64
int  A[MAX_VEX][MAX_VEX] ;
int  Path[MAX_VEX][MAX_VEX] ;
void Floyd_path (AdjGraph *G)
{  int j, k, m ; 
for ( j=0; j<G->vexnum; j++)
for ( k=0; k<G->vexnum; k++)
{  A[j][k]=G->adj[j][k] ; Path[j][k]=-1 ;  }
       /*  各数组的初始化  */
for ( m=0; m<G->vexnum; m++)
for ( j=0; j<G->vexnum; j++)
for ( k=0; k<G->vexnum; k++)
   if ((A[j][m]+A[m][k])<A[j][k])
        {  A[j][k]=A[j][m]+A[m][k] ;
            Path[j][k]=k ; 
        }    /*  修改数组A和Path的元素值  */
for ( j=0; j<G->vexnum; j++)
for ( k=0; k<G->vexnum; k++)
if (j!=k)
    {  printf(“%d到%d的最短路径为:\n”, j, k) ;
        printf(“%d  ”,j) ; prn_pass(j, k) ;
        printf(“%d  ”, k) ;
        printf(“最短路径长度为: %d\n”,A[j][k]) ;
    }
}      /*   end of Floyd  */
void  prn_pass(int j , int k)
{   if (Path[j][k]!=-1)
    {  prn_pass(j, Path[j][k]) ;
        printf(“, %d” , Path[j][k]) ;
        prn_pass(Path[j][k],  k) ;
     }
}

每一对顶点间的最短路径
•Floyd算法分析
•因为Floyd算法的时间复杂度主要在于三重for循环，所以很明显，其复
杂度是O(n×n×n)。对比起Dijkstra算法，Floyd算法在形式上比较简单。
•若有向图中存在带负权值的边，但不存在负权值的回路时，则Floyd算
法依然能够正常执行。
•Floyd算法本质上是一个递推算法，其核心的运算是矩阵运算。Floyd算
法的意义在于该算法将图论问题转化为代数问题进行求解，而图的邻
接矩阵是问题转化的桥梁。图论问题是非数值计算问题，而矩阵的运
算是典型的数值计算问题，这一转化实现了非数值计算与数值计算的
统一。
Data Structure & Algorithms65

Exercise
•已知无向图G，给出求距V0的各顶点中最短路径长度最长的一个顶点的
算法。
66Data Structure & Algorithms

Summary
•图的示例
•图的基本概念：无向图，有向图，带权图
•图的存储表示：邻接矩阵，邻接表，邻接多重表
•图的遍历与连通性：深度优先搜索，广度优先搜索 
•最小生成树：Kruskal算法，Prim算法
•最短路径：Dijkstra算法，Floyd算法
•有向无环图及其应用：拓扑排序，关键路径
Data Structure & Algorithms67
