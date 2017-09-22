# LATEX

标签（空格分隔）： LATEX

---

[toc]

### 1.1 LATEX 语句

LaTeX每一行称作一条语句,共三种：命令、数据、注释。
命令分为：普通命令和环境。普通命令以 “\” 开头，大多只有一行；而环境包含一对起始声明和结尾声明，用于多行的场合。命令和环境可以互相嵌套。
数据就是普通内容。
注释语句以 “%” 起始，在编译过程中被忽略。

### 1.2 文档结构

#### 1.2.1 文档类、序言、正文

文档类声明用来指定文档的语言；
序言用来完成特殊任务，如引入宏包、定义命令、设置环境等；
正文就是文档的实际内容。即 \begin{document} 和 \end{document} 之间的部分。

```tex
\documentclass[options]{class}      % 文档类声明
\usepackage[options]{package}       % 引入宏包
...
\begin{document}                    % 正文
...
\end{document}
```

常用的文档类有三种：article、report、book

#### 1.2.2 标题、摘要、章节

```tex
\title{标题}
\author{作者}
\today
\maketitle      % 必须放在后面
```

```tex
\begin{abstract}
摘要
\end{abstract}
```

常用的层次结构：

```tex
\chapter{...}
\section{...}
\subsection{...}
\subsubsection{...}
```

> 注：高级层次可以包含若干低级层次；
article中没有chapter；
report和book支持上面的所有层次


#### 1.2.3 目录

```tex
\tableofcontents            % 生成整个文档的目录
\setcounter{tocdepth}{2}    % 指定目录层级深度
```

如果不想让某个章节标题出现在目录中，可以使用带 **\*** 的命令来声明章节

```tex
\chapter*{...}
\section*{...}
\subsection*{...}
```

生成插图和表格的目录命令：

```tex
\listoffigures
\listoftables
```

### 1.3 文字排版

#### 1.3.1 字符输入

文档中可输入内容大致分为：普通字符、控制符、特殊符号、拼音符号、预定义字符串等。
有两种输入模式：文本模式（缺省值）和数学模式。普通的行间数学模式用  \$...\$  来表示
