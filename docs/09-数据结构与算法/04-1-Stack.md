# 04-1-Stack

> 主题库：数据结构与算法　｜　源文件：04-1-Stack.pdf　｜　15,559 字符　｜　51 个片段　｜　更新于 2026-11-11


Stacks
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

Content
• Definition and operations
• Implementation
• Applications
Data Structure & Algorithms2
1
2
3
4
5
5
4
3
2
1

An Example
•Word中的误操作
•如果在使用Word时发生了误操作，比如删错了内容，你会怎么办？
•熟悉Word的人会说，这个不要紧，用“撤销”工具就可以恢复被误删的内容了，
“撤销”工具可以撤销前面做的操作。
Data Structure & Algorithms3

Definition of Stacks
•限定仅在表尾(栈顶)进行插入和删除操作的线性表。
Data Structure & Algorithm Analysis4
Last In, First Out（LIFO）

More Examples
•When solving a problem, you may need some 
mechanism that has the property “last in and first 
out”.
•Examples: 
–Balancing symbols or bracket matching: To check that 
every right brace, bracket, and parentheses must 
correspond to its left counterpart
•e.g. [( )] is legal, but [( ] ) is illegal
–Postfix expression (reverse polish notation) evaluation: 
an operator is placed after their operands
•e.g. a b * c +, a b c+*, a b+c*,  a b +c +. 

Push and Pop
•The primary operations on a stack are push, 
and pop.
•push
–Add an element to the top of the stack
•pop
–Remove the element at the top of the stack
top
empty stack
A
top
push an element
top
push another
A
B
top
pop
A

Push and Pop
Data Structure & Algorithms7
AAAAAA
BB
C
BD
Push(A)Push(B)Push(C)Push(D)PopPop

Example: Balancing Symbols
•To check that every right brace, bracket, and parentheses must 
correspond to its left counterpart
–e.g. [( )] is legal, but [( ] ) is illegal
•Algorithm
(1)   Make an empty stack.
(2)   Read characters until end of file
i.    If the character is an opening symbol, push it onto the stack
ii.   If it is a closing symbol, then if the stack is empty, report an error
iii.  Otherwise, pop the stack. If the symbol popped is not the   
      corresponding opening symbol, then report an error
(3)   At end of file, if the stack is not empty, report an error

Example: Evaluating Postfix Expressions
•Postfix expression (reverse polish notation): an 
operator is placed after their operands
–No braces are required
–Easy to compute by using stacks
•Postfix expression computation:
–When an operand is read, push it into stack;
–When an operator is read, pop its operands, compute 
the result and push the result onto stack
•Exercise: Write an algorithm that converts an infix 
expression to a postfix expression.

Stack ADT
•A stack is a list in which insertion and deletion take 
place at the same end
–This end is called top
–The other end is called bottom
•Stacks are known as LIFO (Last In, First Out) lists.
–The last element inserted will be the first to be retrieved

Implementation of Stacks
•Any list implementation could be used to 
implement a stack
–Arrays (static: the size of stack is given initially)
–Linked lists (dynamic: never become full)
•We will explore implementations based on arrays 
and linked lists
•Let’s use an array to implement a stack first

Stack class
typedef doulbe T; 
class Stack {
public:
Stack();// constructor
~Stack();// destructor
      Stack(const Stack &rhs);
              const Stack & operator=(const Stack &rhs);
bool empty() const;
bool full() const;
const T& top()const;//examine, without popping 
void push(const T x);
void pop(); //not efficient if design: T pop()
private:
      int size;         //size of the array values
int maxtop;// max stack size = size - 1
int top;// current top of stack
T* values;// element array
};

Stack class
typedef double T; 
class Stack {
public:
Stack()// constructor
~Stack() { delete [] values; }// destructor
      Stack(const Stack &rhs);
      const Stack & operator=(const Stack &s); 
bool empty() { return top == -1; }
bool full() { return top == maxtop; }
const T& top()const;   // examine, without popping
void push(const T x);
void pop();
private:
      int size; //size of the array values
int maxtop;// max stack size = size - 1
int top;// current top of stack
T* values;// element array
};

Stack class
•Attributes of Stack
–maxtop: the max size of stack
–top: the index of the top element of stack
–values: points to an array which stores elements of stack
•Operations of Stack
–empty: returns true if stack is empty, returns false otherwise
–full: returns true if stack is full, returns false otherwise
–top: returns the element at the top of stack
–push: adds an element to the top of stack
–pop: deletes the element at the top of stack

Array Implementations: Create Stack
•The constructor of Stack
–Allocate a stack array of size. By default, 
size = 0.
–Initially top is set to -1. It means the stack is empty. 
–When the stack is full, top will have its maximum 
value, i.e. size – 1.
Stack::Stack():size(256) {
values=new T[size];
top=-1;
      maxtop=size - 1;
}
Although the constructor dynamically allocates the stack array, 
the stack is still static. The size is fixed after the initialization.

Array Implementations: push Stack
•void push(const T x);
–push an element onto the stack
–Precondition: stack is not full
–Note top always represents the index of the top 
element. After pushing an element, increment top.
void Stack::push(const T x) {
if (!full()) 
          values[++top]= x;
}

Array Implementations: pop Stack
•void pop()
–pop the element at the top of the stack
–Precondition: stack is not empty
–Don’t forget to decrement top
void Stack::pop() {
if (!empty()) {
       top--;
}
}

Array Implementations: Stack top
•Const T& top()const
–Returns the top element of the stack
–Precondition: stack is not empty
–Unlike pop, this function does not remove the top element
Const T& Stack::top()const {
if (!empty()) 
 return values[top];
}

Using Stack
int main(void) {
Stack stack();
stack.push(5.0);
stack.push(6.5);
stack.push(-3.0);
stack.push(-8.0);
cout << "top: " << stack.top() << endl;
stack.pop();
cout << "top: " << stack.top() << endl;
while (!stack.empty()) {
          stack.pop();
      }
return 0;
}
result

•We could use list as the data member to store stack 
elements.
•To make the best out of the code of List, we 
implement Stack by inheriting List
–To let Stack access private member head, we 
make Stack as a friend of List
Implementations based on Linked List
class List {
public:
List(void) { head = NULL; }// constructor
~List(void);// destructor
bool empty()const;
Node* insert(Node* p, T x);
Node* find (T x);
Node* erase(Node *p);
 private:
Node* head;
friend class Stack;
};

Implementation based on Linked List
class Stack : private List {
public:
Stack() {}// constructor
~Stack() {}// destructor
         bool empty()const{return List::empty();}
const T& top()const { 
if (head != NULL) {
return head->data;
}
void push(const T x) { insert (head, x); }
void pop() { 
if (head != NULL) {
T val = head->data;
DeleteNode(val);
}
}
};
Note: the stack 
implementation 
based on a linked 
list will never be full.
Stack will not export 
other methods of 
lists.

Array implementation versus 
linked list implementations
•push, pop, top are all constant-time operations in 
both array implementation and linked list 
implementation
–For array implementation, the operations are 
performed in very fast constant time

Recursion and Stacks
•Recursive calls are implemented by using 
stacks;
•Recursion can be replaced by stacks and 
iteration (迭代);

Function  A calls B, B calls C:
A
B
C
First started, last 
finished
Save returning 
address and 
parameters  
Start from the 
saved address

Stack Frames for Subprograms 
When a program calls a subprogram, the 
system:
•Store  returning location and parameters;
•Allocate storage for  all the local variables.
•Then the control is passed to the 
subprogram.
The information about the returning address, 
parameters and local variables is stored in the 
activation record(活动记录） for the function call.

When the subprogram is finished, 
•Returns the result of the subprogram;
•Releases the storage allocated for the 
subprogram; 
•the control is passed to the caller and 
execution starts at the returning address.

Temporary storage areas for use by functions would be 
allocated in a list with the property last in, first out.  So, 
stack is the data structure to manage function call 
mechanism. 
When a function call is made:
• the activation record is pushed into the stack, and
• when the function is finished, the activation record for the 
function is popped off. 
•There is no difference between this recursive call and other 
function calls: for each recursive call, an activation record 
for the instance of the call is pushed into the stack. 

Computing factorial(4)
factorial(0) =1;
factorial(n) = n*factorial(n-1);
   
 
return 1 
factorial(4) 
return 4 * factorial(3) 
return 3 * factorial(2) 
return 2 * factorial(1) 
return 1 * factorial(0) 
Step 9: return 24 
Step 0: executes factorial(4) 
Step 1: executes factorial(3) 
Step 2: executes factorial(2) 
Step 3: executes factorial(1) 
Step 5: return 1 
Step 6: return 1 
Step 7: return 2 
Step 8: return 6 
Step 4: executes factorial(0) 

Stack Frame for factorial
Space  Required
for factorial(4)
1
Space  Required
for factorial(4)
2
Space  Required
for factorial(3)
Space  Required
for factorial(4)
3
Space  Required
for factorial(3)
Space  Required
for factorial(2)
Space  Required
for factorial(4)
4
Space  Required
for factorial(3)
Space  Required
for factorial(2)
Space  Required
for factorial(1)
Space  Required
for factorial(4)
5
Space  Required
for factorial(3)
Space  Required
for factorial(2)
Space  Required
for factorial(1)
Space  Required
for factorial(0)
Space  Required
for factorial(4)
6
Space  Required
for factorial(3)
Space  Required
for factorial(2)
Space  Required
for factorial(1)
Space  Required
for factorial(4)
7
Space  Required
for factorial(3)
Space  Required
for factorial(2)
Space  Required
for factorial(4)
8
Space  Required
for factorial(3)
Space  Required
for factorial(4)
9

Stack Application: function calls and recursion
•Take the example of factorial! And run it.
int fac(int n){
int product;
if(n <= 1) product = 1;
else product = n * fac(n-1);
return product;
}
void main(){
int number;
cout <<"Enter a positive integer:"<< endl;
cin >> number;
cout << fac(number) << endl;
}
L1
L3
L2
Activation Record: (return address, parameters, local vars)
L0

Call is to ‘push’ and return is to ‘pop’!
int fac(int n){
int product;
if(n <= 1) product = 1;
else product = n * fac(n-1);
return product;
}
L1
L2
Compute fac(2):

Call is to ‘push’ and return is to ‘pop’!
int fac(int n){
int product;
if(n <= 1) product = 1;
else product = n * fac(n-1);
return product;
}
L1
L2
Compute fac(2):
Stack
product:
n: 2
Add: L0
product:
n: 2
Add: L0
(A) 调用f(2)，
活动记录入栈，
转去执行f
product:1
n:1
Add:L2
 
(B) 调用f(1), 
product赋值1，
f(1)返回product, 
活动记录出栈
product:n*1
n:2
Add:L0
(C ) 转L2,将函
数返回值与n
之积赋product
(D) f(2)调用结
束，弹出活动
记录，将结果
2返回，转L0
继续执行。

Space for recursion algorithms
•Space for recursion is the maximum number of 
activation records, or the number  of embedded 
recursive calls, or the height of the recursion tree
•For recursive factorial, S(n) = O(n); 
•For Hanoi tower, S(n) = O(n).

Exercises
•例子1：若一个栈的输入序列是1，2，3，...，n，输出序列的第一个元素是n，
则第i个输出元素是（）。
•A. 不确定       B. n-i       C. n-i-1       D. n-i+1
•解释：第n个元素先出栈，说明前n-1个元素都已经按顺序入栈，由“后进先出”的特点可知，此时的
输出序列一定是输入序列的逆序。
•例子2：若一个栈的输入序列是1，2，3，...，n，输出序列的第一个元素是 i，
则第 j 个输出元素是（）。
•A. i-j-1       B. i-j        C. j-i+1        D. 不确定
•解释：当第i个元素第一个出栈时，则i之前的元素可以依次排在i之后出栈，但剩余的元素可以在此时
进栈，并且也可能排在i之前的元素出栈，所以，第j个出栈的元素是不确定的。
Data Structure & Algorithms34

Exercises
•例子：设有一个数列的输入顺序为123456，若采用栈结构，并以S和X
分别表示进栈和出栈操作，请回答以下问题：
•1. 能否得到输出顺序为325641的序列？
•2. 能否得到输出顺序为154623的序列？
•解释1：1可以得到输出顺序为325641的序列，操作为 S S S X X S S X S X X X 
•解释2：不能得到输出顺序为154623的序列。因为执行 S X S S S S X X S X，得到输出序列
1546后，栈中的元素从栈顶到栈底为32，不能让2先出栈，所以得不到输出序列154623。
Data Structure & Algorithms35

Exercises
•例子：有一个带头结点的单链表L，设计一个算法利用链栈将其所有结点逆置。
•思路：用p遍历单链表L的所有数据结点，设计一个不带头结点的链栈 lst（其结
点类型与单链表的结点类型相同），先将单链表L的所有数据结点进栈，然后出
栈所有结点并重新建立单链表L，其中采用尾插法建表。
Data Structure & Algorithms36

Exercises
•例子：有一个带头结点的单链表L，设计一个算法利用链栈将其所有结点逆置。
•对比不采用栈结构的方式（单纯链表操作）：将头结点摘下，然后从第一结点
开始，依次前插入到头结点的后面（头插法建立单链表），直到最后一个结点
为止，则实现了链表的逆置，如图所示。
Data Structure & Algorithms37

More Applications
•数制转换
•非负十进制数转换成其它进制的数的一种简单方法
Data Structure & Algorithms38
例：十进制转换成八进制  (3553)
10 
= (6741)
8
11
4
1
4
7
1
4
7
6
Empty
n = 3553
n%8 = 1
n/8 = 444
n = 444
n%8 = 4
n/8 = 55
n = 55
n%8 = 7
n/8 = 6
n = 6
n%8 = 6
n/8 = 0
n = 0

Applications
•表达式求值
•中缀表达式 
            23+(34*45)/(5+6+7)
•后缀表达式 
            23 34 45 * 5 6 + 7 + / +
Data Structure & Algorithms39
4 + 2 x 3 - 10 / 5
51 x (24 - 15 / 3) + 6
四则运算规则：
先乘除，后加减，先括号内，后括号外。
同一运算级别，从左到右。

表达式的语法定义
•中缀表达式的语法公式               后缀表达式的语法公式
Data Structure & Algorithms40

Applications
•Symbol Priority
Data Structure & Algorithms41
+-*/()#
+>><<<>>
->><<<>>
*>>>><>>
/>>>><>>
(<<<<<=
)>>>>>>
#<<<<<=
θ
1
θ
2
Compare
θ
1
θ
2
and

Applications
•中缀表达式求值
Data Structure & Algorithms42
•1：运算数栈置空，操作符栈压入算符“#”
•2：依次读入表达式的每个单词
•3：如果是运算数，压入运算数栈
•4：如果是操作符，将操作符栈顶元素θ
1
与读入的操作符θ
2 
进行优先级比较
•4.1 如果栈顶元素优先级低，将θ
2 
压入操作符栈
•4.2 如果相等，弹出操作符栈
•4.3 如果栈顶元素优先级高，弹出两个运算数，一个操作符，进行计算，
并将计算结果压入运算数栈，重复第4步的判断
•5:直至整个表达式处理完毕

Applications
Data Structure & Algorithms43
•3*(7-2)#
步骤    操作符栈  运算数栈    输入字符         操作
1           #                                   3*(7-2)#       压入“3”
2           #                 3                  *(7-2)#       压入“*”
3           #*               3                  (7-2)#         压入“(”
4           #*(              3                  7-2)#          压入“7”
5           #*(              37                -2)#            压入“-”
6           #*(-             37                2)#             压入“2”
7           #*(-             372              )#               弹出“-”压入7-2
8           #*(              35                )#                弹出“(”
9           #*               35               #                  计算3*5
10         #                 15               #           操作符栈空，结束

Applications
•中缀表达式求值
Data Structure & Algorithms44
•1：运算数栈置空，操作符栈压入算符“#”
•2：依次读入表达式的每个单词
•3：如果是运算数，压入运算数栈
•4：如果是操作符，将操作符栈顶元素θ
1
与读入的操作符θ
2 
进行优先级比较
•4.1 如果栈顶元素优先级低，将θ
2 
压入操作符栈
•4.2 如果相等，弹出操作符栈
•4.3 如果栈顶元素优先级高，弹出两个运算数，一个操作符，进行计算，
并将计算结果压入运算数栈，重复第4步的判断
•5:直至整个表达式处理完毕

Applications
•中缀后缀表达式的转换
Data Structure & Algorithms45
4+3*5                              4,3,5 *+
2*(5+9*4/2)+6*5          2,5,9,4*2/+*6,5*+
中缀后缀
中缀表达式中，操作符的出现次序与计算顺序不一致；
后缀表达式中，操作符的出现次序就是计算次序。

Applications
Data Structure & Algorithms46
A+B*C-D#                          ABC*+D-
读到的符号           操作符栈         输出序列
    A                              #                     A
    +                              #+                   A
    B                              #+                   AB
    *                               #+*                 AB
   C                              #+*                 ABC
    -                              #-                   ABC*+
   D                              #-                  ABC*+D
   #                               #                   ABC*+D-
+-*/()#
+>><<<>>
->><<<>>
*>>>><>>
/>>>><>>
(<<<<<=
)>>>>>>
#<<<<<=
•中缀转后缀表达式

Applications
•Conversion algorithm
Data Structure & Algorithms47
•1：操作符栈压入算符“#”
•2：依次读入表达式的每个单词
•3：如果是运算数，则输出
•4：如果是操作符，将操作符栈顶元素θ
1 
与读入的操作符θ
2 
进行优先级比较
–4.1 如果栈顶元素优先级低，将θ
2 
压入操作符栈
–4.2 如果相等，弹操作符栈
–4.3 如果栈顶元素优先级高，弹出栈定元素并输出，重复第4步的判断
•5：直至整个表达式处理完毕

Applications
•后缀表达式求值
Data Structure & Algorithms48
4+3*5                         4,3,5 *+
2*(5+9*4/2)+6*5     2,5,9,4*2/+*6,5*+
求解算法：
•设定一个运算数栈OPND; 
•从左向右依次读入，当读到的是运算数，将其加入到运算数栈中；
•若读入的是操作符，从运算数栈取出两个元素，与读入的操作符进行
运算，将运算结果加入到运算数栈。
•直到表达式的最后一个操作符处理完毕。

Summary
• Definition and operations
•后进先出的顺序表
• Implementation
•顺序栈
•链栈
• Applications
•中缀和后缀表达式求值
•表达式转换
Data Structure & Algorithms49
