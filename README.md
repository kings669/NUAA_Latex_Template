# NUAA LaTeX Template

南京航空航天大学2026专业学位硕士学位论文单文件模板，依据提供的撰写要求和专硕 Word 模板整理。

## 目录结构

```text
NUAA_Latex_Template/
├── master.tex                       # 唯一需要编辑的 LaTeX 源文件
├── README.md                        # 使用说明
├── fonts/                           # 源码实际使用的10个字体文件
├── fig/                             # 放置自己的论文图片
└── preview/
    └── NUAA_Latex_Template.pdf       # 示例预览
```

目录不包含历史备份、排版对比记录、重复字体、旧文档类或编译中间文件。

## 编译

保留整个目录，使用已安装的 TeX Live 或 MacTeX，在项目根目录执行：

```sh
latexmk -xelatex -outdir=build master.tex
```

结果为 `build/master.pdf`。`preview/NUAA_Latex_Template.pdf` 是预先生成的示例，不会自动更新。

没有 latexmk 时，用 XeLaTeX 至少运行两次，以更新目录与交叉引用：

```sh
mkdir -p build
xelatex -interaction=nonstopmode -halt-on-error -output-directory=build master.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=build master.tex
```

使用 **XeLaTeX**，不能直接用 pdfLaTeX。字体按 `fonts/` 相对路径加载，无需全局安装。本模板直接使用标准 `ctexbook`，不需要自定义 `.cls`。

## 从哪里修改

源码按阅读顺序分区：

| 分区 | 内容 |
|---|---|
| 1. 基本信息 | 中英文题名、作者、学院、专业、导师、日期、分类号与论文编号 |
| 2. 字体与排版 | 字体、页面、行距、标题、页眉页脚和目录样式；一般无需修改 |
| 3—4. 封面 | 自动引用开头的中英文信息 |
| 5. 声明 | 独创性与使用授权声明，签名栏留空 |
| 6—7. 摘要 | 直接替换中英文摘要正文与关键词 |
| 8. 目录与前置表格 | 自动目录、示例图表清单、注释表、缩略词表 |
| 9. 正文 | 直接增加章、节与正文 |
| 10. 文后部分 | 参考文献、致谢、研究成果、附录 |

### 基本信息

修改开头的字段，例如：

```tex
\newcommand{\AuthorName}{你的姓名}
\newcommand{\CollegeName}{你的学院}
\newcommand{\ProfessionalCategory}{你的专业类别}
\newcommand{\ProfessionalField}{你的专业领域}
```

`\ThesisTitle` 用于页眉和文档信息，`\CoverTitle` 用于封面，可用 `\\` 手动换行。题目变更时同时修改两者。没有行业导师时，将 `\IndustrySupervisor` 内容留空。

中英文日期分别填写。当前保留 Word 的示例日期，**February, 2025 不是当前毕业日期**。专业类别、领域及英文授予学位名称应按学校对应表填写。

### 摘要与正文

当前是与 Word 对应的示例，需替换为自己的内容。硕士中文摘要按提供的要求为400—600字、关键词5—8个，中英文内容应对应。

在正文分区增加章节：

```tex
\chapter{相关理论与技术}
\section{研究方法}
在此填写正文。
```

目录会自动提取标题并更新页码，不要手动修改生成的 `.toc` 文件。

### 图片和表格

图片放入 `fig/`，例如 `fig/system.pdf`：

```tex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=.8\textwidth]{system.pdf}
  \caption{系统模型}
  \label{fig:system}
\end{figure}
```

图题置于图下，表题放在表格内容上方。通过 `\ref{fig:system}` 引用，图表注释可使用 `\figtabnote{说明文字}`。

当前图表清单保留 Word 示例名称与示例页码，**不会随插图自动更新**。实际写作后，可将清单标题下的示例条目替换为：

```tex
\makeatletter
\@starttoc{lof}
\bigskip
\@starttoc{lot}
\makeatother
```

### 参考文献

当前文后保留的是 Word 中的著录格式示例，需替换为真实文献。保持单文件可使用手写文献，将原“参考文献”标题及示例内容整体替换为：

```tex
\begin{thebibliography}{99}
  \bibitem{sample} 作者. 文题. 刊名, 年, 卷(期): 起止页码.
\end{thebibliography}
```

正文用 `\cite{sample}` 自动生成上标顺序编号。绪论中的 `[1]` 是原 Word 演示标记，录入真实文献后也应替换为引用命令。不要同时保留原“参考文献”章标题与 `thebibliography`，避免双标题。

## 当前排版约定

- 默认专业学位硕士学位论文。
- A4双面；左右边距不镜像：左3.0cm、右2.8cm，上下3.3cm。
- 页码奇数在右下角、偶数在左下角。
- 前置页用大写罗马数字 I、II、III…，绪论起用阿拉伯数字1、2、3…。
- 各部分正常换页，不强制奇数页起始，不自动补空白页。
- 正文宋体五号、固定20磅行距。
- 标题与页眉间距保留本次与 Word 核对后的设置。
- 缩略词是带边框的两列表格。

部分布局小数用于补偿 LaTeX 与 Word 的行框差异，不能直接视为 Word 原生段前段后数值。调整时建议保留副本，并同时检查页眉、标题和正文。

## 字体与交付

字体来自提供的简启体及本机已有字体；对外分发或使用前，请核实相应字体授权。该项目是整理后的 LaTeX 模板，并非学校官方发布的文档类。

正式提交前，请替换所有示例、日期及分类号，并按学院和印刷要求核对封面、签名与书脊。编译后可删除整个 `build/`，重新编译会自动生成；请保留源码、字体和自己的图片。
