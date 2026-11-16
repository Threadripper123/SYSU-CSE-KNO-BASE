# 09 STL-第十七周-图集

> 主题库：程序设计　｜　源文件：09 STL-第十七周-图集.pdf　｜　1,190 字符　｜　8 个片段　｜　更新于 2026-11-11
中山大學   SUN YAT-SEN UNIVDRSITY

中山大學     SUN YAT-SEN UNIVERSITY

The Standard Template Library Hewlett Packard Laboratories 1501 Page Mill Road, Palo Alto, CA 94304 stepanov''@hpl.hp.com & lee @hpl.hp.com March7,1994

HEWLETT PACARD Mehdi Jazayeri, Meng Lee, and Alex Stepanov Hewlett-PackardLaboratories [

26.3 Sequencecontainers array> <deque> <forward_list> <list> <vector> 26.4 Associativecontainers <map> <set> 26.5 Unordered associative containers <unordered_map> 26.6 ( Containeradaptors <queue> <stack>

●read  increment cYmYanaGll LegacyRandomAccessIterator (with multiple  decrement random access storage Iterators that fall into one of the above categories and also m eet the requirem ents of LegacyOutputIterator are called mutable iterators.  write ● increment passes)

学大山中立园

中山大學 SENUN 使用容器： exp1.cpp  [a eeo ainclude <iostreamo //expl.cpp usingnamespacestd; intmain() ivec.front()= 100； // 将第θ个元素修改为100 cout <<"the first element："<< ivec <く endl； // 输出第θ个元素的值 ivec =102；//将第1、第2个元素修改为102、 103 ivec.at(2)=103; cout <<"the second element："<< ivec.at(1)<<endl； // 输出第 1、 第 2个元素的值 cout <<"the third element:" << ivec << endl; cout <<"thelastelement："<くivec <くendl； // 输出最后个元素的值
