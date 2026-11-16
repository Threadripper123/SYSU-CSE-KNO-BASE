# Chapter7 Wireless and Mobile Networks-图集

> 主题库：计算机网络　｜　源文件：Chapter7 Wireless and Mobile Networks-图集.pdf　｜　3,834 字符　｜　22 个片段　｜　更新于 2026-11-11
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

Computer Networking  大 X   谢逸中山大学·计算机学院 2024.Fal

Chapter 7 JamesF.Kurose|KeithW.Ross Wireless and A note on the use of these PowerPoint slides: Weremaking these slides freely available toall( aculty, students readers). They''re in PowerPoint form so you see the animations; CO MPUTEP and can add, modify, and delete slides (including this one) and NETW ORKIN G slide content to suit yourneeds. They obviously representa lotof work on our part. In return for use, we only ask the following: A TOp-DOwN APPROAcH EighthEdition  Y youuse heseslides e.g i a lass thatyoumention the source (after all, wed like people to use our book!) If you post any slides on a www site, that you note that they are adapted from (or perhaps identical to) our slides, and note our copyright of this material. Computer Networking: For a revision history, see the slide note for this page. A Top-Down Approach Thanksandenjoy! JFK/KWR 8hedition am uroseeithRoss All material copyright 1996-2023 J.F Kurose and K.W.Ross,All Rights Reserved Pearson, 2020

• Keywords: CDMA, CDMA encode/decode, CSMA/CA, obile P,

Ch. 7: Wireless and Mobile Networks  # wireless(mobile)phone subscribersnow exceeds# wiredphone  # wireless Internet-connected devices equals # wireline Internet-connected laptops, Internet-enabled phones promise anytime untethered Internet access  two important (but different) challenges mobility: handling the mobile user who changes point of attachment to network

7.2 Wireless links, characteristics 7.5 Principles: addressing and routing cDmA  to mobile users 7.3 IEEE 802.lIwirelessLANs 7.6 MobileIP c Wi-Fi ) . Handlingmobilityin ellular a4 CellularlnternetAccess networks architecture 7.8 Mobility and higher-layer protocols standards (e.g., GSM) 7.9 Summary 6-

SU-MIMO MU-MIMO IEEE8 802.11 Wireless LAN 802.11n(WiFi4) 802.11ac(WiFi5) OFDM 8OMhz信道拥绑 802.11ac 256QAM FEC Wave1(2013) 802.11n MINO WiFi4关键技术 MINO 802.11aC WiFi5 关健技术 MU-MIMO 40Mhz 802.11ac Wave2(2015) 160/80+80Mhz捆练 shortgi Wireless and Mobile Networks: 7- 25

Thanks Q&A Email:xieyi5@ mail.sysu.edu.cn https:l/cse.sysu.edu.cn/content/2462
