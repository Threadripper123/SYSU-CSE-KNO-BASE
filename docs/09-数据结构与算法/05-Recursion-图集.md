# 05-Recursion-图集

> 主题库：数据结构与算法　｜　源文件：05-Recursion-图集.pdf　｜　8,105 字符　｜　37 个片段　｜　更新于 2026-11-11
4! 3! 2! 1! 0! 4*3! 3*2! 2*1! 1*0! 0! =24 =6 =2 =1 =1

factorial(7)  ) = =   factorial(6)  factorial(5)       factorial(4)   =   factorial(3) =     3         factorial(2)  2 factorial(1) =      2  1 = 5040

Yntfactorielint) woid msin() Yntfactyny printfEnter any positiweinteger:") seanfgd &n) fact= factorial(n) factorial(3); printf( Factorial of % d is % d7 n,fact); Yntfactoria intn) E int tempr if(n==0) tem p = n * factorial( n-1 ) 3*factorial( 2 ) ; return temp; int factorial(int n ) I E int temp; if(n==o) return 1; else 2factorial(1) temp =n *factorial(n-1 ) return temp; int factorial(intn) E int tem p; if(n==0) return 1; 1*fectorial(0); else temp = n *factorial(n-1 ) return temp; intfactorial(intn) E int tem p; return 1 temp=nfactorialn-1 returntem

int factorial(int n) / Pre: nis a nonnegative integer. Post: Return the value of the factorial ofn. */ if(n == 0) return 1; else return n*factorial(n — 1);

4!=4×3! = 4×(3×2!) =4×(3×(2×1!)) =4×(3×(2×(1×0!))) = 4×(3×(2×(1×1))) =4×(3×(2×1)) =4×(3×2) =4×6 = 24.

2 3

void move(intcount,intstart,int finish,int temp); Pre: There are at least count disks on the tower start. The top disk(if any)on each of towers temp and finish is larger than any of the top count disks on tower start. Post: The top count disks on start have been moved to finish; temp(used for temporary storage)hasbeenreturned

void move(int count,int start, int finish, int temp) if (count> 0){ move(count— 1,start,temp,finish); cout <"Move disk"< count<<"''from " start "to"<finish ""<end move(count — 1,temp, finish,start);

Find the key step. Begin by asking yourself,"How can this problem be divided into parts?" or"How will the key step in Find a stopping rule. This stopping rule is usually the small, special case that is trivial or easy to handle without r Outline your algorithm. Combine the stopping rule and the key step, using an if statement to select between them. Check termination. Verifythat therecursion willalways terminate.Be sure thatyour algorithm correctlyhandles ex- tremecases.

Time M A

M C

Start M Finish B C D

move(3,1,3,2) move(2,1,2,3) move(2,2,31) move(1,3,2,1) move (1, 1,3, 2) move (1, 2, 1, 3) move (1,1,3,2) move(0,1,2,3) move(0,3,1,2) move(0,2,3,1) move(0,1,2,3) move(0,2,3,1) move(0,1,2,3) move(0,3,1,2) move(0,2,3,1)

D D Stack B C C C D D D Space A A A A A A A D D for D D D cata M M M M M M M M M M M M M M M

if(n == 0)return 1; else return n * factorial(n — 1);

n! (n−1)! (n−2)! 21 11 O!

int factorial(intn) intcount, product=1; for(count=1;count<=n;count++) product =count; returnproduct

Eaample factorial(5) = 5 factorial(4) n! = 5 *(4 *factorial(3)) (n-1)! =5*(4*(3÷factorial(2))) = 5*(4 *(3 (2 factorial(1)))) (n-2)! =5 *(4 (3 (2*(1 factorial(0))))  =5÷(4*(3*(2*(11))) =5*(4*(3*(2*1))) 21 =5*(4*(3*6)) 11 =5*(4*6) =5*24 0! =120.

Gnanonacc nn /* fibonacci: recursiveversion Pre: The parameternisanonnegative integer. Post: The functionreturns the nthFibonaccinumber. */ if(n<=0) return0; else if(n == 1) return 1; else return fibonacci(n — 1)+ fibonacci(n — 2);

F6 F5 F5 F4 F3 F3 F2 F3 F2 F2 F1 Fo F4 F3 F1 F2 F1 Fo F1 Fo O 2 1 F1 F0 F1 F0 F3 F2 2 F1 F1 Fo F1 Fo $F2 0 F1 F1 Fo

lafibonacci:iterativeversion Pre: The parametern is a nonnegative integer. Post: Thefunctionreturns thenthFibonaccinumber. */ E int lastbut_one; II second previousFibonaccinumber, Fi-2 intlast_value; I previousFibonaccinumber,Fi-1 int current; II currentFibonacci numberF if(n<= 0)returnO; else if(n == 1)return 1; last_but_one=0; last_value=1; for(inti=2;i<=n;i++){ current=last_but_one +last_value; last_but_one=last_value; last_value= current; return current; 子

void m ove(int count, int start, int finish, int temp) l move:iterativeversion Pre: Diskcount isa valid disk to be moved. Post: Moves count disks from start to finish using tem p for temporary storage. */ E int swap; II temporary storage to swaptowers while (count > 0) { II Replace the if statement with a loop. move(count — 1,start,temp,finish); II first recursive call cout "Move disk"< count "from"< start <"to"<finish<""< endl; count —— ; II Change parameters tomimic the second recursive call. swap=start; start=temp; temp=swap; 子

x Deadlend

solve_from(Queens configuration) ifQueens configuration already containseightqueens for every chessboard square p that is unguarded by configuration { add a queen on square p to configuration; solve_from(configuration); remove thequeen from squarepofconfiguration;

class Queens { Queens(intsize); bool is_solved() const; void print() const; boolunguarded(intcolconst void insert(int col); int board_size; Il dimension of board = maximum number of queens cYa naG int count; Il current number of queens = first unoccupied row bool queen_square[max_board][max_board];

vaidsolve_from Queens &configuration / Pre: The Queensconfiguration representsa partially completed arrangement of Post: All n -queens solutions that extend the given configuration are printed. The configurationisrestoredtoitsinitialstate. Uses: Theclass Queens and the function solve_from, recursively. * if(configuration.is_solved())configuration.print(); ElSE for(intcol=0;col<configuration.board_size;col++) Yaonfiguration.unguarded(col configuration.insert(col); solve_from (configuration); II Recursively continue to add queens. configuration.remove(col);

int main) Post: All solutions to the n-queens puzzleforthe selected board size are printed. Uses: Theclass Queensand the recursive function solve_from. */ E intboard_size; print_information() cout "What isthe sizeofthe board?" flush; cin > board_size; if(board_size 0llboard_size>max_board) cout <"Thenumbermust be betweenOand" maxboard endl; EISEL Queensconfiguration(board_size); IIInitializeempty configuration. solve_from(configuration); l Find allsolutions extending configuration. 子

void Queens::insert(intcol) / Pre: The square inthe first unoccupiedrow(rowcount)and column colis not guarded byanygqueen Post: Aquieen has been inserted into the square at row-count and column col; count has beeninonemehted by 1./ queen_square[count++][col]=true;

boolQueens::unguarded(intcol)const /* Post: Returns true orfalse according as the square in the first unoccupied row (row count)and column col is not guarded by any queen.*/ inti; bool ok = true; II turns false if we find a queen in column or diagonal for(i=O;ok&&i<count;i+ 十) ok =!queen_square[i][col]; I Checkupper partofcolumn for(i=1;ok&&count —i>=0&&col —i>=0;i++) ok =!queen_square[count —i] [col —i]; II Check upper-left diagonal for(i=1;ok&&count—i>=0&&col+i<board_size;i+十) ok =!queen_square[count —i][col+i]; II Check upper-right diagonal

2 5 8 8 68 R 18 69 0 3中 28 4 48 68 只7 305050 32 5 204 8 56 10 51 202 70 7 07 704 07 5ó Solution 7ó  Solution

12.试将下列递推过程改写为递归过程。  void ditui(int n) int i; _=n; while(i>1) printf("d",i--);

13.试将下列递归过程改写为非递归过程 E void test(int&sum) { int x; scanf(x); if(x==0)Sum=0; else{ test(sum); sum+=x; } printf(sum);

12．该递推过程可改写为下列递归过程。 void digui(int j) { if(j>1) digui(j-1); 该递归过程是“尾递归”过程，因此可以不设栈，而通过直接改变过程中的参数值， 利用循环结构代替递归调用。 13．递归算法test的功能实际上是不断读入整数，并逆向求和。要将其改为非递归，可利用堆栈实现，先将输入数据压入堆栈，然后弹栈并依次求和。改写后的递归算法如下： void testi(int &sum) { stacks; intx; scanf(x); initstack(s); while(x) { push(s,x); scanf(x); sum=0; printf(sum); while(pop(s,x)) { sum+=x; printf(sum); }

算法2非递归解法输入：n； 输出：res; 1:function F(n) 2: ifn==1then 3: return 1; 4: elseif n == 2then 6: endif 7: a=1,b= 2,res = 0; 8: fori=3→ n do 9: res=a+b; 10: a=b; 11: b= res; 12: end for 13: return res; 14: end function

算法3递归解法输入：n； 输出：res； 1:function F(n) 2: ifn==1then return1; elseifn==2then 4: 5: return2; 6: endif 7: returnF(n−1)+ F(n−2); 8:endfunction

(1)递归算法 { if(m==0)return(n+1); else if(n==0)returnakm(m-1,1); elsereturn akm(m-1,akm(m, n-1)); (2)非递归算法 typedef struct{ intmval;intnval; stack;

intamintmintn { //S[max]为附设栈，top为栈顶指针 cYanaa S[top].mval = m; S[top].nval = n; do{ while(S[top].mval) while(S[top].ncal) { top++;S[top].mval=S[top-1].mval; S[top].nval=S[top-1].nval-1; S[top].mval--;S[top].nval=1; cYa { top-; S[top].mval--; S[top].nval =S[top+1].nval +1; }while(top !=1 |l S[top].mval I= 0); top-; return (S[top+1].nval+1);
