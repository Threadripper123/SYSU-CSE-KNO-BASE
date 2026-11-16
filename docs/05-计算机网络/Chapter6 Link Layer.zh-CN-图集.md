# Chapter6 Link Layer.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter6 Link Layer.zh-CN-图集.pdf　｜　3,119 字符　｜　36 个片段　｜　更新于 2026-11-11
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

计算机网络大谢逸中山大学・计算机学院 2024.秋季 ●

JamesF.Kurose|keithW.ross 第6章链路层和局域网说明在使用的这些PowerPoint幻灯片： 我们remaking这些幻灯片 freelv可用的到所有 (facultv. students， readers). Thev'' re 在PowerPointform因此. 你见.animations； 和可以添加．修改．和删除幻灯片（包括这个一个）和幻灯片内容到 suitvour needs. Thev obviouslv representalot的 C MPUTER 工作在我们的part. 在返回用于使用，我们仅askfoIlowing： NETWORKING 如果你使用这些幻灯 (e.g.，在a课堂）这你mention它们的  ATOP-DOWNAPPROACH 源 (之后所有， 我们'' d 像 people 到使用我们的 book!) Eighth Edition  如果你 post任何幻灯片在a网站站点，这你说明这they 是adapted从 （或perhapsidentical到）我们的幻灯片， 和说明 [bl a o at _al 用于arevision历史，见幻灯片说明用于这个page. ATop-Down方法谢l和使用愉快！JFK/KWR 8thedition 所有材料版权 1996 2n23 JimKurose，KeithRoss J.F Kurose 和 K.W. Ross， 所有 Rights Reserved Pearson, 2020

链路层，局域网：提纲 6.1  引言，服务 6.2 2 差错检测，纠正 6.3  多路访问协议 6.4  局域网编址，ARP 以太网 6.5 链路虚拟化：MPLS 交换机 6.6 数据中心网络 VLANS 6.7a天在生存期的aWeb 请求 5-5

A 如何做发送数据到B？ 如何做每个网络element进程数据？  B 7

链路层：引言在a相同子节点是considered到有  uniformstandard物理地址这是用于用于 addressing. MAC地址是用于用于编址在相同子网 9

链路层：引l terminology: 主机和路由器： 节点 3 c A 通信信道这连接全局ISP  aaiacent 节点along通信路径：链路有线链路无线链路局域网层 2 分组： 帧， encapsulates < 数据报 [数据链路e aresnonsiiitv的到物理上 adjacent 节点在a链路 5-10
