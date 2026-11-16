# ch18 - Concurrency Control

> 主题库：数据库系统原理　｜　源文件：ch18 - Concurrency Control.pdf　｜　48,669 字符　｜　90 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 18 : Concurrency Control 

©Silberschatz, Korthand Sudarshan18.2Database System Concepts -7
th
Edition
Outline
Lock-Based Protocols
Timestamp-Based Protocols
Validation-Based Protocols
Multiple Granularity
MultiversionSchemes
Insert and Delete Operations
Concurrency in Index Structures

©Silberschatz, Korthand Sudarshan18.3Database System Concepts -7
th
Edition
Lock-Based Protocols
A lock is a mechanism to control concurrent access to a data item
Data items can be locked in two modes :
1.  exclusive(X) mode. Data item can be both read as well  as   
written. X-lock is requested using lock-Xinstruction.
2.  shared(S) mode. Data item can only be read. S-lock is          
requested using lock-Sinstruction.
Lock requests are made to concurrency-control manager. Transaction 
can proceed only after request is granted.

©Silberschatz, Korthand Sudarshan18.4Database System Concepts -7
th
Edition
Lock-Based Protocols (Cont.)
Lock-compatibility matrix
A transaction may be granted a lock on an item if the requested lock 
is compatible with locks already held on the item by other transactions
Any number of transactions can hold shared locks on an item, 
But if any transaction holds an exclusive on the item no other 
transaction may hold any lock on the item.

©Silberschatz, Korthand Sudarshan18.6Database System Concepts -7
th
Edition
Schedule With Lock Grants
Grants omitted in rest of 
chapter
•Assume grant happens 
just before the next 
instruction following lock 
request
This schedule is not 
serializable (why?)
A  locking protocolis a 
set of rules followed by all 
transactions while 
requesting and releasing 
locks.
Locking protocols enforce 
serializability by restricting 
the set of possible 
schedules.

©Silberschatz, Korthand Sudarshan18.7Database System Concepts -7
th
Edition
Deadlock
Consider the partial schedule
Neither T
3
nor T
4
can make progress —executing  lock-S(B)causes 
T
4
to wait for T
3
to release its lock on B, while executing  lock-X(A)
causes T
3
to wait for T
4
to release its lock on A.
Such a situation is called a deadlock. 
•To handle a deadlock one of T
3
or T
4
must be rolled back 
and its locks released.

©Silberschatz, Korthand Sudarshan18.8Database System Concepts -7
th
Edition
Deadlock (Cont.)
The potential for deadlock exists in most locking protocols. Deadlocks 
are a necessary evil.
Starvationis also possible if concurrency control manager is badly 
designed. For example:
•A transaction may be waiting for an X-lock on an item, while a 
sequence of other transactions request and are granted an S-lock 
on the same item.  
•The same transaction is repeatedly rolled back due to deadlocks.
Concurrency control manager can be designed to prevent starvation.

©Silberschatz, Korthand Sudarshan18.9Database System Concepts -7
th
Edition
The Two-Phase Locking Protocol
A protocol which ensures conflict-serializable schedules.
Phase 1: Growing Phase
•Transaction may obtain locks 
•Transaction may not release locks
Phase 2: Shrinking Phase
•Transaction may release locks
•Transaction may not obtain locks
The protocol assures serializability. It can be proved that the 
transactions can be serialized in the order of their lock points(i.e., 
the point where a transaction acquired its final lock). 
Time
Locks

©Silberschatz, Korthand Sudarshan18.10Database System Concepts -7
th
Edition
The Two-Phase Locking Protocol (Cont.)
Two-phase locking does notensure freedom from deadlocks
Extensions to basic two-phase locking needed to ensure 
recoverability of freedom from cascading roll-back
•Strict two-phase locking: a transaction must hold all its exclusive locks 
till it commits/aborts.
Ensures recoverability and avoids cascading roll-backs
•Rigorous two-phase locking: a transaction must hold all locks till 
commit/abort. 
Transactions can be serialized in the order in which they commit.
Most databases implement rigorous two-phase locking, but refer to it 
as simply two-phase locking

©Silberschatz, Korthand Sudarshan18.11Database System Concepts -7
th
Edition
The Two-Phase Locking Protocol (Cont.)
Two-phase locking is not a necessary 
condition for serializability
•There are conflict serializable 
schedules that cannot be obtained if the 
two-phase locking protocol is used.  
In the absence of extra information 
(e.g., ordering of  access to data), two-
phase locking is necessary for conflict 
serializability in the following sense:
•Given a transaction T
i
that does not 
follow two-phase locking, we can 
find a transaction T
j
that uses two-
phase locking, and a schedule for T
i
and T
j
that is not conflict 
serializable.

©Silberschatz, Korthand Sudarshan18.12Database System Concepts -7
th
Edition
Locking Protocols
Given a locking protocol (such as 2PL)
•A schedule S is legalunder a locking protocol if it can be 
generated by a set of transactions that follow the protocol 
•A protocol ensuresserializability if all legal schedules under that 
protocol are serializable

©Silberschatz, Korthand Sudarshan18.13Database System Concepts -7
th
Edition
Lock Conversions
Two-phase locking protocol with lock conversions:
–Growing Phase:        
•can acquire a lock-S on item
•can acquire a lock-X on item
•can converta lock-S to a lock-X (upgrade)
–Shrinking Phase:
•can release a lock-S
•can release a lock-X
•can convert a lock-X to a lock-S  (downgrade)
This protocol ensures serializability

©Silberschatz, Korthand Sudarshan18.14Database System Concepts -7
th
Edition
Automatic Acquisition of Locks
A transaction T
i
issues the standard read/write instruction, without 
explicit locking calls.
The operation read(D) is processed as:
ifT
i
has a lock on D
then
read(D) 
else begin
if necessary wait until no other  
transaction has a lock-Xon D
grant T
i
a lock-Son D;
read(D)
end

©Silberschatz, Korthand Sudarshan18.15Database System Concepts -7
th
Edition
Automatic Acquisition of Locks (Cont.)
write(D)is processed as:
if T
i
has a  lock-Xon D
then
write(D)
else begin
if necessary wait until no other trans. has any lock on D,
if T
i
has a lock-Son D
then
upgradelock on Dto lock-X
else
grant T
i
a lock-Xon D
write(D)
end;
All locks are released after commit or abort

©Silberschatz, Korthand Sudarshan18.16Database System Concepts -7
th
Edition
Implementation of Locking
Alock manager can be implemented as a separate process 
Transactions can send lock and unlock requests as messages
The lock manager replies to a lock request by sending a lock grant 
messages (or a message asking the transaction to roll back, in case 
of  a deadlock)
•The requesting transaction waits until its request is answered
The lock manager maintains an in-memory data-structure called a 
lock table to record granted locks and pending requests

©Silberschatz, Korthand Sudarshan18.17Database System Concepts -7
th
Edition
Lock Table
Dark rectangles indicate granted locks, 
light colored ones indicate waiting 
requests
Lock table also records the type of lock 
granted or requested
New request is added to the end of the 
queue of requests for the data item, and 
granted if it is compatible with all earlier 
locks
Unlock requests result in the request 
being deleted, and later requests are 
checked to see if they can now be 
granted
If transaction aborts, all waiting or 
granted requests of the transaction are 
deleted 
•lock manager may keep a list of 
locks held by each transaction, to 
implement this efficiently

©Silberschatz, Korthand Sudarshan18.18Database System Concepts -7
th
Edition
Graph-Based Protocols
Graph-based protocols are an alternative to two-phase locking
Impose a partial ordering →on the set D= {d
1
, d
2
,..., d
h
} of all data 
items.
•If d
i
→d
j
then any transaction accessing both d
i
and d
j
must 
access d
i
before accessing d
j
.
•Implies that the set Dmay now be viewed as a directed acyclic 
graph, called a database graph.
The tree-protocolis a simple kind of graph protocol. 

©Silberschatz, Korthand Sudarshan18.19Database System Concepts -7
th
Edition
Tree Protocol
Tree protocol:
1.Only exclusive locks are allowed.
2.The first lock by T
i
may be on any data item. Subsequently, a data Q
can be locked by T
i
only if the parent of Qis currently locked by T
i
.
3.Data items may be unlocked at any time.
4.A data item that has been locked and unlocked by T
i
cannot 
subsequently be relocked by T
i

©Silberschatz, Korthand Sudarshan18.20Database System Concepts -7
th
Edition
Graph-Based Protocols (Cont.)
The tree protocol ensures conflict serializability as well as freedom 
from deadlock.
Unlocking may occur earlier in the tree-locking protocol than in the 
two-phase locking protocol.
•Shorter waiting times, and increase in concurrency
•Protocol is deadlock-free, no rollbacks are required
Drawbacks
•Protocol does not guarantee recoverability or cascade freedom
Need to introduce commit dependencies to ensure recoverability 
•Transactions may have to lock data items that they do not access.
increased locking overhead, and additional waiting time
potential decrease in concurrency
Schedules not possible under two-phase locking are possible under 
the tree protocol, and vice versa.

©Silberschatz, Korthand Sudarshan18.21Database System Concepts -7
th
Edition
Deadlock Handling
System is deadlockedif there is a set of transactions such that every 
transaction in the set is waiting for another transaction in the set.

©Silberschatz, Korthand Sudarshan18.22Database System Concepts -7
th
Edition
Deadlock Handling
Deadlock preventionprotocols ensure that the system will never
enter into a deadlock state. Some prevention strategies:
•Require that each transaction locks all its data items before it 
begins execution (pre-declaration).
•Impose partial ordering of all data items and require that a 
transaction can lock data items only in the order specified by the 
partial order (graph-based protocol).

©Silberschatz, Korthand Sudarshan18.23Database System Concepts -7
th
Edition
More Deadlock Prevention Strategies
wait-diescheme —non-preemptive
•Older transaction may wait for younger one to release data item.
•Younger transactions never wait for older ones; they are rolled 
back instead.
•A transaction may die several times before acquiring a lock
wound-waitscheme —preemptive
•Older transaction wounds(forces rollback) of younger transaction 
instead of waiting for it. 
•Younger transactions may wait for older ones.
•Fewer rollbacks than wait-diescheme.
In both schemes, a rolled back transactions is restarted with its 
original timestamp. 
•Ensures that older transactions have precedence over newer 
ones, and starvation is thus avoided.

©Silberschatz, Korthand Sudarshan18.24Database System Concepts -7
th
Edition
Deadlock prevention (Cont.)
Timeout-Based Schemes:
•A transaction waits for a lock only for a specified amount of time. 
After that, the wait times out and the transaction is rolled back.
•Ensures that deadlocks get resolved by timeout if they occur
•Simple to implement
•But may roll back transaction unnecessarily in absence of 
deadlock
difficult to determine good value of the timeout interval.
•Starvation is also possible

©Silberschatz, Korthand Sudarshan18.25Database System Concepts -7
th
Edition
Deadlock Detection
Wait-for graph
•Vertices: transactions
•Edge fromT
i
→T
j
. : if T
i
is waiting for a lock held in conflicting 
mode byT
j
The system is in a deadlock state if and only if the wait-for graph has 
a cycle.  
Invoke a deadlock-detection algorithm periodically to look for cycles.
Wait-for graph without a cycleWait-for graph  with a cycle

©Silberschatz, Korthand Sudarshan18.26Database System Concepts -7
th
Edition
Deadlock Recovery
When deadlock is  detected :
•Some transaction will have to rolled back (made a victim) to break 
deadlock cycle.  
Select that transaction as victim that will incur minimum cost
•Rollback -- determine how far to roll back transaction
Total rollback: Abort the transaction and then restart it.
Partial rollback: Roll back victim transaction only as far as 
necessary to release locks that another transaction in cycle is 
waiting for
Starvation can happen (why?)
•One solution: oldest transaction in the deadlock set is never 
chosen as victim

©Silberschatz, Korthand Sudarshan18.27Database System Concepts -7
th
Edition
Multiple Granularity
Allow data items to be of various sizes and define a hierarchy of data 
granularities, where the small granularities are nested within larger 
ones
Can be represented graphically as a tree (but don't confuse with tree-
locking protocol)
When a transaction locks a node in the tree explicitly, it implicitlylocks 
all the node's descendentsin the same mode.
Granularity of locking (level in tree where locking is done):
•Fine granularity(lower in tree): high concurrency, high locking 
overhead
•Coarse granularity(higher in tree): low locking overhead, low 
concurrency

©Silberschatz, Korthand Sudarshan18.28Database System Concepts -7
th
Edition
Example of Granularity Hierarchy
The levels, starting from the coarsest (top) level are
•database
•area 
•file
•record

©Silberschatz, Korthand Sudarshan18.29Database System Concepts -7
th
Edition
Intention Lock Modes
In addition to S and X lock modes, there are three additional lock 
modes with multiple granularity:
•intention-shared(IS): indicates explicit locking at a lower level of 
the tree but only with shared locks.
•intention-exclusive(IX): indicates explicit locking at a lower level 
with exclusive or shared locks
•shared and intention-exclusive(SIX): the subtree rooted by that 
node is locked explicitly in shared mode and explicit locking is 
being done at a lower level with exclusive-mode locks.
intention locks allow a higher level node to be locked in S or X mode 
without having to check all descendent nodes.

©Silberschatz, Korthand Sudarshan18.30Database System Concepts -7
th
Edition
Compatibility Matrix with Intention Lock Modes
The compatibility matrix for all lock modes is: 

©Silberschatz, Korthand Sudarshan18.31Database System Concepts -7
th
Edition
Multiple Granularity Locking Scheme
Transaction T
i
can lock a node Q, using the following rules:
1.The lock compatibility matrix must be observed.
2.The root of the tree must be locked first, and may be locked in any mode.
3.A node Qcan be locked by T
i
in S or IS mode only if the parent of Qis 
currently locked by T
i
in either IX or IS mode.
4.A node Qcan be locked by T
i
in X, SIX, or IX mode only if the parent of Q
is currently locked by T
i
in either IX or SIX mode.
5.T
i
can lock a node only if it has not previously unlocked any node (that is, 
T
i
is two-phase).
6.T
i
can unlock a node Qonly if none of the children of Qare currently 
locked by T
i
.
Observe that locks are acquired in root-to -leaf order, whereas they 
are released in leaf-to -root order.
Lock granularity escalation: in case there are too many locks at a 
particular level, switch to higher granularity S or X lock

©Silberschatz, Korthand Sudarshan18.32Database System Concepts -7
th
Edition
Insert/Delete Operations and
Predicate Reads
Locking rules for insert/delete operations
1.An exclusive lock must be obtained on an item before it is deleted
2.A transaction that inserts a new tuple into the database is automatically 
given an X-mode lock on the tuple
Ensures that 
•reads/writes conflict with deletes
•Inserted tuple is not accessible by other transactions until the transaction 
that inserts the tuple commits

©Silberschatz, Korthand Sudarshan18.33Database System Concepts -7
th
Edition
Phantom Phenomenon
Example of phantom phenomenon.
•A transaction T1 that performs predicate read (or scan) of a relation 
selectcount(*)
frominstructor
wheredept_name = 'Physics'
•and a transaction T2 that inserts a tuple while T1 is active but after 
predicate read 
insert intoinstructorvalues('11111', 'Feynman', 'Physics', 94000)
(conceptually) conflict in spite of not accessing any tuple in common.
If only tuple locks are used, non-serializable schedules can result
•E.g. the scan transaction does not see the new instructor, but may read 
some other tuple written by the update transaction
Can also occur with updates
•E.g. update Wu’s department from Finance to Physics

©Silberschatz, Korthand Sudarshan18.34Database System Concepts -7
th
Edition
Non-Serializable Execution Due to 
Phantom Phenomenon
T1
T2
Read(instructor where
dept_name=‘Physics’)
Insert Instructor in Physics
Insert Instructor in Comp. Sci.
Read(instructor where
dept_name=‘Comp. Sci.’)
Another Example:  T1 and T2 both find maximum instructor ID in 
parallel, and create new instructors with ID = maximum ID + 1
•Both instructors get same ID, not possible in serializable 
schedule
Commit

©Silberschatz, Korthand Sudarshan18.35Database System Concepts -7
th
Edition
Handling Phantoms
There is a conflict at the data level
•The transaction performing predicate read or scanning the relation is 
reading information that indicates what tuples the relation contains
•The transaction inserting/deleting/updating a tuple updates the same 
information.
•The conflict should be detected, e.g. by locking the information.
One solution: 
•Associate a data item with the relation, to represent the information about 
what tuples the relation contains.
•Transactions scanning the relation acquire a shared lock in the data item, 
•Transactions inserting or deleting a tuple acquire an exclusive lock on the 
data item. (Note: locks on the data item do not conflict with locks on 
individual tuples.)
Above protocol provides very low concurrency for insertions/deletions.

©Silberschatz, Korthand Sudarshan18.36Database System Concepts -7
th
Edition
Index Locking To Prevent Phantoms
Index locking protocol to prevent phantoms
•Every relation must have at least one index. 
•A transaction can access tuples only after finding them through one or 
more indices on the relation
•A transaction T
i
that performs a lookup must lock all the index leaf nodes 
that it accesses, in S-mode
Even if the leaf node does not contain any tuple satisfying the index 
lookup (e.g. for a range query, no tuple in a leaf is in the range)
•A transaction T
i
that inserts, updates or deletes a tuple t
i
in a relation r
must update all indices to r
must obtain exclusive locks on all index leaf nodes affected by the 
insert/update/delete
•The rules of the two-phase locking protocol must be observed
Guarantees that phantom phenomenon won’t occur

©Silberschatz, Korthand Sudarshan18.37Database System Concepts -7
th
Edition
Next-Key Locking to Prevent Phantoms
Index-locking protocol to prevent phantoms locks entire leaf node
•Can result in poor concurrency if there are many inserts
Next-key locking protocol: provides higher concurrency
•Lock all values that satisfy index lookup (match lookup value, or 
fall in lookup range)
•Also lock next key value in index
even for inserts/deletes
•Lock mode: S for lookups, X for insert/delete/update
Ensures detection of query conflicts with inserts, deletes and updates
3   5   8   11   1418  24   38  55
Consider B+-tree leaf nodes as below, with query predicate 7 ≤X ≤16.  
Check what happens with next-key locking when inserting: (i) 15 and (ii) 7

©Silberschatz, Korthand Sudarshan18.38Database System Concepts -7
th
Edition
TIMESTAMP BASED 
CONCURRENCY CONTROL

©Silberschatz, Korthand Sudarshan18.39Database System Concepts -7
th
Edition
Timestamp-Based Protocols
Each transaction T
i
is issued a timestamp TS(T
i
) when it enters the 
system.
•Each transaction has a uniquetimestamp
•Newer transactions have timestamps strictly greater than earlier ones
•Timestamp could be based on a logical counter
Real time may not be unique
Can use (wall-clock time, logical counter) to ensure 
Timestamp-based protocols manage concurrent execution such that 
time-stamp order = serializability order
Several alternative protocols based on timestamps

©Silberschatz, Korthand Sudarshan18.40Database System Concepts -7
th
Edition
Timestamp-Ordering Protocol
The timestamp ordering (TSO) protocol
Maintains for each data Q two timestamp values:
•W-timestamp(Q) is the largest time-stamp of any transaction that 
executed write(Q) successfully.
•R-timestamp(Q) is the largest time-stamp of any transaction that 
executed read(Q) successfully.
Imposes rules on read and write operations to ensure that 
•any conflicting operations are executed in timestamp order
•out of order operations cause transaction rollback

©Silberschatz, Korthand Sudarshan18.41Database System Concepts -7
th
Edition
Timestamp-Based Protocols (Cont.)
Suppose a transaction T
i
issues a read(Q)
1.If TS(T
i
) ≤W-timestamp(Q), then T
i
needs to read a value of Q
that was already overwritten.
Hence, the readoperation is rejected, and T
i
is rolled back.
2.If TS(T
i
) ≥W-timestamp(Q), then the readoperation is executed, 
and R-timestamp(Q) is set to 
max(R-timestamp(Q), TS(T
i
)).

©Silberschatz, Korthand Sudarshan18.42Database System Concepts -7
th
Edition
Timestamp-Based Protocols (Cont.)
Suppose that transaction T
i
issues write(Q).
1.If TS(T
i
) < R-timestamp(Q), then the value of Qthat T
i
is 
producing was needed previously, and the system assumed that 
that value would never be produced. 
Hence, the writeoperation is rejected, and T
i
is rolled back.
2.If TS(T
i
) < W-timestamp(Q), then T
i
is attempting to write an 
obsolete value of Q. 
Hence, this writeoperation is rejected, and T
i
is rolled back.
3.Otherwise, the writeoperation is executed, and W-timestamp(Q) 
is set to TS(T
i
).

©Silberschatz, Korthand Sudarshan18.43Database System Concepts -7
th
Edition
Example of Schedule Under TSO
And how about this one,
where initially
R-TS(Q)=W-TS(Q)=0
Assume that initially:
R-TS(A) = W-TS(A) = 0
R-TS(B) = W-TS(B) = 0
Assume TS(T
25
) = 25 and         
TS(T
26
) = 26
Is this schedule valid under TSO?

©Silberschatz, Korthand Sudarshan18.44Database System Concepts -7
th
Edition
Another Example Under TSO
A partial schedule for several data items for transactions with
timestamps 1, 2, 3, 4, 5, with all R-TS and W-TS = 0 initially

©Silberschatz, Korthand Sudarshan18.45Database System Concepts -7
th
Edition
Correctness of Timestamp-Ordering Protocol
The timestamp-ordering protocol guarantees serializability since all 
the arcs in the precedence graph are of the form:
Thus, there will be no cycles in the precedence graph
Timestamp protocol ensures freedom from deadlock as no transaction 
ever waits.  
But the schedule may not be cascade-free, and may  not even be 
recoverable.

©Silberschatz, Korthand Sudarshan18.46Database System Concepts -7
th
Edition
Recoverability and Cascade Freedom
Solution 1:
•A transaction is structured such that its writes are all performed at 
the end of its processing
•All writes of a transaction form an atomic action; no transaction 
may execute while a transaction is being written
•A transaction that aborts is restarted with a new timestamp
Solution 2: Limited form of locking: wait for data to be committed 
before reading it
Solution 3: Use commit dependencies to ensure recoverability

©Silberschatz, Korthand Sudarshan18.47Database System Concepts -7
th
Edition
Thomas’Write Rule
Modified version of the timestamp-ordering protocol in which obsolete 
writeoperations may be ignored under certain circumstances.
When T
i
attempts to write data item Q, if TS(T
i
) <W-timestamp(Q), 
then T
i
is attempting to write an obsolete value of {Q}. 
•Rather than rolling back T
i
as the timestamp ordering protocol 
would have done, this {write} operation can be ignored.
Otherwise this protocol is the same as the timestamp ordering 
protocol.
Thomas' Write Rule allows greater potential concurrency. 
•Allows some view-serializable schedules that are not conflict-
serializable.

©Silberschatz, Korthand Sudarshan18.48Database System Concepts -7
th
Edition
Validation-Based Protocol
Idea: can we use commit time as serialization order?
To do so:
•Postpone writes to end of transaction
•Keep track of data items read/written by transaction
•Validationperformed at commit time, detect any out-of-serialization order 
reads/writes
Also called as optimistic concurrency controlsince transaction 
executes fully in the hope that all will go well during validation

©Silberschatz, Korthand Sudarshan18.49Database System Concepts -7
th
Edition
Validation-Based Protocol
Execution of transaction T
i
is done in three phases.
1.  Read and execution phase: Transaction T
i
writes only to         
temporary local variables
2.  Validation phase: Transaction T
i
performs a  '‘validation test'' 
to determine if local variables can be written without violating         
serializability.
3.  Write phase: If T
i
is validated, the updates are applied to the 
database; otherwise, T
i
is rolled back.
The three phases of concurrently executing transactions can be    
interleaved, but each transaction must go through the three phases in 
that order.
•We assume for simplicity that the validation and write phase occur 
together, atomically and serially
I.e., only one transaction executes validation/write at a time. 

©Silberschatz, Korthand Sudarshan18.50Database System Concepts -7
th
Edition
Validation-Based Protocol (Cont.)
Each transaction T
i
has 3 timestamps
•StartTS(T
i
) : the time when T
i
started its execution
•ValidationTS(T
i
): the time when T
i
entered its validation phase
•FinishTS(T
i
) : the time when T
i
finished its write phase
Validation tests use above timestamps and read/write sets to ensure 
that serializability order is determined by validation time
•Thus, TS(T
i
) = ValidationTS(T
i
)
Validation-based protocol has been found to give greater degree of 
concurrency than locking/TSO if probability of conflicts is low. 

©Silberschatz, Korthand Sudarshan18.51Database System Concepts -7
th
Edition
Validation Test for Transaction T
j
If for all T
i
with TS (T
i
) < TS (T
j
) either one of the following condition 
holds:
•finishTS(T
i
) < startTS(T
j
) 
•startTS(T
j
) < finishTS(T
i
) < validationTS(T
j
) and the set of data items 
written by T
i
does not intersect with the set of data items read by T
j
.  
then validation succeeds and T
j
can be committed.  
Otherwise, validation fails and T
j
is aborted.
Justification:  
•First condition applies when execution is not concurrent
The writes of T
j
do not affect reads of T
i
since they occur after T
i
has 
finished its reads.
•If the second condition holds, execution is concurrent, T
j
does not read  
any item written by T
i
.

©Silberschatz, Korthand Sudarshan18.52Database System Concepts -7
th
Edition
Schedule Produced by Validation
Example of schedule produced using validation

©Silberschatz, Korthand Sudarshan18.53Database System Concepts -7
th
Edition
MULTIVERSION
CONCURRENCY CONTROL

©Silberschatz, Korthand Sudarshan18.54Database System Concepts -7
th
Edition
Multiversion Schemes
Multiversionschemes keep old versions of data item to increase 
concurrency.  Several variants:
•MultiversionTimestamp Ordering
•MultiversionTwo-Phase Locking
•Snapshot isolation
Key ideas:
•Each successful writeresults in the creation of a new version of 
the data item written.
•Use timestamps to label versions.
•When a read(Q) operation is issued, select an appropriate version 
of Qbased on the timestamp of the transaction issuing the read 
request, and return the value of the selected version.  
reads never have to wait as an appropriate version is returned 
immediately.

©Silberschatz, Korthand Sudarshan18.55Database System Concepts -7
th
Edition
Multiversion Timestamp Ordering
Each data item Qhas a sequence of versions <Q
1
, Q
2
,...., Q
m
>. Each 
version Q
k
contains three data fields:
•Content-- the value of version Q
k
.
•W-timestamp(Q
k
) -- timestamp of the transaction that created 
(wrote) version Q
k
•R-timestamp(Q
k
) -- largest timestamp of a transaction that 
successfully read version Q
k

©Silberschatz, Korthand Sudarshan18.56Database System Concepts -7
th
Edition
Multiversion Timestamp Ordering (Cont)
Suppose that transaction T
i
issues a read(Q) or write(Q) operation.  
Let Q
k
denote the version of Qwhose write timestamp is the largest 
write timestamp less than or equal to TS(T
i
).
1.If transaction T
i
issues a read(Q), then
the value returned is the  content of version Q
k
If R-timestamp(Q
k
) < TS(T
i
), set R-timestamp(Q
k
) = TS(T
i
), 
2.If transaction T
i
issues a write(Q)
1.if TS(T
i
) <R-timestamp(Q
k
), then transaction T
i
is rolled back. 
2.if TS(T
i
) =W-timestamp(Q
k
), the contents of Q
k
are overwritten
3.Otherwise,  a new version Q
i
of Qis created
•  W-timestamp(Q
i
) and R-timestamp(Q
i
) are initialized to 
TS(T
i
). 

©Silberschatz, Korthand Sudarshan18.57Database System Concepts -7
th
Edition
Multiversion Timestamp Ordering (Cont)
Observations
•Reads always succeed
•A write by T
i
is rejected if some other transaction T
j
that (in the 
serialization order defined by the timestamp values) should read  
T
i
's write, has already read a version created by a transaction 
older than T
i
.
Protocol guarantees serializability

©Silberschatz, Korthand Sudarshan18.58Database System Concepts -7
th
Edition
Multiversion Two-Phase Locking
Differentiates between read-only transactions and update transactions
Update transactions acquire read and write locks, and hold all locks 
up to the end of the transaction. That is, update transactions follow 
rigorous two-phase locking.
•Read of a data item returns the latest version of the item
•The first writeof Q by T
i
results in the creation of a new version Q
i
of the data item Q written
W-timestamp(Q
i
) set to ∞ initially
•When update transaction T
i
completes, commit processing occurs:
Value ts-counter stored in the database is used to assign timestamps
•ts-counter is locked in two-phase manner
Set TS(T
i
) = ts-counter+ 1
Set W-timestamp(Q
i
) = TS(T
i
) for all versions Q
i 
that it creates
ts-counter= ts-counter +1

©Silberschatz, Korthand Sudarshan18.59Database System Concepts -7
th
Edition
Multiversion Two-Phase Locking (Cont.)
Read-only transactions
•are assigned a timestamp = ts-counterwhen they start execution
•follow the multiversiontimestamp-ordering protocol for performing 
reads
Do not obtain any locks
Read-only transactions that start after T
i
increments ts-counterwill 
see the values updated by T
i
. 
Read-only transactions that start before T
i
increments the
ts-counterwill see the value before the updates by T
i
.
Only serializable schedules are produced.

©Silberschatz, Korthand Sudarshan18.60Database System Concepts -7
th
Edition
MVCC: Implementation Issues
Creation of multiple versions increases storage overhead
•Extra tuples
•Extra space in each tuple for storing version information
Versions can, however, be garbage collected
•E.g. if Q has two versions Q5 and Q9, and the oldest active 
transaction has timestamp > 9, than Q5 will never be required 
again
Issues with 
•primary key and foreign key constraint checking
•Indexing of records with multiple versions
See textbook for details

©Silberschatz, Korthand Sudarshan18.61Database System Concepts -7
th
Edition
Snapshot Isolation
Motivation: Decision support queries that read large amounts of data 
have concurrency conflicts with OLTP transactions that update a few 
rows
•Poor performance results
Solution 1:  Use multiversion2-phase locking
•Give logical “snapshot”of database state to read only transaction
Reads performed on snapshot
•Update (read-write) transactions use normal locking
•Works well, but how does system know a transaction is read only?
(Partial) Solution 2: Give snapshot of database state to every 
transaction
•Reads performed on snapshot
•Use 2-phase locking on updated data items
•Problem: variety of anomalies such as lost update can result
•Better solution: snapshot isolation level (next slide)

©Silberschatz, Korthand Sudarshan18.62Database System Concepts -7
th
Edition
Snapshot Isolation
A transaction T1 executing with Snapshot 
Isolation
•takes snapshot of committed data at 
start
•always reads/modifies data in its own 
snapshot
•updates of concurrent transactions are 
not visible to T1 
•writes of T1 complete when it commits
•First-committer-wins rule:
Commits only if no other concurrent 
transaction has already written data 
that T1 intends to write.
T1T2T3
W(Y := 1)
Commit
Start
R(X) 0
R(Y)1
W(X:=2)
W(Z:=3)
Commit
R(Z) 0
R(Y) 1
W(X:=3)
Commit-Req
Abort
Concurrent updates not visible
Own updates are visible
Not first-committer of X
Serialization error, T2 is rolled back

©Silberschatz, Korthand Sudarshan18.63Database System Concepts -7
th
Edition
Snapshot Read
Concurrent updates invisible to snapshot read

©Silberschatz, Korthand Sudarshan18.64Database System Concepts -7
th
Edition
Snapshot Write:First Committer Wins
•Variant: “First-updater-wins”
Check for concurrent updates when write occurs by locking item
–But lock should be held till all concurrent transactions have finished
(Oracle uses this plus some extra features)
Differs only in when abort occurs, otherwise equivalent 

©Silberschatz, Korthand Sudarshan18.65Database System Concepts -7
th
Edition
Benefits of SI
Reads are never blocked, 
•and also don’t block other txnsactivities
Performance similar to Read Committed
Avoids several anomalies
•No dirty read, i.e. no read of uncommitted data
•No lost update
i.e. update made by a transaction is overwritten by another transaction 
that did not see the update)
•No non-repeatable read
i.e. if read is executed again, it will see the same value
Problems with SI
•SI does not always give serializable executions
Serializable: among two concurrent txns, one sees the effects of the 
other
In SI: neither sees the effects of the other
•Result: Integrity constraints can be violated

©Silberschatz, Korthand Sudarshan18.66Database System Concepts -7
th
Edition
Snapshot Isolation
E.g. of problem with SI
•Initially A = 3 and B = 17
Serial execution:  A = ??, B = ??
if both transactions start at the same time, 
with snapshot isolation:  A = ?? , B = ??
Called skew write
Skew also occurs with inserts
•E.g:
Find max order number among all orders
Create a new order with order number = previous max + 1
Two transaction can both create order with same number
•  Is an example of phantom phenomenon

©Silberschatz, Korthand Sudarshan18.67Database System Concepts -7
th
Edition
Snapshot Isolation Anomalies
SI breaks serializability when transactions modify different items, each 
based on a previous state of the item the other modified
•Not very common in practice
E.g., the TPC-C benchmark runs correctly under SI
when txnsconflict due to modifying different data, there is usually also 
a shared item they both modify, so SI will abort one of them
•But problems do occur
Application developers should be careful about write skew
SI can also cause a read-only transaction anomaly, where read-only 
transaction may see an inconsistent state even if updaters are 
serializable
•We omit details
Using snapshots to verify primary/foreign key integrity can lead to 
inconsistency
•Integrity constraint checking usually done outside of snapshot

©Silberschatz, Korthand Sudarshan18.68Database System Concepts -7
th
Edition
Serializable Snapshot Isolation
Serializable snapshot isolation (SSI): extension of snapshot 
isolation that ensures serializability
Snapshot isolation tracks write-write conflicts, but does not track read-
write conflicts
•where T
i
writes a data a data item Q, T
j
reads an earlier version of Q, but 
T
j
is serialized after T
i
Idea:  track read-write dependencies separately, and roll-back 
transactions where cycles can occur
•Ensures serializability
•Details in book
Implemented in PostgreSQL from version 9.1 onwards
•PostgreSQL implementation of SSI also uses index locking to detect 
phantom conflicts, thus ensuring true serializability

©Silberschatz, Korthand Sudarshan18.69Database System Concepts -7
th
Edition
SI Implementations
Snapshot isolation supported by many databases 
•Including Oracle, PostgreSQL, SQL Server, IBM DB2, etc
•Isolation level can be set to snapshot isolation
Oracle implements “first updater wins”rule (variant of “first 
committer wins”)
•concurrent writer check is done at time of write, not at commit time
•Allows transactions to be rolled back earlier
Warning: even if isolation level is set to serializable, Oracle actually 
uses snapshot isolation
•Old versions ofPostgreSQL prior to 9.1 did this too
•Oracle and PostgreSQL < 9.1 do not support true serializable execution

©Silberschatz, Korthand Sudarshan18.70Database System Concepts -7
th
Edition
Working Around SI Anomalies
Can work around SI anomalies for specific queries by using select .. 
for update (supported e.g. in Oracle)
•E.g.,
selectmax(orderno) fromorders for update
read value into local variable maxorder
insert into orders (maxorder+1, ...)
select for update (SFU) clause treats all data read by the query as if 
it were also updated, preventing concurrent updates
Can be added to queries to ensure serializability in many applications
•Does not handle phantom phenomenon/predicate reads though

©Silberschatz, Korthand Sudarshan18.71Database System Concepts -7
th
Edition
WEAK LEVELS OF 
CONCURRENCY

©Silberschatz, Korthand Sudarshan18.72Database System Concepts -7
th
Edition
Weak Levels of Consistency
Degree-two consistency: differs from two-phase locking in that S-
locks may be released at any time, and locks may be acquired at any 
time
•X-locks must be held till end of transaction
•Serializability is not guaranteed, programmer must ensure that no 
erroneous database state will occur]
Cursor stability: 
•For reads, each tuple is locked, read, and lock is immediately 
released
•X-locks are held till end of transaction
•Special case of degree-two consistency

©Silberschatz, Korthand Sudarshan18.73Database System Concepts -7
th
Edition
Weak Levels of Consistency in SQL
SQL allows non-serializable executions
•Serializable: is the default
•Repeatable read: allows only committed records to be read, and 
repeating a read should return the same value (so read locks 
should be retained)
However, the phantom phenomenon need not be prevented
•  T1 may see some records inserted by T2, but may not see others 
inserted by T2
•Read committed:  same as degree two consistency, but most 
systems implement it as cursor-stability
•Read uncommitted: allows even uncommitted data to be read
In most database systems, read committed is the default consistency 
level
•Can be changed as database configuration parameter, or per 
transaction
set isolation level serializable

©Silberschatz, Korthand Sudarshan18.74Database System Concepts -7
th
Edition
Concurrency Control across User 
Interactions
Many applications need transaction support across user interactions
•Can’t use locking for long durations
Application level concurrency control
•Each tuple has a version number
•Transaction notes version number when reading tuple
selectr.balance, r.versioninto:A, :version 
fromr where acctId=23
•When writing tuple, check that current version number is same as 
the version when tuple was read
update r set r.balance= r.balance+ :deposit, r.version= 
r.version+1 
whereacctId= 23 andr.version= :version

©Silberschatz, Korthand Sudarshan18.75Database System Concepts -7
th
Edition
Concurrency Control across User 
Interactions
Equivalent to optimistic concurrency control without validating 
read set
•Unlike SI, reads are not guaranteed to be from a single snapshot.
•Does not guarantee serializability
•But avoids some anomalies such as “lost update anomaly”
Used internally in Hibernate ORM system
Implemented manually in many applications
Version numbers stored in tuples can also be used to support first 
committer wins check of snapshot isolation

©Silberschatz, Korthand Sudarshan18.76Database System Concepts -7
th
Edition
ADVANCED TOPICS IN 
CONCURRENCY CONTROL

©Silberschatz, Korthand Sudarshan18.77Database System Concepts -7
th
Edition
Online Index Creation
Problem: how to create an index on a large relation without affecting 
concurrent updates
•Index construction may take a long time
•Two-phase locking will block all concurrent updates
Key ideas:  
•build index on a snapshot of the relation, but keep track of all updates that 
occur after snapshot
Updates are not applied on the index at this point
•Then apply subsequent updates to catch up
•Acquire relation lock towards end of catchup phase to block concurrent 
updates
•Catch up with remaining updates, and add index to system catalog
•Subsequent transactions will find the index in catalogand update it

©Silberschatz, Korthand Sudarshan18.78Database System Concepts -7
th
Edition
Concurrency in Index Structures
Indices are unlike other database items in that their only job is to help 
in accessing data.
Index-structures are typically accessed very often, much more than 
other database items. 
•Treating index-structures like other database items, e.g. by 2-
phase locking of index nodes can lead to low concurrency.   
There are several index concurrency protocols where locks on 
internal nodes are released early, and not in a two-phase fashion.
•It is acceptable to have nonserializableconcurrent access to an 
index as long as the accuracy of the index is maintained.
In particular, the exact values read in an internal node of a 
B
+
-tree are irrelevant so long as we land up in the correct leaf 
node.

©Silberschatz, Korthand Sudarshan18.79Database System Concepts -7
th
Edition
Concurrency in Index Structures (Cont.)
Crabbing protocolused instead of two-phase locking on the nodes 
of the B
+
-tree during search/insertion/deletion:
•First lock the root node in shared mode.
•After locking all required children of a node in shared mode, release the 
lock on the node
•During insertion/deletion, upgrade leaf node locks to exclusive mode.
•When splitting or coalescing requires changes to a parent, lock the parent 
in exclusive mode.
Above protocol can cause excessive deadlocks
•Searches coming down the tree deadlock with updates going up the tree
•Can abort and restart search, without affecting transaction
The B-link treelocking protocolimproves concurrency
•Intuition: release lock on parent before acquiring lock on child
And deal with changes that may have happened between lock release 
and acquire

©Silberschatz, Korthand Sudarshan18.80Database System Concepts -7
th
Edition
Concurrency Control in Main-Memory 
Databases
Index locking protocols can be simplified with main-memory 
databases
•Short term lock can be obtained on entire index for duration of an 
operation, serializing updates on the index
Avoids overheads of multiple lock acquire/release
No major penalty since operations finish fast, since there is no disk 
wait
Latch-free techniques for data-structure update can speed up 
operations further

©Silberschatz, Korthand Sudarshan18.81Database System Concepts -7
th
Edition
Latch-Free Data-structure Updates
This code is not safe without latches if executed concurrently:
insert(value, head) {
node = new node
node−>value = value
node−>next = head
head= node
}
This code is safe
insert latchfree(head, value) {
node = new node
node−>value = value
repeat
oldhead= head
node−>next = oldhead
result = CAS(head, oldhead, node)
until (result == success)
}

©Silberschatz, Korthand Sudarshan18.82Database System Concepts -7
th
Edition
Latch-Free Data-structure Updates
This code is not safe without latches if executed concurrently:
insert(value, head) {
node = new node
node−>value = value
node−>next = head
head= node
}
This code is safe
insert latchfree(head, value) {
node = new node
node−>value = value
repeat
oldhead= head
node−>next = oldhead
result = CAS(head, oldhead, node)
until (result == success)
}

©Silberschatz, Korthand Sudarshan18.83Database System Concepts -7
th
Edition
Latch-Free Data-structures (Cont.)
Consider:
delete latchfree(head) {
/* This function is not quite safe; see explanation in text. */
repeat
oldhead= head
newhead= oldhead−>next
result = CAS(head, oldhead, newhead)
until (result == success)
}
Above code is almost correct, but has a concurrency bug
•P1 initiates delete with N1 as head; concurrently P2 deletes N1 and next 
node N2, and then reinserts N1 as head, with N3 as next
•P1 may set head as N2 instead of N3.
Known as ABA problem
See book for details of how to avoid this problem

©Silberschatz, Korthand Sudarshan18.84Database System Concepts -7
th
Edition
Concurrency Control with Operations
Consider this non-two phase schedule, 
which preserves database integrity 
constraints
Can be understood as transaction 
performing increment operation
•E.g. increment(A, -50), increment (B, 50)
•As long as increment operation does not 
return actual value, increments can be 
reordered
Increments commute
•New increment-mode lock to support 
reordering
•Conflict matrix with increment lock mode
Two increment operations do not 
conflict with each other

©Silberschatz, Korthand Sudarshan18.85Database System Concepts -7
th
Edition
Concurrency Control with Operations 
(Cont.)
Undo of increment(v, n) is performed by increment (v, -n) 
Increment_conditional(v, n):
•Updates v by adding n to it, as long as final v > 0, fails otherwise
•Can be used to model, e.g. number of available tickets, avail_tickets, for a 
concert
•Increment_conditionalis NOT commutative
E.g. last few tickets for a concert
•But reordering may still be acceptable

©Silberschatz, Korthand Sudarshan18.86Database System Concepts -7
th
Edition
Real-Time Transaction Systems
Transactions in a system may have deadlines within which they must 
be completed.
•Hard deadline: missing deadline is an error
•Firm deadline: value of transaction is 0 in case deadline is missed
•Soft deadline: transaction still has some value if done after deadline
Locking can cause blocking
Optimistic concurrency control (validation protocol) has been shown 
to do will in a real-time setting

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
End of Chapter 18

©Silberschatz, Korthand Sudarshan18.88Database System Concepts -7
th
Edition
View Serializability
Let Sand S ́be two schedules with the same set of 
transactions.  Sand S
 ́are view equivalentif the following 
three conditions are met, for each data item Q,
1.If in schedule S, transaction T
i
reads the initial value of Q, 
then in schedule S
’also transaction T
i
must read the initial 
value of Q.
2.If in schedule S transaction T
i
executes read(Q), and that 
value was produced by transaction T
j
(if any), then in 
schedule S
’also transaction T
i
must read the value of Q
that was produced by the same write(Q) operation of 
transaction T
j
.
3.The transaction (if any) that performs the final write(Q) 
operation in schedule S must also perform the final
write(Q) operation in schedule S
’.
As can be seen, view equivalence is also based purely on 
reads and writesalone.

©Silberschatz, Korthand Sudarshan18.89Database System Concepts -7
th
Edition
View Serializability (Cont.)
A schedule Sis view serializableif it is view equivalent to a 
serial schedule.
Every conflict serializable schedule is also view serializable.
Below is a schedule which is view-serializable but not conflict 
serializable.
What serial schedule is above equivalent to?
Every view serializable schedule that is not conflict serializable 
has blind writes.

©Silberschatz, Korthand Sudarshan18.90Database System Concepts -7
th
Edition
Test for View Serializability
The precedence graph test for conflict serializability cannot be 
used directly to test for view serializability.
•Extension to test for view serializability has cost exponential 
in the size of the precedence graph.
The problem of checking if a schedule is view serializable falls in 
the class of NP-complete problems. 
•Thus existence of an efficient algorithm is extremelyunlikely.
However practical algorithms that just check some sufficient
conditionsfor view serializability can still be used.

©Silberschatz, Korthand Sudarshan18.91Database System Concepts -7
th
Edition
Other Notions of Serializability
The schedule below produces same outcome as the serial 
schedule < T
1
,T
5
>, yet is not conflict equivalent or view 
equivalent to it.
Determining such equivalence requires analysis of operations 
other than read and write.
•Operation-conflicts, operation locks
