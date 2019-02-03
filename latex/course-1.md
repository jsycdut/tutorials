# latex-course-1 basic

本文介绍简单的，laetx写文档的基本概念。

1. 文档类型
2. 作者标记
3. 章节与子章节
4. 斜体 粗体 强调
5. 新开一行

LaTex，其实是TeX的子集，具体内容，参见[WikiPedia](https://en.wikipedia.org/wiki/LaTeX)。

其实很简单，LaTeX就像是Markdown一样，是标记语言，你只需要用那些已经设定好的标记将你的文本包围起来，就可以了。

* 文档类型

文档类型，就是这篇文档，是文章，还是书籍，还是报告，使用`\documentclass{}`将你的文档类型包围起来，比如`\documentclass{article}`，一般这些标记放在一片文档的开头。

* 作者标记

作者标记，用的是`\author{}`，以及配合`\title{}`和正文中的`\maketitle`来实现标题，在花括号中写入对应的内容即可。

* 章节与子章节

`\section{} \subsection{}`对应着章节和子章节，而且是自动给你排好序的。

* 斜体 粗体 强调

`\textit{}`对应着斜体，英文是斜体，中文却给你整成楷体字，`\textbf{}`对应着粗体，对英文粗体没错，中文也是粗体，`\emph{}`也送出一个强调，英文看起来和斜体差不多，中文嘛，看起来，还是楷体。

* 新开一行

新开一行，简单，你连续两个回车再写内容，就是新开一行了，或者要图简单的话，在行末写个`\newline`，也给你新开一行。但是，双回车之后写的文本，会自动空两格，也就是出现了缩进，`\newline`则是没有缩进的，直接给你顶格写。

## 例子

下面是一个例子，为了支持中文，引入了ctex包，然后基础实现了上面的内容。
```latex
%文档类
\documentclass{article} 

 %使用宏包ctex，支持中文
\usepackage[UTF8]{ctex}

%作者
\author{金世钰 Daemon Foxx} 

%标题
\title{ {\LaTeX} 文档试验品} 

 %文档开始
\begin{document}

%打印标题
\maketitle 

% 定义段落
\section{过年回家被相亲} 

此处省略八百字\dots % 省略号

% 另一个段落
\section{好气啊\dots} 
两格回车换个新行

 % 子段落
\subsection{好气啊，第一集}

%粗体
This is a line of subsection, and \textbf{bold 粗体} 

%斜体
\textit{This should be italic but 中文却不是} 

 \emph{emphasized  text looks like italic but 中文也不一样} 

%下划线
\underline{下划线文字} 

%不正常的双引号
"这个双引号不正常"

%正常的单引号
`正常的单引号'

% 正常的双引号
``正常的双引号'' 

 % 文档终结
\end{document}
```

## 视频资源

* [Basic Compiling, Titles, Scections, Formatting and Syntax](https://www.youtube.com/watch?v=mfRmmZ_84Mw&list=PL-p5XmQHB_JSQvW8_mhBdcwEyxdVX0c1T&index=2)
