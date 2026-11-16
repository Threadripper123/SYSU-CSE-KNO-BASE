# ch04 Boolean Algebra and Logic Simplification-图集

> 主题库：数字电路基础　｜　源文件：ch04 Boolean Algebra and Logic Simplification-图集.pdf　｜　4,916 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLINE 4-1 Boolean Operations and Expressions 4-2 Laws and Rules of Boolean Algebra 4-3 DeMorgan''sTheorems ! 4 -4 Boolean Analysis of Logic Circuits 4-5 Simplification Using Boolean Algebra 4-6 Standard Forms ofBoolean Expressions 4–7 Boolean Expressions and Truth Tables 4-8 TheKarnaughMap 4-9 Karnaugh Map SOPMinimization 4-10 Five-VariableKarnaughMaps  4-11 Describing Logic with an HDL

KEY S  Variable 3 - Product-of-sums(PoS) Complement arnaughmap  Sumterm  θ  Minimization Productterm Dontcare   Sum-of-products(SOP)  VHDL

0+0=0 0+1=1 1+0=1 1+1=1

0·0=0 0·1=0 1·0=0 1·1=1

A+B B+A

AB = BA  B A

—A+ (B+C) A+B B+C A+M+C

—ABC) AB BC (AB)C

A X A X=A(B+C) X=AB+AC

TABLE 4-1 Basic rulesof Boolean algebra. 1.A+0=A 7.A·A=A 2.A+1=1 8.A·A=0

3.A·O=0 9.A=A 4.A·1=A 10.A+AB=A 5.A+A=A 11.A+AB=A+B 6. A + A = 1 12.(A + B)(A + C) = A + BC A, B, or C can represent a single variable or a combination of variables.

—X=1 -X= X=A+0=A

A=I A= - X=1 -X=1 X=A+1=1

X= —X=1 X=A+A=A

—x=1 A=1 A=0- —X=1

A☰1 A=O -A=0 A=0 -A=I A=I

TABLE 4-2 Rule 10: A + AB = A. Open file T04-02 to verify. A B AB A+AB 0 1 0 0 1 0 0 1 B 1 1 1

A— straight connection eQual

TABLE4-3 Rule 11: A + AB = A + B. Open file T04-03 to verify. A B AB A+AB A+B 0 0 0 0 0 A 0 1 1 1 1 1 0 0 B 1 1

1 1 0 1 1 A.

Rule 11: A + AB = A + B This rule canbe proved as follows: A+AB=(A+AB)+AB Rule10:A=A+AB =(AA+AB)+AB Rule7:A=AA = AA+ AB+AA+AB Rule8:addingAA=0 =(A + A)(A + B) Factoring =1·(A+B) Rule6:A+A=1 =A+B Rule4:dropthel

TABLE 4-4 Rule 12:(A + B)(A + C) = A + BC. Open file T04-04 to Verify. A B C A+B A+C (A +B)(A+C) BC A +BC 0 0 0 0 0 0 0 0 0 0 1 0 1 0 0 0   0 1 0 1 0 0 0 0 0 1 1 1 1 1 1 1 C- 1 0 0 1 1 1 0 1

0 0 1 1 0 1 1 1 0 1  1 1 1 1 1 1 1 1 C equal

Rule 12:(A + B A + C)= A + BC This rule can be proved asfollows: (A + B)(A + C) = AA + AC + AB + BC Distributive law =A+AC+ AB+BC Rule7:AA=A = A(1 + C)+ AB + BC Factoring(distributive law) =A:1+AB +BC Rule2:1+C=1 = A(1 + B)+ BC Factoring(distributive law) =A·1+BC Rule2:1+B=1 =A+BC Rule4:A·1=A

X Y XY X +Y 0 0 1 1 0 1 1 1 NAND Negative-OR 1 0  1 1 1 1 0 0

211 21 0 0 1 1 0 1 0 0 NOR Negative-AND 1 0 0 0 1 1 0 0

C CD B+ CD A(B + CD) A

TABLE E4-5 Truth table for the logic circuit in Figure 4–18. A B C D A(B + CD) (

0 0 0 0 0 0 0 0 1 0 0 0 1 0 0 0 0 1 1 0 0 1 0 0 0

0 1 0 1 0 0 1 1 0 0 0 1 1 1 0 1 0 0 0 0 1 0 0 1 0 1 0 1 0 0

1 0 1 1 1 1  1 0 0 1 1 1 0 1 1 1 1 1 0 1 1 1 1 1 1

Tmuthitable ACD+A Boolean expression

AB+A(B+C)+B(B+C)· B+AC (a) (b) These two circuitsare equivalent.

LC1   AB ogicConverter-XLC1 Out  A 5 0 0 B E F G H Conversions 002 C 03 A U1 004 11 11 AB 005 008 11 5IP AB AND2 3 07 AB 11 之 A|E NAN□ U4 AND2 OR3 0R2 U3 AC+日

AND2 A B C 1 1 2 3 4

SamplifythefollowingBooleanexpression AB +AC+ABC Step 1: Apply DeMorgan''s theorem to the first term. (AB)(AC)+ ABC Step2: Apply DeMorgan''s theorem toeachterminparentheses. (A+ B)A+ C )+ ABC Step 3: Apply the distributive law to the two terms in parentheses. AA+AC+AB+BC+ABC Step 4: Apply rule 7(AA = A) to the first term , and apply rule 10 [A B + ABC = AB(1 + C) = A B] to the third and last terms. A+AC+AB+BC Step 5: Applyrule 10[A + AC = A(1 + C) = A]to the first and second terms. A+AB+BC Step 6: Apply rule 10 [A + AB = A(1 + B) = A] to the first and second terms. A+BC

= X =AB + BCD + AC

= X=(A +B)(B+C+D)(A+C)

= - X = AB + BCD + AC

TABLE 4-6 A B C X Product Term 0 0 0 0 0 0 1 1 ABC

0 1 0 0 0 1 1 0 1 0 0 1 ABC 1 0  1 0 1 1 0 0 1 1 1 1 ABC

TABLE 4-7 cYa A B C X Sum Term  0 0 0 0 (A+B+ 0 0 1 1

0 1 0 0 (A + B + C) 1 1 0 (A +B + C) 1 0 0 1 1 0 1 0 (A + B + C) 1 1 0 0 (A +B +C) 1 1 1 1

TABLE  4-8 A B C X

0 0 1 0 0 1 0 0 0 1 1 1 1 0 0 1 1 0 1 0

1 1 0 1 1 1 1 1

C C AB  0 1 AB 0 1 00 00 AB ABC 01 01 ABC ABC 11 11 ABc ABC 10 10 ABC ABC (a) (b)

CD CD AB 00 01 11 10 AB 00 01 11 10 00 00 ABCDABCDABCDABCD 01 01 ABCDABCDABCDABCD 11 11 ABCDABCDABCDABCD    10 10 ABCDABCDABCDABCD (a) (b)

AB 00 01 11 10  11

C AB 0 1 ABC   + ABC + ABC + ABC  000 001 110 100 O

ABCD CD 11 AB 00 01 10 00 ABCD  01 1 ABCD 11 1 ABCD 10 1 ABCD ABCD ABCD

C AB 0 1 00 1 1 01 1 1 11 1 10 1 1

CD AB  00 01 11 10 00 1 1 11 1 1 10 1 1 1 1

C C CD CD AB 0 1 AB 0 1 00 01 11 10 00  01 11 10  AB  AB 00 1 00 1 1 00 1 1 00 1 1 01 1 01 1 01 1 1 1 1 01 1 1 1 11 1 1 11 11 11 1 1 1 10 10 1 1 10 1 1 10 1 1 1 (a) (b) (c) (d)

C C CD CD AB 0 1 AB 0 1 AB 00 01 11 10 /00  AB 01 11 10 00 00 0 00 1 01 01 01 01 1 11 11 11 11 10 10 10 10 (a) (b) (c) (d)

CD AB 00 01 11 10 00 1 1 -AC 01 1 1 1 1 B 11 1 1 1 1 10 1 ACD

AB B Ac D BC CD AB C 0 1 C CD AB  0 AB 00 01 11 1 10 AB 00 01 11 10  00 00 00 1 1 00 1  Ac AB 01 01 01 01 1 11 11 AC 11 11 1 10 10 1 10 10 ABC (a) AB ABD Bē (b) (c) (d)

C AB 0 1 00 1  AC 01 1 11 10 1 1 .B

BC CD AB 00 01 11 10 D 00 1 1 1 01 1 1 11 1 1 10 1 1 1
