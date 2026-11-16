# Lecture09.IntCodeGeneration

> 主题库：编译原理　｜　源文件：Lecture09.IntCodeGeneration.pdf　｜　18,728 字符　｜　53 个片段　｜　更新于 2026-11-11


©Copyright 2008-2023, Sun Yat-senUniversity
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
Principles of 
Compiler Construction

Page 2/53
Lecture 9.  Semantic Analysis and 
Intermediate Code Generation
1.Introduction
2.Types and Declarations
3.Assignments and Expressions
4.Type Checking
5.Boolean Expressions
6.Backpatching and Flow-of-Control 
Statements

Page 3/53
1. Introduction
Review
⚫Front end vs. back end
mn: mfront ends and nback ends. 
⚫Interface between front ends and back ends
Intermediate representation
Why IR? Extendabilityand optimization. 
⚫Semantic (static) analysis
The most common analysis
⚫Type checking
Other static checking
⚫Unreachable code
⚫Use of uninitialized variables
⚫etc.

Page 4/53
Static Checking
Semantic analysis also focuses on the 
well-formness of source code
⚫Due to the expressiveness power of Context-
Free Grammars. 
⚫For example, 
Number matching of actual parameters. 
⚫Context sensitive requirements cannot be 
specified using a context free grammar. 
break statement must be in a loop or switch. 
⚫Requires a complicated and unnatural context free 
grammar. 

Page 5/53
Intermediate Representation
High level intermediate representations
⚫AST and DAG
⚫Suitable for tasks like static type checking
Low level intermediate representations
⚫3-address code: x = y opz
⚫Suitable for machine-dependent tasks, such as 
register allocation and instruction selection. 
IR choice/design are application specific
⚫C language is commonly used (AT&T Bell Lab 
Advanced C++)

Page 6/53
Three-Address Code
Compiler-generated temporary variables
⚫x + y * z
⚫t
1
= y * z
t
2
= x + t
1
An example
⚫t
1
= b –c
t
2
= a * t
1
t
3
= a + t
2
t
4
= t
1
* d
t
5
= t
3
+ t
4
*
a–
+
+
*
cb
d

Page 7/53
Addresses
Addresses in 3-address code
⚫Name (variables in source code)
May be implemented as a pointer or reference 
to its entry in the symbol table.
⚫Constant
Type conversions must be considered. 
⚫Compiler-generated temporary
Useful for optimization. 
Register allocation. 

Page 8/53
Instructions
Common 3-address instructions
⚫x = y opz// arithmetic and logical
x = opy// negation and conversion
x = y// copy
⚫goto L// unconditional jump
ifx goto L// conditional jump
ifFalse x goto L// conditional jump
if x opy goto L// relational operation
⚫paramx
1
// parameter passing
paramx
2
...
paramx
n
call p, n// procedure call
y = call p, n// function call
return y// return a value

Page 9/53
Instructions (cont')
Common 3-address instructions
⚫x = y[i]// indexed copy, i is the offset
x[i] = y
⚫x = &y// address and pointer assignment
x = *y
*x = y

Page 10/53
Three-Address Code: Example
Source code
⚫do i = i + 1; 
while (a[i] < v);
Translation to 3-address code (symbolic labels)
⚫L:t
1
= i + 1
i = t
1
t
2
= i * 8
t
3
= a[t
2
]
ift
3
< v gotoL
Another translation form (position numbers)
⚫100:t
1
= i + 1
101:i = t
1
102:t
2
= i * 8
103:t
3
= a[t
2
]
104:ift
3
< v goto100

Page 11/53
Implementations of 
Three-Address Code
Quadruples (quads)
⚫Pros and cons? 
Triples
⚫Pros and cons? 
Indirect triples
⚫Pros and cons? 
Space consuming
Flexibility to optimizations

Page 12/53
1) Quadruples
Source code
⚫a = b * –c + b * –c
Three-address code
⚫t
1
= minus c
t
2
= b * t
1
t
3
= minus c
t
4
= b * t
3
t
5
= t
2
+ t
4
a = t
5
Quads
oparg
1
arg
2
result
0minusct
1
1*bt
1
t
2
2minusct
3
3*bt
3
t
4
4+t
2
t
4
t
5
5=t
5
a
......

Page 13/53
2) Triples
oparg
1
arg
2
0minusc
1*b(0)
2minusc
3*b(2)
4+(1)(3)
5=a(4)
......
Three-address code
⚫t
1
= minus c
t
2
= b * t
1
t
3
= minus c
t
4
= b * t
3
t
5
= t
2
+ t
4
a = t
5

Page 14/53
3) Indirect Triples
35(0)
36(1)
37(2)
38(3)
39(4)
40(5)
......
Three-address code
⚫t
1
= minus c
t
2
= b * t
1
t
3
= minus c
t
4
= b * t
3
t
5
= t
2
+ t
4
a = t
5
oparg
1
arg
2
0minusc
1*b(0)
2minusc
3*b(2)
4+(1)(3)
5=a(4)
......
In Java, array of 
instruction objects

Page 15/53
2. Types and Declarations
Declaration
⚫Literals: implicitly
⚫Variables: explicitly
⚫Other names: explicitly
Type checking in strong-typing languages
⚫Type compatibility
⚫Type inference
⚫Implicit type conversion
⚫Resolving overloading operators

Page 16/53
Simplified Grammar
Declare only one name at a time
D→T id ;D  |  
T→B C  |  record {D }
B→int|  double
C→[ num ]C  |  

Page 17/53
Translation of Type Declarations
Computing types and their widths
T→B{t = B.type;  w = B.width }
C{T.type = C.type;  T.width = C.width }
B→int{B.type = INTEGER;  B.width = 4 }
B→double{B.type = DOUBLE;  B.width = 8 }
C→[ num ]C
1
{C.type = array(num.value, C
1
.type); 
C.width = num.value C
1
.width }
C→{C.type = t;  C.width = w }
Type 
expression
Just try it: int[2][3]
What is T.typeand T.width? 

Page 18/53
Translation of Type Declarations (cont')
Computing relative addresses
P→{offset = 0 }
D
D→T id ;{top.put(id.lexeme, T.type, offset);
offset += T.width }
D
1
D→
Embedded actions can be 
removed with markers
top denotes the 
current symbol table

Page 19/53
Another Example
Enter types and their widths
P→{offset = 0 }
D
D→D ;D
D→id :T{table.enter(id.name, T.type, offset); 
offset += T.width }
T→integer{T.type = INTEGER;  T.width = 4 }
T→real{T.type = REAL;  T.width = 8 }
T→array[ num ]ofT
1
{T.type = array(num.value, T
1
.type); 
T.width = num.value T
1
.width }
T→^T
1
{T.type = pointer(T
1
.type);
T.width = 4 }
Just try it: k: array [5] of ^real
What are the side effects? 
DBv1

Page 20/53
Translation of Scoping
Declarations in nested procedures
P→{tableStack.push(newTable(null));
offsetStack.push(0) }
D{addWidth(tableStack.top(), offsetStack.top()); 
tableStack.pop();
offsetStack.pop() }
D→D ;D
D→proc id ;{tableStack.push(newTable(tableStack.top()));
offsetStack.push(0) }
D
1
;S{addWidth(tableStack.top(), offsetStack.top()); 
t = tableStack.top();  tableStack.pop();
offsetStack.pop(); 
tableStack.top().enter(id.name, t) }
D→id : T{tableStack.top().enter(id.name, T.type, offsetStack.top()); 
offsetStack.top() += T.width}
DBv1

Page 21/53
Translation of Scoping (cont')
Field names in records
T→record{tableStack.push(newTable(null));
offsetStack.push(0) }
D end{T.type = record(tableStack.top());
T.width = offsetStack.top(); 
tableStack.pop();
offsetStack.pop() }
DBv1

Page 22/53
3. Assignments and Expressions
Intermediate code generation
⚫Code concatenation
gen(...)
||
No side effects
⚫Incremental generation
DBv1: emit(...)
DBv2: overloading gen(...)
Side effects

Page 23/53
Translation of Expressions
Code concatenation (syntax-directed 
definition)
ProductionsSemantic Rules
1
S →id=E ;
S.code = E.code || gen(top.get(id.lexeme)  '='  E.addr)
2
E →E
1
+ E
2
E.addr = new Temp();
E.code = E
1
.code || E
2
.code || gen(E.addr  '='  E
1
.addr  '+'  E
2
.addr)
3
E →–E
1
E.addr = new Temp();
E.code = E
1
.code || gen(E.addr  '='  'minus'  E
1
.addr)
4
E →(E
1
)
E.addr = E
1
.addr;
E.code = E
1
.code
5
E →id
E.addr= top.get(id.lexeme);
E.code= ""

Page 24/53
Translation of Expressions (cont')
Incremental translation (translation 
scheme)
S→id =E ;{gen(top.get(id.lexeme)  '='  E.addr) }
E→E
1
+E
2
{E.addr = new Temp();
gen(E.addr  '='  E
1
.addr  '+'  E
2
.addr) }
E→–E
1
{E.addr = new Temp();
gen(E.addr  '='  'minus'  E
1
.addr) }
E→(E
1
){E.addr = E
1
.addr }
E→id{E.addr = top.get(id.lexeme) }

Page 25/53
Another Example
Declared variables
S→id :=E ;{p = symbolTable.lookup(id.name);
if(p == null)  throw new SomeException(); 
emit(p'='  E.place) }
E→E
1
+E
2
{E.place = new Temp();
emit(E.place  '='  E
1
.place  '+'  E
2
.place) }
E→–E
1
{E.place = new Temp();
emit(E.place  '='  'minus'  E
1
.place) }
E→(E
1
){E.place = E
1
.place }
E→id{p = symbolTable.lookup(id.name);
if(p == null)  throw new SomeException(); 
E.place = p}
DBv1

Page 26/53
Addressing Array Elements
2-dimensional array layout
⚫Row major vs. column major
A[1, 1]
A[1, 2]
A[1, 3]
A[2, 1]
A[2, 2]
A[2, 3]
1st row
2nd row
A[1, 1]
A[2, 1]
A[1, 2]
A[2, 2]
A[1, 3]
A[2, 3]
1st column
2nd column
3rd column

Page 27/53
Addressing Array Elements
Relative address of array elements
⚫A[i]
base + (i–low) w
iw + (base –low w)
⚫A[i
1
, i
2
]
base + ((i
1
–low
1
) n
2
+ i
2
–low
2
)w
((i
1
n
2
) + i
2
)w +(base –(low
1
n
2
+ low
2
)w)
⚫A[i
1
, i
2
, ..., i
k
]
((...((i
1
n
2
+ i
2
)n
3
+ i
3
)...) n
k
+ i
k
) w +
base –((...((low
1
n
2
+ low
2
)n
3
+ low
3
)...)n
k
+ low
k
)w
Constant
for optimization

Page 28/53
Addressing Tips
For each increment of a new dimension, 
addressing is calculated recursively, e.g. 
from k to k + 1
⚫For variable part V:V n
k+1
+ i
k+1
⚫For constant part C:C n
k+1
+ low
k+1

Page 29/53
Grammar for Array References
Array references in Pascal: a[2, 3]
⚫L→id [ Elist]|  id
⚫Elist→Elist,E  |  E
Grammar transformation (why?)
⚫L→Elist]|  id
⚫Elist→Elist,E  |  id [ E
L
Elist]id[
Elist,E
E
L
Elist]id[
Elist,E
Eid[

Page 30/53
Translation Scheme
Addressing array elements in Pascal
(1) S→L :=E{if(L.offset == null)emit(L.place  '='  E.place)
else emit(L.place  '['  L.offset  ']'  '='  E.place) }
(2) E→E
1
+E
2
{E.place = new Temp();
emit(E.place  '='  E
1
.place  '+'  E
2
.place) }
(3) E→( E
1
){E.place = E
1
.place }
(4) E→L{if(L.offset == null)  E.place = L.place
else {
E.place = newTemp();
emit(E.place  '='  L.place  '['  L.offset  ']')
} }
(5) L→Elist ]{L.place = new Temp();
emit(L.place  '='  constant(Elist.array));
L.offset = new Temp();
emit(L.offset  '='  Elist.place  '*'  width(Elist.array) }
(6) L→id{L.place = id.place;
L.offset = null}
DBv1
L is a simple id (if L.offset is null)
or an array reference
L.place= base –C * w
L.offset= V * w

Page 31/53
Translation Scheme (cont')
Addressing array elements in Pascal (cont')
(7) Elist→Elist
1
,E{t = new Temp();
m = Elist
1
.ndim + 1;
emit(t  '='  Elist
1
.place  '*'  limit(Elist
1
.array, m));
emit(t  '+='  E.place);
Elist.array = Elist
1
.array;
Elist.place = t;
Elist.ndim = m }
(8) Elist→id[E{Elist.array = id.place;
Elist.place = E.place;
Elist.ndim = 1 }
DBv1
Elist.array= base
Elist.place= V 
Elist.ndim= dimensions

Page 32/53
Another Translation Scheme
Array references in C/C++: a[2][3]
⚫For all n, low
n
= 0
⚫Addressing formula
A[i]
⚫base + iw
A[i
1
][i
2
]
⚫base + i
1
w
1
+ i
2
w
2
⚫w
1
is the width of a row
⚫w
2
is the width of an element in a row
A[i
1
][i
2
]...[i
k
]
⚫base + i
1
w
1
+ i
2
w
2
+ ... + i
k
w
k
Java does NOT use
row-major storage for arrays
DBv2

Page 33/53
Another Translation Scheme (cont')
Translation scheme
S→id =E ;{gen(top.get(id.lexeme)  '='  E.addr) }
S→L =E ;{gen(L.array.base'['  L.addr']'  '='  E.addr) }
E→E
1
+E
2
{E.addr= new Temp();
gen(E.addr'='  E
1
.addr  '+'  E
2
.addr) }
E→id{E.addr= top.get(id.lexeme) }
E→L{E.addr= new Temp();
gen(E.addr'='  L.array.base'['  L.addr']') }
L→id [E ]{L.array= top.get(id.lexeme);
L.type= L.array.type.element;
L.addr= new Temp();
gen(L.addr'='  E.addr'*'  L.type.width) }
L→L
1
[E ]{L.array= L
1
.array;
L.type= L
1
.type.element;
t = new Temp();
L.addr= new Temp();
gen(t  '='  E.addr'*'  L.type.width);
gen(L.addr'='  L
1
.addr  '+'  t) }
L only for array reference
E.addr = E.place
L.array.base = L.place
L.addr = L.offset
DBv2

Page 34/53
DBv2

Page 35/53
4. Type Checking
Strong typing vs. weak typing
⚫Strongnessis relative
Type definitions
⚫Primitive types: enumeration of constant
⚫Composite types: type expressions
⚫Type of functions: signatures
if fhas type s →tand xhas type s
then expression f(x)has type t

Page 36/53
Translation Scheme: An Example
Type checking, inference and implicit casting
E→E
1
*E
2
{E.place := new Temp();
if(E
1
.type == TK_INT && E
2
.type == TK_INT) {
emit(E.place  '='  E
1
.place  '*
int
'  E
2
.place);
E.type = TK_INT;
} elsif(E
1
.type == TK_REAL &&E
2
.type == TK_REAL) {
emit(E.place  '='  E
1
.place  '*
real
'  E
2
.place);
E.type = TK_REAL;
} elsif(E
1
.type == TK_INT &&E
2
.type == TK_REAL) {
t := new Temp();
emit(t  '='  'int2real'  E
1
.place);
emit(E.place  '='  t  '*
real
'  E
2
.place);
E.type = TK_REAL;
} elsif(...) {  ...  }
}

Page 37/53
5. Boolean Expressions
Boolean expressions are used in
⚫Flows of control
⚫Computing logical values

Page 38/53
Computing Logical Values
a < bequals to if (a < b) then 1 else 0
E→E
1
orE
2
{E.place = new Temp();
emit(E.place  '='  E
1
.place  'or'  E
2
.place) }
E→E
1
andE
2
{E.place = new Temp();
emit(E.place  '='  E
1
.place  'and'  E
2
.place) }
E→notE
1
{E.place = new Temp();
emit(E.place  '='  'not'  E
1
.place) }
E→(E
1
){E.place = E
1
.place }
E→id
1
relop id
2
{E.place = new Temp();
emit('if'  id
1
.place  relop.op  id
2
.place  'goto'  currentStmt+3);
emit(E.place  '='  '0');
emit('goto'  currentStmt+2);
emit(E.place  '='  '1') }
E→true{E.place = new Temp();
emit(E.place  '='  '1') }
E→false{E.place = new Temp();
emit(E.place  '='  '0') }
DBv1

Page 39/53
Computing Logical Values: An Example
Source code
⚫a < b orc < d ande < f
Intermediate code
100:ifa < b goto103108:ife < f goto111
101:t
1
= 0109:t
3
= 0
102:goto104110:goto112
103:t
1
= 1111:t
3
= 1
104：ifc < d goto107112:t
4
= t
2
andt
3
105:t
2
= 0113:t
5
= t
1
ort
4
106:goto108114:...
107:t
2
= 1
DBv1

Page 40/53
Short-Circuit Evaluation
Flow-of-Control Statements
⚫S→if  (B  )S
1
⚫S→if  (B  )S
1
elseS
2
⚫S→while  (B  )S
1
Short-circuit evaluation for &&and ||
⚫For higher evaluation efficiency
⚫And ...

Page 41/53
Generated Code Illustration
B.code
S
1
.code
→B.true
→B.false
B.true: 
B.false: 
...
B.code
S
1
.code
→B.true
→B.false
B.true: 
B.false: 
S
2
.code
gotoS.next
...
S.next: 
B.code
S
1
.code
→B.true
→B.false
B.true: 
B.false: 
gotobegin
...
begin: 
if
if-elsewhile

Page 42/53
Syntax-Directed Definition for 
Flow-of-Control Statements
ProductionsSemantic Rules
P  →S
S.next = new Label();
P.code = S.code  ||  label(S.next)
S  →assign
S.code= assign.code
S  →S
1
S
2
S
1
.next = newLabel();
S
2
.next = S.next;
S.code = S
1
.code  ||  label(S
1
.next)  ||  S
2
.code
S  →if (B )S
1
B.true = new Label();
B.false = S
1
.next = S.next;
S.code = B.code  || label(B.true)  ||  S
1
.code
S  →if (B )S
1
elseS
2
B.true = new Label();
B.false = new Label();
S
1
.next = S
2
.next = S.next;
S.code = B.code  || label(B.true)  ||  S
1
.code  ||  
gen('goto'  S.next)  ||  label(B.false)  ||  S
2
.code
S  →while (B )
S
1
begin = new Label();
B.true= new Label();
B.false= S.next;
S
1
.next = begin;
S.code= label(begin) || B.code|| label(B.true) || S
1
.code || gen('goto'  begin)
Where does
S.nextcome from? 
Avoid redundant
gotos

Page 43/53
Syntax-Directed Definition for Booleans
ProductionsSemantic Rules
B  →B
1
||B
2
B
1
.true = B.true;
B
1
.false = new Label();
B
2
.true = B.true;
B
2
.false = B.false;
B.code = B
1
.code  ||  label(B
1
.false)  ||  B
2
.code
B  →B
1
&&B
2
B
1
.true = new Label();
B
1
.false = B.false;
B
2
.true = B.true;
B
2
.false = B.false;
B.code = B
1
.code  ||  label(B
1
.true)  ||  B
2
.code
B  →!B
1
B
1
.true = B.false;
B
1
.false = B.true;
B.code = B
1
.code
B  →E
1
relopE
2
B.code = E
1
.code  ||  E
2
.code  
||  gen('if'  E
1
.addr  relop.op  E
2
.addr  'goto'  B.true)
||  gen('goto'  B.false)
B  →true
B.code = gen('goto'  B.true)
B  →false
B.code = gen('goto'  B.false)
Short-Circuit
Evaluation

Page 44/53
Syntax-Directed Translation: 
An Example
Source code
⚫if(x < 100 || x > 200 && x != y)  x = 0
Intermediate code
ifx < 100 gotoL
2
gotoL
3
L
3
:ifx > 200 gotoL
4
gotoL
1
L
4
:ifx != y gotoL
2
gotoL
1
L
2
:x = 0
L
1
:...

Page 45/53
6. Backpatching and 
Flow-of-Control Statements
In SDD for Flow-of-Control Statements
⚫Where does S.nextcome from? 
⚫Only after all intermediate code are generated, 
can S.nextbe computed. 
In SDD for Booleans
⚫Where do B.trueand B.falsecome from? 
⚫Must be provided by the context of the boolean
expressions. 
⚫The context depends on the result of S.next. 

Page 46/53
Design Motivation and Solution
Motivation
⚫One-pass code generation
Solution
⚫Using backpatching
It is a general approach to dealing with 
initial values which must be computed at 
the end. 

Page 47/53
Backpatching for 
Boolean Expressions
Translation scheme
B→B
1
||M B
2
{backpatch(B
1
.falseList, M.instruction); 
B.trueList = merge(B
1
.trueList, B
2
.trueList);
B.falseList = B
2
.falseList; }
B→B
1
&&M B
2
{backpatch(B
1
.trueList, M.instruction); 
B.trueList = B
2
.trueList;
B.falseList = merge(B
1
.falseList, B
2
.falseList); }
B→!B
1
{B.trueList = B
1
.falseList;
B.falseList = B
1
.trueList; }
B→(B
1
){B.trueList = B
1
.trueList;
B.falseList = B
1
.falseList; }
B→E
1
relopE
2
{B.trueList = new List(nextInstruction);
B.falseList = new List(nextInstruction + 1);
emit('if'  E
1
.addr  relop.op  E
2
.addr  'goto__');
emit('goto__'); }
B→true{B.trueList = new List(nextInstruction);
emit('goto__'); }
B→false{B.falseList = new List(nextInstruction);
emit('goto__'); }
M→{M.instruction = nextInstruction; }

Page 48/53
Backpatching for 
Flow-of-Control Statements
Translation scheme
S→if(B )M S
1
{backpatch(B.trueList, M.instruction); 
S.nextList = merge(B.falseList, S
1
.nextList); }
S→if(B )M
1
S
1
N elseM
2
S
2
{backpatch(B.trueList, M
1
.instruction); 
backpatch(B.falseList, M
2
.instruction);
S.nextList = merge(S
1
.nextList, N.nextList, S
2
.nextList); }
S→whileM
1
(B )M
2
S
1
{backpatch(B.trueList, M
2
.instruction);
backpatch(S
1
.nextList, M
1
.instruction);
S.nextList = B.falseList;
emit('goto'  M
1
.instruction); }
S→{L }{S.nextList = L.nextList; }
S→A ;{S.nextList = newList();  // Assignment or Atom }
M→{M.instruction = nextInstruction; }
N→{N.nextList = newList(nextInstruction);
emit('goto__'); }
L→L
1
M S{backpatch(L
1
.nextList, M.instruction);
L.nextList = S.nextList; }
L→S{L.nextList = S.nextList; }

Page 49/53
Exercise 9.1
Let Abe declared as a [2..4] x [1..5] array 
of integers and each integer occupy 4 bytes. 
What is the translation result of input 
token string: x := A[3, 2]? 
⚫Tips: use the translation scheme for Pascal. 
DBv1, pp.484-485

Page 50/53
Exercise 9.2
Let abe declared as a 5 x 6 array of 
integers and each integer occupy 4 bytes. 
What is the translation result of input 
token string: i= a[3][2]? 
⚫Tips: use the translation scheme for C/C++.
DBv2, pp.383

Page 51/53
Exercise 9.3
What is the translation result of input 
token string: x < 100 || x > 200 && 
x != y? 
⚫Tips: use the translation scheme for boolean
expressions with backpatching.
⚫Suppose that the start position of the 
generated code is 100. 
DBv2, pp.411-416

Page 52/53
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 6.2 on introduction to intermediate 
representations. 
Section 6.5 on type checking. 
Section 6.3, 6.4, 6.6 and 6.7 on translations of 
various program constructs. 
⚫Skip Reading: 
Section 6.1 on AST and DAG. 
Section 6.8 and 6.9 on translations of switches 
and procedures. 

Page 53/53
Enjoy the Course!
