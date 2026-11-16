# ch05 Combinational Logic Analysis-图集

> 主题库：数字电路基础　｜　源文件：ch05 Combinational Logic Analysis-图集.pdf　｜　7,437 字符　｜　54 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLINE 5-1 Basic Combinational Logic Circuits 5-2 Implementing Combinational Logic 5-3 TheUniversalPropertyofNANDandNORGates 5-4 Combinational LogicUsingNANDand NORGates 5-5 Logic Circuit Operation with Pulse Waveform Inputs 5–6 Troubleshooting 5-7 Combinational LogicwithVHDL

A- ≥1 A. AB SOP B- B.. X=AB+ CD &x D D CD (a) Logic diagram (ANSI standard distinctive (b) ANSI standard rectangular outline symbol shaesy ols

A B C G3 X Low-level G2 indicator G1

A· _AB POS A &x ≥1 B· AB+CD AB+CD=(A+B)(C+D) B (a) (b)

A B C G3 X Low-level   J2  indicator G1

B- A A- -X =1- B B- X (a) Logic diagram (b) ANSI distinctive (c) AN SI rectangular

XOR A A B B AB (a) X = AB + AB (b) X = AB + AB

Dat bts Evenatyb

Data biti Error Even parity bit

A AB X= AB + CDE CDE

二 C- ABCD - CD ·X=AB(CD +EF) D- D 四 X=ABCD+ABEF HEF ABEE E (a) (b) Sum-of-products implementation of the circuit in part (a)

TABLE5-3 anputs Output A B C X  Product Term 0 0 0 0 0 0 1 0 0 1 0 0 0 1 1 1 ABC 1 0 0 1 ABC

1 0 1 0 1 1 0 0 1 1 1 0

A A· ABC B· X=ABC+ABC AB

TABLE 5-4 A B C X Product Term 0 0 0 0 0 0 1 0 0 1 0 0 0 1 1 1 ABC

1 0 0 0 1 0 1 1 ABC 1 1 0 1 ABC 1 1 1 0

ABC ABC ABC

A. B X C D

The expression for the outputof the circuit is X=(ABC)C+ABC+D ApplyingDeMorgan''s theoremand Booleanalgebra, X=(A+B+C)C+A+B+C+D =AC+BC+CC+A+B+C+D =AC+BC+C+A+B+C+D S F2 =C(A+B+1)+A+B+D X=A+B+C+D The simplified circuit is a 4-input OR gate as shown in Figure 5–15. FIGURE5-15 693  020

三

CD  AB 00 01 11 10 00 1 1 B 01 X 11 —ACD B 10 1 1 (a) (b)

A- -A A- A A- AB B- AB=AB AB (b) TwoNANDgates used as an ANDgate A- G G3 -AB=A+B  A+B B- G2 B (c) Three NAND gates usedas an OR gate A A+  G -A+B A+B B (d) FourNANDgates used as aNORgate

(a) One NOR gate used as an inverter A+B -A+B A+B aaTwoNORgatesusedasanORgae A G3 A- -A+B=AB AB B- A· T G4 AB AB AB 4· B- B 福 (d)FourNORgates used as aNANDgate

G2actsasAND G2  G1 AB+CD G3 G acts as OR G3 acts as AND a OriginalNANDlogicdiagramshowingefectve gate operation relative to the output expression Bubblescancel A= G2 A= G2 = G1 AB+CD = G1 AB+CD G3 G3 Bubblesancel (b)Equivalent NAND/Negative-OR logic diagram (c)AND-OR equivalent

a AB ABC ABCD D (ABCD)EF ABCD)E =(AB+C)D+EF =(AB+C)D+EF (a) Several Boolean steps are required to arrive at final output expression. AND AB AND AB+ (ABC)D Bubble Cancels D bar (AB + C)D + Bubbleadds bar toC  OR EF Bubble cancelsbar OR AND (b) Output expression can be obtained directly from the function of each gate symbol in the diagram.

A G3 B G2 G1 - X D E G5 G4

A A+B B (A+B)C x=(A+B)C+D+)F D D+E. G5 E +E)F

AB Bubble cancels bar  ABC Bubblecancel bar ABC+DE = -ABC+D+ DE Bubble cancels bar Bubbles add bars to D and E (a) (b)

G -X=(A+B)(C+D) 1

G2acts as OR Bubbles cancel (A+B)(C+D) G1 (A+B)(C+D) G1actsasAND aubblesancel (a) (b)

- A+B A+B+C A+B+C+D C D = A+B+C+D+E+F =(A+B+C+D)(E+F) E+F (A+B+C+D)(E+F) =((A+B)C+D)(E+F) (A+B)C+D)(E+F) ca OR Bubblecancelsbar A OR A+B (A+B)C B (A+B)C+D Bubble C cancelsbar D [(A=B)C+D](E+F) BubbleaddsbartoC AND E+F Bubble cancels bar [A OR (b) Output expression can be obtained directly from the function of each gate symbol in the diagram.

A B T 2 C G1 - D E G4 E

A G2 AB X =(AB+ C)(DE+ F)=(AB +C)(DE+ F) B- D DE G5 G4 DE+F

A A X Inputs·S B 1 B C Y X X=A(B+C)=AB+AC

A+ ABC X=(A+B)C+CD=(A+B)C+CD=AC+BC+CD

BC CD AC AC A 1 B 一 1 一一 1 一一一一一一一一一一 1 X=AC+BCCD I I

Interconneetioms Inputsodefirred in oort statenrent Signala device VHDL A device VHDL device VHDL B com ponent Ourtonuitdleiineal m erestatee (a)Hardware implementationwith fixed-function logic (b)VHDL structuralimplementation

entityAND_gateis port (A, B: in bit; X: out bit); A end entity AND_gate; B architecture ANDfunction of AND_gate is X <= A and B; 2-input AND gate end architecture ANDfunction; enityOR_gateis port (A, B: in bit; X: out bit); 4 end entityOR_gate; B architecture ORfunction ofOR_gate is X<=AorB; 2-input OR gate end architecture ORfunction;

IN1 OUT1 G1 IN2 G3 OUT3 IN3 G2 IN4 OUT2

architectureLogicOperationofAND_OR_Logicis component AND_gate is Component declaration for theANDgate port(A,B:inbit); X:outbit); endcomponentAND_gate; theORgate end componentOR_gate; signalOUT1,OUT2:bit;  Signaldeclaration begin G1:AND_gateportmap(A =>IN1,B=>IN2,X=>OUT1); G2:AND_gateportmap(A=>IN3,B=>IN4,X=>OUT2); Component  G3:OR_gate port map(A => OUT1, B =>OUT2, X =>OUT3), instantiations

A=>IN1 X=>OUT1 IN1 G1 X B  IN2 A=>OUT1 X=>OUT3 B=>IN2 OUT1 A G3 X OUT3 A=>IN3 OUT2 IN3 A B=>OUT2 G2 X Ba 0UT2 IN4 B=>IN4 X=>OUT2

entitySOP_Logicis  port(IN1,IN2,IN3,IN4,IN5,IN6,IN7,IN8:inbit;OUT4:outbit); end entitySOP_Logic; architecture LogicOperation of SOP_Logic is O - emponentdeclaraionfor -inutNANDgate componentNAND_gate3is  port(A, B,C:inbit X:outbit); end componentNAND_gate3; --componentdeclaration for2-inputNANDgate componentNAND_gate2 is port(A,B:inbit;X:outbit); endcomponentNAND_gate; signalOUT1,OUT2,OUT3:bit; begin 0 8 68 => 01000 G1:NAND_gate3portmap(A=>IN1, B=>IN2,C=>IN3,X =>OUT1); G2:NAND_gate3portmap(A=>IN4,B=>IN5,C=>IN6,X=>OUT2); G3:NAND_gate2portmap(A=>IN7,B=>IN8,X=>OUT3); G4:NAND_gate3portmap (A=>OUT1, B=>OUT2,C=>OUT3,X=>OUT4); end architecture LogicOperation; o s 0 m 2OTGJOAVA

IN1 OUT1 IN2 G1 IN3 IN4 OUT2 IN5 G2 G4 - OUT4 IN6 IN7 UT3 G3 IN8

TextEditor □  File Edit View Project Assignments Processing Tools Window entity Combinational is port(A,B,C,D:inbit;X,Y:outbit); anaentityCombinational arehitectureExampleofCombinationalis bEgiN X<=(AandB)ornotC; Y<=CornotD; end architecture Example;

Waveform Editor □ 区 Name: 50.0 ns 100.0 ns 150.0 ns 200.0 ns 250.0 ns 300.0 ns D I 1 1 1 1 1 1 1 1 1 C I 1 1 1 1 1 1 1 1 1 B I 1 1 1 1 A 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 X

Inlet valve Flow sensor Finlet Lmax Monitoring Vinlet and control T Level Lmin logic Voutlet sensors Tanalog Outlet Heater Temperature valve transducer To mixing vat

TABLE5— Truth table forinlet valve control. max Lm n mlet Vinlet Description 0 0 0 1 Level below minimum. No inletflow. 0 0 1 1 Level below minimum.Inlet flow. 0 1 0 0 Level ε above min and below m ax. No inlet flow. 0 1 1 1 Levelε above min and below max.Inlet flow. 1 0 0 X Invalid

1 0 1 X Invalid 1 1 0 0 Level at maximum.No inlet flow. 1 1 1 0 Level at maximum.Inlet flow.

TABLE 5-7 Lmax Lmin Finlet T Voutlet Description 0 0 0 0 0 Level below r minimum.No inlet flow.Templow. 0 O O O Level below minim um . No inlet flow. Temp correct.

0 0 1 0 0 Levelbelow minimum.Inlet flow.Temp low. 0 0 1 1 0 Level below minimum.Inlet flow.Temp correct. 0 1 0 0 0 Level above min and below max.No inlet flow.Temp low. 0 1 0 1 1 Level above min and below max. No inlet flow.Temp 0 1 1 0 0 Level above min and below max.Inlet flow.Temp low. 0 1 1 1 0 Level above min and below max.Inlet flow.Temp

0 1 1 Invalid  1 1 0 x Invalid 1 1 1 Ivalid 1 1 0 0 0 Level at maximum.No inlet flow.Temp low. 1 1 0 1 1 Level at maximum.No inlet flow.Temp correct. 1 1 1 0 0 Level at maximum.Inlet flow.Temp low. 1 1 1 1 0 Level at maximum.Inlet flow.Temp correct.

8-bit 8-bitBCDfor binary code measured temperature Analog-to- Binary-to- Taiog Temperature- digital BCD T control logic converter converter 8-bitBCDfor specified temperature

TM8 TM7 TM6 BCD for TMs temperature (ZM) T 4 . TM3 TM2 TM TSg TS7 TS6 TS5 TS4 TS₃ TS2 TS1 BCD for specified temperature(TS)
