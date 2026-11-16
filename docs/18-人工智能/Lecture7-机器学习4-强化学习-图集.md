# Lecture7-机器学习4-强化学习-图集

> 主题库：人工智能　｜　源文件：Lecture7-机器学习4-强化学习-图集.pdf　｜　7,150 字符　｜　45 个片段　｜　更新于 2026-11-11
Reinforcement Learning An lntroduction secondedition Richard S.Sutton and Andrew G.Barto

MORGANSCLAYPOOL PUBLISHERS Learning CsabaSzepesvr SYNTHESSBECIURES ONARTFICIAL INHAACFNOPANRMACHINELEARNING Roeatd J. Brachman asdThomas G.Dieerich, Sries Eiaer

MMereowieing Mar uhnvan OicrTo (Bds) Reinforcement Learning State-of-the-Art

Supervised Unsupervised Learning Learning MM

Englneerlng Neurosclence Relnforcement. Learning Cperalons assscaiveperam Mathematlcs pngy Psychology Economlcs

pricing, trading recommendation adaptive adaptive proficiency est. DTRs portfolio opt. e-commerce, OR decision traffic signal recommendation diagnosis risk mgmt customer mgmt control control education games EHR/EMR business finance energy transportation education healthcare cYmaanagement attention and exploration representation model multi-agent R memory agent unsupervised state 4 hierarchical learning action reward Rl environment relational learning RL policy value reWard function tolearn science games robotics NLP computer computer engineering vision ystems art Go, poker sim -to-real seq. gen. recognition topics in maths, physics, Dota, bridge co-robot translation detection computer chemistry, music Starcraft control dialog, QA,lE,IR perception science drawing, animation Yuxi Li, Deep Reinforcement Learning, https://arxiv.org/abs/1810.06339, 2018

Video Pinball 2539% Boxing 1707% Breakout 17% Star Gunner % Robotank 508% Atlantis 449% Crazy Climber 419% Gopher 400% Demon Attack  Name This Game 2% Krull 277% Assauit 246 Road Runner 2% Kangaroo 224% James Bond 145% Tennis" 143% Pong.  Space Invaders  Beam Rider  Tutankham 112% [Kung-Fu Maste 1 Freeway 102% Time Pilot" 00% Enduro 97% Fishing Derby  Up and Down 2% lce Hockey 79% Q"bert  H.E.R.O. 76 At human-level or above Asterix  Below human-level Battle Zone 67% Wizard of Wor 67% Chopper Command  Centipede 2% Bank Heist 57 River Raid  Zaxxon 54% Amidar  Alien  Venture  Seaquest'' 25% Double Dunk"  Bowling_ +14% Ms. Pac-Man'' 13% Asteroids 7% Frostbite" 6% Gravitar 5% DQN Private Eye F2% Montezumas Revenge o Best linear learner 0 100 200 300 400 500 600 1,000 4,500%

Human-level control through deep reinforcement

Convolution Convolution Fully connected Fully connected

00:05:30 Google DeepMind ChallengeMatch LESEDCL 00:28:28

Human expert Supervised Learning Reinforcement Learning Self-play data Value network positions policynetwor olicynetwor Classification SelfPlay SelfPlay Regression

010 010 010 10

游戏状态空间复杂度游戏树复杂度井字棋 10^4 10^5 国际跳棋 10^21 10^31 国际象棋 10^46 10^123 中国象棋 10^48 10^150 五子棋 10^105 10^70 围棋 10^172 10^360 https://blog.csdn.net/AD _plus

信息集平均大小 1048 麻将 1015 桥牌 103 德州扑克围棋 0 1067 10121 1016210172 https://k 信集数具D_plus

AlphaStar LiquidMaNa  117/181 110/126

Google''s DeepMind AI Just Taught Itself To Walk It t  might Ilook goofy  D 0:00 / 1:50 Scrollfordetails o 非

SURREAL: Open-SourCe ReinforcementLearning Framework and Robot Manipulation BenC..

Al trained toControl trafc It only takes a tap on the brakes to start a traffic jam. 0:00 /1:42 Scrollfordetails cc

torcsRL:training a Reinforcement Learning Agent for TORCs   Reedy 90 270 60  1 0:00 / 3:54 Scrollfordetails o

cancer -diabetes chronic diseases -Anemia -hIV DynamicTreatment Regimes Mental illness (epilepsy,depression, Schizophrenia,Substance Addiction) sepsis Anesthesia others (Ventilation, Heparin Dosing, etc.) structured data medical image RL Automated Medical (segmentation,object Diagnosis detection/localization/tracing) Unstructured Data freetext Resource Scheduling & Allocation Healthcareresource scheduling,taskallocation Other general Optimal process domains control surgicalrobotoperation, FES, Communication rate control etc. drugdiscovery de novo design Hhealthmanagement physical actitie promotion, Weight management, etc.

Explore Content  Journal Information Publish With Us Subscribe nature > nature medicine > comm ent > article Camment Publeshedn0 January 201e [Gaaidelinesforree nforcem entlearninginthealthcare Omer Gottesman, Fredrik Johansson, Matthieu Kom orowski, Aldo Faisal, David Sontag, Finale Doshi-Velez Nature Medicine 25, 16–18(2019) Cite this article 9928 Accesses 33 Citations |138 Altmetric | Metrics In this Comment, we provide guidelines for reinforcement learning for decisions about [Ya nGent treatment thatwe tope will acceleratetterate anwhic obsereatienalcehort

导演席中国度日本防空识别区东防空究区

LS-610

七七

CCT 3 新间 Cccom 无人机战场上的“冷血杀手” 军情时间到

D 腾讯视频

· → Makeaction ai(left, right, or up) aa bserve a new rame stateS n Makeaction C2 • (stateg action, reward) trajectory: S1,O1,r1,S2,a2,r2, … , ST,aT,rT.

• Qπ (St, ) = E [ Ut | St = St, ].  Ut depends on states St, St+1,… , Sn and actions At, At+1,,An.

Qπ(St, ) = E [ Ut St = St, Regard St and at as observed values. Regard St+1,· , Sn and At+1, , An as random Variables.

Qπ(St, ) E[ Ut | St = St, ]. Regard St+1,, Sn and At+1,,An as random Variables.

●Qπ(, ) [U t = , ] St+1 ~ p (·|St, at . ), At+1 ~ π(· | St+1 ), Sn ~ p(· |Sn−1, An−1 ). ( An ~ π( · |Sn ).

• Qπ(St, 0 ) = E [Ut|St = St, = 0t]. F ReturnUtdepends on actionsAt,At+1,At+2, 0a a  andstates St,St+1,St+2,…  Actions arerandom: P[A = a |S = s = π(a|s). (Policyfunction.)  States are random: P[s'' = = S, A = ] = p(s''|s,a). (Statetr s''|S ansition.)

• Qπ(St, ) = E [ Ut | St = St, ]. Qπ(St, at) dependson St,at,π, andp. ( Qπ(St, at) is dependent of St+1, … , Sn and At +1,,An.

Definition: Optimal action-value function. • Q*(St,0t)= maxQπ(St,0t). TT

Definition:Discounted return. • Ut = Rt + γ Rt+1 + γ2 Rt+2 + …. + yn−t Rn. Definition: Action-value function. • Qπ(St, ) = E [ Ut | St = St, ]. Definition:State-value function. ⋅πs)= E [Qπ(,)]

Definition: Discounted return. • Ut = Rt + γ Rt+1 + γ2 Rt+2 . + yn−t Rn. Definition: Action-value function. • Qπ(St, ) = E [ Ut | St = St, ]. Definition: State-Value function. • Vπ(St) = E [Qπ(St,)] Taken W.r.t.the action A ~ π (· Ist).

Definition: Discounted return. • Ut = Rt + γ Rt+1 + γ2 Rt+2 + … + γn−t Rn. Definition: Action-value function. • Qπ(St, ) = E [ Ut | St = St, ]. Definition: State-value function. • Vπ(st) = E [Qπ(St, )] = ∑ π( |st) · Qπ(St, ). (Actions are discrete.) Taken W.r.t.the action A ~ π( Ist).

• Ut = Rt + γ Rt+1 + γ 2 Rt+2 + … + γn−t Rn· Definition:Action-value function. • Qπ (St, ) = E [ Ut | St = St, ]. • Vπ(st) = E [Qπ(St, )] = ∑ π ( |st) · Qπ(St, ). (Actions are discrete.) • Vπ(st) = E [Qπ(st, )] = ∫ π( |st) · Qπ(St, ) d . (Actions are continuous.)

-valuefunction: Qπ(S,)=E[UtlS =S, ]. • Given policy π, Qπ(s, ) evaluates how good it is for an agent to pick action while beingin state s.

-Valuefunction: Qπ(s,)=E[Ut|S =S, ]. • Given policy π, Qπ(s, ) evaluates how good it is for an agent to pick action while beingin states. • For fixed policy π, Vπ(s) evaluates how good the situation is in state s.

Suppose we have a good policy π(@|s). • random sampling: Ot ~ π( Ist).

Suppose we know the optimal action-value function Q*(s, @). • choose the action that maximizes the value: at = argmaxa Q*(st, a).

区

Ut = Rt + γ Rt+1 + γ 2 Rt+2 + … •Action-value function: Qπ(St, ) )  = E [Ut|St, ].

• Optimal action-value function: Q*(St,at) = maxQπ(St, at). TT
