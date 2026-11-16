# 2024-OS-CH-4-进程-part1-图集-矢量

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part1-图集-矢量.pdf　｜　11,213 字符　｜　73 个片段　｜　更新于 2026-11-13
#define TxT "hello" int main(void){ printf("%s\n",TxT);

Astaticlibrary ar with".a"file calee alledan A bunch of "dot-o" files. .a archive.

gcc -shared A bunch of"dot-o" files.

.SO cateckswhetherthe functions used in the" so"files or not. A smaller program! p

.a O [Taef nal cYambinationof theabove two codes.

codes one by one. Construct the program cYaaeteeewit allte bjecl 13

gcc code.c I

$8 gcc-oprog 0

Step 1. *.C

Kernel } ( (1G)  stack (1G)- mmap heap (2G) .bss .data .text A (1 17

Figure 3.9 Process State Transition Diagram

c态模型 Admit Dispatch New Ready Runni spend Blocked

New     Suspend Activate Dispatch eady/ ispend Ready Running Suspend Timeout Activate ocked/

學计算机学院

Processor I/O I/O I/O Memory

Process cYmage Memory Tables Process Memory 1 Devices I/O Tables Files Processes File Tables Primary Process Table Process1 Process Process3 Image Process n Process

These tables provide information about: • existence of files • location on secondary memory !damurrentstatus

alling the system call memory nemory m I the user-space to YaadsthePlD inted cae mi7 ernel-space memory Program counter

The modifiable part of the user space. May include program data, a user stack area, and programs that may be modified. UserProgram The program to be executed. Stack Each process has one or more last-in-first-out (LIFO) stacks associated with it. A stack is used to store parameters and calling addresses for procedure and system calls. Process Control Block Data needed by the OS to control the process(see Table 3.5).

Code Data Segment Segment Program 11 Counter ffset OpCode Operand Instruction Da Operand

K ment E er

Registers

Registers Code Data Sgment Sgment Stack Sgment Counter [apCode 0erand Pointer Registers Code Data Stack Segment Segment Segment Off set Program Counter Stack OpCode Operand Pointer Data Operand Program instructions Heap Stack 58

[Ya Handler auoDCot ood ctocl

Con threadstack Save thread statein thread control block o Interrupt handler Handle Interrupt Choose nextl thread  Load i I thread state from control block Thread  Pop PC from thread stack (return from handler) Control Resume prior task lock

Code Stack Segment Segment Offset Program Counter Stack Pointer Program instructions Stack

yieldO Saye DC on. thread stack

o yield) function Save thread state in i I thread control block (SP, registers, segment pointers, …..) Choose nextt thread Load thread state :from controlll block  .Pop PC from thread stack (return from handler) Thread Control Block

Code Stack Segment Segment Offset Program Counter Stack Pointer Program instructions Stack

#include <stdio.h> //I printf() #include <unistd.h> //: getpid() int main(void){ printf("My PID is ;%d\n",  getpid() );

also implies the parent-child relationship. init SSH forkC) Server Shell & exec*() fork() & xec*() fork() top & exec*() 71

init Now, this poor process becomes anorphan. SSH → server shell

[Taaae SSH  → server shell 1 top

far ( is alled. 1 fork() returns. … Originalexecution flow  new process cYaY a The process invokes fork )). W hat is doing here? kernel is fork()-ing Ylaw of originalprocess  The kernel is doing som ething secret.What are those things? 77

./fork_eXample_l PID1234 Ready (PID=1234) My PID is 1234 [M PID is 123 $ PID1235 int main(void) { Process 1235 is created by printf("Ready (PID = %d)\n", getpid()); the fork()  ) system call, and forkO : wecall itthe printf("My PID is %d\n", getpid()); 78 会发生什么？

int main(void) printf( Ready (PID = %d)\n", getpid()); fork(; printf("My PID : is %d\n", getpid()); return 0; Weatdo ekno sofar -Both thel parent and the child execute the same program before and after fork(). -The( child process starts itsi ; execution at the location that fork () is returned, not from the beginning of the program.

1 int main(void) { 2 intresult; printf("beforefork...\n"); 4 result = fork(); 5 printf("result = %d.\n", result); 7 if(result == 0){ 8 printf("I''mthechild.\n"); printf("programterminated.\n");

1 : int main(void) { [2b intresult; printf("beforefork...\n"); result=fork(); 5 printf("result= %d.\n",result); 7 if(result == 0) { 8 printf("Imthechild.\n"; printf("programterminated.\n");

Let there be ( only ONE CPU. Then. -)nly one process is allowed tol be executed atone time. However, we( can''t predict which process will be( chosen bythe Os. -By the time,thisI mechanism is called process scheduling. In this example,we assume that the parent, PlD 1 1234, runs first, after  the fork() call.

1 int main(void){ [2b intresult; 3 printf("before fork ...\n"); 4 result=forkO; 5 printf(result=%d.\n",result); lse{ 7 if(result == 0) { .") rintf("I IDis n" i(); pr int thechi theparent "; printf( IDis " pi ) intf("M f("I'' c printf(programterminated.\n");

$ ./fork example_2 before fork result = 1235 For parent, the return value of fork ) is the PID of the created child.

1 intmain(void){ 2 int result; 3 printf("before fork ...\n"); 4 result =fork(); 5 printf("result = %d.\n", result); 9 8 if(result==0){ pri "I''m thechild.\n"); } printf("My PID is %dn",,getpid(); ntf( ElsE{ printf("I''m the parent.\n"); printf("My PID is %d\n",getpid()); printf("program terminated.\n"); 17

1 int main(void){ [2b _ntresult; 3 printf("before fork ...\n"); 4 result=forkO; printfresult=%din result; 6 7 if(result == 0) { 8 printf("I''m the child.\n"); 9 printf("MyPIDis%d\n",getpid)); 1O J 11 else { 12 13 printf("I''m the parent.\n"); printf("My PID is %d\n", getpid()); 14 15

$ ./fork example_2 before fork result = 1235 I''m thep arent. My PID is 1234 program terminated. result = 0 For child, the return value of fork()is0.

1 intmain(void){ intresult; 3 printf("before fork ...\n"); 4 result = fork(); 5 printf("result = %d.\n",result); 9 8 } 7 if(result == 0) { { the child. n"); printf("My else PID is%din",getpid();; prin tf("I''m printf("I''m theparent.\n"); printf("My PID is %d\n", getpid());  6 printf("program terminated.\n"); [17

Program code They are sharing the same piece of code. Memory Including localvariables,globalvariables,anddynamically Opened files If the parent has opened a file "A", then the child will also have [Kernel''s internal] file“A" opened automatically. Program counter That''s whytthey k both execute from the S sam e ) line of code [CPuregisterl after fork( returns.

Return value of fork() PlD of the child process. 0 PlD Unchanged. Different,notnecessarily be Parent process Unchanged. Doesn''t have the same parent canaa e Running time Cumulated. Just created, so should be 0.

This guy invoked OS Kernel fork(). Ynside kernelprocessesare arranged asa_l doublylinked Process list calledthetasklist. Process 1234 345 Q:Whatiseachnode? PID 1234 PID 1234 nning time Running time COPYINg Array of opened files Array of opened files

This guy invoked OS Kernel fork ). Process ) Process → 45 [a2a4 と PID 1234    updated. Running time reset to0. Array of opened files Array of opened files preserved.

[Thas guy invoked Os Kernel fork(). Process ) Process  [1234 45 PID 1234    updated. Running time resettoO. Array of opened files Array of opened files preserved. updated. Add a new child.

Anewnodeis Thisguy invoked OS Kernel introduced. fork(). Process Process Process 1234 345 1235 PID 1234    updated. Running time reset to0. Array of opened files Array of opened files preserved. updated. Add a new child.

Thisguyinvoked OsKernel fork(. Process Process Process   1234 345 1235

What I happened to user space?

fork(). Process  Process Process → 1234 345 1235 Variable allocated What happened [Yama m to user space? Global = Code + variable constants

This guy invoked Os Kernel fork(. Process Process Process  1234 345 1235 Local Dynamically- Local Dynamically- allocated allocated Variable variable [Ymemory memory opying Global 三 Code Global 三 + Code + variable constants variable constants

Ready to return Ready 0s Kernel to return from forkC) from 1 forkC Process 1 Process Process  345 1234 1235 PID 1234 PID 1235 Running time Running time Array of opened files Array of opened files List of children Pointer to my parent

-

intI main(void){ $ ./exec_example printf("before execl ·..\n"); execl("/bin/ls","/bin/ls", NULL); printf("after execl ·..\n"); Arguments of the execlO call Returnl 0; } 1st argument:the program name,"/bin/ls"in the cYaaml. 3rd argument: indicate the end I of the list of arguments.

int main(void){ $ ./exec_ _example before execl printf("before execl ...\n"); 2 exec_example exec_example.c execl("/bin/ls","/bin/ls",NULL); [aa_ntf( after execl .. in"); Return O; [Yab [Wa a

$ ./exec_example before xecl exec example exec example.c

Tammmy(aiTu tie Taimmiy Taso TeTmbeis [Wa The shell prompt appears! int I main(void) { $ ./exec_example before execl printf("before execl ...\n"); exec_ example exec_example.c execl("/bin/ls","/bin/ls",NULL); $ printf("afterexecl ...\n"); return 0; The 0utput says: (1) The gray code blockis not reached! WHYISTHAT?! 103

intl main(void){ printf("before execl ...\n"); execl("/bin/ls", "/bin/ls",NULL); printf("after execl ...\n"); Originally, the r process is executing the program “exec_example".

The rogram ls" int main(void){ int main(int argc, char**argv) printf(before execl ...in); exit(); execl("/bin/ls" /bin/ls" NuL); printf("after execl ...\n"); The execl 6 callchanges  theexecutionfrom   "exec_example"to “/bin/ls"

int main(int argc, char argv) The “return" orthe “exit ()" { statement in “ /bin/ls" will terminate exit(e); Therefore, it is certain that the process cannot go back to the old program!

cYaaaal The process returns to user-space but is executing another program. old code New code Process The kernel is doing something secret. What are those things?

exec*a Process D Process 1 45 1234 Local Dynamically- variable allocated memory What happens to the Global 三 user-space memory code + variable constants

This guy i invoked OS Kernel exec*(). 345 1234 [Yariable allocated Cleared! Global 三 Code + Reset based variable constants Changed to program code!

exec*(. Process Process 1234 345 Local Dynamically- The kernel code updates the variable allocated cmemory content on the user-space memory. Global = Code + Also, isters''values, such as the reg variable constants program counter, will also be reset.

[Yxecute command Resume PareNt I T Child Switch to Terminate target program

1 int system_test(const char *cmd_str) { if(cmd_str == -1)  3 return -1; 4 if(fork() == e 0) {  5 execl("/bin/sh", "/bin/sh", "-c", cmd_≤ , _str, NULL); fprintf(stderr "%s: command not · found\n",cmd_str); 7 exit(-1); 8 10 return O; 11 12 13 int main(void) { What is the 14 printf("before...\n\n"); 15 system_ _test("/bin/ls"); output now? ae printf("inafter...\n"); 17 return 0; 18 }

int : system_test(const char *cmd_str) {  if(cmd_str == -1) 3 return -1; 4 if(fork() == 0) { 5 execl("/bin/sh", .   "/bin/sh",    "-c", cmd_str,NuLl); "%s: command notfound\n",cmd_str);  7 exit(-); 8 9 Wait(NULL); 10 return 0; 11 } $./system_implement_2 12 before... 13 int main(void) { 14 system_implement_2 printf("before...\n\n"); System_implement_2.c 16 printf("\nafter...\n"); after..· 17 return 0; $ 18 }

wait() wake up suspended. fork() Terminate Case.

CASEZ. WaIt() nosuspension YGnaneeded fork() Terminate
