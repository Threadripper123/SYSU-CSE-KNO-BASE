# DS-CH-7-一致性和复制-图集

> 主题库：分布式系统　｜　源文件：DS-CH-7-一致性和复制-图集.pdf　｜　3,605 字符　｜　23 个片段　｜　更新于 2026-11-11
80

Heip

一致性分布式系统分区可容错性

Process Process Process caal Distributed data store

副本Ａ 副本B 一致性单元一致性单元 x=6;y=3  1 x=2;y=5  1 操作结果操作结果 <5,B> x:=x+2 [x=2] <5,B> x:=x+2 [x=2] <8, A> y:=y+2 [y=2] <10,B> y:=y+5 [y=5] <12,A> y:=y+1 [y=3] <14,A> x:=y*2 [x=6] 向量时钟A =(15,5) 向量时钟B =(0,11) 顺序偏差 =3 顺序偏差 =2 数值偏差 =(1,5） 数值偏差 =(3,6) 图7.2 阐述一致性偏差的示例（摘自（Yu和Vahdat2002））

一致性单元数据项更新更新传播更新推更新更新更新传播副本1 副本2 副本1 副本2 (a) (b) 图7.3 为一致性单元选择适当的粒度 （a）两次更新后才进行更新传播；（b）无需更新传播

P1: W(x)a P1: W(x)a P2: W(×)b P2: W(×)b P3: R(x)b R(x)a P3: R(x)b R(x)a P4: R(x)b R(x)a P4: R(x)a R(x)b (a) (b)

P1:W(x)a P1: W(x)a P2: R(x)a W(x)b P2: W(×)b P3: R(x)b R(x)a P3: R(x)b R(x)a P4: R(x)a R(x)b P4: R(x)a R(x)b (a) (b)

The principle of a mobile user accessing different replicas of a distributed cYaaee Client mevestootherlocaton and ransparenn connecte cYana e l Replicas need to mainnain Wide-area network Read and write operations Portable computer

L: L W(x)—— L2: S(x ) −_ W(x2) L2: − (x2) (a) (b) 图7.13 单一进程P对同一数据存储的两个不同本地副本所执行的写操作 （a）提供单调写一致性的数据存储：（b）不提供单调写一致性的数据存储

邮箱或机号密码 facebook 登录忘记帐户？ 注册联系你我，分享生活，尽在Facebook 永久免费使用姓名机号或邮箱创建密码

Server-initiated replication Client-initiated replication Permanent Yelecas cYaee nnatedreelal cCYanan ennated reeleea l ClientS

C Client Server with COPYOfF Fe Server Q counts access from C and C2 as if they would come from P

Listofclientcaches None 2: Update (and  possibly fetch update) Poll and update 3: Immediate ( (or fetch-update time) Fetch-update time 1:State at server 2eMessagestobeexchanged 3:Response timei at theclient

• Age-based leases: An object that hasn''t changed for a long time, will not change in the near future, so provide a long-lasting lease ● Renewal-frequency based leases: The more often a client requests a specific object, the longer the expiration time for that client (for that object) Will e 。 State-based leases: The more loaded a server is, the shorter the Yapiration imes become

图片来源： 视党中国wawvog.com

veryserverShasalegdenotedas    ( Consider a data item X and let va/ W denote the numerical change in its Yalue af era write operation W . Assume that YW:Val(W) >0  W is initially forwarded to one of the N replicas, denoted as origin( W . TW i,j are the writes executed by serverS that originated from S TW[i,j = ∑{val(W)|origin(W) = Sj & W ∈Li}

Note Actual value v(t) of x: V(t) = Vinit + + ∑ TW[K,K] k=1 value Vj of x at server Sj: +∑7 Vj = Vinit + TW [i, K] [=1

Problem We need to ensure thatvt — e iforevery serverS Approach Let every server Sk m aintain a view  TWk[i,j] of what it believes is the value of TW[i,j]. This information can be gossiped when an update is propagated. Note 0≤ TWk[i,j]≤ TW[i,j]≤TW[,j]

Client Client ae W1 W5 R1 R2 W4 W4 W3 W3 Data store W2 W3 W4 [Wa. Wrntnerequest R1. Readreques W 2. Forward request to primary R2. Response to read W4.Acknowledgeupdate [YWe.Acknowledge write cempleted

Client Client for item x for item x Backup server  R1 R2 W1 W3 W5 W5 W4 W4 Data store W5 W2 W4 W 1. Write request R1. Read request W2. Move item x to new primary R2. Response to read W3.Acknowledge write completed [YWaeac nowledgeupdate

Update1is Update2is performed before performed before

Ensure that each operation is carried out in such a way that a majority vote is established: distinguish  read quorum and I write quorum Three examples of the voting algorithm.( (a) A correct choice of read and write set. (b) ) A choice ) that m ay I lead to write-write conflicts.(c) A correct choice,  known as ROWA (read 1one, write all) 0 A B C D A B C D A B C D E F G H E F G H E G H J K L K K J L J NR=3,  Nw = 10 NR=7, Nw = 6 NR= 1, Nw = 12
