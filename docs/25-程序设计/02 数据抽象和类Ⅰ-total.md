# 02 数据抽象和类Ⅰ-total

> 主题库：程序设计　｜　源文件：02 数据抽象和类Ⅰ-total.pdf　｜　16,524 字符　｜　50 个片段　｜　更新于 2026-11-11


数据抽象和类I 
中山大学计算机学院
主讲人：
XXX
中山大学MOOC课程
组

01
02
03
04
05
基本概念
结构体、类与对象
成员函数
Public、private与访问控
制
构造函数与析构函数
CONTENT
S
目 录
06
This关键字

1.1 研究背景
抽象
Data abstraction：只关心该数据“是什么”及“如何使用”，
而不关心它是如何运作的。
Control abstraction：只关心这个行为能够为我们带来什么，而
不关心这个行为的具体实现方法。 

1.1 研究背景
抽象数据类型
在程序设计中，对于被抽象的数据，称为抽象数据类型
（Abstract Data Type, ADT）。
一种ADT应具有
1   说明部分（说明该该ADT是什么及如何使用）：说明部分描
述数据值的特性和作用于这些数据之上的操作。ADT的用户
仅须明白这些说明，而无须知晓其内部实现。 
2   实现部分。

1.1 研究背景
抽象数据类型转化
把DATE设计为一种数据类型。

内部包含年月日等数据以及在这些数据上可进行的操作。

用户利用DATE就可以定义多个变量。

用户可调用每个变量中公开的操作，但无法直接访问每个变量
中被隐藏的内部数据。

用户也无需关心变量中各操作的具体实现。

于是DATE就是一种封装好的数据类型。这就达到了信息隐藏
和封装的目的。

1.1 研究背景
抽象数据类型大致结构-以日期为例
int year, month, day;
set()
get()
increment()
decrement()
Type
    DATE 
Data
   Each DATE value is date 
   in day/month/year
Operation
   Set the date
   Get the date
   Increment the date by one day
   Decrement the date by one day

1.1 研究背景
回顾C语言当中的结构体
C 数组允许定义可存储相同类型数据项的变量，结构是 C 编程中另
一种用户自定义的可用的数据类型，它允许您存储不同类型的数据项。
结构用于表示一条记录，我们继续以
上面的日期为例，我们可能会关心：
•
Year
•
Month
•
Day
但C语言中的 struct 只能包含变量，
不能包括实现ADT当中操作的函数！
struct Date { 
    int year; 
    int month; 
    int day ; 
} date; 
void Set( int, int, 
int ); 
int getMonth(); 
int getDay(); 
int getYear; 
void Print(); 
void Increment(); 
void Decrement(); 

1.1 研究背景
C++ 对象&类
C++ 在 C 语言的基础上增加了面向对象
编程，C++ 支持面向对象程序设计。类
是 C++ 的核心特性，通常被称为用户定
义的类型。
类用于指定对象的形式，它包含了数据表
示法和用于处理数据的方法。类中的数据
和方法称为类的成员。函数在一个类中被
称为类的成员。
定义一个类，本质上是定义一个数据类型
的蓝图。这实际上并没有定义任何数据，
但它定义了类的名称意味着什么，也就是
说，它定义了类的对象包括了什么，以及
可以在这个对象上执行哪些操作。

1.1 研究背景
Date类
Internal data
year
month
day
se
t
getYear
getMont
h
getDa
y
increment
decrement
Client code
类（class）
DATE

1.1 研究背景
类的例子
类定义是以关键字 class 开
头，后跟类的名称。类的主体
是包含在一对花括号中。类定
义后必须跟着一个分号或一个
声明列表。例如，我们使用关
键字  class 定义 Date 数据
类型，如右所示。
class DATE {
     public:
void Set( int, int, int );
int getMonth() const;
int getDay() const;
int getYear() const;
void Print() const;
void Increment();
void Decrement();
    private:
            int month;
            int day;
            int year;
};

1.1 研究背景
类的例子
在{}中列出类的成员。类的成员包括：

数据成员：一般说来，数据成员是需要隐藏的
对象；即外部的程序是不能直接访问这些数据
的，应该通过函数成员来访问这些数据。所以
一般情况下，数据成员通过关键字private声
明为私有成员（private member）

函数成员：通过关键字public声明为公有成
员（public member）。外部程序可以访问
共有成员，但无法访问私有成员。
对于类的使用者（即用户代码，简称用户）而言，只
需要获得DATE.h，即可调用类对象的公有函
数访问其内部的数据成员。使用者无法直接访
问私有成员，也无需知晓公有函数的内部实现。
class DATE {
     public:
void Set( int, int, int );
int getMonth() const;
int getDay() const;
int getYear() const;
void Print() const;
void Increment();
void Decrement();
    private:
            int month;
            int day;
            int year;
};

1.1 研究背景
结构体与类
C语言中的 struct 只能包含变量，而 C++ 中的 class 除
了可以包含变量，还可以包含函数。set() 是用来处理成员变量的
函数，在C语言中，我们将它放在了 struct Date 外面，它和成员
变量是分离的；而在 C++ 中，我们将它放在了 class Date 内部，
使它和成员变量聚集在一起，看起来更像一个整体。

1.1 研究背景
结构体、类与对象
结构体和类都可以看做一种由用户自己定义的
复杂数据类型，在C语言中可以通过结构体名
来定义变量，在 C++ 中可以通过类名来定义
变量。不同的是，通过结构体定义出来的变量
还是 叫变量，而通过类定义出来的变量有了新
的名称， 叫做对象（Object）。
在右 边代码中，我们先通过 class 关键字定义
了一个类 Date，然后又通过 Date 类创建了
一个对象 date。
class DATE {
     public:
void Set( int, int, int );
int getMonth() const;
int getDay() const;
int getYear() const;
void Print() const;
void Increment();
void Decrement();
    private:
            int month;
            int day;
            int year;
};
DATE date;

1.1 研究背景
成员函数
类的成员函数是指那些把定义和  原型写
在类定义内部的函数，就像类定义中的
其他变量一  样。类成员函数是类的一个
成员，它可以操作类的任意对象，可以
访问对象中的所有成员。
让我们看看之前定义的类 Date，现在
我们要使用成员函数来访问类的成员，
而不是直接访问这些类的成员。
class DATE {
     public:
void Set( int, int, int );
int getMonth() const;
int getDay() const;
int getYear() const;
void Print() const;
void Increment();
void Decrement();
    private:
            int month, day, year;
};

1.1 研究背景
成员函数
成员函数可以定义在类定义内部，
或者 单独 使用范围解析运算
符 :: 来定义。在类定义中定义的
成员函数   把函数声明为内联的，
即便没有使用 inline 标识符  。所
以您可以   按照 如下方式定义  Set( 
int Y, int M, int D) 函数。
class DATE { 
    public: 
        void Set(int newYear, 
                      int newMonth, 
                      int newDay ) { 
            month = newMonth; 
            day = newDay; 
            year = newYear; 
        } 
        ...... 
    private: 
        ...... 
}; 

1.1 研究背景
成员函数
也可以在类的外部使用范
围解析运算符 :: 定义该函
数，如右所示。
class DATE { 
    public: 
        void Set(int newYear, 
                      int newMonth, 
                      int newDay ); 
        ...... 
    private: 
        ...... 
}; 
void DATE::Set(int newYear, 
                         int newMonth, 
                         int newDay ) { 
    month = newMonth; 
    day = newDay; 
    year = newYear; 
} 

1.1 研究背景
DATE类中成员函数
#include "DATE.h"
#include <iostream>
using namespace std;
int DaysInMonth( int, 
int );  
void DATE::Set(int newYear, 
                           
int newMonth,
                           
int newDay )
{  }
int DATE::getMonth() const
{  }
int DATE::getDay() const
{  }
int DATE::getYear() const
{  }
void DATE::Print() const
{  }
void DATE::Increment()
{  }
void DATE::Decrement()
{  }
int DaysInMonth(  int mo,  int yr  )
{  }

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of each 
member //function of DATE.
void DATE::Set(int newYear, 
                           int newMonth,
                           int newDay ) {
    month = newMonth;
    day = newDay;
    year = newYear; 
}

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of each member function 
of //DATE.
int DATE::getMonth() const
{
    return month;
}
int DATE::getDay() const
{
    return day;
}
int DATE::getYear() const
{
    return year;
}

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of each 
member function of //DATE.
void DATE::Print() const
{
    switch (month)
    {
        case 1 : cout << "January";
                 break;
        case 2 : cout << "February";
                 break;
                        :
        case 12 : cout << "December";
    }
    cout << ' ' << day << ", " << year << 
endl << endl;
}

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of each member function 
of //DATE.
void DATE::Increment()
{
    day++;
    if (day > DaysInMonth(month, year))
    {
        day = 1;
        month++;
        if (month > 12)
        {
            month = 1;
            year++;
        }
    }
}

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of each member function of DATE.
void DATE::Decrement()
{  day--;
    if ( day == 0 )
    {
        if( month == 1 )
        { 
             day = 31;
             month = 12;
             year--;
        }
        else
        {
             month--;
             day = DaysInMonth( month, year );
        }
    }
}

1.1 研究背景
其他成员函数实现
//DATE.cpp  the implementation of the auxiliary function 
//DaysInMonth.
int DaysInMonth( /* in */ int mo, /* in */ int yr  )
{
    switch (mo)
   {
     case 1: case 3: case 5: case 7: case 8: case 10: case 12:
              return 31;
     case 4: case 6: case 9: case 11:
             return 30;
     case 2:  
          if ((yr % 4 == 0 && yr % 100 != 0) ||yr % 400 == 0)
              return 29;
          else
              return 28;
    }
}

1.1 研究背景
成员函数
在DATE.cpp文件开头需要加入预处理命令
                                #include “DATE.h”
        这是因为在DATE.cpp中要用到用户自定义的标识符DATE，而它的定义在DATE.h中。
在DATE.h中，各函数原型是在{}中的。根据标识符的作用域规则，它们的作用范围仅在类
定义中，而不包括DATE.cpp。因此在DATE.cpp中需要利用作用域解释运算
符“::”来指明这里的函数是类DATE里的成员函数。
DATE.cpp中有时还包括DATE内部要使用到的函数，例如DaysInMonth。这种函数并非对
外公开供用户使用，因此可以将其声明为类的私有成员。若在该函数中没有涉及该类
的数据成员，则无需将它们声明为类的成员。

1.1 研究背景
访问成员变量和成员函数
调用成员函数和成员变量是在对
象上使用   点运 算符（.），这  样
它就能操作与该对象相关的数据，
如下所示：
date.flag = 1;
date.set(2022,1,30);
class DATE { 
    public: 
        void Set(int newYear, 
                      int newMonth, 
                      int newDay ) { 
            month = newMonth; 
            day = newDay; 
            year = newYear; 
        } 
        int flag;
        ...... 
    private: 
        ...... 
}date; 

1.1 研究背景
访问成员变量和成员函数例子
//client.cpp
#include "DATE.h"
#include <iostream>
using namespace std;
int main()
{
     DATE date1, date2; //①
     int tmp;
     date1.Set( 1999, 10, 
1 );
     date1.Print();
     date1.Increment();
     date1.Print();
                         :
}
     date2.Set( 1997, 7, 1 );
     date2.Print();
     date2.Decrement();
     date2.Print();
     tmp = date1.getYear();
     tmp++;
     date1.Set( tmp, 12, 20 );
     date1.Print();
     cout << date1.year;  
                                   //
error?
     return 0;
}

1.1 研究背景
公有（public）成员
公有成员在程序中类的外部是可访问的。您可以不使用任何成员函数来设置和
获取公有变量的值，如下所示：
#include <iostream> 
using namespace std; 
class Line { 
    public: 
        double length; 
        void setLength( double len );     
        double getLength( void ); 
}; 
// 成员函数定义 
double Line::getLength(void) { 
    return length ; 
} 
void Line::setLength( double len ) { 
    length = len; 
} 

1.1 研究背景
公有（public）成员
公有成员在程序中类的外部是可访问的。您可以不使用任何成员函数来设置和
获取公有变量的值，如下所示：
// 程序的主函数 
int main( ) { 
    Line line; 
    // 设置长度 
    line.setLength(6.0); 
    cout << "Length of line : " << 
line.getLength() <<endl; 
    // 不使用成员函数设置长度 
    line.length = 10.0; 
    // OK: 因为 length 是公有的
    cout << "Length of line : " << 
line.length <<endl; 
    return 0; 
}
输出结果：
Length of line : 6 
Length of line : 10 

1.1 研究背景
私有（private）成员
私有成员变量或函数在类的外
部是不可访问的，甚至 是不可
查看的。只有类和友元 函数可
以访问私有成员。
默认 情况下，类的所有成员都
是私有的。例如在右面的类中，
width 是一个私有成员，这意
味着，如   果您没有使用任何访
问修饰符  ，类的成员将被假定
为私有成员。
class Box { 
    double width; 
    public: 
        double length; 
        void setWidth( double wid ); 
        double getWidth( void );
};

1.1 研究背景
私有（private）成员
实际操作中，我们一般会在私有区域定义数据，在公有区域 定义相关的函数，
以便在类的外部也可以调用这些函数，如下所示：
#include <iostream> 
using namespace std; 
class Box { 
    public: 
        double length; 
        void setWidth( double wid ); 
        double getWidth( void ); 
    private: 
        double width; 
}; 
// 成员函数定义 
double Box::getWidth(void) { 
    return width ; 
} 
void Box::setWidth( double wid ) { 
    width = wid; 
}

1.1 研究背景
私有（private）成员
实际操作中，我们一般会在私有区域定义数据，在公有区域 定义相关的函数，
以便在类的外部也可以调用这些函数，如下所示：
// 程序的主函数 
int main( ) { 
    Box box; 
    // 不使用成员函数设置长度 
    box.length = 10.0; 
    // OK: 因为 length 是公有的 
    cout << "Length of box : " << box.length <<endl; 
    // 不使用成员函数设置宽度 
    // box.width = 10.0; 
    // Error: 因为 width 是私有的 box.setWidth(10.0); 
    // 使用成员函数设置宽度 
    cout << "Width of box : " << box.getWidth() <<endl; 
    return 0; 
}
输出结果：
Length of box : 10 
Width of box : 10 

1.1 研究背景
构造函数
类的构造函数是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。
构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于
为某些成员变量设置初始值。下面的实例有助于更好地理解构造函数的概念：
#include <iostream> 
using namespace std; 
class Line { 
    public: 
        void setLength( double len ); 
        double getLength( void ); 
        Line(); // 这是构造函数 
    private: 
        double length; 
}; 
// 成员函数定义，包括构造函数 
Line::Line(void) { 
    cout << "Object is being created" << endl; 
} 
void Line::setLength( double len ) { 
    length = len; 
}

1.1 研究背景
构造函数
类的构造函数是类的一种特殊的成员函数，它会在每次创建类的新对象时执行。
构造函数的名称与类的名称是完全相同的，并且不会返回任何类型，也不会返回 void。构造函数可用于
为某些成员变量设置初始值。下面的实例有助于更好地理解构造函数的概念：
double Line::getLength( void ) { 
    return length; 
} 
// 程序的主函数 
int main( ) { 
    Line line; 
    // 设置长度 
    line.setLength(6.0); 
    cout << "Length of line : " << line.getLength() <<endl; 
    return 0; 
}
输出结果：
Object is being created 
Length of line : 6 

1.1 研究背景
带参数的构造函数
默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象
赋初始值，如下面的例子所示：
#include <iostream> 
using namespace std; 
class Line { 
    public: 
        void setLength( double len ); 
        double getLength( void ); 
        Line(double len); // 这是构造函数 
    private: 
        double length; 
}; 
// 成员函数定义，包括构造函数 
Line::Line(double len) { 
    cout << "Object is being created, length = " << len << endl; 
    length = len;
} 
void Line::setLength( double len ) { 
    length = len; 
}

1.1 研究背景
带参数的构造函数
默认的构造函数没有任何参数，但如果需要，构造函数也可以带有参数。这样在创建对象时就会给对象
赋初始值，如下面的例子所示：
double Line::getLength( void ) { 
    return length; 
} 
// 程序的主函数 
int main( ) { 
    Line line(10.0); 
    // 获取默认设置的长度 
    cout << "Length of line : " << line.getLength() <<endl; 
    // 再次设置长度 
    line.setLength(6.0); 
    cout << "Length of line : " << line.getLength() <<endl;    
    return 0; 
}
输出结果：
Object is being created, length = 10 
Length of line : 10 
Length of line : 6 

1.1 研究背景
构造函数例子-Date类
// DATE.h----类DATE的说明文件
class DATE
{
    public:
         DATE( int, int, int ); //构造函数
         DATE();              //缺省构造函数
         void Set( int, int, int);
         int getMonth() const;
         int getDay() const;
         int getYear() const;
         void Print() const;
         void Increment();
         void Decrement();
                        :
    private:
            int month;
            int day;
            int year;
};

1.1 研究背景
构造函数例子-Date类
//DATE.cpp ---- 类DATE中各成员函数的实现
//除增加了构造函数的定义外，其余代码与之前的DATE.cpp相同
                                                :
DATE::DATE( int initYear, int initMonth, int initDay )
{
     year = initYear;          //在构造函数中进行初始化
     month = initMonth;
     day  = initDay;
}
DATE::DATE()
{
     year = 2000;
     month = 1;
     day = 1;
}                                                 :

1.1 研究背景
构造函数例子-Date类
//client.cpp ---- 类DATE中各成员函数的实现
#include "DATE.h"
#include <iostream>
using namespace std;
int main()
{
     DATE date1;                //①自动调用缺省构造函数
     DATE  date2( 1976, 12, 20 ); //②自动调用另一个有参数的构造函数
     
     date1.print(); 
     date2.print(); 
    
     return 0; 
}

1.1 研究背景
默认构造函数
         如果设计的类没有构造函数，C++编译器会自动为该类型建立一个缺省构造函数。该构造
函数没有任何形参，且函数体为空。应该养成编写构造函数的习惯。
class DATE                                                                     
                              
{
    public:
         DATE( int = 2000, int = 1, int = 1 );  
                                      :
};
DATE::DATE( int initYear, int initMonth, int initDay )                        
    
{
     year = initYear;  month = initMonth;  day  = initDay;
}
int main() {                         //client.cpp 
     DATE  date1;                    //initYear, initMonth, initDay分别为2000，1，1
     DATE  date2( 1976 );            //initYear, initMonth, initDay分别为1976，1，1
     DATE  date2( 1976, 12 );        //initYear, initMonth, initDay分别为1976，12，1
     DATE  date2( 1976, 12, 20 );    //initYear, initMonth, initDay分别为1976，12，10
}

1.1 研究背景
析构函数
类的析构函数是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。
析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返
回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前
释放资源。下面的实例有助于更好地理解析构函数的概念：
#include <iostream> 
using namespace std; 
class Line { 
    public: 
        void setLength( double len ); 
        double getLength( void ); 
        Line(); // 这是构造函数声明 
        ~Line(); // 这是析构函数声明 
    private: 
        double length; 
}; 
// 成员函数定义，包括构造函数 
Line::Line(void) { 
    cout << "Object is being created" << endl; 
}

1.1 研究背景
析构函数
类的析构函数是类的一种特殊的成员函数，它会在每次删除所创建的对象时执行。
析构函数的名称与类的名称是完全相同的，只是在前面加了个波浪号（~）作为前缀，它不会返
回任何值，也不能带有任何参数。析构函数有助于在跳出程序（比如关闭文件、释放内存等）前
释放资源。下面的实例有助于更好地理解析构函数的概念：
Line::~Line(void) { 
    cout << "Object is being deleted" << endl; 
} 
void Line::setLength( double len ) { 
    length = len; 
} 
double Line::getLength( void ) { 
    return length; 
} 
// 程序的主函数 
int main( ) { 
    Line line; 
    // 设置长度 line.setLength(6.0); 
    cout << "Length of line : " << line.getLength() <<endl; 
    return 0; 
}
输出结果：
Object is being created 
Length of line : 6 
Object is being deleted 

1.1 研究背景
This指针
在 C++ 中，每一个对象都能通
过 this 指针来访问自己的地址。
this 指针是所有成员函数的隐含参数。
因此，在成员函数内部，它可以用来指
向调用对象。
友元函数没有 this 指针，因为友元不是
类的成员。只有成员函数才有 this 指针。
下面的实例有助于更好地理解 this 指针
的概念：
#include <iostream> 
using namespace std; 
class Box { 
    public: 
        // 构造函数定义 
        Box(double l=2.0, double b=2.0, double h=2.0) { 
            cout <<"Constructor called." << endl; 
            length = l; 
            breadth = b; 
            height = h; 
         } 
        double Volume() { 
            return length * breadth * height; 
         } 
         int compare(Box box) { 
             return this->Volume() > box.Volume(); 
         } 
     private: 
         double length; // Length of a box 
         double breadth; // Breadth of a box 
         double height; // Height of a box 
};

1.1 研究背景
This指针
在 C++ 中，每一个对象
都能通过 this 指针来访
问自己的地址。this 指
针是所有成员函数的隐含
参数。因此，在成员函数
内部，它可以用来指向调
用对象。
友元函数没有 this 指针，
因为友元不是类的成员。
只有成员函数才
有 this 指针。下面的实
例有助于更好地理解 this 
指针的概念：
int main(void) { 
    Box Box1(3.3, 1.2, 1.5); // Declare box1 
    Box Box2(8.5, 6.0, 2.0); // Declare box2 
    if(Box1.compare(Box2)) { 
        cout << "Box2 is smaller than Box1" <<endl; 
    } else { 
        cout << "Box2 is equal to or larger than Box1" <<endl; 
    } 
    return 0; 
}
输出结果：
Constructor called. 
Constructor called. 
Box2 is equal to or larger than Box1 

谢谢 
中山大学计算机学院
主讲人：
XXX
中山大学MOOC课程
组
