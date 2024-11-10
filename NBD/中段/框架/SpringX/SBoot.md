--Spring框架的框架

‍

‍

‍

直接基于Spring框架进行项目的开发依赖配置比较繁琐, Spring框架4.0版本之后，又推出了一个全新的框架：SpringBoot

学习SpringBoot的原理, 就是解析SpringBoot当中的起步依赖与自动配置的原理

通过SpringBoot来简化Spring框架的开发(是简化不是替代)

‍

### Header

‍

#### 插件

‍

##### Lombok

‍

模型类上添加注解

* @Setter:为模型类的属性提供setter方法
* @Getter:为模型类的属性提供getter方法
* @ToString:为模型类的属性提供toString方法
* @EqualsAndHashCode:为模型类的属性提供equals和hashcode方法
* ==@Data:是个组合注解，包含上面的注解的功能==
* ==@NoArgsConstructor:提供一个无参构造函数==
* ==@AllArgsConstructor:提供一个包含所有参数的构造函数==

‍

‍

‍

#### 环境

‍

##### 版本推荐

Spring 最近的推荐版本

```xml
3.1.5
```

‍

##### 官网构建

​`Idea`​ 使用了官网提供了快速构建 `SpringBoot`​ 工程的组件实现快速构建

```yml
https://spring.io/projects/spring-boot
```

‍

‍

##### 打包

在构建 `SpringBoot`​ 工程时已经在 `pom.xml`​ 中配置了如下插件

```xml
<plugin>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-maven-plugin</artifactId>
</plugin>
```

所以我们只需要使用 `Maven`​ 的 `package`​ 指令打包就会在 `target`​ 目录下生成对应的 `Jar`​ 包. 

> 注意：该插件必须配置，不然打好的 `jar`​ 包也是有问题的.

‍

‍

##### 启动

进入 `jar`​ 包所在位置，在 `命令提示符`​ 中输入如下命令

​`jar -jar XXX.jar`​

‍

‍

‍

# 知识

‍

‍

‍

## 评价

‍

### 优势

‍

==简化配置, 快速开发==

‍

SpringBoot的项目中起步依赖的一个共同特征：以`spring-boot-starter-`​开头. 每一个起步依赖，都用于开发一个特定的功能

SpringBoot框架之所以使用起来更简单更快捷，是因为SpringBoot框架底层提供了两个非常重要的功能：**起步依赖**，**自动配置**

1. 起步依赖简化pom文件当中依赖的配置
2. 自动配置简化框架在使用时bean的声明以及bean的配置

‍

‍

### 对比Spring

‍

对比一下 `Spring`​ 程序和 `SpringBoot`​ 程序. 

|类/配置文件|Spring|SpringBoot|
| ------------------------| ----------| ------------|
|pom文件中的坐标|手工添加|勾选添加|
|web3.e配置类|手工制作|无|
|Spring/SpringMVC配置类|手工制作|无|
|控制器|手工制作|手工制作|

‍

‍

* **坐标**  
  ​`Spring`​ 程序中的坐标需要自己编写，而且坐标非常多  
  ​`SpringBoot`​ 程序中的坐标是我们在创建工程时进行勾选自动生成的
* **web3.0配置类**  
  ​`Spring`​ 程序需要自己编写这个配置类. 这个配置类大家之前编写过，肯定感觉很复杂  
  ​`SpringBoot`​ 程序不需要我们自己书写
* **配置类**  
  ​`Spring/SpringMVC`​ 程序的配置类需要自己书写. 而 `SpringBoot`​ 程序则不需要书写. 

  ‍

  **编写** **​`web3.0`​**​ **的配置类**

  作为 `web`​ 程序，`web3.0`​ 的配置类不能缺少，而这个配置类还是比较麻烦的

  **编写** **​`SpringMVC`​**​ **的配置类**

  做到这只是将工程的架子搭起来. 要想被外界访问，最起码还需要提供一个 `Controller`​ 类，在该类中提供一个方法

  **编写** **​`Controller`​**​ **类**

‍

‍

​`SpringBoot`​ 程序优点恰巧就是针对 `Spring`​ 的缺点

* 自动配置. 这个是用来解决 `Spring`​ 程序配置繁琐的问题
* 起步依赖. 这个是用来解决 `Spring`​ 程序依赖设置繁琐的问题
* 辅助功能（内置服务器,...）. 我们在启动 `SpringBoot`​ 程序时既没有使用本地的 `tomcat`​ 也没有使用 `tomcat`​ 插件，而是使用 `SpringBoot`​ 内置的服务器.

‍

> 假如我们没有使用SpringBoot进行web程序的开发, 需要:
>
> spring-webmvc
>
> servlet-api
>
> jackson-databind(JSON处理工具包)
>
> aop依赖、aspect依赖
>
> --还需要保证版本匹配，否则就可能会出现版本冲突问题. 
>
> 使用了SpringBoot，因为Maven的依赖传递, 引入 springboot-starter-web即可

‍

‍

## 前置

‍

### Web请求过程分析

‍

大致流程: 浏览器选择目标IP地址, 访问对应端口下的服务器, 服务器处理请求调用相关资源, 之后响应给浏览器

‍

**浏览器**

输入网址：http://192.168.100.11:8080/hello

通过IP地址192.168.100.11定位到网络上的一台计算机,通过端口号8080找到计算机上运行的程序

‍

**服务器**

* 接收到浏览器发送的信息（如：/hello）
* 在服务器上找到/hello的资源
* 把资源发送给浏览器

‍

# 基础

‍

‍

## 目录

三层架构

‍

### 职责

* src/main/java：存放代码
* src/main/resources
* static: 存放静态文件，比如 css、js、image, （访问方式 [http://localhost:8080/js/main.js](http://localhost:8080/js/main.js)）
* templates:存放静态页面jsp,html,tpl
* config:存放配置文件,application.properties
* resources:

‍

### 静态文件加载

同个文件的加载顺序,静态资源文件 Spring Boot 默认会挨个从

* META/resources >
* resources >
* static >
* public

里面找是否存在相应的资源，如果有则直接返回，不在默认加载的目录，则找不到

‍

默认配置

* spring.resources.static-locations = classpath:/META-INF/resources/,classpath:/resources/,classpath:/static/,classpath:/public/

可以自己在配置文件中设定路径

‍

‍

### 要点

‍

#### 启动类位置

应用启动的位置(Application启动类位置)

‍

三种形式

* 当启动类和controller在同一类中时，在该类上添加注解@Controller即可；
* 当启动类和controller分开时，==启动类要放在根目录下==，启动类上只需要注解@SpringBootApplication；

  > 强烈推荐，不然漏配置扫描包，项目庞大，出现问题则难排查
  >
* 当启动类和controller分开时，如果启动类在非根目录下，需要在启动类中增加注解@ComponentScan，并配置需要扫描的包名，如(basePackages = )  
  @ComponentScan(basePackages ={"net.xdclass.controller","net.xdclass.service"}

‍

‍

#### Jar包

‍

**打包后的springboot里面的目录结构**

解压查看

```html
example.jar
 |
 +-META-INF
 |  +-MANIFEST.MF
 +-org
 |  +-springframework
 |     +-boot
 |        +-loader
 |           +-<spring boot loader classes>
 +-BOOT-INF
    +-classes
    |  +-mycompany
    |     +-project
    |        +-YourClasses.class
    +-lib
       +-dependency1.jar
       +-dependency2.jar
```

‍

‍

## 初始化

‍

‍

### 起步依赖

‍

在SpringBoot给我们提供的这些起步依赖当中，已提供了当前程序开发所需要的[所有的常见依赖](https://docs.spring.io/spring-boot/docs/2.7.7/reference/htmlsingle/#using.build-systems.starters), **起步依赖的原理就是Maven的依赖传递**

‍

#### 探索父工程

普通SpringBoot工程都有一个父工程, 依赖的版本号，在父工程中按照推荐的版本集合统一管理(成组给你打包好, 不用自己适配了)

当然也可以放弃采用这一工程的版本, 而是使用自己定义的版本集合

```html
  <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.0.6</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>
```

‍

‍

#### 探索父父工程

‍

​`properties`​ 标签中定义了各个技术软件依赖的版本，避免了我们在使用不同软件技术时考虑版本的兼容问题

​`dependencyManagement`​ 标签是进行依赖版本锁定，但是并没有导入对应的依赖；如果我们工程需要那个依赖只需要引入依赖的 `groupid`​ 和 `artifactId`​ 不需要定义 `version`​

而 `build`​ 标签中也对插件的版本进行了锁定,不难理解我们工程的的依赖为什么都没有配置 `version`​

‍

‍

#### 探索依赖

‍

​`pom.xml`​ 中配置了如下依赖

​`spring-web`​ 和 `spring-webmvc`​ 的依赖，这就是为什么我们的工程中没有依赖这两个包还能正常使用 `springMVC`​ 中的注解的原因. 

而依赖 `spring-boot-starter-tomcat`​ ，从名字基本能确认内部依赖了 `tomcat`​，所以我们的工程才能正常启动. 

‍

‍

**starter**

* ​`SpringBoot`​ 中常见项目名称，定义了当前项目使用的所有项目坐标，以达到减少依赖配置的目的

‍

**parent**

* 所有 `SpringBoot`​ 项目要继承的项目，定义了若干个坐标版本号（依赖管理，而非依赖），以达到减少依赖冲突的目的
* ​`spring-boot-starter-parent`​（2.5.0）与 `spring-boot-starter-parent`​（2.4.6）共计57处坐标版本不同

‍

‍

### **实际开发**

* 使用任意坐标时，仅书写GAV中的G和A，V由SpringBoot提供

  > G：groupid
  >
  > A：artifactId
  >
  > V：version
  >
* 如发生坐标错误，再指定version（要小心版本冲突）

‍

‍

### 程序启动

创建的每一个 `SpringBoot`​ 程序时都包含一个类似于下面的类，我们将这个类称作引导类

```java
@SpringBootApplication
public class Springboot01QuickstartApplication {
  
    public static void main(String[] args) {
        SpringApplication.run(Springboot01QuickstartApplication.class, args);
    }
}
```

‍

**注意**

* ​`SpringBoot`​ 在创建项目时，采用jar的打包方式
* ​`SpringBoot`​ 的引导类是项目的入口，运行 `main`​ 方法就可以启动项目  
  因为我们在 `pom.xml`​ 中配置了 `spring-boot-starter-web`​ 依赖，而该依赖通过前面的学习知道它依赖 `tomcat`​ ，所以运行 `main`​ 方法就可以使用 `tomcat`​ 启动咱们的工程.

‍

‍

### SBHW

基于Spring官方骨架，选择Maven创建工程, 基本信息描述完毕之后，勾选web开发SWeb

‍

**请求处理类**

HelloController通过@RestController注解为请求处理类, 通过@RequestMapping注解来指明处理请求路径

```java
@RestController // 该注解表示该类为请求处理类, 所有方法都将返回json格式数据
public class HelloController { // 该类为一个控制器类
  
    @RequestMapping("/hello") // http://localhost:8080/hello为该方法处理的访问路径
    public String hello() {
        System.out.println("Hello World ~");        return "Hello World ~";
    }//访问http://localhost:8080/hello
 }
```

‍

‍

## 搭建

‍

### 三层架构

基础实现

‍

**控制层：** 接收前端请求,处理并响应

```java
@RestController
public class EmpController {
    private EmpService empService = new EmpServiceA();//下一层业务层的对象

    @RequestMapping("/listEmp")
    public Result list(){
        List<Emp> empList = empService.listEmp();//调用service层, 获取数据  
        return Result.success(empList); //响应数据
```

‍

**业务逻辑层：** 处理具体的业务逻辑

```java
public interface EmpService {//业务逻辑接口（制定业务标准）
    //获取员工列表
    public List<Emp> listEmp();
}
```

```java
public class EmpServiceA implements EmpService {
    private EmpDao empDao = new EmpDaoA();

    @Override
    public List<Emp> listEmp() {
        List<Emp> empList = empDao.listEmp();        //1. 调用dao, 获取数据

        empList.stream().forEach(emp -> {       //2. 对数据进行转换处理 - gender, job
            //处理 gender 1: 男, 2: 女
            String gender = emp.getGender();
            if("1".equals(gender)){
                emp.setGender("男");

            String job = emp.getJob();//处理job - 1: 讲师, 2: 班主任 , 3: 就业指导
            if("1".equals(job)){
                emp.setJob("讲师");
......
        });
        return empList;
```

‍

**数据访问层：**  负责CURD

```java
//数据访问层接口（制定标准）
public interface EmpDao {
    public List<Emp> listEmp(); //获取员工列表数据
}
```

```java
//数据访问实现类
public class EmpDaoA implements EmpDao {
    @Override
    public List<Emp> listEmp() {
        //1. 加载并解析emp.xml
        String file = this.getClass().getClassLoader().getResource("emp.xml").getFile();
        System.out.println(file);
        List<Emp> empList = XmlParserUtils.parse(file, Emp.class);
        return empList;
    }
}
```

‍

‍

‍

### CRUD

‍

**部门管理**

```java
//部门管理控制器,限制访问方法为GET
@Slf4j //lombok提供的日志注解
@RestController
public class DeptController {
    //限定访问方法为GET: 两种方法:
    //1. @GetMapping("/dept") ,推荐
    //2. @RequestMapping(value="/dept", method=GET)
    @RequestMapping("/dept") //访问路径
    public Result list(){
        log.info("查询全部部门数据"); //采用更好的方法来记录日志
        return Result.success("查询成功");
    }// http://localhost:8080/dept
}
```

之后的开发路径: Service->Mapper, 略

前后联调: Nginx服务器直接丢到项目工程下, 访问90端口

‍

‍

**员工管理**

‍

##### 查

‍

**分页查询**

设置请求参数默认值

@RequestParam(defaultValue="默认值")

封装一个查询结果对象, 返回数据和总记录数

```java
@Data
@NoArgsConstructor
@AllArgsConstructor
public class PageBean {
 private Long total; //总记录数
 private List rows; //当前页数据列表
}
```

‍

**条件分页查询实现**

‍

**EmpController**

```java
@Slf4j
@RestController
@RequestMapping("/emps")
public class EmpController {

    @Autowired
    private EmpService empService;

    //条件分页查询
    @GetMapping
    public Result page(@RequestParam(defaultValue = "1") Integer page,
                       @RequestParam(defaultValue = "10") Integer pageSize,
                       String name, Short gender,
                       @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate begin,
                       @DateTimeFormat(pattern = "yyyy-MM-dd") LocalDate end) {
        //记录日志
        log.info("分页查询，参数：{},{},{},{},{},{}", page, pageSize,name, gender, begin, end);
        //调用业务层分页查询功能
        PageBean pageBean = empService.page(page, pageSize, name, gender, begin, end);
        //响应
        return Result.success(pageBean);
    }
}
```

‍

**EmpService**

```java
public interface EmpService {
    /**
     * 条件分页查询
     * @param page     页码
     * @param pageSize 每页展示记录数
     * @param name     姓名
     * @param gender   性别
     * @param begin   开始时间
     * @param end     结束时间
     * @return
     */
    PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin, LocalDate end);
}
```

‍

**EmpServiceImpl**

```java
@Slf4j
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;

    @Override
    public PageBean page(Integer page, Integer pageSize, String name, Short gender, LocalDate begin, LocalDate end) {
        //设置分页参数
        PageHelper.startPage(page, pageSize);
        //执行条件分页查询
        List<Emp> empList = empMapper.list(name, gender, begin, end);
        //获取查询结果
        Page<Emp> p = (Page<Emp>) empList;
        //封装PageBean
        PageBean pageBean = new PageBean(p.getTotal(), p.getResult());
        return pageBean;
    }
}
```

‍

**EmpMapper**

```java
@Mapper
public interface EmpMapper {
    //获取当前页的结果列表
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
}
```

‍

抽取编写MYSQL

**EmpMapper.xml**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="cwx.mapper.EmpMapper">

    <!-- 条件分页查询 -->
    <select id="list" resultType="cwx.pojo.Emp">
        select * from emp
        <where>
            <if test="name != null and name != ''">
                name like concat('%',#{name},'%')
            </if>
            <if test="gender != null">
                and gender = #{gender}
            </if>
            <if test="begin != null and end != null">
                and entrydate between #{begin} and #{end}
            </if>
        </where>
        order by update_time desc
    </select>
</mapper>
```

‍

‍

##### 增

‍

**EmpController**

```java
@Slf4j
@RestController
@RequestMapping("/emps")
public class EmpController {

    @Autowired
    private EmpService empService;

    @PostMapping //新增
    public Result save(@RequestBody Emp emp){
        log.info("新增员工, emp:{}",emp);
        empService.save(emp);
        return Result.success();
    }
}
```

‍

**EmpService**

```java
public interface EmpService {
    void save(Emp emp);
}
```

‍

**EmpServiceImpl**

```java
@Slf4j
@Service
public class EmpServiceImpl implements EmpService {
    @Autowired
    private EmpMapper empMapper;

    @Override
    public void save(Emp emp) {
        //补全数据
        emp.setCreateTime(LocalDateTime.now());
        emp.setUpdateTime(LocalDateTime.now());
        //调用添加方法
        empMapper.insert(emp);
    }

}
```

‍

**EmpMapper**

```java
@Mapper
public interface EmpMapper {
    //新增员工
    @Insert("insert into emp (username, name, gender, image, job, entrydate, dept_id, create_time, update_time) " +
            "values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime});")
    void insert(Emp emp);

}
```

‍

‍

##### 改

‍

xml绑定文件

EmpMapper.xml

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.itheima.mapper.EmpMapper">

    <!--更新员工信息-->
    <update id="update">
        update emp
        <set>
            <if test="username != null and username != ''">
                username = #{username},
            </if>
            <if test="password != null and password != ''">
                password = #{password},
            </if>
            <if test="name != null and name != ''">
                name = #{name},
            </if>
            <if test="gender != null">
                gender = #{gender},
            </if>
            <if test="image != null and image != ''">
                image = #{image},
            </if>
            <if test="job != null">
                job = #{job},
            </if>
            <if test="entrydate != null">
                entrydate = #{entrydate},
            </if>
            <if test="deptId != null">
                dept_id = #{deptId},
            </if>
            <if test="updateTime != null">
                update_time = #{updateTime}
            </if>
        </set>
        where id = #{id}
    </update>

    <!-- 省略... -->
   
</mapper>
```

‍

‍

### 启动

‍

启动类型

* IDEA开发中启动

  * 本地开发中常用
* 外置Tomcat中启动

  * 接近淘汰
  * tomcat版本兼容问题复杂
  * 微服务容器化部署复杂
* Jar方式打包启动

  * 官方推荐，工作中最常用
  * 步骤：pom文件新增maven插件

```html
<build>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                </plugin>
            </plugins>
</build>

如果没有加，则执行jar包 ，报错如下
java -jar spring-boot-demo-0.0.1-SNAPSHOT.jar
no main manifest attribute, in spring-boot-demo-0.0.1-SNAPSHOT.jar
```

‍

必备打包、启动命令

```html
构建：mvn install
构建跳过测试类 mvn install -Dmaven.test.skip=true 

target目录下有对应的jar包就是打包后项目

进到对应的target目录启动 java -jar xxxxx.jar  即可
想后台运行，就用守护进程 nohup java -jar xxx.jar &
```

‍

‍

## 请求

‍

### 注解

‍

一个顶两的注解

```
@GetMapping = @RequestMapping(method = RequestMethod.GET)
@PostMapping = @RequestMapping(method = RequestMethod.POST)
@PutMapping = @RequestMapping(method = RequestMethod.PUT)
@DeleteMapping = @RequestMapping(method = RequestMethod.DELETE)
```

‍

‍

### 简单参数

‍

‍

#### ==原始方式==

仅做了解, 不会使用到

在原始的Web程序当中，需要通过Servlet中提供的API：HttpServletRequest<sup>(请求对象)</sup>获取请求的相关信息. 不仅繁琐,而且要我们用户手动进行类型转换

```json
//根据指定的参数名获取请求参数的数据值语法
String  request.getParameter("参数名")
```

```java
@RestController
public class RequestController { // 该类为一个控制器类
    //原始方式
    @RequestMapping("/simpleParam") // http://localhost:8080/simpleParam?name=Tom&age=10
    public String simpleParam(HttpServletRequest request) {
        // http://localhost:8080/simpleParam?name=Tom&age=10
        // 请求参数： name=Tom&age=10   （有2个请求参数）
        // 第1个请求参数： name=Tom   参数名:name，参数值:Tom
        // 第2个请求参数： age=10     参数名:age , 参数值:10

        String name = request.getParameter("name");//name就是请求参数名
        String ageStr = request.getParameter("age");//age就是请求参数名

        int age = Integer.parseInt(ageStr);//需要手动进行类型转换
        System.out.println(name + "  :  " + age);
        return "OK";
    }
}
```

‍

#### ==SB方式==

‍

对原始的API进行了封装，接收参数的形式更加简单. 简单参数参数名与形参变量名相同，通过注解的方式定义同名的形参即可接收参数, 会进行自动匹配以及类型转换; 如果参数对应不上,不会报错, 只是显示null

不匹配，可以参数前使用 @RequestParam<sup>( 含义: 绑定一个确认对象的变量, 后面接受的数据一个个匹配进去)</sup> 然后通过value属性执行请求参数名来完成映射.

@RequestParam中的required属性默认为true代表该请求参数必须传递; 如果该参数是可选的置为false

‍

```java
@RestController
public class RequestController { 
    @RequestMapping("/simpleParam")
    public String simpleParam(String name, Integer age) {
        System.out.println(name + "  :  " + age); //在控制台打印请求参数
        return "OK"; //给浏览器返回一个字符串"OK"
    }    // http://localhost:8080/simpleParam?name=Tom&age=10
}
```

‍

‍

### 实体参数

‍

如果请求参数比较多，考虑将请求参数封装到一个实体类对象中

‍

遵守如下规则：**请求参数名**与**实体类的属性名**相同

复杂实体对象(类中还有类):复杂实体对象, (.)操作符引出

‍

POJO实体类包的简单实体对象举例

```java
public class User { // 该类为一个实体类
    private String name;
    private Integer age;
    //get,set,toString
}
```

‍

Request处理类中Controller

```java
    @RequestMapping("/simplePojo")
    public String simplePojo(User user) { //要求请求参数名和实体类的属性名保持一致
        System.out.println(user);  
        return "OK"; 
    }
```

‍

‍

### 数组参数

‍

前端请求时两种传递形式：?hobby=game&hobby=java  ?hobby=game,java

请求参数名与形参**数组名称**相同且多个，定义数组类型形参即可

```java
    @RequestMapping("/arrayParam")
    public String arrayParam(String[] hobby) {   //数组集合参数, 传递多个同名参数
        System.out.println(Arrays.toString(hobby)); //在控制台打印请求参数
    }
```

‍

‍

### 集合参数

‍

请求参数名与形参**集合对象名**相同且多个，需要 @RequestParam 绑定参数关系

```java
    @RequestMapping("/listParam")
    public String listParam(@RequestParam List<String> hobby){ 
        System.out.println(hobby);
    }
```

‍

‍

### JSON参数

‍

‍

服务端Controller接收JSON

* 传递json格式的参数，在Controller中会使用实体类进行封装
* 封装规则：**JSON数据键名与形参对象属性名相同，定义POJO类型形参即可接收参数. 需要使用 **​ **==@RequestBody==**​**标识. **

‍

```java
    @RequestMapping("/jsonParam")
    public String jsonParam(@RequestBody User user){ //@RequestBody注解表示请求参数是一个JSON格式字符串,需要封装到实体类中
    }
```

‍

‍

### 路径参数

‍

通过请求URL直接传递参数，使用{…}来标识该路径参数，需要使用 @PathVariable 获取路径参数

‍

前端：通过请求URL直接传递参数

后端：使用{…}来标识该路径参数，需要使用@PathVariable获取路径参数

‍

‍

请求地址动态不能写死, 需要 **{id}插值表达式**来接受

```java
    @RequestMapping("/path/{id}")
    public String pathParam(@PathVariable Integer id){
```

‍

传递多个路径参数则直接斜杠分割第二个参数

```java
@RequestMapping("/path/{id}/{name}") //注意{}以及/的组合使用
    public String pathParam2(@PathVariable Integer id, @PathVariable String name){
```

‍

‍

### 日期参数

‍

日期的格式多种多样, 在进行封装时需要通过@DateTimeFormat注解，以及其pattern属性来设置日期的格式

 @DateTimeFormat注解的pattern属性中指定了哪种日期格式

后端controller方法中需要使用Date类型或LocalDateTime类型来封装传递的参数

```java
    @RequestMapping("/dateParam")
    public String dateParam(@DateTimeFormat(pattern = "yyyy-MM-dd HH:mm:ss") LocalDateTime updateTime){ //这里的LocalDateTime是java8的日期时间类
        System.out.println(updateTime);
    }
```

‍

## 响应

‍

controller方法中的return利用@ResponseBody注解响应给浏览器

在类上添加的@Rest Controller其实是一个组合注解(@Controller + @ResponseBody)

添加@RestController就相当于添加了@ResponseBody, 表示当前类下所有的方法返回值为响应数据; 返回值如果是一个POJO对象或集合时，会先转换为JSON格式再响应

‍

‍

### **统一响应类R**

> 一般被定义为Result类

‍

前端按照统一格式的返回结果进行解析(仅一种解析方案)，就可以拿到数据

统一的返回结果使用类来描述，在这个结果中包含三个信息

* 响应状态码：当前请求是成功，还是失败
* 状态码信息：给页面的提示信息
* 返回的数据：给前端响应的数据（字符串、对象、集合）

‍

‍

‍

==Result示例==

```java
public class Result {
    private Integer code;//响应码，1 代表成功; 0 代表失败
    private String msg;  //响应码 描述字符串
    private Object data; //返回的数据
    //Getset 构造省略

    //增删改 成功响应(不需要给前端返回数据)
    public static Result success(){
        return new Result(1,"success",null);
    }
    //查询 成功响应(把查询结果做为返回数据响应给前端)
    public static Result success(Object data){
        return new Result(1,"success",data);
    }
    //失败响应
    public static Result error(String msg){
        return new Result(0,msg,null);
    }
}
```

‍

==Controller示例==

```java
@RestController
public class ResponseController { //响应统一格式的结果
    @RequestMapping("/hello")
    public Result hello(){
        //return new Result(1,"success","Hello World ~");
        return Result.success("Hello World ~");
    }

    //响应统一格式的结果
    @RequestMapping("/getAddr")
    public Result getAddr(){
        Address addr = new Address();
        addr.setProvince("广东");
        addr.setCity("深圳");
        return Result.success(addr);
    }
}
```

‍

‍

‍

## IOC

‍

> 注解使用    参考Spring文档

* 如果是在项目当中自己的类交给IOC容器管理直接使用 **@Component**以及它的衍生注解来声明
* 第三方依赖需要在配置类中定义一个加上 **@Bean注解**的方法

‍

注解@Component以及三个衍生注解声明IOC容器中的bean对象

|注解|说明|位置|
| -------------| --------------| --------------------|
|@Component|基础注解|不属于以下三类|
|@Controller|@C的衍生注解|标注在控制器类上|
|@Service|@C的衍生注解|标注在业务类上|
|@Repository|@C的衍生注解|标注在数据访问类上|

‍

声明bean的时候，可以通过value属性**指定bean的名字**; 没有指定默认为类名首字母小写

使用以上注解都可以声明bean，但是在springboot集成web开发中，**声明控制器bean只能用@Controller**

‍

‍

### 获取Bean

‍

默认情况下，SpringBoot项目在启动的时候会自动的<sup>（还会受到作用域及延迟初始化影响，这里主要针对于默认的单例非延迟加载的bean而言. ）</sup>创建IOC容器(也称为Spring容器)，并且在启动的过程当中会自动的将bean对象都创建存放在IOC容器当中. 应用程序在运行时需要依赖什么bean对象，就直接进行依赖注入就可以了. 

‍

‍

主动从**注入的IOC容器**中获取到bean对象

1. 根据**name**获取bean

    ```java
    Object getBean(String name)
    ```
2. 根据**类型**获取bean

    ```java
    <T> T getBean(Class<T> requiredType)
    ```
3. 根据name获取bean（带**类型转换**）

    ```java
    <T> T getBean(String name, Class<T> requiredType)
    ```

‍

‍

### Bean作用域

@Scope     配置作用域

‍

singleton 单件默认只有一个实例

prototype 创建新的实例

|**作用域**|**说明**|
| -------------| -------------------------------------------------|
|singleton|容器内同名称的bean只有一个实例（单例）（默认）|
|prototype|每次使用该bean时会创建新的实例（非单例）|
|request|每个请求范围内会创建新的实例（web环境中，了解）|
|session|每个会话范围内会创建新的实例（web环境中，了解）|
|application|每个应用范围内会创建新的实例（web环境中，了解）|

‍

‍

### Bean初始化

@Lazy      延迟初始化(到第一次使用)

‍

‍

### 三方Bean

\@Bean注解    

第三方提供的类是只读的, 无法在其上添加@Component或衍生注解

‍

‍

#### 配置类

配置类定义 @Bean

‍

单独定义一个**配置类**(config>>CommonConfig)

**方法上加上一个@Bean注解**，Spring容器启动时自动调用方法并将返回值声明为容器当中的Bean对象. 

如果第三方bean需要依赖其它bean，直接在bean定义方法中**设置形参**即可，容器会根据类型自动装配

通过@Bean注解的name或value属性声明bean的名称，不指定则默认bean的名称就是方法名

‍

```java
@Configuration //配置类  (在配置类当中对第三方bean进行集中的配置管理)
public class CommonConfig {
    //声明第三方bean
    @Bean //将当前方法的返回值对象交给IOC容器管理, 成为IOC容器bean
          //通过@Bean注解的name/value属性指定bean名称, 如果未指定, 默认是方法名
    public SAXReader reader(DeptService deptService){
        System.out.println(deptService);
        return new SAXReader();
    }
}
```

‍

#### 启动类

启动类上添加@Bean, 这样太蠢了...不建议使用（项目中要保证启动类的纯粹性）

‍

```java
@SpringBootApplication
public class SpringbootWebConfig2Application {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootWebConfig2Application.class, args);
    }

    //声明第三方bean
    @Bean //将当前方法的返回值对象交给IOC容器管理, 成为IOC容器bean
    public SAXReader saxReader(){
        return new SAXReader();
    }
}
```

‍

Test中注入SAXReader

```java
class SpringbootWebConfig2ApplicationTests {

    @Autowired
    private SAXReader saxReader;

    //第三方bean的管理
    @Test
    public void testThirdBean() throws Exception {

        Document document = saxReader.read(this.getClass().getClassLoader().getResource("1.xml"));
        Element rootElement = document.getRootElement();
        String name = rootElement.element("name").getText();
        String age = rootElement.element("age").getText();

        System.out.println(name + " : " + age);
    }
    //省略其他代码...
}
```

‍

‍

### 组件扫描

‍

使用四大注解声明的bean，要想生效还需要被组件扫描注解@ComponentScan扫描,默认扫描的范围是SpringBoot启动类所在包及其子包. 可以手动添加@ComponentScan注解，指定要扫描的包

‍

> 按照规范组织, 将定义的三层包都放在引导类所在包的子包下，定义的bean就会被自动扫描

‍

‍

## DI

> 见Spring文档

‍

### @Autowired

@Autowired注解自动装配，默认按照**类型**进行自动装配

‍

‍

‍

### @Resource

@Resource与@Autowired 区别

* @Autowired  是spring框架提供的注解，而@Resource是JDK提供的注解
* @Autowired  按照类型注入   @Resource按照名称注入

‍

‍

### 示例

Controller层、Service层、Dao层的代码解耦完整的三层代码

‍

**Controller层**

```java
@RestController
public class EmpController {
    @Autowired //运行时,从IOC容器中获取该类型对象,赋值给该变量
    private EmpService empService ;

    @RequestMapping("/listEmp")
    public Result list(){
        //1. 调用service, 获取数据
        List<Emp> empList = empService.listEmp();
        //3. 响应数据
        return Result.success(empList);
    }
}
```

‍

**Service层**

```java
@Component //将当前对象交给IOC容器管理,成为IOC容器的bean
public class EmpServiceA implements EmpService {
    @Autowired //运行时,从IOC容器中获取该类型对象,赋值给该变量
    private EmpDao empDao ;

    @Override
    public List<Emp> listEmp() {
        List<Emp> empList = empDao.listEmp(); //调用dao, 获取数据
    }
}
```

‍

**Dao层**

```java
@Component //将当前对象交给IOC容器管理,成为IOC容器的bean
public class EmpDaoA implements EmpDao {
    }
```

‍

‍

‍

## 自动配置

‍

> 当Spring容器启动后，一些配置类、bean对象就自动存入到了IOC容器中，完成了创建不需要我们手动去声明.
>
> 解析自动配置的原理就是分析在 SpringBoot项目当中引入对应的依赖之后, 是如何将依赖jar包当中所提供的bean以及配置类直接加载到当前项目的SpringIOC容器当中的

‍

**自动配置原理就是在配置类中定义一个@Bean标识的方法，而Spring会自动调用配置类中使用@Bean标识的方法，并把方法的返回值注册到IOC容器中**

‍

> Q: 引入进来的**第三方依赖**当中的**bean**以及**配置类**为什么没有生效？(经典扫描不到问题)
>
> * 在类上添加@Component注解来声明bean对象之后，还需要保证@Component注解能被Spring的组件扫描到.
> * SpringBoot项目中的@SpringBootApplication注解，具有包扫描的作用，但是它只会扫描**启动类所在的当前包以及子包**.

‍

### 方法

‍

#### 一 启动类组件扫描

Spring的@ComponentScan注解扫描: 使用繁琐, 性能低, SpringBoot中并没有采用

```java
@SpringBootApplication
@ComponentScan({"com.itheima","com.example"}) //指定要扫描的包
public class SpringbootWebConfigApplication
```

‍

‍

#### 二 @Import

使用@Import导入的类会被Spring加载到IOC容器中

‍

启动类  @Import(HeaderConfig.class)

‍

‍

#### 三 导入ImportSelector接口实现类

ImportSelector接口是spring中导入外部配置的核心接口

‍

##### 实现类

```java
public class MyImportSelector implements ImportSelector {
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        //返回值字符串数组（数组中封装了全限定名称的类）
        return new String[]{"com.example.HeaderConfig"};
    }
}
```

##### 启动类

```java
@Import(MyImportSelector.class)
```

‍

‍

基于以上方式完成自动配置，当要引入一个第三方依赖时还要知道第三方依赖中有哪些配置类和哪些Bean对象, 很不友好，而且比较繁琐.

‍

#### 四 @EnableXxxxx注解

> 第三方依赖自身最清楚到底有哪些bean和配置类, 第三方依赖自己指定bean对象和配置类, 是SpringBoot当中所采用的方式

比较常见的方案就是第三方依赖给提供一个注解，这个注解一般都以@EnableXxxx开头的注解，注解中封装的就是@Import注解

‍

‍

* 第三方依赖中提供的注解

```java
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Import(MyImportSelector.class)//指定要导入哪些bean对象或配置类
public @interface EnableHeaderConfig { 
}
```

在使用时只需在启动类上加上@EnableXxxxx注解即可

‍

‍

### 原理分析

‍

> ==注意==   这里记录的主讲用的是2.0, 技术栈有不同

‍

#### @SBA**源码跟踪**

‍

自动配置原理源码入口就是@SpringBootApplication注解，在这个注解中封装了3个注解

1. @SpringBootConfiguration

    * 声明当前类是一个配置类
2. @ComponentScan

    * 进行组件扫描（SpringBoot中默认扫描的是启动类所在的当前包及其子包）
3. @EnableAutoConfiguration

    * 封装了@Import注解（Import注解中指定了一个ImportSelector接口的实现类）
    * 在实现类重写的selectImports()方法，读取当前项目下所有依赖jar包中META-INF/spring.factories、META-INF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports两个文件里面定义的配置类（配置类中定义了@Bean注解标识的方法）.

‍

当SpringBoot程序启动时，就会加载配置文件当中所定义的配置类，并将这些配置类信息(类的全限定名)封装到String类型的数组中，最终通过@Import注解将这些配置类全部加载到Spring的IOC容器中，交给IOC容器管理. 

从**SpringBoot启动类**上使用的核心注解@SpringBootApplication开始分析

‍

启动类代码固定, 在@**SpringBootApplication**注解中包含了：

* 元注解（单一功能, JavaEE里面的）
* @SpringBootConfiguration (表示是配置类)
* @EnableAutoConfiguration (Enable开头)
* @ComponentScan (组件扫描)

‍

==@SpringBootConfiguration==

‍

    @Configuration    表明SpringBoot启动类就是一个配置类. 

    @Indexed    用来加速应用启动的（不用关心）. 

‍

‍

==@EnableAutoConfiguration ==​**自动配置核心注解**

‍

    @Import注解导入实现ImportSelector接口的实现类AutoConfigurationImportSelector

‍

==@ComponentScan==

‍

    用来进行组件扫描，扫描启动类所在包及其子包下所有被@Component及其衍生注解声明的类

    SpringBoot启动类，之所以具备扫描包功能，就是因为包含了@ComponentScan注解. 

‍

‍

#### 条件装配注解

@Conditional注解

自动配置类声明bean的时候，除了在方法上加了一个@Bean注解以外，还会经常用到以**Conditional开头**的这一类条件装配的注解的注解

‍

作用    按照一定的条件进行判断，在满足给定条件后才会注册对应的bean对象到Spring的IOC容器中. 

位置    方法、类

‍

@Conditional本身是一个父注解，派生出大量的子注解：

* @ConditionalOnClass：判断环境中有对应字节码文件，才注册bean到IOC容器.
* @ConditionalOnMissingBean：判断环境中没有对应的bean(类型或名称)，才注册bean到IOC容器.
* @ConditionalOnProperty：判断配置文件中有对应属性和值，才注册bean到IOC容器.

‍

‍

#### 核心

 **@EnableAutoConfiguration**是自动配置的核心

‍

它封装了一个@Import注解，Import注解里面指定了一个ImportSelector接口的实现类. 

在这个实现类中，重写了ImportSelector接口中的selectImports()方法. 

而selectImports()方法中会去读取两份配置文件，并将配置文件中定义的配置类做为selectImports()方法的返回值返回，返回值代表的就是需要将哪些类交给Spring的IOC容器进行管理. 

‍

那么所有自动配置类的中声明的bean都会加载到Spring的IOC容器中吗? 并不会，因为这些配置类中在声明bean时，通常都会添加@Conditional开头的注解，这个注解就是**进行条件装配**. 而Spring会根据Conditional注解有选择性的进行bean的创建. 

@Enable 开头的注解底层，它就封装了一个注解 import 注解，它里面指定了一个类，是 ImportSelector 接口的实现类. 在实现类当中，我们需要去实现 ImportSelector 接口当中的一个方法 selectImports 这个方法. 这个方法的返回值代表的就是我需要将哪些类交给 spring 的 IOC容器进行管理. 

‍

此时它会去读取两份配置文件，一份儿是 spring.factories，另外一份儿是 autoConfiguration.imports. 而在 autoConfiguration.imports 这份儿文件当中，它就会去配置大量的自动配置的类. 

而前面我们也提到过这些所有的自动配置类当中，所有的 bean都会加载到 spring 的 IOC 容器当中吗？其实并不会，因为这些配置类当中，在声明 bean 的时候，通常会加上这么一类@Conditional 开头的注解. 这个注解就是进行条件装配. 所以SpringBoot非常的智能，它会根据 @Conditional 注解来进行条件装配. 只有条件成立，它才会声明这个bean，才会将这个 bean 交给 IOC 容器管理. 

‍

‍

## 配置文件

‍

‍

> ​

‍

‍

**内部属性**

创建对应的配置文件

‍

**外部属性**

通过IDE运行配置修改 VM options<sup>(系统属性参数)</sup>, Program arguments<sup>(命令行参数)</sup>

‍

项目已经打包上线后: 使用命令：java <sub>系统属性</sub> -jar XXX.jar <sub>命令行参数</sub>

运行jar程序时设置Java系统属性(前置-jar)和命令行参数(.jar后置)

```
java -Dserver.port=9000 -jar XXXXX.jar --server.port=10010
```

‍

‍

### 优先级

‍

三类内部属性配置 + 两种外部属性配置, 推荐统一使用一种格式的配置

**优先级**    ==命令行参数 &gt; 系统属性参数 &gt; properties参数 &gt; yml参数 &gt; ==​==*yaml参数*==

1. 命令行参数 （格式：--key=value）

    > --server.port=10010
    >
2. Java系统属性 （格式： -Dkey=value）

    > -Dserver.port=9000
    >
3. properties配置文件
4. yml配置文件
5. yaml配置文件(忽略)

‍

‍

#### 位置

SpringBoot 定义了配置文件不同的放置的位置, 不同位置的优先级是不同的. 级别越高优先级越高

‍

SpringBoot 中4级配置文件放置位置

* 1级：classpath：application.yml
* 2级：classpath：config/application.yml
* 3级：file ：application.yml
* 4级：file ：config/application.yml

‍

‍

### 格式

优先使用Yml

SpringBoot 程序的配置文件名必须是 application ，只是后缀名不同而已. 

‍

**格式对比**

1. application.properties

    ```properties
    server.port=8080
    server.address=127.0.0.1
    ```
2. application.yml

    ```yaml
    server:
      port: 8080
      address: 127.0.0.1
    ```
3. application.yaml

    ```yaml
    server:
      port: 8080
      address: 127.0.0.1
    ```

‍

#### properties

‍

‍

‍

#### yml

具体分为

* ​`.yml`​ (主流)
* ​`.yaml`​

‍

‍

##### **优点**

* 容易阅读  
  ​`yaml`​ 类型的配置文件比 `xml`​ 类型的配置文件更容易阅读，结构更加清晰
* 容易与脚本语言交互
* 以数据为核心，重数据轻格式  
  ​`yaml`​ 更注重数据，而 `xml`​ 更注重格式

‍

##### **语法**

* 大小写敏感
* 数值前边必须有空格，作为分隔符
* 使用缩进表示层级关系，缩进时，不允许使用Tab键，只能用空格（idea中会自动将Tab转换为空格）
* 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
* ​`#`​表示注释，从这个字符一直到行尾，都会被解析器忽略

‍

==核心规则：数据前面要加空格与冒号隔开==

> 数组数据在数据书写位置的下方使用减号作为数据开始符号，每行书写一个数据，减号与数据间空格分隔，例如

```yaml
enterprise:
  name: itcast
  age: 16
  tel: 4006184000
  subject:
    - Java
    - 前端
    - 大数据
```

‍

‍

### 参数

‍

‍

#### 简化注入

 **@ConfigurationProperties**

@Value注解只能一个一个的进行外部属性的注入, @ConfigurationProperties可以批量的将外部的属性配置注入到bean对象的属性中

> 在application.properties或者application.yml中配置了阿里云OSS的四项参数之后，如果java程序中需要这四项参数数据，我们直接通过@Value注解来进行注入. 需要注入的属性较多写起来就会比较繁琐
>
> 简化这些配置参数的注入, 直接将配置文件中配置项的值**自动的注入**到对象的属性中

‍

##### 步骤

1. 需要创建一个实现类，且实体类中的属性名和配置文件当中**key的名字必须要一致**

    > 比如：配置文件当中叫endpoints，实体类当中的属性也得叫endpoints，另外实体类当中的属性还需要提供 getter / setter方法
    >
2. 需要将实体类交给Spring的IOC容器管理，成为IOC容器当中的bean对象
3. 在实体类上添加`@ConfigurationProperties`​注解，并通过perfix属性来指定配置参数项的前缀  
    ​`ConfigurationProperties(prefix =aliyun.oss")`​

‍

引入依赖, 自动的识别被`@Configuration Properties`​注解标识的bean对象

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
</dependency>
```

‍

> 如果要注入的属性非常的多，并且还想做到复用，就可以定义这么一个bean对象. 通过 configuration properties 批量的将外部的属性配置直接注入到 bin 对象的属性当中. 在其他的类当中，我要想获取到注入进来的属性，直接注入 bin 对象然后调用 get 方法，就可以获取到对应的属性值了

‍

‍

### 读取

‍

#### @Value

@Value，获取配置文件中的数据, 通常用于外部配置的属性注入

```java
@Value("${配置文件中的key}")
```

‍

使用 ​@Value("表达式")​ 注解可以从配置文件中读取数据，注解中用于读取属性名引用方式是：​${一级属性名.二级属性名……}​

‍

##### 示例

```java
@RestController
@RequestMapping("/books")
public class BookController {
  
    @Value("${lesson}")
    private String lesson;
    @Value("${server.port}")
    private Integer port;
   
```

‍

‍

#### Environment对象

> 框架内容大量使用，但是在开发中很少使用.

‍

上面方式读取到的数据特别零散，​SpringBoot​ 还可以使用 ​@Autowired​ 注解注入 ​Environment​ 对象的方式读取数据. 这种方式 SpringBoot​ 会将配置文件中所有的数据封装到 ​Environment​ 对象中，如果需要使用哪个数据只需要通过调用 ​Environment​ 对象的 getProperty(String name)​ 方法获取. 

‍

##### 示例

```java
@RestController
@RequestMapping("/books")
public class BookController {
  
    @Autowired
    private Environment env;
  
    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(env.getProperty("lesson"));
        System.out.println(env.getProperty("enterprise.name"));
        System.out.println(env.getProperty("enterprise.subject[0]"));
        return "hello , spring boot!";
    }
}
```

‍

‍

‍

#### 自定义对象

‍

将配置文件中的数据封装到我们自定义的实体类对象中的方式

‍

##### 步骤

1. 将实体类 `bean`​​ 的创建交给 `Spring`​​ 管理.   
    在类上添加 `@Component`​​ 注解
2. 使用 `@ConfigurationProperties`​​ 注解表示加载配置文件  
    在该注解中也可以使用 `prefix`​​ 属性指定只加载指定前缀的数据
3. 在 `BookController`​​ 中进行注入

‍

##### 注意

使用自定义对象，在实体类上有如下警告提示: `Spring Boot Configuration Annotation Processor not configured`​

添加如下依赖即可

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

‍

##### **示例**

​`Enterprise`​ 实体类

```java
@Component
@ConfigurationProperties(prefix = "enterprise")
public class Enterprise {
    private String name;
    private int age;
    private String tel;
    private String[] subject;
...生成
```

‍

​`BookController`​ 

```java
@RestController
@RequestMapping("/books")
public class BookController {
  
    @Autowired
    private Enterprise enterprise;

    @GetMapping("/{id}")
    public String getById(@PathVariable Integer id){
        System.out.println(enterprise.getName());
        System.out.println(enterprise.getAge());
        System.out.println(enterprise.getSubject());
        System.out.println(enterprise.getTel());
        System.out.println(enterprise.getSubject()[0]);
        return "hello , spring boot!";
    }
}
```

‍

‍

### 多环境配置

切换环境时只需要改一个配置即可. 不同类型的配置文件多环境开发的配置都不相同

> 对于开发环境、测试环境、生产环境的配置肯定都不相同，比如我们开发阶段会在自己的电脑上安装 `mysql`​ ，连接自己电脑上的 `mysql`​ 即可，但是项目开发完毕后要上线就需要该配置，将环境的配置改为线上环境的

‍

‍

#### yaml方式

在 `application.yml`​ 中使用 `---`​ 来分割不同的配置，内容如下

```yaml
#开发
spring:
  profiles: dev #给开发环境起的名字
server:
  port: 80
---
#生产
spring:
  profiles: pro #给生产环境起的名字
server:
  port: 81
---
```

‍

spring.profiles 是用来给不同的配置起名字的. 

‍

告知 SpringBoot 使用哪段配置

```yaml
#设置启用的环境
spring:
  profiles:
    active: dev

---
#开发
spring:
  profiles: dev
server:
  port: 80
---
#生产
spring:
  profiles: pro
server:
  port: 81
---
#测试
spring:
  profiles: test
server:
  port: 82
---
```

‍

‍

‍

> 在上面配置中给不同配置起名字的 `spring.profiles`​ 配置项已经过时. 最新用来起名字的配置项是
>
> ```yaml
> #开发
> spring:
>   config:
>     activate:
>       on-profile: dev
> ```

‍

‍

#### properties方式

需要定义不同的配置文件

​`SpringBoot`​ 只会默认加载名为 `application.properties`​ 的配置文件，所以需要在其中中设置启用哪个配置文件

```properties
spring.profiles.active=pro
```

‍

‍

#### 命令行启动参数设置

使用 SpringBoot 开发的程序以后都是打成 jar 包，通过 `java -jar xxx.jar`​ 的方式启动服务的

SpringBoot 提供了在运行 jar 时设置开启指定的环境的方式

```
java –jar xxx.jar –-spring.profiles.active=test
```

临时修改端口号

```
java –jar xxx.jar –-server.port=88
```

同时设置多个配置，比如即指定启用哪个环境配置，又临时指定端口

```
java –jar springboot.jar –-server.port=88 –-spring.profiles.active=test
```

‍

### 实操

‍

#### 配置属性技巧

‍

> SpringBoot 内置属性过多且所有属性集中在一起修改，在使用时很不方便.

通过提示键+关键字修改属性  
例如要设置日志的级别时，可以在配置文件中书写 `logging`​，就会提示出来

‍

## [组件]()

‍

### 日志框架

> 另见调试 - 日志文档

‍

‍

# 高级

‍

## **bean操作**

‍

‍

### bean**继承**

两个类之间大多数的属性都相同，避免重复配置，通过bean标签的parent属性重用已有的Bean元素的配置信息 继承指的是配置信息的复用，和Java类的继承没有关系

```xml
<bean id="video" class="net.xdclass.sp.domain.Video" scope="singleton">

        <property name="id" value="9"/>
        <property name="title" value="Spring 5.X课程" />

</bean>


<bean id="video2" class="net.xdclass.sp.domain.Video2" scope="singleton" parent="video">

        <property name="summary" value="这个是summary"></property>

</bean>
```

‍

‍

### 属性依赖

如果类A是作为类B的属性, 想要类A比类B先实例化，设置两个Bean的依赖关系

```xml
<bean id="video" class="net.xdclass.sp.domain.Video" scope="singleton">

        <property name="id" value="9"/>
        <property name="title" value="Spring 5.X课程" />

</bean>

<!--设置两个bean的关系，video要先于videoOrder实例化-->

<bean id="videoOrder" class="net.xdclass.sp.domain.VideoOrder" depends-on="video">
        <property name="id" value="8" />
        <property name="outTradeNo" value="23432fnfwedwefqwef2"/>
        <property name="video" ref="video"/>
</bean>
```

‍

### 后置处理器

BeanPostProcessor

bean的二次加工

‍

* 是Spring IOC容器给我们提供的一个扩展接口
* 在调用初始化方法前后对 Bean 进行额外加工，ApplicationContext 会自动扫描实现了BeanPostProcessor的 bean，并注册这些 bean 为后置处理器
* 是Bean的统一前置后置处理而不是基于某一个bean

‍

* 执行顺序

  ```
  Spring IOC容器实例化Bean
  调用BeanPostProcessor的postProcessBeforeInitialization方法
  调用bean实例的初始化方法
  调用BeanPostProcessor的postProcessAfterInitialization方法
  ```
* 注意：接口重写的两个方法不能返回null，如果返回null那么在后续初始化方法将报空指针异常或者通过getBean()方法获取不到bean实例对象

```java
public class CustomBeanPostProcessor implements BeanPostProcessor,Ordered {

    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {

        System.out.println("CustomBeanPostProcessor1 postProcessBeforeInitialization beanName="+beanName);

        return bean;
    }

    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println("CustomBeanPostProcessor1 postProcessAfterInitialization beanName="+beanName);
        return bean;
    }


    public int getOrder() {
        return 1;
    }
}

```

* 可以注册多个BeanPostProcessor顺序

  * 在Spring机制中可以指定后置处理器调用顺序，通过BeanPostProcessor接口实现类实现Ordered接口getOrder方法，该方法返回整数，默认值为 0优先级最高，值越大优先级越低

‍

### 允许bean覆盖

两个配置类都想创建bean对象, 需要允许bean覆盖

‍

yml

```java
spring:
  main:
    banner-mode: off # 关闭启动时的图标
    allow-bean-definition-overriding: true #允许覆盖bean定义
```

‍

## MockMvc

‍

**简介：讲解Springboot的MockMvc调用api层接口**

‍

如何测试Controller对外提供的接口

* 增加类注解 @AutoConfigureMockMvc
* 注入一个MockMvc类
* 相关API ：

  * perform执行一个RequestBuilder请求
  * andExpect：添加ResultMatcher->MockMvcResultMatchers验证规则
  * andReturn：最后返回相应的MvcResult->Response

‍

‍

修改核心:

```java
  MvcResult mvcResult =  mockMvc.perform(MockMvcRequestBuilders.get("/api/v1/pub/video/list"))
                .andExpect(MockMvcResultMatchers.status().isOk()).andReturn();

```

```java
	@Autowired
    private MockMvc mockMvc;

    @Test
    public void testVideoListApi()throws Exception{

       MvcResult mvcResult =  mockMvc.perform(MockMvcRequestBuilders.get("/api/v1/pub/video/list"))
                .andExpect(MockMvcResultMatchers.status().isOk()).andReturn();

       int status = mvcResult.getResponse().getStatus();

       System.out.println(status);

        //会乱码
        //String result = mvcResult.getResponse().getContentAsString();

        // 使用下面这个，增加 编码 说明，就不会乱码打印
        String result = mvcResult.getResponse().getContentAsString(Charset.forName("utf-8"));

       System.out.println(result);

    }
```

‍

## 任务

‍

### 启动任务

在Spring boot项目的实际开发中，我们有时需要项目服务启动时加载一些数据或预先完成某些动作。为了解决这样的问题，Spring boot 为我们提供了一个方法：通过实现接口 CommandLineRunner 来实现这样的需求。

实现方式：只需要一个类即可，无需其他配置。

```java
@Component
@Order(value=1)
public class CommandLineRunnerImpl implements CommandLineRunner {

    @Override
    public void run(String... args) throws Exception {
            // DOTO
    }
}
```

@Order注解标识了执行顺序，Order值越小，优先级越大，越先执行

‍

### 定时任务

‍

常见定时任务工具

* Java自带的java.util.Timer类配置比较麻烦，时间延后问题
* Quartz框架: 配置更简单,xml或者注解适合分布式或者大型调度作业
* SpringBoot框架自带

‍

SpringBoot注解开启定时任务

* 启动类里面 @EnableScheduling开启定时任务，自动扫描
* 定时任务业务类 加注解 @Component被容器扫描
* 定时执行的方法加上注解 @Scheduled(fixedRate=2000) 定期执行一次

‍

#### **常用定时任务表达式配置和在线生成器**

‍

* cron 定时任务表达式 @Scheduled(cron="*/1 * * * * *") 表示每秒

  * crontab 工具 [https://tool.lu/crontab/](https://tool.lu/crontab/)
* fixedRate: 定时多久执行一次（上一次开始执行时间点后xx秒再次执行；）
* fixedDelay: 上一次执行结束时间点后xx秒再次执行

‍

### 异步任务

‍

> 使用场景：适用于处理log、发送邮件、短信……等
>
> * 下单接口->查库存 1000
> * 余额校验 1500
> * 风控用户1000

‍

#### EnableAsync

‍

* 启动类里面使用@EnableAsync注解开启功能，自动扫描
* 定义异步任务类并使用@Component标记组件被容器扫描,异步方法加上@Async

‍

‍

‍

#### Future

‍

注意点

* 要把异步任务封装到类里面，不能直接写到Controller
* 增加Future 返回结果 AsyncResult("task执行完成");
* 如果需要拿到结果 需要判断全部的 task.isDone()

‍

## 跨域

‍

‍

> ‍
>
> 使用 HttpServletResponse 在 Controller 层传递参数来控制跨域请求，这是一种手动设置响应头的方法，可以实现局部跨域
>
> ‍
>
> Controller
>
> ```java
> @RequestMapping("/index")
> public String index(HttpServletResponse response) {
>   response.addHeader("Access-Allow-Control-Origin","*");
>   return "index";
> }
> ```
>
> 这种方法并不是 Servlet 整合 Springboot 的技术栈，而是 Springboot 提供的一种原生的 API，可以直接使用它来操作底层的 Servlet 对象. Springboot 本身就是基于 Spring 和 Servlet 的框架，它对 Servlet 进行了封装和简化，让您可以更方便地开发 Web 应用.
>
> ‍
>
> ‍
>
> ‍
>
> ‍
>
> 返回新的 CorsFilter，这是一种全局跨域的方法，可以在任意配置类中返回一个新的 CorsFilter Bean，并添加映射路径和具体的 CORS 配置信息. 例如，您可以在您的配置类中添加如下代码：
>
> ```java
> @Configuration
> public class GlobalCorsConfig {
>   @Bean
>   public CorsFilter corsFilter() {
>     //1. 添加 CORS配置信息
>     CorsConfiguration config = new CorsConfiguration();
>     //放行哪些原始域
>     config.addAllowedOrigin("*");
>     //是否发送 Cookie
>     config.setAllowCredentials(true);
>     //放行哪些请求方式
>     config.addAllowedMethod("*");
>     //放行哪些原始请求头部信息
>     config.addAllowedHeader("*");
>     //暴露哪些头部信息
>     config.addExposedHeader("*");
>     //2. 添加映射路径
>     UrlBasedCorsConfigurationSource corsConfigurationSource = new UrlBasedCorsConfigurationSource();
>     corsConfigurationSource.registerCorsConfiguration("/**",config);
>     //3. 返回新的CorsFilter
>     return new CorsFilter(corsConfigurationSource);
>   }
> }
> ```
>
> ‍
>
> 重写 WebMvcConfigurer，这也是一种全局跨域的方法，可以在任意配置类中实现 WebMvcConfigurer 接口，并重写 addCorsMappings 方法，添加 CORS 配置信息. 例如，您可以在您的配置类中添加如下代码：
>
> ```java
> @Configuration
> public class CorsConfig implements WebMvcConfigurer {
>   @Override
>   public void addCorsMappings(CorsRegistry registry) {
>     registry.addMapping("/**")
>       //是否发送Cookie
>       .allowCredentials(true)
>       //放行哪些原始域
>       .allowedOrigins("*")
>       //放行哪些请求方式
>       .allowedMethods(new String[]{"GET", "POST", "PUT", "DELETE"})
>       //放行哪些原始请求头部信息
>       .allowedHeaders("*")
>       //暴露哪些头部信息
>       .exposedHeaders("*");
>   }
> }
> ```
>
> ‍
>
> 使用注解 @CrossOrigin，这是一种局部跨域的方法，可以在控制器类或方法上使用 @CrossOrigin 注解，指定允许跨域的原始域. 例如，您可以在您的控制器类或方法上添加如下注解：
>
> ```java
> @RestController
> @CrossOrigin(origins = "*") //表示该类的所有方法允许跨域
> public class HelloController {
>   @RequestMapping("/hello")
>   @CrossOrigin(origins = "*") //表示该方法允许跨域
>   public String hello() {
>     return "hello world";
>   }
> }
> ```
>
> ‍
>
> 使用自定义 Filter 实现跨域，这是一种全局跨域的方法，可以编写一个自定义的 Filter 类，实现 Filter 接口，并在 doFilter 方法中设置响应头，然后将该类注解为 @Component，让 Springboot 自动扫描并注册该 Filter. 例如，您可以编写如下的 Filter 类：
>
> ```java
> @Component
> public class MyCorsFilter implements Filter {
>   public void doFilter(ServletRequest req, ServletResponse res, FilterChain chain) throws IOException, ServletException {
>     HttpServletResponse response = (HttpServletResponse) res;
>     response.setHeader("Access-Control-Allow-Origin", "*");
>     response.setHeader("Access-Control-Allow-Methods", "POST, GET, OPTIONS, DELETE");
>     response.setHeader("Access-Control-Max-Age", "3600");
>     response.setHeader("Access-Control-Allow-Headers", "x-requested-with,content-type");
>     chain.doFilter(req, res);
>   }
>   public void init(FilterConfig filterConfig) {}
>   public void destroy() {}
> }
> ```
>
> ‍

‍

```
     1）JSONP
     2）Http响应头配置允许跨域
             nginx层配置 https://www.cnblogs.com/hawk-whu/p/6725699.html
     3）程序代码中处理 SpringBoot 通过拦截器配置
   
      //表示接受任意域名的请求,也可以指定域名
     response.setHeader("Access-Control-Allow-Origin", request.getHeader("origin"));
   
     //该字段可选，是个布尔值，表示是否可以携带cookie
     response.setHeader("Access-Control-Allow-Credentials", "true");
   
     response.setHeader("Access-Control-Allow-Methods", "GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS");
   
     response.setHeader("Access-Control-Allow-Headers", "*");
```

* options请求，这个需要注意
* 注意点: 假如接口报错，则跨域配置可能不生效（报错建议断点调试，排除异常错误）

‍

‍

## AOP

‍

‍

### 概念

面向切面编程, 面向特定方法编程, 在不改动这些原始方法的基础上，针对特定的方法进行功能的增强. （无侵入性: 解耦）

‍

**应用场景**

* 记录系统的操作日志
* 权限控制
* 事务管理：我们前面所讲解的Spring事务管理，底层其实也是通过AOP来实现的，只要添加@Transactional注解之后，AOP程序自动会在原始方法运行前先来开启事务，在原始方法运行完毕之后提交或回滚事务

‍

**优势**

* 代码无侵入：没有修改原始的业务方法，就已经对原始的业务方法进行了功能的增强或者是功能的改变
* 减少了重复代码
* 提高开发效率
* 维护方便

‍

‍

#### 原理

‍

Spring的AOP底层是基于动态代理技术来实现的，也就是说在程序运行的时候，会自动的基于动态代理技术为目标对象生成一个对应的代理对象. 在代理对象当中就会对目标对象当中的原始方法进行功能的增强. 

‍

‍

### 要素

‍

#### **目标对象**

‍

**Target**，通知所应用的对象

‍

‍

#### **连接点**

‍

**JoinPoint**，**可以被AOP控制的方法**, 封装了连接点方法在执行时的相关信息, 在SpringAOP当中连接点特指方法的执行, JoinPoint抽象了连接点，用它可以获得方法执行时的相关信息

‍

* 对于@Around通知，获取连接点信息只能使用ProceedingJoinPoint类型
* 对于其他四种通知，获取连接点信息只能使用JoinPoint，它是ProceedingJoinPoint的父类型

‍

##### 示例

```java
@Slf4j
@Component
@Aspect
public class MyAspect7 {

    @Pointcut("@annotation(com.itheima.anno.MyLog)")
    private void pt(){}
   
    //前置通知
    @Before("pt()")
    public void before(JoinPoint joinPoint){
        log.info(joinPoint.getSignature().getName() + " MyAspect7 -> before ...");
    }
  
    //后置通知
    @Before("pt()")
    public void after(JoinPoint joinPoint){
        log.info(joinPoint.getSignature().getName() + " MyAspect7 -> after ...");
    }

    //环绕通知
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable {
        //获取目标类名
        String name = pjp.getTarget().getClass().getName();
        log.info("目标类名：{}",name);

        //目标方法名
        String methodName = pjp.getSignature().getName();
        log.info("目标方法名：{}",methodName);

        //获取方法执行时需要的参数
        Object[] args = pjp.getArgs();
        log.info("目标方法参数：{}", Arrays.toString(args));

        //执行原始方法
        Object returnValue = pjp.proceed();

        return returnValue;
    }
}
```

‍

‍

#### **通知**

‍

**Advice**，特定的重复的逻辑，体现为一个方法, 也就是共性功能<sup>（AOP面向切面编程当中，我们只需要将重复的代码逻辑抽取出来单独定义. 抽取出来的这一部分重复的逻辑，也就是共性的功能. ）</sup>

‍

‍

#### **切入点**

‍

**PointCut**，匹配连接点的条件，通知仅会在切入点方法执行时被应用

在通知当中所定义的共性功能到底要应用在哪些方法上涉及到了切入点pointcut概念. 切入点指的是匹配连接点的条件. 通知仅会在切入点方法运行时才会被应用. 通常会通过一个**切入点表达式**来描述切入点

‍

‍

#### 切入点表达式

‍

决定项目中的哪些方法需要加入通知

‍

‍

##### execution切入点表达式

* 根据所指定的方法的描述信息来匹配切入点方法，这种方式也是最为常用的一种方式
* 如果我们要匹配的切入点方法的方法名不规则，或者有一些比较特殊的需求，通过execution切入点表达式描述比较繁琐

```
execution(访问修饰符?  返回值  包名.类名.?方法名(方法参数) throws 异常?)
```

根据方法的返回值、包名、类名、方法名、方法参数等信息来匹配, `?`​表示可以省略的部分

* 访问修饰符  可省略（比如: public、protected）
* 包名.类名  可省略
* throws 异常  可省略（注意是方法上声明抛出的异常，不是实际抛出的异常）

‍

###### Tips

* 所有业务方法名在命名时尽量规范，方便切入点表达式快速匹配. 如：查询类方法都是 find 开头，更新类方法都是update开头

  ```java
  //业务类
  @Service
  public class DeptServiceImpl implements DeptService {

      public List<Dept> findAllDept() {
         //省略代码...
      }

      public Dept findDeptById(Integer id) {
         //省略代码...
      }

      public void updateDeptById(Integer id) {
         //省略代码...
      }

      public void updateDeptByMoreCondition(Dept dept) {
         //省略代码...
      }
      //其他代码...
  }
  ```

  ‍

  ```java
  //匹配DeptServiceImpl类中以find开头的方法
  execution(* com.itheima.service.impl.DeptServiceImpl.find*(..))
  ```
* 描述切入点方法通常基于接口描述，而不是直接描述实现类，增强拓展性

  ```java
  execution(* com.itheima.service.DeptService.*(..))
  ```
* 在满足业务需要的前提下，尽量缩小切入点的匹配范围. 如：包名匹配尽量不使用 ..，使用 * 匹配单个包

  ```java
  execution(* com.itheima.*.*.DeptServiceImpl.find*(..))
  ```

‍

‍

##### annotation 切入点表达式

基于注解的方式来匹配切入点方法. 这种方式虽然多一步操作，我们需要自定义一个注解，但是相对来比较灵活. 我们需要匹配哪个方法，就在方法上加上对应的注解就可以了

‍

@annotation

设定自定义注解简化书写, 匹配多个无规则<sup>（比如：list()和 delete()）</sup>的方法

1. 编写自定义注解
2. 业务类要作为连接点的方法上添加该自定义注解

‍

###### 示例

自定义注解：MyLog, 去找加了这个注释的方法

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyLog {
}
```

```java
    @Override
    @MyLog //自定义注解（表示：当前方法属于目标方法）
    public List<Dept> list() {
        List<Dept> deptList = deptMapper.list();
        //模拟异常
        //int num = 10/0;
        return deptList;
    }
```

‍

‍

##### 抽取

@PointCut

公共的切入点表达式抽取出来，需要用到时引用即可

‍

简化示例

```java
@Slf4j
@Component
@Aspect
public class MyAspect1 {

    //切入点方法（公共的切入点表达式）
    @Pointcut("execution(* com.itheima.service.*.*(..))")
    private void pt(){

    }

    //前置通知（引用切入点）
    @Before("pt()")
    public void before(JoinPoint joinPoint){
        log.info("before ...");

    }

    //环绕通知
    @Around("pt()")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        log.info("around before ...");

        //调用目标对象的原始方法执行
        Object result = proceedingJoinPoint.proceed();
        //原始方法在执行时：发生异常
        //后续代码不在执行

        log.info("around after ...");
        return result;
    }

    //后置通知
    @After("pt()")
    public void after(JoinPoint joinPoint){
        log.info("after ...");
    }

    //返回后通知（程序在正常执行的情况下，会执行的后置通知）
    @AfterReturning("pt()")
    public void afterReturning(JoinPoint joinPoint){
        log.info("afterReturning ...");
    }

    //异常通知（程序在出现异常的情况下，执行的后置通知）
    @AfterThrowing("pt()")
    public void afterThrowing(JoinPoint joinPoint){
        log.info("afterThrowing ...");
    }
}
```

‍

‍

‍

#### **切面**

‍

**Aspect**，描述通知与切入点的对应关系（= 通知+切入点）

当通知和切入点结合在一起就形成了一个切面, 描述当前aop程序需要针对于哪个原始方法，在什么时候执行什么样的操作

‍

‍

‍

‍

### 通知类型

‍

* @Around：环绕通知，此注解标注的通知方法在目标方法**前、后都被执行**
* @Before：前置通知，此注解标注的通知方法在目标方法**前被执行**
* @After ：后置通知，此注解标注的通知方法在目标方法**后被执行**，无论是否有异常都会执行(最终通知)
* @AfterReturning ： 返回后通知，此注解标注的通知方法在目标方法后被执行，**有异常不会执行**
* @AfterThrowing ： 异常后通知，此注解标注的通知方法**发生异常后执行**

‍

‍

#### 作用范围

‍

当切入点方法使用private修饰时，仅能在当前切面类中引用该表达式; 当外部其他切面类中也要引用当前类中的切入点表达式，就需要把private改为public，并采用全类名.方法名形式

‍

##### 示例

```java
@Slf4j
@Component
@Aspect
public class MyAspect2 {
    //引用MyAspect1切面类中的切入点表达式
    @Before("com.itheima.aspect.MyAspect1.pt()")
    public void before(){
        log.info("MyAspect2 -> before ...");
    }
}
```

‍

‍

### 通知顺序

‍

不同切面类中，默认按照切面类的类名字母排序

* 目标方法前的通知方法：字母排名靠前的**先执行**
* 目标方法后的通知方法：字母排名靠前的**后执行**

‍

控制

1. ~~修改切面类的类名~~
2. @Order

‍

‍

@Order示例

```java
@Slf4j
@Component
@Aspect
@Order(2)  //切面类的执行顺序（前置通知：数字越小先执行; 后置通知：数字越小越后执行）
public class MyAspect2 {
    //前置通知
    @Before("execution(* com.itheima.service.*.*(..))")
    public void before(){
        log.info("MyAspect2 -> before ...");
    }

    //后置通知 
    @After("execution(* com.itheima.service.*.*(..))")
    public void after(){
        log.info("MyAspect2 -> after ...");
    }
}
```

‍

### 实例

‍

#### 公共字段切面+反射实现自动填充

‍

赋值方式为

1). 在新增数据时, 将createTime、updateTime 设置为当前时间, createUser、updateUser设置为当前登录用户ID.

2). 在更新数据时, 将updateTime 设置为当前时间, updateUser设置为当前登录用户ID.

**使用AOP切面编程，实现功能增强，来完成公共字段自动填充功能.**

‍

**实现步骤：**

1). 自定义注解 AutoFill，用于标识需要进行公共字段自动填充的方法

2). 自定义切面类 AutoFillAspect，统一拦截加入了 AutoFill 注解的方法，通过反射为公共字段赋值

3). 在 Mapper 的方法上加入 AutoFill 注解

‍

##### 自定义注解

com.sky.annotation包

‍

自定义注解公共写法

OperationType枚举

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface AutoFill {
    //数据库操作类型：UPDATE INSERT
    OperationType value();
}
```

使用枚举来做

```java
/**
 * 数据库操作类型
 */
public enum OperationType {

    /**
     * 更新操作
     */
    UPDATE,

    /**
     * 插入操作
     */
    INSERT

}
```

‍

‍

##### **自定义切面**

**AutoFillAspect**

com.sky.aspect包

```java
/**
 * 自定义切面，实现公共字段自动填充处理逻辑
 */
@Aspect
@Component
@Slf4j
public class AutoFillAspect {

    /**
     * 切入点
     */
    @Pointcut("execution(* com.sky.mapper.*.*(..)) && @annotation(com.sky.annotation.AutoFill)")
    public void autoFillPointCut(){}

    /**
     * 前置通知，在通知中进行公共字段的赋值
     */
    @Before("autoFillPointCut()")
    public void autoFill(JoinPoint joinPoint){
        log.info("开始进行公共字段自动填充...");

        //获取到当前被拦截的方法上的数据库操作类型
        MethodSignature signature = (MethodSignature) joinPoint.getSignature();//方法签名对象
        AutoFill autoFill = signature.getMethod().getAnnotation(AutoFill.class);//获得方法上的注解对象
        OperationType operationType = autoFill.value();//获得数据库操作类型

        //获取到当前被拦截的方法的参数--实体对象
        Object[] args = joinPoint.getArgs();
        if(args == null || args.length == 0){
            return;
        }

        Object entity = args[0];

        //准备赋值的数据
        LocalDateTime now = LocalDateTime.now();
        Long currentId = BaseContext.getCurrentId();

        //根据当前不同的操作类型，为对应的属性通过反射来赋值
        if(operationType == OperationType.INSERT){
            //为4个公共字段赋值
            try {
                Method setCreateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_TIME, LocalDateTime.class);
                Method setCreateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_CREATE_USER, Long.class);
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                //通过反射为对象属性赋值
                setCreateTime.invoke(entity,now);
                setCreateUser.invoke(entity,currentId);
                setUpdateTime.invoke(entity,now);
                setUpdateUser.invoke(entity,currentId);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }else if(operationType == OperationType.UPDATE){
            //为2个公共字段赋值
            try {
                Method setUpdateTime = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_TIME, LocalDateTime.class);
                Method setUpdateUser = entity.getClass().getDeclaredMethod(AutoFillConstant.SET_UPDATE_USER, Long.class);

                //通过反射为对象属性赋值
                setUpdateTime.invoke(entity,now);
                setUpdateUser.invoke(entity,currentId);
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
}
```

‍

##### 应用注解

在Mapper接口的方法上加入 AutoFill 注解

```java
  @AutoFill(value = OperationType.INSERT)
    void insert(Category category);

  @AutoFill(value = OperationType.UPDATE)
    void update(Category category);
```

‍

后续就不需要手动插入了

删除分类数据不需要设置修改时间等

```java
        //设置创建时间、修改时间、创建人、修改人
        category.setCreateTime(LocalDateTime.now());
        category.setUpdateTime(LocalDateTime.now());
        category.setCreateUser(BaseContext.getCurrentId());
        category.setUpdateUser(BaseContext.getCurrentId());
```

‍

‍

‍

## 事务

@Transactional

> 见Spring

‍

SB一般会在业务层当中来控制事务，因为在业务层当中，一个业务功能可能会包含多个数据访问的操作. 在业务层来控制事务，我们就可以将多个数据访问操作控制在一个事务范围内

‍

### 配置

‍

#### **事务管理日志**

方便调试

```java
#spring事务管理日志
logging:
  level:
    org.springframework.jdbc.support.JdbcTransactionManager: debug
```

‍

### 使用

‍

在​具体实现类​上添加​@Transactional​注解，同时也要在主启动类上加上​@EnableTransactionManagement​注解

‍

## 校验

‍

### 情境

> 服务器端接收到浏览器发送过来的请求之后首先要对请求进行校验. 先要校验一下用户登录了没有，如果用户已经登录了，就直接执行对应的业务操作就可以了；如果用户没有登录，此时就不允许他执行相关的业务操作，直接给前端响应一个错误的结果，最终跳转到登录页面，要求他登录成功之后，再来访问对应的数据. 
>
> HTTP协议特性是无状态协议, 每一次请求都是独立的，下一次请求并不会携带上一次请求的数据: 登陆之后执行其他业务操作时，服务器也并不知道这个员工到底登陆了没有. 因为两次请求之间是独立的
>
> 真正的登录功能应该是：在浏览器发起请求时，需要在服务端进行统一拦截进行登录校验; 登陆后才能访问后端系统页面，需要将用户登录成功的信息存起来记录成功标记; 不登陆则跳转登陆页面进行登陆.

‍

‍

## 技术栈联动

‍

* 会话跟踪(3)

  一种维护浏览器状态的方法，服务器需要识别多次请求是否来自于同一浏览器，以便在同一次会话的多次请求间共享数据

  > 浏览器与服务器之间的一次连接称为一次会话. 第一次访问服务器的时候会话就建立了，直到有任何一方断开连接. 在一次会话当中，是可以包含多次请求和响应的. 
  >
  > 会话是和浏览器关联的，当有三个浏览器客户端和服务器建立了连接时，就会有三个会话. 同一个浏览器在未关闭之前请求了多次服务器，这多次请求是属于同一个会话. 
  >

  1. Cookie客户端会话跟踪技术
  2. Session服务端会话跟踪技术
  3. 令牌技术
* 统一拦截(2)

  > 携带令牌(JWT)到服务端，服务端需要统一拦截所有的请求，从而判断是否携带的有合法的JWT令牌, 具体见SB过滤器和拦截器
  >

  1. Servlet规范中的Filter过滤器
  2. Spring提供的Interceptor拦截器

‍

‍

### Cookie

‍

客户端会话跟踪技术，存储在客户端浏览器.

使用cookie来跟踪会话可以在浏览器第一次发起请求来请求服务器时在服务器端设置一个cookie

‍

> 服务器端在给客户端在响应数据的时候，会自动的将 cookie 响应给浏览器，浏览器接收到响应回来的 cookie 之后将 cookie 存储在浏览器本地. 接下来在后续的每一次请求当中，都会将浏览器本地所存储的 cookie 自动地携带到服务端. 接下来在服务端我们就可以获取到 cookie 的值. 
>
> 去判断一下这个 cookie 的值是否存在，如果不存在这个cookie，就说明客户端之前是没有访问登录接口的；如果存在 cookie 的值，就说明客户端之前已经登录完成了

‍

#### 实现

cookie 是 HTTP 协议当中所支持的技术，各大浏览器厂商都支持了这一标准. 在 HTTP 协议官方给我们提供了一个响应头和请求头

* 响应头 Set-Cookie ：设置Cookie数据的
* 请求头 Cookie：携带Cookie数据的

‍

**==自动化进行==**

1. 服务器会 **自动** 的将 cookie 响应给浏览器.
2. 浏览器接收到响应回来的数据之后，会 **自动** 的将 cookie 存储在浏览器本地.
3. 在后续的请求当中，浏览器会 **自动** 的将 cookie 携带到服务器端.

‍

‍

#### 示例

设置的cookie存放在应用-存储-Cookie一览, 通过响应头Set-Cookie响应给浏览器，并且浏览器会将Cookie，存储在浏览器端. 获取的时候在网络-请求标头中可以看到

```java
@Slf4j
@RestController
public class SessionController {

    //设置Cookie
    @GetMapping("/c1")
    public Result cookie1(HttpServletResponse response){
        response.addCookie(new Cookie("login_username","itheima")); //设置Cookie/响应Cookie
        return Result.success();
    }

    //获取Cookie
    @GetMapping("/c2")
    public Result cookie2(HttpServletRequest request){
        Cookie[] cookies = request.getCookies();
        for (Cookie cookie : cookies) {
            if(cookie.getName().equals("login_username")){
                System.out.println("login_username: "+cookie.getValue()); //输出name为login_username的cookie
            }
        }
        return Result.success();
    }
}  
```

‍

‍

‍

#### 评价

‍

优点： 是HTTP协议中支持的技术 无需我们手动操作

缺点：

1. 移动端APP(Android、IOS)中无法使用Cookie
2. 不安全，用户可以**自己禁用Cookie**
3. Cookie不能跨域

‍

### Session

服务器端会话跟踪技术，存储在服务器端

底层基于Cookie实现的会话跟踪，如果Cookie不可用，则该方案也就失效了

‍

‍

#### 实现

‍

1. 获取Session

    基于 Session 进行会话跟踪，浏览器在第一次请求服务器的时候直接在服务器当中来获取到会话对象Session. 如果是第一次请求, 对象不存在服务器会自动的创建一个会话对象Session
2. 响应Cookie

    每一个会话对象Session ，它都有一个ID

    接下来，服务器端在给浏览器响应数据的时候会将 Session 的 ID 通过 Cookie 响应给浏览器. 其实在响应头当中增加了一个 Set-Cookie 响应头. 这个 Set-Cookie 响应头对应的值是cookie. cookie 的名字是固定的 JSESSIONID 代表的服务器端会话对象 Session的ID.  浏览器会自动识别这个响应头，然后自动将Cookie存储在浏览器本地.
3. 查找Session

    在后续的每一次请求当中，都会将 Cookie 的数据获取出来，并且携带到服务端. 接下来服务器拿到JSESSIONID这个 Cookie 的值，也就是 Session 的ID, 之后就会从众多的 Session 当中来找到当前请求对应的会话对象Session

‍

‍

#### 示例

```java
    @GetMapping("/s1")
    public Result session1(HttpSession session){
        log.info("HttpSession-s1: {}", session.hashCode());

        session.setAttribute("loginUser", "tom"); //往session中存储数据
        return Result.success();
    }
    // http://localhost:8080/s1

    @GetMapping("/s2")
    public Result session2(HttpServletRequest request) {
        HttpSession session = request.getSession();
        log.info("HttpSession-s2: {}", session.hashCode());

        Object loginUser = session.getAttribute("loginUser"); //从session中获取数据
        log.info("loginUser: {}", loginUser);
        return Result.success(loginUser);
    }
    // http://localhost:8080/s2
```

两次请求，获取到的Session会话对象的hashcode是一样的就说明是同一个会话对象. 而且，第一次请求时，往Session会话对象中存储的值，第二次请求时，也获取到了.

在响应标头可以显示Set-Cookie 内容

‍

‍

#### 评价

‍

优点：Session是存储在服务端的，安全

缺点：

* 服务器集群下无法直接使用Session
* 移动端APP(Android、IOS)中无法使用Cookie
* 用户可以自己禁用Cookie
* Cookie不能跨域

‍

‍

‍

### 令牌

‍

最常用的技术

令牌其实就是一个用户身份的标识，本质就是一个字符串

‍

>     在浏览器请求登录接口的时候，如果登录成功，就生成一个令牌，令牌就是用户的合法身份凭证. 接下来我在响应数据的时候，我就可以直接将令牌响应给前端. 接下来我们在前端程序当中接收到令牌之后，就需要将这个令牌存储起来. 这个存储可以存储在 cookie 当中，也可以存储在其他的存储空间(比如：localStorage)当中.   
>     接下来，在后续的每一次请求当中，都需要将令牌携带到服务端. 携带到服务端之后，接下来我们就需要来校验令牌的有效性. 如果令牌是有效的，就说明用户已经执行了登录操作，如果令牌是无效的，就说明用户之前并未执行登录操作.   
>     此时，如果是在同一次会话的多次请求之间，我们想共享数据，我们就可以将共享的数据存储在令牌当中就可以了.

‍

‍

#### 实现

默认实现方式JWT令牌

‍

‍

#### 评价

‍

* 优点：

  * 支持PC端、移动端
  * 解决集群环境下的认证问题
  * 减轻服务器的存储压力(无需在服务器端存储)
* 缺点：

  * 需要自己实现  
    包括令牌的生成、令牌的传递、令牌的校验

‍

‍

‍

## 过滤器

Filter

‍

### 概念

JavaWeb三大组件(Servlet、Filter、Listener)之一, 过滤器可以把对资源的请求拦截下来，从而实现一些通用的操作，比如：登录校验、统一编码处理、敏感字符处理等. 使用了过滤器之后，要想访问web服务器上的资源，必须先经过过滤器处理完毕之后，才可以访问对应的资源

‍

‍

### 流程

‍

过滤器当中我们拦截到了请求之后，如果希望继续访问后面的web资源，就要执行放行操作

调用 FilterChain对象当中的doFilter()方法，在调用doFilter()这个方法之前所编写的代码属于放行之前的逻辑. 

在放行后, 访问完 web 资源之后还会回到过滤器当中，回到过滤器之后如有需求还可以执行放行之后的逻辑，放行之后的逻辑我们写在doFilter()这行代码之后. 

‍

‍

### 拦截路径

‍

|拦截路径|urlPatterns值|含义|
| --------------| ---------------| ------------------------------------|
|拦截具体路径|/login|只有访问 /login 路径时，才会被拦截|
|目录拦截|/emps/*|访问/emps下的所有资源，都会被拦截|
|拦截所有|/*|访问所有资源，都会被拦截|

‍

‍

### 实例

1. 定义过滤器 ：定义一个类，实现 Filter 接口并重写其所有方法.
2. 配置过滤器： Filter类上加 @WebFilter 注解，配置拦截资源的路径. 引导类上加 @ServletComponentScan 开启Servlet组件支持

‍

‍

### **定义过滤器**

* init方法：过滤器的初始化方法. 在web服务器启动的时候会自动的创建Filter过滤器对象，在创建过滤器对象的时候会自动调用init初始化方法，这个方法只会被调用一次.
* doFilter方法：这个方法是在每一次拦截到请求之后都会被调用，所以这个方法是会被调用多次的，每拦截到一次请求就会调用一次doFilter()方法.
* destroy方法： 是销毁的方法. 当我们关闭服务器的时候，它会自动的调用销毁方法destroy，而这个销毁方法也只会被调用一次.

‍

```java
package cwx.filter;

import jakarta.servlet.*;

import java.io.IOException;

//定义一个类，实现一个标准的Filter过滤器的接口
public class DemoFilter implements Filter {
    @Override //初始化方法, 只调用一次
    public void init(FilterConfig filterConfig) throws ServletException {
        System.out.println("init 初始化方法执行了");
    }

    @Override //拦截到请求之后调用, 调用多次
    public void doFilter(ServletRequest request, ServletResponse response, FilterChain chain) throws IOException, ServletException {
        System.out.println("Demo 拦截到了请求...放行前逻辑");
        //放行
        chain.doFilter(request,response);
    }

    @Override //销毁方法, 只调用一次
    public void destroy() {
        System.out.println("destroy 销毁方法执行了");
    }
}
```

‍

### **配置过滤器**

定义完Filter之后还需完成Filter的配置，只需要在Filter类上添加@WebFilter，并指定属性urlPatterns，通过这个属性指定过滤器要拦截哪些请求;

在过滤器Filter中，如果不执行放行操作，将无法访问后面的资源.  放行操作：chain.doFilter(request, response);

还需要在启动类上面加上一个注解@ServletComponentScan, 开启SpringBoot项目对于Servlet组件的支持

‍

‍

### 过滤器链

指一个web应用程序当中，可以配置多个过滤器，多个过滤器就形成了一个过滤器链. 链上的过滤器在执行的时候会一个一个的执行，会先执行第一个Filter，放行之后再来执行第二个Filter，如果执行到了最后一个过滤器放行之后，才会访问对应的web资源. 

‍

‍

**优先级**

以注解方式配置的Filter过滤器，它的执行优先级是按时过滤器类名的自动排序确定的，类名排名越靠前，优先级越高. 

修改类名即可

‍

‍

### 登录校验

‍

#### 流程

‍

* 要进入到后台管理系统，我们必须先完成登录操作，此时就需要访问登录接口login.
* 登录成功之后，我们会在服务端生成一个JWT令牌，并且把JWT令牌返回给前端，前端会将JWT令牌存储下来.
* 在后续的每一次请求当中，都会将JWT令牌携带到服务端，请求到达服务端之后，要想去访问对应的业务功能，此时我们必须先要校验令牌的有效性.
* 对于校验令牌的这一块操作，我们使用登录校验的过滤器，在过滤器当中来校验令牌的有效性. 如果令牌是无效的，就响应一个错误的信息，也不会再去放行访问对应的资源了. 如果令牌存在，并且它是有效的，此时就会放行去访问对应的web资源，执行相应的业务操作.

‍

#### Tips

1. **登录请求例外,** 拦截到了之后，都需要校验令牌
2. 拦截到请求后，**有令牌，且令牌校验通过(合法)可以放行；否则都返回未登录错误结果**

‍

‍

#### 操作步骤

1. 获取请求url
2. 判断请求url中是否包含login，如果包含，说明是登录操作，放行
3. 获取请求头中的令牌（token）
4. 判断令牌是否存在，如果不存在，返回错误结果（未登录）
5. 解析token，如果解析失败，返回错误结果（未登录）
6. 放行

‍

‍

#### 示例

使用第三方json处理的工具包fastjson

依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>fastjson</artifactId>
    <version>1.2.76</version>
</dependency>
```

‍

**登录校验过滤器LoginCheckFilter**

```java
@Slf4j
@WebFilter(urlPatterns = "/*") //拦截所有请求
public class LoginCheckFilter implements Filter {

    @Override
    public void doFilter(ServletRequest servletRequest, ServletResponse servletResponse, FilterChain chain) throws IOException, ServletException {
        //前置：强制转换为http协议的请求对象、响应对象 （转换原因：要使用子类中特有方法）
        HttpServletRequest request = (HttpServletRequest) servletRequest;
        HttpServletResponse response = (HttpServletResponse) servletResponse;

        //1.获取请求url
        String url = request.getRequestURL().toString();
        log.info("请求路径：{}", url); //请求路径：http://localhost:8080/login


        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行
        if(url.contains("/login")){
            chain.doFilter(request, response);//放行请求
            return;//结束当前方法的执行
        }


        //3.获取请求头中的令牌（token）
        String token = request.getHeader("token");
        log.info("从请求头中获取的令牌：{}",token);


        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasLength(token)){
            log.info("Token不存在");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）
        try {
            JwtUtils.parseJWT(token);
        }catch (Exception e){
            log.info("令牌解析失败!");

            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            //我们之前是通过调用后面的方法转换,但是这里要就地转换,所以我们直接调用转换工具类.
            String json = JSONObject.toJSONString(responseResult);
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return;
        }


        //6.放行
        chain.doFilter(request, response);

    }
}
```

‍

‍

## 拦截器

Interceptor

‍

### 概念

Spring框架中提供的一种动态拦截方法调用的机制，类似于过滤器, 在指定方法调用前后，根据业务需要执行预先设定的代码

统一拦截的技术拦截浏览器发送过来的所有的请求，拦截到这个请求之后，就可以通过请求来获取之前所存入的登录标记，在获取到登录标记且标记为登录成功，就说明员工已经登录了. 如果已经登录，我们就直接放行. 

在拦截器当中做一些通用性的操作，比如：我们可以通过拦截器来拦截前端发起的请求，将登录校验的逻辑全部编写在拦截器当中. 在校验的过程当中，如发现用户登录了(携带JWT令牌且是合法令牌)，就可以直接放行，去访问spring当中的资源. 如果校验时发现并没有登录或是非法令牌，就可以直接给前端响应未登录的错误信息. 

‍

‍

```java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
        //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**")//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
                .excludePathPatterns("/login");//设置不拦截的请求路径
    }
}
```

‍

### 流程

‍

过滤器->前端控制器->拦截器->Controller->return

‍

> * 当我们打开浏览器来访问部署在web服务器当中的web应用时，此时我们所定义的过滤器会拦截到这次请求. 拦截到这次请求之后，它会先执行放行前的逻辑，然后再执行放行操作. 而由于我们当前是基于springboot开发的，所以放行之后是进入到了spring的环境当中，也就是要来访问我们所定义的controller当中的接口方法.
> * Tomcat并不识别所编写的Controller程序，但是它识别Servlet程序，所以在Spring的Web环境中提供了一个非常核心的Servlet：DispatcherServlet（前端控制器），所有请求都会先进行到DispatcherServlet，再将请求转给Controller.
> * 当我们定义了拦截器后，会在执行Controller的方法之前，请求被拦截器拦截住. 执行`preHandle()`​方法，这个方法执行完成后需要返回一个布尔类型的值，如果返回true，就表示放行本次操作，才会继续访问controller中的方法；如果返回false，则不会放行（controller中的方法也不会执行）.
> * 在controller当中的方法执行完毕之后，再回过来执行`postHandle()`​这个方法以及`afterCompletion()`​ 方法，然后再返回给DispatcherServlet，最终再来执行过滤器当中放行后的这一部分逻辑的逻辑. 执行完毕之后，最终给浏览器响应数据.

‍

‍

### 拦截路径

‍

在注册配置拦截器的时候，我们要指定拦截器的拦截路径，通过`addPathPatterns("要拦截路径")`​方法，就可以指定要拦截哪些资源. 

在配置拦截器时，不仅可以指定要拦截哪些资源，还可以指定不拦截哪些资源，只需要调用`excludePathPatterns("不拦截路径")`​方法，指定哪些资源不需要拦截. 

‍

‍

|拦截路径|含义|举例|
| -----------| ----------------------| -----------------------------------------------------|
|/*|一级路径|能匹配/depts，/emps，/login，不能匹配 /depts/1|
|/**|任意级路径|能匹配/depts，/depts/1，/depts/1/2|
|/depts/*|/depts下的一级路径|能匹配/depts/1，不能匹配/depts/1/2，/depts|
|/depts/**|/depts下的任意级路径|能匹配/depts，/depts/1，/depts/1/2，不能匹配/emps/1|

‍

‍

### 实例

‍

#### 配置类

配置类比较固定

‍

实现HandlerInterceptor接口，并重写其所有方法

```java
//自定义拦截器
@Component
public class LoginCheckInterceptor implements HandlerInterceptor {
    //目标资源方法执行前执行。 返回true：放行    返回false：不放行
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
  
        return true; //true表示放行
    }

    //目标资源方法执行后执行
    @Override
    public void postHandle(HttpServletRequest request, HttpServletResponse response, Object handler, ModelAndView modelAndView) throws Exception {
        System.out.println("postHandle ... ");
    }

    //视图渲染完毕后执行，最后执行
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        System.out.println("afterCompletion .... ");
    }
}
```

> 注意：
>
> preHandle方法：目标资源方法执行前执行.  返回true：放行 返回false：不放行
>
> postHandle方法：目标资源方法执行后执行
>
> afterCompletion方法：视图渲染完毕后执行，最后执行

‍

‍

#### **注册配置拦截器**

实现WebMvcConfigurer接口，并重写addInterceptors方法

```java
@Configuration  
public class WebConfig implements WebMvcConfigurer {

    //自定义的拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

  
    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor).addPathPatterns("/**");//设置拦截器拦截的请求路径（ /** 表示拦截所有请求）
    }
}
```

‍

### 登录校验

‍

关闭Filter之后换成拦截器interceptor就可以

‍

#### 示例

‍

**登录校验拦截器**

```java
//自定义拦截器
@Component //当前拦截器对象由Spring创建和管理
@Slf4j
public class LoginCheckInterceptor implements HandlerInterceptor {
    //前置方式
    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        System.out.println("preHandle .... ");
        //1.获取请求url
        //2.判断请求url中是否包含login，如果包含，说明是登录操作，放行

        //3.获取请求头中的令牌（token）
        String token = request.getHeader("token");
        log.info("从请求头中获取的令牌：{}",token);

        //4.判断令牌是否存在，如果不存在，返回错误结果（未登录）
        if(!StringUtils.hasLength(token)){
            log.info("Token不存在");

            //创建响应结果对象
            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            //设置响应头（告知浏览器：响应的数据类型为json、响应的数据编码表为utf-8）
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return false;//不放行
        }

        //5.解析token，如果解析失败，返回错误结果（未登录）
        try {
            JwtUtils.parseJWT(token);
        }catch (Exception e){
            log.info("令牌解析失败!");

            //创建响应结果对象
            Result responseResult = Result.error("NOT_LOGIN");
            //把Result对象转换为JSON格式字符串 (fastjson是阿里巴巴提供的用于实现对象和json的转换工具类)
            String json = JSONObject.toJSONString(responseResult);
            //设置响应头
            response.setContentType("application/json;charset=utf-8");
            //响应
            response.getWriter().write(json);

            return false;
        }

        //6.放行
        return true;
    }
```

‍

**注册配置拦截器**

```java
@Configuration  
public class WebConfig implements WebMvcConfigurer {
    //拦截器对象
    @Autowired
    private LoginCheckInterceptor loginCheckInterceptor;

    @Override
    public void addInterceptors(InterceptorRegistry registry) {
       //注册自定义拦截器对象
        registry.addInterceptor(loginCheckInterceptor)
                .addPathPatterns("/**")
                .excludePathPatterns("/login");
    }
}
```

‍

‍

## 代理

‍

‍

‍

#### 静态代理

* 什么是静态代理

  * 由程序创建或特定工具自动生成源代码，在程序运行前，代理类的.class文件就已经存在
  * 通过将目标类与代理类实现同一个接口，让代理类持有真实类对象，然后在代理类方法中调用真实类方法，在调用真实类方法的前后添加我们所需要的功能扩展代码来达到增强的目的
  * A -> B -> C
* 优点

  * 代理使客户端不需要知道实现类是什么，怎么做的，而客户端只需知道代理即可
  * 方便增加功能，拓展业务逻辑
* 缺点

  * 代理类中出现大量冗余的代码，非常不利于扩展和维护
  * 如果接口增加一个方法，除了所有实现类需要实现这个方法外，所有代理类也需要实现此方法. 增加了代码维护的复杂度

‍

```java
public class StaticProxyPayServiceImpl implements PayService {

    private PayService payService;

    public  StaticProxyPayServiceImpl(PayService payService){
        this.payService = payService;
    }

    public String callback(String outTradeNo) {

        System.out.println("StaticProxyPayServiceImpl callback begin");

        String result = payService.callback(outTradeNo);

        System.out.println("StaticProxyPayServiceImpl callback end");

        return result;
    }

    public int save(int userId, int productId) {

        System.out.println("StaticProxyPayServiceImpl save begin");

        int id = payService.save(userId, productId);

        System.out.println("StaticProxyPayServiceImpl save end");

        return id;
    }
}
```

‍

‍

‍

#### JDK动态代理

**AOP常见的实现的策略JDK动态代理**

‍

* 什么是动态代理

  * 在程序运行时，运用反射机制动态创建而成，无需手动编写代码
  * JDK动态代理与静态代理一样，目标类需要实现一个代理接口,再通过代理对象调用目标方法
* 实操

```java
定义一个java.lang.reflect.InvocationHandler接口的实现类，重写invoke方法

//Object proxy:被代理的对象  
//Method method:要调用的方法  
//Object[] args:方法调用时所需要参数  
public interface InvocationHandler {  
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable;  
}  
```

‍

```java
public class JdkProxy implements InvocationHandler {

    //目标类
    private Object targetObject;

    //获取代理对象
    public Object newProxyInstance(Object targetObject){
        this. targetObject = targetObject;

        //绑定关系，也就是和具体的哪个实现类关联
        return  Proxy.newProxyInstance(targetObject.getClass().getClassLoader(),
                targetObject.getClass().getInterfaces(),this);
    }


    public Object invoke(Object proxy, Method method, Object[] args) {


        Object result = null;

        try{
            System.out.println("通过JDK动态代理调用 "+method.getName() +", 打印日志 begin");

            result = method.invoke(targetObject,args);

            System.out.println("通过JDK动态代理调用 "+method.getName() +", 打印日志 end");

        }catch (Exception e){

            e.printStackTrace();
        }

        return result;


    }
}
```

‍

‍

#### CGLib动态代理

**AOP常见的实现的策略JDK动态代理**

‍

* 什么是动态代理

  * 在程序运行时，运用反射机制动态创建而成，无需手动编写代码
  * CgLib动态代理的原理是对指定的业务类生成一个子类，并覆盖其中的业务方法来实现代理

‍

```java
public class CglibProxy  implements MethodInterceptor {

    //目标类
    private Object targetObject;

    //绑定关系
    public Object newProxyInstance(Object targetObject){
        this.targetObject = targetObject;

        Enhancer enhancer = new Enhancer();
        //设置代理类的父类（目标类）
        enhancer.setSuperclass(this.targetObject.getClass());

        //设置回调函数
        enhancer.setCallback(this);

        //创建子类（代理对象）
        return enhancer.create();
    }

    public Object intercept(Object o, Method method, Object[] args, MethodProxy methodProxy) throws Throwable {

        Object result = null;

        try{

            System.out.println("通过CGLIB动态代理调用 "+method.getName() +", 打印日志 begin");

            result = methodProxy.invokeSuper(o,args);

            System.out.println("通过CGLIB动态代理调用 "+method.getName() +", 打印日志 begin");

        }catch (Exception e){
            e.printStackTrace();
        }


        return result;
    }


}
```

‍

#### 总结

**CGlib和JDk动态代理**

* 动态代理与静态代理相比较，最大的好处是接口中声明的所有方法都被转移到调用处理器一个集中的方法中处理，解耦和易维护
* 两种动态代理的区别：

  * JDK动态代理：要求目标对象*实现一个接口*，但是有时候目标对象只是一个单独的对象,并没有实现任何的接口,这个时候就可以用CGLib动态代理
  * CGLib动态代理,它是在内存中构建一个子类对象从而实现对目标对象功能的扩展
  * JDK动态代理是自带的，CGlib需要引入第三方包
  * CGLib动态代理基于继承来实现代理，所以无法对final类、private方法和static方法实现代理
* Spring AOP中的代理使用的默认策略：

  * 如果目标对象实现了接口，则默认采用JDK动态代理
  * 如果目标对象没有实现接口，则采用CgLib进行动态代理
  * 如果目标对象实现了接扣，程序里面依旧可以指定使用CGlib动态代理

‍

‍

‍

## 整合

‍

### Junit

> Spring需要使用 `@RunWith`​ 注解指定运行器，使用 `@ContextConfiguration`​ 注解来指定配置类或者配置文件.

‍

SB流程

1. 在测试类上添加 `SpringBootTest`​ 注解
2. 使用 `@Autowired`​ 注入要测试的资源
3. 定义测试方法进行测试

‍

‍

### Mybatis

‍

‍

application.yml

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db
    username: root
    password: root
```

‍

‍

‍

#### Druid数据源

‍

依赖

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

‍

​​application.yml​

通过 `spring.datasource.type`​ 来配置使用什么数据源

```yaml
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC
    username: root
    password: 2333
    type: com.alibaba.druid.pool.DruidDataSource
```

‍

‍

### MP

‍

MybatisPlus提供了starter，实现了自动Mybatis以及MybatisPlus的自动装配功能

‍

‍

#### 依赖

mybatis-plus-boot-starter + druid

druid数据源可以加也可以不加，SpringBoot有内置的数据源，可以配置成使用Druid数据源

MP通过依赖传递已经将MyBatis与MyBatis整合Spring的jar包导入，不需要额外在添加MyBatis的相关jar包

```xml
<dependency>
    <groupId>com.baomidou</groupId>
    <artifactId>mybatis-plus-boot-starter</artifactId>
    <version>3.4.1</version>
</dependency>

<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid</artifactId>
    <version>1.1.16</version>
</dependency>
```

‍

‍

‍

#### 配置

resources默认生成的是properties配置文件，可以将其替换成yml文件，并在文件中配置数据库连接的相关信息

​`application.yml`​

```yaml
spring:
  datasource:
    type: com.alibaba.druid.pool.DruidDataSource
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/mybatisplus_db?serverTimezone=UTC 
    username: root
    password: 2333
```

‍

serverTimezone是用来设置时区，UTC是标准时区--东八区的将其修改为`Asia/Shanghai`​

‍

‍

#### 扫描@Mapper

‍

Dao接口要想被容器扫描到

‍

1. 在Dao接口上添加`@Mapper`​注解，并且确保Dao处在引导类所在包或其子包中

    * 缺点是需要在**每一Dao接口中添加注解**
2. 在引导类上添加`@MapperScan`​注解，其属性为所要扫描的Dao所在包

    * 好处是**只需要写一次，则指定包下的所有Dao接口都能被扫描到**，`@Mapper`​就可以不写.

‍

‍

> userDao注入的时候下面有红线提示的原因是什么?
>
> * UserDao是一个接口，不能实例化对象
> * 只有在服务器启动IOC容器初始化后，由框架创建DAO接口的代理对象来注入
> * 现在服务器并未启动，所以代理对象也未创建，IDEA查找不到对应的对象注入，所以提示报红
> * 一旦服务启动，就能注入其代理对象，所以该错误提示不影响正常运行.

‍

#### **继承BaseMapper**

‍

为了简化单表CRUD，MybatisPlus提供了一个基础的`BaseMapper`​接口，其中已经实现了单表的CRUD

‍

修改DAO或者Mapper层代码接口，让其继承`BaseMapper`​

跟之前整合MyBatis相比不需要**在DAO接口中编写方法和SQL语句了**，只需要**继承**​**​`BaseMapper`​**​**接口即可**

‍

‍

#### 测试

‍

需要注入对应的impl类

‍

‍

#### 示例

```java
@SpringBootTest
class MpDemoApplicationTests {

	@Autowired
	private UserDao userDao;
	@Test
	public void testGetAll() {
		List<User> userList = userDao.selectList(null);
		System.out.println(userList);
	}
}
```

```java
    @Test
    void testInsert() {
        User user = new User();
        user.setId(5L);
        user.setUsername("Lucy");
        user.setPassword("123");
        user.setPhone("18688990011");
        user.setBalance(200);
        user.setInfo("{\"age\": 24, \"intro\": \"英文老师\", \"gender\": \"female\"}");
        user.setCreateTime(LocalDateTime.now());
        user.setUpdateTime(LocalDateTime.now());
        userMapper.insert(user);
    }

    @Test
    void testSelectById() {
        User user = userMapper.selectById(5L);
        System.out.println("user = " + user);
    }

    @Test
    void testSelectByIds() {
        List<User> users = userMapper.selectBatchIds(List.of(1L, 2L, 3L, 4L, 5L));
        users.forEach(System.out::println);
    }

    @Test
    void testUpdateById() {
        User user = new User();
        user.setId(5L);
        user.setBalance(20000);
        userMapper.updateById(user);
    }

    @Test
    void testDelete() {
        userMapper.deleteById(5L);
    }
```

‍

‍

‍

‍

‍

# 实操

‍

‍

‍

## Bugfix

‍

### MysqlURL时区配置

‍

​`SpringBoot`​ 版本低于2.4.3(不含)，Mysql驱动版本大于8.0时，需要在url连接串中**配置时区** `jdbc:mysql://localhost:3306/ssm_db?serverTimezone=UTC`​，或在**MySQL数据库端配置时区**解决此问题

‍

‍

‍

### 启动springboot的测试类虚拟机报错

```xml
Java HotSpot(TM) 64-Bit Server VM warning: Sharing is only supported for boot loader classes because bootstrap classpath has been appended
```

‍

原因：

Java HotSpot（TM）64位服务器虚拟机已附加引导程序类路径

‍

解决办法：

IDEA—》Settings—》Build—》Debugger—》Async Stack Traces 异步堆栈跟踪 —》Instrumenting agent 代理

取消Instrumenting agent的勾选即可

‍

您可以忽略这个警告。这只是意味着对于没有由引导类装入器装入的类禁用类数据共享。

右键: 像这样折叠就能屏蔽了
