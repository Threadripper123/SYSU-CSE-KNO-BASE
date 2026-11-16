# 10-Graph2-图集

> 主题库：数据结构与算法　｜　源文件：10-Graph2-图集.pdf　｜　5,225 字符　｜　26 个片段　｜　更新于 2026-11-11
代理 m

A A B A A B B C C O B C C D

1 2 3 1 2 3 7 の C G 4 7 C G 4 深度优先搜索过程深度优先生成树

Algorithm DFS(s 1.   for each vertexv  2. do flag[u]:= false; 3. RDFS(s); Algorithm RDFS(u) 1. fag[u := true; 2. for each neighbor w of u 3. do ifflag[w]= false 4. thenRDFS(w);

template<int max_size> void Digraph<max_size>::depth_first(void(*visit)(Vertex&))const / Post: The function *visit has been performed at each vertex of the Digraph in depth-frstorde Uses: Method traverseto producetherecursive depth-firstorder. * boolvisited[max_size]; VertexV; for(allvinG)visited[v]=false; for(allvinG)if(!visited[v]) traversev,visited,visi);

void Digraph<max_size>::traverse(Vertex&v, boolvisited[], void *visit)(Vertex &)) const /* Pre: v is a vertex of the Digraph. Post: The depth-first traversal, using function * visit, has been completed for V and for all vertices that can bereached from v. Uses:traverserecursively. * E visited[v]= true; for(allw adjacent tov) if(!visited[w]) traverse(w,visited,visit);

Start 0  4 Breadth-first traversal

广度优先搜業定程广度优先生成树

Algorithm BFS(s) Input:sisthesource vertex Output: Mark all vertices that can be visited from s. 1. for each vertexu 2. do flag[u]:= false; 3. Q = empty queue; 4. fag[s := true; 5. enqueue(Q,s); 6. while Q isnotempty 7. dov:= dequeue(Q); 8. for each w adjacent to v 9. do ifflag[w]= false 10. then flag[w]:= true; 11. enqueue(Q,w)

void Digraph<max_size>::breadth_first(void (*visit)(Vertex &))const /* Post: The function *visit has been performed at each vertex of the Digraph in breadth-firstorder. Uses:MethodsofclassQueue.* E Queue q; boolvisited[max_size]; for(allvinG)visited[v]=false; for (all v in G) q.append(v); while(!q.empty()){ q.retrieve(w); if(!visited[w]){ visited[w ] = true; cavnait w); for(all x adjacent to w) q.append(x); q.serve(); 子

A E C

F B A D C E H

current = QulgetFront )) OulDeoueue (O) visit () Ou EnOueuie ( currentl

順將以首结点的左登子給点入以列 f(adeuenrona-Ishid NUL) 队首经点的右孩子給点入队列 f ((cDeoetrona-a rcnior-NuLt)

qlEnQtene(w  tnQuete (rd

dataadj dest link B 1 2      邻接表出边表)

1 A.edge= 1 0 1 0 0 0

（1）有向图共有8个顶点，10条边，所以邻接矩阵是一个88的矩阵，其中有10 个元素为1。在邻接表表示中，表向量大小为8，所有顶点邻接表的结点数为10。该图的邻接矩阵与邻接表如图7-11所示。 4 0 00001000 B 4 0 10001000 1     01000000 2 C 1   00100001  3 7 2  00000000 4 E 00001000 年，年入 F 4 00000101 5 00000100 G  7 + 5    定区  H 图 7-11

（2）按邻接表从顶点D开始的深度优先序列为：DHFECBAG； 广度优先序列为：DHCFBEAG。

cYaa _ public: Digraph(); voidread(); voidwrite(); W methodstodo a-topoloqicalsort voiddepth_sort(List<Vertex>&topological_order); voidbreadth_sort(List<Vertex>&topological_order); private: intcount; List<Vertex> neighbors[graph_size]; voidrecursive_depth_sort(Vertexv,boolvisited[], List<Vertex>&topological_order);

0 1

void Digraph<graph_size>::recursive_depth_sort(Vertexv,bool*visited, List<Vertex>&topological_order / Pre: Vertexv of the Digraph doesnot belong to the partiallycomoleted List topological_order. Post: All the successors ofv and finallyv itself are added to topological_order with a depth-first search. Uses: Methods ofclass List amd the functionlrecursive_ depth_sort.*/ visited[v]=true; intdegree = neighbors[v].size(); for(inti=0;i<degree;i++){ Vertex w ; I A (neighboring) successor ofv if(!visited[w ]) W/ Orderthe suiccessorsof w recursive_depth_sort(w,visited,topological_order); topological_order.insert(o, v); Putvintotopological_ordel.

template <intgraph_size> void Digraph<graph_size>::depth_sort(List<Vertex>&topological_order) / Post: The vertices of the Digraph are placedinto List topological_order withia depth-first traversalofthiose yertices that do not belong-to a cycle. Uses: MlethodsofclassList,andfunctionrecursive_depth_sort toperformdepth- firsttraversal. l boolvisited[graph_size]; Vertexv; for(v = O; v <count; v++)visited[v] = false; for(v=O;v cOunt;v++) if(!visited[vl) W Addv.andits successorsinto(topolagical order recursive_depth_sort(v,visited,topological_order:

template<intgraph_size>voidDigraph<graph_size>: breadth_sort(List< Vertex> &topological_order) /* Post: The vertices of the Digraph are arranged into the Listtopological_order which is found with a breadth-firsttraversal of those vertices that do not belong to a cycle. Uses:Methods of classesQueue and List.*/ {topological_order.clear(); Vertex V, w; an predecessor_countlgraph_sizel; for(v=0;v<count;v++)predecessor_count[v]=0; for(v=0;V<count;v++) for(inti=0;i<neighbors[v].size();i++){ neighbors[v].retrieve(i, w); II Loop over all edgesv — w. predecessor_count[w]十十; Queueready_to_process; for(V=O;V<count;V++) if(predecessor_count[v]== 0) ready_toprocess.append(v); while (!ready_to_process.empty()){ ready_to_process.retrieve(v); topological_order.insert(topological_order.size(), v); for(intj=0;j<neighbors[v].size();j++){ neighbors[v].retrieve(j, w); II Traverse successors ofv. predecessor_count[w]——; if(predecessor_count[w] == 0) ready_to_process.append(w); ready_to_process.serve(); 子子

例：  1 2345  辅助数组viste [n1  1 0 1  0 0 0 0 1 11   1 1 1  0 0 0 0 2 0 1 1 1 1 1 1  0 0 0 0 3 0 0 0 1 1 1 1 4 1 0 0 0 0 4 0 0 0 0 0 1 1 5 0 0 0 0 5 0 0 0 0 1 1 1 6 0 0 0 1 0 0 0 0 0 0 0 0 1 v2 >v1—→v3—>v5—→v4—>v6

CaSE • 已知一个有向图如下图所示： A B 7 1 A.dge= 2 H dataadj dest link A destlink B B  G 邻接表(出边表) •写出该图的邻接矩阵和邻接表 •按所写的邻接表，求出从顶点D开始的深度和广度优先搜索遍历序列
