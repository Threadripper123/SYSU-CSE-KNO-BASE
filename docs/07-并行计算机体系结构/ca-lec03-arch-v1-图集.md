# ca-lec03-arch-v1-图集

> 主题库：并行计算机体系结构　｜　源文件：ca-lec03-arch-v1-图集.pdf　｜　2,735 字符　｜　33 个片段　｜　更新于 2026-11-11
SUN YAT-SEN UNIVERSITY

计算阵列服务阵列 CPU CPU CPU CPU CPU     服务  + + +  节点 GPU GPU GPU  GPU GPU 互连通信子系统存储阵列云对象云对象云对象元数据存储存储存储 . 存储

384-端口交换机1 384-端口交换机i 384-端口交换机10 机架0 机架i 机架119

IBM 3

IEM IBM System 29

VAX11/780 THEORIGINALUNIT OFONE.

Embedded Computers

dreamse n i 27

[Tal Feature mobile device Desktop Server things/  (PMD) scale com puter em bedded Priceofsystem $100–$1000 $300–$2500 $5000–$10,000,000 $100,000–$200,000,000 $10–$100,000  Priceof $10-$100 $50-$500 $200-$2000 $50-$250 $0.01-$100 Criticalsystem Cost, energy, Price Throughput Price-performance, Price, energy designissues media performance, availability, throughput,energy application- performance, energy, graphics scalability, energy proportionality specific

指令 [处理器 —条据流数据单指令流单数据流（SISD）

[指e —条指令流处理器处理器处理器处理器处理器五条数据流数据

限

ARPA B6700 网接口外围设备 B6700 激光存多路开关储器 B6700 内存 48 B6700 CPU 487 48  CU 48 CDC BIOM 模式位线 64 CDB 控制线 128 PE63 PEO PEO PE1 PE63 64X8 DFS PEMO PEM1 PEM63 256 256 CU总线 1/Q总线 1024 实时 / 10S 。 1024 装置图8.5111iacIV系统总框图

PE56 PE57 PE63 PE63 PE00 PE01 PE07 PE08 PE07 PE08 PE09 PE15 PE16 PE55 PE56 PE57 PE63 PE00 PE00 PE01 PE07

移位 V7 逻辑运算 V6 加 V5 主 V4 向量 V3 功能 V2 部件 V1 存倒数 Vo 乘向量寄存器组控制加系 (8×64) VM  向量屏蔽 +公部件统控制后援寄存器标量寄存器 ↑ T0-T63 S0-S7 VL 向量长度

[指令五条指令流处理器处理器处理器处理器处理器条数据流数据

指令 [三条指e流处理器处理器处理器处理器处理器五条数据流据

INSTRUTIoNns i INTRUCTIONns strucin Stream da stream A Processor Processor a stream Processor SMD MMD INSTRUTIONs   i NSTRuCTIONs strucin I trems Processor Processor   Processor Processor  Processor    Processor Processor P Processor Processor rocessor Processor Processor

CPU CPU CPU CPU

CPU PU PU Memory Memory Memory caa Memory Memory Memory CPU CPU CPU

UMA

NUMA

CPU1 CPU2 CPU3 Core 1 Core N CoreN Core 1 CoreN Core 存储器 /O 存储器 1/0 存储器 1/O 总线存储器 /O 存储器 1/O Core 1 Core N Core 1 CoreN CPU4 CPU5

CPU1 CPU2 CPU3 Core CoreN Core CoreN 1 ... Core Core N 1   多级缓存多级缓存多级缓存存储器 /O 存储器 1/O 存储器 /O 总线存储器 /0 存储器 1/0 多级缓多级缓存 Core1 CoreN Core CoreN CPU4 CPU5

MPP节点1 MPP节点2 MPP节点3 I/O总线 Core CoreN CoreN Core1 Core CoreN 硬 [多级缓存多级缓存多级缓存本地存储器本地存储器本地存储器盘网络接口网络接口网络接口定制网络

COW节点1 COW节点2 COW节点3 硬盘硬盘硬盘 Core 1 Core N Core CoreN Core CoreN  .. 多级缓存多级缓存多级缓存本地存储器本地存储器本地存储器网络接口网络接口网络接口商用网络 (以太网、 ATM)

服务器机架机架交换机阵列交换机服务器

控制器输入设备运算器输出设备存储器

地址程序 （指令) CPU 同一个存储器数据 [

Input a Input b Input c f cYaaa l

ModernGPU (2006) Hypertteadtechnology (About2000) Static dataflowcompute (MIT,1974) Stream processor cEarlyof2lcentury Dynamic dataflow computer (Monsoon,1990)

4 B Abache  Beam 2015 unified programming ApacheFlin model Spark 2014 APAcHE Flink: programming model & hadoop systemfortheintegration MEpReduce 2010 of streaming&batch 2006 programmingmodel& MapReduce: system for complex processing

[传统计算机分类法 [计算机分模拟计算数字计算混合计算专用计算通用计算微型机小型机中型机大型机巨型机机机机机机
