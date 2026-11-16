# 11-2-Search2

> 主题库：数据结构与算法　｜　源文件：11-2-Search2.pdf　｜　11,032 字符　｜　45 个片段　｜　更新于 2026-11-11


Hashing 
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Data and Computer Science
Data Structure & Algorithms

哈希（散列）查找
•什么是哈希查找
•哈希函数
•冲突处理
•查找复杂度分析
•应用
Data Structure & Algorithms2

More Efficient Searching
•Question: Can searching be more efficient than 
logarithmic O(log n) significantly?

An Example (1)
•Example: a set of students represented as (id, name, 
data).
11379100Wang Ling...
11379102Gao Min...
11379104Zhang Bin...
11379106Li Ping...
11379107Liang Hao...
.........
11379134Liang Li...

An Example (2): Storing the record in an 
array
•We store the records in an array: a record is stored in 
some address  according its id (key) using the 
function:
    h(key) = last 3 digits of key -100
10379100Wang Ling...
...
10379102Gao Min...
...
10379104Zhang Bin...
10379106Li Ping...
10379107Liang Hao...
.........
0
1
2
3
4
6
6
7
8
h(10379102)=2, so the 
student record with id 
10379102 is stored at 
index 2 in the array.

An Example (3): Searching costs constant 
time
•Searching a student with id = k: compute its 
storing address h(k), access the data at h(k).
•Inserting a student with id = k: compute its 
storing address h(k) and store the record at 
index h(k);
•Deleting a student with id = k: compute its 
storing address h(k) and remove the record at 
index h(k);
•Complexity: constant time as long as computing 
h(k) is easy.

哈希(散列)查找
•基本思想：“关键字->存储地址”的映射（哈希函数）
•在记录的存储地址和它的关键字之间建立一个确定的对应关系；这样，
不经过比较，一次存取就能得到所查元素的查找方法。
Data Structure & Algorithms7

Hashing, Hash Function and Hash Table
•Problem: a set of records of the form (key, data), 
each with a unique key, need efficient insertion, 
deletion and searching.

哈希(散列)查找
•基本概念
•哈希函数：在记录的关键字与记录的存储地址之间建立的一种对应关
系叫哈希函数。
    哈希函数是一种映象，是从关键字空间到存储地址空间的一种映象。可写成：
addr(a
i
)=H(k
i
) ，其中i是表中一个元素，addr(a
i
)是a
i
的地址， k
i
是a
i
的关键字。
•哈希表：应用哈希函数，由记录的关键字确定记录在表中的地址，并
将记录放入此地址，这样构成的表叫哈希表。
•哈希查找(又叫散列查找)：利用哈希函数进行查找的过程叫哈希查找。       
Data Structure & Algorithms9

哈希(散列)查找
•冲突：对于不同的关键字k
i
、k
j
，若k
i
k
j
，但H(k
i
)=H(k
j
)的现象叫冲突
(collision) 。
•同义词：具有相同函数值的两个不同的关键字，称为该哈希函数的同
义词。
•哈希函数通常是一种压缩映象，所以冲突不可避免，只能尽量减少；
当冲突发生时，应该有处理冲突的方法。设计一个散列表应包括：
①  散列表的空间范围，即确定散列函数的值域；
②  构造合适的散列函数，使得对于所有可能的元素(记录的关键字)，
函数值均在散列表的地址空间范围内，且出现冲突的可能尽量小； 
③  处理冲突的方法。即当冲突出现时如何解决。
Data Structure & Algorithms10

哈希函数的构造
        哈希函数是一种映象，其设定很灵活，只要使任何关键字的哈希函数
值都落在表长允许的范围之内即可。哈希函数“好坏”的主要评价因素有：
◆ 散列函数的构造简单，计算速度快
◆ 能“均匀”地将散列表中的关键字映射到地址空间（即输出结果尽量
“随机”）。所谓“均匀” (uniform)是指发生冲突的可能性尽可能最
少。 
Data Structure & Algorithms11

哈希函数的构造
1 直接定址法
        取关键字或关键字的某个线性函数作哈希地址，即H(key)=key    或   
H(key)=a·key+b (a,b为常数)
       特点：直接定址法所得地址集合与关键字集合大小相等，不会发生冲
突，但实际中很少使用。
Data Structure & Algorithms12

哈希函数的构造
2  数字分析法
        对关键字进行分析，取关键字的若干位或组合作为哈希
地址。
       适用于关键字位数比哈希地址位数大，且可能出现的关
键字事先知道的情况。
Data Structure & Algorithms13

哈希函数的构造
•例： 设有80个记录，关键字为8位十进制数，哈希地址为2
位十进制数。
Data Structure & Algorithms14
┇
8  1  3   4  6  5  3  2
8  1  3   7  2  2  4  2
8  1  3   8  7  4  2  2
8  1  3   0  1  3  6  7
8  1  3   2  2  8  1  7 
8  1  3   3  8  9  6  7
8  1  3   6  8  5  3  7
8  1  4   1  9  3  5  5
   
分析:    只取8
              只取1
              只取3、4
              只取2、7、5
             数字分布近乎随机
所以:  取任意两位或两位
            与另两位的叠加作哈希地址

哈希函数的构造
3  平方取中法
•将关键字平方后取中间几位作为哈希地址。
•一个数平方后中间几位和数的每一位都有关，则由随机分布的关键字
得到的散列地址也是随机的。
•散列函数所取的位数由散列表的长度决定。
•这种方法适于不知道全部关键字情况，是一种较为常用的方法。
Data Structure & Algorithms15

哈希函数的构造
4  折叠法
•将关键字分割成位数相同的几部分(最后一部分可以不同)，然后取这几
部分的叠加和作为哈希地址。
•数位叠加有移位叠加和间界叠加两种。
◆ 移位叠加：将分割后的几部分低位对齐相加。
◆ 间界叠加：从一端到另一端沿分割界来回折迭，然后对齐相加。
适于关键字位数很多，且每一位上数字分布大致均匀情况。
Data Structure & Algorithms16

哈希函数的构造
•例： 设关键字为0442205864，哈希地址位数为4 。两种
不同的地址计算方法如下：
Data Structure & Algorithms17
5 8 6 4
4 2 2 0
0 4
1 0 0 8 8
H(key)=0088
移位叠加
5 8 6 4
0 2 2 4
0 4
   6 0 9 2
H(key)=6092
间界叠加

哈希函数的构造
5  除留余数法
•取关键字被某个不大于哈希表表长m的数p除后所得余数作哈希地址，即：
H(key)=key   MOD  p     (pm)
•是一种简单、常用的哈希函数构造方法。
•利用这种方法的关键是p的选取，p选的不好，容易产生同义词。p的选取的分析：
◆  选取p=2
i 
(pm)：运算便于用移位来实现，但等于将关键字的高位忽略而仅留下
低位二进制数。高位不同而低位相同的关键字是同义词。
 ◆ 选取p=qf (q、f 都是质因数，pm)：则所有含有q或f因子的关键字的散列地址
均是q或f的倍数。
 ◆ 选取p为素数或p=qf (q、f是质数且均大于20，pm)：常用的选取方法，能减
少冲突出现的可能性。
Data Structure & Algorithms18

哈希函数的构造
6  随机数法
      取关键字的随机函数值作哈希地址，即H(key) = random(key)
当散列表中关键字长度不等时，该方法比较合适。
选取哈希函数，考虑以下因素
◆ 计算哈希函数所需时间；
◆ 关键字的长度；
◆ 哈希表长度（哈希地址范围）；
◆ 关键字分布情况；
◆ 记录的查找频率。
Data Structure & Algorithms19



冲突处理的方法
•冲突处理：当出现冲突时，为冲突元素找到另一个存储位置。
1  开放定址法
基本方法：当冲突发生时，形成某个探测序列；按此序列逐个探测散列表中的
其他地址，直到找到给定的关键字或一个空地址(开放的地址)为止，将发生冲
突的记录放到该地址中。散列地址的计算公式是： 
H
i
(key)=(H(key)+d
i
)  MOD m，i=1, 2, ..., k(km-1)
其中：H(key)：哈希函数；m：散列表长度；
   d
i
：第i次探测时的增量序列；
H
i
(key) ：经第i次探测后得到的散列地址。
Data Structure & Algorithms21

冲突处理的方法--线性探测法
(1)  线性探测法 (linear probing)
       将散列表T[0 ...m-1]看成循环向量。当发生冲突时，从初次发生冲突的
位置依次向后探测其他的地址。
增量序列为：d
i
=1, 2, 3, ..., m-1
        设初次发生冲突的地址是h，则依次探测T[h+1]，T[h+2]...，直到T[m-1]
时又循环到表头，再次探测T[0]，T[1]...，直到T[h-1]。探测过程终止的情
况是：
◆ 探测到的地址为空：表中没有记录。若是查找则失败；若是插入则
将记录写入到该地址；
◆ 探测到的地址有给定的关键字：若是查找则成功；若是插入则失败；
◆ 直到T[h]：仍未探测到空地址或给定的关键字，散列表满。
Data Structure & Algorithms22

冲突处理的方法--线性探测法
例1 ：设散列表长为7，记录关键字组为：15, 14, 28, 26, 56, 23，散列
函数：H(key)=key   MOD  7，冲突处理采用线性探测法。
解:   H(15)=15  MOD 7=1            H(14)=14  MOD 7=0 
H(28)=28  MOD 7=0  冲突   H
1
(28)=1  又冲突 H
2
(28)=2        
H(26)=26  MOD 7=5
H(56)=56  MOD 7=0  冲突   H
1
(56)=1  又冲突
H
2
(56)=2   又冲突    H
3
(56)=3 
H(23)=23  MOD 7=2  冲突   H
1
(23)=3  又冲突
H
2
(23)=4
Data Structure & Algorithms23
 0      1      2      3       4        5      6
14  15   28   56   23   26

冲突处理的方法--线性探测法
线性探测法的特点
◆ 优点：只要散列表未满，总能找到一个不冲突的散列地址；
◆ 缺点：每个产生冲突的记录被散列到离冲突最近的空地址
上，从而又增加了更多的冲突机会 ( 这种现象称为冲突的
“聚集”)。
Data Structure & Algorithms24

冲突处理的方法--二次探测法（平方探测法）
(2)  二次探测法
增量序列为：d
i
=1²,-1²,2²,-2²,3²,......±k²  (km/2)
•上述{15, 14, 28, 26, 56, 23}若采用二次探测法进行冲突处理：
H(15)=15  MOD 7=1         H(14)=14  MOD 7=0
H(28)=28  MOD 7=0     冲突      H
1
(28)=1     又冲突
H
2
(28)=0    又冲突   H
3
(28)=4
H(26)=26  MOD 7=5
H(56)=56  MOD 7=0     冲突      H
1
(56)=1     又冲突
H
2
(56)=0   又冲突  H
3
(56)=4  又冲突  H
4
(56)=0  又冲突 H
5
(56)=2
H(23)=23  MOD 7=2      冲突      H
1
(23)=3
Data Structure & Algorithms25
14  15   56   23   28   26
 0     1       2       3      4       5       6

冲突处理的方法--二次探测法（平方探测法）
二次探测法的特点
◆ 优点：探测序列跳跃式地散列到整个表中，不易产生冲
突的“聚集”现象；
◆ 缺点：不能保证探测到散列表的所有地址。
Data Structure & Algorithms26

冲突处理的方法--伪随机探测法
(3) 伪随机探测法
•增量序列使用一个伪随机函数来产生一个落在闭区间[1，m-1]
的随机序列。
Data Structure & Algorithms27

Case
例2 ： 表长为11的哈希表中已填有关键字为17，60，29的记
录，散列函数为H(key)=key  MOD  11 。 现有第4个记录，
其关键字为38，按三种处理冲突的方法，将它填入表中。
①H(38)=38 MOD 11=5    冲突
       H1=(5+1) MOD 11=6    冲突
       H2=(5+2) MOD 11=7    冲突
       H3=(5+3) MOD 11=8    不冲突
28Data Structure & Algorithms
0    1    2     3    4     5    6     7     8    9   10
60  17   29
38

Case
②  H(38)=38 MOD 11=5    冲突
       H
1
=(5+1²) MOD 11=6    冲突
        H
2
=(5-1²) MOD 11=4    不冲突
③H(38)=38 MOD 11=5    冲突
      设伪随机数序列为9，则H
1
=(5+9) MOD 11=3 不冲突
Data Structure & Algorithms29
0    1    2     3    4     5    6     7     8    9   10
60  17   29
38
38
38

Exercise
•设有一组关键字{9, 01, 23, 14, 55, 20, 84, 27}, 采用哈希函数
H(key)=key MOD 7, 表长为10，用如下形式的二次探测再散列方法
Hi=(H(key)+di) MOD 10 (di=1
2
, 2
2
, 3
2
, ...,)解决冲突。要求对该关键
字序列构造哈希表，并计算查找成功的平均查找长度。
30Data Structure & Algorithms

Exercise
•在地址空间为0~13的散列区中，对以下关键字 {Jan, Feb, Mar, Apr, May, 
Jun, Jul, Aug, Sep, Oct, Nov, Dec} 构造哈希表，设哈希函数为 H(key)=
 
퐀/2
 
，
其中퐀为key中首字母在字母表中的序号（A对应序号1）。
•（1）用线性探测再散列方法得到哈希表；
•（2）计算查找成功和查找失败时的平均查找长度。
31Data Structure & Algorithms
Ad.012345678910111213
KeyAprAugDecFebJanMarMayJunJulSepOctNov
M121111245256
查找成功：AVL成功 = 总的地址计算次数/填入记录项数 = 31/12
查找失败：AVL失败 = [(5+4+3+2+1)+(9+8+7+6+5+4+3+2+1)]/14 = 60/14
表中Ad.为地址号，M为地址计算次数

冲突处理的方法--再哈希法
2 再哈希法 (Re-hashing)
•构造若干个哈希函数，当发生冲突时，利用不同的哈希函数再计算下一
个新哈希地址，直到不发生冲突为止。即：H
i
=RH
i
(key)     i=1, 2, ..., k
        RH
i 
：一组不同的哈希函数。第一次发生冲突时，用RH
1
计算，第二
次发生冲突时，用RH
2
计算...依此类推知道得到某个H
i
不再冲突为止。
◆  优点：不易产生冲突的“聚集”现象；
◆  缺点：计算时间增加。
Data Structure & Algorithms32

冲突处理的方法--链地址法
3  链地址法 (Chaining Address)
•方法：将所有关键字为同义词(散列地址相同)的记录存储在一个单链表
中，并用一维数组存放链表的头指针。
•设散列表长为m，定义一个一维指针数组：
RecNode *linkhash[m]，其中RecNode是结点类型，每个分量的初值为空。
凡散列地址为k的记录都插入到以linkhash[k]为头指针的链表中，插入位
置可以在表头或表尾或按关键字排序插入。
Data Structure & Algorithms33

冲突处理的方法--链地址法（拉链法）
•例： 已知一组关键字(19, 14, 23, 1, 68, 20, 84, 27, 55, 11, 10, 79) ，哈希
函数为：H(key)=key MOD 13，用链地址法（拉链法）处理冲突，如图所示 。
Data Structure & Algorithms34
用链地址法处理冲突的散列表
平均查找长度：
ASL
成功
=(1x6+2x4+3x1+4x1)/6=21/6
ASL
不成功
=(1x7+2x2+3x3+5x1)/13=25/13

冲突处理的方法--建立公共溢出区
4  建立公共溢出区
方法：在基本散列表之外，另外设立一个溢出表保存与基本表中记录冲突
的所有记录。
设散列表长为m，设立基本散列表hashtable[m]，每个分量保存一个记录；
溢出表overtable[m]，一旦某个记录的散列地址发生冲突，都填入溢出表中。
Data Structure & Algorithms35

冲突处理的方法--建立公共溢出区
•例： 已知一组关键字(15, 4, 18, 7, 37, 47) ，散列表长度为7 ，
哈希函数为：H(key)=key MOD 7，用建立公共溢出区法处
理冲突。得到的基本表和溢出表如下：
Data Structure & Algorithms36
Hashtable表：
散列地址    0      1      2     3      4     5     6
  关键字      7     15    37            4    47    
overtable表：
溢出地址    0      1      2     3      4     5     6
  关键字     18

不同处理冲突方法的对比
•例： 已知一组关键字(19, 14, 23, 1, 68, 20, 84, 27, 55, 11, 10, 79) ，哈希函
数为：H(key)=key MOD 13，用链地址法处理冲突，如图所示 。
37Data Structure & Algorithms
链地址法线性探测再散列
ASL(13) = (1x7+2x2+3x3+5x1)/13 = 1.92ASL(12) = (1x6+2+3x3+4+9)/12 = 2.5
111214313913

哈希查找过程及分析
1   哈希查找过程
        哈希表的主要目的是用于快速查
找，且插入和删除操作都要用到查找。
由于散列表的特殊组织形式，其查找
有特殊的方法。
       设散列为HT[0...m-1]，散列函数
为H(key)，解决冲突的方法为R(x, i) ，
则在散列表上查找定值为K的记录的
过程如图所示。
Data Structure & Algorithms38
给定k值
计算H(k)
此地址为空?
关键字==k?
查找失败
查找成功
按处理冲突
方法计算H
i
N
Y
Y
N
散列表的查找过程

哈希查找过程及分析
#define NULLKEY   -1    /*   根据关键字类型定义空标识   
*/
typedef  struct
{  KeyType   key ;     /*   关键字域   */
otherType  otherinfo ;   /*   记录的其它域   */
}RecType ;
int  Hash_search(RecType HT[], KeyType k, int m)
/*    查找散列表HT中的关键字K,用开放定址法解决冲突   */
{  int h, j ;
h=h(k) ;
while (j<m && !EQ(HT[h].key, NULLKEY) )
        {   if (EQ(HT[h].key, k) )   return(h) ;
else h=R(k, ++j) ;   
}
return(-1) ;
}
Data Structure & Algorithms39
#define M 15
typedef struct node
{  KeyType key;
struct node *link;
}HNode;
HNode *hash_search(HNode *t[], KeyType k)
{  HNode *p;    int i;
i=h(k);
if (t[i]==NULL)     return(NULL);
p=t[i];
while(p!=NULL)
if (EQ(p->key, k))  return(p);  
else  p=p->link;
return(NULL);
}      /*   查找散列表HT中的关键字K,用链地
址法解决冲突  */
2  查找算法

哈希查找分析
•从哈希查找过程可见：尽管散列表在关键字与记录的存储地址之间建
立了直接映象，但由于“冲突”，查找过程仍是一个给定值与关键字
进行比较的过程，评价哈希查找效率仍要用ASL。
•哈希查找时关键字与给定值比较的次数取决于：
◆ 哈希函数；
◆ 处理冲突的方法；
◆ 哈希表的填满因子 。填满因子的定义是：
Data Structure & Algorithms40
表中填入的记录数
哈希表长度
=

哈希查找分析
•各种散列函数所构造的散列表的ASL如下
(1)   线性探测法的平均查找长度是：
 (2)   二次探测、伪随机探测、再哈希法的平均查找长度是：
(3)   用链地址法解决冲突的平均查找长度是：
Data Structure & Algorithms41

思考
•顺序查找，二分查找，哈希(散列)查找的时间分别为O(n)，O(logn)，O(1)，
既然有了高效的查找方法，为什么低效的方法还不放弃？
•时间复杂度是判断查找方法的一个重要指标，但不是唯一的指标。使用哪
种查找方法需要综合考虑。
•哈希查找时间效率为O(1)，查找速度最快，但需要构建哈希函数，计算哈
希地址，查找时需要有解决冲突的方法；
•二分查找时间O(logn)，需要元素有序且顺序存储，排序操作的时间开销大；
•顺序查找时间最差为O(n)，但对查找表没有要求，数据有序和无序都可以，
在数据量比较小的时候使用方便。
42Data Structure & Algorithms

哈希的应用
•搜索引擎
•随着互联网上图像的数据信息日渐庞大，用户对网上图像搜索的要求也在
不断提高，图像搜索引擎应运而生。以图搜图，搜索引擎根据用户上传的
图片，搜索出与之相同或相似的图片。
•搜图的原理是对用户上传的图片生成一个“指纹”特征值，与数据库中已
有的图片指纹比对，结果越接近，说明图片越相似。这里的关键技术称为
“感知哈希算法（Perceptual Hash Algorithm）”。
43Data Structure & Algorithms

哈希的应用
•数字签名
•散列技术的应用除了存储方法外，还有密码学领域和版权保护等领
域。例如，著名的MD5加密方法也是一种散列。
44Data Structure & Algorithms
