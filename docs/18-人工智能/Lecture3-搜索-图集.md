# Lecture3-搜索-图集

> 主题库：人工智能　｜　源文件：Lecture3-搜索-图集.pdf　｜　6,625 字符　｜　51 个片段　｜　更新于 2026-11-11
ArtificialIntelligence [人工智能 ChaoYu(余超） School of Computer Science and Engineering [Tabl _l__

中国象棋-一-作者： 善文回区 (C)棋谱 (S)设置 Q)查看 H)帮助 1一：进車馬象士帥士象馬車炮兵兵兵兵兵第· 馬· 湘· 陆 ·將陆· 湘· 炮車共访问82091个节点,花费时间0.797s  电脑停止悔棋还原

围棋区开始S）退出X）

新始 123456789ABCDEF

3.2盲目搜索 2 3 4 8 5 65 6 7 8 9 10 11 12 13 [a 8 8 8 6 14 15 16 17 18 19 20 21 a 8 8 8 6 6 5     5  22 23 24 25 GG 26 8 3 8 3 2 8 3 2 8 3 2 3 3 2 1 4 2 4 7 4 7 4 7 8 4 8 7 6 5 7 6 5 6 5 6 5 6 5 7 6 5

深度p 节点数p 时间空间 0 1 1μS 100B 2 111 1 s 11 KB 4 11111 11 s 1 MB 6 106 18min 111MB 8 108 31h 11 GB 10 1010 128d 1TB 12 1012 35y 111TB 14 1014 3500y 11111 TB

标准。 宽度优先+ 深度优先+ 有界深度+ 选代加深p 时间ρ bd b bd 空间ρ b b bl bd 最优是。 否。 否。 是。 完备是否。 如果1>d，是是。

Procedure Graph-Search Begin 建立只含初始结点S的搜索图G计算f（S）：将S放人OPEN表：将CLOSED表初始化为空 Whi1eOPEN表不空D。 Begin 从OPEN表中取出E（n）值最小的结点，将n从OPEN表中删除并放人CLOSED表 Ifn是目标结点Then根据n的父指针指出从s。到n的路径，算法停止 Else Begin 扩展结点 I结点n有子结点 Then Begin （1)生成的子结点集合{m把m作为n的子结点加入到G中，并计算r（m） （2）Im未曾在OPEN和CLOSED表中出现，Then将它们配上刚计算过的值，将m的父指针指向n并把它们放人OPEN表 （3)Ifm已经在OPEN表中，Then该结点一定有多个父结点，在这种情况下， 比较m相对于n的值和m相对于其原父指针指向的结点的值，若前者不小于后者，则不做任何更改，否则将m的值更改为m相对于n的值，m的父指针更改为 （4）Im已经在CLOSE表中，Then该结点同样也有多个父结点。在这种情况下，比较m相对于n的值和m相对于其源父指针指向的结点的r值。 如果前者不小于后者，则不作任何更改，否则将m从CLOSED表移到OPEN 表置m的父指针指向 （5）按值从小到大的次序对OPEN表中的结点重新排序 End End End End

S0 6 2 4 5

SO 6 4  5

7  5 2 8 3 2 8 3 2 8 3 1+5 1 6 4 1+3 1 4 1+5 1 6 4  7 5 7 6 5 7  5  2 8 3 2  3 2 8 3 2+3 2+3  1 4 1 8 4 2+4 1 4  7 6 5 7 6 5 7 6 5  8 3 2 8 3  2 3 2 3  2 1 4 7 1 4 1 8 4 1 8 4 7 6 5 6 5 7 6 5 7 6 5 3+3 3+4 3+2 3+4  8 4 4+1 7 _6 5 5+0 8  4 5+2 7 8 4 7 6 5  6 5 目标结点t

Straight-line distance radea 71 toBucharest Neamt Arad 366 □ 口 zerind 87 Bucharest 0 75/ 151 Craiova 160 口1 |lasi Dobreta 242 Arad□ 140 92 Eforie 161 Fagaras Sibiu 178 99 Fagaras Giurgiu 118 77 □ 80 Vaslui Hirsova 151 Iasi 226 Timisoara RimnicuVilcea Lugoj  □ 244 142 Mehadia 241 1 ↑ 211 □ Lugoi 97 Pitesti Neamt 234  Oradea 70 □ 380 98 146 85_ Pitesti 98 □ □ Hirsova □ Mehadia 10个 Urziceni Rinnicu Vilcea 193 75 138 86 Sibiu 253 Bucharest Timisoara 329 Dobreta □ 120 /90 □ Urziceni 80 □ Craiova Eforie Vaslui 199 □ Giurgiu Zerind 374

(a)The initial state  Arad 366=0+366 (b) After expanding Arad Arad Y Sibiu Timiso ara Zerind 393=140+253 447=118+329 449=75+374 (c) After expanding Sibiu Arad Sibiu Timisoara Zerind 447=118+329 449=75+374 caArad Fagaras Oradea I Rmmicu Vileca 646=280+366 415=239+176 671=291+380 413=220+193 (d) After expanding Rimnicu Vilcea Arad Sibiu Timisoara Zerind 447=118+329 449=75+374 Arad Fagaras Oradea Rumriz Vileca 646=280+366 415=239+176 671=291+380 Craiova Pitesti Sibiu 526=366+160 417=317+100 553=300+253 Sibiu Timisoara Zerind 447=118+329 449=75+374 Arad Fagaras Oradea Rimtiza Vike 646=280+366 671=291+380 Sibiu Bucharest Craiova Pitesti Sibiu 591=338+253 450=450+0 526=366+160 417=317+100553=300+253 ( After expanding Pitesti Arad cSibiu Timisoara Zerind 447=118+329 449=75+374 Arad Fagaras Oradea Rimmcu Vilcea 646=280+366 671=291+380  Sibiu Bucharest Craiova Pitesti Sibiu 591=338+253 450=450+0 526=366+160 553=300+253 Bucharest Craiova Rmrica Vilaca 418=418+0 615=455+160 607=414+193

Depth IDs A*(Misplaced) 1 A*(Manhattan) 2 [24 39,15 1, 41

Procedure IDA Begin 初始化当前的深度限制c=1； 把初始结点压入栈；并假定c=OC； While栈不空Do Begin 弹出栈顶元素n Ifn=goal，Then结束，返回n以及从初始结点到n的路径 Else Begin Eorn的每个子结点nDo Begin Iff（n） c,Then把n压人栈 Elsec=mln(c,f(n'')) End End End I栈为空并且c=oc,rhen停止并退出； If栈为空井且c≠c，mhenc=c，并返回2 End

2 -1 2□ 6□ □0 □-1 □-1 20 -10 -10 60 -10 00 -50 0-2 0-10-7 0-1 29 51 -1-1 3 68 -1 2 0 -574 -26.-1 -7 -14 3 图318四层博树

(O) (3) (4) * * * * * 0 0 (2) 0 (1) (0) (1) (0) (1) (1) (2) 0 10 0 (-1) (0) (-1) (0) (-2)

function MiNIMAx-DEcisioN(state)returns an action ← MAXVALUE(state) return the action in SucCEssORs(state) with value v function MAx-VALuE(state)returns a utility value ifTERMINAL-TEsT(state)thenreturnUTLIrY(state) fora,sinSuCCEssORs(state)do ← MA NVALUE( returnw function MIN- VALUE(state) returns a utility value ifTERMINAL-TEST(state)then returnUTLrrY(state) fora,sinSuCCEssORs(state)do U← MIN(,MAX-VALUE(s))

中国象棋盘棋平均 50步，总状态数约为10160 假设每秒搜索104个节点，约需10145年结论：不能搜索整个状态空间

MAX △≥3 MIN 2

MAX ≥3 MIN w 2 12 8 2

MAX △≥3 MIN 3  7≤2 ≤14 12 8 2 14

MAX 3 MIN 2 5 8 2

MAX X MIN 2 ×x2 X X 8 2 14

3 05-333-3022-30-23541-30689-3

b n 0 3 h11 k 6 a -3 3 -3 -3 - -291-3 j 6 -3 05-333-3022-30-23541-30689-3

这 0 3 OMINnodes X

s_ a≥-1 X A B= ≤-1 0 0← (1) (0) (1) (0) (-1) C (-1)

A3+ A6+ A2 1 ≠ ≠ ≠ ≠ AS A1+ A4 ≠

Selection Expansion Sim ulation Backpropagation Tree Default Policy  Policy

洞穴探宝地图地狱(hell) 强盗 (robbers) 喷泉(foutain) 美人鱼入口 (entry) (mermaid) 出口宝藏(gold) 食物(food) (exit) 妖怪

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 A C J 1 23

1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 A 日 c 2 16 G 18 2 8 4 H 6 5 1 20 14 3 2 」 15 （30 27 10 3 31 29 25 23 33 游戏结東区 24 28 i 黑棋获胜! 26 硝定 0 Black Human a exe 浏开始悔保存造载入棋错 white Local Ai aiexe

1.e4c62 d4d5 3.Nc3 dxe4

Google DeepMind OAlphaGo challenge match 8-15March2016 02000 392 e AlphaGo Lee Sedol

2 8 3 2 8 3 ① 2 3 1 6 4  1 4  1 8 4 7 5 7 5 7 6 6 5 f=4 f=3 f=3 1 2 3 1 2 3 2 3 8 4 8 4 1 8 4  4  7 6 5 7 6 5 7 6 5

2 8 3 ② 2 8 3 8 3 8 3 1 4  1 4  2 1 4  2 1 4 7 6 5 7 6 5 7 6 5 7 6 5 f=3 f=3 f=3 f=3 1 3 1 3 8 1 3 8 1 3 8 2 4 8 2 4 4 2 4 4 2 4  1 7 6 5 7 - 6 5 7 5 7 5  6

1

Q 1,1))

Q 1,1))

1,1) 1) (2,3)) ((1, 1) (2,4))

1,1)) ((1,2)) 1) (2,3)) ((1,1) (2,4)  ((1,2)(2,4))

1,1)) ((1,2)) 1)(2,3)) ((1,1)(2,4))  ((1,2) (2,4)) , 1) (2,4) (3 .2)) ((1,2) (2,4) (3,1))

深度 1 3 4 5 7 8 2 ● 10 11 12

3 8  4 2 2 2 4 8 4 1 8 4 6 4 4 1 4 8 8 6 2 8 8 3 2 8 3 2 8 2 8 3 2 3 L 2 1 4 7 1 4 4 1 4 5 7 8 4 8 4  6 5 6 6 6 6 5 6 目标

棵树的过程（下图） 深度 1 2 3 8 八 0 7 11 12

1 8 4 8 3 2 3 2 2  4 1 8 4 1 8 4 2 7 6 5 7 6 5 8 3 3 8  8 4 6  4 1 4 7 7 6 5 6  - 4 8 a b 2 8 2 8 3 2 8 3 2 8 2 8  3 12  4 1 6 4 2 1 4 7  1 4  4 3 1 4 5 7 8 4 8 4 5 7 6 5 6 5 6 5   6 6 5 2 8 3 2 8 8 3 2 8 2 2 8 2 8 3 4 2  4 7  4 4 3 1 4 5 目标 4  6 5 6

2 8 3 1 4 7 5 2 8 3 2 8 3  3   1+5 1 6 4 1+3 1  8 6 4 7 5 7 6 1+5 7  5 2 8 3 2 3 2 8 3 1 4 2+3 2+3 1 8 4 1 4 2+4 7 6 5 7 6 5 7 6 5 8 3 2 8 3   2 3   2 1 4 3+4 7 1 4 3+2    1 8 4 3+3 3+4 7 6 5 6 5    7 6 5

第3步： (0,0) g+h=0+10 (0,3) M N (4,0) 1+8 /  1+8 水壶问题的状态 1+8 1 空间扩展图 P (3,0) R (4,3) 2+4 2+10 S 3+4  在第3步， 可以得到节点S， 其中 3+4=7。3 现在，在节点S、  R、N中，  节点S的估价函数值最小， 所以下一步就会选择S节点扩展。 该过程一直进行下去， 直到到达目标节点

(6,1,MAX) (5,2,MAX) (4,3,MAX) (5, 1, 1, MIN) (4, 2, 1, MIN)  (3, 2, 2, MIN) (3, 3, 1, M IN) √  (4, 1, 1, 1, MAX) (3, 2, 1, 1, MAX) (2, 2, 2, 1, MAX) (3, 1, 1, 1, 1, MIN) (2, 2, 1, 1, 1, MIN)

(7，MIN) 的博弈 (6,1,MAX) (5,2,MAX) (4,3,MAX) (5, 1, 1, MIN) (4, 2, 1, MIN) ( 3, 2, 2, MIN) (3, 3, 1, MIN) √  (4, 1, 1, 1, MAX) (3, 2, 1, 1, MAX) (2, 2, 2, 1, MAX) (3, 1, 1, 1, 1, MIN) (2, 2, 1, 1, 1, MIN) (2, 1, 1, 1, 1, 1, MAX)
