# Lecture03.LexicalAnalysis

> 主题库：编译原理　｜　源文件：Lecture03.LexicalAnalysis.pdf　｜　14,521 字符　｜　55 个片段　｜　更新于 2026-11-11


Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
©Copyright 2008-2022, Sun Yat-senUniversity

Page 2/54
Lecture 3. Lexical Analysis
1.Introduction
2.Scanner Construction
3.Lexical Specification
4.Finite Automata
5.Transformation and Equivalence
6.Limits of Regular Languages
7.Lexical Analysis in Practice

Page 3/54
1. Introduction
Software Architecture: Pipes and Filters
Scanner
stream of 
characters
stream of 
tokens



Page 4/54
Structure of a Scanner
Implicit lexical rules
Specific
Scanner
stream of 
characters
stream of 
tokens
lexical
rules
Specific to some predefined language. 

Page 5/54
Structure of a Scanner (cont')
Explicit lexical rules (interpretation model)
General
Scanner
stream of 
characters
stream of 
tokens
lexical
rules
No hard-coding of language-specific code.
scanner


Page 6/54
SE-303 Principles of Compiler Construction
©Copyright 2008-2010,  Sun Yat-sen University
Structure of a Scanner (cont')
Explicit lexical rules (compilation model)
Specific
Scanner
stream of 
characters
stream of 
tokens
Generator
(e.g. lex and JFlex)
lexical
rules
lexCJFlexJava


Page 7/54
Input Buffering
Most runtime of the front end of a 
compiler is spent on scanning. 
⚫But the most time consuming stage is 
optimization. 
Buffer Pairs
⚫2-buffer scheme: each buffer is of size N. 
⚫Avoid overwriting: |lexeme| + |lookaheads| N
eof
E=M*C*2
eof
*
eof
lexemeBegin
forward
4KB4KB
sentinel
sentinel
sentinel
CPU
buffer
1K4Kbuffer
lexemetoken
eofbuffer


Page 8/54
Output Tokens
5 kinds for most modern programming 
languages
⚫Identifiers: getBalance, weight, ...
⚫Reserved words: IF, ELSE, WHILE, ...
⚫Constants: 10, 3.14, -1.26E-5, 'a', "abc", ...
⚫Operators: +, -, *, /, <<, ...
⚫Punctuation: (, ), ;, :, ...
Output
⚫A Pair of <kind, associatedAttributeValues>
Some values are pointers to the symbol table.
⚫Symbol (string) table
Array, LinkedList, HashSet, TreeSet, ...




Page 9/54
Interaction with Parser
Aim at reducing passes
2 levels of abstraction
⚫Logically
A pipe through which tokens are 
transferred. 
⚫Physically
A disk file of the token sequence
Concurrent threads or co-routines
Method invocation

Page 10/54
2. Scanner Construction
Definition of lexical rules: 3 equivalent 
notations
⚫Expressions
Regular expression 
Regular definition
⚫Grammars
Regular grammar
Left/right linear grammar
⚫Finite automata
Deterministic Finite Automata (DFA)
Nondeterministic Finite Automata (NFA)

Page 11/54
Begin with Formal Specification
Specification of tokens
⚫Regular definition
Hierarchical and brief, suitable for humans.
Then transferred to regular expressions. 
Then transferred to finite automata. 
⚫Regular grammar
Compliant with the spec. of syntax rules. 
Then transferred to finite automata.
Transferring finite automata
⚫NFA →DFA →Reduced DFA
⚫Suitable for implementation on a machine. 

Page 12/54
Steps of Construction
RegularDefinition
RegularExpressionRegularGrammarNFA
DFA
ReducedDFA








Page 13/54
Programming a Scanner
Implicit transition diagram
⚫A manual approach
⚫State transitions are hard-coded in the 
program. 
Explicit transition diagram
⚫A table-driven approach
⚫No hard-coding of specific lexical rules
⚫Lead to automatic scanner generation. 

Page 14/54
3. Lexical Specification
Regular Expression
Regular Definition
Regular Grammar
Transformation and Equivalence

Page 15/54
Regular Expression
Regular expression: constructively defined
⚫Basis:
is a regular expr; L() = {}. 
ais a regular expr if a ; L(a) = {a}. 
⚫Induction: if rand sare regular expressions, 
r | sis a regular expr; L(r | s) = L(r) L(s). 
r sis a regular expr; L(r s) = L(r) L(s). 
r
*
is a regular expr; L(r
*
) = (L(r))
*
. 
(r)is a regular expr; L((r)) = L(r).
⚫Extensions: 
r
+
= r r
*
=r
*
rr
*
= r
+ 
| 
r
?
= r | 
[ace]= a | c | e[A–Z]= A | B | ...| Z

Page 16/54
Regular Expression Language
Regular expression: a language
⚫Syntax
a (a | b) b
a (a | b)
*
⚫Semantics
{aab, abb}
{a, aa, ab, aaa, aab, aba, abb, ...}
Language is an alternative approach to 
problem solving. 

Page 17/54
Discussions
Signature of an operator
⚫true: →bool
⚫+: int int →int
⚫+: real real →real  (overloading)
What is the signature of the semantic 
function L in the previous slides ?
⚫Syntactic category
⚫Semantic category
⚫Mapping

Page 18/54
Alphabet: Lexical vs. Syntax
Both scanner and parser are based 
over an alphabet . 
But the meaning (elements) of is 
quite different. 
⚫Scanner: elements in are characters 
in source programs. 
ASCII, EBCDIC, Unicode, ...
⚫Parser: elements in are tokens 
generated and passed by the scanner. 

Page 19/54
Algebraic Laws for 
Regular Expressions
r | s = s | r
r | (s | t) = (r | s) | t
r | r = r
r(st) = (rs)t
r(s | t) = rs | rt
(s | t)r = sr | tr
r= r = r
r*r* = r*
r* = | r | rr | ...
(r*)* = r*
rr* = r*r
(r* | s*)* = (r*s*)*
(r*s*)* = (r | s)*
(rs)*r = r(sr)*
(r | s)* = (r*s)*r*
Discussion:  what does it mean that
two regular expressions are equivalent ?

Page 20/54
Binding to Practice
Identifiers in Pascal
(a | b | ... | z | A | B | ... | Z)( (a | b | ... | z | A | B | ... | Z)| (0 | 1 | ... | 9))
*
Hard to read and write

Page 21/54
Regular Definition
In the form of
⚫d
1
→r
1
⚫d
2
→r
2
⚫...
⚫d
n
→r
n
where  d
i
(d
i
= d
j
i = j), and
r
i
is a regular expr over {d
1
, d
2
, ..., d
i –1
}

Page 22/54
Binding to Practice
Identifiers in Pascal (extended)
⚫letter_  →A | B | ... | Z | a | b | ... | z | _
⚫digit →0 | 1 | ... | 9
⚫id →letter_ (letter_ | digit)
*
Real numbers in Pascal
⚫digit  →0 | 1 | ... | 9
⚫digits  →digit digit
*
⚫optionalFraction →. digits | 
⚫optionalExponent→(E (+ | -| ) digits) | 
⚫real  →digits optionalFraction optionalExponent
Hierarchical representation of regular 
expression

Page 23/54
Binding to Practice (cont')
Writing a regular definition with extended 
operators
⚫digit  →[0–9]
⚫digits  →digit
+
⚫real  →digits (. digits)
?
(E [+-]
?
digits)
?
Note the difference between '–' and '-'

Page 24/54
Transforming between Regular 
Definitions and Regular Expressions
Regular definition →regular expression
⚫Top-down, stepwise substitution
⚫Discussion: does the substitution process 
always terminate ?
Regular expression →regular definition
⚫Simply add a new symbol and "→". 
Equivalence
⚫Defined by the language they denote
⚫Bi-direction


Page 25/54
Transforming Regular Definitions to 
Regular Grammars
Difficulties arise from the closure operator
⚫Transform to a left/right recursive production. 
In regular definition
⚫id  →letter (letter | digit)
*
Introduce a right part
⚫id  →letter rid
⚫rid  →rid (letter | digit) | (not permitted in Grammar)
that is, rid  →rid letter | rid digit | 
⚫or right-linear, rid  →letter rid | digit rid | 

definitionexpression

Page 26/54
Class 3 Grammars
Right Linear (Regular) Grammar
⚫A →aB or A →a, where A, B N a {}.
Left Linear Grammar
⚫A →Ba or A →a, where A, B N a {}.
Extended Forms
⚫Right Linear (Regular) Grammar
A →B or A →, where A, B N 
*
.
⚫Left Linear Grammar 
A →Bor A →, where A, B N 
*
.
1 
2 
3 

Page 27/54
4. Finite Automata
3 types of finite automata
⚫DFA (Deterministic Finite Automata)
⚫NFA (Nondeterministic Finite Automata)
⚫-NFA (Nondeterministic Finite Automata with 
empty transitions)
Languages defined by a finite automaton
Representation of finite automata
⚫For humans: Transition Diagrams
⚫For computers/machines: Transition Tables

Page 28/54
Formal Definition
A finite automaton is a 5-tuple: M = (, S, 
, s
0
, F), where
⚫is the input alphabet. 
⚫S = is a finiteset of states. 
⚫is a transition function. 
⚫s
0 
S is the start (initial) state. 
⚫F S is a set of final (accepting) states. 
DFA, NFA, and -NFA only differ in 
⚫For a DFA, :  S →S
⚫For an NFA, :  S →2
S
⚫For an -NFA, :  S ({}) →2
S



Page 29/54
Language Defined by an FA
Overloadingthe transition function
⚫For any a , s S and 
*
,
(s, ) = s
(s, a) = ((s, ), a)
Given a finite automaton M, 
⚫L(M) = {| 
* 
s F. (s
0
, ) = s}

Page 30/54
Transition Diagrams
Graphical representation of FA
B
CDE
A
1
0,1
0




0
10
0
1


1234
8
76
510
9
11
E
D
ABC
10
0
1
0
1
10
initial state
final state


Page 31/54
Commonly Used in Practice
Description of the state transition of a 
stateful object
UML (Unified Modeling Language)
⚫De facto standard for object-oriented modeling 
and design. 
⚫Statechart of a class. 

Page 32/54
Internal Representation
Transition table
⚫Rows
DFA, NFA, and -NFA:  State S
⚫Columns
DFA and NFA: Alphabet 
-NFA:  {}
⚫Cells
DFA: an element of S
NFAand -NFA:  a subset of S
That is why NFA & -NFA are not suitable 
for implementation. 

Page 33/54
5. Transformation and Equivalence
Regular Grammar to -NFA
Regular Expression to -NFA
Determination (of -NFA)
Reduction (of DFA)

Page 34/54
Regular Grammar to -NFA
Algorithm
⚫Add a new final state f
⚫For any a {} and A, B N, 
If A →a where a {}, let (A, a) = f
If A →aB, let (A, a) = B

Page 35/54
An Example
Given a regular grammar
A  →0 | 0B | 1D
B  →0D | 1C
C  →0 | 0B | 1D
D  →0D | 1D
We have an 
equivalent 
-NFA
AfC
B
D
0
0
1
0
0
0
0,1
1
1

Page 36/54
Regular Expression to -NFA
Thompson's construction (McNaughton-
Yamada-Thompson algorithm)
⚫A constructive approach corresponding with 
the constructive definition of regular 
expressions
⚫Basis:

a 

a

Page 37/54
⚫Induction:
r = s | t
2 new states
4 new transitions
r = s t
No new states 
or transitions
r = s
*
2 new states
4 new transitions
N(s)
N(t)

N(s)N(t)

N(s)




Page 38/54
An Example
Given regular expression
⚫(a | b)
*
abb
We have
⚫a | b
⚫(a | b)
*



0



6
32

1
a
54

b
7
a
8
b
9
b
10

Page 39/54
-NFA to DFA
Both 2 approaches are applied
⚫Subset construction
⚫-closure construction
Subset
⚫A state in the new DFA corresponds to a 
subset of states in the original NFA. 
-closure
⚫The empty transition does not consume any 
input. 

Page 40/54
An Example
A = {0}, B = {1, 2}, C = {3, 4, 5, 6, 8, 11}
D = {9, 10, 11, 5, 6, 8}, E = {7, 10, 11, 5, 6, 8}
E
D
ABC
10
0
1
0
1
10




0
10
0
1


1234
8
76
510
9
11

Page 41/54
Calculate the -closure
Let -closure({0}) = {0} = A, then
⚫(A, 1) = -closure({1}) = {1, 2} = B
⚫(B, 0) = -closure({3}) = {3, 4, 5, 6, 8, 11} = C
⚫(C, 0) = -closure({9}) = {5, 6, 8, 9, 10, 11} = D
⚫(C, 1) = -closure({7}) = {5, 6, 7, 8, 10, 11} = E
⚫(D, 0) = -closure({9}) = D
⚫(D, 1) = -closure({7}) = E
⚫(E, 0) = -closure({9}) = D
⚫(E, 1) = -closure({7}) = E

Page 42/54
Reduction of DFA
Find out equivalent classes of states
⚫Approach 1: 
Use a table to represent the equivalent 
relationship
Only a triangle is needed. 
⚫Approach 2: 
Use partitions of the set of states

Page 43/54
An Example
B
CDE
A
1
0,1
0
E
D
ABC
10
0
1
0
1
10
B

2
(A, 1) = B
(B, 1) = ⊥
C

1
Final

1
Final
D

1
Final

1
Final

3
(C, 0) = D 
= (D, 0)
(C, 1) = E 
= (D, 1)
E

1
Final

1
Final

4
(C, 0) = D 
= (E, 0)
(C, 1) = E 
= (E, 1)

5
(D, 0) = D 
= (E, 0)
(D, 1) = E 
= (E, 1)
ABCD

  

Page 44/54
More Example
{1, 2, 4, 5, 6, 7, 8} {3}  (final and non-final, that is from )
{1, 4, 5, 6, 7} {2, 8} {3}  (from 1 to final and non-final)
{1, 5, 7} {4, 6} {2, 8} {3}  (from 0 to final and non-final)
{1, 5} {7} {4, 6} {2, 8} {3}  (from 0 to state {2, 8} and {7})
{1, 5} {7} {4, 6} {2, 8} {3}  (or from 1 to state {6} and {5})
No difference found, terminate. 
010
110
0
1
1
0
1
0
1
0
1
0
1234
8765
01
1
0
1
0
0
1
0
15283
746
1

Page 45/54
6. Limits of Regular Languages
Regular languages are not able to handle 
infinite counting and matching
⚫e.g. {a
n 
b c
n
| n 1 } is NOT a regular set. 
The matching of parentheses in most 
programming languages 
⚫Can not be described by regular expressions
⚫Can not be generated by regular grammars
⚫Can not be recognized by finite automata


Page 46/54
7. Lexical Analysis in Practice
Scanner in practice: trained in our lab-
time
⚫Construct a scanner manually
Define the lexical rules
Construct a finite automaton
Write a scanner based on the blueprint of the 
finite automaton
⚫Construct a scanner with a scanner generator
lex, Flex, and JFlex

Page 47/54
String Processing
Regular expression is a common way to 
describe a pattern in a string. 
⚫Trade-off and consequence in the 
implementation of regular expressions
Recognizing with an NFA
Regular expression to DFA directly
RegularExpressionNFA
DFA
ReducedDFA

1
2
3

Page 48/54
Exercise 3.1
Give the recognized tokens of the following 
program in Pascal. 
functionmax(i, j: integer): integer; 
{return the maximum of integers i and j}
begin
ifi > j thenmax := i elsemax := j
end;

Page 49/54
Exercise 3.2
(DBv2, Ch.3, pp.125, ex.3.3.2) Describe 
the languages denoted by the following 
regular expressions:
⚫a (a | b)
* 
a
⚫a
* 
b a
* 
b a
* 
b a
*

Page 50/54
Exercise 3.3
(DBv2, Ch.3, pp.125, ex.3.3.4) Most 
Languages are case sensitive, so keywords 
can be written only one way, and the 
regular expressions describing their 
lexemes are very simple. 
However, some languages, like Pascal and 
SQL, are case insensitive. For example, the 
SQL keyword SELECTcan also be written 
select, Select, or sELEcT. 
Show how to write a regular expression for 
a keyword in a case insensitive language. 
Illustrate your idea by writing the 
expression for SELECTin SQL. 

Page 51/54
Exercise 3.4
Given the following regular expression
1
*
(0 | 01)
*
(1) Transform it to an equivalent finite automaton.
(2) Construct an equivalent DFA for the result of 
exercise (1).
(3) Reduce the result of (2) and get a reduced DFA. 

Page 52/54
Exercise 3.5
**
Given the alphabet = { z, o, / }, a 
comment in a program over begins with 
"/o" and ends with "o/". Embedded 
comments are not permitted. 
(1) Draw a DFA that recognizes nothing but all the 
comments in the source programs. 
(2) Write a single regular expression that exactly 
describes all the comments in the source 
programs. 

Page 53/54
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive reading: 
Section 2.6 and 3.1–3.2 for introduction to 
scanner. 
Section 3.3 for regular expressions and regular 
definitions. 
Section 3.6–3.7, 3.9.6 for finite automata and 
related transformation.
⚫Skip reading: 
Section 3.4–3.5 and 3.8 for scanner generator. 
Section 3.9.1–3.9.5 for regular expressions 
directly to DFAs.

Page 54/54
Enjoy the Course!
