# Chapter6 Link Layer-图集-矢量

> 主题库：计算机网络　｜　源文件：Chapter6 Link Layer-图集-矢量.pdf　｜　3,688 字符　｜　68 个片段　｜　更新于 2026-11-13
Net to NeNtransmission path App —十一

App

MAC to MAC

App

A caaata ob? network element process data? 。

tnatisuseaTor

logy: androuters:nodes io と unication channels that connect global IsP ent nodes along communication nks dlins IeSslins S packetframe,encapsulates  anm data-link layer has responsibility of transferring datagram from one node to physically adjacent_ node over a link  5-10

Ethernet IP IP FR Frame relay IP FR IP WLAN WLAN

cau memoRy controller transrission networkada

cpu memory memory cpu controller  controller

application cYa datagram 1 hetwork liNK

R5 S1 R3 目b标： S1 C1获取C2的IP， 填 R1 C1根据IPc₂查路由 R2 跳路由器R1的地址 R4 C1广播查询IPR₁对 [TabmACR C1把MACR1 填入链 S2 C1把链路帧发给S1 S1根据帧头的MAC S3 S4 R1抽取IP，根据IP ，决定下一跳转发 R1广播查询IPR2对 C2 址MACR2 R1把MACR21 填入帧 R2 R2 ?

10101    R

R = remainder[-, D2 ,  G

G 1001

tim

i I 1

spatial layout of nodes

collision detect/abort tImE

data POll master data

cable headend CMTS splitter cable [modem cable modem ISP upstream''Internet frames, TV control, transmitted upsnream w

cable headend MAP frame for CMTS Downstream channeli

islots containing Assigned minislots co

i adapter D -1A-2F-BB-76-09-AD F7-2B-08-53 58-23-D7- 门 0C-C4-11-6F-E3-9

IP MAC 逻辑地址物理地址全局有效局部有效层次化平面型网间寻址网内寻址

←1A-2F-BB-76- 6.7.23 137.19 [a8-23-D  10C-C4-11-6F

oou Targ 08-53 58 13

ARP table in A IPaddr MAC addr

② 08-53 58 13 ② E  re

ARP table in A IPaddr MAC addr

08-53

22  -FF-55 49-E 222.222.222.220 111.111.111.110 11 112 222.222.

22  -FF-55 49- 222.222.222.220 111.111.111.110 11 112 222.222.

MACSrC:74-29-9C-E8-FF MACdest:E6-E9-00-17- IPSrC:111.111.111.1 IPdest:222.222.2

22  FF-55 49- 222.222.222.220 1A-23-F9-CD-06-9B 111.111.111.110 11 112 222.222.

222_ -FF-55 49- 222.222.222.220 1A-23-F9-CD-06-9B 111.111.111.110 11 112 222.222.

IP srC: 111.111.111.1 IPdest:222.222.2 IP [Eh Phy

IP srC: 111.111.111.1 IPdest:222.222.2 IP Eh Phy

22  -FF-55 49- 222.222.222.220 111.111.111.110 11 112 222.222.

IP srC:111.111.111.1 IPdest:222.222.2

RA A R B S3 S1 R1 R3 R5 R6 R7 R2 S2 S4 R

tea.nrame ls aropbea dest. source data preamble( address address (payload) CRC / 2

MAC protocol application and frame format transport  network 100BASE-TX 100BASE-T2 100BASE-FX link 100BASE-T4 100BASE-SX 100BASE-BX physical   copper (twister fiber phvsicallayer

B' C A''

MAC addr interface TTL A 1 60

Source: A Dest: A'' AA

MAC addr interface TTL A 1 60 A'' 4 60

AA'' n C

Source: A Dest:A'' AA''

S4 F

S4 F

to external network c IPsubnet

a pplication ransport datagram network frame link ne physical link frame cYaeaecal switch ing ne: .vvoik datagram ink frame ing phy ysical ca plication transport network link

Science EleCtriCal Engineering

0 15 16

Toutei 16

9 15,  4 8

LYPE preamble dest. source address address data(payload) cCRc 802.1   TYPE preamble dest. source address address data(payload) CRC router  2-byteTagProtocol Identifier Recomput (value:81-00) CRC

15 16

三层 A 交换 B

10 10 10 62 62 22 DTE1 3 2 3 DTE3 6

20. 2 1

R6 X X D R4 R3 R5 D3

entry router(R4) can use different MPLS x  routes to A based, e.g., on source addres R6 (R5,A)->Label1; (R6,A)->Label2 D xC R4 R3 R A P2

RSP-TE R6 XC X D XC R4 R5 modified c link state X X flooding

in out out label label dest interface 10 A 0 in out out 12 D 0 label label  dest interface 8 A 1 10 6 A 1 X 12 9 D 0 R6 X P X 0 -D R4 R3 R5 R2 in out out in out out label label dest interface label label dest interface 6 A 0   8 6 A 0

IP分组 IP分组路由器

路由协议路由表转发信息表 c REQ(65.8) 向向向向 0 0 LERO LSRO 0 0 LSR1 LER1 2\   2   IP Label 表标记信息表 0   让前缀下一跳入端口出端口 1 LSR2 目的地址前缀下一入端口出端 □ 1入标记出标记 8 LSR0 0 1 LER0 65.8 LSR0 0 1 5 8 LSR2 0 2 LSR0 65.8 LSR2 0 2 5 8 8 LER1 0 1 LSR2 65.8 LER1 0 1 8 2 8 0 LER1 65.8 0 2
