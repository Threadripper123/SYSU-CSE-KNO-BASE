# 第5章第3讲-DRAM刷新+Cache基本原理-图集

> 主题库：计算机组成原理　｜　源文件：第5章第3讲-DRAM刷新+Cache基本原理-图集.pdf　｜　2,598 字符　｜　22 个片段　｜　更新于 2026-11-12
000)

Recevea res RA from CPU ls block No Access main containing RA memory for block in cache containingRA YES Fetch RA word Allocate cache and deliver slot for main toCPU memoryblock memory block DelverRAwor ToCPU into cache slot す DONE

Validbits TAG DATA CPU A MemAI MAIN MEMORY Meml B

直接映射的Cache组织示意图主存储器 0块 Cache 1块标槽号 0块群 0槽 1槽 15块  16块 - 17块 15 槽 1块群 7位 31块主存地址 7位 4位 9位 2032块主存标记 Cache槽号块内地址 2033块 127块群主存号   Cache地址 ：  2047块

全相联映射的Cache组织示意图 Cache 主存储器标记槽号 0块 0槽 1块 1槽  - 15块 15槽 11位主存地址 2047块 11位 9位标记-主存块号块内地址

31 4 0 Ex:0x01 CacheTag   Cache Data

1 valid bit i t tag bits B = 2 bytes per line per line per cache block valid tag 0 1 B_ valid tag 0 1 B valid tag 0 1 B S = 2s sets valid tag 0 1 B valid tag 0 1 B set S-1: valid tag 0 1 B Cache size: C = B x E x S data bytes

组相联映射的Cache组织示意图主存储器 0块  Cache 1块 0组标记 0槽  0组群标记 1槽 7块标记 2槽  1组 8块标记 3槽 9块 - 门 1组群 - - 15块标记 14槽  7组 …  标记 15槽 8位 3位 9位  255组群标记组号块内地址 2046块 2047块主存地址20位

One-way set associative Block Tag Data 0 1 2 3 4 5 6

Two-waysetassociative Set Tag I Data Tag [ Data 2 3

Address of memory Hit Contents ofcache blocks afterreference blockaccessed  ormiss 0 1 2 3 0 miss Memory[o] 8 miss Memory[8] 0 miss MemoryfO] 6 miss Memory[O] Memory[6] 8 miss Memory[8] Memory[6]

Address ofmemory Hit Contents of cache blocks after reference block accessed ormiss Set0 Set0 Set1 Set1 0 miss MemoryO] 8 miss MemorylO] Memory[8] 0 hit Memory[O] Memory[8] 6 miss Memory[O] Memory[6] 8 miss Memory[8] Memory[6l]

Address ofmemory Hit Contents of cache blocks after reference blockaccessed ormiss Block0 Block1 Block2 Block3 o miss emory[O] 8 miss Memory[0] Memory[8] 0 hit Memory[] Memory[8] 6 miss Memory[0] Memory[8] Memory[6] 8 hit Memory[0] Memory[8] Memory[6]

大

漫大宁立国

2 周期序号 49487.49488 49489.49490 14999950000 存储器状态访存刷新地址序号任意 1 2 511 512 周期数 49488个周期 512个周期 -50000周期（8ms） （a）集中式刷新操作周期分配图存储器状态访存刷新访存刷新地址序号任意 1 任意 512 - 15465ns  160ns- 15465ns 十 160ns 8ms/512(15625ns). 8ms/512(15625ns) 8ms （b）分散式刷新操作周期分配图存储器状态一个访存周期一个刷新操作周期一个访存周期一个刷新操作周期任意 1 任意 512 -160ns. 160ns. --160ns -160ns- 系统访存周期系统访存周期 -512个系统访存周期 （c）透明式刷新操作周期分配图图5.34刷新操作周期分配图

Capacity Latency Cost Register <1KB 1ns $$$$ SRAM 1MB 2ns $$$ DRAM 1GB 10nS $ Hard disk* 100GB 10ms $ Want 1006B 1ns cheap *non-volatile

爱编程不爱bug 爱生活不爱黑眼圈

o

[a型存取n间典型e量 a存器 1KB 主存储器RAM和ROM) 512MB-4GB  后备存储器(磁帶库、光盘库） 10TB-100TB  non-volatile

寄存器主存储器(RAM和rOM 后备存储器(磁带库、光盘库)

5.2.3DRAM的刷新 DRAM刷新的有关参数 a、 刷新周期 Tre(refreshCycle 对同一存储位元连续两次刷新， 仍能保证鉴别出原存信息的最大允许间隔时间，即在T内必须对每个单元刷新一遍一般为 ms级，亦称为刷新间隔时间在Trc时间内，应刷新存储芯片中的所有存储位在DRAM 芯片的主要性能参数中，都给出 Trc。i 通常为2ms、 4ms、 amS [刷新周期vS.信息保持时间，哪个 T 大？ 计算机原理
