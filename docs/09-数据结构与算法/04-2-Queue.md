# 04-2-Queue

> 主题库：数据结构与算法　｜　源文件：04-2-Queue.pdf　｜　14,416 字符　｜　56 个片段　｜　更新于 2026-11-11


Queues
Yan Pan
E-mail: panyan5@mail.sysu.edu.cn 
School of Computer Science and Engineering
Data Structure & Algorithms

Queue（队列）
Data Structure & Algorithms2
•Definition: A queue is a list in which all additions to the list are 
made at one end, and all deletions from the list are made at 
the other end. Queues are also called first-in, first-out lists, or 
FIFO for short.

Queue（队列）
•先进先出（First In First Out）
•限制访问点的线性表 
•按照到达的顺序来释放元素
•所有的插入在表的一端进行，所有的删除都在表的
另一端进行
•主要元素
•队头（front）
•队尾（rear）
Data Structure & Algorithms
First in first out(FIFO)
A1   A2   A3   A4   A5... ... An
Insertion
Deletion
（front）（rear）
3

应用举例
•队列在日常生活中和计算机程序设计中，有着非常重要的作用，在此，仅举出
两个方面例子来说明它，其它应用在后面的学习中将会遇到。
•第一个例子就是CPU资源的竞争问题。在具有多个终端的计算机系统中，有多
个用户需要使用CPU各自运行自己的程序，它们分别通过各自终端向操作系统
提出使用CPU的请求。
•操作系统按照每个请求在时间上的先后顺序，将其排成一个队列，每次把CPU
分配给队头用户使用，当相应的程序运行结束，则令其出队，再把CPU分配给
新的队头用户，直到所有用户任务处理完毕。
Data Structure & Algorithms4

应用举例
•第二个例子就是主机与外部设备之间速度不匹配的问题。
•以主机和打印机为例来说明，主机输出数据给打印机打印，主机输出数据的速度比打印
机打印的速度要快得多，若直接把输出的数据送给打印机打印，由于速度不匹配，显然
是不行的。
•所以解决的方法是设置一个打印数据缓冲区，主机把要打印输出的数据依此写如到这个
缓冲区中，写满后就暂停输出，继而去做其它的事情。
•打印机就从缓冲区中按照先进先出的原则依次取出数据并打印，打印完后再向主机发出
请求，主机接到请求后再向缓冲区写入打印数据，这样利用队列既保证了打印数据的正
确，又使主机提高了效率。 
Data Structure & Algorithms5

应用举例
•第三个例子是电子商务系统中的缓冲队列。
•在电子商务网站的订单输入系统中，峰值订单率可能是平均订单率的两倍到三倍。
•由于订单输入系统必须具有处理峰值负载的能力，因此大数据量处理能力在很大一部分
时间内都是闲置的。
•如果订单输入系统中的发货任务用队列缓冲（即并不实时处理），则发货系统就无须具
有处理峰值订单负载的能力。
•如果在出现峰值时对异步任务进行排队，并在空闲时执行（异步相对同步而言，异步任
务指接收到但并不用立即处理的事物），将显著提高系统的处理效率。
Data Structure & Algorithms6

Queue ADT
•Like a stack, a queue is also a list. However, with a 
queue, insertion is done at one end, while deletion is 
performed at the other end.
•Accessing the elements of queues follows a First In, 
First Out (FIFO) order.
–Like customers standing in a check-out line in a store, the 
first customer in is the first customer served.

Enqueue and Dequeue
•Primary queue operations: Enqueue and Dequeue
•Like check-out lines in a store, a queue has a front 
and a rear. 
•Enqueue – insert an element at the rear of the 
queue
•Dequeue – remove an element from the front of 
the queue
Insert 
(Enqueue)
Remove
(Dequeue)
rearfront

Implementation of Queue
•Just as stacks can be implemented as arrays or 
linked lists, so with queues.
•Dynamic queues have the same advantages over 
static queues as dynamic stacks have over static 
stacks

Queue Implementation of Array
•There are several different algorithms to 
implement Enqueue and Dequeue
•Naïve way
–When enqueuing, the front index is always fixed 
and the rear index moves forward in the array.
front
rear
Enqueue(3)
3
front
rear
Enqueue(6)
3
6
front
rear
Enqueue(9)
3
6
9

Queue Implementation of Array
•Naïve way (cont’d)
–When dequeuing, the front index is fixed, and the 
element at the front the queue is removed. Move 
all the elements after it by one position. 
(Inefficient!!!)
Dequeue()
front
rear
6
9
Dequeue()
Dequeue()
front
rear
9
rear = -1 
front

Queue Implementation of Array
•A better way
–When an item is enqueued, the rear index moves 
forward.
–When an item is dequeued, the front index also 
moves forward by one element 
XXXXOOOOO   (rear) 
OXXXXOOOO   (after 1 dequeue, and 1 enqueue)
OOXXXXXOO   (after another dequeue, and 2 enqueues)
OOOOXXXXX   (after 2 more dequeues, and 2 enqueues)
(front)
The problem here is that the rear index cannot move beyond the 
last element in the array.

Implementation using Circular Array
•Using a circular array
•When an element moves past the end of a circular 
array, it wraps around to the beginning, e.g.
–OOOOO7963  4OOOO7963 (after Enqueue(4))
–After Enqueue(4), the rear index moves from 3 to 4.
•How to detect an empty or full queue, using a 
circular array algorithm? 
–Use a counter of the number of elements in the queue.

Queue Implementation of Circular Array
class Queue {
public:
Queue(int size = 10);// constructor
~Queue() { delete [] values; }// destructor
      Queue(const Queue &rhs);
      const Queue & operator(const Queue &rhs)
bool empty(void)const;
bool full(void)const;
bool push(T x); //enqueue
bool pop(T & x);//dequeue
      const T& head()const;//return the front element
 
private:
int front;// front element index
int rear;// rear index
int counter;// number of elements
int maxSize;// size of array queue
T* values;// element array
};

Queue Class
•Attributes of Queue
–front/rear: front/rear index
–counter: number of elements in the queue
–maxSize: capacity of the queue
–values: point to an array which stores elements of the 
queue
•Operations of Queue
–empty: return true if queue is empty, return false otherwise
–full: return true if queue is full, return false otherwise
–push: add an element to the rear of queue
–pop: delete the element at the front of queue

Create Queue
•Queue(int size = 10)
–Allocate a queue array of size. By default, size = 10.
–front is set to 0, pointing to the first element of the 
array
–rear is set to -1. The queue is empty initially.
Queue::Queue(int size /* = 10 */) {
values=new T[size];
maxSize=size;
front=0;
rear=-1;
counter=0;
}

empty & full
•Since we keep track of the number of elements 
that are actually in the queue: counter, it is 
easy to check if the queue is empty or full.
bool Queue::empty() const{
if (counter)return false;
elsereturn true;
}
bool Queue::full() const{
if (counter < maxSize)return false;
elsereturn true;
}

Enqueue
bool Queue::push(T x) {
if (full()) {
return false;
}
else {
// calculate the new rear position (circular)
rear= (rear + 1) % maxSize; 
// insert new item
values[rear]= x;
// update counter
counter++;
return true;
}
}

Dequeue
bool Queue::pop(T & x) {
if (empty()) {
 
return false;
}
else {
// retrieve the front item
x= values[front];
// move front 
front= (front + 1) % maxSize;
// update counter
counter--;
return true;
}
}

Using Queue
int main(void) {
Queue queue(5);
cout << "Enqueue 5 items." << endl;
for (int x = 0; x < 5; x++)
queue.push(x);
cout << "Now attempting to enqueue again..." << endl;
queue.push(5);
T value;
queue.pop(value);
cout << "Retrieved element = " << value << endl;
queue.push(7);
return 0;
}

Exercises
•假设Q[0...10]作为一个队列，初始状态为front=rear=0，分别画出在非循
环队列和循环队列做完以下操作后，队列的头尾指针的状态变化情况，
若不能进队，请指出其元素，并说明理由。
•(1) d, e, b, g, h 进队
•(2) d, e 出队
•(3) i, j, k, l, m 进队
•(4) b 出队
•(5) n, o, p 进队
Data Structure & Algorithms21

Exercises
•非循环队列进队和出队的变化情况
Data Structure & Algorithms22

Exercises
•循环队列进队和出队的变化情况
Data Structure & Algorithms23

Exercises
•设循环队列的容量为40（序号从0到39），现经过一系列的入队和出队
运算后，有
•  ① front=11，rear=19;   
•  ② front=19，rear=11；
•问在这两种情况下，循环队列中各有元素多少个？ 
Data Structure & Algorithms24
可直接使用队列长度计算公式： (N＋r－f)% N，其中N为队列容量40，r，f分
别为尾指针、头指针所指向元素，则可以得出：
1）L =（40＋19－11）% 40 = 8
2）L =（40＋11－19）% 40 = 32

循环队列 vs. 非循环队列
•循环队列一定优于非循环队列吗？什么情况下使用非循环队列？
•队列主要是用于保存中间数据，而且保存的数据满足先产生先处理的
特点。非循环队列可能存在数据假溢出，即队列中还有空间，可是队
满的条件却成立了。为此，改为循环队列，这样克服了假溢出。
•但并不能说循环队列一定优于非循环队列，因为循环队列中出队元素
的空间可能被后来进队的元素占用，如果算法要求在队列操作结束后
利用进队的所有元素实现某种功能，这样循环队列就不适合了，这种
情况下需要使用非循环队列。
Data Structure & Algorithms25

Problem: boundary conditions
Data Structure & Algorithms26

Problem: boundary conditions
Data Structure & Algorithms27
No 
difference 

Deal with boundary conditions
Data Structure & Algorithms28
•A circular array with front and rear indices and one position left vacant.
•A circular array with front and rear indices and a Boolean flag to indicate 
fullness (or emptiness).
•A circular array with front and rear indices and an integer counter of entries.
•A circular array with front and rear indices taking special values to indicate 
emptiness.

Queue Implementation based on Linked List
class Queue {
public:
Queue() {// constructor
front = rear = NULL;
counter= 0;
}
~Queue() {// destructor
T value;
while (!empty()) Dequeue(value);
}
        //add big three
bool empty()const {
if (counter) return false;
else    return true;
}
void push(T x);
bool pop(T & x);
private:
Node* front;// pointer to front node
Node* rear;// pointer to last node
int counter;// number of elements
};

Enqueue
void Queue::push(T x) {
Node* newNode=new Node;
newNode->data=x;
newNode->next=NULL;
if (empty()) {
front=newNode;
rear=newNode;
}
else {
rear->next=newNode;
rear=newNode;
}
counter++;
}
8
rear
rear
newNode
5
58

Dequeue
bool Queue::pop(T & x) {
if (empty()) {
return false;
}
else {
x=front->data;
Node* nextNode=front->next;
delete front;
front=nextNode;
counter--;
}
}
8
front
5
583
front

Array Implementation vs. Linked Implementation
•Array Queue
•Fixed memory space
•easy to visit the elements
•Linked queue
•Flexible to manage arbitrary length of data 
•Not easy to randomly visit the elements
Data Structure & Algorithms32

Variants of queues
Data Structure & Algorithms
输入受限的队列：限定在一端进行输入，可以在两端进行
删除的队列。
......
输出受限的队列：限定在一端进行输出，可以在两端进行
加入的队列。
......
33

Priority Queue (优先队列)
•普通的队列是一种先进先出的数据结构，元素在队列尾追加，从队列头删除。
在优先队列中，每个元素都有一个优先级。具有最高优先级的元素最先出队。
优先队列具有最高进先出（largest-in，first-out）的行为特征。
•对优先级队列执行的操作有查找、插入一个新元素和删除三种。
•优先队列和普通队列都适用在处理或服务对象需要按序逐个进行操作的场合，
不过优先队列关心对象的优先级，将队列按优先级顺序做出队处理而已。
•例子：
•操作系统的任务调度
•多媒体通讯网络中的数据包调度
•集合中的元素搜索
Data Structure & Algorithms34

Priority Queue (优先队列)
•对操作系统而言，当有多个任务需要处理的时候，一般而言，解决这个问题的简单策略就
是把任务放在一个队列中，先来者先服务，这是公平的，虽然对每个任务本身来说，都期
望是能够被立即执行而不仅仅是被执行。
•实际中会出现提交一个执行时间很短的任务可能要等很长的时间才能被执行的情形，因为
在它之前已经有许多执行时间很长的任务，在队列中等待执行，这样会导致用户的使用感
受不好。
•解决此问题的一个策略是利用优先队列，按照“任务需要执行的时间越短，它的优先权越
高”的原则，按任务的轻重顺序执行，可以使用户的平均等待时间最短。
Data Structure & Algorithms35

Applications
•只要满足先来先服务特性的应用均可采用队列作为其数据
组织方式或中间数据结构。 
•调度或缓冲
•消息缓冲器 
•邮件缓冲器 
•计算机的硬设备之间的通信也需要队列作为数据缓冲 
•操作系统的资源管理 
•广度优先搜索
Data Structure & Algorithms36

Application of Queues: Breadth First Search
•Breadth-first search (BFS) is a strategy for 
searching in a graph when search is limited to 
essentially two operations: 
•(a) visit and inspect a node of a graph; 
•(b) gain access to visit the nodes that neighbor 
the currently visited node. 
•The BFS begins at a root node and inspects all 
the neighboring nodes. Then for each of those 
neighbor nodes in turn, it inspects their 
neighbor nodes which were unvisited, and so 
on. 
Data Structure & Algorithms37

Application of Queues: Breadth First Search
n为了实现逐层访问, 算法中使用了一个队列, 以记忆正在访
问的这一层和上一层的顶点, 以便于向下一层访问。
n为避免重复访问, 需要一个辅助数组。
 visited [ ]，给被访问过的顶点加标记。
Data Structure & Algorithms38

Application of Queues: Breadth First Search
Data Structure & Algorithms
template<class Type> 
void BFS ( Graph <Type>& G, int v ) {
    int n = G.NumberOfVertices( );
    int * visited = new int[n];
    for ( int i = 0; i < n; i++ ) 
         visited[i] = 0;
    cout << G.GetValue (v) << ' ';   
     visited[v] = 1;
    Queue<int> q;
    // 进队列    
    q.EnQueue (v); 
    ...
    delete [ ] visited;     
}
while ( !q.IsEmpty ( ) ) {      // 队空搜索结束
        q.GetFront(v);  q.DeQueue ( ); 
        int w = G.GetFirstNeighbor (v);  
        while ( w != -1 ) {   // 若邻接顶点 w 存在
   if ( !visited[w] ) {     // 未访问过
       cout << G.GetValue (w) << ‘ ’;         
       visited[w] = 1;   q.EnQueue (w);    
            }
             w = G.GetNextNeighbor (v, w);
        }        // 重复检测 v 的所有邻接
    }    // 外层循环，判队列空否
39

Application of Queues: Palindrome (回文)
•A palindrome is a word, phrase, number, or other sequence of 
symbols or elements that reads the same forward or reversed.
•For example, “abba” and “abcba” are palindromes, but “abcde” 
or “ababab” are not palindromes.
•Please write an algorithm to check whether a string is a 
palindrome.
Data Structure & Algorithms40

Application of Queues: Palindrome
•Basic idea: Use “stack” and “queue” simultaneously.
Data Structure & Algorithms41

Application of Queues: Dual Stack
•利用两个栈S1和S2模拟一个队列，写出入队和出队的算法 (可用栈的基
本操作)。(某年研究生入学试题12分)
Data Structure & Algorithms42

Application of Queues: Dual Stack
•Algorithm overview
Data Structure & Algorithms43

Application of Queues: Dual Stack
Data Structure & Algorithms44

Application of Queues: Dual Stack
Data Structure & Algorithms45

Application of Queues: Simulation of an Airport
•Simulation is the use of one system to imitate the behavior of 
another system. 
•A computer simulation is a program to imitate the behavior of 
the system under study.
Data Structure & Algorithms46
GAME: Airport Mania (疯狂机场)

Application of Queues: Simulation of an Airport
•The same runway is used for both landings and takeoffs. （机场只有一条跑道，同
一条跑道既用作起飞、也用作降落）
•One plane can land or take off in a unit of time, but not both. （某一个单位时间可
以有一辆飞机起飞或降落但不能两辆飞机同时起飞或降落）
•A random number of planes arrive in each time unit.（某个单位时间降落的飞机数
量随机产生）
•A plane waiting to land goes before one waiting to take off.（等待降落的飞机位于
等待起飞的飞机之前）
•The planes that are waiting are kept in queues landing and takeoff, both of which 
have a strictly limited size.（飞机排队等待降落及起飞，且队列有长度限制）
Data Structure & Algorithms47

Simulation procedure
•For each time unit in the given simulation time interval: 
    1. Generate random number of arriving planes and taking off planes. 
    2. For each arriving plane, put the plane into the landing queue if the 
landing queue is not full, otherwise, refuse (direct it to another airport);
    3. For each taking of plane, put it into taking off queue;
    4. Taking care of the runway: if the landing queue is not empty, let one land. 
Otherwise, if the taking off queue is not empty, let one take off. Otherwise, let 
the runway idle. 
Data Structure & Algorithms48

Poisson values
Data Structure & Algorithms49
•A discrete random variable X is said to have a Poisson distribution with 
parameter λ > 0, if, for k = 0, 1, 2, ..., the probability mass function of X is 
given by:

Simulation: the main function
Data Structure & Algorithms50
\\ 降落
\\ 起飞

Snake Game (贪吃蛇)
•利用队列这个数据结构编写一个简单的贪吃蛇小游戏。
•原理分析： 队列的特点就是先进先出（FIFO），我们就是利用他的这个特点来模拟一条
贪吃蛇。我们首先考虑一下这个游戏需要哪些元素，然后怎么用终端模拟这些元素。
•蛇：蛇在终端上的表示就是很多个字符连在一起就是蛇，比如： @@@@ 或者 222222 
都是一条蛇。
Data Structure & Algorithms51

Snake Game (贪吃蛇)
•然而，队列是有队头和队尾之分，那么用它表示的蛇也必须有蛇头和蛇尾。
•这个点很重要，就是需要用队尾表示蛇头，因为队列是先进先出的数据结构，蛇在移动
的时候，他的尾巴要消失，然后需要在移动的方向上加上一段蛇，这样就能使蛇移动。
•在数据结构上来说就是队列的一个元素先出队列，然后再将一个元素入队。
Data Structure & Algorithms52
蛇尾蛇头蛇头蛇尾
蛇头
蛇向下移动尾巴消失
然后在下方添加一段蛇，
这样蛇就向下移动了

Snake Game (贪吃蛇)
•蛇吃到食物的情况：
•看了蛇的移动，那么蛇吃到食物比移动更容易，直接将一段蛇添加到原来的蛇上面就好
了，在数据结构上就是表现为一个元素进入队列的操作。
•如图：蛇向下走一步就可以吃到食物了。
•以上是蛇正常游戏，那么接下来就是游戏结束的条件。
•撞到边框： 边框就用坐标表示，然后在对应的坐标上输出字符。
•撞到自己： 每走一步，就判断头的坐标是否与原来蛇的节点坐标重复，若重复则游戏结束。
Data Structure & Algorithms53
蛇尾
蛇头
食物
蛇尾
蛇头
蛇吃到食物，变成三个节点

Summary
•队列：先进先出（First In First Out）
•限制访问点的线性表 
•按照到达的顺序来释放元素
•所有的插入在表的一端进行，所有的删除都在表的另一端进行
•循环队列(Circular Queue)
•为充分利用向量空间，克服“假溢出”现象。将为队列分配的向量空间看
成为一个首尾相接的圆环，并称这种队列为循环队列。
Data Structure & Algorithms54
