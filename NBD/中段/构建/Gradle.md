--新一代项目构建工具

‍

Gradle 是一款Google 推出的基于 JVM、通用灵活的项目构建工具，支持 Maven，JCenter 多种第三方仓库;支持传递性依赖管理、废弃了繁杂的xml 文件，转而使用简洁的、支持多种语言(例如：java、groovy 等)的 build 脚本文件. 

‍

### Header

> 主要还是Maven, 这个先放着不整理了

‍

#### 环境

使用Spring脚手架 https://start.spring.io/; 或是IDEA

可以修改下载源来提升速度(我可以开全局代理)

‍

‍

# 知识

‍

## 概念

‍

### 评价

Spring、SpringBoot等Spring家族的源码基本上基于Gradle构建的.   
总之，虽然目前市面上常见的项目构建工具有Ant、Maven、Gradle，主流还是Maven，但是未来趋势是Gradle. 

‍

‍

# 基础

‍

## 搭建

‍

‍

‍

### 空项目JavaWeb

使用基础的WEB-INF, 与Servlet等配置基本相同

‍

‍

### 整合SB

从一个基础项目变为SB整合(其实IDEA直接选择SB即可自动搭建完, 这里也记录一下)

SB手架方法略

‍

#### **引入springboot 插件**

```groovy
plugins {
id 'org.springframework.boot' version '2.3.7.RELEASE' //维护springboot版本号,不单独使用,和下面两个插件一起用
id 'io.spring.dependency-management' version '1.0.10.RELEASE'
//进行依赖管理,在引入其它boot依赖时省略版本号、解决jar包冲突问题id 'java'
}
```

‍

#### **引入所需要的依赖**

```groovy
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter'
    implementation 'org.springframework.boot:spring-boot-starter-web' 
    //省略版本,原生bom支持,插件management提供
    ......
}
```

‍

#### **执行gradle bootRun 指令**

要想运行当前Springboot 项目，直接执行gradle bootRun 指令或者idea 右侧按钮即可. 当然如果想让当前项目打成可执行jar 包，只需执行： gradle bootJar 指令即可. 

Cloud 项目创建也可以借助于脚手架创建，与Boot 项目类似. 

‍

‍

### 拓展SB

帮助指定版本号

```groovy
buildscript {

    repositories {
        maven { url 'https://maven.aliyun.com/repository/public' }
    }

    dependencies {
        classpath 'org.springframework.boot:spring-boot-gradle-plugin:2.4.1'
    }
}

apply plugin: 'org.springframework.boot'
apply plugin: 'io.spring.dependency-management'
```

‍

‍

## **生命周期**

‍

### 阶段

生命周期分为三大阶段: Initialization -> Configuration -> Execution.

初始化 - 构建 - 执行

‍

Initialization 阶段主要目的是初始化构建, 它又分为两个子过程,一个是执行 Init Script,另一个是执行 Setting Script. 

‍

● init.gradle 文件会在每个项目 build 之前被调用，用于做一些初始化的操作，它主要有如下作用：  
  ○ 配置内部的仓库信息（如公司的 maven  仓库信息）；  
  ○ 配置一些全局属性；  
  ○ 配置用户名及密码信息（如公司仓库的用户名和密码信息）.   

● Setting Script 则更重要, 它初始化了一次构建所参与的所有模块.   

‍

Configuration 阶段：加载项目中所有模块的 Build Script. 所谓 "加载" 就是执行 build.gradle 中的语句, 根据脚本代码创建对应的 task, 最终根据所有 task (的依赖关系, 类似Maven那里的描述)生成由 Task 组成的有向无环图(Directed Acyclic Graphs)

‍

Execution 阶段: 根据上个阶段构建好的有向无环图，按着顺序执行 Task【Action 动作】

‍

### **钩子函数**

‍

‍

#### 初始化阶段

* 在 settings.gradle 执行完后,会回调 Gradle 对象的 settingsEvaluated 方法
* 在构建所有工程 build.gradle 对应的Project 对象后,也既初始化阶段完毕,会回调 Gradle 对象的projectsLoaded 方法

‍

#### 配置阶段

Gradle 会循环执行每个工程的 build.gradle 脚本文件

* 在执行当前工程build.gradle 前,会回调Gradle 对象的 beforeProject 方法和当前Project 对象的 beforeEvaluate 方法, 虽然 beforeEvalute 属于 project 的生命周期, 但是此时 build script 尚未被加载, 所以 beforeEvaluate 的设置依 然要在 init script 或 setting script 中进行,不要在 build script 中使用 project.beforeEvaluate 方法.
* 在执行当前工程 build.gradle 后,会回调 Gradle 对象的afterProject 方法和当前Project 对象的 afterEvaluate 方法
* 在所有工程的 build.gradle 执行完毕后，会回调 Gradle 对象的 projectsEvaluated 方法
* 在构建 Task 依赖有向无环图后,也就是配置阶段完毕,会回调TaskExecutionGraph 对象的 whenReady 方法

‍

#### 执行阶段

Gradle 会循环执行Task 及其依赖的 Task

* 在当前 Task 执行之前,会回调 TaskExecutionGraph 对象的 beforeTask 方法
* 在当前 Task 执行之后,会回调 TaskExecutionGraph 对象的 afterTask 方法当所有的 Task 执行完毕后，会回调 Gradle 对象的 buildFinish 方法.

‍

提示：Gradle 执行脚本文件的时候会生成对应的实例，主要有如下几种对象：  

1. Gradle 对象：在项目初始化时构建，全局单例存在，只有这一个对象
2. Project 对象：每一个build.gradle文件 都会转换成一个 Project 对象,类似于maven中的pom.xml文件
3. Settings 对象：settings.gradle 会转变成一个 settings  对象,和整个项目是一对一的关系,一般只用到include方法
4. Task对象: 从前面的有向无环图中，我们也可以看出，gradle最终是基于Task的,一个项目可以有一个或者多个Task

‍

‍

## 配置文件

‍

### settings

‍

作用：主要是在项目初始化阶段确定一下引入哪些工程需要加入到项目构建中,为构建项目工程树做准备. 

工程树：gradle 中有工程树的概念，类似于 maven 中的project 与module. 

内容：里面主要定义了当前 gradle 项目及子 project 的项目名称  

位置：必须放在根工程目录下.   

名字：为settings.gradle 文件，不能发生变化  

对应实例：与 org.gradle.api.initialization.Settings 实例是一一对应的关系. 每个项目只有一个settings 文件.   

关注：作为开发者我们只需要关注该文件中的include 方法即可. 使用相对路径【 :  】引入子工程.   

一个子工程只有在setting 文件中配置了才会被 gradle 识别,这样在构建的时候才会被包含进去

‍

**项目名称中 &quot;:&quot; 代表项目的分隔符, 类似路径中的 &quot;/&quot;. 如果以 &quot;:&quot; 开头则表示相对于 root project** . 然后 Gradle 会为每个带有 build.gradle 脚本文件的工程构建一个与之对应的 Project 对象. 

‍

### build.gradle

‍

build.gradle 是一个gradle 的构建脚本文件,支持java、groovy 等语言. 

每个project 都会有一个build.gradle 文件,该文件是项目构建的入口,可配置版本、插件、依赖库等信息. 

每个build 文件都有一个对应的 Project 实例,对build.gradle 文件配置，本质就是设置Project 实例的属性和方法. 

由于每个 project 都会有一个build 文件,那么Root Project 也不列外.Root Project 可以获取到所有 Child Project,所以在Root Project 的 build 文件中我们可以对Child Project 统一配置,比如应用的插件、依赖的maven 中心仓库等. 

‍

‍

#### 常用属性方法

‍

‍

余下内容太细, 建议即用即查

‍

#### **Subprojects 与 Allprojects**

allprojects 是对所有project(包括Root Project+ child Project[当前工程和所有子工程])的进行统一配置，而subprojects  
是对所有Child Project 的进行统一配置

‍

#### ext 用户自定义属性

Project 和 Task 都允许用户添加额外的自定义属性，要添加额外的属性，通过应用所属对象的ext 属性即可实现. 添加之后可以通过 ext 属性对自定义属性读取和设置，如果要同时添加多个自定义属性,可以通过 ext 代码块

‍

#### **Buildscript**

buildscript 里是gradle 脚本执行所需依赖，分别是对应的 maven 库和插件

‍

‍

### 仓库地址

‍

mavenLocal(): 指定使用maven本地仓库，而本地仓库在配置maven时settings文件指定的仓库位置. 

maven { url 地址}: 指定maven仓库，一般用私有仓库地址或其它的第三方库【比如阿里镜像仓库地址】. 

mavenCentral()：这是Maven的中央仓库，无需配置，直接声明就可以使用. 

jcenter():JCenter中央仓库，实际也是是用的maven搭建的，但相比Maven仓库更友好，通过CDN分发，并且支持https访问 ->在新版本中已经废弃了，替换为了mavenCentral(). 

‍

总之, gradle可以通过指定仓库地址为本地maven仓库地址和远程仓库地址相结合的方式，避免每次都会去远程仓库下载依赖库. 这种方式也有一定的问题，如果本地maven仓库有这个依赖，就会从直接加载本地依赖，如果本地仓库没有该依赖，那么还是会从远程下载. 但是下载的jar不是存储在本地maven仓库中，而是放在自己的缓存目录中，默认在USER\_HOME/.gradle/caches目录,当然如果我们配置过GRADLE\_USER\_HOME环境变量，则会放在GRADLE\_USER\_HOME/caches目录,那么可不可以将gradle caches指向maven repository. 我们说这是不行的，caches下载文件不是按照maven仓库中存放的方式. 

‍

## 任务

Task

‍

### 定义

总体分为两大类:一种是通过 Project 中的task()方法,另一种是通过tasks 对象的 create 或者register 方法. 

‍

```groovy
task('A',{//任务名称,闭包都作为参数println "taskA..."
})

task('B'){//闭包作为最后一个参数可以直接从括号中拿出来println "taskB..."
}

task C{//groovy语法支持省略方法括号:上面三种本质是一种println "taskC..."
}

def map=new HashMap<String,Object>(); map.put("action",{println "taskD.."}) //action属性可以设置为闭包task(map,"D");

tasks.create('E'){//使用tasks的create方法println "taskE.."
}

tasks.register('f'){ //注：register执行的是延迟创建。也即只有当task被需要使用的时候才会被创建。
println "taskF	"
}
```

定义任务的时候可以直接指定任务属性，也可以给已有的任务动态分配属性：

```java
//①.F是任务名，前面通过具名参数给map的属性赋值,以参数方式指定任务的属性信息
task(group: "atguigu",description: "this is task B","F")
//②.H是任务名，定义任务的同时，在内部直接指定属性信息
task("H") {
group("atguigu") description("this is the task H")
}
//③.Y是任务名，给已有的任务 在外部直接指定属性信息
task "y"{}
y.group\="atguigu"
clean.group("atguigu") //案例：给已有的clean任务重新指定组信息
可以在 idea 中看到: 上面自定义的那几个任务和 gradle 自带的 clean 任务已经跑到：atguigu 组了
```

‍

‍

项目实质上是 Task 对象的集合. 一个 Task 表示一个逻辑上较为独立的执行过程，比如编译Java 源代码，拷贝文件， 打包Jar 文件，甚至可以是执行一个系统命令. 另外，一个 Task 可以读取和设置Project 的Property 以完成特定的操作. 

‍

**提示 1 :** task 的配置段**是在**配置阶段**完成**

**提示 2** :**task 的doFirst、doLast 方法是**执行阶段**完成，** 并且doFirst 在doLast 执行之前执行 **. **

**提示 3:** 区分任务的配置段和任务的行为,**任务的配置段在**配置阶段执行,**任务的行为在**执行阶段执行

‍

底层原理

无论是定义任务自身的 action,还是添加的doLast、doFirst 方法，其实底层都被放入到一个Action 的List 中了

最初这个 action List 是空的，当我们设置了 action【任务自身的行为】,它先将action 添加到列表中，此时列表中只有一个action,后续执行doFirst 的时候doFirst 在action 前面添加，执行 doLast 的时候doLast 在action 后面添加. 

doFirst 永远添加在actions List 的第一位，保证添加的Action 在现有的 action List 元素的最前面；doLast 永远都是在action List 末尾添加，保证其添加的Action 在现有的action List 元素的最后面. 

一个往前面添加,一个往后面添加，最后这个action List 就按顺序形成了doFirst、doSelf、doLast 三部分的 Actions,就达到 doFirst、doSelf、doLast 三部分的 Actions 顺序执行的目的. 

‍

### **依赖**

‍

方式一：参数方式依赖

```groovy
task A {
    doLast {
        println "TaskA.."
    }
}
task 'B' {
    doLast {
        println "TaskB.."
    }
}
//参数方式依赖: dependsOn后面用冒号
task 'C'(dependsOn: ['A', 'B']) {
    doLast {
        println "TaskC.."
    }
}}
```

‍

方式二:内部依赖

```groovy
//参数方式依赖
task 'C' {
    //内部依赖：dependsOn后面用 = 号
    dependsOn= [A,B] 
    doLast {
        println "TaskC.."
    }
}
```

‍

**方式三：外部依赖**

 **//** 外部依赖:可变参数,引号可加可不加

​`<span class="ne-text">C.dependsOn(B,'A')</span>`​

当然：task 也支持跨项目依赖

拓展 1：当一个 Task 依赖多个Task 的时候，被依赖的Task 之间如果没有依赖关系，那么它们的执行顺序是随机的,并无影响. 

拓展 2 **：重复依赖的任务只会执行一次**

‍

‍

### 执行

‍

任务执行语法：gradle [taskName...] [--option-name...]

‍

|**分类**|**解释**|
| --| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|**常见的任务（*）**|gradle build: 构建项目:编译、测试、打包等操作<br />gradle run :运行一个服务,需要application 插件支持，并且指定了主启动类才能运行<br />gradle clean: 请求当前项目的 build 目录<br />gradle init : 初始化 gradle 项目使用<br />gradle wrapper: 生成wrapper 文件夹的<br />|
|**项目报告相关任务**|gradle projects : 列出所选项目及子项目列表，以层次结构的形式显示gradle tasks: 列出所选项目【当前 project,不包含父、子】的已分配给任务组的那些任务. <br />gradle tasks --all :列出所选项目的所有任务. <br />gradle tasks --group\="build setup":列出所选项目中指定分组中的任务. <br />gradle help --task someTask :显示某个任务的详细信息<br />gradle dependencies :查看整个项目的依赖信息，以依赖树的方式显示<br />gradle properties 列出所选项目的属性列表|
|**调试相关选项**|-h,--help: 查看帮助信息<br />-v, --version:打印 Gradle、 Groovy、 Ant、 JVM 和操作系统版本信息. <br />-S, --full-stacktrace:打印出所有异常的完整(非常详细)堆栈跟踪信息. <br />-s,--stacktrace: 打印出用户异常的堆栈跟踪(例如编译错误). <br />-Dorg.gradle.daemon.debug\=true: 调试 Gradle  守护进程. <br />-Dorg.gradle.debug\=true:调试 Gradle 客户端(非 daemon)进程. <br />-Dorg.gradle.debug.port\=(port number):指定启用调试时要侦听的端口号. 默认值为 5005. |
|**性能选项:**|--build-cache, --no-build-cache： 尝试重用先前版本的输出. 默认关闭(off). <br />--max-workers: 设置 Gradle 可以使用的woker 数. 默认值是处理器数. <br />-parallel, --no-parallel: 并行执行项目. 有关此选项的限制，请参阅并行项目执行. 默认设置为关闭(off)<br /> **【备注: 在gradle.properties 中指定这些选项中的许多选项，因此不需要命令行标志】** <br />|
|**守护进程选项**|--daemon, --no-daemon:  使用 Gradle 守护进程运行构建. 默认是on<br />--foreground:在前台进程中启动 Gradle  守护进程. <br />-Dorg.gradle.daemon.idletimeout\=(number of milliseconds):<br />Gradle Daemon 将在这个空闲时间的毫秒数之后停止自己. 默认值为 10800000(3 小时). |
|**日志选项**|-Dorg.gradle.logging.level\=(quiet,warn,lifecycle,info,debug):<br />通过 Gradle 属性设置日志记录级别. <br />-q, --quiet: 只能记录错误信息<br />-w, --warn: 设置日志级别为 warn<br />-i, --info: 将日志级别设置为 info<br />-d, --debug:登录调试模式(包括正常的堆栈跟踪)|
|**其它(*)**|-x:-x 等价于: --exclude-task : 常见gradle -x test clean build<br />--rerun-tasks: 强制执行任务，忽略up-to-date ,常见gradle build --rerun-tasks<br />--continue: 忽略前面失败的任务,继续执行,而不是在遇到第一个失败时立即停止执行. 每个遇到的故障都将在构建结束时报告，常见：gradle build --continue. <br />gradle init --type pom :将maven 项目转换为gradle 项目(根目录执行)<br />gradle [taskName] :执行自定义任务|

‍

### 现成**任务类型**

‍

一些现成的任务类型帮助我们快速完成想要的任务，我们只需要在创建任务的时候，指定当前任务的类型即可，然后即可使用这种类型中的属性和API 方法了. 

|**常见任务类型**|**该类型任务的作用**|
| --| --|
|**Delete**|**删除文件或目录**|
|**Copy**|**将文件复制到目标目录中. 此任务还可以在复制时重命名和筛选文件. **|
|**CreateStartScripts**|**创建启动脚本**|
|**Exec**|**执行命令行进程**|
|**GenerateMavenPom**|**生成 Maven 模块描述符(POM)文件. **|
|**GradleBuild**|**执行 Gradle 构建**|
|**Jar**|**组装 JAR 归档文件**|
|**JavaCompile**|**编译 Java 源文件**|
|**Javadoc**|**为 Java 类 生 成 HTML API 文 档**|
|**PublishToMavenRepository**|**将 MavenPublication  发布到 mavenartifactrepostal. **|
|**Tar**|**组装 TAR 存档文件**|
|**Test**|**执行 JUnit (3.8.x、4.x 或 5.x)或 TestNG 测试. **|
|**Upload**|**将 Configuration 的构件上传到一组存储库. **|
|**War**|**组装 WAR 档案. **|
|**Zip**|**组装 ZIP 归档文件. 默认是压缩 ZIP 的内容. **|

‍

### **顺序**

[三种方式](https://docs.gradle.org/current/dsl/org.gradle.api.Task.html)可以指定 Task 执行顺序：

1. dependsOn 强依赖方式
2. 通过 Task 输入输出
3. 通过 API 指定执行顺序

‍

‍

### 分配

一旦注册了任务,就可以通过 API 访问它们. 例如，您可以使用它在运行时动态地向任务添加依赖项. Ant 不允许这样的事情发生. 

‍

‍

构建 4 个任务,但是任务 0 必须依赖于任务 2 和 3,那么代表任务 2 和 3 需要在任务 0 之前优先加载

```groovy
times { counter -> tasks.register("task$counter") {
doLast {
println "I'm task number $counter"
}}}
tasks.named('task0') { dependsOn('task2', 'task3') }
```

‍

‍

### 开关

每个任务都有一个 enabled 默认为的标志 true. 将其设置为 false 阻止执行任何任务动作. 禁用的任务将标记为“跳过”

```groovy
task disableMe {
doLast {
println 'This task is Executing...'
}
enabled(true)//直接设置任务开启，默认值为true
}
```

‍

‍

### **超时**

任务都有一个 timeout 可用于限制其执行时间的属性. 当任务达到超时时，其任务执行线程将被中断. 该任务将被标记为失败. 终结器任务仍将运行. 如果 --continue 使用，其他任务可以在此之后继续运行. 不响应中断的任务无法超时. Gradle 的所有内置任务均会及时响应超时

```groovy
task a() {
doLast {
Thread.sleep(1000)
println "当前任务a执行了"
}
timeout = Duration.ofMillis(500)
}
task b() {
doLast {
println "当前任务b执行了"
}
}
```

‍

### **查找**

```groovy
//根据任务名查找
tasks.findByName("atguigu").doFirst({println "尚硅谷校区1：北京	"})
tasks.getByName("atguigu").doFirst({println "尚硅谷校区2：深圳	"})
//根据任务路径查找【相对路径】
tasks.findByPath(":atguigu").doFirst({println "尚硅谷校区3：上海		"}) tasks.getByPath(":atguigu").doFirst({println "尚硅谷校区4：武汉	"})
```

‍

### **规则**

‍

当我们执行、依赖一个不存在的任务时，Gradle 会执行失败,报错误信息. 那我们能否对其进行改进,当执行一个不存在的任务时，不是报错而是打印提示信息呢？

‍

```groovy
task hello {
doLast {
println 'hello 尚硅谷的粉丝们'
}
}

tasks.addRule("对该规则的一个描述，便于调试、查看等"){ String taskName -> task(taskName) {
doLast {
println "该${taskName}任务不存在，请查证后再执行"
}
}
}
```

‍

### **断言**

断言就是一个条件表达式. Task 有一个 onlyIf 方法. 它接受一个闭包作为参数，如果该闭包返回 true 则该任务执行， 否则跳过. 这有很多用途，比如控制程序哪些情况下打什么包，什么时候执行单元测试，什么情况下执行单元测试的时候不执行网络测试等. 具体案例如下所示：

```groovy
task hello {
doLast {
println 'hello 尚硅谷的粉丝们'
}
}

hello.onlyIf { !project.hasProperty('fensi') }
```

‍

### **默认任务**

‍

允许您定义一个或多个在没有指定其他任务时执行的默认任务

‍

```groovy
defaultTasks 'myClean', 'myRun' tasks.register('myClean'){
doLast {
println 'Default Cleaning!'
}
}
tasks.register('myRun') { doLast {
println 'Default Running!'
}
}
tasks.register('other') { doLast {
println "I'm not a default task!"
}
}
```

‍

## 依赖

**Dependencies**

‍

依赖在build.gradle文件内用dependencies围起区域

‍

### **依赖方式**

‍

依赖分别为直接依赖，项目依赖，本地jar 依赖

```groovy
dependencies {
    //①.依赖当前项目下的某个模块[子工程]
    implementation project(':subject01')
    //②.直接依赖本地的某个jar文件
    implementation files('libs/foo.jar', 'libs/bar.jar')
    //②.配置某文件夹作为依赖项
    implementation fileTree(dir: 'libs', include: ['*.jar'])
    //③.直接依赖
    implementation 'org.apache.logging.log4j:log4j:2.17.2'
}
```

‍

**直接依赖：在项目中直接导入的依赖，** 完整版写法如下：(使用冒号简略)

**implementation group: **​ ****'org.apache.logging.log4j'****​ **, name: **​ ****'log4j'****​ **, version**: '2.17.2'

‍

**group/name/version 共同定位一个远程仓库, version 最好写一个固定的版本号，以防构建出问题，** implementation 类似

‍

对比 maven 中的依赖:

>  **&lt;** dependencies **&gt;**
>
>  **    &lt;** dependency **&gt;**
>
>  **        &lt;** groupId **&gt;log4j&lt;/** groupId **&gt;**
>
>  **        &lt;** artifactId **&gt;log4j&lt;/** artifactId **&gt;**
>
>  **        &lt;** version **&gt;1.2.12&lt;/** version **&gt;**
>
>  **        &lt;** scope **&gt;compile&lt;/** scope **&gt; //依赖范围**
>
>  **    &lt;/** dependency **&gt;**
>
>  **&lt;/** dependencies **&gt;**

‍

‍

**项目依赖**: 从项目的某个模块依赖另一个模块

implementation **project(':subject01')**

这种依赖方式是直接依赖本工程中的libary module，这个 libary module 需要在setting.gradle 中配置. 

‍

本地jar 依赖：本地 jar 文件依赖，一般包含以下两种方式

‍

 **//直接依赖某文件**

implementation files('libs/foo.jar', 'libs/bar.jar')

‍

 */*​ **/配置某文件夹作为依赖项**

implementation fileTree(dir: 'libs', include: ['*.jar'])

‍

### 下载

‍

刷新后自动下载补全

‍

### **类型**

类似于maven的scope标签, 确定使用的范围

‍

|**compileOnly**|**由java插件**提供,**曾短暂的叫provided**,后续版本已经改成了compileOnly,适用于编译期需要而不需要打包的情**况**|
| :-| :---------------------------------------------------------------------------------------------------------------------|
|**runtimeOnly**|**由 java 插件**提供,只在运行期有效,编译时不需要,**比如mysql 驱动包**.  **,取代老版本中被移除的 runtime**|
|**implementation**|**由 java 插件**提供,**针对源码[src/main 目录]** ,在编译、运行时都有效,**取代老版本中被移除的 compile**|
|**testCompileOnly**|**由 java 插件**提供,用于编译测试的依赖项，运行时不需要|
|**testRuntimeOnly**|**由 java 插件**提供,只在测试运行时需要，而不是在测试编译时需要,**取代老版本中被移除的testRuntime**|
|**testImplementation**|**由 java 插件**提供,针对测试代码[src/test 目录] 取代老版本中被移除的testCompile|
|**providedCompile**|**war 插件提**供支持，编译、测试阶段代码需要依赖此类jar 包，而运行阶段容器已经提供了相应的支持，所**以无需将这些文件打入到war 包中了;** 例如servlet-api.jar、jsp-api.jar|
|**compile**|**编译范围依赖在所有的 classpath 中可用，同时它们也会被打包. ** 在gradle 7.0 已经移除|
|**runtime**|**runtime 依赖在运行和测试系统的时候需要,在编译的时候不需要,比如mysql 驱动包. ** 在 gradle 7.0 已经移除|
|**api**|**java-library 插件**提供支持,这些依赖项可以传递性地导出给使用者，用于编译时和运行时. 取代老版本中被**移除的 compile**|
|**compileOnlyApi**|**java-library 插件**提供支持,在声明模块和使用者在编译时需要的依赖项，但在运行时不需要. |

‍

#### 官方文档

‍

[https://docs.gradle.org/current/userguide/java_library_plugin.html#java_library_plugin](https://docs.gradle.org/current/userguide/java_library_plugin.html)​**[:](https://docs.gradle.org/current/userguide/java_library_plugin.html)**  各个依赖范围的关系和说明

[https://docs.gradle.org/current/userguide/upgrading_version_6.html#sec:configuration_removal** **](https://docs.gradle.org/current/userguide/upgrading_version_6.html): 依赖范围升级和移除

[https://docs.gradle.org/current/userguide/java_library_plugin.html#java_library_plugin](https://docs.gradle.org/current/userguide/java_library_plugin.html#java_library_plugin)：API 和implemention 区别

[https://docs.gradle.org/current/userguide/java_plugin.html#java_plugin](https://docs.gradle.org/current/userguide/java_plugin.html)​**[:](https://docs.gradle.org/current/userguide/java_plugin.html)**  执行java 命令时都使用了哪些依赖范围的依赖. 

‍

java 插件提供的功能，java-library 插件都提供 **. **

‍

#### **api 与implementation 区别**

<u>api类似'包含', imp类似'引用'</u>

api 的适用场景是多module 依赖，moduleA 工程依赖了 module B，同时module B 又需要依赖了 module C，modelA 工程也需要去依赖 module C,这个时候避免重复依赖module,可以使用 module B api  依赖的方式去依赖module C,modelA 工程只需要依赖 moduleB 即可.   

总之，除非涉及到多模块依赖，为了避免重复依赖咱们会使用api,其它情况我们**优先选择implementation**，拥有大量的api依赖项会显著增加构建时间. 

‍

‍

### **冲突**

默认下，Gradle 会使用最新版本的 jar 包【考虑到新版本的 jar 包一般都是向下兼容的】，实际开发中，还是建议使用官方自带的这种解决方案. 当然除此之外，Gradle 也为我们提供了一系列的解决依赖冲突的方法: exclude 移除一个依赖，不允许依赖传递，强制使用某个版本. 

‍

‍

#### Exclude 排除某个依赖

‍

```groovy

dependencies {
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1' testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1' implementation('org.hibernate:hibernate-core:3.6.3.Final'){

//排除某一个库(slf4j)依赖:如下三种写法都行
exclude group: 'org.slf4j' exclude module: 'slf4j-api'
exclude group: 'org.slf4j',module: 'slf4j-api'
}

//排除之后,使用手动的引入即可。implementation 'org.slf4j:slf4j-api:1.4.0'
}
```

‍

#### 不允许依赖传递

‍

```groovy
dependencies {
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1' testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1' implementation('org.hibernate:hibernate-core:3.6.3.Final'){
transitive(false)//不允许依赖传递，一般不用
}


//排除之后,使用手动的引入即可
implementation 'org.slf4j:slf4j-api:1.4.0'
}
在添加依赖项时,如果设置 transitive 为false,表示关闭依赖传递。即内部的所有依赖将不会添加到编译和运行时的类路径。
```

‍

#### 强制使用某个版本

  

```groovy
dependencies {
testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1' testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1' implementation('org.hibernate:hibernate-core:3.6.3.Final')

//强制使用某个版本!! 【官方建议使用这种方式】
implementation('org.slf4j:slf4j-api:1.4.0**!!**')


//这种效果和上面那种一样,强制指定某个版本
implementation('org.slf4j:slf4j-api:1.4.0'){
version{
strictly("1.4.0")
}}}
```

‍

‍

#### 查看依赖冲突

‍

```groovy
//配置当 Gradle 构建遇到依赖冲突时，就立即构建失败
configurations.all() {
Configuration configuration -\>
configuration.resolutionStrategy.failOnVersionConflict()//当遇到版本冲突时直接构建失败
}
```

‍

‍

## **插件Plugins**

‍

优点:

1. 促进代码重用、减少功能类似代码编写、提升工作效率
2. 促进项目更高程度的模块化、自动化、便捷化
3. 可插拔式的的扩展项目的功能

‍

作用:

1、可以添加任务【task】到项目中，从而帮助完成测试、编译、打包等. 

2、可以添加依赖配置到项目中. 

3、可以向项目中拓展新的扩展属性、方法等. 

4、可以对项目进行一些约定，如应用 Java 插件后，约定src/main/java 目录是我们的源代码存在位置，编译时编译这个目录下的Java 源代码文件. 

‍

### **分类**

‍

* 脚本插件
* 对象<sup>（二进制 ）</sup>插件

  * 内部插件
  * 第三方插件
  * 自定义插件

‍

‍

#### **脚本插件**

脚本插件的本质就是一个脚本文件，使用脚本插件时通过apply from:将脚本加载进来就可以了

‍

下面定义一段脚本，在 build.gradle 文件中使用

```groovy
//version.gradle文件
ext {
    company= "尚硅谷" cfgs = [
    compileSdkVersion : JavaVersion.VERSION_1_8
    ]

    spring = [
    version : '5.0.0'
    ]
}

//build.gradle文件
//map作为参数
apply from: 'version.gradle' task taskVersion{
    doLast{
        println "公司名称为：${company},JDK版本是${cfgs.compileSdkVersion},版本号是${spring.version}"
          }
}
```

‍

意义：脚本文件模块化的基础，可按功能把我们的脚本进行拆分一个个公用、职责分明的文件，然后在主脚本文件引用， 比如：将很多共有的库版本号一起管理、应用构建版本一起管理等. 

‍

‍

#### **内部插件**

(**核心插件)**

二进制插件[对象插件]就是实现了 org.gradle.api.Plugin  接口的插件，每个 Java Gradle 插件都有一个 plugin id

‍

可通过**map具名参数方式**使用一个 Java 插件

```groovy
apply plugin : 'java' 
```

‍

**或者：** 也可以使用**闭包**作为project.apply方法的一个参数

```groovy
apply{
    plugin 'java'
}
```

‍

将 Java 插件应用到我们的项目中了，对于 Gradle 自带的核心插件都有唯一的 plugin id，其中 java  是Java 插件的 plugin id,这个 plugin id 必须是唯一的，可使用应用包名来保证plugin id 的唯一性.   

这里的 java  对应的具体类型是 org.gradle.api.plugins.JavaPlugin，所以可以使用如下方式使用 Java 插件：

‍

使用方式1  Map具名参数,全类名

```groovy
apply plugin:org.gradle.api.plugins.JavaPlugin
//org.gradle.api.plugins默认导入
```

‍

使用方式2  apply **plugin:** JavaPlugin

```groovy
apply plugin: 'java' //核心插件，无需事先引入
```

使用方式3  **插件的id**

‍

[官方链接](https://docs.gradle.org/current/userguide/plugin_reference.html)

‍

#### **第三方插件**

使用第三方发布的二进制插件，一般需要配置对应的仓库和类路径

‍

**使用传统的应用方式:**

引用依赖和仓库信息, 然后应用

```groovy
buildscript {
    ext {
            springBootVersion = "2.3.3.RELEASE"
        }

repositories {
    mavenLocal()
        maven { url 'http://maven.aliyun.com/nexus/content/groups/public' }
    jcenter()
}

// 此处先引入插件
dependencies {
    classpath("org.springframework.boot:spring-boot-gradle-plugin:${springBootVersion}")
}


//再应用插件
apply plugin: 'org.springframework.boot' //社区插件,需要事先引入，不必写版本号

```

‍

新式方式DSL

如果是第三方插件已经被托管在 https://plugins.gradle.org/ 网站上，就可以不用在 buildscript 里配置 classpath依赖了，直接使用新出的 plugins DSL 的方式引用：

```groovy
plugins {
    id 'org.springframework.boot' version '2.4.1'
}
```

‍

注意：

1. 如果使用老式插件方式buildscript{}要放在build.gradle 文件的最前面,而新式plugins{}没有该限制
2. 托管在网站gradle 插件官网的第三方插件有两种使用方式，一是传统的buildscript 方式，一种是 plugins DSL 方式

‍

‍

#### **自定义插件**

‍

##### 自用版

这种方式实现的插件我们一般不使用，因为这种方式局限性太强，只能本Project，而其他的Project 不能使用. 

```groovy
interface GreetingPluginExtension { 
Property<String> getMessage() Property<String> getGreeter()
}

class GreetingPlugin implements Plugin<Project> {
    void apply(Project project) {
        def extension = project.extensions.create('greeting', GreetingPluginExtension)
        project.task('hello') {
            doLast {
                println "${extension.message.get()} from ${extension.greeter.get()}"
            }
        }
    }
}

apply plugin: GreetingPlugin

// Configure the extension using a DSL block
greeting{
message = 'Hi'
greeter = 'Gradle'
}

//执行: gradle -q hello
```

‍

##### buildSrc项目

‍

buildSrc 是Gradle 默认的插件目录，编译 Gradle 的时候会自动识别这个目录，将其中的代码编译为插件. 项目中都可使用

还可以把插件上传maven中

略

‍

### **关注点**

‍

#### **插件引用**

apply plugin: '插件名'

‍

#### 主要功能

Gradle 中的任务依赖关系是很重要的，它们之间的依赖关系就形成了构建的基本流程. 

‍

#### 工程结构

有些插件对工程目结构有约定，所以我们一般遵循它的约定结构来创建工程，这也是 Gradle 的“约定优于配置”原则

‍

#### **依赖管理**

不同的插件提供了不同的依赖管理

‍

#### **常用属性**

例如Java插件会为工程添加一些常用的属性,我们可以直接在编译脚本中直接使用

‍

### **分析**

‍

**参考官方文档**

‍

‍

## 包装器

Wrapper

‍

**Gradle Wrapper**实际上就是对 Gradle 的一层包装，用于解决实际开发中可能会遇到的不同的项目需要不同版本的 Gradle. 实际上有了 Gradle Wrapper 之后，我们本地是可以不配置 Gradle 的,下载Gradle 项目后，使用 gradle 项目自带的wrapper 操作也是可以的. 

gradlew、gradlew.cmd的使用方式与gradle使用方式完全一致，只不过把gradle指令换成了gradlew指令.   
当然,我们也可在终端执行 gradlew 指令时，指定指定一些参数,来控制 Wrapper 的生成，比如依赖的版本等

‍

GradleWrapper 的执行流程：

1. 当我们第一次执行 ./gradlew build 命令的时候，gradlew 会读取 gradle-wrapper.properties 文件的配置信息
2. 准确的将指定版本的 gradle 下载并解压到指定的位置(GRADLE\_USER\_HOME目录下的wrapper/dists目录中)
3. 构建本地缓存(GRADLE\_USER\_HOME目录下的caches目录中),下载再使用相同版本的gradle就不用下载了4.之后执行的 ./gradlew 所有命令都是使用指定的 gradle 版本

‍

注意：前面提到的 GRALE\_USER\_HOME 环境变量用于这里的Gradle Wrapper 下载的特定版本的gradle 存储目录. 如果我们没有配置过GRALE\_USER\_HOME 环境变量,默认在当前用户家目录下的.gradle 文件夹中. 

‍

### 使用

‍

下载别人的项目或者使用操作以前自己写的不同版本的gradle项目时：用Gradle wrapper,也即:gradlew  

使用本地gradle?新建一个项目时: 使用gradle指令即可

‍

‍

‍

‍

## 测试

Test

‍

测试任务自动检测并执行测试源集中的所有单元测试. 测试执行完成后会生成一个报告. 支持JUnit 和 TestNG 测试. 

无论是 Junt4.x 版本还是Junit5.x 版本，我们只需在 build.gradle 目录下执行gradle test 指令，gradle 就会帮我们执行所有的加了@Test 注解的测试，并生成测试报告. 

‍

示例

```groovy
dependencies {
    testImplementation 'org.junit.jupiter:junit-jupiter-api:5.8.1' 
    testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine:5.8.1'
}
test {
    useJUnitPlatform()
}
```

‍

‍

## **文件操作**

‍

几种常见的文件操作方式：

* **本地文件**​
* **文件集合**
* **文件树**
* **文件拷贝**
* **归档文件**

**参考官方文档：**​[https://docs.gradle.org/current/userguide/working_with_files.htm](https://docs.gradle.org/current/userguide/working_with_files.html)​**[l](https://docs.gradle.org/current/userguide/working_with_files.html)**

‍

‍

## **项目发布**

‍

### 基础三步走

#### 引入发布插件

```groovy
plugins {
id 'java-library' //如果发布war包，需要war插件,java-library支持带源码、文档发布
id 'maven-publish'
}
```

‍

#### 设置发布代码

```groovy
//带源码和javadoc的发布:需要'java-library'插件支持:它是java的升级版，java插件的功能java-library都有
javadoc.options.encoding="UTF-8" //防止乱码
java {
	withJavadocJar() //带有Java文档
	withSourcesJar() //带有源文件
} 
```

```groovy
publishing {
    publications {
        myLibrary(MavenPublication) {
                groupId = 'org.gradle.sample' //指定GAV坐标信息artifactId = 'library'
                version = '1.1'

                from components.java//发布jar包
                //from components.web///!需要引入war插件，发布war包
            }
        }
    repositories {
        //本地仓库位于USER_HOME/.m2/repository mavenLocal()
        //发布项目到私服中
        maven {
            name = 'myRepo' //name属性可选,表示仓库名称，url必填
            //发布地址:可以是本地仓库或者maven私服
  
            //url = layout.buildDirectory.dir("repo")
            // change URLs to point to your repos, e.g. http://my.org/repo
            def releasesRepoUrl = layout.buildDirectory.dir('repos/releases')
            def snapshotsRepoUrl = layout.buildDirectory.dir('repos/snapshots')
            url = version.endsWith('SNAPSHOT') ? snapshotsRepoUrl : releasesRepoUrl
  
            //认证信息:用户名和密码
            //credentials {
            //username = 'joe'
            //password = 'secret'
            //}
        }
    }
}
```

‍

#### 执行发布指令

publish    发布到 repositories 中指定的仓库(为比如 Maven 私服)

publishToMavenLocal    : 执行所有发布任务中的操作发布到本地 maven 仓库【默认在用户家目录下.m2/repository】. 

‍

### 打包部署

‍

将一个java项目打成war 包之后，就需要部署到服务器运行，使用Tomcat或是Gretty插件

(Tomcat不赘述)

‍

‍

‍

# 高级

‍

## 分模块

SSM多项目, 和Maven类似, 抽取公共依赖至父工程进行统一管理

‍

### 示例

代码和配置文件同单体ssm一样. 只不过做了拆分

‍

**在根工程settings.gradle文件设置**

```groovy
rootProject.name = 'meinian-parent' 
include 'meinian-bean'
include 'meinian-dao' 
include 'meinian-service' 
include 'meinian-web'
include 'meinian-mobile-web'
//...... include所有东西
```

‍

**在根工程 build.gradle 文件中抽取子模块的公共配置**

```groovy
group 'com.atguigu' version '1.0-SNAPSHOT'
  
    subprojects {
    //添加插件
    apply plugin: 'java'
    //基本JDK配置sourceCompatibility = 1.8
    targetCompatibility = 1.8
  
    compileJava.options.encoding "UTF-8" compileTestJava.options.encoding "UTF-8"
  
    tasks.withType(JavaCompile) { 
        options.encoding = "UTF-8"
    }
  
    group 'com.atguigu' version '1.0-SNAPSHOT'
  
    repositories {
        mavenLocal()
        maven {url "https://maven.aliyun.com/repository/public"} 
        maven {url "https://maven.aliyun.com/repository/central"} 
        maven {url "https://maven.aliyun.com/repository/google"} 
        maven {url "https://maven.aliyun.com/repository/spring"} 
        mavenCentral()
    }
  
    //依赖的配置:设置通用的依赖
  
    dependencies {
        testImplementation 'org.junit.jupiter:junit-jupiter-api' testRuntimeOnly 'org.junit.jupiter:junit-jupiter-engine' implementation 'log4j:log4j:1.2.17'
    }
  
    test {
        useJUnitPlatform()
    }
  
}
```

‍

**在根工程的build.gradle 文件中配置各个模块的依赖信息**

```groovy
project("meinian-bean"){ 
    dependencies {
    compileOnly 'org.projectlombok:lombok:1.18.24'
    }
}

project("meinian-dao"){
apply plugin: 'java-library'//支持api 

dependencies {
    api project(':meinian-bean')
    implementation 'org.mybatis:mybatis-spring:1.2.3' implementation 'com.alibaba:druid:1.0.15' implementation 'org.mybatis:mybatis:3.3.0' implementation 'mysql:mysql-connector-java:5.1.36'
}

}

project("meinian-service"){

apply plugin: 'java-library'//支持api 

dependencies {
    api project(':meinian-dao')
    implementation 'org.springframework:spring-web:4.1.7.RELEASE' implementation 'org.springframework:spring-test:4.0.5.RELEASE' implementation 'org.springframework:spring-jdbc:4.1.7.RELEASE' implementation 'org.aspectj:aspectjweaver:1.8.6'
}

}

project("meinian-web"){ 

apply plugin: 'war' 

dependencies {
    implementation project(':meinian-service')
    implementation 'org.springframework:spring-webmvc:4.1.7.RELEASE' implementation "com.fasterxml.jackson.core:jackson-databind:2.2.3" implementation "com.fasterxml.jackson.core:jackson-annotations:2.2.3" implementation "com.fasterxml.jackson.core:jackson-core:2.2.3" compileOnly 'javax.servlet:servlet-api:2.5'
    implementation 'jstl:jstl:1.2'
}

}

project("meinian-mobile-web"){ 

apply plugin: 'war' 

dependencies {
    //implementation project(':meinian-bean') implementation project(':meinian-service')
    implementation 'org.springframework:spring-webmvc:4.1.7.RELEASE' implementation "com.fasterxml.jackson.core:jackson-databind:2.2.3" implementation "com.fasterxml.jackson.core:jackson-annotations:2.2.3" implementation "com.fasterxml.jackson.core:jackson-core:2.2.3" compileOnly 'javax.servlet:servlet-api:2.5'
    implementation 'jstl:jstl:1.2'
}

}
```

抽取之后，各子模块的build.gradle 文件就不用配置了. 

‍

## Gretty插件

‍

Gretty 是一个功能丰富的 gradle 插件，用于在嵌入的 servlet 容器上运行 web 应用程序,让项目开发和部署更加简单. 目前Gretty 插件已经作为 gradle 的核心库使用了,Gretty 其核心功能为：

底层支持 jetty,tomcat 等Servlet 容器  
支持项目热部署、HTTPS、调试

‍

部署流程

**第一步：引入 Gretty 插件**

```groovy
plugins {
    id 'war'
    id 'org.gretty' version '2.2.0'
}
```

‍

**第二步:指定maven 仓库**

```groovy
repositories {
    //指定jcenter仓库，一定要放在前面
    jcenter() 
    mavenCentral()
}
```

‍

**第三步:针对Gretty 插件的设置**

```groovy
gretty {
    httpPort = 8888
    contextPath = "/web"
    debugPort = 5005	// default 
    debugSuspend = true // default 
    httpsEnabled = true
    managedClassReload=true // 修改了类之后重新加载
    //servletContainer = 'tomcat8' //如果不指定默认的servlet容器，支持tomcat7/8，默认是使用的是Jetty服务器
    httpsPort = 4431
}
```

‍

**第四步:执行Gretty 插件**

​`gradle appRun`​

‍

‍

‍

# 实操

‍
