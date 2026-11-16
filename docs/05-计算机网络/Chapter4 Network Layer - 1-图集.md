# Chapter4 Network Layer - 1-图集

> 主题库：计算机网络　｜　源文件：Chapter4 Network Layer - 1-图集.pdf　｜　2,944 字符　｜　19 个片段　｜　更新于 2026-11-11
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

Chapter 4: network layer chaptergoals: understand principles behind network layer services, ■ focusing on data plane: networklayerservicemodels forwardingversusrouting how a router works ● generalized forwarding ● instantiation, implementation in the Internet 4-5

Networklayer TCPpayload TCPH IP payload IPH transport segment from sending to receiving host application transport an Sending sideencapsulates datalink o physical network network seo  into datagrams data link data link network  physical physical  data link 一 physical network network On receiving side, delivers segm ents data link  datalink physical physical to transport layer network network data link data link networklayerprotocolsin eVeryhost physical physical work cataln router physical application network _transport router examines header fields in all IP data link network k network physical data link data link data link F physical physical 4-6

buffer management: datagram drop: which| packet to add, drop when switch buffer link R fabric layer line buffers are full protocol termination ● tail drop: drop arriving packet queueing (send) scheduling ● priority: drop/remove on priority basis maaking whichpackets tomarkto Abstraction: queue signal congestion(ECN, RED) acket 1 aacket departuresl arrivals queue link (waitingarea) (server network Layer: 4-32

[aoaeaol The firewall refuses ;  incoming TCPconn. the firewall refuses the firstincoming UDP. • If A and B Iocate behind their NAT, respectively, how to caommunicate directlyviaa hole? 192.168.1.10 Howtolet them eer NAT Communicate NAT directly? peer peer peer NAT 10.0.0.10 76
