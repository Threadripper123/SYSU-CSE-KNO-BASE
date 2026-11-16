# 2024-OS-CH-4-进程-part2-图集

> 主题库：操作系统　｜　源文件：2024-OS-CH-4-进程-part2-图集.pdf　｜　7,847 字符　｜　32 个片段　｜　更新于 2026-11-11
Help

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

process A process A shared memory process B message queue mo m1 m2 m3 mn kernel kernel (a) (b)

------ Messages Limits ---- max queues system wide = 320o0 max size of message (bytes) = 8192 default max size of queue (bytes) = 16384 Shared Memory Limits  max number of segments = 4096 max seg size (kbytes)= 18014398509465599 max t total shared memory (kbytes)= 18014398509481980 min seg size (bytes) = 1 --  Semaphore Limits max number of arrays = 32000 max semaphores perarray = 32000 maxsemaphoressystemwide=1024000000 max ops per semop call = 500 semaphore max value = 32767

Sending Receiving Sending Receiving Processes Processes Processes Processes n

isscgy@ubuntu:/mnt/os-2o2o$ gcc -o b.out alg.9-2-msgrcv.c isscgy@ubuntu:/mnt/os-2o2o$./b.out /home/isscgy/mysh shared file object stat error: No such file or directory isscgy@ubuntu:/mnt/os-2o2o$./b.out/home/isscgy/myshm 2 IPCkey= 0x27011c6c Selected message type = 2 aumberofmessagesremainding=8 ey msgid owner perms used-bytes messages 0x27011c6c iSSCgy 666 4096 isscgy@ubuntu:/mnt/os-2o2o$

isscgy@ubuntu:/mnt/os-2o2o$./b.out /home/isscgy/myshm 0 IPCkey=0x27011c6c Selected message type = 0 1 Luffy 1Zoro 1 sanji 4 Franky 5Brook 6 Sunny numberof received messages=8 aumberofmessagesremainding=o do you want to delete this msg queue?(y/n)y ey msqid owner perms used-bytes messages isscgy@ubuntu:/mnt/os-2o20$

/* ClientCode */ //constructthe header message.header.msgh_size = sizeof(message); message.header.msgh_remote_port = server; message.header.msgh_local_port = client; //send themessage mach msg(&message.header,// message header MACH_SENDMSG,// sending a message sizeof(message), // size of message sent 0, // maximum size of received message unnecessary MACH PORTNULL, // name of receive port - unnecessary MACH_MSGTIMEOUT_NONE, // no time outs MACH_PORT_NULL // no notifyport

/*ServerCode*/ structmessagemessage; // receive the message machmsg(&message.header,// message header MACH_RCvMSG,//sendingamessage 0,//size of message sent sizeof(message), // maximum size of received message server,// name of receive port MACH_MSG_TIMEOUT_NONE,//notimeouts MACH PORT_NULL // no notify port

Client Server Connection request Connection Handle Port Handle Client CommunicationPort Server Handle CommunicationPort Shared Section Object (>256 bytes)

Parent Child fd[0] fd [0] cn

Message queue Send Receive P1 P2 Warite Read P1 P2 Read and write in any sizes

isscgy@ubuntu:/mnt/os-2o2o$ ./a.out 1o24 testing buffer size = 1024 write(): Resource temporarily unavailable single pipe buffer count= 64, last written = 1024 bytes total written = 65536 bytes = 64 KiB isscgy@ubuntu:/mnt/os-2o2os./a.out4o96 testing buffer size = 4096 write(): Resource temporarily unavailable single pipe buffer count= 16, last written = 4096 bytes total written = 65536 bytes = 64 KiB tsscgy@ubuntu:/mnt/os-2o2os./a.out4097 testinq buffer size = 4097 write(): Resource temporarily unavailable single pipe buffer count = 11, last written = 4o96 bytes total written = 45066 bytes = 44 KiB isscgy@ubuntu:/mnt/os-2o2os./a.out32768 testing buffer size = 32768 write(): Resource temporarily unavailable single pipe buffer count = 2, last written = 32768 bytes total written = 65536 bytes = 64 KiB isscgy@ubuntu:/mnt/os-2o2o$./a.out 32769 testing buffer size = 32769 write(): Resource temporarily unavailable single pipe buffer count = 2, last written = 28673 bytes total written = 61442 bytes = 60 KiB isscgy@ubuntu:/mnt/os-2o2o$

isscgy@ubuntu:/mnt/os-2o2o$ gcc alg.1o-2-pipecapacity.c isscgy@ubuntu/mnt/os-2o2os./a.out single pipe buffer = 64 KiB, pipes created: 51o total used size: 33423360 bytes = 32640 KiB, or 32 MiB sscgyaubuntu/mnt/os-2o2os

isscgy@ubuntu:/mnt/os-2o2o$ gcc alg.1o-3-pipe-ord-1.c isscgy@ubuntu:/mnt/os-2o2o$./a.out piperead=helloworld Enter some text: good morning pipe read = good morning Enter some textend piperead = end isscgy@ubuntu:/mnt/os-2o20$

hostX (146.86.5.20) socket (146.86.5.20:1625) web server (161.25.19.8) socket (161.25.19.8:80)

importjava.net.*; importjava.io.*; abliclassateServer publicstaticvoid mainString[] args){ ServerSocket sock = new ServerSocket(6013); /* now listen for connections */ while(true){ Socket client = sock.accept(); PrintWriter pout = new PrintWriter(client.getOutputStream(), true); /* write the Date to the socket */ pout.println(new java.util.Date().toString()); /* close the socket and resume */ /* listening for connections */ client.close(); } } catch(IoExceptionioe){ System.err.println(ioe);

importjava.net.*; import java.io.*; public class DateClient public static void main(String args){ try{ /* makeconnection to server socket */ Socket sock = new Socket("127.0.0.1",6013); InputStream in=sock.getInputStream(); BufferedReaderbin=new BufferedReader(new InputStreamReader(in)); /* read the datefrom the socket */ Stringline; while ( (line = bin.readLine()) != null) System.out.println(line); /*close the socket connection*/ sock.close(); catch (IOException ioe){ System.err.println(ioe);

DATA DATA  code -code   HTTP code XML XML code  code code XMLRPC Source:lYStervinou

PTOTORESPOSE gRPC Server Ruby Client Proto eg C++ Service gRPC Proto Respose( Stub Android-Java Client

Trarsport Services Protocol Support Socket & HTTP & Google Datagram WebSocket Protobuf zlib/gzip Large File HTTP Tunnel RTSP Compression Transfer In-YMPipe LegacyText.Binary Protocols   Universal Communication API

Uuidgen Interface defnition fi le IDL compiler Client code Client stub Header Server stub SerVer code #include #include C compiler C compiler C compiler C compiler Client Client stub SerVer stub Server object file object file object fi le object file Linker Runtime Runtime  library library  ) Linker binary binary

#include "rpc test.h" teSt 1 *argp, CLIENT *clnt)  *clntres; rmernset(( *)&clnt_res, o sizeof clntres)); if (clntcall (clnt, TEST, (xdrproct) xdrwrapstring, (caddr t) argp, (xdrproct) xdr wrapstring, (caddrt) &clntres TIMEOUT) !=RPC_SUCCESS) { return (NuLL); return (&clntres);

#ifndef  RPC TESTHRPCGEN #define  RPCTESTHRPCGEN #include <rpc/rpc.h> #endit #define VERSION #ifdefine( STDC )1ldefined( Cplusplus) testprog1 reeresult svCxpRT xdrproc t, Caddr t) #define TEST 1 test1svc(); [aba _

#include "rpc test.h" #include <stdio .h> #include <stdlib.h> #include /rpc/ prmap clnt.hy #include <string.h> #include <rmemory.h> #include <sys/socket.h> #include <netinet/in.h> #ifndef SIG PF #define SIGPF #endif testprog 1( svc req*rqstp, svcxprt *transp)  *test arg, } argurnent; xdrproct xdr argurnent, xdr result; switch (rqstp->rq proc) { case NuLLPROC: ( ) svc Sendreply (transp, (xdrproc t) xdrvoid, ( *)NuLL) ; return; case TEST: xdr argurnent = (xdrproct) xdr wrapstring; xdr result= (xdrproc t)  xdrwrapstring; 1ocal svcreq*))test_1svc; break;

#include "rpc test .h" testprog_1( *host) CLIENT *clnt; testl arg; #ifndef DEBUG clnt = clnt ( create (host, TEsTPRoG, vERsIoN, udp" ) if {clnt == NULL) clnt pcreateerror (host); exit (1); result1 = test 1(&test1_arg,( clnt); if (reSult1 == NULL) clnt_perror (clnt, "callfailed"); #ifndef DEBUG clnt destroy (clnt); [Ya _e if (argc printf ("usage: s server_host\n", argv[o); exit (1):

1n 1ud C S test svc **argp, Svcreg *rgstp eturn &result

cGaal https://opensource.google.com/

Ymassage xamelel aptionalstring stringVal =1; aptionalbytesbytesVal=; message EmbeddedMessage { int32int32Val=1; stringstringVal =2; optional EmbeddedMessage embeddedExample1 = 3; repeatedint32 repeatedInt32Val=4; repeated string repeatedStringVal = 5; 子

ProtoF ,Request gRPC Stub ca ne Proto Ruby Client gRPC Server Proto Regu C++ Service gRPC Proto eponse(S) Stub Android-Java Client

中山大學计算机学院（软件学院） SENUN SUN YAT-SENUNIVERSITY SCHOOLOFCOMPUTER SCIENCE AND ENGINEERING [bl_e a _ e_l_ child Parent 1235 1234
