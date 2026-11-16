# 05函数和作用域-final-图集

> 主题库：程序设计　｜　源文件：05函数和作用域-final-图集.pdf　｜　3,436 字符　｜　25 个片段　｜　更新于 2026-11-11
中山大學

SUN YAT-SEN UNIVDRSITY

int main(void){ add(); return 0;

int main(void){ add(5); return o;

int main(void){ int x = add(5); return o;

int main(){ int x = add(); return 0;

int main(void){ inty=5; int x = add(y); return 0;

void add(void){ int a; a intb=2; b I a a b3 error int c b; i error } }

void add(void){ int a = 0; 6 if (a == 1){ int b = 1; int c = 2; int d = a; } C = d; //error } a 9 int t = c; //error } }

Void  add(void){ int a = 0; if(a==1){ int b = 1; jelse{ int c; int d = a; C 1= a; } a 9; intt=a a; }

中山大學     SUN YAT-SEN UNIVERSITY

#include <stdio.h> int tot; void add(void){ int a = tot; int main(){ add(); return e;

void add(int a,int b){ intr = 1; int main(){ add(1, 2); return 0;

#include <stdio.h> int a; void add(int a){ int b = 0; if (a ==1){ int b = 1; }else{ inta =2; intd = a; } a 9; = int main(){ add(e); int b = a;

int main( ){ int b = a; intb =1;  //error intb = 1; //ok return 0;

void recurse()) recurse(); int main() recurse();

int sum(int n) { if (n != 0) // sum() function calls itself return n+ sum(n-1); else return n; int main()日 intnumber, result; printf("Entera positive  integer: I scanf("%d", &number); result = sum(number); printf("sum =%d", result);

intmain){ 3 result=sum(number); } ③ 3+3=6 is returned int sum(int n){ if(n != 0) return n+sum(n-1) else  returnn; } 2+1=3 2 isreturned if(n!= 0) returnn+sum(n-1) else return n; } 1+0=1 1 isreturned intsum(intn){ if(n != 0) returnn+sum(n-1) else return n; } 0 0 int sum(int n){ is returned if(n!=0) return n +sum(n-1) else returnn;

. C:\WINDOWS\system32\cmd.exe  × input the number of diskes:3  The step to move 3 diskes: A—C A->B C->B ->c B->A B— —>C ->C 意键e

学大山中立园

中山大學 SUN TAT-SEN UNINERSITY

01 函数是什么 [Tab a T _? 函数的设计 [02 函数的声明与定义函数的实现函数的使用目录 03 函数的调用 CONTENT 04 变量的作用域 S 05 递归函数 06 总结

中山大學 SENUN SUN YAT-SEN UNIVERSITY 思考求100～ 200间的全部素数，可用一个函数来判断一个数是否是素数。 #include<stdio.h> #include<math.h> #include<stdio.h> int mainO{ intmain) for(n=101;n<=200;n=n+2){ { int n,k,i,m = 0; if(isPrim e(n)) { for(n=101;n<=200;n=n+2) printf %d",n); k=sqrt(n); m =m +1; for(i=2;i<=k;i++) } if(n%i==0)break； if(m%10==0)printf(“\n")；//m 累计到 10 的倍数 ， 换行 if(i>=k+1) } { printf("% d ",n); printf("\n"); m=m+1; return0; } } Yf m%10==0 printf"\n"); } int isPrim e(int x) { int i,k; printf ("\n"); kasarte) Yeturn for(i=2; i<=k;i++) if(x%i==0) break;//如果n被i整除， 终止内循环， 此时i<k+1 if ia=k+1) //若i>=k+1， 表示n未曾被整除 return1; return 0

递归函数解题思路： 为便于理解，先分析将A座上3个盘子移到C座上的过程： ①将A座上2个盘子移到B座上（借助C座）。 A B C ②将A座上1个盘子移到C座上。 移动前 ③将B座上2个盘子移到C座上（借助A座）。 其中第②步可以直接实现。第①步又可用递归方法分解为： 将A座上1个盘子从A座移到C座； 将A座上1个盘子从A座移到B座; A B C 将C座上1个盘子从C座移到B座。 ① 将B座上1个盘子从B座移到A座上； 将B座上1个盘子从B座移到C座上； A B C 将A座上1个盘子从A座移到C座上。 ② 将以上综合起来，可得到移动3个盘子的步骤为： A→C，A→B，C→B，A→C，B→A，B→C，A→C。 共经历7步。 由此可推出：移动n个盘子要经历 (2n-1) 步。 A B C 3

中山大學 . C:\WINDOWS\system32\cmd.exe ×  A- 递归函数 #include <stdio.h> void hanoi(int n.char one.char two.char three) // 定义 hanoi 函数 intmain) //将n个盘从one座借助two座，移到three座  { voidmove(charx,chary)； // 对move函数的声明  void hanoi(int n,char one,char two,char three); if(n==1) 对hanoi函数的声 move(one,three) int m ; else printf("input the number of diskes:"); { hanoi(n-1,one,three,two); scanf("%d",& m); move(one,three); printf("The step to move %d diskes:\n",m); hanoi(n-1,two,one,three); hanoi(m,ABC) } } Yaidmovecharx,chary /定lmove函数 { printf("%c->%c\n",x,y);} 在本程序中，调用递归函数hanoi，其终止条件为 hanoi 函数的参数n 的值等于1。 显然，此时不必再调用hanoi 函数了， 直接执行move函数即可。 在本程序中move函数并未真正移动盘子，而只是输出移盘的方案(表示从哪一个座移到哪一个座)
