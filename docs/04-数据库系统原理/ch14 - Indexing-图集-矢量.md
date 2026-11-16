# ch14 - Indexing-图集-矢量

> 主题库：数据库系统原理　｜　源文件：ch14 - Indexing-图集-矢量.pdf　｜　6,035 字符　｜　35 个片段　｜　更新于 2026-11-13
10101 10101 Srinivasan Comp.Sci. 65000 12121 → 12121 Wu Finance 90000 15151 15151 Mozart Music 40000 22222 22222 Einstein Physics 95000 32343 32343 El Said History 60000 33456 33456 Gold Physics 87000 45565 → 45565 Katz Comp.Sci. 75000 58583 58583 Califieri History 62000 - 76543 → 76543 Singh Finance 80000 76766 76766 Crick Biology 72000 83821 83821 Brandt Comp.Sci. 92000 98345 98345 Kim Elec.Eng. 80000

Biology 76766 Crick Biology 72000 Comp. Sci. 10101 Srinivasan Comp.Sci. 65000 Elec.Eng. 45565 Katz Comp.Sci. 75000 Finance 83821 Brandt Comp.Sci. 92000 History 98345 Kim Elec.Eng. 80000 Music 12121 Wu Finance 90000 Physics 76543 Singh Finance 80000 32343 E1Said History 60000 58583 Califieri History 62000 15151 Mozart Music 40000 22222 Einstein Physics 95000 33465 Gold Physics 87000

10101 10101 Srinivasan Comp.Sci. 65000 32343 12121 Wu Finance 90000 76766 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 oncepts - 7th Edition 14.8 © Silberschatz, Korth and S

data block

40000 10101 Srinivasan Comp.Sci. 65000 / 60000 12121 Wu Finance 90000 62000 15151 Mozart Music 40000 65000 1 22222 Einstein Physics 95000 72000 32343 El Said History 60000 75000 33456 Gold Physics 87000 80000 45565 Katz Comp.Sci. 75000 87000 / 90000 58583 Califieri History 62000 92000 76543 Singh Finance 80000 95000 76766 Crick Biology 72000  83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

Mozart Rootnode Brandt|. Califieri Crick → E Einstein El Said   . Gold Katz Kim Mozart Singh  Srinivasan Wu 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 80000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 60000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Co p.Sci. 92000 98345 Kim Elec.Eng. 80000

If Lj, L; are leaf nodes and i < j, L;'' s search-key values are less taanoreaualtoLeeearch-keyvalues pointstonext leafnodein search-keyorder Brandt | Califieri | Crick → Pointer to next leaf node 10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 MoZart Music 40000 22222 Einstein Physics 95000 32343 El Said History 80000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 60000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 n Concepts - 7th Edition 14.21 ©Silberschatz, Korth and Sudarsl

Brandt  Califieri Crick Einstein El Said Gold Katz Kim Mozart Singh S Srinivasan Wu

Mozart Adams Brandt Califieri Crick Einstein E1 Said Gold Katz Kim Mozart Singh  Srinivasan Wu

Mozart Adams Brandt Califieri Crick Einstein E1 Said Gold Katz Kim Mozart Singh  Srinivasan Wu

Mozart Rootnode Einstein Gold Srinivasan : - Internal nodes Brandt Califieri Crick Einstein E1 Said → Gold . Katz Kim → Mozart Singh  Srinivasan . Wu  [Califeri Einstein Gold Srinivasan Adams B Brandt Califieri Crick Einstein E1 Said Gold Katz Kim Mozart Singh Srinivasan Wu

Gold ozar alife Ensten K m Srinivasan Adams Brandt Calieri Crick Einstein E1 Said Gold Katz Kim Lamport Mozart Singh Srinivasan Wu

Mozart Califieri Einstein. Gold. Srinivasan Adams Brandt Califieri Crick  Einstein E1 Said Gold Katz Kim Mozart Singh Srinivasan Wu

Adams Brandt Califieri Crick Einstein E1 Said Gold Katz Kim Mozart Singh  Srinivasan Wu   Before andafterdeleting "Srinivasan l

Gol Affected nodes Adams Brandt  Califieri i Crick Einstein E1 Said Gold Katz Kim Mozart Singh Wu

A dams Brandt Califieri Crick Einstein El Said Gold Katz  Kim Mozart

Adams Brandt  Califieri Crick  Einstein El Said Gold Katz Kim Mozart Singh Wu

Gol Adams Brandt Califieri Crick Einstein El Said Gold Katz  Kim Mozart

Calif i Einstein Gold Adams Brandt Califieri Crick Einstein El Said Katz Kim Mozart

(A,4) (B,) + (C,1) (D,9) (E,4)+ (F,7) (G,3) (H,3) (I,4) J,8) +(K,1) (L,6) (M ,4) (N,8) (P,6)

Mozart Rootnode Brandt . Califieri Crick . Einstein|. El Said  Gold . Katz . Kim . Mozart|. Singh  Srinivasan Wu Database System Concepts- 7th Edition 14.48 ©Silberschatz, Korth and Sudarshan

Einstein Katz Singh record record record Brandt| Califieri Crick El Said Gold  Kim Mozart Srinivasan Wu Brandt Califieri reeord reeord andsoonforotherrecords

bucketl overflow buckets for bucket 1 buckete bucket3

[12121 Wu F nance 90000 76543 Singh F nance 80000 15151 Mozart Music 40000 76766 Crick Biology 72000 32343 El Said History 80000 10101 Srinivasan Comp. Sci. 65000 58583 Califieri History 60000 45565 Katz Comp. . Sci. 75000 83821 Brandt Comp.Sci. 92000

bucket3 22222 Einstein Physics 95000 33456 Gold Physics 87000 98345 Kim Elec.F .Eng. 80000

bucket 7

record number ID gender income_level 0 76766 m L1 1 22222 f L2 2 12121 f L1 3 15151 m L4 4 58583 f L3

ps for gender Bitm aps for 10010 L1 10100 01101 L2 01000 L3 00001 L4 00010 L5 00000

A B C I G 1 I 313 I 1 H - 1 D I [2! E F

BB1 BB2 BB3 A B C D E F G H

B C G 3 2! E cent e t Edition

BB1 BB2 BB3 A B C D E F G H I

10101 Srinivasan Comp. Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 60000  33456 Gold Physics 87000  45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000  76766 Crick Biology 72000 83821 Brandt Comp. Sci. 92000 98345 Kim Elec.Eng· 80000

History Biology Chemistry Elec. Eng. Finance History Music Physics

bucket 0 76766 bucket 1 45565 76543 bucket 2 22222 76766 Crick Biology 72000 10101 Srinivasan Comp.Sci. 65000 bucket 3 45565 Katz Comp.Sci. 75000 10101 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 12121 Wu Finance 90000 bucket 4 76543 Singh Finance 80000 32343 El Said History 60000 58583 Califieri History 62000 → 15151 MoZart Music 40000 bucket 5   22222 Einstein Physics 95000 15151 58583 → 33465 Gold Physics 87000 33456 98345 bucket 6 83821 hash index on instructor, on bucket 7 12121 32343
