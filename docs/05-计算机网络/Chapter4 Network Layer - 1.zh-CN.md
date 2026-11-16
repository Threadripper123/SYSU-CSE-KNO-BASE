# Chapter4 Network Layer - 1.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter4 Network Layer - 1.zh-CN.pdf　｜　58,066 字符　｜　103 个片段　｜　更新于 2026-11-11


Computer Networking
䉒䙨
ѝኡབྷᆖg䇑㇇ᵪᆖ䲒
2024. Fall
计算机网络
2024. 秋季

Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
Chapter 4
Network Layer:
Data Plane
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)
If you post any slides on a www site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2020
J.F Kurose and K.W. Ross, All Rights Reserved
计算机 网络:
Down 方法
第 4 章
网络层:
数据平面
说明 在 使用 的 这些 PowerPoint 幻灯片:
我们’
re making 这些 幻灯片 freely av
ailable 到 所有 (f
readers). They’re 在 PowerPoint fo
rm 因此 你 见 animations;
和 可以 添加, 修改, 和 删除 slides (包括 这个 一个) 和
幻灯片 内容 到 suit your 需要
的
工作 在 我们的 part. 在 返回 用于
使用, 我们 仅 ask following:
如果 你 使用 这些 幻灯片 (e.g., 在
a 课堂) 这 你 mention 它们的
源 (之后 所有, 我们’
d 像 people 到 使用 我们的 book!)
如果 你 post 任何 幻灯片 在 a 网站
站点, 这 你 说明 这 they
是 adapted 从 (或 perhaps iden
tical 到) 我们的 幻灯片, 和 说明
我们的 版权 的 这个 material.
用于 a revision 历史, 见 th
e 幻灯片 说明 用于 这个 page.
谢谢 和 使用愉快! JFK/KWR
所有 材料 版权 1996-2020
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved

Assignments :
z(CN, ver8)  1ǃ4ǃ6ǃ11ǃ14ǃ15ǃ18ǃ21ǃ23ǃ24
zA team project
zKeywords: Forwarding and Routing, Virtual Circuit and Datagram 
Networks, Switch, IPv4, Addressing, DHCP, NAT, IPv6, SDN, flow 
table, Openflow
作业：
团队项目
关键词： 转发 和 路由, Virtual 电路 和 数据报
网络, 交换机, IPv4, 编址, DHCP, NAT, IPv6, SDN, 流
表, Openflow

4.1 Overview of Network layer
data plane
control plane
4.2 What’s inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-4
4.1 概述 的 网络层
数据平面
控制平面
4.2 什么
’s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
网络 地址 翻译
4.4 Generalized 转发 和 SDN
匹配
动作
OpenFlow 示例 的 match-
plus-action 在 动作
第 4 章：提纲

Chapter 4: network layer
chapter goals:
understand principles behind network layer services, 
focusing on data plane:
•network layer service models
•forwarding versus routing
•how a router works
•generalized forwarding
instantiation, implementation in the Internet
4-5
第 4 章：网络层
本章目标：
理解网络层服务背后的原理，
focusing 在 数据平面:
网络层服务模型
转发与路由
路由器如何工作
通用转发
在 Internet 中的实例化与实现

4-6
Network layer
ztransport segment from sending to 
receiving host 
zon sending side encapsulates 
segmentsinto datagrams
zon receiving side, delivers segments 
to transport layer
znetwork layer protocols in everyhost, 
router
zrouter examines header fields in all IP 
datagrams passing through it
application
transport
network
data link
physical
application
transport
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
IP payload
IPH
TCP payload
TCPH
网络层
运输 报文段 从 发送 到
接收 主机
在 发送 一侧 encapsulates
报文段进入数据报
在 接收 一侧, 交付 报文段
到 运输层
网络层 协议 在每一个主机,
路由器
路由器 examines 首部 字段 在 所有 IP
数据报 passing 通过 它
应用
运输
网络
数据 链路
物理
应用
运输
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
网络
数据 链路
物理
IP 载荷
TCP 载荷

4-7
Two key network-layer functions
zrouting:determine route 
taken by packets from source 
to dest. 
routing algorithms
zforwarding:move packets 
from router䇻s input to 
appropriate router output
zConnection Setup
Not included in IP
analogy:
routing:process of planning trip 
from source to dest
forwarding:process of getting 
through single interchange
两个 关键 网络-layer 功能
路由确定 路由
经过 由 分组 从 源
到 dest.
路由 算法
转发移动 分组
从 路由器s 输入 到
合适的 路由器 输出
连接建立
不 included 在 IP
路由:进程 的
从 源 到 dest
转发进程 的
通过单个 interchange

Network layer: data plane, control plane
Data plane
local, per-router function
determines how datagram 
arriving on router input port is 
forwardedto router output port
forwarding function
Control plane
network-wide logic
determines how datagram is routed
among routers along end-
end path from 
source host to destination host
two control-plane approaches:
•traditional routing algorithms: implemented 
in routers
•software-defined networking (SDN): 
implemented in (remote) servers
1
2
3
0111
values in arriving 
packet header
33
1
4-8
网络层：数据平面, 控制平面
数据平面
本地, per-路由器 功能
determines 如何 数据报
到达的 在 路由器 输入 端口 是
转发到 路由器 输出 端口
转发 功能
控制平面
determines 如何 数据报 是
之间 路由器 along end-
端 路径 从
源 主机 到 目的地 主机
两个 control-plane 方法:
传统路由算法:
在 路由器
software-defined 网络 (SDN)
implemented 在 (远程) 服务器
值 在 到达的
分组 首部

Per-router control plane
Routing
Algorithm
Individual routing algorithm components in each and every router interact in the 
control plane
data
plane
control
plane
5-9
1
2
0111
1
values in arriving 
packet header
3
Per-路由器 控制平面
路由
算法
Individual 路由 算法
在 每个 和 每一个 路由器
interact 在
控制平面
数据
平面
控制
平面
值 在 到达的
分组 首部

data
plane
control
plane
Logically centralized control planepgyyyyyyyyyyyyyy
A distinct (typically remote) controller interacts with local control agents (CAs)
Remote Controller
CA
CA
CA
CA
CA
5-10
1
2
0111
1
3
values in arriving 
packet header
数据
平面
控制
平面
Logically centralized 控制平面
distinct (通常 远程) 控制器 interacts 与 本地 控制 agents (CAs)
远程 控制器
值 在 到达的
分组 首部

4-11
Network service model
Q:What service modelfor Āchannelātransporting datagrams from sender 
to receiver?
example services for individual 
datagrams:
guaranteed delivery
guaranteed delivery with less than 
40 msec delay
example services for a flow of 
datagrams:
zin-orderdatagram delivery
zguaranteed minimum bandwidth to 
flow
zrestrictions on changes in inter-
packet spacing
网络 服务模型
什么服务模型
用于
信道transporting 数据报 从 发送方
到 接收方?
示例 服务 用于 individual
数据报:
guaranteed 交付
guaranteed 交付 与 less 比
40 msec 时延
示例 服务 用于 a 流 的
数据报:
数据报 交付
guaranteed minimum 带宽 到
流
restrictions 在 变化 在 inter-
分组 spacing

Network-layer service model
Network
Architecture
Internet
ATM
ATM
Internet
Internet
Service
Model
best effort
Constant Bit Rate
Available Bit Rate
Intserv Guaranteed
(RFC 1633)
Diffserv
(RFC 2475) 
Bandwidth
none
Constant rate
Guaranteed min
yes
possible
Loss
no
yes
no
yes
possibly
Order
no
yes
yes
yes
possibly
Timing
no
yes
no
yes
no
EŽŐƵĂƌĂŶƚĞĞƐŽŶ͗
ŝ͘ ƐƵĐĐĞƐƐĨƵůĚĂƚĂŐƌĂŵĚĞůŝǀĞƌLJƚŽĚĞƐƚŝŶĂƚŝŽŶ
ŝŝ͘ ƚŝŵŝŶŐŽƌŽƌĚĞƌŽĨĚĞůŝǀĞƌLJ
ŝŝŝ͘ďĂŶĚǁŝĚƚŚĂǀĂŝůĂďůĞƚŽĞŶĚͲĞŶĚĨůŽǁ
/ŶƚĞƌŶĞƚ͞ďĞƐƚĞĨĨŽƌƚ͟ƐĞƌǀŝĐĞŵŽĚĞů
Quality of Service (QoS) Guarantees ?
Network Layer: 4-12
网络层 服务模型
网络
体系结构
服务
模型
尽力而为
Constant 比特 速率
可用的 比特 速率
带宽
Constant 速率
Guaranteed 最小值
是
可能
丢包
无
是
无
是
无
是
是
是
无
是
无
是
无
Quality 的 服务 (QoS) Guarantees ?
网络层: 4-12

4-13
Network layer service models:
Network
Architecture
Internet
ATM
ATM
ATM
ATM
Service
Model
best effort
CBR
VBR
ABR
UBR
Bandwidth
none
constant
rate
guaranteed
rate
guaranteed 
minimum
none
Loss
no
yes
yes
no
no
Order
no
yes
yes
yes
yes
Timing
no
yes
yes
no
no
Congestion
feedback
no (inferred
via loss)
no
congestion
no
congestion
yes
no
Guarantees?
UBR˖Unspecified Bit Rate
CBR˖Constant Bit Rate
ABR˖Available Bit Rate
VBR˖Variable Bit Rate
网络层 服务模型:
网络
体系结构
服务
模型
尽力而为
带宽
速率
速率
丢包
无
是
是
无
无
无
是
是
是
是
无
是
是
无
无
拥塞
无 (inferred
通过 丢包)
无
拥塞
无
拥塞
是
无
Unspecified 比特 速率
Constant 比特 速率
可用的 比特 速率
变量 比特 速率

Reflections on best-effort  service:
simplicity of mechanism has allowed Internet to be widely 
deployed adopted
sufficient provisioning of bandwidthallows performance of 
real-time applications (e.g., interactive voice, video) to be 
“good enough” for “most of the time”
replicated, application-layer distributed services 
(datacenters, content distribution networks) connecting 
close to clients’ networks, allow services to be provided 
from multiple locations
congestion control of “elastic” services helps
/ƚ͛ƐŚĂƌĚƚŽĂƌŐƵĞǁŝƚŚƐƵĐĐĞƐƐŽĨďĞƐƚͲĞĨĨŽƌƚƐĞƌǀŝĐĞŵŽĚĞů
Network Layer: 4-14
Reflections 在 best-effort 服务:
simplicity 的 mechanism有 allowed Internet 到 是 widely
provisioning 的 带宽允许 性能 的
real-time 应用 (e.g., 交互式 voice, 视频) 到 是
“好的 enough” 用于 “most 的 时间”
replicated, 应用-layer 分布式 服务
(datacenters, 内容分发网络) connecting
关闭 到 客户’ 网络, allow 服务 到 是 提供
从 多路 locations
拥塞控制 的 “elastic” 服务 helps
网络层: 4-14

4.1 Overview of Network layer
data plane
control plane
4.2 What’s inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-15
4.1 概述 的 网络层
数据平面
控制平面
4.2 什么’
s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
网络 地址 翻译
4.4 Generalized 转发 和 SDN
匹配
动作
OpenFlow 示例 的 match-
plus-action 在 动作
第 4 章：提纲

4-16
Router architecture overview
two key router functions:
run routing algorithms/protocol (RIP, OSPF, BGP)
forwardingdatagrams from incoming to outgoing link
high-seed 
switching
fabric
routing 
processor
router input ports
router output ports
forwarding data 
plane  (hardware)
routing, management
control plane (software)
forwarding tables computed,
pushed to input ports
r1
r2
r4
r3
Which router is the best 
choice for a given dstIP?
r1
Which port goes to 
the given router?
路由器 体系结构 概述
两个 关键 路由器 功能:
运行
路由
转发数据报 从 传入的 到 outgoing 链路
交换
结构
路由
路由器 输入 端口
路由器 输出 端口
转发 数据
平面 (hardware)
路由, 管理
控制平面 (软件)
转发表s 计算得到的,
pushed 到 输入 端口
哪个 路由器 是 best
choice 用于 a 给定的 dstIP?
哪个 端口 goes 到
给定的 路由器?

4-17
line
termination
link 
layer 
protocol
(receive)
lookup,
forwarding
queueing
Input port functions
decentralized switching:
zgiven datagram dest., lookup output port using 
forwarding table in input port memory (“
match plus 
action”)
zgoal: complete input port processing at 䇺line 
speed䇻
zqueuing: if datagramsarrive faster than forwarding 
rate into switch fabric
physical layer:
bit-level reception
data link layer:
e.g., Ethernet
see chapter 5
switch
fabric
线路
链路
层
协议
(接收)
转发
排队
输入 端口 功能
分散式 交换
给定的 数据报, lookup 输出 端口 使用
转发表 在 输入 端口 内存
匹配 加
动作
目标: 完整的 输入 端口 processing 在
线路
queuing: 如果 数据报到达 faster 比 转发
速率 进入 交换机 结构
物理 层:
数据 链路层:
e.g., 以太网
见 第 5 章
交换机
结构

Destination Address Range
11001000 00010111 00010000 00000000
through
11001000 00010111 00010111 11111111
11001000 00010111 00011000 00000000
through
11001000 00010111 00011000 11111111
11001000 00010111 00011001 00000000
through
11001000 00010111 00011111 11111111
otherwise
Link Interface
0
1
2
3
Q:but what happens if 
ranges don䇻t divide up 
so nicely? 
Destination-based forwarding
forwarding table
4-18
192.168.1.1 ~ 192.168.1.5       r1
192.168.1.6 ~ 192.168.1.10     r2
192.168.1.11 ~ 192.168.1.15    r1
192.168.1.16 ~ 192.168.1.20    r2
...
目的地 地址 Range
通过
通过
通过
链路 接口
但是 什么 happens 如果
t divide 上
因此 nicely?
Destination-based 转发
转发表

Longest prefix matching
Destination Address Range
11001000 00010111 00010*** *********
11001000 00010111 00011000 *********
11001000 00010111 00011*** *********
otherwise
DA: 11001000  00010111  00011000  10101010
examples:
DA: 11001000  00010111  00010110  10100001 
which interface?
which interface?
when looking for forwarding table entry for given 
destination address, use longestaddress prefix that 
matches destination address.
longest prefix matching
Link interface
0
1
2
3
4-19
目的地 地址 Range
示例:
哪个 接口?
哪个 接口?
当 looking 用于 转发表 entry 用于 给定的
目的地 地址, 使用地址 prefix 这
匹配 目的地 address.
链路 接口

Longest prefix matching
zwe’ll seewhy longest prefix matching is used shortly, when 
we study addressing
zlongest prefix matching: often performed using ternary 
content addressable memories(TCAMs)
content addressable: present address to TCAM: retrieve address in 
one clock cycle, regardless of table size
Cisco Catalyst: can up ~1M routing table entries in TCAM
4-20
我们
ll 见longest prefix matching 是 用于 shortly, 当
我们 study 编址
longest prefix matching: 经常 performed 使用
内容 addressable memories
内容 addressable:present 地址 到 TCAM: retrieve 地址 在
一个 clock cycle, regardless 的 表 大小
Cisco Catalyst: 可以 上 ~1M 路由表 entries 在 TCAM

21
Longest prefix matchingggp
ternary content addressable 
memories(TCAMs)
ternary 内容 addressable

4-22
Switching fabrics
transfer packet from input buffer to appropriate output buffer
switching rate: rate at which packets can be transfer from 
inputs to outputs
often measured as multiple of input/output line rate
N inputs: switching rate N times line rate desirable
three types of switching fabrics
memory
memory
bus
crossbar
交换 fabrics
传输 分组 从 输入 缓冲区 到 合适的 输出 缓冲区
交换 速率: 速率 在 哪个 分组 可以 是 传输 从
inputs 到 outputs
经常 measured as 多路 的 input/output 线路 速率
N inputs: 交换 速率 N times 线路 速率 desirable
three types 的 交换 fabrics
内存
内存
总线

4-23
Switching via memory
first generation routers:
ztraditional computers with switching under direct control of CPU
zpacket copied to system䇻s memory
zspeed limited by memory bandwidth (2 bus crossings per datagram)
input
port
(e.g.,
Ethernet)
memory
output
port
(e.g.,
Ethernet)
system bus
交换 通过 内存
第一个 generation 路由器:
传统的 computers 与 交换机ing under 直接 控制 的 CPU
分组 copied 到 系统s 内存
speed limited 由 内存 带宽 (2 总线 crossings 每 数据报)
输入
端口
以太网)
内存
输出
端口
以太网)
系统 总线

4-24
Switching via a bus
datagram from input port memory
to output port memory via a shared 
bus
bus contention:switching speed 
limited by bus bandwidth
32 Gbps bus, Cisco 5600: sufficient 
speed for access and enterprise 
routers
bus
交换 通过 a 总线
数据报 从 输入 端口 内存
到 输出 端口 内存 通过 a 共享
总线
总线 contention:交换 speed
limited 由 总线 带宽
32 Gbps 总线, Cisco 5600: sufficient
speed 用于 访问 和 enterprise
路由器
总线

4-25
Switching via interconnection network
overcome  bus bandwidth limitations
banyan networks, crossbar, other 
interconnection nets initially 
developed to connect processors in 
multiprocessor
advanced design: fragmenting 
datagram into fixed length cells, 
switch cells through the fabric. 
Cisco 12000: switches 60 Gbps
through the interconnection network
crossbar
ΘdžΘŵƵůƚŝƐƚĂŐĞƐǁŝƚĐŚ
ďƵŝůƚĨƌŽŵƐŵĂůůĞƌͲƐŝnjĞĚƐǁŝƚĐŚĞƐ
交换 通过 interconnection 网络
overcome 总线 带宽 limitations
banyan 网络, crossbar, 其他
interconnection nets 初始时
developed 到 连接 processors 在
数据报 进入fixed 长度
交换机 cells 通过 fabric.
Cisco 12000: 交换机 60 Gbps
通过 interconnection 网络

26
1
2
3
4
1
2
3
4
123 41234
crossbar
䙊/ޣᯝᔰ
Switching via interconnection network
交换 通过 interconnection 网络

scaling, using multiple switching “planes” in parallel: 
speedup, scaleup via parallelism
Switching via interconnection network
ĨĂďƌŝĐƉůĂŶĞϬ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϬ
ĨĂďƌŝĐƉůĂŶĞϭ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϭ
ĨĂďƌŝĐƉůĂŶĞϮ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϮ
ĨĂďƌŝĐƉůĂŶĞϯ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϯ
ĨĂďƌŝĐƉůĂŶĞκ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞκ
ĨĂďƌŝĐƉůĂŶĞρ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞρ
ĨĂďƌŝĐƉůĂŶĞς
͘͘͘
͘͘͘
͘
͘
͘
ĨĂďƌŝĐƉůĂŶĞς
ĨĂďƌŝĐƉůĂŶĞϳ
͘͘͘
͘͘͘
ŝƐĐŽZ^ƌŽƵƚĞƌ͗
ďĂƐŝĐƵŶŝƚ͗ΘƐǁŝƚĐŚŝŶŐƉůĂŶĞƐ
ĞĂĐŚƉůĂŶĞ͗ϯͲƐƚĂŐĞŝŶƚĞƌĐŽŶŶĞĐƚŝŽŶ
ŶĞƚǁŽƌŬ
ƵƉƚŽϭϬϬ͛ƐdďƉƐƐǁŝƚĐŚŝŶŐĐĂƉĂĐŝƚLJ
Network Layer: 4-27
scaling, 使用 多路 交换 “平面” 在 parallel:
speedup, scaleup 通过 parallelism
交换 通过 interconnection 网络
网络层: 4-27

͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘
͘
͘
͘͘͘
͘͘͘
4-28
Input port queuing
zfabric slower than input ports combined -> queueing may occur at input 
queues 
queueing delay and loss due to input buffer overflow!
zHead-of-the-Line (HOL) blocking:queued datagram at front of queue 
prevents others in queue from moving forward
output port contention:
only one red datagram can be 
transferred.
lower red packet is blocked
switch
fabric
one packet time later: 
green packet 
experiences HOL 
blocking
switch
fabric
输入 端口 queuing
结构 slower 比 输入 端口 combined -> 排队 可能 occur 在 输入
排队 时延 和 丢包 由于 到 输入 缓冲区 overflow!
queued 数据报 在 front 的 队列
prevents others 在 队列 从 moving 转发
输出 端口 contention:
仅 一个 red 数据报 可以 是
lower red 分组 是 blocked
交换机
结构
一个 分组 时间 later:
green 分组
交换机
结构

4-29
Output ports
bufferingrequired when datagrams arrive from fabric faster 
than the transmission rate
scheduling disciplinechooses among queued datagrams for 
transmission
line
termination
link 
layer 
protocol
(send)
switch
fabric
datagram
buffer
queueing
This slide in HUGELY important!
Datagram (packets) can be lost 
due to congestion, lack of buffers
Priority scheduling – who gets best 
performance, network neutrality
Why is the lose in output serious than the input?
输出 端口
required 当 数据报 到达 从 结构 faster
比 传输 速率
chooses 之间 queued 数据报 用于
传输
线路
链路
层
协议
(发送)
交换机
结构
数据报
缓冲区
排队
这个 幻灯片 在 HUGELY 重要!
数据报 (分组) 可以 是 丢失的
由于 到 拥塞, lack 的 缓冲区
优先级 scheduling – who 获得 best
性能, 网络 neutrality
Why 是 lose 在 输出 serious 比 输入?

4-30
Output port queueing
zbuffering when arrival rate via switch exceeds output line 
speed
zqueueing (delay) and loss due to output port buffer overflow!
at t,packets more
from input to output
one packet time later
switch
fabric
switch
fabric
输出 端口 排队
buffering 当 arrival 速率 通过 交换机 exceeds 输出 线路
排队 (时延) 和 丢包 由于 到 输出 端口 缓冲区 overflow!
在分组 更多
从 输入 到 输出
一个 分组 时间 later
交换机
结构
交换机
结构

4-31
How much buffering?
zRFC 3439 rule of thumb: average buffering equal to 䇾typical䇿
RTT (say 250 msec) times link capacity C
e.g., C = 10 Gpbs link: 2.5 Gbit buffer
zrecent recommendation: with Nflows, buffering equal to 
RTT  C
.
N
如何 much buffering?
RFC 3439 rule 的 thumb: average buffering equal 到
RTT (say 250 msec) times 链路 容量 C
e.g., C = 10 Gpbs 链路: 2.5 Gbit 缓冲区
recent recommendation: 与flows, buffering equal 到

Buffer Management
ďƵĨĨĞƌŵĂŶĂŐĞŵĞŶƚ͗
ĚƌŽƉ͗
ǁŚŝĐŚƉĂĐŬĞƚƚŽĂĚĚ͕ĚƌŽƉǁŚĞŶ
ďƵĨĨĞƌƐĂƌĞĨƵůů
•ƚĂŝůĚƌŽƉ͗ĚƌŽƉĂƌƌŝǀŝŶŐƉĂĐŬĞƚ
•ƉƌŝŽƌŝƚLJ͗ĚƌŽƉͬƌĞŵŽǀĞŽŶƉƌŝŽƌŝƚLJ
ďĂƐŝƐ
line
termination
link 
layer 
protocol
(send)
switch
fabric
datagram
buffer
queueing 
scheduling
ŵĂƌŬŝŶŐ͗ǁŚŝĐŚƉĂĐŬĞƚƐƚŽŵĂƌŬƚŽ
ƐŝŐŶĂůĐŽŶŐĞƐƚŝŽŶ;E͕ZͿ
Z
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
ďƐƚƌĂĐƚŝŽŶ͗ƋƵĞƵĞ
Z
Network Layer: 4-32
缓冲区 管理
线路
链路
层
协议
(发送)
交换机
结构
数据报
缓冲区
排队
队列
(waiting 区域)
分组
分组
链路
(服务器)
网络层: 4-32

Scheduling mechanisms
zscheduling: choose next packet to send on link
zFIFO (first in first out) scheduling: send in order of arrival to 
queue
real-world example?
discard policy: if packet arrives to full queue: who to discard?
tail drop: drop arriving packet
priority: drop/remove on priority basis
random: drop/remove randomly
g
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
4-33Network Layer: Data Plane
选择 下一个 分组 到 发送 在 链路
FIFO (第一个 在 第一个 查看) scheduling:发送 在 order 的 arrival 到
队列
real-world 示例?
discard 策略:如果 分组 到达 到 完整的 队列: who 到 discard?
drop 到达的 分组
优先级:drop/remove 在 优先级 basis
随机:
队列
(waiting 区域)
分组
分组
链路
(服务器)
网络层：数据平面

packet scheduling: deciding 
which packet to send next on link
first come, first served
priority
round robin
weighted fair queueing
Packet Scheduling: FCFS
&&^͗ƉĂĐŬĞƚƐƚƌĂŶƐŵŝƚƚĞĚŝŶ
ŽƌĚĞƌŽĨĂƌƌŝǀĂůƚŽŽƵƚƉƵƚ
ƉŽƌƚ
ĂůƐŽŬŶŽǁŶĂƐ͗&ŝƌƐƚͲŝŶͲĨŝƌƐƚͲ
ŽƵƚ;&/&KͿ
ƌĞĂůǁŽƌůĚĞdžĂŵƉůĞƐ͍
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
ďƐƚƌĂĐƚŝŽŶ͗ƋƵĞƵĞ
Z
Network Layer: 4-34
分组 scheduling:
哪个 分组 到 发送 下一个 在 链路
第一个 come, 第一个 served
优先级
weighted fair 排队
分组 Scheduling: FCFS
队列
(waiting 区域)
分组
分组
链路
(服务器)
网络层: 4-34

Scheduling policies: priority
priority scheduling: send 
highest priority queued 
packet 
zmultiple classes, with 
different priorities
class may depend on marking 
or other header info, e.g. IP 
source/dest, port numbers, etc.
real world example? 
high priority queue
(waiting area)
low priority queue
(waiting area)
arrivals
classify
departures
link
(server)
4-35
1
3
2
4
5
5
5
2
2
1
1
3
3
4
4
arrivals
departures
packet 
in 
service
Scheduling policies: 优先级
优先级 scheduling:发送
highest 优先级 queued
分组
多路, 与
不同的 priorities
课堂 可能 depend 在 marking
或 其他 首部 信息, e.g. IP
source/dest, 端口 编号, etc.
real world 示例?
high 优先级 队列
(waiting 区域)
low 优先级 队列
(waiting 区域)
链路
(服务器)
分组
在
服务

Scheduling policies: still more
Round Robin (RR) scheduling:
zmultiple classes
zcyclically scan class queues, sending one complete 
packet from each class (if available)
zreal world example?
4-36
arrivals
classify
departures
link
(server)
1
2
3
4
5
5
5
2
3
1
1
3
3
4
4
arrivals
departures
packet 
in 
service
Scheduling policies: 仍然 更多
多路 classes
cyclically scan 课堂 queues, 发送一个 完整的
分组 从 每个 课堂(如果 可用的)
real world 示例?
链路
(服务器)
分组
在
服务

Weighted Fair Queuing (WFQ): 
zgeneralized Round Robin
zeach class gets weighted amount of service in each cycle
zreal-world example?
Scheduling policies: still more
4-37
每个 课堂 获得 weighted 数量 的 服务 在 每个 cycle
real-world 示例?
Scheduling policies: 仍然 更多

4.1 Overview of Network layer
data plane
control plane
4.2 Whatÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-41
4.1 概述 的 网络层
数据平面
控制平面
4.2 什么
s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
网络 地址 翻译
4.4 Generalized 转发 和 SDN
匹配
动作
OpenFlow 示例 的 match-
plus-action 在 动作
第 4 章：提纲

4-42
The Internet network layer
forwarding
table
host, router network layer functions:
routing protocols
•path selection
•RIP, OSPF, BGP
IP protocol
•addressing conventions
•datagram format
•packet handling conventions
ICMP protocol
•error reporting
•router 
Āsignalingā
transport layer: TCP, UDP
link layer
physical layer
network
layer
Internet 网络层
转发
表
主机, 路由器 网络层 功能:
路由协议
路径 selection
IP 协议
编址 conventions
数据报 格式
分组 处理 conventions
ICMP 协议
差错 reporting
路由器
运输层: TCP, UDP
链路层
物理 层
网络
层

4-43
ver
length
32 bits
data 
(variable length,
typically a TCP 
or UDP segment)
16-bit identifier
header
checksum
time to
live
32 bit source IP address
Head
er len
type of
service
flgs
fragment
offset
upper
layer
32 bit destination IP address
options (if any)
IP datagram format
IP protocol version
number
header length
(bytes)
upper layer protocol
to deliver payload to
total datagram
length (bytes)
Ātypeāof data 
for
fragmentation/
reassembly
max number
remaining hops
(decremented at 
each router)
e.g. timestamp,
record route
taken, specify
list of routers 
to visit.
how much overhead?
20 bytes of TCP
20 bytes of IP
= 40 bytes + app 
layer overhead
“16-bit identifier” is NOT a number for reordering
No reordering in Net Layer
TCP=6;
UDP=17
长度
32 比特
数据
(变量 长度,
通常 a TCP
或 UDP 报文段)
首部
校验和
时间 到
32 比特 源 IP 地址
类型 的
服务
层
32 比特 目的地 IP 地址
options (如果 任何)
IP 数据报 格式
IP 协议 version
编号
首部 长度
(字节)
upper 层 协议
到 交付 载荷 到
数据报
长度 (字节)
类型
的 数据
用于
最大值 编号
(decremented 在
每个 路由器)
record 路由
经过, specify
列表 的 路由器
到 visit.
如何 much overhead?
20 字节 的 TCP
20 字节 的 IP
= 40 字节 + 应用
层 overhead
比特 identifier” 是 NOT a 编号 用于 reordering
无 reordering 在 网络 层

4-44
IP fragmentation, reassembly
znetwork links have MTU (max.transfer
size) - largest possible link-level frame
different link types, different MTUs 
zlarge IP datagram divided 
(
䇾fragmented䇿) within net
one datagram becomes several 
datagrams
䇾reassembled䇿only at final 
destination
IP header bits used to identify, 
order related fragments
g
fragmentation:
in:one large datagram
out:3 smaller datagrams
reassembly
...
..............................
网络 链路 有 MTU (max.transfer
大小) - largest 可能 link-level 帧
不同的 链路 types, 不同的 MTUs
大的 IP 数据报 divided
) 在...内 网络
一个 数据报 becomes several
数据报
仅 在 最终
目的地
IP 首部 比特 用于 到 identify,
order 相关 fragments
在:一个 大的 数据报
查看:
3 smaller 数据报

...
..............................
4-45
ID
=x
offset
=0
fragflag
=0
length
=4000
ID
=x
offset
=0
fragflag
=1
length
=1500
ID
=x
offset
=185
fragflag
=1
length
=1500
ID
=x
offset
=370
fragflag
=0
length
=1040
one large datagram becomes
several smaller datagrams
example:
4000 byte datagram
Payload=3980
MTU = 1500 bytes
1480 bytes in 
data field
offset =
1480/8 
IP fragmentation, reassembly
(1500-20)+(1500-20)+(1040-20)=3980
长度
长度
长度
长度
一个 大的 数据报 becomes
several smaller 数据报
示例:
4000 字节 数据报
MTU = 1500 字节
1480 字节 在
数据 字段

4.1 Overview of Network layer
data plane
control plane
4.2 What
ÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-46
4.1 概述 的 网络层
数据平面
控制平面
4.2 什么
s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
网络 地址 翻译
4.4 Generalized 转发 和 SDN
匹配
动作
OpenFlow 示例 的 match-
plus-action 在 动作
第 4 章：提纲

4-47
IP addressing: introduction
zIP address:32-bit identifier for 
host, router interface
zinterface:connection between 
host/router and physical link
Router’s typically have multiple interfaces
host typically has one or two interfaces 
(e.g., wired Ethernet, wireless 802.11)
zIP addresses associated with each 
interface
223.1.1.1
223.1.1.2
223.1.1.3
223.1.1.4
223.1.2.9
223.1.2.2
223.1.2.1
223.1.3.2
223.1.3.1
223.1.3.27
223.1.1.1 = 11011111 00000001 00000001 00000001
223
111
1.2
222222222222222
2232.
223.1
IP 编址: 引言
IP 地址:
32-bit identifier 用于
主机, 路由器
接口
接口:
连接 之间
主机/路由器 和 物理 链路
路由器’s 通常 有 多路 接口
主机 通常 有 一个 或 两个 接口
(e.g., 有线 以太网, 无线 802.11)
IP 地址 associated 与 每个
接口

4-49
Subnets
zIP address:
subnet part - high order bits
host part - low order bits 
zwhat䇻s a subnet ?
device interfaces with same 
subnet part of IP address
can physically reach each other 
without intervening router
network consisting of 3 subnets
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
222
subnet
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1
IP 地址:
子网 部分 - high order 比特
主机 部分 - low order 比特
什么s a 子网 ?
设备 接口 与相同
子网 部分 的 IP 地址
可以 物理上 reach 每个 其他
没有 intervening 路由器
网络 consisting 的 3 subnets
子网

4-50
recipe
to determine the subnets, 
detach each interface from its 
host or router, creating islands 
of isolated networks
each isolated network is called 
a subnet
subnet mask: /24
Subnets
223.1.1.0/24
223.1.2.0/24
223.1.3.0/24
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
s
22
subnet
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1
到 确定 subnets,
detach 每个 接口 从 它的
主机 或 路由器
的 isolated 网络
每个 isolated 网络 是 called
子网
子网 mask: /24
子网

4-51
how many?
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.2
223.1.2.1
223.1.2.6
223.1.3.2
223.1.3.1
223.1.3.27
223.1.1.2
223.1.7.0
223.1.7.1
223.1.8.0223.1.8.1
223.1.9.1
223.1.9.2
Subnets
11
33
如何 许多?

52
যছ
६ӯ
ࡖܗՔלޣ
ࣩযছݤ
ॕ▁ЖՔࡖܗࣩ
যছՙ
ࣩࡖܗդ▁ЖՔޣ
যছՙ
ࠠЖযছЗ
ݤ޲Оלޣ
'

Ȉ
(

Ȉ
)

Ȉ    
IP Address Classes
IP 地址 Classes

53
⿱ᴹIPൠ൰ᱟа⇥؍⮉
ⲴIPൠ൰DŽਚᱟ֯⭘൘
ተฏ㖁ѝˈ൘
Internetк
ᱟн֯⭘ⲴDŽ⿱ᴹ
IPൠ
൰Ⲵ㤳തᴹ
:
㊫࡛
IP
㤳ത
IP
㤳ത㖁㔌ᮠ䟿
A10.x.x.x/810.0.0.0~10.255.255.2551
B172.16-32.x.x/16172.16.0.0~172.31.255.255 16
C192.168.x.x/24192.168.0.0~192.168.255.255 255
IP Address Classes
যছՙՙ޲О
֬֩ࡼ
࣐҄
ࣹࣩ֬֩
࣐҄
і਽ࣩڿ㓢
ՔјЈՔ
ޯ֧যছІࣩޯО޲ͣ੎ ৼ
*.)6
Ԣ੠ͤ
NUYZOJՔјЈՔ   ޯ֧যছІࣩߏՕО޲ NUYZOJ
ҵҵЈՔՔј
Ј֭֗ͣ՞ષ࣒ݏٹযছІૡਸ਼ޯ֧Ր
ૃՆͤ
TKZOJҵЈՔՔј   تTKZOJІࣩޥےО޲ૡਸ਼ݏٹ

ஂҵۋҵࣩ
ݤџѽ
ՔјՔј   ࣐и֩ޯ૆ѝࢺ࡚֚ੰ
IP 地址 Classes

4-54
IP addressing: CIDR
CIDR:Classless InterDomainRouting
subnet portion of address of arbitrary length
address format: a.b.c.d/x, where x is # bits in 
subnet portion of address
11001000  00010111  00010000  00000000
subnet
part
host
part
IP: 200.23.16.0/23
23bits
11111111  11111111  1111111000000000
᧙⸱
IP 编址: CIDR
子网 portion 的 地址 的 arbitrary 长度
地址 格式:, 哪里 x 是 # 比特 在
子网 portion 的 地址
子网
部分
主机
部分
23比特

4-56
IP addresses: how to get one?
Q:How does a hostget IP address?
zhard-coded by system admin in a 
file
Windows: control-panel->network->configuration->tcp/ip-
>properties
UNIX: /etc/rc.config
zDHCP: Dynamic Host Configuration Protocol: 
dynamically get address from as server
䇾plug-and-play䇿
IP 地址: 如何 到 获得 一个?
如何 做 a主机获得 IP 地址?
hard-coded 由 系统 admin 在 a
文件
dynamically 获得 地址 从 as 服务器

4-57
DHCP: Dynamic Host Configuration Protocol
goal:allow host to dynamicallyobtain its IP address from network server when 
it joins network
can renew its lease on address in use
allows reuse of addresses (only hold address while connected/䇾on䇿)
support for mobile users who want to join network (more shortly)
DHCP overview:
host broadcasts 䇾DHCP discover䇿msg [optional]
DHCP server responds with 䇾DHCP offer䇿msg [optional]
host requests IP address: 䇾DHCP request䇿msg
DHCP server sends address: 䇾DHCP ack䇿msg
DHCP: 动态 主机 Configuration 协议
目标:
allow 主机 到obtain 它的 IP 地址 从 网络 服务器 当
它 joins 网络
可以 renew 它的 lease 在 地址 在 使用
允许 reuse 的 地址 (仅hold 地址 while connected/在
支持 用于 移动用户s who want 到 join 网络 (更多 shortly)
DHCP 概述:
主机 broadcasts
DHCP 服务器 responds 与
主机 请求 IP 地址:DHCP 请求
DHCP 服务器 发送 地址:

58
DHCP client-server scenario
223.1.1.0/24
223.1.2.0/24
223.1.3.0/24
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
2222
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1
DHCP
server
arriving DHCP
clientneeds 
address in this
network
DHCP 客户-服务器 场景
服务器
到达的
客户需要
地址 在 这个
网络

4-59
DHCP server: 223.1.2.5
arriving
client
DHCP discover
src : 0.0.0.0, 68     
dest.: 255.255.255.255,67
yiaddr:    0.0.0.0
transaction ID: 654
DHCP offer
src: 223.1.2.5, 67      
dest:  255.255.255.255, 68
yiaddrr: 223.1.2.4
transaction ID: 654
lifetime: 3600 secs
DHCP request
src:  0.0.0.0, 68     
dest::  255.255.255.255, 67
yiaddrr: 223.1.2.4
transaction ID: 655
lifetime: 3600 secs
DHCP ACK
src: 223.1.2.5, 67      
dest:  255.255.255.255, 68
yiaddrr: 223.1.2.4
transaction ID: 655
lifetime: 3600 secs
DHCP client-server scenario
Broadcast: is there a 
DHCP server out there?
Broadcast: I’m a DHCP 
server! Here’s an IP 
address you can use 
Broadcast: OK.  I’ll take 
that IP address!
Broadcast: OK.  You’ve 
got that IP address!
yiaddr: 
your Internet address
DHCP 服务器: 223.1.2.5
到达的
客户
DHCP 请求
DHCP 客户-服务器 场景
广播: 是 存在 a
DHCP 服务器 查看 存在?
广播: I’m a DHCP
服务器! Here’s IP
地址 你 可以 使用
广播: OK.
这 IP 地址!
广播: OK.你’ve
got 这 IP 地址!
your Internet 地址

4-60
DHCP: more than IP addresses
DHCP can return more than just allocated IP address on 
subnet:
address of first-hop router for client
name and IP address of DNS sever
network mask (indicating network versus host portion of address)
DHCP: 更多 比 IP 地址
DHCP 可以 返回 更多 比 仅 allocated IP 地址 在
子网:
地址 的first-hop 路由器用于 客户
名称 和 IP 地址 的
网络 mask(indicating 网络 与 主机 portion 的 地址)

4-61
connecting laptop needs its IP 
address, addr of first-hop router, 
addr of DNS server: use DHCP
router with DHCP 
server built into 
router
DHCP request encapsulated in UDP, 
encapsulated in IP, encapsulated in 802.1 
Ethernet
Ethernet frame broadcast (dest: FFFFFFFFFFFF) 
on LAN, received at router running DHCP 
server
Ethernet demuxed to IP demuxed, UDP 
demuxed to DHCP 
168.1.1.1
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCPDHCP
DHCP
DHC
DHC
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCPDHC
DHC
DHC
DHCP
DHCP
DHCP: example
connecting laptop 需要 它的 IP
地址, addr 的 first-hop 路由器,
addr 的 DNS 服务器: 使用 DHCP
路由器 与 DHCP
服务器 built 进入
路由器
DHCP 请求 encapsulated 在
encapsulated 在 IP, encapsulated 在 802.1
以太网
以太网 帧 广播 (dest:
在 局域网, 已收到 在 路由器 运行 DHCP
服务器
以太网 demuxed 到 IP demuxed, UDP
demuxed 到 DHCP
DHCP: 示例

4-62
zDCP server formulates DHCP ACK 
containing client䇻s IP address, IP 
address of first-hop router for client, 
name & IP address of DNS server
encapsulation of DHCP server, frame 
forwarded to client, demuxing
up to DHCP 
at client
DHCP: example
router with DHCP 
server built into 
router
DHCP
DHCP
DHCP
DHC
DHC
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCPDHC
DHC
DHC
DHCP
client now knows its IP address, name and 
IP address of DSN server, IP address of its 
first-hop router
DCP 服务器 formulates DHCP ACK
包含客户s IP 地址
地址 的
first-hop 路由器
用于 客户,
名称 & IP 地址 的DNS 服务器
encapsulation 的 DHCP 服务器, 帧
转发 到 客户, demuxing
上 到 DHCP
在 客户
DHCP: 示例
路由器 与 DHCP
服务器 built 进入
路由器
客户 现在 知道 它的 IP 地址, 名称 和
IP 地址 的 DSN 服务器, IP 地址 的 它的
first-hop 路由器

4-63
DHCP: Wireshark output (home LAN)
Message type: Boot Reply (2)
Hardware type: Ethernet
Hardware address length: 6
Hops: 0
Transaction ID: 0x6b3a11b7
Seconds elapsed: 0
Bootp flags: 0x0000 (Unicast)
Client IP address: 192.168.1.101 (192.168.1.101)
Your (client) IP address: 0.0.0.0 (0.0.0.0)
Next server IP address: 192.168.1.1 (192.168.1.1)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Server host name not given
Boot file name not given
Magic cookie: (OK)
Option: (t=53,l=1) DHCP Message Type = DHCP ACK
Option: (t=54,l=4) Server Identifier = 192.168.1.1
Option: (t=1,l=4) Subnet Mask = 255.255.255.0
Option: (t=3,l=4) Router = 192.168.1.1
Option: (6) Domain Name Server
Length: 12; Value: 445747E2445749F244574092; 
IP Address: 68.87.71.226;
IP Address: 68.87.73.242; 
IP Address: 68.87.64.146
Option: (t=15,l=20) Domain Name = "hsd1.ma.comcast.net."
reply
Message type: Boot Request (1)
Hardware type: Ethernet
Hardware address length: 6
Hops: 0
Transaction ID: 0x6b3a11b7
Seconds elapsed: 0
Bootp flags: 0x0000 (Unicast)
Client IP address: 0.0.0.0 (0.0.0.0)
Your (client) IP address: 0.0.0.0 (0.0.0.0)
Next server IP address: 0.0.0.0 (0.0.0.0)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Server host name not given
Boot file name not given
Magic cookie: (OK)
Option: (t=53,l=1) DHCP Message Type = DHCP Request
Option: (61) Client identifier
Length: 7; Value: 010016D323688A; 
Hardware type: Ethernet
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Option: (t=50,l=4) Requested IP Address = 192.168.1.101
Option: (t=12,l=5) Host Name = "nomad"
Option: (55) Parameter Request List
Length: 11; Value: 010F03062C2E2F1F21F92B
1 = Subnet Mask; 15 = Domain Name
3 = Router; 6 = Domain Name Server
44 = NetBIOS over TCP/IP Name Server
......
request
DHCP: Wireshark 输出 (归属 局域网)
报文 类型:Boot 应答 (2)
Hardware 类型: 以太网
Hardware 地址 长度: 6
客户 IP 地址: 192.168.1.101 (192.168.1.101)
Your (客户) IP 地址: 0.0.0.0 (0.0.0.0)
下一个 服务器 IP 地址: 192.168.1.1 (192.168.1.1)
Relay 代理 IP 地址: 0.0.0.0 (0.0.0.0)
客户 MAC 地址: Wistron_23
服务器 主机 名称 不 给定的
Boot 文件 名称 不 给定的
选项: (t=53,l=1) DHCP 报文 类型 = DHCP ACK
选项: (t=54,l=4) 服务器 Identifier = 192.168.1.1
选项: (t=1,l=4) 子网 Mask = 255.255.255.0
选项: (t=3,l=4) 路由器 = 192.168.1.1
选项: (6) 域 名称 服务器
长度: 12; 值: 445747E2445749F244574092;
IP 地址: 68.87.71.226;
IP 地址: 68.87.73.242;
IP 地址: 68.87.64.146
选项: (t=15,l=20) 域 名称 = "hsd1.ma.comcast.net."
应答
报文 类型:Boot 请求 (1)
Hardware 类型: 以太网
Hardware 地址 长度: 6
客户 IP 地址: 0.0.0.0 (0.0.0.0)
Your (客户) IP 地址: 0.0.0.0 (0.0.0.0)
下一个 服务器 IP 地址: 0.0.0.0 (0.0.0.0)
Relay 代理 IP 地址: 0.0.0.0 (0.0.0.0)
客户 MAC 地址: Wistron_23:68:8a (00:16:d3:23:68:8a)
服务器 主机 名称 不 给定的
Boot 文件 名称 不 给定的
选项: (t=53,l=1)DHCP 报文 类型 = DHCP 请求
选项: (61) 客户 identifier
长度: 7; 值: 010016D323688;
Hardware 类型: 以太网
客户 MAC 地址: Wistron_
选项: (t=50,l=4) Requested IP 地址 = 192.168.1.101
选项: (t=12,l=5) 主机 名称 = "nomad"
选项: (55) Parameter 请求 列表
长度: 11; 值: 010F03062C2E2F1F21F92B
1 = 子网 Mask; 15 = 域 名称
3 = 路由器; 6 = 域 名称 服务器
44 = NetBIOS 在 TCP/IP 名称 服务器
请求

4-64
IP addresses: how to get one?
Q:how does networkget subnet part of IP addr?
A:gets allocated portion of its provider ISP䇻s address space
ISP's block          11001000  00010111  00010000  00000000    200.23.16.0/20
Organization 0    11001000  00010111  00010000  00000000    200.23.16.0/23
Organization 1    11001000  00010111  0001001
0  00000000    200.23.18.0/23
Organization 2    11001000  00010111  0001010
0  00000000    200.23.20.0/23
...                                          .....                                   ....                ....
Organization 7    11001000  00010111  0001111
0  00000000    200.23.30.0/23
IP 地址: 如何 到 获得 一个?
如何 做网络获得 子网 部分 的 IP addr?
获得 allocated portion 的 它的 提供商s 地址 空间
组织 0
组织 1
组织 2
组织 7

4-65
Hierarchical addressing: route aggregation
ĀSend me anything
with addresses 
beginning 
200.23.16.0/20ā
200.23.16.0/23
200.23.18.0/23
200.23.30.0/23
Fly-By-Night-ISP
Organization 0
Organization 7
Internet
Organization 1
ISPs-R-Us
ĀSend me anything
with addresses 
beginning 
199.31.0.0/16ā
200.23.20.0/23
Organization 2
.
.
.
.
.
.
hierarchical addressing allows efficientadvertisement of routing information:
ggg
200.23.16.0/20
199.31.0.0/16
2023.10.18
Hierarchical 编址: 路由 aggregation
发送 me anything
与 地址
组织 0
组织 7
组织 1
发送 me anything
与 地址
组织 2
hierarchical 编址 允许高效的advertisement 的 路由 信息:

4-66
ISPs-R-Us has a more specific route to Organization 1
ĀSend me anything
with addresses beginning 
200.23.16.0/20ā
200.23.16.0/23
200.23.18.0/23
200.23.30.0/23
Fly-By-Night-ISP
Organization 0
Organization 7
Internet
Organization 1
ISPs-R-Us
ĀSend me anything
with addresses beginning 
199.31.0.0/16 or 200.23.18.0/23
ā
200.23.20.0/23
Organization 2
.
.
.
.
.
.
Hierarchical addressing: more specific routes
200.23.16.0/20
199.31.0.0/16
200.23.18.0/23
Organization 1
ISPs-R-Us 有 a 更多 特定的 路由 到 组织 1
发送 me anything
与 地址 beginning
组织 0
组织 7
组织 1
发送 me anything
与 地址 beginning
或
组织 2
Hierarchical 编址: 更多 特定的 路由
组织 1

4-67
IP addressing: the last word...
Q:how does an ISP get block of addresses?
A:ICANN: Internet Corporation for Assigned 
Names and Numbers http://www.icann.org/
allocates addresses
manages DNS
assigns domain names, resolves disputes
IP 编址: 最后 word...
如何 做 ISP 获得 block 的 地址?
orporation 用于
ames 和
allocates 地址
assigns 域 名称, resolves disputes

4-68
NAT: network address translation
10.0.0.1
10.0.0.2
10.0.0.3
10.0.0.4
138.76.29.7
local network
(e.g., home network)
10.0.0/24
rest of
Internet
datagrams with source or 
destinationin this network
have 10.0.0/24 address for 
source, destination (as usual)
alldatagramsleavinglocal
network have samesingle source 
NAT IP address: 138.76.29.7,different 
source port numbers
1
NAT: 网络 地址 翻译
本地 网络
(e.g., 归属网络)
rest 的
数据报 与源 或
目的地在 这个 网络
有 10.0.0/24 地址 用于
源, 目的地 (as usual)
所有
数据报本地
网络 有相同单个 源
NAT IP 地址: 138.76.29.7,不同的
源 端口 编号

4-69
motivation:local network uses just oneIP address as far as 
outside world is concerned:
range of addresses not needed from ISP:  just one IP address 
for all devices
can change addresses of devices in local network without 
notifying outside world
can change ISP without changing addresses of devices in local 
network
devices inside local net not explicitly addressable, visible by 
outside world (a security plus)
NAT: network address translation
本地 网络 使用 仅一个IP 地址 as far as
outside world 是 concerned:
range 的 地址 不 needed 从 ISP: 仅一个 IP 地址
用于 所有 设备
可以 变化 地址 的 设备 在 本地 网络 没有
可以 变化 ISP 没有 changing 地址 的 设备 在 本地
网络
设备 inside 本地 网络 不 explicitly addressable, visible 由
outside world (a 安全 加)
NAT: 网络 地址 翻译

4-70
10.0.0.1
10.0.0.2
10.0.0.3
S: 10.0.0.1, 3345
D: 128.119.40.186, 80
1
10.0.0.4
138.76.29.7
1:host 10.0.0.1 
sends datagram to 
128.119.40.186, 80
NAT translation table
WAN side addrLAN side addr
138.76.29.7, 500110.0.0.1, 3345
......                                         ......
S: 128.119.40.186, 80 
D: 10.0.0.1, 3345
4
S: 138.76.29.7, 5001
D: 128.119.40.186, 80
2
2:NAT router
changes datagram
source addr from
10.0.0.1, 3345 to
138.76.29.7, 5001,
updates table
S: 128.119.40.186, 80 
D: 138.76.29.7, 5001
3
3:reply arrives
dest. address:
138.76.29.7, 5001
4:NAT router
changes datagram
dest addr from
138.76.29.7, 5001 to 10.0.0.1, 3345 
NAT: network address translation
1
㊫
࡛
IP
㤳ത
IP
㤳ത㖁㔌ᮠ䟿
A10.x.x.x/810.0.0.0~10.255.255.2551
B172.16-32.x.x/16172.16.0.0~172.31.255.255 16
C192.168.x.x/24192.168.0.0~
192.168.255.255 255
主机 10.0.0.1
发送 数据报 到
NAT 翻译 表
WAN 一侧 addr
局域网 一侧 addr
NAT 路由器
变化 数据报
源 addr 从
10.0.0.1, 3345 到
updates 表
应答 到达
dest. 地址:
NAT 路由器
变化 数据报
dest addr 从
138.76.29.7, 5001 到 10.0.0.1,
NAT: 网络 地址 翻译

4-71
implementation:NAT router must:
outgoing datagrams: replace(source IP address, port #) of every 
outgoing datagram to (NAT IP address, new port #)
. . . remote clients/servers will respond using (NAT IP address, new port #) 
as destination addr
remember (in NAT translation table)every (source IP address, port #)  
to (NAT IP address, new port #) translation pair
incoming datagrams: replace(NAT IP address, new port #) in dest
fields of every incoming datagram with corresponding (source IP 
address, port #) stored in NAT table
NAT: network address translation
实现NAT 路由器 必须:
outgoing 数据报: replace(源 IP 地址, 端口 #) 的 每一个
outgoing 数据报 到 (NAT IP 地址, 新的 端口 #)
. . . 远程 clients/servers 将 respond 使用 (NAT IP 地址, 新的 端口 #)
as 目的地 addr
remember (在 NAT 翻译 表)每一个 (源 IP 地址, 端口 #)
到 (NAT IP 地址, 新的 端口 #) 翻译 pair
传入的 数据报: replace(NAT IP 地址, 新的 端口 #) 在 dest
字段 的 每一个 传入的 数据报 与 corresponding (源 IP
地址, 端口 #) 存储的 在 NAT 表
NAT: 网络 地址 翻译

4-72
z16-bit port-number field: 
60,000 simultaneous connections with a singleLAN-side 
address!
zNAT is controversial:
routers should only process up to layer 3
violates end-to-end argument
NAT possibility must be taken into account by app designers, e.g., P2P 
applications
address shortage should instead be solved by IPv6
NAT: network address translation
16-bit port-number 字段:
连接 与 a单个
局域网-side
地址!
NAT 是 controversial:
路由器 should 仅 进程 上 到层 3
NAT possibility 必须 是 经过 进入 account 由 应用 designers, e.g., P2P
应用
地址 shortage should instead 是 solved 由 IPv6
NAT: 网络 地址 翻译

4-73
NAT traversal problem
zclient wants to connect to server with 
address 10.0.0.1
server address 10.0.0.1 local to LAN (client 
can
䇻t use it as destination addr)
only one externally visible NATed address: 
138.76.29.7
zsolution1:statically configure NAT to 
forward incoming connection requests 
at given port to server
e.g., (123.76.29.7, port 2500) always 
forwarded to 10.0.0.1 port 25000
10.0.0.1
10.0.0.4
NAT 
router
138.76.29.7
client
?
NAT traversal 问题
客户 wants 到 连接 到 服务器 与
地址 10.0.0.1
服务器 地址 10.0.0.1 本地 到 局域网 (客户
可以
t 使用 它 as 目的地 addr)
仅 一个 externally visible NATed 地址:
NAT 到
转发 传入的 连接 请求
在 给定的 端口 到 服务器
e.g., (123.76.29.7, 端口 2500) 总是
转发 到 10.0.0.1 端口 25000
路由器
客户

4-74
NAT traversal problem
zsolution 2:Universal Plug and Play (UPnP) 
Internet Gateway Device (IGD) Protocol.  
Allows NATed host to:
learn public IP address (138.76.29.7)
add/remove port mappings (with lease 
times)
i.e., automate static NAT port map 
configuration
10.0.0.1
NAT 
router
Run IGD 
protocol
NAT traversal 问题
Universal Plug 和 Play (UPnP)
Internet 网关 设备 (IGD) Protocol.
允许 NATed 主机 到:
learn 公共 IP 地址 (138.76.29.7)
add/remove 端口 mappings (与 lease
i.e., automate 静态 NAT 端口 map
路由器
运行 IGD
协议

4-75
NAT traversal problem
zsolution 3:relaying(used in Skype)
NATed client establishes connection to relay
external client connects to relay
relay bridges packets between to connections
138.76.29.7
client
1.connection to
relay initiated
by NATed host
2.connection to
relay initiated
by client
3.relaying 
established
p
NAT 
router
10.0.0.1
NAT traversal 问题
(用于 在 Skype)
NATed 客户 establishes 连接 到 relay
external 客户 连接 到 relay
relay bridges 分组 之间 到 连接
客户
连接 到
由 NATed 主机
连接 到
由 客户
路由器

How to make a hole
zThe firewall refuses incoming TCP conn.
zthe firewall refuses the first incoming UDP.
zIf A and B locate behind their NAT, respectively, how to 
communicate directly via a hole?
76
peer
192.168.1.10
10.0.0.10
peer
How to let them 
communicate 
directly?
NAT
NAT
peerpeer
NAT
如何 到 生成 a hole
传入的 TCP conn
第一个 传入的 UDP
如果 和 B locate 背后的 它们的 NAT, respectively, 如何 到
通信 directly 通过 a hole?
对等方
对等方
如何 到 let them
通信
对等方对等方

ᢃ⍎ᯩ⌅:
zFull Cone(ޘ䭕ර)
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞ
ᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort), ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ
䜭ሶ֯⭘䘉ሩ
(PublicIP:PublicPort)DŽ↔ཆˈԫօཆ䜘ѫᵪਚ㾱⸕䚃䘉њ(PublicIP:PublicPort)
ቡਟԕਁ䘱ᮠᦞ㔉(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव.
4-77
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
ԫ᜿⸕䚃
䈕ൠ൰ㄟ
ਓⲴཆ䜘
ѫᵪ˄н
䲀ཆ䜘IP
઼ㄟਓ˅
完整的 Cone(
UDP 套接字(LocalIP:LocalPort)套接字
套接字
UDP 套接字

zRestricted Cone
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort), ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ䜭ሶ
֯⭘䘉ሩ(PublicIP:PublicPort)DŽ↔ཆˈྲ᷌ԫօཆ䜘ѫᵪᜣ㾱ਁ䘱ᮠᦞ㔉䘉њ޵㖁ѫᵪˈਚ㾱⸕
䚃䘉њ(PublicIP:PublicPort)ᒦф޵㖁ѫᵪѻࡽᴮ⭘䘉њsocketੁ䘉њཆ䜘ѫᵪIPਁ䘱䗷ᮠᦞ
DŽਚ㾱┑䏣䘉єњᶑԦˈ䘉њཆ䜘ѫᵪቡਟԕ⭘㠚ᐡⲴ
(IP, ԫօㄟਓ)ਁ䘱ᮠᦞ㔉
(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव
4-78
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
ᴮ㓿⭘䈕ൠ
൰ㄟਓ䙊ؑ
䗷Ⲵཆ䜘ѫ
ᵪ˄䲀IPˈ
н䲀ㄟਓ˅
UDP 套接字(LocalIP:LocalPort)套接字
套接字
套接字
UDP 套接字

zPort Restricted Cone
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort),ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ䜭ሶ֯
⭘䘉ሩ(PublicIP:PublicPort)DŽ↔ཆˈྲ᷌ԫօཆ䜘ѫᵪᜣ㾱ਁ䘱ᮠᦞ㔉䘉њ޵㖁ѫᵪˈਚ㾱⸕䚃
䘉њ(PublicIP:PublicPort)ᒦф޵㖁ѫᵪѻࡽᴮ⭘䘉њsocketੁ䘉њཆ䜘ѫᵪ(IP, Port)ਁ
䘱䗷ᮠᦞDŽਚ㾱┑䏣䘉єњᶑԦˈ䘉њཆ䜘ѫᵪቡਟԕ⭘㠚ᐡⲴ
(IP,Port)ਁ䘱ᮠᦞ㔉
(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव.
4-79
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
(IP, Port)
ᴮ㓿⭘䈕ൠ൰
ㄟਓ䙊ؑ䗷Ⲵ
ཆ䜘ѫᵪ˄䲀
IP઼ㄟਓ˅
端口 Restricted Cone
UDP 套接字(LocalIP:LocalPort)套接字
套接字
套接字
(IP, 端口)
(IP,端口)
UDP 套接字
(IP, 端口)

zSymmetric˄ሩ〠ᖒ˅
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP, LocalPort),ᖃ⭘䘉њsocketᅮа⅑ਁᮠᦞ㔉ཆ䜘ѫᵪ1ᰦ, 
NATѪަ᱐ሴањ(PublicIP1,Port1),ԕਾ޵㖁ѫᵪਁ䘱㔉ཆ䜘ѫᵪ1Ⲵᡰᴹᮠᦞ䜭ᱟ⭘䘉њ
(PublicIP1,Port1)˗ྲ᷌޵㖁ѫᵪ਼ᰦ⭘䘉њsocket㔉ཆ䜘ѫᵪ2ਁ䘱ᮠᦞˈᅮа⅑ਁ䘱ᰦˈ
NATՊѪަ࠶䝽ањ(PublicIP2,Port2), ԕਾ޵㖁ѫᵪਁ䘱㔉ཆ䜘ѫᵪ2Ⲵᡰᴹᮠᦞ䜭ᱟ⭘䘉њ
(PublicIP2,Port2).
4-80
UDP socket
(LocalIP:LocalPort) 
(IP, Port)
(PublicIP2: PublicPort2)
(IP, Port)
(PublicIP,Port)
оཆ䜘ѫᵪ᥶
㔁DŽࡽ䶒ࠐ⿽
ᯩᔿ⋑ᴹ᥶㔁
UDP 套接字(LocalIP, LocalPort),
套接字
(PublicIP1,端口1),
(PublicIP1,端口1)
套接字
(PublicIP2,端口2),
(PublicIP2,端口2).
UDP 套接字
(IP, 端口)
(IP, 端口)
(PublicIP,端口)

4-81
4.1 introduction
4.2 virtual circuit and datagram 
networks
4.3 what
䇻s inside a router
4.4 IP: Internet Protocol
datagram format
IPv4 addressing
IPv6
4.5 routing algorithms
link state
distance vector
hierarchical routing
4.6 routing in the Internet
RIP
OSPF
BGP
4.7 broadcast and multicast routing
Chapter 4: outline
4.1 引言
4.2 virtual 电路 和 数据报
网络
4.3 什么
s inside a 路由器
4.4 IP: Internet 协议
数据报 格式
IPv4 编址
4.5 路由 算法
链路状态
距离向量
hierarchical 路由
4.6 路由 在 Internet
4.7 广播 和 multicast 路由
第 4 章：提纲

4-82
IPv6: motivation
zinitial motivation:32-bit address space soon to 
be completely allocated.  
zadditional motivation:
header format helps speed processing/forwarding
header changes to facilitate QoS
IPv6 datagram format: 
fixed-length 40 byte header
no fragmentation allowed
初始 motivation:32-bit 地址 空间 soon 到
是 completely allocated.
首部 格式 helps speed processing/转发
首部 变化 到 facilitate QoS
IPv6 数据报 格式:
fixed-length 40 字节 首部
无 fragmentation allowed

4-83
IPv6 datagram format
priority:identify priority among datagrams in flow
flow Label:identify datagrams in same Āflow.ā
(concept ofĀflowānot well defined).
next header:identify upper layer protocol for data
data
destination address
(128 bits)
source address
(128 bits)
payload len
next hdr
hop limit
flow label
pri
ver
32 bits
IPv6 数据报 格式
优先级:identify 优先级 之间 数据报 在 流
流 标签:identify 数据报 在 相同
(concept 的流
不 well defined).
下一个 首部:
identify upper 层 协议 用于 数据
数据
目的地 地址
(128 比特)
源 地址
(128 比特)
载荷 len
下一个 hdr
跳 limit
流 标签
32 比特

4-84
Other changes from IPv4
zchecksum:removed entirely to reduce processing time at 
each hop
zoptions:allowed, but outside of header, indicated by 䇾Next 
Header䇿field
zICMPv6:new version of ICMP
additional message types, e.g. 䇾Packet Too Big䇿
multicast group management functions
其他 变化 从 IPv4
校验和removed entirely 到 reduce processing 时间 在
每个 跳
allowed, 但是 outside 的 首部, indicated 由
下一个
首部字段
新的 version 的 ICMP
additional 报文 types, e.g.分组 Too Big
multicast group 管理 功能

4-85
Transition from IPv4 to IPv6
znot all routers can be upgraded simultaneously
no 䇾flag days䇿
how will network operate with mixed IPv4 and 
IPv6 routers? 
ztunneling:IPv6 datagram carried as payloadin 
IPv4 datagram among IPv4 routers
IPv4 source, 
dest addr
IPv4 header fields 
IPv4 datagram
IPv6 datagram
IPv4 payload 
UDP/TCP payload
IPv6 source dest addr
IPv6 header fields
IPv6
IPv6
IPv4
Transition 从 IPv4 到 IPv6
不 所有 路由器 可以 是 upgraded simultaneously
无
如何 将 网络 operate 与 mixed IPv4 和
IPv6 路由器?
IPv6 数据报 承载的 as载荷在
IPv4 数据报 之间 IPv4 路由器
IPv4 源,
IPv4 首部 字段
IPv4 数据报
IPv6 数据报
IPv4 载荷
UDP/TCP 载荷
IPv6 源 dest addr
IPv6 首部 字段

4-87
flow: X
src: A
dest: F
data
A-to-B:
IPv6
Flow: X
Src: A
Dest: F
data
src:B
dest: E
B-to-C:
IPv6 inside
IPv4
E-to-F:
IPv6
flow: X
src: A
dest: F
data
B-to-C:
IPv6 inside
IPv4
Flow: X
Src: A
Dest: F
data
src:B
dest: E
physical view:
A
B
IPv6
IPv6
E
IPv6
IPv6
F
C
D
logical view:
IPv4 tunnel 
connecting IPv6 routers
E
IPv6
IPv6
F
A
B
IPv6
IPv6
Tunneling
IPv4
IPv4
Double protocol stack
流: X
数据
流: X
数据
流: X
数据
流: X
数据
物理 view:
connecting IPv6 路由器
Double 协议 stack

4.1 Overview of Network layer
data plane
control plane
4.2 What
ÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-89
4.1 概述 的 网络层
数据平面
控制平面
4.2 什么
s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
网络 地址 翻译
4.4 Generalized 转发 和 SDN
匹配
动作
OpenFlow 示例 的 match-
plus-action 在 动作
第 4 章：提纲

Generalized Forwarding and SDN
2
3
0100 1101
values in arriving
packet䇻s header
logically-centralized routing controller
1
control plane
data plane
Each router contains a flow table that is computed and 
distributed by a logically centralized routing controller
local flow table
headers  counters  actions
4-90
Generalized 转发 和 SDN
值 在 到达的
分组s 首部
logically-centralized 路由 控制器
控制平面
数据平面
每个 路由器 包含 a流 表
这 是 计算得到的 和
分布式 由 a路由 控制器
本地 流 表
首部 counters 动作

zĨůŽǁ͗ĚĞĨŝŶĞĚďLJŚĞĂĚĞƌĨŝĞůĚǀĂůƵĞƐ;ŝŶůŝŶŬͲ͕ŶĞƚǁŽƌŬͲ͕ƚƌĂŶƐƉŽƌƚͲůĂLJĞƌĨŝĞůĚƐͿ
zŐĞŶĞƌĂůŝnjĞĚĨŽƌǁĂƌĚŝŶŐ͗ƐŝŵƉůĞƉĂĐŬĞƚͲŚĂŶĚůŝŶŐƌƵůĞƐ
ŵĂƚĐŚ͗ƉĂƚƚĞƌŶǀĂůƵĞƐŝŶƉĂĐŬĞƚŚĞĂĚĞƌĨŝĞůĚƐ
ĂĐƚŝŽŶƐ͗ĨŽƌŵĂƚĐŚĞĚƉĂĐŬĞƚ͗ĚƌŽƉ͕ĨŽƌǁĂƌĚ͕ŵŽĚŝĨLJ͕ŵĂƚĐŚĞĚƉĂĐŬĞƚŽƌƐĞŶĚ
ŵĂƚĐŚĞĚƉĂĐŬĞƚƚŽĐŽŶƚƌŽůůĞƌ
ƉƌŝŽƌŝƚLJ͗ĚŝƐĂŵďŝŐƵĂƚĞŽǀĞƌůĂƉƉŝŶŐƉĂƚƚĞƌŶƐ
ĐŽƵŶƚĞƌƐ͗ηďLJƚĞƐĂŶĚηƉĂĐŬĞƚƐ
Flow table abstraction
ZŽƵƚĞƌ͛ƐĨůŽǁƚĂďůĞĚĞĨŝŶĞ
ƌŽƵƚĞƌ͛ƐŵĂƚĐŚнĂĐƚŝŽŶƌƵůĞƐ
&ůŽǁƚĂďůĞ
ŵĂƚĐŚ
ĂĐƚŝŽŶ
Network Layer: 4-91
流 表 抽象
网络层: 4-91

zĨůŽǁ͗ĚĞĨŝŶĞĚďLJŚĞĂĚĞƌĨŝĞůĚƐ
zŐĞŶĞƌĂůŝnjĞĚĨŽƌǁĂƌĚŝŶŐ͗ƐŝŵƉůĞƉĂĐŬĞƚͲŚĂŶĚůŝŶŐƌƵůĞƐ
ŵĂƚĐŚ͗ƉĂƚƚĞƌŶǀĂůƵĞƐŝŶƉĂĐŬĞƚŚĞĂĚĞƌĨŝĞůĚƐ
ĂĐƚŝŽŶƐ͗ĨŽƌŵĂƚĐŚĞĚƉĂĐŬĞƚ͗ĚƌŽƉ͕ĨŽƌǁĂƌĚ͕ŵŽĚŝĨLJ͕ŵĂƚĐŚĞĚƉĂĐŬĞƚŽƌ
ƐĞŶĚŵĂƚĐŚĞĚƉĂĐŬĞƚƚŽĐŽŶƚƌŽůůĞƌ
ƉƌŝŽƌŝƚLJ͗ĚŝƐĂŵďŝŐƵĂƚĞŽǀĞƌůĂƉƉŝŶŐƉĂƚƚĞƌŶƐ
ĐŽƵŶƚĞƌƐ͗ηďLJƚĞƐĂŶĚηƉĂĐŬĞƚƐ
&ůŽǁƚĂďůĞ
ŵĂƚĐŚ
ĂĐƚŝŽŶ
ϭ
Ϯ
ϯ
κ
* : wildcard
ƐƌĐсϭϬ͘ϭ͘Ϯ͘ϯ͕ĚĞƐƚсΎ͘Ύ͘Ύ͘ΎƐĞŶĚƚŽĐŽŶƚƌŽůůĞƌ
ƐƌĐсϭ͘Ϯ͘Ύ͘Ύ͕ĚĞƐƚсΎ͘Ύ͘Ύ͘Ύ ĚƌŽƉ
ƐƌĐсΎ͘Ύ͘Ύ͘Ύ͕ĚĞƐƚсϯ͘κ͘Ύ͘ΎĨŽƌǁĂƌĚ;ϮͿ
Flow table abstraction
Network Layer: 4-92
流 表 抽象
网络层: 4-92

OpenFlow: Flow Table Entries
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ZƵůĞĐƚŝŽŶ^ƚĂƚƐ
ϭ͘ &ŽƌǁĂƌĚƉĂĐŬĞƚƚŽƉŽƌƚ;ƐͿ
Ϯ͘ ŶĐĂƉƐƵůĂƚĞĂŶĚĨŽƌǁĂƌĚƚŽĐŽŶƚƌŽůůĞƌ
ϯ͘ ƌŽƉƉĂĐŬĞƚ
κ͘ ^ĞŶĚƚŽŶŽƌŵĂůƉƌŽĐĞƐƐŝŶŐƉŝƉĞůŝŶĞ
ρ͘ DŽĚŝĨLJ&ŝĞůĚƐ
WĂĐŬĞƚнďLJƚĞĐŽƵŶƚĞƌƐ
>ŝŶŬůĂLJĞƌ
EĞƚǁŽƌŬůĂLJĞƌ
dƌĂŶƐƉŽƌƚůĂLJĞƌ
OpenFlow: 流 表 Entries

Destination-based forwarding:
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ĐƚŝŽŶ
ΎΎ  Ύ Ύ Ύ
ρϭ͘ς͘Ϭ͘Θ
ΎΎΎ
ƉŽƌƚς
Examples
IP datagrams destined to IP address  51.6.0.8 should be 
forwarded to router output port 6 
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
&ŽƌǁĂƌĚ
ΎΎ  Ύ Ύ Ύ Ύ Ύ Ύ
ϮϮ
ĚƌŽƉ
Firewall:
do not forward (block) all datagrams destined to TCP  port 22
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
&ŽƌǁĂƌĚ
ΎΎ  Ύ Ύ
ϭϮΘ͘ϭϭε͘ϭ͘ϭ
ΎΎΎΎ
ĚƌŽƉ
do not forward (block) all datagrams sent by host 128.119.1.1
Destination-based 转发:
示例
IP 数据报 destined 到 IP 地址 51.6.0.8 should 是
转发 到 路由器 输出 端口
做 不 转发 (block) 所有 数据报 destined 到 TCP 端口 22
做 不 转发 (block) 所有 数据报 已发送 由 主机 128.119.1.1

Destination-based layer 2(switch) forwarding:
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ĐƚŝŽŶ
ΎΎΎΎΎΎΎΎ
ƉŽƌƚϯ
Examples
layer 2 frames from MAC address 22:A7:23:11:E1:02 should be 
forwarded to output port 3 
ϮϮ͗ϳ͗Ϯϯ͗
ϭϭ͗ϭ͗ϬϮ
4-95
Destination-based 层(交换机) 转发:
示例
层 2 帧 从 MAC 地址 22:7:23:11:E1:02 should 是
转发 到 输出 端口

KƉĞŶ&ůŽǁĂďƐƚƌĂĐƚŝŽŶ
ZŽƵƚĞƌ
•ŵĂƚĐŚ͗ůŽŶŐĞƐƚĚĞƐƚŝŶĂƚŝŽŶ/W
ƉƌĞĨŝdž
•ĂĐƚŝŽŶ͗ĨŽƌǁĂƌĚŽƵƚĂůŝŶŬ
^ǁŝƚĐŚ
•ŵĂƚĐŚ͗ĚĞƐƚŝŶĂƚŝŽŶD
ĂĚĚƌĞƐƐ
•ĂĐƚŝŽŶ͗ĨŽƌǁĂƌĚŽƌĨůŽŽĚ
&ŝƌĞǁĂůů
•ŵĂƚĐŚ͗/WĂĚĚƌĞƐƐĞƐĂŶĚdWͬhW
ƉŽƌƚŶƵŵďĞƌƐ
•ĂĐƚŝŽŶ͗ƉĞƌŵŝƚŽƌĚĞŶLJ
Ed
•ŵĂƚĐŚ͗/WĂĚĚƌĞƐƐĂŶĚƉŽƌƚ
•ĂĐƚŝŽŶ͗ƌĞǁƌŝƚĞĂĚĚƌĞƐƐĂŶĚƉŽƌƚ
ŵĂƚĐŚнĂĐƚŝŽŶ͗ƵŶŝĨŝĞƐĚŝĨĨĞƌĞŶƚŬŝŶĚƐŽĨĚĞǀŝĐĞƐ
4-96

IP Src = 10.3.0.5
IP Dst = 10.2.*.*
forward(3)
match
action
ingress port = 1
IP Dst = 10.2.0.4
ingress port = 2
IP Dst = 10.2.0.3
forward(4)
match
action
forward(3)
ingress port = 1
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(4)
match
action
KƉĞŶ&ůŽǁĞdžĂŵƉůĞ
Host h1
10.1.0.1
Host h2
10.1.0.2
Host h4
10.2.0.4
Host h3
10.2.0.3
Host h5
10.3.0.5
s1
s2
s3
1
2
3
4
1
2
3
4
1
2
3
4
Host h6
10.3.0.6
3
s2
1
33
44
s
controllercontrollertll
Example: datagrams from 
hosts h5 and h6 should 
be sent to h3 or h4, via s1 
and from there to s2
IP Src = 10.3.0.6
IP Dst = 10.2.*.*
forward(4)
2023.10.23
转发(
匹配
动作
ingress 端口 = 1
ingress 端口 = 2
转发(4)
匹配
动作
转发(3)
ingress 端口 = 1
转发(4)
匹配
动作
主机 h1
主机 h2
主机 h4
主机 h3
主机 h5
主机 h6
控制器
示例:数据报 从
主机 h5 和 h6 should
是 已发送 到 h3 或 h4, 通过 s1
和 从 存在 到 s2
转发(

OpenFlow example
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(3)
match
action
ingress port = 2
IP Dst = 10.2.0.3
ingress port = 2
IP Dst = 10.2.0.4
forward(3)
match
action
forward(4)
ingress port = 1
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(4)
match
action
Host h1
10.1.0.1
Host h2
10.1.0.2
Host h4
10.2.0.4
Host h3
10.2.0.3
Host h5
10.3.0.5
s1
s2
s3
1
2
3
4
1
2
3
4
1
2
3
4
Host h6
10.3.0.6
controllercontrollertll
s1sss
1
4
s2s
4
sss
1
4
KƌĐŚĞƐƚƌĂƚĞĚƚĂďůĞƐĐĂŶĐƌĞĂƚĞ
ŶĞƚǁŽƌŬͲǁŝĚĞďĞŚĂǀŝŽƌ͕Ğ͘Ő͕͗͘
ĚĂƚĂŐƌĂŵƐĨƌŽŵŚŽƐƚƐŚρĂŶĚ
ŚςƐŚŽƵůĚďĞƐĞŶƚƚŽŚϯŽƌŚκ͕
ǀŝĂƐϭĂŶĚĨƌŽŵƚŚĞƌĞƚŽƐϮ
Network Layer: 4-98
OpenFlow 示例
转发(3)
匹配
动作
ingress 端口 = 2
ingress 端口 = 2
转发(3)
匹配
动作
转发(4)
ingress 端口 = 1
转发(4)
匹配
动作
主机 h1
主机 h2
主机 h4
主机 h3
主机 h5
主机 h6
控制器
网络层: 4-98

Middleboxes
͞ĂŶLJŝŶƚĞƌŵĞĚŝĂƌLJďŽdžƉĞƌĨŽƌŵŝŶŐĨƵŶĐƚŝŽŶƐĂƉĂƌƚ
ĨƌŽŵŶŽƌŵĂů͕ƐƚĂŶĚĂƌĚĨƵŶĐƚŝŽŶƐŽĨĂŶ/WƌŽƵƚĞƌŽŶ
ƚŚĞĚĂƚĂƉĂƚŚďĞƚǁĞĞŶĂƐŽƵƌĐĞŚŽƐƚĂŶĚ
ĚĞƐƚŝŶĂƚŝŽŶŚŽƐƚ͟
DŝĚĚůĞďŽdž;Z&ϯϮϯκͿ

Middleboxes everywhere!
ĞŶƚĞƌƉƌŝƐĞ
ŶĞƚǁŽƌŬ
ŶĂƚŝŽŶĂůŽƌŐůŽďĂů/^W
ĚĂƚĂĐĞŶƚĞƌ
ŶĞƚǁŽƌŬ
Ed͗ŚŽŵĞ͕
ĐĞůůƵůĂƌ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů
&ŝƌĞǁĂůůƐ͕/^͗ĐŽƌƉŽƌĂƚĞ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů͕ƐĞƌǀŝĐĞƉƌŽǀŝĚĞƌƐ͕
/^WƐ
>ŽĂĚďĂůĂŶĐĞƌƐ͗
ĐŽƌƉŽƌĂƚĞ͕ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌ͕ĚĂƚĂĐĞŶƚĞƌ͕
ŵŽďŝůĞŶĞƚƐ
ĂĐŚĞƐ͗ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌ͕ŵŽďŝůĞ͕EƐ
ƉƉůŝĐĂƚŝŽŶͲ
ƐƉĞĐŝĨŝĐ͗ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌƐ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů͕
E

Middleboxes
ŝŶŝƚŝĂůůLJ͗ƉƌŽƉƌŝĞƚĂƌLJ;ĐůŽƐĞĚͿŚĂƌĚǁĂƌĞƐŽůƵƚŝŽŶƐ
ŵŽǀĞƚŽǁĂƌĚƐ͞ǁŚŝƚĞďŽdž͟ŚĂƌĚǁĂƌĞŝŵƉůĞŵĞŶƚŝŶŐŽƉĞŶW/
ŵŽǀĞĂǁĂLJĨƌŽŵƉƌŽƉƌŝĞƚĂƌLJŚĂƌĚǁĂƌĞƐŽůƵƚŝŽŶƐ
ƉƌŽŐƌĂŵŵĂďůĞůŽĐĂůĂĐƚŝŽŶƐǀŝĂŵĂƚĐŚнĂĐƚŝŽŶ
ŵŽǀĞƚŽǁĂƌĚƐŝŶŶŽǀĂƚŝŽŶͬĚŝĨĨĞƌĞŶƚŝĂƚŝŽŶŝŶƐŽĨƚǁĂƌĞ
^E͗;ůŽŐŝĐĂůůLJͿĐĞŶƚƌĂůŝnjĞĚĐŽŶƚƌŽůĂŶĚĐŽŶĨŝŐƵƌĂƚŝŽŶŵĂŶĂŐĞŵĞŶƚ
ŽĨƚĞŶŝŶƉƌŝǀĂƚĞͬƉƵďůŝĐĐůŽƵĚ
ŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶƐǀŝƌƚƵĂůŝnjĂƚŝŽŶ;E&sͿ͗ƉƌŽŐƌĂŵŵĂďůĞƐĞƌǀŝĐĞƐŽǀĞƌ
ǁŚŝƚĞďŽdžŶĞƚǁŽƌŬŝŶŐ͕ĐŽŵƉƵƚĂƚŝŽŶ͕ƐƚŽƌĂŐĞ

The IP hourglass
/W
dW
hW
,ddW
^DdW
Yh/
^,
ZdW
͙
ƚŚĞƌŶĞƚ
tŝ&ŝ ůƵĞƚŽŽƚŚ
WWW
WW
͙
ĐŽƉƉĞƌƌĂĚŝŽĨŝďĞƌ
/ŶƚĞƌŶĞƚ͛Ɛ͞ƚŚŝŶǁĂŝƐƚ͗͟
ŽŶĞŶĞƚǁŽƌŬůĂLJĞƌ
ƉƌŽƚŽĐŽů͗/W
ŵƵƐƚďĞŝŵƉůĞŵĞŶƚĞĚ
ďLJĞǀĞƌLJ;ďŝůůŝŽŶƐͿŽĨ
/ŶƚĞƌŶĞƚͲĐŽŶŶĞĐƚĞĚ
ĚĞǀŝĐĞƐ
ŵĂŶLJƉƌŽƚŽĐŽůƐ
ŝŶƉŚLJƐŝĐĂů͕ůŝŶŬ͕
ƚƌĂŶƐƉŽƌƚ͕ĂŶĚ
ĂƉƉůŝĐĂƚŝŽŶ
ůĂLJĞƌƐ

The IP hourglass, at middle age
/W
dW
hW
,ddW
^DdW
Yh/
^,
ZdW
͙
ƚŚĞƌŶĞƚ
tŝ&ŝ ůƵĞƚŽŽƚŚ
WWW
WW
͙
ĐŽƉƉĞƌƌĂĚŝŽĨŝďĞƌ
/ŶƚĞƌŶĞƚ͛ƐŵŝĚĚůĞĂŐĞ
͞ůŽǀĞŚĂŶĚůĞƐ͍͟
ŵŝĚĚůĞďŽdžĞƐ͕
ŽƉĞƌĂƚŝŶŐŝŶƐŝĚĞƚŚĞ
ŶĞƚǁŽƌŬ
&ŝƌĞǁĂůůƐ
ĐĂĐŚŝŶŐ
IP hourglass, 在 middle age

Architectural Principles of the Internet
͞DĂŶLJŵĞŵďĞƌƐŽĨƚŚĞ/ŶƚĞƌŶĞƚĐŽŵŵƵŶŝƚLJǁŽƵůĚĂƌŐƵĞƚŚĂƚƚŚĞƌĞŝƐŶŽĂƌĐŚŝƚĞĐƚƵƌĞ͕ďƵƚŽŶůLJĂƚƌĂĚŝƚŝŽŶ͕
ǁŚŝĐŚǁĂƐŶŽƚǁƌŝƚƚĞŶĚŽǁŶĨŽƌƚŚĞĨŝƌƐƚϮρLJĞĂƌƐ;ŽƌĂƚůĞĂƐƚŶŽƚďLJƚŚĞ/Ϳ͘,ŽǁĞǀĞƌ͕ŝŶǀĞƌLJŐĞŶĞƌĂůƚĞƌŵƐ͕
ƚŚĞĐŽŵŵƵŶŝƚLJďĞůŝĞǀĞƐƚŚĂƚ
Z&ϭερΘ
ƚŚĞŐŽĂůŝƐĐŽŶŶĞĐƚŝǀŝƚLJ͕ƚŚĞƚŽŽůŝƐƚŚĞ/ŶƚĞƌŶĞƚ
WƌŽƚŽĐŽů͕ĂŶĚƚŚĞŝŶƚĞůůŝŐĞŶĐĞŝƐĞŶĚƚŽĞŶĚƌĂƚŚĞƌƚŚĂŶŚŝĚĚĞŶŝŶƚŚĞ
ŶĞƚǁŽƌŬ͘͟
dŚƌĞĞĐŽƌŶĞƌƐƚŽŶĞďĞůŝĞĨƐ͗
ƐŝŵƉůĞĐŽŶŶĞĐƚŝǀŝƚLJ
/WƉƌŽƚŽĐŽů͗ƚŚĂƚŶĂƌƌŽǁǁĂŝƐƚ
ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉůĞdžŝƚLJĂƚŶĞƚǁŽƌŬĞĚŐĞ
Architectural 原理 的 Internet

The end-end argument
ƐŽŵĞŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶĂůŝƚLJ;Ğ͘Ő͕͘ƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ͕ĐŽŶŐĞƐƚŝŽŶͿ
ĐĂŶďĞŝŵƉůĞŵĞŶƚĞĚŝŶŶĞƚǁŽƌŬ͕ŽƌĂƚŶĞƚǁŽƌŬĞĚŐĞ
ĞŶĚͲĞŶĚŝŵƉůĞŵĞŶƚĂƚŝŽŶŽĨƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŚŽƉͲďLJͲŚŽƉ;ŝŶͲŶĞƚǁŽƌŬͿŝŵƉůĞŵĞŶƚĂƚŝŽŶŽĨƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ

The end-end argument
͞dŚĞĨƵŶĐƚŝŽŶŝŶƋƵĞƐƚŝŽŶĐĂŶĐŽŵƉůĞƚĞůLJĂŶĚĐŽƌƌĞĐƚůLJďĞŝŵƉůĞŵĞŶƚĞĚŽŶůLJ
ǁŝƚŚƚŚĞŬŶŽǁůĞĚŐĞĂŶĚŚĞůƉŽĨƚŚĞĂƉƉůŝĐĂƚŝŽŶƐƚĂŶĚŝŶŐĂƚƚŚĞĞŶĚƉŽŝŶƚƐŽĨƚŚĞ
ĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵ͘dŚĞƌĞĨŽƌĞ͕ƉƌŽǀŝĚŝŶŐƚŚĂƚƋƵĞƐƚŝŽŶĞĚĨƵŶĐƚŝŽŶĂƐĂ
ĨĞĂƚƵƌĞŽĨƚŚĞĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵŝƚƐĞůĨŝƐŶŽƚƉŽƐƐŝďůĞ͘;^ŽŵĞƚŝŵĞƐĂŶ
ŝŶĐŽŵƉůĞƚĞǀĞƌƐŝŽŶŽĨƚŚĞĨƵŶĐƚŝŽŶƉƌŽǀŝĚĞĚďLJƚŚĞĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵŵĂLJ
ďĞƵƐĞĨƵůĂƐĂƉĞƌĨŽƌŵĂŶĐĞĞŶŚĂŶĐĞŵĞŶƚ͘Ϳ
tĞĐĂůůƚŚŝƐůŝŶĞŽĨƌĞĂƐŽŶŝŶŐĂŐĂŝŶƐƚůŽǁͲůĞǀĞůĨƵŶĐƚŝŽŶŝŵƉůĞŵĞŶƚĂƚŝŽŶƚŚĞ͞ĞŶĚͲ
ƚŽͲĞŶĚĂƌŐƵŵĞŶƚ͘͟
^ĂůƚnjĞƌ͕ZĞĞĚ͕ůĂƌŬϭεΘϭ
ƐŽŵĞŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶĂůŝƚLJ;Ğ͘Ő͕͘ƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ͕ĐŽŶŐĞƐƚŝŽŶͿ
ĐĂŶďĞŝŵƉůĞŵĞŶƚĞĚŝŶŶĞƚǁŽƌŬ͕ŽƌĂƚŶĞƚǁŽƌŬĞĚŐĞ

Where’s the intelligence?
ϮϬ
ƚŚ
ĐĞŶƚƵƌLJƉŚŽŶĞŶĞƚ͗
•ŝŶƚĞůůŝŐĞŶĐĞͬĐŽŵƉƵƚŝŶŐĂƚ
ŶĞƚǁŽƌŬƐǁŝƚĐŚĞƐ
/ŶƚĞƌŶĞƚ;ƉƌĞͲϮϬϬρͿ
•ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉƵƚŝŶŐĂƚ
ĞĚŐĞ
/ŶƚĞƌŶĞƚ;ƉŽƐƚͲϮϬϬρͿ
•ƉƌŽŐƌĂŵŵĂďůĞŶĞƚǁŽƌŬĚĞǀŝĐĞƐ
•ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉƵƚŝŶŐ͕ŵĂƐƐŝǀĞ
ĂƉƉůŝĐĂƚŝŽŶͲůĞǀĞůŝŶĨƌĂƐƚƌƵĐƚƵƌĞĂƚĞĚŐĞ
哪里’s intelligence?

Chapter 4: done!
Question: how do forwarding tables 
(destination-based forwarding) or flow 
tables (generalized forwarding) computed?
Answer: by the control plane (next chapter)
4.1 Overview of Network layer: data plane 
and control plane
4.2 What䇻s inside a router
4.3 IP: Internet Protocol
•datagram format
•fragmentation
•IPv4 addressing
•NAT
•IPv6
4.4 Generalized Forward and SDN
•match plus action
•OpenFlow example
4-
108
第 4 章:
问题:如何 做 转发表s
(destination-based 转发) 或 流
表 (generalized 转发) 计算得到的?
由 控制平面 (下一个 本章)
4.1 概述 的 网络层：数据平面
和 控制平面
4.2 什么
s inside a 路由器
4.3 IP: Internet 协议
数据报 格式
IPv4 编址
4.4 Generalized 转发 和 SDN
匹配 加 动作
OpenFlow 示例

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
