# 第5章第4讲-Cache映射与性能影响-图集

> 主题库：计算机组成原理　｜　源文件：第5章第4讲-Cache映射与性能影响-图集.pdf　｜　2,458 字符　｜　13 个片段　｜　更新于 2026-11-12
CPU Regs L1d-cache L2 MaIN Unified L1 i-cache Cache Memory Disk

-32KB, 4-way L1 $I -32KB, 8-way L1 $D -256KB, 8-wayL2 -8 MB, 16-way L3

32KL1 32KL1 32KL1 32KL1 I-cache I-cache I-cache I-cache 32KL1 32K1 32K1T 32KL0 D.cacho D-cacha D.cache Dcedha 256K 256K 256K 256K L2 L2 L L2 8MBLastLevelCache NehalemCorei7处理器缓存结构图

Type Whatcached Wherecached Latency(cycles Managed by CPU registers 4-byte word On-chip CPU registers 0 Compiler TLB Address translations On-chip TLB 0 Hardware L1 cache 32-byte block On-chip L1 cache 1 Hardware L2 cache 32-byte block Off-chip L2 cache 10 Hardware Virtual memory 4-KB page Main memory 100 Hardware + Os Buff er cache Parts of fi les Main memory 10o Os Network buffer cache Parts of fi les Local disk 10,0oo,000 AFS/NFS client Browsercache Webpages Localdisk 10,000,000 Web browser Web cache Web pages Rem ote server disks 1,000,000,000 Web proxy server

大

Block size vs. miss rate Cache size H&P:Figure 5.11 25 (bytes) 20. -1k Missrate 15. -4k -16k 10. 64k 5 -256k 0 16 32 64 128 256

15% 1KB 12% 4KB 6% 8KB 3% 16KB  32KB  64KB 128KB One-way Two-way Four-way Eight-way

漫大宁立国

000)

ON REFERENCE TO Mem[X]: Look forX among tags... HIT: X==TAG(i),forsomecachelinei WRITE: changeDATA[i];Start Write to Mem[X] MISS:X not found in TAG ofany cache line REPLACEMENTSELECTION: Select some linek to hold Mem [X] READ: ReadMem[] SetTAG[kj=X, DATA[k]=Mem[X] WRITE: Start Writeto Mem[X] SetTAG[k]=X,DATA[k]=newMem[X]

ON REFERENCE TO Mem[X]: Look forX among tags.. HIT: X= TAG(i),forsomecachelineI WRITE: changeDATA); MISS: X not found in TAG of any cache line REPLACEMENTSELECTION: SelectsomelinektoholdMem[X] Write Back: Write Data(k)to Mem[Tag[k]] READ: ReadMem Set TAG[k]=X, DATA[k]= Mem[X] WRITE:    SetTAG[k]=X,DATA[k]=newMem[X]

Write-backw/"Dirty" bits DV TAG DATA CPU A MemA MAIN 0 MEMORY E MemB ON REFERENCE TO Mem[X]: Look forXamong tagS.. HIT: X = TAG(i),forsomecachelineI READ: returnDATA(i) WRITE: changeDATA(i); var iic ]D[=1 V e MISS:X not found in TAG of any cache line REPLACEMENTSELECTION Select some line k to hold Mem[X] If DIk]==1(Write Back) Write Data(k) to Mem[Tag[k]1 READ: ReadMem[X];SetTAG[k]=X, DATA[k]=Mem[X],D[k]=O WRITE: CvaI M D[]=1 TEVC Set TAG[k]= X, DATA[k]=newMem[X]

回顾- — 5.3.2Cache(高速缓存）是什么样的？  SEUN Cache 是小容量、高速缓冲存储器，由SRAM 组成一般将Cache和主存的存储空间都划分为若干大小相同的块（主存中称为：块Block、 Cache中称为： 行line） 实现Cache机制需要解决的问题主存块和 Cache 之间如何映射？ 给出的主存地址怎么样转换为Cache 地址？ ： 写数据时，怎样保证Cache 和MM一致性？ 》（ Cache已满时，怎么办？ 计算机原理
