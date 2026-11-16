# 第2章第3讲_MIPS指令集和程序的机器表示-图集

> 主题库：计算机组成原理　｜　源文件：第2章第3讲_MIPS指令集和程序的机器表示-图集.pdf　｜　3,649 字符　｜　21 个片段　｜　更新于 2026-11-12
表1-1MIPS32中通用寄存器的约定用法寄存器名字约定命名用途 $0 zero 总是为0 $1_ at 留作汇编器生成一些合成指令 $2、$3 v0、v1 用来存放子程序返回值 $4~$7 a0~a3 调用子程序时，使用这4个寄存器传输前4个非浮点参数 $8~$15 t0~t7 临时寄存器，子程序使用时可以不用存储和恢复 $16~S23 s0~s7 子程序寄存器变量，改变这些寄存器值的子程序必须存储旧的值并在退出前恢复，对调用程序来说值不变 $24、$25 t8、t9 临时寄存器， 子程序使用时可以不用存储和恢复 $26、$27 Sk0、Sk1 由异常处理程序使用 $28或SgP Sp 全局指针 $29或Ssp SP 堆栈指针 $30或Sfp s8/fp 子程序可以用来做堆栈帧指针 $31 ra 存放子程序返回地址g.csdn.net/leishangwen

信息活

Instruction Format op s d shamt funct address add R 0 reg reg reg 0 32ten n.a. Sub (subtract) R reg reg reg 0 34ten n.a. add immediate - 8ten reg reg n.a. n.a. n.a. constant 1w (load word) - 35ten reg reg n.a. n.a. n.a. address SW (store word) - 43ten reg reg n.a. n.a. n.a. address

Logical operations C operators Java operators MiPs instructions Shiftrght r1 Bit-bybitNOT nr

i=j i↑j i==j? EISE f=g+h f=g-h Exit:

80000 0 0 19 9 4 0 80004 0 9 22 9 0 32 80008 35 9 8  80012 5 8 21 2 80016 8 19 19 1 80020  20000 80024

大

High address $fp− $ fp− $Sp- $fp→ Saved argument registers (if any) Saved return address Saved saved registers (if any) Local arrays and $ p→ structures (if any) Low address a. b. c.

again: addi $to,$zero,1 ;copy locked value 11 $tl,0($sl) ;load1inked sc $t0,0($sl) ;storeconditional beq $to,$zero,again ;branchifstorefails add $s4.$zero,$tl ;put1oad value in $s4

$s p → 7fff fffChx Stack $gp → 1000 8000hex Static data 10000000nex Text PC→0040 0000hex Reserved

Cprogram Compiler Assemblylanguage program Assembler Object: Machine language module Object: Library routine (machine language) Linker Loader Memory

Object file header Name Procedure A Textsize 100hex Data size 20hex Textsegment address lnstrucion 0 1w$a0,0($gp) 4 ja10 Datasegment (x) Relocation information Address Instruction type Dependency 0 1w x 4 ja】 B Symboltable Label Address B

Object file header Name Procedure B Text size 200hex Data size 30hex Textsegment Address Instruction 0 sw$a1.0($gp) 4 jal0 …  Datasegment 0 Y) Relocation information Address Instruction type Dependency 4 jal Symboltable Label Address Y A _

Executable file header Text size 300hex Data size 50hex Text segment Address Instruction 00400000nex 1w$a0, 8000nex($gp) 00400004hex jal 400100nex Y 00400100hex SW $a1, 8020hex($gp) 00400104hex ja1 400000nex 10000000hex (X) 10000020hex (Y)

Instruction class MIpPs examples HLL correspondence Integer Ft. pt. Arithmetic add, sub, a ddi operations in assignment statements 24% 48% Data transfer 1w,sw,1b,sb,1ui references to data structures,such as arrays 36% 39% Logical and,or,nor,andi,ori, operations in assignment statements 18% 4% Conditional branch beq,bne,slt,s1ti if statements and loops 18% 6% Jump j,jr,jal procedure calls,returns,and case/switch statements 3% 0%

80000 0 0 19 9 4 0 80004 0 9 22 9 0 32 80008 35 9 8 0 80012 5 8 21 2 80016 8 19 19 1 80020 2 20000 80024

Loop:sll $tl,$s3,2   非 Temp reg $tl = 4 * i add $tl,$tl,$s6 非 $tl = address of save[i] 1w $t0.0($tl) 非 Temp reg $t0 = save[i] bne $t0,$s5, Exit 非 go to Exit if save[i] ≠ k addi $s3.$s3.l 非 i =i+1 j Loop 非go to Loop

漫大宁立国

2.4.8 基本指令和指令类型 IBMS7/370数据传达指令傘盼记符操作名称传送字节数传送说明 L 取数 4 存储器专寄存器 LH 取字 2 存专寄器 IR 数 4 寄存器专寄器 LER 取数短 4 浮点寄存器专逐到浮点寄存器 IE 取数短 4 存专浮点寄器 IDR 取数长 8 浮点寄存器专到乳点寄器 D 取数长  8 存储专到浮点寄器 ST 存数 4 寄器专到存储器 SIH 存半字 2 寄存器专到存储器 SIC 存字节 1 寄器专經存储器 SIE 存数短 4 浮点寄存器专到存储器 SID 存数长 8 浮寄器专到存储器

2． 寻址方式 [MlpSae方式  MIPS所有指令都是32位宽， 按字地址对齐 0字节 1字节 2字节 3字节  [如baee？ 伪直接寻址 Q 0p Address て26位） PC ord 3O

MiPS 的 cai/return/ Jump/brancn compare 指今 Instruction Example Meaning jump j10000 go to10000 call / return jump and link jal 10000 $ra = PC + 4 ;go to 10000 forrocedureall jump register jr $ra go to $ra for switch, procedure return  bltblebgt，bge无对应的机器指令，但有对应的汇编指e 46
