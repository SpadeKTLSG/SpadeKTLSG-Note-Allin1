> 暂时记录

‍

‍

## 基本概念

### tex语言介绍

Tex是一种语言类型。同时其也是一种排版引擎。基本的TeX系统只有300多个元命令 (primitive) ，十分精悍，但是很难读懂。

### tex语言使用流程

> 语言格式.tex -> 编译程序tex/etex/latex -> .dvi -> 排版程序pdfTex/PdfLatex -> .Pdf

### tex语言格式分类

* Plain Tex是一种语言格式。最小宏集
* LaTeX也是一种语言格式。常见宏集合
* ConTeXt：另一种常见的格式。另一种常见宏集合

分别由Tex语言中不同的宏包定义的语言格式

‍

### tex编译排版介绍

#### tex语言编译工具

* tex命令是用来编译Plain Tex书写的.tex文件生成.dvi文件程序。
* etex命令是用来编译Plain Tex书写的.tex文件生成.dvi文件程序。
* latex命令用来编译使用LaTeX语言写的.tex文件生成为.dvi文件程序。

#### tex语言排版工具

dvipdfmx程序用来对dvi文件进行排版生成pdf文件。

#### tex语言编译排版工具

> 用来将tex文件直接编译成pdf文件。自动包括了编译和排版的过程

* PdfTex是用来排版Plain Tex语言格式的dvi文件，生成PDF文档。
* PdfLaTeX是用来排版LaTeX语言格式的dvi文件，生成PDF文档。
* xetex命令用来编译Plain TeX格式写的dvi文件。使用操作系统字符集，支持Unicode字符集。
* xeLatex命令用来编译LaTeX格式写的dvi文件。使用操作系统字符集，支持Unicode字符集。
* LuaTeX：TeX 语言的一个完整的有扩展的实现。LuaTeX支持Unicode、系统字体和内嵌语言扩展，能直接输出PDF格式文件，也可以仍然输出 DVI 格式。
* LuaLaTeX：TeX 语言的一个完整的有扩展的实现。LuaTeX支持Unicode、系统字体和内嵌语言扩展，能直接输出PDF格式文件，也可以仍然输出 DVI 格式。dvi

补充：

* latexmk 是一个集成命令工具，能够自动运行多次xelatex、biblatex等工具，一次运行多次编译。

‍

### tex发行版介绍

一个完整的TeX需要最基本的TeX引擎、格式支持、各种辅助宏包、一些转换程序、GUI、编辑器、文档查看器等等。通过选择不同的组合就构成了不同的发行版。

* TeX Live：支持Linux，Windows，Mac OS
* MiKTeX：只支持Windows
* CTeX：CTeX基于MiKTeX，并加入了中文的支持，只支持Windows。同时CTEX是一个网站，ctex是可以很好支持中文的宏包。

‍

## 文件结构

```text
\documentclass{article}
\title{hello}
\author{estom}
\usepackage{ctex}
\newcommad\degree{^\circ}
\begin{document}

\maketitle

\section{hello}
hello world殷康龙
\end{document}
```

‍

### 具体结构

#### 导言区

* 用来声明文档类型\documentclass{article}
* 文档的基础信息\title{hello}
* 导入的宏包\usepackage{ctex}
* 定义新的latex命令\newcommad\degree{^\circ}

#### 正文区

* 用来完成文档主体。\begin{document}
* 能够设置不同的环境\begin{equation}

> 不同的环境下能够编写不同形式的命令。

### 设置中文文档

* 使用\usepackage{ctex}宏包
* 使用ctex提供的\documentclass{ctexarticle}文档类
* 然后使用xelatex命令进行编译。

> ctex宏包主要提供中文字体的排版。xelatex命令主要实现utf8字体的编译。

### 查看帮助手册

* texdoc ctex可以查看帮助手册。
* texdoc lshot-zh可以查看一个简单的教程

‍

## 字体设置

‍

### 字体属性

‍

#### 字体编码

* 正文字体编码
* 数学字体编码

#### 字体族

* 罗马字体
* 无衬线字体
* 打字机字体

‍

字体族命令，作用于参数。  
\textrm{罗马字体} \textsf{无衬线字体} \texttt{打字机字体}

字体族声明，作用于后续文本  
\rmfamliy \sffamily \ttfamily  
hello world

可以使用大括号进行分组，限定字体声明的范围。

‍

#### 字体系列

* 粗细\textmd{Medium series} \mdseries
* 宽度\textbf{boldface series} \bfseries

#### 字体形状

* 直立\textup{} \upshape
* 斜体\textit{} \itshape
* 伪斜体\textsl{} \slshape
* 小型大写\textsc{} \sctext

#### 字体类型

> 中文字体设置在ctex宏包当中。

* 宋体\songti{}
* 黑体\heiti{}
* 仿宋\fangsong{}
* 楷书\kaishu{}

‍

#### 字体大小

> 基于文档类型的normalsize大小设置\document[12pt]{article},对于英文字体的normalsize只有10,11,12pt。字体大小由一系列声明（无参数命令）控制。

* \tiny
* \scriptsize
* \footnotesize
* \small
* \normalsize
* \large
* \Large
* \LARGE
* \huge
* \Huge
* ctex红包提供了中文字体大小设置基础字体大小。\zihao{5}或者\zihao{-4}表示基础字体为五号或者小四。

> 尽量使用\newcommand命令完成新命令的定义。

‍

### 命令类型

* 有参数命令\usepackage{ctex}
* 无参数命令\rmfamily
* 环境命令：\begin \end成对出现，在不同环境下有不同的latex命令。

‍

## 文档提纲

‍

### 构建

\section{}  
\subseciton{}  
\subsubsection{}

#### ctex文章结构

* 使用ctex宏包不改变section结构
* 使用ctexarticle文档类，section居中，可以通过\ctexset{}命令修改ctex中关于section的基础设置。
* 能够使用\ctexset{}命令设置大量关于section的细节，包括编号形式与位置。

#### 产生目录

\tableofcontents

#### 换行方法

* 插入空行
* \
* \par命令

> 养成内容与格式分离的习惯。

‍

## 特殊字符

‍

### 空白符

* 空行产生分段，多个空行等同一个
* 空格产生空格，多个空格等于一个，中文中没有空格
* 自动缩进，不需要空格实现缩进，禁止使用全角空格
* 使用\quad\qquad\空格a~b硬空格\kern\hskip\hspace{}\hphantom{}\hfill实现空格。

### 控制符

\# \% \$ \textbackslash\{\}

### 排版符号

### 标志符号

### 引号

### 连字符

### 非英文字符

### 重音符号

‍

‍

## 插图

### 引入宏包

\usepackage{graphicx}

‍

### 指定图片搜索路径

\graphicspath{{figures/},{pics/}}

‍

### 导入图片

\includegraphics[选项]{文件名}

* scale指定缩放比例
* height指定高度
* width指定宽度
* angle旋转角度  
  格式PDF、EPS、PNG、JPEG、BMP

‍

## 表格

### 生成表格

使用tabular 环境生成表格  
/begin{tabular}{l | c || c |p{0.5cm}| r}  
a & b & c & d & e\\  
\hline 横线  
/end{tabular}

### 三线表

### 长跨页表格

### 说明文档

textdoc booktab

textdoc longtab

‍

‍

## 浮动体管理

### figure 浮动体环境

\ref{}命令能够引用标签

\begin{figure}[htbp]指定排版位置，实现灵活分页  
\centering 居中排版文职

\caption{} 设置标题  
\label{}设置浮动体标签  
\end{figure}

### table 浮动体环境

\ref{}命令实现表格交叉应用

\begin{table}  
\centering

\captiong{}设置标题  
\label{}设置浮动体标签  
\end{table}

‍

‍

## 数学公式

‍

### 行内公式

```xml
- $ $
- \( \)
- begin{math} end{math}

```

‍

### 行间公式

‍

```xml
- $$ $$
- \[ \]
- begin{displaymath} end{displaymath}
```

‍

‍

### 行间公式自动编号

\usepackage{asmath}

\ref{}  
begin{equation}

‍

\label{}设置标签进行交叉引用

end{equation}

不需要的自动编号

```xml
\begin{equation*}
\end{equation*}
```

‍

## 矩阵排版

‍

### matrix环境

#### 导入amsmath宏包

/usepackage{smsmath}

#### matrix环境

begin{matrix}  
end{matrix}

begin{bmatrix}  
中括号  
end{bmatrix}

begin{Bmatrix}  
大括号  
end{Bmatrix}

begin{vmatrix}  
单竖线  
end{vmatrix}

begin{Vmatrix}  
双竖线  
end{Vmatrix}

#### 省略号实现

\dots

\vdots

\ddots

#### 分块矩阵

begin{pmatrix}  
分块矩阵  
end{pmatrix}

#### 行内矩阵

begin{smallmatrix}  
end{smallmatrix}

#### 行列合并

\multicolumn{2}{c}\raisebox{调整高度}

#### 跨行括号

\left(  \right)

#### array环境实现矩阵

同tabular环境  
begin{array}  
end{array}

可以使用array环境的嵌套实现更高级的矩阵。

‍

‍

## 多行公式

```xml

\begin{gather}
每一行都带编号\\
\notag 阻止公式编号
\end{gather}

\begin{gather*}
不带编号的多行公式\\
\end{gather*}

begin{align}
代编号的对齐公式
end{align}

begin{align*}
不带编号的对齐公式
end{align*}

begin{equation}
begin{split}
一个公式的分行排版
end{split}
end{equation}

begin{equation}
begin{case}
分段函数的排版
end{case}
end{equation}

```

‍
