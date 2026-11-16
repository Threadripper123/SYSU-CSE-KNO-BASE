# DS-CH-4-通信-图集

> 主题库：分布式系统　｜　源文件：DS-CH-4-通信-图集.pdf　｜　11,758 字符　｜　56 个片段　｜　更新于 2026-11-11
天河无用二号关用二号

80

中算院件院 SENU SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Holp

Application protocol Application Presentation protocol Presentation 6 Session protocol Session 5 Transport protocol Transport 4 Network protocol Network 3 Data link protocol Data link Physical protocol

国网 m0m.niplc.cem B 5ngwen0218 6e.28130620180243011140

数据链路层报头网络层报头一传输层报头一会话层报头 -表示层报头一应用层报头 [消息数据层报 [实b际出在网络h的图4.2 在网络上传输的典型消息

应用层传输层互联网层网络访问层 Ba百码

Transport Services Protocol Support Socket& HTTP& Google SSL.StartTLS Datagram WebSocket Protobuf zlib/gzip Large File HTTP Tunnel RTSP Legacy Text. Binary Protocols In-VM Pipe with Unit Testability Extensible Event Model Universal Communication APl   Zero-Copy-Capable Rich Byte Buffer

应用协议应用层 6 中间件协议中间件层 5 传输协议传输层网络协议网络层 3 数据路层物理协议物理层网络图4.3 调整后的网络通信参考模型

Application Middleware Operating Host-to-host protocol Physical/Link-level protocol Hardware

在请求子任务时在请求传送时在服务器处理完后同步化同步化同步化客户请求传输中断 [Tab 应答服务器时间 I

ActiveMQ

[Tbaee 客户 [ab用程 hl用 o 请求「 应答服务器--- 调用本地过程时间 [ab_ e 图4.6客户与服务器之间的RPC的原理

Client machine Server machine Clientprocess Serverprocess 1.Clennalll procedure Implementation 6.Stubmakes afdoit local call to "doit r = doit(a,b) r = doit(a,b) Client stub proc:"doi proc:"doit 5.Stub unpacks type1: val(a) 2.Stub builds type1: val(a) type2:val(b) message message type2: val(b) proc:"doi 4.ServerOs Client Os type1: val(a) Server OS hands message [Yype2: val(b to server stub 3.Message is sent across the network Client procedure calls client stub. 6 Server does local call; returns result to stub. Stubbuildsmessage callslocalOs Stubbuildsmessage callsOs 3 0ssendsmessagetoremote s. 8 0ssendsmessagetoclientsOs. Stub OS gives message to stub. 10 9 ⑤ 4 ent''s OS gives message to stub. Client stub unpacks parameters; calls Remote Cli unpacks result; returns to client. server.

Wainfor Callbackto client Client acceptance Call remote Return procedure from call Return cYann ACCEPT Reauest reaues Server Call local procedure Time

Server Call localprocedure Callbackstoclient Client Callremote procedures Server Call local procedure Time

DATA DATA code code  code XML HTTP XML code  code code XML-RPC Source: JY Stervinou

PTOTORESPOSE gRPC Server Ruby Client Proto eg C++ Service gRPC Proto Respose( Stub Android-Java Client

Transport Services Protocol Support Socket & HTTP & Google SSL.StartTLs Datagram WebSocket Protobuf zlib/gzip Large File HTTP Tunnel RTSP In-Vm Pipe Legacy Text. Binary Protocols with Unit Testability Extensible Event Model UniversalCommunicationAPl Zero-Copy-Capable Rich Byte Buffer

Uuidgen Interface defnition fi le IDL compiler Client code Client stub Header Server stub SerVer code #include #include C compiler C compiler C compiler C compiler Client Client stub SerVer stub Server object file object file object fi le object file Linker Runtime Runtime  library library  ) Linker binary binary

#include "rpc test.h" teSt 1 *argp, CLIENT *clnt)  *clntres; rmernset(( *)&clnt_res, o sizeof clntres)); if (clntcall (clnt, TEST, (xdrproct) xdrwrapstring, (caddr t) argp, (xdrproct) xdr wrapstring, (caddrt) &clntres TIMEOUT) !=RPC_SUCCESS) { return (NuLL); return (&clntres);

#ifndef  RPC TESTHRPCGEN #define  RPCTESTHRPCGEN #include <rpc/rpc.h> #endit #define VERSION #ifdefine( STDC )1ldefined( Cplusplus) testprog1 reeresult svCxpRT xdrproc t, Caddr t) #define TEST 1 test1svc(); [aba _

#include "rpc test.h" #include <stdio .h> #include <stdlib.h> #include /rpc/ prmap clnt.hy #include <string.h> #include <rmemory.h> #include <sys/socket.h> #include <netinet/in.h> #ifndef SIG PF #define SIGPF #endif testprog 1( svc req*rqstp, svcxprt *transp)  *test arg, } argurnent; xdrproct xdr argurnent, xdr result; switch (rqstp->rq proc) { case NuLLPROC: ( ) svc Sendreply (transp, (xdrproc t) xdrvoid, ( *)NuLL) ; return; case TEST: xdr argurnent = (xdrproct) xdr wrapstring; xdr result= (xdrproc t)  xdrwrapstring; 1ocal svcreq*))test_1svc; break;

#include "rpc test .h" testprog_1( *host) CLIENT *clnt; testl arg; #ifndef DEBUG clnt = clnt ( create (host, TEsTPRoG, vERsIoN, udp" ) if {clnt == NULL) clnt pcreateerror (host); exit (1); result1 = test 1(&test1_arg,( clnt); if (reSult1 == NULL) clnt_perror (clnt, "callfailed"); #ifndef DEBUG clnt destroy (clnt); [Ya _e if (argc printf ("usage: s server_host\n", argv[o); exit (1):

1n 1ud C S test svc **argp, Svcreg *rgstp eturn &result

cGaal https://opensource.google.com/

message xamplel{ aationalstringstringVal=; tptional ytesbytesval=2; message EmbeddedMessage { int32 int32Val=1; string stringVal= 2; optional B EmbeddedMessage embeddedExamplel = 3;  aepeated int32repeatedInt32Val=4; aeeeatedstringrepeatedStringval = ; 子

ProtoF Request gRPC Stub canOn Proto Ruby Client Proto egu C++Service gRPC ProtoA Stub Response(s)  Android-Java Client

目录机器目标服务器 2.注册服务 3.查询服务器服务器客户机 5.RPC调用服务器 1.注册端点客户 4.请求端点 DCE 守护程序端点列表图4.13 DCE中的客户-服务器绑定过程

服务器 socket bind listen accept read write close 同步点- 通信一  ，      socket connect write read close 客图4.15 使用套接字的面向连接通信模式

Server 1  socket  * 2 s = sOcket(AF_ _INET, SOCK_STREAM) 3 s.bind((HOST, PORT)) 4 s.listen(l) 5 (conn, addr) = s.accept() # returns new socket and addr. client 6  True: # forever 7 data = conn.recv(l024) # receive data from client 8  data:  # stop if client stopped 9 conn.send( (data)+ ) # return sent data plus an 10 conn.close() # close the connection 1 socket  * 2 S = SOcket(AF_INET, SOCK_STREAM) 3 s.connect((HosT, PORT)) # connect to SerVer (block until accepted) 4 s.send( Hello world) # send same data 5 data = s.recv(lo24) # receive the response 6  data # print the result 7 s.close() # close the connection

1 Zmq 2b Context ZmaeContext() 4 pl = "tcp://+ HOsT ++ PORTl # how  and where to Connect 5 p2 = "tcp /+ HosT ++ PORT2 # how and where to connect 6 s = context.socket1 (zmq.REP) # create reply socket 8 s.bind(pl) # bind socket to address 9 s.bind(p2) # bind socket to address 10  True: 11 message = s.recv() # wait for incoming message 12  "sTop  message: # if not to stop. 13 s.sendi l(message + ) # append "" to message 14 else. 15  # break out of loop and end

2 context = zmq.Context() 4 php = "tcp://"+ HOsT ++ PORT # # how and where to connect 5 s  context.socket(zmq.REQ) Create socket s.connect (php)   # block until  connected 8 s.send(Hello World) send message 9 message = s.recv() block until response  10 s.send(sTop) # tell server to  stop 11  message #ì print result

Server 1 zmg time 2 3 context zmq.Context() 4 s I context.s .socket(zmq.PuB) # create a publisher socket 5 p = "tcp://+ HOsT ++ PoRT # how and where to communicate 6 s.bind(p) # bind socket  to the address 7 True: 8 time.sleep(5) # wait every 5 seconds 9 s.send(TM  + time.asctime()) # publish the current time Client 2 3 context = zmq.Context() 4 s I context.socket1 (zmq.SuB) # create a subscriber socket 5 p = "tcp://+ HOsT ++ PORT # how and where to communicate 6 s.connect1 (p) # connect to the server 7 s.setsockopt(zmq.SUBsCRIBE, "TME) # subscribe to TIME messages 9 i (5): # Five  iterations 10 time = s.recv() # receive a message

Source 1 zmq, time,l pickle, sys, random  3 context=zmq.Context( 4 me = (sys.argv[1]) 5 s = context.socket(zmq.PusH) # create a push socket 6 src = SRCl me == 1  SRC2 # check task source host 7 prt = PORT1  me == 1 PORT2 # check task  source port 8 p = "tcp: /+ . src ++ prt # how and where to connect 9 s.bind(p) # bind socket to address L1 i (l0o): # generate l00workloads 12 workload = random.randint(l, 100) # compute workload 13 s.send(pickle.dumps((me, workload))) # send workload to worker

Worker 1 zmq time, pickle, sys 3 context = zmq.Context () 4 me = (sys.argv[1]) 5 r = context.socket(zmq.PuLL) # create a pull socket 6 pl = "tcp://+ SRCl +:"+ PoRT1 # address first task source 7 p2 = "tcp://+ SRc2 +:"+ PoRT2 #  address second task source 8 r.connect(pl) # connect to task source 1 9 r.connect(p2) # connect to task source 2 1 True: 12 work = pickle.loads(r.recv()) # receive work from a source 13 time.sleep(work[1]*0.0l) # pretend to work

MPI_bsend Append outgoing message to a local send buffer MPI_send Send a message and wait until copied to local or Yemote bufer MPI_ssend Send a message and wait until transmission starts MPI_sendrecv Send a message and wait for reply MPI_isend Pass reference to outgoing message, and continue MPI_issend Pass reference to outgoing message, and wait until Yeceipt snarte MPI_recv Receive a message; block if there is none MPI_irecv Check if there is an incoming message, but do not bloCk

put Appenda message to a specified queue get Block until the   specified queue is nonempty, and rem ove the firstmessage poll Check a  specified  queue for messages, and remove the first. Neverblock notify Install : a handler to be called when a message is put into the: specified 1 queue

发送方主动发送方主动发送方被动发送方被动接收方主动接收方被动接收方主动接收方被动 (a) {b) (c) (d) 图4.17  使用队列的松散耦合通信的4种组合方式

Lookup Source queue contact address Destination queue manager ofdestination manager Yueuemanager ogical queue-level address (name) LocalOS Addresslookup ocalOS database Network address

发送方A 应用程序应用程序接收队列 111 R2 [TYe_ 发送队列 ： 应用程序 R1 I 接收方B 应用程序路由器图4.20 含有路由器的消息队列系统的一般组织结构

Source Message broker Destination Application Application Broker plugins Rules Interface _nterface Queuing layer 三 LocalOS LocalOS LocalOS

发送客户路由表发送队列客户的接收队列接收客户程序队列管理器队列管理器程序 MQ Interface 服务器存根服务器存根 MCA MCA MCA MCA 存根存根 RPC 本网络 （同步的） 企业网消息传输到其他远程 （异步的） 队列管理器图4,22 IBM消息队列系统的一般组织结构

属性描述 transporttype(传输类型） 决定要采用的传输协议 FIFOdelivery（先进先出传输） 表明消息将按照发送的次序到达 messagelength(消息长度） 单个消息的最大长度 setupretrycount（建立连接的重试次数） 指定启动远程MCA的最大重试次数 deliveryretries（消息交付重试次数） MCA将收到的消息放入队列的最大重试次数图4.23 与消息通道代理相关的一些属性

别名表路由表 LA1QMC QMB SQI 别名表路由表 LA2QMD QMC SQ1 LA1QMA] QMASQI QMDSQ2 A2QMD QMC SQI QMD Q1 SQ2 SQ1 QMA SQ1 QMB 路由表 SQ1 QMC 路由表 QMASQI QMASQ1 QMC SQ2 SQ2 1 QMBSQI QMB SQ1 别名表 QMDSQ1 LA1QMA  SQ1 QMD LA2QMC 图4.24   使用了路由表和别名表的MQ队列网络的常见组织结构

原语描述 MQopen 开启一个队列（可能位于远程） MQclosc ；· 关闭个队列 MQput 将消息放入开启的队列中 MQget 从队列（队列可以在本地）中获取消息图4.25 消息队列接口中的原语

从源地出来的数据包1 2345678  到达缓冲区的数据包 8 从缓冲区移出的数据包 6 8 回放间隔 0 5 10 15 20 时间(秒) 图4.27 使用缓冲区来减少抖动

（1）数据传输所要求的比特率。 （2）创建会话的最大延时（例如，应用程序何时可以开始发送数据）。 （3）端到端的最大延时（例如，数据单元到达接收端花费多少时间）。 （4）最延时动 (5)最大返延时。

30 31- 0 2 29 (3) Actual node Shortcut 4 21 5 26 6 Nonexisting node 25 7  8 23 9 Node responsible for keys {5,6,7,8,9} 2   10 20 12

aasicapproach Ynntiator generatesamulticast dentifiermid 2 Lookup succ(mid), the !node responsible for mid. Request is ; routed to succ(mid), which will becomethe root. P wants 4 If ''s to join, it sends a join I re quest too the root. When request arrives at Q:  Q has not seen a join request before → it becomes forwarder; P becomes child of Q. Join request continues to be forwarded. • Q knows about tree  → P  becomes child of Q. No needto forward join request el anymore.

Principle operations ● A node P selects another node Q from the system at random. O Pul: P only 【 pulls in new updates from Q Pus Ponly pushes itsownupdatesto Q Push-pulPand Q send updates to eachother bservation For push-pull it takes O(log(N)) rounds to disseminate updates to all N nodes  (round = when every node has taken the initiative to start an exchange).

digest of A push A digest of A-B B 一 update A-B digest of A pull A B update B-A digest of A push-pull A B update B-A, digest A-B

.0 =0 08 0.6 1 push N PU 0.2 0 5 10 15 20 25 Round

Basicmodel A server S having an update to report, contacts other servers. If a server is contacted to which the update has already propagated, S stops contacting other servers with probabiity Pstop. Observation If s is the fraction of ignorant servers (i.e., which are unaware of the update), it S=e−(1/Pstop+1)(1−s)

中山大计算机学院(软件学院) NUNS SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTER SCIENCEAND ENGINEERING E 大纲背景知识远程过程调用 3 面向消息的通信面向流的通信 5 多播通信
