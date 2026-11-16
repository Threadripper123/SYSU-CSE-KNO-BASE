# ch19 - Recovery System

> 主题库：数据库系统原理　｜　源文件：ch19 - Recovery System.pdf　｜　49,245 字符　｜　101 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 19: Recovery System

©Silberschatz, Korth and Sudarshan19.2Database System Concepts -7
th
Edition
Outline
Failure Classification
Storage Structure
Recovery and Atomicity
Log-Based Recovery
Remote Backup Systems

©Silberschatz, Korth and Sudarshan19.3Database System Concepts -7
th
Edition
Failure Classification
Transaction failure:
•Logical errors: transaction cannot complete due to some internal 
error condition
•System errors: the database system must terminate an active 
transaction due to an error condition (e.g., deadlock)
System crash: a power failure or other hardware or software failure 
causes the system to crash.
•Fail-stop assumption: non-volatile storage contents are assumed 
to not be corrupted by system crash
Database systems have numerous integrity checks to prevent 
corruption of disk data 
Disk failure: a head crash or similar disk failure destroys all or part of 
disk storage
•Destruction is assumed to be detectable: disk drives use 
checksums to detect failures

©Silberschatz, Korth and Sudarshan19.4Database System Concepts -7
th
Edition
Recovery Algorithms
Suppose transaction T
i
transfers $50 from account Ato account B
•Two updates: subtract 50 from A and add 50 to B 
Transaction T
i
requires updates to A and B to be output to the 
database. 
•A failure may occur after one of these modifications have been made but 
before both of them are made. 
•Modifying the database without ensuring that the transaction will commit  
may leave the database in an inconsistent state
•Not modifying the database may result in lost updates if failure occurs 
just after transaction commits
Recovery algorithms have two parts
1.Actions taken during normal transaction processing to ensure enough 
information exists to recover from failures
2.Actions taken after a failure to recover the database contents to a state 
that ensures atomicity, consistency and durability

©Silberschatz, Korth and Sudarshan19.5Database System Concepts -7
th
Edition
Storage Structure
Volatile storage:
•Does not survive system crashes
•Examples: main memory, cache memory
Nonvolatile storage:
•Survives system crashes
•Examples:  disk, tape, flash memory, non-volatile RAM 
•But may still fail, losing data
Stable storage:
•A mythical form of storage that survives all failures
•Approximated by maintaining multiple copies on distinct 
nonvolatile media
•See book for more details on how to implement stable storage

©Silberschatz, Korth and Sudarshan19.6Database System Concepts -7
th
Edition
Stable-Storage Implementation
Maintain multiple copies of each block on separate disks
•copies can be at remote sites to protect against disasters such as fire or 
flooding.
Failure during data transfer can still result in inconsistent copies: 
Block transfer can result in
•Successful completion
•Partial failure: destination block has incorrect information
•Total failure: destination block was never updated
Protecting storage media from failure during data transfer (one 
solution):
•Execute output operation as follows (assuming two copies of each block):
1.Write the information onto the first physical block.
2.When the first write successfully completes, write the same 
information onto the second physical block.
3.The output is completed only after the second write successfully 
completes.

©Silberschatz, Korth and Sudarshan19.7Database System Concepts -7
th
Edition
Protecting storage media from failure (Cont.)
Copies of a block may differ due to failure during output operation. 
To recover from failure:
1.First find inconsistent blocks:
1.Expensive solution: Compare the two copies of every disk block.
2.Better solution: 
•Record in-progress disk writes on non-volatile storage (Flash, Non-
volatile RAM or special area of disk). 
•Use this information during recovery  to find blocks that may be 
inconsistent, and only compare copies of these. 
•Used in hardware RAID systems
2.If either copy of an inconsistent block is detected to have an error (bad 
checksum), overwrite it by the other copy.  If both have no error, but are 
different, overwrite the second block by the first block.   

©Silberschatz, Korth and Sudarshan19.8Database System Concepts -7
th
Edition
Data Access
Physical blocksare those blocks residing on the disk. 
Buffer blocksare the blocks residing temporarily in main memory.
Block movements between  disk and main memory are initiated 
through the following two operations:
•input(B) transfers the physical block B  to main memory.
•output (B) transfers the buffer block B to the disk, and replaces 
the appropriate physical block there.
We assume, for simplicity, that each data item fits in, and is stored 
inside, a single block.

©Silberschatz, Korth and Sudarshan19.9Database System Concepts -7
th
Edition
Data Access (Cont.)
Each transaction T
i
has its private work-area in which local copies of 
all data items accessed and updated by it are kept.
•T
i
's local copy of a data item Xis called x
i
.
Transferring data items between system buffer blocks and its private 
work-area done by:
•read(X) assigns the value of data item Xto the local variable x
i
.
•write(X) assigns the value of local variable x
i
to data item {X} in the buffer 
block.
•Note:output(B
X
) need not immediately follow write(X). System can 
perform the outputoperation when it deems fit.
Transactions 
•Must perform read(X) before accessing Xfor the first time (subsequent 
reads can be from local copy) 
•write(X) can be executed at any time before the transaction commits

©Silberschatz, Korth and Sudarshan19.10Database System Concepts -7
th
Edition
Example of Data Access
X      
Y     
A
B
x
1
y
1 
buffer
Buffer Block A
Buffer Block B
input(A)
output(B) 
read(X)
write(Y)
disk
work area
of T
1
work area
of T
2 
memory
x
2

©Silberschatz, Korth and Sudarshan19.11Database System Concepts -7
th
Edition
Recovery and Atomicity
To ensure atomicity despite failures, we first output information 
describing the modifications to stable storage without modifying the 
database itself.
We study log-based recoverymechanismsin detail
•We first present key concepts
•And then present the actual recovery algorithm
Less used alternative: shadow-copyandshadow-paging(brief 
details in book)
shadow-copy

©Silberschatz, Korth and Sudarshan19.12Database System Concepts -7
th
Edition
Log-Based Recovery
A  logis a sequence of  log records. The records  keep information 
about update activities on the database.
•The logis kept on stable storage 
When transaction T
i
starts, it registers itself by writing a
<T
i
start> log record
Before T
i
executes write(X), a log record 
<T
i
, X,  V
1
,  V
2
> 
is written, whereV
1
is the value of Xbefore the write (the old 
value),and V
2
is the value to be written to X (the new value). 
When T
i
finishes it last statement, the log record <T
i
commit> is 
written. 
Two approaches using logs
•Immediate database modification
•Deferred database modification. 

©Silberschatz, Korth and Sudarshan19.13Database System Concepts -7
th
Edition
Immediate Database Modification
The immediate-modificationscheme allows updates of an 
uncommitted transaction to be made to the buffer, or the disk itself, 
before the transaction commits
Update log record must be written beforedatabase item is written
•We assume that the log record is output directly to stable storage
•(Will see later that how to postpone log record output to some extent)
Output of updated blocks to disk can take place at any time before or 
after transaction commit
Order in which blocks are output can be different from the order in 
which they are written.
The deferred-modificationscheme performs updates to buffer/disk 
only at the time of transaction commit
•Simplifies some aspects of recovery
•But has overhead of storing local copy

©Silberschatz, Korth and Sudarshan19.14Database System Concepts -7
th
Edition
Transaction Commit
A transaction is said to have committed when its commit log record is 
output to stable storage 
•All previous log records of the transaction must have been output 
already 
Writes performed by a transaction may still be in the buffer when the 
transaction commits, and may be output later

©Silberschatz, Korth and Sudarshan19.15Database System Concepts -7
th
Edition
Immediate Database Modification Example
Log                                Write                     Output
<T
0
start>
<T
0
,A, 1000, 950>
<T
0
,B, 2000, 2050>
A= 950
B= 2050
<T
0
commit>
<T
1
start>
<T
1
, C, 700, 600>
C= 600
B
B 
, B
C
<T
1
commit>
B
A
Note: B
X
denotes block containing X.
B
C
output before T
1 
commits
B
A
output after T
0 
commits

©Silberschatz, Korth and Sudarshan19.16Database System Concepts -7
th
Edition
Concurrency Control and Recovery
With concurrent transactions, all transactions share a single disk 
buffer and a single log
•A buffer block can have data items updated by one or more transactions
We assume that if a transaction T
i
has modified an item, no other 
transaction can modify the same item until T
i
has committed or 
aborted
•i.e., the updates of uncommitted transactions should not be visible to 
other transactions
Otherwise, how to perform undo if T
1
updates A, then T
2
updates A 
and commits, and finally T
1
has to abort?
•Can be ensured by obtaining exclusive locks on updated items and 
holding the locks till end of transaction (strict two-phase locking)
Log records of different transactions may be interspersed in the log.

©Silberschatz, Korth and Sudarshan19.17Database System Concepts -7
th
Edition
Undo and Redo Operations
Undo and Redo of Transactions
•undo(T
i
)    -- restores the value of all data items updated by T
i
to 
their old values, going backwards from the last log record for T
i
Each time a data item X is restored to its old value V a special  
log record <T
i
, X, V> is written out
When undo of a transaction is complete, a log record 
<T
i
abort> is written out.
•redo(T
i
)    -- sets the value of all data items updated by T
i
to the 
new values, going forward from the first log record for T
i
No logging is done in this case

©Silberschatz, Korth and Sudarshan19.18Database System Concepts -7
th
Edition
Recovering from Failure
When recovering after failure:
•TransactionT
i
needs to be undone if the log 
Contains the record <T
i
start>,
But does not contain either the record <T
i
commit> or <T
i
abort>.
•Transaction T
i
needs to be redone if the log 
Contains the records <T
i
start>
And contains the record <T
i
commit> or <T
i
abort>

©Silberschatz, Korth and Sudarshan19.19Database System Concepts -7
th
Edition
Recovering from Failure (Cont.)
Suppose that  transaction T
i
was undone earlier and the <T
i
abort> 
record  was written to the log,  and then a failure occurs,
On recovery from failure  transaction T
i
is redone
•Such a redoredoes all the original actions of transaction T
i
including the steps that restored old values
Known as repeating history
Seems wasteful, but simplifies recovery greatly

©Silberschatz, Korth and Sudarshan19.20Database System Concepts -7
th
Edition
Immediate DB Modification Recovery Example
Below we show the log as it appears at three instances of time.
Recovery actions in each case above are:
(a)  undo (T
0
): B is restored to 2000 and A to 1000, and log records
<T
0
, B, 2000>, <T
0
, A, 1000>, <T
0
, abort> are written out
(b) redo (T
0
) and undo (T
1
): Aand Bare set to 950 and 2050 and C is 
restored to 700.  Log records <T
1
, C, 700>, <T
1
, abort> are written out.
(c)  redo (T
0
) and redo (T
1
): A and B are set to 950 and 2050 
respectively. Then Cis set to 600

©Silberschatz, Korth and Sudarshan19.21Database System Concepts -7
th
Edition
Checkpoints
Redoing/undoing all transactions recorded in the log can be very slow 
•Processing the entire log is time-consuming if the system has run for a 
long time
•We might unnecessarily redo transactions which have already output 
their updates to the database.
Streamline recovery procedure by periodically performing 
checkpointing
1.Output all log records currently residing in main memory onto stable 
storage.
2.Output all modified buffer blocks to the disk.
3.Write a log record <checkpoint L> onto stable storage where Lis a list 
of all transactions active at the time of checkpoint.
4.All updates are stopped while doing checkpointing

©Silberschatz, Korth and Sudarshan19.22Database System Concepts -7
th
Edition
Checkpoints (Cont.)
During recovery we need to consider only the most recent transaction 
T
i
that started before the checkpoint, and transactions that started 
after T
i
. 
•Scan backwards from end of log to find the most recent <checkpoint L> 
record 
•Only transactions that are in Lor started after the checkpoint need to be 
redone or undone
•Transactions that committed or aborted before the checkpoint already 
have all their updates output to stable storage.
Some earlier part of the log may be needed for undo operations
•Continue scanning backwards till a record <T
i
start> is found for every 
transaction T
i
in L.
•Parts of log prior to earliest <T
i
start> record above are not needed for 
recovery, and can be erased whenever desired.

©Silberschatz, Korth and Sudarshan19.23Database System Concepts -7
th
Edition
Example of Checkpoints
T
1
can be ignored (updates already output to disk due to 
checkpoint)
T
2
and T
3
redone.
T
4
undone
T
c
T
f
T
1
T
2
T
3
T
4
checkpoint
system failure

©Silberschatz, Korth and Sudarshan19.24Database System Concepts -7
th
Edition
Recovery Algorithm

©Silberschatz, Korth and Sudarshan19.25Database System Concepts -7
th
Edition
Recovery Algorithm
So far: we covered key concepts
Now: we present the components of the basic recovery 
algorithm
Later: we present extensions to allow more concurrency

©Silberschatz, Korth and Sudarshan19.26Database System Concepts -7
th
Edition
Recovery Algorithm
Logging(during normal operation):
•<T
i
start> at transaction start
•<T
i
, X
j
,  V
1
,  V
2
> for each update, and 
•<T
i
commit> at transaction end
Transaction rollback (during normal operation)
•Let T
i
be the transaction to be rolled back
•Scan log backwards from the end, and for each log record of T
i
of the 
form <T
i
, X
j
,  V
1
,  V
2
> 
Perform the undo by writing V
1 
to X
j
,
Write a log record <T
i
, X
j
,  V
1
> 
•  such log records are called compensation log records
•Once the record <T
i
start> is found stop the scan and write the log record 
<T
i
abort> 

©Silberschatz, Korth and Sudarshan19.27Database System Concepts -7
th
Edition
Recovery Algorithm (Cont.)
Recovery from failure: Two phases
•Redo phase:  replay updates of alltransactions, whether they 
committed, aborted, or are incomplete
•Undo phase: undo all incomplete transactions
Redo phase:
1.Find last <checkpointL> record, and set undo-list to L.
2.Scan forward from above <checkpointL> record
1.Whenever a  record <T
i
, X
j
,  V
1
,  V
2
> or<T
i
, X
j
, V
2
>  is found, redo it 
by writing V
2  
to X
j
2.Whenever a log record <T
i
start> is found, add T
i
to undo-list
3.Whenever a log record <T
i
commit> or <T
i
abort> is found, remove 
T
i
from undo-list

©Silberschatz, Korth and Sudarshan19.28Database System Concepts -7
th
Edition
Recovery Algorithm (Cont.)
Undo phase: 
1.Scan log backwards from end 
1.Whenever a log record <T
i
, X
j
,  V
1
,  V
2
> is found where T
i
is in undo-
list perform same actions as for transaction rollback:
1.perform undo by writing V
1
to X
j
.
2.write a log record <T
i
, X
j
,  V
1
>
2.Whenever a log record <T
i
start> is found where T
i
is in undo-list, 
1.Write a log record <T
i
abort> 
2.Remove T
i
from undo-list
3.Stop when undo-list is empty
i.e., <T
i
start> has been found for every transaction in undo-list
After undo phase completes, normal transaction processing can 
commence

©Silberschatz, Korth and Sudarshan19.29Database System Concepts -7
th
Edition
Example of Recovery

©Silberschatz, Korth and Sudarshan19.30Database System Concepts -7
th
Edition
Log Record Buffering
Log record buffering: log records are buffered in main memory, 
instead of being output directly to stable storage.
•Log records are output to stable storage when a block of log 
records in the buffer is full, or a log forceoperation is executed.
Log force is performed to commit a transaction by forcing all its log 
records (including the commit record) to stable storage.
Several log records can thus be output using a single output 
operation, reducing the I/O cost.

©Silberschatz, Korth and Sudarshan19.31Database System Concepts -7
th
Edition
Log Record Buffering (Cont.)
The rules below must be followed if log records are buffered:
•Log records are output to stable storage in the order in which they 
are created. 
•Transaction T
i
enters the commit state only when the log record 
<T
i
commit> has been output to stable storage.
•Before a block of data in main memory is output to the database, 
all log records pertaining to data in that block must have been 
output to stable storage. 
This rule is called the write-ahead loggingor WALrule
Strictly speaking,  WAL only requires undo information to be 
output

©Silberschatz, Korth and Sudarshan19.32Database System Concepts -7
th
Edition
Database Buffering
Database maintains an in-memory buffer of data blocks
•When a new block is needed, if buffer is full an existing block 
needs to be removed from buffer
•If the block chosen for removal has been updated, it must be 
output to disk
The recovery algorithm supports the no-force policy: i.e., updated 
blocks need not be written to disk when transaction commits
•force policy: requires updated blocks to be written at commit
More expensive commit
The recovery algorithm supports the steal policy: i.e., blocks 
containing updates of uncommitted transactions can be written to 
disk, even before the transaction commits

©Silberschatz, Korth and Sudarshan19.33Database System Concepts -7
th
Edition
Database Buffering (Cont.)
If a block with uncommitted updates is output to disk, log records with 
undo information for the updates are output to the log on stable 
storage first
•(Write ahead logging)
No updates should be in progress on a block when it is output to disk.  
Can be ensured as follows.
•Before writing a data item, transaction acquires exclusive lock on block 
containing the data item
•Lock can be released once the write is completed. 
Such locks held for short duration are called latches.
To output a block to disk
1.First acquire an exclusive latch on the block
Ensures no update can be in progress on the block
2.Then perform a log flush
3.Then output the block to disk
4.Finally release the latch on the block

©Silberschatz, Korth and Sudarshan19.34Database System Concepts -7
th
Edition
Buffer Management (Cont.)
Database buffer can be implemented either
•In an area of real main-memory reserved for the database, or
•In virtual memory
Implementing buffer in reserved main-memory has drawbacks:
•Memory is partitioned before-hand between database buffer and 
applications, limiting flexibility.  
•Needs may change, and although operating system knows best 
how memory should be divided up at any time, it cannot change 
the partitioning of memory.

©Silberschatz, Korth and Sudarshan19.35Database System Concepts -7
th
Edition
Buffer Management (Cont.)
Database buffers are generally implemented in virtual memory in spite 
of some drawbacks: 
•When operating system needs to evict a page that has been modified, 
the page is written to swap space on disk.
•When database decides to write buffer page to disk, buffer page may be 
in swap space, and may have to be  read from swap space on disk and 
output to the database on disk, resulting in extra I/O! 
Known as dual pagingproblem.
•Ideally when OS needs to evict a page from the buffer, it should pass 
control to database, which in turn should
1.Output the page to database instead of to swap space (making sure 
to output log records first), if it is modified
2.Release the page from the buffer, for the OS to use
Dual paging can thus be avoided, but common operating systems do not 
support such functionality.

©Silberschatz, Korth and Sudarshan19.36Database System Concepts -7
th
Edition
Fuzzy Checkpointing
To avoid long interruption of normal processing during checkpointing, 
allow updates to happen during checkpointing
Fuzzy checkpointingis done as follows:
1.Temporarily stop all updates by transactions
2.Write a <checkpointL> log record and force log to stable storage
3.Note list Mof modified buffer blocks
4.Now permit transactions to proceed with their actions
5.Output to disk all modified buffer blocks in list M
blocks should not be updated while being output
Follow WAL: all log records pertaining to a block must be 
output before the block is output
6.Store a pointer to the checkpointrecord in a fixed position 
last_checkpointon disk

©Silberschatz, Korth and Sudarshan19.37Database System Concepts -7
th
Edition
Fuzzy Checkpointing (Cont.)
When recovering using a fuzzy checkpoint, start scan from the 
checkpointrecord pointed to by last_checkpoint
•Log records before last_checkpointhave their updates reflected 
in database on disk, and need not be redone.
•Incomplete checkpoints, where system had crashed while 
performing checkpoint, are handled safely
......
<checkpoint L>
.....
<checkpoint L>
.....
Log
last_checkpoint

©Silberschatz, Korth and Sudarshan19.38Database System Concepts -7
th
Edition
Failure with Loss of Nonvolatile Storage
So far we assumed no loss of non-volatile storage
Technique similar to checkpointing used to deal with loss of non-
volatile storage
•Periodically dumpthe entire content of the database to stable 
storage
•No transaction may be active during the dump procedure; a 
procedure similar to checkpointing must take place
Output all log records currently residing in main memory onto 
stable storage.
Output all buffer blocks onto the disk.
Copy the contents of the database to stable storage.
Output a record <dump> to log on stable storage.

©Silberschatz, Korth and Sudarshan19.39Database System Concepts -7
th
Edition
Recovering from Failure of Non-Volatile 
Storage
To recover from disk failure
•restore database from  most recent dump. 
•Consult the log and redo all transactions that committed after the 
dump
Can be extended to allow transactions to be active during dump; 
known as fuzzy dumpor online dump
•Similar to fuzzy checkpointing

Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Remote Backup Systems

©Silberschatz, Korth and Sudarshan19.41Database System Concepts -7
th
Edition
Remote Backup Systems
Remote backup systems provide high availability by allowing 
transaction processing to continue even if the primary site is 
destroyed.

©Silberschatz, Korth and Sudarshan19.42Database System Concepts -7
th
Edition
Remote Backup Systems (Cont.)
Detection of failure: Backup site must detect when primary site has 
failed 
•to distinguish primary site failure from link failure maintain several 
communication links between the primary and the remote backup.
•Heart-beat messages
Transfer of control: 
•To take over control backup site first perform recovery using its copy of 
the database and all the long records it has received from the primary.
Thus, completed transactions are redone and incomplete transactions 
are rolled back.
•When the backup site takes over processing it becomes the new primary
•To transfer control back to old primary when it recovers, old primary must 
receive redo logs from the old backup and apply all updates locally.

©Silberschatz, Korth and Sudarshan19.43Database System Concepts -7
th
Edition
Remote Backup Systems (Cont.)
Time to recover: To reduce delay in takeover, backup site 
periodically procesesthe redo log records (in effect, performing 
recovery from previous database state), performs a checkpoint, and 
can then delete earlier parts of the log. 
Hot-Spareconfiguration permits very fast takeover:
•Backup continually processes redo log record as they arrive, 
applying the updates locally.
•When failure of the primary is detected the backup rolls back 
incomplete transactions, and is ready to  process new 
transactions.
Alternative to remote backup: distributed database with replicated 
data
•Remote backup is faster and cheaper, but less tolerant to failure 
more on this in Chapter 19

©Silberschatz, Korth and Sudarshan19.44Database System Concepts -7
th
Edition
Remote Backup Systems (Cont.)
Ensure durability of updates by delaying transaction commit until 
update is logged at backup; avoid this delay by permitting lower 
degrees of durability.
One-safe: commit as soon as transaction’s commit log record is 
written at primary
•Problem: updates may not arrive at backup before it takes over.
Two-very-safe: commit when transaction’s commit log record is 
written at primary and backup
•Reduces availability since transactions cannot commit if either site 
fails.
Two-safe: proceed as in two-very-safe if both primary and backup are 
active. If only the primary is active, the transaction commits as soon 
as is commit log record is written at the primary. 
•Better availability than two-very-safe; avoids problem of lost 
transactions in one-safe. 

Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Recovery with Early Lock Release and 
Logical Undo Operations

©Silberschatz, Korth and Sudarshan19.46Database System Concepts -7
th
Edition
Recovery with Early Lock Release
Support for high-concurrency locking techniques, such as those used 
for B
+
-tree concurrency control, which release locks early
•Supports “logical undo”
Recovery based on “repeating history”, whereby recovery executes 
exactly the same actions as normal processing

©Silberschatz, Korth and Sudarshan19.47Database System Concepts -7
th
Edition
Logical Undo Logging
Operations like B
+
-tree insertions and deletions release locks early. 
•They cannot be undone by restoring old values (physical undo), since 
once a lock is released, other transactions may have updated  the B
+
-
tree.
•Instead, insertions (resp. deletions) are undone  by executing a deletion 
(resp. insertion) operation (known as logical undo).  
For such operations, undo log records should contain the undo 
operation to be executed
•Such logging is called logical undo logging, in contrast to physical 
undo logging
Operations are called logical operations
•Other examples:
delete of tuple, to undo insert of tuple 
•  allows early lock release on space allocation information
subtract amount deposited, to undo deposit
•  allows early lock release on bank balance

©Silberschatz, Korth and Sudarshan19.48Database System Concepts -7
th
Edition
Physical Redo
Redo information is logged physically(that is, new value for each 
write) even for operations with logical undo
•Logical redo is very complicated since database state on disk may 
not be “operation consistent”when recovery starts
•Physical redo logging does not conflict with early lock release

©Silberschatz, Korth and Sudarshan19.49Database System Concepts -7
th
Edition
Operation Logging
Operation logging is done as follows:
1.When operation starts, log <T
i
, O
j
,operation-begin>. HereO
j
is a 
unique identifier of the operation instance.
2.While operation is executing, normal log records with physical redo and 
physical undo information are logged. 
3.When operation completes, <T
i
, O
j
,operation-end, U>is logged, where 
Ucontains information  needed to perform a logical undo information.
Example: insert of (key, record-id) pair (K5, RID7) into index I9 (Key at 
location X, record-id at location X+8) with old values Old1 and Old2
<T1, O1, operation-begin>
....
<T1, X, Old1, K5>
<T1, X+8, Old2, RID7>
<T1, O1, operation-end, (delete I9, K5, RID7)>
Physical redo of steps in insert

©Silberschatz, Korth and Sudarshan19.50Database System Concepts -7
th
Edition
Operation Logging (Cont.)
If crash/rollback occurs before operation completes:
•the operation-endlog record is not found, and 
•the physical undo information is used to undo operation.
If crash/rollback occurs after the operation completes:
•the operation-endlog record is found, and in this case
•logical undo is performed using U;  the physical undo information 
for the operation is ignored.
Redo of operation (after crash) still uses physical redo information.

©Silberschatz, Korth and Sudarshan19.51Database System Concepts -7
th
Edition
Transaction Rollback with Logical Undo
Rollback of transaction T
i
is done as follows: 
Scan the log backwards 
1.If a log record <T
i
, X, V
1
, V
2
> is found, perform the undo and log a 
al <T
i
, X, V
1
>.
2.If a <T
i
,  O
j
,operation-end, U> record is found
Rollback the operation logically using  the undo information U. 
•Updates performed during roll back are logged just like 
during normal operation execution.  
•At the end of the operation rollback, instead of logging an 
operation-endrecord, generate a record 
<T
i
,  O
j
,operation-abort>.
Skip all preceding log records for T
i
until the record
<T
i
,  O
j
operation-begin>  is found

©Silberschatz, Korth and Sudarshan19.52Database System Concepts -7
th
Edition
Transaction Rollback with Logical Undo 
(Cont.)
Transaction rollback, scanning the log backwards (cont.):
3.If a redo-only record is found ignore it
4.If a <T
i
,  O
j
,operation-abort> record is found:
skip all preceding log records for T
i
until the record 
<T
i
,  O
j
,operation-begin> is found.
5.Stop the scan when the record <T
i
,start> is found
6.Add a <T
i
,abort> record to the log
Some points to note:
Cases 3 and 4 above can occur only if the database crashes while a  
transaction is being rolled back.
Skipping of log records as in case 4 is important to prevent multiple 
rollback of the same operation.

©Silberschatz, Korth and Sudarshan19.53Database System Concepts -7
th
Edition
Transaction Rollback with Logical Undo
Transaction rollback during 
normal operation

©Silberschatz, Korth and Sudarshan19.54Database System Concepts -7
th
Edition
Failure Recovery with Logical Undo

©Silberschatz, Korth and Sudarshan19.55Database System Concepts -7
th
Edition
Transaction Rollback: Another Example
Example with a complete and an incomplete operation
<T1, start>
<T1, O1, operation-begin>
....
<T1, X, 10, K5>
<T1, Y, 45, RID7>
<T1, O1, operation-end, (delete I9, K5, RID7)>
<T1, O2, operation-begin> 
<T1, Z, 45, 70>   
T1 Rollback begins here
<T1, Z, 45>     redo-only log record during physical undo (of incomplete O2)
<T1, Y, .., ..>   Normal redo records for logical undo of O1
...
<T1, O1, operation-abort>  What if crash occurred immediately after this?
<T1, abort>

©Silberschatz, Korth and Sudarshan19.56Database System Concepts -7
th
Edition
Recovery Algorithm with Logical Undo
Basically same as earlier algorithm, except for changes described 
earlier for transaction rollback
1.(Redo phase): Scan log forward from last < checkpointL> record till 
end of log
1.Repeat historyby physically redoing all updates of  all 
transactions, 
2.Create an undo-list during the scan as follows
undo-listis set to Linitially
Whenever <T
i
start> is found T
i
is added to undo-list
Whenever <T
i
commit> or <T
i
abort> is found, T
i
is deleted 
from undo-list
This brings database to state as of crash, with committed as well as 
uncommitted transactions having been redone.
Now undo-listcontains transactions that are incomplete, that is, 
have neither committed nor been fully rolled back.

©Silberschatz, Korth and Sudarshan19.57Database System Concepts -7
th
Edition
Recovery with Logical Undo (Cont.)
Recovery from system crash (cont.)
2.(Undo phase): Scan log backwards, performing undo on log records 
of transactions found inundo-list.  
•Log records of transactions being rolled back are processed as 
described earlier, as they are found
Single shared scan for all transactions being undone
•When <T
i
start> is found for a transaction T
i
in undo-list, write a 
<T
i
abort> log record.
•Stop scan when <T
i
start> records have been found for all T
i
in 
undo-list
This undoes the effects of incomplete transactions (those with neither 
commitnor abortlog records). Recovery is now complete.

Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
ARIES Recovery Algorithm

©Silberschatz, Korth and Sudarshan19.59Database System Concepts -7
th
Edition
ARIES
ARIES is a state of the art recovery method 
•Incorporates numerous optimizations to reduce overheads during 
normal processing and to speed up recovery 
•The recovery algorithm we studied earlier is modeled after 
ARIES, but greatly simplified by removing optimizations
Unlike the recovery algorithm described earlier, ARIES 
1.Uses log sequence number (LSN)to identify log records
Stores LSNs in pages to identify what updates have already 
been applied to a database page
2.Physiological redo
3.Dirty page table to avoid unnecessary redosduring recovery
4.Fuzzy checkpointing that only records information about dirty 
pages, and does not require dirty pages to be written out at 
checkpoint time
More coming up on each of the above ...

©Silberschatz, Korth and Sudarshan19.60Database System Concepts -7
th
Edition
ARIES Optimizations
Physiological redo
•Affected page is physically identified, action within page can be 
logical
Used to reduce logging overheads
•e.g. when a record is deleted and all other records have to 
be moved to fill hole
Physiological redo can log just the record deletion 
Physical redo would require logging of old and new values for 
much of the page
Requires page to be output to disk atomically
•Easy to achieve with hardware RAID, also supported by 
some disk systems
•Incomplete page output can be detected by checksum 
techniques, 
But extra actions are required for recovery 
Treated as a media failure

©Silberschatz, Korth and Sudarshan19.61Database System Concepts -7
th
Edition
ARIES Data Structures
ARIES uses several data structures
•Log sequence number (LSN) identifies each log record
Must be sequentially increasing
Typically an offset from beginning of log file to allow fast access
•  Easily extended to handle multiple log files
•Page LSN
•Log records of several different types
•Dirty page table

©Silberschatz, Korth and Sudarshan19.62Database System Concepts -7
th
Edition
ARIES Data Structures: Page LSN
Each page contains a PageLSNwhich is the LSN of the last log 
record whose effects are reflected on the page
•To update a page:
X-latch the page, and write the log record 
Update the page
Record the LSN of the log record in PageLSN
Unlock page
•To flush page to disk, must first S-latch page
Thus page state on disk is operation consistent
•  Required to support physiological redo
•PageLSNis used during recovery to prevent repeated redo 
Thus ensuring idempotence

©Silberschatz, Korth and Sudarshan19.63Database System Concepts -7
th
Edition
ARIES Data Structures: Log Record
Each log record contains LSN of previous log record of the same 
transaction
•LSN in log record may be implicit
Special redo-only log record called compensation log record (CLR) 
used to log actions taken during recovery that never need to be 
undone
•Serves the role of operation-abort log records used in earlier recovery algorithm
•Has a field UndoNextLSNto note next (earlier) record to be undone
Records in between would have already been undone
Required to avoid repeated undo of already undone actions
LSN  TransID   PrevLSN   RedoInfo    UndoInfo
LSN  TransIDUndoNextLSNRedoInfo
1
23
4
4'
3'
2'
1'

©Silberschatz, Korth and Sudarshan19.64Database System Concepts -7
th
Edition
ARIES Data Structures: DirtyPage Table
DirtyPageTable
•List of pages in the buffer that have been updated
•Contains, for each such page
PageLSNof the page
RecLSNis an LSN such that log records before this LSN have 
already been applied to the page version on disk
•  Set to current end of log when a page is inserted into dirty 
page table (just before being updated)
•  Recorded in checkpoints, helps to minimize redo work

©Silberschatz, Korth and Sudarshan19.65Database System Concepts -7
th
Edition
ARIES Data Structures

©Silberschatz, Korth and Sudarshan19.66Database System Concepts -7
th
Edition
ARIES Data Structures: Checkpoint Log
Checkpoint log record
•Contains: 
DirtyPageTableand list of active transactions
For each active transaction, LastLSN, the LSN of the last log 
record written by the transaction
•Fixed position on disk notes LSN of last completed
checkpoint log record
Dirty pages are not written out at checkpoint time
Instead, they are flushed out continuously, in the background
Checkpoint is thus very low overhead
•can be done frequently

©Silberschatz, Korth and Sudarshan19.67Database System Concepts -7
th
Edition
ARIES Recovery Algorithm
ARIES recovery involves three passes
Analysis pass: Determines
•Which transactions to undo
•Which pages were dirty (disk version not up to date) at time of crash
•RedoLSN: LSN from which redo should start
Redo pass:
•Repeats history, redoing all actions from RedoLSN
RecLSNand PageLSNsare used to avoid redoing actions already 
reflected on page 
Undo pass:
•Rolls back all incomplete transactions
Transactions whose abort was complete earlier are not undone
•  Key idea: no need to undo these transactions: earlier undo actions 
were logged, and are redone as required

©Silberschatz, Korth and Sudarshan19.68Database System Concepts -7
th
Edition
Aries Recovery: 3 Passes
Analysis, redo and undo passes
Analysis determines where redo should start
Undo has to go back till start of earliest incomplete transaction
Last checkpoint
Log
Time
End of Log
Analysis pass
Redo pass
Undo pass

©Silberschatz, Korth and Sudarshan19.69Database System Concepts -7
th
Edition
ARIES Recovery: Analysis
Analysis pass
Starts from last complete checkpoint log record
•Reads DirtyPageTablefrom log record
•Sets RedoLSN= min of RecLSNsof all pages in DirtyPageTable
In case no pages are dirty, RedoLSN= checkpoint record’s 
LSN
•Sets undo-list = list of transactions in checkpoint log record
•Reads LSN of last log record for each transaction in undo-list from 
checkpoint log record
Scans forward from checkpoint
.. Cont. on next page ...

©Silberschatz, Korth and Sudarshan19.70Database System Concepts -7
th
Edition
ARIES Recovery: Analysis (Cont.)
Analysis pass (cont.)
Scans forward from checkpoint
•If any log record found for transaction not in undo-list, adds transaction to 
undo-list
•Whenever an update log record is found
If page is not in DirtyPageTable, it is added with RecLSNset to LSN of 
the update log record
•If transaction end log record found, delete transaction from undo-list
•Keeps track of last log record for each transaction in undo-list
May be needed for later undo
At end of analysis pass:
•RedoLSNdetermines where to start redo pass
•RecLSNfor each page in DirtyPageTableused to minimize redo work
•All transactions in undo-list need to be rolled back

©Silberschatz, Korth and Sudarshan19.71Database System Concepts -7
th
Edition
ARIES Redo Pass
Redo Pass: Repeats history by replaying every action not already 
reflected in the page on disk, as follows:
Scans forward from RedoLSN.  Whenever an update log record is 
found:
1.If the page is not in DirtyPageTableor the LSN of the log record is 
less than the RecLSNof the page in DirtyPageTable, then skip 
the log record
2.Otherwise fetch the page from disk.  If the PageLSNof the page 
fetched from disk is less than the LSN of the log record, redo the 
log record
NOTE: if either test is negative the effects of the log record have 
already appeared on the page.  First test avoids even fetching the 
page from disk!

©Silberschatz, Korth and Sudarshan19.72Database System Concepts -7
th
Edition
ARIES Undo Actions
When an undo is performed for an update log record
•Generate a CLR containing the undo action performed (actions performed 
during undo are logged physicalyor physiologically). 
CLR for   record nnoted as n’in figure below
•Set UndoNextLSNof the CLR to the PrevLSNvalue of the update log 
record
Arrows indicate UndoNextLSNvalue
ARIES supports partial rollback
•Used e.g. to handle deadlocks by rolling back just enough to release 
reqd. locks
•Figure indicates forward actions after partial rollbacks 
records 3 and 4 initially, later 5 and 6, then full rollback
1
23
4
4'
3'
5
65'
2'
1'
6'

©Silberschatz, Korth and Sudarshan19.73Database System Concepts -7
th
Edition
ARIES: Undo Pass
Undo pass:
Performs backward scan on log undoing all transaction in undo-list
•Backward scan optimized by skipping unneeded log records as follows:
Next LSN to be undone for each transaction set to LSN of last log 
record for transaction found by analysis pass.
At each step pick largest of these LSNs to undo, skip back to it and 
undo it 
After undoing a log record
•  For ordinary log records, set next LSN to be undone for 
transaction to PrevLSNnoted in the log record
•  For compensation log records (CLRs) set next LSN to be undo 
to UndoNextLSNnoted in the log record
All intervening records are skipped since they would have 
been undone already
Undosperformed as described earlier

©Silberschatz, Korth and Sudarshan19.74Database System Concepts -7
th
Edition
Recovery Actions in ARIES

©Silberschatz, Korth and Sudarshan19.75Database System Concepts -7
th
Edition
Other ARIES Features
Recovery Independence
•Pages can be recovered independently of others
E.g. if some disk pages fail they can be recovered from a 
backup while other pages are being used
Savepoints:
•Transactions can record savepointsand roll back to a savepoint
Useful for complex transactions
Also used to rollback just enough to release locks on deadlock

©Silberschatz, Korth and Sudarshan19.76Database System Concepts -7
th
Edition
Other ARIES Features (Cont.)
Fine-grained locking:
•Index concurrency algorithms that permit tuple level locking on 
indices can be used
These require logical undo, rather than physical undo, as in 
earlier recovery algorithm
Recovery optimizations:  For example:
•Dirty page table can be used to prefetchpages during redo
•Out of order redo is possible:
redo can be postponed on a page being fetched from disk, and
performed when page is fetched.  
Meanwhile other log records can continue to be processed

©Silberschatz, Korth and Sudarshan19.77Database System Concepts -7
th
Edition
Recovery in Main Memory Databases
Normal recovery algorithms can be used with main-memory 
databases
But optimizations are possible
•No redo-logging for indices: indices can be rebuilt quickly in-memory on 
recovery
•No undo logging if only committed data is written to disk
•Parallel recovery is important to load large amounts of data in-memory 
and perform recover with minimum delay

Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
End of Chapter 19

©Silberschatz, Korth and Sudarshan19.79Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.80Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.81Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.82Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.83Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.84Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.85Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.86Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan19.87Database System Concepts -7
th
Edition

Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Extra

©Silberschatz, Korth and Sudarshan19.89Database System Concepts -7
th
Edition
Shadow Paging
Shadow pagingis an alternative to log-based recovery; this scheme 
is useful if  transactions execute serially
Idea: maintaintwopage tables during the lifetime of a transaction –the 
current page table, and the shadow page table
Store the shadow page table in nonvolatile storage, such that state of 
the database prior to transaction execution may be recovered. 
•Shadow page table is never modified during execution
To start with, both the page tables are identical. Only current page 
table is used for data item accesses during execution of the 
transaction.
Whenever any page is about to be written for the first time
•A copy of this page is made onto an unused page. 
•The current page table is then made to point to the copy
•The update is performed on the copy

©Silberschatz, Korth and Sudarshan19.90Database System Concepts -7
th
Edition
Sample Page Table

©Silberschatz, Korth and Sudarshan19.91Database System Concepts -7
th
Edition
Example of Shadow Paging
Shadow and current page tables after write to page 4 

©Silberschatz, Korth and Sudarshan19.92Database System Concepts -7
th
Edition
Shadow Paging (Cont.)
To commit a transaction :
1.  Flush all modified pages in main memory to disk
2.  Output current page table to disk
3.  Make the current page table the new shadow page table, as follows:
•keep a pointer to the shadow page table at a fixed (known) location on 
disk.
•to make the current page table the new shadow page table, simply update 
the pointer to point to current page table on disk
Once pointer to shadow page table has been written, transaction is 
committed.
No recovery is needed after a crash —new transactions can start 
right away, using the shadow page table.
Pages not pointed to from current/shadow page table should be freed 
(garbage collected).

©Silberschatz, Korth and Sudarshan19.93Database System Concepts -7
th
Edition
Show Paging (Cont.)
Advantages of shadow-paging over log-based schemes
•no overhead of writing log records
•recovery is trivial
Disadvantages:
•Copying the entire page table is very expensive
Can be reduced by using a page table structured like a B
+
-tree
•  No need to copy entire tree, only need to copy paths in the tree 
that lead to updated leaf nodes
•Commit overhead is high even with above extension
Need to flush every updated page, and page table
•Data gets fragmented (related pages get separated on disk)
•After every transaction completion, the database pages containing old 
versions of modified data need to be garbage collected 
•Hard to extend algorithm to allow transactions to run concurrently
Easier to extend log based schemes

©Silberschatz, Korth and Sudarshan19.94Database System Concepts -7
th
Edition
Block Storage Operations

©Silberschatz, Korth and Sudarshan19.95Database System Concepts -7
th
Edition
Portion of the Database Log Corresponding to 
T
0
and T
1

©Silberschatz, Korth and Sudarshan19.96Database System Concepts -7
th
Edition
State of the Log and Database Corresponding to 
T
0 
and T
1

©Silberschatz, Korth and Sudarshan19.97Database System Concepts -7
th
Edition
Portion of the System Log Corresponding to 
T
0
and T
1

©Silberschatz, Korth and Sudarshan19.98Database System Concepts -7
th
Edition
State of System Log and Database 
Corresponding to T
0
and T
1

©Silberschatz, Korth and Sudarshan19.99Database System Concepts -7
th
Edition
Undo and Redo Operations
Undoof a log record <T
i
, X,  V
1
,  V
2
> writes the oldvalue V
1
toX
Redoof a log record <T
i
, X,  V
1
,  V
2
> writes the newvalue V
2
toX
Undo and Redo of Transactions
•undo(T
i
) restores the value of all data items updated by T
i
to their old 
values, going backwards from the last log record for T
i
Each time a data item X is restored to its old value V a special  log 
record <T
i
, X, V> is written out
When undo of a transaction is complete, a log record 
<T
i
abort> is written out.
•redo(T
i
) sets the value of all data items updated by T
i
to the new values, 
going forward from the first log record for T
i
No logging is done in this case

©Silberschatz, Korth and Sudarshan19.100Database System Concepts -7
th
Edition
Recovering from Failure
When recovering after failure:
•TransactionT
i
needs to be undone if the log 
contains the record <T
i
start>,
but does not contain either the record <T
i
commit> or <T
i
abort>.
•Transaction T
i
needs to be redone if the log 
contains the records <T
i
start>
and contains the record <T
i
commit> or <T
i
abort>
Note that If transaction T
i
was undone earlier and the <T
i
abort> 
record written to the log, and then a failure occurs, on recovery from 
failure T
i
is redone
•such a redo redoes all the original actionsincluding the steps that 
restored old values
Known as repeating history
Seems wasteful, but simplifies recovery greatly

©Silberschatz, Korth and Sudarshan19.101Database System Concepts -7
th
Edition
Example of Checkpoints
T
1
can be ignored (updates already output to disk due to 
checkpoint)
T
2
and T
3
redone.
T
4
undone
T
c
T
f
T
1
T
2
T
3
T
4
checkpoint
system failure
