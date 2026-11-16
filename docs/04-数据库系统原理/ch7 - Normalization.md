# ch7 - Normalization

> 主题库：数据库系统原理　｜　源文件：ch7 - Normalization.pptx　｜　90,427 字符　｜　107 个片段　｜　更新于 2026-11-11
Chapter 7: Normalization

Normalization TheoryDecide whether a particular relation R is in “good” form.In the case that a relation R is not in “good” form, decompose it into set of relations {R1, R2, ..., Rn} such that Each relation is in good form The decomposition is a lossless decompositionOur theory is based on:Functional dependenciesMultivalued dependencies

First Normal FormDomain is atomic if its elements are considered to be indivisible unitsExamples of non-atomic domains:Set of names, composite attributesIdentification numbers like CS101 that can be broken up into partsA relational schema R is in first normal form if the domains of all attributes of R are atomicNon-atomic values complicate storage and encourage redundant (repeated) storage of dataExample: Set of accounts stored with each customer, and set of owners stored with each accountWe assume all relations are in first normal form (and revisit this in Chapter 22: Object Based Databases)

First Normal Form (Cont.)Atomicity is actually a property of how the elements of the domain are used.Example: Strings would normally be considered indivisible Suppose that students are given roll numbers which are strings of the form CS0012 or EE1127If the first two characters are extracted to find the department, the domain of roll numbers is not atomic.Doing so is a bad idea: leads to encoding of information in application program rather than in the database.

Functional DependenciesThere are usually a variety of constraints (rules) on the data in the real world.For example, some of the constraints that are expected to hold in a university database are:Students and instructors are uniquely identified by their ID.Each student and instructor has only one name.Each instructor and student is (primarily) associated with only one department.Each department has only one value for its budget, and only one associated building.

Functional Dependencies (Cont.)An instance of a relation that satisfies all such real-world constraints is called a legal instance of the relation; A legal instance of a database is one where all the relation instances are legal instancesConstraints on the set of legal relations.Require that the value for a certain set of attributes determines uniquely the value for another set of attributes.A functional dependency is a generalization of the notion of a key.

Functional Dependencies Definition Let R be a relation schema   R and   RThe functional dependency   holds on R if and only if for any legal relations r(R), whenever any two tuples t1 and t2 of r agree on the attributes , they also agree on the attributes . That is, 
 t1[] = t2 []  t1[ ] = t2 [ ] 
Example: Consider r(A,B ) with the following instance of r.

On this instance, B  A hold; A  B does NOT hold, 
41 53 7

Closure of a Set of Functional DependenciesGiven a set F set of functional dependencies, there are certain other functional dependencies that are logically implied by F. If A  B and B  C, then we can infer that A  Cetc.The set of all functional dependencies logically implied by F is the closure of F.We denote the closure of F by F+.

Keys and Functional DependenciesK is a superkey for relation schema R if and only if K  RK is a candidate key for R if and only if K  R, andfor no   K,   RFunctional dependencies allow us to express constraints that cannot be expressed using superkeys. Consider the schema: in_dep (ID, name, salary, dept_name, building, budget ). We expect these functional dependencies to hold: dept_name building ID  building but would not expect the following to hold: dept_name  salary

Use of Functional DependenciesWe use functional dependencies to:To test relations to see if they are legal under a given set of functional dependencies. If a relation r is legal under a set F of functional dependencies, we say that r satisfies F.To specify constraints on the set of legal relationsWe say that F holds on R if all legal relations on R satisfy the set of functional dependencies F.Note: A specific instance of a relation schema may satisfy a functional dependency even if the functional dependency does not hold on all legal instances. For example, a specific instance of instructor may, by chance, satisfy name  ID.

Trivial Functional DependenciesA functional dependency is trivial if it is satisfied by all instances of a relationExample: ID, name  ID name  nameIn general,    is trivial if    

Lossless DecompositionWe can use functional dependencies to show when certain decomposition are lossless. For the case of R = (R1, R2), we require that for all possible relations r on schema R r = R1 (r ) R2 (r ) A decomposition of R into R1 and R2 is lossless decomposition if at least one of the following dependencies is in F+:R1  R2  R1R1  R2  R2The above functional dependencies are a sufficient condition for lossless join decomposition; the dependencies are a necessary condition only if all constraints are functional dependencies

ExampleR = (A, B, C)F = {A  B, B  C)R1 = (A, B), R2 = (B, C)Lossless decomposition: R1  R2 = {B} and B  BCR1 = (A, B), R2 = (A, C)Lossless decomposition: R1  R2 = {A} and A  ABNote: B  BC is a shorthand notation for B  {B, C}

OutlineFeatures of Good Relational DesignFunctional DependenciesDecomposition Using Functional DependenciesNormal FormsFunctional Dependency TheoryAlgorithms for Decomposition using Functional DependenciesDecomposition Using Multivalued Dependencies More Normal FormAtomic Domains and First Normal FormDatabase-Design ProcessModeling Temporal Data

Dependency PreservationTesting functional dependency constraints each time the database is updated can be costlyIt is useful to design the database in a way that constraints can be tested efficiently. If testing a functional dependency can be done by considering just one relation, then the cost of testing this constraint is lowWhen decomposing a relation it is possible that it is no longer possible to do the testing without having to perform a Cartesian Produced.A decomposition that makes it computationally hard to enforce functional dependency is said to be NOT dependency preserving.

Dependency Preservation ExampleConsider a schema: dept_advisor(s_ID, i_ID, department_name)With function dependencies: i_ID  dept_name s_ID, dept_name  i_IDIn the above design we are forced to repeat the department name once for each time an instructor participates in a dept_advisor relationship. To fix this, we need to decompose dept_advisorAny decomposition will not include all the attributes in s_ID, dept_name  i_IDThus, the composition NOT be dependency preserving 

Normal Forms

Boyce-Codd Normal FormA relation schema R is in BCNF with respect to a set F of functional dependencies if for all functional dependencies in F+ of the form    where   R and   R, at least one of the following holds:   is trivial (i.e.,   ) is a superkey for R

Boyce-Codd Normal Form (Cont.)Example schema that is not in BCNF: in_dep (ID, name, salary, dept_name, building, budget ) because :dept_name building, budget holds on in_depbut dept_name is not a superkeyWhen decompose in_dept into instructor and department instructor is in BCNFdepartment is in BCNF

Decomposing a Schema into BCNFLet R be a schema R that is not in BCNF. Let   be the FD that causes a violation of BCNF.We decompose R into:( U  )( R - (  -  ) )In our example of in_dep,  = dept_name = building, budgetand in_dep is replaced by ( U  ) = ( dept_name, building, budget )( R - (  -  ) ) = ( ID, name, dept_name, salary )

ExampleR = (A, B, C)F = {A  B, B  C)R1 = (A, B), R2 = (B, C)Lossless-join decomposition: R1  R2 = {B} and B  BCDependency preservingR1 = (A, B), R2 = (A, C)Lossless-join decomposition: R1  R2 = {A} and A  ABNot dependency preserving (cannot check B  C without computing R1 R2)

BCNF and Dependency PreservationIt is not always possible to achieve both BCNF and dependency preservation Consider a schema: dept_advisor(s_ID, i_ID, department_name)With function dependencies: i_ID  dept_name s_ID, dept_name  i_IDdept_advisor is not in BCNF i_ID is not a superkey.Any decomposition of dept_advisor will not include all the attributes in s_ID, dept_name  i_IDThus, the composition is NOT be dependency preserving

Third Normal FormA relation schema R is in third normal form (3NF) if for all:    in F+ at least one of the following holds:   is trivial (i.e.,   ) is a superkey for REach attribute A in  –  is contained in a candidate key for R. (NOTE: each attribute may be in a different candidate key)If a relation is in BCNF it is in 3NF (since in BCNF one of the first two conditions above must hold).Third condition is a minimal relaxation of BCNF to ensure dependency preservation (will see why later).

3NF ExampleConsider a schema: dept_advisor(s_ID, i_ID, dept_name)With function dependencies: i_ID  dept_name s_ID, dept_name  i_IDTwo candidate keys = {s_ID, dept_name}, {s_ID, i_ID }We have seen before that dept_advisor is not in BCNFR, however, is in 3NF s_ID, dept_name is a superkey i_ID  dept_name and i_ID is NOT a superkey, but:{ dept_name} – {i_ID } = {dept_name } anddept_name is contained in a candidate key

Overview of Normalization

Redundancy in 3NFConsider the schema R below, which is in 3NFWhat is wrong with the table?R = (J, K, L )F = {JK  L, L  K }And an instance table:Repetition of informationNeed to use null values (e.g., to represent the relationship l2, k2 where there is no corresponding value for J)

Comparison of BCNF and 3NFAdvantages to 3NF over BCNF. It is always possible to obtain a 3NF design without sacrificing losslessness or dependency preservation. Disadvantages to 3NF. We may have to use null values to represent some of the possible meaningful relationships among data items. There is the problem of repetition of information.

Goals of NormalizationLet R be a relation scheme with a set F of functional dependencies.Decide whether a relation scheme R is in “good” form.In the case that a relation scheme R is not in “good” form, need to decompose it into a set of relation scheme {R1, R2, ..., Rn} such that:Each relation scheme is in good form The decomposition is a lossless decompositionPreferably, the decomposition should be dependency preserving.

How good is BCNF?There are database schemas in BCNF that do not seem to be sufficiently normalized Consider a relation inst_info (ID, child_name, phone)where an instructor may have more than one phone and can have multiple childrenInstance of inst_info

There are no non-trivial functional dependencies and therefore the relation is in BCNF Insertion anomalies – i.e., if we add a phone 981-992-3443 to 99999, we need to add two tuples (99999, David, 981-992-3443) (99999, William, 981-992-3443)How good is BCNF? (Cont.)

It is better to decompose inst_info into:inst_child:inst_phone:This suggests the need for higher normal forms, such as Fourth Normal Form (4NF), which we shall see laterHigher Normal Forms 

Functional-Dependency Theory

Functional-Dependency Theory RoadmapWe now consider the formal theory that tells us which functional dependencies are implied logically by a given set of functional dependencies.We then develop algorithms to generate lossless decompositions into BCNF and 3NFWe then develop algorithms to test if a decomposition is dependency-preserving

Closure of a Set of Functional DependenciesGiven a set F set of functional dependencies, there are certain other functional dependencies that are logically implied by F. If A  B and B  C, then we can infer that A  Cetc.The set of all functional dependencies logically implied by F is the closure of F.We denote the closure of F by F+.

Closure of a Set of Functional DependenciesWe can compute F+, the closure of F, by repeatedly applying Armstrong’s Axioms:Reflexive rule: if   , then    Augmentation rule: if   , then     Transitivity rule: if   , and   , then   These rules are Sound -- generate only functional dependencies that actually hold, and Complete -- generate all functional dependencies that hold.

Features of Good Relational DesignsSuppose we combine instructor and department into in_dep, which represents the natural join on the relations instructor and departmentThere is repetition of informationNeed to use null values (if we add a new department with no instructors) 

Example of F+R = (A, B, C, G, H, I)F = { A  B A  C CG  H CG  I B  H}Some members of F+A  H by transitivity from A  B and B  HAG  I by augmenting A  C with G, to get AG  CG and then transitivity with CG  I CG  HI by augmenting CG  I to infer CG  CGI, and augmenting of CG  H to infer CGI  HI, and then transitivity

Closure of Functional Dependencies (Cont.)Additional rules:Union rule: If    holds and    holds, then     holds.Decomposition rule: If     holds, then    holds and    holds.Pseudotransitivity rule:If    holds and     holds, then     holds.The above rules can be inferred from Armstrong’s axioms.

Procedure for Computing F+To compute the closure of a set of functional dependencies F: F + = F repeat for each functional dependency f in F+ apply reflexivity and augmentation rules on f add the resulting functional dependencies to F + for each pair of functional dependencies f1and f2 in F + if f1 and f2 can be combined using transitivity then add the resulting functional dependency to F + until F + does not change any further NOTE: We shall see an alternative procedure for this task later

Closure of Attribute SetsGiven a set of attributes a, define the closure of a under F (denoted by a+) as the set of attributes that are functionally determined by a under F Algorithm to compute a+, the closure of a under F result := a; while (changes to result) do for each    in F do begin if   result then result := result   end

Example of Attribute Set ClosureR = (A, B, C, G, H, I)F = {A  B A  C CG  H CG  I B  H}(AG)+1. result = AG2. result = ABCG (A  C and A  B)3. result = ABCGH (CG  H and CG  AGBC)4. result = ABCGHI (CG  I and CG  AGBCH)Is AG a candidate key? Is AG a super key?Does AG  R? == Is R  (AG)+ Is any subset of AG a superkey?Does A  R? == Is R  (A)+ Does G  R? == Is R  (G)+ In general: check for each subset of size n-1

Uses of Attribute ClosureThere are several uses of the attribute closure algorithm:Testing for superkey:To test if  is a superkey, we compute +, and check if + contains all attributes of R.Testing functional dependenciesTo check if a functional dependency    holds (or, in other words, is in F+), just check if   +. That is, we compute + by using attribute closure, and then check if it contains . Is a simple and cheap test, and very usefulComputing closure of FFor each   R, we find the closure +, and for each S  +, we output a functional dependency   S.

Canonical CoverSuppose that we have a set of functional dependencies F on a relation schema. Whenever a user performs an update on the relation, the database system must ensure that the update does not violate any functional dependencies; that is, all the functional dependencies in F are satisfied in the new database state.If an update violates any functional dependencies in the set F, the system must roll back the update.We can reduce the effort spent in checking for violations by testing a simplified set of functional dependencies that has the same closure as the given set. This simplified set is termed the canonical coverTo define canonical cover we must first define extraneous attributes.An attribute of a functional dependency in F is extraneous if we can remove it without changing F + 

Extraneous AttributesRemoving an attribute from the left side of a functional dependency could make it a stronger constraint. For example, if we have AB  C and remove B, we get the possibly stronger result A  C. It may be stronger because A  C logically implies AB  C, but AB  C does not, on its own, logically imply A  CBut, depending on what our set F of functional dependencies happens to be, we may be able to remove B from AB  C safely. For example, suppose thatF = {AB  C, A  D, D  C}Then we can show that F logically implies A  C, making extraneous in AB  C.

Extraneous Attributes (Cont.)Removing an attribute from the right side of a functional dependency could make it a weaker constraint. For example, if we have AB  CD and remove C, we get the possibly weaker result AB  D. It may be weaker because using just AB  D, we can no longer infer AB  C.But, depending on what our set F of functional dependencies happens to be, we may be able to remove C from AB  CD safely. For example, suppose that F = { AB  CD, A  C.Then we can show that even after replacing AB  CD by AB  D, we can still infer $AB  C and thus AB  CD.

Extraneous AttributesAn attribute of a functional dependency in F is extraneous if we can remove it without changing F + Consider a set F of functional dependencies and the functional dependency    in F.Remove from the left side: Attribute A is extraneous in  if A   and F logically implies (F – {  })  {( – A)  }.Remove from the right side: Attribute A is extraneous in  ifA   and The set of functional dependencies (F – {  })  { ( – A)} logically implies F.Note: implication in the opposite direction is trivial in each of the cases above, since a “stronger” functional dependency always implies a weaker one

A Combined Schema Without RepetitionConsider combining relations sec_class(sec_id, building, room_number) and section(course_id, sec_id, semester, year) into one relationsection(course_id, sec_id, semester, year, building, room_number)No repetition in this caseNot all combined schemas result in repetition of information

Testing if an Attribute is ExtraneousLet R be a relation schema and let F be a set of functional dependencies that hold on R . Consider an attribute in the functional dependency   .To test if attribute A   is extraneous in  Consider the set: F' = (F – {  })  { ( – A)}, check that + contains A; if it does, A is extraneous in  To test if attribute A   is extraneous in  Let  =  – {A}. Check if    can be inferred from F. Compute + using the dependencies in F If + includes all attributes in  then , A is extraneous in  

Examples of Extraneous AttributesLet F = {AB  CD, A  E, E  C }To check if C is extraneous in AB  CD, we: Compute the attribute closure of AB under F' = {AB  D, A  E, E  C}The closure is ABCDE, which includes CDThis implies that C is extraneous

Canonical CoverF logically implies all dependencies in Fc , and Fc logically implies all dependencies in F, andNo functional dependency in Fc contains an extraneous attribute, andEach left side of functional dependency in Fc is unique. That is, there are no two dependencies in Fc1  1 and 2  2 such that 1 = 2 A canonical cover for F is a set of dependencies Fc such that 

Canonical CoverTo compute a canonical cover for F: repeat Use the union rule to replace any dependencies in F of the form 1  1 and 1  2 with 1  1 2 Find a functional dependency    in Fc with an extraneous attribute either in  or in  /* Note: test for extraneous attributes done using Fc, not F*/ If an extraneous attribute is found, delete it from    until (Fc not change Note: Union rule may become applicable after some extraneous attributes have been deleted, so it has to be re-applied

Example: Computing a Canonical CoverR = (A, B, C)F = {A  BC B  C A  B AB  C}Combine A  BC and A  B into A  BCSet is now {A  BC, B  C, AB  C}A is extraneous in AB  CCheck if the result of deleting A from AB  C is implied by the other dependenciesYes: in fact, B  C is already present!Set is now {A  BC, B  C}C is extraneous in A  BC Check if A  C is logically implied by A  B and the other dependenciesYes: using transitivity on A  B and B  C. Can use attribute closure of A in more complex casesThe canonical cover is: A  B B  C

Dependency Preservation Let Fi be the set of dependencies F + that include only attributes in Ri. A decomposition is dependency preserving, if (F1  F2  …  Fn )+ = F +Using the above definition, testing for dependency preservation take exponential time.Not that if a decomposition is NOT dependency preserving then checking updates for violation of functional dependencies may require computing joins, which is expensive.

Dependency Preservation (Cont.)Let F be the set of dependencies on schema R and let R1, R2 , .., Rn be a decomposition of R.The restriction of F to Ri is the set Fi of all functional dependencies in F + that include only attributes of Ri .Since all functional dependencies in a restriction involve attributes of only one relation schema, it is possible to test such a dependency for satisfaction by checking only one relation.Note that the definition of restriction uses all dependencies in in F +, not just those in F.The set of restrictions F1, F2 , .. , Fn is the set of functional dependencies that can be checked efficiently.

Testing for Dependency PreservationTo check if a dependency    is preserved in a decomposition of R into R1, R2, …, Rn , we apply the following test (with attribute closure done with respect to F)result = repeat for each Ri in the decomposition t = (result  Ri)+  Ri result = result  t until (result does not change)If result contains all attributes in , then the functional dependency    is preserved.We apply the test on all dependencies in F to check if a decomposition is dependency preservingThis procedure takes polynomial time, instead of the exponential time required to compute F+ and (F1  F2  …  Fn)+ 

ExampleR = (A, B, C )F = {A  B B  C}Key = {A}R is not in BCNFDecomposition R1 = (A, B), R2 = (B, C)R1 and R2 in BCNFLossless-join decompositionDependency preserving

Algorithm for Decomposition Using Functional Dependencies 

DecompositionThe only way to avoid the repetition-of-information problem in the in_dep schema is to decompose it into two schemas – instructor and department schemas.Not all decompositions are good. Suppose we decompose employee(ID, name, street, city, salary) into employee1 (ID, name) employee2 (name, street, city, salary) The problem arises when we have two employees with the same nameThe next slide shows how we lose information -- we cannot reconstruct the original employee relation -- and so, this is a lossy decomposition.

Testing for BCNFTo check if a non-trivial dependency   causes a violation of BCNF1. compute + (the attribute closure of ), and 2. verify that it includes all attributes of R, that is, it is a superkey of R.Simplified test: To check if a relation schema R is in BCNF, it suffices to check only the dependencies in the given set F for violation of BCNF, rather than checking all dependencies in F+.If none of the dependencies in F causes a violation of BCNF, then none of the dependencies in F+ will cause a violation of BCNF either.However, simplified test using only F is incorrect when testing a relation in a decomposition of RConsider R = (A, B, C, D, E), with F = { A  B, BC  D}Decompose R into R1 = (A,B) and R2 = (A,C,D, E) Neither of the dependencies in F contain only attributes from (A,C,D,E) so we might be mislead into thinking R2 satisfies BCNF. In fact, dependency AC  D in F+ shows R2 is not in BCNF. 

Testing Decomposition for BCNFEither test Ri for BCNF with respect to the restriction of F+ to Ri (that is, all FDs in F+ that contain only attributes from Ri)Or use the original set of dependencies F that hold on R, but with the following test:for every set of attributes   Ri, check that + (the attribute closure of ) either includes no attribute of Ri- , or includes all attributes of Ri.If the condition is violated by some    in F+, the dependency   (+ - )  Rican be shown to hold on Ri, and Ri violates BCNF.We use above dependency to decompose RiTo check if a relation Ri in a decomposition of R is in BCNF

BCNF Decomposition Algorithm result := {R };done := false;compute F +;while (not done) do if (there is a schema Ri in result that is not in BCNF) then begin let    be a nontrivial functional dependency that holds on Ri such that   Ri is not in F +, and    = ; result := (result – Ri )  (Ri – )  (,  ); end else done := true; 
 Note: each Ri is in BCNF, and decomposition is lossless-join.

Example of BCNF Decompositionclass (course_id, title, dept_name, credits, sec_id, semester, year, building, room_number, capacity, time_slot_id)Functional dependencies:course_id→ title, dept_name, creditsbuilding, room_number→capacitycourse_id, sec_id, semester, year→building, room_number, time_slot_idA candidate key {course_id, sec_id, semester, year}.BCNF Decomposition:course_id→ title, dept_name, credits holdsbut course_id is not a superkey. We replace class by:course(course_id, title, dept_name, credits)class-1 (course_id, sec_id, semester, year, building, room_number, capacity, time_slot_id)

BCNF Decomposition (Cont.)course is in BCNFHow do we know this?building, room_number→capacity holds on class-1 but {building, room_number} is not a superkey for class-1.We replace class-1 by:classroom (building, room_number, capacity)section (course_id, sec_id, semester, year, building, room_number, time_slot_id)classroom and section are in BCNF.

Third Normal FormThere are some situations where BCNF is not dependency preserving, and efficient checking for FD violation on updates is importantSolution: define a weaker normal form, called Third Normal Form (3NF)Allows some redundancy (with resultant problems; we will see examples later)But functional dependencies can be checked on individual relations without computing a join.There is always a lossless-join, dependency-preserving decomposition into 3NF.

3NF Example -- Relation dept_advisordept_advisor (s_ID, i_ID, dept_name)F = {s_ID, dept_name  i_ID, i_ID  dept_name}Two candidate keys: s_ID, dept_name, and i_ID, s_IDR is in 3NFs_ID, dept_name  i_ID s_ID dept_name is a superkey i_ID  dept_name dept_name is contained in a candidate key

Testing for 3NFNeed to check only FDs in F, need not check all FDs in F+.Use attribute closure to check for each dependency   , if  is a superkey.If  is not a superkey, we have to verify if each attribute in  is contained in a candidate key of RThis test is rather more expensive, since it involve finding candidate keysTesting for 3NF has been shown to be NP-hardInterestingly, decomposition into third normal form (described shortly) can be done in polynomial time 

3NF Decomposition Algorithm Let Fc be a canonical cover for F;i := 0;for each functional dependency    in Fc do if none of the schemas Rj, 1  j  i contains   then begin i := i + 1; Ri :=   endif none of the schemas Rj, 1  j  i contains a candidate key for R then begin i := i + 1; Ri := any candidate key for R; end /* Optionally, remove redundant relations */ repeatif any schema Rj is contained in another schema Rk then /* delete Rj */ Rj = R;; i=i-1;return (R1, R2, ..., Ri) 

3NF Decomposition Algorithm (Cont.)Each relation schema Ri is in 3NFDecomposition is dependency preserving and lossless-joinProof of correctness is at end of this presentation (click here)Above algorithm ensures

A Lossy Decomposition

3NF Decomposition: An ExampleRelation schema:cust_banker_branch = (customer_id, employee_id, branch_name, type )The functional dependencies for this relation schema are:customer_id, employee_id  branch_name, typeemployee_id  branch_namecustomer_id, branch_name  employee_idWe first compute a canonical coverbranch_name is extraneous in the r.h.s. of the 1st dependencyNo other attribute is extraneous, so we get FC = customer_id, employee_id  type employee_id  branch_name customer_id, branch_name  employee_id

3NF Decompsition Example (Cont.)The for loop generates following 3NF schema: (customer_id, employee_id, type ) (employee_id, branch_name) (customer_id, branch_name, employee_id)Observe that (customer_id, employee_id, type ) contains a candidate key of the original schema, so no further relation schema needs be addedAt end of for loop, detect and delete schemas, such as (employee_id, branch_name), which are subsets of other schemasresult will not depend on the order in which FDs are consideredThe resultant simplified 3NF schema is: (customer_id, employee_id, type) (customer_id, branch_name, employee_id)

Comparison of BCNF and 3NFIt is always possible to decompose a relation into a set of relations that are in 3NF such that:The decomposition is losslessThe dependencies are preservedIt is always possible to decompose a relation into a set of relations that are in BCNF such that:The decomposition is losslessIt may not be possible to preserve dependencies.

Design GoalsGoal for a relational database design is:BCNF.Lossless join.Dependency preservation.If we cannot achieve this, we accept one ofLack of dependency preservation Redundancy due to use of 3NFInterestingly, SQL does not provide a direct way of specifying functional dependencies other than superkeys. Can specify FDs using assertions, but they are expensive to test, (and currently not supported by any of the widely used databases!)Even if we had a dependency preserving decomposition, using SQL we would not be able to efficiently test a functional dependency whose left hand side is not a key.

Multivalued Dependencies

Multivalued Dependencies (MVDs)Suppose we record names of children, and phone numbers for instructors:inst_child(ID, child_name)inst_phone(ID, phone_number)If we were to combine these schemas to getinst_info(ID, child_name, phone_number)Example data:(99999, David, 512-555-1234)(99999, David, 512-555-4321)(99999, William, 512-555-1234)(99999, William, 512-555-4321)This relation is in BCNFWhy?

Multivalued DependenciesLet R be a relation schema and let   R and   R. The multivalued dependency    holds on R if in any legal relation r(R), for all pairs for tuples t1 and t2 in r such that t1[] = t2 [], there exist tuples t3 and t4 in r such that: t1[] = t2 [] = t3 [] = t4 [] t3[] = t1 [] t3[R – ] = t2[R – ] t4 [] = t2[] t4[R – ] = t1[R – ] 

MVD -- Tabular representation Tabular representation of   

MVD (Cont.)Let R be a relation schema with a set of attributes that are partitioned into 3 nonempty subsets. Y, Z, WWe say that Y  Z (Y multidetermines Z )if and only if for all possible relations r (R )  r and  r then  r and  rNote that since the behavior of Z and W are identical it follows that Y  Z if Y  W 

ExampleIn our example: ID  child_name ID  phone_numberThe above formal definition is supposed to formalize the notion that given a particular value of Y (ID) it has associated with it a set of values of Z (child_name) and a set of values of W (phone_number), and these two sets are in some sense independent of each other.Note: If Y  Z then Y  ZIndeed we have (in above notation) Z1 = Z2The claim follows.

Lossless DecompositionLet R be a relation schema and let R1 and R2 form a decomposition of R . That is R = R1 U R2We say that the decomposition is a lossless decomposition if there is no loss of information by replacing R with the two relation schemas R1 U R2 Formally,  R1 (r)  R2 (r) = rAnd, conversely a decomposition is lossy if r   R1 (r)  R2 (r) = r 

Use of Multivalued DependenciesWe use multivalued dependencies in two ways: 1. To test relations to determine whether they are legal under a given set of functional and multivalued dependencies2. To specify constraints on the set of legal relations. We shall concern ourselves only with relations that satisfy a given set of functional and multivalued dependencies.If a relation r fails to satisfy a given multivalued dependency, we can construct a relations r that does satisfy the multivalued dependency by adding tuples to r. 

Theory of MVDsFrom the definition of multivalued dependency, we can derive the following rule:If   , then    That is, every functional dependency is also a multivalued dependencyThe closure D+ of D is the set of all functional and multivalued dependencies logically implied by D. We can compute D+ from D, using the formal definitions of functional dependencies and multivalued dependencies.We can manage with such reasoning for very simple multivalued dependencies, which seem to be most common in practiceFor complex dependencies, it is better to reason about sets of dependencies using a system of inference rules (Appendix C).

Fourth Normal FormA relation schema R is in 4NF with respect to a set D of functional and multivalued dependencies if for all multivalued dependencies in D+ of the form   , where   R and   R, at least one of the following hold:   is trivial (i.e.,    or    = R) is a superkey for schema RIf a relation is in 4NF it is in BCNF

Restriction of Multivalued DependenciesThe restriction of D to Ri is the set Di consisting ofAll functional dependencies in D+ that include only attributes of RiAll multivalued dependencies of the form   (  Ri) where   Ri and    is in D+ 

4NF Decomposition Algorithm result: = {R};done := false;compute D+;Let Di denote the restriction of D+ to Ri while (not done) if (there is a schema Ri in result that is not in 4NF) then begin let    be a nontrivial multivalued dependency that holds on Ri such that   Ri is not in Di, and ; result := (result - Ri)  (Ri - )  (, ); end else done:= true; Note: each Ri is in 4NF, and decomposition is lossless-join

ExampleR =(A, B, C, G, H, I) F ={ A  B B  HI CG  H }R is not in 4NF since A  B and A is not a superkey for RDecomposition a) R1 = (A, B) (R1 is in 4NF) b) R2 = (A, C, G, H, I) (R2 is not in 4NF, decompose into R3 and R4) c) R3 = (C, G, H) (R3 is in 4NF) d) R4 = (A, C, G, I) (R4 is not in 4NF, decompose into R5 and R6)A  B and B  HI  A  HI, (MVD transitivity), andand hence A  I (MVD restriction to R4) e) R5 = (A, I) (R5 is in 4NF) f)R6 = (A, C, G) (R6 is in 4NF)

Additional issues

Further Normal FormsJoin dependencies generalize multivalued dependencieslead to project-join normal form (PJNF) (also called fifth normal form)A class of even more general constraints, leads to a normal form called domain-key normal form.Problem with these generalized constraints: are hard to reason with, and no set of sound and complete set of inference rules exists.Hence rarely used

Overall Database Design ProcessR could have been generated when converting E-R diagram to a set of tables.R could have been a single relation containing all attributes that are of interest (called universal relation).Normalization breaks R into smaller relations.R could have been the result of some ad hoc design of relations, which we then test/convert to normal form.We have assumed schema R is given

ER Model and NormalizationWhen an E-R diagram is carefully designed, identifying all entities correctly, the tables generated from the E-R diagram should not need further normalization.However, in a real (imperfect) design, there can be functional dependencies from non-key attributes of an entity to other attributes of the entityExample: an employee entity with attributes department_name and building, functional dependency department_name buildingGood design would have made department an entityFunctional dependencies from non-key attributes of a relationship set possible, but rare --- most relationships are binary 

Example of Lossless Decomposition Decomposition of R = (A, B, C) R1 = (A, B) R2 = (B, C)

Denormalization for PerformanceMay want to use non-normalized schema for performanceFor example, displaying prereqs along with course_id, and title requires join of course with prereqAlternative 1: Use denormalized relation containing attributes of course as well as prereq with all above attributesfaster lookupextra space and extra execution time for updatesextra coding work for programmer and possibility of error in extra codeAlternative 2: use a materialized view defined a course prereqBenefits and drawbacks same as above, except no extra coding work for programmer and avoids possible errors

Other Design IssuesSome aspects of database design are not caught by normalizationExamples of bad database design, to be avoided: Instead of earnings (company_id, year, amount ), use earnings_2004, earnings_2005, earnings_2006, etc., all on the schema (company_id, earnings).Above are in BCNF, but make querying across years difficult and needs new table each yearcompany_year (company_id, earnings_2004, earnings_2005, earnings_2006)Also in BCNF, but also makes querying across years difficult and requires new attribute each year.Is an example of a crosstab, where values for one attribute become column namesUsed in spreadsheets, and in data analysis tools

Modeling Temporal DataTemporal data have an association time interval during which the data are valid.A snapshot is the value of the data at a particular point in timeSeveral proposals to extend ER model by adding valid time toattributes, e.g., address of an instructor at different points in timeentities, e.g., time duration when a student entity existsrelationships, e.g., time during which an instructor was associated with a student as an advisor.But no accepted standardAdding a temporal component results in functional dependencies like ID  street, city not holding, because the address varies over timeA temporal functional dependency X  Y holds on schema R if the functional dependency X  Y holds on all snapshots for all legal instances r (R).

Modeling Temporal Data (Cont.)In practice, database designers may add start and end time attributes to relationsE.g., course(course_id, course_title) is replaced by course(course_id, course_title, start, end)Constraint: no two tuples can have overlapping valid timesHard to enforce efficientlyForeign key references may be to current version of data, or to data at a point in timeE.g., student transcript should refer to course information at the time the course was taken

End of Chapter 7

Proof of Correctness of 3NF Decomposition Algorithm

Correctness of 3NF Decomposition Algorithm3NF decomposition algorithm is dependency preserving (since there is a relation for every FD in Fc)Decomposition is losslessA candidate key (C ) is in one of the relations Ri in decompositionClosure of candidate key under Fc must contain all attributes in R. Follow the steps of attribute closure algorithm to show there is only one tuple in the join result for each tuple in Ri

Correctness of 3NF Decomposition Algorithm (Cont.)Claim: if a relation Ri is in the decomposition generated by the above algorithm, then Ri satisfies 3NF.Proof:Let Ri be generated from the dependency   Let   B be any non-trivial functional dependency on Ri. (We need only consider FDs whose right-hand side is a single attribute.)Now, B can be in either  or  but not in both. Consider each case separately.

Correctness of 3NF Decomposition (Cont.)Case 1: If B in :If  is a superkey, the 2nd condition of 3NF is satisfiedOtherwise  must contain some attribute not in Since   B is in F+ it must be derivable from Fc, by using attribute closure on .Attribute closure not have used  . If it had been used,  must be contained in the attribute closure of , which is not possible, since we assumed  is not a superkey.Now, using  (- {B}) and   B, we can derive  B (since    , and B   since   B is non-trivial)Then, B is extraneous in the right-hand side of  ; which is not possible since   is in Fc.Thus, if B is in  then  must be a superkey, and the second condition of 3NF must be satisfied.

Correctness of 3NF Decomposition (Cont.)Case 2: B is in .Since  is a candidate key, the third alternative in the definition of 3NF is trivially satisfied.In fact, we cannot show that  is a superkey.This shows exactly why the third alternative is present in the definition of 3NF.Q.E.D.
