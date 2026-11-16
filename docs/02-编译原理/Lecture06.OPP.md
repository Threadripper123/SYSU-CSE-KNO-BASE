# Lecture06.OPP

> 主题库：编译原理　｜　源文件：Lecture06.OPP.pdf　｜　9,379 字符　｜　44 个片段　｜　更新于 2026-11-11


Principles of 
Compiler Construction
©Copyright 2008-2022, Sun Yat-senUniversity
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn

Page 2/44
Lecture 6.  Operator Precedence 
Parsing (OPP)
1.Bottom-Up Parsing
2.Abstract Model for Shift-Reduce 
Parsing
3.Operator-Precedence Parser
4.What Have We discarded?
5.Does OPP Disappear?

Page 3/44
1. Bottom-Up Parsing
A motivating example
⚫Construct a parse tree beginning at the leaves 
and working up towards the root. 
⚫For the grammar
E→E +T  |  T
T→T *F  |  F
F→(E )|  n

Page 4/44
*nn
Begin with leaves: n * n

Page 5/44
F
*nn
The 1st nis the handle, after reduction 
we have the right sentential form: F * n

Page 6/44
T
F
*nn
Fis the handle and we have: T * n

Page 7/44
TF
F
*nn
nis the handle in T * n, we have: T * F

Page 8/44
T
TF
F
*nn
T * Fis the handle

Page 9/44
E
T
TF
F
*nn
T is the handle

Page 10/44
One More Example
For the grammar
S→aB cD e
B→B b|  b
D→d

Page 11/44
S
c
B
DB
b
ea
bd
Reduction = handle (b) pruning
Right sentential form: a b b c d e

Page 12/44
S
c
B
DBea
bd
Bb is the handle
Right sentential form: a B b c d e

Page 13/44
S
cDBea
d
dis the handle
Right sentential form: a B c d e

Page 14/44
S
cDBea
aBcDe is the handle
Right sentential form: a B c D e

Page 15/44
S
Successfully reduced to the root
Right sentential form: S

Page 16/44
Review
Reducable string in a sentential form
⚫Handle of a rightsentential form
If S 
*
rm
A
rm
, then 
production A→in the position following is 
a handle of .
⚫Properties
The substring to the right of the handle (i.e. 
in the previous definition) must contain only 
terminals. 
⚫Note that it is a right-most (rm) derivation. 

Page 17/44
Review (cont')
Handle in a parse tree
⚫A left-most two-level subtree. 
⚫Not practical. Only helpful to understand the 
concept. 
Critical problem in bottom-up parsing
⚫How to find a reducablestring in a sentential 
form?
LR parsing: handle (left-most simple phrase)
OPP: left-most prime phrase

Page 18/44
2. Abstract Model for Shift-Reduce 
Parsing
Symmetric to LL(1) parsing
⚫Table-driven approach
Parsing decision making based on a table
LL(1) parsing tablevs.
⚫Operator-precedence relation table
⚫LR(0)/SLR(1)/LALR(1)/LR(1) parsing table
⚫Explicit stack
Top-down parsing: [$S, $] [$, $]
Bottom-up parsing: [$, $] [$S, $]
⚫Actions
Top-down parsing: match-derive
Bottom-up parsing: shift-reduce

Page 19/44
An Abstract Model
A shift-reduce parser
Shift-Reduce Parser
Input
Output
Logical vs. Physical
+n*n$n
lookahead
$
Y
X
top
Stack
search with [top, lookahead]

Page 20/44
A Motivating Example
Given the previous grammar
S→aB cD e
B→B b|  b
D→d
For the previous sentence
a b b c d e

Page 21/44
A Motivating Example (cont')
StepStackInputActionOutput
1$a b b c d e $shift
2$ ab b c d e $shift
3$ a bb c d e $reduce
B →b
4$ a Bb c d e $shift
5$ a B bc d e $reduce
B →B b
6$ a Bc d e $shift
7$ a B cd e $shift
8$ a B c de $reduce
D →d
9$ a B c De $shift
10$ a B c D e$reduce
S →a B c D e
11$ S$accept

Page 22/44
More Motivating Examples
Given the ambiguousgrammar for 
expressions
E→E +E  |  E *E  |  (E )|  n
For the following sentence
n + n * n

Page 23/44
More Motivating Examples (cont')
StepStackInputActionOutput
1$n + n * n $shift
2$ n+ n * n $reduce
E →n
3$ E+ n * n $shift
4$ E +n * n $shift
5$ E + n* n $reduce
E →n
6$ E + E* n $shift
7$ E + E *n $shift
8$ E + E * n$reduce
E →n
9$ E + E * E$reduce
E →E *E
10$ E + E$reduce
E →E +E
11$ E$accept

Page 24/44
What Is Abstract in the Model?
What is the form of the parsing table?
⚫Operator-precedence relation table
⚫LR parsing table
What is the contents of the stack?
⚫Operators
⚫Parsing states
How to determine a reducablesubstring 
on the top of the stack?
⚫Left-most prime phrase
⚫Handles

Page 25/44
Concrete Implementations
Implementations of the abstract model for 
shift-reduce parsing
SLR(1) ParserLR(0) ParserLALR(1) ParserLR(1) Parser
OPP ParserLR Parser
Shift-Reduce Parser

Page 26/44
3. Operator-Precedence Parser
Concrete implementations
⚫Parsing table
Operator-precedence relations
⚫Explicit stack
Operators
⚫Reducable substring
Left-most prime phrases

Page 27/44
A Concrete Model for OPP Parser
Operator Precedence Parser
Operator-Precedence
Parser
Input
Output
Logical vs. Physical
+n*n$n
lookahead
$
+
*
top
Stack
search with [top, lookahead]

Page 28/44
Operator Grammar
A small but important class of grammars
⚫There is no -production. 
⚫No production has two adjacent nonterminals. 
For example
⚫Not an operator grammar
E→E A E  |  (E )|  −E  |  id
A→+|  −|  *|  /|  ^
⚫An operator grammar
E→E +E  |  E −E  |  E *E  |  E /E  |  E ^E
|(E )|  −E  |  id

Page 29/44
Operator-Precedence
Three precedence relations
⚫a b
⚫a b
⚫a b
Properties of the binary relation
⚫Non-reflexive
⚫Non-symmetric
⚫Non-transitive
⚫A partial relation

Page 30/44
Operator-Precedence Ralation Table
id+*$
id

+

*

$


Page 31/44
Parsing Algorithm
initialize();
for (;;) {
if (top == $ && lookahead == $)  accept();
topOp = stack.topMostTerminal();
if (topOp lookahead || topOp lookahead) {  // shift
stack.push(lookahead);
lookahead = scanner.getNextToken();
} else if (topOp lookahead) {  // reduce
do {
topOp = stack.pop();
} while (stack.topMostTerminal() || topOp);
} else  error();
}

Page 32/44
Parsing a Sentence
Step
StackInputReferenceActionOutput
1$n + n * n $
$n
shift
2$ n+ n * n $
n+
reduce
E →n
3$ E+ n * n $
$+
shift
4$ E +n * n $
+n
shift
5$ E + n* n $
n*
reduce
E →n
6$ E + E* n $
+*
shift
7$ E + E *n $
*n
shift
8$ E + E * n$
n$
reduce
E →n
9$ E + E * E$
*$
reduce
E →E *E
10$ E + E$
+$
reduce
E →E +E
11$ E$accept

Page 33/44
More Practical OPP Table
+
−
*/^id()$
+

−

*

/

^

id

(


)

$


Page 34/44
Another Form of OPP Table
+
−
*/^id()$
+
reducereduceshiftshiftshiftshiftshiftreducereduce
−
reducereduceshiftshiftshiftshiftshiftreducereduce
*
reducereducereducereduceshiftshiftshiftreducereduce
/
reducereducereducereduceshiftshiftshiftreducereduce
^
reducereducereducereduceshiftshiftshiftreducereduce
id
reducereducereducereducereducereducereduce
(
shiftshiftshiftshiftshiftshiftshiftshift
)
reducereducereducereducereducereducereduce
$
shiftshiftshiftshiftshiftshiftshift

Page 35/44
Error Recovery in OPP
+
−
*/^id()$
+
reducereduceshiftshiftshiftshiftshiftreducereduce
−
reducereduceshiftshiftshiftshiftshiftreducereduce
*
reducereducereducereduceshiftshiftshiftreducereduce
/
reducereducereducereduceshiftshiftshiftreducereduce
^
reducereducereducereduceshiftshiftshiftreducereduce
id
reducereducereducereducereduceerror1error1reducereduce
(
shiftshiftshiftshiftshiftshiftshiftshifterror2
)
reducereducereducereducereduceerror1error1reducereduce
$
shiftshiftshiftshiftshiftshiftshifterror3accept
Phrase-Level Recovery

Page 36/44
Handling Overloading Operators
Overloading unary and binary operators
⚫id –id * id
⚫id * –id
Distinguish or resolve overloading by the 
scanner
⚫Remember the previous processed token. 

Page 37/44
4. What Have We Discarded?
Automatically table construction
⚫Construct an operator-precedence relation 
table from an unambiguousgrammar of 
expressions.
⚫Trade-off: cost vs. cost
Writing an unambiguous grammar
Constructing an precedence relation table

Page 38/44
Saving Table Space
+
−
*/^id()$
f
224446060
g
113355500
Precedence functions
⚫Pack an nnmatrix into a 2 nmatrix. 
⚫Save somespace to store the parsing table. 
⚫But something is missing. 
Each pair of operators must have a relation. 

Page 39/44
5. Does OPP Disappear?
OPP has disappeared from many modern 
compiler textbooks, e.g. 
⚫DBv2 (while DBv1 concerned with OPP)
⚫Tiger book
⚫The Art of Compiler Design
⚫etc.

Page 40/44
Reasons for Ignoring OPP
Some reasons are: 
⚫OPP only handles very simple languages.
⚫OPP isn't very good at discovering when a 
string isn't in the language.
⚫Operator precedences have been incorporated 
into LR parsers, so writing an LR parser for 
expressions with many precedence levels isn't 
very hard.
⚫Space isn't nearly as much an issue as it used 
to be. 
⚫OPP is often taught in introductory Data
Structures courses. 

Page 41/44
But OPP Still Works
Even in commercial applications, specially 
combination of OPP and recursive parsing
⚫Some Sun javaccompiler
Recursive parser parses expressions coarsely. 
OPP parses the binary operators correctly. 
⚫Some open source Verilog simulator
OPP parses Verilog expressions as a separate 
section of a hand coded recursive descent parser. 
Verilog expressions are complex and language is 
irregular and non context-free. 
⚫The early Symantec's THINK C compiler
⚫The original C compiler at Bell Labs (reported) 

Page 42/44
Exercise 6.1
Consider the grammar
S→(L )|  a
L→L ,S|  S
We have the following operator-precedence 
relations for the grammar. 
⚫Show the detailed process 
of the parsing of the 
sentence (a, (a, a)), 
follow the style in the 
previous slides. 
a(),$
a

(



)

,

$


Page 43/44
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 4.5 for the abstract model for shift-
reduce parsing. 
Dragon Book, 1
nd
Edition (DBv1)
⚫Comprehensive Reading:
Section 4.6 for the OPP details. 
⚫Skip Reading: 
Section 4.6 on precedence functions. 

Page 44/44
Enjoy the Course!
