# Chapter7 Wireless and Mobile Networks

> 主题库：计算机网络　｜　源文件：Chapter7 Wireless and Mobile Networks.pdf　｜　16,724 字符　｜　50 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall

7-2
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
Chapter 7
Wireless and
Mobile Networks
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)

If you post any slides on a www  site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved

Homework(ver.8, CN)

1, 5, 8, 11, 12

Keywords: CDMA, CDMA encode/decode, CSMA/CA, 
mobile IP, 

Ch. 7: Wireless and Mobile Networks
Background:

# wireless (mobile) phone subscribers now exceeds # wired phone 
subscribers (5-to-1)!

# wireless Internet-connected devices equals # wirelineInternet-connected 
devices

laptops, Internet-enabled phones promise anytime untethered Internet access

two important (but different) challenges

wireless:communication over wireless link

mobility:handling the mobile user who changes point of attachment to network
6-4

Chapter 7outline
7.1 Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and routing 
to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer protocols
7.9Summary
6-5

Elements of a wireless network
6-6
network 
infrastructure

Elements of a wireless network
6-7
wireless hosts

laptop, smartphone

run applications

may be stationary (non-
mobile) or mobile
wireless does notalways 
mean mobility
network 
infrastructure

Elements of a wireless network
6-8
base station

typically connected to 
wired network

relay -responsible for 
sending packets between 
wired network and 
wireless host(s) in its 
“area”
e.g., cell towers,  
802.11 access points 
network 
infrastructure

Elements of a wireless network
6-9
wireless link

typically used to connect 
mobile(s) to base station

also used as backbone 
link 
multiple access protocol 
coordinates link access 

various data rates, 
transmission distance
network 
infrastructure

Characteristics of selected wireless links
W ireless and Mobile  Networks:  7-10
IndoorOutdoorMidrange
outdoor
Long range
outdoor
10-30m
50-200m
200m-4Km4Km-15Km
2 Mbps
4G LTE
802.11ac
802.11n
802.11g
802.11b
3.5 Gbps
600 Mbps
54 Mbps
11 Mbps
Bluetooth
802.11ax
14 Gbps
5G
10 Gbps
802.11 af,ah

11
中国的5G频谱规划

Elements of a wireless network
6-12
infrastructure mode

base station connects 
mobiles into wired 
network

handoff: mobile changes 
base station providing 
connection into wired 
network
network 
infrastructure

Elements of a wireless network
6-13
ad hoc mode

no base stations

nodes can only 
transmit to other 
nodes within link 
coverage

nodes organize 
themselves into a 
network: route 
among themselves

Wireless network taxonomy
6-14
single hop
multiple hops
infrastructure
(e.g., APs)
no
Infrastructure
(No upper network )
host connects to 
base station (WiFi,
WiMAX, cellular) 
which connects to 
larger Internet
no base station, no
connection to larger 
Internet (Bluetooth, 
ad hoc nets)
host may have to
relay through several
wireless nodes to 
connect to larger 
Internet: mesh net
no base station, no
connection to larger 
Internet. May have to
relay to reach other 
a given wireless node
MANET, VANET

Chapter 7 outline
7.1Introduction 
Wireless
7.2 Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and 
routing to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer 
protocols
7.9Summary
6-15

Wireless Link Characteristics (1)
important differences from wired link ....

decreased signal strength: radio signal attenuates as it 
propagates through matter (path loss)

interference from other sources: standardized wireless 
network frequencies (e.g., 2.4 GHz) shared by other 
devices (e.g., phone); devices (motors) interfere as 
well

multipath propagation: radio signal reflects off 
objects ground, arriving ad destination at slightly 
different times
.... make communication across (even a point to point) 
wireless link much more “difficult”
6-16

Wireless Link Characteristics (2)

SNR: signal-to-noise ratio

larger SNR –easier to extract signal from 
noise (a “good thing”)

SNR versus BER tradeoffs

given physical layer:increase power -> increase 
SNR->decrease BER

given SNR:choose physical layer that meets 
BER requirement, giving highest thruput

SNR may change with mobility: 
dynamically adapt physical layer 
(modulation technique, rate) 
6-17
1020
3040
QAM256 (8 Mbps)
QAM16 (4 Mbps)
BPSK (1 Mbps)
SNR(dB)
BER
10
-1
10
-2
10
-3
10
-5
10
-6
10
-7
10
-4

BER
Wireless network characteristics
Multiple wireless senders and receivers create 
additional problems (beyond multiple access):
6-18
A
B
C
Hidden terminal problem

B, A hear each other

B, C hear each other

A, C can not hear each other
means A, C unaware of their 
interference at B
A
B
C
A’s signal
strength
space
C’s signal
strength
Signal attenuation:

B, A hear each other

B, C hear each other

A, C can not hear each other 
interfering at B

BER
Code Division Multiple Access (CDMA)

unique “code”assigned to each user; i.e., code set 
partitioning

all users share same frequency, but each user has own 
“chipping”sequence (i.e., code) to encode data

allows multiple users to “coexist”and transmit 
simultaneously with minimal interference (if codes are 
“orthogonal”)

encoded signal= (original data) X (chipping sequence)

decoding:inner-product of encoded signal and chipping 
sequence
6-19

BER
Code Division Multiple Access (CDMA)
6-20
A
B
C
D
A发送数据d
A
, A的CDMA地址码c
A
B发送数据d
B
, B的CDMA地址码c
B
c
A
与c
B
正交：
c
A
⊙c
B
=0；
c
A
⊙c
A
=1；
c
B
⊙c
B
=1；
发送数据：
A: d
A
⋅c
A
；
B: d
B
⋅c
B
；
A+B: d
A
⋅c
A
+ d
B
⋅c
B
接收数据：
C：(d
A
⋅c
A
+ d
B
⋅c
B
) ⊙c
A
= d
A
D：(d
A
⋅c
A
+ d
B
⋅c
B
) ⊙c
B
= d
B
d
A
d
B
d
A
d
B
+
20241223

CDMA encode/decode
6-21
slot 1
slot 0
d
1
= -1
111
1
1
-
1
-
1
-1
-
Z
i,m
= d
i
.
c
m
d
0
= 1
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
slot 0
channel
output
slot 1
channel
output
channel output Z
i,m
sender
code
data
bits
slot 1
slot 0
d
1
= -1
d
0
= 1
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
111
1
1
-
1
-
1
-1
-
slot 0
channel
output
slot 1
channel
output
receiver
code
received
input
D
i 
= 
S
Z
i,m
.
c
m
m=1
M
M

CDMA: two-sender interference
6-22
using same code as 
sender 1, receiver recovers 
sender 1’s original data 
from summed channel 
data!
Sender 1
Sender 2
channel sums together 
transmissions by sender 1 
and 2
1，1，1，-1，1，-1，-1，-1

1，-1，1，1，1，-1，1，1
0

Chapter 6 outline
7.1Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3 IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4Cellular Internet Access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing and routing 
to mobile users
7.6Mobile IP
7.7Handling mobility in cellular 
networks
7.8Mobility and higher-layer protocols
7.9Summary
6-23

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-24
all use CSMA/CA for multiple access, and have base-station and ad-hoc network versions

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-25
802.11n (WiFi4)
802.11ac(WiFi5)

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-26
802.11ax (WiFi6)802.11be (WiFi7)

IEEE 802.11 Wireless LAN
W ireless and Mobile  Networks:  7-27
OFDM

802.11 LAN architecture
6-29

wireless host communicates with base 
station
base station = access point (AP)

Basic Service Set (BSS) (aka “cell”) 
in infrastructure mode contains:
wireless hosts
access point (AP): base station
ad hoc mode: hosts only
BSS 1
BSS 2
Internet
hub, switch
or router

802.11: Channels, association

802.11b: 2.4GHz-2.485GHz spectrum divided into 
11
channels at different frequencies

AP admin chooses frequency for AP

interference possible: channel can be same as 
that chosen by neighboring AP!

host: must associatewith an AP

scans channels, listening for beacon frames
containing AP’s name (SSID) and MAC address

selects AP to associate with

may perform authentication [Chapter 8]

will typically run DHCP to get IP address in AP’s 
subnet
6-30
BSS

802.11: passive/active scanning
6-31
AP 2
AP 1
H1
BBS 2
BBS 1
1
2
3
1
passive scanning:
(1)beacon frames sent from APs
(2)association Request frame sent: H1 to selected AP 
(3)association Response frame sent from  selected AP 
to H1
AP 2
AP 1
H1
BBS 2
BBS 1
1
2
2
3
4
active  scanning: 
(1)Probe Request frame broadcast from H1
(2)Probe Response frames sent from APs
(3)Association Request frame sent: H1 to selected AP 
(4)Association Response frame sent from selected AP 
to H1

IEEE 802.11: multiple access

avoid collisions: 2
+
nodes transmitting at same time

802.11: CSMA -sense before transmitting

Don’t collide with ongoing transmission by other node

802.11: nocollision detection!

difficult to receive (sense collisions) when transmitting due to weak received signals 
(fading)

Can’t sense all collisions in any case: hidden terminal, fading

goal: avoid collisions:CSMA/C(ollision)A(voidance)
6-32
space
A
B
C
A
B
C
A’s signal
strength
C’s signal
strength

IEEE 802.11 MAC Protocol: CSMA/CA
802.11 sender
1 
if sense channel idlefor DIFS  then
transmit entire frame (no CD)
2 if sense channel busy then
start random backofftime
timer counts down while channel idle
transmit when timer expires
if no ACK, increase random backoffinterval, repeat 2
802.11 receiver
-
if frame received OK
return ACK after SIFS (ACK needed due to hidden 
terminal problem) 
6-33
sender
receiver
DIFS
data
SIFS
ACK
Short InterframeSpace（SIFS）
DCF InterframeSpace(DIFS):在DCF协议中，节点在开始发送数据
之前需要监测信道是否空闲。如果信道已经空闲，则节点仍需等待
DIFS段时间才开始发送数据；而如果在DIFS时间段内任一时刻信道
被监测为忙，则节点不得不推迟它的数据发送。

Avoiding collisions (more)
idea:allow sender to “
reserve
”channel rather than random access of 
data frames: avoid  collisions of long  data frames

sender first transmits 
small
request-to-send (RTS) packets to BS using 
CSMA

RTSs may still collide with each other (but they’re short)

BS broadcasts clear-to-send CTS in response to RTS

CTS heard by all nodes

sender transmits data frame

other stations defer transmissions 
6-34
avoid data frame collisions completely 
using small reservation packets!

Collision Avoidance: RTS-CTS exchange
6-35
AP
AB
time
DATA (A)
reservation collision
defer

802.11 frame: addressing
6-36
frame
control
duration
address
1
address
2
address
4
address
3
payloadCRC
226662
6
0 -2312
4
seq
control
Address 2: MAC address
of wireless host or AP 
transmittingthis frame
Address 1: MAC address
of wireless host or AP
to 
receive
this frame
Address 3: MAC address
of routerinterface to 
which AP is attached
Address 4: used only in 
ad hoc mode
Why？

6-37
Internet
router
H1
R1
AP MAC addrH1 MAC addrR1 MAC addr
address 1
address 2
address 3
802.11frame
R1 MAC addr  H1 MAC addr 
dest. address 
source address 
802.3frame
802.11 frame: addressing


H1 remains in same IP 
subnet: IP address can 
remain same

switch: which AP is 
associated with H1?

self-learning(Ch. 5): switchwill 
see frame from H1 and 
“remember”which switch 
port can be used to reach H1
6-42
802.11: mobility within same subnet
H1
BBS 2
BBS 1
BER

Rate adaptation

base station, mobile dynamically 
change transmission rate 
(physical layer modulation 
technique) as mobile moves, SNR 
varies 
6-43
802.11: advanced capabilities
QAM256 (8 Mbps)
QAM16 (4 Mbps)
BPSK (1 Mbps)
1020
3040
SNR(dB)
BER
10
-1
10
-2
10
-3
10
-5
10
-6
10
-7
10
-4
operating point
1. SNR decreases, BER increase 
as node moves away from base 
station
2. When BER becomes too 
high, switch to lower 
transmission rate but with 
lower BER

BER
6-44
power management
node-to-AP: “I am going to sleep until next beacon frame”
AP knows not to transmit frames to this node
node wakes up before next beacon frame

beacon frame: contains list of mobiles with AP-to-mobile frames 
waiting to be sent
node will stay awake if AP-to-mobile frames to be sent; otherwise sleep again 
until next beacon frame
802.11: advanced capabilities

BER

less than 10 m diameter

replacement for cables (mouse, 
keyboard, headphones)

ad hoc: no infrastructure

master/slaves:

slaves request permission to send (to 
master)

master grants requests

802.15: evolved from Bluetooth 
specification

2.4-2.5 GHz radio band

up to 721 kbps
6-45
M
radius of
coverage
S
S
S
P
P
P
P
M
S
Master device
Slave device
Parked device (inactive)
P
802.15: personal area network

Chapter 6 outline
7.1 Introduction 
Wireless
7.2Wireless links, characteristics

CDMA
7.3IEEE 802.11 wireless LANs 
(“Wi-Fi”)
7.4 Cellular Internet access

architecture

standards (e.g., GSM)
Mobility
7.5Principles: addressing 
and routing to mobile 
users
7.6Mobile IP
7.7Handling mobility in 
cellular networks
7.8Mobility and higher-
layer protocols
7.9Summary
6-46

Mobile device  
(UE)
Base station
(eNode-B)  
Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-49
radio access
network
all-IP Enhanced Packet Core 
(EPC)
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
to 
Internet
...
Mobile device:
smartphone, tablet, 
laptop, IoT, ... with 4G 
LTE radio
64-bit International 
Mobile Subscriber 
Identity (IMSI), stored on 
SIM (Subscriber Identity 
Module) card
LTE jargon: User 
Equipment (UE)

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-50
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Base station:
at “edge” of carrier’s network
manages wireless radio 
resources, mobile devices in 
its coverage area (“cell”)
coordinates device 
authentication with other 
elements
similar to WiFi AP but:
•active role in user mobility
•coordinates with nearly 
base stations to optimize 
radio use
LTE jargon: eNode-B

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-51
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Home Subscriber Service 
stores info about mobile 
devices for which the HSS’s 
network is their “home 
network”
works with MME in device 
authentication 

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-52
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Serving Gateway (S-GW), 
PDN Gateway (P-GW)
lie on data path from mobile 
to/from Internet
P-GW
•gateway to mobile cellular 
network
•Looks like nay other internet 
gateway router
•provides NAT services 
other routers:
•extensive use of tunneling

Elements of 4G LTE architecture
W ireless and Mobile  Networks:  7-53
Mobility 
Management 
Entity (MME)
Serving Gateway (S-GW)
Home Subscriber 
Service (HSS)
PDN gateway (P-GW)
...
to 
Internet
Mobile device  
(UE)
Base station
(eNode-B)  
Mobility Management 
Entity
device authentication 
(device-to-network, 
network-to-device) 
coordinated with mobile 
home network HSS 
mobile device management: 
•device handover between cells
•tracking/paging device location
path (tunneling) setup from 
mobile device to P-GW

Chapter 7 summary
Wireless

wireless links:

capacity, distance

channel impairments

CDMA

IEEE 802.11 (“Wi-Fi”)

CSMA/CA reflects wireless channel 
characteristics

cellular access

architecture

standards (e.g., GSM, 3G, 4G LTE)
Mobility

principles: addressing, 
routing to mobile users

home, visited networks

direct, indirect routing

care-of-addresses

case studies

mobile IP

mobility in GSM

impact on higher-layer 
protocols
6-85
The End of Chapter 7

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
