# Lecture7-机器学习4-强化学习-图集-矢量

> 主题库：人工智能　｜　源文件：Lecture7-机器学习4-强化学习-图集-矢量.pdf　｜　2,313 字符　｜　69 个片段　｜　更新于 2026-11-13
Action ∈ {left,  -  right, up}    left right

Policy function π:(S, )  0,1 : (   π( | S) = P(A = | S = S)-

oPolicy function s [0, πC S) = P(A = S = S))

Environment

agen Environment

agent State St +1 Reward rt Environment

S

π( |s ) = 0.2, π ( |s) 0.1, π( |s ) = 0.7.

S

S1  a1  S2  r1

Rt Rt+1 Rt+2 Rn  Ut is arandom variable

A random . ample(

St-1 at-− 1 M St

TT St-1 at-1 St [rt-1 r-1

Conv Dense

at= argmaxaQ (s t a; w)

a t= argmaxaQ (s t, a; w )

St+1~p( |st, at)

v at+1 = argmaxaQ(St +1, a; w)

at +1 1= argmaxaQ(St+1,a; w)

St+2~ p(· |st+1, at+1) [Tabb St

a t+2 = argmaxaQ(st +2, a; w)

—1 rt+2 a t+2 = argmaxaQ(St +2, a; w)

St+3~ p(· |St+2, a t +2)

NYC 1000 minutes (estimate). Atlanta

NYC 1000 minutes (estimate). 860 minutes (actual). atlanta

NYC Atlanta

NYC 300 minutes (actual). DC

Y NYC DC 600 minutes (estimate). atlanta

600 minutes (estimate). Atlanta

M odel''s estim ate Actual tim e M odel''s estimate

M odel''s estim ate Actual tim e M odel''s estimate

= γ · (Rt+1 + γ ·Rt+2 + γ2· Rt+3 + γ3 · Rt+4 + … )

(Rt +1 + + Y · Rt +2 | - + γ2 · Rt +3 + γ 3 · Rt +4 + [= U +1

Q(St+1, ;w)]. estimate eof Ut+1 ≈ E[U t+1]

rt + γ · Q (St+1, ; w). [Taa na

Q(st ;w) Prediction

Action a1 Action a2 Action az Action a4

Conv Dense softmax

Definition: Action-Value function. • Qπ(St, ) = E [Ut|St = St, ].

∂v(s;θ)

Policy gradient: Derivative of V (s; O) w.r.t. 0. (θ) π;) π() = ∑ π( |s; θ) ∂ 10g π ( |s;θ) Qπ(s, )  ∂ θ

∂log π( |s;θ)

Value-Based Actor-Critic Policy-Based Methods Methods Methods

Conv Dense softmax

Network Environment

Conv

Conv

Policy Head

Value Head ure

Conv

pl ayer''s action opponent''s action PN I

St

First, for all the valid actions a, calculate the score: T (a |St ; ) score(a) = Q(a) + n * -  1 + N(a)

St

ST Prob = 0.4 \pro

ST St+1 at+1 aaponentsaction St+1 Fast player''s action Rolloutt  apponent''saction ST

ST St+1 Record V(St+1).

ST Records: Records:  (1), y(2), V(1), −2), V(1), v(2), L • (1) · v(2)

ST Records: Records: V(1), ν(2), V(1), v(2), V(1), v(2), (1) · V(2)

ST D QC Q(()

First, for all the valid actions a, calculate the score:

= Ut+1

aY Ne not know te xpectati Approimat t using Monte Carl (Me.

Qπ(St+1, Ot+1)

δt · . ∂ q(St,@ ;w) ∂w

[Rt + γ · Qπ(St+1, At+1)

At+1 argmaX1 xQ*(St+1,@)

rt + y ·maxQ*(St+1, )   TDtargetyt
