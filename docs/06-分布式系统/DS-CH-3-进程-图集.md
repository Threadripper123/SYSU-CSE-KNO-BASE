# DS-CH-3-进程-图集

> 主题库：分布式系统　｜　源文件：DS-CH-3-进程-图集.pdf　｜　9,481 字符　｜　39 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

Thread #1 Thread #2

Process A Process B S1: Switch from user space Ya kernelspace S3: Switch from kernel space to user space Yperatingsystem S2: Switch context from process A to process B

MRU A D B   B LRU (a) (b) (c)

用户空间库调度器内核空间用户操作系统调度器线程1 进程A 用户线程2 内核线程1 用户内核0 线程3 内核1 内核线程2 KT1 KT2 用户运行运行线程4 内核进程B 线程3 用户线程5  进程C 用户线程6

用户空间库调度器内核空间用户操作系统调度器线程1 内核线程1 进程A 用户线程2 内核用户线程2 线程3 内核内核0 内核1 线程3 KT2 KT3 用户线程4 内核运行运行进程B 线程4 用户线程5 内核线程5 进程C 用户内核线程6  线程6

用户空间库调度器用户操作系统调度器线程1 线程池用户进程A 线程2 内核线程1 用户线程3  内核线程2 用户线程1 内核0 内核1 户内核线程2 线程3 KT2 KT4  运运进程B 用户线程3 内核用户线程4 线程4 用户内核线程5  线程5

Userspace Thread Lightweight process ernelspace LWP executing a thread

Thread-level parallelism: TLP Let C; denote the fraction of tim e that exactly i threads are being executed ninmultaneously ENTICI TP 1—CO with N the maximum number of threads that (can) execute at the same time. Practical measurements A typical Web browser has a TLP value between 1.5 and 2.5 threads are primarilyused forlgicallyorganizing browsers.

Dispatcher thread to a worker thread Server Worker thread Request comingin [fram henetwork aperating system

模型特征多线程并行，使用会导致阻塞的系统调用单线程进程非并行，使用会导致阻塞的系统调用有限状态机并行，使用非阻塞系统调用图3.41 构建服务器的3种方式

Program Program lmplementationof mimicking Aon B Hardware/software system A Hardware/software system B

Application/Libraries Application/Libraries Application/Libraries Operating: |system Runtime system Operating system Virtual rI m achine monitor Operating system Virtual machine monitor Operating system Hardware Hardware Hardware (a) (b) (C)

application/Libraries Privileged Host operating system General inetructions instructions Hardware

CONTAINER CONTAINER CONTAINER Java NET StaticBnary Debian Ubuntu Alpine

cY wb

wrielle Containee 0lad references Timage parent

Developers IT Operatlcns

业 Dockerfile 1 # Create image based on the official Node 6 image from dockerhub FROM node:latest 3 4 # Create a directory where our app will be placed S RUNmkdir -p/usr/src/app 6 #  Change directory so that our commands run inside this new directory 8 WORKDIR /usr/src/app 9 e #Copydependencydefinitions 11 CopY package-json /usr/src/app 12 13 # Install dependecies 14 RUN npminstall 15 16 # Get all the code needed to run the app 17 COPY ./usr/src/app 18 19 # Expose the port the app runs in 20 EXP0SE4200 21 22 # Serve the app 23 CMD "r npm " "start

客户机服务机客户机服务机 [应用程用程用程用程 [特定应用t 与用中间件中间件中间件无关的协议中间件 [本be操系e 本e操 e 本ee 本ee 网络网络 (a) (b) 图 3.8 （a）带自用协议的网络连接的应用程序；（b）允许访问远程应用程序的通用解决方案

应用程序服务器应用程序服务器用户终端窗口管理器应用程序 Xlib接口 Xlib Xlib 本地操作系统本地操作系统 X协议 X内核设备驱动程终端（包括显示器、 键盘、鼠标等） 图3.9 XWindow系统基本组织结构

Client machine Server 1 Server 2 Server 3 Client Server Server Server appl appl appl appl Client side handles requestI replication Replicated request

EtherNet/IP-2 44818/tcp EtherNet-IP-2 # EtherNet/IP messaging EtherNet/IP-2 44818/udp EtherNet-IP-2 # EtherNet/IP nessaging m3da 449oo/tcp # M3DA (efficient mnachine-to-machine con rn3da-disc 449oo/udp # M3DA Discovery (efficient rnachine-to-m achine cornunication) asrp 45ooo/tcp # NSi AutoStore Status Monitoring Protoc ol data transfer aSrp-ron 45ooo/udp # NSi AutoStore Status Monitoring Protoc ol device monitoring asrps 45oo1/tcp # Nsi AutoStore Status Monitoring Protoc olsecure data transfer Synctest 45045/tcp #Remote application control invision-ag 45054/tcp # InVision AG invision-ag 45054/udp # InVisionAG eba 45678/tCp # EBA PRISE eba 45678/udp #EBA PRISE dai-shell 45824/tcp # Server for the DAI family of client-se rver products qdb2service 45825/tcp # Qpuncture Data Access Service qdb2service 45e25/udp Qpuncture Data Access Service Ssr−serverrngr 45966/tcp # SSRServerMgr 3sr-serverrmgr 45966/udp # SSRServerMgr sp-remotetablet 4699e/tcp # connection between corputer and a  Slgn ature tablet rnediabox 46999/tcp # MediaBox Server rnediabox 46999/udp # MediaBox Server rnbus 47000/tcp # Message Bus rnbus 470oo/udp # MessageBus winrm 47oo1/tcp # Windows Remote Management Service jvl-nactalk 47ioo/udp Configurationofrnotors conneced to ln dustrial ethernet dbbrowse 47557/tcp #Databeam Corporation dbbrowse 47557/udp # Databeam Corporation directplaysrvr 47624/tcp # Direct Play Server directplaysrvr 47624/udp # Direct Play Server ap 47806/tcp #ALCProtocol ap 47806/udp #ALCProtocol bacnet 47eoe/tcp # Building Automation and Control Networ bacnet 47eos/udp # Building Autoration and Control Networ nimcontroller 4eooo/tcp #Nimbus Controller nirncontroller 4eooo/udp # Nirmbus Controller nirspooler 48oo1/tcp # Nimbus Spooler nirnspooler 48001/udp # Nirbus Spooler nirnhub 48002/udp # Nirnbus Hub nimngtw 48003/tcp # Nirmbus Gateway nirngtw 48003/udp # Nirbus Gateway

tcP 0 0127.0.0.1:43984 127.0.0.1:2379 ESTZBLISHED tcp 0 0127.0.0.1:10252 127.0.0.1:57092 TIMEWAIT tcp 0 0127.0.0.1:2379 127.0.0.1:43908 ESTABLISHED tcp 0 0222.200.180.178:40316 222.200.180.178:9999 ESTABLISHED tcp 0 0222.200.180.178:39780 222.200.180.178:6443 ESTABLISHED tcp 0 0222.200.180.178:60560 222.200.180.178:9090 ESTABLISHED tCp 0 0222.200.180.178:9090 222.200.180.178:60564 ESTABLISHED tcp 0 0127.0.0.1:2379 127.0.0.1:45296 TIMEWAIT tCp 0 0127.0.0.1:43992 127.0.0.1:2379 ESTABLISHED tcp 0 0127.0.0.1:43918 127.0.0.1:2379 ESTABLISHED tcp 0 0222.200.180.178:39876 222.200.180.178:6443 ESTABLISHED tCp 0 0222.200.180.178:39920 222.200.180.178:6443 ESTABLISHED tcp 0 0127.0.0.1:2379 127.0.0.1:43938 ESTABLISHED tCp 0 0222.200.180.178:39802 222.200.180.178:6443 ESTABLISHED tcp 0 0222.200.180.178:36878 222.200.180.178:3306 ESTABLISHED tcp 0 0222.200.180.178:9090 222.200.180.178:60556 ESTABLISHED tcp 0 0127.0.0.1:43946 127.0.0.1:2379 ESTABLISHED tcp 0 0127.0.0.1:2379 127.0.0.1:43898 ESTABLISHED tCP 0 010.96.0.1:59266 10.96.0.9:8081 TIMEWAIT tcp 0 0127.0.0.1:2379 127.0.0.1:43880 ESTABLISHED tcp 0 0127.0.0.1:10251 127.0.0.1:60262 TIMEWAIT tCp 0 0127.0.0.1:43948 127.0.0.1:2379 ESTABLISHED tcp 0 0222.200.180.178:60556 222.200.180.178:9090 ESTABLISHED tCP 0 0 222.200.180.178:35376 10.96.0.1:443 ESTABLISHED tcp 0 0127.0.0.1:2379 127.0.0.1:43888 ESTABLISHED tcp 0 0127.0.0.1:2379 127.0.0.1:43946 ESTABLISHED tCP 0 0222.200.180.178:60564 222.200.180.178:9090 ESTABLISHED tcp 0 0127.0.0.1:43990 127.0.0.1:2379 ESTABLISHED tCP 0 0127.0.0.1:10251 127.0.0.1:60306 TIMEWAIT tcp 0 0127.0.0.1:43876 127.0.0.1:2379 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.184:38702 ESTABLISHED tcp6 0 0127.0.0.1:6443 127.0.0.1:43500 ESTABLISHED tCp6 0 0222.200.180.178:6443 10.96.0.9:35037 ESTABLISHED tcp6 0 0222.200.180.178:3306 222.200.180.178:36878 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:35376 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.183:55776 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:42364 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:39898 ESTABLISHED tCp6 0 0222.200.180.178:3306 222.200.180.178:36220 ESTZBLISHED tcp6 0 0 222.200.180.178:6443 222.200.180.178:54102 ESTZBLISHED tcp6 0 0222.200.180.178:6443 222.200.180.179:34888 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.184:38690 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:39802 ESTABLISHED tCp6 0 0222.200.180.178:6443 222.200.180.178:54086 ESTABLISHED tcp6 0 0 222.200.180.178:6443 222.200.180.178:39942 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:39780 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:39972 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.178:39168 ESTABLISHED tCp6 0 0222.200.180.178:6443 222.200.180.178:39604 ESTABLISHED tcp6 0 0222.200.180.178:6443 222.200.180.183:43114 ESTABLISHED

ftp-data 20 File Transfer [Default Data] ftp 21 File Transfer [Control] telnet 23 Telnet smtp 25 Simple Mail Transfer WWW 80 Web (HTTP)

Server machine Server machine 2.Request 2.Continue Register Client machine service Server Client machine end point service Specific Server Client Client Super- Create Server 1.Ask for Daemon 1.Request server and hand off end point End-point service request table

Logical s switch Application/compute servers Distributed aaessiblymuliple fileedatabase system Dispatched request 人 Client reguests Ferst tier Second tier Third tier

Logicallya single TCpP Response Server connection Request Request Client ) Switch (handedoff) Server

6.nServerresponses 5.Forward Server 3.Handof messages Distributor Othermessages Dis- Client Switch 4.Inform atcher 1. Pass setup request Distributor 2. Dispatcher selects

认为服务器客户1 分布式服务器X 地址为HA 客户1知道它是X 服务器1 APP 认为已连接到X TCP 地址为CA1的 MIPv6 访问点认为X的地址为CA1- - -— IP 因特网 l为服务器客户2 服务器2 [Tebe___ A APP 认为已连接到X TCP 地址为CA2的.1  认为X的地址为CA2一 MIPv6 访问点 IP 客户2知道它是X

User-assigned Priviliged management virtual machines virtual machines Vserver Vserver Vserver Vserver Vserver Linuxenhanced operating system Hardware

2. Client and server communicate Client Server 1. Client fetches code Service-specific client-side code Code repository

Before ∈ execution After execution  Client Server Client Server code code Cs exec exec* resource resource code code REv exec exec* resource resource CS: Client-Server REV: Remote evaluation

Before execution After execution Client Server Client Server code code CoD exec ← exec* resource resource code code MA EXec exc* resource resource resource resource CoD: Code-on-demand MA: Mobile agents

Migration 4 ↑ Downime Time

中山大學计算机学院（软件学院） NUN SUNYAT-SEN UNIVERSITY SCHOOLOF COMPUTER SCIENCEAND ENGINEERING  Docker VS VM Bins/Libs Bins/ibs Bins/Libs Bins/Libs HostOperating System HostOperating System
