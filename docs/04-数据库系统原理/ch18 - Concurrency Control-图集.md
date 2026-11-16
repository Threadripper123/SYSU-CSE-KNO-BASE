# ch18 - Concurrency Control-图集

> 主题库：数据库系统原理　｜　源文件：ch18 - Concurrency Control-图集.pdf　｜　1,563 字符　｜　39 个片段　｜　更新于 2026-11-11
Chapter1 18:ConcurrencyControl

Outuline

Lock

BasedProtocols(Cot

T1 T2 concurrency-control manager lock-X(B) grant-X(BT) read(B) B:=B-50 write(B) unlock(B) lock-S(A) grant-S(A, T2) read(A) unlock(A) lock-S(B) grant-S(B, T2) read(B) unlock(B) display(A+B) lock-X(A) read(A) A:=A+50 write(4) unlock(A)

Deadlck

T 3 T 4 lock-X(B) read(B) B:=B-50 write(B) lock-S(A) read(A) lock-S(B) lock-X(A)

Deadlock (Cont)

The Two

PhaseLocking Protocol

T 1 T 2 lock-X(B) read(B) B:=B-50 write(B) unlock(B) lock-S(A) read(A) unlock(A) lock-S(B) read(B) unlock(B) display(A + B) lock-X(A) read(A) A:=A+50 write(4) unlock(A)

aekConversions

Automatic AcquisItion of Locks (Cont-)

LOck Table

23 T23 T1 T8 T2 I912 T23 14 T1 T I44 granted waiting T8

TrneProtool

More Deadlock   Prevention Strategles

DeadlockDetectlon

DeadlockRecovery

Example of Granularity Hierarchy

Compatiblty Matrixwith ntentlon LockModes

Multiple Granularity Locking Scheme

PredlcateReads

Phantom  Phenomenon

Non

Index Locking To Prevent Phantoms

Key Locking     to         Prevent Phantoms

Tamestamp

Ordering   Protocol

Example of Schedule Under TsO

T25 T26 read(B) read(B) B:=B-50 write(B) read(A) read(A) display(A + B) A:=A+50 write(A) display(A + B)

T27 T28 read(Q) write(Q write(Q)

Another ExampleUnder Tso

T 1 T2 T 3 T4 T5 read (X) read() read(Y) write (Y) write(Z) read (Z) read(Z) abort read (X) read (W) Write(W abort write(Y write (Z)

Correctness of Timestamp

Ordering  Protocol

transaction transaction with smaller with larger

Recoverabllty and Cascade Freedom

Thomas
