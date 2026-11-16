# Chapter6 Link Layer

> 主题库：计算机网络　｜　源文件：Chapter6 Link Layer.pdf　｜　45,166 字符　｜　129 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall

Chapter 6
The Link Layer and LANs
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:

If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)
If you post any slides on a www site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2023
J.F Kurose and K.W. Ross, All Rights Reserved
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020

Homework

Ch8：2、5、11、13、14、15、17、18、21、26、29、31、32

Keywords: VLAN,  CSMA/CD/CA, CRC, TDM, CDM, ALOHA, 
ARP, self-learning, ATM, MPLS, PPP,Ethernet,
MAC 
protocols

5-4
Chapter 6: Link layer
our goals:

understand principles behind link layer 
services:

error detection, correction

sharing a broadcast channel: multiple access

link layer addressing

local area networks: Ethernet, VLANs

instantiation, implementation of various link 
layer technologies

5-5
Link layer, LANs: outline
6.1 introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

6
链路层网络：
应用层网络：
传输层网络：
网络层网络：
App
App
App
App
App
App
App
App
Point to point
Logical channel
Net to Net transmission path
MAC to MAC

7
A
B
How does A send data to B?
How does each network element process data?

Link layer: introduction

Each network interface card has an address named physical 
address

TV-net, telephone-net, data-net, ...

Physical address is different from IP address

Physical addresses do NOThave a uniform format and standard. It 
may be 12bits, 48bits, or others.
8

Link layer: introduction

In a same subnet, the nodes are considered to have a 
uniform standard physical address that is used for 
addressing.
9
MAC address is used for 
addressing in the same subnet

5-10
Link layer: introduction
terminology:

hosts and routers: nodes

communication channels that connect 
adjacent
nodes along communication 
path: links

wired links

wireless links

LANs

layer-2 packet: frame,encapsulates 
datagram
data-link layerhas responsibility of 
transferring datagram from one node 
to 
physically adjacent
node over a link
global ISP

5-11
Link layer: context

datagram transferred by different link 
protocols over different links:

e.g., Ethernet on firstlink, frame relay 
on intermediatelinks, 
802.11 on lastlink

each  link protocol provides different 
services

e.g., may or may not provide rdtover 
link
Ethernet
Frame relay
WLAN
ETH
IP
FR
IP
WLAN
IP
FR
IP

5-12
Link layer services

framing, link access:

encapsulate datagram into frame, adding 
header, trailer

channel access if shared medium

“MAC”addresses used in frame headers 
to identify source, dest

different from IP address!

reliable delivery between adjacent nodes

we learned how to do this already (chapter 
3)!

seldom used on low bit-error link (fiber, 
some twisted pair)

wireless links: high error rates

Q:why both link-level and end-end reliability?
IPMAC Hdr
...
...

5-13

flow control:

pacing between adjacent sending and receiving nodes

error detection: 

errors caused by signal attenuation, noise. 

receiver detects presence of errors: 

signals sender for retransmission or drops frame 

error correction:

receiver identifies and correctsbit error(s) without resorting to 
retransmission

half-duplex and full-duplex

with half duplex, nodes at both ends of link can transmit, but not 
at same time
Link layer services (more)
Why no congestion control?
...
...

5-14
Where is the link layer implemented?

in each and every host

link layer implemented in “adaptor”(aka 
network interface cardNIC) or on a chip

Ethernet card, 802.11 card; Ethernet 
chipset

implements link, physical layer

attaches into host’s system buses

combination of hardware, software, 
firmware
controller
physical
transmission
cpu
memory
host 
bus 
(e.g., PCI)
network adapter
card
application
transport
network
link
link
physical

controller
physical
memory
CPU
Interfaces communicating
controller
physical
cpumemory
application
transport
network
link
link
physical
application
transport
network
link
link
physical
sending side:
encapsulates datagram in frame
adds error checking bits, reliable 
data transfer, flow control, etc.
receiving side:
looks for errors, reliable data 
transfer, flow control, etc.
extracts datagram, passes to 
upper layer at receiving side
link
h
link
h
link
h
link
h
link
h
link
h
datagram
datagram
datagram
Link Layer: 6-15

16
S3
CCC
S4
CCC2
S2
S
C
C
C
S1
C1
C
C
目标：C1→C2
C1获取C2的IP，填入IP报头
C1根据IP
C2
查路由表得到下一
跳路由器R1的地址IP
R1
C1广播查询IP
R1
对应的物理地
址MAC
R1
C1把MAC
R1
填入链路帧头；
C1把链路帧发给S1;
S1根据帧头的MAC
R1
转发给
R1；
R1抽取IP，根据IP
C2
查路由表
，决定下一跳转发给IP
R2
；
R1广播查询IP
R2
对应的物理地
址MAC
R2
R1把MAC
R2
填入帧头，转发给
R2
R2  ?
S1
R2
R1
R3
R4
R5

5-17
Link layer, LANs: outline
6.1introduction, services
6.2 error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

Link Layer5-18
Error detection
EDC= Error Detection and Correction bits (redundancy)
D    = Data protected by error checking, may include header fields 
•Error detection not 100% reliable!
•protocol may miss some errors, but rarely
•larger EDC field yields better detection and correction
otherwise

Link Layer5-19
Parity checking
single bit parity:

detect single bit errors
two-dimensional bit parity:

detect and correct single bit errors
0
0

5-20
Internet checksum 
(review)
sender:

treat segment contents as 
sequence of 16-bit integers

checksum: addition (1’s 
complement sum) of 
segment contents

sender puts checksum 
value into UDP checksum 
field
receiver:

compute checksum of 
received segment

check if computed 
checksum equals 
checksum field value:

NO -error detected

YES -no error detected. 
But maybe errors 
nonetheless?
goal:
detect “errors”(e.g., flipped bits) in transmitted packet 
(note: used at transport layer only)

5-21
Cyclic redundancy check

more powerful error-detection coding

view data bits, D, as a binary number

choose r+1 bit pattern (generator), G

goal: choose r CRC bits, R, such that

<D,R> exactly divisible by G (modulo 2) 

receiver knows G, divides <D,R> by G.  If non-zero remainder: 
error detected!

can detect all burst errors less than r+1 bits

widely used in practice (Ethernet, 802.11 WiFi, ATM)
R=f(D)


generator
22
Cyclic redundancy check
6543    2    1    0
6   4   2
1  0  1  0  1  1  1+
1
( )
x  x  x  x  x  x  x
x x x x
G x
             
    

4   3
( )1
?
G x x x x   

1        0       1        0       1       1       1
2
0
2
1
2
2
2
3
2
6
2
4
2
5

5-23
CRC example
want:
D
.
2
r
XOR  R = nG
equivalently:
D
.
2
r
= nGXOR  R 
equivalently:
if we divide D
.
2
r
by G, 
want remainder R to 
satisfy:
R
= remainder[           ]
D
.
2
r
G
R
r=3
Sender: 
101110011
001110011
111110011
100110011
...
DR
余数的位数一定要是比除数位数只能
少一位，哪怕前面位是0，甚至是全
为0（整除时）也都不能省略

5-24
Link layer, LANs: outline
5.1introduction, services
5.2error detection, correction 
5.3 multiple access protocols
5.4LANs

addressing, ARP

Ethernet

switches

VLANS
5.5link virtualization: MPLS
5.6data center networking
5.7a day in the life of a web 
request

5-25
Multiple access links, protocols
two types of “links”:

point-to-point

PPP for dial-up access

point-to-point link between Ethernet switch, host

broadcast (shared wire or medium)

old-fashioned Ethernet

upstream HFC

802.11 wireless LAN
shared wire (e.g., 
cabled Ethernet)
shared RF
(e.g., 802.11 WiFi)
shared RF
(satellite) 
humans at a
cocktail party 
(shared air, acoustical)

5-26
Multiple access protocols

single shared broadcast channel 

two or more simultaneous transmissions by nodes: interference 

collisionif node receives two or more signals at the same time
multiple access protocol

distributed algorithm that determines how nodes share channel, i.e., 
determine when node can transmit

communication about channel sharing must use channel itself! 

no out-of-band channel for coordination

5-27
An ideal multiple access protocol
given:broadcast channel of rate R bps
desiderata:
1. when one node wants to transmit, it can send at full rate R.
2. when M nodes want to transmit, each can send at average rate 
R/M
3. fully decentralized:

no special node to coordinate transmissions

no synchronization of clocks, slots
4. simple

5-28
MAC protocols: taxonomy
three broad classes:

channel partitioning

divide channel into smaller “pieces”(time slots, frequency, code)

allocate piece to node for exclusive use

random access

channel not divided, allow collisions

“recover”from collisions

“taking turns”

nodes take turns, but nodes with more to send can take longer turns

5-29
Channel partitioning MAC protocols: TDMA
TDMA: time division multiple access

access to channel in "rounds" 

each station gets fixed length slot (length = pkttrans time) 
in each round 

unused slots go idle 

example: 6-station LAN, 1,3,4 have pkt, slots 2,5,6 idle 
1
3
4
1
3
4
6-slot
frame
6-slot
frame
fr
equency bands

5-30
FDMA: frequency division multiple access 

channel spectrum divided into frequency bands

each station assigned fixed frequency band

unused transmission time in frequency bands go idle 

example: 6-station LAN, 1,3,4 have pkt, frequency bands 2,5,6 idle 
fr
equency bands
FDM cable
Channel partitioning MAC protocols: FDMA

5-31
Random access protocols

when node has packet to send

transmit at full channel data rate R.

no a prioricoordination among nodes

two or more transmitting nodes ➜“collision”,

random access MAC protocolspecifies: 

how to detect collisions

how to recover from collisions (e.g., via delayed retransmissions)

examples of random access MAC protocols:

ALOHA

slotted ALOHA

CSMA, CSMA/CD, CSMA/CA

Link Layer5-32
Pure (unslotted) ALOHA

unslottedAloha: simpler, no synchronization

when frame first arrives

transmit immediately

collision probability increases:

frame sent at t
0
collides with other frames sent in [t
0
-1,t
0
+1]
Collides may 
happen at any time

5-33
Pure ALOHA efficiency
Pr[success by given node] = Pr[node transmits] 
.
Pr[no other node transmits in [t
0
-1,t
0
] ]
.
Pr[no other node transmits in [t
0
,t
0
+1]]
= p 
. 
(1-p)
N-1
. 
(1-p)
N-1  
= p 
. 
(1-p)
2(N-1)
Pr[asuccessful slots] = Np(1-p)
2N-1
... choosing optimum p and then letting N 
= 1/(2e) = .18
worsethan the following slotted Aloha!

5-34
Slotted ALOHA
assumptions:

all frames same size

time divided into equal size 
slots(time to transmit 1 
frame)

nodes start to transmit only 
slot 
beginning

nodes are synchronized

if 2 or more nodes transmit 
in slot, all nodes detect 
collision
operation:

when node obtains fresh 
frame, transmits in next slot

if no collision:node can 
send new frame in next 
slot

if collision:node 
retransmits frame in each 
subsequent slot with 
prob. puntil success

5-35
Pros:

single active node can 
continuously transmit at 
full rate of channel

highly decentralized: only 
slots in nodes need to be 
in sync

simple
Cons:

collisions, wasting slots

idle slots

nodes may be able to detect 
collision in less than time to 
transmit packet

clock synchronization
Slotted ALOHA
1
1
1
1
2
3
22
33
node 1
node 2
node 3
C
CCSS
SE
EE
Collides only 
happen at the 
beginning

5-36

suppose:Nnodes with many 
frames to send, each transmits 
in slot with probability p

probthat given node has 
success in a slot  = p(1-p)
N-1

probof a successful slots = 
Np(1-p)
N-1

max efficiency: find p* 
that maximizes 
Np(1-p)
N-1

for many nodes, take limit 
of Np*(1-p*)
N-1 
as N goes 
to infinity, gives:
max efficiency = 1/e = .37
efficiency
: long-run 
fraction of successful slots 
(many nodes, all with many 
frames to send)
at best:
channel
used for useful 
transmissions 37%
of time!
!
Slotted ALOHA: efficiency

5-37
CSMA (carrier sense multiple access)
CSMA:
listen before transmit:
if channel sensed idle:transmit entire frame

if channel sensed busy, defer transmission 

human analogy: don’t interrupt others!

5-38
CSMA collisions

collisions canstill occur: 
propagation delay means  two 
nodes may not hear each other’s 
transmission

Nodes don’t detect collision 
during transmission

collision: entire packet 
transmission time wasted

distance & propagation delay play 
role in in determining collision 
probability
spatial layout of nodes 

5-39
CSMA/CD (collision detection)
CSMA/CD:
carrier sensing, deferral as in CSMA

collisions detectedwithin short time

colliding transmissions aborted, reducing channel wastage 

collision detection:

easy in wired LANs: measure signal strengths, compare 
transmitted, received signals

difficult in wireless LANs: received signal strength overwhelmed 
by local transmission strength 

human analogy: the polite conversationalist 

5-40
CSMA/CD (collision detection)
spatial layout of nodes 

5-41
Ethernet CSMA/CD algorithm
1. NIC receives datagram from 
network layer, creates frame
2. If NIC senses channel idle, starts 
frame transmission. If NIC senses 
channel busy, waits until channel 
idle, then transmits.
3. If NIC transmits entire frame 
without detecting another 
transmission, NIC is done with 
frame !
4. If NIC detects another 
transmission while transmitting,  
aborts and sends jam signal
5. After aborting, NIC enters binary 
(exponential) backoff: 

after m
th
collision, NIC chooses 
Kat random from {0,1,2, ..., 2
m
-
1}. NIC waits K·512 bit times, 
returns to Step 2

longer backoffinterval with more 
collisions

42
根据标准，10Mbps 以太网采用中继器时，连接最大长度为2500米，
最多通过4个中继器，因此10Mbps 以太网帧的最小传输时间为51.2
微秒。这段时间可以传输的数据是512位，所以也叫512位时间。这个
时间被定义为以太网slot，即碰撞槽。512位= 64字节，这也是以太
网frame最少64字节的原因。
无论在Ethernet II还是IEEE802.3标准中，从preamble到FCS字段的
最小帧长必须是64字节。最小帧长度可确保以太网网络接口卡有足够
的传输时间来准确检测冲突。这个最短时间是根据网络的最大电缆长
度和帧沿电缆长度传播所需的时间确定的。基于最小帧长度为64字节
并且使用6字节地址字段的要求。这意味着每个数据字段的最小长度
是46个字节。

5-43
CSMA/CD efficiency

t
prop
= max prop delay between 2 nodes in LAN

t
trans
= time to transmit max-size frame

efficiency goes to 1 (i.e., NO collision)

as t
prop
goes to 0

as t
trans
goes to infinity

better performance than ALOHA: and simple, cheap, decentralized
!
transprop
/tt
efficiency
51
1


No time difference
Always occupying the channel

5-44
“Taking turns”MAC protocols
channel partitioning MAC protocols:

share channel efficientlyand fairlyat high load

inefficient at low load: delay in channel access, 1/N 
bandwidth allocated even if only 1 active node! 
random access MAC protocols

efficient at low load: single node can fully utilize 
channel

high load: collision overhead
“taking turns”protocols
look for best of both worlds!

5-45
polling:

master node “invites”slave nodes to transmit in turn

typically used with “dumb”slave devices

concerns:

polling overhead 

latency

single point of failure (master)
master
slaves
poll
data
data
“Taking turns”MAC protocols

5-46
token passing:

control 
token
passed from one node to next sequentially.

token message

concerns:
token overhead 
latency
single point of failure (token)
T
data
(nothing
to send)
T
“Taking turns”MAC protocols

cable headend
CMTS
ISP
cable modem
termination system

multiple 40Mbps downstream (broadcast) channels
single CMTS transmits into channels

multiple30 Mbps upstream channels
multiple access: all users contend for certain upstream channel 
time slots (others assigned)
A Case: Cable access network
cable
modem
splitter
...
...
Internet frames,TVchannels, control  transmitted 
downstream at different frequencies
upstream Internet frames, TV control,  transmitted 
upstream at different frequencies in time slots
47

5-48
DOCSIS: data over cable service interface spec 

FDMover upstream, downstream frequency channels

TDMupstream: some slots assigned, some have contention
downstream MAP frame: assigns upstream slots
request for upstream slots(and data) transmitted random 
access (binary backoff) in selected slots
MAP frame for
Interval [t1, t2]
Residences with cable modems
Downstream channel i
Upstream channel j
t
1
t
2
Assigned minislots containing cable modem
upstream data frames
Minislots containing 
minislots request frames
cable headend
CMTS
Cable access network

TDM+FDM+CDM
49

5-50
Summary of MAC protocols

channel partitioning,by time, frequency or code

Time Division, Frequency Division

random access(dynamic), 

ALOHA, S-ALOHA, CSMA, CSMA/CD

carrier sensing: easy in some technologies (wire), hard in others 
(wireless)

CSMA/CD used in Ethernet

CSMA/CA used in 802.11

taking turns

polling from central site, token passing

bluetooth, FDDI,  token ring 
A question: why do we use CSMA/CD rather than TDMA/FDMA？

5-51
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

5-52
MAC addresses and ARP

32-bit IP address: 

network-layeraddress for interface

used for layer 3 (network layer) forwarding

MAC (or LAN or physical or Ethernet) address:

function:used ‘locally”to get frame from one 
interface to another physically-connected interface 
(same network, in IP-addressing sense)

48 bit MAC address (for most LANs) burned in NIC 
ROM, also sometimes software settable

e.g.: 1A-2F-BB-76-09-AD
hexadecimal (base 16) notation
(each “number”represents 4 bits)
After the packet enters the local 
network, it depends on its MAC 
address for addressing
Switch
NOT router

5-53
LAN addresses and ARP
each adapter on LAN has unique 
LANaddress 
NIC decides the frame is received 
or not by its destination MAC 
address.
adapter
1A-2F-BB-76-09-AD
58-23-D7-FA-20-B0
0C-C4-11-6F-E3-98
71-65-F7-2B-08-53
LAN
(wired or
wireless)

5-54
LAN addresses (more)

MAC address allocation administered by IEEE

manufacturer buys portion of MAC address space (to assure 
uniqueness)

analogy:

MAC address: like identity card number (fixed)

IP address: like postal address (changeable)

MAC flataddress  ➜portability 

can move LAN card from one LAN to another

IP hierarchical address notportable

address depends on IP subnet to which node is attached
IPMAC
逻辑地址物理地址
全局有效局部有效
层次化平面型
网间寻址网内寻址

5-55
ARP: address resolution protocol
ARP table: each IP node (host, router) on 
LAN has table

IP/MAC address mappings for 
some LAN nodes:
< IP address; MAC address; TTL>

TTL (Time To Live): time after 
which address mapping will be 
forgotten (typically 20 min)
Question:how to determine
interface’s MAC address, 
knowing its IP address?
1A-2F-BB-76-09-AD
58-23-D7-FA-20-B0
0C-C4-11-6F-E3-98
71-65-F7-2B-08-53
LAN
137.196.7.23
137.196.7.78
137.196.7.14
137.196.7.88

5-56
ARP protocol: same LAN

A wants to send datagram to B

B’s MAC address NOTin A’s ARP table.

A broadcastsARP query packet, 
containing B's IP address 

destMAC address = FF-FF-FF-FF-FF-FF

all nodes on LAN receive ARP query 

B receives ARP packet, replies to A 
with its (B's) MAC address

frame sent to A’s MAC address (unicast)

A caches (saves) IP-to-MAC address 
pair in its ARP table until information 
becomes old (times out)

soft state: information that times out 
(goes away) unless refreshed

ARP is “plug-and-play”:

nodes create their ARP tables without 
intervention from net administrator

ARP protocol in action
Link Layer: 6-57
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
A
broadcasts ARP query, containing B's IP addr
•destination MAC address = FF-FF-FF-FF-
FF-FF
•all nodes on LAN receive ARP query 
1
Source MAC:  71-65-F7-2B-
08-53
Source IP: 137.196.7.23
Target IP address: 
137.196.7.14
...
1
Ethernet frame (sent to FF-FF-FF-FF-FF-
FF)

ARP protocol in action
Link Layer: 6-58
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC 
addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
B
replies to A with ARP 
response, giving its MAC 
address
2
Target IP address: 
137.196.7.14
Target MAC address: 
58-23-D7-FA-20-B0
...
2
ARP message into Ethernet frame 
(sent to 71-65-F7-2B-08-53)

ARP protocol in action
Link Layer: 6-59
58-23-D7-FA-20-B0
137.196.7.14
B
C
D
TTL
71-65-F7-2B-08-53
137.196.7.23
A
ARP table in A
IP addr
MAC addr
TTL
example: A wants to send datagram to B
•B’s MAC address not in A’s ARP table, so A uses ARP to find B’s MAC address
A
receives B’s reply, adds B 
entry into its local ARP table
3
137.196.
7.14
58-23-D7-FA-20-B0
500

5-60
walkthrough: send datagram from A to B via R

focus on addressing –at IP (datagram) and MAC layer (frame)

assume A knows B’s IP address

assume A knows IP address of first hop router, R (how?)

assume A knows R’s MAC address (how?)
Addressing: routing to another LAN
R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-61
Addressing: routing to another LAN
IP
Eth
Phy
IP src: 111.111.111.111
IP dest: 222.222.222.222

A creates IP datagram with IP source A, destination B 

A creates link-layer frame with R's MAC address as dest, frame 
contains A-to-B IP datagram
MAC src: 74-29-9C-E8-FF-55
MAC dest: E6-E9-00-17-BB-4B

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-62
Addressing: routing to another LAN
IP
Eth
Phy

frame sent from A to R
IP
Eth
Phy

frame received at R, datagram removed, passed up to IP
MAC src: 74-29-9C-E8-FF-55
MAC dest: E6-E9-00-17-BB-4B
IP src: 111.111.111.111
IP dest: 222.222.222.222
IP src: 111.111.111.111
IP dest: 222.222.222.222

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-63
Addressing: routing to another LAN
IP src: 111.111.111.111
IP dest: 222.222.222.222

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy
IP
Eth
Phy

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-64
Addressing: routing to another LAN

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
IP src: 111.111.111.111
IP dest: 222.222.222.222
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy
IP
Eth
Phy

R
1A-23-F9-CD-06-9B
222.222.222.220
111.111.111.110
E6-E9-00-17-BB-4B
CC-49-DE-D0-AB-7D
111.111.111.112
111.111.111.111
74-29-9C-E8-FF-55
A
222.222.222.222
49-BD-D2-C7-56-2A
222.222.222.221
88-B2-2F-54-1A-0F
B
5-65
Addressing: routing to another LAN

R forwards datagram with IP source A, destination B 

R creates link-layer frame with B's MAC address as dest, frame 
contains A-to-B IP datagram
IP src: 111.111.111.111
IP dest: 222.222.222.222
MAC src: 1A-23-F9-CD-06-9B
MAC dest: 49-BD-D2-C7-56-2A
IP
Eth
Phy

66
A  sends an IP to B, please describe the entire 
addressing process, including the IP and MAC. 
A
S1R1
S2
R3
S3
R5
S4
R6
B
SR
: switch: router
R2
R4
R7
R
R

5-67
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

5-68
Ethernet
“dominant”wired LAN technology: 

cheap $20 for NIC

first widely used LAN technology

simpler, cheaper than token LANs and ATM

kept up with speed race: 10 Mbps –10 Gbps
Metcalfe’s Ethernet sketch

5-69
Ethernet: physical topology

bus: popular through mid 90s

all nodes in same collisiondomain (can collide with 
each other)

star: prevails today

active switchin center

each “spoke”runs a (separate) Ethernet protocol 
(nodes do NOTcollide with each other)
switch
bus: 
coaxial cable
star

5-70
Ethernet frame structure
sending adapter encapsulates IP datagram (or 
other network layer protocol packet) in 
Ethernet frame
preamble: 

7 bytes with pattern 10101010followed by one 
byte with pattern 10101011

used to synchronize receiver, sender clock 
rates
dest.
address
source
address
data 
(payload)
CRC
preamble
type

5-71
Ethernet frame structure (more)

addresses: 6 byte source, destination MAC 
addresses

if adapter receives frame with matching destination 
address, or with broadcast address (e.g. ARP 
packet), it passes data in frame to network layer 
protocol

otherwise, adapter discards frame

type: indicates higher layer protocol (mostly IP but 
others possible, e.g., Novell IPX, AppleTalk)

CRC: cyclic redundancy check at receiver

error detected: frame is dropped
dest.
address
source
address
data 
(payload)
CRC
preamble
type

5-72
Ethernet: unreliable, connectionless

connectionless: no handshaking between 
sending and receiving NICs 

unreliable: receiving NIC doesn’t send Acks
or Nacksto sending NIC

data in dropped frames recovered only if initial 
sender uses higher layer rdt(e.g., TCP), 
otherwise dropped data lost

Ethernet’s MAC protocol: unslottedCSMA/CD 
wthbinary backoff

5-73
802.3 Ethernet standards: link & physical layers

manydifferent Ethernet standards

common MAC protocol and frame format

different speeds: 2 Mbps, 10 Mbps, 100 Mbps, 
1Gbps, 10G bps

different physical layer media: fiber, cable
application
transport
network
link
physical
MAC protocol
and frame format
100BASE-TX
100BASE-T4
100BASE-FX
100BASE-T2
100BASE-SX
100BASE-BX
fiber physical layer
copper (twister
pair) physical layer

5-75
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request
2023.11.26

5-76
Ethernet switch

link-layer device: takes an activerole

store, forward Ethernet frames

examine incoming frame’s MAC address, selectivelyforward  
frame to one-or-more outgoing links when frame is to be 
forwarded on segment, uses CSMA/CD to access segment

Transparent (No IP and MAC address)

hosts are unaware of presence of switches

plug-and-play, self-learning

switches do not need to be configured

5-77
Switch: multiplesimultaneous transmissions

hosts have dedicated, direct connection to 
switch

switches buffer packets

Ethernet protocol used on eachincoming link, 
but no collisions; full duplex

each link is its own collision domain

switching:A-to-A’and B-to-B’can transmit 
simultaneously, without collisions 
switch with six interfaces
(1,2,3,4,5,6)
A
A’
B
B’
C
C’
1
2
3
4
5
6

5-78
Switch forwarding table
Q:how does switch know A’
reachable via interface 4, B’
reachable via interface 5?
switch with six interfaces
(1,2,3,4,5,6)
A
A’
B
B’
C
C’
1
2
3
4
5
6

A:each switch has a switch table,
each entry:
(MAC address of host, interface to 
reach host, time stamp)
looks like a routing table!
Q:how are entries created, 
maintained in switch table? 
something like a routing protocol?

A
A’
B
B’
C
C’
1
2
3
4
5
6
5-79
Switch: self-learning

switchlearnswhich hosts can be 
reached through which interfaces

when frame received, switch 
“learns”location of sender: 
incoming LAN segment

records sender/location pair in 
switch table
A A’
Source: A
Dest: A’
MAC addr   interface    TTL
Switch table 
(initially empty)
A
1
60

A
A’
B
B’
C
C’
1
2
3
4
5
6
5-81
Self-learning, forwarding: example
A A’
Source: A
Dest: A’
MAC addr   interface    TTL
switch table 
(initially empty)
A1
60
A A’
A A’
A A’A A’
A A’

frame destination, A’, 
location unknown:
flood
A’A

destination A location 
known:
A’
4
60
selectively send 
on just one link

5-82
Interconnecting switches

switches can be connected together
Q:sending from A to G-how does S
1
know to 
forward frame destined to F via S
4
and S
3
?

A:self learning! (works exactly the same as in 
single-switch case!)
A
B
S
1
C
D
E
F
S
2
S
4
S
3
H
I
G

5-83
Self-learning multi-switch example
Suppose C sends frame to I, I responds to C

Q:show switch tables and packet forwarding in S
1
, S
2
, S
3
, S
4
A
B
S
1
C
D
E
F
S
2
S
4
S
3
H
I
G

5-84
Institutional network
to external
network
router
IP subnet
mail server
web server

5-85
Switches vs. routers
both are store-and-forward: 
routers: network-layer devices 
(examine network-layer headers)
switches: link-layer devices (examine 
link-layer headers)
both have forwarding tables:
routers: compute tables using routing 
algorithms, IP addresses
switches: learn forwarding table using 
flooding, learning, MAC addresses 
application
transport
network
link
physical
network
link
physical
link
physical
switch
datagram
application
transport
network
link
physical
frame
frame
frame
datagram

5-86
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

5-87
VLANs: motivation
consider:

CS user moves office to EE, 
but wants connect to CS 
switch?

single 
broadcast domain
:
all layer-2 broadcast 
traffic(ARP, DHCP, 
unknown location of 
destination MAC 
address) must cross 
entire LAN

security/privacy, 
efficiency issues
Computer 
Science
Electrical
Engineering
Computer
Engineering
What is the difference between 
VLAN and the general LAN 
formed by router?

Link Layer
5-88
VLANs
port-based VLAN: switch ports grouped (by 
switch management software) so that 
singlephysical switch ......
switch(es) supporting 
VLAN capabilities can 
be configured to 
define multiple virtual
LANS over single 
physical LAN 
infrastructure.
Virtual Local 
Area Network
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
Electrical Engineering
(VLAN ports 1-8)
...
1
8
2
7
9
1610
15
...
Computer Science
(VLAN ports 9-16)
... operates as multiple virtual switches
VLAN1:Port1~8
VLAN2:Port9~16

5-89
Port-based VLAN

traffic isolation:frames to/from 
ports 1-8 can onlyreach ports 1-8

can also define VLAN based on MAC 
addresses of endpoints, rather than switch 
port

dynamic membership:ports can be 
dynamically assigned among VLANs

forwarding between VLANS: done via routing
(just as with separate switches)
in practice vendors sell combined switches plus 
routers
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
router

5-90
VLANS spanning multiple switches

trunk port:carries frames between VLANS defined over multiple 
physical switches

frames forwarded within VLAN between switches can’t be vanilla 802.1 frames 
(must carry VLAN ID info)

802.1q protocol adds/removed additional header fields for frames forwarded 
between trunk ports
1
8
9
10
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
2
7
3
Ports 2,3,5 belong to EE VLAN
Ports 4,6,7,8 belong to CS VLAN
5
46
8
16
1

5-91
type
2-byte Tag Protocol Identifier
(value: 81-00) 
Tag Control Information (12 bit VLAN ID field, 
3 bit priority field like IP TOS)
Recomputed 
CRC
802.1Q VLAN frame format
802.1 frame
802.1Q frame
dest.
address
source
address
data (payload)
CRC
preamble
dest.
address
source
address
preamble
data (payload)
CRC
type
1
8
9
1610
2
7
...
Electrical Engineering
(VLAN ports 1-8)
Computer Science
(VLAN ports 9-15)
15
...
router

92
1).1主机发送普通的数据帧;
2).switch1收到此帧首先需要对其解封装，查看二层帧头部帧目的MAC地址;
3).从表中查找其目的MAC地址对应的VLANID与接收该帧的接口对应的VLANID是否相同，如果相同则找到对应的出接
口，如果不同则丢弃该帧;
4).找到出接口后，打上对应的VLAN 标签，封装成802.1Q的帧，从Trunk接口发送出去;
5).到达switch2后，解封装查看帧头部的目的MAC地址;
6).从表中查找其目的MAC地址对应的VLAN ID与接收该帧头部的VLAN ID是否匹配，如果匹配，则查找对应的出接口，如
果不同则丢弃该帧;
7).找到出接口后，封装成原始的帧，从相应端口转发出去。
同一个交换机中，端口/MAC与
VLAN是捆绑关系，因此，主机发
送帧不需要带上VLAN ID。
在跨越不同交换机时，下一个交换
机无法通过普通帧判断它所属的
VLAN ID，所以，从Trunk口发出
去前，必须插入VLAN ID。

93
HUB、2-layer switch、3-layer switch

94
HUB、2-layer switch、3-layer switch

95
HUB、2-layer switch、3-layer switch


三层交换技术：二层交换技术+三层转发技术。解决局域网中
网段划分之后，网段中子网必须依赖路由器进行管理的局面，
解决了传统路由器低速、复杂所造成的网络瓶颈问题。
96
HUB、2-layer switch、3-layer switch
第三层交换、第四层交
换、多层交换、多层数
据包分类和路由交换机

97
HUB、2-layer switch、3-layer switch

98
HUB、2-layer switch、3-layer switch

99
HUB、2-layer switch、3-layer switch

假设两个使用IP协议的站点A、B通过三层交换机进行通信

发送站点A在开始发送时，把自己的IP地址与B站的IP地址比较
，判断B站是否与自己在同一子网内。

若目的站B与发送站A在同一子网内，则进行二层的转发。

若两个站点不在同一子网内，发送站A要向“缺省网关”发出
ARP(地址解析)封包，而“缺省网关”的IP地址其实是三层交换
机的三层交换模块。

A把数据发给三层交换机的“缺省网关”；
101
HUB、2-layer switch、3-layer switch
AB
三层
交换


三层交换的“缺省网关”收到报文后，按照常规路由，寻找下一
跳出口的MAC及端口。并建立“目的IP~出口MAC~下一跳入口
MAC~端口号”信息。

交换机直接把三层的目的IP映射到二层下一跳的MAC所对应的端
口

从这以后，当A向B发送的数据包经过三层交换时，全部交给二
层交换处理，信息得以高速交换。由于仅仅在路由过程中才需要
三层处理，绝大部分数据都通过二层交换转发，因此三层交换机
的速度很快，接近二层交换机的速度。（不需要查路由表）
102
HUB、2-layer switch、3-layer switch
目的IP出口MAC下一跳入口MAC端口号

5-103
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4 LANs

addressing, ARP

Ethernet

switches

VLANS
6.5 link virtualization: MPLS
6.6data center networking
6.7a day in the life of a web 
request

DTE1
(3,10)-----(2,62)
(3,62)-----(1,22)
DTE3
DTE3
10
62
62
22
Step1:
Step2:
Step3:
DTE1
DTE1
DTE3
DTE3
DTE1
13
2
3
10
10
(3,10)-----(2,62)
(3,62)-----(1,22)
13
2
3
(Interface #, VC #)
104
The VC mode
实质：电路交换与分
组交换的结合体！
电路交换：一次寻路
，预留资源。
缺点：利用率低
分组交换：每次寻路
，利用率高。
缺点：速度低
2023.12.4

5-105
Multiprotocol label switching (MPLS)

initial goal: high-speed IP forwarding using fixed length 
label (instead of IP address) 

fast lookup using fixed length identifier (rather than shortest prefix 
matching)

borrowing ideas from Virtual Circuit (VC) approach

but IP datagram still keeps IP address!
PPP or Ethernet 
header
IP header
remainder of link-layer frame
MPLS header
label
Exp
S
TTL
20
3
1
5
Exp―优先级。
S ―栈底。标签栈中最后进入的标签
位置，s值为0。S值为1表明此为最底
层标签。正因为这个字段表明了
MPLS的标签理论上可以无限嵌套，从
而提供无限的业务支持能力。

5-106
MPLS capable routers

a.k.a. label-switched router

forward packets to outgoing interface based only on label 
value (don’t inspect IP address)

MPLS forwarding table distinct from IP forwarding tables

flexibility:  MPLS forwarding decisions can differfrom those 
of IP

use destination andsource addresses to route flows to same 
destination differently (traffic engineering)

re-route flows quickly if link fails: pre-computed backup paths 
(useful for VoIP)

5-107
R2
D
R3
R5
A
R6
MPLS versus IP paths
IP router

IP routing: path to destination 
determined by destination address 
alone
R4

5-108
R2
D
R3
R4
R5
A
R6
MPLS versus IP paths
IP-only
router

IP routing: path to destination determined by 
destination address alone
MPLS and 
IP router

MPLS routing: path to destination can be based on 
source and dest. address
fast reroute: precompute backup routes in case of link 
failure
entry router (R4)  can use differentMPLS 
routes to A based, e.g., on source address
(R5, A)->Label 1;   (R6, A)-> Label 2

5-109
MPLS signaling

modify OSPF link-state flooding protocols to carry info 
used by MPLS routing, 

e.g., link bandwidth, amount of “reserved”link bandwidth
D
R4
R5
A
R6

entry MPLS router uses RSVP-TE signaling protocol to set up MPLS 
forwarding at downstream routers
modified 
link state 
flooding
RSVP-TE

5-110
R1
R2
D
R3
R4
R5
0
1
0
0
A
R6
in         out                 out
label     label   dest    interface
6        -A       0
in         out                 out
label     label   dest    interface
10      6      A       1
12      9      D       0
in         out                 out
label     labeldestinterface
10      A       0
12      D       0
1
in         out                 out
label     label   dest    interface
8        6      A       0
0
8      A       1
MPLS forwarding tables
入口路由器负责加首个标签
出口路由器负责清理标签

111
IP网络的逐跳式分组转发
匹配查找匹配查找匹配查找匹配查找匹配查找
IP分组
路由器路由器路由器路由器路由器路由器
IP分组
IP分组IP分组IP分组IP分组IP分组
匹配查找
IP分组IP分组
路由器
下一跳中继
匹配查找

112
LER0LSR0LSR1
LSR2
LER1
REQ（65.8)
LABEL(5)
下一跳
入端口
出端口
65.8
LSR0
0
0
101
2
0
1
0
1
0
1
2
目的地址前缀
65.8
65.8
65.8
LSR2
LER1
----
0
2
01
0
----
1
路由表
LER0
LSR0
LSR2
LER1
下一跳
入端口出端口
65.8
LSR0
0
目的地址前缀
65.8
65.8
65.8
LSR2
LER1
----
0
2
01
0
----
1
LER0
LSR0
LSR2
LER1
入标记出标记
---
5
5
8
82
2
---
标记信息表
Step1: routing for the path;
Step2: labeling each link;
Step3: forwarding based on label
Replace the global routing table with the 
locallabel switching table
路由协议
路由表
MPLS路由控制
转发信息表
IPLabel
数据

113
MPLS网络结构
LER
LER
LER
LER
LER
LER
LER
LSRLSR
LSR
LSR
标记交换路径
LSP
IP
IPLABEL
IPLABEL
IPLABEL
IP
标记分布协议
LDP
标记分布协议
LDP
标记分布协议
LDP
路由协议
路由表
MPLS路由控制
转发信息表

5-114
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6 data center networking
6.7a day in the life of a web 
request

5-115
Data center networks 

10’s to 100’s of thousands of hosts, often closely coupled, 
in close proximity:

e-business(e.g. Amazon)

content-servers(e.g., YouTube, Akamai, Apple, Microsoft)

search engines, data mining (e.g., Google)

challenges:
multiple applications, each serving 
massive numbers of clients 
managing/balancing load, avoiding 
processing, networking, data bottlenecks  
Inside a 40-ft Microsoft container, 
Chicago data center

Datacenter networks: network elements
Link Layer: 6-116
Server racks
20-40 server blades: hosts 
Top of Rack (TOR) switch
one per rack
40-100Gbps Ethernet to blades      
Tier-2 switches
connecting to  ~16 TORs below
Tier-1 switches
connecting to  ~16 T-2s below
Border routers
connections outside datacenter
...
...
...
...
...
...
...
...

Datacenter networks: network elements
Link Layer: 6-117
Facebook F16 data center network topology:
https://engineering.fb.com/data-center-engineering/f16-minipack/    (posted 3/2019)

Datacenter networks: multipath
Link Layer: 6-118
910
11
12
13
14
15
16
two disjointpaths highlighted between racks 1 and 11
rich interconnection among switches, racks:
•increased throughput between racks (multiple routing paths possible)
•increased reliability via redundancy

...
...
...
...
...
...
...
...
Datacenter networks: application-layer routing
Link Layer: 6-119
Load 
balancer
Internet
load balancer: 
application-layer 
routing
receives external 
client requests
directs workload 
within data center
returns results to 
external client 
(hiding data center 
internals from client)


link layer: 

RoCE: remote DMA (RDMA) over Converged Ethernet

transport layer: 

ECN (explicit congestion notification) used in transport-layer 
congestion control (DCTCP, DCQCN)

experimentation with hop-by-hop (backpressure) congestion 
control

routing, management:

SDN widely used within/among organizations’ datacenters

place related services, data as close as possible (e.g., in same 
rack or nearby rack) to minimize tier-2, tier-1 communication
Datacenter networks: protocol innovations
Link Layer: 6-120

5-121
Link layer, LANs: outline
6.1introduction, services
6.2error detection, correction 
6.3multiple access protocols
6.4LANs

addressing, ARP

Ethernet

switches

VLANS
6.5link virtualization: MPLS
6.6data center networking
6.7 a day in the life of a web 
request（DIY）

5-122
Synthesis: a day in the life of a web request

journey down protocol stack complete!

application, transport, network, link

putting-it-all-together: synthesis!

goal:identify, review, understand protocols 
(at all layers) involved in seemingly simple 
scenario: requesting www page

scenario:student attaches laptop to 
campus network, requests/receives 
www.google.com

5-123
A day in the life: scenario
Comcast network 
68.80.0.0/13
Google’s network 
64.233.160.0/19 
64.233.169.105
web server
DNS server
school network 
68.80.2.0/24
web page
browser

router
(runs DHCP)
5-124
A day in the life... connecting to the Internet

connecting laptop needs to 
get its own IP address, addr
of first-hop router, addrof 
DNS server: use DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP

DHCP request encapsulated in UDP, 
encapsulated in IP, encapsulated in 
802.3 Ethernet

Ethernet frame broadcast(dest: 
FFFFFFFFFFFF) on LAN, received at 
router running DHCP server

Ethernet demuxedto IP demuxed, 
UDP demuxedto DHCP 

router
(runs DHCP)
5-125

DHCP server formulates 
DHCP ACKcontaining 
client’s IP address, IP 
address of first-hop router 
for client, name & IP address 
of DNS server
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCP
DHCP

encapsulation at DHCP server, 
frame forwarded (switch learning) 
through LAN, demultiplexingat 
client
Client now has IP address, knows name & addrof DNS 
server, IP address of its first-hop router

DHCP client receives DHCP ACK 
reply
A day in the life... connecting to the Internet

router
(runs DHCP)
5-126
A day in the life... ARP
(before DNS, before HTTP)

before sending HTTP request, 
need IP address of 
www.google.com:  DNS
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS

DNS query created, encapsulated in 
UDP, encapsulated in IP, encapsulated 
in Eth.  To send frame to router, need 
MAC address of router interface: 
ARP

ARP query broadcast, received by 
router, which replies with ARP reply 
giving MAC address of router 
interface

client now knows MAC address of 
first hop router, so can now send 
frame containing DNS query 
ARP query
Eth
Phy
ARP
ARP
ARP reply

router
(runs DHCP)
5-127
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS
DNS
DNS

IP datagram containing DNS 
query forwarded via LAN 
switch from client to 1
st
hop 
router

IP datagram forwarded from 
campus network into comcast
network, routed (tables created by 
RIP, OSPF, IS-IS and/or BGP
routing protocols) to DNS server

Demux’edto DNS server

DNS server replies to client with IP 
address of www.google.com
Comcast network 
68.80.0.0/13
DNS server
DNS
UDP
IP
Eth
Phy
DNS
DNS
DNS
DNS
A day in the life... using DNS

router
(runs DHCP)
5-128
A day in the life...TCP connection carrying HTTP
HTTP
TCP
IP
Eth
Phy
HTTP

to send HTTP request, 
client first opens TCP 
socketto web server

TCP SYN segment (step 1 in 3-
way handshake) inter-domain 
routed to web server

TCP connection established!
64.233.169.105
web server
SYN
SYN
SYN
SYN
TCP
IP
Eth
Phy
SYN
SYN
SYN
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK
SYNACK

web server responds with TCP 
SYNACK (step 2 in 3-way 
handshake)

router
(runs DHCP)
5-129
A day in the life... HTTP request/reply
HTTP
TCP
IP
Eth
Phy
HTTP

HTTP request sent into 
TCP socket

IP datagram containing HTTP 
request routed to 
www.google.com

IP datagram containing HTTP 
reply routed back to client
64.233.169.105
web server
HTTP
TCP
IP
Eth
Phy

web server responds with 
HTTP reply (containing web 
page)
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP
HTTP

web page finally (!!!) displayed

5-130
Chapter 6: Summary

principles behind data link layer services:

error detection, correction

sharing a broadcast channel: multiple access

link layer addressing

instantiation and implementation of various 
link layer technologies

Ethernet

switched LANS, VLANs

virtualized networks as a link layer: MPLS

synthesis: a day in the life of a web request

5-131
Chapter 6: let’s take a breath

journey down protocol stack complete(except PHY)

solid understanding of networking principles, practice

..... could stop here .... but lotsof interesting topics!

wireless

multimedia

security 
The End of Chapter 6

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
