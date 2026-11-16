# ch17 - Transactions-图集

> 主题库：数据库系统原理　｜　源文件：ch17 - Transactions-图集.pdf　｜　1,626 字符　｜　26 个片段　｜　更新于 2026-11-11
Outuline

Example of Fund . Transfer (Cont)

ACID Properties

Transaction State

Schedule

cYariallablity

Conflicting「   Instructions

Cascading Rollbacks

Cascadeless Schedules

Concurency Control ( VS. Serializability Tests

92

Implementation of lsolation Levels

Transactions as SQL Statements

End  of Chapter  17

partially committed (committed failed aborted

T1 T2 read (A) A:=A-50 write (A) read (B) B:=B+50 commit read (A) temp := A * 0.1 A := A - temp write(A) read (B) B = B + tem write (B) commit

T1 T2 read (A) temp := A *0.1 write(A) read (B) B := B + temp write (B) commit read (A) A:=A-50 write(A) read (B) B:=B+50 commit

T1 T2 read (A) A:=A-50 cYaa read (A) temp := A * 0.1 AA  write (A) read (B) B:=B + 50 write (B) cammit read (B) B := B + temp ccnammit

T1 T2 read (A) A:=A-50 read (A) temp := A * 0.1 A=Atem write(A) read (B) write(A) read (B) B := B + 50 write(B) commit commit

T1 T2 T 1 T 2 read (A) read (A) write (A) write (A) read (A) read (B) write (A) write (B) read(A) read (B) write (A) write (B) read (B) read (B) write (B)

read(Q) Write(Q Write(Q swap instructions in the above schedule to obtain hedule < T3, T4 >, or the serial schedule < T4, T₃ >.

[T27 T T2 read(e) s above equivalent to? l . e c s conflictserializar t

T1 T5 read (A) A:=A-50 read (B) B := B - 10 read (B) B:=B + 50 write (B) read (A) A:=A+10

Ti Tj Tk TM (a) Ti Ti Tj Tk Tk Tj Tm Tm (b) (c)

T Tg read (A) write (A) read (A) commit read (B) a would have read (and possibly shown to the

[Ta0 T1 read (A) read (B) write (A) read (A) read (A) abort nadaTlmustalsoberolled back. undoing of a significant amount of work
