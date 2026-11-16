# Chap10 图像分割-图集

> 主题库：数字图像处理　｜　源文件：Chap10 图像分割-图集.pdf　｜　3,939 字符　｜　36 个片段　｜　更新于 2026-11-13
Ifslate poit Ram tep _ineS Fla scgment_ Intensity values ; 5 5 4 3 2 1 0 00 6 00 0 0 1 3 1 00 0 0 77 77 Firstderivative -1-1 -1-1-10 0 6 −6000 1 2 -2-100 0 7 000 Secondderivative -100001 06-12600 11 -411 00 7 –700

1 1 1 1 -8 1 1 1 1

人

-1 -1 -1 2 -1 -1 -1 2 -1 -1 -1 2 2 2 2 -1 2 -1 -1 2 -1 -1 2 -1 -1 -1 -1 -1 -1 2 -1 2 -1 2 -1 -1 Horizontal +45° Vertical –45°

Horizontal intensity profile First derivative Second derivative Zero crossing

Z1 Z2 Z3 Z4 Z5 Z6 Z7 Z8 Z9 -1 0 0 -1 0 1 1 0 Roberts

-1 -1 1 1

-1 -1 -1 -1 0 1 0 0 0 -1 0 1 1 1 1 -1 0 1 ca -1 -2 -1 -1 0 1 0 0 0 -2 0 2 1 2 1 -1 0 1 Sobel

-3 -3 5 -3 5 5 5 5 5 5 5 -3 -3 0 5 -3 0 5 -3 0 -3 5 0 -3 -3 -3 5 -3 -3 -3 -3 -3 -3 -3 -3 -3 N N W SW 5 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 -3 5 0 -3 5 0 -3 -3 0 -3 -3 0 5 5 -3 -3 5 5 -3 5 5 5 -3 5 5 S SE E NE

∇2G(x, y) = ∂2 ∂2 G(x, y)= ∂x2 - +· − ∂y2 ∂2 ∂2 x2+y2 x2 + y2−2σ2 x2+y2 ∂x2 + 2σ2 2σ2 e ∂y2 4

∇2G ∇2G 0 0 -1 0 0 0 -1 -2 -1 0 -1 -2 16 -2 -1 0 -1 -2 -1 0 Zero crossing Zero crossing 0 0 -1 0 0 220

LL TT

-157.5° + 157.5° +45°edge -112.5° +112.5° — Verticaledge -67.5° +67.5° -45°edge -22.5° +22.5° 0° Horizontal edge

1）0：左边和右边 2）45：右上和左下 3）90：上边和下边 4）135：左上和右下

-157.5° /+157.5° Edge- Edge normal (gradient vector) -22.5° +22.5° X

é

→y θ'' θmin θ 0 θmax → 0 cosθ+ ysinθ = ρ min P 0 (x,y) y) ρ max xcosθ + ysinθ = p X ρ p

FGURE10.31 (a) A 502× 564 aerial image ofan airport.(b) Edge map obtained using Canny''s algorithm.(c) Hough parameter space (the boxes highlight the points associated with long vertical lines).(d) Lines in the image plane anreponding othepoint highligtedbytheboxes e Line superimposedonteoriinalimag

T T1 T2

FlGURE 10.33(a) Noiseless 8-bit image.(b) Image with additive Gaussian noise ofmean O and standard deviation of 10 intensity levels.(c) Image with additive Gaussian noise of mean O and standard deviation of 50intensity levels.

0 63 127 191 255 0 0.2 04 0.6 0.8 1 0 63 127 191 255 FIGURE10.34 (a) Noisy image.(b)Intensity ramp inthe range [0.2, 0.6].(c) Productof (a)and (b).(d)through (f)

FIGURE 10.35 (a) Noisy fingerprint.(b) Histogram.(c) Segmented result using a global threshold (thin image border addedforclarity).(Originalimage courtesyofthe NationalInstituteofStandards andTechnology).

Q

65 0 127 191 255

Do Q

FIGURE10.37 (a) Noisyimage from Fig.10.33(c) and (b)its histogram.(c) Result obtained using Otsu''s method. (d) Noisy image smoothed using a 5 × 5 averaging kernel and (e) its histogram. (f) Result of thresholding using

a FIGURE 10.38 (a) Noisy image and (b)its histogram .(c) Result obtained using Otsu''s method.(d) Noisyimage smoothed using a 5 × 5 averaging kernel and (e) its histogram.(f) Result of thresholding using Otsu''s method. Thresholding failed in both cases to extract the object of interest. (See Fig. 10.39 for a better solution.)

D 63 127 191 255 10 63 127 191 255

63 127 191 255 63 127 191 255

63 127 191 255 a FGURE10.42 (a) Image of an iceberg.(b) Histogram.(c) Image segmented into three regions using dual Otsu thresholds.

nbt iy tetnne toe ndnnlt i betnun to ley ny dndta offenne d to r felnea fkn . dng tat ofennek udreeo.gakronofthllouut gakonofthl udresgakoofthlfounte tayaohaaidolthyothw har aohaaidofthyothw tutalohaaidofthothw hart af torkly onlov fo a torkly onelsow fo aid to ley oneleon fo a thy umlof tvo thotsang th um of tvo thothaan f th umlof tuvo thothaand and paidthtecaiptarhurse and haidth tece tarher tand haid th tecei tarhurst ath amdh the presento th bmdh.thefprsen sath hmgh theefprsente! ey aliewenfeof ''dud lon alienenfeoff dudlon uy alinedfeoff dndlonfer artain trats orpanatvof a keon hhelhere anfg son -hte lheirs ay wtracts -hthe lheirs'' anfa Cartain ovparale Zaikeon tracts ov hanaleof La andairerlonf,thousandayn wlon thous andacrerton thousanpayre

0 63 127 191 255 0 63 127 191 255

R R R1 R2 R1 R2 R3 R4 R3 RB R4 RAn (R2) R43) RA)

「  中

true σ>α AND 0<m<b

watershedlines catchment catchment basin basin
