# 数据库系统原理-图表说明（book.pdf ER图等）

> 主题库：数据库系统原理　｜　源文件：（纯文本条目）　｜　1,540 字符　｜　7 个片段　｜　更新于 2026-11-11
# 数据库系统原理 — 图表说明（book.pdf 关键图）

> 本文件由视觉识图生成，用于让图片/ER 图可被检索。原文为矢量图形，普通文本抽取无法获得其结构信息。
> 来源：D:\个人文件夹\桌面\新建文件夹\知识库\数据库系统原理\book.pdf（Silberschatz《Database System Concepts》第6版）

## Figure 6.15 大学企业 E-R 图（PDF 第 293 页 / 书内第 264 页）

本章 6.7 节用它讲解如何把 E-R 图转换为关系模式。图形构成：

**实体（矩形，含属性）**
- **department 系**：dept_name（主键，带下划线）、building、budget
- **instructor 教师**：ID（主键）、name、salary
- **student 学生**：ID（主键）、name、tot_cred
- **course 课程**：course_id（主键）、title、credits
- **section 开课班（弱实体，双边框矩形）**：sec_id、semester、year
- **time_slot 时间段**：time_slot_id（主键）、day、start_time、end_time
- **classroom 教室**：building、room_number（主键组合）、capacity

**联系（菱形）**
- course_dept：course → department，课程属于哪个系
- inst_dept：instructor → department，教师所属系
- stud_dept：student → department，学生所属系
- advisor：instructor ↔ student，导师指导关系
- teaches：instructor ↔ section，教师授课
- takes：student ↔ section，学生选课；该联系带属性 **grade 成绩**（虚线框，联系属性）
- sec_course：section → course，开课班对应课程
- sec_time_slot：section → time_slot，开课班的上课时间段
- sec_class：section → classroom，开课班使用教室（弱实体标识联系）
- prereq：course 上的递归联系（自联系），表示先修课，带两个角色标注 course_id 与 prereq_id

**记忆要点**：section 是弱实体，依赖 course + time_slot + classroom 共同确定；takes 是多对多联系且带描述属性 grade；prereq 是课程对自身多对多自联系。

检索关键词：ER图、E-R模型、实体联系图、大学企业、系 department、教师 instructor、学生 student、课程 course、开课班 section、教室 classroom、时间段 time_slot、成绩 grade、先修课 prereq、弱实体。

## 识别方法说明

该图在 PDF 中是**矢量绘制**（该页位图数量为 0、矢量绘图对象 81 个），因此：
- 插件内建的「PDF 图片描述」基于抽取内嵌位图，对矢量图无效；
- 正确做法是把页面渲染成 PNG 再交给视觉模型或人工识读；
- 渲染脚本：C:\Users\CCW\.dsh\render_pages.py，页面筛选脚本：C:\Users\CCW\.dsh\find_diagrams.py。
