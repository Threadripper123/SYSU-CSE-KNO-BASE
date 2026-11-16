# 05-Recursion

> 主题库：数据结构与算法　｜　源文件：05-Recursion.pdf　｜　10,321 字符　｜　63 个片段　｜　更新于 2026-11-11


Recursion
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

Recursion
•递归的定义
•子程序（或函数）直接调用自己或通过一系列调用语句间接调用自己，是
一种描述问题和解决问题的基本方法。 
•递归的基本思想
•问题分解：把一个不能或不好解决的大问题转化为一个或几个小问题，再
把这些小问题进一步分解成更小的小问题，直至每个小问题都可以直接解
决。 
•递归的要素
•递归边界条件：确定递归到何时终止，也称为递归出口；
•递归模式：大问题是如何分解为小问题的，也称为递归体。 
Data Structure & Algorithms2

Example: Factorials
•In Mathematics:
Base case
（递归基）
General case that 
reduces to simpler 
case（递归步）
An algorithm is recursive (递归的) if it calls 
itself (directly or indirectly) to do part of its 
work. 

Example: Factorials
•阶乘 n! 的递归定义如下：
Data Structure & Algorithms4
10
()
(1)0
n
fn
nfnn






long factorial (long n)
{
     int temp;
     if (n==0) 
           return 1;
     else  
     {  
          temp=n*factorial(n-1);
          return temp;
      }
}

Example: Factorials
•程序执行展示
Data Structure & Algorithms5

+: concise and elegant
-: can require keeping track of many 
partial computations

The process of recursive call

Every recursive process consists of two parts:
1.A smallest, base case that gives a solution;
2.A general method that reduces a particular case 
to one or more of the smaller cases, reducing 
the problem all the way to the base case 
eventually.

The tower of Hanoi
Task: move the tower from 1 to 3 using 2 as 
temporary storage.
Rules:  1. move only one disk at one time;
            2. No larger disk can be placed on a   
                  smaller disk.

We have to get to this state and then move 
the bottom disk
Divide and conquer(分治法）
1
2
3

1
2
3
A similar problem

213
Solved by 
recursion

The Solution--Recursive Function
The base case is count == 0

Designing recursive algorithms

Tree of function calls(函数调用树) 
Tree of function calls is a tool to analyzing the process of 
function calls.  
•A node(结点) denotes a function call with the function 
name;
•A function may invokes other functions, which are 
denoted as children;
•Different recursive calls appear as different vertices 
with the same name;
•Recursion tree: the recursive part of the tree of function 
calls.



invoke the 
function
Return from 
the function
Meaning of the tree of 
function calls: the 
traversal of the tree is 
the trace of the function 
calls

Tree of function calls and the 
execution order
Recursive call
Execution 
order

Recursion tree for three disks
Number of nodes is 
the number of disk 
movements. 

The number of inner nodes:
 1+2+2
2
+ ... +2
63 
= 2
64 
– 1 ≈ 1.6 × 10
19
It will take more than 500 billion (五千亿年）years to 
finish the task suppose one can move one disk every 
second.  
Note also that our program also produces the best 
possible solution.
The number of moves  to solve Hanoi for 
64 disks?

Recursion and Stacks
•Recursive calls are implemented by using stacks;
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
When a program calls a subprogram, the system:
•Store  returning location and parameters;
•Allocate storage for  all the local variables.
•Then the control is passed to the subprogram.
The information about the returning address and 
parameters is stored in the activation record(活
动记录） for the function call.

When the subprogram is finished, 
•Return the result of the subprogram;
•Release the storage allocated for the subprogram; 
•the control is passed to the caller and execution 
starts at the returning address.

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

Activation record for 
the main program M
Stack frames(运行栈): each column shows the stack contents at a given time:
D calls itself

Analyzing space and time complexity
What correspond to time 
complexity?

3
Time complexity: 3 time units 
+
A’s and D’s time units
time complexity: count 
the total time units in 
the function call tree.
3
1
2
2
1
The time 
complexity is 
proportional to 
the number of 
nodes. 
Space 
complexity: 
count the 
height of 
the call tree.

Complexity of factorial 
Space complexity：space used by vars 
and activation records = O(n)
Time complexity：
 T(0) = 1
 T(n)= 1 + T(n-1) = O(n)     (recurrence 
relation 递推关系） 

Complexity of Hanoi
Counting on the recursive tree:
S(n)   = O(n)
T(n) = O(2
n
)
Or using recurrence relation:
T(n)= T(n-1) + 1+T(n-1)      
T(0) = 0 
T(n) = O(2
n
)  

What we learn about recursion
•A recursive algorithm must have a base case, which gives a 
direct solution, and a  method which reduces general cases to 
simpler cases. 
•Recursion is done by using stacks:
–When a call is made, the activation record is pushed into the 
stack and control is passed to the called function;
–When the call is finished, the activation record is popped off 
the stack and execution starts from the address in the popped 
record. 
•The recursive tree of a recursive algorithm  can be used to 
analyze the algorithm: 
–The space complexity is proportional to the height of the 
recursive tree;
– The time complexity is proportional to the number of nodes.

Recursive or Nonrecursive
Factorials
Recursive version
Nonrecursive 
version

The recursive version is 
more expensive in space 
and time 



Recursion tree for F
7
F
5 
is repeated
F
3 
is repeated 5 times

Using Iteration 
（迭代） 
 
Nonrecursive 
version
Also known as dynamic programming
(动态规划)

•Space complexity for recursive version is linear in  n.
•Time complexity for the recursive version is 
exponential with n (>= 2
n/2
).
•Time complexity for the nonrecursive version is linear 
in n.
•The space complexity for the nonrecursive version: it  
uses only 4 variables.

Hanoi with tail recursion
Last executed statement is a 
recursive call

Hanoi without tail recursion

When to use recursion
•If time and space is not an issue, then 
recursive version is much easier to understand 
and preferred.
•If duplicate tasks are involved, then data 
structure other than stacks will be appropriate.
•Recursion can always be replaced by iteration 
and stacks.  But no point to do it if not 
necessary.

Backtracking(回溯）
A method to try all possibilities using recursion.
When there are several possibilities, 
•take one and go on;
•go back to the most recent choice, and try another 
possibility when a dead end is reached. 

Eight Queens Puzzle

Four Queens Solution
guarded
Take 1 of the 4 legitimate positions
backtrack
backtrack
Dead end
backtrack

Backtracking
•Backtracking is a recursive method that goes back to 
the most recent choice and tries another possibility.
•In (b), we go back to row 2 and put the queen in 
column 4 instead.
•In (c) , there is no other possibilities in row two. We go 
back to row 1 and put the queen in column 2.   

Outline of the solution
•Imagining  using a class called Queens to represent a 
partial configuration(格局, 状态） of queens;
•Add a queen in the configuration and continue the 
search from the new configuration;
•If this does not lead to a solution, then remove the 
newly added queens and try another position and 
continue the search until a solution is reached. 

When it is returned here,  the 
queen added at p is removed 
and  try next possible position. 
Exit: a solution is 
found
Exit: dead end
Outline of the solution

The Queen Class
 (count, col) denotes a  
possible position

The Backtracking Function
(count, col) denotes 
a  possible position

The main program

The position is 
empty
Queens :: Queens(int size)
/* Post: The Queens object is set up as an empty 
configuration on a chessboard with size squares in each 
row and column. */
{
  board_size = size;
  count = 0;
  for (int row = 0; row < board_size; row++)
     for (int col = 0; col < board_size; col++)
          queen_square[row][col] = false;
}
Initializing the chessboard

Inserting a queen
count  is modified 
after insertion.
Occupied 

It is not necessary to check the lower  
part. (?)

Part of the recursion tree for eight queens problem
Effectively pruning 
a recursion tree  
Guarded positions are 
not investigated
Dead end

Analysis of Backtracking
•Effectiveness of backtracking: positions that are 
discovered impossible prevent the later 
investigation of fruitless paths.
•Number of configurations that there is only one 
queen in each row: 8
8
•Number of configurations that there is only one 
queen in each row and each column: 8!
•Backtracking cut this number further: it does not 
investigate impossible paths. See the recursion 
tree. 

Lower Bounds
However, the amount of work done by backtracking 
still grows rapidly with n. 
To place a queen on the first n/4 rows, the minimum 
positions to investigate:
   n(n-3)(n-6)...(n-3n/4) > (n/4)
n/4  
> 2
n 
 (n > 68). 
   
The queen guards at most three 
positions in the next row

Task: Improvement
•The loops in function unguarded  takes 
considerable time when  checking if a position is 
guarded in three directions.
•An improvement is to keep track which columns 
are  guarded using an one dimension bool array, 
which diagonals  are guarded using two bool 
arrays. This make the function unguarded much 
simpler.

Summary 
•Recursion: concept and principles of designing 
recursive algorithms;
•Understand how recursion is implemented in 
machine;
•Analyze time and space complexity using 
recursion trees;
•Divide and conquer, a method to solve problems 
using recursion;
•Backtracking, a method to try all possibilities 
using recursion.

Exercise
Data Structure & Algorithms58

Exercise
Data Structure & Algorithms59

Exercise
•楼梯有n阶台阶，上楼可以一步上1阶，也可以一步上2阶，编一程序计
算共有多少种不同的走法。例如，当n=3时，共有3种走法，即1+1+1，
1+2，2+1。
•请分别给出递归与非递归的实现代码
Data Structure & Algorithms60

Exercise
Data Structure & Algorithms61

Exercise
•已知Ackerman函数的定义如下：
•(1) 写出递归算法
•(2) 利用栈操作写出非递归算法
Data Structure & Algorithms62

Exercise
Data Structure & Algorithms63
