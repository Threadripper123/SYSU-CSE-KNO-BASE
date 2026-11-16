# Chapter7 Wireless and Mobile Networks.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter7 Wireless and Mobile Networks.zh-CN.pdf　｜　24,317 字符　｜　52 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
计算机网络
2024. 秋季

7-2
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
Chapter 7
Wireless and
Mobile Networks
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)

If you post any slides on a www  site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
计算机 网络:
Down 方法
第 7 章
无线 和
移动网络s
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
谢谢 和 使用愉快! JFK/KWR
所有 材料 版权 1996
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved

Homework(ver.8, CN)

1, 5, 8, 11, 12

Keywords: CDMA, CDMA encode/decode, CSMA/CA, 
mobile IP, 
作业
关键词： CDMA, CDMA
移动 IP,

Ch. 7: Wireless and Mobile Networks
Background:

# wireless (mobile) phone subscribers now exceeds # wired phone 
subscribers (5-to-1)!

# wireless Internet-connected devices equals # wirelineInternet-connected 
devices

laptops, Internet-enabled phones promise anytime untethered Internet access

two important (but different) challenges

wireless:communication over wireless link

mobility:handling the mobile user who changes point of attachment to network
6-4
: 无线 和 移动网络s
# 无线 (移动) phone subscribers 现在 exceeds # 有线 phone
到
# 无线 Internet连接的 设备 equals #连接的
设备
enabled phones promise anytime untethered Internet 访问
两个 重要 (但是 不同的) challenges
无线:通信 在 无线链路
移动性:处理 移动用户 who 变化 点 的 attachment 到 网络

Chapter 7outline
7.1 Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and routing 
to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer protocols
7.9Summary
6-5
本章提纲
7.1 引言
无线
无线链路, 特性
IEEE 802.11 无线 局域网
蜂窝 Internet 访问
体系结构
移动性
原理: 编址 和 路由
到 移动用户s
移动 IP
处理 移动性 在 蜂窝
网络
移动性 和 higher层 协议
总结

Elements of a wireless network
6-6
network 
infrastructure
Elements 的 a 无线网络
网络
基础设施

Elements of a wireless network
6-7
wireless hosts

laptop, smartphone

run applications

may be stationary (non-
mobile) or mobile
wireless does notalways 
mean mobility
network 
infrastructure
Elements 的 a 无线网络
无线 主机
运行 应用
可能 是 stationary (non
移动) 或 移动
无线 做不总是
mean 移动性
网络
基础设施

Elements of a wireless network
6-8
base station

typically connected to 
wired network

relay -responsible for 
sending packets between 
wired network and 
wireless host(s) in its 
“area”
e.g., cell towers,  
802.11 access points 
network 
infrastructure
Elements 的 a 无线网络
基站
通常 连接的 到
有线 网络
responsible 用于
发送 分组 之间
有线 网络 和
无线 主机(s) 在 它的
区域
e.g., 小区 towers,
802.11 接入点s
网络
基础设施

Elements of a wireless network
6-9
wireless link

typically used to connect 
mobile(s) to base station

also used as backbone 
link 
multiple access protocol 
coordinates link access 

various data rates, 
transmission distance
network 
infrastructure
Elements 的 a 无线网络
无线链路
通常 用于 到 连接
移动(s) 到 基站
also 用于 as 骨干
链路
多路访问协议
coordinates 链路 访问
various 数据 速率,
传输 距离
网络
基础设施

Characteristics of selected wireless links
W ireless and Mobile  Networks:  7-10
IndoorOutdoorMidrange
outdoor
Long range
outdoor
10-30m
50-200m
200m-4Km4Km-15Km
2 Mbps
4G LTE
802.11ac
802.11n
802.11g
802.11b
3.5 Gbps
600 Mbps
54 Mbps
11 Mbps
Bluetooth
802.11ax
14 Gbps
5G
10 Gbps
802.11 af,ah
特性 的 selected 无线链路
无线 和 移动网络s: 7

11
中国的5G频谱规划

Elements of a wireless network
6-12
infrastructure mode

base station connects 
mobiles into wired 
network

handoff: mobile changes 
base station providing 
connection into wired 
network
network 
infrastructure
Elements 的 a 无线网络
基础设施 mode
基站 连接
mobiles 进入 有线
网络
切换: 移动 变化
基站 提供
连接 进入 有线
网络
网络
基础设施

Elements of a wireless network
6-13
ad hoc mode

no base stations

nodes can only 
transmit to other 
nodes within link 
coverage

nodes organize 
themselves into a 
network: route 
among themselves
Elements 的 a 无线网络
无 基站s
节点 可以 仅
传输 到 其他
节点 在...内 链路
节点 organize
themselves 进入 a
网络: 路由
之间 themselves

Wireless network taxonomy
6-14
single hop
multiple hops
infrastructure
(e.g., APs)
no
Infrastructure
(No upper network )
host connects to 
base station (WiFi,
WiMAX, cellular) 
which connects to 
larger Internet
no base station, no
connection to larger 
Internet (Bluetooth, 
ad hoc nets)
host may have to
relay through several
wireless nodes to 
connect to larger 
Internet: mesh net
no base station, no
connection to larger 
Internet. May have to
relay to reach other 
a given wireless node
MANET, VANET
无线网络 taxonomy
单个 跳
多路 hops
基础设施
无
基础设施
(无 upper 网络 )
主机 连接 到
基站 (WiFi,
WiMAX, 蜂窝)
哪个 连接 到
无 基站, 无
连接 到 larger
主机 可能 有 到
relay 通过 several
无线 节点到
连接 到 larger
mesh 网络
无 基站, 无
连接 到 larger
Internet. 可能 有 到
relay 到 reach 其他
a 给定的 无线 节点

Chapter 7 outline
7.1Introduction 
Wireless
7.2 Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and 
routing to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer 
protocols
7.9Summary
6-15
第 7 章：提纲
引言
无线
7.2 无线链路, 特性
IEEE 802.11 无线 局域网
蜂窝 Internet 访问
体系结构
移动性
原理: 编址 和
路由 到 移动用户s
移动 IP
处理 移动性 在 蜂窝
网络
移动性 和 higher层
协议
总结

Wireless Link Characteristics (1)
important differences from wired link ....

decreased signal strength: radio signal attenuates as it 
propagates through matter (path loss)

interference from other sources: standardized wireless 
network frequencies (e.g., 2.4 GHz) shared by other 
devices (e.g., phone); devices (motors) interfere as 
well

multipath propagation: radio signal reflects off 
objects ground, arriving ad destination at slightly 
different times
.... make communication across (even a point to point) 
wireless link much more “difficult”
6-16
无线链路 特性 (1)
重要differences 从 有线 链路 ....
decreased 信号强度:radio 信号 attenuates as 它
propagates 通过 matter (路径损耗)
干扰 从 其他 sources:standardized 无线
网络 frequencies (e.g., 2.4 GHz) 共享 由 其他
设备 (e.g., phone); 设备 (motors) interfere as
多径传播:
radio 信号 reflects off
对象 ground, 到达的 ad 目的地 在 slightly
不同的 times
.... 生成 通信 across (even a 点 到 点)
无线链路 much 更多

Wireless Link Characteristics (2)

SNR: signal-to-noise ratio

larger SNR –easier to extract signal from 
noise (a “good thing”)

SNR versus BER tradeoffs

given physical layer:increase power -> increase 
SNR->decrease BER

given SNR:choose physical layer that meets 
BER requirement, giving highest thruput

SNR may change with mobility: 
dynamically adapt physical layer 
(modulation technique, rate) 
6-17
1020
3040
QAM256 (8 Mbps)
QAM16 (4 Mbps)
BPSK (1 Mbps)
SNR(dB)
BER
10
-1
10
-2
10
-3
10
-5
10
-6
10
-7
10
-4
无线链路 特性 (2)
SNR: 信号到
easier 到 extract 信号 从
好的 thing
SNR 与 BER tradeoffs
给定的 物理 层:增大 power> 增大
给定的 SNR:
选择 物理 层 这 meets
SNR 可能 变化 与 移动性:
dynamically adapt 物理 层
(modulation technique, 速率)

BER
Wireless network characteristics
Multiple wireless senders and receivers create 
additional problems (beyond multiple access):
6-18
A
B
C
Hidden terminal problem

B, A hear each other

B, C hear each other

A, C can not hear each other
means A, C unaware of their 
interference at B
A
B
C
A’s signal
strength
space
C’s signal
strength
Signal attenuation:

B, A hear each other

B, C hear each other

A, C can not hear each other 
interfering at B
无线网络 特性
多路 无线 senders 和 receivers create
additional problems (beyond 多路访问):
隐藏终端 问题
hear 每个 其他
B, C hear 每个 其他
, C 可以 不 hear 每个 其他
means , C unaware 的 它们的
干扰 在 B
s 信号
强度
空间
s 信号
强度
信号 attenuation:
hear 每个 其他
B, C hear 每个 其他
, C 可以 不 hear 每个 其他
interfering 在 B

BER
Code Division Multiple Access (CDMA)

unique “code”assigned to each user; i.e., code set 
partitioning

all users share same frequency, but each user has own 
“chipping”sequence (i.e., code) to encode data

allows multiple users to “coexist”and transmit 
simultaneously with minimal interference (if codes are 
“orthogonal”)

encoded signal= (original data) X (chipping sequence)

decoding:inner-product of encoded signal and chipping 
sequence
6-19
码 划分 多路访问 (CDMA)
码
assigned 到 每个 用户; i.e., 码 集合
划分
所有 用户
share 相同 频率
, 但是 每个 用户 有 自己的
序列 (i.e., 码) 到 encode 数据
允许 多路 用户 到和 传输
simultaneously 与 minimal 干扰 (如果 codes 是
encoded 信号= (原始 数据) X (chipping 序列)
product 的 encoded 信号 和 chipping
序列

BER
Code Division Multiple Access (CDMA)
6-20
A
B
C
D
A发送数据d
A
, A的CDMA地址码c
A
B发送数据d
B
, B的CDMA地址码c
B
c
A
与c
B
正交：
c
A
⊙c
B
=0；
c
A
⊙c
A
=1；
c
B
⊙c
B
=1；
发送数据：
A: d
A
⋅c
A
；
B: d
B
⋅c
B
；
A+B: d
A
⋅c
A
+ d
B
⋅c
B
接收数据：
C：(d
A
⋅c
A
+ d
B
⋅c
B
) ⊙c
A
= d
A
D：(d
A
⋅c
A
+ d
B
⋅c
B
) ⊙c
B
= d
B
d
A
d
B
d
A
d
B
+
20241223
码 划分 多路访问 (CDMA)

CDMA encode/decode
6-21
slot 1
slot 0
d
1
= -1
111
1
1
-
1
-
1
-1
-
Z
i,m
= d
i
.
c
m
d
0
= 1
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
slot 0
channel
output
slot 1
channel
output
channel output Z
i,m
sender
code
data
bits
slot 1
slot 0
d
1
= -1
d
0
= 1
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
slot 0
channel
output
slot 1
channel
output
receiver
code
received
input
D
i 
= 
S
Z
i,m
.
c
m
m=1
M
M
时隙 1
时隙 0
时隙 0
信道
输出
时隙 1
信道
输出
信道 输出 Z
发送方
码
数据
比特
时隙 1
时隙 0
时隙 0
信道
输出
时隙 1
信道
输出
接收方
码
已收到
输入

CDMA: two-sender interference
6-22
using same code as 
sender 1, receiver recovers 
sender 1’s original data 
from summed channel 
data!
Sender 1
Sender 2
channel sums together 
transmissions by sender 1 
and 2
1，1，1，-1，1，-1，-1，-1

1，-1，1，1，1，-1，1，1
0
CDMA: 两个发送方 干扰
使用 相同 码 as
发送方 1, 接收方 recovers
发送方 1
s 原始 数据
从 summed 信道
数据!
发送方 1
发送方 2
信道 sums together
transmissions 由 发送方 1
和 2

Chapter 6 outline
7.1Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and routing 
to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer protocols
7.9Summary
6-23
第 6 章：提纲
引言
无线
无线链路, 特性
7.3 IEEE 802.11 无线 局域网
蜂窝 Internet 访问
体系结构
移动性
原理: 编址 和 路由
到 移动用户s
移动 IP
处理 移动性 在 蜂窝
网络
移动性 和 higher层 协议
总结

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-24
all use CSMA/CA for multiple access, and have base-station and ad-hoc network versions
IEEE 802.11 无线 局域网
无线 和 移动网络s: 7
所有 使用 CSMA/CA 用于 多路访问, 和 有 基站 和 adhoc 网络 versions

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-25
802.11n (WiFi4)
802.11ac(WiFi5)
IEEE 802.11 无线 局域网
无线 和 移动网络s: 7

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-26
802.11ax (WiFi6)802.11be (WiFi7)
IEEE 802.11 无线 局域网
无线 和 移动网络s: 7
802.11是 (WiFi7)

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-27
OFDM
IEEE 802.11 无线 局域网
无线 和 移动网络s: 7

802.11 LAN architecture
6-29

wireless host communicates with base 
station
base station = access point (AP)

Basic Service Set (BSS) (aka “cell”) 
in infrastructure mode contains:
wireless hosts
access point (AP): base station
ad hoc mode: hosts only
BSS 1
BSS 2
Internet
hub, switch
or router
802.11 局域网 体系结构
无线 主机 communicates 与 基
站
基站 = 接入点 (AP)
Basic 服务 集合 (BSS)
小区
在 基础设施 mode 包含:
无线 主机
接入点 (AP): 基站
ad hoc mode: 主机 仅
hub, 交换机
或 路由器

802.11: Channels, association

802.11b: 2.4GHz-2.485GHz spectrum divided into 
11
channels at different frequencies

AP admin chooses frequency for AP

interference possible: channel can be same as 
that chosen by neighboring AP!

host: must associatewith an AP

scans channels, listening for beacon frames
containing AP’s name (SSID) and MAC address

selects AP to associate with

may perform authentication [Chapter 8]

will typically run DHCP to get IP address in AP’s 
subnet
6-30
BSS
802.11: 信道, association
2.485GHz 频谱 divided 进入
信道 在 不同的 frequencies
AP admin chooses 频率 用于 AP
干扰 可能: 信道 可以 是 相同 as
这 chosen由 neighboring AP
主机: 必须与 AP
scans 信道, listening 用于beacon 帧
包含 AP
’s 名称 (
) 和 MAC 地址
selects AP 到 associate 与
可能 perform authentication [第 8 章]
将 通常 运行 DHCP 到 获得 IP 地址 在 AP
子网

802.11: passive/active scanning
6-31
AP 2
AP 1
H1
BBS 2
BBS 1
1
2
3
1
passive scanning:
(1)beacon frames sent from APs
(2)association Request frame sent: H1 to selected AP 
(3)association Response frame sent from  selected AP 
to H1
AP 2
AP 1
H1
BBS 2
BBS 1
1
2
2
3
4
active  scanning: 
(1)Probe Request frame broadcast from H1
(2)Probe Response frames sent from APs
(3)Association Request frame sent: H1 to selected AP 
(4)Association Response frame sent from selected AP 
to H1
802.11: 被动/主动扫描
被动扫描:
beacon 帧 已发送 从 APs
association 请求 帧 已发送: H1 到 selected AP
association 响应 帧 已发送 从 selected AP
到 H1
主动扫描
Probe 请求 帧 广播 从 H1
Probe 响应 帧 已发送 从 APs
Association 请求 帧 已发送: H1 到 selected AP
Association 响应 帧 已发送 从 selected AP
到 H1

IEEE 802.11: multiple access

avoid collisions: 2
+
nodes transmitting at same time

802.11: CSMA -sense before transmitting

Don’t collide with ongoing transmission by other node

802.11: nocollision detection!

difficult to receive (sense collisions) when transmitting due to weak received signals 
(fading)

Can’t sense all collisions in any case: hidden terminal, fading

goal: avoid collisions:CSMA/C(ollision)A(voidance)
6-32
space
A
B
C
A
B
C
A’s signal
strength
C’s signal
strength
IEEE 802.11: 多路访问
avoid 碰撞s: 2节点transmitting 在 相同 时间
sense 之前 transmitting
’t collide 与 ongoing 传输 由 其他 节点
无碰撞 检测!
difficult 到 接收 (sense 碰撞s) 当 transmitting 由于 到 weak 已收到 signals
可以’t sense 所有 碰撞s 在 任何 case: 隐藏终端, fading
目标:avoid 碰撞s
空间
s 信号
强度
s 信号
强度

IEEE 802.11 MAC Protocol: CSMA/CA
802.11 sender
1 
if sense channel idlefor DIFS  then
transmit entire frame (no CD)
2 if sense channel busy then
start random backofftime
timer counts down while channel idle
transmit when timer expires
if no ACK, increase random backoffinterval, repeat 2
802.11 receiver
-
if frame received OK
return ACK after SIFS (ACK needed due to hidden 
terminal problem) 
6-33
sender
receiver
DIFS
data
SIFS
ACK
Short InterframeSpace（SIFS）
DCF InterframeSpace(DIFS):在DCF协议中，节点在开始发送数据
之前需要监测信道是否空闲。如果信道已经空闲，则节点仍需等待
DIFS段时间才开始发送数据；而如果在DIFS时间段内任一时刻信道
被监测为忙，则节点不得不推迟它的数据发送。
IEEE 802.11 MAC 协议: CSMA/CA
802.11 发送方
如果 sense 信道 idle
用于 DIFS
则
传输 entire 帧 (无 CD
2 如果 sense 信道 busy 则
开始 随机时间
计时器 counts down while 信道 idle
传输 当 计时器 expires
如果 无 ACK, 增大 随机
802.11 接收方
如果 帧 已收到 OK
返回 ACK 之后 SIFS (由于 到 隐藏
终端 问题)
发送方
接收方
数据
之间的帧空间
之间的帧空间(DIFS):

Avoiding collisions (more)
idea:allow sender to “
reserve
”channel rather than random access of 
data frames: avoid  collisions of long  data frames

sender first transmits 
small
request-to-send (RTS) packets to BS using 
CSMA

RTSs may still collide with each other (but they’re short)

BS broadcasts clear-to-send CTS in response to RTS

CTS heard by all nodes

sender transmits data frame

other stations defer transmissions 
6-34
avoid data frame collisions completely 
using small reservation packets!
Avoiding 碰撞s (更多)
思想:allow 发送方 到信道 rather 比 随机访问 的
数据 帧: avoid 碰撞s 的 long 数据 帧
发送方 第一个 传输请求到发送(RTS) 分组 到 BS 使用
RTSs 可能 仍然 collide 与 每个 其他 (但是 they
到发送 CTS在 响应 到 RTS
CTS heard 由 所有 节点
发送方 传输 数据 帧
其他 stations defer transmissions
avoid 数据 帧 碰撞s completely
使用 small reservation 分组!

Collision Avoidance: RTS-CTS exchange
6-35
AP
AB
time
DATA (A)
reservation collision
defer
碰撞 Avoidance: RTSCTS 交换
时间
reservation 碰撞

802.11 frame: addressing
6-36
frame
control
duration
address
1
address
2
address
4
address
3
payloadCRC
226662
6
0 -2312
4
seq
control
Address 2: MAC address
of wireless host or AP 
transmittingthis frame
Address 1: MAC address
of wireless host or AP
to 
receive
this frame
Address 3: MAC address
of routerinterface to 
which AP is attached
Address 4: used only in 
ad hoc mode
Why？
802.11 帧: 编址
帧
控制
地址地址地址地址
载荷
控制
地址 2:MAC 地址
的无线 主机 或 AP
这个 帧
地址 1:MAC 地址
的无线 主机或
到
接收
这个 帧
地址 3:MAC 地址
的路由器接口 到
哪个 AP 是 attached
地址 4:用于 仅 在

6-37
Internet
router
H1
R1
AP MAC addrH1 MAC addrR1 MAC addr
address 1
address 2
address 3
802.11frame
R1 MAC addr  H1 MAC addr 
dest. address 
source address 
802.3frame
802.11 frame: addressing
路由器
地址 1
地址 2
地址 3
帧
dest. 地址
源 地址
帧
802.11 帧: 编址


H1 remains in same IP 
subnet: IP address can 
remain same

switch: which AP is 
associated with H1?

self-learning(Ch. 5): switchwill 
see frame from H1 and 
“remember”which switch 
port can be used to reach H1
6-42
802.11: mobility within same subnet
H1
BBS 2
BBS 1
BER
H1 remains 在 相同 IP
子网: IP 地址 可以
remain 相同
交换机: 哪个 AP 是
associated 与 H1?
学习交换机将
见 帧 从 H1 和
哪个 交换机
端口 可以 是 用于 到 reach H1
802.11: 移动性 在...内 相同 子网

Rate adaptation

base station, mobile dynamically 
change transmission rate 
(physical layer modulation 
technique) as mobile moves, SNR 
varies 
6-43
802.11: advanced capabilities
QAM256 (8 Mbps)
QAM16 (4 Mbps)
BPSK (1 Mbps)
1020
3040
SNR(dB)
BER
10
-1
10
-2
10
-3
10
-5
10
-6
10
-7
10
-4
operating point
1. SNR decreases, BER increase 
as node moves away from base 
station
2. When BER becomes too 
high, switch to lower 
transmission rate but with 
lower BER
速率 adaptation
基站, 移动 dynamically
变化 传输 速率
(物理 层 modulation
technique) as 移动 moves, SNR
operating 点
1. SNR decreases, BER 增大
as 节点 moves away 从 基
站
2. 当 BER becomes too
high, 交换机 到 lower
传输 速率 但是 与

BER
6-44
power management
node-to-AP: “I am going to sleep until next beacon frame”
AP knows not to transmit frames to this node
node wakes up before next beacon frame

beacon frame: contains list of mobiles with AP-to-mobile frames 
waiting to be sent
node will stay awake if AP-to-mobile frames to be sent; otherwise sleep again 
until next beacon frame
802.11: advanced capabilities
power 管理
节点到
I am going 到 sleep 直到 下一个 beacon 帧
AP 知道 不 到 传输 帧 到 这个 节点
节点 wakes 上 之前 下一个 beacon 帧
beacon 帧: 包含 列表 的 mobiles 与 AP到移动 帧
waiting 到 是 已发送
节点 将 stay awake 如果 AP
到
移动 帧 到 是 已发送; otherwise sleep again
直到 下一个 beacon 帧

BER

less than 10 m diameter

replacement for cables (mouse, 
keyboard, headphones)

ad hoc: no infrastructure

master/slaves:

slaves request permission to send (to 
master)

master grants requests

802.15: evolved from Bluetooth 
specification

2.4-2.5 GHz radio band

up to 721 kbps
6-45
M
radius of
coverage
S
S
S
P
P
P
P
M
S
Master device
Slave device
Parked device (inactive)
P
802.15: personal area network
less 比 10 m diameter
replacement 用于 cables (mouse,
ad hoc: 无 基础设施
slaves 请求 permission 到 发送 (到
master grants 请求
802.15: evolved 从 Bluetooth
上 到 721 kbps
radius 的
Master 设备
Slave 设备
Parked 设备 (inactive)
802.15: personal 区域 网络

Chapter 6 outline
7.1 Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4 Cellular Internet access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing 
and routing to mobile 
users
7.6Mobile IP
7.7Handling mobility in 
cellular networks
7.8Mobility and higher-
layer protocols
7.9Summary
6-46
第 6 章：提纲
引言
无线
无线链路, 特性
IEEE 802.11 无线 局域网
7.4 蜂窝 Internet 访问
体系结构
移动性
原理: 编址
和 路由 到 移动
用户
移动 IP
处理 移动性 在
蜂窝 网络
移动性 和 higher
层 协议
总结

Mobile device  
(UE)
Base station
(eNode-B)  
Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-49
radio access
network
all-IP Enhanced Packet Core 
(EPC)
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
to 
Internet
...
Mobile device:
smartphone, tablet, 
laptop, IoT, ... with 4G 
LTE radio
64-bit International 
Mobile Subscriber 
Identity (IMSI), stored on 
SIM (Subscriber Identity 
Module) card
LTE jargon: User 
Equipment (UE)
移动 设备
基站
Elements 的 4G LTE 体系结构
无线 和 移动网络s: 7
radio 访问
网络
所有
IP Enhanced 分组 核心
移动性
管理
Serving 网关 (
归属 Subscriber
服务 (
PDN 网关 (
到
移动 设备:
laptop, IoT, ... 与 4G
比特 International
移动 Subscriber
Identity (IMSI), 存储的 在
LTE jargon: 用户

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-50
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Base station:
at “edge” of carrier’s network
manages wireless radio 
resources, mobile devices in 
its coverage area (“cell”)
coordinates device 
authentication with other 
elements
similar to WiFi AP but:
•active role in user mobility
•coordinates with nearly 
base stations to optimize 
radio use
LTE jargon: eNode-B
Elements 的 4G LTE 体系结构
无线 和 移动网络s: 7
移动性
管理
Serving 网关 (
归属 Subscriber
服务 (
PDN 网关 (
到
移动 设备
基站
基站:
在 “边缘” 的 carrier’s 网络
manages 无线 radio
resources, 移动 设备 在
它的 coverage 区域 (“小区”)
coordinates 设备
authentication 与 其他
similar 到 WiFi AP 但是:
主动 role 在 用户 移动性
coordinates 与 nearly
基站s 到 optimize
radio 使用

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-51
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Home Subscriber Service 
stores info about mobile 
devices for which the HSS’s 
network is their “home 
network”
works with MME in device 
authentication 
Elements 的 4G LTE 体系结构
无线 和 移动网络s: 7
移动性
管理
Serving 网关 (
归属 Subscriber
服务 (
PDN 网关 (
到
移动 设备
基站
归属 Subscriber 服务
stores 信息 关于 移动
设备 用于 哪个 HSS’s
网络 是 它们的 “归属
网络”
works 与 MME 在 设备

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-52
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Serving Gateway (S-GW), 
PDN Gateway (P-GW)
lie on data path from mobile 
to/from Internet
P-GW
•gateway to mobile cellular 
network
•Looks like nay other internet 
gateway router
•provides NAT services 
other routers:
•extensive use of tunneling
Elements 的 4G LTE 体系结构
无线 和 移动网络s: 7
移动性
管理
Serving 网关 (
归属 Subscriber
服务 (
PDN 网关 (
到
移动 设备
基站
Serving 网关 (S
PDN 网关 (P
lie 在 数据 路径 从 移动
to/来自 Internet
网关 到 移动 蜂窝
网络
Looks 像 nay 其他 Internet
网关 路由器
提供 NAT 服务
其他 路由器:
extensive 使用 的 tunneling

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-53
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Mobility Management 
Entity
device authentication 
(device-to-network, 
network-to-device) 
coordinated with mobile 
home network HSS 
mobile device management: 
•device handover between cells
•tracking/paging device location
path (tunneling) setup from 
mobile device to P-GW
Elements 的 4G LTE 体系结构
无线 和 移动网络s: 7
移动性
管理
Serving 网关 (
归属 Subscriber
服务 (
PDN 网关 (
到
移动 设备
基站
移动性 管理
设备 authentication
(设备到网络,
网络到设备)
coordinated 与 移动
归属网络 HSS
移动 设备 管理:
设备 切换 之间 cells
tracking/paging 设备 location
路径 (tunneling) setup 从
移动 设备 到 P

Chapter 7 summary
Wireless

wireless links:

capacity, distance

channel impairments

CDMA

IEEE 802.11 (“Wi-Fi”)

CSMA/CA reflects wireless channel 
characteristics

cellular access

architecture

standards (e.g., GSM, 3G, 4G LTE)
Mobility

principles: addressing, 
routing to mobile users

home, visited networks

direct, indirect routing

care-of-addresses

case studies

mobile IP

mobility in GSM

impact on higher-layer 
protocols
6-85
The End of Chapter 7
第 7 章总结
无线
无线链路:
容量, 距离
信道 impairments
CSMA/CA reflects 无线 信道
特性
蜂窝 访问
体系结构
移动性
原理: 编址,
路由 到 移动用户s
归属, 被访网络s
直接, indirect 路由
的地址
移动 IP
移动性 在 GSM
impact 在 higher层
协议
端 的 第 7 章

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
