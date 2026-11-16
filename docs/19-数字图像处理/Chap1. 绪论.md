# Chap1. 绪论

> 主题库：数字图像处理　｜　源文件：Chap1. 绪论.pdf　｜　8,095 字符　｜　64 个片段　｜　更新于 2026-11-13


第一章. 绪论
骆伟祺
中山大学计算机学院
Email：luoweiqi@mail.sysu.edu.cn
1

School of  Computer Science and Engineering
▪教程
✓中文版：（美）冈萨雷斯等著.数字图像处理（第四版）.
阮秋琦、阮宇智译.电子工业出版社出版.2020.
✓英文版：RafaelC.Gonzalez&RichardE.Woods,Digital
ImageProcessing(4th),2018
▪参考资料
✓工具书：DigitalImageProcessingUsingMATLAB,3rdEd.
Gonzalez,Woods,andEddins,2020
✓网站及论坛：http://www.imageprocessingplace.com
2

School of  Computer Science and Engineering
▪先修课程：程序设计基础、高等数学、复变函数、
概率统计、线性代数
▪编程工具：
▪编辑工具：
3

School of  Computer Science and Engineering
▪成绩评定
平时成绩：20%，作业+考勤（3次缺席直接记0分）
小项目：10%
小测试：10%
期末考试：60%  
▪助教+ 答疑：QQ群
4

School of  Computer Science and Engineering
1.基础知识
2.数字图像处理及其应用
第一章:绪论
5

School of  Computer Science and Engineering
1.基础知识
2.数字图像处理及其应用
第一章:绪论
6

School of  Computer Science and Engineering
•一图胜千言万语（A picture is worth a thousand words）
图像是人们从客观世界取得信息的重要来源：人类在接受的信息中，视
觉信息占75％，其他如听觉、味觉、触觉、嗅觉总的加起来不过占25％。
•眼见未必为全部
数字图像技术是人类视觉延续的重要手段：人的眼睛只能看到可见光部
分，而迄今为止人类发现可成像的射线已有多种, 扩大了人类认识客观世界
的能力。
1.图像& 数字图像
7

School of  Computer Science and Engineering
•什么是“图像”
图像是人类视觉的基础，是自然景物的客观反映，是人类认识世界和人类本身的
重要源泉。
✓分离定义：“图“是物体反射或透射光的分布，而“像“则是人的视觉系统所接
受的图在人脑中所形版的印象或认识。
同一个“图“，不同的人所产生的”像“可能是不一样的
✓函数定义：图像定义为一个二维函数푓(푥,푦) , 其中x 和y 是平面坐标, 任何一对
坐标（x,y）处的幅值f 称为图像在该点的灰度或颜色值（向量）。
▪什么是“数字图像”
从函数角度出发，当图像函数푓(푥,푦)的x, y 和푓(푥,푦)取值是离散且有限时，则
该图像称为“数字图像”。
数字图像是一个自变量(x, y) 和函数值f(x,y)都是离散且有限的二元函数！
1.图像v.s. 数字图像
8

School of  Computer Science and Engineering
1.数字图像
9
空间分辨率：512 ×512
O
1  2 ...                          511
1   2...                            511
X
Y
灰度值：푓(푥,푦)=60∈{0,1,...,255}  
......
......
矩阵表示
灰度图像

School of  Computer Science and Engineering
1.数字图像
10
分辨率：512 ×512
色彩值：f (x,y) = [60,65,120]
......
......
矩阵表示
彩色图像

School of  Computer Science and Engineering
1.Lena –数字图像处理的明星
11
1972年1997年
2015年
Lena是指刊于1972年11月号《花花公子》（Playboy）杂志上的一张裸体
插图照片的一部分，是一张大小为512 ×512 标准测试图。
该图在数字视频处理学习与研究中颇为知名，常被用作数字视频处理各
种实验，如压缩和降噪等。

School of  Computer Science and Engineering
1. 常见的成像设备
12
数码相机手机
扫描仪
激光扫描仪遥感卫星
CT机器

School of  Computer Science and Engineering
1. 其它成像方法
13
西班牙87岁老太用电脑画图工具创作

School of  Computer Science and Engineering
1. 其它成像方法
14
三维模型+光照三维模型+光照+材质
属于计算机图形学领域

School of  Computer Science and Engineering
1. 其它成像方法
15
数学方法：分型（自相似性）

School of  Computer Science and Engineering
1. 其它成像方法
16
GAN：生成对抗网络

School of  Computer Science and Engineering
1. 其它成像方法
17
Diffusion Model (扩散模型) +CLIP (Contrastive Language-Image Pre-training) 

School of  Computer Science and Engineering
1. 显示设备
18
显示器手机
投影仪
全息投影
绘图仪（转为模拟图像显示）

School of  Computer Science and Engineering
1. 标志性事件
19
1921年，由电报打印机采样
特殊字体产生的数字图像
1922年，通过光学还原电报
打印机产生的数字图像
1929年，产生15级
色调的图像
1964年，美国航天
器传输月球照片
2013年，“嫦娥3”，月
面高分辨率全彩照片
70年代初，第一台CT
（图像重构）
1920s，纽约-伦敦
海底电缆传输数字
化的新闻图片。传
递时间从一个多星
期减少到3个小时。

School of  Computer Science and Engineering
1.基础知识
2.数字图像处理及其应用
第一章:绪论
20

School of  Computer Science and Engineering
▪数字图像可用矩阵表示，数字图像处理则相当于对“矩阵”进行处理
2. 数字图像处理
21
成像
数字图像处理
存储与传输
分析
显示
数字图像处理研究涉及到了成像、存储与传输，显示、及分析等多个方面。

School of  Computer Science and Engineering
2. 基本方法/任务
22
6.彩色图像处
理
5.图像复原
3&4.图像滤
波和增强
2.图像获取
7.小波变换和
其他图像变换
8.图像压缩和
水印
9.形态学图像
处理
10.图像分割
11.特征提取
12.图像模式
分类
知识库
这些处理的输出通常为图像
这些处理的输出通常为图像属性
问题域

School of  Computer Science and Engineering
2. 空域图像去噪-第三章相关
23
空域滤波就是设计一些运算模板，将模板和图像中对应的各像素
点的灰度值做积分/微分运算，以达到图像平滑或锐化的目的。
加性椒盐噪声图
中值滤波平滑结果

School of  Computer Science and Engineering
2. 空域图像增强-第三章相关
24
空间域增强就是指直接对图像的像素灰度值做某些运算，从而达到对
某些像素的灰度值的改变，以改善图片视觉效果。
原图像
变换增强效果

School of  Computer Science and Engineering
2. 空域图像增强-第三章相关
25
拉普拉斯算子图像锐化增强效果
图像锐化是通过增强图像中的边缘和细节，使图像看起来更清晰

School of  Computer Science and Engineering
2. 频域图像增强-第四章相关
26
频域增强是指通过在图像的频域或变换域进行处理后，使图
像得到增强的方法。
指纹原图
频域增强后的指纹

School of  Computer Science and Engineering
2. 频域图像滤波-第四、七章相关
27
x=
频域变换
频域滤波器
频谱图
滤波后频谱图
图像
重构
频率域涉及：小波变换、傅里叶变换、离散余弦变换等
小波
变换

School of  Computer Science and Engineering
2. 频域图像滤波-第四章相关
28
傅里叶频谱
低通滤波器高通滤波器
偏移高通滤波器

School of  Computer Science and Engineering
2. 图像复原-第五章相关
29
成像过程中凡出现畸变、模糊、失真或混入噪声，使得所成图像降
质，称之为图像“退化”。
图像复原可看作图像退化的逆过程，使得复原后的图像符合一定的
准则，达到改善图像质量的目的。
运动模糊+噪声
逆滤波
Wiener滤波

School of  Computer Science and Engineering
2. 彩色图像处理-第六章相关
30
RGB颜色模型
原图、Red通道、Green、Blue通道
图像颜色空间转换
BGR、RGB、GRAY、HSV、YCrCb、HLS、XYZ、LAB 和YUV

School of  Computer Science and Engineering
2. 图像压缩-第七、八章相关
小波编码: 压缩比分别为34:1, 67:1, 108:1, 167:1
31
图像压缩是一种通过减少图像数据量来减小文件大小的过程，同时尽可
能保持图像质量。这种过程可以分为有损压缩和无损压缩两种方式。

School of  Computer Science and Engineering
2. 数字水印-第八章相关
32
水印嵌入
载体图像载密图像
信道传输
水印提取
数字水印主要用于版权保护、内容认证、追踪与监控、防伪标识等。

School of  Computer Science and Engineering
2. 信息隐藏-第八章相关
33
大容量嵌入
信息无损提取
3k ×4k 空域图像
载密图像
信息隐藏主要
用于隐秘通讯
3,529,321字

School of  Computer Science and Engineering
2. 数学形态学-第九章相关
边界提取
区域填充
34
形态学是一种基于集合论的数学理论，用于分析和处理图像中的形状和结构。

School of  Computer Science and Engineering
把图像分解成构成它的部件和对象的过程；达到有选择性地定位感兴趣
对象在图像中的位置和范围。
2. 图像分割-第十章相关
35

School of  Computer Science and Engineering
特征提取是指使用计算机提取图像中属于特征性的信息的方法及过程
图像特征：几何特征、形状特征、幅值特征、直方图特征、颜色特征等等
常见特征提取算法：PCA、LDA、SIFT、SURF、HOG、LBP、GMM
2. 特征提取-第十一章相关
36
轮廓特征
颜色特征
SIFTSURF

School of  Computer Science and Engineering
2. 图像模式分类–第十二章相关
37
三种基本方法：
基于未知模式与指定原型匹配的分类
最优统计分类器
神经网络

School of  Computer Science and Engineering
2. 医学领域应用
38
三维快速血管造影成像
头部CT 
主要应用如X射线层析摄影(CT)、核磁共振(MRI)、超声成像、血管
造影、细胞和染色体自动分类等。这些技术和设备大大提高了治疗诊断
水平，减轻了病人的痛苦。

School of  Computer Science and Engineering
2. 医学领域应用
39
Gamma射线成像示例超声波成像示例
癌细胞检测
远程医疗

School of  Computer Science and Engineering
2. 遥感和航空测量应用
40
遥感：主要用于资源、矿藏勘探、国土规划、高速公路选线、灾害调查
、农作物估产、气象预报以及军事目标监视等。
气象卫星云图
土地变化监测图
云南省大理州森林火
灾的遥感图

School of  Computer Science and Engineering
2. 工业生产应用
41
该领域的应用从70年代起取得
了迅速的发展，主要有产品质
量检测、生产过程的自动控制
、CAD／CAM等。
网裂龟裂

School of  Computer Science and Engineering
2. 军事安全应用
42
军事目标的侦察、制导和警戒系统、自动灭火器的控制及反伪装
等等都需要用到图像处理技术；

School of  Computer Science and Engineering
2. 公共安全应用
43
公安部门的现场照片、指纹、虹膜、面部、手迹、印章等的处理和
辨识也要借助图像处理。
指纹识别可见光、热红外人脸识别
步态识别
笔迹识别虹膜识别

School of  Computer Science and Engineering
2. 图像通信应用
44
常见的数字图像和数字视频设备如可视电话、会议电视、卫星电
视、数字电视、高清晰度电视、VCD、DVD等
会议电视终端
可视电话
卫星电视接收器

School of  Computer Science and Engineering
2. 机器视觉应用
45
机器视觉主要用于军事侦察、危险环境的自主机器人，邮政、医院和家庭
服务的智能机器人，装配线工件识别、定位，太空机器人的自动操作等。
ASIMO踢球
ASIMO上楼梯
无人驾驶、驾驶辅助

School of  Computer Science and Engineering
2. 娱乐领域应用
46
图像处理（结合CG）在影视特效、游戏创作、滤镜应用、虚拟现实等均
有广泛应用。

School of  Computer Science and Engineering
2. 各光谱波段的应用实例
47
根据光子能量对光谱波段分组：
伽马射线、X射线、紫外线、可见光、红外线、微波、无线电波

School of  Computer Science and Engineering
2. 伽马射线成像的应用实例
48
主要用途包括核医学和天文观测
(a) 骨骼扫描图像
(b) PET 图像
(c) 天鹅星座环图像
(d) 来自核反应堆真空管的伽马辐
射（亮点）

School of  Computer Science and Engineering
2. X射线成像的应用实例
49
主要用途包括医学、工业、天文观测
(a) 胸部X射线图像
(b) 主动脉造影图像
(c) 头部CT图像
(d) 电路板图像
(e) 天鹅星座环图像

School of  Computer Science and Engineering
主要用途包括平板印刷术、工业检测、现为方法、激光、生物成像和天文观测。
(a) 正常玉米的图像(b) 患黑穗病的玉米图像(c) 天鹅星座环的图像
2. 紫外波段成像的应用实例
50

School of  Computer Science and Engineering
可见光和红外波段成像：主要用途
包括光显微方法、天文学、遥感、
工业检测、交通执法等
2. 可见光和红外波段成像的应用实例
51
飓风“卡特莉娜”
人口稠密地区的红外卫星图像
指纹、纸币、车牌识别

School of  Computer Science and Engineering
2.微波波段成像的应用实例
52
主要用途雷达，特点：在任何范围、时间内，不管天气、周围光照条件如何都有
收集数据的能力。雷达是探测地表不可接近地区的唯一方法。
西藏东南部崎岖山区的星载雷达图像

School of  Computer Science and Engineering
2.无线电波段成像的应用实例
53
主要应用于医学和天文学。如在医学中，无线电波用于磁共振成像（MRI）。如
下图显示了人的膝盖和脊椎图像。

School of  Computer Science and Engineering
2. 不同波段成像对比
54
覆盖电磁波谱的蟹状星云脉冲星图像（位于每幅图像的中心）

School of  Computer Science and Engineering
2. 其它有趣研究-Image Inpainting
55

School of  Computer Science and Engineering
2.其它有趣研究-物体去除
56

School of  Computer Science and Engineering
2.其它有趣研究-视觉质量提升
57
美颜
照片去雾

School of  Computer Science and Engineering
2.其它有趣研究-视觉还原
58
“一键卸妆”

School of  Computer Science and Engineering
2.其它有趣研究-Image Morphing
59

School of  Computer Science and Engineering
2.其它有趣研究-属性编辑
60

School of  Computer Science and Engineering
2.其它有趣研究-Deepfake
61

School of  Computer Science and Engineering
2.其它有趣研究-Image Super-Resolution
62

School of  Computer Science and Engineering
2.其它有趣研究-图像风格转换
63

School of  Computer Science and Engineering
▪数字图像处理是一门新兴学科，起源于20世纪20年代，20世纪60年
－70年代得到迅速发展。80年代中－90年代才形成独立的学科体系。
▪数字图像处理，就是采用计算机对图像进行信息加工。图像处理的主
要内容有：图像的采集、增强、复原、变换、编码、重建、分割、配
准、嵌拼、融合、特征提取、模式识别和图像理解。
▪数字图像处理是一个跨学科的前沿科技领域，它与数学、光学、电子
学、计算机技术、成像技术、视觉心理学、视觉生理学、计算机视觉
和人工智能密切相关，有着十分广泛的应用。
小结
64
