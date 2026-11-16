# Lecture12.CodeOptimization-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture12.CodeOptimization-图集-矢量.pdf　｜　4,535 字符　｜　32 个片段　｜　更新于 2026-11-13
B1 (1) Sum = 0 (2) i = 1 B2 (3) t1 = ∠ 4 * i (4) t2 addr(A) - 4 (5) t3 t2[t₁]  (6) t4 (7) t5 addr(B) ) − 4 )   (8) t6 = t5[t4] (9) t7 t3 * t6 (10) Sum = sum + t7 (11) i = i+ 1 (12) if i ≤ 20 goto B2

B1 (1) Sum = 0 (2) i = 1 (4) t2 addr(A) - 4 (7) t5 addr(B) - 4  B2 (3) (5) t3 =  t2[t₁] (6) t4 = t₁ (8) t6 t5[t4] (9) t7 = t3 * t6  (10) sum = sum + t7 (12) if i ≤ 20 goto B2

B1 (1) Sum = 0 (2) i 1 (4) t2 addr(A) – 4 (7) t5 addr(B) 4 (3) t1 4 * i B2 (5) t3 t2[t₁] (6) t4 t1 (8) t6 tg[t4] (9) t7 t3 * t6 (10) Sum = = sum + t7 (11) i = i +1 (3'') t1 t 1 (l2) if goto B2

B1 (1) SUm 0 (2) i = 1 (4) t2 addr(A) 4 (7) t5 addr (B) 4 (3) t 4 B2 (5) t3 t2[t1] (6) t4 t1 (8) t6 t5[ ] (9) t7 t3 * t  6 (10) Sum = Sum + t 7 ican be removed (ll) i i =  + 1 if not live on exit (3 ) t1 t1 4 (12) if t1 ≤ 80 goto B2

B1 (1) Sum (2) (4) t2 addr(A) 4 (7) t5 addr(B) — 4 (3) t1 4 B2 (5) t3 t2[t1] (6) (8) t6 t5[t1] (9) t7 t3 * t6 (10) Sum I sum t7 (3 ) t 1 t1 4 (12) if t1 ≤ 80 goto B. 2

B1 (1) Sum = 0 (4) t2 = addr(A) − 4 (7) t5  = addr(B) − 4 (3) t 1 4 B2 (5) t3 = t2[t₁] (8) t6 t5[t1]   (9) t7 t₃ * t6 1 = (10) sum = sum + t7 (3'') t1 = t₁ + 4 (12) if t1 ≤ 80 goto B2

t2 D

A t1 )t2 D

B A,B, t5 t1,t3 t2,t4 t

One More Example B t

1 业 2 火 3 4 5 6 7

1 业 2 少 3 4 5 6 7

There are 3 back edges: 5 → 5 7 → 4 4→ 2 There are 3 natural loops: {5} defined by5 5→ 5 {4,! 5, 6, 7} defined  by7 → 4 {2, : 3, ·, 4, 5, '' 6, 7} de fined by 4 → 2

1 火 2 水 3 4 5 6 7

1 2 业 3 4 5 6 7

1 2 山 3 4 5 6 7

An E  Example: E Family of Inducti Basic induction variable: Family of induction variables:

B1 (1) S SUm = (2) i = 1 B2 (3) t₁ = 4 * i (4)   t2 = addr(A) - 4     (5) t3 t2 [七₁] (6) t4 4 * i (7)  t5 addr(B) − 4   =     (8) )  t6 = t t5[t4]  (9) t7 = tz * t6 (10) sum = sum + tγ (11) i = i + 1 (12) if i ≤ 20 goto B2

an xa ele_ StrengthReducti Create a new variable (e.g.ty''and tan for each induction variable in family (e.g.tuand Initialize new variables at the end can thepreheader j=C*i j'' = j + C2 // only if C2 ≠ 0

(1) sum = 0  B₁ (2 ) i = 1 (2a) t₁''= 4 * i (2b) t4'' = 4 * i B2 (3 ) 1 t 1 = 4 * i (4) t2 = addr(A) - 4 (5) t3 = t2[t₁] (6) t4 4 * i (7) t5 =    addr(B) − 4  (8) t6 = t5[t4] (9) t7 = tz * t6 (10) sum = sum + t7 (11) i = i + 1 (12) if i ≤ 20 goto B2 Page40/

(1) sum = 0 B₁ (2 ) i = 1 (2a) t₁''= 4 * i (2b) t4'' = 4 * i B2 (3) t1 = 1 t ₁ (4) t2 = ε addr(A) − 4  (5) t3 t2[t₁] (6) t4 t4'' (7) t5 = addr(B) – 4 (8) t6 = t5[t4] (9) t7 = tz * t6  (10) sum = sum + t7 (11) i = i + 1 (12) if i ≤ 20 goto B2 Page41/

An Example: Sarength Reduct Change the definition of each induction variable (e.g. . t1 and   j=j

an Example Strength Reduct Add linear assignments to new variables following i '' the unique definition of the basic induction variable( ): j=j±t If C ==±l, only one statement needtobeadded

(1) Sum = 0 B1 (2) i = 1 (2a) t1''=4 * i (2b) t4'' = 4 * i B2 (3) t ₁ t1'' (4)  t2 addr(A) - 4 = (5) t3 t2[ti]  (6) t4 t4'' (7) t5 = addr(B) - 4 (8) t6 = t5[t4] (9) t7 t3 * t6 (10) sum = sum + t7  (11) i = i + 1 (11a)t1'' + 4 = t1'' (11b)t4'' = t4'' + 4 (12) if i ≤ 20 goto B2 Page 42/

an xample: liminate Dead If induction variable is not live anexit change the use of to (e.g. change reference from tuand tu and then remove the c definition of (e.g.ti andt4).

(1) Sum = 0 B₁ (2 ) i = 1 (2a) t1''=4 * i (2b) t4''= 4 * i $B₂| （3） 七1 (4) t2 =  addr(A) - 4 (5) t3 = 1 t2[ t₁''] t (6) (7) t5 = addr(B) - 4 (8) t6 [t4''] t5 (9) t7 t3 * t6 (10) sum = sum + t7 (11) i = i + 1 (11a)t₁''= 4 t1'' (11b)t4'' = t4'' + 4 (12) if i ≤ 20 goto B2 Page 43/

(1) Sum= 0 B₁ (2 ) i = 1 (2a) t1''=4 * i (2b) t4'' = 4 * i B₂ (3) (4) t2 addr(A) - 4 (5) t3 = t2[t1''] (6) 七 (7) t5 = addr(B) - 4 (8) t6 t5[t4''] (9) t7 tz * t6 (10) Sum = sum + t7 (11) i i + 1 (11a)t1'' := t1 + 4  (11b)t4'' = t4 + 4 (12) if i ≤ 20 goto B2 (12a)R = 4 * 20 (l2b)if t1'' ≤ R goto B2 Paqe44/

An xample: Change Loopっ C Pick a new induction variable from the family (say ti''), then change the loop condition.

An Example: Remove Basic lr If f the basic induction variable is not live on exit, the definition of can be removed.

(1) sum = 0 B₁ (2 ) i = 1 (2a) t₁''=4 *i (2b) t4'' = 4 *i B2 (4)   t2 =   addr(A) − 4 (5) t 3 = 1 t2[t1 ] (7) t5 =  addr(B) − 4  (8) t6 = = t5[t4 ] (9) t7 = t3 * t6 (10) sum = sum + tγ (11a)t₁'' = t₁'' + 4 (11 b)t4 = t 4 + 4 (12a)R=4*20 (12b)if t₁'' ≤ R goto B2 Page46/

an xample (1) gen[land kil Only variable i a B B1 d₁: i = 2 d2 : j = i + 1 B1  B2 B2 d3 : i = 1 B3 B4 B3 d4 : j = j + 1 B5 B4 d5: j = j -4 B5 d{ a = i dz:b=j

of 1 2 3 k 5 4 6
