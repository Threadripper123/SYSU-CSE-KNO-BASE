# 06-String-图集

> 主题库：数据结构与算法　｜　源文件：06-String-图集.pdf　｜　3,137 字符　｜　36 个片段　｜　更新于 2026-11-11
typedef struct ElemType data[LIST_SIZE]; annlas aSequenList

数组下标内存元素序号 0 a1 1 1 a2 2 n 1 an LAST LIST_SIZE-1

AScll printablecharacters oloI DEC HEX Simbolo DEC cio 64 40h 96 65 41h 97 66 42h B 98 67 43h c 99 68 44h D 100 69 45h E 101 70 46h F 102

A B C D A B C A B CA ABCABCD

0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 a b a c a à b a c c a b a c a b a a a b a c a b  b a c a b a s a c a b  b a c a b a  a c a b a b a _c a b abacab

1.S[0]为B， P[0]为A， 不匹配， 执第② 条指令：“如果失配 (即S[j]！=P[j]）， 令i=i-(-1)，j=0”， S[1]跟P[0] 匹配，相当于模式串要往右移动位 （i=1，j=0） BBC ABCDAB  ABCDABCDABDE

2.S[1]跟P[0]还是不匹配， 继续执第 ②条指令：“如果失配 (即S[j]！=P[j]）， 令i=i-(-1)，j=0”， S[2]跟P[0] 匹配 (i=2，j=0）， 从而模式串不断的向右移动一位 （不断的执 “令i=i-(-1)， =0”，i从2变到4，j一直为0） BBC ABCDAB ABCDABCDABDE

3.直到S[4]跟P[0]匹配成功 (i=4，j=0），此时按照上面的暴力匹配算法的思路，转而执行第①条指令：“如果当前字符匹配成功 （即S[i]== P[i]） ， 则i++， j++”， 可得S[j为S[5]， P[]为P[1]， 即接下来S[5]跟P[1]匹配 （i=5， j=1)

4.S[5]跟P[1]匹配成功， 继续执第①条指令：“如果当前字符匹配成功 (即S[==P[j]）， 则i++，j++”， 得到 S[6]跟P[2]匹配 （i=6， j=2）, 如此进下去 BBC ABCDAB 3ABCDABCDABDE

5.直到S[10]为空格字符，P[6]为字符D (i=10，j=6）， 因为不匹配， 重新执第②条指令：“如果失配 (即S[j]！= P[j]）， 令i=i-(-1)， j=0”， 相当于S[5]跟P[0]匹配 (i=5， j=0) ABCDA

6. 此，我们可以看到，如果按照暴匹配算法的思路，尽管之前本串和模式串已经分别匹配到了S[9、 P[5]， 但因为S[10]跟P[6]不匹配， 所以本串回溯到S[5]， 模式串回溯到P[0]， 从让S[5]跟P[0]匹配。 S[5]肯定跟P[0]失配。为什么呢？因为在之前第4步匹配中，我们已经得知S[5]=P[1]=B， P[0]=A，即P[1] !=P[0]，故S[5]必定不等于P[0]，所以回溯过去必然会导致失配。那有没有种算法，让i不往回退，只需要移动j即可

继续拿之前的例子来说，当S[10]跟P[6]匹配失败时，KMP不是简单的如朴素匹配那样把模式串右移位， 是执第②条指令：“如果j!=-1， 且当前字符匹配失败 （即S[!=P[j]）， 则令i不变，j=next[i]”， 即j从6变到2 （后我们将求得P[6]， 即字符D对应的next值为2）， 所以相当于模式串向右移动的位数为j-next[j]位 (j-next[j]=6-2= BBC  ABCDAB ABCDABCDABDE   ACDA 向右移动4位后，S[10]跟P[2]继续匹配。为什么要向右移动4位呢，因为移动4位后，模式串中有个“AB”可以继续跟S[8]S[9]匹配，相当于在模式串中找相同的前缀和后缀，然后根据前缀后缀求出next数组，最后基于next数组进匹配 （不关 next数组怎么求来的， 只想看匹配过程是咋样的， 可直接跳到下 3.3.4节）。 BBC ABCDAB ABCDABCDABDE

假设现在本串S配到i位置，模式串P配到位置 。 如果j=-1， 或者当前字符匹配成功 （即S ==P]1）， 都令i++，j++， 继续匹配下个字符； 。 如果j!=-1， 且当前字符匹配失败 （即S[!=P[j]）， 则令i不变，j=next[j]。 此举意味着失配时， 模式串P相对本$S向移动了-nxt位。 ■换之，当匹配失败时，模式串向右移动的位数为：失配字符所在位置-失配字符对应的next 值(next数组的求解会在下的3.3.3节中详细阐述），即移动的实际位数为：j-next[j，且

模式串的各个子串前缀后缀最大公共元素长度 A 0 AB A ABC AAB CBC 0 ABCd A,AB,ABC D,CD,BCd 0 ABCDAB A,AB,ABC,ABCD,ABCDA B,AB,DAB,CDAB,BCDAB 2 ABCDABD 。 ABCDAB BCDABD

字待 A B C D A B D 最大前缀后缀 1 2 公共元素长度

BBC ABCDAB ABCDABCDABDE

BBC ABCDAB ABCDABCDABDE ABCDABD

BBC ABCDAB ABCDABCDABDE AA

模式串 A B C D A B D 前后缀最公 0 0 0 0 1 2 0 共元素长度

BBC ABCDAB ABCDABCDABDE

BBC ABCDAB ABCDABCDABDE

BBC ABCDAB ABCDABCDABDE ABCDABD

BBC ABCDAB ABCDABCDABDE AA

模式串 A B C D A B  前后缀最公 0 0 0 0 1 2 0 共元素长度

模式串 A B C D A B D 最大长度值 0 0 0 0 1 2 0 next数组 -1 0 0 0 1 2

学符 A B C D A B D Next值 -1 0 0 0 0 1 2

BBC ABCDAB ABCDABCDABDE ABCDABD

BBC ABCDAB ABCDABCDABDE ABCAB

BBC ABCDAB ABCDABCDABDE ABCDABD

BBC ABCDAB ABCDABCDABDE

BBC ABCDAB ABCDABCDABDE

BBC ABCDAB ABCDABCDABDE ABCDAE D

0 1234 5 模式串 abcd a b d next -1 0 0 0 2

-C1 C2 -C3 B1 B2 B3

j=nextlil -C1 C2 C3 B1 B2 B3

Naivepatternmatehing xhaustion 0 1 2 3 4 5 6 7 8  9 10 11 12 13 14 15 16 17  a b a c a a b a c c a b c   a a b  a a a b a c a b a b a c a b a b a c a b a b a c a b a b a c a b a b a c a b abacab 把模式与目标逐一进行比较（首位置开始），直到碰到不匹配的字符为止算法可在第一个匹配或是目标的结束处 。 Data Structure & Algorithms 20
