# ch18-figures2

> 主题库：数据库系统原理　｜　源文件：ch18-figures2.pdf　｜　3,385 字符　｜　12 个片段　｜　更新于 2026-11-11
Lock-Based Protocols (Cont.) Lock-compatibility matrix S S true false X false false A transaction may be granted a lock on an item if the requested lock is compatible with locks already held on the item by other transactions Any number of transactions can hold shared Iocks on an item, But if any transaction holds an exclusive on the item no other tansactionmayholdanylockontheitem Database System Concepts - 7th Edition 18.4 ©Silberschatz, Korth and Sudarshan

A B F D E G H Treerotocol 1. Only exclusive locksare allowed. 2 ahefirst lockbyT maybeonanydata item.Subsequently adata Q Yan belocked by aonlyif theparentofQ is urrentlylocked byT a. Data itemsmay beunlockedatany time 4. A data item that has been locked and unlocked by T; cannot Yaubseluentyerelocked yT Database System Concepts - 7th Edition 18.19 © Silberschatz, Korth and Sudarshan

Wait-forgraph Verticestransactions  Edge from T, → T, : if T; is waiting for a lock held in conflicting The system is in a deadlock state if and only if the wait-for graph has acycle.  Invoke a deadlock-detection algorithm periodically to look for cycles. T18 T20 T18 T20) T 17 1 (T19 ) 19 W ait-for graph without a cycle W ait-for graph with a cycle Database System Concepts - 7th Edition 18.25 ©Silberschatz, Korth and Sudarshan

ExampleofGranularity Hierarchy DB database  area fiIE  record Database System Concepts - 7th Edition 18.28 © Silberschatz, Korth and Sudarshan

Tampatibility Matri wit _ntention Lock Modes IS IX S SIX X IS true true true true false IX true true false false false S true false true false false SIX true false false false false X false false false false false Database System Concepts - 7th Edition 18.30 ©Silberschatz, Korth and Sudarshan

T—EST—P BASED Database System Concepts - 7th Edition 18.38 ©Silberschatz, Korth and Sudarshan

The timestamp-ordering protocol guarantees serializability since all the arcs in the precedence graph are of the form transaction transaction with smaller with larger Thus, there willbe no cycles in theprecedence graph Timestamp protocol ensures freedom from deadlock as no transaction Yeerwaits. But the schedule m ay not be cascade-free, and may not even be recoverable. Database System Concepts - 7th Edition 18.45 ©Silberschatz, Korth and Sudarshan

Database System Concepts - 7th Edition 18.53 ©Silberschatz, Korth and Sudarshan

Database System Concepts - 7th Edition 18.71 ©Silberschatz, Korth and Sudarshan

Database System Concepts - 7th Edition 18.76 ©Silberschatz, Korth and Sudarshan

Latch-Free Data-structure Updates aaaia odei notsafe ithout latehesi ecuted oncurrently insert(value, head) { node = new node node->value =value node->next = head head= node  This code is safe insert latchfree(head,value){ node = new node node->value =value RepEat oldhead = head node->next = oldhead result = CAS(head, oldhead, node) untlresult=esuccess Database System Concepts - 7th Edition 18.81 © Silberschatz, Korth and Sudarshan

Latch-Free Data-structure Updates aaaia odei notsafe ithout latehesi ecuted oncurrently insert(value, head) { node = new node node->value =value node->next = head head= node  This code is safe insert latchfree(head,value){ node = new node node->value =value RepEat oldhead = head node->next = oldhead result = CAS(head, oldhead, node) untlresult=esuccess Database System Concepts - 7th Edition 18.82 ©Silberschatz, Korth and Sudarshan
