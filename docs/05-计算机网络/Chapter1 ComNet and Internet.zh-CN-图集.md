# Chapter1 ComNet and Internet.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter1 ComNet and Internet.zh-CN-图集.pdf　｜　7,708 字符　｜　40 个片段　｜　更新于 2026-11-11
JamesF.Kurose Kelth W.Ross COMPUTER NEIWORKING ATOP-DOWNAPPROACH

Application SMTP FTP TFTP DNS SNMP  BOOTP layer Transpont SCTP TCP UDP layer ICMP ICMP Network IP layer ARP RARP Data link layer UnderlyingLANorWAN technology Physical layer

em em

内导体绝缘体编织层编织层外护套

PACKET DATA HEADER Sender Receiver

Physical Connectionissetup W hen call connection is made SwitchingOffices

3 R2 3 Sender R1 R3 2 Receiver 4 5 R6 3 4 R4

average queueing delay La/R 1

计算机网络  A 谢逸中山大学・计算机学院 2024. 秋季

JamesF.KuroseIKelth W.Ross 第1章引言说明在使用的这些PowerPoint幻灯 ： 我们''remaking这些幻灯片freely可用的到所有 (faculty，stuents， readers).They''re在PowerPointform因此你见animations； 和可以添加，修改，和删除幻灯片(包括这个一个) CO MPUTER 幻灯片内容到 suit your 需要 They obviously representa loi的工作在我们的part.在返回用于使用，我们仅askfollowing： NETWORKING ： 如果你使用这些幻灯片 (e.g.．在a课堂)这你mention ATOP-DOWNAPPROACH 源(之后所有，我们''d像people到使用我们的book！) ： 如果你post任何幻灯片在a网站点，这你说明这they 是adapted从 (或perhapsidentical到)我们的幻灯片， 和说明我们的版权的这个material． 计算机网络： 用于arevision历史，见幻灯片说明用于这个page． 自顶向下方法谢谢和使用愉快！ JFK/KWR ǒ"eaItIOn Jim Kurose, Keith Ross J.F Kurose 和 K.W. Ross， 所有 Rights Reserved 1-2

计算机网络是不同的从其他字段： 计算机网络是 artificial系统，不自然的p 哪个leads到existence的a大的编号的网络是大的， complex， 和time-varying，makin describe使用generalmathematical模型它involveslow-level物理网络 middle-level数和 l  hiah-levelbehaviorundele ; Data-drivenanalysis是仅a部分的它，while物理mecl 和原理是更多重要仅由mastering underlying 核心 technology可以 right speak

第一个问题： 什么happens到网络在内容(text， multimedia)  opene 计算机网络如何到achieve它？ 8

• Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33 C Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33 ●Practice：18,19 下横杠题目为必题关键词：协议，网络边缘，访问网络， 分组交换，电路交换，时延，Encapsulation

Internet： ε a “nuts 和 bolts''" ''v ie w Internet：  网络的络”  anterconnectedISPs 视频 e- Game Social Web 服务 commerce 服务通信服务 Internet：Unified 协议，policies， 和 rules Telephone television 移动 satellite Power 络络络络络

nternet： a "nuts 和 bolts. " view 协议是everywhere 移动网络 +G 1 1 national或全局lSP 控制发送，接收的报文 e.g.. HTTP (Web）. streaming 视频. Skype， TCP， IP， WiFi， 4G， 以太 Streaming Skype 视频本地或  anternetstandards 区域的 Se RFC：请求用于Comments 归属络内容提供  IETF:Internet Engineering Task Force HTTR 络 datacenter 络以太网 1 TCP enterprise 网络 WiFi 引言：1-9

[Tlbmeeee 基础设施这提供基础设施服务到应用： 移动网络 Web，streaming视频， multimedia  national或全局ISP commerce, social media, inter- Streaming 连接的appliances， Skype 视频提供编程接口本地或区域的到_分布式应用： ISP 归属网络内容 ●“连接 ks”''到， Sending/receiving 应到 HTTP 络 acent “hoo 提供商络 dat 使 Internet运输 allowing  er 服务 ● 提供服务options，analogous到  postal服务 enterprise 络引言：1-10

什么sa协议？ human协议： 网络协议： i 什么 s时间？ machines rather 比 hum  Ⅰ有a问题所有通信activity在 introductions Internetgoverned 由协特定的 msgs 已发送特定的动作经过当msgs 已收到，或a他events ahuman协议和 a计算机网络prot HI TCP连接请求 HI TCP连接 . 响应 Got the 时间？ 时间获得nttp://www.awl.com/kuros 2:00 <file>

什么 sa协议？ 网络协议： Application SMTP FTP TFTP DNS layer [协议 define 格式 Order 的 m SgS 已发送和 Transport ScTP TcI 已收到之间 layer 络 entities， IcMP IcMP  和i 动作经过在 mSg 传输， Network IP layer receipt Data link layer 例如：中国广东省广州市大学城中山大学计算机学院 UnderlyingLA techno Physical School 的计算机 Science 和 Engineering， Sun layer YatsenUniveRrsity GuangzhouHigher ducation Mega 中心， China 计算机大学城广州市中国中山大学广东省

closerIook在Internet结构移动网络络边缘： national或全局ISP 主机：客户和服务器服务器经常在数据centers 本地或区域的ISP 归属网络内容提供商络 AC 网络络引言：1-14

closerlook在 EInternet结构移动网络 [be a 主机：客户和服务器服务器经常在数据centers 本地或接入网络，物理区域的 [Ymaa _a e 🌈有线，无线通信链路 enternrica 络

closerlook在Internet结构移动网络 [Te  主机：客户和服务器服务器经常在数据centers 本地或接入网络，物理媒体： 区域的有线，无线通信链路归属网络 [Tbe __ interconnected路由器网络的网络 A 网络

无线接入网络共享无线访问网络连接端系统到路由器通过基站aka “接入点 - [t线_局域 ： widearea无线 ia 在..内building(100ft）  提供由telco （蜂窝） 802.11b/a (WiFi): 11. 540p erator, 1U''s Km Mbps传输速率 ■ 之间1和10Mbps 3G4G,5G 到 Internet 到Internet

主机：发送分组的数据主机发送】 功能：   takes应用报文 breaks 进入smaller 块， 已知 as 分组 ，的长度L比特两传输分组进入访问 L比网络在传输速率R 链路传输速率，aka 21 链路容量，aka链路带宽  R:链路传输主机分组时间 needed到传输 = 传输 _L(比特) L-hlt 时延分组进入链路 R(bits/sec)

分组-switching：store-and-forward PACKET DATA HEADER Sender Receiver 1-3

分组-switching：store-and-forward 每分组源 Rbps Rbps 目的地  takes LIRseconds到传输 (push one-hop numerica 查看)L-bit 分组进入链路在 R bps L =7.5 Mbits store 和转发： entire分组必须 R= 1.5 Mbps 到达在路由器之前它可以是 one-hop传输已传输在下一个链路时延=5sec a end-end时ae =2 R(assumingZero propagation时延） 更多在时延shortly

两个关键 network-core 功能路由 determines 转发移动 7 Source- 分组目的地路由经过由分组从路由器’s输入到合适的路由算法路由器车输出 [ca算法本地转发表首部值输出链路 0100 3 0101 2 0111 1001 1 目的地地e在到达的分组'' s首部 4-34

alternative核心：电交换 end-end resources allocated到，reserved用于 “调在diagram， 每个链路有four circuits.   调用获得2ndE 电路在top链路和电路在 right Iink. dedicated resources:无 sharing   circuit-like (guaranteed) 性能电路报文段idle如果不用于由调用无 ● Commonly 用于在传统的 telephone 络支持多距

分组交换与电路交换分组交换允许更多用户到使用网络！ 示例： 报1Mb/s链路 ATTENTION N 每个用户： ：  ：用户 100kb/s当主动  1Mbps ·主动10%的时间 acircuit-switchingl Q：如何did我们获得值0.00 10用户  10*100kb/s=1Mb ● 分组交换： (binomialdistribution) P(X 与 35 用户， probability>10 Q：什么 happens 如果 >35 用户主动在相同时间是less 比.0004* *更多示例见在线交互练习

分组交换与电路交换是分组交换a slam dunk winner?   great用于 bursty数据 resource sharing simpler， 无调用 setup excessive拥塞可能： 分组时延和丢包协lneeded用于可靠数据传输， [a_控制 Q：如何到提供circuit-likebehavior？ 带宽guaranteesneeded用于audio/video应用仍然unsolved问题 Q：humananalogies的reservedresources （电路交换） versl on-demand allocation(分组-switching)?

Internet结构：网络的网络选项： 连接每个访问ISP到每一个其他访问 ISP? 访问访问访问网络网络网络访络访问 - 网络网络访问访问网络网络 connecting 每个访问 ISP 到每个其他 directlydoesn''tscale: O(N ) 网络访问网络访问网络访问网络网络访问网络 1网络

anternet结构： e的 e 选项： 连接每个访问ISP到a全局transitISP？ Customer和 [ 供 lSPs有economicagreement 网络I 网络访问网络访问访问网络网络访问访问网络网络全局 X ISP 访问区 X 网络访问网络访问网络访问网络访问访问络访问| 络 1网络

第1章：引言 1.1 什么是theInternet？ 1.2F F 网络边缘端系统，接入网络，链路 1.3网络核心分组交换，电路交换，网络结构 □ 1.4时延，丢包，吞吐量在网络 1.5 协议分层，服务模型 1.6网络underattack：安全 1.7历史 1-51

Foursources s的分组时延传输 A propagation B nodal processing 排队 dnodal = d。 + dtrans + dprop proc C 队列 dproc nodalprocessing 队列 ： 排队时延查看比特差错时间 waiting在输出链路传输确定输出链路  depends在拥塞Ievel白通常 くmsec

“Real”Internet时延s和路由 （DlY）  什么   “real”Internet时延 & 丢包look像？  aceroute(win： tracert) program： 提供时延 measurement从源到路由器alongend-end Internet 路径 towardsdestination. 用于所有 i： 发送 threeUDP分组这将reach路由器 i在路径towards 目的地路由器i将返回分组到发送方发送方timesintervalbetween传输和reply． 1-59

traceroute:gaia.cs.umass.edu 到 www.eurecom.fr abilene-vbns.abilene.ucaid.edu (198.32.11.9) 22 ms 18 ms 22 ms nycm-wash.abilene.ucaid.edu(198.32.8.46)22ms22ms 22ms trans-C 8 62.40.103.253(62.40.103.253) 104 ms 109 ms 106 ms 链路 9 ( de2-1.de1.de.geant.net (62.40.96.129) 109 ms 102 ms 104 ms 10 de.fr1.fr.geant.net (62.40.96.50) 113''ms 121 ms 114 ms [1 renater-gw.fr1.fregeantanet(62.40.103.54) 112 ms 114 ms 112ms 12 nio-n2.cssi.renater.fr(193.51.206.13) 111 ms 114 ms 116 ms 13 nice.cssi.renater.fr(195.220.98.102) 123 ms 125 ms 124 ms [5 eurecom-valbonneeR3t2.ft.net(193.48.50.54) 135 ms 128 s 33ms 16 194.214.211.25(194.214.211.25) 126 ms 128 ms 126 ms 17 18 *** *means无响应 (probe丢失的， 路由器不replying) 19 fantasia.eurecom.fr(193.55.113.142) 132 ms 128 ms 136 ms *a做一些 traceroutes 从 exotic countries 在 www.traceroute.or

[分a包队列(aka缓冲区）preceding链路在缓冲区有fi 容量分组到达的到完整的队列dropped（aka丢失的） 丢失的分组可能是retransmitted由previous节源端系统， 或不在所有缓冲区 A (waiting区域） 分组 being已传输 B 分a到达的到完整的缓区是丢失的 * 查看查看 Java applet用于交互式 animation 在 que uing 和丢包

[吞吐量吞吐量： 速率 (bits/timeunit） 在哪个比特 transferred之间发送方/接收方 instantaneous:速率在给定的点在时间 average 速率在longerperiod的时间服务器发送比.特链pipe 这可以 carry 链pipe 这可以 carry (fluid） 进入pipe fluid在速率 fluid在速率 1-62

第1章：引言 1.1 什么是theInternet？ 1.2网络边缘端系统，接入网络，链路 1.3网络核心分组交换，电路交换，网络结构 1.4时延，丢包，吞吐量在网络 1.5 协议分层，服务模型 1.6 网络under attack：安全 1.7历史 1-91

Badguys：attack服务器， 网络基础设施 Denial的服务 （DoS）： attackers生成resources （服务器， 带宽）unavailable到legitimate流量由 overwhelming resource与bogus流量 1.select target     2.break进入主机around 网络 (见botnet) o 3.发送分组到target从  compromised主机 ter rget 1-95

引l：总结 Covereda a“ton | 的材料！ 你现在有 Ynternet概述 Context. 什么   sa协议？ "feel”的络边缘，核心，访问络 ● 更多dept [分a-switching与 follow！ Yaterneta构性能：丢句时延，  吞吐量 ● 安全端的第历史
