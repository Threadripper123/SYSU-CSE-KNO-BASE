# ch13 - Data Storage Structures-图集

> 主题库：数据库系统原理　｜　源文件：ch13 - Data Storage Structures-图集.pdf　｜　4,452 字符　｜　41 个片段　｜　更新于 2026-11-11
Chapter  13: Data  Storage Structures

Yalea rgan aton

ixed F

record 0 10101 Srinivasan Comp.Sci. 65000 record 1 12121 Wu Finance 90000 record2 15151 Mozart Music 40000 record 3 22222 Einstein Physics 95000 record 4 32343 E1Said History 60000 record5 33456 Gold Physics 87000 record 6 45565 Katz Comp.Sci. 75000 record 7 58583 Califieri History 62000 record 8 76543 Singh Finance 80000 record 19 76766 Crick Biology 72000 record 10 83821 Brandt Comp.Sci. 92000 recOrd 11 98345 Kim Elec.Eng. 80000

record 0 10101 Srinivasan Comp.Sci. 65000 record 1  12121 Wu Finance 90000 record2 15151 Mozart Music 40000 record 14 32343 ElSaid History 60000 record5 33456 Gold Physics 87000 record 6 45565 Katz Comp.Sci. 75000 record 7 58583 Califieri History 62000 record8 76543 Singh Finance 80000 record 19 76766 Crick Biology 72000 record10 83821 Brandt Comp.Sci. 92000 record 11 98345 Kim Elec.Eng. 80000

record 0 10101 Srinivasan Comp.Sci. 65000 record 1 12121 Wu Finance 90000 record 2 15151 Mozart Music 40000 recOrd 11 98345 Kim Elec.Eng. 80000 record 4 32343 E1 Said History 60000 record5 33456 Gold Physics 87000 record6 45565 Katz Comp.Sci. 75000  record 7 58583 Califieri History 62000 record 8 76543 Singh Finance 80000 record9 76766 Crick Biology 72000 record10 83821 Brandt Comp.Sci. 92000

header record 0 10101 Srinivasan Comp.Sci. 65000 record1  record 2 15151 Mozart Music 40000 record3 22222 Einstein Physics 95000 record 4 record5 33456 Gold Physics 87000 record6 record 7 58583 Califieri History 62000 record8 76543 Singh Finance 80000 record9 76766 Crick Biology 72000 record10 83821 Brandt Comp.Sci. 92000 record 11 98345 Kim Elec.Eng. 80000

Nullbitmap (storedinIbyte) 21,5 26,10 36,10 65000 10101 Srinivasan Comp.Sci. Bytes0 021 26 36 45

Varlable

Length Records: Slotted Page Structure

Storing Large Objects

4 2 1 4- 7 3 6 5 12 0 11056

4 7 2 6

Sequential File Organization (Cont-)

Multitable Clustering Flle Organization

dept_name building budget Comp. Sci. Taylor 100000 Physics Watson 70000

ID name dept_name salary 10101 Srinivasan Comp. Sci. 65000 33456 Gold Physics 87000 45565 Katz Comp. Sci.  75000 83821 Brandt Comp. Sci. 92000

Comp.Sci. Taylor 100OO0 10101 Srinivasan Comp. . Sci. 65000  45565 Katz Comp. Sci. 75000 83821 Brandt Comp.Sci. 92000 Physics Watson 70000 33456 Gold Physics 87000

MultitableC Clustering  4 Flle Organization (cont)

Partitioning

Data DictionaryS Storage

Storage Access 一

Buffer (in-memory) BufferManager - Hash Table Disk

Buffer Manager

Bufer

Optimization of Disk Block Access 3  3  (Cont-)

Column

Oriented -  Storage

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 Figure 13.14 Columnar representation of the instructor relation.

Columnar Representation

Columnar File Representation

StorageOrganization n

Maln

Memory  Databases

Col1 Data Col2 Data Col3 Data Col4 Data Col5 Data Col1 Data Col2 Data Col3 Data Col4 Data Col5 Data Col1 Data Col2 Data Col3 Data Col4 Data Col5 Data

End of Chapter

13

Block Header Records Size # Entries Free Space  number of record entries Records can be moved around within a page to keep them contiguous with no empty space between them; entry in the header must be updated.

10101 Srinivasan Comp. Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.l Eng· 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 32222 Verdi Music 48000

Relation_ metadata Atribute_ metadata 1 Yelation name relation name [maamber_of_atributes atribute _name atorage_organization omain_type location position LENOTH Index_metadata index name relation name INDEX_YPE User_metadata index_  _attributes username encrypted_password View_metadata group view name
