# Lecture12.CodeOptimization

> 主题库：编译原理　｜　源文件：Lecture12.CodeOptimization.pdf　｜　20,534 字符　｜　63 个片段　｜　更新于 2026-11-11


©Copyright 2008-2023, Sun Yat-senUniversity
Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn

Page 2/63
Lecture 12.  Code Optimization
1.Introduction
2.Local Optimization
3.Control-Flow Analysis and Loop 
Optimization
4.Data-Flow Analysis and Global 
Optimization

Page 3/63
1. Introduction
Terminology
⚫Code optimization vs. code improvement
Precondition
⚫Semantics-preserving transformations
Trade-off and consequence
⚫Time efficiency vs. space efficiency
⚫Compiler efficiency vs. target code efficiency

Page 4/63
Optimization Levels
Three levels of optimization
⚫Source code
Manual, but the most effective. 
⚫Intermediate code
General and automatic. 
Necessary even you write good source code. 
⚫Target code
Machine dependent (e.g. registers and 
pipelines). 

Page 5/63
Optimization Scopes
Four scopes of optimization
⚫Peephole optimization
Based on a sliding window, the smallest one. 
⚫Local optimization
Within a basic block. 
⚫Loop optimization
Within a loop. 
⚫Global optimization
The biggest scope. 
In-Procedure vs. Inter-Procedure

Page 6/63
An Example
sum = 0;
for(inti = 1; i <= 20; i++)  sum += A[i] * B[i];
B
2
B
1
(3)t
1
=4 * i
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=4 * i
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
Common 
subexpressions

Page 7/63
An Example: 
Eliminating Common Subexpressions
B
2
B
1
(3)t
1
=4 * i
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
1
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
Loop-invariant 
computations
Loop-invariant 
computations

Page 8/63
An Example: 
Code Motion in Loop Optimization
B
2
B
1
(3)t
1
=4 * i
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
1
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
(4)t
2
=addr(A) -4
(7)t
5
=addr(B) -4
Induction variable: 
t
1
and i remain in 
lock-step

Page 9/63
An Example: Induction Variables 
and Reduction in Strength
B
2
B
1
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
1
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(3')t
1
=t
1
+ 4
(12)ifi 20gotoB
2
(1)sum=0
(2)i=1
(4)t
2
=addr(A) -4
(7)t
5
=addr(B) -4
(3)t
1
=4 * i
Loop condition can 
be changed

Page 10/63
An Example: 
Loop Condition Transformation
B
2
B
1
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
1
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(3')t
1
=t
1
+ 4
(12)ift
1
80gotoB
2
(1)sum=0
(2)i=1
(4)t
2
=addr(A) -4
(7)t
5
=addr(B) -4
(3)t
1
=4 * i
i can be removed 
if not live on exit
i is known as 1
t
4
is known as t
1

Page 11/63
An Example: 
Constant and Copy Propagation
B
2
B
1
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
1
(8)t
6
=t
5
[t
1
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(3')t
1
=t
1
+ 4
(12)ift
1
80 gotoB
2
(1)sum=0
(2)i=1
(4)t
2
=addr(A) -4
(7)t
5
=addr(B) -4
(3)t
1
=4
Suppose i is not 
live on exit
Suppose i is not 
live on exit
Suppose t
4
is not 
live on exit

Page 12/63
An Example: 
Eliminating Redundant Operations
B
2
B
1
(5)t
3
=t
2
[t
1
]
(8)t
6
=t
5
[t
1
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(3')t
1
=t
1
+ 4
(12)ift
1
80 gotoB
2
(1)sum=0
(4)t
2
=addr(A) -4
(7)t
5
=addr(B) -4
(3)t
1
=4

Page 13/63
2. Local Optimization
Basic BlockDAGBasic Block
Transformations
⚫Common subexpressions
⚫Constant and copy propagation
⚫Eliminating redundant operations

Page 14/63
One More Example (1)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0

Page 15/63
One More Example (2)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1

Page 16/63
One More Example (3)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
R
r
t
2
+

Page 17/63
One More Example (4)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
R
r
t
2
*
A
+

Page 18/63
One More Example (5)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
R
r
t
2
*
A,B
+

Page 19/63
One More Example (6)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
,t
3
R
r
t
2
*
A,B
+
2

Page 20/63
One More Example (7)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
,t
3
R
r
t
2
,t
4
*
A,B
+
2

Page 21/63
One More Example (8)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
,t
3
R
r
t
2
,t
4
*
A,B,t
5
+
2

Page 22/63
One More Example (9)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
,t
3
R
r
t
2
,t
4
*
A,B,t
5
t
6
–
+
2

Page 23/63
One More Example (10)
(1)t
0
=3.14
(2)t
1
=2 * t
0
(3)t
2
=R + r
(4)A=t
1
* t
2
(5)B=A
(6)t
3
=2 * t
0
(7)t
4
=R + r
(8)t
5
=t
3
* t
4
(9)t
6
=R -r
(10)B=t
5
* t
6
3.14
t
0
2
6.28
t
1
,t
3
R
r
t
2
,t
4
*
A,B,t
5
2
t
6
*
B
–
+

Page 24/63
One More Example (end)
(1)t
0
=3.14
(2)t
1
=6.28
(3)t
3
=6.28
(4)t
2
=R + r
(5)t
4
=t
2
(6)A=6.28 * t
2
(7)t
5
=A
(8)t
6
=R –r
(9)B=A * t
6
3.14
t
0
6.28
t
1
,t
3
R
r
t
2
,t
4
*
A,t
5
t
6
*
B
–
+
A leaf indicates 
a value from 
outside the block
A label indicates 
a value available 
to other blocks

Page 25/63
Three important subproblems
⚫How to define a loop based on a flow graph? 
⚫How to find a loop in a flow graph? 
⚫How to optimize a loop? 
3. Control-Flow Analysis and 
Loop Optimization

Page 26/63
A loop is a strongly connected subgraph 
with a unique entry (header). 
⚫Properties: 
Strongly connected
Unique entry (destination of code motion)
A loop can be expressed as a sequence of 
nodes. 
Define Loops Based on Flow Graphs

Page 27/63
An Example
1
2
4
3
5
7
6
Thereare3loops:
{5}
{4,5,6,7}
{2,3,4,5,6,7}
TheyareNOTloops:
{2,4}Both2and4areentries
{2,3,4}Both2and4areentries
{4,5,7}Both4and7areentries
{4,6,7}Both4and7areentries

Page 28/63
Dominators
Notations
⚫m DOMn means m is a dominator of n. 
⚫D(n) is the set of all dominators of n. 
D(n) = {m | m DOMn}
Properties
⚫The entry is a dominator of all nodes in the 
loop. 
⚫The binary relation DOMis a partial order
Reflective, transitive, and antisymmetric. 

Page 29/63
Algorithm to Calculate D(n)
Input: flow graph G = (N, E, n
0
)
⚫N= set of nodes; E = set of edges; n
0
= entry. 
Algorithm
1.D(n
0
) = {n
0
};
2.foreach(n N -{n
0
})  D(n) = N;
3.changed = true;
4.while(changed) {
5.changed = false;
6.foreach(n N -{n
0
}) {
7.newd = {n}(
pPRE(n)
D(p));
8.if (D(n) newd) {
9.D(n) = newd; changed = true;
10.}
11.}
12.}
PRE = predecessor

Page 30/63
The Previous Example
1
2
4
3
5
7
6
D(1) = {1}
D(2) = {1, 2}
D(3) = {1, 2, 3}
D(4) = {1, 2, 4}
D(5) = {1, 2, 4, 5}
D(6) = {1, 2, 4, 6}
D(7) = {1, 2, 4, 7}

Page 31/63
Back Edges and Natural Loops
Back edge
⚫a →bis a back edge if a →b Eb DOMa. 
Natural loop
⚫A natural loop defined by a back edge a →b
= {b} {nodes that can reach awithout going 
through b}

Page 32/63
The Previous Example
1
2
4
3
5
7
6
Thereare3backedges:
5→5
7→4
4→2
Thereare3naturalloops:
{5}definedby5→5
{4,5,6,7}definedby7→4
{2,3,4,5,6,7}definedby4→2

Page 33/63
Find Loops by Back Edges
Input: back edge n →d. 
Algorithm:
1.voidinsert(Node m) {
2.if(m loop) {// d will not be pushed
3.loop = loop {m};
4.stack.push(m)
5.}
6.}
7.void main() {
8.Stack stack = newStack();
9.loop = {d};// PRE(d) will not be added
10.insert(n);
11.while(stack.notEmpty()) {
12.m = stack.pop();
13.foreach(p PRE(m))  insert(p)
14.}
15.}

Page 34/63
Example #1
Given the back edge 7 →4
1.initialize: loop = {4, 7}, stack = [7]. 
2.pop 7; insert 5 and 6; 
loop = {4, 7, 5, 6}, stack =[5, 6]. 
3.pop 6; insert 4 (already in loop); 
loop has no change, stack = [5]. 
4.pop 5; insert 4 and 5 (both in loop); 
loop has no change, stack = []. 
5.result: loop = {4, 7, 5, 6}. 
1
2
4
3
5
7
6

Page 35/63
Example #2
Given the back edge 4 →2
1.initialize: loop = {2, 4}, stack = [4]. 
2.pop 4; insert 2, 3 and 7 (2 already in loop); 
loop ={2, 4, 3, 7}, stack = [3, 7]. 
3.pop 7; insert 5 and 6;  
loop = {2, 4, 3, 7, 5, 6}, stack = [3, 5, 6]. 
4.pop 6; insert 4 (already in loop); 
loop had no change, stack = [3, 5]. 
5.pop 5; insert 4 and 5 (both in loop); 
loop has no change, stack = [3]. 
6.pop 3; insert 2 (already in loop); 
loop has no change, stack = []. 
7.result: loop = {2, 4, 3, 7, 5, 6}. 
1
2
4
3
5
7
6

Page 36/63
Properties of Natural Loops
Natural loops do not cover all of loops in 
common sense
⚫E.g. there is no back edge in the following flow graph, 
but it does have a loop in common sense: {2, 3}. 
⚫Only in a reducible flow graph, can the back edges 
find all loops. 
Reducible flow graph
⚫After removing all back edges, 
the subgraph is acyclic. 
⚫In a reducible flow graph, the 
only entry to a loop is the header. 
⚫A flow graph generated from a structured program 
is commonly reducible. 
3
1
2

Page 37/63
Loop Optimization: 
Code Motion
Target of code motion
⚫Following the predecessor ofthe header of the 
loop. 
What code can be moved? 
For an instruction x = y opz, 
1.It is a loop-invariant operation.
All possible definitions ofyandzare outside the 
loop, including constants, or (recursively)
Defined by loop-invariant values. 
2.No other statement in the loop defines x. 
3.All uses of xin the loop are defined by it. 

Page 38/63
Loop Optimization: Reducing Strength 
and Eliminating Induction Variables
Basic induction variable
⚫i = i C
Unique assignment to iin the loop. 
Cis loop-invariant. 
Family of induction variables
⚫j = C
1
* i C
2
Both C
1
and C
2
are loop-invariant. 
Motivation
⚫Substitute iwith some jin the family. 
The multiplication of jcan be removed. 
Then ican be eliminated. 
⚫Specially effective to indexing variables. 

Page 39/63
An Example: 
Family of Induction Variables
B
2
B
1
(3)t
1
=4 * i
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=4 * i
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
Only one loop:
{B
2
}
Basic induction variable:
i
Family of induction variables:
t
1
= (i, 4, 0) = 4 * i + 0
t
4
= (i, 4, 0) = 4 * i + 0

Page 40/63
An Example: 
Strength Reduction (1)
B
2
B
1
(3)t
1
=4 * i
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=4 * i
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
Create a new variable j'(e.g. t
1
'and 
t
4
') for each induction variable in 
familyj = C
1
* i C
2
(e.g. t
1
and 
t
4
). 
Initialize new variables at the end 
of the preheader: 
j' = C
1
* i
j' = j' + C
2
// only if C
2
0

Page 41/63
An Example: 
Strength Reduction (2)
B
2
B
1
(3)t
1
=t
1
'
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
4
'
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
Change the definition of each 
induction variable (e.g. t
1
and 
t
4
): 
j = j'

Page 42/63
An Example: 
Strength Reduction (3)
B
2
B
1
(3)t
1
=t
1
'
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
]
(6)t
4
=t
4
'
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
]
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(11a)t
1
'=t
1
' + 4
(11b)t
4
'=t
4
' + 4
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
Add linear assignments to new 
variables following the unique 
definition of the basic induction 
variable (i= iC):
t = C
1
* C
j' = j' t
If C == 1, only one statement 
need to be added:
j' = j' C
1

Page 43/63
An Example: 
Eliminate Dead Induction Variables
B
2
B
1
(3)t
1
=t
1
'
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
']
(6)t
4
=t
4
'
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
']
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(11a)t
1
'=t
1
' + 4
(11b)t
4
'=t
4
' + 4
(12)ifi 20 gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
If induction variable jis not live 
on exit, 
change the use of jto j'(e.g. 
change reference from t
1
and t
4
to t
1
'and t
4
'), 
and then remove the definition of 
j(e.g. t
1
and t
4
). 

Page 44/63
An Example: 
Change Loop Condition
B
2
B
1
(3)t
1
=t
1
'
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
']
(6)t
4
=t
4
'
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
']
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(11a)t
1
'=t
1
' + 4
(11b)t
4
'=t
4
' + 4
(12)ifi 20 gotoB
2
(12a)R = 4 * 20
(12b)ift
1
' R gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
Pick a new induction variable from 
the family (sayt
1
'), then change 
the loop condition. 

Page 45/63
An Example: 
Remove Basic Induction Variable
B
2
B
1
(3)t
1
=t
1
'
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
']
(6)t
4
=t
4
'
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
']
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11)i=i + 1
(11a)t
1
'=t
1
' + 4
(11b)t
4
'=t
4
' + 4
(12)ifi 20 gotoB
2
(12a)R = 4 * 20
(12b)ift
1
' R gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
If the basic induction variable iis 
not live on exit, the definition of 
ican be removed. 

Page 46/63
An Example: 
After Loop Optimization
B
2
B
1
(4)t
2
=addr(A) -4
(5)t
3
=t
2
[t
1
']
(7)t
5
=addr(B) -4
(8)t
6
=t
5
[t
4
']
(9)t
7
=t
3
* t
6
(10)sum=sum + t
7
(11a)t
1
'=t
1
' + 4
(11b)t
4
'=t
4
' + 4
(12a)R = 4 * 20
(12b)ift
1
' R gotoB
2
(1)sum=0
(2)i=1
(2a) t
1
' = 4 * i
(2b) t
4
' = 4 * i
The optimized code facilitate 
further local optimization. 

Page 47/63
Collect information about data flows
⚫How a variable is assigned (definition)? 
⚫How a variable is referred (use)? 
Control-flow vs. data-flow
⚫Control-flow analysis: basic blocks are 
considered as blackboxes. 
⚫Data-flow analysis: basic blocks are considered 
as whiteboxes. 
4. Data-Flow Analysis and 
Global Optimization

Page 48/63
Where Global Information Are Needed? 
Local optimization
⚫Assignments to a variable can be removed if the 
variable is never used.
Loop optimization: code motion
⚫Determine loop-invariant operations according to 
the definitions of variables. 
⚫Code motion requires the operation is the unique 
definition in the loop. 
⚫Code motion also requires the defined variable is 
not live on the exit of the loop. 
Loop optimization: induction variable elimination
⚫induction variables can be removed if it is not 
used outside the loop. 
Code generation
⚫Information on liveness on exit facilitate register 
utilization. 

Page 49/63
What Global Information Are Needed? 
Definition
⚫All assignments (sources) of an R-value in a 
statement. 
Use
⚫All possible use of an L-value in a statement. 
Liveness
⚫Will the variable be referred as an R-value 
after a statement. 

Page 50/63
Basic Concepts
Points in a flow graph
⚫Between two adjacent statements. 
⚫Before the first and after the last statement. 
Definition of a variable x
⚫A statement that (may) assign(s) a value to x. 
⚫L-value. 
Use of a variable x
⚫A statement that refers xas an operand. 
⚫R-value. 
statement
statement
between statements

Page 51/63
Basic Concepts (cont')
Definition dreaches a point p
⚫There exists a path from the point 
immediately following dto p, such that dis 
not "killed" along the path. 
⚫While we use ximmediately following p, the 
value of xmay be determined by d. 

Page 52/63
Ud-Chains vs. Du-Chains
Ud-chain: the use-definition chain of a 
variable xin a use statement s
⚫Set of definitions of xthat can reach s. 
⚫Useful for finding loop-invariants. 
⚫Also for global constant folding. 
Du-chain: the definition-use chain of a 
variable xin a definition statement s
⚫Set of uses of xthat can be reached from s. 
⚫Useful for eliminating induction variables in 
loop optimization. 
⚫Also for finding family of induction variables. 

Page 53/63
Reaching Definition Analysis
Forward data-flow equation
out[B] = (in[B] -kill[B]) gen[B]
in[B] = 
pPRE(B)
out[p]
in[B]: ud-chain before the entry of B. 
out[B]: ud-chain after the exit of B. 
gen[B]: all definitions in B that can reach 
the exit of B. 
kill[B]: all definitions outside B that are 
killed by B. 

Page 54/63
Construction of Ud-Chains
Input: gen[]and kill[]; Output: in[]and out[]. 
Algorithm
for(inti = 1; i <= n; i++) {    // initialize
in[Bi] = ;  out[Bi] = gen[Bi];
}
changed = true;
while(changed) {    // iterative
changed = false;
for(i = 1; i <= n; i++) {
newIn = 
pPRE[Bi]
out[p];
if(newIn in[Bi]) {
changed = true;
in[Bi] = newIn;
out[Bi] = (in[Bi] -kill[Bi]) gen[Bi];
}
}
}

Page 55/63
An Example: 
(1) gen[ ] and kill[ ] is known
Only variable iand jare considered
d
1
:i=2
d
2
:j=i+1
d
3
:i=1
d
4
:j=j+1
d
5
:j=j-4
d
6
:a=i
d
7
:b=j
B
1
B
4
B
5
B
2
B
3
B
gen[B]kill[B]
SetVectorSetVector
B
1
{d
1
, d
2
}1100000{d
3
, d
4
, d
5
}0011100
B
2
{d
3
}0010000{d
1
}1000000
B
3
{d
4
}0001000{d
2
, d
5
}0100100
B
4
{d
5
}0000100{d
2
, d
4
}0101000
B
5

0000000

0000000

Page 56/63
An Example: 
(2) Iterations of in[ ] and out[ ] 
Depth-first visit: B
1
, B
2
, B
3
, B
4
and B
5
B
Init1
st
2
nd
3
rd
4
th
in[B]out[B]in[B]out[B]in[B]out[B]in[B]out[B]in[B]out[B]
B
1
0000000110000000100001100000011000011000000111100110000001111001100000
B
2
0000000001000011000000110000111110001111001111100011110011111000111100
B
3
0000000000100001100000011000011110000110000111100001100001111000011000
B
4
0000000000010000110000010100001100000101000011000001010000110000010100
B
5
0000000000000000111000011100001110000111000011100001110000111000011100

Page 57/63
An Example: 
(3) Construction of Ud-Chains
Compute ud-chains with in[B]. 
⚫If s.xhas definitions before sin B, ud-chain 
of s.xis a singleton (definition nearest to s). 
⚫Otherwise, ud-chain of s.xis all definitions of 
xin in[B]. 
Result ud-chains
⚫Variable i at definition d
2
: {d
1
}
⚫Variable j at definition d
4
: {d
2
, d
4
, d
5
}
⚫Variable j at definition d
5
: {d
4
}
⚫Variable i at definition d
6
: {d
3
}
⚫Variable j at definition d
7
: {d
4
, d
5
}
d
3
is the definition 
of i, notj! 
d
4
and d
5
are the 
definitions of j, noti! 

Page 58/63
Global Constant Propagation 
and Folding Based on Ud-Chains
changed = true;
while (changed) {
changed = false;
foreach(statement [S: x = ...]) {
foreach(operand S.y) {  // constant propagation
if (S.y.ud-chain has only one iandiis [y = CONST]) {
replaceall S.ywith CONST;
changed = true;
}
}
if(Shas op andeach operand is CONST) { // folding
letC = result of constant operation; 
replace Swith [x = C];
changed = true;
}
}
}

Page 59/63
More Data-Flow Equations:
Available Expressions
Forwarddata-flow equation
out[B] = (in[B] -E_kill[B]) E_gen[B]
in[B] = iif(B == ENTRY, , 
pPRE(B)
out[p])
in[B]: available expressions before B. 
out[B]: available expressions after B. 
E_gen[B]: expressions generated by B. 
E_kill[B]: expressions killed by B. 
Motivation
⚫Available expression E = X op Yat sis the last 
evaluation of Efrom entry point to s, and no 
redefinition of Xand Yafter the definition of E. 
⚫Useful: global common expression elimination. 

Page 60/63
More Data-Flow Equations:
Liveness Analysis
Backwarddata-flow equation
in[B] = (out[B] -def[B]) use[B]
out[B] = 
sSUCC(B)
in[s]
in[B]: live variables before B. 
out[B]: live variables after B. 
use[B]: live variables generated by B. 
def[B]: live variables killed by B. 
SUCC = successor

Page 61/63
Exercise 12.1
Given the following flow graph: 
⚫Compute the dominators of 
all nodes. 
⚫Find all back edges in the 
flow graph. 
⚫Find all natural loops 
defined by each back 
edge. 
1
2
5
3
4
6

Page 62/63
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Review:
Section 8.4-8.5 on DAG-based block optimization.
⚫Comprehensive Reading: 
Section 9.1 on an example of loop optimization. 
Section 9.6.1, 9.6.6 on basic concepts of loop 
optimization. 
Section 9.2.1-9.2.4 on data-flow equations and 
reaching definition analysis. 
⚫Skip Reading: 
Section 9.2.5-9.2.6 on liveness and available 
expression analysis. 
Section 9.6.4 on properties of reducible flow graphs. 

Page 63/63
Enjoy the Course!
