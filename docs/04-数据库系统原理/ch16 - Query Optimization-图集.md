# ch16 - Query Optimization-图集

> 主题库：数据库系统原理　｜　源文件：ch16 - Query Optimization-图集.pdf　｜　1,788 字符　｜　41 个片段　｜　更新于 2026-11-11
Chapter 1e:a QueryOptmzation

Ilname, title instruCtor O dept_name=Music teaches Tcourse_id titl 1n structor teaches course_id,ttl COurse COurse (a) Initial expression tree (b) Transformed expresslon tree

(sort to remove duplicates SORTID sortD (hash join) 0 dept_name = Music course_id, title useindex instructor teaches course

Viewing    Query Evaluation  Plans

Generating Equlvalent Expressions

EqulvalenceRules

Pictorial Depiction of Egulvalence Rules

Rule5 E1 E2 E2 E1 Rale a E3 EI E 2 E 2 E 3 00 RuleTa attributes from E, 0θ E2 E E2 E

ExerCi

Transformation Example: Pushing Selections

M ultiple Transformations (Cont)

ilname, title Llname,title ayear=201 dept_name = Music year = 2017 teaches Icourse_id, title instructor teaches CourSe Course (a) Initial expression tree (b) Tree after multiple transformations

JolnOrdering Example Cont

Enumeration of Equivalent Expressions

Implementing Transformation Based Optimization

Cost Estimation

Cost

Based Optimizaton

Dynamic Programming In Optimization

Join Order C Optimization Agorithm

Joln C Order Optimization Algorithm  (cont)

Left Deep Joln    Trees

5 r3 r4 r r1 r2 rl r2 (a) Left-deep join tree (b) Non-left-deep join tree

Interesting SortOrders

Structure of Query Optimizers (Cont)

Statistics for Cost  Estimation .

Statistical Information for Cost Estimation 1

50 1-5 ; 6-10 11-15 16-20 21-25 ValUe

Histograms (cont-)

Slze Estimation of Complex Solections

S1*S2 ,*...*S

Joln Operation: Running Example

Estimation of the Size of Joins (Cont)

Size Estimation for Other Operations

Size stimation (Cont

Estimation of Number of Distinct Values

Estimation  of Distinct Values R  (Cont-)  「

ADDITIONALOPTIMIZATION

TECHNIQUES

Optimizing「 NestedS   Subqueries    (Cont）

MaterializedView Maintenance
