# Lecture05.LL Parsing

> 主题库：编译原理　｜　源文件：Lecture05.LL Parsing.pdf　｜　9,304 字符　｜　32 个片段　｜　更新于 2026-11-11


Principles of 
Compiler Construction
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
©Copyright 2008-2022, Sun Yat-senUniversity

Page 2/32
Lecture 5. LL(1) Parsing
1.Predictive Parsing Table
2.Table-Driven Parser
3.Error Recovery in LL(1) Parsing

Page 3/32
1. Predictive Parsing Table
Making decision of actions based on a 
parsing table
⚫Precondition: LL(1) grammars
Four actions in a top-down parser
⚫Derive
⚫Match
⚫Accept
⚫Error

Page 4/32
Review
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

Page 5/32
Parsing Table: An Example
For the previous grammar
Non-
terminal
lookahead
n+*()$
ET E'T E'
E'+T E'

TF T'F T'
T'

*F T'

Fn(E )

Page 6/32
Construction of Parsing Table
Algorithm: for each A →, 
⚫Add A →to M[A, a] for each aFIRST() . 
⚫If FIRST(), add A →to M[A, b] for each b
FOLLOW(A). 
Note that b may be the $ symbol, which 
indicates the end of input tokens. 
Discussions: conflicts in a parsing table
⚫Is it possible that one single entry has 
multiple productions? 
⚫What does it mean? 

Page 7/32
Conflicts in Parsing Table
The left factored dangling-else grammar
S→ifE  thenS  S'  |  other
S'→elseS  |  
E→expr
Non-
terminal
lookahead
ifthenelseotherexpr$
SifE thenS S'other
S'

else S

Eexpr

Page 8/32
Discussions
Where conflicts occur in the parsing table? 
⚫Left recursion
A →A a|  b
⚫Left factor
A →ab|  ac
⚫Ambiguity

Page 9/32
2. Table-Driven Parser
No hard-codings of specific grammar
⚫Parser is driven by a parsing table. 
The controller is general enough to handle all 
LL(1) grammars. 
⚫The parsing table contains all information 
about the grammar. 
Automatic generation of an LL(1) parser is the 
generation of a parsing table from the LL(1) 
grammar. 

Page 10/32
Model of Table-Driven Parsers
Components of the parser
⚫Specific to table-driven predictive parsers
Parsing Table
Controller
(Explicit) stack
⚫General to all parsers
Input: can be abstracted as a lookahead. 
Output: logical vs. physical

Page 11/32
Model of Table-Driven Parsers (cont')
A table-driven predictive parser
Predictive Parsing
Controller
Input
Output
Logical vs. Physical
+n*n$n
lookahead
$
E'
T'
top
Stack
search with [top, lookahead]

Page 12/32
Initial and Accepting Configuration
Initial configuration
⚫Stack: $ S
⚫Lookahead: points to the 1
st
input symbol 
and the input string ends with $
Accepting configuration
⚫Stack: $
⚫Lookahead: points to $

Page 13/32
Predictive Parsing Controller
initialize();
while(! stack.top().equals(new Symbol('$')) {
top = stack.top();
if(top instanceofTerminal) {  // matcha terminal
stack.pop();
match(top);
} else{  // derivea nonterminal
if (table[top, lookahead] isX →Y
1
Y
2
...Y
k
) {
output(X →Y
1
Y
2
...Y
k
);
stack.pop();
stack.push(Y
k
, Y
k-1
, ..., Y
1
);  // Y
1
on top
} else error();  // empty entry in parsing table
}
}
if (lookahead.equals(new Symbol('$'))  accept();
else  error();

Page 14/32
LL(1) Parsing: An Example
Given the following CFG
S→aB a
B→bB  |  
Non-
terminal
lookahead
ab$
SaB a
B

bB

Page 15/32
Parsing Process
Step
StackInputReferenceActionOutput
0$Sa b b a $[S, a] = aB aderive
S →aB a
1$aB aa b b a $match
2$aBb b a $[B, b] = bBderive
B →bB
3$aB bb b a $match
4$aBb a $[B, b] = bBderive
B →bB
5$aB bb a $match
6$aBa $
[B, a] = 
derive
B →
7$aa $match
8$$accept
Non-
terminal
lookahead
ab$
SaB a
BbB

Page 16/32
LL(1) Parsing: More Examples
Given an input string:n + n * n
Non-
terminal
lookahead
n+*()$
ET E'T E'
E'+T E'

TF T'F T'
T'

*F T'

Fn(E )

Page 17/32
Parsing Process
Step
StackInputReferenceActionOutput
0$En + n * n $[E, n] = T E'derive
E →T E'
1$E' Tn + n * n $[T, n] = F T'derive
T →F T'
2$E' T' Fn + n * n $[F, n] = nderive
F →n
3$E' T' nn + n * n $match
4$E' T'+ n * n $
[T', +] = 
derive
T' →
5$E'+ n * n $[E', +] = +T E'derive
E' →+T E'
6$E' T ++ n * n $match
7$E' Tn * n $[T, n] = F T'derive
T →F T'
8$E' T' Fn * n $[F, n] = nderive
F →n
Non-
terminal
lookahead
n+*()$
ET E'T E'
E'+T E'
TF T'F T'
T'*F T'
Fn(E )

Page 18/32
Parsing Process 
(cont')
Step
StackInputReferenceActionOutput
9$E' T' nn * n $match
10$E' T'* n $[T', *] = *F T'derive
T' →*F T'
11$E' T' F ** n $match
12$E' T' Fn $[F, n] = nderive
F →n
13$E' T' nn $match
14$E' T'$
[T', $] = 
derive
T' →
15$E'$
[E', $] = 
derive
E' →
16$$accept
Non-
terminal
lookahead
n+*()$
ET E'T E'
E'+T E'
TF T'F T'
T'*F T'
Fn(E )

Page 19/32
Error Report: Missing Operand
Step
StackInputReferenceActionOutput
0$En + * n $[E, n] = T E'derive
E →T E'
1$E' Tn + * n $[T, n] = F T'derive
T →F T'
2$E' T' Fn + * n $[F, n] = nderive
F →n
3$E' T' nn + * n $match
4$E' T'+ * n $
[T', +] = 
derive
T' →
5$E'+ * n $[E', +] = +T E'derive
E' →+T E'
6$E' T ++ * n $match
7$E' T* n $[T, *] = emptyerror
Missing operand

Page 20/32
Error Report: Missing Operator
Step
StackInputReferenceActionOutput
0$En n * n $[E, n] = T E'derive
E →T E'
1$E' Tn n * n $[T, n] = F T'derive
T →F T'
2$E' T' Fn n * n $[F, n] = nderive
F →n
3$E' T' nn n * n $match
4$E' T'n * n $[T', n] = emptyerror
Missing operator

Page 21/32
Step
StackInputReferenceActionOutput
0$En + n ( $[E, n] = T E'derive
E →T E'
1$E' Tn + n ( $[T, n] = F T'derive
T →F T'
2$E' T' Fn + n ( $[F, n] = nderive
F →n
3$E' T' nn + n ( $match
4$E' T'+ n ( $
[T', +] = 
derive
T' →
5$E'+ n ( $[E', +] = +T E'derive
E' →+T E'
6$E' T ++ n ( $match
7$E' Tn ( $[T, n] = F T'derive
T →F T'
8$E' T' Fn ( $[F, n] = nderive
F →n
9$E' T' nn ( $match
10$E' T'( $[T', (] = emptyerror
Missing operator

Page 22/32
Step
StackInputReferenceActionOutput
0$En + n ) $[E, n] = T E'derive
E →T E'
1$E' Tn + n ) $[T, n] = F T'derive
T →F T'
2$E' T' Fn + n ) $[F, n] = nderive
F →n
3$E' T' nn + n ) $match
4$E' T'+ n ) $
[T', +] = 
derive
T' →
5$E'+ n ) $[E', +] = +T E'derive
E' →+T E'
6$E' T ++ n ) $match
7$E' Tn ) $[T, n] = F T'derive
T →F T'
8$E' T' Fn ) $[F, n] = nderive
F →n
9$E' T' nn ) $match
10$E' T') $[T', *] = *F T'derive
T' →*F T'
11$E' T') $
[T', )] = 
derive
T' →
12$E') $
[E', )] = 
derive
E' →
13$) $error

Page 23/32
3. Error Recovery in LL(1) Parsing
What should the parser do in an error 
case? 
⚫Give an error message
Meaningful, as much as possible
⚫Recover from that error case
Be able to continue the parsing with the 
rest of the input

Page 24/32
Errors in LL(1) Parsing
Three types of errors
⚫Mismatch of terminals
Top of the stack is a terminal, but does not 
match with the lookahead. 
⚫[top, lookahead] = empty
There is no production candidate. 
⚫Empty stack with input remaining
The stack is empty, but the input string does 
not end.

Page 25/32
Parsing Table with Synchronization
Non-
terminal
lookahead
n+*()$
ET E'T E'synchsynch
E'+T E'

TF T'synchF T'synchsynch
T'

*F T'

Fnsynchsynch(E )synchsynch
FOLLOW(E) = FOLLOW(E') = { ), $ }
FOLLOW(T) = FOLLOW(T') = { +, ), $ }
FOLLOW(F) = { +, *, ), $ }
Drop the top
(less)
Drop the lookahead
(more)

Page 26/32
Parsing with Error Recovery
Error recovery strategies
⚫If [top, lookahead] = empty, then
Skip the lookahead, i.e. forward the input and 
keep the stack unchanged. 
⚫If [top, lookahead] = synch, then
Skip the top, i.e. pop the stack and keep the 
input unchanged. 
⚫If toplookahead, then
Skip the top, i.e. pop the stack and keep the 
input unchanged.

Page 27/32
Step
StackInputReferenceActionOutput
0$E* n * + n $[E, *] = emptyskip inputerror
1$En * + n $[E, n] = T E'derive
E →T E'
2$E' Tn * + n $[T, n] = F T'derive
T →F T'
3$E' T' Fn * + n $[F, n] = nderive
F →n
4$E' T' nn * + n $match
5$E' T'* + n $[T', *] = *F T'derive
T' →*F T'
6$E' T' F ** + n $match
7$E' T' F+ n $[F, +] = synchskip toperror
8$E' T'+ n $
[T', +] = 
derive
T' →
9$E'+ n $[E', +] = +T E'derive
E' →+T E'
10$E' T ++ n $match
11$E' Tn $[T, n] = F T'derive
T →F T'
12$E' T' Fn $[F, n] = nderive
F →n
13$E' T' nn $match
14$E' T'$
[T', $] = 
derive
T' →
15$E'$
[E', $] = 
derive
E' →
16$$end

Page 28/32
Error Recovery Techniques
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

Page 29/32
Exercise 5.1
Given the following grammar
S→(L )|  a
L→L ,S|  S
⚫Construct an LL(1) parsing table for the grammar
Note: you must eliminate the left recursion first. 
⚫Draw the detailed process of the parsing of the 
sentence (a, (a, a)), follow the style in the 
previous slides. 

Page 30/32
Exercise 5.2 
**
Given the following grammar
A→B  |  B C
B→aB  |  
C→ab
⚫Left factor the grammar. 
⚫After left factoring, is the grammar an LL(1) 
grammar? or is it an LL(k) grammar? and why? 
Note: you may try the input string ab. 

Page 31/32
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 4.4.3–4.4.4 for LL(1) parsing. 
Section 4.1.4 and 4.4.5 for error recovery in 
LL(1) parsing. 
⚫Skip Reading: 
Section 4.2.7 for differences between CFGs and 
regular expressions. 

Page 32/32
Enjoy the Course!
