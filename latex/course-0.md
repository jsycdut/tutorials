# LaTeX基础知识

### 引擎 格式 编译命令

以下内容引用自《106分钟LaTeX》第3页。

* 引擎

全称为排版引擎，是编译源代码并生成文档的程序，如TeX，pdfTeX，XeTeX，LuaTeX等引擎。

* 格式

是定义了一组命令的代码集。LaTeX就是最广泛应用的一个格式，高德纳本人还编写了一个简单的plain TeX格式，没有定义诸如\documentclass \section等命令。

* 编译命令

是实际调用的，结合了引擎和格式的命令。如xelatex是结合XeTeX引擎和LaTeX格式的一个编译命令。


以下是一个表格，显示了对应引擎，plain TeX格式，LaTeX格式以及产出文档格式。

| 引擎   | plain TeX格式 | LaTeX格式 | 产出文档格式 |
| :-:    | :-:           | :-:       | :-:          |
| TeX    | tex           | N/A       | dvi          |
| pdfTeX | etex          | latex     | dvi          |
|        | pdftex        | pdflatex  | pdf          |
| XeTeX  | xetex         | xelatex   | pdf          |
| LuaTeX | luatex        | lualatex  | pdf          |

对于中文环境，使用xelatex格式比较好，因为其支持UTF-8，其对应的引擎为XeTeX，调用命令为xelatex。
