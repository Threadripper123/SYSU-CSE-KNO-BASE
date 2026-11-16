# Lecture4-高级搜索-图集

> 主题库：人工智能　｜　源文件：Lecture4-高级搜索-图集.pdf　｜　3,389 字符　｜　16 个片段　｜　更新于 2026-11-11
ArtificialIntelligence [人工智能 ChaoYu(余超） School of Computer Science and Engineering [Tabl _l__

父个体1 子个体1 编码父个体2 子个体2  解码父个体3 子个体3   父个体4 于个体4 遗传域(基因型) 选择交叉变异 3 下一代香足展终代 B5

10 2 3 9 4 75 6

初帕种群 0001100000 010111101 0000000101 1001110100 1010101010 1110010110 1001011011 1100000001 1001110100 0001010011 (12) (5) (i9) (i0) (i4) 选生 0001·100000 111 0010110 1100000001 1001110100 1010101 010 1110-010110 100-1011011 100111·0100 110000001 0001010011 1 0001010110 1111011011 1100000100 1001110100 1010101011 1110100000 1000010110 10111001 1100000001 0001010010 变异

世代低个体搜索间

产生初 → 计足否满足 Yes_ 佳个体始种群适应度优化准则 NO 开帕选择结果交叉变异

80 60. 1 40- 部 20, 2 5 5 55

0.6 0.4 0.2 -0.2 -1 -1 -0.8 -0.6 -0.4 -0.2 0 0.2 0.4 0.6 0.8

caenode,bestneighbor:Node 1) 随机生成 cur node 的 sol, evaluate(cur node)+ 2)for =1to∞ o 3) → T = update(t) 4) →if T = 0 then retun cur node 5)→successors ← neighbor builder() 6）→从successors中随机选择一个节点， 记它为next_node T) → ∆ E← next node.value- curnode.value 8) →if ∆E 0 then cur_node ← next_node 9)→else 以概率 e-E/T执行cur_node← next_nodet Input: n: Node Output:n对应的路径代价 int distance← 0; 1) for i = 2 to k dlo 2) → distance ← distance + Dist(n.sol[i-1], n.sol[i]) a) distance ← distance + Dist n.sol[k ,n.sol[1 4) retun distance;

Output: a set of Node that is the neighbor of n. neighbors={} neighbor:Node for =2tolo 2 →forj=3todo 3)→ → 1 neighbor← n 4)+→ 交换neighbor:sol中的第i个城市和第j个城市的位置 5) → → ! neighbors ← neighborsU {neighbor}← 61 )ret nneighbors Yaunput T, an integer that represents temperature if t = 0 then →T个to elseT← λ

#设待解决的TSP问题有k个城市。 1 population← { } #生成含有10个不相同个体的初始种群 2)i←0 4)→随机生成一个长度为k的字符串，字符取自{0,..,k-1}且不重复， 记此字符串为individualt 5)→ if population 不含有 individual thent 6) → → population← populationU { individual }← 7)→ + i←i+1t 8) wlhile (i>= 9) 9) num iteration= 0; 11) newpopulation← { } 12)-for i = 1 to SIZE(population) do 13) → parent1 ← random select(population, evaluate())+ 14)→ → parent2 ← random select(population, evaluate))+ 15) → child← reproduce(parent l,parent2) 16)+→ 在[0，1]内生成一个随机数r 17)→ → if r ≤ β then 18)+→→ mutate(child) 19)+ → new_population ← new_population U { child } 20)-population← new population [Y num_iteration t 22) u til num iteration ≥ 100000t

population，种群集 evaluate，评估个体的适应度函数，见4.3.1的解答 Output：individual， 被选择的个体。 sum←0 fori=1toSIZE(population do → evaluate(population/iD → sum ← sum + population/il.value fori=1toSIZE(population do → 生成一个[0,1]内的随机数r → i if (r < (population[i].value/sum)) then → → individual ← population[i]← → → retun individual← function reproduce Input:x, y， 两个个体 Output：x和y交叉得到的下一代个体z n ← LENGTH(x); c ← random number from 1 to n ret rn APPEND(SUBSTRING(x, 1, c),SUBSTRING(, c+1, n)

18 1 14 13 13 12 14 14 Q 14 16 13 15 12 14 12 16 Q 1 12 18 13 15 12 4 1 Q 15 14 1 16 1 6 Q 14 17 15 Q 14 16 16 Q 17 Q 16 18 15 Q 15 Q Q 18 14 Q 15 15 14 Q 16 Q 14 14 13 7 12 14 12 18 Q

个体染色体适应度选择概率累积概率 1 0001100000 8 0.086 957 0.086 957 2 0101111001 5 0.054 348 0.141 304 3 0000000101 2 0.021739 0.163043 4 1001110100 10 0.108 696 0.271 739 5 1010101010 7 0.076087 0.347 826 6 1110010110 12 0.130 435 0.478 261 7 1001011011 5 0.054 348 0.532 609 8 1100000001 19 0.206 522 0.739 130 9 1001110100 10 0.108 696 0.847  826 10 0001010011 14 0.152 174 1.000 000

f (x1, x2) = 20 + x2 + x2 2 − 10(c0s2 πx, + c0s 2 πx2) −5 ≤ x, ≤ 5 i = 1, 2 初始种群第二代种群 -0.2 -0.4

-0.8 -1 -0.8 -0.6 -0.4 -0.2 0 0.2 0.4 0.6 0.8 -1 -0.8 -0.6 -0.4 -0.2 0 0.2 0 1
