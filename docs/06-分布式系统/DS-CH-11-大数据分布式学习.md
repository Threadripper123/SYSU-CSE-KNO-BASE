# DS-CH-11-大数据分布式学习

> 主题库：分布式系统　｜　源文件：DS-CH-11-大数据分布式学习.pdf　｜　1,586 字符　｜　110 个片段　｜　更新于 2026-11-11


Distributed Systems
分布式系统
陈鹏飞
计算机学院
chenpf7@mail.sysu.edu.cn
办公室：学院楼310
主页：http://sdcs.sysu.edu.cn/node/3747

2
第十一讲
—
大数据
+
分布式机器学习

3
分布式文件系统

4
补充内容
Milestones in Distributed File Systems

5
补充内容
Milestones in Distributed File Systems

6
补充内容
Milestones in Distributed File Systems

7
补充内容
Milestones in Distributed File Systems

8
补充内容
Milestones in Distributed File Systems

9
补充内容
Milestones in Distributed File Systems
http://www.xrootd.org/

10
补充内容
Milestones in Distributed File Systems

11
补充内容
What’s the next?

12
补充内容

13
大数据分析

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

36

37

38

39

40

41

42

43

44

45

46

47

48

49

50

51

52

53

54

55

56

57

58
分布式机器学习

59

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
Key problems
1.Accessing the parameters requires an enormous
amount of network bandwidth.
2.Many machine learning algorithms are sequential.
3.At scale, fault tolerance is critical.

75
Architecture

76

77

78

79

80

81

82

83

84

85

86

87

88

89
Consistent Hashing
The parameter server partitions keys much as a conventional distributed hash table does. K
eys and server node IDs are both inserted into the hash ring.

90

91
深度学习

92

93

94

95

96

97

98

99

100

101

102

103

104

105

106

107

108

109
分布式深度学习平台
1. floydhub: https://www.floydhub.com/;
2. OpenPAI
：
https://github.com/Microsoft/pai;
3. PaddlePaddle
：
http://www.paddlepaddle.org/;
4. PiplineAI
：
https://pipeline.ai/;
5. Kubeflow
：
https://www.kubeflow.org/;
6. Polyaxon
：
https://polyaxon.com/;

110
谢谢！
