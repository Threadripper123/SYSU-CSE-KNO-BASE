# Lecture7-机器学习2-监督学习-图集-矢量

> 主题库：人工智能　｜　源文件：Lecture7-机器学习2-监督学习-图集-矢量.pdf　｜　2,874 字符　｜　52 个片段　｜　更新于 2026-11-13
梯度

梯度

zation Toss oi testi g uata large small ful 9Y

f θ1(x) y = fθ mple , f θ

zation Tossoi tcst Tg uata large small

1(x) F *(* (x)

L(O*): large

f θ1(x) ,fe ay

1 layer 2 layer 3 layer 4layer 5 layer 2017— 0.28k 0.18k 0.14k 0.10k 0.34k 202

zation 1oss oi tcsti g uata large small cY can GY itting mismatch

optimization TOSs OlT tcSt g uata large small Yre powerful cetimination cYechnology averfiting mismatch

Large los:

Fle m

C

Yrs [Yll connected CNN

y  a+ bx

select this ONE Training

Training Set Training Validation Usi Set set dat You Model 1 → mse = 0.9 bet Model 2 → mse : = 0.7 Model3 —mse=0.5

Training Set Model1 Train Train Val mSe = 0.2 Train Val Train mse = :0.4  Val Train Train mse = 0.3 Avg mse =0.3 Testing Set Testing

eral Tosso a mguata jide large_ small odel loss on te ias optimization large

▲ ·Loss Gradient Descent Very slow at the [Yaatea 广 Stuck at saddle point Stuck at local minim ∂L/∂w ∂L/∂w ∂L/∂w ≈0 =0 =0

Small Large cYaa update Faster Slower (no parallel) cYaa Same (not too update Same large) (with parallel) N Gradient Noisy Stable LIN Generalization Better Worse Batch size is a hvperparam eter voi have to decide

↑ Loss Consider the physical world ….. How about put this phenomenon in gradientdescent?

Negative of ∂L/aw + Last Movement Negative of aL / ∂w  LastMovement Real Movement

nol gra

30 900 500

1 (gt){ 2 t + 1. x:

Smaller Learning Rate

Larger Learning Rate

α(ot−1)2 a + (1 − α)(gt)

α(σ)2· + (1 − α)(gi)2

α( )2· + (1− α)( )2

Iiiiuence, arid thie past graaierts Tav less influence. small ot iNCreaSE ot larger step smaller step decrease Yarger step

0<α<1 α(σt−1)2 · + (1 − α)(gt) 2

2：ifD中样本全属于同一类别Cthen 情形(1) 4:endif 递归近 5：ifA= ORD中样本在A上取值相同then 情形( 6： 将node标记为叶结点，其类别标记为D中样本数最多的类；return [a:endif 利用当前结点的后验分布 8： 从A中选择最优划分属性a*; 9：fora*的每一个值ado 10： 为node生成一个分支；  令D表示D中在a上取值为a的样本子集； 递归 11： 情开 ifD，为空then 12： 将分支结点标记为叶结点，其类别标记为D中样本最多的类；return 13: else 将父结点的样本分布作为当前结点的先验分布 15: endif 16:endfor [T++

D 否  9 Prob  17

0 0 0 0

0 0 0 0 2

0 0 0

A B· C

W b (llw| - xi + IlwIl

C 2 b b margi ll in b

op(w)= { () if w satisfies primal constraints otherwise.

min max L(ω,α,β) p W α,β:αi≥0 对偶问题 Dual Problem 原问题 Primal Problem

op(w) ={ () if w ) satisfies primal constraints  otherwise.

min m ax L(w, α, β) C W a,β:αi≥0 原问题 Primal Problem

αi gi(ν*) 0, i : , k gi( ) 0, i k k α 0, i

When we construct the Lagrangian for our optimization problem we have: (ν, b, ) = 2||2 − ∑α [(i(v () ) b) − ] 8)

When we construct the Lagrangian for our optimization problem we have: L(ν, b, α) = || 2 − ∑ i [y (i( Tx(i) + b) − 1]. (8)

a b

式(3.2.5)对α求极大，也等价于式(3.2.5)取负数后对α求极小，即 n n min αiαjyiYj ixIXj n αi (3.2.6) A i=1j=1 i=1 同时满足约束条件： N αx Yi = 0 (3.2.7) =1 0 ≤ αi ≤ C,i = 1,2, . ..,n. 至此，我们得到了原始最优化问题页(3.1.4)和对偶最优化问题页(3.2.6)、(3.2.7)

1 VS5 2,3,4,5 1,2,3,4 2VS5 1 VS4 3,4,5 3VS5 2VS4 1VS3 1,2,3 4VS5 3VS4 2VS3 1VS2 5
