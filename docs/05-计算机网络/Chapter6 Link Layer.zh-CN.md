# Chapter6 Link Layer.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter6 Link Layer.zh-CN.pdf　｜　64,147 字符　｜　129 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
计算机网络
秋季

Chapter 6
The Link Layer and LANs
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)
If you post any slides on a www site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
第 6 章
链路层和局域网
说明 在 使用 的 这些 PowerPoint 幻灯片:
我们’
re making 这些 幻灯片 freely 可用的 到 所有 (faculty, students,
readers). They’re 在 PowerPoint form 因此 你 见 animations;
和 可以 添加, 修改, 和 删除 幻灯片 (包括 这个 一个) 和
幻灯片 内容 到 suit your needs. They obviously represent a
的
工作 在 我们的 part. 在 返回 用于 使用, 我们 仅 ask following:
如果 你 使用 这些 幻灯片 (e.g., 在 a 课堂) 这 你 mention 它们的
源 (之后 所有, 我们’
d 像 people 到 使用 我们的 book!)
如果 你 post 任何 幻灯片 在 a 网站 站点, 这 你 说明 这 they
是 adapted 从 (或 perhaps identical 到) 我们的 幻灯片, 和 说明
我们的 版权 的 这个 material.
用于 a revision 历史, 见 幻灯片 说明 用于 这个 page.
谢谢 和 使用愉快!
所有 材料 版权 1996
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved
计算机 网络:
Down 方法

Homework

Ch8：2、5、11、13、14、15、17、18、21、26、29、31、32

Keywords: VLAN,  CSMA/CD/CA, CRC, TDM, CDM, ALOHA, 
ARP, self-learning, ATM, MPLS, PPP,Ethernet,
MAC 
protocols
作业
关键词： VLAN, CSMA/CD/CA, CRC, TDM, CDM, ALOHA,
学习, ATM, MPLS, PPP,以太网,
协议

5-4
Chapter 6: Link layer
our goals:

understand principles behind link layer 
services:

error detection, correction

sharing a broadcast channel: multiple access

link layer addressing

local area networks: Ethernet, VLANs

instantiation, implementation of various link 
layer technologies
本章链路层
我们的目标：
understand 原理 背后的 链路层
服务:
差错检测, 纠正
sharing a 广播 信道: 多路访问
链路层 编址
局域网s: 以太网, VLANs
实例化, 实现 的 various 链路
层 technologies

5-5
Link layer, LANs: outline
6.1 introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

6
链路层网络：
应用层网络：
传输层网络：
网络层网络：
App
App
App
App
App
App
App
App
Point to point
Logical channel
Net to Net transmission path
MAC to MAC
应用
应用
应用
应用
应用
应用
应用
应用
点 到 点
Logical 信道
网络 到 网络
传输路径
MAC 到 MAC

7
A
B
How does A send data to B?
How does each network element process data?
如何做 发送 数据 到 B?
如何 做 每个 网络 element 进程 数据?

Link layer: introduction

Each network interface card has an address named physical 
address

TV-net, telephone-net, data-net, ...

Physical address is different from IP address

Physical addresses do NOThave a uniform format and standard. It 
may be 12bits, 48bits, or others.
8
链路层: 引言
每个 网络 接口 card 有 地址 named 物理
地址
网络, telephone
网络, 数据
网络, ...
物理 地址 是 不同的 从 IP 地址
物理 地址 做有 auniform 格式 和 standard
. 它
可能 是 12比特, 48比特, 或 others.

Link layer: introduction

In a same subnet, the nodes are considered to have a 
uniform standard physical address that is used for 
addressing.
9
MAC address is used for 
addressing in the same subnet
链路层: 引言
在 a 相同 子网, 节点 是 considered 到 有
uniform standard 物理 地址这 是 用于 用于
MAC 地址 是 用于 用于
编址 在 相同 子网

5-10
Link layer: introduction
terminology:

hosts and routers: nodes

communication channels that connect 
adjacent
nodes along communication 
path: links

wired links

wireless links

LANs

layer-2 packet: frame,encapsulates 
datagram
data-link layerhas responsibility of 
transferring datagram from one node 
to 
physically adjacent
node over a link
global ISP
链路层: 引言
主机 和 路由器:节点
通信 信道 这 连接
节点 along 通信
路径:链路
有线 链路
无线链路
局域网
层2 分组:帧,
数据报
数据
链路层
有 responsibility 的
transferring 数据报 从 一个 节点
到
物理上 adjacent
节点 在 a 链路
全局 ISP

5-11
Link layer: context

datagram transferred by different link 
protocols over different links:

e.g., Ethernet on firstlink, frame relay 
on intermediatelinks, 
802.11 on lastlink

each  link protocol provides different 
services

e.g., may or may not provide rdtover 
link
Ethernet
Frame relay
WLAN
ETH
IP
FR
IP
WLAN
IP
FR
IP
链路层: context
数据报 transferred 由不同的 链路
协议 在 不同的 链路
e.g., 以太网 在
第一个
链路, 帧 relay
在链路,
在最后链路
每个 链路 协议 提供 不同的
服务
e.g., 可能 或 可能 不 提供在
链路
以太网
帧 relay

5-12
Link layer services

framing, link access:

encapsulate datagram into frame, adding 
header, trailer

channel access if shared medium

“MAC”addresses used in frame headers 
to identify source, dest

different from IP address!

reliable delivery between adjacent nodes

we learned how to do this already (chapter 
3)!

seldom used on low bit-error link (fiber, 
some twisted pair)

wireless links: high error rates

Q:why both link-level and end-end reliability?
IPMAC Hdr
...
...
链路层 服务
framing, 链路 访问:
encapsulate 数据报 进入 帧, adding
首部, trailer
信道 访问 如果 共享 介质
地址 用于 在 帧 首部
到 identify 源,
不同的 从 IP 地址
可靠交付 之间 adjacent 节点
我们 learned 如何 到 做 这个 already (本章
seldom 用于 在 low 比特
差错 链路 (fiber,
一些 twisted pair)
无线链路: high 差错 速率
why 二者 链路level 和 端端 reliability?

5-13

flow control:

pacing between adjacent sending and receiving nodes

error detection: 

errors caused by signal attenuation, noise. 

receiver detects presence of errors: 

signals sender for retransmission or drops frame 

error correction:

receiver identifies and correctsbit error(s) without resorting to 
retransmission

half-duplex and full-duplex

with half duplex, nodes at both ends of link can transmit, but not 
at same time
Link layer services (more)
Why no congestion control?
...
...
流量控制:
pacing 之间 adjacent 发送 和 接收 节点
差错检测
差错 caused 由 信号 attenuation, noise.
接收方 detects presence 的 差错:
signals 发送方 用于 重传 或 drops 帧
差错纠正:
接收方 identifies和 corrects比特 差错(s) 没有 resorting 到
重传
duplex 和 完整的
与 half duplex, 节点 在 二者 端 的 链路 可以 传输, 但是 不
在 相同 时间
链路层 服务 (更多)
Why 无 拥塞控制?

5-14
Where is the link layer implemented?

in each and every host

link layer implemented in “adaptor”(aka 
network interface cardNIC) or on a chip

Ethernet card, 802.11 card; Ethernet 
chipset

implements link, physical layer

attaches into host’s system buses

combination of hardware, software, 
firmware
controller
physical
transmission
cpu
memory
host 
bus 
(e.g., PCI)
network adapter
card
application
transport
network
link
link
physical
哪里 是 链路层 implemented?
在 每个 和 每一个 主机
链路层 implemented 在
网络 接口 cardNIC) 或 在 a chip
以太网 card, 802.11 card; 以太网
implements 链路, 物理 层
attaches 进入 主机’s 系统 buses
combination 的 hardware, 软件,
控制器
物理
传输
内存
主机
总线
网络 适配器
应用
运输
网络
链路
链路
物理

controller
physical
memory
CPU
Interfaces communicating
controller
physical
cpumemory
application
transport
network
link
link
physical
application
transport
network
link
link
physical
sending side:
encapsulates datagram in frame
adds error checking bits, reliable 
data transfer, flow control, etc.
receiving side:
looks for errors, reliable data 
transfer, flow control, etc.
extracts datagram, passes to 
upper layer at receiving side
link
h
link
h
link
h
link
h
link
h
link
h
datagram
datagram
datagram
Link Layer: 6-15
控制器
物理
内存
接口 communicating
控制器
物理
内存
应用
运输
网络
链路
链路
物理
应用
运输
网络
链路
链路
物理
发送 一侧:
encapsulates 数据报 在 帧
adds 差错 checking 比特, 可靠
数据 传输, 流量控制, etc.
接收 一侧:
looks 用于 差错, 可靠 数据
传输, 流量控制, etc.
extracts 数据报, passes 到
upper 层 在 接收 一侧
链路链路链路链路链路链路
数据报
数据报
数据报
链路层: 6

16
S3
CCC
S4
CCC2
S2
S
C
C
C
S1
C1
C
C
目标：C1→C2
C1获取C2的IP，填入IP报头
C1根据IP
C2
查路由表得到下一
跳路由器R1的地址IP
R1
C1广播查询IP
R1
对应的物理地
址MAC
R1
C1把MAC
R1
填入链路帧头；
C1把链路帧发给S1;
S1根据帧头的MAC
R1
转发给
R1；
R1抽取IP，根据IP
C2
查路由表
，决定下一跳转发给IP
R2
；
R1广播查询IP
R2
对应的物理地
址MAC
R2
R1把MAC
R2
填入帧头，转发给
R2
R2  ?
S1
R2
R1
R3
R4
R5

5-17
Link layer, LANs: outline
6.1introduction, services
6.2 error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

Link Layer5-18
Error detection
EDC= Error Detection and Correction bits (redundancy)
D    = Data protected by error checking, may include header fields 
•Error detection not 100% reliable!
•protocol may miss some errors, but rarely
•larger EDC field yields better detection and correction
otherwise
链路层
差错检测
EDC= 差错检测和纠正 比特 (redundancy)
= 数据 protected 由 差错 checking, 可能 include 首部 字段
差错检测 不 100% 可靠!
协议 可能 miss 一些 差错, 但是 rarely
larger EDC 字段 yields better 检测 和 纠正

Link Layer5-19
Parity checking
single bit parity:

detect single bit errors
two-dimensional bit parity:

detect and correct single bit errors
0
0
链路层
单个 比特 parity:
etect 单个 比特 差错
两个
dimensional 比特 parity:
检测 和 correct 单个 比特 差错

5-20
Internet checksum 
(review)
sender:

treat segment contents as 
sequence of 16-bit integers

checksum: addition (1’s 
complement sum) of 
segment contents

sender puts checksum 
value into UDP checksum 
field
receiver:

compute checksum of 
received segment

check if computed 
checksum equals 
checksum field value:

NO -error detected

YES -no error detected. 
But maybe errors 
nonetheless?
goal:
detect “errors”(e.g., flipped bits) in transmitted packet 
(note: used at transport layer only)
Internet 校验和
发送方:
treat 报文段 contents as
序列 的
比特 integers
校验和: addition (1
complement sum) 的
报文段 contents
发送方 puts 校验和
值 进入 UDP 校验和
字段
接收方:
计算 校验和 的
已收到 报文段
查看 如果 计算得到的
校验和 equals
校验和 字段 值:
差错 detected
无 差错 detected.
但是 maybe 差错
目标:
检测差错
(e.g., flipped 比特) 在 已传输 分组
(说明: 用于 在 运输层 仅)

5-21
Cyclic redundancy check

more powerful error-detection coding

view data bits, D, as a binary number

choose r+1 bit pattern (generator), G

goal: choose r CRC bits, R, such that

<D,R> exactly divisible by G (modulo 2) 

receiver knows G, divides <D,R> by G.  If non-zero remainder: 
error detected!

can detect all burst errors less than r+1 bits

widely used in practice (Ethernet, 802.11 WiFi, ATM)
R=f(D)
Cyclic redundancy 查看
更多 powerful 差错检测 coding
view 数据 比特,
, as a binary 编号
选择
r+1 比特
目标: 选择 r CRC 比特,, such 这
<D,R> exactly divisible 由 G (modulo 2)
接收方 知道 G, divides <D,R> 由 G. 如果 non
差错 detected!
可以 检测 所有 burst 差错 less 比 r+1 比特
widely 用于 在 practice (以太网, 802.11


generator
22
Cyclic redundancy check
6543    2    1    0
6   4   2
1  0  1  0  1  1  1+
1
( )
x  x  x  x  x  x  x
x x x x
G x
             
    

4   3
( )1
?
G x x x x   

1        0       1        0       1       1       1
2
0
2
1
2
2
2
3
2
6
2
4
2
5
Cyclic redundancy 查看

5-23
CRC example
want:
D
.
2
r
XOR  R = nG
equivalently:
D
.
2
r
= nGXOR  R 
equivalently:
if we divide D
.
2
r
by G, 
want remainder R to 
satisfy:
R
= remainder[           ]
D
.
2
r
G
R
r=3
Sender: 
101110011
001110011
111110011
100110011
...
DR
余数的位数一定要是比除数位数只能
少一位，哪怕前面位是0，甚至是全
为0（整除时）也都不能省略
CRC 示例
如果 我们 divide D
由 G,
want remainder R 到
发送方:

5-24
Link layer, LANs: outline
5.1introduction, services
5.2error detection, correction 
5.3 multiple access protocols
5.4LANs

addressing, ARP

Ethernet

switches

VLANS
5.5link virtualization: MPLS
5.6data center networking
5.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
5.3 多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-25
Multiple access links, protocols
two types of “links”:

point-to-point

PPP for dial-up access

point-to-point link between Ethernet switch, host

broadcast (shared wire or medium)

old-fashioned Ethernet

upstream HFC

802.11 wireless LAN
shared wire (e.g., 
cabled Ethernet)
shared RF
(e.g., 802.11 WiFi)
shared RF
(satellite) 
humans at a
cocktail party 
(shared air, acoustical)
多路访问 链路, 协议
两个 types 的
链路
点到点
PPP 用于 dial
上 访问
点到点 链路 之间以太网 交换机, 主机
广播 (共享 wire 或 介质)
fashioned 以太网
上游 HFC
802.11 无线 局域网
共享 wire (e.g.,
cabled 以太网)
共享 RF
共享 RF
humans 在 a
(共享 air, acoustical)

5-26
Multiple access protocols

single shared broadcast channel 

two or more simultaneous transmissions by nodes: interference 

collisionif node receives two or more signals at the same time
multiple access protocol

distributed algorithm that determines how nodes share channel, i.e., 
determine when node can transmit

communication about channel sharing must use channel itself! 

no out-of-band channel for coordination
多路访问协议
单个 共享 广播 信道
两个 或 更多 simultaneous transmissions 由 节点: 干扰
碰撞如果 节点 接收 两个 或 更多 signals 在 相同 时间
多路访问协议
分布式 算法 这 determines如何 节点 share 信道
确定 当 节点 可以 传输
通信 关于 信道 sharing 必须 使用 信道 itself!
无 查看
的信道 用于 coordination

5-27
An ideal multiple access protocol
given:broadcast channel of rate R bps
desiderata:
1. when one node wants to transmit, it can send at full rate R.
2. when M nodes want to transmit, each can send at average rate 
R/M
3. fully decentralized:

no special node to coordinate transmissions

no synchronization of clocks, slots
4. simple
ideal 多路访问协议
给定的:
广播 信道 的 速率 R bps
1. 当 一个 节点 wants 到 传输, 它 可以 发送 在
完整的 速率
2. 当 M 节点 want 到 传输, 每个 可以 发送 在average 速率
3. fully 分散式:
无 special 节点 到 coordinate transmissions
无 synchronization 的 clocks, 时隙

5-28
MAC protocols: taxonomy
three broad classes:

channel partitioning

divide channel into smaller “pieces”(time slots, frequency, code)

allocate piece to node for exclusive use

random access

channel not divided, allow collisions

“recover”from collisions

“taking turns”

nodes take turns, but nodes with more to send can take longer turns
MAC 协议s: taxonomy
信道划分
divide 信道 进入 smaller(时间 时隙, 频率, 码)
allocate piece 到 节点 用于 exclusive 使用
随机访问
信道 不 divided, allow 碰撞s
从 碰撞s
轮流
节点 take turns, 但是 节点 与 更多 到 发送 可以 take longer turns

5-29
Channel partitioning MAC protocols: TDMA
TDMA: time division multiple access

access to channel in "rounds" 

each station gets fixed length slot (length = pkttrans time) 
in each round 

unused slots go idle 

example: 6-station LAN, 1,3,4 have pkt, slots 2,5,6 idle 
1
3
4
1
3
4
6-slot
frame
6-slot
frame
fr
equency bands
信道划分 MAC 协议s: TDMA
TDMA: 时间 划分 多路访问
访问 到 信道 在 "rounds"
每个 站 获得 fixed 长度 时隙 (长度 =trans 时间)
在 每个 round
unused 时隙 go idle
示例: 6站 局域网, 1,3,4 有
, 时隙 2,5,6 idle
时隙
帧
时隙
帧

5-30
FDMA: frequency division multiple access 

channel spectrum divided into frequency bands

each station assigned fixed frequency band

unused transmission time in frequency bands go idle 

example: 6-station LAN, 1,3,4 have pkt, frequency bands 2,5,6 idle 
fr
equency bands
FDM cable
Channel partitioning MAC protocols: FDMA
FDMA: 频率 划分 多路访问
信道 频谱 divided 进入 频率 bands
每个 站 assigned fixed 频率 band
unused 传输 时间 在 频率 bands go idle
示例: 6站 局域网, 1,3,4 有, 频率 bands 2,5,6 idle
FDM 电缆
信道划分 MAC 协议s: FDMA

5-31
Random access protocols

when node has packet to send

transmit at full channel data rate R.

no a prioricoordination among nodes

two or more transmitting nodes ➜“collision”,

random access MAC protocolspecifies: 

how to detect collisions

how to recover from collisions (e.g., via delayed retransmissions)

examples of random access MAC protocols:

ALOHA

slotted ALOHA

CSMA, CSMA/CD, CSMA/CA
随机访问 协议
当 节点 有 分组 到 发送
传输 在 完整的 信道 数据 速率 R.
无coordination 之间 节点
两个 或 更多 transmitting 节点碰撞
随机访问 MAC 协议
如何 到 检测 碰撞s
如何 到 recover 从 碰撞s (e.g., 通过 时延ed retransmissions)
示例 的 随机访问 MAC 协议s:

Link Layer5-32
Pure (unslotted) ALOHA

unslottedAloha: simpler, no synchronization

when frame first arrives

transmit immediately

collision probability increases:

frame sent at t
0
collides with other frames sent in [t
0
-1,t
0
+1]
Collides may 
happen at any time
链路层
Aloha: simpler, 无 synchronization
当 帧 第一个 到达
传输
碰撞 probability increases:
帧 已发送 在 tcollides 与 其他 帧 已发送 在 [t
Collides 可能
happen 在 任何 时间

5-33
Pure ALOHA efficiency
Pr[success by given node] = Pr[node transmits] 
.
Pr[no other node transmits in [t
0
-1,t
0
] ]
.
Pr[no other node transmits in [t
0
,t
0
+1]]
= p 
. 
(1-p)
N-1
. 
(1-p)
N-1  
= p 
. 
(1-p)
2(N-1)
Pr[asuccessful slots] = Np(1-p)
2N-1
... choosing optimum p and then letting N 
= 1/(2e) = .18
worsethan the following slotted Aloha!
Pure ALOHA 效率
[success 由 给定的 节点] =[节点 传输]
[无 其他 节点 传输 在 [t
[无 其他 节点 传输 在 [t
successful 时隙
... choosing optimum p 和 则 letting
比 following slotted Aloha!

5-34
Slotted ALOHA
assumptions:

all frames same size

time divided into equal size 
slots(time to transmit 1 
frame)

nodes start to transmit only 
slot 
beginning

nodes are synchronized

if 2 or more nodes transmit 
in slot, all nodes detect 
collision
operation:

when node obtains fresh 
frame, transmits in next slot

if no collision:node can 
send new frame in next 
slot

if collision:node 
retransmits frame in each 
subsequent slot with 
prob. puntil success
所有 帧相同 大小
时间 divided 进入
equal 大小
时隙(时间 到 传输 1
帧)
节点 开始 到 传输仅
时隙
节点 是 synchronized
如果 2 或 更多 节点 传输
在 时隙, 所有 节点 检测
碰撞
当 节点 obtains fresh
帧, 传输 在
下一个 时隙
如果 无 碰撞:节点 可以
发送 新的 帧 在 下一个
时隙
如果 碰撞:节点
retransmits 帧 在 每个
subsequent 时隙 与
直到 success

5-35
Pros:

single active node can 
continuously transmit at 
full rate of channel

highly decentralized: only 
slots in nodes need to be 
in sync

simple
Cons:

collisions, wasting slots

idle slots

nodes may be able to detect 
collision in less than time to 
transmit packet

clock synchronization
Slotted ALOHA
1
1
1
1
2
3
22
33
node 1
node 2
node 3
C
CCSS
SE
EE
Collides only 
happen at the 
beginning
单个 主动 节点 可以
continuously 传输 在
完整的 速率 的 信道
highly 分散式: 仅
时隙 在 节点 需要 到 是
在 sync
碰撞s, wasting 时隙
idle 时隙
节点 可能 是 able 到 检测
碰撞 在 less 比 时间 到
传输 分组
节点 1
节点 2
节点 3
Collides 仅
happen 在

5-36

suppose:Nnodes with many 
frames to send, each transmits 
in slot with probability p

probthat given node has 
success in a slot  = p(1-p)
N-1

probof a successful slots = 
Np(1-p)
N-1

max efficiency: find p* 
that maximizes 
Np(1-p)
N-1

for many nodes, take limit 
of Np*(1-p*)
N-1 
as N goes 
to infinity, gives:
max efficiency = 1/e = .37
efficiency
: long-run 
fraction of successful slots 
(many nodes, all with many 
frames to send)
at best:
channel
used for useful 
transmissions 37%
of time!
!
Slotted ALOHA: efficiency
节点 与 许多
帧 到 发送, 每个 传输
在 时隙 与 probability
这 给定的 节点 有
success 在 a 时隙 =
的 a
successful 时隙
最大值 效率: find p*
这 maximizes
用于 许多 节点, take limit
的
到 infinity, 给出:
最大值 效率 = 1/e = .37
效率
运行
fraction 的
successful 时隙
(许多 节点, 所有 与 许多
帧 到 发送)
在 best:
信道
用于 用于 有用
的 时间!
Slotted ALOHA: 效率

5-37
CSMA (carrier sense multiple access)
CSMA:
listen before transmit:
if channel sensed idle:transmit entire frame

if channel sensed busy, defer transmission 

human analogy: don’t interrupt others!
CSMA (carrier sense 多路访问)
listen 之前 传输:
如果 信道 sensed idle:传输 entire 帧
如果 信道 sensed busy, defer 传输

5-38
CSMA collisions

collisions canstill occur: 
propagation delay means  two 
nodes may not hear each other’s 
transmission

Nodes don’t detect collision 
during transmission

collision: entire packet 
transmission time wasted

distance & propagation delay play 
role in in determining collision 
probability
spatial layout of nodes 
CSMA 碰撞s
碰撞s可以
仍然 occur:
propagation 时延 means 两个
节点 可能 不 hear 每个 其他
传输
节点 don’t 检测 碰撞
during 传输
碰撞:entire 分组
传输 时间 wasted
距离 & propagation 时延 play
role 在 在 determining 碰撞
spatial layout 的 节点

5-39
CSMA/CD (collision detection)
CSMA/CD:
carrier sensing, deferral as in CSMA

collisions detectedwithin short time

colliding transmissions aborted, reducing channel wastage 

collision detection:

easy in wired LANs: measure signal strengths, compare 
transmitted, received signals

difficult in wireless LANs: received signal strength overwhelmed 
by local transmission strength 

human analogy: the polite conversationalist 
CSMA/CD (碰撞 检测)
carrier sensing, deferral as 在 CSMA
碰撞s
在...内 short 时间
colliding transmissions aborted, reducing 信道 wastage
碰撞 检测:
easy 在 有线 局域网: measure 信号强度s, compare
已传输, 已收到 signals
difficult 在 无线 局域网: 已收到 信号强度 overwhelmed
由 本地 传输 强度

5-40
CSMA/CD (collision detection)
spatial layout of nodes 
CSMA/CD (碰撞 检测)
spatial layout 的 节点

5-41
Ethernet CSMA/CD algorithm
1. NIC receives datagram from 
network layer, creates frame
2. If NIC senses channel idle, starts 
frame transmission. If NIC senses 
channel busy, waits until channel 
idle, then transmits.
3. If NIC transmits entire frame 
without detecting another 
transmission, NIC is done with 
frame !
4. If NIC detects another 
transmission while transmitting,  
aborts and sends jam signal
5. After aborting, NIC enters binary 
(exponential) backoff: 

after m
th
collision, NIC chooses 
Kat random from {0,1,2, ..., 2
m
-
1}. NIC waits K·512 bit times, 
returns to Step 2

longer backoffinterval with more 
collisions
以太网 CSMA/CD 算法
NIC 接收 数据报 从
网络层, 创建 帧
如果 NIC senses 信道 idle, starts
帧 transmission. 如果 NIC senses
信道 busy, waits 直到 信道
idle, 则 transmits.
如果 NIC 传输 entire 帧
没有 detecting 另一个
传输, NIC 是 done 与
帧 !
如果 NIC detects 另一个
传输 while transmitting,
aborts 和 发送 jam 信号
之后 aborting, NIC enters
之后碰撞, NIC chooses
在 随机 从
512 比特 times
returns 到 步骤 2
interval 与 更多
碰撞s

42
根据标准，10Mbps 以太网采用中继器时，连接最大长度为2500米，
最多通过4个中继器，因此10Mbps 以太网帧的最小传输时间为51.2
微秒。这段时间可以传输的数据是512位，所以也叫512位时间。这个
时间被定义为以太网slot，即碰撞槽。512位= 64字节，这也是以太
网frame最少64字节的原因。
无论在Ethernet II还是IEEE802.3标准中，从preamble到FCS字段的
最小帧长必须是64字节。最小帧长度可确保以太网网络接口卡有足够
的传输时间来准确检测冲突。这个最短时间是根据网络的最大电缆长
度和帧沿电缆长度传播所需的时间确定的。基于最小帧长度为64字节
并且使用6字节地址字段的要求。这意味着每个数据字段的最小长度
是46个字节。
时隙
帧
以太网 II

5-43
CSMA/CD efficiency

t
prop
= max prop delay between 2 nodes in LAN

t
trans
= time to transmit max-size frame

efficiency goes to 1 (i.e., NO collision)

as t
prop
goes to 0

as t
trans
goes to infinity

better performance than ALOHA: and simple, cheap, decentralized
!
transprop
/tt
efficiency
51
1


No time difference
Always occupying the channel
CSMA/CD 效率
= 最大值 prop 时延 之间 2 节点 在 局域网
= 时间 到 传输 最大值大小 帧
效率 goes 到 1 (i.e., NO 碰撞)
goes 到 0
goes 到 infinity
better 性能 比 ALOHA: 和 simple, cheap, 分散式
效率
无时间 difference
总是信道

5-44
“Taking turns”MAC protocols
channel partitioning MAC protocols:

share channel efficientlyand fairlyat high load

inefficient at low load: delay in channel access, 1/N 
bandwidth allocated even if only 1 active node! 
random access MAC protocols

efficient at low load: single node can fully utilize 
channel

high load: collision overhead
“taking turns”protocols
look for best of both worlds!
轮流MAC 协议s
信道划分 MAC 协议s:
share 信道和在 high 负载
inefficient 在 low 负载: 时延 在 信道 访问, 1/N
带宽 allocated even 如果 仅 1 主动 节点!
随机访问 MAC 协议s
高效的 在 low 负载: 单个 节点 可以 fully utilize
信道
high 负载: 碰撞 overhead
轮流协议
look 用于 best 的 二者 worlds!

5-45
polling:

master node “invites”slave nodes to transmit in turn

typically used with “dumb”slave devices

concerns:

polling overhead 

latency

single point of failure (master)
master
slaves
poll
data
data
“Taking turns”MAC protocols
master 节点slave 节点 到 传输 在 turn
通常 用于 与slave 设备
单个 点 的 failure (master)
数据
数据
轮流MAC 协议s

5-46
token passing:

control 
token
passed from one node to next sequentially.

token message

concerns:
token overhead 
latency
single point of failure (token)
T
data
(nothing
to send)
T
“Taking turns”MAC protocols
控制
passed 从 一个 节点 到 下一个 sequentially.
token 报文
单个 点 的 failure (token)
数据
到 发送)
轮流MAC 协议s

cable headend
CMTS
ISP
cable modem
termination system

multiple 40Mbps downstream (broadcast) channels
single CMTS transmits into channels

multiple30 Mbps upstream channels
multiple access: all users contend for certain upstream channel 
time slots (others assigned)
A Case: Cable access network
cable
modem
splitter
...
...
Internet frames,TVchannels, control  transmitted 
downstream at different frequencies
upstream Internet frames, TV control,  transmitted 
upstream at different frequencies in time slots
47
电缆 headend
电缆 modem
termination 系统
多路40Mbps downstream (广播) 信道
单个 CMTS 传输 进入 信道
多路30 Mbps 上游 信道
多路访问:
所有 用户 contend 用于 certain 上游 信道
时间 时隙
Case: 电缆 访问 网络
电缆
帧,TV信道, 控制 已传输
downstream 在不同的 frequencies
上游 Internet 帧, TV 控制, 已传输
上游在 不同的 frequencies 在 时间 时隙

5-48
DOCSIS: data over cable service interface spec 

FDMover upstream, downstream frequency channels

TDMupstream: some slots assigned, some have contention
downstream MAP frame: assigns upstream slots
request for upstream slots(and data) transmitted random 
access (binary backoff) in selected slots
MAP frame for
Interval [t1, t2]
Residences with cable modems
Downstream channel i
Upstream channel j
t
1
t
2
Assigned minislots containing cable modem
upstream data frames
Minislots containing 
minislots request frames
cable headend
CMTS
Cable access network
数据 在 电缆 服务 接口 spec
在 上游, downstream 频率 信道
上游: 一些 时隙 assigned, 一些 有 contention
downstream MAP 帧: assigns 上游 时隙
请求 用于 上游 时隙(和 数据) 已传输 随机
访问 (binary
) 在 selected 时隙
MAP 帧 用于
Residences 与 电缆 modems
Downstream 信道 i
上游 信道 j
Assigned minislots 包含 电缆 modem
上游 数据 帧
Minislots 包含
minislots 请求 帧
电缆 headend
电缆 访问 网络

TDM+FDM+CDM
49

5-50
Summary of MAC protocols

channel partitioning,by time, frequency or code

Time Division, Frequency Division

random access(dynamic), 

ALOHA, S-ALOHA, CSMA, CSMA/CD

carrier sensing: easy in some technologies (wire), hard in others 
(wireless)

CSMA/CD used in Ethernet

CSMA/CA used in 802.11

taking turns

polling from central site, token passing

bluetooth, FDDI,  token ring 
A question: why do we use CSMA/CD rather than TDMA/FDMA？
总结 的 MAC 协议s
信道划分,由 时间, 频率 或 码
时间 划分, 频率 划分
随机访问(动态),
carrier sensing: easy 在 一些 technologies (wire), hard 在 others
(无线)
CSMA/CD 用于 在 以太网
CSMA/CA 用于 在 802.11
轮流
polling 从 central 站点, token passing
问题: why 做 我们 使用 CSMA/CD rather 比 TDMA/FDMA

5-51
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-52
MAC addresses and ARP

32-bit IP address: 

network-layeraddress for interface

used for layer 3 (network layer) forwarding

MAC (or LAN or physical or Ethernet) address:

function:used ‘locally”to get frame from one 
interface to another physically-connected interface 
(same network, in IP-addressing sense)

48 bit MAC address (for most LANs) burned in NIC 
ROM, also sometimes software settable

e.g.: 1A-2F-BB-76-09-AD
hexadecimal (base 16) notation
(each “number”represents 4 bits)
After the packet enters the local 
network, it depends on its MAC 
address for addressing
Switch
NOT router
MAC 地址es 和 ARP
比特 IP 地址:
网络层地址 用于 接口
用于 用于 层 3 (网络层) 转发
MAC (或 局域网 或 物理 或 以太网) 地址:
功能:用于到 获得 帧 从 一个
接口 到 另一个 物理上连接的 接口
(相同 网络, 在 IP编址 sense)
48 比特 MAC 地址 (用于 most 局域网) burned 在 NIC
ROM, also sometimes 软件 settable
hexadecimal (基 16) 记号
(每个编号
represents 4 比特)
之后 分组 enters 本地
网络, 它 depends 在 它的 MAC
地址 用于编址
交换机
NOT 路由器

5-53
LAN addresses and ARP
each adapter on LAN has unique 
LANaddress 
NIC decides the frame is received 
or not by its destination MAC 
address.
adapter
1A-2F-BB-76-09-AD
58-23-D7-FA-20-B0
0C-C4-11-6F-E3-98
71-65-F7-2B-08-53
LAN
(wired or
wireless)
局域网 地址 和 ARP
每个 适配器 在 局域网 有 unique
地址
NIC decides 帧 是 已收到
或 不 由 它的 目的地 MAC
适配器
(有线 或
无线)

5-54
LAN addresses (more)

MAC address allocation administered by IEEE

manufacturer buys portion of MAC address space (to assure 
uniqueness)

analogy:

MAC address: like identity card number (fixed)

IP address: like postal address (changeable)

MAC flataddress  ➜portability 

can move LAN card from one LAN to another

IP hierarchical address notportable

address depends on IP subnet to which node is attached
IPMAC
逻辑地址物理地址
全局有效局部有效
层次化平面型
网间寻址网内寻址
局域网 地址 (更多)
MAC 地址 allocation administered 由 IEEE
manufacturer buys portion 的 MAC 地址 空间 (到 assure
MAC 地址: 像编号 (
IP 地址: 像 postal地址 (changeable)
地址
可以 移动 局域网 card 从 一个 局域网 到 另一个
IP hierarchical 地址不
地址depends 在 IP 子网到 哪个 节点 是 attached

5-55
ARP: address resolution protocol
ARP table: each IP node (host, router) on 
LAN has table

IP/MAC address mappings for 
some LAN nodes:
< IP address; MAC address; TTL>

TTL (Time To Live): time after 
which address mapping will be 
forgotten (typically 20 min)
Question:how to determine
interface’s MAC address, 
knowing its IP address?
1A-2F-BB-76-09-AD
58-23-D7-FA-20-B0
0C-C4-11-6F-E3-98
71-65-F7-2B-08-53
LAN
137.196.7.23
137.196.7.78
137.196.7.14
137.196.7.88
ARP: 地址 resolution 协议
ARP 表:
每个 IP 节点 (主机, 路由器) 在
局域网 有 表
IP/MAC 地址 mappings 用于
一些 局域网 节点:
< IP 地址; MAC 地址; TTL>
TTL (时间 到 Live): 时间 之后
哪个 地址 mapping 将 是
forgotten (通常 20 最小值)
问题:如何 到 确定
接口s MAC 地址,
knowing 它的 IP 地址?

5-56
ARP protocol: same LAN

A wants to send datagram to B

B’s MAC address NOTin A’s ARP table.

A broadcastsARP query packet, 
containing B's IP address 

destMAC address = FF-FF-FF-FF-FF-FF

all nodes on LAN receive ARP query 

B receives ARP packet, replies to A 
with its (B's) MAC address

frame sent to A’s MAC address (unicast)

A caches (saves) IP-to-MAC address 
pair in its ARP table until information 
becomes old (times out)

soft state: information that times out 
(goes away) unless refreshed

ARP is “plug-and-play”:

nodes create their ARP tables without 
intervention from net administrator
ARP 协议: 相同 局域网
wants 到 发送 数据报 到 B
’s MAC 地址在 ’s ARP table.
ARP 查询 分组,
包含 B's IP 地址
MAC 地址 = FF
所有 节点 在 局域网 接收 ARP 查询
B 接收 ARP 分组, replies 到
与 它的 (B's) MAC 地址
帧 已发送 到 ’s MAC 地址 (
到MAC 地址
pair 在 它的 ARP 表 直到 信息
becomes old (times 查看)
soft 状态: 信息 这 times 查看
ARP 是和
节点 create 它们的 ARP 表没有
intervention 从 网络 administrator

ARP protocol in action
Link Layer: 6-57
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
A
broadcasts ARP query, containing B's IP addr
•destination MAC address = FF-FF-FF-FF-
FF-FF
•all nodes on LAN receive ARP query 
1
Source MAC:  71-65-F7-2B-
08-53
Source IP: 137.196.7.23
Target IP address: 
137.196.7.14
...
1
Ethernet frame (sent to FF-FF-FF-FF-FF-
FF)
ARP 协议 在 动作
链路层: 6
ARP 表 在
示例: wants 到 发送 数据报 到 B
s MAC 地址 不 在s ARP 表, 因此 使用 ARP 到 find B’s MAC 地址
broadcasts ARP 查询, 包含 B's IP
目的地 MAC 地址 = FF
所有 节点 在 局域网 接收 ARP 查询
源 MAC:
源 IP:
Target IP 地址:
以太网 帧 (已发送 到

ARP protocol in action
Link Layer: 6-58
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC 
addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
B
replies to A with ARP 
response, giving its MAC 
address
2
Target IP address: 
137.196.7.14
Target MAC address: 
58-23-D7-FA-20-B0
...
2
ARP message into Ethernet frame 
(sent to 71-65-F7-2B-08-53)
ARP 协议 在 动作
链路层: 6
ARP 表 在
示例: wants 到 发送 数据报 到 B
s MAC 地址 不 在s ARP 表, 因此 使用 ARP 到 find B’s MAC 地址
replies 到 与 ARP
响应, giving 它的 MAC
地址
Target IP 地址:
Target MAC 地址:
ARP 报文 进入 以太网 帧
(已发送 到

ARP protocol in action
Link Layer: 6-59
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
A
receives B’s reply, adds B 
entry into its local ARP table
3
137.196.
7.14
58-23-D7-FA-20-B0
500
ARP 协议 在 动作
链路层: 6
ARP 表 在
示例: wants 到 发送 数据报 到 B
s MAC 地址 不 在s ARP 表, 因此 使用 ARP 到 find B’s MAC 地址
接收 B’s 应答, adds B
entry 进入 它的 本地 ARP 表

5-60
walkthrough: send datagram from A to B via R

focus on addressing –at IP (datagram) and MAC layer (frame)

assume A knows B’s IP address

assume A knows IP address of first hop router, R (how?)

assume A knows R’s MAC address (how?)
Addressing: routing to another LAN
R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
发送 数据报 从 到 B 通过 R
focus 在 编址在 IP (数据报) 和 MAC 层 (帧)
assume 知道 B’s IP 地址
assume 知道 IP 地址 的 第一个 跳 路由器, R (如何?)
assume 知道 R’s MAC 地址 (如何?)
编址: 路由 到 另一个 局域网

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-61
Addressing: routing to another LAN
IP
Eth
Phy
IP src: 111.111.111.111
IP dest: 222.222.222.222

A creates IP datagram with IP source A, destination B 

A creates link-layer frame with R's MAC address as dest, frame 
contains A-to-B IP datagram
MAC src: 74-29-9C-E8-FF-55
MAC dest: E6-E9-00-17-BB-4B
编址: 路由 到 另一个 局域网
创建 IP 数据报 与 IP 源 , 目的地 B
创建 链路层 帧 与R's MAC 地址 as, 帧
包含到B IP 数据报

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-62
Addressing: routing to another LAN
IP
Eth
Phy

frame sent from A to R
IP
Eth
Phy

frame received at R, datagram removed, passed up to IP
MAC src: 74-29-9C-E8-FF-55
MAC dest: E6-E9-00-17-BB-4B
IP src: 111.111.111.111
IP dest: 222.222.222.222
IP src: 111.111.111.111
IP dest: 222.222.222.222
编址: 路由 到 另一个 局域网
帧 已发送 从 到 R
帧 已收到 在 R, 数据报 removed, passed 上 到 IP

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-63
Addressing: routing to another LAN
IP src: 111.111.111.111
IP dest: 222.222.222.222

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy
IP
Eth
Phy
编址: 路由 到 另一个 局域网
R forwards 数据报 与 IP 源 , 目的地 B
R 创建 链路层 帧 与 B's MAC 地址 as, 帧
包含到B IP 数据报

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-64
Addressing: routing to another LAN

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
IP src: 111.111.111.111
IP dest: 222.222.222.222
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy
IP
Eth
Phy
编址: 路由 到 另一个 局域网
R forwards 数据报 与 IP 源 , 目的地 B
R 创建 链路层 帧 与 B's MAC 地址 as, 帧
包含到B IP 数据报

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-65
Addressing: routing to another LAN

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
IP src: 111.111.111.111
IP dest: 222.222.222.222
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy
编址: 路由 到 另一个 局域网
R forwards 数据报 与 IP 源 , 目的地 B
R 创建 链路层 帧 与 B's MAC 地址 as, 帧
包含到B IP 数据报

66
A  sends an IP to B, please describe the entire 
addressing process, including the IP and MAC. 
A
S1R1
S2
R3
S3
R5
S4
R6
B
SR
: switch: router
R2
R4
R7
R
R
发送 IP 到 B, please describe entire
编址 进程, 包括 IP 和 MAC.
: 交换机
: 路由器

5-67
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-68
Ethernet
“dominant”wired LAN technology: 

cheap $20 for NIC

first widely used LAN technology

simpler, cheaper than token LANs and ATM

kept up with speed race: 10 Mbps –10 Gbps
Metcalfe’s Ethernet sketch
以太网
有线 局域网 technology:
cheap $20 用于 NIC
第一个 widely 用于 局域网 technology
simpler, cheaper 比 token 局域网 和 ATM
kept 上 与 speed race: 10 Mbps
s 以太网 sketch

5-69
Ethernet: physical topology

bus: popular through mid 90s

all nodes in same collisiondomain (can collide with 
each other)

star: prevails today

active switchin center

each “spoke”runs a (separate) Ethernet protocol 
(nodes do NOTcollide with each other)
switch
bus: 
coaxial cable
star
以太网: 物理 拓扑
总线:
popular 通过 mid 90s
所有 节点 在 相同碰撞域 (可以 collide 与
每个 其他)
主动交换机在 中心
每个runs a (separate) 以太网 协议
(节点 做collide 与 每个 其他)
交换机
总线:
coaxial 电缆

5-70
Ethernet frame structure
sending adapter encapsulates IP datagram (or 
other network layer protocol packet) in 
Ethernet frame
preamble: 

7 bytes with pattern 10101010followed by one 
byte with pattern 10101011

used to synchronize receiver, sender clock 
rates
dest.
address
source
address
data 
(payload)
CRC
preamble
type
以太网 帧 结构
发送 适配器 encapsulates IP 数据报 (或
其他 网络层 协议 分组) 在
以太网 帧
7 字节 与 patternfollowed 由 一个
字节 与 pattern 10101011
用于 到 synchronize 接收方, 发送方 clock
速率
地址
源
地址
数据
(载荷)
类型

5-71
Ethernet frame structure (more)

addresses: 6 byte source, destination MAC 
addresses

if adapter receives frame with matching destination 
address, or with broadcast address (e.g. ARP 
packet), it passes data in frame to network layer 
protocol

otherwise, adapter discards frame

type: indicates higher layer protocol (mostly IP but 
others possible, e.g., Novell IPX, AppleTalk)

CRC: cyclic redundancy check at receiver

error detected: frame is dropped
dest.
address
source
address
data 
(payload)
CRC
preamble
type
以太网 帧 结构 (更多)
地址:6 字节 源, 目的地 MAC
地址
如果 适配器 接收 帧 与 matching 目的地
地址, 或 与广播 地址
分组), 它 passes 数据 在 帧 到 网络层
协议
otherwise, 适配器 discards 帧
类型:higher 层 协议
(mostly IP 但是
others 可能, e.g., Novell IPX, AppleTalk)
cyclic redundancy 查看 在 接收方
差错 detected: 帧 是 dropped
地址
源
地址
数据
(载荷)
类型

5-72
Ethernet: unreliable, connectionless

connectionless: no handshaking between 
sending and receiving NICs 

unreliable: receiving NIC doesn’t send Acks
or Nacksto sending NIC

data in dropped frames recovered only if initial 
sender uses higher layer rdt(e.g., TCP), 
otherwise dropped data lost

Ethernet’s MAC protocol: unslottedCSMA/CD 
wthbinary backoff
以太网: unreliable, 无连接
无连接:无 握手 之间
发送 和 接收 NICs
接收 NIC doesn’t 发送
或到 发送 NIC
数据 在 dropped 帧 recovered 仅 如果 初始
发送方 使用 higher 层
otherwise dropped 数据 丢失的
以太网’s MAC 协议:

5-73
802.3 Ethernet standards: link & physical layers

manydifferent Ethernet standards

common MAC protocol and frame format

different speeds: 2 Mbps, 10 Mbps, 100 Mbps, 
1Gbps, 10G bps

different physical layer media: fiber, cable
application
transport
network
link
physical
MAC protocol
and frame format
100BASE-TX
100BASE-T4
100BASE-FX
100BASE-T2
100BASE-SX
100BASE-BX
fiber physical layer
copper (twister
pair) physical layer
802.3 以太网 standards: 链路 & 物理 层
许多不同的 以太网 standards
common MAC 协议 和 帧 格式
不同的 speeds: 2 Mbps, 10 Mbps, 100 Mbps,
不同的 物理 层 media: fiber, 电缆
应用
运输
网络
链路
物理
MAC 协议
和 帧 格式
fiber 物理 层
pair) 物理 层

5-75
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
2023.11.26
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-76
Ethernet switch

link-layer device: takes an activerole

store, forward Ethernet frames

examine incoming frame’s MAC address, selectivelyforward  
frame to one-or-more outgoing links when frame is to be 
forwarded on segment, uses CSMA/CD to access segment

Transparent (No IP and MAC address)

hosts are unaware of presence of switches

plug-and-play, self-learning

switches do not need to be configured
以太网 交换机
链路层 设备: takes主动
store, 转发 以太网帧
examine 传入的 帧’s MAC 地址,转发
帧 到 一个或更多 outgoing 链路 当 帧 是 到 是
转发 在 报文段, 使用 CSMA/CD 到 访问 报文段
Transparent (无 IP 和 MAC 地址)
主机 是 unaware 的 presence 的 交换机
和学习
交换机 做 不 需要 到 是 configured

5-77
Switch: multiplesimultaneous transmissions

hosts have dedicated, direct connection to 
switch

switches buffer packets

Ethernet protocol used on eachincoming link, 
but no collisions; full duplex

each link is its own collision domain

switching:A-to-A’and B-to-B’can transmit 
simultaneously, without collisions 
switch with six interfaces
(1,2,3,4,5,6)
A
A’
B
B’
C
C’
1
2
3
4
5
6
交换机:多路
主机 有 dedicated, 直接 连接 到
交换机
交换机 缓冲区 分组
以太网 协议 用于 在每个传入的 链路,
但是 无 碰撞s; 完整的 duplex
每个 链路 是 它的 自己的 碰撞 域
交换:到和 B到可以 传输
, 没有 碰撞s
交换机 与 six 接口

5-78
Switch forwarding table
Q:how does switch know A’
reachable via interface 4, B’
reachable via interface 5?
switch with six interfaces
(1,2,3,4,5,6)
A
A’
B
B’
C
C’
1
2
3
4
5
6

A:each switch has a switch table,
each entry:
(MAC address of host, interface to 
reach host, time stamp)
looks like a routing table!
Q:how are entries created, 
maintained in switch table? 
something like a routing protocol?
交换机 转发表
如何 做 交换机 know
reachable 通过 接口 4, B
reachable 通过 接口 5?
交换机 与 six 接口
每个 交换机 有 a交换表,
每个 entry:
(MAC 地址 的 主机, 接口 到
reach 主机, 时间 stamp)
looks 像 a 路由表!
如何 是 entries created,
maintained 在 交换表?
something 像 a 路由 协议?

A
A’
B
B’
C
C’
1
2
3
4
5
6
5-79
Switch: self-learning

switchlearnswhich hosts can be 
reached through which interfaces

when frame received, switch 
“learns”location of sender: 
incoming LAN segment

records sender/location pair in 
switch table
A A’
Source: A
Dest: A’
MAC addr   interface    TTL
Switch table 
(initially empty)
A
1
60
交换机: self学习
交换机哪个 主机 可以 是
reached 通过 哪个 接口
当 帧 已收到, 交换机
location 的 发送方:
传入的 局域网 报文段
records 发送方/location pair 在
交换表
源:
接口
交换表
(初始时 empty)

A
A’
B
B’
C
C’
1
2
3
4
5
6
5-81
Self-learning, forwarding: example
A A’
Source: A
Dest: A’
MAC addr   interface    TTL
switch table 
(initially empty)
A1
60
A A’
A A’
A A’A A’
A A’

frame destination, A’, 
location unknown:
flood
A’A

destination A location 
known:
A’
4
60
selectively send 
on just one link
学习, 转发: 示例
源:
接口
交换表
(初始时 empty)
帧 目的地,
目的地 location
已知:
selectively 发送
在 仅 一个 链路

5-82
Interconnecting switches

switches can be connected together
Q:sending from A to G-how does S
1
know to 
forward frame destined to F via S
4
and S
3
?

A:self learning! (works exactly the same as in 
single-switch case!)
A
B
S
1
C
D
E
F
S
2
S
4
S
3
H
I
G
Interconnecting 交换机
交换机 可以 是 连接的 together
发送 从到 G如何 做 Sknow 到
转发 帧 destined 到 F 通过 S和 S
自学习! (works exactly 相同 as 在
单个
交换机 case!)

5-83
Self-learning multi-switch example
Suppose C sends frame to I, I responds to C

Q:show switch tables and packet forwarding in S
1
, S
2
, S
3
, S
4
A
B
S
1
C
D
E
F
S
2
S
4
S
3
H
I
G
学习 multi交换机 示例
Suppose C 发送 帧 到 I, I responds 到 C
show 交换表s 和 分组 转发 在 S

5-84
Institutional network
to external
network
router
IP subnet
mail server
web server
Institutional 网络
到 external
网络
路由器
IP 子网
邮件 服务器
Web 服务器

5-85
Switches vs. routers
both are store-and-forward: 
routers: network-layer devices 
(examine network-layer headers)
switches: link-layer devices (examine 
link-layer headers)
both have forwarding tables:
routers: compute tables using routing 
algorithms, IP addresses
switches: learn forwarding table using 
flooding, learning, MAC addresses 
application
transport
network
link
physical
network
link
physical
link
physical
switch
datagram
application
transport
network
link
physical
frame
frame
frame
datagram
交换机 vs. 路由器
二者 是 store和转发:
路由器:网络层 设备
(examine 网络层 首部)
交换机链路层 设备 (examine
链路层 首部)
二者 有 转发表s:
路由器:计算 表 使用 路由
算法, IP 地址
交换机:learn 转发表 使用
泛洪, 学习, MAC 地址es
应用
运输
网络
链路
物理
网络
链路
物理
链路
物理
交换机
数据报
应用
运输
网络
链路
物理
帧
帧
帧
数据报

5-86
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-87
VLANs: motivation
consider:

CS user moves office to EE, 
but wants connect to CS 
switch?

single 
broadcast domain
:
all layer-2 broadcast 
traffic(ARP, DHCP, 
unknown location of 
destination MAC 
address) must cross 
entire LAN

security/privacy, 
efficiency issues
Computer 
Science
Electrical
Engineering
Computer
Engineering
What is the difference between 
VLAN and the general LAN 
formed by router?
CS 用户 moves office 到 EE,
但是 wants 连接 到 CS
交换机?
单个
广播 域
所有 层广播
流量
的
目的地 MAC
地址) 必须
entire 局域网
安全/privacy,
效率 issues
计算机
计算机
什么 是 difference 之间
VLAN 和 general 局域网
formed 由 路由器?

Link Layer
5-88
VLANs
port-based VLAN: switch ports grouped (by 
switch management software) so that 
singlephysical switch ......
switch(es) supporting 
VLAN capabilities can 
be configured to 
define multiple virtual
LANS over single 
physical LAN 
infrastructure.
Virtual Local 
Area Network
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
Electrical Engineering
(VLAN ports 1-8)
...
1
8
2
7
9
1610
15
...
Computer Science
(VLAN ports 9-16)
... operates as multiple virtual switches
VLAN1:Port1~8
VLAN2:Port9~16
链路层
端口基于 VLAN:交换机 端口 grouped (由
交换机 管理 软件) 因此 这
单个物理 交换机 ......
交换机(
VLAN capabilities 可以
是 configured 到
define 多路
局域网 在 单个
物理 局域网
Virtual 本地
区域 网络
(VLAN 端口 1
计算机 Science
(VLAN 端口 9
(VLAN 端口 1
计算机 Science
(VLAN 端口 9
多路
virtual 交换机
端口1~8
VLAN2:端口9~16

5-89
Port-based VLAN

traffic isolation:frames to/from 
ports 1-8 can onlyreach ports 1-8

can also define VLAN based on MAC 
addresses of endpoints, rather than switch 
port

dynamic membership:ports can be 
dynamically assigned among VLANs

forwarding between VLANS: done via routing
(just as with separate switches)
in practice vendors sell combined switches plus 
routers
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
router
端口基于 VLAN
流量 isolation:帧 to/来自
端口 1
8 可以
仅reach 端口 1
可以 also define VLAN 基于 在 MAC
地址 的 endpoints, rather 比 交换机
端口
动态 membership
端口 可以 是
dynamically assigned 之间 VLANs
转发 之间 VLANS:
done 通过
路由
(仅 as 与 separate 交换机)
在 practice vendors sell combined 交换机 加
路由器
(VLAN 端口 1
计算机 Science
(VLAN 端口 9
路由器

5-90
VLANS spanning multiple switches

trunk port:carries frames between VLANS defined over multiple 
physical switches

frames forwarded within VLAN between switches can’t be vanilla 802.1 frames 
(must carry VLAN ID info)

802.1q protocol adds/removed additional header fields for frames forwarded 
between trunk ports
1
8
9
10
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
2
7
3
Ports 2,3,5 belong to EE VLAN
Ports 4,6,7,8 belong to CS VLAN
5
46
8
16
1
VLANS spanning 多路 交换机
trunk 端口:
carries 帧 之间 VLANS defined 在 多路
物理 交换机
帧 转发 在...内 VLAN 之间 交换机 可以’t 是 vanilla 802.1 帧
必须 carry VLAN ID 信息
802.1q 协议 adds/removed additional 首部 字段 用于 帧 转发
之间 trunk 端口
(VLAN 端口 1
计算机 Science
(VLAN 端口 9
端口 2,3,5 belong 到 EE VLAN
端口 4,6,7,8 belong 到 CS VLAN

5-91
type
2-byte Tag Protocol Identifier
(value: 81-00) 
Tag Control Information (12 bit VLAN ID field, 
3 bit priority field like IP TOS)
Recomputed 
CRC
802.1Q VLAN frame format
802.1 frame
802.1Q frame
dest.
address
source
address
data (payload)
CRC
preamble
dest.
address
source
address
preamble
data (payload)
CRC
type
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
router
类型
字节 Tag 协议 Identifier
(值: 81
Tag 控制 信息 (12 比特 VLAN ID 字段
3 比特 优先级 字段 像 IP TOS)
802.1Q VLAN 帧 格式
802.1 帧
802.1Q 帧
地址
源
地址
数据 (载荷)
地址
源
地址
数据 (载荷)
类型
(VLAN 端口 1
计算机 Science
(VLAN 端口 9
路由器

92
1).1主机发送普通的数据帧;
2).switch1收到此帧首先需要对其解封装，查看二层帧头部帧目的MAC地址;
3).从表中查找其目的MAC地址对应的VLANID与接收该帧的接口对应的VLANID是否相同，如果相同则找到对应的出接
口，如果不同则丢弃该帧;
4).找到出接口后，打上对应的VLAN 标签，封装成802.1Q的帧，从Trunk接口发送出去;
5).到达switch2后，解封装查看帧头部的目的MAC地址;
6).从表中查找其目的MAC地址对应的VLAN ID与接收该帧头部的VLAN ID是否匹配，如果匹配，则查找对应的出接口，如
果不同则丢弃该帧;
7).找到出接口后，封装成原始的帧，从相应端口转发出去。
同一个交换机中，端口/MAC与
VLAN是捆绑关系，因此，主机发
送帧不需要带上VLAN ID。
在跨越不同交换机时，下一个交换
机无法通过普通帧判断它所属的
VLAN ID，所以，从Trunk口发出
去前，必须插入VLAN ID。
交换机

93
HUB、2-layer switch、3-layer switch
层 交换机层 交换机

94
HUB、2-layer switch、3-layer switch
层 交换机层 交换机

95
HUB、2-layer switch、3-layer switch
层 交换机层 交换机


三层交换技术：二层交换技术+三层转发技术。解决局域网中
网段划分之后，网段中子网必须依赖路由器进行管理的局面，
解决了传统路由器低速、复杂所造成的网络瓶颈问题。
96
HUB、2-layer switch、3-layer switch
第三层交换、第四层交
换、多层交换、多层数
据包分类和路由交换机
层 交换机层 交换机

97
HUB、2-layer switch、3-layer switch
层 交换机层 交换机

98
HUB、2-layer switch、3-layer switch
层 交换机层 交换机

99
HUB、2-layer switch、3-layer switch
层 交换机层 交换机

假设两个使用IP协议的站点A、B通过三层交换机进行通信

发送站点A在开始发送时，把自己的IP地址与B站的IP地址比较
，判断B站是否与自己在同一子网内。

若目的站B与发送站A在同一子网内，则进行二层的转发。

若两个站点不在同一子网内，发送站A要向“缺省网关”发出
ARP(地址解析)封包，而“缺省网关”的IP地址其实是三层交换
机的三层交换模块。

A把数据发给三层交换机的“缺省网关”；
101
HUB、2-layer switch、3-layer switch
AB
三层
交换
层 交换机层 交换机


三层交换的“缺省网关”收到报文后，按照常规路由，寻找下一
跳出口的MAC及端口。并建立“目的IP~出口MAC~下一跳入口
MAC~端口号”信息。

交换机直接把三层的目的IP映射到二层下一跳的MAC所对应的端
口

从这以后，当A向B发送的数据包经过三层交换时，全部交给二
层交换处理，信息得以高速交换。由于仅仅在路由过程中才需要
三层处理，绝大部分数据都通过二层交换转发，因此三层交换机
的速度很快，接近二层交换机的速度。（不需要查路由表）
102
HUB、2-layer switch、3-layer switch
目的IP出口MAC下一跳入口MAC端口号
层 交换机层 交换机

5-103
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5 link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

DTE1
(3,10)-----(2,62)
(3,62)-----(1,22)
DTE3
DTE3
10
62
62
22
Step1:
Step2:
Step3:
DTE1
DTE1
DTE3
DTE3
DTE1
13
2
3
10
10
(3,10)-----(2,62)
(3,62)-----(1,22)
13
2
3
(Interface #, VC #)
104
The VC mode
实质：电路交换与分
组交换的结合体！
电路交换：一次寻路
，预留资源。
缺点：利用率低
分组交换：每次寻路
，利用率高。
缺点：速度低
2023.12.4
步骤1:
步骤2:
步骤3:
(接口 #, VC #)

5-105
Multiprotocol label switching (MPLS)

initial goal: high-speed IP forwarding using fixed length 
label (instead of IP address) 

fast lookup using fixed length identifier (rather than shortest prefix 
matching)

borrowing ideas from Virtual Circuit (VC) approach

but IP datagram still keeps IP address!
PPP or Ethernet 
header
IP header
remainder of link-layer frame
MPLS header
label
Exp
S
TTL
20
3
1
5
Exp―优先级。
S ―栈底。标签栈中最后进入的标签
位置，s值为0。S值为1表明此为最底
层标签。正因为这个字段表明了
MPLS的标签理论上可以无限嵌套，从
而提供无限的业务支持能力。
Multiprotocol 标签 交换 (MPLS)
初始 目标: highspeed IP 转发 使用 fixed 长度
标签 (instead 的 IP 地址
快速 lookup 使用 fixed 长度 identifier (rather 比
borrowing ideas 从 Virtual 电路 (VC) 方法
但是 IP 数据报 仍然 keeps IP 地址!
PPP 或 以太网
首部
IP 首部
remainder 的 链路
层 帧
MPLS 首部
标签

5-106
MPLS capable routers

a.k.a. label-switched router

forward packets to outgoing interface based only on label 
value (don’t inspect IP address)

MPLS forwarding table distinct from IP forwarding tables

flexibility:  MPLS forwarding decisions can differfrom those 
of IP

use destination andsource addresses to route flows to same 
destination differently (traffic engineering)

re-route flows quickly if link fails: pre-computed backup paths 
(useful for VoIP)
MPLS capable 路由器
a.k.a. 标签交换式 路由器
转发 分组 到 outgoing 接口 基于仅 在 标签
值 (t inspect IP 地址
MPLS 转发表 distinct 从 IP 转发表s
MPLS 转发 decisions 可以从 those
的 IP
使用 目的地和源 地址到 路由 flows 到 相同
目的地 differently (流量 engineering)
路由 flows 快速地 如果 链路 fails: pre
计算得到的 backup 路径
(有用 用于 VoIP)

5-107
R2
D
R3
R5
A
R6
MPLS versus IP paths
IP router

IP routing: path to destination 
determined by destination address 
alone
R4
MPLS 与 IP 路径
IP 路由器
IP 路由路径 到 目的地
determined 由 目的地 地址

5-108
R2
D
R3
R4
R5
A
R6
MPLS versus IP paths
IP-only
router

IP routing: path to destination determined by 
destination address alone
MPLS and 
IP router

MPLS routing: path to destination can be based on 
source and dest. address
fast reroute: precompute backup routes in case of link 
failure
entry router (R4)  can use differentMPLS 
routes to A based, e.g., on source address
(R5, A)->Label 1;   (R6, A)-> Label 2
MPLS 与 IP 路径
仅
路由器
IP 路由:路径 到 目的地 determined由
目的地 地址 alone
MPLS 和
IP 路由器
MPLS 路由:路径 到 目的地 可以 是 基于 在
源 和. 地址
快速 reroute:
precompute backup 路由 在 case 的 链路
entry 路由器 (R4) 可以 使用
不同的
路由 到 基于, e.g., 在 源地址
> 标签 2

5-109
MPLS signaling

modify OSPF link-state flooding protocols to carry info 
used by MPLS routing, 

e.g., link bandwidth, amount of “reserved”link bandwidth
D
R4
R5
A
R6

entry MPLS router uses RSVP-TE signaling protocol to set up MPLS 
forwarding at downstream routers
modified 
link state 
flooding
RSVP-TE
修改OSPF 链路状态泛洪 协议 到 carry 信息
用于 由 MPLS 路由,
e.g., 链路 带宽, 数量 的链路 带宽
entry MPLS 路由器 使用 RSVP
TE signaling 协议 到 集合 上 MPLS
转发 在 downstream 路由器
链路状态
泛洪

5-110
R1
R2
D
R3
R4
R5
0
1
0
0
A
R6
in         out                 out
label     label   dest    interface
6        -A       0
in         out                 out
label     label   dest    interface
10      6      A       1
12      9      D       0
in         out                 out
label     labeldestinterface
10      A       0
12      D       0
1
in         out                 out
label     label   dest    interface
8        6      A       0
0
8      A       1
MPLS forwarding tables
入口路由器负责加首个标签
出口路由器负责清理标签
在
查看查看
标签标签接口
在
查看查看
标签标签接口
在
查看
查看
标签标签接口
在
查看查看
标签标签接口
MPLS 转发表s

111
IP网络的逐跳式分组转发
匹配查找匹配查找匹配查找匹配查找匹配查找
IP分组
路由器路由器路由器路由器路由器路由器
IP分组
IP分组IP分组IP分组IP分组IP分组
匹配查找
IP分组IP分组
路由器
下一跳中继
匹配查找

112
LER0LSR0LSR1
LSR2
LER1
REQ（65.8)
LABEL(5)
下一跳
入端口
出端口
65.8
LSR0
0
0
101
2
0
1
0
1
0
1
2
目的地址前缀
65.8
65.8
65.8
LSR2
LER1
----
0
2
01
0
----
1
路由表
LER0
LSR0
LSR2
LER1
下一跳
入端口出端口
65.8
LSR0
0
目的地址前缀
65.8
65.8
65.8
LSR2
LER1
----
0
2
01
0
----
1
LER0
LSR0
LSR2
LER1
入标记出标记
---
5
5
8
82
2
---
标记信息表
Step1: routing for the path;
Step2: labeling each link;
Step3: forwarding based on label
Replace the global routing table with the 
locallabel switching table
路由协议
路由表
MPLS路由控制
转发信息表
IPLabel
数据
步骤1: 路由 用于 路径;
步骤2: labeling 每个 链路;
步骤3: 转发 基于 在 标签
全局路由表 与
本地标签 交换 表
标签

113
MPLS网络结构
LER
LER
LER
LER
LER
LER
LER
LSRLSR
LSR
LSR
标记交换路径
LSP
IP
IPLABEL
IPLABEL
IPLABEL
IP
标记分布协议
LDP
标记分布协议
LDP
标记分布协议
LDP
路由协议
路由表
MPLS路由控制
转发信息表

5-114
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6 data center networking
6.7a day in the life of a web 
request
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-115
Data center networks 

10’s to 100’s of thousands of hosts, often closely coupled, 
in close proximity:

e-business(e.g. Amazon)

content-servers(e.g., YouTube, Akamai, Apple, Microsoft)

search engines, data mining (e.g., Google)

challenges:
multiple applications, each serving 
massive numbers of clients 
managing/balancing load, avoiding 
processing, networking, data bottlenecks  
Inside a 40-ft Microsoft container, 
Chicago data center
数据 中心 网络
s 到 100s 的 thousands 的 主机, 经常 closely coupled,
在 关闭 proximity:
内容服务器
, 数据 mining (e.g., Google)
多路 应用, 每个 serving
massive 编号 的 客户
managing/balancing 负载, avoiding
processing, 网络, 数据 bottlenecks
Chicago 数据 中心

Datacenter networks: network elements
Link Layer: 6-116
Server racks
20-40 server blades: hosts 
Top of Rack (TOR) switch
one per rack
40-100Gbps Ethernet to blades      
Tier-2 switches
connecting to  ~16 TORs below
Tier-1 switches
connecting to  ~16 T-2s below
Border routers
connections outside datacenter
...
...
...
...
...
...
...
...
Datacenter 网络: 网络 elements
链路层: 6
服务器 racks
40 服务器 blades: 主机
Top 的 Rack (TOR) 交换机
一个 每 rack
100Gbps 以太网 到 blades
2 交换机
connecting 到 ~16 TORs below
1 交换机
connecting 到 ~16 T
Border 路由器
连接 outside datacenter

Datacenter networks: network elements
Link Layer: 6-117
Facebook F16 data center network topology:
https://engineering.fb.com/data-center-engineering/f16-minipack/    (posted 3/2019)
Datacenter 网络: 网络 elements
链路层: 6
Facebook F16 数据 中心 网络 拓扑:
中心

Datacenter networks: multipath
Link Layer: 6-118
910
11
12
13
14
15
16
two disjointpaths highlighted between racks 1 and 11
rich interconnection among switches, racks:
•increased throughput between racks (multiple routing paths possible)
•increased reliability via redundancy
Datacenter 网络: multipath
链路层: 6
两个
路径 highlighted 之间 racks 1 和 11
rich interconnection 之间 交换机, racks:
increased 吞吐量 之间 racks (多路 路由 路径 可能)
increased reliability 通过 redundancy

...
...
...
...
...
...
...
...
Datacenter networks: application-layer routing
Link Layer: 6-119
Load 
balancer
Internet
load balancer: 
application-layer 
routing
receives external 
client requests
directs workload 
within data center
returns results to 
external client 
(hiding data center 
internals from client)
Datacenter 网络: 应用层 路由
链路层: 6
负载
负载 balancer:
应用层
路由
接收 external
客户 请求
在...内 数据 中心
returns results 到
external 客户
(hiding 数据 中心
internals 从 客户)


link layer: 

RoCE: remote DMA (RDMA) over Converged Ethernet

transport layer: 

ECN (explicit congestion notification) used in transport-layer 
congestion control (DCTCP, DCQCN)

experimentation with hop-by-hop (backpressure) congestion 
control

routing, management:

SDN widely used within/among organizations’ datacenters

place related services, data as close as possible (e.g., in same 
rack or nearby rack) to minimize tier-2, tier-1 communication
Datacenter networks: protocol innovations
Link Layer: 6-120
链路层:
: 远程 DMA (RDMA) 在 Converged 以太网
运输层:
ECN (explicit 拥塞 notification) 用于 在 运输层
拥塞控制 (DCTCP, DCQCN)
experimentation 与 跳由跳 (backpressure) 拥塞
控制
路由, 管理
SDN widely 用于 within/among organizations’ datacenters
place 相关 服务, 数据 as 关闭 as 可能 (e.g., 在 相同
rack 或 nearby rack) 到 minimize tier
1 通信
Datacenter 网络: 协议 innovations
链路层: 6

5-121
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7 a day in the life of a web 
request（DIY）
链路层, 局域网: 提纲
引言, 服务
差错检测, 纠正
多路访问协议
局域网
编址, ARP
以太网
交换机
链路虚拟化: MPLS
数据中心网络
a 天 在 生存期 的 a Web
请求

5-122
Synthesis: a day in the life of a web request

journey down protocol stack complete!

application, transport, network, link

putting-it-all-together: synthesis!

goal:identify, review, understand protocols 
(at all layers) involved in seemingly simple 
scenario: requesting www page

scenario:student attaches laptop to 
campus network, requests/receives 
www.google.com
a 天 在 生存期 的 a Web 请求
journey down 协议 stack 完整的!
应用, 运输, 网络, 链路
它
所有
目标:identify, review, understand 协议
(在 所有 层) involved 在 seemingly simple
场景: requesting 网站 页面
场景:student attaches laptop 到
campus 网络, requests/receives

5-123
A day in the life: scenario
Comcast network 
68.80.0.0/13
Google’s network 
64.233.160.0/19 
64.233.169.105
web server
DNS server
school network 
68.80.2.0/24
web page
browser
天 在 生存期:场景
Comcast 网络
s 网络
Web 服务器
DNS 服务器
school 网络
Web 页面

router
(runs DHCP)
5-124
A day in the life... connecting to the Internet

connecting laptop needs to 
get its own IP address, addr
of first-hop router, addrof 
DNS server: use DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP

DHCP request encapsulated in UDP, 
encapsulated in IP, encapsulated in 
802.3 Ethernet

Ethernet frame broadcast(dest: 
FFFFFFFFFFFF) on LAN, received at 
router running DHCP server

Ethernet demuxedto IP demuxed, 
UDP demuxedto DHCP 
路由器
天 在 生存期...connecting 到 Internet
connecting laptop 需要 到
获得 它的 自己的 IP 地址,
的 第一个
跳 路由器,的
DNS 服务器: 使用
DHCP 请求
在
encapsulated 在
, encapsulated 在
以太网
以太网 帧广播
FFFFFFFFFFFF) 在 局域网, 已收到 在
路由器 运行
服务器
以太网
到 IP
到 DHCP

router
(runs DHCP)
5-125

DHCP server formulates 
DHCP ACKcontaining 
client’s IP address, IP 
address of first-hop router 
for client, name & IP address 
of DNS server
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP

encapsulation at DHCP server, 
frame forwarded (switch learning) 
through LAN, demultiplexingat 
client
Client now has IP address, knows name & addrof DNS 
server, IP address of its first-hop router

DHCP client receives DHCP ACK 
reply
A day in the life... connecting to the Internet
路由器
DHCP 服务器 formulates
包含
客户s IP 地址, IP
地址 的 第一个跳 路由器
用于 客户, 名称 & IP 地址
的 DNS 服务器
encapsulation 在 DHCP 服务器,
帧 转发 (交换机 学习
通过 局域网,多路分解在
客户
客户 现在 有 IP 地址, 知道 名称 &的 DNS
服务器, IP 地址 的 它的 第一个
跳 路由器
DHCP 客户 接收 DHCP ACK
应答
天 在 生存期...connecting 到 Internet

router
(runs DHCP)
5-126
A day in the life... ARP
(before DNS, before HTTP)

before sending HTTP request, 
need IP address of 
www.google.com:  DNS
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS

DNS query created, encapsulated in 
UDP, encapsulated in IP, encapsulated 
in Eth.  To send frame to router, need 
MAC address of router interface: 
ARP

ARP query broadcast, received by 
router, which replies with ARP reply 
giving MAC address of router 
interface

client now knows MAC address of 
first hop router, so can now send 
frame containing DNS query 
ARP query
Eth
Phy
ARP
ARP
ARP reply
路由器
天 在 生存期...
(之前 DNS, 之前 HTTP)
之前 发送请求,
需要 IP 地址 的
DNS 查询 created, encapsulated 在
UDP, encapsulated 在 IP, encapsulated
在 Eth. 到 发送 帧 到 路由器, 需要
MAC 地址 的 路由器 接口:
ARP 查询广播, 已收到 由
路由器, 哪个 replies 与
ARP 应答
giving MAC 地址 的 路由器
接口
客户 现在 知道 MAC 地址 的
第一个 跳 路由器, 因此 可以 现在 发送
帧 包含 DNS 查询
ARP 查询
ARP 应答

router
(runs DHCP)
5-127
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS
DNS
DNS

IP datagram containing DNS 
query forwarded via LAN 
switch from client to 1
st
hop 
router

IP datagram forwarded from 
campus network into comcast
network, routed (tables created by 
RIP, OSPF, IS-IS and/or BGP
routing protocols) to DNS server

Demux’edto DNS server

DNS server replies to client with IP 
address of www.google.com
Comcast network 
68.80.0.0/13
DNS server
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS
DNS
A day in the life... using DNS
路由器
IP 数据报 包含 DNS
查询 转发 通过 局域网
交换机 从 客户 到 1跳
路由器
IP 数据报 转发 从
campus 网络 进入
网络, routed (表 created 由
路由协议) 到 DNS 服务器
到 DNS 服务器
DNS 服务器 replies 到 客户 与 IP
地址 的
Comcast 网络
DNS 服务器
天 在 生存期...使用 DNS

router
(runs DHCP)
5-128
A day in the life...TCP connection carrying HTTP
HTTP
TCP
IP
Eth
Phy
HTTP

to send HTTP request, 
client first opens TCP 
socketto web server

TCP SYN segment (step 1 in 3-
way handshake) inter-domain 
routed to web server

TCP connection established!
64.233.169.105
web server
SYN
SYN
SYN
SYN
TCP
IP
Eth
Phy
SYN
SYN
SYN
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK

web server responds with TCP 
SYNACK (step 2 in 3-way 
handshake)
路由器
天 在 生存期...TCP 连接 carrying HTTP
到 发送 HTTP 请求,
客户 第一个 opens
套接字
到 Web 服务器
SYN 报文段
(步骤 1 在 3
way 握手) 之间的域
routed 到 Web 服务器
连接 established!
Web 服务器
Web 服务器 responds 与
(步骤 2 在 3
握手)

router
(runs DHCP)
5-129
A day in the life... HTTP request/reply
HTTP
TCP
IP
Eth
Phy
HTTP

HTTP request sent into 
TCP socket

IP datagram containing HTTP 
request routed to 
www.google.com

IP datagram containing HTTP 
reply routed back to client
64.233.169.105
web server
HTTP
TCP
IP
Eth
Phy

web server responds with 
HTTP reply (containing web 
page)
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP

web page finally (!!!) displayed
路由器
天 在 生存期...
HTTP 请求
已发送 进入
TCP 套接字
IP 数据报 包含 HTTP
请求 routed 到
IP 数据报 包含 HTTP
应答 routed back 到 客户
Web 服务器
Web 服务器 responds 与
HTTP 应答(包含 Web
页面)
Web 页面

5-130
Chapter 6: Summary

principles behind data link layer services:

error detection, correction

sharing a broadcast channel: multiple access

link layer addressing

instantiation and implementation of various 
link layer technologies

Ethernet

switched LANS, VLANs

virtualized networks as a link layer: MPLS

synthesis: a day in the life of a web request
本章总结
原理 背后的 数据 链路层 服务:
差错检测, 纠正
sharing a 广播 信道: 多路访问
链路层 编址
实例化 和 实现 的 various
链路层 technologies
以太网
交换式 局域网, VLANs
virtualized 网络 as a 链路层: MPLS
synthesis: a 天 在 生存期 的 a Web 请求

5-131
Chapter 6: let’s take a breath

journey down protocol stack complete(except PHY)

solid understanding of networking principles, practice

..... could stop here .... but lotsof interesting topics!

wireless

multimedia

security 
The End of Chapter 6
本章
journey down 协议 stack完整的
solid understanding 的 网络 原理, practice
..... 可以 stop here .... 但是的 interesting topics!
无线
安全
端 的 第 6 章

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
