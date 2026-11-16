# ch4 - homework

> 主题库：数据库系统原理　｜　源文件：ch4 - homework.pdf　｜　1,323 字符　｜　4 个片段　｜　更新于 2026-11-11


ch4
1. 外连接表达式可以在SQL中不使用SQL outer join操作来计算。为了说明这一点，展示如何
重写以下SQL查询而不使用外连接表达式：
 
select * from student natural left outer join takes
 
select * from student natural full outer join takes
2. SQL允许外键依赖引用相同的关系，如以下示例中所示：
create table manager
(employee ID char(20),
manager ID char(20),
primary key employee ID,
foreign key (manager ID) references manager(employee ID)
on delete cascade );
1
2
3
4
5
6
在这里，"employee ID" 是"manager"表的主键，这意味着每个员工最多只有一个经理。外
键子句要求每位经理也必须是一名员工。请解释当"manager"关系中的元组被删除时会发生
什么事情。  
3. 定义一个名为"tot_credits"的视图（year, num_credits），该视图显示每年所修的总学分
数。
4. 请表达以下查询的SQL，不使用子查询和集合操作（参考fig 1）。
select ID
from student
except
select s_id
from advisor
where i_ID is not null;
1
2
3
4
5
6
5. 重新编写查询，使用内连接和使用"using"条件来代替"natural join" （参考fig1）：
select *
from section natural join classroom;
1
2

ID
course_id
sec_id
semester
year
grade
ID
name
dept_name
tot_cred
building
room_number
capacity
s_id
i_id
ID
course_id
sec_id
semester
year
takes
section
classroom
teaches
prereq
course_id
prereq_id
course_id
title
dept_name
credits
course
student
dept_name
building
budget
department
instructor
ID
name
dept_name
salary
advisor
time_slot
time_slot_id
day
start_time
end_time
course_id
sec_id
semester
year
building
room_number
time_slot_id
Figure 1 Schema diagram for the university database.
6.  Arrselectpublic
BrselectA
