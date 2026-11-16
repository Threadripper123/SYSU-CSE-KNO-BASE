# Lecture9-作业讲解课-图集

> 主题库：人工智能　｜　源文件：Lecture9-作业讲解课-图集.pdf　｜　6,384 字符　｜　40 个片段　｜　更新于 2026-11-11
中大學 SUAN

bagg x，y表示盘子比盘子y大 abouex，y)表示盘子在盘子y上方 move(x,y，z)表示将盘子x从柱子y移动到柱子z

(1)S = P (a, x, f (g(y ))), P(z, h(z, u), f (u)) <1>δ0 = ∈, W0 = S,D0 = { a,z } <2> 令δ1 = {a/ z}, W1 = {P(a, x, f (g(y))), P (a, h(a, u), f (u))} 3>W1未合—， D1 ={x,h(a,u)} <4>δ2 = {a/ z, h(a, u)/x}, W 2 = { P(a, h(a.u), f (g(y))), P (a, h(a, u), f (u))} <5>W2未合—,D2=(g(y)，u)

(2)S = { P(f(a), g(s)),P(y,y )} <1>δ0 = ∈, W 0 = S, D 0 = { f (a), y } <2> 令δ1 = { f (a)/y }, W1 = { P( f (a), g(s)), P (f (a), f (a))} <3>W₁未合一，D1={f(a)，g(s)}，无变量符号，故S不可合一

(3)S = {P(a, x, h(g(z))), P(z, h(y), h(y))} <1>δ0 = ∈, W0 = S, D 0 = {a, z } <2> 令δ1 = {a/ z}, W1 = { P(a, x, h(g(a))), P(a, h(y), h(y))} <3>W1未合—,D1={x,h(y)} <4>δ2 = {a/ z, h(y)/x}, W2 = {P(a, h(y), h(g(a))), P(a, h(y), h(y))} 5>W2未合 —,D2={g(a)，y } <6>δ3 = {a/ z, h(g(a)/x, g(a)/y}, W₃ = {P(a, h(g(a)), h(g(a)))} 故δ₃={a/z,h(g(a)/x,g(a)/y}为s的最—般合—。

brother(x,y)： 表示x是y的兄弟 sisiter(x，y)： 表示x是y的姐妹 woman(x)：表示x是女性

S = S1∪ S2 ∪ S3 ∪S—4 = {−brother (x, y) √ −woman (x) , ¬sister (x, y) V woman (x),

(1)—brother(x,y)√—woman(x) a —sister(x,y)Ywoman(x 3)sister(Mary,Bill 4brother(Mary,Tome) (1)与(4)归结, σ = {Mary/x, Tom/y} ⇒ (5)¬woman(Mary) (2)与(3)归结, σ = {Mary/x, Bill/y} ⇒ (6)woman(Mary) (5)与(6)归结 ⇒（7)NIL

likes(x，y)：表示x喜欢y steal(x, y)： 表示x可能会偷窃y

(1)thief(John) (2)likes(Paul, wine) (3)likes(Paul, cheese) (4)—likes( Paul, y) V likes(John, y) (5)—thief(x) √ −likes(x, y) √ steal(x, y) (6)—steal(John, wine) (7) steal(John, cheese)

(5) 与 (6)归结 , σ = {John/x, wine/y} → (8)¬thief(John) √ ¬likes(John, wine) (1)与(8)归结 (9)−likes(John,wine) (4)与(9)归结， σ ={wine/y}→(10)¬likes(Paul, wine) (2)与(10)归结 N I L

(5)与(7)归结, σ = {John/x, cheese/y} → (11)—thief(John) √ ¬likes(John, cheese) )与(11)归结 (12) likes(John,cheese) (4)与(12)归结， σ = {cheese/y} → (13)¬likes(Paul, cheese) (3) 与(13)归结 ⇒ N I L

study(x)表示x肯学习 win(x)表示x中彩 lucky(x)表示x幸运 happy(x)表示x快乐 pass(x, y)表示x通过考试y

) pass(x,history)√—win(x)√happy(x) (2)−study(x) √ pass(x, y) (3)−lucky(x) √ pass(x, y) (4)−study(zhang) (5)lucky(zhang) (6)—lucky(x) V win(x) (7)—happy(zhang) (1)与(7)归结 ， σ = {zhang/x} → (8)一pass(zhang, history) √ ¬win(zhang) (5)与(6)归结，σ={zhang/x}→(9)win(zhang) (8)与(9)归结 (10)一pass(zhang,history)

S (7) 4 10/ 11 A (8) 8 C B (5) 5 (6) 8 15 20 2/ E D (3) 16 (5) 1 H 20 19 F 7) 1 (3) 137 1 3 (4) G 2 5 13 (0)  16 J 7 K (5) (3)

Node[cost] Closed List 4 10 11 H[32] A E[36] D F A B C (8) (6) (5) B 8 8 5 15 20 2 C  G D D E F B D (5) (5) (3) (3) (6) (5) 16 20 1 H 一 F (7) (4) (3) 13 G

3 17 2 12 15 25   0 2 5  3 2 14

MA× 3 p= MIN a=3 ③ 2=- β=3 ③ )β=3 MTN ③=3 a= d=3 a= 2=2 a=3 2 2B=2 $B=3 2 ③ )=3 3 17 2 12 15 25 0 2 5 3 2 14

D E F H

AⅡB • A#D|B •AⅡD|C ·A±C ·B业C • A业B |D • Bμ D | A, C

A B 回

E (M S B

x1 = T, 1 = 1, 2, 3, ps1(+) = 3 , ps1(−) = = = x1 = F, s2 = 4, 5, 6, ps2(+) = , ps2(−) = =  E(s1, x1) = −ps1(+)l0g2(ps1(+) − ps1 (−)l0g2(ps1 (−)) = 0.92 E(s2, x1) = −ps2 (+)log2(ps2 (+)) − ps2 (−)l0g2(ps2 (−)) = 0.92 E((s, x), x1) = s × E(s1, x1) + s2 × E (s2, x1) = 0.92

X1 = T x1 = F (_,x2) (S2,x2) x2 = T x2 = F x2 = T x2 = F

答： 设ig(x) = i+e-， 根据已知条件可求得： h1 = x1 · W11 + x2 · W13 + b1 = 0.08 × 0.24 + 0.12 × 0.2 + 0.28 = 0.0192+0.024+ 0.28 sig(h1): = ≈ 0.58 1 + e−h1 h2 = x1 · W12 + x2 · W14 + b1 = 0.08 × 0.3 + 0.12 × 0.16 + 0.28 = 0.024+ 0.0192+ 0.28

sig(h2) ) = ≈ 0.58 1 + e−h2 y1 = sig(h1) · W21 + sig(h2) · ω23 + b2 = 0.58 × 0.5 + 0.58 × 0.55 + 0.6 = 0.29 + 0.319 + 0.6 =1.209 y2 = sig(h1) · w22 + sig(h2) · ω24 + b2 = 0.58 × 0.45 + 0.58 × 0.4 + 0.6 = 0.493+ 0.6 = 1.093

又因为 e −x sig′(x) : c− 可求得 sig′(h1) : (1 + e−h1)2 ≈ 0.2435 sig(h2) ≈0.2435

LOSS为 Loss1 (y1 − Label1)2: (y − 2y1Label1 + Labeli) 2 2 : Loss2  (y2 − Label2)2 : (y2 − − 2y2Label2 + Label2) 2 2 − ;

y1 − Label1 = 1.209 − 0.05 = 1.159 y2 − Label2 = 1.093 – 0.95 = 0.143 ∂W21 ∂y1 ∂W21 = ( =  ( (y1 − Label1) · sig(h1) =1.159×0.58 =0.672

∂LoSS ∂LoSS ∂y ∂w23 ∂y1 ∂w23 (y1 − Label1) · sig(h2) — = 1.159 × 0.58 [Tab0.e72 ∂w22 ∂y2 ∂w22  (y2 − Label2) · sig(h1) = 0.143 × 0.58 =0.083

∂Loss ∂Loss1 ∂Loss2 ∂b2 ∂b2 ∂b2 ∂Loss1 1  ∂y1 ∂Loss2 ∂y2   ∂y1 ∂b2 + ∂y2 ∂22 (y1 − Label1) + (y2 − Label2) = 1.159 + 0.143 [a1.302

∂Loss2 ∂LosS2 ∂y2 ∂W24 ∂y2 ∂w24 (y2 − Label2) · sig(h1) = 0.143 × 0.58 =0.083

OLOSS OLOSSI ∂LOSSZ ∂W11 ∂W11 ∂W11 ∂Loss1 ∂y1 dsig(h1) ) )  ∂h1 ∂Loss2 ∂y2 dsig(h1) ∂h1 ∂y1 ∂sig(h1) dx ∂w11 ∂y2 ∂sig(h1) dx ∂w11 (y1 − Label1) · w21 · sig (h1) · x1 + (y2 − Label2) · w22· sig (h1) · x1 = [(y1 − Label1) · w 21 + (y2 − Label2) · W22] · sig (h1) ·x1  = ( (1.159 × 0.5 + 0.143 × 0.45) × 0.2435 × 0.08 =0.0125

OLOSS OLOSS1 OLOSSZ ∂W12 ∂W12 ∂W12 ∂LoSS1 ∂y1 ∂h2 ∂LOSS2 ∂y2 ∂h2 − · sig (h2)· − · sig (h2) ∂y1 ∂sig(h2) ∂w12 ∂y2 ∂sig(h2) ∂w12 = [(y1 − Label1) · w23 + (y2 − Label2) · w24] · sig (h2) · x1  = (1.159× 0.55 + 0.143 × 0.4)× 0.2435× 0.08 =0.0135

∂Loss ∂Loss1 ∂Loss2 ∂W13 ∂W13 ∂W13 ∂Loss1 ∂y1 ∂h1 ∂Loss2 ∂y2 ∂h1 ∂y1 · sig (h1) ·sig (h1)· ∂sig(h1) ∂w13 ∂y2 ∂sig(h1) ∂w13 = [(y1 − Labe 1)· W21 + (y2 − Labe 2) · W22]· si h1) ·x2 =(1.159×0.5+0.143×0.45)×0.2435×0.12 =0.0188 ∂Loss ∂Loss1 ∂Loss2 ∂W14 ∂w14 ∂W14 ∂Loss1 ∂y1 ∂h2 ∂Loss2 ∂y1 · sig (h2) ∂y2 ∂h2 ∂sig(h2) ∂w14 ∂y2 ·sig (h2)· ∂sig(h2) ∂ω14 = [(y1 − Labe 1) · W23 +(y2 − Labe 2) · ω24]· sig (h2)·x2 a 1.159×0.55+0.143×0.4 ×0.2435×0.12 [a0.0202

∂LOSS ∂LOSS1 ∂LOSS2 ∂L0ss1 ∂y1 ∂h1 ∂y1 ∂h2 · sig (h1)· · sig (h2)· ∂y1 ∂sig(h1) ∂b1 ∂sig(h2) ∂b1 ∂L0ss2 ∂y2 ∂h1 ∂y2 ∂h2 + ∂y2 ∂sig(h1) · sig (h1)· ∂b1 · sig (h2)· ∂sig(h2) ∂b1 = (y1 − Label1) · [w 21 · sig (h1) + w23· sig (h2)] + (y2 − Label2) · [w22 · sig (h1) + w24 · sig (h2)] = 1.159 × (0.5 × 0.2435 + 0.55 × 0.2435) + 0.143 × (0.45 × 0.2435 + 0.4 × 0.2435) = 0.326

[根据公式 ∂Loss W=W—Q· 可得 ∂LosS1 wω21 + = W21 · - · α ∂w21 : 0.5 − 0.5 × 0.672 = 0.164 ∂Loss1 w23 ,+ = W23 − α ∂w23 = = = 0.55 − 0.5 × 0.672 = 0.214 ∂LosS2 w22 + = W22 · : 0.45 − 0.5 × 0.083 = 0.4085  · α ∂w22 w24 : ∂Loss2 : W24 − α· = 0.4 − 0.5 × 0.083 = 0.3585 ∂w24 ∂Loss b2 = b2 − α· ∂b2 0.6 − 0.5 × 1.302 = −0.051

wτ1 W11 = 0.24 −0.5 × 0.0125 = 0.23375 - α · ∂w11 ω12 W12 - α = 0.3 - 0.5 × 0.0135 = 0.29325 O12 ωi3 = W13 − α · = 0.2 − 0.5 × 0.0188 = 0.1906

综上所述，更新后的参数为： w11 = 0.2338 w12 = 0.2933 w13 = 0.1906 w14 = 0.1499 b1 = 0.117 w21 = 0.164 w22 = 0.4085 w23 = 0.214 w24 = 0.3585 b2 = −0.051

第二次作业   1.1用A*搜索算法求解图中所示的最短路径，！ 其中S表示初始节点， ] G表示目标节点。 相邻节点之间的代价用边上的数字表示， ， ， 在每个节点中括号里的数字表示此节点到目标节点的代价。 s Open Close 7 4 S 10/ 11 A (8) 8 C B (5) 5 (6) 8 15/ 20 2 E D (3) 16 (5) 1 H 20 19 F (7 1 (3) 13 1 3 (4) 2 G 5 13\ (0) 16 J 7 K (5) (3)
