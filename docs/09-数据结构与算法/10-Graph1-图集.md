# 10-Graph1-图集

> 主题库：数据结构与算法　｜　源文件：10-Graph1-图集.pdf　｜　1,732 字符　｜　31 个片段　｜　更新于 2026-11-11
SOCIAL ∩ETWORK DESIGN

公司宽带公共产品产品 111.113.116 VLan4 客服  E VLan6 115,118,108 PVID4 PVID6 124,131-136 海蜘蛛路由器 VLan1 1.100 VLan1 VLan4 PVID1 PVID1 H3C百兆二层网管交换机 PVID4 公共 1.210  会议室 VLan1-10 VLan1-10 E  PVID1 PVID1 VLan1 PVID1 Tunk4,6,7,8 公共服务器华为全千兆三层网管交换机  E E VLan 1 VLan1 190,192,打印1.11 1.20 PVID 1 仓库宽带公司Wiif PVID1 Vlan2- 1.15 无线网桥 PVID2 VLan3 公司到宿舍公司同事机无线网桥 PVID3 1.4 公司到仓库 1.7 VLan1-10 PVID1 8 仓库无线网桥营销部01 Trunk3,5 1.6 公司同事手机 144-146  宿舍02无线路由猫营销部02 1.8 仓库无线路 161,163,165,166 公司同事平板宿舍01无线网由猫 1.5 1.1 公司同事平板 H3C百兆二层网管交换机 .50 VLa PVID 宿舍宽带宿舍02同事平板 T 155,166 VLan1 宿舍01无线路由猫 VLan3 PVID1 VLan5 1.10 PVID3 PVID5 宿舍02同事手机仓库同事手机仓库办公电脑 61-65 e 营销部03 宿舍01同事笔记本人事 162,164 宿舍01同事手机宿舍01同事手机公共 126,门禁1.2

A A B A A B B C C O B C C D

子图 0 2 1 1 2 ②

1 2 2 1 2 3 3 3

VO V5 V6 V1 V2 V8 Vz V4 V3

VO V1 V5 V V2 V8 Vz V4 V3

Vo 3

V1 V2 V3

VO V1 V2. V: 3

V0 V1 V2 V0 2 V3_ V1 V5 V3 V4 V5 V4

cYa 1 2 3 2

[010001 10001 V2 A= 01010 10000 00010 V V 5

data firstarc adjvex nextarc Info

data adj dest link dest link B C B

dataadi dest limk A    0    dest link B 1 2 2 C A C  邻接表(出边表) data adi dest ink     逆邻接表 （入边表)

A 6  D dataadi    destcost link  1 3 5  9  0 A   1  2 I B B 2 C 8    (项点表) (出边表)

Vo 1 2 Vo 3 $V2 3 V2 $V3 0 V 2

d a Tstout

A 0 A B D 1 B

A E A 25 B 82 3 4 2 C 6 C 3 D CA

Firstin 终顶点 Firstout 始顶点廣出

0  10030 8 10   8 0 88 8 0 60 0 20  ∞  10 0 0 850 0

100 30 60 10 10 20 50

1  1 100 30   5 10        2      3 V3 3 20 2 60 4 4

Firstin： 终顶点入 Firstout 始台顶点， 出

B

Selectaallnriangles sharing this vertex

smallscale gaps and overlaps

V (V3

[图的基本概念 • 若代表一条边的顶点序偶是无序的(即该边无方向)，则称此图为 。 ・若代表一条边的顶点序偶是有序的(即边有方向)，则称此图为 。 • 通常用n表示图中顶点的数目，用e表示边或弧的数目。无向图中e的取值范围是从0到n(n- 1)/2， 有向图中e的取值范围是从0到n(n- 1） 无向图有向图 Data Structure & Algorithms 6
