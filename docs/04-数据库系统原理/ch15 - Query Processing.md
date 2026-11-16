# ch15 - Query Processing

> 主题库：数据库系统原理　｜　源文件：ch15 - Query Processing.pdf　｜　33,727 字符　｜　59 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 15: Query Processing

©Silberschatz, Korthand Sudarshan15.2Database System Concepts -7
th
Edition
Chapter 15:  Query Processing
Overview 
Measures of Query Cost
Selection Operation  
Sorting 
Join Operation 
Other Operations
Evaluation of Expressions

©Silberschatz, Korthand Sudarshan15.3Database System Concepts -7
th
Edition
Basic Steps in Query Processing
1.Parsing and translation
2.Optimization
3.Evaluation

©Silberschatz, Korthand Sudarshan15.4Database System Concepts -7
th
Edition
Basic Steps in Query Processing (Cont.)
Parsing and translation
•translate the query into its internal form.  This is then translated 
into relational algebra.
•Parser checks syntax, verifies relations
Evaluation
•The query-execution engine takes a query-evaluation plan, 
executes that plan, and returns the answers to the query.

©Silberschatz, Korthand Sudarshan15.5Database System Concepts -7
th
Edition
Basic Steps in Query Processing: 
Optimization
A relational algebra expression may have many equivalent 
expressions
•E.g., σ
salary<75000
(∏
salary
(instructor)) is equivalent to 
∏
salary
(σ
salary<75000
(instructor))
Each relational algebra operation can be evaluated using one of 
several different algorithms
•Correspondingly, a relational-algebra expression can be evaluated 
in many ways. 
Annotated expression specifying detailed evaluation strategy is called 
an evaluation-plan.  E.g.:
•Use an index on salaryto find instructors with salary < 75000,
•Or perform complete relation scan and discard instructors with 
salary ≥75000

©Silberschatz, Korthand Sudarshan15.6Database System Concepts -7
th
Edition
Basic Steps: Optimization (Cont.)
Query Optimization: Amongst all equivalent evaluation plans choose 
the one with lowest cost. 
•Cost is estimated using statistical information from the
database catalog
e.g. number of tuples in each relation, size of tuples, etc.
In this chapter we study
•How to measure query costs
•Algorithms for evaluating relational algebra operations
•How to combine algorithms for individual operations in order to 
evaluate a complete expression
In Chapter 16
•We study how to optimize queries, that is, how to find an 
evaluation plan with lowest estimated cost

©Silberschatz, Korthand Sudarshan15.7Database System Concepts -7
th
Edition
Measures of Query Cost
Many factors contribute to time cost
•disk access, CPU, and network communication
Cost can be measured based on 
•response time, i.e. total elapsed time for answering query, or
•total resource consumption
We use total resource consumption as cost metric
•Response time harder to estimate, and minimizing resource 
consumption is a good idea in a shared database
We ignore CPU costs for simplicity
•Real systems do take CPU cost into account
•Network costs must be considered for parallel systems
We describe how estimate the cost of each operation
•We do not include cost to writing output to disk

©Silberschatz, Korthand Sudarshan15.8Database System Concepts -7
th
Edition
Measures of Query Cost
Disk cost can be estimated as:
•Number of seeks             * average-seek-cost
•Number of blocks read     * average-block-read-cost
•Number of blocks written * average-block-write-cost
For simplicity we just use the number of block transfersfrom disk 
and the number of seeksas the cost measures
•t
T
– time to transfer one block
Assuming for simplicity that write cost is same as read cost
•t
S
– time for one seek
•Cost for b block transfers plus S seeks
b * t
T
+ S * t
S
t
S
andt
T
depend on where data is stored; with 4 KB blocks:
•High end magnetic disk: t
S
= 4 msecand t
T
=0.1 msec
•SSD: : t
S
= 20-90 microsec and t
T
= 2-10 microsec for 4KB 

©Silberschatz, Korthand Sudarshan15.9Database System Concepts -7
th
Edition
Measures of Query Cost (Cont.)
Required data may be buffer resident already, avoiding disk I/O
•But hard to take into account for cost estimation
Several algorithms can reduce disk IO by using extra buffer space 
•Amount of real memory available to buffer depends on other 
concurrent queries and OS processes, known only during 
execution
Worst case estimates assume that no data is initially in buffer  and 
only the minimum amount of memory needed for the operation is 
available
•But more optimistic estimates are used in practice

©Silberschatz, Korthand Sudarshan15.10Database System Concepts -7
th
Edition
Selection Operation
File scan
Algorithm A1(linear search).  Scan each file block and test all 
records to see whether they satisfy the selection condition.
•Cost estimate = b
r
block transfers + 1 seek
b
r
denotes number of blocks containing records from relation r
•If selection is on a key attribute, can stop on finding record
cost = (b
r
/2) block transfers + 1 seek
•Linear search can be applied regardless of 
selection condition or
ordering of records in the file, or 
availability of indices
Note: binary search generally does not make sense since data is not 
stored consecutively
•except when there is an index available, 
•and binary search requires more seeks than index search

©Silberschatz, Korthand Sudarshan15.11Database System Concepts -7
th
Edition
Selections Using Indices
Index scan – search algorithms that use an index
•selection condition must be on search-key of index.
A2 (clustering index, equality on key).  Retrieve a single record that 
satisfies the corresponding equality condition  
•Cost= (h
i
+ 1) * (t
T
+ t
S
)
A3 (clustering index, equality on nonkey) Retrieve multiple 
records. 
•Records will be on consecutive blocks
Let b = number of blocks containing matching records
•Cost= h
i
* (t
T
+ t
S
) + t
S
+ t
T
* b

©Silberschatz, Korthand Sudarshan15.12Database System Concepts -7
th
Edition
Selections Using Indices
A4(secondary index, equality on key/non-key).
•Retrieve a single record if the search-key is a candidate key
Cost = (h
i
+ 1) * (t
T
+ t
S
)
•Retrieve multiple records if search-key is not a candidate key
each of nmatching records may be on a different block  
Cost =  (h
i
+ n) * (t
T
+ t
S
)
•  Can be very expensive!

©Silberschatz, Korthand Sudarshan15.13Database System Concepts -7
th
Edition
Selections Involving Comparisons
Can implement selections of the form σ
A≤V 
(r) or σ
A ≥V
(r) by using
•a linear file scan,
•or by using indices in the following ways:
A5(clustering index, comparison).(Relation is sorted on A)
For σ
A ≥V
(r)use index to find first tuple ≥vand scan relation 
sequentially  from there
For σ
A≤V 
(r) just scan relation sequentially till first tuple > v; do 
not use index
A6(clustering index, comparison). 
For σ
A ≥V
(r)use index to find first index entry ≥vand scan 
index sequentially  from there, to find pointers to records.
For σ
A≤V 
(r) just scan leaf pages of index finding pointers to 
records, till first entry > v
In either case, retrieve records that are pointed to
requires an I/O per record;Linear file scan may be cheaper!

©Silberschatz, Korthand Sudarshan15.14Database System Concepts -7
th
Edition
Implementation of Complex Selections
Conjunction:  σ
θ1
∧
θ2
∧. . . 
θn
(r)
A7(conjunctive selection using one index).
•Select a combination of θ
i
and algorithms A1 through A7 that 
results in the least cost for σ
θi
(r).
•Test other conditions on tuple after fetching it into memory buffer.
A8(conjunctive selection using composite index).  
•Use appropriate composite (multiple-key) index if available.
A9(conjunctive selection by intersection of identifiers).
•Requires indices with record pointers. 
•Use corresponding index for each condition, and take intersection 
of all the obtained sets of record pointers. 
•Then fetch records from file
•If some conditions do not have appropriate indices, apply test in 
memory.

©Silberschatz, Korthand Sudarshan15.15Database System Concepts -7
th
Edition
Algorithms for Complex Selections
Disjunction:σ
θ1
∨
θ2 
∨. . . 
θn 
(r).
A10(disjunctive selection by union of identifiers). 
•Applicable if all conditions have available indices.  
Otherwise use linear scan.
•Use corresponding index for each condition, and take union of all 
the obtained sets of record pointers. 
•Then fetch records from file
Negation:  σ
¬θ
(r)
•Use linear scan on file
•If very few records satisfy ¬θ, and an index is applicable to θ
Find satisfying records using index and fetch from file

©Silberschatz, Korthand Sudarshan15.16Database System Concepts -7
th
Edition
Bitmap Index Scan
The bitmap index scan algorithm of PostgreSQL
•Bridges gap between secondary index scan and linear file scan when 
number of matching records is not known before execution
•Bitmap with 1 bit per page in relation
•Steps:
Index scan used to find record ids, and set bit of corresponding page 
in bitmap
Linear file scan fetching only pages with bit set to 1
•Performance
Similar to index scan when only a few bits are set
Similar to linear file scan when most bits are set
Never behaves very badly compared to best alternative

©Silberschatz, Korthand Sudarshan15.17Database System Concepts -7
th
Edition
Sorting
We may build an index on the relation, and then use the index to read 
the relation in sorted order.  May lead to one disk block access for 
each tuple.
For relations that fit in memory, techniques like quicksort can be used.  
For relations that don’t fit in memory, external 
sort-merge is a good choice. 

©Silberschatz, Korthand Sudarshan15.18Database System Concepts -7
th
Edition
Example: External Sorting Using Sort-Merge

©Silberschatz, Korthand Sudarshan15.19Database System Concepts -7
th
Edition
External Sort-Merge
1.Create sortedruns.  Let ibe 0 initially.
Repeatedly do the following till the end of the relation:
(a)  Read Mblocks of relation into memory
(b)  Sort the in-memory blocks
(c)  Write sorted data to run R
i
; increment i.
Let the final value ofibe N
2.Merge the runs (next slide).....
Let Mdenotememory size (in pages). 

©Silberschatz, Korthand Sudarshan15.20Database System Concepts -7
th
Edition
External Sort-Merge (Cont.)
2.Merge the runs (N-way merge). We assume (for now) that N< M. 
1.Use Nblocks of memory to buffer input runs, and 1 block to buffer 
output. Read the first block of each run into its buffer page
2.repeat
1.Select the first record (in sort order) among all buffer pages
2.Write the record to the output buffer.  If the output buffer is full 
write it to disk.
3.Delete the record from its input buffer page.
Ifthe buffer page becomes empty then
read the next block (if any) of the run into the buffer. 
3.untilall input buffer pages are empty:

©Silberschatz, Korthand Sudarshan15.21Database System Concepts -7
th
Edition
External Sort-Merge (Cont.)
If N≥M, several merge passesare required.
•In each pass, contiguous groups of M - 1 runs are merged. 
•A pass reduces the number of runs by a factor of M-1, and 
creates runs longer by the same factor. 
E.g.  If M=11, and there are 90 runs, one pass reduces the 
number of runs to 9, each 10 times the size of the initial runs
•Repeated passes are performed till all runs have been merged 
into one.

©Silberschatz, Korthand Sudarshan15.22Database System Concepts -7
th
Edition
External Merge Sort (Cont.)
Cost analysis:
•1  block per run leads to too many seeks during merge
Instead use b
b
buffer blocks per run
read/write b
b
blocks at a time
Can merge M/b
b
–1 runs in one pass
•Total number of merge passes required: log 
M/bb–1
(b
r
/M).
•Block transfers for initial run creation as well as in each pass is 2b
r
for final pass, we don’t count write cost 
•  we ignore final write cost for all operations since the output 
of an operation may be sent to the parent operation without 
being written to disk
Thus total number of block transfers for external sorting:
b
r
( 2 log
M/bb–1 
(b
r
/ M)+ 1) 
•Seeks: next slide

©Silberschatz, Korthand Sudarshan15.23Database System Concepts -7
th
Edition
External Merge Sort (Cont.)
Cost of seeks
•During run generation: one seek to read each run and one seek to 
write each run
2 b
r
/ M
•During the merge phase
Need 2 b
r
/ b
b
seeks for each merge pass 
•  except the final one which does not require a write
Total number of seeks:
2 b
r
/ M+ b
r
/ b
b
(2 log
M/bb–1
(b
r
/ M)-1)

©Silberschatz, Korthand Sudarshan15.24Database System Concepts -7
th
Edition
Join Operation
Several different algorithms to implement joins
•Nested-loop join
•Block nested-loop join
•Indexed nested-loop join
•Merge-join
•Hash-join
Choice based on cost estimate
Examples use the following information
•Number of records of student:  5,000     takes: 10,000
•Number of blocks of   student:     100     takes:      400

©Silberschatz, Korthand Sudarshan15.25Database System Concepts -7
th
Edition
Nested-Loop Join
To compute the theta join        r⨝
θ
s
for eachtuple t
r
in rdo begin
for each tuple t
s
in sdo begin
test pair (t
r
,t
s
) tosee if they satisfy the join condition θ
if they do, add t
r
• t
s
to the result.
end
end
ris called the outerrelationand sthe inner relationof the join.
Requires no indices and can be used with any kind of join condition.
Expensive since it examines every pair of tuples in the two relations. 

©Silberschatz, Korthand Sudarshan15.26Database System Concepts -7
th
Edition
Nested-Loop Join (Cont.)
In the worst case, if there is enough memory only to hold one block 
of each relation, the estimated cost is 
n
r
∗b
s
+b
r
block transfers, plus      n
r
+b
r
seeks
If the smaller relation fits entirely in memory, use that as the inner 
relation.
•Reduces cost to b
r
+ b
s
block transfers and 2 seeks
Assuming worst case memory availability cost estimate is
•with student as outer relation:
5000 ∗400 + 100 = 2,000,100 block transfers,
5000 + 100 = 5100 seeks 
•with takes as the outer relation 
10000 ∗100 + 400 = 1,000,400 block transfers and 10,400 seeks
If smaller relation (student) fits entirely in memory, the cost 
estimate will be 500 block transfers.
Block nested-loops algorithm (next slide) is preferable.

©Silberschatz, Korthand Sudarshan15.27Database System Concepts -7
th
Edition
Block Nested-Loop Join
Variant of nested-loop join in which every block of inner relation is 
paired with every block of outer relation.
for each block B
r
ofrdo begin
for eachblock B
s
of s do begin
for eachtuple t
r
in B
r 
do begin
for each tuple t
s
in B
s
do begin
Check if (t
r
,t
s
) satisfy the join condition 
if they do, add t
r
• t
s
to the result.
end
end
end
end

©Silberschatz, Korthand Sudarshan15.29Database System Concepts -7
th
Edition
Indexed Nested-Loop Join
Index lookups can replace file scans if
•join is an equi-join or natural join and
•an index is available on the inner relation’s join attribute
Can construct an index just to compute a join.
For each tuple t
r
in the outer relation r,use the index to look up tuples 
in sthat satisfy the join condition with tuple t
r
.
Worst case:  buffer has space for only one page of r, and, for each 
tuple in r, we perform an index lookup on s.
Cost of the join:  b
r
(t
T
+ t
S
) + n
r
∗c
•Where cis the cost of traversing index and fetching all matching stuples 
for one tuple or r
•ccan be estimated as cost of a single selection on susing the join 
condition.
If indices are available on join attributes of both r and s,
use the relation with fewer tuples as the outer relation.

©Silberschatz, Korthand Sudarshan15.31Database System Concepts -7
th
Edition
Merge-Join
1.Sort both relations on their join attribute (if not already sorted on the 
join attributes).
2.Merge the sorted relations to join them
1.Join step is similar to the merge stage of the sort-merge 
algorithm.  
2.Main difference is handling of duplicate values in join attribute —
every pair with same value on join attribute must be matched
3.Detailed algorithm in book

©Silberschatz, Korthand Sudarshan15.32Database System Concepts -7
th
Edition
Merge-Join (Cont.)
Can be used only for equi-joins and natural joins
Each block needs to be read only once (assuming all tuples for any 
given value of the join attributes fit in memory
Thus the cost of merge join is: 
b
r
+ b
s
block transfers  + b
r
/ b
b
+ b
s
/ b
b
seeks
•+ the cost of sorting if relations are unsorted.
hybrid merge-join: If one relation is sorted, and the other has a 
secondary B
+
-tree index on the join attribute
•Merge the sorted relation with the leaf entries of the B
+
-tree . 
•Sort the result on the addresses of the unsorted relation’s tuples
•Scan the unsorted relation in physical address order and merge 
with previous result, to replace addresses by the actual tuples
Sequential scan more efficient than random lookup

©Silberschatz, Korthand Sudarshan15.33Database System Concepts -7
th
Edition
Hash-Join
Applicable for equi-joins and natural joins.
A hash functionhis used to partition tuples of both relations 
hmaps JoinAttrsvalues to {0, 1, ..., n}, where JoinAttrs denotes the 
common attributes of rand s used in the natural join. 
•r
0
, r
1
, . . ., r
n
denote partitions of r tuples
Each tuple t
r
∈r is put in partition r
i
where i = h(t
r
[JoinAttrs]).
•r
0
,, r
1
. . ., r
n
denotes partitions of stuples
Each tuple t
s
∈sis put in partition s
i
, where i = h(t
s
[JoinAttrs]).
Note: In book,  Figure 12.10 r
i   
is denoted as H
ri, 
s
i 
is denoted as H
si  
and
nis denoted as n
h. 

©Silberschatz, Korthand Sudarshan15.34Database System Concepts -7
th
Edition
Hash-Join (Cont.)

©Silberschatz, Korthand Sudarshan15.36Database System Concepts -7
th
Edition
Hash-Join Algorithm
1.Partition the relation susing hashing function h.  When 
partitioning a relation, one block of memory is reserved as 
the output buffer for each partition.
2.Partition rsimilarly.
3.For each i:
(a)Load s
i
into memory and build an in-memory hash index 
on it using the join attribute.  This hash index uses a 
different hash function than the earlier one h.
(b)Read the tuples in r
i
from the disk one by one.  For each 
tuple t
r
locate each matching tuple t
s
in s
i
using the in-
memory hash index.  Output the concatenation of their 
attributes.
The hash-join of rand s is computed as follows.
Relation sis called the build inputand  r is called the probe input.

©Silberschatz, Korthand Sudarshan15.37Database System Concepts -7
th
Edition
Hash-Join algorithm (Cont.)
The value nand the hash function his chosen such that each s
i
should fit in memory.
•Typically n is chosen as b
s
/M* f  where f is a “fudge factor”, 
typically around 1.2
•The probe relation partitions s
i
need not fit in memory
Recursive partitioningrequired if number of partitions n is greater 
than number of pages Mof memory.
•instead of partitioning nways, useM –1 partitions for s
•Further partition the M –1 partitions using a different hash function
•Use same partitioning method on r
•Rarely required: e.g., with block size of 4 KB, recursive partitioning 
not needed for relations of < 1GB with memory size of 2MB, or 
relations of < 36 GB with memory of 12 MB

©Silberschatz, Korthand Sudarshan15.38Database System Concepts -7
th
Edition
Handling of Overflows
Partitioning is said to be skewedif some partitions have significantly 
more tuples than some others
Hash-table overflowoccurs in partition s
i
if s
i
does not fit in memory.  
Reasons could be
•Many tuples in s with same value for join attributes
•Bad hash function
Overflow resolutioncan be done in build phase
•Partition s
i
is further partitioned using different hash function. 
•Partition r
i
must be similarly partitioned.
Overflow avoidanceperforms partitioning carefully to avoid 
overflows during build phase
•E.g. partition build relation into many partitions, then combine them
Both approaches fail with large numbers of duplicates
•Fallback option: use block nested loops join on overflowed  partitions

©Silberschatz, Korthand Sudarshan15.39Database System Concepts -7
th
Edition
Cost of Hash-Join
If recursive partitioning is not required: cost of hash join is
3(b
r
+b
s
)+4 ∗n
h  
block transfers +
2
( b
r 
/ b
b
+ b
s 
/ b
b
)  seeks
If recursive partitioning required:
•number of passes required for partitioningbuild relation s to less 
than M blocks per partition is log
M/bb
–1
(b
s
/M)
•best to choose the smaller relation as the build relation.
•Total cost estimate is: 
2(b
r
+ b
s
)log
M/bb
–1
(b
s
/M)+ b
r
+ b
s  
block transfers + 
2
(b
r 
/ b
b
+ b
s 
/ b
b
) log
M/bb
–1
(b
s
/M) seeks
If the entire build input can be kept in main memory no partitioning is 
required
•Cost estimate goes down to b
r
+ b
s
.

©Silberschatz, Korthand Sudarshan15.41Database System Concepts -7
th
Edition
Hybrid Hash–Join
Useful when memory sized are relatively large, and the build input is 
bigger than memory.
Main feature of hybrid hash join:
Keep the first partition of the build relation in memory.
E.g. With memory size of 25 blocks, instructor can be partitioned into 
five partitions, each of size 20 blocks.
•Division of memory:
The first partition occupies 20 blocks of memory
1 block is used for input, and 1 block each for buffering the other 4 
partitions.
teaches is similarly partitioned into five partitions each of size 80
•the first is used right away for probing, instead of being written out
Cost of 3(80 + 320) + 20 +80 = 1300 block transfers for
hybrid hash join, instead of 1500 with plain hash-join.
Hybrid hash-join most useful if M>> 
s
b

©Silberschatz, Korthand Sudarshan15.42Database System Concepts -7
th
Edition
Complex Joins
Join with a conjunctive condition:
r ⨝
θ1∧  θ2∧... ∧ θn
s
•Either use nested loops/block nested loops, or
•Compute the result of one of the simpler joins r ⨝
θi
s
final result comprises those tuples in the intermediate result 
that satisfy the remaining conditions
θ
1
∧. . . ∧θ
i–1
∧θ
i+1
∧. . . ∧θ
n
Join with a disjunctive condition
r 
⨝
θ
1
∨
θ
2
∨... ∨
θ
n
s
•Either use nested loops/block nested loops, or
•Compute as the union of the records in individual joins r ⨝
θ
i
s:
(r ⨝
θ
1
s) ∪(r ⨝
θ
2
s) ∪. . . ∪(r ⨝
θ
n
s) 

©Silberschatz, Korthand Sudarshan15.43Database System Concepts -7
th
Edition
Joins over Spatial Data
No simple sort order for spatial joins
Indexed nested loops join with spatial indices
•R-trees, quad-trees, k-d -B-trees

©Silberschatz, Korthand Sudarshan15.44Database System Concepts -7
th
Edition
Other Operations
Duplicate elimination can be implemented via hashing or sorting.
•On sorting duplicates will come adjacent to each other, and all but 
one set of duplicates can be deleted.  
•Optimization: duplicates can be deleted during run generation as 
well as at intermediate merge steps in external sort-merge.
•Hashing is similar – duplicates will come into the same bucket.
Projection:
•perform projection on each tuple 
•followed by duplicate elimination. 

©Silberschatz, Korthand Sudarshan15.45Database System Concepts -7
th
Edition
Other Operations : Aggregation
Aggregationcan be implemented in a manner similar to duplicate 
elimination.
•Sortingor hashingcan be used to bring tuples in the same group 
together, and then the aggregate functions can be applied on each 
group.
•Optimization: partial aggregation
combine tuples in the same group during run generation and 
intermediate merges, by computing partial aggregate values
For count, min, max, sum: keep aggregate values on tuples 
found so far in the group.  
•  When combining partial aggregate for count, add up the 
partial aggregates
For avg, keep sum and count, and divide sum by count at the 
end

©Silberschatz, Korthand Sudarshan15.46Database System Concepts -7
th
Edition
Other Operations : Set Operations
Set operations (∪, ∩and ):  can either use variant of merge-join 
after sorting, or variant of hash-join.
E.g., Set operations using hashing:
1.Partition both relations using the same hash function
2.Process each partition ias follows.  
1.Using a different hashing function, build an in-memory hash 
index on r
i
.
2.Process s
i
as follows
r ∪s:  
1.Add tuples in s
i
to the hash index if they are not already 
in it.  
2.At end of s
i
add the tuples in the hash index to the 
result.

©Silberschatz, Korthand Sudarshan15.47Database System Concepts -7
th
Edition
Other Operations : Set Operations
E.g., Set operations using hashing:
1.as before partition rand s, 
2.as before, process each partition ias follows
1.build a hash index on r
i
2.Process s
i
as follows
•r∩s: 
1.output tuples in s
i
to the result if they are already there 
in the hash index
•r–s:
1.for each tuple in s
i
, if it is there in the hash index, delete 
it from the index. 
2.At end of s
i
add remaining tuples in the hash index to 
the result. 

©Silberschatz, Korthand Sudarshan15.48Database System Concepts -7
th
Edition
Answering Keyword Queries
Indices mapping keywords to documents
•For each keyword, store sorted list of document IDs that contain the 
keyword
Commonly referred to as a inverted index
E.g.: database:  d1, d4, d11, d45, d77, d123
distributed:  d4, d8, d11, d56, d77, d121, d333
•To answer a query with several keywords, compute intersection of lists 
corresponding to those keywords
To support ranking, inverted lists store extra information
•“Term frequency” of the keyword in the document
•“Inverse document frequency” of the keyword
•Page rank of the document/web page

©Silberschatz, Korthand Sudarshan15.49Database System Concepts -7
th
Edition
Other Operations : Outer Join
Outer join can be computed either as 
•A join followed by addition of null-padded non-participating tuples.
•by modifying the join algorithms.
Modifying merge join to compute r ⟕s
•In r ⟕s, non participating tuples are those in r –Π
R
(r ⨝s)
•Modify merge-join to compute r ⟕s:  
During merging, for every tuple t
r
from r that do not match any 
tuple in s, output t
r
padded with nulls.
•Right outer-join and full outer-join can be computed similarly.

©Silberschatz, Korthand Sudarshan15.50Database System Concepts -7
th
Edition
Other Operations : Outer Join
Modifying hash join to compute r ⟕s
•If  ris probe relation, output non-matching rtuples padded with 
nulls
•If ris build relation, when probing keep track of which 
rtuples matched stuples.  At end ofs
i
output 
non-matched rtuples padded with nulls 

©Silberschatz, Korthand Sudarshan15.51Database System Concepts -7
th
Edition
Evaluation of Expressions
So far: we have seen algorithms for individual operations
Alternatives for evaluating an entire expression tree
•Materialization:  generate results of an expression whose inputs 
are relations or are already computed, materialize(store) it on 
disk.  Repeat.
•Pipelining:  pass on tuples to parent operations even as an 
operation is being executed
We study above alternatives in more detail

©Silberschatz, Korthand Sudarshan15.52Database System Concepts -7
th
Edition
Materialization
Materialized evaluation:  evaluate one operation at a time, starting at 
the lowest-level.  Use intermediate results materialized into temporary 
relations to evaluate next-level operations.
E.g., in figure below, compute and store
then compute the store its join with instructor, and finally compute the 
projection on name. 
)(
"
Watson"
department
building=
σ

©Silberschatz, Korthand Sudarshan15.53Database System Concepts -7
th
Edition
Materialization (Cont.)
Materialized evaluation is always applicable
Cost of writing results to disk and reading them back can be quite 
high
•Our cost formulas for operations ignore cost of writing results to 
disk, so
Overall cost  =  Sum of costs of individual operations + 
cost of writing intermediate results to disk
Double buffering: use two output buffers for each operation, when 
one is full write it to disk while the other is getting filled
•Allows overlap of disk writes with computation and reduces 
execution time

©Silberschatz, Korthand Sudarshan15.54Database System Concepts -7
th
Edition
Pipelining
Pipelined evaluation:  evaluate several operations simultaneously, 
passing the results of one operation on to the next.
E.g., in previous expression tree, don’t store result of
•instead, pass tuples directly to the join..  Similarly, don’t store 
result of join, pass tuples directly to projection. 
Much cheaper than materialization: no need to store a temporary 
relation to disk.
Pipelining may not always be possible – e.g., sort, hash-join. 
For pipelining to be effective, use evaluation algorithms that generate 
output tuples even as tuples are received for inputs to the operation. 
Pipelines can be executed in two ways:  demand drivenand 
producer driven
)(
"
Watson"
department
building=
σ

©Silberschatz, Korthand Sudarshan15.55Database System Concepts -7
th
Edition
Pipelining (Cont.)
In demand driven or lazyevaluation
•system repeatedly requests next tuple  from top level operation
•Each operation requests  next tuple from children operations as required, 
in order to output its next tuple
•In between calls, operation has to maintain “state”so it knows what to 
return next
In producer-drivenor eagerpipelining
•Operators produce tuples eagerly and pass them up to their parents
Buffer maintained between operators, child puts tuples in buffer, 
parent removes tuples from buffer
if buffer is full, child waits till there is space in the buffer, and then 
generates more tuples
•System schedules operations that have space in output buffer and can 
process more input tuples
Alternative name: pulland pushmodels of pipelining

©Silberschatz, Korthand Sudarshan15.56Database System Concepts -7
th
Edition
Pipelining (Cont.)
Implementation of demand-driven pipelining
•Each operation is implemented as an iteratorimplementing the 
following operations
open()
•  E.g. file scan: initialize file scan
state: pointer to beginning of file
•  E.g.mergejoin: sort relations;
state: pointers to beginning of sorted relations
next()
•  E.g. for file scan: Output next tuple, and advance and store 
file pointer
•  E.g. for merge join:  continue with merge from earlier state 
till 
next output tuple is found.  Save pointers as iterator state.
close()

©Silberschatz, Korthand Sudarshan15.57Database System Concepts -7
th
Edition
Blocking Operations
Blocking operations:  cannot generate any output until all input is 
consumed
•E.g. sorting, aggregation, ...
But can often consume inputs from a pipeline, or produce outputs to a 
pipeline
Key idea: blocking operations often have two suboperations
•E.g. for sort:  run generation and merge
•For hash join:  partitioning and build-probe 
Treat them as separate operations

©Silberschatz, Korthand Sudarshan15.58Database System Concepts -7
th
Edition
Pipeline Stages
Pipeline stages: 
•All operations in a stage run concurrently
•A stage can start only after preceding stages have completed execution

©Silberschatz, Korthand Sudarshan15.59Database System Concepts -7
th
Edition
Evaluation Algorithms for Pipelining
Some algorithms are not able to output results even as they get input 
tuples
•E.g. merge join, or hash join
•intermediate results written to disk and then read back
Algorithm variants to generate (at least some) results on the fly, as 
input tuples are read in
•E.g. hybrid hash join generates output tuples even as probe relation 
tuples in the in-memory partition (partition 0) are read in
•Double-pipelined join technique: Hybrid hash join, modified to buffer 
partition 0 tuples of both relations in-memory, reading them as they 
become available, and output results of any matches between partition 0 
tuples
When a new r
0
tuple is found, match it with existing s
0
tuples, output 
matches, and save it in r
0
Symmetrically for s
0
tuples

©Silberschatz, Korthand Sudarshan15.60Database System Concepts -7
th
Edition
Pipelingfor Continuous-Stream Data
Data streams
•Data entering database in a continuous manner
•E.g.  Sensor networks, user clicks, ...
Continuous queries
•Results get updated as streaming data enters the database
•Aggregation on windows is often used
E.g. tumbling windows divide time into units, e.g. hours, minutes
Need to use pipelined processing algorithms
•Punctuationsused to infer when all data for a window has been received

©Silberschatz, Korthand Sudarshan15.61Database System Concepts -7
th
Edition
Query Processing in Memory
Query compilation to machine code
•Overheads of interpretation
E.g. repeatedly finding attribute location within tuple, from metadata 
Overhead of expression evaluation
•Compilation can avoid many such overheads and speed up query 
processing
•Often via generation of Java byte code / LLVM, with just-in -time (JIT) 
compilation
Column-oriented storage
•Allows vector operations (in conjunction with compilation)
Cache conscious algorithms

©Silberschatz, Korthand Sudarshan15.62Database System Concepts -7
th
Edition
Cache Conscious Algorithms
Goal: minimize cache misses, make best use of data fetched into the 
cache as part of a cache line
For sorting:
•Use runs that are as large as L3 cache (a few megabytes) to avoid cache 
misses during sorting of a run
•Then merge runs as usual in merge-sort
For hash-join
•First create partitions such that build+probepartitions fit in memory
•Then subpartitionfurther s.t.build subpartition+indexfits in L3 cache
Speeds up probe phase significantly by avoiding cache misses
Lay out attributes of tuples to maximize cache usage
•Attributes that are often accessed together should be stored adjacent to 
each other
Use multiple threads for parallel query processing
•Cache misssleads to stall of one thread, but others can proceed

©Silberschatz, Korthand Sudarshan15.63Database System Concepts -7
th
Edition
End of Chapter 15
