# 2024-OS-CH-6-CPU调度-图集

> 主题库：操作系统　｜　源文件：2024-OS-CH-6-CPU调度-图集.pdf　｜　6,060 字符　｜　44 个片段　｜　更新于 2026-11-11
Help

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

(inteD Core"i5 8

inte Core” emin

0

Yaadstore add store CPU burst readfromfile wait for I/O I/O burst catoreincrement index CPU burst wait for I/O I/Oburst laadstore add store CPU burst Yead from file wait for l/O -l/O burst

160 140 120 60 40 20 Y 0 8 16 24 32 40 burst duration (milliseconds)

queueheader PCB7 PCB2 ready head 1 queue tail registers registers

Po 0 executing save state into PCB。 dispatch latency restore state Yrom CB executing

12 [T 10 ti 6 4 2 time CPUburst(t) 6 4 6 4 13 13 13 "guess"(τ) ) 10  8 6 6 5 9 11 12

process time = 10 quantum context switches 12 0 0 10 6 1 0 6 10 1 9 0 1 2 3 4 5 6 7 8 9 10

1 process time 25 6 120 1 P 31 15 7 11.0  10.5 10.0 9.5 9.0 1 2 3 4 5 6 7  taime quantum

real-time processes system processes interactive processes batchprocesses

intel  Core"i7

quantum =8 quantum = 16 FCFS

where T, = turn aro und tim e or residence tim e; total tim e in system , waiting plus exe- Ts = average service time; average time spent in Running state

CPU( −1) CPU(i) = 2 CPU(i) Pj(i) = Basej + 2 + nicej where CPU,(i) = measure of processor utilization by process j through interval i P i = priority of process at beginning of interval i; lower values equal

rOot@r~# Chrt=m SCHED OTHERmin/max ρriority O/O SCHED_FIFO min/max priority 1/99 SCHED RR min/maX priority 1/99 SCHED_BATCH min/max priority O/0 SCHED_IDLE min/max priority O/O SCHED_DEADLINE min/max priority O/O  root@r ~# chrt =ρ 32273 pid 32273''s current scheduling policy: SCHED OTHER pid 32273''s current scheduling priority: O

root@r ~# chrt =f -ρ 15 32273 root@r ~# chrt=p 32273 oid 32273''s Current scheduling policy: SCHED_FIFO oid 32273''s current scheduling priority: 15

T T To To T T Tn T To T coreo Core. coren coreo core1 coren 1 1 common readyi  queue ( per-core run queues (a) (b)

CPU CPU o ac fastacceess fast access [Ymamory memery

C compute cycle M memory stall cycle thread C M C M C M C M

thread1 C M C M C M C threado  C M C M C M C tIme

event E fi rst occurs event latency TO TI real-tim e system responds to E TimE

interrupt determine task T running interrupt TYPE ( context  switch ISR latency time

eeent responsetoevent process made interrupt available Yeal-tame process execution conflicts  dispatch time

d d d t t t [Time period1 period2 period3

0 10 20 30 40 50 60 70 80 90 100 110 120 130140 150 160170180 190200

deadlines P1 P2 P1 P1,P2 P P2」 P1 P2 0 10 20 30 40 50 60 70 80 90 100 110 120 130 140 150 160

deadlines P1 P2 P1 P1 P2 0 10 20 30 40 50 60 70 80 90 100  110 ) 120 130 ) 140 150 160

The new algorithm is • highest preference to real-  next-highest preference to  kernel-mode processes ?lowest preference to other user-mode processes

Realtime(159-10o) Kernel(99-6o) Time-shared(59-0) guaranteed to be selected to run before guaranteed to be selected to run before lowest-priority processes, intended for ca

The Linux CFS scheduler provides an eficient algorithm for selecting which task torun next. Eachrunnable task isplacedin ared-black tree — a balanced binary search tree whose key is based on the value of vrunt ime. This tree is shown below: To) Taskwith the smallest T T2 value ofvruntime 1 T3 T4) T5) T6 samaller larger Value of vruntime W hen a task becomes runnable, it is added to the tree. If a task on the tree is not runnable (for exam ple, if it is blocked while waiting for I/ O), it is removed. Generally speaking, tasks that have been given less processing time (smaller values of vrunt ime) are toward the left side of the tree, and tasks that have been given more processing time are on the right side. According to the properties of a binary search tree, the leftmost node has the smallest keyvalue, which for the sakeof the CFS scheduler means that itis the task with the highest priority. Because the red-black tree is balanced, navigating it to discover the leftmost node will require O(lg N) operations (where N is the number of nodesin the tree).However,for efficiency reasons,the Linux scheduler caches this value in the variable rb_leftmost, and thus determining which task to run next reguires only retrieying the cached yalue.

Real-Time Normal 0 99100 139 Higher Lower PRIORitY

physical processor domain (NUMA node) domain。 domain1 coreo core2 2 2 core1 core3 3

real- high above below idle time normal normal normal priority time-critical 31 15 15 15 15 15 highest 26 15 12 10 8 6 abovenormal 25 14 11 9 7 5 normal 24 13 10 8 6 4 below normal 23 12 9 7 5 3 lowest 22 11 8 6 4 2 idle 16 1 1 1 1 1

time return time quantum from priority quantum expired sleep 0 200 0 50 5 200 0 50 10 160 0 51 15 160 5 51 20 120 10 52 25 120 15 52 30 80 20 53 35 80 25 54 40 40 30 55 45 40 35 56 50 40 40 58  55 40 45 58 59 20 49 59

global scheduling priority order 169 highest first interrupt threads 160 159 realtime (RT) threads 100 9 system (SYS) threads 60 59 fair share (FSS) threads fixed priority (FX) threads timeshare (TS) threads interactive (lA) threads lowest 0 last

P1 P2 P3 P 4 P 5 0 10 39 42 49 61

P. P 3 P1 P₂ P2 4 5 0 3 10 20 32 61

simulation statistics forFCFS FCFS CPU 10 1O 213 actual CPu 12 performance process IO 112 simulation  statistics  eXecution CPU 2 forSjF IO D  147 SJF CPU 173 trace tape caim ulation statistics Yaa RR(c =14 RR(q =14)

[中计算院 SENUNNN SUN YAT-SENUNIVERSITY SCHOOL OF COMPUTER SCIENCE AND ENGINEERING 处理器调度 User Space Process Process Process ernel Space Scheduling AIg Process D Cintel Context- Corei 中 switching

中山大學 0 5 10 15 20  让计算机学院一 SENUNN SUNYAT-SEN UNIVERSITY SCHOOLOF COMPUTEF - I 1 First-Come-FirSt - - Served (FCFS) - - 调度策略比较 D - - 一 E 一 A Round-Robin B (RR), q = 1 C 1 1 D E - 1 A Round-Robin B (RR),q=4 C 1 1 1 D - 1 E - I - - 表9.4－进程调度示例 A Shortest Process B Next（SPN） C - 进程到达时间服务时间 - - D - I A 0 3 E - B 2 6 A ShortestRemaining B - C 4 4 Time(SRT) C - - - - - - D D 6 5 1 E - - E 8 A Highest Response B RatioNext(HRRN C - D - E 1 - 1 A Feedback  q=1 C - - D - E 1 A Feedback q = 2 B C 0 5 10 15 20 _gure9.5 AComparisonofSchedulingPolieies
