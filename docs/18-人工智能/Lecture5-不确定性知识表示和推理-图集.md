# Lecture5-不确定性知识表示和推理-图集

> 主题库：人工智能　｜　源文件：Lecture5-不确定性知识表示和推理-图集.pdf　｜　6,013 字符　｜　50 个片段　｜　更新于 2026-11-11
ArtificialIntelligence [人工智能 ChaoYu(余超） School of Computer Science and Engineering [Tabl _l__

3y1 x2 2 53 256 @ 3 34 ©-1 135 5 3s T5 206 3 6 贝叶斯网马尔科夫随机场因图 p(x)= (x (s) p(x)= 9 Iφ( p(x) φ(,x) (x)= (x) Z ( q) c

p (X) = p ( X1, X2, . . ., XN 图论概率论

Xα )= ∑ (x) x=argmax (x XEX 求边缘概率、最大后验概率状态等

Gaussian noise denoised stdev5o

Apetstte OO  ONSTAOI ESSENTIALS COMMUNICATE FIND  OnParenting Fideliny Our homeony [iswhere] vestnaontso theweb cover contents news experts fun handbook talbiliceid heip feedbaeh Tnerearemowaysto searchforspecxc Describethechild Ages Todder sex Fenae 回 infov ation in in nedroo-dor n boces a t e Complatnt: Abdoennal pan Orfarertrg Ip Farrd by riat .Reievart informntion wal

A

观察到1个数据，点数为5 0.25  0.2       0.15  0.1 0.1 0.05 4面 6面 8面 12面 20面 + 先验概率后验概率 O

例1.1.1 一种诊断某癌症的试剂，经临床试验有如下记录： 癌症病人试验结果是阳性的概率为95%，非癌症病人试验结果是阴性的概率为95%.现用这种试剂在某社区进行癌症普查，设该社区癌症发病率为0.5%，问某人反应为阳性时该如何判断他是否患有癌症， 设A表示“反应为阳性”的事件，B表示“被诊断者患癌症”的事件，则B1=B和B2=B构成完备事件群.由题意 P(AB1)=0.95, P(AB2)==1 −0.95 =0.05, P(B1)=0.005, P(B2)=0.995. 现在要算的是P（B1A）和P（B2|A).由贝叶斯公式易得 P(B1|A)= (AB)(B) P(A|B1)P(B1)+P(A|B2)P(B2) = 0.087 = 8.7% P(B2|A)=1 −0.087 =0.913 = 91.3%. 某人真正患癌症的可能性很小，只有8.7%，告诉他不必紧张可以到医院去做进一先的检查，以便排除这一疑点

I D G Prob. i° d0 g 0.126 i0 d0 g{ 0.168 0 d g{ 0.126 io d g{ 0.009 io d g{ 0.045 0 d g{ 0.126 i d0 g 0.252 i d g{ 0.0224 i d0 g{ 0.0056  d g 0.06 i d{ g^{ 0.036 i d g 0.024

[ (I=i0)=0.6 (I=)=0.4 ∑,G = p(i,d0,g1)+ p(i,d,g2)+ p(i,d0,g +(i,d1,g1)+p(i,d1.g²)+p(i,d,g3)

p ( X ) = □ pi(X Pa (Xi  ì 节点定义 d0 d1 i0 i1 0.6 0.4 0.7 0.3 ● 试题难度 (D)：d0 (低) , d1 (高) Difficulty Intelligence 智力（I）：io（低）， （高） i0,d0 0.3 Grade SAT 0.4 0.3 ● 考试成绩 (G) : g1 (A) , g2 (B), g3 (C) 0, 1 0.05 0.25 0. i1,d0 0.9 0.08 0.02 s0 s1 Letter  高考成绩 (S) :s0 (低) , s1 (高 ) 0.5 0. 0.2 0.95 0.05 i 0.2 0.8 10 1 是否得到推荐信 ()：10(否)，11(是) 0.1 0.9 0.4 .6 83 0.99 0.01 6

= P(D)P(I)P(G |I, D)P(S |I )P(L |G) 联合概率分布结构化分解的好处: 枚举法：2*2*3*2*2-1=47个参数 □结构化分解：1+1+8+3+2=15个参数

□更一般地，假设n个二元随机变量的联合概率分布，表示该分布需要2n-1个参数。如果用贝叶斯网络建模，假设每个节点最多有k个父节点，所需要的参数最多为n*2k，一般每个变量局部依赖于少数 [变量

= P(d1)P(i)P(g i , d1)P( i )P(10 |g ) = 0.4×0.7×0.05×0.05×0.1 =0.00007

= P(d )P(i1)P(g1 i , d0)P(s1 i1 )P(|1 |g1) = 0.6×0.3×0.9×0.8×0.9 =0.11664

熬夜赖床迟到联合概率分布链式法则： 条件独立： p(A, C, B) P(B A, C ) = P(B C ) =P(A)P(C A)P(BA,C ) p(A, C, B)=P(A)P(C |A)P(B C )

α θ θ z z1 z2 X Y X1 Y1 X2 Y2

α θ Z W N M

Malaria Flu Cold)

Malaria  Flu Cold)

C

a b

a b 2 C

a 6 c

f a a j e b e b

Difficulty Intelligence Grade SAT

p(I,D,G,S,L) = P(I)P(D | I)P( G |I , D)P(L I , D, G) P(S | I, D, G, L) = P (I)P(D)P(GI, D)P(L |G)P(S )

(D|I = P(D)  P(L |G) = P (L|I , D, G) P (S | ) = P (S | I, D , G, L )

既孕图 r dimersion 单—： Naive Bayes → P(xly)= πp(xily=1) 混合：GMM Bayesian Network. 云动态模型. MarkoV Chain； 从单—到混合时间 Gussian Process (天限维高斯分布). 人人有限刻无限. 连续： Gussian Bayesian Network 空间(离散→连克) A(离散) 时间 HMM( LDS (kalman Filter).连续.线性. ② z是离散的 Partide Filter ( 高斯非线性). r f xz~ N(μ2) NAIVE BAYES G MM

边缘概率： p(xα)=∑p(x) 三类最大后验概率状态：x = argmaxp(x) 根既率推理 x=X 求归—化因子：Z=∑□φ(x) X C

p(x)

Variableelimanation(VE) 精确推断 belief propagation (BP). sum-product algorithm.(针对树结构). Inference( junction tree adlgorithm （普通图结构 ) Lop beliet PrOpAgatiOn (有环图)  近似以推断 merte carlo inference：Tmportance Sampling， MCMC.  VAriAtiONaL InfErENCE.

边际化. HMM . ① Evaluation： P(0)= = P( 0){后前向  I O → O —→ O Irderence1 ②Leaming：λ Vitebì Algorith 。(云忘母 HMM: dYNAMIC BaYESiAN NETWOrk.

a b c 0.25 a b c2 0.35 2(-4.C)=∑ (1.BC) (4)=∑φ2(A1.C) a b2 c c 0.08 a b2 c2 0.16 a c1 0.33 a2 b c 2 0.07 a2 c{ 0.05 a2 0.12 a2 b2 c1 0 a{ c2 0.07 a2 b2 c2 0 a^{ c 0.63 0.24 a^{ b c 0.15 a{ c2 0.39 a3 b c2 0.21 a{ b2 c 0.09 a³ b2 c2 0.18

P(E)α∑∑∑∑P(A,B,C,D,E) DCBd = ∑∑∑∑ φ(A, Bφ2(B,C)φ₃(C,D)φ₂(D, E) B = ∑∑∑ φ₂(B,C)φ₃(C,D)φ₂(D,E)∑ φ₁(A.B) DCB A = ∑∑∑ φ₂(B,C)φ₃(C,D)φ(D,E)τ₁(B) DCB

Coherence Difficulty Intelligence Leer Job Happy

消元变量涉及因涉及变量新因  φC(C),φD(D,C) C,D T1(D) φG(G,I,D),τ1(D) G,I,D τ2(G,I) φI(I),φs(S,I),τ2(G,I) G,S,I T3(G,S) S T4(G,J),τ3(G,S),φL(L,G) G,J,L,S T5(J,L,S) L T5(J,L,S),φJ(J,L,S) J, L,S T6(J, L) T6(J,L) J, L T7(J)

I

消元变量涉及因子涉及变量新因子 G φG(G,I,D),φL(L,G),φH(H,G,J) G,I,D,L,J,H T1(I,D,L,J,H) I φI(I),φs(S,I),T1(I,D,L,S,J,H) S,I,D,L,J,H T2(D,L,S,J,H) S φJ(J,L,S),T2(D,L,S,J,H) D, L,S,J, H T3(D,L,J, H) L T3(D,L,J,H) D,L,J,H T4(D,J,H) H T4D,JH) DJH T5(D,J) C T5(D,J),φc(C),φD(D,C) D,J,C T6(D, J)

//Φ为因子集，Z为消元变量集 1：令变量消元顺序为 .. 2:fori=1....k 3: —Sum-Product-Eliminate-Var(①.Z) 4:φ←Ⅱ =φ Sum-Product-Eliminate Var(①, 1:①''←{φ ∈①:Z∈Scope[φ]} 2:①”←①-①'' 4:τ←∑2 5:return ① U{}

@ →⑥— ⑥—→@ (假设abc₁d均为离散的二值randomvariable) aBICIDELOY Pld)=∑ P(ab,c,d) abc = ∑ P(a)·P(b|a). p(clb)· P(d|c). a,bic = p(a=0)-P(b=o|a=0)- P(c=0|b=o). P (d=0|c=0) a b C d. +P(a=1).P(b=0|a=1).P(c=0|b=0).P(d=0|c=0) +- + P(a=1) P(b=1|a=1)·P(c=1 |b=1) P(d=1|c=1) = Σ p(clb). P(d|c)·∑ pla). P(b|a) ab+ac= a(btc) bic a φ(a) φ(a,b) Palb). =EP(dc) P(cb).$a(b) PCO = PEd) (乘法对加法的分配律)

P(a,b,c,d,e)= P(a). P(b|a). P(c|b)- P(d|c)· P(e|d). JOrWARd algOrithm p(e)= MP(a,b,c,d,e) abc,d = ∑P(e|d)·∑P(d|c). P(c|b).∑P(b|a).P(a) a a Masb(b) mb→>c (C). P(c] = ∑ P(ab,cid,e). a b d,e forward-backward algorithm. = ( EP(c|b)· Pcb|a)· P(a) -(∑P(d|@) ∑ P(e|d) CHaIN → TREe 有向→无向

P(a,b,c,d)= z4a(a).4(b)·4c(c)(d(d) a · (ab(a,b)· 4b,n(b,c). 4b,d(b,d). ma→b → a P(a)= 三p(a,b,c,d). md→ b M ， b c,d mboa(xa)=∑ 4ab 4bMcb(xb).md-→b(xb) XD P(a)=$amb→a(xa). GENEA M = E KENBG)-i P(xi)=4iπ M→i(Xi) KENBi) 女不要直接去求边缘概车P(a)，P(b)，P(o)，P(d) 只需求mi→j

mb→a 1m ma→b 5 $mc→b md→b

mj→i=\ Pab , Pb· mc=b· md→b   belief SEIf HILDREN )belief(b)= 4b· children Mba= bBEliefB BP=VE+ Cadīng. (直接求mj P(xi)) 图的遍历

Ba (SeqUential ImplementatiOn DGET ROUT ASSUME AIS ROUT ② collect message. for xi in NB(rout): collectmsg (xi) ③ distribute message. 2. for xj in NB(rort): distribute(xj)   可得 Mij for All ijEV 从PPIXK) KEV. BP (Parallel Implementation).

Model Data X1 X 2 X 3 X4 cYa f C f t t f f f t t x₁ (x2 (x3) (x4

A[1,...,X4,C] X₁ X2 X3 X4 C Count X1 X2 X3 X4 Count  : : :  : : : : : : t f t t 1 40.7 t f t t 100 → t f t t 2 12.1      t f t t 3 47.2

X1 X2  X3    X4 C Count t f t t 1 40.7 t f t t 2 12.1 t f t t 3 47.2
