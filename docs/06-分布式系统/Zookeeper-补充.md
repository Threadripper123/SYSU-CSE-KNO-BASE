# Zookeeper-补充

> 主题库：分布式系统　｜　源文件：Zookeeper-补充.pdf　｜　11,961 字符　｜　42 个片段　｜　更新于 2026-11-11


ZooKeeper
1
Claudia Hauff

ZooKeeper
•
Developed at Yahoo!Research
•
Started as sub-project of Hadoop, now atop-level  
Apacheproject
•
Development is driven by applicationneeds
A highly-available service forcoordinating  
processes of distributedapplications.
http://zookeeper.apache.org/
2

ZooKeeper in theHadoop  
ecosystem
HDFS
Pig  
(DataFlow)
Hive  
(SQL)
Sqoop
(DataTransfer)
Z
oo
K
ee
p
er 
(
Coo
r
dina
t
i
o
n
)
A
v
r
o 
(
Ser
ia
l
iza
t
i
o
n
)
MapReduce (JobScheduling/Execution)
HBase (ColumnDB)
3

Coordination
Propercoordination  
is noteasy.
4

5
Fallacies ofdistributed  
computing
•
The network isreliable
•
There is nolatency
•
The topology does notchange
•
The network ishomogeneous
•
The bandwidth isinfinite
•
...

Motivation
•
In the past: a single program running on asingle
computer with a singleCPU
•
Today: applications consist of independent programs  
running on a changing set ofcomputers
•
Difficulty: coordination of those independentprograms
•
Developers have to deal with coordination logicand
application logic at the sametime
9
ZooKeeper: designed to relieve developersfrom  
writing coordination logiccode.

Lets think....

Question: how do you elect theleader?
applicationlogic
A program that  
crawls theWeb
one machine (the leader)  
should coordinate theeffort
coordinationlogic
a cluster witha
few hundredmachines
11

Question: how do you lock aservice?
a cluster witha
few hundredmachines
12
applicationlogic
A program that  
crawls theWeb
The progress of thecrawl  
is stored in a DB: who  
accesses what &when?
coordinationlogic
onedatabase

Question: how can the configurationbe  
distributed?
a cluster witha
few hundredmachines
13
applicationlogic
A program that  
crawls theWeb
Every workershould  
start with the same  
configuration
coordinationlogic
configurationfile

Solutionapproaches
•
Be specific: develop a particular service foreach  
coordinationtask
•
•
•
Lockingservice
Leaderelection
etc.
•
15
Be general: provide an API to make many services  
possible
ZooKeeperTheRest
API thatenablesapplication 
developers to implement  
their own primitiveseasily
specificprimitivesare 
implemented on the  
serverside

How can adistributed  
system looklike?
Slave
SlaveSlaveSlave
MASTER
scalability
-
+
-
-
simple
coordination performed by the master  
single point offailure

How can adistributed  
system looklike?
scalability is still anissue
-
+
not a single point of failureanymore

How can adistributed  
system looklike?
+scalability

What makes distributed  
system coordinationdifficult?
Partial failures make application writingdifficult
message
nothing comesback
Sender does notknow:
•
•
whether the message wasreceived
whether the receiver’s process diedbefore/after  
processing themessage
19
networkfailure

Typical coordinationproblems  
in distributedsystems
•
Static configuration: a list of operational parameters for the  
systemprocesses
•
Dynamic configuration: parameter changes on thefly
•
Group membership: who isalive?
•
Leader election: who is in charge who is abackup?
•
Mutually exclusive access to critical resources(locks)
•
Barriers (supersteps in Giraph forinstance)
20
The ZooKeeper API allows us to implement all these coordination  
taskseasily.

ZooKeeperprinciples

ZooKeeper’sdesign  
principles
•
API iswait-free
•
•
•
No blocking primitives in ZooKeeper  
Blocking can be implemented by aclient  
Nodeadlocks
•
Guarantees
•
•
Client requests are processed in FIFOorder  
Writes to ZooKeeper arelinearisable
•
Clients receive notifications of changes beforethe  
changed data becomesvisible
Remember the  
diningphilosophers, 
forks &deadlocks.
18

ZooKeeper’s strategy tobe  
fast andreliable
•
ZooKeeper service is an ensemble of serversthat  
use replication (highavailability)
•
Data is cached on the clientside:
•
What if a new leader iselected?
•
•
Potential solution: polling (notoptimal)
Watch mechanism: clients can watch foran
update of a given dataobject
Example: a client caches the ID of the currentleader
instead of probing ZooKeeper everytime.
ZooKeeper is optimisedfor  
read-dominantoperations!
19

20
ZooKeeperterminology
•
Client: user of the ZooKeeperservice
•
Server: process providing the ZooKeeperservice
•
znode: in-memory data node in ZooKeeper,  
organised in a hierarchical namespace (the datatree)
•
Update/write: any operation which modifies thestate  
of the datatree
•
Clients establish a session when connectingto  
ZooKeeper

ZooKeeper’s datamodel:  
filesystem
•
znodes are organised in a hierarchicalnamespace
•
znodes can be manipulated by clients throughthe  
ZooKeeperAPI
•
znodes are referred to by UNIX style filesystem
paths
/
/app2/app1
/app1/p_3/app1/p_1/app1/p_2
All znodes store data (file like) & canhave
children (directorylike).
21

znodes
•
znodes are not designed for general datastorage  
(usually require storage in the order of kilobytes)
•
znodes map to abstractions of theclient  
application
Group membership protocol:  
Client process pi creates znode p_i  
under/app1.
/app1persists as long as the process  
isrunning.
/
22
/app2/app1
/app1/p_3/app1/p_1/app1/p_2

znodeflags
•
Clients manipulate znodes by creatingand
deletingthem
•
EPHEMERAL flag: clients create znodeswhich  
are deleted at the end of the client’ssession
•
SEQUENTIAL flag: monotonicallyincreasing  
counter appended to a znode’spath;
counter value of a new znode under a parentis  
always larger than value of existingchildren
ephemeral (Greek): passing,short-lived
/app1_5
/app1_5/p_1
/app1_5/p_2
/app1_5/p_3
create(/app1_5/p_,data,SEQUENTIAL)

24
znodes & watchflag
•
Clients can issue read operations on znodes witha  
watchflag
•
Server notifies the client when the information on the  
znode haschanged
•
Watches are one-time triggers associated with a  
session (unregistered once triggered or sessioncloses)
•
Watch notifications indicate the change, not thenew  
data

25
Sessions
•
A client connects to ZooKeeper and initiatesa  
session
•
Sessions have an associatedtimeout
•
ZooKeeper considers a client faulty if it does not  
receive anything from its session for more than that  
timeout
•
Session ends: faulty client or explicitly endedby  
client

A few implementationdetails
30
ZooKeeper data is replicated on each serverthat  
composes theservice
replicatedacross  
allservers
(in-memory)
write request requires  
coordination betweenservers
updates first  
logged to disk;  
write-aheadlog  
and snapshot  
forrecovery
Source:http://bit.ly/13VFohW

A few implementationdetails
•
ZooKeeper server servicesclients
•
Clients connect to exactly one server tosubmit  
requests
•
•
read requests served from the localreplica
write requests are processed by anagreement  
protocol (an elected server leader initiates  
processing of the writerequest)
31

Lets workthrough  
someexamples

ZooKeeperAPI
•
String create(path, data,flags)
•
creates a znode with path name path, stores data in it and setsflags  
(ephemeral,sequential)
•
void delete(path,version)
•
deletes the anode if it is at the expectedversion
•
Stat exists(path,watch)
•
watch flag enables the client to set a watch on theznode
•
(data, Stat) getData(path,watch)
•
returns the data and meta-data of theznode
•
Stat setData(path, data,version)
•
writes data if the version number is the current version of theznode
•
String[] getChildren(path,watch)
Nopartialread/writes  
(noopen,seekor  
closemethods).
29
Note: no  
createLock()
or similarmethods.

Example:configuration
Questions:
1.How does a new worker queryZK  
for aconfiguration?
2.How does an administratorchange
the configuration on thefly?
3.How do the workers read thenew
configuration?
/
/app2/app1
/app1/config
/app1/progress
[configuration stored in/app1/config]
1.getData(/app1/config,true)
2.setData(/app1/config/config_data,-1)  
[notify watchingclients]
3.getData(/app1/config,true)
String create(path,data,flags)
void  
Stat
delete(path,  
exists(path,
version)  
watch)
(data,Stat)getData(path,watch)
StatsetData(path,data,version)
•
•
•
•
•
•
String[]getChildren(path,watch)
appconfiguration
30

Example:group
membership
/
/app1/workers/worker1
/app1
/app1/workers
/app1/workers/worker2
Stringcreate(path,data,flags)
void  
Stat
delete(path,  
exists(path,
version)  
watch)
(data,Stat)getData(path,watch)
StatsetData(path,data,version)
•
•
•
•
•
•
String[]getChildren(path,watch)
Questions:
1.How can all workers (slaves) of an  
application register themselves onZK?
2.How can a process find out aboutall
active workers of anapplication?
[a znode is designated to storeworkers]
1.create(/app1/workers/  
worker,data,EPHEMERAL)
2.getChildren(/app1/workers,true)
31

Example:  
simplelocks
36
/
/app1
/app1/workers/worker1
/app1/workers
/app1/workers/worker2
/app1/lock1
all processes compete at all times for thelock
Question:
1. How can all workers of an application use a single resourcethrough  
alock?
•
Stringcreate(path,data,flags)
•voiddelete(path,version)
•Statexists(path,watch)
(data,Stat)getData(path,watch)
StatsetData(path,data,version)
•
•
•
String[]getChildren(path,watch)
ok?
use lockedresource
create(/app1/lock1,...,EPHE.)
getData(/app1/lock1,true)
yes

Example:
locking without herdeffect
37
/
/app1/locks/lock_1/app1/locks/lock_2
id=min(ids)?
exit (uselock)
id=create(/app1/locks/lock_,SEQ.|EPHE.)
yes
ids =getChildren(/app1/locks/,false)
/app1
no
/app1/locks
exists(max_id<id,true)
wait fornotification
Question:
1. How can all workers of an application use a single resourcethrough  
alock?

Example:  
leaderelection
38
/app1
/app1/workers/leader
/app1/workers
/app1/workers/worker1
yes
ok?
follow
create(/app1/workers/leader,IP,EPHE.)
ok?
yes
lead
getData(/app1/workers/leader,true)
/
no
if the leader dies, elect again (“herdeffect”)
Stringcreate(path,data,flags)
void  
Stat
delete(path,  
exists(path,
version)  
watch)
(data,Stat)getData(path,watch)
StatsetData(path,data,version)
•
•
•
•
•
•
String[] getChildren(path,watch)
Question:
1. How can all workers of an application elect a leaderamong  
themselves?

ZooKeeper  
applications

36
The Yahoo! fetchingservice
•
Fetching Service is part of Yahoo!’s crawlerinfrastructure
•
Setup: master commands page-fetchingprocesses
•
•
Master provides the fetchers withconfiguration
Fetchers write back information of their status andhealth
•
Main advantage ofZooKeeper:
•
•
Recovery from master failures  
Guaranteed availability despitefailures
•
Used primitives of ZK: configuration metadata,leader  
election

37
Yahoo! messagebroker
•
A distributed publish-subscribesystem
•
The system manages thousands of topics that clients  
can publish messages to and receive messagesfrom
•
The topics are distributed among a set of serversto  
providescalability
•
Used primitives of ZK: configuration metadata(to  
distribute topics), failure detection and group  
membership

Yahoo! messagebroker
ephemeralnodes
primary andbackup  
server per topic;  
topicsubscribers
monitoredby  
allservers
Source:http://bit.ly/13VFohW
38

Throughput
onlyread
re
quests
onlywrite  
requests
crossingeventually  
alwayshappens
Setup: 250 clients, each client has at least100  
outstanding requests (read/write of 1Kdata)
Source:http://bit.ly/13VFohW
39

Recovery fromfailure
44
Setup: 250 clients, each client has at least100  
outstanding requests (read/write of 1Kdata);
5 ZK machines (1 leader, 4 followers), 30%writes
(1)failure & recoveryof  
afollower
(2)failure & recoveryof  
a differentfollower
(3)failure of theleader
(4)failure of followers  
(a,b), recovery at(c)
(5)failure of theleader
(6)recovery ofthe  
leader
Source:http://bit.ly/13VFohW

References
•
[book] ZooKeeper by Junqueira & Reed,2013  
(available on the TUD campusnetwork)
•
[paper] ZooKeeper: Wait-free coordination forInternet-
scale systems by Hunt et al., 2010; http://bit.ly/ 
13VFohW
41

42
Summary
•
Whirlwind tour throughZooKeeper
•
Why do we needit?
•
Data model of ZooKeeper:znodes
•
Example implementations of different coordination  
tasks
