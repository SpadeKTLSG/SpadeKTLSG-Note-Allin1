--后端Web服务器

‍

‍

Tomcat服务器软件是一个免费开源的轻量级web应用服务器. 因为Tomcat支持Servlet/JSP规范，所以Tomcat也被称为Web容器、Servlet容器[官网](https://tomcat.apache.org/)

Tomcat内部提供了最为核心的WEB容器实现，开发者可以将自己所编写的“.java *”、“* .jsp”程序直接放到WEB容器后就会自动的由WEB容器加载并执行，同时利用WEB容器可以非常方便的使用JavaEE中的各种服务（例如：JDBC服务连接SQL数据库）

‍

### Header

‍

‍

# 知识

‍

‍

‍

‍

# 基础

‍

‍

## 搭建

‍

#### 后端本地部署

‍

* 准备静态资源
* 将静态资源部署到Web服务器上

  * 放到Tomcat的webapps目录内,启动服务器修改地址栏为对应路径即可
* 启动Web服务器使用浏览器访问对应的资源

  * http://127.0.0.1:8080

‍

‍

## 目录

‍

Tomcat下子目录结构

|No.|目录名称|作用描述|
| -----| ----------| ------------------------------------------------------------------------|
|1|bin|保存所有的二进制可执行程序文件，例如：“startup.bat”表示启动Tomcat|
|2|conf|保存所有的配置文件，有两个重要的配置文件“server.xml”、“web.xml”|
|3|lib|保存项目运行所需要的“*.jar”文件，理解为一个Tomcat内部支持的CLASSPATH|
|4|logs|保存所有的日志数据文件|
|5|webapps|进行项目热部署目录，可以直接配置发布项目|
|6|work|临时的工作目录，保存所有编译后的“*.class”文件|

‍

‍

## ​交互

‍

### 访问

‍

‍

如果需要进行本机WEB服务访问，那么直接输入“http://localhost:8080”即可，而如果是当前运行的Tomcat是一台**分配了IP的公网服务器**，则远程用户直接输入服务器的IP地址（http://ip地址:8080）也可以访问当前的WEB服务

‍

#### 个性化访问配置

不希望每一次都输入localhost，还是希望可以输入一些域名的方式进行访问，也可以考虑在本机的操作系统中做出一个模拟，修改Windows 系统中的hosts文件，文件路径:` C\Windows\System32\drivers\etc\hosts`​

```xml
127.0.0.1 tomcat_server
```

‍

## ​控制

‍

Tomcat核心配置server.xml文件, 修改即可改变其启动页, 可以配置主机的名称,端口号等

‍

#### 关闭

正常关闭：bin\shutdown.bat   或   在Tomcat启动窗口中按下 Ctrl+C

‍

‍

#### 端口

‍

conf/server.xml

```html
   <Connector port="8080" protocol="HTTP/1.1"
   connectionTimeout="20000"
   redirectPort="8443"
   maxParameterCount="1000"
   />
```

‍

> 注: HTTP协议默认端口号为80，如果将Tomcat端口号改为80，则将来访问Tomcat时，将不用输入端口号. 但是这样就相当是把前端的nginx服务器的位子占用了, 非常不建议(除非是特殊服务器

‍

‍

‍

‍

## 内存策略

‍

Tomcat是基于JDK上的一种应用服务，如果没有JDK支持，Tomcat是无法正常启动的，但是需要注意的是，所有的Java程序在运行的时候都会启动有一个Java进程，但是这个进程如果要想正常执行，则一定要为其分配相应的内存

‍

### 观察范例

观察默认的内存分配

```html

<body>
<% //此处用于编写Java程序代码

out.println("<h1>MAX_MEMORY : " + Runtime.getRuntime().maxMemory() + "</h1>");

out.println("<h1>TOTAL_MEMORY : " + Runtime.getRuntime().totalMemory() + "</h1>");

%>

</body>
```

浏览器观察执行结果

```html
MAX_MEMORY : 4238344192
TOTAL_MEMORY : 50331648
```

这个是字节, 除以三次1024得到有几个G

‍

> 在Java 基础课程讲解的时候已经重点分析过了默认情况下的内存分配问题，在整个的JVM之中存在有一个伸缩区的概念，  
> 默认情况下会为每一个JVM分配本机物理的内存“1/4”(MaxMemory)，而同时初始化的内存为本机物理内存的“1/64"(TotalMemory)，这两个内存大小之间的数值就是可伸缩的范围，这个范围的伸缩是需要不断的判断才可以实现正确分配的.

‍

在Tomcat运行期间，所有的程序都是基于JVM进程运行的，而在默认情况下系统会为JVM进程最多分配物理内存的“1/4”，初始化的内存大小为“1/64”，每个线程的内存大小为“1M”，这样就有可能引发程序执行的性能问题，那么在使用前就必须对Tomcat内存参数进行调整

‍

这样的分配会存在有如下两个问题:

* 如果你是一台专属的WEB服务器，发现所有的资源并没有全部分配给Tomcat，会产生严重的性能浪费;
* 如果现在保留的伸缩区过大，那么在高并发的状态下，就有可能出现分配不及时所造成的的内存溢出问题. ·

所以在整个的实际项目运行之中，如果不进行合理的内存调整，最终就有可能造成服务的不稳定因素，如果要想进行调整肯定是要调整JVM参数

‍

### 调整JVM参数

‍

BIN目录找到 catalina.bat, 用编辑器打开

跳过前面几百行的注释, 直接在顶部setlocal语句往上的空间追加配置项

‍

‍

‍

配置

```xml
set "JAVA_OPTS=%JAVA_OPTS% -Xmx10g -Xms10g -Xss256k -Xlog:gc*"
```

‍

%JAVA_OPTS%    原始配置不可忽略

-Xmx10g    设置Tomcat的**最大可用内存**，本次设置的大小为10G

-Xms10g    设置Tomcat的**初始化内存**，初始化内存一般与最大内存相同，此处为10G

-Xss256k    配置**每个线程的内存**大小为256K

-Xlog:gc\*    开启打印GC处理日志

‍

‍

执行后刷新页面变化

‍

```html
MAX_MEMORY : 10737418240
TOTAL_MEMORY : 10737418240
```

‍

‍

## SB整合

‍

‍

SpringBoot中，引入的web运行环境(starter-web起步依赖)已经集成了内置的Tomcat服务器

‍

SpringBoot进行web程序开发时内置了一个核心的Servlet程序` DispatcherServlet`​，称之为 核心控制器(我称之为CPU). 它负责接收页面发送的请求，然后根据规则将请求再转发给后面的请求处理器Controller，请求处理器处理完请求之后，最终再由`DispatcherServlet`​给浏览器响应数据

请求到达tomcat之后tomcat会负责解析这些请求数据，将解析后的请求数据传递给Servlet程序的HttpServletRequest对象，还给Servlet程序传递了一个参数 HttpServletResponse，通过这个对象，我们就可以给浏览器设置响应数据

‍

‍

‍

SpringBoot中配置依赖

‍

* spring-boot-starter-web：包含了web应用开发所需要的常见依赖
* spring-boot-starter-test：包含了单元测试所需要的常见依赖

‍

‍

在IDEA之中除了可以方便的进行WEB 项目的创建之外，也可以直接配置Tomcat，直接在IDEA工具的内部整合，但是这就要求开发者一定要首先在电脑上已经进行了Tomcat安装(这个安装仅仅是做为一个解压缩的操作形式出现)

需要注意的是，在IDEA 中配置的Tomcat实际上并不是原始的Tomcat，仅仅是将存在的Tomcat的相关配置拷贝到IDEA工作目录之中，包括:端口、服务项等信息. 

‍

‍

在进行Tomcat配置的时候实际上是存在有Tomcat应用环境的，但是这个应用环境之中**不包含有模块的部署**，例如:在当前的系统之中存在有一个yootk模块，这个模块要想使用，就需要在Tomcat进行部署的配置

‍

启动配置第二栏导入工件的问题->导入项目(部署-来自工件)之后即可在浏览器访问本地资源配置

需要注意的是，在进行Tomcat部署的时候(旧IDEA)一般默认将项目名称作为虚拟目录的名称出现，这样的做法实际上是不符合正常使用要求的，所以一定要进行手工的修改. 

‍

‍

但是需要说明的是，这个时候的配置并不是WebApp应用的配置，仅仅是一个工具中的配置.   
不同的工具导入了相同的项目，那么这个配置就会失效了，所以如果假设你确定需要设置一个首页，则可以修改“web  
WEB-INF/web.xml”配置文件，在这个配置文件里面直接添加一个欢迎页的列表项

```html
    <welcome-file-list>
        <welcome-file>hello.jsp</welcome-file>
        <welcome-file>muyan.jsp</welcome-file>
        <welcome-file>yootk.html</welcome-file>
    </welcome-file-list>
```

在使用的时候会按照顺序进行查找，同时如果没有配置的话，则会找到“index.html" 、  
"index.jsp”之类的以“index”开头的程序作为欢迎页

此时对于工具中的Tomcat也需要将其访问的默认路径设置为根路径, 才能更好观察结果

‍

这个在Tomcat配置: conf->web.xml 找`<welcome-file-list>`​

‍

从实际的开发来讲，一般不建议修改默认的首页配置，因为index形式已经几乎都是深入人心,相当于只要访问了WEBApp  
的应用目录都会使用index.html静态页面作为首页进行展示. 

‍

‍

## 编码

‍

### 乱码处理

‍

Tomcat接受浏览器请求，在处理数据时产生乱码，原因是：tomcat不知道浏览器发来数据的编码格式，此时tomcat会使用默认的ISO8859-1去解析，导致乱码. 

‍

#### get请求

对于get请求提交的数据，在不同版本的tomcat中有不同的处理方式，在tomcat8及以上的版本，服务器默认以utf-8的编码方式处理请求参数，这一点可以从tomcat8官方文档中可以看出.

‍

而对于tomcat8以下的版本，服务器会默认以ISO8859-1的编码方式处理请求参数，在tomcat7官方文档中可以看出, 如果浏览器页面是utf-8编码，就不用设置get请求的请求参数编码处理方式，服务器默认就会用utf-8的方式处理请求参数，而在tomcat8以下的tomcat服务器中，需要我们手动编解码处理请求参数，如下所示. 

```java
String username = request.getParameter("username");
username = new String(username.getBytes("iso8859-1"),"utf-8");
```

或者在tomcat配置文件（server.xml）加上如下配置

```xml
<Connector connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443" URIEncoding="UTF-8" useBodyEncodingForURI = "true"/>
```

1. URIEncoding：这个参数用来针对url传参方式，也就是get请求的编码类型的设定
2. useBodyEncodingForURI：一个boolean类型的参数，用来决定url传参方式的编码是否与请求体编码一致，

‍

#### post请求

在获取请求参数之前设置

```
request.setCharacterEncoding("utf-8");
```

‍

#### 响应

‍

ServletResponse有两个方法用于设置响应的编码格式

```java
setContentType("text/html;charset=UTF-8");
setCharacterEncoding("UTF-8");
```

这两个方法其实有两个作用

‍

1. 向客户端浏览器回响应时，会添加一个content-type头，并指定编码类型为UTF-8，让浏览器知道字节数据的解码类型并正确显示.
2. 当通过ServletResponse.getWriter()获取Writer，并通过其写入的所有字符串都会默认以UTF-8进行编码为字节输出到浏览器.

当然如果你是通过ServletResponse.getOutputStream()进行直接写入时，tomcat不做任何处理，直接输出到浏览器. 

‍

‍

‍

# 高级

‍

## 虚拟目录

‍

Tomcat启动之后用户就可以直接通过浏览器进行访问，所有的程序一定要整合到Tomcat之中才可以正常执行，Tomcat为了便于这样的整合操作提供了虚拟目录的配置，每一个虚拟目录都是一套独立且完整的WEB项目. 

‍

在一个Tomcat之中实际上可以同时运行多个WEB应用，每一个WEB应用都属于独立的个体(这些WEB应用彼此之间是无法互相直接进行访问的，必须通过某些的协议要求才可以实现访问)，那么对于这样的独立应用，可以直接在Tomcat中进行配置，而这样的独立应用在Tomcat中的配置就称为虚拟目录，**对于虚拟目录的配置在Tomcat中提供有两种模式**. 

‍

### Tomcat应用所在的磁盘

‍

在“$(TOMCAT_HOME}/webapps”目录中可以直接进行虚拟目录的创建，但是如果要想创建，应该按照如下的目录的结构进行存储

```xml
WEB根目录
   |-WEB-INF

     |-web.xml    WEB配置的部署描述符
```

‍

对于“WEB-INF/web.xml”文件信息可以直接通过ROOT目录进行拷贝获得

> 例如:本次直接在“${TOMCAT_HOME/webapps”目录之中创建一个“muyan”的目录，随后将以上的“WEB-INF/web.xml”配置拷贝到此目录之中，这个时候该目录对应的浏览器上的访问路径就是“/muyan”.

‍

实际上这个时候并不是没有配置成功，而是说从Tomcat 5.5版本开始为了WEB 项目的部署安全，将文件列表功能直接关闭了，如果要想打开这样的列表功能，需要修改“${TOMCAT_HOME}/conf/web.xml”配置文件，修改“listings”配置项

‍

‍

进入web.xml搜索listings

找到配置文件地址

```xml
 <servlet>
        <servlet-name>default</servlet-name>
        <servlet-class>org.apache.catalina.servlets.DefaultServlet</servlet-class>
        <init-param>
            <param-name>debug</param-name>
            <param-value>0</param-value>
        </init-param>
        <init-param>
            <param-name>listings</param-name>
            <param-value>false</param-value>
        </init-param>
        <load-on-startup>1</load-on-startup>
    </servlet>
```

这个false改成true

‍

之后外置Tomcat启动startup, 然后直接访问对应目录位置`http://localhost:8080/CWX/`​

这个CWX在`Tomcat\webapps`​中, 并且拷贝了根目录的WEB-INF文件夹.

‍

‍

### 指定的磁盘中创建专属的工作目录

> 这个工作目录也必须是符合于JavaEE要求的目录结构

‍

（必须要有WEB-INF,必须要有web.xml配置文件)

但是这个文件目录是一个独立于磁盘上的，如果要想在Tomcat中生效，那么就需要修改Tomcat中的server.xml配置文件，在这个配置文件里面，进行路径的匹配. 

特别需要注意的是:每一个不同的WEB应用都有其各自的访问路径，这个访问路径的定义不允许重复，如果现在访问路径出现了重复，最终的情况就是Tomcat无法正常启动. 

‍

核心:WEB-INF

‍

创建一个前端项目包, 包含WEB-INF

配置在最底端\</Host>标签处添加<context path>

‍

原来的

```xml
    <Engine name="Catalina" defaultHost="localhost">

      <!--For clustering, please take a look at documentation at:
          /docs/cluster-howto.html  (simple how to)
          /docs/config/cluster.html (reference documentation) -->
      <!--
      <Cluster className="org.apache.catalina.ha.tcp.SimpleTcpCluster"/>
      -->

      <!-- Use the LockOutRealm to prevent attempts to guess user passwords
           via a brute-force attack -->
      <Realm className="org.apache.catalina.realm.LockOutRealm">
        <!-- This Realm uses the UserDatabase configured in the global JNDI
             resources under the key "UserDatabase".  Any edits
             that are performed against this UserDatabase are immediately
             available for use by the Realm.  -->
        <Realm className="org.apache.catalina.realm.UserDatabaseRealm"
               resourceName="UserDatabase"/>
      </Realm>

      <Host name="localhost"  appBase="webapps"
            unpackWARs="true" autoDeploy="true">

        <!-- SingleSignOn valve, share authentication between web applications
             Documentation at: /docs/config/valve.html -->
        <!--
        <Valve className="org.apache.catalina.authenticator.SingleSignOn" />
        -->

        <!-- Access log processes all example.
             Documentation at: /docs/config/valve.html
             Note: The pattern used is equivalent to using pattern="common" -->
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />
        <   ->这里    />
      </Host>
    </Engine>
  </Service>
</Server>
```

‍

添加

```xml
        <Valve className="org.apache.catalina.valves.AccessLogValve" directory="logs"
               prefix="localhost_access_log" suffix=".txt"
               pattern="%h %l %u %t &quot;%r&quot; %s %b" />

        <Context path="/SpadeK" docBase ="D:\Workshop\Codes\Backup\TEST\SpadeK"/>


      </Host>
    </Engine>
  </Service>
</Server>
```

​`<Context path="/SpadeK" docBase ="D:\Workshop\Codes\Backup\TEST\SpadeK"/>`​

‍

访问对应端口`path`​目录得到结果

http://localhost:8080/SpadeK/hello.html

‍

‍

如果不想每次都写/SpadeK虚拟路径, 就把它写为/, 这样后面就自动忽略直接写资源即可了

‍

‍

同样需要调整Web.xml中的listings为true

‍

‍

#### 注意

当删除了配置的虚拟路径的文件之后, Tomcat便会崩溃了, 查看日志发现报错

```xml
Caused by: java.lang.IllegalArgumentException: 指定的主资源集 [D:\Workshop\Codes\Backup\TEST\SpadeK] 无效
```

因此还是保留着罢, 暂时

‍

‍

‍

‍

‍

# 实操

‍

## Bugfix

‍

‍

### **中文乱码**

Tomcat8.5以后的版本已经处理了中文乱码的问题，但是IDEA中的Tomcat插件有的只到Tomcat7:

插件的Configuration中加上`<uriEncoding>UTF-8</uriEncoding><!--访问路径编解码字符集-->`​设置即可

```java
<build>
    <plugins>
      <plugin>
        <groupId>org.apache.tomcat.maven</groupId>
        <artifactId>tomcat7-maven-plugin</artifactId>
        <version>2.1</version>
        <configuration>
          <port>80</port><!--tomcat端口号-->
          <path>/</path> <!--虚拟目录-->
          <uriEncoding>UTF-8</uriEncoding><!--访问路径编解码字符集-->
        </configuration>
      </plugin>
    </plugins>
  </build>
```

‍

### 引用库

servlet-api可以直接从本地Tomcat的lib中导入, 加到全局库中

‍
