# Chapter5 Network Layer - 2.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter5 Network Layer - 2.zh-CN-图集.pdf　｜　7,417 字符　｜　44 个片段　｜　更新于 2026-11-11
学大山中立团

山大

James F.Kurose Keith W.Ross COMPUTER NETWORKING ATOP-DOWNAPPROACH

Host Router Multicast Host Router

4b 4c AS4 2a 3a 2b 3b AS2 AS3 1a Receive BGP advertisements for 1d 212.21.21.21 from Forward toward Advertise Server B since it is closer. 212.21.21.21 CDN Server A Advertise 212.21.21.21 CDN ServerB Figure 5.12 Using IP-anycast to bring users to the closest CDN server

TBM

network-control applications routing access load control balance control northbound APl plane SDN Controller (network operating system) southbound API

network-control applications routing access load Control balance

SDN-controlled switches

northboundAPl (network operating system) southboundAPl SDN-controlled switches

?

nof ;  DN e?

前4个字节 0 8 16 31 都是一样的类型代码检验和 （这4个字节取决于ICMP报文的类型） ICMP的数据部分（长度取决于类型） ICMP报文首部数据部分 P数据报

NMS NETCONF ccalient NETCONF IP Network NETCONF Server  0

Netconf-Client [a ContentLaye C onfgrai Data Notfcain cedit-configo Message Layer <rpc>,<rpc- reply> < notification > Layer SSH,TLS,BEEP/TLS,SOAP/HTTP/TLS, Netconf-Server

Netconf-Client ca Layer Example Contnt a r Confguai D a N oticain peration Layer eget-configo, Message Layer <rpc>,<rpc- reply> <notification> cYaea neel Layer SSH, TLS, BEEP/TLS, SOAP/HTTP/TLS,. Netconf-Server

Netconf-Client ca Layer amele ContentL r ongurai D a N oticatinD aperation Layer eget-confgo, Message Layer <rpc>,<rpc- reply> <notification> cYacaReaTYanee Layer SSH, TLS, BEEP/TLS, SOAP/HTTP/TLS,… Netconf-Server

网络管理系统 IT运维系统 YANG模型 YANG模型 NETCONF RESTCONF Client Client [TNet o SSH HTTPS NETCONF RESTCONF Server YANG模型 Server

YANG YIN ontainerauto-recovery-times containername= auto-recovery- listauto-recovery-time{ times"> leaferror-down-type{ typeerror-down-type; 无损转换 <keyname="error-down-type"> leafname="error-down-type"> } <typename="error-down-type"/> leaftime-value{ /leafo typeuinta2 leafname=time-value> } rangevalue="30.86400"/> units"s"; 无损转换 <unitsvalue="s"/> mandatorytrue mandatoryvalue true"/ /leafo

01 <?xml version="1.0" encoding="UTF-8"?> 02<rpcmessage-id="10l" 03 xmlns="urn:ietf:params:xml:ns:netconf:base:l.o"> 04 <edit-config> 05 <target> 06 <running/> 07 </target> 08 <config>  09 <top xmlns="http://example.com/schema/ 1.2/config"> <interface>  <name>Ethernet0/0</name> <mtu>1500</mtu> </interface> </top> </config> 16 </edit-config> 17 </rpc>

计算机网络大谢逸中山大学・计算机学院 2024. 秋季

第5章 JamesFKurose KeithW.Ross [e层： 控制平面说明在使用的这些PowerPoint幻灯片： 我们''remaking这些幻灯片freely可用的到所有 (faculty，students， readers)． They'' re在 C MPUTER PowerPointform因此你见.animations： 和可以添加. 修改. 和删除幻灯片 (包括这个一个） 和幻灯T 内容到 suit vour needs. Thev obviously represent a ot NE丁W ORKlN G 的工作在我们的part.在返回用于使用， 我们仅askfoIlowing： ATOP-DOWNAPPROACH Eighth Edition 如果你使用这些幻灯片(e.g.，在a课堂)这你mention它们的源(之后所有， 我们d像people到使用我们的book！)  如果你post任何幻灯片在a网站站点. 这你说明这thev是adapted从（或 perhapsidentical到）我们的幻灯片，和说明我们的版权的这个material． 计算机终. 用 arevision历史， 见幻灯说明于这个 page Top-Down 法谢谢和使用愉快！ JFK/KWR 8tedition Jim Kurose, Keith Ross 所有材料版权 1996 2n23 Pears0n，2020 J.F Kurose 和 K. W. Ross， 所有 Rights Reserved 5-2

网络 ·层功能回顾：两个网络层功能： 数据平面转发· 移动分组从路中器 s输入到给定的a转发表合适的路由器输出控制平面路中确定路中经过中分组从源到目的地 Createa路由表用于转发两个方法到构建网络控制平面： 每路由器控制（传统的） 逻辑集中式控制（软件定义网络） 5-6

每路由器控制平面   Individual路由算法components 在每个和每一个路由器 interact与每个其他在控制平面到计算转发表s 路中算法控制平面 -- Localforwarding 数据 table 平面 header output 0100  0110  0111  1001 1 5-7

图抽象 5 Ev33Ew3 2 Eu 2 3 1 —z> 1 E = 集合的链路 = { (1,V), (u,×), (v,x)， (V,w )， (x,w), (x,y)，(w,y)，(w,z), (y,z) } aside：图抽象是有用在其他络场景．e.g.． P2P，哪里 N是集合的对等方和E是集合的TCP连接 4-11

图抽象：费用 5 c(x,x'')= 链路费用 ((,x) v 3—Ewy e.gc(w,z)=5 费用可以总是是1．或 CU 2 EZ 成反比相关到带宽 3  或成反比相关到 EX V- 拥塞路径费用 (x ，×2,×3, ,×p) = C(x₁,×2) + c(×2,×3) + . + c(×ρ-1,×ρ) 关键问题：什么是最少的费用路径之间u和z？ 路由算法： 算法这finds这最少的费用路径 4-12

第5章：提纲 5.1引言 5.5SDN控制平面 5.2 路由协议 5.6ICMP： Internet控制链路状态报文协议距离向量 5.7网络管理和SNMP 5.3 内部的AS路由在Internet： OSPF 5.4路由之间ISPs：BGP 5-14

链路状态路由算法 Dijkstras 算法记号： ~Iv · 路弗田从节占v到y； 络拓扑，链路费用s已知到所有  =∞如果不直接邻居节点 n·业前估的路径费用 accomplished通过链路状态播 M 从源到 dest.v 所有节点有相同信息  nl八节占long路径计算最少的费用路径从一个从源到v 节点（源 ）到所有其他节点N' '·隹个的节占…h~a最少的费用路径definitively已知 ■ 给出转发表用于这节点迭代式：之后k迭代.know 最少的费用路径到kdest.s 4-15

Dikstras 算法： 另一个示例步骤 N'' D(v) ,p(v) D(w),p(w) ) D( ×),p(×) D(y),p(y) D(z),p(z) 2 uxy 2,u 3,y 4,y 3 uxyv 3,y 4,y 4 uxyvw 4,y 5 UXyVwZ 5 2 EuV EZ3 Eu3 2 3 1 Z3 Exu- Eyy 1 20241117 4-18

Dijkstra''s算法：示例 (2) 得到的shortest 路径tree从u: EX3 Ey 得到的转发表在u: 目的地链路 v (u,y) x (u,x) y (u,x) w (u,x) z (u,x) 4-19

第5章：提纲 5.1引言 5.5 SDN控制平面 5.2 路由协议 5.6 ICMP： Internet控制链路状态报文协议距离向量 5.7网络管理和SNMP 5.3内部的AS路由在lnternet： 5.4 路由之间ISPs：BGP 5-21

距离向量算法迭代式，， 异步：  每个本地每个节点： 迭代caused由： 本地链路费用变化等待用于（变化在本地链路费用或msg从邻居) 「 DV更新报文从邻居 [TabT 每个节点通知邻居仅当重新计算估计它的DV变化邻居则通知它们的邻居如果如果DV到任何dest有 [必要时 hanged 居  -2

距离向量：链路费用变化 50 节点detects本地链路费用变化 ,updates路由信息，recalculates 本地 DV “t 的 n ac  travels快速如果DV变化，通知邻居  to:ydetects 链路费用变化， updates 它的 DV， informs 它的 neighhors t：z接收更新从 ν．updates它的表，计算新的最少的费用到x，发送它的邻居它的DV． t2：v接收 zs更新.updates它的距离table. Vs最少的费用做不变化，因此y做不发送a报文到 z. 网络层：5 13

Internet 法到scalable路由 aggregate路由器进入regions已知as autonomous 系统 9 AS)(a.k.a. domains 内部的.AS路由之间的.AS路由路中之间主机，路由器在相同AS路由之间AS' 'es （“网络”） gatewaysperform之间的域路由所有路由器在AS必须运行相同内部的.  (as wellas内部的域路由) [域协议路中器在不同的 1 可以运行不同的 ■  内部的域路由协议关路由器：在 “边缘” 的它的自己的AS，  有链路（s）到路由器（s）在其他AS''es 5-52

之间的-AS tasks AS1必须： 数据报destined outside 的AS1： 1. Iearn 哪个 dests是通过 AS2， 个通过 AS3 ce器should转发分a到关路由器 2. propagate 这个 reachability信息但是哪个 — 个？ 1 到所有路由器在AS job的之间的AS路由！ 3c 3a 3b AS3 2c 1c 其他 2a 网络其他 E1a 2b  网络 1b AS2 AS1 E1d 4-54

Internet 之间的 .AS  路由：BGP 3c 3a 2C 3b 2a AS3 2b 1c AS2 BGP： Implementing   i 通信之间 ASes 20241118 4-68

eBGP， iBGP连接 2b 2a   2d 1a 1c a 3c ASZ 1d 3d AS1 AS3 2b 2d a 1c 关路由器运二者 AS2 eBGP 和iBGP protools 1d 3d AS1 AS3 5-71

路aattributes和BGP路由 M prefix + attributes = 路由 ''   两个重要 attributes： AS-PATH:列表的ASes通过哪个prefixadvertisement有passed  NEXT-HOP：indicates特定的 -AS路由器到下—个跳AS 策略基于路由：      网关接收路由advertisement使用 import策略到 accept/decline路 eg never路由通过ASY）  AS 策略 alsodetermines whether到 advertise  路径到其他其他   5 5-74

BGP路径advertisement AS3 AS1 3b 1b 3a 3c 1a 1c eBGP AS2 3d 1d AS3× A,A3,× a c iBGP 2d ◼ AS2 路由器 2c 接收路径 advertisementAS3,X(通过eBGI） 从 AS3 路由器 3a 其干左Ac）笑略，AS2 路由器 2caccepts路径AS3，X，propagates（通过iBGP） 到 ● 所有AS2路由器基于在AS2策略，AS2路由器2a （ （通过eBGP） 路径AS2,AS3，X 到ASI路由器1c 5-75

AS3 AS1 3b 1b AS3,× 3a 3c 1a 1c AS2 b 3d 1d AS3,× AS2,AS3,X 2a 2c 2d 网关路由器可能Iearn关于多路路径到目的地： AS1网关路由器 clearns路径AS2AS3X从a AS1网关路由器 1cIearns路径AS3X从3a 基于在策略， AS1 网关路由器 1cchooses 路径AS3,X， 和advertises路径在...内AS1i 通过iBGP 道 5-76

BGP报文 BGP报 exchanged之间对等在TCP连接 BGP报文： OPEN：opensTCP连接到远程BGP对等和 authenticates发送BGP对等方 UPDATE：advertises新的路径（或 withdrawsold） KEEPALIVE：keeps连接alive在absence的UPDATES；also ACKOPEn请求 ■NOTIFlCATlON：reports 差错在previousmsg； also用于到关闭连接 5-77

BGP：achieving策略通过advertisements legend： 垾供络  C 网络：   SupposelSP仅wants到路由流量to/来自它的customer 网络 (做不 want到carry transit流量之间其他ISPs) advertises路径Aw到B和到C Bchooses不到advertiseBAw到C： R获得天 “rovenile” 用于路由CBAw，sincenone的C，，w  是B''scustomers C 不learn关于 CBAw路径  C将路由CAw（不使用B）到获得到w 5-81

BGP：achieving策略通过advertisements B 络 SupposelSP仅wants到路由流量to/来自它的 customer 网络 (做不 want 到 carry transit 流量之间其他 ISPs) ■，B，C是提供商网络  X，W，Y 是customer (的提供商网络)  X 是 dual-homed:attached 到两个网络策略到enforce：X做不want到路由从B到C通过X ■ .因此X将不advertise到Ba路由到C 5-82

软件定义网络（SDN) [YWayalegically Centralized 控制平面 ？ easier网络管理 ：avoid路由器misconfigurations， greater flexibility的流量 flows 表基于转发 （回顾OpenFlowAPI） 允许 “编程” 路由器 centralized “编程” e ： 计算表centrally和 distribute 分布式 “编程：更多 ：计算表asresult 的分布式算法（协议）implemented在每个和每一个路由器 5-93
