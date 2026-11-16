# string + inline + macro

> 主题库：程序设计　｜　源文件：string + inline + macro.pdf　｜　5,273 字符　｜　13 个片段　｜　更新于 2026-11-11


补充材料
string + inline + macro

cstring和string是表示字符串的不同方法，
因此有不同，也有相似之处，如提供的函
数的功能很相似，但使用方法不同。
•
cstring:以null字符结尾的字符数组, 要调用对c串
进行操作的函数，如strlen, strcpy,strcmp等函数要
include<cstring>
•
 string是一个类，类里包含数据和成员函数,要使用
string这种数据类型，需要#include<string>
•
建议：能使用string，就不使用cstring.
2
数据类型不同，头文件不同
cstring 和 string

1. c_str()：生成一个const char*指针，指向以空字符终止的数组。
①这个数组的数据是临时的，当有一个改变这些数据的成员函数被调用
后，其中的数据就会失效。因此要么现用先转换，要么把它的数据复
制到用户自己可以管理的内存中。
②c_str()返回一个客户程序可读不可改的指向字符数组的指针，不需要
手动释放或删除这个指针。
2. data():与c_str()类似，但返回的数组不以空字符终止。
3. size_t copy (char* s, size_t len, size_t pos = 0) const;
③从string类型对象中至多复制len个字符到字符指针s指向的空间中。
默认从首字符开始，但是也可以指定，开始的位置（记住从0开始）。
返回真正从对象中复制的字符。------用户要确保s指向的空间足够保
存len个字符。
String2cstring.cpp
3个函数：c_str(),data(),copy() 常用c_str
String到cstring的转换

#include <string>
#include <cstring> 
#include <iostream>
using namespace std;
/*
int main(){
// 不同的构造函数调用 
string s1;
string s2 = "Bravo";
string s3 = s2;
string s4( 10, 'x' );
cout << s3 << endl;
string fileName = "input.dat";
char fileName_cstr[100]; 
strcpy(fileName_cstr, fileName.c_str());
             //string类里提供的c_str成员函数，将字符串转换为C串，返回的是const char 
* 
cout << fileName_cstr << endl;
cout << fileName.length() << endl; //length成员函数求字符串的长度
} 
String到cstring的转换

5
String变量上的操作
输出：
字符串1的长度是：16
s4: Object-Oriented Programming
s1 += s2 : Object-Oriented Programming
s1.erase( 7, 9 ): Object-Programming
s1.insert( 7, s3 ):Object-Oriented Programming
s1.replace( 7, 9, "**" ): Object-**Programming
#include <string>
#include <iostream>
using namespace std;
int main(){
                string s1 = “Object-Oriented ”;    string s2 = "Programming";
string s3 = s1.substr( 7, 9 );  //substr成员函数用于提取子串 
string s4 = s1 + s2; 
// 在string中重载了operator + 用于把两个字符串拼接在一起，类似于c串的strcat操
作，但更直观好用 
//length 和 size 都可以用于求字符串的长度 
int len_str1, len_str2;
len_str1 = s1.length();   len_str2 = s2.size();
                   string outputStr;
                   outputStr = "字符串1的长度是：";
cout << outputStr << len_str1<< endl; 
cout <<"s4: "<< s4 << endl;
s1 += s2; //运算符重载
cout << "s1 += s2 : " << s1 << endl;
s1.erase( 7, 9 );   //删除从第7个位置处开始的9个字符
cout <<"s1.erase( 7, 9 ): " << s1 << endl;
s1.insert( 7, s3 );  //从第7个位置处插入字符串
cout <<"s1.insert( 7, s3 ):" << s1 << endl;
s1.replace( 7, 9, "**" );  //从第7个位置开始的9个字符替换为字符串
cout << "s1.replace( 7, 9, \"**\" ): " << s1 << endl;
return 0;}
Stringfuncs.cpp

6
StringFind.cpp
String变量上的操作：查找和比较
#include <string>
#include <iostream>
using namespace std;
int main(){
    string s1 = "Object-Oriented Programming";
    string s2 = "Programming";
    string s3 = "c++"; 
    int idx = s1.find( s2 );  //字符串搜索
    if( idx < s1.length() ) 
          cout << "Found at index: "<< idx << endl;
   else
         cout << "Not found" << endl;
    idx = s1.find(s3);
    if(idx == string::npos)//常量
          cout << "Not found" << endl;
     if( s1 > s2 ) //运算符重载，同样可以用== , <, !=等比较运算符
cout <<"\""+s1+"\""<< " is greater than "<<"\""+s2+"\""<<endl;
     else
cout <<"\""+s1+"\""<< " is not greater than "<<"\""+s2+"\""<<endl;
    return 0;
}
输出：
Found at index: 16
Not found
"Object-Oriented Programming" is not 
greater than "Programming"

•
inline 函数 

在函数申明或定义前加inline以表明函数是内联的

内联函数的得失
•
编译器将使用相应的函数代码替换函数调用
•
函数代码的多个拷贝插入到程序中（通常使程序变得更
大），如果程序在10个地方调用同一个内联函数，则该
程序将包含该函数代码的10个副本（见下一页的图示）
•
减少了函数调用的开销，特别是小函数的函数调用的开销

你可以要求函数是inline的，然而，由于各种原因，编译器
可能无法满足该要求（如函数过大，或注意到函数调用了自
己，内联函数不能递归）

内联函数通常都很小，经常使用（使用内联的规则）
7
inline内联函数

8
inline内联函数

9
Inline-square.cpp
inline内联函数
#include <iostream>
using namespace std;
inline double square(double x) {return x*x;}
int main() {
double a, b;
double c = 13.0;
a = square(5.0);
b = square(4.5 + 7.5);
cout <<"a = "<< a << ", b = "<< b << endl;
cout <<"c = "<< c ;
cout << ", c square = " << square(c++) 
<<endl;
cout << "Now c = "  << c << endl; 
return 0;
}
输出：
a = 25, b = 144
c = 13, c square = 169
Now c = 14

•
 相似之处

每个调用都用定义进行替换

减少函数调用开销，提高程序效率

如果扩展较大或有较多扩展，可执行映像会变得
相当大
•
不同之处

宏是由预处理器扩展，内联函数由编译器扩展。

宏展开做文本替换而不考虑代码的语义；但内联
函数的扩展考虑语义。

macro:没有类型安全检查
•
 与宏相比通常选择内联函数
10
inline内联函数与宏（macro)

11
macro-square.cpp
macro
//程序段1演示定义macro时常见的问题 
#include <iostream>
#define SQUARE(x)  x*x
using namespace std;
int main() {
double a, b;
double c = 13.0;
a = SQUARE(5.0); // 5.0*5.0
b = SQUARE(4.5 + 7.5); //4.5 + 7.5 * 4.5 + 7.5
cout <<"a = "<< a << ", b = "<< b << endl;
cout <<"c = "<< c ;
cout << ", c square = " << SQUARE(c++) 
<<endl; // c++*c++
cout << "Now c = "  << c << endl; 
return 0;
}
输出：
a = 25, b = 45.75
c = 13, c square = 182
Now c = 15
 

12
macro-square.cpp
macro
//上述部分问题的解决方法 
#include <iostream>
#define SQUARE(x)  ((x)*(x)) 
using namespace std;
int main() {
double a, b;
double c = 13.0;
a = SQUARE(5.0); // ((5.0)*(5.0))
b = SQUARE(4.5 + 7.5); //((4.5 + 7.5) * (4.5 + 7.5))
cout <<"a = "<< a << ", b = "<< b << endl;
cout <<"c = "<< c ;
cout << ", c square = " << SQUARE(c++) <<endl; 
// ((c++)*(c++))因为宏不能按值传递这一点仍然无法通过加
小括号来解决 
cout << "Now c = "  << c << endl; 
return 0;
}
输出：
a = 25, b = 144
c = 13, c square = 182
Now c = 15 

13
多语句的
宏.cpp
macro
//该程序段演示如何定义包含多行语句的宏 
#include <iostream>
#define ADD(a, b)                  \
    do{                             \
        int c = ((a) + (b)) ;        \
        cout << c;         \
    }while(0);
using namespace std;
int main()
{
    ADD(2, 5);
    return 0 ;
}
输出：
7
