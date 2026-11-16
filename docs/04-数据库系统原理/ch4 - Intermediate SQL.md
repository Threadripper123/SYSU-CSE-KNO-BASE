# ch4 - Intermediate SQL

> 主题库：数据库系统原理　｜　源文件：ch4 - Intermediate SQL.pdf　｜　24,059 字符　｜　58 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korthand Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 4 : Intermediate SQL

©Silberschatz, Korthand Sudarshan4.2Database System Concepts -7
th
Edition
Chapter 4:  Intermediate SQL
Join  Expressions
Views
Transactions
Integrity Constraints
SQL Data Types and Schemas
Index Definition in SQL
Authorization

©Silberschatz, Korthand Sudarshan4.3Database System Concepts -7
th
Edition
Joined Relations
Join operationstake two relations and return as a 
result another relation.
A join operation is a Cartesian product which requires 
that tuples in the two relations match (under some 
condition).  It also specifies the attributes that are 
present in the result of the join
The join operations are typically used as subquery 
expressions in the from clause
Three types of joins:
•Natural join
•Inner join
•Outer join

©Silberschatz, Korthand Sudarshan4.4Database System Concepts -7
th
Edition
Natural Join in SQL
Natural join matches tuples with the same values for all 
common attributes, and retains only one copy of each 
common column.
List the names of instructors along with the course ID of 
the courses that they taught
•select name, course_id
from students, takes
where student.ID = takes.ID;
Same query in SQL with “natural join” construct
•select name,course_id
from student natural join takes;

©Silberschatz, Korthand Sudarshan4.5Database System Concepts -7
th
Edition
Natural Join in SQL (Cont.)
The fromclause in can have multiple relations combined 
using natural join:
select A
1
, A
2
, ... A
n
from r
1
natural join r
2
natural join .. natural join r
n
where  P ;

©Silberschatz, Korthand Sudarshan4.6Database System Concepts -7
th
Edition
Student Relation

©Silberschatz, Korthand Sudarshan4.7Database System Concepts -7
th
Edition
Takes Relation

©Silberschatz, Korthand Sudarshan4.8Database System Concepts -7
th
Edition
studentnatural join takes

©Silberschatz, Korthand Sudarshan4.9Database System Concepts -7
th
Edition
Dangerous in Natural Join
Beware of unrelated attributes with same name which get 
equated incorrectly
Example -- List the names of students instructors along with 
the titles of courses that they have taken
•Correct version
select name, title
from student natural join takes, course
where takes.course_id= course.course_id;
•Incorrect version
select name, title
from student natural join takes natural join course;
This query omits all (student name, course title) pairs where 
the student takes a course in a department other than the 
student's own department. 
The  correct  version (above), correctly outputs such pairs.

©Silberschatz, Korthand Sudarshan4.13Database System Concepts -7
th
Edition
Outer Join
An extension of the join operation that avoids loss of 
information.
Computes the join and then adds tuples form one relation 
that does not match tuples in the other relation to the result 
of the join. 
Uses nullvalues.
Three forms of outer join:
•left outer join
•right outer join
•full outer join

©Silberschatz, Korthand Sudarshan4.14Database System Concepts -7
th
Edition
Outer Join Examples
Relation course
Relation prereq
Observe that 
courseinformationis missing  for  CS-437
prereqinformationis missing for CS-315

©Silberschatz, Korthand Sudarshan4.15Database System Concepts -7
th
Edition
Left Outer Join
coursenatural left outer joinprereq
In relational algebra:   course ⟕prereq

©Silberschatz, Korthand Sudarshan4.16Database System Concepts -7
th
Edition
Right Outer Join
coursenatural right outer joinprereq
In relational algebra:  course ⟖prereq

©Silberschatz, Korthand Sudarshan4.17Database System Concepts -7
th
Edition
Full Outer Join
coursenatural full outer joinprereq
In relational algebra:      course ⟗prereq

©Silberschatz, Korthand Sudarshan4.18Database System Concepts -7
th
Edition
Joined Types and Conditions
Join operationstake two relations and return as a result 
another relation.
These additional operations are typically used as subquery 
expressions in the from clause
Join condition– defines which tuples in the two relations 
match, and what attributes are present in the result of the join.
Join type– defines how tuples in each relation that do not 
match any tuple in the other relation (based on the join 
condition) are treated.

©Silberschatz, Korthand Sudarshan4.19Database System Concepts -7
th
Edition
Joined Relations –Examples
coursenatural right outer join prereq
coursefull outer join prerequsing (course_id)

©Silberschatz, Korthand Sudarshan4.20Database System Concepts -7
th
Edition
Joined Relations –Examples 
course inner join prereqon
course.course_id= prereq.course_id
What is the difference between the above, and a natural 
join? 
course left outer joinprereqon
course.course_id= prereq.course_id

©Silberschatz, Korthand Sudarshan4.21Database System Concepts -7
th
Edition
Joined Relations –Examples
coursenatural right outer join prereq
coursefull outer join prerequsing (course_id)

©Silberschatz, Korthand Sudarshan4.22Database System Concepts -7
th
Edition
Views
In some cases, it is not desirable for all users to see the 
entire logical model (that is, all the actual relations stored in 
the database.)
Consider a person who needs to know an instructors name 
and department, but not the salary.  This person should see a 
relation described, in SQL, by 
select ID, name, dept_name
from instructor
A viewprovides a mechanism to hide certain data from the 
view of certain users. 
Any relation that is not of the conceptual model but is made 
visible to a user as a “virtual relation” is called a view.

©Silberschatz, Korthand Sudarshan4.23Database System Concepts -7
th
Edition
View Definition
A view is defined using the create view statement which 
has the form
create view vas < query expression >
where <query expression> is any legal SQL expression.  
The view name is represented by v.
Once a view is defined, the view name can be used to refer 
to the virtual relation that the view generates.
View definition is not the same as creating a new relation 
by evaluating the query expression
•Rather, a view definition causes the saving of an 
expression; the expression is substituted into queries 
using the view.

©Silberschatz, Korthand Sudarshan4.24Database System Concepts -7
th
Edition
View Definition and Use
A view of instructors without their salary
create view facultyas
select ID, name, dept_name
from instructor
Find all instructors in the Biology department
select name
from faculty
where dept_name = 'Biology'
Create a view of department salary totals
create view departments_total_salary(dept_name, total_salary) as
select dept_name, sum (salary)
from instructor
group by dept_name;

©Silberschatz, Korthand Sudarshan4.25Database System Concepts -7
th
Edition
Views Defined Using Other Views
One view may be used in the expression defining another 
view 
A view relation v
1
is said to depend directly on a view 
relation v
2
if v
2
is used in the expression defining v
1
A view relation v
1
is said to depend onview relation v
2
if 
either v
1 
depends directly to v
2 
or there is a path of 
dependencies from v
1
to v
2
A view relation vis said to be recursiveif it depends on 
itself.

©Silberschatz, Korthand Sudarshan4.26Database System Concepts -7
th
Edition
Views Defined Using Other Views
create view physics_fall_2017as
select course.course_id, sec_id, building, room_number
from course, section
where course.course_id= section.course_id
and course.dept_name= 'Physics'
and section.semester= 'Fall'
and section.year= '2017';
create view physics_fall_2017_watson as
select course_id, room_number
from physics_fall_2017
where building= 'Watson';

©Silberschatz, Korthand Sudarshan4.27Database System Concepts -7
th
Edition
View Expansion
Expand  the view :
create view physics_fall_2017_watsonas
select course_id, room_number
from physics_fall_2017
where building= 'Watson'
To:
create view physics_fall_2017_watsonas
select course_id, room_number
from (select course.course_id, building, room_number
from course, section
where course.course_id= section.course_id
and course.dept_name= 'Physics'
and section.semester= 'Fall'
and section.year= '2017')
where building= 'Watson';

©Silberschatz, Korthand Sudarshan4.28Database System Concepts -7
th
Edition
View Expansion (Cont.)
A way to define the meaning of views defined in terms of other 
views.
Let view v
1
be defined by an expression e
1
that may itself 
contain uses of view relations.
View expansion of an expression repeats the following 
replacement step:
repeat
Find any view relation v
i
in e
1
Replace the view relation v
i
by the expression defining v
i
untilno more view relations are present in e
1
As long as the view definitions are not recursive, this loop will 
terminate

©Silberschatz, Korthand Sudarshan4.29Database System Concepts -7
th
Edition
Materialized Views
Certain database systems allow view relations to be 
physically stored.
•Physical copy created when the view is defined.
•Such views are called Materialized view:
If relations used in the query are updated, the 
materialized view result becomes out of date
•Need to maintainthe view, by updating the view 
whenever the underlying relations are updated.

©Silberschatz, Korthand Sudarshan4.30Database System Concepts -7
th
Edition
Update of a View
Add a new tuple to faculty view which we defined earlier
insert into faculty 
values ('30765', 'Green', 'Music');
This insertion must be represented by the insertion into  the 
instructorrelation
•Must have a  value for salary.
Two approaches
•Reject the insert
•Inset the tuple
('30765', 'Green', 'Music', null)
into the instructorrelation

©Silberschatz, Korthand Sudarshan4.31Database System Concepts -7
th
Edition
Some Updates Cannot be Translated Uniquely
create view instructor_infoas
select ID, name, building
from instructor, department
where instructor.dept_name= department.dept_name;
insert into instructor_info
values ('69987', 'White', 'Taylor');
Issues
•Which department, if multiple departments in Taylor?
•What if no department is in Taylor?

©Silberschatz, Korthand Sudarshan4.32Database System Concepts -7
th
Edition
And Some Not at All
create view history_instructorsas
select *
from instructor
where dept_name= 'History';
What happens if we insert
('25566', 'Brown', 'Biology', 100000)
into history_instructors?

©Silberschatz, Korthand Sudarshan4.33Database System Concepts -7
th
Edition
View Updates in SQL 
Most SQL implementations allow updates only on simple 
views
•The from clause has only one database relation.
•The select clause contains only attribute names of the 
relation, and does not have any expressions, aggregates, 
or distinct specification.
•Any attribute not listed in the select clause can be set to 
null
•The query does not have a group by or having clause.

©Silberschatz, Korthand Sudarshan4.34Database System Concepts -7
th
Edition
Transactions
Atransaction consists of a sequence of query and/or update 
statements and is a “unit” of work
The SQL standard specifies that a transaction begins implicitly 
when an SQL statement is executed.  
The transaction must end with one of the following statements:
•Commit work. The updates performed by the transaction 
become permanent in the database. 
•Rollback work. All  the updates performed by the SQL 
statements in the transaction are undone.
Atomic transaction
•either fully executed or rolled back as if it never occurred
Isolation from concurrent transactions

©Silberschatz, Korthand Sudarshan4.35Database System Concepts -7
th
Edition
Integrity Constraints
Integrity constraints guard against accidental damage 
to the database, by ensuring that authorized changes 
to the database do not result in a loss of data 
consistency. 
•A checking account must have a balance greater 
than $10,000.00
•A salary of a bank employee must be at least 
$4.00 an hour
•A customer must have a (non-null) phone number

©Silberschatz, Korthand Sudarshan4.36Database System Concepts -7
th
Edition
Constraints on a Single Relation
not null
primary key
unique
check (P), where P is a predicate

©Silberschatz, Korthand Sudarshan4.37Database System Concepts -7
th
Edition
Not Null Constraints 
not null
•Declare nameand budgetto be not null
name varchar(20) not null
budget numeric(12,2) not null

©Silberschatz, Korthand Sudarshan4.38Database System Concepts -7
th
Edition
Unique Constraints 
unique( A
1
, A
2
, ..., A
m
)
•The unique specification states that the attributes 
A
1
, A
2
, ..., A
m 
form a candidate key.
•Candidate keys are permitted to be null (in contrast 
to primary keys).

©Silberschatz, Korthand Sudarshan4.39Database System Concepts -7
th
Edition
The check clause
The  check (P) clause specifies a predicate P that must be 
satisfied by every tuple in a relation.
Example:  ensure that semester is one of fall, winter, spring or 
summer
create table section 
(course_idvarchar (8),
sec_idvarchar (8),
semester varchar (6),
year numeric (4,0),
building varchar (15),
room_numbervarchar (7),
time slot id varchar (4), 
primary key (course_id, sec_id, semester, year),
check(semester in ('Fall', 'Winter', 'Spring', 'Summer')))

©Silberschatz, Korthand Sudarshan4.40Database System Concepts -7
th
Edition
Referential Integrity
Ensures that a value that appears in one relation for a 
given set of attributes also appears for a certain set of 
attributes in another relation.
•Example:  If “Biology” is a department name 
appearing in one of the tuples in the instructor
relation, then there exists a tuple in the 
departmentrelation for “Biology”.
Let A be a set of attributes.  Let R and S be two 
relations that contain attributes A and where A is the 
primary key of S. A is said to be a  foreign keyof R if 
for any values of A appearing in R these values also 
appear in S.

©Silberschatz, Korthand Sudarshan4.41Database System Concepts -7
th
Edition
Referential Integrity (Cont.)
Foreign keys can be specified as part of the SQL create
table statement 
foreign key (dept_name) references department
By default, a foreign key references the primary-key 
attributes of the referenced table.
SQL allows  a list of attributes of the referenced relation to 
be specified explicitly.
foreign key (dept_name) references department 
(dept_name)

©Silberschatz, Korthand Sudarshan4.42Database System Concepts -7
th
Edition
Cascading Actions in Referential Integrity
When a referential-integrity constraint is violated, the normal 
procedure is to reject the action that caused the violation.
An alternative, in case of delete or update is to cascade
create table course (
(...
dept_name varchar(20),
foreign key (dept_name) references department
on delete cascade
on update cascade,
. . .) 
Instead of cascade we can use :  
•set null,
•set default

©Silberschatz, Korthand Sudarshan4.43Database System Concepts -7
th
Edition
Integrity Constraint Violation During Transactions
Consider:
create table person (
IDchar(10),
name char(40),
motherchar(10),
father char(10),
primary keyID,
foreign key fatherreferences person,
foreign key motherreferences person)
How to insert a tuple without causing constraint violation?
•Insert father and mother of a person before inserting person
•OR, set father and mother to null initially, update after 
inserting all persons (not possible if father and mother 
attributes declared to be not null) 
•OR defer constraintchecking

©Silberschatz, Korthand Sudarshan4.44Database System Concepts -7
th
Edition
Complex Check Conditions
The predicate in the check clause can be an arbitrary predicate 
that can include a subquery.
check (time_slot_idin (select time_slot_idfrom time_slot))
The check condition states  that the  time_slot_idin each tuple 
in the sectionrelation is actually the identifier of a time slot in 
the time_slotrelation.
•The condition has to be checked not only when a tuple is 
inserted or modified in section, but also when the relation 
time_slotchanges 

©Silberschatz, Korthand Sudarshan4.45Database System Concepts -7
th
Edition
Assertions
An assertionis a predicate expressing a condition that we 
wish the database always to satisfy.
The following constraints, can be expressed using assertions:
For each tuple in the studentrelation, the value of the attribute 
tot_credmust equal the sum of credits of courses that the 
student has completed successfully.
An instructor cannot teach in two different classrooms in a 
semester in the same time slot
An assertion in SQL takes the form:
create assertion<assertion-name> check (<predicate>);

©Silberschatz, Korthand Sudarshan4.46Database System Concepts -7
th
Edition
Built-in Data Types in SQL 
date:Dates, containing a (4 digit) year, month and date
•Example:  date'2005-7-27'
time:Time of day, in hours, minutes and seconds.
•Example: time'09:00:30'        time'09:00:30.75'
timestamp:date plus time of day
•Example:  timestamp'2005-7-27 09:00:30.75'
interval:period of time
•Example:   interval  '1' day
•Subtracting a date/time/timestamp value from another 
gives an interval value
•Interval values can be added to date/time/timestamp 
values

©Silberschatz, Korthand Sudarshan4.47Database System Concepts -7
th
Edition
Large-Object Types
Large objects (photos, videos, CAD files, etc.) are stored as 
a large object:
•blob: binary large object -- object is a large collection of 
uninterpreted binary data (whose interpretation is left to 
an application outside of the database system)
•clob: character large object -- object is a large collection 
of character data
When a query returns a large object, a pointer is returned 
rather than the large object itself.

©Silberschatz, Korthand Sudarshan4.48Database System Concepts -7
th
Edition
User-Defined Types
create type construct in SQL creates user-defined type
create type Dollarsas numeric (12,2) final 
Example:
create table department
(dept_name varchar(20),
building varchar(15),
budget Dollars);

©Silberschatz, Korthand Sudarshan4.49Database System Concepts -7
th
Edition
Domains
create domainconstruct in SQL-92 creates user-defined 
domain types
create domain person_namechar(20) not null
Types and domains are similar.  Domains can have 
constraints, such as not null, specified on them.
Example:
create domain degree_levelvarchar(10)
constraint degree_level_test
check (value in ('Bachelors', 'Masters', 'Doctorate'));

©Silberschatz, Korthand Sudarshan4.50Database System Concepts -7
th
Edition
Index Creation
Many queries reference only a small proportion of the records 
in a table. 
It is inefficient for the system to read every record to find  a 
record with  particular value
An indexon an attribute of a relation is a data structure that 
allows the database system to find those tuples in the relation 
that have a specified value for that attribute efficiently, without 
scanning through all the tuples of the relation.
We create an index with the create index command
create index <name> on <relation-name> (attribute);

©Silberschatz, Korthand Sudarshan4.51Database System Concepts -7
th
Edition
Index Creation Example
create table student
(ID varchar(5),
name varchar(20) not null,
dept_name varchar(20),
tot_cred numeric (3,0) default 0,
primary key (ID))
create index studentID_indexon student(ID)
The query:
select * 
from student
where ID = '12345'
can be executed by using the index to find the required 
record,  without looking at all records of student

©Silberschatz, Korthand Sudarshan4.52Database System Concepts -7
th
Edition
Authorization
We may assign a user several forms of authorizations on 
parts of the database.
•Read- allows reading, but not modification of data.
•Insert- allows insertion of new data, but not 
modification of existing data.
•Update- allows modification, but not deletion of data.
•Delete- allows deletion of data.
Each of these types of authorizations is called a privilege. 
We may authorize the user all, none, or a combination of 
these types of privileges on specified parts of a database, 
such as a relation or a view.

©Silberschatz, Korthand Sudarshan4.53Database System Concepts -7
th
Edition
Authorization (Cont.)
Forms of authorization to modify the database schema
•Index- allows creation and deletion of indices.
•Resources- allows creation of new relations.
•Alteration- allows addition or deletion of attributes in a 
relation.
•Drop- allows deletion of relations.

©Silberschatz, Korthand Sudarshan4.54Database System Concepts -7
th
Edition
Authorization Specification in SQL
The grantstatement is used to confer authorization
grant<privilege list>on <relation or view > to<user list>
<user list> is:
•a user-id
•public, which allows all valid users the privilege 
granted
•A role (more on this later)
Example:
•grantselect on  departmenttoAmit,  Satoshi
Granting a privilege on a view does not imply granting any 
privileges on the underlying relations.
The grantor of the privilege must already hold the privilege 
on the specified item (or be the database administrator).

©Silberschatz, Korthand Sudarshan4.55Database System Concepts -7
th
Edition
Privileges in SQL
select: allows read access to relation, or the ability to 
query using the view
•Example: grant users U
1
, U
2
, and U
3
select
authorization on theinstructorrelation:
grant select on instructor to U
1
, U
2
, U
3
insert: the ability to insert tuples
update: the ability  to update using the SQL update 
statement
delete: the ability to delete tuples.
all privileges: used as a short form for all the allowable 
privileges

©Silberschatz, Korthand Sudarshan4.56Database System Concepts -7
th
Edition
Revoking Authorization in SQL
The revokestatement is used to revoke authorization.
revoke <privilege list>on <relation or view> from <user 
list>
Example:
revoke select on student  from U
1
, U
2
, U
3
<privilege-list> may be all to revoke all privileges the 
revokeemay hold.
If <revokee-list> includes public, all users lose the 
privilege except those granted it explicitly.
If the same privilege was granted twice to the same user 
by different grantees, the user may retain the privilege 
after the revocation.
All privileges that depend on the privilege being revoked 
are also revoked.

©Silberschatz, Korthand Sudarshan4.57Database System Concepts -7
th
Edition
Roles
Aroleis  a way to distinguish among various users as far 
as what  these users can access/update in the database.
To create a role we use:
create a role <name>
Example:
•create roleinstructor
Once a role is created we can assign “users” to the role 
using:
•grant<role> to <users>

©Silberschatz, Korthand Sudarshan4.58Database System Concepts -7
th
Edition
Roles Example
create roleinstructor;
grantinstructorto Amit;
Privileges can be granted to roles:
•grantselectontakestoinstructor;
Roles can be granted to users, as well as to other roles
•createroleteaching_assistant
•grantteaching_assistanttoinstructor;
Instructorinherits all privileges of teaching_assistant
Chain of roles
•createroledean;
•grantinstructortodean;
•grantdeantoSatoshi;

©Silberschatz, Korthand Sudarshan4.59Database System Concepts -7
th
Edition
Authorization on Views
create view geo_instructoras
(select *
from instructor
where dept_name = 'Geology');
grant select on geo_instructorto geo_staff
Suppose that a  geo_staffmember issues
•select *
from geo_instructor;
What if 
•geo_staffdoes not have permissions on instructor?
•creator of view did not have some permissions on 
instructor?

©Silberschatz, Korthand Sudarshan4.60Database System Concepts -7
th
Edition
Other Authorization Features
referencesprivilege to create foreign key
•grant reference (dept_name) on department to Mariano;
•why is this required?
transfer of privileges
•grant select on department to Amitwith grant option;
•revoke select on department from Amit, Satoshi cascade;
•revoke select on department from Amit, Satoshi restrict;
•And more!

©Silberschatz, Korthand Sudarshan4.61Database System Concepts -7
th
Edition
End of Chapter 4
