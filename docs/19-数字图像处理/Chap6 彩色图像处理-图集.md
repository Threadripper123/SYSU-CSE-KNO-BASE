# Chap6 彩色图像处理-图集

> 主题库：数字图像处理　｜　源文件：Chap6 彩色图像处理-图集.pdf　｜　2,850 字符　｜　33 个片段　｜　更新于 2026-11-13
OPTICAL PRISM ULTRAVIOLET

GAMMA X-RAYS U-V INFRA- MICRO- RAYS RED WAVES T-V RADIO .001nm Inm 10nm 0oo1ft 21ft 1ft 100f ULRAVIOLET VISIBLESPECTRUM NFRARED 300 400 500 600 700 1000 1500

4 5 35 75 Blue Green Red   450   500   550   6 00 650  700m

MIXTURES OF LIGHT (Additive primaries) GREEN YELLOW CZ WHITE RED E4U HAGENTA MIXTURESOFPIGMENTS (Subtractiveprimaries) YELLOW REO GREE BLACK MAGENTA GYAN BLUE PRIMARY AND SECONDARY COLORS OFLIGHTANDPIGMENT

(C.I.E.CHROMATICITY DIAGRAM) 520 .8-  SPECTRALENERGYLOCUS (WAVELENGTH,NANOMETERS) 510 560 GREEN GOLD 1 WARMWHITE 590 COOLWHITE  DAYLIGHT 1 PINK O 620 4/90 RED EO POINT EO DEEPBLUE OF -. EQUAL ENERGY BLUE 480 1 470 260 450 .0 18 .0 .5 xaxis

(R = 0) (G = 0) (B = 0)

ReD Green RGB Color [TYma

B (0,0,1) Bluec ,Cyan 1 1 Magenta - T_e 1 1 Black 1. Grayscale (0, 1, 0) G GaREEn (1, 0, 0) Red Yellow R

White White Yyan Yellow Cyan Yellow [T Blue d Red Blue Red Black Black

Green Yellow S_ S H H Cyan  →Red Cyang S Yellow Cyan。 Y Red . H  Blue Magenta Blue Magenta Red Blue Magenta

White 1= 0.75 1 Green Cyan Yellow 1 H I=0.5 Blue S Red Magenta 1 1 1 1 1 Black White 1 = 0.75 Green Yellow 1=05cyan< H S Red Blue Magenta Black

[ θ B≤G H= 360 -θ B > G θ = arccos 3 (cR − G) + (R − GG − B) 3 S = 1 −  (R + G + B ) [min(R, G, B)] I = - (R + G + B )

00≤ H < 1200  S cOs H R = I| 1 + − G=1−(R+B) B = I(1−s) 1200≤H<2400 H = H−120° G = I 1 + − S c0s H B=1−(R+G) R = I(1− s ) 240°≤H<3600 H=H−2400 S c0s H B = I 1 + · R=1−(G+B) G = I(1−s ) Cos(600-H

L=100 Green Yellow -a +b -b Blue +a Red L=0 Lab

White L* yellow +b* Green Red -a* +a* blue -b* Black

新型冠状病毒肺炎疫情：中国数据来源：国家及各地卫生健原委员会数据截至 2020-02-2710：00 （建议电脑端查看） 内蒙古白治区山东省青海省西同南西藏白治区安德道 r LET 福建省中自的区台省香港特别行改区门特别行政区北京大学可视化与可视分析实验室中国南询 PKU Visualization and Visual Analytics Group

新型_状病毒肺炎情：中国数据来：国及地卫健康员会数据截至2020-02-2710:00 （建议电脑端查看） 内黄古日治区西疆自治区自省东面香港特别行政区特别行政区 O  北京大学可视化与可视分析实验室中国南 PKUVisualizationand Visual AnalyticsGroup

cYamanan Yl (White) L − 1f  Slicing plane (Black) 0) y X

2 0 Li Intensity levels

10 >20

ReD transformation GrEEn f(x, y) G(x, y) transformation cYaa

ReD a GrEEN BLUE Explosive Bag- Explosive Bag Background- Background

g1(x, y) g2(x, y) f2(x, y) Transformation T2 Additional f k(x, y) Transformation TK

Pixel- Voxel (x,y) (x, y) 2-Dneighborhood- 3-D neighborhood- Grayscale image R GB color image

Fullcolorimage Cyan Magenta Yellow Black Cyan Magenta Yellow Red Green Blue Hue Saturation Intensity

R,G,B CMY K C,M,Y HS 0 1 0 10 10 1 0 0 1

0 R,GB  H 1

cBmne Magena Cyan complemens Red Green Yellow

Fhat Corrected 0 R,G,B Carracfed 0 RGB Dark Corrected O

OriginalCorrected 1 lenin Vk Hc in Wcaki black blaek cu n cyan B C 0 0 0 Heai n Weakin Heavyin Weakin magcn magenta yelom yellow 0 0 0

H S Histogrambefore processing 1 (median =0.36) 0.5 Histogram after processing (median = 0.5) 0 0.36

D 1 2 4

B B B G G R R R
