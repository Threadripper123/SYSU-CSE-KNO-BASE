# Zookeeper-补充-图集-矢量

> 主题库：分布式系统　｜　源文件：Zookeeper-补充-图集-矢量.pdf　｜　1,256 字符　｜　22 个片段　｜　更新于 2026-11-13
MapReduce (Job Scheduling/Execution) HBase (Column DB)

HDFS

(Data Transfer)

Hive (SQL)

Aprogram th at crawls the Web

application logic

application logic

A program  that crawls the Web

application logic

MASTER Slave Slave Slave Slave

app1/r /appl/p

app1

app cre

replicated across all   servers ice (in-memory) → Replicated Database xn updates first

app configuration

Stat setData(path, data Vers Questions: String[l getChildren(path, wa 1.How Can allworkers (slaves) ofan   application register them selves on ZK ? 2. How cCan 0ut about all aprocess find active Workers ofan application? /apP a znode i s designated tL o s t ore workers] 1.Create e(/appl/workers/ wOrker data,EPHEMERAL)  /appl/workers 2. getChildren(/appl/workers,true)

Create(/appl/lockl,.,EPHE.) /appl yes /appl/woi ok?  use  locked resource /appl/workers/workerl /appl/wo all processes compete at all times for the lock

/aPPI appl/workers

id=create(/appl/locks/lock ,SEQ.|EPHE.) getChildren(/appl/locks/,false) YES id=min(ids)? → exit (use lock) NO exists(maxid<id,true) lappl/locks/lock wait for notification

elock) /APPI /appl/locks

getData(/appl/workers/leader,true ok? follw yesl create(/appl/workers/leader, IP, EPHE.) no ok? lead /appl  YeS

/aPPI /appl/workers
