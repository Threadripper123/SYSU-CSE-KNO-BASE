# Chapter5 Network Layer - 2

> 主题库：计算机网络　｜　源文件：Chapter5 Network Layer - 2.pdf　｜　50,225 字符　｜　126 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall

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

Assignments :

Ch5 (ver8, cn): 3, 7, 11,14, 16, 19

A team project

Keywords: Forwarding and Routing, Virtual Circuit and Datagram 
Networks, Addressing, DHCP, NAT, ICMP, Routing Algorithms (DV, 
LS), RIP, OSPF, BGP 

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

Distance vector example: iteration
Network Layer: 5-34
.... and so on
Let’s next take a look at the iterative computationsat nodes

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

72
Theoretically,aborderroutercantransmittheAS
accessibilitymessagetoallinternalroutersthrough
iBGP,includingallnon-borderrouters.However,this
willsharplyincreasetheburdenontheinternalrouter,
becausetheroutingtableofanycorerouterisvery
largeandchangesfrequently.
Therefore,itisnotagoodideatopropagatetheAS
accessibilitymessagetoallinternalrouters.

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

5.4.6 Putting the Pieces Together: Obtaining 
Internet Presence

Please read the Section 5.4.6 carefully

IP addressing, DNS, BGP and intra-AS routing
88

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

NETCONF overview
Network Layer: 5-126

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

Sample NETCONF RPC message
Network Layer: 5-132
note message id
change the running configuration 
change MTU of Ethernet 0/0 interface to 1500
change a configuration

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

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
