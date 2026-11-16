# ch01 Introductory Concepts-图集

> 主题库：数字电路基础　｜　源文件：ch01 Introductory Concepts-图集.pdf　｜　7,732 字符　｜　56 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

1-1 DigitalandAnalogQuantities 1-2 Binary Digits, LogicLevels, and Digital Waveforms 1–3 BasicLogicOperations 1-4 Introduction to the System Concept 1-5 Fixed-Function Integrated Circuits 1-6 Testand Measurement Instruments /o 1-7 Introduction to Programmable Logic

KEYTERMS Key terms are in order of appearance in the chapter.  Digital  NOT Pulse d l 2 OR  Dutycycle   Integrated circuit (IC) Clock Fixed-function logic Timingdiagram Trubleshooting  Serial  SPLD  Logic  FPGA Input Compiler

Temperature 100 95 90 85 80 75 Tie fday 1 2 3 4 5 6 7 8 9 10 11 12 1 2 3 4 5 6 7 8 9 10 11 12 AM P.M

Temperature (F) 100 85 80 75 70 Tie fday 1 2 3 4 5 6 7 8 9 10 11 12 1 2 3 4 5 6 7 8 9 10 11 12 AM P.M

Original sound waves Microphone Reproduced Linearamplifier audiosignal SpEaker Amplified audio signal

CDdrive 10110O11101 Digital-to-analog Linear amplifier Digitaldata Cenverter Analog of music audio Speaker sigNal [a

Yaama HIGH (binary 1) YH(min) Unacceptable Yaama LOW (binary 0) VL(min)

Rising Or Falling or Falling or  Rising or  leading edge trailing edge leading edge trailing edge tO t1 to t1 (a) Positive-going pulse (b) Negative-going pulse

Overshoot Rimgimg Droop 90% Anmplitudle i 50% 10%   amn Ringing Base line  Undershoot  Rise time Fall time

Period = T 1 = T2 = T3 = ... = Tn (a) Periodic (square wave) (b) Nonperiodic

0 1 10 11

B tiue 1 Clock 0 一 represented by   !     1         T     !  1    1 WaveformA I I 1 I

Clock 2 3 4 5 6 7 8 A 一 1 1 一

Sending Receiving device device Sending t0 t1 t2 t3 1 t4 t5 t6 t7 Receiving device device (a) Serial transfer of 8 bits of binary data. Interval to to t is first. (b) Parallel transfer of 8 bits of binary data. The beginning time is to.

ClOCk 一一

NOT AND OR

E0G (1) · LOW (O) LOW (O) GI

EIC I (1) LO (0) HGA (① LO (0) IOGI (1) HOGT (1) H GH (1) LOW (0) LOW (0) LO (0) LO (O) LO (0)

HUCH (1) LOW (0) ACA (1) UGH (1) HIGH (1) LOW (O) IIGII (1)· LO (O) LO (O) LO (O)

A>B Bay A>B A A baim20ry A=B 0pts A=B. I N B B A<B eO A<B H (a) Basic magnitude comparator (b) Example: A is less than B(2< 5)as indicated by the HIGH output (A < B)

Bmary A A binary nunbets Cauyout B Cout Binary out Bmatyi code for 9 B Carryin Binaryo in Bimary eode fon 12 (a) Basic adder (b) Example: A plus B(3 +9= 12)

HOH Encoder codefon9 7 8 0 1

2 3 Caleulator eypad

Bimary-codedl input 7-segment display

ultiplexe De ltiplexer A s Data from Data from Data from Data from  O nnnn  A to D B toE CtoF A to D ∆t1 ∆t1 ∆t2 ∆t3 ∆1 ∆1 sequence sequence

Sierialbits Initially, the registercontains onlyinvalid 1 ] 0 0 0 0 dataorallzerosas shownhere. First bit(1) is shifted serially into the 1      0 0 0 register. Second bit(O)is shifted serially into  0    0 register andfirst bit is shifted right.       Third bit(1)isshiftedintoregisterand  0 thefirst and second bits are shifted right. Fourt bitOisshiftedinoregiserand  the first, second, and third bits are shifted right. The register now stores all four bits and isfull.

Panrailelbitsi oimr imput iimes! cYn a 0 0 0 0 containing only nondata zeros. caaa

Parallel output lines Biany Bin y Biay Biary Buaay Counter code code code code code 2 3 4 5 for1 for2 for3 for4 for5 Sequence of binary codesthat represent thenumberofinputpulsescounted.

Tablets/bottle 7 8 9 Encoder Decoder 4 5 6 Register A  1 2 3 A 0 # . Numberof Keypad for entering tablets per bottle Code of tablets per bottle converter  A Comp A A=B HIGHclosesvalve and advances Binary code for B conveyor.LOw actual number of keeps valve open. tablets in bottle New total Valve   Adder sum Sensorl Counter A M Register Onepulse B from sensor B Cout Decoder for each tablet B advances Conveyor counterby1. HIGHcausesnew control sum to be stored. Pulseresets countertozero MUX when next bottle is in place. Current total sum The binary code representing the number of tablets bottled each time ↑ RegisterB has reached the maximum accumulated count.  Sw tchingsequence controlinput DEMUX number of tablets bottled over time

Piastic Chip calse

(a) Dual in-line package (DIP) (b) Small-outline IC (SOIC)

(a) SSOP(153× 193 mils) (b) PLCC(350 × 350 mils) (c)LCC(350 × 350mils)

(d)LQFP (7 X 7 mm) (e)Laminate CSP bottom view (f)FBGA bottom view (3.5×3.5mm) (4×4 mm)

Pinl Noteh Pin 3 19 idlentitier 1 | 16 2 |15 4 11 18 3 [ ] 14 4i 13 5| 6i 8 14 7I 8 j9 9 13 (a) DIP or SSOP (b) PLCC orLCC

Tekfroni. 200MH2 Horiz 3 Ext Tri

Oscilloscope Acquisition circuits  Processor 1010011010 Vertical circuits ADC Memory 101o011010 Reconstruction can s lay 1Paoe circuits Trigger circuits Horizontal circuits

BASYS2 DIGILENT iCelOooo BEYOND   日日日日 Board unden test

Tektronix TDS2024CFiurRAsANRAsE osciLLoscopE FOuRCHANNEL 2ooMHz AutoRange Save/Recall Measure Acquire Help T gge Cotrels 2GS/s Tek 0D M Pos:268.0ns CURSOR Ref Tirm Fre Set To Trig Set to 1 Source 50% Menu Zero CH1  Vertical PosiTioN △t 604,0ns $1.656MH2 Position △V144mV Mertieal comtiols Cursor 1 -4.00ns 138rV Math Menu 3 Horiz Cursor2 Scale 600ns -8.00 mV CH1200 V M100ns CH1 91. V Scale CH32.00A 4-Aug-1015:16 1.87497MHz  300VCATI1 300V C 20nc1i0p 8 ExtTrig 1 2 3 4 USB  FlashDrive

(a) Untriggered waveform display (b) Triggered waveform display

0

Ch1 1V 10us

Tektronix TLA6404 LogicAnalyzer TLA[off-line]-[Waveform 1] - 可 × Waveform -80ns -70ns -60ns s0ns .40ns .3cns 20ns -1 ns 0pis 10ns 20ns 30ns 40ns 50ns. 80ns 70ns 80ns C2 La1.Sample 80.00 ns 11 80.00 ns 1 1 1 11 1111 1111 1 1111 1 La1:Burst Mark 82.750 ns LA 1: Magnivur Sample -82.500 ns LAT:MagnVu Burst   82.771 ns TDS72548:Sarple -82.589 ns 5.1v iView Magnivu Max :5.035 V Navigation- TDS7254B:Burst in : -0.219 V  . v System Setup Activity  La 1:Stub(0) A 1:MagniVrStubU) Trigger Listing Waveform 2V TDS72548:Stub 入 N 入 LA1 Xtak(1) LA 1:Xtalk(O) Vertical Horizontal LA 1: MagniVurXtalk(1] Position Position LA 1: MagniVu Xtaik(0) 3.2v TDS7254B:Xtalk1 High : 2.387 V  LOW :1.647V TDs7254B:Xtalk2 Peak-te-Peak 1-252~V Size Scale 1.2v LA1:CK00 LA1:FF_D LA1:FF_Q

LA1:Count_L LA1 CountO L Touch Screen LA 1: Count2 L of Ctr Shift La 1.Count3_L LAT:Count4_L 全合 (MarkD) 合合 For Help,press F1 回 Tektronix

Charmmel and Acquisition and inputs sampling memory display

Clock Trigger logic circuits and memory

LlLlLl LL Sample Bimary Hex Time 1 F ns 1 10 E 10ns 1101 D 20ns 4 1100 C 30ns 5 1011 B 40ns 6 1010 A 50ns 7 1001 9 60ns 8 1000 8 70ns

1070 0

Tektronix AWG 5012CArbitrary Waveform Generater 1.2 GSs S DVD Tektronix AFG2021 u  Continuous Tk 4 M Modatin SWeep Fr 1.00000000000mt T 8 9 Cane Run Sore Iten Aani 1.000v FMFreg O ke FMSource Modutstion 5 6 BKS Otet nv Sue Squre Dviation 10000Me FM 2 3 Freqoincy Burst Channel Trigger 50s Men Onon 0 nle Manual On h C Da g Us Output Output Input 0n h Devdato Sa0 s 1 2 ca Arbitrar waveformgenerae b uneio generae

FLLJKE 116 TRUE RMS MULTIMETER 11.6 c C/F HOLD MINMAX RANGE AUTO-V OFF Hz Loz mV= COM CAT 600V (b)Handheld DMM

BK PRECISON 54925V2DIGITMULTIMETER I I 8.88日 GVm C T N VANF 2DAUTO CC AC COPCGCA I knH EXTTH FOCTE LCCALSTO L 12A FUSL0 154 W RaD SAT9 aeeM

Tektronix PWs2323 DC Power Supply 0-32v, 3A cv eter Power Enter   V-Set I-Set 4 5 6  0    Output Menu On/Off 7 8  9  Save Recall

Lamof=Low  One flash = single pulse Logicprobe Repetitive flashes = pulses FIGURE1-49 Illustration of how a typical logic pulser and a logic probe can be used to apply a pulse to a given point and check for resulting pulse activity at another part of the circuit. Indications may vary depending on manufacturer.

Programmable logic PLDs FPGAs

SPLDs CPLDs

Programmable array and Reprogrammable array and AND array AND array programmable (a)PAL (b)GAL
