# Chap2 数字图像基础-图集

> 主题库：数字图像处理　｜　源文件：Chap2 数字图像基础-图集.pdf　｜　2,430 字符　｜　35 个片段　｜　更新于 2026-11-13
期建入眼之量集

角膜虹膜前房睫状肌晶状体！ 睫状小带视轴玻璃体视网膜盲点中央凹巩膜脉络膜- 神经和销

N 80 Bl0 Ra 1 35. 01 90000 45000 80° 60° 40° 20° 0° 20° 40° 60° 80° Degrees from visual axis (center of fovea)

15,m 100m-  -17mm—    +

强闪光  - Ba Bb_ 暗视觉明视觉暗阈值一 -6 -4 -2 0 2 4  光强的对数(mL)

ECVP ECVPECVP ECVPECVPECVP ECVPECVPECVP ECVP ECVPECVP  ECVP  I ECVP ECVP ECVP ECVpECVp ECVP ECVP ECVP

暗箱镜头 t 元件

iNFRARED OPTICALPRISM ULTRAVIOLET

-λ!

Energyofone photon(electronvols 106 105 104 103 102 101 1 10-1 102 103 104 10-5 10-6 107 108 109 Frequency(Hz) 1021 1020 1019 1018 1017 1016 1013 1012 1011   1015 1014 4   1010 109 108 107  106 105 10-12 10-11 10-10 10-9 10-8 |107 10-6 10-5 10-4 103 10-2 |10-1 1 101 102 103 Gammarays X-rays Ultraviolet Infrared Microwaves Radiowave 0.4×10-6 0.5×10-6 0.6×10-6 0.7×10-6 Ultraviolet Violet Bhue Green Yellow Orange Red Infrared

EnERY [e Sensing material W Voltage waveform out

—胶片传感器转动线性运动胶片每旋转一个增量且传感器完整地从左到右线性运动一次，输出图像的一行图2.13 单个传感器通过运动来生成二维图像

ab 每线性运动一个增量， 输出图像的一行图像重建三维物体的剖面图像成像区域线性运动线性条带传感器 X射线源  X射线源转动 －三维物体 _环形条带传感器图2.14 (a)使用线性条带传感器获取图像；(b）使用环形条带传感器获取图像

照射（能量）源输出数字化后图像成像系统 (内部)图像平面场景元素 Bc cde 图2.15 数字图像获取示例：(a)照射（能量）源；(b)场景；(c)成像系统；(d)场景到图像平面的投影：(e)数字化后的图像

Scene

B c A B A B B Sampling

0 2 N-1 0 Y 1 1 2 M-I X

f(x, y) x Origin →y 0000000 000000 00 0 0 3 .5.5.5 0 111 1 1 .5 X

(O,1)α=M (0,0)α=0 →   −(0,2)α=2M Image f(x, y) (M−1,0)α=M−1→ −(M−1,N−1)α=MN−1 ↓ —(M−1,1)α=2M−1

10 PPI 20PPI 2,54cm 2,54cm www.sjpcw.cn

0 1 1 0 1-. -1 0 1--1    1  1 0 1 0 0 1 0 0 1 0 0 0 1 0 0 1 0 0 1

AUB

B Bc

A∩B B

A−B

B1 B2 B1 AND B2 AND

B1 OR B2 OR

Sxy The value of thispixel is the average value of the pixelsinSy Imagef Imageg

s : = T (z) 2551 SL Z 0 Z0 255

0

Taransformation Coordinate 0 1 0 y= y 0 0 1  X Scaling/Reflection 0 Cx 0 0 cxx y'' = cy (For reflection, set  one scaling factor to−1 y 0 x = T antheothero0 0 0 1 Rotation(about the os θ −sin θ 0− x = x cos θ− y in θ T i 0 0 1_ Translation 1 0 tx] xc = x + tx c 0 1 ty y = y + ty T 00 1 Shear (vertical) 1 v 0] c = x + sy − yc 0 1 0 y′ = y 0  0 1 Shear (horizontal) 1 0 0] c = x  0 0  1

…a a aaaaaaaa anaa a …a      aaaaaaaa anaaaaaa

Z  Component image 3 (Blue) Component image2(Green Component image1(Red

T (, v) R[ (, ν)] f(x, y)[ Operation Inverse Transform g(x, y) R transform

4.灰度分辨率灰度分辨率：灰度级中可分辨的最小变化。 对采样点强度值的离散化过程（连续变化区间转换为单个特定编码）， 确定图像的灰度等级物理a界强范曝光范围成像系统动态范围强度量化 1 2 3 244 255 数字图像灰度级别（灰度分辨率） 31 .  A  School of Computer Science and Engineering VuMM
