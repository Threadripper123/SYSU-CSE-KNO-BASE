# ca-lec01-intro-v1-图集

> 主题库：并行计算机体系结构　｜　源文件：ca-lec01-intro-v1-图集.pdf　｜　6,271 字符　｜　49 个片段　｜　更新于 2026-11-11
SUN YAT-SEN UNIVERSITY

计算机体系结构-24秋群号：891569343 扫一扫二维码，加入群聊

日日日E 1010 0 1100 品 0000000 00000 1080

SMT SMT SMT CPU CPU CPU CPU Core0  Core2   Core1   SMT Core3  2MB 2MB  2MB   2MB of of of of 8MBL3 8MBL3 8MBL3 8MBL3   Cache Cache Cache Cache

John L.Hennessy David A.Patterson COMPUTER ARCHITECTURE AQuantitative Approach M MORGAN KAUFMANN

计算机体系结构化研究法 /机量  COMPUTER ARCHITECTURE A QuantitatveAppraach 工 M

Cal

行计算系列丛书并行计算机体系结构 (第2版)  陈国良吴俊敏主编高里教育公版社

2019最美科技工作者让机器更好地服炎

中国科学院学研究教材系列AICompuingSystems 智能计算系统 d dldli 从深度学到模型第2   金面贯穿人工智能整个软硬件技术栈以应用驱动，形成智能领域的系统思维以大模型为牵引，为通用人工智能提供排机械工业出版社

采用龙芯团队推出的L.ogArch指令系统，全面展现指令系统设计的发展益势从硬件工程师的角度理解软件，从软件工程师的角度理解硬件优化篇章结构与教学体验，全书开源且配有丰富的教学资源工出

LOORGSCN

OSCHOOLBUSD

Intel_Corei74cores4.2_GHz(Boostto4.5GHz) IntelCorei74cores4.0GHz(Boostto4.2GHz) Intel Core i74 cores 4.0 GHz (Boost to 4.2 GHz) Intel Xeon4cores 3.7 GHz(Boostto4.1 GHz) 100,000 IntelXeon4cores3.6 GHz(Boostto4.0 GHz) DigitalAlphastation 5/300,300 MHz 280 23%/year 12%/year 3.5%/year Digitalalphastation 4/266,266 mhz 183 100 IBMPOWERstation100,150 MHz 117 Digital3000 axp/500,150 mHz 80 HP9000/750,66MHz 51 IBMRS6000/540,30 MHz 52%/year 24 MIPS MHz 18 MIPSM/120,16.7MHz 10 13 Sun-4/260.16.7MHz 9 VAX8700,22MHz 5 AX-11/780,5 MHz 25%/year 1978 1980 19821984 19861988 1990 1992 19941996 19982000 20022004 2006200820102012 20142016 2018

40 Years ofMicroprocessor Trend Data 107 Transistors 106 Pdynamic≈N*C*V2 *A (ThOuSands) 105 Single-Thread lPerformance 104 (SpecINT×10³) Frequency(mHz) 103 Typical Power (W) 102 101 Numberof Logical Cores 100 1970 1980 1990 2000 2010 2020 Year Dennard Scaling Fails inHere

1010 108- 106 104- [Ta 102 1— 10-2- Clockspeeds(MHz)  1960 T 1974 1988 2002 2016

RISC

Aligned Not Aligned

FirstOperand Second Operand Destination Register(direct) op rs rt rd reaister Immediate op rs rt immed Base+index op rs rt immed I Memory Yegister PC-relative op rs rt immed Memory PC

Instructiontype/opcode Datatransers 1b,1bu,sb 1h, lhu,sh iW,TU,SW 1d,sd flw,fid,f sw,f sd fmv._.x,fmv.x._ csrrw,csrrwi.csrrs, csrrsi,csrrc,csrrci Arithmetic/logical add.addi.addw.addiw sub,subw mul,mulw,mulh,mulhsu, mulhu div,divu,rem,remu divw,divuw,remw,remuw and,andi or,ori,xor,xori

Instructiontype/opcode Floating point fadd.d.fadd.s fsub.d.fsub.s fmul.d.fmui.s fmadd.d.fmadd.s.fnmadd. fnmadd.s fmsub.d.fmsub.s.fnmsub. fnmsub.s fdiv.d,fdiv.s fsqrt.d,fsqrt.s fmax.d,fmax.s,fmin.d, fmin.s fcvt._·_,fcvt._·_u, fcvt._u._ feq._,flt._,fle._ fclass.d, fclass.s fsgnj._,fsgnjn._, fsgnjx._

aperand Opcode Address PC Memory

31 2524 2019 1514 1211 76 0 funct7 rs2 rs1 funct3 rd opcode  R-type  imm [11:0] rs1 funct3 rd opcode I-type imm [11:5] rs2 rs1 funct3 imm [4:0] opcode S-type  imm [12] imm [10:5] rs2 rs1 funct3 imm [4:1|11] opcode B-type imm[31:12] rd opcode U-type imm [20|10:1|11|19:12] rd opcode  J-type

100,000 Network DIsk × 100 (Latencyimprovement =Bandwidthimprovement) 10 10 100 Relative Latency Improvement

250nm 1801 nm 0.10 130 m 100 90nm 65 nm Feature 4s nm Size 0.01 10 1980 1990 2000 2010 2020

Energydynamic ∞ Capacitive load × Voltage2 2

Energydynamic × 1/2 × Capacitive load × Voltage2 2 2

Powerdynamic ∞ 1/2 × Capacitive load × Voltage2 × Frequency switched

10,000 Intel Skylake Core i7 IntelPentium4Xeon 4200 MHzin2017 3200MHzin2003 Intel PentiunhI 1000 MHzn2000 2%lyear 1000 DigitalAlpha21164A 500MHzi1996 DigitaApha210641 0%/ye 50MHz in1992 100 4 MIPS M200 25 MHzi19d9 10 Sun-4SPARC 16.7 MHzin1986 DigitalVAX-11/780 5MHzin 1978 15%lyear

100— .4GHz2 80- 1 .8GHz 2 60- 1GHz 40 20_ 0 DVS savings(%) Idle 7 14 21 29 36 43 50 57 64 71 79 86 93 100 Computeload (%)

Powerstatic ∞ Currentstatic × Voltage

Relativeenergy cost Relativearea cost Operation: Energy (pJ) Area (μm²) 8bAdd 0.03 36 16bAdd 0.05 67 32bAdd 0.1 137 16bFB Add 0.4 1360 32bFBAdd 0.9 4184 8bMult 0.2 282 32bMult 3.1 3495 16bFBMult 1.1 1640 32bFBMult 3.7 7700 32bSRAM Read(8KB) 5 N/A 32bDRAMRead 640 N/A 1  10 100 1000 10000 1 10 100 1000

80 70 One dollar in 1977 =$2.95in 2001 16Mbits CostMB =$500in1997 60 =$0.35in2000 50 emad exceeded upply =$0.08 in2001 2 price slow dro bis D 40 bis 4 56 i 30 64K bits 20 10 上 16Kbits 64Mbits 0

Siliconingot Blank wafers Slicer 2oto30processingsteps 8-12in diameter <0.1in thick 12-24inlong Tested dies Individual dies Patterned wafer Die Dicer Tester Packageddies Tested Packaged dies Bond die to Part Ship to pacage Tester Customers

Availability = Function of: Reliabiity, Maintainability Rellability Operating state 1 Plant design, operation, and m aintenance Down     1 1 state 1

Time Between Failures Time to Tim e to Repair Failure System Resume Normal System Failure Operations Failure

fails repairs 100hr 3hr 120hr 2hr 140hr 4hr t A=MTTF/(MTTF+MTTR) MTTF=(100 + 120 + 140)/3 = 120 (hr/case) M TTR =(3 + 2 + 4)/ 3 = 3 (hr/case) A=120/120+3 =120/123 = 0.975

任务级并数据级并 1~15 数据集 1~5 6~10 11~15 任务1 任务1 任务1 任务1 任务2 任务3 任务2 任务2 任务2 任务3 任务3 任务3 [汇总汇总

Y PARALLE     SECONDEDITION ALMASI/GOTTLIEB

NODE ODE NODE NODE NODE ODE memory memory memory memory memory memory core core core core core core core core core core core core core core core core core core core core core core core core

L2 L2   06 09   b DDR3 W DDR3 L2  L2 00 15 L2 03 12 健   2  4    UPC BE DB Network PCle P PU PU 00 MSg PI 17 unit  SerDes SerDes

WE compute node login /remote partition server node infiniband switch gatewaynode mammmanagementharware

New Applications More Performance

Grand Challenge problems Global change Humangenome Fluidturbulence 1 TB Vehicle dynamics  Ocean circulation 10 GB Structural biology Vehicle 1GB 72-hour cYa nat 100 MB 48-hour 3Dplasma weather modeling Chemical dynamics 2D 10 MB Oilreservoir airfoil modeling 100MFLOPS 1GFLOPS 10 GFLOPS 100 GFLOPS 1TFLOPS Computational performance requirement

1 ZFlops 100Eflops 10eelops GenomicsResearch 1EFlops 10PFlops Medical Imaging 1PFlops 100TFlops 10TFlops 1TFlops 10 GFlops 1GFlops Forecast 100MFlops 1993 1999 2005 2011 2017 2023 2029 ComputingPowerAvailableToday intel Source:www.top500.org 2 emS eの

Time-constrainedscaling example Real-time 3D graphics: more compute power allows for rendering of much more complex scene Problem size metrics:number ofpolygons, texels sampled, shader length,etc. Ha -Lfe(998) Assassin'' e Un ( 4

D TRON

40 20

0000000000 0 0220000033
