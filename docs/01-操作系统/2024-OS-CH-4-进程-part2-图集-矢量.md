# 2024-OS-CH-4-进程-part2-图集-矢量

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part2-图集-矢量.pdf　｜　5,088 字符　｜　26 个片段　｜　更新于 2026-11-13
wait() wait() is called. returns. waitC blocksthe cYaarent. Parent 1 Ofcourse, the kernel coordinates the series of events. But, Child child Process what on earth is going on? Child is terminated through the exit () system call.

Child Parent Process 一 Process  1234 1235

This guy invoked OS Kernel exit(). Child Parent Process Process   1234 1235 C Y PID 235 I Running time What changes will be made      for the PCB?

This guy invoked OSKernel exit(). Child Parent Process Process  + 1234 1235 The kernel frees all the allocated memory. PID 235 = Running time E.g.,the list of opened files areallosed. before exit() may be safe .

This guy invoked OS Kernel exit(). Parent Child 1235 1234 Then, the kernel removes everything on the user- aacememory about the cancenednrocess Local Dynamically including program code allocated Variable and allocated memory. memory Globa l Code + Rememberthat kernel is Variable constants invincible

M个HH This guy invoked OS Kernel exit(). Child Parent Process Process   1234 1235 What is next? HoW about permanently removing the child?   Local  e variable allocated memory Global Code + Variable constants

Pant 3

Removed from the process table immediately? Not really! Why?

This guy invoked OS Kernel exit(). Parent Child Process Process 1234 1235  1 i Remain the entry of the child in the process table PID = 1235 aterminated state) Running time Array of opened files Resources Deallocate

This guy invoked OS Kernel exit(). Parent Child Process Process  → 1234 1235 C 7 The child is now called zombie. PID = 1235 Its storage in the kernel-space Running time memory is kept to a minimum Array of opened files The PID (1235 in this example) and process structure are awned by thechild

b This guy invoked OS Kernel exit(). Parent Child 1234 1235 / SiGCHLD HowtoWake up parent? The kernel notifi es the parent of the child process about the PID = 1235  termination of its child. Running time Array of opened files The notificationisa signalcalled SIGCHLD.

exit() Although the child is still in the exit() is returns. system, it is no longer running. calld. Thereisno program code!!l It turns into a mindless zombie... You cannot kill a zombie process, asitis (1) (2)(3) already dead. Then how to eliminate it? 1

Step (1) Clean up m ost of the allocated kernel-space m emory. Stee ( eNotify theparent withSlGCHlD.

How to proceed wait() wait() with waitO? Now, it is trivial to is called. returns. wait( I I signalis the trick! locsthe howto But SIGCHLD Child Child Process c

This guy invoked wait(). How to handle SIGCHLD? OS Kernel Child Parent Process Process  → 1234 1235 i 1 Theernelsetsasignal andlingroutine PID 1234 Running time That signal handling routine will be executed when SlGCHlD comes.

wait(). Parent Child Process Process  [1234 235 to the :: SIGCHLD signal (the signal handlers  aD 1234 I aresetc 0nly whenwait iscalled. [Waat ithe arentis executing at ertasks Array of opened files (not call the'':w ait() system call) wheni child terminates (see the2nd caseofwaitelater)?

wait(). Parent Child 1234 1235 PID = = 1234 Running time Theformalwayosay Array of opened files    until. Signal handlers Guess: when to wake up?

This guy invoked OS Kernel wait(). Child Parent Process Process  → 1234 1235 Y PID=1234 SIGCHLD from Running time 1235 2 Signal hand ler s

This guyinvoked OSKernel wait(). Parent Child Process Process 1234 1235 When SIGCHLD comes, the signal handlingl PID routine i: is invoked! = 1234 SIGcHLD from Running time 1235 Note: since the parent is still inside Array of opened files the: :system i Icall, instead of the  original program code, the parent Signal handlers process is still blocked in some * sense

This guy invoked wait (). Now, the child is truly OS Kernel dead. Parent a Process Pro Ss 1234  1235   Default Handling of SIGCHLD PID 1234 I TGCH Running time 1 235 SIGCHLD; Array of opened files 2. Destroy the child process Signal handlers that sendsher the signal.

Readytoreturn OsKernel from wait(). Parent Process Thesignalhandleristhenremoved,i.e.,the 1234 process is ignoring SIGCHLD again. going back to the user space. PID = 1234 So, tlookslike waiteisreturned fromits Running time invocation". aeray of openedfiles Thisisthereasonwhywaitesystemcallwaits Signee- ers for any one of the child processes.

Ready to return OS Kernel from wait(). Parent 1234 PID 1234 Running time Array of opened files thePID of the terminated Ichild.

wait() wait() wait() blocks the parent. Parent 1 SIGCHLD Child Child So, the child will be Process X given a clean death by the wait() Child is terminated through the exit() system call. system call.

smge) kernel Processe Processl shmat() sharedmemory shmat() /  n shared memory  stared memory

N SUN YAT-SEN UNIVERSITY SCHOOL OF COMPUTER SCIENCE AND ENGINEERING Linux: Pipes #include <stdlib.h> 普通管道 #include <unistd.h> /*ablockingreadversion */ #include <sys/wait.h> int main(void) #define  READEND 0 { #define WRITE END 1

(i  Linux的RPC编译工具：rpcgen  版本编号 programTesTpROG{ versionVERSION string TEST(string) = 1; }=1 }=1  编译接口文件：rpcgenrpc_test.x RPC函数
