# 2024-OS-CH-4-进程-part1-图集

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part1-图集.pdf　｜　8,211 字符　｜　27 个片段　｜　更新于 2026-11-11
Help

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

80

new admitted interrupt exit terminated ready running I/O or eventcompletion I/O or event wait waiting

表3.2 导致进程终止的原因事件说明正常完成进程自行执行一个操作系统服务调用，表示它已经结束运行进程运行时间超过规定的时限。可以测量多种类型的时间，包括总运行时间（“挂钟时间”）、花费在执时行上的时间，以及对于交互进程从上一次用户输入到当前时刷的时间总量无可用内存系续无法编足进程需要的内存空间超出范围进程试图访间不允许访问的内存单元保护错误进程试图使用不允许使用的资源或文件，或试图以一种不正确的方式使用，如往只读文件中写算术情误进程试图进行被禁止的计算。如除以零成存储大于硬件可以接纳的数字时间超出进程等待某一事件发生的时间超过了规定的最大值 1VO失败在输入或输出期间发生错误， 如找不到文件、在过规定的最多努力次数后仍然读/写失败（如遇到磁带上的一个坏区时）或无效操作（如从行式打印机中读） 无效指令进程试图执行一个不存在的指令（通常是由于转移到了数据区并企图执行数据） 特权指令进程试图使用为操作系统保留的指令数据误用婚误类型观末初始化的一块数据操作员或操作系统干涉由于某些原因，操作员或操作系统终止进程（如出现死领时） 父进程终止当一个父进程终止时，操作系统可能会自动终止该进程的所有子进程父进程请求父进程通常具有终止其任何子进程的权力

Swapping The OS needs to release sufficient main memory to bring in a process that is ready to execute. Other OS reason The OS may suspend a background or utility process or a process that is suspected of causing a problem. Interactive user request A user may wish to suspend execution of a program for purposes of debugging or in connection with the use ofaresource. Timing A process may be executed periodically (e.g., an accounting or system monitoring process) and may be suspended while waiting for the next time interval. Parent process request A parent process may wish to suspend execution of adescendent to examineormodify the suspended process, or to coordinate the activity of various cdescendants.

allocation of main processes memory to allocation memory of secondary to P rocesses Protection of main atributes Or virtual of blocks  information memory needed virtual to memory manage

HanYo e ss operationisen f an Io needs sto o esource os beingused as sthe transer a sacvo

inte Core” emin

userprocess usermode (modebit=1) user process executing calls system call return from system call kernel trap return [modebit=0 modebit=1 と kernel mode eaecutesystemcall (modeb =o

processnumber arnogram nounter registers memory  limits  list ofopen nfiles

640 structtaskstruct 641 #ifdef CONFIGTHREAD INFOIN TASK 642  643 * For reasons of header soup (see current thread infoO), this 644 *must be the first element of task struct. 645 / 646 struct thread info thread_info; 647#endif 648 /*-1 unrunnable,0 runnable, >o stopped: */ 649 volatile long state; 650 651  652 * This begins the randomizable portion of task_struct. Only 653 * scheduling-critical itemsl should be added above here. 654  655 randomizedstructfieldsstart 656 657 void *stack; 658 refcountt usage; 659 /* Per task flags (PF*), defined further below:*/ 660 unsigned int flags; 661 unsignedint ptrace; 662 663 #ifdefCONFIGSMP 664 int On_cpu; 665 struct callsinglenode  ( wake_entry; 666 #ifdefCONFIGTHREADINFO INTASK 667 /* Current Cpu:*/ 668 unsignedint cpu; 669#endif 670 unsigned int wakee_flips; 671 unsignedlong wakee_flip_decay_ts; 672 structtask_struct *last_wakee; 673 674

struct task struct struct task struct struct task struct process information process information process information Current (currently executing proccess)

registers of the control and Program status information contents of status status EFLAGS register gisters word is an example ofa processorre (PsW) an PSW used by :''an y gistersre stack Os running on

9 1513 0 OD s XID（标识标志） CDF（方向标志） XVIP（虚拟中新挂起） XF（中断允许标志） XVIF（虚拟中新标志） XTF（陷阱标意） XAC（对齐检查） $SF（特号标志） XVM （虚报8086模式） SZF（零标志） XRF（恢复标志） SAF（辅助进位标） XNT（ 套任务标志） SPF（奇调校验标志） XIOPL（IO特权级） SCF（进位标志） SOF（出标志） S表示状态标志（状态标业） C表示控制标毒（控制标志） X表示系统标志（系统标志） 同影位为保闻位图3.12 x86EFLAGS寄存器

表3.6 x86的EFLAGS寄存器位状态标忠（条件码） AF（辅助速位标志） 在使用AL当存器的8位算术或逻辑运算中，指明半个字哲间的进位或借位 CF（进位标志） 指明算术运算后，最左位的进位或借位情况：也可被某些移位或循环操作改变 OF（溢出标志） 指明加法或减法后的算术溢出情况 PF（奇偶校验标志） 指明算术或逻辑运算结果的奇偶情况。1表示偶数奇偶校验，0表示奇数奇偶校验 SF（符号标态） 指明算术或逻辑运算结果的符号位情况 ZF（零标志） 指明算术或逻辑运算的结果是否为零 [控制标确定字符串处理指令是递增或递减到16位率寄存器SI和DI（用于16位操作），还是递增或递减 DF（方向标志） 到32位寄存器ESI和EDI（用于32位操作） 系统标志（应用程序无法修改） AC（对齐检查） 设置字减双字基按非字边界等址还是按非双字边界寻址 ID（标识符位） 如果该位可被置位和请弥，则处理器支持CPUID指令。该指令提供关于厂商、产品系列和型号的信息 RF（恢复标志） 允许程序员禁用调试异常。以便调试异常后可以重新启动该指令面不会立即导致另一个调试异常 IOPL（IO特权级） 置位会使得在保护模式运行期间，处理器对所有VO设备的访问生成一个异常 IF（中断允许标志） 置位时处理器将识别外部中断 TF（陷阱标志） 置位时每个指令执行后会引发一个中断。可用于调试 NT（族套任务标志） 指明当前任务账套在以保护模式运行的另一个任务中 VM（虚损8086模式） 允许程序员启用或禁用虚拟8086模式，以换定处理器是否像8086机那样运行 VIP（虚拟中断挂起） 虚拟8086模式下用于指明一个减多个中断正在等待服务 VIF（虚拟中新标志） 虚拟8086模式下用于代替F

queue header PCB7 PCB2 Ready head queue tail registers registers PCBz PCB14 PCB Wait head cueue ta_l

进程控制块运行态就堵态阳唐态图3.14 进程链表结构

readyqueue CPu l/0 l/o wait queue I/0 request ctim e slice child child create child taemination terminates wait queue process interrupt interrupt wait for an Occurs wait queue interrupt

process P。 operating system process P1 save state into PCB。 idle interrupt or system call executing cYae a reload state from PCBo cYaace n

The steps in update the process a full  the process proCess save the context of controlblock of move control 1block ( of switch are: the processor theprocess this process to the currently in the appropriate queue Running state select another process for execution restore the context of the 1 processor to that which existed cat the time the update memory update the process controlblockof selected process was managementdata lastswitchedout structures theprocess selected

systemd(1)—TModemManager(738) {ModemManager}(794)  {ModemManager}(822) —NetworkManager(752)— -dhclient(95o) {NetworkManager}(817) {NetworkManager}(819) accounts-daemon(756)—T   {acCounts-daemon}(777) accounts-daemon)(82o)  alsact1(833) avahi-daemon(i3oo) avahi-daemon(i3o2) -boltd(1854)—{bo1td}(1855)  {bo1td}(1857) -colord(1318)- {colord}(1321) {colord}(1323) cron(749) dbus-daemon(750) dnsmasq(972)-—dnsmasq(973) gdm3(860)— gdm-session-wor(877)— gdm-x-session(940)—TXorg(942)- {Xorg}(1104)  {Xorg}(1105) {Xorg}(1106) {Xorg}(1107) {Xorg}(1108) {Xorg}(1109) {Xorg}(1110) {Xorg}(1111) {Xorg}(1112) {Xorg}(1113) {Xorg}(1114) {Xorg}(1115) {Xorg}(1118)

#include<sys/types.h> #include <stdio.h> #include <unistd.h> intmain( E pid_t pid; /* fork a child process */ pid=fork(); if (pid< O) { /* error occurred */ fprintfstderr ForkFailed" return1; else if (pid == O) { /* child process */ execlp("/bin/ls","ls",NULL); else { /* parent process */ /* parent will wait for the child to complete */ printf("ChildComplete"); returno

1   #include<stdio.h> 2   #include<stdlib.h> //exit  3  // fork   #include<unistd.h>  5intmain() { 6 intx = 100; 7 intrc = fork(); 8 if (rc < 0) { 9 // fork failed; exit 10 fprintf(stderr, "forkfailed\n"); 11 exit(EXIT_FAILURE); 12 } else if (rc == ) {  13 x = 101; 14 printf("''x in child process: %d the address is %d\n", x, &x); 15 } e else { 16 x=102 17 printf("x in parent process: %d\n, the address is %d\n", x,&x); 18 19 return o;

1 #include<stdio.h> 2 #include <stdlib.h> 3  #include <sys/types.h> 4 #include<unistd.h> #include <sys/wait.h>   intmain(void)  8 9 int count = 1; 10 pid_t childpid; 11 12 childpid = vfork(); /* child shares parent''s address space */ 13 if (childpid < 0) { 14 perror("vfork()"); 15 returnEXITFAILURE; 16 } 17 else /* vfork() returns 2 values: 0 for child pro and childpid for parent pro 18 if (childpid == ) { /* This is child pro, parent hung up until child exi 19 count++; 20 printf("Child pro pid = %d, count =%d (addr =%p)\n", getpid(), coun count); 21 printf("Child taking a nap...\n"); 22 sleep(L0); 23 printf("Child waking up!\n"); 24 _exit(0); /* or exec(o); "return" will cause stack smashing */ 25 } 26 else { /* This is parent pro, start when the vforked child terminated */ 21,23

#include <stdio.h> #include <windows.h> STARTUPINFOsi; PROCESS_INFORMATIONpi; /* allocate memory */ ZeroMemory(&si,sizeof(si)); si.cb = sizeof(si); ZeroMemory(&pisizeof(pi); /* create child process */ if (!CreateProcess(NuLL, /* use command line */ "C:\\wINDOwS\\system32\\mspaint.exe",/* command */ NULL, /* don''t inherit process handle */ NULL, /* don''t inherit thread handle */ FALSE,/* disable handle inheritance */ O,/* no creationflags*/ NULL,/* use parent''s environment block */ NULL,/* use parent''s existing directory */ &si, &pi)) fprintf(stderr, "Create Process Failed"); Teturn—; } /* parent will wait for the child to complete */ WaitForSingleObject(pi.hProcess,INFINITE); printf("Child Complete"); /* close handles */ CloseHandle(pi.hProcess); CloseHandle(pi.hThread);
