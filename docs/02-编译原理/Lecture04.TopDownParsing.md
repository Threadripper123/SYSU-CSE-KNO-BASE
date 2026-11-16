# Lecture04.TopDownParsing

> 主题库：编译原理　｜　源文件：Lecture04.TopDownParsing.pdf　｜　18,056 字符　｜　79 个片段　｜　更新于 2026-11-11


Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
©Copyright 2008-2022, Sun Yat-senUniversity

Page 2/79
Lecture 4. Top-Down Parsing
1.Introduction to Parsing
2.Top-Down Parsing
3.Rewriting Grammars
4.Top-Down Parser with Backtracking
5.ToBePredictive
6.Recursive Descent Predictive Parser

Page 3/79
1. Introduction to Parsing
Parser: input, process, and output (IPO)
Parser
stream of 
tokens
parse
tree
Two questions must be answered. 
Logical vs. PhysicalLogical vs. Physical

Page 4/79
Structure of a Parser
Implicit syntactic rules
Specific
Parser
stream of 
tokens
parse
tree
syntactic
rules
Specific to some predefined language. 

Page 5/79
Structure of a Parser (cont')
Explicit syntactic rules
(interpretation model)
General
Parser
stream of 
tokens
parse
tree
syntactic
rules
No hard-coding of language-specific code.

Page 6/79
Structure of a Parser (cont')
Explicit syntactic rules
(compilation model)
Specific
Parser
stream of 
tokens
parse
tree
Generator
(e.g. yacc & JavaCUP)
syntactic 
rules

Page 7/79
Review
The pipe between a parser and a scanner
⚫Method invocation (procedure call)
⚫Logical vs. physical
Context-free grammars
⚫Parse tree, derivation, and reduction
⚫Ambiguity

Page 8/79
Capability of Context-Freedom
What languages it can generate? 
⚫L
0
= {a
n 
b
n
| n 1}
Abstraction of some problem in practice
What languages it can not generate? 
⚫L
1
= {c | (a | b)
*
a, b, c }
Abstraction of some problem in practice
How to solve the problem? 
⚫L
2
= {a
n 
b
m
c
n
d
m
| n 1 m 1}
Abstraction of some problem in practice
How to solve the problem? 

Page 9/79
2. Top-Down Parsing
Parsing strategies
⚫Top-down parsing
How to choose a unique production in multiple 
candidates? 
⚫Bottom-up parsing
How to find the handle in a sentential form? 

Page 10/79
Top-Down Parsing: Motivation
A motivating example
type→simple
|^  id
|array[  simple]  oftype
simple→integer
|char
|numdotdotnum
⚫array[1..10] ofinteger

Page 11/79
Parsing Process (Initial)
array[numintegerdotdotnum]of
type
Derive with "type →array [ simple] of  type"

Page 12/79
Parsing Process (Action 1)
array[numintegerdotdotnum]of
type
simple][arrayoftype
Match !

Page 13/79
Parsing Process (Action 2)
array[numintegerdotdotnum]of
type
simple][arrayoftype
Match !

Page 14/79
Parsing Process (Action 3)
array[numintegerdotdotnum]of
type
simple][arrayoftype
Derive with "simple →num dotdot num"

Page 15/79
Parsing Process (Action 4)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Match !

Page 16/79
Parsing Process (Action 5)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Match !

Page 17/79
Parsing Process (Action 6)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Match !

Page 18/79
Parsing Process (Action 7)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Match !

Page 19/79
Parsing Process (Action 8)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Match !

Page 20/79
Parsing Process (Action 9)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnum
Derive with "type →simple"

Page 21/79
Parsing Process (Action 10)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnumsimple
Derive with "simple →integer"

Page 22/79
Parsing Process (Action 11)
array[numintegerdotdotnum]of
type
simple][arrayoftype
dotdotnumnumsimple
integer
Match and Accept !

Page 23/79
Top-Down Parsers: Perspectives
Perspective 1: 
parsing capability vs. efficiency
⚫Top-down parser with backtracking
⚫Predictive parser
Two orthogonalperspectives

Page 24/79
Top-Down Parsers: Perspectives (cont')
Perspective 2: 
parser implementation
⚫Recursive descent parser
A parser with backtracking
A predictive parser
⚫Table-driven parser
Non-recursive programs with an explicit 
stack and a parsing table. 
An approach to automation (usually 
predictive)

Page 25/79
3. Rewriting Grammars
Resolving ambiguities
⚫Trade-off and consequence
Elimination of -productions
⚫Systematic elimination of left recursions
Elimination of left recursions
⚫Avoid infinite loop in top-down parsing
Left-factoring
⚫Avoid backtracking in top-down parsing
Grammar
Transformation
Bottom-up
Predicted
Predicted


Page 26/79
Resolving Ambiguities
Review: ambiguities in practice
⚫Expression
⚫Dangling-else
Resolving ambiguities
⚫Ad hoc constraints
⚫Trade-off and consequence

Page 27/79
Ambiguous Expressions
Ambiguous grammar
expr→expr+expr
|expr *expr
|(expr )|  n
Unambiguous grammar
expr→expr+term|  term
term→term*factor|  factor
factor→(expr)|  n

Page 28/79
Rewriting Rules
Ad hoc but heuristic rewriting rules
⚫Rules for precedence
⚫Rules for associativity

Page 29/79
Dangling-elseProblem
Grammar
stmt→ifexprthen  stmt
|ifexprthen  stmtelse  stmt
|other
Example
if E
1
then S
1
elseif E
2
then S
2
else S
3
Ambiguity
ifE
1
thenif E
2
then S
1
elseS
2
ifE
1
thenif E
2
then S
1
elseS
2

Page 30/79
Unambiguous Grammar
Additional disambiguation rule
⚫Each elseis matched with the closest unmatched then. 
Unambiguous grammar
stmt→matched_stmt|  open_stmt
matched_stmt→ifexprthenmatched_stmtelsematched_stmt
|other
open_stmt→ifexprthenstmt
|ifexprthenmatched_stmtelseopen_stmt
Example
ifE
1
thenif E
2
then S
1
elseS
2

Page 31/79
Eliminating -Productions
An -free grammar
⚫No production body is (-production), or
⚫The only -production is S →, and S does not 
appear in the body of any productions. 
Elimination algorithm
⚫For every production A →X
1
X
2
... X
n
, where 
X
i
N, 1 i n
⚫Add new productions A →a
1
a
2
... a
n
, where
(X
i

*
) (a
i
= X
i
)
(X
i

*
) (a
i
= X
i
a
i
= )
1 i n. a
i

Some textbook defines –free 
grammar with only the first restriction

Page 32/79
Eliminating -Productions: Example 1
Original grammar
S→A a|  b
A→A c|  S d|  
Rewriting grammar
S→A a|  a|  b
A→A c|  c|  S d

Page 33/79
Eliminating -Productions: Example 2
Original grammar
S→aS bS  |  bS aS  |  
Equivalent -free grammar
S'→S  |  
S→aS bS  |  abS|  aS b|  ab
|bS aS  |  baS|  bS a|  ba
Augmented grammar

Page 34/79
Theorem on –Free Grammars
Given any context-free grammar G, there 
exists an -free grammar G', so that 
L(G) –{} = L(G')
⚫L(G) L(G) = L(G')
⚫The only difference between G and G' is the 
productions. 

Page 35/79
Eliminating Left Recursions
Simple immediate left recursion
A→A |  
A→A'
A'→A'  |  
Elimination algorithm
A→A
1
|  A
2
|  ...  |  A
m
|
1
|  
2
|  ...  |  
n
A→
1
A'  |  
2
A'  |  ...  |  
n
A' 
A'→
1
A'  |  
2
A'  |  ...  |  
m
A'  |  
From left recursion
to right recursion

Page 36/79
Immediate Left Recursion: Example
Original grammar
E→E +T  |  T
T→T *F  |  F
F→(E )|  n
Equivalent grammar without left 
recursions
E→T E'
E'→+T E'  |  
T→F T'
T'→* F T'  |  
F→(E )|  n

Page 37/79
Systematic Elimination
Preconditions
⚫No cycles, e.g. A 
+
A
⚫No -productions, e.g. A →
Elimination algorithm
Arrange the nonterminalsin some order A
1
, A
2
, ..., A
n
. 
for i= 1 to ndo begin
for j= 1 toi–1 do begin
Replace each production of the form A
i
→A
j

by the production A
i
→
1
| 
2
| ... | 
k
, 
where A
j
→
1
| 
2
| ... | 
k
are all current A
j
-productions
end
Eliminate the immediate left recursion among the A
i
-productions
end
Why? 

Page 38/79
Systematic Elimination: Example 1
Original grammar
S→A a|  b
A→A c|  S d|  
Rewriting grammar
⚫Eliminating -productions:
S→A a|  a|  b
A→A c|  c|  S d
⚫Eliminating left recursions, ordered by S, A: 
S→A a|  a|  b
A→A c|  c| A ad|  ad|  bd
A→c A'  |  ad A'  |  bdA'
A'→c A'  |  ad A'  |  

Page 39/79
Systematic Elimination: Example 2
Original grammar
S→A c|  c
A→B b|  b
B→S a|  a
Rewriting with different order
Ordered by S, A, B:
S→A c|  c
A→B b|  b
B→A ca|  ca|  a
B→B bca|  bca|  ca|  a
B→b c a B'  |  ca B'  |  a B'
B'→b c a B'  |  
Ordered by B, A, S: 
B→S a|  a
A→S ab|  ab|  b
S→S abc|  abc|  bc|  c
S→a b c S'  |  bc S'  |  c S'
S'→a b c S'  |  
Equivalent

Page 40/79
Left Factoring
Simple left factoring
A→
1
|  
2
A→A'
A'→
1
|  
2
Left factoring algorithm
A→
1
|  
2
|  ...  |  
n
|  
A→A'  |  
A'→
1
|  
2
|  ...  |  
n

Page 41/79
Left Factoring: Example
Original grammar
S→ifE  then S|  if  E  then  Selse  S  |  other
E→bool
After left factoring
S→ifE  then S S'  |  other
S'→else  S  |  
E→bool

Page 42/79
Conclusions: Why Rewriting?
For all parsing techniques
⚫Resolving ambiguities: why? 
Only for top-down parsing
⚫Eliminating -productions: why? 
⚫Eliminating left recursions: why? 
⚫Left factoring: why? 

Page 43/79
4. Top-down Parser with Backtracking
Trade-off and consequence
⚫Pros: powerful to handle most CFG. 
⚫Cons: complex, and low efficiency. 
Only used to demonstrate the idea 
of top-down parsing 
⚫Why a left recursion leads to infinite 
loop? 
⚫The meaning of the first symbol that a 
nonterminal can derive. 

Page 44/79
Making Decisions on Actions
Given the following grammar
S→xA x
A→ab|  a|  b
Parsing with only one lookahead
⚫Sentence: xax
Predictive if  there are 2 
lookaheads
S
Axx
[S, x]
S
Axx
ab
[A, a]
S
Axx
a
[A, a]
Decision 
Making

Page 45/79
Decision on Actions (cont')
Given the following grammar
S→bA S  |  a
A→aA  |  
Backtracking caused by an -production
⚫Sentence: ba
Decision 
Making
S
ASb
[S, b]
S
ASb
a
[A, a]
A
S
ASb

[A, a]
a

Page 46/79
Four Actions in Top-Down Parsing
General actions
⚫Accept
⚫Error
Actions specific to top-down parsing
⚫match
⚫derive

Page 47/79
5. To BePredictive
How can a parser be predictive? 
⚫Sufficient and necessary conditions for the 
grammar. 
⚫How to select a unique production candidate 
with regard to the lookahead. 

Page 48/79
FIRST() and FOLLOW()
Purposes of these two functions
⚫aFIRST(A) and bFOLLOW(A) 
S
Ab

a

Page 49/79
Function FIRST()
Intent: while choosing one of A →| 
with regard to the lookahead x, we must 
have FIRST() FIRST() = . 
Syntax (signature)
⚫FIRST: (N)
*
→2
{}
Semantics
⚫{x| 
*
x..., x} FIRST()
⚫
*
FIRST()
Why do we permit in FIRST()? 

Page 50/79
Function FIRST() (cont')
Given X N, we have FIRST(X)
⚫X FIRST(X) = {X}
⚫X N X →Y
1
Y
2
...Y
n

1 in. aFIRST(Y
i
) 
1 ji–1. FIRST(Y
j
)  
aFIRST(X)
1 kn. FIRST(Y
k
)  
FIRST(X)
⚫So we haveX →FIRST(X)

Page 51/79
Function FIRST() (cont')
Given = X
1
X
2
...X
n
(N)
*
, we have 
FIRST()
⚫FIRST(X
1
) –{} FIRST()
⚫2 in. 1 ji–1. FIRST(X
j
)  
FIRST(X
i
) –{} FIRST()
⚫1 in. FIRST(X
i
)  FIRST()
Overloading

Page 52/79
Function FOLLOW()
Intent: while choosing one of A →| 
with regard to the lookahead x, we must 
have FIRST() FOLLOW(A) = . 
Syntax (signature)
⚫FOLLOW: N →2
{$}
, where a special symbol $
indicates the end of input token stream. 
Semantics
⚫{x| S 
*
Axx} FOLLOW(A) 
⚫S 
*
A  $FOLLOW(A)

Page 53/79
Function FOLLOW() (cont')
Given X N, we have FOLLOW(X)
⚫X = S  $FOLLOW(X)
⚫A →XFIRST() –{}FOLLOW(X)
⚫(A →X) (A →XFIRST())  
FOLLOW(A)FOLLOW(X)

Page 54/79
Example 1
Given the following grammar
E→T E'
E'→+T E'  |  
T→F T'
T'→* F T'  |  
F→(E )|  n
We have
⚫FIRST(F) = FIRST(T) = FIRST(E) = {(, n}
⚫FIRST(T') = {*, }
⚫FIRST(E') = {+, }
⚫FOLLOW(E) = FOLLOW(E') = {), $}
⚫FOLLOW(T) = FOLLOW(T') = {+, ), $}
⚫FOLLOW(F) = {+, *, ), $}

Page 55/79
Example 2
Given the following grammar
A→B C
B→A x|  x|  
C→yC  |  y
We have
SymbolFIRSTFOLLOW
Ax  yx  $
B
x  y  
y
Cyx  $
Heuristics in manual calculation: 
Leftvs. right
Top-down vs. bottom-up

Page 56/79
Conditions for Predictive Parsing
Sufficient and necessary conditions
CFG G is LL(1) iif whenever A →| are 
two distinct productions of G, the 
following conditions hold
⚫FIRST() FIRST() = 
⚫FIRST()  
FIRST() FOLLOW(A) = 

Page 57/79
Discussions
What benefits from the LL(1) conditions? 
⚫Decision of the derive/error action in the 
parser with regard to the current nonterminal 
and a single lookahead. 
⚫Only 0 or 1 production will be chosen. 
They are not predictive (why?)
⚫Ambiguous grammars
⚫Grammars with left recursions
⚫Grammars with left factors

Page 58/79
6. Recursive Descent Predictive Parser
An approach suitable for manual 
development of a top-down parser. 

Page 59/79
Development Phrases
Development of a recursive descent 
predictive parser
1.Resolve ambiguities in the grammar. 
2.Eliminate left recursions in the grammar. 
3.Left factor the grammar. 
4.Construct transition diagrams from the 
grammar. 
5.Reduce the transition diagrams (ad hoc and 
optional). 
6.Write the parser using the transition diagrams 
as blue print. 

Page 60/79
Transition Diagram
Visualize the predictive parser
⚫For each nonterminal A
Create an initial and final state. 
For each A →X
1
X
2
...X
n
, create a path 
from the initial state to the final state, 
with edges labeled X
1
, X
2
, ..., X
n 
.
⚫If A →, the path is labeled . 

Page 61/79
Transition Diagram: Example
Given the following grammar
E→T E'E'→+T E'  |  
T→F T'T'→* F T'  |  
F→(E )|  n
Transform to transition diagrams
012
E:
TE'
789
T:
FT'
346
E':
+E'
5
T

101113
T':
*T'
12
F

141517
F:
()
16
E
n

Page 62/79
Reduction of Transition Diagrams
Ad hoc rules: iterative substitution
34
6
E':
+
T

346
E':
+E'
5
T

34
6
E':
+
5
T



Page 63/79
Reduction of Transition Diagrams (cont')
T
36
+

0
E:
34
6
+
T

0
E:
T
012
E:
TE'

Page 64/79
Reduced Transition Diagrams
T
36
+

0
E:
F
1013
*

7
T:
141517
F:
()
16
E
n

Page 65/79
Discussion
Transition diagrams: (recursive descent 
predictive) parsing vs. scanning
⚫Point 1
⚫Point 2

Page 66/79
Write a Recursive Predictive Parser
Use the transition diagrams as the blue-
print of the parser
⚫Write a recursive procedure for each diagram. 
The procedure for the diagram of S is the 
main entry. 
⚫Design the control flow of the procedure 
mimicking the paths in the diagram, with 
regard to the lookahead. 
If the label of an edge in the path is a 
terminal, perform the matchaction. 
If the label is a nonterminal, perform the 
deriveaction, that is invoking the procedure 
of the nonterminal (recursively).

Page 67/79
Coding Rules
A  →a B b
voidA() {
match(a);
B();
match(b);
}
voidmatch(Token tok) {
if(lookahead == tok) {
lookahead = scanner.getNextToken();
} elseerror();
}

Page 68/79
Coding Rules (cont')
A  →a B b  |  b A B
voidA() {
if(lookahead == a) {
match(a);  B();  match(b)
} else if(lookahead == b) {
match(b);  A();  B()
}else  error()
}

Page 69/79
Coding Rules (cont')
A  →a B b  |  b A B  |  C
voidA() {
if(lookahead == a) {
match(a);  B();  match(b)
} else if(lookahead == b) {
match(b);  A();  B()
}else  C();
}
voidA() {
if(lookahead == a) {
match(a);  B();  match(b)
} else if(lookahead == b) {
match(b);  A();  B()
} else if (lookahead inFIRST(C)) {
C()
}elseerror();
}

Page 70/79
Coding Rules (cont')
A  →a B b|  b A B  |  
voidA() {
if(lookahead == a) {
match(a);  B();  match(b)
} else if (lookahead == b) {
match(b);  A();  B()
} else;    // do nothing
}
voidA() {
if(lookahead == a) {
match(a);  B();  match(b)
} else if (lookahead == b) {
match(b);  A();  B()
} else if(lookahead inFOLLOW(A)) {
// do nothing, more accurate!
} elseerror();
}

Page 71/79
Example 1
voidtype() throws SyntacticException{
if(lookahead.equals(newToken('^'))) {
match(newToken('^'));
match(newToken(Token.ID));
} elseif(lookahead.equals(newToken(Token.ARRAY))) {
match(newToken(Token.ARRAY));
match(newToken('['));
simple();
match(newToken(']'));
match(newToken(Token.OF));
type();
} elsesimple();
}
type→simple
|^  id
|array[  simple  ]  oftype
simple→integer
|char
|numdotdotnum
Coding is direct and intuitive while using 
unreduced transition diagrams as a blueprint. 

Page 72/79
Example 1 (cont')
voidsimple() throws SyntacticException{
if(lookahead.equals(newToken(Token.INTEGER))) {
match(newToken(Token.INTEGER));
} elseif(lookahead.equals(newToken(Token.CHAR))) {
match(newToken(Token.CHAR));
} elseif(lookahead.equals(newToken(Token.NUM))) {
match(newToken(Token.NUM));
match(newToken(Token.DOTDOT));
match(newToken(Token.NUM));
} elsethrow newSyntacticException();
}
voidmatch(Token tok) throws SyntacticException{
if(lookahead.equals(tok))
lookahead = scanner.getNextToken();
elsethrow newSyntacticException();
}
type→simple
|^  id
|array[  simple  ]  oftype
simple→integer
|char
|numdotdotnum

Page 73/79
Example 2
voidexpr() throws SyntacticException{
term();
while(lookahead.equals(newToken('+'))){
match(newToken('+'));
term();
}
}
voidterm() throws SyntacticException{
factor();
while(lookahead.equals(newToken('*'))){
match(newToken('*'));
factor();
}
}
T
36
+

0
E:
F
1013
*

7
T:
141517
F:
()
16
E
n
Some tricks are needed while coding with 
reduced transition diagrams. 

Page 74/79
Example 2 (cont')
voidfactor() throws SyntacticException{
if(lookahead.equals(newToken('('))) {
match(newToken('('));
expr();
match(newToken(')'));
} else if(lookahead.equals(newToken(Token.NUM))) {
match(newToken(Token.NUM));
} else{
throw newSyntacticException();
}
}
T
36
+

0
E:
F
1013
*

7
T:
141517
F:
()
16
E
n

Page 75/79
Exercise 4.1
Given the following grammar
S→(L )|  a
L→L , S  |  S
⚫Eliminate left recursions in the grammar. 
⚫Draw the transition diagrams for the grammar. 
⚫Write a recursive descent predictive parser. 
⚫Indicate the procedure call sequence for an input 
sentence (a, (a, a)). 

Page 76/79
Exercise 4.2
Consider the context-free grammar
S→aS bS  |  bS aS  |  
⚫Can you construct a predictive parser for the 
grammar? and why? 

Page 77/79
Exercise 4.3
Compute the FIRST and FOLLOW for the 
start symbol of the following grammar
S→S S +|  S S *|  a

Page 78/79
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 2.4, 4.1.1–4.1.2 and 4.4.1 for the 
introduction to top-down parsing. 
Section 4.2 and 4.3 for context-free grammar 
and grammar transformations. 
Section 4.4.2 for function FIRST and FOLLOW. 
⚫Skip Reading: 
Section 4.1.3–4.1.4 for error recovery in top-
down parsing. 

Page 79/79
Enjoy the Course!
