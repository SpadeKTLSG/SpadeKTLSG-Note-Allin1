--动态Web技术

‍

Servlet是Java实现的CGI技术（Common Gateway Interface，公共网关接口），是Java为实现动态WEB开发在较早时期推出一项技术

Servlet（Server Applet）, 一种使用 Java 语言来开发动态网站的技术，它是运行在 Web 服务器或应用服务器上的程序，可以接收和处理来自客户端的请求，生成动态的 Web 内容

‍

Servlet其实就是一个遵循Servlet开发的java类。Servlet是由服务器调用的，运行在服务器端。

* Java基本程序，依赖jdk和jre，就能在虚拟机上运行
* servlet是一种特殊的Java类，遵循新的标准和规范，需要web容器才能提供web服务（没有主类，根据映射选择性执行的Java程序），例如tomcat。相当于为了扩展java无法作为动态语言提供web服务的，建立了新的java技术标准，但是需要额外的运行环境支撑。web容器就是servlet的运行环境。

‍

‍

### Header

‍

‍

‍

# 知识

‍

## 概念

‍

‍

‍

### 定义

‍

#### servlet

想开发servlet程序，只需要

1. 编写一个类，实现Servlet接口
2. 把开发好的Java类部署到web服务器中

把实现了Servlet接口的Java程序叫做Servlet

‍

* 简介：是JavaServlet的简称，用Java编写的运行在Web服务器或应用服务器上的程序,具有独立于平台和协议的特性, 主要功能在于交互式地浏览和生成动态Web内容
* 作用：接收用户通过浏览器传来的表单数据，或者读取数据库信息返回给浏览器查看，创建动态网页
* 接口路径：package javax.servlet

  * 有两个常见的子类：HttpServlet、GenericServlet

‍

#### 路径映射

‍

Servlet访问URL使用路径映射（注意：一定要加 / 开头）

url-pattern：以”/’开头,可以用 /xxx/yy 来区分模块，* 是通配符，最好用模块区分，防止通配符都映射成但不同优先级导致问题

‍

### 功能

Java Servlet 是运行在 Web 服务器或应用服务器上的程序，它是作为来自 Web 浏览器或其他 HTTP 客户端的请求和 HTTP 服务器上的数据库或应用程序之间的中间层. 

使用 Servlet可以收集来自网页表单的用户输入，呈现来自数据库或者其他源的记录，还可以动态创建网页

‍

‍

Servlet 执行以下主要任务

* 读取客户端（浏览器）发送的显式的数据. 这包括网页上的 HTML 表单，或者也可以是来自 applet 或自定义的 HTTP 客户端程序的表单.
* 读取客户端（浏览器）发送的隐式的 HTTP 请求数据. 这包括 cookies、媒体类型和浏览器能理解的压缩格式等等.
* 处理数据并生成结果. 这个过程可能需要访问数据库，执行 RMI 或 CORBA 调用，调用 Web 服务，或者直接计算得出对应的响应.
* 发送显式的数据（即文档）到客户端（浏览器）. 该文档的格式可以是多种多样的，包括文本文件（HTML 或 XML）、二进制文件（GIF 图像）、Excel 等.
* 发送隐式的 HTTP 响应到客户端（浏览器）. 这包括告诉浏览器或其他客户端被返回的文档类型（例如 HTML），设置 cookies 和缓存参数，以及其他类似的任务.

‍

## 评价

‍

### 问题

早期的Servlet技术由于需要实现动态WEB的响应，所以要通过Servlet实现页面的最终显示，这样在程序中就会存在有大量的IO输出操作，如下所示：

```java
out.println("") ;
out.println(" ... ") ;
out.println("") ;
```

‍

**页面模版**

虽然通过这样的技术可以实现最终的HTML页面响应，但是一定会带来程序代码的维护困难，所以早期的开发者会考虑定义一个 HTML显示模版文件，而后利用IO流加载模版文件，最终完成页面展示

‍

**JSP产生**

Servlet技术受到了微软ASP技术的启发，将Servlet改进，推出了JSP技术，但是这并不表示Servlet技术已经被JSP所取代，两者之间在开发中有很强的互补性，因为Servlet使用纯Java编写，所以更加适合于编写Java程序代码，而JSP将更加适合于动态页面的展示. 

‍

### JSP

‍

* 什么是JSP: ；

  * 使用JSP标签在HTML网页中插入Java相关代码，标签通常以<%开头 以%>结束
  * JSP本身就是一种Servlet, JSP在第一次被访问的时候会被编译为HttpJspPage类,是HttpServlet的一个子类
  * 为什么用这个：和原生Servle 相比JSP可以很方便的编写HTML网页而不用去大量的用println语句输出html代码
  * 通俗来说：jsp就是在html里面写java代码，servlet就是在java里面写html代码
* 添加jsp-api.jar到项目里面，和添加servlet-api.jar一样的步骤
* JSP内置了9个对象可以直接用（先简单知道就行）：out、session、response、request、config、page、application、pageContext、exception

‍

```java

request  HttpServletRequest类的实例

response HttpServletResponse类的实例

out PrintWriter类的实例，用于把结果输出至网页上

session HttpSession类的实例

application ServletContext类的实例，与应用上下文有关

config  ServletConfig类的实例

pageContext PageContext类的实例，提供对JSP页面所有对象以及命名空间的访问

page    Java类中的this关键字

Exception   Exception类的对象，代表发生错误的JSP页面中对应的异常对象
```

‍

* JSP脚本程序

<% 代码片段 %>

```java
<%
out.println("IP address is " + request.getRemoteAddr());
%>
```

‍

* JSP表达式的语法格式：（不能用分号结束）

<%= 表达式 %>

​`<%=request.getRequestURL()%>`​

‍

* 中文编码问题，顶部添加这些信息(部分同学)

​`<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>`​

‍

* JSP的现状：2015年之前很公司使用，过后互联网发展很块，各类分布式技术架构，前端框架、后端框架大量出现，性能和便利性比JSP强很多，

所以基本很少企业使用JSP了，但是这个是学javaweb里面基础知识，大家可以简单学，不用花特别多时间(学校或者其他老旧的书本会花很多时间讲这个)

```java
<%@ page language="java" contentType="text/html; charset=UTF-8" pageEncoding="UTF-8"%>

<html>

 <head>

   <title>小滴课堂</title>

 </head>

 <body>

<h4>

 <%=request.getRequestURL()%>

</h4>

</html>

```

‍

# 基础

‍

‍

## 结构

‍

### 目录

所有的 HTML 文件都位于顶级目录 *servlet* 下

```
/servlet
    /images
    /WEB-INF
        /classes
        /lib
        /web.xml
```

web.xml    应用程序的部署描述符

classes    目录包含了所有的 Servlet 类和其他类文件

‍

‍

类文件所在的目录结构与它们的包名称匹配

一个完全合格的类名称 **cn.twle.demo.HelloServlet** ，那么这个 Servlet 类必须位于以下目录中

```
servlet/WEB-INF/classes/cn/twle/demo/HelloServlet.class
```

‍

## 搭建

‍

### 基础示例1

最最基础的JavaWeb配置

‍

创建一整个空模块控制Servlet所有项目, 之后通过java和Web框架创建一个新项目, 之后在Web里面创建index.html

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>

<h1>导航页面</h1>
<br>
<a href="/index/helloServlet">进入servlet</a>
<!--/index是项目根目录,后面是Servlet标识-->

</body>
</html>
```

配置Tomcat服务器, 运行菜单栏中的运行配置(注意: 不是修改运行选项, 是全局的运行配置) 到部署一栏创建工件, 并把应用程序上下文改为对准为Html文档里对应的根目录

‍

项目导入 servlet-api.jar 文件

修改web.xml文件, 指定链接

```html
    <servlet>
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>HelloServlet</servlet-class>
    </servlet>

    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/HelloServlet</url-pattern>
    </servlet-mapping>
```

‍

修改Java文件HelloServlet.java, 继承Servlet, 重写方法中的service, 添加页面响应语句

```java
        servletResponse.setContentType("text/html;charset=utf-8");
        PrintWriter out = servletResponse.getWriter();
        out.println("这是我们的Servlet页面，Hello！！！");  
```

‍

### 基础示例2

‍

‍

响应请求

```java
// 所有的Servlet类中一定会存在有强制性的父类的继承要求，因为Servlet在90年代末的出现
// 当时的开发技术还没有现在的形式多样化，所以对于Servlet就有了这种强制性继承
// 因为现阶段的面向对象程序的设计理念是不要求存在有强制性的继承关系的
public class HelloServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 在doGet()方法中已经明确的给出了HttpServletRequest、HttpServletResponse接口实例
        // 这个接口实例是在每一次进行请求处理的时候，由容器负责提供的
        // 在Java中提供了两个打印流：PrintStream、PrintWriter，HTML实际上是文本，所以字符流合适
        PrintWriter out = resp.getWriter(); // 获得了客户端浏览器的输出流
        out.println("<html>");
        out.println("<head>");
        out.println("   <title>Yootk - JavaWEB</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("   <h1>www.yootk.com</h1>");
        out.println("</body>");
        out.println("</html>");
        out.close(); // 关闭输出流
    }
}
```

在整个的Servlet中对于响应流的获取只能够获取唯一的一次，即“resp.getwriter()”代码只能够使用一次

‍

‍

所有的Servlet定义完成后都需要保存在WEB-INF/classes目录之中，这样才能够被WEB容器所加载，让其可以进行用户的请求处理，则必须修改web.xml配置文件，追加Servlet的映射访问路径

‍

映射访问路径

```html
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>   <!-- 定义一个Servlet程序配置类 -->
        <servlet-name>HelloServlet</servlet-name>
        <servlet-class>com.yootk.servlet.HelloServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>HelloServlet</servlet-name>
        <url-pattern>/hello.action</url-pattern> <!-- 映射路径必须使用“/”开头 -->
    </servlet-mapping>
</web-app>
```

多个映射路径的使用，与Servlet之间的关联是通过“<servlet-name>”配置名称来进行捆绑的

‍

> 通过当前所给出的程序结构可以感受到: 将JSP原始的处理操作全部都转为了Servlet编程, 之前每一个处理的时候都会有一个表单提交页，那么如果要是有了Servlet, 实际上就将这个表单的提交处理程序由JSP转为了Servlet，可以确定的一点是:JSP能够完成的功能Servlet都是可以完成的.

‍

## **生命周期**

Servlet 生命周期可被定义为从创建直到毁灭的整个过程. 一次创建，到处服务, 一个Servlet只会有一个对象，服务所有的请求.

‍

Servlet 遵循的过程

‍

* 实例化（使用构造方法创建对象）
* 初始化  执行init方法  

  它是在服务器装入 Servlet 时执行的,即第一次访问这个Servlet才执行
* 服务  执行service方法

  service() 方法是 Servlet 的核心. 每当一个客户请求一个HttpServlet 对象，该对象的service() 方法就要被调用
* 销毁  执行destroy方法

‍

第一个到达服务器的 HTTP 请求被委派到 Servlet 容器. 

Servlet 容器在调用 service() 方法之前加载 Servlet. 

然后 Servlet 容器处理由多个线程产生的多个请求，每个线程执行一个单一的 Servlet 实例的 service() 方法. 

最后，Servlet 是由 JVM 的垃圾回收器进行垃圾回收的

‍

### 基础

在JakartaEE标准中为了便于用户进行Servlet生命周期的控制，专门提供了一系列的生命周期控制方法，开发者只需要在子类中覆写表所列出的方法即可实现Servlet基础生命周期控制. 

|No.|方法|类型|所属类型|描述|
| -----| ---------------------------------------------------------------------------------------------------------| ------| ------------------| -----------------|
|1|public void init() throws  ServletException|方法|GenericServlet类|Servlet初始化|
|2|protected void doGet(HttpServletRequestreq,HttpServletResponseresp) throwsServletException,IOException|方法|HttpServlet类|HTTP服务处理|
|3|protected void doPost(HttpServletRequestreq,HttpServletResponseresp) throwsServletException,IOException|方法|HttpServlet类|HTTP服务处理|
|4|public void destroy()|方法|Servlet接口|Servlet服务销毁|

‍

```java
@WebServlet("/life")
public class LifeCycleServlet extends HttpServlet {
    public LifeCycleServlet() {
        System.out.println("【LifeCycleServlet.()】构造方法。");
    }
    @Override
    public void init() throws ServletException {
        System.out.println("【LifeCycleServlet.init()】Servlet初始化。");
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【LifeCycleServlet.doGet()】处理GET请求。");
    }
    @Override   // 进行GET请求的处理
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【LifeCycleServlet.doPost()】处理POST请求。");
    }

    @Override
    public void destroy() {
        System.out.println("【LifeCycleServlet.destroy()】Servlet销毁。");
    }
}

```

‍

**执行操作**

在第一次访问此Servlet映射路径时，会由WEB容器自动调用无参构造方法实例化指定本类对象，随后会通过init()方法进行Servlet初始化控制，最后在进行请求处理，如图所示. 而如果现在用户再次发出请求，由于该Servlet对象已经存在，所以直接调用服务方法进行请求处理即可，最后在容器关闭或者该Servlet长期不使用时才会调用destroy()方法进行资源释放

‍

‍

​

‍

#### init()

‍

被设计成只调用一次. 它在第一次创建 Servlet 时被调用，在后续每次用户请求时不再调用. 因此，它是用于一次性初始化，就像 Applet 的 init 方法一样. 

Servlet 创建于用户第一次调用对应于该 Servlet 的 URL 时，但是您也可以指定 Servlet 在服务器第一次启动时被加载. 

当用户调用一个 Servlet 时，就会创建一个 Servlet 实例，每一个用户请求都会产生一个新的线程，适当的时候移交给 doGet 或 doPost 方法. init() 方法简单地创建或加载一些数据，这些数据将被用于 Servlet 的整个生命周期. 

‍

```java
public void init() throws ServletException {
  // 初始化代码...
}
```

‍

‍

‍

#### doGet()

‍

GET 请求来自于一个 URL 的正常请求，或者来自于一个未指定 METHOD 的 HTML 表单，它由 doGet() 方法处理. 

```
public void doGet(HttpServletRequest request,
                  HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

‍

‍

#### doPost()

‍

POST 请求来自于一个特别指定了 METHOD 为 POST 的 HTML 表单，它由 doPost() 方法处理. 

```
public void doPost(HttpServletRequest request,
                   HttpServletResponse response)
    throws ServletException, IOException {
    // Servlet 代码
}
```

‍

‍

#### destroy()

‍

只会被调用一次，在 Servlet 生命周期结束时被调用. destroy() 方法可以让您的 Servlet 关闭数据库连接、停止后台线程、把 Cookie 列表或点击计数器写入到磁盘，并执行其他类似的清理活动. 

在调用 destroy() 方法之后，servlet 对象被标记为垃圾回收. destroy 方法定义如下所示：

```
  public void destroy() {
    // 终止化代码...
  }
```

‍

### 扩展

|No.|方法|类型|所属类型|描述|
| -----| ----------------------------------------------------------------------------------------------------------------| ------| ----------------| ---------------|
|1|public void init(ServletConfigconfig) throwsServletException|方法|Servlet接口|Servlet初始化|
|2|public void service(ServletRequestreq,ServletResponseres) throwsServletException,IOException|方法|Servlet接口|服务请求处理|
|3|protected void service(HttpServletRequest req, HttpServletResponse resp) throws  ServletException, IOException|方法|HttpServlet 类|HTTP请求处理|

‍

```java
@WebServlet(value="/life", loadOnStartup = 1, initParams = {
        @WebInitParam(name = "message", value = "www.yootk.com")
})
public class LifeCycleServlet extends HttpServlet {
    public LifeCycleServlet() {
        System.out.println("【LifeCycleServlet.()】构造方法。");
    }
    @Override
    public void init() throws ServletException {
        System.out.println("【LifeCycleServlet.init()】Servlet初始化。");
    }

    @Override
    public void init(ServletConfig config) throws ServletException {
        // 此时要通过Servlet的配置获取指定名称的初始化参数内容
        System.out.println("【LifeCycleServlet.init(ServletConfig config)】Servlet初始化，message = " + config.getInitParameter("message"));
    }

    @Override
    protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【LifeCycleServlet.service()】处理用户HTTP请求。");
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【LifeCycleServlet.doGet()】处理GET请求。");
    }
    @Override   // 进行POST请求的处理
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【LifeCycleServlet.doPost()】处理POST请求。");
    }

    @Override
    public void destroy() {
        System.out.println("【LifeCycleServlet.destroy()】Servlet销毁。");
    }
}
```

‍

#### service()

‍

执行实际任务的主要方法. Servlet 容器（即 Web 服务器）调用 service() 方法来处理来自客户端（浏览器）的请求，并把格式化的响应写回给客户端. 

每次服务器接收到一个 Servlet 请求时，服务器会产生一个新的线程并调用服务. service() 方法检查 HTTP 请求类型（GET、POST、PUT、DELETE 等），并在适当的时候调用 doGet、doPost、doPut，doDelete 等方法. 

service() 方法由容器调用，service 方法在适当的时候调用 doGet、doPost、doPut、doDelete 等方法. 所以，您不用对 service() 方法做任何动作，您只需要根据来自客户端的请求类型来重写 doGet() 或 doPost() 即可. 

‍

```java
public void service(ServletRequest request, 
                    ServletResponse response) 
      throws ServletException, IOException{
}
```

‍

‍

## 表单

‍

创建基础表单input.html

```html
<body>
<form action="input.action" method="post">
    请输入信息：<input type="text" name="message" value="沐言科技：www.yootk.com">
    <button type="submit">提交</button>
</form>
</body>
```

动态WEB请求中最重要的是与访问用户的交互性，而交互性的基本的体现形式就是HTML表单，而表单中常用的提交模式为POST，这样就必须在处理的Servlet类中明确的进行doPost()方法覆写. 

‍

‍

### 路径匹配

在进行表单提交时需要注意请求处理的Servlet映射路径要与HTML页面路径相匹配，例如：此时的input.html页面储存在了“pages/front/param”父路径之中，为了便于访问一般都会将Servlet也定义在同样的父路径下

针对访问路径问题, 最佳的解决方案就是让处理的Servlet与表单保持在同样的一个路径下

‍

随后需要创建一个新的Servlet，名称为“InputServlet”，需要注意的是，如果要配置新的Servlet，一定要在 web.xml配置文件中追加访问路径，而这个访问路径是绝对不能够重复的（如果你的访问路径重复了，则Tomcat启动之后将自动进行关闭)

‍

表单处理类Servlet

```java
public class InputServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8"); // 最早是写在JSP页面的
        resp.setContentType("text/html;charset=UTF-8"); // 如果不设置就出现显示乱码
        String msg = req.getParameter("message"); // 接收请求参数
        PrintWriter out = resp.getWriter(); // 获得了客户端浏览器的输出流
        out.println("<html>");
        out.println("<head>");
        out.println("   <title>Yootk - JavaWEB</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("   <h1>" + msg + "</h1>"); // 输出请求参数
        out.println("</body>");
        out.println("</html>");
        out.close(); // 关闭输出流
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp); // 调用doGet()处理
    }
}
```

‍

```html
    <servlet>   <!-- 定义一个Servlet程序配置类 -->
        <servlet-name>InputServlet</servlet-name>
        <servlet-class>com.yootk.servlet.InputServlet</servlet-class>
    </servlet>
    <servlet-mapping>
        <servlet-name>InputServlet</servlet-name>
        <url-pattern>/pages/message/input.action</url-pattern> <!-- 映射路径必须使用“/”开头 -->
    </servlet-mapping>
```

‍

## 响应

‍

doGet() 方法处理 HTTP GET 请求，使用 doPost() 方法处理 HTTP POST 请求

doHead、doDelete等，一样的都是根据http提交Method来区分

‍

### 基础实现响应

‍

#### **实现 Servlet**

‍

最初级的方法, 需要实现接口里的全部方法

```java
public class ServletDemo1 implements Servlet {

     //生命周期方法:当Servlet第一次被创建对象时执行该方法,该方法在整个生命周期中只执行一次
    public void init(ServletConfig arg0) throws ServletException {
                System.out.println("=======init=========");
        }

    //生命周期方法:对客户端响应的方法,该方法会被执行多次，每次请求该servlet都会执行该方法
    public void service(ServletRequest arg0, ServletResponse arg1)
            throws ServletException, IOException {
        System.out.println("hehe");

    }

    //生命周期方法:当Servlet被销毁时执行该方法
    public void destroy() {
        System.out.println("******destroy**********");
    }
//当停止tomcat时也就销毁的servlet。
    public ServletConfig getServletConfig() {

        return null;
    }

    public String getServletInfo() {

        return null;
    }
}
```

‍

#### **继承 GenericServlet**

极少用

```java
public class ServletDemo2 extends GenericServlet {

    @Override
    public void service(ServletRequest arg0, ServletResponse arg1)
            throws ServletException, IOException {
        System.out.println("heihei");

    }
}
```

‍

#### **继承 HttpServlet**

==经常用==

```java
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        System.out.println("haha");
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp)
            throws ServletException, IOException {
        System.out.println("ee");
        doGet(req,resp);
    }

```

‍

#### 示例

‍

##### 初始简单响应

```java
@WebServlet("/FormServlet")
public class FormServlet extends HttpServlet {
    //响应Get和Post请求

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");

        PrintWriter out = response.getWriter();
        String title = "使用 GET 方法读取表单数据 | 简单教程(www.twle.cn)";

        String docType = "<!DOCTYPE html>";
        out.println(docType +
                "<title>" + title + "</title>" +
                "<p>" + title + "</p>" +
                "<ul>" +
                "  <li><b>站点名</b>："
                + request.getParameter("name") + "\n" +
                "  <li><b>网址</b>："
                + request.getParameter("url") + "\n" +
                "</ul>");
    }
  
    //http://localhost:8080/index/FormServlet?name="简单编程"&url="www.twle.cn"

    // 处理 POST 方法请求的方法
    public void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException
    {
        doGet(request, response);
    }
  
    //http://localhost:8080/index/FormServlet?name="简单编程"&url="www.twle.cn"
  
  
}

```

‍

##### 获得表单参数

‍

**getParameterNames()**  该方法返回一个枚举，其中包含未指定顺序的参数名

然后我们就可以以标准方式循环枚举，使用 *hasMoreElements()*  方法来确定何时停止，使用 *nextElement()*  方法来获取每个参数的名称

```java
@WebServlet(name = "ReadAllParamsServlet", urlPatterns = {"read_all_params"}, loadOnStartup = 1)
public class ReadAllParamsServlet extends HttpServlet
{

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");
        PrintWriter out = response.getWriter();
        String title = "读取所有的表单数据 | 简单教程(www.twle.cn)";
        String docType =
                "<!doctype html>";
        out.println(docType +
                "<meta charset=\"utf-8\"><title>" + title + "</title>" +
                "<body bgcolor=\"#f0f0f0\">\n" +
                "<p>" + title + "</p>\n" +
                "<table width=\"100%\" border=\"1\">\n" +
                "<tr bgcolor=\"#949494\">\n" +
                "<th>参数名称</th><th>参数值</th>\n"+
                "</tr>\n");

        Enumeration<String> paramNames = request.getParameterNames();

        while(paramNames.hasMoreElements()) {
            String paramName = (String)paramNames.nextElement();
            out.print("<tr><td>" + paramName + "</td>\n");
            String[] paramValues =
                    request.getParameterValues(paramName);
            // 读取单个值的数据
            if (paramValues.length == 1) {
                String paramValue = paramValues[0];
                if (paramValue.length() == 0)
                    out.println("<td><i>没有值</i></td>");
                else
                    out.println("<td>" + paramValue + "</td>");
            } else {
                // 读取多个值的数据
                out.println("<td><ul>");
                for (String paramValue : paramValues) {
                    out.println("<li>" + paramValue);
                }
                out.println("</ul></td>");
            }
            out.print("</tr>");
        }
        out.println("\n</table>\n</body>");
    }

    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        // TODO Auto-generated method stub
        doGet(request, response);
    }

}
```

### doGet

http用get方式提交的请求，普通的查询就会进入到此方法

> 页面和已编码的信息中间用 ? 字符分隔
>
> ```
> https://www.twle.cn/hello?name=twle&greet=nice to meet you
> ```
>
> GET 方法是默认的从浏览器向 Web 服务器传递信息的方法，它会产生一个很长的字符串，出现在浏览器的地址栏中
>
> 如果要向服务器传递的是密码或其他的敏感信息，请不要使用 GET 方法(明文字符串)
>
> GET 方法有大小限制：请求字符串中最多只能有 1024 个字符
>
> 这些信息使用 QUERY_STRING 头传递，并可以通过 QUERY_STRING 环境变量访问

Servlet 使用 doGet() 方法处理这种类型的请求

‍

‍

‍

### doPost

‍

post

> POST 方法打包信息的方式与 GET 方法基本相同，但是 POST 方法不是把信息作为 URL 中 ? 字符后的文本字符串进行发送，而是把这些信息作为一个单独的消息
>
> 消息以标准输出的形式传到后台程序，我们可以解析和使用这些标准输出

Servlet 使用 doPost() 方法处理这种类型的请求

‍

### 处理

‍

Servlet 处理表单数据，这些数据会根据不同的情况使用不同的方法自动解析

‍

#### **getParameter()**

调用 request.getParameter() 方法来获取表单参数的值

‍

#### **getParameterValues()**

如果参数出现一次以上，则调用该方法，并返回多个值，例如复选框

‍

#### **getParameterNames()**

如果想要得到当前请求中的所有参数的完整列表，则调用该方法

‍

‍

## 状态码

‍

Java Servlet HttpServletResponse 类中的以下方法可以设置 HTTP 状态码

‍

### **setStatus ( int statusCode )**

设置一个任意的状态码. setStatus 方法接受一个 int（状态码）作为参数. 如果您的反应包含了一个特殊的状态码和文档，请确保在使用PrintWriter实际返回任何内容之前调用 setStatus

‍

### sendRedirect(String url)

生成一个 302 响应，连同一个带有新文档 URL 的 Location 头

‍

### **sendError(int code, String message)**

发送一个状态码（通常为 404），连同一个在 HTML 文档内部自动格式化并发送到客户端的短消息

‍

实例

‍

发送 407 错误码 和 "Need authentication!!!" 错误消息

```java
@WebServlet(name = "ShowErrorServlet", urlPatterns = {"show_error"})
public class ShowErrorServlet extends HttpServlet
{

    public void doGet(HttpServletRequest request,
                    HttpServletResponse response)
            throws ServletException, IOException
    {
        // 设置错误代码和原因
        response.sendError(403, "www.twle.cn Need authentication!!!" );

        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");
    }
}
```

‍

‍

## 注解开发

‍

‍

传统的Servlet程序定义之后往往都需要通过web.xml进行配置，但是这样的配置结构会随着项目代码的不断完善而造成web.xml文件过大的问题，而为了简化Servlet的配置形式，从Servlet 3.0之后提供了基于Annotation注解的配置模式，开发者可以直接在Servlet程序类中使用“@WebServlet”注解实现Servlet配置

‍

|No.|属性名称|类型|描述|
| -----| ----------------| --------------------| ---------------------------------------------------------------------------------------|
|01|name|java.lang.String|Servlet名称，等价于“<servlet-name>”元素定义，如果没有设置名称则会使用类名称自动配置|
|02|urlPatterns|java.lang.String[]|Servlet映射路径，等价于“<url-pattern>”元素定义|
|03|value|java.lang.String[]|等价于“urlPatterns”属性配置，两个属性不可同时出现|
|04|loadOnStartup|int|Servlet加载顺序，等价于“<load-on-startup>”元素定义|
|05|initParams|WebInitParam []|Servlet初始化参数，等价于“<init-param>”元素定义|
|06|asyncSupported|boolean|Servlet是否支持异步处理模式，等价于“<async-supported>”元素定义|
|07|description|java.lang.String|Servlet描述信息，等价于“<description>”元素定义|
|08|displayName|java.lang.String|Servlet显示名称，等价于“<display-name>”元素定义|

‍

```java
@WebServlet("/hello.action") // 直接使用默认的value属性进行匹配
public class HelloServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 在doGet()方法中已经明确的给出了HttpServletRequest、HttpServletResponse接口实例
        // 这个接口实例是在每一次进行请求处理的时候，由容器负责提供的
        // 在Java中提供了两个打印流：PrintStream、PrintWriter，HTML实际上是文本，所以字符流合适
        PrintWriter out = resp.getWriter(); // 获得了客户端浏览器的输出流
        out.println("<html>");
        out.println("<head>");
        out.println("   <title>Yootk - JavaWEB</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("   <h1>www.yootk.com</h1>");
        out.println("</body>");
        out.println("</html>");
        out.close(); // 关闭输出流
    }

}
```

‍

```java
@WebServlet(description = "使用注解实现的Servlet的配置，适用于Servlet 3.0标准之后",
    urlPatterns = {"/hello.action", "/muyan.yootk", "/muyan/yootk/*"}) // 直接使用默认的value属性进行匹配
public class HelloServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        // 在doGet()方法中已经明确的给出了HttpServletRequest、HttpServletResponse接口实例
        // 这个接口实例是在每一次进行请求处理的时候，由容器负责提供的
        // 在Java中提供了两个打印流：PrintStream、PrintWriter，HTML实际上是文本，所以字符流合适
        PrintWriter out = resp.getWriter(); // 获得了客户端浏览器的输出流
        out.println("<html>");
        out.println("<head>");
        out.println("   <title>Yootk - JavaWEB</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("   <h1>www.yootk.com</h1>");
        out.println("</body>");
        out.println("</html>");
        out.close(); // 关闭输出流
    }

}
```

此时就实现了多个映射路径的访问处理，利用Annotation就可以直接实现web.xml配置的简化处理. 

‍

‍

## 内置对象

Servlet可以实现全部JSP程序功能，所以在Servlet中可以直接进行内置对象的处理操作，在所有的Servlet实现子类中只要覆写了父类中的doXxx()方法，就可以直接获取到HttpServletRequest、HttpServletResponse的对象实例，同时由于所有的Servlet类都是Servlet接口的子类，那么就可以直接通过Servlet接口实例获取ServletConfig内置对象，实现初始化参数的获取. 

‍

‍

**获得初始化配置参数**

在之前进行Servlet生命周期分析的时候，曾经分析过一个“init()”方法，这个方法中可以直接接收ServletConfig内置对象，也就是说如果要想获取所有的初始化配置参数，最简单的方式就是可以在init()中完成，同时这个方法只在Servlet使用之前默认执行一次，所以适合于完成初始化操作. 

```java
@WebServlet(urlPatterns = {"/inner"}, initParams = {
        @WebInitParam(name = "message", value = "www.yootk.com"),
        @WebInitParam(name = "teacher", value = "Small Lee")},
        loadOnStartup = 1) // 直接使用默认的value属性进行匹配
public class InnerObjectServlet extends HttpServlet {
    @Override
    public void init(ServletConfig config) throws ServletException {
        Enumeration<String> enu = config.getInitParameterNames(); // 获取全部初始化参数名称
        while (enu.hasMoreElements()) {
            String paramName = enu.nextElement(); // 获取初始化参数名称
            System.out.println("【InnerObjectServlet.init(ServletConfig config)】" + paramName + " = " + config.getInitParameter(paramName));
        }
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
    }

    @Override   // 进行GET请求的处理
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}

```

‍

通过以上的分析就可以发现，只要利用父类所提供的getServletConfig()方法就可以轻松的获取到ServletConfig内置对象，这样就不必将初始化参数的获取操作只放在init()方法中完成了. 

```java
@WebServlet(urlPatterns = {"/inner"}, initParams = {
        @WebInitParam(name = "message", value = "www.yootk.com"),
        @WebInitParam(name = "teacher", value = "Small Lee")},
        loadOnStartup = 1) // 直接使用默认的value属性进行匹配
public class InnerObjectServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        Enumeration<String> enu = super.getServletConfig().getInitParameterNames(); // 获取全部初始化参数名称
        while (enu.hasMoreElements()) {
            String paramName = enu.nextElement(); // 获取初始化参数名称
            System.out.println("【InnerObjectServlet.init(ServletConfig config)】" + paramName + " = " + super.getServletConfig().getInitParameter(paramName));
        }
    }

    @Override   // 进行GET请求的处理
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

有了GenericServlet.init()方法的支持，可以在Servlet程序的任意位置上获取到ServletConfig内置对象来进行操作，但是如果从实际的开发来讲，所有的初始化参数一般都是在第一次加载的时候才会使用到. 

‍

### Application

‍

获取ServletContext接口实例

application内置对象对应的类型为“jakarta.servlet.ServletContext”接口实例，是一个保存在WEB容器中的内置对象，所有的用户都可以直接使用同一个application对象. 在Servlet中如果要想获取ServletContext接口实例，则可以通过GenericServlet类或者ServletRequest接口中的“getServletContext()”方法来完成

```java
@WebServlet("/inner")
public class InnerObjectServlet extends HttpServlet {
    @Override
    public void init() throws ServletException {
        System.out.println("【InnerObjectServlet.init()】RealPath = " + super.getServletContext().getRealPath("/"));
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.out.println("【InnerObjectServlet.doGet()】{GenericServlet}RealPath = " + super.getServletContext().getRealPath("/"));
        System.out.println("【InnerObjectServlet.doGet()】{ServletRequest}RealPath = " + req.getServletContext().getRealPath("/"));
    }
}
```

虽然GenericServlet类中提供了“getServletContext()”方法获取到application对象，但是在一些情况下是有可能获取不到的，所以最稳妥的获取方式就是通过“ServletRequest”接口来完成. 

‍

‍

### Session

‍

session是进行请求用户身份认证的重要内置对象，session内置对象对应的类型为“jakarta.servlet.http.HttpSession”，所有的session都必须通过HTTP客户端的Cookie获取相应的标识后才可以实现服务器状态的获取，所以如果要想在Servlet 中获取session内置对象就只有依靠HttpServletRequest接口来实现

```java
@WebServlet("/inner")
public class InnerObjectServlet extends HttpServlet {

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        HttpSession session = req.getSession(); // 获取HttpSession对象
        System.out.println("SESSIONID = " + session.getId()); // 获取当前的SessionID
        session.invalidate(); // Session失效
    }
```

‍

## **作用域对象**

‍

### 定义

‍

* 就是对象的生命周期，在javaweb开发里面有多个不同生命周期的对象

  > 比如：PageContext，ServletRequest，HttpSession，ServletContext；
  >
* 对象里面包含属性和对应的数据，所以不同作用域对象使用场景会不同

‍

### ServletContext

* 它代表了servlet环境的上下文，相当于一个全局存储空间
* 同一个WEB应用程序中，所有的Servlet和JSP都可以共享同一个区域，是最大的作用域对象

（webapps下的每个目录就是一个应用程序)

* 四大作用域对象-用于存取数据(举个形象的例子)：

  * PageContext(页面)->ServletRequest(请求)->HttpSession(会话)->【ServletContext】(应用)；
* 生命周期：在WEB服务器启动时创建，服务器关闭时销毁

‍

‍

## 跳转

‍

Servlet作为原生的Java程序，可以非常方便的实现各种业务逻辑的调用，同时也可以更加方便的从指定的数据源中获取所需要的数据信息，然而Servlet却存在有一个非常严重的问题，就是其**并不适合于HTML页面展示**，这样在实际的开发中就会考虑将所需的数据传递到JSP中进行数据展示

这个时候是将用户的请求相关的业务逻辑全部交给了Servlet来完成，而对于显示的部分Setrvlet不进行任何的操作，全部都用JSP来完成，如果要想在Servlet中实现JSP或者是其他页面的跳转，则可以使用两种处理形式:客户端跳转、服务端跳转. 

‍

### 客户端

‍

显示页面

为了便于整体资源的显示操作，所以使用了<base>元素，同时在该页面之中并没有涉及到繁琐的业务处理，仅仅是进行了一些属性内容的接收显示. ·

```java
<h1>REQUEST属性：<%=request.getAttribute("request-msg")%></h1>
<h1>SESSION属性：<%=session.getAttribute("session-msg")%></h1>
```

```java
<%@ page pageEncoding="UTF-8" %>    <%-- 设置显示编码 --%>
<%
    String basePath = request.getScheme() + "://" + request.getServerName() + ":" + request.getServerPort() + request.getContextPath() + "/";
%>
<html>
<head>
    <title>沐言科技：www.yootk.com</title>
    <base href="<%=basePath%>">
</head>
<body>
<div><img src="images/yootk.png"></div>
<h1>【Request属性接收】request-msg = <%=request.getAttribute("request-msg")%></h1>
<h1>【Session属性接收】session-msg = <%=session.getAttribute("session-msg")%></h1>
</body>
</html>
```

‍

每一个Servlet中进行的请求处理操作都会自动提供有HttpServletResponse内置对象，在HttpServletResonse接口中提供了一个请求重定向的处理方法：sendRedirect()，利用此方法即可实现Servlet跳转到JSP页面的功能. 

```java
@WebServlet("/jump")
public class JumpServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("request-msg", "www.yootk.com"); // request属性传递
        req.getSession().setAttribute("session-msg", "edu.yootk.com"); // session属性传递
        resp.sendRedirect("/show.jsp"); // 页面在根路径之中，所以可以直接跳转
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

‍

‍

### 服务端

RequestDispatcher接口

由于Servlet传递到JSP页面中的属性内容往往会包含有大量的数据信息，这样就需要在每次请求后将所传递的属性内容进行清空，所以这样就需要使用到服务器端跳转­，在Servlet中的服务器端跳转主要通过RequestDispatcher接口来实现

‍

```java
@WebServlet("/jump")
public class JumpServlet extends HttpServlet {
    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setAttribute("request-msg", "www.yootk.com"); // request属性传递
        req.getSession().setAttribute("session-msg", "edu.yootk.com"); // session属性传递
        RequestDispatcher requestDispatcher = req.getRequestDispatcher("/show.jsp"); // 具备了跳转的功能
        requestDispatcher.forward(req, resp); // 服务端跳转
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}
```

此时可以清楚的发现，request和session属性全部都可以直接传递到JSP页面之中，那么就得出一个重要的结论:在WEB开发中，对于JSP页面中的数据显示，往往只是显示一次，所以这个时候最佳的传递的处理形式使用的就是request属性范围. 

‍

‍

# 高级

‍

## 过滤器

‍

### 概念

传统的WEB开发中，由于只关心用户请求与响应的基本结构处理. 所以就需要开发者在Servlet程序中编写大量的非业务核心的处理逻辑，例如：“登录认证检查”、“编码设置”等，为了解决这些重复的公共操作，在Servlet 2.3标准中提供了过滤器的概念，利用过滤器可以自动的实现在请求和响应中间处理操作，使得程序的开发更加的简洁，也更加便于代码的维护

‍

由于HTTP协议中包含有两“请求”和“响应”两个组成部分，所以每一个过滤器都分为两部分：请求过滤、响应过滤，同时在项目中允许提供有多个过滤器，这多个过滤器也将按照定义顺序依次执行. 开发者定义过滤器时必须提供有一个过滤器的处理类，该类需要明确继承HttpFilter父类

‍

Servlet里面的过滤器作用

* 动态地拦截请求和响应，变换或使用包含在请求或响应中的信息
* 在客户端的请求访问后端资源之前，拦截这些请求.
* 在服务器的响应发送回客户端之前，处理这些响应.

‍

### 生命周期

* init(FilterConfig filterConfig) //只容器初始化的时候调用一次，即应用启动的时候加载一次
* doFilter(ServletRequest request, ServletResponse response, FilterChain chain) 只要命中过滤规则就触发，可以在filter中根据条件决定是否调用chain.doFilter(request, response)方法， 即是否让目标资源执行
* destroy() //只容器销毁的时候调用一次，即应用停止的时候调用一次

‍

‍

### HttpFilter类

‍

HttpFilter类中定义的方法

|No.|方法|所属归类|描述|
| -----| ------------------------------------------------------------------------------------------------------------------------------------| ---------------| ---------------------------------------------------------------|
|1|default voidinit(FilterConfig filterConfig)  throwsServletException|Filter|获取过滤器初始化配置参数|
|2|public void doFilter(ServletRequest  request, ServletResponse response, FilterChain chain) throws IOException,  ServletException|Filter|执行目标请求或下一个过滤器，通过FilterChain将请求向下继续传递|
|3|default void destroy()|Filter|过滤器销毁处理|
|4|public String getInitParameter(String  name)|FilterConfig|获取指定名称的初始化参数|
|5|Enumeration<String>  getInitParameterNames()|FilterConfig|获取全部初始化参数名称|
|6|public ServletContext  getServletContext()|FilterConfig|获取Servlet上下文|
|7|public FilterConfig getFilterConfig()|GenericFilter|获取FilterConfig对象|
|8|public void init() throws  ServletException|GenericFilter|Filter初始化，等价于“super.init(filterConfig)”|
|9|protected void  doFilter(HttpServletRequest req, HttpServletResponse res, FilterChain chain)  throws IOException, ServletException|HttpFilter|处理HTTP请求过滤|

‍

‍

### 搭建

如果要想定义一个过滤器,则一定要使用一个类,同时该类一定要继承自HttpFilter父类(如果你有需要也可以直接实现Filter接口，但是所提供的支持方法就会减少). 

‍

‍

#### 示例

```java
public class BaseFilter extends HttpFilter { // 实现一个Http过滤器

    @Override
    public void init(FilterConfig config) throws ServletException {    // 过滤器初始化
        System.out.println("【BaseFilter.init()】过滤器初始化，初始化参数：message = " + config.getInitParameter("message"));
    }

    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("【BaseFilter.doFilter()】用户请求过滤...");
    }

    @Override
    public void destroy() {
        System.out.println("【BaseFilter.destroy()】过滤器销毁");
    }
}
```

‍

‍

#### 配置

> 提醒:早期的Servlet (Filter也属于Servlet）程序编写的时候，是需要注意程序的编写顺序的  
> <servlet>标签要写在一起, 而“<filter>”标签也要写在一起，不能互相混写，但是新版本之中由于修改了XML定义，所以可以混写

```xml
    <filter>
        <filter-name>BaseFilter</filter-name>
        <filter-class>com.yootk.filter.BaseFilter</filter-class>
        <init-param>
            <param-name>message</param-name>
            <param-value>www.yootk.com</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>BaseFilter</filter-name>
        <url-pattern>/*</url-pattern>   <!-- 过滤器的执行路径，此时表示匹配所有WEB路径 -->
    </filter-mapping>

```

‍

示例2 放行

```java
public class BaseFilter extends HttpFilter { // 实现一个Http过滤器

    @Override
    public void init(FilterConfig config) throws ServletException {    // 过滤器初始化
        System.out.println("【BaseFilter.init()】过滤器初始化，初始化参数：message = " + config.getInitParameter("message"));
    }

    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("【BaseFilter.doFilter()】用户请求过滤...");
        chain.doFilter(request, response); // 放行
        System.out.println("【BaseFilter.doFilter()】服务端响应过滤...");
    }

    @Override
    public void destroy() {
        System.out.println("【BaseFilter.destroy()】过滤器销毁");
    }
}
```

‍

#### 注意

过滤器是依据匹配的路径来进行执行处理的

在进行过滤器处理的时候必须使用"chain.doFilter(request,response);”方法进行用户请求目标路径的跳转，如果不跳转，则表示当前的响应由过滤器自行完成

过滤器的基本生命周期与Servlet类似,但是不需要做任何的配置, 其就在容器初始化的时候自动进行init()方法的调用

FilterConfig 与ServletConfig的作用相同，全部都是获取初始化的配置参数的

‍

‍

### 转发模式

Dispatcher

‍

#### DispatcherType

在默认情况下只要项目中配置了过滤器，并且用户所请求的路径与过滤路径相匹配时，都会自动的进行过滤器的执行，但是在Servlet3.0标准中进一步规范化了过滤器的执行范围，例如：跳转触发、包含触发等，这些转发模式的范围全部都通过jakarta.servlet.DispatcherType枚举类定义

‍

|No.|函数|描述|
| -----| ----------------------------------------| -------------------------------------------------|
|1|jakarta.servlet.DispatcherType.REQUEST|在每次请求时执行过滤器，此为默认转发模式|
|2|jakarta.servlet.DispatcherType.ASYNC|在开启异步响应时转发|
|3|jakarta.servlet.DispatcherType.ERROR|在出现错误时转发|
|4|jakarta.servlet.DispatcherType.FORWARD|在执行“RequestDispatcher.forward()”操作时转发|
|5|jakarta.servlet.DispatcherType.INCLUDE|在执行“RequestDispatcher.include()”操作时转发|

‍

‍

#### 示例

```java

public class BaseFilter extends HttpFilter { // 实现一个Http过滤器
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("【BaseFilter.doFilter()】Request属性：" +
                request.getAttribute("request-msg") + "、Session属性：" +
                request.getSession().getAttribute("session-msg"));
        chain.doFilter(request, response); // 跳转到目标处理路径
    }
}
```

‍

```xml
    <filter>
        <filter-name>BaseFilter</filter-name>
        <filter-class>com.yootk.filter.BaseFilter</filter-class>
    </filter>
    <filter-mapping>
        <filter-name>BaseFilter</filter-name>
        <url-pattern>/*</url-pattern>   <!-- 过滤器的执行路径，此时表示匹配所有WEB路径 -->
        <dispatcher>REQUEST</dispatcher>
        <dispatcher>FORWARD</dispatcher>
        <dispatcher>INCLUDE</dispatcher>
        <dispatcher>ERROR</dispatcher>
        <dispatcher>ASYNC</dispatcher>
    </filter-mapping>
```

在正常的情况下是在每次请求的时候才会进行过滤器的触发，但是一旦配置了转发模式，就可以针对于服务端跳转来实现这样过滤处理，所以就可以直接获取到所设置的属性内容了. 

‍

‍

### 注解配置

Servlet 3.0版本后，为了简化过滤器的配置，提供了“@WebFilter”注解项，直接在过滤器类的定义上使用此注解，随后设置好相应的属性即可

|No.|属性名称|类型|描述|
| -----| -----------------| --------------------| -------------------------------------------------------------------------------------|
|01|filterName|java.lang.String|指定过滤器的名称，等价于“<filter-name>”配置|
|02|urlPatterns|java.lang.String[]|设置过滤器的触发路径，等价于“<url-pattern>”配置|
|03|value|java.lang.String[]|设置过滤器路径，与“urlPatterns”作用相同|
|04|servletNames|java.lang.String[]|指定过滤器应用于那些Servlet，使用“@WebServlet”中的name匹配|
|05|initParams|WebInitParam []|设置过滤器初始化参数，等价于“<init-param>”配置|
|06|asyncSupported|boolean|是否支持异步响应，等价于“<async-supported>”配置|
|07|description|java.lang.String|过滤器描述信息|
|08|displayName|java.lang.String|过滤器显示名称|
|09|dispatcherTypes|DispatcherType[]|指定过滤器的转发模式. 取值范围包括如下几种：ASYNC、ERROR、FORWARD、INCLUDE、REQUEST|

‍

#### 示例

```java
@WebFilter(urlPatterns = {"/pages/*", "/admin/*"},
    initParams = {
        @WebInitParam(name = "message", value = "www.yootk.com")
    })
public class BaseFilter extends HttpFilter { // 实现一个Http过滤器
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("【BaseFilter.doFilter()】message = " + super.getInitParameter("message"));
        chain.doFilter(request, response); // 跳转到目标处理路径
    }
}
```

‍

‍

‍

### 执行顺序

‍

利用过滤器可以方便的实现请求数据的处理，同时在JavaWEB 中允许开发者配置多个不同的过滤器，而这多个过滤器之间依靠类名称的字母顺序进行执行，例如，现在有两个Filter类同时映射到了一个过滤路径，一个类名称为“AFilter”，另一个类名称为“BFilter”，则按照字母顺序“AFilter”会先执行

实际上默认情况下过滤器的执行顺序就是类名称的字母顺序，按照字母的升序进行排列, 之所以拿出来做分析是因为容易把过滤器的名称和“filterName”搞混，在进行每一个过滤器定义的时候都可以为其定义一个唯一的名称，如果没有定义该名称则默认会使用类名称作为组件的名称. 

‍

‍

### 编码过滤

‍

在每一次WEB请求时为了可以获得正确的数据内容，都需要对请求和响应数据进行编码处理，这样一来，几乎所有的Servlet与JSP程序都需要调用“setCharacterEncoding()”方法，这样一来就使得编码的维护困难. 

虽然在实际开发中会广泛的使用“UTF-8”编码，但是在项目中依然有可能会出现其他的程序编码，而为了便于程序编码的管理，可以将所有的编码设置交由过滤器完成，同时利用初始化参数的形式实现项目编码的动态配置，这样以来就使得编码的配置更加方便，提高了代码的可重用性. 

过滤器的执行顺序是由其自身的类名称来决定的，和“@WebFilter”之中的“filterName”属性没有任何的直接联系. 

‍

#### 示例

```java
public class EncodingFilter extends HttpFilter { // 定义编码过滤类
    // 默认的编码，如果用户没有配置任何的编码，则使用此编码
    public static final String DEFAULT_ENCODING = "UTF-8";
    private String charset; // 接收初始化参数

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.charset = filterConfig.getInitParameter("charset"); // 接收初始化参数
        if (this.charset == null || "".equals(this.charset)) {  // 没有配置编码
            this.charset = DEFAULT_ENCODING; // 使用默认编码
        }
    }
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        request.setCharacterEncoding("UTF-8");
        response.setCharacterEncoding("UTF-8");
        chain.doFilter(request, response);
    }
}
```

‍

配置

```xml
 <filter>
        <filter-name>EncodingFilter</filter-name>
        <filter-class>com.yootk.filter.EncodingFilter</filter-class>
        <init-param>
            <param-name>charset</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>EncodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

‍

‍

```java
@WebServlet("/input.action")
public class InputServlet extends HttpServlet {
    public InputServlet() {
        System.out.println("【InputServlet.()】构造方法。");
    }

    @Override
    public void init() throws ServletException {
        System.out.println("【InputServlet.init()】Servlet初始化。");
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        resp.setContentType("text/html;charset=UTF-8"); // 如果不设置就出现显示乱码
        String msg = req.getParameter("message"); // 接收请求参数
        PrintWriter out = resp.getWriter(); // 获得了客户端浏览器的输出流
        out.println("<html>");
        out.println("<head>");
        out.println("   <title>Yootk - JavaWEB</title>");
        out.println("</head>");
        out.println("<body>");
        out.println("   <h1>" + msg + "</h1>"); // 输出请求参数
        out.println("</body>");
        out.println("</html>");
        out.close(); // 关闭输出流
    }
    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp); // 调用doGet()处理
    }
}
```

‍

‍

### 登录检测

为了保证项目的运行安全，所有的使用者都必须经过认证，并且通过session实现认证信息的保存，这样就要求在每一个请求前都自动进行认证检查，当认证检查通过后才允许进行请求处理，而当认证检查失败后就会自动跳转到登录页，这样就可以将认证检查的代码直接放在过滤器中，在每次请求前自动进行相关登录认证检查的处理操作

‍

#### 示例

‍

**登录页面**

```java
<body>
<img src="../../images/logo.png" style="width:200px;">
<h1>欢迎您的访问，请为自己认真学习编程技术，<a href="../../logout.jsp">系统注销</a></h1>
</body>
```

‍

**过滤器**

```java
public class LoginFilter extends HttpFilter { // 定义编码过滤类
    private String auth; // 保存session的检验标记

    @Override
    public void init(FilterConfig filterConfig) throws ServletException {
        this.auth = filterConfig.getInitParameter("auth"); // 接收初始化参数
    }
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        if (this.auth != null) {    // 现在需要进行验证
            if (request.getSession().getAttribute(this.auth) != null) { // 属性存在
                chain.doFilter(request, response); // 正常访问
            } else {    // 属性不存在
               request.getRequestDispatcher("/login.jsp").forward(request, response);
            }
        } else { // 如果没有设置session属性名称，不需要验证
            chain.doFilter(request, response); // 直接转发到目标访问路径
        }
    }
}

```

‍

**配置**

```xml
    <filter>
        <filter-name>LoginFilter</filter-name>
        <filter-class>com.yootk.filter.LoginFilter</filter-class>
        <init-param>
            <param-name>auth</param-name>
            <param-value>id</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>LoginFilter</filter-name>
        <url-pattern>/pages/*</url-pattern>
    </filter-mapping>
```

‍

## 监听器

监听器是一个实现了特定接口的普通Java类，用于监听其他对象的创建和销毁，监听其他对象的方法执行和属性改变；

监听器是Servlet 3.1提供的重要组件，利用监听器可以方便实现WEB中指定操作状态的监控处理，在用户每次向服务器端发送请求时，实际上都会自动的被请求监听器所监听，同时也会自动的产生有一个相应的请求事件，这样开发者就可以编写专属的事件处理类，对请求的状态进行控制

‍

分类：

* ServletContextLitener
* HttpSessionListener
* ServletRequestListener

‍

‍

#### ServletContextListener全局配置加载

**实战自定义ServletContext监听器**

* 使用场景：加载全局配置，初始化项目信息
* web.xml配置

```java
  <context-param>
        <param-name>url</param-name>
        <param-value>https://xdclass.net</param-value>
    </context-param>

    <context-param>
        <param-name>topic</param-name>
        <param-value>小滴课堂java高级工程师成长专题视频</param-value>
    </context-param>
```

‍

监听器开发

```java
@WebListener
public class ContextListener implements ServletContextListener {

    @Override
    public void contextInitialized(ServletContextEvent sce) {
  
        System.out.println("ContextListener contextInitialized");
        ServletContext servletContext = sce.getServletContext();
        String url = servletContext.getInitParameter("url");
        String topic = servletContext.getInitParameter("topic");
  
        Config config = new Config();
        config.setTopic(topic);
        config.setUrl(url);
        servletContext.setAttribute("config",config);
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
  
        System.out.println("ContextListener contextDestroyed");
    }
}
```

‍

```java
//获取上下文对象
ServletContext sc = sce.getServletContext();
sc.setAttribute("onlineNum",0);
```

‍

HttpSessionListener开发

```java
@WebListener
public class SessionListener implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("SessionListener sessionCreated");
  
        ServletContext servletContext  = se.getSession().getServletContext();
  
        //获取在线人数
        Integer onlineNum = (Integer)servletContext.getAttribute("onlineNum");
  
        //新增1
        servletContext.setAttribute("onlineNum",++onlineNum);
  
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("SessionListener sessionDestroyed");
  
        ServletContext servletContext  = se.getSession().getServletContext();
  
        //获取在线人数
        Integer onlineNum = (Integer)servletContext.getAttribute("onlineNum");
  
        //减少1
        servletContext.setAttribute("onlineNum",--onlineNum);
  
    }
}
```

‍

add.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>小滴课堂javaweb统计在线人数</title>
</head>
<body>

<hr>

近30分钟在线人数: ${applicationScope.onlineNum}
</body>
</html>
```

‍

delete.jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>小滴课堂 xdclass.net 专题课程</title>
</head>
<body>

销毁session
<hr>
<% request.getSession().invalidate(); %>
</body>
</html>
```

‍

注意：

* 关闭启动tomcat自动打开浏览器，因为会触发会触发多个session
* 使用多个浏览器测试 粗略统计，如果是多机器分布式情况，需要用到分布式缓存

‍

#### ServletRequestListener监听

jakarta.servlet.ServletRequestListener提供了客户端请求的监听控制，开发者可以利用此接口实现用户请求初始化监听，以及用户请求销毁监听，当事件被监听到后都会自动的产生一个“ServletRequestEvent”事件源对象，开发者可以通过此对象获取ServletRequest与ServletContext接口实例

‍

网络播放量处理示例

```java
public class VideoCountListener implements ServletRequestListener  {
    @Override
    public void requestInitialized(ServletRequestEvent sre) { // 请求初始化
        System.out.println("【VideoCountListener.requestInitialized()】进行本次请求的处理。");
        // 一般来讲对于视频播放是有一个前提：打开之后跳转了再进行视频的播放，所以在播放之前要实现一个访问量的更新
        HttpServletRequest request = (HttpServletRequest) sre.getServletRequest();
        String previousUrl = request.getHeader("Referer"); // 之前的访问路径
        // 视频的访问路径：localhost:8080/muyan/yootk/video/10
        String pattern = ".+/video/vid=\\d+"; // 路径的正则匹配
        if (previousUrl.matches(pattern)) { // 路径拼配
            System.out.println("【VideoCountListener.requestInitialized()】视频访问量增加处理。");
        }
    }

    @Override
    public void requestDestroyed(ServletRequestEvent sre) {
        System.out.println("【VideoCountListener.requestDestroyed()】本次请求处理完成。");
    }
}
```

```java
    <listener>
        <listener-class>com.yootk.listener.VideoCountListener</listener-class>
    </listener>
```

```java
@WebServlet("/video/*")
public class VideoServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.getRequestDispatcher("/video.jsp").forward(req, resp);
    }
}
```

‍

页面

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ page import="com.yootk.dbc.DatabaseConnection" %>
<%@ page import="java.sql.*" %>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/index.js"></script>
</head>
<body>
	<div class="container contentback">
		<div class="row">
			<div class="col-sm-12">
				<video width="1140" height="715" controls="controls" autoplay="autoplay" loop="loop">
					<source src="images/main.mp4" type="video/mp4">
				</video>
			</div>
		</div>
	</div>
</body>
</html>
```

‍

‍

‍

#### ServletRequestAttributeListener监听

请求属性监听

‍

在JavaWEB开发中，属性传递属于最核心的数据处理操作，在每一次服务器跳转中，都可以实现request属性的传递，但是在跨越多个JSP/Servlet时也有可能会产生属性修改（设置的属性名称相同），属性删除等操作，那么这些就可以通过jakarta.servlet.ServletRequestAttributeListener监听接口来实现

```java
public class GlobalRequestAttributeRecord implements ServletRequestAttributeListener { // 请求属性监听

    @Override
    public void attributeAdded(ServletRequestAttributeEvent srae) { // 属性增加时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeAdded() - 属性增加】name = " + srae.getName() + "、value = " + srae.getValue());
    }

    @Override
    public void attributeReplaced(ServletRequestAttributeEvent srae) { // 属性替换时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeReplaced() - 属性替换】name = " + srae.getName() + "、value = " + srae.getValue());
    }

    @Override
    public void attributeRemoved(ServletRequestAttributeEvent srae) { // 属性删除时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeRemoved() - 属性删除】name = " + srae.getName() + "、value = " + srae.getValue());
    }
}
```

‍

```java
    <listener>
        <listener-class>
            com.yootk.listener.GlobalRequestAttributeRecord
        </listener-class>
    </listener>
```

‍

```java
<%
	request.setAttribute("message", "沐言科技：www.yootk.com"); // 设置request属性
%>
<jsp:forward page="request_attribute_replace.jsp"/>


<%
	request.setAttribute("message", "沐言科技：www.yootk.com"); // 设置request属性
	request.removeAttribute("message"); // 属性删除
%>

```

‍

#### @WebListener注解

```java
@WebListener							// 配置监听器public class GlobalRequestAttributeRecord implements ServletRequestAttributeListener {}

```

‍

```java
@WebListener
public class GlobalRequestAttributeRecord implements ServletRequestAttributeListener { // 请求属性监听

    @Override
    public void attributeAdded(ServletRequestAttributeEvent srae) { // 属性增加时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeAdded() - 属性增加】name = " + srae.getName() + "、value = " + srae.getValue());
    }

    @Override
    public void attributeReplaced(ServletRequestAttributeEvent srae) { // 属性替换时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeReplaced() - 属性替换】name = " + srae.getName() + "、value = " + srae.getValue());
    }

    @Override
    public void attributeRemoved(ServletRequestAttributeEvent srae) { // 属性删除时触发
        System.out.println("【GlobalRequestAttributeRecord.attributeRemoved() - 属性删除】name = " + srae.getName() + "、value = " + srae.getValue());
    }
}
```

‍

‍

### HttpSessionListener

WEB开发中为了便于用户管理，每一个用户的请求都会为其分配一个唯一的Session，以方便进行状态的维护，而在用户进行session创建时就可以利用监听器进行状态的监控，例如：session创建、销毁、属性操作等，这些操作状态都提供有完善的事件监控，开发者只要捕获到这些事件就可以进行监听处理

```java
@WebListener
public class UserStateMonitor implements HttpSessionListener { // 针对于Session的状态实现监听

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("【UserStateMonitor.sessionCreated()】SessionID = " + se.getSession().getId());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("【UserStateMonitor.sessionDestroyed()】SessionID = " + se.getSession().getId());
    }
}

```

‍

手工注销

```java
<%
	session.invalidate(); // 手工注销
%>

```

‍

过期时间配置

```java
    <session-config>
        <session-timeout>30</session-timeout>
    </session-config>

```

‍

#### HttpSessionIdListener

当用户成功的通过服务器获取HTTP资源后，服务器会自动通过Cookie的形式将当前客户的SessionID保存在客户端浏览器，这样每当通过该浏览器发出请求后，都可以自动的匹配上WEB容器中所保存的SessionID，在最初的JavaWEB开发中，一旦SessionID生成则不可修改，而在Servlet 3.1标准中对这一限制进行了扩充，允许用户通过HttpServletRequest子接口提供的“changeSessionId()”方法获取一个新的SessionID，同时提供了一个“jakarta.servlet.http.HttpSessionIdListener”接口可以对每次SessionID修改的状态进行监听

‍

监听

```java
@WebListener
public class UserSessionChange implements HttpSessionIdListener {
    @Override
    public void sessionIdChanged(HttpSessionEvent httpSessionEvent, String s) {
        System.out.println("【UserSessionChange.sessionIdChanged()】新的SessionID = " + httpSessionEvent.getSession().getId() + "、旧的SessionID = " + s + "、Session属性：" + httpSessionEvent.getSession().getAttribute("message"));
    }
}

```

‍

‍

```java
<%
	request.changeSessionId(); // 更改SessionID
%>
<h1>【Session属性】message = <%=session.getAttribute("message")%></h1>
```

```java
<%
   session.setAttribute("message", "沐言科技：www.yootk.com"); // 设置request属性
%>

```

‍

‍

#### HttpSessionAttributeListener

session属性范围可以在一次用户请求中始终进行用户数据信息的存储，为了可以实现属性操作的监听，在JavaWEB编程开发中提供了“jakarta.servlet.http.HttpSessionAttributeListener”监听接口，利用该接口可以实现属性设置、属性替换以及属性删除的事件监听处理

‍

‍

```java
@WebListener
public class SessionAttributeMonitor implements HttpSessionAttributeListener {
    @Override
    public void attributeAdded(HttpSessionBindingEvent se) {    // 属性增加时触发
        System.out.println("【SessionAttributeMonitor.attributeAdded()】属性增加，属性名称：" + se.getName() + "、属性内容：" + se.getValue());
    }

    @Override
    public void attributeRemoved(HttpSessionBindingEvent se) { // 属性删除时触发
        System.out.println("【SessionAttributeMonitor.attributeRemoved()】属性删除，属性名称：" + se.getName() + "、属性内容：" + se.getValue());
    }

    @Override
    public void attributeReplaced(HttpSessionBindingEvent se) { // 属性替换时触发
        System.out.println("【SessionAttributeMonitor.attributeReplaced()】属性替换，属性名称：" + se.getName() + "、属性内容：" + se.getValue());
    }
}
```

‍

```java
<%
	session.setAttribute("message", "沐言科技：www.yootk.com"); // 设置session属性
%>

<%
	session.setAttribute("message", "沐言科技：www.yootk.com"); // 设置session属性
	session.removeAttribute("message"); // 删除属性
%>

```

‍

#### HttpSessionBindingListener

项目开发中由于session经常需要保存有用户的数据信息，所以在session对象中最为重要的操作就是属性的保存与删除，除了使用HttpSessionAttributeListener接口实现属性状态的跟踪外，在JavaWEB中又提供了一个不需要进行注册的监听接口，该接口为“jakarta.servlet.http.HttpSessionBindingListener”，在实际使用中，开发者只需要在进行session属性操作时，将属性内容设置为HttpSessionBindingListener子类对象实例，即可自动的进行valueBound()与valueUnbound()两个监听方法的调用

‍

```java

public class UserAuthenticationListener implements HttpSessionBindingListener { // 是一个特殊结构的类
    private String userid; // 保存用户id
    public UserAuthenticationListener(String userid) {  // 不提供无参构造方法
        this.userid = userid; // 保存用户id信息
    }
    public String getUserid() {
        return userid;
    }

    @Override
    public void valueBound(HttpSessionBindingEvent event) { // 绑定处理
        // 在实际的项目之中，此处可以编写很多种操作代码的形式，例如：可以考虑将该数据保存在Redis里面。
        System.out.println("【UserAuthenticationListener.valueBound()】属性绑定，name = " + event.getName() + "、value = " + event.getValue());
    }

    @Override
    public void valueUnbound(HttpSessionBindingEvent event) {
        System.out.println("【UserAuthenticationListener.valueUnbound()】属性解绑，name = " + event.getName() + "、value = " + event.getValue());
    }
}
```

‍

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ page import="com.yootk.listener.UserAuthenticationListener" %>	<%-- 导入程序的处理类 --%>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/index.js"></script>
</head>
<body>
<%
	UserAuthenticationListener user = new UserAuthenticationListener("yootk.com"); // 设置用户名
	session.setAttribute("user", user); // 属性保存
	Thread.sleep(3000); // 等待3秒的时间
	session.removeAttribute("user"); // 属性删除
%>
</body>
</html>
```

‍

‍

#### HttpSessionActivationListener

Session钝化与激活

了便于保持用户的状态，就需要在WEB容器中存储有大量的用户session信息，但是随着并发访问用户的增多，此种保存模式必然会带来较大的内存占用，而导致频繁的GC操作，从而影响服务器的处理性能，为了解决这一问题，JavaWEB提供了session的序列化管理机制，可以将暂时不活跃的session信息保存到其对应的二进制文件之中（此操作称为“钝化”），而后再需要的时候可以根据SessionID通过磁盘恢复对应的Session数据（此操作称为“激活”），这样就可以极大的减少服务器内存占用的问题，从而实现较高的服务器处理性能. 

‍

‍

序列化配置文件

WEB项目/META-INF/context.xml

```java
<Context>   		<!-- 配置上下文 -->
    <!-- 配置Session持久化管理策略，当session超过1分钟不活跃时，则进行持久化处理 -->
    <Manager className="org.apache.catalina.session.PersistentManager" maxIdleSwap="1">
        <!-- 定义文件存储，同时设置文件存储的路径 -->
        <Store className="org.apache.catalina.session.FileStore" directory="h:/session"/>
    </Manager>
</Context>

```

```java
public class SessionStoreListener implements HttpSessionActivationListener, Serializable {
    private String userid;
    public SessionStoreListener(String userid) {
        this.userid = userid;
    }
    @Override
    public void sessionDidActivate(HttpSessionEvent se) {
        System.out.println("【Session激活】SessionId = " + se.getSession().getId() + "、userid = " + this.userid);
    }

    @Override
    public void sessionWillPassivate(HttpSessionEvent se) {
        System.out.println("【Session钝化】SessionId = " + se.getSession().getId() + "、userid = " + this.userid);
    }
}

```

```java
<%
	SessionStoreListener store = new SessionStoreListener("yootk.com"); // 设置用户名
	session.setAttribute("user", store); // 属性保存
%>
```

Session持久化管理

‍

在JavaWEB中，针对于Session数据持久化管理提供了一个jakarta.servlet.http.HttpSessionActivationListener接口，在该接口中可以针对于session数据序列化（钝化方法“sessionWillPassivate()”）以及session数据反序列化（激活方法“sessionDidActivate()”）实现状态监听

‍

‍

### ServleContext

‍

在Tomcat中会同时拥有多个不同的WEB上下文环境，每一个WEB上下文都拥有独立的应用环境，在WEB容器中会针对WEB上下文的创建、销毁、属性操作等实现相应的处理事件，以方便用户进行不同状态的监控

每一个WEB应用启动后都存在有一个ServletContext实例化对象，所以在ServletContext初始化完成以及ServletContext销毁都会产生有相应的处理事件，事件产生后可以通过jakarta.servlet.ServletContextListener接口实现事件的监听处理

‍

```java
<%@ page language="java" contentType="text/html; charset=UTF-8"
	pageEncoding="UTF-8"%>
<%@ page import="java.util.*" %>	<%-- 导入程序的处理类 --%>
<%@include file="/pages/plugins/include_basepath.jsp" %>
<html>
<head>
<base href="<%=basePath%>">
<jsp:include page="/pages/plugins/include_javascript.jsp" />
<script type="text/javascript" src="js/index.js"></script>
</head>
<body>
<%
	Enumeration<String> enu = application.getAttributeNames();
	while (enu.hasMoreElements()) {
		String name = enu.nextElement(); // 获取属性名称
%>
		<p><%=name%> = <%=application.getAttribute(name)%></p>
<%
	}
%>
</body>
</html>
```

```java
@WebListener
public class WebContextListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) {
        System.out.println("【WebContextListener.contextInitialized()】Servlet上下文初始化：" + sce.getServletContext().getVirtualServerName());
    }

    @Override
    public void contextDestroyed(ServletContextEvent sce) {
        System.out.println("【WebContextListener.contextDestroyed()】Servlet上下文销毁：" + sce.getServletContext().getVirtualServerName());
    }
}
```

‍

#### ServletContextAttributeListener

每一个WEB应用都可以通过application内置对象实现上下文属性的操作配置，如果需要对属性的操作事件进行处理，则可以直接通过jakarta.servlet.ServletContextAttributeListener接口完成

‍

```java
@WebListener
public class WebAttributeListener implements ServletContextAttributeListener {
    @Override
    public void attributeAdded(ServletContextAttributeEvent scae) {
        System.out.println("【WebAttributeListener.attributeAdded()】属性增加，属性名称：" + scae.getName() + "、属性内容：" + scae.getValue());
    }

    @Override
    public void attributeReplaced(ServletContextAttributeEvent scae) {
        System.out.println("【WebAttributeListener.attributeReplaced()】属性替换，属性名称：" + scae.getName() + "、属性内容：" + scae.getValue());
    }

    @Override
    public void attributeRemoved(ServletContextAttributeEvent scae) {
        System.out.println("【WebAttributeListener.attributeRemoved()】属性删除，属性名称：" + scae.getName() + "、属性内容：" + scae.getValue());
    }
}
```

```java
<%
	application.setAttribute("message", "沐言科技：www.yootk.com");
%>
```

‍

‍

### 实操

‍

#### **统计当前在线人数**

‍

ContextLisener配置

```java
//获取上下文对象
ServletContext sc = sce.getServletContext();
sc.setAttribute("onlineNum",0);
sc.setAttribute("totalVisit",0);
```

‍

RequestListener开发

```java
@WebListener
public class SessionListener implements HttpSessionListener {

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("SessionListener sessionCreated");
  
        ServletContext servletContext  = se.getSession().getServletContext();
  
        //获取在线人数
        Integer onlineNum = (Integer)servletContext.getAttribute("onlineNum");
  
        //新增1
        servletContext.setAttribute("onlineNum",++onlineNum);
  
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("SessionListener sessionDestroyed");
  
        ServletContext servletContext  = se.getSession().getServletContext();
  
        //获取在线人数
        Integer onlineNum = (Integer)servletContext.getAttribute("onlineNum");
  
        //减少1
        servletContext.setAttribute("onlineNum",--onlineNum);
  
    }
}
```

‍

add.jsp

```java
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>

<head>
    <title>小滴课堂javaweb统计在线人数</title>
</head>
<body>

<hr>

近30分钟在线人数: ${applicationScope.onlineNum}
<hr>
应用服务器启动后总访问次数：${totalVisit}
</body>
</html>
```

‍

## 动态注册

‍

> 到现在为止已经学习了Servlet、Filter、Listener，可以发现，所有这些的组件都需要进行WEB容器的整合配置  
> (想生效必须手工配置，无法通过程序管理)

‍

更加方便的服务注册功能，开发者可以直接在容器启动时，利用反射机制实现WEB组件类的引入

‍

‍

‍

### Servlet

Servlet程序类是JavaWEB开发中的主要程序组件，如果开发者需要通过程序动态的进行Servlet的注册，则可以直接使用ServletContext接口提供的方法. 

|No.|方法|类型|描述|
| -----| --------------------------------------------------------------------------------------------------------| ------| ------------------------------------------------------------------------------|
|1|public <T extends Servlet> TcreateServlet(Class<T>clazz)  throwsServletException|普通|创建Servlet接口对象实例|
|2|publicServletRegistration.Dynamic addServlet(StringservletName,  Class<? extends Servlet>servletClass)|普通|利用反射类Class获取Servlet接口对象实例，并自动向WEB容器中进行注册|
|3|public ServletRegistration.Dynamic  addServlet(String servletName, Servlet servlet)|普通|将一个已经实例化完成的Servlet接口实例注册到WEB容器之中|
|4|public ServletRegistration.Dynamic  addServlet(String servletName, String className)|普通|传入Servlet的完整类名称，而后基于反射的方式自动进行实例化并注册到WEB容器之中|

‍

#### 示例

```java
@WebListener
public class DynamicRegistrationListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) { // 容器初始化进行组件注册
        // 所有的组件的动态注册都是基于ServletContext接口实例完成的
        ServletContext application = sce.getServletContext(); // 获取上下文接口实例
        ServletRegistration.Dynamic registration = application.addServlet("helloServlet", HelloServlet.class); // 反射类加载
        registration.setLoadOnStartup(1); // 设置Servlet启动时的加载
        registration.setInitParameter("message", "www.yootk.com");
        registration.addMapping("/hello.action", "/muyan.yootk", "/muyan/yootk/*");
    }
}
```

‍

‍

### Filter

ServletContext提供的Filter注册方法

|No.|方法|类型|描述|
| -----| -----------------------------------------------------------------------------------------------| ------| ----------------------------------------------------------------------------------------|
|1|public <T extends Filter> TcreateFilter(Class<T>clazz)  throwsServletException|普通|根据Filter的Class对象实例创建一个Filter接口对象实例|
|2|FilterRegistration.Dynamic  addFilter(String filterName, Class<? extends Filter> filterClass)|普通|通过Filter的Class对象实例创建一个Filter接口对象，同时设置filterName，并向WEB容器中注册|
|3|FilterRegistration.Dynamic  addFilter(String filterName, Filter filter)|普通|将一个已经创建完成的Filter接口对象，注册到WEB容器之中，并为其设置filterName|
|4|public FilterRegistration.Dynamic  addFilter(String filterName, String className)|普通|通过完整的Filter类名称动态实例化一个Filter接口对象，并将其动态注册到WEB容器之中|

‍

```java
import java.io.IOException;
public class BaseFilter extends HttpFilter { // 实现一个Http过滤器
    @Override
    protected void doFilter(HttpServletRequest request, HttpServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("【BaseFilter.doFilter()】执行过滤处理操作...");
        chain.doFilter(request, response); // 跳转到目标处理路径
    }
}
```

```java
@WebListener
public class DynamicRegistrationListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) { // 容器初始化进行组件注册
        // 所有的组件的动态注册都是基于ServletContext接口实例完成的
        ServletContext application = sce.getServletContext(); // 获取上下文接口实例
        FilterRegistration.Dynamic filter = application.addFilter("baseFilter", "com.yootk.filter.BaseFilter");
        filter.addMappingForUrlPatterns(EnumSet.of(DispatcherType.REQUEST, DispatcherType.FORWARD), false, "/*");
    }
}

```

‍

#### 示例

‍

### Listener

ServletContext提供的Listener注册方法

|No.|方法|类型|描述|
| -----| -----------------------------------------------------------------------------------------| ------| ----------------------|
|1|public <T extendsEventListener>  TcreateListener(Class<T>clazz)  throwsServletException|普通|创建Listener对象实例|
|2|public voidaddListener(Class<?  extendsEventListener>listenerClass)|普通|注册Listener|
|3|public void addListener(String  className)|普通|注册Listener|
|4|public <T extends EventListener>  void addListener(T t)|普通|注册Listener|

‍

‍

#### 示例

```java
public class UserStateMonitor implements HttpSessionListener { // 针对于Session的状态实现监听

    @Override
    public void sessionCreated(HttpSessionEvent se) {
        System.out.println("【UserStateMonitor.sessionCreated()】SessionID = " + se.getSession().getId());
    }

    @Override
    public void sessionDestroyed(HttpSessionEvent se) {
        System.out.println("【UserStateMonitor.sessionDestroyed()】SessionID = " + se.getSession().getId());
    }
}

```

‍

```java
@WebListener
public class DynamicRegistrationListener implements ServletContextListener {
    @Override
    public void contextInitialized(ServletContextEvent sce) { // 容器初始化进行组件注册
        // 所有的组件的动态注册都是基于ServletContext接口实例完成的
        ServletContext application = sce.getServletContext(); // 获取上下文接口实例
        try {
            EventListener listener = application.createListener(UserStateMonitor.class); // 创建一个事件监听类
            application.addListener(listener);
            // 等价操作：application.addListener("com.yootk.listener.UserStateMonitor");
        } catch (ServletException e) {
            e.printStackTrace();
        }
    }
}

```

‍

‍

### ServletContainerInitializer

‍

虽然在ServletContext接口中提供了WEB组件的动态注册支持，但是在使用前开发者必须自定义监听器，而后还需要进行一系列的代码配置后才能够生效，这样的做法实际上并不利于组件的动态扩展应用. 在一个完整的WEB程序中，为了便于代码的管理，往往需要引入一系列的Java程序库（*.jar文件），这些JAR文件中的内部可能就包含有程序所需要的WEB组件，而此时最佳的做法就是在项目直接引入此JAR文件，而后让这个JAR文件中的WEB组件自动进行配置，当不再需要使用到这些组件的时候只要从项目中移除JAR文件即可

‍

‍

#### 示例

```java

public class HelloServlet extends HttpServlet {
    @Override
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        resp.getWriter().println("<h1>沐言科技：www.yootk.com</h1>");
    }
}

```

‍

```java
@HandlesTypes({HelloServlet.class}) // 必须在此处配置允许自动初始化的Servlet名称
public class WebApplicationInitializer implements ServletContainerInitializer {
    @Override
    public void onStartup(Set<Class<?>> set, ServletContext servletContext) throws ServletException {
        ServletRegistration.Dynamic registration = servletContext.addServlet("HelloServlet", "com.yootk.servlet.HelloServlet");
        registration.addMapping("/hello.action");
    }
}
```

‍

‍

‍

‍

‍

## 消息日志

‍

除了一般日志, Servlet API 还提供了一个简单的输出信息的方式: log() 方法

```java
// 调用两个 ServletContext.log 方法
        ServletContext context = getServletContext( );

        if (par == null || par.equals(""))

        // 通过 Throwable 参数记录版本
            context.log("No message received:",
                new IllegalStateException("Missing parameter"));
        else
            context.log("Here is the visitor's message: " + par);
```

‍

ServletContext 把它的文本消息记录到 Servlet 容器的日志文件中

对于 Tomcat，这些日志可以在 <Tomcat-installation-directory>/logs 目录中找到

这些日志文件确实对新出现的错误或问题的频率给出指示

正因为如此，建议在通常不会发生的异常的 catch 子句中使用 log() 函数

‍

## JDB调试器

‍

可以使用调试 applet 或应用程序的 jdb 命令来调试 Servlet

为了调试一个 Servlet，我们可以调试 sun.servlet.http.HttpServer，然后把它看成是 HttpServer 执行 Servlet 来响应浏览器端的 HTTP 请求

‍

这与调试 applet 小程序非常相似

与调试 applet 不同的是，实际被调试的程序是 sun.applet.AppletViewer

大多数调试器会自动隐藏如何调试 applet 的细节. 同样的，对于 servlet，您必须帮调试器执行以下操作：

* 设置您的调试器的类路径 classpath，以便它可以找到 sun.servlet.http.Http-Server 和相关的类
* 设置您的调试器的类路径 classpath，以便它可以找到您的 servlet 和支持的类，通常是在 server_root/servlets 和 server_root/classes 中

您通常不会希望 server_root/servlets 在您的 classpath 中，因为它会禁用 servlet 的重新加载

但是这种包含规则对于调试是非常有用的

它允许您的调试器在 HttpServer 中的自定义 Servlet 加载器加载 Servlet 之前在 Servlet 中设置断点

如果您已经设置了正确的类路径 classpath，就可以开始调试 sun.servlet.http.HttpServer

可以在您想要调试的 Servlet 代码中设置断点，然后通过 Web 浏览器使用给定的 Servlet（http://localhost:8080/servlet/ServletToDebug）向 HttpServer 发出请求. 您会看到程序执行到断点处会停止

‍

## HTTP响应报头

‍

Java Servlet **HttpServletResponse** 类提供了以下方法来设置 HTTP 响应报头

‍

|序号|方法 & 描述|
| :-----| :---------------------------------------------------------------|
|1|**String encodeRedirectURL(String url)** <br />为 sendRedirect 方法中使用的指定的 URL 进行编码|
|2|**String encodeURL(String url)** <br />对包含 session 会话 ID 的指定 URL 进行编码|
|3|**boolean containsHeader(String name)** <br />返回一个布尔值，指示是否已经设置已命名的响应报头|
|4|**boolean isCommitted()** <br />返回一个布尔值，指示响应是否已经提交|
|5|**void addCookie(Cookie cookie)** <br />把指定的 cookie 添加到响应|
|6|**void addDateHeader(String name, long date)** <br />添加一个带有给定的名称和日期值的响应报头|
|7|**void addHeader(String name, String value)** <br />添加一个带有给定的名称和值的响应报头|
|8|**void addIntHeader(String name, int value)** <br />添加一个带有给定的名称和整数值的响应报头|
|9|**void flushBuffer()** <br />强制任何在缓冲区中的内容被写入到客户端|
|10|**void reset()** <br />清除缓冲区中存在的任何数据，包括状态码和头|
|11|**void resetBuffer()** <br />清除响应中基础缓冲区的内容，不清除状态码和头|
|12|**void sendError(int sc)** <br />使用指定的状态码发送错误响应到客户端，并清除缓冲区|
|13|**void sendError(int sc, String msg)** <br />使用指定的状态发送错误响应到客户端|
|14|**void sendRedirect(String location)** <br />使用指定的重定向位置 URL 发送临时重定向响应到客户端|
|15|**void setBufferSize(int size)** <br />为响应主体设置首选的缓冲区大小|
|16|**void setCharacterEncoding(String charset)** <br />设置被发送到客户端的响应的字符编码（MIME 字符集）例如，UTF-8|
|17|**void setContentLength(int len)设置在 HTTP Servlet**<br />响应中的内容主体的长度，该方法设置 HTTP Content-Length 头|
|18|**void setContentType(String type)** <br />如果响应还未被提交，设置被发送到客户端的响应的内容类型|
|19|**void setDateHeader(String name, long date)** <br />设置一个带有给定的名称和日期值的响应报头|
|20|**void setHeader(String name, String value)** <br />设置一个带有给定的名称和值的响应报头|
|21|**void setIntHeader(String name, int value)** <br />设置一个带有给定的名称和整数值的响应报头|
|22|**void setLocale(Locale loc)** <br />如果响应还未被提交，设置响应的区域|
|23|**void setStatus(int sc)** <br />为该响应设置状态码|

‍

```java
@WebServlet(name = "RefreshServlet", urlPatterns = {"auto_refresh"}, loadOnStartup = 1)
public class RefreshServlet extends HttpServlet {
    @Serial
    private static final long serialVersionUID = 1L;

    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        // 设置刷新自动加载的事件间隔为 1 秒
        response.setIntHeader("Refresh", 1);

        // 设置响应内容类型
        response.setContentType("text/html;charset=UTF-8");

        // 获取当前的时间
        Calendar calendar = new GregorianCalendar();
        String am_pm;
        int hour = calendar.get(Calendar.HOUR);
        int minute = calendar.get(Calendar.MINUTE);
        int second = calendar.get(Calendar.SECOND);
        if(calendar.get(Calendar.AM_PM) == Calendar.AM)
            am_pm = "AM";
        else
            am_pm = "PM";

        String CT = hour+":"+ minute +":"+ second +" "+ am_pm;

        PrintWriter out = response.getWriter();
        String title = "使用 Servlet 自动刷新页面 | 简单教程(www.twle.cn)";
        String docType = "<!DOCTYPE html> \n";
        out.println(docType +
                "<title>" + title + "</title>"+
                "<body bgcolor=\"#f0f0f0\">" +
                "<p>" + title + "</p>" +
                "<p>当前时间是：" + CT + "</p>");
    }

}
```

‍

‍

## 异步

传统的Servlet 在进行用户请求处理与响应时都是在一个线程上完成的，这样当处理与响应时间过长时就会造成该线程的长期占用，从而导致系统资源耗尽的问题，为了解决此类问题，在Servlet 3.0标准中提供了异步响应支持，可以由开发者自行创建一个异步线程进行客户端响应

‍

### 异步支持方法

在Servlet类中提供了AsyncContext接口进行异步响应操作，而要想获取此接口的实例化对象，就必须通过ServletRequest接口实现

|No.|方法|类型|描述|
| -----| -------------------------------------------------------------------------------------------------------------------------------| ------| --------------------|
|1|public AsyncContext getAsyncContext()|方法|获取异步上下文|
|2|public boolean isAsyncStarted()|方法|是否启动异步上下文|
|3|public boolean isAsyncSupported()|方法|是否支持异步处理|
|4|public AsyncContext startAsync()  throwsIllegalStateException|方法|开启异步上下文|
|5|public AsyncContext  startAsync(ServletRequest servletRequest, ServletResponse servletResponse)  throws IllegalStateException|方法|开启异步上下文|

异步的响应机制是提高用户处理性能的一种有效手段，但是当前所讲解的操作结构属于JakartaEE标准处理形式，而在后续学习到Spring开发框架的时候，还会继续学习到"Reactive”

‍

‍

### 请求响应

jakarta.servlet.AsyncContext接口是实现异步响应处理的核心接口，此接口的对象实例通过Servlet创建，在该接口进行异步处理时，必须明确的传递一个Runnable接口实例，而后可以直接在run()方法中进行请求的接收与响应处理

‍

#### **AsyncContext接口方法**

‍

由于最终的异步响应全部由AsyncContext接口完成，同时在响应处理时还需要明确的获取ServletRequest、ServletResponse接口实例

|No.|方法|类型|描述|
| -----| --------------------------------------------------| ------| -----------------------------|
|1|public void start(Runnable run)|方法|启动一个异步处理的子线程|
|2|public void complete()|方法|异步线程处理完毕|
|3|public ServletRequest getRequest()|方法|获取ServletRequest接口实例|
|4|public ServletResponse getResponse()|方法|获取ServletResponse接口实例|
|5|public void dispatch(String path)|方法|服务器端跳转|
|6|public void addListener(AsyncListener  listener)|方法|设置异步响应监听|
|7|public void setTimeout(long timeout)|方法|设置响应超时时间|
|8|public longgetTimeout()|方法|获取响应超时时间|

‍

‍

#### 示例

```java
@WebServlet(value = "/async", asyncSupported = true) // 默认情况下是不开启异步支持的
public class AsyncServlet extends HttpServlet {
    private class WorkerThread implements Runnable {   // 创建一个线程类
        private AsyncContext asyncContext; // 负责最终的异步处理
        public WorkerThread(AsyncContext asyncContext) {
            this.asyncContext = asyncContext;
        }
        @Override
        public void run() { // 进行异步响应
            System.err.println("【WorkerThread.run()】ThreadName = " + Thread.currentThread().getName());
            String msg = this.asyncContext.getRequest().getParameter("info"); // 获取当前用户的请求参数
            try {
                TimeUnit.SECONDS.sleep(2); // 模拟处理延迟
                // 通过异步上下文获取一个响应对象，并且输出响应内容，但是对于异步处理完成之后需要给出一个信号
                this.asyncContext.getResponse().getWriter().println("<h1>【ECHO】" + msg +"</h1>");
                this.asyncContext.complete(); // 异步处理完成
            } catch (Exception e) {
                e.printStackTrace();
            }
        }

    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.err.println("【AsyncServlet.doGet()】ThreadName = " + Thread.currentThread().getName());
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        // 所有的AsyncContext相关的处理操作全部都是在ServletRequest接口中定义
        if (req.isAsyncSupported()) { // 判断当前是否支持有异步请求处理能力
            AsyncContext asyncContext = req.startAsync(); // 创建异步处理的上下文对象
            asyncContext.start(new WorkerThread(asyncContext)); // 启动异步线程
        } else {
            resp.getWriter().println("<h1>骚蕊，我还不支持异步请求处理，请放过我吧！</h1>");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}

```

‍

‍

‍

### 响应监听

‍

为方便Servlet进行异步处理线程的操作与控制，在进行异步响应结构设计时，专门提供了一个监听接口，利用这个接口可以方便的实现异步处理线程的若干状态的监听，例如：异步开启监听（StartAsync）、异步处理完毕监听（Complete）、错误响应监听（Error）、超时监听（Timeout），为便于用户实现异步监听，专门提供了AsyncListener接口

‍

#### 示例

```java
@WebServlet(value = "/async", asyncSupported = true) // 默认情况下是不开启异步支持的
public class AsyncServlet extends HttpServlet {
    private class WorkerThread implements Runnable {   // 创建一个线程类
        private AsyncContext asyncContext; // 负责最终的异步处理
        public WorkerThread(AsyncContext asyncContext) {
            this.asyncContext = asyncContext;
        }
        @Override
        public void run() { // 进行异步响应
            System.err.println("【WorkerThread.run()】ThreadName = " + Thread.currentThread().getName());
            String msg = this.asyncContext.getRequest().getParameter("info"); // 获取当前用户的请求参数
            try {
                TimeUnit.SECONDS.sleep(2); // 模拟处理延迟
                // 通过异步上下文获取一个响应对象，并且输出响应内容，但是对于异步处理完成之后需要给出一个信号
                this.asyncContext.getResponse().getWriter().println("<h1>【ECHO】" + msg +"</h1>");
                this.asyncContext.complete(); // 异步处理完成
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    private class WorkerAsyncListener implements AsyncListener {   // 异步监听操作
        @Override
        public void onComplete(AsyncEvent asyncEvent) throws IOException {
            System.out.println("【WorkerAsyncListener.onComplete()】异步线程处理完毕，接收参数内容：" +
                    asyncEvent.getSuppliedRequest().getParameter("info"));
        }
        @Override
        public void onTimeout(AsyncEvent asyncEvent) throws IOException {
            System.out.println("【WorkerAsyncListener.onTimeout()】异步处理时间超时，接收参数内容：" +
                    asyncEvent.getSuppliedRequest().getParameter("info"));
        }
        @Override
        public void onError(AsyncEvent asyncEvent) throws IOException {
            System.out.println("【WorkerAsyncListener.onError()】异步线程处理错误，接收参数内容：" +
                    asyncEvent.getSuppliedRequest().getParameter("info"));
        }
        @Override
        public void onStartAsync(AsyncEvent asyncEvent) throws IOException {
            System.out.println("【WorkerAsyncListener.onStartAsync()】开启了一个异步处理线程，接收参数内容：" +
                    asyncEvent.getSuppliedRequest().getParameter("info"));
        }
    }

    @Override   // 进行GET请求的处理
    protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        System.err.println("【AsyncServlet.doGet()】ThreadName = " + Thread.currentThread().getName());
        req.setCharacterEncoding("UTF-8");
        resp.setCharacterEncoding("UTF-8");
        resp.setContentType("text/html;charset=UTF-8");
        // 所有的AsyncContext相关的处理操作全部都是在ServletRequest接口中定义
        if (req.isAsyncSupported()) { // 判断当前是否支持有异步请求处理能力
            AsyncContext asyncContext = req.startAsync(); // 创建异步处理的上下文对象
            asyncContext.addListener(new WorkerAsyncListener()); // 绑定异步监听
            asyncContext.setTimeout(5000); // 200毫秒应该结束异步操作
            asyncContext.start(new WorkerThread(asyncContext)); // 启动异步线程
        } else {
            resp.getWriter().println("<h1>骚蕊，我还不支持异步请求处理，请放过我吧！</h1>");
        }
    }

    @Override
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
        this.doGet(req, resp);
    }
}

```

‍

‍

‍

‍

### 文件上传下载

‍

#### 文件上传

‍

##### 实操

前端开发

1）表单的提交方法必须是post

2）需要声明是一个文件上传组件

3）必须设置表单的enctype="multipart/form-data

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>小滴课堂文件上传样例</title>
</head>
<body>

<form action="<%=request.getContextPath()%>/fileUpload" method="post" enctype="multipart/form-data">

    用户名:<input type="text" name="username"/>
    头像:<input type="file" name="img">
    <input type="submit" value="提交">

</form>
</body>
</html>

```

‍

后端开发

```java
@WebServlet("/fileUpload")
@MultipartConfig
public class FileUploadServlet extends HttpServlet {

    @Override
    protected void doPost(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        String username = request.getParameter("username");

        System.out.println("username="+username);

        Part part = request.getPart("img");

        //获取真实文件名称
        String header = part.getHeader("content-disposition");
        String realFileName = header.substring(header.indexOf("filename=")+10,header.length()-1);

        System.out.println("realFileName="+realFileName);


        //获取真实的文件内容
        InputStream is =  part.getInputStream();

        //web-inf目录外界不能直接访问，如果文件机密性强则放这里
        //String dir = this.getServletContext().getRealPath("/WEB-INF/file");

        String dir = this.getServletContext().getRealPath("/file");

        File dirFile = new File(dir);

        //如果目录不存在，则创建
        if(!dirFile.exists()){
            dirFile.mkdirs();
        }

        String uniqueName = UUID.randomUUID()+realFileName;

        //文件流拷贝
        //File file = new File(dir,realFileName);

        File file = new File(dir,uniqueName);

        FileOutputStream out = new FileOutputStream(file);

        byte[] buf = new byte[1024];
        int len;

        while ((len = is.read(buf))!=-1 ){
            out.write(buf,0,len);
        }
        out.close();
        is.close();

        //图片访问
        request.getRequestDispatcher("/file/"+uniqueName).forward(request,response);
    }
}
```

‍

注意点

* 考虑上传文件存储的目录
* 防止文件重名覆盖，防止一个目录下面出现太多文件，限制上传文件的最大值，上传的文件判断后缀名是否合法

‍

‍

#### 文件下载

‍

javaweb文件下载

* 只需通过超链接即可实现，就是通过超链接，在连接地址里写上文件的路径，浏览器会自动打开该文件
* 普通的文本，图片等浏览器能直接显示内容的浏览器都能直接打开并显示
* 如果是浏览器无法打开的文件，比如exe等浏览器就会提示你下载改文件或者使用当前系统自带的工具打开该文件

‍

‍

##### 实操

**后端开发**

* 客户端发送请求给服务端告诉服务端需要下载的文件，服务端读取该文件转换为输入流，在通过outputstream响应给客户端，需要设置response的头信息

‍

```java
@WebServlet("/download")
public class FileDownloadServlet extends HttpServlet {

    @Override
    protected void doGet(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        //客户端传递需要下载的文件名
        String file = request.getParameter("file");


        //获取文件在我们项目中的路径,发布到tomcat的实际路径
        String path = request.getServletContext().getRealPath("/file/");

        String filePath = path+file;

        FileInputStream fis = new FileInputStream(filePath);

        response.setCharacterEncoding("UTF-8");

        //指明响应的配置信息，包含附件
        response.setHeader("Content-Disposition","attachment; filename="+file);

        //如果文件名不包含中文可以不设置该项
        //如果包含中文名，则需要设置编码，否则文件名下载后中文字符会乱码
        //getBytes指定了编码的方式，ISO-8859-1指定了解码（读取）的方式,想要转换编码，就是先编码，再解码
        //response.setHeader("Content-Disposition","attachment; filename="+new String(file.getBytes("gb2312"),"ISO-8859-1"));

        ServletOutputStream out = response.getOutputStream();

        byte[] buf = new byte[1024];
        int len;

        while ((len= fis.read(buf))!=-1){
            out.write(buf,0,len);
        }

        out.close();

    }
}
```

‍

前端开发

```java
<a href="<%=request.getContextPath()%>/download?file=test1.png">下载</a>
```

‍

‍

## Cookie

‍

### 概念

‍

**背景**

HTTP协议作是无状态协议，无状态指每次request请求之前是相互独立的，当前请求并不会记录它的上一次请求信息.  存在这样的问题，既然无状态，那完成一套完整的业务逻辑，需要发送多次请求 --标识这些请求都是同个浏览器操作

‍

**解决方案**

* 浏览器发送request请求到服务器，服务器除了返回请求的response之外，还给请求分配一个唯一标识ID和response一并返回给浏览器
* 服务器在本地创建一个map结构，专门以key-value存储这个ID标识和浏览器的关系
* 当浏览器的第一次请求后已经分配一个ID，当第二次访问时会自动带上这个标识ID，服务会获取这个标识ID去map里面找上一次request的信息状态且做对应的更新操作 服务端生成这个全局的唯一标识，传递给客户端用于标记这次请求就是cookie； 服务器创建的那个map结构就是session.
* cookies由服务端生成，用于标记客户端的唯一标识，在每次网络请求中，都会被传送.
* session服务端自己维护的一个map数据结构，记录key-Object上下文内容状态
* 核心：它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态. Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能.  浏览器查看多个站点的cookie

‍

### 属性

* Name : 名称
* Value : 值
* Domain：表示当前cookie所属于哪个域或子域下面
* Expires/Max-age：表示了cookie的有效期，是一个时间，过了这个时间，该cookie就失效了
* Path：表示cookie的所属路径.
* size: 大小，多数浏览器都是4000多个字节
* http-only: 表示这个cookie不能被客户端使用js读取到，是不公开的cookie

  * (chrome调试器的console中输入document.cookie将得不到标记为HttpOnly的字段)
* Secure: 标记为 Secure 的Cookie只应通过被HTTPS协议加密过的请求发送给服务端,

  * 从 Chrome 52 和 Firefox 52 开始，不安全的站点（http:）无法使用Cookie的 Secure 标记
* SameSite(特有的，可以忽略)

‍

### 缺陷

* cookie会被附加在每个HTTP请求中，增加了流量.
* 在HTTP请求中的cookie是明文传递的，所以安全性成问题，除非用HTTPS
* Cookie的大小有限制，对于复杂的存储需求来说不满足
* 如果cookie泄露，你猜猜会发生什么问题？

‍

### 实操

‍

**javaweb操作浏览器cookie**

‍

* 获取请求的cookie

```java
  Cookie[] cookies = request.getCookies();
  for(Cookie cookie : cookies){
     cookie.getDomain();
  }

    private final String name;
      private String value;

      private int version = 0; // ;Version=1 ... means RFC 2109 style
  
      //
      // Attributes encoded in the header's cookie fields.
      //
      private String comment; // ;Comment=VALUE ... describes cookie's use
      private String domain; // ;Domain=VALUE ... domain that sees cookie
      private int maxAge = -1; // ;Max-Age=VALUE ... cookies auto-expire
      private String path; // ;Path=VALUE ... URLs that see the cookie
      private boolean secure; // ;Secure ... e.g. use SSL
      private boolean httpOnly; // Not in cookie specs, but supported by browsers
```

响应返回cookie

```java
Cookie cookie = new Cookie("token","sfwerawefewadaewfafewafa");
//20秒过期时间，过期后不会自动携带过去
cookie.setMaxAge(20);
response.addCookie(cookie);
request.getRequestDispatcher("/index.jsp").forward(request,response);
```

* js获取cookie，可以获取token，但是获取不到JSESSIONID,因为 http-only原因

‍

‍

## Session

‍

‍

### 概念

cookie和session都是为了弥补http协议的无状态特性，对server端来说无法知道两次http请求是否来自同一个用户，利用cookie和session就可以让server端知道多次http请求是否来自同一用户

‍

‍

### 使用

（和Cookie知识点一样，两者互相配合）

* 浏览器第一次发送request请求到服务器，服务器除了返回请求的response之外，还给请求分配一个唯一标识sessionId和response一并返回给浏览器
* 服务器在本地创建一个map结构，专门以key-value存储这个sessionId和浏览器的关系
* 当浏览器的第一次请求后已经分配一个sessionId，当第二次访问时会自动带上这个标识sessionId
* 服务器通过查找这个sessionId就知道用户状态了，并更新sessionId的最后访问时间.
* 注意： Session是有时限性的：比如如果30分钟内某个session都没有被更新，服务器就会删除这个它.

‍

### 总结

* 服务端生成这个全局的唯一标识，传递给客户端用于标记这次请求就是cookie；
* 服务器创建的那个map结构就是session.
* cookies由服务端生成，用于标记客户端的唯一标识，在每次网络请求中，都会被传送.
* session服务端自己维护的一个map数据结构，记录key-Object上下文内容状态
* 总言之cookie是保存在客户端，session是存在服务器，session依赖于cookie
* cookie里面存储的就是JSESSIONID

‍

### 实操

‍

#### javaweb会话技术HttpSession用户登录实战

‍

‍

HttpSession 类操作api

```java
HttpSession session = request.getSession();

//获取sessionid，java里面叫jsessionid
System.out.println("sessionid="+session.getId());
  
//创建时间戳,毫秒
 System.out.println("getCreationTime="+session.getCreationTime());
  
//是否是初次创建，记得情况浏览器的cookie,验证sessionid
System.out.println("isNew="+session.isNew());

//往session存储东西
session.setAttribute("name","小滴课堂 xdclass.net");
```

‍

登录Servlet实战

```java
    protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
  
        String name = req.getParameter("name");
        String pwd = req.getParameter("pwd");
  
        if(name.equals("xdclass") && pwd.equals("123")){
  
            User user = new User();
            user.setId(121);
            user.setName(name);
            user.setHost("xdclass.net");

            req.getSession().setAttribute("loginUser",user);
            req.getRequestDispatcher("/WEB-INF/user.jsp").forward(req,resp);
  
        }else{
            req.setAttribute("msg","账号密码错误");
            req.getRequestDispatcher("/login.jsp").forward(req,resp);
        }


    }
```

‍

* 登录Jsp

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
<form action="<%=request.getContextPath()%>/loginServlet" method="post">
    名称：<input type="text" name="name"/>
    <br/>
    密码：<input type="password" name="pwd"/>
    <input type="submit" value="登录">
    消息提示 ${msg}
</form>
</body>
</html>
```

‍

* 登录成功页面

```html
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
id:${loginUser.id}
<br>
name:${loginUser.name}
<a href="/logout_servlet">退出</a>
</body>
</html>
```

‍

* 退出登录实现方式: session.invalidate();
* 过期时间配置web.xml,单位分钟

‍

```html
<session-config>
    <session-timeout>30</session-timeout>
</session-config>
```

‍

‍

## 重定向

‍

‍

请求重定向

* 客户端发送请求，Servlet做出业务逻辑处理
* Servlet调用response.sendRedirect("xxx.jsp")方法，把要访问的目标资源作为response响应信息发给客户端浏览器
* 客户端浏览器重新访问服务器资源xx.jsp，服务器再次对客户端浏览器做出响应
* 请求重定向，不能访问WEB-INF下的文件，浏览器上的窗口地址会改版，可以用于跳转第三方地址或者应用里面的其他Servelt、jsp等
* 例子： response.sendRedirect("/WEB-INF/admin.jsp");

‍

注意点

* 重定向是取不到request中的存储的数据,如果当前servlet是**重定向，浏览器可以看到两个请求**

  * 案例测试：在reqeust中设置值，然后在请求转发到页面，使用EL表达式取值
* 调用sendRedirect()方法，会在响应中设置Location响应报头，这个过程对于用户来说是透明的，浏览器会自动完成新的访问
* 重定向路径问题：如果没有加 http 开头，则认为是当前应用里面的servlet重定向，默认加上应用上下文；如果有加http则会使用配置的全路径进行跳转
* 如果请求转发可以满足需要时，尽量使用请求转发，而不是重定向，效率性能更好(取值和转发)

‍

### 请求转发

```html
request.getRequestDispatcher(URL地址).forward(request, response)
```

* 客户端发送请求，Servlet做出业务逻辑处理.
* Servlet调用forword()方法，服务器Servlet把目标资源返回给客户端浏览器
* 可以访问WEB-INF下的文件,WEB-INF的文件一般是需要一定的权限才可以访问
* 例子：req.getRequestDispatcher("/WEB-INF/admin.jsp").forward(req,resp);

‍

* 注意点：在浏览器地址栏中不会显示出转发后的地址，属于服务器内部转发，整个过程处于同一个请求当中，所以转发中数据的存取可以用request作用域

```html
//存储java对象到request作用域，转发后一样可以获取到值，具体怎么获取？JSP或者EL表达式
request.setAttribute("name","jack");
```

‍

### 请求流程

```html
浏览器->Tomcat服务器(Servlet1->Servlet2)->浏览器
```

‍

## WEB-INF目录

‍

### 存放模板文件

WEB-INF目录不允许浏览器直接访问，所以我们的视图模板文件放在这个目录下，是一种保护。以免外界可以随意访问视图模板文件。

访问WEB-INF目录下的页面，都必须通过Servlet转发过来，简单说就是：不经过Servlet访问不了。  
这样就方便我们在Servlet中检查当前用户是否有权限访问。  
那放在WEB-INF目录下之后，重定向进不去怎么办？  
重定向到Servlet，再通过Servlet转发到WEB-INF下。

‍

## EL表达式

‍

让JSP访问JavaBean中的数据更简单

‍

### **简介**

‍

全称 Expression Language，让JSP写起来更加简单. 表达式语言的灵感来自于 ECMAScript 和 XPath 表达式语言，它提供了在 JSP 中简化表达式的方法，让Jsp的代码更加简化

‍

‍

### 语法

EL表达式的格式都是以 { }表示. 例如  {userinfo}代表获取变量userinfo的值，\${对象.属性}，可以有多层操作

当EL表达式中的变量不给定范围时，则默认在page范围查找，然后依次在request、session、application范围查找，如果找到不再继续找下去，但是假如全部的范围都没有找到时，就回传""

可以用范围作为前缀表示属于哪个范围的变量，例如：\${pageScope.userinfo}表示访问page范围中的userinfo变量

‍

属性范围在EL中的名称

【jsp中】    【EL表达式中】  
Page        pageScope

Request    requestScope

Session    sessionScope

Application    applicationScope

‍

对比在jsp中取值

‍

​`<%= (String)request.getAttribute("name")%> `​等价于 `${name}`​

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

# 实操

‍

XD

## **面试题**

‍

* 说下servlet的生命周期

  * 实例化->使用构造方法创建对象
  * 初始化->执行init方法：Servlet 的生命期中，仅执行一次 init() 方法，它是在服务器装入 Servlet 时执行的,即第一次访问这个Servlet才执行
  * 服务->执行service方法，service() 方法是 Servlet 的核心. 每当一个客户请求一个HttpServlet 对象，该对象的service() 方法就要被调用
  * 销毁-> 执行destroy方法,destroy() 方法仅执行一次，即在服务器停止且卸装 Servlet 时执行该方法
* Servlet API中forward()和redirect()的区别

  * 重定向会改变URL地址，请求转发不会改变URL地址
  * 重定向不可以使用多个作用域的内容，请求转发可以
  * 重定向可以用URL访问外部资源，请求转发只能跳转内部资源
  * 重定向会触发多次请求；转发的话只在内部跳转
* 说下Cookie和Session的区别和联系

  * cookie数据保存在客户端，session数据保存在服务端
  * cookie不是很安全，容易泄露，不能直接明文存储信息
  * Cookie大小和数量存储有限制
* 客户端存储除了Cookie，还有什么？

  * localStroage
  * sessionStorage

‍

‍

‍

## Bugfix

‍

‍

‍

‍
