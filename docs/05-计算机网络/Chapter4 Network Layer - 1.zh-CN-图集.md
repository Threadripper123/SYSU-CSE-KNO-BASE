# Chapter4 Network Layer - 1.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter4 Network Layer - 1.zh-CN-图集.pdf　｜　11,268 字符　｜　47 个片段　｜　更新于 2026-11-11
学大山中立园

大

James F. Kurose |Kelth W. Ross COMPUTER NETWORKING ATOP-DOWN APPROACH EighthEdition

路由器从舞个端端口2 路由器2 □出呢？ 端口3 路由器3 IP报文端口1 NP 端口4 路由器4 端口5 路由器5  —IP报文— 目的IP地址源IP地址协议类型数据 100.0.0.1- KEY 路申表： 转发信息返回NP 100.120.10.100 100.120.10.100 :R2 120.100.11.130 Index 120.100.11.130:R3 100.0.0.1 100.0.0.1 :R4 101121.10.100 101.121.10.100 :R5 TCAI QDR SRAI

软件算法的实现方式 1.线性查找 SRAM AFCD CC 1122 AAAA [2.叉树查找 3  4 5 5 3.HASH查

Keyword: 1 0 1 1 0 * * * TCAM1 0 1 0 1 0 * 0 1 0 1 1 0 1 0 0 1 0 1 2 1 * 0 1 0 0 1 1   3 1 0 1 1 1 0 0  0 Add=4  匹配4 4 1 0 1 1 0 0 1 0 5 1 0 1 0 0 * 1 1

1 2 S3 S1 S2 S3

WI classify arrivals link departures WZ

A类地址 0i 8位 24位 B类地址 10 16位 16位 C 类地址 110; 24位位 D 类地址多播地址 E 类地址保留为今后使用

A类e net-id r 8位 16位 [Ta4 位 D 类地址 1110 多播地

[nntenet协议版本 4 TCP/Pv4 属性常规如果网络支持此功能，则可以获取自动指派的IP设置。否则，你需要从网 e系统Y理员处获得适l的P设置自动获得IP 地址(O) Yb使用下面的IP地址(S)： IP 地址()： . 子网掩码(U)： . 默l网关(D)： 自动获得DNS服务器地址(B） 使用下面的DNS服务器地址(E)： 首选DNS服务器（P）： [TabmDNS服 A 退出时验证设置(L) 高级(V) 确定取消

常规备用配置如果网络支持此功能，则可以获取自动指派的IP设置，否则，你需要从网络系统管理员处获得适当的P设置自动获得IP地址(O) 使用下面的IP地址(S)： IP地址(1)： 子网掩码(u)： 默认网关(D)： 自动获得DNS服务器地址(B) 使用下面的DNS服务器地址（E)： 首选DNS服务器(P）： 备用DNS服务量（A)： □ 退出时验证设置(L) 高级(m).. 确定取消

nternet

skype relay

IPv6报头格式版本号流量等级 (8bit) 流标签 (20bit) c4bit) 数据长度(1 bit) 下报头(8bit) 跳限制(8bit) c  下一报头（8bit） 报头扩展长度（8bit） 扩展报头数据（可变） 下一报头（8bit） 报头扩展长度（8bit） 上扩展报头数据（可变）

第4章 James F.Kurose |Kelth W. Ross [网络层： 说明在使的这些PowerPoint幻灯 ： 我l们re making 这些幻灯片 freelv a\ailable 到所有 (ficultv.Students， readers). They'' re 在 PowerPoint form 因此你见. animations: 和可以添加．修改．和删除slides（包括这个一个）和幻灯内容到 suit vour 需要 ； Thev obviouslv representa lot 的 C MPUTER 工作在我们的part．在返回用于使用， 我们仅askfoIIowing： NE丁WORKING ◼如果你使用这些幻灯片(e.g.，在a课堂)这你mention它们的 ATOP-DOWNAPPROACH 源 (之后所有， 我们'' d 像 people 到使用我们的 book！) Eight hEdition 如果你post任何幻灯在a 站站点.这你说明这thev ■ 是adapted从 (或.perhapsiden tical到) 我们的幻灯片， 和说明我们的版权的这个material． 计算机络. 用于arevision历史， 见tle幻灯片说明用于这个page. ATop-Down方法谢谢和使用愉快！ JFK/KWR 8thedition Jim Kurose, Keith Ross J.F Kurose 和 K.W. Ross， 所有 Rights Reserved Pearson, 2020

第4章：提纲 4.1概述的网络层 4.4Generalized转发和SDN 数据平面匹配控制平面动作 4.2 什么''s inside a 路由器 OpenFlow 例的match-  4.3 IP： Internet 协议 plus-action在动作数据报格式 fragmentation IPv4 编址  [网络地址翻译 IPv6  4-4

第4章：网络层本章目标： 理解网络层服务背后的原理， □ focusing在数据平面： [e层服模型转发与路由 ● 通用转发 ● 在Internet中的实例化与实现 4-5

网络层 TCP 载荷 TCPH IP率载荷 IPH 运输报文段从发送到接收主机应用运输络在发送 ≤—侧∈ 数据链路 encapsulates 6 物理 o  络络报文段进入数据报数据链路数据链路络物理物理数据链路物理 ( 在接收一侧， 络络  ： 交付报文段数据链路数据链路物理物理到运输层 [网络络数据链路数据链路网络层协议在每个主机， 物理 ：络物理数据链路路由器物理应用网络运输路由器数据链路 eXamines 首部络.   字段在所有 IIP 络络物理数据链路数据链路数据链路物理物理数据报 passing 通过它物理  4-6

[网络层：数据平面，控制平面数据平面控制平面 determines加n何数据报是 r ted determines如何数据报  →a路中器alnnoend— l路径从 i 到达的在路由器输入端口是源主机到目的地主机转发到路由器输出端口两个control-plane方法： □ 转发功能传统路中算法： ● implemented 在路由器 cnf+ware-defined l终 (SDN d implemented在远程) 服务器值在到达的 [Taa i部 0111 4-8

distinct （通常远程） 控制器interacts与本地控制agents (CAs) 远程控制器平面数据平面 CA CA CA CA CA 值在到达的 0111 5-10

网络层服务模型： 络服务 Guarantees ？ 拥塞体系结构模型带宽丢包 Order Timing feedback Internet 尽而为 none 无 ;   无无无 （inferred  通过丢包) ATM CBR Constant 是 -是- 是无速率拥塞 ATM VBR auaranteed是 -是 - . 是    . 无速率拥塞 ATM ABR guaranteed无是无是 cYmnanem um ATM UBR none 无是无  CBR：Constant比特速率 ABR：可用的比特速率 VBR： 变量比特速率 UBR：Unspecified比特速率 4-13

路由器体系结构概述两个关键路由器功能： 心运行路由 algorithms/protocol(RIP， OSPF，BGP)  转发数据报从传入的到outgoing链路 2 转发表s计算得到的． 路由路由．管理 pushed到输入端口 PrOcessor r4 控制平面 (软件) 转发数据平面 (hardware) 哪个路由器是best choice a给定的dstlP? hinh-seed [Tab _ 结构 r1 哪个端口goes到给定的路由器？ 路由器输入端口路由器输出端口 4-16

转发表但是什么happens如果目的地地址Range链路接口rang esdon''tdivide上因此 nicely？ 11001000 00010111 00010000 00000000 通过 0 11001000 00010111 00010111 11111111 192.168.1.1~ 192.168.1.5 r1 192.168.1.6 ~192.168.1.10 r2 11001000 00010111 00011000 00000000 192.168.1.11~ 192.168.1.15 r1 通过 1 192.168.1.16~192.168.1.20 r2 通过 2 otherwise 3 4-18

交换通过interconnection网络 ` overcome总线带宽limitations 心 banyan网络，crossbar， 其他 interconnection nets初始时 developed到连接 processors在 crossbar  advanced desian: fraamenting 数据报进入 fixed 长度 cells， 交换机cells通过fabric.  Cisco12000： 交换机 60Gbps 通过interconnection网络 Yaamultistageswitch built from smaller-sized switches4-25

输入端口queuing 结构slower比输入端口combined-〉排队可能occur在输入 aueues 排队时延和丢包由于到输入缓冲区overflow！  Head-of-the-Line(HOL)blockina：aueued数据报在front的队列 preventsothers在队列从moving转发交换机， 交换机, 结构结构输出端 contention： —个分组时间later： 仅一个red数据报可以是 green分组 Iower red 分组是 blocked blocking 4-28

Schedulingpolicies： 优先级优先级scheduling： 发送 high优先级队列 arrivals departures  分组多路 classes， 与 classify 链路 (服务器) 不同的 priorities (waiting 区域) 课堂可能depend在marking ② 或其他首部信息，e.g.IP ③ 4 ⑤ source/dest， 端口编号， etc. arrivals real world示例？ 分组左  1 3 ② 4 5 服务 4-35

Sahedul ngpolicies： 仍然更多 RoundRobin(RR)scheduling 多路 lasses cyclically scan 课堂 ''（ queues， 发送一个完整的分组从每个课堂 （如果可用的） realworld 示例？ 2 1 ③ 4 ⑤ arrivals 左 1 ③ 2 4 5 服务服务器) 5 4-36

Internet网络层主机，路由器网络层功能： 运输层：TCP，UDP IP协议路由协议 ·编址 conventions •路径 selection p数据报格式络 ·RIP,OSPF,BGP ·分组处理conventions 层转发 ICMP协议表 ·差错reporting •路由器 nngnaling" 链路层物理层 4-42

IP 数据报格式 IP协议 version 编号 32比特 total 数据报首部长度长度（字节） Head 米刑的 (字节） ve eren 服务长度用于类型 , 的数据 fragment 16-bit identifier- fragmentation/ 最大值编号时间到 reassembly ubber 首部 remainina hops live 层校验和 (decremented 在每个路由器） 32比特源IP地址   32比特目的地 IP地址 TCP=6； upper层协议 UDP=17 到交付载荷到 options （如果任何） e.a.timestamp， record路由如何数据 muchoverhead？ 经过. specifv 列表的路由器  20 字节的 TCP （变量长度. 通常a TCP 到visit.  20）  字节的IP 或UDP报文段）  =40 定节+应用层 overhead “16 比特 identifier” 是NoTa编号用于reordering 4-43 无reordering 在网络层

第4 章：提纲 4.1概述的络层 4.4Generalized转发和SDN 数据平面匹配控制平面动作  4.2什么' 's s sinsidea路由器O penFlow 示例的match- 4.3IP：Internet协议 plus-action在动作数据报格式 fragmentation IPv4 编址网络地址翻译 IPV6 4-46

IP编址：引言 223.1.1.1 IP 地址： 32-bitidentifier用于主机， 路由器接口 223.12.1 接口： 连接之间 223.1.1.2 223.1.1.4 223.1,2.9 主机/路由器和物理链路路由器 s通常有多路接口 223.1.3.27 223.1.1.3  223..2. 主机通常有一个或两个接口 (e.g.， 有线以太网， 无线 802.11) ● IP 地址 associated 与每个 T223.1.3.1 223.1.3.2 接口 223.1.1.1= 11011111 00000001 00000001 00000001 223 1 1 1 4-47

DHCP： 示例 DHCp DHCP connectinglaptop需要它的lP DHCP UDP DHCP IP 地址. addr É 的 firsthop 路由器. DHCP Eth addr的DNS服务器： 使用 |DHCP  Phy encapsulated 在 IP， encapsulated在 802.1 DHCP DHCP 以太网 168.1.1.1 DHCP UDP DHCP IP  (des+· FFFFFFFFFFFF} □DHCP Eth 路由器网帖广播与DHCP Phy 在局域 ，  已收到在路由器运 DHCP 服务器buiIt进入服务器路由器大 demixed到lIPdemuxed， UDP demuxed至DHCP 4-61

DHCP： 示例 ● DCP 服务器1 formulates DHCP ACK  DHCP DHCP 包含客户 sIP地址 IP DHCP UDP 地址的 firsthop 路由器用于客户， DHCP IP DHCP Eth 名称&IP地址的 DNS服务器 Phy 上到DHCP DHCP DHCP 转发到客户，demuxing D日C UDP 在客 TDHCP IP DHCP Eth 路由器与DHCP Phy 服务器buit进入安现在知道它的IP 地ti  名称和  DHCP 路由器 IPm+ 的DSN月服务器，IP地址的它的 first-hop 路由器 4-6

DHCP：Wireshark输出 （归属局域网) 报文类型· Roo+请求(1) 请求报文类型· Ron+应答(2) 应答 Hardware 型. 以大网 Hardware类型. 以大 Hardware地址长度：6 Hardware地址长度：6 Hos:0 Hops:0 T ansaction ID:0x6b3a1fbt Transaction ID: 0x6b3a11b7 Seconds elapsed: 0 Seconds elapsed: 0 Bootn flaas:Ox000o (llnicast) Rootn flaas:Ox000o(lInicast) 客户 1P地t址·0000 （0000） 客户 1P地t址·1921681101 （192168 1.101） Your(客户)1Pt地t·0000(0000) Yor (客户)1P 地ti·0000 (0000) 下—个服务器 1P 地址·0000 (0000） 下—个服务器1P 地址·192 16811(192.168.1.1） Relav代理IP地t·0000(O000) Relav代理IP地t·0000(00.0.0) 客户 MAC: t地ti · Wis+ron 23·68:8a (00:16:d3:23:68:8a) 客户 MAC t地ti · Wis+rnn 23 S8:8a(00:16:d3:23:68:8a) 服务器主机名称不给定的服务器主机名称不给定的 Boot文件名称不给定的 Boot文件名称不给定的 Maniccnnkie:(Ok) Maaiccnnkie.(Ok) 选项·(+=53I=1) DHCP报类型 = DHCP 请求选项·(+=53I=1) DHCP 报型 =DHCP ACK 选项. (61) 客户 iden+ifier 选项.( (+=54l=4) 服务器 Identifier=192 168 11 长度· 7· 值· 010016D323688； 选项·(+=11=4） 子 Mask= 255255255.0 Hardware 型· 以大网选项·(+=3l=4） 路中器 =192 1681.1 客户 MAC： 地t址· Wistron 3·68·8a(0016·d3·23·68：8a) 选项 . (6） 域名称服务器选项·(+=50 I=4） RenIes+edIPt地ti =192.168.1.101 长度·12· 值·445747F2445749F244574092； 选项·(+=121=5) 主机名称 ="nomad" 1P 地t址·68 8771226. 选项. (55) Parameter 请求列表 IP t地ti. 68 87 73 242. 长度.11. 值.010F03062C2F2F1F21F92R IP1 地址·688764146 1 = 子 Mask· 15 = t域名称选项：(t=15，I=20) 域名称 = "hsd1.ma.comcast.net." 3=路中器.6=域名称服务器 44 = NetBIOS 在 TCP/IP 名称服务器 4-63

IP地e：如何到获得一个？ Q：如何做网络获得子部分的IPaddr？ A获得allocatedportion的它的提供商 lSPs地址空间 ISP''sblock 110010000 0010111000 1000000000000 200.23.16. 0/20 组织 0 11001000 00010111 00010001 0 00000000 200.23.16.0/  123 组织 ！1 11001000 00010111 00010010 00000000 200.23.18.0/ / 123 组织 2 11001000 00010111 00010100 00000000 200.23.20.0/ / 1 23  组织 7 11001000 00010111 （ 000111101 00000000 200.23.30.0/23 4

Hierarchical编址： 更多特定的路由 ISPs-R-Us有a更多特定的路由到组织1 [aa 200.23.16.0/23 [a_a_ “发送me anvthing 200.23.18.0/23 与地址beginning 组织 2 Fly-By-Night-ISP 200.23.16.0/20 Internet 200.23.20.0/23 200.23.16.0/20 组织7 X 200.23.30.0/23 ISPs-R-Us “发送 me anvthing 199.31.0.0/16 与地址beginning 组织 1 199.31.0.0/16 或 200.23.18.0/23” 200.23.18.0/23 4-66

NAT：网络地址翻译 [rest的本地网络 Internet (e.g.， 归属网络) 10.0.0/24 10.0.0.1 10.0.0.4  10.0.0.2 138.76.29.7 10.0.0.3 所有数据报 leana 木地数据报与源或网终有相同单个源日的地在这个网终 NAT1P地址：13876297不同的右1000/24地+ 田干源端口编号源，目的地(asusual) 4-68

NAT：网络地址翻译 motivation：本地络使用仅一个IP地址asfaras utsideworld是concerned  range的地址不needed从ISP：仅 —个IP地址用於所有设备 █ 可以变化地址的设备在本地网络没有 notifying outside world █ 可以变化ISP没有changing地址的设备在本地络设备 inside 本地网络不 explicitly addressable， visible 由 outside world （a安全加) 4-69

NAT：网络地址翻译 NAT翻译表 1 主机 10001 2 NΔT路中哭 WAN一侧addr 局域 ]一侧addr 数据报发送数据报到变化· 源 addr 从 138.76.29.7, 5001 10.0.0.1, 3345 128.119.40.186,80 10.0.0.1，3345 到 138762975001, updates表  S:10.0.0.1, 3345 D:128.119.40.186,80 10.0.0.1 S: 138.76.29.7, 5001 2 D:128.119.40.186, 80 10.0.0.4  10.0.0.2 138.76.29. S:128.119.40186,80 HF -/L D:10.0.0.1,3345 S:128.119.40.186,80 3 LL D：138.76.29.7，5001 4-NAT ！ 路中器 10.0.0.3 3应签 ：到达变化数据报 dest. 地址： dectaddr IP范围 IP范围网络数量 138.76.29.7，5001 138.76.29.7， 5001 到 10.0.0.1，3345 A 10.x.x.x/8 10.0.0.0~10.255.255.255 1 B 172.16-32.X.X/16 172.16.0.0~172.31.255.255 16 4-70 C 192.168.x.x/24 192.168.0.0~192.168.255.255 255

NAT：网络地址翻译实现 NAT路由器必须： outgoing数据报到（NATIP地址，新的端口#） 远程 ~Iian+c/cervers将respond使用 (NATIP地址，新的端口 #） as目的地addr ramamhar (在 N∆T翻译表） 每—个(酒IP地址，端 #) 到（NATIP地址，新的端口#）翻译pair 传入的数#报·renlace (NATID地+ 新的淫□#）左dec+ 定段的每一个佳入的数据报与 orrosponding（源IP 地址，端口#）存储的在NAT表 4-71

NAT：网络地址翻译 16-bit port-number 字段： M 6oooesitAullaotus 连接与a 单个局域网-side [e! NAT是controversial： 路由器should仅进程上到层3  NAT possibility 必须是经过进入 account由应用designers， e.g.， P2P 应用地址shortageshouldinstead是solved由IPv6  -72

Yaatraversal a题客户 wants到连接到服务器与地址 10.0.0.1 服务器地址10.0.0.1本地到局域网（客户可以t使用它as目的地addr） 10.0.0.1 客户 ■ 仅 — 个externally visibleNATed 地址： 138.76.29.7 10.0.0.4 solution1: staticallv confiaure NAT到转发传入的连接请求 138.76.29.7 NΔT 路由器在给定的端口到服务器 ■ e.g.．(123.76.29.7. 端 2500） 总是转发到 10.0.0.1 端 25000 4-73

NaTtraversal问题 solution 2:Univer saI Plug 和 Plav (UPnP) Internet网关设备 （IGD） Protocol． 10.0.0.1 允许NATed主机到： . . 二二   Iearn 公共IP地址 （138.76.29.7） 协议  add/remove端口mappings（与lease times) 路由器 i.e.， automate静态NAT 端口 map  configuration 4-74

如何到生成ahole ● The fireWall refuses 传入的 TcP conn the firewall refuses the第一个传入的UDP 如果和Blocate 背后的它们的NAT， respectively， 如何到通信directly通过ahole？ 192.168.1.10 如aa 到 letthem 对等 AT 通信 AT directly? 对等方对等方对等方 NAT 10.0.0.10 76

IPv6数据报格式优先级：identify优先级之间数据报在流流标签： iidentify数据报在相同 i“ flow. ''.” （concept 的 “ 流 ” 不welldefined). [下一个部：identifyupper层协l用于数据 IPv6报头格式 ver pri 流标签版本号载荷 len 下一个hdr 跳 limit 流量等级 (8bit） 流标签 (20bit） (4bit) 源地+址 (128 比特) 数据长度(16bit) 下一报头(8bit) 跳限制(8bit) [a的ee (128 比特) 源地址(i28bit） 目的地址（128bit） 数据下一报头（8bit） 报头扩展长度（8bit） 扩展报头数据（可变） 32比特 ． 、  下一报头（8bit） 报头扩展长度（8bit） 扩展报头数据（可变） 4-83

第4章：提纲 4.1概述的网络层 4.4Generalized转发和SDN 数据平面匹配控制平面动作 4.2 什么 sinsidea路由器 OpenFlow 示例的 match一  4.3IP： Internet协议 plus-action在动作数据报格式 faa IPv4 编址 [络地e译 IPv6 4-89
