# 04-1-Stack-图集

> 主题库：数据结构与算法　｜　源文件：04-1-Stack-图集.pdf　｜　961 字符　｜　8 个片段　｜　更新于 2026-11-11
文件开女键入面布局品剪切粘贴五号粘贴自复制剪切格式键“213 e  剪贴板键图 4 插对象键入 [仅粘贴文本键入插入表格取消

Top a n an-1 a2 Bottom . a1

top 2 4

voidReverse(LinkList *&L) { LinkList*p=L->next,*q; LinkList*lst=NULL； //定义一个不带头节点的链栈 while(p!=NULL) //将p所指节点进栈 { q=p->next; p->next=lst； //*p节点进栈 lst=P P=QI q=L； //g指向新建链表的尾节点 while(lst!=NULL) //出栈所有节点 { q->next=lst; q=lst; lst=lst->next; q->next=NULL； //尾节点的next域置为NuLL

LinkListReverse1(LinkList L){
//L是带头结点的单链表，本算法将L就地逆置
LNode*p，*r； //p为工作指针，r为p的后继，以防断链
p=L->next； //从第一个元素结点开始
L->next=NULL； //先将头结点L的next域置为NULL
while（p!=NULL){ //依次将元素结点摘下
r=p->next； //暂存p的后继
p->next=L->next； //将p结点插入到头结点之后
returnL;

<表达式>：：=<项>+<项> <项>一<项> <项> <项> ：：= <因子>*<因子> <因 >/<因子> <因子> <因子> ：：= <常数> （<表达式>） <常数> <数字> <数字><常数> <数字> ::= 0|1|2|3|4|5|6|7|8|9

<表达式>：：=<项> <项>+  <项> <项> 一 <项> <项> <因 > <因 > * <因子> <因子>/  <因子> <因子 <常数> <常数>: <数字> <数字><常数> <数字> ::=0|1|2|3|4|5|6|7|8|9

Content caDefinitionand operations Applications 5 1 4 2 3 3 2 4 1 5 Data Structure & Algorithms 2
