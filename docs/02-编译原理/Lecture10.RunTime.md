# Lecture10.RunTime

> 主题库：编译原理　｜　源文件：Lecture10.RunTime.pdf　｜　9,394 字符　｜　34 个片段　｜　更新于 2026-11-11


©Copyright 2008-2023, Sun Yat-senUniversity
Prof. Wen-junLI
School of Computer Science andEngineering
lnslwj@mail.sysu.edu.cn
Principles of 
Compiler Construction

Page 2/34
Lecture 10.  Run-Time Environment
1.Storage Management
2.Stack and Activation Record
3.In-Process Communication
4.Heap Management
5.Garbage Collection

Page 3/34
Operating SystemSystem Software
Run-Time Environment
Target Programs
Program Abstractions
Names, Scopes, Bindings, Procedures, ...
For example, the subprogram abstraction 
needs the support by a run-time stack
Overview

Page 4/34
1. Storage Management
Typical storage layout and allocation
Code Area
Static Area
Heap
Free Memory
Stack
Low
High
Activation Record
new / delete
malloc / free
global and static

Page 5/34
2. Stack and Activation Record
Support the subprogram abstraction
⚫Also called Control Stack. 
General design rules
⚫Activation on Demand

Page 6/34
PKU-1990
pp. 294

Page 7/34
Activation Tree
Activation tree and run-time stack
main()
qsort(1, 9)readArray()
partition(1, 9)qsort(1, 3)qsort(5, 9)
partition(1, 3)qsort(1, 0)qsort(2, 3)partition(5, 9)qsort(5, 5)qsort(7, 9)
partition(2, 3)qsort(2, 1)qsort(3, 3)partition(7, 9)qsort(7, 7)qsort(9, 9)
main()
qsort(1, 9)
qsort(1, 3)
qsort(2, 3)
qsort(3, 3)

Page 8/34
Activation Record (or Frame)
Typical organization of activation records
Control Link
Returned Values
Saved Machine Status
Access Link
Temporaries
Local Data
Actual Parameters
return address & registers
user defined
compiler generated
pointer to Caller's AR
pointer to the outer AR
set by Caller, register preferred
set by Callee, register preferred

Page 9/34
Access Link
Access link in the quick sort example
qsort(1, 9)
access link
v
qsort(1, 3)
access link
v
main()
access link
a
partition(1, 3)
access link
swap(1, 3)
access link
qsort(1, 9)
access link
v
qsort(1, 3)
access link
v
main()
access link
a
partition(1, 3)
access link
qsort(1, 9)
access link
v
qsort(1, 3)
access link
v
main()
access link
a
qsort(1, 9)
access link
v
main()
access link
a
DisplayTable for 
very deep nesting

Page 10/34
3. In-Process Communication
Implementation of procedure calls
⚫Calling sequence
Allocate an activation record
Enter information into fields
...
⚫Return sequence
Restore the state of the machine
Continue the execution of Caller
...
In-Process Communication
vs.
Inter-Process Communication (IPC)

Page 11/34
Caller vs. Callee
Implementation of procedure calls
Control Link
Returned Values
Saved Machine Status
Access Link
Temporaries
Local Data
Actual Parameters
Caller
Control Link
Returned Values
Saved Machine Status
Access Link
Temporaries
Local Data
Actual Parameters
Callee
top_sp
Caller's responsibility
Callee's responsibility

Page 12/34
Activation Record Design
Control Link
Returned Values
Saved Machine Status
Access Link
Temporaries
Local Data
Actual Parameters
Caller
Control Link
Returned Values
Saved Machine Status
Access Link
Temporaries
Local Data
Actual Parameters
Callee
top_sp
Caller's responsibility
Callee's responsibility
Close to Caller's AR, to facilitate 
Caller to set actual arguments 
and get returned value
Fixed items placed in 
middle, even debuggers 
can access them
Items with variable length at the 
end, e.g. dynamic arrays. 
4
3
2
1
Caller does not know local data 
and temporaries in Callee! 

Page 13/34
Calling Sequence
A typical calling sequence
1.Caller evaluates the actual arguments. 
2.Caller stores a return address and old top_sp
to Callee's activation record. 
3.Caller pushes an activation record to the 
stack (increments top_sp). 
4.Callee saves the register values and other 
status information.
5.Callee initializes local data and begin the  
execution. 

Page 14/34
Return Sequence
A typical return sequence
1.Callee sets the return value. 
2.Callee pops the activation record (decrements 
top_sp). 
3.Callee restores the registers. 
4.Callee goes to the return address in the 
status field. 
5.Caller gets the return value (even top_sp
has been decremented). 

Page 15/34
4. Heap Management
The critical problem of heap management 
is fragmentation
⚫Both timeefficiency and spaceefficiency are 
considered. 
⚫How to minimize fragmentation? 

Page 16/34
Multithreading
Multithreading programs share a common 
heap
⚫Of course each program must have its own 
control stack. 
Communication between procedures in 
different threads are easier than IPC
⚫The shared heap can be utilized in 
communication. 

Page 17/34
5. Garbage Collection
Automatic Memory Management? 
manualautomatic
AllocationDeallocation
automaticautomaticStack
manualHeap

Page 18/34
Terminology
Garbage
⚫Objects that can not be referenced. 
Memory leaks
⚫Maybe the most troublesome bugs. 
⚫Does Java have memory leaks? 
Profiler (Rational Purify)
⚫JVMPI: JVM Profiler Interface since JDK 1.2 
(Java 2). 
⚫JVM TI: JVM Tool Interface since JDK 5.0
Garbage collection
⚫Pros and cons? 
⚫High-Level Abstraction in Languages. 
Great expectation to 
JVMGCI

Page 19/34
Performance Metrics
Overall Execution Time
Space Usage
⚫Avoid fragmentation
Pause Time
⚫Critical to real-time applications
Program Locality
⚫The program spend most of time executing a 
relative small code fraction and touching only a 
small data fraction. (90% time on 10%code)
⚫Locality facilitate the utilization of memory 
hierarchy (register, cache, memory, disk, etc. ). 
⚫Garbage collection can improve both temporal 
locality and spatial locality. 

Page 20/34
Garbage Collection Algorithms
Reference counting (immediately)
Trace-based algorithms (periodically)
⚫Basic Mark-and-Sweep
⚫Baker's Mark-and-Sweep
⚫Basic Mark-and-Compact
⚫Cheney's Copying Collector
Short-pause algorithms
⚫Incremental garbage collection
⚫Incremental reachability analysis
⚫Partial collection
⚫Generational garbage collection

Page 21/34
Reference Counting
counter++ vs. counter--
Heap
Static
Stack
Memory
Location
Reference
Counter
Cyclic 
reference
Root
reachable object
unreachable object

Page 22/34
Reference Counting (cont')
Pros
⚫Simple
⚫Incremental collection
⚫Immediate collection
Cons
⚫Can not collect unreachable cyclic data 
structures. 
⚫Additional operations lead to expensive 
overhead. 
Depends on computation, not only on number 
of objects. 

Page 23/34
Mark-and-Sweep
2 steps: trace (mark), then sweep. 
Heap
Static
Stack
Root
reachable object
unreachable object

Page 24/34
Mark-and-Sweep (cont')
Pros
⚫High efficiency if little garbage exist. 
⚫Be able to collect cyclic references. 
Cons
⚫Low efficiency with large amount of garbage
Improvement by H. Baker (1992)
Keep a list of all allocated objects. 
⚫Pause of the mutator (program) makes it 
difficult to apply to real-time applications. 
⚫Leads to fragmentation in the heap. 

Page 25/34
Mark-and-Compact
Before vs. after
Heap
Static
Stack
Root
321
before
after
Heap
Static
Stack
Root
321
reachable object
unreachable object
n
free space

Page 26/34
Mark-and-Compact (cont')
3 steps
1.Marking phase: similar to Mark-and-Sweep. 
2.For each reachable object, computes a new 
address from the low end of the heap. 
3.Copies objects to new locations and updates 
all references in the root area and objects. 
Pros and cons
⚫Pro: avoids fragmentation. 
So as to improve the space usage and 
program locality. 
⚫Con: step 3 (copying in the compacting) is 
expensive. 
Updating references is simple and fast. 

Page 27/34
Copying Collector
Before vs. after
before
after
Heap
Static
Stack
Root
321
Heap
Static
Stack
Root
321
reachable object
unreachable object
n
free space

Page 28/34
Copying Collector (cont')
Heap is partitioned into 2 semispaces
⚫Reachable objects are moved as soon as they 
are discovered. 
⚫Does not touch any unreachable objects. 
Pros
⚫More efficient and popular. 
Cons
⚫Only a half of heap memory can be utilized. 

Page 29/34
Comparing Costs
Mark-and-Sweep
⚫Number of chunks in the heap
Baker's Improvement
⚫Number of reached objects
Mark-and-Compact
⚫Number of chunks in the heap + 
⚫Total size of the reached objects
Copying Collector
⚫Total size of the reached objects

Page 30/34
Generational Garbage Collection
Motivation
⚫Difference between handling short-lived and 
long-lived objects. 
Approach
⚫Objects are divided into multiple generations 
based on some criteria. 
Usually related to the age of the objects. 
⚫GC can be done at different time intervals and 
even using different techniques, based on the 
generations. 

Page 31/34
Implementation in JVM
Use a two-generation (young and old) 
approach
Young generation: copying collector
⚫Newly created objects tend to die young. 
Old generation: Mark-and-Compact
⚫A good mix of technologies for performance

Page 32/34
Exercise 10.1
Surf the Internet and write a short paper to 
compare GC on Java platform with GC on 
Microsoft .Net platform. 

Page 33/34
Further Reading
Dragon Book, 2
nd
Edition (DBv2)
⚫Comprehensive Reading: 
Section 7.2-7.3 on control stack and activation 
records. 
Section 7.4 on heap management,includingthe 
concepts ofTemporalLocalityandSpatial Locality. 
Section 7.5-7.7 on garbage collection. 
⚫Skip Reading: 
Section 7.1 on introduction to storage management. 
Section 7.8 on advanced topics in garbage collection. 
More on garbage collection: R. Jones and R. Lins. 
Garbage Collection: Algorithms for Automatic 
Dynamic Memory Management. Wiley, 1996, 
ISBN 0-471-94148-4

Page 34/34
Enjoy the Course!
