# Lecture07.LR-Parsing

> 主题库：编译原理　｜　源文件：Lecture07.LR-Parsing.pdf　｜　22,999 字符　｜　91 个片段　｜　更新于 2026-11-11


©Copyright 2008-2022, Sun Yat-senUniversity
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
Principles of 
Compiler Construction

Page 2/91
Lecture 7.  LR Parsing
1.Introduction to LR Parsing
2.Motivation of LR Parsing
3.Simple LR Parsing
LR(0) and SLR(1)
4.More Powerful LR Parsing
LR(1) and LALR(1)
5.Ambiguity in LR Parsing
6.Error Recovery in LR Parsing

Page 3/91
1. Introduction to LR Parsing
Proposed by
⚫D. Knuth (Stanford U.). On the Translation of Languages from 
Left to Right. Information and Control, 8(6), 1965, pp.607-639
Prof. 高德纳: The Art of Computer Programming(TAOCP), T
E
X, 
Literate Programming, LR Parsing, Attribute Grammar, etc. 
Pros
⚫Recognize almost all practical CFGs (more 
powerful than LL parsing). 
⚫High efficiency. 
⚫Detect errors as soon as possible. 
Cons
⚫A large size of parsing table. 
⚫Hard to construct the parsing table manually. 

Page 4/91
2. Motivation of LR Parsing
Critical: shift-reduce decision making
⚫Maintain states to keep track of where we are 
in the parsing procedure. 
States are represented by items
⚫A  →X Y Zyields four items (LR(0) items)
A  →•X Y Z
A  →X •Y Z
A  →X Y •Z
A  →X Y Z •
⚫A  →yields only one item: A  →•

Page 5/91
Items
Different forms of items
⚫A  →X •a Z
A “shift”item
Indicates the shift of symbol a. 
⚫A  →X •B Z
A “reduce-expected”item. 
Indicates the expectation of reducing a B from the 
remaining input string. 
⚫A  →X Y Z •
A “reduce”item
Indicates a handle has appeared on top of stack. 
⚫S  →X Y Z •
An “accept”item (special form of reduce items)
Indicates the state of accepting the input. 

Page 6/91
Augmented Grammar
How and why of augmented grammars? 
⚫Add a new start symbol S'
S'  →S
S  →...
⚫Intent of augmenting a grammar
The start symbol will never appear in the 
body of any productions. 
Then the accepting state (items) is unique: 
S'  →S •

Page 7/91
A Motivating Example
Consider the following grammar
S'→S
S→aA  |  bB
A→cA  |  d
B→cB  |  d

Page 8/91
S' →•S
I
0
Initial state

Page 9/91
S' →•S
S→•aA
S→•bB
I
0
Equivalent closure

Page 10/91
S' →•S
S→•aA
S→•bB
I
0
Equivalent closure
Kernel 
items
Nonkernel items or 
closure items

Page 11/91
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S
If the remaining string
can be reduced to S
(expected!)

Page 12/91
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
I
2
S
a
If the first symbol of 
remaining string is a
(shift!)

Page 13/91
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S
a
Equivalent closure in 
state I
2

Page 14/91
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
If the first symbol of 
remaining string is b
(shift!)

Page 15/91
All possible states
from state I
2
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
A→d•
I
5
d
S→aA •
I
4
A

Page 16/91
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
All possible states
from state I
6

Page 17/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B
d
All possible states
from state I
3

Page 18/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
All possible states, 
recognized by a DFA

Page 19/91
Working with the DFA
Consider the following sentence:
a c c d
We have the right-most derivation:
S' Sa A a cA a c c A a c c d

Page 20/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
| a c c d
( initialize )

Page 21/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
a | c c d
( shift )

Page 22/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
a c | c d
( shift )

Page 23/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
a c c | d
( shift )

Page 24/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
a c c d|
( shift )

Page 25/91
Input string
a c c A|
( reduce with A →d)
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Expectation: the dream comes true! 

Page 26/91
Input string
a c A|
( reduce with A →cA )
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c

Page 27/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
a A|
( reduce with A →cA )

Page 28/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Input string
S|
( reduce with S →aA 
and accept)

Page 29/91
Parsing Decisions
Step
SymbolStateInputReferenceActionOutput
1$0a c c d $
S→•aA
shift
2$ a0 2c c d $
A→•cA
shift
3$ a c0 2 6c d $
A→•cA
shift
4$ a c c0 2 6 6d $
A→•d
shift
5$ a c c d0 2 6 6 5$
A→d•
reduce
A→d
6$ a c c A0 2 6 6 7$
A→cA •
reduce
A→cA
7$ a c A0 2 6 7$
A→cA •
reduce
A→cA
8$ a A0 2 4$
S→aA •
reduce
S→aA
9$ S0 1$
S'→S •
accept

Page 30/91
Review
Implementations of the abstract 
model for shift-reduce parsing
SLR(1) ParserLR(0) ParserLALR(1) ParserLR(1) Parser
OPP ParserLR Parser
Shift-Reduce Parser

Page 31/91
LR Parsers
Concrete implementations
⚫Parsing table
ACTION
GOTO
⚫Explicit stack
States
Grammar symbols (optional)
⚫Reducable substring
Handles

Page 32/91
A Concrete Model for LR Parser
LR Parser
LR Parser
Input
Output
Logical vs. Physical
+n*n$n
lookahead
0
2
4
top
Stack
search with [top, lookahead]
$
a
A
ACTIONGOTO

Page 33/91
What Language the DFA recognizes? 
Viable prefixes of the grammar
⚫All strings of grammar symbols that can 
appear on the stack. 
⚫A viable prefixis a prefix of  a right-sentential form, 
that does not continue past the right end of  the right-most 
handle of  that sentential form. (DBv2, pp.256, seg.4, 
line.2-4)
Learn how to build 
up a formulation! 

Page 34/91
Properties of Viable Prefixes
A viable prefix must be a prefix of a right-
sentential form. 
⚫S 
*
rm
, where is the content of the stack 
and contains no nonterminals. 
Not all prefixes of a right-sentential form 
are viable prefixes. 
⚫E
*
rm
F *n
rm
(E) * n, 
⚫where (, ( E, ( E )are viable prefixes, 
⚫but ( E ) *is not, since the parser will perform 
reduction once the handle appears. 

Page 35/91
Definition: Valid Item
The definition of a valid item
⚫Item A →
1
•
2
is valid for a viable prefix 
1
, 
if there exists a derivation 
S' 
*
rm
A 
rm

1

2

where contains terminals only. 
⚫Hints provided by valid items
If 
2
, the parser should do shift()since a 
handle has not appear on top of parsing stack.
If 
2
= , the parser should do reduce().

Page 36/91
Theorem 1 on Valid Items
Foundation of closure(), which is used to 
construct the states of DFA. 
⚫If A →
1
•B 
2
is a valid item for 
1
, 
and B →is a production, 
then B →•is also a valid item for 
1
.
⚫[Proof]
S'
*
rm
A 
rm

1
B 
2
(definition)

*
rm

1
B (suppose 
2

*
rm
)

rm

1
(we have B →)
B →•is a valid item for 
1
(definition)

Page 37/91
Theorem 2 on Valid Items
Foundation of goto(), which is used to 
construct the transitions of DFA. 
⚫If A →
1
•X 
2
is a valid item for 
1
, 
then A →
1
X •
2
is a valid item for 
1
X.
⚫[Proof]
S' 
*
rm
A 
rm

1
X 
2
(definition)
S' 
*
rm
A 
rm

1
X
2
(definition)
A →
1
X •
2
is valid for 
1
X(definition)

Page 38/91
3. Simple LR Parsing
Steps of parsing table construction
⚫Augment the grammar. 
To ensure a unique accepting state. 
⚫Draw the DFA recognizing all viable prefixes of 
the grammar. 
⚫Calculate FIRST() and FOLLOW() sets of all 
nonterminal symbols. 
Or on-demand calculating while filling the table. 
⚫Fill the LR parsing table (ACTION and GOTO). 
Using lookahead to decide reductions. 

Page 39/91
Augmented Grammar
Given the previous example augmented 
and numbered:
(0)S'→S
(1)S→aA
(2)S→bB
(3)A→cA
(4)A→d
(5)B→cB
(6)B→d

Page 40/91
FIRST() and FOLLOW() Sets
We have 
⚫FIRST(S) = { a, b }
⚫FIRST(A) = { c, d }
⚫FIRST(B) = { c, d }
⚫FOLLOW(S) = { $ }
⚫FOLLOW(A) = { $ }
⚫FOLLOW(B) = { $ }

Page 41/91
c
c
S' →•S
S→•aA
S→•bB
I
0
S' →S •
I
1
S→a•A
A→•cA
A→•d
I
2
S→b•B
B→•cB
B→•d
I
3
S
a
b
A→c •A
A→•cA
A→•d
I
6
c
S→aA •
I
4
A
A→d•
I
5
d
d
A→cA •
I
7
A
B→c•B
B→•cB
B→•d
I
10
S→bB •
I
8
B→d •
I
9
B→cB •
I
11
B
d
d
B
c
Recall: DFA recognizing all viable prefixes

Page 42/91
LR(0) Parsing Table
State
ACTIONGOTO
abcd$SAB
0s2s31
1acc
2s6s54
3s10s98
4r1r1r1r1r1
5r4r4r4r4r4
6s6s57
7r3r3r3r3r3
8r2r2r2r2r2
9r6r6r6r6r6
10s10s911
11r5r5r5r5r5
LR(0) uses 0lookahead to decide reducing

Page 43/91
Overlap of Two Tables
State
ACTIONGOTO
abcd$SAB
0s2s31
1acc
2s6s54
3s10s98
4r1r1r1r1r1
5r4r4r4r4r4
6s6s57
7r3r3r3r3r3
8r2r2r2r2r2
9r6r6r6r6r6
10s10s911
11r5r5r5r5r5

Page 44/91
Overlap of Two Tables (cont')
State
ACTIONGOTO
abcd$SAB
0s2s31
1acc
2s6s54
3s10s98
4r1r1r1r1r1
5r4r4r4r4r4
6s6s57
7r3r3r3r3r3
8r2r2r2r2r2
9r6r6r6r6r6
10s10s911
11r5r5r5r5r5

Page 45/91
Decisions Based on Parsing Table
Step
SymbolStateInputReferenceActionOutput
1$0a c c d $a[0, a] = s2shift
2$ a0 2c c d $a[2, c] = s6shift
3$ a c0 2 6c d $a[6, c] = s6shift
4$ a c c0 2 6 6d $a[6, d] = s5shift
5$ a c c d0 2 6 6 5$
a[5, $] = r4
g[6, A] = 7
reduce
A→d
6$ a c c A0 2 6 6 7$
a[7, $] = r3
g[6, A] = 7
reduce
A→cA
7$ a c A0 2 6 7$
a[7, $] = r3
g[2, A] = 4
reduce
A→cA
8$ a A0 2 4$
a[4, $] = r1
g[0, S] = 1
reduce
S→aA
9$ S0 1$a[1, $] = accaccept

Page 46/91
Simple LR (SLR) Parsing Table
State
ACTIONGOTO
abcd$SAB
0s2s31
1acc
2s6s54
3s10s98
4r1
5r4
6s6s57
7r3
8r2
9r6
10s10s911
11r5
SLR(1)simply uses 1lookahead to decide reducing
( Lookaheads must be in the FOLLOWset of  the reduced symbol )

Page 47/91
Difference between LR(0) and SLR(1)
Step
Symbol
StateInputReferenceActionOutput
1$0a d d $a[0, a] = s2shift
2$ a0 2d d $a[2, d] = s5shift
3$ a d0 2 5d $a[5, d] = r4, g[2, A] = 4reduce
A→d
4$ a A0 2 4d $a[4, d] = r1, g[0, S] = 1reduce
S→aA
5$ S0 1d $a[1, d] = emptyerror
Step
Symbol
StateInputReferenceActionOutput
1$0a d d $a[0, a] = s2shift
2$ a0 2d d $a[2, d] = s5shift
3$ a d0 2 5d $a[5, d] = emptyerror
Parsing with the LR(0) parsing table
Parsing with the SLR(1) parsing table

Page 48/91
A More Complicated Example
Consider the unambiguousgrammar for 
expressions
E→E +T  |  T
T→T *F  |  F
F→(E )|  id
Augmented grammar (numbered)
(0)E'→E
(1)E→E +T
(2)E→T
(3)T→T *F
(4)T→F
(5)F→(E )
(6)F→id

Page 49/91
DFA recognizing all viable prefixes
(
id
T
E' →•E
E→•E +T
E→•T
T→•T *F
T→•F
F→•(E )
F→•id
I
0
F→( •E )
E→•E +T
E→•T
T→•T *F
T→•F
F→•(E )
F→•id
I
4
E→E +•T
T→•T *F
T→•F
F→•(E )
F→•id
I
1
E'→E •
E→E •+T
I
6
I
9
E→E +T •
T→T •*F
I
2
E→T •
T→T •*F
T→T *•F
F→•(E )
F→•id
I
7
T→T *F•
I
10
E→E •+T
F→(E •)
I
8
F→(E )•
I
11
T→F •
I
3
F→id•
I
5
E
T
F
id
(
+T
E)
*
F
id
id
F
*
+
(
(
F

Page 50/91
FIRST() and FOLLOW() Sets
From the previous lectures, it is easy to 
calculate the following sets: 
⚫FIRST(E) = { (, id }
⚫FIRST(T) = { (, id }
⚫FIRST(F) = { (, id }
⚫FOLLOW(E) = { +, ), $ }
⚫FOLLOW(T) = { +, *, ), $ }
⚫FOLLOW(F) = { +, *, ), $ }

Page 51/91
SLR(1) Parsing Table
State
ACTIONGOTO
id+*()$ETF
0s5s4123
1s6acc
2r2s7r2r2
3r4r4r4r4
4s5s4823
5r6r6r6r6
6s5s493
7s5s410
8s6s11
9r1s7r1r1
10r3r3r3r3
11r5r5r5r5

Page 52/91
This Is Not an LR(0) Grammar! 
State
ACTIONGOTO
id+*()$ETF
0s5s4123
1s6acc
2r2r2s7/r2r2r2r2
3r4r4r4r4r4r4
4s5s4823
5r6r6r6r6r6r6
6s5s493
7s5s410
8s6s11
9r1r1s7/r1r1r1r1
10r3r3r3r3r3r3
11r5r5r5r5r5r5

Page 53/91
4. More Powerful LR Parsing
A Grammar That Is Not SLR(1)
LR(1) Parsing Table
LALR(1) Parsing Table
Conflicts in LALR(1) Parsing

Page 54/91
An Unambiguous But Not SLR(1) 
Grammar
Consider the unambiguous grammar, 
which is not SLR(1): 
S→L =R  |  R
L→*R  |  id
R→L

Page 55/91
Conflicts in Items
Part of the DFA recognizing all viable prefixes
S' →•S
S→•L =R
S→•R
L→•*R
L→•id
R→•L
I
0
L
S→L •=R
R→L •
I
2
A shift/reduce conflicton 
input symbol '=', since 
'=' FOLLOW(R)
...

Page 56/91
Motivation of LR(1) Parsing
How to make use of the lookahead? 
⚫LR(0):  does not use the lookahead. 
All columns are filled with the same reduction. 
⚫SLR(1): simply make use of the lookahead. 
Only columns in the FOLLOW set are filled. 
More accurate than LR(0), thus less conflicts. 
⚫More powerful LR parsing –LR(1)
Only columns (symbols) that can follow the 
viable prefixes are filled. 
More accurate than SLR(1). 
⚫Trade-off –LALR(1)
More efficient but less powerful than LR(1).
More powerful than SLR(1). 

Page 57/91
Definition: Valid LR(1) Item
The definition of a valid item
⚫Item [A →
1
•
2
, a] is valid for a viable prefix 

1
, if there exists a derivation 
S' 
*
rm
A 
rm

1

2

where starts with a, or = a= $. 

Page 58/91
Theorem 1 on Valid LR(1) Items
Foundation of closure(), which is used to 
construct the states of DFA. 
⚫If [A →
1
•B 
2
, a] is a valid item for 
1
, 
and B →is a production, 
then [B →•, b] is also a valid item for 
1
, 
where bFIRST(
2
a). 
⚫[Proof]
S'
*
rm
A a
rm

1
B 
2
a(definition)

*
rm

1
B b(suppose 
2
a 
*
rm
b )

rm

1
b(we have B →)
[B →•, b] is valid for 
1
(definition)

Page 59/91
Theorem 2 on Valid LR(1) Items
Foundation of goto(), which is used to 
construct the transitions of DFA. 
⚫If [A →
1
•X 
2
, a] is a valid item for 
1
, 
then [A →
1
X •
2 
, a] is a valid item for 
1
X.
⚫[Proof]
S' 
*
rm
A a
rm

1
X 
2
a(definition)
S' 
*
rm
A a
rm

1
X
2
a(definition)
[A →
1
X •
2 
, a] is valid for 
1
X(definition)

Page 60/91
An Example
Consider the following grammar
(0)S'→S
(1)S→C C
(2)C→cC
(3)C→d

Page 61/91
FIRST() and FOLLOW() Sets
It is easy to calculate the following sets: 
⚫FIRST(S) = FIRST(C) = { c, d }
⚫FOLLOW(S) = { $ }
⚫FOLLOW(C) = { c, d, $ }

Page 62/91
DFA recognizing all viable prefixes
S' →•S, $
S→•C C, $
C→•cC, c/d
C→•d, c/d
I
0
C
S→C •C, $
C→•cC, $
C→•d, $
I
2
c
C→c•C, $
C→•cC, $
C→•d, $
I
6
C
C→cC •, $
I
9
S' →S •, $
I
1
S
S→C C •, $
I
5
C
C→d•, $
I
7
ddc
C→c•C, c/d
C→•cC, c/d
C→•d, c/d
I
3
c
C
C→cC •, c/d
I
8
c
C→d•, c/d
I
4
d
d

Page 63/91
LR(1) Parsing Table
State
ACTIONGOTO
cd$SC
0s3s412
1acc
2s6s75
3s3s48
4r3r3
5r1
6s6s79
7r3
8r2r2
9r2

Page 64/91
LALR(1) Parsing Table
Lookahead-LR parsing in practice
⚫Number of states, such as C or Pascal
LR(0) = SLR(1): several hundreds. 
LR(1): several thousands (10 times). 
⚫Strategy for LALR(1): merge the states with 
the same core. 
E.g.  I
3
and I
6
, I
4
and I
7
, I
8
and I
9
Lookaheads of the same item are merged. 
GOTO() depends only on the core. 

Page 65/91
LALR(1) vs. LR(1)
The merge will never produce newshift-
reduce conflicts
⚫Suppose in the merged state
[A →•, a] calls for a reduction
[B →•a , b] calls for a shift
⚫Since the original states have the same core, 
there must be some state have
[A →•, a] calls for a reduction
[B →•a , c] calls for a shift (for some c)
⚫Then the original state already has conflicts. 

Page 66/91
LALR(1) vs. LR(1) (cont')
But the merge will produce newreduce-
reduce conflicts
⚫For example
S'→S
S→aA d|  bB d|  aB e|  bA e
A→c
B→c
⚫{[A →c•, d], [B →c•, e]} is valid for viable 
prefix ac, {[A →c•, e], [B →c•, d]} is valid 
for viable prefix bc. But the merge has conflicts:
{[A →c•, d/e], [B →c•, d/e]}

Page 67/91
Example: New Conflicts in LALR(1)
Part of the DFA recognizing all viable prefixes
S' →•S, $
S→•aA d, $
S→•bB d, $
S→•aB e, $
S→•bA e, $
I
0
a
S→a•A d, $
S→a•B e, $
A→•c, d
B→•c, e
I
1
c
A→c•, d
B→c•, e
I
2
b
S→b•B d, $
S→b•A e, $
A→•c, e
B→•c, d
I
3
c
A→c•, e
B→c•, d
I
4

Page 68/91
More Efficient Algorithm to Construct 
LALR(1) Parsing Tables
Just feel free to ignore it. 
But why do you 
ignore it? 

Page 69/91
5. Ambiguity in LR Parsing
Trade-off and consequence for ambiguity 
in LR parsing
⚫Resolving ambiguities at the grammar level
Pros and cons? 
⚫Resolving ambiguities at the parsing table level
Pros and cons? 
⚫Resolving ambiguities at the source code level
Pros and cons? 

Page 70/91
Ambiguous Expression Grammar
Given the ambiguous grammar
(0)E'→E
(1)E→E +E
(2)E→E *E
(3)E→(E )
(4)E→id

Page 71/91
DFA recognizing all viable prefixes
shift-reduce conflict
+
I
4
E' →•E
E→•E +E
E→•E *E
E→•(E )
E→•id
I
0
E →id•
I
3
*
E' →E •
E→E •+E
E→E •*E
I
1
E →E +•E
E→•E +E
E→•E *E
E→•(E )
E→•id
E →E *•E
E→•E +E
E→•E *E
E→•(E )
E→•id
I
5
id
E →(E •)
E→E •+E
E→E •*E
I
6
)
E →(E )•
I
9
E
E
E →(•E )
E→•E +E
E→•E *E
E→•(E )
E→•id
I
2
(
E →E +E •
E→E •+E
E→E •*E
I
7
E
E →E *E •
E→E •+E
E→E •*E
I
8
E
*+
*
+
+
*
(
id
(
(
id
id

Page 72/91
SLR(1) Parsing Table
State
ACTIONGOTO
id+*()$E
0s3s21
1s4s5acc
2s3s26
3r4r4r4r4
4s3s27
5s3s28
6s4s5s9
7r1/s4r1/s5r1r1
8r2/s4r2/s5r2r2
9r3r3r3r3
Resolve ambiguities 
at the parsing table level

Page 73/91
Dangling-elseGrammar
Given the ambiguous grammar
(0)S'→S
(1)S→iS eS
(2)S→iS
(3)S→a

Page 74/91
shift-reduce conflict
S' →•S
S→•iS eS
S→•iS
S→•a
I
0
S →i•S eS
S →i•S
S→•iS eS
S→•iS
S→•a
I
2
S →iS •eS
S→iS •
I
4
S →iS e•S
S→•iS eS
S→•iS
S→•a
I
5
S'→S •
I
1
S→a•
I
3
iSe
a
S
a
i
i
a
S →iS eS •
I
6
S
DFA Recognizing All Viable Prefixes

Page 75/91
SLR(1) Parsing Table
State
ACTIONGOTO
iea$S
0s2s31
1acc
2s2s34
3r3r3
4r2/s5r2
5s2s36
6r1r1
Resolve ambiguities 
at the parsing table level

Page 76/91
Parsing a Sentence
Step
SymbolStateInputReferenceActionOutput
1$0i i a e a $a[0, i] = s2shift
2$ i0 2i a e a $a[2, i] = s2shift
3$ i i0 2 2a e a $a[2, a] = s3shift
4$ i i a0 2 2 3e a $
a[3, e] = r3
g[2, S] = 4
reduce
S→a
5$ i i S0 2 2 4e a $a[4, e] = s5shift
6$ i i S e0 2 2 4 5a $a[5, a] = s3shift
7$ i i S e a0 2 2 4 5 3$
a[3, $] = r3
g[5, S] = 6
reduce
S→a
8$ i i S e S0 2 2 4 5 6$
a[6, $] = r1
g[2, S] = 4
reduce
S→iS eS
9$ i S0 2 4$
a[4, $] = r2
g[0, S] = 1
reduce
S→iS
10$ S0 1$a[1, $] = accaccept

Page 77/91
6. Error Recovery in LR Parsing
For errors in an input sentence
⚫All errors will be found by all LR parsers
If we can construct a parsing table without 
conflicts. 
⚫More reductions before reporting an error
LR(0) SLR(1) LALR(1) LR(1)
But all of them will never shift an erroneous 
input symbol onto the stack. 

Page 78/91
Review: Error Recovery Techniques
Panic-Mode Error Recovery 
⚫Skip inputs until synchronizing token found. 
Phrase-Level Error Recovery 
⚫Assign each empty entry a specific error 
routine. 
Error-Productions 
⚫Suitable for common errors but not all errors. 
Global-Correction 
⚫Globally analyze the input to find the error. 
⚫Expensive and not in practice.

Page 79/91
Panic-Mode Error Recovery
Pop until a state swith a GOTO() on a 
particular nonterminal Ais found. 
⚫Usually Arepresents major constructs, e.g. 
stmt, expr, or block. 
⚫It indicates that the construct A has errors. 
Push GOTO(s, A). 
0 or more lookaheads are then discarded, 
until a symbol that can follow A. 
It is easier to understand if we look 
from the viewpoint of DFA

Page 80/91
Phrase-Level Error Recovery
Examine each empty entry, and assign it 
a pointer to a specific error-handling 
routine.
Ad hoc: depending on the usage of the 
language.  

Page 81/91
The Previous DFA recognizing all viable prefixes
+
I
4
E' →•E
E→•E +E
E→•E *E
E→•(E )
E→•id
I
0
E →id•
I
3
*
E' →E •
E→E •+E
E→E •*E
I
1
E →E +•E
E→•E +E
E→•E *E
E→•(E )
E→•id
E →E *•E
E→•E +E
E→•E *E
E→•(E )
E→•id
I
5
id
E →(E •)
E→E •+E
E→E •*E
I
6
)
E →(E )•
I
9
E
E
E →(•E )
E→•E +E
E→•E *E
E→•(E )
E→•id
I
2
(
E →E +E •
E→E •+E
E→E •*E
I
7
E
E →E *E •
E→E •+E
E→E •*E
I
8
E
*+
*
+
+
*
(
id
(
(
id
id

Page 82/91
The Previous Parsing Table
State
ACTIONGOTO
id+*()$E
0s3e1e1s2e2e11
1e3s4s5e3e2acc
2s3e1e1s2e2e16
3r4r4r4r4r4r4
4s3e1e1s2e2e17
5s3e1e1s2e2e18
6e3s4s5e3s9e4
7r1r1s5r1r1r1
8r2r2r2r2r2r2
9r3r3r3r3r3r3
Postpone error detection until one or 
more reductions are made

Page 83/91
Error-Handling Routines
e1: an operand ('id' or '(') is expected. 
⚫pushstate 3;  // add a symbol 'id'
e2: unbalanced right parenthesis. 
⚫dropone lookahead;  // remove ')'
e3: an operator is expected. 
⚫pushstate 4;  // add a symbol '+'
e4: a right parenthesis is expected. 
⚫pushstate 9;  // add a symbol ')'

Page 84/91
Parsing an Erroneous Input
Step
SymbolStateInputReferenceActionOutput
1$0id + ) $a[0, id] = s3shift
2$ id0 3+ ) $
a[3, +] = r4
g[0, E] = 1
reduce
E→id
3$ E0 1+ ) $a[1, +] = s4shift
4$ E +0 1 4) $a[4, )] = e2drop
Unbalanced ')'
5$ E +0 1 4$a[4, $] = e1push 3
Operand expected
6$ E + id0 1 4 3$
a[3, $] = r4
g[4, E] = 7
reduce
E→id
7$ E + E0 1 4 7$
a[7, $] = r1
g[0, E] = 1
reduce
E→E +E
8$ E0 1$a[1, $] = accend

Page 85/91
Conclusions: 
Context-Free Grammar Classification

Page 86/91
Exercise 7.1
Consider the grammar
S→(S R|  a
R→,S R|  )
⚫Try to construct an SLR(1) parsing table for the 
grammar, and see if there are conflicts in the 
parsing table. 

Page 87/91
Exercise 7.2
Consider the grammar
S→S a b|  aR
R→S |  a
Is the grammar an SLR(1) grammar? and 
why? 

Page 88/91
Exercise 7.3
Consider the grammar
S→A
A→B A |  
B→aB |  b
⚫Prove that the grammar is an LR(1) grammar. 
⚫Construct an LR(1) parsing table for the grammar. 
⚫Show the detailed parsing procedure for the 
sentence abab, following the style in slides of 
this lecture. 

Page 89/91
Exercise 7.4
*
(DBv2, pp.278, ex.4.7.4) Show that the 
grammar
S→A a|  bA c|  d c|  b d a
A→d
is LALR(1) but not SLR(1). 

Page 90/91
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 4.6 on SLR(1) parsing. 
Section 4.7 on LR(1) and LALR(1) parsing. 
Section 4.8.1-4.8.2 on ambiguities in LR 
parsing. 
⚫Skip Reading: 
Section 4.8.3 on error recovery in LR parsing. 

Page 91/91
Enjoy the Course!
