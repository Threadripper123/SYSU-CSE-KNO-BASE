# Chapter3 Transport Layer-图集-矢量

> 主题库：计算机网络　｜　源文件：Chapter3 Transport Layer-图集-矢量.pdf　｜　8,701 字符　｜　78 个片段　｜　更新于 2026-11-13
application network data link physical

应用进程应用进程 AP AP， AP O 端口端口 AP： 运输层提供应用进程间的逻辑通信 3 1P层

ca器 a器 AP P LAN, WAN AN, AP 2 AP Pb协l的作范运输层协议TCP和UDP 的作用范围

Manager ~APP Secretary ~TCP Staff- IP 3-8

Sender 1 ispassedanapplication- app. msg i  layermessage   determines segment app.msg n OO  Th 1 heade ields alues   creates segment mework passes segmenttoiP c

Receiver: receives segment from IP  checks header values   1   pp. msg extractsapplication-layer      message metiork (IP) demultiplexes message up      toapplicationviasocket  hys         app.msg

• reliable, in-order delivery aannectionsetup unreliable, unordered network  data link delivery: network aaan cYanG a no-frills extension of  data link apt. cation data link network services not available: delayguarantees bandwidth guarantees

client APACHE transport netork link

application transport  network link

application transport  network link

S application P-client1 P-client₂ application taransport netwbrk transpol t network hl networl link physical link physical physica Transport La

htaandle data frommultiple demultiplexing y I atreceiver: header (later used for use header info to deliver demultiplexing) received  segm ents to correct socket application P1 P2 application socket P3  +  P4 trans e process   transpot netwofk  t  transpr link physicl link physica physic 泪

App1 App2 App3 App1 App2 App3 NETFLlx socket1]1 socket2 socket3 socket1 socket2 socket3 Imultiplexingl lem ultiplex g TCPsegments UDP TCPsegments UDP multiplexing demultiplexing

source port # dest port # ather header fields Yata Yaaylead

IP Hdr IP payload

DatagramSocket mysocket2 DatagramSocket DatagramSocket = new mysocketl = new Datagramsocket (6428); 发送端在本地 DatagramSocket (9157）； 的镜像。 (5775）； 接收端程序 application ： 混合了来自在本地的一 application P1 application 个镜像 P3 P3,P4的数据， P4 transpi jo transp C Ol jrt transp netw or ork or netwo  1 link networ link physi idal link physic: al physicc

发送端在本接收端程序 application 地的镜像。 在本地的一 application P4 P5 P6 独立对应每 application 个镜像 P3 一个通信对 P2 P3 transpcr 5rt 端应用程序 transp( rt netv, //ork 可以区分 tl ansport networ link 来源 n etwor link phyi /sical li i hk physical SerVer:IP hysical addressB host: IP host: IP addressA address source IP,portt B,80 dest IP,port: A,9i57 s0urce IP,port: C,5775 dest IP,port: B,80 source IP,port: A,9157 dest IP, port:B,80 source IP,port: C,9157

tnreaded sever application application application P4 P3 P2 P3 transpdi rt transp Trt netv/ /ork tra nsport networl -k link ne:1 twork link phy /sical lir k physice 0 server:IP p Tysical address B host: IP ast: Ip address ddress A source IP,port: B,80 dest IP,port: A,9157 source IP,port: C,5775 dest IP,port: B,80 source IP,port: C,9157

SN iPciet application application taransport nransport (UDP) (UDP) network (IP) network (IP) link link physical physical c

UDPsenderactions:   ispassedanapplication- SNMPmsg  DO o    ayermessage     hteaderfieldsvalues etor creates UDP segment

32 bits source port # dest port # length checksum aaplication DAta (payload)

内了计算检验和 [充没有 [实质作用 UDP部节 4 4 1 2 及净荷源IP地址目的IP地址 0 17 UDP长度

UDP 户数据报部数据送在前首部数据 IP 据报

reliable channel Sending IPs Receiving IPs IP layer unreliable channel IP

Manager ~APP Secretary ~TCP

rdt send : called from above, called by (e.g., by app.). Passed data to rdt to deliver data to upper deliver to receiver upper laver rdt_send()↓data data deliver_data() reliable data reliable data sender transfer protocol transfer protocol receiver sending side) (receiving side) udtsnd() packet packet rdt rcv() canreliablechannel called byrdt calledwhenpacket to transfer packet over arrives on rcv-side of channel unreliable channel to receiver

actions taken on state transition catate state 1 _event_ 2 actions

rdtsend(data) snkpkt = make_pkt(data, checksum) 1+ Send(sndpl rdtrcv(rcvpkt)&& Wait for Wait foX isNAK(rcvpkt) )  ACK or ))u rdt_rcv(rcvpkt call from UCi dt_send(sndp corrupt(rcvp above NAK udt send(NAH rdt rcv(rcvpkt) && isACK(rcvpkt) Wait for Λ  call fro m balow   rdtrcv(rcvpkt)&  extract(rcvpkt,da    deliver data(dat udt send(ACK)

rdt_send(data) snkpkt = make_pkt(data, send(sndpk rcv(rcvpkt)&& Nait for Wait for isNAK(rc +) rdt rcv(rcvpkt) call from ACK or udt_send(sndp corrupt(rcvpkt boVe NAK kt) 1Idtsend(NAK rdtrcv(rcvpkt) && isACK(rcvpkt)    Wait for ▲   call from balow  rdtrcv(rcvpkt) &8 stop and wait notcorrupt(rcvpk extract(rcvpkt,dat sender sends one packet, deliver data(data then waits for receiver response udt send(ACK)

cYae aaaa sndpkt = make_pkt(ACK, chksum) udt_send(sndpkt) (rcvpkt))  chksum) Wait foN W ait for from from below below chksum

sndpkt= makepkt(O,data,checksum) (corrupt(rcvpkt)ll rdt_rcv(rcvpkt) start timer Λ Λ Kait for Wait timeout call Ofrom for udt_send(sndpkt) above ACKO rdt_rcv(rcvpkt) && notcorrupt(rcvpkt) rdt_rcv(rcvpkt) aaisACk(rcvpkt1) && notcorruptrcvpkt stop timer && isACK(rcvpkt,0) W ait W ait for eout for call1from send(sndpkt) ACK1 above rdt_rcv(rcvpkt) rdt_send(data) Λ ynkti,8.8

0 pkt0 r i : ack0 S pkt1 : ack1 s acko s

pkt0 ack0 pkt1 ack1 pkt0 ack0

c e LS ackO pkt1 s ack1 1  pkt1 r pkt0 (c : ack1 ack0  pkto S r ackO S

.0 pkto acko S 0 1. pkt1 ack1 X1 loss 1 pkt1 ack1 1 :0 pkto ackO S

sender receiver send pkt0 _pkto acko send acko rcvacko Yendpktl pktl ackl send ackl rcvackl sendpkto pkto acko send acko

sender receive t=0 r firs RTT -last AC nexl L/R cae

sender receiver d,t=( L/R= -firstp -last bi ACK next↓ L/R p uti

ACKO 2345678

12345678

Λ else cYa ee_ aaaa [base= nextseqnum=1 WaIt dt_rcv(rcvpkt

0 2  3  4 re K re

send pkto send pkt1 send pkt- rece sendpkt3  rece (wait) rece  acko,sendpkt4 ack1, send pkt5 rece recordack3arrived   rece send pkt2 Record acke arrived RCV record ack5 arrived pkt3

2 pkt0 2 pkt1 2 pkt2 2 pkt3 pkt0 W

2 pkt0  2 pktl 2a a pkt2  t pkt0

32 bits ta ed) Source port # dest port # Cour by b # sequence number of d lid acknowledgementnumber (not head not 1 ow len used! l lAD receive window RlSlF  d) # checksum Urgdata pointer  r IN: options (variable length) t AB wn aaplication c data ca aarnableleng

outgoig segmeint lon seidel source port # dest port # sequence number cY checksum rg pointer window size sender sequence number space sent sent,not- usable not ACKed yet ACKed but not  usable ( "in- yet sent flight") incomingsegment to sen source port # dest port# neauencenumber acknowledgementnumber A rwnd checksum urg pointer 3-82

S Seq=42, ACK=79, data = C'' Se q=79, ACK= ,data = C'' b Seq=,ACK=80

50 RTT: gaia.cs.um ass.edu to fantasia.eurecom.fr 00 50 00 50 8. 22 29 20 78. 85 92 99

Create segment, seq. #f: NextSeq pass segment to IP (i.e., “send"  NextSeqNum = extSeqNum +l  if (timer currently not running) NextSeqNum = InitialSeqNum wait start tim er SendBase =]nitialSeqNum for Yaaransmit noteyet-acked segment wint smallest sea.# cYana

SendBase=92 ACK=100 cYaae2 8 SendBase=100 bytes of data. SendBase=120 cYaaaa e

Seq=92, 8 bytesof data Seq=92, 8 bytes of data

Seq=92, 8 bytesof data  Seq=100, 20 bytes of data ACK=100  Seq= , 15 bytes of data

Seq=92, 8 bytes of data Seq= 100, 20 bytes of data ACK=100 1 Seq=100, 20 bytes of data

cappliceatiol y process mama elicnation OS TCP socket receiver buffers S TCP g code S IP code 1

wnd freebuffersace TCDcoamo K . load

connection state: ESTAB connection Variables: seq # client-to-server maerver-to-client rcvBuffer Size

connection state: ESTAB connection variables: seq # client-to-server aerver-to-client rcvBuffer size

req_conn(x) ES acc_conn(x)

Let''s talk OK

ex req_conn(x) 0 ESTAB mit n( acc_conn(x) AB req_conn(x) caannection ent x completes server ESTAB halfopen connection! (no client!)

req_conn(x) ESTAB  ( data(x+1) accept    data(x+1 connection na a completes server 1 es forgetsx req_conn(x) ESTAB data(x+1) accept data(x+1

n, x msg SYNbit=1,Seq=x choc end nsg, ot=1, Seq=y ACKbit=1; ACk n=x+1  i ICK; cain Aekbit=1,ACKnum=y+1 data lrecei dica

server-side Client closed connectionSocket = comesocket.accept();  Λ Socket N(x) news num y,ACKnum=x+1) cket for listen SYN cYback oclient SYN SYN rcvd sent SYNA ESTAB ACK( ACKnum=y+1)

ntSocket.close() can no longer FINbit=1, seq=x receive data ACKbit=1;ACKnum =x+1  close FINbit=1, seq=y T ca se ACKbit=1; ACKnum=y+1 tammed wait Yaa_ a cYaanmentenfetimel

呑吐量理想的拥塞控制实际的拥塞控制无拥塞控制死锁 （吞吐量=0 提供的负 0  拥塞

sriginal data: in throughput: NOUT [Tae shared output link buff ers

2 I 1 R/2

R/2

-2 n lorig ne HostA e c shared ca

copy A free buff er space! finite shared output

2 R/2

copy no buffer space!

A reebuf erspacel

/2 whensen  some/pacl retrahsmis asymptotic is still R /2 λin R/2

A free buffer space!

12 when senc some pack retransmis that are de R/2

c when senc some pack retransmis including that are de c R/2 入：
