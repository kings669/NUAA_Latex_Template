# NUAA LaTeX Template

南京航空航天大学2026专业学位硕士学位论文模板，依据提供的撰写要求和专硕 Word 模板整理。

## 目录结构

```text
NUAA_Latex_Template/
├── master.tex                  # 编译入口，选择个人稿或示例
├── nuaa-layout.tex             # 字体、版面、页眉页脚与标题样式
├── nuaa-frontmatter.tex        # 封面、声明、摘要、目录及清单的排版
├── examples/                   # 随模板更新的示例内容
├── thesis/                     # 自行创建，存放个人论文
├── fonts/                      # 编译所用字体
├── fig/                        # 兼容原模板的图片路径
└── preview/
    └── NUAA_Latex_Template.pdf
```

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

## 开始写论文

先复制一份可编译的内容作为起点：

```sh
cp -R examples thesis
```

根目录存在 `thesis/metadata.tex` 时，`master.tex` 从 `thesis/` 读取内容，否则编译示例。已有 `thesis/` 时直接编辑其中的文件。

| 文件 | 内容 |
|---|---|
| `thesis/metadata.tex` | 中英文封面信息及摘要关键词 |
| `thesis/abstract-zh.tex`、`thesis/abstract-en.tex` | 中英文摘要正文 |
| `thesis/chapters.tex` | 正文入口，可以用 `\input{thesis/chapters/01-introduction}` 按章拆分 |
| `thesis/backmatter.tex` | 参考文献、致谢、研究成果及可选附录 |
| `thesis/figure-table-list.tex` | 示例的手写清单。删除此副本后会自动生成图、表清单 |
| `thesis/symbols.tex`、`thesis/abbreviations.tex` | 注释表和缩略词表的表格行。最后一行由模板加底线，无需写 \\ \hline。可省略对应文件 |
| `thesis/preamble.tex` | 可选，用于加载个人论文所需的宏包和自定义命令 |
| `thesis/fig/` | 论文图片 |

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

在 `thesis/chapters.tex` 或其引用的文件中增加章节：

```tex
\chapter{相关理论与技术}
\section{研究方法}
在此填写正文。
```

目录会自动提取标题并更新页码，不要手动修改生成的 `.toc` 文件。

### 图片和表格

图片放入 `thesis/fig/`，例如 `thesis/fig/system.pdf`：

```tex
\begin{figure}[htbp]
  \centering
  \includegraphics[width=.8\textwidth]{system.pdf}
  \caption{系统模型}
  \label{fig:system}
\end{figure}
```

图题置于图下，表题放在表格内容上方。通过 `\ref{fig:system}` 引用，图表注释可使用 `\figtabnote{说明文字}`。

自动图表清单从各图表的 `\caption` 提取条目，重复编译后更新页码。

### 参考文献

当前文后保留的是 Word 中的著录格式示例，需替换为真实文献。可在 `thesis/backmatter.tex` 中使用手写文献，将示例的“参考文献”标题及其条目整体替换为：

```tex
\begin{thebibliography}{99}
  \bibitem{sample} 作者. 文题. 刊名, 年, 卷(期): 起止页码.
\end{thebibliography}
```

正文用 `\cite{sample}` 自动生成上标顺序编号。绪论中的 `[1]` 是原 Word 演示标记，录入真实文献后也应替换为引用命令。不要同时保留原“参考文献”章标题与 `thebibliography`，避免双标题。

## 从模板同步排版更新

在 GitHub 创建一个空的私有论文仓库。克隆本模板，将模板远端改名为 `upstream`，然后把自己的私有仓库添加为 `origin`：

```sh
git clone git@github.com:kings669/NUAA_Latex_Template.git nuaa-thesis
cd nuaa-thesis
git remote rename origin upstream
git remote add origin git@github.com:你的GitHub用户名/nuaa-thesis.git
git push -u origin main
```

将个人内容提交到 `thesis/` 下。每次同步前先提交手头的论文修改，然后执行：

```sh
git fetch upstream
git merge upstream/main
latexmk -xelatex -outdir=build master.tex
```

检查 PDF 后可用 `git push origin main` 更新私有仓库。模板维护者请将示例修改放在 `examples/`。

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
