# ch09 Shift Registers-图集

> 主题库：数字电路基础　｜　源文件：ch09 Shift Registers-图集.pdf　｜　5,571 字符　｜　46 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTER OUTLINE 9-1 BasicShiftRegisterOperations 9-2 Serial In/Serial Out Shift Registers 9-3 Serial In/Parallel Out Shift Registers 9–4 Parallel In/Serial Out Shift Registers 9-5 Parallel In/Parallel OutShiftRegisters 9–6 BidirectionalShiftRegisters 9-7 ShiftRegister Counters 9-8 Shift Register Applications 9-9 Logic Symbols with Dependency Notation 9–10 Troubleshooting SystemApplicationActivity

llis stored and appeams on output. ois stoiedandappearsionioutout. (1) D 0) D WhenalisonD WhenaOisonD L Q becomes a 1 at the CL .  Q becomes a 0 at the triggeringedgeofCLK triggeringedgeofCLK orremainsaIifalready orremainsaOifalready intheSETstate. intheRESETstate.

Datain 1 Da out Dataot6 DatalBn Dataout v (a) Serial in/shift right/serial out (b)Serial in/shift left/serial out (c) Parallel in/serial out a (d) Serial in/parallel out (e) Parallel in/parallel out (f) Rotate right (g) Rotate left

FF0 FF1 FF2 FF3 Serial Qo Q1 O2 Q3 data D D D D Serial data ouitput Serial data output CLK ●

Data in SR G 8 CLK C

Shifing a 4-bitcodeinto theshiftregister Data bitsare indicated bya beigescreen.  CLK FF0(Qo) FF1(Q1) FF2(Q2) FF3(Q3) Initial  0  0 0 0 0 0 0 0  0 0 0 1 0 Shiftinga 4-bitcode outof theshiftregister Data bits areindicated bya beige screen. CLK FFO(Qo) FF1(Q1) FF2(Q2) nitia 1 FF3(Q3) 0 0 0 0 0 0 0 0

FF0 FF1 FF2 FF3 FF4 Data Qo Q1 2 3 Q4 input D D D D D Data Output C C C C C CLK  CLK Data input 0i (a)  QO 0 Q1 Q2 after five Q3 Q4 (b)

Data imnput D  D  D  D Datainput D SRG 4 L Q0 Q1 Q2 03 Q0Q1Q2Q₃ (a) (b)

Data in CLK Q0 Q2 Q3

(1) A SRG 8 (2) (9) CLR (8) CLK C

CLR A Serial - 1 inputs B CLK Q0 Q1 Q2 Q3 Q4 Q5 Q6 Clear Clear

D0 D1 D2 D3 SHIFTILOAD G G G2 G6 G3 G G4 Serial D D Qe Ωi D Ω2 D data Q3 >C OUt C C FF0 FF1 FF2 FF3 Datuin CLK 2 3 4 6 D0 D1 D2 D3 SHIFTILOAD SHIETILOAD SRG4 Serialdata out CLK C Dataout(Q3) 0 0 Lastdata bit

Do DDD D D D D (11 ) (12)(13)(14)    (3) (4) (5) (6) (1) (9) (10) SER (15) CLKINHI (2)

CLK CLKINH SER0 (LOW) SHILD Do D1 D2 D3 0 Data D4 D5 1O D6 D7 1 2 0 Inhibit Serial shift Load

Parallel data inputs Do D D2 D3 D D D D C C C CLK Q0 Q 2 3 Parallel data outputs

Do D1 D2 D3 (4) (5) (6) (7) Serial  SRG4 inpuits  3 (9) SHILD (1) CLR (10) (15) (14) (13) (12) Co O1 Q2 Q3

CLK CLR Serial J inputs K SHILD Do Parallel D1 DATA inputs D2 D3 QO Parallel e1 Q2 Q3 Serial shift Serial shift  Clear Load

RIGHTILEFT Serial  data in G1 G5 G2 G6 G3 G7 G4) G8 D D D C C  CLK

RIGHT/LEFT (igh) (left) (right) (lef) (a) CLK 十一一一 1 2。 1 0 0 1 0 0 Q1 1 0 1 1 0 1

Q2 1 1 1 1 1 0  (b) Q3 0

Da D1 D2 D3 (3) (4) (5) (6) (1) CLR SRG4  (9)  (10) (2) SRSER (7) SL SER (11) (15) (14) (13) (12) Qo Q1 O2 Q3

CLK Mode So 1 1 controlE 4 1 inpmts CLR Seral SRSER data puts SL SER Do  ParallelI D1 dara aputs

Shiftright Shint lefu Inhibit Clear ead Cle

FF0 FF1 FF2 FF3 Q0 2 2 D D D D C V C V C V C CLK (a Four-bitJohnson counter FF0 FF1 FF2 FF3 FF4 Qo Q1 Q2 Q3 C C C C ·C   V CLK    (b)Five-bit Johnson counter

Four-bit Johnson sequence.  ClockPulse Q0 Q1 Q2 Q3  0 0 0 0  1 0 0 0 1 1 0 0 1 1 1 0 1 1 1 1 0 1 1 1 0 0 1 1 0 0 0

CLK 2 3 4 5 6  7 8 1 1 Q0 1 一 1 1 一一 Q1 1 1 一一 1 1 1 Q2. 1 1 Q3

CLK 2 3 4 5 6 8 10 Q0. 一一一一一 1 一一一一上上一一 Q1. Q2. 一一 1 一一 1 Q3. 1 Q4

Five-bitJohnsonsequence. ClockPulse Q0 Q1 Q2 Q3 Q4 0 0 0 0 0 0 1 1 0 0 0 0 1 1 0 0 0 1 1 1 0 0 1 1 1 1 0 1 1 1 1 1 0 1 1 1 1 0 0 1 1 1 0 0 0 1 1 9 6 0 0 0 0

PRE Q0 1 Q2 Q3 Q4 Q5 Q6 Q7 s D D D D D D D D D D 9 CLK

CLK 3 10 1 1 1 1 1 Q0 1 1 1 1 Q2 1 1 1 1 1 1 1 1 Q3 QA 十 T 十 Q5 一一 1 T T 1 1 1 1 Q6 1 Q7 LO Q9

Ten-bitring counter sequence. ClockPulse Q0 Q1 Q2 Q3 Q4 Q5 Q6 Q7 Q8 Q9 0 1 0 0 0 0 0 0 0  0  1 0 1 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0  0 0 0 1 0 0 0  0 0 00 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 0 0 0 0 0 1 _

SRG8 O7 Data in Data out CLK  1MHz 1μs CLK I I Data in . I Dataout ta=8 Ms

Data in SRG 8* CLR CLK C 500kHz

Qo Qi O2 O3 Q4 O5 Q Q7 aData shifts from Qo toward Q .

CLK Datai Q0 Q1 Q2 1 Datal 十十 1 06 16μs 1

HIGH LOW Do (4) D (5) D2 (6) D3 (7) (2) SRG4 (3) ) (9) SHLD (1) CLR (10) CLK C (15) (14) (13) (12)

SHILD CLK Qo 1 Q1. 一 1 I 1 1 Q3

Serial D SRG8 0 data in Control flip-flop CLK GEN Data-input 」 CLK register HIGH- J EN Q  C QoQ1 2Q34256QT K CLR LOAD SRG8 CTRDIV8 CLR Data-output Tc O register 「 「 Do D1 D2 Dz D4 D5 D6 D7 1 Parallel data out One-shot TCoCLK

Start bit (0) q  s D4 Stop D3 Stop D2 D 1 o 6 i bit (1)i bit (1

D D6 D5 D4 D3 D 2 D1 D0 Serial Start data in bit  0 0 0 1 0 Stop bits Control flip-flop Q CLK Q0. Q1 Q2. Data 03 input register 4 Q5 06 Q7 TC·CLK CLR Do _ O D1 D2- 0 Data D3 output  og ster D4 D5 - 0 D6 _ 0 D{

Data bus Transmitter Receiver dataregister dataregister Transmitter Receiver CLK : parallel in/serial CL K · serial in/parallel autshiftregister outshif register Serial data out Serial data in

Paralel data busl Serial dlata ouit Micro- External processor UART Serial data in device system system , etc.)

Power on LOAD SH/LD +Vcc Ring count SRG4 d CLK SRG4 (5 kHz) C >C +V 20222Q 45 27 ya Clock inhibit 1 2 3 4 5 6 7 8 1 2 3 4 5 6 7 8 ROWencoder COLUMNencoder 1 2 4 1 2 4 Switchclosuref 1 DoDDD4 Key code register c C 26Q 223 5 One-shots To RON

(9)。 SRG8 CLR R (8) CLK C1/→ (1) A- (3) (2) 1D Q0 B (4) (5) 22 (6) Q3 (10) Q4 (11) 25 (12) 26 (13)

(1) SRG4 CER R S- (9) 0 S- (10) M= 1 (11) CLK C4 >1—一/ — (2) SRSER 1,4D (15) Do 3,4D D (4) (14) 3, 4D 1 D2 (5) (13) 3, 4D (6) Q2 · 3,4D (7) . (12) SLSER 2,4D
