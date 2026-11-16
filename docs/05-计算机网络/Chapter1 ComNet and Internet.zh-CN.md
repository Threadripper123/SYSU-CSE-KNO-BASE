# Chapter1 ComNet and Internet.zh-CN

> 主题库：计算机网络　｜　源文件：Chapter1 ComNet and Internet.zh-CN.pdf　｜　146,207 字符　｜　108 个片段　｜　更新于 2026-11-11


2024/09/10
1
Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
1-2
Chapter 1 Introduction
A note on the use of these PowerPoint slides:We’re making these slides freely available to all (faculty, stu
dents, 
readers). They’re in PowerPoint form so you see the animations;
 
and can add, modify, and delete slides  (including this one) an
d 
slide content to suit your needs
. They obviously represent a 
lot
of 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention the
ir 
source (after all, we’d like people to use our book!)

If you post any slides on a www 
site, that you note that they 
are adapted from (or perhaps ident
ical to) our slides, and note
 
our copyright of this material.
For a revision history, see the slide note for this page. Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: A Top-Down Approach 8
th
edition 
Jim Kurose, Keith RossPearson, 2020
3
Computer Network is different from other fields:

Computer networks are 
artificial systems
, not natural products, , 
which leads to the existence of a large number of 
artificial concepts
；

The network is 
large, complex, and time-varying
, making it difficult to 
describe using general mathematical models
；

It involves 
low-level physical networks
, 
middle-level data analysis
, 
and 
high-level behavior understanding
；

Data-driven analysis is only a part of it, while 
physical mechanisms 
and principles
are more important
。

Only by mastering the 
underlying core technology 
can one gain the 
right to speak
。
4
The first question:
Content(text, multimedia)
Computer network
A
B
How to achieve it?
What happens to the network in 
end-to-end communication?
计算机网络
2024. 秋季

2024/09/10
1
Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
1-2
Chapter 1 Introduction
A note on the use of these PowerPoint slides:We’re making these slides freely available to all (faculty, stu
dents, 
readers). They’re in PowerPoint form so you see the animations;
 
and can add, modify, and delete slides  (including this one) an
d 
slide content to suit your needs
. They obviously represent a 
lot
of 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention the
ir 
source (after all, we’d like people to use our book!)

If you post any slides on a www 
site, that you note that they 
are adapted from (or perhaps ident
ical to) our slides, and note
 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: A Top-Down Approach 8
th
edition 
Jim Kurose, Keith RossPearson, 2020
3
Computer Network is different from other fields:
Computer networks are 
artificial systems
, not natural products, , 
which leads to the existence of a large number of 
artificial concepts
；

The network is 
large, complex, and time-varying
, making it difficult to 
describe using general mathematical models
；

It involves 
low-level physical networks
, 
middle-level data analysis
, 
and 
high-level behavior understanding
；

Data-driven analysis is only a part of it, while 
physical mechanisms 
and principles
are more important
。

Only by mastering the 
underlying core technology 
can one gain the 
right to speak
。
4
The first question:
Content(text, multimedia)
Computer network
A
B
How to achieve it?
What happens to the network in 
end-to-end communication?
第 1 章 引言
说明 在 使用 的 这些 PowerPoint 幻灯片:我们’re making 这些 幻灯片 freely 可用的 到 所有 (faculty, stureaders). They’re 在 PowerPoint form 因此 你 见 animations;
和 可以 添加, 修改, 和 删除 幻灯片 (包括 这个 一个)
幻灯片 内容 到 suit your 需要
的
工作 在 我们的 part. 在 返回 用于 使用, 我们 仅 ask following:
如果 你 使用 这些 幻灯片 (e.g., 在 a 课堂) 这 你 mention源 (之后 所有, 我们’d 像 people 到 使用 我们的 book!)如果 你 post 任何 幻灯片 在 a 网站
站点, 这 你 说明 这 they
是 adapted 从 (或 perhaps ident
ical 到) 我们的 幻灯片, 和 说明
我们的 版权 的 这个 material.
用于 a revision 历史, 见 幻灯片 说明 用于 这个 page.
谢谢 和 使用愉快! JFK/KWR
所有 材料 版权 1996-2023
J.F Kurose 和 K.W. Ross, 所有 Rights Reserved
计算机 网络:自顶向下 方法

2024/09/10
1
Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
1-2
Chapter 1 Introduction
A note on the use of these PowerPoint slides:We’re making these slides freely available to all (faculty, stu
dents, 
readers). They’re in PowerPoint form so you see the animations;
 
and can add, modify, and delete slides  (including this one) an
d 
slide content to suit your needs
. They obviously represent a 
lot
of 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention the
ir 
source (after all, we’d like people to use our book!)

If you post any slides on a www 
site, that you note that they 
are adapted from (or perhaps ident
ical to) our slides, and note
 
our copyright of this material.
For a revision history, see the slide note for this page. Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: A Top-Down Approach 8
th
edition 
Jim Kurose, Keith RossPearson, 2020
3
Computer Network is different from other fields:

Computer networks are 
artificial systems
, not natural products, , 
which leads to the existence of a large number of 
artificial concepts
；

The network is 
large, complex, and time-varying
, making it difficult to 
describe using general mathematical models
；

It involves 
low-level physical networks
, 
middle-level data analysis
, 
and 
high-level behavior understanding
；

Data-driven analysis is only a part of it, while 
physical mechanisms 
and principles
are more important
。

Only by mastering the 
underlying core technology 
can one gain the 
right to speak
。
4
The first question:
Content(text, multimedia)
Computer network
A
B
How to achieve it?
What happens to the network in 
end-to-end communication?
计算机 网络 是 不同的 从 其他 字段:
计算机 网络 是
artificial 系统
, 不 自然的 products, ,
哪个 leads 到 existence 的 a 大的 编号 的
网络 是
大的, complex, 和 time-varying
, making 它 difficult 到
describe 使用 general mathematical 模型
它 involves
low-level 物理 网络
middle-level 数据 analysis
和
Data-driven analysis 是 仅 a 部分 的 它, while
物理 mechanisms
和 原理
是 更多 重要
仅 由 mastering
underlying 核心 technology
可以 一个 gain
right 到 speak

2024/09/10
1
Computer Networking
谢逸
中山大学·计算机学院
2024. Fall
1-2
Chapter 1 Introduction
A note on the use of these PowerPoint slides:We’re making these slides freely available to all (faculty, stu
dents, 
readers). They’re in PowerPoint form so you see the animations;
 
and can add, modify, and delete slides  (including this one) an
d 
slide content to suit your needs
. They obviously represent a 
lot
of 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention the
ir 
source (after all, we’d like people to use our book!)

If you post any slides on a www 
site, that you note that they 
are adapted from (or perhaps ident
ical to) our slides, and note
 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: A Top-Down Approach 8
th
edition 
Jim Kurose, Keith RossPearson, 2020
3
Computer Network is different from other fields:
Computer networks are 
artificial systems
, not natural products, , 
which leads to the existence of a large number of 
artificial concepts
；

The network is 
large, complex, and time-varying
, making it difficult to 
describe using general mathematical models
；

It involves 
low-level physical networks
, 
middle-level data analysis
, 
and 
high-level behavior understanding
；

Data-driven analysis is only a part of it, while 
physical mechanisms 
and principles
are more important
。

Only by mastering the 
underlying core technology 
can one gain the 
right to speak
。
4
The first question:
Content(text, multimedia)
Computer network
A
B
How to achieve it?
What happens to the network in 
end-to-end communication?
第一个 问题:
内容(text, multimedia)
计算机 网络
如何 到 achieve 它?
什么 happens 到 网络 在
end-to-end 通信?

2024/09/10
2
Assignments:

Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Practice: 18, 19

Keywords : protocol, network edge, access network, Packet Switching, Circuit Switching, delay, Encapsulation
5
下横杠题目为必做题
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-6
Internet
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional 
ISP
datacenter network
content provider network
Packet switches
: forward 
packets (chunks of data)

routers
, 
switches
Communication links

fiber, copper, radio, satellite

transmission rate: 
bandwidth
Billions of connected computing 
devices
: 

hosts
= end systems 

running 
network 
apps 
at 
Internet’s “edge”
Networks

collection of devices, routers, links: managed by an organization

Internet: “
network of networks”

Interconnected ISPs
The Internet: a “nuts and bolts” view
Introduction: 1-8
Telephone 
network
television 
network
Mobile 
network
Power 
network
......
satellite 
network
Internet: Unified protocols, policies, and rules
video 
service
e-
commerce
Game 
service
Web 
Services
Social 
communication
......
作业：
关键词： 协议, 网络边缘, 访问 网络,分组 交换, 电路 交换, 时延, Encapsulation

2024/09/10
2
Assignments:
Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Practice: 18, 19

Keywords : protocol, network edge, access network, Packet Switching, Circuit Switching, delay, Encapsulation
5
下横杠题目为必做题
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-6
Internet
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet switches
: forward 
packets (chunks of data)

routers
, 
switches
Communication links

fiber, copper, radio, satellite

transmission rate: 
bandwidth
Billions of connected computing 
devices
: 

hosts
= end systems 

running 
network 
apps 
at 
Internet’s “edge”
Networks

collection of devices, routers, links: managed by an organization

Internet: “
network of networks”

Interconnected ISPs
The Internet: a “nuts and bolts” view
Introduction: 1-8
Telephone 
network
television 
network
Mobile 
network
Power 
network
......
satellite 
network
Internet: Unified protocols, policies, and rules
video 
service
e-
commerce
Game 
service
Web 
Services
Social 
communication
......
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络 结构
时延, 丢包, 吞吐量 在 网络协议分层, 服务模型网络 under attack: 安全
历史

2024/09/10
2
Assignments:

Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Practice: 18, 19

Keywords : protocol, network edge, access network, Packet Switching, Circuit Switching, delay, Encapsulation
5
下横杠题目为必做题
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-6
Internet
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet switches
: forward 
packets (chunks of data)

routers
, 
switches
Communication links

fiber, copper, radio, satellite

transmission rate: 
bandwidth
Billions of connected computing 
devices
: 

hosts
= end systems 

running 
network 
apps 
at 
Internet’s “edge”
Networks

collection of devices, routers, 
links: managed by an organization

Internet: “
network of networks”

Interconnected ISPs
The Internet: a “nuts and bolts” view
Introduction: 1-8
Telephone 
network
television 
network
Mobile 
network
Power 
network
......
satellite 
network
Internet: Unified protocols, policies, and rules
video 
service
e-
commerce
Game 
service
Web 
Services
Social 
communication
......
Internet: a “nuts 和 bolts” view
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的
网络
内容提供商网络
分组 交换机
: 转发
分组 (块 的 数据)
路由器
交换机
通信 链路
传输 速率:
带宽
Billions 的 连接的
设备
主机
= 端 系统
运行
网络
应用
在
Internet’s “边缘”
网络
collection 的 设备, 路由器,
链路: 管理的 由组织

2024/09/10
2
Assignments:
Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33

Practice: 18, 19

Keywords : protocol, network edge, access network, Packet Switching, Circuit Switching, delay, Encapsulation
5
下横杠题目为必做题
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-6
Internet
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet switches
: forward 
packets (chunks of data)

routers
, 
switches
Communication links

fiber, copper, radio, satellite

transmission rate: 
bandwidth
Billions of connected computing 
devices
: 

hosts
= end systems 

running 
network 
apps 
at 
Internet’s “edge”
Networks

collection of devices, routers, links: managed by an organization

Internet: “
network of networks”

Interconnected ISPs
The Internet: a “nuts and bolts” view
Introduction: 1-8
Telephone 
network
television 
network
Mobile 
network
Power 
network
......
satellite 
network
Internet: Unified protocols, policies, and rules
video 
service
e-
commerce
Game 
service
Web 
Services
Social 
communication
......
网络 的 网络”
Internet: a “nuts 和 bolts” view
引言: 1-8
网络
网络
移动
网络
网络
网络
Internet: Unified 协议, policies, 和 rules
视频
服务
服务
服务
通信

2024/09/10
3
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network

protocols are everywhere
•
control sending, receiving of messages
•
e.g., HTTP (Web), streaming video, 
Skype, TCP, IP, WiFi, 4G, Ethernet
Ethernet
HTTP
Skype
IP
WiFi
4G
TCP
Streamingvideo

Internet  standards
•
RFC: Request for Comments
•
IETF: Internet Engineering Task Force
Introduction: 1-9

Infrastructure 
that provides 
services to applications:

Web, streaming video, multimedia teleconferencing, email, games, e-commerce, social media, inter-connected appliances, ...
The Internet: a “services” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
HTTP
Skype
Streamingvideo

provides 
programming interface 
to distributed applications:
•
“hooks” allowing sending/receiving  apps to “
connect
”
to, use  Internet transport 
service
•
provides service options, analogous to postal service
Introduction: 1-10
Applications/services
Infrastructure
What’s a protocol?
human protocols:

“
what
’
s the time?
”

“
I have a question
”

introductions
network protocols:

machines rather than humans

all communication activity in Internet 
governed by protocols
1-11
Hi
Hi
Got the
time?
2:00
TCP connection
response
Get
http://www.awl.com/kurose-ross
<file>
time
TCP connection
request
a human protocol           and          a computer network prot
ocol:
specific
msgs
sent
specific
actions
taken when msgs
received, or other events
What’s a protocol?
network protocols:
protocols
define
format
,
order
of
msgs   sent   and
received
among   network
entities,  and
actions  taken
on
msg
transmission,
receipt
1-12
例如
: 
中国广东省广州市大学城中山大学计算机学院
School of Computer Science and Engineering, Sun Yat-sen University, Guangzhou Higher Education Mega Center, China
计算机
大学城
广州市
中国
学院
中山大学
广东省
Internet: a “nuts 和 bolts” view
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的
网络
内容提供商网络
协议 是 everywhere
控制 发送, 接收 的 报文
e.g., HTTP (Web), streaming 视频,
Skype, TCP, IP, WiFi, 4G, 以太网
以太网
视频
RFC: 请求 用于 Comments
引言: 1-9

2024/09/10
3
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network

protocols are everywhere
•
control sending, receiving of messages
•
e.g., HTTP (Web), streaming video, Skype, TCP, IP, WiFi, 4G, Ethernet
Ethernet
HTTP
Skype
IP
WiFi
4G
TCP
Streamingvideo

Internet  standards
•
RFC: Request for Comments
•
IETF: Internet Engineering Task Force
Introduction: 1-9

Infrastructure 
that provides 
services to applications:

Web, streaming video, multimedia 
teleconferencing, email, games, e-
commerce, social media, inter-
connected appliances, ...
The Internet: a “services” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
HTTP
Skype
Streamingvideo

provides 
programming interface 
to distributed applications:
•
“hooks” allowing sending/receiving  apps to 
“
connect
”
to, use  Internet transport 
service
•
provides service options, analogous to 
postal service
Introduction: 1-10
Applications/services
Infrastructure
What’s a protocol?
human protocols:
“
what
’
s the time?
”

“
I have a question
”

introductions
network protocols:

machines rather than humans

all communication activity in Internet 
governed by protocols
1-11
Hi
Hi
Got the
time?
2:00
TCP connection
response
Get
http://www.awl.com/kurose-ross
<file>
time
TCP connection
request
a human protocol           and          a computer network prot
ocol:
specific
msgs
sent
specific
actions
taken when msgs
received, or other events
What’s a protocol?
network protocols:
protocols
define
format
,
order
of
msgs   sent   and
received
among   network
entities,  and
actions  taken
on
msg
transmission,
receipt
1-12
例如
: 
中国广东省广州市大学城中山大学计算机学院
School of Computer Science and Engineering, Sun Yat-sen University, Guangzhou Higher Education 
Mega Center, China
计算机
大学城
广州市
中国
学院
中山大学
广东省
基础设施
这 提供
服务 到 应用:
Web, streaming 视频, multimedia
teleconferencing, 电子邮件, games, e-连接的 appliances, ...
Internet：“服务”视角
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的
网络
内容提供商网络
视频
提供
编程 接口
到 分布式 应用:
“hooks” allowing sending/receiving 应用 到
连接
到, 使用 Internet 运输
服务
提供 服务 options, analogous 到
postal 服务
引言: 1-10
应用/services
基础设施

2024/09/10
3
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network

protocols are everywhere
•
control sending, receiving of messages
•
e.g., HTTP (Web), streaming video, Skype, TCP, IP, WiFi, 4G, Ethernet
Ethernet
HTTP
Skype
IP
WiFi
4G
TCP
Streamingvideo

Internet  standards
•
RFC: Request for Comments
•
IETF: Internet Engineering Task Force
Introduction: 1-9

Infrastructure 
that provides 
services to applications:

Web, streaming video, multimedia teleconferencing, email, games, e-commerce, social media, inter-connected appliances, ...
The Internet: a “services” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
HTTP
Skype
Streamingvideo

provides 
programming interface 
to distributed applications:
•
“hooks” allowing sending/receiving  apps to “
connect
”
to, use  Internet transport 
service
•
provides service options, analogous to postal service
Introduction: 1-10
Applications/services
Infrastructure
What’s a protocol?
human protocols:
“
what
’
s the time?
”

“
I have a question
”

introductions
network protocols:
machines rather than humans

all communication activity in Internet 
governed by protocols
1-11
Hi
Hi
Got the
time?
2:00
TCP connection
response
Get
http://www.awl.com/kurose-ross
<file>
time
TCP connection
request
a human protocol           and          a computer network prot
ocol:
specific
msgs
sent
specific
actions
taken when msgs
received, or other events
What’s a protocol?
network protocols:
protocols
define
format
,
order
of
msgs   sent   and
received
among   network
entities,  and
actions  taken
on
msg
transmission,
receipt
1-12
例如
: 
中国广东省广州市大学城中山大学计算机学院
School of Computer Science and Engineering, Sun Yat-sen University, Guangzhou Higher Education Mega Center, China
计算机
大学城
广州市
中国
学院
中山大学
广东省
什么’s a 协议?
human 协议:
什么
s 时间?
I 有 a 问题
网络 协议:
machines rather 比 humans所有 通信 activity 在
governed 由 协议
时间?
TCP 连接
响应
获得
时间
TCP 连接
请求
a human 协议
和
a 计算机 网络 prot
特定的
已发送
特定的
动作
经过 当 msgs
已收到, 或 其他 events

2024/09/10
3
The Internet: a “nuts and bolts” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter 
network
content 
provider network

protocols are everywhere
•
control sending, receiving of messages
•
e.g., HTTP (Web), streaming video, Skype, TCP, IP, WiFi, 4G, Ethernet
Ethernet
HTTP
Skype
IP
WiFi
4G
TCP
Streaming
video

Internet  standards
•
RFC: Request for Comments
•
IETF: Internet Engineering Task Force
Introduction: 1-9

Infrastructure 
that provides 
services to applications:

Web, streaming video, multimedia teleconferencing, email, games, e-
commerce, social media, inter-connected appliances, ...
The Internet: a “services” view
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
HTTP
Skype
Streamingvideo

provides 
programming interface 
to distributed applications:
•
“hooks” allowing sending/receiving  apps to 
“
connect
”
to, use  Internet transport 
service
•
provides service options, analogous to 
postal service
Introduction: 1-10
Applications/services
Infrastructure
What’s a protocol?
human protocols:
“
what
’
s the time?
”

“
I have a question
”

introductions
network protocols:
machines rather than humans

all communication activity in Internet 
governed by protocols
1-11
Hi
Hi
Got the
time?
2:00
TCP connection
response
Get
http://www.awl.com/kurose-ross
<file>
time
TCP connection
request
a human protocol           and          a computer network prot
ocol:
specific
msgs
sent
specific
actions
taken when msgs
received, or other events
What’s a protocol?
network protocols:
protocols
define
format
,
order
of
msgs   sent   and
received
among   network
entities,  and
actions  taken
on
msg
transmission,
receipt
1-12
例如
: 
中国广东省广州市大学城中山大学计算机学院
School of Computer Science and Engineering, Sun 
Yat-sen University, Guangzhou Higher Education 
Mega Center, China
计算机
大学城
广州市
中国
学院
中山大学
广东省
什么’s a 协议?
网络 协议:
协议
格式
的
已发送
和
已收到
之间
网络
entities, 和
动作 经过
在
传输,
School 的 计算机 Science 和 Engineering, Sun
Mega 中心, China

2024/09/10
4
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2 network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-13
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Introduction: 1-14
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical 
media:

wired, wireless communication 
links
Introduction: 1-15
A closer look at Internet structure
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication links
Network core: 

interconnected routers

network of networks
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Introduction: 1-16
第 1 章: 引言
1.1 什么
是
1.2 网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络 结构
时延, 丢包, 吞吐量 在 网络协议分层, 服务模型网络 under attack: 安全
历史

2024/09/10
4
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2 network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-13
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Introduction: 1-14
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication 
links
Introduction: 1-15
A closer look at Internet structure
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication links
Network core: 

interconnected routers

network of networks
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Introduction: 1-16
closer look 在 Internet 结构
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的 ISP
网络
内容提供商网络
网络边缘:
主机: 客户 和 服务器服务器 经常 在 数据 centers
引言: 1-14

2024/09/10
4
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2 network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-13
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Introduction: 1-14
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication 
links
Introduction: 1-15
A closer look at Internet structure
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication links
Network core: 

interconnected routers

network of networks
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Introduction: 1-16
closer look 在 Internet 结构
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的 ISP
网络
内容提供商网络
网络边缘:
主机: 客户 和 服务器服务器 经常 在 数据 centers
接入网络, 物理
有线, 无线 通信
链路
引言: 1-15

2024/09/10
4
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2 network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-13
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Introduction: 1-14
A closer look at Internet structure
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication 
links
Introduction: 1-15
A closer look at Internet structure
Network edge:

hosts: clients and servers

servers often in data centers
Access networks, physical media:

wired, wireless communication links
Network core: 

interconnected routers

network of networks
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Introduction: 1-16
closer look 在 Internet 结构
网络边缘:
主机: 客户 和 服务器服务器 经常 在 数据 centers
接入网络, 物理媒体:
有线, 无线 通信 链路
网络核心:
interconnected 路由器网络 的 网络
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的 ISP
网络
内容提供商网络
引言: 1-16

2024/09/10
5
Access networks and physical media
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Q: How to connect end 
systems to edge router?

residential access nets

institutiona
l access networks (school, 
company)

mobile access networks (WiFi, 4G/5G)
Introduction: 1-17
Access net: digital subscriber line (DSL)
central office
ISP
telephonenetwork
DSLAM
voice, data transmitted
at different frequencies over
dedicated 
line to central office

use 
existing
telephone line to central office DSLAM

data over DSL phone line goes to Internet

voice over DSL phone line goes to telephone net

< 2.5 Mbps upstream transmission rate (typically < 1 Mbps)

< 24 Mbps downstream transmission rate (typically < 10 Mbps)
DSL
modem
splitter
DSL access 
multiplexer
1-18
4KHz
电话
数据
Access net: cable network
cable
modem
splitter
...
cable headend
Channels
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
D
A
T
A
D
A
T
A
C
O
N
T
R
O
L
12 34
56789
frequency division multiplexing:
different channels transmitted
in different frequency bands
1-19

HFC: hybrid fiber coax

asymmetric: up to 30Mbps downstream transmission rate, 2 Mbps upstream transmission rate

network 
of cable, fiber attaches homes to ISP router

homes 
share access network
to cable headend

unlike DSL, which has dedicated access to central office
Access net: cable network
1-20
data, TV transmitted at different 
frequencies over 
shared
cable 
distribution network
cable
modem
splitter
...
cable headend
CMTS
ISP
cable modem
termination system
接入网络 和 物理媒体
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的 ISP
网络
内容提供商网络
Q: 如何 到 连接 端
系统 到 边缘 路由器?
residential 访问 nets
l 接入网络 (school,
移动 接入网络 (WiFi, 4G/5G)
引言: 1-17

2024/09/10
5
Access networks and physical media
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Q: How to connect end 
systems to edge router?

residential access nets

institutiona
l access networks (school, 
company)

mobile access networks (WiFi, 4G/5G)
Introduction: 1-17
Access net: digital subscriber line (DSL)
central office
ISP
telephonenetwork
DSLAM
voice, data transmitted
at different frequencies over
dedicated 
line to central office

use 
existing
telephone line to central office DSLAM

data over DSL phone line goes to Internet

voice over DSL phone line goes to telephone net

< 2.5 Mbps upstream transmission rate (typically < 1 Mbps)

< 24 Mbps downstream transmission rate (typically < 10 Mbps)
DSL
modem
splitter
DSL access 
multiplexer
1-18
4KHz
电话
数据
Access net: cable network
cable
modem
splitter
...
cable headend
Channels
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
D
A
T
A
D
A
T
A
C
O
N
T
R
O
L
12 34
56789
frequency division multiplexing:
different channels transmitted
in different frequency bands
1-19

HFC: hybrid fiber coax

asymmetric: up to 30Mbps downstream transmission rate, 2 Mbps 
upstream transmission rate

network 
of cable, fiber attaches homes to ISP router

homes 
share access network
to cable headend

unlike DSL, which has dedicated access to central office
Access net: cable network
1-20
data, TV transmitted at different 
frequencies over 
shared
cable 
distribution network
cable
modem
splitter
...
cable headend
CMTS
ISP
cable modem
termination system
访问 网络: digital subscriber 线路 (DSL)
网络
voice, 数据 已传输
在 不同的 frequencies 在
线路 到 central office
使用
telephone 线路 到 central office DSLAM
数据 在 DSL phone 线路 goes 到 Internetvoice 在 DSL phone 线路 goes 到 telephone 网络
< 2.5 Mbps 上游 传输 速率 (通常 < 1 Mbps)< 24 Mbps downstream 传输 速率 (通常 < 10 Mbps)
DSL 访问

2024/09/10
5
Access networks and physical media
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Q: How to connect end 
systems to edge router?

residential access nets

institutiona
l access networks (school, 
company)

mobile access networks (WiFi, 4G/5G)
Introduction: 1-17
Access net: digital subscriber line (DSL)
central office
ISP
telephonenetwork
DSLAM
voice, data transmitted
at different frequencies over
dedicated 
line to central office

use 
existing
telephone line to central office DSLAM

data over DSL phone line goes to Internet

voice over DSL phone line goes to telephone net

< 2.5 Mbps upstream transmission rate (typically < 1 Mbps)

< 24 Mbps downstream transmission rate (typically < 10 Mbps)
DSL
modem
splitter
DSL access 
multiplexer
1-18
4KHz
电话
数据
Access net: cable network
cable
modem
splitter
...
cable headend
Channels
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
D
A
T
A
D
A
T
A
C
O
N
T
R
O
L
12 34
56789
frequency division multiplexing:
different channels transmitted
in different frequency bands
1-19

HFC: hybrid fiber coax

asymmetric: up to 30Mbps downstream transmission rate, 2 Mbps upstream transmission rate

network 
of cable, fiber attaches homes to ISP router

homes 
share access network
to cable headend

unlike DSL, which has dedicated access to central office
Access net: cable network
1-20
data, TV transmitted at different 
frequencies over 
shared
cable 
distribution network
cable
modem
splitter
...
cable headend
CMTS
ISP
cable modem
termination system
访问 网络: 电缆 网络
电缆
电缆 headend
信道
频率 划分 多路复用:
不同的 信道 已传输
在 不同的 频率 bands

2024/09/10
5
Access networks and physical media
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Q: How to connect end 
systems to edge router?

residential access nets

institutiona
l access networks (school, 
company)

mobile access networks (WiFi, 4G/5G)
Introduction: 1-17
Access net: digital subscriber line (DSL)
central office
ISP
telephonenetwork
DSLAM
voice, data transmitted
at different frequencies over
dedicated 
line to central office

use 
existing
telephone line to central office DSLAM

data over DSL phone line goes to Internet

voice over DSL phone line goes to telephone net

< 2.5 Mbps upstream transmission rate (typically < 1 Mbps)

< 24 Mbps downstream transmission rate (typically < 10 Mbps)
DSL
modem
splitter
DSL access 
multiplexer
1-18
4KHz
电话
数据
Access net: cable network
cable
modem
splitter
...
cable headend
Channels
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
V
I
D
E
O
D
A
T
A
D
A
T
A
C
O
N
T
R
O
L
12 34
56789
frequency division multiplexing:
different channels transmitted
in different frequency bands
1-19

HFC: hybrid fiber coax

asymmetric: up to 30Mbps downstream transmission rate, 2 Mbps 
upstream transmission rate

network 
of cable, fiber attaches homes to ISP router

homes 
share access network
to cable headend

unlike DSL, which has dedicated access to central office
Access net: cable network
1-20
data, TV transmitted at different 
frequencies over 
shared
cable 
distribution network
cable
modem
splitter
...
cable headend
CMTS
ISP
cable modem
termination system
asymmetric: 上 到 30Mbps downstream 传输 速率, 2 Mbps
上游 传输 速率
网络
的 电缆, fiber attaches homes 到 ISP 路由器
share 访问 网络
到 电缆 headend
unlike DSL, 哪个 有 dedicated 访问 到 central office
访问 网络: 电缆 网络
数据, TV 已传输 在 不同的
frequencies 在
共享
电缆
distribution 网络
电缆
电缆 headend
电缆 modem
termination 系统

2024/09/10
6
Access net: home network
to/from headend or 
central office
cable or DSL modem
router, firewall, NAT
wired Ethernet (100 Mbps)
wireless access 
point (54 Mbps)
wireless
devices
often combined 
in single box
1-21
Enterprise access networks (Ethernet)

typically used in companies, universities, etc

10 Mbps, 100Mbps, 1Gbps, 10G
bps transmission rates

today, end systems typically connect into Ethernet switch
Ethernet 
switch
institutional mail,
web servers
institutional router
institutional link to 
ISP (Internet)
1-22
Wireless access networks

shared 
wireless
access network connects end system to router

via base station aka 
“
access point
”
wireless LANs:

within building (100 ft)

802.11b/g (WiFi): 11, 54 Mbps transmission rate
wide-area wireless access

provided by telco (cellular) 
operator, 10’s km

between 1 and 10 Mbps 

3G, 4G, 5G
to Internet
to Internet
1-23
Host: sends 
packets
of data
host sending function:
takes application message

breaks into smaller chunks, known as 
packets
, of length 
L
bits

transmits packet into access network at 
transmission rate R

link transmission rate, aka link 
capacity, aka link 
bandwidth
R: 
link transmission rate
host
1
2
two packets, L
bits each
packet
transmission
delay
time needed to
transmit 
L
-bit
packet into link
L
(bits)
R
(bits/sec)
=
=
1-24
...
访问 网络: 归属网络
to/来自 headend 或
电缆 或 DSL modem
路由器, firewall, NAT
有线 以太网 (100 Mbps)
无线 访问
点 (54 Mbps)
无线
设备
经常 combined
在 单个 box

2024/09/10
6
Access net: home network
to/from headend or 
central office
cable or DSL modem
router, firewall, NAT
wired Ethernet (100 Mbps)
wireless access 
point (54 Mbps)
wireless
devices
often combined 
in single box
1-21
Enterprise access networks (Ethernet)

typically used in companies, universities, etc

10 Mbps, 100Mbps, 1Gbps, 10G
bps transmission rates

today, end systems typically connect into Ethernet switch
Ethernet 
switch
institutional mail,
web servers
institutional router
institutional link to 
ISP (Internet)
1-22
Wireless access networks

shared 
wireless
access network connects end system to router

via base station aka 
“
access point
”
wireless LANs:

within building (100 ft)

802.11b/g (WiFi): 11, 54 Mbps transmission rate
wide-area wireless access

provided by telco (cellular) operator, 10’s km

between 1 and 10 Mbps 

3G, 4G, 5G
to Internet
to Internet
1-23
Host: sends 
packets
of data
host sending function:

takes application message

breaks into smaller chunks, 
known as 
packets
, of length 
L
bits

transmits packet into access 
network at 
transmission rate R

link transmission rate, aka link 
capacity, aka link 
bandwidth
R: 
link transmission rate
host
1
2
two packets, L
bits each
packet
transmission
delay
time needed to
transmit 
L
-bit
packet into link
L
(bits)
R
(bits/sec)
=
=
1-24
...
Enterprise 接入网络 (以太网)
通常 用于 在 companies, universities, etc
bps 传输 速率
today, 端 系统 通常 连接 进入 以太网 交换机
以太网
交换机
institutional 邮件,
Web 服务器
institutional 路由器
institutional 链路 到

2024/09/10
6
Access net: home network
to/from headend or 
central office
cable or DSL modem
router, firewall, NAT
wired Ethernet (100 Mbps)
wireless access 
point (54 Mbps)
wireless
devices
often combined 
in single box
1-21
Enterprise access networks (Ethernet)

typically used in companies, universities, etc

10 Mbps, 100Mbps, 1Gbps, 10G
bps transmission rates

today, end systems typically connect into Ethernet switch
Ethernet 
switch
institutional mail,
web servers
institutional router
institutional link to 
ISP (Internet)
1-22
Wireless access networks

shared 
wireless
access network connects end system to router

via base station aka 
“
access point
”
wireless LANs:

within building (100 ft)

802.11b/g (WiFi): 11, 54 Mbps transmission rate
wide-area wireless access

provided by telco (cellular) operator, 10’s km

between 1 and 10 Mbps 

3G, 4G, 5G
to Internet
to Internet
1-23
Host: sends 
packets
of data
host sending function:
takes application message

breaks into smaller chunks, known as 
packets
, of length 
L
bits

transmits packet into access network at 
transmission rate R

link transmission rate, aka link 
capacity, aka link 
bandwidth
R: 
link transmission rate
host
1
2
two packets, L
bits each
packet
transmission
delay
time needed to
transmit 
L
-bit
packet into link
L
(bits)
R
(bits/sec)
=
=
1-24
...
无线 接入网络
共享
无线
访问 网络 连接 端 系统 到 路由器
通过 基站 aka
接入点
无线 局域网:
在...内 building (100 ft)Mbps 传输 速率
wide-area 无线 访问
提供 由 telco (蜂窝)之间 1 和 10 Mbps
到 Internet
到 Internet

2024/09/10
6
Access net: home network
to/from headend or 
central office
cable or DSL modem
router, firewall, NAT
wired Ethernet (100 Mbps)
wireless access 
point (54 Mbps)
wireless
devices
often combined 
in single box
1-21
Enterprise access networks (Ethernet)

typically used in companies, universities, etc

10 Mbps, 100Mbps, 1Gbps, 10G
bps transmission rates

today, end systems typically connect into Ethernet switch
Ethernet 
switch
institutional mail,
web servers
institutional router
institutional link to 
ISP (Internet)
1-22
Wireless access networks

shared 
wireless
access network connects end system to router

via base station aka 
“
access point
”
wireless LANs:

within building (100 ft)

802.11b/g (WiFi): 11, 54 Mbps transmission rate
wide-area wireless access

provided by telco (cellular) operator, 10’s km

between 1 and 10 Mbps 

3G, 4G, 5G
to Internet
to Internet
1-23
Host: sends 
packets
of data
host sending function:
takes application message

breaks into smaller chunks, known as 
packets
, of length 
L
bits

transmits packet into access network at 
transmission rate R

link transmission rate, aka 
link 
capacity, aka link 
bandwidth
R: 
link transmission rate
host
1
2
two packets, L
bits each
packet
transmission
delay
time needed to
transmit 
L
-bit
packet into link
L
(bits)
R
(bits/sec)
=
=
1-24
...
主机: 发送
分组
的 数据
主机 发送 功能:
takes 应用 报文breaks 进入 smaller 块,已知 as
分组
, 的 长度
比特
传输 分组 进入 访问网络 在
传输 速率 R
链路 传输 速率, aka
链路
容量, aka 链路
带宽
链路 传输 速率
主机
两个 分组,
比特 每个
分组
传输
时延
时间 needed 到
传输
分组 进入 链路
(比特)

2024/09/10
7
Physical media

bit:
propagates between
transmitter/receiver pairs

physical link:
what lies between 
transmitter & receiver

guided media: 

signals propagate in solid media: copper, fiber, coax

unguided media:

signals propagate freely, e.g., 
radio
twisted pair (TP
)

two insulated copper wires

Category 5: 100 Mbps, 1 Gpbs Ethernet

Category 6: 10Gbps
1-25
Physical media: coax, fiber
coaxial cable:
two concentric copper conductors

bidirectional

broadband:

multiple channels on cable

HFC
fiber optic cable:
glass fiber carrying light pulses, each pulse a bit

high-speed operation:

high-speed point-to-point transmission (e.g., 10
’
s-
100
’
s Gpbs transmission 
rate)

low error rate: 

repeaters spaced far apart 

immune to electromagnetic noise
1-26
Links: physical media
Wireless radio

signal carried in various “bands” in electromagnetic spectrum

no physical “wire”

broadcast, “half‐duplex” 
(sender to receiver)

propagation environment effects:
•
reflection 
•
obstruction by objects
•
Interference/noise
Radio link types:

Wireless LAN
(WiFi)
•
10‐100’s Mbps; 10’s of meters

wide‐area
(e.g., 4G cellular)
•
10’s Mbps over ~10 Km 

Bluetooth: 
cable replacement
•
short distances, limited rates

terrestrial  microwave
•
point‐to‐point; 45 Mbps channels

satellite
•
up to 45 Mbps per channel
•
270 msec end‐end delay
Introduction: 1-27
Chapter 1: Introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 network core

packet switching,  circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-28
物理媒体
比特:
propagates 之间
transmitter/接收方 pairs物理 链路:
什么 lies 之间
transmitter & 接收方
signals propagate 在 solid
两个 insulated copper
Gpbs 以太网

2024/09/10
7
Physical media

bit:
propagates between
transmitter/receiver pairs

physical link:
what lies between 
transmitter & receiver

guided media: 

signals propagate in solid media: copper, fiber, coax

unguided media:

signals propagate freely, e.g., radio
twisted pair (TP
)

two insulated copper wires

Category 5: 100 Mbps, 1 Gpbs Ethernet

Category 6: 10Gbps
1-25
Physical media: coax, fiber
coaxial cable:
two concentric copper conductors

bidirectional

broadband:

multiple channels on cable

HFC
fiber optic cable:
glass fiber carrying light pulses, each pulse a bit

high-speed operation:

high-speed point-to-point transmission (e.g., 10
’
s-
100
’
s Gpbs transmission 
rate)

low error rate: 

repeaters spaced far apart 

immune to electromagnetic noise
1-26
Links: physical media
Wireless radio

signal carried in various “bands” in electromagnetic spectrum

no physical “wire”

broadcast, “half‐duplex” 
(sender to receiver)

propagation environment effects:
•
reflection 
•
obstruction by objects
•
Interference/noise
Radio link types:

Wireless LAN
(WiFi)
•
10‐100’s Mbps; 10’s of meters

wide‐area
(e.g., 4G cellular)
•
10’s Mbps over ~10 Km 

Bluetooth: 
cable replacement
•
short distances, limited rates

terrestrial  microwave
•
point‐to‐point; 45 Mbps channels

satellite
•
up to 45 Mbps per channel
•
270 msec end‐end delay
Introduction: 1-27
Chapter 1: Introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 network core

packet switching,  circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-28
物理媒体: coax, fiber
coaxial 电缆:
两个 concentric copper
多路 信道 在 电缆
fiber optic 电缆:
每个 pulse a 比特
传输 (e.g., 10
s Gpbs 传输
速率)
low 差错 速率:
immune 到 electromagnetic

2024/09/10
7
Physical media

bit:
propagates between
transmitter/receiver pairs

physical link:
what lies between 
transmitter & receiver

guided media: 

signals propagate in solid media: copper, fiber, coax

unguided media:

signals propagate freely, e.g., 
radio
twisted pair (TP
)

two insulated copper wires

Category 5: 100 Mbps, 1 Gpbs Ethernet

Category 6: 10Gbps
1-25
Physical media: coax, fiber
coaxial cable:
two concentric copper conductors

bidirectional

broadband:

multiple channels on cable

HFC
fiber optic cable:
glass fiber carrying light pulses, each pulse a bit

high-speed operation:

high-speed point-to-point transmission (e.g., 10
’
s-
100
’
s Gpbs transmission 
rate)

low error rate: 

repeaters spaced far apart 

immune to electromagnetic noise
1-26
Links: physical media
Wireless radio

signal carried in various 
“bands” in electromagnetic spectrum

no physical “wire”

broadcast, “half‐duplex” 
(sender to receiver)

propagation environment effects:
•
reflection 
•
obstruction by objects
•
Interference/noise
Radio link types:

Wireless LAN
(WiFi)
•
10‐100’s Mbps; 10’s of meters

wide‐area
(e.g., 4G cellular)
•
10’s Mbps over ~10 Km 

Bluetooth: 
cable replacement
•
short distances, limited rates

terrestrial  microwave
•
point‐to‐point; 45 Mbps channels

satellite
•
up to 45 Mbps per channel
•
270 msec end‐end delay
Introduction: 1-27
Chapter 1: Introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 network core

packet switching,  circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-28
链路: 物理媒体
无线 radio
信号 承载的 在 various
“bands” 在 electromagnetic频谱无 物理 “wire”广播, “half‐duplex”
(发送方 到 接收方)
obstruction 由 对象
干扰/noise
Radio 链路 types:
无线 局域网
10‐100’s Mbps; 10’s 的 meters
wide‐区域
(e.g., 4G 蜂窝)
10’s Mbps 在 ~10 Km
电缆 replacement
short distances, limited 速率
点‐到‐点; 45 Mbps 信道
上 到 45 Mbps 每 信道270 msec 端‐端 时延
引言: 1-27

2024/09/10
7
Physical media

bit:
propagates between
transmitter/receiver pairs

physical link:
what lies between 
transmitter & receiver

guided media: 

signals propagate in solid media: copper, fiber, coax

unguided media:

signals propagate freely, e.g., radio
twisted pair (TP
)

two insulated copper wires

Category 5: 100 Mbps, 1 Gpbs Ethernet

Category 6: 10Gbps
1-25
Physical media: coax, fiber
coaxial cable:
two concentric copper conductors

bidirectional

broadband:

multiple channels on cable

HFC
fiber optic cable:
glass fiber carrying light pulses, each pulse a bit

high-speed operation:

high-speed point-to-point transmission (e.g., 10
’
s-
100
’
s Gpbs transmission 
rate)

low error rate: 

repeaters spaced far apart 

immune to electromagnetic noise
1-26
Links: physical media
Wireless radio

signal carried in various “bands” in electromagnetic spectrum

no physical “wire”

broadcast, “half‐duplex” 
(sender to receiver)

propagation environment effects:
•
reflection 
•
obstruction by objects
•
Interference/noise
Radio link types:

Wireless LAN
(WiFi)
•
10‐100’s Mbps; 10’s of meters

wide‐area
(e.g., 4G cellular)
•
10’s Mbps over ~10 Km 

Bluetooth: 
cable replacement
•
short distances, limited rates

terrestrial  microwave
•
point‐to‐point; 45 Mbps channels

satellite
•
up to 45 Mbps per channel
•
270 msec end‐end delay
Introduction: 1-27
Chapter 1: Introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 network core

packet switching,  circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-28
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
1.3 网络核心
分组 交换, 电路 交换, 网络 结构
时延, 丢包, 吞吐量 在 网络协议分层, 服务模型网络 under attack: 安全历史

2024/09/10
8

mesh of interconnected routers

packet-switching: hosts break application-layer messages into 
packets

forward packets
from one router to 
the next
, across links on path from 
source to destination

each packet transmitted 
at full link 
capacity
The network core
1-29
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet-switching: store-and-forward
1-30
Packet-switching: store-and-forward

takes 
L
/
R
seconds to transmit (push 
out) 
L
-bit packet into link at 
R
bps

store and forward:
entire packet must  
arrive at router before it can be 
transmitted on next link
one-hop numerical example:
L
= 7.5 Mbits

R
= 1.5 Mbps

one-hop transmission 
delay = 5 sec
more on delay shortly ...
1-31
source
R
bps
destination
1
2
3
L
bits
per packet
R
bps

end-end delay = 2
L
/
R
(assuming zero 
propagation delay)
Packet Switching: queueing delay, loss
1-32
queuing and loss
: 

If arrival rate (in bits) to link exceeds transmission rate of 
link for a period of 
time:

packets will 
queue
, wait to be transmitted on link 

packets can be 
dropped
(lost) if memory (buffer) fills up
A
B
C
R
= 100 Mb/s
R
= 1.5 Mb/s
D
E
queue of packets
waiting for output link
Queueing
occurs 
when work arrives faster than it can be serviced:
2023.8.30
mesh 的 interconnected 路由器
分组-switching: 主机 break应用-layer 报文 进入
分组
转发 分组
从 一个 路由器 到
下一个
, across 链路 在 路径 从
源 到 目的地
每个 分组 已传输
在 完整的 链路
容量
网络核心
移动网络
归属网络
网络
national 或 全局 ISP
本地 或区域的 ISP
网络
内容提供商网络

2024/09/10
8

mesh of interconnected routers

packet-switching: hosts break application-layer messages into packets

forward packets
from one router to 
the next
, across links on path from 
source to destination

each packet transmitted 
at full link 
capacity
The network core
1-29
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet-switching: store-and-forward
1-30
Packet-switching: store-and-forward

takes 
L
/
R
seconds to transmit (push 
out) 
L
-bit packet into link at 
R
bps

store and forward:
entire packet must  
arrive at router before it can be transmitted on next link
one-hop numerical example:
L
= 7.5 Mbits

R
= 1.5 Mbps

one-hop transmission delay = 5 sec
more on delay shortly ...
1-31
source
R
bps
destination
1
2
3
L
bits
per packet
R
bps

end-end delay = 2
L
/
R
(assuming zero 
propagation delay)
Packet Switching: queueing delay, loss
1-32
queuing and loss
: 

If arrival rate (in bits) to link exceeds transmission rate of 
link for a period of 
time:

packets will 
queue
, wait to be transmitted on link 

packets can be 
dropped
(lost) if memory (buffer) fills up
A
B
C
R
= 100 Mb/s
R
= 1.5 Mb/s
D
E
queue of packets
waiting for output link
Queueing
occurs 
when work arrives faster than it can 
be serviced:
2023.8.30
分组-switching: store-and-forward

2024/09/10
8

mesh of interconnected routers

packet-switching: hosts break application-layer messages into 
packets

forward packets
from one router to 
the next
, across links on path from 
source to destination

each packet transmitted 
at full link 
capacity
The network core
1-29
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet-switching: store-and-forward
1-30
Packet-switching: store-and-forward

takes 
L
/
R
seconds to transmit (push 
out) 
L
-bit packet into link at 
R
bps

store and forward:
entire packet must  
arrive at router before it can be 
transmitted on next link
one-hop numerical example:
L
= 7.5 Mbits

R
= 1.5 Mbps

one-hop transmission delay = 5 sec
more on delay shortly ...
1-31
source
R
bps
destination
1
2
3
L
bits
per packet
R
bps

end-end delay = 2
L
/
R
(assuming zero 
propagation delay)
Packet Switching: queueing delay, loss
1-32
queuing and loss
: 

If arrival rate (in bits) to link exceeds transmission rate of 
link for a period of 
time:

packets will 
queue
, wait to be transmitted on link 

packets can be 
dropped
(lost) if memory (buffer) fills up
A
B
C
R
= 100 Mb/s
R
= 1.5 Mb/s
D
E
queue of packets
waiting for output link
Queueing
occurs 
when work arrives faster than it can be serviced:
2023.8.30
分组-switching: store-and-forward
seconds 到 传输 (push
查看)
-bit 分组 进入 链路 在
store 和 转发:
entire 分组 必须
到达 在 路由器 之前 它 可以 是
已传输 在 下一个 链路
one-hop numerical 示例:
one-hop 传输时延 = 5 sec
更多 在 时延 shortly ...
源
目的地
比特
每 分组
end-end 时延 = 2
propagation 时延)

2024/09/10
8

mesh of interconnected routers

packet-switching: hosts break application-layer messages into packets

forward packets
from one router to 
the next
, across links on path from 
source to destination

each packet transmitted 
at full link 
capacity
The network core
1-29
mobile network
home network
enterprise
network
national or global ISP
local or regional ISP
datacenter network
content provider network
Packet-switching: store-and-forward
1-30
Packet-switching: store-and-forward

takes 
L
/
R
seconds to transmit (push 
out) 
L
-bit packet into link at 
R
bps

store and forward:
entire packet must  
arrive at router before it can be transmitted on next link
one-hop numerical example:
L
= 7.5 Mbits

R
= 1.5 Mbps

one-hop transmission delay = 5 sec
more on delay shortly ...
1-31
source
R
bps
destination
1
2
3
L
bits
per packet
R
bps

end-end delay = 2
L
/
R
(assuming zero 
propagation delay)
Packet Switching: queueing delay, loss
1-32
queuing and loss
: 

If arrival rate (in bits) to link exceeds transmission rate of 
link for a period of 
time:

packets will 
queue
, wait to be transmitted on link 

packets can be 
dropped
(lost) if memory (buffer) fills up
A
B
C
R
= 100 Mb/s
R
= 1.5 Mb/s
D
E
queue of packets
waiting for output link
Queueing
occurs 
when work arrives faster than it can be serviced:
2023.8.30
分组 交换: 排队 时延, 丢包
queuing 和 丢包
如果 arrival 速率 (在 比特) 到 链路 exceeds 传输 速率 的
链路 用于 a period 的
时间:
分组 将
队列
, 等待 到 是 已传输 在 链路
分组 可以 是
(丢失的) 如果 内存 (缓冲区) fills 上
队列 的 分组
waiting 用于 输出 链路
排队当 工作 到达faster 比 它 可以是 serviced:

2024/09/10
9
33
Two key network-core functions
Which way 
to choose?
Which way 
to choose?
routing
forwarding
4-34
Two key network-core functions
forwarding
:
move
packets 
from router’s input to appropriate router 
output
routing
:
determines
source-
destination 
route
taken by packets

routing algorithms
routing algorithm
local forwarding table
header value
output link
01000101
0111
1001
3221
1
2
3
destination address in arrivingPacket’s header
Benefit: Resource sharing
Alternative core: circuit switching
1-35
Benefit: Resource exclusive
Alternative core: circuit switching
end-end resources allocated to, reserved for 
“
call
”
between 
source & dest:
1-36

In diagram, each link has four circuits. 

call gets 
2
nd
circuit in top link and 
1
st
circuit in right link.

dedicated resources: 
no sharing

circuit-like (guaranteed) performance

circuit segment idle if not used by call 
(
no 
sharing
)

Commonly used in traditional telephone networks
Q: How does circuit switching 
support multiple users?
2
nd
circuit 
1
st
circuit 
两个 关键 network-core 功能
哪个 way
到 选择?
哪个 way
到 选择?
路由
转发

2024/09/10
9
33
Two key network-core functions
Which way 
to choose?
Which way 
to choose?
routing
forwarding
4-34
Two key network-core functions
forwarding
:
move
packets 
from router’s input to appropriate router 
output
routing
:
determines
source-
destination 
route
taken by packets

routing algorithms
routing algorithm
local forwarding table
header value
output link
01000101
0111
1001
3
22
1
1
2
3
destination address in arrivingPacket’s header
Benefit: Resource sharing
Alternative core: circuit switching
1-35
Benefit: Resource exclusive
Alternative core: circuit switching
end-end resources allocated to, reserved for 
“
call
”
between 
source & dest:
1-36

In diagram, each link has four circuits. 

call gets 
2
nd
circuit in top link and 
1
st
circuit in right link.

dedicated resources: 
no sharing

circuit-like (guaranteed) performance

circuit segment idle if not used by call 
(
no 
sharing
)

Commonly used in traditional telephone 
networks
Q: How does circuit switching 
support multiple users?
2
nd
circuit 
1
st
circuit 
两个 关键 network-core 功能
转发
移动
分组
从 路由器’s 输入 到 合适的路由器
输出
路由
目的地
路由
经过 由 分组
路由 算法
路由 算法
本地 转发表
首部 值
输出 链路
目的地 地址 在 到达的分组’s 首部

2024/09/10
9
33
Two key network-core functions
Which way 
to choose?
Which way 
to choose?
routing
forwarding
4-34
Two key network-core functions
forwarding
:
move
packets 
from router’s input to appropriate router 
output
routing
:
determines
source-
destination 
route
taken by packets

routing algorithms
routing algorithm
local forwarding table
header value
output link
01000101
0111
1001
3221
1
2
3
destination address in arrivingPacket’s header
Benefit: Resource sharing
Alternative core: circuit switching
1-35
Benefit: Resource exclusive
Alternative core: circuit switching
end-end resources allocated to, reserved for 
“
call
”
between 
source & dest:
1-36

In diagram, each link has four circuits. 

call gets 
2
nd
circuit in top link and 
1
st
circuit in right link.

dedicated resources: 
no sharing

circuit-like (guaranteed) performance

circuit segment idle if not used by call 
(
no 
sharing
)

Commonly used in traditional telephone networks
Q: How does circuit switching 
support multiple users?
2
nd
circuit 
1
st
circuit 
Alternative 核心: 电路 交换

2024/09/10
9
33
Two key network-core functions
Which way 
to choose?
Which way 
to choose?
routing
forwarding
4-34
Two key network-core functions
forwarding
:
move
packets 
from router’s input to appropriate 
router 
output
routing
:
determines
source-
destination 
route
taken by packets

routing algorithms
routing algorithm
local forwarding table
header value
output link
01000101
0111
1001
3
221
1
2
3
destination address in arrivingPacket’s header
Benefit: Resource sharing
Alternative core: circuit switching
1-35
Benefit: Resource exclusive
Alternative core: circuit switching
end-end resources allocated to, reserved for 
“
call
”
between 
source & dest:
1-36

In diagram, each link has four circuits. 

call gets 
2
nd
circuit in top link and 
1
st
circuit in right link.

dedicated resources: 
no sharing

circuit-like (guaranteed) performance

circuit segment idle if not used by call 
(
no 
sharing
)

Commonly used in traditional telephone 
networks
Q: How does circuit switching 
support multiple users?
2
nd
circuit 
1
st
circuit 
Alternative 核心: 电路 交换
end-end resources allocated 到, reserved 用于
调用
之间
源 & dest:
在 diagram, 每个 链路 有 four circuits.
调用 获得
电路 在 top 链路 和
电路 在 right link.
无 sharing
circuit-like (guaranteed) 性能
电路 报文段 idle 如果 不 用于 由 调用
无
Commonly 用于 在 传统的 telephone
网络
Q: 如何 做 电路 交换
支持 多路 用户?
电路
电路

2024/09/10
10
Circuit switching: FDM versus TDM
FDM
frequency
time
TDM
frequency
time
4 users
Example:
1-37
帧
1 2 3 4
帧
1 2 3 4
How to allocate channels and circuits
38
Packet switching versus circuit switching
circuit switchingPacket switching 
Circuit switching: send data after the path is determined
Packet switching: send data while routing
Packet switching versus circuit switching
example:
1 Mb/s link

each user: 
•
100 kb/s when 
“
active
”
•
active 10% of time

circuit-switching:

10 users  10*100kb/s=1Mb

packet switching:

with 35 users, probability > 10 
active at same time is less than .0004 *
packet switching allows more users to use network!
N
users
1 Mbps link
Q:
how did we get value 0.0004?
Q:
what happens if  > 35 users ?
1-39
* Check out the online interactive exercises for more examples
(binomial distribution)
() (1)
kknk
n
PXkCpp

   

great for bursty data

resource sharing

simpler, no call setup

excessive congestion possible:
packet delay and loss

protocols needed for reliable data transfer, congestion control

Q:
How to provide circuit-like behavior?

bandwidth guarantees needed for audio/video apps

still an unsolved problem
is packet switching a 
“
slam dunk winner?
”
Q:
human analogies of reserved resources (circuit switching) versu
s 
on-demand allocation (packet-switching)?
Packet switching versus circuit switching
1-40
电路 交换: FDM 与 TDM
频率
时间
频率
时间
4 用户
示例:
如何 到 allocate 信道 和 circuits

2024/09/10
10
Circuit switching: FDM versus TDM
FDM
frequency
time
TDM
frequency
time
4 users
Example:
1-37
帧
1 2 3 4
帧
1 2 3 4
How to allocate channels and circuits
38
Packet switching versus circuit switching
circuit switchingPacket switching 
Circuit switching: send data after the path is determined
Packet switching: send data while routing
Packet switching versus circuit switching
example:
1 Mb/s link

each user: 
•
100 kb/s when 
“
active
”
•
active 10% of time

circuit-switching:

10 users  10*100kb/s=1Mb

packet switching:

with 35 users, probability > 10 active at same time is less than .0004 *
packet switching allows more users to use network!
N
users
1 Mbps link
Q:
how did we get value 0.0004?
Q:
what happens if  > 35 users ?
1-39
* Check out the online interactive exercises for more examples
(binomial distribution)
() (1)
kknk
n
PXkCpp

   

great for bursty data

resource sharing

simpler, no call setup

excessive congestion possible:
packet delay and loss

protocols needed for reliable data transfer, 
congestion control

Q:
How to provide circuit-like behavior?

bandwidth guarantees needed for audio/video apps

still an unsolved problem
is packet switching a 
“
slam dunk winner?
”
Q:
human analogies of reserved resources (circuit switching) versu
s 
on-demand allocation (packet-switching)?
Packet switching versus circuit switching
1-40
分组 交换 与 电路 交换
电路 交换分组 交换
电路 交换: 发送 数据 之后 路径 是 determined
分组 交换: 发送 数据 while 路由

2024/09/10
10
Circuit switching: FDM versus TDM
FDM
frequency
time
TDM
frequency
time
4 users
Example:
1-37
帧
1 2 3 4
帧
1 2 3 4
How to allocate channels and circuits
38
Packet switching versus circuit switching
circuit switchingPacket switching 
Circuit switching: send data after the path is determined
Packet switching: send data while routing
Packet switching versus circuit switching
example:

1 Mb/s link

each user: 
•
100 kb/s when 
“
active
”
•
active 10% of time

circuit-switching:

10 users  10*100kb/s=1Mb

packet switching:

with 35 users, probability > 10 
active at same time is less than .0004 *
packet switching allows more users to use network!
N
users
1 Mbps link
Q:
how did we get value 0.0004?
Q:
what happens if  > 35 users ?
1-39
* Check out the online interactive exercises for more examples
(binomial distribution)
() (1)
kknk
n
PXkCpp

   

great for bursty data

resource sharing

simpler, no call setup

excessive congestion possible:
packet delay and loss

protocols needed for reliable data transfer, congestion control

Q:
How to provide circuit-like behavior?

bandwidth guarantees needed for audio/video apps

still an unsolved problem
is packet switching a 
“
slam dunk winner?
”
Q:
human analogies of reserved resources (circuit switching) versu
s 
on-demand allocation (packet-switching)?
Packet switching versus circuit switching
1-40
分组 交换 与 电路 交换
示例:
1 Mb/s 链路
每个 用户:
100 kb/s 当
主动
主动 10% 的 时间10 用户
分组 交换:
与 35 用户, probability > 10
主动 在 相同 时间 是 less比 .0004 *
分组 交换 允许 更多 用户 到 使用 网络!
用户
1 Mbps 链路
如何 did 我们 获得 值 0.0004?什么 happens 如果 > 35 用户 ?
* 更多示例见在线交互练习

2024/09/10
10
Circuit switching: FDM versus TDM
FDM
frequency
time
TDM
frequency
time
4 users
Example:
1-37
帧
1 2 3 4
帧
1 2 3 4
How to allocate channels and circuits
38
Packet switching versus circuit switching
circuit switchingPacket switching 
Circuit switching: send data after the path is determined
Packet switching: send data while routing
Packet switching versus circuit switching
example:
1 Mb/s link

each user: 
•
100 kb/s when 
“
active
”
•
active 10% of time

circuit-switching:

10 users  10*100kb/s=1Mb

packet switching:

with 35 users, probability > 10 active at same time is less than .0004 *
packet switching allows more users to use network!
N
users
1 Mbps link
Q:
how did we get value 0.0004?
Q:
what happens if  > 35 users ?
1-39
* Check out the online interactive exercises for more examples
(binomial distribution)
() (1)
kknk
n
PXkCpp

   

great for bursty data

resource sharing

simpler, no call setup

excessive congestion possible:
packet delay and loss

protocols needed for reliable data transfer, congestion control

Q:
How to provide circuit-like behavior?

bandwidth guarantees needed for audio/video apps

still an unsolved problem
is packet switching a 
“
slam dunk winner?
”
Q:
human analogies of reserved resources (circuit switching) versu
s 
on-demand allocation (packet-switching)?
Packet switching versus circuit switching
1-40
great 用于 bursty 数据
simpler, 无 调用 setup
excessive 拥塞 可能:
分组 时延 和 丢包
协议 needed 用于 可靠数据传输,拥塞控制
如何 到 提供 circuit-like behavior?
带宽 guarantees needed 用于 audio/video 应用仍然 unsolved 问题
是 分组 交换 a
human analogies 的 reserved resources (电路 交换) versu
on-demand allocation (分组-switching)?
分组 交换 与 电路 交换

2024/09/10
11
Internet structure: network of networks

End systems 
connect to Internet via 
access ISPs 
(Internet Service 
Providers)

Residential, company and university ISPs

Access ISPs 
in turn must be interconnected. 

So that any two hosts can 
send packets to each other

Resulting network of 
networks 
is very complex

Evolution was driven by 
economics
and 
national policies

Let’s take a stepwise approach to
 describe current Internet str
ucture
41
Internet structure: network of networks
Question: 
given 
millions
of access ISPs, how to 
connect them together?
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
42
Internet structure: network of networks
Option: 
connect each access ISP to every other access 
ISP? 
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
connecting each access ISP to each 
other directly 
doesn’t scale: 
O(
N
2
) 
connections.
43
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
Option: 
connect each access ISP to a global transit ISP? 
Customer
and 
provider 
ISPs have economic agreement.
globalISP
44
Internet 结构: 网络 的 网络
端 系统
连接 到 Internet 通过
访问 ISPs
(Internet 服务
Residential, company 和 university ISPs
访问 ISPs
在 turn 必须 是 interconnected.
因此 这 任何 两个 主机 可以
发送 分组 到 每个 其他
得到的 网络 的
网络
是 very complex
Evolution was driven 由
和
Let’s take a stepwise 方法 到
describe 当前 Internet str

2024/09/10
11
Internet structure: network of networks

End systems 
connect to Internet via 
access ISPs 
(Internet Service 
Providers)

Residential, company and university ISPs

Access ISPs 
in turn must be interconnected. 

So that any two hosts can 
send packets to each other

Resulting network of 
networks 
is very complex

Evolution was driven by 
economics
and 
national policies

Let’s take a stepwise approach to
 describe current Internet str
ucture
41
Internet structure: network of networks
Question: 
given 
millions
of access ISPs, how to 
connect them together?
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
42
Internet structure: network of networks
Option: 
connect each access ISP to every other access 
ISP? 
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
connecting each access ISP to each 
other directly 
doesn’t scale: 
O(
N
2
) 
connections.
43
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
Option: 
connect each access ISP to a global transit ISP? 
Customer
and 
provider 
ISPs have economic agreement.
globalISP
44
Internet 结构: 网络 的 网络
问题:
给定的
的 访问 ISPs, 如何 到
连接 them together?
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络

2024/09/10
11
Internet structure: network of networks

End systems 
connect to Internet via 
access ISPs 
(Internet Service 
Providers)

Residential, company and university ISPs

Access ISPs 
in turn must be interconnected. 

So that any two hosts can 
send packets to each other

Resulting network of 
networks 
is very complex

Evolution was driven by 
economics
and 
national policies

Let’s take a stepwise approach to
 describe current Internet str
ucture
41
Internet structure: network of networks
Question: 
given 
millions
of access ISPs, how to 
connect them together?
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
42
Internet structure: network of networks
Option: 
connect each access ISP to every other access 
ISP? 
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
connecting each access ISP to each 
other directly 
doesn’t scale: 
O(
N
2
) 
connections.
43
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
Option: 
connect each access ISP to a global transit ISP? 
Customer
and 
provider 
ISPs have economic agreement.
globalISP
44
Internet 结构: 网络 的 网络
选项:
连接 每个 访问 ISP 到 每一个 其他 访问
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
connecting 每个 访问 ISP 到 每个
其他 directly

2024/09/10
11
Internet structure: network of networks

End systems 
connect to Internet via 
access ISPs 
(Internet Service 
Providers)

Residential, company and university ISPs

Access ISPs 
in turn must be interconnected. 

So that any two hosts can 
send packets to each other

Resulting network of 
networks 
is very complex

Evolution was driven by 
economics
and 
national policies

Let’s take a stepwise approach to
 describe current Internet str
ucture
41
Internet structure: network of networks
Question: 
given 
millions
of access ISPs, how to 
connect them together?
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
42
Internet structure: network of networks
Option: 
connect each access ISP to every other access 
ISP? 
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
connecting each access ISP to each 
other directly 
doesn’t scale: 
O(
N
2
) 
connections.
43
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
Option: 
connect each access ISP to a global transit ISP? 
Customer
and 
provider 
ISPs have economic agreement.
globalISP
44
Internet 结构: 网络 的 网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
选项:
连接 每个 访问 ISP 到 a 全局 transit ISP?
和
提供商
ISPs 有 economic agreement.
全局

2024/09/10
12
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....
ISP B
ISP A
ISP C
45
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....  which 
must be interconnected
ISP B
ISP A
ISP C
IXP
IXP
peering link
Internet exchange point
46
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and regional networks may arise to connect access nets to ISP
S 
ISP B
ISP A
ISP C
IXP
IXP
regional net
47
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and content provider networks  (e.g., Google, Microsoft,   Ak
amai ) may run 
their own network, to bring services, content close to end user
s
ISP B
ISP A
ISP B
IXP
IXP
regional net
Content provider network
48
Internet 结构: 网络 的 网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
但是 如果 一个 全局 ISP 是 viable business, 存在 将 是 competi

2024/09/10
12
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....
ISP B
ISP A
ISP C
45
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....  which 
must be interconnected
ISP B
ISP A
ISP C
IXP
IXP
peering link
Internet exchange point
46
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and regional networks may arise to connect access nets to ISP
S 
ISP B
ISP A
ISP C
IXP
IXP
regional net
47
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and content provider networks  (e.g., Google, Microsoft,   Ak
amai ) may run 
their own network, to bring services, content close to end user
s
ISP B
ISP A
ISP B
IXP
IXP
regional net
Content provider network
48
Internet 结构: 网络 的 网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
但是 如果 一个 全局 ISP 是 viable business, 存在 将 是 competi
tors .... 哪个
必须 是 interconnected
peering 链路
Internet 交换 点

2024/09/10
12
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....
ISP B
ISP A
ISP C
45
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....  which 
must be interconnected
ISP B
ISP A
ISP C
IXP
IXP
peering link
Internet exchange point
46
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and regional networks may arise to connect access nets to ISP
S 
ISP B
ISP A
ISP C
IXP
IXP
regional net
47
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and content provider networks  (e.g., Google, Microsoft,   Ak
amai ) may run 
their own network, to bring services, content close to end user
s
ISP B
ISP A
ISP B
IXP
IXP
regional net
Content provider network
48
Internet 结构: 网络 的 网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
... 和 区域的 网络 可能 arise 到 连接 访问 nets 到 ISP
区域的 网络

2024/09/10
12
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....
ISP B
ISP A
ISP C
45
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
But if one global ISP is viable business, there will be competi
tors ....  which 
must be interconnected
ISP B
ISP A
ISP C
IXP
IXP
peering link
Internet exchange point
46
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and regional networks may arise to connect access nets to ISP
S 
ISP B
ISP A
ISP C
IXP
IXP
regional net
47
Internet structure: network of networks
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
access
net
... and content provider networks  (e.g., Google, Microsoft,   Ak
amai ) may run 
their own network, to bring services, content close to end user
s
ISP B
ISP A
ISP B
IXP
IXP
regional net
Content provider network
48
Internet 结构: 网络 的 网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
访问
网络
... 和 内容 提供商 网络 (e.g., Google, Microsoft,
amai ) 可能 运行
它们的 自己的 网络, 到 bring 服务, 内容 关闭 到 端 用户
区域的 网络
内容 提供商 网络

2024/09/10
13
Internet structure: network of networks

at center: small # of 
well-connected 
large networks

“
tier-1
”
commercial 
ISPs
(e.g., Level 3, 
Sprint, AT&T, NTT), 
national & 
international 
coverage

content provider 
network 
(e.g, Google): 
private network that connects it data centers to Internet, often bypassing tier-1, regional ISPs
1-49
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
Regional ISP
Regional ISP
IXP
IXP
Tier 1 ISP
Tier 1 ISP
Google
IXP
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 delay, loss, throughput in networks1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-51
How do loss and delay occur?
packets 
queue
in router buffers

packet arrival rate to link (temporarily) exceeds output link c
apacity

packets queue, wait for turn
A
B
packet being transmitted 
(delay)
packets queueing
(delay)
free (available) buffers: arriving packets dropped (
loss
) if no free buffers
1-52
Four sources of packet delay
d
proc
: nodal processing

check bit errors

determine output link

typically < msec
A
B
propagation
transmission
nodal
processing
queueing
d
queue
: queueing delay

time waiting at output link for transmission 

depends on congestion level of router
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-53
Internet 结构: 网络 的 网络
在 中心: small # 的
大的 网络
内容 提供商
网络私有 网络 这连接 它 数据centers 到 Internet,经常 bypassing tier-1,区域的 ISPs
访问
访问
访问
访问
访问
访问
访问
访问
区域的 ISP
区域的 ISP

2024/09/10
13
Internet structure: network of networks

at center: small # of well-connected large networks

“
tier-1
”
commercial 
ISPs
(e.g., Level 3, 
Sprint, AT&T, NTT), national & international coverage

content provider network 
(e.g, Google): 
private network that connects it data centers to Internet, often bypassing tier-1, regional ISPs
1-49
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
Regional ISP
Regional ISP
IXP
IXP
Tier 1 ISP
Tier 1 ISP
Google
IXP
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 delay, loss, throughput in networks1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-51
How do loss and delay occur?
packets 
queue
in router buffers

packet arrival rate to link (temporarily) exceeds output link c
apacity

packets queue, wait for turn
A
B
packet being transmitted 
(delay)
packets queueing
(delay)
free (available) buffers: arriving packets dropped (
loss
) if no free buffers
1-52
Four sources of packet delay
d
proc
: nodal processing

check bit errors

determine output link

typically < msec
A
B
propagation
transmission
nodal
processing
queueing
d
queue
: queueing delay

time waiting at output link for 
transmission 

depends on congestion level of router
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-53
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络 结构
1.4 时延, 丢包, 吞吐量 在 网络
协议分层, 服务模型网络 under attack: 安全
历史

2024/09/10
13
Internet structure: network of networks

at center: small # of 
well-connected large networks

“
tier-1
”
commercial 
ISPs
(e.g., Level 3, 
Sprint, AT&T, NTT), national & international coverage

content provider network 
(e.g, Google): 
private network that connects it data centers to Internet, often bypassing tier-1, regional ISPs
1-49
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
Regional ISP
Regional ISP
IXP
IXP
Tier 1 ISP
Tier 1 ISP
Google
IXP
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 delay, loss, throughput in networks1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-51
How do loss and delay occur?
packets 
queue
in router buffers

packet arrival rate to link (temporarily) exceeds output link c
apacity

packets queue, wait for turn
A
B
packet being transmitted 
(delay)
packets queueing
(delay)
free (available) buffers: arriving packets 
dropped (
loss
) if no free buffers
1-52
Four sources of packet delay
d
proc
: nodal processing

check bit errors

determine output link

typically < msec
A
B
propagation
transmission
nodal
processing
queueing
d
queue
: queueing delay

time waiting at output link for transmission 

depends on congestion level of router
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-53
如何 做 丢包 和 时延 occur?
分组
队列
在 路由器 缓冲区
分组 arrival 速率 到 链路 (temporarily) exceeds 输出 链路 c
分组 队列, 等待 用于 turn
分组 being 已传输
(时延)
分组 排队
(时延)
free (可用的) 缓冲区: 到达的 分组
丢包
) 如果 无 free 缓冲区

2024/09/10
13
Internet structure: network of networks

at center: small # of well-connected large networks

“
tier-1
”
commercial 
ISPs
(e.g., Level 3, 
Sprint, AT&T, NTT), national & international coverage

content provider network 
(e.g, Google): 
private network that connects it data centers to Internet, often bypassing tier-1, regional ISPs
1-49
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
access
ISP
Regional ISP
Regional ISP
IXP
IXP
Tier 1 ISP
Tier 1 ISP
Google
IXP
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 delay, loss, throughput in networks1.5
protocol layers, service models
1.6
networks under attack: security
1.7
history
1-51
How do loss and delay occur?
packets 
queue
in router buffers

packet arrival rate to link (temporarily) exceeds output link c
apacity

packets queue, wait for turn
A
B
packet being transmitted 
(delay)
packets queueing
(delay)
free (available) buffers: arriving packets dropped (
loss
) if no free buffers
1-52
Four sources of packet delay
d
proc
: nodal processing

check bit errors

determine output link

typically < msec
A
B
propagation
transmission
nodal
processing
queueing
d
queue
: queueing delay

time waiting at output link for transmission 

depends on congestion level of router
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-53
Four sources 的 分组 时延
查看 比特 差错确定 输出 链路
通常 < msec
传输
排队
队列
: 排队 时延
时间 waiting 在 输出 链路 用于传输depends 在 拥塞 level 的 路由器
队列

2024/09/10
14
d
trans
: transmission delay:

L
: packet length (bits) 

R
: link 
bandwidth (bps)

d
trans
= L/R
d
prop
: propagation delay:

d
: length of physical link

s
: propagation speed in medium 
(~2x10
8
m/sec)

d
prop
= 
d
/
s
d
trans
and 
d
prop
Very Very different
Four sources of packet delay
propagation
nodal
processing
queueing
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-54
A
B
transmission
* Check out the Java applet for an interactive animation on tra
ns vs. prop delay
Caravan analogy

cars 
“
propagate
”
at 
100 km/hr

toll booth takes 12 sec to service car (
bit transmission time
)

car~bit; caravan ~ packet

Q:
How long until caravan is lined 
up before 2nd toll booth?

time to 
“
push
”
entire 
caravan through toll booth onto highway = 12*10 = 120 sec

time for 
last car 
to 
propagate from 1st to 2nd toll both: 100km/(100km/hr)= 1 hr

A:
about 62 minutes
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-55
相当于网卡，传输时延
:12s
传播时延：
1hr
传播时延
>>
传输时延
Caravan analogy (more)

suppose cars now 
“
propagate
”
at 
1000 km/hr

and suppose toll booth now takes 
one min 
to service a car

Q:
Will cars arrive to 2nd booth before all cars serviced at 
first booth?

A: Yes!
after 
7
min, 1st car arrives at second booth; 
three cars 
still at 1st booth.
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-56
传播时延：
6m
传输时延
:1m

R:
link bandwidth (bps)

L:
packet length (bits)

a: 
average
packet arrival rate
traffic intensity 
= La/R

La/R
~ 0: avg. queuing delay small

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, av
erage delay infinite!
average  queueing 
delay
La/R ~ 0
Queuing delay (revisited)
La/R -> 1
1-57
* Check out the Java applet for an interactive animation on que
uing and loss
: 传输 时延:
: 分组 长度 (比特)
: 链路
带宽 (bps)
: propagation 时延:
: 长度 的 物理 链路
: propagation speed 在 介质
和
Very Very 不同的
Four sources 的 分组 时延
排队
队列
传输
* 查看 查看 Java applet 用于 交互式 animation 在 tra
ns vs. prop 时延

2024/09/10
14
d
trans
: transmission delay:

L
: packet length (bits) 

R
: link 
bandwidth (bps)

d
trans
= L/R
d
prop
: propagation delay:

d
: length of physical link

s
: propagation speed in medium 
(~2x10
8
m/sec)

d
prop
= 
d
/
s
d
trans
and 
d
prop
Very Very different
Four sources of packet delay
propagation
nodal
processing
queueing
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-54
A
B
transmission
* Check out the Java applet for an interactive animation on tra
ns vs. prop delay
Caravan analogy

cars 
“
propagate
”
at 
100 km/hr

toll booth takes 12 sec to service 
car (
bit transmission time
)

car~bit; caravan ~ packet

Q:
How long until caravan is lined 
up before 2nd toll booth?

time to 
“
push
”
entire 
caravan through toll booth onto highway = 12*10 = 120 sec

time for 
last car 
to 
propagate from 1st to 2nd toll both: 100km/(100km/hr)= 1 hr

A:
about 62 minutes
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-55
相当于网卡，传输时延
:12s
传播时延：
1hr
传播时延
>>
传输时延
Caravan analogy (more)

suppose cars now 
“
propagate
”
at 
1000 km/hr

and suppose toll booth now takes 
one min 
to service a car

Q:
Will cars arrive to 2nd booth before all cars serviced at 
first booth?

A: Yes!
after 
7
min, 1st car arrives at second booth; 
three cars 
still at 1st booth.
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-56
传播时延：
6m
传输时延
:1m

R:
link bandwidth (bps)

L:
packet length (bits)

a: 
average
packet arrival rate
traffic intensity 
= La/R

La/R
~ 0: avg. queuing delay small

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, av
erage delay infinite!
average  queueing 
delay
La/R ~ 0
Queuing delay (revisited)
La/R -> 1
1-57
* Check out the Java applet for an interactive animation on que
uing and loss
在
toll booth takes 12 sec 到 服务
比特 传输 时间
car~比特; caravan ~ 分组
如何 long 直到 caravan 是 lined
上 之前 2nd toll booth?
时间 到caravan 通过 toll时间 用于
最后 car
到
propagate 从 1st 到2nd toll 二者:
关于 62 minutes

2024/09/10
14
d
trans
: transmission delay:

L
: packet length (bits) 

R
: link 
bandwidth (bps)

d
trans
= L/R
d
prop
: propagation delay:

d
: length of physical link

s
: propagation speed in medium 
(~2x10
8
m/sec)

d
prop
= 
d
/
s
d
trans
and 
d
prop
Very Very different
Four sources of packet delay
propagation
nodal
processing
queueing
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-54
A
B
transmission
* Check out the Java applet for an interactive animation on tra
ns vs. prop delay
Caravan analogy

cars 
“
propagate
”
at 
100 km/hr

toll booth takes 12 sec to service car (
bit transmission time
)

car~bit; caravan ~ packet

Q:
How long until caravan is lined 
up before 2nd toll booth?

time to 
“
push
”
entire 
caravan through toll booth onto highway = 12*10 = 120 sec

time for 
last car 
to 
propagate from 1st to 2nd toll both: 100km/(100km/hr)= 1 hr

A:
about 62 minutes
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-55
相当于网卡，传输时延
:12s
传播时延：
1hr
传播时延
>>
传输时延
Caravan analogy (more)

suppose cars now 
“
propagate
”
at 
1000 km/hr

and suppose toll booth now takes 
one min 
to service a car

Q:
Will cars arrive to 2nd booth before all cars serviced at 
first booth?

A: Yes!
after 
7
min, 1st car arrives at second booth; 
three cars 
still at 1st booth.
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-56
传播时延：
6m
传输时延
:1m

R:
link bandwidth (bps)

L:
packet length (bits)

a: 
average
packet arrival rate
traffic intensity 
= La/R

La/R
~ 0: avg. queuing delay small

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, av
erage delay infinite!
average  queueing 
delay
La/R ~ 0
Queuing delay (revisited)
La/R -> 1
1-57
* Check out the Java applet for an interactive animation on que
uing and loss
Caravan analogy (更多)
suppose cars 现在
在
和 suppose toll booth 现在 takes
一个 最小值
到 服务 a car
将 cars 到达 到 2nd booth 之前 所有 cars serviced 在
第一个 booth?
: 是!
之后
最小值, 1st car 到达 在 second booth;
仍然 在 1st booth.

2024/09/10
14
d
trans
: transmission delay:

L
: packet length (bits) 

R
: link 
bandwidth (bps)

d
trans
= L/R
d
prop
: propagation delay:

d
: length of physical link

s
: propagation speed in medium 
(~2x10
8
m/sec)

d
prop
= 
d
/
s
d
trans
and 
d
prop
Very Very different
Four sources of packet delay
propagation
nodal
processing
queueing
d
nodal
= 
d
proc
+ 
d
queue
+ 
d
trans
+  
d
prop
1-54
A
B
transmission
* Check out the Java applet for an interactive animation on tra
ns vs. prop delay
Caravan analogy

cars 
“
propagate
”
at 
100 km/hr

toll booth takes 12 sec to service 
car (
bit transmission time
)

car~bit; caravan ~ packet

Q:
How long until caravan is lined 
up before 2nd toll booth?

time to 
“
push
”
entire 
caravan through toll booth onto highway = 12*10 = 120 sec

time for 
last car 
to 
propagate from 1st to 2nd toll both: 100km/(100km/hr)= 1 hr

A:
about 62 minutes
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-55
相当于网卡，传输时延
:12s
传播时延：
1hr
传播时延
>>
传输时延
Caravan analogy (more)

suppose cars now 
“
propagate
”
at 
1000 km/hr

and suppose toll booth now takes 
one min 
to service a car

Q:
Will cars arrive to 2nd booth before all cars serviced at 
first booth?

A: Yes!
after 
7
min, 1st car arrives at second booth; 
three cars 
still at 1st booth.
toll 
booth
toll 
booth
ten-car caravan
100 km
100 km
1-56
传播时延：
6m
传输时延
:1m

R:
link bandwidth (bps)

L:
packet length (bits)

a: 
average
packet arrival rate
traffic intensity 
= La/R

La/R
~ 0: avg. queuing delay small

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, av
erage delay infinite!
average  queueing 
delay
La/R ~ 0
Queuing delay (revisited)
La/R -> 1
1-57
* Check out the Java applet for an interactive animation on que
uing and loss
链路 带宽 (bps)
分组 长度 (比特)
分组 arrival 速率
流量 intensity
~ 0: avg. queuing 时延 small
1: avg. queuing 时延 大的
> 1: 更多
工作
到达的
比 可以 是 serviced, av
erage 时延 infinite!
average 排队
时延
Queuing 时延 (revisited)
* 查看 查看 Java applet 用于 交互式 animation 在 que
uing 和 丢包

2024/09/10
15
58

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, average delay infinite!
Why?
1
23 45 67
8
恒定速率
λ
到达，
LaR=1
，满负荷，没有排队
1
23 45 67
8
平均速率依然为
λ
，但是
{2
，
4
，
6
，
8}
时刻
出现排队，各有
1
次排队
1
23 45 67
8
平均速率依然为
λ
，但是
{4
，
8}
时刻出现排
队，各有
3
次排队
原因：分组到达不是恒定速率，具有随机突发性！
理论分析参阅排队论
“
Real
”
Internet delays and routes 
（
DIY
）

what do 
“
real
”
Internet delay & loss look like? 

Traceroute(win: 
tracert
) 
program: provides delay 
measurement from source to router along end-end Internet path towards destination.  For all 
i:

sends 
three
UDP packets that will reach router 
i
on path towards 
destination

router 
i
will return packets to sender

sender times interval betw
een transmission and reply.
3 probes
3 probes
3 probes
1-59
“
Real
”
Internet delays, routes
1  cs-gw (128.119.240.254)  1 ms  1 ms  2 ms2  border1-rt-fa5-1-0.gw.umass.edu (128.119.3.145)  1 ms  1 ms 
 2 ms
3  cht-vbns.gw.umass.edu (128.119.3.130)  6 ms 5 ms 5 ms
4  jn1-at1-0-0-19.wor.vbns.
net (204.147.132.129)  16 ms 11 ms 1
3 ms 
5  jn1-so7-0-0-0.wae.vbns.net
 (204.147.136.136)  21 ms 18 ms 18
 ms 
6  abilene-vbns.abilene.ucaid.edu (198.32.11.9)  22 ms  18 ms  
22 ms
7  nycm-wash.abilene.ucaid.edu (198.32.8.46)  22 ms  22 ms  22 
ms
8  62.40.103.253 (62.40.103.253)  104 ms 109 ms 106 ms
9  de2-1.de1.de.geant.net (62.40.96.129)  109 ms 102 ms 104 ms10  de.fr1.fr.geant.net (62.40.96.50)  113 ms 121 ms 114 ms
11  renater-gw.fr1.fr.geant.net (62.40.103.54)  112 ms  114 ms 
 112 ms
12  nio-n2.cssi.renater.fr (193.51.206.13)  111 ms  114 ms  116
 ms
13  nice.cssi.renater.fr (195.220.98.102)  123 ms  125 ms  124 
ms
14  r3t2-nice.cssi.renater.fr (195.220.98.110)  126 ms  126 ms 
 124 ms
15  eurecom-valbonne.r3t2.ft.net (193.48.50.54)  135 ms  128 ms
  133 ms
16  194.214.211.25 (194.214.211.25)  126 ms  128 ms  126 ms17  * * *18  * * *19  fantasia.eurecom.fr (193.55.113.142)  132 ms  128 ms  136
ms
traceroute:
gaia.cs.umass.edu to www.eurecom.fr
3 delay measurements from gaia.cs.umass.edu to cs-gw.cs.umass.edu 
* means no response (probe lost, router not replying)
trans-oceaniclink
1-60
* Do some traceroutes from exotic countries at www.traceroute.or
g
Packet loss

queue (aka buffer) preceding link in buffer has finite capacity

packet arriving to full queue dropped (aka lost)

lost packet 
may be retransmitted 
by previous node, by 
source end system, 
or not at all
A
B
packet being transmitted
packet arriving tofull buffer is 
lost
buffer 
(waiting area)
1-61
* Check out the Java applet for an interactive animation on que
uing and loss
1: avg. queuing 时延 大的
> 1: 更多
工作
到达的
比 可以 是 serviced, average 时延 infinite!

2024/09/10
15
58

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, average delay infinite!
Why?
1
23 45 67
8
恒定速率
λ
到达，
LaR=1
，满负荷，没有排队
1
23 45 67
8
平均速率依然为
λ
，但是
{2
，
4
，
6
，
8}
时刻
出现排队，各有
1
次排队
1
23 45 67
8
平均速率依然为
λ
，但是
{4
，
8}
时刻出现排
队，各有
3
次排队
原因：分组到达不是恒定速率，具有随机突发性！
理论分析参阅排队论
“
Real
”
Internet delays and routes 
（
DIY
）

what do 
“
real
”
Internet delay & loss look like? 

Traceroute(win: 
tracert
) 
program: provides delay 
measurement from source to router along end-end 
Internet path towards destination.  For all 
i:

sends 
three
UDP packets that will reach router 
i
on path towards 
destination

router 
i
will return packets to sender

sender times interval betw
een transmission and reply.
3 probes
3 probes
3 probes
1-59
“
Real
”
Internet delays, routes
1  cs-gw (128.119.240.254)  1 ms  1 ms  2 ms2  border1-rt-fa5-1-0.gw.umass.edu (128.119.3.145)  1 ms  1 ms 
 2 ms
3  cht-vbns.gw.umass.edu (128.119.3.130)  6 ms 5 ms 5 ms4  jn1-at1-0-0-19.wor.vbns.
net (204.147.132.129)  16 ms 11 ms 1
3 ms 
5  jn1-so7-0-0-0.wae.vbns.net
 (204.147.136.136)  21 ms 18 ms 18
 ms 
6  abilene-vbns.abilene.ucaid.edu (198.32.11.9)  22 ms  18 ms  
22 ms
7  nycm-wash.abilene.ucaid.edu (198.32.8.46)  22 ms  22 ms  22 
ms
8  62.40.103.253 (62.40.103.253)  104 ms 109 ms 106 ms9  de2-1.de1.de.geant.net (62.40.96.129)  109 ms 102 ms 104 ms10  de.fr1.fr.geant.net (62.40.96.50)  113 ms 121 ms 114 ms11  renater-gw.fr1.fr.geant.net (62.40.103.54)  112 ms  114 ms 
 112 ms
12  nio-n2.cssi.renater.fr (193.51.206.13)  111 ms  114 ms  116
 ms
13  nice.cssi.renater.fr (195.220.98.102)  123 ms  125 ms  124 
ms
14  r3t2-nice.cssi.renater.fr (195.220.98.110)  126 ms  126 ms 
 124 ms
15  eurecom-valbonne.r3t2.ft.net (193.48.50.54)  135 ms  128 ms
  133 ms
16  194.214.211.25 (194.214.211.25)  126 ms  128 ms  126 ms17  * * *18  * * *19  fantasia.eurecom.fr (193.55.113.142)  132 ms  128 ms  136
ms
traceroute:
gaia.cs.umass.edu to www.eurecom.fr
3 delay measurements from gaia.cs.umass.edu to cs-gw.cs.umass.edu 
* means no response (probe lost, router not replying)
trans-oceaniclink
1-60
* Do some traceroutes from exotic countries at www.traceroute.or
g
Packet loss

queue (aka buffer) preceding link in buffer has finite 
capacity

packet arriving to full queue dropped (aka lost)

lost packet 
may be retransmitted 
by previous node, by 
source end system, 
or not at all
A
B
packet being transmitted
packet arriving tofull buffer is 
lost
buffer 
(waiting area)
1-61
* Check out the Java applet for an interactive animation on que
uing and loss
Internet 时延s 和 路由
什么 做
Internet 时延 & 丢包 look 像?
program: 提供 时延
measurement 从 源 到 路由器 along end-end
Internet 路径 towards destination. 用于 所有
发送
UDP 分组 这 将 reach 路由器
在 路径 towards
目的地路由器
将 返回 分组 到 发送方
发送方 times interval betw
een 传输 和 reply.

2024/09/10
15
58

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, average delay infinite!
Why?
1
23 45 67
8
恒定速率
λ
到达，
LaR=1
，满负荷，没有排队
1
23 45 67
8
平均速率依然为
λ
，但是
{2
，
4
，
6
，
8}
时刻
出现排队，各有
1
次排队
1
23 45 67
8
平均速率依然为
λ
，但是
{4
，
8}
时刻出现排
队，各有
3
次排队
原因：分组到达不是恒定速率，具有随机突发性！
理论分析参阅排队论
“
Real
”
Internet delays and routes 
（
DIY
）

what do 
“
real
”
Internet delay & loss look like? 

Traceroute(win: 
tracert
) 
program: provides delay 
measurement from source to router along end-end Internet path towards destination.  For all 
i:

sends 
three
UDP packets that will reach router 
i
on path towards 
destination

router 
i
will return packets to sender

sender times interval betw
een transmission and reply.
3 probes
3 probes
3 probes
1-59
“
Real
”
Internet delays, routes
1  cs-gw (128.119.240.254)  1 ms  1 ms  2 ms2  border1-rt-fa5-1-0.gw.umass.edu (128.119.3.145)  1 ms  1 ms 
 2 ms
3  cht-vbns.gw.umass.edu (128.119.3.130)  6 ms 5 ms 5 ms
4  jn1-at1-0-0-19.wor.vbns.
net (204.147.132.129)  16 ms 11 ms 1
3 ms 
5  jn1-so7-0-0-0.wae.vbns.net
 (204.147.136.136)  21 ms 18 ms 18
 ms 
6  abilene-vbns.abilene.ucaid.edu (198.32.11.9)  22 ms  18 ms  
22 ms
7  nycm-wash.abilene.ucaid.edu (198.32.8.46)  22 ms  22 ms  22 
ms
8  62.40.103.253 (62.40.103.253)  104 ms 109 ms 106 ms9  de2-1.de1.de.geant.net (62.40.96.129)  109 ms 102 ms 104 ms10  de.fr1.fr.geant.net (62.40.96.50)  113 ms 121 ms 114 ms11  renater-gw.fr1.fr.geant.net (62.40.103.54)  112 ms  114 ms 
 112 ms
12  nio-n2.cssi.renater.fr (193.51.206.13)  111 ms  114 ms  116
 ms
13  nice.cssi.renater.fr (195.220.98.102)  123 ms  125 ms  124 
ms
14  r3t2-nice.cssi.renater.fr (195.220.98.110)  126 ms  126 ms 
 124 ms
15  eurecom-valbonne.r3t2.ft.net (193.48.50.54)  135 ms  128 ms
  133 ms
16  194.214.211.25 (194.214.211.25)  126 ms  128 ms  126 ms
17  * * *18  * * *
19  fantasia.eurecom.fr (193.55.113.142)  132 ms  128 ms  136
ms
traceroute:
gaia.cs.umass.edu to www.eurecom.fr
3 delay measurements from gaia.cs.umass.edu to cs-gw.cs.umass.edu 
* means no response (probe lost, router not replying)
trans-oceaniclink
1-60
* Do some traceroutes from exotic countries at www.traceroute.or
g
Packet loss

queue (aka buffer) preceding link in buffer has finite capacity

packet arriving to full queue dropped (aka lost)

lost packet 
may be retransmitted 
by previous node, by 
source end system, 
or not at all
A
B
packet being transmitted
packet arriving tofull buffer is 
lost
buffer 
(waiting area)
1-61
* Check out the Java applet for an interactive animation on que
uing and loss
Internet 时延s, 路由
网络 (204.147.132.129)
gaia.cs.umass.edu 到 www.eurecom.fr
3 时延 measurements 从gaia.cs.umass.edu 到 cs-gw.cs.umass.edu
* means 无 响应 (probe 丢失的, 路由器 不 replying)
链路
* 做 一些 traceroutes 从 exotic countries 在 www.traceroute.or

2024/09/10
15
58

La/R 
→
1: avg. queuing delay large

La/R 
> 1: more 
“
work
”
arriving 
than can be serviced, average delay infinite!
Why?
1
23 45 67
8
恒定速率
λ
到达，
LaR=1
，满负荷，没有排队
1
23 45 67
8
平均速率依然为
λ
，但是
{2
，
4
，
6
，
8}
时刻
出现排队，各有
1
次排队
1
23 45 67
8
平均速率依然为
λ
，但是
{4
，
8}
时刻出现排
队，各有
3
次排队
原因：分组到达不是恒定速率，具有随机突发性！
理论分析参阅排队论
“
Real
”
Internet delays and routes 
（
DIY
）

what do 
“
real
”
Internet delay & loss look like? 

Traceroute(win: 
tracert
) 
program: provides delay 
measurement from source to router along end-end Internet path towards destination.  For all 
i:

sends 
three
UDP packets that will reach router 
i
on path towards 
destination

router 
i
will return packets to sender

sender times interval betw
een transmission and reply.
3 probes
3 probes
3 probes
1-59
“
Real
”
Internet delays, routes
1  cs-gw (128.119.240.254)  1 ms  1 ms  2 ms2  border1-rt-fa5-1-0.gw.umass.edu (128.119.3.145)  1 ms  1 ms 
 2 ms
3  cht-vbns.gw.umass.edu (128.119.3.130)  6 ms 5 ms 5 ms4  jn1-at1-0-0-19.wor.vbns.
net (204.147.132.129)  16 ms 11 ms 1
3 ms 
5  jn1-so7-0-0-0.wae.vbns.net
 (204.147.136.136)  21 ms 18 ms 18
 ms 
6  abilene-vbns.abilene.ucaid.edu (198.32.11.9)  22 ms  18 ms  
22 ms
7  nycm-wash.abilene.ucaid.edu (198.32.8.46)  22 ms  22 ms  22 
ms
8  62.40.103.253 (62.40.103.253)  104 ms 109 ms 106 ms9  de2-1.de1.de.geant.net (62.40.96.129)  109 ms 102 ms 104 ms10  de.fr1.fr.geant.net (62.40.96.50)  113 ms 121 ms 114 ms11  renater-gw.fr1.fr.geant.net (62.40.103.54)  112 ms  114 ms 
 112 ms
12  nio-n2.cssi.renater.fr (193.51.206.13)  111 ms  114 ms  116
 ms
13  nice.cssi.renater.fr (195.220.98.102)  123 ms  125 ms  124 
ms
14  r3t2-nice.cssi.renater.fr (195.220.98.110)  126 ms  126 ms 
 124 ms
15  eurecom-valbonne.r3t2.ft.net (193.48.50.54)  135 ms  128 ms
  133 ms
16  194.214.211.25 (194.214.211.25)  126 ms  128 ms  126 ms17  * * *18  * * *19  fantasia.eurecom.fr (193.55.113.142)  132 ms  128 ms  136
ms
traceroute:
gaia.cs.umass.edu to www.eurecom.fr
3 delay measurements from gaia.cs.umass.edu to cs-gw.cs.umass.edu 
* means no response (probe lost, router not replying)
trans-oceaniclink
1-60
* Do some traceroutes from exotic countries at www.traceroute.or
g
Packet loss

queue (aka buffer) preceding link in buffer has finite 
capacity

packet arriving to full queue dropped (aka lost)

lost packet 
may be retransmitted 
by previous node, by 
source end system, 
or not at all
A
B
packet being transmitted
packet arriving tofull buffer is 
lost
buffer 
(waiting area)
1-61
* Check out the Java applet for an interactive animation on que
uing and loss
分组 丢包
队列 (aka 缓冲区) preceding 链路 在 缓冲区 有 finite
容量
分组 到达的 到 完整的 队列 dropped (aka 丢失的)
丢失的 分组
可能 是 retransmitted
由 previous 节点, 由
源 端 系统,
或 不 在 所有
分组 being 已传输
分组 到达的 到完整的 缓冲区 是
丢失的
缓冲区
(waiting 区域)
* 查看 查看 Java applet 用于 交互式 animation 在 que
uing 和 丢包

2024/09/10
16
Throughput

throughput:
rate (bits/time unit) at which bits 
transferred between sender/receiver

instantaneous:
rate at given point in time

average:
rate over longer period of time
server, with
file of F bits 
to send to client
link capacity
R
s
bits/sec
link capacity
R
c
bits/sec
server sends bits 
(fluid) into pipe
pipe that can carry
fluid at rate
R
s
bits/sec)
pipe that can carry
fluid at rate
R
c
bits/sec)
1-62
Throughput (more)

R
s
< R
c
What is average end-end throughput?
R
s
bits/sec
R
c
bits/sec

R
s
> R
c
What is average end-end throughput?
link on end-end path that constrains  end-end throughput
bottleneck link
R
s
bits/sec
R
c
bits/sec
1-63
Throughput: Internet scenario
10 connections 
(fairly) share 
backbone bottleneck link R bits/sec
R
s
R
s
R
s
R
c
R
c
R
c
R

per-connection end-
end throughput: min(R
c
,R
s
,R/10)

in practice: 
R
c
or 
R
s
is 
often bottleneck
1-64
10
条连接
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5 protocol layers, service models1.6
networks under attack: security
1.7
history
1-65
吞吐量
吞吐量:
速率 (bits/time unit) 在 哪个 比特
transferred 之间 发送方/接收方
速率 在 给定的 点 在 时间
速率 在 longer period 的 时间
服务器, 与
文件 的 F 比特
到 发送 到 客户
链路 容量
链路 容量
服务器 发送 比特
(fluid) 进入 pipe
pipe 这 可以 carry
fluid 在 速率
pipe 这 可以 carry
fluid 在 速率

2024/09/10
16
Throughput

throughput:
rate (bits/time unit) at which bits 
transferred between sender/receiver

instantaneous:
rate at given point in time

average:
rate over longer period of time
server, with
file of F bits 
to send to client
link capacity
R
s
bits/sec
link capacity
R
c
bits/sec
server sends bits 
(fluid) into pipe
pipe that can carry
fluid at rate
R
s
bits/sec)
pipe that can carry
fluid at rate
R
c
bits/sec)
1-62
Throughput (more)

R
s
< R
c
What is average end-end throughput?
R
s
bits/sec
R
c
bits/sec

R
s
> R
c
What is average end-end throughput?
link on end-end path that constrains  end-end 
throughput
bottleneck link
R
s
bits/sec
R
c
bits/sec
1-63
Throughput: Internet scenario
10 connections 
(fairly) share 
backbone bottleneck link R bits/sec
R
s
R
s
R
s
R
c
R
c
R
c
R

per-connection end-end throughput: min(R
c
,R
s
,R/10)

in practice: 
R
c
or 
R
s
is 
often bottleneck
1-64
10
条连接
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5 protocol layers, service models
1.6
networks under attack: security
1.7
history
1-65
吞吐量 (更多)
什么 是 average end-end 吞吐量?
什么 是 average end-end 吞吐量?
链路 在 end-end 路径 这 constrains end-end
吞吐量
瓶颈 链路

2024/09/10
16
Throughput

throughput:
rate (bits/time unit) at which bits 
transferred between sender/receiver

instantaneous:
rate at given point in time

average:
rate over longer period of time
server, with
file of F bits 
to send to client
link capacity
R
s
bits/sec
link capacity
R
c
bits/sec
server sends bits 
(fluid) into pipe
pipe that can carry
fluid at rate
R
s
bits/sec)
pipe that can carry
fluid at rate
R
c
bits/sec)
1-62
Throughput (more)

R
s
< R
c
What is average end-end throughput?
R
s
bits/sec
R
c
bits/sec

R
s
> R
c
What is average end-end throughput?
link on end-end path that constrains  end-end throughput
bottleneck link
R
s
bits/sec
R
c
bits/sec
1-63
Throughput: Internet scenario
10 connections 
(fairly) share 
backbone bottleneck link R bits/sec
R
s
R
s
R
s
R
c
R
c
R
c
R

per-connection end-end throughput: min(R
c
,R
s
,R/10)

in practice: 
R
c
or 
R
s
is 
often bottleneck
1-64
10
条连接
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5 protocol layers, service models1.6
networks under attack: security
1.7
history
1-65
吞吐量: Internet 场景
10 连接
骨干 瓶颈 链路 R bits/sec
端 吞吐量:最小值(R在 practice:
或
是
经常 瓶颈

2024/09/10
16
Throughput

throughput:
rate (bits/time unit) at which bits 
transferred between sender/receiver

instantaneous:
rate at given point in time

average:
rate over longer period of time
server, with
file of F bits 
to send to client
link capacity
R
s
bits/sec
link capacity
R
c
bits/sec
server sends bits 
(fluid) into pipe
pipe that can carry
fluid at rate
R
s
bits/sec)
pipe that can carry
fluid at rate
R
c
bits/sec)
1-62
Throughput (more)

R
s
< R
c
What is average end-end throughput?
R
s
bits/sec
R
c
bits/sec

R
s
> R
c
What is average end-end throughput?
link on end-end path that constrains  end-end 
throughput
bottleneck link
R
s
bits/sec
R
c
bits/sec
1-63
Throughput: Internet scenario
10 connections 
(fairly) share 
backbone bottleneck link R bits/sec
R
s
R
s
R
s
R
c
R
c
R
c
R

per-connection end-end throughput: min(R
c
,R
s
,R/10)

in practice: 
R
c
or 
R
s
is 
often bottleneck
1-64
10
条连接
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5 protocol layers, service models1.6
networks under attack: security
1.7
history
1-65
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络 结构
时延, 丢包, 吞吐量 在 网络
1.5 协议分层, 服务模型
网络 under attack: 安全历史

2024/09/10
17
Protocol 
“
layers
”
Networks are complex,with many 
“
pieces
”
:

hosts

routers

links of various media

applications

protocols

hardware, software
Question:
is there any hope of 
organizing
structure of 
network?
.... or at least our 
discussion of networks?
1-66
67
Protocol 
“
layers
”
Application
transport
network
link
physical
application
presentation
sessiontransport
network
link
physical
ISO/OSI reference model
Internet protocol stack
Why layering?
dealing with complex systems:

explicit structure allows identification, relationship of 
complex system’s pieces

layered 
reference model
for discussion

modularization eases maintenance, updating of system

change of implementation of layer
’
s service transparent to rest 
of system

e.g., change in gate procedur
e doesn’t affect rest of system

layering considered harmful?
1-68
Modular
Internet protocol stack

application:
supporting network applications

FTP, SMTP, HTTP

transport:
process-process data transfer

TCP, UDP

network:
routing of datagrams from source 
to destination

IP, routing protocols

link:
data transfer between neighboring  
network elements

Ethernet, 802.111 (WiFi), PPP

physical:
bits 
“
on the wire
”
1-69
Application
transport
network
link
physical
协议
层
网络 是 complex,与 许多
主机
路由器
链路 的 various media
应用
协议hardware, 软件
问题:
是 存在 任何 hope 的
结构 的
网络?
.... 或 在 最少的 我们的
讨论 的 网络?

2024/09/10
17
Protocol 
“
layers
”
Networks are complex,with many 
“
pieces
”
:

hosts

routers

links of various media

applications

protocols

hardware, software
Question:
is there any hope of 
organizing
structure of 
network?
.... or at least our 
discussion of networks?
1-66
67
Protocol 
“
layers
”
Application
transport
network
link
physical
application
presentation
sessiontransport
network
link
physical
ISO/OSI reference model
Internet protocol stack
Why layering?
dealing with complex systems:

explicit structure allows identification, relationship of complex system’s pieces

layered 
reference model
for discussion

modularization eases maintenance, updating of system

change of implementation of layer
’
s service transparent to rest 
of system

e.g., change in gate procedur
e doesn’t affect rest of system

layering considered harmful?
1-68
Modular
Internet protocol stack

application:
supporting network applications

FTP, SMTP, HTTP

transport:
process-process data transfer

TCP, UDP

network:
routing of datagrams from source 
to destination

IP, routing protocols

link:
data transfer between neighboring  
network elements

Ethernet, 802.111 (WiFi), PPP

physical:
bits 
“
on the wire
”
1-69
Application
transport
network
link
physical
协议
层
应用
运输
网络
链路
物理
应用
运输
网络
链路
物理
ISO/OSI reference 模型
Internet 协议 stack

2024/09/10
17
Protocol 
“
layers
”
Networks are complex,with many 
“
pieces
”
:

hosts

routers

links of various media

applications

protocols

hardware, software
Question:
is there any hope of 
organizing
structure of 
network?
.... or at least our 
discussion of networks?
1-66
67
Protocol 
“
layers
”
Application
transport
network
link
physical
application
presentation
sessiontransport
network
link
physical
ISO/OSI reference model
Internet protocol stack
Why layering?
dealing with complex systems:

explicit structure allows identification, relationship of 
complex system’s pieces

layered 
reference model
for discussion

modularization eases maintenance, updating of system

change of implementation of layer
’
s service transparent to rest 
of system

e.g., change in gate procedur
e doesn’t affect rest of system

layering considered harmful?
1-68
Modular
Internet protocol stack

application:
supporting network applications

FTP, SMTP, HTTP

transport:
process-process data transfer

TCP, UDP

network:
routing of datagrams from source 
to destination

IP, routing protocols

link:
data transfer between neighboring  
network elements

Ethernet, 802.111 (WiFi), PPP

physical:
bits 
“
on the wire
”
1-69
Application
transport
network
link
physical
dealing 与 complex 系统:
explicit 结构 允许 identification, relationship 的
complex 系统’s pieces
reference 模型
用于 讨论
modularization eases maintenance, updating 的 系统
变化 的 实现 的 层
s 服务 transparent 到 rest
的 系统
e.g., 变化 在 gate procedur
e doesn’t affect rest 的 系统

2024/09/10
17
Protocol 
“
layers
”
Networks are complex,with many 
“
pieces
”
:

hosts

routers

links of various media

applications

protocols

hardware, software
Question:
is there any hope of 
organizing
structure of 
network?
.... or at least our 
discussion of networks?
1-66
67
Protocol 
“
layers
”
Application
transport
network
link
physical
application
presentation
sessiontransport
network
link
physical
ISO/OSI reference model
Internet protocol stack
Why layering?
dealing with complex systems:

explicit structure allows identification, relationship of complex system’s pieces

layered 
reference model
for discussion

modularization eases maintenance, updating of system

change of implementation of layer
’
s service transparent to rest 
of system

e.g., change in gate procedur
e doesn’t affect rest of system

layering considered harmful?
1-68
Modular
Internet protocol stack

application:
supporting network applications

FTP, SMTP, HTTP

transport:
process-process data transfer

TCP, UDP

network:
routing of datagrams from source 
to destination

IP, routing protocols

link:
data transfer between neighboring  
network elements

Ethernet, 802.111 (WiFi), PPP

physical:
bits 
“
on the wire
”
1-69
Application
transport
network
link
physical
Internet 协议 stack
应用:
supporting 网络 应用
运输:
process-process 数据 传输
网络:
路由 的 数据报 从 源
到 目的地
IP, 路由协议
链路:
数据 传输 之间 neighboring
网络 elements
以太网, 802.111 (WiFi), PPP
物理:
比特
在 wire
应用
运输
网络
链路
物理

2024/09/10
18
ISO/OSI reference model

presentation:
allow applications to interpret 
meaning of data, e.g., 
encryption, 
compression
, machine-specific conventions

session:
synchronization, check pointing, 
recovery of data exchange

Internet stack 
“
missing
”
these layers!

these services, 
if needed,
must be implemented in 
application

needed?
1-70
application
presentation
sessiontransport
network
link
physical
2023.9.4
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用进程数据先传送到应用层加上应用层首部，成为应用层 PDU
PDU (Protocol Data Unit)：协议数据单元。OSI 参考模型把对等层次之间传送的数据单位称为该层的协议数据单元 PDU。
71
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层 PDU 再传送到运输层
加上运输层首部，成为运输层报文
72
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层报文再传送到网络层加上网络层首部，成为 IP 数据报（或分组）
73
ISO/OSI reference 模型
allow 应用 到 interpret
meaning 的 数据, e.g.,
synchronization, 查看 pointing,
recovery 的 数据 交换
这些 层!
这些 服务,
如果 needed,
必须 是 implemented 在
应用
应用
运输
网络
链路
物理

2024/09/10
18
ISO/OSI reference model
presentation:
allow applications to interpret 
meaning of data, e.g., 
encryption, 
compression
, machine-specific conventions

session:
synchronization, check pointing, 
recovery of data exchange

Internet stack 
“
missing
”
these layers!

these services, 
if needed,
must be implemented in 
application

needed?
1-70
application
presentation
sessiontransport
network
link
physical
2023.9.4
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用进程数据先传送到应用层加上应用层首部，成为应用层 PDU
PDU (Protocol Data Unit)：协议数据单元。OSI 参考模型把对等层次之间传送的数据单位称为该层的协议数据单元 PDU。
71
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层 PDU 再传送到运输层加上运输层首部，成为运输层报文
72
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层报文再传送到网络层加上网络层首部，成为 IP 数据报（或分组）
73
PDU (协议 数据 Unit)：协议数据单元。

2024/09/10
18
ISO/OSI reference model

presentation:
allow applications to interpret 
meaning of data, e.g., 
encryption, 
compression
, machine-specific conventions

session:
synchronization, check pointing, 
recovery of data exchange

Internet stack 
“
missing
”
these layers!

these services, 
if needed,
must be implemented in 
application

needed?
1-70
application
presentation
sessiontransport
network
link
physical
2023.9.4
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用进程数据先传送到应用层加上应用层首部，成为应用层 PDU
PDU (Protocol Data Unit)：协议数据单元。OSI 参考模型把对等层次之间传送的数据单位称为该层的协议数据单元 PDU。
71
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层 PDU 再传送到运输层加上运输层首部，成为运输层报文
72
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层报文再传送到网络层加上网络层首部，成为 IP 数据报（或分组）
73

2024/09/10
18
ISO/OSI reference model
presentation:
allow applications to interpret 
meaning of data, e.g., 
encryption, 
compression
, machine-specific conventions

session:
synchronization, check pointing, 
recovery of data exchange

Internet stack 
“
missing
”
these layers!

these services, 
if needed,
must be implemented in 
application

needed?
1-70
application
presentation
session
transport
network
link
physical
2023.9.4
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用进程数据先传送到应用层加上应用层首部，成为应用层 PDU
PDU (Protocol Data Unit)：协议数据单元。OSI 参考模型把对等层次之间传送的数据单位称为该层的协议数据单元 PDU。
71
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层 PDU 再传送到运输层加上运输层首部，成为运输层报文
72
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层报文再传送到网络层加上网络层首部，成为 IP 数据报（或分组）
73

2024/09/10
19
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
IP 数据报再传送到数据链路层加上链路层首部和尾部，成为数据链路层帧
74
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层帧再传送到物理层最下面的物理层把比特流传送到物理媒体
75
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
电信号（或光信号）在物理媒体中传播
从发送端物理层传送到接收端物理层
物理传输媒体
76
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
物理层接收到比特流，上交给数据链路层
77

2024/09/10
19
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
IP 数据报再传送到数据链路层加上链路层首部和尾部，成为数据链路层帧
74
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层帧再传送到物理层最下面的物理层把比特流传送到物理媒体
75
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
电信号（或光信号）在物理媒体中传播
从发送端物理层传送到接收端物理层
物理传输媒体
76
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
物理层接收到比特流，上交给数据链路层
77

2024/09/10
19
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
IP 数据报再传送到数据链路层加上链路层首部和尾部，成为数据链路层帧
74
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层帧再传送到物理层最下面的物理层把比特流传送到物理媒体
75
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
电信号（或光信号）在物理媒体中传播
从发送端物理层传送到接收端物理层
物理传输媒体
76
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
物理层接收到比特流，上交给数据链路层
77

2024/09/10
19
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
IP 数据报再传送到数据链路层加上链路层首部和尾部，成为数据链路层帧
74
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层帧再传送到物理层最下面的物理层把比特流传送到物理媒体
75
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
电信号（或光信号）在物理媒体中传播
从发送端物理层传送到接收端物理层
物理传输媒体
76
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
物理层接收到比特流，上交给数据链路层
77

2024/09/10
20
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部
取出数据部分，上交给网络层
78
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去首部，
取出数据部分上交给运输层
79
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去首部，
取出数据部分上交给应用层
80
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去首部，
取出应用程序数据上交给应用进程
81

2024/09/10
20
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部
取出数据部分，上交给网络层
78
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去首部，
取出数据部分上交给运输层
79
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去首部，
取出数据部分上交给应用层
80
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去首部，
取出应用程序数据上交给应用进程
81

2024/09/10
20
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部
取出数据部分，上交给网络层
78
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去首部，
取出数据部分上交给运输层
79
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去首部，
取出数据部分上交给应用层
80
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去首部，
取出应用程序数据上交给应用进程
81

2024/09/10
20
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部
取出数据部分，上交给网络层
78
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去首部，
取出数据部分上交给运输层
79
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去首部，
取出数据部分上交给应用层
80
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去首部，
取出应用程序数据上交给应用进程
81

2024/09/10
21
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
82
主机
1 
向主机
2 
发送数据
主机 1
54321
AP
1
主机 2
54321
AP
2
应用程序数据
应用层首部
H
5
10100110100101  比 特 流 110101110101
应用程序数据
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
H
4
H
5
应用程序数据
H
4
运输层首部
H
3
网络层首部
H
2
链路层
首部
T
2
链路层
尾部
注意观察加入或剥去首部（尾部）的层次
83
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
主机 2 的物理层收到比特流后
交给数据链路层
10100110100101  比 特 流 110101110101
H
2
T
2
H
3
H
4
H
5
应用程序数据
84
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部后
把帧的数据部分交给网络层
H
3
H
4
H
5
应用程序数据
H
2
T
2
H
3
H
4
H
5
应用程序数据
85

2024/09/10
21
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
82
主机
1 
向主机
2 
发送数据
主机 1
54321
AP
1
主机 2
54321
AP
2
应用程序数据
应用层首部
H
5
10100110100101  比 特 流 110101110101
应用程序数据
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
H
4
H
5
应用程序数据
H
4
运输层首部
H
3
网络层首部
H
2
链路层
首部
T
2
链路层
尾部
注意观察加入或剥去首部（尾部）的层次
83
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
主机 2 的物理层收到比特流后
交给数据链路层
10100110100101  比 特 流 110101110101
H
2
T
2
H
3
H
4
H
5
应用程序数据
84
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部后
把帧的数据部分交给网络层
H
3
H
4
H
5
应用程序数据
H
2
T
2
H
3
H
4
H
5
应用程序数据
85

2024/09/10
21
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
82
主机
1 
向主机
2 
发送数据
主机 1
54321
AP
1
主机 2
54321
AP
2
应用程序数据
应用层首部
H
5
10100110100101  比 特 流 110101110101
应用程序数据
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
H
4
H
5
应用程序数据
H
4
运输层首部
H
3
网络层首部
H
2
链路层
首部
T
2
链路层
尾部
注意观察加入或剥去首部（尾部）的层次
83
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
主机 2 的物理层收到比特流后
交给数据链路层
10100110100101  比 特 流 110101110101
H
2
T
2
H
3
H
4
H
5
应用程序数据
84
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部后
把帧的数据部分交给网络层
H
3
H
4
H
5
应用程序数据
H
2
T
2
H
3
H
4
H
5
应用程序数据
85

2024/09/10
21
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
82
主机
1 
向主机
2 
发送数据
主机 1
54321
AP
1
主机 2
54321
AP
2
应用程序数据
应用层首部
H
5
10100110100101  比 特 流 110101110101
应用程序数据
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
H
4
H
5
应用程序数据
H
4
运输层首部
H
3
网络层首部
H
2
链路层
首部
T
2
链路层
尾部
注意观察加入或剥去首部（尾部）的层次
83
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
主机 2 的物理层收到比特流后
交给数据链路层
10100110100101  比 特 流 110101110101
H
2
T
2
H
3
H
4
H
5
应用程序数据
84
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
数据链路层剥去帧首部和帧尾部后
把帧的数据部分交给网络层
H
3
H
4
H
5
应用程序数据
H
2
T
2
H
3
H
4
H
5
应用程序数据
85

2024/09/10
22
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去分组首部后
把分组的数据部分交给运输层
H
4
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
86
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去报文首部后
把报文的数据部分交给应用层
H
5
应用程序数据
H
5
应用程序数据
H
4
87
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去应用层 PDU 首部后
把应用程序数据交给应用进程
应用程序数据应用程序数据
H
5
88
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
89

2024/09/10
22
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去分组首部后
把分组的数据部分交给运输层
H
4
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
86
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去报文首部后
把报文的数据部分交给应用层
H
5
应用程序数据
H
5
应用程序数据
H
4
87
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去应用层 PDU 首部后
把应用程序数据交给应用进程
应用程序数据应用程序数据
H
5
88
主机
1 
向主机
2 
发送数据
54321
5
4
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
89

2024/09/10
22
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去分组首部后
把分组的数据部分交给运输层
H
4
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
86
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去报文首部后
把报文的数据部分交给应用层
H
5
应用程序数据
H
5
应用程序数据
H
4
87
主机
1 
向主机
2 
发送数据
5432
1
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去应用层 PDU 首部后
把应用程序数据交给应用进程
应用程序数据应用程序数据
H
5
88
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
89

2024/09/10
22
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
网络层剥去分组首部后
把分组的数据部分交给运输层
H
4
H
5
应用程序数据
H
4
H
5
应用程序数据
H
3
86
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
运输层剥去报文首部后
把报文的数据部分交给应用层
H
5
应用程序数据
H
5
应用程序数据
H
4
87
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
应用层剥去应用层 PDU 首部后
把应用程序数据交给应用进程
应用程序数据应用程序数据
H
5
88
主机
1 
向主机
2 
发送数据
54321
54
3
2
1
主机 1
AP
1
主机 2
AP
2
我收到了 AP
1
发来的
应用程序数据！
89

2024/09/10
23
network
link
physical
application
transport
network
link
physical
application
transport
network
link
physical
Encapsulation: an end-end view
source
H
t
H
n
M
segment
H
t
datagram
destination
H
t
H
n
H
l
M
H
t
H
n
M
H
t
M
M
H
t
H
n
H
l
M
H
t
H
n
M
H
t
H
n
M
H
t
H
n
H
l
M
router
switch
message
M
H
t
M
H
n
frame
link
physical
Introduction: 1-90
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6 networks under attack: security1.7
history
1-91
Network security

field of network security:

how bad guys can 
attack
computer networks

how we can 
defend
networks against attacks

how to 
design
architectures that are immune to 
attacks

Internet not originally designed with (much) 
security in mind

original vision:
“
a group of mutually trusting 
users attached to a transparent network
”

Internet protocol designers playing 
“
catch-up
”

security considerations in all layers!
1-92
Bad guys: put malware into hosts via Internet
malware can get in host from:

virus: 
a small program written to a
lter the way a computer 
operates, without the permission or knowledge of the user. A virus must meet two criteria:

It must execute itself.

It must replicate itself.

worm: 
self-replicating infection by 
passively
receiving object 
that gets itself executed

What is the difference 
between virus and worm?

Virus: Effect host’s behavior

Worm: travel from computer to computer
1-93
网络
链路
物理
应用
运输
网络
链路
物理
应用
运输
网络
链路
物理
源
报文段
数据报
目的地
路由器
交换机
报文
帧
链路
物理
引言: 1-90

2024/09/10
23
network
link
physical
application
transport
network
link
physical
application
transport
network
link
physical
Encapsulation: an end-end view
source
H
t
H
n
M
segment
H
t
datagram
destination
H
t
H
n
H
l
M
H
t
H
n
M
H
t
M
M
H
t
H
n
H
l
M
H
t
H
n
M
H
t
H
n
M
H
t
H
n
H
l
M
router
switch
message
M
H
t
M
H
n
frame
link
physical
Introduction: 1-90
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6 networks under attack: security
1.7
history
1-91
Network security

field of network security:

how bad guys can 
attack
computer networks

how we can 
defend
networks against attacks

how to 
design
architectures that are immune to 
attacks

Internet not originally designed with (much) security in mind

original vision:
“
a group of mutually trusting 
users attached to a transparent network
”

Internet protocol designers playing 
“
catch-up
”

security considerations in all layers!
1-92
Bad guys: put malware into hosts via Internet

malware can get in host from:

virus: 
a small program written to a
lter the way a computer 
operates, without the permission or knowledge of the user. A 
virus must meet two criteria:

It must execute itself.

It must replicate itself.

worm: 
self-replicating infection by 
passively
receiving object 
that gets itself executed

What is the difference 
between virus and worm?

Virus: Effect host’s behavior

Worm: travel from computer to computer
1-93
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络结构
时延, 丢包, 吞吐量 在 网络协议分层, 服务模型
1.6 网络 under attack: 安全
历史

2024/09/10
23
network
link
physical
application
transport
network
link
physical
application
transport
network
link
physical
Encapsulation: an end-end view
source
H
t
H
n
M
segment
H
t
datagram
destination
H
t
H
n
H
l
M
H
t
H
n
M
H
t
M
M
H
t
H
n
H
l
M
H
t
H
n
M
H
t
H
n
M
H
t
H
n
H
l
M
router
switch
message
M
H
t
M
H
n
frame
link
physical
Introduction: 1-90
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6 networks under attack: security1.7
history
1-91
Network security

field of network security:

how bad guys can 
attack
computer networks

how we can 
defend
networks against attacks

how to 
design
architectures that are immune to 
attacks

Internet not originally designed with (much) 
security in mind

original vision:
“
a group of mutually trusting 
users attached to a transparent network
”

Internet protocol designers playing 
“
catch-up
”

security considerations in all layers!
1-92
Bad guys: put malware into hosts via Internet
malware can get in host from:

virus: 
a small program written to a
lter the way a computer 
operates, without the permission or knowledge of the user. A virus must meet two criteria:

It must execute itself.

It must replicate itself.

worm: 
self-replicating infection by 
passively
receiving object 
that gets itself executed

What is the difference 
between virus and worm?

Virus: Effect host’s behavior

Worm: travel from computer to computer
1-93
网络 安全
字段 的 网络 安全:
如何 bad guys 可以
计算机 网络
如何 我们 可以
网络 against attacks
如何 到
architectures 这 是 immune 到
Internet 不 originally designed 与 (much)
安全 在 mind
原始 vision:
a group 的 mutually trusting
用户 attached 到 a transparent 网络
Internet 协议 designers playing安全 considerations 在 所有 层!

2024/09/10
23
network
link
physical
application
transport
network
link
physical
application
transport
network
link
physical
Encapsulation: 
an end-end view
source
H
t
H
n
M
segment
H
t
datagram
destination
H
t
H
n
H
l
M
H
t
H
n
M
H
t
M
M
H
t
H
n
H
l
M
H
t
H
n
M
H
t
H
n
M
H
t
H
n
H
l
M
router
switch
message
M
H
t
M
H
n
frame
link
physical
Introduction: 1-90
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6 networks under attack: security1.7
history
1-91
Network security

field of network security:

how bad guys can 
attack
computer networks

how we can 
defend
networks against attacks

how to 
design
architectures that are immune to 
attacks

Internet not originally designed with (much) security in mind

original vision:
“
a group of mutually trusting 
users attached to a transparent network
”

Internet protocol designers playing 
“
catch-up
”

security considerations in all layers!
1-92
Bad guys: put malware into hosts via Internet
malware can get in host from:

virus: 
a small program written to a
lter the way a computer 
operates, without the permission or knowledge of the user. A 
virus must meet two criteria:

It must execute itself.

It must replicate itself.

worm: 
self-replicating infection by 
passively
receiving object 
that gets itself executed

What is the difference 
between virus and worm?

Virus: Effect host’s behavior

Worm: travel from computer to computer
1-93
Bad guys: put malware 进入 主机 通过 Internet
malware 可以 获得 在 主机 从:
a small program written 到 a
lter way a 计算机
operates, 没有 permission 或 knowledge 的 user.
virus 必须 meet 两个 criteria:
它 必须 execute itself.它 必须 replicate itself.
self-replicating infection 由
接收 对象
这 获得 itself executed什么 是 difference
之间 virus 和 worm?
Virus: Effect 主机’s behaviorWorm: travel 从 计算机 到 计算机

2024/09/10
24
Bad guys: put malware into hosts via Internet

spyware malware
can record keystrokes, web sites 
visited, upload info to collection site

infected host can be enrolled in  
botnet,
used for spam. 
DDoS attacks
1-94
Denial of Service (DoS):
attackers make resources (server, 
bandwidth) unavailable to legitimate traffic by overwhelming resource with bogus traffic
1.
select target
2.
break into hosts around 
the network (see botnet)
3.
send packets to target from 
compromised hosts
Bad guys: attack server, network infrastructure
target
1-95
Bad guys can sniff packets
packet 
“
sniffing
”
:

broadcast media (shared ethernet, wireless)

promiscuous network interface r
eads/records all packets (e.g., 
including passwords!) passing by
A
B
C
src:B dest:A     payload

wireshark software used for end-of-chapter labs is a (free) pac
ket-
sniffer
1-96
Bad guys can use fake addresses
IP spoofing:
send packet with false source address
A
B
C
src:B
dest:A  payload
1-97
... lots more on security (throughout, Chapter 8)
Bad guys: put malware 进入 主机 通过 Internet
可以 record keystrokes, Web 站点
被访, upload 信息 到 collection 站点
infected 主机 可以 是 enrolled 在
用于 用于 spam.

2024/09/10
24
Bad guys: put malware into hosts via Internet

spyware malware
can record keystrokes, web sites 
visited, upload info to collection site

infected host can be enrolled in  
botnet,
used for spam. 
DDoS attacks
1-94
Denial of Service (DoS):
attackers make resources (server, 
bandwidth) unavailable to legitimate traffic by overwhelming resource with bogus traffic
1.
select target
2.
break into hosts around 
the network (see botnet)
3.
send packets to target from 
compromised hosts
Bad guys: attack server, network infrastructure
target
1-95
Bad guys can sniff packets
packet 
“
sniffing
”
:

broadcast media (shared ethernet, wireless)

promiscuous network interface r
eads/records all packets (e.g., 
including passwords!) passing by
A
B
C
src:B dest:A     payload

wireshark software used for end-of-chapter labs is a (free) pac
ket-
sniffer
1-96
Bad guys can use fake addresses
IP spoofing:
send packet with false source address
A
B
C
src:B
dest:A  payload
1-97
... lots more on security (throughout, Chapter 8)
Denial 的 服务 (DoS):
attackers 生成 resources (服务器,
带宽) unavailable 到 legitimate 流量 由overwhelming resource 与 bogus 流量
break 进入 主机 around
网络 (见 botnet)
发送 分组 到 target 从
compromised 主机
Bad guys: attack 服务器, 网络 基础设施

2024/09/10
24
Bad guys: put malware into hosts via Internet

spyware malware
can record keystrokes, web sites 
visited, upload info to collection site

infected host can be enrolled in  
botnet,
used for spam. 
DDoS attacks
1-94
Denial of Service (DoS):
attackers make resources (server, 
bandwidth) unavailable to legitimate traffic by overwhelming resource with bogus traffic
1.
select target
2.
break into hosts around 
the network (see botnet)
3.
send packets to target from 
compromised hosts
Bad guys: attack server, network infrastructure
target
1-95
Bad guys can sniff packets
packet 
“
sniffing
”
:

broadcast media (shared ethernet, wireless)

promiscuous network interface r
eads/records all packets (e.g., 
including passwords!) passing by
A
B
C
src:B dest:A     payload

wireshark software used for end-of-chapter labs is a (free) pac
ket-
sniffer
1-96
Bad guys can use fake addresses
IP spoofing:
send packet with false source address
A
B
C
src:B
dest:A  payload
1-97
... lots more on security (throughout, Chapter 8)
Bad guys 可以 sniff 分组
分组
广播 media (共享 以太网, 无线)promiscuous 网络 接口 r
eads/records 所有 分组 (e.g.,
包括 passwords!) passing 由
载荷
wireshark 软件 用于 用于 end-of-chapter labs 是 a (free) pac

2024/09/10
24
Bad guys: put malware into hosts via Internet

spyware malware
can record keystrokes, web sites 
visited, upload info to collection site

infected host can be enrolled in  
botnet,
used for spam. 
DDoS attacks
1-94
Denial of Service (DoS):
attackers make resources (server, 
bandwidth) unavailable to legitimate traffic by overwhelming resource with bogus traffic
1.
select target
2.
break into hosts around 
the network (see botnet)
3.
send packets to target from 
compromised hosts
Bad guys: attack server, network infrastructure
target
1-95
Bad guys can sniff packets
packet 
“
sniffing
”
:

broadcast media (shared ethernet, wireless)

promiscuous network interface r
eads/records all packets (e.g., 
including passwords!) passing by
A
B
C
src:B dest:A     payload

wireshark software used for end-of-chapter labs is a (free) pac
ket-
sniffer
1-96
Bad guys can use fake addresses
IP spoofing:
send packet with false source address
A
B
C
src:B
dest:A  payload
1-97
... lots more on security (throughout, Chapter 8)
Bad guys 可以 使用 fake 地址
发送 分组 与 false 源 地址
载荷
... lots 更多 在 安全 (throughout, 第 8 章)

2024/09/10
25
Lines of defense:

authentication: 
proving you are who you say you are
•
cellular networks provides hardwa
re identity via SIM card; no s
uch 
hardware assist in traditional Internet

confidentiality: 
via encryption

integrity checks: 
digital signatures prevent/detect tampering

access restrictions:  
password‐protected VPNs

firewalls: 
specialized “middleboxes” in access and core 
networks:

off‐by‐default: filter incoming 
packets to restrict senders, re
ceivers, 
applications 

detecting/reacting to DOS attacks
... lots more on security (throughout, Chapter 8)
Introduction: 1-98
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network 
structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7 history (read after class)
1-99
Introduction: summary
covered a 
“
ton
”
of material!

Internet overview

what
’
s a protocol?

network edge, core, access network

packet-switching versus circuit-switching

Internet structure

performance: loss, delay, throughput

layering, service models

security

history
you now have:
context, overview, “
feel
”
of networking

more depth, detail 
to 
follow!
The End of Chapter 1
Email: xieyi5@mail.sysu.edu.cnhttps://cse.sysu.e
du.cn/content/2462
Thanks
Q & A
线路 的 defense:
proving 你 是 who 你 say 你 是
蜂窝 网络 提供 hardwa
re identity 通过 SIM card; 无 s
hardware assist 在 传统的 Internet
通过 encryption
访问 restrictions:
specialized “middleboxes” 在 访问 和 核心
网络:
off‐由‐default: filter 传入的
分组 到 restrict senders, re
应用
detecting/reacting 到 DOS attacks
... lots 更多 在 安全 (throughout, 第 8 章)
引言: 1-98

2024/09/10
25
Lines of defense:

authentication: 
proving you are who you say you are
•
cellular networks provides hardwa
re identity via SIM card; no s
uch 
hardware assist in traditional Internet

confidentiality: 
via encryption

integrity checks: 
digital signatures prevent/detect tampering

access restrictions:  
password‐protected VPNs

firewalls: 
specialized “middleboxes” in access and core 
networks:

off‐by‐default: filter incoming 
packets to restrict senders, re
ceivers, 
applications 

detecting/reacting to DOS attacks
... lots more on security (throughout, Chapter 8)
Introduction: 1-98
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network 
structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7 history (read after class)
1-99
Introduction: summary
covered a 
“
ton
”
of material!

Internet overview

what
’
s a protocol?

network edge, core, access network

packet-switching versus circuit-switching

Internet structure

performance: loss, delay, throughput

layering, service models

security

history
you now have:
context, overview, “
feel
”
of networking

more depth, detail 
to 
follow!
The End of Chapter 1
Email: xieyi5@mail.sysu.edu.cnhttps://cse.sysu.e
du.cn/content/2462
Thanks
Q & A
第 1 章: 引言
1.1 什么
是
网络边缘
端 系统, 接入网络, 链路
网络核心
分组 交换, 电路 交换, 网络
结构
时延, 丢包, 吞吐量 在 网络协议分层, 服务模型网络 under attack: 安全
1.7 历史 (read 之后 课堂)

2024/09/10
25
Lines of defense:

authentication: 
proving you are who you say you are
•
cellular networks provides hardwa
re identity via SIM card; no s
uch 
hardware assist in traditional Internet

confidentiality: 
via encryption

integrity checks: 
digital signatures prevent/detect tampering

access restrictions:  
password‐protected VPNs

firewalls: 
specialized “middleboxes” in access and core 
networks:

off‐by‐default: filter incoming 
packets to restrict senders, re
ceivers, 
applications 

detecting/reacting to DOS attacks
... lots more on security (throughout, Chapter 8)
Introduction: 1-98
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network 
structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7 history (read after class)
1-99
Introduction: summary
covered a 
“
ton
”
of material!

Internet overview

what
’
s a protocol?

network edge, core, access network

packet-switching versus circuit-switching

Internet structure

performance: loss, delay, throughput

layering, service models

security

history
you now have:
context, overview, “
feel
”
of networking

more depth, detail 
to 
follow!
The End of Chapter 1
Email: xieyi5@mail.sysu.edu.cnhttps://cse.sysu.e
du.cn/content/2462
Thanks
Q & A
引言: 总结
的 材料!
Internet 概述什么
s a 协议?
网络边缘, 核心, 访问 网络
分组-switching 与Internet 结构
性能: 丢包, 时延,吞吐量layering, 服务模型
安全
历史
你 现在 有:
context, 概述,
的 网络
更多 depth, detail
到
端 的 第 1 章

2024/09/10
25
Lines of defense:

authentication: 
proving you are who you say you are
•
cellular networks provides hardwa
re identity via SIM card; no s
uch 
hardware assist in traditional Internet

confidentiality: 
via encryption

integrity checks: 
digital signatures prevent/detect tampering

access restrictions:  
password‐protected VPNs

firewalls: 
specialized “middleboxes” in access and core 
networks:

off‐by‐default: filter incoming 
packets to restrict senders, re
ceivers, 
applications 

detecting/reacting to DOS attacks
... lots more on security (throughout, Chapter 8)
Introduction: 1-98
Chapter 1: introduction
1.1 what 
is
the Internet?
1.2
network edge

end systems, access networks, links
1.3 
network core

packet switching, circuit switching, network 
structure
1.4 
delay, loss, throughput in networks
1.5
protocol layers, service models
1.6
networks under attack: security
1.7 history (read after class)
1-99
Introduction: summary
covered a 
“
ton
”
of material!

Internet overview

what
’
s a protocol?

network edge, core, access network

packet-switching versus circuit-switching

Internet structure

performance: loss, delay, throughput

layering, service models

security

history
you now have:
context, overview, “
feel
”
of networking

more depth, detail 
to 
follow!
The End of Chapter 1
Email: xieyi5@mail.sysu.edu.cnhttps://cse.sysu.e
du.cn/content/2462
Thanks
Q & A
电子邮件: xieyi5@mail.sysu.edu.cn
谢谢
