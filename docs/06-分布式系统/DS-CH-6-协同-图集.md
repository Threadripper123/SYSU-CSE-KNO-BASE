# DS-CH-6-协同-图集

> 主题库：分布式系统　｜　源文件：DS-CH-6-协同-图集.pdf　｜　6,728 字符　｜　34 个片段　｜　更新于 2026-11-11
天河无用二号关用二号

80

中算院件院 SENU SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Holp

编译器所在的 2144 2145 2146 2147  本地时钟的时间算机 Output.o 被创建编辑器所在的 2142 2143 2144 2145  -本地时钟的时间计算 outputc被创建图6.1 当每台机器都有它自己的时钟时，一个发生于另一个事件之后的事件可能会被标记上较早的时间

Cp(t) = F ( F(t)dt ⇒ dCρ(t) F(t) dt F dCp(t) t:1−ρ≤ ≤1+ρ

Clock time, C   1 dt ) dC(t) dt UTC

A δTreq δT, res

Assumption: δ Treq = T2 − T1 ≈ T4 − T3 = δ Tres θ = T3 + ((T2 − T1) + (T4 − T3))/2 − T4 = ((T2 − T1) + (T3 − T4))/2 δ =((T4 − T1 −(T3 − T2))/2

Tamedaemon 3:00 3:00 3:00 3:05 0 +5 3:00 -10 +15 3:00 +25 -20 Netwerk 2:50 3:25 2:50 3:25 3:05 3:05

World Famouy

Each process P  maintains a local IcounterC; and adjusts this counter   Foreach new event that takes place ! within Pj, Ci is incremented by1.   Each time a m essage I m is sent by r process Pi, the K message  receives a timestamp ts(m)= C. Whenever a message I 1 m is received by a process  Pj, Pj adjusts its local Counter Cj to max C, ts(m ; then executes step 1 before passing Jm to  the application.  Property I P1 is satisfied by (1); Property P2 by (2) and (3).  It can still occur that two events happen at the same time. Avoid this by breaking1l ties through process ; IDs.

application layer Message is delivered Application sends message to application Adjust localclock Adjust localclock Middleware layer V Middleware sends message Message is received Network layer

Ypdate1 update Update1is Update2is performed before performed before

1 Process 2 __init__(self, han): 3 self.queue = [] # The request queue 4 self.clock = 0 # The current logical clock 6 requestToEnter(self: 7 self.clock = self.clock + 1 # Increment clock value 8 self.queue.append((self.clock, self.procID, ENTER) ) # Append request to q 9 self.cleanupQ() # Sort the queue 10 self.chan.sendTo(self.otherProcs, (self.clock, self.procID,ENTER)) # Send request 12 allowToEnter(self, requester): .3 self.clock = self.clock + 1 # Increment clock value 14 self.chan.sendTo([requester], (self.clock, self.procID,ALLOw)) # Permit other 16 release (self) : 17 tmp = [r r self.queue[1:] r[2] == ENTER] | # Remove all ALLOWs 18 self.queue = tmp # and copy to new queue 19 self.clock = self.clock + 1 # Increment clock value 20 self.chan.sendTo(self.otherProcs, (self.clock, self.procID,RELEASE)) # Release c allowedToEnter(self 23 commProcs = ([req[1] req self.queue[1:]]) # See who has sent a message 24 (self.queue[0][1] self.procID (self.otherProcs)== (commProcs))

1 receive(self): 2 msg = self.chan.recvFrom(self.otherProcs)[1] # Pick up any message 3 self.clock = (self.clock, msg[ol) # Adjust clock value.. 4 self.clock = self.clock + 1 # ...and increment 5 mSg[2]==ENTER: 6 self.queue.append(msg) # AppendanENTER request 7 self.allowToEnter(msg[ll) # and unconditionally allow 8 mSg[2]==ALLOW: 9 self.queue.append (msg) # Append an ALLOw 10 mSg[2 ==RELEASE: 1 (self.queue[o) # Justremovefirstmessage 12 self.cleanupQ() # And sort and cleanup

bservation Lamport''s clocks do not guarantee that if C(a) < C(b) that a causally preceded Concurentmessage 1 transmission Observation using logical clocks  Eventε a: m i ,is received atT = 16;

Beforeexecuting an eventP executes VCi[i vCi[i + 1. 2 When process Pj sends a message m to Pj, it sets m''s (vector) timestamp ts(m) equal to VC; after having executed step 1. ca ponthereceiptofamessagem rocesssets VCj[k] ← max{ VCj[k], ts( m)[k]} for each k, after which it executes step 1 and then delivers the message to the application.

Capturing potential causality when exchanging messages P1- (1,1,0) (2,1,0) (3,1,0) (4,1,0) P1 - (1,1,0) (2,1,0) (3,1,0) (4,1,0) P2− (4,3,0) P2 − (2,3,0) (0,1,0) (4,2,0) (0,1,0) (2,2,0) (2,,1) (4,3,2) (2,3,1) (4,,2) (a) (b) ANalySiS ts(m4) ts(m4) (a) (2,1,0) (4,3,0) Yes No m2 may causally |''precede m4 (b) (4,1,0) (2,3,0) No No m 2 and | m4 may ''conf lict

(1,O,O) (1,1,0) m (1,1,0) P 2 m* (1,0,0) P 3 (0,0,0) (1,0,0) (1,1,0)

图片来源：视党中国wwwvog.com

Request Request ok Release NOREply ok Queue is 2 empty Coordinator (a) (b) (c) (a) Process P1 asks the coordinator for permission to access a shared resource.Permissionisgranted. (b) Process P2 then asks permission to access the same resource. The coordinator does notreply (c) When Py releases the resource, it tells the coordinator, which then replies ToP2.

Howrobustisthis system? ● Let p = ∆t/T be the  ! probability that a coordinator resets during a time interval △t, while having a li lifetime : of T.  ● The probability P[k] that k out of m coordinators res eti'' ( during the same interval is P[K = ) (1 −ρ) − O f coordinators reset  correctness is violated when  there is only a minority of nonfaulty coordinators:: when m — f ≤ N/2, or, f ≥ m — N/2. ∑km—n/zP[ 0 The probability of aviolationis

Violation probabilities for various parameter values    N m P Violation N m P Violation 8 5 3 sec/hour <10-15 8 5 30: 1sec/hour <10-10 8 6 3 sec/hour <10-18 8 6 30: sec/hour <10-11 16 9 3 sec/hour <10-27 16 9 30≤ Sec/hour <10-18 16 12 3 sec/hour <10 j−36 16 12 30 Sec/hour <10-24  32 17 3 sec/hour <10 j−52 32 17 30: Sec/hour <10- j-35 32 24 3 sec/hour <10-73 32 24 30 Sec/hour <10 j 49

8 ACCesses resource 0 0 0 8 12 OK OK OK 8 2 Accesses 2 12 OK resource 12 (a) (b) (c)  Two processes want to access a shared resource at the same moment. Po has the lowest timestamp, so it wins. When process Pois done,it sends an OK also, so P2 can now go ahead.

Token

Algorithm entry/exit (in message times) Centralized 3 2 Distributed 2.(N −1) 2·(N−1) Token ing 1,..,∞ 0,.. ,N − 1 Decentralized 2·m.k + m,k = 1,2, ... 2·m.k

Prin iple Consider N processes {Po, ..., Pn–1} and let id(Pk) = k. When a process Pk notices that the coordinator is no longer responding to requests, it initiates an elecon ① Pk sends an ELECTION message to all processes with higher identifiers: P +1,P+2,. ,PN—1. 2 If no one responds, Pk wins the election and becomes coordinator. 3 If one of the higher-ups answers, it takes over and Pk''s job is done.

The bully election algorith m 1 1 2 Electon 5 2 5 2 oK 5 Election 4  Election  6 ④ Ok 6 4  6 0 ③ 0 ③ 0 3 1 ② 5 2 5 TOK ④ ⑥ ④ 1 Coordinator 6 0 3 0 3

[6,0,l] [6,0,1,2] [6,0,1,2,3] 2 3 4 [3,4,5,6,0,1] [3,4,5,6,0,1,2] [3] [6,0] [,4,5,6,0] [3,4] [6,0,1,2,3,4]  [3,4,5] 0 6 5  The solid line shows the election messages initiated by P6

Capacity-   d ② Broadcasting d ② ⑥ b 8 node ⑥  b ① 4 a f ④ a f g ② ④ 9 ② ④ 4 h ⑤ ④ ⑤ h

C d ② d ② e e 6 6 b 1 ① ④ f 4 a g receives a e receives g broadcast g ② broadcast 4 from b first from g fi rst ④ ⑤ ④ h ⑤ h 8 ⑧

0 3 ② C d [d,2] ② [c,] d ④ a 4 a [f,4] g ② f receives [h,8] 4 ④ broadcast g ② from e first i,4] 4 h ⑤ 4 [h,8] h [i,5] ⑤ 一

中山大计算机学院(软件学院） NUN SUNYAT-SEN UNIVERSITY SCHOOL OF COMPUTER SCIENCE AND ENGINEERING  时钟同步逻辑时钟 3 互斥举算法 3 小结
