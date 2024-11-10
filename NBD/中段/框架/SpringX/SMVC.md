--Spring的Web框架

‍

‍

Servlet的再封装, 一种基于Java实现MVC模型的轻量级Web框架

在Spring框架的生态中，对web程序开发提供了很好的支持，如：全局异常处理器、拦截器这些都是Spring框架中web开发模块所提供的功能，而Spring框架的web开发模块也称为SpringMVC

SpringMVC不是一个单独的框架，它是Spring框架的一部分，是Spring框架中的web开发模块，是用来简化原始的Servlet程序开发的(对Servlet进行了封装)

优点: 使用简单、开发便捷(相比于Servlet), 灵活性强

‍

‍

### Heade​r​​​

‍

#### 环境

‍

#### 兼容

‍

##### JSON

导入jackson包

```java
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```

‍

# 知识

‍

## 概念

‍

### 任务

SpringMVC主要负责的就是

* controller如何接收请求和数据
* 如何将请求和数据转发给业务层
* 如何将响应数据转换成json发回到前端

‍

‍

‍

## 命名规范

* 控制层包名：xxxx.controller , 类名XXXController
* 业务逻辑层包名：xxxx.service, 接口名XXXService, 小包名impl(内含实现类)
* 数据访问层包名：xxxx.dao, 接口名XXXDao, 小包名impl(内含实现类)

‍

‍

## 请求执行流程

```yml
 Browser->C->S->D->S->C->Browser
```

1. 前端发起的请求，由Controller层接收（Controller响应数据给前端）
2. Controller层调用Service层来进行逻辑处理（Service层处理完后，把处理结果返回给Controller层）
3. Serivce层调用Dao层（逻辑处理过程中需要用到的一些数据要从Dao层获取）
4. Dao层操作文件中的数据（Dao拿到的数据会返回给Service层）

‍

==完整过程==

1. 浏览器发送一个请求会先到Tomcat的web服务器
2. Tomcat服务器接收到请求以后，判断请求的是静态资源还是动态资源
3. 静态资源    直接到Tomcat的项目部署目录下去直接访问    动态资源    交给项目的后台代码进行处理
4. 进入到到中央处理器(SpringMVC中的内容)，SpringMVC会根据配置的过滤器规则进行拦截
5. 如果满足规则则进行处理，找到其对应的controller类中的方法进行执行, 完成后返回结果
6. 在每个Controller方法执行的前后添加业务就是拦截器要做的事

‍

‍

servlet处理请求和数据的时候，存在的问题是一个servlet只能处理一个请求

‍

针对web层进行了优化，采用了MVC设计模式，将其设计为`controller`​、`view`​和`Model`​

* **controller**负责请求和数据的接收，接收后将其转发给service进行业务处理
* **service**根据需要会调用dao对数据进行增删改查
* **dao**把数据处理完后将结果交给service,service再交给controller
* controller根据需求组装成Model和View,Model和View组合起来生成页面转发给前端浏览器

  > 这样做的好处就是controller可以处理多个请求，并对请求进行分发，执行不同的业务操作. 
  >
  > 因为是异步调用，所以后端不需要返回view视图，将其去除
  >
  > 前端如果通过异步调用的方式进行交互，后台就需要将返回的数据转换成json格式进行返回
  >

‍

‍

‍

## 结构

‍

### 三层架构

C - S - D

Controller==控制层==        **接收请求响应数据**

Service==业务(逻辑)层==        **业务逻辑处理**

Dao==持久层== / 数据访问层           **数据访问操作**

‍

在三层架构当中，前端发起请求首先会到达Controller(不进行逻辑处理)，然后Controller会直接调用Service进行逻辑处理, Service再调用Dao完成数据访问操作. 

一些通用的业务处理借助于**JavaWeb**当中三大组件之一的过滤器**Filter**或者是**Spring**的拦截器**Interceptor**来实现. 

> Filter过滤器、Cookie、 Session这些都是传统的JavaWeb提供的技术. 
>
> JWT令牌、阿里云OSS对象存储服务，是现在企业项目中常见的一些解决方案. 
>
> IOC控制反转、DI依赖注入、AOP面向切面编程、事务管理、全局异常处理、拦截器等，这些技术都是 Spring Framework框架当中提供的核心功能. 
>
> Mybatis就是一个持久层的框架，是用来操作数据库的.

‍

‍

### **Web容器层次结构**

大->小

* ServletContext

  * WebApplicationContext

    * UserController  
      /save -> save()

‍

‍

## 评价

‍

‍

## 注解记录合集

‍

‍

### 基础

‍

#### @Controller

|名称|@Controller|
| ------| -------------------------------|
|类型|类注解|
|位置|SpringMVC控制器类定义上方|
|作用|设定SpringMVC的核心控制器bean|

‍

#### @RequestMapping

|名称|@RequestMapping|
| ----------| ---------------------------------|
|类型|类注解或方法注解|
|位置|SpringMVC控制器类或方法定义上方|
|作用|设置当前控制器方法请求访问路径|
|相关属性|value(默认)，请求访问路径|

‍

#### @ResponseBody

|名称|@ResponseBody|
| ------| --------------------------------------------------|
|类型|类注解或方法注解|
|位置|SpringMVC控制器类或方法定义上方|
|作用|设置当前控制器方法响应内容为当前返回值，无需解析|

‍

‍

#### @ComponentScan

|名称|@ComponentScan|
| ----------| -------------------------------------------------------------------------------------------------------------------------------------------------|
|类型|类注解|
|位置|类定义上方|
|作用|设置spring配置类扫描路径，用于加载使用注解格式定义的bean|
|相关属性|excludeFilters:排除扫描路径中加载的bean,需要指定类别(type)和具体项(classes)<br />includeFilters:加载指定的bean，需要指定类别(type)和具体项(classes)|

---

#### @RequestParam

|名称|@RequestParam|
| ----------| ----------------------------------------------------|
|类型|形参注解|
|位置|SpringMVC控制器方法形参定义前面|
|作用|绑定请求参数与处理器方法形参间的关系|
|相关参数|required：是否为必传参数<br />defaultValue：参数默认值|

‍

#### @EnableWebMvc

|名称|@EnableWebMvc|
| ------| ---------------------------|
|类型|==配置类注解==|
|位置|SpringMVC配置类定义上方|
|作用|开启SpringMVC多项辅助功能|

‍

#### @RequestBody

|名称|@RequestBody|
| ------| ----------------------------------------------------------------------------|
|类型|==形参注解==|
|位置|SpringMVC控制器方法形参定义前面|
|作用|将请求中请求体所包含的数据传递给请求参数，此注解一个处理器方法只能使用一次|

‍

#### @ResponseBody

|名称|@ResponseBody|
| ----------| -------------------------------------------------------------------------|
|类型|==方法\类注解==|
|位置|SpringMVC控制器方法定义上方和控制类上|
|作用|设置当前控制器返回值作为响应体,<br />写在类上，该类的所有方法都有该注解功能|
|相关属性|pattern：指定日期时间格式字符串|

‍

---

### 高级

‍

#### @RestController

|名称|@RestController|
| ------| -----------------------------------------------------------------------------------|
|类型|==类注解==|
|位置|基于SpringMVC的RESTful开发控制器类定义上方|
|作用|设置当前控制器类为RESTful风格，<br />等同于@Controller与@ResponseBody两个注解组合功能|

‍

#### @GetMapping @PostMapping @PutMapping @DeleteMapping

|名称|@GetMapping @PostMapping @PutMapping @DeleteMapping|
| ----------| ----------------------------------------------------------------------------------------------|
|类型|==方法注解==|
|位置|基于SpringMVC的RESTful开发控制器方法定义上方|
|作用|设置当前控制器方法请求访问路径与请求动作，每种对应一个请求动作，<br />例如@GetMapping对应GET请求|
|相关属性|value（默认）：请求访问路径|

‍

---

### 整合

‍

#### @RestControllerAdvice

|名称|@RestControllerAdvice|
| ------| ------------------------------------|
|类型|==类注解==|
|位置|Rest风格开发的控制器增强类定义上方|
|作用|为Rest风格开发的控制器类做增强|

**说明:** 此注解自带@ResponseBody注解与@Component注解，具备对应的功能

‍

#### @ExceptionHandler

|名称|@ExceptionHandler|
| ------| -------------------------------------------------------------------------------------------------|
|类型|==方法注解==|
|位置|专用于异常处理的控制器方法上方|
|作用|设置指定异常的处理方案，功能等同于控制器方法，<br />出现异常后终止原始控制器执行,并转入当前方法执行|

**说明：** 此类方法可以根据处理的异常不同，制作多个方法分别处理对应的异常

‍

‍

# 基础

‍

## 搭建

‍

@Controller

|名称|@Controller|
| ------| -------------------------------|
|类型|类注解|
|位置|SpringMVC控制器类定义上方|
|作用|设定SpringMVC的核心控制器bean|

‍

@RequestMapping

|名称|@RequestMapping|
| ----------| ---------------------------------|
|类型|类注解或方法注解|
|位置|SpringMVC控制器类或方法定义上方|
|作用|设置当前控制器方法请求访问路径|
|相关属性|value(默认)，请求访问路径|

‍

@ResponseBody

|名称|@ResponseBody|
| ------| --------------------------------------------------|
|类型|类注解或方法注解|
|位置|SpringMVC控制器类或方法定义上方|
|作用|设置当前控制器方法响应内容为当前返回值，无需解析|

‍

### 示例

‍

> pom.xml只导入了`spring-webmvc`​jar包的原因是它会自动依赖spring相关坐标

‍

‍

**请求处理**

Controller类加注解@RequestMapping("/save")处理请求

```java
@Controller
public class UserController {
  
    @RequestMapping("/save")
    @ResponseBody
    public String save(){
        System.out.println("user save ...");
        return "{'info':'springmvc'}";
    }
}
```

‍

**配置类**

使用配置类替换web.xml

​`ServletContainersInitConfig extends AbstractDispatcherServletInitializer`​

‍

AbstractDispatcherServletInitializer类是SpringMVC提供的快速初始化Web3.0容器的抽象类, 提供了三个接口方法供用户实现

* **createServletApplicationContext**  
  创建Servlet容器时，加载SpringMVC对应的bean并放入WebApplicationContext对象范围中，而WebApplicationContext的作用范围为ServletContext范围，即整个web容器范围
* **getServletMappings**  
  设定SpringMVC对应的请求映射路径，即SpringMVC拦截哪些请求
* **createRootApplicationContext**  
  如果创建Servlet容器时需要加载非SpringMVC对应的bean,使用当前方法进行，使用方式和createServletApplicationContext相同

‍

createServletApplicationContext()用来加载SpringMVC环境, 初始化WebApplicationContext对象并加载指定配置类

createRootApplicationContext()用来加载Spring环境

getServletMappings()指定MVC处理的请求路径

‍

```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {
   
    //加载springmvc配置类
    protected WebApplicationContext createServletApplicationContext() {
        //初始化WebApplicationContext对象
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        //加载指定配置类
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    //设置由springmvc控制器处理的请求映射路径
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //加载spring配置类
    protected WebApplicationContext createRootApplicationContext() {
        return null;
    }
}
```

‍

‍

## 工作流程

使用过程总共分两个阶段: `启动服务器初始化过程`​和`单次请求过程`​

‍

‍

### 总览

‍

> Servlet开发
>
> 1.创建web工程(Maven结构)
>
> 2.设置tomcat服务器，加载web工程(tomcat插件)
>
> 3.导入坐标(Servlet)
>
> 4.定义处理请求的功能类(UserServlet)
>
> 5.设置请求映射(配置映射关系)

‍

SpringMVC的制作过程

1.创建web工程(Maven结构)

2.设置tomcat服务器，加载web工程(tomcat插件)

3.导入坐标(==SpringMVC==+Servlet)

4.定义处理请求的功能类(==UserController==)

5.==设置请求映射(配置映射关系)==

6.==将SpringMVC设定加载到Tomcat容器中==

‍

‍

### 启动服务器初始化

‍

1. 服务器启动，执行ServletContainersInitConfig类，初始化web容器

    * 功能类似于以前的web.xml
2. 执行createServletApplicationContext方法，创建了WebApplicationContext对象

    * 该方法加载SpringMVC的配置类SpringMvcConfig来初始化SpringMVC的容器
3. 加载SpringMvcConfig配置类
4. 执行@ComponentScan加载对应的bean, 扫描指定包及其子包下所有类上的注解

    * 如Controller类上的@Controller注解
5. 加载UserController，每个@RequestMapping的名称对应一个具体的方法
6. 执行getServletMappings方法，设定SpringMVC拦截请求的路径规则

‍

‍

### 单次请求

‍

1. 发送请求`http://localhost/save`​
2. web容器发现该请求满足SpringMVC拦截规则，将请求交给SpringMVC处理
3. 解析请求路径/save
4. 由/save匹配执行对应的方法save(）

    * 上面的第五步已经将请求路径和方法**建立了对应关系**，通过/save就能找到对应的save方法
5. 执行save()
6. 检测到有@ResponseBody直接将save()方法的返回值作为响应体返回给请求方

‍

## bean加载控制

‍

‍

@ComponentScan

|名称|@ComponentScan|
| ----------| -------------------------------------------------------------------------------------------------------------------------------------------------|
|类型|类注解|
|位置|类定义上方|
|作用|设置spring配置类扫描路径，用于加载使用注解格式定义的bean|
|相关属性|excludeFilters:排除扫描路径中加载的bean,需要指定类别(type)和具体项(classes)<br />includeFilters:加载指定的bean，需要指定类别(type)和具体项(classes)|

‍

### 情境

> Q: 如何让Spring和SpringMVC分开加载各自的内容? /bean对象，让SpringMVC加载还是让Spring? /避免Spring错误加载到SpringMVC的bean?
>
> A: **Spring排除掉SpringMVC控制的bean**

* SpringMVC加载其相关bean(**表现层bean**),也就是**controller包下的类**

  * 在**SpringMVC**的配置类`SpringMvcConfig`​中`@ComponentScan`​ 将其扫描范围**设置到controller**
* Spring控制的bean

  * **业务bean(Service)**
  * **功能bean**(DataSource,SqlSessionFactoryBean,MapperScannerConfigurer等)
  * 在**Spring**的配置类`SpringConfig`​中`@ComponentScan`​ 与MVC的controller进行解耦

‍

不同实现**方式**

* Spring加载的bean设定扫描范围为**精准范围**，例如service包、dao包等
* Spring加载的bean设定扫描范围为com.itheima,**排除**掉controller包中的bean
* 不区分Spring与SpringMVC的环境, 堆在一起[了解即可]

‍

### 精准范围

```java
@Configuration
@ComponentScan({"com.itheima.service","comitheima.dao"})
public class SpringConfig {
}
```

> 上述只是通过例子说明可以精确指定让Spring扫描对应的包结构，真正在做开发的时候，因为Dao最终是交给`MapperScannerConfigurer`​对象来进行扫描处理的，只需要将其扫描到service包即可

‍

‍

### 范围排除

排除掉controller包中的bean

‍

@ComponentScan:

```java
excludeFilters=@ComponentScan.Filter(
    	type =          ,
        classes =       
    )
```

```java
@Configuration
@ComponentScan(value="com.itheima",
    excludeFilters=@ComponentScan.Filter(
    	type = FilterType.ANNOTATION,
        classes = Controller.class
    )
)
public class SpringConfig {
}
```

‍

@ComponentScan属性

excludeFilters属性：设置扫描加载bean时，排除的过滤规则

‍

@ComponentScan.Filter

* type属性：设置排除规则，当前使用按照bean定义时的注解类型进行排除

  1. **ANNOTATION：按照注解排除 (这个就够了)**
  2. ASSIGNABLE_TYPE:按照指定的类型过滤
  3. ASPECTJ:按照Aspectj表达式排除，基本上不会用
  4. REGEX:按照正则表达式排除
  5. CUSTOM:按照自定义规则排除

  ‍
* classes属性：设置排除的具体注解类，当前设置排除@Controller定义的bean

‍

### 加载配置类

在tomcat服务器启动时将配置类加载要修改ServletContainersInitConfig, 两种实现方法

‍

#### extends AbstractDispatcherServletInitializer

基础的配置上修改

```java
public class ServletContainersInitConfig extends AbstractDispatcherServletInitializer {

    protected WebApplicationContext createServletApplicationContext() {
        AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringMvcConfig.class);
        return ctx;
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    protected WebApplicationContext createRootApplicationContext() {
      AnnotationConfigWebApplicationContext ctx = new AnnotationConfigWebApplicationContext();
        ctx.register(SpringConfig.class);
        return ctx;
    }

}
```

‍

‍

#### extends AbstractAnnotationConfigDispatcherServletInitializer

==最终配置==, 不用再去创建`AnnotationConfigWebApplicationContext`​对象，不用手动`register`​对应的配置类

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {

    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }

    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }

    protected String[] getServletMappings() {
        return new String[]{"/"};
    }
}
```

‍

‍

‍

## 请求

‍

‍

#### 类型

‍

‍

##### GET

‍

发送请求与参数

```java
    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(String name,int age){
        System.out.println("普通参数传递 name ==> "+name);
        System.out.println("普通参数传递 age ==> "+age);
        return "{'module':'commonParam'}";
```

```
http://localhost/commonParam?name=itcast
```

‍

‍

##### POST

和GET一致，不用做任何修改

‍

‍

‍

‍

### 映射路径

‍

#### 格式

> Q: 团队多人开发，每人设置不同的请求路径，冲突问题该如何解决?
>
> A: 为不同模块**设置模块名作为请求路径前置**

‍

例如

> 对于Book模块的save,将其访问路径设置`http://localhost/book/save`​

‍

‍

#### 优化

优化路径配置: 抽取公有访问参数到方法头

```java
@RequestMapping("/user")
public class UserController {
```

‍

* @RequestMapping注解value属性前面加不加`/`​都可以

‍

‍

### 参数类型

‍

* 普通参数
* POJO类型参数
* 嵌套POJO类型参数
* 数组类型参数
* 集合类型参数
* JSON - 普通/对象/对象数组
* 日期

‍

@RequestParam

|名称|@RequestParam|
| ----------| ----------------------------------------------------|
|类型|形参注解|
|位置|SpringMVC控制器方法形参定义前面|
|作用|绑定请求参数与处理器方法形参间的关系|
|相关参数|required：是否为必传参数<br />defaultValue：参数默认值|

‍

@EnableWebMvc

|名称|@EnableWebMvc|
| ------| ---------------------------|
|类型|==配置类注解==|
|位置|SpringMVC配置类定义上方|
|作用|开启SpringMVC多项辅助功能|

‍

@RequestBody

|名称|@RequestBody|
| ------| ----------------------------------------------------------------------------|
|类型|==形参注解==|
|位置|SpringMVC控制器方法形参定义前面|
|作用|将请求中请求体所包含的数据传递给请求参数，此注解一个处理器方法只能使用一次|

‍

‍

#### 普通

地址参数名与形参变量名相同，定义形参即可接收参数; 不一致使用@RequestParam注解

```java
    @RequestMapping("/commonParamDifferentName")
    @ResponseBody
    public String commonParamDifferentName(@RequestPaam("name") String userName , int age){
    }
```

> 写上@RequestParam注解框架就不需要自己去解析注入，能提升框架处理性能

‍

‍

#### POJO

请求参数名与形参对象属性名相同，定义POJO类型**形参**即可接收参数; 请求参数key的名称要和POJO中属性的名称一致，否则无法封装. 

```java
    @RequestMapping("/pojoParam")
    @ResponseBody
    public String pojoParam(User user){
    }
```

‍

‍

**嵌套POJO**

结构同上, 按照对象层次结构关系即可接收嵌套POJO属性参数

‍

‍

#### 数组

同名请求参数可以直接映射到对应名称的形参数组对象中, 请求参数名与形参对象属性名相同且请求参数为多个，定义数组类型即可接收参数

对于简单数据类型使用数组会比集合更简单些

```java
    @RequestMapping("/arrayParam")
    @ResponseBody
    public String arrayParam(String[] likes){
    }
```

‍

‍

#### 集合

‍

> 简单的将集合对象写在参数里产生错误的原因
>
> SpringMVC将List看做是一个POJO对象来处理，将其创建一个对象并准备把前端的数据封装到对象中，但是**List是一个接口**无法创建对象，所以报错

‍

使用`@RequestParam`​注解映射到对应名称的集合对象中作为数据, 请求参数名与形参集合对象名相同且请求参数为多个，@RequestParam绑定参数关系

```java
@RequestMapping("/listParam")
@ResponseBody
public String listParam(@RequestParam List<String> likes){
}
```

‍

#### JSON

以普通数组为例

‍

##### 数组

json普通数组（["value1","value2","value3",...]）

‍

‍

##### 对象

json对象（{key1:value1,key2:value2,...}）, 可能有嵌套对象

```java
{
	"name":"itcast",
	"age":15
    "address":{
        "province":"beijing",
        "city":"beijing"
    }
}
```

```java
@RequestMapping("/pojoParamForJson")
@ResponseBody
public String pojoParamForJson(@RequestBody User user){
}
```

‍

‍

##### 对象数组

json对象数组（[{key1:value1,...},{key2:value2,...}]）

‍

```java
[
    {"name":"itcast","age":15},
    {"name":"itheima","age":12}
]
```

```java
@RequestMapping("/listPojoParamForJson")
@ResponseBody
public String listPojoParamForJson(@RequestBody List<User> list){
}
```

‍

‍

#### 日期

SpringMVC默认支持的字符串转日期的格式为`yyyy/MM/dd`​, 大多数情况下需要修改

‍

@DateTimeFormat

|名称|@DateTimeFormat|
| ----------| ---------------------------------|
|类型|==形参注解==|
|位置|SpringMVC控制器方法形参前面|
|作用|设定日期时间型数据格式|
|相关属性|pattern：指定日期时间格式字符串|

‍

**设置日期格式**

​`@DateTimeFormat`​

```java
@RequestMapping("/dataParam")
@ResponseBody
public String dataParam(Date date, @DateTimeFormat(pattern="yyyy-MM-dd") Date date1)
```

‍

‍

‍

‍

‍

‍

### 示例

‍

#### 初始Controller

```java
@Controller
public class UserController {

    @RequestMapping("/commonParam")
    @ResponseBody
    public String commonParam(){
        return "{'module':'commonParam'}";
    }
}
```

> 使用Maven生命周期: Tomcat -run运行, 使用Postman进行调试

‍

‍

## 响应

‍

* 响应页面
* 响应数据

  * 文本数据
  * **json数据**

‍

‍

@ResponseBody

|名称|@ResponseBody|
| ----------| -------------------------------------------------------------------------|
|类型|==方法\类注解==|
|位置|SpringMVC控制器方法定义上方和控制类上|
|作用|设置当前控制器返回值作为响应体,<br />写在类上，该类的所有方法都有该注解功能|
|相关属性|pattern：指定日期时间格式字符串|

‍

**说明**

该注解可以写在类上或者方法上, 写在类上就是该类下的所有方法都有@ReponseBody功能

‍

当方法上有@ReponseBody注解后

* 方法的返回值为字符串，会将其作为文本内容直接响应给前端
* 方法的返回值为对象，会将对象转换成JSON响应给前端

‍

此处又使用到了类型转换，内部还是通过Converter接口的实现类完成的，所以Converter除了实现日期格式转换外，它还可以实现:

* 对象转Json数据(POJO -> json)
* 集合转Json数据(Collection -> json)

‍

‍

### 页面

> [了解]

‍

**设置返回页面**

```java
@Controller
public class UserController {
  
    @RequestMapping("/toJumpPage")
    //注意
    //1.此处不能添加@ResponseBody,如果加了该注入，会直接将page.jsp当字符串返回前端
    //2.方法需要返回String
    public String toJumpPage(){
        System.out.println("跳转页面");
        return "page.jsp"; //去找page.jsp
    }
  
}
```

‍

**在静态资源里 &gt;&gt; webapp/page.jsp**

```java
<html>
    <body>
    <h2>Hello Spring MVC!</h2>
    </body>
</html>
```

页面跳转不适合采用PostMan进行测试; 直接打开浏览器输入`http://localhost/toJumpPage`​

‍

​

‍

### 文本

> [了解]

‍

**设置返回文本内容**

```java
@Controller
public class UserController {
  
   	@RequestMapping("/toText")	//注意此处该注解就不能省略，如果省略了,会把response text当前页面名称去查找，如果没有回报404错误
    @ResponseBody
    public String toText(){
        System.out.println("返回纯文本数据");
        return "response text";
    }
}
```

‍

‍

### JSON

‍

#### POJO对象

返回值为实体类对象，设置返回值为实体类类型，即可实现返回对应对象的json数据，需要依赖==@ResponseBody注解和@EnableWebMvc==注解

```java
@Controller
public class UserController {
  
    @RequestMapping("/toJsonPOJO")
    @ResponseBody
    public User toJsonPOJO(){
        System.out.println("返回json对象数据");
        User user = new User();
        user.setName("itcast");
        user.setAge(15);
        return user;
    }
}
```

‍

‍

#### POJO集合

封装存放后通过集合对象返回

```java
@Controller
public class UserController {
  
    @RequestMapping("/toJsonList")
    @ResponseBody
    public List<User> toJsonList(){
 
        User user1 = new User();
        user1.setName("传智播客");
        user1.setAge(15);

        User user2 = new User();
        user2.setName("黑马程序员");
        user2.setAge(12);

        List<User> userList = new ArrayList<User>();
        userList.add(user1);
        userList.add(user2);

        return userList;
    }
}
```

‍

‍

## 格式优化

‍

### **问题解决**

‍

每个方法的@RequestMapping注解中都定义了访问路径/books，重复性太高. 

```
将@RequestMapping提到类上面，用来定义所有方法共同的访问路径。
```

‍

每个方法的@RequestMapping注解中都要使用method属性定义请求方式，重复性太高. 

```
使用@GetMapping  @PostMapping  @PutMapping  @DeleteMapping代替
```

‍

每个方法响应json都需要加上@ResponseBody注解，重复性太高. 

```
1.将ResponseBody提到类上面，让所有的方法都有@ResponseBody的功能
2.使用@RestController注解替换@Controller与@ResponseBody注解，简化书写
```

‍

## JSON

‍

单单指定ajax的data为json数据，但是还是以表单内容发送

真正发送数据到后台时都是按照表单内容发送，所以后端可以直接用对象接收

ajax的contentType默认是 `application/x-www-form-urlencoded`​

‍

即如果真的要发送**完全的JSON数据**，就需要指定contentType=`"application/json"`​,但是此时后台就**没有办法直接使用对象接收**数据 (一般情况前端使用ajax发送json数据,后端使用对象来接收)

‍

‍

### 请求响应

‍

请求方式 GET

contentType无论指定成什么类型，后台都可以直接用对象接收

‍

请求方式 POST

contentType="application/**x-www-form-urlencoded**" (默认)  后台**也可以使用对象接收**

contentType="application/json"  前端ajax发送的数据data内容需要变成json字符串才行, 后台需要使用`@RequestBody`​​来解析JSON

‍

‍

**示例**

‍

前端

```java
<button id="btn2">btn2-ajax-json-对象类型</button>
<script src="/js/jquery-2.1.0.js"></script>
<script>
    $("#btn2").click(function (){
        $.ajax({
            url:"/test5.do",
            type:"post",
            data:{"id":1,"username":"张三","score":98.0,"birthday":"2022-12-12"},
            // contextType:"application/json",
            success:function (ret){
                if (ret.code == 0){
                    alert(ret.msg)
                }
            },
            error:function (){
                alert("服务器正忙!")
            }
        })
    });
</script>
```

‍

后端接收

```java
public class User {

    private int id;
    private String username;
    private double score;
  
    // 日期,默认只能解析yyyy/MM/dd类型的日期
    @DateTimeFormat(pattern = "yyyy-MM-dd")
    private Date birthday;
    // set get...
}
```

```java
    @RequestMapping("/test5")
//  后台直接使用User对象来接收,JSON的key要与User的属性一致
    public ResultData test5(User user){
        System.out.println("user = " + user);
        return ResultData.ok();
    }
```

‍

‍

### 示例

‍

###### 依赖导入

SpringMVC默认使用的是jackson来处理json的转换

```java
<dependency>
    <groupId>com.fasterxml.jackson.core</groupId>
    <artifactId>jackson-databind</artifactId>
    <version>2.9.0</version>
</dependency>
```

‍

###### 调试方法

使用调试工具: Body - raw -Type:JSON

‍

‍

###### 开启注解

SpringMVC注解支持

SpringMVC的配置类中开启SpringMVC的注解支持，包含了将JSON转换成对象的功能

```java
@Configuration
@ComponentScan("com.itheima.controller")

@EnableWebMvc    //开启json数据类型自动转换

public class SpringMvcConfig {
}
```

‍

‍

###### 配置参数映射

**参数前添加@RequestBody**

将外部传递的json数组数据映射到形参的集合对象中作为数据

```java
@RequestMapping("/listParamForJson")
@ResponseBody
public String listParamForJson(@RequestBody List<String> likes){
    System.out.println("list common(json)参数传递 list ==> "+likes);
    return "{'module':'list common for json param'}";
}
```

‍

###### 启动

‍

‍

‍

## 文件传输

‍

### 文件上传

‍

#### Servlet旧示例

‍

示例

‍

**依赖**

第三方(commons-io,commons-fileupload)依赖

```xml
    <dependency>
      <groupId>commons-io</groupId>
      <artifactId>commons-io</artifactId>
      <version>2.4</version>
    </dependency>


    <dependency>
      <groupId>commons-fileupload</groupId>
      <artifactId>commons-fileupload</artifactId>
      <version>1.3.3</version>
      <!-- 排除,防止依赖冲突 -->

      <exclusions>
        <exclusion>
          <groupId>javax.servlet</groupId>
          <artifactId>servlet-api</artifactId>
        </exclusion>
      </exclusions>

    </dependency>
```

‍

**XML配置** 文件上传解析器

```xml
<!-- 文件上传解析器 -->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <!-- 允许最大上传大小 -->
    <property name="maxUploadSize" value="102400"/>
</bean>
```

‍

**表单**

```html
<!--
    文件上传使用的标签是form+input
    必须使用请求方法: post
    必须指定enctype=""
-->
<form action="/upload.do" method="post" enctype="multipart/form-data">
    文件上传<input type="file" name="source"><br>
    <input type="submit" value="上传">
</form>
```

‍

**后端**

```java
   /**
     * 文件上传
     * @param source 类型必须是MultipartFile,参数名必须是和前端name值一样
     *               这样,文件上传时,MultipartFile内部就会有文件对象
     * @param request 通过请求对象可以获得服务器地址,后续讲图片上传到服务对应位置
     * @return
     */
    @RequestMapping("/upload")
    public ResultData upload(MultipartFile source, HttpServletRequest request) throws IOException {

        String originalFilename = source.getOriginalFilename( ); //得到名称
        System.out.println("originalFilename = " + originalFilename);

        // 产生随机图片名
        String prefix = UUID.randomUUID( ).toString( );
        System.out.println("prefix = " + prefix);

        // 获得文件后缀
        String suffix = FilenameUtils.getExtension(originalFilename);

        // 组合成新文件名
        String filename = prefix+"."+suffix;

        // 获得服务器路径
        String realPath = request.getServletContext( ).getRealPath("/upload_file");
        System.out.println("realPath = " + realPath);

        // 创建文件夹
        File parentFile = new File(realPath);
        if (!parentFile.exists()) {
            parentFile.mkdir();
        }

        // 开始上传
        source.transferTo(new File(parentFile,filename));

        return ResultData.ok();
    }
```

‍

‍

##### **回显**

‍

方案一: 上传成功后,将图片**路径返回给前端**,前端再处理

方案二: 使用ajax上传,上传成功后,将图片路径返回给**ajax的回调函数**

‍

‍

**页面表单**

使用ajax来完成文件上传,form里面不需要指定其他东西

```html
<h1>上传图片并回显</h1>
<!--
    使用ajax来完成文件上传,form里面不需要指定其他东西
-->
<form id="formData">
    文件上传<input type="file" name="source"><br>
    <input type="button" onclick="ajaxUpload()" value="上传">
</form>

<!-- 回显图片 -->
<img width="300px" id="img" src="" alt="图片">

<script src="/js/jquery-2.1.0.js"></script>
<script>
    // 文件上传
    function ajaxUpload(){
        // 获得表单对象
        var formData = new FormData($("#formData")[0]);
        // 使用ajax文件上传
        $.ajax({
            url:"/upload.do",
            type:"post",
            data:formData,
            async:false,
            cache:false,
            contentType:false,
            processData:false,
            success:function (ret) {
                if (ret.code == 0) {
                    console.log(ret.data)
                    $("#img").attr("src",ret.data);
                }else{
                    alert("上传失败1");
                }
            },
            error:function () {
                alert("上传失败2");
            }
        });
    }
</script>
</body>
</html>
```

‍

**后端**

将图片路径返回了

```java
       return ResultData.ok("/upload_file/"+filename);
```

‍

‍

‍

### 文件下载

‍

‍

#### Servlet旧示例

‍

‍

前端

```html
<div>
  <div>
      <!-- 图片路径是服务器路径 -->
    <img width="300px" src="/upload_file/81d30e55-d6ca-4463-ba3b-5732504128f3.jpg">

      <button>
          <!-- 下载时,请求中携带文件名即可 -->
          <a href="/download.do?filename=81d30e55-d6ca-4463-ba3b-5732504128f3.jpg">下载</a>
      </button>

  </div>
  <div>
    <img width="300px" src="/upload_file/dazhong.jpg">
      <a href="/download.do?filename=dazhong.jpg">下载</a>
  </div>
</div>
```

‍

后端

```java
/**
     * 下载图片
     * 下载是需要响应一个对话框,选择文件下载到什么地方
     * 不需要跳转页面,或者返回JSON
     */
@RequestMapping("/download")
public void download(String filename, HttpServletRequest request, HttpServletResponse response) throws IOException {

    String realPath = request.getServletContext( ).getRealPath("/upload_file");
    String filepath = realPath+"/"+filename;

    //设置响应头 告知浏览器，要以附件的形式保存内容

    //浏览器显示的下载文件名
    response.addHeader("content-disposition","attachment;filename="+filename);
    response.setContentType("multipart/form-data");

    //读取目标文件，写出给客户端
    IOUtils.copy(new FileInputStream(filepath),response.getOutputStream());
}
```

‍

## ServletMVC

(OLD)

旧版的Servlet+SpringMVC工程

‍

### 前端控制器

‍

前端控制器(DispatchServlet),核心思想是拦截到请求,然后分发请求,同处理器映射器找到处理器,然后通过处理器适配器执行处理器.

DispatchServlet是一个"大脑",做统一调度,当请求到达时创建,并且要去找springmvc.xml从而通过映射器,适配器,视图解析器来完成后续工作

‍

配置web.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_3_1.xsd"
version="3.1">
  
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 前端发出请求,映射到DispatcherServlet后,就要找HandlerMapping以及HandlerAdapter -->
        <!-- 这些组件都在springmvc配置文件中,所以,在这需要加载springmvc.xml -->
        <init-param>
            <param-name>contextConfigLocation</param-name>
             <!-- 将配置文件放置/resources/springmvc.xml -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>
    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <!-- 请求路径是/,匹配到所有请求,包括html,js,css,图片等请求 -->
        <!-- <url-pattern>/</url-pattern>-->

        <!-- springmvc的习惯是请求加.do -->
        <!-- 这样就会匹配到所有请求中带.do的请求 -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>

</web-app>
```

‍

‍

### 编码格式拦截器

```
    <!-- 编码格式拦截器 -->
    <filter>
        <filter-name>encodingFilter</filter-name>
        <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
        <init-param>
            <param-name>encoding</param-name>
            <param-value>UTF-8</param-value>
        </init-param>
    </filter>
    <filter-mapping>
        <filter-name>encodingFilter</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>
```

‍

‍

‍

### 页面跳转

‍

servlet中跳转页面的功能

请求转发    forward

重定向    redirect

‍

SpringMVC封装的请求转发和重定向的特点见Servlet

‍

#### **请求转发**

> 在Controller的方法的返回值中写forward:路径即可完成跳转
>
> ​` forward:/view/ok.html forward:/test.do`​
>
> 注意: 跳转后的路径要写完整

```java
//请求转发至其他页面
    @RequestMapping("/forward")
    public String forward(){
        return "forward:/view/ok.html";
    }

//请求转发至其他请求
    @RequestMapping("/forward2")
    public String forward2(){
        return "forward:/test.do";
    }
```

‍

#### **重定向**

> 在Controller的方法的返回值中写redirect:路径即可完成跳转
>
> ​` redirect:/view/ok.html redirect:/test.do`​
>
> 注意: 跳转后的路径要写完整

```java
    @RequestMapping("/redirect")
    public String redirect(){
        return "redirect:/view/ok.html";
    }

    @RequestMapping("/redirect2")
    public String redirect2(){
        return "redirect:/test.do";
    }
```

‍

#### 数据共享

‍

request域

* 只有在一次请求中有效,即请求转发后能取出

‍

session域

* 一次会话有效,只要会话不结束,无论请求转发还是重定向都能取出

‍

示例

```java
    @RequestMapping("/req")
    public String requestAndSession(HttpServletRequest request, HttpSession session) {

        // 存入
        request.setAttribute("req","req-Value");
        session.setAttribute("se","se-Value");

        return "forward:/req2.do";
    }


    @RequestMapping("/req2")
    public String requestAndSession2(HttpServletRequest request, HttpSession session) {

       // 取出
        String v1 = (String) request.getAttribute("req");
        System.out.println("v1 = " + v1);
        String v2 = (String) session.getAttribute("se");
        System.out.println("v2 = " + v2);

        return "ok";
    }
}
```

‍

‍

### 拦截器

‍

配置类及XML配置示例

```java
public class MyInterceptor implements HandlerInterceptor {

    /**
     * 目前Controller,执行前,执行拦截
     * 一般用于: 校验
     * @return  返回true,即认为放行,返回false不放行
     */
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        return true;
    }


    /**
     * 在目标Controller方法执行完,但是afterCompletion方法前执行
     * 对目标方法的返回再处理,可以对响应再定制.
     */
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("post" );
    }


    /**
     * 目标Controller的方法全部执行完执行
     * 一般: 资源回收
     */
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("after" );
    }
}
```

```xml
    <!-- 配置拦截器 -->
    <mvc:interceptors>
        <mvc:interceptor>

            <!-- mapping path: 要拦截的路径 -->
            <!-- <mvc:mapping path="/user/**"/>-->
            <mvc:mapping path="/**"/>

            <!-- exclude-mapping 排除拦截的 -->
            <mvc:exclude-mapping path="/login"/>
            <mvc:exclude-mapping path="/test.do"/>

            <!-- 指定拦截器类 -->
            <bean class="com.qf.interceptor.MyInterceptor"/>

        </mvc:interceptor>
    </mvc:interceptors>
```

‍

‍

### 静态资源处理

‍

在webapp|-view中创建html页面

> 如果前端控制器(DispatcherServlet)类映射的是*.do
>
> 那么前端的静态资源都是不影响加载的

> 如果前端控制器(DispatcherServlet)类映射的是/
>
> 那么所有请求都要经过前端控制器,静态资源也不例外.那么此时静态资源无法完成映射.

‍

前端控制器设置静态资源映射

```xml
    <servlet>
        <servlet-name>dispatcherServlet</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
    </servlet>

    <servlet-mapping>
        <servlet-name>dispatcherServlet</servlet-name>
        <!-- 此处要改成 / ,这样才能映射匹配到所有请求 -->
        <url-pattern>/</url-pattern>
    </servlet-mapping>
```

```xml
    <!-- 静态资源映射
        mapping="访问路径"
        location="本地资源位置"
		"/**" 前端的任何请求去匹配后台处于"/"下的静态资源
    -->
    <mvc:resources mapping="/**" location="/"/>
```

‍

‍

### 全局异常处理

‍

#### 示例

‍

处理Controller的所有异常

```java
@Component // 配置到spring容器
public class MyExceptionHandler implements HandlerExceptionResolver {

    @Override
    public ModelAndView resolveException(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) {

        ModelAndView mv = new ModelAndView( );

        if (ex instanceof ArithmeticException) {

            mv.setViewName("redirect:/view/500.html");

        } else if (ex instanceof AuthException) {

            mv.setViewName("redirect:/view/auth.html");

        }
        return mv;
    }
}
```

‍

### MVC XML配置

-> /resources/springmvc.xml

‍

内容

* 处理器
* 处理器映射器
* 处理器适配器
* 视图解析器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 1 自定义的Controller -->
    <bean id="hiController" class="com.qf.controller.HiController" name="/hi.do" />

    <!-- 2 处理器映射器 -->
    <bean class="org.springframework.web.servlet.handler.BeanNameUrlHandlerMapping"/>

    <!-- 3 处理器适配器 -->
    <bean class="org.springframework.web.servlet.mvc.SimpleControllerHandlerAdapter"/>

    <!-- 4 视图解析器 -->
    <bean id="viewResolver" class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!-- 用来拼接页面名字的前缀 -->
        <property name="prefix" value="/"/>
        <property name="suffix" value=".html"/>
        <!-- 将后台Controller返回的视图名,组装成完整页面路径: /hi.html -->
    </bean>
</beans>
```

‍

‍

‍

‍

‍

# 高级

‍

## RESTful

Restful格式

‍

@PathVariable

|名称|@PathVariable|
| ------| ----------------------------------------------------------------------|
|类型|==形参注解==|
|位置|SpringMVC控制器方法形参定义前面|
|作用|绑定路径参数与处理器方法形参间的关系，要求路径参数名与形参名一一对应|

‍

@RestController

|名称|@RestController|
| ------| -----------------------------------------------------------------------------------|
|类型|==类注解==|
|位置|基于SpringMVC的RESTful开发控制器类定义上方|
|作用|设置当前控制器类为RESTful风格，<br />等同于@Controller与@ResponseBody两个注解组合功能|

‍

@GetMapping @PostMapping @PutMapping @DeleteMapping

|名称|@GetMapping @PostMapping @PutMapping @DeleteMapping|
| ----------| ----------------------------------------------------------------------------------------------|
|类型|==方法注解==|
|位置|基于SpringMVC的RESTful开发控制器方法定义上方|
|作用|设置当前控制器方法请求访问路径与请求动作，每种对应一个请求动作，<br />例如@GetMapping对应GET请求|
|相关属性|value（默认）：请求访问路径|

‍

‍

### 传递路径参数

前端发送请求的时候使用:`http://localhost/users/1`​,路径中的`1`​就是要传递的参数

‍

后端获取参数，需要做如下修改

* 修改@RequestMapping的value属性，将其中修改为`/users/{id}`​，目的是和路径匹配
* 在方法的形参前添加@**PathVariable**注解

```java
@Controller
public class UserController {
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
	@RequestMapping(value = "/users/{id}/{name}",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(@PathVariable Integer id,@PathVariable String name) {
        System.out.println("user delete..." + id+","+name);
        return "{'module':'user delete'}";
    }
}
```

‍

==注意==

如果方法形参的名称和路径`{}`​中的值不一致, 通过@RequestParam注解绑定

如果有多个参数需要传递, 使用`http://localhost/users/1/tom`​: `1`​和`tom`​就是要传递的两个参数

‍

‍

#### 语法

‍

设定Http请求动作(动词)

```java
@RequestMapping(value="",method = RequestMethod.POST|GET|PUT|DELETE)
```

‍

设定请求参数(路径变量)

```java
@RequestMapping(value="/users/{id}",method = RequestMethod.DELETE)
public String delete(@PathVariable Integer id)
```

‍

‍

### 收参三注解

**三收参数注解辨析**

`@RequestBody`​​、`@RequestParam`​​、`@PathVariable`​​

‍

* 区别

  * @RequestParam用于接收url地址传参或表单传参
  * @RequestBody用于接收json数据, 应用较广
  * @PathVariable用于接收路径参数，使用{参数名称}描述路径参数
* 应用

  * 后期开发中，发送请求参数超过1个时，以json格式为主，@RequestBody应用较广
  * 如果发送非json格式数据，选用@RequestParam接收请求参数
  * 采用RESTful进行开发，当参数数量较少时，例如1个，可以采用@PathVariable接收请求路径变量，通常用于传递id值

‍

‍

### 示例

‍

> 替换成RESTful的开发方式
>
> 1.之前不同的请求有不同的路径,现在要将其修改为统一的请求路径
>
> 修改前: 新增: /save ,修改: /update,删除 /delete...
>
> 修改后: 增删改查: /users
>
> ‍
>
> 2.根据GET查询、POST新增、PUT修改、DELETE删除对方法的请求方式进行限定
>
> ‍
>
> 3.发送请求的过程中设置请求参数

‍

‍

#### 新增

```java
@Controller
public class UserController {
	//设置当前请求方法为POST，表示REST风格中的添加操作
    @RequestMapping(value = "/users",method = RequestMethod.POST)
    @ResponseBody
    public String save() {
        System.out.println("user save...");
        return "{'module':'user save'}";
    }
}
```

将请求路径更改为`/users`​

* 访问该方法使用 POST: `http://localhost/users`​

使用method属性限定该方法的访问方式为`POST`​

* 如果发送的不是POST请求，比如发送GET请求，则会报错

‍

#### 删除

```java
@Controller
public class UserController {
    //设置当前请求方法为DELETE，表示REST风格中的删除操作
	@RequestMapping(value = "/users",method = RequestMethod.DELETE)
    @ResponseBody
    public String delete(Integer id) {
        System.out.println("user delete..." + id);
        return "{'module':'user delete'}";
    }
}
```

* 将请求路径更改为`/users`​

  * 访问该方法使用 DELETE: `http://localhost/users`​

‍

‍

‍

#### 查询

==根据ID==

```java
@Controller
public class UserController {
    //设置当前请求方法为GET，表示REST风格中的查询操作
    @RequestMapping(value = "/users/{id}" ,method = RequestMethod.GET)
    @ResponseBody
    public String getById(@PathVariable Integer id){
        System.out.println("user getById..."+id);
        return "{'module':'user getById'}";
    }
}
```

将请求路径更改为`/users`​

* 访问该方法使用 GET: `http://localhost/users/666`​

‍

==查询所有==

```java
@Controller
public class UserController {
    //设置当前请求方法为GET，表示REST风格中的查询操作
    @RequestMapping(value = "/users" ,method = RequestMethod.GET)
    @ResponseBody
    public String getAll() {
        System.out.println("user getAll...");
        return "{'module':'user getAll'}";
    }
}
```

‍

#### 前后交互

‍

**需求**

图片列表查询，从后台返回数据，将数据展示在页面上

新增图片，将新增图书的数据传递到后台，并在控制台打印

**说明:** 此次案例的重点是在SpringMVC中如何使用RESTful实现前后台交互，所以本案例并没有和数据库进行交互，所有数据使用`假`​数据来完成开发

‍

步骤

> 1.搭建项目导入jar包
>
> 2.编写Controller类，提供两个方法，一个用来做列表查询，一个用来做新增
>
> 3.在方法上使用RESTful进行路径设置
>
> 4.完成请求、参数的接收和结果的响应
>
> 5.使用PostMan进行测试
>
> 6.将前端页面拷贝到项目中
>
> 7.页面发送ajax请求
>
> 8.完成页面数据的展示

‍

‍

##### 接口开发

‍

RESTful配置Controller

```java
@RestController
@RequestMapping("/books")
public class BookController {

    @PostMapping
    public String save(@RequestBody Book book){
        System.out.println("book save ==> "+ book);
        return "{'module':'book save success'}";
    }

 	@GetMapping
    public List<Book> getAll(){
        System.out.println("book getAll is running ...");
        List<Book> bookList = new ArrayList<Book>();

        Book book1 = new Book();
        book1.setType("计算机");
        book1.setName("SpringMVC入门教程");
        book1.setDescription("小试牛刀");
        bookList.add(book1);

        Book book2 = new Book();
        book2.setType("计算机");
        book2.setName("SpringMVC实战教程");
        book2.setDescription("一代宗师");
        bookList.add(book2);

        Book book3 = new Book();
        book3.setType("计算机丛书");
        book3.setName("SpringMVC实战教程进阶");
        book3.setDescription("一代宗师呕心创作");
        bookList.add(book3);

        return bookList;
    }

}
```

‍

测试新增

```json
{
    "type":"计算机丛书",
    "name":"SpringMVC终极开发",
    "description":"这是一本好书"
}
```

‍

‍

##### 访问静态资源

‍

拷贝静态页面

‍

‍

访问pages目录下的books.html: 打开浏览器输入`http://localhost/pages/books.html`​ 出现错误

‍

是因为**SpringMVC拦截了静态资源**，根据/pages/books.html**去controller找对应的方法(怎么可能找得到?!)**

这是因为初始快速配置, 拦截了/,也就是所有访问

```java
protected String[] getServletMappings(){
    return new String[]{"/"};   
}
```

这里配置的是/, `http://localhost/pages/books.html`​满足这个规则

‍

‍

==放行静态资源==

config>> SpringMvcSupport

```java
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {

    //设置静态资源访问过滤，当前类需要设置为配置类，并被扫描加载
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //当访问/pages/????时候，从/pages目录下查找内容
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}
```

‍

该配置类在config目录下，SpringMVC扫描的是controller包, 因此还需要将SpringMvcConfig配置类进行修改

或者`@ComponentScan("com.itheima")`​, 标记

```java
@Configuration
@ComponentScan({"com.itheima.controller", "com.itheima.config"}) //读取这个配置
@EnableWebMvc
public class SpringMvcConfig {
}
```

‍

‍

‍

‍

‍

‍

## 静态资源处理

‍

SpringMVC拦截了静态资源，一般去controller找链接对应的方法

‍

**初始快速配置**拦截了"/" 所有访问

```java
protected String[] getServletMappings(){
    return new String[]{"/"};   
}
```

‍

创建**控制类**

config>> SpringMvcSupport

```java
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {

    //设置静态资源访问过滤，当前类需要设置为配置类，并被扫描加载
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        //当访问/pages/????时候，从/pages目录下查找内容
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
        registry.addResourceHandler("/js/**").addResourceLocations("/js/");
        registry.addResourceHandler("/css/**").addResourceLocations("/css/");
        registry.addResourceHandler("/plugins/**").addResourceLocations("/plugins/");
    }
}
```

‍

该配置类在config目录下，SpringMVC扫描的是controller包, 因此还需要将SpringMvcConfig配置类进行修改

或者`@ComponentScan("com.itheima")`​, 标记

```java
@Configuration
@ComponentScan({"com.itheima.controller", "com.itheima.config"}) //读取这个配置
@EnableWebMvc
public class SpringMvcConfig {
}
```

‍

‍

‍

## 全局异常处理

‍

没有做任何的异常处理时，三层架构处理异常的方案

> 若是Mapper的异常, 它会往上抛(谁调用Mapper就抛给谁)，会抛给service. service 中也存在异常了，会抛给controller. 而在controller当中，我们也没有做任何的异常处理，所以最终异常会再往上抛. 最终抛给框架之后，框架就会返回一个JSON格式的数据，里面封装的就是错误的信息，但是框架返回的JSON格式的数据并不符合我们的开发规范.

‍

配置好处

* 统一的错误页面或者错误码
* 对用户更友好

‍

解决方案

* ~~在所有Controller的所有方法中进行try…catch处理~~
* 设置**全局异常处理器, 简单、优雅**

‍

* 类添加注解

  * @ControllerAdvice，如果需要返回json数据，则方法需要加@ResponseBody
  * @RestControllerAdvice, 默认返回json数据，方法不需要加@ResponseBody
* 方法添加处理器

  * 捕获全局异常,处理所有不可知的异常
  * @ExceptionHandler(value=Exception.class)

‍

```java
@RestControllerAdvice = @ControllerAdvice + @ResponseBody
```

处理异常的方法返回值会转换为json后再响应给前端

在全局异常处理器当中通过@ExceptionHandler注解当中的value属性来指定我们要捕获的是哪一类型的异常. 

‍

‍

### 实例

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    //处理异常
    @ExceptionHandler(Exception.class) //指定能够处理的异常类型
    public Result ex(Exception e){
        e.printStackTrace();//打印堆栈中的异常信息

        //捕获到异常之后，响应一个标准的Result
        return Result.error("对不起,操作失败,请联系管理员");
    }
}
```

‍

‍

指定拦截的注解类型: RestController和Controller

```java
@ControllerAdvice(annotations = {RestController.class, Controller.class})
@ResponseBody
@Slf4j
public class GlobalExceptionHandler {

    //SQLIntegrityConstraintViolationException, 就是数据库的完整性约束异常
    @ExceptionHandler(SQLIntegrityConstraintViolationException.class)
    public R<String> exceptionHandler(SQLIntegrityConstraintViolationException ex) {
        log.error(ex.getMessage()); //日志打印错误信息

        if (ex.getMessage().contains("Duplicate entry")) { //如果错误信息包含Duplicate entry, 则说明是主键重复了
            String[] split = ex.getMessage().split(" "); //将错误信息按照空格分割, 返回一个字符串数组
            String msg = split[2] + "已存在,请尝试其他名称"; //获取第三个元素, 即重复的主键值对象
            return R.error(msg);
        }

        return R.error("未知错误, 请联系管理员");
    }

    //自定义业务异常
    @ExceptionHandler(CustomException.class)
    public R<String> exceptionHandler(CustomException ex) {
        log.error(ex.getMessage());

        return R.error(ex.getMessage());
    }
}
```

‍

### 自定义异常处理

‍

```java
/**
 * 自定义业务异常类
 */
public class CustomException extends RuntimeException { //继承运行时异常
    public CustomException(String message) { //返回消息
        super(message);
    }
    //同时需要在异常处理类中添加异常处理方法
}

```

‍

‍

‍

## 拦截器

动态拦截方法调用的机制，在SpringMVC中动态拦截控制器方法的执行

‍

‍

### **执行流程**

当有拦截器后，请求会先进入preHandle方法，

 如果方法返回true，则放行继续执行后面的handle[controller方法]和后面的方法(postHandle+afterCompletion)

 如果返回false，则直接跳过后面所有方法的执行

‍

### **作用**

* 在指定的方法调用前后执行预先设定的代码
* 阻止原始方法的执行
* 总结：拦截器就是用来做增强的
* 拦截器和过滤器在作用和执行顺序上也很相似

‍

### 评价

**拦截器和过滤器**之间的区别

* 归属不同：Filter属于Servlet技术(底层)，Interceptor属于SpringMVC技术(封装)
* 拦截内容不同：Filter对所有访问进行增强，Interceptor仅针对SpringMVC的访问进行增强

​​

‍

### 拦截器类

‍

ProjectInterceptor类实现HandlerInterceptor接口，重写接口中的三个方法

拦截器类要被SpringMVC容器扫描到

‍

#### 示例

```java
@Component
//定义拦截器类，实现HandlerInterceptor接口
//注意当前类必须受Spring容器控制
public class ProjectInterceptor implements HandlerInterceptor {

    @Override
    //原始方法调用前执行的内容
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...");
        return true;
    }

    @Override
    //原始方法调用后执行的内容
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    //原始方法调用完成后执行的内容
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

‍

### 简化编写

SpringMvcConfig实现WebMvcConfigurer接口省去SpringMvcSupport编写, 但是导致侵入性强

‍

#### 示例

```java
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
    }
}
```

‍

### 一般配置

在SpringMvcSupport配置拦截器

‍

registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*" );

```java

@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        //配置拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*" );
    }
}
```

‍

Config里面添加包扫描

```java
@Configuration

@ComponentScan({"com.itheima.controller","com.itheima.config"})

@EnableWebMvc
public class SpringMvcConfig{
}
```

‍

### 参数

‍

这三个方法中，最常用的是==preHandle==,在这个方法中可以通过返回值来决定是否要进行放行

把业务逻辑放在该方法中，如果满足业务则返回true放行，不满足则返回false拦截

‍

‍

#### 前置处理方法

原始方法之前运行preHandle

```java
public boolean preHandle(HttpServletRequest request,
                         HttpServletResponse response,
                         Object handler) throws Exception {
    System.out.println("preHandle");
    return true;
}
```

* request:请求对象
* response:响应对象
* handler:被调用的处理器对象，本质上是一个方法对象，对反射中的Method对象进行了再包装

‍

使用request对象可以获取请求数据中的内容，如获取请求头的`Content-Type`​

```java
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    String contentType = request.getHeader("Content-Type");
    System.out.println("preHandle..."+contentType);
    return true;
}
```

‍

使用handler参数，可以获取方法的相关信息

```java
public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
    HandlerMethod hm = (HandlerMethod)handler;
    String methodName = hm.getMethod().getName();//可以获取方法的名称
    System.out.println("preHandle..."+methodName);
    return true;
}
```

‍

‍

#### 后置处理方法

原始方法运行后运行，如果原始方法被拦截，则不执行

```java
public void postHandle(HttpServletRequest request,
                       HttpServletResponse response,
                       Object handler,
                       ModelAndView modelAndView) throws Exception {
    System.out.println("postHandle");
}
```

前三个参数和上面的是一致的

modelAndView:如果处理器执行完成具有返回结果，可以读取到对应数据与页面信息，并进行调整

因为现在基本都是返回json数据，所以该参数的使用率不高

‍

‍

#### 完成处理方法

拦截器最后执行的方法，无论原始方法是否执行

```java
public void afterCompletion(HttpServletRequest request,
                            HttpServletResponse response,
                            Object handler,
                            Exception ex) throws Exception {
    System.out.println("afterCompletion");
}
```

前三个参数与上面的是一致的; ex: 如果处理器执行过程中出现异常对象，可以针对异常情况进行单独处理

因为现在已经有全局异常处理器类，所以该参数的使用率也不高

‍

‍

### 拦截器链配置

配置多个拦截器

‍

##### 创建

实现接口，并重写接口中的方法

```java
@Component
public class ProjectInterceptor2 implements HandlerInterceptor {
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...222");
        return false;
    }

    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...222");
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...222");
    }
}
```

‍

##### 配置

```java
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc

//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;
    @Autowired
    private ProjectInterceptor2 projectInterceptor2;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
        registry.addInterceptor(projectInterceptor2).addPathPatterns("/books","/books/*");
    }
}
```

‍

##### 顺序

拦截器执行的顺序是和配置顺序有关，先进后出

* 当配置多个拦截器时，形成拦截器链
* 拦截器链的运行顺序参照拦截器添加顺序为准
* 当拦截器中出现对原始处理器的拦截，后面的拦截器均终止运行
* 当拦截器运行中断，仅运行配置在前面的拦截器的afterCompletion操作

‍

preHandle：与配置顺序相同，必定运行

postHandle:与配置顺序相反，可能不运行

afterCompletion:与配置顺序相反，可能不运行. 

顺序不太好记，最终只需要把握住一个原则即可:==以最终的运行结果为准==

‍

拦截器链在false情况下的执行顺序

* 全部成功的执行顺序    pre1->pre2->pre3->handle->post3->post2->post1->after3->after2->after1
* 3号返回错误    pre1->pre2->pre3->after2->after1
* 2号返回错误    pre1->pre2->after1
* 1号返回错误    pre1

‍

‍

### 示例

‍

#### 创建拦截器类

ProjectInterceptor类实现HandlerInterceptor接口，重写接口中的三个方法

拦截器类要被SpringMVC容器扫描到. 

```java
@Component
//定义拦截器类，实现HandlerInterceptor接口
//注意当前类必须受Spring容器控制
public class ProjectInterceptor implements HandlerInterceptor {

    @Override
    //原始方法调用前执行的内容
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle...");
        return true;
    }

    @Override
    //原始方法调用后执行的内容
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle...");
    }

    @Override
    //原始方法调用完成后执行的内容
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion...");
    }
}
```

‍

‍

#### 配置拦截器类

SpringMvcSupport->addInterceptors >> registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*" );

```java
@Configuration
public class SpringMvcSupport extends WebMvcConfigurationSupport {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        registry.addResourceHandler("/pages/**").addResourceLocations("/pages/");
    }

    @Override
    protected void addInterceptors(InterceptorRegistry registry) {
        //配置拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*" );
    }
}
```

‍

#### 添加包扫描

SpringMvcConfig

```java
@Configuration

@ComponentScan({"com.itheima.controller","com.itheima.config"})

@EnableWebMvc
public class SpringMvcConfig{
}
```

‍

#### 简化编写

SpringMvcConfig实现WebMvcConfigurer接口简化SpringMvcSupport, 但是导致侵入性强

```java
@Configuration
@ComponentScan({"com.itheima.controller"})
@EnableWebMvc
//实现WebMvcConfigurer接口可以简化开发，但具有一定的侵入性
public class SpringMvcConfig implements WebMvcConfigurer {
    @Autowired
    private ProjectInterceptor projectInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //配置多拦截器
        registry.addInterceptor(projectInterceptor).addPathPatterns("/books","/books/*");
    }
}
```

‍

‍

## **消息转换器**​

‍

### 实例

‍

#### 完善时间显示

‍

##### **属性注解**

在属性上加上注解，对日期进行格式化

```java
    //@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime createTime;

    //@JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
    private LocalDateTime updateTime;
```

‍

##### **SpringMVC的消息转换器（推荐)**

在WebMvcConfiguration中扩展SpringMVC的消息转换器，统一对日期类型进行格式处理

```java
	/**
     * 扩展Spring MVC框架的消息转化器
     * @param converters
     */
    protected void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
        log.info("扩展消息转换器...");
        //创建一个消息转换器对象
        MappingJackson2HttpMessageConverter converter = new MappingJackson2HttpMessageConverter();
        //需要为消息转换器设置一个对象转换器，对象转换器可以将Java对象序列化为json数据
        converter.setObjectMapper(new JacksonObjectMapper());
        //将自己的消息转化器加入容器中
        converters.add(0,converter);
    }
```

‍

‍

‍

## 整合

SSM整合: 把`Mybatis`​、`Spring`​和`SpringMVC`​三个框架整合在一起完成业务功能开发

‍

‍

### 流程

‍

 **(1) 创建工程**

1. 创建一个Maven的web工程
2. pom.xml添加SSM需要的依赖jar包
3. 编写Web项目的**入口配置类**，实现`AbstractAnnotationConfigDispatcherServletInitializer`​​​重写以下方法

    * getRootConfigClasses() ：返回Spring的配置类->需要==SpringConfig==配置类
    * getServletConfigClasses() ：返回SpringMVC的配置类->需要==SpringMvcConfig==配置类
    * getServletMappings() : 设置SpringMVC请求拦截路径规则
    * getServletFilters() ：设置过滤器，解决POST请求中文乱码问题

‍

 **(2) SSM整合**[==重点是各个配置的编写==]

‍

1. SpringConfig

    * 标识该类为配置类 @Configuration
    * 扫描Service所在的包 @ComponentScan
    * 在Service层要管理事务 @EnableTransactionManagement
    * 读取外部的properties配置文件 @PropertySource
    * 整合Mybatis需要引入Mybatis相关配置类 @Import

      1. 第三方数据源配置类 JdbcConfig

          * 构建DataSource数据源，DruidDataSouroce,需要注入数据库连接四要素， @Bean @Value
          * 构建平台事务管理器，DataSourceTransactionManager,@Bean
      2. Mybatis配置类 MybatisConfig

          * 构建SqlSessionFactoryBean并设置别名扫描与数据源，@Bean
          * 构建MapperScannerConfigurer并设置DAO层的包扫描
2. SpringMvcConfig

    * 标识该类为配置类 @Configuration
    * 扫描Controller所在的包 @ComponentScan
    * 开启SpringMVC注解支持 @EnableWebMvc

‍

 **(3)功能模块**[与具体的业务模块有关]

1. 创建数据库表
2. 根据数据库表创建对应的**模型类**
3. 通过**Dao层**完成数据库表的增删改查(接口+自动代理)
4. 编写**Service层**[Service接口+实现类]

    * @Service
    * @Transactional
    * 整合Junit对业务层进行单元测试

      * @RunWith
      * @ContextConfiguration
      * @Test
5. 编写**Controller层**

    * 接收请求 @RequestMapping @GetMapping @PostMapping @PutMapping @DeleteMapping
    * 接收数据 简单、POJO、嵌套POJO、集合、数组、JSON数据类型

      * @RequestParam
      * @PathVariable
      * @RequestBody
    * 转发业务层

      * @Autowired
    * 响应结果

      * @ResponseBody

‍

‍

### 整合配置

下面罗列创建的内容

‍

‍

#### web项目

(maven-webapp模板)

‍

#### 依赖

‍

#### 项目包结构

* config目录存放的是相关的配置类
* controller编写的是Controller类
* dao存放的是Dao接口，因为使用的是Mapper接口代理方式，所以没有实现类包
* service存的是Service接口，impl存放的是Service实现类
* resources:存入的是配置文件，如Jdbc.properties
* webapp:目录可以存放静态资源
* test/java:存放的是测试类

‍

#### SpringConfig

```java
@Configuration
@ComponentScan({"com.itheima.service"})
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MyBatisConfig.class})
@EnableTransactionManagement
public class SpringConfig {
}
```

‍

#### JdbcConfig

```java
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String username;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource dataSource = new DruidDataSource();
        dataSource.setDriverClassName(driver);
        dataSource.setUrl(url);
        dataSource.setUsername(username);
        dataSource.setPassword(password);
        return dataSource;
    }

    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager ds = new DataSourceTransactionManager();
        ds.setDataSource(dataSource);
        return ds;
    }
}
```

‍

#### MybatisConfig

```java
public class MyBatisConfig {
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean factoryBean = new SqlSessionFactoryBean();
        factoryBean.setDataSource(dataSource);
        factoryBean.setTypeAliasesPackage("com.itheima.domain");
        return factoryBean;
    }

    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();
        msc.setBasePackage("com.itheima.dao");
        return msc;
    }
}
```

‍

#### SpringMVCConfig

```java
@Configuration
@ComponentScan("com.itheima.controller")
@EnableWebMvc
public class SpringMvcConfig {
}
```

‍

#### ServletConfig_Web项目入口

```java
public class ServletConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    //加载Spring配置类
    protected Class<?>[] getRootConfigClasses() {
        return new Class[]{SpringConfig.class};
    }
    //加载SpringMVC配置类
    protected Class<?>[] getServletConfigClasses() {
        return new Class[]{SpringMvcConfig.class};
    }
    //设置SpringMVC请求地址拦截规则
    protected String[] getServletMappings() {
        return new String[]{"/"};
    }

    //设置post请求中文乱码过滤器
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("utf-8");
        return new Filter[]{filter};
    }
}
```

‍

‍

#### jdbc.properties

在resources下提供jdbc.properties. 设置数据库连接四要素

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm_db
jdbc.username=root
jdbc.password=2333
```

‍

‍

### 功能模块

‍

需求:对表tbl_book进行新增、修改、删除、根据ID查询和查询所有

‍

‍

#### 数据库表

MySQL

下面是数据库建表SQL语句

```sql
create database ssm_db character set utf8;
use ssm_db;
create table tbl_book(
  id int primary key auto_increment,
  type varchar(20),
  name varchar(50),
  description varchar(255)
)

insert  into `tbl_book`(`id`,`type`,`name`,`description`) values (1,'计算机理论','Spring实战 第五版','Spring入门经典教程，深入理解Spring原理技术内幕');
```

‍

#### 模型类

```java
public class Book {
    private Integer id;
    private String type;
    private String name;
    private String description;
    //getter...setter...toString省略
}
```

‍

#### Dao接口

这里用的是Mybatis手写注解SQL语句, 后面我会用MybatisPlus

```java
public interface BookDao {

//    @Insert("insert into tbl_book values(null,#{type},#{name},#{description})")
    @Insert("insert into tbl_book (type,name,description) values(#{type},#{name},#{description})")
    public void save(Book book);

    @Update("update tbl_book set type = #{type}, name = #{name}, description = #{description} where id = #{id}")
    public void update(Book book);

    @Delete("delete from tbl_book where id = #{id}")
    public void delete(Integer id);

    @Select("select * from tbl_book where id = #{id}")
    public Book getById(Integer id);

    @Select("select * from tbl_book")
    public List<Book> getAll();
}
```

‍

#### Service接口+实现类

```java
@Transactional
public interface BookService {
    /**
     * 保存
     * @param book
     * @return
     */
    public boolean save(Book book);

    /**
     * 修改
     * @param book
     * @return
     */
    public boolean update(Book book);

    /**
     * 按id删除
     * @param id
     * @return
     */
    public boolean delete(Integer id);

    /**
     * 按id查询
     * @param id
     * @return
     */
    public Book getById(Integer id);

    /**
     * 查询全部
     * @return
     */
    public List<Book> getAll();
}
```

‍

```java
@Service
public class BookServiceImpl implements BookService {
    @Autowired
    private BookDao bookDao;

    public boolean save(Book book) {
        bookDao.save(book);
        return true;
    }

    public boolean update(Book book) {
        bookDao.update(book);
        return true;
    }

    public boolean delete(Integer id) {
        bookDao.delete(id);
        return true;
    }

    public Book getById(Integer id) {
        return bookDao.getById(id);
    }

    public List<Book> getAll() {
        return bookDao.getAll();
    }
}
```

‍

**说明:**

* bookDao在Service中注入的会提示一个红线提示，为什么呢?

  * BookDao是一个接口，没有实现类，接口是不能创建对象的，所以最终注入的应该是代理对象
  * 代理对象是由Spring的IOC容器来创建管理的
  * IOC容器又是在Web服务器启动的时候才会创建
  * IDEA在检测依赖关系的时候，没有找到适合的类注入，所以会提示错误提示
  * 但是程序运行的时候，代理对象就会被创建，框架会使用DI进行注入，所以程序运行无影响.
* 如何解决上述问题?

  * 可以不用理会，因为运行是正常的
  * 设置错误提示级别

‍

#### Contorller

```java
@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private BookService bookService;

    @PostMapping
    public boolean save(@RequestBody Book book) {
        return bookService.save(book);
    }

    @PutMapping
    public boolean update(@RequestBody Book book) {
        return bookService.update(book);
    }

    @DeleteMapping("/{id}")
    public boolean delete(@PathVariable Integer id) {
        return bookService.delete(id);
    }

    @GetMapping("/{id}")
    public Book getById(@PathVariable Integer id) {
        return bookService.getById(id);
    }

    @GetMapping
    public List<Book> getAll() {
        return bookService.getAll();
    }
}
```

‍

‍

#### 单元测试

使用`Spring整合Junit`​单元测试

‍

测试类,注入Service

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(classes = SpringConfig.class)
public class BookServiceTest {

    @Autowired
    private BookService bookService;


}
```

‍

测试方法

```java
    @Test
    public void testGetById(){
        Book book = bookService.getById(1);
        System.out.println(book);
    }

    @Test
    public void testGetAll(){
        List<Book> all = bookService.getAll();
        System.out.println(all);
    }
```

‍

‍

### 统一结果封装

‍

表现层与前端数据传输**协议定义**

* 为了封装返回的结果数据:  ==创建结果模型类，封装数据到data属性中==
* 为了封装返回的数据是何种操作及是否操作成功:  ==封装操作结果到code属性中==
* 操作失败后为了封装返回的错误信息:  ==封装特殊消息到message(msg)属性中==

‍

设置统一数据返回结果类 **Result**

```java
public class Result{
	private Object data;
	private Integer code;
	private String msg;
}
```

‍

#### Result

可以自动生成

```java
public class Result {
    //描述统一格式中的数据
    private Object data;
    //描述统一格式中的编码，用于区分操作，可以简化配置0或1表示成功失败
    private Integer code;
    //描述统一格式中的消息，可选属性
    private String msg;

    public Result() {
    }
	//构造方法是方便对象的创建
    public Result(Integer code,Object data) {
        this.data = data;
        this.code = code;
    }
	//构造方法是方便对象的创建
    public Result(Integer code, Object data, String msg) {
        this.data = data;
        this.code = code;
        this.msg = msg;
    }
	//setter...getter...省略
}
```

‍

#### 返回码Code类

```java
//状态码
public class Code {
    public static final Integer SAVE_OK = 20011;
    public static final Integer DELETE_OK = 20021;
    public static final Integer UPDATE_OK = 20031;
    public static final Integer GET_OK = 20041;

    public static final Integer SAVE_ERR = 20010;
    public static final Integer DELETE_ERR = 20020;
    public static final Integer UPDATE_ERR = 20030;
    public static final Integer GET_ERR = 20040;
}

```

> 根据需要自行增减，例如将查询再进行细分为GET_OK,GET_ALL_OK,GET_PAGE_OK等

‍

‍

#### 改Controller返回值

‍

返回Result对象, 外套一层**三元表达式**来处理成功和失败情况

前端根据返回的结果，先从中获取`code`​,根据code判断，如果成功则取`data`​属性的值，如果失败，则取`msg`​中的值做提示

```java
//统一每一个控制器方法返回值
@RestController
@RequestMapping("/books")
public class BookController {

    @Autowired
    private BookService bookService;

    @PostMapping
    public Result save(@RequestBody Book book) {
        boolean flag = bookService.save(book);
        return new Result(flag ? Code.SAVE_OK:Code.SAVE_ERR,flag);
    }

    @PutMapping
    public Result update(@RequestBody Book book) {
        boolean flag = bookService.update(book);
        return new Result(flag ? Code.UPDATE_OK:Code.UPDATE_ERR,flag);
    }

    @DeleteMapping("/{id}")
    public Result delete(@PathVariable Integer id) {
        boolean flag = bookService.delete(id);
        return new Result(flag ? Code.DELETE_OK:Code.DELETE_ERR,flag);
    }

    @GetMapping("/{id}")
    public Result getById(@PathVariable Integer id) {
        Book book = bookService.getById(id);
        Integer code = book != null ? Code.GET_OK : Code.GET_ERR;
        String msg = book != null ? "" : "数据查询失败，请重试！";
        return new Result(code,book,msg);
    }

    @GetMapping
    public Result getAll() {
        List<Book> bookList = bookService.getAll();
        Integer code = bookList != null ? Code.GET_OK : Code.GET_ERR;
        String msg = bookList != null ? "" : "数据查询失败，请重试！";
        return new Result(code,bookList,msg);
    }
}
```

‍

‍

### 统一异常处理

‍

QA

1. 各个层级均出现异常，异常处理代码书写在哪一层?  
    ==所有的异常均抛出到表现层进行处理==
2. 异常的种类很多，表现层如何将所有的异常都处理到呢?  
    ==异常分类==
3. 表现层处理异常，每个方法中单独书写，代码书写量巨大且意义不强，如何解决?  
    ==AOP==

‍

#### 异常处理器

异常处理器: 集中的、统一的处理项目中出现的异常

创建异常处理器类返回结果给前端

‍

确保SpringMvcConfig能够扫描到异常处理器类

@RestControllerAdvice用于标识当前类为REST风格对应的异常处理器

@ExceptionHandler注解中可以添加参数，参数是某个异常类的class，代表这个方法专门处理该类异常

```java
@RestControllerAdvice
public class ProjectExceptionAdvice {
    //自定义的异常处理器，保留对Exception类型的异常处理，用于处理非预期的异常

    @ExceptionHandler(Exception.class)
    public Result doException(Exception ex){
      	System.out.println("嘿嘿,异常你哪里跑！")
        return new Result(666,null,"嘿嘿,异常你哪里跑！");
    }
}
```

‍

@RestControllerAdvice

|名称|@RestControllerAdvice|
| ------| ------------------------------------|
|类型|==类注解==|
|位置|Rest风格开发的控制器增强类定义上方|
|作用|为Rest风格开发的控制器类做增强|

**说明:** 此注解自带@ResponseBody注解与@Component注解，具备对应的功能

‍

@ExceptionHandler

|名称|@ExceptionHandler|
| ------| -------------------------------------------------------------------------------------------------|
|类型|==方法注解==|
|位置|专用于异常处理的控制器方法上方|
|作用|设置指定异常的处理方案，功能等同于控制器方法，<br />出现异常后终止原始控制器执行,并转入当前方法执行|

**说明：** 此类方法可以根据处理的异常不同，制作多个方法分别处理对应的异常

‍

#### 异常处理方案

‍

##### 分类

‍

因为异常的种类有很多，如果每一个异常都对应一个@ExceptionHandler，那得写多少个方法来处理各自的异常，所以我们在处理异常之前，需要对异常进行一个分类:

‍

1. 业务异常（BusinessException）

    * 规范的用户行为产生的异常

      * 用户在页面输入内容的时候未按照指定格式进行数据填写，如在年龄框输入的是字符串
    * 不规范的用户行为操作产生的异常

      * 如用户故意传递错误数据
2. 系统异常（SystemException）

    * 项目运行过程中可预计但无法避免的异常

      * 比如数据库或服务器宕机
3. 其他异常（Exception）

    * 编程人员未预期到的异常，如:用到的文件不存在

‍

异常的种类及出现异常的原因:

* 框架内部抛出的异常：因使用不合规导致
* 数据层抛出的异常：因外部服务器故障导致（例如：服务器访问超时）
* 业务层抛出的异常：因业务逻辑书写错误导致（例如：遍历业务书写操作，导致索引异常等）
* 表现层抛出的异常：因数据收集、校验等规则导致（例如：不匹配的数据类型间导致异常）
* 工具类抛出的异常：因工具类书写不严谨不够健壮导致（例如：必要释放的连接长期未释放等）

‍

##### 解决

* 业务异常（BusinessException）

  * 发送对应消息传递给用户，提醒规范操作

    * 大家常见的就是提示用户名已存在或密码格式不正确等
* 系统异常（SystemException）

  * 发送固定消息传递给用户，安抚用户

    * 系统繁忙，请稍后再试
    * 系统正在维护升级，请稍后再试
    * 系统出问题，请联系系统管理员等
  * 发送特定消息给运维人员，提醒维护

    * 可以发送短信、邮箱或者是公司内部通信软件
  * 记录日志

    * 发消息和记录日志对用户来说是不可见的，属于后台程序
* 其他异常（Exception）

  * 发送固定消息传递给用户，安抚用户
  * 发送特定消息给编程人员，提醒维护（纳入预期范围内）

    * 一般是程序没有考虑全，比如未做非空校验等
  * 记录日志

‍

##### 实现

‍

###### 自定义异常类

自定义异常处理器，用于封装异常信息，对异常进行分类

* 让自定义异常类继承`RuntimeException`​, 后期在抛出这两个异常的时候，就不用try...catch...或throws
* 添加`code`​属性更好的区分异常是来自哪个业务

```java
public class SystemException extends RuntimeException{
    private Integer code;

    public Integer getCode() {
        return code;
    }

    public void setCode(Integer code) {
        this.code = code;
    }

    public SystemException(Integer code, String message) {
        super(message);
        this.code = code;
    }

    public SystemException(Integer code, String message, Throwable cause) {
        super(message, cause);
        this.code = code;
    }

}
```

‍

‍

###### 将其他异常包成自定义异常

假如在BookServiceImpl的getById方法抛异常了: 包装

‍

具体的包装方式有：

* 方式一:`try{}catch(){}`​在catch中重新throw我们自定义异常即可.
* 方式二:直接throw自定义异常即可

```java
public Book getById(Integer id) {
    //模拟业务异常，包装成自定义异常
    if(id == 1){
        throw new BusinessException(Code.BUSINESS_ERR,"请不要使用你的技术挑战我的耐性!");
    }

    //模拟系统异常，将可能出现的异常进行包装，转换成自定义异常
    try{
        int i = 1/0;
    }catch (Exception e){
        throw new SystemException(Code.SYSTEM_TIMEOUT_ERR,"服务器访问超时，请重试!",e);
    }
    return bookDao.getById(id);
}
```

‍

为了使`code`​看着更专业些，在Code类中再新增需要的属性

```java
//状态码
public class Code {
    public static final Integer SAVE_OK = 20011;
    public static final Integer DELETE_OK = 20021;
    public static final Integer UPDATE_OK = 20031;
    public static final Integer GET_OK = 20041;

    public static final Integer SAVE_ERR = 20010;
    public static final Integer DELETE_ERR = 20020;
    public static final Integer UPDATE_ERR = 20030;
    public static final Integer GET_ERR = 20040;
    public static final Integer SYSTEM_ERR = 50001;
    public static final Integer SYSTEM_TIMEOUT_ERR = 50002;
    public static final Integer SYSTEM_UNKNOW_ERR = 59999;

    public static final Integer BUSINESS_ERR = 60002;
}
```

‍

###### 处理器类中处理自定义异常

```java
//@RestControllerAdvice用于标识当前类为REST风格对应的异常处理器
@RestControllerAdvice

public class ProjectExceptionAdvice {

    //@ExceptionHandler用于设置当前处理器类对应的异常类型
    @ExceptionHandler(SystemException.class)
    public Result doSystemException(SystemException ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        return new Result(ex.getCode(),null,ex.getMessage());
    }

    @ExceptionHandler(BusinessException.class)
    public Result doBusinessException(BusinessException ex){
        return new Result(ex.getCode(),null,ex.getMessage());
    }


    //除了自定义的异常处理器，保留对Exception类型的异常处理，用于处理非预期的异常
    @ExceptionHandler(Exception.class)
    public Result doOtherException(Exception ex){
        //记录日志
        //发送消息给运维
        //发送邮件给开发人员,ex对象发送给开发人员
        return new Result(Code.SYSTEM_UNKNOW_ERR,null,"系统繁忙，请稍后再试！");
    }
}
```

‍

‍

# 实操

‍

‍

‍

## BUGFIX

‍

### 静态资源映射

无法通过地址栏访问到后台资源, 需要往上套一层静态目录(static), 否则无法找到

想要的效果

```java
浏览器输入：http://localhost:8088/SystemData/UserData/Avatar/Mintimate.jpeg
可以直接访问项目文件下的：/SystemData/UserData/Avatar/Mintimate.jpeg，
浏览器输入：http://localhost:8088/SystemDataTest/UserData/Avatar/Mintimate.jpeg
可以直接访问项目文件下的：/Test/UserData/Avatar/Demo.jpeg，
```

‍

**配置MVCconfig静态资源映射**

‍

我的解决方法

改为.addResourceLocations("classpath:backend/");

同时调整目录结结构为直接放在对应的resources文件夹下

```java
@Slf4j
@Configuration
public class WebMvcConfig extends WebMvcConfigurationSupport {
    @Override
    protected void addResourceHandlers(ResourceHandlerRegistry registry) {
        log.info("开始进行静态资源映射...");
        registry.addResourceHandler("/backend/**")
                .addResourceLocations("classpath:static/backend/");
        //将classpath:static/backend/目录下的资源映射到请求/backend/路径下


        registry.addResourceHandler("/front/**")
                .addResourceLocations("classpath:static/front/");
    }
}
```

‍

大佬建议

添加一个配置类，并继承`WebMvcConfigurationSupport`​，实现`addResourceHandlers`​方法，并打上`@Configuration`​注解，使其成为配置类：

```java
// 静态资源映射
registry.addResourceHandler("/SystemData/**")
        .addResourceLocations("file:"+IMG_PATH);
registry.addResourceHandler("/SystemDataTest/**")
        .addResourceLocations("file:"+IMG_PATH_TWO);

```

‍

‍

**POST请求中文乱码**

解决方案:

配置过滤器ServletContainersInitConfig

CharacterEncodingFilter是在spring-web包中，所以用之前需要导入对应的jar包. 

```java
public class ServletContainersInitConfig extends AbstractAnnotationConfigDispatcherServletInitializer {
    protected Class<?>[] getRootConfigClasses() {
.......
    //乱码处理
    @Override
    protected Filter[] getServletFilters() {
        CharacterEncodingFilter filter = new CharacterEncodingFilter();
        filter.setEncoding("UTF-8");
        return new Filter[]{filter};
    }
}
```
