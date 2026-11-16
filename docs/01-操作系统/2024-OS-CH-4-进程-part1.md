# 2024-OS-CH-4-进程-part1

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part1.pdf　｜　36,576 字符　｜　141 个片段　｜　更新于 2026-11-11


Operating Systems Principles
操作系统原理
陈鹏飞
计算机学院
1

第四讲 —进程
2

3
v
进程概念
v
进程调度
v
进程上的操作
v
进程间通信
v
IPC在共享存储系统中的应用
v
消息传递系统中的IPC
v
IPC系统示例
v
客户机-服务器系统中的通信
!"

4
#$
v
确定进程的各个组件，并说明如何在操作系统中表示和
调度这些组件。
v
描述如何在操作系统中创建和终止进程，包括使用执行
这些操作的适当系统调用开发程序。
v
描述并对比使用共享内存和消息传递的进程间通信。
v
设计使用管道和POSIX共享内存执行进程间通信的程序
。
v
描述使用套接字和远程过程调用的客户机-服务器通信
。
v
设计与Linux操作系统交互的内核模块。

5
What is a process? 
3
Process
Relationship?
Differences?
Execution?
Program
Informally, a process is a program in execution.
%&'()*

6
()'&+
What is a program?
•What is a program?
–A program is a just a piece of code.
•But, which code do you mean?
–High-level language code: C or C++?
–Low-level language code: assembly code?
–Not-yet an executable: object code?
–Executable: machine code?
5

7
,-./&+01&
Flow of building a program (1 of 2)
6
Pre-processor
Compiler & 
Optimizer
Assembly code: hello.s
C code: hello.cExpanded C code: hello.c
#define TXT “hello”
intmain(void) {
printf(“%s\n”, TXT);
return 0;
}

8
,-&+01&
Flow of building a program (2 of 2)
11
Assembly code: hello.s
Assembler
“as” in Linux.
Linker
“ld” in Linux.
Object code: hello.o
Executable: hello
Static/Dynamic
library

9
23456
(Still...2 of 2) Assembler and Linker
•The assemblerassembles “hello.s” and 
generates an object code “hello.o”
–A step closer to machine code
–Try: “as hello.s–o hello.o”
•The linkerputs together all object files as well as 
the libraries
–There are two kinds of libraries: statically-linkedand 
dynamically-linkedones
12

10
789
Sidetrack: Library files 
•A library file is...
–just a bunch of function implementations.
–for the linker to look for the function(s) that the target C 
program needs. 
13
A bunch of “dot-o” files.
Shared library 
with “.so” file 
extension.
A static library 
with “.a” file 
extension. It is 
also called an 
archive.
.so
.a

11
789
Sidetrack: Library files 
14
.a.so
Linking with static library file.
.o.o
The final 
program is the 
combination of 
the above two 
codes.
The linker only 
checks whether the 
functions usedin 
“.o” files exists in 
the “.so” files or 
not.
A smaller 
program!
Linking with dynamic library file.

12
:;<=>/?89
How to compile multiple files?
•gccby default hides all the intermediate steps.
–Executable: “gcc-o hello hello.c” generates 
“hello” directly.
–Object code: “gcc-c hello.c” generates “hello.o” 
directly.
•How about working with multiple files?
15

13
:;<=>/?89
How to compile multiple files?
16
Step 1.
Prepare all the source files.
Important: there must be 
one and only one file 
containing the main function
.
Step 2.
Compile them into object 
codes one by one.
Step 3.
$ gcc–o prog*.o
Construct the program 
together with all the object 
codes.
$ gcc–c code.c
......
Remember, below shows one of the solution.
*.c
*.o
prog

14
&+'()
Conclusion on “what is a program?”
•A program is just an executable file!
–It is static;
–It may be associated with dynamically-linked files;
•“*.so” in Linux and “*.dll” in Windows.
•It may be compiled from more than one file
17

15
%&0!@
v
操作系统执行作为进程运行的各种程序。
v
进程–正在执行的程序；进程执行必须按顺序进行。
v
多部分构成
§程序代码，也称为文本段
§当前活动，包括程序计数器、处理器寄存器
§包含临时数据的堆栈
•函数参数、返回地址、局部变量
§包含全局变量的数据部分
§包含在运行时动态分配的内存的堆

16
%&0!@
v
程序是存储在磁盘上的被动实体（可执行文件）；进程处
于活动状态
§当一个可执行文件加载到内存中时，程序变成进程
v
通过GUI鼠标单击、命令行输入其名称等启动程序的执行。
v
一个程序可以是多个进程
§考虑多个用户执行同一个程序

17
%&ABCD0EF
Kernel
stack
mmap
heap
.bss
.data
.text
0xFFFFFFFF
|Initialized	global	and	static	vars
|Uninitialized	global	and	static	vars
|Dynamically	allocated	variables
|Program	codes,	global	constants,	
string	literals
|Function	information,	
local	dynamic	vars,	local	constants
(2G)
(1G)
(1G)
|Memory	mapping
|ProcessEnvironment
0xC0000000
0x00000000
Kernel space
user space
0x08048000
(128M)

18
%&0!@

19
v
当进程执行时，它会改变状态
§新建：正在创建进程
§正在运行：正在执行指令
§等待：进程正在等待某个事件发生
§就绪：进程正在等待分配给处理器
§已终止：进程已完成执行
%&0GH

20
%&0GH
5状态进程模型

21
!"#$
Dispatcher
= Running
= Ready
Figure 3.7   Process States for Trace of Figure 3.4
= Blocked
05101520253035404550
Process C
Process B
Process A

22
!"%&'()

23
*+'!"
§
!"#$
%&'()*+,-.*+/0123/456789
%:'();<-.=>?@ABCDEFGH&I
J*-.!"567)D-KLMNO9

24
,*+#$-.
Event
Occurs
New
Suspend
Ready
Blocked
RunningExit
Figure 3.9  Process State Transition Diagram with Suspend States
Admit
(a) With One Suspend State
Suspend
Event Wait
Event
Occurs
Activate
Dispatch
Timeout
Release
Ready/
Suspend
New
Ready
Blocked
RunningExit
Admit
Admit
(b) With Two Suspend States
Event Wait
Event
Occurs
Dispatch
Timeout
Activate
Suspend
Suspend
Activate
Suspend
Release
Blocked/
Suspend
Event
Occurs
New
Suspend
Ready
Blocked
RunningExit
Figure 3.9  Process State Transition Diagram with Suspend States
Admit
(a) With One Suspend State
Suspend
Event Wait
Event
Occurs
Activate
Dispatch
Timeout
Release
Ready/
Suspend
New
Ready
Blocked
RunningExit
Admit
Admit
(b) With Two Suspend States
Event Wait
Event
Occurs
Dispatch
Timeout
Activate
Suspend
Suspend
Activate
Suspend
Release
Blocked/
Suspend

25
/*+#$'!"-.
Event
Occurs
New
Suspend
Ready
Blocked
RunningExit
Figure 3.9  Process State Transition Diagram with Suspend States
Admit
(a) With One Suspend State
Suspend
Event Wait
Event
Occurs
Activate
Dispatch
Timeout
Release
Ready/
Suspend
New
Ready
Blocked
RunningExit
Admit
Admit
(b) With Two Suspend States
Event Wait
Event
Occurs
Dispatch
Timeout
Activate
Suspend
Suspend
Activate
Suspend
Release
Blocked/
Suspend

26
*+!"'01
v
-.PQRSTU9
v
-.VQ<WVQ;<X
Y+,Z[9
v
\I]^-.TUDV
_`abcd=>LM
ABDabVef-.
ghDWVefi-.
12EFGH
v
jkab.lmnopq
GH-UABr"Dst
^-.uvwx+ABy
4z

27
!"*+'()

28
!"234
ProcessorI/OI/O
Figure 3.10  Processes and Resources (resource allocation at one snapshot in time)
I/O
Main
Memory
Computer
Resources
Virtual
Memory
P
1
P
2
P
n

29
!"234
Memory
Devices
Files
Processes
Process 1
Memory Tables
Process
Image
Process
1
Process
Image
Process
n
I/O Tables
File Tables
Figure 3.11  General Structure of Operating System Control Tables
Primary Process Table
Process 2
Process 3
Process n

30
567
vUsed to keep track of both 
main (real) and secondary 
(virtual)  memory
vProcesses are maintained on 
secondary memory using some 
sort of virtual memory or 
simple swapping mechanism 

31
89:7
vUsed by the OS to manage 
the I/O devices and 
channels of the computer 
system
vAt any given time, an I/O 
device may be available or 
assigned to a particular 
process

32
;<7
•existence of files
•location on secondary memory
•current status
•other attributes
These tables provide 
information about:
vInformation may be maintained and used by a file management system
•in which case the OS has little or no knowledge of files
•In other operating systems, much of the detail of file management is managed 
by the OS itself

33
!"7
v
必须进行维护以管理进程
v
必须直接或间接引用内存、
I/O
和文件
v
操作系统必须能够访问表本身，因此需要进行内存管理
操作系统在什么地方创建进程表?

34
Process is going back and forth...
•What happens...
–When the process is calling the system call 
“getpid()”
•Where to get the PID
–PCB (in kernel-space memory)
•The CPU switches from the user-space to 
the kernel-space, and reads the PID
9
System Memory
Kernel-space
memory
User-space
memory
Program 
counter
=>?@ABCD

35
User Mode & Kernel Mode
•Remember this?
11
Another question: How much time was spent in each part?
=>?@ABCD

36
v
fEFGH{|}~ÄÅÇÉ-.Ñ#Ö*Üá9
v
à-.{|â)äãå-.{|Üá9
!"#$%&'()*

37
!"#$%&'()*
v进程状态–正在运行、等待等。
v程序计数器–下一个执行指令的位置
vCPU寄存器–所有以进程为中心的寄存
器的内容
vCPU调度信息-优先级、调度队列指针
v内存管理信息–分配给进程的内存
v记账信息–使用的CPU、启动后经过的
时钟时间、时间限制
vI/O状态信息–分配给进程的I/O设备、
打开的文件列表
与每个进程相关的信息或者进程的表示
（也称为任务控制块）

38
!"#$%+,-
v
!"#$%&'()*+,-./0123
v
45!"#$%6789'()*:;!".:
<=>3
§!"#$%&'()*+,-./0123401564
%7894:;<=>5?*+@ABCD>EFGHI
§JKL!"#$&MNI
v
?@.A&BCDE&FGH
n()OP%&QRA;STUOP=V+,OWTU#
$XYZ[&OP&\9;]^
nOP=V+_`abK%&cdefA;SZ[g!"
#$%&hi*+I

v
Linux中的进程控制块位于source code/include/linux
39
!"#$%&'()*
Demo

40
v
到目前为止，进程只有一个执行线程
v
考虑每个进程具有多个程序计数器
§可以同时执行多个线程（多条指令流）
•多线程控制
v
然后必须在PCB中存储线程详细信息和多个程序计数器
v
在第4章中详细探讨
."

41
/01234!"#$%&'()*
由C结构task_struct表示
  {  pid t_ pid；/*进程标识符*/
   unsigned int time_slice /*调度信息*/
  struct task_struct* 父进程；/*此进程的父级*/
  struct list_head children；/*此进程的子进程*/
   struct *files；/*打开文件列表*/
   struct mm_struct*mm；/*此进程的地址空间*/
   ... }

42
Global
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
Process 
structure
(PCB)
Kernel code 
with system 
calls
26
Relationship between Process Data & PCB
Kernel Space
User space
Process
Invoking system 
calls. E.g., fork(), 
exec*(), wait().
Access 
process’ 
internal

43
!"5678
 
 
User Data 
 The modifiable part of the user space. May include program data, a user stack area, and 
programs that may be modified. 
 
User Program 
 The program to be executed.  
 
Stack 
 Each process has one or more last-in-first-out (LIFO) stacks associated with it. A stack is 
used to store parameters and calling addresses for procedure and system calls.  
 
Process Control Block 
 Data needed by the OS to control the process (see Table 3.5). 
 

44
!"E.FG
 
Process Identification 
 
Identifiers 
 Numeric identifiers that may be stored with the process control block include 
 •Identifier of this process  
 •Identifier of the process that created this process (parent process)  
 •User identifier 
 
Processor State Information 
 
User-Visible Registers 
 A user-visible register is one that may be referenced by means of the machine language that the 
processor executes while in user mode. Typically, there are from 8 to 32 of these registers, although 
some RISC implementations have over 100. 
 
Control and Status Registers 
 These are a variety of processor registers that are employed to control the operation of the processor. 
These include 
 •Program counter: Contains the address of the next instruction to be fetched 
 •Condition codes: Result of the most recent arithmetic or logical operation (e.g., sign, zero, carry, 
equal, overflow) 
 •Status information: Includes interrupt enabled/disabled flags, execution mode 
  
Stack Pointers 
 Each process has one or more last-in-first-out (LIFO) system stacks associated with it. A stack is used 
to store parameters and calling addresses for procedure and system calls. The stack pointer points to 
the top of the stack. 
 

45
 
Process Control Information 
 
Scheduling and State Information 
 This is information that is needed by the operating system to perform its scheduling function. Typical 
items of information: 
 •Process state: Defines the readiness of the process to be scheduled for execution (e.g., running, 
ready, waiting, halted). 
 •Priority: One or more fields may be used to describe the scheduling priority of the process. In 
some systems, several values are required (e.g., default, current, highest-allowable) 
 •Scheduling-related information: This will depend on the scheduling algorithm used. Examples 
are the amount of time that the process has been waiting and the amount of time that the process 
executed the last time it was running. 
 •Event: Identity of event the process is awaiting before it can be resumed. 
 
Data Structuring  
 A process may be linked to other process in a queue, ring, or some other structure. For example, all 
processes in a waiting state for a particular priority level may be linked in a queue. A process may 
exhibit a parent-child (creator-created) relationship with another process. The process control block 
may contain pointers to other processes to support these structures. 
 
Interprocess Communication 
 Various flags, signals, and messages may be associated with communication between two 
independent processes. Some or all of this information may be maintained in the process control 
block. 
 
Process Privileges 
 Processes are granted privileges in terms of the memory that may be accessed and the types of 
instructions that may be executed. In addition, privileges may apply to the use of system utilities and 
services. 
 
Memory Management 
 This section may include pointers to segment and/or page tables that describe the virtual memory 
assigned to this process. 
 
Resource Ownership and Utilization 
 Resources controlled by the process may be indicated, such as opened files. A history of utilization of 
the processor or other resources may also be included; this information may be needed by the 
scheduler. 
!"HIFG

46
!"JKL
vEach process is assigned a unique 
numeric identifier
§otherwise there must be a mapping 
that allows the OS to locate the 
appropriate tables based on the 
process identifier
vMany of the tables controlled by the 
OS may use process identifiers to 
cross-reference process tables
vMemory tables may be organized to 
provide a map of main memory with 
an indication of which process is 
assigned to each region
§similar references will 
appear in    I/O and file 
tables
vWhen processes communicate with 
one another, the process identifier 
informs the OS of the destination of 
a particular communication
vWhen processes are allowed to 
create other processes, identifiers 
indicate the parent and descendents
of each process

47
MNO#$PQ

48
MNO#$PQ

49
RST'UVWX=Y

51
!"#$99:;<=
v
çéèn
v
GHèn
jkl*mI
noPpqrst*m
u/
kl*mI
vwxy=V*m>z
{*mI
z{*m|&}!"#
$z{,}!"#$%
~&#$Ä;I

52
!">?
v
Process scheduler从可用进程中选择下一个在CPU核心上
执行的进程
v
目标——最大限度地利用CPU，快速将进程切换到CPU核心
v
维护进程的调度队列
§就绪队列–驻留在主内存中、就绪并等待执行的所有进
程的集合
§等待队列–等待事件（即I/O）的一组进程
§进程在各种队列之间迁移

53
!">?

54
!"Z7[\

55
!">?@A

56
!"BC

57
CS 423: Operating Systems Design
What’s a ‘real’ CPU?
3
Program 
Counter
Program instructions
Code 
Segment
Offset
Heap
Data 
Segment
Offset
Operand
Data 
Operand
Current
Instruction
OpCode
Stack 
Segment
Offset
Stack
Pointer
Stack
Registers
What’s the STATEof a real CPU?

58
CS 423: Operating Systems Design
The Context Switch
4
Program 
Counter
Program instructions
Code 
Segment
Offset
Heap
Data 
Segment
Operand
Data 
Operand
OpCode
Stack 
Segment
Stack
Pointer
Registers
Stack
Program 
Counter
Program instructions
Code 
Segment
Offset
Heap
Data 
Segment
Operand
Data 
Operand
OpCode
Stack 
Segment
Stack
Pointer
Stack
Save State
(Context)
Load State
(Context)
Registers
!"DEFBC

59
CS 423: Operating Systems Design
CTX Switch: Interrupt
9
Program 
Counter
Program instructions
Code 
Segment
Offset
Stack 
Segment
Stack
Pointer
Stack
Program 
Counter
Program instructions
Code 
Segment
Offset
Stack 
Segment
Stack
Pointer
Stack
Save PC on thread stack
Jump to Interrupt handler
Handler
-Save thread state in thread control block 
(SP, registers, segment pointers, ...)
-Handle Interrupt
-Choose next thread
-Load thread state from control block
-Pop PC from thread stack (return from handler)
-Resume prior task
Thread 
Control 
Block
Thread 
Control 
Block
Registers
Registers
!"DEFBC&GH*

60
!"DEFBCIJ0KLMN
CS 423: Operating Systems Design
10
Program 
Counter
Program instructions
Code 
Segment
Offset
Stack 
Segment
Stack
Pointer
Registers
Stack
Program 
Counter
Program instructions
Code 
Segment
Offset
Stack 
Segment
Stack
Pointer
Registers
Stack
Save PC on thread stack
Jump to yield() function
yield()
-Save thread state in thread control block 
(SP, registers, segment pointers, ...)
-Choose next thread
-Load thread state from control block
-Pop PC from thread stack (return from handler)
Thread 
Control 
Block
Thread 
Control 
Block
Can also CTX Switch from 
Yield

61
OPGH
v
)ê
v
ëí
Øì:î-.uï*ñ
Åó/Z[òï9
%Cô)ê9
%öõúù)ê
%'(ûü
ØC†°
%S-.)êîVe
TU*¢£C†§9
••U¶-.ß®*©™12
 ́ ̈9
•EFGH≠Æ©™1 ́ ̈*
Ø∞.±9
%≤pCD:î≥•U-
.¥μ\∂∑ABD∏
π"-.9
%k≤pCDEFGH*
ÉF&∫ª>©™*º
Ω}EFGH*¥æ9

62
!"QR+ST

63
UVOPG+WXYZ[
v
某些移动系统（如早期版本的iOS）只允许运行一个进程，
其他程序则暂停运行
v
由于屏幕不稳定，iOS提供有限形式的多任务
§单个前台进程-通过用户界面控制
§多个后台进程–内存中，正在运行，但不在显示器上，并
且有限制
§限制包括单个、短任务、接收事件通知、特定的长时间运
行任务（如音频播放）
v
Android运行后台限制较少
§后台进程使用服务执行任务
§即使后台进程挂起，服务也可以继续运行
§服务没有用户界面，内存使用量小

64
!"#$%

65
!"\,
Process Operations
•Process 
–It associates with all the files openedby that process.
–It attaches to all the memorythat is allocated for it.
–It contains every accounting information,
•running time, current memory usage, who owns the process, 
etc.
•You couldn’t operate any things without processes.
30

66
31
Process Operations
•System must provide mechanisms for:
–process identification
–process creation
–program execution
–process termination
•Some basicand importantsystem calls
–getpid()
–fork()
–exec*()
–wait()
–exit()
!"\,

67
!"\,
Process identification
•How can we identify processes?
–Each process is given an unique ID number, and is called 
the process ID, or the PID.
–The system call, getpid(), prints the PID of the calling 
process.
33
$ ./getpid
My PID is 1234
$ ./getpid
My PID is 1235
$ ./getpid
My PID is 1237
#include <stdio.h>   // printf()
#include <unistd.h>  // getpid()
intmain(void) {
printf("My PID is %d\n”, getpid() );
}

68
35
Process Creation
•A process may create several new processes
–Parentprocess: the creating process
–Childrenprocesses: the new processes
•The first process
–The kernel, while it is booting up, creates the first 
process –init.
–The “init” process:
•has PID = 1, and
•is running the program code “/sbin/init”.
–Its first task is to create more processes...
!"]^

69
!"\,
Ø!"']^[\

70
/0123G+!"_

71
`a!"
Process blossoming
•You can view the tree with the command:
–“pstree”; or
–“pstree–A” for ASCII-character-only display.
37
init
SSH 
server
fork()
& exec*()
Shell
top
fork()
& exec*()
fork()
& exec*()
also implies the parent-child relationship.

72
Process blossoming...with orphans?
•However, termination can happen, at any time and in any 
place...
–All the resources are deallocatedto OS when a process terminates
–A process may become an orphan when its parent terminated 
–An orphan turns the hierarchy from a treeinto a forest!
–Plus, no one would know the termination of the orphan.
38
init
SSH 
server
Shell
top
Now, this poor 
process becomes 
an orphan.
`a!"

73
`a!"
Process blossoming...with re-parent!
•In Linux...
–We have the re-parent operation.
–The “init” process will become the step-mother of all 
orphans.
•Well...Windows maintains a forest-like hierarchy.
39
init
SSH 
server
Shell
top
re-parent

74
bc!"de+fO
v
父进程创建子进程，子进程又创建其他进程，形成进程树；
v
通常，通过进程标识符（pid）识别和管理过程；
v
资源共享
§父和子共享所有资源
§子进程共享父进程资源的子集
§父进程和子进程不共享任何资源
v
执行
§父进程和子进程同时并发执行
§父进程等待直到子进程终止
v
地址空间
§ 子进程是父进程的复制品；
§ 子进程加载另一个新的程序；

75
!"+]^
UNIX示例
Øfork（）系统调用创
建新进程
Ø在fork（）之后使用
exec（）系统调用，
用新程序替换进程的
内存空间
Ø父进程调用wait（），
等待子进程终止

76
!"]^
vProcess 
creation is by 
means of the 
kernel system 
call, fork(  )
vThis causes the 
OS, in Kernel 
Mode, to:
1
•Allocate a slot in the process table for the new process
2
•Assign a unique process ID to the child process
3
•Make a copy of the process image of the parent, with the 
exception of any shared memory
4
•Increments counters for any files owned by the parent, 
to reflect that an additional process now also owns those 
files
5
•Assigns the child process to the Ready to Run state
6
•Returns the ID number of the child to the parent process, 
and a 0 value to the child process

77
ghijIN
fork()
•From a programmer’s view, fork()behaves like 
the following:
22
Original execution flow
of a process
The process
invokes fork().
The process splits into two!
Flow of original process
Flow of newly-created process
fork()is called.
fork()returns.
new process
Whatis doing here?
kernel is fork()-ing
The kernel is doing something
secret. What are those things?

78
!"+]^ghijIN
Process creation –fork()system call
•So, how do fork()and the processes behave?
43
intmain(void) {
printf(“Ready(PID = %d)\n”, getpid());
fork();
printf(“My PID is %d\n”, getpid() );
return 0;
}
PID 1234
PID 1235
My PID is 1235
$ _
Process 1234 is the original 
process, and we call it the 
parent process.
Process 1235 is created by 
the fork() system call, and 
we call it the child process.
Why is this line of code executed twice?
$ ./fork_example_1
Ready (PID=1234)
My PID is 1234
会发生什么？

79
Process creation –fork()system call
•So, how do fork()and the processes behave?
44
What do we know so far?
-Both the parent and the child execute the same program before and after fork().
-The child process starts its execution at the location that fork()is returned, not 
from the beginning of the program.
intmain(void) {
printf(“Ready(PID = %d)\n”, getpid());
fork();
printf(“My PID is %d\n”, getpid() );
return 0;
}
!"+]^ghijIN

80
Process creation –fork()system call
45
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
PID 1234
One more example
!"+]^ghijIN

81
Process creation –fork()system call
46
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
PID 1234PID 1235
fork()
One more example
!"+]^ghijIN

82
Process creation –fork()system call
47
$ ./fork_example_2
Before fork ...
PID 1234PID 1235
fork()
Let there be only ONE CPU. Then...
-Only one processis allowed to be executed at one time.
-However, we can’t predict which process will be chosen by the OS.
-By the time, this mechanism is called process scheduling. 
In this example, we assume that the parent, PID 1234, 
runs first, after the fork()call.
Assumption
!"+]^ghijIN

83
Process creation –fork()system call
48
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
PID 1234
(running)
PID 1235
(waiting)
Important
For parent, the return 
value of fork()is the 
PID of the created child.
result = 1235
!"+]^ghijIN

84
!"+]^ghijIN
Process creation –fork()system call
49
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
result = 1235
I’m the parent.
My PID is 1234
program terminated.
PID 1234
(dead)
PID 1235
(waiting)

85
!"+]^ghijIN
Process creation –fork()system call
50
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
result = 1235
I’m the parent.
My PID is 1234
program terminated.
result = 0
PID 1234
(dead)
PID 1235
(running)
Important
For child, the return value 
of fork()is 0.

86
Process creation –fork()system call
51
1  intmain(void) {
2    intresult;
3    printf("before fork ...\n");
4    result = fork();
5    printf("result = %d.\n", result);
6  
7    if(result == 0) {
8      printf("I'm the child.\n");
9      printf("My PID is %d\n", getpid());
10    }
11    else {
12      printf("I'm the parent.\n");
13      printf("My PID is %d\n", getpid());
14    }
15
16    printf("program terminated.\n");
17  }
$ ./fork_example_2
before fork ...
result = 1235
I’m the parent.
My PID is 1234
program terminated.
result = 0
I’m the child.
My PID is 1235
program terminated.
$ _
PID 1234
(dead)
PID 1235
(dead)
!"+]^ghijIN

87
!"+]^ghijIN
Process creation –fork()system call
•fork()behaves like “cell division”.
–It creates the child process by cloningfrom the parent 
process, including...
52
CloneditemsDescriptions
Program code
[File& Memory]
They are sharing the same piece of code.
MemoryIncludinglocal variables, global variables, and dynamically 
allocated memory.
Opened files
[Kernel’s internal]
If the parent has openeda file “A”, then the child will also have 
file “A” opened automatically.
Programcounter
[CPU register]
That’s whythey both execute from the same line of code 
after fork() returns.

88
!"+]^ghijIN
Process creation –fork()system call
•However...
–fork()does not clone the following...
–Note: they are all data inside the memory of kernel.
53
Distinct itemsParentChild
Returnvalue of fork()
PID of thechild process.0
PIDUnchanged.Different,not necessarily be 
“Parent PID + 1”
ParentprocessUnchanged.Doesn’t have the same parent 
as thatof the parent process.
Running timeCumulated.Just created, so shouldbe 0.

89
fork()in action –the start...
27
OS Kernel
Process
1234
Process
345
PID = 1234
Running time
Array of opened files
PID = 1234
Running time
Array of opened files
copying
Inside kernel, processes are 
arranged as a doubly linked 
list, called the task list.
Q: What is each node?
This guy invoked 
fork().
_`abCDcAdcef?A`d

90
fork()in action –kernel-space update
28
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
PID = 1235
Running time
Array of opened files
This guy invoked 
fork().
Process
345
reset to 0.
updated.
preserved.

91
fork()in action –kernel-space update
29
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
PID = 1235
Running time
Array of opened files
This guy invoked 
fork().
Process
345
List of childrenPointer to my parent
reset to 0.
updated.
preserved.
updated.
Add a new 
child.

92
fork()in action –kernel-space update
30
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
PID = 1235
Running time
Array of opened files
This guy invoked 
fork().
Process
345
List of childrenPointer to my parent
A new node is 
introduced.
reset to 0.
updated.
preserved.
updated.
Add a new 
child.

93
fork()in action –user-space update
31
OS Kernel
Process
1234
This guy invoked 
fork().
Process
1235
Process
345
What happened 
to user space?

94
fork()in action –user-space update
32
OS Kernel
Process
1234
This guy invoked 
fork().
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
Process
1235
Process
345
What happened 
to user space?

95
fork()in action –user-space update
33
OS Kernel
Process
1234
This guy invoked 
fork().
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
copying
Process
1235
Process
345

96
fork()in action –finish
34
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
PID = 1235
Running time
Array of opened files
Ready to return 
from fork()
List of childrenPointer to my parent
Return value = 1235Return value = 0
Process
1235
Process
345
Ready to return 
from fork()

97
ghijIN"klm&]^nopq+!"*
Demo

98
rghijIN"klm&]^nost+!"*

99
uvw01Mhxy4z'{]^|}+!"

100
!"]^d~
vAfter creating the process the Kernel can do one of the 
following, as part of the dispatcher routine:
§stay in the parent process
§transfer control to the child process
§transfer control to another process

101
"k+:;
Program execution
•execl()–a member of the execsystem call 
family (and the family has 6 members).
56
intmain(void) {
printf("before execl...\n");
execl("/bin/ls", "/bin/ls", NULL);
printf("after execl...\n");
return 0;
}
$ ./exec_example
before execl...
Arguments of the execl()call
1
st
argument: the program name, “/bin/ls” in the 
example.
2
nd
argument: 1
st
argument to the program.
3
rd
argument: indicate the end of the list of arguments. 

102
"k+:;
Program execution
•execl()–a member of the execsystem call 
family (and the family has 6 members).
61
intmain(void) {
printf("before execl...\n");
execl("/bin/ls", "/bin/ls", NULL);
printf("after execl...\n");
return 0;
}
$ ./exec_example
before execl...
exec_example
exec_example.c
GUESS:
What happens next?

103
"k+:;
Program execution
•execl()–a member of the execsystem call 
family (and the family has 6 members).
62
$ ./exec_example
before execl...
exec_example
exec_example.c
$ _
intmain(void) {
printf("before execl...\n");
execl("/bin/ls", "/bin/ls", NULL);
printf("after execl...\n");
return 0;
}
WHAT?!
The shell prompt appears!
The output says:
(1)The gray code block is not reached!
(2)The process is terminated!
WHY IS THAT?!

104
"k+:;
Program execution
•The execsystem call family is not simply a function 
that “invokes” a command.
63
intmain(void) {
printf("before execl...\n");
execl("/bin/ls", "/bin/ls", NULL);
printf("after execl...\n");
return 0;
}
Process
Originally, the process is executing the 
program “exec_example”.

105
Program execution
•The execsystem call family is not simply a function 
that “invokes” a command.
64
intmain(void) {
printf("before execl...\n");
execl("/bin/ls", "/bin/ls", NULL);
printf("after execl...\n");
return 0;
}
Process
The execl() call changes the execution from 
“exec_example” to “/bin/ls”
/* The program “ls” */
intmain(intargc, char ** argv)
{
......
exit(0);
}
"k+:;

106
"k+:;
Program execution
•The execsystem call family is not simply a function 
that “invokes” a command.
65
Process
The “return” or the “exit()” 
statement in “/bin/ls” will terminate 
the process...
Therefore, it is certain that the process 
cannot go back to the old program!
/* The program “ls” */
intmain(intargc, char ** argv)
{
......
exit(0);
}

107
"k+:;
Program execution -observation
•The process is changing the code that is executing and never 
returns to the original code.
–The last two lines of codes are therefore not executed.
•The process that calls any one of the member of the exec 
system call family will throw away many things, e.g.,
–Memory: local variables, global variables, and dynamically 
allocated memory;
–Register value: e.g., the program counter;
•But, the process will preservesomething, including:
–PID;
–Process relationship;
–Running time, etc.
66

108
exec*()
•How about the exec*()call family?
38
exec*()is called.
The kernel is doing something
secret. What are those things?
The process returns to user-space
but is executing another program.
Process
Old 
code
New 
code
e.g., execl("/bin/ls", "/bin/ls", NULL);

109
exec*()in action –the start...
39
OS Kernel
Process
1234
Process
345
PID = 1234
Running time
Array of opened files
The kernel searches the target 
program file.
If it is not found, the process returns 
from the system call.
Let’s assume that it can be found.
This guy invoked 
exec*().
Searching

110
exec*()in action –the end
40
OS Kernel
Process
1234
Process
345
This guy invoked 
exec*().
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
What happens to the 
user-space memory

111
exec*()in action –the end
41
OS Kernel
Process
1234
Process
345
This guy invoked 
exec*().
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
Cleared!
Cleared!
Reset based 
on the new 
code!
Changed to 
the new 
program code!

112
exec*()in action –the end
42
OS Kernel
Process
1234
Process
345
This guy invoked 
exec*().
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
The kernel code updates the 
content on the user-space memory.
Also, registers’ values, such as the 
program counter, will also be reset.
Demo

113
hijIN4Ä4K3KÅÇIN
When fork()meets exec*()...
•The mix can become:
–A shell,
–The system()library call, etc...
68
Execute
command
Switch to 
target program
Terminate
Resume
Parent
Child
类似于：
Demo

114
hijIN4Ä4K3KÅÇIN
fork() + exec*() = system()?
69
1  intsystem_test(constchar *cmd_str) {
2      if(cmd_str== -1)
3          return -1;
4  if(fork() == 0) {
5 execl(cmd_str, cmd_str, NULL);
6 fprintf(stderr, 
"%s: command not found\n", cmd_str);
7 exit(-1);
8 }
9 return 0;
10 }
11 
12 intmain(void) {
13 printf("before...\n\n");
14 system_test("/bin/ls");
15 printf("\nafter...\n");
16 return 0;
17 }
$ ./system_implement_1
before...
system_implement_1
system_implement_1.c
after...
$ _
Is this the 
only result?

115
hijIN4Ä4K3KÅÇIN
fork() + exec*() = system()?!
70
1  intsystem_test(constchar *cmd_str) {
2      if(cmd_str== -1)
3          return -1;
4 if(fork() == 0) {
5 execl(cmd_str, cmd_str, NULL);
6 fprintf(stderr, 
"%s: command not found\n", cmd_str);
7 exit(-1);
8 }
9 return 0;
10 }
11 
12 intmain(void) {
13 printf("before...\n\n");
14 system_test("/bin/ls");
15 printf("\nafter...\n");
16 return 0;
17 }
$ ./system_implement_1
before...
after...
system_implement_1
system_implement_1.c
$ _
Some strange cases 
happened when the 
program is 
executed 
repeatedly
!! Why?

116
hijIN4Ä4K3KÅÇIN
fork() + exec*() = system()...
71
1  intsystem_test(constchar *cmd_str) {
2      if(cmd_str== -1)
3          return -1;
4 if(fork() == 0) {
5 execl(cmd_str, cmd_str, NULL);
6 fprintf(stderr, 
"%s: command not found\n", cmd_str);
7 exit(-1);
8 }
9 return 0;
10 }
11 
12 intmain(void) {
13 printf("before...\n\n");
14 system_test("/bin/ls");
15 printf("\nafter...\n");
16 return 0;
17 }
Let’s re-color the program!
Parent process
Child process
Both processes
$ ./system_implement_1
before...
after...
system_implement_1
system_implement_1.c
$ _
$ ./system_implement_1
before...
after...
system_implement_1
system_implement_1.c
$ _

117
hijIN4Ä4
K3KÅÇINÉÑyÖyÜKáIN
fork() + exec*() = system()...
72
ParentChild
fork()
Parent
then
Expected execution 
sequence.
$ ./system_implement_1
before...
after...
system_implement_1
System_implement_1.c
$ _
$ ./system_implement_1
before...
system_implement_1
System_implement_1.c
after...
$ _
Possible execution 
sequence.
Parent
Child
Parent
then
fork()

118
fork() + exec*() = system()...
•Don’t forget that we’re trying to implement a system()-
compatible function...
–It is very weird to allow different execution orders.
•How to let the child to execute first?
–But...we can’t control the process scheduling of the OS to 
this extent.
•Then, our problem becomes...
–How to suspendthe execution of the parent process?
–How to wakethe parent up after the child is terminated?
74
hijIN4Ä4
K3KÅÇINÉÑyÖyÜKáIN

119
fork()+ exec*() + wait() = system()
75
1  intsystem_test(constchar *cmd_str) {
2      if(cmd_str== -1)
3          return -1;
4 if(fork() == 0) {
5 execl("/bin/sh", "/bin/sh",
"-c", cmd_str, NULL);
6 fprintf(stderr, 
"%s: command not found\n", cmd_str);
7 exit(-1);
8 }
9      wait(NULL);
10 return 0;
11 }
12 
13 intmain(void) {
14 printf("before...\n\n");
15 system_test("/bin/ls");
16 printf("\nafter...\n");
17 return 0;
18 }
What is the 
output now?

120
fork()+ exec*() + wait() = system()
76
1  intsystem_test(constchar *cmd_str) {
2      if(cmd_str== -1)
3          return -1;
4 if(fork() == 0) {
5 execl("/bin/sh", "/bin/sh",
"-c", cmd_str, NULL);
6 fprintf(stderr, 
"%s: command not found\n", cmd_str);
7 exit(-1);
8 }
9      wait(NULL);
10 return 0;
11 }
12 
13 intmain(void) {
14 printf("before...\n\n");
15 system_test("/bin/ls");
16 printf("\nafter...\n");
17 return 0;
18 }
$ ./system_implement_2
before...
system_implement_2
System_implement_2.c
after...
$ _
The parent is 
suspended until 
the child 
terminates

121
!"àâxä0ÜIN
wait()–properties explained
•The wait()system call suspendthe calling parent 
process (Case 1).
•When to wake up?
–wait()returns and wakes up the calling process when 
the one of its child processes changes from running to 
terminated.
77
wait()
fork()
Terminate
wake up
Case 1.
Parent is 
suspended.

122
!"àâxä0ÜIN
wait()–properties explained
•What happens if
–There were no running children;
–There were no children;
•wait()does not suspend the calling process 
(Case 2) 
78
wait()
fork()
Terminate
Case 2.
no suspension
is needed.

123
!"àâxä0ÜIN
More powerful wait()?
•Limitation of wait()?
–waits for any one of the children
–Detect child termination only
•How to wait for a particular process?
–waitpid()
80

124
xä0ÜIN4ãå4wä0Üç0MIN
wait()VS  waitpid()
81
wait()waitpid()
Wait for any one of the children.Dependingon the parameters, 
waitpid()will wait for a particular 
child only.
Detect child termination only.
Depending on the parameters,
waitpid()
can detect child’s status 
changing
: 
-from running to suspended, and
-from suspended to running.
For more details, you must read the man pages of wait()and waitpid().

125
