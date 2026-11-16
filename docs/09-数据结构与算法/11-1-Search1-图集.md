# 11-1-Search1-图集

> 主题库：数据结构与算法　｜　源文件：11-1-Search1-图集.pdf　｜　6,854 字符　｜　26 个片段　｜　更新于 2026-11-11
YAHOPO! or

中山大学-Googto构图搜索 × Zhuo ← → c 价  www.googto.com/?q=中山大学 ☆ 8p 三 Q搜索图片学术搜索技巧 0  症  Googt . 中山大学 Qsearch 时间范围  Hidden wk About3,820,000results(0.47seconds) 时间不限  SunYat-Sen University    中山学SUNYAT-SENUNIVERSITY  Publicuniversityin Guangzhou,China  语言范围  www.sysu.edu.cn/ sysu.edu.cn SunYat-sen Universitywasfounded in 1924 byDr.SunYat-sen.ltisacomprehensive mul  所有语言 university,includingthe humanities,socialsciences, Sun Yat-senUniversity,also knownas   Zhongshan University,isapublic universityin 中山大学 - 中山大学外国留学生办公室 -Schools& DepartmentsAz-SYSu Guangdong,China.ltwasfoundedin1924by Sun Yat-sen, a revolutionary and the founding 结果条数国 fatherof the Republic of China.Wikipedia 中山大学SUNYAT-SENUNIVERSITY 10条 （快） Founder:SunYat-sen  www.Sysu.edu.cn/2012/cn/ 中山大学是由孙中山先生亲手创立，有着100多年的办学传统，由原中山六学和中山医科大学 Founded：1924 包括人文科学、 社会科学、 自然科学、 技术科学、 工学、 医学、. Headquarters:GuangdongProvince,China 字词位置  Enrollment:85,100(2011) 任何位置中山大学-维基百科， 自由的百科全书  Undergraduates:21,623 https//zh.wiipedia.org/zh-hk/中山大学中山大學 （简称： 中大，英语：SunYat-senUniversity，SYSU）， 是中国大陆的综合性大学。 Notablealumni 件类型中先生于1924年创的国东学， 1926年为纪念创始 View10+more 任意类型國立中山大學-维基百科，自由的百科全书 https://zh.wikipedia.org/zh-hk/國立中山大學國立中大學 （英语： NationalSunYat-senUniversity）， 是中華民國一所前身由孫中山創来源过滤也是分享教育部 「遵向頂尖大計畫」5年50o億補助的研究型 … Shou-Wu Xu Kenneth Pan Huang Baidu Zhang Ningsheni Huang Yiyang Huahua Soso 中学 （ 中国东省校 ） 百度百科 People also searchfor View 5+ more Sogou baike.baidu.com/subview/4809/5274738.htm 360doc 中山大学 （SunYat-senUniversity）， 简称中大, 前身是孙中山先生创立于1924年的国立广：   一百多年办学传统，是学术与文化中国南方重镇和人才培养南方.     搜索模式历史沿革-原中山医科大学-原中山大学-办学规模

Unsuccessfulsearch:ncomparisons. Successfulsearch,bestcase 1comparison Successfulsearch,worstcase:ncomparisons. Successfulsearch, average case: (n + 1)comparisons.

4

5  2 0 3

Idea: In searchingan ordered list,first compare the target to the key in the center of the list. Ifit is smaller, restrict the search to the left half; otherwise restrict the search to the right half, and repeat. In this way, at each step we reduce the lengthofthelisttobesearchedbyhal

Initialization: Set bottom = 0; top = the_list.size()— 1;  Compare target with the Record at the midpoint, mid=(bottom +top)/2; Change the appropriate index top or bottom to restrict the search to the appropriate half of the list. Loopterminates whentop ≤ bottom,if ithas not terminated Make progress toward termination by ensuring that the number of items remaining tobe searched, top — bottom + 1, strictly decreases at each iteration of the process.

Error_coderecursive_binary_1(constOrdered_list &the_list, canstKey &targetintbotom inttop int&position / Pre: bottom and top define the range in the list to search for the target. Post: If aRecord in the range of locations from bottom to top in the_list has key equal to target, then position locates one such entry and a code of success is returned. Otherwise, theError_code ofnot_present is Uses:recursive_binary_1and methods ofthe classes List and Record.*/

{ Recorddata; if(bottom < top) { II List has more than one entry. intmid=(bottom +top)/2; the_list.retrieve(mid, data); if(data < target) II Reduce to top halfof list. return recursive_binary_1(the_list, target, mid + 1, top, position); else II F Reduce to bottom half of list. returnrecursive_binary_1(the_list,target, botnom, mid position else if(top < bottom) returnnot_present; I List isempty. else { II List has exactly one entry. position = bottom; the_list.retrieve(bottom , data); if(data == target)return success; elsereturnnot_present

The division of the list into sublists is described in the follow- ingdiagram: <target  ≥target

Only entries strictly less than target appearin the first part of the list, but the last part contains entries greater than or When the middle part of thelist is reduced to size 1, it will be guaranteed to be the first occurrence of the target if it appears morethanonceinhelist.

Error_codebinary_search 1(constOrdered_list&the_list, canstKey &target int&position / Post: If a Record in the_list has Key equal to target, then position lo- cates one such entry and a code of success is returned. Otherwise, Uses:MethodsforclassesListandRecord. /

Recorddata int bottom = 0,top = the_list.size() — 1; while(botom top) int mid=(bottom +top)/2; the_list.retrieve(mid, data); Yndata targe bottom = mid + 1; else top=mid; if(top < bottom)return not_present; else position = bottom; the_list.retrieve(bottom, data); if(data == target)return success; elsereturnnot_present;

Recognizing Eguality in Binary Search Method: Check at each stage to seeif the target has been Error_coderecursive_binary_2(constOrdered_list&the_list, const Key &target, int bottom, int top, int &position) I Pre: The indices bottom to top define the range in the list to search for the _targ t. Post: If a Record in the range from bottom to top in the_list has key equal to target, then position locates one such entry, and a code of suc- cess is returned. Otherwise, not_present is returned, andposition is ndefined. Uses: recursive_binary_2,together withmethods from theclasses Ordered_listandRecord. /

E Record data; if botom <=top) int mid=(bottom +top)/2; the_list.retrieve(middata) ifn(data == target { posataon = mid returnsuccess else if(data <target) return recursive_binary_2(the_listtarget mid + 1,top, position); EISE return recursive_binary_2(the_list,target, bottom, mid — 1,position) 子

Error_code binary_search_2(constOrdered_list &the_list, constKey &target,int&position f Post: f a Record in the_listhas key equal to target,then position cates one such entry and a code of success is returned. Othenw not_presentisreturned and positionisundefined. Uses: MethodsforclassesOrdered_listandRecord. " aecorddata int bottom = 0,top = the_list.size()— 1; while(botom =top){ position =(bottom + top)/2; the_list.retrieve(position, data); if(data == target)returnsuccess; if(data target)bottom = position + 1; else top = position — 1; Yetaurnnotpresent

The division of the list into sublists is described in the follow- ingdiagra <target > target

The fi rst part of the list contains only entries strictly less than target, and the lastpart contains only entries strictly greater thantarget If target appears more than once in the list, then the search maayre urn anyi nstanceof he arget. Proof of progress toward termination is easier than for the

二 2 3 3 n n

5 3  4 5 8 9 10 1 字二三字 2 3 4 5 F 6 8 回回自

Secondversion ≠ 2 2 八 3 6 自回回 4 回回 10

TaRgE TARgE less than greater than key at mid key at mid: Search from Search from 1 to mid -1. mid +1 to top.

② 8 3 6 9 F F 4 F 10 日 E E F F F

S R Y

S

[a顾字符串匹配 ・ 子在主串中的定位称为模式匹配PatternMatching)或串匹配(String Matching)。模式匹配成功是指在主串s中能够找到模式串T，否则，称 [Tl___T ____ 0 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 17 a b a c a a b a c c a b a c a b a a 1 a b a c a b - a b a c a b I a b a c a b a b a c a b a b a c a b a b a c a b a_ b _a _c _a_ b
