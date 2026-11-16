# ch03 Logic Gates-图集

> 主题库：数字电路基础　｜　源文件：ch03 Logic Gates-图集.pdf　｜　3,633 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLIE 3-1 TheInverter 3-2 TheAND Gate 3-3 TheORGate 3-4 The NAND Gate 3-5 TheNORGate 3-6 The Exclusive-OR and Exclusive-NORGates 3—7 Fixed-FunctionLogic 3-8 Troubleshooting 3-9 ProgrammableLogic

KEYTERMS  Inverter  Complement  Truth table  AND gate  Timing diagram  Enable  Boolean algebra  ORgate NANDgate ANDaay  Exclusive-OR gate  Antifuse  Exclusive-NORgate  EPROM  CMOS  EEPROM  Bipolar  Flash  Propagationdelay time SRAM I  Fan-out  Targetdevice Unit load TAG

(a) Distinctive shape symbols (b) Rectangular outline symbols with negation indicators with polarity indicators

TABLE 3-1 Inverter truth table.

Input Output LOW HIGH

HIGH I (1) L OW (O)

HOGH (1 HGH LOW (0) LO (0) t1 t2 t1 t2 Input pulse Output pulse

Input Output 一一 t1 t2

0 Input   − Output

1 Input 0 一一一 1 Output 0

A- O-- X=A

1 1 0 1 0 0 0 1

A- A- &x -X -X B B (a) Distinctive shape (b) Rectangular outline with the AND (&) qualifyingsy bol

LOW (0) LOW (0) LOW (O) LOW (0) LOW (O) HGH (1)- HIGH (1) HIGH (1) LOW (O) HIGH (1) LOW (O) HIGH

TABLE 3-2 Truth table for a 2-input

A D gate.

A B X 0 0 0

0 1 0 1 0 0 1 1 1

1 = HIGH, 0 ) = LOW

TABLE 3-3 Inputs Output

A B C X 0 0 0 0

1 1 1 1

A          A I 一 |X B B 1 1 t1− −t2− −t3− −t5 一一

A HIGH LOW A Inputs X B HIGH B I  LOW 1 一 1 1 1 1 一 Output X HIGH 1 1 1 |  LOW

1 kH2 74LS11N  00000000000000000000000000001001 15 Ready C TniggerC R [a a  Time Channel_A Channel_B Channel_C Channel_D Reverse  856.820 ms 500.000 mV 4.500 √ 4.500 √ 0.000 V T2 Save Timebase Channel_D Trigger- A Scale 2 ms/Div Scale 5 VDiv Edge F Ext 日 Xposition 回 Yposition -2.6 D Level 回 √ YT ABA+B C AC 0 DC     Sing. Nor. Auto None A Et

A = X=AB X=ABC = X=ABCD B (a) (b) (c)

TABLE 3-4 A B AB X

0 0 0·0 = 0 7 0 1 0·1 = 0  1 0 1·0  = 0

1 1 1·1 = : 1

Counter Enable -1ms- Register, Resettozero decoder betweenenablepulses. and frequency display

HIGH = On Ignition A LOW =Off switch Audible HIGH=Unbuckled Sat B alarm LOW = Buckled belt C circuit H GHaCLivatES alarmm Timer Ignition on = H GH for 3Os

A A- ≥1 -X- X B B (a) Distinctive shape (b) Rectangular outline with the a M qualifyingsymbol

LOW (0) LO (0) LO (O) HIGH (1) LOW (O) HIGH (1) HIGH (1) HIGH (1) HIGH (I) HIGH (① LOW (O) HIGH C

TABLE 3-5  -  Truth table for a 2-input

n gate. Inputs Output

A B X 0 0 0

0 1 1 1 0 1 1 1 1

1: = HIGH, 0 = LOW

1 A      A- X 1 B -t2- -t3- -t4 1

InputA A X B I I OutputX When either input or both inputs are HlGH, the outputis H GHI.

A = B X=A+B X=A+B+C = X=A+B+C+D (a) (b) (c)

TABLE 3-6 A B  A+ -B=X -

0 0 0 + 0  = 0  0 1 0 + 1 = 1 1 0 1 + 0  ) = 1 -

1 1 1+1=1

Open door/window HIGH = Open LOW = Closed HIGHactivates alarmml Alarm

(a) Distinctive shape, 2-inputNAND gate and its (b)Rectangular outline, 2-input NAND

1 = HIGH, 0 = LOW.

LOW (0) LO (0) HIGH (1) HIGH (1) LOW (0) HIGH (1) HIGH (1) HIGH (1) HIGH  (1) LOW V(O) LOW (O) HIGH (1)

TABLE3-7 Truth table for   a  2-input

NAnD gate. Inputs Output

A B X 0 0 1

0 1 1 1 0 1 1 1 0

A A. X B B Bubbleindicates an active-ILOW output.  X Aand Bare both HGH during these four time intervals: therefore, X is LOW.

NAND Negative-OR

TankA Levelsensor HIGH LOW Green light indicates both HIGH tanks are greater than TankB 1/4 full. Levelsensor

TankA HIGH LO W Red light andnicatesl Tank B one or both tanks are less I than 1/4 full.

TABLE 3- A B AB = :X

1 1 1·1 = 1 = 1 0

(a) Distinctive shape, 2-inputNOR gate and its NOT/OR (b)Rectangular outline, 2-input equivalent NORgatewithpolarityindicator
