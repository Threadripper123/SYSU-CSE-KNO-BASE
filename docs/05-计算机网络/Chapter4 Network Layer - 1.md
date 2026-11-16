# Chapter4 Network Layer - 1

> 主题库：计算机网络　｜　源文件：Chapter4 Network Layer - 1.pdf　｜　42,559 字符　｜　102 个片段　｜　更新于 2026-11-11


Computer Networking
䉒䙨
ѝኡབྷᆖg䇑㇇ᵪᆖ䲒
2024. Fall

Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020
Chapter 4
Network Layer:
Data Plane
A note on the use of these PowerPoint slides:
We’re making these slides freely available to all (faculty, students, 
readers). They’re in PowerPoint form so you see the animations; 
and can add, modify, and delete slides  (including this one) and 
slide content to suit your needs. They obviously represent a lotof 
work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their 
source (after all, we’d like people to use our book!)
If you post any slides on a www site, that you note that they 
are adapted from (or perhaps identical to) our slides, and note 
our copyright of this material.
For a revision history, see the slide note for this page. 
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2020
J.F Kurose and K.W. Ross, All Rights Reserved

Assignments :
z(CN, ver8)  1ǃ4ǃ6ǃ11ǃ14ǃ15ǃ18ǃ21ǃ23ǃ24
zA team project
zKeywords: Forwarding and Routing, Virtual Circuit and Datagram 
Networks, Switch, IPv4, Addressing, DHCP, NAT, IPv6, SDN, flow 
table, Openflow

4.1 Overview of Network layer
data plane
control plane
4.2 What’s inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-4

Chapter 4: network layer
chapter goals:
understand principles behind network layer services, 
focusing on data plane:
•network layer service models
•forwarding versus routing
•how a router works
•generalized forwarding
instantiation, implementation in the Internet
4-5

4-6
Network layer
ztransport segment from sending to 
receiving host 
zon sending side encapsulates 
segmentsinto datagrams
zon receiving side, delivers segments 
to transport layer
znetwork layer protocols in everyhost, 
router
zrouter examines header fields in all IP 
datagrams passing through it
application
transport
network
data link
physical
application
transport
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
network
data link
physical
IP payload
IPH
TCP payload
TCPH

4-7
Two key network-layer functions
zrouting:determine route 
taken by packets from source 
to dest. 
routing algorithms
zforwarding:move packets 
from router䇻s input to 
appropriate router output
zConnection Setup
Not included in IP
analogy:
routing:process of planning trip 
from source to dest
forwarding:process of getting 
through single interchange

Network layer: data plane, control plane
Data plane
local, per-router function
determines how datagram 
arriving on router input port is 
forwardedto router output port
forwarding function
Control plane
network-wide logic
determines how datagram is routed
among routers along end-
end path from 
source host to destination host
two control-plane approaches:
•traditional routing algorithms: implemented 
in routers
•software-defined networking (SDN): 
implemented in (remote) servers
1
2
3
0111
values in arriving 
packet header
33
1
4-8

Per-router control plane
Routing
Algorithm
Individual routing algorithm components in each and every router interact in the 
control plane
data
plane
control
plane
5-9
1
2
0111
1
values in arriving 
packet header
3

data
plane
control
plane
Logically centralized control planepgyyyyyyyyyyyyyy
A distinct (typically remote) controller interacts with local control agents (CAs)
Remote Controller
CA
CA
CA
CA
CA
5-10
1
2
0111
1
3
values in arriving 
packet header

4-11
Network service model
Q:What service modelfor Āchannelātransporting datagrams from sender 
to receiver?
example services for individual 
datagrams:
guaranteed delivery
guaranteed delivery with less than 
40 msec delay
example services for a flow of 
datagrams:
zin-orderdatagram delivery
zguaranteed minimum bandwidth to 
flow
zrestrictions on changes in inter-
packet spacing

Network-layer service model
Network
Architecture
Internet
ATM
ATM
Internet
Internet
Service
Model
best effort
Constant Bit Rate
Available Bit Rate
Intserv Guaranteed
(RFC 1633)
Diffserv
(RFC 2475) 
Bandwidth
none
Constant rate
Guaranteed min
yes
possible
Loss
no
yes
no
yes
possibly
Order
no
yes
yes
yes
possibly
Timing
no
yes
no
yes
no
EŽŐƵĂƌĂŶƚĞĞƐŽŶ͗
ŝ͘ ƐƵĐĐĞƐƐĨƵůĚĂƚĂŐƌĂŵĚĞůŝǀĞƌLJƚŽĚĞƐƚŝŶĂƚŝŽŶ
ŝŝ͘ ƚŝŵŝŶŐŽƌŽƌĚĞƌŽĨĚĞůŝǀĞƌLJ
ŝŝŝ͘ďĂŶĚǁŝĚƚŚĂǀĂŝůĂďůĞƚŽĞŶĚͲĞŶĚĨůŽǁ
/ŶƚĞƌŶĞƚ͞ďĞƐƚĞĨĨŽƌƚ͟ƐĞƌǀŝĐĞŵŽĚĞů
Quality of Service (QoS) Guarantees ?
Network Layer: 4-12

4-13
Network layer service models:
Network
Architecture
Internet
ATM
ATM
ATM
ATM
Service
Model
best effort
CBR
VBR
ABR
UBR
Bandwidth
none
constant
rate
guaranteed
rate
guaranteed 
minimum
none
Loss
no
yes
yes
no
no
Order
no
yes
yes
yes
yes
Timing
no
yes
yes
no
no
Congestion
feedback
no (inferred
via loss)
no
congestion
no
congestion
yes
no
Guarantees?
UBR˖Unspecified Bit Rate
CBR˖Constant Bit Rate
ABR˖Available Bit Rate
VBR˖Variable Bit Rate

Reflections on best-effort  service:
simplicity of mechanism has allowed Internet to be widely 
deployed adopted
sufficient provisioning of bandwidthallows performance of 
real-time applications (e.g., interactive voice, video) to be 
“good enough” for “most of the time”
replicated, application-layer distributed services 
(datacenters, content distribution networks) connecting 
close to clients’ networks, allow services to be provided 
from multiple locations
congestion control of “elastic” services helps
/ƚ͛ƐŚĂƌĚƚŽĂƌŐƵĞǁŝƚŚƐƵĐĐĞƐƐŽĨďĞƐƚͲĞĨĨŽƌƚƐĞƌǀŝĐĞŵŽĚĞů
Network Layer: 4-14

4.1 Overview of Network layer
data plane
control plane
4.2 What’s inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-15

4-16
Router architecture overview
two key router functions:
run routing algorithms/protocol (RIP, OSPF, BGP)
forwardingdatagrams from incoming to outgoing link
high-seed 
switching
fabric
routing 
processor
router input ports
router output ports
forwarding data 
plane  (hardware)
routing, management
control plane (software)
forwarding tables computed,
pushed to input ports
r1
r2
r4
r3
Which router is the best 
choice for a given dstIP?
r1
Which port goes to 
the given router?

4-17
line
termination
link 
layer 
protocol
(receive)
lookup,
forwarding
queueing
Input port functions
decentralized switching:
zgiven datagram dest., lookup output port using 
forwarding table in input port memory (“
match plus 
action”)
zgoal: complete input port processing at 䇺line 
speed䇻
zqueuing: if datagramsarrive faster than forwarding 
rate into switch fabric
physical layer:
bit-level reception
data link layer:
e.g., Ethernet
see chapter 5
switch
fabric

Destination Address Range
11001000 00010111 00010000 00000000
through
11001000 00010111 00010111 11111111
11001000 00010111 00011000 00000000
through
11001000 00010111 00011000 11111111
11001000 00010111 00011001 00000000
through
11001000 00010111 00011111 11111111
otherwise
Link Interface
0
1
2
3
Q:but what happens if 
ranges don䇻t divide up 
so nicely? 
Destination-based forwarding
forwarding table
4-18
192.168.1.1 ~ 192.168.1.5       r1
192.168.1.6 ~ 192.168.1.10     r2
192.168.1.11 ~ 192.168.1.15    r1
192.168.1.16 ~ 192.168.1.20    r2
...

Longest prefix matching
Destination Address Range
11001000 00010111 00010*** *********
11001000 00010111 00011000 *********
11001000 00010111 00011*** *********
otherwise
DA: 11001000  00010111  00011000  10101010
examples:
DA: 11001000  00010111  00010110  10100001 
which interface?
which interface?
when looking for forwarding table entry for given 
destination address, use longestaddress prefix that 
matches destination address.
longest prefix matching
Link interface
0
1
2
3
4-19

Longest prefix matching
zwe’ll seewhy longest prefix matching is used shortly, when 
we study addressing
zlongest prefix matching: often performed using ternary 
content addressable memories(TCAMs)
content addressable: present address to TCAM: retrieve address in 
one clock cycle, regardless of table size
Cisco Catalyst: can up ~1M routing table entries in TCAM
4-20

21
Longest prefix matchingggp
ternary content addressable 
memories(TCAMs)

4-22
Switching fabrics
transfer packet from input buffer to appropriate output buffer
switching rate: rate at which packets can be transfer from 
inputs to outputs
often measured as multiple of input/output line rate
N inputs: switching rate N times line rate desirable
three types of switching fabrics
memory
memory
bus
crossbar

4-23
Switching via memory
first generation routers:
ztraditional computers with switching under direct control of CPU
zpacket copied to system䇻s memory
zspeed limited by memory bandwidth (2 bus crossings per datagram)
input
port
(e.g.,
Ethernet)
memory
output
port
(e.g.,
Ethernet)
system bus

4-24
Switching via a bus
datagram from input port memory
to output port memory via a shared 
bus
bus contention:switching speed 
limited by bus bandwidth
32 Gbps bus, Cisco 5600: sufficient 
speed for access and enterprise 
routers
bus

4-25
Switching via interconnection network
overcome  bus bandwidth limitations
banyan networks, crossbar, other 
interconnection nets initially 
developed to connect processors in 
multiprocessor
advanced design: fragmenting 
datagram into fixed length cells, 
switch cells through the fabric. 
Cisco 12000: switches 60 Gbps
through the interconnection network
crossbar
ΘdžΘŵƵůƚŝƐƚĂŐĞƐǁŝƚĐŚ
ďƵŝůƚĨƌŽŵƐŵĂůůĞƌͲƐŝnjĞĚƐǁŝƚĐŚĞƐ

26
1
2
3
4
1
2
3
4
123 41234
crossbar
䙊/ޣᯝᔰ
Switching via interconnection network

scaling, using multiple switching “planes” in parallel: 
speedup, scaleup via parallelism
Switching via interconnection network
ĨĂďƌŝĐƉůĂŶĞϬ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϬ
ĨĂďƌŝĐƉůĂŶĞϭ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϭ
ĨĂďƌŝĐƉůĂŶĞϮ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϮ
ĨĂďƌŝĐƉůĂŶĞϯ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞϯ
ĨĂďƌŝĐƉůĂŶĞκ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞκ
ĨĂďƌŝĐƉůĂŶĞρ
͘͘͘
͘͘͘
ĨĂďƌŝĐƉůĂŶĞρ
ĨĂďƌŝĐƉůĂŶĞς
͘͘͘
͘͘͘
͘
͘
͘
ĨĂďƌŝĐƉůĂŶĞς
ĨĂďƌŝĐƉůĂŶĞϳ
͘͘͘
͘͘͘
ŝƐĐŽZ^ƌŽƵƚĞƌ͗
ďĂƐŝĐƵŶŝƚ͗ΘƐǁŝƚĐŚŝŶŐƉůĂŶĞƐ
ĞĂĐŚƉůĂŶĞ͗ϯͲƐƚĂŐĞŝŶƚĞƌĐŽŶŶĞĐƚŝŽŶ
ŶĞƚǁŽƌŬ
ƵƉƚŽϭϬϬ͛ƐdďƉƐƐǁŝƚĐŚŝŶŐĐĂƉĂĐŝƚLJ
Network Layer: 4-27

͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘͘͘
͘
͘
͘
͘͘͘
͘͘͘
4-28
Input port queuing
zfabric slower than input ports combined -> queueing may occur at input 
queues 
queueing delay and loss due to input buffer overflow!
zHead-of-the-Line (HOL) blocking:queued datagram at front of queue 
prevents others in queue from moving forward
output port contention:
only one red datagram can be 
transferred.
lower red packet is blocked
switch
fabric
one packet time later: 
green packet 
experiences HOL 
blocking
switch
fabric

4-29
Output ports
bufferingrequired when datagrams arrive from fabric faster 
than the transmission rate
scheduling disciplinechooses among queued datagrams for 
transmission
line
termination
link 
layer 
protocol
(send)
switch
fabric
datagram
buffer
queueing
This slide in HUGELY important!
Datagram (packets) can be lost 
due to congestion, lack of buffers
Priority scheduling – who gets best 
performance, network neutrality
Why is the lose in output serious than the input?

4-30
Output port queueing
zbuffering when arrival rate via switch exceeds output line 
speed
zqueueing (delay) and loss due to output port buffer overflow!
at t,packets more
from input to output
one packet time later
switch
fabric
switch
fabric

4-31
How much buffering?
zRFC 3439 rule of thumb: average buffering equal to 䇾typical䇿
RTT (say 250 msec) times link capacity C
e.g., C = 10 Gpbs link: 2.5 Gbit buffer
zrecent recommendation: with Nflows, buffering equal to 
RTT  C
.
N

Buffer Management
ďƵĨĨĞƌŵĂŶĂŐĞŵĞŶƚ͗
ĚƌŽƉ͗
ǁŚŝĐŚƉĂĐŬĞƚƚŽĂĚĚ͕ĚƌŽƉǁŚĞŶ
ďƵĨĨĞƌƐĂƌĞĨƵůů
•ƚĂŝůĚƌŽƉ͗ĚƌŽƉĂƌƌŝǀŝŶŐƉĂĐŬĞƚ
•ƉƌŝŽƌŝƚLJ͗ĚƌŽƉͬƌĞŵŽǀĞŽŶƉƌŝŽƌŝƚLJ
ďĂƐŝƐ
line
termination
link 
layer 
protocol
(send)
switch
fabric
datagram
buffer
queueing 
scheduling
ŵĂƌŬŝŶŐ͗ǁŚŝĐŚƉĂĐŬĞƚƐƚŽŵĂƌŬƚŽ
ƐŝŐŶĂůĐŽŶŐĞƐƚŝŽŶ;E͕ZͿ
Z
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
ďƐƚƌĂĐƚŝŽŶ͗ƋƵĞƵĞ
Z
Network Layer: 4-32

Scheduling mechanisms
zscheduling: choose next packet to send on link
zFIFO (first in first out) scheduling: send in order of arrival to 
queue
real-world example?
discard policy: if packet arrives to full queue: who to discard?
tail drop: drop arriving packet
priority: drop/remove on priority basis
random: drop/remove randomly
g
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
4-33Network Layer: Data Plane

packet scheduling: deciding 
which packet to send next on link
first come, first served
priority
round robin
weighted fair queueing
Packet Scheduling: FCFS
&&^͗ƉĂĐŬĞƚƐƚƌĂŶƐŵŝƚƚĞĚŝŶ
ŽƌĚĞƌŽĨĂƌƌŝǀĂůƚŽŽƵƚƉƵƚ
ƉŽƌƚ
ĂůƐŽŬŶŽǁŶĂƐ͗&ŝƌƐƚͲŝŶͲĨŝƌƐƚͲ
ŽƵƚ;&/&KͿ
ƌĞĂůǁŽƌůĚĞdžĂŵƉůĞƐ͍
queue
(waiting area)
packet
arrivals
packet
departures
link
(server)
ďƐƚƌĂĐƚŝŽŶ͗ƋƵĞƵĞ
Z
Network Layer: 4-34

Scheduling policies: priority
priority scheduling: send 
highest priority queued 
packet 
zmultiple classes, with 
different priorities
class may depend on marking 
or other header info, e.g. IP 
source/dest, port numbers, etc.
real world example? 
high priority queue
(waiting area)
low priority queue
(waiting area)
arrivals
classify
departures
link
(server)
4-35
1
3
2
4
5
5
5
2
2
1
1
3
3
4
4
arrivals
departures
packet 
in 
service

Scheduling policies: still more
Round Robin (RR) scheduling:
zmultiple classes
zcyclically scan class queues, sending one complete 
packet from each class (if available)
zreal world example?
4-36
arrivals
classify
departures
link
(server)
1
2
3
4
5
5
5
2
3
1
1
3
3
4
4
arrivals
departures
packet 
in 
service

Weighted Fair Queuing (WFQ): 
zgeneralized Round Robin
zeach class gets weighted amount of service in each cycle
zreal-world example?
Scheduling policies: still more
4-37

4.1 Overview of Network layer
data plane
control plane
4.2 Whatÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-41

4-42
The Internet network layer
forwarding
table
host, router network layer functions:
routing protocols
•path selection
•RIP, OSPF, BGP
IP protocol
•addressing conventions
•datagram format
•packet handling conventions
ICMP protocol
•error reporting
•router 
Āsignalingā
transport layer: TCP, UDP
link layer
physical layer
network
layer

4-43
ver
length
32 bits
data 
(variable length,
typically a TCP 
or UDP segment)
16-bit identifier
header
checksum
time to
live
32 bit source IP address
Head
er len
type of
service
flgs
fragment
offset
upper
layer
32 bit destination IP address
options (if any)
IP datagram format
IP protocol version
number
header length
(bytes)
upper layer protocol
to deliver payload to
total datagram
length (bytes)
Ātypeāof data 
for
fragmentation/
reassembly
max number
remaining hops
(decremented at 
each router)
e.g. timestamp,
record route
taken, specify
list of routers 
to visit.
how much overhead?
20 bytes of TCP
20 bytes of IP
= 40 bytes + app 
layer overhead
“16-bit identifier” is NOT a number for reordering
No reordering in Net Layer
TCP=6;
UDP=17

4-44
IP fragmentation, reassembly
znetwork links have MTU (max.transfer
size) - largest possible link-level frame
different link types, different MTUs 
zlarge IP datagram divided 
(
䇾fragmented䇿) within net
one datagram becomes several 
datagrams
䇾reassembled䇿only at final 
destination
IP header bits used to identify, 
order related fragments
g
fragmentation:
in:one large datagram
out:3 smaller datagrams
reassembly
...
..............................

...
..............................
4-45
ID
=x
offset
=0
fragflag
=0
length
=4000
ID
=x
offset
=0
fragflag
=1
length
=1500
ID
=x
offset
=185
fragflag
=1
length
=1500
ID
=x
offset
=370
fragflag
=0
length
=1040
one large datagram becomes
several smaller datagrams
example:
4000 byte datagram
Payload=3980
MTU = 1500 bytes
1480 bytes in 
data field
offset =
1480/8 
IP fragmentation, reassembly
(1500-20)+(1500-20)+(1040-20)=3980

4.1 Overview of Network layer
data plane
control plane
4.2 What
ÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-46

4-47
IP addressing: introduction
zIP address:32-bit identifier for 
host, router interface
zinterface:connection between 
host/router and physical link
Router’s typically have multiple interfaces
host typically has one or two interfaces 
(e.g., wired Ethernet, wireless 802.11)
zIP addresses associated with each 
interface
223.1.1.1
223.1.1.2
223.1.1.3
223.1.1.4
223.1.2.9
223.1.2.2
223.1.2.1
223.1.3.2
223.1.3.1
223.1.3.27
223.1.1.1 = 11011111 00000001 00000001 00000001
223
111
1.2
222222222222222
2232.
223.1

4-49
Subnets
zIP address:
subnet part - high order bits
host part - low order bits 
zwhat䇻s a subnet ?
device interfaces with same 
subnet part of IP address
can physically reach each other 
without intervening router
network consisting of 3 subnets
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
222
subnet
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1

4-50
recipe
to determine the subnets, 
detach each interface from its 
host or router, creating islands 
of isolated networks
each isolated network is called 
a subnet
subnet mask: /24
Subnets
223.1.1.0/24
223.1.2.0/24
223.1.3.0/24
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
s
22
subnet
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1

4-51
how many?
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.2
223.1.2.1
223.1.2.6
223.1.3.2
223.1.3.1
223.1.3.27
223.1.1.2
223.1.7.0
223.1.7.1
223.1.8.0223.1.8.1
223.1.9.1
223.1.9.2
Subnets
11
33

52
যছ
६ӯ
ࡖܗՔלޣ
ࣩযছݤ
ॕ▁ЖՔࡖܗࣩ
যছՙ
ࣩࡖܗդ▁ЖՔޣ
যছՙ
ࠠЖযছЗ
ݤ޲Оלޣ
'

Ȉ
(

Ȉ
)

Ȉ    
IP Address Classes

53
⿱ᴹIPൠ൰ᱟа⇥؍⮉
ⲴIPൠ൰DŽਚᱟ֯⭘൘
ተฏ㖁ѝˈ൘
Internetк
ᱟн֯⭘ⲴDŽ⿱ᴹ
IPൠ
൰Ⲵ㤳തᴹ
:
㊫࡛
IP
㤳ത
IP
㤳ത㖁㔌ᮠ䟿
A10.x.x.x/810.0.0.0~10.255.255.2551
B172.16-32.x.x/16172.16.0.0~172.31.255.255 16
C192.168.x.x/24192.168.0.0~192.168.255.255 255
IP Address Classes
যছՙՙ޲О
֬֩ࡼ
࣐҄
ࣹࣩ֬֩
࣐҄
і਽ࣩڿ㓢
ՔјЈՔ
ޯ֧যছІࣩޯО޲ͣ੎ ৼ
*.)6
Ԣ੠ͤ
NUYZOJՔјЈՔ   ޯ֧যছІࣩߏՕО޲ NUYZOJ
ҵҵЈՔՔј
Ј֭֗ͣ՞ષ࣒ݏٹযছІૡਸ਼ޯ֧Ր
ૃՆͤ
TKZOJҵЈՔՔј   تTKZOJІࣩޥےО޲ૡਸ਼ݏٹ

ஂҵۋҵࣩ
ݤџѽ
ՔјՔј   ࣐и֩ޯ૆ѝࢺ࡚֚ੰ

4-54
IP addressing: CIDR
CIDR:Classless InterDomainRouting
subnet portion of address of arbitrary length
address format: a.b.c.d/x, where x is # bits in 
subnet portion of address
11001000  00010111  00010000  00000000
subnet
part
host
part
IP: 200.23.16.0/23
23bits
11111111  11111111  1111111000000000
᧙⸱

4-56
IP addresses: how to get one?
Q:How does a hostget IP address?
zhard-coded by system admin in a 
file
Windows: control-panel->network->configuration->tcp/ip-
>properties
UNIX: /etc/rc.config
zDHCP: Dynamic Host Configuration Protocol: 
dynamically get address from as server
䇾plug-and-play䇿

4-57
DHCP: Dynamic Host Configuration Protocol
goal:allow host to dynamicallyobtain its IP address from network server when 
it joins network
can renew its lease on address in use
allows reuse of addresses (only hold address while connected/䇾on䇿)
support for mobile users who want to join network (more shortly)
DHCP overview:
host broadcasts 䇾DHCP discover䇿msg [optional]
DHCP server responds with 䇾DHCP offer䇿msg [optional]
host requests IP address: 䇾DHCP request䇿msg
DHCP server sends address: 䇾DHCP ack䇿msg

58
DHCP client-server scenario
223.1.1.0/24
223.1.2.0/24
223.1.3.0/24
223.1.1.1
223.1.1.3
223.1.1.4
223.1.2.9
223.1.3.2
223.1.3.1
2222
223.1.1.2
223.1.3.27
223.1.2.2
223.1.2.1
DHCP
server
arriving DHCP
clientneeds 
address in this
network

4-59
DHCP server: 223.1.2.5
arriving
client
DHCP discover
src : 0.0.0.0, 68     
dest.: 255.255.255.255,67
yiaddr:    0.0.0.0
transaction ID: 654
DHCP offer
src: 223.1.2.5, 67      
dest:  255.255.255.255, 68
yiaddrr: 223.1.2.4
transaction ID: 654
lifetime: 3600 secs
DHCP request
src:  0.0.0.0, 68     
dest::  255.255.255.255, 67
yiaddrr: 223.1.2.4
transaction ID: 655
lifetime: 3600 secs
DHCP ACK
src: 223.1.2.5, 67      
dest:  255.255.255.255, 68
yiaddrr: 223.1.2.4
transaction ID: 655
lifetime: 3600 secs
DHCP client-server scenario
Broadcast: is there a 
DHCP server out there?
Broadcast: I’m a DHCP 
server! Here’s an IP 
address you can use 
Broadcast: OK.  I’ll take 
that IP address!
Broadcast: OK.  You’ve 
got that IP address!
yiaddr: 
your Internet address

4-60
DHCP: more than IP addresses
DHCP can return more than just allocated IP address on 
subnet:
address of first-hop router for client
name and IP address of DNS sever
network mask (indicating network versus host portion of address)

4-61
connecting laptop needs its IP 
address, addr of first-hop router, 
addr of DNS server: use DHCP
router with DHCP 
server built into 
router
DHCP request encapsulated in UDP, 
encapsulated in IP, encapsulated in 802.1 
Ethernet
Ethernet frame broadcast (dest: FFFFFFFFFFFF) 
on LAN, received at router running DHCP 
server
Ethernet demuxed to IP demuxed, UDP 
demuxed to DHCP 
168.1.1.1
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCP
DHCPDHCP
DHCP
DHC
DHC
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCPDHC
DHC
DHC
DHCP
DHCP
DHCP: example

4-62
zDCP server formulates DHCP ACK 
containing client䇻s IP address, IP 
address of first-hop router for client, 
name & IP address of DNS server
encapsulation of DHCP server, frame 
forwarded to client, demuxing
up to DHCP 
at client
DHCP: example
router with DHCP 
server built into 
router
DHCP
DHCP
DHCP
DHC
DHC
DHCP
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
UDP
IP
Eth
Phy
DHCP
DHCP
DHCPDHC
DHC
DHC
DHCP
client now knows its IP address, name and 
IP address of DSN server, IP address of its 
first-hop router

4-63
DHCP: Wireshark output (home LAN)
Message type: Boot Reply (2)
Hardware type: Ethernet
Hardware address length: 6
Hops: 0
Transaction ID: 0x6b3a11b7
Seconds elapsed: 0
Bootp flags: 0x0000 (Unicast)
Client IP address: 192.168.1.101 (192.168.1.101)
Your (client) IP address: 0.0.0.0 (0.0.0.0)
Next server IP address: 192.168.1.1 (192.168.1.1)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Server host name not given
Boot file name not given
Magic cookie: (OK)
Option: (t=53,l=1) DHCP Message Type = DHCP ACK
Option: (t=54,l=4) Server Identifier = 192.168.1.1
Option: (t=1,l=4) Subnet Mask = 255.255.255.0
Option: (t=3,l=4) Router = 192.168.1.1
Option: (6) Domain Name Server
Length: 12; Value: 445747E2445749F244574092; 
IP Address: 68.87.71.226;
IP Address: 68.87.73.242; 
IP Address: 68.87.64.146
Option: (t=15,l=20) Domain Name = "hsd1.ma.comcast.net."
reply
Message type: Boot Request (1)
Hardware type: Ethernet
Hardware address length: 6
Hops: 0
Transaction ID: 0x6b3a11b7
Seconds elapsed: 0
Bootp flags: 0x0000 (Unicast)
Client IP address: 0.0.0.0 (0.0.0.0)
Your (client) IP address: 0.0.0.0 (0.0.0.0)
Next server IP address: 0.0.0.0 (0.0.0.0)
Relay agent IP address: 0.0.0.0 (0.0.0.0)
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Server host name not given
Boot file name not given
Magic cookie: (OK)
Option: (t=53,l=1) DHCP Message Type = DHCP Request
Option: (61) Client identifier
Length: 7; Value: 010016D323688A; 
Hardware type: Ethernet
Client MAC address: Wistron_23:68:8a (00:16:d3:23:68:8a)
Option: (t=50,l=4) Requested IP Address = 192.168.1.101
Option: (t=12,l=5) Host Name = "nomad"
Option: (55) Parameter Request List
Length: 11; Value: 010F03062C2E2F1F21F92B
1 = Subnet Mask; 15 = Domain Name
3 = Router; 6 = Domain Name Server
44 = NetBIOS over TCP/IP Name Server
......
request

4-64
IP addresses: how to get one?
Q:how does networkget subnet part of IP addr?
A:gets allocated portion of its provider ISP䇻s address space
ISP's block          11001000  00010111  00010000  00000000    200.23.16.0/20
Organization 0    11001000  00010111  00010000  00000000    200.23.16.0/23
Organization 1    11001000  00010111  0001001
0  00000000    200.23.18.0/23
Organization 2    11001000  00010111  0001010
0  00000000    200.23.20.0/23
...                                          .....                                   ....                ....
Organization 7    11001000  00010111  0001111
0  00000000    200.23.30.0/23

4-65
Hierarchical addressing: route aggregation
ĀSend me anything
with addresses 
beginning 
200.23.16.0/20ā
200.23.16.0/23
200.23.18.0/23
200.23.30.0/23
Fly-By-Night-ISP
Organization 0
Organization 7
Internet
Organization 1
ISPs-R-Us
ĀSend me anything
with addresses 
beginning 
199.31.0.0/16ā
200.23.20.0/23
Organization 2
.
.
.
.
.
.
hierarchical addressing allows efficientadvertisement of routing information:
ggg
200.23.16.0/20
199.31.0.0/16
2023.10.18

4-66
ISPs-R-Us has a more specific route to Organization 1
ĀSend me anything
with addresses beginning 
200.23.16.0/20ā
200.23.16.0/23
200.23.18.0/23
200.23.30.0/23
Fly-By-Night-ISP
Organization 0
Organization 7
Internet
Organization 1
ISPs-R-Us
ĀSend me anything
with addresses beginning 
199.31.0.0/16 or 200.23.18.0/23
ā
200.23.20.0/23
Organization 2
.
.
.
.
.
.
Hierarchical addressing: more specific routes
200.23.16.0/20
199.31.0.0/16
200.23.18.0/23
Organization 1

4-67
IP addressing: the last word...
Q:how does an ISP get block of addresses?
A:ICANN: Internet Corporation for Assigned 
Names and Numbers http://www.icann.org/
allocates addresses
manages DNS
assigns domain names, resolves disputes

4-68
NAT: network address translation
10.0.0.1
10.0.0.2
10.0.0.3
10.0.0.4
138.76.29.7
local network
(e.g., home network)
10.0.0/24
rest of
Internet
datagrams with source or 
destinationin this network
have 10.0.0/24 address for 
source, destination (as usual)
alldatagramsleavinglocal
network have samesingle source 
NAT IP address: 138.76.29.7,different 
source port numbers
1

4-69
motivation:local network uses just oneIP address as far as 
outside world is concerned:
range of addresses not needed from ISP:  just one IP address 
for all devices
can change addresses of devices in local network without 
notifying outside world
can change ISP without changing addresses of devices in local 
network
devices inside local net not explicitly addressable, visible by 
outside world (a security plus)
NAT: network address translation

4-70
10.0.0.1
10.0.0.2
10.0.0.3
S: 10.0.0.1, 3345
D: 128.119.40.186, 80
1
10.0.0.4
138.76.29.7
1:host 10.0.0.1 
sends datagram to 
128.119.40.186, 80
NAT translation table
WAN side addrLAN side addr
138.76.29.7, 500110.0.0.1, 3345
......                                         ......
S: 128.119.40.186, 80 
D: 10.0.0.1, 3345
4
S: 138.76.29.7, 5001
D: 128.119.40.186, 80
2
2:NAT router
changes datagram
source addr from
10.0.0.1, 3345 to
138.76.29.7, 5001,
updates table
S: 128.119.40.186, 80 
D: 138.76.29.7, 5001
3
3:reply arrives
dest. address:
138.76.29.7, 5001
4:NAT router
changes datagram
dest addr from
138.76.29.7, 5001 to 10.0.0.1, 3345 
NAT: network address translation
1
㊫
࡛
IP
㤳ത
IP
㤳ത㖁㔌ᮠ䟿
A10.x.x.x/810.0.0.0~10.255.255.2551
B172.16-32.x.x/16172.16.0.0~172.31.255.255 16
C192.168.x.x/24192.168.0.0~
192.168.255.255 255

4-71
implementation:NAT router must:
outgoing datagrams: replace(source IP address, port #) of every 
outgoing datagram to (NAT IP address, new port #)
. . . remote clients/servers will respond using (NAT IP address, new port #) 
as destination addr
remember (in NAT translation table)every (source IP address, port #)  
to (NAT IP address, new port #) translation pair
incoming datagrams: replace(NAT IP address, new port #) in dest
fields of every incoming datagram with corresponding (source IP 
address, port #) stored in NAT table
NAT: network address translation

4-72
z16-bit port-number field: 
60,000 simultaneous connections with a singleLAN-side 
address!
zNAT is controversial:
routers should only process up to layer 3
violates end-to-end argument
NAT possibility must be taken into account by app designers, e.g., P2P 
applications
address shortage should instead be solved by IPv6
NAT: network address translation

4-73
NAT traversal problem
zclient wants to connect to server with 
address 10.0.0.1
server address 10.0.0.1 local to LAN (client 
can
䇻t use it as destination addr)
only one externally visible NATed address: 
138.76.29.7
zsolution1:statically configure NAT to 
forward incoming connection requests 
at given port to server
e.g., (123.76.29.7, port 2500) always 
forwarded to 10.0.0.1 port 25000
10.0.0.1
10.0.0.4
NAT 
router
138.76.29.7
client
?

4-74
NAT traversal problem
zsolution 2:Universal Plug and Play (UPnP) 
Internet Gateway Device (IGD) Protocol.  
Allows NATed host to:
learn public IP address (138.76.29.7)
add/remove port mappings (with lease 
times)
i.e., automate static NAT port map 
configuration
10.0.0.1
NAT 
router
Run IGD 
protocol

4-75
NAT traversal problem
zsolution 3:relaying(used in Skype)
NATed client establishes connection to relay
external client connects to relay
relay bridges packets between to connections
138.76.29.7
client
1.connection to
relay initiated
by NATed host
2.connection to
relay initiated
by client
3.relaying 
established
p
NAT 
router
10.0.0.1

How to make a hole
zThe firewall refuses incoming TCP conn.
zthe firewall refuses the first incoming UDP.
zIf A and B locate behind their NAT, respectively, how to 
communicate directly via a hole?
76
peer
192.168.1.10
10.0.0.10
peer
How to let them 
communicate 
directly?
NAT
NAT
peerpeer
NAT

ᢃ⍎ᯩ⌅:
zFull Cone(ޘ䭕ර)
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞ
ᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort), ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ
䜭ሶ֯⭘䘉ሩ
(PublicIP:PublicPort)DŽ↔ཆˈԫօཆ䜘ѫᵪਚ㾱⸕䚃䘉њ(PublicIP:PublicPort)
ቡਟԕਁ䘱ᮠᦞ㔉(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव.
4-77
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
ԫ᜿⸕䚃
䈕ൠ൰ㄟ
ਓⲴཆ䜘
ѫᵪ˄н
䲀ཆ䜘IP
઼ㄟਓ˅

zRestricted Cone
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort), ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ䜭ሶ
֯⭘䘉ሩ(PublicIP:PublicPort)DŽ↔ཆˈྲ᷌ԫօཆ䜘ѫᵪᜣ㾱ਁ䘱ᮠᦞ㔉䘉њ޵㖁ѫᵪˈਚ㾱⸕
䚃䘉њ(PublicIP:PublicPort)ᒦф޵㖁ѫᵪѻࡽᴮ⭘䘉њsocketੁ䘉њཆ䜘ѫᵪIPਁ䘱䗷ᮠᦞ
DŽਚ㾱┑䏣䘉єњᶑԦˈ䘉њཆ䜘ѫᵪቡਟԕ⭘㠚ᐡⲴ
(IP, ԫօㄟਓ)ਁ䘱ᮠᦞ㔉
(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव
4-78
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
ᴮ㓿⭘䈕ൠ
൰ㄟਓ䙊ؑ
䗷Ⲵཆ䜘ѫ
ᵪ˄䲀IPˈ
н䲀ㄟਓ˅

zPort Restricted Cone
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP:LocalPort) ᅮа⅑֯⭘䘉њsocket㔉ཆ䜘ѫᵪਁ䘱ᮠᦞᰦ
NATՊ㔉ަ࠶䝽ањޜ㖁(PublicIP:PublicPort),ԕਾ⭘䘉њsocketੁཆ䶒ԫօѫᵪਁ䘱ᮠᦞ䜭ሶ֯
⭘䘉ሩ(PublicIP:PublicPort)DŽ↔ཆˈྲ᷌ԫօཆ䜘ѫᵪᜣ㾱ਁ䘱ᮠᦞ㔉䘉њ޵㖁ѫᵪˈਚ㾱⸕䚃
䘉њ(PublicIP:PublicPort)ᒦф޵㖁ѫᵪѻࡽᴮ⭘䘉њsocketੁ䘉њཆ䜘ѫᵪ(IP, Port)ਁ
䘱䗷ᮠᦞDŽਚ㾱┑䏣䘉єњᶑԦˈ䘉њཆ䜘ѫᵪቡਟԕ⭘㠚ᐡⲴ
(IP,Port)ਁ䘱ᮠᦞ㔉
(PublicIP:PublicPort)ˈ޵㖁Ⲵѫᵪቡ㜭᭦ࡠ䘉њᮠᦞव.
4-79
UDP socket
(LocalIP:LocalPort) 
(PublicIP: PublicPort)
(IP, Port)
ᴮ㓿⭘䈕ൠ൰
ㄟਓ䙊ؑ䗷Ⲵ
ཆ䜘ѫᵪ˄䲀
IP઼ㄟਓ˅

zSymmetric˄ሩ〠ᖒ˅
޵㖁ѫᵪᔪ・ањUDP socket(LocalIP, LocalPort),ᖃ⭘䘉њsocketᅮа⅑ਁᮠᦞ㔉ཆ䜘ѫᵪ1ᰦ, 
NATѪަ᱐ሴањ(PublicIP1,Port1),ԕਾ޵㖁ѫᵪਁ䘱㔉ཆ䜘ѫᵪ1Ⲵᡰᴹᮠᦞ䜭ᱟ⭘䘉њ
(PublicIP1,Port1)˗ྲ᷌޵㖁ѫᵪ਼ᰦ⭘䘉њsocket㔉ཆ䜘ѫᵪ2ਁ䘱ᮠᦞˈᅮа⅑ਁ䘱ᰦˈ
NATՊѪަ࠶䝽ањ(PublicIP2,Port2), ԕਾ޵㖁ѫᵪਁ䘱㔉ཆ䜘ѫᵪ2Ⲵᡰᴹᮠᦞ䜭ᱟ⭘䘉њ
(PublicIP2,Port2).
4-80
UDP socket
(LocalIP:LocalPort) 
(IP, Port)
(PublicIP2: PublicPort2)
(IP, Port)
(PublicIP,Port)
оཆ䜘ѫᵪ᥶
㔁DŽࡽ䶒ࠐ⿽
ᯩᔿ⋑ᴹ᥶㔁

4-81
4.1 introduction
4.2 virtual circuit and datagram 
networks
4.3 what
䇻s inside a router
4.4 IP: Internet Protocol
datagram format
IPv4 addressing
IPv6
4.5 routing algorithms
link state
distance vector
hierarchical routing
4.6 routing in the Internet
RIP
OSPF
BGP
4.7 broadcast and multicast routing
Chapter 4: outline

4-82
IPv6: motivation
zinitial motivation:32-bit address space soon to 
be completely allocated.  
zadditional motivation:
header format helps speed processing/forwarding
header changes to facilitate QoS
IPv6 datagram format: 
fixed-length 40 byte header
no fragmentation allowed

4-83
IPv6 datagram format
priority:identify priority among datagrams in flow
flow Label:identify datagrams in same Āflow.ā
(concept ofĀflowānot well defined).
next header:identify upper layer protocol for data
data
destination address
(128 bits)
source address
(128 bits)
payload len
next hdr
hop limit
flow label
pri
ver
32 bits

4-84
Other changes from IPv4
zchecksum:removed entirely to reduce processing time at 
each hop
zoptions:allowed, but outside of header, indicated by 䇾Next 
Header䇿field
zICMPv6:new version of ICMP
additional message types, e.g. 䇾Packet Too Big䇿
multicast group management functions

4-85
Transition from IPv4 to IPv6
znot all routers can be upgraded simultaneously
no 䇾flag days䇿
how will network operate with mixed IPv4 and 
IPv6 routers? 
ztunneling:IPv6 datagram carried as payloadin 
IPv4 datagram among IPv4 routers
IPv4 source, 
dest addr
IPv4 header fields 
IPv4 datagram
IPv6 datagram
IPv4 payload 
UDP/TCP payload
IPv6 source dest addr
IPv6 header fields
IPv6
IPv6
IPv4

4-87
flow: X
src: A
dest: F
data
A-to-B:
IPv6
Flow: X
Src: A
Dest: F
data
src:B
dest: E
B-to-C:
IPv6 inside
IPv4
E-to-F:
IPv6
flow: X
src: A
dest: F
data
B-to-C:
IPv6 inside
IPv4
Flow: X
Src: A
Dest: F
data
src:B
dest: E
physical view:
A
B
IPv6
IPv6
E
IPv6
IPv6
F
C
D
logical view:
IPv4 tunnel 
connecting IPv6 routers
E
IPv6
IPv6
F
A
B
IPv6
IPv6
Tunneling
IPv4
IPv4
Double protocol stack

4.1 Overview of Network layer
data plane
control plane
4.2 What
ÿs inside a router
4.3 IP: Internet Protocol
datagram format
fragmentation
IPv4 addressing
network address translation
IPv6
4.4 Generalized Forward and SDN
match
action
OpenFlow  examples of match-
plus-action in action
Chapter 4: outline
4-89

Generalized Forwarding and SDN
2
3
0100 1101
values in arriving
packet䇻s header
logically-centralized routing controller
1
control plane
data plane
Each router contains a flow table that is computed and 
distributed by a logically centralized routing controller
local flow table
headers  counters  actions
4-90

zĨůŽǁ͗ĚĞĨŝŶĞĚďLJŚĞĂĚĞƌĨŝĞůĚǀĂůƵĞƐ;ŝŶůŝŶŬͲ͕ŶĞƚǁŽƌŬͲ͕ƚƌĂŶƐƉŽƌƚͲůĂLJĞƌĨŝĞůĚƐͿ
zŐĞŶĞƌĂůŝnjĞĚĨŽƌǁĂƌĚŝŶŐ͗ƐŝŵƉůĞƉĂĐŬĞƚͲŚĂŶĚůŝŶŐƌƵůĞƐ
ŵĂƚĐŚ͗ƉĂƚƚĞƌŶǀĂůƵĞƐŝŶƉĂĐŬĞƚŚĞĂĚĞƌĨŝĞůĚƐ
ĂĐƚŝŽŶƐ͗ĨŽƌŵĂƚĐŚĞĚƉĂĐŬĞƚ͗ĚƌŽƉ͕ĨŽƌǁĂƌĚ͕ŵŽĚŝĨLJ͕ŵĂƚĐŚĞĚƉĂĐŬĞƚŽƌƐĞŶĚ
ŵĂƚĐŚĞĚƉĂĐŬĞƚƚŽĐŽŶƚƌŽůůĞƌ
ƉƌŝŽƌŝƚLJ͗ĚŝƐĂŵďŝŐƵĂƚĞŽǀĞƌůĂƉƉŝŶŐƉĂƚƚĞƌŶƐ
ĐŽƵŶƚĞƌƐ͗ηďLJƚĞƐĂŶĚηƉĂĐŬĞƚƐ
Flow table abstraction
ZŽƵƚĞƌ͛ƐĨůŽǁƚĂďůĞĚĞĨŝŶĞ
ƌŽƵƚĞƌ͛ƐŵĂƚĐŚнĂĐƚŝŽŶƌƵůĞƐ
&ůŽǁƚĂďůĞ
ŵĂƚĐŚ
ĂĐƚŝŽŶ
Network Layer: 4-91

zĨůŽǁ͗ĚĞĨŝŶĞĚďLJŚĞĂĚĞƌĨŝĞůĚƐ
zŐĞŶĞƌĂůŝnjĞĚĨŽƌǁĂƌĚŝŶŐ͗ƐŝŵƉůĞƉĂĐŬĞƚͲŚĂŶĚůŝŶŐƌƵůĞƐ
ŵĂƚĐŚ͗ƉĂƚƚĞƌŶǀĂůƵĞƐŝŶƉĂĐŬĞƚŚĞĂĚĞƌĨŝĞůĚƐ
ĂĐƚŝŽŶƐ͗ĨŽƌŵĂƚĐŚĞĚƉĂĐŬĞƚ͗ĚƌŽƉ͕ĨŽƌǁĂƌĚ͕ŵŽĚŝĨLJ͕ŵĂƚĐŚĞĚƉĂĐŬĞƚŽƌ
ƐĞŶĚŵĂƚĐŚĞĚƉĂĐŬĞƚƚŽĐŽŶƚƌŽůůĞƌ
ƉƌŝŽƌŝƚLJ͗ĚŝƐĂŵďŝŐƵĂƚĞŽǀĞƌůĂƉƉŝŶŐƉĂƚƚĞƌŶƐ
ĐŽƵŶƚĞƌƐ͗ηďLJƚĞƐĂŶĚηƉĂĐŬĞƚƐ
&ůŽǁƚĂďůĞ
ŵĂƚĐŚ
ĂĐƚŝŽŶ
ϭ
Ϯ
ϯ
κ
* : wildcard
ƐƌĐсϭϬ͘ϭ͘Ϯ͘ϯ͕ĚĞƐƚсΎ͘Ύ͘Ύ͘ΎƐĞŶĚƚŽĐŽŶƚƌŽůůĞƌ
ƐƌĐсϭ͘Ϯ͘Ύ͘Ύ͕ĚĞƐƚсΎ͘Ύ͘Ύ͘Ύ ĚƌŽƉ
ƐƌĐсΎ͘Ύ͘Ύ͘Ύ͕ĚĞƐƚсϯ͘κ͘Ύ͘ΎĨŽƌǁĂƌĚ;ϮͿ
Flow table abstraction
Network Layer: 4-92

OpenFlow: Flow Table Entries
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ZƵůĞĐƚŝŽŶ^ƚĂƚƐ
ϭ͘ &ŽƌǁĂƌĚƉĂĐŬĞƚƚŽƉŽƌƚ;ƐͿ
Ϯ͘ ŶĐĂƉƐƵůĂƚĞĂŶĚĨŽƌǁĂƌĚƚŽĐŽŶƚƌŽůůĞƌ
ϯ͘ ƌŽƉƉĂĐŬĞƚ
κ͘ ^ĞŶĚƚŽŶŽƌŵĂůƉƌŽĐĞƐƐŝŶŐƉŝƉĞůŝŶĞ
ρ͘ DŽĚŝĨLJ&ŝĞůĚƐ
WĂĐŬĞƚнďLJƚĞĐŽƵŶƚĞƌƐ
>ŝŶŬůĂLJĞƌ
EĞƚǁŽƌŬůĂLJĞƌ
dƌĂŶƐƉŽƌƚůĂLJĞƌ

Destination-based forwarding:
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ĐƚŝŽŶ
ΎΎ  Ύ Ύ Ύ
ρϭ͘ς͘Ϭ͘Θ
ΎΎΎ
ƉŽƌƚς
Examples
IP datagrams destined to IP address  51.6.0.8 should be 
forwarded to router output port 6 
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
&ŽƌǁĂƌĚ
ΎΎ  Ύ Ύ Ύ Ύ Ύ Ύ
ϮϮ
ĚƌŽƉ
Firewall:
do not forward (block) all datagrams destined to TCP  port 22
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
&ŽƌǁĂƌĚ
ΎΎ  Ύ Ύ
ϭϮΘ͘ϭϭε͘ϭ͘ϭ
ΎΎΎΎ
ĚƌŽƉ
do not forward (block) all datagrams sent by host 128.119.1.1

Destination-based layer 2(switch) forwarding:
Ύ
^ǁŝƚĐŚ
WŽƌƚ
D
ƐƌĐ
D
ĚƐƚ
ƚŚ
ƚLJƉĞ
s>E
/
/W
^ƌĐ
/W
Ɛƚ
/W
WƌŽƚ
dW
ƐƉŽƌƚ
dW
ĚƉŽƌƚ
ĐƚŝŽŶ
ΎΎΎΎΎΎΎΎ
ƉŽƌƚϯ
Examples
layer 2 frames from MAC address 22:A7:23:11:E1:02 should be 
forwarded to output port 3 
ϮϮ͗ϳ͗Ϯϯ͗
ϭϭ͗ϭ͗ϬϮ
4-95

KƉĞŶ&ůŽǁĂďƐƚƌĂĐƚŝŽŶ
ZŽƵƚĞƌ
•ŵĂƚĐŚ͗ůŽŶŐĞƐƚĚĞƐƚŝŶĂƚŝŽŶ/W
ƉƌĞĨŝdž
•ĂĐƚŝŽŶ͗ĨŽƌǁĂƌĚŽƵƚĂůŝŶŬ
^ǁŝƚĐŚ
•ŵĂƚĐŚ͗ĚĞƐƚŝŶĂƚŝŽŶD
ĂĚĚƌĞƐƐ
•ĂĐƚŝŽŶ͗ĨŽƌǁĂƌĚŽƌĨůŽŽĚ
&ŝƌĞǁĂůů
•ŵĂƚĐŚ͗/WĂĚĚƌĞƐƐĞƐĂŶĚdWͬhW
ƉŽƌƚŶƵŵďĞƌƐ
•ĂĐƚŝŽŶ͗ƉĞƌŵŝƚŽƌĚĞŶLJ
Ed
•ŵĂƚĐŚ͗/WĂĚĚƌĞƐƐĂŶĚƉŽƌƚ
•ĂĐƚŝŽŶ͗ƌĞǁƌŝƚĞĂĚĚƌĞƐƐĂŶĚƉŽƌƚ
ŵĂƚĐŚнĂĐƚŝŽŶ͗ƵŶŝĨŝĞƐĚŝĨĨĞƌĞŶƚŬŝŶĚƐŽĨĚĞǀŝĐĞƐ
4-96

IP Src = 10.3.0.5
IP Dst = 10.2.*.*
forward(3)
match
action
ingress port = 1
IP Dst = 10.2.0.4
ingress port = 2
IP Dst = 10.2.0.3
forward(4)
match
action
forward(3)
ingress port = 1
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(4)
match
action
KƉĞŶ&ůŽǁĞdžĂŵƉůĞ
Host h1
10.1.0.1
Host h2
10.1.0.2
Host h4
10.2.0.4
Host h3
10.2.0.3
Host h5
10.3.0.5
s1
s2
s3
1
2
3
4
1
2
3
4
1
2
3
4
Host h6
10.3.0.6
3
s2
1
33
44
s
controllercontrollertll
Example: datagrams from 
hosts h5 and h6 should 
be sent to h3 or h4, via s1 
and from there to s2
IP Src = 10.3.0.6
IP Dst = 10.2.*.*
forward(4)
2023.10.23

OpenFlow example
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(3)
match
action
ingress port = 2
IP Dst = 10.2.0.3
ingress port = 2
IP Dst = 10.2.0.4
forward(3)
match
action
forward(4)
ingress port = 1
IP Src = 10.3.*.*
IP Dst = 10.2.*.*
forward(4)
match
action
Host h1
10.1.0.1
Host h2
10.1.0.2
Host h4
10.2.0.4
Host h3
10.2.0.3
Host h5
10.3.0.5
s1
s2
s3
1
2
3
4
1
2
3
4
1
2
3
4
Host h6
10.3.0.6
controllercontrollertll
s1sss
1
4
s2s
4
sss
1
4
KƌĐŚĞƐƚƌĂƚĞĚƚĂďůĞƐĐĂŶĐƌĞĂƚĞ
ŶĞƚǁŽƌŬͲǁŝĚĞďĞŚĂǀŝŽƌ͕Ğ͘Ő͕͗͘
ĚĂƚĂŐƌĂŵƐĨƌŽŵŚŽƐƚƐŚρĂŶĚ
ŚςƐŚŽƵůĚďĞƐĞŶƚƚŽŚϯŽƌŚκ͕
ǀŝĂƐϭĂŶĚĨƌŽŵƚŚĞƌĞƚŽƐϮ
Network Layer: 4-98

Middleboxes
͞ĂŶLJŝŶƚĞƌŵĞĚŝĂƌLJďŽdžƉĞƌĨŽƌŵŝŶŐĨƵŶĐƚŝŽŶƐĂƉĂƌƚ
ĨƌŽŵŶŽƌŵĂů͕ƐƚĂŶĚĂƌĚĨƵŶĐƚŝŽŶƐŽĨĂŶ/WƌŽƵƚĞƌŽŶ
ƚŚĞĚĂƚĂƉĂƚŚďĞƚǁĞĞŶĂƐŽƵƌĐĞŚŽƐƚĂŶĚ
ĚĞƐƚŝŶĂƚŝŽŶŚŽƐƚ͟
DŝĚĚůĞďŽdž;Z&ϯϮϯκͿ

Middleboxes everywhere!
ĞŶƚĞƌƉƌŝƐĞ
ŶĞƚǁŽƌŬ
ŶĂƚŝŽŶĂůŽƌŐůŽďĂů/^W
ĚĂƚĂĐĞŶƚĞƌ
ŶĞƚǁŽƌŬ
Ed͗ŚŽŵĞ͕
ĐĞůůƵůĂƌ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů
&ŝƌĞǁĂůůƐ͕/^͗ĐŽƌƉŽƌĂƚĞ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů͕ƐĞƌǀŝĐĞƉƌŽǀŝĚĞƌƐ͕
/^WƐ
>ŽĂĚďĂůĂŶĐĞƌƐ͗
ĐŽƌƉŽƌĂƚĞ͕ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌ͕ĚĂƚĂĐĞŶƚĞƌ͕
ŵŽďŝůĞŶĞƚƐ
ĂĐŚĞƐ͗ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌ͕ŵŽďŝůĞ͕EƐ
ƉƉůŝĐĂƚŝŽŶͲ
ƐƉĞĐŝĨŝĐ͗ƐĞƌǀŝĐĞ
ƉƌŽǀŝĚĞƌƐ͕
ŝŶƐƚŝƚƵƚŝŽŶĂů͕
E

Middleboxes
ŝŶŝƚŝĂůůLJ͗ƉƌŽƉƌŝĞƚĂƌLJ;ĐůŽƐĞĚͿŚĂƌĚǁĂƌĞƐŽůƵƚŝŽŶƐ
ŵŽǀĞƚŽǁĂƌĚƐ͞ǁŚŝƚĞďŽdž͟ŚĂƌĚǁĂƌĞŝŵƉůĞŵĞŶƚŝŶŐŽƉĞŶW/
ŵŽǀĞĂǁĂLJĨƌŽŵƉƌŽƉƌŝĞƚĂƌLJŚĂƌĚǁĂƌĞƐŽůƵƚŝŽŶƐ
ƉƌŽŐƌĂŵŵĂďůĞůŽĐĂůĂĐƚŝŽŶƐǀŝĂŵĂƚĐŚнĂĐƚŝŽŶ
ŵŽǀĞƚŽǁĂƌĚƐŝŶŶŽǀĂƚŝŽŶͬĚŝĨĨĞƌĞŶƚŝĂƚŝŽŶŝŶƐŽĨƚǁĂƌĞ
^E͗;ůŽŐŝĐĂůůLJͿĐĞŶƚƌĂůŝnjĞĚĐŽŶƚƌŽůĂŶĚĐŽŶĨŝŐƵƌĂƚŝŽŶŵĂŶĂŐĞŵĞŶƚ
ŽĨƚĞŶŝŶƉƌŝǀĂƚĞͬƉƵďůŝĐĐůŽƵĚ
ŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶƐǀŝƌƚƵĂůŝnjĂƚŝŽŶ;E&sͿ͗ƉƌŽŐƌĂŵŵĂďůĞƐĞƌǀŝĐĞƐŽǀĞƌ
ǁŚŝƚĞďŽdžŶĞƚǁŽƌŬŝŶŐ͕ĐŽŵƉƵƚĂƚŝŽŶ͕ƐƚŽƌĂŐĞ

The IP hourglass
/W
dW
hW
,ddW
^DdW
Yh/
^,
ZdW
͙
ƚŚĞƌŶĞƚ
tŝ&ŝ ůƵĞƚŽŽƚŚ
WWW
WW
͙
ĐŽƉƉĞƌƌĂĚŝŽĨŝďĞƌ
/ŶƚĞƌŶĞƚ͛Ɛ͞ƚŚŝŶǁĂŝƐƚ͗͟
ŽŶĞŶĞƚǁŽƌŬůĂLJĞƌ
ƉƌŽƚŽĐŽů͗/W
ŵƵƐƚďĞŝŵƉůĞŵĞŶƚĞĚ
ďLJĞǀĞƌLJ;ďŝůůŝŽŶƐͿŽĨ
/ŶƚĞƌŶĞƚͲĐŽŶŶĞĐƚĞĚ
ĚĞǀŝĐĞƐ
ŵĂŶLJƉƌŽƚŽĐŽůƐ
ŝŶƉŚLJƐŝĐĂů͕ůŝŶŬ͕
ƚƌĂŶƐƉŽƌƚ͕ĂŶĚ
ĂƉƉůŝĐĂƚŝŽŶ
ůĂLJĞƌƐ

The IP hourglass, at middle age
/W
dW
hW
,ddW
^DdW
Yh/
^,
ZdW
͙
ƚŚĞƌŶĞƚ
tŝ&ŝ ůƵĞƚŽŽƚŚ
WWW
WW
͙
ĐŽƉƉĞƌƌĂĚŝŽĨŝďĞƌ
/ŶƚĞƌŶĞƚ͛ƐŵŝĚĚůĞĂŐĞ
͞ůŽǀĞŚĂŶĚůĞƐ͍͟
ŵŝĚĚůĞďŽdžĞƐ͕
ŽƉĞƌĂƚŝŶŐŝŶƐŝĚĞƚŚĞ
ŶĞƚǁŽƌŬ
&ŝƌĞǁĂůůƐ
ĐĂĐŚŝŶŐ

Architectural Principles of the Internet
͞DĂŶLJŵĞŵďĞƌƐŽĨƚŚĞ/ŶƚĞƌŶĞƚĐŽŵŵƵŶŝƚLJǁŽƵůĚĂƌŐƵĞƚŚĂƚƚŚĞƌĞŝƐŶŽĂƌĐŚŝƚĞĐƚƵƌĞ͕ďƵƚŽŶůLJĂƚƌĂĚŝƚŝŽŶ͕
ǁŚŝĐŚǁĂƐŶŽƚǁƌŝƚƚĞŶĚŽǁŶĨŽƌƚŚĞĨŝƌƐƚϮρLJĞĂƌƐ;ŽƌĂƚůĞĂƐƚŶŽƚďLJƚŚĞ/Ϳ͘,ŽǁĞǀĞƌ͕ŝŶǀĞƌLJŐĞŶĞƌĂůƚĞƌŵƐ͕
ƚŚĞĐŽŵŵƵŶŝƚLJďĞůŝĞǀĞƐƚŚĂƚ
Z&ϭερΘ
ƚŚĞŐŽĂůŝƐĐŽŶŶĞĐƚŝǀŝƚLJ͕ƚŚĞƚŽŽůŝƐƚŚĞ/ŶƚĞƌŶĞƚ
WƌŽƚŽĐŽů͕ĂŶĚƚŚĞŝŶƚĞůůŝŐĞŶĐĞŝƐĞŶĚƚŽĞŶĚƌĂƚŚĞƌƚŚĂŶŚŝĚĚĞŶŝŶƚŚĞ
ŶĞƚǁŽƌŬ͘͟
dŚƌĞĞĐŽƌŶĞƌƐƚŽŶĞďĞůŝĞĨƐ͗
ƐŝŵƉůĞĐŽŶŶĞĐƚŝǀŝƚLJ
/WƉƌŽƚŽĐŽů͗ƚŚĂƚŶĂƌƌŽǁǁĂŝƐƚ
ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉůĞdžŝƚLJĂƚŶĞƚǁŽƌŬĞĚŐĞ

The end-end argument
ƐŽŵĞŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶĂůŝƚLJ;Ğ͘Ő͕͘ƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ͕ĐŽŶŐĞƐƚŝŽŶͿ
ĐĂŶďĞŝŵƉůĞŵĞŶƚĞĚŝŶŶĞƚǁŽƌŬ͕ŽƌĂƚŶĞƚǁŽƌŬĞĚŐĞ
ĞŶĚͲĞŶĚŝŵƉůĞŵĞŶƚĂƚŝŽŶŽĨƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ĂƉƉůŝĐĂƚŝŽŶ
ƚƌĂŶƐƉŽƌƚ
ŶĞƚǁŽƌŬ
ĚĂƚĂůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŶĞƚǁŽƌŬ
ůŝŶŬ
ƉŚLJƐŝĐĂů
ŚŽƉͲďLJͲŚŽƉ;ŝŶͲŶĞƚǁŽƌŬͿŝŵƉůĞŵĞŶƚĂƚŝŽŶŽĨƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ

The end-end argument
͞dŚĞĨƵŶĐƚŝŽŶŝŶƋƵĞƐƚŝŽŶĐĂŶĐŽŵƉůĞƚĞůLJĂŶĚĐŽƌƌĞĐƚůLJďĞŝŵƉůĞŵĞŶƚĞĚŽŶůLJ
ǁŝƚŚƚŚĞŬŶŽǁůĞĚŐĞĂŶĚŚĞůƉŽĨƚŚĞĂƉƉůŝĐĂƚŝŽŶƐƚĂŶĚŝŶŐĂƚƚŚĞĞŶĚƉŽŝŶƚƐŽĨƚŚĞ
ĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵ͘dŚĞƌĞĨŽƌĞ͕ƉƌŽǀŝĚŝŶŐƚŚĂƚƋƵĞƐƚŝŽŶĞĚĨƵŶĐƚŝŽŶĂƐĂ
ĨĞĂƚƵƌĞŽĨƚŚĞĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵŝƚƐĞůĨŝƐŶŽƚƉŽƐƐŝďůĞ͘;^ŽŵĞƚŝŵĞƐĂŶ
ŝŶĐŽŵƉůĞƚĞǀĞƌƐŝŽŶŽĨƚŚĞĨƵŶĐƚŝŽŶƉƌŽǀŝĚĞĚďLJƚŚĞĐŽŵŵƵŶŝĐĂƚŝŽŶƐLJƐƚĞŵŵĂLJ
ďĞƵƐĞĨƵůĂƐĂƉĞƌĨŽƌŵĂŶĐĞĞŶŚĂŶĐĞŵĞŶƚ͘Ϳ
tĞĐĂůůƚŚŝƐůŝŶĞŽĨƌĞĂƐŽŶŝŶŐĂŐĂŝŶƐƚůŽǁͲůĞǀĞůĨƵŶĐƚŝŽŶŝŵƉůĞŵĞŶƚĂƚŝŽŶƚŚĞ͞ĞŶĚͲ
ƚŽͲĞŶĚĂƌŐƵŵĞŶƚ͘͟
^ĂůƚnjĞƌ͕ZĞĞĚ͕ůĂƌŬϭεΘϭ
ƐŽŵĞŶĞƚǁŽƌŬĨƵŶĐƚŝŽŶĂůŝƚLJ;Ğ͘Ő͕͘ƌĞůŝĂďůĞĚĂƚĂƚƌĂŶƐĨĞƌ͕ĐŽŶŐĞƐƚŝŽŶͿ
ĐĂŶďĞŝŵƉůĞŵĞŶƚĞĚŝŶŶĞƚǁŽƌŬ͕ŽƌĂƚŶĞƚǁŽƌŬĞĚŐĞ

Where’s the intelligence?
ϮϬ
ƚŚ
ĐĞŶƚƵƌLJƉŚŽŶĞŶĞƚ͗
•ŝŶƚĞůůŝŐĞŶĐĞͬĐŽŵƉƵƚŝŶŐĂƚ
ŶĞƚǁŽƌŬƐǁŝƚĐŚĞƐ
/ŶƚĞƌŶĞƚ;ƉƌĞͲϮϬϬρͿ
•ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉƵƚŝŶŐĂƚ
ĞĚŐĞ
/ŶƚĞƌŶĞƚ;ƉŽƐƚͲϮϬϬρͿ
•ƉƌŽŐƌĂŵŵĂďůĞŶĞƚǁŽƌŬĚĞǀŝĐĞƐ
•ŝŶƚĞůůŝŐĞŶĐĞ͕ĐŽŵƉƵƚŝŶŐ͕ŵĂƐƐŝǀĞ
ĂƉƉůŝĐĂƚŝŽŶͲůĞǀĞůŝŶĨƌĂƐƚƌƵĐƚƵƌĞĂƚĞĚŐĞ

Chapter 4: done!
Question: how do forwarding tables 
(destination-based forwarding) or flow 
tables (generalized forwarding) computed?
Answer: by the control plane (next chapter)
4.1 Overview of Network layer: data plane 
and control plane
4.2 What䇻s inside a router
4.3 IP: Internet Protocol
•datagram format
•fragmentation
•IPv4 addressing
•NAT
•IPv6
4.4 Generalized Forward and SDN
•match plus action
•OpenFlow example
4-
108

Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
