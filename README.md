# NUAA LaTeX Template

南京航空航天大学专业学位硕士学位论文单文件模板，依据提供的2026年撰写要求和专硕 Word 模板整理。

## 目录结构

```text
NUAA_Latex_Template/
├── master.tex                       # 唯一需要编辑的 LaTeX 源文件
├── README.md                        # 本使用说明
├── fonts/                           # 排版实际使用的字体，不需要全局安装
├── fig/                             # 放置自己的论文图片
└── preview/
    └── NUAA_Latex_Template.pdf       # 当前示例的预览文件
```

未放入历史备份、排版对比记录、重复字体、旧文档类及编译中间文件。原目录和历史备份仍在上级目录中。

## 快速开始

1. 保留整个目录，尤其不要单独移走 `master.tex` 或 `fonts/`。
2. 使用已安装的 TeX Live 或 MacTeX，选择 **XeLaTeX** 编译引擎。
3. 在此目录打开终端，执行：

   ```sh
   latexmk -xelatex -outdir=build master.tex
   ```

4. 打开 `build/master.pdf` 查看自己编译的结果。`preview/NUAA_Latex_Template.pdf` 是随模板附带的示例，不会自动更新。

也可直接使用 XeLaTeX（至少运行两次，以更新目录和交叉引用）：

```sh
mkdir -p build
xelatex -interaction=nonstopmode -halt-on-error -output-directory=build master.tex
xelatex -interaction=nonstopmode -halt-on-error -output-directory=build master.tex
```

请在项目根目录运行命令。中文和字体配置依赖 XeLaTeX，不能直接用 pdfLaTeX 编译。本模板基于标准 `ctexbook`，不需要额外的 `nuaathesis.cls`。

## 从哪里修改

`master.tex` 已按以下顺序分区，可通过注释标题定位：

| 分区 | 修改内容 |
|---|---|
| 1. 基本信息 | 中英文题目、姓名、学院、专业类别、专业领域、导师、毕业年月、分类号及论文编号 |
| 2. 字体与排版 | 字体路径、纸张、页边距、标题、页眉页脚、目录样式；一般无需改动 |
| 3—4. 中英文封面 | 封面布局，由开头的信息字段自动填充 |
| 5. 声明 | 独创性及使用授权声明，签名位置留空 |
| 6—7. 中英文摘要 | 摘要正文和关键词，直接在相应位置替换 |
| 8. 目录与图表清单 | 自动目录、示例图表清单、注释表、缩略词表 |
| 9. 正文 | 由 `chapter`、`section`、`subsection` 组织论文内容 |
| 10. 文后部分 | 参考文献、致谢、研究成果及附录 |

### 基本信息

修改文件开头的字段，例如：

```tex

ewcommand{AuthorName}{你的姓名}

ewcommand{CollegeName}{你的学院}

ewcommand{ProfessionalCategory}{你的专业类别}

ewcommand{ProfessionalField}{你的专业领域}
```

`ThesisTitle` 用于页眉和文档信息；`CoverTitle` 用于封面，可用 `\` 手动换行。题目变更时请同时修改两者。没有行业导师时，将 `IndustrySupervisor` 的内容留空，封面会隐藏该行。

中文日期和英文日期分别填写，当前保留 Word 的示例日期；**英文的 February, 2025 不是当前毕业日期**。专业类别、领域和英文授予学位名称应根据学校对应表填写。

### 摘要与正文

示例内容与专硕 Word 模板对应，并非完整论文。硕士中文摘要按提供的要求为400—600字，关键词5—8个；中英文内容应对应。

增加章节时，直接在正文分区继续写：

```tex
chapter{相关理论与技术}
section{研究方法}
在此填写正文。
```

目录自动提取章节并更新页码，不要手动修改生成的 `.toc` 文件。

### 插图与表格

图片放入 `fig/`，例如 `fig/system.pdf`：

```tex
egin{figure}[htbp]
  centering
  includegraphics[width=.8	extwidth]{system.pdf}
  caption{系统模型}
  label{fig:system}
end{figure}
```

图题置于图下，表题应放在表格内容上方。正文通过 `
ef{fig:system}` 引用。图表的注释可使用 `igtabnote{说明文字}`。

当前图表清单保留了 Word 的示例名称和示例页码，**不会随插图自动更新**。实际写作后可将“图表清单”标题下的示例条目替换为：

```tex
makeatletter
@starttoc{lof}
igskip
@starttoc{lot}
makeatother
```

### 参考文献

当前文后保留的是 Word 中的“参考文献著录格式示例”，需替换为真实文献。保持单文件可使用手写条目：将原“参考文献”标题及其示例内容整体替换为：

```tex
egin{thebibliography}{99}
  ibitem{sample} 作者. 文题. 刊名, 年, 卷(期): 起止页码.
end{thebibliography}
```

正文使用 `cite{sample}` 自动生成上标顺序编号。本示例绪论中的 `[1]` 为原 Word 的演示标记，录入真实文献后也应替换为 `cite{...}`。不要同时保留空白“参考文献”章标题与 `thebibliography`，避免双标题。

## 当前排版约定

- 默认专业学位硕士学位论文，不是实践成果报告。
- A4双面；左右边距不镜像：左3.0cm、右2.8cm，上下3.3cm。
- 奇数页码在右下角，偶数页码在左下角。
- 前置页使用大写罗马数字 I、II、III…；绪论开始使用阿拉伯数字1、2、3…。
- 各部分正常换页，不强制奇数页起始，不自动补空白页。
- 正文宋体五号、固定20磅行距。
- 摘要、目录、前置标题和章标题保留本次核对后的间距设置。
- 缩略词为带边框的两列表格。

部分布局参数含小数，是为匹配 Word 导出页面对 LaTeX 行框做的补偿，不能直接当作 Word 原生段前段后值。若需统一改版，建议先保留副本，再同时检查标题、页眉与正文位置。

## 字体与交付

字体按相对路径加载。`fonts/` 只保留源码实际引用的字体。字体文件来自提供的简启体及本机既有字体；对外发布或转交前，请核实相应字体的授权。

此项目是整理后的 LaTeX 模板，并非学校官方发布的 LaTeX 文档类。正式提交前填写所有占位内容、日期和分类号，并根据学院及印刷要求核对封面、签名与书脊。

编译后可删除整个 `build/`，再次编译会自动生成；请保留 `master.tex`、`fonts/` 和自己的图片。
