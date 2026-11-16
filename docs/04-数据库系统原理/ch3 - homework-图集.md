# ch3 - homework-图集

> 主题库：数据库系统原理　｜　源文件：ch3 - homework-图集.pdf　｜　886 字符　｜　3 个片段　｜　更新于 2026-11-11
with dept_total(dept_name,value as
(selectdept_name,sum(salary)
groupby dept_name),
dept_total_avg(value) as
faram dept_total dept_total_avg
where dept_total.value >= dept_total_avg.value;

classroom(building, room_number, capacity)
department(dept_name, building, budget)
caurse course_i ,title dept_name credits
instructor(ID, name,dept_name,salary)
section(course_id, sec_id, semester, year, building, room_number, time_slot_id)
teaches(ID, course_id,sec_id, semester, year)
student(ID, name, dept_name,tot_cred)
takes(ID, course_id,sec_id, semester, year, grade)
a _D,_
time_slot(time_slot_id,day,start_time,end_time
prereq(course_id, prereq_id)

branch(branch_name,branch_city,assets customer (ID , customer_name, custo mer_street, customer_city) loan(loan_number,branch_name,amount borrower(ID,loan_number) account(account_number,branch_name,balance) depos itor(ID,account_number)
