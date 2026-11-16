# ch5 - Advanced SQL

> 主题库：数据库系统原理　｜　源文件：ch5 - Advanced SQL.pdf　｜　33,561 字符　｜　71 个片段　｜　更新于 2026-11-11


Database System Concepts, 7
th
Ed.
©Silberschatz, Korth and Sudarshan
See www.db-book.comfor conditions on re-use 
Chapter 5: Advanced SQL

©Silberschatz, Korth and Sudarshan5.2Database System Concepts -7
th
Edition
Outline
Accessing SQL From a Programming Language
Functions and Procedures
Triggers
Recursive Queries
Advanced Aggregation Features

©Silberschatz, Korth and Sudarshan5.3Database System Concepts -7
th
Edition
Accessing SQL from a Programming Language
Not all queries can be expressed in SQL, since SQL does not provide 
the full expressive power of a general-purpose language.
Non-declarative actions -- such as printing a report, interacting with a 
user, or sending the results of a query to a graphical user interface --
cannot be done from within SQL.
A database programmer must have access to a general-purpose programming 
language for at least two reasons

©Silberschatz, Korth and Sudarshan5.4Database System Concepts -7
th
Edition
Accessing SQL from a Programming Language (Cont.)
A general-purpose program  --can connect to and communicate with 
a database server using a collection of functions
Embedded SQL --provides a means by which a program can interact 
with a database server.  
•The  SQL statements are translated at compile time  into function 
calls.  
•At runtime,  these function calls connect to the database  using an 
API  that provides dynamic  SQL facilities.
There are two approaches to accessing  SQL from a general-purpose 
programming language

©Silberschatz, Korth and Sudarshan5.5Database System Concepts -7
th
Edition
JDBC

©Silberschatz, Korth and Sudarshan5.6Database System Concepts -7
th
Edition
JDBC
JDBCis a Java API for communicating with database systems supporting 
SQL.
JDBC supports a variety of features for querying and updating data, and 
for retrieving query results.
JDBC also supports metadata retrieval, such as querying about relations 
present in the database and the names and types of relation attributes.
Model for communicating with the database:
•Open a connection
•Create a “statement”object
•Execute queries using the statement object to send queries and fetch 
results
•Exception mechanism to handle errors

©Silberschatz, Korth and Sudarshan5.7Database System Concepts -7
th
Edition
JDBC Code
public static void JDBCexample(String dbid, String userid, String passwd) 
{ 
try (Connection conn = DriverManager.getConnection(     
"jdbc:oracle:thin:@db.yale.edu:2000:univdb", userid, passwd); 
Statement stmt=   conn.createStatement();
) 
{ 
... Do Actual Work ....
}
catch (SQLExceptionsqle) { 
System.out.println("SQLException: " + sqle);
}
}
NOTE: Above syntax works with Java 7, and JDBC 4 onwards. 
Resources opened in “try (....)” syntax (“try with resources”) are 
automatically closed at the end of the try block

©Silberschatz, Korth and Sudarshan5.8Database System Concepts -7
th
Edition
JDBC Code for  Older Versions of Java/JDBC
public static void JDBCexample(String dbid, String userid, String passwd) 
{ 
try { 
Class.forName("oracle.jdbc.driver.OracleDriver"); 
Connection conn = DriverManager.getConnection(     
"jdbc:oracle:thin:@db.yale.edu:2000:univdb", userid, passwd); 
Statement stmt=   conn.createStatement(); 
... Do Actual Work ....
stmt.close();
conn.close();
}
catch (SQLExceptionsqle) { 
System.out.println("SQLException: " + sqle);
}
}
NOTE:  Class.forNameis not required from JDBC 4 onwards. The try with 
resources syntax  in prevslide is preferred for Java 7 onwards. 

©Silberschatz, Korth and Sudarshan5.9Database System Concepts -7
th
Edition
JDBC Code (Cont.)
Update to database
try {
stmt.executeUpdate(
"insert into instructor values('77987', 'Kim', 'Physics', 98000)");
} catch (SQLExceptionsqle)
{
System.out.println("Could not insert tuple. " + sqle);
}
Execute query and fetch and print results
ResultSetrset=   stmt.executeQuery(
"select dept_name, avg(salary)
from instructor
group by dept_name");
while (rset.next()) {
System.out.println(rset.getString("dept_name") + " " +
rset.getFloat(2));
}

©Silberschatz, Korth and Sudarshan5.10Database System Concepts -7
th
Edition
JDBC SUBSECTIONS       
Connecting to the Database
Shipping SQL Statements to the Database System
Exceptions and Resource Management
Retrieving the Result of a Query
Prepared Statements
Callable Statements
Metadata Features
Other Features
Database Access from Python

©Silberschatz, Korth and Sudarshan5.11Database System Concepts -7
th
Edition
JDBC Code Details       
Getting result fields:
•rs.getString(“dept_name”) and rs.getString(1) equivalent if 
dept_nameis the first argument of select result.
Dealing with Null values
inta = rs.getInt(
“a”);
if (rs.wasNull()) Systems.out.println(
“Got null value”);

©Silberschatz, Korth and Sudarshan5.12Database System Concepts -7
th
Edition
Prepared Statement
PreparedStatementpStmt=   conn.prepareStatement( 
"insert into instructor values(?,?,?,?)");
pStmt.setString(1, "88877");
pStmt.setString(2, "Perry");
pStmt.setString(3, "Finance");
pStmt.setInt(4, 125000);
pStmt.executeUpdate();
pStmt.setString(1, "88878");
pStmt.executeUpdate();
WARNING: always use prepared statements when taking an input from 
the user and adding it to a query
•NEVER create a query by concatenating strings
•"insert into instructor values(' " + ID + " ', ' " + name + " ', " + " ' + dept 
name + " ', " ' balance + ')“
•What if name is “D'Souza”?

©Silberschatz, Korth and Sudarshan5.13Database System Concepts -7
th
Edition
SQL Injection
Suppose query is constructed using
•"select * from instructor where name = '" + name + "'"
Suppose the user, instead of entering a name, enters:
•X' or 'Y' =   'Y
then the resulting statement becomes:
•"select * from instructor where name = '" + "X' or 'Y' =   'Y" + "'"
•which is:
select * from instructor where name = 'X' or 'Y' =   'Y'
•User could have even used
X'; update instructor set salary = salary + 10000; --
Prepared stamentinternally uses:
"select * from instructor where name = 'X\' or \'Y\' =   \'Y'
•Always use prepared statements, with user inputs as parameters

©Silberschatz, Korth and Sudarshan5.14Database System Concepts -7
th
Edition
Metadata Features
ResultSet metadata
E.g.after executing query to get a ResultSet rs:
•ResultSetMetaData rsmd = rs.getMetaData();
for(int i = 1; i <= rsmd.getColumnCount(); i++) {
System.out.println(rsmd.getColumnName(i));
System.out.println(rsmd.getColumnTypeName(i));
}
How is this useful?

©Silberschatz, Korth and Sudarshan5.15Database System Concepts -7
th
Edition
Metadata (Cont)
Database metadata
DatabaseMetaDatadbmd=   conn.getMetaData();
// Arguments to getColumns: Catalog, Schema-pattern, Table-pattern,
// and Column-Pattern
// Returns: One row for each column; row has a number of attributes
// such as COLUMN_NAME, TYPE_NAME
// The value null indicates all Catalogs/Schemas.  
// The value “” indicates current catalog/schema
// The value “%” has the same meaning as SQL likeclause
ResultSetrs =   dbmd.getColumns(null, "univdb", "department", "%");
while( rs.next()) {
System.out.println(rs.getString("COLUMN_NAME"),
rs.getString("TYPE_NAME");
}
And where is this useful?

©Silberschatz, Korth and Sudarshan5.16Database System Concepts -7
th
Edition
Metadata (Cont)
Database metadata
DatabaseMetaDatadbmd=   conn.getMetaData();
// Arguments to getTables: Catalog, Schema-pattern, Table-pattern,
// and Table-Type
// Returns: One row for each table; row has a number of attributes
// such as TABLE_NAME, TABLE_CAT, TABLE_TYPE, ..
// The value null indicates all Catalogs/Schemas.  
// The value “” indicates current catalog/schema
// The value “%” has the same meaning as SQL likeclause
// The last attribute is an array of types of tables to return.  
//    TABLE means only regular tables
ResultSetrs =   dbmd.getTables(“”, "", “%", new String[] {“TABLES”});
while( rs.next()) {
System.out.println(rs.getString(“TABLE_NAME“));
}
And where is this useful?

©Silberschatz, Korth and Sudarshan5.17Database System Concepts -7
th
Edition
Finding Primary Keys
DatabaseMetaDatadmd=   connection.getMetaData();
// Arguments below are:  Catalog, Schema, and Table
// The value “”  for Catalog/Schema indicates current catalog/schema
//  The value null indicates all catalogs/schemas
ResultSetrs =   dmd.getPrimaryKeys(“”, “”, tableName);
while(rs.next()){
// KEY_SEQ indicates the position of the attribute in 
// the primary key, which is required if a primary key has multiple
// attributes
System.out.println(rs.getString(“KEY_SEQ”),  
rs.getString("COLUMN_NAME");
}

©Silberschatz, Korth and Sudarshan5.18Database System Concepts -7
th
Edition
Transaction Control in JDBC
By default, each SQL statement is treated as a separate transaction that 
is committed automatically
•bad idea for transactions with multiple updates
Can turn off automatic commit on a connection
•conn.setAutoCommit(false);
Transactions must then be committed or rolled back explicitly
•conn.commit();     or
•conn.rollback();
conn.setAutoCommit(true) turns on automatic commit.

©Silberschatz, Korth and Sudarshan5.19Database System Concepts -7
th
Edition
Other JDBC Features
Calling functions and procedures
•CallableStatementcStmt1 = conn.prepareCall("{? = call some 
function(?)}");
•CallableStatementcStmt2 = conn.prepareCall("{call some 
procedure(?,?)}");
Handling large object types
•getBlob() and getClob() that are similar to the getString() method, but 
return objects of type Blob and Clob, respectively
•get data from these objects by getBytes()
•associate an open stream with Java Blob or Clobobject to update large 
objects
blob.setBlob(intparameterIndex, InputStreaminputStream).

©Silberschatz, Korth and Sudarshan5.20Database System Concepts -7
th
Edition
JDBC Resources
JDBC Basics Tutorial
•https://docs.oracle.com/javase/tutorial/jdbc/index.html

©Silberschatz, Korth and Sudarshan5.21Database System Concepts -7
th
Edition
SQLJ
JDBC is overly dynamic, errors cannot be caught by compiler
SQLJ: embedded SQL in Java
•#sqliterator deptInfoIter( String deptname, intavgSal);
deptInfoIteriter= null;
#sqliter= { select dept_name, avg(salary) from instructor
group by deptname };
while (iter.next()) {
String deptName=   iter.dept_name();
intavgSal=   iter.avgSal();
System.out.println(deptName+ " " + avgSal);
}
iter.close();

©Silberschatz, Korth and Sudarshan5.22Database System Concepts -7
th
Edition
ODBC

©Silberschatz, Korth and Sudarshan5.23Database System Concepts -7
th
Edition
ODBC
Open DataBaseConnectivity (ODBC) standard 
•standard for application program to communicate with a database 
server.
•application program interface (API) to 
open a connection with a database, 
send queries and updates, 
get back results.
Applications such as GUI, spreadsheets, etc. can use ODBC

©Silberschatz, Korth and Sudarshan5.24Database System Concepts -7
th
Edition
Embedded SQL
The SQL standard defines embeddingsof SQL in a variety of programming 
languages such as C, C++, Java, Fortran, and PL/1, 
A language to which SQL queries are embedded is referred to as a host 
language, and the SQL structures permitted in the host language comprise 
embedded SQL.
The basic form of these languages follows that of the System R embedding of 
SQL into PL/1.
EXEC SQLstatement is used in the host language to identify embedded SQL 
request to the preprocessor
EXEC SQL <embedded SQL statement >;
Note:  this varies by language: 
•In some languages, like COBOL,  the semicolon is replaced with END-
EXEC 
•In Java embedding uses    # SQL { .... };

©Silberschatz, Korth and Sudarshan5.25Database System Concepts -7
th
Edition
Embedded SQL (Cont.)
Before executing any SQL statements, the program must first connect to 
the database.  This is done using:
EXEC-SQL connect to  serveruseruser-name usingpassword;
Here, serveridentifies the server to which a connection is to be 
established.
Variables of the host language can be used within embedded SQL 
statements.  They are preceded  by a colon  (:) to distinguish from SQL 
variables (e.g.,  :credit_amount)
Variables used as above must be declared within DECLARE section, as 
illustrated below. The syntax for declaring the variables, however, follows 
the usual host language syntax.
EXEC-SQL BEGIN DECLARE SECTION}
intcredit-amount ;
EXEC-SQL END DECLARE SECTION;

©Silberschatz, Korth and Sudarshan5.26Database System Concepts -7
th
Edition
Embedded SQL (Cont.)
To write an embedded SQL query, we use the 
declare ccursor for  <SQL query> 
statement.  The  variable cis used to identify the query
Example:
•From within a host language, find the ID and name of students who  
have completed more than the number of credits stored in variable 
credit_amountin the host langue
•Specify the query in SQL as follows:
EXEC SQL
declare ccursor for 
select ID, name
from student
where tot_cred> :credit_amount
END_EXEC

©Silberschatz, Korth and Sudarshan5.27Database System Concepts -7
th
Edition
Embedded SQL (Cont.)
Theopenstatement for our example is as follows:
EXEC SQL openc;
This statement causes the database system to execute the query and  to 
save the results within a temporary relation.  The query uses the value of 
the host-language variable credit-amountat the time the openstatement 
is executed.
The fetch statement causes the values of one tuple in the query result to 
be placed on host language variables.
EXEC SQLfetch c into :si, :snEND_EXEC
Repeated calls to fetch get successive tuples in the query result

©Silberschatz, Korth and Sudarshan5.28Database System Concepts -7
th
Edition
Embedded SQL (Cont.)
A variable called SQLSTATE in the SQL communication area (SQLCA) 
gets set to '02000' to indicate no more data is available
The closestatement causes the database system to delete the 
temporary relation that holds the result of the query.
EXEC SQL closec;
Note: above details vary with language.  For example, the Java              
embedding defines Java iterators to step through result tuples.

©Silberschatz, Korth and Sudarshan5.29Database System Concepts -7
th
Edition
Updates Through Embedded SQL
Embedded SQL expressions for database modification (update, insert, 
and delete)
Can update tuples fetched by cursor by declaring that the cursor is for 
update
EXEC SQL 
declare c cursor for
select *
from instructor
wheredept_name=   'Music'
for update
We then iterate through the tuples by performing  fetchoperations on the 
cursor (as illustrated earlier), and after fetching each tuple we execute the 
following code:
update instructor
setsalary = salary+ 1000
where current of c

©Silberschatz, Korth and Sudarshan5.30Database System Concepts -7
th
Edition
Functions and Procedures

©Silberschatz, Korth and Sudarshan5.31Database System Concepts -7
th
Edition
Functions and Procedures
Functions and procedures allow  “business logic”  to be stored in the 
database and executed from SQL statements.
These can be defined either by the procedural component of SQL or  by 
an external programming language such as Java, C, or C++.
The syntax we present here is defined by the SQL standard.
•Most databases implement nonstandard versions of this syntax.

©Silberschatz, Korth and Sudarshan5.32Database System Concepts -7
th
Edition
Declaring SQL Functions
Define a function that, given the name of a department, returns the count of 
the number of instructors in that department.
create function dept_count(dept_namevarchar(20))
returns integer
begin
declare d_countinteger;
select count (* ) into d_count
from instructor
where instructor.dept_name=   dept_name
return d_count;
end
The function dept_countcan be used to find the department names and 
budget of all departments with more that 12 instructors.
select dept_name, budget
fromdepartment
where dept_count(dept_name) > 12

©Silberschatz, Korth and Sudarshan5.33Database System Concepts -7
th
Edition
Table Functions
The SQL standard supports functions that can return tables as results; such 
functions are called table functions
Example: Return all instructors in a given department
createfunctioninstructor_of(dept_namechar(20))
returnstable  (
ID varchar(5),
namevarchar(20),
dept_namevarchar(20),
salarynumeric(8,2))
returntable
(selectID, name, dept_name, salary
frominstructor
whereinstructor.dept_name=   instructor_of.dept_name)
Usage
select *
from table (instructor_of('Music'))

©Silberschatz, Korth and Sudarshan5.37Database System Concepts -7
th
Edition
Language Constructs (Cont.)
Forloop
•Permits iteration over all results of a query
Example:   Find the budget of all departments
declare n  integer default 0;
for r  as
select budget from department                                                     
where dept_name= 'Music' 
do
set n = n +   r.budget
end for

©Silberschatz, Korth and Sudarshan5.40Database System Concepts -7
th
Edition
External Language Routines
SQL allows us to define functions in a programming language such as 
Java, C#, C or C++. 
•Can be more efficient than functions defined in SQL, and 
computations that cannot be carried out in SQL\can be executed by 
these functions.
Declaring external language procedures and functions
create procedure dept_count_proc(indept_namevarchar(20),
out count integer)
language C
external name '/usr/avi/bin/dept_count_proc'
create function dept_count(dept_namevarchar(20))
returns integer
language C
external name '/usr/avi/bin/dept_count'

©Silberschatz, Korth and Sudarshan5.42Database System Concepts -7
th
Edition
Security with External Language Routines
To deal with security problems, we can do on of the following:
•Use sandboxtechniques
That is, use a safe language like Java, which cannot be used to  
access/damage other parts of the database code.
•Run external language functions/procedures in a separate process, 
with no access to the database process’memory.
Parameters and results communicated via inter-process 
communication
Both have performance overheads
Many database systems support both above approaches as well as direct 
executing in database system address space.

©Silberschatz, Korth and Sudarshan5.43Database System Concepts -7
th
Edition
Triggers

©Silberschatz, Korth and Sudarshan5.44Database System Concepts -7
th
Edition
Triggers
A triggeris a statement that is executed automatically by the system as a 
side effect of a modification to the database.
To design a trigger mechanism, we must:
•Specify the conditions under which the trigger is to be executed.
•Specify the actions to be taken when the trigger executes.
Triggers introduced to SQL standard in SQL:1999, but supported even 
earlier using non-standard syntax by most databases.
•Syntax illustrated here may not work exactly on your database 
system; check the system manuals

©Silberschatz, Korth and Sudarshan5.46Database System Concepts -7
th
Edition
Trigger to Maintain credits_earned value
create trigger credits_earnedafter update of takes on (grade)
referencing new row as nrow
referencing old row as orow
for each row
when nrow.grade<> 'F' and nrow.gradeis not null
and (orow.grade= 'F' or orow.gradeis null)
begin atomic
update student
set tot_cred= tot_cred+ 
(select credits
from course
where course.course_id= nrow.course_id)
where student.id = nrow.id;
end;

©Silberschatz, Korth and Sudarshan5.47Database System Concepts -7
th
Edition
Statement Level Triggers
Instead of executing a separate action for each affected row, a single 
action can be executed for all rows affected by a transaction
•Use     for each statement      instead of    for each row
•Use     referencing old tableor   referencing new tableto refer to 
temporary tables  (called transition tables) containing the affected 
rows
•Can be more efficient when dealing with SQL statements that update 
a large number of rows

©Silberschatz, Korth and Sudarshan5.48Database System Concepts -7
th
Edition
When Not To Use Triggers
Triggers were used earlier for tasks such as 
•Maintaining summary data (e.g., total salary of each department)
•Replicating databases by recording changes to special relations 
(called changeor deltarelations) and having a separate process that 
applies the changes over to a replica 
There are better ways of doing these now:
•Databases today provide built in materialized view facilities to 
maintain summary data
•Databases provide built-in support for replication
Encapsulation facilities can be used instead of triggers in many cases
•Define methods to update fields
•Carry out actions as part of the update methods instead of 
through a trigger 

©Silberschatz, Korth and Sudarshan5.49Database System Concepts -7
th
Edition
When Not To Use Triggers (Cont.)
Risk of unintended execution of triggers, for example, when
•Loading data from a backup copy
•Replicating updates at a remote site
•Trigger execution can be disabled before such actions.
Other risks with triggers:
•Error leading to failure of critical transactions that set off the trigger
•Cascading execution

©Silberschatz, Korth and Sudarshan5.50Database System Concepts -7
th
Edition
Recursive Queries

©Silberschatz, Korth and Sudarshan5.51Database System Concepts -7
th
Edition
Recursion in SQL
SQL:1999 permits recursive view definition
Example: find which courses are a prerequisite, whether directly or 
indirectly, for a specific course 
with recursive rec_prereq(course_id, prereq_id) as (
select course_id, prereq_id
from prereq
union
select rec_prereq.course_id, prereq.prereq_id, 
from rec_rereq, prereq
where rec_prereq.prereq_id= prereq.course_id
)
select ∗
from rec_prereq;
This example view, rec_prereq,is called the transitive closureof the prereq
relation

©Silberschatz, Korth and Sudarshan5.52Database System Concepts -7
th
Edition
The Power of Recursion
Recursive views make it possible to write queries, such as transitive 
closure queries, that cannot be written without recursion or iteration.
•Intuition:  Without recursion, a non-recursive non-iterative program 
can perform only a fixed number of joins of prereqwith itself
This can give only a fixed number of levels of managers
Given a fixed non-recursive query, we can construct a database 
with a greater number of levels of prerequisites on which the 
query will not work
Alternative: write a procedure to iterate as many times as required
•   See procedure findAllPrereqsin book

©Silberschatz, Korth and Sudarshan5.54Database System Concepts -7
th
Edition
Example of Fixed-Point Computation

©Silberschatz, Korth and Sudarshan5.55Database System Concepts -7
th
Edition
Advanced Aggregation Features

©Silberschatz, Korth and Sudarshan5.56Database System Concepts -7
th
Edition
Ranking
Ranking is done in conjunction with an order by specification. 
Suppose we are given a relation 
student_grades(ID, GPA) 
giving the grade-point average of each student
Find the rank of each student.
select ID, rank() over (order by GPAdesc) as s_rank
from student_grades
An extra order by clause is needed to get them in sorted order
select ID, rank() over (order by GPAdesc) as s_rank
from student_grades
order by s_rank
Ranking may leave gaps: e.g. if 2 students have the same top GPA, both 
have rank 1, and the next rank is 3
•dense_rankdoes not leave gaps, so next dense rank would be 2

©Silberschatz, Korth and Sudarshan5.57Database System Concepts -7
th
Edition
Ranking
Ranking can be done using basic SQL aggregation, but resultant query is 
very inefficient
select ID, (1 + (select count(*)
from student_gradesB
where B.GPA > A.GPA)) as s_rank
from student_gradesA
order by s_rank;

©Silberschatz, Korth and Sudarshan5.58Database System Concepts -7
th
Edition
Ranking (Cont.)
Ranking can be done within partition of the data.
“Find the rank of students within each department.”
select ID, dept_name,
rank () over (partition by dept_nameorder by GPA desc) 
as dept_rank
from dept_grades
order by dept_name, dept_rank;
Multiple rankclauses can occur in a single selectclause.
Ranking is done afterapplying group byclause/aggregation
Can be used to find top-n results
•More general than the limitnclause supported by many databases, 
since it allows top-n within each partition

©Silberschatz, Korth and Sudarshan5.59Database System Concepts -7
th
Edition
Ranking (Cont.)
Other ranking functions:  
•percent_rank(within partition, if partitioning is done)
•cume_dist(cumulative distribution)
fraction of tuples with preceding values
•row_number(non-deterministic in presence of duplicates)
SQL:1999 permits the user to specify nulls firstor nulls last
select ID, 
rank ( ) over (order by GPA descnulls last) as s_rank
from student_grades

©Silberschatz, Korth and Sudarshan5.60Database System Concepts -7
th
Edition
Ranking (Cont.)
For a given constant n, the ranking the function ntile(n) takes the tuples in 
each partition in the specified order, and divides them into nbuckets with 
equal numbers of tuples.
E.g.,
select ID, ntile(4) over (order by GPA desc) as quartile
from student_grades;

©Silberschatz, Korth and Sudarshan5.61Database System Concepts -7
th
Edition
Windowing
Used to smooth out random variations. 
E.g., moving average: “Given sales values for each date, calculate for 
each date the average of the sales on that day, the previous day, and the 
next day”
Window specificationin SQL:
•Given relation sales(date, value)
select date, sum(value) over 
(order by date between rows 1 preceding and 1following)
from sales

©Silberschatz, Korth and Sudarshan5.62Database System Concepts -7
th
Edition
Windowing
Examples of other window specifications:
•between rows unbounded preceding and current
•rows unbounded preceding
•range  between 10preceding and current row
All rows with values between current row value –10 to current value
•range interval 10day preceding
Not including current row

©Silberschatz, Korth and Sudarshan5.63Database System Concepts -7
th
Edition
Windowing (Cont.)
Can do windowing within partitions
E.g., Given a relation transaction (account_number, date_time, value), 
where value is positive for a deposit and negative for a withdrawal
•“Find total balance of each account after each transaction on the 
account”
select account_number, date_time,
sum (value) over
(partition by account_number
order by date_time
rows unbounded preceding)
as balance
from transaction
order by account_number, date_time

©Silberschatz, Korth and Sudarshan5.64Database System Concepts -7
th
Edition
OLAP

©Silberschatz, Korth and Sudarshan5.65Database System Concepts -7
th
Edition
Data Analysis and OLAP
Online Analytical Processing (OLAP)
•Interactive analysis of data, allowing data to be summarized and 
viewed in different ways in an online fashion (with negligible delay)
Data that can be modeled as dimension attributes and measure attributes 
are called multidimensional data.
•Measure attributes
measure some value
can be aggregated upon
e.g., the attribute number of the sales relation
•Dimension attributes
define the dimensions on which measure attributes (or aggregates 
thereof) are viewed
e.g., attributes item_name, color, andsize of the sales relation

©Silberschatz, Korth and Sudarshan5.66Database System Concepts -7
th
Edition
Example sales relation 
...
...
...
...
...
...
...
...

©Silberschatz, Korth and Sudarshan5.67Database System Concepts -7
th
Edition
Cross Tabulation of salesby item_name and color
The table above is an example of a cross-tabulation(cross-tab), also 
referred to as a pivot-table.
•Values for one of the dimension attributes form the row headers
•Values for another dimension attribute form the column headers
•Other dimension attributes are listed on top
•Values in individual cells are (aggregates of) the values of the 
dimension attributes that specify the cell.

©Silberschatz, Korth and Sudarshan5.68Database System Concepts -7
th
Edition
Data Cube
A data cubeis a multidimensional generalization of a cross-tab
Can have n dimensions; we show 3 below 
Cross-tabs can be used as views on a data cube

©Silberschatz, Korth and Sudarshan5.70Database System Concepts -7
th
Edition
Cross Tabulation With Hierarchy
Cross-tabs can be easily extended to deal with hierarchies
•Can drill down or roll up on a hierarchy

©Silberschatz, Korth and Sudarshan5.71Database System Concepts -7
th
Edition
Relational Representation of Cross-tabs
Cross-tabs can be represented 
as relations
•We use the value allis used 
to represent aggregates.
•The SQL standard actually 
uses null values in place of 
alldespite confusion with 
regular null values.

©Silberschatz, Korth and Sudarshan5.72Database System Concepts -7
th
Edition
Extended Aggregation to Support OLAP
The cubeoperation computes union of group by’s on every subset of the 
specified attributes
Example relation for this section
sales(item_name, color, clothes_size, quantity)
E.g., consider the query
select item_name, color, size, sum(number)
fromsales
group by cube(item_name, color, size)
This computes the union of eight different groupings of the sales relation:
{ (item_name, color, size), (item_name, color), 
(item_name, size),           (color, size), 
(item_name),                   (color), 
(size),                              ( ) }
where ( ) denotes an empty group by list.
For each grouping, the result contains the null value 
for attributes not present in the grouping. 

©Silberschatz, Korth and Sudarshan5.73Database System Concepts -7
th
Edition
Online Analytical Processing Operations
Relational representation of cross-tab that we saw earlier, but with null in 
place of all, can be computed by
select item_name, color, sum(number)
from sales
group by cube(item_name, color)
The function grouping()can be applied on an attribute
•Returns 1 if the value is a null value representing all, and returns 0 in all 
other cases. 
select item_name, color, size, sum(number),
grouping(item_name) as item_name_flag,
grouping(color) as color_flag,
grouping(size) as size_flag,
from sales
group by cube(item_name, color, size)

©Silberschatz, Korth and Sudarshan5.74Database System Concepts -7
th
Edition
Online Analytical Processing Operations
Can use the function decode()in the selectclause to replace 
such nulls by a value such as all
•E.g., replace item_namein first query by 
decode( grouping(item_name), 1, ‘all’, item_name)

©Silberschatz, Korth and Sudarshan5.75Database System Concepts -7
th
Edition
Extended Aggregation (Cont.)
The rollupconstruct generates union on every prefix of specified list of 
attributes 
E.g., 
select item_name, color, size, sum(number)
from sales
group by rollup(item_name, color, size)
•Generates union of four groupings:
{ (item_name, color, size), (item_name, color), (item_name), ( ) }
Rollup can be used to generate aggregates at multiple levels of a
hierarchy.
E.g., suppose table itemcategory(item_name, category) gives the category of 
each item. Then  
select category, item_name, sum(number)
from sales, itemcategory
where sales.item_name=   itemcategory.item_name
group by rollup(category, item_name)
would give a hierarchical summary by item_nameand by category.

©Silberschatz, Korth and Sudarshan5.76Database System Concepts -7
th
Edition
Extended Aggregation (Cont.)
Multiple rollups and cubes can be used in a single group by clause
•Each generates set of group by lists, cross product of sets gives overall 
set of group by lists
E.g., 
select item_name, color, size, sum(number)
from sales
group by rollup(item_name), rollup(color, size)
generates the groupings 
{item_name, ()} X {(color, size), (color), ()} 
= { (item_name, color, size), (item_name, color), (item_name), 
(color, size), (color), ( ) }

©Silberschatz, Korth and Sudarshan5.77Database System Concepts -7
th
Edition
Online Analytical Processing Operations
Pivoting:changing the dimensions used in a cross-tab is called 
Slicing: creating a cross-tab for fixed values only
•Sometimes called dicing, particularly when values for multiple 
dimensions are fixed.
Rollup: moving from finer-granularity data to a coarser granularity 
Drill down: The opposite operation -  that of moving from coarser-
granularity data to finer-granularity data

©Silberschatz, Korth and Sudarshan5.78Database System Concepts -7
th
Edition
OLAP Implementation
The earliest OLAP systems used multidimensional arrays in memory to 
store data cubes, and are referred to as multidimensional OLAP (MOLAP)
systems.
OLAP implementations using only relational database features are called 
relational OLAP (ROLAP)systems
Hybrid systems, which store some summaries in memory and store the 
base data and other summaries in a relational database, are called hybrid 
OLAP (HOLAP)systems.

©Silberschatz, Korth and Sudarshan5.79Database System Concepts -7
th
Edition
OLAP Implementation (Cont.)
Early OLAP systems precomputed allpossible aggregates in order to 
provide online response
•Space and time requirements for doing so can be very high
2
n
combinations of group by
•It suffices to precompute some aggregates, and compute others on 
demand from one of the precomputed aggregates
Can compute aggregate on (item_name, color) from an aggregate 
on (item_name, color, size) 
•   For all but a few “non-decomposable” aggregates such as 
median
•   is cheaper than computing it from scratch 
Several optimizations available for computing multiple aggregates
•Can compute aggregate on (item_name, color) from an aggregate on 
(item_name, color, size)
•Can compute aggregates on (item_name, color, size), 
(item_name, color) and (item_name) using a single sorting 
of the base data

©Silberschatz, Korth and Sudarshan5.80Database System Concepts -7
th
Edition
End of Chapter 5
