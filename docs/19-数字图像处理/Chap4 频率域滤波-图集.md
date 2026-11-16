# Chap4 频率域滤波-图集

> 主题库：数字图像处理　｜　源文件：Chap4 频率域滤波-图集.pdf　｜　2,305 字符　｜　34 个片段　｜　更新于 2026-11-13
WN

八八八八

Z1Z2 θ1+θ2 ZZ θ2 Z1  x

δ(t) ∆T(t) 0 t0 ... −3∆T −2∆T −∆T 0 ∆T 2∆T 3∆T δ(x) s∆x(x) 1- δ(x − x0) 0 x0 .. −3∆X−2ΔX −ΔX 0 ∆X 2ΔX 3ΔX ..

F ) = f()−π = A−π —0a [e−2 = J_W/ −A − A [e−-jπW − ejπW] j2π [ j2π [ A | -[ejπW − e−jπW] [a = AW , sin(π W) (πW)

f(t) A −W/2   0 W/2

F() AW N 1/W- 1/W 2/W

AW . . −2/W 1 0 − 2/W . .. =1/W 1/W

S∆T(t) -2∆T −∆T0 ∆T 2∆T

f(t) t

fk = f(k∆ T) K

···-2∆T-ΔT 0 ΔT 2ΔT

F() μ

F( μ 0 F ( ) −2/∆T −1/∆T 0 1/∆T 2/∆T F() μ −2/∆T −1/∆T 0 1/∆T 2/∆T 一—— −3/∆T −2/∆T −1/∆T 0 1/∆T 2/∆T 3/∆T

F() max 0 max

F() U -1 0 1 1 2ΔT 2ΔT ΔT

F() μ −2/∆T −1/∆T 0 1/∆T 2/∆T H() ∆T μ 0 μ [ma ma

I

F() max max −3/∆T −2/∆T −1/∆Ti 0 i1/∆T  2/∆T 3/∆T H( A ∆T μ 0 F ) = H()F()  − −max 0 max

t 0 )1234 ● △T

O δ(x − x0, y − yo) x y0 y

|F(,ν)| f(t,z) ATZ T/2 Z/2 N

T Z AZ AT

Footprint of a  2-D ideal lowpass (box) filter [ma max

1□ Low frequency    High frequency Samplinggrid

FIGURE 4.17 Various aliasing enf ects resulting cYaam interaction between the frequency of 2-D signals and the sampling rate used to digitize them. The regions outside the sampling grid are continuous and free of aliasing.

ca FIGURE 4.18 Aliasing. In (a) and (b) the squares are of sizes 16 and 6 pixels on the side. In (c) and (d) the squares are of sizes 0.95 and 0.48 pixels, respectively. Each sm all square in (c) is one pixel. Both aliased. Note how (d) masquerades as a"normal"image.

(0,0))N/2− N −1− (0,0)− N/2− N −1 -F(0,0) 2 2 M/2√ M/2− cYaa M−1 M−1→ U u  = M × N data array computed by the D FT with f (x, y) as input = M × N data array computed by the D FT with f (x, y)(−1)X+y as input c

X u

I

SpatialDomain FrequencyDomain 1) f(x, y) eal → F (,ν) = F(− ,−v)  2) f(x, y) imaginary → F (−u,−v) = −F(u,ν) f(x, y) real → R(u,ν) even; I(u,ν) odd f(x, y) imaginary c R(u,v) odd; I(u, ν) even 5) f −x,−y) real → F (,ν) c mplx 6) f(−x,−y) complex F(−u,−ν) complex 7) f (x, y) complex → F (−u,−ν) complex 8) f(x, y) real and even F(u,v) real and even 9) f(x, y) real and odd → F(u, v) imaginary and odd 10) f(x, y) imaginary and even  F(u,v) imaginary and even  1 2 f(x, y) imaginary and odd  F u,v) real and odd f (x, y) complex and even 1 F(u,v) complex and even 13) f (x, y) complex and odd ← F(u,v) complex and odd

H,) M/2 N/2 u 0

H(u,v) H(,v) H(,ν) M/2 N/2 M/2 u N/2 M/2 N/2

H(u,) M/2 N/2
