# 09 STL-第十八周-图集

> 主题库：程序设计　｜　源文件：09 STL-第十八周-图集.pdf　｜　724 字符　｜　8 个片段　｜　更新于 2026-11-11
中山大學 
 
SUN YAT-SEN UNIVERSITY

学大山中立园

中山大學

SUN YAT-SEN UNIVDRSITY

类别插入后擦除后容器条件迭代器合法？ 引用合法？迭代器合法？ 引用合法? array  N/A N/A N/A 插入更改容量 vector 香是被修改元素前否于被修改元素或其后顺序容器 deque 否是是， 除了被擦除元素修改首或尾元素否否仅修改中部 1ist 是是，除了被擦除元素 forwardlist 是是，除了被擦除元素 cYen [TYmaaln_ e_ 关联容器是 map 是，除了被擦除元素 unordered set 否 N/A 插入导致重哈希 unordered multiset 无序关联容器是 unordered_map 是是，除了被擦除元素无重哈希 unordered_multimap

中山大學 [o器配器 ・容器适配器提供顺序容器之上的不同功能接口（界面） ）c   C 类名说日明所在头件 stack 找。 元素的插入和删除只在栈顶 <stack> 进行，支持后进先出（LIFO） 的元素ia queue 队列。在尾端插入元素， 在头端 <queue> 删除元素，支持先进先出 （FIFO）的元素访问 priority_deque 带优先级管理的队列。 元素按优 <queue> 先级从高到低排列，使用元素类型的<操作确定优先级

中山大學 S— 适配器与容器 1） stack 适配器可以建立在 vector、 list 或 deque 容器上。 2）（ queue 适配器只能建立在list或deque 容器上。 ・如果创建适配器对象时不指定基础容器，则」      默认
