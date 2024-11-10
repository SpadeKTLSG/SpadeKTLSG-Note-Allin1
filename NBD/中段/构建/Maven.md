--Java项目管理

‍

‍

管理和构建java项目的工具, 基于项目对象模型POM(Project Object Model)概念，通过一小段描述信息来管理项目的构建、报告和文档

‍

### Header

‍

‍

#### 环境

‍

‍

‍

‍

# 知识

‍

## 概念

‍

### 内容

‍

‍

> 高级内容

* 分模块设计与开发
* 继承与聚合
* 私服

‍

‍

### 评价

‍

**优势**

1. 依赖管理:  方便快捷的管理项目依赖的资源(jar包)，避免版本冲突问题
2. 统一项目结构:  自动生成统一、标准的项目目录结构
3. 项目构建:  提供了标准的、跨平台的自动化项目构建方式

‍

# 基础

‍

## 搭建

‍

### 配置

‍

#### classpath

classpath代表的是类路径. 在maven的项目中，其实指的就是

‍

src/main/resources    配置文件及静态资源文档地方

或

src/main/java    存放源代码地方

‍

#### 系统环境变量

> 需要配置

系统环境变量 >> M2_HOME Maven目录下的bin

            >> MAVEN_HOME Maven目录

系统path >> %MAVEN_HOME%\bin

‍

‍

### 镜像

‍

‍

#### 可选列表

‍

##### 阿里云

```xml
<mirror>  
	<id>alimaven</id>  
	<name>aliyun maven</name>  
	<url>http://maven.aliyun.com/nexus/content/groups/public/</url>
	<mirrorOf>central</mirrorOf>  
</mirror>
```

‍

#### 配置

[官网](http://maven.apache.org/settings.html)建议

有时候，阿里云maven源有的包下载不了

设置如果阿里云下载不了就去下载中央库

‍

设置包括mirror和profile两部分

‍

**mirrors**

```html
     <mirror>
     <id>aliyunmaven</id>
     <mirrorOf>central</mirrorOf>
     <name>阿里云公共仓库</name>
     <url>https://maven.aliyun.com/repository/central</url>
    </mirror>
    <mirror>
      <id>repo1</id>
      <mirrorOf>central</mirrorOf>
      <name>central repo</name>
      <url>http://repo1.maven.org/maven2/</url>
    </mirror>
    <mirror>
     <id>aliyunmaven</id>
     <mirrorOf>apache snapshots</mirrorOf>
     <name>阿里云阿帕奇仓库</name>
     <url>https://maven.aliyun.com/repository/apache-snapshots</url>
    </mirror>
```

‍

**profiles**

```html
    <profile>  
        <repositories>
           <repository>
                <id>aliyunmaven</id>
                <name>aliyunmaven</name>
                <url>https://maven.aliyun.com/repository/public</url>
                <layout>default</layout>
                <releases>
                        <enabled>true</enabled>
                </releases>
                <snapshots>
                        <enabled>true</enabled>
                </snapshots>
            </repository>
            <repository>
                <id>MavenCentral</id>
                <url>http://repo1.maven.org/maven2/</url>
            </repository>
            <repository>
                <id>aliyunmavenApache</id>
                <url>https://maven.aliyun.com/repository/apache-snapshots</url>
            </repository>
        </repositories>       
     </profile>
```

‍

‍

### 项目导入

‍

(目标是对应项目的pom.xml文件)

‍

方式

* Maven面板    <kbd>+</kbd>​快速导入
* idea导入模块    项目结构->模块->导入模块

‍

‍

#### 建议

1. 建议外部进入新项目时候点击pom文件打开
2. 初始化时候src-main/test和pom以外的文件/文件夹可以删掉
3. 一般选择的Java开始模板: maven-archetype-quickstart
4. 刚开始在idea中搜索不到仓库中的jar包, 因为仓库中的jar包索引尚未更新到idea中, 需要更新idea中maven的索引(刷新)
5. 在刚开始时候, 如果长时间没有构建成功, 很可能是网络代理问题或者是设置中的Maven路径配置不正确, 比如切换为了默认的Maven, 然后全部堆到自己的C盘

‍

‍

‍

‍

‍

## 组成

‍

### 模型

‍

‍

#### 构建生命周期/阶段

利用插件来完成标准化构建流程

‍

#### 项目对象模型

将项目抽象成一个对象模型，有自己专属的坐标

‍

#### 依赖管理模型

项目中需要jar包时，通过Maven直接把jar包复制到项目下的lib目录

‍

‍

### 结构

‍

src-> main/test    实际项目/测试项目

pom.xml    项目配置

‍

* src/main/java: java源代码目录
* src/main/resources: 配置文件目录
* src/test/java: 测试代码
* src/test/resources: 测试配置文件信息

‍

‍

#### Pom文件

‍

信息

```xml
    <modelVersion>4.0.0</modelVersion>

    <!-- 当前项目坐标 -->
    <groupId>com.itheima</groupId>
    <artifactId>maven_project1</artifactId>
    <version>1.0-SNAPSHOT</version>

    <!-- 打包方式 -->
    <packaging>jar</packaging>
```

‍

‍

### 仓库

用于存储资源，管理各种jar包, 本质就是一个目录

用来存储开发中所有依赖(就是jar包)和插件

‍

#### 分类

* 本地仓库： 自己计算机上的一个目录(用来存储jar包)
* 中央仓库： Maven团队维护, 全球唯一. 地址：[https://repo1.maven.org/maven2/](https://repo1.maven.org/maven2/)
* 远程仓库： 一般由公司团队搭建的私有仓库, 也叫私服

‍

‍

‍

### 坐标

坐标，就是资源(jar包)的唯一标识，通过坐标可以定位到所需资源(jar包)位置, 定义项目或引入项目中需要的依赖

‍

‍

#### 组成

* groupId   定义当前Maven项目隶属组织名称（通常是域名反写）
* artifactId   定义当前Maven项目名称（通常是模块名称）
* version   定义当前项目版本号

‍

#### 查找

不知道具体的坐标, 从mvn的仓库`https://mvnrepository.com/`​中进行搜索

‍

‍

#### 作用范围限制

导入依赖的jar包后，默认情况下，可以在任何地方使用

scope标签限制, 就加在<dependency>里面

‍

‍

* 主程序    main文件夹范围内
* 测试程序    test文件夹范围内
* 打包/运行    package指令范围内

‍

|**scope**值|**主程序**|**测试程序**|**打包（运行）**|**范例**|
| -----------------| ---| ---| ---| -------------|
|compile（默认）|Y|Y|Y|log4j|
|test|-|Y|-|junit|
|provided|Y|Y|-|servlet-api|
|runtime|-|Y|Y|jdbc驱动|

‍

‍

## 属性

‍

声明一个属性变量，在其他地方使用该变量，当变量的值发生变化后，所有使用变量的地方，就会跟着修改

‍

### 类型

‍

‍

==类型==

* 自定义属性（常用）   ${XXX}
* 内置属性            ${XXX}
* Setting属性        ${setting.XXX}
* Java系统属性        ${系统属性分类.XXX}
* 环境变量属性        ${env.XXX}

‍

‍

#### 查看

例如获取系统属性

在cmd命令行中输入`mvn help:system`​

具体使用，就是使用 `${key}`​来获取，key为等号左边的，值为等号右边的，比如获取红线的值，对应的写法为 `${java.runtime.name}`​

‍

‍

### 管理

‍

#### 基础属性解耦

‍

父工程中定义属性properties

```xml
<properties>
    <spring.version>5.2.10.RELEASE</spring.version>
    <junit.version>4.12</junit.version>
    <mybatis-spring.version>1.3.0</mybatis-spring.version>
</properties>
```

‍

修改依赖的version

这样就利用{}在需要的时候调用对应信息了

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-webmvc</artifactId>
    <version>${spring.version}</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>${spring.version}</version>
</dependency>
```

‍

‍

#### 扩大属性范围

‍

让Maven对于属性的管理范围能更大些

‍

##### 父工程定义属性

```xml
<properties>
   <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
</properties>
```

‍

##### jdbc.properties文件中引用属性

在jdbc.properties，将jdbc.url的值直接获取Maven配置的属性

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=${jdbc.url}
jdbc.username=root
jdbc.password=root
```

‍

##### 设置maven过滤文件范围

Maven在默认情况下是从当前项目的`src\main\resources`​下读取文件进行打包. 需要打包的资源文件是在maven_02_ssm下, 需要我们通过配置来指定下具体的资源目录. 

```xml
<build>
    <resources>
        <!--设置资源目录-->
        <resource>
            <directory>../maven_02_ssm/src/main/resources</directory>
            <!--设置能够解析${}，默认是false -->
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

‍

**说明:** directory路径前要添加`../`​的原因是maven_02_ssm相对于父工程的pom.xml路径是在其上一层的目录中，所以需要添加. 

‍

‍

#### 多项目属性管理

‍

##### 设置资源目录

笨方法--可以配，但是如果项目够多的话，这个配置也是比较繁琐

```xml
<build>
    <resources>
        <!--设置资源目录，并设置能够解析${}-->
        <resource>
            <directory>../maven_02_ssm/src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
        <resource>
            <directory>../maven_03_pojo/src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
        ...
    </resources>
</build>
```

‍

##### **basedir基础目录**

```xml
<build>
    <resources>
        <!--
			${project.basedir}: 当前项目所在目录,子项目继承了父项目，
			相当于所有的子项目都添加了资源目录的过滤
		-->
        <resource>
            <directory>${project.basedir}/src/main/resources</directory>
            <filtering>true</filtering>
        </resource>
    </resources>
</build>
```

‍

‍

‍

#### 没有找到web.xml问题解决

原因就是Maven发现你的项目为web项目，就会去找web项目的入口web.xml[配置文件配置的方式]，发现没有找到，就会报错. 

‍

##### 方法1 添加一个web.xml文件

在maven_02_ssm项目的`src\main\webapp\WEB-INF\`​添加一个web.xml文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
         version="3.1">
</web-app>
```

‍

##### 方法2 忽略web.xml检查

配置maven打包war时，忽略web.xml检查

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.2.3</version>
            <configuration>
                <failOnMissingWebXml>false</failOnMissingWebXml>
            </configuration>
        </plugin>
    </plugins>
</build>
```

‍

‍

### 版本管理

‍

#### SNAPSHOT（快照版本）

* 项目开发过程中临时输出的版本，称为快照版本
* 快照版本会随着开发的进展不断更新

‍

#### RELEASE（发布版本）

* 项目开发到一定阶段里程碑后，向团队外部发布较为稳定的版本，这种版本所对应的构件文件是稳定的
* 即便进行功能的后续开发，也不会改变当前发布版本内容，这种版本称为发布版本

‍

‍

还经常能看到一些发布版本

* alpha版:内测版，bug多不稳定内部版本不断添加新功能
* beta版:公测版，不稳定(比alpha稳定些)，bug相对较多不断添加新功能
* 纯数字版

‍

‍

‍

## 依赖

‍

指当前项目运行所需要的jar包

* 依赖传递
* 可选依赖
* 排除依赖

‍

### 引入依赖

‍

#### **找依赖**

* 利用中央仓库搜索的依赖坐标
* 利用IDEA工具搜索依赖
* 熟练上手maven后，快速导入依赖

‍

#### 流程

‍

1. 在 pom.xml 中编写 <dependencies> 标签
2. 在 <dependencies> 标签中 使用 <dependency> 引入坐标
3. 定义坐标的 groupId 包结构，artifactId 项目名，version 版本号
4. 点击右上角刷新依赖按钮，引入最新加入的坐标()

‍

‍

### 依赖传递性

‍

maven的依赖具有传递性，会自动把所依赖的其他jar包也一起导入; **有的依赖前面有箭头**​ **​`>`​** ​ **, 有的依赖前面没有, 这就是间接依赖了**

‍

#### 直接依赖

在当前项目中通过依赖配置建立的依赖关系

‍

#### 间接依赖

被依赖的资源如果依赖其他资源，当前项目间接依赖该其他资源

‍

##### 度

从当前出发, 直接依赖的, 间接依赖的,间间接依赖的 ... 标记为1度, 2度, 3度... **度数越低, 离主人越近**

‍

### 依赖覆盖

> 不用刻意去记，一切以maven的dependencies面板上显示的为准

* 特殊优先    当同级配置了相同资源的不同版本，后配置的覆盖先配置的.
* 路径优先    当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
* 声明优先    当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的

‍

‍

### 依赖查找顺序

‍

本地仓库 --> 远程仓库<sup>（拥有私服的情况下）</sup>--> 中央仓库

‍

### 可选依赖

‍

可选依赖指对外隐藏当前所依赖的资源---不透明

```java
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>maven_03_pojo</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--可选依赖是隐藏当前工程所依赖的资源，隐藏后对应资源将不具有依赖传递-->
    <optional>true</optional>
</dependency>
```

‍

‍

### 排除依赖

排除依赖指主动断开依赖的资源，被排除的资源无需指定版本---不需要

‍

‍

#### 冲突

指项目依赖的某一个jar包，有多个不同的版本，因而造成类包版本冲突

‍

‍

#### 实操

主动断开依赖的资源

‍

在依赖项列表中的引入处和<三个>同级的地方写 exclusions+exclude, 再写 groupid和artifactid 组id和工件id, 被排除的资源无需指定版本

```xml
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>maven_04_dao</artifactId>
    <version>1.0-SNAPSHOT</version>
    <!--排除依赖是隐藏当前资源对应的依赖关系-->
    <exclusions>
        <exclusion>
            <groupId>com.itheima</groupId>
            <artifactId>maven_03_pojo</artifactId>
        </exclusion>
    </exclusions>
</dependency>
```

> ​`exclusions`​标签带`s`​说明我们是可以依次排除多个依赖到的jar包，比如maven_04_dao中有依赖junit和mybatis,我们也可以一并将其排除. 
>
> 不管Maven怎么选，最终的结果都会在Maven的`Dependencies`​面板中展示出来，展示的是哪个版本，也就是说它选择的就是哪个版本

‍

如果想更全面的查看Maven中各个坐标的依赖关系，可以点击Maven面板中的`show Dependencies`​(图表)

‍

‍

‍

## 生命周期

‍

Maven的生命周期是抽象的, 包含了项目的清理，初始化，编译，测试，打包，集成测试，验证，部署和站点生成等阶段

在**同一套**生命周期中，执行后面的生命周期时，前面的生命周期都会执行

‍

### 概念

‍

#### **抽象**

生命周期本身不做任何实际工作, 实际任务都交由插件来完成

‍

#### 分组

可以被划分为**相互独立的**3套

* clean：只有清理工作
* default：核心工作. 编译、测试、打包、安装、部署等
* site：生成报告、发布站点等

‍

‍

#### 常用

• clean    移除上一次构建生成的文件

• compile    编译项目源代码

• test    使用合适的单元测试框架运行测试(junit)

• package    将编译后的文件打包，生成jar文件放到target目录下

• install    安装项目到本地仓库, 进入Maven.repo.项目坐标位置查看jar文件

‍

‍

‍

### 执行

‍

#### 操作

* idea工具栏，选择对应的生命周期执行, 运行可以点快捷栏绿三角
* idea如果只想完成当前选定的任务而跳过前面的任务,点击闪电
* 在DOS命令行中，maven命令执行 **​`mvn clean ......`​** ​

‍

#### 顺序

‍

##### 常用

    **clean --&gt; compile --&gt; test --&gt; package --&gt; install**

‍

‍

##### 全部

> 当运行package生命周期时，clean不会运行，compile会运行.  因为compile与package属于同一套生命周期，而clean与package不属于同一套生命周期

‍

    clean --> validate --> compile --> test --> package --> verify --> install --> site --> deploy

‍

|阶段|处理|描述||
| ---------------| ----------| ----------------------------------------------------------| --|
|验证 validate|验证项目|验证项目是否正确且所有必须信息是可用的||
|编译 compile|执行编译|源代码编译在此阶段完成||
|测试 Test|测试|使用适当的单元测试框架（例如JUnit）运行测试。||
|包装 package|打包|创建JAR/WAR包如在 pom.xml 中定义提及的包||
|检查 verify|检查|对集成测试的结果进行检查，以保证质量达标||
|安装 install|安装|安装打包的项目到本地仓库，以供其他项目使用||
|部署 deploy|部署|拷贝最终的工程包到远程仓库中，以共享给其他开发人员和工程||

‍

#### clean生命周期

清理项目，包含三个phase。

1. pre-clean：执行清理前需要完成的工作
2. clean：清理上一次构建生成的文件
3. post-clean：执行清理后需要完成的工作

‍

#### default生命周期

构建项目，重要的phase如下。

1. validate：验证工程是否正确，所有需要的资源是否可用。
2. compile：编译项目的源代码。
3. test：使用合适的单元测试框架来测试已编译的源代码。这些测试不需要已打包和布署。
4. Package：把已编译的代码打包成可发布的格式，比如jar。
5. integration-test：如有需要，将包处理和发布到一个能够进行集成测试的环境。
6. verify：运行所有检查，验证包是否有效且达到质量标准。
7. install：把包安装到maven本地仓库，可以被其他工程作为依赖来使用。
8. Deploy：在集成或者发布环境下执行，将最终版本的包拷贝到远程的repository，使得其他的开发者或者工程可以共享。

‍

#### site生命周期

建立和发布项目站点，phase如下

1. pre-site：生成项目站点之前需要完成的工作
2. site：生成项目站点文档
3. post-site：生成项目站点之后需要完成的工作
4. site-deploy：将项目站点发布到服务器

‍

‍

### 打包

‍

资源会放到target目录中, 默认只打此项目内容, 不打依赖项

‍

#### 类型

‍

##### jar

普通java模块打包，springboot项目基本都是jar包（内嵌tomcat运行）,大多数

‍

##### war

普通web程序打包，需要部署在外部的tomcat服务器中运行. 少

‍

##### pom

父工程或聚合工程，该模块不写代码，仅进行依赖管理

‍

‍

## 测试

‍

### 单元测试

‍

‍

> test目录下已经自动创建好了测试类, 有注解@SpringBootTest，代表该测试类已经与SpringBoot整合, 运行时会自动通过引导类加载Spring的环境（IOC容器）

‍

‍

### 跳过测试

‍

在执行`install`​指令的时候，Maven都会按照顺序从上往下依次执行，每次都会执行`test`​

‍

#### 含义

跳过测试环节

* 可以确保每次打包或者安装的时候，程序的正确性，假如测试已经通过在我们没有修改程序的前提下再次执行打包或安装命令，由于顺序执行，测试会被再次执行，就有点耗费时间了.
* 功能开发过程中有部分模块还没有开发完毕，**测试无法通过**，但是想要把其中某一部分进行快速打包，此时由于测试环境失败就会导致打包失败.

‍

#### 实现

‍

##### IDEA工具

‍

按钮为`Toggle 'Skip Tests' Mode`​

在测试与不测试之间进行切换

点击一下，出现测试画横线的图片说明测试已经被关闭，再次点击就会恢复. 

‍

这种方式最简单，但是有点"暴力"，**会把所有的测试都跳过**，如果我们想更精细的控制哪些跳过哪些不跳过，就需要使用配置插件的方式. 

‍

‍

##### 配置插件

‍

在父工程中的pom.xml中添加测试插件配置

```xml
<build>
    <plugins>
        <plugin>
            <artifactId>maven-surefire-plugin</artifactId>
            <version>2.12.4</version>
            <configuration>
                <skipTests>false</skipTests>
                <!--排除掉不参与测试的内容-->
                <excludes>
                    <exclude>**/BookServiceTest.java</exclude>
                </excludes>
            </configuration>
        </plugin>
    </plugins>
</build>
```

‍

**参数**

skipTests: 如果为true，则跳过所有测试，如果为false，则不跳过测试

excludes：哪些测试类不参与测试，即排除，针对skipTests为false来设置的

includes: 哪些测试类要参与测试，即包含,针对skipTests为true来设置的

‍

‍

##### 命令行

‍

使用Maven的命令行

```xml
mvn 指令 -D skipTests
```

‍

注意事项

* 执行的项目构建指令必须包含测试生命周期，否则无效果. 例如执行compile生命周期，不经过test生命周期.
* 该命令可以不借助IDEA，直接使用cmd命令行进行跳过测试，需要注意的是cmd要在pom.xml所在目录下进行执行.

‍

‍

‍

# 高级

‍

‍

‍

## 分模块

‍

### 拆分

一般组合起来的项目不方便项目的维护和管理、项目中的通用组件难以复用. 分模块设计我们在进行项目设计阶段，就可以将一个大的项目拆分成若干个模块，方便项目的管理维护、拓展，也方便模块键的相互调用、资源共享. 

‍

> 以pojo-utils-else组合为例拆分
>
> * 将pojo包下的实体类，抽取到一个maven模块中 tlias-pojo
> * 将utils包下的工具类，抽取到一个maven模块中 tlias-utils  
>   方法: 创建一个包, 注意路径对应(对应深度前的文件名称)
> * 其他的业务代码，放在tlias-web-management这个模块中，直接引入对应的依赖即可

‍

‍

#### **方法**

(1)按照功能拆分

(2)按照模块拆分

‍

### 流程

‍

(1) 创建Maven模块

(2) 书写模块代码

分模块开发需要先针对模块功能进行设计，再进行编码. 不会先将工程开发完毕，然后进行拆分. 拆分方式可以按照功能拆也可以按照模块拆. 

(3)通过maven指令安装模块到本地仓库(install 指令)

‍

‍

### 示例

‍

#### 抽取

抽取domain层

项目中创建domain包

拷贝到该包中

‍

删除原项目中的domain包

删除后，`maven_02_ssm`​项目中用到`Book`​的类中都会有红色提示

‍

‍

#### 建立依赖关系

在`maven_02_ssm`​项目的pom.xml添加`maven_03_pojo`​的依赖

```xml
<dependency>
    <groupId>com.itheima</groupId>
    <artifactId>maven_03_pojo</artifactId>
    <version>1.0-SNAPSHOT</version>
</dependency>
```

因为添加了依赖，所以在`maven_02_ssm`​中就已经能找到Book类，所以刚才的报红提示就会消失. 

‍

‍

#### 编译项目

报错注意

Maven会从本地仓库找对应的jar包，但是本地仓库又不存在该jar包所以会报错. 

在IDEA中有这个项目，则需要将其装到本地仓库

‍

**使用maven的install命令，把其安装到Maven的本地仓库中**

‍

‍

#### 构建聚合与继承工程

‍

创建一个空的Maven项目，可以将项目中的`src`​目录删除掉，这个项目作为聚合工程和父工程

‍

设置加入  作为模块

该项目可以被聚合工程管理，同时会继承父工程. 

创建成功后，maven_parent即是聚合工程又是父工程，maven_web中也有parent标签，继承的就是maven_parent,对于难以配置的内容都自动生成. 

‍

## 继承

继承描述的是两个工程间的关系，与java中的继承相似，子工程可以继承父工程中的配置信息，常见于依赖关系的继承. 

‍

### 问题

问题情境

> 三个工程pojo、utils、web-management中都引入了lombok的依赖, 分别配置了一次

问题解决

> 再创建一个父工程 parent ，然后让上述的三个模块来继承. 再将各个模块中都共有的依赖都提取到父工程 tlias-parent中进行配置，只要子工程继承了父工程，依赖它也会继承下来，这样就无需在各个子工程中进行配置了.

‍

### 概念

‍

#### **作用**

简化依赖配置、统一管理依赖, 减少版本冲突

‍

‍

‍

### 实现

‍

**简要**

1. 将所有项目公共的jar包依赖提取到父工程的pom.xml中，子项目就可以不用重复编写，简化开发

    **父工程主要是用来快速配置依赖jar包和管理项目中所使用的资源**

    将各个子工程当中共有的这部分依赖统一的定义在父工程 parent 当中

    ```xml
    <parent>
        <groupId>...</groupId>
        <artifactId>...</artifactId>
        <version>...</version>
        <relativePath>....</relativePath>
    </parent>
    ```
2. 将所有项目的jar包配置到父工程的dependencyManagement标签下，实现版本管理，方便维护

    * ==dependencyManagement标签不真正引入jar包，只是管理jar包的版本==
    * 子项目在引入的时候，只需要指定groupId和artifactId，不需要加version
    * 当dependencyManagement标签中jar包版本发生变化，所有子项目中有用到该jar包的地方对应的版本会自动随之更新

‍

‍

‍

* 创建Maven模块，设置打包类型为pom

  ```xml
  <packaging>pom</packaging>
  ```
* 在父工程的pom文件中配置依赖关系(子工程将沿用父工程中的依赖关系),一般只抽取子项目中公有的jar包

  ```xml
  <dependencies>
      <dependency>
          <groupId>org.springframework</groupId>
          <artifactId>spring-webmvc</artifactId>
          <version>5.2.10.RELEASE</version>
      </dependency>
      ...
  </dependencies>
  ```
* 在父工程中配置子工程中可选的依赖关系

  ```xml
  <dependencyManagement>
      <dependencies>
          <dependency>
              <groupId>com.alibaba</groupId>
              <artifactId>druid</artifactId>
              <version>1.1.16</version>
          </dependency>
      </dependencies>
      ...
  </dependencyManagement>
  ```
* 在子工程中配置当前工程所继承的父工程

  ```xml
  <!--定义该工程的父工程-->
  <parent>
      <groupId>com.itheima</groupId>
      <artifactId>maven_01_parent</artifactId>
      <version>1.0-RELEASE</version>
      <!--填写父工程的pom文件,可以不写-->
      <relativePath>../maven_01_parent/pom.xml</relativePath>
  </parent>
  ```
* 在子工程中配置使用父工程中可选依赖的坐标

  ```xml
  <dependencies>
      <dependency>
          <groupId>com.alibaba</groupId>
          <artifactId>druid</artifactId>
      </dependency>
  </dependencies>
  ```

‍

#### Tips

1. 设置打包方式 <packaging>pom</packaging>
2. 父工程中的src就可以删掉了,没用了
3. 子工程中，配置了继承关系之后，坐标中的groupId是可以省略的，因为会自动继承父工程
4. relativePath指定父工程的pom文件的相对位置（如果不指定，将从本地仓库/远程仓库查找该工程）
5. ../ 代表本项目的上一级目录
6. 若依赖版本冲突, **以子工程为准**
7. 所有的springboot项目都有一个统一的父工程  spring-boot-starter-parent
8. 子工程中使用父工程中的可选依赖时，仅需要提供群组id和项目id，无需提供版本，版本由父工程统一提供，避免版本冲突

    子工程中还可以定义父工程中**没有定义的依赖关系**,只不过不能被父工程进行版本统一管理.

‍

‍

### 多重继承

‍

> 与java语言类似，Maven不支持多继承，一个maven项目只能继承一个父工程，如果继承了spring-boot-starter-parent，就没法继承我们自己定义的父工程 tlias-parent了

‍

‍

‍

### 版本锁定

‍

父工程的pom文件 `<dependencyManagement>`​ 统一管理依赖版本,并不会将这个依赖直接引入进来. 子工程中引入时候即可省略<version>

‍

```xml
<!--统一管理依赖版本-->
<dependencyManagement>
    <dependencies>
        <!--坐标 参数 * 3-->
        <dependency>
           ......
        </dependency>
    </dependencies>
</dependencyManagement>
```

‍

### 优化子项目依赖版本

‍

#### **问题情境**

如果把所有用到的jar包都管理在父项目的pom.xml，看上去更简单些，但是这样就会导致有很多项目引入了过多自己不需要的jar包

‍

#### **在父工程的pom.xml来定义依赖管理**

```xml

<dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
    </dependencies>
</dependencyManagement>
```

‍

#### **将子工程的pom.xml中的junit依赖删除掉**

刷新Maven

刷新完会发现，在子工程maven_02_ssm项目中的junit依赖并没有出现，所以我们得到一个结论:

​==​`<dependencyManagement>`​==​==标签不真正引入jar包，而是配置可供子项目选择的jar包依赖==

子项目要想使用它所提供的这些jar包，需要自己添加依赖，并且不需要指定`<version>`​

‍

在子工程maven_02_ssm的pom.xml添加junit的依赖

```java
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <scope>test</scope>
</dependency>
```

‍

**注意：这里就不需要添加版本了，这样做的好处就是当父工程dependencyManagement标签中的版本发生变化后，子项目中的依赖版本也会跟着发生变化**

‍

在子工程maven_04_dao的pom.xml添加junit的依赖

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <scope>test</scope>
</dependency>
```

‍

这个时候，maven_02_ssm和maven_04_dao这两个项目中的junit版本就会跟随着父项目中的标签dependencyManagement中junit的版本发生变化而变化. 不需要junit的项目就不需要添加对应的依赖即可. 

‍

‍

‍

‍

### 示例

‍

创建一个空的Maven项目并将其打包方式设置为pom

‍

#### **在子项目中设置其父工程**

```xml
<parent>
    <groupId>com.itheima</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <!--设置父项目pom.xml位置路径-->
    <relativePath>../maven_01_parent/pom.xml</relativePath>
</parent>
```

‍

#### **优化子项目共有依赖导入问题**

‍

将子项目共同使用的jar包都抽取出来，维护在父项目的pom.xml中

删除子项目中已经被抽取到父项目的pom.xml中的jar包，如在`maven_02_ssm`​的pom.xml中将已经出现在父项目的jar包删除掉

删除完后，你会发现父项目中有依赖对应的jar包，子项目虽然已经将重复的依赖删除掉了，但是刷新的时候，子项目中所需要的jar包依然存在. 

当项目的`<parent>`​标签被移除掉，会发现多出来的jar包依赖也会随之消失. 

将子项目所有依赖删除，然后添加上父项目坐标

‍

‍

### 模板

‍

父工程parent的pom.xml文件配置

```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.7.5</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>

<groupId>com.itheima</groupId>
<artifactId>tlias-parent</artifactId>
<version>1.0-SNAPSHOT</version>
<packaging>pom</packaging>
```

‍

子工程配置

```xml
<parent>
    <groupId>com.itheima</groupId>
    <artifactId>tlias-parent</artifactId>
    <version>1.0-SNAPSHOT</version>
    <relativePath>../tlias-parent/pom.xml</relativePath>
</parent>

<artifactId>tlias-utils</artifactId>
<version>1.0-SNAPSHOT</version>
```

‍

在父工程中配置各个工程共有的依赖

```xml
<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>1.18.24</version>
    </dependency>
</dependencies>
```

‍

‍

‍

### 属性配置

‍

也可以通过自定义属性及属性引用的形式，在父工程中将依赖的版本号进行集中管理维护. 我们要想修改依赖的版本，就只需要在父工程中自定义属性的位置，修改对应的属性值即可

‍

#### 自定义属性

```xml
<properties>
	<lombok.version>1.18.24</lombok.version>
</properties>
```

‍

#### 引用属性${}

```xml
<dependency>
    <groupId>org.projectlombok</groupId>
    <artifactId>lombok</artifactId>
    <version>${lombok.version}</version>
</dependency>
```

‍

示例

```xml
<properties>
    <maven.compiler.source>11</maven.compiler.source>
    <maven.compiler.target>11</maven.compiler.target>
    <lombok.version>1.18.24</lombok.version>
    <jjwt.version>0.9.1</jjwt.version>
    <aliyun.oss.version>3.15.1</aliyun.oss.version>
    <jaxb.version>2.3.1</jaxb.version>
    <activation.version>1.1.1</activation.version>
    <jaxb.runtime.version>2.3.3</jaxb.runtime.version>
</properties>


<dependencies>
    <dependency>
        <groupId>org.projectlombok</groupId>
        <artifactId>lombok</artifactId>
        <version>${lombok.version}</version>
    </dependency>
</dependencies>

<!--统一管理依赖版本-->
<dependencyManagement>
    <dependencies>
        <!--JWT令牌-->
        <dependency>
            <groupId>io.jsonwebtoken</groupId>
            <artifactId>jjwt</artifactId>
            <version>${jjwt.version}</version>
        </dependency>
......
```

‍

‍

> ​ **​`<dependencyManagement>`​** ​ **与**  **​`<dependencies>`​** ​ **的区别**
>
> * ​`<dependencies>`​ 是直接依赖，在父工程配置了依赖，子工程会直接继承下来.
> * ​`<dependencyManagement>`​ 是统一管理依赖版本，不会直接依赖，还需要在子工程中引入所需依赖(无需指定版本)

‍

‍

## 聚合

问题情境

> 模块之间的依赖关系错综复杂，那此时要进行项目的打包、安装操作，是非常繁琐的.

‍

### 概念

‍

==聚合工程==

**聚合工程主要是用来管理项目,**  将多个模块组织成一个整体，轻松实现项目的一键构建

通常是一个不具有业务功能的"空"工程（有且仅有一个pom文件）

一般来说，继承关系中的父工程与聚合关系中的聚合工程是同一个

‍

**作用**

快速构建项目, 无需根据依赖关系手动构建，直接在聚合工程上构建即可, 所聚合的其他模块全部都会执行

当工程中某个模块发生更新（变更）时，必须保障工程中与已更新模块关联的模块同步更新，此时可以使用聚合工程来解决批量模块同步构建的问题. 

‍

**实现**

在聚合工程中通过 `<moudules>`​ 设置当前聚合工程所包含的子模块

```java
<!--聚合其他模块-->
<modules>
    <module>../tlias-pojo</module>
    <module>../tlias-utils</module>
    <module>../tlias-web-management</module>
</modules>
```

聚合工程管理的项目在进行运行的时候，会按照项目与项目之间的依赖关系来自动决定执行的顺序和配置的顺序无关

‍

‍

### 示例

‍

空的maven项目, 将项目的打包方式改为pom

‍

pom.xml添加所要管理的项目

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.itheima</groupId>
    <artifactId>maven_01_parent</artifactId>
    <version>1.0-RELEASE</version>
    <packaging>pom</packaging>
  
    <!--设置管理的模块名称-->
    <modules>
        <module>../maven_02_ssm</module>
        <module>../maven_03_pojo</module>
        <module>../maven_04_dao</module>
    </modules>
</project>
```

‍

‍

### 继聚对比

‍

#### 作用

* 聚合用于快速构建项目，对项目进行管理
* 继承用于快速配置和管理子项目中所使用jar包的版本(简化依赖配置、统一管理依赖)

‍

#### 相同点

* 聚合与继承的pom.xml文件打包方式均为pom，可以将两种关系制作到同一个pom文件中
* 聚合与继承均属于设计型模块，并无实际的模块内容

‍

#### 不同点

* 聚合是在当前模块中配置关系，聚合可以感知到参与聚合的模块有哪些
* 继承是在子模块中配置关系，父模块无法感知哪些子模块继承了自己

‍

‍

## 多环境

‍

### 流程

父工程中定义多环境

```xml
<profiles>
	<profile>
    	<id>环境名称</id>
        <properties>
        	<key>value</key>
        </properties>
        <activation>
        	<activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    ...
</profiles>
```

‍

使用多环境(构建过程)

```
mvn 指令 -P 环境定义ID[环境定义中获取]
```

‍

### 示例

‍

==父工程配置多个环境,并指定默认激活环境==

```xml
<profiles>
    <!--开发环境-->
    <profile>
        <id>env_dep</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.1.1.1:3306/ssm_db</jdbc.url>
        </properties>
        <!--设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
    </profile>
    <!--生产环境-->
    <profile>
        <id>env_pro</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.2.2.2:3306/ssm_db</jdbc.url>
        </properties>
    </profile>
    <!--测试环境-->
    <profile>
        <id>env_test</id>
        <properties>
            <jdbc.url>jdbc:mysql://127.3.3.3:3306/ssm_db</jdbc.url>
        </properties>
    </profile>
</profiles>
```

‍

==切换默认环境为生产环境==

调整位置即可

```html
        <!--设定是否为默认启动环境-->
        <activation>
            <activeByDefault>true</activeByDefault>
        </activation>
```

‍

==命令行实现环境切换==

```xml
mvn install -P env_test
```

‍

‍

## 私服

‍

一种特殊的远程仓库，它是架设在局域网内的仓库服务，用来代理位于外部的中央仓库，用于解决团队内部的资源共享与资源同步问题. 

‍

### 概念

私服    公司内部搭建的用于存储Maven资源的服务器

远程仓库    Maven开发团队维护的用于存储Maven资源的服务器

‍

#### **应用情境**

两个项目组之间如何基于私服进行资源的共享, 中央仓库全球只有一个,没用,有了私服之后，各个团队就可以直接来连接私服了. 

而如果我们在项目中需要使用其他第三方提供的依赖，如果本地仓库没有，也会自动连接私服下载，如果私服没有，私服此时会自动连接中央仓库，去中央仓库中下载依赖，然后将下载的依赖存储在私服仓库及本地仓库中. 

就是架设在局域网内部的一台服务器，一种特殊的远程仓库; 私服在企业项目开发中，一个项目/公司，只需要一台即可. 无需我们自己搭建，会使用即可

‍

‍

> (1)在没有私服的情况下，我们自己创建的服务都是安装在Maven的本地仓库中
>
> (2)私服中也有仓库，我们要把自己的资源上传到私服，最终也是放在私服的仓库中
>
> (3)其他人要想使用你所上传的资源，就需要从私服的仓库中获取
>
> (4)当我们要使用的资源不是自己写的，是远程中央仓库有的第三方jar包，这个时候就需要从远程中央仓库下载，每个开发者都去远程中央仓库下速度比较慢(中央仓库服务器在国外)
>
> (5)私服就再准备一个仓库，用来专门存储从远程中央仓库下载的第三方jar包，第一次访问没有就会去远程中央仓库下载，下次再访问就直接走私服下载
>
> (6)前面在介绍版本管理的时候提到过有`SNAPSHOT`​和`RELEASE`​，如果把这两类的都放到同一个仓库，比较混乱，所以私服就把这两个种jar包放入不同的仓库
>
> (7)上面我们已经介绍了有三种仓库，一种是存放`SNAPSHOT`​的，一种是存放`RELEASE`​还有一种是存放从远程仓库下载的第三方jar包，那么我们在获取资源的时候要从哪个仓库种获取呢?
>
> (8)为了方便获取，我们将所有的仓库编成一个组，我们只需要访问仓库组去获取资源.

‍

‍

### 分类

‍

‍

#### 私服**仓库分类**

* RELEASE：存储自己开发的RELEASE发布版本的资源.
* SNAPSHOT：存储自己开发的SNAPSHOT发布版本的资源.
* Central：存储的是从中央仓库下载下来的依赖.

‍

#### 项目版本分类

* RELEASE(发布版本)：功能趋于稳定、当前更新停止，可以用于发行的版本，存储在私服中的RELEASE仓库中.
* SNAPSHOT(快照版本)：功能不稳定、尚处于开发中的版本，即快照版本，存储在私服的SNAPSHOT仓库中.

‍

#### 私服三大类

‍

* 宿主仓库hosted  ->上传

  保存无法从中央仓库获取的资源

  * 存储自主研发
  * 存储第三方非开源项目,比如Oracle,因为是付费产品，所以中央仓库没有
* 代理仓库proxy  ->下载

  * 代理远程仓库，通过nexus访问其他公共仓库，例如中央仓库
* 仓库组group  ->下载

  * 将若干个仓库组成一个群组，简化配置
  * 仓库组不能保存资源，属于设计型仓库

‍

‍

‍

### 本地访问配置

‍

#### 问题情境

> * 通过IDEA将开发的模块上传到私服，中间是要经过本地Maven的
> * 本地Maven需要知道私服的访问地址以及私服访问的用户名和密码
> * 私服中的仓库很多，Maven最终要把资源上传到哪个仓库?
> * Maven下载的时候，又需要携带用户名和密码到私服上找对应的仓库组进行下载，然后再给IDEA

‍

需要在本地Maven的配置文件`settings.xml`​中进行配置

‍

‍

#### 私服上配置仓库

‍

设置方法路径(网页操作)

->Repositories->Create-> maven2(hosted) >>itheima-snapshot (+Version=Snapshot) + itheima-release (+Version=Snapshot) -> Confirm

​​

第5，6步骤是创建itheima-snapshot仓库

第7，8步骤是创建itheima-release仓库

‍

下面要启用本地和私服的交互需要2个对象

‍

‍

#### 配置本地Maven对私服的访问权限

```xml
<servers>
    <server>
        <id>itheima-snapshot</id>
        <username>admin</username>
        <password>admin</password>
    </server>
    <server>
        <id>itheima-release</id>
        <username>admin</username>
        <password>admin</password>
    </server>
</servers>
```

‍

‍

#### 配置私服访问路径

```xml
<mirrors>
    <mirror>
        <!--配置仓库组的ID-->
        <id>maven-public</id>
        <!--*代表所有内容都从私服获取-->
        <mirrorOf>*</mirrorOf>
        <!--私服仓库组maven-public的访问路径-->
        <url>http://localhost:8081/repository/maven-public/</url>
    </mirror>
</mirrors>
```

为了避免阿里云Maven私服地址的影响，建议先将之前配置的阿里云Maven私服镜像地址注释掉，等练习完后，再将其恢复. 

在Maven-Public中管理成员,加入刚创建的2个新仓库(注意上方URL是访问地址)

至此本地仓库就能与私服进行交互了

‍

‍

### 上传下载

‍

#### 配置工程上传私服的具体位置

pom.xml

```xml
 <!--配置当前工程保存在私服中的具体位置-->
<distributionManagement>
    <repository>
        <!--和maven/settings.xml中server中的id一致，表示使用该id对应的用户名和密码-->
        <id>itheima-release</id>
         <!--release版本上传仓库的具体地址-->
        <url>http://localhost:8081/repository/itheima-release/</url>
    </repository>
    <snapshotRepository>
        <!--和maven/settings.xml中server中的id一致，表示使用该id对应的用户名和密码-->
        <id>itheima-snapshot</id>
        <!--snapshot版本上传仓库的具体地址-->
        <url>http://localhost:8081/repository/itheima-snapshot/</url>
    </snapshotRepository>
</distributionManagement>
```

‍

#### 发布资源到私服

‍

Maven生命周期: deploy

或者执行Maven命令

```
mvn deploy
```

‍

**注意**

> 要发布的项目都需要配置`distributionManagement`​标签，要么在自己的pom.xml中配置，要么在其父项目中配置，然后子项目中继承父项目即可. 
>
> 现在发布是在itheima-snapshot仓库中，如果想发布到itheima-release仓库中就需要将项目pom.xml中的version修改成RELEASE即可.

‍

‍

#### 配置阿里云下载依赖

如果私服中没有对应的jar，会去中央仓库下载，速度很慢

设置->Repositories->maven-central->Remote storage 配置为阿里云中央仓库地址

​​

‍

‍

### 实现示例

‍

==三步配置，一条指令==

1. maven本体配置访问私服的用户名、密码
2. maven本体配置连接私服的地址(url地址)
3. pom.xml文件中配置上传资源的位置(url地址)
4. 上传资源到私服仓库，就执行deploy<sup>(maven生命周期)</sup>

‍

‍

#### **访问用户名/密码**

maven安装目录下的conf/settings.xml中的servers中

```xml
<server>
    <id>maven-releases</id>
    <username>admin</username>
    <password>admin</password>
</server>
  
<server>
    <id>maven-snapshots</id>
    <username>admin</username>
    <password>admin</password>
</server>
```

‍

#### **依赖下载的仓库组地址**

maven安装目录下的conf/settings.xml中的mirrors、profiles中

```xml
<mirror>
    <id>maven-public</id>
    <mirrorOf>*</mirrorOf>
    <url>http://192.168.150.101:8081/repository/maven-public/</url>
</mirror>
```

```xml
<profile>
    <id>allow-snapshots</id>
        <activation>
        	<activeByDefault>true</activeByDefault>
        </activation>
    <repositories>
        <repository>
            <id>maven-public</id>
            <url>http://192.168.150.101:8081/repository/maven-public/</url>
            <releases>
            	<enabled>true</enabled>
            </releases>
            <snapshots>
            	<enabled>true</enabled>
            </snapshots>
        </repository>
    </repositories>
</profile>
```

‍

#### **配置发布地址**

项目目录下的pom.xml

```xml
<distributionManagement>
    <!-- release版本的发布地址 -->
    <repository>
        <id>maven-releases</id>
        <url>http://192.168.150.101:8081/repository/maven-releases/</url>
    </repository>

    <!-- snapshot版本的发布地址 -->
    <snapshotRepository>
        <id>maven-snapshots</id>
        <url>http://192.168.150.101:8081/repository/maven-snapshots/</url>
    </snapshotRepository>
</distributionManagement>
```

‍

## POM

‍

POM( Project Object Model，项目对象模型 ) 是 Maven 工程的基本工作单元，是一个XML文件，包含了项目的基本信息，用于描述项目如何构建，声明项目依赖，等等。

‍

pom.xml 项目模型对象文件。POM 中可以指定以下配置：

* 坐标GAV、Name、pakage、author
* 项目依赖 dependencies/dependency
* 项目模块 modules/module
* 项目集成 parent
* 项目构建 build、plugins/plugin
* 配置文件 profiles/profile

‍

pom工程之间的关系

* 在dependency中的依赖关系
* 在parent中的继承关系
* 在module中的聚合关系

‍

‍

### 依赖关系

dependency

> 依赖的传递主要通过dependency标签实现的

‍

#### scope依赖范围标签

scope的取值用来定义依赖生效的空间（main目录、test目录）和时间（开发中、部署中）。

标签位置 dependencies/dependency/scope  
标签取值 compile/test/provided/system/runtime/import

* compile是最基本的标签，在所有的范围、所有的时间都能生效。
* test标签，仅仅在test目录开发阶段生效的依赖，生效空间和时间范围最小。
* prvoide标签，在开发阶段生效，在部署阶段不生效的依赖（因为部署环境提供了该依赖，不需要打包到jar或者war包中）
* import maven也是单继承。通过import导入多个依赖。导入的依赖必须是POM工程，package中的打包方式是pom

‍

##### system

强行将当前系统下的jar包作为依赖导入进来。使用系统根路径，可以是相对路径。

‍

##### runtime

编译时不需要，但是运行时需要的jar。实际运行时需要接口的实现类。JDBC接口标准提供了一系列借口，能够进行编译。但是运行时需要JDBC的具体实现。

‍

#### optional

可选依赖 {#optional可选依赖 data-source-line="41"}在开发阶段需要的类，但是在运行阶段可能不需要的类，就不需要再打包的时候导入到其中。

‍

‍

|值|解释|
| ----------| ---------------------------------------------------------------------------------------------------------------------------------------------|
|compile|​`默认的scope`​，表示 dependency 都可以在生命周期中使用。而且，这些dependencies 会传递到依赖的项目中。适用于所有阶段，会随着项目一起发布。|
|provided|跟compile相似，但是表明了dependency 由JDK或者容器提供，例如Servlet AP和一些Java EE APIs。这个scope 只能作用在编译和测试时，同时没有传递性。|
|runtime|表示dependency不作用在编译时，但会作用在运行和测试时，如JDBC驱动，适用运行和测试阶段。|
|test|表示dependency作用在测试时，不作用在运行时。 只在测试时使用，用于编译和运行测试代码。不会随项目发布。|
|​`system<span> </span>`​|与provided类似，但是它不会去maven仓库寻找依赖，而是在本地找；而`systemPath`​标签将提供本地路径|
|import|这个标签就是 引入该dependency的pom中定义的所有dependency定义|

‍

#### 依赖的传递性

能够通过依赖传递，导入大量相关的间接依赖。而不需要手动导入所有的依赖。

* A依赖B，B依赖C的时候，A不需要引入C即可使用C依赖。A会间接依赖C
* compile范围的依赖能够传递，test和provide范围的依赖无法传递。

‍

#### 依赖的排除

阻断依赖的传递性。多个间接依赖可能不兼容，需要排除不兼容的间接依赖。

‍

标签位置 dependency/exclusions/exclusion

```xml
<dependency>
    <exclusions>
        <exclusion>
            <groupId></groupId>
            <artifactId></artifactId>
        </exclusion>
    <exclusions>
</dependency>
```

‍

#### 版本仲裁

1. 最短路径优先
2. 路径相同先声明者优先。

‍

### 继承关系parent

> 依赖的继承主要是通过parent标签实现的。

‍

‍

#### 继承关系

Maven工程之间，A工程继承B工程

* B工程：父工程
* A工程：子工程

本质上是A工程的pom.xml中的配置继承了B工程中pom.xml的配置。

继承，也就是父工程，主要管理依赖信息的版本

‍

‍

#### 继承的作用

在付工程中统一管理项目中的依赖信息，管理以来信息的版本。

背景

* 一个大型的项目进行了模块拆分
* 一个project下面创建了很多module
* 每一个module都需要自己的依赖信息

需求

* 每一个module中各自维护各自的依赖信息很容易发生不一致的情况，不易统一管理
* 使用框架内的的不同的jar包，应该是同一个版本，所以整个项目使用的框架版本需要统一
* 使用框架时所需要的jar包组合，需要经过长期摸索和反复调试，最终确定一个可用的组合。

通过在父工程中为整个项目维护依赖信息的组合，既保证了整个项目的使用规范、准确的jar包；又能讲以往的经验沉淀下来，节约时间和精力。

‍

‍

#### 继承的使用

1. 创建父工程，修改工程的打包方式。只有打包方式为pom的maven工程能够管理其他的maven工程。**打包方式为pom**的maven工程总不写业务代码，是专门管理其他maven工程的工程。

```xml
<groupId>com.ykl</groupId>
<artifactId>project-maven</artifactId>
<version>1.0.2</version>
<packaging>pom</packaging>
```

2. 创建模块工程。idea中的module。从聚合分解的角度分析就是聚合和模块两种工程。而不是父子工程。
3. 模块间的依赖关系

    1. 在父工程中通过modules来引入子工程
    2. 在子工程中通过parent引入父工程

```xml
<parent>
    <groupId></groupId>
    <artifactId></artifactId>
    <version></version>
</parent>
```

4. 在付工程进行依赖信息的统一管理

```xml
<dependencyManagement>
    <depencies>
        <dependency>
        </dependency>
    </dependencies>
</dependencyManagement>
```

* 并不是在父工程管理了依赖，子工程具体使用哪个依赖，还要明确配置。仅仅是不需要配置版本号。
* 对于已经在付工程进行管理的依赖，子工程引用时可以不写version

  * 父子工程版本一致，可以省略了version标签
  * 父子工程版本冲突，子工程的版本覆盖了父工程中的版本。

‍

#### 继承中的属性

创建自定以的属性标签，标签名也就是属性名，标签纸也就是属性值。

引用方式 。通过属性名解析后才知道具体的版本号值。

```xml
<properties>
    <com.ykl.spring.version>4.1.0.RELEASE</com.ykl.spring.version>
</properties>

    ${com.ykl.spring.version}
```

‍

#### 继承内容

父POM中的大多数元素都能被子POM继承，这些元素包含：

```text
groupId
version
description
url
inceptionYear
organization
licenses
developers
contributors
mailingLists
scm
issueManagement
ciManagement
properties
dependencyManagement
dependencies
repositories
pluginRepositories
build
plugin executions with matching ids
plugin configuration
etc.
reporting
profiles
```

‍

### 聚合关系modules

‍

#### 聚合的含义

通过moudules标签聚合子项目。好处

1. 如果没有聚合标签，或者说聚合功能，则需要去每个模块下执行maven命令，并且按照依赖顺序从前到后执行。
2. 如果有聚合标签，可以一键执行maven命令。mvn 会按照模块之间的依赖关系依次执行命令。
3. 能够显示各个所有的模块。

```xml
  <modules>
    <module>project04-maven-moduele</module>
    <module>project05-maven-moduele</module>
    <module>project06-maven-moduele</module>
  </modules>
```

#### 循环依赖

* 防止循环依赖，会出现报错

```xml
the ... dependencies has cyclic dependency

```

‍

‍

### POM层次

‍

#### 基本概念

Maven不仅仅用来做依赖管理和项目构建。还用来做**项目管理**。

POM有四个层次：超级POM、父POM、当前POM、生效POM

‍

#### 超级pom

默认继承了超级POM。超级POM

* moduleversion版本信息
* repository 仓库的地址
* pluginrepostory 插件仓库的地址
* builid 主要设置各个目录的位置。
* 插件管理
* profiles 配置文件

‍

#### 父POM

> 当前的POM的parent

‍

#### 当前POM

> 下层POM会继承上层POM。最近的有效配置是下层pom

‍

#### 有效POM

> 最终生效的POM文件，将所有的父POM叠加到一起

```text
mvn help:effective-pom
```

‍

### POM标签大全

‍

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="http://maven.apache.org/POM/4.0.0http://maven.apache.org/maven-v4_0_0.xsd">
    <!--父项目的坐标。如果项目中没有规定某个元素的值，那么父项目中的对应值即为项目的默认值。 坐标包括group ID，artifact ID和 
        version。 -->
    <parent>
        <!--被继承的父项目的构件标识符 -->
        <artifactId />
        <!--被继承的父项目的全球唯一标识符 -->
        <groupId />
        <!--被继承的父项目的版本 -->
        <version />
        <!-- 父项目的pom.xml文件的相对路径。相对路径允许你选择一个不同的路径。默认值是../pom.xml。Maven首先在构建当前项目的地方寻找父项 
            目的pom，其次在文件系统的这个位置（relativePath位置），然后在本地仓库，最后在远程仓库寻找父项目的pom。 -->
        <relativePath />
    </parent>
    <!--声明项目描述符遵循哪一个POM模型版本。模型本身的版本很少改变，虽然如此，但它仍然是必不可少的，这是为了当Maven引入了新的特性或者其他模型变更的时候，确保稳定性。 -->
    <modelVersion>4.0.0</modelVersion>
    <!--项目的全球唯一标识符，通常使用全限定的包名区分该项目和其他项目。并且构建时生成的路径也是由此生成， 如com.mycompany.app生成的相对路径为：/com/mycompany/app -->
    <groupId>asia.banseon</groupId>
    <!-- 构件的标识符，它和group ID一起唯一标识一个构件。换句话说，你不能有两个不同的项目拥有同样的artifact ID和groupID；在某个 
        特定的group ID下，artifact ID也必须是唯一的。构件是项目产生的或使用的一个东西，Maven为项目产生的构件包括：JARs，源 码，二进制发布和WARs等。 -->
    <artifactId>banseon-maven2</artifactId>
    <!--项目产生的构件类型，例如jar、war、ear、pom。插件可以创建他们自己的构件类型，所以前面列的不是全部构件类型 -->
    <packaging>jar</packaging>
    <!--项目当前版本，格式为:主版本.次版本.增量版本-限定版本号 -->
    <version>1.0-SNAPSHOT</version>
    <!--项目的名称, Maven产生的文档用 -->
    <name>banseon-maven</name>
    <!--项目主页的URL, Maven产生的文档用 -->
    <url>http://www.baidu.com/banseon</url>
    <!-- 项目的详细描述, Maven 产生的文档用。 当这个元素能够用HTML格式描述时（例如，CDATA中的文本会被解析器忽略，就可以包含HTML标 
        签）， 不鼓励使用纯文本描述。如果你需要修改产生的web站点的索引页面，你应该修改你自己的索引页文件，而不是调整这里的文档。 -->
    <description>A maven project to study maven.</description>
    <!--描述了这个项目构建环境中的前提条件。 -->
    <prerequisites>
        <!--构建该项目或使用该插件所需要的Maven的最低版本 -->
        <maven />
    </prerequisites>
    <!--项目的问题管理系统(Bugzilla, Jira, Scarab,或任何你喜欢的问题管理系统)的名称和URL，本例为 jira -->
    <issueManagement>
        <!--问题管理系统（例如jira）的名字， -->
        <system>jira</system>
        <!--该项目使用的问题管理系统的URL -->
        <url>http://jira.baidu.com/banseon</url>
    </issueManagement>
    <!--项目持续集成信息 -->
    <ciManagement>
        <!--持续集成系统的名字，例如continuum -->
        <system />
        <!--该项目使用的持续集成系统的URL（如果持续集成系统有web接口的话）。 -->
        <url />
        <!--构建完成时，需要通知的开发者/用户的配置项。包括被通知者信息和通知条件（错误，失败，成功，警告） -->
        <notifiers>
            <!--配置一种方式，当构建中断时，以该方式通知用户/开发者 -->
            <notifier>
                <!--传送通知的途径 -->
                <type />
                <!--发生错误时是否通知 -->
                <sendOnError />
                <!--构建失败时是否通知 -->
                <sendOnFailure />
                <!--构建成功时是否通知 -->
                <sendOnSuccess />
                <!--发生警告时是否通知 -->
                <sendOnWarning />
                <!--不赞成使用。通知发送到哪里 -->
                <address />
                <!--扩展配置项 -->
                <configuration />
            </notifier>
        </notifiers>
    </ciManagement>
    <!--项目创建年份，4位数字。当产生版权信息时需要使用这个值。 -->
    <inceptionYear />
    <!--项目相关邮件列表信息 -->
    <mailingLists>
        <!--该元素描述了项目相关的所有邮件列表。自动产生的网站引用这些信息。 -->
        <mailingList>
            <!--邮件的名称 -->
            <name>Demo</name>
            <!--发送邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <post>banseon@126.com</post>
            <!--订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <subscribe>banseon@126.com</subscribe>
            <!--取消订阅邮件的地址或链接，如果是邮件地址，创建文档时，mailto: 链接会被自动创建 -->
            <unsubscribe>banseon@126.com</unsubscribe>
            <!--你可以浏览邮件信息的URL -->
            <archive>http:/hi.baidu.com/banseon/demo/dev/</archive>
        </mailingList>
    </mailingLists>
    <!--项目开发者列表 -->
    <developers>
        <!--某个项目开发者的信息 -->
        <developer>
            <!--SCM里项目开发者的唯一标识符 -->
            <id>HELLO WORLD</id>
            <!--项目开发者的全名 -->
            <name>banseon</name>
            <!--项目开发者的email -->
            <email>banseon@126.com</email>
            <!--项目开发者的主页的URL -->
            <url />
            <!--项目开发者在项目中扮演的角色，角色元素描述了各种角色 -->
            <roles>
                <role>Project Manager</role>
                <role>Architect</role>
            </roles>
            <!--项目开发者所属组织 -->
            <organization>demo</organization>
            <!--项目开发者所属组织的URL -->
            <organizationUrl>http://hi.baidu.com/banseon</organizationUrl>
            <!--项目开发者属性，如即时消息如何处理等 -->
            <properties>
                <dept>No</dept>
            </properties>
            <!--项目开发者所在时区， -11到12范围内的整数。 -->
            <timezone>-5</timezone>
        </developer>
    </developers>
    <!--项目的其他贡献者列表 -->
    <contributors>
        <!--项目的其他贡献者。参见developers/developer元素 -->
        <contributor>
            <name />
            <email />
            <url />
            <organization />
            <organizationUrl />
            <roles />
            <timezone />
            <properties />
        </contributor>
    </contributors>
    <!--该元素描述了项目所有License列表。 应该只列出该项目的license列表，不要列出依赖项目的 license列表。如果列出多个license，用户可以选择它们中的一个而不是接受所有license。 -->
    <licenses>
        <!--描述了项目的license，用于生成项目的web站点的license页面，其他一些报表和validation也会用到该元素。 -->
        <license>
            <!--license用于法律上的名称 -->
            <name>Apache 2</name>
            <!--官方的license正文页面的URL -->
            <url>http://www.baidu.com/banseon/LICENSE-2.0.txt</url>
            <!--项目分发的主要方式： repo，可以从Maven库下载 manual， 用户必须手动下载和安装依赖 -->
            <distribution>repo</distribution>
            <!--关于license的补充信息 -->
            <comments>A business-friendly OSS license</comments>
        </license>
    </licenses>
    <!--SCM(Source Control Management)标签允许你配置你的代码库，供Maven web站点和其它插件使用。 -->
    <scm>
        <!--SCM的URL,该URL描述了版本库和如何连接到版本库。欲知详情，请看SCMs提供的URL格式和列表。该连接只读。 -->
        <connection>
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/banseon-maven2-trunk(dao-trunk)
        </connection>
        <!--给开发者使用的，类似connection元素。即该连接不仅仅只读 -->
        <developerConnection>
            scm:svn:http://svn.baidu.com/banseon/maven/banseon/dao-trunk
        </developerConnection>
        <!--当前代码的标签，在开发阶段默认为HEAD -->
        <tag />
        <!--指向项目的可浏览SCM库（例如ViewVC或者Fisheye）的URL。 -->
        <url>http://svn.baidu.com/banseon</url>
    </scm>
    <!--描述项目所属组织的各种属性。Maven产生的文档用 -->
    <organization>
        <!--组织的全名 -->
        <name>demo</name>
        <!--组织主页的URL -->
        <url>http://www.baidu.com/banseon</url>
    </organization>
    <!--构建项目需要的信息 -->
    <build>
        <!--该元素设置了项目源码目录，当构建项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <sourceDirectory />
        <!--该元素设置了项目脚本源码目录，该目录和源码目录不同：绝大多数情况下，该目录下的内容 会被拷贝到输出目录(因为脚本是被解释的，而不是被编译的)。 -->
        <scriptSourceDirectory />
        <!--该元素设置了项目单元测试使用的源码目录，当测试项目的时候，构建系统会编译目录里的源码。该路径是相对于pom.xml的相对路径。 -->
        <testSourceDirectory />
        <!--被编译过的应用程序class文件存放的目录。 -->
        <outputDirectory />
        <!--被编译过的测试class文件存放的目录。 -->
        <testOutputDirectory />
        <!--使用来自该项目的一系列构建扩展 -->
        <extensions>
            <!--描述使用到的构建扩展。 -->
            <extension>
                <!--构建扩展的groupId -->
                <groupId />
                <!--构建扩展的artifactId -->
                <artifactId />
                <!--构建扩展的版本 -->
                <version />
            </extension>
        </extensions>
        <!--当项目没有规定目标（Maven2 叫做阶段）时的默认值 -->
        <defaultGoal />
        <!--这个元素描述了项目相关的所有资源路径列表，例如和项目相关的属性文件，这些资源被包含在最终的打包文件里。 -->
        <resources>
            <!--这个元素描述了项目相关或测试相关的所有资源路径 -->
            <resource>
                <!-- 描述了资源的目标路径。该路径相对target/classes目录（例如${project.build.outputDirectory}）。举个例 
                    子，如果你想资源在特定的包里(org.apache.maven.messages)，你就必须该元素设置为org/apache/maven /messages。然而，如果你只是想把资源放到源码目录结构里，就不需要该配置。 -->
                <targetPath />
                <!--是否使用参数值代替参数名。参数值取自properties元素或者文件里配置的属性，文件在filters元素里列出。 -->
                <filtering />
                <!--描述存放资源的目录，该路径相对POM路径 -->
                <directory />
                <!--包含的模式列表，例如**/*.xml. -->
                <includes />
                <!--排除的模式列表，例如**/*.xml -->
                <excludes />
            </resource>
        </resources>
        <!--这个元素描述了单元测试相关的所有资源路径，例如和单元测试相关的属性文件。 -->
        <testResources>
            <!--这个元素描述了测试相关的所有资源路径，参见build/resources/resource元素的说明 -->
            <testResource>
                <targetPath />
                <filtering />
                <directory />
                <includes />
                <excludes />
            </testResource>
        </testResources>
        <!--构建产生的所有文件存放的目录 -->
        <directory />
        <!--产生的构件的文件名，默认值是${artifactId}-${version}。 -->
        <finalName />
        <!--当filtering开关打开时，使用到的过滤器属性文件列表 -->
        <filters />
        <!--子项目可以引用的默认插件信息。该插件配置项直到被引用时才会被解析或绑定到生命周期。给定插件的任何本地配置都会覆盖这里的配置 -->
        <pluginManagement>
            <!--使用的插件列表 。 -->
            <plugins>
                <!--plugin元素包含描述插件所需要的信息。 -->
                <plugin>
                    <!--插件在仓库里的group ID -->
                    <groupId />
                    <!--插件在仓库里的artifact ID -->
                    <artifactId />
                    <!--被使用的插件的版本（或版本范围） -->
                    <version />
                    <!--是否从该插件下载Maven扩展（例如打包和类型处理器），由于性能原因，只有在真需要下载时，该元素才被设置成enabled。 -->
                    <extensions />
                    <!--在构建生命周期中执行一组目标的配置。每个目标可能有不同的配置。 -->
                    <executions>
                        <!--execution元素包含了插件执行需要的信息 -->
                        <execution>
                            <!--执行目标的标识符，用于标识构建过程中的目标，或者匹配继承过程中需要合并的执行目标 -->
                            <id />
                            <!--绑定了目标的构建生命周期阶段，如果省略，目标会被绑定到源数据里配置的默认阶段 -->
                            <phase />
                            <!--配置的执行目标 -->
                            <goals />
                            <!--配置是否被传播到子POM -->
                            <inherited />
                            <!--作为DOM对象的配置 -->
                            <configuration />
                        </execution>
                    </executions>
                    <!--项目引入插件所需要的额外依赖 -->
                    <dependencies>
                        <!--参见dependencies/dependency元素 -->
                        <dependency>
                            ......
                        </dependency>
                    </dependencies>
                    <!--任何配置是否被传播到子项目 -->
                    <inherited />
                    <!--作为DOM对象的配置 -->
                    <configuration />
                </plugin>
            </plugins>
        </pluginManagement>
        <!--使用的插件列表 -->
        <plugins>
            <!--参见build/pluginManagement/plugins/plugin元素 -->
            <plugin>
                <groupId />
                <artifactId />
                <version />
                <extensions />
                <executions>
                    <execution>
                        <id />
                        <phase />
                        <goals />
                        <inherited />
                        <configuration />
                    </execution>
                </executions>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
                <goals />
                <inherited />
                <configuration />
            </plugin>
        </plugins>
    </build>
    <!--在列的项目构建profile，如果被激活，会修改构建处理 -->
    <profiles>
        <!--根据环境参数或命令行参数激活某个构建处理 -->
        <profile>
            <!--构建配置的唯一标识符。即用于命令行激活，也用于在继承时合并具有相同标识符的profile。 -->
            <id />
            <!--自动触发profile的条件逻辑。Activation是profile的开启钥匙。profile的力量来自于它 能够在某些特定的环境中自动使用某些特定的值；这些环境通过activation元素指定。activation元素并不是激活profile的唯一方式。 -->
            <activation>
                <!--profile默认是否激活的标志 -->
                <activeByDefault />
                <!--当匹配的jdk被检测到，profile被激活。例如，1.4激活JDK1.4，1.4.0_2，而!1.4激活所有版本不是以1.4开头的JDK。 -->
                <jdk />
                <!--当匹配的操作系统属性被检测到，profile被激活。os元素可以定义一些操作系统相关的属性。 -->
                <os>
                    <!--激活profile的操作系统的名字 -->
                    <name>Windows XP</name>
                    <!--激活profile的操作系统所属家族(如 'windows') -->
                    <family>Windows</family>
                    <!--激活profile的操作系统体系结构 -->
                    <arch>x86</arch>
                    <!--激活profile的操作系统版本 -->
                    <version>5.1.2600</version>
                </os>
                <!--如果Maven检测到某一个属性（其值可以在POM中通过${名称}引用），其拥有对应的名称和值，Profile就会被激活。如果值 字段是空的，那么存在属性名称字段就会激活profile，否则按区分大小写方式匹配属性值字段 -->
                <property>
                    <!--激活profile的属性的名称 -->
                    <name>mavenVersion</name>
                    <!--激活profile的属性的值 -->
                    <value>2.0.3</value>
                </property>
                <!--提供一个文件名，通过检测该文件的存在或不存在来激活profile。missing检查文件是否存在，如果不存在则激活 profile。另一方面，exists则会检查文件是否存在，如果存在则激活profile。 -->
                <file>
                    <!--如果指定的文件存在，则激活profile。 -->
                    <exists>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/
                    </exists>
                    <!--如果指定的文件不存在，则激活profile。 -->
                    <missing>/usr/local/hudson/hudson-home/jobs/maven-guide-zh-to-production/workspace/
                    </missing>
                </file>
            </activation>
            <!--构建项目所需要的信息。参见build元素 -->
            <build>
                <defaultGoal />
                <resources>
                    <resource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </resource>
                </resources>
                <testResources>
                    <testResource>
                        <targetPath />
                        <filtering />
                        <directory />
                        <includes />
                        <excludes />
                    </testResource>
                </testResources>
                <directory />
                <finalName />
                <filters />
                <pluginManagement>
                    <plugins>
                        <!--参见build/pluginManagement/plugins/plugin元素 -->
                        <plugin>
                            <groupId />
                            <artifactId />
                            <version />
                            <extensions />
                            <executions>
                                <execution>
                                    <id />
                                    <phase />
                                    <goals />
                                    <inherited />
                                    <configuration />
                                </execution>
                            </executions>
                            <dependencies>
                                <!--参见dependencies/dependency元素 -->
                                <dependency>
                                    ......
                                </dependency>
                            </dependencies>
                            <goals />
                            <inherited />
                            <configuration />
                        </plugin>
                    </plugins>
                </pluginManagement>
                <plugins>
                    <!--参见build/pluginManagement/plugins/plugin元素 -->
                    <plugin>
                        <groupId />
                        <artifactId />
                        <version />
                        <extensions />
                        <executions>
                            <execution>
                                <id />
                                <phase />
                                <goals />
                                <inherited />
                                <configuration />
                            </execution>
                        </executions>
                        <dependencies>
                            <!--参见dependencies/dependency元素 -->
                            <dependency>
                                ......
                            </dependency>
                        </dependencies>
                        <goals />
                        <inherited />
                        <configuration />
                    </plugin>
                </plugins>
            </build>
            <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
            <modules />
            <!--发现依赖和扩展的远程仓库列表。 -->
            <repositories>
                <!--参见repositories/repository元素 -->
                <repository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </repository>
            </repositories>
            <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
            <pluginRepositories>
                <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
                <pluginRepository>
                    <releases>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </releases>
                    <snapshots>
                        <enabled />
                        <updatePolicy />
                        <checksumPolicy />
                    </snapshots>
                    <id />
                    <name />
                    <url />
                    <layout />
                </pluginRepository>
            </pluginRepositories>
            <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
            <dependencies>
                <!--参见dependencies/dependency元素 -->
                <dependency>
                    ......
                </dependency>
            </dependencies>
            <!--不赞成使用. 现在Maven忽略该元素. -->
            <reports />
            <!--该元素包括使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。参见reporting元素 -->
            <reporting>
                ......
            </reporting>
            <!--参见dependencyManagement元素 -->
            <dependencyManagement>
                <dependencies>
                    <!--参见dependencies/dependency元素 -->
                    <dependency>
                        ......
                    </dependency>
                </dependencies>
            </dependencyManagement>
            <!--参见distributionManagement元素 -->
            <distributionManagement>
                ......
            </distributionManagement>
            <!--参见properties元素 -->
            <properties />
        </profile>
    </profiles>
    <!--模块（有时称作子项目） 被构建成项目的一部分。列出的每个模块元素是指向该模块的目录的相对路径 -->
    <modules />
    <!--发现依赖和扩展的远程仓库列表。 -->
    <repositories>
        <!--包含需要连接到远程仓库的信息 -->
        <repository>
            <!--如何处理远程仓库里发布版本的下载 -->
            <releases>
                <!--true或者false表示该仓库是否为下载某种类型构件（发布版，快照版）开启。 -->
                <enabled />
                <!--该元素指定更新发生的频率。Maven会比较本地POM和远程POM的时间戳。这里的选项是：always（一直），daily（默认，每日），interval：X（这里X是以分钟为单位的时间间隔），或者never（从不）。 -->
                <updatePolicy />
                <!--当Maven验证构件校验文件失败时该怎么做：ignore（忽略），fail（失败），或者warn（警告）。 -->
                <checksumPolicy />
            </releases>
            <!-- 如何处理远程仓库里快照版本的下载。有了releases和snapshots这两组配置，POM就可以在每个单独的仓库中，为每种类型的构件采取不同的 
                策略。例如，可能有人会决定只为开发目的开启对快照版本下载的支持。参见repositories/repository/releases元素 -->
            <snapshots>
                <enabled />
                <updatePolicy />
                <checksumPolicy />
            </snapshots>
            <!--远程仓库唯一标识符。可以用来匹配在settings.xml文件里配置的远程仓库 -->
            <id>banseon-repository-proxy</id>
            <!--远程仓库名称 -->
            <name>banseon-repository-proxy</name>
            <!--远程仓库URL，按protocol://hostname/path形式 -->
            <url>http://192.168.1.169:9999/repository/</url>
            <!-- 用于定位和排序构件的仓库布局类型-可以是default（默认）或者legacy（遗留）。Maven 2为其仓库提供了一个默认的布局；然 
                而，Maven 1.x有一种不同的布局。我们可以使用该元素指定布局是default（默认）还是legacy（遗留）。 -->
            <layout>default</layout>
        </repository>
    </repositories>
    <!--发现插件的远程仓库列表，这些插件用于构建和报表 -->
    <pluginRepositories>
        <!--包含需要连接到远程插件仓库的信息.参见repositories/repository元素 -->
        <pluginRepository>
            ......
        </pluginRepository>
    </pluginRepositories>
 
 
    <!--该元素描述了项目相关的所有依赖。 这些依赖组成了项目构建过程中的一个个环节。它们自动从项目定义的仓库中下载。要获取更多信息，请看项目依赖机制。 -->
    <dependencies>
        <dependency>
            <!--依赖的group ID -->
            <groupId>org.apache.maven</groupId>
            <!--依赖的artifact ID -->
            <artifactId>maven-artifact</artifactId>
            <!--依赖的版本号。 在Maven 2里, 也可以配置成版本号的范围。 -->
            <version>3.8.1</version>
            <!-- 依赖类型，默认类型是jar。它通常表示依赖的文件的扩展名，但也有例外。一个类型可以被映射成另外一个扩展名或分类器。类型经常和使用的打包方式对应， 
                尽管这也有例外。一些类型的例子：jar，war，ejb-client和test-jar。如果设置extensions为 true，就可以在 plugin里定义新的类型。所以前面的类型的例子不完整。 -->
            <type>jar</type>
            <!-- 依赖的分类器。分类器可以区分属于同一个POM，但不同构建方式的构件。分类器名被附加到文件名的版本号后面。例如，如果你想要构建两个单独的构件成 
                JAR，一个使用Java 1.4编译器，另一个使用Java 6编译器，你就可以使用分类器来生成两个单独的JAR构件。 -->
            <classifier></classifier>
            <!--依赖范围。在项目发布过程中，帮助决定哪些构件被包括进来。欲知详情请参考依赖机制。 - compile ：默认范围，用于编译 - provided：类似于编译，但支持你期待jdk或者容器提供，类似于classpath 
                - runtime: 在执行时需要使用 - test: 用于test任务时使用 - system: 需要外在提供相应的元素。通过systemPath来取得 
                - systemPath: 仅用于范围为system。提供相应的路径 - optional: 当项目自身被依赖时，标注依赖是否传递。用于连续依赖时使用 -->
            <scope>test</scope>
            <!--仅供system范围使用。注意，不鼓励使用这个元素，并且在新的版本中该元素可能被覆盖掉。该元素为依赖规定了文件系统上的路径。需要绝对路径而不是相对路径。推荐使用属性匹配绝对路径，例如${java.home}。 -->
            <systemPath></systemPath>
            <!--当计算传递依赖时， 从依赖构件列表里，列出被排除的依赖构件集。即告诉maven你只依赖指定的项目，不依赖项目的依赖。此元素主要用于解决版本冲突问题 -->
            <exclusions>
                <exclusion>
                    <artifactId>spring-core</artifactId>
                    <groupId>org.springframework</groupId>
                </exclusion>
            </exclusions>
            <!--可选依赖，如果你在项目B中把C依赖声明为可选，你就需要在依赖于B的项目（例如项目A）中显式的引用对C的依赖。可选依赖阻断依赖的传递性。 -->
            <optional>true</optional>
        </dependency>
    </dependencies>
    <!--不赞成使用. 现在Maven忽略该元素. -->
    <reports></reports>
    <!--该元素描述使用报表插件产生报表的规范。当用户执行"mvn site"，这些报表就会运行。 在页面导航栏能看到所有报表的链接。 -->
    <reporting>
        <!--true，则，网站不包括默认的报表。这包括"项目信息"菜单中的报表。 -->
        <excludeDefaults />
        <!--所有产生的报表存放到哪里。默认值是${project.build.directory}/site。 -->
        <outputDirectory />
        <!--使用的报表插件和他们的配置。 -->
        <plugins>
            <!--plugin元素包含描述报表插件需要的信息 -->
            <plugin>
                <!--报表插件在仓库里的group ID -->
                <groupId />
                <!--报表插件在仓库里的artifact ID -->
                <artifactId />
                <!--被使用的报表插件的版本（或版本范围） -->
                <version />
                <!--任何配置是否被传播到子项目 -->
                <inherited />
                <!--报表插件的配置 -->
                <configuration />
                <!--一组报表的多重规范，每个规范可能有不同的配置。一个规范（报表集）对应一个执行目标 。例如，有1，2，3，4，5，6，7，8，9个报表。1，2，5构成A报表集，对应一个执行目标。2，5，8构成B报表集，对应另一个执行目标 -->
                <reportSets>
                    <!--表示报表的一个集合，以及产生该集合的配置 -->
                    <reportSet>
                        <!--报表集合的唯一标识符，POM继承时用到 -->
                        <id />
                        <!--产生报表集合时，被使用的报表的配置 -->
                        <configuration />
                        <!--配置是否被继承到子POMs -->
                        <inherited />
                        <!--这个集合里使用到哪些报表 -->
                        <reports />
                    </reportSet>
                </reportSets>
            </plugin>
        </plugins>
    </reporting>
    <!-- 继承自该项目的所有子项目的默认依赖信息。这部分的依赖信息不会被立即解析,而是当子项目声明一个依赖（必须描述group ID和 artifact 
        ID信息），如果group ID和artifact ID以外的一些信息没有描述，则通过group ID和artifact ID 匹配到这里的依赖，并使用这里的依赖信息。 -->
    <dependencyManagement>
        <dependencies>
            <!--参见dependencies/dependency元素 -->
            <dependency>
                ......
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!--项目分发信息，在执行mvn deploy后表示要发布的位置。有了这些信息就可以把网站部署到远程服务器或者把构件部署到远程仓库。 -->
    <distributionManagement>
        <!--部署项目产生的构件到远程仓库需要的信息 -->
        <repository>
            <!--是分配给快照一个唯一的版本号（由时间戳和构建流水号）？还是每次都使用相同的版本号？参见repositories/repository元素 -->
            <uniqueVersion />
            <id>banseon-maven2</id>
            <name>banseon maven2</name>
            <url>file://${basedir}/target/deploy</url>
            <layout />
        </repository>
        <!--构件的快照部署到哪里？如果没有配置该元素，默认部署到repository元素配置的仓库，参见distributionManagement/repository元素 -->
        <snapshotRepository>
            <uniqueVersion />
            <id>banseon-maven2</id>
            <name>Banseon-maven2 Snapshot Repository</name>
            <url>scp://svn.baidu.com/banseon:/usr/local/maven-snapshot</url>
            <layout />
        </snapshotRepository>
        <!--部署项目的网站需要的信息 -->
        <site>
            <!--部署位置的唯一标识符，用来匹配站点和settings.xml文件里的配置 -->
            <id>banseon-site</id>
            <!--部署位置的名称 -->
            <name>business api website</name>
            <!--部署位置的URL，按protocol://hostname/path形式 -->
            <url>
                scp://svn.baidu.com/banseon:/var/www/localhost/banseon-web
            </url>
        </site>
        <!--项目下载页面的URL。如果没有该元素，用户应该参考主页。使用该元素的原因是：帮助定位那些不在仓库里的构件（由于license限制）。 -->
        <downloadUrl />
        <!--如果构件有了新的group ID和artifact ID（构件移到了新的位置），这里列出构件的重定位信息。 -->
        <relocation>
            <!--构件新的group ID -->
            <groupId />
            <!--构件新的artifact ID -->
            <artifactId />
            <!--构件新的版本号 -->
            <version />
            <!--显示给用户的，关于移动的额外信息，例如原因。 -->
            <message />
        </relocation>
        <!-- 给出该构件在远程仓库的状态。不得在本地项目中设置该元素，因为这是工具自动更新的。有效的值有：none（默认），converted（仓库管理员从 
            Maven 1 POM转换过来），partner（直接从伙伴Maven 2仓库同步过来），deployed（从Maven 2实例部 署），verified（被核实时正确的和最终的）。 -->
        <status />
    </distributionManagement>
    <!--以值替代名称，Properties可以在整个POM中使用，也可以作为触发条件（见settings.xml配置文件里activation元素的说明）。格式是<name>value</name>。 -->
    <properties />
</project>
```

‍

‍

‍

‍

## settings.xml

‍

maven的两大配置文件：settings.xml和pom.xml。其中settings.xml是maven的全局配置文件，pom.xml则是文件所在项目的局部配置。

‍

‍

‍

# 实操

‍

## Bugfix

‍

‍

### 依赖配置

‍

‍

#### Maven依赖项获取源代码失败

报错

```java
Failed to get sources. Instead, stub sources have been generated by the disassembler.
Implementation of methods is unavailable.
```

‍

仅当前项目：在当前项目的pom.xml的properties标签中添加

downloadSources, 允许下载源

```xml
<downloadSources>true</downloadSources>
```

‍

全局：在maven的settings.xml的profiles标签中添加

```xml
<profile>
    <id>downloadSources</id>
    <properties>
        <downloadSources>true</downloadSources>
        <downloadJavadocs>true</downloadJavadocs>
    </properties>
</profile>
```

‍

‍

#### SQLserver驱动找不到

[CSDN](https://blog.csdn.net/uhb6577/article/details/85331611)  [CSDN2](https://blog.csdn.net/qq_42704130/article/details/120956900)

```java
Missing artifact com.microsoft.sqlserver:sqljdbc4:jar:4.0
```

‍

原因

直接    指定路径下确实没有sqljdbc4.jar文件

根本    微软不允许以maven的方式直接下载该文件. 

‍

‍

##### 下载相关库

1. 官网下载  
    [Microsoft SQL Server JDBC 驱动程序](https://www.microsoft.com/zh-CN/download/details.aspx?id=11774)
2. Maven仓库下载  
    [Home » com.microsoft.sqlserver » sqljdbc4 » 4.0](https://mvnrepository.com/artifact/com.microsoft.sqlserver/sqljdbc4/4.0)

进入下载库所在的目录

​`mvn install:install-file -Dfile=<jar包的绝对路径> -DgroupId=<groupId名> -DartifactId=<artifactId名> -Dversion=<jar版本> -Dpackaging=<文件打包方式>`​

```java
mvn install:install-file -Dfile=sqljdbc4-4.0.jar -DgroupId=com.microsoft.sqlserver -DartifactId=sqljdbc4 -Dversion=4.0 -Dpackaging=jar
```

‍

### 打包问题

‍

#### 相关的文件被忽略

pom.xml

包含资源

```xml
<build>
    <resources>
        <resource>
            <directory>src/main/java</directory>
            <includes>
                <include>**/*.xml</include>
            </includes>
            <filtering>false</filtering>
        </resource>
    </resources>
</build>
```

‍

#### 打包为war报错

```java
在项目web-demo上执行目标org.apache.maven.plugins:maven-war:2.2:war失败:无法加载插件org.apache.maven.plugins:maven-war:2.2中的mojo 'war'，因为API不兼容:org.codehaus.plexus.component.repository. exception.com componentlookupexception:无法访问属性的默认字段
```

因为插件版本与API不兼容导致的错误

在pom.xml文件的 `<project> </project>`​ 中添加如下插件

```xml
<build>
    <!-- To define the plugin version in your parent POM -->
    <pluginManagement>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-war-plugin</artifactId>
                <version>3.3.1</version>
            </plugin>
        </plugins>
    </pluginManagement>
    <!-- To use the plugin goals in your POM or parent POM -->
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-war-plugin</artifactId>
            <version>3.3.1</version>
        </plugin>
    </plugins>
</build>
```

‍

‍
