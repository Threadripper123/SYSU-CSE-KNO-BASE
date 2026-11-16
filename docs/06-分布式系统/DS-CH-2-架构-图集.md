# DS-CH-2-架构-图集

> 主题库：分布式系统　｜　源文件：DS-CH-2-架构-图集.pdf　｜　4,770 字符　｜　41 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

Closed and Centralized Open and Open and Decentralized Cloud IT Infrastructure Centralized Cloud and Fog Infrastructure

Layer N LayeR N-1 Request ↓  ↑ Response fOW fOw Layer 2 T Layer1 (a)

Object Object Object Method call abject ObjeCt (b)

Component Component Publish (a)

Component Component Data delivery Publish Shared (persistent) data space

cYWaaan fer resul Client requests A responses request response Server provide service time >

Request/Response downcall LayerN Layer N-1 Layer2 Layer1

One-way call Layer N Layer N-1 Layer N-2 Layer N-3

Layer N Layer N-1 Handle Upcall Layer N-2

PartyA PartyB Layer N Layer N Interface Service Layer N-1 Layer N-1

sOcket 2 S = SOcket(AF_  _INET, SOCK_STREAM) 3 (conn, addr) = s.accept() # returns new socket and addr. client 4  True: # forever 5 data = conn.recv(l024) # receive data from client 6  data:  # stop if client stopped 7 conn.send(( (data)+ ) # return sent data plus an n +' '" 8 conn.close() # close the connection

1 sOcket 2 S = SOCket(AF_ _INET, SOCK_ _STREAM) 3 s.connect( :((HosT, PoRT)) # connect  to server (block until accepted) 4 s.send( Hello, world) # send some data 5 data = s.recv(lo24) # receive the: response 6  data # print the result 7 s.close() # close the connection

[mb m层含列表的关键字表达式 HTML页面 HTML 生成器查询处理层已排名的排名算法数据库查询含元信息的 Web页面标题含Web页面的数据层数据库图2.4 因特网搜索引擎简化成三个不同的层

Object Object ObjECt Method call ObjECt ObjECt

State Method Interface

Basic operations aperation Description Pui Create anewresource GET Retrieve the: state of a resource in some representation DELETE Delete al resOurCe PosT Modify aresource by transferring anew state

Amazon Sa SOAPinterface Bucket operations Object operations ListAllMyBuckets PutObjectInline DeleteBucket CopyObject aastBucket GetObject GetBucketAccessControlPolicy GetObjectExtended YetbucketAccessControlPolicy DeleteObject GetBucketLoggingStatus GetObjectAccessControlPolicy SetBucketLoggingStatus SetObjectAccessControlPolicy

Temeorally Temeorally cYaupled ecoupld Referentially Direct M ailbox Referentially Event- Shared decoupled based data space

Component Component Component Component Subscribei Notification delivery Publish Subscribe: Data delivery Publish Comonent Shared(persistenndataspace

客户机用户接口用户接口用户接口用户接口用户接口 [应用程_ 应用程应用程序数据库 [用数据库数据库数据库数据库数据库 [ab器 (a) (b) (c) (d) (e) 图2.5 各种客户-服务器组织结构(a)～（e）

[Tba e 用户接口（表示） 请求操作「 返回结果 [Tba 应用程序服务器-- 请求数据 /返回数据数据库服务器---- 时间

internet Cghg lmgml Applaafign Database(s)

0000 0001 1001 [1000 0010 0011: . 1011! 1010 0100 0101 1101 0110 0111 1111 1110

实际结点 14 {13,14,15} {0,1}  {8,9,10,11,12} {2,3,4} [相关数据键值 {5,,

30 31- -0 ① 2 29 (3) Actual node  Shortcut ④ 2 5) 26 0 Nonexisting 25 7 Yde 9 keys{5,6,7,8,9} 22 10 ② 20 19 13  17 16 15  lookup(3)@9: 28 → 1 → 4

与 (0.6,0.7)处的结点 (0,1) 相关的键值 (1,1) (0.9.0.9) (0.9.0.9) (0.2,0.8) (0.2,0.8) (0.6,0.7) (0.6,0.7) 实际结点 . (0.9,0.6)  (0.9,0.6) (0.2,0.45) (0.2,0.3) (0.7,0.2) (0.7,0.2) (0.2,0.15) (0,0) (1,0) (a) (b)

主动线程的动作（周期性重复〕： 从当前的部分视图中选择一个对等体P if PUSH_MODE{ mybuffer[MyAddress moveHoldestentriestothe end append first c/2 entries to mybuffer; sendmybuf eroP ElSE send trigger to P; 从当前视图和P的缓冲区构建一个新的部分视图： 增加新的部分视图中每个项自的年龄 (a)

被动线程的动作： [Tbb 意程O _收 YfULL_MODE mybuffer-[(MyAddress,0)] permute partial view; move H oldest entries to the end; append first c/2 entries to mybuffer; 从当前视图和P的缓冲区构建一个新的部分视图： 增加新的部分视图中每个项目的年龄 (b)

结构化覆盖特定覆盖网络到其他特定拓朴结构的网络的协议结点的链接随机对等体随机的盖网络随机视图协议结点的链接

Time

Regular peer Superpeer Superpeer network

Client Content provider iSP iSP

客户结点结点1 Lookup(F） BitTorrent F的.torent 存储了F的结点2 Web页面指向文件文件指向跟踪结点列表服务器的器的引用 Web服务器引用文件服务器跟踪器 ）结点N

客户应用程序被中断调用 B.do_something(value) [Tbm_l_ 请求级中断器无中断调用 invoke(B, &do_something,value) 对象中间件． 消息级中断器 send([B, "do_something", value]) [T本be 操 se [u对B

不可控参数（干扰/噪声） 初始配置纠正观测输出分布式系统的核心调整方法参考输人尺度预测分析已测量输出 l整触发器

avg_load avg_mem avg_procs 0.06 0.55 47 机器A 机器B 机器（ C IP-addr load mem procs 192.168.1.2 0.03 0.08 43 192.168.1.3 0.05 0.50 20 192.168.1.4 0.10 0.35 78

Resource Usage Memory CPU Disk Goroutines 238 MiB 0.20- 7.2MiB 290 191MiB 0.15 6.7MiB 285 143MiB 6. MiB 280 0.10 95 MiB 5.7MiB 275 48 MiB 0.05 5.2MiB- 270 4.8MiB 0B 0 265 17:06 17:07 17:08 17:09 17:10 17:06 17:07 17:08 17:09 17:10 4.3MiB 17:06 17:07 17:08 17:09 Virtual Mem 0ry Resident Memory Alloc - Heap inHuse - Total (k8s) - discovery (k8s) - istio-proxy (k8s) 17:10 260  Stack in-use Total(k8s) - discovery (k8s) = istio-proxy (k8s) - pilot (self-reported) - discovery - istio-proxy 17:06 17:07 17:08 17:09 17:10 xDS 0.5 ops - 1.0 ops 1.3– 0.4ops - 1.2- 0.5ops - 0.3 ops - 1.1- 0.2 ops 00ps- 1.0- 0.9 0.1ops - -0.5ops 0.8- 0 ops -1.0ops 0.7 17:06 17:07 17:08 17:09 17:10 17:06 17:07 17:08 17:09 17:10 17:06 17:07 17:08 17:09 17:10
