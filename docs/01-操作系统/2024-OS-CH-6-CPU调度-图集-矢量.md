# 2024-OS-CH-6-CPU调度-图集-矢量

> 主题库：操作系统　｜　源文件：2024-OS-CH-6-CPU调度-图集-矢量.pdf　｜　10,387 字符　｜　67 个片段　｜　更新于 2026-11-13
旨令周期的分布 B ranch(%) Flatig poin(%) A rithmetic(%)) Load(%) S tre() Web 36 Feed1 4 4 34 10 Feed2 41 14 Ads1 34 10 Ads2 38 5 13 Cache1 38 4 17 Cache2 36 18 400.perlbench 38 13 401.bzip2 43 30 10  403.gcc 36 18 429.mcf 31 35 445.gobmk 42 26 13 456.hmmer 37 1J 15 458.sjeng 44 24 9 462.libquantum J 28 3 464.h264ref 41 38 / 471.omnetpp 30 29 16 473.astar 39 34 11 483.xalancbmk 29 31 31 8 % 20 40 60 80 100

Question.How toimproveCPU utilization (CPU is m uch faster than I/O)? Question.How to improve system responsiveness (interactive applications)?

Multiprogramming

(3) Running 1 Termination (2) Wattnl for results Process cYmanane l

int main(void) ) { int x= 1;

New Terminated (Just fork()-ed) (Zombie) Waiting (blocked) Process States 12

New every process is created using Terminated fork(). Ready Running Waiting (blocked States

The process is ready. It means it is ready to run but is not running. New A process may become "ready" after... - it is just created by fork O); - it has been running on the CPU for some time and the OS chooses Ready anotherprocess torun; - returning from blocked states. all ready processes are kept on a list Wait calledreadyqueue queue header PCB7 PCB2 ready head Process queue tail registers registers States 14

aaeOschoosesthis rocessobe New running on the CPU and changes Terminated Ready Running Waiting cYa n 15

The process is blocked. While the process is running,it N w maybeWaitingforsomething Terminated and becomesblocked voluntarily. Waiting cYa

ala al Sometimes, the process has to wait for the response from the device and, Nevertheless, this blocking state is interruptible.E.g., "Ctrl + C" can get the process out of the waiting state (but goes to termination state instead. Waiting (interruptible) cYa t

Sometimes, a process needs to wait for a resource but it doesn''t want to be disturbed while it is waiting. In other words, the process wants that resource very much. Then, the process status is set to the uninterruptible status. Ready Runn ng Waiting (uninterruptible) Process States

When response arrives, the status of the process changes back to Ready. from any one of the blocked states. Process data Waiting cYaa 19

The processmay Terminated Running Process

H工T久H汉三王 So, whatis process scheduling? Mainly about how to make all the ready processes  become"Running"  This is the called short-term scheduling

Long-term Time-out 1 tch Ready Queue Short-term Release Ds scheduling Processor Medium-term Ready, SuspendQueue users Medium-term Blecked,SuspendQueue Blocked Queue aeent EventWait Occurs

操作系统何时可以承要接受哪些作业担一个或多个额外的并将其转化为进 [进程程

可以从队列中创建进程，但必须 [Tab _

先来先服务

按照优先级、期望的执行时间等

事件包括： [时钟中中 [ll

A new processis When"fork is invokedandreturnssuccessfully. Then, whether the parent or the child is scheduled is up to the cachedule ''s decision. An existing process The CPU is freed. The scheduler should choose another process to run. A process waits for The CPU is freed. The scheduler should choose another process to run. A process finishes The interrupt handling routine makes a scheduling request, if

become running? (Note that the running process may not terminate at that time) Cantextswiteching 上下文切换 Question #2: How to decide which process should be running? Scheduling criteria & scheduling algorithms 调度标准和算法 Question #3: How to design scheduling in a real/specific system ? [实 Multiprocessor system, real-time system, algorithm eveluation [3

AlgorithmProperties CPUl utilization Throughput Turnaround time Waiting time Response time

Types Nonpreemptive Application Multiprocessor

What factors/performance measures shouldbe( carefully considered?  Turnaround time Throughput Waiting time CPU utilization Response time

Cpu tilization。 CPU的利用率 We want to keep CPU as busy as possible. Theoretically, can range  from 0-100%, but in real system,range from 40%-90% Turnaround The higher the better time Throughput Waiting time CPU utilization Response time

Throughput. 吞吐量 Number of processes that are completed per The higher the better Turnaround time Taroughut Waiting time CPU utilization Response time 20

Turnaround time. 周转时间 Time to execute the process: interval from the1 time of submission to the1 time of completion (total running time + waiting time+ doingl/o) The lower the :better Turnaround time Throughput Waiting time CPU utilization Response time

Waiting time. . . 三等待时间气 The time spent waiting in the ready queue [Ta elowerthe better [Ta nao d time Throughput Waiting time CPU utilization Response time 41

Responsetime. 响应时间 The time from the submission of a request until · | the 1 : first response is produced (useful measure forinteractive systems) [Taaelowerthe better time Throughput Waiting time CPU utilization Response time 42

Question Common Can we optimize all the above goal measures simultaneously? Policy Big enforcement Big conflict conflict Design Tradeoff CPu-/o Fairness Balance Little conflict

A set of P1 P2 P3 P4 processes [ ar ach It is interestingtonotethat Arrival Pu this is a non-sense! process... Time requirement [Yaaaa 离线调度算法 An offline scheduling algorithm assumes that you know all the processes submitted to the system before hand. But, an online Online Vs scheduling algorithm does not have such an assumption. Offline 在线调度算法 Yet, every real scheduler has to work in an "online scenario". So, we

HT- Individual & average turnaround time Saheduling Individual & average Number of. context Sawitehing

Algorithms Preemptive? Target System or First-in, First-out No. Out-of-date aFIFO) Shortest-job-first (SJF) Can be both. Out-of-date Round-robin(RR) Yes. Modern Priority scheduling Yes. Modern Priority scheduling The real implementation! with multiplequeues.

Output P1 P2 P3 0 2 4 6 8 1 1 1 1 1 2 2 2 2 2 3 0 2 4 6 8 0 2 4 6 8 0 Waiting time: P1 = 0;P2 = 23; P3 = 25; Average waiting time = (0+23+25)/3 = 16; ( Task Arrival cPu Time Req. Turnaround time:P1= 24; P2=2 26;P3 = 28; P2 1 3 Average turnaround time = ( (24+26+28)/3 = 26;   P3 2 3 Input 4O

Algorithms Preemptive? Target System First-come,1 , first-served arFirst-in,First-out No. Out-of-date aFIFO) Shortest-job-first (SJF) Can be I : both. Out-of-date Round-robin (RR) Yes. Modern rianrityscheduling Yes. Modern Priority: scheduling The real implementation! with multiple queues.

Non-preemptive SJF Preemptive SJF Average waiting time 4 3 (smallest) Average turnaround time 8 7(smallest) #of context switching 3 (smallest) 5 Task A 平均等待时间和平均周转时间降低， 但是  P1 代价是增加了上下文切换的成本； P2

Non-preemptive SJF Preemptive SJF Average waiting time 4 3(smallest) Averaget turnaround time 8 7 (smallest) # of context switching 3 (smallest) 5 Ta A 可以证明SJF调度算法是最优的， 因为SJF P1 的平均等待时间最小； 但是， 如何知道下 P2 次CPU执行的长度？ P3

Challenge: How to know the length of the next CPU cYaauee Solution: Prediction (by expecting that the next CPU Tn+1 = α tn + (1 − α)Tn. burst will be similar in length to the previous ones) 12 Ti 10 Predicted 8 value ti 6 4 Mostrecentinfor ation 2

Nan-preemptive SJF Preemptive SJF RR [aeerage a iting t me 4.(argest Average turnaround 8 7 .(argest) time So, the RRalgorithm gets all the bad! Why do we still need it? The responsiveness of the I processes is great under the RR algorithm. E.g., you  won''t feel a job is "frozen"| because every job is on the CPU from time to time!

Priorityi class 4 Non-preemptive, SJF The processes are Priorityi class 3 RR with quantum = 10 units. permanently assigned to Priorityi class2 RR with quantum = 20l units. Priority class1 RR with quantum = 40 units. scheduling among queues

Priority 4 Priority E.g., using round-robin in each queue. Increasing priority

Priority class3 Increasing priority E.g., using round-robin in each queue.

Priority class 4 Increasing priority E.g., using round-robin in each queue.

E.g., using round-robin in each queue. Increasing priority

E.g., using round-robin in each queue. Increasing priority

Priority class 3 A process drops to a RR with quantum = 10 units. Priority class 2 RR withi quantum = 20 units. after it has l; used iI up its Priority class 1 RR with quantum = 40 ) units. quantum recharged.

Round FCFS robin SPN SRT HRRN Feedback Selection max[w] constant min[s] w +s) min[s-e] max (see text) function s Preemptive Decision Non- Non- Preemptive Non- Preemptive May be small May be gnood good Provides Response there is a response good Not time large response Provides good time for time for response response time emphasized variance in short short time process execution processes processes times e rhead Minimum M in mum Can behigh Can ehigh Can behigh Ca behigh processes penalizes treatment SYaareatton No No Possible Possible No Possible

N SUN YAT-SEN UNIVERSITY  SCHOOL OF COMPUTER SCIENCE AND ENGINEERING 10 6 5 Priority 4 3 2 Pioity 0.2 0.3 0.4_ 0.5 0.6 0.7 0.8 0.9

0, 9 priorityclasses λ₁=λ2 s2=5□ts1 8 7 6 [Ta 5 with preemption 4 Nopriority 2 0.1 0.2 0.3 0.4 0.5 0.6 0.7 0.8 0.9

1im e Priority 0 – count count Priority count count Priority count count 60 0 0 60 0 0 60 0 0 1 1 2 2 60 60 1- 90 30 30 60 0 0 60 0 0 1 1 1 2 2 2 2- 6 6 6 4 1 1 9 3 30 5 30 16 16 17 17 3- 75 75 96 37 37 74 15 15 67 0 15 17 2 17 4 5 6 78 18 18 8 3 9 3 19 19 20 20 8 78 98 39 39 7 3 18 6 1 18

1I e Priority CPU count Priority CPU count Priority CPU count 0–  60 0 60 0 60 0 1 2 75 30 60 0 60 0 1 2 2- 60 67 15 75 30 60  2 3- 60 63 8 67 15 75 30 9 67 4- 76 33 63 7 67 15 8 9 5- 67 - 68 16 76 33 63 7

113

25 Si c 15 10  05 pr 0 1 2 3 4 5

15 Si pr 10 05  00 98 0 1 2 3 4 5

scmpoltamccopomeme dcontrol oflaboratoryexperiments oprocesscontrolinindustrialplants cYaa na l • military command and control systems

为 n(21/n −1) nes P1 P21 P1 P2 20 30 40 50 60 70 80 90 128

n n(21n −1) 1 1.0 2 0.828 3 0.779 4 0.756 5 0.743 6 0.734 8 1n 2≈0.693 129

s locked by T1 byT2 3 u 0 C

by T 3 by T 1

to lock s) s unlock

A mimimum B middle C middle D maxmum

Ciass vaiue Seyuemce 159 first 100 99 Kernel 60 59 [Ta ead

159 n 210

Priority Class Thread Type Description 0 - 63 Bottom -half kernel Scheduled by interrupts. Can block to await a Yesurce. 64 - 127 Top-half kernel Runs until blocked or done. Can block to await a resource. 128 -159 Real-time user Allowed to run until blocked or until a higher priority thread becomes available. Preemptive secheduling. 160 - 223 Time-sharing user Adjusts priorities based on processor usage. 224 - 255 Idle user Only run when there are no time sharing or real-

(P
