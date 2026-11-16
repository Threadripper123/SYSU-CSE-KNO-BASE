# DS-CH-10-文件系统-图集

> 主题库：分布式系统　｜　源文件：DS-CH-10-文件系统-图集.pdf　｜　2,637 字符　｜　24 个片段　｜　更新于 2026-11-11
天河天天二号天号

80

中算机院院 S SUNYAT-SENUNIVERSITY SCHOOLOFCOMPUTERSCIENCEANDENGINEERING

Help

1.文件移到客户上客户服务器客户服务器 [Ta 文 [文客户请求问远程文件文件保存 2.在客户上执行访问 3.当客户完成时， [Tabe _ __ e (a) (b) 图 11.1 (a)远程访问模型；(b)上载/下载模型

Client Server Yystemcalllaye Systemcalllay (VFS) layer (VFS) layer Local fi le Local file system interface NFS client NFS server system interface RPC client RPC server stub stub

操作版本3 版本4 描述 create 有无创建一个常规文件 create 无有创建一个非常规文件 link 有有创建一个文件的硬链接 symlink 有无创建一个文件的符号链接 mkdir 有无在给定目录下创建一个子目录 mknod 有无创建个特殊文件 rename 有有更改文件名 remove 有有从文件系统中删除一个文件 rmdir 有无从一个目录中删除一个空的子目录 open 无有打开一个文件 close 无有关闭一个文件

[Tablaa _ T e a b c d e a b a b a b a b c d e c e c d c d a b c d e a e e (a) (b) 图11.4 用于分布文件的两种方法之间的区别： （a）把整个文件分布在多个服务器上；（b）对文件进行带区划分以进行并行访问

cYile name, chun inde GFS client Master contact address ana ructaens Ctunk-seRee state ChunkID, range Chunk server Chunk server Chunk server Caun data Linux fi le Linux file Linux file system system system

[ab文件系统部点文件系统层 lvy lvy vy 面向块的存储器 DHash DHash DHash DHT层 Chord Chord Chord 络图11.6 Ivy分布式文件系统的组织结构

Client Server Client Server LOOKUP OPEN LOOKUP READ Lookup name Lookup name Open fi le READ Raaad ile aa [Raaad ile aa Time Time (a) (b)

Client Client Invalidate, Reply Invalidate, Reply Server Serve Invalidate Reply Invalidate Reply Client Client Time Time (a) (b)

客户A 服务器客户 B remote bin users work bin vu steen me mbox mbox mbox 由客户挂载由客户挂载的输出目录的输出目录络图11.11 在NFS中挂载远程文件系统（的一部分）

输出的目录包客户含输入的子目录服务器A 服务器B bin/ packages / 客户从服务器A输 draw 入目录 draw ntal instal B输入目录 nstal 网络客户需要从服务器图11.12 从NFS.中的多个服务器挂载嵌套的目录

客户机服务器计算机 —1. 查找 “/home/alice" 3.挂载请求 NFS客户自动挂载器 alice 2.创建子目录“alice” [T本b文件se 口 home ALICE 4.从服务器挂载子目录 “ alice” 图11.131 简单的NFS自动挂载器

接合点描述 GNS接合点引用另一个GNS实例逻辑文件系统名称引用将在定位服务中查找的子树逻辑文件名称引用将在定位服务中查找的文件物理文件系统名称引用可直接远程访问的子树物理文件名称引用可直接远程访问的文件图11.15 GNS中的接合点

客户机#】 a b 进程A abc 2.写人“c” 1.读取“ab” 文件服务器原始文件单个机器 ab ab 进程A 3.读取“ab” a b c 客户机#2 进程B ab 进程B 1.写入“c” 2.读取“abc” (a) (b) 图 11.16

方法释 UNIX语义一个文件上的每个操作对所有进程都是即时可见的会话语义在文件关闭之前，所有改动对其他进程都是不可见的不可改变的文件不允许更新文件：简化了共享和复制 [a 有动图11.174 种处理分布式系统中的共享文件的方法

操作描述 lock 为--定范围的字节创建锁 lockt 测试是否授予了相冲突的锁 locku 删除一定范围的字节上的锁 renew 继续租用指定的锁图11.18 NFS版本4中关于文件锁定的操作

[Tablab ____ a NONE READ WRITE BOTH READ 成功失败成功失败请求访问 WRITE 成功成功失败失败 BOTH 成功失败失败失败 (a) [Tlbe_ 文t__ea NONE READ WRITE BOTH READ 成功失败成功失败当前访问状态 WRITE 成功成功失败失败 BOTH 成功失败失败失败 (b) （a）在当前拒绝状态下，客户请求共享访问时的情况；（b）在当前文件访问状态下，客户请求拒绝状态时的情况

存储器客户应 NFS 缓存用程序服务器石盘缓存络图11.21 NFS中的客户端缓存

请求准备前准备提交应答客户主机副本副本副本时间图 11.26 Byzantine容错性中的不同阶段

2.2上 FRE 1.8 1.6 1.45 0.2 0.4 1 0.6 0.8 结点可用性图11.27 将比率rrep/rec作为结点可用性a的函数

客户服务器 [Thb e e用访问访向控制控制本地文件系统接口 NFS客户 NFS服务器本地文件系统接口安全信道 RPC 存 RPC服器存图11.281 NFS安全性体系结构
