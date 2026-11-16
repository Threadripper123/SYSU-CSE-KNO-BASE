# Lecture08.SyntaxDirectedTranslation-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture08.SyntaxDirectedTranslation-图集-矢量.pdf　｜　5,686 字符　｜　41 个片段　｜　更新于 2026-11-13
syi ax-uinecteu: D int k 9.

A A.S A→B B B.I V A.S = B.i; B.i = A.S + 1 |

semantic attributes VM actions

Productions SemanticRules 1 L.Val = E.val L → En Side-effects 2 E →  E1 + T E.val = E1.Val + T.val 3 E · → T E.val = T.val 4 T1 → T2 * F T1.Val = T2. Val * F.val 5 T → F T.val = F.val 6 F →(E ) F.val = E.Val 7 F → c d digit F.val = digit.lexval ubfix stle

T.val=4 F.val=4 5 digit.lexval

=15 =15 F.val=5 digit.lexval

Taval Tval-3 VAl=3 it.lexval= 3

Synthesized attributes

= REAL II L.inh = F i 1 eal Linh=REAL

Y.node= 6 E.node 1 + T.node =  5 id node Tnode= 3 id  i d um n anewLeafid,entry-a node: num  = | : new Leaf(num, 4) =| neW Node(-'', P1, P2) id = new Leaf(id, entry-c) = neW Node(+'', P3,P4)

d a b C

Work with explicit parse tree

5 6 = REAL L.inh = REAL cYa 8'' eal L.inh = REAL id₃ 3 9 ￥ 10 L.inh=REAL id2 不：

S-Attributed Definition Evaluation while LR Parsing

Implementation in LR Parsing  1 stack[ntop].val = stack[top − 1].val; L → En  print(stack[ntop].val); 2 E : → ntop].val = stack[top E1 + T stack[ − 1].val = ''+'' − 2].val + stack[top].val; stack[top 3 E→T

Step States Attributes Input Code 0utput (Illustrative) 1 $ $ 3 *5 +4 n$ 2 $ 3 $3 *5 +4 n$ __ F→digit 3 $F $3 *5+4n$ __ T→F 4 $T $3 *5 +4n$ 5 $T* $3* 5+4n$ 6 $T*5 $3 *5 +4n$ __ F→digit 7 $T*F $3*5 +4n$ 3*5 T→T*F 8 $ T $15 +4 n$ _ E →T 9 $E $15 +4n$ 10 $E + $ 15 + 4 n $ 11 $ E + 4 $ 15 + 4 n $ _ F → digit 12 $E +F $15 + 4 n $ __ T →F 13 $E +T $15 +4 n $ 15+4 E→E+T 14 $E $19 n $ 15 $E n $19 n $ print(19) L→ E n 16 $L $ 19 $ accept $

for (each child m of n, from left to right) { evaluate inherited attributes of m; dfvisit(m) evaluate synthesized attributes of n

L-Attributed Definition S Translation Scheme

→ E n { print(E.val); } E → E1 + T { E.val = E1.Val + T.val; } E → T { E.val = T.val; } T → T, * F { T.val = T .Val * F.val; } T → F { T.val = F.val; } F → (E ) { F.val = E.val; } F → digit { F.val = digit.lexval; }

→ En {print(stack[top — 1]val); top=top—1;} E → E1 + T { stack[top − 2].val = stack[top − 2].val + stack[top].val; top = top − 2; } T → T, * F { stack[top − 2].val = stack[top − 2].val * stack[top].val; top = top - 2; } T→F F → (E) {stack[top − 2].val = stack[top − 1].val; top = top − 2; } → digit

E → T R R → addop T { print(addop.lexeme); }  R ε T → num { print(num.val);}

En E {print(+);} E1 +T E → T T {pint(* );} T1*F T F F (E) F → digit { print(digit.lexval); } Pag

Does not satisfy the requirements: S A₁ A2 { A .in = 1; A2.in = 2; } A → a { print(A.in); }

No. Productions Semantic Actions 1 S → B B.pS = 10 B1.ps : = B.ps ht = height 2 B2.ps: B.ps dp = depth B → B1B2 = bht= max(Bht,B2ht B.dp= = max(B1.dp,B2.dp) B1.pS = B.pS 3 B2·ps = | B.pS ×70% B→B1sub B2 B.ht : = max(B1.ht, B2.ht − B.ps × 25%) Higher precedence, B.dp : = max(B1.dp, B2.dp + B.ps × 25%) rightassociativity B1.pS = : B.pS

S → {B.ps =10;} B B { B1.pS = B.pS; } B1 {B2.pS = B.pS;} B2 { B.ht = max(B1.ht, B2.ht); B.dp = max(B1.dp,B2.dp);} B { B1·pS = B.pS; } B1sub {B2.ps =B.ps×70%;} B2 {B.ht = max(B1.ht, B2.ht− B.ps × 25%); B.dp = max(B1.dp,B2.dp+ B.ps×25%);} B { B1·pS = B.pS; } B1) {B.ht=B1.ht; B.dp = B1.dp;} B → text  { B.ht = getHight(B.ps, text.lexval); B.dp = getDepth(B.ps, text.lexval); }

E → TR R → + T { print(+); } R R ε

E → E1 + T { print(''+''); } E T

TOTCCOTPTCCC E : → E₁ + T { E.Val = E1.Val + T.val; } E → E1 − T { E.val = E₁.Val − T.val; } E → T { E.val = T.val; } T → (E ) { T.val = E.val; } T → num { T.val = num .val; } E → T { R.i = T.val; } R { E.val= R.s; } R → + T { R .i = R.i + T.val; } 1 { R.S = R .S;} R i → −T { R.i = R.i−T.val;} {R.S=RS;} R → ε { R.S = R.i; } T → (E) { T.val = E.val; } T { T.val = num.Val; } num Page· 4

= 9 R.i= [Tmab a l

1=9 al=5 al=5 Tval

[=4 al=2 R.i= val=2

A A1Y { A.a = g(A .a, Y.y); } A − X { A.a = f(X.x); } A → X { R.i = f(X.x); } R { A.a = R.s; } R → Y { R₁.i = g(R.i, Y.y); } R 1 { R.S = R1.S; } R → ε { R.S = R.i; }

R → addop T { Rj.i = mknode(addop.lexeme, R.i, T.nptr); } R1 { R.S = R .S; } R → ε { R.S = R.i; } void R() { if(lookahead== addop) { match(addop); I() R() } elSE { // do nothing

R →→ addop T { Rj.i = mknode(addop.lexeme, R.i, T.nptr); } {R.S=R ;} R → ε { R.S = R.i; }  SyntaxTreeNodeR(SyntaxTreeNodei)  SyntaxTreeNodes; //synthesizedattributes SyntaxTreeNode t nptr, rl i, rl s; // for children char addopLexeme;: // temporary  if (lookahead== addop) addopLexeme=lookahead.lexval; match(addop); tnptr= T(); rl_i=mknode(addopLexeme, i, t_nptr); rl s = R(rl i); S= rl S; }else S=i; returnS Page 51/

PiaccToiaci. E → TR R + T { print(+ ); } R  E T → num { print(num.val); }  E TR R → +T MR E T → num { print(num.val); } M 1 ε {print(+ );} N ε {print(—); }

D→T T →  int {T.type = INTEGER; } T → real { T.type = REAL; } L L1, id { addType(id.entry, L.inh); } L → id { addType(id.entry, L.inh); }

D T T → int {T.type = INTEGER; } T → real { T.type = REAL; } L1, id { addType(id.entry, L.inh); } L → id { addType(id.entry, L.inh); }

S aA C S → bAB { ; } C C → c { C.s = g(C.i); } S→aA { ; } S → b A B { M.i = A.S; } M { M.S; } C C → c { C.s = g(C.i); } M  ε { M.S = M.i; } Page 57/63

S → a A { M.i = A.S; } M { M.S;} C → c { C.s = g(C.i); }  M → ε { M.S = f(M.i); }

S → aA { ; } C → c { C.s = g(C.i); }

Productions SemanticActions S→ KB B.pS= K.S K →ε K.s = 10 Anytime when B is reduced, B1.ps = B.ps B.ps is immediately under B Li=B.pS B→B1 B2 B2.pS = S B.ht = max(B1.ht, B2.ht) B.dp= max(B1.dp,B2.dp) L→ ε L.S = L.i B1.pS= B.pS a→B1subMB2 B2_pS=M__ B.ht = max(B₁.ht, B,.ht − B.ps × 25%) B.dp = max(B1.dp, B2.dp + B.ps × 25%) M→ε M.s = M.i×70% B1.pS = B.pS B→(NB1) N.i= B.pS B.ht = B.ht B.dp = B1.dp N→ ε N.S = N.i B.ht= getHight(B.ps, text.lexval a→et B.dp = getDepth(B.ps, text.lexval) 53
