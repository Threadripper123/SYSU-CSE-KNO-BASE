# 01 From C to C++-第一周

> 主题库：程序设计　｜　源文件：01 From C to C++-第一周.pdf　｜　28,842 字符　｜　100 个片段　｜　更新于 2026-11-11


From C to C++
中山大学计算机学院
主讲人：陆勇中山大学MOOC课程
组

1.1 研究背景
From C to C++
C++作为一门面向对象的语言，具有面向对象编程的众多特点：
1、封装、继承和多态 
2、作为抽象数据类型的类 
3、易于调试和维护 
4、是软件开发的主流
......
而封装、继承和多态作为面向对象语言的三大特性会在后续课程中详细介绍。

1.1 研究背景
C++ 发展历史

Merges notions from Sumula 67 and notions from C

1979,1980,C with Classes, Bjarne Stroustrup at Bell Labs

1983,first C++ complier implemented

Keeps C's efficiency, flexibility and philosophy, while enjoying object-oriented programming 

1985, Cfront Release 1.0, The C++ programming language V1.0

1990, Cfront Release 3.0, The C++ programming language V2.0

1994, first draft of ANSI/ISO proposed standard

1997, final draft passed, The C++ programming language V3.0

1998, ANSI/ISO standard, ISO/IEC:98-14882

1.1 研究背景
C++ 发展历史

2011，C++11标准

            Boost库对该版本影响很大
    包括正则表达式（正则表达式详情）、
              完备的随机数生成函数库、
               新的时间相关函数，
               原子操作支持、
               标准线程库 
               新的for语法、
               auto关键字、
               新的容器类、更好的union支持、数组
初始化列表的支持以及变参模板的支持等

1.1 研究背景
C++ 发展历史
下面介绍一些C++中和C不同或是新增
的语法内容。

命名空间

流式IO操作( std::cin , std::cout )

引用

const 和 constexpr

Namespace 命名空间

Namespaces 命名空间用于防止名字冲突

C++标准程序库中的所有标识符都被定义于一个名为 
std 的命名字空间中。

命名空间中声明的标识符的作用域，从声明点到整个
命名空间，范围包括使用指令指定的命名空间范围。

命名空间
一个系统通常都不会仅由一个人来开发完成，不同的人开发同一个系统，
不可避免地会出现变量或函数的命名冲突，当所有人的代码测试通过，
没有问题时，将所有人的代码结合到一起，因为变量或函数重名而导致
的问题将会造成一定的混乱，例如：
    int flag = 1; //A声明的变量
    // ...... //若干行代码
    bool flag = true; //B声明的变量
但两个声明放到同一个函数中的时候，很明显编译器会提示出flag变量
重新定义的错误。这种问题若不加以处理是无法编译通过的。

命名空间
namespace name {
   // 代码声明
}
这个时候我们就可以用到命名空间来解决这个问题了。
命名空间的定义使用关键字 namespace，后跟命名空间的名
称，如下所示：
为了调用带有命名空间的函数或变量，需要在前面加上命
名空间的名称，如下所示：
name::code;  // code 可以是变量或函数

命名空间三种方式

使用由命名空间、域解析操作符::和标识符组成的限定名字

using 声明语句 

在局部或全局使用using 指令
std::cin >>  a;
using  std::cin;
cin >>  a;
using  namespace  std ; 
cin >>  a;

命名空间
namespace A
{ // A的变量声明
    int flag = 1;
}
namespace B
{ // B的变量声明
    bool flag = 0;
}
而我们就可以像上面这样声明flag，就可以解决前
面遇到的问题了。调用的时候用A::flag或者说
B::flag就可以了。 “::”操作符我们称之为域解析操
作符
    using A::flag;
    flag = 0; //使用A定义的变量flag
    B::flag = false; //使用B定义的变量flag
在代码的开头用using声明了A::flag，其含义是using声
明以后的程序中如果出现未指明的flag时，则使用
A::flag，但是若要使用B定义的flag，则仍需要
B::flag。
当然如果每次用到flag我们都要在flag前面加东西，
难免有些麻烦。
除了直接使用域解析操作符，还可以采用using声明
我们可以这样调用：

命名空间
    using namespace A;
    flag = 0; //使用A定义的变量flag
    B::flag = false; //使用B定义的变量flag
using声明不仅仅可以针对命名空间其中的一个变量，也可
以用于声明整个命名空间，例如：
如果命名空间 A 中还定义了其他的变量，则同样具有flag
变量的效果，在using声明后，若出现未具体指定命名空间
的命名冲突变量，则默认采用A命名空间中的变量。

命名空间
    using namespace A;
    flag = 0; //使用A定义的变量flag
    B::flag = false; //使用B定义的变量flag
using声明不仅仅可以针对命名空间中的一个变量，也可以
用于声明整个命名空间，例如：
现在回到前面我们的问题，using namespace std;的作用是什么？
C++标准库中的函数或者对象都是在命名空间std中定义的，所
以我们要使用标准函数库中的函数或对象都要使用std来限定。
一种写法是每次需要调用库函数的时候我们在前面加std::，
一种写法是我们在开头加上using namespace std;

命名空间

可以使用域解析操作符，引用隐藏的全局变量 ::
int x;
void f2( )
{
     int x = 1;   // hide global
     ::x = 2;      // assign to global
     x=2;  //assign to local
}
but, there is 
no way to use
 a hidden local 
name

C++ I/O

C++语言没有直接提供输入输出， I/O是标准库中的一
个类和实例。

C++ 标准输入输出头文件是 iostream

iostream 库 overloads(重载） 两个移位运算符<<, >>   
插入运算符<<，提取运算符>>

定义了三个标准流对象:  cout, cin, cerr

C++ 中的 "hello world" 
#include <iostream>
using namespace std;
int main()
{
cout<<"hello world!\n";
return 0;
}
#include <iostream.h>   //旧标准
int main() 
{
cout<<"hello world!\n";
return 0;
}

I/O

C++没有内建 I/O.

通过库提供的输入输出流实现 I/O.
executing
program
ostream
istream

<iostream>  头文件
定义了三个对象

An istream object named cin (keyboard)

An ostream object named cout (screen)

An ostream object named cerr (screen)

>> Operator
>> 输入或提取运算符
>> 二元操作符
>> 左结合
Expression   Has value
cin  >> agecin
Statement
cin  >>  age  >>  weight;

I/O
C++ 输入 输出 可以使用之前我们在 C 语言中的scanf、printf。也可
以使用C++中新增的流输入输出cin、cout。
来看看下面这个示例：
#include <iostream>      //EX1－6.cpp
using namespace std;
int main()
{
  int someInt;
  float someFloat;
  char someChar;
  cout << "the answer is: " << 3*4 << endl; 
  cin  >> someInt  >> someFloat >> someChar ; 
  return 0;
}
可以发现，我们输入输出不需要
像scanf一样用到格式化字符，
一定程度上来说简单了一些。

cout 输出的实质
cout << “the answer is: ” << 3*4 << endl;  
‘t’ ‘h’ ‘e’...’s’ ‘:’ ‘ ‘ ‘1’ ‘2’ ‘\n’
显
示
器
cout数据流
<<
“the anwser is: ”  12  ‘\n’
              cout << “the answer is: ”;
              cout << 3*4;
              cout << endl;  

cout << “the answer is: ” << 3*4 << endl;  
//该语句在屏幕上输出 the answer is 12
1.计算机对3*4求值得整数值12；
2. <<把字符‘t’、’h’ ... ‘s’、’:’、’ ‘放入cout流中；
3. <<把整数值12转化为字符‘1’和‘2’，也放入cout流中
4.  endl产生一个换行符，该字符也被放入cout流中
5.cout把这些字符送往显示器
‘t’ ‘h’ ‘e’...’s’ ‘:’ ‘ ‘ ‘1’ ‘2’ ‘\n’
显
示
器
cout数据流
<<
“the anwser is: ”  12  ‘\n’
cout 输出的实质

Extraction Operator(>>)

预定义的变量cin用来表示从标准输入设备（键盘）的
输入流

提取运算符 >>  读 “get from” ，需要2个操作数；
左操作数是一个流的表达式，如 cin；右操作数是简
单类型的变量

运算符> >试图从输入流中提取下一个项目，存入右操
作数的变量

cin  >> someInt  >> someFloat >> someChar ; 
1.键盘输入的字符一个一个进入输入流cin里面；
2. 一个>>代表一个输入过程。>>从cin中一个接一个获取字符，
   这个获取过程在哪里结束取决于变量的数据类型。该获取过
   程结束后，>>根据变量的数据类型，把刚才获得的字符序列
   转化成跟变量类型一致的数据；然后把这个数据赋给变量。
3. 下一个>>开始。
cin与输入的实质

memory
‘1’ ‘3’ ‘ ’ ‘3’ ‘.’ ‘1’ ‘4’ ‘ ’ ‘9’
>>
someInt
 ‘ ’ ‘3’ ‘.’ ‘1’ ‘4’ ‘ ’ ‘9’
>>
someFloat
 ‘ ’ ‘9’
>>
someChar
cin
功能：读取、转化
cin与输入的实质

格式控制 Manipulators

输入输出通过格式控制进行格式化.

Included <iostream> 
可直接使用的格式控制包括 (e.g., endl, flush[把buffer
中的数据送出.], dec, hex, oct, left, right, fixed 定点格
式, showpoint  etc. ) 

Included <iomanip>
Manipulators，需要参数的格式控制包括 (e.g., 
setw(n) , setprecision(n) 数字的位数, etc. )

Manipulators: Fixed and Showpoint

cout流的输出使用以下语句，使用十进制格
式（非科学记数法），包括一个小数点（即
使对于小数部分为0的浮点数）
cout  <<   fixed  <<   showpoint ;

Manipulators: setprecision(n)

输出语句中(<<)
头文件需要包含   #include <iomanip>

与fixed组合用（定点）控制输出流显示浮点数小数部分位
数
单独使用（无fixed ）则设置数值有效位数

直到另一项设置前该设置一直有效

What is exact output?
#include  <iomanip > // for setw( ) and setprecision( )
#include  <iostream>
using  namespace  std;
int main (  )
{
    float   myNumber  =  123.4587 ;
    cout  <<  fixed  <<   showpoint  ;        // use decimal format
                     // print decimal points
    cout  <<  "Number is "  <<  setprecision ( 3 )   
              <<  myNumber     <<  endl ;
    return  0 ;
}
Number is 123.459 
EX1_7.cpp

Manipulator: setw

设置输出字符占据的宽度
"set width" lets us control how many 
character positions the next data item should 
occupy when it is output 

只适用于数值、字符串，不适用于char变量
setw is only for formatting numbers and strings, 
not char type data 

setw(n)

用于输出运算符(<<).
头文件必须包含  #include <iomanip> 

参数n指出紧跟其后的输出项占用的字符位置个数，并
向右对齐。如果n太窄，字符所占位置个数会扩大。

setw 设置 仅对紧跟其后的一个输出项有效。一 旦按 指
定的宽度输出紧跟其后的输出项后，又回 到原来的   缺省
输出 方式。

What is exact output?
#include  <iomanip>          // for setw( )
#include  <iostream>
#include  <string>
using  namespace  std;
int  main (  )
{
    int  myNumber    =  123 ;
    int  yourNumber  =  5 ;
    cout  <<  setw ( 10 )     <<   "Mine"  
    <<  setw ( 10 )     <<  "Yours"           <<   endl
              <<  setw ( 10 )     <<  myNumber
    <<  setw ( 10 )     <<  yourNumber   <<   endl ;
    return 0 ;
}
EX1_8.cpp

OUTPUT
12345678901234567890
      Mine     Yours
       123         5
each is displayed right-justified and
each is located in a total of 10 positions
position

What is exact output?
#include  <iomanip>      // for setw( ) and setprecision( )
#include  <iostream>
using  namespace  std;
int  main (  )
{
    float myNumber    =  123.4 ;
    float yourNumber  =  3.14159 ;
    cout  <<  fixed  <<   showpoint  ; // use decimal format
           // print decimal points
    cout  <<  "Numbers are: "  <<  setprecision ( 4 ) <<  endl 
              <<   setw ( 10 )            <<  myNumber          <<   endl
    <<   setw ( 10 )            <<  yourNumber       <<   endl ;
    return 0 ;
}
EX1_9.cpp

OUTPUT 
Numbers are: 
  123.4000
    3.1416
each is displayed right-justified and
rounded if necessary and each is
located in a total of 10 positions with 
4 places after the decimal point
12345678901234567890

Manipulators
#include <iostream>
#include <iomanip>
using namespace std;
int main() 
{
int i;
for( i = 1; i < 1000; i *= 10 )
cout << setw(6) << i << endl;
for( i = 1; i < 1000; i *= 10 )
cout << i << endl;
int a = 5;
cout << left << setw(10) << "Karen"
 << right << setw(6) << a << endl;
double b = 1234.5;
cout << setprecision(2);
cout << setw(8) << b << endl;
return 0;
}
EX1_10.cpp

Manipulators
#include <iostream>
using namespace std;
int main() 
{
int i = 91;
cout << "i = " << i << "(decimal)\n";
cout << "i = " << oct << i << "(octal)\n";
cout << "i = " << hex << i <<"(hexadecimal)\n";
cout << "i = " << dec << i << "(decimal)\n";
return 0;
}
EX1_11.cpp

文件 I/O

读取和  写入（ 磁盘）文件：由一个输入文件相关的变
量代 替cin，一个输出文件相关联的变量代   替
cout。
Technique reading from and writing to (disk) files: to 
replace cin by a variable associated with an input file 
and to replace cout by a variable associated with an 
output file.

包含头文件  fstream
Include the header fstream to use files. 

文件 I/O

运算符>>用于输入，和cin的用法相   同；<<用于
输出，和cout的用法相   同。
The operator >> is used for input in the same way 
that is used with cin, and << is used for output in the 
same way that it is used with cout.

类ifstream的变量读取文件；类ofstream的变量  写
入文件
A variable of type ifstream to read from a file; A 
variable of type ofstream to write to a file

文件 I/O
To use disk I/O

Access #include <fstream>

选择有效的文件流标识符并声明
Choose valid identifiers for your filestreams and declare 
them

打开磁盘文件Open the files and associate them with disk 
names 

在输入、输入语句中使用文件流标识符Use your filestream 
identifiers in your I/O statements(using  >> and << , 
manipulators, get, ignore) 

Close the files 关闭文件

文件 I/O 语句
#include <fstream>
ifstream  myInfile;       // Declarations
ofstream  myOutfile; 
myInfile.open("myIn.dat"); // Open files
myOutfile.open("myOut.dat");
myInfile.close();  // Close files
myOutfile.close();

打开文件
Opening a file

Associates the C++ identifier for your file 
with the physical(disk) name for the file

如果输入文件不存在，则打开不成功
If the input file does not exist on disk, open is 
not successful 

若输出文件不存在，则建立文件；
If the output file does not exist on disk, a new 
file with that name is created

若输出文件已存在，则删除原文件的内容，使
其成为一个空文件。If the output file already 
exists, it is erased

文件流失败状态

当文件流进入失败状态
When a stream enters the fail state, 

使用该文件流的进一步操作都不   起效
Further I/O operations using that stream 
have no effect at all 

计算机不会自动停  下来，也不会给出任何错
误信息
The computer does not automatically halt the 
program or give any error message 

文件流失败状态

文件流  失败状态   可能的原   因
Possible reasons for entering fail state include

无效的输入数据
Invalid input data (often the wrong type) 

打开一个不存在的输入文件
Opening an input file that doesn‘t exist 

输出文件时磁盘已满 或文件是   写保护
Opening an output file on a disk that is  already full 
or is write-protected 

测试文件是否打开成功
#include <iostream>
#include <fstream>
using namespace std;
int main() 
{
ifstream inFile;
ofstream outFile;
int i;
int j;
inFile.open( "input.dat" );
if( !inFile ) {    //打开文件不成功，inFile为0
cerr <<"Unable to open input"<< endl;
exit(0);
}
}
EX1_12.cpp

#include <fstream>
using namespace std;
const int cutoff = 6000;
const float rate1 = 0.3;
const float rate2 = 0.6;
int main() 
{
ifstream infile;
ofstream outfile;
int income, tax;
infile.open( "income.in" );
outfile.open( "tax.out" );
while ( infile >> income ) 
{
if ( income < cutoff )
tax = rate1 * income;
else
tax = rate2 * income;
outfile << "Income = " << income 
<< " greenbacks\n"
<< "Tax = " << tax
<< " greenbacks\n";
}
infile.close();
outfile.close();
return 0;
}
EX1_13.cpp

类型转换

static_cast

相关的类型之间的转换：类层次 结构中基类和 派生类
之间指 针或引用的转换；基本数据类型之间的转换，
如把int转换成char，把enum转换成int；浮点型
指针转换为整数型（reinterpret_cast）。
average = (float) hits / (float) at_bats;
average = static_cast<float>(hits) / static_cast<float>(at_bats);

类型转换

static_cast

用于把数据类型从一种转换为另一种，做为显式
类型转换使用
Used to convert one data type to another and 
hands all reasonable casts
average = (float) hits / (float) at_bats;
average = static_cast<float>(hits) / static_cast<float>(at_bats);

类型转换

const_cast

Used to cast away constness.转换 常量
#include <iostream>
using namespace std;
int main()
{
const int i = 100;
const int *p = &i;
int *q = const_cast<int*>(p);
int j = i;
cout << i << endl << j << 
endl << *p << endl << *q <<endl;
return 0;
}
EX1_14.cpp
常量指针被
转化成 非常
量的指 针，
并且仍然指
向原来的对
象。可以通
过该指 针对
它的数据成
员任意改 变。

类型转换

reinterpret_cast
强制类型转换符。两种不同类型 之间转换，把一个整数转
换成一个指针，也可以把一个指针转换成另一个指针。
converts between unrelated types such as an integer to a 
pointer or  a pointer to an unrelated pointer type.

用于把一 种类型指  针转换成另一种类型指 针
Used to convert a pointer of one type to a pointer of another 
type.

必须 谨慎使用。
只进行一 种简单的二进制位复制，不进行二进制数据分析。
Implementation dependent, must be used with caution.

类型转换
void* and  reinterpret_cast
int *ip;
char *cp;
void *gp;       //无类型指针，可以指向任何数据类型。
cp=ip;   
ip=cp;
cp=gp;
gp=cp;
cp=reinterpret_cast<char*>(gp);
//illegal C++ but legal C
//illegal C++ but legal C
//illegal C++ but legal C
//legal C++ 

类型转换

dynamic_cast

支持继承关系交叉转换Used for 
casting across or within inheritance.

常用于虚函数的类进行下行转换，
dynamic_cast具有类型检查的功能，
比static_cast更安全。
This cast is used with classes having 
virtual functions. 

1.1 研究背景
引用
引用C++对C语言的重要扩充。引用就是某一变量（目
标）的一个别名，对引用的操作与对变量直接操作完全一样。
引用的声明方法：类型标识符 &引用名=目标变量名；
引用引入了对象的一个同义词。定义引用的表示方法与定义
指针 相似 ，只是用&代替了*。
下面我们看几个引用的简单示例。

1.1 研究背景
引用
例1 利用引用为变量再起 名称
int main() //程序Ref1
{
    int a = 1024;
    int *p  = &a; // p是指针; &a是a的地址
    int &x = a;    // x是引用，它实际上与a是同一个变
量
    cout << "a = " << a << endl;
    cout << "x = " << x << endl;
    cout << " *p = " << *p << endl;
    x = 2000;
    cout << "a = " << a << endl;
     return 0;
}

1.1 研究背景
引用
例2：在 形参中使用引用
//程序RefSwap
void swap(int &x, int &y)
{
    int temp;
    temp = x;
    x = y;
    y = temp;
}
int main()
{
    int a, b;
    cin >> a >> b;
    swap(a, b);
    cout << "max = " << a<< " "<< "min = " << b;
}
x
a
....
y
b
....

1.1 研究背景
引用
假如形参不使用引用
//程序RefSwap
void swap(int x, int y)
{
    int temp;
    temp = x;
    x = y;
    y = temp;
}
int main()
{
    int a, b;
    cin >> a >> b;
    swap(a, b);
    cout << "max = " << a<< " "<< "min = " << b;
}
x
a
....
y
b
....

1.1 研究背景
引用
与引用相   比，这个指   针例子 又如何？
void swap(int *p1, int *p2)
{
    int temp;
    temp = *p1;
    *p1 = *p2;
    *p2 = temp;
}
int main()
{
    int a = 5, b = 9;
    int *p1, *p2;
    p1 = &a;
    p2 = &b;
    swap(p1, p2);
    cout << "a = " << a << " "<< "b = " << b;
}

&a
main的p1
&a
swap的p1
5
a
&b
main的p2
&b
swap的p2
9
b
&a
main的p1
&a
swap的p1
9
a
&b
main的p2
&b
swap的p2
5
b
&a
main的p1
9
a
&b
main的p2
5
b
在main()里面
调用swap()时
实参值传递给
形参
在swap()里面
返回到main()里面
&a
main的p1
5
a
&b
main的p2
9
b

•
相同：可以使一个函数向调用者返回多个数值。
•
不同：原理不同。引用传递中，形参、实参实质为同一变
量，或者说是为某个变量起多了一个名字。而使用指针作
函数参数，则是使被调用函数获得某变量的地址，从而使
用这个地址访问这个变量。
•
从返回值的角度看，引用形参比利用指针方便。
指针与引用 
引用

例3 返回 值不使用引用
double f(  double x )     
{
    double y;
    y = sin(x);
    return y;
}
int  main()
{
    double a = 3.14/6;
    double y;
    y = f( a );
    cout <<  "y = " <<  y <<  endl;
    return 0;
} 
0.4990.499
不使用引用
f中的y
main中的y
0.499
临时变量
引用

例3 返回 值使用引用
double& f(  double 
x )     //程序ref2
{
    static double y;
    y = sin(x);
    return y;
}
int  main()
{
    double a = 3.14/6;
    double y;
    y = f( a );
    cout <<  "y = " <<  y <<  endl;
    return 0;
} 
0.4990.499
使用引用
f中的y
main中的y
引用

例3 返回 值使用引用
double& f(  double x )    
{
    double y;
    y = sin(x);
    return y;
}
int  main()
{
    double a = 3.14/6;
    double y;
    y = f( a );
    cout <<  "y = " <<  y <<  endl;
    return 0;
} 
0.4990.499
使用引用
f中的y
main中的y
引用

1.1 研究背景
const和constexpr
想必 大家对const这个关键字并不陌生，
而constexpr 是 C++11 标准新 添加的关
键字，在 此之前（C++ 98/03标准） 只有 
const 关键字，其在实际 使用中  经常会 表
现出两种不同的语义。
举个例子，在右边 的示例代码中，dis_1() 
和 dis_2() 函数中都包含一个 const int 
x，但 dis_1() 函数中的 x 无法完成 初始
化 array 容器的 任务 ，而 dis_2() 函数中
的 x 却可以。
#include <iostream>
#include <array>
using namespace std;
void dis_1(const int x){
    //错误，x是只读 的变量
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
void dis_2(){
    const int x = 5;
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
int main()
{
   dis_1(5);
   dis_2();
}

1.1 研究背景
const和constexpr
这是因为，dis_1() 函数中的“const int x”只
是想强调   x 是一个只读的变量，其本质仍 为变量，
无法用来初始化 array 容器；  而 dis_2() 函数
中的“const int x”，表明 x 是一个只读变量
的同时，x 还是一个值为 5 的常量，所以可以用
来初始化 array 容器。
C++ 11标准中，为了解决 const 关键字的双重
语义问题 ，保留 了 const 表示“只读”的语义，
而将“常量”的语义  划分给了新添加 的 
constexpr 关键字。因此  C++11 标准中，建
议将 const 和 constexpr 的功能  区分开，  即
凡是表达“只读”语义的  场景 都使用 const，表
达“常量”语义的  场景 都使用 constexpr。
#include <iostream>
#include <array>
using namespace std;
void dis_1(const int x){
    //错误，x是只读 的变量
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
void dis_2(){
    const int x = 5;
    array <int,x> myarr{1,2,3,4,5};
    cout << myarr[1] << endl;
}
int main()
{
   dis_1(5);
   dis_2();
}

1.1 研究背景
auto
auto的原理就是  根据后面的 值，自行在编译   期间推测前
面的类型是什么。auto的作用就是为了简化变量初始化，如
果这个变量有一个比较长  的初始 化类型，就可以用auto代替。
注意点：
1.用auto声明的变量必须初始化
2.函数和  模板参  数不 能被 声明为auto
3.因为auto是一个  占位 符， 并不是一个他自己 的类型，因此不能用于类型
转换或其他一些操作，如sizeof和typeid
4.定义在一个auto序列的变量  必须始终推导成同一类型
auto x1 = 5,x2 = 5.0,x3='r';  //这样写是不合法的，因为各个变量类型不同

1.1 研究背景
auto
比如下面的代码：
vector<int> a;
vector<int>::iterator it = a.begin();
当然定义it对应的变量类型时，名字有点长。
我们在初始化it的时候可以这  样写:
auto    it = ve.begin();
而在 遍历迭  代器的时候我们也可以用auto简化代码：
int main(){
    vector<int>v;
    v.push_back(1);
    v.push_back(2);
    v.push_back(3);
    for(auto i : v){
        cout<<i<<" ";
    }
    cout<<endl;
    return 0;
}

1.1 研究背景
布尔数据类型 bool

C++新内置了   布尔 数据类型 : bool.
bool flag1,flag2;
flag1 = ( 3 < 5 );
flag2 = ( 5%3 == 0 );
cout << flag1<< endl;
cout << flag2<< endl;
1
0
显示结果
EX1_15.cpp

1.1 研究背景
枚举型 Enumeration
 

枚举类型一  旦被定义，当一个整型使用
enum maritalStatus { single, maried };
maritalStatus m;
m = single;
int sum = 0;
if( m == single ) sum++;
enum { MIN_SIZE = 0, MAX_SIZE = 100 };
int minVal = MIN_SIZE;
int arr[MAX_SIZE];
EX1_16.cpp

1.1 研究背景
变量申明

C语言中，变量申明必须要在函数开头部分。

C++中， 允许 变量定义语句在程序中的任何地方   ，只要
在是使用   它之 前定义过   就可以；
而且C++允许 重复定义变量，C语言不  允许 重复定义
变量。

1.1 研究背景
结构体

结构体 无需放在声明部分
struct need not be included as part of the variable 
declaration. 
struct Point {
double x, y;
};
Point p;
p.x = 2.0;
p.y = 1.0;
cout <<" ("<< p.x <<", "<< p.y << " ) "<< endl;
EX1_17.cpp

1.1 研究背景
结构体

In C++, a struct can contain functions.
结构体 能够包含函数
struct Point {
double x, y;
void setVal(double, double);
};
Point p;
p.x = 3.1415926;
p.y = 1.0;
p.setVal(4.11, -13.090);
EX1_18.cpp

1.1 研究背景
字符串

An alternative to C's null-terminated arrays of char.

Use of type string requires the header string  字符串
#include <string>
using namespace std;
string s1;
string s2 = "Bravo";
string s3 = s2;
string s4( 10, 'x' );
cout << s3 << endl;
string fileName = "input.dat";
ifstream inFile;
inFile.open( fileName.c_str() ); 
cout << fileName.length() << endl;
c_str()将 
fileName转化为字
符串型
EX1_19.cpp

1.1 研究背景
字符串操作
string s1 = "Object-Oriented ";
string s2 = "Programming";
string s3 = s1.substr( 7, 9 );  //字符串提取
string s4 = s1 + s2;
cout << s4 << endl;
s1 += s2;
cout << s1 << endl;
s1.erase( 7, 9 );   //删除字符串
cout << s1 << endl;
s1.insert( 7, s3 );  //插入字符串
cout << s1 << endl;
s1.replace( 7, 9, "**" );  //替换字符串
cout << s1 << endl;
EX1_20.cpp

1.1 研究背景
字符串操作
int idx = s1.find( s2 );  //字符串搜索
if( idx < s1.length() ) 
cout << "Found at index: "<< idx << endl;
else
cout << "Not found" << endl;
if( s1 > s2 )
cout <<"\""+s1+"\""<<
" is greater than "<<"\""+s2+"\""<<endl;
else
cout <<"\""+s1+"\""<<
" is not greater than "<<"\""+s2+"\""<<endl;
EX1_21.cpp

1.1 研究背景
内联函数 Inline Functions

Inline functions 

减少特别是小函数的函数调用的开  销Reduce function call 
overhead—especially for small functions

函数返回 类型 加 inline
Qualifier inline before a function's return type in the function 
definition 

Trade-off of inline functions  比较内联函数

函数代  码的多个拷贝 插入到程序中（通常使程序更大）

1.1 研究背景
内联函数 Inline Functions
#include <iostream>
using namespace std;
inline void swap(int&, int&);
int main() {
int i=7, j=-3;
swap(i,j);
cout <<"i = "<< i << endl  <<"j = "<< j << endl;
return 0;
}
void swap(int& a, int& b) {
int t;
t = a;
a = b;
b = t;
}
EX1_30.cpp

1.1 研究背景
内联函数 Inline Functions

Inline function: each occurrence of a call of the 
function should be replaced with the code that 
implements the function.
内联函数：函数调用时用函数体代码替 换调用语句。

However, the compiler, for various reasons, may not 
be able to honor the request.
然而，由于  各种原因，编译器可能无法兑现的要求

inline functions are usually small, frequently-used 
functions.  内联函数通  常都很小，经常使用

1.1 研究背景
内联函数 Inline Functions

Using inline functions can reduce
execution time but may increase program size.
内联函数能减少程序执行时间但会程序代码会加长

1.1 研究背景
内联函数 Inline Functions

The inline qualifier should be used
only with small, frequently used functions.
内联函数通  常用于都很小，经常 使用的函数

1.1 研究背景
Inline Function V.S. Macro

Similarities 相似之处

Each occurrence is replaced with the definition.
调用时，使用定义替换

The overhead of a function call is avoided so that 
the program may execute more efficiently.
减少调  用开 销，提 高程序效 率

The size of the executable image can become quite 
large if the expansions are large or there are many 
expansions.
如果扩 展较 大或有  较多扩展，可执行映像 会变得相当大

1.1 研究背景
Inline Function V.S. Macro

Dissimilarities 不同之处

A macro is expanded by the preprocessor, an inline 
function is expanded by the compiler.
宏是由预  处理 器扩 展，内 联函数由  编译 器扩 展。

Macro expansions do text substitution without regard to 
the semantics of the code; but inline function 
expansions take into account the semantics.
宏展开做文本 替换而不考虑 代码的语义；但内联函数的扩展考虑  语义。

Macro:No type-safety checking.没有类型安全检查

Inline functions are generally preferable to macros.
与宏相比通常选择  内联函数

1.1 研究背景
缺省参数

Default argument 缺省 参数

A default value to be passed to a parameter 传递缺
省值

Used when the function call does not specify an 
argument for that parameter 函数调用无需  传特 定值

Must be the rightmost argument(s) in a function's 
parameter list  必须位于参数列表的最右边

Should be specified with the first occurrence of the 
function name 应在第一次出现函数名时指定

Typically the function prototype 通常在函数原型中设定

1.1 研究背景
缺省参数
// function prototype that specifies default arguments
int boxVolume( int length = 1, int width = 1, int height = 1 );
int main() {
// no arguments--use default values for all dimensions
cout << "The default box volume is: " << boxVolume();
// specify length; default width and height
cout << "\n\nThe volume of a box with length 10,\n"
     << "width 1 and height 1 is: " << boxVolume( 10 );
// specify length and width; default height
cout << "\n\nThe volume of a box with length 10,\n"
     << "width 5 and height 1 is: " << boxVolume( 10, 5 );
// specify all arguments
cout << "\n\nThe volume of a box with length 10,\n"
     << "width 5 and height 2 is: " << boxVolume( 10, 5, 2 )  << endl;
}
EX1_31.cpp
Default arguments
Calling function with no arguments
Calling function with one argument
Calling function with two arguments
Calling function with three arguments

1.1 研究背景
缺省参数
// function boxVolume calculates the volume of a box
int boxVolume( int length, int width, int height ) {
return length * width * height;
} 
The default box volume is: 1
The volume of a box with length 10,
width 1 and height 1 is: 10
The volume of a box with length 10,
width 5 and height 1 is: 50
The volume of a box with length 10,
width 5 and height 2 is: 100
EX1_31.cpp

1.1 研究背景
缺省参数
#include <string>
using namespace std;
void fo( int val,   float f = 12.6,char c = '\n', string msg = "Error" ) 
{
return;
}
int main() 
{
fo( 14, 48.3f, '\t', "OK" );
fo( 14, 48.3f, '\t' );
fo( 14, 48.3f );
fo( 14 );
return 0;
}
EX1_32.cpp

1.1 研究背景
缺省参数
//***** ERROR: Invalid mix of default 
// and nondefault values ***
void g( int val = 0, float s, char t = '\n', string msg = "error" );
Default arguments may 
only be provided for 
trailing arguments only
只能放最后

1.1 研究背景
函数重载

Overloaded functions 函数重载

Overloaded functions have

Same name 相同的函数名

Different sets of parameters 不同的参数  集

Commonly used to create several functions of 
the same name that perform similar tasks, but 
on different data types
常用于 创建的名  称相同，执行类 似任务  的多
个函数，   但对应不同的数据类型

1.1 研究背景
函数重载
#include <iostream>
using namespace std;
int square( int x ) {  //for int values
cout << "square of integer " << x << " is ";
return  x * x;
} 
double square( double y ) {  //for double values
cout << "square of double " << y << " is ";
return  y * y;
} 
int main() {
cout << square( 7 ) << endl; // calls int version
cout << square( 7.5 ) << endl; // calls double version
return 0;
}
87
EX1_33.cpp
Defining a square function for ints
Defining a square function for doubles
Output confirms that the proper 
function was called in each case

1.1 研究背景
函数重载

How the compiler differentiates overloaded 
functions编译 器如 何区 分重载函数 

Overloaded functions are distinguished by their 
signatures 重载函数是由其特征 的区别

Signature is a combination of a function's name and its 
parameter types (in order) 特征包含函数名和参数类型

Name mangling or name decoration 
（名字重整或名字修饰）

Compiler encodes each function identifier with the number 
and types of its parameters to enable type-safe linkage
编译 器通过函数参数的数量和类型来区别

1.1 研究背景
函数重载
// Name mangling.
int square( int x ) {// function square for int values
return x * x;
}
double square( double y ) {// function square for double values
return y * y;
} 
// function that receives arguments of types
// int, float, char and int &
void nothing1( int a, float b, char c, int &d ) {
// empty function body
} 
EX1_34.cpp
Overloaded square functions

1.1 研究背景
函数重载
// function that receives arguments of types
// char, int, float & and double &
int nothing2( char a, int b, float &c, double &d ) {
return 0;
} 
int main() {
return 0;
} 
EX1_34.cpp
@square$qi
@square$qd
@nothing1$qifcri
@nothing2$qcirfrd
_main
main is not mangled because it cannot be overloaded
Mangled names of overloaded functions

1.1 研究背景
函数重载
#include <iostream>
#include <iomanip>
using namespace std;
void print(int a);
void print(double a);
int main() 
{
int x = 8;
double y = 8;
print(x);
print(y);
return 0;
}
void print(int a) 
{
cout << a << endl;
}
void print(double a) 
{
cout << showpoint << a << endl;
}
EX1_35.cpp

1.1 研究背景
函数重载
void print(int);
void print(const char*);
void print(double);
void print(long);
void print(char);
void h(char c, int i, short s, float f)
{
print(c);// exact match: invoke print(char)
print(i);// exact match: invoke print(int)
print(s);// integral promotion: invoke print(int)
print(f);// float to double promotion: print(double)
print('a');    // exact match: invoke print(char)
print(49);     // exact match: invoke print(int)
print(0);       // exact match: invoke print(int)
print("a");  // exact match: invoke print(const char*)
}

1.1 研究背景
函数重载
int pow(int, int);
double pow(double, double);
complex pow(double, complex);
complex pow(complex, int);
complex pow(complex, double);
complex pow(complex, complex);
void k(complex z)
{
int i = pow(2,2);// invoke pow(int,int)
double d = pow(2.0,2.0);// invoke pow(double,double)
complex z2 = pow(2,z);// invoke pow(double,complex)
complex z3 = pow(z,2);// invoke pow(complex,int)
complex z4 = pow(z,z);// invoke pow(complex,complex)
double d = pow(2.0,2);// error: pow(int(2.0),2)  or 
//pow(2.0,double(2))?
}

1.1 研究背景
函数特征

Overloaded functions must have distinct signatures.

A function‘s signature consists of     函数 特征

Function name 函数名

The number, data types, and order of arguments
个数、类型、顺序

Functions can not be distinguished by return types alone.   
函数不能仅通过返回 类型 区分。

Examples:
void m(double, int);
void m(int, double);
double m(int, double);

1.1 研究背景
The new And delete Operators

The new and delete operator are available to 
manipulate free store(heap). 操作空的堆内存

Free store is a system-provided memory pool for 
objects whose lifetime is directly managed by the 
programmer.空的堆内存是由系统 提供的，程序直接管理 的
对象的生命周期的内存  池

The programmer creates the object by using new 
and destroys it by using delete.
程序使用new申请创  建的对象，通过delete释放。

1.1 研究背景
The new And delete Operators
int* p;
int* q;
p=new int(5);//allocation and initialization
q=new int[10]; //gets q[0] to q[9] with q=&q[0] 
delete p;
delete []q;
•
An appropriate amount of store is allocated from 
free store to contain the named type.
从内存空间分配适 合的容量存储 指定的类型数据
•
The base address of the object is returned as the 
value of the new expression.  new表达式返回首地址
•
The operator new either returns the value 0 or 
throws an appropriate exception when memory is  
unavailable. 当内存不可用 返回值0或抛出异常

1.1 研究背景
The new And delete Operators

The operator delete destroys an object created by 
new.
用delete进行 释放 new建立的对象

In effect returning its allocated storage to free store 
for reuse.可以把  先前占用的内存空间释放。
int* p;
int* q;
p=new int(5);//allocation and initialization
q=new int[10]; //gets q[0] to q[9] with q=&q[0] 
delete p;
delete []q;//the original allocation was an array of objects.

1.1 研究背景
The new And delete Operators
int* p;
int* q;
p = new int;
q = new int;
*p = 40;
*q = *p;
q = p;
int* arr = new int [3];
arr[0] = 3;
arr[1] = *p;
arr[2] = 4;
delete q;
delete p;// Error!
delete [] arr;
p
q
arr
Stack
Heap
98

谢谢
中山大学计算机学院
主讲人：陆勇中山大学MOOC课程
组
