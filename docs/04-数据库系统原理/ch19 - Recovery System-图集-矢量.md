# ch19 - Recovery System-图集-矢量

> 主题库：数据库系统原理　｜　源文件：ch19 - Recovery System-图集-矢量.pdf　｜　1,363 字符　｜　14 个片段　｜　更新于 2026-11-13
buffer A  input(A) ; A B- Y B ead(X) write(Y) ×2 y1

acheckpointLo  <checkpoint L> LOG

imary

4  4

4 4 6

A B

caaaa <T0, A,1000, 950> <T0,B,2000,2050> aaacommitl caaaae <T1, C,700,600>

caaaa <T0, A,1000,950> <T0,B,2000,2050> caaaa <T1, C,700,600>

<T0 start> <T0, A,10 <T0,B,200

aag nningoflog Startlogrecords found for all alder Tostarta taransactionsin <To,B,2000,2050> undo T1 starty Torollback Redo Pass checkpoint{To,T}> (duringnormal T commita begins cacaaa End of log To rollback <T2, A, 500, 400> at crash! complete <To, B, 2000> Log records at crash Undo list: T2 Undo Pass added during <T2, A, 500> recovery T2aborto T2rolled back in undo pass newer

PageID PageLSN RecLSN 7567 2345  4894 7567 7564 7200 7565 7565 Page 4894 Page9923 DirtyPageTable 7565 7567:<T145,4894.1,40,60> Page 7200 7566:<T143'' c0mmit> Database Buffer Log Buffer (PrevLSN and UndoNextLSN fields not shown) Stabledata Stablelog 4566 404 7565:<T143,7200.2, 60,80> 7564:<T145,4894.1, 20,40> Page4894 Page7200

newer PrevLSN [7571_<T146 Commito 7570: <T146, 2390.4,50,90> / 7569:. <T1461 begin> 7568:checkpoint Txn lastLSN T145 7567 PageID PageLSN RecLSN 4894 7567 7564 7200 7565 7565 7567:<T145,4894.1,40,60>  Analysis PASS 7566<T143c0mt CLB 7565:<T143,7200.2,60>  7564:<T145,4894.1,20,40> Redo 7563:<T145I; begin> pass older 7562:. <T143,7200.2, 60, 80> UndoNextLSN

imary

A B
