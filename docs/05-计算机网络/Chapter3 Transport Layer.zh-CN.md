# Chapter3 Transport Layer.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter3 Transport Layer.zh-CN.pdf　｜　88,473 字符　｜　160 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
计算机网络
2024. 秋季

A note on the use of these Powerpoint slides:
We’re making these slides freely available to all (faculty, students, readers). They’re in 
PowerPoint form so you see the animations; and can add, modify, and delete slides  
(including this one) and slide content to suit your needs. They obviously represent a lot
of work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their source (after all, we’d 
like people to use our book!)
If you post any slides on a www site, that you note that they are adapted from (or 
perhaps identical to) our slides, and note our copyright of this material.
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2016
J.F Kurose and K.W. Ross, All Rights Reserved
Chapter 3
Transport Layer
2-2
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
说明 在 使用 的 thes
e Powerpoint 幻灯片:
我们
re making 这些 幻灯片 freely 可用的 到 所有 (faculty, studen
ts, readers). They’re 在
PowerPoint form 因此 你 见 animations; 和 可以 添加, 修改,和 删除 幻灯片
(包括 这个 一个) 和 幻灯片 内容 到 suit your needs. They
的 工作 在 我们的 part. 在 返回 用于 使用, 我们 仅 ask followi
如果 你 使用 这些 幻灯片 (e.g., i
n a 课堂) 这 你 mention
ir 源 (之后 所有, 我们
像 people 到 使用 我们的 book!)
如果 你 post 任何 幻灯片 在 a 网站 站点
, 这 你 说明 这 they 是 adapted 从 (或
perhaps identical 到) 我们的 幻灯片, 和 说明 我们的 版权 的 thi
谢谢 和 使用愉快! JFK/KWR
所有 材料 版权 1996-2016
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved
第 3 章
运输层
计算机 网络:
自顶向下 方法

Assignments :
Ch3 (ver8, CN): 3、4、7、14、15、20、22、27、32、36、40
、43、48、52、56、58
Keywords: multiplexing, demultiplexing, reliable data transfer, flow 
control, congestion control, UDP, TCP, Go back n, Selective repeat, 
TCP window, AIMD
作业：
关键词： 多路复用, 多路分解, 可靠数据传输,流
控制, 拥塞控制, UDP, TCP, Go back n, Selective rep
TCP 窗口, AIMD

3-4
Chapter 3: Transport Layer
our goals: 
understand principles 
behind transport layer 
services:
multiplexing, demultiplexing
reliable data transfer
flow control
congestion control
learn about Internet transport 
layer protocols:
UDP: connectionless transport
TCP: connection-oriented 
reliable transport
TCP congestion control
第 3 章：运输层
我们的目标：
understand 原理
背后的 运输层
服务:
多路复用, 多路分解
可靠数据传输
流量控制
拥塞控制
learn 关于 Internet 运输
层 协议:
UDP: 无连接传输
TCP: 面向连接
可靠 运输
TCP 拥塞控制

3-5
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输:
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接
管理
3.6 原理 的
拥塞控制
3.7 TCP 拥塞
控制

3-6
Transport services and protocols
providelogical communicationbetween app 
processes running on different hosts
transport protocols run in end systems 
send side: breaks app messages into 
segments, passes to  network layer
rcv side: reassembles segments into 
messages, passes to app layer
more than one transport protocol available to 
apps
Internet: TCP and UDP
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
20241012
运输服务和协议
提供逻辑通信之间 应用
进程 运行 在 不同的 主机
运输协议 运行 在 端 系统
发送 一侧: breaks 应用 报文 进入
报文段, passes 到 网络层
rcv 一侧: reassembles 报文段 进入
报文, passes 到 应用 层
更多 比 一个 运输协议 可用的 到
应用
Internet: TCP 和 UDP
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

5
4
3
2
1
运输层提供应用进程间的逻辑通信
主机A主机B
应用进程应用进程
路由器1路由器2
AP
1
LAN
2
WAN
AP
2
AP
3
AP
4
IP 层
LAN
1
AP
1
AP
2
AP
4
端口
端口
5
4
3
2
1
IP 协议的作用范围
运输层协议TCP 和UDP 的作用范围
AP
3
Transport vs. network layer
7
运输 vs. 网络层

3-8
Transport vs. network layer
network layer:logical communication between hosts
transport layer:logical communication between processes 
relies on, enhances, network layer services
网络层：主机间数据交互
传输层：进程间数据交互
Manager
~App
Secretary
~TCP
Staff-
IP
运输 vs. 网络层
网络层:逻辑通信之间 主机
运输层:逻辑通信之间 进程
relies 在, enhances, 网络层 服务
~应用

physical
link
network (IP)
application
physical
link
network (IP)
application
transport
Transport Layer Actions
Sender:
app. msg
is passed an application‐
layer message
determines segment 
header fields values
creates segment
passes segment to IP
transport
T
h
T
h
app. msg
Transport Layer: 3-10
物理
链路
网络 (IP)
应用
物理
链路
网络 (IP)
应用
运输
运输层 动作
发送方:
是 passed 应用‐
层 报文
determines 报文段
首部 字段 值
创建 报文段
passes 报文段 到 IP
运输
运输层: 3-10

physical
link
network (IP)
application
physical
link
network (IP)
application
transport
Transport Layer Actions
transport
Receiver:
app.  msg
extracts application‐layer 
message
checks header values
receives segment from IP
T
h
app. msg
demultiplexes message up 
to application via socket
Transport Layer: 3-11
物理
链路
网络 (IP)
应用
物理
链路
网络 (IP)
应用
运输
运输层 动作
运输
接收方:
extracts 应用‐层
报文
checks 首部 值
接收 报文段 从 IP
demultiplexes 报文 上
到 应用 通过 套接字
运输层: 3-11

3-12
Internet transport-layer protocols
reliable, in-order delivery 
(TCP)
congestion control 
flow control
connection setup
unreliable, unordered 
delivery: UDP
no-frills extension of 
“best-effort”IP
services not available: 
delay guarantees
bandwidth guarantees
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
Internet 运输层协议
可靠、按序交付
拥塞控制
流量控制
连接建立
交付:
no-frills extension 的
服务 不 可用的:
时延 guarantees
带宽 guarantees
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

3-13
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输: UDP
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
Transport Layer: 3-14
运输
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
HTTP 服务器
客户
运输层: 3-14

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
HTTP msg
Transport Layer: 3-15
运输
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
HTTP 服务器
客户
运输层: 3-15

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
HTTP msg
H
t
H
n
HTTP msg
Transport Layer: 3-16
运输
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
HTTP 服务器
客户
运输层: 3-16

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
H
n
Transport Layer: 3-17
运输
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
HTTP 服务器
客户
运输层: 3-17

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
1
client
2
P-client
1
P-client
2
Transport Layer: 3-18
运输
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
HTTP 服务器
客户
客户
P-客户
P-客户
运输层: 3-18

3-19
Multiplexing/demultiplexing
process
socket
use header info to deliver
received segments to correct 
socket
demultiplexing at receiver:
handle data from multiple
sockets, add transport 
header (later used for 
demultiplexing)
multiplexing at sender:
transport
application
physical
link
network
P2P1
transport
application
physical
link
network
P4
transport
application
physical
link
network
P3
Multiplexing: Aggregate data from different applications to the TCP and IP layers 
Demultiplexing: Distribute network layer data to different applications
多路复用/多路分解
进程
套接字
使用 首部 信息 到 交付
已收到 报文段 到 correct
套接字
多路分解 在 接收方:
处理 数据 从 多路
sockets, 添加 运输
首部 (later 用于 用于
多路分解)
多路复用 在 发送方:
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
多路复用: Aggregate 数据 从 different 应用 到TCP 和 IP 层
多路分解: Distribute network 层 数据 到 不同的 appl

20
App1App2
socket1
socket2
IP
multiplexing
demultiplexing
Multiplexing/demultiplexing
App3
socket3
TCP segments
App1App2
socket1
socket2
IP
App3
socket3
UDP
TCP segments
UDP
multiplexing
demultiplexing
应用1应用2
多路复用
多路分解
多路复用/多路分解
应用3
TCP 报文段
应用1应用2
应用3
TCP 报文段
多路复用
多路分解

3-21
How demultiplexing works
host receives IP datagrams
each datagram has source IP 
address, destination IP address
each datagram carries one transport-
layer segment
each segment has source, 
destination port number 
host uses IP addresses & port 
numbersto direct segment to 
appropriate socket
source port #   dest port #
32 bits
application
data 
(payload)
other header fields
TCP/UDP segment format
IP payloadIP Hdr
srcIP+dstIP+srcPort+dstPort+protocol
如何 多路分解 works
主机 接收 IP 数据报
每个 数据报 有 源 IP
地址, 目的地 IP 地址
每个 数据报 carries 一个 transport-
层 报文段
每个 报文段 有 源,
目的地 端口 编号
主机 使用IP 地址 & 端口
编号到 直接 报文段 到
合适的 套接字
源 端口 #
dest 端口 #
32 比特
应用
数据
(载荷)
其他 首部 字段
TCP/UDP 报文段 格式
IP 载荷

3-22
Connectionless demultiplexing
recall:created socket has 
host-local port #:
DatagramSocket mySocket1        
= new DatagramSocket(12534);
when host receives
UDP segment:
checks destination port 
# in segment
directs UDP segment to 
socket with that port #
recall:when creating 
datagram to sendinto 
UDP socket, must specify
destination IP address
destination port #
IP datagrams with 
same dest. port #,but 
different source IP 
addresses and/or 
source port numbers 
will be directed to same 
socket at dest
无连接 多路分解
回顾:created 套接字 有
主机-local 端口 #:
= 新的 DatagramSocket(
当 主机接收
UDP 报文段:
checks 目的地 端口
# 在 报文段
directs UDP 报文段 到
套接字 与 这 端口
回顾:当 creating
数据报 到发送
进入
UDP 套接字, 必须 specify
目的地 IP 地址
目的地 端口 #
IP 数据报 与
相同 dest. 端口 #,
但是
不同的 源 IP
地址 and/or
源 端口 编号
将 是 directed 到
相同
套接字
在 dest

3-23
Connectionless demux: example
DatagramSocket
serverSocket = new 
DatagramSocket
(6428);
transport
application
physical
link
network
P3
transport
application
physical
link
network
P1
transport
application
physical
link
network
P4
DatagramSocket 
mySocket1 = new 
DatagramSocket 
(5775);
DatagramSocket 
mySocket2 = new 
DatagramSocket
(9157);
source port: 9157
dest port: 6428
source port: 6428
dest port: 9157
source port: ?
dest port: ?
source port: ?
dest port: ?
接收端程序（P1）无需区分所收到数据是来自哪一个发送端
接收端程序
在本地的一
个镜像
发送端在本地
的镜像。
混合了来自
P3,P4的数据，
无法区分来源
无连接 demux: 示例
serverSocket = 新的
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
运输
应用
物理
链路
网络
mySocket1 = 新的
mySocket2 = 新的
源 端口: 9157
dest 端口: 6428
源 端口: 6428
dest 端口: 9157
源 端口: ?
dest 端口: ?
源 端口: ?
dest 端口: ?

3-24
Connection-oriented demux
TCP socket identified by 4-
tuple: 
source IP address
source port number
dest IP address
dest port number
demux: receiver uses all four 
values to direct segment to 
appropriate socket
server host may support many 
simultaneous TCP sockets:
each socket identified by its own 
4-tuple
web servers have different 
sockets for each connecting 
client
non-persistent HTTP will have 
different socket for each request
面向连接 demux
TCP 套接字 identified 由 4-
源 IP 地址
源 端口 编号
dest IP 地址
dest 端口 编号
demux: 接收方 使用 所有 four
值 到 直接 报文段 到
合适的 套接字
服务器 主机 可能 支持 许多
每个 套接字 identified 由 它的 自己的
Web 服务器 有 不同的
sockets 用于 每个 connecting
客户
non-persistent HTTP 将 有
不同的 套接字 用于 每个 请求

3-25
Connection-oriented demux: example
transport
application
physical
link
network
P3
transport
application
physical
link
P4
transport
application
physical
link
network
P2
source IP,port: A,9157
dest IP, port: B,80
source IP,port: B,80
dest IP,port: A,9157
host: IP 
address A
host: IP 
address C
network
P6
P5
P3
source IP,port: C,5775
dest IP,port: B,80
source IP,port: C,9157
dest IP,port: B,80
three segments, all destined to IP address: B,
dest port: 80 are demultiplexed to 
different sockets
server: IP 
address B
Different Apps (Instances)
接收端程序
在本地的一
个镜像
发送端在本
地的镜像。
独立对应每
一个通信对
端应用程序
。可以区分
来源
面向连接 demux: 示例
运输
应用
物理
链路
网络
运输
应用
物理
链路
运输
应用
物理
链路
网络
源 IP,端口: ,9157
dest IP, 端口: B,80
源 IP,端口: B,80
dest IP,端口: ,9157
主机: IP
地址
主机: IP
地址 C
网络
源 IP,端口: C,5775
dest IP,端口: B,80
源 IP,端口: C,9157
dest IP,端口: B,80
three 报文段, 所有 destined 到 IP 地址: B,
dest 端口: 80 是 demultiplexed 到
不同的
服务器: IP
地址 B
不同的 应用 (Instances)

3-26
Connection-oriented demux: example
transport
application
physical
link
network
P3
transport
application
physical
link
transport
application
physical
link
network
P2
source IP,port: A,9157
dest IP, port: B,80
source IP,port: B,80
dest IP,port: A,9157
host: IP 
address A
host: IP 
address C
server: IP 
address B
network
P3
source IP,port: C,5775
dest IP,port: B,80
source IP,port: C,9157
dest IP,port: B,80
P4
threaded server
面向连接 demux: 示例
运输
应用
物理
链路
网络
运输
应用
物理
链路
运输
应用
物理
链路
网络
源 IP,端口: ,9157
dest IP, 端口: B,80
源 IP,端口: B,80
dest IP,端口: ,9157
主机: IP
地址
主机: IP
地址 C
服务器: IP
地址 B
网络
源 IP,端口: C,5775
dest IP,端口: B,80
源 IP,端口: C,9157
dest IP,端口: B,80
threaded 服务器

Summary
Multiplexing, demultiplexing: based on segment, datagram 
header field values
UDP: demultiplexing using destination port number (only)
TCP: demultiplexing using 4‐tuple: source and destination IP 
addresses, and port numbers
Multiplexing/demultiplexing happen at alllayers
Transport Layer: 3-28
总结
多路复用, 多路分解: 基于 在 报文段, 数据报
首部 字段 值
多路分解 使用 目的地 端口 编号 (仅)
多路分解 使用 4‐tuple: 源 和 目的地 IP
地址, 和 端口 编号
多路复用/多路分解 happen 在
所有
层
运输层: 3-28

3-29
Chapter 3 outline
3.1 transport-layer 
services
3.2 multiplexing and 
demultiplexing
3.3 connectionless 
transport: UDP
3.4 principles of reliable 
data transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control
第 3 章：提纲
3.1 运输层
服务
3.2 多路复用 和
多路分解
3.3 无连接
运输: UDP
3.4 原理 的 可靠
数据 传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接
管理
3.6 原理 的
拥塞控制
3.7 TCP 拥塞
控制

3-30
UDP: User Datagram Protocol [RFC 768]
“no frills,”“bare bones”
Internet transport protocol
“best effort”service, UDP 
segments may be:
lost
delivered out-of-order to app
connectionless:
no handshaking between UDP 
sender, receiver
each UDP segment handled 
independently of others
UDP use:
streaming multimedia apps (loss 
tolerant, rate sensitive)
DNS
SNMP
reliable transfer over UDP: 
add reliability at application layer
application-specific error 
recovery!
UDP: 用户 数据报 协议 [RFC 768]
无 frills,
Internet 运输协议
尽力而为服务, UDP
报文段 可能 是:
丢失的
delivered out-of-order 到 应用
无连接:
无 握手 之间 UDP
发送方, 接收方
每个 UDP 报文段 handled
independently 的 others
UDP 使用:
streaming multimedia 应用 (丢包
tolerant, 对速率敏感)
可靠 传输 在 UDP:
添加 reliability 在 应用层
应用-specific 差错

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
transport
(UDP)
physical
link
network (IP)
application
Transport Layer: 3-31
SNMP 服务器
SNMP 客户
运输
物理
链路
网络 (IP)
应用
UDP: 运输层 动作
运输
物理
链路
网络 (IP)
应用
运输层: 3-31

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
UDP sender actions:
SNMP msg
is passed an application‐
layer message
determines UDP segment 
header fields values
creates UDP segment
passes segment to IP
UDP
h
UDP
h
SNMP msg
Transport Layer: 3-32
SNMP 服务器
SNMP 客户
运输
物理
链路
网络 (IP)
应用
运输
物理
链路
网络 (IP)
应用
UDP: 运输层 动作
UDP 发送方 动作:
是 passed 应用‐
层 报文
determines UDP 报文段
首部 字段 值
创建 UDP 报文段
passes 报文段 到 IP
运输层: 3-32

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
UDP receiver actions:
SNMP msg
extracts application‐layer 
message
checks UDP checksum 
header value
receives segment from IP
UDP
h
SNMP msg
demultiplexes message up 
to application via socket
Transport Layer: 3-33
SNMP 服务器
SNMP 客户
运输
物理
链路
网络 (IP)
应用
运输
物理
链路
网络 (IP)
应用
UDP: 运输层 动作
UDP 接收方 动作:
extracts 应用‐层
报文
checks UDP 校验和
首部 值
接收 报文段 从 IP
demultiplexes 报文 上
到 应用 通过 套接字
运输层: 3-33

3-34
UDP: segment header
source port #
dest port #
32 bits
application
data 
(payload)
UDP segment format
length
checksum
length, in bytes of 
UDP segment, 
including header
no connection 
establishment (which 
can add delay)
simple: no connection 
state at sender, receiver
small header size
no congestion control: 
UDP can blast away as 
fast as desired
why is there a UDP?
UDP: 报文段 首部
源 端口 #
dest 端口 #
32 比特
应用
数据
(载荷)
UDP 报文段 格式
长度
校验和
长度, 在 字节 的
UDP 报文段,
包括 首部
无 连接
establishment (哪个
可以 添加 时延)
simple: 无 连接
状态 在 发送方, 接收方
small 首部 大小
无 拥塞控制:
UDP 可以 blast away as
快速 as desired
why 是 存在 a UDP?

3-35
UDP checksum
sender:
treat segment contents, 
including header fields,  as 
sequence of 16-bit integers
checksum: addition (one’s 
complement sum) of segment 
contents
sender puts checksum value into 
UDP checksum field
receiver:
compute checksum of received 
segment
check if computed checksum equals 
checksum field value:
NO - error detected
YES - no error detected. But 
maybe errors nonetheless?More 
later ....
Goal:detect “errors”(e.g., flipped bits) in transmitted segment
UDP 校验和
发送方:
报文段 contents
包括首部 字段
序列 的 16-bit integers
校验和: addition (一个’s
complement sum) 的 报文段
发送方 puts 校验和 值 进入
UDP 校验和 字段
接收方:
计算 校验和 的 已收到
报文段
查看 如果 计算得到的 校验和 equals
校验和 字段 值:
NO - 差错 detected
YES - 无 差错 detected.
但是
maybe 差错 nonetheless?更多
目标:检测差错(e.g., flipped 比特) 在 已传输 报文段

伪首部 源端口 目的端口
长度
检验和
数据首部
UDP长度源IP 地址    目的IP 地址
017
IP 数据报
字节
44112
12
2222
字节
发送在前
数据首部
UDP 用户数据报
在计算检验和时，临时把“伪首部”和UDP 用户数据报连接在一起。伪
首部仅仅是为了计算检验和。
UDP头部
及净荷
填充0,没有
实质作用

3-38
Internet checksum: example
example: add two 16-bit integers
11  1  1  0  0  1  1  0  0  1  1  0  0  1  1  0
11  1  0  1  0  1  0  1  0  1  0  1  0  1  0  1
1  1  0  1  1  1  0  1  1  1  0  1  1  1  0  1  1
11  0  1  1  1  0  1  1  1  0  1  1  1  1  0  0
10  1  0  0  0  1  0  0  0  1  0  0  0  0  1  1
wraparound
sum
checksum
Note:when adding numbers, a carryout from the most 
significant bit needs to be added to the result
0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1
Internet 校验和: 示例
示例: 添加 两个 16-bit integers
校验和
说明:当 adding 编号, a carryout 从 most
significant 比特 需要 到 是 added 到 result

Summary: UDP
“no frills” protocol: 
•segments may be lost, delivered out of order
•best effort service: “send and hope for the best”
UDP has its plusses:
•no setup/handshaking needed (no RTT incurred)
•can function when network service is compromised
•helps with reliability (checksum)
build additional functionality on top of UDP in application layer 
(e.g., HTTP/3)
总结
“无 frills” 协议:
报文段 可能 是 丢失的, delivered 查看 的 order
尽力而为 服务: “发送 和 hope 用于 best”
UDP 有 它的 plusses:
无 setup/握手 needed (无 RTT incurred)
可以 功能 当 网络 服务 是 compromised
helps 与 reliability (校验和)
build additional functionality 在 top 的 UDP 在 应用 lay

3-40
Chapter 3 outline
3.1 transport-layer 
services
3.2 multiplexing and 
demultiplexing
3.3 connectionless 
transport: UDP
3.4 principles of reliable 
data transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control
第 3 章：提纲
3.1 运输层
服务
3.2 多路复用 和
多路分解
3.3 无连接
运输: UDP
3.4 原理 的 可靠
数据 传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接
管理
3.6 原理 的
拥塞控制
3.7 TCP 拥塞
控制

3-43
Principles of reliable data transfer
important in application, transport, link layers
top-10 list of important networking topics!
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)
原理 的 可靠数据传输
重要 在 应用, 运输, 链路层s
top-10 列表 的 重要 网络 topics!
特性 的 unreliable 信道 将 确定
复杂度 的 可靠数据传输 协议 (rdt)

3-44
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)
Principles of reliable data transfer
important in application, transport, link layers
top-10 list of important networking topics!
?
IP
IP layer
Sending IPs
Receiving IPs
Manager
~App
Secretary
~TCP
特性 的 unreliable 信道 将 确定
复杂度 的 可靠数据传输 协议 (rdt)
原理 的 可靠数据传输
重要 在 应用, 运输, 链路层s
top-10 列表 的 重要 网络 topics!
IP 层
发送 IPs
接收 IPs
~应用

3-45
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)
important in application, transport, link layers
top-10 list of important networking topics!
Principles of reliable data transfer
特性 的 unreliable 信道 将 确定
复杂度 的 可靠数据传输 协议 (rdt)
重要 在 应用, 运输, 链路层s
top-10 列表 的 重要 网络 topics!
原理 的 可靠数据传输

3-46
Reliable data transfer: getting started
senderreceiver
rdt_send():called from above, 
(e.g., by app.). Passed data to 
deliver to receiver upper layer
udt_send():called by rdt,
to transfer packet over 
unreliable channel to receiver
rdt_rcv():called when packet 
arrives on rcv-side of channel
deliver_data():called by 
rdtto deliver data to upper
可靠数据传输: getting started
发送方接收方
called 从 上方,
(e.g., 由 app.). Passed 数据 到
交付 到 接收方 upper 层
called 由 rdt,
到 传输 分组 在
unreliable 信道 到 接收方
called 当 分组
到达 在 rcv-side 的 信道
called 由
到 交付 数据 到 upper

3-47
We’ll:
incrementally develop sender, receiver sides of reliable data 
transfer protocol (rdt)
consider only unidirectional data transfer
but control info will flow on both directions!
use finite state machines (FSM)  to specify sender, receiver
state
1
state
2
event causing state transition
actions taken on state transition
state:when in this 
“state”next state 
uniquelydetermined 
by next event
event
actions
Reliable data transfer: getting started
我们’ll:
incrementally develop 发送方, 接收方 sides 的
ransfer 协议 (rdt)
consider 仅 unidirectional 数据 传输
但是 控制 信息 将 流 在 二者 directions!
使用 finite 状态 machines (FSM) 到 specify 发送方, 接收方
状态
状态
event causing 状态 transition
动作 经过 在 状态 transition
状态:当 在 这个
状态
下一个 状态
由 下一个 event
动作
可靠数据传输: getting started

3-48
rdt1.0: reliable transfer over a reliable channel
underlying channel (i.e., IP) perfectly reliable
no bit errors
no loss of packets
separate FSMs for sender, receiver:
sender sends data into underlying channel
receiver reads data from underlying channel
Wait for 
call from 
above
packet = make_pkt(data)
udt_send(packet)
rdt_send(data)
extract (packet,data)
deliver_data(data)
Wait for 
call from 
below
rdt_rcv(packet)
senderreceiver
rdt1.0: 可靠 传输 在 a可靠 信道
underlying 信道 (i.e., IP) perfectly可靠
无 比特 差错
无 丢包 的 分组
separate FSMs 用于 发送方, 接收方:
发送方 发送 数据 进入 underlying 信道
接收方 reads 数据 从 underlying 信道
等待 用于
调用 从
上方
分组 = make_pkt(数据)
udt_send(分组)
rdt_send(数据)
extract (分组,数据)
deliver_data(数据)
等待 用于
调用 从
rdt_rcv(分组)
发送方接收方

3-49
underlying channel may flip bits in packet
checksum to detect bit errors
thequestion: how to recover from errors:
acknowledgements (ACKs):receiver explicitly tells 
sender that pkt received OK
negative acknowledgements (NAKs):receiver 
explicitly tells sender that pkt had errors
sender retransmits pkt on receipt of NAK
new mechanisms in rdt2.0(beyond rdt1.0):
error detection
receiver feedback: control msgs (ACK,NAK) rcvr-
>sender
rdt2.0: channel with bit errors
How do humans recover from “errors”
during conversation?
underlying 信道 可能 flip 比特 在 分组
校验和 到 检测 比特 差错
问题: 如何 到 recover 从 差错:
接收方 explicitly 告诉
发送方 这 pkt 已收到 OK
接收方
explicitly 告诉 发送方 这 pkt had 差错
发送方 retransmits pkt 在 receipt 的 NAK
新的 mechanisms 在
差错检测
接收方 feedback: 控制 msgs (ACK,NAK) rcvr-
>发送方
rdt2.0: 信道 与 比特 差错
如何 做 humans recover 从差错

3-50
underlying channel may flip bits in packet
checksum to detect bit errors
thequestion: how to recover from errors:
acknowledgements (ACKs):receiver explicitly tells sender that pkt
received OK
negative acknowledgements (NAKs):receiver explicitly tells sender that 
pkt had errors
sender retransmitspkt on receipt of NAK
new mechanisms in rdt2.0(beyond rdt1.0):
error detection
feedback: control msgs (ACK,NAK) from receiver to sender
rdt2.0: channel with bit errors
underlying 信道 可能 flip 比特 在 分组
校验和 到 检测 比特 差错
问题: 如何 到 recover 从 差错:
接收方 explicitly 告诉 发送方 这 pkt
已收到 OK
接收方 explicitly 告诉 发送方 这
pkt had 差错
发送方pkt 在 receipt 的 NAK
新的 mechanisms 在
差错检测
feedback: 控制 msgs (ACK,NAK) 从 接收方 到 发送方
rdt2.0: 信道 与 比特 差错

3-51
rdt2.0: FSM specification
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
call from 
below
sender
receiver
Wait for 
call from 
above
sndpkt = make_pkt(data, checksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndpkt)
Wait for 
ACK or 
NAK
rdt_send(data)

extract(rcvpkt,数据)
deliver_data(数据)
损坏(rcvpkt)
等待 用于
调用 从
发送方
接收方
等待 用于
调用 从
上方
sndpkt = make_pkt(数据, 校验和)
等待 用于
ACK 或
rdt_send(数据)

3-52
rdt2.0: operation with no errors
Wait for 
call from 
above
snkpkt = make_pkt(data, 
checksum)
udt_send(sndpkt)
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndp
kt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
ACK or 
NAK
Wait for 
call from 
below
rdt_send(data)

rdt2.0: operation 与 无 差错
等待 用于
调用 从
上方
snkpkt = make_pkt(数据,
校验和)
extract(rcvpkt,数据)
deliver_data(数据)
损坏(rcvpkt)
等待 用于
ACK 或
等待 用于
调用 从
rdt_send(数据)

3-53
rdt2.0: error scenario
Wait for 
call from 
above
snkpkt = make_pkt(data, 
checksum)
udt_send(sndpkt)
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndp
kt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
ACK or 
NAK
Wait for 
call from 
below
rdt_send(data)

stop and wait
sender sends one packet, 
then waits for receiver response
rdt2.0: 差错 场景
等待 用于
调用 从
上方
snkpkt = make_pkt(数据,
校验和)
extract(rcvpkt,数据)
deliver_data(数据)
损坏(rcvpkt)
等待 用于
ACK 或
等待 用于
调用 从
rdt_send(数据)
stop 和 等待
发送方 发送 一个 分组,
则 waits 用于 接收方 响应

3-54
rdt2.0 has a fatal flaw!
what happens if ACK/NAK corrupted?
sender doesn’t know what happened at 
receiver!
Can’t just retransmit after timeout: 
possible duplicate
handling duplicates: 
sender retransmits current pkt if 
ACK/NAK corrupted
sender adds sequence number
to each pkt
receiver discards (doesn’t deliver 
up) duplicate pkt
KEY QUESTION: 
duplicates
rdt2.0 有 a fatal flaw!
什么 happens 如果 ACK/NAK corrupted?
发送方 doesn’t know 什么 happened 在
接收方!
可以’t 仅 重传 之后 超时:
可能
重复的
处理 duplicates
发送方 retransmits 当前 pkt 如果
发送方 adds
序列 编号
到 每个 pkt
接收方 discards (doesn’t 交付
上) 重复的 pkt

3-55
rdt2.1: sender, handles garbled ACK/NAKs
Wait for 
call 0from 
above
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
rdt_send(data)
Wait for 
ACK or 
NAK 0
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isNAK(rcvpkt) )
sndpkt = make_pkt(1, data, checksum)
udt_send(sndpkt)
rdt_send(data)
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt) 
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isNAK(rcvpkt) )
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt)
Wait for
call 1from 
above
Wait for 
ACK or 
NAK 1


No sequence 
number in ACK
rdt2.1: 发送方, 处理 出错的 ACK/NAK
等待 用于
调用
从
上方
, 数据, 校验和)
rdt_send(数据)
等待 用于
ACK 或
( 损坏(rcvpkt) ||
, 数据, 校验和)
rdt_send(数据)
( 损坏(rcvpkt) ||
等待 用于
调用
从
上方
等待 用于
ACK 或
无 序列
编号 在 ACK

3-56
Wait for 
0 from 
below
sndpkt = make_pkt(NAK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && 
not corrupt(rcvpkt) &&
has_seq0(rcvpkt)
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq1(rcvpkt)
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
Wait for 
1 from 
below
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq0(rcvpkt) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && (corrupt(rcvpkt)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && 
not corrupt(rcvpkt) &&
has_seq1(rcvpkt)
rdt_rcv(rcvpkt) && (corrupt(rcvpkt)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
sndpkt = make_pkt(NAK, chksum)
udt_send(sndpkt)
rdt2.1: receiver, handles garbled ACK/NAKs
Duplicate Pkt
Duplicate Pkt
等待 用于
从
不 损坏(rcvpkt) &&
extract(rcvpkt,数据)
deliver_data(数据)
等待 用于
从
extract(rcvpkt,数据)
deliver_data(数据)
rdt_rcv(rcvpkt) && (损坏(rcvpkt)
不 损坏(rcvpkt) &&
rdt_rcv(rcvpkt) && (损坏(rcvpkt)
rdt2.1: 接收方, 处理 出错的 ACK/NAK
重复的 Pkt
重复的 Pkt

3-57
rdt2.1: discussion
sender:
seq # added to pkt
two seq. #’s (0,1) will suffice.  
Why?
must check if received 
ACK/NAK corrupted 
State # is twice 
state must “remember” whether 
“expected” pkt should have seq # 
of 0 or 1 
receiver:
must check if received packet 
is duplicate
state indicates whether 0 or 1 is 
expected pkt seq #
note: receiver can notknow if 
its last ACK/NAKreceived OK 
at sender
rdt2.1: 讨论
发送方:
seq # added 到 pkt
两个 seq. #’s (0,1) 将 suffice.
必须 查看 如果 已收到
状态 # 是 twice
状态 必须 “remember” whether
“expected” pkt should 有 seq #
的 0 或 1
接收方:
必须 查看 如果 已收到 分组
是重复的
状态 indicates whether 0 或 1 是
说明: 接收方 可以不
know 如果
它的
最后 ACK/NAK已收到 OK
在 发送方

3-58
rdt2.2: a NAK-free protocol
same functionality as rdt2.1, using ACKs 
only
instead of NAK, receiver sends ACK for last 
pkt receivedOK
receiver must explicitlyinclude seq # of pkt being 
ACKed
duplicate ACK at sender results in same 
action as NAK: retransmit current pkt
rdt2.2: a NAK-free 协议
相同 functionality as rdt2.1, 使用 ACK
仅
instead 的 NAK,接收方 发送 ACK 用于
最后
pkt 已收到
接收方 必须
include seq # 的 pkt being
重复的 ACK 在 发送方 results 在 相同
动作 as NAK:重传 当前 pkt

3-59
rdt2.2: sender, receiver fragments
Wait for 
call 0from 
above
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
rdt_send(data)
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,1))
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,0)
Wait for 
ACK
0
sender FSM
fragment
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq1(rcvpkt) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK1, chksum)
udt_send(sndpkt)
Wait for 
0from 
below
rdt_rcv(rcvpkt) && 
(corrupt(rcvpkt) ||
has_seq1(rcvpkt))
udt_send(sndpkt)
receiver FSM
fragment

duplicate ACK for pkt 1
: Retransmit pkt 0
Duplicate pkt 1
: Retransmit ACK of pkt 1
For rdt2.1, no 
sequence 
number in ACK
Sequence 
number must be 
in the ACK 
rdt2.2: 发送方, 接收方 fragments
等待 用于
调用
从
上方
sndpkt = make_pkt(0, 数据, 校验和)
rdt_send(数据)
( 损坏(rcvpkt) ||
等待 用于
发送方 FSM
extract(rcvpkt,数据)
deliver_data(数据)
等待 用于
从
(损坏(rcvpkt) ||
接收方 FSM
重复的 ACK 用于 pkt 1
: 重传 pkt 0
重复的 pkt 1
: 重传 ACK 的 pkt 1
用于 rdt2.1, 无
序列
编号 在 ACK
序列
编号 必须 是
在 ACK

3-60
rdt3.0: channels with errorsand loss
new assumption:underlying 
channel can also lose
packets (data, ACKs)
checksum, seq. #, ACKs, 
retransmissions will be of help 
... but not enough
approach:sender waits 
“reasonable” amount of timefor 
ACK 
retransmits if no ACK received in this 
time
if pkt (or ACK) just delayed (not lost):
retransmission will be  duplicate, 
but seq. # already handles this
receiver must specify seq # of pkt
being ACKed
requires countdown timer
2023.9.25
rdt3.0: 信道 与差错和丢包
新的 assumption:
信道 可以 also
分组 (数据, ACK)
校验和, seq. #, ACK,
retransmissions 将 是 的 help
... 但是
不 enough
方法:发送方 waits
“reasonable” 数量 的时间
用于
retransmits 如果 无 ACK 已收到 在 这个
时间
如果 pkt (或 ACK) 仅 时延ed (不 丢失的):
重传 将 是 重复的,
但是 seq. # already 处理 这个
接收方 必须 specify seq # 的 pkt
requires countdown 计时器

3-61
rdt3.0 sender
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
start_timer
rdt_send(data)
Wait 
for 
ACK0
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,1) )
Wait for 
call 1 from 
above
sndpkt = make_pkt(1, data, checksum)
udt_send(sndpkt)
start_timer
rdt_send(data)
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,0)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,0) )
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,1)
stop_timer
stop_timer
udt_send(sndpkt)
start_timer
timeout
udt_send(sndpkt)
start_timer
timeout
rdt_rcv(rcvpkt)
Wait for 
call 0from 
above
Wait 
for 
ACK1

rdt_rcv(rcvpkt)



rdt3.0 发送方
数据, 校验和)
rdt_send(数据)
等待
用于
( 损坏(rcvpkt) ||
等待 用于
调用 1 从
上方
数据, 校验和)
rdt_send(数据)
( 损坏(rcvpkt) ||
超时
超时
等待 用于
调用 0从
上方
等待
用于

3-62
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
pkt1
ack1
ack0
ack0
(a) no loss
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
ack1
ack0
ack0
(b) packet loss
pkt1
X
loss
pkt1
timeout
resend pkt1
rdt3.0 in action
发送方
接收方
发送 ack0
发送 ack1
发送 ack0
发送 pkt0
发送 pkt1
发送 pkt0
(a) 无 丢包
发送方
接收方
发送 ack0
发送 ack1
发送 ack0
发送 pkt0
发送 pkt1
发送 pkt0
(b) 分组 丢包
丢包
超时
rdt3.0 在 动作

3-63
rdt3.0 in action
rcv pkt1
send ack1
(detect duplicate)
pkt1
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
ack1
ack0
ack0
(c) ACK loss
ack1
X
loss
pkt1
timeout
resend pkt1
rcv pkt1
send ack1
(detect duplicate)
pkt1
sender
receiver
rcv pkt1
send ack0
rcv ack0
send pkt1
send pkt0
rcv pkt0
pkt0
ack0
(d) premature timeout/ delayed ACK
pkt1
timeout
resend pkt1
ack1
send ack1
send pkt0
rcv ack1
pkt0
ack1
ack0
send pkt0
rcv ack1
pkt0
rcv pkt0
send ack0
ack0
rcv pkt0
send ack0
(detect duplicate)
Would you please design other possible error scenarios?
rdt3.0 在 动作
发送 ack1
(检测 重复的)
发送方
接收方
发送 ack0
发送 ack1
发送 ack0
发送 pkt0
发送 pkt1
发送 pkt0
(c) ACK 丢包
丢包
超时
发送 ack1
(检测 重复的)
发送方
接收方
发送 ack0
发送 pkt1
发送 pkt0
(d) premature timeout/ 时延ed ACK
超时
发送 ack1
发送 pkt0
发送 pkt0
发送 ack0
发送 ack0
(检测 重复的)
Would 你 please design 其他 可能 差错 scenarios?

3-64
Performance of rdt3.0
rdt3.0 is correct, but performance stinks
e.g.: 1 Gbps link, 15 ms prop. delay, 8000 bit packet:
U 
sender
: utilization– fraction of time sender busy 
sending
U 
sender 
= 
.
008 
30.008
= 0.00027  
L / R 
RTT + L / R 
= 
if RTT=30 msec, 1KB pkt every 30 msec: 33kB/sec 
thruput over 1 Gbps link
network protocol limits use of physical resources!
D
trans
=
L
R
8000 bits
10
9 
bits/sec
==
8 microsecs
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
pkt1
ack1
ack0
ack0
性能 的 rdt3.0
rdt3.0 是 correct, 但是
性能 stinks
e.g.: 1 Gbps 链路, 15 ms prop. 时延, 8000 比特 分组:
发送方
利用率
– fraction 的 时间 发送方 busy
发送
发送方
如果
=30 msec, 1KB pkt 每一个 30 msec: 33kB/sec
thruput 在 1 Gbps 链路
网络 协议 limits 使用 的 物理 resources!
8000 比特
发送方
接收方
发送 ack0
发送 ack1
发送 ack0
发送 pkt0
发送 pkt1
发送 pkt0

3-65
rdt3.0: stop-and-wait operation
sender
receive
r
RTT
last packet bit transmitted, 
t = L / R
first packet bit arrives
last packet bit arrives, send 
ACK
ACK arrives, send next 
packet, t = RTT + L / R
U
sender 
= 
.
008 
30.008 
= 0.00027  
L / R
RTT + L / R 
= 
first packet bit transmitted, t = 0
发送方
接收
最后 分组 比特 已传输,
第一个 分组 比特 到达
最后 分组 比特 到达, 发送
ACK 到达, 发送 下一个
分组,
发送方
第一个 分组 比特 已传输, t = 0

3-66
Pipelined protocols
pipelining:sender allows multiple, “in-flight”, 
yet-to-be-acknowledged pkts
range of sequence numbers must be increased
buffering at sender and/or receiver
two generic forms of pipelined protocols: 
go-Back-N, selective repeat
Pipelined 协议
发送方 允许 多路,
range 的 序列 编号 必须 是 increased
buffering 在 发送方 and/or 接收方
两个 generic forms 的 pipelined 协议:

3-67
Pipelining: increased utilization
senderreceiver
RTT 
last bit transmitted, t = L / R
first packet bit arrives
last packet bit arrives, send ACK
ACK arrives, send next 
packet, t = RTT + L / R
last bit of 2
nd
packet arrives, send 
ACK
last bit of 3
rd
packet arrives, 
send ACK
3-packet pipelining increases
utilization by a factor of 3!
U 
sender 
= 
.
0024 
30.008 
= 0.00081  
3L / R
RTT + L / R 
= 
first packet bit transmitted, t = 0
Pipelining: increased 利用率
发送方接收方
最后 比特 已传输, t = L / R
第一个 分组 比特 到达
最后 分组 比特 到达, 发送 ACK
ACK 到达, 发送 下一个
分组, t = RTT + L / R
最后 比特 的 2
分组 到达, 发送
最后 比特 的 3
分组 到达,
发送 ACK
3-分组 pipelining increases
利用率 由 a factor 的 3!
发送方
第一个 分组 比特 已传输, t = 0

3-68
Pipelined protocols: overview
Go-back-N:
sender can have up to N 
unacked packets in pipeline
receiver only sends cumulative 
ack
Doesn’t ack packet if there’s a gap
sender has timer for oldest 
unacked packet
when timer expires, retransmit all
unacked packets
Selective Repeat:
sender can have up to N 
unack’ed packets in pipeline
rcvr sends individual ackfor 
each packet
sender maintains timer for 
each unacked packet
when timer expires, retransmit 
onlythat unacked packet
Pipelined 协议: 概述
发送方 可以 有 上 到 N
unacked 分组 在 pipeline
接收方 仅 发送
Doesn’t ACK 分组 如果 存在’s a gap
发送方 有 计时器 用于 oldest
unacked 分组
当 计时器 expires,重传
所有
unacked 分组
发送方 可以 有 上 到 N
unack’ed 分组 在 pipeline
rcvr 发送
用于
每个 分组
发送方 维护 计时器 用于
每个 unacked 分组
当 计时器 expires,重传
仅这unacked 分组

3-69
Go-Back-N: sender
k-bit seq # in pkt header
“window” of up to N, consecutive unack’ed pkts allowed
ACK(n): ACKs all pkts up to, including seq # n -
“cumulative ACK”
may receive duplicate ACKs (see receiver)
timer for oldest in-flight pkt
timeout(n):retransmit packet n and all higher seq # pkts
in window
1 2 3 45 6 7 8 9 ABCDE F
Go-Back-N: 发送方
k-bit seq # 在 pkt 首部
“窗口” 的 上 到 N, consecutive unack’ed pkts allowed
ACK(n): ACK 所有 pkts 上 到, 包括 seq # n -
可能 接收 重复的 ACK (见 接收方)
计时器 用于 oldest in-flight pkt
超时(n):重传 分组 n 和 所有 higher seq # pkts
在 窗口

3-70
Go-Back-N: sender
0 1 2 3 4 5 6 7 8
Sender:
N =8
receiver:
ACK 0
0 1 2 3 4 5 6 7 8
0 1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 81 2 3 4 5 6 7 8
Go-Back-N: 发送方
发送方:
接收方:

3-71
GBN: sender extended FSM
Wait
start_timer
udt_send(sndpkt[base])
udt_send(sndpkt[base+1])
...
udt_send(sndpkt[nextseqnum-1])
timeout
rdt_send(data)
if (nextseqnum < base+N) {
sndpkt[nextseqnum] = make_pkt(nextseqnum,data,chksum)
udt_send(sndpkt[nextseqnum])
if (base == nextseqnum)
start_timer
nextseqnum++
}
else
refuse_data(data)
base = getacknum(rcvpkt)+1
If (base == nextseqnum)
stop_timer
else
start_timer
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt) 
base=1
nextseqnum=1
rdt_rcv(rcvpkt) 
&& corrupt(rcvpkt)

//窗口中第一个分组启动计时器
//为窗口中下一个已发分组启动计时器
//窗口底部位于还没发出的数据上,无需计时
GBN: 发送方 extended FSM
等待
udt_send(sndpkt[基])
超时
rdt_send(数据)
如果 (nextseqnum < base+N) {
= make_pkt(nextseqnum,数据,chksum)
如果 (基 == nextseqnum)
否则
refuse_data(数据)
基 = getacknum(rcvpkt)+1
如果 (基 == nextseqnum)
否则
&& 损坏(rcvpkt)

3-72
ACK-only: always send ACK for correctly-
received pktwith highest in-orderseq #
may generate duplicate ACKs
need only remember expectedseqnum
out-of-order pkt: 
discard (don’t buffer): no receiver buffering!
re-ACK pkt with highest in-order seq #
Wait
udt_send(sndpkt)
default
rdt_rcv(rcvpkt)
&& notcurrupt(rcvpkt)
&& hasseqnum(rcvpkt,expectedseqnum) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(expectedseqnum,ACK,chksum)
udt_send(sndpkt)
expectedseqnum++
expectedseqnum=1
sndpkt =    
make_pkt(expectedseqnum,ACK,chksum)

GBN: receiver extended FSM
1 2 3 4 5 are received
ACK 5
1 2     4 5 are received
ACK 2; 4-5 are discarded
ACK-only: 总是 发送
ACK 用于 correctly-
已收到 pkt与 highest
可能 generate 重复的 ACK
需要 仅 remember
discard (don’t 缓冲区):
无 接收方 buffering!
re-ACK pkt 与 highest in-order seq #
等待
extract(rcvpkt,数据)
deliver_data(数据)
GBN: 接收方 extended FSM
e 已收到
4 5 是 已收到
ACK 2; 4-5 是 discarded

3-73
GBN in action
send  pkt0
send  pkt1
send  pkt2
send  pkt3
(wait)
sender
receiver
receive pkt0, send ack0
receive pkt1, send ack1
receive pkt3, discard, 
(re)send ack1
rcv ack0, send pkt4
rcv ack1, send pkt5
pkt 2 timeout
send  pkt2
send  pkt3
send  pkt4
send  pkt5
X
loss
receive pkt4, discard, 
(re)send ack1
receive pkt5, discard, 
(re)send ack1
rcv pkt2, deliver, send ack2
rcv pkt3, deliver, send ack3
rcv pkt4, deliver, send ack4
rcv pkt5, deliver, send ack5
ignore duplicate ACK
0 1 2 3 4 5 6 7 8 
sender window (N=4)
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 45 6 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
GBN 在 动作
发送
发送
发送
发送
(等待)
发送方
接收方
接收 pkt0, 发送 ack0
接收 pkt1, 发送 ack1
接收 pkt3, discard,
(re)发送 ACK
rcv ack0, 发送 pkt4
rcv ack1, 发送 pkt5
pkt 2 超时
发送
发送
发送
发送
丢包
接收 pkt4, discard,
(re)发送 ACK
接收 pkt5, discard,
(re)发送 ACK
rcv pkt2, 交付, 发送 ack2
rcv pkt3, 交付, 发送 ack3
rcv pkt4, 交付, 发送 ack4
rcv pkt5, 交付, 发送 ack5
ignore 重复的 ACK
发送方 窗口 (N=4)

3-74
Selective repeat
receiver individuallyacknowledges all correctly received pkts
buffers pkts, as needed, for eventual in-order delivery to upper layer
sender only resends pkts for which ACK not received
sender timer for each unACKed pkt
sender window
Nconsecutive seq #s
limits seq #s of sent, unACKed pkts
接收方acknowledges 所有 correctly 已收到 pkts
缓冲区 pkts, as needed, 用于 even
tual in-order 交付 到 upper 层
发送方 仅 resends pkts 用于 哪个 ACK 不 已收到
发送方 计时器 用于 每个 unACKed pkt
发送方 窗口
limits seq #s 的 已发送, unACKed pkts

3-75
Selective repeat: sender, receiver windows
1 2 3 45 6 7 8 9 ABCDE F
1 2 3 45 6 7 8 9 ABCDE F
Selective repeat: 发送方, 接收方 windows

3-76
Selective repeat
data from above:
if next available seq # in 
window, send pkt
timeout(n):
resend pkt n, restart timer
ACK(n)in 
[sendbase,sendbase+N]:
mark pkt n as received
if n smallest unACKed pkt, 
advance window base to 
next unACKed seq # 
sender
pkt n in 
[rcvbase, rcvbase+N-1]
send ACK(n)
out-of-order: buffer
in-order: deliver (also 
deliver buffered, in-
order pkts), advance 
window to next not-yet-
received pkt
pkt n in [rcvbase-N,rcvbase-1]
ACK(n)
otherwise:
ignore 
receiver
数据 从 上方:
如果 下一个 可用的 seq # 在
窗口, 发送 pkt
超时(n):
resend pkt n, restart 计时器
在
mark pkt n as 已收到
如果 n smallest unACKed pkt,
advance 窗口 基 到
下一个 unACKed seq #
发送方
pkt n 在
发送 ACK(n)
out-of-order: 缓冲区
in-order: 交付 (also
交付 buffered, in-
窗口 到 下一个 not-yet-
已收到 pkt
pkt n 在
接收方

3-77
Selective repeat in action
send  pkt0
send  pkt1
send  pkt2
send  pkt3
(wait)
sender
receiver
receive pkt0, send ack0
receive pkt1, send ack1
receive pkt3, buffer, 
send ack3
rcv ack0, send pkt4
rcv ack1, send pkt5
pkt 2 timeout
send  pkt2
X
loss
receive pkt4, buffer, 
send ack4
receive pkt5, buffer, 
send ack5
rcv pkt2; deliver pkt2,
pkt3, pkt4, pkt5; send ack2
record ack3 arrived
0 1 2 3 4 5 6 7 8 
sender window (N=4)
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 45 6 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
record ack4 arrived
record ack5 arrived
Q: what happens when ack2 arrives?
Selective repeat 在 动作
发送
发送
发送
发送
(等待)
发送方
接收方
接收 pkt0, 发送 ack0
接收 pkt1, 发送 ack1
接收 pkt3, 缓冲区,
发送 ACK
rcv ack0, 发送 pkt4
rcv ack1, 发送 pkt5
pkt 2 超时
发送
丢包
接收 pkt4, 缓冲区,
发送 ACK
接收 pkt5, 缓冲区,
发送 ACK
rcv pkt2; 交付 pkt2,
发送 ack2
发送方 窗口 (N=4)
Q: 什么 happens 当 ack2 到达?

3-78
Selective repeat: dilemma
example: 
seq #’s: 0, 1, 2, 3
window size=3
receiver window
(after receipt)
sender window
(after receipt)
0 1 23 0 1 2
0 1 23 0 1 2
0 1 23 0 1 2
pkt0
pkt1
pkt2
0 1 23 0 1 2
pkt0
timeout
retransmit pkt0
01 2 30 1 2
0 12 3 01 2
0 1 2 3 0 12
X
X
X
will accept packet
with seq number 0
(b) oops!
0 1 23 0 1 2
0 1 23 0 1 2
0 1 23 0 1 2
pkt0
pkt1
pkt2
0 123 01 2
pkt0
01 2 30 1 2
0 12 3 01 2
0 1 2 3 0 12
X
will accept packet
with seq number 0
0 1 2 3 0 1 2
pkt3
(a) no problem
receiver can’t see sender side.
receiver behavior identical in both cases!
Something is (very) wrong!
receiver sees no difference in two 
scenarios!
duplicate data accepted as new in (b)
Q:what relationship between seq # size and 
window size to avoid problem in (b)?
Window size (W) up to N/2
Why？
示例:
窗口 size=3
接收方 窗口
(之后 receipt)
发送方 窗口
(之后 receipt)
超时
重传 pkt0
将 accept 分组
与 seq 编号 0
将 accept 分组
与 seq 编号 0
(a) 无 问题
接收方 可以’t 见 发送方 side.
接收方 behavior identical 在 二者 cases!
Something 是 (very) wrong!
接收方 sees 无 difference 在 两个
重复的 数据 accepted as 新的 在 (b)
什么relationship 之间 seq # 大小 和
窗口 大小到 avoid 问题 在 (b)?
窗口 大小 (W) 上 到 N/2

3-79
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输:
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

3-80
TCP: Overview  RFCs: 793,1122, 2018, 5681, 7323
full duplex data:
bi-directional data flow in same 
connection
MSS: maximum segment size
connection-oriented:
handshaking (exchange of 
control msgs) inits sender, 
receiver state before data 
exchange
flow controlled:
sender will not overwhelm 
receiver
point-to-point:
one sender, one receiver
reliable, in-order byte steam:
no “message boundaries”
pipelined:
TCP congestion and flow control 
set window size
What is the purpose 
of the connection?
TCP: 概述 RFCs:
完整的 duplex 数据:
bi-directional 数据 流 在 相同
连接
MSS: maximum 报文段 大小
面向连接:
握手 (交换 的
控制 msgs) inits 发送方,
接收方 状态 之前 数据
交换
流量控制led:
发送方 将 不 overwhelm
接收方
一个 发送方, 一个 接收方
可靠, in-order字节 steam:
无 “报文 boundaries”
TCP 拥塞 和 流量控制
集合 窗口 大小
什么 是 purpose
的 连接?

3-81
TCP segment structure
source port #
dest port #
32 bits
application
data 
(variable length)
sequence number
acknowledgement number
receive window
Urg data pointer
checksum
F
SR
PAU
head
len
not
used
options (variable length)
URG: urgent data 
(generally not used)
ACK: ACK #
valid
PSH: push data now
(generally not used)
RST, SYN, FIN:
connection estab
(setup, teardown
commands)
# bytes 
rcvr willing
to accept
counting
by bytes 
of data
(not segments!)
Internet
checksum
(as in UDP)
TCP 报文段结构
源 端口 #
dest 端口 #
32 比特
应用
数据
(变量 长度)
序列 编号
acknowledgement 编号
接收 窗口
Urg 数据 pointer
校验和
不
用于
options (变量 长度)
URG: urgent 数据
(generally 不 用于)
PSH: push 数据 现在
(generally 不 用于)
连接 estab
# 字节
到 accept
由 字节
的 数据
(不 报文段!)
校验和
(as 在 UDP)

3-82
TCP seq. numbers, ACKs
sequence numbers:
byte stream “number” of first byte in 
segment’s data
acknowledgements:
seq # of NEXT byte expectedfrom other side
cumulative ACK
Q:how receiver handles out-of-order segments
A: TCP spec doesn’t say, - up to 
implementor
source port #
dest port #
sequence number
acknowledgement number
checksum
rwnd
urg pointer
incoming segment to sender
A
sent 
ACKed
sent, not-
yet ACKed
(“in-
flight”)
usable
but not 
yet sent
not 
usable
window size
N
sender sequence number space 
source port #
dest port #
sequence number
acknowledgement number
checksum
rwnd
urg pointer
outgoing segment from sender
Neither GBN nor selective repeat 
TCP seq. 编号, ACK
序列 编号:
字节 stream “编号” 的 第一个 字节 在
报文段’s 数据
seq # 的NEXT 字节 expected从 其他 一侧
如何 接收方 处理 out-of-order 报文段
: TCP spec doesn’t say, - 上 到
源 端口 #
dest 端口 #
序列 编号
acknowledgement 编号
校验和
传入的 报文段 到 发送方
已发送
已发送, not-
但是 不
yet 已发送
不
窗口 大小
发送方 序列 编号 空间
源 端口 #
dest 端口 #
序列 编号
acknowledgement 编号
校验和
outgoing 报文段 从 发送方

3-83
TCP seq. numbers, ACKs
User
types
‘C’
host ACKs
receipt 
of echoed
‘C’
host ACKs
receipt of
‘C’, echoes
back ‘C’
simple telnet scenario
Host B
Host A
Seq=42, ACK=79, data = ‘C’
Seq=79, ACK=43, data = ‘C’
Seq=43, ACK=80
Seq: 当前序号；
Ack: 待接收序号；
TCP seq. 编号, ACK
用户
主机 ACK
的 echoed
主机 ACK
receipt 的
simple telnet 场景
主机 B
主机
, 数据 =
, 数据 =

3-84
TCP round trip time, timeout
Q:how to set TCP timeout 
value?
longer than RTT
but RTT varies
too short:premature timeout, 
unnecessary retransmissions
too long:slow reaction to 
segment loss
Q:how to estimate RTT?
SampleRTT:measured time from 
segment transmission until ACK 
receipt
ignore retransmissions
SampleRTTwill vary, want estimated 
RTT “smoother”
average several recent
measurements, not just current 
SampleRTT
RTT (milliseconds)
RTT (milliseconds)
TCP round trip 时间, 超时
如何 到 集合 TCP超时
值
longer 比 RTT
但是 RTT varies
premature 超时,
slow reaction 到
报文段 丢包
如何 到 估计 RTT?
measured 时间 从
报文段 传输 直到 ACK
将 vary, want estimated
measurements, 不 仅 当前

Transport Layer
3-85
RTT: gaia.cs.umass.edu to fantasia.eurecom.fr
100
150
200
250
300
350
1815222936435057647178859299106
time (seconnds)
RTT (milliseconds)
SampleRTTEstimated RTT
EstimatedRTT = (1-)*EstimatedRTT + *SampleRTT
exponential weighted moving average
influence of past sample decreases exponentially fast
typical value: =0.125
TCP round trip time, timeout
RTT (milliseconds)
RTT: gaia.cs.umass.edu to fantasia.eurecom.fr
sampleRTT
EstimatedRTT
time (seconds)
运输层
RTT: gaia.cs.umass.edu 到 fantasia.eurecom.fr
时间 (seconnds)
influence 的 past sample decreases exponentially 快速
typical 值:
TCP round trip 时间, 超时
RTT: gaia.cs.umass.edu 到 fantasia.eurecom.fr
时间 (seconds)

3-86
timeout interval:EstimatedRTTplus “safety margin”
large variation in EstimatedRTT ->larger safety margin
estimate SampleRTT deviation from EstimatedRTT: 
DevRTT = (1-)*DevRTT +
*|SampleRTT-EstimatedRTT|
TCP round trip time, timeout
(typically, = 0.25)
TimeoutInterval = EstimatedRTT + 4*DevRTT
estimated RTT
“safety margin”
超时 interval:
加
大的 variation 在
估计 SampleRTT deviati在 从 EstimatedRTT:
TCP round trip 时间, 超时
(通常,

3-87
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输: UDP
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

3-88
TCP reliable data transfer
TCP creates rdt service on top of 
IP’s unreliable service
pipelined segments
cumulative acks
single retransmission timer
retransmissions  triggered by:
timeout events
duplicate acks
Let’s initially consider 
simplified TCP sender:
ignore duplicate acks
ignore flow control, congestion 
control
retransmission
TCP 可靠数据传输
TCP 创建 rdt 服务 在 top 的
IP’s unreliable 服务
pipelined 报文段
单个 重传 计时器
retransmissions triggered 由:
超时 events
重复的 ACK
Let’s 初始时 consider
simplified TCP 发送方:
ignore 重复的 ACK
ignore 流量控制, 拥塞
控制
重传

3-89
TCP sender events:
data rcvd from app:
create segment with seq #
seq # is byte-stream number
of first data byte in  segment
start timer if not already 
running 
think of timer as for oldest 
unacked segment
expiration interval: 
TimeOutInterval
timeout:
retransmit segment that caused 
timeout
restart timer
ack rcvd:
if ack acknowledges previously 
unacked segments
update what is known to be ACKed
start timer if there are  still 
unacked segments
TCP 发送方 events:
数据 rcvd 从 应用:
create 报文段 与 seq #
seq # 是byte-stream 编号
的 第一个 数据 字节 在 报文段
开始 计时器 如果 不 already
运行
think 的 计时器 as 用于 oldest
unacked 报文段
超时:
重传 报文段 这 caused
超时
restart 计时器
如果 ACK acknowledges previously
unacked 报文段
更新 什么 是 已知 到 是 ACKed
开始 计时器 如果 存在 是 仍然
unacked 报文段

3-90
TCP sender (simplified)
wait
for 
event
NextSeqNum = InitialSeqNum
SendBase = InitialSeqNum

create segment, seq. #: NextSeqNum
pass segment to IP (i.e., “send”)
NextSeqNum = NextSeqNum + length(data) 
if (timer currently not running)
start timer
data received from application above
retransmit not-yet-acked segment         
with smallest seq. #
start timer
timeout
if (y > SendBase) { 
SendBase = y 
/* SendBase–1: last cumulatively ACKed byte */
if (there are currently not-yet-acked segments)
start timer
else stop timer 
} 
ACK received, with ACK field value y 
timeout
timeout
TCP 发送方
等待
用于
create 报文段, seq. #: NextSeqNum
pass 报文段 到 IP (i.e.,
发送
eqNum + 长度(数据)
如果 (计时器 currently 不 运行)
开始 计时器
数据 已收到 从 应用 上方
重传 not-yet-acked 报文段
与 smallest seq. #
开始 计时器
超时
如果 (y > SendBase) {
/* SendBase–1: 最后 cumulatively ACKed 字节 */
如果 (存在 是 currently
not-yet-acked 报文段)
开始 计时器
否则 stop 计时器
ACK 已收到, 与 ACK 字段 值 y

3-91
TCP: retransmission scenarios
lost ACK scenario
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=92, 8 bytes of data
X
timeout
ACK=100
premature timeout
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=92,  8
bytes of data
timeout
ACK=120
Seq=100, 20 bytes of data
ACK=120
SendBase=100
SendBase=120
SendBase=120
SendBase=92
TCP: 重传 scenarios
丢失的 ACK 场景
主机 B
主机
Seq=92, 8 字节 的 数据
Seq=92, 8 字节 的 数据
超时
premature 超时
主机 B
主机
Seq=92, 8 字节 的 数据
字节 的 数据
超时
Seq=100, 20 字节 的 数据

3-92
TCP: retransmission scenarios
X
cumulative ACK
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=120,  15 bytes of data
timeout
Seq=100, 20 bytes of data
ACK=120
TCP: 重传 scenarios
主机 B
主机
Seq=92, 8 字节 的 数据
15 字节 的 数据
超时
Seq=100, 20 字节 的 数据

timeout
3-93
TCP ACK generation [RFC 1122, RFC 2581]
event at receiver
arrival of in-order segment with
expected seq #. All data up to
expected seq # already ACKed
arrival of in-order segment with
expected seq #. One other 
segment has ACK pending
arrival of out-of-order segment
higher-than-expect seq. # .
Gap detected
arrival of segment that 
partially or completely fills gap
TCP receiver action
delayed ACK. Wait up to 500ms
for next segment. If no next segment,
send ACK
immediately send single cumulative 
ACK, ACKing both in-order segments 
immediately sendduplicate ACK,
indicating seq. # of next expected byte
immediate sendACK, provided that
segment starts at lower end of gap
event 在 接收方
arrival 的 in-order 报文段 与
expected seq #. 所有 数据 上 到
arrival 的 in-order 报文段 与
expected seq #. 一个 其他
报文段 有 ACK pending
arrival 的 out-of-order 报文段
arrival 的 报文段 这
partially 或 completely fills gap
TCP 接收方 动作
时延ed ACK. 等待 上 到
用于 下一个 报文段. 如果 无 下一个 报文段,
发送 ACK
immediately 发送单个
ACK, ACKing 二者 in-order 报文段
immediately 发送重复的 ACK
indicating seq. # 的 下一个 expected 字节
immediate 发送ACK, 提供 这
报文段 starts 在 lower 端 的 gap

3-94
TCP fastretransmit
time-out period  often relatively 
long:
long delay before resending lost 
packet
detect lost segments via 
duplicate ACKs.
sender often sends many 
segments back-to-back
if segment is lost, there will likely 
be many duplicate ACKs.
if sender receives 3ACKs for 
same data 
(“triple duplicate 
ACKs”),
resend unacked
segment with smallest seq #
likely that unacked segment lost, 
so don’t wait for timeout
TCP fast retransmit
Time out
1
2
3
4
1
1
1
1
ACK(?)
timeout
快速重传
time-out period 经常 relatively
long 时延 之前 resending 丢失的
分组
检测 丢失的 报文段 通过
重复的 ACK
发送方 经常发送 许多
报文段 back-to-back
如果 报文段 是 丢失的, 存在 将 likely
是 许多 重复的 ACK.
如果 发送方 接收ACK 用于
相同 数据
triple 重复的
报文段 与 smallest seq #
likely 这 unacked 报文段 丢失的,
因此 don’t 等待 用于 超时
TCP 快速 重传
时间 查看

3-95
X
fast retransmit after sender 
receipt of triple duplicate ACK
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
timeout
ACK=100
ACK=100
ACK=100
TCP fast retransmit
Seq=100, 20 bytes of data
Seq=100, 20 bytes of data
快速 重传 之后 发送方
receipt 的 triple 重复的 ACK
主机 B
主机
Seq=92, 8 字节 的 数据
超时
TCP 快速 重传
Seq=100, 20 字节 的 数据
Seq=100, 20 字节 的 数据

3-96
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented transport: 
TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输:
3.4 原理 的 可靠 数据
传输
3.5 面向连接的传输:
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

3-97
TCP flow control
application
process
TCP socket
receiver buffers
TCP
code
IP
code
application
OS
receiver protocol stack
application may 
remove data from 
TCP socket buffers .... 
... slower than 
TCP 
receiver is 
delivering
(sender is 
sending)
from sender
receiver controls sender, so 
sender won’t overflow 
receiver’s buffer by 
transmitting too much, too fast
flow control
TCP 流量控制
应用
进程
TCP 套接字
接收方 缓冲区
码
码
应用
接收方 协议 stack
应用 可能
remove 数据 从
TCP 套接字 缓冲区 ....
... slower 比
接收方 是
(发送方 是
发送)
从 发送方
接收方 controls 发送方
, 因此
发送方 won’t overflow
接收方’s 缓冲区 由
transmitting too much, too 快速
流量控制

3-98
TCP flow control
buffered data
free buffer space
rwnd
RcvBuffer
TCP segment payloads
to application process
receiver “advertises”free buffer space 
by including rwndvalue in TCP header 
of receiver-to-sender segments
RcvBuffersize set via socket options 
(typical default is 4096 bytes)
many operating systems autoadjust
RcvBuffer
sender limits amount of unacked (“in-
flight”) data to receiver’s rwndvalue 
guarantees receive buffer will not 
overflow
receiver-side buffering
TCP 流量控制
buffered 数据
free 缓冲区 空间
TCP 报文段 payloads
到 应用 进程
接收方free 缓冲区 空间
由 包括值 在 TCP 首部
的 接收方-to-发送方 报文段
大小 集合 通过 套接字 options
(typical default 是 4096 字节)
许多 operating 系统 autoadjust
发送方 limits 数量 的 unacked (
) 数据 到 接收方’s
值
guarantees 接收 缓冲区 将 不
接收方-side buffering

3-99
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输: UDP
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

3-100
Connection Management
before exchanging data, sender/receiver “handshake”:
agree to establish connection (each knowing the other willing to establish 
connection)
agree on connection parameters
connection state: ESTAB
connection variables:
seq # client-to-server
server-to-client
rcvBuffersize
at server,client 
application
network
connection state: ESTAB
connection Variables:
seq # client-to-server
server-to-client
rcvBuffersize
at server,client 
application
network
Socket clientSocket =   
newSocket("hostname","port 
number");
Socket connectionSocket = 
welcomeSocket.accept();
连接管理
之前 exchanging 数据, 发送方/接收方握手
agree 到 establish 连接 (每个 knowing 其他 willing t
连接)
agree 在 连接 parameters
连接 状态: ESTAB
连接 variables:
seq # 客户-to-服务器
服务器-to-客户
大小
在 服务器,客户
应用
网络
连接 状态: ESTAB
连接 Variables:
seq # 客户-to-服务器
服务器-to-客户
大小
在 服务器,客户
应用
网络
套接字
newSocket("hostname","端口
编号");
套接字

3-101
Q:will 2-way handshake always 
work in network?
variable delays
retransmitted messages (e.g. 
req_conn(x)) due to message loss
message reordering
Can’t “see”other side
2-way handshake:
Let’s talk
OK
ESTAB
ESTAB
choose x
req_conn(x)
ESTAB
ESTAB
acc_conn(x)
Agreeing to establish a connection
2023.9.27
将 2-way 握手 总是
工作 在 网络?
变量 时延s
retransmitted 报文 (e.g.
req_conn(x)) 由于 到 报文 丢包
报文 reordering
可以’t
见其他 一侧
2-way 握手:
选择 x
Agreeing 到 establish a 连接

3-103
Agreeing to establish a connection
2-way handshake failure scenarios:
retransmit
req_conn(
x)
ESTAB
req_conn(x)
half open connection!
(no client!)
client 
terminates
server
forgets x
connection 
x completes
retransmit
req_conn(
x)
ESTAB
req_conn(x)
data(x+1)
retransmit
data(x+1)
accept
data(x+1)
choose x
req_conn(x)
ESTAB
ESTAB
acc_conn(x)
client 
terminates
ESTAB
choose x
req_conn(x)
ESTAB
acc_conn(x)
data(x+1)
accept
data(x+1)
connection 
x completes
server
forgets x
Agreeing 到 establish a 连接
2-way 握手 failure scenarios:
重传
half open 连接!
(无 客户!)
客户
服务器
连接
重传
数据(x+1)
重传
数据(x+1)
数据(x+1)
选择 x
客户
选择 x
数据(x+1)
数据(x+1)
连接
服务器

3-104
TCP 3-way handshake
choose init seq num, x
send TCP SYN msg
SYNbit=1, Seq=x
ESTAB
choose init seq num, y
send TCP SYNACK
msg, acking SYN
SYNbit=1, Seq=y
ACKbit=1; ACKnum=x+1
received SYNACK(x) 
indicates server is live;
send ACK for SYNACK;
this segment may contain 
client-to-server data
received ACK(y) 
indicates client is live
ACKbit=1, ACKnum=y+1
SYNSENT
ESTAB
SYN RCVD
client state
LISTEN
server state
LISTEN
TCP 3-way 握手
选择 init seq num, x
发送 TCP SYN msg
选择 init seq num, y
发送 TCP SYNACK
已收到 SYNACK(x)
indicates 服务器 是 live;
发送 ACK 用于 SYNACK;
这个 报文段 可能 contain
客户-to-服务器 数据
已收到 ACK(y)
indicates 客户 是 live
客户 状态
服务器 状态

3-105
TCP 3-way handshake: FSM
closed

listen
SYN
rcvd
SYN
sent
ESTAB
Socket clientSocket =   
newSocket("hostname","port 
number");
SYN(seq=x)
Socket connectionSocket = 
welcomeSocket.accept();
SYN(x)
SYNACK(seq=y,ACKnum=x+1)
create new socket for 
communication back to client
SYNACK(seq=y,ACKnum=x+1)
ACK(ACKnum=y+1)
ACK(ACKnum=y+1)

Client-sideserver-side
TCP 3-way 握手: FSM
已发送
套接字
newSocket("hostname","端口
编号");
套接字
create 新的 套接字 用于
通信 back 到 客户
客户-side
服务器-side

3-106
TCP: closing a connection
client, server each close their side of 
connection
send TCP segment with FIN bit = 1
respond to received FIN with ACK
on receiving FIN, ACK can be combined with 
own FIN
simultaneous FIN exchanges can be 
handled
TCP: closing a 连接
客户, 服务器 每个 关闭 它们的 一侧 的
连接
发送 TCP 报文段 与 FIN 比特 = 1
respond 到 已收到 FIN 与 ACK
在 接收 FIN, ACK 可以 是 combined 与
自己的 FIN
simultaneous FIN exchanges 可以 是

3-107
FIN_WAIT_2
CLOSE_WAIT
FINbit=1, seq=y
ACKbit=1; ACKnum=y+1
ACKbit=1; ACKnum=x+1
wait for server
close
can still
send data
can no longer
send data
LAST_ACK
CLOSED
TIMED_WAIT
timed wait 
for 2*max 
segment lifetime
CLOSED
TCP: closing a connection
FIN_WAIT_1
FINbit=1, seq=x
can no longer
send but can
receive data
clientSocket.close()
client state
server state
ESTAB
ESTAB
The key idea of TCP: ACK is necessary for each sent segment.
等待 用于 服务器
关闭
可以 仍然
发送 数据
可以 无 longer
发送 数据
timed 等待
用于 2*max
报文段 lifetime
TCP: closing a 连接
可以 无 longer
发送 但是 可以
接收 数据
客户 状态
服务器 状态
关键 思想 的 TCP: ACK 是 必要时 用于 每个 已发送 报文段.

3-108
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输:
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

拥塞控制所起的作用
提供的负载
吞吐量
理想的拥塞控制
实际的拥塞控制
0
死锁（吞吐量= 0）
无拥塞控制
拥塞
轻度
拥塞

3-110
congestion:
informally: “too many sources sending too much data too fast 
for 
networkto handle”
different from flow control!
manifestations:
lost packets (buffer overflow at routers)
long delays (queueing in routerbuffers)
a top-10 problem!
Principles of congestion control
拥塞
too 许多 sources 发送too much 数据 too 快速
用于
网络
到 处理
不同的 从 流量控制!
丢失的 分组
(缓冲区 overflow 在
路由器
long 时延s(排队 在路由器缓冲区)
a top-10 问题!
原理 的 拥塞控制

3-111
Causes/costs of congestion: scenario 1 
two senders, two receivers
one router, infinite buffers 
output link capacity: R
no retransmission
maximum per-connection 
throughput: R/2
unlimitedshared 
output link buffers
Host A
original data: 
in
Host B
throughput:

out
R/2
R/2

out

in
R/2
delay

in
large delays as arrival rate, 

in
, approaches capacity
Due to infinite 
buffers
Causes/costs 的 拥塞: 场景 1
两个 senders, 两个 receivers
一个 路由器,
infinite 缓冲区
输出 链路 容量: R
无 重传
吞吐量: R/2
共享
输出 链路 缓冲区
主机
原始 数据:
在
主机 B
吞吐量:
查看
查看
在
时延
在
大的 时延s as arrival 速率,
在
, 方法 容量
由于 到 infinite
缓冲区


out
delay
3-112
one router, finitebuffers 
sender retransmissionof timed-outpacket
application-layer input = application-layer output:
in
= 
out
transport-layer input includes retransmissions:′
in

in
finite shared 
output link buffers
Host A

in
: original data
Host B

out
'
in
: original data, plus
retransmitted data
‘
Causes/costs of congestion: scenario 2 
无策略的重传导致网络中数据继续增加， 拥塞更加严重！
一个 路由器,
缓冲区
发送方重传的分组
应用-layer 输入 =应用-layer 输出:
在
查看
运输层 输入 includes
在在
共享
输出 链路 缓冲区
主机
在
: 原始 数据
主机 B
查看
在
: 原始 数据,加
retransmitted 数据
Causes/costs 的 拥塞: 场景 2

(Sender should retransmits timed-out pkts at the right time!!!)
3-113
idealization: perfect knowledge
sender sends only when router buffers available 
finite shared output 
link buffers

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
copy
free buffer space!
R/2
R/2

out

in
Causes/costs of congestion: scenario 2 
Host B
A
Considering sender’s retransmission
选择合适的时
机进行重传
(发送方 should retransmits timed-out pkts 在 right 时间!!!)
发送方 发送 仅 当 路由器缓冲区 可用的
finite 共享 输出
链路 缓冲区
在
: 原始 数据
查看
在
: 原始 数据,
加
数据
复制
free 缓冲区 空间!
查看
在
Causes/costs 的 拥塞: 场景 2
主机 B
Considering 发送方’s重传


out
3-114

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
copy
no buffer space!
Idealization: known pkts are indeed lost
packets can be lost, dropped at router due  to full buffers
sender only resends if packet knownto be lost instead of timed-out.
Causes/costs of congestion: scenario 2 
A
Host B
Considering sender’s retransmission
Time-out ് loss
在
: 原始 数据
查看
在
: 原始 数据,
加
数据
复制
无 缓冲区 空间!
已知 pkts 是 indeed 丢失的
分组 可以 是 丢失的, dropped 在 路由器 由于 到 完整的 缓冲区
发送方 仅 resends 如果分组已知
到 是 丢失的
instead 的 timed-out.
Causes/costs 的 拥塞: 场景 2
主机 B
Considering 发送方’s重传
丢包

3-115

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
free buffer space!
Causes/costs of congestion: scenario 2 
Idealization: known losspackets 
can be lost, dropped at router due  to 
full buffers
sender only resends if packet known
to be lost
R/2
R/2

in

out
when sending at R/2, 
some packets are 
retransmissions but 
asymptotic goodput
is still R/2 (why?)
A
Host B
选择丢失分组
进行精准重传

out
在
: 原始 数据
查看
在
: 原始 数据,
加
数据
free 缓冲区 空间!
Causes/costs 的 拥塞: 场景 2
已知 丢包
分组
可以 是 丢失的, dropped 在 路由器 由于 到
完整的 缓冲区
发送方 仅 resends 如果 分组已知
到 是 丢失的
在
查看
当 发送 在 R/2,
一些 分组 是
retransmissions 但是
是 仍然 R/2 (
主机 B


out
3-116
A

in

out
'
in
copy
free buffer space!
timeout
R/2
R/2

in

out
when sending at R/2, 
some packets are 
retransmissions 
including duplicated 
that are delivered!
Host B
Realistic: duplicates
packets can be lost, dropped at router due  to 
full buffers
sender times out prematurely, sending two 
copies, both of which are delivered
Causes/costs of congestion: scenario 2 
在
查看
在
复制
free 缓冲区 空间!
超时
在
查看
当 发送 在 R/2,
一些 分组 是
包括 duplicated
这 是 delivered!
主机 B
分组 可以 是 丢失的, dr
opped 在 路由器 由于 到
完整的 缓冲区
发送方 times 查看 prematurely, 发送两个
, 二者 的 哪个 是 delivered
Causes/costs 的 拥塞: 场景 2

3-117
R/2

out
when sending at R/2, 
some packets are 
retransmissions 
including duplicated 
that are delivered!
“costs” of congestion:
more work (retrans) for given “goodput”
unneeded retransmissions: link carries multiple copies of pkt
decreasing goodput
R/2

in
Causes/costs of congestion: scenario 2 
Realistic: duplicates
packets can be lost, dropped at router 
due  to full buffers
sender times out prematurely, sending 
two copies, both of which are delivered
查看
当 发送 在 R/2,
一些 分组 是
包括 duplicated
这 是 delivered!
“费用” 的 拥塞:
更多 工作 (retrans) 用于 给定的 “goodput”
: 链路 carries 多路 copies 的 pkt
在
Causes/costs 的 拥塞: 场景 2
分组 可以 是 丢失的, dropped 在 路由器
由于 到 完整的 缓冲区
发送方 times 查看 prematurely, 发送
两个
, 二者 的 哪个 是 delivered


out
3-118
four senders
multihop paths
timeout/retransmit
Q:what happens as 
in
and 
in
’
increase?
finite shared output 
link buffers
Host A

out
Causes/costs of congestion: scenario 3 
Host B
Host C
Host D

in
: original data
'
in
: original data, plus
retransmitted data
A:as red  
in
’
increases, 
all arriving bluepkts at 
upper queue are 
dropped, blue throughput 
0
multihop 路径
什么 happens as
在
和
在
增大
finite 共享 输出
链路 缓冲区
主机
查看
Causes/costs 的 拥塞: 场景 3
主机 B
主机 C
主机 D
在
: 原始 数据
在
: 原始 数据,加
retransmitted 数据
在
所有 到达的
pkts 在
upper 队列 是
dropped, blue 吞吐量

3-119
another “cost”of congestion:
when packet dropped, any upstream transmission capacity used 
for that packet was wasted!
Cause upstream congestion!
Causes/costs of congestion: scenario 3 
C/2
C/2

out

in
’
另一个费用的 拥塞:
当 分组 dropped,任何 上游传输 容量 用于
用于 这 分组 was
Cause 上游 拥塞!
Causes/costs 的 拥塞: 场景 3
查看
在


out
3-120
Approaches towards congestion control
two broad approaches towards congestion control:
end-end congestion 
control:
no explicit feedback 
from network
congestion inferred 
from end-system 
observed loss, delay
approach taken by TCP
network-assisted 
congestion control:
routers provide feedback to 
end systems
single bit indicating 
congestion (SNA, DECbit, 
TCP/IP ECN, ATM)
explicit rate for sender to 
send at
方法 towards 拥塞控制
两个 broad 方法 towards 拥塞控制:
end-end 拥塞
控制:
无 explicit feedback
从 网络
拥塞 inferred
从 end-system
observed 丢包, 时延
方法 经过 由 TCP
拥塞控制:
路由器 提供 feedback 到
端 系统
单个 比特 indicating
拥塞 (SNA, DECbit,
explicit 速率 用于 发送方 到
发送 在

3-121
Case study: ATM ABR congestion control
ABR: available bit rate:
“elastic service”
if sender’s path “underloaded”: 
sender should use available 
bandwidth
if sender’s path congested: 
sender throttled to minimum 
guaranteed rate
RM (resource management) cells:
sent by sender, interspersed with 
data cells
bits in RM cell set by switches 
(“network-assisted
”) 
NI bit:no increase in rate (mild 
congestion)
CI bit:congestion indication
RM cells returned to sender by 
receiver, with bits intact
Case study: ATM ABR 拥塞控制
ABR: 可用的 比特 速率:
elastic 服务
如果 发送方’s 路径
发送方 should 使用 可用的
带宽
如果 发送方’s 路径 拥塞的:
发送方 throttled 到
guaranteed 速率
RM (resource 管理) cells:
已发送 由 发送方, interspersed 与
数据 cells
比特 在 RM 小区 集合 由 交换机
NI 比特:
无 增大 在 速率 (mild
拥塞)
CI 比特:
拥塞 indication
returned 到 发送方由
接收方, 与 比特 intact

3-122
Case study: ATM ABR congestio control
two-byte ER (explicit rate) field in RM cell
congested switch may lower ER value in cell
Senders’ send rate thus max supportable rate on path
EFCI bit in data cells: set to 1 in congested switch
if data cell preceding RM cell has EFCI set, receiversets CI bit in 
returned RM cell
RM celldata cell
专用控制
分组方式,
占用带宽
Case study: ATM ABR congestio 控制
two-byte ER (explicit 速率) 字段 在 RM 小区
拥塞的 交换机 可能 lower ER 值 在 小区
Senders’ 发送 速率 thus 最大值 supportable 速率 在 路径
EFCI 比特 在 数据 cells: 集合 到 1 在 拥塞的 交换机
如果 数据 小区 preceding RM ce
ll 有 EFCI 集合, 接收方
集合 CI 比特
在
returned RM 小区
RM 小区
数据 小区

Transport Layer
3-123
network-assisted congestion control:
two bits in IP header (ToS field) marked by network routerto indicate 
congestion
congestion indication carried to receiving host
receiver (seeing congestion indication in IP datagram) ) sets ECE bit on 
receiver-to-sender ACK segmentto notify sender of congestion
Explicit Congestion Notification (ECN)
source
application
transport
network
link
physical
destination
application
transport
network
link
physical
ECN=00
ECN=11
ECE=1
IP datagram
TCP ACK segment
捎带方式,
节省带宽
运输层
network-assisted 拥塞控制:
两个 比特 在 IP 首部 (ToS 字段)由 网络 路由器
到 indicate
拥塞
拥塞 indication carried 到 接收 主机
接收方 (seeing 拥塞 indication 在 IP 数据报) ) 集合 E
CE 比特 在
接收方-to-发送方 ACK 报文段到 通知 发送方 的 拥塞
Explicit 拥塞 Notification (ECN)
源
应用
运输
网络
链路
物理
目的地
应用
运输
网络
链路
物理
IP 数据报
TCP ACK 报文段

3-124
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control
第 3 章：提纲
3.1 运输层 服务
3.2 多路复用 和
多路分解
3.3 无连接传输:
3.4 原理 的 可靠 数据
传输
3.5 面向连接
运输: TCP
报文段结构
可靠数据传输
流量控制
连接管理
3.6 原理 的 拥塞
控制
3.7 TCP 拥塞控制

3-125
TCP congestion control: additive increase 
multiplicative decrease
approach:sender increases transmission rate (window size), probing 
for usable bandwidth, until loss occurs
additive increase:increase  cwndby 1 MSS every RTT until loss 
detected
multiplicative decrease:cut cwndin halfafter loss 
cwnd:
TCP sender 
congestion window size
AIMD saw tooth
behavior: probing
for bandwidth
additively increase window size ...
.... until loss occurs (then cut window in half)
time
TCP 拥塞控制: additive 增大
multiplicative 减小
方法:发送方 increases 传输 速率 (窗口 大小), probing
用于 usable 带宽, 直到 丢包 occurs
additive 增大:增大由1 MSS 每一个 RTT
直到 丢包
multiplicative 减小
在
之后 丢包
TCP 发送方
拥塞 窗口 大小
用于 带宽
additively 增大 窗口 大小 ...
.... 直到 丢包 occurs (则 cut 窗口 在 half)
时间

cwnd:
TCP sender 
congestion window size
3-126
TCP Congestion Control: details
sender limits transmission:
cwndis dynamic, function of perceived network congestion
TCP sending rate:
roughly:send cwnd bytes, wait 
RTT for ACKS, then send more 
bytes
last byte
ACKed
sent, not-
yet ACKed
(“in-
flight”)
last byte 
sent
cwnd
sender sequence number space 
rate
~
~
cwnd
RTT
bytes/sec
减小窗口→降低速率→减少网络数据→缓解拥塞
TCP 拥塞控制: details
发送方 limits 传输:
是动态, 功能 的 perceived 网络 拥塞
TCP 发送 速率:
发送 cwnd 字节, 等待
RTT 用于 ACK, 则 发送 更多
字节
最后 字节
已发送, not-
最后 字节
已发送
发送方 序列 编号 空间
速率

3-127
TCP Slow Start 
when connection begins, increase 
rate exponentially until first loss 
event:
initially cwnd= 1 MSS
doublecwndevery RTT
done by incrementing cwndfor every 
ACK received
summary:initial rate is slowbut 
ramps up exponentially fast
Host A
RTT
Host B
time
TCP Slow 开始
当 连接 begins,增大
速率 exponentially
直到 第一个 丢包
初始时
每一个 RTT
done 由 incrementing
用于 每一个
ACK 已收到
总结:初始 速率 是 slow
但是
ramps 上 exponentially 快速
主机
主机 B
时间

RTT
3-128
TCP: detecting, reacting to loss
loss indicated by timeout:
cwndset to 1 MSS; 
window then grows exponentially(as in slow start) to 
threshold, then grows linearly
loss indicated by 3 duplicate ACKs: TCP RENO
dup ACKs indicate network capable of  delivering some 
segments 
cwndis cut in half window then grows linearly
TCP Tahoe always sets cwndto 1 (timeout or 3 duplicate 
acks)
TCP: detecting, reacting 到 丢包
丢包 indicated 由超时
集合 到 1 MSS;
窗口 则 grows
(as 在 slow 开始) 到
threshold, 则 grows linearly
丢包 indicated 由3 重复的 ACK
dup ACK indicate 网络 capable 的 delivering 一些
报文段
是
cut 在 half
窗口 则 grows
TCP Tahoe 总是 集合到 1 (超时 或 3 重复的

3-129
Q:when should the exponential 
increase switch to linear? 
A:when cwndgets to 1/2 of its 
value before timeout.
Implementation:
variable ssthresh
on loss event, ssthreshis set to 1/2 of cwndjust 
before loss event
TCP: switching from slow start to CA
Cwnd >= ssthresh
Ssthresh=1/2 Cwnd
当 should exponential
增大 交换机 到 linear?
当
获得 到 1/2 的 它的
值 之前 timeout.
实现:
变量
在 丢包 event,
是 集合 到
1/2 的
仅
之前 丢包 event
TCP: 交换 从 slow 开始 到 CA

3-130
Summary: TCP Congestion Control
timeout
ssthresh = cwnd/2
cwnd = 1 MSS
dupACKcount = 0
retransmit missing segment

cwnd > ssthresh
congestion
avoidance 
cwnd = cwnd + MSS    (MSS/cwnd)
dupACKcount = 0
transmit new segment(s), as allowed
new ACK
.
dupACKcount++
duplicate ACK
fast
recovery 
cwnd = cwnd + MSS
transmit new segment(s), as allowed
duplicate ACK
ssthresh= cwnd/2
cwnd = ssthresh + 3
retransmit missing segment
dupACKcount == 3
timeout
ssthresh = cwnd/2
cwnd = 1 
dupACKcount = 0
retransmit missing segment
ssthresh= cwnd/2
cwnd = ssthresh + 3
retransmit missing segment
dupACKcount == 3
cwnd = ssthresh
dupACKcount = 0
New ACK
slow 
start
timeout
ssthresh = cwnd/2 
cwnd = 1 MSS
dupACKcount = 0
retransmit missing segment
cwnd = cwnd+MSS
dupACKcount = 0
transmit new segment(s), as allowed
new ACK
dupACKcount++
duplicate ACK

cwnd = 1 MSS
ssthresh = 64 KB
dupACKcount = 0
New
ACK!
New
ACK!
New
ACK!
Increase one MSS 
when  ALL acks of a 
cwndare received
Very Important!!!
Increase one MSS 
for each ack
总结: TCP 拥塞控制
超时
重传 missing 报文段
拥塞
传输 新的 报文段(s), as allowed
新的 ACK
重复的 ACK
快速
传输 新的 报文段(s), as allowed
重复的 ACK
重传 missing 报文段
超时
重传 missing 报文段
重传 missing 报文段
新的 ACK
开始
超时
重传 missing 报文段
传输 新的 报文段(s), as allowed
新的 ACK
重复的 ACK
新的
新的
新的
增大 一个 MSS
当ALL ACK 的 a
是 已收到
Very 重要!!!
增大 一个 MSS
用于
每个 ACK

3-131
TCP throughput
avg. TCP thruput as function of window size, RTT?
ignore slow start, assume always data to send
W: window size (measured in bytes)where loss occurs
avg. window size (# in-flight bytes) is ¾ W
avg. thruput is 3/4W per RTT
W
W/2
avg TCP thruput = 
3
4
W
RTT
bytes/sec
2023.10.9
TCP 吞吐量
avg. TCP thruput as 功能 的 窗口 大小, RTT?
ignore slow 开始, assume 总是 数据 到 发送
W: 窗口 大小
(measured 在 字节)
哪里 丢包 occurs
avg. 窗口 大小 (# in-flight 字节) 是   W
avg. thruput 是 3/4W 每 RTT

TCP BIC
TCP RENO 的问题： 窗口恢复慢
132
Congestion
avoidance
Slow
start
Congestion
avoidance
在reno版本的拥塞控制下，进入拥塞避免状态或快速恢复状态后，每经过一个RTT才会将窗口
大小加1，如果链路状况是好的，但RTT很长，reno需要很长时间才能达到最佳拥塞窗口
拥塞
开始
拥塞

TCP BIC
133
•丢包时的拥塞窗口大小为Wmax
•减小后的窗口为Wmin
• Wmin<W<Wmax
• reno采用加法去搜索W的方式实在是太慢了，因此BIC采用了二
分搜索的方式去找到这个W

TCP CUBIC
TCP BIC存在的问题：BIC不能用到RTT较小，丢包较高的环境中
134

TCP CUBIC
思路：把窗口调整与RTT脱钩，不再以RTT为单位调整窗口
135
3
max
()  (   )Wt   Ct  KW
t是从窗口上次降低开始到现在的时间,K是从窗口降低到再次到达Wmax的时间. 
t=0,W(0)=βWmax
最大值

TCP and the congested “bottleneck link”
TCP (classic, CUBIC) increase TCP’s sending rate until packet loss occurs 
at some router’s output: the bottleneck link
source
application
TCP
network
link
physical
destination
application
TCP
network
link
physical
bottlenecklink (almost always busy)
packet queue almost 
never empty, sometimes 
overflows packet (loss)
Transport Layer: 3-138
TCP 和 拥塞的 “瓶颈 链路”
TCP (classic, CUBIC) 增大 TCP’s 发送 速率 直到 分组 l
在 一些 路由器’s 输出:瓶颈 链路
源
应用
网络
链路
物理
目的地
应用
网络
链路
物理
瓶颈 链路 (almost 总是 busy)
分组 队列 almost
overflows 分组 (丢包)
运输层: 3-138

TCP and the congested “bottleneck link”
TCP (classic, CUBIC) increase TCP’s sending rate until packet loss occurs 
at some router’s output: the bottleneck link
source
application
TCP
network
link
physical
destination
application
TCP
network
link
physical
understanding congestion: useful to focus on congested bottleneck link
insight: increasing TCP sending rate will 
NOTincrease end‐end throughout 
with congested bottleneck
insight: increasing TCP 
sending rate will
increase measured RTT
RTT
Goal: “keep the end‐end pipe just full, but not fuller”
Transport Layer: 3-139
TCP 和 拥塞的 “瓶颈 链路”
TCP (classic, CUBIC) 增大 TCP’s 发送 速率 直到 分组 l
在 一些 路由器’s 输出:瓶颈 链路
源
应用
网络
链路
物理
目的地
应用
网络
链路
物理
understanding 拥塞: 有用 到 focus 在 拥塞的 bottlene
ck 链路
发送 速率 将
增大 端‐端 throughout
与 拥塞的 瓶颈
发送 速率将
增大 measured RTT
目标:
“keep 端‐端 pipe 仅 完整的, 但是 不 fuller”
运输层: 3-139

Delay-based TCP congestion control
Keeping sender‐to‐receiver pipe “just full enough, but no fuller”: keep 
bottleneck link busy transmitting, but avoid high delays/buffering
RTT
measured
Delay‐based approach:
RTT
min
‐minimum observed RTT(uncongested path)
uncongested throughput with congestion window cwndis cwnd/RTT
min
if measured throughput “very close” to  uncongested throughput
increase cwndlinearly                /* since path not congested */ 
else if measured throughput “far below” uncongested throughout
decrease cwndlinearly /* since path is congested */
RTT
measured
measured 
throughput
=
# bytes sent in 
last RTT interval
Transport Layer: 3-140
时延-based TCP
Keeping 发送方‐到‐接收方 pipe “仅 完整的 enough, 但是 无 fulle
瓶颈 链路 busy transmitting, 但是 avoid high 时延s/缓冲区
时延‐基于 方法:
最小值
‐ minimum observed RTT (uncongested 路径)
uncongested 吞吐量 与 拥塞 窗口
是 cwnd/RTT
最小值
如果 measured 吞吐量 “very 关闭” 到 uncongested 吞吐量
增大/* since 路径 不 拥塞的 */
否则 如果 measured 吞吐量 “far below” uncongested throughout
减小
/* since 路径 是 拥塞的 */
吞吐量
# 字节 已发送 在
最后 RTT interval
运输层: 3-140

Delay-based TCP congestion control
congestion control without inducing/forcing loss
maximizing throughout (“keeping the just pipe full... ”) while keeping 
delay low (“...but not fuller”)
a number of deployed TCPs take a delay‐based approach
BBRdeployed on Google’s (internal) backbone network
Transport Layer: 3-141
时延-based TCP
拥塞控制 没有 inducing/forcing 丢包
仅 pipe 完整的... ”) while ke
时延 low (“...但是 不 fuller”)
a 编号 的 deployed TCPs take a 时延‐基于 方法
deployed 在 Google’s (internal) 骨干 网络
运输层: 3-141

3-142
TCP Futures: TCP over “long, fat pipes”
example: 1500 byte segments, 100ms RTT, want 10 Gbps
throughput
requires W = 83,333 in-flight segments
throughput in terms of segment loss probability, L [Mathis 1997]:
➜to achieve 10 Gbps throughput, need a loss rate of L = 2·10
-10  
– a very 
small loss rate!
new versions of TCP for high-speed
TCP throughput = 
1.22
.
MSS
RTT
L
TCP Futures: TCP 在
示例: 1500 字节 报文段, 100ms RTT, want 10 Gbps
吞吐量
requires W = 83,333 in-flight 报文段
吞吐量 在 terms 的 报文段 丢包 probability, L
到 achieve 10 Gbps 吞吐量, 需要 a 丢包 速率 的 L = 2 10
small 丢包 速率!
新的 versions 的 TCP 用于 high-speed
TCP 吞吐量 =

3-143
fairness goal:if K TCP sessions share same bottleneck link of 
bandwidth R, each should have average rate of R/K
TCP connection 1
bottleneck
router
capacity R
TCP Fairness
TCP connection 2
公平性 目标:如果 K TCP sessions share 相同 瓶颈 链路 的
带宽 R, 每个 should 有 average 速率 的 R/K
TCP 连接 1
瓶颈
路由器
容量 R
TCP 公平性
TCP 连接 2

3-144
Why is TCP fair?
two competing sessions:
additive increase gives slope of 1, as throughout increases
multiplicative decrease decreases throughput proportionally 
R
R
equal bandwidth share
Connection 1 throughput
congestion avoidance: additive increase
loss: decrease window by factor of 2
congestion avoidance: additive increase
loss: decrease window by factor of 2
Why 是 TCP fair?
两个 competing sessions:
additive 增大 给出 slope o
multiplicative 减小 decreases 吞吐量 proportionally
equal 带宽 share
连接 1 吞吐量
拥塞 avoidance: additive 增大
丢包: 减小 窗口 由 factor 的 2
拥塞 avoidance: additive 增大
丢包: 减小 窗口 由 factor 的 2

3-145
Fairness (more)
Fairness and UDP
multimedia apps often do not 
use TCP
do not want rate throttled by 
congestion control
instead use UDP:
send audio/video at constant rate, 
tolerate packet loss
Fairness, parallel TCP 
connections
application can open multiple 
parallel connections between 
two hosts
web browsers do this 
e.g., link of rate R with 9 
existing connections:
new app asks for 1 TCP, gets rate R/10
new app asks for 11 TCPs, gets R/2 
公平性 (更多)
公平性 和 UDP
multimedia 应用 经常 做 不
使用 TCP
做 不 want 速率 throttled 由
拥塞控制
instead 使用 UDP:
发送 audio/video 在 constant 速率,
tolerate 分组 丢包
公平性, parallel TCP
连接
应用 可以 open 多路
parallel 连接 之间
两个 主机
Web browsers 做 这个
e.g., 链路 的 速率 R 与 9
existing 连接:
新的 应用 asks 用于 1 TCP, 获得 速率 R/10
新的 应用 asks 用于 11 TCPs, 获得 R/2

146
Basic Control Model
Let’s assume window-basedoperation
Reduce windowwhen congestion is perceived
How is congestion signaled?
Either markordroppackets
When is a router congested?
Drop tail queues – when queue is full
Average queue length – at some threshold
Increase windowotherwise
Probe for available bandwidth – how?
Supplementary Content
Basic 控制 模型
Reduce 窗口当 拥塞 是 perceived
如何 是 拥塞 signaled?
或分组
当 是 a 路由器 拥塞的?
Drop tail queues – 当 队列 是 完整的
Average 队列 长度 – 在 一些 threshold
增大 窗口
Probe 用于 可用的 带宽 – 如何?
Supplementary 内容

147
Simple linear control
Many different possibilities for reaction to 
congestion and methods for probing
Examine simple linearcontrols
Window(t + 1) = a + b Window(t)
Different a
i
/b
i
for increase and a
d
/b
d
for decrease
Simple linear 控制
许多 不同的 possibilities 用于 reaction 到
拥塞 和 methods 用于 probing
窗口(t + 1) = a + b 窗口(t)
不同的用于 增大和用于 减小

148
Simple linear control
Multiplicative increase, additive decrease
a
I
=0, b
I
>1;     a
D
<0, b
D
=1
Additive increase, additive decrease
a
I
>0, b
I
=1;     a
D
<0, b
D
=1
Multiplicative increase, multiplicative decrease
a
I
=0, b
I
>1;    a
D
=0, 0<b
D
<1
Additive increase, multiplicative decrease
a
I
>0, b
I
=1;    a
D
=0, 0<b
D
<1
Which one?









decreasetxba
increasetxba
tx
iDD
iII
i
)(
)(
)1(
Simple linear 控制
Multiplicative 增大, additive 减小
Additive 增大, additive 减小
Multiplicative 增大, multiplicative 减小
Additive 增大, multiplicative 减小
哪个 一个?
减小
增大

149
()
(1)
()
Ii
i
Di
b x  t   increase
xt
ax  t  decrease





Multiplicative Increase, 
Additive Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(x
1h
+a
D
,x
2h
+a
D
)
(b
I
(x
1h
+a
D
), b
I
(x
2h
+a
D
))
Does not 
converge to 
fairness
Not stable at all
Does not 
converges to 
efficiency
a
I
=0, b
I
>1, a
D
<0, b
D
=1
User 2: x
2
增大
t 减小
Multiplicative 增大,
Additive 减小
用户 1: x
用户 2: x
公平性
线路
效率
线路
做 不
收敛 到
公平性
不 stable
在 所有
做 不
converges 到
效率

User 2: x
2
150
Additive Increase, 
Additive Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(x
1h
+a
D
,x
2h
+a
D
)
(x
1h
+a
D
+a
I
),
x
2h
+a
D
+a
I
))
Does not 
converge to 
fairness
Does not 
converge to 
efficiency
()
(1)
()
Ii
i
Di
ax  t   increase
xt
ax  t  decrease






a
I
>0, b
I
=1, a
D
<0, b
D
=1
Additive 增大,
Additive 减小
用户 1: x
用户 2: x
公平性
线路
效率
线路
做 不
收敛 到
公平性
做 不
收敛 到
效率
增大
t 减小

151
Multiplicative Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(b
d
x
1h
,b
d
x
2h
)
(b
I
b
D
x
1h
,
b
I
b
D
x
2h
)
Does not 
converge to 
fairness
Converges to 
efficiency iff
10
1


D
I
b
b
()
(1)
()
Ii
i
Di
b x  t   increase
xt
bx tdecrease




a
I
=0, b
I
>1, a
D
=0, 0<b
D
<1
User 2: x
2
Multiplicative 增大,
Multiplicative 减小
用户 1: x
用户 2: x
公平性
线路
效率
线路
做 不
收敛 到
公平性
Converges 到
效率 iff
增大
t 减小

User 2: x
2
152
Multiplicative Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(b
d
x
1h
,b
d
x
2h
)
(b
I
b
D
x
1h
,
b
I
b
D
x
2h
)
Does not converge to fairness
Converges to efficiency iff
10
1


D
I
b
b
()
(1)
()
Ii
i
Di
b x  t   increase
xt
bx tdecrease




a
I
=0, b
I
>1, a
D
=0, 0<b
D
<1
Multiplicative 增大,
Multiplicative 减小
用户 1: x
用户 2: x
公平性
线路
效率
线路
做 不 收敛 到 公平性
Converges 到 效率 iff
增大
t 减小

153
(b
D
x
1h
+a
I
,
b
D
x
2h
+a
I
)
Additive Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(b
D
x
1h
,b
D
x
2h
)
Converges to 
fairness
Converges to 
efficiency
Increments 
smaller as 
fairness 
increases
(x
1h
,x
2h
)
()
(1)
()
IIi
i
Di
ab x  t   increase
xt
bx tdecrease





a
I
>0, b
I
=1, a
D
=0, 0<b
D
<1
Additive 增大,
Multiplicative 减小
用户 1: x
用户 2: x
公平性
线路
效率
线路
Converges 到
公平性
Converges 到
效率
公平性
增大
t 减小

User 2: x
2
随机早期检测RED (Random Early Detection) 
使路由器的队列维持两个参数，即队列长度最小门限TH
min
和最大门限TH
max
。
RED 对每一个到达的数据报都先计算平均队列长度L
AV
。
若平均队列长度小于最小门限TH
min
，则将新到达的数据报放入队列进行排队。
若平均队列长度超过最大门限TH
max
，则将新到达的数据报丢弃。
若平均队列长度在最小门限TH
min
和最大门限TH
max
之间，则按照某一概率p将新
到达的数据报丢弃。
从队首
发送
最小门限TH
min
最大门限TH
min
分组
到达
平均队列长度L
av
排队丢弃
以概率p丢弃
RED 将路
由器的到
达队列划
分成为三
个区域
156
RED (随机 Early 检测)
最小值
最大值
最小值
最大值
最小值
最大值
最小值
最小值

丢弃概率p 与TH
min
和Th
max
的关系
最小门限TH
min
最大门限TH
max
平均队列长度L
av
分组丢弃概率p
1.0
0
p
max
当L
AV
Th
min 
时，丢弃概率p= 0。
当L
AV
Th
max 
时，丢弃概率p= 1。
当TH
min
L
AV
TH
max
时，0 p 1 。
例如，按线性规律变化，从0 变到p
max
。
157
最小值
最大值
最小值
最大值
最大值
最小值
最大值
最小值
最大值
最大值

瞬时队列长度和
平均队列长度的区别
队列长度
时间
瞬时队列长度
平均队列长度
158

TCP, UDP: principal transport protocols for 40 years
different “flavors” of TCP developed, for specific scenarios:
Evolving transport-layer functionality
moving transport–layer functions to application layer, on top of UDP
•HTTP/3: QUIC
ScenarioChallenges
Long, fat pipes (large data transfers)   Many packets “in flight”; loss shuts down pipeline
Wireless networksLoss due to noisy wireless links, mobility; TCP treat 
this as congestion loss
Long‐delay linksExtremely long RTTs
Data center networksLatency sensitive
Background traffic flowsLow priority, “background” TCP flows 
Transport Layer: 3-159
端口 协议 用于 40 years
不同的 “flavors” 的 TCP developed, 用于 特定的 scenarios:
Evolving 运输层 functionality
moving 运输–层 功能 到 应用层, 在 top o
场景
Long, fat pipes (大的 数据 transfers)
许多 分组 “在 flight”;丢包 shuts down pipeline
无线网络s
丢包 由于 到 noisy 无线链路, 移动性; TC
这个 as 拥塞 丢包
Long‐时延 链路
数据 中心 网络
Background 流量 flows
Low 优先级, “background” TCP flows
运输层: 3-159

application-layer protocol, on top of UDP
increase performance of HTTP
deployed on many Google servers, apps (Chrome, mobile YouTube app) 
QUIC: Quick UDP Internet Connections
IP
TCP
TLS
HTTP/2
IP
UDP
QUIC
HTTP/2 (slimmed)
Network
Transport
Application
HTTP/2 over TCP
HTTP/3
HTTP/2 over QUIC over UDP
Transport Layer: 3-160
应用-layer 协议, 在 top 的 UDP
增大 性能 的 HTTP
deployed 在 许多 Google 服务器, 应用 (Chrome, 移动 YouTube a
QUIC: Quick UDP Internet 连接
网络
运输
应用
HTTP/2 在 TCP
HTTP/2 在 QUIC 在 UDP
运输层: 3-160

QUIC: Quick UDP Internet Connections
adopts approaches we’ve studied in this chapter for 
connection establishment, error control, congestion control
multiple application-level “streams” multiplexed over 
single QUIC connection
separate reliable data transfer, security
common congestion control
•error and congestion control: “Readers familiar with TCP’s loss 
detection and congestion control will find algorithms here that parallel 
well‐known TCP ones.” 
[from QUIC specification]
•connection establishment: reliability, congestion control, 
authentication, encryption, state established in one RTT
Transport Layer: 3-161
QUIC: Quick UDP Internet 连接
adopts 方法 我们’ve studied 在 这个 本章 用于
连接 establishment, 差错 控制, 拥塞控制
多路 应用-level “streams” multiplexed 在
单个 QUIC 连接
separate 可靠数据传输, 安全
common 拥塞控制
差错 和 拥塞控制:
“Readers familiar 与 TCP’s 丢包
检测 和 拥塞控制 将 find 算法 here 这
well‐已知 TCP ones.”
[从 QUIC specification]
连接 establishment:reliability, 拥塞控制,
authentication, encryption, 状态 established 在 一个 RTT
运输层: 3-161

QUIC: Connection establishment
TCP handshake
(transport layer)
TLS handshake
(security)
TCP (reliability, congestion control 
state) 
+ TLS (authentication, crypto 
state)
2 serial handshakes
data
QUIC handshake
data
QUIC:  reliability, congestion control, 
authentication, crypto state
1 handshake
Transport Layer: 3-162
QUIC: 连接 establishment
TCP 握手
(运输层)
TLS 握手
(安全)
(reliability, 拥塞控制
状态)
状态)
数据
QUIC 握手
数据
reliability, 拥塞控制,
authentication, crypto 状态
1 握手
运输层: 3-162

QUIC: streams: parallelism, no HOL blocking
(a) HTTP 1.1
TLS encryption
TCP RDT
TCP Cong. Contr.
transport
application
(b) HTTP/2 with QUIC: no HOL blocking
TCP RDT
TCP Cong. Contr.
TLS encryption
error!
HTTP 
GET 
HTTP 
GET 
HTTP 
GET 
QUIC Cong. Cont.
QUIC 
encrypt
QUIC
RDT
QUIC
RDT
QUIC
RDT
QUIC 
encrypt
QUIC 
encrypt
UDP
UDP
QUIC Cong. Cont.
QUIC 
encrypt
QUIC
RDT
QUIC
RDT
QUIC
RDT
QUIC 
encrypt
QUIC 
encrypt
error!
HTTP 
GET 
HTTP 
GET 
HTTP 
GET 
Transport Layer: 3-163
QUIC: streams: parallelism, 无 HOL blocking
运输
应用
(b) HTTP/2 与 QUIC:
无 HOL blocking
差错!
差错!
运输层: 3-163

transport
application
Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
