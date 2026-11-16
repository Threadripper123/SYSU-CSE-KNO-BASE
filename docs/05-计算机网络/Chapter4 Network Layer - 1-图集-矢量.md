# Chapter4 Network Layer - 1-图集-矢量

> 主题库：计算机网络　｜　源文件：Chapter4 Network Layer - 1-图集-矢量.pdf　｜　7,433 字符　｜　70 个片段　｜　更新于 2026-11-13
application transport datalink o physical network network data link data link network physical physical data link physical network network data link  data link cYhysieal physical network network data link data link physical work physical data link physical application network transport data link network Fk network physical data link data link data link 4 physical physical physical 4-6

TCP payload TCPH IP payload IPH

packet header 0111

Remote C Controller plane data clane CA CA CA CA CA n arriving header 0111 3 5

forwarding tables computed, routing pushed to input ports routing, management processor control plane (software) farardingdata plane (hardwaree high-seed awitching fabriC

r2 r1 r4 r3 Which router is the best choice for a given dstlP?

r1 Which port goes to the given router?

lookup, link forwarding line layer switch termination protocol fabric areceive) queueing illayer: ception clayer decentralized switching thernet dataaram dest ookup

forwarding table Destination Address Range Link Interface 11001000    00010111- C 0001000000000000  through 0 11001000000101110001011111111111 1100100000010111( ( 0001100000000000     through 1 11001000000101110001100011111111 11001000000101110001100100000000   through 2 11001000000101110001111111111111 otherwise 3

Dee nationAddressRange in interface 0 11001000 00010111 00010********* 11001000 00010111 00011000 1 ××××××××× 11001000 00010111 00011 ** 2 Otherise 3

crossbar

memory

input Output 10 mo 100_ms memory (e.g., (e.g., Ethernet Ethernet さ

fabric plane 0 fabric plane 1 fabric plane 2 fabric plane.3 fabric plane 4 fabric plane 5 fabric plane 6 fabric plane 7 S1 2 S1 为

switch   fabric

switch fabric

switch  fabric

1 1  switch fabric

datagram switch buffer link fabric layer R line protocol termination queueing (send) scheduling

packet de arrivas a

② 1 3 4 5 ET 1 3 2 4 6 CE ① 4 ⑤

(wa gaca) departures  aaerver low priority auel

2 1 ③ 4 ⑤ S ET 1 ③ 2 4 5 ce tures ① ③ 4 ⑤

departures  claesify link

transport layer:TCP, UDP IP  protocol routing protocols • addressing conventions aath selection c ata gram format ·RIP, OSPF, BGP packet handling conventions forwarding ICMP protocol table ·errorI reporting cYaa et link layer physical layer

X fragmente in: one la out:3 sm cYa reassembly

one large datagram becomes several smaller datagrams Iength ID fragflag offset =1500 =x =1 =0  =1500 =x =1 =185 length ID fragflag offset =1040 =x =0 =370

length ID fragflag offset =4000 =X =0 =0

3.1.1.1 223.1.2 223.1.1.4 223.1.2.9 223.1.3.27 23.1.1.3 223.1.2.2 1223.1.3.1 223.1.3.2

223.1.1.1 223.1.1.2 223.1.2.1 223.1.1.4 223.1.2.9 223.1.2.2 223.1.1.3 223.1.327 subnet |223.1.3.2 223.1.3.1

223.12.0/24 223.1.1.1 223.1.1.2 223.1.2.1 223.1.1.4 223.1.2.9 223.1.2.2 223.1.13 223327 subnet [Ta [223.1.3.2 223.1.3.1

223.1.1.2 223.1.1. 1223.1.1.4 223.1.9. 23.1.7.0 223.1.9.1 223.1.7.1 223.1.8.1 223.1.8.0 X  223.1.2.6 23.1: 223. 1.3.27 .2.1」 1223.1.2.2 223.1.3.1」 122 3

网络最大可指派第一个可指派的最后一个可指派的每个网络中类别的网络数网络号网络号最大主机数 A 126 (27-2) 1 126 16777214 B 16383 (214-1) 128.1 191.255 65534

网络号主机号源地址目的地址使用使用代表的意思 0 0 可以不可在本网络上的本主机（见 6.6 节DHCP 协议 0 host-id 可以不可在本网络上的某台主机host-id 全1 全1 不可可以只在本网络上进行广播 （各路由器均不转发） net-id 全1 不可可以对net-id上的所有主机进行广播

类别 IP范围 IP范围络数量 A 10.x.x.x/8 10.0.0.0~10.255.255.255 1 B 172.16-32.X.×/16 172.16.0.0~172.31.255.255 16 C 192.168.x.x/24 192.168.0.0~192.168.255.255 255

part part 11001000 00010111 00010000 00000000

14 1111111 4 000 000

223.1.1.0724 server 223. 11.1.1 223.1.2.1 223.1_1.2 223.1.1. 1.4 223.1.2.9 223.1.2.2 223.1.1.3 223.1.3.3 .27 223.1.2.0/24 223.1.3.1 223.1.3.2

server:223.1.2.5 DHCP discover arriving client Broadcast: is there a DHCp server out there? DHCPoffer iaddr Broadcast: I''m a DHCP your Internet address server! Here''s an IP address you can use DHCPrequest Broadcast: OK. I''ll take that IP address! Broadcast: OK. You''ve got that IP address!

DHCP DHCP  DHCP IP DHCP Eth Phy IDHCP DHCP DHCP 168.1.1.1 IDHCp IP DHCP Eth router with DHCP Phy server built into

DHCP DHCP DHCP IP IDHCP Eth Phy DHCP DHCP D  aDHCP IP DHCP Eth router with DHCP Phy SerVer built into DHcP rou l ter

Organizationo 200.23.16.0/23 Organization 1 “Send me anything 200.23.18.0/23 withaddresses beginning Orqanization 2 Fly-By-Night-ISP 200.23.16.0/20” 200.23.20.0/23 200.23.16.0/20 Ir Organization 7 、 200.23.30.0/23 IS Ps-R-Us “ Send me anything 199.31.0.0/16 with addresses beginning 199.31.0.0/16”

200.23.16.0/23 Organization 1 “Send me anything 200.23.18.0/23 wnth addresses beginning Orqanization 2 Fly-By-Night-ISP 200.23.16.0/20 , In 200.23.20.0/23 200.23.16.0/20 200.23.30.0/23 ISPs-R-Us “Send me anything 199.31.0.0/16 with addresses beginning Organization 1 199.31.0.0/16 or 200.23.18.0/2 200.23.18.0/23

restof local network Internet (e.g., home network) 10.0.0/24 10.0.0. 10.0.0.4 X  10.0.0 138.76.29.7  10.0.0

IP范围 IP范围络数量 A 10.x.x.x/8 10.0.0.0~10.255.255.255 1 B 172.16-32.X.X/16 172.16.0.0~172.31.255.255 16 c 192.168.x.x/24 192.168.0.0~192.168.255.255 255

10.0.0.1 client 10.0.0.4 sts 138.76.29.7 NAT router

10.0.0.1 in IGD otocol NAT router

2.connection to  relay initiated d 10.0.0.1 . connection to by client relay initiated rskype byNATed host aarelayng established xC 138.76.29.7 NAT router

92.168.1.10 peer NAT peer NAT 10.0.0.10

NAT peer peer

UDPsocket (LocalIP:LocalPort) (PublicIP: PublicPort)

UDPsocket (LocalIP:LocalPort) (PublicIP: PublicPort)

Ulsocket (LocalIP:LocalPort) (PublicIP: PublicPort)

ca (IP, Port) (LocalIP:LocalPort) (PublicIP2: PublicPort2)

ver pri flow label payload len next hdr hop limit sourcei address (128bits) cYestination address (128bis) DAta

IPv6 IPv4 IPv6

dest addr IPv6: source destaddr vT uy Ou UDP/C aload

Doubie piotocoi stacr A B IPv4 t tunne/ E F connecting IPv6 routers IPv6 IPv6 IPv6 IPv6 A B C D E F IPv6 IPv6 IPv4 IPv4 IPv6 IPv6 flow:X src:B src:B flow:X src:A dest:E dest:E src:A dest:F dest:F  Flow:X Flow:X Src:A Src:A data Dest:F Dest:F data data data A-to-B: E-to-F:

logically-centralized routing controller ol plane plane Yanalfl ew able headers counters actions 0100|1101 3 2 values in arriving x caacket_eheader

Flow table Router''s flow table match action router''s match+actic

src = *.*.*.* Flo w table  src=1.2.*. 米 match action src=10.1.2.3 1 4 3

1. Forward packettoports 2.  Encapsulate   and forward tocontroller 4. Send to normalr processing K pipeline  5. Modify Fields Switch VLAN MAC MAC Eth IP IP IP TCP TCP Port ID Src dst type Src Dst Prot Sport dport

Host h6 d(4) 10.3.0.6 s3 OpenFlow confroller 4 3 ost h5 0.3.0.5 S1 S2 ost h1 4 3 0.1.0.1 Host h2 10. 1. 0.2 match

[match acion ingress ort=1 IPSrc = 10.3. ** forward(4)  .  :  IPDst= 10.2.

1P SrC = 10.3.*. Hosth6 IP Dst = 10.2.*.* forWard(3) 10.3.0.6 s3 openFlow Coneroller 2 3 Host h5 10.3.0.5  S2 Host h1 2 10.1.0.1 match

national or global ISP datacenter network enterprise c network

HTTP SMTP RTP QUIC DASH 1 TCP UDP IP Ethernet PPP PDCP WiFi Bluetooth copper radio fiber

HTTP SMTP RTP / QUIC DASH TCP UDP NAT caching IP NV Firewalls Ethernet PPP PDCP WiFi Bluetooth copper radio fiber

application application transport network hop-by-hop (in-network) implementation of reliable data transfer transport network aan physical network data link link link link link link physical physical physical physical physical physical

application end-end im plem entation of reliable data transfer applicatio transport transpor netor netor data link data link physical physical
