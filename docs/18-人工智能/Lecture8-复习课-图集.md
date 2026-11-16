# Lecture8-复习课-图集

> 主题库：人工智能　｜　源文件：Lecture8-复习课-图集.pdf　｜　4,915 字符　｜　39 个片段　｜　更新于 2026-11-11
An interpretation is a pairS =(D,I) • D is the domain, can be any non-empty set  I is a mapping from the set of predicate and function symbols o If P is a predicate symbol ofarity n, I( P) is an n-ary relation over D, i.e., I(P) C Dn Ifp is a O-ary predicate symbol, i.e., a propositional symbol, o If f is a function symbol of arity n,I(f) is an n-ary function over D, i.e., I(f ) : Dn → D • Ifc is a O-ary function symbol, i.e., a constant symbol, I(c)∈D

O 1 PAO R0062 四三 =PV=QVR =R 证明：化成子句集 P√Q S ={P, ¬ P√ ¬ OVR, 1SVQ, 1TVQ, T, 1Q = TVQ I R} 归结可图的演绎树表示由于根部出现空子句，因此命题R得证。

一个表达式的项可以是常量符号、变量符号或函数式表达式的例（instance）是指在表达式中用置换项置换变量后得到的个特定的表达式。 用σ = {v₁/t1, V₂/t2,. , Vn/tn} 来表示任一置换宗辰 v/t是指表达式中的变量v以项t来替换，且不允许v用与v有关的项t：（但是t，中可以包含其它变量）作置换。 为了便于理解， 后续记σ={v1= t1,V2=t2,., n= tn}。 用σ对表达式E作置换后的例简记为Eσ。

令 θ ={x = f(y), y = z}, σ = {x = a, y = b, z = y} 1. Get S = {x = f(b), y = y,x = a, y = b,z = y} 。2.Deletex=a;Deletey =b o3.Deletey=y θσ = S = {x = f(b), z = y } 这样的合成法可使(E0)σ=E(θσ)，即但置换是不可交换的，即0σ≠σ0。 空置换c={} 是个置换，且θc=θ。

A substitution σ of two formulas f and g is a Most General Unifier (MGU) if oσisa unifier.  For every other unifier θ of f and g there must exist a third substitution λsuchthatθ = σλ. This says that every other unifier is "more specialized" than σ. The MGU of a pair of formulas f and g is unique up to renaming.

P(f (x),≈ ) and P(y, a) • σ = {y = f(a),x = a, z = a} is a unifier, but not an M GU θ = { y = f (x),z = a}is an MGU ● σ = θλ,whereλ ={x =a}

Given two atomic formulasfandg  σ = { }; S = { f , g}  If S contains an identical pair of formulas, stop and return σ as theMGUoffandg. Else find the disagreement set D ={e1,e2} of S Ife1 = Va variable, and e2 =t a term not containingV (or vice-versa)thenletσ = σ{V =t}; S = S{V = t};Goto2 Else stop, f and g cannot beunified. Neme anuodaieo, we mmmuse Gomposeo whnth Acommnonremronlisto usg add y = ttoo.

已知： ： （1） 会朗读的人是识字的， （2） 海豚都不识字， (3) 有些海豚是很机灵的。 证明：有些很机灵的东西不会朗读。 解：把问题用谓词逻辑描述如下， 已知： (1) √x(R(x)→L(x)) (2) √x(D(x)→¬L(x)) (3) x(D(x)\1(x)) 求证： x(I(x)∧¬ R(x))

前提化简，待证结论取反并化成子句形， 1.( (¬R(x), L(x)) 2.(¬D(y),¬L(y)) 3.D(a) 4. l(a) 5.(¬|(z), R(z)) 6. R[4,5]{z= a} R(a) 7. R[1,6]{x=a} L(a) 8. R[2,7] {y= a} ¬D(a) 9. R[3,8] )

2 3 7 5 2 8 3 2 8 3 2 8 3 1+5 1 6 4 1+3- 1  4 1+5 1 6 4 7 5 7 6 5 7 5 2 8 3 2  3 2 8 3 2+3  1 4 2+3 1 8 4 2+4 1 4  6 5 7 6 5 7 6 5  8 3 2 8 3  2 3 2 3  2 1 4 7 1 4 1 8 4 1 8 4 6 5 6 5 7 6 5 7 6 5 3+3 3+4 3+2 3+4 1 3  8 4 4+1 7 6 5 1 2 3 1  3 5+0 8  4 5+2 7 4 7 6 5  6u 5 目标结点

2 2□ 6□ □0 □-1 □-1 20 -10 -10 60 -10 00 -50 0-2 0-10-7 0-1 29 51 -1-1 3 68 -1 2 0 -5 74 -26.-1.-7 -143 图3-18四层博弈树

MAX △≥3 MIIN 12

MAX ≥3 MIN ≤2 8 2

MAX △≥3 MIN ≤2 7≤14 X X 8

MAX △≥3 MIN 2 7X5

MAX △x3 MIN 72 7 x2

b n 0 3 h11 k 6 a -3 3  -3 -3 - -291-3 j 6 -3 05-33 3-3022-30-23541-30689-3

I D G Prob. i0 d0 g 0.126 i0 d0 g{ 0.168 d0 g{ 0.126 io d{ g^ 0.009 io d g{ 0.045 i0 d g{ 0.126 i d0 g 0.252 i d0 ^2 0.0224 i d0 g{ 0.0056  d{ g 0.06 i d{ g{ 0.036 i d g 0.024

(=0=0.6 (I= )=0.4 i∑(,G = p(i,d0,g1)+ p(id,g)+ p(i,d0,g +(i,d1,g1)+p(i,d1.g²)+p(i,d,g3)

x step by we succeed joy sad onehot 1 1 1 1 0.9 0.1 TF 0.4 0.2 0.2 0.2 0.9 0.1 TF-IDF 1.03 0.7 0.6 1.16 0.9 0.1

p (X) = (X r (X,  i 节点定义 d0 d1 i0 i 0.6 0.4 0.7 0.3 试题难度 (D) :d0(低), d1(高) Difficulty Intelligence 智力 (I)：io(低)， (高) g g2 g3 i0,d0 SAT 0.3 0.4 Grade 0.3 考试成绩 (G) :g1 (A) , g2 (B) ,g3 (C) i0,d 0.05 0.25 0.7 i1,d0 0.9 0.08 0.02 1 Letter s0 高考成绩 (S) :50 ( 低) , s1 (高) 0.5 0.3 0.2 0 0.95 0.05  0.2 0.8 10 11 是否得到推荐信 (L)：10(否)，11(是) 0.1 0.9 2 0.4 0.6 a 0.99 0.1 6

[联合 = P(D)P(I)P(GI, D)P(S I)P(L |G) 联合概率分布结构化分解的好处： 枚举法：2*2*3*2*2-1=47个参数 □结构化分解：1+1+8+3+2=15个参数

= P(d1)P(i )P( i, d1)P(s1 i)P(10 |g1) =0.4×0.7×0.05×0.05×0.1 =0.00007

A  熬夜赖床迟到联合概率分布链式法则： 条件独立： p(A,C, B) P(BA,C) =P(B C) =P(A)P(CA)P(BA,C) P(A, C, B)=P(A)P(C A)P(B C)

C a 6

a b

a 6

a 6 c

f f a a e b e 6 C

1: SelectK pointsastheinitialcentroids. 3: Form K clusters by assigning all points to the closest centroid. 4:  Recomputethecentroidofeach cluster.

Case1 y =g(x) z =h(y) dz dz dy ∆x→∆y→∆z dx dy dx CASEZ x = g(s) y = h(s) z = k(x, y) △x ∆s Δz dz ∂z dx ∂z dy ds ∂x ds ∂y ds △y

L(θ) = = ∑cn(θ) ∂L(θ) - ∑ i ∂cn(θ)    ∂w ∂w n=1 n=1 Y

W1 Z X1 y1 W2  Z =x 1W1+x2W2 +b x2 Y2 Forwardpass: ac ∂z ac Compute ∂z/aw forall parameters aw w∂z Backwardpass: (Chain rule) Compute ∂C/az for all activation functioninputs z

Compute ∂z/aw for all parameters x1 - W1 Z Y1 2 Z =x1W1+x2W2+b ∂z/∂w1 =? x1 The value of theinput ∂z/aw2 =? x2 connected by theweight

Compute ∂C /az for all activation function inputs z z x1 W1 Z a W3 1 a = σ(z) W2 4 x2 +  ∂c  ∂a∂c  ∂z ∂z∂a σ(z)

∂c ∂a∂c ∂c ∂z''ac ∂z'' ∂c ∂z +· ∂z∂a ∂a ∂ai ∂z ∂a ∂z (Chainrule)

3 -1 -3 -1 -3 1 0 -3 -3 -3 0 1 3 -2 -2 -1

Output Gate Forget Gate Cell InputGate Block

第六章机器学习   [bae 1 0.73 2 0.72 3 0.51 0 -2 -1_ -1 1 0 -2 -1 0.5 -2 0.12 -1 0.85 1 -1 4 0 0 2 相当于函数功能 f([1]) ： [0.62] [0.51] ） ）： f([1%])： [0.83」 [0.85] 输入一个向量，输出另一个向量网络结构相当于定义了函数集空间
