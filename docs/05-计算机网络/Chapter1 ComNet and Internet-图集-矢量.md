# Chapter1 ComNet and Internet-图集-矢量

> 主题库：计算机网络　｜　源文件：Chapter1 ComNet and Internet-图集-矢量.pdf　｜　3,996 字符　｜　67 个片段　｜　更新于 2026-11-13
がh 1-2

Comp 5 How te

33 33 on

ternet: a"nuts and b Billions of connected computing devices: hosts= end systems running network apps at anternet''s"edge Packet switches: forward packets(chunks of data)  routers, switches   Communication links fiber, copper, radio, satellite

etwork of networks" ctedISPs commerce service Internet: Unified protocols, p television Mobile

nternet: a “nuts an ocols areeverywhere trol sending, receiving of messages HTTP (Web), streaming J video, oe, TCP, IP, WiFi, 4 G, Ethernet caRequest forComments F: Internet Engineering Task Force

terme asevlces tructure that provides es to applications: cYatreaming v deo, m ultimedia anferencing, mailgames mercesocialmediainter Sky Yantedappliances, . es programming interface ributed applications c allowingsending/receiving appsto ce touselnternetranspor

oser look at Intern osts: clients and servers ervers often in data centers

ork atacenter

oserlook at lntern sts cieta ewers dia rired, wireless comm unication

t V O e C te Ce s n e t rk o s o ys s si c a a e edi dia i vir ire rer ed i wir ire re ele es ass ss c o o n rn nr n m ur ni nic icat ca ati tic ior li C n lir in k s n s

ccess networksanc Howtoconnectend ystemstoedgerouter? stitutional access networks (school, obile access networks (WiFi, 4G/5G)

cmtent orovider network datacenber network Introduction:1-17

1-18

use exlsting teiepnoneT  data overDSLphone  voice over DSL phon  < 2.5 Mbps upstream tr  < 24 Mbps downstream

frequen in diff er

ncydivision  multiple  rent frequency ban

HFC: hybrid fiber COax asymmetric: up to upstream transmiss  network of cable, fiber homes share acce unlike DSL,which I

often combined cana wireless access ro wired

typically used in co  10 Mbps, 100Mbps  today, end system

rnet switch

VIFT):TT,54

lication messa to smaller chur packets, of ler packet into ac t transmission ansmission ra pacity,aka lir

link link ban

sh of interconnected routers cket-switching: hosts break alication-layer messagesinto orward packets from one router to he next, ε across links on path from [auarcetodestination ach packet transmitted at full link

o datacerter network 1-29

ver 1-30

nyL-prpacke tore and 】forwa rive at router   cana n e propa

ens will ,wai Yns can be

routing algorithms rou Yocal forwarding table eader value outpt link 0100  0101 0111 1001  destination address in arriving Packets header

mines sourc taken by packe

forw ce- ets from cYaaute Bene

dresources alloca se &dest: gram, each link has fo gets 2nd circuit in to cuit in right link. ated resources: no sha uit-like ( (guaranteed) segment idle if not us

帧 1234 1234

etswitchingve hingallowsmor ATTENTION active 100kb/s=1Mb time is less ut the online interactive exercises fo

ture 41

42

S? connecting each acces other directly doesn''t onnection

global X ISP

ISPC

IXP ISP C pe

IXP ISPC

Internet structure: net center:small# of I-connected je networks Tier1ISP tier-1 Commercial SPs (e.g., Level 3, print,AT&T,NTT), IXP overage Region antentprovider etwork (e.g, Google): rivate network that ften bypassing tier-1,

cture 1-51

pa free (available) bu dropped (loss) if

dproc: n check determ typical

catransitransmissi L:packet le R:link ban dtrans = L/R *Check out the Java

eous:rate at givel rate over longer p nds bits pipe thatc capipe fluid at

R。 bits/sec Rc What is averag Rgbits/sec eneck link end-end path that co cY

end throughput: in r practice:R orR is

cYaYoee ofl structure c

lINK Internet protocol sta

deali exp cor la

ternet rotocol  stack application:   S supporting ne transport: process-proces TC D

/OSl reference model resentation: allow appli eaning of data, e.g., en ompression, machine-s

[AP 应用进程数据先加上应用层首部 PDU(Protoco 2 OSI参考模型把 3 该层的协议数据 2

[ 5 应用层 PDU再传 4 加上运输层首部 3 2

5 4 运输层报文再传送 3 加上网络层首部， 2

[A 3 IP数据报再传送 2 加上链路层首部

[ 3 数据链路层帧再信最下面的物理层扣

AP1 5 4 3 2

AP1 运输层首部网络层部 4  链路层部  3   H2 H3 H 1010011010

AP1 把分 4 H4 3 H3 H4 2 1

87

AP， 1 把报 5 H5 H4 Hs 3 2

AP1 5 H5 4 应用把应 3 2 1

AP1 5 4 3 2
