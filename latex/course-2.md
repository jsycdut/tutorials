# course-2

本文主要涉及，有序列表，无序列表以及重叠嵌套，字体大小。

* enumerate

* itemize

列表不管是有序还是无序，都需要嵌套到一个`begin end`标签块里面。

## enumerate

使用`enumerate`标签做一个有序列表，里面的子项目使用`\item content`作为子标签
```tex
\begin{enumerate}
\item first
\item second
\end{enumerate}
```

## itemize

使用 `itemize`标签做一个无序列表，里面的子项目使用`\item content`作为子标签
```tex
\begin{itemize}
\item unsort 1
\item unsort 2
\item unsort 3
\end{itemize}
```

## 字体大小

使用`\tiny \small \normalsize \large \Large |LARGE`声明字体的相对大小
```tex
\LARGE{最大} \Large{次大} \large{大} \normalsize{常规} \small{小} \tiny{最小}
```
## 列表嵌套
至于列表嵌套，直接在列表里面嵌入另外一个同样结构的列表即可。
```tex
\section{有序列表}

\begin{enumerate}
\item 编程语言
\begin{enumerate}
\item Java
\item C
\item C++
\item node.js
\end{enumerate}
\item 编辑器
\begin{itemize}
\item Vim
\item Emacs
\end{itemize}
\end{enumerate}

\section{无序列表}
\begin{itemize}
\item 三上悠亚
\item 水野朝阳
\item 佐々木あき
\item 宇都宫紫苑
\end{itemize}

```

## 实例

以下代码，使用涵盖了必要的注释和本文所有涉及到的知识点。

```tex
\documentclass{article}

\usepackage[UTF8]{ctex}

\title{Introduction to {\LaTeX}}
\author{金世钰 Daemon Foxx}
\date{\today}

\begin{document}
\maketitle

% 字体相对大小
\LARGE{最大} \Large{次大} \large{大} \normalsize{常规} \small{小} \tiny{最小} \normalsize{}

% 居中实例

\begin{center}
\textit{\tiny{携手揽腕入罗帷}}

\textit{\small{含羞带笑把灯吹}}

\textit{\normalsize{金针刺破桃花蕊}}

\textit{\large{不敢高声暗皱眉}}
\end{center}

% 有序列表以及嵌套
\section{语言以及编辑器}

\begin {enumerate}
\item 编程语言
\begin{enumerate}
\item Java
\item C
\item C++
\item node.js
\end{enumerate}
\item 编辑器
\begin{itemize}
\item Vim
\item Emacs
\end{itemize}
\end{enumerate}

% 无序列表以及嵌套
\section{无名教师}

\begin{itemize}
\item 三上悠亚
\item 宇都宫紫苑
\end{itemize}

\end{document}
```

效果如下

![NO](https://raw.githubusercontent.com/jsycdut/photos/master/latex/enumerate-and-itemize.png)
