# ch3 - Introduction to SQL

> 主题库：数据库系统原理　｜　源文件：ch3 - Introduction to SQL.pdf　｜　28,171 字符　｜　62 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 3: Introduction to SQL

©Silberschatz, Korth and Sudarshan3.2Database System Concepts -7
th
Edition
Outline
Overview of The SQL Query Language
SQL Data Definition
Basic Query Structure of SQL Queries
Additional Basic Operations
Set Operations
Null Values
Aggregate Functions
Nested Subqueries
Modification of the Database

©Silberschatz, Korth and Sudarshan3.3Database System Concepts -7
th
Edition
History
IBM Sequel language developed as part of System R project at the IBM 
San Jose Research Laboratory
Renamed Structured Query Language (SQL)
ANSI and ISO standard SQL:
•SQL-86
•SQL-89
•SQL-92 
•SQL:1999 (language name became Y2K compliant!)
•SQL:2003
Commercial systems offer most, if not all, SQL-92 features, plus varying 
feature sets from later standards and special proprietary features.  
•Not all examples here may work on your particular system.

©Silberschatz, Korth and Sudarshan3.4Database System Concepts -7
th
Edition
SQL Parts
DML -- provides the ability to query information from the database and to 
insert tuples into, delete tuples from, and modify tuples in the database.
integrity – the  DDL includes commands for specifying integrity 
constraints.
View definition -- The DDL  includes commands for defining views.
Transaction control –includes commands for specifying the beginning and 
ending of transactions.
Embedded  SQL  and dynamic SQL --define how SQL statements can 
be embedded within general-purpose programming languages.
Authorization – includes commands for specifying access rights to 
relations and views.

©Silberschatz, Korth and Sudarshan3.5Database System Concepts -7
th
Edition
Data Definition Language
The schema for each relation.
The type of values associated with each attribute.
The Integrity constraints
The set of indices to be maintained for each relation.
Security and authorization information for each relation.
The physical storage structure of each relation on disk.
The SQL data-definition language (DDL) allows the specification of 
information about relations, including:

©Silberschatz, Korth and Sudarshan3.6Database System Concepts -7
th
Edition
Domain Types in SQL
char(n).Fixed length character string, with user-specified length n.
varchar(n). Variable length character strings, with user-specified 
maximum length n.
int.Integer (a finite subset of the integers that is machine-dependent).
smallint.Small integer (a machine-dependent subset of the integer 
domain type).
numeric(p,d).Fixed point number, with user-specified precision of p
digits, with ddigits to the right of decimal point.  (ex., numeric(3,1), allows 
44.5 to be stores exactly, but not 444.5 or 0.32)
real, double precision.Floating point and double-precision floating point 
numbers, with machine-dependent precision.
float(n).Floating point number, with user-specified precision of at least n
digits.
More are covered in Chapter 4.

©Silberschatz, Korth and Sudarshan3.7Database System Concepts -7
th
Edition
Create Table Construct
An SQL relation is defined using thecreate table command:
create table r 
(A
1
D
1
, A
2
D
2
, ..., A
n
D
n
,
(integrity-constraint
1
),
...,
(integrity-constraint
k
))
•ris the name of the relation
•each A
i
is an attribute name in the schema of relation r
•D
i
is the data type of values in the domain of attribute A
i
Example:
create tableinstructor(
IDchar(5),
name           varchar(20),
dept_name  varchar(20),
salarynumeric(8,2))

©Silberschatz, Korth and Sudarshan3.8Database System Concepts -7
th
Edition
Integrity Constraints in Create Table
Types of integrity constraints
•primary key(A
1
, ..., A
n 
)
•foreign key (A
m
, ..., A
n 
) references r
•not null
SQL prevents any update to the database that violates an integrity 
constraint.
Example:
create tableinstructor(
IDchar(5),
name           varchar(20) not null,
dept_name  varchar(20),
salarynumeric(8,2),
primary key (ID),
foreign key (dept_name) references department);

©Silberschatz, Korth and Sudarshan3.9Database System Concepts -7
th
Edition
And a Few More Relation Definitions
create tablestudent(
IDvarchar(5),
namevarchar(20) not null,
dept_namevarchar(20),
tot_crednumeric(3,0),
primary key (ID),
foreign key (dept_name) references department);
create tabletakes(
IDvarchar(5),
course_idvarchar(8),
sec_idvarchar(8),
semestervarchar(6),
yearnumeric(4,0),
gradevarchar(2), 
primary key (ID, course_id, sec_id, semester, year),
foreign key (ID) references student,
foreign key (course_id, sec_id, semester, year) references section);

©Silberschatz, Korth and Sudarshan3.10Database System Concepts -7
th
Edition
And more still
create tablecourse(
course_idvarchar(8),
titlevarchar(50),
dept_namevarchar(20),
creditsnumeric(2,0),
primary key (course_id),
foreign key (dept_name) references department);

©Silberschatz, Korth and Sudarshan3.11Database System Concepts -7
th
Edition
Updates to tables
Insert 
•insert into instructor values ('10211', 'Smith', 'Biology', 66000);
Delete
•Remove all tuples from the studentrelation
delete from student  
Drop Table
•drop table r
Alter
•alter table r add A D
where Ais the name of the attribute to be added to relation r and 
Dis the domain of A.
All exiting tuples in the relation are assigned nullas the value for 
the new attribute.  
•alter table rdropA     
where Ais the name of an attribute of relationr
Dropping of attributes not supported by many databases.

©Silberschatz, Korth and Sudarshan3.12Database System Concepts -7
th
Edition
Basic Query Structure 
A typical SQL query has the form:
select A
1
, A
2
, ..., A
n
fromr
1
, r
2
, ..., r
m
where P
•A
i 
represents an attribute
•R
i
represents a relation
•Pis a predicate.
The result of an SQL query is a relation.

©Silberschatz, Korth and Sudarshan3.13Database System Concepts -7
th
Edition
The select Clause
The selectclause lists the attributes desired in the result of a query
•corresponds to the projection operation of the relational algebra
Example: find the names of all instructors:
select name
from instructor
NOTE:  SQL names are case insensitive (i.e., you may use upper- or 
lower-case letters.)  
•E.g.,  Name≡ NAME≡ name
•Some people use upper case wherever we use bold font.

©Silberschatz, Korth and Sudarshan3.14Database System Concepts -7
th
Edition
The select Clause (Cont.)
SQL allows duplicates in relations as well as in query results.
To force the elimination of duplicates, insert the keyword distinctafter 
select.
Find the department names of all instructors, and remove duplicates
select distinct dept_name
from instructor
The keyword all specifies that duplicates should not be removed.
select alldept_name
from instructor

©Silberschatz, Korth and Sudarshan3.15Database System Concepts -7
th
Edition
The select Clause (Cont.)
An asterisk in the select clause denotes “all attributes”
select *
from instructor
An attribute can be a literal  with  no from  clause
select  '437'
•Results is a table with one column and a single row with value “437”
•Can give the column a name using:
select '437' as FOO
An attribute can be a literal with from  clause
select  'A'
from instructor
•Result is a table with one column and Nrows (number of tuples in 
the instructorstable), each row with value “A”

©Silberschatz, Korth and Sudarshan3.16Database System Concepts -7
th
Edition
The select Clause (Cont.)
The selectclause can contain arithmetic expressions involving the 
operation, +, –, ∗, and /, and operating on constants or attributes of 
tuples.
•The query: 
selectID, name, salary/12
from instructor
would return a relation that is the same as the instructor relation, 
except that the value of the attribute salary is divided by 12.
•Can rename “salary/12” using the as clause:
select ID, name, salary/12  as monthly_salary

©Silberschatz, Korth and Sudarshan3.17Database System Concepts -7
th
Edition
The where Clause
The whereclause specifies conditions that the result must satisfy
•Corresponds to the selection predicate of the relational algebra.  
To find all instructors in Comp. Sci. dept
select name
from instructor
where dept_name ='Comp. Sci.'
SQL allows the use of the logical connectives and, or, and not 
The operands of the logical connectives can be expressions involving the 
comparison operators <, <=, >, >=, =, and <>.
Comparisons can be applied to results of arithmetic expressions
To find all instructors in Comp. Sci. dept with salary > 80000
select name
from instructor
where dept_name ='Comp. Sci.'and salary > 80000

©Silberschatz, Korth and Sudarshan3.18Database System Concepts -7
th
Edition
The from Clause
The fromclause lists the relations involved in the query
•Corresponds to the Cartesian product operation of the relational 
algebra.
Find the Cartesian product instructor X teaches
select ∗
from instructor, teaches
•generates every possible instructor – teaches pair, with all attributes 
from both relations.
•For common attributes (e.g., ID), the attributes  in the resulting table 
are renamed using the  relation name (e.g., instructor.ID)
Cartesian product not very useful directly, but useful combined with 
where-clause condition (selection operation in relational algebra).

©Silberschatz, Korth and Sudarshan3.19Database System Concepts -7
th
Edition
Examples
Find the names of all instructors who have taught some course and the 
course_id
•select name, course_id
from instructor , teaches
where instructor.ID = teaches.ID 
Find the names of all instructors in the Art  department who have taught 
some course and the course_id
•select name, course_id
from instructor , teaches
where instructor.ID = teaches.ID  andinstructor. dept_name = 'Art'

©Silberschatz, Korth and Sudarshan3.20Database System Concepts -7
th
Edition
The Rename Operation
The SQL allows renaming relations and attributes using the as clause:
old-name asnew-name
Find the names of all instructors who have a higher salary than 
some instructor in 'Comp. Sci'.
•select distinct T.name
from instructor as T, instructor as S
where T.salary>   S.salaryand S.dept_name= 'Comp. Sci.’
Keyword asis optional and may be omitted
instructor as T ≡ instructorT

©Silberschatz, Korth and Sudarshan3.21Database System Concepts -7
th
Edition
Self Join Example
Relation emp-super
Find the supervisor of “Bob”
Find the supervisor of the supervisor of “Bob”
Can you find  ALL the supervisors (direct and indirect) of “Bob”?

©Silberschatz, Korth and Sudarshan3.22Database System Concepts -7
th
Edition
String Operations
SQL includes a string-matching operator for comparisons on character 
strings.  The operator likeuses patterns that are described using two 
special characters:
•percent ( % ).  The % character matches any substring.
•underscore ( _ ).  The _ character matches any character.
Find the names of all instructors whose name includes the substring “dar”.
select name
from instructor
wherename like '%dar%' 
Match the string “100%”
like '100 \%' escape  '\' 
in that above we use backslash (\) as the escape character.

©Silberschatz, Korth and Sudarshan3.23Database System Concepts -7
th
Edition
String Operations (Cont.)
Patterns are case sensitive. 
Pattern matching examples:
•'Intro%' matches any string beginning with “Intro”.
•'%Comp%' matches any string containing “Comp” as a substring.
•'_ _ _' matches any string of exactly three characters.
•'_ _ _ %' matches any string of at least three characters.
SQL supports a variety of string operations such as
•concatenation (using “||”)
•converting from upper to lower case (and vice versa)
•finding string length, extracting substrings, etc.

©Silberschatz, Korth and Sudarshan3.24Database System Concepts -7
th
Edition
Ordering the Display of Tuples
List in alphabetic order the names of all instructors 
select distinct name
from    instructor
order by name
We may specify descfor descending order or ascfor ascending order, 
for each attribute; ascending order is the default.
•Example:  order bynamedesc
Can sort on multiple attributes
•Example: order by dept_name, name

©Silberschatz, Korth and Sudarshan3.25Database System Concepts -7
th
Edition
Where Clause Predicates
SQL includes a betweencomparison operator
Example:  Find the names of all instructors with salary between $90,000 
and $100,000 (that is, ≥$90,000 and ≤$100,000)
•selectname
from instructor
where salary between 90000 and 100000
Tuple comparison
•select name, course_id
from instructor, teaches
where (instructor.ID, dept_name) = (teaches.ID, 'Biology');

©Silberschatz, Korth and Sudarshan3.26Database System Concepts -7
th
Edition
Set Operations
Find courses that ran in Fall 2017 or in Spring 2018
(selectcourse_idfrom section where sem= 'Fall' and year = 2017)
union
(selectcourse_idfrom section where sem= 'Spring' and year = 2018)
Find courses that ran in Fall 2017 and in Spring 2018
(selectcourse_idfrom section where sem= 'Fall' and year = 2017)
intersect
(selectcourse_idfrom section where sem= 'Spring' and year = 2018)
Find courses that ran in Fall 2017 but not in Spring 2018
(selectcourse_idfrom section where sem= 'Fall' and year = 2017)
except
(selectcourse_idfrom section where sem= 'Spring' and year = 2018)

©Silberschatz, Korth and Sudarshan3.27Database System Concepts -7
th
Edition
Set Operations (Cont.)
Set operations union, intersect, and except 
•Each of the above operations automatically eliminates duplicates
To retain all duplicates use the
•union all,
•intersect all
•except all.

©Silberschatz, Korth and Sudarshan3.28Database System Concepts -7
th
Edition
Null Values
It is possible for tuples to have a null value, denoted by null, for some of 
their attributes
nullsignifies an unknown value or that a value does not exist.
The result of any arithmetic expression involving nullis null
•Example:  5 + nullreturns null
The predicate  is nullcan be used to check for null values.
•Example: Find all instructors whose salary is null.
selectname
frominstructor
where salary is null
The predicate is not null succeeds if the value on which it is applied is 
not null.

©Silberschatz, Korth and Sudarshan3.29Database System Concepts -7
th
Edition
Null Values (Cont.)
SQL treats as unknownthe result of any comparison involving a null 
value (other than predicates is null and  is not null).
•Example: 5 < nullornull<> nullornull= null
The predicate in a whereclause can involve Boolean operations (and, 
or, not); thus the definitions of the Boolean operations need to be  
extended to deal with the value unknown.
•and :(trueand unknown)  = unknown,    
(falseand unknown) = false,
(unknown andunknown) = unknown
•or:    (unknownortrue)   = true,
(unknownorfalse)  = unknown
(unknown orunknown) = unknown
Result of where clause predicate is treated as false if it evaluates to 
unknown

©Silberschatz, Korth and Sudarshan3.30Database System Concepts -7
th
Edition
Aggregate Functions
These functions operate on the multiset of values of a column of a 
relation, and return a value
avg: average value
min:  minimum value
max:  maximum value
sum:  sum of values
count:  number of values

©Silberschatz, Korth and Sudarshan3.31Database System Concepts -7
th
Edition
Aggregate Functions Examples
Find the average salary of instructors in the Computer Science department 
•select avg(salary)
from instructor
where dept_name= 'Comp. Sci.';
Find the total number of instructors who teach a course in the Spring 2018 
semester
•select count (distinct ID)
from teaches
where semester = 'Spring' and year = 2018;
Find the number of tuples in the course relation
•select count (*)
from course;

©Silberschatz, Korth and Sudarshan3.32Database System Concepts -7
th
Edition
Aggregate Functions –Group By
Find the average salary of instructors in each department
•select dept_name, avg(salary) asavg_salary
from instructor
group by dept_name;

©Silberschatz, Korth and Sudarshan3.33Database System Concepts -7
th
Edition
Aggregation (Cont.)
Attributes in select clause outside of aggregate functions must appear in 
group bylist
•/* erroneous query */
select dept_name, ID, avg(salary)
from instructor
group by dept_name;

©Silberschatz, Korth and Sudarshan3.34Database System Concepts -7
th
Edition
Aggregate Functions –Having Clause
Find the names and average salaries of all departments whose average 
salary is greater than 42000
Note: predicates in the havingclause are applied after the formation of 
groups whereas predicates in the whereclause are applied before forming 
groups
select dept_name, avg(salary) asavg_salary
from instructor
group by dept_name
having avg(salary) > 42000;

©Silberschatz, Korth and Sudarshan3.35Database System Concepts -7
th
Edition
Nested Subqueries
SQL provides a mechanism for the nesting of subqueries. A subqueryis 
a select-from-whereexpression that is nested within another query.
The nesting can be done in the following SQL query
select A
1
, A
2
, ..., A
n
fromr
1
, r
2
, ..., r
m
where P
as follows:
•From clause: r
i
can be replaced by any valid subquery
•Where clause: Pcan be replaced with an expression of the form:
B<operation> (subquery)
Bis an attribute and <operation> to be defined later.
•Select clause: 
A
i   
can be replaced be a subquery that generates a single value.

©Silberschatz, Korth and Sudarshan3.36Database System Concepts -7
th
Edition
Set Membership

©Silberschatz, Korth and Sudarshan3.37Database System Concepts -7
th
Edition
Set Membership 
Find courses offered in Fall 2017 and in Spring 2018
Find courses offered in Fall 2017 but not in Spring 2018
select distinct course_id
from section
where semester = 'Fall' and year= 2017 and 
course_idin (select course_id
from section
where semester = 'Spring' and year= 2018);
select distinct course_id
from section
where semester = 'Fall' and year= 2017 and 
course_idnot in (select course_id
from section
where semester = 'Spring' and year= 2018);

©Silberschatz, Korth and Sudarshan3.38Database System Concepts -7
th
Edition
Set Membership (Cont.)
Name all instructors whose name is neither “Mozart” nor Einstein”
select distinct name
from instructor
where name not in ('Mozart', 'Einstein') 
Find the total number of (distinct) students who have taken course 
sections taught by the instructor with ID 10101
Note: Above query can be written in a much simpler manner.  
The formulation above is simply to illustrate SQL features
select count (distinct ID)
from takes
where (course_id, sec_id, semester, year) in 
(select course_id, sec_id, semester, year
from teaches
where teaches.ID= 10101);

©Silberschatz, Korth and Sudarshan3.39Database System Concepts -7
th
Edition
Set Comparison

©Silberschatz, Korth and Sudarshan3.40Database System Concepts -7
th
Edition
Set Comparison –“some”Clause
Find names of instructors with salary greater than that of some (at least 
one) instructor in the Biology department.
Same query using > someclause
select name
from instructor
where salary > some (select salary
from instructor
where dept name = 'Biology');
select distinct T.name
from instructor as T, instructor as S
where T.salary> S.salaryand S.deptname = 'Biology';

©Silberschatz, Korth and Sudarshan3.41Database System Concepts -7
th
Edition
Definition of  “some”Clause
F <comp> some r ⇔ ∃ t ∈rsuch that (F <comp> t )
Where <comp> can be:  <,   ≤,   >,   =,  ≠
0
5
6
(5 < some) = true
0
5
0
) = false
5
0
5
(5 ≠some
) = true (since 0 ≠5)
(read:  5 < some tuple in the relation) 
(5 < some
) = true
(5 = some
(= some) ≡in
However, (≠some) ≡not in

©Silberschatz, Korth and Sudarshan3.42Database System Concepts -7
th
Edition
Set Comparison –“all”Clause
Find the names of all instructors whose salary is greater than the salary of 
all instructors in the Biology department.
select name
from instructor
where salary > all (select salary
from instructor
where dept name = 'Biology');

©Silberschatz, Korth and Sudarshan3.43Database System Concepts -7
th
Edition
Definition of “all”Clause
F <comp> all r ⇔ ∀ t ∈r(F <comp> t)
0
5
6
(5 < all) = false
6
10
4
) = true
5
4
6
(5 ≠all
) = true (since 5 
≠4 and 5 ≠6)
(5 < all
) = false
(5 = all
(≠all) ≡not in
However, (= all) ≡in

©Silberschatz, Korth and Sudarshan3.44Database System Concepts -7
th
Edition
Test for Empty Relations
The existsconstruct returns the value trueif the argument subquery is 
nonempty.
exists r ⇔ r ≠Ø
not exists r ⇔ r = Ø

©Silberschatz, Korth and Sudarshan3.45Database System Concepts -7
th
Edition
Use of “exists”Clause
Yet another way of specifying the query “Find all courses taught in both the 
Fall 2017 semester and in the Spring 2018 semester”
select course_id
from section as S
where semester = 'Fall' and year = 2017 and 
exists  (select *
from section as T
where semester = 'Spring' and year= 2018 
and S.course_id= T.course_id);
Correlation name– variable S  in the outer query
Correlated subquery –the inner query

©Silberschatz, Korth and Sudarshan3.46Database System Concepts -7
th
Edition
Use of “not exists”Clause
Find all students who have taken all courses offered in the Biology 
department.
Note that X – Y = Ø   ⇔X ⊆Y
Note: Cannot write this query using = all and its variants
select distinct S.ID, S.name
from student as S
where not exists ( (select course_id
from course
where dept_name= 'Biology')
except
(select T.course_id
from takes as T
where S.ID = T.ID));
•First nested query lists all courses offered in Biology
•Second nested query lists all courses a particular student took

©Silberschatz, Korth and Sudarshan3.47Database System Concepts -7
th
Edition
Test for Absence of Duplicate Tuples
The uniqueconstruct tests whether a subquery has any duplicate tuples 
in its result.
The uniqueconstruct evaluates to “true” if a given subquery contains no 
duplicates .
Find all courses that were offered at most once in 2017
select T.course_id
from course as T
where unique ( select R.course_id
from section as R
where T.course_id= R.course_id
and R.year= 2017);

©Silberschatz, Korth and Sudarshan3.48Database System Concepts -7
th
Edition
Subqueries in the From Clause

©Silberschatz, Korth and Sudarshan3.49Database System Concepts -7
th
Edition
Subqueries in the Form Clause
SQL allows a subquery expression to be used in the from clause
Find the average instructors’ salaries of those departments where the 
average salary is greater than $42,000.”
select dept_name, avg_salary
from ( select dept_name, avg(salary) as avg_salary
from instructor
group by dept_name)
where avg_salary> 42000;
Note that we do not need to use the having clause
Another way to write above query
select dept_name, avg_salary
from ( select dept_name, avg(salary) 
from instructor
group by dept_name) 
as dept_avg(dept_name, avg_salary)
where avg_salary> 42000;

©Silberschatz, Korth and Sudarshan3.50Database System Concepts -7
th
Edition
With Clause
The withclause provides a way of defining a temporary relation whose 
definition is available only to the query in which the withclause occurs. 
Find all departments with the maximum budget 
with max_budget(value) as 
(select max(budget)
from department)
select department.name
from department, max_budget
where department.budget= max_budget.value;

©Silberschatz, Korth and Sudarshan3.51Database System Concepts -7
th
Edition
Complex Queries using With Clause
Find all departments where the total salary is greater than the average of 
the total salary at all departments
with dept_total (dept_name, value) as
(select dept_name, sum(salary)
from instructor
group by dept_name),
dept_total_avg(value) as
(select avg(value)
from dept_total)
select dept_name
from dept_total, dept_total_avg
where dept_total.value> dept_total_avg.value;

©Silberschatz, Korth and Sudarshan3.52Database System Concepts -7
th
Edition
Scalar Subquery
Scalar subquery is one which is used where a single value is expected
List all departments along with the number of instructors in each 
department
select dept_name, 
( select count(*) 
from instructor 
where department.dept_name= instructor.dept_name)
as num_instructors
from department;
Runtime error if subquery returns more than one result tuple

©Silberschatz, Korth and Sudarshan3.53Database System Concepts -7
th
Edition
Modification of the Database
Deletion of tuples from a given relation.
Insertion of new tuples into a given relation
Updating of values in some tuples in a given relation

©Silberschatz, Korth and Sudarshan3.54Database System Concepts -7
th
Edition
Deletion
Delete all instructors
delete from instructor
Delete all instructors from the Finance department
delete from instructor
where dept_name= 'Finance’;
Delete all tuples in the instructor relation for those instructors associated 
with a department located in the Watson building.
delete from instructor
where dept name in (select dept name
from department
where building = 'Watson');

©Silberschatz, Korth and Sudarshan3.55Database System Concepts -7
th
Edition
Deletion (Cont.)
Delete all instructors whose salary is less than the average salary of 
instructors
•Problem:  as we delete tuples from instructor, the average salary 
changes
•Solution used in SQL:
1.First, compute avg(salary) and find all tuples to delete
2.Next, delete all tuples found above (without recomputing avgor 
retesting the tuples)
delete from instructor
where salary < (select avg(salary) 
from instructor);

©Silberschatz, Korth and Sudarshan3.56Database System Concepts -7
th
Edition
Insertion
Add a new tuple to course
insert into course
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
or equivalently
insert into course (course_id, title, dept_name, credits)
values ('CS-437', 'Database Systems', 'Comp. Sci.', 4);
Add a new tuple to student  with tot_creds set to null
insert into student
values ('3003', 'Green', 'Finance', null);

©Silberschatz, Korth and Sudarshan3.57Database System Concepts -7
th
Edition
Insertion (Cont.)
Make each student in the Music department who has earned more than 
144 credit hours an instructor in the Music department with a salary of  
$18,000.
insert into instructor
select ID, name, dept_name, 18000
from student 
where dept_name = 'Music' and total_cred> 144;
The select from wherestatement is evaluated fully before any of its 
results are inserted into the relation.  
Otherwise queries like
insert intotable1 select* fromtable1
would cause problem

©Silberschatz, Korth and Sudarshan3.58Database System Concepts -7
th
Edition
Updates
Give  a  5% salary raise to all instructors
update instructor
set salary = salary * 1.05
Give  a 5% salary raise to those instructors who earn less than 70000
update instructor
set salary = salary * 1.05
where salary < 70000;
Give  a 5% salary raise to instructors whose salary is less than average
update instructor
set salary = salary * 1.05
where salary <  (select avg(salary)
from instructor);

©Silberschatz, Korth and Sudarshan3.59Database System Concepts -7
th
Edition
Updates (Cont.)
Increase salaries of instructors whose salary is over $100,000 by 3%, and 
all others by a 5% 
•Write two update statements:
update instructor
set salary = salary * 1.03
where salary > 100000;
update instructor
set salary = salary * 1.05
where salary <= 100000;
•The order is important
•Can be done better using the case statement (next slide)

©Silberschatz, Korth and Sudarshan3.60Database System Concepts -7
th
Edition
Case Statement for Conditional Updates
Same query as before but with case statement
update instructor
set salary = case
when salary <= 100000 then salary * 1.05
else salary * 1.03
end

©Silberschatz, Korth and Sudarshan3.61Database System Concepts -7
th
Edition
Updates with Scalar Subqueries
Recompute and update tot_creds value for all students
update student S 
set tot_cred = (select sum(credits)
from takes, course
where takes.course_id= course.course_idand 
S.ID= takes.ID.and
takes.grade<> 'F' and
takes.gradeis not null);
Sets tot_credsto null for students who have not taken any course
Instead of sum(credits), use:
case 
when sum(credits) is not null then sum(credits)
else 0
end

©Silberschatz, Korth and Sudarshan3.62Database System Concepts -7
th
Edition
End of Chapter 3
