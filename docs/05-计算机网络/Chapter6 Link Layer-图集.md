# Chapter6 Link Layer-图集

> 主题库：计算机网络　｜　源文件：Chapter6 Link Layer-图集.pdf　｜　3,675 字符　｜　34 个片段　｜　更新于 2026-11-11
學大山中立团

James F. Kurose Kelth W. Ross COMPUTER NETWORKING ATOP-DOWNAPPROACH EighthEdition

datagram datagram Y bits in D'' N OK  detected ? error ←d data bits — DE DCD' 'E DC'' bet-errerronelink

aarity ddatabits bit 0111000110101011 0

row parity + d1,1 d1,] d1,j+1 d2,1 d2,j d2,j+1 di,1 d i,j1 column paarity di+1,1 di+1] dit1,j+1 101011 101011 111100 1 1100 parity error 011101 011101 101010 101010 no errors parity error correctable single bit error

dbits → rbits bit D:data bits to be sent R:CRC bits pattern D2 XORR mathematica formula

G 101011 10 01101110 0 00 1001 101 000 D 1010 1001 110 000 110 0 1001 1010 1001 011 R

1

will overlap will overlap withstartof with end of Ti''s frame -- i fr ame 4 i''s  T

node I frame to1 t t+1 0 0

space to

collision detect/abort timE

space A

CDMA Code Division Multiple Access CODE TIME

STATKN TRANSCEVER TAP 区 INERAE ARE INTERFACE CONTECICER THE THE尺

计算机1 计算机2 计算机3 集线器计算机4 计算机5 计算机6 计算机7 计算机8

CRSC

802.1QTRUNK ylan 20 Switch1 Dotl.q Switch2 Dotl.q 2 3 4 A B C D

传统HUB的工作过程 HUB 土豆，土豆信已收到地瓜的。 收下是我的信， 不是给我不是给我的，丢弃的，丢弃

传统Switch的已知地址报文转发过程哦,4口连的送信人叫土豆，而1口连设备叫地瓜， 的设备叫土豆，OK，转 OK,送4口! 1口！ 收到，发一个回哦，这封信是给执地瓜的。 这两人都不叫地瓜,都不会收到信

三层交换机功能模型 0:16 10. 23 ETH2:10.110.2.254/24 二— ETH1:O 124/24 10. 0 . /2 10.110.1.69/24 10.110.1.88/24 10.110.2.200/24 : Q. 2 3 G:10.110.1.254 G:10.110.1.254 G:10.110.2.254

传统Switch的地址未知报文广播过程哦，不知道地找我的人叫土豆，而1口瓜在哪里，发连的设备叫土豆，OK， 寻人启事转1口！ 土豆，土豆哦，这封信是给地瓜的。 地瓜在这不是找我不是找我的，丢弃的，丢弃

三层交换机中的路由和二层交换 。二层交换引擎：实现同一网段内的快速二层转发三层路由引擎：实现跨网段的三层路由转发

●传统三层技术对每个报文进行处理，并基于第三层地址转发报文。这一方法称为报文到报文(PxP)

基于流交换的三层交换技术不在三层处理所有报文的的方法称之为流交换（FS） 第一报文后续报文

节点A 节点B DTE1 3 2 3 1 DTE3 DTE2 4 2 DTE4 节点A路由表节点B路由表呼叫入出出端口号LCN 呼叫入端口号LCN 端口号LCN 端口号 LCN 1 3 10 2 62 1 3 62 1 22 2 4 12 2 63 2 3 63 2 23

Spine switch Fabrc Switch o Top-of-rack switch

X X  X Tier-2switches TOR switches 1 2345 6 8 - Figure 6.31 Highly interconnected data network topology

GOOGLe Google Search Tm Feeling Lucky Pretere  Advertising Programs - Business Soiutiors - About Google 2009 P a

UMASSCS

Computer Networking   谢逸中山大学・计算机学院 2024.FaII

JamesF.Kurose|KeithW.Ross Chapter6  The Link Layer and LANs A note on the use of these PowerPoint slides: W e''re making these slides freely available to all (faculty, students, readers). They''re in PowerPoint form so you see the animations; and can add, modify, and delete slides ; (including this one) and slide content to suit your needs. They obviously represent a lot of CO MPUTER work on our part. In return for use, we only ask the following: NETW ORKlNG  If you use these slides (e.g., in a class) that you mention their A TOP-DO w N APPROAcH source (after all, we''d like people to use our book!) Eighth Edition  If you post any slides on a www site, that you note that they are adapted from (or perhaps identical to) our slides, and note our copyright of this material. Computer Networking: For a revision history, see the slide note for this page. ATop-Down Approach 8th edition All material copyright 1996-2023 Jim Kurose, Keith Ross J.F Kurose and K.W . Ross, All Rights Reserved Pearson, 2020

Link Iayer, LANs: outline 61introduction services 6.2 error detection, correction aa multiple access protocols 6.4 ANS Ethernet 6.5 link virtualization: MPLS switches 6.6 data center networking VLANS 6.7 a day in the life of a web Yecuest 5-5

A dayin thelife.. HTTP request/reply Googl  HTTP  Web pagefinally (! ) displayed Geoge Seac re Feag Lucty  TCP IP Phy XC a HTTPrequest sent into Yautet _ atagramcontainingHTTP www.google.com [ IP   Eth  web serverrespondswith   Phy HTTP reply (containing web XC page) web sServer  IP datagram containingHTTP 64.233.169.105 Yeplyrouted backto client 5-129
