# Lecture7-机器学习4-强化学习

> 主题库：人工智能　｜　源文件：Lecture7-机器学习4-强化学习.pdf　｜　59,571 字符　｜　340 个片段　｜　更新于 2026-11-11


人工智能
强化学习
Reinforcement Learning

Textbook
•Reinforcement Learning: An Introduction (Second 
Edition), Richard S. Sutton and Andrew G. Barto, 
MIT Press, Cambridge, MA, 2018.
2
nd
Edition PDF version:
http://incompleteideas.net/sutton/book/RLbook2018.pdf
2
nd
Edition Website:
http://incompleteideas.net/sutton/book/the-book-2nd.html
1
st
Edition html version:
http://incompleteideas.net/book/first/ebook/the-book.html
Richard S. Sutton 
Andrew G. Barto

Textbook
Reinforcement Learning State-of-the-Art, 
WieringM.A., 
Springer, 2016.
Algorithms for Reinforcement Learning, 
Csaba Szepesvári, 
Morgan & Claypool Publishers, 2010.
https://sites.ualberta.ca/~szepesva/rlbook.html

What is reinforcement learning?

About RL 
“Theideathatwelearnbyinteractingwithourenvironmentisprobablythe
firsttooccurtouswhenwethinkaboutthenatureoflearning.Whenan
infantplays,wavesitsarms,orlooksabout,ithasnoexplicitteacher,butit
doeshaveadirectsensori-motorconnectiontoitsenvironment.Exercising
thisconnectionproducesawealthofinformationaboutcauseandeffect,
abouttheconsequencesofactions,andaboutwhattodoinordertoachieve
goals.Throughoutourlives,suchinteractionsareundoubtedlyamajor
sourceofknowledgeaboutourenvironmentandourselves.Whetherweare
learningtodriveacarortoholdaconversation,weareallacutelyawareof
howourenvironmentrespondstowhatwedo,andweseektoinfluencewhat
happensthroughourbehavior.Learningfrominteractionisafoundational
ideaunderlyingnearlyalltheoriesoflearningandintelligence.”
——Richard S. Sutton 
Richard Sutto，
Distinguished research 
scientist at DeepMind 

About RL 
“Reinforcementlearningproblemsinvolvelearningwhattodo---howtomap
situationstoactions---soastomaximizeanumericalrewardsignal.Inan
essentialwaytheseareclosed-loopproblemsbecausethelearningsystem‘s
actionsinfluenceitslaterinputs.Moreover,thelearnerisnottoldwhichactions
totake,asinmanyformsofmachinelearning,butinsteadmustdiscoverwhich
actionsyieldthemostrewardbytryingthemout.Inthemostinterestingand
challengingcases,actionsmayaffectnotonlytheimmediaterewardbutalsothe
nextsituationand,throughthat,allsubsequentrewards.Thesethree
characteristics---beingclosed-loopinanessentialway,nothavingdirect
instructionsastowhatactionstotake,andwheretheconsequencesofactions,
includingrewardsignals,playoutoverextendedtimeperiods---arethethree
mostimportantdistinguishingfeaturesofthereinforcementlearningproblem.”
——Richard S. Sutton 

About RL 
RL, in a nutshell,is to “learnto make good 
sequences of decisionsthrough trail-and-errors 
with the environment”

About RL 
•Thus, there are four basic aspects in RL
•Optimization (good decisions)
•Delayed consequences (sequential)
•Exploration (trail-and-error)
•Generalization (learn)

About RL 
•The goal is to find an optimal (or near-optimal) way to make 
decisions
•The evaluation of optimality can be explicitly measured or 
provided in terms of utility functions, e.g., 
•the shortest path between two cities given a network of roads
•the fast speed that a robot is able to run
•the maximum area for a multi-robot system to cover
•the most money a gambling agent can win
•the least time for a group of vehicles to pass a crossroad
•the highest possibility to win a war
•...

Characteristics of RL 
What makes RL different from other related paradigms?
•Two fundamental problems in sequential decision making: 
learningvsplanning 智能规划
•Reinforcement Learning:
•The environment is initially unknown
•The agent interacts with the environment
•The agent improves its policy
•Planning
•A model of the environment is known
•The agent performs computations with its model (without any
•external interaction)
•The agent improves its policy
a.k.a. deliberation, reasoning, introspection, pondering, thought, search

What makes RL different from other related paradigms?
•Three fundamental problems in machine learning
Characteristics of RL 

What makes RL different from other related paradigms?
•Three fundamental problems in machine learning
•Supervised learning
•Classification or prediction from labeled (action,outcome) pairs
•No interactions
•No sequential decisions
•No explorations
•Unsupervised learning
•Discover inherent correlations among data
•No interactions
•No sequential decisions
•No explorations
Characteristics of RL 

InterdisciplinaryRL 
[from David Silver’s course]

Applicationsof RL 

Applications of RL–game playing
MnihV, KavukcuogluK, Silver D, et al. Human-level control through deep reinforcement learning[J]. 
nature, 2015, 518(7540): 529-533.

Applications of RL–game playing
Silver, David, et al. "Mastering the game of Go with deep neural networks and tree search."nature529.7587 (2016): 484-489.

Applications of RL–game playing
DotaOpenAIFive
(OpenAI,2018)
StarCraft AlphaStar
(DeepMind, 2019, Nature)
Texas Hold'emPluribus
(Facebook&CMU, 2019, Science )
Win professionals（top 10）
Win $1000/hour, i.e., $5/hand
Defeat world champion
宇宙原子数

Applications of RL –robot locomotion control

Applications of RL –robot manipulation control

Applications of RL–traffic control

Applications of RL–autonomous driving 

Applications of RL–healthcare
Yu C, Liu J, NematiS. Reinforcement learning in healthcare: A survey[J]. ACM Computing Survey, 2021

Applications of RL–UAV military

Applications of RL–UAV military

Applications of RL–UAV military

Terminologies

Terminology: state andaction
state푠(thisframe)
Action푎∈{left, right,up}
up
leftright
Agent

Terminology: state andaction
state푠(thisframe)
Action푎∈{left, right,up}
leftright
up
Agent

Terminology:policy
leftright
up
policy흅
• Policyfunction휋:푠,푎↦0,1:
휋푎|푠=ℙ퐴=푎|푆=푠.

Terminology:policy
leftright
up
policy흅
•Policyfunction휋:푠,푎↦0,1:
휋푎|푠=ℙ퐴=푎|푆=푠.

Terminology:policy
leftright
up
•Policyfunction휋:푠,푎↦0,1:
휋푎|푠=ℙ퐴=푎|푆=푠.
policy흅

Terminology:policy
leftright
up
•휋푎|푠is the probability of taking  
action 퐴= 푎given state 푠,e.g.,
•휋left|푠=0.2,
•휋right| s=0.1,
•휋up|푠=0.7.
policy흅

Terminology:policy
leftright
up
policy흅
•휋푎|푠is the probability of taking  
action 퐴= 푎given state 푠,e.g.,
•휋left|푠=0.2,
•휋right| s=0.1,
•휋up|푠=0.7.
•Upon observing state 푆= 푠, the  
agent’s action 퐴can berandom.

Terminology:policy
leftright
up
Random or deterministicpolicy?

Terminology:reward
•Collecta coin:푅=+1
reward푅

Terminology:reward
reward푅
•Collect acoin:
•Win thegame:
푅=+1
푅=+10000

Terminology:reward
reward푅
•Collect acoin:
•Win thegame:
푅=+1
푅=+10000
•Touch a Goomba: 푅=−10000
(gameover).

Terminology:reward
•Collect acoin:
•Win thegame:
푅=+1
푅=+10000
•Touch a Goomba: 푅=−10000
(gameover).
•Nothing happens: 푅=0
reward푅

Terminology: statetransition
statetransition
oldstate
newstate
action

Terminology: statetransition
•E.g., “up” action leads to a newstate.
statetransition
up
oldstate
newstate
action

Terminology: statetransition
•E.g., “up” action leads to a newstate.
•State transition can berandom.
•Randomness is from the environment.
statetransition
up
oldstate
newstate
action

Terminology: statetransition
•E.g., “up” action leads to a newstate.
•State transition can berandom.
•Randomness is from the environment.
statetransition
up
w.p. 0.8w.p. 0.2
oldstate
newstate
action

Terminology: statetransition
•E.g., “up” action leads to a newstate.
•State transition can berandom.
•Randomness is from theenvironment.
•푝푠′푠,푎=ℙ푆′=푠′푆= 푠, 퐴=푎.
statetransition
w.p. 0.8w.p. 0.2
oldstate
newstate
action

Agent-EnvironmentInteraction

Agent-EnvironmentInteraction
Environment
State푠
푡
Agent

Agent-EnvironmentInteraction
Environment
Action푎
푡
State푠
푡
Agent

State푠
d
Agent-EnvironmentInteraction
Environment
Agent
Action푎
푡
Reward푟
푡
State푠
푡+1

Two Sources ofRandomness

푠
left  
0.2
right  
0.1
up  
0.7
Randomness inActions

푠
퐴~휋(⋅|푠)
Given state 푠, the action can be  
random, e.g.,.
•휋“left”푠=0.2,
•휋“right”푠=0.1,
•휋“up”푠=0.7.
left  
0.2
right  
0.1
up  
0.7
Randomness inActions
Actions have randomness

Randomness inStates
푠
푎
•State transition can berandom.
•The environment generates the  
new state 푆′by
푆′~ 푝(⋅ |푠, 푎).

Randomness inStates
푠
푎
0.80.2
•State transition can berandom.
•The environment generates the  
new state 푆′by
푆′~ 푝(⋅ |푠, 푎).
Given state S=푠andA=푎
States transitions have randomness
푆′~ 푝(⋅ |푠, 푎)

•The randomness in action is from the policyfunction:
퐴~휋⋅푠.
•The randomness in state is from the state-transitionfunction:
푆′~푝⋅푠, 푎).
Two Sources ofRandomness

Play game usingAI
•Observe state 푠
푡
, select action 푎
푡
~ 휋⋅푠
푡
, and execute푎
푡
.
•The environment gives new state푠
푡+1
and reward푟
푡
.

Play game usingAI
푠
1
푎
1
•Observe state 푠
푡
, select action 푎
푡
~ 휋⋅푠
푡
, and execute푎
푡
.
•The environment gives new state푠
푡+1
and reward푟
푡
.
푠
2
푟
1

Play game usingAI
푠
1
푎
1
푎
2
푎
3
푠
2
푟
1
푠
3
푟
2
푠
4
푟
3
⋯
•Observe state 푠
푡
, select action 푎
푡
~ 휋⋅푠
푡
, and execute푎
푡
.
•The environment gives new state 푠
푡+1
and reward푟
푡
.

Play game usingAI
•(state, action, reward)trajectory:
푠
1
,푎
1
,푟
1
,푠
2
,푎
2
,푟
2
,⋯,푠
푛
,푎
푛
,푟
푛
.
•One episode is from the the beginning to the end (Mario wins ordies).
푠
1
푎
1
푎
2
푎
3
푠
2
푟
1
푠
3
푟
2
푠
4
푟
3
⋯

Play game usingAI
•(state, action, reward)trajectory:
푠
1
,푎
1
,푟
1
,푠
2
,푎
2
,푟
2
,⋯,푠
푛
,푎
푛
,푟
푛
.
•One episode is from the beginning to the end (Mario wins ordies).
•What is a goodpolicy?
•A good policy leads to bigcumulativereward:
•Use the rewards to guide the learning ofpolicy.

Rewards andReturns

Return
•푈
푡
=푅
푡
+푅
푡+1
+푅
푡+2
+푅
푡+3
+⋯
Definition:Return (a.k.a.cumulative futurereward).

Return
•푈
t 
= 푅
t
+푅
t + 1  
+ 푅
t + 2  
+ 푅
t+3 
+⋯
Definition:Return (aka cumulative futurereward).
Question:At time 푡, are 푅
t
and 푅
t + 1
equallyimportant?
•Which of the followings do youprefer?
•I give you $100 rightnow.
•I will give you $100 one yearlater.

Return
•푈
t 
= 푅
t
+푅
t + 1  
+ 푅
t + 2  
+ 푅
t+3 
+⋯
Definition:Return (aka cumulative futurereward).
Question:At time 푡, are 푅
t
and 푅
t + 1
equallyimportant?
•Which of the followings do youprefer?
•I give you $50 rightnow.
•I will give you $100 one yearlater.

Return
•푈
t 
= 푅
t
+푅
t + 1  
+ 푅
t + 2  
+ 푅
t+3 
+⋯
Definition:Return (aka cumulative futurereward).
Question:At time 푡, are 푅
t
and 푅
t + 1
equallyimportant?
•Which of the followings do youprefer?
•I give you $50 rightnow.
•I will give you $100 one yearlater.
•Future reward is less valuable than presentreward.
•푅
t + 1
should be given less weight than푅
t
.

DiscountedReturns
•푈
t 
= 푅
t
+푅
t + 1  
+ 푅
t + 2  
+ 푅
t+3 
+⋯
Definition:Return (aka cumulative futurereward).
•훾: discount factor (tuninghyper-parameter).
•푈
t
= 푅
t
+ 훾푅
t + 1
+ 훾
2
푅
t + 2
+ 훾
3
푅
t+3
+⋯
Definition:Discounted return (aka cumulative discounted futurereward).

DiscountedReturns
•푈
t
=푅
t
+훾푅
t + 1
+훾
2
푅
t + 2
+⋯+훾
n-t
푅
n
.
Definition:Discounted return (at time푡).

Randomness inReturns
At the end of the game, we observe푢
t
.
Definition:Discounted return (at time푡).
•We observe all the rewards,푟
t
,푟
t + 1
,푟
t + 2
,⋯,푟
n
.
•Wetherebyknowthe discountedreturn:
•푈
t
=푅
t
+훾푅
t + 1
+훾
2
푅
t + 2
+⋯+훾
n-t
푅
n
.

Randomness inReturns
At time 푡, the rewards, 푅
t
, ⋯ , 푅
n
, are random, so the return 푈
t
israndom.
Definition:Discounted return (at time푡).
•푈
t
=푅
t
+훾푅
t + 1
+훾
2
푅
t + 2
+⋯+훾
n-t
푅
n
.

Randomness inReturns
At time 푡, the rewards, 푅
t
, ⋯ , 푅
n
, are random, so the return 푈
t
israndom.
Definition:Discounted return (at time푡).
•푈
t
=푅
t
+훾푅
t + 1
+훾
2
푅
t + 2
+⋯+훾
n-t
푅
n
.
•For any 푖≥푡,Reward 푅
i
depends on 푆
i
and퐴
i
.
.•States can berandom:
•Actions can berandom:
•If either 푆
i
or 퐴
i
is random, then 푅
i
israndom.

Randomness inReturns
At time 푡, the rewards, 푅
t 
, ⋯ , 푅
n
, are random, so the return 푈
t
israndom.
Definition:Discounted return (at time푡).
•푈
t
=푅
t
+훾푅
t + 1
+훾
2
푅
t + 2
+⋯+훾
n-t
푅
n
.
•For any 푖≥푡,Reward 푅
i
depends on 푆
i
and퐴
i
.
•푈
t  
dependson푅
t
,푅
t + 1
,⋯,푅
n
.
•➔푈
t  
dependson푆
t 
,퐴
t 
, 푆
t + 1
,퐴
t + 1
,⋯,
푆
n 
,퐴
n
.

Randomness inReturns
Time
TimetEnd

Randomness inReturns
Time
TimetEnd
푅
n
푅
t
푅
t + 1
푅
t + 2
⋯
푈
t
is a randomvariable

Randomness inReturns
Time
TimeEnd
푟
t
푟
t + 1
푟
t + 2
⋯푟
n
t
푢
t
is an observedvalue

ValueFunctions

Action-ValueFunction푄푠,푎
Definition:Discountedreturn(aka cumulative discounted futurereward).

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
•.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.
Actions are random：States are random：

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

Action-ValueFunction푄푠,푎
Definition: Action-valuefunctionfor policy 휋.
Definition:Discountedreturn.

State-ValueFunction푉푠

State-ValueFunction푉푠

State-ValueFunction푉푠

State-ValueFunction푉푠

Understanding the ValueFunctions

Understanding the ValueFunctions

Evaluating ReinforcementLearning

How does AI control the game

•Gym is a toolkit for developing and comparing reinforcement learningalgorithms.
•https://gym.openai.com/
OpenAIGym
Cart Pole
Pendulum
Classical controlproblems

•Gym is a toolkit for developing and comparing reinforcement learningalgorithms.
•https://gym.openai.com/
OpenAIGym
Pong
SpaceInvader
AtariGames
Breakout

•Gym is a toolkit for developing and comparing reinforcement learningalgorithms.
•https://gym.openai.com/
OpenAIGym
Ant
Humanoid
MuJoCo(Continuous controltasks.)
HalfCheetah

OpenAIGym

Play CartPoleGame
importgym
env =gym.make('CartPole-v0')
•Get the environment of CartPole fromGym.
•“env” provides states andreward.

Play CartPoleGame
state =env.reset()
for t inrange(100):  
env.render()  
print(state)
action =env.action_space.sample()
state, reyard, done, info =env.step(action)
ifdone:
print('Finished')
break
env.close()
A randomaction.
“done=1” means finished (win or lose thegame)
A window pops up renderingCartPole.

Summary

•Agent
•Environment
•State푠
•Action 푎
•Reward푟
Summary
Terminologies

•Agent
•Environment
•State푠
•Action푎
•Reward푟
•Policy휋푎푠
•Statetransition푝푠'푠,푎
Summary
Terminologies

•Agent
•Environment
•State푠
•Action푎
•Reward푟
•Policy휋푎푠
•Statetransition푝푠' 푠,푎
Summary
TerminologiesReturn and Value
•Return:

•Agent
•Environment
•State푠
•Action푎
•Reward푟
•Policy휋푎푠
•Statetransition푝푠' 푠,푎
Summary
TerminologiesReturn and Value

Summary
强化学习问题形式化:
强化学习的学习目标: 找到能最大化累计奖励的策略
The RL problem of a single agent  can be regarded as a Markov decision process (MDP). 马尔可
夫决策过程（Markov Decision Process, MDP）是Sequential decision的数学模型，用于在系
统状态具有马尔可夫性质的环境中模拟智能体可实现的随机性策略与回报
单智能体强化学习问题可以形式化定义为马尔可夫决策过程MDP六元组.
푺,푨,푹,푻,푷
ퟎ
,휸。其中, 푺表示状态空间，푨表示动作空间，푹=푹(풔,풂)
表示奖励函数, 푻:푺×푨×푺→[ퟎ,ퟏ]表示状态转移函数，푷
ퟎ
表示初始状态
分布，휸是折扣因子。

Play game usingAI
푎
t-1
푠
t-1
푟
t-1
푠
t
푟
t-1
⋯
Timet

Play game usingAI
푎
t-1
푠
t-1
푟
t-1
푠
t
푟
t-1
⋯
푎
t
휋
Timet

Play game usingAI

Play game usingAI



Value-Based ReinforcementLearning

Action-ValueFunctions

DiscountedReturn
•The return depends on actions 퐴
t
, 퐴
t +1
,퐴
t +2
,⋯andstates푆
t
,푆
t +1
,푆
t +2
,⋯
•Actions arerandom:ℙ  퐴= 푎| 푆=푠=휋푎푠.
•Statesarerandom:ℙ  푆’= 푠’ |푆= 푠, 퐴=푎=푝푠’푠,푎.
(Policy function.)  
(Statetransition.)
•푈
t
= 푅
t
+ 훾푅
t +1
+ 훾
2
푅
t +2
+ 훾
3
푅
t+3
+⋯
Definition:Discounted return (aka cumulative discounted futurereward).

Action-ValueFunctions푄푠,푎
Definition: Action-value function for policy휋.
•푄
π
푠
t 
,푎
t
=피푈
t 
|푆
t
= 푠
t 
, 퐴
t
=푎
t
.
•Taken w.r.t. actions 퐴
t +1
, 퐴
t +2
,퐴
t+3
,⋯andstates푆
t +1
,푆
t +2
,푆
t +3
,⋯
•Integrate out everything except for the observations: 퐴
t
= 푎
t
and 푆
t
=푠
t
.
•푈
t
= 푅
t
+ 훾푅
t +1
+ 훾
2
푅
t +2
+ 훾
3
푅
t+3
+⋯
Definition:Discounted return (aka cumulative discounted futurereward).

Action-ValueFunctions푄푠,푎
Definition: Optimal action-valuefunction.
π
•푄
⋆
푠
t
,푎
t
=max푄
π
푠
t 
,푎
t
.
•Whatever policy function 휋is used, the result of taking 푎
t
at state푠
t
cannot be betterthan푄
⋆
푠
t
,푎
t
.
•푈
t
= 푅
t
+ 훾푅
t +1
+ 훾
2
푅
t +2
+ 훾
3
푅
t+3
+⋯
Definition:Discounted return (aka cumulative discounted futurereward).
Definition: Action-value function for policy휋.
•푄
π
푠
t 
,푎
t
=피푈
t 
|푆
t
= 푠
t 
, 퐴
t
=푎
t
.

Deep Q-Network(DQN)

Approximate the QFunction
Goal:Win the game (≈ maximize the totalreward.)
Question: If weknow푄
⋆
푠,푎, what is the bestaction?

Approximate the QFunction
Goal:Win the game (≈ maximize the totalreward.)
Question: If weknow푄
⋆
푠,푎, what is the bestaction?
•Obviously, the bestactionis푎
⋆  
=argmax푄
⋆
푠,푎.
푎
푄
⋆ 
is an indicator of how good it is for an agent  
to pick action 푎while being in state푠.

Approximate the QFunction
Goal:Win the game (≈ maximize the totalreward.)
•Obviously, the bestactionis푎
⋆  
=argmax푄
⋆
푠,푎.
푎
Question: If weknow푄
⋆
푠,푎, what is the bestaction?
Challenge: We do notknow푄
⋆
푠,푎.
•Solution:Deep Q Network(DQN)
•Use neural network 푄(푠, 푎; 퐰) toapproximate푄
⋆
푠,푎.

Conv
state푠
feature
Dense
Deep Q Network(DQN)
•Input shape: size of thescreenshot.
•Outputshape:dimension of actionspace.
푄푠, “left”;퐰
푄푠,“right”;퐰
푄푠, “up”;퐰

Conv
state푠
feature
Dense
푄푠,“right”;퐰=1000
푄푠,“up”;퐰=3000
Deep Q Network(DQN)
Question:Based on the predictions, what should be theaction?
•Input shape: size of thescreenshot.
•Outputshape:dimension of actionspace.
푄푠,“left”;퐰=2000

푠
t
푎
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
Apply DQN to PlayGame

푠
t
푎
t
푠
t+1
푟
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
푠
t +1
~푝(⋅ |푠
t 
,푎
t
)
Apply DQN to PlayGame

푎
t +1
푎
t +1
= argmax
푎
푄(푠
t +1
, 푎;퐰)
Apply DQN to PlayGame
푠
t
푎
t
푠
t+1
푟
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
푠
t +1
~푝(⋅ |푠
t 
,푎
t
)

푎
t +1
푠
t +2
푟
t +1
푠
t +2
~푝(⋅ |푠
t +1
,푎
t +1
)
Apply DQN to PlayGame
푎
t +1
= argmax
푎
푄(푠
t +1
, 푎;퐰)
푠
t
푎
t
푠
t+1
푟
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
푠
t +1
~푝(⋅ |푠
t 
,푎
t
)

푎
t +2
푎
t +2
= argmax
푎
푄(푠
t +2
, 푎;퐰)
Apply DQN to PlayGame
푎
t +1
푠
t +2
푟
t +1
푠
t +2
~푝(⋅ |푠
t +1
,푎
t +1
)
푎
t +1
= argmax
푎
푄(푠
t +1
, 푎;퐰)
푠
t
푎
t
푠
t+1
푟
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
푠
t +1
~푝(⋅ |푠
t 
,푎
t
)

⋯
푠
t +3
푟
t +2
푠
t +3
~푝(⋅ |푠
t +2
,푎
t +2
)
Apply DQN to PlayGame
푎
t +2
푎
t +2
= argmax
푎
푄(푠
t +2
, 푎;퐰)
푎
t +1
푠
t +2
푟
t +1
푠
t +2
~푝(⋅ |푠
t +1
,푎
t +1
)
푎
t +1
= argmax
푎
푄(푠
t +1
, 푎;퐰)
푠
t
푎
t
푠
t+1
푟
t
푎
t
= argmax
푎
푄(푠
t
, 푎;퐰)
푠
t +1
~푝(⋅ |푠
t 
,푎
t
)
Question:How to learn the DQN?

Temporal Difference (TD)Learning
Reference
1.Sutton and others: A convergent O(n) algorithm for off-policy temporal-difference learning with linear function  
approximation. In NIPS,2008.
2.Sutton and others: Fast gradient-descent methods for temporal-difference learning with linear function  
approximation. In ICML,2009.

•I want to drive from NYC toAtlanta.
•Model푄퐰estimates the time cost, e.g., 1000minutes.
NYC
Atlanta
Example
Question: How do I update themodel?
1000 minutes(estimate).

•I want to drive from NYC toAtlanta.
•Model푄퐰estimates the time cost, e.g., 1000minutes.
NYC
Atlanta
Example
Question: How do I update themodel?
•Make a prediction: 푞= 푄(퐰), e.g., 푞=1000.
•Finish the trip and get the target 푦, e.g., 푦=860.
1000 minutes(estimate).
860 minutes(actual).

•I want to drive from NYC toAtlanta.
•Model푄퐰estimates the time cost, e.g., 1000minutes.
NYC
Atlanta
Example
Question: How do I update themodel?
•Make a prediction: 푞= 푄(퐰), e.g., 푞=1000.
•Finish the trip and get the target 푦, e.g., 푦=860.
2
•Loss:퐿=
1
푞− 푦
2
.
•Gradient:
∂퐰∂퐰∂q∂퐰
∂L
= 
∂q
⋅ 
∂L
=푞− 푦⋅ 
∂Q퐰
.
∂퐰
t
•Gradient descent: 퐰
t+1
= 퐰
t
− 훼⋅  
∂L
│
퐰=퐰
.
1000 minutes(estimate).
860 minutes(actual).
Issue: Too naïve,need the complete flow for the update.

•I want to drive from NYC toAtlanta.
•Model푄퐰estimates the time cost, e.g., 1000minutes.
NYC
Atlanta
Example
Question: How do I update themodel?
•Can I update the model before finishing thetrip?

•I want to drive from NYC to Atlanta (viaDC).
•Model푄퐰estimates the time cost, e.g., 1000minutes.
NYC
Atlanta
Example
Question: How do I update themodel?
•Can I update the model before finishing thetrip?
•Can I get a better 퐰as soon as I arrived atDC?
DC

NYC
Atlanta
Temporal Difference (TD)Learning
1000 minutes(estimate).
•Model’sestimate:
NYCtoAtlanta:1000 minutes(estimate).

NYC
DC
Temporal Difference (TD)Learning
300 minutes(actual).
•Model’sestimate:
NYCtoAtlanta:1000 minutes(estimate).
•I arrived at DC; actual timecost:
NYCtoDC:300 minutes(actual).

•Model’sestimate:
NYCtoAtlanta:1000 minutes(estimate).
•I arrived at DC; actual timecost:
NYCtoDC:300 minutes(actual).
•Model now updates itsestimate:
DC toAtlanta:600 minutes(estimate).
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).

•Model’sestimate:푄퐰= 1000minutes.
•Updated estimate: 300 + 600 = 900minutes.
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).
TDtarget.

•Model’sestimate:푄퐰= 1000minutes.
•Updated estimate: 300 + 600 = 900minutes.
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).
TDtarget.
•TD target y = 900 is a more reliable estimate than1000.

•Model’sestimate:푄퐰= 1000minutes.
•Updated estimate: 300 + 600 = 900minutes.
•Loss:퐿=
1
푄퐰− 푦
2
.
2
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).
TDtarget.
•TD target y = 900 is a more reliable estimate than1000.
TDerror

•Model’sestimate:푄퐰= 1000minutes.
•Updated estimate: 300 + 600 = 900minutes.
2
•Loss:퐿=
1
푄퐰− 푦
2
.
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).
TDtarget.
•TD target y = 900 is a more reliable estimate than1000.
•Gradient:
∂L
=1000−900⋅ 
∂Q퐰
.
∂퐰∂퐰
TDerror

•Model’sestimate:푄퐰= 1000minutes.
•Updated estimate: 300 + 600 = 900minutes.
2
•Loss:퐿=
1
푄퐰− 푦
2
.
•Gradient:
∂L
=1000−900⋅ 
∂Q퐰
.
∂퐰∂퐰
∂퐰
t
•Gradient descent: 퐰
t+1
= 퐰
t
− 훼⋅  
∂L
│
퐰=퐰
.
NYC
DC
Atlanta
Temporal Difference (TD)Learning
600 minutes(estimate).
300 minutes(actual).
TDtarget.
•TD target y = 900 is a more reliable estimate than1000.

•Model’sestimates:
•NYC to Atlanta: 1000minutes.
•DC to Atlanta: 600minutes.
•➔NYC to DC: 400minutes.
NYC
DC
Atlanta
Why does TD learningwork?
600 minutes(estimate).

•Model’sestimates:
•NYC to Atlanta: 1000minutes.
•DC to Atlanta: 600minutes.
•➔NYC to DC: 400minutes.
•Groundtruth:
•NYC to DC: 300minutes.
•TD error: 훿= 400 − 300 =100
NYC
DC
Atlanta
Why does TD learningwork?
600 minutes(estimate).
300 minutes(actual).

TD Learning forDQN

•In the “driving time” example, we have theequation:
푇
N YC→AT L
≈푇
NYC→DC
+ 푇
DC→ATL
.
How to apply TD learning toDQN?
Model’sestimateActual timeModel’sestimate

•In the “driving time” example, we have theequation:
푇
NYC→ATL
≈푇
NYC→DC 
+ 푇
DC→ATL
.
How to apply TD learning toDQN?
Model’sestimateActual timeModel’sestimate
•In deep reinforcementlearning:
푄푠
t
,푎
t
;퐰≈ 푟
t
+ 훾⋅푄푠
t +1
,푎
t +1
;퐰.

•푈
t
= 푅
t
+ 훾⋅ 푅
t+1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+ 훾
4
⋅ 푅
t+4
+⋯
How to apply TD learning toDQN?
Definition of discountedreturn:
=훾⋅푅
t+1
+ 훾⋅ 푅
t+2
+ 훾
2
⋅ 푅
t+3
+ 훾
3
⋅ 푅
t+4
+⋯

How to apply TD learning toDQN?
•푈
t
= 푅
t
+ 훾⋅ 푅
t +1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+ 훾
4
⋅ 푅
t+4
+⋯
= 푅
t
+훾⋅푅
t +1
+ 훾⋅ 푅
t +2
+ 훾
2
⋅ 푅
t +3
+ 훾
3
⋅ 푅
t +4
+⋯
=푈
t +1

How to apply TD learning toDQN?
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.
•푈
t
= 푅
t
+ 훾⋅ 푅
t +1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+ 훾
4
⋅ 푅
t+4
+⋯
= 푅
t
+훾⋅푅
t +1
+ 훾⋅ 푅
t +2
+ 훾
2
⋅ 푅
t +3
+ 훾
3
⋅ 푅
t +4
+⋯
=푈
t +1

How to apply TD learning toDQN?
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.
•DQN’soutput,푄푠
t
,푎
t
;퐰, is anestimateof푈
t
.
•DQN’soutput,푄푠
t +1
,푎
t +1
;퐰, is anestimateof푈
t +1
.
TD learning forDQN:

How to apply TD learning toDQN?
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.
estimateof푈
t +1
≈피[푈
t+1
]
estimateof푈
t
≈피[푈
t
]
TD learning forDQN:
•DQN’soutput,푄푠
t
,푎
t
;퐰, is anestimateof푈
t
.
•DQN’soutput,푄푠
t +1
,푎
t +1
;퐰, is anestimateof푈
t +1
.
•Thus,푄푠
t
,푎
t
;퐰≈피푅
t
+ 훾⋅ 푄푆
t +1
,퐴
t +1
;퐰.

How to apply TD learning toDQN?
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.
TDtargetPrediction
TD learning forDQN:
•DQN’soutput,푄푠
t
,푎
t
;퐰, is anestimateof푈
t
.
•DQN’soutput,푄푠
t +1
,푎
t +1
;퐰, is anestimateof푈
t +1
.
•Thus,푄푠
t
,푎
t
;퐰≈푟
t
+ 훾⋅푄푠
t +1
,푎
t +1
;퐰.

Train DQN using TDlearning
•Prediction:푄푠
t
,푎
t
;퐰
t
.
•TDtarget:
푦
t
= 푟
t
+ 훾⋅푄(푠
t +1
, 푎
t +1
;퐰
t
)

Train DQN using TDlearning
a
•Prediction:푄푠
t
,푎
t
;퐰
t
.
•TDtarget:
푦
t
= 푟
t
+ 훾⋅푄푠
t +1
, 푎
t +1
;퐰
t
= 푟
t
+ 훾⋅max푄푠
t+1
,푎;퐰
t
.

Train DQN using TDlearning
•Prediction:푄푠
t
,푎
t
;퐰
t
.
•TDtarget:
푦
t
= 푟
t
+ 훾⋅푄푠
t +1
, 푎
t +1
;퐰
t
= 푟
t
+ 훾⋅max푄푠
t +1
,푎;퐰
t
.
•Loss:퐿
t
=
1
2
a
푄푠
t 
,푎
t 
;퐰− 푦
t
2
.
∂L
t
∂퐰
퐰=퐰
t
•Gradientdescent:퐰
t +1
= 퐰
t
−훼⋅│.

Summary

Definition: Optimal action-valuefunction.
π
•푄
⋆
푠
t
,푎
t
=max피푈
t 
|푆
t
= 푠
t
, 퐴
t
=푎
t
.
Value-Based ReinforcementLearning

Definition: Optimal action-valuefunction.
π
DQN:Approximate푄
⋆
푠,푎using a neural network(DQN).
•푄푠,푎;퐰is a neural network parameterized by퐰.
•Input: observed state푠.
•Output: scores for all the action 푎∈풜.
Value-Based ReinforcementLearning
•푄
⋆
푠
t
,푎
t
=max피푈
t 
|푆
t
= 푠
t
, 퐴
t
=푎
t
.

Temporal Difference (TD)Learning
Algorithm: One iteration of TDlearning.
1.Observe state 푆
t
= 푠
t
and perform action 퐴
t
= 푎
t
.  
2.Predictthe value:푞
t
=푄푠
t
,푎
t
;퐰
t
.
t
3.Differentiatethevaluenetwork:퐝=
∂Qs
t
,a
t
;퐰
∂퐰
퐰=퐰
t
│.

1.Observe state 푆
t
= 푠
t
and perform action 퐴
t
= 푎
t
.  
2.Predictthe value:푞
t
=푄푠
t
,푎
t
;퐰
t
.
t
3.Differentiatethevaluenetwork:퐝=
∂Qs
t
,a
t
;퐰
∂퐰
퐰=퐰
t
│.
a
4.Environment provides new state 푠
t+1
and reward 푟
t
. 
5.ComputeTDtarget:푦
t
=푟
t
+ 훾⋅ max푄푠
t+1
,푎;퐰
t
.
Temporal Difference (TD)Learning
Algorithm: One iteration of TDlearning.

1.Observe state 푆
t
= 푠
t
and perform action 퐴
t
= 푎
t
.  
2.Predictthe value:푞
t
=푄푠
t
,푎
t
;퐰
t
.
t
3.Differentiatethevaluenetwork:퐝=
4.Environment provides new state 푠
t +1
and reward 푟
t
.  
5.ComputeTDtarget:푦
t
=푟
t
+ 훾⋅ max푄푠
t+1
,푎;퐰
t
.
a
6.Gradientdescent:퐰
t +1
= 퐰
t
−훼⋅푞
t
−푦
t
⋅퐝
t
.
Temporal Difference (TD)Learning
Algorithm: One iteration of TDlearning.
∂Qs
t
,a
t
;퐰
∂퐰
퐰=퐰
t
│.

Play Breakout usingDQN
(The video was posted on YouTube byDeepMind)

Policy-Based ReinforcementLearning

Policy FunctionApproximation

Policy Function 휋푎푠
•Why not just pre-program a policy?
•No or limited knowledge of how the world functions 
•A policy that predesigned for a domain might not work in 
other related domains.

•Policyfunction휋푎푠is a probability density function (PDF).
•It takes state 푠asinput.
•It output the probabilities for all the actions,e.g.,
휋left푠
휋right푠
휋up푠
=0.2,
=0.1,
=0.7.
•Randomly sample action 푎random drawn from thedistribution.
Policy Function 휋푎푠

Canwedirectlylearn a policyfunction휋푎푠?
Action푎
1
Action푎
2
Action푎
3
Action푎
4
⋯
State 푠
1
State 푠
2
State 푠
3
⋮
•If there are only a few states and actions, then yes, wecan.
•Draw a table (matrix) and learn theentries.

•If there are only a few states and actions, then yes, wecan.
•Draw a table (matrix) and learn theentries.
•What if there are too many (or infinite) states oractions?
Canwedirectlylearn a policyfunction휋푎푠?
Action푎
1
Action푎
2
Action푎
3
Action푎
4
⋯
State 푠
1
State 푠
2
State 푠
3
⋮

PolicyNetwork휋푎푠;훉
Policy network: Use a neural net toapproximate휋푎|푠.
•Usepolicynetwork휋푎|푠;훉to approximate휋푎|푠.
•훉: trainable parameters of the neuralnet.

Conv
state푠
t
feature
Dense
“left”,0.2
“right”,0.1
“up”,0.7
Softmax
PolicyNetwork휋푎푠;훉
Policy network: Use a neural net toapproximate휋푎|푠.
•Usepolicynetwork휋푎|푠;훉to approximate휋푎|푠.
•훉: trainable parameters of the neuralnet.

•
•Here,풜=“left”,“right”,“up”is the set allactions.
•That is why we use softmaxactivation.
Conv
state푠
t
feature
Dense
“left”,0.2
“right”,0.1
“up”,0.7
Softmax
PolicyNetwork휋푎푠;훉

State-Value FunctionApproximation

Action-ValueFunction
•U 
t  
= 푅
t 
+ 훾⋅ 푅
t+1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+⋯
Definition:Discountedreturn.
•The return depends on actions 퐴
t  
, 퐴
t+1
,퐴
t+2
,⋯and states 푆
t   
, 푆
t+1
, 푆
t+2
,⋯
•Actions arerandom:ℙ  퐴= 푎| 푆=푠=휋푎푠.(Policyfunction.)
•Statesarerandom:ℙ  푆’= 푠’|푆= 푠, 퐴=푎=푝푠’  푠,푎.(Statetransition.)

Action-ValueFunction
Definition: Action-valuefunction.
•푄
SC
푠
>
,푎
>
=피푈
>     
|푆
>     
= 푠
>
,퐴
>        
=푎
>
.
•U 
t  
= 푅
t 
+ 훾⋅ 푅
t+1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+⋯
Definition:Discountedreturn.
The expectation is taken w.r.t.  
actions 퐴
t+1
, 퐴
t+2
, 퐴
t+3
, ⋯  
and states 푆
t+1
, 푆
t+2
, 푆
t+3
,⋯

State-ValueFunction
Definition: Action-valuefunction.
•푄
S
푠
>
,푎
>
=피푈
>     
|푆
>     
= 푠
>
,퐴
>        
=푎
>
.
Definition: State-valuefunction.
•푉
S
푠
>
=피
V
푄
S
푠
>
,퐴
•U 
t  
= 푅
t 
+ 훾⋅ 푅
t+1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+⋯
Definition:Discountedreturn.
Integrateout action퐴~휋(⋅|푠
t
).

State-ValueFunction
Definition: Action-valuefunction.
•푄
S
푠
>
,푎
>
=피푈
>     
|푆
>     
= 푠
>
,퐴
>        
=푎
>
.
Definition: State-valuefunction.
•푉
S
푠
>
=피
V
푄
S
푠
>
,퐴=
∑
C
휋푎푠
>
⋅푄
S
푠
>
,푎.
•U 
t  
= 푅
t 
+ 훾⋅ 푅
t+1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+⋯
Definition:Discountedreturn.
Integrateout action퐴~휋(⋅|푠
t
).

Policy-Based ReinforcementLearning
•푉
S
푠
>
=피
V
푄
S
푠
>
,퐴=
∑
C
휋푎푠
>
⋅푄
S
푠
>
,푎.
Definition: State-valuefunction.
Approximate state-valuefunction.
•Approximate policyfunction휋푎푠
t
by policynetwork휋푎|푠
t  
;훉.

Policy-Based ReinforcementLearning
•푉
S
푠
>
=피
V
푄
S
푠
>
,퐴=
∑
C
휋푎푠
>
⋅푄
S
푠
>
,푎.
Definition: State-valuefunction.
Approximate state-valuefunction.
•Approximate policyfunction휋푎푠
t
by policynetwork휋푎|푠
t  
;훉.
•Approximate valuefunction푉휋푠
t
by:
푉푠
t   
;훉=
∑
푎
휋푎푠
t   
;훉⋅푄
휋
푠
t    
,푎.

Policy-Based ReinforcementLearning
•푉푠;훉=
∑
C
휋푎푠;훉⋅푄
S
푠,푎.
Definition: Approximate state-valuefunction.
Policy-based learning: Learn 훉thatmaximizes퐽훉=피
S
푉푆;훉.

•Update policy by: 훉← 훉+ 훽⋅ 
Policy-Based ReinforcementLearning
•푉푠;훉=
∑
C
휋푎푠;훉⋅푄
S
푠,푎.
Definition: Approximate state-valuefunction.
•Observe state푠.
Policygradient
(Stochastic)
Policy-based learning: Learn 훉thatmaximizes퐽훉=피
S
푉푆;훉.
How to improve 훉? Policy gradientascent!

PolicyGradient
Reference
•Sutton and others: Policy gradient methods for reinforcement learning with function approximation. In NIPS,  
2000.

•
PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.

•
PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.

•
PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
Push derivative inside the summation

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
^훉
= 
∑
C 
^ S (C|`;훉)
⋅푄
S
푠,푎
Pretend 푄
휋
is independent of 훉.  
(It may not betrue.)
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
Policy Gradient
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
•

PolicyGradient
•푉푠;훉=
∑
푎
휋푎푠;훉⋅푄
휋
푠,푎.
Definition: Approximate state-valuefunction.
Policy gradient:Derivativeof푉푠;훉w.r.t.훉.
The expectation is taken w.r.t. the random variable 퐴~휋(⋅ |푠;훉).
•
Note: This derivation is over-simplified and notrigorous.

PolicyGradient
Policygradient:

Calculate PolicyGradient
PolicyGradient:

Calculate PolicyGradient
1.Randomly sample an action according to 휋(⋅ |푠;훉).
PolicyGradient:

2.Calculate퐠, 훉= 
Calculate PolicyGradient
1.Randomly sample an action aaccording to 휋(⋅ |푠;훉).
PolicyGradient:

3.Use퐠, 훉as an approximation to the policy gradient 
2.Calculate퐠, 훉= 
Calculate PolicyGradient：Monte Carlo Approximate
1.Randomly sample an action according to 휋(⋅ |푠;훉).
PolicyGradient:

Update policy network using policygradient

Algorithm
1.Observe the state푠
t  
.
2.Randomly sample action 푎
t 
accordingto휋⋅푠
t   
;훉
t
.

Algorithm
1.Observe the state푠
t  
.
2.Randomly sample action 푎
t
accordingto휋⋅푠
t   
;훉
t    
.
3.Compute푞
t 
≈푄
S
푠
t   
,푎
t  
(some estimate).
4.Differentiatepolicynetwork:

Algorithm
1.Observe the state푠
t  
.
2.Randomly sample action 푎
t
accordingto휋⋅푠
t    
;훉
t    
.
3.Compute푞
t 
≈푄
S
푠
t   
,푎
t  
(some estimate).
4.Differentiatepolicynetwork:
5.(Approximate)policygradient:
6.Updatepolicynetwork:

1.Observe the state푠
t  
.
2.Randomly sample action 푎
t
accordingto휋(⋅|푠
t    
;훉
t      
).
3.Compute푞
t 
≈푄
S
푠
t   
,푎
t  
(some estimate).
Algorithm
4.Differentiatepolicynetwork:
5.(Approximate)policygradient:
6.Updatepolicynetwork:
How?

1.Observe the state푠
t  
.
2.Randomly sample action 푎
t
accordingto휋(⋅|푠
t    
;훉
t      
).
3.Compute푞
t 
≈푄
S
푠
t   
,푎
t  
(some estimate).
Algorithm
3
.
4.Differentiatepolicynetwork:퐝
l,>
=
^defS(C
m
|`
m
,훉) 
￨
훉n훉
.
^훉
m
5.(Stochastic)policygradient:퐠o훉
>
≈ 푞
> 
⋅퐝
l,>
.
6.Updatepolicynetwork:훉
>L5  
= 훉
>      
+ 훽⋅ 퐠o훉
>
.
How?
Option 1:REINFORCE.
•Play the game to the end and generate thetrajectory:
푠
1
,푎
1
,푟
1
,푠
2
,푎
2
,푟
2
,⋯,푠
T
,푎
T
,푟
T
.
•Compute the discounted return , for all 푡.
•Since, we can use 푢
t 
to approximate.
•➔Use 푞
t  
=푢
t  
.

Algorithm
4.Differentiatepolicynetwork:퐝
l,>
=
^defS(C
m
|`
m
,훉) 
￨
훉n훉
.
^훉
m
5.(Stochastic)policygradient:퐠o훉
>
≈ 푞
> 
⋅퐝
l,>
.
6.Updatepolicynetwork:훉
>L5  
= 훉
>      
+ 훽⋅ 퐠o훉
>
.
Option 2: Approximate using a neuralnetwork.
•This leads to the actor-criticmethod.
1.Observe the state푠
t  
.
2.Randomly sample action 푎
t
accordingto휋(⋅|푠
t    
;훉
t      
).
3.Compute푞
t 
≈푄
S
푠
t   
,푎
t  
(some estimate).
3
.
How?

Summary

•If a good policy function 휋is known, the agent can be controlled  
by thepolicy:randomly sample
•Approximate policyfunction휋푎푠by policynetwork휋푎푠;훉.
•Learn the policy network by policy gradientalgorithm.
•Policy gradient algorithm learn 훉thatmaximizes
Policy-BasedLearning

Actor-CriticMethods

Actor-Critic  
Methods
Value-Based  
Methods
Policy-Based  
Methods

Value Network and PolicyNetwork

State-Value FunctionApproximation
Definition: State-valuefunction.
•푉
휋
푠=
∑
푎
휋푎푠⋅푄
휋
푠,푎

State-Value FunctionApproximation
Definition: State-valuefunction.
Policy network(actor):
•Use neural net휋푎|푠;훉to approximate휋푎|푠.
•훉: trainable parameters of the neuralnet.
•푉
휋
푠=
∑
푎
휋푎푠⋅푄
휋
푠,푎

State-Value FunctionApproximation
Definition: State-valuefunction.
Policy network(actor):
•Use neural net휋푎|푠;훉to approximate휋푎|푠.
•훉: trainable parameters of the neuralnet.
Value network(critic):
•Use neuralnet푞푠,푎;퐰toapproximate푄
휋
(푠,푎)
•퐰: trainable parameters of the neuralnet.
•푉
휋
푠=
∑
푎
휋푎푠⋅푄
휋
푠,푎

State-Value FunctionApproximation
Definition: State-valuefunction.
Policy network(actor):
•Use neural net휋푎|푠;훉to approximate휋푎|푠.
•훉: trainable parameters of the neuralnet.
Value network(critic):
•Use neuralnet푞푠,푎;퐰toapproximate푄
휋
푠,푎.
•퐰: trainable parameters of the neuralnet.
•푉
휋
푠=
∑
푎
휋푎푠⋅푄
휋
푠,푎≈
∑
푎
휋푎푠;휃⋅푞(푠,푎;풘)

Conv
state푠
Dense
“right”,0.1
“up”,0.7
Softmax
PolicyNetwork(Actor):휋푎푠;훉
•Input: state 푠, e.g., a screenshot of SuperMario.
•Output: probability distribution over theactions.
•Let 풜be the set all actions, e.g.,풜=“left”,“right”,“up”.
•
∑
푎∈풜
휋푎푠;휃=1.(Thatiswhyweusesoftmaxactivation.)
“left”, 0.2

ValueNetwork(Critic):푞푠, 푎;퐰

Actor-CriticMethod
policy network(actor)value network(critic)

Train the NeuralNetworks

Train thenetworks
Definition: State-value function approximated using neuralnetworks.
Training: Update the parameters 훉and퐰.
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.

Train thenetworks
Definition: State-value function approximated using neuralnetworks.
Training: Update the parameters 훉and퐰.
•Update policynetwork 휋푎푠;훉to increase the state-value푉푠;훉,퐰.
•Actor gradually performsbetter.
•Supervision is purely from the value networkq(critic).
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.

Train thenetworks
Definition: State-value function approximated using neuralnetworks.
Training: Update the parameters 훉and퐰.
•Update policynetwork 휋푎푠;훉to increase the state-value푉푠;훉,퐰.
•Actor gradually performsbetter.
•Supervision is purely from the value networkq(critic).
•Update valuenetwork푞푠,푎;퐰to better estimate thereturn.
•Critic’s judgement becomes moreaccurate.
•Supervision is purely from therewards.
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.

Train thenetworks
Definition: State-value function approximated using neuralnetworks.
Training: Update the parameters 훉and퐰.
1.Observe the state푠
t
.
2.Randomly sample action 푎
t
accordingto휋⋅푠
t 
;훉
t
.
3.Perform 푎
t
and observe new state 푠
t +1
and reward 푟
t
.
4.Update 퐰(in value network) using temporal difference(TD).
5.Update 훉(in policy network) using policygradient.
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.
(裁判更准确）
（运动员分数更高）

•Compute푞푠
t 
,푎
t 
;퐰
t
and푞푠
t+1 
,푎
t+1
;퐰
t
.
•TDtarget:푦
t
= 푟
t
+ 훾⋅푞푠
t+1
,푎
t+1 
;퐰
t
.
Update valuenetwork푞usingTD

2
•Loss:퐿퐰=
1
푞푠
t 
,푎
t 
;퐰−푦
t
2
.
•Gradientdescent:풘
푡+1
=풘
푡
−훼⋅
휕퐿풘
휕풘
|
풘=풘
푡
.
Update valuenetwork푞usingTD
•Compute푞푠
t 
,푎
t 
;퐰
t
and푞푠
t+1 
,푎
t+1
;퐰
t
.
•TDtarget:푦
t
= 푟
t
+ 훾⋅푞푠
t+1
,푎
t+1 
;퐰
t
.

Update policynetwork휋using policygradient
Definition: State-value function approximated using neuralnetworks.
Policy gradient:Derivativeof푉푠
t
;훉,퐰w.r.t.훉.
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.

Update policynetwork휋using policygradient
Definition: State-value function approximated using neuralnetworks.
Policy gradient:Derivativeof푉푠
t
;훉,퐰w.r.t.훉.
Algorithm: Update policy network using stochastic policygradient.
•푉푠;휽,풘=
∑
푎
휋푎푠;휽⋅푞푠,푎;풘.

Environment
State푠
Action푎
Actor-CriticMethod
Policy
Network
(Actor)

Actor-CriticMethod

Actor-Critic Method: UpdateActor

Actor-Critic Method: UpdateCritic
Policy
Network
Environment
(Actor)
Value  
Network  
(Critic)
Reward푟
Value푞
State푠
Action푎

Summary ofAlgorithm

Summary ofAlgorithm

Summary ofAlgorithm

Summary ofAlgorithm

Summary

Policy Network and ValueNetwork

Roles of Actor andCritic
During training
•Agent is controlled by policynetwork(actor):푎
t 
∼ 휋(⋅ |푠
t
;훉).
•Value network 푞(critic) provides the actor withsupervision.

•Agent is controlled by policynetwork(actor):푎
t
∼ 휋(⋅ |푠
t
;훉).
•Value network 푞(critic) will not beused.
Roles of Actor andCritic
During training
Aftertraining
•Agent is controlled by policynetwork(actor):푎
t 
∼ 휋(⋅ |푠
t
;훉).
•Value network 푞(critic) provides the actor withsupervision.

Training
Update the policy network (actor) by policygradient.
•Seek toincreasestate-value:
•Compute policy gradient: 
•Perform gradientascent.

Training
Update the value network (critic) by TDlearning.
•Predicted action-value: 푞
t
=푞푠
t
,푎
t
;퐰.
•TD target: 푦
t
= 푟
t
+ 훾⋅푞푠
t+1
, 푎
t+1
;퐰
•Gradient:
•Perform gradientdescent.
Update the policy network (actor) by policygradient.
•Seek toincreasestate-value:
•Compute policy gradient: 
•Perform gradientascent.

AlphaGo

•What taught in this lecture is not exactly the same to the original  
AlphaGo papers[1,2].
•There are simplificationshere.
Disclaimer
Reference
1.Silver and others: Mastering the game of Go with deep neural networks and tree search. Nature,2016.
2.Silver and others: Mastering the game of Go without human knowledge. Nature,2017.

GoGame
•The standard Go board has a 19×19 grid of  
lines, containing 361points.
•State: arrangement of black, white, andspace.
•State 푠can be a 19×19×2 tensor of 0 or1.
•(AlphaGo actually uses a 19×19×48 tensor to  
store otherinformation.)
•Action: place a stone on a vacantpoint.
•Action space:풜⊂1,2,3,⋯ , 361.
•Go is verycomplex.
•Number of possible sequence ofactions is10
170
.

AlphaGo

High-LevelIdeas

Training andExecution
1.Initialize policy network using behavior cloning.  
(Supervised learning from humanexperience.)
2.Train the policy network using policy gradient.(Two policy  
networks play against eachother.)
3.After training the policy network, use it to train a value  
network.
Training in 3steps:

Training andExecution
Training in 3steps:
1.Initialize policy network using behavior cloning.  
(Supervised learning from humanexperience.)
2.Train the policy network using policy gradient. (Two policy  
networks play against eachother.)
3.After training the policy network, use it to train a value  
network.
•Do Monte Carlo Tree Search (MCTS) using the policy and  
valuenetworks.
Execution (actually play Gogames):

PolicyNetwork

State (of AlphaGoZero)

Conv
feature
Policy Network (of AlphaGoZero)
Dense
state
19×19×17tensor
⋮
휋1 푠,훉
휋2 푠,훉
휋3 푠,훉
휋359 푠,훉
휋360 푠,훉
휋361 푠,훉
Probability distribution  
over the 361actions

Conv
Policy Network (ofAlphaGo)
state
19×19×48tensor
⋮
휋1 푠,훉
휋2 푠,훉
휋3 푠,훉
휋359 푠,훉
휋360 푠,훉
휋361 푠,훉
Probability distribution  
over the 361actions

Initialize Policy Network by BehaviorCloning

203165
93   92  201 151 57  4951 16433
156 154 205 35 202 34 152 50 10 47 43 45 221 220 4 2239
155 157   3   204  85 14915048  44  468 222 231 160161
22987   84 186184196236 232 2355
228  39   31  169 271  86 183 5882   81  224 79  837   268 153199
230 148  36   37   41  168 185 18118218888  195  89   80   68   32  176177
38   40   96   90  167 189 187 264  70  238 251 67  636   192 175197
242 269 270 145 143 146 255 191 227 237 253 65  64178198
194 193 207  95  139 142 144 261 265 254 170 252  69   66  248 78
42  267 99  98226 225 213 209 214 266  71   72   77 76
60   56  240 97  94256 174 136 212 140 260  73  114  75 12
208  52   53  241 91  100190 171 172 257 141 219 115 116  15   16  112 158
59  54  23133 101 102 258 173 262 121 263 119 110  27   14   11   25 113
210 55130 131 103 104 259 217 129 215 218 117  62  120  28   18   26 111
211135   1  132128216 137 206 233 118  74   13  108   2   249 107159
29  13422138 30 179 200 20 105 109 126 1719
61  24  166180106 124 21  127239272
163147162244 243 123122125246247
Learning from human’srecord
•Initially, the network’s parameters are  
random.
•If two policy networks play against each  
other, they would do random actions.
•It would take very long before they make  
reasonable actions.

51 16433
165
196
93 92 201 151 5749
203
155 157 3 204 85 149150
156 154 205 35 202 34 152 50 10 47 43 45 221 220 4 2239
48  44  468 222 231 160161
236 232 2355
82   81  224 79  837 268 153199
230 148  36   37   41  168 185 18118218888  195  89   80   68   32  176177
38   40   96   90  167 189 187 264  70  238 251 67  636   192 175197
242 269 270 145 143 146 255 191 227 237 253 65  64178198
194 193 207  95  139 142 144 261 265 254 170 252  69   66  248 78
42  267 99  98226 225 213 209 214 266  71   72   77 76
60   56  240 97  94256 174 136 212 140 260  73  114  75 12
208  52   53  241 91  100190 171 172 257 141 219 115 116 15 16 112 158
22987 84 186184
228 39 31 169 271 86 18358
59  54  23133 101 102 258 173 262 121 263 119 110  27   14   11   25 113
210 55130 131 103 104 259 217 129 215 218 117  62  120  28   18   26 111
211135   1  132128216 137 206 233 118  74   13  108   2   249 107159
29  13422138 30 179 200 20 105 109 126 1719
61  24  166180106 124 21  127239272
163147162244 243 123122125246247
Learning from human’srecord
•Initially,thenetwork’sparametersare
random.
•Human’sequencesofactionshavebeen
recorded.(KGSdatasethas160Kgames’
recordsform2000.)

51 16433
165
196
93 92 201 151 5749
203
155 157 3 204 85 149150
156 154 205 35 202 34 152 50 10 47 43 45 221 220 4 2239
48  44  468 222 231 160161
236 232 2355
59  54  23133 101 102 258 173 262 121 263 119 110  27   14   11   25 113
210 55130 131 103 104 259 217 129 215 218 117  62  120  28   18   26 111
211135   1  132128216 137 206 233 118  74   13  108   2   249 107159
82   81  224 79  837 268 153199
230 148  36   37   41  168 185 18118218888  195  89   80   68   32  176177
38   40   96   90  167 189 187 264  70  238 251 67  636   192 175197
242 269 270 145 143 146 255 191 227 237 253 65  64178198
194 193 207  95  139 142 144 261 265 254 170 252  69   66  248 78
42  267 99  98226 225 213 209 214 266  71   72   77 76
60   56  240 97  94256 174 136 212 140 260  73  114  75 12
208  52   53  241 91  100190 171 172 257 141 219 115 116 15 16 112 158
22987 84 186184
228 39 31 169 271 86 18358
29  13422138 30 179 200 20 105 109 126 1719
61  24  166180106 124 21  127239272
163147162244 243 123122125246247
Learning from human’srecord
•Initially,thenetwork’sparametersare
random.
•Human’sequencesofactionshavebeen
recorded.(KGSdatasethas160Kgames’
records.)
•Behavior cloning: Let the policy network  
imitate humanplayers.
•After behavior cloning, the policy  
network beats advancedamateur.

BehaviorCloning
•Reinforcement learning: Supervision is from rewards given  
by the environment.
•Imitation learning: Supervision is from experts’actions.
•Agent does not seerewards.
•Agent simply imitates experts’ actions.
Behavior cloning is not reinforcementlearning!

BehaviorCloning
•Reinforcement learning: Supervision is from rewards given  
by the environment.
•Imitation learning: Supervision is from experts’actions.
•Behavior cloning is one of the imitation learningmethods.
•Behavior cloning is simply classification orregression.
Behavior cloning is not reinforcementlearning!

BehaviorCloning
•Observe this state 푠
t
.

BehaviorCloning
•Observe this state 푠
t
.
•Policy network makes aprediction:
퐩
t
=휋1 푠
t
, 훉, ⋯ , 휋361푠
t
,훉∈0,1
361
.
w.p.0.4
w.p.0.1
w.p.0.2
w.p.0.3

BehaviorCloning
•Observe this state 푠
t
.
•Policy network makes aprediction:
퐩
t
=휋1 푠
t
, 훉, ⋯ , 휋361푠
t
,훉∈0,1
361
.
•The expert’s action is 푎
푡
∗
=281.
•Let퐲
t
∈0,1
361
be theone-hot
encode of푎
푡
∗
=281
Expert’saction

BehaviorCloning
•Observe this state 푠
t
.
•Policy network makes aprediction:
퐩
t
=휋1 푠
t
, 훉, ⋯ , 휋361푠
t
,훉∈0,1
361
.
•The expert’s action is 푎
⋆ 
=281.
t
•Let퐲
t
∈0,1
<=0 
be theone-hot
encode of푎
⋆ 
=281.
t
•Loss=CrossEntropy퐲
t
,퐩
t
.
•Use gradient descent to update policy  
network.
Expert’saction

After behaviorcloning...
•Suppose the current sate 푠
t
has appeared in trainingdata.
•The policy network imitates expert’s action 푎
t
. (Which is a goodaction!)

After behaviorcloning...
•Suppose the current sate 푠
t
has appeared in trainingdata.
•The policy network imitates expert’s action 푎
t
. (Which is a goodaction!)
Question: Why bother doing RL after behaviorcloning?
•What if the current state 푠
t
has not appeared in trainingdata?
•Then the policy network’ action 푎
t
can bebad.

After behaviorcloning...
•Suppose the current sate 푠
t 
has appeared in trainingdata.
•The policy network imitates expert’s action 푎
t
. (Which is a goodaction!)
Question: Why bother doing RL after behaviorcloning?
•What if the current state 푠
t
has not appeared in trainingdata?
•Then the policy network’ action 푎
t
can bebad.
•Number of possible states is toobig.
•There is a big chance that 푠
t
has not appeared in trainingdata.
Behavior cloning+RLbeatsbehaviorcloningwith 80%chance.

Train Policy Network Using PolicyGradient

Reinforcement learning of policynetwork
Player
(Agent)
policy networkwith  
latestparam
Opponent
(Environment)
policy networkwith  
oldparam
V.S.
•Player’s parameters are the latest parameters of the policynetwork.
•Opponent’s parameters are randomly selected from previousiterations.

Reinforcement learning of policynetwork
•Suppose a game ends at step푇.
•Rewards:
•푟
1
= 푟
2
= 푟
3
= ⋯ = 푟
T-1
= 0. (When the game has notended.)
•푟
T
= +1(winner).
•푟
T
= −1(loser).
Reinforcement learning is guided byrewards.

Reinforcement learning of policynetwork
•Suppose a game ends at step푇.
•Rewards:
•푟
1
= 푟
2
= 푟
3
= ⋯ = 푟
T-1
= 0. (When the game has notended.)
•푟
T
= +1(winner).
•푟
T
= −1(loser).
•Recall that return is defined by 푢
푡
=
∑
푖=푡
푇
푟
푖
. (No discounthere.)
•Winner’sreturns:푢
1
= 푢
2
= ⋯ = 푢
T
=+1.
•Loser’sreturns:
푢
1
= 푢
2
= ⋯ = 푢
T
=−1.
Reinforcement learning is guided byrewards.

PolicyGradient
•Recall that 
휕푙표푔휋(푎
푡
|푠
푡
,휃)
휕휃
∗푄
휋
(푠
푡
,푎
푡
)
is approximate policy gradient.
Policy gradient: Derivative of state-valuefunction푉푠;훉w.r.t.훉.

PolicyGradient
Policy gradient: Derivative of state-valuefunction푉푠;훉w.r.t.훉.
•Recall that 
휕푙표푔휋(푎
푡
|푠
푡
,휃)
휕휃
∗푄
휋
(푠
푡
,푎
푡
)
is approximate policy gradient.
•By definition, the action value is 푄
휋
푠
푡
,푎
푡
=피푈
푡
푠
푡
,푎
푡
.
•Thus, we can replace 푄
휋
푠
푡
,푎
푡
by the observed return 푢
푡
.
•Approximate policy gradient: 
휕log휋(푎
푡
|푠
푡
,휃)
휕휃
∗푢
푡

Train policy network using policygradient
•Two policy networks play a game to the end. (Player v.s.Opponent.)
•Getatrajectory:푠
1
, 푎
1
,푠
2
,푎
2
,⋯, 푠
T
,푎
T
.
•After the game ends, update the player’s policynetwork.
•The player’s returns: 푢
1
= 푢
2
= ⋯ = 푢
T
. (Either +1 or−1.)
•Sum of approximatepolicygradients:헀
휃
=
∑
푡=1
푇
휕log휋(푎
푡
|푠
푡
,휃)
휕휃
∗푢
푡
•Updatepolicynetwork:훉←훉+ 훽⋅헀
휃
.
Repeat thefollowings:

Play Go using the policynetwork
•The policy network 휋has beenlearned.
•Observing the current state 푠
t 
, randomly sampleaction
푎
t
∼휋⋅푠
t
,훉.

Play Go using the policynetwork
•The policy network 휋has beenlearned.
•Observing the current state 푠
t 
, randomly sampleaction
푎
t
∼휋⋅푠
t
,훉.
•The learned policy network 휋is strong, but not strongenough.
•A small mistake may change the gameresult.

Train the ValueNetwork

Definition: State-valuefunction.
•푉
휋
푠= 피푈
t 
|푆
t
=푠, where 푈
t
= +1 (if win) and −1 (if lose).
•The expectation is taken with respectto
•Thefutureactions퐴
t
,퐴
t+1
,⋯,퐴
T
.
•Thefuturestates푆
t+1
,푆
t+2
,⋯,푆
T
State-ValueFunction

State-ValueFunction
Approximate state-value function using a valuenetwork.
•Use a neuralnetwork푣푠;퐰toapproximate푉
휋
푠.
•It evaluate how good the current situationis.
Definition: State-valuefunction.
•푉
휋
푠= 피푈
t
|푆
t
=푠, where 푈
t
= +1 (if win) and −1 (if lose).
•The expectation is taken with respectto
•Thefutureactions퐴
t
,퐴
t+1
,⋯,퐴
T
.
•Thefuturestates푆
t+1
,푆
t+2
,⋯,푆
T

Conv
feature
Policy Value Networks (AlphaGoZero)
Policy  
Head
state
19×19×17tensor
⋮
휋(361|푠,휃)
Value  
Head
State value(scalar)
휋(1|푠,휃)

Train the valuenetwork
After finishing training the policy network, train the valuenetwork.
Not Actor-Critic!

1.Play a game to theend.
•If win, let 푢
1
= 푢
2
= ⋯ = 푢
T
= +1.
•If lose, let 푢
1
= 푢
2
= ⋯ = 푢
T
= −1.
2.Loss:퐿=
∑
푡=1
푇
1
2
푣푠
푡
;푤−푢
푡
2
3.Update: 푤←푤−훼∗
휕퐿
휕푤
Train the valuenetwork
After finishing training the policy network, train the valuenetwork.
Repeat thefollowings:

Monte Carlo TreeSearch

How do human playGo?
•Suppose I choose action 푎
t
.
•What will be my opponent’s action? (His action leads to state푠
t+1
.)
•What will I be my action 푎
t+1
upon observing푠
t+1
?
•What will be my opponent’s action? (His action leads to state푠
t+2
.)
•What will I be my action 푎
t+2
upon observing푠
t+3
?
⋮
•If you can exhaustively foresee all the possibilities, you willwin.
Players must look ahead two or moresteps.

•Strange: I went forward in time... to view alternate futures. To see all the possible outcomes of the comingconflict.
•Quill: How many did yousee?
•Strange: Fourteen million six hundred andfive.
•Stark: How many did wewin?
•Strange: ...One.

p
n
r
u
8
Select actions by look-aheadsearch
•Randomly select an action푎.
•Look ahead and see whether 푎leads to win or  
lose.
•Repeat this procedure manytimes.
•Choose the action 푎that has the highestscore.
player’saction
opponent’saction
Mainidea

Monte Carlo Tree Search(MCTS)
1.Selection: The player makes an action 푎. (Imaginary action; not  
actual move.)
2.Expansion: The opponent makes an action; the state updates. (Also  
imaginary action; made by the policynetwork.)
3.Evaluation: Evaluate the state-value and get score 푣. Play thegame
to the end to receive reward 푟. Assign score 
푣+푟
2
to action푎.
4.Backup: Use the score 
푣+푟
2
to updateaction-values.
Every simulation of Monte Carlo Tree Search (MCTS) has 4steps:

푠
t
Step 1:Selection
Question: Observing 푠
t
, which action shall weexplore?
validactions

푠
t
Step 1:Selection
Question: Observing 푠
t
, which action shall weexplore?
•푄푎:Action-value computed by MCTS. (To bedefined.)
•휋푎| 푠
t
;훉:The learned policynetwork.
•푁푎: Given 푠
t
, how many times we have selected 푎sofar.
First, for all the valid actions 푎, calculate thescore:
validactions
푠푐표푟푒푎=푄푎+휂∗
휋(푎|푠
푡
;휃)
1+푁(푎)
푄(푎) isinitialized as 0

푠
t
Step 1:Selection
Question: Observing 푠
8 
, which action shall weexplore?
0.6
0.3
0.8
•푄푎:Action-value computed by MCTS. (To bedefined.)
•휋푎| 푠
t 
;훉:The learned policynetwork.
•푁푎: Given 푠
t 
, how many times we have selected 푎sofar.
First, for all the valid actions 푎, calculate thescore:
푠푐표푟푒푎=푄푎+휂∗
휋(푎|푠
푡
;휃)
1+푁(푎)

•푄푎:Action-value computed by MCTS. (To bedefined.)
•휋푎| 푠
t 
;훉:The learned policynetwork.
•푁푎: Given 푠
t 
, how many times we have selected 푎sofar.
Step 1:Selection
Question: Observing 푠
8 
, which action shall weexplore?
Second, the action with thebiggestscore푎is selected.
푠
t
0.60.8
0.3
First, for all the valid actions 푎, calculate thescore:
푠푐표푟푒푎=푄푎+휂∗
휋(푎|푠
푡
;휃)
1+푁(푎)

•Given 푎
t
, the opponent’s action 푎
푡
′
will lead tothe
new state푠
t+1
.
푠
t
푎
t
Step 2:Expansion
Question: What will be the opponent'saction?

•Given푎
푡
, the opponent’s action 푎
푡
′
will lead tothe
new state푠
t+1
.
•The opponent’s action is randomly sampledfrom
Here, is the state observed by theopponent.
푠
t
Step 2:Expansion
Question: What will be the opponent'saction?
Prob =0.4
푎
t
Prob =0.6
푎
푡
′
∼휋(∗|푠
푡
′
;휃)

푠
t
Step 2:Expansion
Question: What will be the opponent'saction?
Prob =0.4
푎
t
Prob =0.6
푠
t+1
•The state-transitionprobability푝푠
t+1
푠
t
,푎
t
isunknown.
•Use the policy function 휋as the state-transition function푝.
•Given푎
푡
, the opponent’s action 푎
푡
′
will lead tothe
new state푠
t+1
.
•The opponent’s action is randomly sampledfrom
Here, is the state observed by theopponent.
푎
푡
′
∼휋(∗|푠
푡
′
;휃)

opponent’saction
•Player’saction: 푎
푘
∼휋(∗|푠
푘
;휃)
•Opponent’saction:푎
푘
′
∼휋(∗|푠
푘
′
;휃)
푠
t
푎
t
Step 3:Evaluation
푠
t+1
⋮
푠
T
푠
t+1
Fast  
Rollout
player’saction
푎
t+1
opponent’saction
player’saction
Run a rollout to the end of the game (step푇).

•Player’saction:푎
k
∼휋*| 푠
k
;훉.
•Opponent’saction:푎
푘
′
∼휋(∗|푠
푘
′
;휃)
•Receive reward 푟
T
at theend.
•Win:푟
T
=+1.
•Lose:푟
T
=−1.
Step 3:Evaluation
Run a rollout to the end of the game (step푇).
opponent’saction
푠
t
푎
t
푠
t+1
⋮
푠
T
푠
t+1
Fast  
Rollout
player’saction
푎
t+1
opponent’saction
player’saction

푠
8
푎
8
Step 3:Evaluation
푠
t+1
Run a rollout to the end of the game (step푇).
•푣푠
t+1
;퐰:output of the valuenetwork.
Evaluate the state푠
t+1
.
•Player’saction:푎
k
∼휋*| 푠
k
;훉.
•Opponent’saction:푎
푘
′
∼휋(∗|푠
푘
′
;휃)
•Receive reward 푟
T
at theend.
•Win:푟
T
=+1.
•Lose:푟
T
=−1.

푠
t
푎
t
Step 3:Evaluation
푠
t+1
Record푉푠
t+1
.
Run a rollout to the end of the game (step푇).
푉푠
푡+1
=
1
2
푣푠
푡+1
;푤+
1
2
푟
푇
Evaluate the state푠
t+1
.
•푣푠
t+1
;퐰:output of the valuenetwork.
•Player’saction:푎
k
∼휋*| 푠
k
;훉.
•Opponent’saction:푎
푘
′
∼휋(∗|푠
푘
′
;휃)
•Receive reward 푟
T
at theend.
•Win:푟
T
=+1.
•Lose:푟
T
=−1.

푠
t
푎
t
Step 4:Backup
Records:
•푉
1
(1)
,
•푉
2
(1)
,
•푉
3
(1)
,
•푉
4
(1)
Records:
•푉
1
(2)
,
•푉
2
(2)
,
•푉
3
(2)
,
•푉
4
(2)
•MCTS repeats such a simulation manytimes.
•Each child of 푎
t
has multiplerecorded푉푠
t+1
.

Step 4:Backup
•MCTS repeats such a simulation manytimes.
•Each child of 푎
t
has multiplerecorded푉푠
t+1
.
•Update action-value:
푄푎
t
=meantherecorded푉
′
푠.
•The 푄values will be used in Step 1(selection).
푠
t
푎
t
Records:
•푉
1
(1)
,
•푉
2
(1)
,
•푉
3
(1)
,
•푉
4
(1)
Records:
•푉
1
(2)
,
•푉
2
(2)
,
•푉
3
(2)
,
•푉
4
(2)

Step 4:Backup
•MCTS repeats such a simulation manytimes.
•Each child of 푎
t
has multiplerecorded푉푠
t+1
.
•Update action-value:
푄푎
t
=meantherecorded푉
′
푠.
•The 푄values will be used in Step 1(selection).
푠
t
푄푎
1
푄푎
2
푄푎
3

푠
t
푄푎
1
푄푎
2
푄푎
3
Revisit Step 1Selection
First, for all the valid actions 푎, calculate thescore:
푠푐표푟푒푎=푄푎+휂∗
휋(푎|푠
푡
;휃)
1+푁(푎)
Second, the action with thebiggestscore푎is selected.

Decision Making afterMCTS
•푁푎: How many times 푎has been selected sofar.
•After MCTS, the player makes actualdecision:
푎
t
=argmax푁푎
푎
푠
t
푄푎
1
푄푎
2
푄푎
3

•MCTS has 4 steps: selection, expansion, evaluation, andbackup.
•To perform one action, AlphaGo repeats the 4 steps for many  
times tocalculate푄푎and푁푎(for every action푎.)
•AlphaGo executes the action 푎with thehighest푁푎value.
•To perform the next action, AlphaGo do MCTS all over again. 
(Initialize푄푎and푁푎tozeros.)
MCTS:Summary

Summary

1.Train a policy network using behaviorcloning.
2.Train the policy network using policy gradientalgorithm.
3.Train a valuenetwork.
Training andExecution
Training in 3steps:
（Regression）
（Classification）
（RL）

1.Train a policy network using behaviorcloning.
2.Train the policy network using policy gradientalgorithm.
3.Train a valuenetwork.
Training andExecution
Training in 3steps:
•Select the “best” action by Monte Carlo TreeSearch.
•To perform one action, AlphaGo repeats simulations many  
times.
Execution (actually play Gogames):

AlphaGoZero

•AlphaGo Zero is stronger than AlphaGo. (100-0 againstAlphaGo.)
•Differences:
•AlphaGo Zero does not use human experience. (No behaviorcloning.)
•MCTS is used to train the policy network.(MCTS or Expert as supervision)
AlphaGo Zero v.s.AlphaGo

•AlphaGo Zero does not use human experience. (No behaviorcloning.)
•For the Go game, human experience isharmful.
•In general, is behavior cloninguseless?
•What if a surgery robot (randomly initialized) is learned purely by  
performing surgery? (Human experience is notused.)
Is behavior cloninguseless?

1.Observe state 푠
t 
.
2.Predictions made by policynetwork:
퐩=휋푎= 1  푠
t
;훉, ⋯,휋푎= 361 푠
t
;훉
∈ℝ
361
.
Training of policynetwork
AlphaGo Zero uses MCTS in training. (AlphaGo doesnot.)

1.Observe state 푠
t
.
2.Predictions made by policynetwork:
퐩=휋푎= 1  푠
t
;훉, ⋯,휋푎= 361 푠
t
;훉
∈ℝ
361
.
3.Predictions made byMCTS:
퐧=normalize푁푎=1,푁푎=2, ⋯,푁푎=361
∈ℝ
361
.
4.Loss:퐿=CrossEntropy퐧,퐩.
5.Use
휕퐿
휕휃
to update훉.
Training of policynetwork
AlphaGo Zero uses MCTS in training. (AlphaGo doesnot.)

Sarsa

Derive TD Target

Discounted Return
•푈
t
= 푅
t
+ 훾⋅ 푅
t +1
+ 훾
2
⋅ 푅
t+2
+ 훾
3
⋅ 푅
t+3
+ 훾
4
⋅ 푅
t+4
+⋯
= 푅
t
+훾⋅(푅
t +1
+ 훾⋅ 푅
t +2
+ 훾
2
⋅ 푅
t +3
+ 훾
3
⋅ 푅
t +4
+⋯)
=푈
t +1
Definition of discounted return:
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.

Discounted Return
Identity:푈
t
= 푅
t
+ 훾⋅푈
t +1
.
⚫Assume  푅
t
depend on (푆
t, 
퐴
t, 
푆
t +1
).

Derive TD Target

Derive TD Target

Derive TD Target

Sarsa：Tabular Version 
表格形式（有限状态与动作）

Tabular Version

Sarsa(Tabular Version)

Sarsa(Tabular Version)

Sarsa’sName

Sarsa：Neural Network Version
网络形式（无限状态与动作）

Value Network Version

Value Network Version

TD Error & Gradient

Summary

Q-Learning

SarsaVS Q-Learning

Derive TD Target

Derive TD Target

Derive TD Target

Derive TD Target

Derive TD Target

Q-Learning：Tabular Version 
表格形式（有限状态与动作）

Q-Learning(Tabular Version)

Q-Learning(Tabular Version)

Q-Learning：DQN Version 
DQN形式（无限状态与动作）

Q-Learning(DQN Version)

Q-Learning(DQN Version)

Q-Learning(DQN Version)

Summary

•AlphaGo:
•Silver and others: Mastering the game of Go with deep neural networks and  
tree search. Nature,2016.
•AlphaGoZero:
•Silver and others: Mastering the game of Go without humanknowledge.
Nature,2017.
Reference

ThankYou!
Most Slides are based on those of Dr. ShusenWang and Prof. David Silver and Sergey Levine’s course, Thanks for the support.
