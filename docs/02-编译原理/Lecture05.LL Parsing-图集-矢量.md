# Lecture05.LL Parsing-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture05.LL Parsing-图集-矢量.pdf　｜　813 字符　｜　6 个片段　｜　更新于 2026-11-13
Non- lookahead terminal n + * ( ) $

[No oaa terminal if then else other 「  expr  - $ Si fEthenSS''o ther

Input n + n * n  lookahead c T  Predictive Parsing Logical vs. Physical Controller Stack search with [top, lookahead]

Non- lookahead terminal a b $

0 $ S e S → a B a  a b b a $   [S, a] = a B a   derive  1 $aBa abba$ match 2 $aB b ba $ [B,b] = bB derive B → bB  3 $aBb bba$ matchi 4 $ a B b a $ [B, b] = b B derive    B → b B   5 $a B b ba$ match 1 6  $ aB a$  [B,a]= ε derive B→ε    7 $a a$ match 8 $ $ accept

Error Report:Missing Operator Step Stack Input Reference Action Output 0 $ E n n * n $ [E, n] = TE'' derive E →T E'' 1 $E''T n n * n $ [T, n] = = F T'' derive T → F T'' 2 $ E''T''F n n * n $ [F, n] = n derive F → n 3 $E''T''n nn*n$ match 4 $ E''T'' n*n$ [T'', n]=empty error  Missing operator
