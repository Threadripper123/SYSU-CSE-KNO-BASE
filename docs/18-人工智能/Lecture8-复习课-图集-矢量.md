# Lecture8-复习课-图集-矢量

> 主题库：人工智能　｜　源文件：Lecture8-复习课-图集-矢量.pdf　｜　954 字符　｜　20 个片段　｜　更新于 2026-11-13
神经网络 1 0.73 2 0.72 3 0.51 0 -2 - -1 -1 0.5 -2 0.12 -1 0.85 1 -1 4 0 2 1 1 0621 701 05

Property 1 一些特征会比整张图片小很多 Property 2 相同特征会出现在不同区域 Property 3 像素二次采样不会改变识别的目标信息 Flatten

1 0 0 0   0 1 0 1 0 0 1 0 0 0 1 1 0 0 1 0 0 0 1 0 0 1 0 0 1 0 0 0 1 0 1 0

-1 1 -1 -1 1 -1 -1 1 -1

1 -1 -1 -1 1  -1 -1 -1 1

1 0 0 0 0 1 0 1 0 1 0 1 0 0 0 1 1 0 0 1 0 0 0 1 0 0 1 0 0 1 0 0 0 1 0 1 0

1 -1 -1 -1 1 -1 -1 -1 1

3 -1

00 0 0 1    0 0 0 1 0   20 0 0 /1 1 0 0  1 0 0 0 10 0 0 0 0 1 0 0 0 0 1 0

-1 -1 2

3 -1      -3  -1 -3 0 -3

-1 1  1 卷积核1  1 0 0 0 -3 0 -1   1 0 0 1  -3 0 0 1 1 0 3 1 0 0 0 1 0 1 0 0 1 0 0 0 1 0 1 0

共享权重

1 0 0 0 0 1   0 1 0 0 1 0 Conv 0 0 1 1 0 0 1 0 0 0 1 0 0 1 0 0 1 0 Max 0 0 1 0 1 0 Pooling

-1 1

0 3

a1 a2 a3 a1 a x1 x2 ×3 arrive  on May 31th

特殊神经元： 4项输入， [auneutGate 1项输出 Memory Forget 遗忘门 Cell Gate 控制信号 anputGate

cral loss on training data de large small odel loss on te ias optimization large

zation looool tco g uaa large small 9Y
