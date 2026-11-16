# gh-office-docx-extra

> 主题库：DSH 技能手册　｜　源文件：（纯文本条目）　｜　1,688 字符　｜　7 个片段　｜　更新于 2026-11-11
---
name: gh-office-docx-extra
description: "Word文档制作与校对。创建、读取、编辑Word文档和报告。仅在相关任务时使用。"
---

# Word文档制作与校对

仅在任务匹配本技能时应用。当前用户要求、系统规则与实际工具权限优先。此为GitHub上游技能的本机兼容适配版，默认中文输出。不要假定上游列出的工具、包、API密钥或Linux命令已可用；先检查环境，优先当前软件已有工具。不要自动调用收费服务、上传本地私密资料或运行上游安装命令。

创建、读取、编辑Word文档和报告。先读取原文件与模板，遵守现有样式，确认页面、页眉页脚、标题层级、目录、表格与页码。在当前环境优先使用已有Office工具或python-docx；复杂修订可依据上游OOXML说明处理。列表用真实编号，表格列宽与总宽一致，图片保留比例，中文字体和段落明确设置。编辑时保留无关内容、批注及修订要求。生成后重新读取检查正文、表格和关系，工具可用时渲染检查分页、截断、字体与对齐。

上游来源：https://github.com/anthropics/skills/tree/2c5958ecac7cee2b583967a2c4981d0f4a75c579/skills/docx

详细参考与脚本见本技能目录的 UPSTREAM.md、references/、scripts/ 或其他附属文件；只在需要时读取。以上兼容约定替代上游关于预装环境、强制服务和默认数量的假设。

## 本机实战补充：中文实验报告（模板 + 文本框代码块）

以下为 2026-11 在 Windows + python-docx 1.2 + LibreOffice 实测有效的做法。

- 套课程模板时以模板 docx 为基底（保留页眉横幅、封面表格），只在其后追加正文；逐个 run 设置 `rFonts ascii/hAnsi="Times New Roman"、eastAsia="宋体"`，仅设 `font.name` 不会改中文。
- **删除 sectPr 里的 `w:docGrid`**。模板若带 `docGrid type="lines" linePitch="312"`，即使样式写了 `snapToGrid=0`，LibreOffice 仍会把 1.5 倍行距吸到 2 个网格，24 页会膨胀到 43 页；删掉后行距才按字体度量计算。
- 代码块用 DrawingML 文本框（`w:drawing/wp:inline/wps:wsp/wps:txbx`），不要用旧 VML `v:shape`：LibreOffice 导出 VML 文本框时会丢边框/填充并把文字漂到页顶。高度按 `行数*14.6pt+13` 固定，段落加 `keepLines`。
- 表格 XML 顺序：`tcPr` 内 `tcW → shd → vAlign`；`tblBorders` 用 `insert_element_before(..., 'w:tblLayout')`，否则 Word 可能报错或丢边框。
- `soffice --headless --convert-to pdf` 可能先返回、后写文件；立刻用 PyMuPDF 渲染会得到旧 PDF。应使用独立 `-env:UserInstallation=file:///D:/lo-profile-xxx`，转换后确认 PDF mtime 晚于 docx，再单独一步渲染页面检查。
- 大量终端截图时按 `width=min(13.5cm, px/120*2.54, 17cm*px/h)` 定宽，既保证可读又能让两张路由表落在同一页；超过约 17cm 高的图会独占一页并留下大片空白。
- 校验：重开 docx 统计 `inline_shapes`（图片 + 文本框）、检查 图/表 编号连续、扫描 document.xml 只出现宋体/Times New Roman，再用 PDF 逐页看裁切与分页。
