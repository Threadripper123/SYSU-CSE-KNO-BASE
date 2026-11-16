# DS-CH-9-paxos-图集

> 主题库：分布式系统　｜　源文件：DS-CH-9-paxos-图集.pdf　｜　9,273 字符　｜　57 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

G

Partition Tolerance BASE Consistency  Availability

SpectrumofConsistencyModels Easier to achieve easier to program good performance Weak/relaxed Strong [ eentual nearnabili Consistency [Herliny] Causal Sequential Release (aka external Consistency Consistency Consistency Consistency)

2

Strict Serializable Serializable Linearizable Repeatable Snapshot Sequential Read lsolation Cursor Monotonic Causal Stability Atomic View Read PRAM Committed Writes Read Read Follow Monotonic Monotonic Your Uncommitted Reads Reads Writes Writes

Not available during some types of network failures. Some Unavailable ar all nodes must pause operations in order to ensure safety. Available on every non-faulty node, so long as clients only Sticky Available talk to the sam e servers, instead of switching to new ones. Available on every non-faulty node, even when the network [TatalAvailable iscompletelydown.

What is the Problem?

T oGeneralsProblm Twogeneralsneed to beagree on time toattack towin. They communicate through messengers, who may be killed on their   Agreement is the problem.

Replicated State Machine Problem (1/2) The solution: replicate the server.

Replicated State Machine Problem (2/2) Make )  the SerVer deterministic (state machine). Replicate the server. Ensure COrrect replicas step through the same sequence of s state transitions (How? Agreement is the problem.

The Agreement Problem ▲

Some nodes propose values (or actions) by sending them to the othrs All nodes must decide whether to accept or reject those values. Concurrent processes and uncertainty of timing, order of events and Failure and recovery of machines/processors, of communication channels.

Validity: only ε a a value that has been proposed may be chosen. Agreement:   no two correct nodes choose different values.  Integrity:   anode chooses at m ost Once.  Termination: eVery correct node eventually choose a avalue.

Agreement in Distributed Systems: Possible Solutions Two-PhaseCommit(2PC) PaXOS

Two-Phase Commit (2PC)

The problem first was encountered in database : systems.  Suppose a database system is  updating some complicated data structures that include parts residing on more than one machine. System  model:  Concurrent prOcesses and uncertainty of timing, order of events and C inputs (asynchronous systems. Failure ● and recovery ( of machines/processors, of communication channels.

You want to organize outing with 3 friends at 6pm Tuesday. ( Go out only ifall friends can make it.

What do you do?  Call each of them and ask if can do 6pm on Tuesday (voting phase) • If all can do Tuesday, call each friend back to ACK (commit) • If one cannot do Tuesday, call other three to cancel (abort)

Intuitive Example (3/3) Critical details  While you were calling everyone to ask, people who  have promised they o 6pm can do Tuesday must reserve that  slot.  You need to remember the  decision and tell anyone whom you have not been able :to reach during . commit/ / /abort phase. Thatis eXactly hoW 2PC Works.

The PC Playrs Caordinator Transaction Manage Begins  transaction. Responsible for Commit/abort. Participants Resource Managers  The servers with the data used in the distributed transaction.

The2PCAlgorithm-Prepare Phase Coordinator asks each participant canCommit. ( Participants must prepare to commit using permanent storage before answering yes. ocktheobjects.  Participants are not allowed to cause an abort after it replies yes to  canCommit. Outcome of the transaction is uncertain until doCommit or doAbort.  ( Other participants might still cause an abort.

The 2PC Algorithm I- ( Commit Phase  The Coordinator collects all votes. lfunanimous yes, Causes Commit.  lfanyr participant voted no, causes abort.  The  fate of the transaction  is decided atomically: at the  coordinator once all participants vote.  Coordinator I records fate using perm anent storage. ● Then broadcasts doCommit Or doAbort1 to participants.

2pC Sequence eof vents cmanCommit mrepaRed" Yes "committed" (objects still doCommit (persistently) locked) haveCommitted. 1 "committed" L "DoNE"

Impossibility of Distributed Consensus with One Faulty Process

FischrLynchPatrsonFP  M.J. Fischer, N.A. Lynch, and M .S. Paterson, Impossibility ofdistributed consensus with onefaulty process, Journalofthe ACM , 1985.

lt is impossible for a setc of processors in an  asynchronous , system to agree on a binary value, even if  only a a single  is s subject   process to an unannounced failure. Thecoreofthe problem l is sasynchrony.

What FLP says: you Cannot guarantee both safety and progress when there is even a single fault at an inopportune moment. What FLP does ; not  say: in practice, . how close get to the  Can you ideal (always safe e and live)?

PaXOS The only known completely-safe and largely-live agreement prtcl. L.Lamport, The part-time parliament,ACM Transactions on

Proposers  Suggests values for consideration by acceptors. Acceptors ● Considers the: values proposed by proposers. Renders = ● an 1 ; accept/i  /reject decision. / earners Learnsthechosenvalue A node can act as m ore than one roles (usually 3).

Paxos Components       Acceptor Proposer Acceptor Learner Proposer Acceptor

Single P oposal Single Acceptor Use just one  acceptor ● Collects proposers proposals. ● Decides the value and tells e everyone else.  Sounds  familiar?  two-phase commit (2PC)  acceptor fails= protocol blocks

SingleProposal MultipleAcceptors i One acceptor is not fault-tolerant enough. et aeemultipleacceptors  From there, must reach a decision. How? Decision = value accepted by the majority. 8  P1: an acceptor must accept first proposal it receives.

Multiple Proposals Multiple Acceptors If there are multiple proposals, no proposal I may get the majority.   3 proposals may each getl/3 of the acceptors. Solution: acceptors Can accept multiple proposals, distinguished by a unique proposal number.

Acceptor:存储节点，接受、处理和存储消息. Quorum(Acceptor的多数派)：n/2+1个Acceptors. Round:1轮包含2个阶段: Phase-1& Phase-2 [每轮编(d)： 单调递增；后写胜出；全局唯一(用于区分Proposer)；

Acceptor看到的最大rnd(last_rnd)： Acceptor记住这个值来识别哪个proposer可以写。 Yaluev_Acceptor接e的值 Valueround numbervrnd): [r 有多数(多于半数)个Acceptor接受了这个值.

Phase 1 Iast rnd=0, v=nil, vrnd=0 last_rnd=0, v=nil,vrnd=0._ 当Acceptor收到phase-1的请求时： 如果请求中rnd比Acceptor的lastrnd小，则拒绝请求 Gi求中的nd保存到本e的lastnd 从此这个Acceptor只接受带有这个last_rnd的phase-2请求。 ● 返回应答，带上自己之前的lastrnd和之前已接受的v.

ProposerX Acceptor1,2,3 Phase1 last rnd=0,v=nil,vrnd=0 last_rnd=0,v=nil,vrnd=0.. [lbroposer收到Acceptor发a的应o ： 如果应答中的lastrnd大于发出的rnd：退出. 不能改变(可能)已经确定的值如果所有应答的v都是空，可以选择自己要写入v. [abe o e

[roposerX Acceptor1,2, X 1, 1, Phase2 Accepted 1,x, 1,x, V=X, vrnd=1 Proposer: 发送phase-2，带上rnd和上一步决定的v

Phase 2 Accepted 1,×1 1,× Acceptor: [ae_nd不等aaceeter的last nd的i求将phase-2请求中的v写入本地，记此v为已接受的值 Iastrnd==rnd保证没有其他Proposer在此过程中写入过其他值

ProposerX Acceptor1,2,3 RNd=1 Phase 1 last_rnd=0, v=nil, vrnd=0 v="X", rnd=1 1, Phase 2 Accepted v=X, vrnd=1

Time [Yaund 1 rnd=1 Phase 1 for X round=2 rnd=2 Phase 1 for Y OK,forgetx v="x",rnd=1 Fail 1,x, 2. 2 Phase 2 v="y",rnd=2 1,×, 2, 2, OK 1,×1 2,y2 2,y2

rouND=3 rND=3 1,X1 2,y2 2,y2 Phase =yvrnd=2 v="x",vrnd=1; choosey X 3,x{ 3,y2 2,y2 ×只能选择v=“y， 因为它可能 [T Y_定 v= y ,vrnd=3 3,x₁ 3,y2 2,y2 Phase2

⚫ Acceptor发送phase-3 到所有learner 色， 让learner知道  [ Livelock 多个Proposer并发对1个值运行paxos的时候，可能会互相覆盖对方的rnd，然后提升自己的rnd再次尝试，然后再次产

If a value v is chosen at proposal number , any value that is ; sent out in ; phase e 2 of any  later proposal numbers m ust be also U.

Paxs-Safetyy (2/3) Decision = Majority (any two majorities share at least one element Therefore after the first round in which there is a decision, any  subsequent round involves at least one acceptor that has accepted

Now suppose our claim is not true, and let m is the first proposal num ber that is later than n and in 2nd phase, the value sent out is This is not possible, because if the proposer P was able to start  2nd phase for w,it means it got a majority to accept round for m (for m > n). So,either: • v would not have been the value decided, or Therefore, once a majority accepts v, that never changes.

If  two or more proposers race to propose new values, they might   step on each other toes all the time.  P1: ● prepare(n1) 2 prepare(n2 P2 accept(n2, v2) Pl: prepare(n3 P2: prepare(n4) n₁n2n3n4 With  randomness,this( Occurs ( exceedingly  l rarely.

1个实例(确定1个值）写入需要2轮RPC. MultiPaxo 约为1轮RPC，确定1个值(第1次RPC做了合并). FastPaxos 没突_轮RPC确定一个值有突：2轮RPC确定一个值

将多个paxos实例的phase-1合并到1个RPC； 使得这些paxos只需要运 phase-2即可 J  [应用： chubby zookeeper megastore spanner

Cheap Paxos  Stoppable Paxos  Fast Paxos  Mencius  EPaxos  Generalized Paxos  Vertical Paxos

Replicated AND Tolerant Paxos

Algorithms Should Be Designed For [a Effi ciency? [a Understandability!

[Yvervie Allows collection of machines to work as coherent group Continuous service, even if some machines fail  Paxos has dominated discussion for 25 years Hardtounderstand Not complete enough for real implementations Neaeconsensusalgorithm Raf Primary design goal: understandability (intuition, ease of explanation) Complete foundation for implementation Different problem decomposition User study shows Raft more understandable than Paxos Widespreadadoption
