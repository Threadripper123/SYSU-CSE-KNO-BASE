# ch13 - Data Storage Structures

> 主题库：数据库系统原理　｜　源文件：ch13 - Data Storage Structures.pdf　｜　13,661 字符　｜　29 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 13: Data Storage Structures

©Silberschatz, Korthand Sudarshan13.2Database System Concepts -7
th
Edition
File Organization
The database is stored as a collection of files.  Each file is a 
sequence of records.  A record is a sequence of fields.
One approach
•Assume record size is fixed
•Each file has records of one particular type only
•Different files are used for different relations
This case is easiest to implement; will consider variable length 
records later
We assume that records are smaller than a disk block
.

©Silberschatz, Korthand Sudarshan13.3Database System Concepts -7
th
Edition
Fixed-Length Records
Simple approach:
•Store record istarting from byte n ∗(i  –1), where n is the size of 
each record.
•Record access is simple but records may cross blocks
Modification: do not allow records to cross block boundaries

©Silberschatz, Korthand Sudarshan13.4Database System Concepts -7
th
Edition
Fixed-Length Records
Deletion of record i:  alternatives:
•move records i+ 1, . . ., nto i, . . . , n –1
•move record n to i
•do not move records, but link all free records on a free list
Record 3 deleted

©Silberschatz, Korthand Sudarshan13.5Database System Concepts -7
th
Edition
Fixed-Length Records
Deletion of record i:  alternatives:
•move records i+ 1, . . ., nto i, . . . , n –1
•move record n to i
•do not move records, but link all free records on a free list
Record 3 deleted and replaced by record 11

©Silberschatz, Korthand Sudarshan13.6Database System Concepts -7
th
Edition
Fixed-Length Records
Deletion of record i:  alternatives:
•move records i+ 1, . . ., nto i, . . . , n –1
•move record n to i
•do not move records, but link all free records on a free list

©Silberschatz, Korthand Sudarshan13.7Database System Concepts -7
th
Edition
Variable-Length Records
Variable-length records arise in database systems in several ways:
•Storage of multiple record types in a file.
•Record types that allow variable lengths for one or more fields such 
as strings (varchar)
•Record types that allow repeating fields (used in some older data 
models).
Attributes are stored in order
Variable length attributes represented by fixed size (offset, length), with 
actual data stored after all fixed length attributes
Null values represented by null-value bitmap

©Silberschatz, Korthand Sudarshan13.8Database System Concepts -7
th
Edition
Variable-Length Records: Slotted Page Structure
Slotted pageheader contains:
•number of record entries
•end of free space in the block
•location and size of each record
Records can be moved around within a page to keep them contiguous 
with no empty space between them; entry in the header must be 
updated.
Pointers should not point directly to record —instead they should 
point to the entry for the record in header.

©Silberschatz, Korthand Sudarshan13.9Database System Concepts -7
th
Edition
Storing Large Objects
E.g. blob/clobtypes
Records must be smaller than pages
Alternatives:
•Store as files in file systems
•Store as files managed by database
•Break into pieces and store in multiple tuples in separate relation
PostgreSQL TOAST

©Silberschatz, Korthand Sudarshan13.10Database System Concepts -7
th
Edition
Organization of Records in Files
Heap– record can be placed anywhere in the file where there is 
space
Sequential– store records in sequential order, based on the value 
of the search key of each record
In a  multitableclustering file organization records of several 
different relations can be stored in the same file
•Motivation: store related records on the same block to 
minimize I/O
B
+
-tree file organization
•Ordered storage even with inserts/deletes
•More on this in Chapter 14
Hashing– a hash function computed on search key; the result 
specifies in which block of the file the record should be placed
•More on this in Chapter 14

©Silberschatz, Korthand Sudarshan13.11Database System Concepts -7
th
Edition
Heap File Organization
Records can be placed anywhere in the file where there is free space
Records usually do not move once allocated
Important to be able to efficiently find free space within file
Free-space map
•Array with 1 entry per block.  Each entry is a few bits to a byte, 
and records fraction of block that is free
•In example below, 3 bits per block, value divided by 8 indicates 
fraction of block that is free
•Can have second-level free-space map
•In example below, each entry stores maximum from 4 entries of 
first-level free-space map
Free space map written to disk periodically, OK to have wrong (old) 
values for some entries (will be detected and fixed)

©Silberschatz, Korthand Sudarshan13.12Database System Concepts -7
th
Edition
Sequential File Organization
Suitable for applications that require sequential processing of 
the entire file 
The records in the file are ordered by a search-key

©Silberschatz, Korthand Sudarshan13.13Database System Concepts -7
th
Edition
Sequential File Organization (Cont.)
Deletion – use pointer chains
Insertion –locate the position where the record is to be inserted
•if there is free space insert there 
•if no free space, insert the record in an overflow block
•In either case, pointer chain must be updated
Need to reorganize the file
from time to time to restore
sequential order

©Silberschatz, Korthand Sudarshan13.14Database System Concepts -7
th
Edition
Multitable Clustering File Organization
Store several relations in one file using a multitableclustering 
file organization
department
instructor
multitable clustering
ofdepartment and
instructor

©Silberschatz, Korthand Sudarshan13.15Database System Concepts -7
th
Edition
Multitable Clustering File Organization (cont.)
good for queries involving department ⨝instructor, and for 
queries involving one single department and its instructors
bad for queries involving only department
results in variable size records
Can add pointer chains to link records of a particular relation

©Silberschatz, Korthand Sudarshan13.16Database System Concepts -7
th
Edition
Partitioning
Table partitioning: Records in a relation can be partitioned into 
smaller relations that are stored separately
E.g. transaction relation may be partitioned into 
transaction_2018, transaction_2019, etc.
Queries written on transactionmust access records in all partitions
•Unless query has a selection such as year=2019, in which case 
only one partition in needed
Partitioning 
•Reduces costs of some operations such as free space 
management
•Allows different partitions to be stored on different storage 
devices 
E.g. transaction partition for current year on SSD, for older 
years on magnetic disk

©Silberschatz, Korthand Sudarshan13.17Database System Concepts -7
th
Edition
Data Dictionary Storage
Information about relations
•names of relations
•names, types and lengths of attributes of each relation
•names and definitions of views
•integrity constraints
User and accounting information, including passwords
Statistical and descriptive data
•number of tuples in each relation
Physical file organization information
•How relation is stored (sequential/hash/...)
•Physical location of relation 
Information about indices (Chapter 14) 
TheData dictionary(also called system catalog) stores 
metadata; that is, data about data, such as

©Silberschatz, Korthand Sudarshan13.18Database System Concepts -7
th
Edition
Relational Representation of System Metadata
Relational 
representation on 
disk
Specialized data 
structures 
designed for 
efficient access, in 
memory

©Silberschatz, Korthand Sudarshan13.19Database System Concepts -7
th
Edition
Storage Access
Blocks are units of both storage allocation and data transfer.
Database system seeks to minimize the number of block transfers 
between the disk and memory.  We can reduce the number of disk 
accesses by keeping as many blocks as possible in main memory.
Buffer– portion of main memory available to store copies of disk 
blocks.
Buffer manager– subsystem responsible for allocating buffer space 
in main memory.

©Silberschatz, Korthand Sudarshan13.20Database System Concepts -7
th
Edition
Buffer Manager
Programs call on the buffer manager when they need a block from 
disk.
•If the block is already in the buffer, buffer manager returns the 
address of the block in main memory
•If the block is not in the buffer, the buffer manager
Allocates space in the buffer for the block
•  Replacing (throwing out) some other block, if required, to 
make space for the new block.
•  Replaced block written back to disk only if it was modified 
since the most recent time that it was written to/fetched 
from the disk.
Reads the block from the disk to the buffer, and returns the 
address of the block in main memory to requester. 

©Silberschatz, Korthand Sudarshan13.21Database System Concepts -7
th
Edition
Buffer Manager
Buffer replacement strategy (details coming up!)
Pinned block: memory block that is not allowed to be written back to disk
•Pindone before reading/writing data from a block
•Unpindone when read /write is complete
•Multiple concurrent pin/unpin operations possible
Keep a pin count, buffer block can be evicted only if pin count = 0
Shared and exclusive locks on buffer
•Needed to prevent concurrent operations from reading page contents 
as they are moved/reorganized, and to ensure only one 
move/reorganize at a time
•Readers get shared lock, updates to a block require exclusive lock
•Locking rules:
Only one process can get exclusive lock at a time
Shared lock cannot be concurrently with exclusive lock
Multiple processes may be given shared lock concurrently

©Silberschatz, Korthand Sudarshan13.22Database System Concepts -7
th
Edition
Buffer-Replacement Policies
Most operating systems replace the block least recently used(LRU 
strategy)
•Idea behind LRU – use past pattern of block references as a 
predictor of future references
•LRU can be bad for some queries
Queries have well-defined access patterns (such as sequential 
scans), and a database system can use the information in a user’s 
query to predict future references
Mixed strategy with hints on replacement strategy provided
by the query optimizer is preferable
Example of bad access pattern for LRU: when computing the join of 2 
relations r and s by a nested loops 
for each tuple trof rdo 
for each tuple tsof sdo 
if the tuples trand tsmatch ...

©Silberschatz, Korthand Sudarshan13.23Database System Concepts -7
th
Edition
Buffer-Replacement Policies (Cont.)
Toss-immediatestrategy – frees the space occupied by a block as soon 
as the final tuple of that block has been processed
Most recently used (MRU) strategy–system must pin the block 
currently being processed.  After the final tuple of that block has been 
processed, the block is unpinned, and it becomes the most recently used 
block.
Buffer manager can use statistical information regarding the probability 
that a request will reference a particular relation
•E.g., the data dictionary is frequently accessed.  Heuristic:  keep 
data-dictionary blocks in main memory buffer
Operating system or buffer manager may reorder writes
•Can lead to corruption of data structures on disk
E.g. linked list of blocks with missing block on disk
File systems perform consistency check to detect such situations
•Careful ordering of writes can avoid many such problems

©Silberschatz, Korthand Sudarshan13.24Database System Concepts -7
th
Edition
Optimization of Disk Block Access (Cont.)
Buffer managers support forced outputof blocks for the purpose of recovery 
(more in Chapter 19)
Nonvolatile write buffersspeed up disk writes by writing blocks to a non-
volatile RAM or flash buffer immediately
•Writes can be reordered to minimize disk arm movement
Log disk– a disk devoted to writing a sequential log of block updates
•Used exactly like nonvolatile RAM
Write to log disk is very fast since no seeks are required
Journaling file systems write data in-order to NV-RAM or log disk
•Reordering without journaling: risk of corruption of file system data

©Silberschatz, Korthand Sudarshan13.25Database System Concepts -7
th
Edition
Column-Oriented Storage
Also known as columnarrepresentation
Store each attribute of a relation separately

©Silberschatz, Korthand Sudarshan13.26Database System Concepts -7
th
Edition
Columnar Representation
Benefits:
•Reduced IO if only some attributes are accessed
•Improved CPU cache performance 
•Improved compression
•Vector processing on modern CPU architectures
Drawbacks
•Cost of tuple reconstruction from columnar representation
•Cost of tuple deletion and update
•Cost of decompression
Columnar representation found to be more efficient for decision 
support than row-oriented representation
Traditional row-oriented representation preferable for transaction 
processing
Some databases support both representations
•Called hybrid row/column stores

©Silberschatz, Korthand Sudarshan13.27Database System Concepts -7
th
Edition
Columnar File Representation
ORC and Parquet: file 
formats with columnar 
storage inside file
Very popular for big-data 
applications
Orc file format shown on 
right:

©Silberschatz, Korthand Sudarshan13.28Database System Concepts -7
th
Edition
Storage Organization in 
Main-Memory Databases
Can store records directly in 
memory without a buffer 
manager
Column-oriented storage can be 
used in-memory for decision 
support applications
•Compression reduces 
memory requirement

©Silberschatz, Korthand Sudarshan13.29Database System Concepts -7
th
Edition
End of Chapter 13
