# Chapter7 Wireless and Mobile Networks.zh-CN-图集

> 主题库：计算机网络　｜　源文件：Chapter7 Wireless and Mobile Networks.zh-CN-图集.pdf　｜　9,132 字符　｜　47 个片段　｜　更新于 2026-11-11
学大山中文

自

James F. Kurose Keith W.Ross COMPUTER NETWORKING ATOP-DOWN APPROACH EighthEdition

5G频率域频率范围名称 FR1 450MHz-6GHz Sub6G FR2 24GHz-52GHz mmWave

高频/低频候选频谱 FR1 3.4-3.6GHz 3.3-3.4GHz 4.4-4.5GHz 4.8-4.99Ghz FR2 24.25-27.5GHz 37-43.5GHz

2 中国移动 China Mobile 60 6 2 0 2 00 2 2 002 002 054 5 2 5 1 52 52 52 0 0 中电 CHICO 3.5G 100M（室内) 100M 100M 3300 3400 3500 3600 中国移动广电军方 China Mobile 人 4.9G 100M 6ON 40M 4800 4900 4960 5000

data d = 1 zim=dicm bits d =.1 channel, m code 1 2 2  1 data =1 8 =1 bits coe d ZIMST d L m=1 d=-1 slot1 slot0  received received input input receve code 同同

标准发布年份频段物理层技术编码式空间流数信道带宽(MHz) 理论速率 802.11 1997 2.4GHz IR、 FHSS和 DSSS .  20 2Mbps 802.11b 1999 2.4GHZ DSSS/CCK .  22 11Mbps 802.11a 1999 5GHz OFDM   20 54Mbps 802.11g 2003 2.4 GHz OFDM 64-QAM 20 54Mbps -  2.4GHZ450 Wi-Fi4 802.11n 2009 2.4 GHz或5 OFDM GHz DSSS/CCK 64-QAM 4 20、40 Mbps 5GHz:600 802.11aC OFDM Wave1 2013 5GHz SU-MIMO 64-QAM 4+4 20、 40 3.74 Gbps iFi5 802.11ac Wave2 2015 5GHz OFDM 8 20、 40、 80、 6.9Gbps 下行MU-MIMO 256-QAM 160 和80+80 Wi-Fi6 20、40、80、 802.11aX 2019 2.4GHz或5 OFDMA 2.4GHz:1.15 GHz 下行MU-MIMO 1024-QAM 4+8 Gbps 上行MU-MIMO 160和80+80 5GHz:9.6Gbps 2.4GHz OFDMA Wi-Fi7 802.11be 2024 MU-MIMO 4096-QAM 16 20、40 80 liA6GhPO1 5GHz或6GHz CMU-MIMO 16Q、3205 [T

OFDM FEC 802.11n WiFi4关键技术 MINO 40Mhz shortGi

80Mhz信道捆绑 802.11ac 256QAM Wave1(2013) MINO 802.11ac WiFi5关健技术 MU-MIMO 802.11ac Wave2(2015) 160/80+80Mhz担练

SU-MIMO MU-MIMO

1024QAM OFDMA多址 802.11ax WiFi6关键技术 MU-MIMO 空间复用 TWT

链路多频多链路频谱分配协同 1RU/User N*RU/User High 空间流单AP 多AP协同调制 8Streams 16Streams 关注程度带宽 1024QAM 4096QAM 160MHz 320MHz LoW

报文报文带宽子载波 RU OFDM OFDMA W eless and Mobile Networks:7-26

ej2x0 OFDM e-j2a d 积分 e)2nf e−j2π d, d,d,d2…d /井 s() 积分 do,d,d2 dx− 信并 1 Ⅰ 1 积分

计算机网络大 X   谢逸中山大学・计算机学院 2024. 秋季

第 7 音 James F.Kurose Keith W.Ross 无线和移动网络s [说ba在使的些PowerPoint 灯我们''remaking 这些幻灯片 freely 可用的到所有 (faculty， students， readers). Thev'' re 在 PowerPoint form 因此你见.animations； C MPUTER 和可以添加，修改.和删除幻灯片（包括这个一个)和 NETWORKING 幻灯片内容到suitvourneeds. They obviouslyrepresent aot的工作在我们的part. 在返回用于使用， 我们仅askfoIIowing： ATOP-DOWNAPPROACH Eighth Eesition . 如果你使用这些幻灯片 (e.g.，在a课堂)这你mention它们的源(之后所有，我们''d像people到使用我们的book！) 如果你 post任何幻灯片在a网站站点，这你说明这 they 是adapted从(或perhapsidentical到) 我们的幻灯片， 和说明 [我l的版权的 Ymaterial l算l a 用于arevision历史， 见幻灯说明于这个page． ATop-Down 法谢谢和使用愉快！ JFK/KWR 8thedition JimKurose，KeithRoss 所有材料版权 199h >n23 J.FKurose 和 K.W. Ross， 所有 Rights Reserved Pearson, 2020

关键词： CDMA， CDMA encode/decode，CSMA/CA, 移动P

Ch.7：无线和移动网络s  # 无线 (移动）phonesubscribers现在exceeds# 有线phone subscribers(5-到I)!  # 无线 Internet j 连接的设备 equals #  i wireline lnternet-连接的 [设备 laptops, Internet-enabled phones promise anytime untethered Internet 访问两个重要 （但是不同的）challenges 无线： 通信在无线链路 ◼ 移动性：处理移动用户who变化点的attachment到网络

本章 7提纲 7.1引言无线移动性 7.2： 无线链路，特性 7.5原理·编t和路由 CDMA 到移动用户s 7.3 IEEE802.11 无线局域  7.6 移动IP (“Wi-Fi”) 7.7外理移动性在蜂窝 7.4 蜂窝 Internet 访问网络体系结构 7.8移动性和higher 层协议 standards e.g GsM .9 总e 6-

Elements的a无线网络 [络基础设施 6-6

Elements的a无线网络无线主机 a aptop,smartphone  运行应用可能是stationarv(non 络移动）或移动基础设施开线做不总是 mean 移动性 6-7

Elements的a无线网络基站通堂连接的到有线网络  relav-resnonsible用干 [发送分a之间 [ae [a线e和基础设施无线主机（s）在它的 “区域” ■ e.g.. 小区 towers. 802.11 接入点s 6-8

Elements的a无线网络无线链路 (C)  通堂用干到连接移动(s)到基站米 F  also用于as骨干链路 [Ta 多ciatl 基础设施 coordinates链路访问 a various数据 .率传输距离 6-9

5G频率域频率范围名称 FR1 450MHz-6GHz Sub6G FR2 24GHz-52GHz mmWave 高频/低频候选频谱 FR1 3.4-3.6GHz 3.3-3.4GHz 4.4-4.5Ghz 中国的5G频谱规划 4.8-4.99Ghz FR2 24.25-27.5GHz 37-43.5GHz 中国移动中用电 chicom 3.5G 100M(室内) 100M 100M 3300 3400 3500 3600 中国移动 chn obile 广电军方 4.9G 100M 6ON 40M 11 4800 4900 4960 5000

Elements的a无线网络基础设施mode  其站连接 [mabilese入有线络切换 · ·移动变化络 [abbee 基础设施连接讲入有线网络 6-12

Elements ；的a无线网络无基站s 传输到其他节点在.．内链路 cOverage themselves #aa 网络·路中之间themselves 6-13

无线网络 taxonomy 单个跳多路_hops 主机连接到主机可能有到基础设施基站 (WiFi． relav 通过 several (e.g，APs) WiMAX. 蜂窝) 无线节点到哪个连接到连接到larger arger_nternet ]nternetnmesh网e 无基站无无连接到larger 无基站.无基础设施连接到larger Internet.  可能有到 relav到 reach其他 (无upper网络 ) Internet(Bluetooth， a给定的无线节点 caae MANET,VANET 6-14

无线链路特性（1) 重要 differences从有线链路 .. A 果 darraacad信强 . radi，信 a++enuatesas它  [aropagates通过matter (路a损耗） Hザ从甘lcouroe ctanlardinedH线终frenenriac(ea 2ΛGH 世亨中甘他设备 (e.g.， phone)； 设备 (motors) interfereas well 多a佳播. rodi，信品refleo+，nff arnind 3 到达的ad目的地在slightly  不同的times Reetea Seamteria hgdowing Bs 生成通信arrncc (ovon a点到点) pathloss Diffractio muitipath 无线链路 much 更多 “ difficult” 6-16

无线链路特性(2) S SNR： 信号到 noiseratio larger SNR - easier 到 extract 信号从 10-1   noise (a “ 好的 thing )  102    SNR 与 BERtradeoffs 103 给定的物理层： :  增 power > 增 SNR->decreaseBER  10-4   给定的SNR:选择物理层这meets 10-5 ■  aaRrequirement,givinghighestthruput 0  SNR可能变化与移动性：·  dvnamicallv adapt 物理层 10-7 10 20 30 40 (modulationtechnique， 速率) SNR(dB) QAM256(8Mbps) QAM16(4 Mbps) BPSK(1Mbps) 6-17

无线网络特性多路无线senders和receiverscreate additional problems (beyond多路访问）:  7B C s信号 C s信号 B 强度强度 A 隐藏终端问题空间  B,hear每个其他信号attenuation：  B，Chear每个其他  B，hear每个其他  C，可以不hear每个其他  B，Chear每个其他 means C可以不hear每个其他  Clnaware的它们的干扰在 B interfer ing 在 B 6-18

信道输出Z M Zim=dCmI 数据 d0=1 1 1 1 发送方比特 d1 =-1 -1-1-1 =1 -1 -1-1-1 码 111 时隙 1 时隙 0 1 11 信道信道 -1 -1-1-1 -11-1 输出输出时隙1 时隙0 m=1 M 已收到 d0 = 1 输入 -1-1- -1 -1 - -1 d1=-1 码 111 时隙1 时隙 0 1 1 11 信道信道 -1 -1-1-1 接收输出输出时隙1 时隙0 6-21

CDMA：两个发送方干扰信道sumstogether [transmissions 由发送方 data Z =dc d  i,m 发送方1 bits d=-1 m 和2 code channel, 1 im 1 data =1 d6=1 发送 2 bits code m dc 1, 1, 1,-1, 1,-1,-1,-1 1,-1,1,1,1,-1,1,1 M d ∑ Zi,m = Cm 2 m=1 0 d=1 使用相同码as 2 d=-1 slot1 slot0 发送 1， 接收 recovers received received  发送方 s 原始数据 input input eeVe 从summed信道 code 国数据！ [6-22

第6 章：提纲 .1引言移动性无线 7.5原理：编址和路由到移动用户s 7.2无线链路，特性 7.6移动IP ■ CDMA 7.3IEEE802.11 无线局域 7.7 处理移动性在蜂窝 Wi-Fi”） 网络 7.4 蜂窝 Internet 访问 7.8 移动性和 higher 层协议体系结构 6-23

IEEE802.11 1  无线局域网标准发布年份频段物理层技术编码式空间流数信道带宽(MHz) 理论速率 802.11 1997 2.4 GHz IR、 FHSS和 DSSS 20 2Mbps 802.11 b 1999 2.4 GHZ DSSS/CCK   22 11 MbpS 802.11a 1999 5GHz OFDM . . 20 54Mbps 802.11g 2003 2.4GHz OFDM 64-QAM 20  54MbpS 2.4GHZ:450 Wi-Fi4 802.11 n 2009 2.4GHz或5 OFDM GHz 20、 40 Mbps DSSS/CCK 64-QAM 4 5GHz:600 802.11ac Wave1 2013 5GHz OFDM SU-MIMO 64-QAM 4+4 20、40 3.74Gbps i-Fi5 802.11ac OFDM Wave2 2015 5GHz 下行MU-MIMO 256-QAM 8 20、 40、 80、 6.9Gbps 160和80+80 OFDMA Wi-Fi6 802.11ax 2019 2.4 GHz或5 20、 40、 80、 2.4GHz:1.15 GHz 下行MU-MIMO 1024-QAM 4+8 Gbps 上行MU-MIMO 160 和80+80 5GHz:9.6GbpS 2.4 GHz OFDMA Wi-Fi7 802.11be 2024 20、40 80 MU-MIMO 4096-QAM 16 li46GhPO1 5GHz或6GHz CMU-MIMO 1 160、 3205  所有使用 CSMA/CA 用于多路访问， 和有基站和ad hoc网络 versions 儿 x rur4~as.7 24

SU-MIMO MU-MIMO 1EEE 802.11 无线局域 802.11n(WiFi4) 802.11ac(WiFi5) OFDM 80Mhz信道拥绑 802.11ac FEC Wave1(2013) 256QAM 802.11n MINO WiFi4关键技术 MINO 802.11aC WiFi5关键技术 MU-MIPAO 40Mhz 802.11ac Wave2(2015) 160/80+80Mhz捆绑 ShortGi 无线和移动网络s：7 25

IEEE 802.11 无线局域网  802.11ax(WiFi6) 802.11是 (WiFi7) 连路 1024QAM 频谱分配多频多链路协同 1*Ru/User N* Ru/User High 空间流单AP 多AP协同 OFDMA多址调制 8Streams 16Streams 关注程度带宽 1024QAM 4096QAM LoW 160MHz 320MHz 802.11aX 报文 WiFi6 关键技术 MU-MIMO 报文空间复用带宽子载波 RU TWT OFDM OFDMA  无线和移动网络s：7 26

IEEE802.11 三无线局域  _ OFDM e j2afo1 e-1240 d。 积分 d d,d,d2… d 积分 1井 s() 信道并 do,d1,d2… dx− 1 1 e e −j2x−1t 积分 FDm:FrequencyDivisionMultiplexing Spectrum Subcarrier Bandwidth Bandwidth 无线和移动络s：7 27

802.11局域网体系结构无线主机communicates与基站 ■ 基站=接入点(AP) Basic服务集合 (BSS)(aka“小区'') 在基础设施mode包含： hub.交换机无线主机或路由器接入点 （AP）： 基站 ε adhocmode： 主机仅 BSS1 BSS2 6-29

802.11： 信道，      association  802IIh·24GH7.2485GH7 频谱divided进入信道在不同的freauencies APadminchooses 频率用于 AP [r可能信可相e， 这chosen 由neighboring AP 主机： 必须 associate 与AP crane信道 lietenina用于hoaron帖包含AP ’s名称（ ）和MAC地址 ， selectsAP到associate与可能performauthentication[第 8 章l 归通运 DHCP到获得IP地址在AP S  BSS 6-30

802.11： 被动/主动扫描 BBS1 BBS2 BBS1 BBS2 0 AP1 ① ① AP2 AP2 AP1 2 2 ② ③ 3 4 H1 H1 被动扫描： 主动扫描 （I)beacon帧已发送从APs (1)Probe请求帧播从H1 (2) association请求帧已发送：H1 到selected AP (2)Probe响应帧已发送从 APs (3) association 响应帧已发送从 selectedAP (3)Association  请求帧已发送： H1 到 selectedAP 到H1 (4)Association响应帧已发送从selectedAP 到H1 6-31

IEEE802.11： 多路 a avoid碰撞s：2 节点 transmitting 在相同时间 802.Il CSMA-sense 之前 transmitting Don''tcollide与ongoing传输由其他节点无碰撞检测！ difficult到接收 (sense碰撞s） 当transmitting由于到weak已收到signals (fading) 可以’tsense所有碰撞s在任何case：隐藏终端，fading a 标 ：avoid 碰撞s CsMA/C(ollision)A(voidance) B B强度号 s 强度C' 信信号 's 空间 6-32

IEEE 802.11 MAC协议： CSMA/CA DCF之间的帧空间(DIFS)： EDCF协议中，节点在开始发送数据之前需要监测信道是否空闲。如果信道已经空闲，则节点仍需等待 802.11发送方 DIFS段时间才开始发送数据： 而如果在DIFS时间段内任一时刻信道被监测为忙，则节点不得不推迟它的数据发送。 13 如果sense信道idle用于DlFS则发送方接收方传输entire帧( 无CD 2 如果sense信道busy则 DlFS 开始随机 backoff时间计时器 counts down while 信道 idle 数据传输当计时器expires 如果无ACK， 增大随机 backoffinterval，repeat2 802.11 接收方 SIFS 如果帧已收到OK ACK 返回ACK之后SIFS( 由於到隐藏终端问题） Short之间的帧空间 (SIFS)

碰撞Avoidance：RTS CTS交換 B AP RTS(A) RTS(B) RTS(A) CTS(A) CTS(A) DATA (A) defer 时间， ACK(A) ACK(A) 6-35

第6章：提纲 7.1引言无线移动性 7.2无线链路，特性 7.5 原理：编址 CDMA 和路由到移动 7.3IEEE802.11 无线局域网用户 (“Wi-Fi”) 7.6移动IP 7.4 蜂窝 Internet访问 7.7处理移动性在体系结构蜂窝网络 standards(e.g.， GSM) 7.8 移动性和 higher 层协议 7.9总结 6-46

第7章总结无线移动性无线链路： 原理·编址 ● 容量，距离路由到移动用户s 信道 impairments 归属， 被访络s  CDMA 直接，indirect路由  IEEE802.LL(“Wi-Fi”） care-的地址 ■ CSMA/CA reflects 无线信道  Case Studies 特性移动IP 蜂窝访问 ● 移动性在GSM ■ 体系结构层 ● imnac:t 在 higher standards(e.g.,GSM,3G,4GLTE) 协l 端的第7章 6-85

Q& 电子邮件：xieyi5@mail.sysu.edu.cn https://cse.sysu.edu.cn/content/2462
