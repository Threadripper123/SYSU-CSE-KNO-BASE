# Lecture08.SyntaxDirectedTranslation

> 主题库：编译原理　｜　源文件：Lecture08.SyntaxDirectedTranslation.pdf　｜　19,914 字符　｜　63 个片段　｜　更新于 2026-11-11


©Copyright 2008-2023, Sun Yat-senUniversity
Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn

Page 2/63
Lecture 8.  Syntax-Directed Translation
1.Introduction
2.Syntax-Directed Definition: Examples
3.Evaluation Order and Dependency Graphs
4.S-Attributed Definitions
5.L-Attributed Definitions and Translation 
Schemes
6.L-Attributed Definitions in Predictive 
Parsing
7.L-Attributed Definitions in LR Parsing

Page 3/63
1. Introduction
What is syntax-directed translation? 
⚫E.g. denotational semantics
Every component has its denotation. 
The denotation of a composite component 
depends only on the denotations of its sub-
components. 
Why syntax-directed? 
D
LT
,kLint
i
,jL

Page 4/63
Applications of 
Syntax-Directed Translation
Two compiling phases are covered
⚫Semantic analysis
⚫Intermediate code generation
Our learning steps
1.The general concepts and framework of 
syntax-directed translation
2.Application of these concepts and framework 
to semantic analysis and intermediate code 
generation. 

Page 5/63
Basic Ideas
Associate grammar symbols with 
attributes
⚫Based on specific applications. 
⚫The datato be manipulated. 
Associate productions with semantic rules
⚫Also named semantic actions. 
⚫The operationsthat manipulate the attributes. 
A →B
A.s = B.i;  B.i = A.s + 1
AA.s
BB.i
...

Page 6/63
More Insights: A Virtual Machine
Syntax-directed translation can be 
explained as a virtual machine
⚫What to do:  semantic actions. 
⚫How to do:  evaluation order, i.e. the order 
in which the actions are performed. 
VM
attributes
semantic
actions

Page 7/63
Challenges
Efficiency of the decision of semantic 
actions execution order
⚫The best way is to execute the semantic 
actions while parsing. 
⚫I.e. the evaluation order of actions is the same 
as the order of parsing output. 
⚫Trade-off:  capability vs. efficiency

Page 8/63
Concepts
Syntax-directed definition
⚫Translation Scheme
Annotated parse tree
⚫Annotated parse tree with actions
Synthesized Attribute
Inherited Attribute

Page 9/63
Syntax-Directed Definition
ProductionsSemantic Rules
1
L  →E n
L.val = E.val
print(L.val)
2
E  →E
1
+T
E.val = E
1
.val + T.val
3
E  →T
E.val = T.val
4
T
1
→T
2
*F
T
1
.val = T
2
.val * F.val
5
T  →F
T.val = F.val
6
F  →(E )
F.val = E.val
7
F  →digit
F.val = digit.lexval
Different 
subfix style
Side-effects

Page 10/63
Annotated Parse Tree
L.val = 19
E.val = 19
+T.val = 4E.val = 15
F.val = 4
digit.lexval = 4*F.val = 5T.val = 3
T.val = 15
digit.lexval = 5F.val = 3
digit.lexval = 3
Synthesized attributes
n
constants
variables

Page 11/63
Inherited Attribute
No.ProductionsSemantic Rules
1
D  →T L
L.inh= T.type
2
T  →int
T.type = INTEGER
3
T  →real
T.type = REAL
4
L  →L
1
,id
L
1
.inh= L.inh
addType(id.entry, L.inh)
5
L  →id
addType(id.entry, L.inh)

Page 12/63
Annotated Parse Tree
D
L.inh = REALT.type = REAL
,id
3
L.inh = REALreal
id
1
,id
2
L.inh = REAL

Page 13/63
2. Syntax-Directed Definition: 
Examples
Construction of Syntax Trees
Construction of DAG
Type Structure of Arrays

Page 14/63
Syntax Trees
No.ProductionsSemantic Rules
1
E  →E
1
+T
E.node = newNode('+', E
1
.node, T.node)
2
E  →E
1
–TE.node = newNode('–', E
1
.node, T.node)
3
E  →T
E.node = T.node
4
T  →(E )
T.node = E.node
5
T  →id
T.node = new Leaf(id, id.entry)
6
T  →num
T.node = new Leaf(num, num.val)

Page 15/63
Syntax Trees (cont')
Syntax tree for a –4 + c
E.node =
T.node =E.node =
id
num
+
T.node =E.node =–
T.node =
id
(1)  p
1
= newLeaf(id, entry-a)
(2)  p
2
= newLeaf(num, 4)
(3)  p
3
= newNode('–', p
1
, p
2
)
(4)  p
4
= newLeaf(id, entry-c)
(5)  p
5
= newNode('+', p
3
, p
4
)
id
id
num
–
+
1
6
5
4
3
2

Page 16/63
DAG
DAG: Directed Acyclic Graph
⚫a + a * (b –c) + (b –c) * d
(1)p
1
= newLeaf(id, entry-a)
(2)p
2
= newLeaf(id, entry-a)
(3)p
3
= newLeaf(id, entry-b)
(4)p
4
= newLeaf(id, entry-c)
(5)p
5
= newNode('–', p
3
, p
4
)
(6)p
6
= newNode('*', p
2
, p
5
)
(7)p
7
= newNode('+', p
1
, p
6
)
(8)p
8
= newLeaf(id, entry-b)
(9)p
9
= newLeaf(id, entry-c)
(10)p
10
= newNode('–', p
8
, p
9
)
(11)p
11
= newLeaf(id, entry-d)
(12)p
12
= newNode('*', p
10
, p
11
)
(13)p
13
= newNode('+', p
7
, p
12
)
*
a–
+
+
*
cb
d

Page 17/63
Type Structures
No.ProductionsSemantic Rules
1
T  →B C
T.type = C.type
C.base = B.type
2
B  →int
B.type = int
3
B  →float
B.type = float
4
C  →[ num ]C
1
C.type = array(num.val, C
1
.type)
C
1
.base = C.base
5
C  →
C.type = C.base

Page 18/63
Type Structures (cont')
Type structure for int[2][3]
T.type = array(2, array(3, integer))
B.type = integerC.base = integer
C.type = array(2, array(3, integer))
int
C.base = integer
C.type = array(3, integer)
2[]
3[]C.base = integer
C.type = integer


Page 19/63
3. Evaluation Order and Dependency 
Graphs
A general framework of working steps: 
⚫Introduce attributes to grammar symbols. 
⚫Define semantic rules for each production. 
⚫Draw the dependency graphbased on the 
parse tree. 
⚫Determine the evaluation orderby 
topological sorting of the dependency graph. 
⚫Execute the semantic rules according to the 
evaluation order. 
Work with explicitparse tree

Page 20/63
Pros and Cons
Pros
⚫A general and powerful approach. 
⚫Can be used to demonstrate the principles of 
evaluation order of syntax-directed definitions. 
Cons
⚫Explicit parse tree. 
⚫Low efficiency and impractical. 

Page 21/63
The Previous Syntax-Directed Definition
No.ProductionsSemantic Rules
1
D  →T L
L.inh= T.type
2
T  →int
T.type = INTEGER
3
T  →real
T.type = REAL
4
L  →L
1
,id
L
1
.inh= L.inh
addType(id.entry, L.inh)
5
L  →id
addType(id.entry, L.inh)

Page 22/63
Dependency Graph: An Example
D
L.inh = REALT.type = REAL
,id
3
L.inh = REALreal
id
1
,id
2
L.inh = REAL
456
78
910
1
2
3
6, 8, 10 are dummy attributes
addType(id.entry, L.inh)

Page 23/63
Evaluation Order
Topological sorting
⚫Specification: dependency graph
⚫Implementation: evaluation order
Mapping a specification to an 
implementation
⚫1 : 1  --without any cycles in the graph
⚫1 : n  --without any cycles in the graph
⚫1 : 0  --with cycles in the graph

Page 24/63
Implicit Parse Trees
S-attributed definitions
⚫Every attribute is synthesized. 
⚫Guarantee an evaluation order that is the same 
order of the output of LR parsing. 
L-attributed definitions
⚫Every attribute is synthesized, or only inherited 
from parent or left siblings (not from right 
siblings). 
⚫Guarantee an evaluation order that is the same 
order of recursive descent predictive parsing. 

Page 25/63
4. S-Attributed Definitions
A syntax-directed definition with only 
synthesized attributes
Evaluation order for S-attributed 
definitions
⚫Upwards, and only upwards
⚫The same order as LR parsing! 

Page 26/63
Specification vs. Implementation
Evaluate S-attributed definitions while 
bottom-up parsing
S-Attributed Definition
Evaluation while LR Parsing
Specification
Implementation

Page 27/63
The Previous Calculator Example
No.ProductionsSemantic Rules
1
L  →E n
L.val = E.val
print(L.val)
2
E  →E
1
+T
E.val = E
1
.val + T.val
3
E  →T
E.val = T.val
4
T
1
→T
2
*F
T
1
.val = T
2
.val *F.val
5
T  →F
T.val = F.val
6
F  →(E )
F.val = E.val
7
F  →digit
F.val= digit.lexval
Associate attributes of  grammar symbols with positions in the parsing stack. 

Page 28/63
Implementation in LR Parsing
No.ProductionsCodeNotes
1
L  →E n
stack[ntop].val = stack[top –1].val; 
print(stack[ntop].val); 
2
E  →E
1
+T
stack[ntop].val = 
stack[top –2].val + stack[top].val; 
stack[top –1].val = '+'
3
E  →T
4
T
1
→T
2
*F
stack[ntop].val = 
stack[top –2].val * stack[top].val; 
stack[top –1].val = '*'
5
T  →F
6
F  →(E )
stack[ntop].val = stack[top –1].val; 
stack[top].val = ')'
stack[top –2].val = '('
7
F  →digit
Setup an attribute stack with the same height as parsing (state) stack: 
ntop = top –| right-side | + 1

Page 29/63
Evaluation While LR Parsing
Step
States
(Illustrative)
AttributesInputCodeOutput
1$$3 * 5 + 4 n $
2$ 3$ 3* 5 + 4 n $--
F→digit
3$ F$ 3* 5 + 4 n $--
T→F
4$ T$ 3* 5 + 4 n $
5$ T *$ 3 *5 + 4 n $
6$ T * 5$ 3 * 5+ 4 n $--
F→digit
7$ T * F$ 3 * 5+ 4 n $3 * 5
T→T *F
8$ T$ 15+ 4 n $--
E→T
9$ E$ 15+ 4 n $
10$ E +$ 15 +4 n $
11$ E + 4$ 15 + 4n $--
F→digit
12$ E + F$ 15 + 4n $--
T→F
13$ E + T$ 15 + 4n $15 + 4
E→E +T
14$ E$ 19n $
15$ E n$ 19 n$print(19)
L→E n
16$ L$ 19$accept

Page 30/63
5. L-Attributed Definitions and 
Translation Schemes
A syntax-directed definition is L-attributed 
if each inherited attribute depends only 
on attributes of itsleft siblings or 
inheritedattributes of its parent. 
⚫Synthesized attributes are supported. 
⚫Can NOT depend on any synthesized attributes 
of its parent!  (Why?)

Page 31/63
Depth-First Evaluation Order
The evaluation order of an L-attributed 
definition
⚫The same as depth-first visiting of the parse 
tree. 
⚫Also the same order as top-down parsing. 
voiddfvisit(n: Node) {
for(each child m of n, from left to right) {
evaluate inherited attributes of m; 
dfvisit(m)
}
evaluate synthesized attributes of n
}

Page 32/63
Translation Schemes
Translation scheme vs. L-attributed 
definition
⚫Explicitevaluation order in a translation 
scheme. 
⚫Perform semantic actions in a left-to-right 
depth-first order. 
L-Attributed Definition
Translation Scheme
Specification
Implementation

Page 33/63
Translation Scheme: Example 1
Postfix translation schemefor an L-
attributed definition: 
L  →E n{print(E.val); }
E  →E
1
+T{ E.val= E
1
.val + T.val; }
E  →T{ E.val= T.val; }
T  →T
1
*F{ T.val= T
1
.val *F.val; }
T  →F{ T.val= F.val; }
F  →(E ){ F.val= E.val; }
F  →digit{ F.val= digit.lexval; }

Page 34/63
Translation Scheme: Example 2
Translation scheme for LR parsing: 
L  →E n{ print(stack[top –1].val); 
top = top –1; }
E  →E
1
+T{ stack[top –2].val = stack[top –2].val + stack[top].val; 
top = top –2; }
E  →T
T  →T
1
*F{ stack[top –2].val = stack[top –2].val * stack[top].val; 
top = top –2; }
T  →F
F  →(E ){ stack[top –2].val = stack[top –1].val; 
top = top –2; }
F  →digit

Page 35/63
Translation Scheme: Example 3
Translation scheme for transformation 
from infix to postfix expressions: 
⚫Actions inside productions. 
E→T R
R→addopT{ print(addop.lexeme); }
R
1
R→
T→num{ print(num.val); }

Page 36/63
Translation Scheme: Example 3 (cont')
E
RT
{ print('–'); }R–T
{ print('+'); }R+T
{ print('2'); }2
{ print('5'); }5
{ print('9'); }9
Parse Tree with Actions

Page 37/63
Translation Scheme: Example 4
Problematic translation scheme for prefix 
expressions: 
⚫In both top-down and bottom-up parsing. 
L→E n
E→{ print('+'); }
E
1
+T
E→T
T→{ print('*'); }
T
1
*F
T→F
F→(E )
F→digit{ print(digit.lexval); }

Page 38/63
Translation Scheme: Example 4 (cont')
L
En
E+T
T
T*F
F
digit
digit
digit
{ print(5); }
{ print(4); }
{ print(3); }
{ print('+'); }
{ print('*'); }
Problems for 
top-down parsing: left-recursion
bottom-up parsing: embedded actions

Page 39/63
From L-Attributed Definitions to 
Translation Schemes
Three transformation rules
1.Inherited attributes of A must be calculated 
before A. 
2.An action must not refer to a synthesized 
attribute of a symbol to the right of the action. 
3.A synthesized attribute must be computed 
after all attributes it references have been 
computed. 
Does not satisfy the requirements:
S→A
1
A
2
{ A
1
.in = 1;  A
2
.in = 2; }
A→a{ print(A.in); }

Page 40/63
Typesetting Boxes: EQNand T
E
X
No.ProductionsSemantic Actions
1
S →B
B.ps = 10
2
B →B
1
B
2
B
1
.ps = B.ps
B
2
.ps = B.ps
B.ht = max(B
1
.ht, B
2
.ht)
B.dp = max(B
1
.dp, B
2
.dp)
3
B →B
1
subB
2
B
1
.ps = B.ps
B
2
.ps = B.ps 70%
B.ht = max(B
1
.ht, B
2
.ht –B.ps 25%)
B.dp = max(B
1
.dp, B
2
.dp + B.ps 25%)
4
B →(B
1
)
B
1
.ps = B.ps
B.ht = B
1
.ht
B.dp = B
1
.dp
5
B →text
B.ht = getHight(B.ps, text.lexval)
B.dp = getDepth(B.ps, text.lexval)
ps = point size
ht = height
dp = depth
Higher precedence, 
right associativity

Page 41/63
From L-Attributed Definition to 
Translation Scheme
S→{ B.ps = 10; }
B
B→{ B
1
.ps = B.ps; }
B
1
{ B
2
.ps = B.ps; }
B
2
{ B.ht = max(B
1
.ht, B
2
.ht); 
B.dp = max(B
1
.dp, B
2
.dp); }
B→{ B
1
.ps = B.ps; }
B
1
sub{ B
2
.ps = B.ps 70%; }
B
2
{ B.ht = max(B
1
.ht, B
2
.ht –B.ps 25%); 
B.dp = max(B
1
.dp, B
2
.dp + B.ps 25%); }
B→({ B
1
.ps = B.ps; }
B
1
){ B.ht = B
1
.ht; 
B.dp = B
1
.dp; }
B→text{ B.ht = getHight(B.ps, text.lexval); 
B.dp = getDepth(B.ps, text.lexval); }

Page 42/63
6. L-Attributed Definitions in Predictive 
Parsing
Development steps
⚫Write a possibleLL(1) grammar for syntax 
rules. 
⚫Define an L-attributed definition by appending 
semantic rules. 
⚫Transform the L-attributed definition to a 
translation scheme. 
⚫Eliminate left-recursionin the translation 
scheme. 
⚫Write a recursive descent predictive parser 
(translator). 
May have 
left-recursions

Page 43/63
Eliminating Left-Recursion: 
A Simple Example
A motivating example: a simple case
⚫Trick: treating actions as terminals if they do 
not calculate any attributes. 
E→E
1
+T { print('+'); }
E→T
E→T R
R→+T { print('+'); } R
R→

Page 44/63
Eliminating Left-Recursion: 
More Examples
A more complex case
E→E
1
+T{ E.val = E
1
.val + T.val; }
E→E
1
–T{ E.val = E
1
.val –T.val; }
E→T{ E.val = T.val; }
T→(E ){ T.val = E.val; }
T→num{ T.val = num.val; }
E→T{ R.i = T.val; }
R{ E.val = R.s; }
R→+T{ R
1
.i = R.i + T.val; }
R
1
{ R.s = R
1
.s; }
R→–T{ R
1
.i = R.i –T.val; }
R
1
{ R.s = R
1
.s; }
R→{ R.s = R.i; }
T→(E ){ T.val = E.val; }
T→num{ T.val = num.val; }

Page 45/63
Eliminating Left-Recursion: 
More Examples (cont')
E
R.i = 9T.val = 9
num.val = 9T.val = 5R.i = 4–
T.val = 2R.i = 6+
num.val = 2
num.val = 5
1
2
3
4
5
6
Evaluation Order of Input Expression 9 –5 + 2

Page 46/63
Eliminating Left-Recursion
General rules
⚫Only available for S-attributed definitions 
(postfix translation schemes). 

Page 47/63
A→A
1
Y { A.a = g(A
1
.a, Y.y); }
A→X{ A.a = f(X.x); }
A→X { R.i = f(X.x); } R { A.a = R.s; }
R→Y { R
1
.i = g(R.i, Y.y); } R
1 
{ R.s = R
1
.s; }
R→{ R.s = R.i; }
A.a = g(g(f(X.x), Y
1
.y), Y
2
.y)
A.a = g(f(X.x), Y
1
.y)Y
2
A.a = f(X.x)Y
1
XR.i = g(g(f(X.x), Y
1
.y), Y
2
.y)
A
R.i = f(X.x)X
R.i = g(f(X.x), Y
1
.y)Y
1

Y
2

Page 48/63
Writing a Predictive Parser
Review: writing a recursive descent 
predictive parser (only for parsing)
⚫Each grammar symbol corresponds to a 
(recursive) subprogram. 
⚫The start symbol corresponds to the main 
entry subprogram. 
⚫In each subprogram, branching actions with 
regard to the lookahead. 

Page 49/63
Writing a Predictive Parser: 
An Example
For parsing only 
R→addopT{ R
1
.i = mknode(addop.lexeme, R.i, T.nptr); }
R
1
{ R.s = R
1
.s; }
R→{ R.s = R.i; }
voidR() {
if(lookahead == addop) {
match(addop);
T();
R()
} else{
// do nothing
}
}

Page 50/63
Writing a Predictive Translator
Writing a translator with semantic actions
⚫Each inherited attribute corresponds to a 
formal parameter. 
⚫All synthesized attributes correspond to the 
return value
Multiple synthesized attributes may be merged 
in a single record. 
⚫Each attribute of the child nodes corresponds 
to a local variable. 
⚫Process the right side of the production
Terminals: match()
Nonterminals: procedure call
Actions: direct execution (copy)

Page 51/63
From a Parser to a Translator
R→addopT{ R
1
.i = mknode(addop.lexeme, R.i, T.nptr); }
R
1
{ R.s = R
1
.s; }
R→{ R.s = R.i; }
SyntaxTreeNodeR(SyntaxTreeNode i) {
SyntaxTreeNode s;  // synthesized attributes 
SyntaxTreeNode t_nptr, r1_i, r1_s;  // for children
char addopLexeme;  // temporary
if(lookahead == addop) {
addopLexeme = lookahead.lexval;
match(addop);
t_nptr = T();
r1_i = mknode(addopLexeme, i, t_nptr);
r1_s = R(r1_i);
s = r1_s; 
} else{
s = i;
}
return s;
}

Page 52/63
7. L-Attributed Definitions in LR Parsing
S-attributed definitions are easy to be 
evaluated by LR parsing. 
⚫See section 4 in these slides. 
What are the challenges for evaluating 
L-attributed definitions in LR parsing? 
1.Not all actions are on the right-most of a 
production body (postfix translation scheme). 
2.Inherited attributes are not stored in the 
parsing stack. 

Page 53/63
Make Use of Tricks
Using markers to move all embedded 
actions to the right-most of a production 
body. 
Tracing inherited attributes in the 
parsing stack. 
1.The simplest case: locating inherited 
attributes which are calculated by copy rules. 
2.Introduce markersto help to locate inherited 
attributes in the stack. 
3.Also make use of new markers to locate 
inherited attributes which are not calculated 
by copy rules. 

Page 54/63
Move Embedded Actions to Right-Most
A marker is an -production that acts as a 
place-holder. 
E→T R
R→+T { print('+'); }R
|–T { print('–'); }R
|
T→num { print(num.val); }
E→T R
R→+T MR
|–T NR
|
T→num{ print(num.val); }
M→{ print('+'); }
N→{ print('–'); }

Page 55/63
Trace a Copied Inherited Attribute
D→T{L.inh = T.type; }
L
T→int{ T.type = INTEGER; }
T→real{ T.type = REAL; }
L→{ L
1
.inh = L.inh; }
L
1
,id{ addType(id.entry, L.inh); }
L→id{ addType(id.entry, L.inh); }
Calculations of inherited attributes are the 
biggest source of embedded actions. 

Page 56/63
Trace a Rewriting Inherited Attribute 
(cont')
D→T{L.inh = T.type; }
L
T→int{ T.type = INTEGER; }
T→real{ T.type = REAL; }
L→{ L
1
.inh = L.inh; }
L
1
,id{ addType(id.entry, L.inh); }
L→id{ addType(id.entry, L.inh); }
ProductionsCode
D →T L
T →int
stack[ntop].val = INTEGER;
T →real
stack[ntop].val = REAL;
L →L
1
,id
addType(stack[top].val, stack[top –3].val);
L →id
addType(stack[top].val, stack[top –1].val);

Page 57/63
Predict Positions of Inherited Attributes
S→aA{C.i = A.s; }
C
S→bA B{ C.i = A.s; }
C
C→c{ C.s = g(C.i); }
S→aA{C.i = A.s; }
C
S→bA B{ M.i = A.s; }
M{ C.i = M.s; }
C
C→c{ C.s = g(C.i); }
M→{ M.s = M.i; }
Introduce new markers

Page 58/63
Store Calculated Inherited Attributes
S→aA{C.i = f(A.s); }
C
C→c{ C.s = g(C.i); }
S→aA{ M.i = A.s; }
M{ C.i = M.s; }
C
C→c{ C.s = g(C.i); }
M→{ M.s = f(M.i); }
Also make use of new markers

Page 59/63
A Practical Example
ProductionsSemantic Actions
S →KBB.ps = K.s
K →K.s = 10
B →B
1
LB
2
B
1
.ps = B.ps
L.i = B.ps
B
2
.ps = L.s
B.ht = max(B
1
.ht, B
2
.ht)
B.dp = max(B
1
.dp, B
2
.dp)
L →L.s = L.i
B →B
1
subMB
2
B
1
.ps = B.ps
M.i = B.ps
B
2
.ps = M.s
B.ht = max(B
1
.ht, B
2
.ht –B.ps25%)
B.dp = max(B
1
.dp, B
2
.dp + B.ps25%)
M →M.s = M.i70%
B →(NB
1
)
B
1
.ps = B.ps
N.i = B.ps
B.ht = B
1
.ht
B.dp = B
1
.dp
N →N.s = N.i
B →text
B.ht = getHight(B.ps, text.lexval)
B.dp = getDepth(B.ps, text.lexval)
Anytime when B is reduced, 
B.psis immediately under B

Page 60/63
A Practical Example (cont')
ProductionsCode
S →K B
K →
stack[ntop].ps = 10;
B →B
1
L B
2
stack[ntop].ht = max(stack[top –2].ht, stack[top].ht);
stack[ntop].dp = max(stack[top –2].dp, stack[top].dp);
L →
stack[ntop].ps = stack[top –1].ps;
B →B
1
subM B
2
stack[ntop].ht = max(stack[top –3].ht, stack[top].ht –stack[top –4].ps 25%); 
stack[ntop].dp = max(stack[top –3].dp, stack[top].dp + stack[top –4].ps 25%); 
M →
stack[ntop].ps = stack[top –2].ps 70%
B →(N B
1
)
stack[ntop].ht = stack[top –1].ht
stack[ntop].dp = stack[top –1].dp
N →
stack[ntop].ps = stack[top –1].ps;
B →text
stack[ntop].ht = getHight(stack[top –1].ps, stack[top].lexval); 
stack[ntop].dp = getDepth(stack[top –1].ps, stack[top].lexval); 
Remove all calculations for 
inherited attributes
ps is treated as a 
synthesized attribute

Page 61/63
Exercise 8.1
Given the translation scheme for the EQN 
language (see pp.40 in this lecture), 
calculate the height and depth of the 
input: text sub text sub text. 
⚫Suppose that for each text, 
getHeight(ps, text.lexval) = 8 * ps
getDepth(ps, text.lexval) = 0

Page 62/63
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 5.1-5.3 on introduction to syntax-
directed translation. 
Section 5.4-5.5 on the implementation of 
translation schemes in top-down and bottom-
up LR parsing. 
⚫Skip Reading: 
DBv1 Section 5.8-5.10 on advanced 
implementation of translation schemes. 

Page 63/63
Enjoy the Course!
