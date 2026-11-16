# 09 STL-第十八周

> 主题库：程序设计　｜　源文件：09 STL-第十八周.pdf　｜　8,319 字符　｜　33 个片段　｜　更新于 2026-11-11


C++语言标准模板库(STL)
中山大学程序设计慕课小组
主讲人：陆勇中山大学MOOC课程
组

01
02
2.3
03
STL是什么
容器
适配器
迭代器
CONTENT
S
目 录
05
STL的痛点：编译速度
04
算法
2.2关联容器
2.1顺序容器

容器适配器
•
容器适配器提供顺序容器之上的不同功能接口（界面）。
类    名说    明所在头文件
stack栈。元素的插入和删除只在栈顶
进行，支持后进先出（LIFO）
的元素访问
<stack>
queue队列。在尾端插入元素，在头端
删除元素，支持先进先出
（FIFO）的元素访问
<queue>
priority_deque带优先级管理的队列。元素按优
先级从高到低排列，使用元素类
型的<操作确定优先级
<queue>

栈的功能接口（操作）
操    作操作效果实现方式
push(item)将值为item的新元素压入栈顶，
无返回值
调用基础容器的
push_back操作实现
pop()删除栈顶元素，无返回值调用基础容器的pop_back
操作实现
top() 返回栈顶元素的值调用基础容器的back操
作实现
empty()若栈为空，则返回true；否则，
返回false
调用基础容器的empty操
作实现
size()返回栈中元素的数目调用基础容器的size操作
实现

队列的功能接口（操作）
操    作操作效果实现方式
push(item)将值为item的新元素插入队
尾，无返回值
调用基础容器的push_back操
作实现
pop()删除队首元素，无返回值调用基础容器的pop_front操
作实现
front() 返回队首元素的值调用基础容器的front操作实
现
back()返回队尾元素的值调用基础容器的back操作实现
empty()若队列为空，则返回true；否
则，返回false
调用基础容器的empty操作实
现
size()返回队列中元素的数目调用基础容器的size操作实现

优先队列的功能接口（操作）
操    作操作效果实现方式
push(item)按优先级顺序将值为item的新元素
插入到队列中适当位置，无返回值
首先调用基础容器的push_back操
作，然后调用堆排序算法push_heap
对元素进行重新排列
pop()删除队首元素（优先级最高的元
素），无返回值
首先调用堆操作算法pop_heap删除
堆顶元素，然后调用基础容器的
pop_back操作
top() 返回队首元素的值调用基础容器的front操作实现
empty()若优先级队列为空，则返回true；
否则，返回false
调用基础容器的empty操作实现
size()返回优先级队列中元素的数目调用基础容器的size操作实现
10
back
87320

适配器 与 容器
•
标准库中定义的容器适配器都是基于顺序容器建立的。
•
程序员在创建适配器对象时可以选择相应的基础容器类。
    1）stack适配器可以建立在vector、list或deque容器上。
    2）queue适配器只能建立在list或deque容器上。
    3）priority_queue适配器只能建立在vector或deque容器上。
•
如果创建适配器对象时不指定基础容器，则stack和queue默认
采用deque实现，而priority_queue则默认采用vector实现。 

适配器 与 容器（Stack 为例）
定义于头文件 <stack>
template<
    class T,
    class Container = std::deque<T>
> class stack;
std::stack 类是容器适配器，它给予程序员栈的功能——特别是 FILO （先进后出）数据结构。
模板形参
             T- 存储的元素类型。
Container- 用于存储元素的底层容器类型。容器必须满足序列容器 (SequenceContainer) 的要求。
另外，它必须提供拥有通常语义的下列函数：
•
back()
•
push_back()
•
pop_back()
标准容器 std::vector 、 std::deque 和 std::list 满足这些要求。若不为特定的 stack 类特化指定容器类，
则使用标准容器 std::deque 。

构造函数示例（Stack 为例）
#include <stack>
#include <deque>
#include <iostream>
 
int main()
{
    std::stack<int> c1;
    c1.push(5);
    std::cout << c1.size() << '\n';
 
    std::stack<int> c2(c1);
    std::cout << c2.size() << '\n';
 
    std::deque<int> deq {3, 1, 4, 1, 5};
    std::stack<int> c3(deq);
    std::cout << c3.size() << '\n';
}
Stack_1.cpp

EXP：Stack
// palindrome.cpp  功能： 判断给定文本是否回文（正读反读都一样的文本）  
// 演示stack适配器的使用
bool palindrome(const string& text);
int main() {
    string text;
    while (true) {
        cout << "Enter the text(\"quit\" to end program):" << endl; // 输入文本
        getline(cin, text);
        if (text == "quit")
            break;
        if (palindrome(text))                                       // 是回文
            cout << "The text you typed is a palindrome." << endl;
        else                                                        // 不是回文
            cout << "The text you typed is NOT a palindrome." << endl;
    }
    
    return 0;
} 
stack.cpp

EXP：Stack
bool palindrome(const string& text) {   
    stack<char,std::vector<int>> cstack;                        // 指定容器类型
    size_t length = text.size();
    size_t comparePos;                                          // 比较位置 
    for (size_t i = 0; i < length/2; i++)                       // 将文本text的前半部分压入栈cstack
        cstack.push(text[i]);
    // 将文本text的后半部分逐个字符与前半部分的对应字符进行比较
    if (length % 2 == 0)                                        // 设定比较起点
        comparePos = length / 2;
    else
        comparePos = length / 2 + 1;
    while (!cstack.empty()) {                                   // 比较对应字符
        if (text[comparePos] != cstack.top())                   // 对应字符不相同
            break;
        cstack.pop();                                           // 对应字符出栈
        comparePos++;                                           // 比较位置后移一个字符
    }
    if (cstack.empty()) return true;
    else    return false;
}
stack.cpp

01
02
2.3
03
STL是什么
容器
适配器
迭代器
CONTENT
S
目 录
05
STL的痛点：编译速度
04
算法
2.2关联容器
2.1顺序容器

迭代器 – 类别（1）
迭代器类别
功    
能
支持的操作备    注
输入迭代器
（input iterator）
读
const *
const ->
输入迭代器的解引用（*）操作
的结果只能作为右值
[1]
使用
输出迭代器
（output iterator）
写
*
->
输出迭代器的解引用（*）操作
的结果可以作为左值
[2]
使用
int main() {
// DEMO1: read only iterator
const vector<int> iv {1,2,3,4,5,6,7};
auto it1 = iv.begin();  
// *it1 = 3; // error: assignment of read-only location
set<int> is {1,2,1,3,1,4};
auto it2 = is.begin();
// *it2 = 5;  // error: assignment of read-only location
} 
it-type.cpp

迭代器 – 类别（2）
int main() {
// DEMO2: forward, bidirection, random
forward_list<int> ifl {1,2,3,4,5,6,7};
list<int> ils {11,12,13,14,15,16,17};
auto it3 = ifl.begin();
it3++;
// it3--;  // error: no 'operator--(int)' declared
auto it4 = ils.begin();
it4++; it4--;
// it4 += 7;  // error: no match for 'operator+='
it1 += 7;
} 
it-type.cpp
迭代器类别功能支持的操作备    注
正向迭代器
（forward iterator） 
读/写
++
==,!=
向前访问下一个
双向迭代器
（bidirectional iterator）
读/写
++,--
==,!=
向前或向后
随机访问迭代器
（random access 
iterator）
读/写
++,--,+,-,+=,-=
==,!=,<,<=,>,>=
[]
iter[n]等价于
*(iter+n)

迭代器 – 类别（3）
容器类别读/写迭代器类别备    注
arrayR/Wrandom 
vectorR/Wrandom
dequeR/Wrandom
listR/Wbidirectional
set/multisetRbidirectional
map/multimapRbidirectional
forward_listR/Wforward

迭代器 – 遍历 与 for（C++11）
•
由每个类型的容器自行定义自己的迭代器。但我们
仅关注类别特征
•
用 auto 关键字可以减少对迭代类型的依赖
•
对于一般遍历，都可以写成右边范式代码
•
因此C++11以后，遍历容器引入新语法
•
其中，container 需要满足以下一个
•
是 array
•
或 有 begin() 和 end() 成员函数的对象
•
或 一个花括号初始化器列表
•
其中， const 和 引用 都是可选的 
int main() {
//Demo3: iterating over the elements
auto _begin = ils.begin();
auto _end = ils.end();
for (auto it = _begin; it != _end; it++) {
auto x = *it；
cout << x << ","; 
}
cout << endl;
for (auto x:ils) {
cout << x << ",";
}
cout << endl;
} 
for (const auto& x:container) {...}

迭代器 - 非法化（失效）
只读方法决不非法化迭代器或引用。修改容器内容的方法可能非法化迭代器和/或引用，总结于此表格。
来源：容器库 (gitee.io)
此处插入指代任何添加一或多个元素到容器的方法，而擦除指代任何从容器移除一或多个元素的方法。
超纲啦！

迭代器 – 失效案例研究
int main() 
{
//EX: delete all 3 in vector. 
// 1.请指出程序中的缺陷，并给出样例数据
// 2.请改正该程序 
vector<int> ivv1 {1,2,3,4,5};
for (auto it = ivv1.begin(); it != ivv1.end(); it++) {
if (*it == 3) ivv1.erase(it);  
}
for (auto x : ivv1) cout << x << ",";
cout << endl;
} 
it-type.cpp

01
02
2.3
03
STL是什么
容器
适配器
迭代器
CONTENT
S
目 录
05
STL的痛点：编译速度
04
算法
2.2关联容器
2.1顺序容器

算法
此处说的“算法”指<algorithm>和<numeric>定义的各种容器上的泛型算法：
全部可用算法：cppreference
这里介绍几个使用实例。
// sort.cpp, compile with -std=c++11
int main() {
  using namespace std;
  //vector vec = { 13219, 1203, 3213, 4132 }; // c++20 支持类模板推导 
  vector<int> vec = { 13219, 1203, 3213, 4132 };
  sort(vec.begin(), vec.end());
  for (auto i : vec) {
    std::cout << i << " "; 
  }
} 
sort.cpp
这样是升序，如何降序排序？

算法
使用<algorithm>时常常遇到类似问题，一般传入函数即可实现新的功能。
// sort2.cpp, compile with -std=c++11
int main() {
  using namespace std;
  vector<int> vec = { 13219, 1203, 3213, 4132 };
  sort(vec.begin(), vec.end(), [](int a, int b) { return a > b; });
  for (auto i : vec) {
    std::cout << i << " "; 
  }
}
sort2.cpp
这个是函数指针？
这个是 lambda 函数！

算法
<algorithm>中有一个count_if函数，如果想要统计一个vector中大于15和大于20的数的数量，可以这么
写：
// count_if.cpp compile with -std=c++11
int main() {
  using namespace std;
  vector<int> vec = { 1, 10, 20, 15, 30 };
  auto greater_then_20 = 
    count_if(vec.begin(), vec.end(), [](int i) { return i > 20; });
  auto greater_then_15 = 
    count_if(vec.begin(), vec.end(), [](int i) { return i > 15; });
  
  cout <<  greater_then_20 << "\n"
  <<  greater_then_15 << "\n";
  
}
count-if.cpp

算法
<algorithm> 提供了 accumulate:
这段代码将v中的所有整数相乘，并存入res中。
// accumulate.cpp, compile with -std=c++11/20
int main() {
  using namespace std;
  //vector v = { 1, 2, 3, 4 };
  vector<int> v = { 1, 2, 3, 4 };
  //auto res = accumulate(v.begin(), v.end(), 1, [](auto i, auto j) { return i 
* j; });
  auto res = accumulate(v.begin(), v.end(), 1, [](decltype(v[0]) i, 
decltype(v[0]) j) { return i * j; });
  cout << res;
}
accumulate.cpp
累加初值
函数如 std::multiplies

01
02
2.3
03
STL是什么
容器
适配器
迭代器
CONTENT
S
目 录
05
STL的痛点：编译速度
04
算法
2.2关联容器
2.1顺序容器

STL编译时发生了什么？
由于C++模板的限制，STL以头文件方式分发。
这也就是说，编译STL时首先会将STL里的所有内容都复制粘贴到源文件中
$ time g++ bits.cpp
real    0m1.005s
user    0m0.834s
sys     0m0.041s
// bits.cpp
#include <bits/stdc++.h>
int main() {}
编译毫无内容的bits.cpp，编译器用了惊人的0.834
秒

STL编译时发生了什么？
$ g++ -E bits.cpp | wc -l
105803
将所有C++标准库包含的后果是：
预处理后得到105803行的源文件！

建议与展望
建议：绝对不要在工程项目中使用bits/stdc++.h，只引入需要的头文件。
C++20引入了现代编程语言一般都具有的module系统，可以解决头文件带来的编译速度问题。
但不要过于乐观，C++标准库在C++20中仍然以头文件分发。目前只有微软实现了C++标准库的
module化，msvc16.8以上版本支持以下写法：
import std.core;
using namespace std;
int main()
{
   cout << "hello, world";
}
