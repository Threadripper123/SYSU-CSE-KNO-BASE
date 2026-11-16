# Chapter3 Transport Layer.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter3 Transport Layer.zh-CN-图集.pdf　｜　11,577 字符　｜　55 个片段　｜　更新于 2026-11-11
學大山中立团

山大，

James F.Kurose Keith W.Ross COMPUTER NETWORKING A TOP-DOWN APPROACH P EighthEdition

APACHE @ HTTP SERVER

email WWW phone... SMTP HTTP RTP.. TCPUDP cYethernetPPP CSMA async sonet ... copper fiber radio.

单向(UDP)发送端单向(UDP)接收端单向(UDP)发送端 Socket  Socket  Socket (P3,F Prtx) (IP3 Prtx (IP3 Prx) Host1 host3 Host2

udtsend()I packet packet rdtrcv() unreliablechannel (a) providedservice (b)serviceimplementation

udt_send() packet packet rdt_rcv() unreliablechannel (a) providedservice (b)serviceimplem entation

elluMle uuld elldDle ddld transfer protocol transfer protocol T (sending side) (receivingside) udt_send() packet packet ↓ rdtrcv() unreliablechanne) (a) provided service (b) service im plem entation

rdt send () reliable channel data data ↑deliver data()

rdt send() data data  deliver data() reliable data reliable data cnaansferprotocol transferprotocol sending side) (receiving side cat_send( packet packet Trdt rcv() unreliable channel

data packet— datapackets 十——————————— — ACkpaCkets (a) a stop-and-wait protocol in operation (b) a pipelined protocol in operation

send base nextsegnum already usable, not ack''ed yet sent sent,not not usable yetack''ed windowsize_

send base nextsegnum already usable, not ack''ed yet sent notusable Yetacked window size N (a) sender view ofsequence numbers

outoforder  l acceptable (buffered)but (within window) alreadyack''ed Expected, not not usable Yetreceived rcv_baSe (b) receiver view of sequence numbers

source port # dest port # cYaYaauencenem e head not receive window len used UAPRSE checksum Urg data pointer options(variable length) Yaaa YYa nablelangt e

HostA HostB Network feedback via receiver feedback

MTU（以太网为1500字节） MAC IP TCP 数据头部头部头部头部 FCS MSS

16- 14 12-1 s CPReno s 0- 8- thresh T 6- theh 4- CPTahoe T 2- 0 1 2 3 4 5 6 7 8 9 10   11 12 13 14 15 Transmission round

cwnd  RTO ssthresh ssthresh Time slow congestion congestion congestion slow congestion start avoidance Sky avoidance avoidance start a oicance l

Additive Ircrease Binary Search ax Mlax Probin 知乎 @SKY

Steady State Behavior https://blog.csdn.nev知 @ ky

JamesF.KuroseIKeithW.Ross 第3章运输层说明在使的thes ePowerpoint幻灯 ： C MPUTER 我们lremaking这些幻灯片 freely可用的到所有 (faculty，students，readers). They''re在 PowerPointform因此你见.animations： 和可以添加. 修改. 和删除幻灯 NETWORKlNG (包括这个 — 个) 和灯内容到 suitvourneeds. Thev obviously representalot 的作在我们的part．在返回用于使，我们仅askfollowig： ATOP-DOWNAPPROACH  Eighth Edition  ■ 如果你使用这些幻灯片(e.g.， na课堂) 这你 mention ir源(之后所有， 我们d 如果你post任何幻灯片在a网站站k．这你说明这thev是adapted从（或计省机风l终· perhaps identical 到) 我们的幻灯片， 和说明我们的版权的 thi material. 自顶向下方法谢谢和使用愉快！JFK/KWR anaedition J.F Kurose 和 K.W. Ross， 所有 Rights Reserved Jim Kurose,Keith Ross 2-2

第3章：提纲 3.1运输层服务 3.5面向连接 3.2多路复用和运输：TCP 多路分解报文段结构 ■ 3.3无连接传输： 可靠数据传输  UDP 流量控制 3.4原理目的可靠数据连接 ■ 传输管理 3.6原理的拥塞控制 3.7TCP拥塞控制 3-5

运输服务和协议应用提供逻辑通信之间应输证络据链器进程运在不同的理数主机运输协议运行在端系统发送一侧：breaks应用报文进入报文段 ，passes到网络层 rcv一侧：reassembles报文段进入报文，passes到应用层更多比一个运输协议可用的到应用运输应用网络数据链路物理 Internet：TCP和UDP 20241012 3-6

运输 VS. 网络层应用进程应用进程 AP， AP， 端□—AP3 AP 端口运输层提供应用进程间的逻辑通信 IP层 3 2 2 主机A 主机B ca器路a器 AP AP LAN WAN LAN2 AP  AP [2 [Pb协l 范运输层协议TCP和UDP的作用范围

Internet 运输层协议可靠、按序交付应用 (TCP)  运输网络 00 数据链路拥塞控制物理络络数据链路流量控制数据链路物理物理络连接建立数据链路物理 unreliable,l unordered 网络数据链路物理交付：  络数据链路物理 no-frillsextension的络数据链路_  应用 “best-effort” 物理 ’IP 络运输数据链路络数据链路服务物理不物理可用的：  时延 guarantees 帶宽 guarantees 3-12

多路复用/多路分解多路复用在发送方： [Tba e c 多路分解在接收方： 部 +r田干用於使田部信自到六付多路分解） 口il竹到l 报文段到correct 套接字应用应用 P1 P2 应用套接字 P3 运输   P4 进程 emailwwwphone.. 运输网络运输 SMTPHTTPRTP. 网络链路络物理 TCPUDP 链路链路十 Ⅲ  物理物理 IP ethernetPPP CSMAasyncsonet. 多路复用：Aggregate数据从d ifferent应用到 TCP和IP层 copperfiberradio.. 多路分解：Distributenetwo rk 层数据到不同的 1 appl cations

无连接 ：： 多路分解回顾：( Created   套接字有回l顾· 当rrea+inσ 主机-local立端口发送讲入 」 #： 数据报到 ： UDP  : 套接字， / 必须 ！ specify DataaramSocket mvSocketl = 新的DatagramSocket( 2534）； 目的地IP地址目的地端口#  当主机接收 IP 数据报与 UDP）  报段： 相同des+ 端 #但是不同的源 P checks目的地端口 tmtiland/nr ■ ＃在报文段源l口编号将是direrted到相同 directsUDP ）报文段到套接字在dest 套接字与这端口 # 单向(UDP)发送端单向(UDP)接收端单向(UDP)发送端 App1 App2 App App Socket Socket Socket 3-22 (IP3, Prtx) (IP₃,Prtx) (IP₃,Prtx) Host1 host3 Host2

无连接 demux： 示例 DataaramSocket serverSocket= 新的 mySocket2=新的 DatagramSocket mySocket1=:  新的 DatagramSocket (6428)； 发送端在本地 DatagramSocket (9157)； 的镜像。 接收端程序应 （5775)； 混合了来自在本地的一应用 P1 应用 P3,P4的数据， 个镜像 P3 P4 运输无法区分来源运输网络运输网络链路网络链路物理链路物理物理源端□·6428 源端·? 源端·9157 源端·？ dest 端口：6428 dest端口：？ 发

面向连接demux TCP套接字identified由4-  服务器主机可能支持许多 tuple: simultaneousTCPsockets: 源IP地址每个套接字identified由它的自己的源端口编号  destIP地址  Web服务器有不同的 dest端口编号 sockets用于每个connecting 客户  demux：接收使所有four 值到直接报文段到 non-persistentHTTP将有不同的套接字用于每个请求合适的套接字 3-24

面向连接 demux： 示例发送端在本接收端程序应用地的镜像。 应用在本地的一 P4 P5） P6 独立对应每应用个镜像 P3 P2  一个通信对 P3 运输端应用程序 ▲ 运输网络可以区分运输 L 。  络链路来源络链路物理链路物理物理服冬哭·ID [TeeB 主机·ID 主机·ID 地址地址C 源IP 端□·R8O dest IP，端 ：，9157 源IP端 ·f5775 destIP.端□：B.80 源IP 湍 · 9157 dest IP， 端口：B,80 源 ID 端·9157 dest IP.端□： B.80 three 报段． 所有 destined 到 IP 地 ：B． dest 端口：80 是 demultiplexed  到不同的  sockets 3-25 不同的应用 (Instances)

面向连接 demux： 示例 threaded) 服务器应用应用应用 P4 P3 P2 F P3 运输运输网络运输络链路络链路物理链路物理服哭：ID 物理 [eeB 主机·ID 主机·ID 地址C 地址源IPi 端□·R8O destIP,端口：，9157 源 IP端 ·f.5775 destIP，端□：B.8O 源IP 端 · 0157 dest IP， 端口：B,80 源 IP 端·9157 dest IP,端口： B,80 3-26

第3章：提纲 3.1运输层 3.5面向连接服务运输：TCP 3.2多路复用和报文段结构多路分解可靠数据传输 3.3无连接流量控制运输：UDP 连接 ■ 管理 3.4原理的可靠 3.6原理的数据传输拥塞控制 3.7TCP拥塞控制 3-29

UDP：报文段之首部 ，  长度．在字节的 32比特 UDP报文段， 源端口# dest端口# 包括首部长度校验和 why是存在aUDP？  无连接 [应用 establishment（哪个 [b （载荷） 可以添加时延）  simple： 无连接状态在发送方，接收方  small首部大小 UDP报文段格式无拥塞控制： UDP 可以 blast away as 快速as desired 3-34

UDP校验和目标：检测差错 (e.g，flipped比特）在已传输报文段发送方： 接收方： ： . treat= 报文E段 on+en+， 计算校验和的已收到 ae 部字段报文段序列l的 16-bitintegers 椋验和n.addi+ion(—个， 查看如果计算得到的校验和equals , c Complementsum） 的报段校验和字段值： 发详亡n+。校验和值进入 ● YES-无差错detected.但是  UDP校验和字段  maybe差错nonetheless？ 更多 Yater 3-35

可靠数据传输：getting started dtsend ：called从上方， called由 (e.g.， 由app.）．Passed数据到 rdt到交付数据到 upper 交付到接收方upper层发送方 reliable data reliable data 接收方 cnaansfe rotocol naransfe rotocoll sending side) (receiving side) udtsend()I packet packet rdt_rcv() cunreliable hannel called由rdt called当分组到传输分组在到达在rcv-side的信道 unreliable信道到接收方 3-46

可靠数据传输：gettingstarted incrementallvdevelop 发送方， 接收方 sides的 reliabledata ransfer协议 (rdt) consider仅unidirectional数据传输但是控制信息将流在二者directions！ 使用 finite 状态 machines（FSM） 到 specify发送方， 接收方 eventcausing状态transition 动作经过在状态transition [状a当在Y “状态，下一个状态 （状态 、 状态 determined eeent_ 2 由下一个event  动作 3-47

rdt1.0：可靠传输在a 可靠信道 underlying 信道直 （i.e.，IP） perfectly 可靠无比特差错无丢包的分组  separateFSMs用于发送方，接收方： 发送发送数据进入underIying信道 ■ 接收方 reads 数据从 under lying 信道等待用于 rdt_send（数据) 等待用于 rdt_rcv(分组) 调用从分组=makepkt(数据） 调用从 extract（分组.数据) 上方 below udt_send(分组) deliver_data(数据) 发送方接收方 3-48

rdt2.0：信道与比特差错 underlving信道可能flip比特在分组验和到检测比特差错 the 问题：如何到recover从差错：  ·，   接收方explicitly告诉发送方这pkt已收到0K 方 explicitlv告诉发送方汶nkthad差错 ：的： 如何做humans recover从 · . 差错垃ilk亡 f;edback： 控制 msgs （ACK，NAK） rcvr- >发送 3-49

rdt2.0：信与比特差错  underlving信道可能flip比特在分组校验和到检测比特差错 the| 问题：如何到recover从差错：  acknowledaements(ACKs):接收方explicitly告诉发送方这pkt 已收到0K neaative acknowledgements (NAKs):接收方 explicitIy 告诉发送方这 pkthad差错发送方 pkt在receipt的NAk  新的mechanisms 在 rdt2.0(beyondrdt1.o)： [be feedback： 控制msgs（ACK，  NAK） 从接收方到发送方 -5

rdt2.0：operation与无差错 rdt_send(数据) snkpkt = make_pkt(数据， 校验和） t send(sndnb rdtrcv(rcvpkt)&& 等待用于等待用于 isNAK(rcvpkt) 调用从 ACK或 ） u rdtrcv(rcvpkt)&& udt_send(sndp 上方 u 损坏(rcvpkt)  NAK udt_send(NAK) rdt_rcv(rcVpkt)&& isACK(rcvpkt)   等待用于 Λ 调用从 balew rdtrcv(rcvpkt)&& r notcorruntlrcvnkt) extract(rcvpkt数据) cYeliverdata(数据e udtsend(ACK) 3-52

rdt_send(数据) snkpkt=make_pkt(数据， [校验和) 1+ send(sndnk rcv(rcvpkt)&& 等待用于等待用于 isNAK(rc + ( rdtrcv(rcvpkt)&& 调用从 AcK或 udt_send(sndp 损坏(rcvpkt) 上方 NAK kt) 1dt_send(NAK) rdtrcv(rcvpkt)＆&isACK(rcvpkt)  等待用于 Λ  调用从 bale  rdtrcv(rcvpkt)&& stop和等待 notcorruntrcvnkt 发送方发送 —个分组 deliverdata(数据） 则waits用于接收方响应 udtsend(ACK) 3-53

rdt2.0 有 a fatal flaw! 什么happens如果ACK/NAKcorrupted？ 发送方doesn'' tknow什么happened在 [收方！ 可以’+仁件→∈超时： 处理duplicates  可能重复的发送方retransmits当前pkt如果 ACK/NAKcorrupted 发送方adds序列编号 KEYQUESTION： 到每个pkt duplicates 接收方discards （doesn’t交付上）重复的pkt 3-54

rdt2.1： 发送方， 处理出错的ACK/NAK rdt_send(数据) sndpkt =makepkt(， 数据， 校验和) udt_send(sndpkt) rdtrcv(rcvpkt)&& (损坏(rcvpkt)一l 等待用于等待用于 ACK或 jsNAK(rcvpkt) 调用从上方 NAK udt_send(sndpkt) rdt_rcv(rcvpkt) rdt_rcv(rcvpkt) &&notcorrupt(rcvpkt) &&isACK(rcvpkt) ) )  &&isACK(rcvpkt) [等待用于 /等待用于\ ACK或调用从 rdtrcv(rcvpkt)&& NAK 入上方 rdt_send(数据) isNAK(rcvpkt)) udt_send(sndpkt) sndpkt = make_pkt(， 数据， 校验和) udt_send(sndpkt) 无序列编号在ACK 3-55

rdt2.1： 接收方， 处理出错的ACK/NAK && has_seq (rcvpkt) extract(rcvpkt.数据) cYae_aa sndpkt = make_pkt(ACK, chksum) udt_eendendpk rdt_rcv(rcvpkt)&&(损坏(rcvpkt) rdt_rcv(rcvpkt)&&(损坏(rcvpkt) sndpkt= make_pkt(NAK,chksum) sndpkt=make_pkt(NAK, chksum) 等待用于等待用于 rdtrcv(rcvpkt)&& 从从 rdtrcv(rcvpkt)&& 不损坏 (rcvpkt) && below/ below 不损坏(rcvpkt) && has_seq(rcvpkt) has_seg (rcvpkt sndpkt=make_pkt(ACK,chksum) sndpkt=make_pkt(ACK,chksum) udt_send(sndpkt) udt_send(sndokt) rdt_rcv(rcvpkt)&& notcorrupt(rcvpkt)  . 重复的 Pkt 重复的 Pkt && has_seg(rcvpkt) cYae_aa sndpkt =make_pkt(ACK, chksum) udt_send(sndpkt 3-56

rdt2.1： 讨论发送方： 接收方： 邮seq#added到pkt ●！ 必须查看如果已收到分组两个seq.#'' s(O，1） 将suffice. 是重复的 Why？ 状态indicateswhether 0 或 1 是必须查看如果已收到 expectedpkt seq# 说明：接收方可以不know如果它的最后ACK/NAK 已收到OK 状态#是twice 在发送方状态必须 “remember” whether ““ expected" r pktshould有seg#  的0或1 3-57

rdt2.2：aNAK-free协l 相同lfunctionalityasrdt2.1， 使用 ACK instead的NAK.接收方发送ACK用于最后 pkt已收到 K 接收方必须 explicitly includeseq# 的pktbeing ACKed 重复的ACK在发送方results在相同动作asNAK： ： · 重传当前pkt 3-58

rdt2.2： 发送方， 接收方fragments  重复的ACK用于pkt1 rdt_send(数据) ： 重传pkt0 sndpkt = make_pkt(O， 数据， 校验和) udtsend(sndpkt) ratrcvircvpkt)&& 等待用于等待用于编号必须是调用从 ACK 在ACK 上发送方FSM fara gm ent rdt_rcvrcvpkt rdtrcv(rcvpkt)&& (损坏(rcvpkt） 一l Λ 等用于接收方F   从   below ragment  rdt_rcv(rcvpkt)&&notcorrupt(rcvpkt) , 用于rdt2.1， 无 &&has_seq1(rcvpkt) 序列  extract(rcvpkt，数据) 编号在ACk 重复的pkt1 deliver data(数据) ： 重传ACK的pkt1  3-59 udn_sendeendp

rdt3.0：信道与差错和丢包新的assumption： underlying 方法：  发送 waits 信道可以also lose “reasonable” 数量的时间用于分组（数据，ACK） ACK 校验和sen# ACk ra+ronsmits如果无ACK已收到在这个时间但是不enough 如果pkt（或ACK）仅时延ed（不丢失的）： 重传将是重复的. 但是sea#already处理这个接收方必须specifyseq#的pkt heina ACKed 3-60

rdt3.0≤ 发送方 rdt rcv(rcvpkt)&& sndpkt=makepkt(O，数据， 校验和) (损坏(rcvpkt)|l rdt_rcv(rcvpkt) start timee Λ Λ 等待用于等待超时调用0从用于上方 ACKo udt_send(sndpkt) rdt_rcv(rcvpkt) aanotcorruptercvpk rdt_rce(rcvpk && isACK(rcvpkt,1) && notcorruptercvpkt stop timer && isACK(rcvpkt,0) 等待等待用于、 超时用于调用1从 udt_send(sndpkt) ACK1 上方 rdt_send(数据) Λ rdtrcvercvpkt&& ( 损坏(rcvpkt)|l sndpkt=make_pkt(1，数据， 校验和) 3-61

性能的rdt3.0 发送方接收方 rata0 是correct 但是性能stinks e.g.：1 Gbps链路， 15ms prop． 时延， 8000 比特分组： 发送 pkt0 _pkto 8000 比特 acko 发送acko Dtrans L = R rr urkn = 8 microsecs  109bits/sec 发送 pkt1 pkt1 ack1 发送ack1 利用率 - fraction的时间发送 busy rrvark1 发送pkt0 pkt0 发送 U， L/R .008 ack0 发送ack0 0.00027 发送 = 1  RTT + L/R 30.008 如果 =3∩ mcer  1kRnk+ 每—个 30msec：33kB/sec  thruput在 Gbps链路网络协议limits使用的物理resources！ 3-6

ipelnedtl pipelining: 发送方允许多路， “in-flight”  yet-to-be-acknowledgedpkts range的序列编号必须是increased buffering在发送方and/or接收方 data packet. data packets (a) a stop-and-wait protocol in operation (b) a pipelined protocol in operation  两个 generic forms 的pipelined 协议： go-Back-N, selective repeat 3-66

Pipelining： increased 利用率发送方接收方第一个分组比特已传输，t=0 最后比特已传输，t=L/R 第个分组比特到达 RTT 最后分组比特到达发送ACK [ACK 最e比特的3分a到达 ACK到达.发送下—个发送ACK 分组，t=RTT+L/R 利用率由afactor的3！ U， 3L/R .0024  发送 = （ C 0.00081 RTT+L/R 30.008 3-6

Pipelned_tl： 概 Go-back-N SelectiveRebeat naced aa inelne nac d a在 ipele 接收方仅发送 Cumulative rr 尘 ndividualack用于 ack 每个.  分组   Doesn'' tACK分组如果存在''sag ap [aaa 当计时器 expires， 重传所有当计时哭 ovnirac 佳  unacked 分组仅这 unacked分组  68
