# Chapter3 Transport Layer

> 主题库：计算机网络　｜　源文件：Chapter3 Transport Layer.pdf　｜　61,013 字符　｜　157 个片段　｜　更新于 2026-11-11


Computer Networking
谢逸
中山大学·计算机学院
2024. Fall

A note on the use of these Powerpoint slides:
We’re making these slides freely available to all (faculty, students, readers). They’re in 
PowerPoint form so you see the animations; and can add, modify, and delete slides  
(including this one) and slide content to suit your needs. They obviously represent a lot
of work on our part. In return for use, we only ask the following:
If you use these slides (e.g., in a class) that you mention their source (after all, we’d 
like people to use our book!)
If you post any slides on a www site, that you note that they are adapted from (or 
perhaps identical to) our slides, and note our copyright of this material.
Thanks and enjoy!  JFK/KWR
All material copyright 1996-2016
J.F Kurose and K.W. Ross, All Rights Reserved
Chapter 3
Transport Layer
2-2
Computer Networking: 
A Top-Down Approach 
8
th
edition 
Jim Kurose, Keith Ross
Pearson, 2020

Assignments :
Ch3 (ver8, CN): 3、4、7、14、15、20、22、27、32、36、40
、43、48、52、56、58
Keywords: multiplexing, demultiplexing, reliable data transfer, flow 
control, congestion control, UDP, TCP, Go back n, Selective repeat, 
TCP window, AIMD

3-4
Chapter 3: Transport Layer
our goals: 
understand principles 
behind transport layer 
services:
multiplexing, demultiplexing
reliable data transfer
flow control
congestion control
learn about Internet transport 
layer protocols:
UDP: connectionless transport
TCP: connection-oriented 
reliable transport
TCP congestion control

3-5
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control

3-6
Transport services and protocols
providelogical communicationbetween app 
processes running on different hosts
transport protocols run in end systems 
send side: breaks app messages into 
segments, passes to  network layer
rcv side: reassembles segments into 
messages, passes to app layer
more than one transport protocol available to 
apps
Internet: TCP and UDP
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
20241012

5
4
3
2
1
运输层提供应用进程间的逻辑通信
主机A主机B
应用进程应用进程
路由器1路由器2
AP
1
LAN
2
WAN
AP
2
AP
3
AP
4
IP 层
LAN
1
AP
1
AP
2
AP
4
端口
端口
5
4
3
2
1
IP 协议的作用范围
运输层协议TCP 和UDP 的作用范围
AP
3
Transport vs. network layer
7

3-8
Transport vs. network layer
network layer:logical communication between hosts
transport layer:logical communication between processes 
relies on, enhances, network layer services
网络层：主机间数据交互
传输层：进程间数据交互
Manager
~App
Secretary
~TCP
Staff-
IP

physical
link
network (IP)
application
physical
link
network (IP)
application
transport
Transport Layer Actions
Sender:
app. msg
is passed an application‐
layer message
determines segment 
header fields values
creates segment
passes segment to IP
transport
T
h
T
h
app. msg
Transport Layer: 3-10

physical
link
network (IP)
application
physical
link
network (IP)
application
transport
Transport Layer Actions
transport
Receiver:
app.  msg
extracts application‐layer 
message
checks header values
receives segment from IP
T
h
app. msg
demultiplexes message up 
to application via socket
Transport Layer: 3-11

3-12
Internet transport-layer protocols
reliable, in-order delivery 
(TCP)
congestion control 
flow control
connection setup
unreliable, unordered 
delivery: UDP
no-frills extension of 
“best-effort”IP
services not available: 
delay guarantees
bandwidth guarantees
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

3-13
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
Transport Layer: 3-14

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
HTTP msg
Transport Layer: 3-15

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
HTTP msg
H
t
H
n
HTTP msg
Transport Layer: 3-16

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
HTTP msg
H
t
H
n
Transport Layer: 3-17

transport
physical
link
network
transport
application
physical
link
network
transport
application
physical
link
network
HTTP server
client
1
client
2
P-client
1
P-client
2
Transport Layer: 3-18

3-19
Multiplexing/demultiplexing
process
socket
use header info to deliver
received segments to correct 
socket
demultiplexing at receiver:
handle data from multiple
sockets, add transport 
header (later used for 
demultiplexing)
multiplexing at sender:
transport
application
physical
link
network
P2P1
transport
application
physical
link
network
P4
transport
application
physical
link
network
P3
Multiplexing: Aggregate data from different applications to the TCP and IP layers 
Demultiplexing: Distribute network layer data to different applications

20
App1App2
socket1
socket2
IP
multiplexing
demultiplexing
Multiplexing/demultiplexing
App3
socket3
TCP segments
App1App2
socket1
socket2
IP
App3
socket3
UDP
TCP segments
UDP
multiplexing
demultiplexing

3-21
How demultiplexing works
host receives IP datagrams
each datagram has source IP 
address, destination IP address
each datagram carries one transport-
layer segment
each segment has source, 
destination port number 
host uses IP addresses & port 
numbersto direct segment to 
appropriate socket
source port #   dest port #
32 bits
application
data 
(payload)
other header fields
TCP/UDP segment format
IP payloadIP Hdr
srcIP+dstIP+srcPort+dstPort+protocol

3-22
Connectionless demultiplexing
recall:created socket has 
host-local port #:
DatagramSocket mySocket1        
= new DatagramSocket(12534);
when host receives
UDP segment:
checks destination port 
# in segment
directs UDP segment to 
socket with that port #
recall:when creating 
datagram to sendinto 
UDP socket, must specify
destination IP address
destination port #
IP datagrams with 
same dest. port #,but 
different source IP 
addresses and/or 
source port numbers 
will be directed to same 
socket at dest

3-23
Connectionless demux: example
DatagramSocket
serverSocket = new 
DatagramSocket
(6428);
transport
application
physical
link
network
P3
transport
application
physical
link
network
P1
transport
application
physical
link
network
P4
DatagramSocket 
mySocket1 = new 
DatagramSocket 
(5775);
DatagramSocket 
mySocket2 = new 
DatagramSocket
(9157);
source port: 9157
dest port: 6428
source port: 6428
dest port: 9157
source port: ?
dest port: ?
source port: ?
dest port: ?
接收端程序（P1）无需区分所收到数据是来自哪一个发送端
接收端程序
在本地的一
个镜像
发送端在本地
的镜像。
混合了来自
P3,P4的数据，
无法区分来源

3-24
Connection-oriented demux
TCP socket identified by 4-
tuple: 
source IP address
source port number
dest IP address
dest port number
demux: receiver uses all four 
values to direct segment to 
appropriate socket
server host may support many 
simultaneous TCP sockets:
each socket identified by its own 
4-tuple
web servers have different 
sockets for each connecting 
client
non-persistent HTTP will have 
different socket for each request

3-25
Connection-oriented demux: example
transport
application
physical
link
network
P3
transport
application
physical
link
P4
transport
application
physical
link
network
P2
source IP,port: A,9157
dest IP, port: B,80
source IP,port: B,80
dest IP,port: A,9157
host: IP 
address A
host: IP 
address C
network
P6
P5
P3
source IP,port: C,5775
dest IP,port: B,80
source IP,port: C,9157
dest IP,port: B,80
three segments, all destined to IP address: B,
dest port: 80 are demultiplexed to 
different sockets
server: IP 
address B
Different Apps (Instances)
接收端程序
在本地的一
个镜像
发送端在本
地的镜像。
独立对应每
一个通信对
端应用程序
。可以区分
来源

3-26
Connection-oriented demux: example
transport
application
physical
link
network
P3
transport
application
physical
link
transport
application
physical
link
network
P2
source IP,port: A,9157
dest IP, port: B,80
source IP,port: B,80
dest IP,port: A,9157
host: IP 
address A
host: IP 
address C
server: IP 
address B
network
P3
source IP,port: C,5775
dest IP,port: B,80
source IP,port: C,9157
dest IP,port: B,80
P4
threaded server

Summary
Multiplexing, demultiplexing: based on segment, datagram 
header field values
UDP: demultiplexing using destination port number (only)
TCP: demultiplexing using 4‐tuple: source and destination IP 
addresses, and port numbers
Multiplexing/demultiplexing happen at alllayers
Transport Layer: 3-28

3-29
Chapter 3 outline
3.1 transport-layer 
services
3.2 multiplexing and 
demultiplexing
3.3 connectionless 
transport: UDP
3.4 principles of reliable 
data transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control

3-30
UDP: User Datagram Protocol [RFC 768]
“no frills,”“bare bones”
Internet transport protocol
“best effort”service, UDP 
segments may be:
lost
delivered out-of-order to app
connectionless:
no handshaking between UDP 
sender, receiver
each UDP segment handled 
independently of others
UDP use:
streaming multimedia apps (loss 
tolerant, rate sensitive)
DNS
SNMP
reliable transfer over UDP: 
add reliability at application layer
application-specific error 
recovery!

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
transport
(UDP)
physical
link
network (IP)
application
Transport Layer: 3-31

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
UDP sender actions:
SNMP msg
is passed an application‐
layer message
determines UDP segment 
header fields values
creates UDP segment
passes segment to IP
UDP
h
UDP
h
SNMP msg
Transport Layer: 3-32

SNMP server
SNMP client
transport
(UDP)
physical
link
network (IP)
application
transport
(UDP)
physical
link
network (IP)
application
UDP: Transport Layer Actions
UDP receiver actions:
SNMP msg
extracts application‐layer 
message
checks UDP checksum 
header value
receives segment from IP
UDP
h
SNMP msg
demultiplexes message up 
to application via socket
Transport Layer: 3-33

3-34
UDP: segment header
source port #
dest port #
32 bits
application
data 
(payload)
UDP segment format
length
checksum
length, in bytes of 
UDP segment, 
including header
no connection 
establishment (which 
can add delay)
simple: no connection 
state at sender, receiver
small header size
no congestion control: 
UDP can blast away as 
fast as desired
why is there a UDP?

3-35
UDP checksum
sender:
treat segment contents, 
including header fields,  as 
sequence of 16-bit integers
checksum: addition (one’s 
complement sum) of segment 
contents
sender puts checksum value into 
UDP checksum field
receiver:
compute checksum of received 
segment
check if computed checksum equals 
checksum field value:
NO - error detected
YES - no error detected. But 
maybe errors nonetheless?More 
later ....
Goal:detect “errors”(e.g., flipped bits) in transmitted segment

伪首部 源端口 目的端口
长度
检验和
数据首部
UDP长度源IP 地址    目的IP 地址
017
IP 数据报
字节
44112
12
2222
字节
发送在前
数据首部
UDP 用户数据报
在计算检验和时，临时把“伪首部”和UDP 用户数据报连接在一起。伪
首部仅仅是为了计算检验和。
UDP头部
及净荷
填充0,没有
实质作用

3-38
Internet checksum: example
example: add two 16-bit integers
11  1  1  0  0  1  1  0  0  1  1  0  0  1  1  0
11  1  0  1  0  1  0  1  0  1  0  1  0  1  0  1
1  1  0  1  1  1  0  1  1  1  0  1  1  1  0  1  1
11  0  1  1  1  0  1  1  1  0  1  1  1  1  0  0
10  1  0  0  0  1  0  0  0  1  0  0  0  0  1  1
wraparound
sum
checksum
Note:when adding numbers, a carryout from the most 
significant bit needs to be added to the result
0  0  0  0  0  0  0  0  0  0  0  0  0  0  0  1

Summary: UDP
“no frills” protocol: 
•segments may be lost, delivered out of order
•best effort service: “send and hope for the best”
UDP has its plusses:
•no setup/handshaking needed (no RTT incurred)
•can function when network service is compromised
•helps with reliability (checksum)
build additional functionality on top of UDP in application layer 
(e.g., HTTP/3)

3-40
Chapter 3 outline
3.1 transport-layer 
services
3.2 multiplexing and 
demultiplexing
3.3 connectionless 
transport: UDP
3.4 principles of reliable 
data transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection 
management
3.6 principles of 
congestion control
3.7 TCP congestion 
control

3-43
Principles of reliable data transfer
important in application, transport, link layers
top-10 list of important networking topics!
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)

3-44
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)
Principles of reliable data transfer
important in application, transport, link layers
top-10 list of important networking topics!
?
IP
IP layer
Sending IPs
Receiving IPs
Manager
~App
Secretary
~TCP

3-45
characteristics of unreliable channel will determine 
complexity of reliable data transfer protocol (rdt)
important in application, transport, link layers
top-10 list of important networking topics!
Principles of reliable data transfer

3-46
Reliable data transfer: getting started
senderreceiver
rdt_send():called from above, 
(e.g., by app.). Passed data to 
deliver to receiver upper layer
udt_send():called by rdt,
to transfer packet over 
unreliable channel to receiver
rdt_rcv():called when packet 
arrives on rcv-side of channel
deliver_data():called by 
rdtto deliver data to upper

3-47
We’ll:
incrementally develop sender, receiver sides of reliable data 
transfer protocol (rdt)
consider only unidirectional data transfer
but control info will flow on both directions!
use finite state machines (FSM)  to specify sender, receiver
state
1
state
2
event causing state transition
actions taken on state transition
state:when in this 
“state”next state 
uniquelydetermined 
by next event
event
actions
Reliable data transfer: getting started

3-48
rdt1.0: reliable transfer over a reliable channel
underlying channel (i.e., IP) perfectly reliable
no bit errors
no loss of packets
separate FSMs for sender, receiver:
sender sends data into underlying channel
receiver reads data from underlying channel
Wait for 
call from 
above
packet = make_pkt(data)
udt_send(packet)
rdt_send(data)
extract (packet,data)
deliver_data(data)
Wait for 
call from 
below
rdt_rcv(packet)
senderreceiver

3-49
underlying channel may flip bits in packet
checksum to detect bit errors
thequestion: how to recover from errors:
acknowledgements (ACKs):receiver explicitly tells 
sender that pkt received OK
negative acknowledgements (NAKs):receiver 
explicitly tells sender that pkt had errors
sender retransmits pkt on receipt of NAK
new mechanisms in rdt2.0(beyond rdt1.0):
error detection
receiver feedback: control msgs (ACK,NAK) rcvr-
>sender
rdt2.0: channel with bit errors
How do humans recover from “errors”
during conversation?

3-50
underlying channel may flip bits in packet
checksum to detect bit errors
thequestion: how to recover from errors:
acknowledgements (ACKs):receiver explicitly tells sender that pkt
received OK
negative acknowledgements (NAKs):receiver explicitly tells sender that 
pkt had errors
sender retransmitspkt on receipt of NAK
new mechanisms in rdt2.0(beyond rdt1.0):
error detection
feedback: control msgs (ACK,NAK) from receiver to sender
rdt2.0: channel with bit errors

3-51
rdt2.0: FSM specification
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
call from 
below
sender
receiver
Wait for 
call from 
above
sndpkt = make_pkt(data, checksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndpkt)
Wait for 
ACK or 
NAK
rdt_send(data)


3-52
rdt2.0: operation with no errors
Wait for 
call from 
above
snkpkt = make_pkt(data, 
checksum)
udt_send(sndpkt)
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndp
kt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
ACK or 
NAK
Wait for 
call from 
below
rdt_send(data)


3-53
rdt2.0: error scenario
Wait for 
call from 
above
snkpkt = make_pkt(data, 
checksum)
udt_send(sndpkt)
extract(rcvpkt,data)
deliver_data(data)
udt_send(ACK)
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt)
rdt_rcv(rcvpkt) && isACK(rcvpkt)
udt_send(sndp
kt)
rdt_rcv(rcvpkt) &&
isNAK(rcvpkt)
udt_send(NAK)
rdt_rcv(rcvpkt) && 
corrupt(rcvpkt)
Wait for 
ACK or 
NAK
Wait for 
call from 
below
rdt_send(data)

stop and wait
sender sends one packet, 
then waits for receiver response

3-54
rdt2.0 has a fatal flaw!
what happens if ACK/NAK corrupted?
sender doesn’t know what happened at 
receiver!
Can’t just retransmit after timeout: 
possible duplicate
handling duplicates: 
sender retransmits current pkt if 
ACK/NAK corrupted
sender adds sequence number
to each pkt
receiver discards (doesn’t deliver 
up) duplicate pkt
KEY QUESTION: 
duplicates

3-55
rdt2.1: sender, handles garbled ACK/NAKs
Wait for 
call 0from 
above
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
rdt_send(data)
Wait for 
ACK or 
NAK 0
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isNAK(rcvpkt) )
sndpkt = make_pkt(1, data, checksum)
udt_send(sndpkt)
rdt_send(data)
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt) 
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isNAK(rcvpkt) )
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt)
Wait for
call 1from 
above
Wait for 
ACK or 
NAK 1


No sequence 
number in ACK

3-56
Wait for 
0 from 
below
sndpkt = make_pkt(NAK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && 
not corrupt(rcvpkt) &&
has_seq0(rcvpkt)
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq1(rcvpkt)
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
Wait for 
1 from 
below
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq0(rcvpkt) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && (corrupt(rcvpkt)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
rdt_rcv(rcvpkt) && 
not corrupt(rcvpkt) &&
has_seq1(rcvpkt)
rdt_rcv(rcvpkt) && (corrupt(rcvpkt)
sndpkt = make_pkt(ACK, chksum)
udt_send(sndpkt)
sndpkt = make_pkt(NAK, chksum)
udt_send(sndpkt)
rdt2.1: receiver, handles garbled ACK/NAKs
Duplicate Pkt
Duplicate Pkt

3-57
rdt2.1: discussion
sender:
seq # added to pkt
two seq. #’s (0,1) will suffice.  
Why?
must check if received 
ACK/NAK corrupted 
State # is twice 
state must “remember” whether 
“expected” pkt should have seq # 
of 0 or 1 
receiver:
must check if received packet 
is duplicate
state indicates whether 0 or 1 is 
expected pkt seq #
note: receiver can notknow if 
its last ACK/NAKreceived OK 
at sender

3-58
rdt2.2: a NAK-free protocol
same functionality as rdt2.1, using ACKs 
only
instead of NAK, receiver sends ACK for last 
pkt receivedOK
receiver must explicitlyinclude seq # of pkt being 
ACKed
duplicate ACK at sender results in same 
action as NAK: retransmit current pkt

3-59
rdt2.2: sender, receiver fragments
Wait for 
call 0from 
above
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
rdt_send(data)
udt_send(sndpkt)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,1))
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,0)
Wait for 
ACK
0
sender FSM
fragment
rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) 
&& has_seq1(rcvpkt) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(ACK1, chksum)
udt_send(sndpkt)
Wait for 
0from 
below
rdt_rcv(rcvpkt) && 
(corrupt(rcvpkt) ||
has_seq1(rcvpkt))
udt_send(sndpkt)
receiver FSM
fragment

duplicate ACK for pkt 1
: Retransmit pkt 0
Duplicate pkt 1
: Retransmit ACK of pkt 1
For rdt2.1, no 
sequence 
number in ACK
Sequence 
number must be 
in the ACK 

3-60
rdt3.0: channels with errorsand loss
new assumption:underlying 
channel can also lose
packets (data, ACKs)
checksum, seq. #, ACKs, 
retransmissions will be of help 
... but not enough
approach:sender waits 
“reasonable” amount of timefor 
ACK 
retransmits if no ACK received in this 
time
if pkt (or ACK) just delayed (not lost):
retransmission will be  duplicate, 
but seq. # already handles this
receiver must specify seq # of pkt
being ACKed
requires countdown timer
2023.9.25

3-61
rdt3.0 sender
sndpkt = make_pkt(0, data, checksum)
udt_send(sndpkt)
start_timer
rdt_send(data)
Wait 
for 
ACK0
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,1) )
Wait for 
call 1 from 
above
sndpkt = make_pkt(1, data, checksum)
udt_send(sndpkt)
start_timer
rdt_send(data)
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,0)
rdt_rcv(rcvpkt) &&  
( corrupt(rcvpkt) ||
isACK(rcvpkt,0) )
rdt_rcv(rcvpkt)   
&& notcorrupt(rcvpkt) 
&& isACK(rcvpkt,1)
stop_timer
stop_timer
udt_send(sndpkt)
start_timer
timeout
udt_send(sndpkt)
start_timer
timeout
rdt_rcv(rcvpkt)
Wait for 
call 0from 
above
Wait 
for 
ACK1

rdt_rcv(rcvpkt)




3-62
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
pkt1
ack1
ack0
ack0
(a) no loss
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
ack1
ack0
ack0
(b) packet loss
pkt1
X
loss
pkt1
timeout
resend pkt1
rdt3.0 in action

3-63
rdt3.0 in action
rcv pkt1
send ack1
(detect duplicate)
pkt1
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
ack1
ack0
ack0
(c) ACK loss
ack1
X
loss
pkt1
timeout
resend pkt1
rcv pkt1
send ack1
(detect duplicate)
pkt1
sender
receiver
rcv pkt1
send ack0
rcv ack0
send pkt1
send pkt0
rcv pkt0
pkt0
ack0
(d) premature timeout/ delayed ACK
pkt1
timeout
resend pkt1
ack1
send ack1
send pkt0
rcv ack1
pkt0
ack1
ack0
send pkt0
rcv ack1
pkt0
rcv pkt0
send ack0
ack0
rcv pkt0
send ack0
(detect duplicate)
Would you please design other possible error scenarios?

3-64
Performance of rdt3.0
rdt3.0 is correct, but performance stinks
e.g.: 1 Gbps link, 15 ms prop. delay, 8000 bit packet:
U 
sender
: utilization– fraction of time sender busy 
sending
U 
sender 
= 
.
008 
30.008
= 0.00027  
L / R 
RTT + L / R 
= 
if RTT=30 msec, 1KB pkt every 30 msec: 33kB/sec 
thruput over 1 Gbps link
network protocol limits use of physical resources!
D
trans
=
L
R
8000 bits
10
9 
bits/sec
==
8 microsecs
sender
receiver
rcv pkt1
rcv pkt0
send ack0
send ack1
send ack0
rcv ack0
send pkt0
send pkt1
rcv ack1
send pkt0
rcv pkt0
pkt0
pkt0
pkt1
ack1
ack0
ack0

3-65
rdt3.0: stop-and-wait operation
sender
receive
r
RTT
last packet bit transmitted, 
t = L / R
first packet bit arrives
last packet bit arrives, send 
ACK
ACK arrives, send next 
packet, t = RTT + L / R
U
sender 
= 
.
008 
30.008 
= 0.00027  
L / R
RTT + L / R 
= 
first packet bit transmitted, t = 0

3-66
Pipelined protocols
pipelining:sender allows multiple, “in-flight”, 
yet-to-be-acknowledged pkts
range of sequence numbers must be increased
buffering at sender and/or receiver
two generic forms of pipelined protocols: 
go-Back-N, selective repeat

3-67
Pipelining: increased utilization
senderreceiver
RTT 
last bit transmitted, t = L / R
first packet bit arrives
last packet bit arrives, send ACK
ACK arrives, send next 
packet, t = RTT + L / R
last bit of 2
nd
packet arrives, send 
ACK
last bit of 3
rd
packet arrives, 
send ACK
3-packet pipelining increases
utilization by a factor of 3!
U 
sender 
= 
.
0024 
30.008 
= 0.00081  
3L / R
RTT + L / R 
= 
first packet bit transmitted, t = 0

3-68
Pipelined protocols: overview
Go-back-N:
sender can have up to N 
unacked packets in pipeline
receiver only sends cumulative 
ack
Doesn’t ack packet if there’s a gap
sender has timer for oldest 
unacked packet
when timer expires, retransmit all
unacked packets
Selective Repeat:
sender can have up to N 
unack’ed packets in pipeline
rcvr sends individual ackfor 
each packet
sender maintains timer for 
each unacked packet
when timer expires, retransmit 
onlythat unacked packet

3-69
Go-Back-N: sender
k-bit seq # in pkt header
“window” of up to N, consecutive unack’ed pkts allowed
ACK(n): ACKs all pkts up to, including seq # n -
“cumulative ACK”
may receive duplicate ACKs (see receiver)
timer for oldest in-flight pkt
timeout(n):retransmit packet n and all higher seq # pkts
in window
1 2 3 45 6 7 8 9 ABCDE F

3-70
Go-Back-N: sender
0 1 2 3 4 5 6 7 8
Sender:
N =8
receiver:
ACK 0
0 1 2 3 4 5 6 7 8
0 1 2 3 4 5 6 7 8
1 2 3 4 5 6 7 81 2 3 4 5 6 7 8

3-71
GBN: sender extended FSM
Wait
start_timer
udt_send(sndpkt[base])
udt_send(sndpkt[base+1])
...
udt_send(sndpkt[nextseqnum-1])
timeout
rdt_send(data)
if (nextseqnum < base+N) {
sndpkt[nextseqnum] = make_pkt(nextseqnum,data,chksum)
udt_send(sndpkt[nextseqnum])
if (base == nextseqnum)
start_timer
nextseqnum++
}
else
refuse_data(data)
base = getacknum(rcvpkt)+1
If (base == nextseqnum)
stop_timer
else
start_timer
rdt_rcv(rcvpkt) && 
notcorrupt(rcvpkt) 
base=1
nextseqnum=1
rdt_rcv(rcvpkt) 
&& corrupt(rcvpkt)

//窗口中第一个分组启动计时器
//为窗口中下一个已发分组启动计时器
//窗口底部位于还没发出的数据上,无需计时

3-72
ACK-only: always send ACK for correctly-
received pktwith highest in-orderseq #
may generate duplicate ACKs
need only remember expectedseqnum
out-of-order pkt: 
discard (don’t buffer): no receiver buffering!
re-ACK pkt with highest in-order seq #
Wait
udt_send(sndpkt)
default
rdt_rcv(rcvpkt)
&& notcurrupt(rcvpkt)
&& hasseqnum(rcvpkt,expectedseqnum) 
extract(rcvpkt,data)
deliver_data(data)
sndpkt = make_pkt(expectedseqnum,ACK,chksum)
udt_send(sndpkt)
expectedseqnum++
expectedseqnum=1
sndpkt =    
make_pkt(expectedseqnum,ACK,chksum)

GBN: receiver extended FSM
1 2 3 4 5 are received
ACK 5
1 2     4 5 are received
ACK 2; 4-5 are discarded

3-73
GBN in action
send  pkt0
send  pkt1
send  pkt2
send  pkt3
(wait)
sender
receiver
receive pkt0, send ack0
receive pkt1, send ack1
receive pkt3, discard, 
(re)send ack1
rcv ack0, send pkt4
rcv ack1, send pkt5
pkt 2 timeout
send  pkt2
send  pkt3
send  pkt4
send  pkt5
X
loss
receive pkt4, discard, 
(re)send ack1
receive pkt5, discard, 
(re)send ack1
rcv pkt2, deliver, send ack2
rcv pkt3, deliver, send ack3
rcv pkt4, deliver, send ack4
rcv pkt5, deliver, send ack5
ignore duplicate ACK
0 1 2 3 4 5 6 7 8 
sender window (N=4)
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 45 6 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 

3-74
Selective repeat
receiver individuallyacknowledges all correctly received pkts
buffers pkts, as needed, for eventual in-order delivery to upper layer
sender only resends pkts for which ACK not received
sender timer for each unACKed pkt
sender window
Nconsecutive seq #s
limits seq #s of sent, unACKed pkts

3-75
Selective repeat: sender, receiver windows
1 2 3 45 6 7 8 9 ABCDE F
1 2 3 45 6 7 8 9 ABCDE F

3-76
Selective repeat
data from above:
if next available seq # in 
window, send pkt
timeout(n):
resend pkt n, restart timer
ACK(n)in 
[sendbase,sendbase+N]:
mark pkt n as received
if n smallest unACKed pkt, 
advance window base to 
next unACKed seq # 
sender
pkt n in 
[rcvbase, rcvbase+N-1]
send ACK(n)
out-of-order: buffer
in-order: deliver (also 
deliver buffered, in-
order pkts), advance 
window to next not-yet-
received pkt
pkt n in [rcvbase-N,rcvbase-1]
ACK(n)
otherwise:
ignore 
receiver

3-77
Selective repeat in action
send  pkt0
send  pkt1
send  pkt2
send  pkt3
(wait)
sender
receiver
receive pkt0, send ack0
receive pkt1, send ack1
receive pkt3, buffer, 
send ack3
rcv ack0, send pkt4
rcv ack1, send pkt5
pkt 2 timeout
send  pkt2
X
loss
receive pkt4, buffer, 
send ack4
receive pkt5, buffer, 
send ack5
rcv pkt2; deliver pkt2,
pkt3, pkt4, pkt5; send ack2
record ack3 arrived
0 1 2 3 4 5 6 7 8 
sender window (N=4)
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 4 5 6 7 8 
0 1 2 3 45 6 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
0 12 3 4 56 7 8 
record ack4 arrived
record ack5 arrived
Q: what happens when ack2 arrives?

3-78
Selective repeat: dilemma
example: 
seq #’s: 0, 1, 2, 3
window size=3
receiver window
(after receipt)
sender window
(after receipt)
0 1 23 0 1 2
0 1 23 0 1 2
0 1 23 0 1 2
pkt0
pkt1
pkt2
0 1 23 0 1 2
pkt0
timeout
retransmit pkt0
01 2 30 1 2
0 12 3 01 2
0 1 2 3 0 12
X
X
X
will accept packet
with seq number 0
(b) oops!
0 1 23 0 1 2
0 1 23 0 1 2
0 1 23 0 1 2
pkt0
pkt1
pkt2
0 123 01 2
pkt0
01 2 30 1 2
0 12 3 01 2
0 1 2 3 0 12
X
will accept packet
with seq number 0
0 1 2 3 0 1 2
pkt3
(a) no problem
receiver can’t see sender side.
receiver behavior identical in both cases!
Something is (very) wrong!
receiver sees no difference in two 
scenarios!
duplicate data accepted as new in (b)
Q:what relationship between seq # size and 
window size to avoid problem in (b)?
Window size (W) up to N/2
Why？

3-79
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

3-80
TCP: Overview  RFCs: 793,1122, 2018, 5681, 7323
full duplex data:
bi-directional data flow in same 
connection
MSS: maximum segment size
connection-oriented:
handshaking (exchange of 
control msgs) inits sender, 
receiver state before data 
exchange
flow controlled:
sender will not overwhelm 
receiver
point-to-point:
one sender, one receiver
reliable, in-order byte steam:
no “message boundaries”
pipelined:
TCP congestion and flow control 
set window size
What is the purpose 
of the connection?

3-81
TCP segment structure
source port #
dest port #
32 bits
application
data 
(variable length)
sequence number
acknowledgement number
receive window
Urg data pointer
checksum
F
SR
PAU
head
len
not
used
options (variable length)
URG: urgent data 
(generally not used)
ACK: ACK #
valid
PSH: push data now
(generally not used)
RST, SYN, FIN:
connection estab
(setup, teardown
commands)
# bytes 
rcvr willing
to accept
counting
by bytes 
of data
(not segments!)
Internet
checksum
(as in UDP)

3-82
TCP seq. numbers, ACKs
sequence numbers:
byte stream “number” of first byte in 
segment’s data
acknowledgements:
seq # of NEXT byte expectedfrom other side
cumulative ACK
Q:how receiver handles out-of-order segments
A: TCP spec doesn’t say, - up to 
implementor
source port #
dest port #
sequence number
acknowledgement number
checksum
rwnd
urg pointer
incoming segment to sender
A
sent 
ACKed
sent, not-
yet ACKed
(“in-
flight”)
usable
but not 
yet sent
not 
usable
window size
N
sender sequence number space 
source port #
dest port #
sequence number
acknowledgement number
checksum
rwnd
urg pointer
outgoing segment from sender
Neither GBN nor selective repeat 

3-83
TCP seq. numbers, ACKs
User
types
‘C’
host ACKs
receipt 
of echoed
‘C’
host ACKs
receipt of
‘C’, echoes
back ‘C’
simple telnet scenario
Host B
Host A
Seq=42, ACK=79, data = ‘C’
Seq=79, ACK=43, data = ‘C’
Seq=43, ACK=80
Seq: 当前序号；
Ack: 待接收序号；

3-84
TCP round trip time, timeout
Q:how to set TCP timeout 
value?
longer than RTT
but RTT varies
too short:premature timeout, 
unnecessary retransmissions
too long:slow reaction to 
segment loss
Q:how to estimate RTT?
SampleRTT:measured time from 
segment transmission until ACK 
receipt
ignore retransmissions
SampleRTTwill vary, want estimated 
RTT “smoother”
average several recent
measurements, not just current 
SampleRTT
RTT (milliseconds)
RTT (milliseconds)

Transport Layer
3-85
RTT: gaia.cs.umass.edu to fantasia.eurecom.fr
100
150
200
250
300
350
1815222936435057647178859299106
time (seconnds)
RTT (milliseconds)
SampleRTTEstimated RTT
EstimatedRTT = (1-)*EstimatedRTT + *SampleRTT
exponential weighted moving average
influence of past sample decreases exponentially fast
typical value: =0.125
TCP round trip time, timeout
RTT (milliseconds)
RTT: gaia.cs.umass.edu to fantasia.eurecom.fr
sampleRTT
EstimatedRTT
time (seconds)

3-86
timeout interval:EstimatedRTTplus “safety margin”
large variation in EstimatedRTT ->larger safety margin
estimate SampleRTT deviation from EstimatedRTT: 
DevRTT = (1-)*DevRTT +
*|SampleRTT-EstimatedRTT|
TCP round trip time, timeout
(typically, = 0.25)
TimeoutInterval = EstimatedRTT + 4*DevRTT
estimated RTT
“safety margin”

3-87
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

3-88
TCP reliable data transfer
TCP creates rdt service on top of 
IP’s unreliable service
pipelined segments
cumulative acks
single retransmission timer
retransmissions  triggered by:
timeout events
duplicate acks
Let’s initially consider 
simplified TCP sender:
ignore duplicate acks
ignore flow control, congestion 
control
retransmission

3-89
TCP sender events:
data rcvd from app:
create segment with seq #
seq # is byte-stream number
of first data byte in  segment
start timer if not already 
running 
think of timer as for oldest 
unacked segment
expiration interval: 
TimeOutInterval
timeout:
retransmit segment that caused 
timeout
restart timer
ack rcvd:
if ack acknowledges previously 
unacked segments
update what is known to be ACKed
start timer if there are  still 
unacked segments

3-90
TCP sender (simplified)
wait
for 
event
NextSeqNum = InitialSeqNum
SendBase = InitialSeqNum

create segment, seq. #: NextSeqNum
pass segment to IP (i.e., “send”)
NextSeqNum = NextSeqNum + length(data) 
if (timer currently not running)
start timer
data received from application above
retransmit not-yet-acked segment         
with smallest seq. #
start timer
timeout
if (y > SendBase) { 
SendBase = y 
/* SendBase–1: last cumulatively ACKed byte */
if (there are currently not-yet-acked segments)
start timer
else stop timer 
} 
ACK received, with ACK field value y 
timeout
timeout

3-91
TCP: retransmission scenarios
lost ACK scenario
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=92, 8 bytes of data
X
timeout
ACK=100
premature timeout
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=92,  8
bytes of data
timeout
ACK=120
Seq=100, 20 bytes of data
ACK=120
SendBase=100
SendBase=120
SendBase=120
SendBase=92

3-92
TCP: retransmission scenarios
X
cumulative ACK
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
Seq=120,  15 bytes of data
timeout
Seq=100, 20 bytes of data
ACK=120

timeout
3-93
TCP ACK generation [RFC 1122, RFC 2581]
event at receiver
arrival of in-order segment with
expected seq #. All data up to
expected seq # already ACKed
arrival of in-order segment with
expected seq #. One other 
segment has ACK pending
arrival of out-of-order segment
higher-than-expect seq. # .
Gap detected
arrival of segment that 
partially or completely fills gap
TCP receiver action
delayed ACK. Wait up to 500ms
for next segment. If no next segment,
send ACK
immediately send single cumulative 
ACK, ACKing both in-order segments 
immediately sendduplicate ACK,
indicating seq. # of next expected byte
immediate sendACK, provided that
segment starts at lower end of gap

3-94
TCP fastretransmit
time-out period  often relatively 
long:
long delay before resending lost 
packet
detect lost segments via 
duplicate ACKs.
sender often sends many 
segments back-to-back
if segment is lost, there will likely 
be many duplicate ACKs.
if sender receives 3ACKs for 
same data 
(“triple duplicate 
ACKs”),
resend unacked
segment with smallest seq #
likely that unacked segment lost, 
so don’t wait for timeout
TCP fast retransmit
Time out
1
2
3
4
1
1
1
1
ACK(?)
timeout

3-95
X
fast retransmit after sender 
receipt of triple duplicate ACK
Host B
Host A
Seq=92, 8 bytes of data
ACK=100
timeout
ACK=100
ACK=100
ACK=100
TCP fast retransmit
Seq=100, 20 bytes of data
Seq=100, 20 bytes of data

3-96
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented transport: 
TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

3-97
TCP flow control
application
process
TCP socket
receiver buffers
TCP
code
IP
code
application
OS
receiver protocol stack
application may 
remove data from 
TCP socket buffers .... 
... slower than 
TCP 
receiver is 
delivering
(sender is 
sending)
from sender
receiver controls sender, so 
sender won’t overflow 
receiver’s buffer by 
transmitting too much, too fast
flow control

3-98
TCP flow control
buffered data
free buffer space
rwnd
RcvBuffer
TCP segment payloads
to application process
receiver “advertises”free buffer space 
by including rwndvalue in TCP header 
of receiver-to-sender segments
RcvBuffersize set via socket options 
(typical default is 4096 bytes)
many operating systems autoadjust
RcvBuffer
sender limits amount of unacked (“in-
flight”) data to receiver’s rwndvalue 
guarantees receive buffer will not 
overflow
receiver-side buffering

3-99
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

3-100
Connection Management
before exchanging data, sender/receiver “handshake”:
agree to establish connection (each knowing the other willing to establish 
connection)
agree on connection parameters
connection state: ESTAB
connection variables:
seq # client-to-server
server-to-client
rcvBuffersize
at server,client 
application
network
connection state: ESTAB
connection Variables:
seq # client-to-server
server-to-client
rcvBuffersize
at server,client 
application
network
Socket clientSocket =   
newSocket("hostname","port 
number");
Socket connectionSocket = 
welcomeSocket.accept();

3-101
Q:will 2-way handshake always 
work in network?
variable delays
retransmitted messages (e.g. 
req_conn(x)) due to message loss
message reordering
Can’t “see”other side
2-way handshake:
Let’s talk
OK
ESTAB
ESTAB
choose x
req_conn(x)
ESTAB
ESTAB
acc_conn(x)
Agreeing to establish a connection
2023.9.27

3-103
Agreeing to establish a connection
2-way handshake failure scenarios:
retransmit
req_conn(
x)
ESTAB
req_conn(x)
half open connection!
(no client!)
client 
terminates
server
forgets x
connection 
x completes
retransmit
req_conn(
x)
ESTAB
req_conn(x)
data(x+1)
retransmit
data(x+1)
accept
data(x+1)
choose x
req_conn(x)
ESTAB
ESTAB
acc_conn(x)
client 
terminates
ESTAB
choose x
req_conn(x)
ESTAB
acc_conn(x)
data(x+1)
accept
data(x+1)
connection 
x completes
server
forgets x

3-104
TCP 3-way handshake
choose init seq num, x
send TCP SYN msg
SYNbit=1, Seq=x
ESTAB
choose init seq num, y
send TCP SYNACK
msg, acking SYN
SYNbit=1, Seq=y
ACKbit=1; ACKnum=x+1
received SYNACK(x) 
indicates server is live;
send ACK for SYNACK;
this segment may contain 
client-to-server data
received ACK(y) 
indicates client is live
ACKbit=1, ACKnum=y+1
SYNSENT
ESTAB
SYN RCVD
client state
LISTEN
server state
LISTEN

3-105
TCP 3-way handshake: FSM
closed

listen
SYN
rcvd
SYN
sent
ESTAB
Socket clientSocket =   
newSocket("hostname","port 
number");
SYN(seq=x)
Socket connectionSocket = 
welcomeSocket.accept();
SYN(x)
SYNACK(seq=y,ACKnum=x+1)
create new socket for 
communication back to client
SYNACK(seq=y,ACKnum=x+1)
ACK(ACKnum=y+1)
ACK(ACKnum=y+1)

Client-sideserver-side

3-106
TCP: closing a connection
client, server each close their side of 
connection
send TCP segment with FIN bit = 1
respond to received FIN with ACK
on receiving FIN, ACK can be combined with 
own FIN
simultaneous FIN exchanges can be 
handled

3-107
FIN_WAIT_2
CLOSE_WAIT
FINbit=1, seq=y
ACKbit=1; ACKnum=y+1
ACKbit=1; ACKnum=x+1
wait for server
close
can still
send data
can no longer
send data
LAST_ACK
CLOSED
TIMED_WAIT
timed wait 
for 2*max 
segment lifetime
CLOSED
TCP: closing a connection
FIN_WAIT_1
FINbit=1, seq=x
can no longer
send but can
receive data
clientSocket.close()
client state
server state
ESTAB
ESTAB
The key idea of TCP: ACK is necessary for each sent segment.

3-108
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

拥塞控制所起的作用
提供的负载
吞吐量
理想的拥塞控制
实际的拥塞控制
0
死锁（吞吐量= 0）
无拥塞控制
拥塞
轻度
拥塞

3-110
congestion:
informally: “too many sources sending too much data too fast 
for 
networkto handle”
different from flow control!
manifestations:
lost packets (buffer overflow at routers)
long delays (queueing in routerbuffers)
a top-10 problem!
Principles of congestion control

3-111
Causes/costs of congestion: scenario 1 
two senders, two receivers
one router, infinite buffers 
output link capacity: R
no retransmission
maximum per-connection 
throughput: R/2
unlimitedshared 
output link buffers
Host A
original data: 
in
Host B
throughput:

out
R/2
R/2

out

in
R/2
delay

in
large delays as arrival rate, 

in
, approaches capacity
Due to infinite 
buffers


out
delay
3-112
one router, finitebuffers 
sender retransmissionof timed-outpacket
application-layer input = application-layer output:
in
= 
out
transport-layer input includes retransmissions:′
in

in
finite shared 
output link buffers
Host A

in
: original data
Host B

out
'
in
: original data, plus
retransmitted data
‘
Causes/costs of congestion: scenario 2 
无策略的重传导致网络中数据继续增加， 拥塞更加严重！

(Sender should retransmits timed-out pkts at the right time!!!)
3-113
idealization: perfect knowledge
sender sends only when router buffers available 
finite shared output 
link buffers

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
copy
free buffer space!
R/2
R/2

out

in
Causes/costs of congestion: scenario 2 
Host B
A
Considering sender’s retransmission
选择合适的时
机进行重传


out
3-114

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
copy
no buffer space!
Idealization: known pkts are indeed lost
packets can be lost, dropped at router due  to full buffers
sender only resends if packet knownto be lost instead of timed-out.
Causes/costs of congestion: scenario 2 
A
Host B
Considering sender’s retransmission
Time-out ് loss

3-115

in
: original data

out
'
in
: original data, 
plusretransmitted 
data
free buffer space!
Causes/costs of congestion: scenario 2 
Idealization: known losspackets 
can be lost, dropped at router due  to 
full buffers
sender only resends if packet known
to be lost
R/2
R/2

in

out
when sending at R/2, 
some packets are 
retransmissions but 
asymptotic goodput
is still R/2 (why?)
A
Host B
选择丢失分组
进行精准重传

out


out
3-116
A

in

out
'
in
copy
free buffer space!
timeout
R/2
R/2

in

out
when sending at R/2, 
some packets are 
retransmissions 
including duplicated 
that are delivered!
Host B
Realistic: duplicates
packets can be lost, dropped at router due  to 
full buffers
sender times out prematurely, sending two 
copies, both of which are delivered
Causes/costs of congestion: scenario 2 

3-117
R/2

out
when sending at R/2, 
some packets are 
retransmissions 
including duplicated 
that are delivered!
“costs” of congestion:
more work (retrans) for given “goodput”
unneeded retransmissions: link carries multiple copies of pkt
decreasing goodput
R/2

in
Causes/costs of congestion: scenario 2 
Realistic: duplicates
packets can be lost, dropped at router 
due  to full buffers
sender times out prematurely, sending 
two copies, both of which are delivered


out
3-118
four senders
multihop paths
timeout/retransmit
Q:what happens as 
in
and 
in
’
increase?
finite shared output 
link buffers
Host A

out
Causes/costs of congestion: scenario 3 
Host B
Host C
Host D

in
: original data
'
in
: original data, plus
retransmitted data
A:as red  
in
’
increases, 
all arriving bluepkts at 
upper queue are 
dropped, blue throughput 
0

3-119
another “cost”of congestion:
when packet dropped, any upstream transmission capacity used 
for that packet was wasted!
Cause upstream congestion!
Causes/costs of congestion: scenario 3 
C/2
C/2

out

in
’


out
3-120
Approaches towards congestion control
two broad approaches towards congestion control:
end-end congestion 
control:
no explicit feedback 
from network
congestion inferred 
from end-system 
observed loss, delay
approach taken by TCP
network-assisted 
congestion control:
routers provide feedback to 
end systems
single bit indicating 
congestion (SNA, DECbit, 
TCP/IP ECN, ATM)
explicit rate for sender to 
send at

3-121
Case study: ATM ABR congestion control
ABR: available bit rate:
“elastic service”
if sender’s path “underloaded”: 
sender should use available 
bandwidth
if sender’s path congested: 
sender throttled to minimum 
guaranteed rate
RM (resource management) cells:
sent by sender, interspersed with 
data cells
bits in RM cell set by switches 
(“network-assisted
”) 
NI bit:no increase in rate (mild 
congestion)
CI bit:congestion indication
RM cells returned to sender by 
receiver, with bits intact

3-122
Case study: ATM ABR congestio control
two-byte ER (explicit rate) field in RM cell
congested switch may lower ER value in cell
Senders’ send rate thus max supportable rate on path
EFCI bit in data cells: set to 1 in congested switch
if data cell preceding RM cell has EFCI set, receiversets CI bit in 
returned RM cell
RM celldata cell
专用控制
分组方式,
占用带宽

Transport Layer
3-123
network-assisted congestion control:
two bits in IP header (ToS field) marked by network routerto indicate 
congestion
congestion indication carried to receiving host
receiver (seeing congestion indication in IP datagram) ) sets ECE bit on 
receiver-to-sender ACK segmentto notify sender of congestion
Explicit Congestion Notification (ECN)
source
application
transport
network
link
physical
destination
application
transport
network
link
physical
ECN=00
ECN=11
ECE=1
IP datagram
TCP ACK segment
捎带方式,
节省带宽

3-124
Chapter 3 outline
3.1 transport-layer services
3.2 multiplexing and 
demultiplexing
3.3 connectionless transport: 
UDP
3.4 principles of reliable data 
transfer
3.5 connection-oriented 
transport: TCP
segment structure
reliable data transfer
flow control
connection management
3.6 principles of congestion 
control
3.7 TCP congestion control

3-125
TCP congestion control: additive increase 
multiplicative decrease
approach:sender increases transmission rate (window size), probing 
for usable bandwidth, until loss occurs
additive increase:increase  cwndby 1 MSS every RTT until loss 
detected
multiplicative decrease:cut cwndin halfafter loss 
cwnd:
TCP sender 
congestion window size
AIMD saw tooth
behavior: probing
for bandwidth
additively increase window size ...
.... until loss occurs (then cut window in half)
time

cwnd:
TCP sender 
congestion window size
3-126
TCP Congestion Control: details
sender limits transmission:
cwndis dynamic, function of perceived network congestion
TCP sending rate:
roughly:send cwnd bytes, wait 
RTT for ACKS, then send more 
bytes
last byte
ACKed
sent, not-
yet ACKed
(“in-
flight”)
last byte 
sent
cwnd
sender sequence number space 
rate
~
~
cwnd
RTT
bytes/sec
减小窗口→降低速率→减少网络数据→缓解拥塞

3-127
TCP Slow Start 
when connection begins, increase 
rate exponentially until first loss 
event:
initially cwnd= 1 MSS
doublecwndevery RTT
done by incrementing cwndfor every 
ACK received
summary:initial rate is slowbut 
ramps up exponentially fast
Host A
RTT
Host B
time

RTT
3-128
TCP: detecting, reacting to loss
loss indicated by timeout:
cwndset to 1 MSS; 
window then grows exponentially(as in slow start) to 
threshold, then grows linearly
loss indicated by 3 duplicate ACKs: TCP RENO
dup ACKs indicate network capable of  delivering some 
segments 
cwndis cut in half window then grows linearly
TCP Tahoe always sets cwndto 1 (timeout or 3 duplicate 
acks)

3-129
Q:when should the exponential 
increase switch to linear? 
A:when cwndgets to 1/2 of its 
value before timeout.
Implementation:
variable ssthresh
on loss event, ssthreshis set to 1/2 of cwndjust 
before loss event
TCP: switching from slow start to CA
Cwnd >= ssthresh
Ssthresh=1/2 Cwnd

3-130
Summary: TCP Congestion Control
timeout
ssthresh = cwnd/2
cwnd = 1 MSS
dupACKcount = 0
retransmit missing segment

cwnd > ssthresh
congestion
avoidance 
cwnd = cwnd + MSS    (MSS/cwnd)
dupACKcount = 0
transmit new segment(s), as allowed
new ACK
.
dupACKcount++
duplicate ACK
fast
recovery 
cwnd = cwnd + MSS
transmit new segment(s), as allowed
duplicate ACK
ssthresh= cwnd/2
cwnd = ssthresh + 3
retransmit missing segment
dupACKcount == 3
timeout
ssthresh = cwnd/2
cwnd = 1 
dupACKcount = 0
retransmit missing segment
ssthresh= cwnd/2
cwnd = ssthresh + 3
retransmit missing segment
dupACKcount == 3
cwnd = ssthresh
dupACKcount = 0
New ACK
slow 
start
timeout
ssthresh = cwnd/2 
cwnd = 1 MSS
dupACKcount = 0
retransmit missing segment
cwnd = cwnd+MSS
dupACKcount = 0
transmit new segment(s), as allowed
new ACK
dupACKcount++
duplicate ACK

cwnd = 1 MSS
ssthresh = 64 KB
dupACKcount = 0
New
ACK!
New
ACK!
New
ACK!
Increase one MSS 
when  ALL acks of a 
cwndare received
Very Important!!!
Increase one MSS 
for each ack

3-131
TCP throughput
avg. TCP thruput as function of window size, RTT?
ignore slow start, assume always data to send
W: window size (measured in bytes)where loss occurs
avg. window size (# in-flight bytes) is ¾ W
avg. thruput is 3/4W per RTT
W
W/2
avg TCP thruput = 
3
4
W
RTT
bytes/sec
2023.10.9

TCP BIC
TCP RENO 的问题： 窗口恢复慢
132
Congestion
avoidance
Slow
start
Congestion
avoidance
在reno版本的拥塞控制下，进入拥塞避免状态或快速恢复状态后，每经过一个RTT才会将窗口
大小加1，如果链路状况是好的，但RTT很长，reno需要很长时间才能达到最佳拥塞窗口

TCP BIC
133
•丢包时的拥塞窗口大小为Wmax
•减小后的窗口为Wmin
• Wmin<W<Wmax
• reno采用加法去搜索W的方式实在是太慢了，因此BIC采用了二
分搜索的方式去找到这个W

TCP CUBIC
TCP BIC存在的问题：BIC不能用到RTT较小，丢包较高的环境中
134

TCP CUBIC
思路：把窗口调整与RTT脱钩，不再以RTT为单位调整窗口
135
3
max
()  (   )Wt   Ct  KW
t是从窗口上次降低开始到现在的时间,K是从窗口降低到再次到达Wmax的时间. 
t=0,W(0)=βWmax

TCP and the congested “bottleneck link”
TCP (classic, CUBIC) increase TCP’s sending rate until packet loss occurs 
at some router’s output: the bottleneck link
source
application
TCP
network
link
physical
destination
application
TCP
network
link
physical
bottlenecklink (almost always busy)
packet queue almost 
never empty, sometimes 
overflows packet (loss)
Transport Layer: 3-138

TCP and the congested “bottleneck link”
TCP (classic, CUBIC) increase TCP’s sending rate until packet loss occurs 
at some router’s output: the bottleneck link
source
application
TCP
network
link
physical
destination
application
TCP
network
link
physical
understanding congestion: useful to focus on congested bottleneck link
insight: increasing TCP sending rate will 
NOTincrease end‐end throughout 
with congested bottleneck
insight: increasing TCP 
sending rate will
increase measured RTT
RTT
Goal: “keep the end‐end pipe just full, but not fuller”
Transport Layer: 3-139

Delay-based TCP congestion control
Keeping sender‐to‐receiver pipe “just full enough, but no fuller”: keep 
bottleneck link busy transmitting, but avoid high delays/buffering
RTT
measured
Delay‐based approach:
RTT
min
‐minimum observed RTT(uncongested path)
uncongested throughput with congestion window cwndis cwnd/RTT
min
if measured throughput “very close” to  uncongested throughput
increase cwndlinearly                /* since path not congested */ 
else if measured throughput “far below” uncongested throughout
decrease cwndlinearly /* since path is congested */
RTT
measured
measured 
throughput
=
# bytes sent in 
last RTT interval
Transport Layer: 3-140

Delay-based TCP congestion control
congestion control without inducing/forcing loss
maximizing throughout (“keeping the just pipe full... ”) while keeping 
delay low (“...but not fuller”)
a number of deployed TCPs take a delay‐based approach
BBRdeployed on Google’s (internal) backbone network
Transport Layer: 3-141

3-142
TCP Futures: TCP over “long, fat pipes”
example: 1500 byte segments, 100ms RTT, want 10 Gbps
throughput
requires W = 83,333 in-flight segments
throughput in terms of segment loss probability, L [Mathis 1997]:
➜to achieve 10 Gbps throughput, need a loss rate of L = 2·10
-10  
– a very 
small loss rate!
new versions of TCP for high-speed
TCP throughput = 
1.22
.
MSS
RTT
L

3-143
fairness goal:if K TCP sessions share same bottleneck link of 
bandwidth R, each should have average rate of R/K
TCP connection 1
bottleneck
router
capacity R
TCP Fairness
TCP connection 2

3-144
Why is TCP fair?
two competing sessions:
additive increase gives slope of 1, as throughout increases
multiplicative decrease decreases throughput proportionally 
R
R
equal bandwidth share
Connection 1 throughput
congestion avoidance: additive increase
loss: decrease window by factor of 2
congestion avoidance: additive increase
loss: decrease window by factor of 2

3-145
Fairness (more)
Fairness and UDP
multimedia apps often do not 
use TCP
do not want rate throttled by 
congestion control
instead use UDP:
send audio/video at constant rate, 
tolerate packet loss
Fairness, parallel TCP 
connections
application can open multiple 
parallel connections between 
two hosts
web browsers do this 
e.g., link of rate R with 9 
existing connections:
new app asks for 1 TCP, gets rate R/10
new app asks for 11 TCPs, gets R/2 

146
Basic Control Model
Let’s assume window-basedoperation
Reduce windowwhen congestion is perceived
How is congestion signaled?
Either markordroppackets
When is a router congested?
Drop tail queues – when queue is full
Average queue length – at some threshold
Increase windowotherwise
Probe for available bandwidth – how?
Supplementary Content

147
Simple linear control
Many different possibilities for reaction to 
congestion and methods for probing
Examine simple linearcontrols
Window(t + 1) = a + b Window(t)
Different a
i
/b
i
for increase and a
d
/b
d
for decrease

148
Simple linear control
Multiplicative increase, additive decrease
a
I
=0, b
I
>1;     a
D
<0, b
D
=1
Additive increase, additive decrease
a
I
>0, b
I
=1;     a
D
<0, b
D
=1
Multiplicative increase, multiplicative decrease
a
I
=0, b
I
>1;    a
D
=0, 0<b
D
<1
Additive increase, multiplicative decrease
a
I
>0, b
I
=1;    a
D
=0, 0<b
D
<1
Which one?









decreasetxba
increasetxba
tx
iDD
iII
i
)(
)(
)1(

149
()
(1)
()
Ii
i
Di
b x  t   increase
xt
ax  t  decrease





Multiplicative Increase, 
Additive Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(x
1h
+a
D
,x
2h
+a
D
)
(b
I
(x
1h
+a
D
), b
I
(x
2h
+a
D
))
Does not 
converge to 
fairness
Not stable at all
Does not 
converges to 
efficiency
a
I
=0, b
I
>1, a
D
<0, b
D
=1
User 2: x
2

User 2: x
2
150
Additive Increase, 
Additive Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(x
1h
+a
D
,x
2h
+a
D
)
(x
1h
+a
D
+a
I
),
x
2h
+a
D
+a
I
))
Does not 
converge to 
fairness
Does not 
converge to 
efficiency
()
(1)
()
Ii
i
Di
ax  t   increase
xt
ax  t  decrease






a
I
>0, b
I
=1, a
D
<0, b
D
=1

151
Multiplicative Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(b
d
x
1h
,b
d
x
2h
)
(b
I
b
D
x
1h
,
b
I
b
D
x
2h
)
Does not 
converge to 
fairness
Converges to 
efficiency iff
10
1


D
I
b
b
()
(1)
()
Ii
i
Di
b x  t   increase
xt
bx tdecrease




a
I
=0, b
I
>1, a
D
=0, 0<b
D
<1
User 2: x
2

User 2: x
2
152
Multiplicative Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(x
1h
,x
2h
)
(b
d
x
1h
,b
d
x
2h
)
(b
I
b
D
x
1h
,
b
I
b
D
x
2h
)
Does not converge to fairness
Converges to efficiency iff
10
1


D
I
b
b
()
(1)
()
Ii
i
Di
b x  t   increase
xt
bx tdecrease




a
I
=0, b
I
>1, a
D
=0, 0<b
D
<1

153
(b
D
x
1h
+a
I
,
b
D
x
2h
+a
I
)
Additive Increase, 
Multiplicative Decrease
User 1: x
1
User 2: x
2
fairness
line
efficiency
line
(b
D
x
1h
,b
D
x
2h
)
Converges to 
fairness
Converges to 
efficiency
Increments 
smaller as 
fairness 
increases
(x
1h
,x
2h
)
()
(1)
()
IIi
i
Di
ab x  t   increase
xt
bx tdecrease





a
I
>0, b
I
=1, a
D
=0, 0<b
D
<1

User 2: x
2
随机早期检测RED (Random Early Detection) 
使路由器的队列维持两个参数，即队列长度最小门限TH
min
和最大门限TH
max
。
RED 对每一个到达的数据报都先计算平均队列长度L
AV
。
若平均队列长度小于最小门限TH
min
，则将新到达的数据报放入队列进行排队。
若平均队列长度超过最大门限TH
max
，则将新到达的数据报丢弃。
若平均队列长度在最小门限TH
min
和最大门限TH
max
之间，则按照某一概率p将新
到达的数据报丢弃。
从队首
发送
最小门限TH
min
最大门限TH
min
分组
到达
平均队列长度L
av
排队丢弃
以概率p丢弃
RED 将路
由器的到
达队列划
分成为三
个区域
156

丢弃概率p 与TH
min
和Th
max
的关系
最小门限TH
min
最大门限TH
max
平均队列长度L
av
分组丢弃概率p
1.0
0
p
max
当L
AV
Th
min 
时，丢弃概率p= 0。
当L
AV
Th
max 
时，丢弃概率p= 1。
当TH
min
L
AV
TH
max
时，0 p 1 。
例如，按线性规律变化，从0 变到p
max
。
157

瞬时队列长度和
平均队列长度的区别
队列长度
时间
瞬时队列长度
平均队列长度
158

TCP, UDP: principal transport protocols for 40 years
different “flavors” of TCP developed, for specific scenarios:
Evolving transport-layer functionality
moving transport–layer functions to application layer, on top of UDP
•HTTP/3: QUIC
ScenarioChallenges
Long, fat pipes (large data transfers)   Many packets “in flight”; loss shuts down pipeline
Wireless networksLoss due to noisy wireless links, mobility; TCP treat 
this as congestion loss
Long‐delay linksExtremely long RTTs
Data center networksLatency sensitive
Background traffic flowsLow priority, “background” TCP flows 
Transport Layer: 3-159

application-layer protocol, on top of UDP
increase performance of HTTP
deployed on many Google servers, apps (Chrome, mobile YouTube app) 
QUIC: Quick UDP Internet Connections
IP
TCP
TLS
HTTP/2
IP
UDP
QUIC
HTTP/2 (slimmed)
Network
Transport
Application
HTTP/2 over TCP
HTTP/3
HTTP/2 over QUIC over UDP
Transport Layer: 3-160

QUIC: Quick UDP Internet Connections
adopts approaches we’ve studied in this chapter for 
connection establishment, error control, congestion control
multiple application-level “streams” multiplexed over 
single QUIC connection
separate reliable data transfer, security
common congestion control
•error and congestion control: “Readers familiar with TCP’s loss 
detection and congestion control will find algorithms here that parallel 
well‐known TCP ones.” 
[from QUIC specification]
•connection establishment: reliability, congestion control, 
authentication, encryption, state established in one RTT
Transport Layer: 3-161

QUIC: Connection establishment
TCP handshake
(transport layer)
TLS handshake
(security)
TCP (reliability, congestion control 
state) 
+ TLS (authentication, crypto 
state)
2 serial handshakes
data
QUIC handshake
data
QUIC:  reliability, congestion control, 
authentication, crypto state
1 handshake
Transport Layer: 3-162

QUIC: streams: parallelism, no HOL blocking
(a) HTTP 1.1
TLS encryption
TCP RDT
TCP Cong. Contr.
transport
application
(b) HTTP/2 with QUIC: no HOL blocking
TCP RDT
TCP Cong. Contr.
TLS encryption
error!
HTTP 
GET 
HTTP 
GET 
HTTP 
GET 
QUIC Cong. Cont.
QUIC 
encrypt
QUIC
RDT
QUIC
RDT
QUIC
RDT
QUIC 
encrypt
QUIC 
encrypt
UDP
UDP
QUIC Cong. Cont.
QUIC 
encrypt
QUIC
RDT
QUIC
RDT
QUIC
RDT
QUIC 
encrypt
QUIC 
encrypt
error!
HTTP 
GET 
HTTP 
GET 
HTTP 
GET 
Transport Layer: 3-163

transport
application
Email: xieyi5@mail.sysu.edu.cn
https://cse.sysu.edu.cn/content/2462
Thanks
Q & A
