# 10-Graph1

> 主题库：数据结构与算法　｜　源文件：10-Graph1.pdf　｜　11,544 字符　｜　46 个片段　｜　更新于 2026-11-11


Graph
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

Graph
•图的示例
•图的基本概念
•图的存储表示
Data Structure & Algorithms2

Graph示例
Data Structure & Algorithms
社交网络图谱
3

Graph示例
Data Structure & Algorithms
计算机网络结构
4

图的基本概念
n图定义   图是由顶点集合(vertex)及顶点间的关系集合组成的一种数据结
构：
             Graph＝( V, E )    
    其中    V = { x | x  某个数据对象} 
    是顶点的有穷非空集合；
            E = {(x, y) | x, y  V } 
    或        E = {<x, y> | x, y  V && Path (x, y)}
    是顶点之间关系的有穷集合，也叫做边(edge)集合。Path (x, y)表示从 x 
到 y 的一条单向通路, 它是有方向的。
Data Structure & Algorithms5
思考：当图的边集为空时，图G还是否存在?
答：还存在，但此时图G只有顶点，即图有
无边、只有点的情形，这个可以看做是图
的特例情况。

图的基本概念
•若代表一条边的顶点序偶是无序的(即该边无方向)，则称此图为无向图。
•若代表一条边的顶点序偶是有序的(即边有方向)，则称此图为有向图。
•通常用n表示图中顶点的数目，用e表示边或弧的数目。无向图中e的取
值范围是从0到n(n - 1)/2，有向图中e的取值范围是从0到n(n - 1)
Data Structure & Algorithms
0
000
1
1
1122
2265433
无向图
有向图
6

图的基本概念
•边数相对较少的图称为稀疏图(sparse graph)，边数相对较多的图称为稠
密图(dense graph)
•任何两顶点间都有边相关联的图称为完全图(complete graph)，完全图显
然具有最大的边数。 
•若有 n 个顶点的无向图有 n(n-1)/2 条边, 则此图为完全无向图。有 n 
个顶点的有向图有n(n-1) 条边, 则此图为完全有向图。
Data Structure & Algorithms7

图的基本概念
n邻接顶点   如果 (u, v) 是 E(G) 中的一条边，则称 u 与 v 互为邻接顶点。
n子图  设有两个图 G＝(V, E) 和 G’＝(V’, E’)。若 V’ V 且 E’E, 则称 图
G’ 是 图G 的子图。
n权  某些图的边具有与它相关的数, 称之为权。这种带权图叫做网络。
Data Structure & Algorithms8

图的基本概念
n顶点的度  一个顶点v的度是与它相关联的边的条数。记作TD(v)。在有
向图中, 顶点的度等于该顶点的入度与出度之和。
n顶点 v 的入度是以 v 为终点的有向边的条数, 记作 ID(v); 
n顶点 v 的出度是以 v 为始点的有向边的条数, 记作 OD(v)。
n路径    在图 G＝(V, E) 中, 若从顶点 v
i 
出发, 沿一些边经过一些顶点 v
p1
, 
v
p2
, ..., v
pm
, 到达顶点v
j
 , 则称顶点序列 (v
i 
 v
p1 
v
p2 
... v
pm 
 v
j
) 为从顶点v
i 
到
顶点 v
j 
的路径。它经过的边(v
i
, v
p1
)、(v
p1
, v
p2
)、...、(v
pm
, v
j
) 应是属于E的
边。
Data Structure & Algorithms9

图的基本概念
n路径长度  非带权图的路径长度是指此路径上边的条数。带权图的路径
长度是指路径上各边的权之和。
n简单路径   若路径上各顶点 v
1
,v
2
,...,v
m 
均不互相重复, 则称这样的路径为
简单路径。
n回路    若路径上第一个顶点 v
1 
与最后一个顶点v
m 
重合, 则称这样的路径
为回路或环。
Data Structure & Algorithms10

图的基本概念
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
Data Structure & Algorithms
(a) 非连通的无向图G3                         （b） 非连通无向图G3的连通分量
非连通无向图的连通分量示意图
11

图的基本概念
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
Data Structure & Algorithms
有向图G
2
的两个强连通分量
12

图的基本概念
•一个连通图的生成树是含有该连通图全部顶点的一个极小连通子图。
•若连通图G的顶点个数为n，则G的生成树的边数为n-1。但是有n-1条边的图不
一定是生成树。
•如果无向图G的一个生成树 G′上添加一条边，则G′中一定有环。
•相反，如果G′的边数小于n-1，则G′一定不连通。
Data Structure & Algorithms
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
13

图的基本概念
•自由树(free tree)是不带简单回路的无向图，它是连通的，并且具有n - 1条边。
•网络(network)是带权的连通图，图7.8中的G4是一个网络。
•如果一个有向图只有一个顶点的入度为0，其余顶点的入度均为1，则称为有向树。
•一个有向图的生成森林由若干棵有向树组成，这些树的并集包含了原图所有顶点，
各有向树的弧不相交。图7.9就是有向图生成森林的示例。
Data Structure & Algorithms
图7.8 网络实例G4                         图7.9 有向图及其生成森林
14

图的抽象数据类型
Data Structure & Algorithms
class Graph {
public:
    Graph ( );
    void InsertVertex ( Type & vertex );// 插入一个顶点
    void InsertEdge ( int v1,  int v2,  int weight );// 插入一条边
    void RemoveVertex ( int v ); // 删除一个顶点
    void RemoveEdge ( int v1,  int v2 );// 删除一条边
    int IsEmpty ( );  // 判断图是否为空
    Type GetWeight ( int v1,  int v2 );// 获取边的权重值
    int GetFirstNeighbor ( int v ); // 获取第一个邻接顶点
    int GetNextNeighbor ( int v1,  int v2 );  // 获取下一个邻接顶点
}
15

图的存储结构及实现
•如何存储图?
•图是由顶点和边组成的，因此在存储中，除了要
存储结点的信息，还要存储边的信息。
•由于图结构的任意性特征，因此相应的存储表示
方法很多。常用的图的存储结构有 邻接矩阵、
邻接表、十字链表 和 邻接多重表。
16Data Structure & Algorithms

图的存储表示
•邻接矩阵 (Adjacency Matrix)
n在图的邻接矩阵表示中，有一个记录各个顶点信息的顶点表，
还有一个表示各个顶点之间关系的邻接矩阵。
n设图 A = (V, E)是一个有 n 个顶点的图, 图的邻接矩阵是一个
二维数组 A.edge[n][n]，定义：
Data Structure & Algorithms





                                              ,
 ),(    , ,
]][[ .
否则
或者如果
0
><1
A
EjiEji
jiEdge
对于n个顶点的图，相邻矩阵的空间代价都为O(n
2
)，与边数无关。 
17

图的存储表示
•无向图的邻接矩阵是对称的;
•有向图的邻接矩阵可能是不对称的。
Data Structure & Algorithms18

图的存储表示
Data Structure & Algorithms
有向图的邻接矩阵
19

图的存储表示
n在有向图中, 统计第 i 行 1 的个数可得顶点 i 的出度，统计
第 j 列 1 的个数可得顶点 j 的入度。
Data Structure & Algorithms
有向带权图的邻接矩阵









ji
ji,ji,ji
ji,ji,jiji
ji
若
或且若
或且若
,
)(,
)(),,(
]][[
0
EE
EEW
A.edge
20

图的存储表示
Data Structure & Algorithms
6
3
1
2
9
5
4
2
0
3
1














06
8053
290
410
A.edge
21
8

图的存储表示
•无向带权图G=(V，E) 的邻接矩阵如下图所示，其元素的定
义如下：
Data Structure & Algorithms
(a)  带权无向图 
(b)   顶点矩阵
无向带权图的数组存储
(c)   邻接矩阵
3
5
4
1
2
6
a
b
c
d
e
3
vexs
a
b
c
d
e
∞ 6   2  ∞ ∞
6   ∞  3   4    3
2    3  ∞  1   ∞
∞ 4   3  ∞   5
∞ 3  ∞  5   ∞
W
ij   
 若(v
i
 , v
j
)E，即v
i
 , v
j
邻接，权值为w
ij
∞   若(v
i
 , v
j
)E，即v
i
 , v
j
不邻接时
A[i][j] =
22

邻接矩阵的实现
Data Structure & Algorithms
       图的邻接矩阵的实现比较容易，定义两个数组分别存储顶点
信息(数据元素)和边或弧的信息(数据元素之间的关系) 。
其存储结构形式定义如下：
#define INFINITY  MAX_VAL     /* 最大值∞ */
/* 根据图的权值类型，分别定义为最大整数或实数 */
#define MAX_VEX  30     /*  最大顶点数目 */
/*  {有向图，无向图，带权有向图，带权无向图}  */
typedef enum {DG, AG, WDG, WAG} GraphKind ;
23

邻接矩阵的实现
Data Structure & Algorithms
typedef struct ArcType
{    VexType  vex1, vex2 ;/*  弧或边所依附的两个顶点 */
ArcValType    ArcVal ; /*  弧或边的权值  */
ArcInfoType  ArcInfo ;/*  弧或边的其它信息   */
} ArcType ;   /*  弧或边的结构定义  */
typedef struct
{   GraphKind  kind ;    /*  图的种类标志  */
int  vexnum , arcnum ;     /*  图的当前顶点数和弧数  */
VexType   vexs[MAX_VEX] ;    /*  顶点数组  */
AdjType   adj[MAX_VEX][MAX_VEX];   /*  边数组  */
} MGraph ;       /*  图的结构定义  */
利用上述定义的数据结构，可以方便地实现图的各种操作。
24

邻接矩阵的实现
图的创建
AdjGraph  *Create_Graph(MGraph * G)
{   printf(“请输入图的种类标志：”) ;
scanf(“%d”, &G->kind) ;
G->vexnum = 0 ;       /*  初始化顶点个数  */
return(G) ; 
}
Data Structure & Algorithms25
图的顶点定位
图的顶点定位操作实际上是确定一个顶点在vexs数组中的位置(下标) ，
其过程完全等同于在顺序存储的线性表中查找一个数据元素。
算法实现：
int  LocateVex(MGraph *G , VexType *vp) 
   {  int  k ;
       for (k=0 ; k<G->vexnum ; k++)
            if (G->vexs[k]==*vp)  return(k) ;
       return(-1) ;     /*  图中无此顶点  */
   }

邻接矩阵的实现
向图中增加顶点
向图中增加一个顶点的操作，类似在顺序存储的线性表的末尾增加一个数据元素。
int  AddVertex(MGraph *G , VexType *vp) 
{  int  k , j ;
if  (G->vexnum >= MAX_VEX)
{  printf(“Vertex Overflow !\n”) ; return(-1) ;  }
if  (LocateVex(G , vp) != -1)
{  printf(“Vertex has existed !\n”) ; return(-1) ; }
k = G->vexnum ; G->vexs[G->vexnum++] = *vp ;  
if (G->kind==DG||G->kind==AG) /*  不带权的有向图或无向图  */
for (j=0 ; j<G->vexnum ; j++)
G->adj[j][k].ArcVal=G->adj[k][j].ArcVal=0 ;  
else
for (j=0 ; j<G->vexnum ; j++)   /*  带权的有向图或无向图  */
{   G->adj[j][k].ArcVal=INFINITY ;
    G->adj[k][j].ArcVal=INFINITY ;
}
return(k) ;
}
Data Structure & Algorithms26

邻接矩阵的实现
向图中增加一条弧
根据给定的弧或边所依附的顶点，修改邻接矩阵中所对应的数组元素。
int  AddArc(MGraph *G , ArcType *arc) 
{   int  k , j ;
k = LocateVex(G , &arc->vex1) ;  j = LocateVex(G , &arc->vex1) ;
if (k==-1||j==-1)     
{  printf(“Arc’s Vertex do not existed !\n”) ;
return(-1) ;
}
if (G->kind==DG||G->kind==WDG) /*  有向图或带权的有向图*/
{G->adj[k][j].ArcVal  = arc->ArcVal;
G->adj[k][j].ArcInfo = arc->ArcInfo ;
}
else  /*  无向图或带权的无向图，需对称赋值  */
{G->adj[k][j].ArcVal = arc->ArcVal ;
G->adj[j][k].ArcVal = arc->ArcVal ;
G->adj[k][j].ArcInfo = arc->ArcInfo ;
G->adj[j][k].ArcInfo = arc->ArcInfo ;
}
return(1) ;  
}
Data Structure & Algorithms27

图的存储表示--邻接表 (Adjacency List)
•当图中的边数较少时，相邻矩阵就会出现大量的零元素，存储这些零元素将耗费大量的
存储空间。对于稀疏图，可以采用邻接表存储法。
•邻接表(Adjacency List) 表示法是一种链式存储结构，由一个顺序存储的顶点表和n个链
接存储的边表组成。
•顶点表有两个域：顶点数据域 和 指向此顶点边表指针域
•边表把依附于同一个顶点 vi 的边（即相邻矩阵中同一行的非0元素）组织成一个单
链表。边表中的每一个表目都代表一条边，由两个主要的域组成：
•与顶点vi邻接的另一顶点的序号
•指向边表中下一个边表目的指针
Data Structure & Algorithms28

图的存储表示--邻接表 (Adjacency List)
•邻接表 (Adjacency List)
Data Structure & Algorithms
无向图的邻接表
同一个顶点发出的边链接在同一个边链表中，每一个链结点代表一
条边 (边结点)，结点中有另一顶点的下标 dest 和指针 link。
29

图的存储表示--邻接表 (Adjacency List)
•有向图的邻接表和逆邻接表
Data Structure & Algorithms30

图的存储表示--邻接表 (Adjacency List)
•网络 (带权图) 的邻接表
Data Structure & Algorithms31

图的存储表示--邻接表 (Adjacency List)
•画出下图G1和G2的邻接表，以及G2的逆邻接表
Data Structure & Algorithms
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
无向图G
1
的邻接表表示
V
0
V
2
V
1
V
3
V
4
0
1
2
3
4
23\
3
4\
034\
012\
12\
有向图G2的邻接表和逆邻接表表示
32

图的存储表示--邻接表 (Adjacency List)
邻接表法的特点
n设图中有 n 个顶点，e 条边，
n则用邻接表表示无向图时，需要 n 个顶点结点，2e 个边结点；
n用邻接表表示有向图时，若不考虑逆邻接表，只需 n 个顶点结点，e 个边结点。
n表头向量中每个分量就是一个单链表的头结点，分量个数就是图中的顶点数目。
n对有向图可以建立正邻接表或逆邻接表。
n正邻接表是以顶点Vi为出度（即为弧的起点）而建立的邻接表。
n逆邻接表是以顶点Vi为入度（即为弧的终点）而建立的邻接表。
n在无向图，顶点Vi的度是第 i 个链表的结点数。
n在有向图中，第 i 个链表中的结点数是顶点Vi的出 (或入)度；求入 (或出)度，须遍历整个邻接表；
n在邻接表的边链表中，各个边结点的链入顺序任意，视边结点输入次序而定，往往按照顶点编
号从小到大排列。在边或弧稀疏的条件下，用邻接表表示比用邻接矩阵表示节省存储空间。
Data Structure & Algorithms33

邻接表的实现
•结点及其类型定义
Data Structure & Algorithms
#define MAX_VEX  30// 最大顶点数
typedef int  InfoType;
typedef enum {DG, AG, WDG,WAG} GraphKind ;
typedef struct LinkNode
{  int  adjvex ;// 邻接点在头结点数组中的位置(下标)
InfoType    info  ;// 与边或弧相关的信息, 如权值
struct LinkNode  *nextarc ;   // 指向下一个表结点
} LinkNode ;         //  表结点类型定义 
typedef struct VexNode
{  VexType  data;// 顶点信息
    int  indegree ;   //  顶点的度, 有向图是入度或出度或没有 
  LinkNode  *firstarc ;// 指向第一个表结点
} VexNode ; // 顶点结点类型定
typedef struct ArcType
{    // 弧或边所依附的两个顶点
 VexType  vex1, vex2 ; 
 // 与边或弧相关的信息, 如权值  
 InfoType    info  ;
} ArcType ;     // 弧或边的结构定义 
typedef struct
{  GraphKind  kind ; // 图的种类标志
    int vexnum ;
    VexNode   AdjList[MAX_VEX] ;
} ALGraph ;     // 图的结构定义 
34

图的存储表示
•邻接多重表 (Adjacency Multilist)
l在邻接多重表中, 每一条边只有一个边结点。为有关边的处理提供了方便。
l无向图的情形
l边结点的结构
Data Structure & Algorithms
mark：是记录是否处理过的标记。
vertex1, vertex2：是该边两顶点位置。
path1：是链接指针，指向下一条依附顶点vertex1的边。
path2：是指向下一条依附顶点vertex2的边链接指针。
需要时还可设置一个存放与该边相关的权值的域 cost。
35

图的存储表示
•顶点结点的结构
•存储顶点信息的结点表以顺序表方式组织, 每一个顶点结点有两个数据成员：
其中，data 存放与该顶点相关的信息，Firstout 是指示第一条依附该顶点的边
的指针。
•在邻接多重表中，所有依附同一个顶点的边都链接在同一个单链表中。 
Data Structure & Algorithms36

图的存储表示
•从顶点 i 出发, 可以循链找到所有依附于该顶点的边，也可以找到它的
所有邻接顶点。
•邻接多重表的结构
Data Structure & Algorithms37

图的存储表示--十字链表
n有向图的情形
n在用邻接表表示有向图时, 有时需要同时使用邻接表和逆邻接表。用有
向图的邻接多重表(十字链表)可把两个表结合起来表示。
u边结点的结构
Data Structure & Algorithms
mark：是处理标记。
vertex1，vertex2：指明该有向边始顶点和终顶点的位置。
path1：是指向始顶点与该边相同的下一条边的指针。
path2：是指向终顶点与该边相同的下一条边的指针。
需要时还可有权值域cost。
38

图的存储表示--十字链表
•顶点结点的结构
Data Structure & Algorithms
每个顶点有一个结点，它相当于出边表和入边表的表头结点：
data：存放与该顶点相关的信息。
Firstin：指针指示以该顶点为终顶点的入边表的第一条边。
Firstout：指针指示以该顶点为始顶点的出边表的第一条边。
39

图的存储表示--十字链表
Data Structure & Algorithms40

图的存储表示--十字链表
•结点类型定义
Data Structure & Algorithms
#define INFINITY  MAX_VAL // 最大值∞ 
#define MAX_VEX  30           // 最大顶点数  
typedef struct ArcNode
{   int  tailvex , headvex ;  // 尾结点和头结点在图中的位置
InfoType    info  ;          // 与弧相关的信息, 如权值
struct ArcNode  *hlink , *tlink ; 
}ArcNode ;    // 弧结点类型定义
typedef struct VexNode
{  VexType  data;     // 顶点信息
ArcNode  *firstin , *firstout ;
}VexNode ;              // 顶点结点类型定义 
typedef struct
{  int vexnum ;
VexNode  xlist[MAX_VEX] ;
}OLGraph ;   //  图的类型定义
从这种存储结构图可以看出，
从一个顶点结点的firstout出发，沿表结点的path1指针构成了正邻接表的链表结构，
而从一个顶点结点的firstin出发，沿表结点的path2指针构成了逆邻接表的链表结构。
41

Exercise
•对有五个顶点{V1, V2, V3, V4, V5}的图的邻接矩阵如图所示：
•解答下列问题：
•画出逻辑结构图。
•绘制出十字链表。
42Data Structure & Algorithms
十字链表参考

Applications: Graph in Combinatorial Optimization
•图结点着色问题是组合最优化中典型的非确定多项式（NP）完全问题，
也是图论中研究得最久的一类问题。
•目前解决该问题的算法很多，如：
•回溯算法、分支限界法、Welsh-Powell 算法、布尔代数法、蚁群算法、贪婪算法、
禁忌搜索算法、神经网络、遗传算法以及模拟退火算法等。
•蛮力法、深度优先或广度优先等算法可以得到最优解，但时间复杂度
较大。
•回溯法的时间复杂度为指数阶的。有的在多项式时间内能得到可行解，
但不是最优解，如 Welsh-Powell 算法和贪婪算法。
•遗传算法和神经网络属于复杂的启发式算法，算法复杂度较大，最终
得到的也是近似解。
43Data Structure & Algorithms

Applications: Graph in Computer Vision
44Data Structure & Algorithms
•Geometry
•Vertex coordinates
•Connectivity
•How do vertices connected?
•List of Edge
•Vertex-Edge
•Vertex-Face
•Combined

Applications: Graph in Computer Graphics
•Mark mesh boundary 
•Create edge adjacency
•Add vertex 
•Add edge 
•Add polygonal face 
•Delete polygonal face 
•Delete edge 
•Delete vertex 
45Data Structure & Algorithms

Summary
•图的示例
•图的基本概念
•图的存储表示
Data Structure & Algorithms46
