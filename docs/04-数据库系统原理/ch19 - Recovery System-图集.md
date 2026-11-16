# ch19 - Recovery System-图集

> 主题库：数据库系统原理　｜　源文件：ch19 - Recovery System-图集.pdf　｜　2,747 字符　｜　35 个片段　｜　更新于 2026-11-11
Chapter 19: RecoverySystem

Outuline

Storage Structure

Stable

cYnanrageem lementatuon

Protecting storage media from fallure

(Cont)

Data Access

Yb-pointer dbpointer old copy of old copy of new copy of database database database (to bedeleted

Log

Immediate Database Modification

Immediate Database Modfication Example

Recovering fromFallure

Immediate DB Modification  Recovery Example

<T0 start> <T0start> <T0start> <T0,A,1000,950> <T0, A, 1000,950> <T0, A,1000,950> <T0,B,2000,2050> <T0,B,2000,2050> <T0, B,2000,2050> Tocommit> Tocommit> <T1, C,700,600> <T1,C,700,600> (a) (b) (c)

Checkpointsで Cont-

Beginning oflog Start log records found for: all older <Tostart> transactionsi in <To,B, 20OO, 2O50> undo list <Tstart Torollback <checkpoint {To,T}> (during Redo Pass norm al <T1, C,700, 600> 0peration) begnns <T1 commit> End oflog Torollback <T2,4 A, 500, 400 > at crash! cComplete <To, B, 2000> V To abort> T2 is incomplete Log  records at crash Undo list:T2 Undo Pass  added during <T2,A, 500> recovery <T2abort> T2 rolled back in ndo ass newer

Fallure with Loss of Nonvolatile Storage

Recovering rom FallureofNon

Volatile

Storage

RemoteBackup Systems (Cont)

Recovery wlth Earty Lock Release and

Loglcal UndoOperatons

Recovery with Earty Lock Release

Logical Undo Logging

Physical Redo

Operation Logging (Cont-)

Transaction Rollback with Loglcal Undo

Transaction  Rollback with   h Logical Undo

If To aborts before operation O1 ends, undo ) of update to Ci 1 will be physical Beginning of log  Tohas Com pleted 0peration O1 <Tostart> on C,l releases lower-level <To,B,  2ooo, 2o5o> lock; undo cannotbe  physical <To, O1, operation-begin> done anymore, logical undo <To, C, 0O, 600> wil add 100toC <To, O1, operation-end, (C, +100)> T1 can Update C since Tohas <T Sta released lower-level lock on C <To, O2, operation-begin> To. <T1, C, 6oo, 4oo> decides T1 releases lo w er-levellock T1, O2, 0peration-end,(C,+2oo)) to abort on C <To,C, 4oo,5oo> Logical undo ofO1 , adds 1oo  <To,O1, onC operation-aborty <To, B, 20oo> O1undo complete <To, abort <T1, Commit>

Startlog records Beginning of log found for all <To start> transactions in <To,B,2000,2050> undo list <To commit> <T₁ start> <T,B,2050,2100> <T1, O4, Operation-begin> <checkpoint {Tj}> Redo Pass T,C,700,400> <T2 start> End of log at <T2, O5, Operation-begin> crash! T2,C, 40o,300> Undo list: T1, T2 Undo Pass <T2,C,400> Update ofC Was partofO5, undone <T2abort> Records physically during recovery  since <T1,C, 400,700> did added O5 not complete during <T1, O4, Operation-abort> <T,B,205o> recovery Logical undo ofO4 adds!  3Oo toC <T₁abort>

Transaction Rollback: Another Example

Recovery Algorithm wlth Logical Undo

Recovery with  Logical Undo (Cont-)
