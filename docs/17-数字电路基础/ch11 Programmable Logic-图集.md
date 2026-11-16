# ch11 Programmable Logic-图集

> 主题库：数字电路基础　｜　源文件：ch11 Programmable Logic-图集.pdf　｜　7,894 字符　｜　44 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTER OUTLINE 11-1 Programmable Logic:SPLDs and CPLDs 11-2 AlteraCPLDs 11-3 XilinxCPLDs 11-4 Macrocells 11-5 ProgrammableLogic:FPGAs 11-6 AlteraFPGAs 11-7 XilinxFPGAs 11-8 ProgrammableLogicSoftware 11-9 BoundaryScanLogic 11-10 Troubleshooting System Application Activity

CPLD  Bed-of-nails  LAB  Flying probe 4 FPGA  Primitive  CLB A G 3 A   Fittertool Intellectual property  Functional simulation Designflow Timingsimulation Targetdevice

A A B B

A A B B X=ABAB+AB

A A B B +V-  +V - +V-  +V-

Input Input lines bufier A A B B ixed connection B- Single line with slash represents multiple AND gate inputs.(In this case,2 inputs) AB Product AB    2∠ X=AB±AB+AB Fuise blown Fuseintact noyconnection) (connection

A A BB C  B- C. ABC 3 X=ABC+ABC+AB+AC AC 3

Or array 2 0R Output 1 gate 10gic 1 1 1 OR Output 2 1 1 gate 1gic 1 Programmable 1 ANDarray PAL: One-time OR Output programmable 3 gate logic 1 一一 - 一一一一一 OR 0utput

From AND Tristate control Inpuit/Output (I/O) gateanray FrOmAND gate aiay (a) Combinational output (active-LOW). An active-HIGH (b)Combinational input/output(active-LOW) gate symbol.

FronAID Input/Output (I/O) gateairay (c) Programmable polarity output

Logic array Logic array  block(LAB) block(LAB) /O SPLD SPLD Logic array Logic array I/O block (LAB) block (LAB) ≥ ·I/O SPLD SPLD PIA /O block(LAB) block(LAB) IO SPLD SPLD 1 1 1 Logic array Logic array I/ block(LAB) block(LAB)  I/O SPLD SPLD

[aa Manufacturer Series Name Design Software Website Altera MAX QuartusI Altera.com Xilinx Coolrunner ISE DesignSuite Xilinx.com Lattice ispMACH ispLEVER classic Latticesemi.com Atmel ATF ProChipDesigner Atmel.com

cYaaaa Numberofmacrocells 10-1700 NumberofLABs 10-221 Maximum operating frequency 20.4 M Hz-4OO M Hz NumberofIOs 10-1156 DC operating voltage 1.8 V, 2.5 V, 3.3 V, 5 V

General-purposeinputs 8-16 I/0 I/0 Logic array block Logic array block I/0 pins/LAB Control LABA) (LABB) control  block block Macrocell1 Macrocell1 8-16 Macrocell2 36 36 Macrocell 2 8-16 16 16 Macrocell 16 Macrocell16 8-16 8-16 PIA Io Logicarrayblock Logicarrayblock I/o control (LABC) ( ABD) Control block block Macrocell1 Macrocell1 8-16 Macrocell 2 36 36 Macrocell 2 8-16 16 16 Macrocell 16 Macrocell 16 8-16 8-16

narallelexpanders froim other macrocells Product-term ToI/o Associated selection logic control matrix block Sharedl expandler 36 lines from PIA 15 expander productteims fromyother macrocells

盒三赏☰ ABC(E+F)=ABCE+ABCF ABCD 画 Product temm firom anotihem nmiacroceuin saimeLAB (a) A 4-input AN D array gate can produce (b) AND gate is expanded to produce two product terms. one 4-variable product term.

Macrocell1 **** ABCD+ABCD+ABCD **** +ABCD+ABCE+ABCF *米 Product-term Expanderterms selection matrix macrocell2 **** ***来 ABCD+ABCD+ABCD *米* 米 Product-term selection matrix 222222 √ Expander termE+F toMacrocell1 A B C D E F

ABCD + ABCD + EFGH — Parallel expander terms ABCDEFGHABCD+ABCD+EFGH

Macrocell1 **** **** *—ABCD+ABCD+ABCD 一 *   * 米米 Parallelexpanderterms loaned toMacrocell2 Product-term selection matrix Macrocell 2 ABCD+ABCD+ABCD +ABCD+ABCD+ ABCD+ABCD+ABCD **** —米 * 米 * Product-term selection matrix A B C D E F

IOE IOE IOE LAB LAB LAB Logic element Logic element Logic element Logic element Logic element Logic element IOE Logic element Logic element Logic element AB AB AB Logic element Logic element Logic element IOE Logic element Logic element Logic element Logic element Logic element Logic element

= AAA 1 0 0  SOP SOP LUT ouitput output 1 An1 一 1 1 (a) Look-up table logic. A 1 is stored at (b) AND/OR array logic

LABs cY ca ca (a)Row/column interconnects (b)Channel-type interconnect

** ** *    $AB+AB+AB+AB ** ** * * AAB B AABB caa AB+AB AB+AB AB+AB ABAB (b) PLA-type array

A i D i D āD ABCD Product-term Sum -ter 222 array array 16 16 macrocells A B C D ABCD+ABCD+ABCD+ABCD ABCD+ABCD+ABCD

Data imputs Data output Select (O selects Do, 1 selects D1)

Global Global Parailelexoanders clear clock from othen maorocells Froml MUX5 IYO ToI/O MUX1 PRE D/T Product- term C selection MUX2 matrix EN CLR

Vcc MUX3 MUX4 36lines 15 expander product from PIA terms from other inaicrocells

GlobalGlobal Parallelexpandens clear clock fom other fromi MUX5 IO 0 To VO MUX1 PRE D/T product- term c selection MUX2 matrix EN CLR MUX3 VCC Shared expander MUX4 36lines 15 expander product from PIA terms from other macrocells

GlobalGlobal Parallelexpandens clear clock fiomoiher macinocells Fromi MUX5 VO 0 To V/O PRE MUX1 D/T product- term selection MUX2 matrix EN CLR Vcc- MUX3 Shared expandler MUX4 36lines 15expander product from PIA termsfrom other macnocells

Piograiuneble 1O O O IO block block block 1O IO block block CLB 1  CLB CLB CLB IVO IO block block CLB   CLB   CLB CLB IO O block block 1 1 1 1 1 CLB   CLB CLB CLB 1  I/O 1O block block FPGA IO IO IO I/O block block block block

CLB CLB Logicmodule Logicmodule Logicmodule Logicmodule Logicmodule Logic module Local Local interconnect interconnect  Logicmodule Logicmodule 个个 Global column Global row interconnect interconnect

SOPoutput Ao A2· Associated LUT I/O logic 一

Selection logic Memory cells A2A1A0 A2A1A0 A2A1A0 0 AO A2A1AO A- SOPoutpu A2A1A0 A2A1Ao O A2A1A0 LUT

4-input 6-input LUT LUT 4-input 2-input LUT LUT 5-input 5-input LUT LUT 4-input 5-input LUT LUT 中

5-input Lut 5-input Lut A6 AgA=A1A3A2+AgAAA3A2+AA5A4A3A2

CLB ReprogramsCLBson Nonvolatile Programming power up or reset ata onfiguration memory (a) Volatile FPGA with on-the-chip nonvolatile configuration memory Host processor Nonvolatile Programming Volatile configuration data FPGA memory (b) Volatile FPGAwith on-board memoryand host processor

RemainingCLBs areprogrammed byuser cYaa portionofCLBs programmed during manufacturing for a m ecific function

Embedded Embedded Embedded I/O memory DsP memory elements 1/Oelements blocks blocks blocks IOEs IOEs IOEs IOEs IOEs LABs LABs LABs LABs LABS LABs IOEs LABs LABs LABs LABS LABs LABS IOEs LABS LABs LABs LABS LABs LABs IOEs LABs LABs LABs LABs LABs LABs IOEs LABS LABs LABS LABS LABs LABS IOEs LABs LABs LABs LABs IOEs LABs LABS LABs LABS IOEs LABs LABs LABS LABS IOEs LABs LABs LABs LABS [e IOEs LABs LABs LABs LABs memory IOEs LABs LABs LABs LABs block IOEs LABS LABs LABs LABS IOEs LABs LABs LABs LABs IOEs LABs LABs LABS LABS IOEs LABs LABs LABs LABs LABs LABs IOEs LABs LABs LABS LABs LABs LABS IOEs LABs LABS LABs LABS LABS LABS

SelectedFPGAparameters. aeature Range NumberofLEs 1,500–813,000 Number ofCLBs 26-359,000 Embedded memory 26kb–63Mb NumberofI/Os 18-1200 DCoperating voltage 1.8 V,2.5 V, 3.3 V, 5 V

Manufacturer Series Name(s) Design Software Website Altera Stratix Quartus I Altera.com Aria Cyclone Xilinx Spartan ISEDesignSuite Xilinx.com Artix Kintex Virtex Lattice iCE4O LatticeDiamond Latticesemi.com MachX02 iCEcube2 [a LatticeXP2 Atmel AT40 IDS Atmel.com

Design entry □ Schematic Timing Functional simulation simulation

Implementation Device programming (downloading)

FPGA CPLD and ASIC from Alera- Windows Internet Explorer A Hito//o altera.com Googie File Edit Viewe Favorites Tools Help  Favotite A FPGA CPLD and ASCfrom Alters AERA. Deeno Cenf r Eberabare m ti ie Prodocts End Marets Technolo Teaining Suspiet About Alters Euy Onine e Language Englist [ t Manage Multiple Protocols natne 国 with One Networking Kit Erle mm rem em Satems Get Detalls Buy Now Dala eat Poco Deeoe C ign Taos eemerg Mesnurem Oorronetn Liceenirg Learn Four Reasons FPGAs are Right for Motor Control Dn 李ese eee Ucone5opor Wateh the Vdeo Intenet|Prctected ModeOn f 80%  (a) Computer (b) Software (CD or Website download) IC4 OC.OK BASYS2 10 DataC DIGILENT 1CSL OOOo BEYONDTHEOR 日日日日 (c) Device (d) Programminghardware(programmingfixture or development board with cable for connection to computer port)
