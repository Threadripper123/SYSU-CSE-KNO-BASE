# Chapter5 Network Layer - 2.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter5 Network Layer - 2.zh-CN.pdf　｜　70,950 字符　｜　128 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
计算机网络
2024. 秋季

5-2
Computer Networking: A 
Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
Chapter 5
Network Layer:
Control Plane
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, readers). They’re in 
PowerPoint form so you see the animations; and can add, modify, and delete slides  
(including this one) and slide content to suit your needs. They obviously represent a lot
of work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention their source (after all, 
we’d like people to use our book!)

If you post any slides on a www site, that you note that they are adapted from (or 
perhaps identical to) our slides, and note our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
计算机 网络:
Down 方法
第 5 章
网络层:
控制平面
说明 在 使用 的 这些 PowerPoint 幻灯片:
我们’
re making 这些 幻灯片 freely 可用的 到 所有 (faculty, students, readers). They’re 在
PowerPoint form 因此 你 见 animations; 和 可以 添加, 修改, 和 删除 幻灯片
(包括 这个 一个) 和 幻灯片 内容 到 suit your needs. They obviously represent a
的 工作 在 我们的 part. 在 返回 用于 使用, 我们 仅 ask following:
如果 你 使用 这些 幻灯片 (e.g., 在 a 课堂) 这 你 mention 它们的 源 (之后 所有,
我们’
d 像 people 到 使用 我们的 book!)
如果 你 post 任何 幻灯片 在 a 网站 站点, 这 你 说明 这 they 是 adapted 从 (或
perhaps identical 到) 我们的 幻灯片, 和 说明 我们的 版权 的 这个 material.
用于 a revision 历史, 见 幻灯片 说明 用于 这个 page.
谢谢 和 使用愉快!
所有 材料 版权 1996
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved

Assignments :

Ch5 (ver8, cn): 3, 7, 11,14, 16, 19

A team project

Keywords: Forwarding and Routing, Virtual Circuit and Datagram 
Networks, Addressing, DHCP, NAT, ICMP, Routing Algorithms (DV, 
LS), RIP, OSPF, BGP 
作业：
团队项目
关键词： 转发 和 路由, Virtual 电路 和 数据报
网络, 编址, DHCP, NAT, ICMP, 路由 算法 (DV,

Chapter 5: network layer control plane
chapter goals:
understand principles behind network 
control plane

traditional routing algorithms

SDN controllers

Internet Control Message Protocol

network management
and their instantiation, implementation in the Internet:

OSPF, BGP, OpenFlow, ODL and ONOS controllers, ICMP, 
SNMP
5-4
第 5 章：网络层控制平面
本章目标：
understand 原理 背后的 网络
控制平面
传统路由算法
SDN 控制器
Internet 控制报文协议
网络管理
和 它们的 实例化, 实现 在 Internet:
OSPF, BGP, OpenFlow, ODL 和 ONOS 控制器, ICMP,

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-5
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

Network-layer functions

forwarding:move packets 
from router’s input to 
appropriate router output
data plane
controlplane
Two approaches to structuring network control plane:
per-router control (traditional)
logically centralized control (software defined networking)
Recall: two network-layer functions:
5-6
routing:determine route taken by 
packets from source to destination
Given a forwarding table
Create a routing table 
for  forwarding
网络层 功能
转发:移动 分组
从 路由器s 输入 到
合适的 路由器 输出
数据平面
控制平面
两个 方法 到 构建 网络 控制平面:
每路由器 控制 (传统的)
逻辑集中式控制 (软件定义网络)
回顾: 两个 网络层 功能:
路由确定 路由 经过 由
分组 从 源 到 目的地
给定的 a 转发表
Create a 路由表
用于
转发

Per-router control plane
Routing
Algorithm
Individual routing algorithm components in each and every router 
interact with each other in control plane to compute forwarding tables
data
plane
control
plane
5-7
每路由器 控制平面
路由
算法
Individual 路由 算法 components
在 每个 和 每一个 路由器
interact 与 每个 其他 在 控制平面 到 计算 转发表s
数据
平面
控制
平面

data
plane
control
plane
Logically centralized control plane
A distinct (typically remote) controller interacts with local 
control agents (CAs) in routers to compute forwarding tables
Remote Controller
CA
CA
CA
CA
CA
5-8
数据
平面
控制
平面
Logically centralized 控制平面
distinct (通常 远程) 控制器 interacts 与 本地
控制 agents (CAs) 在 路由器 到 计算 转发表s
远程 控制器

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-9
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

Routingprotocols
Routing protocol goal:
determine “good” paths (equivalently, 
routes), from sending hosts to receiving host, through network of 
routers

path: sequence of routers packets will traverse in going from 
given initial source host to given final destination host

“good”: least “cost”, “fastest”, “least congested”

routing: a “top-10” networking challenge!
5-10
路由协议
路由协议目标:
确定“好的”路径 (equivalently,
路由), 从 发送 主机 到 接收 主机, 通过 网络 的
路由器
路径: 序列 的 路由器 分组 将 经过 在 going 从
给定的 初始 源 主机 到 给定的 最终 目的地 主机
“好的”: 最少的 “
费用最快的
”, “最少的
拥塞的
路由: a “top10” 网络 挑战!

4-11
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
graph: G = (N,E)
N = set of routers = { u, v, w, x, y, z }
E = set of links ={ (u,v), (u,x), (v,x), (v,w), (x,w), (x,y), (w,y), (w,z), (y,z) }
Graph abstraction
aside:graph abstraction is useful in other network contexts, e.g., 
P2P, where Nis set of peers and Eis set of TCP connections
图: G = (N,E)
E = 集合 的 链路 ={ (
图抽象
图抽象 是 有用 在 其他 网络 场景, e.g.,
P2P, 哪里
是 集合 的 对等方 和
是 集合 的 TCP 连接

4-12
Graph abstraction: costs
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
c(x,x’) = cost of link (x,x’)
e.g., c(w,z) = 5
cost could always be 1, or 
inversely related to bandwidth,
or inversely related to 
congestion
cost of path (x
1
, x
2
, x
3
,..., x
p
) = c(x
1
,x
2
) + c(x
2
,x
3
) + ... + c(x
p-1
,x
p
)  
key question:
what is the least-cost path between u and z ?
routing algorithm:
algorithm that finds that least cost path
图抽象: 费用
) = 链路费用 (
费用 可以 总是 是 1, 或
成反比 相关 到 带宽
或成反比 相关 到
拥塞
路径费用 (x
关键 问题:
什么 是 最少的费用 路径 之间u 和 z
路由 算法:
算法 这 finds 这 最少的 费用 路径

Routing algorithm classification
Network Layer: 5-13
global: 
all routers have complete
topology, link cost info
•“link state” algorithms
decentralized: 
iterative process of computation, 
exchange of info with neighbors
•routers initially only know link costs to attached 
neighbors
•“distance vector” algorithms
dynamic
: routes 
change more quickly
•periodic updates or in 
response to link cost 
changes
static: 
routes change 
slowly over time
global or decentralized information?
How fast do 
routes change?
路由算法分类
网络层: 5
全局:
所有 路由器 有完整的
拓扑, 链路费用 信息
“链路状态”
算法
分散式:
迭代式 进程 的 computation,
交换 的 信息 与邻居
路由器 初始时 仅 know 链路费用s 到 attached
邻居
“距离向量”算法
动态
路由
变化 更多 快速地
周期性更新s 或 在
响应 到 链路费用
变化
静态:
路由 变化
缓慢地 在 时间
全局信息还是分散信息?
如何 快速 做
路由 变化?

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-14
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

4-15
A Link-State Routing Algorithm
Dijkstra’salgorithm

net topology, link costs known to all 
nodes

accomplished via “link state broadcast”

all nodes have same info

computes least cost paths from one 
node (‘source”) to all other nodes

gives forwarding tablefor that node

iterative: after k iterations, know 
least cost path to k dest.’s
notation:

c(x,y):
link cost from node x to y;  
= ∞ if not direct neighbors

D(v):
current value of cost of path 
from source to dest. v

p(v):
predecessor node along path 
from source to v

N':
set of nodes whose least cost 
path definitively known
链路状态 路由 算法
算法
网络 拓扑, 链路费用s 已知 到 所有
节点
accomplished 通过链路状态广播
所有 节点有 相同 信息
计算 最少的 费用 路径 从 一个
节点 (源) 到 所有 其他 节点
给出转发表用于 这 节点
迭代式: 之后 k 迭代, know
最少的 费用 路径 到 k
记号:
链路费用 从 节点 x 到 y;
= ∞ 如果 不 直接 邻居
当前 值 的 路径费用
从 源 到
前驱 节点 along 路径
从 源 到 v
集合 的 节点 whose 最少的 费用
路径 definitively 已知

4-16
Dijsktra’s Algorithm
1  Initialization:
2    N'= {u} 
3    for all nodes v 
4      if v adjacent to u 
5          then D(v) = c(u,v) 
6      else D(v) = ∞
7 
8   Loop
9     find w not in N'such that D(w) is a minimum 
10    add w to N'
11    update D(v) for all v adjacent to w and not in N': 
12       D(v) = min( D(v), D(w) + c(w,v) ) 
13    /* new cost to v is either old cost to v or known 
14     shortest path cost to w plus cost from w to v */ 
15  until all nodes in N'
s 算法
用于 所有 节点 v
如果 v adjacent 到 u
则 D(v) = c(
否则 D(v) =
find w 不 在 N
such 这 D(w) 是 a minimum
添加 w 到 N
更新 D(v) 用于 所有 v adjacent 到 w 和 不 在 N
D(v) = 最小值( D(v), D(w) + c(
/* 新的 费用 到 v 是 either old 费用 到 v 或 已知
shortest 路径 费用 到 w 加 费用 从 w 到 v */
直到 所有 节点 在 N

4-17
w
3
4
v
x
u
5
3
7
4
y
8
z
2
7
9
Dijkstra’salgorithm: example
Step
N'
D(v),
p(v)
0
1
2
3
4
5
D(w),
p(w)
D(x),
p(x)
D(y),
p(y)
D(z),
p(z)
u
∞ 
∞ 
7,u
3,u
5,u
uw
∞ 
11
,w
6,w
5,u
14
,x 
11,
w 
6,w
uwx
uwxv
14
,x 
10,
v 
uwxvy
12
,y 
notes:

construct shortest path tree by tracing 
predecessor nodes

ties can exist (can be broken arbitrarily)
uwxvyz
Find the least cost paths between u and other nodes
算法: 示例
步骤
说明:
构造 shortest 路径 tree 由 追踪
前驱 节点
平局 可以 存在 (可以 是 打破 任意地)
最少的 费用 路径 之间 u 和 其他 节点

4-18
Dijkstra’s algorithm: another example
Step
0
1
2
3
4
5
N'
u
ux
uxy
uxyv
uxyvw
uxyvwz
D(v),p(v)
2,u
2,u
2,u
D(w),p(w)
5,u
4,x
3,y
3,y
D(x),p(x)
1,u
D(y),p(y)
∞
2,x
D(z),p(z)
∞ 
∞ 
4,y
4,y
4,y
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
u
y
x
wv
z
20241117
s 算法: 另一个 示例
步骤

4-19
Dijkstra’s algorithm: example (2) 
u
y
x
wv
z
resulting shortest-path tree from u:
v
x
y
w
z
(u,v)
(u,x)
(u,x)
(u,x)
(u,x)
destination
link
resulting forwarding table in u:
s 算法: 示例 (2)
得到的 shortest
路径 tree 从 u:
目的地
链路
得到的 转发表 在 u:

4-20
Dijkstra’s algorithm, discussion
algorithm complexity:n nodes

each iteration: need to check all nodes, w, not in N

n(n+1)/2 comparisons: O(n
2
)

more efficient implementations possible: O(nlogn)
oscillations possible:

e.g., support link cost equals amount of carried traffic:
A
D
C
B
1
1+x
x
0
x
1
1
0
0
initially
A
D
C
B
given these costs,
find new routing....
resulting in new costs
2+x
0
0
0
1+x
1
A
D
C
B
given these costs,
find new routing....
resulting in new costs
0
2+x
1+x
1
0
0
A
D
C
B
given these costs,
find new routing....
resulting in new costs
2+x
0
0
0
1+x
1
s 算法, 讨论
算法复杂度:n 节点
每个 迭代: 需要 到 查看 所有 节点, w, 不 在 N
n(n+1)/2 比较: O(n
更多 高效的 implementations 可能: O(
可能发生振荡:
e.g., 支持 链路费用 equals 数量 的 承载的 流量:
初始时
给定的 这些 费用,
find 新的 路由....
得到的 在 新的 费用
给定的 这些 费用,
find 新的 路由....
得到的 在 新的 费用
给定的 这些 费用,
find 新的 路由....
得到的 在 新的 费用

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-21
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

4-22
Distance vector algorithm 
Bellman-Ford equation (dynamic programming)
let   d
x
(y) := cost of least-cost path from x to y
then
d
x
(y) = min{c(x,v)+ d
v
(y) }
v
cost to neighborv
mintaken over all neighborsv of x
cost from neighbor v 
to destination y
邻居能去到的，自己一定能通过邻居到达；
多个邻居能到的，找一个总代价最小的邻居作为下一跳；
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
距离向量 算法
Ford equation (动态规划)
(y) := 费用 的 最少的
费用 路径 从 x 到 y
则
最小值
费用 到
邻居
最小值
经过 在 所有
邻居
v 的 x
费用 从 邻居 v
到 目的地 y

4-23
Bellman-Ford example 
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
v, x, w are u’s neighbors, d
v
(z) = 5, d
x
(z) = 3, d
w
(z) = 3
d
u
(z) = min { c(u,v) +  d
v
(z),
c(u,x) +  d
x
(z),
c(u,w) + d
w
(z) }
= min {2 + 5,
1 + 3,
5 + 3}  = 4
node achieving minimum is next
hop in shortest path, used inforwarding table
B-F equation says:
Ford 示例
v, x, w 是 u’s 邻居, d
(z) = 最小值 { c(
= 最小值 {2 + 5,
节点 achieving minimum 是 下一个
跳 在 shortest 路径, 用于 在
转发表

4-24
Distance vector algorithm 

d
x
(y)= estimate of least cost from x to y
d
x
(y) = min{c(x,v)+ d
v
(y) }

x maintains  distance vector d
x
= [d
x
(y): y єN ]

node x:

knows cost to each neighbor vby detecting: c(x,v)

maintains its neighbors’distance vectors. For each neighbor v, x 
maintains 
d
v
= [d
v
(y): y єN ]
u
y
x
wv
z
2
2
1
3
1
1
2
5
3
5
v
How to obtain d
v
(y), 
y ∈N ?
距离向量 算法
= 估计 的 最少的 费用 从 x 到 y
最小值
x 维护 距离向量
节点 x:
知道 费用 到 每个邻居 v由 detecting:
维护 它的 邻居距离向量s. 用于 每个 邻居 v, x
维护
如何 到 obtain d

4-25
key idea:

from time-to-time, each node sends its own distance vector
estimate to neighbors

when x receives new DV estimate from neighbor, it updates 
its own DV using B-F equation:
D
x
(y) ← min
v
{c(x,v) + D
v
(y)}  for each node y ∊N

under minor, natural conditions, the estimate D
x
(y) converge to the 
actual least cost d
x
(y)
Distance vector algorithm 
关键 思想:
从 时间到时间, 每个 节点发送 它的 自己的 距离向量
估计 到邻居
当 x 接收 新的 DV 估计 从邻居
, 它 updates
它的 自己的 DV 使用 B
最小值
(y)} 用于 每个 节点 y
under 较小的, 自然的 条件, 估计(y) 收敛 到
实际的 最少的 费用
距离向量 算法

4-26
iterative, asynchronous:
each local 
iteration caused by: 

local link cost change 

DV update message from neighbor
distributed:

each node notifies neighbors onlywhen 
its DV changes

neighbors then notify their neighbors if 
necessary
wait
for (change in local link 
cost or msgfrom neighbor)
recompute
estimates
if DV to any desthas 
changed, 
notify
neighbors 
each node:
Distance vector algorithm 
迭代式, 异步:
每个 本地
迭代 caused 由:
本地 链路费用 变化
DV 更新报文 从 邻居
分布式:
每个 节点 通知 邻居仅当
它的 DV 变化
邻居 则 通知 它们的 邻居 如果
必要时
等待
用于 (变化 在 本地 链路
费用 或
从 邻居)
重新计算
估计
如果 DV 到 任何
有
通知
邻居
每个 节点:
距离向量 算法

DV 
D
a
(a)=0
D
a
(b) = 8
D
a
(c) =∞
D
a
(d) =1
D
a
(e) =∞
D
a
(f) =∞
D
a
(g) =∞
D
a
(h) =∞
D
a
(i) =∞
Distance vector: example
Network Layer: 5-27
g
hi
1
1
1
11
1
1
1
1
8
1
t=0
All nodes have 
distance 
estimates to 
nearest 
neighbors (only)
A few asymmetries:
missing link
larger cost
d
ef
a
b
c
All nodes send 
their local 
distance vector 
to their 
neighbors
距离向量: 示例
网络层: 5
所有 节点 有
距离
估计 到
邻居 (仅)
missing 链路
larger 费用
所有 节点 发送
它们的 本地
距离向量
到 它们的
邻居

Distance vector example: iteration
Network Layer: 5-28
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=1
g
hi
1
1
1
11
1
1
1
1
8
1
d
ef
a
b
c
距离向量 示例: 迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的 新的
本地 距离
向量 到
邻居

Distance vector example: iteration
Network Layer: 5-29
g
h
i
1
1
11
1
1
1
1
1
8
1
d
e
f
a
b
c
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=1
compute
compute
compute
compute
compute
compute
compute
compute
compute
距离向量 示例: 迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的 新的
本地 距离
向量 到
邻居
计算
计算
计算
计算
计算
计算
计算
计算
计算

Distance vector example: iteration
Network Layer: 5-30
g
hi
1
1
1
11
1
1
1
1
8
1
d
ef
a
b
c
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=1
距离向量 示例:迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的
新的
本地 距离
向量 到
邻居

Distance vector example: iteration
Network Layer: 5-31
g
hi
1
1
1
11
1
1
1
1
8
1
d
ef
a
b
c
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=2
距离向量 示例: 迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的 新的
本地 距离
向量 到
邻居

Distance vector example: iteration
Network Layer: 5-32
g
h
i
1
1
11
1
1
1
8
1
2
1
d
e
f
a
bc
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=2
compute
compute
compute
compute
compute
compute
compute
compute
compute
距离向量 示例: 迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的 新的
本地 距离
向量 到
邻居
计算
计算
计算
计算
计算
计算
计算
计算
计算

Distance vector example: iteration
Network Layer: 5-33
g
hi
1
1
1
11
1
1
1
1
8
1
d
ef
a
b
c
All nodes:
receive distance 
vectors from 
neighbors
compute their 
new local  
distance vector
send their new 
local distance 
vector to 
neighbors
t=2
距离向量 示例:迭代
网络层: 5
所有 节点:
接收 距离
vectors 从
邻居
计算 它们的
新的 本地
距离向量
发送 它们的 新的
本地 距离
向量 到
邻居

Distance vector example: iteration
Network Layer: 5-34
.... and so on
Let’s next take a look at the iterative computationsat nodes
距离向量 示例:迭代
网络层: 5
.... 和 因此 在
Let’s 下一个 take a look 在 迭代式
在 节点

DV in a: 
D
a
(a)=0
D
a
(b) = 8
D
a
(c) =∞
D
a
(d) =1
D
a
(e) =∞
D
a
(f) =∞
D
a
(g) =∞
D
a
(h) =∞
D
a
(i) =∞
Distance vector example: computation
Network Layer: 5-35
g
hi
1
1
1
11
1
1
1
1
8
1
t=1
DV in b:
D
b
(f) =∞
D
b
(g) =∞
D
b
(h) =∞
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =∞
D
b
(e) =1
b receives DVs 
from a, c, e
a
b
c
d
ef
DV in c:
D
c
(a) =∞
D
c
(b) =1
D
c
(c) = 0
D
c
(d) =∞
D
c
(e) =∞
D
c
(f) =∞
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =∞
DV in e:
D
e
(a) =∞
D
e
(b) =1
D
e
(c) =∞
D
e
(d) =1
D
e
(e) = 0
D
e
(f) =1
D
e
(g) =∞
D
e
(h) =1
D
e
(i) =∞
DV 在 a:
距离向量 示例: computation
网络层: 5
DV 在 b:
b 接收 DVs
从 a, c, e
DV 在 c:
DV 在 e:

Distance vector example: computation
DV in a: 
D
a
(a)=0
D
a
(b) = 8
D
a
(c) =∞
D
a
(d) =1
D
a
(e) =∞
D
a
(f) =∞
D
a
(g) =∞
D
a
(h) =∞
D
a
(i) =∞
DV in b:
D
b
(f) =∞
D
b
(g) =∞
D
b
(h) =∞
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =∞
D
b
(e) =1
DV in c:
D
c
(a) =∞
D
c
(b) =1
D
c
(c) = 0
D
c
(d) =∞
D
c
(e) =∞
D
c
(f) =∞
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =∞
DV in e:
D
e
(a) =∞
D
e
(b) =1
D
e
(c) =∞
D
e
(d) =1
D
e
(e) = 0
D
e
(f) =1
D
e
(g) =∞
D
e
(h) =1
D
e
(i) =∞
Network Layer: 5-36
g
h
i
1
1
11
1
1
1
1
1
8
1
t=1
b receives DVs 
from a, c, e, 
computes:
a
bc
d
e
f
DV in b:
D
b
(f) =2
D
b
(g) =∞
D
b
(h) =2
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =2
D
b
(e) =1
e
compute
b
D
b
(a) = min{c
b,a
+D
a
(a), c
b,c 
+D
c
(a), c
b,e
+D
e
(a)}  = min{8,∞,∞} = 8 
D
b
(c) = min{c
b,a
+D
a
(c), c
b,c 
+D
c
(c), c
b,e 
+D
e
(c)}  = min{∞,1,∞} = 1 
D
b
(d) = min{c
b,a
+D
a
(d), c
b,c 
+D
c
(d), c
b,e 
+D
e
(d)}  = min{9,2,∞} = 2 
D
b
(f) = min{c
b,a
+D
a
(f), c
b,c 
+D
c
(f), c
b,e 
+D
e
(f)}  = min{∞,∞,2} = 2 
D
b
(i) = min{c
b,a
+D
a
(i), c
b,c 
+D
c
(i), c
b,e
+D
e
(i)}  = min{∞,∞, ∞} = ∞ 
D
b
(h) = min{c
b,a
+D
a
(h), c
b,c 
+D
c
(h), c
b,e
+D
e
(h)}  = min{∞,∞, 2} = 2 
D
b
(e) = min{c
b,a
+D
a
(e), c
b,c 
+D
c
(e), c
b,e 
+D
e
(e)}  = min{∞,∞,1} = 1 
D
b
(g) = min{c
b,a
+D
a
(g), c
b,c 
+D
c
(g), c
b,e
+D
e
(g)}  = min{∞,∞, ∞} = ∞ 
距离向量 示例: computation
DV 在 a:
DV 在 b:
DV 在 c:
DV 在 e:
网络层: 5
b 接收 DVs
从 a, c, e,
计算:
DV 在 b:
计算
(a) = 最小值{c
(c) = 最小值{c
(d) = 最小值{c
(f) = 最小值{c
(i) = 最小值{c
(h) = 最小值{c
(e) = 最小值{c
(g) = 最小值{c

DV in a: 
D
a
(a)=0
D
a
(b) = 8
D
a
(c) =∞
D
a
(d) =1
D
a
(e) =∞
D
a
(f) =∞
D
a
(g) =∞
D
a
(h) =∞
D
a
(i) =∞
Distance vector example: computation
Network Layer: 5-37
g
hi
1
1
1
11
1
1
1
1
8
1
t=1
DV in b:
D
b
(f) =∞
D
b
(g) =∞
D
b
(h) =∞
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =∞
D
b
(e) =1
c receives DVs 
from b
a
b
c
d
ef
DV in c:
D
c
(a) =∞
D
c
(b) =1
D
c
(c) = 0
D
c
(d) =∞
D
c
(e) =∞
D
c
(f) =∞
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =∞
DV in e:
D
e
(a) =∞
D
e
(b) =1
D
e
(c) =∞
D
e
(d) =1
D
e
(e) = 0
D
e
(f) =1
D
e
(g) =∞
D
e
(h) =1
D
e
(i) =∞
DV 在 a:
距离向量 示例: computation
网络层: 5
DV 在 b:
c 接收 DVs
从 b
DV 在 c:
DV 在 e:

Distance vector example: computation
Network Layer: 5-38
g
h
i
1
1
8
1
t=1
DV in b:
D
b
(f) =∞
D
b
(g) =∞
D
b
(h) =∞
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =∞
D
b
(e) =1
c receives DVs 
from b 
computes:
a
bc
d
e
f
DV in c:
D
c
(a) =∞
D
c
(b) =1
D
c
(c) = 0
D
c
(d) =∞
D
c
(e) =∞
D
c
(f) =∞
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =∞
D
c
(a) = min{c
c,b
+D
b
(a}} = 1 + 8 = 9 
D
c
(b) = min{c
c,b
+D
b
(b)} = 1 + 0 = 1
D
c
(d) = min{c
c,b
+D
b
(d)} = 1+ ∞ = ∞ 
D
c
(e) = min{c
c,b
+D
b
(e)} = 1 + 1 = 2
D
c
(f) = min{c
c,b
+D
b
(f)} = 1+ ∞ = ∞ 
D
c
(g) = min{c
c,b
+D
b
(g)} = 1+ ∞ = 
∞ 
D
c
(i) = min{c
c,b
+D
b
(i)} = 1+ ∞ = ∞ 
D
c
(h) = min{c
c,b
+D
b
(h)} = 1+ ∞ = ∞ 
DV in c:
D
c
(a) =9
D
c
(b) =1
D
c
(c) = 0
D
c
(d) =2
D
c
(e) =∞
D
c
(f) =∞
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =∞
compute
* Check out the online interactive 
exercises for more examples: 
h
ttp://gaia.cs.umass.edu/kurose_ross/interactive/
距离向量 示例: computation
网络层: 5
DV 在 b:
c 接收 DVs
从 b
计算:
DV 在 c:
(a) = 最小值{c
(b) = 最小值{c
(d) = 最小值{c
(e) = 最小值{c
(f) = 最小值{c
(g) = 最小值{c
(i) = 最小值{c
(h) = 最小值{
DV 在 c:
计算
* 查看 查看 在线 交互式
练习 用于 更多 示例:

Distance vector example: computation
1
1
11
1
1
1
1
1
8
1
t=1
DV in b:
D
b
(f) =∞
D
b
(g) =∞
D
b
(h) =∞
D
b
(i) =∞
D
b
(a) = 8
D
b
(c) =1
D
b
(d) =∞
D
b
(e) =1
e receives DVs 
from b, d, f, h
a
b
c
DV in f:
D
c
(a) =∞
D
c
(b) =∞
D
c
(c) = ∞
D
c
(d) =∞
D
c
(e) =1
D
c
(f) =0
D
c
(g) =∞
D
c
(h) =∞
D
c
(i) =1
DV in e:
D
e
(a) =∞
D
e
(b) =1
D
e
(c) =∞
D
e
(d) =1
D
e
(e) = 0
D
e
(f) =1
D
e
(g) =∞
D
e
(h) =1
D
e
(i) =∞
DV in h:
D
c
(a) =∞
D
c
(b) =∞
D
c
(c) = ∞
D
c
(d) =∞
D
c
(e) =1
D
c
(f) =∞
D
c
(g) =1
D
c
(h) =0
D
c
(i) =1
DV in d:
D
c
(a) =1
D
c
(b) =∞
D
c
(c) = ∞
D
c
(d) =0
D
c
(e) =1
D
c
(f) =∞ 
D
c
(g) = 1
D
c
(h) =∞
D
c
(i) =∞
d
e
f
g
h
i
Q: what is new DV computed in 
e at t=1?
compute
距离向量 示例: computation
DV 在 b:
e 接收 DVs
从 b, d, f, h
DV 在 f:
DV 在 e:
DV 在 h:
DV 在 d:
Q: 什么 是 新的 DV 计算得到的 在
e 在
计算

Distance vector: state information diffusion
t=0
c’s state at t=0 is at c only
g
h
i
1
1
11
1
1
1
1
1
8
1
d
e
f
a
bc
c’s state at t=0 has propagated to b, and 
may influence distance vector 
computations up to 
1 
hop away, i.e., at b
t=1
c’s state at t=0 may now influence distance 
vector computations up to 
2 
hops away, 
i.e., at b and now at a, e as well
t=2
c’s state at t=0 may influence distance 
vector computations up to 
3
hops away, i.e., 
at b,a,e and now at c,f,h as well
t=3
c’s state at t=0 may influence distance 
vector computations up to 
4
hops away, 
i.e., at b,a,e, c, f, h and now at g,i as well
t=4
Iterative communication, computation steps diffuses information through network: 
t=1 
t=2 
t=3 
t=4 
距离向量: 状态 信息 diffusion
c’s 状态 在 t=0 是 在 c 仅
c’s 状态 在 t=0 有 propagated 到 b, 和
可能 influence 距离向量
computations 上 到
跳 away, i.e., 在 b
c’s 状态 在 t=0 可能 现在 influence 距离
向量 computations 上 到
i.e., 在 b 和 现在 在 a, e as well
c’s 状态 在 t=0 可能 influence 距离
向量 computations 上 到
在 b,a,e 和 现在 在 c,f,h as well
c’s 状态 在 t=0 可能 influence 距离
向量 computations 上 到
i.e., 在 b,a,e, c, f, h 和 现在 在 g,i as well
迭代式 通信, computation steps diffuses 信息 通过 网络:

Distance vector: link cost changes
Network Layer: 5-43
“good news 
travels fast”
t
0 
: ydetects link-cost change, updates its DV, informs its 
neighbors.
t
1 
: zreceives update from y, updates its table, computes new 
least cost to x, sends its neighbors its DV.
t
2 
: yreceives z’s update, updates its distance table.  y’s least 
costs do notchange, so ydoes notsend a message to z. 
link cost changes:
node detects local link cost change 
updates routing info, recalculates local DV
if DV changes, notify neighbors
x
z
1
4
50
y
1
距离向量: 链路费用 变化
网络层: 5
“好的
travels 快速”
detects 链路
费用 变化, updates 它的 DV, informs 它的
接收 更新 从, updates 它的 表, 计算 新的
最少的 费用 到
, 发送 它的 邻居 它的 DV.
接收s 更新, updates 它的 距离 table.
s 最少的
费用 做不变化, 因此做不发送 a 报文 到
链路费用 变化:
节点 detects 本地 链路费用 变化
updates 路由 信息, recalculates 本地 DV
如果 DV 变化, 通知 邻居

Distance vector: link cost changes
Network Layer: 5-44
link cost changes:
node detects local link cost change 
“bad news travels slow” –count-to-infinity 
problem:
x
z
1
4
50
y
60
•y sees direct link to xhas new cost 60, but z has said it has a path at 
cost of 5. So ycomputes “my new cost to x will be 6, via z); notifies zof 
new cost of 6 to x.
•z learns that path to xviay has new cost 6, so zcomputes “my new 
cost to xwill be 7 via y), notifies yof new cost of 7 to x.
•y learns that path to xvia z has new cost 7, so ycomputes “my new 
cost to xwill be 8 via y), notifies zof new cost of 8 to x.
•z learns that path to xviay has new cost 8, so zcomputes “my new 
cost to xwill be 9 via y), notifies yof new cost of 9 to x.
...
see text for solutions.  Distributed algorithms are tricky!
距离向量: 链路费用 变化
网络层: 5
链路费用 变化:
节点 detects 本地 链路费用 变化
到
问题:
sees 直接 链路 到
有 新的 费用 60,
但是 z 有 said 它 有 a 路径 在
费用 的 5
. 因此
计算 “my 新的 费用 到 x 将 是 6, 通过 z); 通知的
新的 费用 的 6 到
learns 这 路径 到
通过
有 新的 费用 6, 因此
计算 “my 新的
费用 到
将 是 7 通过 y), 通知
的 新的 费用 的 7 到
learns 这 路径 到
通过
有 新的 费用 7, 因此
计算 “my 新的
费用 到
将 是 8 通过 y), 通知
的 新的 费用 的 8 到
learns 这 路径 到
通过
有 新的 费用 8, 因此
计算 “my 新的
费用 到
将 是 9 通过 y), 通知
的 新的 费用 的 9 到
见 text 用于 solutions.
分布式 算法 是 tricky!

45
解决方法：毒性反转
x
z
1
4
50
y
T=0：
X tells Y:  D
X
(Z)=50;
X tells Z:  D
X
(Y)=4;
Y tells X:  D
Y
(Z)=1;
Y tells Z:  D
Y
(X)=4;
Z tells X:  D
Z
(Y)=1;
Z tells Y:  D
Z
(X)=50;
Z updates:
D
Y
(X) = 5
D
Z
(X) = 50
Z  propagates:
Z tells X:  D
Z
(Y)=1;
Z tells Y:  D
Z
(X)=∞;
x
z
1
4
50
y
60
T=1：
...
Y tells Z:  D
Y
(X)=60;
...
Due to the current shortest 
path passing through Y, 
blocking the loop path 
Z updates:
D
Y
(X) = 60
D
Z
(X) = 50
Z  propagates:
Z tells X:  D
Z
(Y)=1;
Z tells Y:  D
Z
(X)=50;
告诉 Y: D
告诉 Z: D
告诉 X: D
告诉 Z: D
告诉 X: D
告诉 Y: D
告诉 X: D
Z 告诉 Y: D
告诉 Z:
由于 到 当前 shortest
路径 passing 通过 Y,
blocking loop 路径
告诉 X: D
Z 告诉 Y: D

4-49
Comparison of LS and DV algorithms
message complexity

LS:with n nodes, E links, O(nE) msgssent  

DV:exchange between neighbors only

convergence time varies
speed of convergence

LS:O(n
2
) algorithm requires O(nE) msgs

may have oscillations

DV:convergence time varies

may be routing loops

count-to-infinity problem
robustness:what happens if router 
malfunctions?
LS:

node can advertise incorrectlinkcost

each node computes only its owntable
DV:

DV node can advertise incorrectpathcost

each node’s table used by others 

error propagate thru network
Comparison 的 LS 和 DV 算法
报文 复杂度
与 n 节点, E 链路, O(
已发送
交换 之间 邻居 仅
convergence 时间 varies
speed 的 convergence
) 算法 requires O(
可能 有 oscillations
convergence 时间 varies
可能 是 路由 loops
到infinity 问题
什么 happens 如果 路由器
节点 可以 advertise链路费用
每个 节点 计算 仅 它的
自己的
表
DV 节点 可以 advertise路径费用
每个 节点s 表 用于 由 others
差错 propagate thru 网络

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-50
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

Making routing scalable
scale:with billions of 
destinations:

Can’t store all destinations in 
routing tables!

routing table exchange would 
swamp links!
administrative autonomy

internet = network of networks

each network admin may want to 
control routing in its own network
our routing study thus far -idealized 
all routers identical
network “flat”
... nottrue in practice
5-51
Making 路由 scalable
与 billions 的
可以
’t store 所有 destinations 在
路由表s!
路由表 交换 would
swamp 链路!
Internet = 网络 的 网络
每个 网络 admin 可能 want 到
控制 路由 在 它的 自己的 网络
我们的 路由 study thus far
所有 路由器 identical
网络
... 不
true 在 practice

aggregate routers into regions known as“autonomous 
systems”(AS) (a.k.a. “domains”)
inter-AS routing

routing among AS’es

gateways perform inter-domain routing 
(as well as intra-domain routing)
Internet approach to scalable routing
intra-AS routing
routing among hosts, routers in sameAS 
(“network”)
all routers in AS must run sameintra-
domain protocol
routers in differentAS can run different
intra-domain routing protocol
gateway router: at “edge” of its own AS, 
has link(s) to router(s) in other AS’es
5-52
aggregate 路由器 进入 regions 已知 as
系统
之间的
AS 路由
路由 之间
gateways perform 之间的
域 路由
(as well as 内部的
域 路由)
Internet 方法 到 scalable 路由
内部的
AS 路由
路由 之间 主机, 路由器 在相同
(“网络”)
所有 路由器 在 AS 必须 运行
相同
内部的
域 协议
路由器 在
不同的
可以 运行
不同的
内部的
域路由 协议
网关 路由器: 在 “边缘” 的 它的 自己的 AS,
有 链路(s) 到 路由器(s) 在 其他

4-53
3b
1d
3a
1c
2a
AS3
AS1
AS2
1a
2c
2b
1b
Intra-AS
Routing 
algorithm
Inter-AS
Routing 
algorithm
Forwarding
table
3c
Interconnected ASes

forwarding table  configured by 
both intra-and inter-AS routing 
algorithm

intra-AS sets entries for 
internal dests

inter-AS & intra-AS sets 
entries for external dests
内部的
路由
算法
之间的
路由
算法
转发
表
转发表 configured 由
二者 内部的
和 之间的AS 路由
算法
内部的
AS 集合 entries 用于
之间的
AS & 内部的
AS 集合
entries 用于 external

4-54
Inter-AS tasks

suppose router inAS1 receives 
datagram destined outsideof AS1:

router should forward packet to 
gateway router, but which one?
AS1 must:
1.
learn which destsare reachable
throughAS2, which throughAS3
2.
propagate this reachabilityinfo 
to all routers inAS1
job of inter-AS routing!
AS3
AS2
3b
3c
3a
AS1
1c
1a
1d
1b
2a
2c
2b
other
networks
other
networks
之间的
suppose 路由器在AS1 接收
数据报 destined的 AS1:
路由器 should 转发 分组 到
网关 路由器
, 但是 哪个 一个?
AS1 必须:
learn 哪个是
通过AS2, 哪个通过
propagate 这个信息
到 所有 路由器
在
job 的 之间的
AS 路由!
其他
网络
其他
网络

4-55
Intra-AS Routing

also known as interior gateway protocols (IGP)

most common intra-AS routing protocols:

RIP: Routing Information Protocol

OSPF: Open Shortest Path First

IGRP: Interior Gateway Routing Protocol (Cisco proprietary 
for decades, until 2016)
内部的AS 路由
also 已知 asinterior 网关 协议 (IGP)
most common 内部的AS 路由协议:
: 路由 信息 协议
: Open Shortest 路径 第一个
IGRP: Interior 网关 路由 协议 (Cisco proprietary
用于 decades, 直到 2016

56
RIP (Routing Information Protocol)
RIP 是内部网关协议IGP 中最先得到广泛使用的协议。
RIP 是一种分布式的、基于距离向量的路由选择协议。
RIP 协议要求网络中的每一个路由器都要维护从它自己到其他每一个目的网络的距离记录。
RIP 认为一个好的路由就是它通过的路由器的数目少，即“距离短”。
RIP 允许一条路径最多只能包含15个路由器。
“距离”的最大值为16 时即相当于不可达。可见RIP 只适用于小型互联网。
RIP 不能在两个网络之间同时使用多条路由。RIP 选择一个具有最少路由器的路由（即最短路由），
哪怕还存在另一条高速(低时延)但路由器较多的路由。
RIP 协议的三个特点
1.仅和相邻路由器交换信息。
2.交换的信息是当前本路由器所知道的全部信息，即自己的路由表。
3.按固定的时间间隔交换路由信息，例如，每隔30 秒。当网络拓扑发生变化时，路由器也及时向相
邻路由器通告拓扑变化后的路由信息。
RIP (路由 信息 协议)

57
路由表的建立
路由器在刚开始工作时，只知道到直接连接的网络的距离（此距离定义为1）。它的路由表是空的。
以后，每一个路由器也只和数目非常有限的相邻路由器交换并更新路由信息。
经过若干次更新后，所有的路由器最终都会知道到达本自治系统中任何一个网络的最短距离和下一跳
路由器的地址。
RIP 协议的收敛(convergence) 过程较快。“收敛”就是在自治系统中所有的结点都得到正确的路
由选择信息的过程。

RIP2 协议的报文格式
4 字节
RIP 报文
路由信息
（20 字节/路由）
可重复出现
最多25 个
IP 数据报
路由标记
网络地址
地址族标识符
距离(1-16)
IP 
首部
UDP 
首部
路由部分
4 字节
子网掩码
下一跳路由器地址
UDP 用户数据报
首部
必为0
版本
命令

RIP2 报文
RIP2 报文由首部和路由部分组成。
RIP2 报文中的路由部分由若干个路由信息组成。每个路由信息需要
用20 个字节。地址族标识符（又称为地址类别）字段用来标志所使
用的地址协议。
路由标记填入自治系统的号码，这是考虑使RIP 有可能收到本自治
系统以外的路由选择信息。
再后面指出某个网络地址、该网络的子网掩码、下一跳路由器地址
以及到此网络的距离。

RIP2 报文
一个RIP 报文最多可包括25 个路由，因而RIP 报文的最大长度是
4+20 x25=504 字节。如超过，必须再用一个RIP 报文来传送。
RIP2 具有简单的鉴别功能。
1.若使用鉴别功能，则将原来写入第一个路由信息（20 个字节）
的位置用作鉴别。
2.在鉴别数据之后才写入路由信息，但这时最多只能再放入24 个
路由信息。

好消息传播得快，坏消息传播得慢
RIP 协议特点：好消息传播得快，坏消息传播得慢。
RIP 存在的一个问题：当网络出现故障时，要经过比较长的时间(例
如数分钟) 才能将此信息传送到所有的路由器。

RIP 协议的优缺点
优点：
1.实现简单，开销较小。
缺点：
1.RIP 限制了网络的规模，它能使用的最大距离为15（16 表示不可达）。
2.路由器之间交换的路由信息是路由器中的完整路由表，因而随着网络规模
的扩大，开销也就增加。
3.“坏消息传播得慢”，使更新过程的收敛时间过长。

4-63
OSPF (Open Shortest Path First)

“open”: publicly available

uses link state algorithm 

LS packet dissemination

topology map at each node

route computation using Dijkstra’s algorithm

OSPF advertisement carries one entry per neighbor 

advertisements floodedto entireAS

carried in OSPF messages directly over IP(rather than TCP or UDP)

IS-IS routingprotocol: nearly identical to OSPF
OSPF (Open Shortest 路径 第一个)
: publicly 可用的
使用 链路状态 算法
LS 分组 dissemination
拓扑 map 在 每个 节点
路由 computation 使用s 算法
OSPF advertisement carries 一个 entry 每 邻居
到
承载的 在 OSPF 报文directly 在 IP(rather 比 TCP 或 UDP)
IS 路由协议: nearly identical 到 OSPF

4-64
OSPF “advanced”features (not in RIP)

security:all OSPF messages authenticated (to prevent 
malicious intrusion) 

multiplesame-cost paths allowed (only one path in RIP)

for each link, multiple cost metrics for different TOS(e.g., 
satellite link cost set “low”for best effort ToS; high for real time 
ToS)

integrated uni-and multicastsupport: 

Multicast OSPF (MOSPF) uses same topology data base as 
OSPF

hierarchicalOSPF in large domains.
features (不 在 RIP)
安全:所有 OSPF 报文 authenticated (到 prevent
相同费用路径s allowed (仅 一个 路径 在 RIP)
用于 每个 链路, 多路 费用 metrics 用于 不同的
satellite 链路费用 集合
用于 尽力而为
; high 用于 real 时间
和支持:
Multicast OSPF (MOSPF) 使用 相同 拓扑 数据 基 as
OSPF 在 大的 domains.

Hierarchical OSPF
Network Layer: 5-65
two-level hierarchy: local area, backbone.
•link-state advertisements flooded only in area, or backbone
•each node has detailed area topology; only knows direction to 
reach other destinations
area border routers: 
“summarize” distances  
to destinations in own 
area, advertise in 
backbone
area 1
area 2
area 3
backbone
internal
routers
backbone router: 
runs OSPF 
limited to 
backbone
boundary router: 
connects to other 
ASes
local routers: 
•flood LS in area only
•compute routing within 
area
•forward packets to 
outside via area border 
router
网络层: 5
两个
本地 区域, backbone.
链路
状态 advertisements flooded 仅 在 区域, 或 骨干
每个 节点 有 detailed 区域 拓扑; 仅 知道 direction 到
reach 其他 destinations
区域 border 路由器:
到 destinations 在 自己的
区域, advertise 在
骨干
区域 1
区域 2
区域 3
骨干
路由器
骨干 路由器:
limited 到
骨干
boundary 路由器:
连接 到 其他
本地 路由器:
flood LS 在 区域 仅
计算 路由 在...内
区域
转发 分组 到
outside 通过 区域 border
路由器

4-66

two-level hierarchy:local area, backbone.

link-state advertisements only in area 

each nodes has detailed area topology; only know direction
(shortest path) to nets in other areas.

area border routers:“summarize”distances  to nets in own area, 
advertise to other Area Border routers.

backbone routers:run OSPF routing limited to backbone.

boundary routers:connect to other AS’s.
Hierarchical OSPF
两个
本地 区域, backbone.
链路
状态 advertisements 仅 在 区域
每个 节点 有detailed 区域 拓扑; 仅 know
(shortest 路径) 到 nets 在
其他 areas
区域 border 路由器:
distances 到 nets 在 自己的 区域,
advertise 到 其他 区域 Border 路由器.
骨干 路由器:运行 OSPF 路由 limited 到 backbone.
boundary 路由器:连接 到 其他 AS

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-67
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

4-68
Internet inter-AS routing: BGP
3b
1d
3a
1c
2a
AS3
AS1
AS2
1a
2c
2b
1b
3c
BGP：Implementing communication between ASes
20241118
Internet 之间的AS 路由: BGP
Implementing 通信 之间

4-69
Internet inter-AS routing: BGP

BGP (Border Gateway Protocol):thede facto inter-domain 
routing protocol

“glue that holds the Internet together”

BGP provides each AS a means to:

eBGP:obtainsubnet reachabilityinformation from neighboring 
ASs.

iBGP:propagateAS reachabilityinformation to all AS-internal 
routers.

determine “good”routes to other networks based on 
reachabilityinformation and policy.

allows subnet to advertise its existence to rest of Internet: “I am 
here”
Internet 之间的AS 路由: BGP
BGP (Border 网关 协议):
de facto 之间的
域
路由 协议
glue 这 holds Internet together
BGP 提供 每个 AS a means 到:
子网信息 从 neighboring
信息 到 所有 AS
路由器.
确定好的路由 到 其他 网络 基于 在
信息 和 policy.
允许 子网 到 advertise 它的 existence 到 rest 的 Internet:

eBGP, iBGP connections
eBGP connectivity
iBGP connectivity
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS 2
AS 3
AS 1
5-71
1c
∂
∂
gateway routers run both 
eBGP and iBGP protools
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS 2
AS 3
AS 1
∂
∂
eBGP, iBGP 连接
网关 路由器 运行 二者
eBGP 和 iBGP

72
Theoretically,aborderroutercantransmittheAS
accessibilitymessagetoallinternalroutersthrough
iBGP,includingallnon-borderrouters.However,this
willsharplyincreasetheburdenontheinternalrouter,
becausetheroutingtableofanycorerouterisvery
largeandchangesfrequently.
Therefore,itisnotagoodideatopropagatetheAS
accessibilitymessagetoallinternalrouters.
路由器可以传输
报文到
所有
路由器通过
包括
所有
路由器
将增大在路由器,
路由表的任何核心路由器是
大的和变化
它
是不好的思想到
报文到
所有
路由器

BGP basics

when AS3 gateway router 3a advertises path 
AS3,X 
to AS2 gateway router 
2c:

AS3 promisesto AS2 it will forward datagrams towards X
BGP session:two BGP routers (“peers”) exchange BGP messages over semi-
permanent TCP connection:
•advertising pathsto different destination network prefixes (BGP  is a “path vector”
protocol)
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS 2
AS 3
AS 1
X
BGP advertisement:
AS3, X
5-73
当 AS3 网关 路由器 3a advertises 路径到 AS2 网关 路由器
到 AS2 它 将 转发 数据报 towards X
两个 BGP 路由器 (
对等方
) 交换 BGP 报文 在
permanent TCP 连接
路径到 不同的 目的地 网络 prefixes (BGP 是 a路径 向量
协议)

Path attributes and BGP routes

advertised prefix includes BGP attributes 

prefix + attributes = “route”

two important attributes:

AS-PATH: list of ASesthrough which prefix advertisement has passed

NEXT-HOP:indicates specific internal-AS router to next-hop AS

Policy-based routing:

gateway receiving route advertisement uses import policyto 
accept/decline path (e.g., never route through AS Y).

AS policy also determines whether to advertisepath to other other 
neighboring ASes
5-74
路径 attributes 和 BGP 路由
路由
两个 重要
列表 的
通过 哪个 prefix advertisement 有 passed
indicates 特定的
AS 路由器 到 下一个
跳 AS
策略基于 路由:
网关 接收 路由 advertisement 使用import 策略到
accept/decline 路径 (e.g., never 路由 通过 AS Y).
AS 策略 also determines whether 到路径 到 其他 其他

BGP path advertisement

Based on AS2 policy, AS2 router 2c accepts path AS3,X, propagates (via iBGP) to 
allAS2 routers
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS2
AS3
AS1
X
AS3,X 
AS2,AS3,X 
AS2 router 2c receives path advertisement 
AS3,X 
(via eBGP) from AS3 router 3a
Based on AS2 policy,  AS2 router 2a advertises(via eBGP)  path 
AS2, AS3, X  
to AS1router 1c
5-75
eBGP
eBGP
iBGP
BGP 路径 advertisement
基于 在 AS2 策略, AS2 路由器 2c accepts 路径 AS3,X, propagates (通过 iBGP) 到
所有
AS2 路由器
AS2 路由器 2c 接收 路径 advertisement
通过 eBGP
) 从 AS3 路由器 3a
基于 在 AS2 策略, AS2 路由器 2a
(通过 eBGP) 路径
到 AS
路由器

BGP path advertisement

AS1gateway router1c learns path AS2,AS3,X from 2a
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS2
AS3
AS1
X
AS3,X 
AS2,AS3,X 
gateway router may learn about multiplepaths to destination:
AS1gateway router1c learns path AS3,X from 3a
Based on policy, AS1gateway router1c chooses path AS3,X, and advertises path within AS1via iBGP
5-76
BGP 路径 advertisement
网关 路由器learns 路径从 2a
网关 路由器 可能 learn 关于多路路径 到 目的地:
网关 路由器
learns 路径
从 3a
基于 在 策略,网关 路由器
chooses 路径
AS3,X, 和 advertises 路径 在...内 AS
通过 iBGP

BGP messages

BGP messages exchanged between peers over TCP connection

BGP messages:

OPEN:opens TCP connection to remote BGP peer and 
authenticates sending BGP peer

UPDATE:advertises new path (or withdraws old)

KEEPALIVE:keeps connection alive in absence of UPDATES; also 
ACKs OPEN request

NOTIFICATION:reports errors in previous msg; also used to close 
connection
5-77
BGP 报文
BGP 报文 exchanged 之间 对等方 在 TCP 连接
BGP 报文:
opens TCP 连接 到 远程 BGP 对等方 和
authenticates 发送 BGP 对等方
新的 路径(或 withdraws old)
keeps 连接 alive 在 absence 的 UPDATES; also
ACK OPEN 请求
差错在 previous
; also 用于 到 关闭
连接

2b
2d
2c
2a
AS 2
3b
3d
3c
3a
AS 3
1b
1d
1c
1a
AS 1
X
BGP path advertisement
Network Layer: 5-78
AS2,AS3,X 
AS3,X
AS3, X
recall: 1a, 1b, 1d learn via iBGP from 1c: “path to X goes through 1c”
at 1d: OSPF intra-domain routing: to get to 1c, use  interface 1
1
2
1
2
dest
interface
...
...
...
...
local link 
interface
s
at 1a, 1d
at 1d: to get to X, use  interface 1
1c1
X
1
AS3,X
AS3,X
AS3,X
BGP 路径 advertisement
网络层: 5
回顾:d learn 通过 iBGP 从c: “路径 到 X goes 通过
在 1
d: OSPF 内部的
域 路由: 到 获得 到c, 使用 接口
接口
本地 链路
接口
在 1a, 1d
在 1
d: 到 获得 到
, 使用 接口

2b
2d
2c
2a
AS 2
3b
3d
3c
3a
AS 3
1b
1d
1c
1a
AS 1
X
BGP path advertisement
Network Layer: 5-79
recall: 1a, 1b, 1d learn via iBGP from 1c: “path to X goes through 1c”
at 1d: OSPF intra-domain routing: to get to 1c, use  interface 1
1
2
at 1d: to get to X, use  interface 1
dest
interface
...
...
...
...
1c2
X
2
at 1a: OSPF intra-domain routing: to get to 1c, use  interface 2
at 1a: to get to X, use  interface 2
BGP 路径 advertisement
网络层: 5
回顾:d learn 通过 iBGP 从c: “路径 到 X goes 通过
在 1
d: OSPF 内部的
域 路由: 到 获得 到c, 使用 接口
在 1
d: 到 获得 到
, 使用 接口
接口
在 1
a: OSPF 内部的
域 路由: 到 获得 到c, 使用 接口
在 1
a: 到 获得 到
, 使用 接口

Hot Potato Routing

2d learns (via iBGP) it can route to X via 2a or 2c

hot potato routing: choose local gateway that has least intra-domain cost 
(e.g., 2d chooses 2a, even though more AS hops to X): don’t worry about 
inter-domain cost!
1b
1d
1c
1a
2b
2d
2c
2a
3b
3d
3c
3a
AS2
AS3
AS1
X
AS3,X 
AS1,AS3,X 
OSPF link weights
201
152
112
263
5-80
To X, 2a or 2c ?
Hot Potato 路由
2d learns (通过 iBGP) 它 可以 路由 到 X 通过 2a 或 2c
hot potato 路由:选择 本地 网关 这 有 最少的 内部的域 费用
, even though 更多 AS hops 到
): don’t worry 关于
之间的
域 费用!
OSPF 链路 weights
到 X, 2a 或 2c ?

A advertises path Aw to B and to C
B chooses not to advertise BAwto C:  
B gets no “revenue”for routing CBAw, since none of  C, A, w 
are B’s customers
C does not learn about CBAwpath
C will route CAw(not using B) to get to w
A
B
C
W
X
Y
legend
:
customer 
network:
provider
network
Suppose an ISP only wants to route traffic to/from its customer 
networks (does not want to carry transit traffic between other ISPs)
5-81
BGP: achieving policy via advertisements
advertises 路径 Aw 到 B 和 到 C
chooses 不 到 advertise
到 C:
B 获得 无用于 路由
, since none 的 C, , w
是 B’s customers
C 做 不 learn 关于路径
C 将 路由
(不 使用 B) 到 获得 到 w
网络:
提供商
网络
Suppose ISP 仅 wants 到 路由 流量 to/来自 它的 customer
网络 (做 不 want 到 carry transit 流量 之间 其他 ISPs)
BGP: achieving 策略 通过 advertisements

BGP: achieving policy via advertisements
A,B,C are provider networks
X,W,Y are customer (of provider networks)
X is dual-homed:attached to two networks
policy to enforce: X does not want to route from B to C via X
.. so X will not advertise to B a route to C
A
B
C
W
X
Y
legend
:
customer 
network:
provider
network
Suppose an ISP only wants to route traffic to/from its customer 
networks (does not want to carry transit traffic between other ISPs)
5-82
BGP: achieving 策略 通过 advertisements
,B,C 是提供商 网络
X,W,Y 是 customer (的 提供商 网络)
X 是
attached 到 两个 网络
策略 到 enforce:
X 做 不 want 到 路由 从 B 到 C 通过 X
.. 因此
X 将 不 advertise 到 B a 路由 到 C
网络:
提供商
网络
Suppose ISP 仅 wants 到 路由 流量 to/来自 它的 customer
网络 (做 不 want 到 carry transit 流量 之间 其他 ISPs)

4-83
Why different Intra-, Inter-AS routing ? 
policy:

inter-AS: admin wants control over how its traffic 
routed, who routes through its net. 

intra-AS: single admin, so no policy decisions 
needed
scale:

hierarchical routing saves table size, reduced 
update traffic
performance: 

intra-AS: can focus on performance

inter-AS: policymay dominate over performance
uncontrollable
Why 不同的 内部的
, 之间的
AS 路由 ?
策略:
之间的
AS: admin wants 控制 在 如何 它的 流量
routed, who 路由 通过 它的 net.
内部的
AS: 单个 admin, 因此 无 策略 decisions
hierarchical 路由 saves 表 大小, reduced
更新 流量
性能:
内部的
AS: 可以 focus 在性能
之间的
策略可能 dominate 在 性能


router may learn about more than one route to 
destination AS, selects route based on:
1.
local preference value attribute: policy decision
2.
shortest AS-PATH 
3.
closest NEXT-HOP router: hot potato routing
4.
additional criteria 
BGP route selection
Network Layer: 5-84
路由器 可能 learn 关于 更多 比 一个 路由 到
目的地 AS, selects 路由 基于 在:
本地 preference 值 attribute: 策略 decision
HOP 路由器: hot potato 路由
BGP 路由 selection
网络层: 5

Unicast & Multicast
85
单播（Unicast）是在一个单
个的发送者和一个接受者之间
通过网络进行的通信。
IP多播是实现数据一对多通信的
模式。从一个源点传送到多个目
的地，数据仅仅拷贝一份。这里
说的数据仅仅拷贝一份，是指在
每一条须要它的两个点之间，数
据仅仅有一份。

IP-Anycast

replicating the same content on different serversin many 
different dispersed geographical locations
86
having each user 
access the content 
from the server that is 
closest
相同 内容 在 不同的 服务器在 许多
不同的 dispersed geographical locations
having 每个 用户
访问 内容
从 服务器 这 是

IP-Anycast

in practice CDNs generally choose NOTto use IP-anycastbecause 
BGP routing changes can result in different packets of the same 
TCP connection arriving at different instances of the Web server

IP-anycastis extensively used by the DNS systemto direct DNS 
queries to the closest root DNS server. 

There are currently 13 IP addresses for root DNS servers. But  
corresponding to each of these addresses, there are multiple DNS root 
servers, with some of these addresses having over 100 DNS root servers 
scattered over all corners of the world. 
87
在 practice CDNs generally 选择
到 使用 IP
BGP 路由 变化 可以 result 在不同的 分组 的 相同
TCP 连接 到达的 在 不同的 instances的 Web 服务器
是 extensively 用于 由DNS 系统到 直接 DNS
queries 到 closest 根 DNS 服务器.
存在 是 currently 13 IP 地址 用于 根 DNS servers. 但是
corresponding 到 每个 的 这些 地址, 存在 是 多路 DNS 根
服务器, 与 一些 的 这些 地址 having 在 100 DNS 根 服务器
scattered 在 所有 corners 的 world.

5.4.6 Putting the Pieces Together: Obtaining 
Internet Presence

Please read the Section 5.4.6 carefully

IP addressing, DNS, BGP and intra-AS routing
88
节 5.4.6
IP 编址, DNS, BGP 和 内部的AS 路由

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-89
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

Software defined networking (SDN)

Internet network layer: historically has been implemented 
via distributed, per-router approach
monolithicrouter contains switching hardware, runs proprietary 
implementation of Internet standard protocols (IP, RIP, IS-IS, OSPF, 
BGP) in proprietary router OS (e.g., Cisco IOS)

different “middleboxes” for different network layer functions: 
firewalls, load balancers, NAT boxes, ...

~2005: renewed interest in rethinking network control plane
5-90
软件定义网络 (SDN)
Internet 网络层: historically 有 been implemented
通过 分布式, 每路由器 方法
路由器 包含 交换 hardware, runs proprietary
实现 的 Internet standard 协议 (IP, RIP, IS
BGP) 在 proprietary 路由器 OS (e.g., Cisco IOS)
不同的 “middleboxes” 用于 不同的 网络层 功能:
firewalls, 负载 balancers, NAT boxes, ...
~2005: renewed interest 在 rethinking 网络 控制平面

Recall: per-router control plane
Routing
Algorithm
Individual routing algorithm components in each and every 
router interact with each other in control plane to compute 
forwarding tables
data
plane
control
plane
5-91
回顾: 每路由器 控制平面
路由
算法
Individual 路由 算法 components
在 每个 和 每一个
路由器
interact 与 每个 其他 在 控制平面 到 计算
转发表s
数据
平面
控制
平面

data
plane
control
plane
Recall: logically centralized control plane
A distinct (typically remote) controller interacts with local 
control agents (CAs) in routers to compute forwarding tables
Remote Controller
CA
CA
CA
CA
CA
5-92
2023.11.6
数据
平面
控制
平面
回顾: logically centralized 控制平面
distinct (通常 远程) 控制器 interacts 与 本地
控制 agents (CAs) 在 路由器 到 计算 转发表s
远程 控制器

Software defined networking (SDN)
Whyalogically centralized control plane?

easier network management: avoid router misconfigurations, greater 
flexibility of traffic flows

table-based forwarding (recall OpenFlow API) allows “programming” 
routers

centralized “programming” easier: compute tables centrally and 
distribute

distributed “programming: more difficult: compute tables as result 
of distributed algorithm (protocol) implemented in each and every 
router 

open (non-proprietary) implementation of control plane
5-93
软件定义网络 (SDN)
控制平面?
网络管理
: avoid 路由器 misconfigurations, greater
flexibility 的 流量 flows
表基于 转发 (回顾 OpenFlow API) 允许“编程”
路由器
centralized “编程”: 计算 表 centrally 和
分布式 “编程: 更多: 计算 表 as result
的 分布式 算法 (协议) implemented 在 每个 和 每一个
路由器
proprietary) 实现 的 控制平面

Vertically integrated
Closed, proprietary
Slow innovation
Small industry
Specialized
Operating
System
Specialized
Hardware
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
Ap
p
App
Specialized
Applications
Horizontal
Open interfaces
Rapid innovation
Huge industry
Microprocessor
Open Interface
Linux
Mac
OS
Windows
(OS)
or
or
Open Interface
Analogy: mainframe to PC evolution
*
* Slide  courtesy: N. McKeown
5-94
系统
应用
应用
Open 接口
Open 接口
或
或
Open 接口
Analogy: main帧 到 PC evolution
* 幻灯片 courtesy: N. McKeown

y
x
wv
z
2
2
1
3
1
1
2
5
3
5
Traffic engineering: difficult
u
v
x
w
y
z
Q: what if w wants to route blue and red traffic differently?
A: can’t do it (with destination based forwarding, and LS, DV routing)(or need a 
new routing algorithm)
5-95
流量 engineering: difficult
什么 如果 w wants 到 路由 blue 和 red 流量 differently?
可以’t 做 它 (与
目的地 基于 转发, 和 LS, DV 路由)
(或 需要 a
新的 路由 算法)

Traffic engineering: difficult
Q: what if network operator wants to split  u-to-z traffic along uvwzand
uxyz(load balancing)?
A: can’t do it (or need a new routing algorithm)
5-96
2
2
1
3
1
1
2
5
3
5
v
w
u
z
y
x
流量 engineering: difficult
什么 如果 网络 operator wants 到 split u
到z 流量 along和
(负载 balancing)?
可以’t 做 它 (或 需要 a 新的 路由 算法)

Software defined networking (SDN)
data
plane
control
plane
Remote Controller
CA
CA
CACA
CA
1: generalized“ flow-
based” forwarding 
(e.g., OpenFlow)
2. control, 
data plane 
separation
3. control plane 
functions 
external to data-
plane switches
...
4. programmable 
control 
applications
routing
access 
control
load
balance
5-97
软件定义网络 (SDN)
数据
平面
控制
平面
远程 控制器
流
基于” 转发
控制,
数据平面
控制平面
功能
external 到 数据
平面 交换机
控制
应用
路由
访问
控制
负载

SDN perspective: data plane switches
Data plane switches

fast, simple, commodity switches 
implementing generalized data-plane 
forwarding (Section 4.4) inhardware

switch flow table computed, installed 
by controller

API for table-based switch control (e.g., 
OpenFlow)

defines what is controllable and what is not

protocol for communicating with 
controller (e.g., OpenFlow)
data
plane
control
plane
SDN Controller
(network operating system)
...
routing
access 
control
load
balance
southbound API
northbound API
SDN-controlled switches
network-control applications
5-98
SDN perspective: 数据平面 交换机
数据平面 交换机
快速, simple, commodity 交换机
implementing generalized 数据平面
转发(节 4.4)在
交换机 流 表 计算得到的, installed
由 控制器
API 用于 表基于 交换机 控制 (e.g.,
defines 什么 是 controllable 和 什么 是 不
协议 用于 communicating 与
控制器 (e.g., OpenFlow)
数据
平面
控制
平面
SDN 控制器
(网络 operating 系统)
路由
访问
控制
负载
controlled 交换机
网络控制 应用

SDN perspective: SDN controller
data
plane
control
plane
SDN Controller
(network operating system)
...
routing
access 
control
load
balance
southbound API
northbound API
SDN-controlled switches
network-control applications
5-99
SDN controller (network OS): 
maintain network state information
interacts with network control applications 
“above” via northbound API
interacts with network switches “below” via 
southbound API
implemented as distributed system for 
performance, scalability, fault-tolerance, 
robustness
Transform the control policy
into a forwarding table
SDN perspective: SDN 控制器
数据
平面
控制
平面
SDN 控制器
(网络 operating 系统)
路由
访问
控制
负载
controlled 交换机
网络控制 应用
SDN 控制器 (网络 OS):
maintain 网络 状态 信息
interacts 与 网络 控制 应用
“上方” 通过
interacts 与 网络 交换机 “below” 通过
implemented as 分布式 系统 用于
性能, scalability, fault
Transform 控制策略
进入 a 转发表

Network-wide distributed, robust  state management
Communication to/from controlled devices
Link-state infoswitch infohost info
statistics
flow tables
...  
...  
OpenFlow
SNMP
...  
network 
graph
intent
RESTful
API
...  
Interface, abstractions for network control apps
SDN
controller
routing
access 
control
load
balance
Components of SDN controller
communication layer: communicate 
between SDN controller and 
controlled switches
Network-wide state management 
layer: state of networks links, 
switches, services: a distributed 
database
Interface layer to network control 
apps:  abstractions API
5-100
网络wide 分布式, robust 状态 管理
通信 to/来自 controlled 设备
链路状态 信息
交换机 信息主机 信息
流 表
网络
图
接口, abstractions 用于 网络 控制 应用
控制器
路由
访问
控制
负载
Components 的 SDN 控制器
通信 层: 通信
之间 SDN 控制器 和
controlled 交换机
网络wide 状态 管理
层: 状态 的 网络 链路,
交换机, 服务: a分布式
数据库
接口 层 到 网络 控制
应用:

SDN perspective: control applications
network-control apps:
“brains” of control:  implement control 
functions using lower-level services, API 
provided by SND controller
unbundled: can be provided by 3
rd
party: 
distinct from routing vendor, or SDN 
controller
data
plane
control
plane
SDN Controller
(network operating system)
...
routing
access 
control
load
balance
southbound API
northbound API
SDN-controlled switches
network-control applications
Decide the controlpolicy
policy making
Plan production
performer
SDN perspective: 控制 应用
网络控制 应用:
” 的 控制: implement 控制
功能 使用 lowerlevel 服务, API
提供 由 SND 控制器
可以 是 提供 由 3
distinct 从 路由 vendor, 或 SDN
控制器
数据
平面
控制
平面
SDN 控制器
(网络 operating 系统)
路由
访问
控制
负载
controlled 交换机
网络控制 应用
Decide 控制策略
策略 making

OpenFlow protocol

operates between controller, switch

TCPused to exchange messages

optional encryption

OpenFlowmessages:

controller-to-switch

switch to controller
OpenFlow Controller
5-102
OpenFlow 协议
operates 之间 控制器, 交换机
用于 到 交换 报文
报文:
控制器到交换机
交换机 到 控制器
Open流量控制ler

OpenFlow: controller-to-switch messages
Key controller-to-switch messages

features: controller queries switch 
features, switch replies

configure: controller queries/sets switch 
configuration parameters

modify-state: add, delete, modify flow 
entries in the OpenFlow tables

packet-out: controller can send this 
packet out of specific switch port
OpenFlow Controller
5-103
OpenFlow: 控制器到交换机 报文
关键 控制器到交换机 报文
控制器 queries 交换机
features, 交换机 replies
控制器 queries/sets 交换机
修改状态:添加, 删除, 修改 流
entries 在 OpenFlow 表
分组
查看:
控制器 可以 发送 这个
分组 查看 的 特定的 交换机 端口
Open流量控制ler

OpenFlow: switch-to-controller messages
Key switch-to-controller messages

packet-in: transfer packet (and its 
control) to controller.  See packet-out 
message from controller

flow-removed: flow table entry deleted 
at switch

port status: inform controller of a 
change on a port.
Fortunately, network operators don’t “program” switches by creating/sending 
OpenFlow messages directly.  Instead use higher-level abstraction at controller
OpenFlow Controller
5-104
OpenFlow: 交换机到控制器 报文
关键 交换机到控制器 报文
分组
在:
传输 分组 (和 它的
控制) 到 controller. 见 分组
查看
报文 从 控制器
流流 表 entry deleted
在 交换机
端口 状态:inform 控制器 的 a
变化 在 a port.
Fortunately, 网络 operators don’t “program” 交换机 由 creating/sending
OpenFlow 报文 directly. Instead 使用 higherlevel 抽象 在 控制器
Open流量控制ler

Link-state info
switch info
host info
statistics
flow tables
...  
...  
OpenFlow
SNMP
...  
network 
graph
intent
RESTful
API
...  
1
2
3
4
5
Dijkstra’s link-state 
Routing
s1
s2
s3
s4
SDN: control/data plane interaction example
S1, experiencing link failure using OpenFlow port status 
message to notify controller
1
SDN controller receives OpenFlow message, updates 
linkstatus info
2
Dijkstra’s routing algorithm application has previously 
registered to be called when ever link status changes.  It is 
called.
3
Dijkstra’s routing algorithm access network graph info, 
link state info in controller,  computes new routes
4
5-105
link state routing app interacts with flow-table-computation 
component in SDN controller, which computes new flow 
tablesneeded
5
Controller uses OpenFlow to install new tables in 
switches that need updating
6
链路状态 信息
交换机 信息
主机 信息
流 表
网络
图
Dijkstra’s 链路
状态
路由
SDN: control/数据平面 interaction 示例
链路 failure
使用 OpenFlow 端口 状态
报文 到 通知 控制器
SDN 控制器 接收 OpenFlow 报文,
链路
状态 信息
路由 算法应用 有 previously
registered 到 是 called 当 ever 链路 状态 changes. 它 是
Dijkstra’s 路由 算法访问 网络 图
信息,
链路状态 信息 在 控制器,计算 新的 路由
链路状态 路由 应用 interacts 与 流表
component 在 SDN 控制器, 哪个 计算新的 流
表
控制器 使用 OpenFlow 到install 新的 表在
交换机 这 需要 updating

topology
manager
Basic Network Service Functions
REST    API
OpenFlow 1.0
...
SNMP
OVSDB
forwarding
manager
switch
manager
host
manager
stats
manager
Network 
service apps 
Service Abstraction Layer (SAL)
Access
Control
Traffic
Engineering
...  
OpenDaylight (ODL) controller
ODL Lithium controller
network apps may be 
contained within, or be 
external to SDN controller
Service Abstraction Layer: 
interconnects internal, 
external applications and 
services
5-106
拓扑
Basic 网络 服务 功能
转发
交换机
主机
网络
服务 应用
服务 抽象 层 (SAL)
访问
控制
流量
OpenDaylight (ODL) 控制器
ODL Lithium 控制器
网络 应用 可能 是
contained 在...内, 或 是
external 到 SDN 控制器
服务 抽象 层:
external 应用 和
服务

Network 
control apps 
...  
REST    API
ONOS
distributed 
core
southbound 
abstractions,
protocols
OpenFlow
Netconf
OVSDB
devicelinkhostflow
packet
northbound 
abstractions,
protocols
Intent
statistics
devices
hosts
links
pathsflow rules
topology
O
pen
N
etwork
OScontroller
control apps separate from 
controller
intent framework: high-level 
specification of service: what 
rather than how
considerable emphasis on 
distributed core: service 
reliability, replication performance 
scaling
5-107
网络
控制 应用
分布式
核心
协议
设备
链路
主机流
分组
协议
设备
主机
链路
路径流 rules
拓扑
控制器
控制 应用 separate 从
控制器
intent 帧工作
specification 的 服务: 什么
rather 比 如何
considerable emphasis 在
分布式 核心: 服务
reliability, replication 性能

SDN:  selected challenges

hardening the control plane: dependable, reliable, 
performance-scalable, secure distributed system

robustness to failures: leverage strong theory of reliable 
distributed system for control plane

dependability, security: “baked in” from day one? 

networks, protocols meeting mission-specific requirements

e.g., real-time, ultra-reliable, ultra-secure

Internet-scaling 
5-
108
hardening 控制平面: dependable, 可靠,
性能scalable, secure 分布式 系统
robustness 到 failures: leverage strong theory 的 可靠
分布式 系统 用于 控制平面
dependability, 安全: “baked 在” 从 天 一个?
网络, 协议特定的 requirements
时间, ultra
可靠, ultra


SDN-computed versus router-computer forwarding tables:

just one example of logically-centralized-computed versus protocol 
computed (clouding)

one could imagine SDN-computed congestion control: 

controller sets sender rates based on router-reported (to controller) 
congestion levels 
SDN and the future of traditional network protocols
Network Layer: 5-109
How will implementation of 
network functionality (SDN 
versus protocols) evolve?
计算得到的 与 路由器计算机 转发表s:
仅 一个 示例 的 logically计算得到的 与 协议
计算得到的 (clouding)
一个 可以 imagine SDN计算得到的 拥塞控制:
控制器 集合 发送方 速率 基于 在 路由器reported (到 控制器)
拥塞 levels
SDN 和 future 的 传统的 网络 协议
网络层: 5
如何 将 实现 的
网络 functionality (SDN
与 协议) evolve?

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-
110
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

4-111
ICMP: internet control message protocol

used by hosts & routers to 
communicate network-level 
information

error reporting: unreachable host, 
network, port, protocol

echo request/reply (used by ping)

network-layer “above”IP:

ICMP msgs carried in IP datagrams

ICMP message:type, code plus first 
8 bytes of IP datagram causing error
TypeCodedescription
0        0         echo reply (ping)
3        0         dest. network unreachable
3        1         desthost unreachable
3        2         destprotocol unreachable
3        3         destport unreachable
3        6         destnetwork unknown
3        7         desthost unknown
4        0         source quench (congestion
control -not used)
8        0         echo request (ping)
9        0         route advertisement
10      0         router discovery
11      0         TTL expired
12      0         bad IP header
ICMPProtocol number：1
ICMP: Internet 控制报文协议
用于 由 主机 & 路由器 到
通信 网络
信息
差错 reporting: unreachable 主机,
网络, 端口, 协议
(用于 由 ping)
网络层上方
ICMP msgs 承载的 在 IP 数据报
ICMP 报文:类型, 码 加 第一个
8 字节 的 IP 数据报 causing 差错
类型码
echo 应答 (ping)
. 网络 unreachable
主机 unreachable
协议 unreachable
端口 unreachable
网络 unknown
主机 unknown
源 quench (拥塞
控制不 用于)
echo 请求 (ping)
路由 advertisement
路由器 discovery
bad IP 首部
协议 编号

4-116
Tracerouteand ICMP

source sends series of UDPsegments 
to dest

first set has TTL =1

second set has TTL=2, etc.

unlikely port number

when nth set of datagramsarrives to 
nth router:

router discards datagrams

and sends source ICMPmessages(type 
11, code 0)

ICMP messages includes name of router & 
IP address

when ICMP messages arrives, 
source records RTTs
stopping criteria:

UDPsegment eventually arrives at 
destination host

destination returns ICMP “port 
unreachable”message (type 3, code 
3)

source stops
1 probes
2 probes
3 probes
和 ICMP
源 发送 series 的报文段
到
第一个 集合 有 TTL =1
second 集合 有 TTL=2, etc.
unlikely 端口 编号
当
th 集合 的
数据报到达 到
nth 路由器:
路由器 discards数据报
和 发送源报文(类型
11, 码 0)
ICMP 报文 includes 名称 的 路由器 &
IP 地址
当 ICMP 报文 到达,
源 records RTTs
报文段 eventually 到达 在
目的地 主机
目的地 returns ICMP端口
报文 (类型 3, 码
源 stops

5.1 introduction
5.2 routing protocols

link state

distance vector
5.3 intra-AS routing in the Internet: 
OSPF
5.4 routing among the ISPs: BGP
5.5 The SDN control plane
5.6 ICMP: The Internet Control 
Message Protocol 
5.7 Network management and SNMP
Chapter 5: outline
5-
117
5.1 引言
5.2 路由协议
链路状态
距离向量
5.3 内部的
AS 路由 在 Internet:
5.4 路由 之间 ISPs: BGP
5.5 SDN 控制平面
5.6 ICMP: Internet 控制
报文 协议
5.7 网络管理 和 SNMP
第 5 章：提纲

What is network management?

autonomous systems (aka “network”): 1000s of interacting 
hardware/software components

other complex systems requiring monitoring, control:

jet airplane

nuclear power plant

others?
"Network managementincludes the deployment, integration 
and coordination of the hardware, software, and human 
elementsto monitor, test, poll, configure, analyze, evaluate, 
and control the network and element resources to meet the 
real-time, operational performance, and Quality of Service 
requirements at a reasonable cost."
5-118
什么 是 网络管理?
autonomous 系统 (aka网络1000s 的 interacting
其他 complex 系统 requiring monitoring, 控制:
网络管理
和 coordination 的hardware, 软件, 和 human
到
和 控制 网络 和 element resources到 meet
时间, operational 性能, 和 Quality 的 服务
requirements 在 a reasonable 费用

Components of network management
Network Layer: 5-119
managed device
managed device
managed device
managed device
managed device
agentdata
agent
data
agentdata
agentdata
agentdata
managing
server/controller
data
Managing server: 
application, 
typically with network
managers (humans) in the loop
Managed device: 
equipment with 
manageable, configurable 
hardware, software 
components
Data: 
device “state” 
configuration data, 
operational data, 
device statistics
Network management 
protocol: 
used by 
managing server to query, 
configure, manage device; 
used by devices to inform 
managing server of data, 
events.
Components 的 网络管理
网络层: 5
管理的 设备
管理的 设备
管理的 设备
管理的 设备
管理的 设备
代理数据
代理
数据
代理数据
代理数据
代理数据
服务器/controller
数据
Managing 服务器:
应用,
通常 与 网络
managers (humans) 在 loop
管理的 设备:
equipment 与
hardware, 软件
数据:
设备 “状态”
configuration 数据,
operational 数据,
设备 statistics
网络管理
协议:
用于 由
managing 服务器 到 查询,
configure, manage 设备;
用于 由 设备 到 inform
managing 服务器 的 数据,

Network operator approaches to management
Network Layer: 5-120
managed device
managed device
managed device
managed device
managed device
agentdata
agent
data
agentdata
agentdata
agentdata
managing
server/controller
data
CLI 
(Command Line Interface) 
•operator issues (types, scripts) direct 
to individual devices (e.g., vis ssh)
SNMP/MIB 
•operator queries/sets devices data 
(MIB) using Simple Network 
Management Protocol (SNMP)
NETCONF/YANG
•more abstract, network-wide, holistic
•emphasis on multi-device configuration 
management. 
•YANG: data modeling language 
•NETCONF: communicate YANG-
compatible actions/data to/from/among 
remote devices 
网络 operator 方法 到 管理
网络层: 5
管理的 设备
管理的 设备
管理的 设备
管理的 设备
管理的 设备
代理数据
代理
数据
代理数据
代理数据
代理数据
服务器/controller
数据
(Command 线路 接口)
operator issues (types, scripts) 直接
到 individual 设备 (e.g., vis ssh)
operator queries/sets 设备 数据
(MIB) 使用 Simple 网络
管理 协议 (SNMP)
更多 abstract, 网络
emphasis 在 multi设备 configuration
YANG: 数据 modeling language
NETCONF: 通信 YANG
compatible actions/data to/来自/among
远程 设备

SNMP protocol
Two ways to convey MIB info, commands:
agent
data
managed device
managing
entity
agent
data
managed device
managing
entity
trap msg
request
request/response mode
trap mode
response
5-121
SNMP 协议
两个 ways 到 convey MIB 信息, commands:
代理
数据
管理的 设备
代理
数据
管理的 设备
请求
响应

SNMP protocol: message types
Network Layer: 5-122
GetRequest
GetNextRequest
GetBulkRequest
manager-to-agent: “get me data”
(data instance, next data in list, 
block of data). 
Message type
Function
SetRequest
manager-to-agent: set MIB value
Response
Agent-to-manager: value, response to 
Request
TrapAgent-to-manager: inform manager
of exceptional event
SNMP 协议: 报文 types
网络层: 5
到代理:
获得 me 数据
(数据 instance, 下一个 数据 在 列表,
block 的 数据).
报文 类型
功能
到代理: 集合 MIB 值
响应
代理到manager: 值, 响应 到
请求
代理到
的 exceptional event

SNMP protocol: message formats
....
PDU
type
(0-3)
Request
ID
Error
Status
(0-5)
Error
Index
Name
ValueName
Value
....
PDU
type
4
Enterprise
Agent
Addr
Trap
Type
(0-7)
Specific
code
Time
stamp
Name
Value
Get/set header
Variables to get/set
Trap header
Trap info
SNMP PDU
5-123
More on network management: see earlier editions of text!
UDP 
UDP HdrUDP payload
SNMP 协议: 报文 formats
类型
请求
差错
状态
差错
名称
值名称
值
类型
代理
类型
特定的
码
时间
名称
值
Get/set 首部
Variables 到 get/set
Trap 首部
Trap 信息
更多 在 网络管理:
见 earlier editions 的 text!
UDP 载荷


managed device’s operational (and some configuration) data

gathered into device MIB module

400 MIB modules defined in RFC’s; many more vendor-specific MIBs
SNMP: Management Information Base (MIB)
Network Layer: 5-124
Object ID           Name                      Type                  Comments
1.3.6.1.2.1.7.1     UDPInDatagrams32-bit counter     total # datagrams delivered 
1.3.6.1.2.1.7.2    UDPNoPorts32-bit counter     # undeliverable datagrams (no application at port)
1.3.6.1.2.1.7.3    UDInErrors32-bit counter     # undeliverable datagrams (all other reasons)
1.3.6.1.2.1.7.4    UDPOutDatagrams32-bit counter    total  # datagrams sent
1.3.6.1.2.1.7.5    udpTableSEQUENCE          one entry for each port currently in use
agentdata
Structure of Management Information (SMI): data definition language
example MIB variables for UDP protocol:
管理的 设备’s operational (和 一些 configuration) 数据
gathered 进入 设备
400 MIB modules defined 在 RFC’s; 许多 更多 vendor特定的 MIBs
SNMP: 管理 信息 基 (MIB)
网络层: 5
对象 ID名称类型
比特 counter
total # 数据报 delivered
比特 counter
# undeliverable 数据报 (无 应用 在 端口)
比特 counter
# undeliverable 数据报 (所有 其他 reasons)
比特 counter
total # 数据报 已发送
一个 entry 用于 每个 端口 currently 在 使用
代理数据
结构 的 管理 信息 (SMI):
数据 definition language
示例 MIB variables 用于 UDP 协议:


goal
: actively manage/configuredevices network-wide

operates between managing server and managed network 
devices
actions: retrieve, set, modify, activate configurations

atomic-commit actions over multiple devices

query operational data and statistics

subscribe to notifications from devices

remote procedure call (RPC) paradigm

NETCONF protocol messages encoded in XML

exchanged over secure, reliable transport 
(e.g., TLS)
protocol
NETCONF (Network Configuration Protocol) overview
Network Layer: 5-125
目标
设备 网络
operates 之间 managing 服务器 和 管理的 网络
设备
动作: retrieve, 集合, 修改, activate configurations
动作 在 多路 设备
查询 operational 数据 和 statistics
subscribe 到 notifications 从 设备
远程 procedure 调用 (RPC) paradigm
NETCONF 协议 报文 encoded 在 XML
exchanged 在 secure, 可靠 运输协议
NETCONF (网络 Configuration 协议) 概述
网络层: 5

NETCONF overview
Network Layer: 5-126
NETCONF 概述
网络层: 5

NETCONF overview

NETCONF认为网络的模型数据可以分为两大类: 状态数据和配
置数据。

状态数据一般指server（设备）的固有属性数据和当前运行的状态数据
等，这类数据仅能查询。

配置数据则是指由用户（以某种方式）配置到server上的数据。而配置
数据本身又可以存在多个数据库，标准中提到了<running/>库用于保存
当前已经生效的配置；<candidate/>用于保存可以提交为生效的数据；
以及<startup/>用于保存启动时的配置数据。
Network Layer: 5-127
NETCONF 概述
服务器
服务器
网络层: 5

NETCONF overview

安全传输层

NETCONF是一个应用层协议, 承载在安全传
输层之上, NETCONF协议中并没有规定建链
和保活相关的内容。

消息层

NETCONF中定义了三种消息类型，分别是
hello，rpc和rpc-reply, notification。

操作层

操作层仅承载在<rpc>和<rpc-reply>消息上，
<hello>和<notification>消息无操作层。

NETCONF协议规定了简单的rpc操作，同时
也支持用户自定义rpc操作。
Network Layer: 5-128
NETCONF 概述
应答, notification
网络层: 5

Selected NETCONF Operations
Network Layer: 5-129
NETCONF                        Operation Description
<get-config>                 Retrieve all or part of a given configuration. A device may have multiple 
configurations. 
<get>                            Retrieve all or part of both configuration state and operational state data.
<edit-config>                Change specified (possibly running) configuration at managed device. 
Managed device <rpc-reply> contains <ok>  or <rpcerror> with rollback.
<copy-config>              Copy a database
<delete-config>            Delete a database
<lock>, <unlock>          Lock (unlock) configuration datastore at managed device (to lock out 
NETCONF, SNMP, or CLIs commands from other sources).
<create-subscription>, Enable event notification subscription from managed device
<notification>
<close-session>
<kill-session>
网络层: 5
Retrieve 所有 或 部分 的 a 给定的 configuration. 设备 可能 有 多路
Retrieve 所有 或 部分 的 二者 configuration 状态 和 operational 状态 data.
变化 specified (possibly 运行) configuration 在 管理的 device.
管理的 设备 <reply> 包含 <ok> 或 <> 与 rollback.
复制 a 数据库
删除 a数据库
Lock (unlock) configuration datastore 在 管理的 设备 (到 lock 查看
NETCONF, SNMP, 或 CLIs commands 从 其他 sources).
subscription>, Enable event notification subscription 从 管理的 设备

NETCONF overview

内容层

在netconf出现之前，我们所熟知
且常用的协议，均采用在协议中
规定报文的结构体，并按字节流
读取并解析的架构。(例如: TLV)

缺乏扩展性

netconf协议本身没有对内容层的
数据结构做任何的限定。而其规
范则体现在其内容层需要使用
Yang   (Yet Another Next 
Generation）语言对其数据进行
建模。
Network Layer: 5-130
NETCONF 概述
(Yet 另一个 下一个
网络层: 5

NETCONF overview

YANG相对于SNMP的
模型MIB，更有层次化
，能够区分配置和状态
，可扩展性强。

设备解析模型时用YIN
（YANG Independent 
Notation）模型文件。
YIN是XML表达方式的
YANG，YIN与YANG
之间使用不同的表达方
法但包含等价的信息。
Network Layer: 5-131
用YIN是为了利用各编程语言中现有的XML解析器等工具。
这些工具可用来进行数据过滤和验证，自动生成代码和文
件或者其他任务，这样可以提升设备解析YANG模型的效率
。
NETCONF 概述
记号
网络层: 5

Sample NETCONF RPC message
Network Layer: 5-132
note message id
change the running configuration 
change MTU of Ethernet 0/0 interface to 1500
change a configuration
Sample NETCONF RPC 报文
网络层: 5
说明 报文 id
变化 运行 configuration
变化 MTU 的 以太网 0/0 接口 到 1500
变化 a configuration

NETCONF initialization, exchange, close
Network Layer: 5-133
Session initiation, 
capabilities exchange: <hello>
Session close:  <close-session>
<rpc>
<rpc-reply>
<rpc>
<rpc-reply>
<rpc>
<rpc-reply>
<notification>
...
...
...
...
...
...
...
...
...
managing
server/controller
data
agentdata
NETCONF initialization, 交换, 关闭
网络层: 5
capabilities 交换: <hello>
Session 关闭: <close
服务器/controller
数据
代理数据

...
...
...
...
...
...
...
...
...
Network layer:  Summary
Network Layer: 5-135
we’ve learned a lot!
approaches to network control plane
•per-router control (traditional)
•logically centralized control (software defined networking)
traditional routing algorithms
•implementation in Internet: OSPF , BGP
SDN controllers
•implementation in practice: ODL, ONOS
Internet Control Message Protocol
network management
next stop:  link layer!
网络层:总结
网络层: 5
我们’ve learned a lot!
方法 到 网络 控制平面
每路由器 控制 (传统的)
逻辑集中式控制 (软件定义网络)
传统路由算法
实现 在 Internet: OSPF , BGP
SDN 控制器
实现 在 practice: ODL, ONOS
Internet 控制报文协议
网络管理
下一个 stop: 链路层!

Chapter 5: 
summary
we’ve learned a lot!

approaches to network control plane

per-router control (traditional)

logically centralized control (software defined 
networking)

traditional routing algorithms

implementation in Internet: OSPF, BGP

SDN controllers

implementation in practice: ODL, ONOS

Internet Control Message Protocol

network management
next stop:  link layer!
5-
136
第 5 章:
总结
我们’ve learned a lot!
方法 到 网络 控制平面
每路由器 控制 (传统的)
逻辑集中式控制 (软件 defined
网络)
传统路由算法
实现 在 Internet: OSPF, BGP
SDN 控制器
实现 在 practice: ODL, ONOS
Internet 控制报文协议
网络管理
下一个 stop: 链路层!

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
