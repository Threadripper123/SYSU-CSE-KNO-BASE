# book-图集-矢量

> 主题库：数据库系统原理　｜　源文件：book-图集-矢量.pdf　｜　8,823 字符　｜　78 个片段　｜　更新于 2026-11-13
view1 view2 view n . Yogical level level

naive users sophisticated database (tellers,agents, application users use write use use application application query administration nterfaces programs tools tools compiler and linker DML queries DDL interpreter application engine cYa baef ermanager lemanager authorizaton transaction manager disk storage indices data dictionary data statistical data

c database system ifectur

c caa cYaa

ID ID  course_ id name sec id dept_name tot_cred semester year [arade section course course id   course id department advisor sec_ id  title : semester dept name s id dept_name i_id year building building time_slot credits budget [Yaom_number timeslotid tme_slot_d dy end_time prereq instructor classroom courseid ID  building prereg_id name capacity teaches salary sec id csemeste year

using (A 1, A 2, . . ., An)

76766 Crick 98988 Tanaka 45565 Katz 12345 Shankar 10101 Srinivasan 00128 Zhang 98345 Kim 76543 BroWn 76543 Singh 76653 Aoi 22222 Einstein 23121 Chavez instructor 44553 Peltier

instructor student ID advisor D name name csalary tot_cred

gRADE student takes section

course course_id title prereq credits prereq_id

project instructor student ID proj_guide D [name name

ite nam e address st_name  middle_initial last_name street city state posta

instructor ID [Tna me first_name [manddle_ ini al last_name address street csareet_number careet_ ame apt_number city state zip { phone_number ] date_of_birth age ( )

A B a1 b1 α2 $b2 a3 b3 AA (a)

A B b a1 b2 a2 b3 a3 b4 DS (b)

B a a2 b1 a3 b2 a4 b3 AS (a)

B a1 b a2 b. 2 a3 b3 a4 b4 (b)

ID 不 advisor ↓ ID [name name calary tot_cred

instructor student ID ID [Tname name salar tot_cred (d) Many-to-many

instructor student ID advisor ID [Tname name csalaYe tot_cred (c) Manv-to-one

instructor student ID 不 advisor ID [name name salary tot_cred ab) One-to-many

instructor student I① advisor [Ta me name

instructor student D 0..* advisor 1..1 D [Ta me name

course section course id sec_course sec_id tit cYa cYedits

cYea nme l course_dept dept name building cYanan l inst_dept stud_dept 二 instructor student ID ID 4 advisor name name salary tot_cred teaches takes grade section course sec_id. time_slot course id  semester title sec_course sec_time_slot time slot id year { day caedits start_time end_nGme prereq ourse_id prereq_id sec_class classroom building cYroomenembe capacity

person name street city employee student cYaY Y l 4 instructor secretary rank hours_per_week

project instructor student proj_guide eval_ for evaluation

project instructor student proj_guide eval_for eYvaleation

cYmar student stud_ section section

staueent department D stud_dept → dept_name (a) Incorrect use of attribute

student marks_in assignment sec_assign section

marks student stud_ section section

[registration section_reg … student_reg section student sec_id. ID year. tot_cred

A RA B RB= E Re

1.h R E C li E1 E2 E3

A2.1 A2.2 A2 A1 A3 E A4

role1 R role2 E1 E2  A E1 role1 R role2 E2 |

A RA B RB E Rc ()

B Rc  C (c)

B

cYname name URL phone URL customer email written_by published_by name address phone book number ISBN title . shopping_basket basket_of cYYear price contains basket_ d cYnaam l - warehouse code stocks address phone

ID name street city salary 57766 Kim Main Perryridge 75000 98776 Kim North Hampton 67000 c emplee l ID name name street cit sa ry 57766 Kim Kim Main Perryridge 75000 98776 Kim Kim North Hampton 67000 natural join

ID name street cit salary 57766 Kim Main Perryridge 75000 57766 Kim North Hampton 67000 98776 Kim Main Perryridge 75000 98776 Kim North Hampton 67000

cYea rnmeetl cYal cYa instructor student ID dept_advisor ID name name salary tot_cred

a1 . . . ai aj + 1 . . . aj aj + 1 . . . an a1 . .. i i + 1 . .. j j + 1 . . t3 a1 . . ai ai + 1 . . . j bj + 1 . . . bn t4 a1 . . . ai i + 1 ... j a j + 1 . . . n

Srinivasan 6500 Comp.Sci. Zhang 102 [Ta me salary dept_name name tot_ced cYna _ 10101 comp_sci 00128 ta ches coerse_dept takes C-101 ecl sec _course sec_id title classroom semester year Intro.to Computer Science packard-101 Fal1 2017

instructor ID name first_ name [YmGeele_ n all last_name address street careet_ number street_name apt_number city state zIP {phone_number} cante_ of_ birt age ()

person Name addres employee student salary tot_credits instructor secretary rank hours_per_week

ID Name Department [00128 ] Zhang Comp.Sci. 12345 Shankar Comp.Sci. 19991 Brandt History

web sereee nenawerk application server database server browser data server

1 internet controller 8 2 cYa l view model 3 layer 4 ser vei

NameNode Metadata (name, replicas, ..) Metadata (name, replicas, .) Block Read DataNodes Block Replication

rk1 r V1 1 V1,V7, rk7 rV2 rk2 rVg,rVi mk1 V1 m2 V2 ▲ rk1 rV7 7 rk2 V2 V rk2 rVi mkn mVn rkj Vn

User Program copy cqpy copy assign assign map reduce Part1 Map1 Reduce1 File1 Part2 Part 3 Map 2 Reduce 1 write File 2 Part 4 local  write Part n read Map n Remote Reduce m File m

Op Op Data Sink Data Source Publish-Subscribe Data ↓ Data System Sink ource Data Daa Sink Source Op Op (b) Publish-suhscribe renresentation of streaming data fow

Data Source Op Data Sink Data Source Op Data Sink Data Op Op Sink Data SourCe

data source 1 cYa loaders  data source 2 cYaa l analysis tools data warehouse

item_info store item_id store_id color sales state saze country item_ d category store_id customer_id date customer number customer_id date_info price name date street month city quarter state year zipcode country

COLOr dark pastel white total skirt 8 35 10 53 dress 20 10 5 35 ne shirt 14 7 28 49 pants 20 2 5 27 total 62 54 48 164

2  5 3 1 11 4 7 6 12 29 2 8 5 7 22 16 8 20 14 20 62 4 34 18 35 10 7 2 54 9 21 45  10 5 28 5 48 42  sm 53 35 49 27 164 large all c skirt dress shirt pants all

0 womenswear sirt 8 10 53 dress 20 5 35 subtotal 28 28 15 88 menswear pants 14 14 28 7 shirt 20 20 5 subtotal 34 34 33 76 total 62 62 48 164

Period: Jan. 1 to March 31, 2009 Region Category Sales Subtotal North Computer Hardware 1,000,000 Computer Software 500,000 South Computer Hardware 200,000 Computer Software 400,000

cY none bachelors masters doctorate ncome income income icome <50K/ c>100K <25K >75K <25K =25K /50 to 100K <50K >=50K 25 to 75K bad average good

1 x 0 x 1 O x Xi 1 x 1

CaChE main memory flash memory magnetic disk cYGanG l magnetic tapes

track t 、 ←— spindle cylinder c — ! read– write head cYaaa arm

18 19

B Disk 1 B Dis 2 Dis 3 Disk 4 B3 B4 i B6 B7 P2 B9 B10

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 0 83821 Brandt Comp.Sci. 92000 1 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 0 83821 Brandt Comp.Sci. 92000 1 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 15151 Mozart Music 40000 22222 Einstein Physics 95000 33456 Gold Physics 87000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 0 83821 Brandt Comp.Sci. 92000 1 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 1 98345 Kim Elec.Eng. 80000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 0 83821 Brandt Comp.Sci. 92000

[a nre reeSpace

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000

10101 Srinivasan Comp.Sci. 65000 12121 Wu Finance 90000 15151 Mozart Music 40000 22222 Einstein Physics 95000 32343 E1 Said History 60000 33456 Gold Physics 87000 45565 Katz Comp.Sci. 75000 58583 Califieri History 62000 76543 Singh Finance 80000 76766 Crick Biology 72000 83821 Brandt Comp.Sci. 92000 98345 Kim Elec.Eng. 80000 32222 Verdi Music 48000

Relation_metadata Attribute_metadata relation name relation name number_of_attributes attribute name storage_organization domain_type lacation positon Yandex_metadata relation name cYGn _ l User_metadata username

User_metadata user name encrypted_password

Index Data Col1 Index Row Data Col2 Index Co13 Index Col4 Index Index Data Col5 Index Row Data Col1 Data Co12 Data Stripe Footer Col3 Data … Col4 Data : - Co15 Data - cYaa Row Data cYa G File Footer
