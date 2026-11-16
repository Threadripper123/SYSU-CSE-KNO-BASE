# Chapter2 Application Layer.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter2 Application Layer.zh-CN.pdf　｜　76,489 字符　｜　133 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
计算机网络
2024. 秋季

Chapter 2
Application Layer
2-2
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their 
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
第 2 章
应用层
说明 在 使用 的 thes
e PowerPoint 幻灯片:
我们’re making 这些 幻灯片 freely可用的 到 所有 (faculty, stu
readers). They’re 在 PowerPoint
form 因此 你 见 animations;
和 可以 添加, 修改, 和 删除 slides (包括 这个 一个)
幻灯片 内容 到 suit your 需要的
工作 在 我们的 part. 在 返回 用于
使用, 我们 仅 ask following:
如果 你 使用 这些 幻灯片 (e.g.,
在 a 课堂) 这 你 mention
源 (之后 所有, 我们’d 像 people 到 使用 我们的 book!)
如果 你 post 任何 幻灯片 在 a 网站 站点, 这 你 说明 这 they
是 adapted 从 (或 perhaps ident
ical 到) 我们的 幻灯片, 和 说明
我们的 版权 的 这个 material.
用于 a revision 历史, 见 t
he 幻灯片 说明 用于 这个 page.
谢谢 和 使用愉快! JFK/KWR
所有 材料 版权 1996-2023
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved
计算机 网络:
自顶向下 方法

Assignments :
Ch2 (ver8): 4,7,8,10,13,15,18,23,24,26,27
A group project
Key words: 
client-server architecture, Transport Services, HTTP, Cookies, 
Non-Persistent and Persistent Connections, Caching and proxy, 
FTP, SMTP, POP3, IMAP, DNS, P2P Architectures
3
作业：
关键 words:
客户-服务器 体系结构, Trans端口 服务, HTTP, Cookies,
Non-Persistent 和 Persistent 连接, Caching 和 proxy,

2-4
Chapter 2: outline
2.1 principles of network 
applications
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket programming with 
UDP and TCP
第 2 章: 提纲
2.1 原理 的 网络
应用
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字编程 与
UDP 和 TCP

2-5
Chapter 2: application layer
our goals:
conceptual, implementation 
aspects of network application 
protocols
transport-layer service models
client-server paradigm
peer-to-peer paradigm
learn about protocols by 
examining popular application-
level protocols
HTTP
FTP
SMTP / POP3 / IMAP
DNS
creating network applications
socket API
Application
transport
第 2 章: 应用层
我们的目标：
conceptual, 实现
aspects 的 网络 应用
协议
运输层服务模型
客户-服务器范式
对等范式
learn 关于 协议 由
examining popular 应用-
level 协议
creating 网络 应用
套接字 API
应用
运输

2-6
Some network apps
e-mail
web
text messaging
remote login
P2P file sharing
multi-user network games
streaming stored video 
(YouTube, Hulu, Netflix) 
voice over IP (e.g., Skype)
real-time video 
conferencing
social networking
search
...
一些 网络 应用
远程 login
P2P 文件 sharing
multi-user 网络 games
streaming 存储的 视频
voice 在 IP
real-time 视频
social 网络

2-7
Creating a network app
write programs that:
run on (different) end systems
communicate over network
e.g., web server software communicates with 
browser software
no need to write software for network-core 
devices
network-core devices do not run user applications 
applications on end systems  allows for rapid app 
development, propagation
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
application
transport
network
data link
physical
Creating a 网络 应用
write programs 这:
运行 在 (不同的)端 系统
通信 在 网络
e.g., Web 服务器 softw是 communicates 与
browser 软件
无 需要 到 write 软件 用于 network-core
设备
network-core 设备 做 不运行 用户 应用
应用 在 端 系统 允许 用于 rapid 应用
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
应用
运输
网络
数据 链路
物理

2-8
Application architectures
possible structure of applications:
client-server (C/S)
peer-to-peer (P2P)
应用 architectures
可能 结构 的 应用:
客户-服务器 (C/S)
对等 (P2P)

2-9
Client-server architecture
server: 
always-on host
permanent IP address
data centers for scaling
clients:
communicate with server
may be intermittently connected
may have dynamic IP addresses
do not communicate directly with each other
client/server
客户-服务器 体系结构
服务器:
always-on 主机
permanent IP 地址
数据 centers 用于 scaling
客户:
通信 与 服务器
可能 是 intermittently 连接的
可能 有 动态 IP 地址
做 不 通信 directly 与 每个 其他
客户/服务器

2-10
P2P architecture
noalways-on server
arbitrary end systems directly 
communicate
peers request service from other peers, 
provide service in return to other peers
self scalability– new peers bring 
new service capacity, as well as 
new service demands
peers are intermittently connected and 
change IP addresses
complex management
peer-peer
P2P 体系结构
无always-on 服务器
arbitrary 端 系统 directly
通信
对等方 请求 服务从 其他 对等方,
提供 服务 在 return 到 其他 对等方
– 新的 对等方 bring
新的 服务 容量, as well as
新的 服务 demands
对等方 是 intermittently 连接的 和
变化 IP 地址
complex 管理

2-11
Processes communicating
process:program running 
within a host
within same host, two processes 
communicate using  inter-process 
communication(defined by OS)
processes in different hosts 
communicate by exchanging 
messages
client process:
process that initiates
communication
server process:
process that waits to 
be contacted
aside: applications with P2P 
architectures have client 
processes & server processes
clients, servers
Communication: Process-To-Process
进程 communicating
进程:program 运行
在...内 a 主机
在...内 相同 主机, 两个 进程
通信 使用
通信(defined 由 OS)
进程 在 不同的 主机
通信 由 exchanging
报文
客户 进程:
进程 这
通信
服务器 进程:
进程 这waits 到
是 contacted
aside: 应用 与 P2P
architectures 有客户
进程 & 服务器 进程
客户, 服务器
通信: Process-To-Process

2-12
Addressing processes
to receive messages, process  must 
have identifier
host device has unique 32-bit IP 
address
Q:does  IP address of host on which 
process runs suffice for identifying 
the process?
example port numbers:
HTTP server: 80
mail server: 25
to send HTTP message to 
gaia.cs.umass.edu web server:
IP address:128.119.245.12
port number:80
A:no, manyprocesses can be 
running on same host
identifier includes both IPaddress 
and Portnumbers associated with 
process on host.
IPaddress 
Portnumbers 
编址 进程
到 接收 报文, 进程 必须
有
主机 设备 有 unique 32-bit IP
地址
做 IP 地址 的 主机 在 哪个
进程 runs suffi
ce 用于 identifying
进程?
示例 端口 编号:
HTTP 服务器: 80
邮件 服务器: 25
到 发送 HTTP 报文 到
gaia.cs.umass.edu Web 服务器:
IP 地址:
端口 编号:
无,许多进程 可以 是
运行 在 相同 主机
二者地址
和
端口
编号associated 与
进程 在 主机.
地址
端口
编号

2-13
SocketsWhat is Socket?
process sends/receives messages to/from its socket
socket analogous to door
sending process shoves message out door
sending process relies on transport infrastructure on other side of 
door to deliver message to socket at receiving process
App-layer process 
sends  data
Data packaging 
identification
Sockets=
IP + Port
Internet
Data packaging 
identification
Sockets=
IP + Port
OS
OS
Host 1
Host 2
App-layer process 
gets  data
什么 是 套接字?
进程 sends/receives me
ssages to/来自 它的
套接字
套接字 analogous 到
发送 进程 shoves 报文 查看
发送 进程 relies 在 transport 基础设施 在 其他 side 的
door 到 交付 报文 到 socket 在 接收 进程
App-layer 进程
发送 数据
数据 packaging
IP + 端口
数据 packaging
IP + 端口
主机 1
主机 2
App-layer 进程
获得 数据

2-14
SocketsWhat is Socket?
The essence of a socket is a special buffer for a process to 
send/receive data;
The definition of a socket is related to its function
Sending: 
Process A
OS
buffer
Socket for A
Label: srcPort+dstIP+dstPort
IPs
Questions:
•What happens when a process sends 
data to multiple destination processes  
on different hosts at the same time?
•What happens when multiple 
processes on the same host send data 
to the same program on the same host 
at the same time?
Virtual Receiving 
Point/mirror point 
of receiving
什么 是 套接字?
essence 的 a 套接字 是 a s
pecial 缓冲区 用于 a 进程 到
send/receive 数据;
definition 的 a 套接字 是 相关 到 它的 功能
发送:
进程
缓冲区
套接字 用于
标签: srcPort+dstIP+dstPort
什么 happens 当 a 进程 发送
数据 到 多路 目的地 进程
在 不同的 主机 在 相同 时间?
什么 happens 当 多路
进程 在 相同 主机 发送 数据
到 相同 program 在 相同 主机
在 相同 时间?
Virtual 接收
Point/mirror 点
的 接收

2-15
SocketsWhat is Socket?
The essence of a socket is a special buffer for a process to 
send/receive data;
The definition of a socket is related to its function
Receiving   (I): 
Process B
OS
buffer
Socket for B
Label: dstPort
IPs
Receiving   (II): 
Process B
OS
buffer
Socket for B
IPs
Label: dstPort+srcIP+srcPort
What is the difference?
Virtual sending 
Point/Mirror 
Point of sending
Virtual sending 
Point/mirror 
point of sending
什么 是 套接字?
essence 的 a 套接字 是 a s
pecial 缓冲区 用于 a 进程 到
send/receive 数据;
definition 的 a 套接字 是 相关 到 它的 功能
接收
进程 B
缓冲区
套接字 用于 B
标签: dstPort
接收
进程 B
缓冲区
套接字 用于 B
标签: dstPort+srcIP+srcPort
什么 是 difference?
Virtual 发送
点 的 发送
Virtual 发送
点 的 发送

2-19
App-layer protocol defines
types of messages exchanged,
e.g., request, response 
message syntax:
what fields in messages & how 
fields are delineated
message semantics
meaning of information in fields
rulesfor when and how processes 
send & respond to messages
open protocols:
defined in RFCs
allows for interoperability
e.g., HTTP, SMTP
proprietary protocols:
e.g., Skype, QQ
App-layer 协议 defines
types 的 报文 exchanged,
e.g., 请求, 响应
报文 syntax:
什么 字段 在报文 & 如何
字段 是 delineated
报文 semantics
meaning 的 信息rmation 在 字段
用于
当 和 如何进程
发送 & respond 到 报文
open 协议:
defined 在 RFCs
允许 用于 interoperability
proprietary 协议:

2-20
What transport service does an app need?
data integrity
some apps (e.g., file transfer, web 
transactions) require 100% reliable 
data transfer
other apps (e.g., audio) can tolerate 
some loss
timing
some apps (e.g., Internet 
telephony, interactive games) 
require low delay to be 
“effective”
throughput
some apps (e.g., multimedia) require 
minimum amount of throughput to be 
“effective”
other apps (“elastic apps”) make 
use of whatever throughput they get 
security
encryption, data integrity, 
...
Application
transport
什么 运输 服务 做 应用 需要?
数据 integrity
一些 应用 (e.g., 文件 传输, Web
100% 可靠
数据 传输
其他 应用 (e.g., au
dio) 可以 tolerate
一些 丢包
一些 应用 (e.g., Internet
telephony, 交互式 games)
require low 时延 到 是
吞吐量
一些 应用 (e.g., multimedia) require
minimum 数量 的 吞吐量 到 是
其他 应用 (
elastic 应用
) 生成
使用 的 whatever 吞吐量 they 获得
安全
encryption, 数据 integrity,
应用
运输

2-21
Transport service requirements: common apps
application
file transfer
e-mail
Web documents
real-time audio/video
stored audio/video
interactive games
text messaging
data loss
no loss
no loss
no loss
loss-tolerant
loss-tolerant
loss-tolerant
no loss
throughput
elastic
elastic
elastic
audio: 5kbps-1Mbps
video:10kbps-5Mbps
same as above 
few kbps up
elastic
time sensitive
no
no
no
yes, 100’s msec
yes, few secs
yes, 100’s msec
yes and no
运输 服务 requirements: common 应用
应用
文件 传输
存储的 audio/video
交互式 games
数据 丢包
无 丢包
无 丢包
无 丢包
丢包-tolerant
丢包-tolerant
丢包-tolerant
无 丢包
吞吐量
视频:10kbps-5Mbps
相同 as 上方
few kbps 上
时间 sensitive
无
无
无
是, 100
是, few secs
是, 100
是 和 无

2-22
Internet transport protocols services
TCP service:
reliable transportbetween sending 
and receiving process
flow control:sender won’t 
overwhelm receiver 
congestion control:throttle sender 
when network overloaded
connection-oriented:setup 
required between client and server 
processes
DOES NOT provide:timing, 
minimum throughput guarantee, 
security
UDP service:
unreliable data transferbetween 
sending and receiving process
does not provide:reliability, flow 
control, congestion control, 
timing, throughput guarantee, 
security, or connection setup, 
Q:why bother?  Why is there a UDP?
Internet 运输协议 服务
TCP 服务:
可靠 运输之间 发送
和 接收 进程
流量控制:发送方 won
overwhelm 接收方
拥塞控制:throttle 发送方
当 网络 overloaded
面向连接:
required 之间 客户 和 服务器
进程
DOES NOT 提供:
minimum 吞吐量 guarantee,
安全
UDP 服务:
un可靠数据传输之间
发送 和 接收 进程
做 不 提供:
reliability, 流
控制, 拥塞控制,
timing, 吞吐量 guarantee,
安全, 或 连接建立,
why bother? Why 是 存在 a UDP?

2-23
Internet apps:  application, transport protocols
application
e-mail
remote terminal access
Web 
file transfer
streaming multimedia
Internet telephony
application
layer protocol
SMTP [RFC 2821]
Telnet [RFC 854]
HTTP [RFC 2616]
FTP [RFC 959]
HTTP (e.g., YouTube), 
RTP [RFC 1889]
SIP, RTP, proprietary
(e.g., Skype)
underlying
transport protocol
TCP
TCP
TCP
TCP
TCP or UDP
TCP or UDP
Internet 应用: 应用, 运输协议
应用
远程 终端 访问
文件 传输
应用
层 协议
运输协议
TCP 或 UDP
TCP 或 UDP

Securing TCP
TCP & UDP 
no encryption
Clear-text passwords sent 
into socket traverse Internet  
in clear-text
SSL
provides encrypted TCP 
connection
data integrity
end-point authentication
SSL is at app layer
Apps use SSL libraries, which 
“talk”to TCP
SSL socket API
Clear-text passwords sent into 
socket traverse Internet  
encrypted 
See Chapter 7
2-24
应用层
SSL
TCP
无 encryption
Clear-text passwords 已发送
进入 套接字 经过 Internet
在 clear-text
提供 encrypted TCP
连接
数据 integrity
SSL 是 在 应用 层
应用 使用 SSL libraries, 哪个
到 TCP
SSL 套接字 API
Clear-text passwords 已发送 进入
套接字 经过 Internet
见 第 7 章

2-25
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket programming 
with UDP and TCP
第 2 章: 提纲
2.1 原理 的 网络
应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字编程
与 UDP 和 TCP

2-26
Web and HTTP
First, a review...
web pageconsists of objects
object can be HTML file, JPEG image, 
Java applet, audio file,...
web page consists of base HTML-file
which includes several referenced 
objects
each object is addressable by a URL, 
e.g.,
www.someschool.edu/someDept/pic.gif
host name
path name
Web 和 HTTP
第一个, a review...
Web 页面consists 的对象
对象 可以 是 HTML 文件, JPEG image,
Java applet, audio 文件,...
Web 页面 consists 的基 HTML-file
哪个 includes
对象
每个 对象 是 addressable 由 a
主机 名称
路径 名称

2-27
HTTP overview
HTTP: hypertext transfer protocol
Web’s application layer protocol
client/server model
client:browser that requests, receives, 
(using HTTP protocol) and “displays”
Web objects 
server:Web server sends (using HTTP 
protocol) objects in response to requests
PC running
Firefox browser
server 
running
Apache Web
server
iphone running
Safari browser
http/1.1 (rfc2616)
HTTP 概述
ansfer 协议
s 应用层 协议
客户/服务器 模型
客户browser 这 requests, 接收,
(使用 HTTP 协议) 和
Web 对象
服务器:Web 服务器 发送 (使用 HTTP
协议) 对象 在 response 到 请求
PC 运行
服务器
运行
服务器
iphone 运行

2-28
HTTP overview (continued)
uses TCP:
client initiates TCP connection (creates 
socket) to server,  port 80
server accepts TCP connection from 
client
HTTP messages (application-layer 
protocol messages) exchanged between 
browser (HTTP client) and Web server 
(HTTP server)
TCP connection closed
HTTP is “stateless”
server maintains no 
information about past client 
requests
protocols that maintain 
“state”are complex!
past history (state) must be maintained
if server/client crashes, their views of 
“state”may be inconsistent, must be 
reconciled
aside
HTTP 概述 (continued)
使用 TCP:
客户 initiates TCP连接 (创建
套接字) 到 服务器, 端口 80
服务器 accepts TCP 连接 从
客户
HTTP 报文 (应用-layer
协议 报文) exchanged 之间
browser (HTTP 客户) 和 Web 服务器
(HTTP 服务器)
TCP 连接 closed
HTTP 是
服务器 维护 无
信息 关于 past 客户
请求
协议 这 maintain
状态是 complex!
past 历史 (状态) 必须 是 maintained
如果 服务器/客户 crashes, 它们的 views 的
状态
可能 是 inconsistent, 必须 是

2-29
HTTP connections
non-persistent HTTP
at most one object sent over 
one TCP connection
connection then closed
downloading multiple objects 
required multiple 
connections
persistent HTTP
multiple objects can be 
sent over single TCP 
connection between 
client, server
HTTP 连接
在 most 一个 对象 已发送 在
一个 TCP 连接
连接 则 closed
多路 对象
required 多路
连接
多路 对象 可以 是
已发送 在 单个 TCP
连接 之间
客户, 服务器

2-30
Non-persistent HTTP
suppose user enters URL:
1a.HTTP client initiates TCP 
connection to HTTP server 
(process) at 
www.someSchool.edu on 
port 80
2.HTTP client sends HTTP request 
message(containing URL) into 
TCP connection socket. 
Message indicates that client 
wants object 
someDepartment/home.index
1b.HTTP server at host 
www.someSchool.edu waiting 
for TCP connection at port 80.  
“accepts”connection, 
notifying client
3.HTTP server receives request 
message, forms response 
messagecontaining requested 
object, and sends message into 
its socket
time
(contains text,
references to 10 
jpeg images)
www.someSchool.edu/someDepartment/home.index
suppose 用户 enters URL:
HTTP 客户
连接 到 HTTP 服务器
(进程) 在
www.someSchool.edu 在
端口 80
HTTP 客户 发送 HTTP请求
报文(包含 URL) 进入
TCP 连接 套接字.
报文 indicates 这 客户
wants 对象
HTTP 服务器 在 主机
用于 TCP 连接 在 端口 80.
连接
notifying 客户
HTTP 服务器 接收 请求
报文, forms响应
报文包含 requested
对象, 和 发送 报文 进入
它的 套接字
时间
(包含 text,
references 到 10

2-31
Non-persistent HTTP (cont.)
5.HTTP client receives response 
message containing html file, 
displays html.  Parsing html 
file, finds 10 referenced jpeg  
objects
6.Steps 1-5 repeated for eachof 
10 jpeg objects
4.HTTP server closes TCP 
connection. 
time
HTTP 客户 接收 响应
报文 包含 html 文件,
文件, finds 10 referenced jpeg
对象
Steps 1-5 repeated 用于
每个的
10 jpeg 对象
HTTP 服务器 closes TCP
时间

2-32
Non-persistent HTTP: response time
RTT (definition):time for a small packet to 
travel from client to server and back
HTTP response time:
one RTT to initiate TCP connection
one RTT for HTTP request and first few 
bytes of HTTP response to return
file transmission time
non-persistent HTTP response time =   
2RTT+ file transmission  time
time to 
transmit 
file
initiate TCP
connection
RTT
request
file
RTT
file
received
time
time
Non-persistent HTTP: 响应 时间
时间 用于 a small 分组 到
travel 从 客户 到 服务器 和 back
HTTP 响应 时间:
一个 RTT 到 initiat
e TCP 连接
一个 RTT 用于 HTTP 请求 和 第一个 few
字节 的 HTTP 响应 到 返回
文件 传输 时间
non-persistent HTTP 响应 时间 =
+ 文件 传输 时间
时间 到
传输
文件
连接
请求
文件
文件
已收到
时间
时间

2-33
Persistent HTTP
non-persistent HTTP issues:
requires 2 RTTs per object
OS overhead for eachTCP 
connection
browsers often open parallel TCP 
connections to fetch referenced 
objects
persistent  HTTP:
server leaves connection open after 
sending response
subsequent HTTP messages  
between same client/server sent 
over open connection
client sends requests as soon as it 
encounters a referenced object
as little as one RTT for all the 
referenced objects
2 RTTs 每 对象
OS overhead 用于每个
连接
browsers 经常 open parallel TCP
连接 到 fetch referenced
对象
服务器 leaves 连接 open 之后
发送 响应
subsequent HTTP 报文
之间 相同 客户/服务器 已发送
在 open 连接
客户 发送 请求 as soon as 它
encounters a referenced 对象
as little as 一个 RTT 用于 所有
referenced 对象

36
non-persistent HTTP 

37
会出现阻塞
因为TCP导致排头阻塞，
HTTP3 采用UDP

2-38
HTTP request message
two types of HTTP messages: request, response
HTTP request message:
ASCII (human-readable format)
request line
(GET, POST, 
HEAD commands)
header
lines
carriage return, 
line feed at start
of line indicates
end of header lines
GET /index.html HTTP/1.1\r\n
Host: www-net.cs.umass.edu\r\n
User-Agent: Firefox/3.6.10\r\n
Accept: text/html,application/xhtml+xml\r\n
Accept-Language: en-us,en;q=0.5\r\n
Accept-Encoding: gzip,deflate\r\n
Accept-Charset: ISO-8859-1,utf-8;q=0.7\r\n
Keep-Alive: 115\r\n
Connection: keep-alive\r\n
\r\n
carriage return character
line-feed character
HTTP 请求 报文
两个 types 的 HTTP 报文:请求响应
HTTP 请求 报文:
ASCII (human-readable 格式)
请求 线路
首部
线路
carriage 返回,
线路 feed 在 开始
的 线路 indicates
端 的 首部 线路
主机: www-net.cs.umass.edu\r\n
Accept: text/html,应用/xhtml+xml\r\n
连接: keep-alive\r\n
carriage 返回 character

2-39
HTTP request message: general format
request
line
header
lines
body
method
sp
sp
cr
lf
version
URL
cr
lf
value
header field name
cr
lf
value
header field name
~
~
~
~
cr
lf
entity body
~
~
~
~
HTTP 请求 报文: general 格式
请求
线路
首部
线路
值
首部 字段 名称
值
首部 字段 名称

2-40
Uploading form input
POST method:
web page often includes form input
input is uploaded to server in entity body
Create new items
URL method:
uses GET method
input is uploaded in 
URL field of request 
line:
www.somesite.com/animalsearch?monkeys&banana
Uploading form 输入
Web 页面 经常 includes form 输入
输入 是 uploaded 到 服务器
在 entity body
Create 新的 items
使用 GET method
输入 是 uploaded 在
URL 字段 的 请求
线路:

2-41
Method types
HTTP/1.0:
GET
POST
HEAD
asks server to check requested 
object without responding it.
HTTP/1.1:
GET, POST, HEAD
PUT
uploads file in entity body to 
path specified in URL field
Modify (idempotent)
DELETE
deletes file specified in the URL 
field
asks 服务器 到 查看 requested
对象 没有 responding it.
uploads 文件 在 entity body 到
路径 specified在 URL 字段
修改 (idempotent)
deletes 文件 specified 在 URL
字段

2-42
HTTP response message
status line
(protocol
status code
status phrase)
header
lines
data, e.g., 
requested
HTML file
HTTP/1.1 200 OK\r\n
Date: Sun, 26 Sep 2010 20:09:20 GMT\r\n
Server: Apache/2.0.52 (CentOS)\r\n
Last-Modified: Tue, 30 Oct 2007 17:00:02 
GMT\r\n
ETag: "17dc6-a5c-bf716880"\r\n
Accept-Ranges: bytes\r\n
Content-Length: 2652\r\n
Keep-Alive: timeout=10, max=100\r\n
Connection: Keep-Alive\r\n
Content-Type: text/html; charset=ISO-8859-
1\r\n
\r\n
data data data data data ... 
HTTP 响应 报文
状态 线路
(协议
状态 码
状态 phrase)
首部
线路
数据, e.g.,
HTML 文件
服务器: Apache/2.0.52 (CentOS)\r\n
Accept-Ranges: 字节\r\n
连接: Keep-Alive\r\n
数据 数据 数据 数据 数据 ...

2-43
HTTP response status codes
200 OK
request succeeded, requested object later in this msg
301 Moved Permanently
requested object moved, new location specified later in this 
msg (Location:)
400 Bad Request
request msg not understood by server
404 Not Found
requested document not found on this server
505 HTTP Version Not Supported
status code appears in 1st line in server-to-
client response message.
some sample codes:
HTTP 响应 状态 codes
请求 succeeded, requested 对象 later 在 这个 msg
requested 对象 moved, 新的 location specified later 在 这个
400 Bad 请求
请求 msg 不 understood 由 服务器
404 不 Found
requested document 不 found 在 这个 服务器
505 HTTP Version 不 Supported
状态 码 appears 在 1st 线路 在 服务器-to-
客户 响应 message.
一些 sample codes

2-44
Trying out HTTP (client side) for yourself
1. Telnet to your favorite Web server:
opens TCP connection to port 80
(default HTTP server port) at cis.poly.edu.
anything typed in sent 
to port 80 at cis.poly.edu
telnet cis.poly.edu 80
2. type in a GET HTTP request:
GET /~ross/ HTTP/1.1
Host: cis.poly.edu
by typing this in (hit carriage
return twice), you send
this minimal (but complete) 
GET request to HTTP server
3. look at response message sent by HTTP server!
(or use Wireshark to look at captured HTTP request/response)
Trying 查看 HTTP (客户 一侧) 用于 yourself
1. Telnet 到 your favorite Web 服务器:
opens TCP 连接 到 端口 80
(default HTTP 服务器 端口) 在 cis.poly.edu.
anything typed 在 已发送
到 端口 80 在 cis.poly.edu
2. 类型 在 a GET HTTP 请求:
主机: cis.poly.edu
由 typing 这个 在 (hit carriage
返回 twice), 你 发送
这个 minimal (但是 完整的)
GET 请求 到 HTTP 服务器
3. look 在 响应 报文 已发送 由 HTTP 服务器!
(或 使用 Wireshark 到 look a

2-45
User-server state: cookies
many Web sites use cookies
four components:
1) cookie header line of HTTP 
responsemessage
2) cookie header line in next HTTP 
requestmessage
3) cookie file kept on user’s host, 
managed by user’s browser
4) back-end database at Web site
example:
Susan always access Internet from 
PC
visits specific e-commerce site for 
first time
when initial HTTP requests arrives 
at site, site creates: 
unique ID
entry in backend database for ID
User-服务器 状态: cookies
许多 Web 站点 使用 cookies
cookie 首部 线路 的 HTTP
响应报文
2) cookie 首部 线路 在 下一个 HTTP
请求报文
3) cookie 文件 kept 在 用户’s 主机,
管理的 由 用户’s browser
4) back-end 数据库 在 Web 站点
示例:
Susan 总是 访问 Internet 从
visits 特定的 e-commerce 站点 用于
第一个 时间
当 初始 HTTP 请求 到达
在 站点, 站点 创建:
entry 在 backend 数据库 用于 ID

2-46
Cookies: keeping “state”(cont.)
client
server
usual http response msg
usual http response msg
cookie file
one week later:
usual http request msg
cookie: 1678
cookie-
specific
action
access
ebay 8734
usual http request msg
Amazon server
creates ID
1678 for user
create
entry
usual http response 
set-cookie: 1678
ebay 8734
amazon 1678
usual http request msg
cookie: 1678
cookie-
specific
action
access
ebay 8734
amazon 1678
backend
database
2023.9.11
状态
客户
服务器
usual HTTP 响应 msg
usual HTTP 响应 msg
cookie 文件
一个 week later:
usual HTTP 请求 msg
特定的
动作
访问
usual HTTP 请求 msg
Amazon 服务器
创建 ID
1678 用于 用户
usual HTTP 响应
usual HTTP 请求 msg
特定的
动作
访问
数据库

2-47
Cookies (continued)
what cookies can be used for:
authorization
shopping carts
recommendations
user session state (Web e-mail)
cookies and privacy:
cookies permit sites to 
learn a lot about you
you may supply name 
and e-mail to sites
aside
how to keep “state”:
protocol endpoints: maintain state at sender/receiver 
over multiple transactions
cookies: http messages carry state
什么 cookies 可以 是 用于 用于:
用户 session 状态 (Web e-mail)
cookies 和 privacy:
cookies permit 站点 到
learn a lot 关于 你
你 可能 supply 名称
和 e-mail 到 站点
如何 到 keep
状态
协议 endpoints: maintain 状态 在 发送方/接收方
在 多路 transactions
cookies: HTTP 报文 carry 状态

2-48
Web caches (proxy server)
user sets browser: Web 
accesses via  cache
browser sends all HTTP 
requests to cache
object in cache: cache 
returns object 
else cache requests 
object from origin 
server, then returns 
object to client
goal:satisfy client request without 
involving origin server (就近访问)
Internet
Internet
Web caches (proxy 服务器)
用户 集合 browser: Web
accesses 通过缓存
browser 发送 所有 HTTP
请求 到缓存
对象 在 cache: 缓存
returns 对象
否则 缓存 请求
对象 从 origin
服务器, 则 returns
对象 到 客户
目标:
satisfy 客户 请求 没有
involving origin 服务器 (

2-49
More about Web caching
cache acts as both client 
and server
server for original requesting client
client to origin server
typically cache is installed 
by ISP (university, 
company, residential ISP)
why Web caching?
reduce response time for client 
request
reduce traffic on an institution’s 
access link
Internet dense with caches: 
enables “poor”content 
providers to effectively deliver 
content (so too does P2P file 
sharing)
更多 关于 Web caching
缓存 acts as 二者 客户
和 服务器
服务器 用于 原始 requesting 客户
客户 到 origin 服务器
通常 缓存 是 installed
由 ISP (university,
reduce 响应 时间 用于 客户
请求
reduce 流量 在 institution
访问 链路
Internet dense 与 caches:
内容
providers 到 effectively 交付
内容 (因此 too 做 P2P 文件

2-50
Caching example: 
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = 99%
total delay   = Internet delay + access delay + LAN delay
=  2 sec+ minutes+ usecs
problem!
瓶颈在哪里？
(15 requests/sec)  (1 Mbits/request)/(100 Mbps) = 0.15
(15 requests/sec)  (1 Mbits/request)/(15 Mbps) = 1
Caching 示例:
avg 对象 大小: 1M 比特
avg 请求 速率 从 browsers 到 origin 服务器:15/sec
avg 数据 速率 到 browsers: 15 Mbps
RTT 从 institutional 路由器 到 任何 origin 服务器: 2 sec
访问 链路 速率: 15 Mbps
局域网 利用率: 15%
访问 链路 利用率 =
total 时延
= Internet 时延
+ 访问 时延 + 局域网 时延
问题!

2-51
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = 99%
total delay   = Internet delay + access delay + LAN delay
=  2 sec + minutes + usecs
Caching example: fatter access link 
100Mbps
100 Mbps
msecs
Cost:increased access link 
speed (not cheap!)
15%
avg 对象 大小: 1M 比特
avg 请求 速率 从 browsers 到 origin 服务器:15/sec
avg 数据 速率 到 browsers: 15 Mbps
RTT 从 institutional 路由器 到 任何 origin 服务器: 2 sec
访问 链路 速率: 15 Mbps
局域网 利用率: 15%
访问 链路 利用率 =
total 时延
= Internet 时延
+ 访问 时延 + 局域网 时延
Caching 示例: fatter 访问 链路
费用:increased 访问 链路
不 cheap

2-52
Caching example: install local cache 
assumptions:
avg object size: 1M bits
avg request rate from browsers to origin 
servers:15/sec
avg data rate to browsers: 15 Mbps
RTT from institutional router to any origin server: 2 
sec
access link rate: 15 Mbps
consequences:
LAN utilization: 15%
access link utilization = ?
total delay   = Internet delay + access delay + LAN 
delay
=  2 sec + minutes + usecs
?
?
How to compute link 
utilization, delay?
Cost:web cache (cheap!)
Caching 示例: install 本地 缓存
avg 对象 大小: 1M 比特
avg 请求 速率 从 browsers 到 origin
服务器:15/sec
avg 数据 速率 到 browsers: 15 Mbps
RTT 从 institutional rout
er 到 任何 origin 服务器: 2
访问 链路 速率: 15 Mbps
局域网 利用率: 15%
访问 链路 利用率 =
total 时延ay + 访问 时延 + 局域网
时延
如何 到 计算 链路
利用率, 时延?
费用:Web 缓存 (cheap!)

2-53
Caching example: install local cache 
Calculating access link utilization, delay 
with cache:
suppose cache hit rate is 0.4
40% requests satisfied at cache, 60% requests satisfied 
at origin 
access link utilization: 
60% of requests use access link 
data rate to browsers over access link = 0.6*15 Mbps = 9 Mbps 
utilization = 9/15 = 0.6
total delay
= 0.6 * (delay from origin servers) +0.4 * (delay when 
satisfied at cache)
= 0.6 (2+0.01) + 0.4 (~msecs~0.01s) 
= ~ 1.2 secs
less than with 100Mbps link (and cheaper too!)
Caching 示例: install 本地 缓存
Calculating 访问 链路 利用率, 时延
与 缓存:
suppose 缓存 hit 速率 是
40% 请求 satisfied 在 缓存, 60% 请求 satisfied
在 origin
访问 链路 利用率:
60% 的 请求 使用 访问 链路
数据 速率 到 browsers 在 acce
ss 链路 = 0.6*15 Mbps = 9 Mbps
利用率 =
total 时延
= 0.6 * (时延 从 origin服务器) +0.4 * (时延 当
satisfied 在 缓存)
less 比 与 100Mbps 链路 (和 cheaper too!)

2-54
Conditional GET 
Goal:don’t send object if cache 
has up-to-date cached version
no object transmission delay
lower link utilization
cache:specify date of cached copy 
in HTTP request
If-modified-since: <date>
server:response contains no 
object if cached copy is up-to-date: 
HTTP/1.0 304 Not Modified
HTTP request msg
If-modified-since: <date>
HTTP response
HTTP/1.0 
304 Not Modified
object 
not 
modified
before
<date>
HTTP request msg
If-modified-since: <date>
HTTP response
HTTP/1.0 200 OK
<data>
object 
modified
after 
<date>
client
server
目标:
don’t 发送 对象 如果 缓存
有 up-to-date cached version
无 对象 传输 时延
lower 链路 利用率
缓存:specify date 的 cached 复制
在 HTTP 请求
服务器:响应 包含 无
对象 如果 cached cop
y 是 up-to-date:
HTTP/1.0 304 不 Modified
HTTP 请求 msg
HTTP 响应
304 不 Modified
对象
不
之前
HTTP 请求 msg
HTTP 响应
对象
之后
客户
服务器

HTTP/2
Key goal: decreased delay in multi‐object HTTP requests
HTTP1.1:introduced multiple, pipelined GETsover single TCP 
connection
server responds in‐order(FCFS: first‐come‐first‐served scheduling) to 
GET requests
with FCFS, small object may have to wait for transmission  (head‐of‐
line (HOL) blocking) behind large object(s)
loss recovery (retransmitting lost TCP segments) stalls object 
transmission
Application Layer: 2-55
关键 目标:
decreased 时延 在 multi‐对象 HTTP 请求
多路, pipelined 获得在 单个 TCP
连接
服务器 responds
在‐order
(FCFS: 第一个‐come‐第一个‐served scheduling) 到
GET 请求
与 FCFS, small 对象 可能 有 到 等待 用于 传输 (
head‐的‐
线路 (HOL) blocking
) 背后的 大的 对象(s)
丢包 recovery (retransmitting 丢失的 TCP 报文段) stalls 对象
传输
应用层: 2-55

HTTP/2
HTTP/2:[RFC 7540, 2015] increased flexibility at serverin sending objects 
to client:
methods, status codes, most header fields unchanged from HTTP 1.1
transmission order of requested objects based on client‐specified object 
priority 
(not necessarily FCFS)
pushunrequested objects to client
divide objects into frames, schedule frames to mitigate HOL blocking
Key goal: decreased delay in multi‐object HTTP requests
Application Layer: 2-56
increased flexibility 在
服务器在 发送 对象
到 客户:
methods, 状态 codes, most 首部 字段 unchanged 从 HTTP 1
传输 order 的 requested 对象 基于 在 客户‐specified 对象
优先级
(不 necessarily FCFS)
unrequested 对象 到 客户
divide 对象 进入 帧
, schedule 帧 到 mitigate HOL blocking
关键 目标:
decreased 时延 在 multi‐对象 HTTP 请求
应用层: 2-56

HTTP/2: mitigating HOL blocking
HTTP 1.1: client requests 1 large object (e.g., video file) and 3 smaller objects
client
server
GET O
1
GET O
2
GET O
3
GET O
4
O
1
O
2
O
3
O
4
object data requested
O
1
O
2
O
3
O
4
objects delivered in order requested: O
2
, O
3
, O
4
wait behind O
1
Application Layer: 2-57
HTTP 1.1: 客户 请求 1 大的 对象 (e.g., 视频 文件) 和
3 smaller 对象
客户
服务器
对象 数据 requested
对象 delivered 在 order requested: O
等待 背后的 O
应用层: 2-57

HTTP/2: mitigating HOL blocking
HTTP/2: objects divided into frames, frame transmission interleaved
client
server
GET O
1
GET O
2
GET O
3
GET O
4
O
2
O
4
object data requested
O
1
O
2
O
3
O
4
O
2
, O
3
, O
4
delivered quickly, O
1 
slightly delayed
O
3
O
1
Application Layer: 2-58
HTTP/2: 对象 divided 进入 帧, 帧 传输 interle
客户
服务器
对象 数据 requested
delivered 快速地, O
slightly 时延ed
应用层: 2-58

HTTP/2 to HTTP/3
HTTP/2 over single TCP connection means:
recovery from packet loss still stalls all object transmissions
•as in HTTP 1.1, browsers have incentive to open multiple parallel 
TCP connections to reduce stalling, increase overall throughput
no security over vanilla TCP connection
HTTP/3: adds security, per object error‐and congestion‐
control (more pipelining) over UDP
•more on HTTP/3 in transport layer
Application Layer: 2-59
HTTP/2 到 HTTP/3
HTTP/2 在 单个 TCP 连接 means:
recovery 从 分组 丢包 仍然 stalls 所有 对象 transmissions
as 在 HTTP 1.1, browsers 有 incentive 到 open 多路 parall
TCP 连接 到 reduce stalling, 增大 overall 吞吐量
无 安全 在 vanilla TCP 连接
adds 安全, 每 对象 差错‐ 和 拥塞‐
控制 (更多 pipelining)在 UDP
更多 在 HTTP/3 在 运输层
应用层: 2-59

2-60
Chapter 2: outline
2.1 principles of 
network applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP
第 2 章: 提纲
2.1 原理 的
网络 应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字
编程
与 UDP 和

2-61
FTP: the file transfer protocol
file transfer
FTP
server
FTP
user
interface
FTP
client
local file
system
remote file
system
user 
at host
transfer file to/from remote host
client/server model
client:side that initiates transfer (either to/from remote)
server:remote host
ftp: RFC 959
ftp server: port 21
FTP: 文件 传输 协议
文件 传输
服务器
用户
接口
客户
本地 文件
系统
远程 文件
系统
用户
在 主机
传输 文件 to/来自 远程 主机
客户/服务器 模型
客户:
一侧 这 initiates transfe
r (either to/来自 远程)
服务器:
远程 主机
FTP 服务器: 端口 21

2-62
FTP: separate control, data connections
FTP client contacts FTP server at port 
21, using TCP 
client authorized over control 
connection
client browses remote directory, sends 
commands over control connection
when server receives file transfer 
command, serveropens 2
nd
TCP data 
connection (for file) to client
after transferring one file, server closes 
data connection
FTP
client
FTP
server
TCP control connection,
server port 21
TCP data connection,
server port 20
server opens another TCP data 
connection to transfer another file
control connection: “out of 
band”
FTP server maintains “state”: 
current directory, earlier 
authentication
FTP: separate 控制, 数据 连接
FTP 客户 contacts FTP 服务器 在 端口
21, 使用 TCP
客户 authorized 在 控制
连接
客户 browses 远程 directory, 发送
commands 在 控制 连接
当 服务器 接收 文件 传输
服务器
TCP 数据
连接 (用于 文件)到客户
之后 transferring 在
e 文件, 服务器 closes
数据 连接
客户
服务器
控制 连接
服务器 端口 21
数据 连接
服务器 端口 20
服务器 opens 另一个 TCP 数据
连接 到 传输 另一个 文件
控制 连接:
查看 的
FTP 服务器 维护状态
当前 directory, earlier

2-63
FTP commands, responses
sample commands:
sent as ASCII text over control 
channel
USER username
PASS password
LISTreturn list of file in current 
directory
RETR filenameretrieves (gets) 
file
STOR filenamestores (puts) 
file onto remote host
sample return codes
status code and phrase (as in 
HTTP)
331 Username OK, password 
required
125 data connection already 
open; transfer starting
425 Can’t open data 
connection
452 Error writing file
已发送 as ASCII text
在 控制
信道
返回 列表 的 文件 在 当前
retrieves (获得)
文件
文件 onto 远程 主机
sample 返回 codes
状态 码 和 phrase (as 在
125 数据 连接 already
open; 传输 starting
425 可以t open 数据
连接
452 差错 writing 文件

2-64
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP
第 2 章: 提纲
2.1 原理 的 网络
应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字
编程
与 UDP 和

2-65
Electronic mail
Three major components:
user agents 
mail servers 
simple mail transfer protocol: SMTP
User Agent
a.k.a. “mail reader”
composing, editing, reading mail 
messages
e.g., Outlook, Thunderbird, iPhone
mail client
outgoing, incoming messages 
stored on server
user mailbox
outgoing 
message queue
mail
server
mail
server
mail
server
SMTP
SMTP
SMTP
user
agent
user
agent
user
agent
user
agent
user
agent
user
agent
Electronic 邮件
用户 agents
邮件 服务器
simple 邮件 传输 协议: SMTP
用户 代理
邮件 reader
composing, editing, reading 邮件
报文
邮件 客户
outgoing, 传入的 报文
存储的 在 服务器
用户 mailbox
报文 队列
邮件
服务器
邮件
服务器
邮件
服务器
用户
代理
用户
代理
用户
代理
用户
代理
用户
代理
用户
代理

2-66
Electronic mail: mail servers
mail servers:
mailboxcontains incoming messages for 
user
message queueof outgoing (to be sent) 
mail messages
SMTP protocolbetween mail servers to 
send email messages
client: sending mail server
“server”: receiving mail server
mail
server
mail
server
mail
server
SMTP
SMTP
SMTP
user
agent
user
agent
user
agent
user
agent
user
agent
user
agent
Electronic 邮件: 邮件 服务器
邮件 服务器:
包含 传入的 报文 用于
用户
报文 队列
的 outgoing (到 是 已发送)
邮件 报文
SMTP 协议之间 邮件 服务器 到
发送 电子邮件 报文
客户: 发送 邮件 服务器
服务器: 接收 邮件 服务器
邮件
服务器
邮件
服务器
邮件
服务器
用户
代理
用户
代理
用户
代理
用户
代理
用户
代理
用户
代理

SMTP RFC (5321)
uses TCP to reliably transfer email message 
from client (mail server initiating 
connection) to server, port 25
direct transfer: sending server (acting like client) 
to receiving server
three phases of transfer
•SMTP handshaking (greeting)
•SMTP transfer of messages
•SMTP closure
command/response interaction (like HTTP)
•commands:ASCII text
•response:status code and phrase
initiate TCP
connection
RTT
time
220
250 Hello
HELO
SMTP 
handshaking
TCP connection
initiated
“client”
SMTP server
“server”
SMTP server
SMTP 
transfers
Application Layer: 2-67
使用 TCP 到 reliably 传输 电子邮件 报文
从 客户 (邮件 服务器 initiating
连接) 到 服务器,
端口 25
直接 传输: 发送 服务器 (acting 像 客户)
到 接收 服务器
three phases 的 传输
SMTP 握手 (greeting)
SMTP 传输 的 报文
command/response interaction (像 HTTP)
响应:状态 码 和 phrase
连接
时间
握手
TCP 连接
“客户”
SMTP 服务器
“服务器”
SMTP 服务器
应用层: 2-67

2-68
user
agent
Scenario: Alice sends message to Bob
1) Alice uses UA to compose message 
“to”bob@someschool.edu
2) Alice’s UA sends message to her 
mail server; message placed in 
message queue
3) client side of SMTP opens TCP 
connection with Bob’s mail server
4) SMTP client sends Alice’s message 
over the TCP connection
5) Bob’s mail server places the message 
in Bob’s mailbox
6) Bob invokes his user agent to read 
message
mail
server
mail
server
1
2
3
4
5
6
Alice’s mail server
Bob’s mail server
user
agent
SMTP
SMTP
用户
代理
场景: Alice 发送 报文 到 Bob
1) Alice 使用 UA 到 compose 报文
到
s UA 发送 报文 到 her
邮件 服务器; 报文 placed 在
报文 队列
3) 客户 一侧 的 SMTP opens TCP
连接 与 Bobs 邮件 服务器
4) SMTP 客户 发送 Alices 报文
在 TCP 连接
s 邮件 服务器 places 报文
在 Bob
6) Bob invokes his 用户 代理 到 read
报文
邮件
服务器
邮件
服务器
s 邮件 服务器
s 邮件 服务器
用户
代理

2-69
Sample SMTP interaction
S: 220 hamburger.edu 
C: HELOcrepes.fr 
S: 250  Hello crepes.fr, pleased to meet you 
C: MAIL FROM: <alice@crepes.fr> 
S: 250 alice@crepes.fr... Sender ok 
C: RCPT TO: <bob@hamburger.edu> 
S: 250 bob@hamburger.edu ... Recipient ok 
C: DATA
S: 354 Enter mail, end with "." on a line by itself 
C: Do you like ketchup? 
C: How about pickles? 
C: .
S: 250 Message accepted for delivery 
C: QUIT
S: 221 hamburger.edu closing connection
Hello crepes.fr, pleased 到 meet 你
MAIL 来自
S: 250 alice@crepes.fr... 发送方 ok
S: 354 Enter 邮件, 端 与 "." 在a 线路 由 itself
C: 做 你 像 ketchup?
C: 如何 关于 pickles?
S: 250 报文 accepted 用于 交付
S: 221 hamburger.edu closing 连接

2-70
Try SMTP interaction for yourself:
telnet servername 25
see 220 reply from server
enter HELO, MAIL FROM, RCPT TO, DATA, QUIT commands
above lets you send email without using email client (reader)
Try SMTP interaction 用于 yourself:
见 220 应答 从 服务器
HELO, MAIL 来自,
上方 lets 你 发送 电子邮件 与
查看 使用 电子邮件 客户 (reader)

2-71
SMTP: final words
SMTP uses persistent connections
SMTP requires message (header & 
body) to be in 7-bit ASCII
SMTP server uses CRLF.CRLFto 
determine end of message
comparison with HTTP:
HTTP: pull
SMTP: push
both have ASCII command/response 
interaction, status codes
HTTP: each object encapsulated in 
its own response msg
SMTP: multiple objects sent in 
multipart msg
SMTP: 最终 words
SMTP 使用persistent 连接
SMTP requires 报文 (首部 &
body) 到 是 在 7-bit ASCII
SMTP 服务器 使用到
确定 端 的 报文
comparison 与 HTTP:
二者 有 ASCII command/response
interaction, 状态 codes
HTTP: 每个 对象 encapsulated 在
它的 自己的 响应 msg
SMTP: 多路 对象 已发送 在

2-72
Mail message format
SMTP: protocol for exchanging 
email msgs
RFC 822: standard for text 
message format:
header lines, e.g.,
To:
From:
Subject:
differentfrom SMTP MAIL 
FROM, RCPT TO:
commands!
Body: the “message”
ASCII characters only
header
body
blank
line
邮件 报文 格式
SMTP: 协议 用于 exchanging
电子邮件 msgs
RFC 822: standard 用于 text
报文 格式:
首部 线路, e.g.,
到:
从:
不同的从
来自, RCPT TO
报文
ASCII characters 仅
首部
线路

2-73
Mail access protocols
SMTP:delivery/storage to receiver’s server
mail access protocol: retrieval from server
POP:Post Office Protocol [RFC 1939]: authorization, download 
IMAP:Internet Mail Access Protocol [RFC 1730]: more features, including 
manipulation of stored msgs on server
HTTP:gmail, Hotmail, Yahoo! Mail, etc.
Sender’s mail 
server
SMTP
SMTP
mail access
protocol
Receiver’s mail 
server
(e.g., POP, 
IMAP
)
user
agent
user
agent
邮件 访问 协议
delivery/storage 到 接收方s 服务器
邮件 访问 协议:retrieval 从 服务器
Post Office 协议 [RFC 1939]: authorization, download
Internet 邮件 访问 协议 [RFC 1730]: 更多 features, includ
manipulation 的 存储的 msgs 在 服务器
gmail, Hotmail, Yahoo! 邮件, etc.
发送方’s 邮件
服务器
邮件 访问
协议
接收方’s 邮件
服务器
用户
代理
用户
代理

2-74
POP3 protocol
authorization phase
client commands: 
user:declare username
pass:password
server responses
+OK
-ERR
transaction phase,client:
list:list message numbers, size
retr:retrieve message by number
dele:delete
quit
C: list 
S: 1 498 
S: 2 912 
S: . 
C: retr 1 
S: <message 1 contents>
S: . 
C: dele 1 
C: retr 2 
S: <message 1 contents>
S: . 
C: dele 2 
C: quit 
S: +OK 
POP3 server signing off
S: +OK POP3 server ready 
C: user bob 
S: +OK 
C: pass hungry 
S: +OKuser successfully logged on
POP3 协议
客户 commands:
用户
服务器 responses
客户:
列表列表 报文 编号, 大小
retrieve 报文 由 编号
删除
C: 列表
POP3 服务器 signing off
POP3 服务器 ready
用户 bob
用户 successfully logged 在

2-75
POP3 (more) and IMAP
more about POP3
previous example uses POP3 
“download and delete”mode
Bob cannot re-read e-mail if he 
changes client
POP3 “download-and-keep”: 
copies of messages on different 
clients
POP3 is stateless across sessions
IMAP
keeps all messages in one place: at 
server
allows user to organize messages 
in folders
keeps user state across sessions:
names of folders and mappings 
between message IDs and folder 
name
POP3 (更多) 和 IMAP
更多 关于 POP3
previous 示例 使用 POP3
download 和 删除
Bob cannot re-read e-mail 如果 he
变化 客户
copies 的 报文 在 不同的
客户
POP3 是 stateless across sessions
keeps 所有 报文 在
一个 place: 在
服务器
允许 用户 到 organize 报文
在 folders
keeps 用户 状态 across sessions:
名称 的 folders 和 mappings
之间 报文 IDs 和 folder
名称

2-76
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP
第 2 章: 提纲
2.1 原理 的 网络
应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字
编程
与 UDP 和

2-77
DNS: domain name system
people:many identifiers:
SSN, name, passport #
Internet hosts, routers:
IP address (32 bit) - used for 
addressing datagrams
“name”, e.g., www.yahoo.com -
used by humans
Q:how to map between IP address 
and name, and vice versa ?
Domain Name System:
distributed databaseimplemented in 
hierarchy of many name servers
application-layer protocol:hosts, 
name servers communicate to 
resolvenames (address/name 
translation)
note: core Internet function, 
implemented as application-layer 
protocol
complexity at network’s “edge”
DNS: 域 名称 系统
许多 identifiers:
SSN, 名称, passport #
Internet 主机, 路由器:
IP 地址 (32 比特) - 用于 用于
编址 数据报
名称
用于 由 humans
如何 到 map 之间 IP 地址
和 名称, 和 vice versa ?
域 名称 系统:
分布式 数据库implemented 在
hierarchy 的 许多名称 服务器
应用-layer 协议:主机,
名称 服务器 通信 到
名称 (address/name
翻译)
说明: 核心 Internet 功能,
implemented as 应用-layer
协议
复杂度 在 网络边缘

2-78
DNS: services, structure 
why not centralize DNS?
single point of failure
traffic volume
distant centralized database
maintenance
DNS services
hostname to IP address translation
host aliasing
canonical, alias names
mail server aliasing
load distribution
replicated Web servers: many IP 
addresses correspond to one name
A: doesn’t scale!
DNS: 服务, 结构
why 不 centralize DNS?
单个 点 的 failure
流量 volume
distant centralized 数据库
DNS 服务
hostname 到 IP 地址 翻译
主机 aliasing
canonical, alias 名称
邮件 服务器 aliasing
负载 distribution
replicated Web 服务器: 许多 IP
地址 correspond 到 一个 名称

Thinking about the DNS
humongous distributed database:
~ billion records, each simple
handles many trillionsof queries/day:
manymore reads than writes
performance matters:  almost every 
Internet transaction interacts with 
DNS ‐msecs count!
organizationally, physically decentralized:
millions of different organizations 
responsible for their records
“bulletproof”: reliability, security
Application Layer: 2-79
Thinking 关于 DNS
humongous 分布式 数据库
~ billion records, 每个 simple
处理 许多
的 queries/day:
许多更多 reads 比 writes
性能 matters:
almost 每一个
Internet transaction interacts 与
organizationally, 物理上 分散式:
millions 的 不同的 organizations
responsible 用于 它们的 records
“bulletproof”: reliability, 安全
应用层: 2-79

2-80
Root DNS Servers
com DNS servers
org DNS serversedu DNS servers
poly.edu
DNS servers
umass.edu
DNS servers
yahoo.com
DNS servers
amazon.com
DNS servers
pbs.org
DNS servers
DNS: a distributed, hierarchical database
client wants IP for www.amazon.com; 1
st
approx:
client queries root server to find com DNS server
client queries .com DNS server to get amazon.com DNS server
client queries amazon.com DNS server to get  IP address for 
www.amazon.com
...
...
根 DNS 服务器
com DNS 服务器
org DNS 服务器edu DNS 服务器
DNS 服务器DNS 服务器
DNS 服务器
DNS 服务器
DNS 服务器
DNS: a 分布式, hierarchical 数据库
客户 wants IP 用于 www.amazon.com; 1
客户queries 根 服务器到find com DNS 服务器
客户queries .com DNS 服务器到获得 amazon.com DNS 服务器
客户queries amazon.com DNS 服务器到 获得 IP 地址 用于

2-81
DNS: root name servers
contacted by local name server that can not resolve name
root name server:
contacts authoritative name server if name mapping not known
gets mapping
returns mapping to local name server
13 root name “servers”worldwide
a. Verisign, Los Angeles CA
(5 other sites)
b. USC-ISI Marina del Rey, CA
l. ICANN Los Angeles, CA
(41 other sites)
e. NASA Mt View, CA
f. Internet Software C.
Palo Alto, CA (and 48 other   
sites)
i. Netnod, Stockholm (37 other sites)
k. RIPE London (17 other sites)
m. WIDE Tokyo
(5 other sites)
c. Cogent, Herndon, VA (5 other sites)
d. U Maryland College Park, MD
h. ARL Aberdeen, MD
j. Verisign, Dulles VA (69 other sites )
g. US DoD Columbus, 
OH (5 other sites)
DNS: 根 名称 服务器
contacted 由 本地 名称 server 这 可以 不 resolve 名称
根 名称 服务器:
contacts 权威 名称 服务器 如果 名称 mapping 不 已知
获得 mapping
returns mapping 到 本地 名称 服务器
根 名称服务器
(5 其他 站点)
(41 其他 站点)
f. Internet 软件 C.
Palo Alto, CA (和 48 其他
站点)
i. Netnod, Stockholm (37 其他 站点)
k. RIPE London (17 其他 站点)
(5 其他 站点)
c. Cogent, Herndon, VA (5 其他 站点)
j. Verisign, Dulles VA (69 其他 站点 )
OH (5 其他 站点)

82
IPv4根服务器分布表
名称 地位主服务器运营者
[1]
主服务器位置
[1]
IP
[1]
ADM and Root serverINTERNI.NET
美国弗吉尼亚州
198.41.0.4
BRoot server
美国信息科学研究所美国加利弗尼亚州
128.9.0.107
CRoot serverPSINet
公司美国弗吉尼亚州
192.33.4.12
DRoot server
马里兰大学美国马里兰州
128.8.10.90
ERoot server
美国航空航天管理局美国加利弗尼亚州
192.203.230.10
FRoot server
因特网软件联盟美国加利弗尼亚州
192.5.5.241
GRoot server
美国国防部网络信息中心   美国弗吉尼亚州
192.112.36.4
HRoot server
美国陆军研究所美国马里兰州
128.63.2.53
IRoot serverAutonomica
公司瑞典斯德哥尔摩
192.36.148.17
JRoot serverVerSign
公司美国弗吉尼亚州
192.58.128.30
KRoot serverRIPE NCC
英国伦敦
192.0.14.129
LRoot serverIANA
美国弗吉尼亚州
198.32.64.12
MRoot serverWIDE Project
日本东京
202.12.27.33
全世界只有13台IPv4根域名服务器。1个为主根服务器在美国。其余12个均为辅根
服务器，其中9台在美国，欧洲2个，位于英国和瑞典，亚洲1个位于日本。
DM 和 根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器
根 服务器

83

2-84
TLD, authoritative servers
top-level domain (TLD) servers:
responsible for com, org, net, edu, aero, jobs, museums, 
and all top-level country domains, e.g.: uk, fr, ca, jp
Network Solutions maintains servers for .com TLD
Educause for .edu TLD
authoritative DNS servers:
Organization’s own DNS server(s), providing 
authoritative hostname to IP mappings for organization’s 
named hosts 
can be maintained by organization or service provider
amazon.com DNS server
.com DNS server
TLD, 权威 服务器
n (TLD) 服务器:
responsible 用于 com, org, n
和 所有 top-level country domains, e.g.: uk, fr, ca, jp
网络 Solutions maintains 服务器 用于 .com TLD
Educause 用于 .edu TLD
权威 DNS 服务器:
组织’s 自己的 DNS 服务器(s), 提供
权威 hostname 到 IP mappings 用于 组织’s
named 主机
可以 是 maintained 由 organization 或 服务 提供商
amazon.com DNS 服务器
.com DNS 服务器

2-85
Local DNS name server
does not strictly belong to hierarchy
each ISP (residential ISP, company, university) has one
also called “default name server”
when host makes DNS query, query is sent to its local DNS 
server
has local cache of recent name-to-address translation pairs (but 
may be out of date!)
acts as proxy, forwards query into hierarchy
本地 DNS 名称 服务器
做 不 strictly belong 到 hierarchy
每个 ISP (residential ISP, company, university) 有 一个
default 名称 服务器
当 主机 makes DNS 查询, que
ry 是 已发送 到 它的 本地 DNS
服务器
有 本地 缓存 的 recent name-to-address 翻译 pairs (bu
可能 是 查看 的 date!)
acts as proxy, forwards 查询 进入 hierarchy

DNS name resolution: iterated query
Example: host at engineering.nyu.edu
wants IP address for gaia.cs.umass.edu
Iterated query:
contacted server replies 
with name of server to 
contact
“I don’t know this name, 
but ask this server”
requesting host at
engineering.nyu.edu
gaia.cs.umass.edu
root DNS server
local DNS server
dns.nyu.edu
1
2
3
4
5
6
authoritative DNS server
dns.cs.umass.edu
7
8
TLD DNS server
Application Layer: 2-86
2023.9.13
DNS 名称 resolution: iterated 查询
示例:
主机 在
wants IP 地址 用于
Iterated 查询:
contacted 服务器 replies
与 名称 的 服务器 到
“I don’t know 这个 名称,
但是 ask 这个 服务器”
requesting 主机 在
根 DNS 服务器
本地 DNS 服务器
权威 DNS 服务器
TLD DNS 服务器
应用层: 2-86

DNS name resolution: recursive query
requesting host at
engineering.nyu.edu
gaia.cs.umass.edu
root DNS server
local DNS server
dns.nyu.edu
1
2
3
4
5
6
authoritative DNS server
dns.cs.umass.edu
7
8
TLD DNS server
Recursive query:
puts burden of name 
resolution on 
contacted name 
server
heavy load at upper 
levels of hierarchy
?
Example: host at engineering.nyu.edu
wants IP address for gaia.cs.umass.edu
Application Layer: 2-87
DNS 名称 resolution: recursive 查询
requesting 主机 在
根 DNS 服务器
本地 DNS 服务器
权威 DNS 服务器
TLD DNS 服务器
Recursive 查询:
puts burden 的 名称
resolution 在
contacted 名称
服务器
heavy 负载 在 upper
levels 的 hierarchy
示例:
主机 在
wants IP 地址 用于
应用层: 2-87

2-88
DNS: caching, updating records
once (any) name server learns mapping, it cachesmapping
cache entries timeout (disappear) after some time (TTL)
TLD servers typically cached in local name servers
thus root name servers not often visited
cached entries may be out-of-date(best effort name-to-address 
translation!)
if name host changes IP address, may not be known Internet-wide until all 
TTLs expire
update/notify mechanisms proposed IETF standard
RFC 2136
once (任何) 名称 服务器 learns mapping, 它
缓存 entries 超时 (disappear) 之后 一些 时间 (
TLD 服务器 通常 cached 在 本地 名称 服务器
thus 根 名称 服务器 不 经常 被访
cached entries 可能 是(尽力而为 name-to-address
翻译!)
如果 名称 主机 变化 IP 地址, 可能 不 是 已知 Internet-wide
直到 所有

2-89
DNS records
DNS:distributed db storing resource records (RR)
type=NS
nameis domain (e.g., foo.com)
valueis hostnameof authoritative 
Name Server for this domain
RR format:(name, value, type, ttl)
type=A
nameis hostname
valueis IP 
Address
type=CNAME
nameis alias name for some “canonical”(the real) 
name
e.g., www.ibm.comis really
servereast.backup2.ibm.com
valueis 
Canonical NAME
type=MX
valueis name of Mailserver
associated withname
分布式 db storin
名称是 域 (e.g., foo.com)
值是的权威
用于 这个 域
RR 格式:名称, 值,类型
名称是 hostname
值
是 IP
名称是
alias 名称 用于 一些
名称
是 really
值是
值是 名称 的
associated 与名称

2-90
DNS protocol, messages
queryand replymessages, both with same 
message format
msg header
identification:16 bit # for 
query, reply to query uses 
same #
flags:
query or reply
recursion desired 
recursion available
reply is authoritative
identificationflags
#questions
questions (variable # of questions)
#additional RRs
#authority RRs
#answer RRs
answers (variable # of RRs)
authority (variable # of RRs)
additional info (variable # of RRs)
2 bytes2 bytes
DNS 协议, 报文
查询和应答报文, 二者 与 相同
报文 格式
msg 首部
16 比特 # 用于
查询, 应答 到 查询 使用
相同 #
查询 或 应答
recursion 可用的
应答 是 权威
questions (变量 # 的 questions)
answers (变量 # 的 RRs)
authority (变量 # 的 RRs)
additional 信息 (变量 # 的 RRs)
2 字节2 字节

92

2-93
name, type fields
for a query
RRs in response
to query
records for
authoritative servers
additional “helpful”
info that may be used
identificationflags
# questions
questions (variable # of questions)
# additional RRs
# authority RRs
# answer RRs
answers (variable # of RRs)
authority (variable # of RRs)
additional info (variable # of RRs)
DNS protocol, messages
2 bytes2 bytes
名称, 类型 字段
用于 a 查询
RRs 在 响应
到 查询
records 用于
权威 服务器
信息 这 可能 是 用于
questions (变量 # 的 questions)
answers (变量 # 的 RRs)
authority (变量 # 的 RRs)
additional 信息 (变量 # 的 RRs)
DNS 协议, 报文
2 字节2 字节

2-94
Inserting records into DNS
example: new startup “Network Utopia”
register name networkuptopia.com at DNS registrar(e.g., 
Network Solutions)
provide names, IP addresses of authoritative name server 
(primary and secondary)
registrar inserts two RRs into .com TLD server:
(networkutopia.com, dns1.networkutopia.com, NS)
(dns1.networkutopia.com, 212.212.212.1, A)
create authoritative server type A record for 
www.networkuptopia.com; type MX record for 
networkutopia.com
Inserting records 进入 DNS
示例: 新的 startup网络 Utopia
register 名称 networkuptopia.com 在
网络 Solutions)
提供 名称, IP 地址 of 权威 名称 服务器
(primary 和 secondary)
两个 RRs进入 .comTLD 服务器
create 权威 服务器 类型 record 用于
网站.networkuptopia.com; 类型 MX record 用于

Attacking DNS
DDoS attacks
Bombard rootservers with 
traffic
Not successful to date
Traffic Filtering
Local DNS servers cache IPs of 
TLD servers, allowing root server 
bypass
Bombard TLDservers
Potentially more dangerous
Redirect attacks
Man-in-middle
Intercept queries
DNS poisoning
Send bogus relies to DNS server, 
which caches
Exploit DNS for DDoS
Send queries with spoofed 
source address: target IP
Requires amplification
2-95
根服务器 与
流量
不 successful 到 date
流量 Filtering
本地 DNS 服务器 缓存 IPs 的
TLD 服务器, allowing 根 服务器
服务器
Potentially 更多 dangerous
发送 bogus relies 到 DNS 服务器,
哪个 caches
Exploit DNS 用于 DDoS
发送 queries 与 spoofed
源 地址: target IP

96
DNS诞生于1983年，在几次修订后最终于1987年定型。整个查询过程以TCP协
议或UDP协议在53端口上进行，并且以明文进行查询。整个过程完全没有验证
、没有隐私可言，这意味着任何人都可以对DNS查询过程进行攻击与篡改。

97

98
DoT(DNS over TLS)标准正式发布于2016年，而
DoH (DNS over HTTPS)标准正式成文于2018年10月
DoT(DNS 在 TLS)
DoH (DNS 在 HTTPS)

2-99
Chapter 2: outline
2.1 principles of network 
applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP 
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 socket 
programming 
with UDP and 
TCP
第 2 章: 提纲
2.1 原理 的 网络
应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 套接字
编程
与 UDP 和

2-100
Pure P2P architecture
noalways-on server
arbitrary end systems directly communicate
peers are intermittently connected and change 
IP addresses
examples:
file distribution (BitTorrent)
Streaming (KanKan)
VoIP (Skype) 
Pure P2P 体系结构
无always-on 服务器
arbitrary 端 系统通信
对等方 是 intermittently 连接的 和 变化
IP 地址
示例:
文件 distribution (BitTorrent)

2-101
File distribution: client-server vs P2P
Question:how much time to distribute file (size F) from one 
server to N  peers?
peer upload/download capacity is limited resource
u
s
u
N
d
N
server
network (with abundant
bandwidth)
file, size F
u
s
:server upload capacity
u
i
:peer i upload 
capacity
d
i
:peer i download 
capacity
u
2
d
2
u
1
d
1
d
i
u
i
文件 distribution: 客户-服务器 vs P2P
问题:
如何 much 时间 到 distribute 文件 (大小
) 从一个
服务器 到N 对等方
对等方 upload/download capa
city 是 limited resource
服务器
网络 (与 abundant
带宽)
文件, 大小 F
服务器 upload 容量
对等方 i upload
容量
对等方 i download
容量

2-102
File distribution time: client-server
server transmission: must
sequentially send (upload) N file
copies
:
time to send one copy: F/u
s 
time to send N copies: NF/u
s
increases linearly in N
time to  distribute F 
to N clients using 
client-server approach
D
c-s
> max{NF/u
s,
,F/d
min
}
client: each client must download file copy
d
min
= min client download rate
min client download time: F/d
min
u
s
network
d
i
u
i
F
文件 distribution 时间: 客户-服务器
服务器 传输:必须
发送 (upload)
文件
时间 到 发送 一个 复制:
时间 到 发送 N copies:
increases linearly 在 N
时间 到 distribute F
到N 客户使用
客户-服务器 方法
> 最大值{NF/u
最小值
客户:每个 客户 必须 download 文件 复制
最小值
= 最小值 客户 download 速率
最小值 客户 download 时间: F/d
最小值
网络

2-103
File distribution time: P2P
server transmission: must upload 
at least one copy
time to send one copy: F/u
s 
time to  distribute F 
to N clients using 
P2P approach
D
P2P
> max{F/u
s,
,F/d
min,
,NF/(u
s
+ u
i
)}
client: each client must download file copy
min client download time: F/d
min
clients: as aggregate must download NFbits
max upload rate (limiting max download rate) is u
s
+ u
i
... but so does this, as each peer brings service capacity
increases linearly in N...
u
s
network
d
i
u
i
F
File:
Minimum Distribution Time
文件 distribution 时间: P2P
服务器 传输:必须 upload
在 最少的 一个 复制
时间 到 发送 一个 复制:
时间 到 distribute F
到 N 客户 使用
P2P 方法
> 最大值{F/u
最小值,
客户:每个 客户 必须 download 文件 复制
最小值 客户 download 时间: F/d
最小值
客户:as aggregate 必须 download
比特
最大值 upload 速率 (limiting 最大值 download 速率) 是 u
... 但是 因此 做 这个, as 每个 对等方
brings 服务 容量
increases linearly 在
网络
文件:

1042-104
0
0.5
1
1.5
2
2.5
3
3.5
05101520253035
N
Minimum Distribution Time
P2P
Client-Server
Client-server vs. P2P: example
client upload rate =u,  F/u = 1 hour,  u
s
= 10u,  d
min
≥ u
s
Minimum Distribution 时间
客户-服务器
客户-服务器 vs. P2P: 示例
客户 upload 速率 =
最小值

2-105
P2P file distribution: BitTorrent
tracker:tracks peers 
participating in torrent
torrent:group of peers 
exchanging  chunks of a file
Alice arrives  ...
file divided into 256Kb chunks
peers in torrent send/receive file chunks
... obtains list
of peers from tracker
... and begins exchanging 
file chunks with peers in torrent
P2P 文件 distribution: BitTorrent
tracks 对等方
participating 在 torrent
group 的 对等方
exchanging 块 的 a 文件
Alice 到达 ...
文件 divided 进入 256Kb 块
对等方 在 torrent send/receive 文件 块
... obtains 列表
的 对等方 从 tracker
... 和 begins exchanging
文件 块 与 对等方 在 torrent

2-106
peer joining torrent: 
has no chunks, but will accumulate 
them over time from other peers
registers with tracker to get list of 
peers, connects to subset of peers 
(“neighbors”)
P2P file distribution: BitTorrent
while downloading, peer uploadschunks to other peers
peer may change peers with whom it exchanges chunks
churn:peers may come and go
once peer has entire file, it may (selfishly) leave or (altruistically) remain in 
torrent
对等方 joining torrent:
有 无 块, 但是 将 accumulate
them 在 时间 从 其他 对等方
registers 与 tracker 到 获得 列表 的
对等方, 连接 到 subset 的 对等方
邻居
P2P 文件 distribution: BitTorrent
ading, 对等方
块 到 其他 对等方
对等方 可能 变化 对等方 与whom 它 exchanges 块
对等方 可能 come 和 go
once 对等方 有 entire 文件, 它 可能(selfishly) leave 或 (altruis
tically) remain 在

2-107
BitTorrent: requesting, sending file chunks
requesting chunks:
at any given time, different peers 
have different subsets of file 
chunks
periodically, Alice asks each 
peer for list of chunks that they 
have
Alice requests missing chunks 
from peers, 
rarest first
sending chunks: tit-for-tat
Alice sends chunks to those four 
peers currently sending her chunks 
at highest rate
other peers are choked by Alice (do not 
receive chunks from her)
re-evaluate top 4every10 secs

every 30 secs: randomly select 
another peer, starts sending chunks
“optimistically unchoke”this peer
newly chosen peer may join top 4
让稀缺资源变多
BitTorrent: requesting, 发送 文件 块
requesting 块:
在 任何 给定的 时间, 不同的 对等方
有 不同的 subsets 的 文件
块
periodically, Alice asks 每个
对等方 用于 列表 的 块 这 they
有
Alice 请求 missing 块
从 对等方,
rarest 第一个
发送 块: tit-for-tat
Alice 发送 块 到 those
对等方
currently 发送 her 块
在 highest 速率
其他 对等方 是 chok
ed 由 Alice (做 不
接收 块 从 her)
每一个10 secs
每一个 30 secs: randomly select
另一个 对等方, starts 发送 块
这个 对等方
newly chosen 对等方 可能 join top 4

2-108
BitTorrent: tit-for-tat
(1) Alice “optimistically unchokes”Bob
(2) Alice becomes one of Bob’s top-four providers; Bob reciprocates
(3) Bob becomes one of Alice’s top-four providers
higher upload rate:find better 
trading partners, get file faster !
(2) Alice becomes 一个 的 Bob’s t
一个 的 Alice’s top-four providers
higher upload 速率:
trading partners, 获得 文件 faster !

Distributed Hash Table (DHT)
Hash table
DHT paradigm
Circular DHT and overlay networks
Peer churn 
109
分布式 Hash 表 (DHT)
Hash 表
Circular DHT 和 overlay 网络
对等方 churn

Distributed Hash Table (DHT)
Background
How to distribute
the chunks to peers and 
manage the (peer, chunk)
110
Data Fragmentation
分布式 Hash 表 (DHT)
如何 到 distribute
块到 对等方 和
manage (对等方, 块)
数据 Fragmentation

Original KeyValue
John Washington132-54-3570
Diana Louise Jones761-55-3791
Xiaoming Liu385-41-0902
Rakesh Gopal441-89-1956
Linda Cohen217-66-5609
................
Lisa Kobayashi177-23-0199
Simple database with(key, value) pairs: 
• key: human name; value: social security #
Simple Database
key: movie title; 
value: movie data.
111
原始 关键值
Simple 数据库 与(关键, 值)
  关键: human 名称; 值: social 安全 #
Simple 数据库
关键: movie title;
值: movie data.

Original KeyHash KeyValue
John Washington8962458132-54-3570
Diana Louise Jones7800356761-55-3791
Xiaoming Liu1567109385-41-0902
Rakesh Gopal2360012441-89-1956
Linda Cohen5430938217-66-5609
................
Lisa Kobayashi9290124177-23-0199
• More convenient to store and search on 
numerical representation of key
• key = hash(original key)
Hash Table
key: hash(movie title); 
value: movie data.
112
原始 关键Hash 关键值
  更多 convenient 到 store 和 search 在
numerical representation 的 关键
  关键 = hash(原始 关键)
Hash 表
关键: hash(movie title);
值: movie data.

Distribute (key, value) pairs over millions of peers
pairs are evenly distributed over peers
Any peer can query database with a key
database returns value for the key
To resolve query, small number of messages exchanged among 
peers
Each peer only knows about a small number of other peers
Robust to peers coming and going (churn)
Distributed Hash Table (DHT)
113
Distribute (关键, 值) pairs 在 millions 的 对等方
pairs 是 evenly 分布式 在 对等方
任何 对等方 可以
查询数据库 与 a 关键
数据库 returns 值 用于 关键
到 resolve 查询, small 编号的 报文 exchanged 之间
对等方
每个 对等方 仅 知道 关于 a small 编号 的 其他 对等方
Robust 到 对等方 coming 和 going (churn)
分布式 Hash 表 (DHT)

Assign key-value pairs to peers
rule: assign (key, value) pair to the peer that has the 
closestID.
convention: closest is the immediate successor of the key.
e.g., ID space of peers is {0,1,2,3,...,63}
suppose 8 peers whose IDs are: 1,12,13,25,32,40,48,60
If key = 51, then assigned to peer 60
If key = 60, then assigned to peer 60
If key = 61, then assigned to peer 1
HASH the 
data’s name 
Peer ID
114
Data ID vs. peer ID  ?
Assign key-value pairs 到 对等方
rule: assign (关键, 值) pair 到 对等方 这 有
convention: closest 是immediate 后继的 key.
ID 空间 的 对等方 是 {0,1,2,3,...,63}
suppose 8 对等方 whose IDs是: 1,12,13,25,32,40,48,60
如果 关键 =
, 则 assigned 到 对等方
如果 关键 =
, 则 assigned 到 对等方
如果 关键 =
, 则 assigned 到 对等方
数据’s 名称
对等方 ID
数据 ID vs. 对等方 ID ?

1
12
13
25
32
40
48
60
Circular DHT
each peer onlyaware of 
immediate successor and 
predecessor.
“overlay network”
115
每个 对等方
仅aware 的
immediate 后继 和
前驱
overlay 网络

1
12
13
25
32
40
48
60
What is the value
associated with key 53 ?
value
O(N) messages
on avgerage to resolve
query, when there
are Npeers
Resolving a query
116
什么 是 值
associated 与 关键 53 ?
值
报文
在 avgerage 到 resolve
查询, 当 存在
是
对等方
Resolving a 查询

Circular DHT with shortcuts
•each peer keeps track of IP addresses of predecessor, successor, short 
cuts.
•reduced from 6 to 3 messages.
•possible to design shortcuts with O(log N) neighbors, O(log N) messages 
in query
1
12
13
25
32
40
48
60
What is the value for
key 53
value
117
Circular DHT 与 shortcuts
每个 对等方 keeps track 的 IP 地址 的前驱, 后继, short
reduced 从 6 到 3 messages.
可能 到 design shortcuts 与邻居,报文
在 查询
什么 是 值 用于
关键 53
值

Peer churn
example: peer 5 abruptly leaves
1
3
4
5
8
10
12
15
handling peer churn:
peers may come and go (churn)
each peer knows address of its two 
successors 
each peer periodically pings its 
two successors to check aliveness
if immediate successor leaves, choose next 
successor as new immediate successor
118
对等方 churn
示例: 对等方 5 abruptly leaves
处理 对等方 churn:
对等方 可能 come 和 go (churn)
每个 对等方 知道 地址 的 它的 两个
每个 对等方 periodically pings 它的
两个 successors 到 查看 aliveness
如果 immediate 后继 leaves, 选择 下一个
后继 as 新的 immediate 后继

Peer churn
example: peer 5 abruptly leaves
peer 4 detects peer 5’s departure; makes 8 its immediate successor
4 asks 8 who its immediate successor is; makes 8’s immediate 
successor its second successor.
1
3
4
8
10
12
15
handling peer churn:
peers may come and go (churn)
each peer knows address of its two 
successors 
each peer periodically pings its 
two successors to check aliveness
if immediate successor leaves, choose 
next successor as new immediate 
successor
119
对等方 churn
示例: 对等方 5 abruptly leaves
对等方 4 detects 对等方 5
s departure; makes 8 它的 immediate 后继
4 asks 8 who 它的 immediate 后继 是; makes 8
后继 它的 second successor.
处理 对等方 churn:
对等方 可能 come 和 go (churn)
每个 对等方 知道 地址 的 它的 两个
每个 对等方 periodically pings 它的
两个 successors 到 查看 aliveness
如果 immediate 后继 leaves, 选择
下一个 后继as 新的 immediate
后继

2-120
Chapter 2: outline
2.1 principles of 
network applications
app architectures
app requirements
2.2 Web and HTTP
2.3 FTP (cancel)
2.4 electronic mail
SMTP, POP3, IMAP
2.5 DNS
2.6 P2P applications
2.7 video streaming 
and content 
distribution 
networks (CDNs)
2.8 socket 
programming with 
UDP and TCP
第 2 章: 提纲
2.1 原理 的
网络 应用
应用 architectures
应用 requirements
2.2 Web 和 HTTP
2.4 electronic 邮件
2.6 P2P 应用
2.7 视频 streaming
和 内容
网络 (CDNs)
2.8 套接字
编程 与
UDP 和 TCP

2-121
Video Streaming and CDNs: context
•Netflix, YouTube: 37%, 16% of downstream residential ISP 
traffic
•~1B YouTube users, ~75M Netflix users
challenge:  scale - how to reach ~1B users?
•single mega-video server won’t work (why?)
challenge: heterogeneity
different users have different capabilities (e.g., wired versus 
mobile; bandwidth rich versus bandwidth poor)
solution: distributed, application-level infrastructure
video traffic: major consumer of Internet bandwidth
视频 Streaming 和 CDNs: context
Netflix, YouTube: 37%, 16% 的 downstream residential ISP
流量
~1B YouTube 用户, ~75M Netflix 用户
挑战: scale - 如何 到 reach ~1B 用户?
单个 mega-video 服务器 won’t 工作 (why?)
挑战: heterogeneity
不同的 用户 有 不同的 capabilities (e.g., 有线 与
移动; 带宽 rich 与 带宽 poor)
分布式, 应用-level 基础设施
视频 流量: major consumer 的 Internet 带宽

video: sequence of images displayed 
at constant rate
e.g., 24 images/sec
digital image: array of pixels
each pixel represented by bits
coding: use redundancy withinand 
betweenimages to decrease # bits 
used to encode image
spatial (within image)
temporal (from one image to next)
Multimedia: video
..........................
spatial coding example: instead 
of sendingN values of same 
color (all purple), send only two 
values: color  value (purple)  and 
number of repeated values (N)
..........................
framei
framei+1
temporal coding example: 
instead of sending 
complete frame at i+1, 
send only differences from 
frame i
2-122
视频: 序列 的 images displayed
在 constant 速率
digital image: array 的 pixels
每个 pixel represented 由 比特
coding: 使用 redundancy
在...内
和
之间images 到 减小 # 比特
用于 到 encode image
spatial (在...内 image)
temporal (从 一个 image 到 下一个)
Multimedia: 视频
spatial coding 示例:
的 发送值 的 相同
color (所有 purple), 发送 仅 两个
值: color 值 (purple) 和
编号 的 repeated 值 (
帧
帧
temporal coding 示例:
instead 的 发送
完整的 帧 在 i+1,
发送 仅 differences 从
帧 i

Multimedia: video
CBR: (constant bit rate): video encoding rate fixed
VBR:  (variable bit rate): video encoding rate changes 
as amount of spatial, temporal coding changes 
examples:
•MPEG 1 (CD-ROM) 1.5 Mbps
•MPEG2 (DVD) 3-6 Mbps
•MPEG4 (often used in Internet, < 1 Mbps)
..........................
spatial coding example: instead 
of sendingN values of same 
color (all purple), send only two 
values: color  value (purple)  and 
number of repeated values (N)
..........................
framei
framei+1
temporal coding example: 
instead of sending 
complete frame at i+1, 
send only differences from 
frame i
2-123
Multimedia: 视频
CBR: (constant 比特 速率):
视频 encoding 速率 fixed
VBR: (变量 比特 速率):视频 encoding 速率 变化
as 数量 的 spatial, temporal coding 变化
示例:
MPEG4 (经常 用于 在 Internet, < 1 Mbps)
spatial coding 示例:
的 发送值 的 相同
color (所有 purple), 发送 仅 两个
值: color 值 (purple) 和
编号 的 repeated 值 (
帧
帧
temporal coding 示例:
instead 的 发送
完整的 帧 在 i+1,
发送 仅 differences 从
帧 i

Streaming stored video: 
simple scenario:
video server
(stored video)
client
Internet
2-124
Main challenges: 
server-to-client bandwidth will varyover time, with changing 
network congestion levels (in house, access network, 
network core, video server)
packet loss, delay due to congestion will delay playout, or 
result in poor video quality
Streaming 存储的 视频:
simple 场景
视频 服务器
(存储的 视频)
客户
服务器-to-客户 带宽 将在 时间, 与 changing
网络 拥塞 levels (在 house, 访问 网络,
网络核心, 视频 服务器)
分组 丢包, 时延 由于 到 拥塞 将 时延 playout, 或
result 在 poor 视频 quality

Streaming stored video
1. video
recorded 
(e.g., 30 
frames/sec)
2. video
sent
streaming: at this time, client  playing out 
early part of video, while server still sending 
later part of video
time
3. video received, played out at client
(30 frames/sec)
network delay
(fixedin this 
example)
Application Layer: 2-125
Streaming 存储的
1. 视频
帧/sec)
视频
已发送
: 在 这个 时间, 客户 playing 查看
early 部分 的 视频, while 服务器 仍然 发送
later 部分 的 视频
时间
视频 已收到, played 查看 在 客户
(30 帧/sec)
网络 时延
在 这个
示例)
应用层: 2-125

Streaming stored video: challenges
continuous playout constraint: during client 
video playout, playout timing must match 
original timing 
•... but network delays are variable (jitter), so will 
need client‐side buffer to match continuous playout 
constraint
other challenges:
•client interactivity: pause, fast‐forward, rewind, 
jump through video
•video packets may be lost, retransmitted
Application Layer: 2-126
Streaming 存储的
: during 客户
视频 playout, playout timing 必须 匹配
原始 timing
... 但是
网络 时延s 是 变量
(jitter), 因此 将
需要
客户‐一侧 缓冲区
到 匹配 continuous playout
其他 challenges:
客户 interactivity: pause, 快速‐转发, rewind,
jump 通过 视频
视频 分组 可能 是 丢失的, retransmitted
应用层: 2-126

Streaming stored video: playout buffering
constant bit 
rate video
transmission
time
variable
network
delay
client video
reception
constant bit 
rate video
playout at client
client playout
delay
buffered
video
client‐side buffering and playout delay: compensate for 
network‐added delay, delay jitter
Application Layer: 2-127
Streaming 存储的
constant 比特
速率 视频
传输
时间
变量
网络
时延
客户 视频
constant 比特
速率 视频
playout 在 客户
客户 playout
时延
视频
客户‐一侧 buffering 和 playout 时延:compensate 用于
网络‐added 时延, 时延 jitter
应用层: 2-127

buffered
video
Streaming multimedia: DASH
server:
divides video file into multiple chunks
each chunk encoded at multiple different rates
different rate encodings stored in different files
files replicated in various CDN nodes
manifest file: provides URLs for different chunks
client
?
client:
periodically estimates server‐to‐client bandwidth
consulting manifest, requests one chunk at a time 
•chooses maximum coding rate sustainable given current bandwidth
•can choose different coding rates at different points in time (depending 
on available bandwidth at time), and from different servers
...
...
...
Dynamic, Adaptive 
Streaming over HTTP
Application Layer: 2-128
服务器:
divides 视频 文件 进入 多路 块
每个 块 encoded 在多路 不同的 速率
不同的 速率 encodings 存储的 在 不同的 文件
文件 replicated 在 various CDN 节点
manifest 文件:
提供 URLs 用于 不同的 块
客户
客户:
periodically 估计 服务器‐到‐客户 带宽
consulting manifest, 请求 一个 块 在 a 时间
chooses maximum coding 速率 sustainable 给定的 当前 带宽
可以 选择 不同的 coding 速率 在 不同的 点 在 时间 (
在 可用的 带宽 在 时间), 和 从 不同的 服务器
treaming 在
应用层: 2-128

...
...
...
Streaming multimedia: DASH
“intelligence” at client: client 
determines
•whento request chunk (so that buffer 
starvation, or overflow does not occur)
•what encoding rate to request (higher 
quality when more bandwidth 
available) 
•whereto request chunk (can request 
from URL server that is “close” to 
client or has high available 
bandwidth) 
Streaming video = encoding +DASH + playout buffering
client
?
Application Layer: 2-129
在 客户:客户
当到 请求 块 (因此 这 缓冲区
starvation, 或 overflow 做 不 occur)
什么 encoding 速率
到 请求 (higher
quality 当 更多 带宽
可用的)
哪里到 请求 块 (可以 请求
从 URL 服务器 这 是 “关闭” 到
客户 或 有 high 可用的
带宽
Streaming 视频 =
客户
应用层: 2-129

Content distribution networks
challenge: how to stream content (selected from millions of 
videos) to hundreds of thousands of simultaneoususers?
option 1: single, large “mega-server”
single point of failure
point of network congestion
long path to distant clients
multiple copies of video sent over outgoing link
....quite simply: this solution doesn’t scale
2-130
内容分发网络
挑战:如何 到 stream 内容 (selected 从 millions 的
videos) 到 hundreds 的 thousands 的用户?
选项 1:单个, 大的 “mega-服务器”
单个 点 的 failure
点 的 网络 拥塞
long 路径 到 distant 客户
多路 copies 的 视频已发送 在 outgoing 链路
....quite simply: 这个 solution

Content distribution networks
challenge: how to stream content (selected from millions of 
videos) to hundreds of thousands of simultaneous users?
option 2: store/serve multiple copies of videos at multiple 
geographically distributed sites (CDN)
enter deep: push CDN servers deep into many access networks 
close to users
used by Akamai, 1700 locations
bring home: smaller number (10’s) of larger clusters in POPs near 
(but not within) access networks
used by Limelight
2-131
内容分发网络
挑战:如何 到 stream 内容 (selected 从 millions 的
videos) 到 hundreds 的 thousands 的 simultaneous 用户?
选项 2:store/serve 多路 copies 的 videos 在 多路
geographically 分布式 站点
push CDN 服务器 deep 进入 许多 接入网络
关闭 到 用户
用于 由 Akamai, 1700 locations
bring 归属:smaller 编号 (10’s) 的 larger clusters 在 POPs near
(但是 不 在...内) 接入网络
用于 由 Limelight

Content Distribution Networks (CDNs)
subscriber requests content from CDN
CDN: stores copies of content at CDN nodes 
•e.g. Netflix stores copies of MadMen
where’s Madmen?
manifest file
•directed to nearby copy, retrieves content
•may choose different copy if network path 
congested
2-132
2023.9.18
内容分发网络 (CDNs)
subscriber 请求 内容 从 CDN
CDN: stores copies 的 内容 在 CDN 节点
e.g. Netflix stores copies 的 MadMen
哪里’s Madmen?
manifest 文件
directed 到 nearby 复制, retrieves 内容
可能 选择 不同的 复制 如果 网络 路径
拥塞的

Content Distribution Networks (CDNs)
Internet host-host communication as a service
OTT challenges: coping with a congested Internet
from which CDN node to retrieve content?
viewer behavior in presence of congestion?
what content to place in which CDN node?
“over the top”
more .. in chapter 7
133
内容分发网络 (CDNs)
Internet 主机-主机 通信 as a 服务
coping 与 a 拥塞的 Internet
从 哪个 CDN 节点 到 retrieve 内容?
viewer behavior 在 presence 的 拥塞?
什么 内容 到 place 在 哪个 CDN 节点?
“在 top”
更多 .. 在 第 7 章

CDN content access: a closer look
Bob (client) requests video http://netcinema.com/6Y7B23V
video stored in CDN at http://KingCDN.com/NetC6y&B23V
netcinema.com
KingCDN.com
1
1. Bob gets URL for video 
http://netcinema.com/6Y7B23V
from netcinema.com web page
2
2. resolve http://netcinema.com/6Y7B23V
via Bob’s local DNS
netcinema’s
authoratative DNS
3
3. netcinema’s DNS returns URL 
http://KingCDN.com/NetC6y&B
23V
4
4&5. Resolve 
http://KingCDN.com/NetC6y&B23
via KingCDN’s authoritative DNS, 
which returns IP address of KingCDN 
server  with video
5
6. request video from
KINGCDN server,
streamed via HTTP
KingCDN
authoritative DNS
Bob’s 
local DNS
server
2-134
CDN 内容 访问: a closer look
Bob (客户) 请求 视频
视频 存储的 在 CDN 在 HTTP://KingCDN.com/NetC6y&B23V
1. Bob 获得 URL 用于 视频
从 netcinema.com Web 页面
通过 Bob’s 本地 DNS
通过 KingCDN’s 权威 DNS,
哪个 returns IP 地址 的 KingCDN
服务器 与 视频
6. 请求 视频 从
KINGCDN 服务器,
streamed 通过 HTTP
权威 DNS
本地 DNS
服务器

Case study: Netflix
1
1. Bob manages      
Netflix account
Netflix registration,
accounting servers
Amazon cloud
CDN
server 
2
2. Bob browses
Netflix video
3
3. Manifest file
returned for 
requested video
4. DASH 
streaming
upload copies of 
multiple versions of 
video to CDN servers
CDN
server 
CDN
server 
2-135
accounting 服务器
服务器
Netflix 视频
3. Manifest 文件
returned 用于
requested 视频
upload copies 的
多路 versions 的
视频 到 CDN 服务器
服务器
服务器

2-136
Chapter 2: outline
2.1 principles of network 
applications
2.2 Web and HTTP
2.3 electronic mail
•SMTP, POP3, IMAP
2.4 DNS
2.5 P2P applications
2.6 video streaming and 
content distribution 
networks
2.7 socket programming 
with UDP and TCP (DIY)
第 2 章: 提纲
2.1 原理 的 网络
应用
2.2 Web 和 HTTP
2.3 electronic 邮件
2.5 P2P 应用
2.6 视频 streaming 和
内容分发
网络
2.7 套接字编程
与 UDP 和 TCP (DIY)

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
