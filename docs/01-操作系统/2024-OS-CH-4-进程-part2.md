# 2024-OS-CH-4-进程-part2

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part2.pdf　｜　52,158 字符　｜　125 个片段　｜　更新于 2026-11-11


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
#$%&
v
进程执行最后一条语句，然后调用exit（）系统调用请
求操作系统删除它。
§将状态数据从子进程返回到父进程（通过wait（））
§进程的资源由操作系统释放
v
父进程可以使用abort（）系统调用终止子进程的执行。
这样做的一些原因：
§子进程已超出分配的资源
§不再需要分配给子进程的任务
§父进程正在退出，如果父进程终止，操作系统不允许子
进程继续
Demo

5
#$%&
v
如果其父进程已终止，则某些操作系统不允许子进程存在。
如果进程终止，则其所有子进程也必须终止。
§级联终止。所有子女、孙辈等均被终止。
§终止由操作系统启动。
v
父进程可以使用wait（）系统调用等待子进程的终止。该
调用返回终止进程的状态信息和pid
pid=等待（&状态）；
v
若并没有父进程等待（并没有调用wait（）），那个么进
程就是一个僵尸
v
若父进程在未调用wait（）的情况下终止，则该进程是孤
立进程

6
#$%&
intmain(void)
{
intcount = 1;
pid_tchildpid, terminatedid;
childpid= fork(); /* child duplicates parent’s address space */
if (childpid< 0) {
perror("fork()");
return EXIT_FAILURE;
}
else
if (childpid== 0) { /* This is child pro */
count++;
printf("child pro pid= %d, count = %d (addr= %p)\n", getpid(), count, 
&count); 
printf("child sleeping ...\n");
sleep(5); /* parent wait() during this period */
printf("\nchildwaking up!\n");
}
else { /* This is parent pro */
terminatedid= wait(0);
printf("parent pro pid= %d, terminated pid= %d, count = %d (addr= %p)\n", 
getpid(), terminatedid, count, &count);
}
printf("\nTestingpoint by %d\n", getpid()); /* executed by child and parent */
return EXIT_SUCCESS;
}
Demo

7
#$%&'()*+,-./)*+,0123
46
wait()and exit()ʹTime Analysis
Child 
Process
Parent
Process
wait()
is called.
Parent
Child
wait()
blocksthe 
parent.
Child is terminated through 
the exit()system call.
wait()
returns.
Of course, the kernel 
coordinates the 
series of events. But, 
whaton earth is 
going on?

8
48
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
Child
Parent
#$%&'()*+,-./)*+,456#$789:

9
#$%&'()*+,-./)*+,456#$789:
49
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
PID = 1235
Running time
Array of opened files
This guy invoked 
exit().
Child
Parent
What changes will be made 
for the PCB?

10
#$%&'()*+,-./)*+,456#$789:
50
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
PID = 1235
Running time
Array of opened files
The kernel frees all the 
allocated memory.
E.g., the list of opened files 
are all closed. 
This guy invoked 
exit().
Child
Parent
dŚĂƚ͛ƐǁŚLJŶŽƚĐĂůůŝŶŐfclose()
before exit()ŵĂLJďĞƐĂĨĞ͙

11
#$%&'()*+,-./)*+,456#$789:
51
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
This guy invoked 
exit().
Child
Parent
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
Then, the kernel removes 
everything on the user-
space memoryabout the 
concerned process, 
including program code 
and allocated memory.
Remember that kernel is 
invincible 

12
#$%&'()*+,-./)*+,456#$789:
52
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
This guy invoked 
exit().
Child
Parent
Global 
variable
Local 
variable
Dynamically-
allocated 
memory
Code + 
constants
¾What is next?
How about permanently removing the child?

13
#$%&'()*+,-./)*+,456#$789:
53
wait()and exit()ʹchild side
OS Kernel
Process
1234
Parent
Removed from the process table immediately?
Not really! Why?

14
#$%&'()*+,-./)*+,456#$789:
54
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
PID = 1235
Running time
Array of opened files
This guy invoked 
exit().
Child
Parent
Remain the entry of the 
child in the process table
(terminated state)
Resources?
Deallocate

15
#$%&'()*+,-./)*+,456#$789:
55
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
PID = 1235
Running time
Array of opened files
The child is now called zombie.
Its storage in the kernel-space 
memory is kept to a minimum
The PID(1235 in this example) 
and process structure are 
owned by the child
This guy invoked 
exit().
Child
Parent

16
#$%&'()*+,-./)*+,456#$789:
56
wait()and exit()ʹchild side
OS Kernel
Process
1234
Process
1235
PID = 1235
Running time
Array of opened files
The kernel notifies the parent of 
the child processabout the 
termination of its child.
The notification is a signalcalled 
SIGCHLD.
This guy invoked 
exit().
Child
Parent
SIGCHLD
How to wake up parent?

17
#$%&;/)*+,
A short summary for exit()
58
Step (1) Clean up most of the allocated kernel-space memory.
Step (2) Clean up all user-space memory.
Step (3) Notify the parent with SIGCHLD.
exit()is 
called.
(1)(2)(3)
exit()
returns.
Although the child is still in the 
system, it is no longer running. 
There is no program code!!!
It turns into a mindless zombie͙
You cannot kill a zombie process, as it is 
already dead. Then how to eliminate it?

18
#$%&'()*+,<./)*+,
59
wait()and exit()ʹthey come together!
Child 
Process
Parent
Process
wait()
is called.
Parent
Child
wait()
blocksthe 
parent.
Child is terminated through 
the exit()system call.
wait()
returns.
Now, it is trivial to 
see that SIGCHLD
signal is the trick!
But, how to 
handle SIGCHLD?
SIGCHLD
How to proceed 
with wait()?

19
#$%&='()*+,<./)*+,45>#$789
60
wait()and exit()ʹparent side
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
The kernel sets a signal handling routine
(and it is a function pointer) to the process.
That signal handling routine will be executed 
when SIGCHLDcomes.
This guy invoked 
wait().
Child
Parent
Signal handlers
When SIGCHLD comes, please handle it.
How to handle SIGCHLD?

20
#$%&='()*+,<./)*+,45>#$789
61
wait()and exit()ʹparent side
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
By default, every process does not respond 
to the SIGCHLDsignal (the signal handlers 
are set only when wait() is called). 
What if the parent is executing other tasks 
(not call the wait() system call) when child 
terminates (see the 2
nd
case of wait() later)?
This guy invoked 
wait().
Child
Parent
Signal handlers

21
#$%&='()*+,<./)*+,45>#$789
62
wait()and exit()ʹparent side
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
The kernel set the process to be sleeping.
The formal way to say: the wait()system 
call blocks the processuntil ...
This guy invoked 
wait().
Child
Parent
Signal handlers
Guess: when to wake up?

22
#$%&='()*+,<./)*+,45>#$789
63
wait()and exit()ʹparent side
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
This guy invoked 
wait().
Child
Parent
Signal handlers
Process
1235
SIGCHLD 
from 
1235

23
#$%&='()*+,<./)*+,45>#$789
64
wait()and exit()ʹparent side
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
When SIGCHLDcomes, the signal 
handling routine is invoked!
Note: since the parent is still inside 
the system call, instead of the 
original program code, the parent 
process is still blocked in some 
ƐĞŶƐĞ͙
This guy invoked 
wait().
Child
Parent
Signal handlers
SIGCHLD 
from 
1235

24
#$%&='()*+,<./)*+,45>#$789
65
wait()and exit()ʹparent side
OS Kernel
Process
1234
Process
1235
PID = 1234
Running time
Array of opened files
This guy invoked 
wait().
Child
Parent
Signal handlers
Default Handling of SIGCHLD
1.Accept and remove the 
SIGCHLD;
2.Destroy the child process 
that sends her the signal.
SIGCHLD 
from 
1235
Now, the child is truly 
dead.

25
#$%&='()*+,<./)*+,45>#$789
66
wait()and exit()ʹparent side
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
Ready to return 
from wait().
Parent
Signal handlers
The signal handler is then removed, i.e., the 
process is ignoring SIGCHLDagain.
It returns to the previously-executing code, 
going back to the user space.
^Ž͕ŝƚůŽŽŬƐůŝŬĞ͞wait()is returned from its 
invocation͘͟
This is the reason why wait() system call waits 
for any one of the child processes.

26
#$%&='()*+,<./)*+,45>#$789
67
wait()and exit()ʹparent side
OS Kernel
Process
1234
PID = 1234
Running time
Array of opened files
Ready to return 
from wait().
Parent
Return value = 1235
Lastly, the return value of wait()system call is 
the PID of the terminated child.

27
#$%&='()*+,<./)*+,45>#$789
68
wait()and exit()ʹparent side
Child 
Process
Parent
Process
wait()
is called.
Parent
Child
wait()
blocksthe 
parent.
Child is terminated through 
the exit()system call.
wait()
returns.
So, the child will be 
given a clean death 
by the wait()
system call.
SIGCHLD
如果父进程在子进程终止后调用wait()结果如何？

28
v
许多web浏览器作为单个进程运行（有些仍然如此）
§如果一个网站出现问题，整个浏览器可能会挂起或崩溃
v
Google Chrome浏览器是多进程的，有3种不同类型的进程：
§浏览器进程管理用户界面、磁盘和网络I/O
§渲染器进程渲染网页，处理HTML、Javascript。为每个打开的
网站创建一个新的渲染器
•在沙箱中运行，限制磁盘和网络I/O，最大限度地减少安全
漏洞攻击的影响
§每种类型插件的插件进程
?#$@ABCDEFGHI;JKL

29
v
系统内的过程可以是独立的，也可以是协作的
v
协作进程可以影响或受其他进程的影响，包括共享数据
v
合作进程的原因：
§信息共享
§计算加速
§模块化
§方便用户
v
协作进程需要进程间通信（IPC）
v
IPC的两种模型
§共享内存
§消息传递
#$MNO

30
#$MNOPQ
（a） 共享内存。（b）信息传递。
多核处理器系统中，消息传递优于共享内存，因为没有高速缓存（Cache）的一致性问
题。

31
RSTUAV
v希望通信的进程之间共享的内存区域，通信双方需要同
时映射共享内存到自己的进程内;
v通信由用户进程控制，而不是由操作系统控制；
v主要问题是提供一种机制，允许用户进程在访问共享内
存时同步其操作；
v第6章和第7章详细讨论了同步；

32
WXYZ[Y\]
v
协作进程的范例：
§生产者进程产生消费者进程所消费的信息；
§例如Web服务器和客户端；
§消息中间件；
v
两种变体：
§无边界缓冲区对缓冲区的大小没有实际限制：
•生产者从不等待；
•如果没有要使用的缓冲区，则消费者将等待；
§有界缓冲区假定存在固定的缓冲区大小：
•如果所有缓冲区都已满，则生产者必须等待；
•如果没有可用的缓冲区，则消费者将等待；

33
^_`ab4RSTUcdef
#define BUFFER_SIZE 10
typedef struct {
... ...    /* item structure */
} item;
item buffer[BUFFER_SIZE];
int in = 0;
int out = 0; 
Ø
共享数据结构
共享buffer的实现采用一个循环数组和两个逻辑指针：in和
out。变量in 指向缓冲区的下一个空位；变量out指向缓冲区的
第一个满位。当in==out时，缓冲区为空；当
(in+1)%BUFFER_SIZE==out时，缓冲区满。

34
^_`ab4RSTUcdef
Ø
生产者
item next_produced;
while (true) {
... ... /* produce an item saved in next_produced*/
while (((in + 1) % BUFFER_SIZE) == out)
; /* buffer full, do nothing */
buffer[in] = next_produced;
in = (in + 1) % BUFFER_SIZE;
}
Ø
消费者
item next_consumed;
while (true) {
while (in == out)
;     /* buffer empty, do nothing */
next_consumed= buffer[out];
out = (out + 1) % BUFFER_SIZE;
/* consume the item in next_consumed*/
}
如何处理多个生产者、消费
者访问共享内存的问题？

35
g)hi/RSTU
Ø
Linux IPC 限制
限制信息在/etc/sysctl.conf文件中

36
g)hi/RSTU
§Key ID
#include <sys/shm.h>
key_tftok(constchar *pathname, intid);
/* key_tis of type int. ftok() convert a pathname and a project identifier 
to an IPC key*/
key_tkey = ftok(“/home/myshm", 0x27);
if((key == -1) {
perror(“ftok()");
} else
printf("key = 0x%x\n", key);
§Create
intshmget(key_tkey, intsize, intshmflg);
/* shmget() allocates a shared memory segment */
/* upper bound of size: 1.9G */
intshmid= shmget(IPC_PRIVATE, 4096, IPC_CREATE|IPC_EXCL|0660);
if(shmid== -1) {
perror("shmget()");
}

37
§Attach
void *shmat(intshmid, constvoid *shmaddr, intshmflg);
void *shmptr= shmat(shmid, 0, 0);
/* shmaddr=0: attaching address is decided by kernel */
if(shmptr== (void *)(-1))
perror("shmat()");
§Detach
intshmdt(constvoid *shmaddr);
if(shmdt(shmptr) == -1)
perror("shmdt()");
g)hi/RSTU

38
g)hi/RSTU
§Release
intshmctl(intshmid,intcmd,structshmid_ds*buf);
if (shmctl(shmid, IPC_RMID, 0) == -1)
perror("shmctl()");
Process1
shared memory
kernel
shared memory
Process2
shared memory
shm
shm
shmat()
shmat()
shmget()

39
g)hi/RSTU
§Single-writer-single-reader problem illustrating shared-memory， 
define shmdata.h
#define TEXT_SIZE 4*1024  /* = PAGE_SIZE, size of each message */
#define TEXT_NUM 1      /* maximal number of messages */
/* total size can not exceed current shmmax,
or an 'invalid argument' error occurs when shmget*/
#define PERM S_IRUSR|S_IWUSR|IPC_CREAT
#define ERR_EXIT(m) \
do { \
perror(m); \
exit(EXIT_FAILURE); \
} while(0)
/* a demo structure, modified as needed */
structshared_struct{
intwritten; /* flag = 0: buffer writable; others: readable */
char mtext[TEXT_SIZE]; /* buffer for message reading and writing */
};

40
g)hi/RSTU
int main(int argc, char *argv[])
{
struct stat fileattr;
key_tkey; /* of type int */
int shmid; /* shared memory ID */
void *shmptr;
struct shared_struct*shared; /* structured shm*/
pid_tchildpid1, childpid2;
char pathname[80], key_str[10], cmd_str[80];
int shmsize, ret;
shmsize= TEXT_NUM*sizeof(struct shared_struct);
printf("max record number = %d, shmsize = %d\n", TEXT_NUM, shmsize);
if(argc<2) {
printf("Usage: ./a.outpathname\n");
return EXIT_FAILURE;
}
strcpy(pathname, argv[1]);
if(stat(pathname, &fileattr) == -1) {
ret = creat(pathname, O_RDWR);
if (ret == -1) {
ERR_EXIT("creat()");
}
printf("shared file object created\n");
}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <sys/shm.h>
#include <fcntl.h>
#include “alg.8-0-shmdata.h"
shmcon.c

41
key = ftok(pathname, 0x27); /* 0x27 a pro_id0x0001 -0xffff, 8 least bits used */
if(key == -1) {
ERR_EXIT("shmcon: ftok()");
}
printf("key generated: IPC key = 0x%x\n", key); /* set any key>0 without ftok() */
shmid= shmget((key_t)key, shmsize, 0666|PERM);
if(shmid== -1) {
ERR_EXIT("shmcon: shmget()");
}
printf("shmcon: shmid= %d\n", shmid);
shmptr= shmat(shmid, 0, 0); /* returns the virtual base address mapping to the 
shared memory, *shmaddr=0 decided by kernel */
if(shmptr== (void *)-1) {
ERR_EXIT("shmcon: shmat()");
}
printf("shmcon: shared Memory attached at %p\n", shmptr);
shared = (struct shared_struct*)shmptr;
shared->written = 0;
sprintf(cmd_str, "ipcs-m | grep '%d'\n", shmid); 
printf("\n------Shared Memory Segments ------\n");
system(cmd_str);
g)hi/RSTU
shmcon.c

42
g)hi/RSTU
if(shmdt(shmptr) == -1) {
ERR_EXIT("shmcon: shmdt()");
}
printf("\n------Shared Memory Segments ------\n");
system(cmd_str);
sprintf(key_str, "%x", key);
char *argv1[] = {" ", key_str, 0};
childpid1 = vfork();
if(childpid1 < 0) {
ERR_EXIT("shmcon: 1st vfork()");
} 
else if(childpid1 == 0) {
execv("./alg.8-2-shmread.o", argv1); /* call shm_readwith IPC key */
}
else {
childpid2 = vfork();
if(childpid2 < 0) {
ERR_EXIT("shmcon: 2nd vfork()");
}
else if (childpid2 == 0) {
execv("./alg.8-3-shmwrite.o", argv1); /* call shmwritewith IPC key 
*/
}
shmcon.c

43
else {
wait(&childpid1);
wait(&childpid2);
/* shmidcan be removed by any process known the 
IPC key */
if (shmctl(shmid, IPC_RMID, 0) == -1) {
ERR_EXIT("shmcon: shmctl(IPC_RMID)");
}
else {
printf("shmcon: shmid= %d removed \n", shmid);
printf("\n------Shared Memory Segments ------
\n");
system(cmd_str);
printf("nothing found ...\n"); 
return EXIT_SUCCESS;
}
}
}
}
g)hi/RSTU
shmcon.c

44
g)hi/RSTU
int main(int argc, char *argv[])
{
void *shmptr= NULL;
struct shared_struct*shared;
int shmid;
key_tkey;
sscanf(argv[1], “%x", &key);
printf("%*sshmread: IPC key = 0x%x\n", 30, " ", key);
shmid= shmget((key_t)key, TEXT_NUM*sizeof(struct shared_struct), 
0666|PERM);
if (shmid== -1) {
ERR_EXIT("shread: shmget()");
}
shmptr= shmat(shmid, 0, 0);
if(shmptr== (void *)-1) {
ERR_EXIT("shread: shmat()");
}
printf("%*sshmread: shmid= %d\n", 30, " ", shmid);    
printf("%*sshmread: shared memory attached at %p\n", 30, " ", shmptr);
printf("%*sshmreadprocess ready ...\n", 30, " ");
shared = (struct shared_struct*)shmptr;
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <string.h>
#include <sys/shm.h>
#include “alg.8-0-shmdata.h"
shmread.c(1)

45
g)hi/RSTU
while (1) {
while (shared->written == 0) {
sleep(1); /* message not ready, waiting ... */
}
printf("%*sYouwrote: %s\n", 30, " ", shared->mtext);
shared->written = 0;
if (strncmp(shared->mtext, "end", 3) == 0) {
break;
}
} /* it is not reliable to use shared->written for process 
synchronization */
if (shmdt(shmptr) == -1) {
ERR_EXIT("shmread: shmdt()");
}
sleep(1);
exit(EXIT_SUCCESS);
}
shmread.c(1)

46
g)hi/RSTU
int main(int argc, char *argv[])
{
void *shmptr= NULL;
struct shared_struct*shared = NULL;
int shmid;
key_tkey;
char buffer[BUFSIZ + 1]; /* 8192bytes, saved from stdin */
sscanf(argv[1], "%x", &key);
printf("shmwrite: IPC key = 0x%x\n", key);
shmid= shmget((key_t)key, TEXT_NUM*sizeof(struct shared_struct), 0666|PERM);
if (shmid== -1) {
ERR_EXIT("shmwite: shmget()");
}
shmptr= shmat(shmid, 0, 0);
if(shmptr== (void *)-1) {
ERR_EXIT("shmwrite: shmat()");
}
printf("shmwrite: shmid= %d\n", shmid);
printf("shmwrite: shared memory attached at %p\n", shmptr);
printf("shmwriteprecessready ...\n");
shared = (struct shared_struct*)shmptr;
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <string.h>
#include <sys/shm.h>
#include “alg.8-0-shmdata.h"
shmwrite.c(1)

47
g)hi/RSTU
while (1) {
while (shared->written == 1) {
sleep(1); /* message not read yet, waiting ... */ 
}
printf("Enter some text: ");
fgets(buffer, BUFSIZ, stdin);
strncpy(shared->mtext, buffer, TEXT_SIZE);
printf("shared buffer: %s\n",shared->mtext);
shared->written = 1;  /* message prepared */
if(strncmp(buffer, "end", 3) == 0) {
break;
}
}
/* detach the shared memory */
if(shmdt(shmptr) == -1) {
ERR_EXIT("shmwrite: shmdt()");
}
sleep(1);
exit(EXIT_SUCCESS);
}
shmwrite.c(1)

48
vPOSIX Shared Memory
§Several IPC mechanisms are available for POSIX systems, including shared 
memory and message passing. 
§POSIX shared memory is organized using memory-mapped files, which
associate the region of shared memory with a file in /dev/shm/.
§For memory sharing, a process must first create a shared-memory object using 
the shm_open() system call:
int shm_open(const char *path, int flags, mode_t mode);
•Example.
fd= shm_open(name, O_CREAT|O_RDWR, 0666);
•path: the name of the shared-memory object. Processes that wish to access 
this shared memory must refer to the object by this name. 
•flags: the shared-memory object is to be created if it does not yet exist 
(O_CREAT) and that the object is open for reading and writing (O_RDWR).
•mode: the file-access permissions of the shared-memory object. 
•A successful call to shm_open() returns an integer file descriptor for the 
shared-memory object.

49
vPOSIX Shared Memory
§Once the object is established, the ftruncate() function is used to
configure the size of the object in bytes. 
intftruncate(intfd, off_tlength);
•E.g., the following call sets the size of the object to 4,096 bytes.
ftruncate(fd, 4096);
§Finally, the mmap() function establishes a memory-mapped file containing 
the shared-memory object. It also returns a pointer to the memory-
mapped file that is used for accessing the shared-memory object.
void *mmap(void *addr, size_tlen, intprot, intflags, 
intfd, off_toffset);
§The API is supported by Linux 2.4 and later, FreeBSD, ...
§Compiling
gcc-lrtfilename.c

50
§Producer-Consumer problem illustrating POSIX shared-memory API.
shmpthreadcon.c(1)
/* gcc-lrt*/
intmain(intargc, char *argv[])
{
char pathname[80], cmd_str[80];
structstat fileattr;
intfd, shmsize, ret;
pid_tchildpid1, childpid2;
if(argc< 2) {
printf("Usage: ./a.outfilename\n");
return EXIT_FAILURE;
}
fd= shm_open(argv[1], O_CREAT|O_RDWR, 0666); 
/* /dev/shm/filename as the shared object, creating if not exist */
if(fd== -1) {
ERR_EXIT(“con: shm_open()");
} 
system("ls -l /dev/shm/");   
shmsize= TEXT_NUM*sizeof(structshared_struct);
ret = ftruncate(fd, shmsize);
if(ret == -1) {
ERR_EXIT(“con: ftruncate()");
}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/stat.h>
#include <sys/wait.h>
#include <fcntl.h>
#include <sys/mman.h>
#include "shmdata.h"

51
char *argv1[] = {" ", argv[1], 0};
childpid1 = vfork();
if(childpid1 < 0) {
ERR_EXIT("shmpthreadcon: 1st vfork()");
} 
else if(childpid1 == 0) {
execv("./shmproducer", argv1); /* call producer with filename */
}
else {
childpid2 = vfork();
if(childpid2 < 0)
ERR_EXIT("shmpthreadcon: 2nd vfork()");
else if (childpid2 == 0)
execv("shmconsumer.o", argv1);/* call consumer with filename */
else {
wait(&childpid1);
wait(&childpid2);
ret = shm_unlink(argv[1]);
if(ret == -1) {
ERR_EXIT(“con: shm_unlink()");
}/* shared object can be removed by any process knew the filename */
system("ls -l /dev/shm/");   
}
}
exit(EXIT_SUCCESS);
}
shmpthreadcon.c(2)

52
Producer-Consumer problem illustrating POSIX shared-memory API.
shmproducer.c
/* gcc-lrt*/
int main(int argc, char *argv[])
{
int fd, shmsize, ret;
void *shmptr;
const char *message_0 = "Hello World!";
fd= shm_open(argv[1], O_RDWR, 0666); /* /dev/shm/filename as the 
shared object */
if(fd== -1) {
ERR_EXIT(“producer: shm_open()");
} 
shmsize= TEXT_NUM*sizeof(struct shared_struct);
shmptr= (char *)mmap(0, shmsize, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 
0);
if(shmptr== (void *)-1) {
ERR_EXIT(“producer: mmap()");
}
sprintf(shmptr,"%s",message_0);
printf("produced message: %s\n", (char *)shmptr);
return EXIT_SUCCESS;
}
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/mman.h>
#include "shmdata.h"

53
Producer-Consumer problem illustrating POSIX shared-memory API.
shmconsumer.c
/* gcc-lrt*/
int main(int argc, char *argv[])
{
int fd, shmsize, ret;
void *shmptr;
fd= shm_open(argv[1], O_RDONLY, 0444);
if(fd== -1) {
ERR_EXIT(“consumer: shm_open()");
} 
shmsize= TEXT_NUM*sizeof(struct shared_struct);
shmptr= (char *)mmap(0, shmsize, PROT_READ, MAP_SHARED, fd, 0);
if(shmptr== (void *)-1) {
ERR_EXIT(“consumer: mmap()");
}
printf("consumed message: %s\n", (char *)shmptr);
return EXIT_SUCCESS;
}
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#include <sys/mman.h>
#include "shmdata.h"

54
jkE4Zlmn
v
进程之间相互通信，无需借助共享变量
v
IPC设施提供两种操作：
§Send 发送（消息）
§Receive 接收（信息）
v
消息大小是固定的或可变的

55
jkE4Zlmn
v
如果进程P和Q希望通信，则它们需要：
§在它们之间建立通信链接
§通过发送/接收交换消息
v
实施问题：
§如何建立联系？
§链接是否可以与两个以上的进程关联？
§每对通信进程之间可以有多少链路？
§链路的容量是多少？
§链接可以容纳的消息大小是固定的还是可变的？
§链接是单向的还是双向的？

56
v
物理：
§共享内存
§硬件总线
§网络
v
逻辑：
§直接或间接
§同步还是异步
§自动或显式缓冲
NOop7qr

57
stNO
v
进程之间必须明确命名：
§发送（P，消息）–向进程P发送消息
§接收（Q，消息）–从流程Q接收消息
v
通信链路的特性:
§链接是自动建立的
§链路仅与一对通信进程相关联
§每对之间只存在一个链接
§链路可能是单向的，但通常是双向的
Ø
这种通信方式展示了寻址的对称性，即发送和接收进
程必须指定对方，以便通信。
Ø
也可以采用寻址的非对称性，即只要发送者指定接收
者，而接收者不需要指定发送者

58
v
通过邮箱或者端口来发送和接收消息
§每个邮箱都有一个唯一的id
§进程只有在共享邮箱时才能通信
v
通信链路的特性
§仅当进程共享公共邮箱时才建立链接
§链接可能与许多进程相关联
§每对进程可以共享多个通信链路
§链路可以是单向的或双向的
MtNO

59
MtNO
v
操作
§创建新邮箱（端口）
§通过邮箱发送和接收邮件
§删除邮箱
v
基本体定义为：
§发送（A，消息）–将消息发送到邮箱A
§接收（A，消息）–从邮箱A接收消息

60
v
邮箱共享
§P1、P2和P3共享邮箱A
§P1，发送；P2和P3接收
§谁得到消息？
v
解决
§允许链接最多与两个进程关联
§一次只允许一个进程执行接收操作
§允许系统任意选择接收器。通知发送方接收者是谁。
v
邮箱所有者
   
邮箱可以被进程拥有；
   邮箱被操作系统拥有；
   邮箱可以转移；
MtNO

61
uv
v
阻塞被认为是同步的
§阻塞发送—在接收进程或者邮箱收到消息之前，发送进
程阻塞；
§Blocking receive（阻塞接收）——在消息可用之前，
接收进程将被阻塞
v
非阻塞被认为是异步的
§非阻塞发送——发送进程发送消息并继续执行；
§非阻塞接收——接收进程接收：
•有效消息，或
•空消息
v
可能有不同的组合
§如果发送和接收都是阻塞的，双方之间有一个交会。
消息传递可以是阻塞的，也可以是非阻塞的

62
WXY4Z[Y=+wx,Zlmn=
3.6IPC in Message-Passing Systems131
message nextproduced;
while (true)
{
/* produce an item in nextproduced */
send(next
produced);
}
Figure 3.14The producer process using message passing.
Different combinations ofsend()andreceive()are possible. When both
send()andreceive()are blocking, we have arendezvousbetween the
sender and the receiver. The solution to the producer–consumer problem
becomes trivial when we use blocking
send()andreceive()statements. The
producer merely invokes the blocking
send()call and waits until the message
is delivered to either the receiver or the mailbox. Likewise, when the consumer
invokes
receive(),itblocksuntilamessageisavailable.Thisisillustratedin
Figures 3.14 and 3.15.
3.6.3   Buffering
Whether communication is direct or indirect, messages exchanged by commu-
nicating processes reside in a temporary queue. Basically, such queues can be
implemented in three ways:
•
Zero capacity.Thequeuehasamaximumlengthofzero;thus,thelink
cannot have any messages waiting in it. In this case, the sender must block
until the recipient receives the message.
•
Bounded capacity.Thequeuehas!nite lengthn;thus, at mostnmessages
can reside in it. If the queue is not full when a new message is sent, the
message is placed in the queue (either the message is copied or a pointer
to the message is kept), and the sender can continue execution without
message nextconsumed;
while (true)
{
receive(nextconsumed);
/* consume the item in next
consumed */
}
Figure 3.15The consumer process using message passing.
3.6IPC in Message-Passing Systems131
message nextproduced;
while (true)
{
/* produce an item in nextproduced */
send(next
produced);
}
Figure 3.14The producer process using message passing.
Different combinations ofsend()andreceive()are possible. When both
send()andreceive()are blocking, we have arendezvousbetween the
sender and the receiver. The solution to the producer–consumer problem
becomes trivial when we use blocking
send()andreceive()statements. The
producer merely invokes the blocking
send()call and waits until the message
is delivered to either the receiver or the mailbox. Likewise, when the consumer
invokes
receive(),itblocksuntilamessageisavailable.Thisisillustratedin
Figures 3.14 and 3.15.
3.6.3   Buffering
Whether communication is direct or indirect, messages exchanged by commu-
nicating processes reside in a temporary queue. Basically, such queues can be
implemented in three ways:
•
Zero capacity.Thequeuehasamaximumlengthofzero;thus,thelink
cannot have any messages waiting in it. In this case, the sender must block
until the recipient receives the message.
•
Bounded capacity.Thequeuehas!nite lengthn;thus, at mostnmessages
can reside in it. If the queue is not full when a new message is sent, the
message is placed in the queue (either the message is copied or a pointer
to the message is kept), and the sender can continue execution without
message nextconsumed;
while (true)
{
receive(nextconsumed);
/* consume the item in next
consumed */
}
Figure 3.15The consumer process using message passing.
生
产
者
消
费
者

63
`U
v
附加到链接的消息队列，不管通信是直接还是间接的；
v
以三种方式之一实现队列：
1.零容量–链路上没有消息排队。发送方必须等待接收方
2.有限容量–有限长度的n条消息，如果链路已满，则发送
者必须等待
3.无限容量–无限长，发送方从不等待

64
vLinux: Message-passing
§Message structure
structmsg_st{
longintmsg_type;
charmtext[TEXT_SIZE];
};
structmsqid_ds{
structipc_permmsg_perm;
time_tmsg_stime;    /* Last msgsndtime */
time_tmsg_rtime; /* Last msgrcvtime */
time_tmsg_ctime; /* Last change time */
msgqnum_tmsg_qnum;  /* Current number of messages in queue 
*/
msglen_tmsg_qbytes; /* Maximum number of bytes allowed in 
queue */
pid_tmsg_lspid;     /* PID of last msgsnd*/
pid_tmsg_lrpid;     /* PID of last msgrcv*/

65
vLinux: Message-passing
§Functions
key_tftok(char*pathname, charproj_id)
intmsgget(key_tkey, intmsgflg)
intmsgsnd(intmsqid, structmsgbuf*msgp, intmsgsz, 
intmsgflg)
intmsgrcv(intmsqid, structmsgbuf*msgp, intmsgsz, 
longmsgtyp, intmsgflg)
intmsgctl(intmsqid,intcmd,structmsqid_ds*buf)

66
vLinux: Message-passing
§Sending & Receiving process illustrating Linux message-passing API.
•msgdata.h
#define TEXT_SIZE 512
/* considering 
------Messages Limits --------
max queues system wide = 32000
max size of message (bytes) = 8192
default max size of queue (bytes) = 16384
------------------------------------------
The size of message is set to be 512, the total number of messages is 16384/512 = 32
If we take the max size 8192, the number would be 16384/8192 = 2. It is not reasonable
*/
/* message structure */
structmsg_struct{
long intmsg_type;
char mtext[TEXT_SIZE]; /* binary data */
};
#define PERM S_IRUSR|S_IWUSR|IPC_CREAT
#define ERR_EXIT(m) \
do { \
perror(m); \
exit(EXIT_FAILURE); \
} while(0)

67
int main(int argc, char *argv[])
{
struct msg_structdata;
long int msg_type;
char buffer[TEXT_SIZE], pathname[80];
int msqid, ret, count = 0;
key_tkey;
FILE *fp;
struct stat fileattr;
if(argc< 2) {
printf("Usage: ./a.outpathname\n");
return EXIT_FAILURE;
}
strcpy(pathname, argv[1]);
if(stat(pathname, &fileattr) == -1) {
ret = creat(pathname, O_RDWR);
if (ret == -1) {
ERR_EXIT("creat()");
}
printf("shared file object created\n");
}
Linux: Message-passing
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/msg.h>
#include <sys/stat.h>
#include <fcntl.h>
#include “msgdata.h”
msgsnd.c

68
Linux: Message-passing
key = ftok(pathname, 0x27); /* project_idcan be any nonzero integer */
if(key < 0) {
ERR_EXIT("ftok()");
}
printf("\nIPCkey = 0x%x\n", key);
msqid= msgget((key_t)key, 0666 | IPC_CREAT);
if(msqid== -1) {
ERR_EXIT("msgget()");
}
fp= fopen("./msgsnd.txt", "rb");
if(!fp) {
ERR_EXIT("source data file: ./msgsnd.txtfopen()");
}
struct msqid_dsmsqattr;
ret = msgctl(msqid, IPC_STAT, &msqattr);
printf("number of messages remainded= %ld, empty slots = %ld\n", 
msqattr.msg_qnum, 16384/TEXT_SIZE-msqattr.msg_qnum);
printf("Blocking Sending ... \n");
msgsnd.c

69
Linux: Message-passingmsgsnd.c
while (!feof(fp)) {
ret = fscanf(fp, "%ld%s", &msg_type, buffer);
if (ret == EOF) break;
printf("%ld%s\n", msg_type, buffer);
data.msg_type= msg_type;
strcpy(data.mtext, buffer);
ret = msgsnd(msqid, (void *)&data, TEXT_SIZE, 0);
/* 0: blocking send, waiting when msg queue is full */
if(ret == -1) {
ERR_EXIT("msgsnd()");
}
count++;
}
printf("number of sent messages = %d\n", count);
fclose(fp);
system("ipcs-q");
exit(EXIT_SUCCESS);
}
Demo

70
Linux: Message-passing
§Sending & Receiving process illustrating Linux message-passing API.
•msgrcv.c(1)
int main(int argc, char *argv[])
{
key_tkey;
structstat fileattr;
char pathname[80];
intmsqid, ret, count = 0;
structmsg_structdata;
long intmsgtype= 0;   /* 0 -type of any messages */
if(argc< 2) {
printf("Usage: ./msgrcvpathname msg_type\n");
return EXIT_FAILURE;
}
strcpy(pathname, argv[1]);
if(stat(pathname, &fileattr) == -1) {
ERR_EXIT("shared file object stat error");
}
if((key = ftok(pathname, 0x27)) < 0) {
ERR_EXIT("ftok()");
}
printf("\nIPCkey = 0x%x\n", key);
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#include <unistd.h>
#include <sys/msg.h>
#include <sys/stat.h>
#include “msgdata.h”

71
msqid= msgget((key_t)key, 0666); /* do not create a new msg queue */
if(msqid== -1) {
ERR_EXIT("msgget()");
}
if(argc< 3)
msgtype= 0;
else {
msgtype= atol(argv[2]);
if (msgtype< 0)
msgtype= 0;
}   /* determinmsgtype(class number) */
printf("Selected message type = %ld\n", msgtype);
while (1) {
ret = msgrcv(msqid, (void *)&data, TEXT_SIZE, msgtype, 
IPC_NOWAIT);
/* Non_blockingreceive */
if(ret == -1) { /* end of this msgtype*/
printf("number of received messages = %d\n", count);
break;
}
printf("%ld%s\n", data.msg_type, data.mtext);
count++;
}
Linux: Message-passing
msgrcv.c(2)

72
Linux: Message-passing
struct msqid_dsmsqattr;
ret = msgctl(msqid, IPC_STAT, &msqattr);
printf("number of messages remainding= %ld\n", msqattr.msg_qnum); 
if(msqattr.msg_qnum== 0) {
printf("do you want to delete this msg queue?(y/n)");
if (getchar() == 'y') {
if(msgctl(msqid, IPC_RMID, 0) == -1)
perror("msgctl(IPC_RMID)");
}
}
system("ipcs-q");
exit(EXIT_SUCCESS);
}
msgrcv.c(3)

73
Linux: Message-passing
运行结果

74
Linux: Message-passing

75
vPOSIX: Message-passing
#include <mqueue.h>
§Open, Close and Unlink
mqd_tmq_open(constchar *name, intoflag, mode_tmode, struct
mq_attr*attr); /* return the mqdes, or -1 if failed */
mqd_tmqID;
mqID= mq_open("/anonymQueue", O_RDWR | O_CREAT, 0666, 
NULL);
mqd_tmq_close(mqd_tmqdes);
mqd_tmq_unlink(constchar *name); /* return -1 if failed */
§Send and Receive
mqd_tmq_send(mqd_tmqdes, constchar *msg_ptr, size_tmsg_len, 
unsigned msg_prio); /* return 0, or -1 if failed */
mq_send(mqID, msg, sizeof(msg), i)
mqd_tmq_receive(mqd_tmqdes, char *msg_ptr, size_tmsg_len, 
unsigned *msg_prio); /* return the number of char received, or -1 
if failed */
mq_attrmqAttr;
mq_getattr(mqID, &mqAttr);
mq_receive(mqID, buf, mqAttr.mq_msgsize, NULL)

76
vPOSIX: Message-passing
§Note that the Linux ipcsutility is not fully compatible to the 
POSIX ipcsutility. The message queues, shared memory 
and semaphores in POSIX can not be sensed by System V 
bash command such as
$ipcs-q

77
v
Mach通信是基于消息的设计
§甚至系统调用也是消息
§每个任务在创建时获得两个端口-内核和通知
§使用mach_msg（）函数发送和接收消息
§通信所需的端口，通过创建mach端口分配（）
§收发灵活；例如，邮箱已满时有四个选项：
•无限期地等待
•最多等待n毫秒
•立即返回
•临时缓存消息
y(zF

78
y(zFZlBC
3.7Examples of IPC Systems137
#include<mach/mach.h>
struct message
{
machmsgheadertheader;
int data;
};
mach
porttclient;
mach
porttserver;
/*
Client Code*/
struct message message;
// construct the header
message.header.msgh
size = sizeof(message);
message.header.msgh
remoteport = server;
message.header.msghlocalport = client;
// send the message
machmsg(&message.header, // message header
MACHSENDMSG, // sending a message
sizeof(message), // size of message sent
0, // maximum size of received message - unnecessary
MACH
PORTNULL, // name of receive port - unnecessary
MACHMSGTIMEOUTNONE, // no time outs
MACH
PORTNULL // no notify port
);
/*
Server Code*/
struct message message;
// receive the message
machmsg(&message.header, // message header
MACH
RCVMSG, // sending a message
0, // size of message sent
sizeof(message), // maximum size of received message
server, // name of receive port
MACH
MSGTIMEOUTNONE, // no time outs
MACHPORTNULL // no notify port
);
Figure 3.18Example program illustrating message passing in Mach.
The send and receive operations themselves are!exible. For instance, when
amessageissenttoaport,itsqueuemaybefull.Ifthequeueisnotfull,
the message is copied to the queue, andthe sending task continues. If the

79
y(zF{|}

80
y(zF~L}

81
v
通过高级本地过程调用（ALPC）设施以消息传递为中心
§仅在同一系统上的两个进程之间工作，类似RPC；
§使用端口（如邮箱）建立和维护通信通道；
§使用两种类型的端口：连接端口和通信端口；
§通讯工作如下：
•客户端打开子系统
连接端口
对象的句柄；
•客户端发送一个连接请求；
•服务器创建一对专用通信端口，并将其中一个端口的句
柄返回给客户端；
•客户端和服务器使用相应的端口句柄发送消息或回调，
并监听回复；
jkEAVÄÅD')hÇHÉÑ

82
')hÇHÉÑÖ7Üáà$âä

83
')hÇHÉÑÖ7Üáà$âä
3.7Examples of IPC Systems139
Connection
Por t
Connection
request
Handle
Handle
Handle
Client
Communication Port
Server
Communication Port
Shared
Section Object
(> 256 bytes)
ServerClient
Figure 3.19Advanced local procedure calls in Windows.
When anALPCchannel is created, one of three message-passing techniques
is chosen:
1.For small messages (up to 256 bytes), the port’s message queue is used
as intermediate storage, and the messages are copied from one process to
the other.
2.Larger messages must be passed through asection object,whichisa
region of shared memory associated with the channel.
3.When the amount of data is too large to!tintoasectionobject,an
APIis
available that allows server processes to read and write directly into the
address space of a client.
The client has to decide when it sets up the channel whether it will need
to send a large message. If the client determines that it does want to send
large messages, it asks for a section object to be created. Similarly, if the server
decides that replies will be large, it creates a section object. So that the section
object can be used, a small message is sent that contains a pointer and size
information about the section object. This method is more complicated than the
!rst method listed above, but it avoids data copying. The structure of advanced
local procedure calls in Windows is shown in Figure 3.19.
It is important to note that the
ALPCfacility in Windows is not part of the
Windows
APIand hence is not visible to the application programmer. Rather,
applications using the Windows
APIinvoke standard remote procedure calls.
When the
RPCis being invoked on a process on the same system, theRPCis
handled indirectly through an
ALPCprocedure call. Additionally, many kernel
services use
ALPCto communicate with client processes.
3.7.4   Pipes
Apipeacts as a conduit allowing two processes to communicate. Pipes were
one of the!rst
IPCmechanisms in earlyUNIXsystems. They typically pro-
vide one of the simpler ways for processes to communicate with one another,
although they also have some limitations. In implementing a pipe, four issues
must be considered:

84
ãåçkjk.é
v
充当允许两个进程通信的管道
v
存在的问题：
§通信是单向的还是双向的？
§在双向通信的情况下，是半双工还是全双工？
§通信进程之间必须存在关系（即父子关系）？
§这些管道可以通过网络使用吗？还是在本地？
v
普通管道–无法从创建它的进程外部访问。通常，父进程创建
管道并使用它与它创建的子进程通信。
v
命名管道–可以在没有父子关系的情况下访问。

85
èNãåçkjk.é
v
普通管道允许标准生产者-消费者风格的通信
v
生产者写入一端（管道的写入端）;
v
使用者从另一端（管道的读取端）读取数据
v
因此，普通管道是单向的;
v
需要通信进程之间的父子关系;
v
Windows调用这些匿名管道
Read end ®
write end ®
¬Read end
¬write end

86
èNãåêÅëí
3.7Examples of IPC Systems141
#include<sys/types.h>
#include<stdio.h>
#include<string.h>
#include<unistd.h>
#define BUFFERSIZE 25
#define READ
END 0
#define WRITE
END 1
int main(void)
{
char writemsg[BUFFERSIZE] = "Greetings";
char read
msg[BUFFERSIZE];
int fd[2];
pid
tpid;
/* Program continues in Figure 3.22 */
Figure 3.21Ordinary pipe in UNIX.
the relationship of the!le descriptors in thefdarray to the parent and child
processes. As this illustrates, any writes by the parent to its write end of the
pipe—
fd[1]—can be read by the child from its read end—fd[0]—of the
pipe.
In the
UNIXprogram shown in Figure 3.21, the parent process creates a
pipe and then sends a
fork()call creating the child process. What occurs
after the
fork()call depends on how the data are to"ow through the pipe.
In this instance, the parent writes to the pipe, and the child reads from it. It is
important to notice that both the parent process and the child process initially
close their unused ends of the pipe. Although the program shown in Figure
3.21 does not require this action, it is an important step to ensure that a process
reading from the pipe can detect end-of-!le (
read()returns 0) when the writer
has closed its end of the pipe.
Ordinary pipes on Windows systems are termedanonymous pipes,and
they behave similarly to their
UNIXcounterparts: they are unidirectional and
employ parent–child relationships between the communicating processes. In
addition, reading and writing to the pipe can be accomplished with the ordi-
nary
ReadFile()andWriteFile()functions. The WindowsAPIfor creating
pipes is the
CreatePipe()function, which is passed four parameters. The
parameters provide separate handles for (1) reading and (2) writing to the pipe,
as well as (3) an instance of the
STARTUPINFOstructure, which is used to specify
that the child process is to inherit the handles of the pipe. Furthermore, (4) the
size of the pipe (in bytes) may be speci!ed.
Figure 3.23 illustrates a parent process creating an anonymous pipe for
communicating with its child. Unlike
UNIXsystems, in which a child pro-
cess automatically inherits a pipe created by its parent, Windows requires the
programmer to specify which attributesthe child process will inherit. This is
142Chapter 3Processes
/* create the pipe */
if (pipe(fd) == -1){
fprintf(stderr,"Pipe failed");
return 1;
}
/* fork a child process */
pid = fork();
if (pid
<0){/* error occurred */
fprintf(stderr, "Fork Failed");
return 1;
}
if (pid>0){/* parent process */
/* close the unused end of the pipe */
close(fd[READ
END]);
/* write to the pipe */
write(fd[WRITE
END], writemsg, strlen(writemsg)+1);
/* close the write end of the pipe */
close(fd[WRITE
END]);
}
else{/* child process */
/* close the unused end of the pipe */
close(fd[WRITE
END]);
/* read from the pipe */
read(fd[READ
END], readmsg, BUFFERSIZE);
printf("read %s",read
msg);
/* close the read end of the pipe */
close(fd[READ
END]);
}
return 0;
}
Figure 3.22Figure 3.21, continued.
accomplished by!rst initializing theSECURITYATTRIBUTESstructure to allow
handles to be inherited and then redirecting the child process’s handles for
standard input or standard output to the read or write handle of the pipe. Since
the child will be reading from the pipe, the parent must redirect the child’s
standard input to the read handle of the pipe. Furthermore, as the pipes are half
duplex, it is necessary to prohibit the child from inheriting the write end of the

v
在window系统中普通的管道称为匿名管道；
v
与Unix的普通管道类似；
v
单向、父子进程之间传输；
87
ìîãå
3.7Examples of IPC Systems143
#include<stdio.h>
#include<stdlib.h>
#include<windows.h>
#define BUFFERSIZE 25
int main(VOID)
{
HANDLE ReadHandle, WriteHandle;
STARTUPINFO si;
PROCESS
INFORMATION pi;
char message[BUFFER
SIZE] = "Greetings";
DWORD written;
/* Program continues in Figure 3.24 */
Figure 3.23Windows anonymous pipe — parent process.
pipe. The program to create the child process is similar to the program in Figure
3.10, except that the!fth parameter is set to
TRUE,indicatingthatthechild
process is to inherit designated handles from its parent. Before writing to the
pipe, the parent!rst closes its unused read end of the pipe. The child process
that reads from the pipe is shown in Figure 3.25. Before reading from the pipe,
this program obtains the read handle to the pipe by invoking
GetStdHandle().
Note that ordinary pipes require a parent–child relationship between the
communicating processes on both
UNIXand Windows systems. This means
that these pipes can be used only for communication between processes on the
same machine.
3.7.4.2  Named Pipes
Ordinary pipes provide a simple mechanism for allowing a pair of processes
to communicate. However, ordinary pipes exist only while the processes are
communicating with one another. On both
UNIXand Windows systems, once
the processes have!nished communicating and have terminated, the ordinary
pipe ceases to exist.
Named pipes provide a much more powerful communication tool. Com-
munication can be bidirectional, and no parent–child relationship is required.
Once a named pipe is established, several processes can use it for communi-
cation. In fact, in a typical scenario, a named pipe has several writers. Addi-
tionally, named pipes continue to existafter communicating processes have
!nished. Both
UNIXand Windows systems support named pipes, although the
details of implementation differ greatly. Next, we explore named pipes in each
of these systems.
Named pipes are referred to as
FIFOsinUNIXsystems. Once created, they
appear as typical!les in the!le system. A
FIFOis created with themkfifo()
system call and manipulated with the ordinaryopen(),read(),write(),
and
close()system calls. It will continue to exist until it is explicitly deleted
144Chapter 3Processes
/* set up security attributes allowing pipes to be inherited */
SECURITY
ATTRIBUTES sa ={sizeof(SECURITYATTRIBUTES),NULL,TRUE};
/* allocate memory */
ZeroMemory(&pi, sizeof(pi));
/* create the pipe */
if (!
CreatePipe(&ReadHandle, &WriteHandle, &sa, 0)){
fprintf(stderr, "Create Pipe Failed");
return 1;
}
/* establish the STARTINFO structure for the child process */
GetStartupInfo(&si);
si.hStdOutput = GetStdHandle(STD
OUTPUTHANDLE);
/* redirect standard input to the read end of the pipe */
si.hStdInput = ReadHandle;
si.dwFlags = STARTF
USESTDHANDLES;
/* don’t allow the child to inherit the write end of pipe */
SetHandleInformation(WriteHandle, HANDLE
FLAGINHERIT, 0);
/* create the child process */
CreateProcess(NULL, "child.exe", NULL, NULL,
TRUE, /* inherit handles */
0, NULL, NULL, &si, &pi);
/* close the unused end of the pipe */
CloseHandle(ReadHandle);
/* the parent writes to the pipe */
if (!WriteFile(WriteHandle, message,BUFFER
SIZE,&written,NULL))
fprintf(stderr, "Error writing to pipe.");
/* close the write end of the pipe */
CloseHandle(WriteHandle);
/* wait for the child to exit */
WaitForSingleObject(pi.hProcess, INFINITE);
CloseHandle(pi.hProcess);
CloseHandle(pi.hThread);
return 0;
}
Figure 3.24Figure 3.23, continued.

88
3.8CommunicationinClient–ServerSystems145
#include<stdio.h>
#include<windows.h>
#define BUFFERSIZE 25
int main(VOID)
{
HANDLE Readhandle;
CHAR buffer[BUFFER
SIZE];
DWORD read;
/* get the read handle of the pipe */
ReadHandle = GetStdHandle(STD
INPUTHANDLE);
/* the child reads from the pipe */
if (ReadFile(ReadHandle, buffer, BUFFER
SIZE, &read, NULL))
printf("child read %s",buffer);
else
fprintf(stderr, "Error reading from pipe");
return 0;
}
Figure 3.25Windows anonymous pipes — child process.
from the!le system. AlthoughFIFOsallowbidirectionalcommunication,only
half-duplex transmission is permitted. If data must travel in both directions,
two
FIFOsaretypicallyused.Additionally,thecommunicatingprocessesmust
reside on the same machine. If intermachine communication is required, sock-
ets (Section 3.8.1) must be used.
Named pipes on Windows systems provide a richer communication mech-
anism than their
UNIXcounterparts. Full-duplex communication is allowed,
and the communicating processes may reside on either the same or different
machines. Additionally, only byte-oriented data may be transmitted across a
UNIX FIFO,whereasWindowssystemsalloweitherbyte-ormessage-oriented
data. Named pipes are created with the
CreateNamedPipe()function, and a
client can connect to a named pipe using
ConnectNamedPipe().Communi-
cation over the named pipe can be accomplished using the
ReadFile()and
WriteFile()functions.
3.8Communication in Client – Server Systems
In Section 3.4, we described how processes can communicate using shared
memory and message passing. These techniques can be used for communica-
tion in client–server systems (Section 1.10.3) as well. In this section, we explore
two other strategies for communication in client–server systems: sockets and
ìîãå

89
ïîãå
v
命名管道比普通管道功能更强大
v
通信是双向的;
v
通信进程之间不需要父子关系;
v
有几个进程可以使用命名管道进行通信;
v
在UNIX和Windows系统上都提供;

90
g)hi/ñóh)/=k)ò;
Named Pipes in UNIX/Linux
Named Pipes are referred to as FIFOs, created with the 
mkfifo() system call.
Once created, they appear as typical files in the file 
system and manipulated with the ordinary open(), 
read(), write(), and close() system calls.
A FIFO will continue to exist until it is explicitly deleted 
from the file system.
FIFOs allow bidirectional communication and half-duplex 
transmission.
If data must travel in both directions, two FIFOs are 
typically used.
The communicating processes must reside on the same 
machine/host.
use socketsif inter-machine communication is required

91
ZlNOôöãå

92
g)hi/õ=k)ò;Ñ
Ø
普通管道
int pipefd[2];
int pipe(int pipefd);
int fcntl(int pipefd[0|1], int cmd);
int fcntl(int pipefd[0|1], int cmd, long 
arg);
ssize_twrite(int pipefd[1], void* buf, 
size_tcount);
ssize_tread(int pipefd[0], void* buf, 
size_tcount);
close(pipefd[0]);
close(pipefd[1]);

93
g)hi/õ=k)ò;Ñ
Ø
命名管道
#define FIFO pathname /* pathname: "/tmp/my_fifo" */
unlink(FIFO); /*delete a name and possibly the file it refers 
to */
mkfifo(FIFO, 0666); 
int fdw= open(FIFO, O_RDWR);
mkfifo(FIFO, 0444); 
int fdr= open(FIFO, O_RDONLY); 
ssize_twrite(int fdw, void* buf, size_tcount);
/* ssizt_t= signed int, sizt_t= unsigned int */
ssize_tread(int fdr, void* buf, size_tcount);
close(fdw);
close(fdr);

94
g)hi/õ=k)ò;Ñ
Ø
管道缓冲
#include <stdio.h>  
#include <stdlib.h>
#include <unistd.h>  
#include <fcntl.h>
#define ERR_EXIT(m) \
do { \
perror(m); \
exit(EXIT_FAILURE); \
} while (0)
int main(int argc, char *argv[])
{
int pipefd[2];
int bufsize;
char *buffer;
int flags, ret, lastwritten, count, totalwritten;    
if(pipe(pipefd) == -1) /* create an ordinary pipe */
ERR_EXIT("pipe()");
flags = fcntl(pipefd[1], F_GETFL);
fcntl(pipefd[1], F_SETFL, flags | O_NONBLOCK); /* set 
write_endNONBLOCK */
bufsize= atoi(argv[1]);
printf("testing buffer size = %d\n", bufsize);
buffer = (char *)malloc(bufsize*sizeof(char));
if(buffer == NULL || bufsize== 0)
ERR_EXIT("malloc()");

95
g)hi/õ=k)ò;Ñ
Ø
管道缓冲
count = 0;
while (1) {
ret = write(pipefd[1], buffer, 
bufsize);
/* bufsizeis better to be 2^k 
*/
if(ret == -1) {
perror("write()");
break;
}
lastwritten= ret;
count++;
}
totalwritten= (count-1)*bufsize+ 
lastwritten;
printf("single pipe buffer count =  %d, 
last written = %d bytes\n", count, 
lastwritten);
printf("total written = %d bytes = %d 
KiB\n", totalwritten, totalwritten/1024); /* 
pipe buffer */
return 0;
}

96
g)hi/õ=k)ò;Ñ
Ø
管道缓冲
Differs from PIPE_BUF in that PIPE_SIZE is the 
length of the actual memory allocation, whereas 
PIPE_BUF makes atomicity guarantees.
check the header of include/linux/pipi_fs_i.h
#define PIPE_SIZE PAGE_SIZE  /* 4 KiB */
#define PIPE_DEF_BUFFERS 16  /* 4*16 = 64 
KiB */

97
g)hi/õ=k)ò;Ñ
Ø
管道容量
int main(void)
{
char buf[PIPE_SIZE];
int testfd[600][2];
int i;
long int ret;
for (i= 0; i< 600; i++) {
if(pipe(testfd[i]) == -1) {
perror("pipe()");
break;
} 
fcntl(testfd[i][1], F_SETFL, O_NONBLOCK); 
ret = write(testfd[i][1], buf, PIPE_SIZE);
if(ret == -1 || ret != PIPE_SIZE) {
perror("write()");
break;
}
}
printf("\nsinglepipe buffer = 64 KiB, pipes created: %d\n", i);
printf("total used size: %ldbytes = %ldKiB, or %.0f MiB\n", (long 
int)i*PIPE_SIZE, (long int)i*PIPE_SIZE/1024, 
(double)i*PIPE_SIZE/1024/1024);
return 0;
}
#include <stdio.h>  
#include <stdlib.h>
#include <unistd.h>  
#include <fcntl.h>
#define PIPE_SIZE 64*1024 /* 64 KiB */

98
g)hi/õ=k)ò;Ñ
Ø
管道容量
int main(void)
{
char buf[PIPE_SIZE];
int testfd[600][2];
int i;
long int ret;
for (i= 0; i< 600; i++) {
if(pipe(testfd[i]) == -1) {
perror("pipe()");
break;
} 
fcntl(testfd[i][1], F_SETFL, O_NONBLOCK); 
ret = write(testfd[i][1], buf, PIPE_SIZE);
if(ret == -1 || ret != PIPE_SIZE) {
perror("write()");
break;
}
}
printf("\nsinglepipe buffer = 64 KiB, pipes created: %d\n", i);
printf("total used size: %ldbytes = %ldKiB, or %.0f MiB\n", (long 
int)i*PIPE_SIZE, (long int)i*PIPE_SIZE/1024, 
(double)i*PIPE_SIZE/1024/1024);
return 0;
}
#include <stdio.h>  
#include <stdlib.h>
#include <unistd.h>  
#include <fcntl.h>
#define PIPE_SIZE 64*1024 /* 64 KiB */

99
g)hi/õ=k)ò;Ñ
Ø
普通管道
/* a blocking read version */
int main(void)
{
char write_msg[BUFSIZ]; /* BUFSIZ = 8192bytes, saved from stdin */
char read_msg[BUFSIZ];
int pipefd[2]; /* pipefd[0] for READ_END, pipefd[1] for WRITE_END */
int flags;
pid_tpid;
if(pipe(pipefd) == -1) { /* create a pipe */
perror("pipe()");
exit(EXIT_FAILURE);
}
flags = fcntl(pipefd[WRITE_END], F_GETFL);
fcntl(pipefd[WRITE_END], F_SETFL, flags | O_NONBLOCK); /* non-blocking 
write */
flags = fcntl(pipefd[READ_END], F_GETFL);
fcntl(pipefd[READ_END], F_SETFL, flags); /* blocking read */
pid= fork(); /* fork a child process */
if(pid< 0) {
perror("fork()");
exit(EXIT_FAILURE);
}
#include <stdlib.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
#include <fcntl.h>
#include <sys/wait.h>
#define READ_END 0
#define WRITE_END 1

100
g)hi/õ=k)ò;Ñ
if(pid> 0) { /* parent process */
while (1) {
printf("Enter some text: ");
fgets(write_msg, BUFSIZ, stdin);
write(pipefd[WRITE_END], write_msg, BUFSIZ);
if (strncmp(write_msg, "end", 3) == 0) 
break;
}
}
else { /* child process */
while (1) {
read(pipefd[READ_END], read_msg, BUFSIZ);
printf("\n\t\t\t\t\tpiperead = %s", read_msg);
if(strncmp(read_msg, "end", 3) == 0) 
break;   
}
}
wait(0);
close(pipefd[WRITE_END]);
close(pipefd[READ_END]);
exit(EXIT_SUCCESS);
}
Ø
普通管道

101
g)hi/õ=k)ò;Ñ
§Pipes in UNIX CLI
•A pipe can be constructed on the UNIX command line using the |
character. The complete command is
ls	|	less
•The commands ls	andless	are running as individual processes. The 
output of ls	is delivered as the input to less. Result of ls:

102
客户机-服务器系统中的通信
v
套接字（Socket）
v
远程过程调用（RPC）
v
套接字被定义为通信的端点
v
IP地址和端口的串联–包含在消息包开头的数字，用于
区分主机上的网络服务;
v
套接字161.25.19.8:1625引用主机161.25.19.8上的端口
1625;
v
通信由一对套接字组成
v
1024以下的所有端口都是相对固定的，用于标准服务；
v
特殊IP地址127.0.0.1（环回），用于表示正在运行进程
的系统
Demo

103
套接字
Demo

104
套接字
v三种类型的套接字
§面向连接（TCP）
§无连接（UDP）
§MulticastSocket类–
数据可以发送到多
个收件人
v考虑java中的这个“日
期”服务器：

105
套接字
客户机的等效日期

106
远程过程调用
v远程过程调用（RPC）抽象网络系统上进程之间的过程调用
§再次使用端口进行服务区分
vStubs–服务器上实际过程的客户端代理
v客户端存根定位服务器并封送参数
v服务器端存根接收此消息，解压缩封送的参数，并在服务
器上执行该过程
v在Windows上，存根代码根据用Microsoft接口定义语言
（MIDL）编写的规范编译

107
远程过程调用
v
通过外部数据表示（XDL）格式处理数据表示，以考虑不
同的体系结构
§大端和小端;
v
远程通信比本地通信有更多的故障场景
§消息可以只传递一次，而不是最多一次
v
操作系统通常提供会合（或配对）服务来连接客户端和服
务器

108
远程过程调用

109
RPC的实际应用
Ø关键点
p对象序列化协议；
p调用控制协议；

110
DCE RPC
ØDCE（分布式计算环境）
OSF（开放软件基金会）下的一个软件系统框架（慢慢消亡）
https://en.wikipedia.org/wiki/Distributed_Computing_Environment
ØDCE的构成
p远程过程调用 DCE/RPC；
p命名/目录服务；
p时间服务；
p安全服务；
p分布式文件系统 DCE/DFS；

111
DCE RPC

112
RPC实现过程  
ØLinux的RPC编译工具：rpcgen
Ø先编写接口文件：rpc_test.x
program TESTPROG {
version VERSION{
string TEST(string) = 1;
} = 1;
} = 1;
Ø编译接口文件:  rpcgenrpc_test.x
产生 rpc_test_clnt.c、 rpc_test_svc.c、rpc_test.h
RPC函数
版本编号
RPC程序编号

113
RPC实现过程  
Ørpc_test_clnt.c的内容

114
RPC实现过程  
rpc_test.h

115
RPC实现过程  
rpc_test_svc.c

116
RPC实现过程  
Ø生成client stub代码:  rpcgen–Sc–o rpc_client.crpc_test.x

117
RPC实现过程  
Ø生成server stub代码:  rpcgen–Ss–o rpc_server.crpc_test.x

118
RPC实现过程  
Ø#include <time.h>
#include "test.h"
char ** test_1_svc(char **argp, structsvc_req*rqstp)
{
static char * result;
static char tmp_char[128];
time_trawtime;
/*
* insert server code here
*/
if( time(&rawtime) == ((time_t)-1) ) {
strcpy(tmp_char, "Error");
result = tmp_char;
return &result;
}
sprintf(tmp_char, "服务器当前时间是 :%s", ctime(&rawtime));
result = tmp_char;
return &result;
}
服务器端代码
1.gcc-Wall -o rpc_clientrpc_client.crpc_test_clnt.c
2.gcc-Wall -o rpc_serverrpc_server.crpc_test_svc.c

119
Protobuf & gRPC
Ø语言无关、平台无关: 即 ProtoBuf支持 Java、C++、Python 等多种语
言，支持多个平台 
Ø高效: 即比 XML 更小（3 ~ 10倍）、更快（20 ~ 100倍）、更为简单;
Ø扩展性、兼容性好: 你可以更新数据结构，而不影响和破坏原有的旧程
序;

120
Protobuf & gRPC
gRPC和restful API都提供了一套通信
机制，用于server/client模型通信，
而且它们都使用http作为底层的传
输协议。
ØgRPC可以通过protobuf来定义接口，从而可以有更加严格的接口约束
条件；
Ø通过protobuf可以将数据序列化为二进制编码，这会大幅减少需要
传输的数据量；
ØgRPC可以方便地支持流式通信；
Demo

121
