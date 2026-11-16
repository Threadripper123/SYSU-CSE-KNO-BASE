# Chapter5 Network Layer - 2-图集

> 主题库：计算机网络　｜　源文件：Chapter5 Network Layer - 2-图集.pdf　｜　3,633 字符　｜　23 个片段　｜　更新于 2026-11-11
学大山中立团

山大

James F.Kurose Keith W.Ross COMPUTER NETWORKING ATOP-DOWNAPPROACH

Host Router Multicast Host Router

4b 4c AS4 2a 3a 2b 3b AS2 AS3 1a Receive BGP advertisements for 1d 212.21.21.21 from Forward toward Advertise Server B since it is closer. 212.21.21.21 CDN Server A Advertise 212.21.21.21 CDN ServerB Figure 5.12 Using IP-anycast to bring users to the closest CDN server

TBM

network-control applications routing access load control balance control northbound APl plane SDN Controller (network operating system) southbound API

network-control applications routing access load Control balance

SDN-controlled switches

northboundAPl (network operating system) southboundAPl SDN-controlled switches

?

nof ;  DN e?

前4个字节 0 8 16 31 都是一样的类型代码检验和 （这4个字节取决于ICMP报文的类型） ICMP的数据部分（长度取决于类型） ICMP报文首部数据部分 P数据报

NMS NETCONF ccalient NETCONF IP Network NETCONF Server  0

Netconf-Client [a ContentLaye C onfgrai Data Notfcain cedit-configo Message Layer <rpc>,<rpc- reply> < notification > Layer SSH,TLS,BEEP/TLS,SOAP/HTTP/TLS, Netconf-Server

Netconf-Client ca Layer Example Contnt a r Confguai D a N oticain peration Layer eget-configo, Message Layer <rpc>,<rpc- reply> <notification> cYaea neel Layer SSH, TLS, BEEP/TLS, SOAP/HTTP/TLS,. Netconf-Server

Netconf-Client ca Layer amele ContentL r ongurai D a N oticatinD aperation Layer eget-confgo, Message Layer <rpc>,<rpc- reply> <notification> cYacaReaTYanee Layer SSH, TLS, BEEP/TLS, SOAP/HTTP/TLS,… Netconf-Server

网络管理系统 IT运维系统 YANG模型 YANG模型 NETCONF RESTCONF Client Client [TNet o SSH HTTPS NETCONF RESTCONF Server YANG模型 Server

YANG YIN ontainerauto-recovery-times containername= auto-recovery- listauto-recovery-time{ times"> leaferror-down-type{ typeerror-down-type; 无损转换 <keyname="error-down-type"> leafname="error-down-type"> } <typename="error-down-type"/> leaftime-value{ /leafo typeuinta2 leafname=time-value> } rangevalue="30.86400"/> units"s"; 无损转换 <unitsvalue="s"/> mandatorytrue mandatoryvalue true"/ /leafo

01 <?xml version="1.0" encoding="UTF-8"?> 02<rpcmessage-id="10l" 03 xmlns="urn:ietf:params:xml:ns:netconf:base:l.o"> 04 <edit-config> 05 <target> 06 <running/> 07 </target> 08 <config>  09 <top xmlns="http://example.com/schema/ 1.2/config"> <interface>  <name>Ethernet0/0</name> <mtu>1500</mtu> </interface> </top> </config> 16 </edit-config> 17 </rpc>

Computer Networking   谢逸中山大学・计算机学院 2024.FaI

Chapter 5 JamesFKuroseKeith Ross Network ayer: Control Plane aanoteontheuseofthesePoweRPointslides We''re making these slides freely available to all (faculty ,,students, readers). They''re in C MPUTER PowerPoint i form so you see the animations; and can add,modify,and delete slides (including this one) and slide content to ) suit your needs. They obviously represent a lot NETWORKlNG of work on our part. In return for use, we only ask the following: A TOP-DOw N APPROACH Eighth Edition If you use these slides (e.g., in a class) that you m ention their source (after all, we''d like people to use our book!) youpostany slidesonawwesitethatyounotethat theyareadaptedfromor perhaps identical to) our slides, and note our copyright of this material. Computer Networking: A Forarevision history,seetheslidenoteforthispage. Top-Down Approach 8h edition Thanksand enjoy! JFK/KW R Jim Kurose,KeithRoss Allmaterialcopyright 1996-2023 PearsOn,2020 aaeauroseandKW.RossAllRightsReseed

Per-router control plane Individual routing algorithm components in each and every router interact with each other in control plane to compute forwarding tables Routing Algorithm control  plane -- Localforwarding data table plane header  output 0100 0110  0111  1001 一 5-7
