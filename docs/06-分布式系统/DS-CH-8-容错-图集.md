# DS-CH-8-容错-图集

> 主题库：分布式系统　｜　源文件：DS-CH-8-容错-图集.pdf　｜　12,046 字符　｜　54 个片段　｜　更新于 2026-11-11
天河无用二号关用二号

80

中算院院 SENU SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Holp

BLOCKCHAIN 8

Availabiity Readiness ; for usage Reliability Continuity of service delivery Safety Very | lowI probability  of catastrophes c Maintainabiity How e easy can a failed system be repaired

Failure A component is not living up to Crashed program Ytae specifications Error Part of a component that can Programming bug lead to a failure Fault Cause ofan error Sloppy programmer

[Tearm Description xample Fault Prevent the Occurrence Don''t hire slop py prevention of a fault programmers Fault tolerance Build: ail component Build each I component suchi that it can mask byt two independent the occurrence of a programmers cYaul Fault rem ova Reduce ! the presence, Get rid of sloppy number, orseriousness programmers ofafaul Fault Estim ate Current Estimate how a forecasting presence, future recruiter i is doing''| when incidence, and it comesi ; to ,hiring consequencesof faults sloppy programmers

Type Description of servers behavior Crash failure Halts, but is working correctly until it halts mission failure Fails to respond to incoming requests Receive omission Fails to receive incoming messages Send omission Fails to send I messages Taming fallure Response lies outside a specified tim e interval Response failure Response is incorrect Value failure The value of the response is wrong State-transition failure Deviates from 1 I the correct flow of control Arbitrary failure M ay I produce arbitrary responses at arbitrary

o Asynchronous system:no assumptions about process execution speeds or message delivery times → cannot reliably detect crash failures. o Synchronous system: process execution speeds and message delivery times are bounded → we can reliably detect omission and timing failures. o In practice we have partially synchronous systems: most of the time, we can assume the system to be synchronous, yet there is no bound on the time that a system is asynchronous → can normally reliably detect crash failue

Fail-stop Crash failures, but reliably detectable Fail-noisy Crash failures, eventually reliably detectable Fail-silent Omission 1 or crash failures: clients cannot tell what wenti :wrong Fail-safe Arbitrary, yet benign failures (i.e., they cannot do any harm) Fail-arbitrary Arbitrary, with m alicious failures

A ③ (a) [投票者 A2 V2 B2 V5 V8 3 v3 B2 V6 V9 (b) 图8.2 三倍模块冗余

Flat group Hierarchical group Coordinator Worker

Commanding Atack! A ac ! Lieutenant Lieutenant ? General General

000

SOORK

span:sr,ss ② Service2 span:sr,ss span:cs,cr span:cs,cr ③ 服务器2 请求 Gateway Service1 ④. span:sr,ss ⑥ 服务器0 服务器1 ⑤ service3 服务器3 DB Zipkin 跟踪数据落盘界面展示

1 In round r, P; m ulticasts its known set of comm ands C! to all others ca At theend ofr eac mergesall received commands intoanew 3 Next command cmd; selected through a globally shared, deterministic function:cmd ← select(C+1).

P1 P 2 decide decide PA decide

ESSENCE We consider| process!; groups in which communication I between process is inconsistent: (a) improper forwarding ( of messages, or I (b) telling different things to different processes. (P1 (P1 a a a b (P3) (P2) (P3) (P2) b b (a) (b)

System model ● We consider aprimaryP:''and n—1 backups B1,... ,Bn-1. ● MessagesI ; may be lost, but thisi: can be detected. ? Messages cannot be corrupted beyond detection. 0 A receiver of a message can reliably detect its sender. BA1: Every nonfaulty backup 【 process stores 1 ;the same value. BA 2: If the prim ary is I ; nonfaulty then every nonfaulty backup process stores Observation Primary faulty= = I BA1 says that backups may store the same, but different (and thus wrong) value than originally sent by the client. Primary not faulty ⇒ satisfying BA2 implies that BA1 is satisfied.

Second message round F T 1 T E {T,F} B1 B2) {T,F} B1 B2) {T,F} E T

{T,{T,F}} B1 Faulty process Second message round F T F P {T,{T,F}} B1) E {F,{T,T}} (B2 B3) {T,{T,F}} T F T T P F {T,{T,T}} (B2) B3 {T,{T,F}} T

x Bounded x Unbounded Unicast Multicast Unicast Multicast massagenransmission

Any networked system providing shared data can provide only two of the fYallowing three properties C: consistency, by which a shared and replicated data item appears as a single,up-to-date copy A: availability, by which updates will always be eventually executed

o Each process is equipped with a failure detection module aaaprocess Pprobesanotherprocess a forareaction • If Q does not react with t time units: Q is suspected to have crashed

Taeclient sunable olecatetheserve 2 The request message from the client to the server is lost. ca Theservercrashesaf er receivinga request The reply message from the server to the client is lost. cY Theclientcrashesafersendingarequest

REQ Server REQ Server REQ Server Receive Receive Receive xecute xecute Cras REP Reply No REP Crash No REP (a) (b) (c)

Where (a) is the normal case, situations (b) and (c) require different solutions. awever we don''t knowwhat happened Twoapproaches o At-least-once-semantics: The server guarantees it will carry out an • At-most-once-semantics: The server guarantees it will carry out an aperationat mostonce.

actionsbetween bracketsnevertakeplace ↑ M → P → C: Crash after reporting com pletion. M → C → P: Crash after reporting completion, but before the update. 4 P → C(→ M):Updatetookplace,and thenacrash. ⑤ C(→P → M):Crash before doing anything 6 C(→ M → P):Crash beforedoinganything

Strategy M → P StrategyP → M Reissue strategy MPC MC(P) C(MP) PMC PC(M) C(PM) Always DUP OK OK DUP DUP OK Never OK ZERO ZERO OK OK ZERO Only when ACKed DUP OK ZERO DUP OK ZERO Only when notACKed OK ZERO OK OK DUP OK Client SerVer Server OK Document updated once = DUP = Document updated twice ZERO = Document not update at all

Sender Recipient Recipient Group membership Group membership Group membership functionality functionality fnctionality Message-handling Message-handling Message-handling Local OS Local OS Local OS Network

Receiver missed Sender Receiver Receiver Receiver Receiver _buffer Last = 24 Last = 24 Last = 23 Last = 24 [M25 M25 M25 M25 Sender Receiver Receiver Receiver Receiver Last = 25 Last = 24 Last = 23 Last = 24 M25 M25 M25 M25 ACK25 ACK25 Missed 24 ACK25 [TYa e

发送方仅接收到一个接收方抑制它们的反馈发送方接收方接收方接收方接收方 T=3 T=4 T=1 T=2 NACK NACK NACK NACK NACK 图8.10  儿个接收方要发送重发请求，但是第—个重发请求抑制了其他的请求

发送方 （长距离）连接协作者局域网 1 R 接收方根图8.11 分等级的可靠多播。每个本地协作者都把消息转

The client who initiated the computation acts as coordinator; processes Yaauiredte commitarethe participants  Phase a Coordinator sends VOTE-REQUEsT to participants(also called a pre- rite • Phase 1b: When participant receives VOTE-REQUEST it returns either VOTE-COMMIT or VOTE-ABORT to coordinator. If it sends VOTE-ABORT, it Phase 2a: Coordinator collects all votes; if all are VOTE-COMMIT, it sends GLOBAL-COMMIT to all participants, otherwise it sends GLOBAL-ABORT  Phase 2b: Each participant waits for GLOBAL-COMMIT or GLOBAL-ABORT and handles accordingly.

Vote-request  Vote-commit  WAIT READY Vote-abort Vote-commit Global-abort Global-commit Global-abort Global-commit ACK : ACK ABORT COMMIT ABORT_ COMMIT (a) (b) 图 8.182PC中协作者参与者的有限状态机 （a）2PC中协作者的有限状态机；（b）参与者的有限状态机

COMMIT Make transition to COMMIT ABORT Make transition to ABORT INIT Make transition to ABORT 2 READY Contact another participant

3 run(self): 4 yetToReceive= (participants) 5 self.log.info( wATT) 6 self.chan.sendTo (participants, VOTE_REQUEST) 7 (yetToReceive) > O: 8 msg = self.chan.recvFrom(participants, TIMEOUT) 9 ( mSg) (msg[1] = VOTE_ABORT): 10 self.log.info( ABoRT) 11 self.chan.sendTo (participants, GLOBAL_ABORT) 13 :# mSg[1]== VOTE_CQMMIT 14 yetToReceive.remove(msg[ol) 15 self.log.info( commT ) 16 self.chan.sendTo (participants, GLOBAL_COMM T)

1 Participant 2 run(self): 3 msg= self.chan.recvFrom(coordinator, TIMEOUT) 4 ( msg): # Crashed coordinator - give up entirely 5 decisiOn=LOCAL_ABORT 6 : # Coordinator will have sent VOTE_REQUEsT 7 decision = self.do_work() 8 decision LOCALABORT: 9 self.chan.sendTo(coordinator, VOTE_ABORT) 10 : # Ready to commit, enter READY state 11 self.chan.sendTo(coordinator, VOTE_COMMIT) 12 msg = self.chan.recvFrom(coordinator, TIMEOUT) 13 ( msg): # Crashed coordinator – check the others 14 self.chan.sendTo (all_participants NEED_DECISION) 15 True: 16 msg = self.chan.recvFromAny() 17 mSg[1 [GLOBAL_COMMIT, GLOBAL_ABORT,LOCAL_ABORT]: 18 decision = msg[1 20 : # Coordinator came to a decision 21 decision = msg[1] 23 True: # Help any other participant when coordinator crashed 24 msg = self.chan.recvFrom(all_participants) 25 mSg[1 = NEED_DECISION: 26 self.chan.sendTo([msg[o]l, decision)

INIT Vote-abort INIT Commit Vote-request WAIT READY Vote-abort Vote-commit Global-abort Prepare-commmit Global-abort, Prepare-commit ACK Ready-commit ABORT PRECOMMIT ABORT PRECOMMIT Global-commit ACK COMMT COMMIT (a) (b) 图 8.22 3PC中协作者和参与者的有限状态机 （a）3PC中协作者的有限状态机；（b）参与者的有限状态机

W hen a failure occurs, we need to bring the system into an error-free state: Foward error recovery: Find a new state from which the system can continue operation O Backward error recovery: Bring the system back into a previous error-free

Initial state Recovery line Checkpoint Failure P2 Time Message sent Inconsistent collection fram P2to P1 of checkpoints

m' 'm 失败 P2 时间图8.25 多米诺效应

EsseNce Each process independently takes checkpoints, with the risk of a cascaded rollback to system startup. ● Let CPi(m ) denote mth checkpoint of process Pj and INTi(m) the interval betweenCPim—1 andCPi(m. ? When process Pj sends a message in interval INT;(m), it piggybacks (i,m) ? When process Pj receives a message in interval INTj(n), it records the dependency INTi(m) → INTj(n). ● The dependency INT;(m ) → INTj(n) is saved to storage when taking checkpointCPj(n). Ybservaton  process Pj rolls back to CPi(m — 1), Pj must roll back to CPj(n — 1).

Essence Each process takes a checkpoint after a globally coordinated action. Use a two-phase blocking protocol: When a participant receives such a message, it takes a checkpoint, stops sending (application) messages, and reports back that it has taken a checkpoint ● When all checkpoints have been confirmed at the coordinator, the latter broadcasts a checkpoint done message to allow all processes to continue Observation It is possible to consider only those processes that depend on the recovery of the coordinator, and ignore the rest

CRU M

Assumption We assume a piecewise deterministic execution model: • The execution of each process can be considered as a sequence of state intervals ? Each state interval starts with a nondeterministic event(e.g , message Yece p xecution in a state interval is deterministic Conclusion If we record nondeterministic events (to replay them later), we obtain a deterministic execution model that will allow us to do a complete replay.

When should we actually log messages? Avoid orphan processes: ? Process Q has just received and delivered messages my and m2 ● Assume that m₂ is never logged. ? After delivering m1 and m2, Q sends message mz to process R Process R receives and subsequently delivers mg: it is an orphan. [Ym1 m1 m2 isnever replayed, Q X soneintherwillm3 m3 m2, m2 .m3 TimE Unlogged message Logged message

Notatons DEP(m): processes to which m has been delivered. If message m* is  causally dependent on the delivery of m, and m* has been delivered to Q, then Q∈DEP(m). ? CopY(m): processes that have a copy ofm , but have not(yet) reliably Characterization Qis orphaned ↔ ∃m : Q ∈ DEP(m) and COPY(m) ⊆ FAIL

For each nonstable message m, there is at most one process dependent on m, that s|DEP(m)l≤1. Consequence An unstable message in a pessimistic protocol must be made stable before

Optmisic protocol For each unstable message m, we ensure that if COPY(m) ⊆ FAlL, then eventuallyalsoDeP(mcFAll. Consequence To guarantee that DEP(m) ⊆ FAlL, we generally rollback each orphan process QunilQ DEP( .

2819 5 l am azon.com''N ErLIK

中山大學计算机学院（软件学院） NUNS SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEAND ENGINEERING I 基本概念进程恢复可靠的通信式分布式提交 5 系统恢复
