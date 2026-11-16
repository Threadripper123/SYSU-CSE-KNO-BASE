# Lecture04.TopDownParsing-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture04.TopDownParsing-图集-矢量.pdf　｜　1,540 字符　｜　23 个片段　｜　更新于 2026-11-13
eamof parse kens tree N Parser

aYnnactC RULES eam of parse pkens General ree ParSER

syntactic RULES Gaenerator (e.g. yacc & JavaCUP) eam of parse kens Specific tree ParsEr

array [ num dotdot num l of integer

Two orthogonal perspectives

Grammar Transformation

Wby?

Decision Making 1

Overloading

SymboI FIRST FOLLOW A ×Y B ×Yε Y C Y

E:—>— E F>8T> 2 T_ 7 E: ③ >(>5 E'' +>4 6 11 F >-12 13 T T O F: 4—— 15 E>——— — 7

Reduction of Transiti  Ad hoc rules: iterative sL E: 3 4 5 E 1 6 さ

E' ' :3 ±4 T 5

T K E' ': 3± 4

T E: O T 3 4

E 0 T 3 ε 6

E: ① >①E>②

E: 0 T 3 ε 6 T: 7 F V (10. ε (13) F: (14 (V15 E> (6 17

void A(){ if(lookahead == a) { match(a); B(); match(b) }∈ else if (lookahead == b){ match(b); A( ); B( ) } else C(); } void A() { if (lookahead == a) { match(a) B( ); match(b) }else if(lookahead== b){ match(b); A( ); B( ) }é else if (lookahead in FIRST(C)) { C ( ) }else errr();

voiaA(){ if(lookahead == a) { match(a); B(); match(b) } else if (lookahead == b) { match(b); A( ); B() } else ; //do nothing voidA(){ if (lookahead == a) { match(a); B(); match(b) } é  ∈ else if (lookahead == b) { match(b); A( ); B( ) } else if (lookahead in FOLLOw(A)) { // do nothing, more accurate! }é else error();

type si mple | array [ simple] of type ha 一 num dotdotnum

E: 3 E×ample2 T_ F、 10 ε/ >③ n voidexpr() throws  SyntacticException { term(); while (lookahead.eguals(new ''Token(''+''))) { term(); void term (l) throws S SyntacticException { factor(); while(l ookahead.equals(new Token(''*''))) { factor();

F: :4 >15 >16→ ① cYn
