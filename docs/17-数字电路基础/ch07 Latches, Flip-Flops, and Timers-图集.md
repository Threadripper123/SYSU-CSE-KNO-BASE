# ch07 Latches, Flip-Flops, and Timers-图集

> 主题库：数字电路基础　｜　源文件：ch07 Latches, Flip-Flops, and Timers-图集.pdf　｜　7,465 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLINE 7–1 Latches 7-2 Edge-Triggered Flip-Flops O) oi 7-3 Flip-Flop Operating Characteristics 7-4 Flip-Flop Applications 7-5 One-Shots 7-6 TheAstableMultivibrator 7–7 Troubleshooting System ApplicationActivity

KEYTERMS Bistable Clear SET Propagationdelaytime  RESET  Set-up time  Clock  Hold time  Edge-triggered flip-flop  Power dissipation f -fl Monostable Toggle Astable

(a) Active-HIGH input S-R latch (b) Active-LOW input S-R latch

S G1 G2 R

Momentary LOW 0 0 一 Outputs make taansiionswhen Notransitions SgoesLOWand occurbecause rem ain in same latch is   state afterS alreadySET. G goes backHIGH.   (HIGHD) D) 0 Latchsia sOut RESET (Q=10) Latc stetsout SET(O =1). (a) Two possibilities for the SET operation 1 Outputsmake transitions when R goes LOW and remain 1 goesbackHIGH. 1「 1 Notransitionsoccur becauselatchis alreadyRESET. Latchastanrtsout SET (O = 1 : Latch siamisyoulRESET(O = 0). (b) Twopossibilitiesforthe RESET operation 2 Outputsdo notchange Output states are uncertain state. Latch when input LOWs go back remainsSETif HIGH at approximately  previouslySETand the same time.  G2 remainsRESETif 1  1 previouslyRESET H GHs on both inouts Simoultaneous LOWs On Loth inputs (c)No-change condition (d)Invalidcondition

TABLE7-1 Truth table for an active-LOW input S-Rlatch. s R 2  Comments 1 ! NC NC No change. 0 1 1 0 LatchSET. 1 0 0 1 LatchRESET. 0 0 1 1 Invalidcondition

S O S R -0 -OR (a) Active-HIGH input (b) Active-LOW input

[Ta (a) R 1 1 一一一一 (b) Q

AVCC +Vcc O R W +V R1 W W R2 2 2 s Erratic transition voltage R due to contact bounce O Position Position 1 ● O R 1 1to2 2to1 (a)Switch contact bounce (b) Contact-bounce eliminator circuit

(2) 1S1 (3)  (4)· (1) 1S2 1 Vcc 4S 4R 3R 3Q  1R  4Q 3S2 3S1 (6) 16 15 14 13 12 11 10 9 d 2S (7) (5) .20 d 2R (11) Q 3S1 (12) (9) (10) O 3S2 3  3R 1 3 4 6 8 (15) -Q 4S (13) 1R 1S1 1S2 1Q 2R 2s 2Q GND (14) 4 Q 4R (b) Pin diagram aaaLogicdiagram

S EN EN (a) Logic diagram (b) Logic symbol

S 一 R 一 (a) EN I | 一 Q (b)

D D EN EN Q (a) Logic diagram (b) Logic symbol

D 一 (a) EN | | | (b)

(2) (16) - 1 1D 1 (1)- (13) C1 EN 0 (15) - C20 20 Inputs Outputs (3) EN C0 mments 210 (14) – D Q 2D 20 (6) (10) 3 3D · 0 1 0 1 RESET 1 1 1 0 SET -3 (4) C3 (11) X 0 Q0 ōo No change (9) (7) (8) Note: Qois the prior output level before the indicated 4 4D -4 input conditions were established.

Dynamicinput S D J -Q C C V -C R O- -0 O K O- S- D- Q J- i C C v C R K - (a) S-R (b) D (c) J-K

CLK C t0 10 (a) S= 1,R= 0flip-flopSETS on positiveclock (b) S= 0,R = 1 flip-flopRESETS on positive edge.(IfalreadySET,itremainsSET.) clock edge. (Ifalready RESET.itremains RESET.) 101- S = ② (aoclange) C TO 10- R (c) S= O,R = Oflip-flopdoesnotchange.(IfSET,it remainsSET; ifRESET,itremainsRESET.)

1 CLK 2 5 0 1 S 1 0 1 1 1 1 1 1 1 R I 1 (a) I 0 1 1 1 1 1 1 1 1 1 1 1 1 1 Q (b) 0

G1 G3 CLK Pulse transition detector G2 G4 (a) A simplified logic diagram for a positive edge-triggered S-R flip-flop Shortpulsespike)produced bydelay Delay— (when both gate inputs are HIGH) CLK (b) A type of pulse transition detector

This gate is enabled. This spike SETS flip-flop. T H G (1) Triggering S edge G1 1 Pulse CLK transition — Positive   detector 0 spike HCL G4 G2 LOW(O)1 ThisgateisdisabledbecauseRisLOW.

becauseSisLOW. Triggering S iG G edge 一 1 pulse CLK transition  detector 0 G4 0  This spike RESETS flip-flop.

D S Q CLK- C R-

CLK D  a) 1 C (b) Q 5 0-

TABLE 7-2 Truth t table f for a positive edge-triggered D flip-flop.  ) D CLK Q Q Comments 0 ↑ 0 1 RESET

1 ↑ 1 0 SET ↑: = clock k transition LOW to HIGH

G1 G3 Pulse CLK transition detector G2 4

1 J J O TO TO CLK tO  C TO (a) J = 1, K = 0 flip-flop SETS on positive clock (b) J = 0, K = 1 flip-flop RESETS on positive edge.(If already SET,it remains SET.) clock edge.(If already RESET, it remains

K K O (c) J = 1, K = 1 flip-flop changes (d) J = 0, K = 0 flip-flop does not change. (If SET, it state(toggle). remainsSET;ifRESET,itremainsRESET.

TABLE 7-3 Truth table for a positive  ∈ edge-triggered J-K flip-flop. 6 J K CLK Q Q Comments 0 0 ↑ Qo o No change 0 1 ↑ 0 1 RESET 1 0 ↑ 1 0 SET

1 1 0 0 Toggle ↑ = clock transition LOW to HIGH Qo = output level prior to clock transition

Pulse CLK L transition  detector G2 T HIIGH

1 CLK 2 0 1 J J 0 1 1 1 (a) K 0 CLK. 1 Q 1 (b) 0 1 一 Toggle No Reset Set Set change

CLK (a) 1 1 1 1 1 I I 1 1 I K 1 1 I (b)

FRE C K CIR

PRE Pulse  transition 底 CLR

HGH  K CLR CLK 4 79 PRE (a) CLR Q (b) Preset Toggle — Clear

(4) (2) S 1D (5) - D 1 (3) WOLR >C (6) -1 R PR (4) (2) S (5) -1 B (1) 1D (3) 1D (10) 1CLK 2PRE (1) C1 (6) 1CLR (10) O R ②D (12) S (9) 2PRE D -2 (12) (9) - 2Q - 21) (11) (11) 2CLK 2CLk (8) >C (13) 2CLR O O- -2 (8) - 2 R  2 L (13) (b) Singleblock logicsymbol Note:TheSandRinsidethe blockindicatethatPRE (a)Individuallogicsymbols SETSandCLRRESETS.

(4) PRE (2) S (5) - J 1 1 (3) K (6) -1 R O (15) RE ② 1 O ③ C (2 S 5) 1K R 1 (3) 2 C 9 2 2 2K (11) (7) (13) K -2 2CL R O 2k (11) (7) - 2 (14) (14) O 2CLR 2CLR (a) Individual logic symbols (b) Single block logic symbol

Pin1 (1CLK)  Pin 2 (1J) Pin3 (1K) Pin4 (1PRE) (a)Pin 15 (1CLR) (b) Pin5 (1Q)

50% pointonltiggeuing odge CLK CLK 50%pOint 50 % point on H G to-LOW 5096lpointonLOw-toH cH tansitioniofQ LPE EP E (a) (b)

PRE S0% point) CLR 50% point 30 pOnT 50 POiat (a) (b)

D1 50%POint CLK 50% point om triggering edge

CLK 50% ointon cYaiggeringedge

Comparison of operating parameters for four IC families of flip-flops of the same typeat25C. CMOs Bipolar(TTL) Parameter 74HC74A 74AHC74 74LS74A 74F74 tPHL(CLK to Q) 17 ns 4.6 ns 40 ns 6.8 ns tpLH(CLKtoQ) 17ns 4.6ns 25ns 8.0ns tPHZ(CLRto Q) 18 ns 4.8 ns 40 ns 9.0 ns tPLH(PRE to Q) 18 ns 4.8 ns 25 ns 6.1 ns ts (set-up time) 14 ns 5.0 ns 20 ns 2.0 ns th(hold time) 3.0 ns 0.5 ns 5 ns 1.0 ns tw(CLK HIGH) 10 ns 5.0 ns 25 ns 4.0ns tw(CLKLOW) 10ns 5.0ns 25ns 5.0ns tw(CLR/PRE) 10ns 5.0ns 25ns 4.0ns fmax f 35MHz 170MHz 25MHz 100MHz Power, quiescent 0.012 mW 1.1 mW Power,50% dutycycle 44mW 88mW

D0- D + Q0 C Do0 R D1 9 D21 D1− D +②1 D30_ C Paralel ParaHel CLR data defa iuputs R outputs CLK D₂- D - Q2 Q00 0 C Q1。 0- 020 R p- D - Q3 Elip-rops Daa cleared stored CIK- C (b) R CLR- (a)

HIGH CLK C K CLK

HIGH HIGH B CLK-   K K Flip-flop A Flip-flop B CLK

fin QA 1 I QB 1 1 fOUt 1

HIGH J QA J QB J Oc- f Flip-flop A Flip-flop B Flip-flop C

CLK- K K Flip-flopA Flip-flop CLK LA OB C 2 3 1Banatry Binary seqlence segnence

J QA J QB J Qc - Q CLK- C C -   K K K

CLK QA O QB 0 Qc

RExT. CExT CX RX/CX Trigger Trigger (a) (b)

t1 t2 +V t1 R t1 t2 G1 Trigger  T c t1 t2 t1 t2  Apparent lLOW

Trigger (a)  These pulsesare ignored by the Trigger one-shot. (b) =

TriggER (a) w TriggEr Retriggers (b)

(3) ≥1 &x 1 2 (4) (6) 1]| (3) 1 (5) ∠2 (4) (6) - B (5) B (9) RiNT· RI CExT· (10) (1) RI RX/CX CX − CX (9) (11) RExT/CExT (11) RX/CX   (10)  RINT CEXT REXT/CExT (a)Traditionallogicsymbol (b)ANSI/IEEEstd.91-1984logicsymbol ( =nonlogicconnection). isthe

A1- ≥1 1 Ai- M1 1 A- M1 1 A2-- Q A2- -Q A2- -Q B LT B- D B LT RI CX RX/CX RI CX RX/CX RI CX RX/CX VcC V C CEXT CExT REXT RINT toVCC tW =0.7(2 kΩ)CExT Vc YW=30n c

① ≥1 n (8) ② 8 ③ ④( ④ 9) R (0 CX R 5) (1) R CX RX(X RXCX (9) (10) (1) (5) CLR RINT CEXT REXTCEXT (a) Traditional logic symbol (b) ANSI/IEEEstd. 91–1984 logicsymbol (X =nonlogicconnection). Listhe qualifyingsymbolforareriggerable oneshot.
