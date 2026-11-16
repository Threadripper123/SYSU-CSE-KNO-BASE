# ch14 - Indexing

> 主题库：数据库系统原理　｜　源文件：ch14 - Indexing.pdf　｜　37,984 字符　｜　81 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 14: Indexing

©Silberschatz, Korth and Sudarshan14.2Database System Concepts -7
th
Edition
Chapter 14:  Indexing
Basic Concepts
Ordered Indices 
B
+
-Tree Index Files
B-Tree Index Files
Hashing
Write-optimized indices 
Spatio-Temporal Indexing

©Silberschatz, Korth and Sudarshan14.3Database System Concepts -7
th
Edition
Basic Concepts
Indexing mechanisms used to speed up access to desired data.
•E.g., author catalog in library
Search Key- attribute to set of attributes used to look up 
records in a file.
An index file consists of records (called index entries) of the 
form
Index files are typically much smaller than the original file 
Two basic kinds of indices:
•Ordered indices:  search keys are stored in sorted order
•Hash indices:search keys are distributed uniformly across 
“buckets”using a “hash function”. 
search-key
pointer

©Silberschatz, Korth and Sudarshan14.4Database System Concepts -7
th
Edition
Index Evaluation Metrics
Access types supported efficiently.  E.g., 
•records with a specified value in the attribute
•or records with an attribute value falling in a specified range 
of values.
Access time
Insertion time
Deletion time
Space overhead

©Silberschatz, Korth and Sudarshan14.5Database System Concepts -7
th
Edition
Ordered Indices
In an ordered index, index entries are stored sorted on the 
search key value.  
Clustering index: in a sequentially ordered file, the index whose 
search key specifies the sequential order of the file.
•Also called primary index
•The search key of a primary index is usually but not 
necessarily the primary key.
Secondary index: an index whose search key specifies an order 
different from the sequential order of the file.  Also called 
nonclusteringindex.
Index-sequential file:sequential file ordered on a search key, 
with a clustering index on the search key.

©Silberschatz, Korth and Sudarshan14.6Database System Concepts -7
th
Edition
Dense Index Files
Dense index—Index record appears for every search-key 
value in the file. 
E.g. index on IDattribute of instructorrelation 

©Silberschatz, Korth and Sudarshan14.7Database System Concepts -7
th
Edition
Dense Index Files (Cont.)
Dense index on dept_name, with instructor file sorted on 
dept_name

©Silberschatz, Korth and Sudarshan14.8Database System Concepts -7
th
Edition
Sparse Index Files
Sparse Index:  contains index records for only some search-key 
values.
•Applicable when records are sequentially ordered on search-key
To locate a record with search-key value Kwe:
•Find index record with largest search-key value < K
•Search file sequentially starting at the record to which the index 
record points

©Silberschatz, Korth and Sudarshan14.9Database System Concepts -7
th
Edition
Sparse Index Files (Cont.)
Compared to dense indices:
•Less space and less maintenance overhead for insertions and deletions.
•Generally slower than dense index for locating records.
Good tradeoff: 
•for clustered index: sparse index with an index entry for every block in 
file, corresponding to least search-key value in the block.
•For unclusteredindex: sparse index on top of dense index (multilevel 
index)

©Silberschatz, Korth and Sudarshan14.10Database System Concepts -7
th
Edition
Secondary Indices Example
Index record points to a bucket that contains pointers to all the 
actual records with that particular search-key value.
Secondary indices have to be dense
Secondary index on salary field of instructor

©Silberschatz, Korth and Sudarshan14.12Database System Concepts -7
th
Edition
Multilevel Index
If index does not fit in memory, access becomes expensive.
Solution: treat index kept on disk as a sequential file and 
construct a sparse index on it.
•outer index – a sparse index of the basic index
•inner index – the basic index file
If even outer index is too large to fit in main memory, yet 
another level of index can be created, and so on.
Indices at all levels must be updated on insertion or deletion 
from the file.

©Silberschatz, Korth and Sudarshan14.13Database System Concepts -7
th
Edition
Multilevel Index (Cont.)

©Silberschatz, Korth and Sudarshan14.16Database System Concepts -7
th
Edition
Indices on Multiple Keys
Composite search key
•e.g. index on instructorrelation on attributes (name, ID)
•Values are sorted lexicographically
E.g.  (John, 12121) < (John, 13514)  and 
(John, 13514) < (Peter, 11223)
•Can query on just name, or on (name, ID)

©Silberschatz, Korth and Sudarshan14.18Database System Concepts -7
th
Edition
Example of B
+
-Tree

©Silberschatz, Korth and Sudarshan14.19Database System Concepts -7
th
Edition
B
+
-Tree Index Files (Cont.)
All paths from root to leaf are of the same length
Each node that is not a root or a leaf has between n/2and 
nchildren.
A leaf node has between (n–1)/2and n–1 values
Special cases: 
•If the root is not a leaf, it has at least 2 children.
•If the root is a leaf (that is, there are no other nodes in 
the tree), it can have between 0 and (n–1) values.
A B
+
-tree is a rooted tree satisfying the following properties:

©Silberschatz, Korth and Sudarshan14.20Database System Concepts -7
th
Edition
B
+
-Tree Node Structure
Typical node
•K
i
are the search-key values 
•P
i
are pointers to children (for non-leaf nodes) or pointers to 
records or buckets of records (for leaf nodes).
The search-keys in a node are ordered 
K
1 
<K
2 
<K
3 
<. . .<K
n–1
(Initially assume no duplicate keys, address duplicates later)

©Silberschatz, Korth and Sudarshan14.21Database System Concepts -7
th
Edition
Leaf Nodes in B
+
-Trees
For i= 1, 2, . . ., n–1, pointer P
i
points to a file record with 
search-key value K
i
, 
If L
i
,  L
j
are leaf nodes and i< j, L
i
’s search-key values are less 
than or equal to L
j
’s search-key values
P
n
points to next leaf node in search-key order
Properties of a leaf node:

©Silberschatz, Korth and Sudarshan14.22Database System Concepts -7
th
Edition
Non-Leaf Nodes in B
+
-Trees
Non leaf nodes form a multi-level sparse index on the leaf nodes.  
For a non-leaf node with mpointers:
•All the search-keys in the subtree to which P
1
points are less 
than K
1 
•For 2 ≤i ≤n – 1, all the search-keys in the subtree to which P
i
points have values greater than or equal to K
i–1
and less than 
K
i 
•All the search-keys in the subtree to which P
n
points have 
values greater than or equal to K
n–1

©Silberschatz, Korth and Sudarshan14.23Database System Concepts -7
th
Edition
Example of B
+
-tree
Leaf nodes must have between 3 and 5 values 
((n–1)/2and n –1, with n= 6).
Non-leaf nodes other than root must have between 3 
and 6 children ((n/2and n with n=6).
Root must have at least 2 children.
B
+
-tree for instructor file (n= 6)

©Silberschatz, Korth and Sudarshan14.24Database System Concepts -7
th
Edition
Observations about B
+
-trees
Since the inter-node connections are done by pointers, 
“logically”close blocks need not be “physically”close.
The non-leaf levels of the B
+
-tree form a hierarchy of sparse 
indices.
The B
+
-tree contains a relatively small number of levels
Level below root has at least 2* n/2values
Next level has at least 2* n/2* n/2values
.. etc.
•If there are Ksearch-key values in the file, the tree height is 
no more than log
n/2
(K)
•thus searches can be conducted efficiently.
Insertions and deletions to the main file can be handled 
efficiently, as the index can be restructured in logarithmic time (as 
we shall see).

©Silberschatz, Korth and Sudarshan14.25Database System Concepts -7
th
Edition
Queries on B
+
-Trees
functionfind(v)
1.C=root
2.while(C is not a leaf node)
1.Let ibe least number s.t.V ≤K
i
.
2.ifthere is no such number i then 
3.Set C= last non-null pointer in C
4.elseif(v= C.K
i
) Set C = P
i +1  
5.elsesetC= C.P
i
3.iffor some i, K
i
= V  then return C.P
i
4.elsereturn null /* no record with search-key value vexists. */

©Silberschatz, Korth and Sudarshan14.26Database System Concepts -7
th
Edition
Queries on B
+
-Trees (Cont.)
Range queries find all records with search key values in a given range
•See book for details of functionfindRange(lb, ub) which returns set 
of all such records
•Real implementations usually provide an iterator interface to fetch 
matching records one at a time, using a next() function

©Silberschatz, Korth and Sudarshan14.27Database System Concepts -7
th
Edition
Queries on B
+-
Trees (Cont.)
If there are Ksearch-key values in the file, the height of the tree 
is no more than log
n/2
(K).
A node is generally the same size as a disk block, typically 4 
kilobytes
•and nis typically around 100 (40 bytes per index entry).
With 1 million search key values and n= 100
•at most log
50
(1,000,000) = 4 nodes are accessed in a lookup 
traversal from root to leaf.
Contrast this with a balanced binary tree with 1 million search key 
values —around 20 nodes are accessed in a lookup
•above difference is significant since every node access may 
need a disk I/O, costing around 20 milliseconds

©Silberschatz, Korth and Sudarshan14.28Database System Concepts -7
th
Edition
Non-Unique Keys
If a search key a
i
is not unique, create instead an index on a composite 
key (a
i 
, A
p
), which is unique
•A
p
could be a primary key, record ID, or any other attribute that 
guarantees uniqueness
Search for a
i
= v can be implemented by a range search on composite 
key, with range (v, 
-∞) to (v, +∞)
But more I/O operations are needed to fetch the actual records
•If the index is clustering, all accesses are sequential
•If the index is non-clustering, each record access may need an I/O 
operation

©Silberschatz, Korth and Sudarshan14.29Database System Concepts -7
th
Edition
Updates on B
+
-Trees:  Insertion
Assume record already added to the file.  Let 
lprbe pointer to the record, and let 
lv be the search key value of the record
1.Find the leaf node in which the search-key value would appear
1.If there is room in the leaf node, insert (v, pr) pair in the leaf 
node
2.Otherwise, split the node (along with the new (v,    pr)  entry) 
as discussed in the next slide, and propagate updates to 
parent nodes.

©Silberschatz, Korth and Sudarshan14.30Database System Concepts -7
th
Edition
Updates on B
+
-Trees:  Insertion (Cont.)
Splitting a leaf node:
•take the n (search-key value, pointer) pairs (including the one being 
inserted) in sorted order.  Place the first n/2in the original node, 
and the rest in a new node.
•let the new node be p,and let kbe the least key value in p.  Insert 
(k,p) in the parent of the node being split. 
•If the parent is full, split it and propagatethe split further up.
Splitting of nodes proceeds upwards till a node that is not full is found. 
•In the worst case the root node may be split increasing the height of 
the tree by 1. 
Result of splitting node containing Brandt, Califieri and Crick on inserting Adams
Next step: insert entry with (Califieri, pointer-to-new-node) into parent

©Silberschatz, Korth and Sudarshan14.31Database System Concepts -7
th
Edition
B
+
-Tree  Insertion
B
+
-Tree before and after insertion of “Adams”
Affected nodes

©Silberschatz, Korth and Sudarshan14.32Database System Concepts -7
th
Edition
B
+
-Tree  Insertion
B
+
-Tree before and after insertion of “Lamport”
Affected nodes
Affected nodes

©Silberschatz, Korth and Sudarshan14.33Database System Concepts -7
th
Edition
Splitting a non-leaf node: when inserting (k,p) into an already full internal 
node N
•Copy N to an in-memory area M with space for n+1 pointers and n 
keys
•Insert (k,p) into M
•Copy P
1
,K
1
, ..., K 
n/2-1
,P 
n/2
from M back into node N
•Copy P
n/2+1
,K
n/2+1
,...,K
n
,P
n+1
from M into newly allocated node N'
•Insert (K
n/2
,N') into parent N
Example
Read pseudocode in book!
Insertion in B
+
-Trees (Cont.)

©Silberschatz, Korth and Sudarshan14.34Database System Concepts -7
th
Edition
Examples of B
+
-Tree Deletion
Deleting “Srinivasan”causes mergingof under-full leaves
Before and after deleting “Srinivasan”
Affected nodes

©Silberschatz, Korth and Sudarshan14.35Database System Concepts -7
th
Edition
Examples of B
+
-Tree Deletion (Cont.)
Leaf containing Singh and Wu became underfull, and borrowed a value 
Kim from its left sibling
Search-key value in the parent changes as a result
Before and after deleting 
“Singh”and “Wu”
Affected nodes

©Silberschatz, Korth and Sudarshan14.36Database System Concepts -7
th
Edition
Example of B
+
-tree Deletion (Cont.)
Node with Gold and Katz became underfull, and was merged with its sibling 
Parent node becomes underfull, and is merged with its sibling
•Value separating two nodes (at the parent) is pulled down when merging
Root node then has only one child, and is deleted
Before and after deletion of 
“Gold”

©Silberschatz, Korth and Sudarshan14.37Database System Concepts -7
th
Edition
Updates on B
+
-Trees: Deletion
Assume record already deleted from file.  Let V be the search key 
value of the record, and Prbe the pointer to the record.
Remove (Pr, V) from the leaf node 
If the node has too few entries due to the removal, and the 
entries in the node and a sibling fit into a single node, then 
merge siblings:
•Insert all the search-key values in the two nodes into a 
single node (the one on the left), and delete the other node.
•Delete the pair (K
i–1
, P
i
),where P
i
is the pointer to the 
deleted node, from its parent, recursively using the above 
procedure.

©Silberschatz, Korth and Sudarshan14.38Database System Concepts -7
th
Edition
Updates on B
+
-Trees:  Deletion
Otherwise, if the node has too few entries due to the removal, but 
the entries in the node and a sibling do not fit into a single node, 
then redistribute pointers:
•Redistribute the pointers between the node and a sibling such 
that both have more than the minimum number of entries.
•Update the corresponding search-key value in the parent of 
the node.
The node deletions may cascade upwards till a node which has  
n/2or more pointers is found.  
If the root node has only one pointer after deletion, it is deleted 
and the sole child becomes the root. 

©Silberschatz, Korth and Sudarshan14.39Database System Concepts -7
th
Edition
Complexity of Updates
Cost (in terms of number of I/O operations) of insertion and deletion of a 
single entry proportional to height of the tree
•With K entries and maximum fanout of n, worst case complexity of 
insert/delete of an entry is O(log
n/2
(K))
In practice, number of I/O operations is less:
•Internal nodes tend to be in buffer
•Splits/merges are rare, most insert/delete operations only affect a 
leaf node
Average node occupancy depends on insertion order
•2/3rds with random, ½ with insertion in sorted order

©Silberschatz, Korth and Sudarshan14.40Database System Concepts -7
th
Edition
Non-Unique Search Keys
Alternatives to scheme described earlier
•Buckets on separate block (bad idea)
•List of tuple pointers with each key
Extra code to handle long lists
Deletion of a tuple can be expensive if there are many 
duplicates on search key (why?)
•  Worst case complexity may be linear!
Low space overhead, no extra cost for queries
•Make search key unique by adding a record-identifier
Extra storage overhead for keys
Simpler code for insertion/deletion
Widely used

©Silberschatz, Korth and Sudarshan14.41Database System Concepts -7
th
Edition
B
+
-Tree File Organization
B
+
-Tree File Organization:
•leaf nodes in a B
+
-tree file organization store records, instead 
of pointers
•Helps keep data records clustered even when there are 
insertions/deletions/updates
Leaf nodes are still required to be half full
•Since records are larger than pointers, the maximum number 
of records that can be stored in a leaf node is less than the 
number of pointers in a nonleaf node.
Insertion and deletion are handled in the same way as insertion 
and deletion of entries in a B
+
-tree index.

©Silberschatz, Korth and Sudarshan14.42Database System Concepts -7
th
Edition
B
+
-Tree File Organization (Cont.)
Good space utilization important since records use more space than 
pointers.  
To improve space utilization, involve more sibling nodes in redistribution 
during splits and merges
•Involving 2 siblings in redistribution (to avoid split / merge where 
possible) results in each node having at least     entries
Example of B
+
-tree File Organization
  
3/2n

©Silberschatz, Korth and Sudarshan14.43Database System Concepts -7
th
Edition
Other Issues in Indexing
Record relocation and secondary indices
•If a record moves, all secondary indices that store record 
pointers have to be updated 
•Node splits in B
+
-tree file organizations become very 
expensive
•Solution: use search key of B
+
-tree file organization instead of 
record pointer in secondary index
Add record-id if B
+
-tree file organization search key is non-
unique
Extra traversal of file organization to locate record
•  Higher cost for queries, but node splits are cheap

©Silberschatz, Korth and Sudarshan14.44Database System Concepts -7
th
Edition
Indexing Strings
Variable length strings as keys
•Variable fanout
•Use space utilization as criterion for splitting, not number of 
pointers
Prefix compression
•Key values at internal nodes can be prefixes of full key
Keep enough characters to distinguish entries in the 
subtrees separated by the key value
•  E.g. “Silas”and “Silberschatz”can be separated by 
“Silb”
•Keys in leaf node can be compressed by sharing common 
prefixes

©Silberschatz, Korth and Sudarshan14.45Database System Concepts -7
th
Edition
Bulk Loading and Bottom-Up Build
Inserting entries one-at-a -time into a B
+
-tree requires ≥1 IO per entry 
•assuming leaf level does not fit in memory
•can be very inefficient for loading a large number of entries at a time 
(bulk loading)
Efficient alternative 1:
•sort entries first (using efficient external-memory sort algorithms 
discussed later in Section 12.4)
•insert in sorted order
insertion will go to existing page (or cause a split)
much improved IO performance, but most leaf nodes half full
Efficient alternative 2: Bottom-up B
+
-tree construction
•As before sort entries
•And then create tree layer-by-layer, starting with leaf level
details as an exercise
•Implemented as part of bulk-load utility by most database systems

©Silberschatz, Korth and Sudarshan14.48Database System Concepts -7
th
Edition
B-Tree Index File Example
B-tree (above) and B+-tree (below) on same data

©Silberschatz, Korth and Sudarshan14.49Database System Concepts -7
th
Edition
Indexing on Flash
Random I/O cost much lower on flash
•20 to 100 microseconds for read/write
Writes are not in-place, and (eventually) require a more expensive erase
Optimum page size therefore much smaller
Bulk-loading still useful since it minimizes page erases
Write-optimized tree structures (discussed later) have been adapted to 
minimize page writes for flash-optimized search trees

©Silberschatz, Korth and Sudarshan14.50Database System Concepts -7
th
Edition
Indexing in Main Memory
Random access in memory 
•Much cheaper than on disk/flash
•But still expensive compared to cache read
•Data structures that make best use of cache preferable
•Binary search for a key value within a large B
+
-tree node results in 
many cache misses
B
+
- trees with small nodes that fit in cache line are preferable to reduce 
cache misses
Key idea:  use large node size to optimize disk access, but structure 
data within a node using a tree with small node size, instead of using an 
array.

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Hashing

©Silberschatz, Korth and Sudarshan14.52Database System Concepts -7
th
Edition
Static Hashing
A bucketis a unit of storage containing one or more entries 
(a bucket is typically a disk block). 
•we obtain the bucket of an entry from its search-key value 
using a hashfunction
Hash function his a function from the set of all search-key 
values Kto the set of all bucket addresses B.
Hash function is used to locate entries for access, insertion 
as well as deletion.
Entries with different search-key values may be mapped to 
the same bucket; thus entire bucket has to be searched 
sequentially to locate an entry. 
In a hash index, buckets store entries with pointers to 
records
In a hash file-organization buckets store records

©Silberschatz, Korth and Sudarshan14.53Database System Concepts -7
th
Edition
Handling of Bucket Overflows
Bucket overflow can occur because of 
•Insufficient buckets 
•Skew in distribution of records.  This can occur due to two 
reasons:
multiple records have same search-key value
chosen hash function produces non-uniform distribution 
of key values
Although the probability of bucket overflow can be reduced, it 
cannot be eliminated; it is handled by using overflow buckets.

©Silberschatz, Korth and Sudarshan14.54Database System Concepts -7
th
Edition
Handling of Bucket Overflows (Cont.)
Overflow chaining– the overflow buckets of a given bucket are chained 
together in a linked list.
Above scheme is called closed addressing (also called closed hashing or 
open hashing depending on the book you use)
•An alternative, called 
open addressing 
(also called 
open hashing or
closed hashing 
depending on the book 
you use) which does not
use overflow buckets,  
is not suitable for 
database applications.

©Silberschatz, Korth and Sudarshan14.56Database System Concepts -7
th
Edition
Example of Hash File Organization 
Hash file organization of instructorfile, using dept_name as key.

©Silberschatz, Korth and Sudarshan14.57Database System Concepts -7
th
Edition
Deficiencies of Static Hashing
In static hashing, function hmaps search-key values to a fixed 
set of Bof bucket addresses. Databases grow or shrink with 
time. 
•If initial number of buckets is too small, and file grows, 
performance will degrade due to too much overflows.
•If space is allocated for anticipated growth, a significant 
amount of space will be wasted initially (and buckets will be 
underfull).
•If database shrinks, again space will be wasted.
One solution: periodic re-organization of the file with a new hash 
function
•Expensive, disrupts normal operations
Better solution: allow the number of buckets to be modified 
dynamically. 

©Silberschatz, Korth and Sudarshan14.58Database System Concepts -7
th
Edition
Dynamic Hashing
Periodic rehashing
•If number of entries in a hash table becomes (say) 1.5 times 
size of hash table, 
create new hash table of size  (say) 2 times the size of the 
previous hash table
Rehash all entries to new table
Linear Hashing
•Do rehashing in an incremental manner
Extendable Hashing
•Tailored to disk based hashing, with buckets shared by 
multiple hash values
•Doubling of # of entries in hash table, without doubling # of 
buckets

©Silberschatz, Korth and Sudarshan14.59Database System Concepts -7
th
Edition
Comparison of Ordered Indexing and Hashing
Cost of periodic re-organization
Relative frequency of insertions and deletions
Is it desirable to optimize average access time at the expense of 
worst-case access time?
Expected type of queries:
•Hashing is generally better at retrieving records having a 
specified value of the key.
•If range queries are common, ordered indices are to be 
preferred
In practice:
•PostgreSQL supports hash indices, but discourages use due to 
poor performance
•Oracle supports static hash organization, but not hash indices
•SQLServer supports only B
+
-trees

©Silberschatz, Korth and Sudarshan14.60Database System Concepts -7
th
Edition
Multiple-Key Access
Use multiple indices for certain types of queries.
Example: 
select ID
frominstructor
wheredept_name = “Finance”and salary= 80000
Possible strategies for processing query using indices on 
single attributes:
1.Use index on dept_name to find instructors with 
department name Finance; test salary = 80000 
2.Use indexonsalary to find instructors with a salary of 
$80000; testdept_name = “Finance”.
3.Use dept_name index to find pointers to all records 
pertaining to the “Finance”department.  Similarly use 
index on salary.  Take intersection of both sets of pointers 
obtained.

©Silberschatz, Korth and Sudarshan14.61Database System Concepts -7
th
Edition
Indices on Multiple Keys
Composite search keysare search keys containing more than 
one attribute
•E.g. (dept_name, salary)
Lexicographic ordering: (a
1
, a
2
) < (b
1
, b
2
) if either 
•a
1
< b
1
, or 
•a
1
=b
1
and  a
2
< b
2

©Silberschatz, Korth and Sudarshan14.62Database System Concepts -7
th
Edition
Indices on Multiple Attributes
With the whereclause
wheredept_name=“Finance”andsalary = 80000
the index on (dept_name, salary) can be used to fetch only 
records that satisfy both conditions.
•Using separate indices in less efficient —we may fetch 
many records (or pointers) that satisfy only one of the 
conditions.
Can also efficiently handle 
wheredept_name= “Finance”and salary < 80000
But cannot efficiently handle
wheredept_name< “Finance”andbalance = 80000
•May fetch many records that satisfy the first but not the 
second condition
Suppose we have an index on combined search-key
(dept_name, salary).

©Silberschatz, Korth and Sudarshan14.63Database System Concepts -7
th
Edition
Other Features
Covering indices
•Add extra attributes to index so (some) queries can avoid 
fetching the actual records
•Store extra attributes only at leaf
Why?
Particularly useful for secondary indices 
•Why?

©Silberschatz, Korth and Sudarshan14.64Database System Concepts -7
th
Edition
Creation of Indices
E.g.
create indextakes_pkontakes(ID,course_ID, year, semester, section)
drop indextakes_pk
Most database systems allow specification of type of index, and clustering.
Indices on primary key created automatically by all databases
•Why?
Some database also create indices on foreign key attributes
•Why might such an index be useful for this query:
takes ⨝σ
name='Shankar'
(student)
Indices can greatly speed up lookups, but impose cost on updates
•Index tuning assistants/wizards supported on several databases to 
help choose indices, based on query and update workload

©Silberschatz, Korth and Sudarshan14.65Database System Concepts -7
th
Edition
Index Definition in SQL
Create an index
create index<index-name> on<relation-name>
(<attribute-list>)
E.g.:  create index b-index onbranch(branch_name)
Use create unique indexto indirectly specify and enforce the 
condition that the search key is a candidate key is a candidate 
key.
•Not really required if SQL uniqueintegrity constraint is 
supported
To drop an index 
drop index <index-name>
Most database systems allow specification of type of index, and 
clustering.

©Silberschatz, Korth and Sudarshan14.66Database System Concepts -7
th
Edition
Write Optimized Indices
Performance of 
B
+
-trees can be poor for write-intensive workloads
•One I/O per leaf, assuming all internal nodes are in memory
•With magnetic disks, < 100 inserts per second per disk
•With flash memory, one page overwrite per insert
Two approaches to reducing cost of writes
•Log-structured merge tree
•Buffer tree

©Silberschatz, Korth and Sudarshan14.67Database System Concepts -7
th
Edition
Log Structured Merge (LSM) Tree
Consider only inserts/queries for 
now
Records inserted first into in-memory 
tree (L
0
tree)
When in-memory tree is full, records 
moved to disk (L
1
tree)
•B
+
-tree constructed using 
bottom-up build by merging 
existing L
1
tree with records from 
L
0
tree
When L
1
tree exceeds some 
threshold, merge into L
2
tree
•And so on for more levels
•Size threshold for L
i+1
tree is k 
times size threshold for L
i
tree 

©Silberschatz, Korth and Sudarshan14.68Database System Concepts -7
th
Edition
LSM Tree (Cont.)
Benefits of LSM approach
•Inserts are done using only sequential I/O operations
•Leaves are full, avoiding space wastage
•Reduced number of I/O operations per record inserted as compared 
to normal B
+
-tree (up to some size)
Drawback of LSM approach
•Queries have to search multiple trees
•Entire content of each level copied multiple times
Stepped-merge index
•Variant of LSM tree with multiple trees at each level
•Reduces write cost compared to LSM tree
•But queries are even more expensive
Bloom filters to avoid lookups in most trees 
Details are covered in Chapter 24 

©Silberschatz, Korth and Sudarshan14.69Database System Concepts -7
th
Edition
LSM Trees (Cont.)
Deletion handled by adding special “delete” entries
•Lookups will find both original entry and the delete entry, and must 
return only those entries that do not have matching delete entry
•When trees are merged, if we find a delete entry matching an 
original entry, both are dropped.
Update handled using insert+delete
LSM trees were introduced for disk-based indices
•But useful to minimize erases with flash-based indices
•The stepped-merge variant of LSM trees is used in many BigData
storage systems
Google BigTable, Apache Cassandra, MongoDB
And more recently in SQLite4, LevelDB, and MyRocksstorage 
engine of MySQL 

©Silberschatz, Korth and Sudarshan14.70Database System Concepts -7
th
Edition
Buffer Tree
Alternative to LSM tree
Key idea: each internal node of B
+
-tree has a buffer to store inserts
•Inserts are moved to lower levels when buffer is full
•With a large buffer, many records are moved to lower level each 
time
•Per record I/O decreases correspondingly 
Benefits
•Less overhead on queries
•Can be used with any tree index structure
•Used in PostgreSQL Generalized Search Tree (GiST) indices
Drawback: more random I/O than LSM tree

©Silberschatz, Korth and Sudarshan14.71Database System Concepts -7
th
Edition
Bitmap Indices
Bitmap indices are a special type of index designed for efficient 
querying on multiple keys
Records in a relation are assumed to be numbered sequentially 
from, say, 0
•Given a number nit must be easy to retrieve record n
Particularly easy if records are of fixed size
Applicable on attributes that take on a relatively small number of 
distinct values
•E.g. gender, country, state, ...
•E.g. income-level (income broken up into a small number of  
levels such as 0-9999, 10000-19999, 20000-50000, 50000-
infinity)
A bitmap is simply an array of bits

©Silberschatz, Korth and Sudarshan14.72Database System Concepts -7
th
Edition
Bitmap Indices (Cont.)
In its simplest form a bitmap index on an attribute has a bitmap 
for each value of the attribute
•Bitmap has as many bits as records
•In a bitmap for value v, the bit for a record is 1 if the record 
has the value v for the attribute, and is 0 otherwise

©Silberschatz, Korth and Sudarshan14.73Database System Concepts -7
th
Edition
Bitmap Indices (Cont.)
Bitmap indices are useful for queries on multiple attributes 
•not particularly useful for single attribute queries
Queries are answered using bitmap operations
•Intersection (and)
•Union (or)
Each operation takes two bitmaps of the same size and applies 
the operation on corresponding bits to get the result bitmap
•E.g.   100110  AND 110011 = 100010
100110  OR  110011 = 110111
NOT 100110  = 011001
•Males with income level L1:   10010 AND 10100 = 10000
Can then retrieve required tuples.
Counting number of matching tuples is even faster

©Silberschatz, Korth and Sudarshan14.74Database System Concepts -7
th
Edition
Bitmap Indices (Cont.)
Bitmap indices generally very small compared with relation size
•E.g. if record is 100 bytes, space for a single bitmap is 1/800 of 
space used by relation.  
If number of distinct attribute values is 8, bitmap is only 1% 
of relation size

©Silberschatz, Korth and Sudarshan14.75Database System Concepts -7
th
Edition
Efficient Implementation of Bitmap Operations
Bitmaps are packed into words;  a single word and (a basic CPU 
instruction) computes and of 32 or 64 bits at once
•E.g. 1-million-bit maps can be and-edwith just 31,250 instruction
Counting number of 1s can be done fast by a trick:
•Use each byte to index into a precomputed array of 256 elements 
each storing the count of 1s in the binary representation
Can use pairs of bytes to speed up further at a higher memory 
cost
•Add up the retrieved counts
Bitmaps can be used instead of Tuple-ID lists at leaf levels of 
B
+
-trees, for values that have a large number of matching records
•Worthwhile if > 1/64 of the records have that value, assuming a 
tuple-id is 64 bits
•Above technique merges benefits of bitmap and B
+
-tree indices

©Silberschatz, Korth and Sudarshan14.76Database System Concepts -7
th
Edition
SPATIAL AND TEMPORAL 
INDICES

©Silberschatz, Korth and Sudarshan14.77Database System Concepts -7
th
Edition
Spatial Data
Databases can store data types such as lines, polygons, in 
addition to raster images 
•allows relational databases to store and retrieve spatial 
information
•Queries can use spatial conditions (e.g. contains or overlaps).
•queries can mix spatial and nonspatial conditions 
Nearest neighbor queries, given a point or an object, find the 
nearest object that satisfies given conditions.
Range queries deal with spatial regions. e.g., ask for objects 
that lie partially or fully inside a specified region.
Queries that compute intersections or unionsof regions.
Spatial join of two spatial relations with the location playing the 
role of join attribute.

©Silberschatz, Korth and Sudarshan14.78Database System Concepts -7
th
Edition
Indexing of Spatial Data
k-d tree- early structure used for 
indexing in multiple dimensions.
Each level of a k-dtree partitions the 
space into two.
•choose one dimension for partitioning 
at the root level of the tree.
•choose another dimensions for 
partitioning in nodes at the next level 
and so on, cycling through the 
dimensions.
In each node, approximately half of the 
points stored in the sub-tree fall on one 
side and half on the other.
Partitioning stops when a node has 
less than a given number of points.
The k-d-  B treeextends the 
k-dtree to allow multiple 
child nodes for each 
internal node; well-suited 
for secondary storage.

©Silberschatz, Korth and Sudarshan14.79Database System Concepts -7
th
Edition
Division of Space by Quadtrees
Quadtrees
Each node of a quadtree is associated with  a rectangular region of space; the top 
node is associated with the entire target space.
Each non-leaf  nodes divides its region into four equal sized quadrants
•correspondingly each such node has four child nodes corresponding to the 
four quadrants and so on
Leaf nodes have between zero and some fixed maximum number of points (set to 
1 in example).

©Silberschatz, Korth and Sudarshan14.80Database System Concepts -7
th
Edition
R-Trees
R-treesare a N-dimensional extension of B
+
-trees, useful for 
indexing sets of rectangles and other polygons.
Supported in many modern database systems, along with 
variants like R
+
-trees and R*-trees.
Basic idea: generalize the notion of a one-dimensional interval 
associated with each B+ -tree node to an 
N-dimensional interval, that is, an N-dimensional rectangle.
Will consider only the two-dimensional case (N = 2) 
•generalization for N > 2 is  straightforward, although R-trees 
work well only for relatively small N
The bounding box of a node is a minimum  sized rectangle that 
contains all the rectangles/polygons associated with the node
•Bounding boxes of children of a node are allowed to overlap

©Silberschatz, Korth and Sudarshan14.81Database System Concepts -7
th
Edition
Example R-Tree
A set of rectangles (solid line) and the bounding boxes (dashed line) of 
the nodes of an R-tree for the rectangles.
The R-tree is shown on the right.

©Silberschatz, Korth and Sudarshan14.82Database System Concepts -7
th
Edition
Search in R-Trees
To find data items intersecting a given query point/region, do the 
following, starting from the root node:
•If the node is a leaf node, output the data items whose keys intersect the 
given query point/region.
•Else, for each child of the current node whose bounding box intersects 
the query point/region, recursively search the child
Can be very inefficient in worst case since multiple paths may need to be 
searched, but works acceptably in practice.

©Silberschatz, Korth and Sudarshan14.83Database System Concepts -7
th
Edition
Indexing Temporal Data
Temporal data refers to data that has an associated time period 
(interval)
Time interval has a start and end time
•End time set to infinity (or large date such as 9999-12-31) if a tuple 
is currently valid and its validity end time is not currently known
Query may ask for all tuples that are valid at a point in time or during a 
time interval
•Index on valid time period speeds up this task

©Silberschatz, Korth and Sudarshan14.84Database System Concepts -7
th
Edition
Indexing Temporal Data (Cont.)
To create a temporal index on attribute a:
•Use spatial index, such as R-tree, with attribute aas one dimension, 
and time as another dimension
Valid time forms an interval in the time dimension
•Tuples that are currently valid cause problems, since value is infinite or 
very large
Solution:  store all current tuples (with end time as infinity) in a 
separate index, indexed on (a, start-time)
•  To find tuples valid at a point in time t in the current tuple index, 
search for tuples in the range (a, 0) to (a,t)
Temporal index on primary key can help enforce temporal primary key 
constraint

©Silberschatz, Korth and Sudarshan14.85Database System Concepts -7
th
Edition
End of Chapter 14

©Silberschatz, Korth and Sudarshan14.86Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan14.87Database System Concepts -7
th
Edition

©Silberschatz, Korth and Sudarshan14.90Database System Concepts -7
th
Edition
Example of Hash Index
hash index on instructor, on attribute ID
