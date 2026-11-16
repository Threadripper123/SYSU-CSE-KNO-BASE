# Chapter3 Transport Layer-图集

> 主题库：计算机网络　｜　源文件：Chapter3 Transport Layer-图集.pdf　｜　2,794 字符　｜　25 个片段　｜　更新于 2026-11-11
學大山中立团

山大，

James F.Kurose Keith W.Ross COMPUTER NETWORKING A TOP-DOWN APPROACH P EighthEdition

APACHE @ HTTP SERVER

email WWW phone... SMTP HTTP RTP.. TCPUDP cYethernetPPP CSMA async sonet ... copper fiber radio.

单向(UDP)发送端单向(UDP)接收端单向(UDP)发送端 Socket  Socket  Socket (P3,F Prtx) (IP3 Prtx (IP3 Prx) Host1 host3 Host2

udtsend()I packet packet rdtrcv() unreliablechannel (a) providedservice (b)serviceimplementation

udt_send() packet packet rdt_rcv() unreliablechannel (a) providedservice (b)serviceimplem entation

elluMle uuld elldDle ddld transfer protocol transfer protocol T (sending side) (receivingside) udt_send() packet packet ↓ rdtrcv() unreliablechanne) (a) provided service (b) service im plem entation

rdt send () reliable channel data data ↑deliver data()

rdt send() data data  deliver data() reliable data reliable data cnaansferprotocol transferprotocol sending side) (receiving side cat_send( packet packet Trdt rcv() unreliable channel

data packet— datapackets 十——————————— — ACkpaCkets (a) a stop-and-wait protocol in operation (b) a pipelined protocol in operation

send base nextsegnum already usable, not ack''ed yet sent sent,not not usable yetack''ed windowsize_

send base nextsegnum already usable, not ack''ed yet sent notusable Yetacked window size N (a) sender view ofsequence numbers

outoforder  l acceptable (buffered)but (within window) alreadyack''ed Expected, not not usable Yetreceived rcv_baSe (b) receiver view of sequence numbers

source port # dest port # cYaYaauencenem e head not receive window len used UAPRSE checksum Urg data pointer options(variable length) Yaaa YYa nablelangt e

HostA HostB Network feedback via receiver feedback

MTU（以太网为1500字节） MAC IP TCP 数据头部头部头部头部 FCS MSS

16- 14 12-1 s CPReno s 0- 8- thresh T 6- theh 4- CPTahoe T 2- 0 1 2 3 4 5 6 7 8 9 10   11 12 13 14 15 Transmission round

cwnd  RTO ssthresh ssthresh Time slow congestion congestion congestion slow congestion start avoidance Sky avoidance avoidance start a oicance l

Additive Ircrease Binary Search ax Mlax Probin 知乎 @SKY

Steady State Behavior https://blog.csdn.nev知 @ ky

Chapter 3 outline 3.1 transport-layer services 3.5 connection-oriented cYemultiplexing esegmentstructure 3.3 connectionless transport: reliable data transfer  UDP flow control 3.4 principles of reliable data connection  3.6 principles of congestion control 3. TCPcongestion 3-5

Transport services and protocols application ` transport protocols run in end systems send side: breaks app messages into segments, passes to network layer rcv side: reassembles segments into  messages,passes to applayer  more than one transport protocol available to apt. ation data link physical Internet:TCPand UDP 20241012 3-6

Thanks Email:xieyi5 @mail.sysu.edu.cn https://cse.sysu.edu.cn/content/2462
