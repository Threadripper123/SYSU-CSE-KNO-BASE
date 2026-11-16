# ch12 Signal Interfacing and Processing-图集

> 主题库：数字电路基础　｜　源文件：ch12 Signal Interfacing and Processing-图集.pdf　｜　6,215 字符　｜　47 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

12-1 Converting Analog Signals to Digital - 12-2 Analog-to-Digital Conversion Methods 12-3 Digital-to-AnalogConversion Methods 12-4 Digital Signal ProcessingBasics 12-5 TheDigital Signal Processor(DSP)

KEYTERMS  Sampling  DSP core 4 Nyquist frequency Mlps  Aliasing  MFL 0 PS  Quantization  MMACS  Analog-to-digital  Pipeline converter(ADC)  Fetch  Digital-to-analog  Execute

10110 10110 M 01101 01101 i11o0 1iioo Analog Enhanced Anti-aliasing Sample-and- Reconstruction ADC DSP DAC Sigial filter filter analog holdcircuit

cYi Output of the D AC Final analog output

input Sampling signal circuit cYam nmnnl Sampledl versionyof input signal

(a) One sample of a ball during a (b) Two samples of a ball during a single (c) Four samples of a ball during a single singlebounce bounce. Thisis the absolute minimum bounce form a rough picture of the path Yauie oelanhinabo oftheball.

frequency spectrum Sampling frequency

frequency spectrum Sampling frequency FSAMPI

input signal Sample-and-hold Sample-and-hold approximation ofinput signal

ADC 01000101 110010101

Quantization level (Code) 4 3 1 1 (11) 1 1 一 1 1 2 1  1 (10) 1 1 1 1 (01) 1 1 1 I 1 Sample 2  3 1 4 intervals  5  8 1    9 |10!11!12|13

S ample Cae 0 0 0 1 1 0 0 01 1 1 01 0 6 1 1 7 1 1 1 8 0 9 1 0 10 i 12 11 13 11

11 10 一 O1 一一一一一一一一一一一一一一一 OO Sample 1 2 3 1 4 1 5 1 1 6 1 7 8 9 10 1 11 12 1 13 1 intervals

ampee 1 0 0 5 10 0 0 7 10 0 40 0 6 0110 10 1010 14 1110 10 15 1111 11 15 1111 12 15 1111 13 14 1110

Quantization lel(Cde) 4 1 13(1101) 12(1100) 1(1011) 10(1010) 9(1001) 8(1000) 7(0111) 6(0110) 5(0101) 4 (0100) 3(0011) 2(0010) 1(0001) 1 2 1 3 1 4 1 5 1 6 1 7 1 8 19110i11112113 1 Sample intervals

valuEs 1111 一 1110 — 1101   1100  1010  1001 1000 二二 0111 0101  0100 0010 ————十—————————十————————— 十————— 000 0000  1 |  Sample 1 2 3  4  5 9 10 | 11 121 intervals  6 8 13   1

RF Inverting input  VoU VirtUAl O V grOLIRL (O V) Represents the high Naninveruinginput internalinputimpedanee (a) Op-amp symbol (b) Op-amp as an inverting amplifier (c) Op-amp as a comparator

+VreF R p-anp comparators Input fromi O sample- and-hold R Do Pall -D1 bimary -D) output R R Enable R R

V 8 6 voltage 3  1 pulses 1  2 3 4 5 6 7 8 9 10 11 12

2 3 4 5 6 7 8 0 10 11 12   Enablepulses           D1 1 1 1 1 Do I 1 I 1 10011101111I11011001010100010011011110111101111

Analog input (Vin) C CLK O1SW R 十 R Counter —MRE Integrator n (ramp generator) Comparator CLEAR Switch control Control logic EN Latches D7 D6 D5 D4 D3 D2 D1 D0 Binaryor BCD ouitput

in CLIK C 0 SW O WW O R =OV A1   + A2 I GH L _ L C Counts up tonand  + R VkEF then resets Fixed interval n t= n counts 0 Variable Variable voltage slope -V 1 control Latches logic EN (a)Fixed-interval,negative-goingramp (whilethe counter countsupton) DD6D5DD3D2 D0

Ci A2 +  + R Counter Vrc   reset n Control Latches logic EN (b) End of fixed-interval when the counter sends a pulse to control logic to switch SW to the —VREF input D D 5 D DD2 Do

+ OLK C S + A2 Counts upuntil o + R r2 rampequals zero Variabletime n 0 -V Fixed-slope ramp count loaded control logic intolatches EN (c Fixedslope, positive-goingramp whilethecountercountsupagain. Whentheramp reaches OV, the counter stops,and the counter output is Ioaded into latches. DDD5 D 2 Do

V Digital-to-analog out converter (DAC) Di Parallel binary D2 output Input D3 + signal (MSB) (LSB) Comparator D Serial SAR binary output CLK

+8V DAC 千4V DAC 2³ 22 21 20 2³ 2²  21 20 1 0 0 0 0 1 0 0 +5.1 V +5.1V LOW D  2 21 20 HIGH D 23  21 20 C SAR SAR 0 0 0 C 0 0 0 RESEt KEEp (a) MSBtrial b 2-bitrial 46V DAC +5V DAC 23 22 21 20 2³ 22 21 20 0 1 1 0  1 0 1 +5.1V +5.1V LOW D 23 22 ] 20 SAR GH D 23 22 21 : SAR C 0 1  0 C 0 1 0  Reset Keep (c) 21-bit trial (d) LSB trial(conversion complete)

VOC (20) cs· (1) (5) (2)  AD C0804 INTR  (19) RD (3) (18) CLK R (out) WR (4) ∇ (17) - D0 CL K IN (6) (16) - D1 Analog Vin+- (7) ∇ (15) - D2 input Vin− (9) ∇ Digital (14) -D3 data REF/2 ∇ (13) -D4 Output D6 (1 1) − D7 − (8) (10) ANLG DGTL GND GND

+MAX Input signal and-hold 0 -MAX Quannized cYaal sigma-delta 20481s 40961s 01s

Analog + ∆ 1-bit n-bit Binary code input Σ Integrator Latch quantizer  counter output ignal A

SAVE/RECALL EASTRE ACOUTRE UTOSET AAOg LO U UT MY CRSOR DSHLAY HARDCOP NSTOP E NERICAI ADC COde HORZONM RIGR DAC ON ION O 0 EOR R MO 1 NE MORON M UTO RamP naOgOIOt 2 VOLTSDIV VOLISDIV SECDIV  FORCETRIGGER PROBECOMP CH1 CH2 EXTTRG 5V

(a Missingcode(gre (bIncor codes(gre (cOffs

VREF W AnALOg   2 1 (a) (b)

oX 8R Vott = i pR 0= = 20 W Rf 4R Io 21 c W I 1 2_ 2R 1 2= 22 0 3X_ R I2 3== 23 W

200KΩ D0 Rf 100kΩ - 0 1 2 3 4 5 6 7 8 9 101112131415 +5V 10kΩ D0 0 +5V 50 kΩ  D1 0 D2 M O OI t +5 V 25kΩ + D2 0 D3 -- +5V D3 0 (a) (b)

5V I0 =0.025mA 200kΩ 1 5V 100kΩ =0.05mA I2= 5V = 0.1mA 50kΩ 13: 5V 25kΩ = 0.2 mA

0 Binary input -0.25 -0.50 -0.75 -1.00 -1.25 -1.50 -1.75 -2.00 -2.25 -2.50 -3.00 -3.25 -3.50 Vout(V)

O D0 Q D1 D2 O D3 R1 2 Rf= 2R 2 2R 2R 2R R R R Op-ap V ut

R Bpinelntleder resistano wthD 0V VmE -RE ( 2 2R==5V Reo=2R D andDogounded (a)EquivalentcircuitforD3=1,D =O,D1=0,D0=0

D 2R 2R R 25 R R R0=2R 2 0 E0 25√> 三 ②R==25√ Do=0 D₃=101 2R- (b)EquivalentcircutforD3=0,D2=1,D1=0,D00

D₂=1 D=1 +5 V 2.5V RTH= R REQ 2R Mh REQ=2R

2R 2R R R R R 2 R V 1.25 28☰- SX 6=0 D =0 D =0 2R (c)EquivalentcircuitforD3=0,D2=0,D1=1,D0=0

2 D=0 2=0 D=0 (0625√ 2R==0625 2R (d)EquivalentcircuitforD3= 0,D2= 0,D1=0, D0=1

SAVE/RECAEL MEASURE ACQUIRE AUTOSET UTELITY CURSOR DISPLAY HARDCOPY RENSTOP  - VERTICAI HORIZONAL TTRIGGERI  。 POSITION POSITION LEVEL CC DA C    POSITION MATH MENU 0 CURSOR1 URSOR HOLDOF HORIzONAL Binary teSt 1 MENU A alOg tCU SETLEVELTO50% 2 VOLTS/DIV VOL.TS/DIV SEC/DIV SeguenCe FoRcETRIGGER SOUrCe TRIGGERVIEW 0 SV 2mV 5V 2mV 5s 5ns PROBECOMP 8 CH1 CH2 EXTTRG 0to2n-1 n 0

Binary 0 3 Binary C 0 input 0 0 0 input 2 C 0  (a) Nonmonotonic output (green) (b) Differential nonlinearity (green)

C 0 1 0 0 1 1: 0 :Binary 0 :Binary 1 0 0 C 0 0 1 m 0 0 0 0 0 0 0 0 0 input 0 0 0 0 0 C 0 J 0 0 C input (cHighandlowgains(green) (d)Offseterror(green)

c Ana g a outpu 0 Binary m 1
