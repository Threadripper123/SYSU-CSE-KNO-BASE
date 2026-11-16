# Chapter1 ComNet and Internet-图集

> 主题库：计算机网络　｜　源文件：Chapter1 ComNet and Internet-图集.pdf　｜　3,224 字符　｜　15 个片段　｜　更新于 2026-11-11
JamesF.Kurose Kelth W.Ross COMPUTER NEIWORKING ATOP-DOWNAPPROACH

Application SMTP FTP TFTP DNS SNMP  BOOTP layer Transpont SCTP TCP UDP layer ICMP ICMP Network IP layer ARP RARP Data link layer UnderlyingLANorWAN technology Physical layer

em em

内导体绝缘体编织层编织层外护套

PACKET DATA HEADER Sender Receiver

Physical Connectionissetup W hen call connection is made SwitchingOffices

3 R2 3 Sender R1 R3 2 Receiver 4 5 R6 3 4 R4

average queueing delay La/R 1

Computer Networking  X 谢逸中山大学·计算机学院 2024.Fal

JamesF.Kuroselkelthw.Ross Chapter 1 Introduction Abnote on theuse of these PowerPoint slides: We re m aking these slides freely available to all (faculty, students Yeaders .They''re in PowerPoint form so you see the animations; and can add, modify, and delete slides (including this one) and COMPUTER Slide content to suit your needs. They obviously represent a lotof work on our part. In return for use, we only ask the following: NETW ORKiN G If you use these slides (e.g., in a class) that you m ention their ATOP-DOWNAPPROACH Saurce af er all wed like people to use our bookl If you post any slides on a www site, that you note that they are adapted from (or perhaps identical to) our slides, and note our copyright of this material. Computer Networking: For a revision history, see the slide note for this page. A Top-Down Approach Thanks and enjoy! JFK/KWR 8th edition All material copyright 1996-2023 Pearson, 2020 J.F Kurose and K.W. Ross, All Rights Reserved 1-2

Computer Network is different from other field Computer networks are artificial , not natural p which leads to the existence of a large number of The network is large, complex, a , maki describe using general m athem atical m odels; It involves low-level physical net , and hiah-levelbehavior underst ; Data-driven analysis is onlyapart of it, while Only bymastering the   Can right o speak。

The first question: W hat happens to the netw end-to-endcommunicat Content(text,multimedia)  Co puternetor How toachieveit?

• Ch1(ver7, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33 ( C Ch1(ver8, CN/EN): 6, 7, 8, 9, 10, 13, 14, 20, 25, 27, 29, 31, 33 Practice：18，19 下题目为必题  Keywords : protocol, network edge, access network, Packet Switching, Circuit Switching, delay, Encapsulation

Acloser look atlnternet struct moblle network server r rs o ofte ter n ir in da at te ta Ce er nt te t ers local lor ACCeet O s C regiona wirer e ad wre ir ks e ele ess ti C rr u m ca n n s torne retwork Network core: [aa entemprse metaork

“RealInternetdelays,routes traceroute:gaia.cs.umass.edu to www.eurecom.fr 3 delay measurements from 10 de.fr1.fr.geant.net (62.40.96.50) 113''ms 121 ms 114 ms 11 renater-gw.fr1.fr.geant.net(62.40.103.54) 112 ms 114 ms 112 ms 12 nio-n2.cssi.renater.fr(193.51.206.13) 111 ms 114 ms 116 ms 13 nice.cssi.renater.fr(195.220.98.102) 123 ms 125 ms 124 ms 14 r3t2-nice.cssi.renater.fr (195.220.98.110) 126 ms 126 ms 124 ms 15 eurecom-valbonne.r3t2.ft.net(193.48.50.54) 135ms 128 ms 133 ms 16 194.214.211.25(194.214.211.25) 126 ms 128 ms 126 ms 17 18 *** * means no response(probe lost, router not replying) 19 fantasia.eurecom.fr(193.55.113.142) 132 ms 128 ms 136 ms * Do some traceroutes from exotic countries at www.traceroute.org
