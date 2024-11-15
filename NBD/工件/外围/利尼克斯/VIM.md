‍

‍

## 注意

惊叹号 (!) 常常具有『强制』的意思

首先，不要相信 vim 是地表最强编辑器之类的鬼话. vim 在控制台应用里有一定应用，但其产生的 swp 文件可能造成信息泄露，所以不管是在开发环境还是在生产环境大量使用 vim 都是强烈不建议的，投入大量时间研究其插件体系也是极大的浪费. 

‍

‍

## 模式

‍

命令模式	command-mode	用于输入指令，如：保存、运行、切换标签、切割屏幕等

插入模式	insert-mode	也即编辑模式，用于编辑文本

可视模式	visual-mode	相当于高亮选取文本后的正常模式

正常模式	normal-mode	用于查看文本，也可复制、粘贴、撤销、重做等, 也可称为底线命令模式（Last line mode）

‍

‍

**命令**

‍

用户刚刚启动 vi/vim，便进入了命令模式.  任何时候，不管用户处于何种模式，只要按一下ESC键，即可使Vi进入命令模式；

此状态下敲击键盘动作会被Vim识别为命令，输入 :  可切换到底线命令模式，以在最底一行输入命令.   
若想要编辑文本：按下i，切换到输入模式

‍

**输入**

‍

在命令模式下输入插入命令i、附加命令a 、打开命令o、修改命令c、取代命令r或替换命令s都可以进入文本输入模式. 

在该模式下，用户输入的任何字符都被Vi当做文件内容保存起来，并将其显示在屏幕上. 在文本输入过程中，若想回到命令模式下，按键ESC即可. 

‍

**底行**

‍

在命令模式下按下:（英文冒号）就进入了底行命令模式. 

‍

==按:冒号即可进入last line mode==

```python
:set nu     列出行号
:set nonu   取消行号
:#7     跳到文件中的第7行

/keyword 查找字符  按n向下
?keyword 查找字符  按N向下
:n1,n2/word1/word2/gc  替换指定范围单词，c表示提示

:w 保存文件
:w filename 以指定的文件名另存

:n1,n2 w [filename] 将 n1 到 n2行另存
:r [filename]   读入另一个文件加到光标所在行后面

:! ls /home  在vi当中察看ls输出信息！
:q    离开vi

:wq 和 :ZZ 和 :x     保存并退出vi
!        强制执行

:% s/^/#/g 来在全部内容的行首添加 # 号注释
:1,10 s/^/#/g 在1~10 行首添加 # 号注释
```

‍

==从command mode进入Insert mode==

```python
按i在当前位置编辑
按a在当前位置的下一个字符编辑
按o插入新行，从行首开始编辑
按R(Replace mode)：R会一直取代光标所在的文字，直到按下 ESC为止；(常用)
```

‍

==按ESC键退回command mode==

h←j↓k↑l→前面加数字移动指定的行数或字符数

‍

‍

## 直属

‍

**翻页bu上下整页，ud上下半页**

ctrl+b：上移一页  
ctrl+f：下移一页  
ctrl+u：上移半页  
ctrl+d：下移半页

‍

**行定位**

Xgg或XG：定位第X行首字符  
G：移动到文章的最后  
7H:当前屏幕的第7行行首  
M：当前屏幕中间行的行首  
7L:当前屏幕的倒数第7行行首

‍

**当前行定位**

$：移动到光标所在行的“行尾”  
0或^：移动到光标所在行的“行首”  
w：光标跳到下个单词的开头  
e：光标跳到下个单词的字尾  
b：光标回到上个单词的开头

‍

**编辑**

x：剪切当前字符  
7x：剪切从当前位置起7个字符  
大写的X，表示从前面一个字符开始往前计算  
dd：剪切光标所在行.   
7dd：从光标所在行开始剪切7行

d7G 删除光标所在到第7行的所有数据

yw：复制当前单词  
7yw：复制从当前位置起7个单词  
yy：复制当前行  
6yy：从当前行起向下复制6行  
y7G 复制游标所在列到第7列的所有数据

p：粘贴

u：撤销  
ctrl+r：取消撤销  
cw：删除当前单词(从光标位置开始计算)，并进入插入模式  
c7w：删除7个单词并进入插入模式

‍

**多行编辑**

按ctrl+V进入块模式，上下键选中快，按大写G选择到末尾，上下左右键移动选择位置

按大写I进去编辑模式，输入要插入的字符，编辑完成按ESC退出

选中要替换的字符后，按c键全部会删除，然后输入要插入的字符，编辑完成按ESC退出

按shift+V可进入行模式，对指定行操作

‍

‍

‍

## **文件**

‍

vim filename	    打开或新建一个文件，并将光标置于第一行的首部  
vim -r filename	    恢复上次 vim 打开时崩溃的文件  
vim -R filename	    把指定的文件以只读方式放入 Vim 编辑器中  
vim + filename	    打开文件，并将光标置于最后一行的首部

vi +n filename	    打开文件，并将光标置于第 n 行的首部  
vi +/pattern filename	    打幵文件，并将光标置于第一个与 pattern 匹配的位置  
vi -c command filename	    在对文件进行编辑前，先执行指定的命令

‍

‍

## 插入模式

‍

i	在当前光标所在位置插入，光标后的文本相应向右移动  
I	在光标所在行的行首插入，行首是该行的第一个非空白字符，相当于光标移动到行首执行 i 命令

o	在光标所在行的下插入新的一行. 光标停在空行首，等待输入文本  
O	在光标所在行的上插入新的一行. 光标停在空行的行首，等待输入文本

a	在当前光标所在位置之后插入  
A	在光标所在行的行尾插入，相当于光标移动到行尾再执行 a 命令

esc键	退出编辑模式

r    替换只会取代光标所在的那一个字符一次  
R    替换会一直取代光标所在的文字，直到按下ESC为止

‍

‍

## 命令模式

‍

### 编辑命令

‍

|快捷键|功能描述|
| ---------------| ------------------|
|↑ 或 ctr + p|上一条命令|
|↓ 或 ctr + n|下一条命令|
|ctr + b|移动到命令行开头|
|ctr + e|移动到命令行结尾|
|ctr + ←|向左一个单词|
|ctr + →|向右一个单词|

‍

‍

### 移动光标

‍

* h 或 向左箭头键(←)        光标向左移动一个字符

  j 或 向下箭头键(↓)        光标向下移动一个字符

  k 或 向上箭头键(↑)        光标向上移动一个字符

  l 或 向右箭头键(→)        光标向右移动一个字符
* 向下移动 30 行         "30j" 或 "30↓" 的组合按键
* [Ctrl] + [f]        屏幕『向下』移动一页，相当于 [Page Down]按键 (常用)

  [Ctrl] + [b]        屏幕『向上』移动一页，相当于 [Page Up] 按键 (常用)

  [Ctrl] + [d]        屏幕『向下』移动半页

  [Ctrl] + [u]        屏幕『向上』移动半页
* ​<kbd>+</kbd>​                光标移动到非空格符的下一行  
  ​<kbd>-</kbd>​                光标移动到非空格符的上一行  
  n                表示空格光标向右移动这一行的 n 个字符
* 0 或功能键[Home]            这是数字『 0 』：移动到这一行的最前面字符处 (常用)

  $ 或功能键[End]             移动到这一行的最后面字符处(常用)
* H                光标移动到这个屏幕的最上方那一行的第一个字符  
  M                光标移动到这个屏幕的中央那一行的第一个字符  
  L                光标移动到这个屏幕的最下方那一行的第一个字符  
  G                移动到这个文档的最后一行(常用)
* nG                n 为数字. 移动到这个文件的第 n 行(可配合 :set nu)

  gg                移动到这个文档的第一行，相当于 1G

  n                n 为数字. 光标向下移动 n 行(常用)

‍

jkhl	基本上下左右  
gg	光标移动到文档首行  
G	光标移动到文档尾行

‍

^或_	光标移动到行首第一个非空字符  
home键或0或者g0	光标移动到行首第一个字符  
g_	光标移动到行尾最后一个非空字符  
end或或者 g 或者g或者g	光标移动到行尾最后一个字符  
gm	光标移动到当前行中间处

‍

b/B	光标向前移动一个单词（大写忽略/-等等特殊字符）  
w/W	光标向后移动一个单词（大写忽略/-等等特殊字符）  
e/E	移到单词结尾（大写忽略/-等等特殊字符）

‍

ctrl+b或pageUp键	翻屏操作，向上翻  
ctrl+f或pageDn键	翻屏操作，向下翻

‍

数字+G	快速将光标移动到指定行

`.	移动到上次编辑处

‍

数字+上下方向键	以当前光标为准，向上/下移动n行  
数字+左右方向键	以当前光标为准，向左/右移动n个字符

‍

H	移动到屏幕顶部  
M	移动到屏幕中间  
L	移动到屏幕尾部

‍

z+Enter键	当前行在屏幕顶部  
z+ .	当前行在屏幕中间  
z+ -	当前行在屏幕底部

shift+6	光标移动到行首  
shift+4	光标移动到行尾

​<kbd>-</kbd>​ 移动到上一行第一个非空字符

​<kbd>+</kbd>​ 移动到下一行第一个非空字符

‍

)	向前移动一个句子  
(	向后移动一个句子  
}	向前移动一个段落  
{	向前移动一个段落

‍

count l	移动到count 列  
counth	向左移动count 字符  
countl	向右移动count字符  
countgo	移动到count字符

‍

‍

### 选中内容

|v|进行字符选中|
| -------------| ------------------------------|
|V 或shift+v|进行行选中|
|gv|选中上一次选择的内容|
|o|光标移动到选中内容另一处结尾|
|O|光标移动到选中内容另一处角落|
|ctr + V|进行块选中|

‍

‍

‍

### 字符转换

‍

|~|转换大小写|
| ---| :----------: |
|u|变成小写|
|U|变成大写|

‍

‍

### 删除

‍

* x, X        x为向后删除一个字符 (相当于 [del] 按键)， X为向前删除一个字符(相当于 [backspace])

  nx        n 为数字，连续向后删除 n 个字符. 例如10x表示连续删除 10 个字符
* dd        删除光标所在的一整行(常用)

  D	删除光标位置到行尾的内容，删除之后，下一行不上移

  ndd        n 为数字. 删除光标所在的向下 n 行，例如 20dd 则是删除 20 行
* d1G        删除光标所在行到首行的所有数据

  dG        删除光标所在行到最后一行的所有数据

  d$        删除光标所在位置到该行的最后一个字符

  d0        删除光标所在位置到该行的最前面一个字符
* :a1,a2d	    删除从 a1 行到 a2 行的文本内容

‍

‍

### 撤销&复原&重复

‍

* u            撤销操作，相对于普通编辑器里面的ctrl+z
* Ctrl+r        恢复操作，相对于普通编辑器里面的ctrl+y
* .            就是小数点！可重复前一个动作
* U(大写)	    撤销所有编辑

‍

‍

### 剪切&复制&粘贴

‍

* yy        复制光标所在行  
  nyy        n 为数字. 复制光标所在的向下 n 行，例如 20yy 则是复制 20 行
* y1G        复制光标所在行到第一行的所有数据  
  yG        复制光标所在行到最后一行的所有数据  
  y0        复制光标所在的那个字符到该行行首的所有数据  
  y$        复制光标所在的那个字符到该行行尾的所有数据
* p, P        p 为将已复制的数据在光标下一行贴上，P 则为贴在光标上一行！
* dd	    剪切光标所在行  
  数字+dd	    以光标所在行为准（包含当前行），向下剪切指定行数  
  D	    剪切光标所在行

‍

‍

**合成行**

‍

J:  将光标所在行与下一行的数据结合成同一行

‍

‍

**搜索**

‍

* /word    向光标之下寻找一个名称为 word 的字符串. 

  ?word    向光标之上寻找一个字符串名称为 word 的字符串.
* n        代表重复前一个搜寻的动作，根据前面输入的/word还是?word向下或向上搜索下一个匹配的字符串. 

  N        表示反向搜索，与n的搜索方向相反.

‍

‍

**替换**

‍

:n1,n2s/word1/word2/g        在第 n1 与 n2 行之间寻找word1并替换为word2

:1,$s/word1/word2/g$表示最后一行，%s表示所有行.   
  或  
:%s/word1/word2/g

g -> gc                    gc中的c表示取代前显示提示字符给用户确认 (confirm) ！

‍

‍

## ==末行模式==

‍

‍

‍

### 文件

‍

:w                 保存编辑数据  
:w!                若文件属性为『只读』时，强制写入该文件(和权限有关)

:q                 离开  
:q!                若曾修改过文件，又不想储存，使用 ! 为强制离开不储存文件

:wq                储存后离开

:wq!                强制储存后离开

ZZ                 若文件没有更动，则不储存离开，若文件已经被更动过，则储存后离开！

:w [filename]        另存为到 filename 文件  
:r [filename]        将另一个文件『filename』的数据加到光标所在行后面

x！	保存文本，并退出 Vim 编辑器

:n1,n2 w [filename]    将 n1 到 n2 行的内容储存成 filename 这个文件. 

:! command            暂时离开 vi 到指令行模式下执行 command 的显示结果！

:set nu            会在每一行的前缀显示该行的行号  
:set nonu取        取消行号显示

查看当前已经打开的所有文件：`:files`​(%a表示激活状态，#表示上一个打开的文件)

切换到指定文件：`:open 文件名`​

切换到上一个文(back previous)：`:bp`​

切换到下一个文件(back next)：`:bn`​

‍

‍

### 查找

“/关键词”

用`N`​、`n`​可以切换上下结果；输入`nohl`​，可以取消高亮

|快捷键|功能描述|
| --------| ------------------------------|
|/abc|从光标所在位置**向前查找**字符串 abc|
|/^abc|查找以 abc 为行首的行|
|/abc$|查找以 abc 为行尾的行|
|?abc|从光标所在位置**向后查找**字符串 abc|
|n或；|向同一方向重复上次的查找指令|
|N或,|向相反方向重复上次的查找指定|

‍

### 替换

r	替换光标所在位置的字符  
R	从光标所在位置开始替换字符，其输入内容会覆盖掉后面等长的文本内容，按“Esc”可以结束

:s/a1/a2	替换当前光标所在行第一处符合条件的内容  
:s/a1/a2/g	替换当前光标所在行所有的 a1 都用 a2 替换  
:%s/a1/a2	替换所有行中，第一处符合条件的内容  
:%s/a1/a2/g	替换所有行中，所有符合条件的内容

:n1,n2 s/a1/a2	将文件中 n1 到 n2 行中第一处 a1 都用 a2 替换  
:n1,n2 s/a1/a2/g	将文件中 n1 到 n2 行中所有 a1 都用 a2 替换

‍

‍

## 可视模式

‍

可视模式下，选中的区域是由两个端点来界定的（一个在左上角，一个在右下角），在默认情况下只可以控制右下角的端点，而使用 o 按键则可以在左上角和右下角之间切换控制端点

‍

v 进入字符可视化模式： 文本选择是以字符为单位的. 

V 进入行可视化模式： 文本选择是以行为单位的. 

Ctrl+v 进入块可视化模式 ： 选择一个矩形内的文本. 

‍

* A        在选定的部分后面插入内容  
  I        在选定的部分前面插入内容  
  d        删除选定的部分  
  c        删除选定的部分并进入插入模式（有批量替换效果）  
  r        把选定的部分全部替换为指定的单个字符

* ​<kbd>&gt;&gt;</kbd>​        向右缩进一个单位，更适合行可视化模式

* ​<kbd>&lt;&lt;</kbd>​        向左缩进一个单位，更适合行可视化模式

* gu        选中区域转为小写  
  gU        选中区域转为大写  
  g~        大小写互调

‍

## 拓展操作

‍

### 代码颜色显示

：syntax on/off

‍

### 内置计算器

a.进入编辑模式  
b.按“ctrl+r，光标变成引号，，输入=，光标转到最后一行  
c.输入需要计算的内容，按下enter后，计算结果回替代上一步中的引号，光标恢复

‍

### 配置

a.文件打开时，末行模式下输入的配置为临时配置，关闭文件后配置无效  
b.修改个人配置文件，可以永久保存个人配置（~/.vimrc，如果没有可以自行创建）  
c.修改全局配置文件，对每个用户生效（vim自带，/etc/vimrc）

注：个人配置文件优先级更高，当个人配置和全局配置发生冲突时，系统以当前用户的个人配置文件为准

‍

### 异常退出

在编辑文件后，未正常保存退出时，会产生异常退出交换文件（.原文件名.swp）  
将交换文件删除后，再次打开文件时，无提示：“#rm -f .原文件名.swp”

‍

### 别名机制

**自定义指令**

Linux中，存在一个别名映射文件： ~/.bashrc  
修改文件内容，可以自定义指令，重新登录账号后生效

‍

### 文件快捷方式

对于深层文件，可以创建文件快捷方式，便于后续操作：#ln -s 源路径 新路径

‍

### 退出方式

（1）在vim中退出文件编辑模式，可以使用:q或者:wq

（2）建议使用:x：使用效果等同于wq，如果文件有改动则先保存后退出；但是如果文件没有做修改，会直接退出，不会修改文件更新时间，避免用户混淆文件的修改时间

‍
