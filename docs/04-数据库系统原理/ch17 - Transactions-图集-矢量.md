# ch17 - Transactions-图集-矢量

> 主题库：数据库系统原理　｜　源文件：ch17 - Transactions-图集-矢量.pdf　｜　1,063 字符　｜　14 个片段　｜　更新于 2026-11-13
partially co mmitted canae failed aborted

T1 T2 read(A) A:=A-50 write(A) read (B) B:=B + 50 write (B) commit read(A) temp := A * 0.1 A:=A- tem write(A) read (B) B := B + temp write (B) commit

T1 T2 read (A) temp := A * 0.1 write(A) read (B) B := B + temp write (B) commit read (A) A:=A-50 write(A) read (B) B:=B+50 commit

T1 T2 read (A) A:=A-50 write (A) read(A) temp := A * 0.1 write(A) read (B) B:=B+50 write(B) commit read (B) B := B + temp write (B) commit

T1 T2 read (A) A:=A-50 read (A) temp := A * 0.1 A:=A- temp read (B) read (B) B := B + 50 commit B = B + tem commit

T1 T2 read (A) write (A) read (A) write (A) read (B) write (B) read (B) write (B)

T1 T2 read (A) write (A) read (B) write (B) read (A) write (A) read (B) write (B)

T3 T4 read (Q) write (Q) write (Q)

T27 T28 T29 read(Q) writee) write Q) write(Q)

T1 T5 read (A) A:=A-50 cYan A read (B) B := B - 10 read (B) B := B + 50 write (B) read (A) A:=A+10

T1 T2

Ti Tj Tk Tm

T Tg read (A) write (A) read (A) commit read (B)

T10 T11 T12 read (A) read (B) write (A) read (A) read (A) abort
