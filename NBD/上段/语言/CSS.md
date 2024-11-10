--层叠样式表CSS

‍

‍

**CSS(Cascading Style Sheets，层叠样式表)** 是用来为结构化文档(如 HTML 文档或 XML 应用)添加样式的计算机语言, 规则由两个主要的部分构成：选择器，以及一条或多条声明

实现了网页中数据和样式的分离，使网页结构更加明细，解决了样式重复定义的问题，提高了开发效率和后期代码的可维护性，另外还增强了网页的美化能力. 

‍

### Header

‍

#### 快速

‍

[菜鸟实例](https://www.runoob.com/css/css-navbar.html)

[CSS 参考手册](https://www.runoob.com/cssref/css-reference.html)

[CSS按钮生成器](https://c.runoob.com/front-end/6222/)

‍

‍

# 知识

‍

## 网页组织

方式

‍

* 表格套表格方式定义网页结构：目前不是主流，只在一些结构简单的页面中有所使用
* div +css方式定义网页结构：目前主流的网页开发方式，可以非常灵活的定义网页

‍

## 设计

‍

### 颜色

* 十六进制 - 如："#ff0000"
* RGB - 如："rgb(255,0,0)"
* 颜色名称 - 如："red"

‍

‍

## 格式

‍

### 注释

只有一种

```html
/* */
```

‍

### 属性值

‍

不要在属性值与单位之间留有空格<sup>（如：&quot;margin-left: 20 px&quot;）</sup>

‍

### 声明

声明总以大括号 {} 括起来, 以分号 ; 结束

‍

‍

# 基础

‍

‍

## 搭建

一般单独写css到css包里,直接引用即可<link rel="",href="">

‍

‍

## 容器标签

本身没有任何特殊的能力，最主要的功能是用来包含其他标签组成一个整体

‍

常见的容器标签

​`<div>`​ 块级元素. 这意味着它的内容自动的开始一个新行

​`<p>`​ 块级元素. 用来声明一个段落. 会在当前段落前后多出额外的空白行

​`<span>`​ 行内元素. 多个行内元素不会要求独占浏览器一行

‍

‍

## 样式引入

‍

‍

### 外部样式表

‍

html和css实现了完全的分离, 其不能包含任何的 html 标签, 以 .css 扩展名进行保存

当样式需要应用于很多页面时，外部样式表将是理想的选择, 每个页面使用 <link> 标签链接到样式表

这样可以通过改变一个文件来改变整个站点的外观

‍

#### 示例

引用

```html
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

```css
hr {color:sienna;}
p {margin-left:20px;}
body {background-image:url("/images/back40.gif");}
```

‍

css文件

```css
span {
    background-color: burlywood;
    color: fuchsia;
}
p{
    background-color: #bdde4f;
    color: #ff4400;

}
```

‍

import导入外部文件

```html
<style>

@import "demo06.css";

</style>
```

‍

‍

### 内嵌样式表

‍

<style>在头部定义内部样式表

单个文档需要特殊的样式时使用, 只在当前的HTML中有效

‍

‍

#### 实例

```html
<head>
    <style>
        /*对所有的div添加背景颜色*/
        div {
            background-color: #d683fb;/*背景颜色*/
            color: #25fffc;/*字体颜色*/
        }
    </style>
</head>
```

‍

‍

‍

‍

### 内联(行内)样式表

相关的标签内使用style属性

‍

‍

内联样式会损失掉样式表的许多优势

* 所有样式都写在一个标签中，代码不够简洁
* 样式**只在一个标签中有效**，其他标签无效

‍

#### 实例

```html
<div style="background-color: cadetblue;color: burlywood">
    第一个div  
</div>
```

‍

‍

### 行内样式

HTML的直接在标签名后面追加样式

‍

### 多重样式

如果某些属性在不同的样式表中被同样的选择器定义，那么属性值将从更具体的样式表中被继承过来. 

‍

## 优先级

优先级仅由选择器组成的匹配规则决定

‍

优先级逐级增加的选择器列表

* 通用选择器（*）
* 元素(类型)选择器
* 类选择器
* 属性选择器
* 伪类
* ID 选择器
* 内联样式

‍

> （内联样式）Inline style > （内部样式）Internal style sheet >（外部样式）External style sheet > 浏览器默认样式

‍

**如果外部样式放在内部样式的后面，则外部样式仍然将覆盖内部样式**

‍

‍

### !important 规则

当 !important 规则被应用在一个样式声明中时,该样式声明会覆盖<sup>（尽管如此, !important规则还是与优先级毫无关系.使用 !important 不是一个好习惯）</sup>CSS中任何其他的声明

‍

‍

### 经验

* **Always** 考虑优化使用样式规则的优先级来解决问题, 而不是 `!important`​
* **Only**  在需要**覆盖全站或外部css的特定页面**中使用 `!important`​
* **Never** 在全站范围的 css 上使用`<span> </span> !important`​
* **Never** 在插件中使用 `!important`​

‍

## 选择器

> 作用：选择HTML中的标签的，对标签做样式的装饰
>
> CSS选择器的分类
>
> * 基本选择器：3种
> * 扩展选择器：6种
>
> CSS选择器优先级： ==行内式（inline）-&gt;ID选择器-&gt;类选择器-&gt;标签选择器==

‍

‍

‍

‍

### 基本选择器

‍

‍

#### 元素选择器

(标签选择器)

‍

直接指定对应标签, 选择器的名字必须是标签的名字

样式会作用于所有同名的标签上

```html
标签名{}
```

```css
元素名称 {
    css样式名:css样式值；
}
```

```css
div {
    color: coral;
    background-color: cornsilk;
}
```

‍

‍

#### id选择器

‍

以id属性来设置id选择器,选择器的名字前面需要加上#

样式会作用于指定id的标签上，而且有且只有一个标签

每一个HTML标签都有一个id属性

id：标签的唯一标识

```html
#id属性值{}
```

```css
#id属性值 {
    css样式名:css样式值；
}  
```

```css
#d1 {
    color: deepskyblue;
    background-color: brown;
}

<div id="d1">我是一个块级元素，测试元素选择器</div>
```

‍

#### class选择器

使用 class指定特定的元素, 又称类选择器

样式会作用于所有class的属性值和该名字一样的标签上.

HTML中以 class 属性表示, CSS中，以一个点 **.** 号显示

‍

根据标签中的class属性选择，可选多个

```html
.class属性值{}
```

```css
.class属性值 {
    css样式名:css样式值；
}
```

```css
所有的 p 元素使用 class="center" 让该元素的文本居中:
p.center {text-align:center;}

多个 class 选择器可以使用空格分开：
.center { text-align:center; }
.color { color:#ff0000; }
```

```css
.c1 {
    color: deeppink;
}
<div class="c1">我是一个块级元素，测试元素选择器</div>
<div class="c1">我是一个块级元素，测试元素选择器</div>
<div class="c1">我是一个块级元素，测试元素选择器</div>
```

‍

### 扩展选择器

‍

#### 后代选择器

‍

HTML标签有多层嵌套关系，不止有子节点，还有孙子节点等等

​`父节点 子节点 {}`​

```css
div span {
    color: deepskyblue;
}

<div id="d2">
    测试后代选择器
    <div>这是子节点div
    	<span>这是孙子节点span</span>
    	<p>这是孙子节点p</p>
    </div>
    <span>这是子节点span</span>
    <p>这是子节点p</p>
</div>
```

‍

#### 子元素选择器

选择特定的标签的子节点，不包含孙子节点

​`父节点>子节点 {}`​

```css
#d2>p {
    color: orangered;
}
<div id="d2">
	测试后代选择器
	<div>这是子节点div
		<span>这是孙子节点span</span>
		<p>这是孙子节点p</p>
	</div>
	<span>这是子节点span</span>
	<p>这是子节点p</p>
</div>
```

‍

#### 分组选择器

可以同时选择多个不同的标签，分成一组设置样式

​`元素选择器,id选择器,class选择器{}`​

```css
span,p,#d1,.c1 {
    color: green;
}
```

‍

#### 嵌套选择器

* **p{ }** : 为所有 **p** 元素指定一个样式.
*  **.marked{ }** : 为所有 **class=&quot;marked&quot;**  的元素指定一个样式.
*  **.marked p{ }** : 为所有 **class=&quot;marked&quot;**  元素内的 **p** 元素指定一个样式.
* **p.marked{ }** : 为所有 **class=&quot;marked&quot;**  的 **p** 元素指定一个样式.

‍

#### 属性选择器

包含所有属性type的标签

可以选择指定标签的某个属性

可以选择指定标签的某个 属性="值" 的形式

可以选择指定标签的 多个 属性键值对 的形式

‍

* **HTML代码：**

```html
<input type="text" name="username" value="测试"/><br>
<input type="password" name="password" value="123"/><br>
<input type="email" name="email"/><br>
```

* 包含所有属性type的标签

  ```css
  *[type] {
      background-color:silver;
  }
  ```
* 可以选择指定标签的某个属性

  ```css
  input[value] {
      background-color: darksalmon;
  }
  ```
* 可以选择指定标签的某个 属性="值" 的形式

  ```css
  input[name="password"] {
      background-color: deepskyblue;
  }
  ```
* 可以选择指定标签的 多个 属性键值对 的形式

  ```css
  input[type][name][value] {
      background-color: mediumpurple;
  }
  ```

‍

#### 伪元素选择器

“伪”：HTML已经准备好的一些选择器

‍

​`:link`​:没有被点击

​`:visited`​:被点击过

​`:hover`​:鼠标停留

​`:active`​:鼠标正在被点击

‍

‍

```html
<a href="#">测试伪元素选择器</a>
```

* 未被点击的状态

  ```css
  a:link{/*未被点击的状态*/
      background-color: wheat;
  }
  ```
* 被点击过的状态

  ```css
  a:visited{/*被点击过的状态*/
      background-color: tomato;
  }
  ```
* 鼠标停留的状态

  ```css
  a:hover{/*鼠标停留的状态*/
      background-color: green;
      font-size: 20px;
  }
  ```
* 被鼠标正在点击的状态

  ```css
  a:active{/*被鼠标正在点击的状态*/
      background-color: aquamarine;
  }
  ```

‍

‍

## 特定对象

‍

### 背景

‍

* background-color
* background-image  背景图<sup>（默认情况下，背景图像进行平铺重复显示，以覆盖整个元素实体.）</sup>
* background-repeat
* background-attachment
* background-position

简写属性

属性值的顺序为上面表的顺序

‍

‍

### 文本

‍

text-decoration    设置或删除文本的装饰    主要是用来删除链接的下划线

text-indent    指定文本的第一行的缩进

‍

‍

#### 对齐方式

当text-align设置为"justify"，每一行被展开为宽度相等，左，右外边距是对齐

‍

#### 文本转换

用来控制在一个文本中的大写和小写字母

```css
p.uppercase {text-transform:uppercase;}
p.lowercase {text-transform:lowercase;}
p.capitalize {text-transform:capitalize;}
```

‍

#### 字体

‍

使用 **CSS3** 可以使用任何字体, 只需简单的将字体文件包含在网站中，它会自动下载给需要的用户

‍

一般CSS中，有两种类型的字体系列名称：

* **通用字体系列** - 拥有相似外观的字体系统组合
* **特定字体系列** - 一个特定的字体系列

‍

##### 字体系列

font-family 属性设置文本的字体系列, 设置几个字体名称作为一种"后备"机制 -> 如果浏览器不支持第一种字体，他将尝试下一种字体

多个字体系列用一个逗号分隔

如果字体系列的名称超过一个字必须用引号

‍

‍

字体样式

* 正常 - 正常显示文本
* 斜体 - 以斜体字显示的文字
* 倾斜的文字 - 文字向一边倾斜<sup>（（和斜体非常类似，但不太支持））</sup>

‍

##### 字体大小

如果不指定，默认大小和普通文本段落一样16像素（16px=1em）

绝对大小：    设置一个指定大小的文本, 不允许<sup>（确定了输出的物理尺寸时绝对大小很有用）</sup>用户在所有浏览器中改变文本大小  
相对大小：    相对于周围的元素来设置大小,允许用户在浏览器中改变文字大小

‍

‍

### 多媒体

‍

* src    规定视频url
* controls    显示播放控件
* width    播放器的宽度
* height    播放器的高度

‍

‍

### 链接

‍

* href    指定资源访问的url
* target    指定在何处打开资源链接
* _self    默认值，在当前页面打开
* _blank    在空白页面打开
* text-decoration    删除链接中的下划线

‍

链接的样式取决于链接状态,也有逻辑上的顺序规则

> a:hover 必须跟在 a:link 和 a:visited后面; a:active 必须跟在 a:hover后面

‍

* a:link - 正常，未访问过的链接
* a:visited - 用户已访问过的链接
* a:hover - 当用户鼠标放在链接上时
* a:active - 链接被点击的那一刻

示例

```html
<style>
    a:link {color:#000000;}      /* 未访问链接*/
    a:visited {color:#00FF00;}  /* 已访问链接 */
    a:hover {color:#FF00FF;}  /* 鼠标移动到链接上 */
    a:active {color:#0000FF;}  /* 鼠标点击时 */
</style>
```

‍

‍

‍

### 表格

‍

* border    表格边框的宽度
* width    表格的宽度
* cellspacing    单元之间空间
* width和height    宽度和高度

‍

#### ==边框==

border

‍

* border-collapse    设置边框是否被折叠成一个单一的边框或隔开

‍

#### ==对齐==

‍

* text-align    水平对齐<sup>（向左，右，或中心）</sup>
* vertical-align    垂直对齐<sup>（顶部，底部或中间）</sup>

‍

#### ==边框==

border-style

‍

* none: 默认无边框
* dotted: 定义一个点线边框
* dashed: 定义一个虚线边框
* solid: 定义实线边框

‍

* border-width    边框宽度<sup>（比如 2px 或 0.1em(单位为 px, pt, cm, em 等)，或者使用 3 个关键字之一: thick 、medium（默认值） 和 thin）</sup>
* border-color    颜色

‍

==轮廓==

outline

‍

==边距==

margin

‍

|值|说明|
| :-----| :-------------------|
|auto|自动设置浏览器边距<sup>（结果会依赖于浏览器）</sup>|
|*length*|定义固定的margin|
| *%*|定义使用百分比边距|

‍

==填充==

padding<sup>(当元素的 padding（填充）内边距被清除时，所释放的区域将会受到元素背景颜色的填充. 单独使用 padding 属性可以改变上下左右的填充. )</sup>

‍

|值|说明|
| :---| :-------------------|
|*length*|定义固定的填充|
| *%*|使用百分比定义填充|

‍

## 通用属性

‍

* margin：0 auto 左右居中
* line-height: px 等于行高时，自动垂直居中
* float：left/right 浮动
* resize：none 禁止textarea拖拽

‍

#### 尺寸

允许你控制元素的高度和宽度. 同样，它允许你增加行间距. 

‍

‍

# 高级

‍

## 显示

‍

### 隐藏

‍

display / visibility    指定一个元素应可见还是隐藏

‍

#### visibility:hidden

可以隐藏某个元素，但隐藏的元素仍需占用一样的空间,仍然会影响布局

‍

#### display:none

隐藏某个元素，且不会占用任何空间. 也就是说无了

‍

### 堆叠

z-index

指定元素堆叠顺序,元素可以有正数或负数的堆叠顺序

具有更高堆叠顺序的元素总是在较低的堆叠顺序元素的前面

> 如果两个定位元素重叠，没有指定z - index，最后定位在HTML代码中的元素将被显示在最前面.

‍

### 溢出

overflow

控制内容溢出元素框时显示的方式, 默认情况下值为 visible，内容溢出元素框

‍

### 浮动

Float

使元素向左或向右浮动，其周围的元素也会重新排列, 避免这种情况，则使用 clear 属性

‍

元素的水平方向浮动，意味着元素只能左右移动而不能上下移动. 一个浮动元素会尽量向左或向右移动直到它的外边缘碰到边框为止. 把几个浮动的元素放到一起，如果有空间的话它们将彼此相邻. 

‍

### 多列

可以将文本内容设计成像报纸一样的多列布局

‍

‍

## 定位

position

指定元素定位类型

‍

1. ==static 定位==

    默认值即没有定位，遵循正常的文档流, 不会受到 top, bottom, left, right影响
2. ==fixed 定位==

    元素相对于窗口是固定位置
3. ==relative 定位==

    相对定位是相对其正常位置. 移动相对定位元素<sup>（相对定位元素经常被用来作为绝对定位元素的容器块）</sup>，但它原本所占的空间不会改变
4. ==absolute 定位==

    位置相对于最近的已定位父元素，如果没有已定位父元素，那么位置相对于<html>.  
    使元素的位置与文档流无关，因此不占据空间<sup>（absolute 定位的元素和其他元素重叠）</sup>.
5. ==sticky 定位==

    粘性定位, 基于用户<sup>（在相对定位与固定位置定位之间切换）</sup>的滚动位置来定位, 行为<sup>（元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位）</sup>就像 **position:relative;**  而当页面滚动超出目标区域时，它表现像 **position:fixed,** 它会固定在目标位置. 指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效. 否则其行为与相对定位相同.

‍

‍

## **组合器**

‍

*  **' '**  后代组合器（Descendant combinator）
*  **'&gt;'**  直接子代组合器（Child combinator）
*  **'~'**  一般兄弟组合器（General sibling combinator）
*  **'+'**  紧邻兄弟组合器（Adjacent sibling combinator）

‍

‍

## 伪类/伪元素

用来添加一些选择器的特殊效果

‍

‍

# 实操

‍

## Bugfix
