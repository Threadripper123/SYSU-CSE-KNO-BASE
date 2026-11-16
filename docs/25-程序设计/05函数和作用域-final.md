# 05函数和作用域-final

> 主题库：程序设计　｜　源文件：05函数和作用域-final.pdf　｜　21,364 字符　｜　67 个片段　｜　更新于 2026-11-11


函数和作用域
中山大学计算机学院
主讲人：陈武辉、周晓聪
中山大学MOOC课程
组

01
02
03
04
05
函数是什么
函数的声明与定义
函数的调用
变量的作用域
递归函数
CONTENT
S
目 录
06
总结
为什么需要函数？
•
函数的设计
•
函数的实现
•
函数的使用

程序员为什么需要函数？
一个三位数称为水仙花数
(narcissus number)，如果它等于
它的每位数字的立方和
例如，153 = 1
3
+5
3
+3
3
是水仙花
数
编写一个程序，输出所有的
三位数水仙花数
一个n位数称为水仙花数，如果
它等于它的每位数字n次方的和
编写一个程序，输入n，
输出所有的n位数水仙花
数
扩展

程序员为什么需要函数？
编写一个程序，输出所有的
三位数水仙花数（它等于它
的每位数字的立方和）
对100到999的每个数num：
1.求出num的百位a、十位b、个
位c
2.判断a的3次方加b的3次方加c
的3次方的和是否等于num
朴素想法
对100到999的每个数num：
1.求出num的百位a、十位b、个
位c
•
a=num/100
•
b=(num/10)%10 
•
c=num%10
2.判断a的3次方加b的3次方加c
的3次方的和是否等于num
#include<stdio.h> 
int main() {
    for (int num = 100; num < 1000; num++) {
        // 求出num的百位a、十位b、个位c
        int a = num / 100;
        int b = (num / 10) % 10;
        int c = num % 10;
        int sum = a*a*a + b*b*b + c*c*c;
        if (sum == num) {
            printf("%d is a narcissus number!
\n", 
                   num);
        }
    }
}

程序员为什么需要函数？
编写一个程序，输出所有的
三位数水仙花数（它等于它
的每位数字的立方和）
对100到999的每个数num：
1.求出num的百位a、十位b、个
位c
•
a=num/100
•
b=(num/10)%10 
•
c=num%10
2.判断a的3次方加b的3次方加c
的3次方的和是否等于num
朴素想法
如何扩展？
100  n位数的最小整数
999  n位数的最大整数
对n位数的最小整数到最大整数的每个数num
1.求出num的每一位数字的n次方和sum
2.判断num是否等于sum，若是输出num
编写一个程序，输入n，
输出所有的n位数水仙花
数
扩展

程序员为什么需要函数？
编写一个程序，输出所有的
三位数水仙花数（它等于它
的每位数字的立方和）
对100到999的每个数num：
1.求出num的百位a、十位b、个
位c
•
a=num/100
•
b=(num/10)%10 
•
c=num%10
2.判断a的3次方加b的3次方加c
的3次方的和是否等于num
朴素想法
扩展
计算n位数的最小整数start;
计算n位数的最大整数end;
对start到end的每个数num:
1.初始化各位数字和sum为零
2.对于i从1到n
•
求出num的第i位数字digit
•
计算digit的n次方digit_power
•
将digit_power加到各位数字和
sum
3.判断num是否等于sum，若是输出
num
自然而然希望使用函数实现下面的功能：
•
计算n位数的最小整数
•
计算n位数的最大整数
•
计算num的第i位数字
•
计算一个数字的n次方

程序员为什么需要函数？
程序员编写程序实现稍微复杂的功能时，从直观想法开始，对要实现的功能进行细化
•
这个细化的过程是对稍微复杂功能的分解过程
在对功能分解到某个层次时，希望有些子功能推迟到后面再细化
•
从而自然而然地引入函数来代表以后要实现这些子功能的代码段
•
推迟细化使得功能分解过程可以在不同的层次注意不同的细节
总之，引入函数是程序员在对要实现的功能进行分解、细化和分层时的需要

函数的设计、使用与实现
希望使用函数实现下面的功能：
•
计算n位数的最小整数
•
计算n位数的最大整数
•
计算num的第i位数字
•
计算一个数字的n次方
对函数进行设计：
•
确定函数名字
•
确定函数参数
•
确定函数返回值
设计
1. 计算n位数的最小整数：
•
函数名：get_minimal_number()
•
参数：整数n
•
返回值：n位数的最小整数
2. 计算n位数的最大整数：
•
函数名：get_maximal_number()
•
参数：整数n
•
返回值：n位数的最大整数
3. 计算num的第i位数字：
•
函数名：get_digit()
•
参数：整数num；整数i：表示取第i
位
•
返回值：n位数num的第i位十进制
数字
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方

函数的设计、使用与实现
1. 计算n位数的最小整数：
•
函数名：get_minimal_number()
•
参数：整数n
•
返回值：n位数的最小整数
2. 计算n位数的最大整数：
•
函数名：get_maximal_number()
•
参数：整数n
•
返回值：n位数的最大整数
3. 计算num的第i位数字：
•
函数名：get_digit()
•
参数：整数num；整数i：表示取第i
位
•
返回值：n位数num的第i位十进制
数字
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方
start = get_minimal_number(n);
end = get_maximal_number(n);
对start到end的每个数num:
1.sum = 0
2.对于i从1到n
•
digit = get_digit(num, i);
•
digit_power = get_power(digit, n);
•
sum = sum + digit_power
3.判断num是否等于sum，若是输出num
使用函数
计算n位数的最小整数start;
计算n位数的最大整数end;
对start到end的每个数num:
1.初始化各位数字和sum为零
2.对于i从1到n
•
求出num的第i位数字digit
•
计算digit的n次方digit_power
•
将digit_power加到各位数字和sum
3.判断num是否等于sum，若是输出num

函数的设计、使用与实现
1. 计算n位数的最小整数：
•
函数名：get_minimal_number()
•
参数：整数n
•
返回值：n位数的最小整数
•
返回10的n-1次方的值
2. 计算n位数的最大整数：
•
函数名：get_maximal_number()
•
参数：整数n
•
返回值：n位数的最大整数
•
返回10的n次方的值减1
3. 计算num的第i位数字：
•
函数名：get_digit()
•
参数：整数num；整数i：表示取第i位
•
返回值：n位数num的第i位十进制数字
•
令round是num整除10的i-1次方
的值
•
返回round模10的值
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方
•
令结果result的初值为1
•
对于i从1到n
•
令result等于result乘
digit
•
返回result的值

函数的设计、使用与实现
函数使用函数实现
函
数
设
计
使用者无需
清楚函数的
具体实现
实现者无需关
注函数会被怎
样使用
函数设计是使用者与实现者之间的合约：
•
函数实现的功能
•
函数名、函数参数、函数返回值
•
函数参数应该满足的约束
•
函数返回值应该满足的约束
函数设计使得函
数使用与实现可
分离
函数使用与函数实现分离的好处：
•
将程序组织成层次化的模块
•
使得程序更容易理解、容易调试
•
使得函数更易扩充、更易重用

C语言的函数设计、实现与使用
程序设计时的功能分解、逐步细化，函数的实现与使用分离与具体程序语言无关!
•
基于过程式程序语言的特点，使用结构化自然语言描述求解问题的算法步骤
•
“结构化”的含义是指使用了表达条件判断与循环的层次化语句结构
C语言的函数设计：函数声明(declaration)
•
也称为函数原型(prototype)、函数接口
(interface)
•
函数名的作用域：函数在哪里声明和定义？
•
函数声明的语法：函数该怎样声明？
C语言的函数实现：函数定义(definition)
•
变量的作用域：变量在哪里声明或定义？
•
变量的生命期：变量在哪一段运行时期存在？
C语言的函数使用：函数调用(call)
•
参数的传递：按值传递、按引用传递
•
返回值的处理
程序的每个标识
符都有作用域
变量对应一段存储
•
名字、地址、类型
•
作用域、生命期

C语言的函数声明
函数声明的语法：
返回类型 函数名(形式参数列表);
•
形式参数列表：一个或多个形式参数
•
每个形式参数给出形式参数类型与形式参数名
•
没有形式参数时直接使用圆括号括住void
•
返回类型 函数名(void)
•
现在也支持直接使用园括号：返回类型 函数名
()
•
没有返回类型时，直接使用void
•
void 函数名(形式参数列表)
•
没有返回类型的函数也称为过程
void是一个
特殊的类型
// 返回n位数的最小整数
int get_minimal_number(int n);
// 返回n位数的最大整数
int get_maximal_number(int n);
// 返回整数num的第i位数字
int get_digit(int num, int i);
// 返回数字digit的n次方
int get_power(int digit, int n);
// 选择菜单项，并返回所选的序号
int select_menu(void);
// 做第i项菜单要做的事情
void do_menu(int i);

C语言的函数声明
函数名的作用域：
•
函数名的作用域是整个源代码文件
•
在声明点之后整个文件都可使用
•
在声明点之后整个文件不可再声明或定义同名字
的函数
•
使用其他文件或库定义的函数
•
使用#include导入包含该函数原型的头文件(.h
文件或.hpp文件）
•
用extern+函数原型的方式声明所使用的函数在
其他源代码文件定义
•
阻止其他源代码文件使用当前文件声明的函数
•
用static+函数原型将声明为静态（内部）函数
#include <stdio.h>
•
使得可使用标准的输入输出函数，例
如scanf()，printf()
extern int select_menu();
•
使得可使用在另外源代码文件中定义
的函数select_menu()
static void do_menu(int i);
•
使得函数do_menu()只能在当前文件
中定义和使用。
初学者可先不关注函数名的作用域，
因为通常只编写一个源代码文件！

C语言的函数定义
函数定义的语法：
返回类型 函数名(形式参数列表) { 函数体 }
•
给出函数原型并给出函数体就是函数定义
•
函数体用花括号括住，可以是任意一段C语言代
码
•
形式参数相当于变量声明
•
形式参数名的作用域是整个函数体，在整个函数
体当做变量使用
•
函数体可使用“return 表达式”返回函数值
•
表达式的类型要与函数返回类型相匹配，即能将
该表达式的值赋给类型为函数返回类型的变量
•
如果返回类型是void，则直接使用return加分
号
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = result * digit;
    return result;
}
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方
•
令结果result的初值为1
•
对于i从1到n
•
令result等于result乘
digit
•
返回result的值
为什么先实
现这个函数？

C语言的函数定义
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = result * digit;
    return result;
}
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方
•
令结果result的初值为1
•
对于i从1到n
•
令result等于result乘
digit
•
返回result的值
因为可以使
用这个函数！
3. 计算num的第i位数字：
•
函数名：get_digit()
•
参数：整数num；整数i：表示取第i位
•
返回值：n位数num的第i位十进制数字
•
令round是num整除10的i-1次方
的值
•
返回round模10的值
// 计算num的第i位数字
int get_digit(int num, int i) {
    const int base = 10;
    int round = num/get_power(base, 
i-1);
    return round % base;
}

C语言的函数调用
函数调用的语法：
函数名(实际参数列表)
•
函数调用是一个表达式（不是语句）
•
任何表达式能出现的地方都可有函数调用
•
没有返回类型时函数调用只能在后面加分号作为单独
的表达式语句
•
实际参数列表是一个或多个表达式
•
实际参数要与形式参数一一对应
•
相对应的实际参数要与形式参数类型匹配，即实际参
数表达式的计算结果能赋值给形式参数类型的变量
// 做第i项菜单要做的事情
void do_menu(int i);
// 另一个函数的定义
int fun() {
    ... ...
    do_menu(3);
}
// 返回数字digit的n次方
int get_power(int digit, int n);
... ...
// 计算num的第i位数字
int get_digit(int num, int i) {
    const int base = 10;
    int round = num/get_power(base, 
i-1);
    return round % base;
}

C语言的函数调用
函数调用时的程序执行流程：
•
发生函数调用时程序执行流程转向被调用函数的函数体
•
首先计算实际参数表达式的结果值，并将其赋值给形式参数，然后执行函数体的语句
•
执行函数体时遇到return语句，程序执行流程转回先前调用的地方
•
计算返回表达式（若返回类型不是void），并作为函数调用这个表达式的计算结果，参与先前调用地方的表达式计算
// 计算num的第i位数字
int get_digit(int num, int i) {
    const int base = 10;
    int round = num/get_power(base, 
i-1);
    return round % base;
}
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = result * digit;
    return result;
}
digit=base;
n=i-1;
r
e
s
u
l
t
的
值
更完整准确的描述需要使用
调用栈，暂时不深入讨论

函数调用时的参数传递：实际参数与形式参数相匹配的过程
•
按值传递(call by value)
•
实际参数是表达式，计算该表达式的值，将这个值赋给形式参数；函数体中对形式参数的修改不会影响实际参数
•
按引用传递(call by reference)
•
实际参数必须是能代表地址的左值（也称为引用），按引用传递使得形式参数与实际参数有相同的引用，即形式参数
与实际参数成为同一段存储的别名
C语言的函数调用
void swap(int x, int y) {
    int t = x; 
    x = y;  y = t;
}
int main() {
    int a = 1, b = 2;
    swap(a, b);
    printf(“a=%d, b=%d”, a, 
b);
}
学习引用类型和指针类型后
会更清楚按引用传递的含义
void swap(int* x, int* y) {
    int t = *x; 
    *x = *y;  *x = t;
}
int main() {
    int a = 1, b = 2;
    swap(&a, &b);
    printf(“a=%d, b=%d”, a, 
b);
}
void swap(int& x, int& y) {
    int t = x; 
    x = y;  y = t;
}
int main() {
    int a = 1, b = 2;
    swap(a, b);
    printf(“a=%d, b=%d”, a, 
b);
}
按值传递
输出：a=1, b=2
按引用传递
输出：a=2, b=1
按值传递
输出：a=2, b=1
1
a
x
1
a
x
1
x
1
a

n位水仙花数程序的实现
// 计算n位数的最小整数
int get_minimal_number(int n) {
    const int base = 10;
    return get_power(10, n-1);
}
1. 计算n位数的最小整数：
•
函数名：
get_minimal_number()
•
参数：整数n
•
返回值：n位数的最小整数
•
返回10的n-1次方的
值
2. 计算n位数的最大整数：
•
函数名：
get_maximal_number()
•
参数：整数n
•
返回值：n位数的最大整数
•
返回10的n次方的值
减1
// 计算n位数的最大整数
int get_maximal_number(int n) {
    const int base = 10;
    return get_power(10, n)-1;
}
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = result * digit;
    return result;
}
4. 计算一个数字digit的n次方：
•
函数名：get_power()
•
参数：整数digit；整数n
•
返回值：整数digit的n次方
•
令结果result的初值为1
•
对于i从1到n
•
令result等于result乘
digit
•
返回result的值

n位水仙花数程序的实现
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = result * digit;
    return result;
}
// 计算n位数的最小整数
int get_minimal_number(int n) {
    const int base = 10;
    return get_power(10, n-1);
}
// 计算n位数的最大整数
int get_maximal_number(int n) {
    const int base = 10;
    return get_power(10, n)-1;
}
// 计算num的第i位数字
int get_digit(int num, int i) {
    const int base = 10;
    int round = num/get_power(base, 
i-1);
    return round % base;
}
// 主程序
int main() {
    int n = 0;
    printf("Input n = ");
    scanf("%d", &n);
    int start = get_minimal_number(n);
    int end = get_maximal_number(n);
    for (int num = start; num <= end; num++) {
        int sum = 0;
        for (int i = 1; i <= n; i++) {
            int digit = get_digit(num, i);
            int digit_power = get_power(digit, n);
            sum = sum + digit_power;
        }
        if (num == sum) printf("%d is a narcissus 
                               number\n", num);
    }
}

n位水仙花数程序的实现，还有什么问题吗？
// 计算n位数的最小整数
int get_minimal_number(int n) {
    const int base = 10;
    return get_power(10, n-1);
}
// 返回数字digit的n次方
int get_power(int digit, int n) {
    int result = 1;
    for (int i = 0; i < n; i++) 
        result = resut * digit;
    return result;
}
n必须大于等于0
n必须大于等于1
函数设计是使用者与实现者之间的合约：
•
函数实现的功能
•
函数名、函数参数、函数返回值
•
函数参数应该满足的约束
•
函数返回值应该满足的约束
函数设计可以更
完整、更准确
程序运行的效率怎么样？
•
函数的使用会导致效率的下降
•
但这通常不是必然的和重要的
程序实际上不能输出超过5位的水仙花
数？
•
函数使用的变量类型是整数
•
即使使用长整数效果也不佳

输出3位水仙花数的另一程序
编写一个程序，输出所有的
三位数水仙花数（它等于它
的每位数字的立方和）
另一想法
对个位a从0到9：
对十位b从0到9：
对百位c从1到9：
数num = 100*c+10*b+a;
和sum=a^3+b^3+c^3;
如果sum等于num输出
num
#include<stdio.h> 
int main() {
    for (int a = 0; a < 10; num++) {
        for (int b = 0; b < 10; num++) {
            for (int c = 1; c < 10; num++) {
                int num = 100*c+10*b+a;
                int sum = a*a*a + b*b*b + c*c*c;
                if (sum == num) {
                    printf("%d is a narcissus 
                        number!\n", num);
                }
            }
        }
    }
}
如何将这个程序扩展
为输出n位水仙花数？

思考
1.编写一个 x
y 
的函数(x为实数, y为整数)
2.编写一个判断字符是否是大写字母的函数
3.编写程序，输入整数n，验证小于等于n的整数满足哥德巴赫猜想：
“任意大于2的偶数可分解成两个素数的和”，请对这个问题进行功能
分解、细化、分层，你会提取哪些函数？给出函数的设计与实现，
并完成主程序。

函数的调用
函数根据参数和返回值可以分为四类：
无参数，无返回值
有参数，无返回值
有参数，有返回值
无参数，有返回值
void
空，无
这两者类型
要一致

函数的调用
函数根据参数和返回值可以分为四类：
无参数，无返回值
有参数，无返回值
有参数，有返回值
无参数，有返回值
调用
call
返回
return
调动
a=5
返回
调用
call
返回
x=6
调动
a=5
返回
x=6

思考
求100～200间的全部素数，可用一个函数来判断一个数是否是素数。
#include<stdio.h>
#include<math.h>
int main() {
      int n,m=0;
      for(n=101; n<=200; n=n+2) {
            if(isPrime(n)) {
 printf("%d ",n);
                   m=m+1;
             }
             if(m%10==0) printf(“\n”);  //m累计到10的倍数， 换行
        }
        printf ("\n");
        return 0;
}
int isPrime(int x) {
      int i,k;
      k=sqrt(x);
      for(i=2; i<=k; i++)
           if(x%i==0) break; //如果n被i整除，终止内  循环，  此时i<k+1
      if(i>=k+1)      //若i>=k+1，表 示n未曾 被整 除
           return 1;
      return 0;
}
#include<stdio.h>
#include<math.h>
int main()
{int n,k,i,m=0;
for(n=101;n<=200;n=n+2)
{k=sqrt(n);
for(i=2;i<=k;i++)
if(n%i==0) break;
if(i>=k+1)
{printf("%d ",n);
m=m+1;
}
if(m%10==0) printf("\n");
}
printf ("\n");
return 0;
}

变量的作用域
变量的作用域（scope），指能够访问（access）该变量的范围，可以是全
局，可以是函数，可以是for循环，也可以是任意用大括号{}括起来的代码
块。
在main函数里无法访
问到add函数里的变量
a，但能访问到变量
x。
在add函数里，无法访
问到main函数里的变
量y，但能访问到变量
a。
变量x的作用域是main函
数
变量a的作用域是add函
数

变量的作用域
根据作用域的范围，变量可以分为三类
位置种类
在函数或代码块里
局部变量
（local variables）
在所有函数外面
全局变量
（Global variables）
在函数参数列表里
形式参数
（Formal 
parameters）

变量的作用域：局部变量
局部变量在函数（块）内声明，仅在该函数（块）内能够访问到。
其实函数和代码块都是由大括号{}包裹起来的
在该大括号内声明的变量仅在该大括号内可以访问
变量b在内部大括号内声明，外部访问不了
但内部大括号可以访问外部大括号的变量
a，
但访问不了同级大括号内的变量
一
个
代
码
块
另一个代码块

变量的作用域：局部变量
局部变量在函数（块）内声明，仅在该函数（块）内能够访问到。
一
个
代
码
块
另一个代码块
函数add
变量a
一个代码块
变量b
另一个代码块
变量c
在一个域内，可以访问包含其域的块（父亲）内变量
局部变量的生命周期也仅在这个块内
一旦当前执行的代码离开了这个块，在该块的变量就被销毁

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
在代码块X内可以访问到变量c
变量a，但访问不到变量t，变
量b

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
局部变量不会自动初始化
必须手动初始化
局部变量的生命周期仅在所在块内
代码执行离开了该块，其变量均被销毁

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量a创建，初始化为
0

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量c创建，并未初始
化
其值不确定。
注意，变量a不会销毁
因为此时还在函数块内

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量d创建，初始化为a的
值0
注意，变量c还没销毁
因为此时还在if else块内

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量d销毁
因为此时已经离开了代码块
X

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量c销毁
因为此时已经离开了if else代码
块

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量t创建，初始化为a的
值9

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量t销毁
因为此时已经离开了代码块
Y

变量的作用域：局部变量
稍复杂的例子
函数add
变量a
If then 代码块
变量b
If else 代码块
变量c
一个代码块X
变量d
另一个代码块Y
变量t
当前执行的代码
变量a销毁
因为此时已经离开了函数块
add

变量的作用域：全局变量
全局变量在函数外声明，一般在代码的顶部
在任何函数内都可以访问
全局变量
tot
函数add里可以访问全局变量
tot
全局变量会在程序初始时自动初始化
生命周期是整个程序的运行
直到程序结束才会被销毁
数据类型初始值
int0
char‘\0’
float0
double0
pointerNULL

变量的作用域：形式参数
形式参数在函数参数列表声明
其性质和局部变量r一样，但会被调用函数的参数初始化
形式参数a,b，分别被初始化为1
和2
在函数add内均可被访问，
生命周期持续整个函数执行过程，当函数执行完后才销毁

变量的作用域：变量同名
变量同名，会产生屏蔽的效果
函数add
变量b
If then 代码块
变量b
If else 代码块
变量a
一个代码块X
变量d
main函数
变量b
全局
变量a
函数add
形式参数a
全局变量
a
d=a
会往上溯源找到第一个符合名字的变量
但同一个代码块里
不可声明两次同样名字的变量

变量的作用域
位置种类初始化生命周期
在函数或代码块里
局部变量
（local 
variables）
必须手动初始化整个函数（块）
在所有函数外面
全局变量
（Global 
variables）
自动初始化为0整个程序
在函数参数列表里
形式参数
（Formal 
parameters）
根据传输参数值
自动初始化
整个函数（块）

思考
 
#include <stdio.h>
#include <math.h>
int main()
{int sign=1;
double pi=0.0,n=1.0,term=1.0;
while(fabs(term)>=1e-6)
{pi=pi+term;
n=n+2; 
sign=-sign;
term=sign/n;
}
pi=pi*4;
printf("pi=%10.8f\n",pi);
return 0;
}
#include <stdio.h>
#include <math.h>
int sign=1;
double n=1.0,term=1.0;
int main() {
double nextTerm(void);
double pi=0.0;
while(fabs(term)>=1e-6) {
pi=pi+term;
term = nextTerm();
}
pi=pi*4;
printf("pi=%10.8f\n",pi);
return 0;
}
double nextTerm(void) {
n=n+2;
sign=-sign;
term=sign/n;
return term;
}
#include <stdio.h>
#include <math.h>
int main() {
double nextTerm(int *sign,double *n);
double pi=0.0;
int sign=1;
double n=1.0,term=1.0;
while(fabs(term)>=1e-6) {
pi=pi+term;
term = nextTerm(&sign, &n);
}
pi=pi*4;
printf("pi=%10.8f\n",pi);
return 0;
}
double nextTerm(int *sign,double *n) {
*n=*n+2;
*sign=-*sign;
return *sign/(*n);
}

思考
输入两个整数，用函数求出他们的最大值、最小值、平均值。
#include<stdio.h>
int main() {
double swap (int *a,int *b);
int a,b;
scanf("%d%d",&a,&b);
double ava=swap(&a,&b);
printf("max=%d,min=%d,ava=%lf",a,b,ava);
return 0;
}
double swap (int *a,int *b) {
if(*a<*b) {
int t = *a;
*a=*b;
*b=t;
}
return (double)(*a+*b)/2;
}

函数的嵌套调用
① 执行main函数的开头部分；
② 遇函数调用语句，调用函数a，流程转去a函数；
③ 执行a函数的开头部分；
④ 遇函数调用语句，调用函数b，流程转去函数b；
⑤ 执行b函数，如果再无其他嵌套的函数，则完成b函数的全部操作；
⑥ 返回到a函数中调用b函数的位置；
⑦ 继续执行a函数中尚未执行的部分，直到a函数结束；
⑧ 返回main函数中调用a函数的位置；
⑨ 继续执行main函数的剩余部分直到结束。
C语言的函数定义是互相平行、 独立的，也就是说，在定义函数时，一个函数内不能再定义另
一个函数，即不能 嵌套 定义，  但可以 嵌套 调用函数，即在调用一个函数的过程中，又调用 另一
个函数  。
main函数a函数b函数
①②③④
调用a函数调用b函数     ⑤
⑨⑧⑦⑥
  结 束
48

函数的嵌套调用
【例 1】输入4个整数，找出其中最大的数。用函数的嵌套调用来处理。
#include <stdio.h>
int main()
{int max4(int a,int b,int c,int d);//对max4的函数声明
int a,b,c,d,max;
printf("Please enter 4 interger numbers:");//提示 输入4个数
scanf("%d %d %d %d",&a,&b,&c,&d);//输入4个数
max=max4(a,b,c,d); //调用max4函数，得到4个数 中的最大者
printf("max=%d \n",max);//输出4个数 中的最大者
return 0;
}
int max4(int a,int b,int c,int d)//定义max4函数  
{int max2(int a,int b);//对max2的函数声明
int m;
m=max2(a,b);//调用max2函数，得到a和b中的大者，放在m
中
m=max2(m,c);//调用max2函数，得到a,b,c中的大者，放在m
中
m=max2(m,d);//调用max2函数，得到a,b,c,d中的大者，放在
m中
return(m);//把m作为函数值带回main函数
}
int max2(int a,int b)//定义max2函数  
{if(a>=b)
return a;//若a≥b，将a作为函数返回值 
else
return b;//若a<b，将b作为函数返回值
}
在主函数 中要调用max4函数，因此在主函数的开头要对max4函数作声明。在max4函数 中3次调用max2函数，因此在max4函数的开
头要对max2函数作声明。由 于在 主函数 中没 有直接调用max2函数，因此在主函数 中不必对max2函数作声明，只须在max4函数 中作声
明即可。
max4函数执行过程: 第1次调用max2函数得到的函数值是a和b中的大者，把它赋给 变量m，第2次调用max2得到m和c中的大者，
也就是a,b,c中的最大者，再把它赋给 变量m。第3次调用max2得到m和d中的大者，也就是a,b,c,d中的最大者，再把它赋给 变量m。
这是一  种递推方法，先求出2个数的大者；再以 此为基 础求出3个数的大者；再以 此为基 础求出4个数的大者。m的值一次一次地变化，
直到实现最终要求 。
49

函数的嵌套调用
(1) 可以将max2函数的函数体改为只用一个return语句，返回一个条件表达式的值: 
(2) 在max4函数中，3个调用max2的语句可以用以下一行代替: 
甚至可以取消变量m，max4函数可写成
先调用“max2(a,b)”，得到a和b中的大者。再调用“max2(max2(a,b),c)”(其中max2(a,b)为已知)，
得到a,b,c三者中的大者。最后由“max2(max2(max2(a,b),c),d)”求得a,b,c,d四者中的大者。
程序 改进
int max2(int a,int b)//定义max2函数  
{return(a>=b?a:b);}//返回条件表达式的值，即a和b中的
大者
m=max2(max2(max2(a,b),c),d);//把函数调用作为函数参数
int max4(int a,int b,int c,int d) 
{int max2(int a,int b);//对max2的函数声明
return max2(max2(max2(a,b),c),d);
}

递归函数
递归函数（recursive function）是一种能够调用自身的函数。 
这种技术叫做递归（recursion）
自身调用
当然，不能无休止的调用
使用条件语句等避免无休止调用，
即满足一定条件后停止自身调用

递归函数

最初，从main函数调用sum函数，并传递
一个参数number。

假设number为3，则在下一次调用sum
时，传递参数2。直到n等于0。

当n等于0时，if条件不满足，执行else语
句里的返回，直到main函数。
一个简单的1+2+3+...+n的递归计算函
数

递归函数

递归函数
【例2】有5个学生坐在一起，问第5个学生多少岁，他说比第4个学生大2岁。问第4个学生岁
数，他说比第3个学生大2岁。问第3个学生，又说比第2个学生大2岁。问第2个学生，说比第
1个学生大2岁。最后问第1个学生，他说是10岁。请问第5个学生多大。
解题思路:
age(n)=10(n=1)
age(n)=age(n-1)+2(n>1)
age(5)age(5)
=age(4)+2=18
age(4)age(4)
=age(3)+2=16
age(3)age(3)
=age(2)+2=14
age(2)age(2)
=age(1)+2=12
age(1)=10

递归函数
【例 2】有5个学生坐在一起，问第5个学生多少岁，他说比第4个学生大2岁。问第4个学生岁
数，他说比第3个学生大2岁。问第3个学生，又说比第2个学生大2岁。问第2个学生，说比第
1个学生大2岁。最后问第1个学生，他说是10岁。请问第5个学生多大。
#include <stdio.h>
int main()
{int age(int n);//对age函数的声明
printf("NO.5,age:%d\n",age(5)); 
return 0;
} 
int age(int n) //定义递  归函数
{int c; //c用作存  放函数的返回值的变量
if(n==1) //如果n等于1
c=10;//年龄 为10
else //如果n不等于1
c=age(n-1)+2;//年龄 是前一个学  生的年龄 加2(如第4个学 生年龄  是第3个学 生年龄  加2)
return(c); //返回 年龄
}
注意分析递归的终止条件。
main
age(n)
n=5
age(n)
n=4
age(n)
n=3
age(n)
n=2
age(n)
n=1
age(5)
输出age(5)
return 0
c=age(4)+2
return (c)
c=age(3)+2
return (c)
c=age(2)+2
return (c)
c=age(1)+2
return (c)
c=10
return (c)
age(5)=18age(4)=16age(3)=14age(2)=12age(1)=10

递归函数
【例3】用递归方法求n!。
 
#include <stdio.h>
int main()
{int fac(int n);//fac函数声明
int n;
int y;
printf("input an integer number:");
scanf("%d",&n);//输入要求阶乘 的数
y=fac(n);
printf("%d!=%d\n",n,y);
return 0;
}
int fac(int n) //定义fac函数
{
int f;
if(n<0)//n不能小于0
printf("n<0,data error!");
else if(n==0||n==1)//n=0或,1时n!=1
f=1;//递归终止  条件
else
f=fac(n-1)*n; //n>1时，n!=n*(n-1)
return(f);
}
main
fac(n)
n=5
fac(n)
n=4
fac(n)
n=3
fac(n)
n=2
fac(n)
n=1
fac(5)
输出fac(5)
f=fac(4)*5f=fac(3)*4f=fac(2)*3f=fac(1)*2f=1
fac(5)=120fac(4)=24fac(3)=6fac(2)=2fac(1)=1
注意
•
程序 中的变量是int型，如果
用Visual C++、GCC以及多
数C编译系统  为int型数 据分
配4个字节，能表示的最大数
为2 147 483 647，当n=12时，
运行 正常，输出为479 001 
600。如果输入13，企图 求
13!，是得不到预期 结果的，
因为求出的结果超过了int型
数据的最大值。可将f,y和fac
函数定义为float或double型。

递归函数
【例 4】Hanoi（汉诺）塔问题。古代有一个梵塔，塔内有3个座A,B,C。开始时A座上有64个盘子，盘子大
小不等，大的在下，小的在上。有一个老和尚想把这64个盘子从A座移到C座，但规定每次只允许移动一个盘，
且在移动过程中在3个座上都始终保持大盘在下，小盘在上。在移动过程中可以利用B座。要求编程序输出移动
盘子的步骤。
ABC
解题思路:
老和尚会这样想: 假如有另外一个和尚能有办法将上面63个盘子
从一个座移到另一座。那么，问题就解决了。此时老和尚只须这样
做: 
① 命令第2个和尚将63个盘子从A座移到B座；
② 自己将1个盘子（最底下的、最大的盘子）从A座移到C座；
③ 再命令第2个和尚将63个盘子从B座移到C座。
第2个和尚又想: 如果有人能将62个盘子从一个座移到另一座，
我就能将63个盘子从A座移到B座，他是这样做的: 
① 命令第3个和尚将62个盘子从A座移到C座；
② 自己将1个盘子从A座移到B座；
③ 再命令第3个和尚将62个盘子从C座移到B座。
......
ABC
63个
盘子
①
③
②

ABC
移动前
解题思路:
为便于理解，先分析将A座上3个盘子移到C座上的过程：
① 将A座上2个盘子移到B座上（借助C座）。
② 将A座上1个盘子移到C座上。
③ 将B座上2个盘子移到C座上（借助A座）。
其中第②步可以直接实现。第①步又可用递归方法分解为: 
 将A座上1个盘子从A座移到C座；
 将A座上1个盘子从A座移到B座；
 将C座上1个盘子从C座移到B座。
第③步可以分解为: 
 将B座上1个盘子从B座移到A座上；
 将B座上1个盘子从B座移到C座上；
 将A座上1个盘子从A座移到C座上。
将以上综合起来，可得到移动3个盘子的步骤为: 
A→C，A→B，C→B，A→C，B→A，B→C，A→C。
共经历7步。由此可推出: 移动n个盘子要经历(2
n
-1)步。
ABC
①
ABC
②
ABC
③
递归函数

递归函数
解题思路:
由上面的分析可知: 将n个盘子从A座移到C座可以分解为以下3个步骤: 
① 将A座上n－1个盘借助C座先移到B座上；
② 把A座上剩下的一个盘移到C座上；
③ 将n－1个盘从B座借助于A座移到C座上。
上面第①步和第③步，都是把n－1个盘从一个座移到另一个座上，采取的办法是一样的，只是座的名字不同而
已。为使之一般化，可以将第①步和第③步表示为: 
将one座上n－1个盘移到two 座(借助three座)。只是在第①步和第③步中，one,two,three和A,B,C的对应
关系不同。对第①步，对应关系是one对应A，two对应B，three对应C。对第③步，是: one对应B，two
对应C，three对应A。
因此，可以把上面3个步骤分成两类操作: 
① 将n－1个盘从一个座移到另一个座上（n＞1）。这就是大和尚让小和尚做的工作，它是一个递归的过程，
即和尚将任务层层下放，直到第64个和尚为止。——hanoi函数
② 将1个盘子从一个座上移到另一座上。这是大和尚自己做的工作。——move函数

递归函数
在本程序 中，调用递   归函数hanoi，其终止  条件为hanoi函数的参数n的值
等于1。显 然， 此时不必再调用hanoi函数了，直接执行move函数 即可。
在本程序 中move函数 并未真正移动盘子，而只是输出移盘 的方 案（表 示从
哪一个 座移 到哪一个 座）。
#include <stdio.h>
int main()
{
void hanoi(int n,char one,char two,char three);
//对hanoi函数的声明 
int m;
printf("input the number of diskes:");
scanf("%d",&m);
printf("The step to move %d diskes:\n",m);
hanoi(m,'A','B','C');
}
void hanoi(int n,char one,char two,char three)//定义hanoi函数
//将n个盘从one座借助two座,移到three座 
{void move(char x,char y);//对move函数的声明 
if(n==1)
move(one,three);
else
{hanoi(n-1,one,three,two);
move(one,three);
hanoi(n-1,two,one,three);
}
}
void move(char x,char y)//定义move函数  
{printf("%c->%c\n",x,y); }

递归函数
递归使程序代码优雅简洁。但是，由于调用函数的缘故，递归  通常要慢得多。
但是递归的概念至关重要， 它经常用在数据结构和算法中，比如图的搜索与
回溯

总结

函数就是一个能完成特定功能的代码块

函数可以有传入参数，也可以有返回值

函数的传入参数分为按值传递和引用传递

函数里定义的变量叫局部变量，传入参数叫做形式参数，函数外定义的变量叫全局变量

函数可以自身调用

思考 1
输入两个数 m,n 求 m!/n! 的值

思考 2
对10个数组元素依次赋值0,1,2,3,4,5,6,7,8,9，要求按逆序输出。

谢谢
