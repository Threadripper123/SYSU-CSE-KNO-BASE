# 10-Graph2

> 主题库：数据结构与算法　｜　源文件：10-Graph2.pdf　｜　12,382 字符　｜　52 个片段　｜　更新于 2026-11-11


Graph
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Data and Computer Science
Data Structure & Algorithms

Graph
•图的遍历 
Data Structure & Algorithms2

图的遍历与连通性
•网络爬虫的搜索问题
•“网络爬虫”是捜索引擎抓取系统软件的重要组成部分，它遍历Web空间，能够扫
描一定IP地址范围内的网站，并沿着网络上的链接从一个网页到另一个网页，从一
个网站到另一个网站采集网页资料。
•面对数量众多的网站，如何无遗漏且不重复地高效访问所有网页，需要有好的策略
来实现图的遍历。
3Data Structure & Algorithms

图的遍历与连通性
n从已给的连通图中某一顶点出发，沿着一 些边访遍图中所有的顶点，
且使每个顶点仅被访问一次，就叫做图的遍历 ( Graph Traversal )。
n图中可能存在回路，且图的任一顶点都可能与其他顶点相通，在访问
完某个顶点之后可能会沿着某些边又回到了曾经访问过的顶点。
n为了避免重复访问，可设置一个标志顶点是否被访问过的辅助数组 
visited [ ]。
Data Structure & Algorithms4

图的遍历与连通性
n如何避免图的同一顶点在遍历时被访问多次？
n辅助数组 visited[ ] 的初始状态为0，在图的遍历过程中，一旦某一个顶点 i
 
被访问，
就立即让 visited[i] 为 1，防止它被多次访问。
n图的遍历的分类:
u深度优先搜索 
         DFS (Depth First Search)
u广度优先搜索 
         BFS (Breadth First Search)
Data Structure & Algorithms5

Depth first traversal
p Start to traverse from a vertex v
0
. 
p  Let w
1
,w
2
, ..., w
k
 be the vertices adjacent to v
0
. Then 
we shall traverse  from w
1 
 until we traverse all the 
vertices to which it is adjacent before returning to 
traverse w
2
, ...,w
k
 .
p  In case there are vertices which have not been visited, 
then chose one vertex and start depth first traversal 
until all vertices are visited.

图的遍历与连通性
Data Structure & Algorithms
n深度优先搜索的示例
7

讨论1：计算机如何实现DFS？
123456
1000000
2000000
3000000
4000000
5000000
6000000
0
0
0
0
0
0
1
2
3
4
5
6
0
1
0
0
0
0
1
1
0
0
0
0
1
1
1
0
0
0
1
1
1
0
1
0
1
1
1
1
1
0
1
1
1
1
1
1
DFS 结果
邻
接
矩
阵
A
辅助数组 visited [n ]
起点
v2→v1→v3→v5→v4→v6
——开辅助数组visited [n]！
例：
123456
1011100
2100010
3100010
4100001
5011000
6000100

Outline of DFS Algorithm
Finding neighbours of v: 
for matrix it is n, for 
adjacency list it is  
degree(v).
RDFS is called once for 
every node.
Time complexity: O(|V|+|E|) if adjacency 
lists are used. 

Recursive traversal



深度优先搜索DFS ( Depth First Search )
•算法分析
•遍历时，对图的每个顶点至多调用一次DFS函数。
•其实质就是对每个顶点查找邻接顶点的过程，取决于存储
结构。当图有e条边，其时间复杂度为O(e)，总时间复杂度
为O(n+e) 。
Data Structure & Algorithms12

深度优先搜索DFS ( Depth First Search )
•给出深度优先搜索的顺序，并绘制出对应的遍历图
Data Structure & Algorithms
深度优先搜索的顺序是a，b，c，f，d，e，g
13

深度优先搜索的应用
Data Structure & Algorithms14
DFS应用的关键：
1、构建图
2、设计用于DFS递归的函数
3、可能要记录中间信息（如路径）

Breadth first traversal
•If the traversal has just visited a vertex v, then it next 
visits all the vertices adjacent to v, putting the vertices 
adjacent to these in a waiting list to be traversed after 
all vertices adjacent to v have been visited.

•Let v be the first vertex to be visited, and w
1
, ..., w
k
 be 
the adjacent vertices of v. 
•We will first put v in the queue. 
•Visit v and put w
1
, ..., w
k
 in the queue
•Then visit w
1 
and put the adjacent vertices of w
1 
into 
the queue, ..., until all the vertices have been visited.
   Breadth first traversal can be done by using a queue. The 
head is the vertex to be visited. When it is visited, all its 
adjacent vertices are put in the queue.

广度优先搜索BFS ( Breadth First Search )
n为了实现逐层访问, 算法中使用了一个队列, 以记忆正在访
问的这一层和上一层的顶点, 以便于向下一层访问。
n为避免重复访问, 需要一个辅助数组，visited [ ]，给被访
问过的顶点加标记。
Data Structure & Algorithms17

广度优先搜索BFS ( Breadth First Search )
•广度优先搜索的示例
Data Structure & Algorithms18

Outline of BFS Algorithm
Why use queue? Need FIFO 
// flag[ ]: visited table

初始化visited
从结点v开始访问：
1.v入队；
2.当队非空时
1.取队头元素w，访问w;
2.将w所有邻接点入队；
3.对头元素出队
3.访问未被访问的结点。

Time Complexity of BFS
(Using Adjacency List)
*Assume adjacency list
nn = number of vertices   m = number of edges
Each vertex will enter Q 
at most once.
Each iteration takes time 
proportional to 1 + deg(v).
O(n + m)

Running Time
*Recall: Given a graph with m edges, what is the total 
degree?
*The total running time of the while loop is:
    this is summing over all the iterations in the while 
loop!
O( Σ
vertex v  
(1 + deg(v)) ) = O(n+m)
 Σ
vertex v  
 deg(v)  =  2m

Time Complexity of BFS
(Using Adjacency Matrix)
*Assume adjacency matrix
nn = number of vertices   m = number of edges
Finding the adjacent vertices of v 
requires checking all elements in 
the row. This takes linear time O(n).
Summing over all the n iterations, 
the total running time is O(n
2
).
O(n
2
)
So, with adjacency matrix, BFS is O(n
2
) 
independent of the number of edges m.  
With adjacent lists, BFS is O(n+m); if 
m=O(n
2
) like in a dense graph, 
O(n+m)=O(n
2
).

对比：深度优先搜索DFS ( Depth First Search )
nDFS 在访问图中某一起始顶点 v 后,  
n由 v 出发, 访问它的任一邻接顶点 w
1
; 
n再从 w
1 
出发, 访问与 w
1
邻 接但还没有访问过的顶点 w
2
; 
n然后再从 w
2 
出发, 进行类似的访问, ... 
n如此进行下去,  直至到达所有的邻接顶点都被访问过的顶点 u 为止。
n接着,  退回一步,  退到前一次刚访问过的顶点,  看是否还有其它没有被访问的
邻接顶点。
n如果有,  则访问此顶点, 之后再从此顶点出发,  进行与前述类似的访问; 
n如果没有, 就再退回一步进行搜索。
n重复上述过程, 直到连通图中所有顶点都被访问过为止。
Data Structure & Algorithms24

对比：广度优先搜索BFS ( Breadth First Search )
nBFS在访问了起始顶点 v 之后, 
n由 v 出发, 依次访问 v 的各个未被访问过的邻接顶点 w
1
, w
2
, ..., w
t
, 
n然后再顺序访问 w
1
, w
2
, ..., w
t 
的所有还未被访问过的邻接顶点。
n再从这些访问过的顶点出发，再访问它们的所有还未被访问过的邻接
顶点，... 
n如此做下去，直到图中所有顶点都被访问到为止。
n广度优先搜索是一种分层的搜索过程, 每向前走一步可能访问一批顶点, 
不像深度优先搜索那样有往回退的情况。
n因此, 广度优先搜索不是一个递归的过程。
Data Structure & Algorithms25

对比：广度优先搜索BFS ( Breadth First Search )
•搜索结果中的结点之间的关系一般可以表示成一棵树，称为解答树。搜索算法的搜索过
程，实际上就是根据初始条件和扩展规则构造一棵解答树，并寻找符合目标状态的结点
的过程。
•广度优先算法需要从初始结点出发，逐步扩展才能找到问题的解。
•如果问题有解，它总是处在解答树的某一层上。解答树在扩展过程中，随着层次的增加，
结点就越来越多，搜索的量也就迅速扩大。
•如假设一个结点衍生出来的相邻结点平均的个数是M个，当开始搜索时，队列中只有一
个起始结点，当起始点出队后，把它相邻的结点入队，那么队列就有M个结点，当下一
层的搜索中再加入元素到队列的时候，结点数达到了M的平方，一旦M较大，解答树的层
次又比较深时，则队列需要很大的内存空间，不然很容易产生数据溢出，导致搜索失败。
•因此广度优先搜索适用于结点的子结点数量不多，并且解答树的层次不太深的情况。
26Data Structure & Algorithms

深度 vs. 广度
27Data Structure & Algorithms
   图 G’=(V, T(G)) 是 G 的极小连
通子图，且 G’ 是一棵树。G’ 称
为图G的一棵生成树。
    从任意点出发按DFS算法得到
生成树 G’ 称为深度优先生成树；
   按BFS算法得到的 G’ 称为广度
优先生成树。

深度 vs. 广度
•用广度优先搜索算法遍历图与深度优先搜索算法遍历图的
区别是邻接点搜索次序不同
•广度优先搜索算法遍历图的总时间复杂度为O(n+e) 。
•图的遍历可以系统地访问图中的每个顶点，因此，图的遍
历算法是图的最基本、最重要的算法，许多有关图的操作
都是在图的遍历基础之上加以变化来实现的。
Data Structure & Algorithms28

思考：
•图的遍历和二叉树的遍历有什么异同?
•先回顾一下二叉树的遍历，树分为“左子树”“右子树”“根”三个部分，只要分
别递归遍历这三个部分，即可完成对二叉树的遍历。
•图不能像二叉树那样，把结点分类为相应的区域或部分，因此不能采用分区域遍历
的方法。图的特点是任一顶点都可能和其余的顶点相邻接，所以在访问了某个顶点
之后，可能沿着某条路径搜索之后，又回到该顶点上。只要能识别出每个顶点在遍
历过程中是否被访问过，即可达到“只访问一次”的要求。
•和二叉树的遍历类似，图的遍历是从图的某顶点出发，访问图中所有顶点，并且每
个顶点仅访问一次。
29Data Structure & Algorithms

二叉树，树，图的层次遍历
•按层次遍历，采用队列操作实现
30Data Structure & Algorithms

Case
•已知一个有向图如下图所示：
•写出该图的邻接矩阵和邻接表
•按所写的邻接表，求出从顶点D开始的深度和广度优先搜索
遍历序列
31Data Structure & Algorithms
ABCD
EH
F
G

Case
•解答
32Data Structure & Algorithms

Graph
•有向无环图及应用 
Data Structure & Algorithms33

有向无环图及其应用
•有向无环图(Directed Acycling Graph)：是图中没有回路(环)的有向图。
是一类具有代表性的图，主要用于研究工程项目的工序问题、工程时
间进度问题等。
•一个工程(project)都可分为若干个称为活动(active)的子工程(或工序)，
各个子工程受到一定的条件约束：某个子工程必须开始于另一个子工
程完成之后；整个工程有一个开始点(起点)和一个终点。人们关心：
◆ 工程能否顺利完成?影响工程的关键活动是什么?
◆ 估算整个工程完成所必须的最短时间是多少?
•对工程的活动加以抽象：图中顶点表示活动，有向边表示活动之间的
优先关系，这样的有向图称为顶点表示活动的网(Activity On Vertex 
Network ，AOV网) 。
Data Structure & Algorithms34

拓扑排序
1 定义
       拓扑排序(Topological Sort) ：由某个集合上的一个偏序得到该
集合上的一个全序的操作。
◆ 集合上的关系：集合A上的关系是从A到A的关系(AA) 。
◆ 关系的自反性：若a∈A有(a，a)∈R，称集合A上的关系R
是自反的。
◆ 关系的对称性：如果对于a，b∈A ，只要有(a，b)∈R就有(b，
a)∈R ，称集合A上的关系R是对称的。
Data Structure & Algorithms35

拓扑排序
◆ 关系的对称性与反对称性：如果对于a，b∈A ，只要有(a，b)∈R就有(b，
a)∈R ，称集合A上的关系R是对称的。如果对于a，b∈A ，仅当a=b时有
(a，b)∈R和(b，a)∈R ，称集合A上的关系R是反对称的。
◆ 关系的传递性：若a，b，c∈A，若(a，b)∈R，并且(b，c)∈R ，则(a，
c)∈R ，称集合A上的关系R是传递的。
◆ 偏序：若集合A上的关系R是自反的，反对称的和传递的，则称R是集
合A上的偏序关系。
◆ 全序：设R是集合A上的偏序关系，a，b∈A，必有aRb或bRa， 则称R
是集合A上的全序关系。 
◆ 即偏序是指集合中仅有部分元素之间可以比较，而全序是指集合中任意
两个元素之间都可以比较。
Data Structure & Algorithms36

拓扑排序
•在AOV网中，若有有向边<i, j>，则i是j的直接前驱，j是i的直接后继；推而广
之，若从顶点i到顶点j有有向路径，则i是j的前驱，j是i的后继。
•在AOV网中，不能有环，否则，某项活动能否进行，是以自身的完成作为前提
条件。
•检查方法：对有向图的顶点进行拓扑排序，若所有顶点都在其拓扑有序序列中，
则无环。
•有向图的拓扑排序：构造AOV网中顶点的一个拓扑线性序列(v’
1
,v’
2
, ⋯,v’
n
)，使
得该线性序列不仅保持原来有向图中顶点之间的优先关系，而且对原图中没有
优先关系的顶点之间也建立一种（人为的）优先关系。
•一个有向图能被拓扑排序的充要条件就是它是一个有向无环图（Directed Acyclic 
Graph，DAG）。这样的图的特点是不能有环路存在。
Data Structure & Algorithms37
注：顶点表示活动的网 (Activity On Vertex Network ，AOV网) 

拓扑排序算法
2 算法思想
① 在AOV网中选择一个没有前驱的顶点且输出； 
② 在AOV网中删除该顶点以及从该顶点出发的(以该顶点为尾的弧)
所有有向弧(边) ；
③ 重复①、②，直到图中全部顶点都已输出(图中无环)或图中不存
在无前驱的顶点(图中必有环)。
Data Structure & Algorithms38
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
v
6
(a)  有向图
(b)  输出v
1
后
v
4
v
2
v
3
v
5
v
6
有向图的拓扑排序过程，其拓扑序列是： (v1,v6,v4,v3,v2,v5)
(c)  输出v
6
后
v
4
v
2
v
3
v
5
(d)  输出v
4
后
v
2
v
3
v
5
(e)  输出v
3
后
v
2
v
5

拓扑排序算法实现说明
•算法实现
◆ 采用正邻接链作为AOV网的存储结构；
◆ 设立堆栈（或队列），用来暂存入度为0的顶点；
◆ 删除顶点以它为尾的弧：弧头顶点的入度减1。
•算法分析：设AOV网有n个顶点，e条边，则算法的主要执行是：
   ◆ 统计各顶点的入度：时间复杂度是O(n+e) ；
       ◆ 入度为0的顶点入栈：时间复杂度是O(n) ；
       ◆ 排序过程：顶点入栈和出栈操作执行n次，入度减1的操作共执行e
次，时间复杂度是O(n+e) ；
             因此，整个算法的时间复杂度是O(n+e) 。
Data Structure & Algorithms39

Assume the following specification



•The running time for both methods is O(n+e), n is the 
number of nodes and e is the number of edges. This 
is because we visit each node and edge once.

Count the number of 
predecessors
Nodes without predecessors are put 
into the queue and ready to traverse
Decrease the number of 
predecessors of v when it is 
traversed. Put any node without 
successors into the queue.

����
-
-)(deg
-
-)(deg
)(
1
0=
1
0=
1
0=
1
0=
3+2=13+2=3+2
n
v
vree
j
n
v
vree
j
enn
n, the number of 
vertices
e, the number of edges
The queue ready_to_process has at most n 
vertices

关键路径 (Critical Path)
•与AOV网相对应的是AOE (Activity On Edge) ，是边表示活动的有向无
环图，如图所示。图中顶点表示事件(Event)，每个事件表示在其前的
所有活动已经完成，其后的活动可以开始；弧表示活动，弧上的权值
表示相应活动所需的时间或费用。
Data Structure & Algorithms45
v
0
v
6
v
5
v
4
v
3
v
2
v
1
v
7
v
8
a1=3
a2=10
a3=9
a4=13
a5=12
a6=7
a7=8
a9=6
a10=11
a12=5
a8=4
a11=2
图7-24  一个AOE网

关键路径 (Critical Path)
•与AOE有关的研究问题
◆ 完成整个工程至少需要多少时间?
◆ 哪些活动是影响工程进度(费用)的关键?
    工程完成最短时间：从起点到终点的最长路径长度(路径上各活动持续时
间之和) 。长度最长的路径称为关键路径，关键路径上的活动称为关键活动。
关键活动是影响整个工程的关键。
        设v
0
是起点，从v
0
到v
i
的最长路径长度称为事件v
i
的最早发生时间，即是
以v
i
为尾的所有活动的最早发生时间。
Data Structure & Algorithms46

关键路径 (Critical Path)
 若活动ai是弧<j, k>，持续时间是dut(<j, k>)，设：
◆ e(i)：表示活动ai的最早开始时间；
◆ l(i)：在不影响进度的前提下，表示活动a
i
的最晚开始时间； 则l(i)-e(i)
表示活动a
i
的时间余量，若l(i)-e(i)=0，表示活动a
i
是关键活动。
◆ ve(i)：表示事件v
i
的最早发生时间，即从起点到顶点v
i
的最长路径长度； 
◆ vl(i)：表示事件v
i
的最晚发生时间。则有以下关系：
Data Structure & Algorithms47
e(i) = ve(j)
l(i) =  vl(k)-dut(<j, k>)
（1）

关键路径 (Critical Path)
含义是：源点事件的最早发生时间设为0；除源点外，只有进入顶点v
j
的所有弧所代表的活
动全部结束后，事件v
j
才能发生。即只有v
j
的所有前驱事件v
i
的最早发生时间ve(i)计算出来
后，才能计算ve(j) 。
方法是：对所有事件进行拓扑排序，然后依次按拓扑顺序计算每个事件的最早发生时间。
含义是：只有v
j
的所有后继事件v
k
的最晚发生时间vl(k)计算出来后，才能计算vl(j) 。
方法是：按拓扑排序的逆顺序，依次计算每个事件的最晚发生时间。
Data Structure & Algorithms48
ve(n-1)    j=n-1，表示v
j
是终点
Min{vl(k)-dut(<j, k>)|<v
j
, v
k
>是网中的弧}
vl(j) = 
（3）
0    j=0，表示v
j
是起点
Max{ve(i)+dut(<i, j>)|<v
i
, v
j
>是网中的弧}
ve(j) = 
（2）

求AOE中关键路径和关键活动
•算法思想
① 利用拓扑排序求出AOE网的一个拓扑序列； 
②  从拓扑排序的序列的第一个顶点(源点)开始，按拓扑顺序依次计算每个
事件的最早发生时间ve(i) ； 
③  从拓扑排序的序列的最后一个顶点(汇点)开始，按逆拓扑顺序依次计算
每个事件的最晚发生时间vl(i) ；
         对于图7-24的AOE网，处理过程如下：
◆  拓扑排序的序列是： (v
0
, v
1
, v
2
, v
3 
,
 
v
4
, v
5 
, v
6 
, v
7 
, v
8
)
Data Structure & Algorithms49
v
0
v
6
v
5
v
4
v
3
v
2
v
1
v
7
v
8
a1=3
a2=10
a3=9
a4=13
a5=12
a6=7
a7=8
a9=6
a10=11
a12=5
a8=4
a11=2

求AOE中关键路径和关键活动
◆ 根据计算ve(i)的公式(2)和计算vl(i)的公式(3) ，计算各个事件的ve(i)和vl(i)值，如下
表所示。
◆ 根据关键路径的定义，知该AOE网的关键路径是： (v
0
, v
2
, v
4
, v
7
 , v
8
) 和(v
0
, v
2
, v
5
 , v
7
 , 
v
8
) 。
关键路径活动是：<v
0
, v
2
>，<v
2
, v
4
>，<v
2
, v
5
>，<v
4
, v
7
>，<v
5
, v
7
>，<v
5
, v
8
> 。
Data Structure & Algorithms50
工程完成最短时间：从起点到终点的最长路径长度(路径上各活动持续时间之和) ，长度最长的路径称为关键路径。

求AOE中关键路径和关键活动
•算法实现
Data Structure & Algorithms51
void critical_path(ALGraph *G)
{  int j, k, m ; LinkNode *p ;
if  (Topologic_Sort(G)==-1)
printf(“\nAOE网中存在回路，错误!!\n\n”) ;
else
{  for ( j=0; j<G->vexnum; j++)
    ve[j]=0 ;     /*  事件最早发生时间初始化  */
for (m=0 ; m<G->vexnum; m++)
    {   j=topol[m] ;
        p=G->adjlist[j].firstarc ;
        for (; p!=NULL; p=p->nextarc )
         {  k=p->adjvex ;
                 if  (ve[j]+p->weight>ve[k])
                       ve[k]=ve[j]+p->weight ; 
             }
    }     /* 计算每个事件的最早发生时间ve值  
*/
for ( j=0; j<G->vexnum; j++)
    vl[j]=ve[j] ;    /*  事件最晚发生时间初始化  */
for (m=G->vexnum-1; m>=0; m--)
    {   j=topol[m] ; p=G->adjlist[j].firstarc ;
        for (; p!=NULL; p=p->nextarc ) 
             {  k=p->adjvex ;
                 if  (vl[k]-p->weight<vl[j])
                     vl[j]=vl[k]-p->weight ;
 }
    }    /* 计算每个事件的最晚发生时间vl值  */
for (m=0 ; m<G->vexnum; m++)
    {   p=G->adjlist[m].firstarc ;
         for (; p!=NULL; p=p->nextarc ) 
              {  k=p->adjvex ;
                  if  ( (ve[m]+p->weight)==vl[k])
                       printf(“<%d, %d>, m, j”) ;
               }
     }     /* 输出所有的关键活动  */
   }     /*  end of else  */
}

求AOE中关键路径和关键活动
(3) 算法分析
   设AOE网有n个事件，e个活动，则算法的主要执行是：
◆ 进行拓扑排序：时间复杂度是O(n+e)
◆ 求每个事件的ve值和vl值：时间复杂度是O(n+e) 
◆ 根据ve值和vl值找关键活动：时间复杂度是O(n+e) 
        因此，整个算法的时间复杂度是O(n+e) 。
Data Structure & Algorithms52
