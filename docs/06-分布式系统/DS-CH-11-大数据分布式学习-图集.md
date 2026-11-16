# DS-CH-11-大数据分布式学习-图集

> 主题库：分布式系统　｜　源文件：DS-CH-11-大数据分布式学习-图集.pdf　｜　16,109 字符　｜　56 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

2002 2005 183 1985 1995 2000 2003 2007 AFS NFs Zebra OceanStore GFS Ceph Venti XRootD andrew File System Client-server ? Roaming home folders [aet ngsystem available . on am us. t asa clear precursor to the Dropbox-like software  pack- , ldentity tokens and agesI today. [...] 1 [It] allowed students ( (like Drew access control lists (ACLs) [ eustonandArashFerdowsi accesstoallher Subtree delegation , stuff fromanyconnected computer." 1 http://www.wired.com/2011/12/backdrop-dropbox Decentralized operation (AFS 3)

2002 2005 1983 1985 1995 2000 2003 2007 AFS NFS Zebra OceanStore GFS Ceph Venti XRootD Netwer ileSystem CLIENT SERVER Client-server System Calls System Calls · Focus onportability k ↓ Separationofprotocol vNoDE/vFs vnoDe/vFs and implementation 1 业 Statelessservers PC Filesystcm 4.2 Filesystem NFS Filesystem Server Routines 1 . Fastcrashrecovery  ● , State re-introduced for RPC/XDR RPC/XDR 88 locng Floppy. Disk NEtWORK Sandberg, Goldberg, Kleiman, Walsh, Lyon (1985)

2002 2005 1983 1985 195 2000 2003 2007 AFS NFS Zebra OceanStore GFS Ceph Venti XRootD Zebra File System [Fle FleC Paallel Striping and parity (RAIN) 1 2 3 4 5 6 Cli ent''s Log Redundant inexpensive  arrayof :nodes   1 2 3 1⊗2⊗3 Separation of data and 4 5 6 1 4 5 6 meta-data

2002 2005 1983 1985 1995 2000 2003 2007 AFS NFS Zebra OceanStore GFS Ceph Venti XRootD OceanStore pool Peer-to-peer pool ● " Global Scale": 1010 users,1014 files pool pool pool Based onpeer-to-peer w overlay network pool ● Nomadic data through Ya pooi  aggressive caching ● Fou ndationfor today''s decentral dropbox replacements Kubiatowicz et al.(2000)

2002 2005 1983 1985 1995 2000 2003 2007 AFS NFS Zebra OceanStore GFS Ceph Wemti XRootD VENTI Emelie: ratio of archival to active data Archival storage Jukebox/ Active —— Venti /Active . De-duplication through 6- aa ententaddressablestorage cY 4 ContenthashespRovide 3- intrinsic file integrity 2 . Merkletreesverifythe 1 file system hierarchy QuinlanandDorward (2002)

2002 200E 1983 1985 1995 2000 2003 2007 AFS NFS Zebra OceanStore GFS Ceph Venti XIRoo:lD XROOtD capen edrect Namespacedelegaton cYaedret open Client c Globaltree ofredirectors xrootd Flexibility 1 / through cmsd decomposition into 641 pluggable components 64 xrootd xrootd Namespace independent   cmsd cmsd from 642= 4096 data: access xrootd xrootd xootd xootd cmsd cmsd cmsd cmsd Hanushevsky (2013)

2002 2005 1983 1985 1995 2000 2003 2007 AFS NFS Zebra OceanStore GFS Cep Veanti_ XRootD CphFleSystemandRADos root row1 choose(1,row attheclusterscale row2 Incremental scalability throughCRUSH cab22 choose(3,cabinet) cab21 distributed hash function Adaptive workload distribution choose(1,disk) Weil (2007)

PFS Why How. Uses InstalI Media Docs Blog IPFS is the Distributeid Web Try it Watch demo

Erasure [an a Fault- Scalabilit POSIX Consensus tolerance, & Usability  Com- pliance create(); , unlink(), stat() Leases read(), , write(), seek() open(), close() essential Distributed , flock() difficultforDFSs File Systems rename() capen unlinked files hard links device files, IPC files a

全社会数据产生的速度非常快 Internet上一分钟产生的数据有多大? 20 47000 61141 New victims of App downloads Hours of music identity theft E ailssent nsales 2+million Search queries AndFuture GrowthisStaggering 3o 1.3 million Hoursofvideo uploaded Vidooviews IP Today,the By 2o 15, the 2 x to view all In 2o 15, numberof  number of itwould take video crossing nelworkod deviccs IP networks ncfv/orl:ad dlovicos the global you 5 years theglobal each second population population

大数据的呈现出指数增长度全球数据存储量呈现爆炸式增长，企业及互联网数据以每年50%的速率在增长，据Gartner预测，到2020年，全球数据量将达到35ZB，气等于80亿块4TB硬盘。 30% growth 软盘硬盘 SAN NAS KB GB TB PB ZB

Typical HPC Machine Compute Nodes Compute: Nodes High 6 end processor(s) cPU CPU CPU Lots of RAM Mem Mem Mem Network Specialized VeryI Network high  performance Storage Server RAiD-based | disk array Storage Server

HPCl MaChine Example   3rd fastest in world 18,688 nodes in Iargest partition 2a 2.6Ghz 6-coreAMD Opteron [acGb memory Total: 2.3 petaflop / 300 TB memory  Each node connected to 6 neighbors via 6.0 GB/s links Storage Server  10 PB I RAID-based disk array

HPCProgramming Model Application Programs Software Packages Hardware Programs described at very low level • Specify detailed control of processing & communications Rely on small number of software packages Limitsclassesofproblems &solutionmethods

Bulk Synchronous  Programming Solving Problem Over Grid G g., finite-element Partitioninto Regions nregionsforpprocessors Map Region per Processor Local computation sequential Periodicallycommunicate boundary values with neighbors

TypicalHPCOperation Characteristics Message Passing Make use  of spatial locality P P2 P3 P P 4 5 Hold all program c data in mem ory (no disk : access) High bandwidth communication High utilization of resources Effective for many scientific Reguirescarefultuning of applicationtoresources –8 — Intolerant of any variability

HPCFaultTolerance P P2 J D D 3 4 5 Checkpoint Periodically store state of all Wasted processes Signifi cant lO tnraffic Restore Restore When failure occurs Reset state to that of last Checkpoint checkpoint All intervening com putation Performance Scaling Very sensitive to number of failing components

Typical Cluster Machine Compute + Storage Nodes Nods Medium- cPU cPu cPu performance Mem Mem Mem prOcesSOrs Modest memory 1-2disks Network Network Conventional  mthernetswitches ● 10 Gb/s :within rack ● 100 Gb/s: across Racks

Machines with | Disks Latsofstoragefor 7200 RPM SATA 6 Gb/s 64MB Cache 3.5 Inch- 3 TB @ $150 WD3003FZEX  by Westerm Digital (5¢/GB) ☆☆☆☆ 1 1,615 customer reviews 28 answered questions List Price: 8249.99 Prce:$149.99 Prlme C 0 m p are 2007 :  2 You Save: $100.00(40%) ? In Stock. 0.75 TB @ $266 Ships from and sold by Amazon.com. Gint-wrap available. Want it Friday, Oct. 30? Order within 55 mins and choose Two-Day Shipping at 35@ / GB checkout. Details Capacity: 3 TB D ra W b aC KS  1 TB 2 TB 3 TB 4 TB  S71.79 Prime $116.99 Prime S149.99 Prime S198.00 Prime Roll over image to Zoom in 5 TB 6 TB Long and highly Variable $260.99 $298.91 NYat veryreleable Not included in HPC [Nnades

Data-lntensive System Challenge For Computation That Accesses 1 TB in 5 minutes  Data distributed over 100+ disks d Assuming uniform data partitioning Compute using 1o0+ processors System Requirements CPU CPU CPU a otsofdisks  I Lots of processors Node1 Node2 Noden  Within reach of fast, local-area network

ldealClusterProgramming Model Machine-lndependent Programming Model Hardware Application programs written in terms of high-level operations on data Rentimesystem ontrolsschedulingload balancing

Map/ReduceProgramming Model Reduce K1 K1 Kr Key-Value Pairs M M M M Map X₁ X2 x3 Xn aAggregate resultsin manydifferentways System deals with issues of resource allocation & reliability –16– Dean & Ghemawat: "MapReduce: Simplified Data

Come,Dick. Come and see. Come,come. Come and see. Come and see Spot. Come, Come Come, Come Come Dick and come. and and see. see. see Spot. Create anwordindex ofset ofdocuments

MapReduceExample 1 3 6 3 1 dick and Come See spot Sum cYa (dick,1) :<come,1) Word-Count (come,1) (come,1) (spot,1) Pairs (come, 1) (come, 2) (and, 1) - and, 1) M M M M M Extract Come Come Come Come Come Dick'' and come. and and see. see. see  Map: generate (word, count) pairs for all w ords in docum ent Reducesumwordcountsacrossdocuments

Hadoop Project hadaop File systemwith files  distributedacrossnodes CPU CPU CPU [TNode1 Node2 Noden Saoremultiple(typically3copiesofeachfileo Yf one node fails, data stillI available Logically, anynodehasaccess to any  1 file May need to fetch across network (ideally, leverage locality for perf.) Map I Reduce programming environment Sotwaremmanagesexecutionoftasksonnodes

Hadoop MapReduce APl F Programmer must supply Mapper & Reducer classes Steps through file one line at a time Code generates sequence of <key, value> pairs d Call output.collect(key, value) [d Default typesforkeys &valuesarestrings Lots of low-level machinery to convert to & from other data types  Given key + iterator that generates sequence of values Generateoneormoreckey,valueopairs ● Call output.collect(key, value)

Cluster Scalability Advantages Distributed system design principles lead to scalable design  Dynamically scheduled tasks with state held in replicated files ProvisioningAdvantages ⚫ More efficient technology refresh

ExampleSparse Matriceswith  B C 20 -1 -10 -80 30 40 X -2 -3 = -60 -25d 50 60 70 - 4 -170-46d Task:ComputeproductC=A-B Assume most matrix entries are 0 aCoreprobleminscientificcomputing Challengingforparallelexecution Demonstrate expressiveness of Map/Reduce

Computing Sparse Matrix Product A B 10 1 -1 ·1 1 20 A ·1 -1 B 1 20 → 3 2 -2 →. 1 30 40 A -2 -3 B 2 30 A 2 -3 2 2 50 B 60 70 2 40 4 3 - 4 A 3 B 3 50 A 3 60 A 2 70 3 A  3 Represent matrix as list of nonzero entries caYraw, ol value, m atri lDi cmarategy d Phase 1: Compute all products ai,k bk,j • Phase 2: Sum products for each entry i,j ?aEach phase involves a Map/Reduce

Phase 1 Map of Matrix Multiply 1 1 Key=1 1 20 1 10 A .3 A 1 1 1 1 30 50 B 2 A 2 3 A →1 40 2 Key = col A ·3 50 Key= 2 3 30 A · 1 2 A →2 2 -2 B 1 3 60 ·2 A 3 60 →2 2 -3 2 A B 3 70 3 A -1 1 Key= 3 B 20 -2 1 A 3 2  1 Key = roW B 40 4 -3 2 3 3 B . 2 2 A B 2 70 3 -4 ·2 3 3 B A

Phase 1“Reduce" of Matrix Multiply Key=1 - -10 1 1 10 1 C A X 1 -1 -50 B →1 3 3 50 A A →1 -60 2 c  1 Key = 2 -90 2 30 ·2 2 -2 2 A   1 B C × 3 60 -. -3 -120 2 2 3 C  1 A  ·2 B -180 3 十 2 C 20 Key = 3 -80 1 2 1 A 3 C -160 40 X3 4 2  2 2  3 A B C 3 -280 → 2 70 C 3 A 3 da Generateall productsai

Phase2 MapofMatrixMultiply 1 -10 1 c Key= 1,1 1 -10 1 c 3 -50· A 1 Key= 1,2 -80 · 1 2 C 2 -60 2 -60 C 1 Key =2,1 C 1 2 -90 2 Key = roW,Col C 2 -90 2 Key = 2,2 c 3 -120 C 1 2 -160 2 C 3 -180 c 2 3 -120 1 -80 2 Key=3,1 C 1 c 3 -50 · A ·1 2 -160 C 2 3 -280 2 3 -280 2 C Key= 3,2 C 3 -180 2 Group products aik bkiwith matching valuesofiand

Phase2Reduce ofMatrix Multiply  Key = 1,1 1 -10 1 -10 C 1 C -80 -80 Key= 1,2 1 2 1 c C Key = 2,1 2 -60 -60 C 2 C C 2 -90 -10 -80 Key=2,2 C 2 -250 2 2 C -60 -25d 2 -160 C 2 T170-4 6d 3 -120 3 -170 Key = 3,1 c 1 C 3 -50 A 1 3 -280 2 Key = 3,2 C 3 -460 c 2 3 -180 2 C

MatrixMultiplyPhase Mapper public class PlMapper extends MapReduceBase implements Mapper 1 public void map(WritableComparable key, Writable values, [aatputCollectorautput Reporterreportero throws IOException { try { GraphEdge e = new GraphEdge (values .toString()) ; IntWritable k; k = new IntWritable(e.toNode); k= new IntWritable(e.fromNode); ) Catch (BadGraphExceptione){)

MatrixMultiplyPhase1 Reducer public class PlReducer extends MapReduceBase implements Reducer public void reduce(WritableComparable key, Iterator values, OutputCollector output, Reporter reporter) Text outv = new Text(""); // Don''t really need output values /* First split edges into A and B categories */ LinkedList<GraphEdge> alist = new LinkedList<GraphEdge>(); LinkedList<GraphEdge> blist = new LinkedList<GraphEdge>(); while(values.hasNext()) { try newGraphEdge(values.next().tostring()); if (e.tag.equals("A")) { else{ 2 [_ae // Continued

MMPhase1Reducer(cont //i Continuation Iterator<GraphEdge> aset = alist.iterator(); // For each incoming J edge 6 while(aset.hasNext()) {  GraphEdge aedge =aset.next(); 1/For each outgoing edge Iterator<GraphEdge> bset = blist.iterator(); while (bset.hasNext()){ GraphEdge :] bedge = bset.next(); GraphEdge newe = aedge.contractProd (bedge); // Null would indicate invalid contraction if (newe !=null) { Text outk = new Text(newe.tostring()); aetput.collectloutkoutv,

Matrix Multiply F Phase 2 N Mapper public class   P2Mapper extendsI MapReduceBase implements Mapper public void map (WritableComparable key, I Writable values, Yutputcollector output, Reporter reportero throws  IoException   String es = values.toString(); try 1/ Key basedonhead&tail nodes String ks =e.fromNode +" " + e.toNode; output.collect(new I Text(ks), newText(e.tostring()));

Matrix Multiply Phase 2 Reducer public class P2Reducer extends MapReduceBase implements Reducer public void reduce(WritableComparable key, Iterator values, OutputCollector output, Reporter reporter) throws IOException { GraphEdge efinal = null; while (efinal == null && values.hasNext()) { try{ efinal = new GraphEdge(values.next() .toString()) ; } catch (BadGraphException e) {} if (efinal != null) { while(values.hasNext())f try GraphEdge eother = newGraphEdge(values.next().toString()); efinal.weight += eother.weight; cCatch (BadGraphException e) {) } if ( (efinal.weight != O) autput.collect(newText(efinal.tostring()), }

Associative Matching is Powerful Communication Primitive Intermediate step in Map/Reduce SimilarStrategyAppliestoOtherProblems [aaaeeo Kiefer, Volk, Lehner, TU Dresden Should do systematic comparison to other sparse matrix

MapReducelmplementation BuiltonTopofParallelFile System  C Google: GFS, Hadoop: HDFS [ aravides globalnaming Reliability via replication(typically 3 copies) Breaks workinto tasks Master schedules tasks on workers dynamically Typically #tasks >> #processors aaInput: Set of files in reliable file system aOutput: Set of files in reliable file system

MapReduce Execution (authors view) User (1) fork . (1) for (1) fr cYa (2) (2) assign assign reduce. map WORkER split 2 (3) read worker (4) local write split 3 worker output file1 split 4 WORKER Input Map Intermediate files Reduce Output files phase (on local diskse phase files

ROutputFiles Reducer Reducer •   Reducer R Reducers Task Mapper Mapper ●  Mapper M Mappers Manager [anaeutFiles Partitioned _nto Blocks

Mappln K h h(K ) { 0,., R −1} 0 1 R-1 Hash Function h Local FilES Maps each key / K to integer i such thatl 0 ≤i<R Mapper Operation Mapper Reads input file blocks Generates pairs (K,V Writes to local file h(K) Block

Mapping Dynamically map input file blocks onto mappers Each generates key/value pairs from its blocks Each writes R files on local file system  R local files Mapper Mapper   · Mapper MMappers TaSk InputFiles(PartitionedintoBlocks)

Handles 1/R of the possible key values Fetches its file from each of M mappers Sorts all of its entries to group values by keys Reducer Reducer • • • Reducer R Reducers

Reducing achReducer xecutesreducerfunction foreach key c Writes output values to parallel file system Reducer Reducer • • • Reducer R Reducers

[ael Input Files (Partitioned into Blocks) Reads set of files from file system caGaenerates new setoff les Can iterate to do more complex processing

Map/ReduceOperation Characteristics Map/Reduce Computation broken into ↓↓↓↓↓↓↓↓ Map many, short-lived tasks Reduce ● Mapping, reducing Map Use disk storage to hold MAP Reduce Strengths c k Ma Greatflexibility n placement Reduce scheduling, and Ioad balancen Can access large data sets Weaknesses Higher overhead Lower raw performance

Sort Benchmark  Partition into M = 15,000 64MB pieces F ? Key= value ⚫ Partition according to most significant bytes Sort locally with R = 4,000 reducers 11800 2Ghz Xeons Each with 2 160GB IDE disks Gigabit ethernet 891 seconds total

InterestingFeatures [Yaelaalea_ee aAssumereliablefile system  Detect failed worker ● Heartbeat mechanism Rescheduefaeedtask Tasks that take long time to execute  Might be bug, flaky hardware, or poor partitioning When done with most tasks, reschedule any remaining k Keep track of redundant executions ⚫ Significantly reduces overall run time

Map/Reduce Fault Tolerance Map/Reduce Store multiple copies ofeach [aaduce lncludingi nterm ediate Reduce ● Continuous checkpointing [aaduce RecoveringfromFailure      X Map S Simplyrecompute Iostresult Reduce ● Localized effect

Exploring ParallelComputationl  Models Map/Reduce SETI@home Threads ● PRAM LowCommunication High Communication Coarse-Grained Fine-Grained Map/Reduce Provides Coarse-Grained Parallelism Computationdone byindependentprocesses Yile-based communication Relatively "natural" programmingmodel Research issue to explore full potential and limits
