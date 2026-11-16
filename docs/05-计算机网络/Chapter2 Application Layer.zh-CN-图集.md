# Chapter2 Application Layer.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter2 Application Layer.zh-CN-图集.pdf　｜　10,647 字符　｜　48 个片段　｜　更新于 2026-11-11
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

JamesF.Kurose KeithW.Ross 第2章应用层说明在使用的thes ePowerPoint幻灯 ： 我们'' remaking这些幻灯片 freelv可用的到所有 (faculty. stulents， readers). They'' re 在 PowerPoint form 因此你见. animations: 和可以添加．修改．和删除slides(包括这个一个) | C MPUTER 幻 T片内容到 suit vour 需要 Thev obviouslv represent a lot的工作在我们的part.在返回用于使用， 我们仅askfoIlowing： NETWORKlNG 如果你使这些幻灯 (e.g.，在a课堂） 这你mention  ATOP-DOWNAPPROACH 源（之后所有，我们’d像people到使用我们的book！) EighthEdition 如果你post任何幻灯片在a网站站点，这你说明这they  是adapted从 （或perhapsidentical到）我们的幻灯片，和说明我们的版权的这个material． 计简机| 网终.  用于arevision历史，见·he幻灯说明用于这个page. 自顶向下方法谢谢和使用愉快！ JFK/KWR 8thedition Jim Kurose, Keith Ross Pearson,2020 J.FKurose 和 K.W. Ross， 所有 Rights Reserved 2-

第2章：应用层我们的目标： conceptual．   实现  M earn   关于 - 协议由  aspects的网络应 examiningpopular - - 应用- 运输层服务模型 HTTP [ep-服务器范式 FTP 对等范式 SMTP/POP3／IMAP DNS reating 网e应用应用套接字 API   运输 2-5

Creating a络应 w riteprorams这： 运在(不同的)端系统通信在网络使e.g.．Web服务器softv是communicates与 [e 无需要到write软件用于network-core 设备应用运输 [络运行用户应用数据链路_ 应用 network-core设备做不物理运输 [Ta 应用在端系统允许用于rapid应用数据链路 P 物理 development,propagation  2-7

进程communicating 进程： ： program运行客户，服务器在．内a主机客户进程： 在.．.内相同主机.．两个进程进程这 initiates 通信使用 inter-process 通信通信 （defined由OS） 服务器进程： 进程这 waits到进程在不同的主机是contacted ● 通信由exchanging [报文  aside： 应用与P2P architectures有 ī ī  客户进程＆服务器进程通信：Process-To-Process 2-11

什么运输服务做应用需要? 数据_ntegrity o叶l 一些应用 （e.g.， 文件传输 Weh   —些应用 （eg multimedia） reauire transactions）require100% 可靠 minimum数量的吞吐量到是数据传输 “effertive” 其他应用 （e.g.，audio） 可以tolerate  其他应用 （“elastic应用”） 牛成 —些丢包使用的whatever吞吐量they获得 timing 安全 — 些应 （e 。 lnternet  encryption， 数据integrity， telenhonv 交互式games) requirelow时延到是 "effective  应用运输 2-20

Internet运输协l服务 TCP服务： UDP服务： 告运问发送 un可靠数据传输之间和接收进程法是穴牛ı. 尘注亡 won t 发送和接收进程 overwhelm接收方做不提供： reliability， 流用室+穴牛. +hr~++le发送方控制， 拥塞控制， [l eoverloaded timing. 吞吐量guarantee. r~~ →间客户和服务器安全，或连接建立， 进程 Q：whybother？Why是存在aUDP？ minimiim 吞吐量  guarantee，  安全 Application  =-  network liNK 2-22 physical

Non-persistentHTTP： 响应时间 RTT(definition):时间用于a smaIl分组到 travel从客户到服务器和back  HTTP 响应时间 ： initiatoTCP  • 一个RTT到initialeTCP 连接连接 —个RTT用于HTTP请求和第一个few RTT  求字节的HTTP响应到返回文件时到文件传输时间 RTT i}传输文件 non-persistent HTTP响应时间 = 文件已收到  +文件传输时间时间时间 2-32

Cookies: keeping “状态 (cont.) 客户服务器 ebay 8734 usua TTP 请求 msg Amazn服务器 cookie 件创建ID  usualHTTP 响应 1678 于户 set-cookie：1678 Create backend ebay8734 entry 数据库 amazon 1678 usualHTTP请求msg 问 Co0kie： 678 C0okie- 特定的动作 USual HTTP 向应 msg —个weeklater： i问 ebay8734 usualHTTP 请求msg amazon1678 c0okie:1678 C0okie 特定的动作 2O23.9.11 usua HTTP 应 msg 2-46

assumptions: avo对兔大 ·1MH特 PublicInternet avo 求 a hrnweere到nriσin服务器：15/sec avg数据速率到hrowcerc·15Mhns  RTT instit_tinnal路器到任aorigin服务器：2Sec  访问链路速率：15Mbps 15 Mbps access link conseauences"  层域利田率·15% 9O% 问题！  访问链路利率=   100MbpsLAN   total时延=lnternet时延+访问时延+局域网时延 =2 + + (15 requests/sec) (1 Mbits/request)/(15 Mbps) = 1 (15 requests/sec)(1 Mbits/request)/(100 Mbps)= 0.15 Institutional network 瓶颈在哪里？ 2-50

第2章：提纲 2.1 原理的 2.6P2P应用网络应用 2.7套接字应用architectures 编程应用requirements 与UDP和 2.2 Web 和 HTTP TCP 2.3 FTP 2.4 electronic 邮件 ■ SMTP,POP3, IMAP 2.5 DNS 2-60

FTP：separate控制数据连接 FTP客户 contactsFTP服务器在端口 コ TCP控制连接 [a器l口 21，使用TCP =   客户 1ε 1 authorized在控制 TCP数据连接连接 FTP 服务器端口20 FTP 客户服务器客户browses远程directorv． 发送 commands在控制连接服冬器onens另—个TcP数据连接到传输另一个文件当服务器接收文件传输控制连接： “查看的 command.服务器opens 2ndTcP数据 hann  连接（用于文件） 到客户  FTP服冬哭维护 “状态之后 transferring在e文件， 服务器closes 当前directory， earlier 数据接 2-62

Electronic 邮件   outaoina 报文队列 Threemajorcomponents □ 用户 mailbox 用户』 用户 agents 代理邮件服务器邮件用户1 服务器 simple邮件传输协议：SMTP  代理 SMTP [邮件用户用户代理服务器代理  SMTP a.k.a.“邮件 reader  Composing. editing，  reading 邮件代理邮件报文服务器 [用户】 e.a..!  Outlook, Thunderbird, iPhone C 代理邮件客户用户   [代理 outgoing. 传入的报文存储的在：  服务器  2-65

Electronic  邮件：邮件服务器邮件服务器： 用户代理 mailboX 包含传入的报文用于邮件用用户用户| 服务器代理报文队列的outgoing（到是已发送） SMTP  邮件用户】  邮件报服务器代理 SMTP 协议之间邮件服务器到 SMTP 发送  |  电子邮件报文用户旧 SMTP 客户：发送邮件：  服务器代理邮件  [abe器 ■ “服务器：接收日邮件服务器『 用户』 [代理 [用户 [代理 2-66

场景：Alice发送报文到Bob 1）Alice使用UA到compose报文 4）SMTP客户发送Alice s报文到 bob@someschool.edu 在TCP连接 2)Alice''sUA发送报文到her 5)Bob''s邮件服务器places报文邮件服务器： 报文placed在在Bob smailbox 报文队列 6）  Bobinvokeshis用户代理到read 3） 客户一侧的 SMTPopens TCP 报连接与Bob s邮件服务器 1用户1 邮件邮件用户1 代理 SMTP 代理服务器 SMTP 服务器 5 Alice s邮件服务器 Bob''s邮件服务器 2-68

邮件报文格式 SMTP： 协议用于exchanging 电子邮件-  msgs 首部 RFC 822： standard用于 text  blank 报文格式： 线路首部线路，e.g., 到：  body [从： Subject: 不同的購从 SMTPMAIL 来自，RCPTTO  ASCllcharacters仅  2-72

邮件访问协议户1 邮件访问 SMTP 户| SMTP 协议代理代理 (e.g-, POP, 4 IMAP) 发送方s邮件接收方’ s邮件服务器服务器 SMTPdelivery/storage到接收 s服务器邮件访问协议： retrieval从服务器 POP：Post 0ffice 协议 [RFC1939]：authorization， download IMAP：Internet邮件访问协议 [RFC1730]： 更多features，includ ng manipulation的存储的msgs在服务器 HTTP：gmail， Hotmail， Yahoo！ 邮件， etc. 2-73

P0P3（更多） 和ＩMAP 更多关於P0P3 IMAP previous示例使用POP3 keeps所有报文在一个place： 在 2 download和删除 mode 服务器 ■ Bobcannotre-reademail如果he 允.许用户到organize报变化客户在folders POP3“download-and-keep":  keeps用户状态acrosssessions:  copies的报文在不同的名称的folders和mappings █ 客户之间报文IDs和folder Popa是statelessacrosssessions 名称 2-75

DNS：域名称系统 [people许多identifiers 域名称系统： SSN，名称，passport# 分布式数据库 implemented在   Internet主机，路由器： hierarchy的许多名称服务器主机， ■ IP地址 （32比特）-用于用于应用-laver协议： 编址数据报名称服务器通信到 “名称 resolve 名称 (address/name ■ .e.a.. www.yahoo.Com - 翻译） [m于_a umans 说明· 核 、Internet功能 Q：如何到map之间IP地址 imnlementedas应用-layer 和名称，和viceversa？ 协议复杂度在网络 s“边缘’ 2-77

DNS：a分布式，hierarchical 数据库根DNS服务器 comDNS服务器 org DNS服务器 eduDNS服务器 umass.edu vahoo.com Dbs.ora Dolv.edu amazon.Com DNs服务器 DNs服务器 DNs服务器 DNs服务器 DNs服务器客 ) wantslP 用于 1 www.amazon.com; 1 it approx： 客 gueries根服务器 3 到findcomDNS服务器客 ● queries.comDNs服务器到获得 amazon.comDNS服务器客到获得 P地e queriesamazon.comDNS服务器 waww.amazon.com

DNS：根名称服务器 contacted由本地名称serve r这可以不resolve名称根名称服务器： contacts权威名称服务器如果名称mapping不已知得mapping [aeternsmapping 到本名称服务器 C. Cogent, Herndon， VA (5 其他站点) d. U Maryland College Park， MD k. RIPE London (17 其他站点) h. ARL Aberdeen. MD j.Verisign， Dulles VA(69 其他站点) i. Netnod， StockhoIm (37 其他站点) e. NASA Mt View. CA m.WIDE Tokvo f. Internet 软件 C. (5 其他站点) PaloAlto，CA(和48其他站点) a.Verisian. Los Angeles CA (5 其他站点) 根名称 “服务器 ” Worldwide b. USC-ISI Marina del Rey, CA I. ICANN Los Anaeles, CA (41 其他站点) a. US DoD Columbus, OH(5其他站点)

DNS  协议，报文 1 查询和应答报文，二者与相同报文格式 2字节 2字节  msg 首部 identification flags  identification:16 比特# 用于 #guestions #answerRRs 查询应答到查询使用相同 # #authorityRRs #additionalRRs fags 查询或应答 questions（变量 # 的 questions） Yrecursion desired recursion可用的 answers(变量 # 的RRs) 应答是权威 authority （变量 # 的 RRs)  additional信息 (变量# 的RRs) 2-90

Insertingrecords 进入 DNS 示例： 新的startup “网络 Utopia 报register 名称 networkuptopia.com 在 DNS registrar(e.g., 提供名称．IP地址o f权威名称服务器 (primary和secondary） registrarinserts两个RRs进入.comTLD服务器 (networkutopia.com, dnsl.networkutopia.com, Ns) (dnsl.networkutopia.com, 212.212.212.1 A)   create权威服务器类型record用于网站．networkuptopia.com； 类型MXrecord用于 networkutopia.com -4

文件distribution：客户-服务器 vsP2P 问题： 如何much时间到distribute文件（大小F）从一个服务器到N对等方？ 对等方 upload/download capa city是 limited resource 服务器pload容量 a 对等 idownload 文件，大小F U1 Us /d1 U2/ /d2 容量服务器 d, n 网络 (与abundant 带宽） ui DN 对方iupload 容量 2-101

文件distribution时间： 客户-服务器服务器传输： 必须 Seeueptially 发送 （upload） N文件  copies: 时间到发送一个复制：F/us [网时间到发送Ncopies：NF/us Ui  客户：每个客户必须download文件复制最小值客户download时间：F/cl小值时问到l dic+rihu+oF 到 N 客户使用 Dc-s>最大值{NF/u,F/d 客户-服务器方法 λ最小值    increasesIinearly在N 2-102
