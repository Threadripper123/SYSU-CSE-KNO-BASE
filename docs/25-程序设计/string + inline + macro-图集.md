# string + inline + macro-图集

> 主题库：程序设计　｜　源文件：string + inline + macro-图集.pdf　｜　2,103 字符　｜　5 个片段　｜　更新于 2026-11-11
8.1 C++内联函数 int main() int main()  hubba(2);← n= 2; for (inti=0;i <n;i++) hubba(4);← cout<s"hubba! 事 cout<<"\n''; hubba(10); n=4; for(inti=0;i<n;i++) cout <"hubba!''; cout<<''In"; n=10; for(inti=0;i<n;i+) cout <<"hubbal '' cout <<''In''; void hubba(int n) for (inti =@; i < n; i++) cout<e ''hubbai cout <<''\n''; 常规函数将程序流程内联函数用内联代转到独立的函数码替换函数调用图8.1 内联函数与常规函数

Cstring 和string Cstring和string是表示字符串的不同方法因此有不同，也有相似之处，如提供的函数的功能很相似，但使用方法不同。  以nu字符结尾的要调用对c串进行操作的函数， 如 strlen，strcpy,strcmp等函数要 include<cstring 是类包含数据和员函数要使用 string这种数据类型， 需要 #include<string> ： 能使用string， 就不使用cstring. 2

String 变量上的操作 #include <string> #include<iostream > using namespace std; int mainO{ string s1 ="Object-Oriented"; string s2 ="Programming''"; strings3=s1. (7，9)；//substr成员函数用于提取子串 string s4 = s1 s2; //在string中重载了operator+ 于把两个字符串拼接在一起， 类似于c串的strcat操作，但更直观好用 //length和size都可以于求字符串的长度输出： intlenstrl,lenstr2 字符 1的长度是：16 len_str1=s1 ; len_str2=s2. ; string outputStr; s4:Object-Oriented Programming outputStr= 字符串 1 的长度是: s1 += s2： Object-Oriented Programming cout << outputStr << len_str1<< endl; s1.erase(7, 9 ): Object-Programming cout <<"s4:"<< s4 << endl; s1.insert(7, s3 ):Object-Oriented Programming s1 s2;//运算符重载 s1.replace(7,9,"**"):Object-**Programming cout<<"s1+=s2:"<<s1<<end1; s1 (7，9)：//删除从第7个位置处开始的9个字符 cout<<"s1.erase(7,9):"<<s1<<endl; s1. (7，s3)；//从第7个位置处插入字符串 Stringfuncs.cpp cout <<"s1.insert( 7,s3 ):"<< s1 << endl; s1. (7，9，"**)；//从第7个位置开始的9个字符替换为字符串 cout<<"s1.replace(7,9,\"*\"):"<<s1 << endl; 5 returnO;}

inline 内联函数与宏 （macro) （ 每个调用都用定义行替换减少函数用开销，提高程效率 √如果扩展较大或有较多扩展，可执行映像会变得相当宏是由预处理器扩展，内联函数由编译器扩展专宏展开做文本替换而不考虑代码的语义；但内联函数的扩展考虑语义。 macro:没有类型安全检查与宏相比通常选择内联函数 10

macro /上述部分问题的解方法 #include<iostream> #define SQUARE(x)((x)*(×)) using namespace std; int main){ doublea,b; 输出： doublec=13.0; a=25,b=144 c = 13, c square = 182 NoW c= 15 a = SQUARE(5.0);//((5.0)*(5.0)) b = SQUARE(4.5 + 7.5); //((4.5 + 7.5)*(4.5 + 7.5)) cout<<a ="<<a<<",b="<<b<<endl; cout<<"c="<<C; cout<< , c square = "<< SQUARE(c++)<<endl; //(c++)(c++))因为宏不能按值传递这一点仍然无法通过加小括号来解决 cout << "Now c = " << c << endl; macro-square.cpp return0; 12
