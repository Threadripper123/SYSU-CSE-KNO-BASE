# DS-CH-5-命名-图集

> 主题库：分布式系统　｜　源文件：DS-CH-5-命名-图集.pdf　｜　6,467 字符　｜　32 个片段　｜　更新于 2026-11-11
天河无用二号关用二号

80

中算院件院 SENU SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Holp

Process P2 Stub cs” * refers to Client stub cs* same: :server stub as stub cs. Process P3 [aecessP Sereerstub Client stub cs Process P4 _Object Local invocation Ynterprocess caamm unication ldentical server stub

Invocation Server stub is no request is longer referenced_ sent to object by any ('' client stub Client stub sets Server stub at object''s a shortcut Current process returns Ythtae currentlocation (a) (b)

主机的宿主位置 ]1.发送包到主机的宿主位置客户的位置 3.建立一条到当前位置的通道 4.发送连续包到当前位置图5.3 移动IP原理

Each node p maintains a inger table FTp[ with at most m entries: FTρ[i = SUCc(ρ + 2 −1) Note: the i-th entry points to the first node succeeding p by at least 2i-1. ● To look up a key k, node p forwards the request to node with index j Yatiseying q = FTp[ ]≤ k < FTp[ + 1] o fp k FTp[1l, the request is also forwarded to FTp[1l

4 Finger table 2 4 3 9 4 9 5 1 Actual node 30 31- -0- 2 20 3 1 9 2 9 2 3 1 ④ 9 3 4 14 5 4 4 5 20 14 Resolvek = 12 26 from node 28 6 Ya 7 8 11 2 11 ReS0lvek=26 9 3 14 4 18 from node1 5 28 22 10 28 2 28 ② 28 1 14 4 1 2 14 18 5 9 13 20 1 19 21 5 28 -15- 17 -16- 18 4 28 2 18 3 28 5 1 4  5 4

N21 successor(N21)  N32 K24 K30

N21  N26 N32 K24 K30

N21 N26 K24 N32 K24 K30

1 ChordNode: 2 finger(self, i): 3 succ = (self.nodeID + (2, i−1)) self.MAXPROC # succ(p+2^(i−1)) 4 lwbi = self.nodeSet.index(self.nodeID) # self in nodeset 5 upbi = (lwbi + 1) % (self.nodeSet) # next neighbor 6 k ((self.nodeSet)): # process segments 7 self.inbetween(succ, self.nodeSet[lwbi]+l, self.nodeSet[upbi]+l): 8 self.nodeSet[upbi # found successor 9 (lwbi, upbi) = (upbi, (upbi+l) % (self.nodeSet)) # next segment 11 recomputeFingerTable(self): 12 self.FT[o] = self.nodeSet [self.nodeSet.index(self.nodeID)-1] # Pred. 13 self.FT[l:] = [self.finger(i) i (l, self.nBits+l)] # Succ. 15 localSuccNode(self, key): 16 self.inbetween (key, self.FT[o]+l, self.nodeID+l): # in (FT[o], self] 1 self.nodeID #responsible node 18 self.inbetween (key, self.nodeID+l, self.FT[1]): # in (self, FT[1]] 19  self.FT[1] # succ. responsible i (l, self.nBits+l): # rest of FT  self.inbetween(key, self.FT[i], self.FT[(i+l) % self.nBits]): 22 self.FT[i # in [FT[i],FT[i+l])

n Theroot drectory Top-level ode d irT) omainT Directory y node   dir(S) of domain S AsubdomainS of top- )- -level - domain T (Sis contained I nT) aleafdomaincontainedinS

Fiele with no data Field for domain M Location record with only one field, containing an address O o Domain D1 Domain D2

结点中有E的记录， 因此把请求转发给结点中没有E的记录，因此把请求转发给父结点、 图5.7 在分层组织的定位服务中的位置查找

Node knows Node has no about E, so request record for E, is no longer forwarded forwarded and stores pointer Nodecreates record and Domain D Ynee (a) (b)

n2: " "elke" home keys n3:'' ':"max (n5 )" s" n4:' ":"steen" n1 /key "/home/steen/keys" elke steen max Leafnode n2 (n3) n4 keys Dnrectorynode .procmail mbox "/homelsteen/mbox"

[n2n"elke" home keys n4:"steen" n1 n5 "/keys" elke ma steen (n2 n3 n4 Data stored in n6 Yprocmail mbox keys /keys m6 "/home/steen/keys"

Name server Name server for foreign name space [MachineA MachineB keys remote home V "nfs://flits.cs.vu.nl/home/steen" steen mbox Reference to foreign name space

Global layer ggv mil org net com edu jp. us. oracle yale acm jeee ac co uva vu eng cs eng Adminis- jack jill keio nec Cs cYa nal layer ftp cs www ai linda csl pc24 robot pub globule Mana- gerial layer Zone index.htm

Item Global Administrational Managerial 1 Worldwide Organization Department [2b Few Many Vastnumbers 3 Seconds Milliseconds Immediate 4 Lazy Immediate Immediate 5 Many None or few None 6 Yes Yes Sometimes 1: Geographical scale 4: Update propagation 2:#Nodes 5:#Replicas 3: Responsiveness 6:Client-side caching?

resolve(dir, [name1, .., nameκ]) sent to Servero responsible for dir Servero resolves 3 resolve(dir, name1).  dir , returning the identification (address) ) of Server , which stores dir. Client sendsresolve(dir, [name2,., namekl) to Server , etc. nl 3.[vu,cs,ftpl  Name sServer Client''s 4. #[vu], [cs,ftp] nl node name vu vu node 6.#[cs],[ftp] [7.[tpn CS NameServer cYa managed by

resolve(dir, [name1,…, namekl) sent to ), Servero responsible fordir  ServeroI resolves resolve(dir, name1) → dirj, and sends resolve(dir1, [name2, .., namek]) to Server , which stores ( dir . J Servero waits for result from Servery, and returns it to client. Cliens nlnode 3.[ el name vu node 4.fpl 5.#[ftp] Name server cs node

Server Should Looks up Passes to Receives Returns for node resolve child and caches to requester cs [ftp] #[ftp] #ftp]  vu [cs,ftp] #[cs] [ftp] #[ftp] #[cs] [al [vu,c , fp] #[vu] #[ s #vu cal l cYa l root [nl, vu, cs,fp] [vu, cs, fp] #[vu] #[n #[vu, cs, ftp] #[nl, vu, cs] #[nl, vu, cs, ftp]

递归名称解析 R1 名称服务器 11 nl结点 R2 客户 I2_ 名称服务器 13 Ral 称 [e称远距离通信图5.18 递归名称解析与迭代名称解析在通信开销方面的对比

记录类型相关实体描述 SOA 区域容纳表示区域的信息 A 主机容纳结点所代表主机的 IP地址 MX 域指向邮件服务器，该服务器处理发往该结点的邮件 SRV 域指向处理某种服务的服务器 NS ： 区域指向实现指定区域的名称服务器 [CNAME 结点包含指定结点主名称的号链接 PTR 主机容纳主机的规范名称 HINFO 主机容纳结点所代表主机的信息 TXT 任意类型容纳任何被认为有用的、特定于实体的信息

名称记录类型记录值 cs.vu.n1 SOA star 1999121502,7200,3600,2419200,86400 c .vu.nl Ns solo.cs.vu.nl cs.vu.n] TXT "Vrije Universiteit  Math. & Comp. Sc." cs.vu.nl M X 1 zephyr.cs.vu.nl cs.vu.nl Mx 3 star.cs.vu.nl star.cs.yu.nl A star.cs.vu.nl MX star.cs.vu.nl MX star.cs.vu.nl HINFO zephyr.cs.vu.nl A zephyr.lyu.nl MX HINFO ftp.cs.vu.nl CNAM E www.cs.vu.nl CNAME

Attribute Abbr. Value Country C NL Locality L Amsterdam Organization O VU University aarganizationalUnit Ou Computer Science CommonName CN Main server Mail_: _Servers 137.37.20.3, 130.37.24.6, 137.37.20.10 FTP_ _Server 130.37.20.20 WWW_Server 130.37.20.20

Essence Directory Inform ation Base: collection of all directory entries in an LDAP service.  Each record is uniquely named as a sequence of naming attributes (called Relative Distinguished Name), so that it can be looked up. O Directory Information Tree: the naming graph of an LDAP directory caYaaeiceeac noderepresentsaderectoryenry C=NL = Vu Universi au = Computer Science CN = Main server N

Organization VU University Organization VU University OrganizationalUnit Computer Science C OrganizationalUnit Computer Science CommonName Main server CommonName Main server HostAddress 192.31.231.42 2 HostAddress 137.37.20.10 Result of search ((C=NL)(O=VU University)(OU= )(CN=Main server))

中山计算机学院(软件学院) ENUNS SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTER SCIENCEAND ENGINEERING E 命名、标识、地址无层次命名结构化命名 [基於属性命名 5 小结
