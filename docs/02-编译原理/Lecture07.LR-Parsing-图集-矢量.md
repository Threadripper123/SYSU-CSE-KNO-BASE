# Lecture07.LR-Parsing-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture07.LR-Parsing-图集-矢量.pdf　｜　2,804 字符　｜　26 个片段　｜　更新于 2026-11-13
I0 S''→·S

Initial state

-0 S''→·S

KerNEl cYnam S →·a A →bB Nonkernel items or closureitems

S s →·aA S→S· S B

[2 S→a·A A→·cA S→·S S S→·aA S→S• S bB I3 S→b·B

I6 A→·CA I5 A→・d A→d· I2 C I4 M A→·cA A S→aA·  Io a A→d I1 S''→·S S→ ·aASS '→S· S →●bB I3 B · c B

Input string laccd (initialize)

Parsing Decisions tep Sy bol State Iput Refeence Action output 1 $ 0 accd$ S→•aA shift 2 a 0 2 c c d$ A → ● cA shift 3 $ a C 0 26 c d $ A →·cA shift 4 $acc 0266 d$ A→·d shift

Shift-Reduce Parser OPP Parser LR Parser LR(O) Parser SLR(1) Parser LALR(1) Parser LR(1) Parser

Input n + n * n  lookahead cYa  A Logical vs.Physical - LR Parser a Stack search with [top, lookahead] ACTION GOTO

Learn how to build up a formulation!

State ACTION GOTO a b C d $ S A B 0 s2 s3 1 1 acc 2 S6 S5 4 3 s10 s9 8 4 r1 r1 r1 r1 r1 5 r4 r4 r4 r4 r4 6 S6 S5 7 7 r3 r3 13 3 r3 8 2 2 2 2 r2 9 r6 r6 r6 r6 r6 10 s10 s9 11 11 r5 r5 r5 r5 r5

Decisions Based onF Parsing Table Step Symbol State Input Reference Action Output 1 0 accd$ a[0,a =s2 shift 2 $ a 0 2 c cd $ a[2, c] = s6 shift 3 $ a c 0 26 cd$ a[6, c]= s6 shift 4 $ ac c 02 66 d$ a[6, d]= s5 shift

1 $ 0 ad d $ a[0,a]=s2 shift 2 $ i a 0 2 d d $ a[2,d]= s5 shift 3 $a d 02 5 d $ a[5,d]= empty error

ACTION GOTO State id + * ( ) $ E T F 0 s5 S4 1 2 3 1 s6 acc 2 r2 s7 r2 r2 3 r4 r4 r4 r4 4 s5 S4 8 2 3 5 r6 r6 r6 r6 6 S5 S4 9 3 7 S5 S4 10 8 s6 s11 9 r1 s7 r1 r1 10 r3 r3 r3 r3 11 r5 r5 r5 r5

A shift/reduce conflict on input symbol''='', since // Io /''=''∈ FOLLOW(R) S''→●S I2 S●=R S →●R S →L· =R →id R→

S''→S•,$ S→CC•,$ C→d •,$ T T Ta Io Iz I Ig S →· C C, $ S → C· C,$ C → c · C, $ C C C C →·c C, c/d C → • c C, $ C → • C C, $ C → c C•,$ [ →ld,c/dl I4 C I3 I8 C→c·C,c/d d C C→d·,c/d C ·CC,c/d C→cC·,c/d C→·d,c/d

ACTION GOTO State C d $ S C 0 s3 S4 1 2 1 acc 2 S6 S7 5 3 s3 S4 8 4 r3 r3 5 r1 6 S6 s7 9 7 r3 8 r2 r2 9 r2

S →a·Be,$ Io C →c:; a A → •c,d S''→ ·S, $ B → • C, e S→oaAd,s, $l S - → • b B d, $ S → • a B e, $ I3 I4 S— →obAe，$ S→b・Bd,$ b S→b·Ae,$ C C●,e

But why do you ignorei it?

I3 I2 I6 Ig →(·E) +E E - (E ) E→id· id E : E E− →E E ·+E E E → E→(E)· →E•*E d E →E E- E E E E E E E E E E E +E (E) *E i E ュ I i E →I E E E E· +E E E +E (E) *E id

ACaIOn GOTO State id + * ( ) $ E 0 S3 s2 1 1 s4 s5 acc 2 S3 S2 6 3 r4 r4 r4 r4 4 S3 S2 7 5 S3 S2 8 6 S4 S5 S9 7 /s4 r1/ r1 r1 8 /s4 /s5 r2 r2 9 r3 r3 r3 r3

S''→·S →i·SeS S →i·S S i→ iS S − S → ises S s→is· S S →iSe•S → iSeS S ;→oiS S →iS•eS e. a S - s i→•a S → iS S → a i − S iSe a S a a s

ACTION GOTO State i e a $ s 0 s2 s3 1 1 acc 2 s2 s3 4 3 r3 r3 4 r2/ r2 5 S2 s3 6 6 r1 r1

ACaIOn GOTO State id + * ( ) $ E 0 s3 el el s2 e2 el 1 1 e3 s4 s5 e3 e2 acc 2 s3 el el s2 e2 e1 6 3 r4 r4 r4 r4 4 s3 el e1 s2 e2 e1 7 5 s3 el el s2 e2 el 8 6 e3 s4 s5 e3 s9 e4 7 r1 s5 r1 r1 8 _ r2 r2  r2 r2 9  r3 r3  r3 r3 Page82/91
