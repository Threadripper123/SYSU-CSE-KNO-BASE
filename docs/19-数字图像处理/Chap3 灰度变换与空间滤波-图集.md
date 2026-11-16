# Chap3 灰度变换与空间滤波-图集

> 主题库：数字图像处理　｜　源文件：Chap3 灰度变换与空间滤波-图集.pdf　｜　3,423 字符　｜　40 个片段　｜　更新于 2026-11-13
原点一 Y0 像素 [其值为f(x0,yo)]  二 (x0,yo)的3×3邻域图像

s = T (r) = T (r) s0 = T (r0) T (r) - T (r) kro k Dark ← → Light Dark ← → Light

= T ( 一 T (r) 1 K Dark Light -

s = T (r) S0 = T ( 0) T(r) c k 1 ro Dark   Light I

L-1 Negative nth power L/2 L/A Yaentit exponenial 0 L/4 L/2 3L/4 L -1 Input intensity levels, r

L—1 Y = 25.0— 0 L/4 L/2 3L/4 L -1

ca FIGURE 3.7 (a) Intensity ramp image. (b) Image as viewed on a simulatedmonitor with a gamma of 2.5. (c) Gamma- corrected image. c Corrected image as viewed On the same Original image Gamma Correction Original image as viewed on a monitor with same monitor

L—1 (r2, 2) L/4 L/4 L/2 3L/4 L - Input intensities, r

s -T (r) s T (r) C A B A B

abc

One 8-bit byte· Bit plane 8 [aaa ca

All Bit Planes Bit plane=8 BitPlane=7 Bit Plane=6 Bit Plane= 5 BitPlane=4 Bit Plane=3 Bit Plane =2 Bit Plane =1

1 2 3 4 5 6 1 2 3 4 5 6 5 4 5 6 2 14 6 4 3 2 2 1 1 6 6 4 6 6 M k 3 4 5 6 6 6 1 4 6 6 2 3 1 3 6 4 6 6

Histogram of Histogram of Histogram of Histogram of dark image light image low-contrast image high-contrast image abcd

T(r) T(r) L-1 L-1 Single Single Sk value, sq L 0 rk L-1 Yalue valee

A → Eq. (3-11) ) → 0 L—1 0 L-1

a b c pr(k) k s(k) FIGURE 3.19 4 Histogram .25. 7.0- .25 equalization. 20 5.6 .20- (a) Original .15 - 4.2· - T(r) 15 histogram. .10. 2.8 .10 (b) Transformation .05· 1.4 .05 function. (c)Equalized histogram. 0 6 k 0 2 3 4 5 6 k 0 2 3 4 5 6 7 Sk

2 51 92 8 64 0 rk 64 128 192 255  Intensity values of original images

pr(rk) pz(zq) 20 - .1 - .15 .10 .10 .05 ● .05  k zq 0 1 2 3 4 5 6 7 0 1 2 3 4 5 6 7 G(zq) 2(zq) 25 20 1 - P .10 十 ● .05 十 Zq Zq 0 1 2 3 4 5 6  7 0 ) 1 2 3 4 5 6 7

0.12 : 0.10 0.08 0.06 0.04 0.02 0 0 63 127 191 255

255 T (r ) → 191 Sk127 63 0 0 63 127 191 255

0.12 0.10 0.08 0.04 上 0.02上1 0 0 63 127 191 255

0.12 P2(zq) 0.10 0.08 0.06 0.04 0.02  0 0 63 127 191 255 zq

0.020 $P{(z) 0.016 0.012 0.008 0.004 0 0 63  127  191  255

255 (1) 191 Sk127 63 2 0 0 63 127 191 255

a

c 一---+

Associative f★(g★h =(f★g ★ Distributive f ★ (g + h) = (f ★ g) + (f ★ h) f ☆ (g + h) = (f ☆ g) + (f ☆ h)

Origin f 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 1 0 0 0 0 0 w 0 0 0 1 0 0 0 0 0 1 0 0 1 2 3 0 0 0 0 0 0 0 0 0 0 0 0 4 5 6 0 0 0 0 0 0 0 0 0 0 0 0 7 8 9 0 0 0 0 0 0 0 (a) (b)

Initial position for w Correlation result Full correlation result 1 2 3 0 0 0 0 0 0 0  (c) (d) (e)

- Rotated w Convolution result Full convolution result (f) (g) (h)

1 1 1 1 2 1 1 1 1 2 4 2 1 1 1 1 2 1

a a       三 caaaaaaa aaaaaaa a …a caaaaaa aaaaaa

如果s和t是独立的（即ρ三O），则PDF可以简化为： 1 f(s, t) S . s = 2πσsσt exp 2 2 这意味着二维高斯分布相当于两个独立的一维高斯分布的乘积。

2 2 j4√2 5 2√5 √17 4 √17 2√5 5 4√2 5 3√2 \\3 \0 3 \\0 √\ 3 3√2 5 \2√5 √13 2√2 √5 2 √5 2√2 1 2√5 i√\{3 3 i 1 0 1 2 4 2 1 3 4 3×3 12√5 \{ 3 2√2 2 \5 2√2 i √\{3 2√5 7×7 4√2 5 2√5 √\17 4 √17 2√5 5 4√2} 9×9 (m−√ (m−√

ca G FIGURE 3.35 本 (a) Samplinga Gaussianfunction toobtainadiscrete 0.3679 0.6065 0.3679 Gaussiankernel. The values shown 1 : are for K = 1 and 0.6065 1.0000 0.6065 4.8976 × 3 ×3 kernel [this nsthesameasFig. 0.3679 0.6065 0.3679 3.31(b)].

TABlE 3.6 Mean and standard deviation of the product (×) and convolution (★) of two 1-D Gaussian functions, f and g. These results generalize directly to the product and convolution of more than two 1-D Gaussian functions $m 02 + 2 +mσ Mean mf mg mf×g σf · + 4 · 2 mf g = mf + mg σ g Standard deviation σf σg g 0 f ×g σ 2 g 0 + σ g

a a a a a aa a a a a  a a a a a a a aaa

a caaaaa

abc
