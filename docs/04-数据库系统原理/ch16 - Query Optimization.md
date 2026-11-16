# ch16 - Query Optimization

> 主题库：数据库系统原理　｜　源文件：ch16 - Query Optimization.pdf　｜　44,950 字符　｜　85 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 16: Query Optimization

©Silberschatz, Korthand Sudarshan16.2Database System Concepts -7
th
Edition
Chapter 16:  Query Optimization
Introduction 
Transformation of Relational Expressions
Catalog Information for Cost Estimation
Statistical Information for Cost Estimation
Cost-based optimization
Dynamic Programming for Choosing Evaluation Plans
Materialized views 

©Silberschatz, Korthand Sudarshan16.3Database System Concepts -7
th
Edition
Introduction
Alternative ways of evaluating a given query
•Equivalent expressions
•Different algorithms for each operation

©Silberschatz, Korthand Sudarshan16.4Database System Concepts -7
th
Edition
Introduction (Cont.)
An evaluation plandefines exactly what algorithm is used for each 
operation, and how the execution of the operations is coordinated.
Find out how to view query execution plans on your favorite 
database

©Silberschatz, Korthand Sudarshan16.5Database System Concepts -7
th
Edition
Introduction (Cont.)
Cost difference between evaluation plans for a query can be 
enormous
•E.g. seconds vs. days in some cases
Steps in cost-based query optimization
1.Generate logically equivalent expressions using equivalence 
rules
2.Annotate resultant expressions to get alternative query plans
3.Choose the cheapest plan based on estimated cost
Estimation of plan cost based on:
•Statistical information about relations. Examples:
number of tuples, number of distinct values for an attribute
•Statistics estimation for intermediate results
to compute cost of complex expressions
•Cost formulae for algorithms, computed using statistics

©Silberschatz, Korthand Sudarshan16.6Database System Concepts -7
th
Edition
Viewing Query Evaluation Plans
Most database support  explain<query>
•Displays plan chosen by query optimizer, along with cost estimates
•Some syntax variations between databases
Oracle:  explain plan for <query> followed by select* fromtable 
(dbms_xplan.display)
SQL Server:  set showplan_texton
Somedatabases(e.g.   PostgreSQL)supportexplainanalyse
<query>
•Showsactualruntimestatisticsfoundbyrunningthequery,inadditionto
showingtheplan
Some databases (e.g. PostgreSQL) show cost as   f..l
•fis the cost of delivering first tuple and lis cost of delivering all results 

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Generating Equivalent Expressions

©Silberschatz, Korthand Sudarshan16.8Database System Concepts -7
th
Edition
Transformation of Relational 
Expressions
Two relational algebra expressions are said to be equivalentif the 
two expressions generate the same set of tuples on every legal
database instance
•Note: order of tuples is irrelevant
•we don’t care if they generate different results on databases that 
violate integrity constraints
In SQL, inputs and outputs are multisets of tuples
•Two expressions in the multiset version of the relational algebra 
are said to be equivalent if the two expressions generate the same 
multiset of tuples on every legal database instance. 
An equivalence rulesays that expressions of two forms are 
equivalent
•Can replace expression of first form by second, or vice versa

©Silberschatz, Korthand Sudarshan16.9Database System Concepts -7
th
Edition
Equivalence Rules
1.Conjunctive selection operations can be deconstructed into a 
sequence of individual selections.
σ
θ
1 
∧θ
2
(E) ≡  σ
θ
1
(σ
θ
2
(E))
2.Selection operations are commutative.
σ
θ
1
(σ
θ
2
(E))≡    σ
θ
2
(σ
θ
1
(E))
3.Only the last in a sequence of projection operations is needed, the 
others can be omitted.
∏
L
1
(∏
L
2
(...(∏
L
n
(E))...))     ≡     ∏
L
1
(E)
whereL
1
⊆L
2
... ⊆L
n
4.Selections can be combined with Cartesian products and theta joins.
a.σ
θ
(E
1
x E
2
)≡    E
1
⨝
θ
E
2
b.σ
θ
1
(E
1
⨝
θ
2
E
2
)≡    E
1
⨝
θ
1
∧
θ
2
E
2

©Silberschatz, Korthand Sudarshan16.10Database System Concepts -7
th
Edition
Equivalence Rules (Cont.)
5.Theta-join operations (and natural joins) are commutative.
E
1
⨝E
2
≡    E
2
⨝E
1
6.(a) Natural join operations are associative:
(E
1
⨝E
2
) ⨝E
3
≡     E
1
⨝(E
2
⨝E
3
)
(b) Theta joins are associative in the following manner:
(E
1
⨝
θ
1
E
2
) ⨝
θ
2 
∧θ
3
E
3
≡    E
1
⨝
θ
1 
∧θ
3
(E
2
⨝
θ
2
E
3
)
where θ
2
involves attributes from only E
2
and E
3
.

©Silberschatz, Korthand Sudarshan16.11Database System Concepts -7
th
Edition
Pictorial Depiction of Equivalence Rules

©Silberschatz, Korthand Sudarshan16.12Database System Concepts -7
th
Edition
Equivalence Rules (Cont.)
7.The selection operation distributes over the theta join operation under 
the following two conditions:
(a)  When all the attributes in θ
0 
involve only the attributes of one 
of the expressions (E
1
) being joined.
σ
θ
0 
(E
1 
⨝
θ
E
2
)      ≡    (σ
θ
0
(E
1
)) ⨝
θ
E
2
(b) When θ
1 
involves only the attributes of E
1
andθ
2 
involves  
only the attributes of E
2
.
σ
θ
1
∧θ
2
(E
1
⨝
θ
E
2
)     ≡(σ
θ
1
(E
1
))⨝
θ
(σ
θ
2
(E
2
))

©Silberschatz, Korthand Sudarshan16.13Database System Concepts -7
th
Edition
8.The projection operation distributes over the theta join operation as 
follows:
(a) if θinvolves only attributes from L
1
∪L
2
:
∏
L
1
∪L
2
(E
1
⨝
θ
E
2
)     ≡     ∏
L
1
(E
1
) ⨝
θ
∏
L
2
(E
2
)
(b) In general, consider a join E
1
⨝
θ
E
2
. 
•Let L
1
and L
2
be sets of attributes from E
1
and E
2
, respectively.  
•Let L
3
be attributes of E
1
that are involved in join condition θ, but 
are not in L
1
∪L
2
, and
•let L
4
be attributes of E
2 
that are involved in join condition θ, but 
are not in L
1
∪L
2
.
∏
L
1
∪L
2
(E
1
⨝
θ
E
2
)     ≡    ∏
L
1
∪L
2
(∏
L
1
∪L
3
(E
1
) ⨝
θ
∏
L
2
∪L
4
(E
2
))
Similar equivalences hold for outerjoinoperations: ⟕, ⟖, and ⟗
Equivalence Rules (Cont.)

©Silberschatz, Korthand Sudarshan16.14Database System Concepts -7
th
Edition
Equivalence Rules (Cont.)
9.The set operations union and intersection are commutative
E
1
∪E
2     
≡   E
2
∪E
1 
E
1
∩E
2     
≡   E
2
∩E
1
(set difference is not commutative).
10.Set union and intersection are associative.
(E
1
∪E
2 
) ∪E
3    
≡E
1
∪(E
2
∪E
3
)
(E
1
∩E
2 
) ∩E
3    
≡   E
1
∩(E
2
∩E
3
)
11.The selection operation distributes over ∪, ∩and –. 
a.  
σ
θ
(E
1
∪E
2
)    ≡   σ
θ
(E
1
) ∪σ
θ
(E
2
)
b.  
σ
θ
(E
1
∩E
2
)    ≡σ
θ
(E
1
) ∩σ
θ
(E
2
)
c.   
σ
θ
(E
1
–E
2
)    ≡   σ
θ
(E
1
) –σ
θ
(E
2
)
d.   
σ
θ
(E
1
∩E
2
)   ≡σ
θ
(E
1
) ∩E
2
e.   σ
θ
(E
1
–E
2
)    ≡σ
θ
(E
1
) –E
2
preceding equivalence does not hold for∪
12.The projection operation distributes over union
Π
L
(E
1
∪E
2
)     ≡(Π
L
(E
1
)) ∪(Π
L
(E
2
)) 

©Silberschatz, Korthand Sudarshan16.15Database System Concepts -7
th
Edition
Exercise
Create equivalence rules involving
•The group by/aggregation operation
•Left outer join operation

©Silberschatz, Korthand Sudarshan16.16Database System Concepts -7
th
Edition
Equivalence Rules (Cont.)
13.Selection distributes over aggregation as below
σ
θ
(
G
훾훾
A
(E))    ≡   
G
훾훾
A
(σ
θ
(E)) 
provided θonly involves attributes in G
14.a. Full outerjoinis commutative:
E
1
⟗E
2     
≡   E
2
⟗E
1 
b. Left and right outerjoinare not commutative, but:
E
1
⟕E
2     
≡   E
2
⟖E
1
15.Selection distributes over left and right outerjoinsas below, provided 
θ
1
only involves attributes of E
1
a. σ
θ
1
(E
1
⟕
θ
E
2
)    ≡   (σ
θ
1
(E
1
)) ⟕
θ
E
2
b. σ
θ
1
(E
1
⟖
θ
E
2
)    ≡    E
2 
⟕
θ
(σ
θ
1
(E
1
))
16.Outerjoinscan be replaced by inner joins under some conditions
a. 
σ
θ
1
(E
1
⟕
θ
E
2
)    ≡   σ
θ
1
(E
1
⨝
θ
E
2
)
b. 
σ
θ
1
(E
1
⟖
θ
E
1
)    ≡    σ
θ
1
(E
1
⨝
θ
E
2
)
provided θ
1
is null rejecting on E
2

©Silberschatz, Korthand Sudarshan16.17Database System Concepts -7
th
Edition
Equivalence Rules (Cont.)
Note that several equivalences that hold for joins do not hold for outerjoins
σ
year=2017
(instructor⟕teaches)  ≢σ
year=2017
(instructor⨝teaches)
Outerjoinsare not associative
(r ⟕s) ⟕t     ≢r ⟕(s ⟕t)
•e.g. with r(A,B) = {(1,1),    s(B,C) = { (1,1)},   t(A,C) = { }

©Silberschatz, Korthand Sudarshan16.18Database System Concepts -7
th
Edition
Transformation Example: Pushing Selections
Query:  Find the names of all instructors in the Music department, 
along with the titles of the courses that they teach
•Π
name, title
(σ
dept_name= ‘Music’
(instructor ⨝(teaches ⨝Π
course_id, title
(course))))
Transformation using rule 7a.
•Π
name, title
((σ
dept_name= ‘Music’
(instructor))⨝
(teaches ⨝Π
course_id, title
(course)))
Performing the selection as early as possible reduces the size of the 
relation to be joined. 

©Silberschatz, Korthand Sudarshan16.20Database System Concepts -7
th
Edition
Multiple Transformations (Cont.)

©Silberschatz, Korthand Sudarshan16.22Database System Concepts -7
th
Edition
Join Ordering Example
For all relations r
1, 
r
2, 
and r
3
,
(r
1
⨝r
2
) ⨝r
3  
= r
1
⨝(r
2
⨝r
3 
)
(Join Associativity)⨝
If r
2
⨝r
3 
is quite large and r
1
⨝r
2
is small, we choose
(r
1
⨝r
2
) ⨝r
3
so that we compute and store a smaller temporary relation.

©Silberschatz, Korthand Sudarshan16.23Database System Concepts -7
th
Edition
Join Ordering Example (Cont.)
Consider the expression
Π
name, title
(σ
dept_name= “Music”
(instructor)⨝teaches) 
⨝Π
course_id, title
(course))))
Could compute   teaches ⨝Π
course_id, title
(course)first, and join 
result with 
σ
dept_name= “Music”
(instructor)
but  the result of the first joinis likely to be a large relation.
Only a small fraction of the university’s instructors are likely to be 
from the Music department
•it is better to compute
σ
dept_name= “Music”
(instructor) ⨝teaches 
first.

©Silberschatz, Korthand Sudarshan16.24Database System Concepts -7
th
Edition
Enumeration of Equivalent Expressions
Query optimizers use equivalence rules to systematicallygenerate 
expressions equivalent to the given expression
Can generate all equivalent expressions as follows: 
•Repeat
apply all applicable equivalence  rules on every subexpression 
of every equivalent expression found so far
add newly generated expressions to the set of equivalent 
expressions 
Until no new equivalent expressions are generated above
The above approach is very expensive in space and time
•Two approaches
Optimized plan generation based on transformation rules
Special case approach for queries with only selections, 
projections and joins

©Silberschatz, Korthand Sudarshan16.25Database System Concepts -7
th
Edition
Implementing Transformation Based Optimization
Space requirements reduced by sharing common sub-expressions:
•when E1 is generated from E2 by an equivalence rule, usually only the top level of 
the two are different, subtrees below are the same and can be shared using 
pointers
E.g. when applying join commutativity
•Same sub-expression may get generated multiple times
Detect duplicate sub-expressions and share one copy
Time requirements are reduced by not generating all expressions
•Dynamic programming
We will study only the special case of dynamic programming for join order 
optimization
E1
E2

©Silberschatz, Korthand Sudarshan16.26Database System Concepts -7
th
Edition
Cost Estimation
Cost of each operator computer as described in Chapter 15
•Need statistics of input relations
E.g. number of tuples, sizes of tuples
Inputs can be results of sub-expressions
•Need to estimate statistics of expression results
•To do so, we require additional statistics
E.g. number of distinct values for an attribute
More on cost estimation later

©Silberschatz, Korthand Sudarshan16.27Database System Concepts -7
th
Edition
Choice of Evaluation Plans
Must consider the interaction of evaluation techniques when choosing 
evaluation plans
•choosing the cheapest algorithm for each operation independently 
may not yield best overall algorithm.  E.g.
merge-join may be costlier than hash-join, but may provide a 
sorted output which reduces the cost for an outer level 
aggregation.
nested-loop join may provide opportunity for pipelining
Practical query optimizers incorporate elements of the following two 
broad approaches:
1.Search all the plans and choose the best plan in a 
cost-based fashion.
2. Uses heuristics to choose a plan.

©Silberschatz, Korthand Sudarshan16.28Database System Concepts -7
th
Edition
Cost-Based Optimization
Consider finding the best join-order for r
1
⨝r
2 
⨝. . .⨝r
n
.
There are (2(n– 1))!/(n– 1)! different join orders for above 
expression.  With n= 7, the number is 665280, with n = 10, the
number is greater than 176 billion!
No need to generate all the join orders.  Using dynamic programming, 
the least-cost join order for any subset of 
{r
1
, r
2
, . . . r
n
} is computed only once and stored for future use.  

©Silberschatz, Korthand Sudarshan16.29Database System Concepts -7
th
Edition
Dynamic Programming in Optimization
To find best join tree for a set of nrelations:
•To find best plan for a set Sof nrelations, consider all possible 
plans of the form:  S
1
⨝(S – S
1
) where S
1
is any non-empty 
subset of S.
•Recursively compute costs for joining subsets of Sto find the cost 
of each plan.  Choose the cheapest of the 2
n
– 2 alternatives.
•Base case for recursion:  single relation access plan
Apply all selections on R
i 
using best choice of indices on R
i
•When plan for any subset is computed, store it and reuse it when 
it is required again, instead of recomputingit
Dynamic programming

©Silberschatz, Korthand Sudarshan16.30Database System Concepts -7
th
Edition
Join Order Optimization Algorithm
procedurefindbestplan(S)
if(bestplan[S].cost ≠∞)
return bestplan[S]
// else bestplan[S] has not been computed earlier, compute it now
if(Scontains only 1 relation)
set bestplan[S].planand bestplan[S].costbased on the best way 
of accessing Susing selections on Sand indices (if any) on S 
else for each non-empty subset S1of S such that S1 ≠S
P1= findbestplan(S1)
P2= findbestplan(S-S1)
for each algorithm A for joining results of P1 and P2
... compute plan and cost of using A (see next page) ..
if cost < bestplan[S].cost 
bestplan[S].cost = cost
bestplan[S].plan = plan;
returnbestplan[S]

©Silberschatz, Korthand Sudarshan16.31Database System Concepts -7
th
Edition
Join Order Optimization Algorithm (cont.)
for each algorithm A for joining results of P1 and P2
// For indexed-nested loops join, the outer could be P1or P2
// Similarly for hash-join, the build relation could beP1 orP2
//   We assume the alternatives are considered as separate algorithms
if algorithm Ais indexed nested loops 
Let P
i
and P
o
denote inner and outer inputs
if P
i
has a single relation r
i
and r
i
has an index on the join attribute
plan = “execute P
o
.plan; join results of P
o
and r
i
using A”, 
with any selection conditions on P
i
performed as part of
the join condition
cost = P
o
.cost+ cost of A
elsecost = ∞; /* cannot use indexed nested loops join */
else 
plan = “execute P1.plan; execute P2.plan; 
join results of P1and P2using A;” 
cost = P1.cost+ P2.cost+ cost of A
.... See previous page

©Silberschatz, Korthand Sudarshan16.32Database System Concepts -7
th
Edition
Left Deep Join Trees
In left-deep join trees,the right-hand-side input for each 
join is a relation, not the result of an intermediate join.

©Silberschatz, Korthand Sudarshan16.33Database System Concepts -7
th
Edition
Cost of Optimization
With dynamic programming time complexity of optimization with 
bushy trees is O(3
n
).  
•With n = 10, this number is 59000 instead of 176 billion!
Space complexity is O(2
n
) 
To find best left-deep join tree for a set of nrelations:
•Consider n alternatives with one relation as right-hand side input and the 
other relations as left-hand side input.
•Modify optimization algorithm:
Replace “for each non-empty subset S1 of S such that S1 ≠S”
By:   for each relation r in S
let S1 = S –r .
If only left-deep trees are considered, time complexity of finding best 
join order is O(n 2
n
)
•Space complexity remains at O(2
n
) 
Cost-based optimization is expensive, but worthwhile for queries on 
large datasets (typical queries have small n, generally < 10)

©Silberschatz, Korthand Sudarshan16.34Database System Concepts -7
th
Edition
Interesting Sort Orders
Consider the expression (r
1
⨝r
2
) ⨝r
3
(with A as common attribute)
An interesting sort order is a particular sort order of tuples that 
could make a later operation (join/group by/order by) cheaper
•Using merge-join to compute r
1
⨝r
2
may be costlier than hash join but 
generates result sorted on A
•Which in turn may make merge-join with r
3
cheaper, which may reduce 
cost of join with r
3
and minimizing overall cost 
Not sufficient to find the best join order for each subset of the set of n
given relations
•must find the best join order for each subset, for each interesting sort 
order
•Simple extension of earlier dynamic programming algorithms
•Usually, number of interesting orders is quite small and doesn’t affect 
time/space complexity significantly

©Silberschatz, Korthand Sudarshan16.35Database System Concepts -7
th
Edition
Cost Based Optimization with Equivalence Rules
Physical equivalence rulesallow logical query plan to be converted 
to physical query plan specifying what algorithms are used for each 
operation.
Efficient optimizer based on equivalent rules depends on
•A space efficient representation of expressions which avoids 
making multiple copies of subexpressions
•Efficient techniques for detecting duplicate derivations of 
expressions
•A form of dynamic programming based on memoization, which 
stores the best plan for a subexpression the first time it is 
optimized, and reuses in on repeated optimization calls on same 
subexpression
•Cost-based pruning techniques that avoid generating all plans
Pioneered by the Volcano project and implemented in the SQL Server 
optimizer

©Silberschatz, Korthand Sudarshan16.36Database System Concepts -7
th
Edition
Heuristic Optimization
Cost-based optimization is expensive, even with dynamic 
programming.
Systems may use heuristics to reduce the number of choices that 
must be made in a cost-based fashion.
Heuristic optimization transforms the query-tree by using a set of rules 
that typically (but not in all cases) improve execution performance:
•Perform selection early (reduces the number of tuples)
•Perform projection early (reduces the number of attributes)
•Perform most restrictive selection and join operations (i.e. with 
smallest result size) before other similar operations.
•Some systems use only heuristics, others combine heuristics with 
partial cost-based optimization.

©Silberschatz, Korthand Sudarshan16.37Database System Concepts -7
th
Edition
Structure of Query Optimizers
Many optimizers considers only left-deep join orders.
•Plus heuristics to push selections and projections down the query 
tree
•Reduces optimization complexity and generates plans amenable 
to pipelined evaluation.
Heuristic optimization used in some versions of Oracle:
•Repeatedly pick “best”relation to join next 
Starting from each of n starting points.  Pick best among these
Intricacies of SQL complicate query optimization
•E.g. nested subqueries

©Silberschatz, Korthand Sudarshan16.38Database System Concepts -7
th
Edition
Structure of Query Optimizers (Cont.)
Some query optimizers integrate heuristic selection and the 
generation of alternative access plans.
•Frequently used approach
heuristic rewriting of nested block structure and aggregation
followed by cost-based join-order optimization for each block
•Some optimizers (e.g. SQL Server) apply transformations to entire query 
and do not depend on block structure
•Optimization cost budgetto stop optimization early (if cost of plan is 
less than cost of optimization)
•Plan cachingto reuse previously computed plan if query is resubmitted
Even with different constants in query  
Even with the use of heuristics, cost-based query optimization 
imposes a substantial overhead.
•But is worth it for expensive queries
•Optimizers often use simple heuristics for very cheap queries, and 
perform exhaustive enumeration for more expensive queries 

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Statistics for Cost Estimation

©Silberschatz, Korthand Sudarshan16.40Database System Concepts -7
th
Edition
Statistical Information for Cost Estimation
n
r
:  number of tuples in a relation r.
b
r
: number of blocks containing tuples of r.
l
r
: size of a tuple of r.
f
r
: blocking factor of r—i.e., the number of tuples of r that fit into one 
block.
V(A, r):number of distinct values that appear in rfor attribute A; same 
as the size of ∏
A
(r).
If tuples of rare stored together physically in a file, then: 

©Silberschatz, Korthand Sudarshan16.41Database System Concepts -7
th
Edition
Histograms
Histogram on attribute ageof relation person
Equi-widthhistograms
Equi-depthhistograms break up range such that each range has 
(approximately) the same number of tuples
•E.g. (4, 8, 14, 19) 
Many databases also store n most-frequent values and their counts
•Histogram is built on remaining values only

©Silberschatz, Korthand Sudarshan16.42Database System Concepts -7
th
Edition
Histograms (cont.)
Histograms and other statistics usually computed based on a random  
sample
Statistics may be out of date
•Some database require a analyzecommand to be executed to update 
statistics
•Others automatically recompute statistics 
e.g. when number of tuples in a relation changes by some percentage

©Silberschatz, Korthand Sudarshan16.43Database System Concepts -7
th
Edition
σ
A=v
(r)
n
r
/ V(A,r) : number of records that will satisfy the selection
Equality condition on a key attribute:size estimate = 1
σ
A≤V
(r) (case of σ
A ≥V
(r) is symmetric)
Let c denote  the estimated number of tuples satisfying the 
condition. 
If min(A,r) and max(A,r) are available in catalog
c = 0 if v < min(A,r)
c =
If histograms available, can refine above estimate
In absence of statistical informationc is assumed to ben
r
/ 2.
Selection Size Estimation

©Silberschatz, Korthand Sudarshan16.44Database System Concepts -7
th
Edition
Size Estimation of Complex Selections
The selectivity of a condition θ
i
is the probability that a tuple in the 
relation rsatisfies θ
i
. 
•If s
i
is the number of satisfying tuples in r, the selectivity of θ
i
is given by 
s
i
/n
r
.
Conjunction:  σ
θ
1∧
θ
2∧. . . ∧
θ
n
(r).  Assuming independence, estimate of 
tuples in theresult is:
Disjunction:σ
θ
1∨
θ
2∨. . . ∨
θ
n 
(r). Estimated number of tuples:
Negation:  σ
¬θ
(r). Estimated number of tuples:
n
r
– size(σ
θ
(r))

©Silberschatz, Korthand Sudarshan16.45Database System Concepts -7
th
Edition
Join Operation:  Running Example
Running example: 
student ⨝takes
Catalog information for join examples:
n
student
= 5,000.
f
student
= 50, which implies that 
b
student
=5000/50 = 100.
n
takes
= 10000.
f
takes
= 25, which implies that 
b
takes
= 10000/25 = 400.
V(ID, takes)= 2500, which implies that on average, each student who 
has taken a course has taken 4 courses.
•Attribute IDin takes is a foreign key referencing student.
•V(ID, student)= 5000 (primary key!)

©Silberschatz, Korthand Sudarshan16.46Database System Concepts -7
th
Edition
Estimation of the Size of Joins
The Cartesian product rx s contains n
r
.n
s
tuples; each tuple 
occupies s
r
+ s
s
bytes.
If R ∩S= ∅, then r⋈sis the same as r  x s. 
If R ∩Sis a key for R, then a tuple of swill join with at most one tuple 
from r
•therefore, the number of tuples in r ⋈sis no greater than the 
number of tuples in s.
If R ∩S in S is a foreign key in Sreferencing R, then the number of 
tuples in r⋈sis exactly the same as the number of tuples in s.
The case for R ∩Sbeing a foreign key referencing Sis 
symmetric.
In the example query student ⋈takes, ID in takesis a foreign key 
referencing student
•hence, the result has exactly n
takes
tuples, which is 10000

©Silberschatz, Korthand Sudarshan16.47Database System Concepts -7
th
Edition
Estimation of the Size of Joins (Cont.)
If R ∩S= {A} is not a key for Ror S.
If we assume that every tuple t in R produces tuples in R    S,the 
number of tuples in R⨝Sis estimated to be:
If the reverse is true, the estimate obtained will be:
The lower of these two estimates is probably the more accurate one.
Can improve on above if histograms are available
•Use formula similar to above, for each cell of histograms on the 
two relations 

©Silberschatz, Korthand Sudarshan16.48Database System Concepts -7
th
Edition
Estimation of the Size of Joins (Cont.)
Compute the size estimates for depositor ⨝customerwithout using 
information about foreign keys:
•V(ID, takes) = 2500, and
V(ID, student) = 5000
•The two estimates are 5000 * 10000/2500 = 20,000 and 5000 * 
10000/5000 = 10000
•We choose the lower estimate, which in this case, is the same as 
our earlier computation using foreign keys.

©Silberschatz, Korthand Sudarshan16.49Database System Concepts -7
th
Edition
Size Estimation for Other Operations
Projection:  estimated size of ∏
A
(r)   =   V(A,r)
Aggregation : estimated size of 
G
훾훾
A
(r)   = V(G,r)
Set operations
•For unions/intersections of selections on the same relation: 
rewrite and use size estimate for selections
E.g. σ
θ1
(r) ∪σ
θ2 
(r)  can be rewritten as σ
θ1 or  θ2 
(r)
•For operations on different relations:
estimated size of r ∪s = size of r+ size of s.   
estimated size of r ∩s  = minimum size ofrand size of s.
estimated size of r–s = r.
All the three estimates may be quite inaccurate, but provide 
upper bounds on the sizes
.

©Silberschatz, Korthand Sudarshan16.50Database System Concepts -7
th
Edition
Size Estimation (Cont.)
Outer join:  
•Estimated size of r ⟕s  = size of  r ⨝s  + size of r
Case of right outer join is symmetric
•Estimated size of r ⟗s  = size of r ⨝s + size of r+ size of s

©Silberschatz, Korthand Sudarshan16.51Database System Concepts -7
th
Edition
Estimation of Number of Distinct Values
Selections: σ
θ
(r) 
If θforces Ato take a specified value: V(A,σ
θ
(r)) = 1.
e.g., A= 3
If θforces A to take on one of a specified set of values: 
V(A,σ
θ
(r)) = number of specified values.
(e.g., (A= 1 V A= 3 V A= 4 )), 
If the selection condition θis of the form A op r
estimated V(A,σ
θ
(r)) = V(A.r) * s
where sis the selectivity of the selection.
In all the other cases: use approximate estimate of
min(V(A,r), n
σθ(r) 
)
•More accurate estimate can be got using probability theory, but 
this one works fine generally

©Silberschatz, Korthand Sudarshan16.52Database System Concepts -7
th
Edition
Estimation of Distinct Values (Cont.)
Joins: r ⨝s
If all attributes in Aare from r
estimatedV(A, r ⨝s) = min (V(A,r), n 
r ⨝s
)
If Acontains attributes A1 from rand A2 from s, then estimated 
V(A,r⨝s) = 
min(V(A1,r)*V(A2 –A1,s), V(A1 –A2,r)*V(A2,s), n
r ⨝s
)
•More accurate estimate can be got using probability theory, but 
this one works fine generally

©Silberschatz, Korthand Sudarshan16.53Database System Concepts -7
th
Edition
Estimation of Distinct Values (Cont.)
Estimation of distinct values are straightforward for projections.
•They are the same in ∏
A (r)
as in r. 
The same holds for grouping attributes of aggregation.
For aggregated values 
•For min(A) and max(A), the number of distinct values can be 
estimated as min(V(A,r), V(G,r))  where G denotes grouping 
attributes
•For other aggregates, assume all values are distinct, and use 
V(G,r)

©Silberschatz, Korthand Sudarshan16.54Database System Concepts -7
th
Edition
ADDITIONAL OPTIMIZATION 
TECHNIQUES
Nested Subqueries
Materialized Views

©Silberschatz, Korthand Sudarshan16.55Database System Concepts -7
th
Edition
Optimizing Nested Subqueries**
Nested query example:
selectname
from instructor
where exists (select *
from teaches
where instructor.ID = teaches.ID andteaches.year= 2019)
SQLconceptually treats nested subqueries in the where clause as 
functions that take parameters and return a single value or set of 
values
•Parameters are variables from outer level query that are used in the 
nested subquery; such variables are called correlation variables
Conceptually, nested subquery is executed once for each tuple in the 
cross-product generated by the outer level fromclause
•Such evaluation is called correlated evaluation 
•Note: other conditions in where clause may be used to compute a join 
(instead of a cross-product) before executing the nested subquery

©Silberschatz, Korthand Sudarshan16.56Database System Concepts -7
th
Edition
Optimizing Nested Subqueries (Cont.)
Correlated evaluation may be quite inefficient since 
•a large number of calls may be made to the nested query 
•there may be unnecessary random I/O as a result
SQL optimizers attempt to transform nested subqueries to joins where 
possible, enabling use of efficient join techniques
E.g.: earlier nested query can be rewritten as 
∏
name
(instructor⨝
instructor.ID=teaches.ID ∧teaches.year=2019
teaches) 
Note: the two queries generate different numbers of duplicates (why?)
•Can be modified to handle duplicates correctly using semijoins

©Silberschatz, Korthand Sudarshan16.57Database System Concepts -7
th
Edition
Optimizing Nested Subqueries (Cont.)
The semijoinoperator ⋉is defined as follows
•A tuple r
i
appears n times in r ⋉
θ
sif it appears ntimes in r, and there is 
at least one matching tuple s
i
in s
E.g.: earlier nested query can be rewritten as 
∏
name
(instructor⋉
instructor.ID=teaches.ID ∧teaches.year=2019
teaches) 
•Or even as:  ∏
name
(instructor⋉
instructor.ID=teaches.ID 
(σ
teaches.year=2019
teaches))
•Now the duplicate count is correct!
The above relational algebra query is also equivalent to
from instructor
where ID in (select teaches.ID 
from teaches
where teaches.year= 2019)

©Silberschatz, Korthand Sudarshan16.58Database System Concepts -7
th
Edition
Optimizing Nested Subqueries (Cont.)
This could also be written using only joins (in SQL) as
with t
1
as
(select distinct ID
from teaches
where year = 2019)
select name
from instructor, t
1
where t
1
.ID = instructor.ID
The query
selectname 
from instructor
where not exists (select *
from teaches
where instructor.ID = teaches.ID andteaches.year= 2019)
can be rewritten using the anti-semijoinoperation as
�
⋉
∏
name
(instructor
�
⋉
instructor.ID=teaches.ID ∧teaches.year=2019
teaches) 

©Silberschatz, Korthand Sudarshan16.59Database System Concepts -7
th
Edition
Optimizing Nested Subqueries (Cont.)
In general, SQL queries of the form below can be rewritten as shown
Rewrite:  select A
fromr
1
, r
2
,..., r
n
whereP
1
and exists (select *
froms
1
, s
2
,..., s
m
whereP
2
1
andP
2
2
)
To: ∏
A
(σ
P1 
(r
1
xr
2
x ...xr
n
) ⋉
P
2
2
σ
P
2
1
(s
1
xs
2
x ...xs
m
)
•P
2
1  
contains predicates that do not involve any correlation variables
•P
2
2
contains predicates involving correlation variables
The process of replacing a nested query by a query with a join/semijoin
(possibly with a temporary relation) is called decorrelation.
Decorrelation is more complicated in several cases, e.g.
The nested subquery uses aggregation, or
The nested subquery is a scalar subquery
•Correlated evaluation used in these cases

©Silberschatz, Korthand Sudarshan16.60Database System Concepts -7
th
Edition
Decorrelation (Cont.)
Decorrelation of scalar aggregate subqueries can be done using 
groupby/aggregation in some cases
select name
from instructor
where 1 <(select count(*)
from teaches
where instructor.ID = teaches.ID 
and teaches.year= 2019)
∏
name
(instructor⋉
instructor.ID=TID ∧1 < cnt
( 
ID as TID 
훾훾
count(*) as cnt
(σ 
teaches.year=2019
(teaches)))) 

©Silberschatz, Korthand Sudarshan16.61Database System Concepts -7
th
Edition
Materialized Views
A materialized view is a view whose contents are computed and 
stored.
Consider the view
create view department_total_salary(dept_name, total_salary)as
select dept_name, sum(salary)
from instructor
group by dept_name
Materializing the above view would be very useful if the total salary by 
department is required frequently
•Saves the effort of finding multiple tuples and adding up their 
amounts

©Silberschatz, Korthand Sudarshan16.62Database System Concepts -7
th
Edition
Materialized View Maintenance
The task of keeping a materialized view up-to -date with the underlying 
data is known as materialized view maintenance
Materialized views can be maintained by recomputationon every 
update
A better option is to use incremental view maintenance
•Changes to database relations are used to compute changes to the 
materialized view, which is then updated
View maintenance can be done by
•Manually defining triggers on insert, delete, and update of each relation in 
the view definition
•Manually written code to update the view whenever database relations 
are updated
•Periodic recomputation(e.g. nightly)
•Incremental maintenance supported by many database systems
Avoids manual effort/correctness issues

©Silberschatz, Korthand Sudarshan16.63Database System Concepts -7
th
Edition
Incremental View Maintenance
The changes (inserts and deletes) to a relation or expressions are 
referred to as its differential
•Set of tuples inserted to and deleted from r are denoted i
r
and d
r
To simplify our description, we only consider inserts and deletes
•We replace updates to a tuple by deletion of the tuple followed by 
insertion of the update tuple 
We describe how to compute the change to the result of each 
relational operation, given changes to its inputs
We then outline how to handle relational algebra expressions 

©Silberschatz, Korthand Sudarshan16.64Database System Concepts -7
th
Edition
Join Operation
Consider the materialized view v= r ⨝s and an update to r
Let r
old
and r
new
denote the old and new states of relation r
Consider the case of an insert to r:  
•We can write r
new
⨝s as (r
old
∪i
r
) ⨝s
•And rewrite the above to  (r
old
⨝s) ∪(i
r
⨝s)
•But (r
old
⨝s) is simply the old value of the materialized view, so the 
incremental change to the view is just      i
r
⨝s
Thus, for inserts     v
new
= v
old
∪(i
r
⨝s)
Similarly for deletes    v
new
= v
old
–(d
r
⨝s)
A, 1
B, 2
1, p
2, r
2, s
A, 1, p
B, 2, r
B, 2, s
C,2
C, 2, r
C, 2, s

©Silberschatz, Korthand Sudarshan16.65Database System Concepts -7
th
Edition
Selection and Projection Operations
Selection: Consider a view v= σ
θ
(r).
•v
new
= v
old
∪σ
θ
(i
r
)
•v
new
= v
old
-σ
θ
(d
r
)
Projection is a more difficult operation 
•R = (A,B), and r(R) = { (a,2), (a,3)}
•∏
A
(r) has a single tuple (a). 
•If we delete the tuple (a,2) from r, we should not delete the tuple (a) from 
∏
A
(r), but if we then delete (a,3) as well, we should delete the tuple
For each tuple in a projection ∏
A
(r) , we will keep a count of how 
many times it was derived
•On insert of a tuple to r, if the resultant tuple is already in ∏
A
(r) we 
increment its count, else we add a new tuple with count = 1
•On delete of a tuple from r, we decrement the count of the corresponding 
tuple in ∏
A
(r) 
if the count becomes 0, we delete the tuple from ∏
A
(r)

©Silberschatz, Korthand Sudarshan16.66Database System Concepts -7
th
Edition
Aggregation Operations
Count: v= 
A
훾훾
count(B)
(r)
. 
•When a set of tuples i
r
is inserted 
For each tuple r in i
r
, if the corresponding group is already present in v, 
we increment its count, else we add a new tuple with count = 1
•When a set of tuples d
r
is deleted
for each tuple t in i
r.
welook for the group t.Ain v, and subtract 1 from 
the count for the group. 
•  If the count becomes 0, we delete from vthe tuple for the group t.A
Sum: v= 
A
훾훾
sum (B)
(r) 
•We maintain the sum in a manner similar to count, except we 
add/subtract the B value instead of adding/subtracting 1 for the count
•Additionally we maintain the count in order to detect groups with no 
tuples.  Such groups are deleted from v
Cannot simply test for sum = 0 (why?)
Avg: How to handle average?
•Maintain sum and count separately, and divide at the end

©Silberschatz, Korthand Sudarshan16.67Database System Concepts -7
th
Edition
Aggregate Operations (Cont.)
min, max: v= 
A
훾훾
min (B) 
(r).  
•Handling insertions on r is straightforward.
•Maintaining the aggregate values min and max on deletions may 
be more expensive.We have to look at the other tuples of rthat 
are in the same group to find the new minimum

©Silberschatz, Korthand Sudarshan16.68Database System Concepts -7
th
Edition
Other Operations
Set intersection: v= r ∩s
•when a tuple is inserted in rwe check if it is present in s, and if so 
we add it to v. 
•If the tuple is deleted from r, we delete it from the intersection if it 
is present. 
•Updates to sare symmetric
•The other set operations, union and set difference are handled in 
a similar fashion.
Outer joins are handled in much the same way as joins but with some 
extra work 
•we leave details to you.

©Silberschatz, Korthand Sudarshan16.69Database System Concepts -7
th
Edition
Handling Expressions
To handle an entire expression, we derive expressions for computing 
the incremental change to the result of each sub-expressions, starting 
from the smallest sub-expressions.
E.g. consider  E
1
⨝E
2
where each of E
1
and E
2
may be a complex 
expression
•Suppose the set of tuples to be inserted into E
1 
is given by D
1 
Computed earlier, since smaller sub-expressions are handled 
first
•Then  the set of tuples to be inserted into E
1
⨝E
2
is given by
D
1 
⨝E
2
This is just the usual way of maintaining joins

©Silberschatz, Korthand Sudarshan16.70Database System Concepts -7
th
Edition
Query Optimization and Materialized Views
Rewriting queries to use materialized views:
•A materialized view v = r ⨝s is available 
•A user submits a query    r ⨝s ⨝t
•We can rewrite the query as v ⨝t
Whether to do so depends on cost estimates for the two alternative
Replacing a use of a materialized view by the view definition:
•A materialized view v = r ⨝s is available, but without any index on it
•User submits a query σ
A=10
(v). 
•Suppose also that shas an index on the common attribute B, and r has 
an index on attribute A. 
•The best plan for this query may be to replacevby r ⨝s, which can lead 
to the query plan σ
A=10
(r)⨝s
Query optimizer should be extended to consider all above 
alternatives and  choose the best overall plan 

©Silberschatz, Korthand Sudarshan16.71Database System Concepts -7
th
Edition
Materialized View Selection
Materialized view selection: “What is the best set of views to 
materialize?”
Index selection:“what is the best set of indices to create”
•closely related, to materialized view selection
but simpler
Materialized view selection and index selection based on typical 
system workload(queries and updates)
•Typical goal: minimize time to execute workload , subject to 
constraints on space and time taken for some critical 
queries/updates
•One of the steps in database tuning 
more on tuning in later chapters
Commercial database systems provide tools (called “tuning 
assistants”or “wizards”) to help the database administrator choose 
what indices and materialized views to create 

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Additional Optimization Techniques

©Silberschatz, Korthand Sudarshan16.73Database System Concepts -7
th
Edition
Top-K Queries
Top-K queries
select* 
fromr, s
wherer.B= s.B
order byr.Aascending
limit10
•Alternative 1: Indexed nested loops join with r as outer
•Alternative 2: estimate highest r.Avalue in result and add 
selection (and r.A<= H) to where clause  
If < 10 results, retry with larger H

©Silberschatz, Korthand Sudarshan16.74Database System Concepts -7
th
Edition
Optimization of Updates
Halloween problem
updateR setA = 5 * A 
whereA > 10
•If index on A is used to find tuples satisfying A > 10, and   tuples 
updated immediately, same tuple may be found         (and 
updated) multiple times
•Solution 1: Always defer updates
collect the updates (old and new values of tuples) and update 
relation and indices in second pass
Drawback: extra overhead even if e.g. update is only on R.B, 
not on attributes in selection condition
•Solution 2: Defer only if required
Perform immediate update if update does not affect attributes 
in where clause, and deferred updates otherwise.

©Silberschatz, Korthand Sudarshan16.75Database System Concepts -7
th
Edition
Join Minimization
Join minimization
selectr.A, r.B
fromr, s
wherer.B= s.B
Check if join with s is redundant, drop it 
•E.g. join condition is on foreign key from r to s, r.Bis declared as 
not null, and no selection on s
•Other sufficient conditions possible
selectr.A, s2.B 
fromr, s ass1, s ass2
wherer.B=s1.B andr.B= s2.B ands1.A < 20 ands2.A < 10
join with s1 is redundant and can be dropped (along with 
selection on s1)
•Lots of research in this area since 70s/80s!

©Silberschatz, Korthand Sudarshan16.76Database System Concepts -7
th
Edition
Multiquery Optimization
Example
Q1: select* from(r natural joint) natural joins
Q2: select* from(r natural joinu) natural joins
•Both queries share common subexpression (r natural join s)
•May be useful to compute (r natural join s) once and use it in both 
queries
But this may be more expensive in some situations
•  e.g. (r natural join s) may be expensive, plans as shown in 
queries may be cheaper
Multiqueryoptimization: find best overall plan for a set of queries, 
expoitingsharing of common subexpressions between queries where 
it is useful

©Silberschatz, Korthand Sudarshan16.77Database System Concepts -7
th
Edition
Multiquery Optimization (Cont.)
Simple heuristic used in some database systems:
•optimize each query separately
•detect and exploiting common subexpressions in the individual 
optimal query plans
May not always give best plan, but is cheap to implement
•Shared scans: widely used special case of multiquery
optimization
Set of materialized views may share common subexpressions
•As a result, view maintenance plans may share subexpressions
•Multiqueryoptimization can be useful in such situations

©Silberschatz, Korthand Sudarshan16.78Database System Concepts -7
th
Edition
Parametric Query Optimization
Example 
select* 
fromr natural joins
wherer.a< $1
•value of parameter $1 not known at compile time
known only at run time
•different plans may be optimal for different values of $1
Solution 1: optimize at run time, each time query is submitted
can be expensive 
Solution 2: Parametric Query Optimization:
•optimizer generates a set of plans, optimal for different values of $1
Set of optimal plans usually small for 1 to 3 parameters
Key issue: how to do find set of optimal plans efficiently
•best one from this set is chosen at run time when $1 is known
Solution 3: Query Plan Caching
•If optimizer decides that same plan is likely to be optimal for all parameter values, it 
caches plan and reuses it, else reoptimize each time
•Implemented in many database systems

©Silberschatz, Korthand Sudarshan16.79Database System Concepts -7
th
Edition
Plan Stability Across Optimizer Changes
What if 95% of plans are faster on database/optimizer version N+1 
than on N, but 5% are slower?
•Why should plans be slower on new improved optimizer?  
Answer: Two wrongs can make a right, fixing one wrong can make 
things worse!
Approaches:
•Allow hints for tuning queries
Not practical for migrating large systems with no access to source 
code
•Set optimization level, default to version N (Oracle)
And migrate one query at a time after testing both plans on new 
optimizer
•Save plan from version N, and give it to optimizer version N+1
Sybase, XML representation of plans (SQL Server)

©Silberschatz, Korthand Sudarshan16.80Database System Concepts -7
th
Edition
Adaptive Query Processing
Some systems support adaptive operators that change execution 
algorithm on the fly
•E.g. (indexed) nested loops join or hash join chosen at run time, 
depending on size of outer input
Other systems allow monitoring of behaviorof plan at run time and 
adapt plan
•E.g. if statistics such as number of rows is found to be very different in 
reality from what optimizer estimated
•Can stop execution, compute fresh plan, and restart
But must avoid too many such restarts

Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
End of Chapter

©Silberschatz, Korthand Sudarshan16.82Database System Concepts -7
th
Edition
Figure 13.01

©Silberschatz, Korthand Sudarshan16.83Database System Concepts -7
th
Edition
Figure 13.02

©Silberschatz, Korthand Sudarshan16.84Database System Concepts -7
th
Edition
Figure 13.03

©Silberschatz, Korthand Sudarshan16.85Database System Concepts -7
th
Edition
Figure 13.04

©Silberschatz, Korthand Sudarshan16.86Database System Concepts -7
th
Edition
Figure 13.06

©Silberschatz, Korthand Sudarshan16.87Database System Concepts -7
th
Edition
Figure 13.08
