# Lecture10.RunTime-图集-矢量

> 主题库：编译原理　｜　源文件：Lecture10.RunTime-图集-矢量.pdf　｜　1,144 字符　｜　16 个片段　｜　更新于 2026-11-13
Code Area C Static Area 2 Heap [a d Stack

qsort(3, 3) qsort(2, 3) qsort(1, 3) qsort(1, 9) cYm aane

Actual Parameters s Returned Values s ControlLink  AccessLink  Saved Machine 2 Status r LocalData L cam a

cYma nan e access link a gsort(1, 9) access link qsort(1, 3) access link partition(1, 3). ) access link swap(1, 3) access link

cYmaane access link  a qsort(1, 9) access link qsort(1, 3) access link partition(1, 3) access link

access link  a qsort(1, 9) access link qsort(1, 3) access link

access link   ACcESS liNk

In-Process S Communication Inter-Process Communication (IPC)

Caller Actual Parameters c Retunedealues Control Link Access Link Saved Machine Status Local Data Temporaries CaNee Actual Parameters Control Link Access Link Sp Saved Machine Status Local Data Temporaries

Root Heap Memory Reference Sa c I 1 Stack I 1 I I cYa etableebec CycliC reference

I 1 Stack 1 1 I I cYa etable

Root Heap 3  Static Stack  befor

Root Heap afte 3 Stack  reachable object

n reachable object freespace Paae 25/3

Copying Collector Before vs. after HEAP ROOt Stack before

[aa Heap 1X2X③ ROOt Static reachable object Stack free space
