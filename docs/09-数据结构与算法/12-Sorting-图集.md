# 12-Sorting-图集

> 主题库：数据结构与算法　｜　源文件：12-Sorting-图集.pdf　｜　5,235 字符　｜　58 个片段　｜　更新于 2026-11-11
1 2  3 4 5

Eortablle Lists

Eortable Lists

umsertom Sort Iniiial Ineert Iruert Isert Insert Ineert ardler Becons third fourth fth sixth entry entry entry entry ertry sorted hen co ceat t cet t 8oted Lrsortedl cow hen cow oow oo oox cat cat nm hen ewe dog 日 ram ram ram leinn hen ewa 自 ewe ewe ewe ewe revnn 1hecn dog dog dog dog dog l rted renn

Example of insertion sort 8 2 4 9 3 6

Example of insertion sort 8 4 9 3 0

Example of insertion sort 8 4 9 3 6 2 8 4 9 3 6

Example of insertion sort 8 4 9 3 6 2 8 4 9 3 0

ALGORITHMS Example of insertion sort  4 9 3 6  2 8 4 9 3 6 2 4 8 9 3 6

ALGORITHMS Example of insertion sort 4 3 2 0 2 8 4 9 3 4 8 3

ALGORITHMS T Example of insertion sort 8 4 9 3 6 2  2 8 4 9 3 6 2 4 8 3 6  2  2 4 8 9  6

ALGORITHMS Example of insertion sort 8 2 4 9 3 6 2 8 4 9 3 6 2 4 8 9 3 6 2 4 8 9 3 6 2 3 4  8 9 6

ALGORITHMS Example of insertion sort 8 4 9 3 6 2 8 4 9 3 6 2 4 8 3 6 2 48 9 6   2 3 4 8 9 0

ALGOrITHms xampleofinsertionsort 8 2 4 9 3 6 2 8 4 9 3 6 2 4 8 9 3 6   2 4 8 9 3 6  2 3 4 8 9 6 2 3 4 6 8  9 done

Tlaim step, commiguous case Befora: Sorted Urvorted S curent Curient C C 1 ahift eatdes right Sarted Sortadl Unrortedl 八

Comigmoms lmsertion Sort

Limkedl lmsertiom Eort

Limkeal lmsertiom Eort

Case 1: tirst_unsorted belongsat head of lst heed ast onted irestunsoned Csse 2: "Tirat ynsorod bekougs between "traling aund "ourent heed tralling culnrent estsoned. tlrst unsonted

Amalsis of Imsertiom Sort The0rem8.1.

Selecmom Sont nltlalrder Sarted hen hen ewe dog cow e ow ow cat cow at at ca ow m cat d am dog og we e eve m n han hn General  step: Unsorted Sorted snall keys large keys Maxirmuum wap unsortedkey Currerltposiltln

Selectiom sort

Eelectiom Sort Swapentries template<classRecord> void Sortable_list<Record>::swap(int low, int high) Record temp; temp = entry[low]; entry[low]=entry[high]; entry[high]=temp;

Fimd mna ximommn Ikey

Amalyeis amcl cormparisom

Exammle of Shem Sort Unsorted SubHsts tncr. 5 s5-Sorted Reco nnbined Dot 4 Dot Dot Dot Eva 4 Eva Anmy Amy Roy 4 Roy Jan Jen Tam 4 Tm Amn Anm Km K Km K Guy 4 Guy す Guy 中 Guy Amy 4 Ammy 中 Eva Eva Jon 年 Jon す Jon 幸 Jon Amn 4 Amn 中 Tom Tom Jlim J Tm Tim Kay Kay 中 Kay  Kay Ron Ron T Ron Ron Jan en Roy Roy

Mxammple of Shem Sort Sublists incr. 3 3-Sorted List incr. 1 Serted T 4 Jon t Kay Kay Kay 1 Tomn 中 Ron Ron Klim 4 Tm Roy T Roy Ron 亨 Key Kim Krm Roy Ron す Tom Tom Tlm Roy T1m Tm Tomm

关键字序列T=（21，25，49，25*，16，08），请给出简单选择排序的具体实现过程。 21,25，49，25*，16，08 第1趟 8，25，49，25，16， 0 第2趟 0 8，16,49，25* 第3趟 08，16，21，25*，254 第4趟 08，16，21，25*，25。49 第5趟 08， 16， 21， 25*， 25， 9 时间效率：O(n2） 虽移动次数较少，但比较次数仍多。 空间效率：0(1） 因为仅占用1个缓冲单元！ 算法的稳定性：不稳定因为排序时，25*到了25的前面。

例： 关键字序列T=（49，38，65，97，76，13，27，49*，55，04）， 请写出希尔排序的具体实现过程。 r[i] 0 1 2 3 4 5 7 8 9 10  49 38 65 9776 13 2749 55 04   第1 (2k=) 132749 550449 38 6 776 第 13 04 49* 38 27 49 55 65 97 76 04 13 27 38 4 55 65 76 97 分开始时的值较大，子序列中的对象较少，排序速度较快；随着排序进展， 值逐渐变小，子序列中对象个数逐渐变多，由于前面工作的基础， 多数对象已基本有序，所以排度仍然快

21 25 49 25* 16 08

21 25 25* 49 16 08

49 21 25 25* 16 08

21 25 25* 49    08

21 25 25* 16 08 49

21 25 16 08   25* 49

21 16-08-25 25* 49

16 08 21 25 25* 49

08 16 21  25 25* 49

Divide amd Congmer Sorting Nlargasort

Start Finish 2 11222 121926293335  26 3 29   26 29 33 35   191222 121922                   2683   2633 35 29 29 35 19 12 12 19 2

Omicsのrt or7 下mmmlere 12 2 5

Pivot ia Thim cYanan ey Dot Tom Guy Pivot ie central key Amy Eva Amn Roy Eva Jon Kim A y Jlimn Tom @uy Rom Dot Jen Kimm Jon Ann Kay Roy Jlinn Kay Ron Tilnn

Mergesort MERGE-SORT4[1..n] 1.Ifn=1,done. 2. Recursively sort A[1..[n/21] and 4[[n/2]+1..n]. 3. Merge the 2 sorted lists. Keyswbroutine:MERGE

AEG- E S Merging two sorted arrays 20 12 1311

Merging two sorted arrays 20 12 13 11

Merging two sorted arrays 20 12 20 12 13 11 13 11 7 9 7 9

AEGORIEHSES Merging two sorted arrays 201 12 2012 13 11 13 11 9 7 9 2 2

Merging two sortedarrays 20 1 12 20 12 2012 13 11 13 11 13 11 9  9 1 2

AIGORITHMS Merging two sorted arrays 20 12 20 12 20 12 13 11 13 11 13 11 7 9 7 9 2 1 2 7

20 12 20 12 20 12 20 12 13 11 13  11 13 11 13 11  7 9 9 9

Mergingtwosortedarrays 20 12 20 12 20 12 20 12 13 11 13 11 13 11 13 11 9 7 1 2

Mergingtwo sortedarrays 20 12 20 12 20 12 20 12 20 12 13 11 13  11 13 11 13 11 13 11 1 2 9

Meringwosortedarrays 20 12 20 12 20 12 20 12 20 12 13 11 13 11 13 11 13 11 13 7 9 7 9 2 11

Merging twosorted arrays 20 12 20 12 20 12 20 12 20 12 20 12 13 11 13 11 13 11 13 11 13 13 7 9 7 9 9 1 2 11

20 12 20 12 20 12 20 12 20 12 20  13 11 13 11 13 11 13 11 13 13  1 2 9 11 12

Mergingtwo sortedarrays 20 12 20 12 20 12 20 12 20 12  20(  13 11 13 11 13 11 13 11 13 13 7 9 7 9 2 11 12 Time = O(n)to merge a total of nelements (linear time).

a Analyzingmergesort T(n) MERGE-SORTA[1..n] Θ(1) 1.Ifn=1,done. 2T(n/2) 2. RecuirsivelysortA[ 1 ..[n/21] Abuse andA[[n/2+1..n]. Θ(n) 3. Merge the 2 sorted lists Sloppiness:Should beT([n/21)+ T(Ln/2 ),  but itturns out not to matter asymptotically.

ALGORETHMS Recurrenceformergesort (1)ifn=1 ()二 2T(n/2)+Θ(n)ifn>1. 2 Weshallusuallyomitstatingthebase case whenT(n)= O(1) for sufficiently smalln,butonly whenit has no effect on the asymptotic solution to the recurrence. CLRSand Lecture2provide severalways to find a good upper bound on T(n).
