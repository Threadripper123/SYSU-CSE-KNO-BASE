# 11-2-Search2-图集

> 主题库：数据结构与算法　｜　源文件：11-2-Search2-图集.pdf　｜　1,929 字符　｜　14 个片段　｜　更新于 2026-11-11
YAHOPO! or

y1 个 y=f(x) (x1) →x x1 x2

y 个 y=f(x f(x1) f(x) 1 2 X

{ K , K 1, . . ., KN−1} possible keys hashfunctionh T[0,...,m −1]

17 #include<asm/types.h> 18 #include <linux/compiler.h> 20 /* 2^31 + 2^29- 2^25 + 2^22 - 2^19 - 2^16 + 1 */ 21 #defineG0LDEN_RATI0PRIME_320x9e370001UL

58 staticinline u32 hash 32(u32val,unsignedintbits) 6e /* On some cpus multiplyis faster, on others gcc willdoshifts */ 61 u32 hash =Val *GOLDEN_RATIO_PRIME_32; 63 /*High bits are more random,so use them.*/ 64 returnhash>>(32 -bits);

Knuthrecommends primes inapproximately golden ratio tothe maximum integer representable by a machine word for multiplicative hashing.Chuck Lever verified the effectivenessofthistechnique http://www.citi.umich.edu/techreports/reports/citi-tr-oo-1.pdf Theseprimes are chosen tobebit-sparse,thatis operations on them can use shifts and additions instead ofmultiplications for machines where multiplications are slow.

= = 0, 0.= =( =0.2 0.9 = 0, 2  =1.0 =5.0 0.8 =-2,σ2= 0,σ2 = = 0.5 0.7 0.6 0.5 0.4 0.3 0.2 0.1 0 -5 -4 -3 -2 -1 0 2 3 4

酒表86构造的哈希表哈希地址 0 关键字 1 2 3 4 5 6 7 8 14 01 9 比较次数 1 23 84 27 55 20      1 112341 2

平均查找长度： ASLuec=(1+1+1+2+   3 5  3+4+1+2)/8。冲实的前提下警超中春套字量以关键字27为例：H（27）=27MOD7= 6（冲突）， H₁=（6+1）MOD10=7（冲突）， H₂=（6+2²）MOD10=0（冲突），H₃=（6+ 33）MOD10=5，所以比较了四次。8素

h(Statue of Liberty)= h (Napoléon)= h (Napoléon) = 10001010 01100001 01100101 flipped bit Should beverydifferent Should besimilar

110101 110110 110111 Query Image Coarse-level Search

互联网黑客

Exercise ・ 在地址空间为o~13的散列区中， 对以下关键字Jan， Feb，Mar，Apr，May， Jun, Jul， Aug，Sep，Oct, Nov, Dec} 构造哈希表， 设哈希函数为 H(key)=li/2」， a中i为key中首字母在字母表中的序号(A对应序号)。 （1）用线性探测再散列方法得到哈希表； （2）计算查找成功和查找失败时的平均查找长度。 Ad. 0 1 2 3 4 5 6 7 8 9 10 11 12 13 Key Apr Aug Dec Feb Jan Mar May Jun Jul Sep  Oct Nov M 1 2 1 1 1 1 2 4 5 2 5 6 表中Ad.为地址号，M为地址计算次数查找成功：AVL成功=总的地址计算次数/填入记录项数= 31/12 查找失败：AVL失败=[(5+4+3+2+1)+(9+8+7+6+5+4+3+2+1)]/14=60/14 Data Structure & Algorithms 1
