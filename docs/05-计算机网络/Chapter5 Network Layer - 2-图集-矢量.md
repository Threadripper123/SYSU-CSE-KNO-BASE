# Chapter5 Network Layer - 2-图集-矢量

> 主题库：计算机网络　｜　源文件：Chapter5 Network Layer - 2-图集-矢量.pdf　｜　4,700 字符　｜　69 个片段　｜　更新于 2026-11-13
Routing Algorithm control plane -- Local forwarding data table plane header output 0100 3 0110 2 0111 2 1001 1

Remote Controller plaNE data plaNE CA CA CA CA CA

5 Ev3 3 Ew3 2 Eu 12 3 1 EZ3 1 -X y- T 2 1

_5 2 uu 2 3 1 EZ X

0 u 7,u 3,u 5,u ∞ ∞ 1 uw 6,w 5,u 11,w ∞ 2 uwX 6,w 11,w 14,x Xr 3 UWXV 10,V 14,× 4 uwxvy 12,y 5 uwxvyz 5/ 4 8 notes: w 2  construct shortest path tree by tracing predecessor nodes 3 7  ties can exist (can be broken arbitrarily) 4-17

0 u 2,u 5,u 1,u 8 8 1 ux 2,u 4,× 2,x 2 Uxy 2,u 3,y 4,y 3 UXyv 3,y 4,y 4 UXyWw 4,y 5 UXyVWZ

5 MvU 3 Mw3 2  uy 2 1 EZ3 3 1 -X Y 2 1

U- CZ

1 |+X  0    3 0 0 X

2+x +×

2+× B- 1+x1 0 0

2+× 1+x1 B- 0 0

5 V,x Ev3 3 1Mw3 2 U-  2 3 1 Z

a. mby 8 1 1 A d mf) 1 1 1 1 1 -9- Thl 1 1

ta. Mbl 8 1 1 1 dV 1 ウfl 1 1 1 1 1 g- Chy il

conaptte eonbpgte- conoplte 8 confouite eoneptte eomtpute conpuite comoute comipute

a. Mb- 8 1 1 1 dV +fl 1 1 1 1 1 1 -g- Mh→ i_

ompite boBpite- bomput 2 domelute doepute dormpit dormetite- eorm plite- eompyit

Cal Mb- 8 1 1 1 1 1 1 1 1 -9- hhU Ci 1 1

Cal Mb- 8 1 1 1 1 1 1 1 1 -91 ThU miン 1 1

-a_ by 8 1 Q: what is new DV computed in Edy coenpute Ef 1 1 1 1 Cgし EhU 1 1il 1

8 1 t=1 t=2 1f l 1 t=3 t=4

Y A 1 × 50 z

50

4 EYU 1 ×ン 50 Z T=O:

MYU 4 1 50 [T=1:

3C 3a 2C 3b AS3 2a 2b 1C AS2 M1a S1b 1d AS1 ntra-AS Routing algorit m Forwarding table

3c 3a AS3 c 1c a E1a 2b KS E1b AS1 E1d AS2

boundary backbone ASes area3 internal area 1 routers

3C 3a 2c 3b AS3 2a 2b 1C AS2 1a S1 E1d AS1

cYanmanecnionsl 2b 2a 20 2d 1a 1c 2.a 3c ASZ 1d 3d

2b ty 2a 20 y 1b 3b 2d 1a 1c 2a 3c tm AS2 Is 1d 3d

AS3 3b AS1 1b 3a 3c 1a 1c AS2 2b 3d 1d BGPadvertisement: 2a -- -- 2c AS3,X 2d 5-73

AS3 AS1 3b 1b 1a 1c、eBGP AS2 2b 3d 1d AS3,× AS2,AS3,X 2a 2c iBGP

DGroatnaavertlsememt AS 3 3b AS3,X 1a AS2 2 allin AS3X rface 1d AS3,× a,1d dest| interface recall: 1a, 1b, 1d learn via iBGP from 1c: "path to X goes through 1c'' .. . .. 1c 1 at 1d: OSPF intra-domain routing: to get to 1c, use interface 1 X 1 at 1d: to get to X, use interface 1

BGr patn aavertisement AS3 3b 1a AS2 2b  1d 2a

dest| interface 1C 2 X 2

AS3 AS1 3b 1b 3a 3c 1a 1c AS2 2b 3d 1d 152 AS3,X AS1,AS3,X 2a 2c 201 263 asPFlinkweights 2d

B B w x A

provider network

provider network

5 2 3 2 W V 2

5 TCP UDP 2 1 3

mable 3. controlplan routing access load control balance functions jons external to d planeswitch Remote Controller control plaNe data CA 2.( .control, dataplane CA CA CA CA Separation forwarding penFlow)

SDN-controlled switches

routing access control

routing load access control balance northbound API SDN Controller (network operating system )

s controled switc es

routing access load control balance Interface,abstractions for network control apps network RESTful graph APl intent statistics flow tables Network-wide distributed. robust state management Link-stateinfo host info switch info a te OpenFlow SNMP Communication to/from controlled devices

SDN-controlledswitches

routing -  access control

OpenFlow

OpenF Flow

Dijkstra''s link-state Routing network RESTful graph API  intent 3 Link-state info host info switch info ② OpenFlow SNMP 6 S2 S1 $4 S3

[Taaf REST AP一 Network Basic| Network Service Functions seervice apps topology switch stats Access Control manager manager manager forwarding host ServiceAbstractionLayer (SAL) OpenFIOw 1.0 SNMP OVSDB

Network control apps northbound REST APl Intent abstractions, protocols hosts paths flow rules topology ONOS devices links statistics distributed core

device link host flow packet southbound OpenFlow Netconf OVSDB protocols

1 probes, 3 probes c2 robes

in the loop agent data managing serverlcontroller data managed device  agent data ment agent data managed device query, managed devicè levice;  agent data cata agent data managed device managed device

agent ddata [Ymanagin server/controller data managed device  agent data agent data managed device managed device n agent data agent data managed device g managed device

Ymanagnng cYanaGY request response agent data managed device

trap msg agent data managed device

Message type Function GetRequest manager-to-agent: “get me data” GetNextRequest (data instance, next data in list, GetBulkRequest block of data). SetRequest manager-to-agent: set MIB value Response Agent-to-manager: value, response to Trap Agent-to-manager: inform manager

PDU Agent Trap Specific type Time Enterprise Type Name Value 4 Addr code  (0-7) stamp  Trap header /  Trap info    /     SNMP PDU      UDPHdr UDPpayload

PDU Err0r type Request Status Error Name  Name Value (0-3) ID Value (0-5) Index

capabilities exchange:<hello> <rpc> <rpc-reply>

<notification> <rpc>_ <rpc-reply>

[Ymanagen server/controller data

<rpc> <rpc-reply>
