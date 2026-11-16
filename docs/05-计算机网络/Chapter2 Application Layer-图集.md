# Chapter2 Application Layer-图集

> 主题库：计算机网络　｜　源文件：Chapter2 Application Layer-图集.pdf　｜　3,959 字符　｜　26 个片段　｜　更新于 2026-11-11
學大山中立团

山大，

James F.Kurose Keith W.Ross COMPUTER NETWORKING A TOP-DOWN APPROACH P EighthEdition

Application transport network link

高松校长率团与香港大学交流雅动深化两校合作中大新间网讯，（通讯员原文量、吴佳文)9月7日至8日，我校校长真松率团访问香港， 与番港大学校长您用合面，并能行交选座读会，进一步推动深化两腔交流合作，香大学调校长响作家、两校翻关院然和能部门换责人遍30人参会。 2023/09/10 2023/09/10 2023/09/10 15 : 中山大学召开学习赏酷习申山大学参与建警五高松校长率团与香港大学近平制时代中国特色社会海峡两岸中山论坛交源推动深化两校合作

syueducn i 从中入户熟箱USC校博生超结吉校友拼 N Q 中山大学首页中大新闻学校概况院系设置师资队伍人才培养科学研究中大看点百年校庆中新闻看多 2023/09/10

(a)SYN (c) ACK (c (d) HTTP/1.1 304 Not modified i (b): )SYN+ACK (c Connect'' Time  Connection handshake delay Data transfer cYGann

オ Server パ パ い   DNS lookup Time Connect Request Process Response Close

(a) Serial connections Transaction 1 Transaction 2 Transaction 3 Transaction 4 server        m      [Connect- 1 Connect-2 Connect-3 Connect-4 1 Time Client cYa e Transaction 1 Transaction 2 Transaction 3 Transaction 4   Server    m         Client Client Transaction-2

Proy e Cient r ee Client Origiag server

Public Internetl 15 Mbps access link Institutional network

Origin servers PublicInternetl 15 Mbps access link 100Mbps AN Institutional network cache

基于DNS的数据库根DNS GSLB 数据库 ③转发到全局负载→ 2 均衡设备 。 ②查询上一级DNS 本地DNS服务器 ⑤DNS响应 ④服务状态检测 ④服务状态检测 ④服务状态检测 ca ①DNS请求内容服务器3 ⑥用户获取内容内容服务器2 用户内容服务器1

NOT SO EASY

"m人计划P6根服务器全球分布情况国家主根服务器辅根服务器国家主根服务器辅根服务器中国 1 3 西班牙 0 1 美国 1 2 奥地利 0 1 日本 1 0 智利 0 1 印度 0 3 南非 0 1 法国 0 )大亚 0 德国 0 2 瑞士 0 1 [罗 0 兰 0 意大利 0 Baid百科

Domain Name System (response) Transaction ID：Ox9adO #事务ID Flags：Ox8l80 Standardqueryresponse,Noerror #报文中的标志字段 1...............=Response:Message is a response QR字段，值为1，因为是一个响应包 .Ooo O...........= Opcode： Standard query (O) # Opcode字段 .....O.. .... .... = Authoritative: Server is not an authority for domain #AA字段 ......O......... = Truncated:Message is not truncated #TC字段 .... ...1 .... .... = Recursion desired: Do query recursively #RD字段 .... .... 1... .... = Recursion available: Server can do recursive queries #RA字段 ........O......=Z:reserved (O) ..........O.....=Answer authenticated:Answer/authority portion [Yaa not authenticated by the seryer .......O....=Non-authenticated data:Unacceptable ..... . .. 0oo0 = Reply code： No error (O) #返回码字段 .. Questions:1 Answer RRs: 2 aeeitionalRRs

ns 表达式 No.Source Destination Protocol Info L 192.168.59.2 192.168.59.133 DNs Standard query response 0x9ado A baidu.com A 192.168.59.1. 192.168.59.2 DNs Standard auerv 0xa4e2 A extensions.anome.ora UserDatagramProtocolSrcPort 53,DstPort:38226 Transaction ID:I ox9ade Flags: 0x818o Standard query response, No error 1... = Response: Message is aresponse .θoe θ.. = Opcode:Standardquery(o) .O.. = Authoritative: Server is not an authority for domain ..0. = Truncated:Messageisnottruncated  : 1 = Recursion desired:Do query recursively 1... = Recursion available: Server can do recursive queries  .0. = z:reserved(o) ..θ. =Answer authenticated: Answer/authority portion was not au ..0 = Non-authenticated data: Unacceptable 000e =Reply code: No error (0) Questions:1 AnswerRRs:2 Authority RRs:5 Queres

Howtoget to blog.synology.com? On-path S DNS DNS routers server sererB senver 6.66.666.666 : (scam website) urity DNSsenver

假的地址为a.b.c.d 相同银行正常交易被宝。o 网质地址为e.f.g.h 招同银行网质 3 正常DNS服务器鱼DNS服务器 a.b.c.d e.f.g.h 2 招行的IP是多少？ 招行的P是多少？ 电脑软硬件应用网

How toget to blog.synology.com ? iiiiiimii DNSoverHTTPS On-oath DNs iNe DoH server routes serverA seryerB (Cloudfare, 1.23.456.789

对等节点对等节点对等节点对等节点对等节点对等节点对等节点

Youl 1 iTube

迅雷看看 www.kankan.can 网络高清影院

Buffering.…

Treatinganetworkapp application as wite programst n o da t run on iferentend systems coomunicateovernetwo e.g.,web server software communicates with E browsersoftware >> naneedtowritesoftwarefornetwork-core devices application transport network-core devices do not run user applications physical transport network applications on end systems allows for rapid app data link  physical development, propagation ) 2-7

Applicationarchitectures possible structure of applications: •client-server(C/S) 2-8
