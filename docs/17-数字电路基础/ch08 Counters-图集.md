# ch08 Counters-图集

> 主题库：数字电路基础　｜　源文件：ch08 Counters-图集.pdf　｜　7,690 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTER OUTLINE 8-1 AsynchronousCounters 8-2 SynchronousCounters 8-3 Up/DownSynchronous Counters 8-4 Design of Synchronous Counters 8-5 Cascaded Counters 8-6 CounterDecoding 8-7 Counter Applications 8-8 Logic Symbols with Dependency Notation 8-9 Troubleshooting System Application Activity

KEYTERS  Asynchronous  Terminal count Modulus Statedagram Synchrnous

CLK 1 2 3 4 Outputs    Qo (LSB)  Q1(MSB)

HIGH FF0 FF1 J0 20 1 21 CLK  K0 1 ō1

Binary state sequence for the counter ClockPulse Q1 Q0 Initially 0 0  1 0 1 1 0 4(recycles) 0 0

FF0 FF1 Do D1 1 CLK

FF0 FF1 FF2 Do 20 D1 21 D2 Q CLR (a) CLK 8 Qo(LSB)  ) 1 1 1 1 1 Q2(MSB)   -   (b) Reoycles back to O

CLK 1 2 3 4 —tpH (CLK to Q ) - pHL(CLK to Q) pLH pLH(Q。tO Q） - pHL (OtOQ) (CLK toQ。) [pLH (Q₁ to Q2)

FFO FF 1 FF2 FF3 C3 CLK 2 345 6789 23456 Q0 Q1 Q2 (b)

RCLR FF0 FF1 FF2 FF3 o Q3 Do D1 D2 D3 CLK CLR CLR CLR CLR (a) CLK 10 4 20 Q1 Glitch Q3 CLR Ctohr (b)

12decoder HIGH  FF0 FF1 FF2 FF3 0 J0 1 3 J1 J2 Q2 J3 CLR K0 K1 K2 K3 CLR CLR CLR CLR  CLR (a) CLK 4 6 8 9 10 12  Q0 Q1 Glitch Q2. Q3 Decoder (CLR) Glitch (b)

Normalnext stete

HIGH (1) CLKB 6 2 (14) 0 1 2 3 (2) CLR CLR CLR CLR RO(1) T  T 了 (3)  RO(2) CLR (12) (9) (8 (1 1)

CLKA C CTR DIV16 CLKA CTR DIV 10 (a) 74HC93 connected as a modulus-16counter (b)74HC93 connected as a decade counter

HIGH FF0 FF1 LO J0 J1 C C K, K

CLK (a) J-K flip-flop

CLK1 CLK2 Q 4 Propagation delay through FF0 Propagation delay through FFO Q1 Propagationdelay throughFF1 0 (a) (b) CLK3 CLK4 Qo 1  Propagation delay through FFO C Propagation delay through FF0 0 PropagationidelaythnoughFFl (C) (d)

CLK 1 2 3 4 LO

FF0 FF1 01 FF2 J0 1 J2 -2 C C CLK-

CLK 2 3 4 5 8 6 LO Q2 1 1

Summary of the analysis of the counter Outputs J-K Inputs At theNextClockPulse CockPuse Q₂ Q1 Qo J2 K₂ J K₁ Jo Ko FF2 FF1 FFO Initially 0 0 0 0 0 0 0 1 1 NC NC Toggle 0 0 1 0 0 1 1 1 1 NC Toggle Toggle 0 1 0 0 0 0 0 1 1 NC NC 0 1 1 I 0  1 0  1  Toggle 1 1 1 1 Toggle Toggle Toggle 0 1 1 NC NC Togle I 0 0 1 i i NC Toggle Toggle 1 1 0 0 0 0 1 1 NC NC Toggle 1 1 1 1 1 1 1 1 Toggle Toggle Toggle Counterrecyelesbacktoooo. NCindicatesNoChange.

HIGH FF0 FF1 01 FF2 012 FF3 C C K0 K1 K2 K3 CLK (a) Q021 2021Q2 Q021 Q0Q122 CLK QO Q1 Q2 Q3 (b)

HIGH FF0 0 FF1 FF2 FF3 Jo J1 J2 J3 1 2 Q3 C -C C >C K0 K1 K2 K3 CLK

CLK 2 3 4 5 6 7 8 9 10 Q 0 1 1 0 1 1 0 0 0 1 1 Q2 0 0 1 1 0 0 0 一

States ofaBCD decade counter. Clock Pulse I 3 ₂20 nitiay 0 0 0 0 0 0 1 0 0 1 0 0 0 1 1 0 0 0 0 0 1 0 0  1 7 0 1 1 1 8 1 0 0 0 9 1 0 0 1 10 (recycles) 0 0 0 0

Data inpuits D0 D1 D2 D 3 (3) (4) (5) (6) (1) CLR (9) CTRDIV16 LOAD (10) ENT ( (15) (7) TC = 15 -RCO ENP (2) CLK (14)(13)(1 2)(11) Q0Q1Q2Q3, Data outputs

L CLR OAD  CLK ENP ENT RCO 12 1S 14 Cotnt- Inhibit ClearPreset

Datal inputs Data inputs D0 D1 D 2 D3 D0 D1 D2 D3 (3) (4) (5) (6) (3) (4) (5) (6) CLR. (1) (1) (9) C CTR DIV 16 LOAD (9) O CTR DIV 16 d ENT (10) (15) (10) O (15) TC=15 ENP (7) (7) TC=15 -RCO CLK. (2) (2) >C C (14)(13)(12)(11) (14)(13)(12)(11) Q0Q1 Q2Q3 Q0 Q1 Q2 Q3 Data ottputs Data outputs

UpDonseqeceorabitbiayouter Cockase Up 0 Dm 0 2 0 0 1 0 0 1 2 0 1 0 0 1 1 1 0 0 5 1 0 1 6 1 1 0

UP 0UP HIGH FF0 FF1 FF2 J0 J1 J2 UPIDOWN C C v C K0 K1 K2 DOWN 2·DOWN

UP/DOWN Up Down: Up K Down CLK (a)                     Q0 0 0 22 01010 1 0 0 01010 010101010

LOAD D0 D1 Data inputs D2 D3 CLK DIU CTEN Data 01 outputs 2 MAX/MIN RCO  8 1 1 2 2 9 1 8 1 1 1 Count up -Inhibit- Count down Load

(15)(1) ) (10)(9) (4) CTEN (12) DO (5) MAX/MIN LOAD (11) CTRDIV10 CK (14) >C (13) (3)( (2) (6) (7)

CLK ExcitathonMines  Input   Hhputs combinational  @h Memory | logic V ouitputs    m 10 121 Suatevariablelines

VOOO 100 001 101 011 111 010 110

Next-state table for 3-bit Gray code counter. Q2 Q1 Q0 Q₂ Q1 0 0 0 0 0 0 1 0 1  0 1 0 1 0 0 0 1 1 0 0 1 1 1 1 1 0 1 1 0 1 0

Transition table for a J-K flip-flop. Output Transitions Flip-Flop Inputs QN QN+1 J K 0 0 0 X 0 1 1 X 1 0 X 1 1 1 X 0 X:"don''t care"

Jomap Komap Q20个 1 Q2Q 1 ThevaluesofJoandKorequired 00 1 00 X cYaa placed on each map in the present-state cell. 01 01 11 11 10 X 10 The values of J0 and K0 required  cYa placed on each map in the present-statecell. Output Flip-Flop PresentState NextState ForthepresentstateOOO, Qo makes a transition from Oto 1 Transitions Inputs Q2 Q1 Qo Q2 Q1    Qo 1 to the next state.   QN QN+1 J K 0 0 0 0 0 1 0 →0 0 X 0 0 1 0 1 1 O  1 1 X 0 1 1 0 1 0 For thepresent state101,Qo 0 X 1 0 1 0 1 1 0 makes atransition from 1 to 0 1 X 0 1 1 0 1 1 1 to the next state. 1 1 1 1 0 1 0 1 Flip-flop transition table 1 0 0 1 0 0 0 0 0

Q21 0 0 1 2221 2 0 0 0 1 221 0 1 00 0 0 00 0 220 1 00 1 X −2q1 01 0 01 X X 01 0 X 210 - 11 x X 11 X X 11 1 X -Q2Q1 10 X X 10 0 0 10 0 X 221 0 0 1 221 2\0 0 1 Q21 0 0 1 00  X 00 X X 00 X 0 01 X X 01 0 0 01 X 1 2Q1 11 0 0 11 0 1 Q220 0 11 X 10 0 10 X X 10 x 1 2 2,0 K2 map K1map K0map

J0 = Q2Q1 + O2O1 = Q2 ⊕ Q1 K0 =Q2Q1+Q2Q1=Q2 Q1 J1= 2Q0 K1=Q2Q0 J2=Q1Q0 K2=Q1Q0

FF0 FF1 FF2 J0 1 1 J2 C  C >C K0 K1 K2 CLK 0 の

1. Specify the counter sequence and draw a state diagram. 2. Derive a next-state table from the state diagram . 3. Develop a transition table showing the flip-fop inputs required for each transition. The transition table is always the same for a given type of fip-flop. 4. Transfer the J and K states from the transition table to Karnaugh maps. There is a Karnaugh mapforeachinputofeachflip-fop. 5. Group the Karnaugh map cells to generate and derive the logic expression for each 6. Implement the expressions with combinational logic,and combine with the fip-flops tocreatethecounter.

O01 11i 010 (②) 101

Next-state table. Present State Next State Q2 Q1 Q0 Q2 Q1 Q0 0 0 1 0 1 0 0 1 0 1 0 1 1 0 1 1 1 1 1 1 1 0 0 1

Transition table for a J-K flip-flop. Output Transitions Flip-Flop Inputs QN QN+1 J K 0 0 0 X 0 1 1 X 1 0 X 1 1 1 X 0

HIGH HIGH J01 J1 2 C C C K0 K1 K2 CL

_221 0 0 1 e21 0 0 1 Q221 0 0 1 01 1 e1 01 01 1 11 x 11 x 11 x 10 x 10 10 x J2map J1map Jmap Q221 0 0 0 1 Q21 0 0 1 Q21 0 1 -2 00 x x 00 1 00 1 01 01 1 01 11 1 11 11 0 10 0 10 X 10 0 2map 1map map

Transition tablefora Dflip-flop. Output Transitions Flip-Flop Input ON QN+1 D 0 0  0 0 1 1 1 1

_21 20 0 1 221 20 0 221 0 0 1 00 0 00 1 00 0 01 01 0 01 1 10 1 10 1 10 Q21 Q1 Q2 D2map D1map P0map

D0 D 1 D2 CLK

1 000 Y=1 100 0 Y=0 001 1 1 0 0 1OT 0I1 0 0 1 111 0 010  1 110 1

Next-state table for 3-bit up/down Gray code counter. [Ta Present State Y = 0 (DOWN) Y = 1(UP) Q2 Q1 Q0 Q2 Q1 Q0 Q2 Q1 Q0 0 0 0 1 0 0 0 0 1 0 0 1 0 0 0 0 1 1 0 1 1 0 0 1 0 1 0 0 1 0 0 1 1 1 1 0 1 1 0 0 1 0 1 1 1 1 1 1 1 1 0 1 0 1 1 0 1 1 1 1 1 0 0 1 0 0 1 0 1 0 0 0 Y= UP/DOWNcontrolinput.

221 00 01 11 10 221 00  01 11 10 2221 00 01 11 10  00  0 0 0 00 0 0   0  ₂ 00 0   x 01 0 1 0 0 - 220 01 x x x x 01 0 X x 2Y 11 x x x x 11 x x x x 11 0 1 x x 10 X X X X 10 0 0 0 1 10 1 0 X X ∂,o0y J2 map J1map e20r J0map e2−|y 2221~ 00 01  11 10 221 00   01 11 10 Q2Q1 00 01 11 10  00 x x x 00 x X x x 00] X 0 1 01 x x x x 01 0 0 0 1 -∂₂e,x 01 x X 1 0 21ā - 11 0 0 0 11 0 0 0 11 x x 0 1 10 0 1 0 0 10 x x X X 10 x X 1 0 e221 Q1Q0Y K2 map K1map K0map Q2Q0Y Q2Q|Y

y J0 0 C K 0 2 C K2 MCLK

HIGH HIGH J0 1 J2 J3 J4 K0 K 1 K2 K4 0 0 K3 Modulus-4 counter Modulus-8 counter

CLK  1112 i314 i5 16 i7 i8 i9 i10i11i12i13i14i15i16i17i18i19i20i21i22i23i24i25i26i27i28i29i30i31i32

HIGH Counter1 fin Counter2 CTEN CTEN TC 100 CTR DIV10 TC CTR DIV10 CLK   Q0 Q1 Q2 Q3 Q0 Q1 Q2 Q3

HIGH 1OOkHZ 1OkHz 1kHz CTEN TC CTEN TC CTEN  ● TC CTR DIV10 CTR DIV10 CTR DIV10

Input CTRDIV8 CTRDIV12 CTRDIV16 →Output Input CTR DIV 10 CTR DIV 4 CTR DIV 7 CTR DIV 5 → Output
