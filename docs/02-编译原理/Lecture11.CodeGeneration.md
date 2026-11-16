# Lecture11.CodeGeneration

> 主题库：编译原理　｜　源文件：Lecture11.CodeGeneration.pdf　｜　13,732 字符　｜　52 个片段　｜　更新于 2026-11-11


©Copyright 2008-2022, Sun Yat-senUniversity
Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn

Page 2/52
Lecture 11.  Code Generation
1.Introduction
2.Abstraction of Target Machines
3.Implementation of Procedures
4.Optimization of Basic Blocks
5.A Simple Code Generator
6.Peephole Optimization

Page 3/52
1. Introduction
Code generators focus on
1. Instruction selection
Choose appropriate target-machine 
instructions to implement IR statements. 
2. Register allocation and assignment
Make full use of registers, the fastest 
computational unit. 
3. Instruction ordering
Decide the order in which the execution of 
instructions is scheduled. 
Also called instruction scheduling. 

Page 4/52
Instruction Selection
Both the speed and the size cost of the 
generated code should be considered. 
⚫Trade-off and consequence. 
Example 1: a = a + 1
⚫LDR0,a
ADDR0,R0,#1
STa,R0
⚫INCa
Example 2: set register R0 to 0
⚫LDR0,#0
⚫XORR0, R0

Page 5/52
Register Allocation and Assignment
Include two subproblems 
⚫Register allocation
Select the set of variables that will reside in 
registers at each point in the program. 
⚫Register assignment 
Pick the specific register that a variable will 
reside in. 
A difficult problem in code generation. 

Page 6/52
Instruction Scheduling
Important for modern pipelined processors
⚫RISC: Reduced Instruction Set Computer
RISC vs. CISC
⚫Data Hazards (stallsin a pipeline): attempt to 
use a datum before it becomes available in 
register. 
A value written to a register will not be available 
in its following cycles. 
The following cycles should be utilized by 
instructions that does not depend on the value
⚫Or they will be wasted.
Optimization: clustering instructions with no 
dependencies together, while preserving original 
semantics.  

Page 7/52
Instruction Scheduling (cont')
An example: (a + b) + con SGI's MIPS 2000
⚫LDR1, a
LDR2, b
NOP
ADDiR1,R1,R2
LDR2,c
NOP
ADDiR1,R1,R2
Optimal generated code
⚫LDR1, a
LDR2, b
LDR3,c
ADDiR1,R1,R2
NOP
ADDiR1,R1,R3

Page 8/52
2. Abstraction of Target Machines
Instruction set
Addressing modes
Program and instruction costs

Page 9/52
Instruction Set
Supports the following instructions
⚫LDdst,addr// load
⚫STx, Ri// store
⚫OPdst,src1,src2// ADD, SUB, ...
OPdst,src1// unary operation
⚫BRL// branch
⚫Bcondr,L// conditional branch

Page 10/52
Addressing Modes
Addressing in instructions
⚫#C// immediate constant, cost = 1
⚫x// absolute, cost = 1
⚫*x// indirect memory, cost = 1
⚫R// direct register, cost = 0
⚫*R// indirect register, cost = 0
⚫a(Ri)// direct indexed, cost = 1
ais a variable or a constant
LDR1,a(R2)
⚫R1 = contents(a + contents(R2))
LDR1,100(R2)
⚫R1 = contents(100 + contents(R2))
⚫*a(Ri)// indirect indexed, cost = 1
LDR1,*100(R2)
⚫R1 = contents(contents(100 + contents(R2)))

Page 11/52
Program and Instruction Costs
Cost of an instruction
⚫= 1 + operand_addr_cost
Cost of a program
⚫= total of instruction costs
Examples
⚫LDR0,R1// cost = 1
⚫LDR0,x// cost = 2
⚫LDR1,*100(R2)// cost = 2

Page 12/52
3. Implementation of Procedures
Static Allocation
Stack Allocation

Page 13/52
Static Allocation
Implementation of call callee
⚫...
STcallee.staticArea,#here+ 20// return address
BRcallee.codeArea// jump to Callee
...
Implementation of return
⚫...
BR*callee.staticArea// return to Caller
Assume that the return address 
is saved at the beginningof 
the activation record. And
20 = 5 words * 4 bytes/word

Page 14/52
An Example: Three-Address Code
// code for c
action
1
call p
action
2
halt
// code for p
action
3
return

Page 15/52
An Example: Implementation
// code for c
100action
1
120ST364,#140// return address
132BR200// call p
140action
2
160HALT// return to operating system
...
// code for p
200action
3
220BR*364// return to its caller
....
// activation record for c
300// return address to Caller
304// local data for c
....
// activation record for p
364140// return address to Caller
368// local data for p

Page 16/52
Stack Allocation
Implementation of initialization
⚫LDSP,#stackStart// initialize the stack
...// code for main()
HALT// terminate
Implementation of call callee
⚫...
ADDSP,SP,#caller.recordSize// push an AR
ST*SP,#here+ 16// return address
BRcallee.codeArea// jump to Callee
SUBSP,SP,#caller.recordSize// pop the AR
...
Implementation of return
⚫...
BR*0(SP)// return to Caller
DBv2 must assume that 
addressing 0(SP)has no 
additional cost

Page 17/52
An Example: Three-Address Code
// code for m
action
1
call q
action
2
halt
// code for p
action
3
return
// code for q
action
4
call p
action
5
call q
action
6
call q
return

Page 18/52
An Example: Implementation
100LDSP,#600
108action
1
128ADDSP,SP,#msize
136ST*SP, #152
144BR300
152SUBSP,SP,#msize
160action
2
180HALT
...
200action
3
220BR*0(SP)
....
300action
4
320ADDSP,SP,#qsize
328ST*SP, #344
336BR200
344SUBSP,SP,#qsize
352action
5
372ADDSP,SP,#qsize
380ST*SP, #396
388BR300
396SUBSP,SP,#qsize
404action
6
424ADDSP,SP,#qsize
432ST*SP, #448
440BR300
448SUBSP,SP,#qsize
456BR*0(SP)
...
600
code for p
code for q
blue for call 
sequence
stack

Page 19/52
4. Optimization of Basic Blocks
Approach
Basic BlockDAGBasic Block
Petri Nets in
Super Compiler

Page 20/52
Basic Blocks
Single entry and single exit 
Example
(10)i = i + 1
(11)ifi <= 10 goto2
(12)i = 1
(13)t
5
= i –1
(14)t
6
= 88 * t
5
(15)a[t
6
] = 1.0
(16)i = i + 1
(17)ifi <= 10 goto13
(1)i = 1
(2)j = 1
(3)t
1
= 10 * i
(4)t
2
= t
1
+ j
(5)t
3
= 8 * t
2
(6)t
4
= t
3
–88
(7)a[t
4
] = 0.0
(8)j = j + 1
(9)ifj <= 10 goto3

Page 21/52
Flow Graphs
i = 1
t
1
= 10 * i
t
2
= t
1
+ j
t
3
= 8 * t
2
t
4
= t
3
–88
a[t
4
] = 0.0
j = j + 1
ifj <= 10 gotoB
3
j = 1
i = 1
i = i + 1
ifi <= 10 gotoB
2
ENTRY
EXIT
t
5
= i –1
t
6
= 88 * t
5
a[t
6
] = 1.0
i = i + 1
ifi <= 10 gotoB
6
B
1
B
5
B
4
B
3
B
2
B
6

Page 22/52
Construction of Flow Graphs
Partition three-address instructions into 
basic blocks
⚫Leader: the 1
st
instruction in a basic block. 
⚫Algorithm to find all leaders
1.The 1
st
three-address instruction
2.Target of a conditional or unconditional 
jump
3.Instruction immediately follows a 
conditional or unconditional jump

Page 23/52
Liveness and Next-Use Information
Calculate liveness and next-use info in a 
basic block: backward scanning
⚫Initialize: for each variable v, 
v.nextUse= none; 
v.liveness= vis temporary ? false: true;
⚫For each i: x = y + z
Attach information of x, yand zto instruction i;
x.liveness= false;
x.nextUse= none; 
y.liveness= z.liveness= true; 
y.nextUse= z.nextUse= i;
Conservative. 
Global data-flow 
analysis in practice

Page 24/52
An Example
Information are stored at the entry of each 
variables in the symbol table
(1)t=a –b
(2)u=a –c
(3)v=t + u
(4)d=v + u 
(1)t
(3),T
=a
(2),T
–b
–,T
(2)u
(3),T
=a
–,T
–c
–,T
(3)v
(4),T
=t
–,F
+ u
(4),T
(4)d
–,T
=v
–,F
+ u
–,F
Var.
Next-UseLiveness
Init(4)(3)(2)(1)Init(4)(3)(2)(1)
a
−
(2)(1)TTT
b
−
(1)TT
c
−
(2)TT
d
−−
TF
t
−
(3)
−
FTF
u
−
(4)(3)
−
FTTF
v
−
(4)
−
FTF

Page 25/52
Optimization Based on DAGs
Perform several code-improving 
transformations
⚫Elimination of local common subexpressions
⚫Elimination of dead code
⚫Reordering of independent statements
⚫Application of algebraic laws

Page 26/52
Local Common Subexpressions
a = b + c
b = a –d
c = b + c
d = a –d
b
0
c
0
+
d
0
–
+
b, d
c
a
a = b + c
d = a –d
c = d + c
b = d
Only if b is
live on exit

Page 27/52
Dead Code
Suppose only 
aand bare 
live on exit
a = b + c
b = b –d
c = c + d
e = b + c
a = b + c
b = b –d
b
0
c
0
+
d
0
–+
bca
+
e
1st
2nd

Page 28/52
Algebraic Identities
Arithmetic identities
⚫x + 0 = 0 + x = x
⚫x * 1 = 1 * x = x
⚫x –0 = x
⚫x / 1 = x
⚫...
Local reduction in strength
⚫2 * x x + x
⚫x / 2x * 0.5
⚫...
Constant folding
⚫2 * 3.146.28
⚫...

Page 29/52
Array References
x = a[i]
a[j] = y
z = a[i]
a
0
i
0
=[ ]
j
0
=[ ]
x
killed
=[ ]
z
y
0

Page 30/52
An Example: 
Construction a DAG from a BB (1)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
4
*
t
1
i
0

Page 31/52
An Example: 
Construction a DAG from a BB (2)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
4
*
t
1
i
0
=[ ]
t
2

Page 32/52
An Example: 
Construction a DAG from a BB (3)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
4
*
t
1
, t
3
i
0
=[ ]
t
2

Page 33/52
An Example: 
Construction a DAG from a BB (4)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
i
0
t
4
=[ ]
t
2

Page 34/52
An Example: 
Construction a DAG from a BB (5)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
t
4
t
5
=[ ]
t
2

Page 35/52
An Example: 
Construction a DAG from a BB (6)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
t
4
t
5
=[ ]
t
2
+
prod
0
t
6

Page 36/52
An Example: 
Construction a DAG from a BB (7)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
t
4
t
5
=[ ]
t
2
+
prod
0
t
6
, prod

Page 37/52
An Example: 
Construction a DAG from a BB (8)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
1
+
t
7
t
4
t
5
=[ ]
t
2
+
prod
0
t
6
, prod

Page 38/52
An Example: 
Construction a DAG from a BB (9)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
1
+
t
7
, i
t
4
t
5
=[ ]
t
2
+
prod
0
t
6
, prod

Page 39/52
An Example: 
Construction a DAG from a BB (10)
t
1
= 4 * i
t
2
= a[t
1
]
t
3
= 4 * i
t
4
= b[t
3
]
t
5
= t
2
* t
4
t
6
= t
5
+ prod
prod = t
6
t
7
= i + 1
i = t
7
ifi <= 20 goto(1)
a
0
b
0
=[ ]
4
*
t
1
, t
3
*
i
0
1
+
t
7
, i20
<=
(1)
t
4
t
5
=[ ]
t
2
+
prod
0
t
6
, prod

Page 40/52
5. A Simple Code Generator
What does "simple" mean? 
⚫Generate code for a single basic block. 
Motivation
⚫Make full use of registers to the best advantage. 
Programmer
Compiler
OS and 
Hardware
StorageAccess CyclesCapability
Registers       1256-8000 Bytes
Cache             3256 KB -1 MB
Main memory20-10032 MB -1 GB
Disk                0.5-5 Mega10 GB -1 TB

Page 41/52
Register and Address Descriptors
Register descriptor
⚫RegDesc: Register→2
Variable
Address descriptor
⚫AddrDesc: Variable→2
Location
⚫Location = Register Memory

Page 42/52
Code-Generation Algorithm
Based on function getReg(x = y op z)
⚫Select registers for variables in three-address 
instruction x = y op z. 
That is R
x
, R
y
and R
z
for x, y and z respectively. 
⚫Make decisions based on the register 
descriptor and the address descriptor. 

Page 43/52
Code-Generation Algorithm (cont')
Machine instructions for x = y + z
⚫Call getReg(x = y + z)to select registers R
x
, R
y
and R
z
for x, y and z. 
⚫Ify regDesc(R
y
),get some y' addrDesc(y)
and issue an instruction: LD R
y
, y'. 
⚫Ifz regDesc(R
z
),get some z' addrDesc(z)
and issue an instruction: LD R
z
, z'. 
⚫Issue an instruction: ADD R
x
, R
y
, R
z
. 
⚫Adjust register and address descriptors. 

Page 44/52
Code-Generation Algorithm (cont')
Machine instructions for x = y
⚫Call getReg(x = y)to select registers R
x
and R
y
. 
getReg()will always choose the same register for 
both x and y. 
⚫Ify regDesc(R
y
),get some y' addrDesc(y)
and issue an instruction: LD R
y
, y'. 
⚫Adjust register descriptor: regDesc(R
y
) = {x}

Page 45/52
Design of getReg(...)
Input: x = y + z
Output: R
x
, R
y
and R
z
Algorithm(use R
y
as an example)
⚫if y in some registers, pick R
y
in them. 
⚫elsifthere is empty registers, pick one as R
y
. 
⚫elsif{ let R be a candidate and R holds v, 
foreachv check: 
ifaddrDesc(v) has other location, R is OK. 
elsifv == x andx != z, R is OK. 
elsifv is not used later, R is OK. 
elsespill, i.e. issue ST v, R. 
⚫choose the R with minimal spills }. 

Page 46/52
6. Peephole Optimization
A simple but effective technique for 
locally code improvement
⚫Examine a sliding window (peephole) of target 
instructions. 
⚫Replace instruction sequence within the 
peephole by a shorter or faster sequence. 

Page 47/52
Eliminating 
Redundant Loads and Stores
Examples
⚫LDR0,a
STa,R0
// eliminated

Page 48/52
Eliminating 
Unreachable Code
Examples
⚫if debug == 1 gotoL1
gotoL2
L1:print debugging information
L2:
if debug != 1 gotoL2
print debugging information
L2:

Page 49/52
Flow-of-Control Optimizations
Examples
⚫goto L1
...
L1:goto L2
goto L2
...
L1:goto L2
⚫if a < b goto L1
...
L1:goto L2
if a < b goto L2
...
L1:goto L2
Recall the translation scheme of
flow-of-control statements! 

Page 50/52
Algebraic Simplification and 
Reduction in Strength
Examples
⚫x = x + 0
// eliminated
⚫x = x * 1
// eliminated
⚫y = x * 2
y = x << 1
⚫y = x * 4
y = x << 2

Page 51/52
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 8.1-8.2 on introduction to code 
generation and abstraction of target machines. 
Section 8.3 on implementation of procedures. 
Section 8.4-8.5 on DAG-based block 
optimization. 
Section 8.6 on a simple code generator. 
⚫Skip Reading: 
Section 8.7 on peephole optimization. 
Section 8.8-8.11 on more advanced topics. 

Page 52/52
Enjoy the Course!
