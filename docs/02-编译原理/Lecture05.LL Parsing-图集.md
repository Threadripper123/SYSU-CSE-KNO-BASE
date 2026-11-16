# Lecture05.LL Parsing-图集

> 主题库：编译原理　｜　源文件：Lecture05.LL Parsing-图集.pdf　｜　1,426 字符　｜　5 个片段　｜　更新于 2026-11-11
大，
2A

Parsing Table:An Example O F For the previous grammar F  广- Non- looahad terminal n + * ( ) $ ET E''T E'' E' '+TE'' ε ε TFT'' FT'' T'' E *F T'' E E F n (E)

Non- lookahead terminal a b $ S aBa Parsing PrOCess B  bB Step Stack Input Reference Action Output $ S a b b a $ [S,a] = aB a derive S → aB a 1 $aBa abba$ match 2 $aB bba$ [B,b] = bB derive B → bB 3 $aBb bba$ match 4 $ a B ba $ [B, b] = b B derive B → b B 5 $aBb ba$ matchl   6 $ aB a$ [B, a]= ε derive B → ε     7  $ a a$ match   8 $ $ accept Page15/32

Non- 10ke2( terminal n E TE TE E +T E Parsing PrOCess FT F ε *FT ε (E) Step Stack Input Reference Action Output 0 $E n + n * n $ [E, n] = TE'' derive E→ TE'' 1 $ E''T n + n * n $ [T, n] = F T'' derive T → F T'' 2 $ E''T''F n + n * n $ [F, n] = n derive F → n 3 $ E''T''n n + n * n $ match 4 $E''T'' + n * n$ [T'',+ ] = ε derive T''→ ε 5 $ E'' + n * n $ [E'', +] = + T E'' derive E'' → + T E'' 6 $E''T + + n * n $ match 7 $ E''T n * n $ [T, n] = F T'' derive T → FT'' 8 $E''T''F n * n $ [F, n] = n derive F→n [aaee

Non- 1ookahead Parsing terminal ProceSS " + * (  $ E TE TE E +TE E (CONt'') FT *FT ε FT εε F n (E) step Stack Input Reference Action Output 9 $E''T''n n*n$ match 10 $E''T'' *n$ [T'',*]=*FT'' derive T''→* FT'' 11 $E''T''F* *n$ match 12 $E''T''F n $ [F, n] = n derive F → n 13 $E''T''n n$ match 14 $ T'' $ [T'', $] = ε derive T'' → ε E'' 155 E'' [E' ',$]= ε derive E''→ε 16 $ $ accept Page18/32
