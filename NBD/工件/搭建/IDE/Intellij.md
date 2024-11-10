--垄断者

‍

### Header

‍

固定版本选择[2023.2.5](https://www.jetbrains.com/zh-cn/idea/download/other.html)

未来更新硬件后启用新版本嵌入AI助手

‍

‍

#### 认证方法

使用这个[链接](https://www.jetbrains.com/shop/eform/students/github/prolong/000C3B95C9CF87B0EC5F6E1B98B9ADDACCCF162F8BBB02A85E4FEB7AB29DEC3BFDF59F7A43CC912E24B4038275BB5641A29567AEDC0192515C8A4AEC62B0724DA200025B2638E8D5C89CAD7C9B5DD772BC320D70A26A32DBBAA0256F18DBDE13ECA3CF4F181BDEAFC14D513773640D00F90CF2CDB36C9289AAB642C08BCDABFF5924?code=c3b4aba50fcf0a30e53f&state=Q8bDVDpjHxEw4uWuwj96CzHEXXPp12JQKV3QLHWiAOV0t9b9N-9eENAKm6_t9skz), 保证Github个人账户里面没有被ban的大陆学校邮箱

然后链接到我的谷歌邮箱, 回到谷歌邮箱进行操作

‍

‍

#### 白嫖工具

‍

白嫖IDEA付费插件的方法: 从硬盘安装->下载包

‍

[项目](https://codeberg.org/alexdh/JetbrainKiller/src/branch/main)&[链接](https://codeberg.org/alexdh/JetbrainKiller/src/branch/main/README_ZH-CN.md)

[介绍](https://zhuanlan.zhihu.com/p/657104146)

自动把要到期的延长

Jetbrains系列软件的无限 30 天试用

‍

‍

# 操作

‍

## 工程结构

‍

### 安装目录

* vmoptions配置信息，包含启动参数，使得idea更快更好运行

  * 最大内存
  * 运行内存
* Users/config用户的配置目录

  * 快捷键
* Users/system

  * cache缓存

‍

### 工程目录

* src 存放代码的文件
* .idea目录 Idea的配置文件
* project.iml 工程结构的配置文件

> 删掉config和system就相当于删除用户数据。还原默认设置。

‍

### 工程组织

> file-project structure。command+;

* Project 整个工程，可以直接添加代码，也可以创建模块后，添加代码。能够配置sdk的版本。
* Module 大型项目可以分为很多模块，相互间彼此依赖。先删除模块，再删除模块中的文件。与Facets对应
* Libraries 项目以来的jar包或者第三方库。
* Facets 英文翻译为：方面，（事务的）面。表述了在Module中使用的各种各样的框架、技术和语言。这些Facets让IDEA知道怎么对待module内容，并保证与相应的框架和语言保持一致。
* Artifacts 英文翻译为：人工产品。是一个项目资源的组合体。例如，一个已编译的java类的集合，一个已打包的java应用。这里可以理解为Maven中的artifactId，成果产物ID，他可以是一个jar或是一个war。

‍

‍

## 功能特性

‍

‍

### 零碎

‍

* 在下载大批量的代码包时, 提前创建一个项目包含这些小项目显得非常舒服
* 创建文件时可以路径.文件名,能够一并把前面的文件夹也给生成了
* ""字符串如果是代码段可以右键查看行为-注入语言,可以完成对应语言的检查等功能, 例如SQL语句
* 控制台输出看的不爽, 有太多无用系统内部信息, 则可以使用右键"像这样折叠内部行"操作来折叠.
* 使用`<p>`​标签来在T'O'DO里面施法(Javadoc)

‍

‍

### 文件操作

‍

#### 删除

Java/IDE中的删除不走回收站

要删除时注意目录里面没有文件或是文件目录

‍

‍

#### 新建

.src中创建java文件时附带的软件包(目录)可以通过.的方式构建

但是在resource目录下创建需要通过/分隔目录和文件本身

‍

‍

### 引用

接口全限定名的获取:  对应目录或方法接口右键复制引用即可

‍

‍

### 可视化依赖

右键图表

‍

### 帮助选项 - 启动查询等

可以检测什么插件拖慢了性能

也可以设置默认的IDEA大小, 帮助提升性能

‍

### 代码模板

IDEA 提供了一些内置的代码模板

‍

例如

​`list.sout`​ =  `System.out.println(list);`​

​`list.var`​ =  `List<User> list1 = list`​

​`list.nn = list.if (list != null)`​

‍

‍

#### 输出模板

* sout   最基本的输出语句，快速生成  System.out.println();
* soutp    参数输出语句.
* soutm    方法名输出语句.
* soutv    最近一个变量的输出语句.
* xxx.sout    指定字符串进行输出，可以使用拼接变量的形式：

‍

‍

#### 循环模板

‍

* fori    最基本的 for 循环
* iter    增强for循环
* list.for    遍历指定List集合的增强for循环

  * list.fori    普通的for 循环

    list.forr    倒序（i 递减） for 循环

‍

#### 条件模板

‍

* ifn  /  xxx.null  快速生成空值判断
* inn  /  xxx.nn

‍

#### 常量模板

‍

* psf    public static final
* prsf    private static final

‍

‍

#### 修改与自定义模板

修改内建的默认模板

> 暂时不推荐, 因为这样就会造成强绑定, 还是自己打比较好

‍

‍

### 快速方法测试

‍

方法上点右键选择Goto -> Test, 勾选方法

Test区域对应的地方生成测试方法

‍

‍

### 可视化图结构

IDEA可视化可以看到表之间关系

‍

‍

## 调试控制台 - 断点技巧

‍

​​

‍

主要使用的执行按钮分别是:步过(不进入函数) , 步入(能进入包括源代码的函数), 单步(只进入我的函数), 后面有一个跳出

无阻塞断点 shift+单击断点

​![Debug按键说明](assets/net-img-Debug按键说明-20240930223120-rw801a3.png)​

‍

‍

### **条件断点**

循环中经常用到这个技巧，比如：遍历1个大List的过程中，想让断点停在某个特定值. 

在断点的位置，右击断点旁边的小红点，会出来一个界面，在Condition这里填入断点条件即可，这样调试时，就会自动停在i=10的位置

‍

### **回到&quot;上一步&quot;**

该技巧最适合特别复杂的方法套方法的场景，好不容易跑起来，一不小心手一抖，断点过去了，想回过头看看刚才的变量值，如果不知道该技巧，只能再跑一遍. 

回到了method1刚开始调用的时候，变量i变成了99，没毛病吧，老铁们，是不是很6 :)

参考上图，method1方法调用method2，当前断点的位置j=100，点击上图红色箭头位置的Drop Frame图标后，时间穿越了

注：好奇心是人类进步的阶梯，如果想知道为啥这个功能叫Drop Frame，而不是类似Back To Previous 之类的，可以去翻翻JVM的书，JVM内部以栈帧为单位保存线程的运行状态，drop frame即扔掉当前运行的栈帧，这样当前“指针”的位置，就自然到了上一帧的位置. 

‍

### **多线程调试**

多线程同时运行时，谁先执行，谁后执行，完全是看CPU心情的，无法控制先后，运行时可能没什么问题，但是调试时就比较麻烦了，最明显的就是断点乱跳，一会儿停这个线程，一会儿停在另一个线程，比如下图：

如果想让线程在调试时，想按自己的愿意来，让它停在哪个线程就停在哪个线程，可以在图中3个断点的小红点上右击，

即：Suspend挂起的条件是按每个线程来，而非All. 把这3个断点都这么设置后，再来一发试试

‍

‍

### **远程调试**

前提是本机有项目的源代码 ，在需要的地方打个断点，然后访问一个远程的url试试，断点就会停下来.

‍

选择运行选项

配置“Remote” 属性：

* Name：配置Remote Debug的名称，可以是任意名称；
* Host：配置服务器的域名或ip地址，Port 使用默认值5005，也可以是其他端口；
* Command line arguments for remote JVM：配置Debug远程服务的命令行启动参数，本地在Debug时会监听远程服务的对应端口并运行调试环境，具体的参数说明如下：

  * -Xdebug：JVM在DEBUG模式下工作；
  * -Xrunjdwp：JVM使用(java debug wire protocol)来运行调试环境；
  * transport：监听Socket端口连接方式,常用的dt_socket表示使用socket连接；
  * server：=y表示当前是调试服务端，=n表示当前是调试客户端；
  * suspend：=n表示启动时不中断；
  * address：表示本地监听的地址和端口。

‍

‍

#### 使用

注意复制图中的 JVM 代码，这将在之后在服务端用于以调试模式启动本程序

生成的代码, 端口随意选没占用的

‍

得到 JAR 包之后，可以使用 Xftp 将其上传到服务器端的 Linux 中

Linux 上有防火墙，默认会阻止所有的远程访问: 在防火墙中永久开放上述约定的端口号 5005

假设上面上传的 JAR 包名为 `demo.jar`​。整合前面复制的 JVM 代码，输入以下命令来运行此 `demo`​ 程序

‍

如果想知道远程服务端的端口号有没有生成，可以在**远程服务端**输入以下命令：（如果运行的程序阻塞了当前的 Shell 窗口，可以在 Xshell 中另开一个 Shell 窗口。）

netstat -na | grep 5005

‍

#### 注意

‍

远程调试指的是使用本地客户端来调试运行在远程服务器上的程序。IntelliJ IDEA 远程调试的原理是，当服务器端以调试模式运行 Java 程序时，如果客户端使用文本相同的字节码和事先约定好的端口号，就可以远程调试该 Java 程序。因此，IntelliJ IDEA 远程调试的必要条件是：

Java 程序必须在服务端已经启动且在调试时正在运行。IntelliJ IDEA 只有触发调试的能力，没有远程部署启动 Java 程序的能力。

Java 程序在服务端以调试模式启动。以调试模式启动需要在运行该 Java 程序时，使用一些调试模式的 JVM 参数。

客户端使用 IntelliJ IDEA 进行调试时，使用与服务端事先约定好的相同端口号，且该端口号在服务端没有被占用。

客户端使用 IntelliJ IDEA 进行调试时，使用的代码在文本上与服务端一致。在文本上一致指的是，客户端使用的代码与服务端使用的代码的文字完全相同，如果不一致，使用的断点将不起作用。文本上一致不包括注释，但包括换行。文本上一致不需要是同一个代码源文件，只需要文本相同即可。

‍

注意端口别被占用。后续这个端口是用来跟远程的java进程通信的。

可以注意到：切换不同的jdk版本，生成的脚本不一样

```java
选择 jdk1.4，则为

-Xdebug -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=50055

这就是你为什么搜其他博客，会有这种配置的原因，其实这个配置也是可行的。但更准确应该按照下面jdk5-8的配置

选择 jdk 5-8，则为

-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=50055

选择 jdk9以上，则为

-agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:50055
```

据说因为jdk9变得安全了，远程调试只允许本地，如果要远程，则需要在端口前配置*

‍

‍

远程调试在自己的断点处停下来了，此时关闭IDEA中的项目停止运行，则还会继续运行

**要保证和远程启动的代码一致. 否则你debug的时候的行数会对不上。报错抛异常倒是不会**

‍

日志不会打印在IDEA的控制台上。即System.out 以及 log.info 还是打印在远程的

‍

远程调试的时候，打了断点，停住后会导致页面的请求卡住。

‍

###### 本地代码修复bug远程调用的时候

如果在远程调试过程自己发现了bug，本地改好后重新启动IDEA里的项目，再到页面调用一次，能修复吗？ **不能，运行的还是远程部署的jar中的代码**

‍

‍

‍

‍

# 外围

‍

‍

## 插件列表

插件无法被导入设置, 需要重新配, 记得保留插件列表

‍

‍

### 直属

‍

#### Github Copilot

‍

#### Chinese Language

‍

#### VScode Theme

‍

‍

#### Extra ToolWindow Colorful icons

界面好看多了

‍

‍

#### Rainbow Brackets

彩虹括弧

控制生效区域与高亮

​<kbd>Ctrl + 右键</kbd>​ = 高亮当前区域  
​<kbd>Alt + 右键</kbd>​ 限制范围高亮

‍

#### Translation

翻译文本

‍

#### Copilot + Chat

AI补全助手 + ChatGPT内置分析

[平替可参考](https://codeium.com/pricing)

‍

#### Redis Helper

直接内嵌入Redis连接库

‍

#### MybatisX

快速定位XML和Mapper位置

‍

#### Grep Console

基础对象高亮

查看错误信息时选择高亮部分代码以及筛选

‍

‍

#### **Key promoter X**

Key promoter X是IDEA的快捷键提示插件

‍

‍

#### **Maven Helper**

​`Maven Helper`​ 是解决`Maven`​依赖冲突的利器，可以快速查找项目中的依赖冲突. 安装后打开`pom`​文件，底部有 `Dependency Analyzer`​视图. 显示红色表示存在依赖冲突，点进去直接在包上右键`Exclude`​排除，`pom`​文件中会做出相应排除包的操作. 

* Con'flicts(冲突)
* All Dependencies as List(列表形式查看所有依赖)
* All Dependencies as Tree(树结构查看所有依赖)，并且这个页面还支持搜索.

‍

‍

#### RestfulToolkit-fix

请求Postman类似玩意    

‍

‍

### 备用

‍

**easy_javadoc**

(暂时停止, 鸡肋)

快速为`Java`​的类、方法、属性加注释, 右Ctrl(Meta键)+\

‍

**Java Stream Debugger**

(捆绑)

​​Java8​的​stream API​很大​程度的简化了我们的代码量，可在使用过程中总会出现奇奇怪怪的​bug​而且不能​debug​. ​

​​Java Stream Debugger​支持了对​stream API​的调试，可以清晰的看到每一步操作数据的变化过程. 

‍

**String Manipulation**

​`String Manipulation`​一个比较实用的字符串转换工具，比如我们平时的变量命名可以一键转换驼峰等格式，还支持对字符串的各种加、解密（`MD5`​、`Base64`​等）操作. 

‍

‍

‍

### 建议

‍

#### AI类

**Codota AI (AI版本)**

用了`Codota`​ 后不再怕对`API`​不会用，举个栗子：当我们用`stream().filter()`​对`List`​操作，可是对`filter()`​用法不熟，按常理我们会百度一下，而用`Codota`​ 会提示很多`filter()`​用法，节省不少查阅资料的时间. 

‍

**ai智能编码提示：aiXcode**

(国内老牌)

‍

#### 检查类

[Link](https://cloud.tencent.com/developer/article/2196226)

* **Alibaba Java Coding Guidelines 阿里巴巴代码规范检查插件**
* SonarLint 代码质量检查插件
* **CheckStyle 代码风格检查插件**

  功能跟Alibaba Java Coding Guidelines类似
* **较便利插件**  
  **RoboPOJOGenerator—JSON （GsonFormat也可以，但是好久没更新过了）**

‍

#### 生成器类

‍

代码生成器 codehelper.generator   或者 GenerateAllSetter

‍

‍

‍

## 快捷键列表

‍

‍

### 直属

* Ctrl+ P 查看当前函数的参数 !
* Ctrl+Shift+J 智能拼接选区代码为一行 !
* 代码模板(主动快速补全) :  Ctrl+ J

  方法的重构，也即代码快速封装的技巧 Ctrl+Alt+M  一键把代码提取到方法里去
* **切换标签页: Alt+左右 和 资源管理器一样**
* **选择代码块: 旁边空白双击**
* 折叠/反折叠: ctrl+-/+
* 折叠全部/全部不折叠: s+c+-/+
* 选中代码: 环绕方式(try, for循等) :  C+Alt+ T
* 快速的多行修改: 按住Alt上下拖动即可插入多行光标
* 断点调试中的放行方法非常好用,多打几个断点,之后进行放行即可
* 回到刚才编辑的地方(历史记录,可多次) C+S+Backspace
* ​`Ctrl + W`​​​​​​：扩展选择

  ​`Ctrl + Shift + W`​​​​​​：收缩选择
* IDEA查看层次结构, 在对应类上使用<kbd>Ctrl+H</kbd>​​​​​
* 设置 <kbd>Ctrl + Alt + S</kbd>​​​​
* 最近修改的代码    ctrl + E
* 将光标跳到当前行的上一行    ctrl + alt + enter
* 自动代码片    ctrl + j
* 选中当前单词    ctrl + w
* 各种东西    Ctrl + ~
* 选中内容 按 ctrl + shift + alt + j 统一修改相同的内容
* 自制重构 <kbd>Shift + F6</kbd>​​ + <kbd>A+S+C + ~ </kbd>​​  三个全按下 + <kbd>~</kbd>​​
* ​<kbd>C + A + W</kbd>​ 关闭当前标签页

‍

‍

### 备用

‍

‍

### 建议

[快捷键大全](https://zhuanlan.zhihu.com/p/614290093)

[快捷键以及演示](https://zhuanlan.zhihu.com/p/615560482)

‍

**快捷键**

> 1.智能补全：Ctrl+Shift+Space
>
> 2.自我修复：Alt+Enter
>
> 3.重构一切：Ctrl+Shift+Alt+T
>
> 5.选你所想【选中上下文相关联代码】：Ctrl+W
>
> 6.代码生成：Template/Postfix +Tab
>
> 7.发号施令：Ctrl+Shift+A
>
> 9.自动完成：Ctrl+Shift+Enter
>
> 10.创造万物：Alt+Insert

‍

‍

‍

## 建议

> 推荐序列

‍

‍

# 实操

‍

## Bugfix

‍

### 简单问题 合集

‍

* 当属性文件打不开的时候, 可能需要修改文件编码- 属性文件编码为UTF-8(默认是个很奇怪的值, 建议一入手就把它改了)
* Github一直推不上去 -->配置GIT代理解决!
* 嵌入提示: 还是不要开为好, 太乱了
* 如果在IDEA中找不到插件，可以手动访问插件市场

  https://plugins.jetbrains.com/plugin
* 删除过世的JDK C:\Users\SK\AppData\Roaming\JetBrains\IntelliJIdea2023.2\options 找到 jdk.table

  备份一份 再直接删掉， 重启idea 右下角会自动弹出一个配置sdks 的，在那里点击+号添加就行，他会自动搜索的
* 自动下载的JDK位置在IDEA安装目录下面的jbr文件夹 D:\APP\Produce\IDE\IDEA\IntelliJ IDEA 2023.2.7\jbr
* 服务/运行配置中, 本地测试需要配置SpringBoot的启动环境配置项目为local
* java: 非法字符: '\\ufeff'报错

  来源于这个IDEA里面设置的BOM，一般在编写时候会给你默认添加这样的一个BOM头，是隐藏起来的，编译时候会给出现编码混乱问题  
  解决办法：在输出栏这里点击UTF-8然后点击移除BOM，然后运行就好了. 这个项目的暂时情况就得到了解决. 想要不会每次新建项目时候都会出现这种问题，那就在idea设置-文件编码这里选择不含BOM

‍

‍
