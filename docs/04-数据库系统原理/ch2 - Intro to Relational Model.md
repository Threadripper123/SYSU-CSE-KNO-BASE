# ch2 - Intro to Relational Model

> 主题库：数据库系统原理　｜　源文件：ch2 - Intro to Relational Model.pdf　｜　12,019 字符　｜　30 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 2: Intro to Relational Model

©Silberschatz, Korth and Sudarshan2.2Database System Concepts -7
th
Edition
Outline
▪Structure of Relational Databases
▪Database Schema
▪Keys
▪Schema Diagrams
▪Relational Query Languages
▪The Relational Algebra

©Silberschatz, Korth and Sudarshan2.3Database System Concepts -7
th
Edition
Example of a InstructorRelation
attributes
(or columns)
tuples
(or rows)

©Silberschatz, Korth and Sudarshan2.4Database System Concepts -7
th
Edition
Relation Schema and Instance
▪A
1
, A
2
, ..., A
n
are attributes
▪R= (A
1
, A
2
, ..., A
n
) is a relation schema
Example:
instructor = (ID,  name, dept_name, salary)
▪A relation instance rdefined over schema Ris denoted  by r (R).
▪The current values a relation are specified by a table
▪An element tofrelationris called a  tupleand is represented by 
a row in a table

©Silberschatz, Korth and Sudarshan2.5Database System Concepts -7
th
Edition
Attributes
▪The set of allowed values for each attribute is called the domainof the 
attribute
▪Attribute values are (normally) required to be atomic; that is, indivisible
▪The special valuenullis a member of every domain. Indicated that the 
value is “unknown”
▪The null value causes complications in the definition of many operations

©Silberschatz, Korth and Sudarshan2.6Database System Concepts -7
th
Edition
Relations are Unordered
▪Order of tuples is irrelevant (tuples may be stored in an arbitrary order)
▪Example: instructorrelation with unordered tuples

©Silberschatz, Korth and Sudarshan2.7Database System Concepts -7
th
Edition
Database Schema
▪Database schema --is the logical structure of the database.
▪Database instance --is a snapshot of the data in the database at a given 
instant in time. 
▪Example:
•schema:   instructor(ID, name, dept_name, salary)
•Instance:

©Silberschatz, Korth and Sudarshan2.8Database System Concepts -7
th
Edition
Keys
▪Let K R
▪K is a superkeyof Rif values for Kare sufficient to identify a unique tuple 
of each possible relation r(R)
•Example:  {ID} and {ID,name} are both superkeysof instructor.
▪SuperkeyKis a candidate keyif Kis minimal
Example:  {ID} is a candidate key for Instructor
▪One of the candidate keys is selected to be the primary key.
•Which one?
▪Foreign keyconstraint: Value in one relation must appear in another
•Referencingrelation
•Referencedrelation
•Example: dept_namein instructoris a foreign key from instructor
referencing department

©Silberschatz, Korth and Sudarshan2.9Database System Concepts -7
th
Edition
Schema Diagram for University Database

©Silberschatz, Korth and Sudarshan2.10Database System Concepts -7
th
Edition
Relational Query Languages
▪Procedural versus non-procedural, or declarative
▪“Pure” languages:
•Relational algebra
•Tuple relational calculus
•Domain relational calculus
▪The above 3 pure languages are equivalent in computing power
▪We will concentrate in this chapter on relational algebra
•Not Turing-machine equivalent
•Consists of 6 basic operations

©Silberschatz, Korth and Sudarshan2.11Database System Concepts -7
th
Edition
Relational Algebra
▪A  procedural language consisting  of a set of operations that take one or 
two relations as input and produce a new relation as their result. 
▪Six basic operators
•select: 
•project: 
•union: 
•set difference: –
•Cartesian product: x
•rename: 

©Silberschatz, Korth and Sudarshan2.12Database System Concepts -7
th
Edition
Select Operation
▪The  select operation selects tuples that satisfy a given predicate.
▪Notation:  
p 
(r)
▪pis called the selection predicate
▪Example: select those tuples of the instructorrelation where the 
instructor is in the “Physics” department.
•Query

dept_name=“Physics” 
(instructor)
•Result

©Silberschatz, Korth and Sudarshan2.13Database System Concepts -7
th
Edition
Select Operation (Cont.)
▪We allow comparisons using 
=, , >, . <. 
in the selection predicate. 
▪We can combine several predicates into a larger predicate by using the 
connectives:
(and), (or), (not)
▪Example: Find the instructors in Physics with a salary greater $90,000, we 
write:

dept_name=“Physics”

salary >90,000
(instructor)
▪The select predicate may  include comparisons between two attributes. 
•Example, find all departments whose name is the same as their 
building name:
•
dept_name=building
(department)

©Silberschatz, Korth and Sudarshan2.14Database System Concepts -7
th
Edition
Project Operation
▪A unary operation that returns its argument relation, with certain attributes 
left out.  
▪Notation:

A
1
,A
2
,A
3
....A
k
(r)
where A
1
, A
2
,  ..., A
k
are attribute names and ris a relation name.
▪The result is defined as the relation of kcolumns obtained by erasing the 
columns that are not listed
▪Duplicate rows removed from result, since relations are sets

©Silberschatz, Korth and Sudarshan2.15Database System Concepts -7
th
Edition
Project Operation Example
▪Example: eliminate the dept_nameattribute of instructor
▪Query:

ID, name, salary
(instructor) 
▪Result:

©Silberschatz, Korth and Sudarshan2.16Database System Concepts -7
th
Edition
Composition of Relational Operations
▪The result of a relational-algebra operation is relation  and therefore of 
relational-algebra operations can be composed together into a 
relational-algebra expression.
▪Consider  the query --Find the names of all instructors in the Physics 
department.

name
(
dept_name=“Physics”
(instructor))
▪Instead of giving the name of a relation as the argument of the projection 
operation, we give an expression that evaluates to a relation.

©Silberschatz, Korth and Sudarshan2.17Database System Concepts -7
th
Edition
Cartesian-Product Operation
▪The Cartesian-product operation (denoted by X)  allows us to combine 
information from any two relations.  
▪Example: the Cartesian product of the relations instructorand teachesis 
written  as:
instructorX  teaches
▪We construct a tuple of the result out of each possible pair of tuples: one 
from the instructorrelation and one from the teachesrelation (see next 
slide)
▪Since the instructorID appears in both relations we distinguish between 
these attribute by attaching to the attribute the name of the relation from 
which the attribute originally came.
•instructor.ID
•teaches.ID

©Silberschatz, Korth and Sudarshan2.18Database System Concepts -7
th
Edition
TheinstructorXteaches  table

©Silberschatz, Korth and Sudarshan2.19Database System Concepts -7
th
Edition
Join Operation
▪The Cartesian-Product 
instructorX  teaches
associates every  tuple of  instructor with every tuple of teaches.
•Most of the resulting rows have information about instructors who did 
NOT teach a particular course. 
▪To get only those tuples of  “instructorX  teaches“ that pertain to 
instructors and the courses that they taught, we write:

instructor.id =  teaches.id
(instructor  xteaches ))
•We get only those tuples of “instructorX  teaches” that pertain to 
instructors and the courses that they taught.
▪The result of this expression, shown in the next slide

©Silberschatz, Korth and Sudarshan2.20Database System Concepts -7
th
Edition
Join Operation (Cont.)
▪Thetable corresponding to:

instructor.id =  teaches.id
(instructor  xteaches))

©Silberschatz, Korth and Sudarshan2.21Database System Concepts -7
th
Edition
Join Operation (Cont.)
▪The join operation allows us to combine  a select operation and a  
Cartesian-Product  operation into a single operation.
▪Consider relations r (R) and s (S)
▪Let  “theta” be a predicate on attributes in the schema R “union” S. The 
join operation  r ⋈
휃
s is defined as follows:
푟⋈
휃
푠=휎
휃
(푟×푠)
▪Thus

instructor.id =  teaches.id
(instructor  xteaches ))
▪Can equivalently be written as 
instructor⋈
Instructor.id = teaches.id
teaches.

©Silberschatz, Korth and Sudarshan2.22Database System Concepts -7
th
Edition
Union Operation
▪The union operation allows us to combine two relations 
▪Notation:  r s
▪For rsto be valid.
1.   r,smust have the same arity(same number of attributes)
2.   The attribute domains must be compatible(example: 2
nd
column of rdeals with the same type of values as does the 
2
nd 
column of s)
▪Example: to find all courses taught in the Fall 2017 semester, or in the 
Spring 2018 semester, or in both

course_id
(
semester=“Fall”Λyear=2017 
(section))  

course_id
(
semester=“Spring”Λyear=2018 
(section))

©Silberschatz, Korth and Sudarshan2.23Database System Concepts -7
th
Edition
Union Operation (Cont.)
▪Result of: 

course_id
(
semester=“Fall”Λyear=2017 
(section))  

course_id
(
semester=“Spring”Λyear=2018 
(section))

©Silberschatz, Korth and Sudarshan2.24Database System Concepts -7
th
Edition
Set-Intersection Operation
▪The  set-intersection  operation allows us to find tuples that are in both 
the input relations.
▪Notation: rs
▪Assume: 
•r, shave the same arity
•attributes of rand sare compatible
▪Example: Find the set of all courses taught in both the Fall 2017 and the 
Spring 2018 semesters.

course_id
(
semester=“Fall”Λyear=2017 
(section)) 

course_id
(
semester=“Spring”Λyear=2018 
(section))
•Result

©Silberschatz, Korth and Sudarshan2.25Database System Concepts -7
th
Edition
Set Difference Operation
▪The set-difference operation allows us to find tuples that are in one relation 
but are not in another. 
▪Notation r –s
▪Set differences must be taken between compatiblerelations.
•rand smust have the samearity
•attribute domains of r and s must be compatible
▪Example: to find all courses taught in the Fall 2017 semester, but not in the 
Spring 2018 semester

course_id
(
semester=“Fall”Λyear=2017 
(section))  −  

course_id
(
semester=“Spring”Λyear=2018 
(section))

©Silberschatz, Korth and Sudarshan2.26Database System Concepts -7
th
Edition
The Assignment  Operation 
▪It is convenient at times to write a relational-algebra expression by 
assigning parts of it to temporary relation variables.  
▪The assignment  operation is  denoted by and works like assignment in 
a programming language.
▪Example: Find all instructor in the “Physics” and Music department.
Physics
dept_name=“Physics”
(instructor)
Music
dept_name=“Music” 
(instructor)
PhysicsMusic
▪With the assignment operation, a query can be written as a sequential 
program consisting of a series of assignments followed by an expression 
whose value is displayed as the result of the query. 

©Silberschatz, Korth and Sudarshan2.27Database System Concepts -7
th
Edition
The Rename Operation 
▪The results of relational-algebra expressions do not have a name that we 
can use to refer to them.  The  rename operator,  ,is provided for that 
purpose
▪The expression:

x
(E)
returns the result of expression Eunder the name x
▪Another form of the rename operation:

x(A1,A2, .. An) 
(E)

©Silberschatz, Korth and Sudarshan2.28Database System Concepts -7
th
Edition
Equivalent Queries
▪There is more than one way to write a query in relational algebra. 
▪Example:  Find information about courses taught by instructors in the 
Physics department with salary greater than 90,000
▪Query 1

dept_name=“Physics”

salary > 90,000
(instructor)
▪Query 2

dept_name=“Physics”
(
salary > 90.000
(instructor))
▪The two queries are not identical; they are, however, equivalent --they 
give the same result on any database.

©Silberschatz, Korth and Sudarshan2.29Database System Concepts -7
th
Edition
Equivalent Queries
▪There is more than one way to write a query in relational algebra. 
▪Example:  Find information about courses taught by instructors in the 
Physics department
▪Query 1

dept_name=“Physics”
(instructor⋈
instructor.ID = teaches.ID
teaches)
▪Query 2
(
dept_name=“Physics”
(instructor))⋈
instructor.ID = teaches.ID
teaches
▪The two queries are not identical; they are, however, equivalent --they 
give the same result on any database.

©Silberschatz, Korth and Sudarshan2.30Database System Concepts -7
th
Edition
End of Chapter 2
