# book-图集

> 主题库：数据库系统原理　｜　源文件：book-图集.pdf　｜　17,549 字符　｜　55 个片段　｜　更新于 2026-11-11
Searchfor: Student Name:Br Show details Brandt Show Brown Search: ID  ▲ Name  Dept.Name  76543 Brown Comp.Sci. 77777 Brown Biology 88888 Brown Finance Showing 1to 3 of3 entries Previous 1 Next

5 mal

SYSTEM CONCEPTS SEVENTH EDITION Abraham Silberschatz Yale University HenryF.Korth Lebigb University Indian Institute of Technology, Bombay MC [Ya

naive users sophisticated database (tellers,agents, application users webusers programmers administrators (analysts use write use use application application qQuery administration campilerand linker DMLqueries DDL nterpreter aaelication program DML compiler query evaluation engine query processor buffer manager file manager authorization transaction and integrity manager cYmanann indices data dictionary Ya statistical data

takes student D ouseid name ee dept_name tot_cred year grade section course  course id  course id department advisor sec id  title semester  dept name s id dept_name i_d year building building time_slot credits budget room _nu mber time slot id Yame_slot_d end_tame class om orse d D building prereg_id name room_number dept_name capacity teaches salary sec id cYamete year

instructor student [name name salary tot_cred gure 6.1 E-R diagram showing entity sets instructor and studer t. The second part contains the names of all the attributes diagram in Figure 6.1 shows two entity sets instructor and sociated with instructor are ID, name, and salary. The attributt are ID, name, and tot_cred. Attributes that are part of the pri see Section 6.5). ip is an association among several entities. For example, we advisor that associates instructor Katz with student Shankar.'' s that Katz is an advisor to student Shankar. A relationship s s of the same type. r two entity sets instructor and student. We define the relatio )te the associations between students and the instructors whc gure 6.2 depicts this association. To keep the figure simple,  onship instance in an E-R schema represents an association ies in the real-world enterprise that is being modeled. As an al instructor entity Katz, who has instructor ID 45565, and th ,who has student ID 12345, participate in a relationship inst 76766 Crick 98988 Tanaka 45565 Katz 12345 Shankar 10101 Srinivasan 00128 Zhang 98345|Kim 76543 BroWn 76543 Singh 76653 Aoi

b1 b1 a1 b2 a2 b2 a2 b3 a3 b₃ a3 b b5 (a) (b) Figure 6.9 Mapping cardinalities.(a) One-to-one.(b) One-to-many. ny-to-many. An entity in A is associated with any number (zero or n ities in B, and an entity in B is associated with any number (zero or n ities in A. (See Figure 6.10b.) ropriate mapping cardinality for a particular relationship set obviously  ceal-world situation that the relationship set is m odeling. an illustration, consider the advisor relationship set. If a student can be al instructors (as in the case of students advised jointly), the relationsh )-m any. In contrast, if a particular university imposes a constraint that a advised by only one instructor, and an instructor can advise several si e relationship set from instructor to student must be one-to-m any. Thus, n A B A B a1 a1 b q2 b₁ a2 b2 a3 b2 a3 b3 a4 b3 a4 b₄ a

instructor student ID 不 advisor ID name name salary tot_cred (a) One-to-one instructor student ID 不 advisor ID name name salary tot_cred aba One-to-many instructor student ID advisor ID name name salary tot_cred (c) Many-to-one instructor student advisor ID ID name name salary tot_cred

department course_dept dept_name building inst_dept stud_dept instructor student ID advisor ID name name salary tot_cred teaches takes grade section cotrse  sec_id. time_slot course id sec_course sec_time_slot title  semester time slot id year day credits start_time end_time prereq se_id prereq_id sec_class classroom cYana l [Yaammnumbe capacity

project instructor student proj_ guide eval_ for evaluation

project instructor student proj_guide eval_for evaluation

marKS student marks_in assignment sec_ assign section (c) Correct alternative to erroneous E-R diagram (b) markS student stud_ section section

A R A B R B Rn= E Rc

E A2 A2.1 R relationship set A2.2 [A3] A4O cYGYan l R relationship set E for weak entity set A1 totalparticipation E R E of entity set in A1 relationship  R many-te-many R relationship R one-to-one R 1..h E E1 role- R name E role indicator E2 E3 E1 E1 cYaa E2 E3 E2 E3

A2.1 A2. uteA3, Al A3 A4, A1 E A4 E1 * R * E2 E1 R E2 E1 1 R 1 E2 E1 R E2 E1 * R 1 E2 E1 R E2 R E1 R E2 E1 E2 ISA total generalization ISA generalization

Ec entnty wit l E class wit A1 attributes(simple, and meth —A1 M 10 composite, prefixes: + multivalued, derived) +M 10 – = private E1 |role1 R role2 E2 binary E1 role1 R role2 E2 cYaGaGnG Y l A1 R A1 E1 rolel R role2 relationship E2 E1 role1 role2 E2 attributes 0.* 0..1 E1 R E2 cardinality E1 0_1 R 0_* E2 constraints E2 E2 n-aayl E1 R relationships E1 R E3 E3 E1 E1   overlapping   overlappir generalization E2 E3 E2 E3 E1 E1 4 disjoint ↓ disjoint generalization E2 E3 E2 E3 E1  R E2 Weak-entity E1 E2 composition

RA A B R RB C B E Rc C (a) (b) RA7 LRAc A B Re- C

author publisher name name address address URL customer written_by email published_by name address phone book number ISBN . cYatle stopeng_basket basket_ f Year contains basket_ d price number warehouse code stocks address phonel

ID name street cit salary 57766 Kim Main Perryridge 75000 98776 Kim North Hampton 67000 ID name name street cit sary 57766 Kim Kim Main Perryridge 75000 98776 Kim Kim North Hampton 67000 [Taa ID name street cit salary 57766 Kim Main Perryridge 75000 57766 Kim North Hampton 67000 98776 Kim Main Perryridge 75000 98776 Kim North Hampton 67000

Srinivasan 6500 Comp.Sci. Zhang 102 [na me salary dept_name name tot_cred cYna _ 10101 comp_sci 00128 aches ceere_dep takes sec_course sec_id title classroom semester year Intro. to Computer Science packard-101 Fall 2017

web server application server database server browser data

network web server and cYnanaann l HTTP

internet controller 2 vniew odel 3 data-access layer 4 database

NameNode Metadata (name, replicas, ..) Metadata (name, replicas,… Block Read DataNodes Replication

rk1 rV1 rk1 V1,rV7, rk7 rV2 rk2 rVg,rVi. mk1 mV1 rk3 rV3 rk3 rV3. mk2 mV2 rk1 V7 [YR rk2 rVg rk2 Vi rkj rVn

User Program copy copy cOpy Master map reduce Part1 大 Map1 Reduce 1 File1 Part 2 Part3 Map 2 ,2  Reduce1 write 1 File 2 [Part4 local write Part n read Map n Reduce m File m Remote

Data Source Op Op Op Data Data Sink Source Data Data Sink Source Data Op Data Sink Source Publish-Subscribe Data Data System Sink Source Data Op Op Data   Sink Source   Data Data Source Data Sink Op Op

ata source 1 cYa loaders ata source2 query and DBMS analysis tools

item _info store item_ id store_id itemname city cnolor sales state size country category item_id customer_id date customer number customer_id date_info price na me date street month ci quarter state year zipcode coun ry

2 5 3 1 11 4 7 6 12 29 2 8 5 7 22 1 8 20 14 20 62 34 4 1 35 10 7 2 54 21 9 145 10 5 28 5 48 42 med 53 35 49 27 164 large all

cGa a dark pastel white total Womenswear skirt 8 8 10 53 dress 20 20 5 35 subtotal 28 28 15 88 menswear pants 14 14 28 27 shirt 20 20 5  subtotal 34 34 33 76 total 62 62 48 164

cY none bachelors masters doctorate income income income income <50K/ >100K <25K >75K <25K >=25K /50 to 100K <50Kc ci =50K ( 25 to 75K bad average good ba average good ecellen

cache cYGanG l magnetic tapes

track t , ← spindle sectors cylinder c — ! read-write head cYaa anam Totation

(a) RAID 0: nonredundant striping (b) RAID 1: mirrored disks P P P P P (c) RAID 5: block-interleaved distributed parity P PQ PQ PQ PQ O P Q

d0 10101 Srinivasan Comp.Sci. 65000 d1 12121 Wu Finance 90000 d2 15151 Mozart Music 40000 d11 98345 Kim Elec.Eng. 80000 d 4 32343 E1Said History 60000 d5 33456 Gold Physics 87000 d6 45565 Katz Comp.Sci. 75000 d 7 58583 Califieri History 62000 d 8 76543 Singh Finance 80000 d 9 76766 Crick Biology 72000 d10 83821 Brandt Comp.Sci. 92000 File of Figure 13.1, with record 3 deleted and final record m ne address of the first record whose contents are deleted. w re the address of the second available record, and so on. I 1ese stored addresses as pointers, since they point to the loc ed records thus form a linked list, which is often referred tt hows the file of Figure 13.1, with the free list, after records c of a new record, we use the record pointed to by the he  pointer to point to the next available record. If no space is ccord to the end of the fi le. 10101 Srinivasan Comp.Sci. 65000 15151 Mozart Music 40000 22222 Einstein Physics 95000 33456 Gold Physics 87000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 2 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000  76543 Singh Finance 80000 二 1 76766 Crick Biology 72000  83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 32222 Verdi Music 48000

Relation_metadata Attribute_metadata Yelationname relationname number_of_attributes attribute name sataerage_organization domain_type Tndex_metadata [Yeeanaonname cYGn _ User_metadata cYanee _anrY utel user name encrypted_password ynew name

Index Data Col1 Index Row Data Col2 Index Col3 Index Stripe Footer Col4 Index Index Data Col5 Index Row Data Coll Data Col2 Data Stripe Footer Col3 Data Co14 Data Col5 Data Index Data Row Data Stripe Footer File Footer

Col1 Data Col2 Data Col3 Data Col4 Data Col5 Data Col1 Data Col2 Data  Col3 Data Col4 Data Col5 Data Col1 Data Col2 Data Col3 Data irection Table Cal4 Dana Col5 Data

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

10101 10101 Srinivasan Comp.Sci. 65000 12121 12121 Wu Finance 90000 15151 15151 Mozart Music 40000 22222 22222 Einstein Physics 95000 32343 32343 E1 Said History 60000 33456 33456 Gold Physics 87000 45565 45565 Katz Comp.Sci. 75000 58583 58583 Califeri History 62000 76543 76543 Singh Finance 80000  76766 76766 Crick Biology 72000 83821 83821 Brandt Comp.Sci. 92000 98345 98345 Kim Elec.Eng. 80000 Figure 14.2 Dense index. search-key (dept_name) order. We continue processing records until we encounter cecord for a department other than History. As we have seen, it is generally faster to locate a record if we have a dense ind cather than a sparse index. However, sparse indices have advantages over dense indic n that they require less space and they impose less maintenance overhead for insertioi There is a trade-off that the system designer must make between access time ar space overhead. Although the decision regarding this trade-off depends on the specif application, a good compromise is to have a sparse index with one index entry p 10101 10101 Srinivasan Comp.Sci. 65000 32343 12121 Wu Finance 90000 76766 / 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

Biology 6766 Crik S Biogy 7200 6 Comp.Sci 10101 i asan Comp.Scid 500 45565 Katz 75000 Elec.Eng. Comp.Sci. Finance 83821 Brandt Comp.Sci. 92000 History 98345 Kim Elec.Eng. 80000 Music 12121 Wu Finance 90000 Physics 76543 Singh Finance 80000 32343 E1 Said History 60000  58583 Califieri History 62000 1 15151 Mozart Music 40000 22222 Einstein Physics 95000 33465 Gold Physics 87000

(cblock 0 d block 0 block 1 block 1

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 Biology 15151 Mozart Music 40000  Elec.Eng. 32343 E1 Said History 60000 nance 33456 Gold Physic 87000 History 45565 Kat Comp.Sci. 75000 58583 History Music  Califieri 62000 76543 Singh Finance 80000 Physics 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

Brandt Califieri Crick Pointer to next leaf node 10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 80000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 60000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

Einstein Gold Srinivasan a Internal nodes Leaf nodes Brandt Califeri Crick → Einstein E1 Said → Gold Katz Kim → Mozart Singh  Srinivasan Wu  → 10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 El Said History 80000 33456 Gold Physics 87000   45565 Katz Comp. Sci. 75000  58583 Califeri History 60000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec. Eng. 80000 Figure 14.9 B+-tree for instructor file (n = 4). Let us consider a node containing m pointers (m ≤ n). For i = 2, 3, ... , m — 1, pointer P, points to the subtree that contains search-key values less than K, and greater than or equal to K, 1. Pointer Pm points to the part of the subtree that contains those key values greater than or equal to Km – 1, and pointer P1 points to the part of the subtree Unlike other nonleaf nodes, the root node can hold fewer than [n/2] pointers; however, it must hold at least two pointers, unless the tree consists of only one node. It is always possible to construct a B+-tree, for any n, that satisfies the preceding require- cYmann . Figure 14.9 shows a complete B+-tree for the instructor file (with n = 4). We have omitted null pointers for simplicity; any pointer field in the figure that does not have Figure 14.10 shows another B +-tree for the instructor fi le. this time with n = 6. Observe that the height of this tree is less than that of the previous tree, which had n=4. EI Said Mozart Brandt Califeri Crick Einstein  E Said Gold Katz Kim Mozart Singh Srinivasan Wu

Adams Brandt Calif eri Crick  Figure 14.13 Split of leaf node on insertion of "Adams". node plus the value being inserted), and put the first n/2] in the existing node and the remaining values in a newly created node. Having split a leaf node, we must insert the new leaf node into the B+-tree structure. In our exam ple, the new node has " Califieri" as its sm allest search-key value. We need to insert an entry with this search-key value, and a pointer to the new node, into the parent of the leaf node that was split. The B +-tree of Figure 14.14 shows the result of the insertion. It was possible to perform this insertion with no further node split, because there was room in the parent node for the new entry. If there were no room , the parent would have had to be split, requiring an entry to be added to its parent. In the worst case, all nodes along the path to the root must be split. If the root itself is split, the Splitting of a nonleaf node is a little different from splitting of a leaf node. Figure 14.15 shows the result of inserting a record with search key "Lamport" into the tree shown in Figure 14.14. The leaf node in which "Lamport" is to be inserted already has entries " Gold", "Katz", and "Kim", and as a result the leaf node has to be split. The new right-hand-side node resulting from the split contains the search-key values "Kim" and "Lamport". An entry (Kim , nl) must then be added to the parent node, where n l is a pointer to the new node, However, there is no space in the parent node to add a new entry, and the parent node has to be split. To do so, the parent node is conceptually expanded temporarily, the entry added, and the overfull node is then immediately split. When an overfull nonleaf node is split, the child pointers are divided among the original and the newly created nodes; in our example, the original node is left with the first three pointers, and the newly created node to the right gets the remaining two pointers. The search key values are, however, handled a little differently. The search key values that lie between the pointers m oved to the right node (in our example, the value "Kim") are moved along with the pointers, while those that lie between the pointers that stay on the left (in our example, "Califieri" and "Einstein") remain undisturbed. ozart Califieri Einstein Gold Srinivasan Adams Brandt → Califieri Crick  - Einstein EI Said → Gold Katz Kim → Mozart Singh Srinivasan Wu

Adams Brandt Califieri Crick Einstein E Said Gold Katz Kim Lamport Mozart Singh Srinivasan Wu

Gl Adams Brandt  Califieri Crick Einstein E1 Said Gold Katz Kim Mozart Singh Wu

Califer Einstein Km Adams Brandt  Califieri Crick  Einstein EI Said  Gold  Katz F Kim Mozart

Califer Einstein Gold Adams Brandt Califieri Crick Einstein E1 Said Katz Kim Mozart

C K (A,4) (B,8) → (C,1) (D,9)(E,4) →(F,7) (G,3)(H,3) (I,4) (J,8) (K,1) (L,6)  → (M,4) (N,8)(P,6)

Einstein Katz Singh Einstein Katz Singh record record record Brandt Califieri Crick . E1 Said Gold   Kim Mozart  4 Srinivasan  Wu
