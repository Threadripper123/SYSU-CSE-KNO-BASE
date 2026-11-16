# ch4 - Intermediate SQL-图集

> 主题库：数据库系统原理　｜　源文件：ch4 - Intermediate SQL-图集.pdf　｜　3,483 字符　｜　41 个片段　｜　更新于 2026-11-11
Chapter 4:

IntermedlateSQl

Chapter 4:    Intermmediate  sQL

Joined   Relations

NaturalJoln inSQl(Cont

Student   Relaton

ID name dept_name tot_cred 00128 Zhang Comp.Sci. 102 12345 Shankar Comp.Sci. 32 19991 Brandt History 80 23121 Chavez Finance 110 44553 Peltier Physics 56 45678 Levy Physics 46 54321 Williams Comp.Sci. 54 55739 Sanchez Music 38 70557 Snow Physics 0 76543 Brown Comp.Sci. 58 76653 Aoi Elec.Eng. 60 98765 Bourikas Elec.Eng. 98 98988 Tanaka Biology 120

Takes Relation

ID course_id sec_id semester year grade 00128 CS-101 1 Fal1 2017 A 00128 CS-347 I Fal1 2017 A- 12345 CS-101 I Fal1 2017 C 12345 CS-190 2 Spring 2017 A 12345 CS-315 1 Spring 2018 A 12345 CS-347 I Fal1 2017 A 19991 HIS-351 1 Spring 2018 B 23121 FIN-201 1 Spring 2018 C+ 44553 PHY-101 I Fal1 2017 B- 45678 CS-101 I Fal1 2017 F 45678 CS-101 I Spring 2018 B+ 45678 CS-319 I Spring 2018 B 54321 CS-101 1 Fal1 2017 A- 54321 CS-190 2 Spring 2017 B+ 55739 MU-199 1 Spring 2018 A- 76543 CS-101 I Fal1 2017 A 76543 CS-319 2 Spring 2018 A 76653 EE-181 1 Spring 2017 C 98765 CS-101 I Fal1 2017 C- 98765 CS-315 H Spring 2018 B 98988 BIO-101 I Summer 2017 A 98988 BIO-301 1 Summer 2018 null

ID name dept_name tot_cred course_id sec_id semester year grade 00128 Zhang Comp.Sci. 102 CS-101 1 Fal1 2017 A 00128 Zhang Comp.Sci. 102 CS-347 1 Fal1 2017 A- 12345 Shankar Comp.Sci. 32 CS-101  Fal1 2017 C 12345 Shankar Comp.Sci. 32 CS-190 2 Spring 2017 A 12345 Shankar Comp.Sci. 32 CS-315  Spring 2018 A 12345 Shankar Comp.Sci. 32 CS-347  Fal1 2017 A 19991 Brandt History 80 HIS-351  Spring 2018 B 23121 Chavez Finance 110 FIN-201  Spring 2018 C+ 44553 Peltier Physics 56 PHY-101  Fal1 2017 B- 45678 Levy Physics 46 CS-101 I Fal1 2017 F 45678 Levy Physics 46 CS-101 I Spring 2018 B+ 45678 Levy Physics 46 CS-319 I Spring 2018 B 54321 Wi1liams Comp.Sci. 54 CS-101 T Fal1 2017 A- 54321 Wil1iams Comp.Sci. 54 CS-190 2 Spring 2017 B+ 55739 Sanchez Music 38 MU-199  Spring 2018 A- 76543 BroWn Comp.Sci. 58 CS-101  Fal1 2017 A 76543 Brown Comp.Sci. 58 CS-319 2 Spring 2018 A 76653 Aoi Elec.Eng. 60 EE-181 I Spring 2017 C 98765 Bourikas Elec.Eng. 98 CS-101  Fal1 2017 C- 98765 Bourikas E1ec.Eng. 98 CS-315  Spring 2018 B 98988 Tanaka Biology 120 BIO-101  Summer 2017 A 98988 Tanaka Biology 120 BIO-301 1 Summer 2018 null

natural join

Dangerous in  Naturai Join

Outer Join

course id  Iprereg_id BIO-301 BIO-101 CS-190 CS-101 CS-347 CS-101

Outer  Joln

Example     les

course id title dept_name credits BIO-301 Genetics Biology 4 CS-190 Game Design Comp. Sci. 4 CS-315 Robotics Comp.Sci. 3

course_id title dept_name credits   prereq _id BIO-301 Genetics Biology  BIO-101 CS-190 GameDesign Comp.Sci. CS-101 CS-315 Robotics Comp.Sci. 3 null CS-347 null null null CS-101

Join types Join conditions lefouterjoin on predicate rightouter join using (A1, A2, ..., An) fullouterjoin

course_id title dept_name  credits  prereq_id BIO-301 Genetics Biology  BIO-101 CS-190 Game Design 1  ( Comp. Sci. CS-101 CS-315 Robotics Comp.Sci. 3 null CS-347 ull null null CS-101

Jolned Relations

Examples

View Definition  -

Views Defined Using   Other Views -

View Expansion

View Expanslon (Cont.)

Some Updates 8 Cannotl tbe Translated Uniquely

AndS SomeNotatA

ViewUpdatesin SQL

Integrity Constralnts

NotNulの Constraints

Unigue Const traints

ReferentialIntegrity (Cont)

Cascading Actions in F Referential Integrity

Integrlty Constraint Volation During Transactions

Assertions

Bullt

arge

User

Defined Types

Domains
