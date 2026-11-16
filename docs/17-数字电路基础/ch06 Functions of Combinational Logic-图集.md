# ch06 Functions of Combinational Logic-图集

> 主题库：数字电路基础　｜　源文件：ch06 Functions of Combinational Logic-图集.pdf　｜　8,760 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTEROUTLINE 6-1 Basic Adders 6-2 ParallelBinaryAdders 6-3 Ripple Carry Versus Look-Ahead Carry Adders 6-4 Comparators 6-5 Decoders 6-6 Encoders 6-7 CodeConverters 6-8 Multiplexers(Data Selectors) 6-9 Demultiplexers 6-10 Parity Generators/Checkers 6-11 Troubleshooting

Half-adder Ripplecarry  Full-adder br qo Look-aheadcarry  Encoder Demultiplexer (DEMUX)  Priority encoder  Parity bit M ultiplexer(M Ux) Glitch

A M Sum Input bits Outputs B Cout Car y

TABLE 6-1 Half-adder truth table.

A B  Cout 乙 0 0 0 0 0 1 0 1 1

1 0 0 1 1 1 1 0 Σ = sum

A and B = input variables (operands)

Σ=A⊕B=AB +AB COUL AB

Iput A M Sum bitS B Cout Output carry Input carry C; in

TABLE 6-2 Full-adder r truth table. A B Cin Cout ∑

0 0 0 0 0 0 0 1 0 1 0 1 0 0 1 0 1 1 1 0

1 0 0 0 1 1 0 1 1 0 1 1 0 1 0 1 1 1 1 1

Cin = i 1 input carry, sometimes designated as CI  1  Yout nutaput carry, sometimesdesignatedas o A and B = input variables(operands)

Σ=(A B) Cin A B t=AB+(AB) n (a) Logic required to form the sum of three bits (b) Complete logic circuit for a full-adder (each half-adder is enclosed by a shaded area)

Halfadder Half-adder M A - A A A St (A B) Cn B.- B Cout B Cout M mp t (A B)Cin Cainry, Ci A M AB ± (A ① B)Cin (a) Arrangement of two half-adders to form a full-adder (b)Full-adder logic symbol

M M M A A 1 A M  B I B O B Cout Cout Cout 0 Cin 0· Cin 1· Cin (a) (b) (c)

A2 B2 A1 B1 General fornat,addition of two2-bitnumbers: A2A1 O +B2B A B A B ΣgΣ2∑1 in in FA2 FA1 Cout M Cout M (MSB) Σ3 2 Σ1 (LSB)

A B Cin A B Cin A B Cin  FA3 FA2 A1 Cout  M Cout M Cout M ∑4 Σ3 ∑2 ∑1 1 0 0 0

TABLE 6-3 Truth table for each 1 stage of    a 4-bit parallel  adder.

Cn —1 An Bn ∑n Cn

0 0 0 0 0 0 0 1 1 0 0 1 0 1 0 0 1 1 0 1

— —  — 1 0 0 1 0 1 0 1 0 1 1 1 0 0 1

1 1 1 1 1

A4B1 A3B3 A2B2 A₁B AB _ A B C A B Cn A B (M1SB) FA4 FA3 FA2 FA1 (LSB) Cour ∑ Cout 三 Cou 三 Coun ∑ (a)Block diagram  M ( Binary A  4-bit numherA sum Binary 1 B number Input_ camy Co C4 Output cary (b)Logic symbol

VOC (1 ) (5) ∑2 1 16 VCC 1 (3) 3 2 B2 2 15 B3 (14) }A (4) 1 A2 3 14 A3 (12) 4-   (1) ∑1 4 13 Σ3 (6) ∑ (13) 1 A1 5 12 4 A4 (2) ² (10) (15) B  B1 6 11 B4 3 (11) 4  C0  10  ∑4 (7) C0 (9) GND 8 9 C4 C4 GND (a) Pin diagram (b) Logic symbol

Ei ts IpLH Propagation delay,C0input to 16 24 tpHL any ∑ output 15 24 ns pLH Propagation delay,anyAor B input 15 24 tpHL   to ∑ outputs 15 24 ns pLh Propagation delay,C0input to 11 17 [pHL C4output 11 22 ns [puh Propagation delay,any Aor B input 11 17 ns pHL toC4output i2 17

Bg B7 B6B5 AgA7 A6A5 B4 BzB2B1 A4 Az A2A1 4 3 2 1 4 3 2 1 Cin  4 3 2 1 4 3 2 1 Cin    B A B A —三_— Cout 4 3 2 1 Cout 4 3 2 1

(5) (5) A1 1 A2 - (3) As1 1 2 (3) A3O- (14) A6 1 A A7 · (14) A A4 (12) 1 (4) Σ₁ A1 (12) 4 (4) 1 Σ5 2 (1) (1) Σ2 Σ 3 (13) Σ ∑6 (13) B1 - (6) Σ3 1 (10) B51 1 Σ7 B2 (2) 2 4 Σ4 (2) 2 4 (10) B6 - Σ B3 1 (15) 3 B B70· (15) 3 B (11 B4 ( 1 4 _ B 4 _ (7) (9) (7) (9) O C0 C4 Co 1 ∑g C 4

VCC ≥ Six-Position AdderModule 1.0kΩ M M YESO— —O A 1 0 B A YE NO Cou 0 Cn 4 1 BD YESo Σ 2 to Fulader1 3 7-segment 1 NOo Σ decoder 4 A B YESO— 4 Cou C0 C4 NOO— —O— Cn 三 Parallel adder1 YESO Full-adder2 O YES1ogic NOo— M YESO— M O A O B NOo— NO Cout A O Cin YES O 4 1 BCD  2 to Full-adder 3 Σ  3 -O 0 3 7-segment decoder 1  NOo 4 M 2 Switches -0 A 3 B 4_ O— B Cout C4 330 esistor (t ical i C0 三 Parallel adder 2 Full-adder 4 NOlogic 100k2resistorsshouldbeconnectedfromtheinputsofthe cmosfull-addersto ground.

A B A B A A B un n M ''out Cout Mout   M out M

FA4 FA3 FA2 FA1 -8ns -8ns -8ns 8ns 32ns

1 1 0 1 1 1 0 1 0 1 A B A B Cin A B C A B in  in in Cout M C out ''out M C M M C out    Generated Propagated carry/ Propagated Propagated cary Generatedcarry carry carry

A4 B4 A3 B3 A2 B2 A1 B1 Cim Cin3 Cim2 Cini A B C Cin A  B Cin A B Cin A B     Cin  FA4 FA3 FA2 FA1 Cout M Cout M Cout M Cout M Cout3 Cott2 Cout1 Full-adder4 Full-adder3 Full-adder2 Full-adder1 Cg4 = A4B4 Cg3= A3B3 Cg2 = A2B2 Cg1 = A1B1 Cp4 = A4 + B4 Cp3 = A3 + B3 Cp2 = A2 + B2 Cp1 = A1 + B1

A4 B4 A3 B3 A2 B2 A1 B1 Cg4 C3 Cg2 Cg1 p p p2 p A B A B A B A B Cou2 or3 COA Σ4(MSB) Z3 ∑2 Σ₁(LSB)

COMP 0. 0 证 ∑A 1 A>B 0- 3 A=B 1 0 1 A<B B 0  3

COMP A0· 0 A A2- A>B A3- 3  A=B Bo- 0 B1 A<B B B2

B3 1 16 VCC (10) COMP 0 (12) ABi 2 15 A3 (13) }A A=Bin 3 14 B2 (15) 3) A >Bin 4 13 A2 (4) (5) A > B (3) A> B (6) A >Bout| 5 12 A1 as i  Us (2) A = B A = B (7) t S  A=Bout| A < B 6 11 B1 (9) A < B 0  10  A < Bout A0 (11) GND 8 9 B0 (14) B (1) 3 Vcc(16),GND(8) (a) Pin diagram (b) Logic symbol

LSB MSBS COMP 0 COMP 0 ∑A A 3_ A B A>B A>B A>B +5 V· A = B A = B A = B A = B Outpuits A<B A<B A<B A<B >B >B 3  74HC85 74HC85

LSB) Ao -1 -X=A₃A2A1A0 O A2· A2 1 A3 (MSB) (a) (b)

TABLE6-4 Decoding functions and truth table fora 4-line-to-16-line (1-of-16) decoder with active-LOW outputs. Digit A3 A2 A1 A0 Function 0 1 2 3 4 5 6 7 8 9 10 11 12 13  14 15 0 0 0 0 0 AzA2A1A0 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 0 0 1 AzA2A1A0 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 2 0 0 1 0 AzA2A1A0 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 1 3 0 0 1 1 AzA2A1A0 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 1 4 0 1 0 0 AzA2A1A0 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1 1 5 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 1

J 0 1 0 1 AzA2A1A0 1 1 1 U 1 1 1 1 1 1 1 1 1 1 6 0 1 1 0 AzA2A1A0 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 1 7 0 1 1 1 A3A2A1A0 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 1 8 1 0 0 0 A3A2A1A0 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 1 9 1 0 0 1 A3A2A1A0 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 1 10 1 0 1 0 AzA2A1A0 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 1 11 1 0 1 1 A3A2A1A0 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 1 12 1 1 0 0 A3A2A1A0 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 1 13 1 1 0 1 AzA2A1A0 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 1 14 1 1 1 0 A3A2A1A0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0 1 15 1 1 1 1 AzA2A1A0 1 1 1 1 1 1 1 1 1 1 1 1 1 1 1 0

B NDEC 0 6 6 b- b 9 0 10   11 0 12 13   14 p 15 b

OUTO    Vc BIN/DEC UTO b 1) OUTI   0 OUT2   OUT1 b ② A1 OUT2 b ③ OUT3   A2 (4) OUT3 O b- OUT4   A3 A  OUT4 b OUT5 6 回 CS2 (6) OUT5 b OUT6   CS1  (23) 7 OUT6  OUT7   OUT15  (22)   (8) OUT7 OUT8   OUT14  (21)   (9) OUT8 OUT9   OUT13  (20) 8 (10) OUT9  OUT10 回 4 0UT12 (11) OUT10  GND 2 13 OUT11  (13) OUT11 (14) OUT12 6 (15) OUT13 63 (16) OUT144 (18) (17) d 0UT15 b (a)Pin diagram (b) Logic symbol

BIN/DEC BIN/DEC Low-order High-Orden OUT0 b 0UT0 16 OUT1 17 0  0UT1  OUT2 0UT2 18  0UT3 0  0UT3  0 OUT4 6 4 0UT4 O 20 OUT5  0UT5 21   1 OUT6  6  0UT6 2 0UT7    2 · OUT7 23  4 OUT8  OUT8 2 OUT10 O -10 0UT10 26 OUT11 b -1 0UT11.  OUT12 -12 0 0UT12 283 0UT13 -18 29  0UT13 0UT15 O- 1 0UT15 74HC154 74HC154

Input/Output ports 8 Databus O processor C EN Keyboard 1/0 C EN 1/O Modem I/O A0 A1 1 Q EN IO port A2 2 address 8 These data Scanner A3 4 lines are either I0 8 10 11 unused or EN 0  i2 - connect to P- otherI/O Ext.disk ports. VO &  1/0  EN 1 1

BCD/DEC (1))  OUTO  (2) OUT1 (3) OUT2  (15) 1 (4) OUT3   (14)(  5) 2 OUT4 (13) 4 (6) 0UT5 (12) 7)     8 0UT6  (9) OUT7 O (10)  OUT8 (11)) OUT9 74HC42

t0 t1 t2 t3 t4 t5 t6 t7 t8 to t10 AO A1 BCD I impurts I A2 1 1 1 (a) A3  I 1 0 1 1 2 3 1 1 1 4 1 1 1 outputs 5 1 1 6 7 8 (b) 9

Ao 1 Outpuit lines BCD A1 2 connect to imput A2· 4 7-segment A e display device 8 f 8

B 1 16 Vcc  BCD/7-seg C 2 15 |f (4) BI/RBO B/RBO

RBI LT 8 4 2 1 RBI LT 8 4 2 1 RBI LT 8 4 2 1 RBI LT 8 4 2 1 BCD-to-7-segment BCD-to-7-segment BCD-to-7-segment BCD-to-7-segment g f e d c b a BI/RBO 8 f e d c b a BI/RBo g f e d c b a BI/RBo g fe d c i b a Bl/RbO amaan ee lan e (a) Illustration of leading zero suppression 0101 0 11 001010 00010 RBI LT 8 4 2 1 RBI LT 8 4 2 1 RBI LT 8 4 2 1 RBI LT 8 4 2 1 BCD-to-7-segment BCD-to-7-segment BCD-to-7-segment BCD-to-7-segment decoder/driver decoder/driver decoder/driver decoder/driver g f e d c b a Bl/RBo g f e d c b a Bl/RBO g f e d c b a Bl/RBo 8 f e d c b a Bl/RBO dp Blanked Blanked (b)Illustration of trailing zero suppression

0 1 2 3 1 Decinnal 4 2 BCD input 5 4 output 6 8 7 8 9

TABLE 6-6 Decimal Digit A3 A2 A1 A0 0 0 0 0 0

1 0 0 0 1 2 0 0 1 0 3 0 0 1 1 4 0 1 0 0 5 0 1 0 1 6 0 1 1 0 7 0 1 1 1

7 0 1 1 1 8 1 0 0 0 9 1 0 0 1

Ao (LSB) 2. 31 A -A 8 A₃ (MSB) 0

VOC (16) D4 1 16 Vcc (11) HPRI/BCD  D1 D5 (12) 2 15 NC Q D2 3( D6 3 D3 4 A3 9 1 D4 D7 4 D3 ② 2 0 D5 4 D8 5 2 D2 ③ D6 8 4 2 6 D1 D7 D8 A1 7 10 D9 (10) D9 GND 8 9 A0 (8) GND (a) Pin diagram (b) Logic diagram

Voc (16) HPRI/BIN (5) EI (15) EO (10) (14) GS 0 (11) (9)= (12) (7) (13) (6) (1) (2) (3) 6 (4) (8) GND

01234567 89101112131415 0  2 3 4 5 6 7 E 012 34 5 67 EI 74LS148 74LS148 EO 1 4 GS E0 2 4 GS A0 A1 A2 A3

+V R{ W1 R8 WR R9 HPRVBCD R4 R5 WR R6 BCDcomplement 8 6 9 M R1 W R2 WR R3 Priorityencoder 3 AllBCD complement lines are HIGHindicatinga 0.  No encoding is necessary;however,this line maybe 0 connected to other circuits that detect the key press.
