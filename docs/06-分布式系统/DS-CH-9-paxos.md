# DS-CH-9-paxos

> 主题库：分布式系统　｜　源文件：DS-CH-9-paxos.pdf　｜　1,220 字符　｜　85 个片段　｜　更新于 2026-11-11


Distributed Systems
分布式系统
陈鹏飞
计算机学院
chenpf7@mail.sysu.edu.cn
办公室：学院楼310
主页：http://sdcs.sysu.edu.cn/node/3747

2
第九讲
—
分布式系统共识
Slides come from drdr.xp@gmail.com, Amir H. Payberah, JinyangLi 

3
背景
Ø分布式系统协作
多个部分协同工作，对外表现出一个整体；
Ø一致性
数据对象在不同副本之间一致；
Ø共识
多个节点对于某件事情比如执行某个指令达成一致；

4
背景
Basically Available、
Soft-state、Eventual 
Consistency  (BASE)

5

6
3
http://jepsen.io/consistency

13
共识
 Paxos

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35
希腊岛屿Paxon 上的执法者（legislators，后面称
为牧师priest）在议会大厅（chamber）中表决通
过法律，并通过服务员传递纸条的方式交流信息，
每个执法者会将通过的法律记录在自己的账目（l
edger）上。问题在于执法者和服务员都不可靠，
他们随时会因为各种事情离开议会大厅，并随时
可能有新的执法者进入议会大厅进行法律表决，
使用何种方式能够使得这个表决过程正常进行，
且通过的法律不发生矛盾。

36

37

38
Multiple 

39

40

41
Paxos定义

42
Paxos定义

43
典型 Paxos：阶段1 

44
典型 Paxos：阶段1 

45
典型 Paxos：阶段2 

46
典型 Paxos：阶段2 

47
例子： 无冲突

48
例子2： 解决并发写冲突

49
例子3：X 不会修改确定的 V 

50
其他

51

52

53

54

55
Paxos: 种类

56
Multi Paxos

57
其他变种
http://paxos.systems/variants.html

58
RAFT(Replicated And Fault Tolerant)

59
简洁

60

61

62

63

64

65

66

67

68

69

70

71

72

73

74

75

76
PBFT

77
PBFT

78
PBFT

79
PBFT

80
PBFT

81
PBFT

82
PBFT

83
PBFT

84
PBFT

85
PBFT

86
PBFT

87
PBFT

88
PBFT

89
PBFT

90
PBFT

91
谢谢！
