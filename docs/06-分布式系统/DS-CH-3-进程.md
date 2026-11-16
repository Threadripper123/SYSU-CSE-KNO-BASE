# DS-CH-3-进程

> 主题库：分布式系统　｜　源文件：DS-CH-3-进程.pdf　｜　8,702 字符　｜　68 个片段　｜　更新于 2026-11-11


Distributed Systems
分布式系统
陈鹏飞
计算机学院
chchenpf7@mail.sysu.edu.cn
办公室：学院楼310
主页：http://sdcs.sysu.edu.cn/node/3747

2
第三讲
—
分布式中的进程和虚拟化

3
分布式系统中的线程
1
虚拟化
2
客户—服务典型结构
3
计算迁移
4
大纲

4
分布式系统中的线程
1

5
进程
Ø
何为进程？
Ø
何为线程？
Ø
操作系统如何管理进
程？
初始化、分配资源、调
度、销毁

6
分布式系统中的线程
Ø
基本思想：
在物理处理器上用软件创建虚拟处理器：
p处理器： 提供和运行一系列指令集合的硬件平台；
p线程： 一个最小的可执行一系列指令的软件处理器。 保
存线程的上下文意味着终止线程当前的执行，在其他时
刻装载保存的线程上下文后，线程可以继续执行；
p进程：包含多个线程的软件处理器，线程需要在进程的
上下文中执行。 

7
上下文切换
Ø
上下文
系统运行过程中的一系列状态，状态的含义因系统的不同而不同
p
处理器上下文
处理器用于运行一系列指令的保存在寄存器中的最小数据集合
（如：栈指针、地址寄存器、程序计数器）；
p
线程上下文
用于执行一系列指令的保存在寄存器和内存中的最小的数据集
合（如：处理器上下文、状态等）；
p
进程上下文
用于执行线程的保存在寄存器和内存中的最小的数据集合
 
（
线程上下文、
MMU
寄存器值、
TLB
）

8
上下文切换
Ø
观察发现
p
线程共享相同的地址空间。
 
线程上下文的切换
可以独立于操作系统；
p
一般来讲进程之间的切换要更复杂、代价更高，
因为需要陷入到
OS
内核才能完成；
p
创建和销毁线程的代价要远远小于对进程的创
建和销毁；

9
为什么要利用线程
主要的原因：
p避免不必要的阻塞： 单线程的进程在进行I/O操作
的时候会被阻塞； 在多线程的进程中，操作系统
可以将CPU切换到进程的另外一个线程；
p更好地发挥并行性：一个具有多线程的进程可以在
多核或者多处理器的CPU上并行执行；
p避免进程上下文切换：架构大型应用的时候不是利
用多个进程而是多个线程；

10
避免进程切换
Ø
避免昂贵的上下文切换
p
多个线程利用相同的地址空间：更容易出错；
p
在线程使用内存时，没有来自
OS/HW
的保护；
p
线程的上下文切换比进程的上下文切换要快得多？；
平衡

11
上下文切换的代价
考虑简单的时钟中断处理器：
p直接代价：用于实际切换和执行中断处理代码的时间；
p间接代价：其他代价，比较常见的是cache刷新的时间；
上下文切换导致的间接代价：
(a)在上下文切换之前；
(b)在上下文切换之后；
(c)在访问区块D之后；
代价高达 80% 

12
线程的实现
Ø
线程的实现方式
p往往以线程包的形式存在；
p完全在用户空间下创建线程库；
p由内核来掌管线程并进行调度；
Ø
用户级线程
p好处：上下文切换代价小；
p缺陷：线程阻塞-> 进程阻塞；

13
用户级别的线程

14
内核级别的线程

15
混合级别的线程

16
线程和操作系统
Ø
内核解决方案
基本的想法是在内核中实现软件包，这也就意味着所有
的操作变成系统调用；
p用于阻塞线程的操作就不再是问题了，内核会在同
一个进程中调度另外一个可用的线程；
p处理外部事件也变得简单了：内核（捕捉所有的事
件）直接调度与线程相关的事件；
p存在的问题是效率，因为每个线程操作都需要陷入
到内核；
Ø结论
尽可能融合用户级别和内核级别的线程，发挥各自的优势。
但是，事实上这种做法带来的性能提升也难以弥补其复杂性
带来的困难。

17
轻量级进程 (LWP)
Ø
基本思想
引入一种两层线程方法：轻量级的进程 (LWP) 能
够执行用户空间的线程；

18
轻量级进程 (LWP)
Ø
主要的操作
p用户级的线程执行系统调用 => LWP执行相应的操作，同
时该线程被挂起。线程维持绑定在该LWP上；
p内核调度负责调度另外一个LWP，该LWP绑定了一个活跃
线程。注意，这个线程可以切换到用户空间中的另外的活
跃线程；
p一个线程调用阻塞用户层的操作 => 上下文切换到一个运
行的线程，并绑定到相同的LWP；
p当没有线程调用时， LWP 保持空闲甚至有可能被内核清
除、回收；

19
在客户端使用多线程
Ø
多线程在分布式系统中的重要意义
Ø
多线程的
Web
客户端
隐藏了网络的延迟；
pWeb浏览器扫描到达的HTML页面，发现需要获取更多页面；
p每一个页面由一个特定的线程获取，每个线程执行HTTP请求；
p随着文件的到达，浏览器将这些文件展示出来；
Ø
服务器之间多个请求
-
响应调用
p客户端同时产生多个调用，每一个线程负责一个调用；
p之后，客户端等待结果返回
p注意： 如果调用的不同的服务器，将会得到线性加速；

20
多线程客户端是否有用？

21
多线程服务器
Ø
提高性能
p启动一个线程远比启动一个进程代价要小很多；
p单线程服务很难在多处理器系统中实现 scale-up（纵向扩展）；
p与多线程客户端相呼应：通过并行响应请求隐藏网络延迟；
Ø
更好的结构
p大部分服务器都具有较高的I/O需求，使用简单容易理解的阻塞调
用可以简化整体结构；
p多线程的程序的代码数量较少，比较容易理解，因为控制流被简
化了；

22
多线程服务器举例：文件服务器

23
虚拟化（Virtualization）
Ø
观察发现
虚拟化（
软硬件的多路复用
）非常重要：
p硬件比软件变化的快；
p需要灵活的可移植性和代码迁移；
p失效和攻击隔离

24
虚拟化（Virtualization）
Ø
主要原理：模拟接口

25
模拟接口
Ø
三个层次上的四种类型的接口
p
指令集架构：
一系列的机器指令，主要分为两类：
a. 特权指令：允许操作系统执行的指令；
b. 通用指令：可以被任何程序执行的指令；
p
系统调用
由操作系统提供的函数；
p
库函数调用，
也称为
应用程序接口（
API
）

26
虚拟化的不同方式
(a ) 进程虚拟机 (Process VM)、(b) 原生虚拟机监控器  (Native VM
M)、(c) 主机虚拟机监控器 (Hosted VMM)
区别：(a) 分离的指令集合，实际是运行在操作系统之上的解释器（J
VM）或者模拟器（Qemu）；（b）底层的指令，同时具有跑在硬件
上的最小的操作系统；（c）底层指令，但是需要一个完整的OS；

27
进一步了解虚拟机：性能
特权指令：
当且仅当在
用户模式下执行时，引
起操作系统陷入；
非特权指令：
其他
所有指令
特殊指令：
控制敏感性指令：可能影响到机器配置的指令（如：寄存器重定位
或者中断表）
行为敏感性指令：指令效果由上下文确定；

28
虚拟化的条件
Ø
采用虚拟化的必要条件
对于任何通用计算机，如果敏感指令集是特权指令的子
集，则可以构建虚拟机监控器；
Ø
问题：条件并不总是被满足
存在这样的敏感指令集，这些指令集在用户空间执行，
但是不会引起操作系统的陷入（POPF）；
Ø
解决方案：
p 模拟所有指令；
p包装非特权敏感指令，将其交给VMM控制执行；
p半虚拟化（Paravirtualization）：修改客户OS，要么阻止
非特权的敏感指令，或者将其变为非敏感指令（也就是改
变上下文）；

29
Docker
什么是容器？
•Standardized packa
ging for  software an
d dependencies
•Isolate apps from each 
other
•Share the same OS ker
nel
•Works for all maj
or Linux  distributi
ons
•Containers native to Wi
ndows  Server 2016

30
Docker
Ø镜像 ---镜像就相当于集装箱，docker 将所有app都标准化，需要
使用就拉取镜像。
Ø仓库---仓库就相当于超级码头，docker将所有镜像存储于仓库。
Ø容器---容器就是应用运行的地方，每个容器是独立的。这就体现
了它的隔离性；
Docker Image
Example: Ubuntu with Node.js and  
Application Code
Docker Container
Created by using an image. Runs  
your application.

31
Docker 
镜像
docker镜像是一系列文件，它起源于linux联合文件系统，通过分层
实现镜像文件的存储。

32
Docker VS VM
App 1App 2
Bins/LibsBins/Libs
Guest OSGuest OS
Hypervisor
Host Operating System
App 1
Bins/Libs
App 2
Bins/Libs
Docker Engine
Host Operating System
Virtual MachinesDocker Containers

33
Docker
Lightweight, open, secureplatform
•Simplify building, shipping, runninga
pps
•Runs natively on Linux or Windows S
erver
•Runs on Windows or Mac Develop
ment  machines (with a virtual mac
hine)
•Relies on "images" and "containers"
What Is Docker?

34
Docker的创建、发布、运行过程

35
Docker Image
The basis of a Docker container. Represents a full application
Docker Container
The standard unit in which the application service resides and e
xecutes
Docker Engine
Creates, ships and runs Docker containers deployable on a ph
ysical or  virtual, host locally, in a datacenter or cloud service pr
ovider
Registry Service (Docker Hub(Public) or Docker Trusted Re
gistry(Private))
Cloud or server based storage and distribution service for your i
mages
Docker相关的词汇

36
Docker相关的命令
$ docker image pull node:latest
$ docker image ls
$ docker container run –d –p 5000:5000 –-name node node:latest
$ docker container ps
$ docker container stop node(or <container id>)
$ docker container rm node (or <container id>)
$ docker image rmi (or <image id>)
$ docker build –t node:2.0 .
$ docker image push node:2.0
$ docker --help

37
•Instructions on  
how to build a  
Docker image
•Looks very similar  
to “native”  comma
nds
•Important to  
optimize your  
Dockerfile
Dockerfile

38
客户-服务器之间的交互
客户机器主要是让个人用户和远程服务器交互。主要包含两
种模式：1、对于每种远程服务，客户机都有一个独立的网络模
块联系这些服务；2、通过一个方便统一的用户接口来对远程服
务直接访问；

39
样例： X windows系统

40
提高X Windows系统性能
Ø
实际观察
p应用程序的逻辑和用户接口之间没有清晰的界限；
p应用程序与X内核之间的交互倾向于采用同步模式；
Ø
改进方法
p应用程序完全控制显示，可以到像素级别（VNC）；
p提供几种高层次的显示操作；
p压缩、增量传输；

41
客户端软件
客户端软件包含用于获得分布式透明性的组件
p访问透明性：客户端拥有用于 RPC访问的存根；
p位置/迁移的透明性：让客户端记录服务器的实际位置；
p副本透明性：客户端存根多个副本的调用
p故障透明性：经常放在客户端（用于屏蔽服务器和通
信问题）

42
服务器软件：常见设计问题
Ø
基本模型：
服务器是实现特定服务的进程，这些服务是
为一组客户提供的。本质上，每个服务器的组织
方式都一样，等待来自客户的请求，随后负责处
理该请求。 

43
服务器的组织方式
Ø
两种基本类型
p迭代服务器（Iterative server）： 服务器顺序处理到达
的请求； 
p并发服务器（Concurrent server）：利用分派器（disp
atcher），选择到达的请求并将它传递给分离的线程或
者进程处理；
Ø观察发现
并发服务器是一种常见的类型：他们能够处理多个请求，        
特别是针对某些阻塞性的操作；

44
服务器连接
静态端口配置：/proc/services动态连接查询：netstat-n

45
服务器连接
Ø
观察发现
: 
大多数服务都跟一个特定的端口绑定在一起
Ø
动态分配一个端口
两者的区别

46
带外通信（out of band communication）
Ø
问题
在服务器已经接收了服务器请求后，是否有可能中断服务器的运行？
Ø
解决方案
1
：
 
利用一个分开的端口用于紧急数据通信
p服务器拥有一个分离的线程或者进程用于紧急通信；
p紧急信息到达  =>   相关联的请求暂时挂起；
p注意：我们需要OS 支持基于优先级的调度策略；
Ø
解决方案
2
：
 
利用传输层的机制
p样例：TCP 允许在同一个连接中传输紧急信息；
p利用OS的信号机制捕捉紧急信息；

47
服务器和状态
Ø无状态服务器（Stateless ser vers）
在处理完请求后，从来不保存关于客户端的精确的信息；
p不记录一个文件是否被打开（文件请求完成就关闭）；
p不保证清空客户端的cache；
p不去追踪客户的信息；
Ø结果
p客户端和服务器完全独立；
p由于客户端或者服务器的宕机导致的状态不一致减少；
p由于服务器不能预测客户端的行为，可能导致性能下降；

48
服务器和状态
Ø有状态服务器（Statefulservers）
记录客户端的状态信息：
p记录客户端打开的文件，这样可以提前实现预取；
p知道客户端缓存了哪些数据，允许客户端在本地保存
共享数据的备份；
Ø观察发现
p有状态的服务器的性能非常高；
p可靠性问题是一个主要的问题；

49
服务器集群
Ø
常见的组织结构
Ø
关键单元
p第一层主要用于请求分发 （掩盖分布式系统的分布式特性）

50
请求处理
Ø
观察
p让第一层处理所有的出入集群的通信会导致性能瓶颈；
Ø
观察
服务器选择算法

51
服务器集群
前端容易出现过载的情况：需要一种新的度量方法
p基于传输层交换方法：前端简单地将TCP请求传递到服务器，只
是会考虑某些简单的性能指标如CPU利用率等；
p基于内容可感知的分派方法：前端会读取请求的内容，然后选择
最合适的服务器；
将基于传输层交换和基于内容可感知的分派方法相结合

52
当服务器扩展到整个互联网
Ø
观察发现
将服务器集群扩展到互联网规模可能会引入管理问题。可以通过
采用单个云计算商提供的数据中心避免这些问题。
Ø
请求分派：如果局部性比较重要的话
常用的方法是利用DNS:
p客户端通过DNS查询特定的服务-客户端的IP地址是请求的一部分；
pDNS服务器记录请求服务的副本服务器信息，并且返回大部分本
地服务的地址；
Ø
客户透明性
为了保障透明性，DNS resolver会代替客户端，问题是有可能距离远。

53
分布式服务器访问的稳定性
分布式服务器的基本思想是构造一个可靠的、高性能的、稳定的服务
器即实现一个稳定的访问点。
Ø具有稳定IPv6地址的分布式服务器

54
分布式服务器：处理细节
Ø
本质：客户端具有
Mobile IPV6
能够透明地跟任意其他节点建立
连接
p客户端 C 通过IPv6 与宿主网络 HA 建立连接；
pHA 被一个宿主代理维护着，HA 可以将连接传递给注册的需要地
址 CA； 
p C 可以应用路由优化方法，通过将网络包直接转发到地址 CA 
（不用传递给宿主agent）；
Ø
协作分布式系统
p原始的服务器维护一个宿主地址，但是会将连接信息传递给协作
节点；
p原始节点和协作节点看起来是一个服务器；

55
管理服务器：PlanetLab
Ø
本质
p不同的组织贡献机器， 这些机器被共享用于进行各种实验； 
Ø
本质
p我们需要保证不同的分布式应用相互不影响 => 虚拟化 

56
PlanetLab基本的组织结构
Ø
总览
ØVserver
p具有自己的软件库、服务器版本等的独立受保护环境。分布式应
用会分发到多个这样的机器上。

57
代码迁移
Ø
负载分布
p确保数据中心中的服务器的复杂运行更加充分（防止浪费能源）
p让计算更加贴近数据端，最小化通信代价（例如移动计算）
Ø
灵活性：当需要的时候将代码迁移到客户端

58
代码迁移的模型

59
代码迁移的模型

60
强/弱可移动性
Ø
对象组件
p代码片段：包含实际执行的代码；
p数据片段：包含状态
p执行状态：包含线程执行对象代码的上下文；
Ø
弱移动性：仅仅移动代码和数据片段（重启执行）
p相对简单，特别是如果代码是可移植的；
p需要区分两种模式：代码推送（Push）和代码拉取（Pull）
Ø
强移动性：移动组件，包括执行状态
p迁移（Migration）：将整个对象从一个机器移动到另外一个机器；
p克隆（Cloning）：开始克隆，将其设置为相同的执行状态；

61
异构系统中的迁移
Ø
主要问题
p目标机器可能不适合执行迁移后的代码；
p进程/线程/处理器的上下文的定义比较依赖于硬件、操作
系统和运行时系统；
Ø
仅有的解决方案：在不同的平台上抽象机器的实现
p解释型语言，拥有自己的VM；
p虚拟机监控器；

62
迁移虚拟机
Ø
迁移镜像：三个不同的方案
p将内存页面推送到新的机器，在迁移过程中重新发送被修
改过的页面；
p停止当前的虚拟机；迁移内存，然后重新启动虚拟机；
p让新的虚拟机按需拉取内存页面：在新的虚拟机上立即创
建进程，并且按需拷贝内存页面；

63
虚拟机迁移的性能
Ø
问题
Ø
在虚拟机迁移过程中虚拟机响应时间的度量
一次完整的虚拟机迁移可能需要几十秒。在迁移期间，服务
可能处于几秒钟的完全不可用的状态。

64
CRIU
在Linux操作系统上实现 checkpoint/Recovery
üUsing this tool, you can freeze a running application (or part of it) an
d checkpoint it to a hard drive as a collection of files. 
üYou can then use the files to restore and run the application 
from the point it was frozen at.
üThe distinctive feature of the CRIU project is that it is mainly 
implemented in user space. There are some more projects d
oing C/R for Linux, and so far CRIUappears to bethe most f
eature-rich and up-to-date with the kernel.
https://github.com/checkpoint-restore/criu
Demo
