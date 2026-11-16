# 09 STL-第十七周

> 主题库：程序设计　｜　源文件：09 STL-第十七周.pdf　｜　14,982 字符　｜　53 个片段　｜　更新于 2026-11-11


C++语言标准模板库(STL)
中山大学程序设计慕课小组
主讲人：陆勇中山大学MOOC课程
组

01
02
03
04
05
STL是什么
容器
迭代器
算法
STL与值语义
CONTENT
S
目 录
06
STL的痛点：编译速度

01
02
03
04
05
STL是什么
容器
迭代器
算法
STL与值语义
CONTENT
S
目 录
06
STL的痛点：编译速度

STL是什么
•
C++标准库的一部分
•
STL的称呼是历史原因导致的，目前的标准中已经没有STL字眼
•
在C++20标准中，STL指的是的如下三章所定义的库：
•
容器库（Containers library, Chap. 26）
•
迭代器库（Iterators library, Chap. 27）
•
算法库（Algorithms library, Chap. 28）
Ref: C++20标准草稿

STL的历史
1993年，Alex Stepanov开发出
STL的原型（Generic C++ 
Components）。后被C++标准委
员会采纳为C++标准的一部分，采
纳时的名称叫The Standard 
Template Library
Ref: The Standard Template Library

STL的历史
In late 1993, I became aware of a new approach to 
containers and their use that had been developed by 
Alex Stepanov. The library he was building was called 
“The STL”. Alex then worked at HP Labs but he had 
earlier worked for a couple of years at Bell Labs, where 
he had been close to Andrew Koenig and where I had 
discussed library design and template mechanisms with 
him. He had inspired me to work harder on generality 
and efficiency of some of the template mechanisms, but 
fortunately he failed to convince me to make templates 
more like Ada generics. Had he succeeded, he wouldn’t 
have been able to design and implement the STL!
Ref: Evolving a language in and for the real world: C++ 
1991-2006

使用容器：exp1.cpp
#include <vector>
#include <iostream>                                                             // exp1.cpp
using namespace std;
int main()
{
    
    vector<int> ivec = {0, 1, 2, 3, 4, 5, 6, 7, 8, 9};           // ivec包含10个元素，值分别为0~9
    ivec.front() = 100;                                          // 将第0个元素修改为100
    cout << "the first element: " << ivec[0] << endl;            // 输出第0个元素的值
    ivec[1] = 102; // 将第1、第2个元素修改为102、103
    ivec.at(2) = 103;
     
    cout << "the second element: " << ivec.at(1) <<endl;         //输出第1、第2个元素的值
    cout << "the third element: " << ivec[2] << endl;
     
    ivec.back() = 999;                                           //将最后一个元素修改为999
    cout << "the last element: " << ivec[9] << endl;             // 输出最后一个元素的值
}
ivec

使用容器：exp1.cpp
100102103345678999
ivec
the first element: 100
the second element: 102
the third element: 103
the last element: 999
要点解释：
•
vector<T> 是动态的连续数组。动态指它的长度是可变的；连续指它的元素可用 T* 访
问。
•
它实现了两种元素访问
•
operator [] ，注意：不检查下标是否越界
•
访问元素函数（返回元素的左值）：front，at(int)，back。其中，at 检查下标越
界，抛出异常
•
实现了列表初始化（直接列表和复制列表）
列表初始化 (C++11 起) (gitee.io)容器库 (gitee.io)

使用迭代器：exp2.cpp
#include <iostream>
#include <vector>
using std::vector;
int main()
{
vector<int> ivec(10, 2);// 创建含10个值为2的元素的vector容器
vector<int>::iterator iter;// 声明迭代器对象
vector<int>::reverse_iterator riter; // 声明反向迭代器对象
iter = ivec.begin(); // 获取指向第0个元素的迭代器
*iter += 10;// 将第0个元素的值加10
riter = ivec.rend(); // riter指向第0个元素的前一位置
*(riter - 1) += 10;// 将第0个元素的值加10
iter = ivec.end(); // iter指向最后一个元素的下一位置
*(iter - 1) = 100; // 将最后一个元素的值改为100
riter = ivec.rbegin(); // riter指向最后一个元素
*riter -= 20;// 将最后一个元素的值减20
}

使用迭代器：exp2.cpp
222222222280
ivec
ivec.begin()ivec.end()
ivec.rbegin()
ivec.rend()
+>
<+
要点解释：
•
迭代器（Iterator）是用于遍历容器元素的指针对象的包装。
•
单向迭代器实现了以下运算符重载
•
operator ++ ，==, != , * 等，例如：++ 表示取容器中的下一个元素
•
容器会提供一个或多个迭代器实现。例如：
•
vector 提供正向和反向的迭代器。
•
通过成员函数 begin()，end() 返回正向迭代器对象实例。
•
通过成员函数 rbegin()，rend() 返回反向迭代器对象实例 

C++11关键字：auto 和 decltype
auto：占位符类型说明符
对于变量，auto x = expr; 从初始化器推导类型
decltype 说明符
decltype（expr）申明从表达式得到的类型
vector<int> ivec(10, 2);
decltype(ivec.begin()) iter;
for (auto it = ivec.begin(); it != ivec.end(); it++ ) {
std::cout  << *it << "," ;
}
std::cout << std::endl;
灵活使用 auto 和 decltype 可以减少程序对模板实参的依赖，提升程序通用性和可读性

扩展阅读：反向迭代器示例
template <typename T>//template-reverse-it.cpp 
class ReverseIterator {
  T* p;
public:
  ReverseIterator(T* x) :p(x) {}
  ReverseIterator(const ReverseIterator& mit) : p(mit.p) {}
  ReverseIterator& operator++() {--p;return *this;}
  ReverseIterator operator++(int) {ReverseIterator tmp(*this); operator+
+(); return tmp;}
  bool operator==(const ReverseIterator& rhs) const {return p==rhs.p;}
  bool operator!=(const ReverseIterator& rhs) const {return p!=rhs.p;}
  T& operator*() {return *p;}
};

基本概念：容器与迭代器
C++标准：Containers are objects that 
store other objects. （容器是储存其他对象
的对象）
容器分为四种：
•
序列式容器
•
关联式容器
•
无序关联式容器
•
容器适配器
简单来说，每个容器提供了一种适配大部分
类型的数据结构

基本概念：容器与迭代器
迭代器：迭代器是每种容器各自定义的
一个或多个不同于容器的类，比如
vector<T>::iterator，它主要用于
访问、修改、增加、删除容器中的元素。
按照功能的不同，C++17之前的迭代
器分为: LegacyInputIterator（输入迭代
器）, LegacyOutputIterator（输出迭代
器）, LegacyForwardIterator（单向迭
代器）, LegacyBidirectionalIterator（双
向迭代器）, 
LegacyRandomAccessIterator（随机迭
代器）五种，C++17加入了 
LegacyContiguousIterator （连续迭代
器）

01
02
03
04
05
STL是什么
容器
迭代器
算法
STL与值语义
CONTENT
S
目 录
04
STL的痛点：编译速度

01
02
03
序列式容器
关联式容器
容器适配器
containe
rs
容 器

序列式容器类型
templateDescription (I&D=insertion&deletion)header
Vector
（动态的连续数组）
1. Implemented by array.
2. Fast I&D at the tail. 尾部
3. Random access supported.
<vector>
List
（双链表）
1. Implemented by double linked list.
2. Fast I&D at arbitrary position.任意位置
3. Random access NOT supported.
<list>
Deque
（双端队列）
1. Implemented by pointer array.
2. Fast I&D at both ends. 双端
3. Random access  supported.
<deque>
L
L[k]
L[k]
V[k]
D[k]
D
v

序列式容器的构造函数
函数使用形式说    明
C<T> c;
创建空容器。
C<T> c(cx);
创建容器c作为cx的副本，c和cx必须是同类型且元素类型
也相同的容器。
C<T> c(b, e);
创建c，并用迭代器b和e所标示范围内的元素对c进行初始
化（c中存放b和e范围内元素的副本）。注：[b,e)半开区
间
C<T> c(n, t);
创建c，并在其中存放n个值为t的元素，t必须是T类型的
值，或者可以转换为T类型的值。
C<T> c(n);
创建c，并在其中存放n个元素。每个元素都是T类型的值
初始化元素。
C<T> c({...});
C<T> c = {...}; 
创建c，用初始化列表中的元素作为c的元素。这是C++11
加入的新方法，最为便利。

容器的构造函数：实例
•
分配指定数目的元素，并对这些元素进行值初始化：
            vector<int> ivec1(10);                     // ivec1包含10个0值元素
•
分配指定数目的元素，并将这些元素初始化为指定值：
            vector<int> ivec2(10, 1);             // ivec2包含10个值为1的元素
•
将vector对象初始化为一段元素的副本：
           int ia[10] = { 0, 1, 2, 3, 4, 5, 6, 7, 8, 9 };
     vector<int> ivec3(ia, ia + 10);             // ivec3包含10个元素，值分别为0~9
•
将一个vector对象初始化为另一个vector对象的副本：
           vector<int> ivec4(ivec3);                  // ivec4包含值为0~9的元素（与ivec3相同）
•
使用初始化列表： vector<int> ivec5 = { 1, 2, 3, 4 }; // ivec5包含1~4的元素

访问序列式容器中的元素
使用形式形式参数返回值备    注
c.back()无
容器c中最后
一个元素的引
用
若容器为空，则该操作的
行为没有定义
c.front()无
容器c中第0
个元素的引用
若容器为空，则该操作的
行为没有定义
c[index] 
index为元
素的下标
（元素在容
器中序号）
返回下标为
index的元素
的引用
list容器不提供该操
作。若下标越界（即小于
0或大于元素数-1），
则该操作的行为没有定义
c.at(index)
index为元
素的下标
返回下标为
index的元素
的引用
list容器不提供该操
作。进行越界检查

访问序列式容器中的元素：迭代器
使用形式返回值备    注
c.begin()
c.cbegin()
迭代器。指向容器c中第0个元
素
带c的方法，如c.cbegin()返回的是
常迭代器，这样的迭代器是只读迭代
器，只能用于读取元素，不能用于修改
元素。（C++11引入）
除此之外，此表中的每个不带c的操作
都有两个版本：一个返回普通迭代器，
一个返回常迭代器。调用何种按照函数
重载的一般规则决定。（也即一共有12
种方法，C++11之前为8种）
c.end()
c.cend()
迭代器。指向容器c中最后一个
元素的下一位置
c.rbegin()
c.crbegin() 
逆向迭代器。指向容器c中最后
一个元素
c.rend
c.crend()
逆向迭代器。指向容器c中第0
个元素的前一位置

在序列式容器中插入元素
使用形式形式参数返回值操作效果
c.insert(iter, t)iter为常迭代
器，t为元素
值
指向新插入元
素的迭代器
在iter所指元素之前插
入值为t的元素
c.insert(iter, n, t)iter为常迭代
器，t为元素
值
无在iter所指元素之前插
入n个值为t的元素
c.insert(iter, b, e) iter为常迭代
器、b和e均
为迭代器
无在iter所指元素之前插
入b和e所指范围内的元
素（不包括e所指向的元
素）
c.push_back(t)t为元素值无在c的尾端增加值为t的
元素
c.push_front(t)t为元素值无在c的头端增加值为t的
元素
C++11前
为普通迭
代器

在序列式容器中插入元素（1）：addElementsDemo.cpp
#include <iostream>
#include <string>
#include <vector>
#include <deque>
using std::deque, std::vector, std::string, std::cout, std::endl;
int main()
{
vector<int> ivec;// 创建空的vector容器，用于存放int型对象
deque<string> sdeq; // 创建空的deque容器，用于存放string型对象
int iarr[] = {100, 100, 100};
// 在vector容器中增加元素：在尾端增加10个元素：值为1~10
for (int i = 1; i < 11; i++)
{
ivec.push_back(i);
}
12345678910
ivec

在序列式容器中插入元素（2）：addElementsDemo.cpp
// 在vector容器头端再增加一个元素，值为20
ivec.insert(ivec.begin(), 20);
// 在vector容器的第四个元素后再增加两个元素，值均为30
ivec.insert(ivec.begin() + 4, 2, 30);
//将数组iarr中的元素增加到vector容器尾端。注意：被插入的元素不包括
//第三个参数所指向的元素因此，要插入iarr中的所有元素，第三个参数应
//该为iarr加3
ivec.insert(ivec.end(), iarr, iarr + 3);
// 在deque容器中增加元素
sdeq.push_back("is");
sdeq.push_front("this");
sdeq.insert(sdeq.end(), "an");
sdeq.insert(sdeq.end(), "example");
2012345678910
ivec
20123303045678910
20123303045678910100100100

在序列式容器中插入元素（3）：addElementsDemo.cpp
// 输出vector容器中的元素
cout << "vector:" << endl;
for (vector<int>::iterator it = ivec.begin(); it != ivec.end(); it++)
cout << *it << ' ';
cout << endl;
// 输出deque容器中的元素
cout << "double-ended queue:" << endl;
for (deque<string>::iterator it = sdeq.begin(); it != sdeq.end(); it++)
cout << *it << ' ';
return 0;
}
练习：打开cpp中print函数模板，并使用该函数反向输出 ivec 和 sdeq 中的元
素

在序列式容器中删除元素
使用形式形式参数返回值操作效果备    注
c.clear()无无
删除容器中的所
有元素
c.erase(iter)
iter为
迭代器
常迭代器。指
向被删除元素
的下一元素
删除iter所指
向的元素
若iter等于c.end()，
则该操作的行为没有定义
c.erase(b, e) 
b和e为
迭代器
常迭代器。指
向被删除元素
段的下一元素
删除b和e所指
范围内的所有元
素（不包括e所
指向的元素）
c.pop_back()无无
删除容器中最后
一个元素
若容器为空，则该操作的
行为没有定义
c.pop_front(t)无无
删除容器中的第
一个元素
若容器为空，则该操作的
行为没有定义。vector
容器不提供该操作
C++11
前为普通
迭代器

在序列式容器中删除元素(1)：elementDeleteDemo.cpp
int main()
{
deque<int> ideq = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
// 输出删除操作之前deque容器中的所有元素
cout << "before delete:" << endl;
print(ideq.begin(),ideq.end());
// 删除容器中的第一个及最后一个元素
ideq.pop_front();
ideq.pop_back();
auto iter = ideq.begin();// iter指向ideq中现存的第0个元素
ideq.erase(ideq.erase(iter + 1)); // 删除ideq中现存的第1、第2个元素
  // 输出删除操作之后list容器中的所有元素
23456789
256789

在序列式容器中删除元素(2)：elementDeleteDemo.cpp
// 删除容器中现存的前三个元素
ideq.erase(ideq.begin(), ideq.begin() + 3);
// 输出删除操作之后list容器中的所有元素
cout << "three elements at front are deleted:" << endl;
print(ideq.begin(),ideq.end());
// 删除剩余的所有元素
ideq.clear();
cout << "after clear:" << endl;
if (ideq.empty()) // 容器为空
cout << "no element in double-ended queue" << endl;
return 0;
}
789

序列式容器的比较操作
比较操作
比较结果
== 
若两个容器中的元素个数相同且对应
位置上的每个元素都相等，则比较结
果为true，否则为false
!=
结果与==操作相反
<，<=，>，
>= 
若一个容器中的所有元素与另一容器
中开头一段元素对应相等，则较短的
容器小于另一容器；否则，两个容器
中第一对不相等元素的比较结果就是
容器的比较结果

序列式容器比较(1)：containerCompare.cpp 
int main()
{
    int iarr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    list<int> ilist1(iarr, iarr+10);
    list<int> ilist2(iarr, iarr+5);
    list<int> ilist3(ilist2);
    list<int> ilist4(ilist2);
   
    ilist4.push_back(12);
    ilist4.push_back(7);
    
    //print all
    vector<string> name = {"list1","list2","list3","list4"};
    vector<list<int>> vec_list = {ilist1,ilist2,ilist3,ilist4};
    for (int i = 0; i < 4; i++) {
    cout << name[i] << ": ";
print(vec_list[i].begin(),vec_list[i].end());
}  

序列式容器比较(1)：containerCompare.cpp 
int main()
{
    int iarr[] = {1, 2, 3, 4, 5, 6, 7, 8, 9, 10};
    list<int> ilist1(iarr, iarr+10);
    list<int> ilist2(iarr, iarr+5);
    list<int> ilist3(ilist2);
    list<int> ilist4(ilist2);
   
    ilist4.push_back(12);
    ilist4.push_back(7);
    
    //print all
    vector<string> name = {"list1","list2","list3","list4"};
    vector<list<int>> vec_list = {ilist1,ilist2,ilist3,ilist4};
    for (int i = 0; i < 4; i++) {
    cout << name[i] << ": ";
print(vec_list[i].begin(),vec_list[i].end());
}  
list1: 1 2 3 4 5 6 7 8 9 10
list2: 1 2 3 4 5
list3: 1 2 3 4 5
list4: 1 2 3 4 5 12 7

序列式容器比较(2)：containerCompare.cpp 
//containerCompare
cout << "ilist2 == ilist3 : ";
if (ilist2 == ilist3)
 cout << "true" << endl;
else
cout << "false" << endl;
    cout << "ilist1 < ilist2 : ";
if (ilist1 < ilist2)
 cout << "true" << endl;
else
cout << "false" << endl;
    //。。。
}  
list1: 1 2 3 4 5 6 7 8 9 10
list2: 1 2 3 4 5
list3: 1 2 3 4 5
list4: 1 2 3 4 5 12 7

序列式容器的容量操作
使用形式形式参数返回值操作效果
c.empty()无
若容器为空，则返回
true；否则返回
false
c.size()无
返回容器中目前所存
放的元素的数目，类
型为C::size_type
c.max_siz
e() 
无
返回容器中可存放元
素的最大数目，类型
为C::size_type
c.resize(
n)
n为元素数
目
无
将容器的大小调整为可存放n个元
素。若n < c.size()，则删除多
余元素；否则，在尾端增加相应数
目的新元素，新元素均采用值初始
化
c.resize(
n, t)
n为元素数
目，t为元
素值
无
新增元素取值为t，其余效果同
c.resize(n)

序列式容器的赋值和交换
使用形式形式参数操作效果备    注
c1 = c2c2为容器首先删除c1中的所有
元素，然后将c2中的
元素复制给c1。
c1和c2必须是同类型
容器且其元素类型也必
须相同。
c.assign(b, e)b和e为一
对迭代器
首先删除c中的所有
元素，然后将迭代器
b和e所指范围内的
元素复制到c中。
b和e不能指向c中
的元素。
b和e所指范围内元
素的类型不必与c的元
素类型相同，只需类型
兼容即可。
c.assign(n, t) n为元素数
目，t为元
素值
首先删除c中的所有
元素，然后在c中存
放n个值为t的元
素。
c1.swap(c2)c2为容器交换c1和c2的所有
元素。实际 上是c1与
c2交换名称 。
c1和c2必须是同类型
容器且其元素类型也必
须相同。

01
02
03
序列式容器
关联式容器
容器适配器
containe
rs
容 器

关联式容器
05110000Zhang 
San
05110001Li Si
05110002Wang Wu
05110003Lin Liu
Key: No. Mapped: Name 
pair 
Set 
常见字典数据
•
学生名册
•
电话簿
•
词典（维基百科）
•
书目
•
....
 基础术语
•
Map /Dict 映射/字
典
•
Key 关键字
•
Value 映射值
•
Pair  /Item 对/项
•
Set 集合
•
无序
•
不重复

关联式容器
类    名说    明所在头文件
map通过键 进行元素存取的关联 数组<map>
multimap支持重复键 的关联 数组<map>
set任意元素的集合<set>
multiset可以重复的集合<set>
unordered_map无序的map，类似于哈希 表<unordered_map>
unordered_set无序的set，类似于哈希 表<unordered_set>

std::pair
std::map的某些函数使用了<utility>中的类std::pair，pair<T1, T2>
代表一个由类型T1和类型T2组成的有序对。
可以直接用构造函数 std::pair<T1, T2> (v1, v2) 进行构造，也可以用
make_pair进行构造：
auto p = make_pair(v1, v2);
通过p.first访问第一个元素，通过p.second访问第二个元素

map的构造函数
函数使用形式说    明
map<K, T> m;
创建空的map容器m，其键类型为K，值
类型为T
map<K, T> m(mx);
创建map容器m作为mx的副本。m和mx
的键类型和值类型都必须相同
map<K, T> m(b, e);
创建map容器m，并用迭代器b和e所标
示范围内的元素对m进行初始化（m中存
放b和e范围内元素的副本）
map<K, T> m = 
   { {...}, {...} }
通过初始化列表创建map，最为简便的方
法。（C++11加入）

向map中插入元素
用map::insert插入，一共有9种重载。
std::map<string, int> m = { { “hello”, 1 } };
m.insert({ “h”, 10 });
m.insert(std::pair{"Kageyama", 180.6});
返回值为一个std::pair，第一个元素为一个指向被插入元素的迭代器，第二个元素指示插入是否成
功。
最简便的方法是直接用operator[]来插入元素：
m["me"] = 10
事实上，由于插入和修改这两个核心操作都可以用这种写法，我们推荐这种写法以保证代码的洁净。

在map中查找元素
使用形式形式参数返回值
m.find(k)
k为要查找
的键
若容器m中存在与k对应的元
素，则返回指向该元素的迭代
器；否则，返回指向m中最后
一个元素的下一位置的迭代器
(即m.end() )。
m.count(k)
k为要查找
的键
k在容器m中的出现次 数。

在map中删除元素
使用形式形式参数返回值操作效果备    注
m.erase(k)
k为要删除元
素的键
被删除元素的个
数，其类型为
map容器中定义
的size_type
若容器m中存
在键为k的元
素，则删除该
元素
若返回值为0，表示
要删除的元素不存在
m.erase(iter)
iter为指向
要删除元素的
迭代器
无
删除iter所
指向的元素
若iter等于
c.end()，则该操作
的行为没有定义
m.erase(b, e) 
b和e为迭代
器，表示要删
除元素的范围
无
删除b和e所
指范围内的所
有元素（不包
括e所指向的
元素）
要么b和e相等（此
时删除范围为空，不
删除任何元素），要
么b所指向的元素出
现在e所指向的元素
之前

map的实例演示：电话簿
*********************************************
  1--insert             2--delete a item
  3--search             4--delete some items
  5--modify             6--display
  0--quit
*********************************************
Enter your choice:1
Enter the name you want to insert: ma
Enter the phone number(s) : 12-3122-1
*********************************************
  1--insert             2--delete a item
  3--search             4--delete some items
  5--modify             6--display
  0--quit
*********************************************
Enter your choice:3
Enter the name you want to search: ma
The phone number of ma is 12-3122-1
源码：phoneNumberBook.cpp

multimap

不支持下标操作。

insert操作每 调用一次都会增加新的元素
（multimap容器中，键相同的元素相邻
存放）。

以键值为参数的erase操作删除该键所关
联的所有元素，并返回被删除元素的数目。

count操作返回指定键的出现次数。

find操作返回的迭代器指向与被查找键相
关联的第一个元素。

结合使用count和find操作依 次访问
multimap容器中与 特定键关联的所有元素。
ChenQi84111111
LiSi84111112
LiSi84111111
LiSi84111234
LiSi 84110100
Wangwu84111100
ZhangSan84111123

multimap
使用形式说    明备    注
m.lower_bound(k)
该操作返回一个迭代
器，指向容器m中第
一个键 >= k的元素
若键k在容器中
不存在，则这两
个操作所返回的
迭代器相同，都
指向k应该插入
的位置
m.upper_bound(k)
该操作返回一个迭代
器，指向容器m中第
一个键 > k的元素
m.equal_range(k) 
该操作返回包含一对
迭代器的pair对
象，其first成员 等
价于
m.lower_bound(k)
，其second成员 则
等价于
m.upper_bound(k)

set
map支持的操作set基本上都支持，但有区别。如下：
1）不支持下标操作。
2）没有定义mapped_type类型 
3）set容器定义的value_type类型不是pair类型，而是与key_type相同，指的
都set中元素的类型。 
std::set<int> s = { 1, 2, 3, 4 };
auto iter1 = s.find(3);
auto iter2 = s.insert(3); // 返回值为指向之前的3的迭代器，没有实际插入发
生
auto b = iter1 == iter2;  // b 为 true
