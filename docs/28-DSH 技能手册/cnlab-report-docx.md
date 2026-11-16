# cnlab-report-docx

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　4,008 字符　｜　18 个片段　｜　更新于 2026-11-11
---
name: cnlab-report-docx
description: 依据 WPS/Word 模板生成中文课程实验报告（.docx）并转 PDF：处理模板文档网格导致的超大行距、缺失表格样式、OOXML 元素顺序、截图占位跨页、LibreOffice 无头转换与逐页渲染验收。
---

# 中文实验报告（模板 + docx + PDF）生成

## 何时使用

- 用户给一份 .docx 模板（含封面表格、评分须知、实验内容），要求"按模板完成实验报告"
- 报告需同时交付 .docx 与 .pdf，含命令行块、表格、拓扑图、截图占位
- 用 python-docx 在模板末尾追加正文，再用 LibreOffice 转 PDF，需要逐页视觉验收

## 工具路由与真实截图补充

- 若当前会话已提供 Univer 并要求用结构化工具，优先按其流程导入模板、编辑与导出；下方 python-docx 示例只适用于允许该流程的会话。
- Workbench 自动化先设进程 DPI awareness，再读取窗口坐标；每次输入或截图前显式激活窗口并核对前台 HWND，防止截到其他应用。执行后等待 Action Output 更新，必须检查绿色成功记录与 Result Grid，不能把按键发送成功当成 SQL 成功。
- 同时保存完整窗口和结果区域原像素裁剪；SHOW GRANTS 长列应在界面内拉宽再拍，避免末尾 GRANT OPTION 被省略。
- 需要 PDF 内 SQL 可复制时，应实际提取文字验证。当前已验证 Univer print-pdf 可能生成图片页；应导出 DOCX，再按 office-export-pdf 使用 LibreOffice Kit 转换。转换目标存在会报 EEXIST，使用新文件名。
- 当前 Univer 图片环绕枚举经 API 核实为 TextWrappingStyle.INLINE；使用前查询版本匹配 API，不照抄过期枚举名。

## 变更记录

- 2026-11-11：补充真实 Workbench 截图焦点/DPI核验和可复制 SQL PDF 导出边界，来源为已验证的数据库权限实验报告流程。

## 环境（本机已验证）

- Python: `C:\Users\CCW\.dsh\dsh-runtimes\dsh-primary-runtime\dependencies\python\python.exe`
- 已装：python-docx、python-pptx、pypdf、pypdfium2、matplotlib、Pillow
- LibreOffice: `C:\Program Files\LibreOffice\program\soffice.exe`
- 中文字体：`C:\Windows\Fonts\msyh.ttc`（微软雅黑，matplotlib 用）、simhei.ttf、simsun.ttc

## 关键坑（按踩坑顺序）

### 1. 模板的文档网格把行距放大 2–3 倍（最坑）

WPS 生成的模板常带 `<w:docGrid w:type="lines" w:linePitch="312"/>`，段落默认 snapToGrid 生效。
LibreOffice 会把 12pt 正文行距从 24.5pt 放大到 45pt（26 页变 38 页）。

修法（两件事都要做）：

```python
for s in doc.sections:
    g = s._sectPr.find(qn("w:docGrid"))
    if g is not None: g.set(qn("w:type"), "none")
# 每个段落（含表格单元格内）插入 <w:snapToGrid w:val="0"/>
```

实测：改后 12pt + line=1.38 得 24.5pt 行距，页数正常。验收尺寸：A4 595x842pt，正文左侧约 36pt 起排。

### 2. 模板往往没有 "Table Grid" 样式

`doc.add_table(...); t.style = "Table Grid"` 会抛 `KeyError: no style with name 'Table Grid'`。
改为手工写 `w:tblBorders`（外框 8、内线 4，颜色 8FA0B3/C3CBD6）。

### 3. OOXML 子元素顺序错会被 Word 判为损坏

python-docx 的 `pPr.append(...)` 会破坏顺序。写一个按序插入器，覆盖 pPr / tblPr / trPr / tcPr：

```python
PPR_ORDER = ["w:pStyle","w:keepNext","w:keepLines","w:pageBreakBefore","w:framePr","w:widowControl",
             "w:numPr","w:suppressLineNumbers","w:pBdr","w:shd","w:tabs","w:suppressAutoHyphens",
             "w:kinsoku","w:wordWrap","w:overflowPunct","w:topLinePunct","w:autoSpaceDE","w:autoSpaceDN",
             "w:bidi","w:adjustRightInd","w:snapToGrid","w:spacing","w:ind","w:contextualSpacing",
             "w:mirrorIndents","w:suppressOverlap","w:jc","w:textDirection","w:textAlignment",
             "w:textboxTightWrap","w:outlineLvl","w:divId","w:cnfStyle","w:rPr","w:sectPr"]
```

trPr 顺序：cnfStyle…wBefore/wAfter, cantSplit, trHeight, tblHeader, tblCellSpacing, jc。
tcPr 顺序：cnfStyle, tcW, gridSpan, hMerge, vMerge, tcBorders, shd, noWrap, tcMar, textDirection, tcFitText, vAlign, hideMark。

### 4. 表格/占位块被拆到两页

- 表格：每行加 `cantSplit`，除最后一行外所有单元格段落 `keep_with_next=True`（标题段落也要 keep_with_next）
- 图片：图片段落 `keep_with_next=True`，图注 `keep_with_next=False`
- **截图占位框外的图注会被单独挤到下一页**：LibreOffice 不认表格行内段落的 keep_with_next，跨页时图注会孤立。
  对策：把占位框高度压到 4cm 以内，或把图注放进同一单元格；不要指望 keep 生效。

### 5. 行长与代码块

VRP 的高级 ACL 规则一行约 115 字符，9pt Consolas + 0.5cm 缩进仍会折行。
这是终端真实折行，可接受；若不想折行，把字号降到 8.2pt 并把左缩进降到 0.3cm。

## 标准流程

1. 抽取素材：`python-pptx` 读课件、`python-docx` 读模板（含表格/图片标记）、`pypdf` 读实验手册
2. 复制模板为输出文件，`Document(OUT)` 后在末尾 `add_paragraph`，先插分页
3. 定义 par/h1/h2/code/table/caption/figure/shot 辅助函数（全部显式设置字体：中文宋体、标题黑体、代码 Consolas）
4. 关闭 docGrid，给所有段落补 snapToGrid=0
5. `soffice --headless -env:UserInstallation=file:///<临时profile> --convert-to pdf --outdir <目录> <docx>`
6. 验收：`pypdfium2` 逐页 render 成 PNG（scale=1.6），拼 contact sheet 看整体，再放大看关键页
7. 文本 lint：搜 `。。`、`，，`、`（）`、`**`，并核对图表编号连续

## 渲染验收代码

```python
import pypdfium2 as pdfium
pdf = pdfium.PdfDocument(pdf_path)
for i in range(len(pdf)):
    pdf[i].render(scale=1.6).to_pil().save(outdir + f"/page-{i+1:02d}.png")
```

## 写内容时的注意事项

- 没跑过的实验：结果一律写成"预期回显/判定标准"，截图用虚线占位框标注要截什么，不要编造实测数据
- 手册笔误要逐条列进"反馈"章节（本课程手册已知：图 4-16 的 "ALC" 拼写、4.5.7 第 5 步提示符 0/0/3 与 0/0/2 矛盾、缺"清除上一实验 ACL"步骤）
