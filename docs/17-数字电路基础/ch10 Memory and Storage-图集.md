# ch10 Memory and Storage-图集

> 主题库：数字电路基础　｜　源文件：ch10 Memory and Storage-图集.pdf　｜　8,065 字符　｜　60 个片段　｜　更新于 2026-11-12
FUNOAMENTALS 00 01 00 00 001 10 00 ELEVENTHEOITION 10 111 10 10 10 10 0 00 00 10 10 0 00 00 11 3Q0100101001 01001010101C013 191070 1100 01031001C070 FLOY□

CHAPTER OUTLINE 10-1 MemoryBasics 10-2 The Random-Access Memory(RAM) 10-3 TheRead-Only Memory(ROM) 10—4 ProgrammableROMs 10-5 TheFlashMemory 10-6 Memory Expansion 107 Special TypesofMemories 10–8 Magneticand Optical Storage 10-9 Troubleshooting

KEY TERMS Memory SRAM  Byte  DRAM  Word  Bus  Cell  PROM AddreSS EPROM Capacity Flash memory Write FIFO ↓ RAM  Harddisk ROM S1CTX

Caches Primary storage Hard disk Secondary storage Auxiliary storage Tertiary storage

6 一 1     1 2 34 56 7 8    61 1 (a) 8 × 8 array  62 1 2 3 4 (b) 16 × 4 array (c) 64 × 1 array

1 2 3 4 5 6 7 8 (b) The address of the blue bit (c) The address of the blue byte is row 5, column 4. is row 3.

8 7 6 5 4 3 2 1 45

(a) Physical structure of 64-bit memory.

R OWS Arrays 1 2 3 4 5 6 7 8 Columns (b) The address of the blue byte is row 5, column 8.

2 3 4 5 6 7 8 (a) The 8 X 8 bit array expanded to a 64 X 8 bit array. This array forms a memory module.

address decoder Address bus Memory array Data bus 1 Rcaddl Wate caaa Readl aite Row address decoder Address bus 1 Memory arrays Data bus 1 Ynlumnaddressdecoder

Address decoder Byte-organized memory array Addressbus 5 0 0 0 0 6 Data bus 7 (3) Write Address code101isplacedonthe addressbusand address5isselected. 3 Write command causes the data byte to be stored in address 5, replacing previous data.

Addressdecoder Byte-organizedmemoryarray 0 2 0 3 Address bus  3      1 0   Data bus     6 7 REaD Address code 011 is placed on the address bus and address 3 is selected. The contents of address 3 is placed on the data bus and shifted into data register. The contents ofaddress 3is not erased by the read operation.

Random- access Memory (RAM) Static Dynamic RAM RAM (SRAM) DRAM Synchronous FastPage Extended Burst Asynchronous SRAMwith Mode Data Out Synchronous EDO DRAM SRA DRA burstfeature DRAM DRAM (BEDO (ASRAM) (SDRAM) (SBSRAM) (FPMDRAM) (EDODRAM) DRAM)

VDD MA T3Q T4 T2 1位线个三字线 0位线

RovSdlect0 RowSelect1 Row Select-2 RowSelectm Data Input/Output DatcI/O Daa VOI DaalO DataVO! BiO Bir1 it2 Bit3

[CHIPSELECT  [OUTPUTENABLE]

Eight input tri—state  buffers—  Input | Column I/O Output Memory arrays | 1 data |  data 256 1Ii 1  256 rows × I/O 1 control Column decoder  28columns×

a Address tnes G 8bits G2 - - 128columns Eight output tri-state buffers (a) Memory array configuration (b)Memoryblock diagram

Address Valid address tAO tEO caa cYam G a aaData out Valid data (a) Read cycle(WEHIGH) tiwc= 1 Address Valid address -ts(A) tWD- —th(D)— I (Data in) Valid data (b) Write cycle (WE LOW )

control Burst logic o A CLK.  A0-A14 Address Address Memory array (external address) 15 register 15 13 decoder 32kx8 WData ouitput megrsieris in the pipelinod SRAM There jisno 8 mretgis-er in tne flow-tbrough WE Write Data input Data output Synchronous SRAM

DataIyO control Output Enable bufers register OE 1100-1107 (DataI/O) 8 8

aunstcomtroll Q1 Q0 CLK- Lowest-onderbits of internal burst address owest-order bits of externaladdress

Clock (CLK) Data bus Address bus Microprocessor Main memory (DRAM) Cache L2cache L1cache (internal)

Column (bit line) Row I I

Column Column buffer buffer Low Low Refresh. Refresh. HIGH G Row- Row Outputbuffer/ Outputbuffer/ Sense amplifier Sense amplifier DoUT- DoUT R/W- LOW R/W LOW OW DN- CH HIGH DN- LOW buffer buffer (a) Writing a 1 into the memory cell (b) Writing a 0 into the memory cell

Column Column 1 1 Refresh Refresh LO HICH ECH HIGH Row Row NON Sense amplifier Sense amplifier DoUT- HGH DOUT- HIGH R/W HGH R/W HIGH AIGH HGH DIN- DIN — Input Input Bit line Bit line (c) Reading a 1 from the memory cell (d) Refreshing a stored 1

refresh control and timing refreshcounter 1 1 1 Memory array Ao/A10 [Da A/A1 - selector decoder 1024 row × A2/A12- Addiess A3/A13 Row 1024 columns AA14- 1ines A5/A15· address A6/A16- A7/A17- register As/A18- Ag/A19 - 1024 2 1024 2 1 1 Input/Output buff ers Column Column and address decoder Senseamplifiers 1 register 1 1 - Dour DiN 1024 CAS RAS R/W

Addresses Row address Colunn address CAS Row address is latched Colunn address is latched when RASis LOW. when CASis LOW.

LIteadcycle Addresses Row adcress Columm addess RAS CAS RIW Dour Vaid data

1 wfite cycle A ddresses Row address Column address RAS CAS RIW Dn Valid data. (b) Write cycle

RAS CAS R/W Row Column 1 Column2 Column 3 Columnn Addresses address address address address address Dour Valid Valid Valid Valid data data data data

Read-Only (ROM) Electrically Mask Programmable Erasable Ultraviolet Erasable ROM ROM EPROM ROM PROM (PROM) (EPROM) (UVEPROM) (EEPROM)

Column Column 1 一 RoW -- RoW +VDD I +VDD Storingal StOringaO

+5V +5V Drin) Drin D D ON OFF Gater Gate +5V- (G) G Soint(S) Soine n-chanel p-hamel ON OFF (a)MOSFETsymbols (b)n-channelswitch

Cell storing ai Cell storing a 0 十 address decoder -Row0 1 -Row1 1 2 -Row2 Address 2- input 1ies 4- 1 1 1 一一一一一 8- 1 14 -Row14 15 ·Row15 Diataouitioutliines

TABLE 11-1 Banary Gray B3 B2 B1 B0 G3 G2 G1 G0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 1 0 0 1 0 0 0 1 1 0 0 1 1 0 0 1 0

0 1 0 0 0 1 1 0 0 1 0 1 0 1 1 1 0 1 1 0 0 1 0 1 0 1 1 1 0 1 0 0 1 0 0 0 1 1 0 0 1 0 0 1 1 1 0 1 1 0 1 0 1 1 1 1 1 0 1 1 1 1 1 0

0 0 0 0 1 1 1 1 1 0 1 1 0 1 1 1 1 1 0 1 0 0 1 1 1 1 1 1 0 0 0

address decoder 0 1 2 3 4 5  Bo 1 Bimanvyeode 6 epptediol adtes imput lines $B_- 8 9 10 11 12 13 14 15 ahay codleloutpat

Chip & EN select

row decoder 32 32×32 l eis Memoryarray row lines 1 1 1 1 1 1 = Cluirmn Column decoders (Four 1-of-8 decoders) addess and VO circuits Output buffers

Addnesstransition Address Previous Valid address on inputs address inputlines (A0-An) Data Valid datayon outputs (00-0) output lines Data output transition CS 1 (Chip seleet)

O Fusible Rows F E E

O 1 10 CEPCM EN

Program Ao-A10 Address n I n + 1 1 tTn(A) -1 1 OE 1 ts(A) its (E) th(D) CE/PGM k ts(Vpp) I, 1 VPP Oo-O7 Data to beprograimmedin

gate Drain Control transistor symbol Source Many electrons = more charge = stored 0. Few electrons = less charge = stored 1.

ERASE To erase a cell, a sufficient positive voltage is applied to the source with respect to the control gate to remove charge from the floating gate

+VD +VD Floating gate Control gate VpR0G To store a O, a sufficient positive voltage is To store a 1, no charge is added and the cell is applied to the controlgate with respect to the left in the erased condition. source toadd chargeto thefloatinggate during programming.

+VD +VD Yontroll gate HVREAD +VREAD  cYmantn l gate 0v 0w W hen a O is read, the transistor remains off W hen a 1 is read, the transistor turns on because because the charge on the floating gate prevents the absence of charge on the floating gate the read voltage from exceeding the turn-on allows the read voltage to exceed the turn-on threshold. threshold.

+V +V Activeload - 1 Dataout0 Dataoutm Comparator Bit line O Reference Bit line m Column select O Column select m

Comparison oftypes ofmemories. One-Transistor In-System Memory Type Nonvolatile High-Density Cell Writability Flash Yes Yes Yes Yes SRAM No No No Yes DRAM No Yes Yes Yes ROM Yes Yes Yes No EEPROM Yes No No Yes UVEPROM Yes Yes Yes No

65,536×8 AddreSs 16bits ROM Address 16 bits ROM 1 bus 65,536 × 4 bus Data 4 bits bus 4 bits Control Control bus bus 8bits Data bUS Address 16 bits ROM ROM 2 16 bits bus 65,536×4 Data 4 bits bus 4 bits Control bUS (a) Two separate65,536 ×4ROMs (b)One 65,536×8 ROM from two 65,536×4 ROMs

A ROM 64k×4 Address A 65,535  Data O 03 . 2 ou put

=  & Enable  EN

ROM1 Address bus >A 0 65,535 & R EN Data bans OM2 >A 0 65,535 Conrtrol . EN BUS

Ya Address bas 16 bits 16bits ROM 1 16 bits ROM 2 16bits ROM 3 16 bits ROM 4 64k ×4 64k ×4 64k ×4 64k ×4 4 bits 4 bits 4 bits 4 bits (enable) Deta 16 bits Bus

RAM2mx2n Adldress s mbits RAM1 RAM2 m bits 2m×n m bits 2m×n Data Data in/out n bits in/out n bits

Yba 2n bis IData bus,

Ao-  Address  bus i A1- 1 1 1 - >A- 0 ≥A- 0 1 1,048,575 1,048,575 1 19 19 Control bus R/ Data  bus
