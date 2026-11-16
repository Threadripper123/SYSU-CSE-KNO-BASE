# 03-Linked List-图集

> 主题库：数据结构与算法　｜　源文件：03-Linked List-图集.pdf　｜　3,403 字符　｜　27 个片段　｜　更新于 2026-11-11
cat 0325 bat 0200 eat C

Eim ply Limlkedl lrnnlemmenta tiom TNodla declaratiom

List declaratiom.

Acmiomsのm aLimlkellist

Actmiomsoma LimlelList 0 Steaok sruotulc simpl

Finclimg a Iist F osiciom

Findlimg a List F ositiom

Imsertiom M ethocl

Imsertiom imtの 2l Lnnkaeol Lnst

DoublyLinkedLists Nodedef nition Node_entryentry; //datamembers Node<Node_entry>*back, *next; NodeO; /lconstructors Node(Node_entry, Node<Node_entry> *link_back = NULL, Node<Node_entry> link_next=NULL);

List definition template<class List_entry> class List{ protected: int count; mutableintcurrent_position mutable Node<List_entry> *current;

Tnst dletimitnomn

Insertioninto aDoublyLinked List previous following current new_node

Insertion into a DoublyLinked List template<classList entry> Error_code List<List_entry>::insert(int pos, const List_entry & x) Node<List_entry>*new_node, *following, *preceding; if (pos< 0 ll pos > count)return range_error; preceding = NULL; if (count== 0) following= NULL; else{set_position(0); following = current;

ELSEL set_positionpos-1 preceding = current; following= preceding->next; new_node = new Node<List entry>(x, preceding, following); if new node= NULL returnoverflow; if(following!=NULL) following->back =new_node; current new_node; current_position pos; counttt;

2 2 3 3 6

first- ai-1 ai ai+1 first CUrrENt CUTTENT first- ai-1 ai ai+1 first current current==NULL

存储分配方式时间性能空间性能顺序存储结构用一段连 ·查找 ·顺序存储结构需要预分配续的存储单元依次存储 ·顺序存储结构0（1） 存储空间，分大了，浪费， 线性表的数据元素 ·单链表(n） 分小了易发生上溢 ·单链表采用链式存储结 ·插入和删除 ·单链表不需要分配存储空构，用一组任意的存储间，只要有就可以分配， ·顺序存储结构需要平均单元存放线性表的元素元素个数也不受限制移动表长一半的元素， 时间为0(n) ·单链表在线出某位置的指针后，插入和删除时间仅为0(1）

int delx(LinkList *&L,ElemType x) { LinkList*pre=L,*p=pre->next； //pre指向*p的前驱节点 while (p!=NULL & p->data!=x) { pre=p; p=p->next； //pre、p同步后移一个节点 if (p!=NULL) //找到值为x的*p节点 { pre->next=p->next; free(p); return 1; elsereturn0； //未找到值为x的节点

L prepre pre P

p=p->next； //prepre、pre、p同步后移一个节点 if（ （p!=NULL） //找到值为x的*p节点 { prepre->next=p; //删除*pre节点 free(pre); //释放*pre节点空间 return1； //成功删除返回1 elsereturn0； //未找到值为x的节点,返回0

int delxl(LinkList *&L,ElemType x) { LinkList*prepre=L,*pre=prepre->next,*p; if(pre->data==x)return0; //第一个数据节点值为x,返回0; [ while(p!=NULL&&p->data!=x) //查找值为x的*p节点 { prepre=prei

void Del X 1(Linklist &L,ElemType x){ //L为带头结点的单链表，本算法删除工中所有值为x的结点 LNode *p=L->next,*pre=L,*q;//置p和pre的初始值 while(!NULL) if(p->data==x){ a=pi //g指向该结点 pre->next=p; //删除*g结点 free(g)； //释放*g结点的空间 else{ //否则，pre和p同步后移 prep p=p->next;

void Del X 2(Linklist &L,ElemType x){ //L为带头结点的单链表，本算法删除L中所有值为x的结点 LNode*p=L->next,*r=L,*q； //r指向尾结点， 其初值为头结点 while(p!=NULL){ if(p->data!=x）{ //*p结点值不为x时将其链接到L尾部 r->next=p; r=Pi P=p->next； //继续扫描 else{ //*p结点值为x时将其释放 API p=p->next // 扫 free(q)； //释放空间 }//while r->next=NULL; //插入结束后置尾结点指针为NULL

LinkListReverse1(LinkList L){ //L是带头结点的单链表，本算法将L就地逆置 LNode*p，*r； //p为工作指针，r为p的后继，以防断链 p=L->next； //从第一个元素结点开始 L->next=NULL； //先将头结点L的next域置为NULL while（p!=NULL){ //依次将元素结点摘下 r=p->next； //暂存p的后继 p->next=L->next； //将p结点插入到头结点之后 returnL;

LinkList Reverse 2(LinkList L){ //依次遍历线性表L并将结点指针反转 LNode.*pre,*p=L->next,*r=p->next; p->next=NuLL； //处理第一个结点 while（r！=NULL）{ //r为空，则说明p为最后一个结点 pre=p； //依次继续遍历 r=r->next; p->next=pre； //指针反转 L->next=p； //处理最后—个结点 returnI;

Two types of physical structure Case: (bat, cat, eat) (2) Linked: 0200 cat 0325 Storage cell 1 ·0208 bat √ Logical relationship is 0200 described by point 0300 0325 at  Data Structure & Algorithms 3
