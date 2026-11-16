# ch2 - Intro to Relational Model-图集-矢量

> 主题库：数据库系统原理　｜　源文件：ch2 - Intro to Relational Model-图集-矢量.pdf　｜　4,152 字符　｜　11 个片段　｜　更新于 2026-11-13
ID name dept_name salary 10101 Srinivasan Comp.Sci. 65000 t 12121 Wu Finance 90000 (o 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

ID name dept_name salary 22222 Einstein Physics 95000 12121 Wu Finance 90000 32343 ElSaid History 60000 45565 Katz Comp.Sci. 75000 98345 Kim Elec.Eng. 80000 76766 Crick Biology 72000 10101 Srinivasan Comp. Sci. 65000 58583 Califieri History 62000 83821 Brandt Comp. Sci. 92000 15151 Mozart Music 40000 33456 Gold Physics 87000 76543 Singh Finance 80000

ID name dept_name salary 22222 Einstein Physics 95000 12121 Wu Finance 90000 32343 E1Said History 60000 45565 Katz Comp.Sci. 75000 98345 Kim Elec.Eng. 80000 76766 Crick Biology 72000 10101 Srinivasan Comp. Sci. 65000 58583 Califieri History 62000 83821 Brandt Comp.Sci. 92000 15151 Mozart Music 40000 33456 Gold Physics 87000 76543 Singh Finance 80000

takes student D D YEAR grade section course course id course id department advisor sec id title dept name s id semester dept_name i_id year building building time_slot credits budget time_slot_id dav prerel instructor classroom courseid D building prereaid , name capacit teaches salary D COurSe iD sec id Semester year

electσ  set difference:-  Cartesian product: x rename_

ID name dept_name salary 22222 Einstein Physics 95000 33456 Gold Physics 87000

ID name salary 10101 Srinivasan 65000 12121 Wu 90000 15151 Mozart 40000 22222 Einstein 95000 32343 E1Said 60000 33456 Gold 87000 45565 Katz 75000 58583 Califieri 62000 76543 Singh 80000 76766 Crick 72000 83821 Brandt 92000 98345 Kim 80000

instructor.ID name dept_name salary teaches.ID course_id sec_id semester year 10101 Srinivasan Comp.Sci. 65000 10101 CS-101 1 Fall 2017 10101 Srinivasan Comp.Sci. 65000 10101 CS-315 1 Spring 2018 10101 Srinivasan Comp.Sci. 65000 10101 CS-347 1 Fall 2017 10101 Srinivasan Comp.Sci. 65000 12121 FIN-201 1 Spring 2018 10101 Srinivasan Comp.Sci. 65000 15151 MU-199 1 Spring 2018 10101 Srinivasan Comp.Sci. 65000 22222 PHY-101 1 Fall 2017 12121 Wu Finance 90000 10101 CS-101 1 Fall 2017 12121 wu Finance 90000 10101 CS-315 1 Spring 2018 12121 Wu Finance 90000 10101 CS-347 1 Fall 2017 12121 wu Finance 90000 12121 FIN-201 1 Spring 2018 12121 Wu Finance 90000 15151 MU-199 1 Spring 2018 12121 Wu Finance 90000 22222 PHY-101 1 Fall 2017 … … … … … …  … … 15151 Mozart Music 40000 10101 CS-101 1 Fall 2017 15151 Mozart Music 40000 10101 CS-315 1 Spring 2018 15151 Mozart Music 40000 10101 CS-347 1 Fall 2017 15151 Mozart Music 40000 12121 FIN-201 1 Spring 2018 15151 Mozart Music 40000 15151 M U-199 1 Spring 2018 15151 Mozart Music 40000 22222 PHY-101 1 Fall 2017 22222 Einstein Physics 95000 10101 CS-101 1 Fall 2017 22222 Einstein Physics 95000 10101 CS-315 1 Spring 2018 22222 Einstein Physics 95000 10101 CS-347 1 Fall 2017 22222 Einstein Physics 95000 12121 FIN-201 1 Spring 2018 22222 Einstein Physics 95000 15151 M U-199 1 Spring 2018 22222 Einstein Physics 95000 22222 PHY-101 1 Fall 2017

instructor.ID name dept_name salary teaches.Id course_id sec_id semester year 10101 Srinivasan Comp.Sci. 65000 10101 CS-101 1 Fall 2017 10101 Srinivasan Comp.Sci. 65000 10101 CS-315 1 Spring 2018 10101 Srinivasan Comp.Sci. 65000 10101 CS-347 1 Fall 2017 12121 Wu Finance 90000 12121 FIN-201 1 Spring 2018 15151 Mozart Music 40000 15151 MU-199 1 Spring 2018 22222 Einstein Physics 95000 22222 PHY-101 1 Fall 2017 32343 E1 Said History 60000 32343 HIS-351 1 Spring 2018 45565 Katz Comp.Sci. 75000 45565 CS-101 1 Spring 2018 45565 Katz Comp.Sci. 75000 45565 CS-319 1 Spring 2018 76766 Crick Biology 72000 76766 BIO-101 1 Summer 2017 76766 Crick Biology 72000 76766 BIO-301 1 Summer 2018 83821 Brandt Comp.Sci. 92000 83821 CS-190 1 Spring 2017 83821 Brandt Comp.Sci. 92000 83821 CS-190 2 Spring 2017 83821 Brandt Comp.Sci. 92000 83821 CS-319 2 Spring 2018 98345 Kim Elec.Eng. 80000 98345 EE-181 1 Spring 2017

course_id CS-101 CS-315 CS-319 CS-347 FIN-201 HIS-351 MU-199 PHY-101

course_id CS-347 PHY-101
