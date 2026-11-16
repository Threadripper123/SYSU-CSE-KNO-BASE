# Lecture01.Introduction

> 主题库：编译原理　｜　源文件：Lecture01.Introduction.pdf　｜　14,043 字符　｜　71 个片段　｜　更新于 2026-11-11


Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
©Copyright 2008-2022, Sun Yat-senUniversity

Page 2/61
Lecture 1. Introduction
1.Computer Languages
2.Language: Definition and 
Processing
3.Structure of a Compiler
4.Compiler Construction
5.Course Description

Page 3/61
Prologue
Why learning compiler courses?
⚫Excellent combination of theory and practice
⚫More insights into programming languages
⚫Classical instance of Programming in the 
Largeand Software Engineering
But for those students who almost never 
develop a compiler
⚫We focus on: language is an alternative 
approach to problem solving. 

Page 4/61
Let's Play a Game
Calculate the following with Windows 
GUI calculator (mouse only)
⚫5 + (8 –2)
⚫(286 + 8716) / (1973 + 348)
What's the revelation?
⚫How about solving this problem using a 
language?

Page 5/61
Language Processing
Language
⚫Programming languages
Including scripts
⚫Domain-Specific Languages (DSL)
SQL, HTML, XML, PostScript/PDF/LaTex, etc. 
Report, workflow, music, recitation, etc. 
Processing
⚫Specific to languages
⚫Even for programming languages
Not only compiling, but also ...
Beautifier, complexity evaluation, structured 
editor, reverse engineering, etc. 

Page 6/61
1. Computer Languages
Language Participants and Courses
Language Definition: How to Keep 
Consistency?
Ambiguity
Syntax, Semantics and Pragmatics

Page 7/61
Participants of a Language
Language
Designer
User
Implementer

Page 8/61
More Participants
Language
Designer
User
Implementer
Proposer
Ada, 1980'

Page 9/61
Corresponding Courses
Language
Programming
User
Principles of
Compiler Construction
Implementer
Foundations of
Programming
Languages
Designer

Page 10/61
How to Keep Consistency
Language
User
Implementer
Designer

Page 11/61
Natural vs. Formal Languages
Natural languages lead to ambiguity
⚫E.g. the order of parameter evaluationin 
programming languages such as Java and C++. 
Fatal weakness of natural languages
⚫Ambiguities can be removed by updates or 
amendments.  
⚫But it's impossible to support automatic 
language processing.
Wide gap between
formal and informal
categories

Page 12/61
Ambiguity
《论语·泰伯篇》
⚫民可使由之不可使知之
⚫民可使由之，不可使知之。
⚫民可使，由之；不可使，知之。
⚫民可使，由之不可，使知之。

Page 13/61
Ambiguities in Computer Programs
Two typical kinds of ambiguities
⚫Precedence and associativity in expressions
a + b * c
⚫Dangling else problem
if x > 0 then if y > 0 then x := 0 else y := 0
if x > 0 then if y > 0 then x := 0else y := 0
Trade-off
⚫Unambiguity: unambiguous rulesonly
⚫Simplicity: ambiguous rules+ additional constraints

Page 14/61
2. Language: Definition and Processing
Syntax, Semantics and Pragmatics
BNF and Syntax Graph
Formal Approach to Syntax
Formal Semantics
Type System

Page 15/61
Syntax, Semantics and Pragmatics
Syntax
⚫The phrase structure of symbols. 
⚫A program must be well-formed. 
Semantics
⚫The meaning of programs, i.e. the connection 
between symbols and the meanings they 
denote. 
Pragmatics
⚫The ways in which context contributes to 
meaning. 
⚫Not quite clear nowadays.

Page 16/61
Syntax + Semantics + Pragmatics
=
Semiotics

Page 17/61
Abstraction of a Language
Thinking in abstraction
⚫Abstract the most important features 
that we take under consideration. 
⚫Ignore other subordinate details. 
E.g. pronunciation of the language
Do you believe it?
⚫{a, ab, abb, abbb, ...} is a language. 
⚫is also a language. 

Page 18/61
Achievements and Opportunities
DomainAchievementsPeriod
Syntax
Semantics
Pragmatics
40’s -60’s
70’s -90’s
--

Page 19/61
Syntax Definition
1.Character set
⚫Properties: finite set; order. 
⚫Examples:
Adaand C++: ASCII
APL: EBCDIC
Java: ?
2.Syntax rules
⚫BNF
⚫Syntax Graph

Page 20/61
BNF: Backus-Naur Form
John Backus (IBM)
(Dec. 3, 1924 -March 17, 2007)
Father of Fortran
ACM Turing Award, 1977
"Much of my work has come from being lazy. 
I didn't like writing programs ..."
--IBM Think Magazine, 1979
Prof. Peter Naur
University of Copenhagen
The 17-page Algol 60 Report
ACM Turing Award, 2005

Page 21/61
Examples of BNF
identifier::=letter { letter | digit }
letter::=A| B| ... | Z| a| b| ... | z
digit::=0| 1| ... | 9
integer::=[ symbol ] unsigned
unsigned::=digit { digit }
symbol::=+| -
digit::=0| 1| ... | 9
for_stmt::=forloop_var :=init direction final 
dostmt
loop_var::=int_var
int_var::=var_id
var_id::=identifier
init::=expr
final::=expr
direction::=to| downto

Page 22/61
More Examples of BNF
Rules: 
bexpr::= bexpr orbterm | bterm
bterm::= bterm andbfactor | bfactor
bfactor::= notbfactor | (bexpr )| true| false
Instances: 
true and false or (not true)
false or true and not false
true and false and (not false and (true or false))

Page 23/61
Syntax Graph
Prof. Niklaus Wirth
Father of Pascal
ACM Turing Award, 1984
letter
letter
digit
identifier
digit
integer
+
-
digit
2
1
9
...

Page 24/61
Formal Approach to Syntax
LR Parsing
BNF
Syntax Graph
Automata
Formal
Languages
lex + yacc
Full Automation of 
Syntax Implementation
Presentation
Technology
Parsing
Technology
Application
Products
Achievements
Theoretical
Foundation

Page 25/61
Achievements
1.A de factostandardto define syntax of 
a new language. 
2.Analyze syntactic propertiesof a 
language. 
⚫Is it (or the syntax defined) ambiguous? 
⚫Is the grammar LL(k) or LR(k)? 
⚫...
3.Automationof syntax processing.
⚫For automation, the input of the processor 
must be formal definitions.
Lexical rules as the input of lex
Syntax rules as the input of yacc

Page 26/61
Difficulties in Formal Semantics
Difficulties in nature
⚫Must be based on formal syntax. 
⚫More complex than syntax. 
⚫More mathematical foundations 
required.
Artificial difficulties
⚫Different viewpoints lead to different 
approaches.
⚫Notations: lack of standardization. 

Page 27/61
Mathematical Difficulties
Mathematical foundations required
⚫Discrete mathematics
Set theory, mathematical logic, abstract algebra, 
category theory, type theory, etc.
⚫Computational models
-calculus, formal languages and automata, process 
algebra, Petri nets, etc.
⚫Proprietary theories
Domain theory, power domain theory, Hoare logicand 
other logics, etc.

Page 28/61
Approaches to Formal Semantics
Different viewpoints lead to various 
approaches to formal semantics: 
⚫Operational semantics
⚫Denotational semantics
⚫Axiomatic semantics
⚫Algebraic semantics
Semantics of ADTs based on category theory 
Levels of
Abstraction

Page 29/61
Expectation of Formal Semantics
A standard to define formal semantics of 
a language. 
Formal analysis of semantic properties.
⚫Is the language strong typing? 
⚫Does the language support block structures? 
⚫Is the language single threading? ...
Automation of the semantic processing of 
language processors.
⚫Formal definition of semantics as the input. 

Page 30/61
Type System
A lightweight formal semantics
⚫Maybe the most successful application 
of formal semantics in practice. 
Type safeness
⚫Compiler can discover all type related 
errors statically. 
C++:  floatx = 3.14;(correct)
Java:  floatx = 3.14;(erroneous)
Explicit type conversion in Java
floatx = (float) 3.14;

Page 31/61
Important Terminology
Static vs. dynamic
⚫I.e. compile-time vs. run-time
Explicitly vs. implicitly
⚫I.e. manually vs. automatically
Logical vs. physical
⚫Two levels of abstraction
Safeness vs. security
⚫Type safeness
⚫Thread safeness

Page 32/61
3. Structure of a Compiler
Basic Concepts of Translation
Phases of a Compiler
A Compiling Example
Software Architecture

Page 33/61
How Does a Compiler Work? 
Translator
SourceTarget

Page 34/61
Basic Concepts
Programming languages
⚫High-level
SP, OOP, functional, logical, concurrent, 
etc.
⚫Low-level
Assembly, machine
⚫Discussion
What's the essential differences between 
high-level and low-level programming 
languages?


Page 35/61
Basic Concepts (cont')
Translation
⚫Discussion
Compiler vs. (macro) assembler
Compiler vs. interpreter (advantages and 
disadvantages)
What is the execution model for Java and 
Microsoft .Net? And why? 
JavaMicrosoft

Page 36/61
Structure of a Compiler
Character Stream
Scanner
Parser
Semantic
Analyzer
Int. Code
Generator
Optimizer
Code
Generator
Token Stream
Parse Tree
Intermediate Code
Target Code
Lexical Definition
Regular Expressions
Linear Grammars
Finite Automata
Syntax Definition
BNF
Semantic Definition
Type System
Other Rules
Semantic Definition
Non-declarative: Actions
Front
End
Back
End
Pipe
Filter
Legend

Filter and Pipline



scan

scan


parse tree





Page 37/61
Architecture Design
Analysis vs. synthesis
⚫Structure analysis
Lexical analysis and syntax analysis
⚫Semantic analysis
Front end vs. back end
⚫Standard intermediate representation (IR/IL) 
supports substitution. 
GCC: GNU (/'gnu:/)Compiler Collection. 
Error recovery and symbol table 
management


Page 38/61
Cousins of a Compiler
Preprocessor
⚫C/C++: #include <...>
Assembler
Linker
Loader
Debugger
IDE: Integrated Development Environment
⚫Editor + Compiler + Linker + Debugger + ... 


Page 39/61
A Compiling Example
Source
⚫position = initial + rate * 60
Scanner
⚫<id,1> <=> <id,2> <+> <id,3> <*> <60>
Parser and semantic analyzer
=
<id,1>
<id,2>
<id,3>
+
*
60
=
<id,1>
<id,2>
<id,3>
+
*
int2float
60
token stream
parse tree
symbol table

<>token

token

token






Page 40/61
A Compiling Example (cont')
Intermediate code generator
⚫t1= int2float(60)
⚫t2= id3 * t1
⚫t3= id2 + t2
⚫id1= t3
Code optimizer
⚫t1= id3 * 60.0
⚫id1= id2 + t1
Code generator
⚫LDFR2, id3
⚫MULFR2, R2, #60.0
⚫LDFR1, id2
⚫ADDFR1, R1, R2
⚫STFid1, R1




Page 41/61
Software Architecture
What is Software Architecture?
Typical SA styles
⚫Layered (3-tier, n-tier)
⚫Pipes and filters
⚫Event-driven
⚫Client-server
⚫etc.
What benefits from SA?
⚫
⚫
Passes
Logical vs. physical
Buffer

Page 42/61
4. Compiler Construction
Requirements for compiler design
T-diagram
Bootstrapping and porting
Compiler generators
⚫Scanner generators
⚫Parser generators
⚫Other generators

Page 43/61
Requirements for Compiler Design
Efficiency of a compiler
⚫Time vs. space
Efficiency of the target code
⚫Time vs. space
Ability to error recovery
High reliability
...
compiler


Page 44/61
T-Diagram: A Formal Notation
T-diagram
T-diagram combination
SourceTarget
Host
HK
M
ST
H
ST
K
HostSource
Target

Page 45/61
Self Compiling
Write a compiler in the same 
language
C++A
C++
C++C++

Page 46/61
Bootstrapping
The bootstrapping process may be 
repeated
C++A
C
CA
A
C++A
A

Page 47/61
Porting
Cross-compiler (2 stages)
⚫Stage 1
⚫Stage 2
C++B
C++
C++A
A
C++B
A
C++B
C++
C++B
A
C++B
B

Page 48/61
C++B
C++
C++A
A
C++B
A
C++B
C++
C++B
B
C++B
C++
C++A
A
C++B
A
C++B
C++
C++B
B
Other Notations of Combination

Page 49/61
Compiler Construction Tools
Formal definitions lead to automatic 
tools
⚫lex:  scanner generator
⚫yacc:  parser generator
(Yet Another Compiler Compiler)
Popular tools
⚫C/C++:  GNU Flexand GNU Bison
⚫Java:  JFlexand JavaCUP, ANTLR
scanner
parser


Page 50/61
lex and yacc
lex
Scanner
Lexical Rules (RE)
and Actions
token streamsource code
yacc
Parser
Syntax Rules (BNF)
and Actions
parse treetoken stream
rules
rules
actions
table

Page 51/61
5. Course Description
Textbook
References
Assessments

Page 52/61
Textbook
The Dragon Book, 2
nd
Ed.
⚫A. Aho, M. Lam, R. Sethi and J. Ullman. 
Compilers: Principles, Techniques, and Tools, 
2nd Ed. 
Addison-Wesley, 2006, ISBN 0-321-48681-1
We only use about 600 pages: 
⚫Chapter 1 –8
⚫Appendix A

Page 53/61
References
The Dragon Book, 
1
st
Ed.
⚫A. Aho, R. Sethi and J. Ullman. 
Compilers: Principles, Techniques, and Tools. 
Addison-Wesley, 1988, ISBN 0-201-10088-6 
⚫李建中、姜守旭译. 
编译原理. 
北京: 机械工业出版社, 计算机科学丛书中文系列, 2003, 
ISBN 7-111-12349-2 
The ancestor of many textbooks compiled 
in Chinese. 

Page 54/61
References
The Tiger Book
⚫A. Appel. 
Modern Compiler Implementation in Java. 
Cambridge University Press, 2002, ISBN 0-521-
82060-X
⚫陈明等译. 
现代编译器的Java实现（第2版）. 
北京: 电子工业出版社, 国外计算机科学教材系列, 2004, 
ISBN 7-121-00270-1
A book worth buying and reading. 

Page 55/61
References
The Whale Book
⚫S. Muchnick. 
Advanced Compiler Design and Implementation. 
Morgan Kaufmann, 1997, ISBN 1-558-60320-4
⚫No Chinese version available. But there is a copyright 
transferred English version in Mainland China 
(published by CMP). 
Focus on compiler optimization. 
Not suitable for beginners. 

Page 56/61
References in Chinese
陈火旺、刘春林、谭庆平、赵克佳、刘越. 
程序设计语言编译原理（第3版）. 
国防工业出版社, 2000, ISBN 7-118-02207-1
杜淑敏、王永宁. 
编译程序设计原理. 
北京大学出版社, 1990, ISBN 7-301-01210-1
张素琴、吕映芝、蒋维杜、戴桂兰. 
编译原理（第2版）. 
清华大学出版社, 清华大学计算机系列教材, 2005, 
ISBN 7-302-08979-5

Page 57/61
Assessments
Lectures
⚫Weeklywrittenassignment 10:  10%
⚫Quiz 5:  30%
⚫Finalexam:60%
Labs
⚫Lab #1:  10%
⚫Lab #2:  15%
⚫Lab #3:  15%
⚫Lab #4:  25%
⚫Lab #5:  35%
1110
51-2

Java




Page 58/61
Exercise 1.1
Imagine an artificial computer language, which can be 
utilized to solve a practical problem, i.e. the application 
of the language. 
⚫Tips 1. Language is an alternative approach to problem 
solving. 
⚫Tips 2. First find a proper problem, then design a 
language to solve the problem. 
Give an example of a complete piece written in the 
proposed language. 
Discuss how to define the new language and try your 
approach. 
Describe the process of changing the thinking of your 
language to a reality, i.e. how to make the artificial 
language usable. 

Page 59/61
Exercise 1.2
Draw a T-diagram with two stages of 
bootstrappings. 
⚫Given a new programming language L++, we 
firstly implement L, a small subset of L++. 
⚫Then we use L to implement L+, a subset of L++ 
and a superset of L. 
⚫Finally, L++ is implemented using L+. 

Page 60/61
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: Section 1.1, 1.2, 1.6
⚫Skip Reading: Section 1.3, 1.4, 1.5
On domain-specific languages
⚫http://en.wikipedia.org/wiki/Domain-specific_programming_language

Page 61/61
Enjoy the Course!

explicitly

Deepseek
explicitly
·IR token streamAST
 LLVM IR
·IR  LLVM IR 
·
 GCCAST JIT 
 AST
