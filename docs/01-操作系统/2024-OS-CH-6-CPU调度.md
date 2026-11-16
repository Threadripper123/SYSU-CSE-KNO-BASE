# 2024-OS-CH-6-CPU调度

> 主题库：操作系统　｜　源文件：2024-OS-CH-6-CPU调度.pdf　｜　50,463 字符　｜　185 个片段　｜　更新于 2026-11-11


Operating Systems Principles
操作系统原理
陈鹏飞
计算机学院
1

第六讲 — 进程调度
                                                              ——处理器调度
2

3
!"
v
基本概念
v
调度标准
v
调度算法
v
线程调度
v
多处理器调度
v
实时CPU调度
v
操作系统示例
v
算法评估

4
#$
Ø了解长程、中程、短程调度和区别；
Ø描述各种CPU调度算法
Ø根据调度标准评估CPU调度算法
Ø解释与多处理器和多核调度相关的问题
Ø描述各种实时调度算法
Ø描述Windows、Linux和Solaris操作系统中使用的调度
算法；
Ø应用建模和仿真评估CPU调度算法
Ø掌握传统UNIX系统中的调度思想；

5
%&'()
Outline
2
Process Communication & 
Synchronization
Process Scheduling
Scheduler
Context-
switching
ProcessProcessProcess
Kernel Space
User Space
P
P
P
Scheduling 
Alg
Process 
lifecycle

6
%&'()
v
通过多道程序设计获得的最大
CPU利用率
v
CPU–I/O执行周期–进程执行
由CPU执行周期和I/O等待周期
组成
v
CPU执行，然后是I/O执行
v
CPU执行分布是主要关注的问
题

7
ISCA ’19, June 22-26, 2019, Phoenix, AZ, USAA. Sriraman et al.
28
10
28
34
RunningQueue latency
Scheduler latencyIO latency
(a)(b)
90
62
69
95
28
10
38
31
5
72
050100
Ads2
Ads1
Feed2
Feed1
Web
Request latency breakdown (%)
Running (%)Blocked (%)
Figure 2: (a) A single request’s latency breakdown for eachμservice:
fewμservices block for a long time, (b) Web’s request latency
breakdown: thread over-subscription causes scheduling delays.
0
50
100
WebFeed1Feed2Ads1Ads2Cache1Cache2
CPU util. (%)
UserKernel & IO
Figure 3: Max. achievable CPU utilization in user- and kernel-mode
acrossμservices: utilization can be low to avoid QoS violations.
tions) vs. “blocked” (stalled, e.g., on I/O) in Fig.2(a). We
omitCache1andCache2from this measurement since their
queries follow concurrent execution paths and time cannot
easily be apportioned as “running” or “blocked”.
Feed1andAds2are almost entirely compute-bound through-
out a request’s life as they are leaves and do not block on re-
quests to other microservices in the common case. They will
benefit directly from architectural features that enhance in-
struction throughput. In contrast,Web,Feed2, andAds1emit
requests to other microservices and hence their queries spend
considerable time blocked. These can benefit from architec-
tural/OS features that support greater concurrency [11,49],
fast thread switching, and better I/O performance [50,51].
We further break downWeb’s “blocked” component in
Fig.2(b) into queuing latency (while a query awaits a worker
thread’s availability), scheduler latency (where a worker is
ready but not running), and I/O latency (where a query is
blocked on a request to another microservice). Although
Web’s scheduler delays are surprisingly high, these delays are
not due to inefficient system design, and are instead triggered
by thread over-subscription. To improveWeb’s throughput,
load balancing schemes continue spawning worker threads
until adding another worker begins degrading throughput.
2.3.3 CPU utilization at peak load.The microservices
also vary in their CPU utilization profile. Fig.3shows the
CPU utilization and its user- and kernel-mode breakdown
when each microservice is operated at the maximum load
it can sustain without violating Quality of Service (QoS)
constraints. We make two observations: (1) CPU resources
are not always fully utilized. (2) Most microservices ex-
hibit a relatively small fraction of kernel/IO wait utilization.
Each microservice faces latency, quality, and reliability con-
straints, which impose QoS requirements that in turn impose
constraints on how high CPU utilization may rise before a
constraint is violated. Our load balancers modulate load to en-
sure constraints are met. More specifically,Cache1,Cache2,
Feed1,Feed2,Ads1, andAds2under-utilize the CPU due to
strict latency constraints enforced to maintain user experience.
These services might benefit from tail latency optimizations,
0
10
20
30
WebFeed1Feed2Ads1Ads2Cache1Cache2
Context switch 
penalty range (%)
Figure 4: Fraction of a second spent context switching (range):
Cache1 & Cache2 can benefit from context switch optimizations.
29
15
24
9
18
22
5
19
23
24
17
21
18
19
16
18
17
7
20
0
0
0
0
0
0
0
0
0
0
0
0
0
0
6
12
2
45
0
31
39
30
41
51
44
37
42
31
36
43
38
36
38
38
34
41
4
36
31
34
29
38
28
24
43
26
35
21
30
27
28
27
26
27
27
34
27
8
11
16
12
3
9
15
13
11
18
10
13
18
17
13
10
14
10
17
020406080100
483.xalancbmk
473.astar
471.omnetpp
464.h264ref
462.libquantum
458.sjeng
456.hmmer
445.gobmk
429.mcf
403.gcc
401.bzip2
400.perlbench
Cache2
Cache1
Ads2
Ads1
Feed2
Feed1
Web
SPEC2006
Our microservices
%
Branch (%)Floating point (%)Arithmetic (%)Load (%)Store (%)
Figure 5: Instruction type breakdown acrossμservices: instruction
mix ratios vary substantially acrossμservices.
which might allow them to operate at higher CPU utilization.
Cache1andCache2exhibit higher kernel-mode utilization
due to frequent context switches, which we inspect next.
2.3.4 Context switch penalty.We report the fraction of
a CPU-second each microservice spends context switching
in Fig.4. We estimate context switch penalty by first aggre-
gating non-voluntary and voluntary context switch counts
reported by Linux’stimeutility. We then estimate upper
and lower context switch penalty bounds using switching
latencies reported by prior works [52,53].
Cache1andCache2incur context switches far more fre-
quently than other microservices, and may spend as much
as 18% of CPU time in switching. These frequent context
switches also lead to worse cache locality, as we will show
in our architectural characterization. Software/hardware op-
timizations [54–62] that reduce context switch latency or
counts might considerably improveCacheperformance.
2.3.5 Instruction mix.We report our microservices’ in-
struction mix and contrast with SPEC CPU2006 benchmarks
in Fig.5. Instruction mix varies substantially across our mi-
croservices, especially with respect to store-intensity and the
presence/absence of floating-point operations. The microser-
vices that include ranking models that operate on real-valued
feature vectors,Ads1,Ads2,Feed1, andFeed2, all include
floating-point operations, andFeed1is dominated by them.
These microservices can likely benefit from optimizations for
dense computation, such as SIMD instructions.
Prior work has reported that key-value stores, likeCache1
andCache2, are typically memory intensive [16]. However,
*+,-./0123

8
45678()
v
进程执行包括了CPU执行和I/O执行；
v
CPU执行时间与频率之间的关系形成指数或者超指数分布；
v
从分布上看，有大量短CPU执行和少量长CPU执行；
v
I/O密集型程序通常是大量短CPU执行，CPU密集型程序可能只
有少量长CPU执行。
Why scheduling is needed
•Process execution
–Consists of a cycle of CPU execution and I/O wait
–CPU burst + I/O burst
3
CPU burst duration

9
45678()
Why scheduling is needed
4
A system may contain many processes which are at different 
states (ready for running, waiting for I/O)
Multiprogramming
Question.How to improve CPU 
utilization (CPU is much faster than I/O)?
Question.How to improve system 
responsiveness (interactive applications)?
Multitasking
Scheduling is required because the number of  computing 
resource –the CPU –is limited.
Demo

10
!"#$%&'

11
9:;<1=)>?@
Programmer’s point of view...
•This is how a fresh programmer looks at a process’ 
life cycle.
7
Running
Waiting
for results
Termination
Process
States
intmain(void) {
intx = 1;
getchar();
return x;
}
(1)
(2)
(3)

12
9:AB1=)>?@
Kernel’s point of view...
8
New
(Justfork()-ed)
Waiting
(blocked)
Terminated 
(Zombie)
Process
States
ReadyRunning
Big Picture

13
9:AB1=)>?@
Kernel’s point of view...
9
Process
States
ReadyRunning
The birth of a process.
Except the first process “init”, 
every process is created using 
fork().
New
Waiting
(blocked)
Terminated 

14
9:AB1=)>?@
Kernel’s point of view...
10
Process
States
ReadyRunning
New
Waiting
(blocked)
Terminated 
The process is ready.
It means it is ready to runbut is not 
running.
A process may become “ready” after...
-it is just created by fork();
-it has been running on the CPU 
for some time and the OS chooses 
another process to run;
-returning from blocked states.
All ready processes are kept on a list 
called ready queue

15
9:AB1=)>?@
Kernel’s point of view...
11
New
Waiting
Terminated 
Process
States
ReadyRunning
Big Picture
The process is running.
The OS chooses this process to be 
running on the CPU and changes 
its state to “Running”.

16
9:AB1=)>?@
Kernel’s point of view...
12
New
Waiting
Terminated 
Process
States
ReadyRunning
Big Picture
The process is blocked.
While the process is running, it 
may be waiting for something
and becomes blocked voluntarily.

17
9:AB1=)>?@
Kernel’s point of view...
13
New
Waiting 
(interruptible)
Terminated 
Process
States
ReadyRunning
Big Picture
Example.  Reading a file.
Sometimes, the process has to wait for the response from the device and, 
therefore, it is blocked
.
Nevertheless, this blocking state is interruptible. E.g., “Ctrl + C” can get 
the process out of the waiting state (but goes to termination state 
instead).

18
9:AB1=)>?@
Kernel’s point of view...
14
New
Waiting
(uninterruptible)
Terminated 
Process
States
ReadyRunning
Big Picture
Sometimes, a process needs to wait for a resource but it doesn’t want to 
be disturbed while it is waiting. In other words, 
the process wants that 
resource very much
. Then, the process status is set to the uninterruptible
status.

19
9:AB1=)>?@
Kernel’s point of view...
15
New
Waiting
Terminated 
Process
States
ReadyRunning
Big Picture
Return back to ready.
When response arrives, the status of the process changes back to Ready. 
from any one of the blocked states.
Process
data

20
9:AB1=)>?@
Kernel’s point of view...
16
New
Waiting
Terminated 
Process
States
ReadyRunning
Big Picture
The process is going to die.
The process may
-choose to terminate itself; or
-force to be terminated.

21
9:AB1=)>?@
What is scheduling?
17
Running
So, what is process scheduling?
Mainly about how to make all the ready 
processes become “Running” 
This is the called short-term scheduling 
or CPU scheduling.
Ready

22
()1CD
 
Long-term scheduling The decision to add to the pool of processes to be 
executed 
 
Medium-term scheduling The decision to add to the number of processes that 
are partially or fully in main memory 
 
Short-term scheduling The decision as to which available process will be 
executed by the processor 
 
I/O scheduling The decision as to which process's pending I/O 
request shall be handled by an available I/O device 
 
!"#$%&'()*+,!"-.%/01234
当CPU空闲时，操作系统同应从就绪队列中选择一个进程来执行。进程
选择
短期调度程序
或CPU调度程序。

23
%&'()
v
567#$%8%9:;<=8>?@A12BCDEB
567FEG%HIJKLMNOPQRSTUR567
V+,W
v
NXYRZ[%-.\?#$%&]()*+,!"-.
%/012G
^M#
$
_M#
$
`M#
$

24
Figure 9.1    Scheduling and Process State Transitions
Ready/
Suspend
New
RunningExit
Blocked
Long-term
scheduling
Long-term
scheduling
Medium-term
scheduling
Medium-term
scheduling
Short-term
scheduling
Ready
Blocked/
Suspend
?@EF

25
()GH
Running
Ready
Blocked
Short Term
Medium Term
Long Term
Blocked,
Suspend
Ready,
Suspend
NewExit
Figure 9.2    Levels of Scheduling

26
()IJ
Figure 9.3    Queuing Diagram for Scheduling
Event Wait
Time-out
Release
Ready Queue
Short-term
scheduling
Medium-term
scheduling
Medium-term
scheduling
Interactive
users
Batch
jobs
Processor
Ready, Suspend Queue
Event
Occurs
Blocked, Suspend Queue
Blocked Queue
Long-term
scheduling

27
K@()
v
abcQRMdLe
<=_56W
v
fg*<=%hi$W
•jk%LMlUJ
mRLM%+,
12%nN3l
oW
•^M#$Mdp
.q
rg<=%
hi$
JX*s
tu%LMvw
:x%yzW
可以从队列中创
建进程，但必须
决定：
操作系统何时可以承
担一个或多个额外的
进程
要接受哪些作业
并将其转化为进
程
先来先服务
按照优先级、期
望的执行时间等

28
L@()
v
_M#$9{|-.%Q}NJ
v
~%ÄÅÇJ|eÉÑÖÜáààâäãåâäçébèéêë6
<=hi$%íìî
•ïñë69QRóòJ|eéôöõú|ùLM
%ïñíìW

29
M@()
v
û'XNüMd?†âÖàá°¢£§•G
v
+,¶Eß®
ÉÉÉ©™¶ ́M$\^M ̈_M ̈`MW
v
≠aébÇÆ+,cQRLMW
v
Ø∞tuLM±≤S≥¥tuμ,LM%∂∑i∏1J
#π`M#$MdW
∂∑∫ª\
•时钟中断；
•I/O 中断；
•操作系统调用
•信号(e.g., 信号量)

30
NO()1PQ
Triggering Events
•When process scheduling happens:
18
A new process is 
created.
When “fork()” is invoked and returns successfully.
Then, whether the parentor the childis scheduled is up to the 
scheduler’s decision.
An existing process 
is terminated.
The CPU is freed. The scheduler should choose another process to run.
A process waitsfor 
I/O.
The CPU is freed. The scheduler should choose another process to run.
A process finishes 
waiting for I/O.
The interrupthandling routine makes a scheduling request, if 
necessary.

31
R81ST
Key Issues
19
Running
Question #1: How to make a ready process 
become running? (Note that the running 
process may not terminate at that time)
Context switching
Ready
Question #2: How to decide which process should be running? 
Scheduling criteria & scheduling algorithms
Question #3: How to design scheduling in a real/specific system? 
Multiprocessor system, real-time system, algorithm eveluation
上下文切换
调度标准和算法
实时调度

32
*+,()@U
v
CPU调度器从就绪队列中的进程中进行选择，并将CPU核心分配给
其中一个进程
§队列可以以各种方式排序
v
CPU调度决策可能在以下情况下发生：
1.从运行状态切换到等待状态（I/O请求或wait（）调用）
2.从运行状态切换到就绪状态（出现中断）
3.从等待切换到就绪 （I/O完成）
4.终止
v
对于情况1和4，在调度方面没有选择。必须选择一个新进程（如
果就绪队列中存在一个）来执行。
v
然而，对于情况2和3，有其他选择。

33
VWXYZVWX()
v当调度仅在情况1和4下发生时，调度方案是非抢占的；
v否则，是抢占式调度；
v在非抢占式调度下，一旦CPU分配给进程，进程将保持CPU，
直到通过终止或切换到等待状态释放CPU为止；
v几乎所有现代操作系统，包括Windows、MacOS、Linux和UNIX
，都使用抢占式调度算法；

34
VWXYZVWX()
v当数据在多个进程之间共享时，抢占式调度可能导致竞争条
件;
v考虑两个共享数据的进程的情况。当一个进程更新数据时，
它被抢占，以便第二个进程可以运行。然后，第二个进程可
能读取处于不一致状态的数据;
v第6章将详细探讨这个问题;
v抢占也影响操作系统内核的设计, 在上下文切换之前，等待
系统调用的完成，或者等待I/O阻塞的发生。确保内核简单，
但是难以应用于实时系统中。

35
()@U
v
调度程序模块向CPU调度程
序选择的进程,提供CPU控
制；这包括：
§切换上下文；
§切换到用户模式；
§跳转到用户程序中的正确位
置以重新启动该程序；
v
Dispatch latency–调度
器停止一个进程并启动另
一个运行进程所需的时间

36
()[\
Scheduling Criteria
•How to choose which algorithm to use in a 
particular situation?
27
Algorithm Properties
CPU utilization
Response time
Throughput 
Turnaround time
Waiting time
Types
Preemptive
Nonpreemptive
Application
Multiprocessor
Real-time sys
Application requirements and algorithm 
properties may vary significantly

37
()[\
30
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
In algorithm design:
What factors/performance measures 
should be carefully considered?
Waiting 
time
Response 
time

38
()[\
31
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
CPU utilization.
We want to keep CPU as busy as possible.
Theoretically, can range from 0-100%, but in 
real system, range from 40%-90%
The higher the better
Waiting 
time
Response 
time
CPU的利用率

39
()[\
32
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
Throughput.
Number of processes that are completed per 
time unit
The higher the better
Waiting 
time
Response 
time
吞吐量

40
()[\
33
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
Turnaround time.
Time to execute the process: interval from 
the time of submission to the time of 
completion (total running time + waiting 
time+ doing I/O)
The lower the better
Waiting 
time
Response 
time
周转时间

41
34
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
Waiting time.
The time spent waiting in the ready queue
The lower the better
Waiting 
time
Response 
time
()[\
等待时间

42
()[\
35
Performance measures
CPU 
utilization
Throughput
Turnaround 
time
Response time.
The time from the submission of a request 
until the first response is produced (useful 
measure for interactive systems)
The lower the better
Waiting 
time
Response 
time
响应时间

43
]^_\
v
最大CPU利用率
v
最大吞吐量
v
最小周转时间
v
最小等待时间
v
最小响应时间
v
大多数情况下，优化的是平均值；有些情况下优化的
是最小值或最大值，也可以最小化方差；

44
`aST
36
Challenge
Question: 
Can we optimize all the above 
measures simultaneously?
Usually can not!
Fairness
Policy 
enforcement
CPU-I/O 
Balance
Little conflict
Big 
conflict
Big 
conflict
Design 
Tradeoff
Common 
goal

45
()bc
•Inputs to the algorithms.
Scheduling algorithms
38
P1
P2P3P4
A set of 
processes
For each 
process...
Arrival 
Time
CPU 
requirement
It is interesting to note that 
this is a non-sense!
How can we know the 
requirement of each task?
Online
VS
Offline
An offline scheduling algorithmassumes that you know all the 
processes submitted to the system before hand. But, an online 
scheduling algorithmdoes not have such an assumption.
Yet, every real scheduler has to work in an “online scenario”. So, we 
have to think in an “online” way...
算法的输入
离线调度算法
在线调度算法

46
()bc
算法的输出
•Outputs of the algorithms.
Scheduling algorithms
39
Scheduling 
order
Individual & average 
turnaround time
Individual & average 
waiting time
Number of context 
switching

47
()bc
40
Different algorithms
AlgorithmsPreemptive?Target System
First-come,first-served 
or First-in, First-out 
(FIFO)
No.Out-of-date
Shortest-job-first (SJF)Can be both.Out-of-date
Round-robin (RR)Yes.Modern
PriorityschedulingYes.Modern
Priorityscheduling
withmultiple queues.
The realimplementation!
Demo

48
()bcde*efghihjk()
先请求CPU的进程首先分配到CPU，通过FIFO队列容易实现;
P1
First-come, first-served scheduling
•Example 1.
41
TaskArrival 
Time
CPU 
Req.
P1024
P213
P323
Gantt Chart
P2P3
Input
0
24
6
81
0
1
2
1
4
1
6
1
8
2
2
2
4
2
6
2
8
3
0
2
0
Output
Waiting time: P1 = 0; P2 = 23; P3 = 25;
Average waiting time = (0+23+25)/3 = 16;
Turnaround time: P1 = 24; P2 = 26; P3 = 28;
Average turnaround time = (24+26+28)/3 = 26;
No preemption

49
()bcde*efghihjk()
P3
First-come, first-served scheduling
•Example 2.
42
TaskArrival 
Time
CPU 
Req.
P303
P213
P1224
Gantt Chart
P2P1
Input order 
changed
0
24
6
81
0
1
2
1
4
1
6
1
8
2
2
2
4
2
6
2
8
3
0
2
0
Output
Waiting time: P1 = 4; P2 = 2; P3 = 0;
Average waiting time = (4+2+0)/3 = 2;
(which is 16 in the previous case)
Turnaround time: P1 = 28; P2 = 5; P3 = 3;
Average turnaround time = (28+5+3)/3 = 12;
(which is 26 in the previous case)

50
()bcde*efghihjk()
FCFS调度器总结：
   —— FCFS是非抢占的，进程得到CPU后会一直占用CPU直到
终止或者请求I/O；
   —— FCFS调度器对输入敏感,即到达的顺序；
   —— FCFS调度器的等待时间过长，特别是在动态情况下
FCFS的调度性能较差(出现护航效应)；
   —— 是否可以做一些改变？

51
()bcdflegmMno]h()
44
Different algorithms
AlgorithmsPreemptive?Target System
First-come,first-served 
or First-in, First-out 
(FIFO)
No.Out-of-date
Shortest-job-first (SJF)Can be both.Out-of-date
Round-robin (RR)Yes.Modern
PriorityschedulingYes.Modern
Priorityscheduling
withmultiple queues.
The realimplementation!

52
ZVW1fle
Non-preemptive SJF
45
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P1P1P1
P1
Set of processes
P2P3P4
Time = 0Time = 2Time = 4Time = 5
0
24
6
81
0
1
2
1
4
1
6
Not allow  preemption
当一个作业的优先级比当前进程的优先级高时，不抢占当前进程，而是允许其继续执
行

53
ZVW1fle
Non-preemptive SJF
46
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P2P3P4
Time = 7
P1
Set of processes
0
24
6
81
0
1
2
1
4
1
6

54
ZVW1fle
Non-preemptive SJF
47
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P2P3P4
Time = 7
P1P3
Set of processes
0
24
6
81
0
1
2
1
4
1
6

55
ZVW1fle
Non-preemptive SJF
48
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P2P3P4
Time = 8
P1P2P3
Set of processes
In this example, we use FIFOto break the tie.
0
24
6
81
0
1
2
1
4
1
6

56
ZVW1fle
Non-preemptive SJF
49
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P2P3P4
Time = 12
P1P4P3
Set of processes
P2
0
24
6
81
0
1
2
1
4
1
6
Time = 16
P4

57
Non-preemptive SJF
50
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
P1P4P3P2
0
24
6
81
0
1
2
1
4
1
6
P4
Waiting time:
Average = (0 + 6 + 3 + 7) / 4 = 4.
P1 = 0; P2 = 6; P3 = 3; P4 = 7;
Turnaround time:
Average = (7 + 10 + 4 + 11) / 4 = 8.
P1 = 7; P2 = 10; P3 = 4; P4 = 11;
ZVW1fle

58
VW1fle
Preemptive SJF
51
0
24
6
81
0
1
2
1
4
1
6
Rules for preemptive scheduling
(for this example only)
-Preemption happens when a new process arrives at 
the system.
-Then, the scheduler steps in and selects the next 
task based on their remaining CPU requirements.
TaskArrival 
Time
CPU Req.
Initial & Remain
P1077
P2244
P3411
P4544
Shortest-remaining-time-first
当一个作业的优先级比当前进程的优先级高时，抢占当前进程，

59
Preemptive SJF
52
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1077
P2244
P3411
P4544
P1
Set of processes
Time = 0
P1
VW1fle

60
VW1fle
Preemptive SJF
53
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2244
P3411
P4544
Set of processes
P2
Time = 2
P2
Preempted!
P1
P2 is selected!
P1P2

61
VW1fle
Preemptive SJF
54
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2242
P3411
P4544
Set of processes
P2P3
Time = 4
P3
Preempted!
P1
P3 is selected!
P1P3
P2

62
VW1fle
Preemptive SJF
55
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2242
P3410
P4544
Set of processes
P2P3
Time = 5
P2
Preempted!
P1
P2 is selected!
P1
P2P3
P4P2

63
VW1fle
Preemptive SJF
56
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2240
P3410
P4544
Set of processes
P2P3
Time = 7
P4P1
P1
P2P3
P4P4
P2
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2240
P3410
P4540
Time = 11
P1P4
TaskArrival 
Time
CPU Req.
Initial & Remain
P1070
P2240
P3410
P4540
P1
Time = 16
P4P1P1

64
VW1fle
Preemptive SJF
57
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2240
P3410
P4544
P4P1P2P3P2
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2240
P3410
P4540
P1P4
TaskArrival 
Time
CPU Req.
Initial & Remain
P1070
P2240
P3410
P4540
P1
Waiting time:
Average = (9 + 1 + 0 + 2) / 4 = 3.
P1 = 9; P2 = 1; P3 = 0; P4 = 2;
Turnaround time:
Average = (16 + 5 + 1 + 6) / 4 = 7.
P1 = 16; P2 = 5; P3 = 1; P4 = 6;

65
()bcdflegmMno]h()
SJF: Short summary
Non-preemptive SJFPreemptive SJF
Average waitingtime43 (smallest)
Average turnaroundtime87 (smallest)
# ofcontext switching3 (smallest)5
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
The waiting time and the turnaround time decrease 
at the expense of the increased number of context 
switching
.
平均等待时间和平均周转时间降低，但是
代价是增加了上下文切换的成本；

66
()bcdflegmMno]h()
SJF: Short summary
Non-preemptive SJFPreemptive SJF
Average waitingtime43 (smallest)
Average turnaroundtime87 (smallest)
# ofcontext switching3 (smallest)5
TaskArrival 
Time
CPU 
Req.
P107
P224
P341
P454
SJF is provably optimal in that it gives the minimum 
average waiting time
Challenge: How to know the length of the next CPU 
request?
可以证明SJF调度算法是最优的，因为SJF
的平均等待时间最小；但是，如何知道下
次CPU执行的长度？

67
()bcdflegmMno]h()
SJF: Short summary
Challenge: How to know the length of the next CPU 
request? 
Solution: Prediction (by expecting that the next CPU 
burst will be similar in length to the previous ones)
General approach
exponential average
Most recent information
Predicted
value

v
RR专门为分时系统设计，类似FCFS但是增加了抢占以切换进程。
v
RR是抢占式调度的；
v
每个进程给定一个较小的时间单位成为时间片，时间片用完后，
CPU选择另外一个进程调度执行；
v
CPU调度程序循环整个就绪队列，一个一个执行；
68
()bcdppgqr()

69
()bcdppgqr()
v
每个进程获得一个小的CPU时间单位（时间量和时间片），通常为
10-100毫秒。此时间过后，进程将被抢占并添加到就绪队列的末
尾。
v
如果就绪队列中有n个进程，且时间片为q，则每个进程一次最多
以q个时间单位的块获取1/n的CPU时间。没有进程等待超过（n-1
）个时间单位。
v
计时器中断每个时间片以安排下一个进程
v
性能
§q如果非常大， 变成先进先出（FCFS）；
§q如果非常小，导致大量的上下文切换，相对于上下文切换，q
必须很大，否则开销太高

70
()bcdppgqr()
Round-robin
63
Rules for Round-Robin
(for this example only)
-The quantum of every process is fixed and is 2 units.
-The process queue is sorted according the processes’ 
arrival time, in an ascending order.
(This rule allows us to break tie.)
TaskArrival 
Time
CPU Req.
Initial & Remain
P1077
P2244
P3411
P4544

71
Round-robin
64
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1077
P2244
P3411
P4544
Set of processes
P1
Q:2
Time = 0
P1
()bcdppgqr()

72
Round-robin
65
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2244
P3411
P4544
Set of processes
Time = 2
P2P1
P1
Q:0
P2
Q:2
P1’s quantum is 0;
P2 is selected!
()bcdppgqr()

73
()bcdppgqr()
Round-robin
66
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2242
P3411
P4544
Set of processes
Time = 4
P3P1
P1
Q:0
P2
Q:0
P1’s & P2’s quanta are 0;
P3 is selected!
P2
P3
Q:2

74
()bcdppgqr()
Round-robin
67
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2242
P3410
P4542
Set of processes
Time = 5
P4P1
P1
Q:0
P2
Q:0
P1’s & P2’s quanta are 0;
P4 is selected!
P2
P3
Q:2
P3
P4
Q:2

75
()bcdppgqr()
Round-robin
68
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1075
P2242
P3410
P4542
Set of processes
Time = 7
P1P1
P1
Q:0
P2
Q:0
Now, recharge is needed.
P2
P3
Q:2
P3
P4
Q:0
P4
P1
Q:2
P2
Q:2
P4
Q:2
Now, recharge is needed.
P1 is selected.

76
Round-robin
69
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1073
P2242
P3410
P4542
Set of processes
Time = 9
P2P1
P1
Q:0
P2
Q:0
P1’s quantum is 0;
P2 is selected!
P2
P3
Q:2
P3
P4
Q:0
P4
P1
Q:0
P2
Q:2
P4
Q:2
P1
()bcdppgqr()

77
()bcdppgqr()
Round-robin
70
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1073
P2240
P3410
P4542
Set of processes
Time = 11
P4P1
P1
Q:0
P2
Q:0
P1’s quantum is 0;
P4 is selected!
P2
P3
Q:2
P3
P4
Q:0
P4
P1
Q:0
P2
Q:2
P4
Q:2
P1P2

78
()bcdppgqr()
Round-robin
71
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1073
P2240
P3410
P4540
Set of processes
Time = 13
P1P1
P1
Q:0
P2
Q:0
P2
P3
Q:2
P3
P4
Q:0
P4
P1
Q:0
P2
Q:2
P4
Q:2
P1P2P4
P1
Q:2
Now, recharge is needed.Now, recharge is needed.
P1 is selected.

79
()bcdppgqr()
Round-robin
73
0
24
6
81
0
1
2
1
4
1
6
TaskArrival 
Time
CPU Req.
Initial & Remain
P1070
P2240
P3410
P4540
P1P2P3P4P1P2P4P1P1
Waiting time:
Average = (9 + 5 + 0 + 4) / 4 = 4.5
P1 = 9; P2 = 5; P3 = 0; P4 = 4;
Turnaround time:
Average = (16 + 9 + 1 + 8) / 4 = 8.5
P1 = 16; P2 = 9; P3 = 1; P4 = 8;

80
ppsfletu
RR  VS  SJF
74
Non-preemptive 
SJF
Preemptive SJFRR
Average waitingtime434.5 (largest)
Average turnaround
time
878.5 (largest)
# ofcontext switching358 (largest)
So, the RR algorithm gets all the bad!  Why do we still need it?
The responsiveness of the processes is great under the RR algorithm. E.g., you 
won’t feel a job is “frozen” because every job is on the CPU from time to time!

81
pp1vw
RR算法的性能很大程度取决于时间片的大小：
  —— 时间片太大，RR算法与FCFS算法一样；
  —— 如果时间片太小，产生大量的上下文切换，反而使
程序的性能降低；
时间片大小不同时上下文切换情况

v
时间片要远大于上下文切换的时间，让上下文切换的时间占
时间片的一小部分；
v
随着时间片大小的增加，一组进程的平均周转时间不一定会
改善。
82
pp1vw
80%的CPU执行时间
应小于时间片

83
]hx()
v
优先级编号（整数）与每个进程相关联；
v
如何定义优先级：内部(时限、内存需求)和外部定义（重
要性、所有者等）；
v
CPU分配给具有最高优先级（最小整数）的进程（最高优先
级）
§抢占式的
§非抢占式
v
SJF是优先级调度，其中优先级是预测的下一个CPU执行时
间的倒数；
v
问题：饥饿–低优先级进程可能永远不会执行；
v
解决方案:老化–随着时间的推移，进程的优先级会增加；

84
]hx()
Priority Scheduling
79
0
24
6
81
0
1
2
1
4
1
6
TaskCPU 
Burst
Priority
P173
P211
P324
P415
P552
P2P5
Assumption:
-All arrive at time 0
-Low numbers represent high priority
P1
P3P4
Problem:
Solution:Aging (gradually increase the priority of 
waiting processes)
Indefinite blocking or starvation

85
yxIJ()
v
仍然是优先级调度器；
v
就绪作业队列分成多个单独的队列，每个队列具有不同的
调度算法；
v
分成前台进程（RR）和后台进程（FCFS）；
The processes are 
permanently assigned to 
one queue
Multilevel queue scheduling
•Definitions.
–It is still a priority scheduler.
–But, at each priority class, different schedulersmay be 
deployed.
–Eg: Foreground processes and background processes
81
Priority class 2
Priority class 3
Priority class 4
Priority class 5
RR with quantum = 20 units.
RR with quantum = 10 units.
Non-preemptive, SJF
Non-preemptive, FIFO
Priority class 1RR with quantum = 40 units.
Just an example.
Fixed-priority preemptive 
scheduling among queues

86
yxIJ()
不同的队列之间应用调度，通常采用固定优先级抢占调度，如
前台队列可以比后台队列具有绝对的优先；

87
yxIJ()
另一种方式是队列之间划分时间片，每个队列都有一定比例的
CPU时间，可用于调度队列内的进程；

88
yxIJ()
Multilevel queue scheduling–an example
•Properties: process is assigned a fix priority when 
they are submitted to the system.
82
Priority 1
Priority 2
Priority 3
Priority 4
Increasing priority
E.g., using round-robin in each queue.

89
yxIJ()
Multilevel queue scheduling–an example
•The highest priority class will be selected.
–To prevent high-priority tasks from running indefinitely.
–The tasks with a higher priority should be short-lived, but 
important
;
83
Priority class 1
Priority class 2
Priority class 3
Priority class 4
E.g., using round-robin in each queue.
Increasing priority

90
yxIJ()
Multilevel queue scheduling–an example
•Lower priority classes will be scheduled only when 
the upper priority classes has no tasks.
84
Priority class 1
Priority class 2
Priority class 3
Priority class 4
E.g., using round-robin in each queue.
Increasing priority

91
yxIJ()
Multilevel queue scheduling–an example
•Of course, it is a good design to have a high-priority 
task preempting a low-priority task.
(conditioned that the high-priority task is short-lived.)
85
Priority class 1
Priority class 2
Priority class 3
Priority class 4
E.g., using round-robin in each queue.
Increasing priority

92
yxIJ()
Multilevel queue scheduling–an example
86
Priority class 1
Priority class 2
Priority class 3
Priority class 4
E.g., using round-robin in each queue.
Increasing priority
•Any problem?
–Fixed priority
–Indefinite blocking or starvation

93
A process drops to a 
lower priority class 
after it has used up its 
quantum and has the 
quantum recharged.
Multilevel feedback queue scheduling
•How to improve the previous scheme?
–Allows a process to move between queues (dynamic 
priority).
–Why needed? 
•Eg.: Separate processes according to their CPU bursts.
87
Priority class 2
Priority class 3
Priority class 4
Priority class 5
RR with quantum = 20 units.
RR with quantum = 10 units.
Non-preemptive, SJF
Non-preemptive, FIFO
Priority class 1RR with quantum = 40 units.
Just an example.
yxz{()IJ

94
yxz{()IJ|}
v
三个队列：
§Q0–RR时间片为8毫秒
§Q1–RR时间片16毫秒
§Q2–FCFS
v
进程调度
§一个新进程进入队列Q0，该队列在RR中
提供服务
•当它获得CPU时，进程接收8毫秒;
•如果进程未在8毫秒内完成，则进程
将移动到队列Q1;
§在Q1，作业再次在RR中服务，并接收额
外的16毫秒;
•如果仍然没有完成，它将被抢占并移
动到队列Q2
l短时间的进程优先级最高；
l长时间的进程自动降低优
先级；

95
yxz{()IJ
n通常，多级反馈队列调度程序可由以下参数定义：
Ø队列数量；
Ø每个队列的调度算法；
Ø用于确定何时升级到高优先级队列的方法；
Ø用于确定何时降级到更低优先级队列的方法；
Ø用于确定当某个进程需要服务时该进程将进入哪个
队列的方法；
n成为最通用的调度算法，通过配置适应特定的系统，也
是最复杂的调度方法；
n老化可以使用多级反馈队列来实现；

96
~@()
v
用户级线程和内核级线程之间的区别;
v
如果支持线程，则调度线程，而不是进程;
v
多对一和多对多模型，线程库安排用户级线程在LWP上运
行
§称为进程级争用范围（PCS），因为调度竞争在进程内
§通常通过程序员设置的优先级来完成;
v
调度到可用CPU上的内核线程是系统级争用范围（SCS）—
—系统中所有线程之间的竞争；
v
对于一对一模型的系统，如Windows、Linux、Solaris等
只采用SCS调度；
v
通常情况下，PCS采用优先级调度；

97
+ÄÅÇÉÑÖ()
v
API允许在创建线程期间指定PCS或SCS：
§PTHREAD_SCOPE_PROCESS：使用PCS调度来调度线程；
§PTHREAD_SCOPE_SYSTEM：使用SCS调度来调度线程；
v
可受操作系统限制–Linux和macOS仅允许
PTHREAD_SCOPE_SYSTEM

98
+ÄÅÇÉÑÖ()
218Chapter 5CPU Scheduling
kernel-level thread to schedule onto aCPU,thekernelusessystem-contention
scope(
SCS). Competition for theCPUwithSCSscheduling takes place among
all threads in the system. Systems using the one-to-one model (Section 4.3.2),
such as Windows and Linux schedule threads using only
SCS.
Typically,
PCSis done according to priority—the scheduler selects the
runnable thread with the highest priority to run. User-level thread priorities
are set by the programmer and are not adjusted by the thread library, although
some thread libraries may allow the programmer to change the priority of
athread.Itisimportanttonotethat
PCSwill typically preempt the thread
currently running in favor of a higher-priority thread; however, there is no
guarantee of time slicing (Section 5.3.3) among threads of equal priority.
5.4.2   Pthread Scheduling
We provided a samplePOSIXPthread program in Section 4.4.1, along with an
introduction to thread creation with Pthreads. Now, we highlight the
POSIX
PthreadAPIthat allows specifyingPCSorSCSduring thread creation. Pthreads
identi!es the following contention scope values:
•
PTHREADSCOPEPROCESSschedules threads usingPCSscheduling.
•
PTHREADSCOPESYSTEMschedules threads usingSCSscheduling.
On   systems   implementing   the   many-to-many   model,   the
PTHREADSCOPEPROCESSpolicy schedules user-level threads onto available
LWPs. The number ofLWPsismaintainedbythethreadlibrary,perhapsusing
scheduler activations (Section 4.6.5). The
PTHREADSCOPESYSTEMscheduling
policy will create and bind an
LWPfor each user-level thread on many-to-many
systems, effectively mapping threads using the one-to-one policy.
The Pthread
IPC(Interprocess Communication) provides two functions for
setting—and getting—the contention scope policy:
•
pthreadattrsetscope(pthreadattrt*attr,intscope)
•
pthreadattrgetscope(pthreadattrt*attr,int*scope)
The!rst parameter for both functions contains a pointer to the attribute set for
the thread. The second parameter for the
pthreadattrsetscope()function
is passed either the
PTHREADSCOPESYSTEMor thePTHREADSCOPEPROCESS
value, indicating how the contention scope is to be set. In the case of
pthreadattrgetscope(),thissecondparametercontainsapointertoan
intvalue that is set to the current value of the contention scope. If an error
occurs, each of these functions returns a nonzero value.
In Figure 5.10, we illustrate a Pthread scheduling
API.Thepro-
gram!rst  determines  the  existing  contention  scope  and  sets  it  to
PTHREADSCOPESYSTEM.Itthencreates!ve separate threads that will
run using the
SCSscheduling policy. Note that on some systems, only certain
contention scope values are allowed. For example, Linux and mac
OSsystems
allow only
PTHREADSCOPESYSTEM.
Pthreads IPC提供两个函数，用于获取或设置竞争范围策略：
5.4Thread Scheduling219
#include<pthread.h>
#include<stdio.h>
#define NUMTHREADS 5
int main(int argc, char *argv[])
{
int i, scope;
pthread
ttid[NUMTHREADS];
pthread
attrtattr;
/* get the default attributes */
pthread
attrinit(&attr);
/* first inquire on the current scope */
if (
pthreadattrgetscope(&attr, &scope) != 0)
fprintf(stderr, "Unable to get scheduling scope
!n");
else
{
if (scope == PTHREADSCOPEPROCESS)
printf("PTHREAD
SCOPEPROCESS");
else if (scope == PTHREAD
SCOPESYSTEM)
printf("PTHREAD
SCOPESYSTEM");
else
fprintf(stderr, "Illegal scope value.
!n");
}
/* set the scheduling algorithm to PCS or SCS */
pthreadattrsetscope(&attr, PTHREADSCOPESYSTEM);
/* create the threads */
for (i = 0; i < NUM
THREADS; i++)
pthread
create(&tid[i],&attr,runner,NULL);
/* now join on each thread */
for (i = 0; i < NUM
THREADS; i++)
pthread
join(tid[i], NULL);
}
/* Each thread will begin control in this function */
void *runner(void *param)
{
/* do some work ... */
pthread
exit(0);
}
Figure 5.10Pthread scheduling API.
5.4Thread Scheduling219
#include<pthread.h>
#include<stdio.h>
#define NUMTHREADS 5
int main(int argc, char *argv[])
{
int i, scope;
pthread
ttid[NUMTHREADS];
pthread
attrtattr;
/* get the default attributes */
pthread
attrinit(&attr);
/* first inquire on the current scope */
if (
pthreadattrgetscope(&attr, &scope) != 0)
fprintf(stderr, "Unable to get scheduling scope
!n");
else
{
if (scope == PTHREADSCOPEPROCESS)
printf("PTHREAD
SCOPEPROCESS");
else if (scope == PTHREAD
SCOPESYSTEM)
printf("PTHREAD
SCOPESYSTEM");
else
fprintf(stderr, "Illegal scope value.
!n");
}
/* set the scheduling algorithm to PCS or SCS */
pthreadattrsetscope(&attr, PTHREADSCOPESYSTEM);
/* create the threads */
for (i = 0; i < NUM
THREADS; i++)
pthread
create(&tid[i],&attr,runner,NULL);
/* now join on each thread */
for (i = 0; i < NUM
THREADS; i++)
pthread
join(tid[i], NULL);
}
/* Each thread will begin control in this function */
void *runner(void *param)
{
/* do some work ... */
pthread
exit(0);
}
Figure 5.10Pthread scheduling API.

99
!"#$
v
!"#$%&!"'()*+,-./01
v
2(#$3456789:;<=>?+,@/0ABC
+0!"1
v
DE/0ABF34564-G(HI@J$K
§
LM
NMOPQRSTUV@WXYZ1
§
[
NMOPQR\]@/0YZ1
§
^
NM+,_=I@`abYZFcdM[12(J$ef
g+0hi?jklmn1

100
!"opqr
 FCFS 
Round 
robin 
SPN SRT HRRN Feedback 
Selection 
function 
max[w] 
constant min[s] min[s – e] 
    
max
w+s
s
! 
" 
# 
$ 
% 
& 
 
(see text) 
Decision 
mode 
Non- 
preemptive 
Preemptive 
(at time 
quantum) 
Non- 
preemptive 
Preemptive 
(at arrival) 
Non- 
preemptive 
Preemptive 
(at time 
quantum) 
Through- 
Put 
Not 
emphasized 
May be 
low if 
quantum 
is too 
small 
High High High 
Not 
emphasized 
Response 
time 
May be 
high, 
especially if 
there is a 
large 
variance in 
process 
execution 
times 
Provides 
good 
response 
time for 
short 
processes 
Provides 
good 
response 
time for 
short 
processes 
Provides 
good 
response 
time 
Provides good 
response time 
Not 
emphasized 
Overhead Minimum Minimum Can  be high Can  be high Can  be high Can  be high 
Effect on 
processes 
Penalizes 
short 
processes; 
penalizes 
I/O bound 
processes 
Fair 
treatment 
Penalizes 
long 
processes 
Penalizes 
long 
processes 
Good balance 
May favor 
I/O bound 
processes 
Starvation No No Possible Possible No Possible 
 
Table 9.3  
Characteristic
s of Various 
Scheduling 
Policies
(Table can be 
found on page 
405 in 
textbook 操作
系统精髓与
设计原理)

101
%qst
§
uv!"#$wx/0
yZ@z{|}1
§
~QÄÅ
§ÇÉÑ
§ÉÑ

102
opqrÖÜ
First-Come-First
Served (FCFS)
05101520
05101520
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
A
B
C
D
E
Round-Robin
(RR), q = 1
Round-Robin
(RR), q = 4
Shortest Process
Next (SPN)
Shortest Remaining
Time (SRT)
Highest Response
Ratio Next (HRRN)
Feedback
q = 1
Feedback
q = 2
i
Figure 9.5   A Comparison of Scheduling Policies

103
BáÖÜ
vAny scheduling discipline that chooses the next item to be 
served independent of service time obeys the relationship:

104
0.1
1
2
3
4
5
6
7
8
9
10
0.20.30.40.5
Utilization (W)
2 priority classes
Q
1
 = Q
2
t
s2
 = 5   t
s1
Normalized response time (
T
r
1
/
T
s
1
)
Figure 9.12 Normalized Response Time for Shorter Processes
0.60.70.80.91.0
No priority
Priority
Priority
with preemption

105
0.1
1
2
3
4
5
6
7
8
9
10
0.20.30.40.5
Utilization (W)
2 priority classes
Q
1
 = Q
2
t
s2
 = 5   t
s1
Normalized response time (
T
r2
/
T
s2
)
Figure 9.13 Normalized Response Time for Longer Processes
0.60.70.80.91.0
No priority
Priority
Priority
with preemption

106
àâäãop
v
基于进程集合的计划决策；
v
每个用户都被分配了处理器的一部分；
v
目标是监控使用情况，为拥有超过其公平份额的用户
提供更少的资源，而为拥有低于其公平份额用户提供
更多的资源

107
Priority
Colored rectangle represents executing process
600
1
2


60
0
1
2


60
7415
16
17


75
15
16
17


75
7818
19
20


78
18
19
20


78
670
1
2


60
15
16
17


75
741515
16
17


75
600
1
2


60
0
1
2


60
6000
1
2


60
6000
903030
963737
98393970318761518
903030
81737933037
75030
6000
Process
CPU
count
Process A
Group 1Group 2
Process BProcess C
Group
CPU
count
Process
CPU
count
Group
CPU
count
Process
CPU
count
Group
CPU
countPriorityPriority
Time
0
1
2
3
4
5
Figure 9.16    Example of Fair Share Scheduler—Three Processes, Two Groups

108
åU@çéèêop
vUsed in both SVR3 and 4.3 BSD UNIX
•these systems are primarily targeted at the time-sharing 
interactive environment
•Designed to provide good response time for interactive users while 
ensuring that low-priority background jobs do not starve
•Employs multilevel feedback using round robin within each of the priority 
queues
•Makes use of one-second preemption
•Priority is based on process type and execution history

109
opë}í

110
opë}í
Priority
600
1
2


60
6715
7633
75306715
600
1
2


60
600
600
7530
637
8
9


67
68167633637
7530
637
8
9


67
6715
600
1
2


60
600
CPU count
Process AProcess BProcess C
PriorityCPU countPriorityCPU count
Time
0
1
2
3
4
5
Figure 9.17   Example of Traditional UNIX Process Scheduling
Colored rectangle represents executing process

v
Chrt  可以用于在Linux OS中查看调度相关的信息，也可以修
改进程的调度策略以及优先级；
Chrt –f –p 15 （优先级) 32273
111
ìî+,@opqr

112
ïz{ñop
Ø前面讲述的内容都是单处理器系统的调度问题，如果有多个处理
器，负载分配成为可能，但是调度问题更加复杂；
Ø主要关注同构处理器；
Ø多处理器调度：
  — 非对称多处理器；
  — 对称多处理器（SMP）；
Ø
对称多处理（SMP）是指每个处理器都是自调度的。
Ø
所有线程都可能位于公共就绪队列（a）中
Ø
每个处理器可能有自己的线程专用队列（b）

113
+,op
vUsually processes are not dedicated to processors
vA single queue is used for all processors
§if some sort of priority scheme is used, there are multiple 
queues based on priority
vSystem is viewed as being a multi-server queuing 
architecture

114
Single
processor
Dual
processor
SRT to FCFS throughput ratio
0
1.00
1.05
1.10
1.15
1.20
1.25
12345
Coefficient of variation
(a) Comparison of RR and FCFS
Coefficient of variation
(b) Comparison of SRT and FCFS
Figure 10.1  Comparison of Scheduling Performance for One and Two Processors
012
Single
processor
Dual
processor
0.98
RR to FCFS throughput ratio
345
1.00
1.05
1.10
1.15

115
fÜ+á1()ST
92
Scheduling Issues with SMP
Processor Affinity
Attempt to keep a 
process running on 
the same processor
Soft/hard affinity
Scheduling between processors
Process migration: Invalidating the cache 
of the first processor and repopulating 
the cache of the second processor)
Process migration is costly
NUMA
CPU scheduler and 
memory-placement 
algorithms work 
together
SMP: Each processor may have its private 
queue of ready processes
Load balancing
Push migration: a specific task 
periodically check the status & rebalance
Pull migration: an idle processor pulls a 
waiting task from busy processor
No absolute rule concerning what policy is best
处理器亲和性
Demo: taskset

116
yB%&'
v
将多个处理器核心放在同一物理芯片上的最新趋势;
v
速度更快，耗电更少；
v
每个核心的线程数量也在增长；
§在进行内存访问时，利用内存暂停，切换到执行；
v
内存停顿，memory stall

117
y~@yB%&'
v
每个核心都有>1个硬件线程。
v
如果一个线程发生内存暂停，切换到另一个线程！
v
如图

118
y~@yB%&'
v
芯片多线程（CMT）为每个核
心分配多个硬件线程。（英
特尔称之为超线程。）
v
在每个内核有2个硬件线程的
四核系统上，操作系统可以
看到8个逻辑处理器。

v
处理器器核的多线程有两种方法：
    — 细粒度： 多线程在更细粒度级别上（通常是指令
周期）切换线程，线程之间的切换很小；
    — 粗粒度：粗粒度线程，线程一直在处理器上执行，
直到一个长延迟时间如内存停顿发生，线程之间的切换成
本高；
v
多线程多核处理器需要两个级别的调度：一个级别由操
作系统调度；另一个级别是指定每个核心如何运行哪个
硬件线程；
119
y~@yB%&'

120
àâäã
vThe operating system, and in particular the scheduler, is 
perhaps the most important component
vCorrectness of the system depends not only on the logical result 
of the computation but also on the time at which the results are 
produced
vTasks or processes attempt to control or react to events that 
take place in the outside world
vThese events occur in “real time” and tasks must be able to 
keep up with them
•control of laboratory experiments
•process control in industrial plants
•robotics
•air traffic control
•telecommunications
•military command and control systems
Examples:

121
óòYôöòYTU
vHard real-time task
vone that must meet its 
deadline
votherwise it will cause 
unacceptable damage 
or a fatal error
（致
命）
to the system
vSoft real-time task
vhas an associated deadline 
that is desirable but not 
mandatory
vit still makes sense to 
schedule and complete the 
task even if it has passed its 
deadline

122
må^çé
v
实时系统的事件驱动性质；
v
事件发生时，系统应尽快地响
应和服务它；
v
Event latency(事件延迟)–
从事件发生到提供服务所经过
的时间量；
v
有两种类型的延迟会影响性能
1.中断延迟–从收到中断到
中断处理例程开始的时间；
2.Dispatch latency(调度延
迟)—从停止一个进程到启
动另一个进程所需的时间
量称为调度延迟，提供抢
占式内核；

123
Lèçé
对实时操作新系统而言，至关重要的是，尽量减少中断延迟，
以确保实时任务得到处理。

124
opõú
v
调度延迟的冲突阶段
：
1.在内核模式下运行
的任何进程的抢占；
2.低优先级进程释放
高优先级进程所需
的资源；
以Solaris为例，禁用抢占的调度延迟超过100ms，而启用抢
占的调度延迟不到1ms，

125
]hx()
v
对于实时调度，调度器必须支持抢占式、基于优先级的调度
§但只能保证软实时性；
v
对于硬实时系统，还必须提供满足截止期限的能力，需要添加
新的调度特征;
v
进程新的特点：周期性进程需要以固定的时间间隔使用CPU
§具有处理时间t、截止日期d、周期p
§0≤ T≤ D≤ P
§定期任务的速率为1/p

126
]hx()
v
进程应向调度器公布其截止时间；
v
采用准入控制（admission-control）
   — 承认进程，保证进程完成；
   —  如果不能保证任务能够在截止期限前完成，拒绝请
求； 

v
单调速率调度算法采用抢占的、静态优先级的策略，调度周期
性任务；
v
每个周期性任务分配一个优先；
127
ê(ëí()
v
优先级是根据其周期的倒数来分配的;
v
周期越短->优先级越高；
v
周期越长->优先级越低
v
P1 (周期为50,处理时间为20)的优先级高于P2（周期为100，
处理时间为35）；截止时间为下一个周期开始的时间
单调速率调度可认为是最优的，因为如果一组进程不能由此算
法调度，它不能由任何其他分配静态优先级的算法调度。

128
ìîïñâó1ê(ëí()
Ø假设进程P1 周期为50，执行时间为25；进程P2对应的值是80，
35；
Ø进程2并不能在截止时间80完成；
Ø单调速率调度有一个限制：CPU利用率是有限的，并不能完全
最大化CPU资源利用率，调度N个进程的最坏情况下的CPU利用
率为
                        
n 
  
n(2
1n
−1)
 
1 1.0 
2 0.828 
3 0.779 
4 0.756 
5 0.743 
6 0.734 
• 
• 
• 
• 
• 
• 
∞ 
ln 2 ≈ 0.693 
 

129
ùoûüop†°ç¢kü
n 
  
n(2
1n
−1)
 
1 1.0 
2 0.828 
3 0.779 
4 0.756 
5 0.743 
6 0.734 
• 
• 
• 
• 
• 
• 
∞ 
ln 2 ≈ 0.693 
 

130
mòïñ0ôöô()õúùeû
Ø调度根据截止期限动态分配优先级；截止期限越早，优先级越
高；截止期限越晚，优先级越低；
Ø单调速率调度的优先级是固定的，而EDF是动态的；
Ø此外，EDF调度不要求进程是周期执行的，也不要求进程的CPU
时间是固定的，唯一的要求是：进程在变成可运行时，应给出
它的截止时间。理论最佳，CPU利用率会到100%。
                        

131
t}2ü()õ+Å†°†Å¢†£É§•ÖÄÉÅÇû
v
op,£S_§+,•Z~¶ß®©e™ ́1
v
 ̈k,£≠ÆØ®F∞VØ±ß1
v
2≤≥¥( ̈k,£μÆ∂`z{ñYZ@Ø∑ß1
v
∏kπ∫ªºqrF4≤≥¥(+,áΩæ∂~
¶YZøπ∫ªºqr¿K¡§¬l√>@®$
ƒE3k@®$F≈á∆«¬l+∫ø

132
789»...
vCan occur in any priority-based preemptive scheduling scheme
vParticularly relevant in the context of real-time scheduling
vBest-known instance involved the Mars Pathfinder mission
vOccurs when circumstances within the system force a higher 
priority task to wait for a lower priority task
无界优先级反转
•优先级反转的持续时间不仅取决于处理共享资源所需
的时间，还取决于其他不相关任务的不可预测的操作

133
 ÀÃ789»...
Figure 10.9  Priority Inversion
T
1
T
2
T
3
s locked
(a) Unbounded priority inversion
preempted
by T
1
preempted
by T
2
s unlocked
time
normal execution
execution in critical section
s locked
blocked by T
3
(attempt to lock s)
t
1
t
2
t
3
t
4
t
5
t
6
t
7
t
8
T
1
T
2
T
3
s locked
by T
3
(b) Use of priority inheritance
preempted
by T
1
s unlocked
s unlocked
s locked
by T
1
blocked by T
3
(attempt to lock s)
t
1
t
2
t
3
t
4
t
5
t
6
t
7

134
789ÕŒ
Figure 10.9  Priority Inversion
T
1
T
2
T
3
s locked
(a) Unbounded priority inversion
preempted
by T
1
preempted
by T
2
s unlocked
time
normal execution
execution in critical section
s locked
blocked by T
3
(attempt to lock s)
t
1
t
2
t
3
t
4
t
5
t
6
t
7
t
8
T
1
T
2
T
3
s locked
by T
3
(b) Use of priority inheritance
preempted
by T
1
s unlocked
s unlocked
s locked
by T
1
blocked by T
3
(attempt to lock s)
t
1
t
2
t
3
t
4
t
5
t
6
t
7

135
œèé–êop
vThe three classes are:
§SCHED_FIFO: First-in-first-out real-time threads
§SCHED_RR: Round-robin real-time threads
§SCHED_OTHER: Other, non-real-time threads
vWithin each class multiple priorities may be used

136
°—^èêòYop
v
POSIX.1b标准
v
API提供用于管理实时线程的函数
v
为实时线程定义两个调度类：
1.SCHED_FIFO-线程使用FCFS策略和FIFO队列进行调度。
对于优先级相同的线程，没有时间片;
2.SCHED_RR-与SCHED_FIFO类似，不同之处在于对优先级
相同的线程进行时间切片；
v
定义两个用于获取和设置计划策略的函数：
1.pthread_attr_getsched_策略（pthread_attr_t*attr
，int*policy）
2.pthread_attr_setsched_策略（pthread_attr_t*attr
，int策略）

137
°—^èêòYop
●POSIX Real-Time Scheduling API
●#include <pthread.h> 
●#include <stdio.h> 
●#define NUM_THREADS 5 
●int main(int argc, char *argv[]) 
●{ 
●int i, policy;   pthread_t_tid[NUM_THREADS]; 
●pthread_attr_tattr; 
●/* get the default attributes */ 
●pthread_attr_init(&attr); 
●/* get the current scheduling policy */   if 
(pthread_attr_getschedpolicy(&attr, &policy) != 0) 
●fprintf(stderr, "Unable to get policy.\n"); 
●else { 
●if (policy == SCHED_OTHER) printf("SCHED_OTHER\n"); 
●else if (policy == SCHED_RR) printf("SCHED_RR\n"); 
●else if (policy == SCHED_FIFO) printf("SCHED_FIFO\n"); 
●} 

138
°—^èêòYop
●/* set the scheduling policy -FIFO, RR, or OTHER */    if 
(pthread_attr_setschedpolicy(&attr, SCHED_FIFO) != 0) 
●fprintf(stderr, "Unable to set policy.\n"); 
●/* create the threads */   for (i= 0; i< NUM_THREADS; i++) 
●pthread_create(&tid[i],&attr,runner,NULL); 
●/* now join on each thread */   for (i= 0; i< NUM_THREADS; i++) 
●pthread_join(tid[i], NULL); 
●}
●
●/* Each thread will begin control in this function */ 
●void *runner(void *param){ 
●/* do some work ... */ 
●pthread_exit(0); 
●} 
●POSIX Real-Time Scheduling API (Cont.)

139
œèé–êop
maximum
(a) Relative thread priorities(b) Flow with FIFO scheduling
D
DBCA
middleC
middleB
minimumA
(c) Flow with RR scheduling
Figure 10.10  Example of Linux Real-Time Scheduling
DBCB
CA

140
œèé–êop
v
在内核版本2.5之前，运行标准UNIX调度算法的变体
v
版本2.5移到固定顺序O（1）调度时间
§基于优先级的调度；
§两个优先级范围：分时和实时;
§实时任务优先级范围从0到99，普通任务优先级从100到139;
§映射到全局优先级，数值越小表示优先级越高;
§优先级越高，q值越大；
§只要时间片中剩余时间，任务就可以运行（活动）；
§如果没有剩余时间（过期），则在所有其他任务使用其时间片之前无
法运行；
§每个CPU运行队列数据结构中跟踪的所有可运行任务
•两个优先级阵列（活动、过期）；
•按优先级索引的任务；
•当不再处于活动状态时，将交换阵列；
§工作正常，但交互过程的响应时间很短；

141
çØ“”M‘’÷◊Mop
vA complete overhaul of the scheduling algorithm used in 
earlier UNIX systems
§Major modifications:
–addition of a preemptable static priority scheduler and the 
introduction of a set of 160 priority levels divided into three 
priority classes
–insertion of preemptionpoints

142
çØ“”M‘’÷◊M789Ät
Figure 10.12  SVR4 Priority Classes
Priority
Class
Real-time
Kernel
Time-shared
Global
Value
Scheduling
Sequence
159




100
first
last
59




0
99


60

143
çØ“”M‘’÷◊M789Ät

144
çØ“”M‘’÷◊M~ÿŸ⁄
00111
159012n
dqactmap
dispq
PP
P
P
P
P
P
P
P
Figure 10.13  SVR4 Dispatch Queues

145
¤‹[[›‘fifl,opÄ
Priority Class Thread Type Description 
0 - 63 Bottom-half kernel Scheduled by interrupts. Can block to await a 
resource. 
64 - 127 Top-half kernel Runs until blocked or done. Can block to await a 
resource. 
128 - 159 Real-time user Allowed to run until blocked or until a higher 
priority thread becomes available. Preemptive 
scheduling. 
160 - 223 Time-sharing user Adjusts priorities based on processor usage. 
224 - 255 Idle user Only run when there are no time sharing or real-
time threads to run. 
 
Note: Lower number corresponds to higher priority 

146
ÇòYop‡·‚„
Figure 10.11   Linux Scheduling Data Structures for Each Processor
140-bit priority array for active queues
140-bit priority array for expired queues
bit 0
(priority 0)
highest-priority
non-empty
active queue
bit 139
(priority 139)
Active Queues:
140 queues by priority;
each queue contains ready
tasks for that priority
Expired Queues:
140 queues by priority;
each queue contains ready
tasks with expired time slices
for that priority

147
‰Âœèé–êÊÁM
v
内核在2.6以后，完全公平调度程序（CFS）
v
Linux系统的调度基于调度类；
v
内容
§每个调度类都有特定的优先权；
§调度程序在最高调度类中选择优先级最高的任务；
§而不是基于固定时间分配的时间片，基于CPU时间的比
例；
§包括两个调度类，可以添加其他调度类；
1.采用CFS调度算法的默认调度类；
2.实时

148
¶ß®¢£©™ ́ ̈1()'•
v
根据从-20到+19的nice值计算时间片；
§值越低优先级越高；
§计算目标延迟–任务至少运行一次的时间间隔；
§如果活动任务的数量增加，目标延迟可能会增加；
v
CFS调度程序在变量vruntime中维护每个任务的虚拟运行
时间
§与基于任务优先级的衰减因子相关–优先级越低，衰减
率越高;
§正常默认优先级产生虚拟运行时间=实际运行时间;
v
为了决定下一个要运行的任务，调度器选择虚拟运行时间
最低的任务；

149
†¤‘Bá

150
®¢£©™•()]hx
v
根据POSIX实时调度.1b
§实时任务具有静态优先级，0~99，普通任务：
100~139
v
两个值域合并映射到全局优先级方案；
v
nice的值-20映射到全局优先级100；
v
+19的nice值映射到优先级139；

151
®¢£©™•()]hx
v
Linux支持负载平衡，但也支持NUMA。
v
调度域是一组可以相互平衡的CPU核心。
v
域按它们共享的内容（即缓存）进行组织目标是防止
线程在域之间迁移。

152
≠¢£Ñ†ÆÖ()'
v
Windows使用基于优先级的抢占式调度;
v
下一个运行最高优先级的线程;
v
分派器就是调度器
v
线程运行直到（1）阻塞，（2）使用完时间片，（3）
被更高优先级的线程抢占；
v
实时线程可以抢占非实时线程；
v
32级优先权方案；
v
优先级可变类为1-15，实时类线程优先级为16-31；
v
优先级0是内存管理线程；
v
为每个优先级排队；
v
如果没有可运行线程，则运行空闲线程（IDLE）；

153
≠¢£Ñ†ÆÖ()'
v
Win32 API标识进程可以属于的几个优先级类
§REALTIME_PRIORITY_CLASS, 
§HIGH_PRIORITY_CLASS, 
ABOVE_NORMAL_PRIORITY_CLASS,
§NORMAL_PRIORITY_CLASS,
§BELOW_NORMAL_PRIORITY_CLASS,
§IDLE_PRIORITY_CLASS
§除实时类外，其他线程的优先级都是可变的；
v
给定优先级类中的线程具有相对优先级
§TIME_CRITICAL, HIGHEST, ABOVE_NORMAL, NORMAL, 
BELOW_NORMAL, LOWEST, IDLE
§优先级类和相对优先级结合起来，以提供优先级数值；
v
基本优先级在类中是NORMAL；
v
如果时间片用完，优先级会降低，但决不会低于优先级基准；

154
≠¢£Ñ†ÆÖ()'
v
如果发生等待，则根据等待的内容提升优先级
v
前景窗口的优先级提高了3倍
v
Windows 7添加了用户模式调度（UMS）
§应用程序创建和管理独立于内核的线程
§对于大量线程，效率更高
§UMS调度程序来自C++等并行运行时（CORCRT）框架的编
程语言库

155
ËèéÈ—L^789

156
f†§ÉÅ¢Ö()
v
基于优先级的调度
v
有六个分类
§分时（默认）（TS）
§交互式（IA）
§实时（RT）
§系统（SYS）
§公平份额（FSS）
§固定优先级（FP）
v
给定的线程一次只能在一个类中
v
每个类都有自己的调度算法
v
分时是多级反馈队列
§可由sysadmin配置的可加载表

157
‘—ÍÎ‹è^opÏ

158
‘—ÍÎ‹è^op

159
f†§ÉÅ¢Ö()
v
调度器将特定于类的优先级转换为每个线程的全局优
先级
§下一个运行优先级最高的线程
§运行直到（1）阻塞，（2）使用完时间片，（3）被
更高优先级的线程抢占；
§通过RR选择具有相同优先级的多个线程；

160
bcØ∞
v
如何为操作系统选择CPU调度算法？
v
确定标准，然后评估算法
v
确定性建模
§分析评价类型;
§获取特定的预定工作负载，并为该工作负载计算每个算
法的性能；
v
考虑在时间0到达的5个过程：

161
±≤≥Ø∞
v
对于每个算法，计算最小平均等待时间
v
简单快速，但需要输入精确数字，仅适用于这些输入
§FCS is 28ms：
§非抢占式SFJ为13ms：
§RR为23ms：

162
¥IμD
v
描述进程的到达，以及CPU和I/O执行的概率
§通常是指数型的，用平均值来描述
§计算平均吞吐量、利用率、等待时间等。
v
描述为服务器网络的计算机系统，每个服务器都有等
待进程队列
§了解到达率和服务率
§计算利用率、平均队列长度、平均等待时间等。

163
®¢§Ç∂Ö•®ÉÆ
v
n=平均队列长度
v
W=队列中的平均等待时间
v
λ=进入队列的平均到达率
v
利特尔定律(œèÌÌÍ[Ó^MœÎL)——在稳定状态下
，离开队列的进程必须等于到达队列的进程，因
此：n=λx W
§适用于任何调度算法和到达分布
v
例如，如果平均每秒到达7个进程，并且队列中
通常有14个进程，则每个进程的平均等待时间=2
秒

164
®¢§Ç∂Ö•®ÉÆ
v
排队模型能力有限
v
更精确的模拟
§计算机系统的程序模型
§时钟是一个变量
§收集指示算法性能的统计信息
§用于驱动通过以下方式收集的模拟的数据：
•基于概率的随机数发生器
•数学上或经验上定义的分布
•跟踪磁带记录真实系统中真实事件的序列

165
†°çopñÔ

166
à∑
§即使是模拟，其准确性也是有限的；
§在实际系统中实现新的调度器和测试
•高成本、高风险；
•环境各不相同；
§最为灵活的调度程序可以根据管理员或用户进行修
改
§利用API或者命令行来修改优先级；
§但环境也各不相同；

167
