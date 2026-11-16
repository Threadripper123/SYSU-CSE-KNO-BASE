# Lecture09.IntCodeGeneration-图集

> 主题库：编译原理　｜　源文件：Lecture09.IntCodeGeneration-图集.pdf　｜　490 字符　｜　4 个片段　｜　更新于 2026-11-11
大，
2A

E.addr=t5 E.addr =c E.addr = t4 c L.array = a L.type = integer L.addr = t3 L.array=a / L.type = array(3, integer) [ E.addr = j ] L.addr=t1  [ E.addr=i ] a.type = array(2, array(3, integer)) I

t1 = =i * 12 t2 = =j*4  t3 = t1 + t2   t4 = a [t3]  t5 = C + t4

1)Quadruples Sourceode a ● a = b *-c+b*-c Three-address code  minus C  = b*t1 result = op arg1 arg2  minus C  = b* t3 0 minus C t1 t5 = t2 + t4 1 1 b t 1 t2 a = = t5  2 minus C t3 Quads 3 * b t3 t4 4 t2 t4 t5 5 ts a Page12/53
