--代表性模板引擎

‍

‍

Thymeleaf是适用于Web和独立环境的现代服务器端(Java模板引擎)

当今Axios更流行, 但是这个模板引擎也要用到

Thymeleaf就是jsp的升级版，是一个模板引擎(java语言编写)，可以处理html、xml、js、css等等，特别是html5. 既可以展示静态数据也可以用后端传的数据直接展示，也可以实现前后端分离. 

轻量级的模板引擎（复杂逻辑业务的不推荐，解析DOM或者XML会占用多的内存） 可以直接在浏览器中打开且正确显示模板页面  
直接是html结尾，直接编辑xdlcass.net/user/userinfo.html  

‍

### Header

‍

# 知识

‍

‍

## **模板引擎**

‍

**模板引擎**（特指用于Web开发的模板引擎）是为了使用户界面与业务数据（内容）分离而产生的，它可以生成特定格式的文档，用于网站的模板引擎就会生成一个标准的html文档. 最重要的就是模板二字，这个意思就是做好一个模板后套入对应位置的数据，最终以html的格式展示出来

‍

在Java中模板引擎还有很多，模板引擎是动态网页发展进步的产物，在最初并且流传度最广的​jsp​它就是一个模板引擎. jsp是官方标准的模板，但是由于jsp的缺点比较多也挺严重的，所以很多人弃用jsp选用第三方的模板引擎，市面上开源的第三方的模板引擎也比较多，有**Thymeleaf、FreeMaker、Velocity**等模板引擎受众较广. 

模板引擎在web领域的主要作用：让网站实现界面和数据分离，这样大大提高了开发效率，让代码重用更加容易

‍

## 物理视图和逻辑视图

‍

* 物理视图

  * 在Servlet中，将请求转发到一个HTML页面文件时，使用的完整的转发路径就是物理视图。eg.`/pages/user/login_success.html`​​
  * 如果我们把所有的HTML页面都放在某个统一的目录下，那么转发地址就会呈现出明显的规律：`/pages/user/login.html`​​、`/pages/user/login_success.html`​​、`/pages/user/regist.html /pages/user/regist_success.html`​​
  * 路径的开头都是：/pages/user/，路径的结尾都是：.html
  * 所以，路径开头的部分我们称之为视图前缀，路径结尾的部分我们称之为视图后缀。
* 逻辑视图

  * 物理视图=视图前缀+逻辑视图+视图后缀

‍

## 特点

‍

‍

### 评价

‍

‍

Thymeleaf作为被Springboot官方推荐的**模板引擎**

* **动静分离：**  Thymeleaf选用html作为模板页，这是任何一款其他模板引擎做不到的！Thymeleaf使用html通过一些特定标签语法代表其含义，但并未破坏html结构，即使无网络、不通过后端渲染也能在浏览器成功打开，大大方便界面的测试和修改.
* **开箱即用：**  Thymeleaf提供标准和Spring标准两种方言，可以直接套用模板实现JSTL、 OGNL表达式效果，避免每天套模板、改JSTL、改标签的困扰. 同时开发人员也可以扩展和创建自定义的方言.
* Springboot官方大力推荐和支持，Springboot官方做了很多默认配置，开发者只需编写对应html即可，大大减轻了上手难度和配置复杂度.

‍

动态页面每次修改打开都需要重新启动程序、输入链接，这个过程其实是相对漫长的. 如果界面设计人员用这种方式进行页面设计时间成本高并且很麻烦，可通过静态页面设计样式，设计完成通过服务端访问即可达成目标UI的界面和应用，达到动静分离的效果. 这个特点和优势是所有模板引擎中Thymeleaf所独有的！

‍

‍

Thymeleaf都是后端渲染的数据，渲染整个html文档，服务器压力大

与之对比Vue的只需要ajax请求数据，后端只需要渲染json数据就好了，服务器压力小很多. 

这种模板引擎底层绝对逃不开Servlet. 

‍

‍

# 基础

‍

## 搭建

‍

‍

### 依赖

‍

spring-boot-starter-thymeleaf

```html
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```

‍

### controller

Thymeleaf就是v(view)视图层，我们需要在controller中指定Thymeleaf页面的url，然后再Model中绑定数据

‍

在com.Thymeleaf文件下创建controller文件夹，在其中创建urlController.java的controller

```java
@Controller
public class urlController {
    @GetMapping("index")//页面的url地址
    public String getindex(Model model)//对应函数
    {
        model.addAttribute("name","bigsai");
        return "index";//与templates中index.html对应
    }
}
```

‍

‍

### **Thymeleaf页面**

在项目的resources目录下的templates文件夹下面创建一个叫index.html的文件

Thymeleaf是一个基于html的模板引擎，但是我们还是需要加入特定标签来声明和使用Thymeleaf的语法

```text
<html xmlns:th="http://www.thymeleaf.org">
```

‍

‍

#### 示例程序

```html
<!DOCTYPE html>
<html  xmlns:th="http://www.thymeleaf.org">
<head>
    <meta charset="UTF-8">
    <title>title</title>
</head>
<body>
hello 第一个Thymeleaf程序
<div th:text="${name}">name是bigsai(我是离线数据)</div>
</body>
</html>
```

‍

‍

### 示例

‍

‍

‍

## **标签**

‍

### **链接表达式**

 **@{…}**

‍

引入链接比如link，href，src

资源地址可以static目录下的静态资源，也可以是互联网中的绝对资源

‍

‍

#### **引入css**

```text
 <link rel="stylesheet" th:href="@{index.css}">
```

‍

#### **引入JavaScript**

```text
 <script type="text/javascript" th:src="@{index.js}"></script>
```

‍

#### **超链接**

```text
<a th:href="@{index.html}">超链接</a>
```

‍

‍

‍

‍

### **变量表达式**

 **${…}**

‍

通过${…}进行取值，这点和ONGL表达式语法一致

‍

如果在controller中的Model直接存储某字符串，我们可以直接`${对象名}`​进行取值

‍

取JavaBean对象也很容易，因为JavaBean自身有一些其他属性，就可以使用`${对象名.对象属性}`​或者`${对象名['对象属性']}`​来取值

除此之外，如果该JavaBean如果写了get方法，也可以通过get方法取值例如`${对象.get方法名}`​

‍

```html
<h2>JavaBean对象</h2>
<table bgcolor="#ffe4c4" border="1">
    <tr>
        <td>介绍</td>
        <td th:text="${user.name}"></td>
    </tr>
    <tr>
        <td>年龄</td>
        <td th:text="${user['age']}"></td>
    </tr>
    <tr>
        <td>介绍</td>
        <td th:text="${user.getDetail()}"></td>
    </tr>
</table>
```

‍

‍

#### **取List集合(each)**

List集合是个有序列表，里面内容可能不止一个，需要遍历List对其中对象取值，而遍历需要用到标签：`th:each`​

具体使用为`<tr th:each="item:${userlist}">`​, 其中item就相当于遍历每一次的对象名，在下面的作用域可以直接使用，而userlist就是你在Model中储存的List的名称

‍

```html
<h2>List取值</h2>
<table bgcolor="#ffe4c4" border="1">
    <tr th:each="item:${userlist}">
        <td th:text="${item}"></td>
    </tr>
</table>
```

‍

‍

‍

#### **直接取Map**

不存JavaBean而是将一些值放入Map中，再将Map存在Model中，我们就需要对Map取值，对于Map取值你可以`${Map名['key']}`​来进行取值

也可以通过`${Map名.key}`​取值，当然你也可以使用`${map.get('key')}`​(java语法)来取值

```html
<h2>Map取值</h2>
<table bgcolor="#8fbc8f" border="1">
    <tr>
        <td>place:</td>
        <td th:text="${map.get('place')}"></td>
    </tr>
    <tr>
        <td>feeling:</td>
        <td th:text="${map['feeling']}"></td>
    </tr>
</table>
```

‍

#### **遍历Map**

如果说你想遍历Map获取它的key和value那也是可以的，这里就要使用和List相似的遍历方法，使用`th:each="item:${Map名}"`​进行遍历，在下面只需使用`item.key`​和`item.value`​即可获得值

‍

```html
<h2>Map遍历</h2>
<table bgcolor="#ffe4c4" border="1">
    <tr th:each="item:${map}">
        <td th:text="${item.key}"></td>
        <td th:text="${item.value}"></td>
    </tr>
</table>
```

‍

‍

### **选择变量表达式**

 ***{…}**

‍

变量表达式不仅可以写成`${...}`​，而且还可以写成`*{...}`​. 

区别：星号语法对选定对象而不是整个上下文评估表达式. 也就是说，只要没有选定的对象，美元(`${…}`​)和星号(`*{...}`​)的语法就完全一样

选定对象是使用`th:object`​属性的表达式的结果

‍

```text
<div th:object="${user}">
    <p>Name: <span th:text="*{name}">赛</span>.</p>
    <p>Age: <span th:text="*{age}">18</span>.</p>
    <p>Detail: <span th:text="*{detail}">好好学习</span>.</p>
</div>
```

‍

当然`*{…}`​也可和`${…}`​混用. 

如果不使用选定对象，完全等价于

```text
<div >
    <p>Name: <span th:text="*{user.name}">赛</span>.</p>
    <p>Age: <span th:text="${user.age}">18</span>.</p>
    <p>Detail: <span th:text="${user.detail}">好好学习</span>.</p>
</div>
```

‍

‍

### **消息表达**

 **#{…}**

使用`#{...}`​语法获取消息

文本外部化是从模板文件中提取模板代码的片段，以便可以将它们保存在单独的文件(通常是.properties文件)中,文本的外部化片段通常称为“消息”

通俗易懂得来说`#{…}`​语法就是用来**读取配置文件中得数据**的

‍

‍

首先在templates目录下建立`home.properties`​

```text
bigsai.nane=bigsai
bigsai.age=22
province=Jiang Su
```

‍

在`application.properties`​中加入以下内容

```text
spring.messages.basename=templates/home
```

‍

这样就可以在Thymeleaf中读取配置的文件了

```text
<h2>消息表达</h2>
<table bgcolor="#ffe4c4" border="1">
    <tr>
    <td>name</td>
    <td th:text="#{bigsai.name}"></td>
    </tr>
    <tr>
    <td>年龄</td>
    <td th:text="#{bigsai.age}"></td>
    </tr>
    <tr>
    <td>province</td>
    <td th:text="#{province}"></td>
    </tr>
</table>
```

‍

‍

# 高级

‍

‍

‍

# 实操

‍

## Bugfix

‍

‍
