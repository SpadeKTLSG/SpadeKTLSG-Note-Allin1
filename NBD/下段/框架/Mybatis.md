--Java持久化后端数据库操作框架

JDBC再封装

优秀的**持久层框架**，是Java程序操作数据库的主流方式, 用于简化JDBC的开发[官网](https://mybatis.org/mybatis-3/zh/index.html)

Mybatis框架，就是对原始的JDBC程序的封装

持久层指的是就是DAO数据访问层，直接对接数据库(三层架构: 表现层controller->业务层service->持久层dao)

‍

使用Mybatis操作数据库，就是在Mybatis中编写SQL查询代码，发送给数据库执行，数据库执行后返回结果. Mybatis会把数据库执行的查询结果，使用实体类封装起来（一行记录对应一个实体类对象）,让程序员更关注于SQL语句

‍

‍

### Header

‍

#### 环境

‍

##### 版本

统一管理

‍

##### 命名规范

‍

**==实体类属性采用驼峰命名==**来与方法匹配来实现自动注入功能

‍

SQL驼峰通用命名规则    _删除, 后面字母大写

表中字段名abc_xyz => 类中属性名abcXyz

‍

‍

#### 插件

‍

‍

##### PageHelper分页插件

支持任何形式的单标、多表的分页查询. 

> 天坑: 只对紧跟着的查询结果进行分页！垃圾

依赖

```xml
<dependency>
    <groupId>com.github.pagehelper</groupId>
    <artifactId>pagehelper-spring-boot-starter</artifactId>
    <version>1.4.2</version>
</dependency>
```

EmpMapper

```java
@Mapper
public interface EmpMapper {
    //获取当前页的结果列表
    @Select("select * from emp")
    public List<Emp> page(Integer start, Integer pageSize);
}
```

EmpServiceImpl

```java
@Override
public PageBean page(Integer page, Integer pageSize) {
    // 设置分页参数
    PageHelper.startPage(page, pageSize); 
    // 执行分页查询
    List<Emp> empList = empMapper.list(name,gender,begin,end); 
    // 获取分页结果
    Page<Emp> p = (Page<Emp>) empList;   
    //封装PageBean
    PageBean pageBean = new PageBean(p.getTotal(), p.getResult()); 
    return pageBean;
}
```

‍

‍

‍

# 知识

‍

## 概念

‍

### ORM框架

对数据库的表和POJO(Plain Ordinary Java Object)Java对象的做映射的框架

‍

‍

‍

### 相比JDBC的优化

1. 数据库连接四要素(驱动、链接、用户名、密码)，都配置在springboot默认的配置文件 application.properties中
2. mybatis自动完成查询结果的映射封装
3. 数据库连接池技术，避免频繁的创建连接、销毁连接而带来的资源浪费.

‍

### Mybatis开发方式

注解    简单语句通过@Method_name("SQL") 表示即可

XML    需要实现复杂的SQL功能，建议使用XML来配置映射语句，也就是将SQL语句写在XML配置文件中

‍

[使用注解还是使用XML方式开发](https://mybatis.net.cn/getting-started.html)

‍

### 对象关系映射

Object Relational Mapping，简称ORM

一种用于实现OOP语言里不同类型系统的数据之间的转换的程序技术. 目的是解决面向对象的对象模型和关系型数据库的数据结构之间的不匹配问题, 它通过使用描述对象和数据库之间映射的元数据，将Java程序中的对象自动持久化到关系数据库中. 典型代表有Hibernate，MyBatis等框架

数据库中的一个表与Java中的一个类对应表中的一条记录与Java类的一个对象对应,表中的一个字段（或列)与Java类的一个属性（或字段）对应

‍

‍

## 关注

‍

‍

### application.properties

```properties
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接的url
spring.datasource.url=jdbc:mysql://localhost:3306/mybatis
#连接数据库的用户名
spring.datasource.username=root
#连接数据库的密码
spring.datasource.password=2333
```

‍

### Mapper接口（编写SQL语句）

```java
@Mapper
public interface UserMapper {
    @Select("select id, name, age, gender, phone from user")
    public List<User> list();
}
```

‍

## 与Java类型对应

‍

JDBC Type  <-->  Java Type

```
JDBC Type           Java Type 

CHAR                String 
VARCHAR             String 
LONGVARCHAR         String 
NUMERIC             java.math.BigDecimal 
DECIMAL             java.math.BigDecimal 
BIT                 boolean 
BOOLEAN             boolean 
TINYINT             byte 
SMALLINT            short 
INTEGER             INTEGER 
INTEGER             int
BIGINT              long 
REAL                float 
FLOAT               double 
DOUBLE              double 
BINARY              byte[] 
VARBINARY           byte[] 
LONGVARBINARY       byte[] 
DATE                java.sql.Date 
TIME                java.sql.Time 
TIMESTAMP           java.sql.Timestamp 
CLOB                Clob 
BLOB                Blob 
ARRAY               Array 
DISTINCT            mapping of underlying type 
STRUCT              Struct 
REF                 Ref 
DATALINK            java.net.URL
```

‍

## 评价

半自动化(半ORM框架)，便于写sql，轻量级，在阿里等大厂广泛使用

‍

# 基础

‍

## 搭建

MyBatis可以配置成适应多种环境, 但每个SqlSessionFactory实例只能选择一种环境

Mybatis默认的事务管理器就是JDBC，连接池: POOLED

‍

‍

### 依赖

‍

#### mybatis起步依赖

mybatis-spring-boot-starter

```xml
 <!-- mybatis起步依赖 -->
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.3.0</version>
        </dependency>
```

‍

‍

#### mysql驱动包依赖

mysql-connector-j

```xml
<!-- mysql驱动包依赖 -->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
```

‍

‍

#### Druid连接池

druid-spring-boot-starter

```xml
<dependency>
    <groupId>com.alibaba</groupId>
    <artifactId>druid-spring-boot-starter</artifactId>
    <version>1.2.8</version>
</dependency>
```

‍

‍

### 连接参数

‍

#### properties

```properties
#驱动类名称
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
#数据库连接的url
spring.datasource.url = jdbc:mysql://localhost:3306/mybatis
#连接数据库的用户名
spring.datasource.username=root
#连接数据库的密码
spring.datasource.password=1234
```

‍

#### YML

‍

* 新版druid数据库配置文件

‍

type在datasource层级下

```yml
spring:
  application:
    name: sk_take_out
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://XXXX/XXXX?serverTimezone=Asia/Shanghai&useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull&useSSL=false&allowPublicKeyRetrieval=true
    username: root
    password: 2333
    type: com.alibaba.druid.pool.DruidDataSource
```

‍

### 自动封装

‍

一般只有实体类属性名和数据库表查询返回的字段名一致，mybatis才会自动封装

解决方案最好是开启驼峰命名

‍

#### **手动结果映射**

通过 @Results 及 @Result 进行手动结果映射

```java
@Results({@Result(column = "dept_id", property = "deptId"),
          @Result(column = "create_time", property = "createTime"),
          @Result(column = "update_time", property = "updateTime")})
@Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where id=#{id}")
public Emp getById(Integer id);
```

‍

‍

#### **开启驼峰命名**

(推荐)

‍

前提是 **实体类的属性** 与 **数据库表中的字段名** 严格遵守驼峰命名.

如果字段名与属性名符合**驼峰命名规则**，mybatis会自动通过驼峰命名规则映射

‍

‍

application.properties中

```xml
mybatis.configuration.map-underscore-to-camel-case = true
```

‍

## 流程

‍

* 每个基于 MyBatis 的应用都是以一个 SqlSessionFactory 的实例为核心
* SqlSessionFactory 的实例可以通过 SqlSessionFactoryBuilder 获得
* SqlSessionFactoryBuilder 可以从 XML 配置文件或一个预先配置的 Configuration 实例来构建出 SqlSessionFactory 实例
* 工厂设计模式里面 需要获取SqlSession ，里面提供了在数据库执行 SQL 命令所需的所有方法

‍

## 对象

‍

### 实体类

实体类的属性名与表中的字段名一一对应

使用快速生成代码方法生成实体类的get等东西

‍

### 属性

properties

通过properties属性来实现引用配置文件. 这些属性都是可外部配置且可动态替换的，既可以在典型的Java属性文件中配置，亦可通过propertie元素的子元素来传递. (db.properties)

‍

在核心配置文件中引入外部配置文件, 可以加入一些字段属性, 如果两个文件(内外)有同一字段,优先使用外部配置文件的.

‍

## 参数匹配

条件查询功能中，我们需要保证**接口**中方法的形参名和**SQL语句**中的参数**占位符名**相同

‍

### resultType

* 查询出的字段在相应的pojo中必须有和它相同的字段对应，或者基本数据类型
* 适合简单查询

‍

‍

### **resultMap**

* 需要自定义字段，或者多表查询，一对多等关系，比resultType更强大
* 适合复杂查询

‍

MyBatis 中最重要最强大的元素

设置**结果集映射**来重新指定接口方法和SQL语句的占位符

```xml
<resultMap id="" type="">
    <!--column是数据库字段,property实体类中属性-->
    <result column="id" property="name"/>
<resultMap>
```

‍

设计思想是，对于简单的语句根本不需要配置显式的结果映射，而对于复杂一点的语句只需要描述它们的关系就行了.   
ResultMap最优秀的地方在于，虽然你已经对它相当了解了，但是根本就不需要显式地用到他们

‍

* Mybatis的SQL语句返回结果有两种

```html
<resultMap id="VideoResultMap" type="Video">

        <!--
        id 指定查询列的唯一标示
        column 数据库字段的名称
        property pojo类的名称
        -->
        <id column="id" property="id" jdbcType="INTEGER" />

        <result column="video_tile" property="title"  jdbcType="VARCHAR" />
        <result column="summary" property="summary"  jdbcType="VARCHAR" />
        <result column="cover_img"  property="coverImg"  jdbcType="VARCHAR" />

</resultMap>
<select id="selectBaseFieldByIdWithResultMap" resultMap="VideoResultMap">
select id , title as video_tile, summary, cover_img from video where id = #{video_id}
</select>

```

‍

## 查询结果映射

‍

* association 映射的是一个pojo类，处理一对一的关联关系.
* collection 映射的一个集合列表，处理的是一对多的关联关系.
* 模板

```html
<!-- column不做限制，可以为任意表的字段，而property须为type 定义的pojo属性-->
<resultMap id="唯一的标识" type="映射的pojo对象">
  <id column="表的主键字段,或查询语句中的别名字段" jdbcType="字段类型" property="映射pojo对象的主键属性" />
  <result column="表的一个字段" jdbcType="字段类型" property="映射到pojo对象的一个属性"/>

  <association property="pojo的一个对象属性" javaType="pojo关联的pojo对象">
    <id column="关联pojo对象对应表的主键字段" jdbcType="字段类型" property="关联pojo对象的属性"/>
    <result  column="表的字段" jdbcType="字段类型" property="关联pojo对象的属性"/>
  </association>

  <!-- 集合中的property 需要为oftype定义的pojo对象的属性-->
  <collection property="pojo的集合属性名称" ofType="集合中单个的pojo对象类型">
    <id column="集合中pojo对象对应在表的主键字段" jdbcType="字段类型" property="集合中pojo对象的主键属性" />
    <result column="任意表的字段" jdbcType="字段类型" property="集合中的pojo对象的属性" />  
  </collection>
</resultMap>
```

‍

‍

### association

‍

**Mybatis复杂对象映射配置ResultMap的association**

association: 映射到POJO的某个复杂类型属性，比如订单order对象里面包含 user对象

‍

```html
  <resultMap id="VideoOrderResultMap" type="VideoOrder">
        <id column="id" property="id"/>

        <result column="user_id" property="userId"/>
        <result column="out_trade_no" property="outTradeNo"/>
        <result column="create_time" property="createTime"/>
        <result column="state" property="state"/>
        <result column="total_fee" property="totalFee"/>
        <result column="video_id" property="videoId"/>
        <result column="video_title" property="videoTitle"/>
        <result column="video_img" property="videoImg"/>


        <!--
         association 配置属性一对一
         property 对应videoOrder里面的user属性名
         javaType 这个属性的类型
         -->
        <association property="user" javaType="User">
            <id property="id"  column="user_id"/>
            <result property="name" column="name"/>
            <result property="headImg" column="head_img"/>
            <result property="createTime" column="create_time"/>
            <result property="phone" column="phone"/>
        </association>

    </resultMap>

    <!--一对一管理查询订单， 订单内部包含用户属性-->
    <select id="queryVideoOrderList" resultMap="VideoOrderResultMap">

        select

         o.id id,
         o.user_id ,
         o.out_trade_no,
         o.create_time,
         o.state,
         o.total_fee,
         o.video_id,
         o.video_title,
         o.video_img,
         u.name,
         u.head_img,
         u.create_time,
         u.phone
         from video_order o left join user u on o.user_id = u.id

    </select>
```

```
// resultmap association关联查询
VideoOrderMapper videoOrderMapper = sqlSession.getMapper(VideoOrderMapper.class);
List<VideoOrder> videoOrderList = videoOrderMapper.queryVideoOrderList();

System.out.println(videoOrderList.toString());
```

‍

### **collection**

Mybatis 复杂对象一对多映射配置ResultMap的collection

‍

collection: 一对多查询结果查询映射，比如user有多个订单

```html
<resultMap id="UserOrderResultMap" type="User">

        <id property="id"  column="id"/>
        <result property="name" column="name"/>
        <result property="headImg" column="head_img"/>
        <result property="createTime" column="create_time"/>
        <result property="phone" column="phone"/>

        <!--
        property 填写pojo类中集合类属性的名称
        ofType 集合里面的pojo对象
        -->
        <collection property="videoOrderList" ofType="VideoOrder">

            <!--配置主键，管理order的唯一标识-->
            <id column="order_id" property="id"/>
            <result column="user_id" property="userId"/>
            <result column="out_trade_no" property="outTradeNo"/>
            <result column="create_time" property="createTime"/>
            <result column="state" property="state"/>
            <result column="total_fee" property="totalFee"/>
            <result column="video_id" property="videoId"/>
            <result column="video_title" property="videoTitle"/>
            <result column="video_img" property="videoImg"/>
        </collection>
    </resultMap>

    <select id="queryUserOrder" resultMap="UserOrderResultMap">
        select
        u.id,
        u.name,
        u.head_img,
        u.create_time,
        u.phone,
        o.id order_id,
        o.out_trade_no,
        o.user_id,
        o.create_time,
        o.state,
        o.total_fee,
        o.video_id,
        o.video_title,
        o.video_img
        from user u left join video_order o on u.id = o.user_id

    </select>

```

```
 // resultmap association关联查询
VideoOrderMapper videoOrderMapper = sqlSession.getMapper(VideoOrderMapper.class);

//resultmap collection测试

List<User> userList = videoOrderMapper.queryUserOrder();

System.out.println(userList.toString());
```

‍

‍

## CRUD

‍

‍

### 增

‍

示例

主键返回实现

```java
@Mapper
public interface EmpMapper {
  
    //会自动将生成的主键值，赋值给emp对象的id属性 @Options(useGeneratedKeys,keyProperty)
    @Options(useGeneratedKeys = true, keyProperty = "id")
    @Insert("insert into emp(username, name, gender, image, job, entrydate, dept_id, create_time, update_time) values (#{username}, #{name}, #{gender}, #{image}, #{job}, #{entrydate}, #{deptId}, #{createTime}, #{updateTime})")
    public void insert(Emp emp);

}
```

‍

‍

### 删

‍

示例

如果mapper接口方法形参只有一个普通类型的参数，{id}、#{value}前后不匹配也可(建议保持名字一致)

```java
    //根据ID删除数据
    @Delete("delete from emp where id = #{id}") //#{}表示占位符, 会自动替换成实际的参数值
    public void delete(Integer id); //可以有返回值
```

‍

‍

‍

### 改

‍

示例

```java
@Mapper
public interface EmpMapper {

    @Update("update emp set username=#{username}, name=#{name}, gender=#{gender}, image=#{image}, job=#{job}, entrydate=#{entrydate}, dept_id=#{deptId}, update_time=#{updateTime} where id=#{id}")
    public void update(Emp emp); //根据id修改
}
```

‍

‍

#### 动态更新

‍

使用动态SQL

如果更新时传递有值，则更新; 如果更新时没有传递值，则不更新

‍

update操作的SQL语句编写在Mapper映射文件中

示例

```java
@Mapper
public interface EmpMapper {
    public void update(Emp emp);
}
```

```xml
    <!--更新操作-->
    <update id="update">
        update emp

        <set>
            <if test="username != null">
                username=#{username},
            </if>
            ......
        </set>

        where id=#{id}
    </update>
```

‍

‍

‍

### 查

‍

‍

示例

```java
@Mapper
public interface EmpMapper {
    @Select("select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time from emp where id=#{id}")
    public Emp getById(Integer id);
}
```

‍

‍

#### 条件查询

‍

通配符在前后时**不能用#** 来生成预编译Sql语句

下面还用$拼接是不行的, 必须继续用#{Name}, 就需要concat('%', #{name}, '%')

```java
    @Select("select * from emp " +
            "where name like '%${name}%' " +
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
}
```

‍

使用**字符串拼接函数：concat('%' , '关键字' , '%')**  解决注入问题完成查询

```java
@Mapper
public interface EmpMapper {
    @Select("select * from emp " +
            "where name like concat('%',#{name},'%') " +
            "and gender = #{gender} " +
            "and entrydate between #{begin} and #{end} " +
            "order by update_time desc")
    public List<Emp> list(String name, Short gender, LocalDate begin, LocalDate end);
}

```

‍

‍

#### 动态条件查询

‍

**原有**

```xml
        select * from emp
        where name like concat('%',#{name},'%')
              and gender = #{gender}
              and entrydate between #{begin} and #{end}
        order by update_time desc

```

‍

**动态**

```xml
<select id="list" resultType="com.itheima.pojo.Emp">
        select * from emp
        <where>
             <!-- if做为where标签的子元素 -->
             <if test="name != null">
                 and name like concat('%',#{name},'%')
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
```

‍

‍

‍

‍

## 实用功能

‍

‍

### 片段提取

‍

对重复的SQL语句进行抽取，通过`<sql>`​标签封装到一个SQL片段，再通过`<include>`​标签进行引用. 

* ​`<sql>`​：定义可重用的SQL片段
* ​`<include>`​：通过属性refid，指定包含的SQL片段

‍

‍

示例

```xml
     <sql id="commonSelect">
        select id, username, password, name, gender, image, job, entrydate, dept_id, create_time, update_time
        from emp
     </sql>
```

```xml
    <include refid="commonSelect"/>
```

‍

‍

### 主键返回

‍

在数据添加成功后，需要获取插入数据库数据的主键

‍

在Mapper接口中的方法上添加@Options注解，指定

```xml
useGeneratedKeys=true

keyProperty="实体类属性名"
```

‍

‍

### 类型别名

‍

#### 注解配置

在实体类比较少的时候，可使用类上注解

>  @Alias("name")
>
> <typeAliases>   
> {: id="20231029164030-ucsgmgt" updated="20231029164030"}
>
>  <typealias>
> {: id="20231029164030-1kggode" updated="20231029164030"}
>
>  <package>
> {: id="20231029164030-nzhwcag" updated="20231029164030"}

‍

‍

如果实体类十分多，建议使用指定包名, 简单, 默认就是类名, 首字母小写

‍

‍

#### XML配置

typeAlias

* 类型别名，给类取个别名，可以不用输入类的全限定名

```html
<!--<select id="selectById" parameterType="java.lang.Integer" resultType="net.xdclass.online_class.domain.Video">-->
    <select id="selectById" parameterType="java.lang.Integer" resultType="Video">
        select * from video where id = #{video_id,jdbcType=INTEGER}

    </select>

```

* 如果有很多类，是否需要一个个配置？

  * 不用一个个配置，使用包扫描即可

  ```xml
  <typeAliases>

          <!--<typeAlias type="net.xdclass.online_class.domain.Video" alias="Video"/>-->
          <package name="net.xdclass.online_class.domain"/>

  </typeAliases>
  ```
* 本身就内置很多别名，比如Integer、String、List、Map 等

‍

‍

### Mapper映射器

两个贵物都是要同名同包才可以使用

Mapper映射resource(推荐)

```xml
<mappers>
    <mapper resource = ".xml"/>
<mappers>
```

‍

Mapper映射class

```xml
<mappers>
    <mapper class= ".xml"/>
</mappers>
```

‍

‍

## 配置类型

‍

‍

### 注解配置

使用@Method(SQL语句)

底层就是动态代理

‍

‍

‍

### XML配置

‍

更规范

‍

#### 要求

‍

XML映射文件的名称与**Mapper接口名称**一致  
将XML映射文件和Mapper接口放置在resources文件区的**相同包(相同结构)** 下（同包同名）(当然偷懒就放旁边也可)

‍

XML映射文件的namespace属性为**Mapper接口全限定名**一致 -- **命名空间绑定**  
sql语句的**id**与Mapper接口中的方法名一致  
resultType属性(**返回类型**)一致

‍

<select>标签用于编写select查询语句, resultType属性是查询返回的单条记录所封装的类型

‍

‍

#### **模板**

‍

创建编写XML映射文件

xml映射文件中的DTD约束，直接官网复制

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="XXXXXX.XXXXXX"> 

    XML映射文件的namespace属性为Mapper, sql语句的id与Mapper接口中的方法名一致，并保持返回类型一致(也需要引用路径指定)

    <!--示例, 查询操作-->
    <select id="list" resultType="com.itheima.pojo.Emp">
        select * from emp
        where name like concat('%',#{name},'%')
              and gender = #{gender}
              and entrydate between #{begin} and #{end}
        order by update_time desc
    </select>
</mapper>
```

‍

‍

### 预编译SQL

‍

Mybatis中使用参数占位符可以实现预编译SQL语句, 参数占位符有两种：${..}、#{..}

‍

####  **#{...}  #? YES**

* 执行SQL时，会将#{…}替换为 **?** ，生成预编译SQL，会自动设置参数值, **建议使用**
* 使用时机：参数传递都使用#{…}

‍

> 在复杂场景中要使用MySQL提供的字符串拼接函数：concat('%' , '关键字' , '%') 生成#{}解决SQL注入问题

‍

‍

####  **${...}  Money? NO**

* 拼接SQL, 直接将参数拼接在SQL语句中，**==存在SQL注入问题, 不建议==**
* 使用时机：对表名、列表进行动态设置时使用

‍

‍

‍

‍

## 动态SQL

‍

页面原型中大多数条件是动态的, 传递了参数才应该组装这个查询条件, 不能把SQL语句写死咯

根据不同条件生成不同语句

‍

‍

### 方法列表

‍

#### ​​<if>​​

判断条件是否成立，true则拼接SQL

  `<if test="条件表达式">sql语句</if>`​

‍

#### ​​<where>​​

元素有内容才插入where子句，去除子句的开头的AND或OR

  `<where></where>`​

‍

#### ​​<set>​​

动态地在行首插入set字句，删掉额外的逗号

  `<set></set>`​

‍

#### ​​<foreach>​​

遍历方法中传递的参数集合

  `<foreach 参数列表 ></foreach>`​

‍

‍

### **参数列表**

collection    遍历的集合

item    遍历出来的元素

separator    分隔符

open    遍历开始前拼接的SQL片段

close    遍历结束后拼接的SQL片段

‍

```xml
<foreach collection="集合名称" item="集合遍历出来的元素/项" separator="每一次遍历使用的分隔符" 
         open="遍历开始前拼接的片段" close="遍历结束后拼接的片段">
</foreach>
```

‍

示例

```xml
    <delete id="deleteByIds">
        delete from emp where id in
        <foreach collection="ids" item="id" separator="," open="(" close=")">
            #{id}
        </foreach>
    </delete>
```

‍

## 代码模板

‍

### 一对多

‍

### 多对一

‍

‍

# 高级

‍

## sql片段

‍

根据业务需要，自定制要查询的字段，并可以复用

‍

### 示例

```html
    <sql id="base_video_field">
        id,title,summary,cover_img
    </sql>

     <select id="selectById" parameterType="java.lang.Integer" resultType="Video">

        select <include refid="base_video_field"/>  from video where id = #       {video_id,jdbcType=INTEGER}

    </select>


    <select id="selectListByXML" resultType="Video">

        select <include refid="base_video_field"/>  from video

    </select>
```

‍

## 日志

‍

可以在控制台中查看到sql语句的执行、执行传递的参数以及执行结果

‍

### 配置​

‍

application.properties中指定mybatis输出日志的位置, 输出控制台

```xml
mybatis.configuration.log-impl = org.apache.ibatis.logging.stdout.StdOutImpl
```

‍

‍

## 连接池

‍

‍

## 高级功能

‍

### 万能Map

‍

假设，我们的实体类，或者数据库中的表，字段或者参数过多，获取字段匹配不方便就应当考虑使用Map<String,Object>, 里面为参数和值, 这样直接取Key即可

‍

‍

‍

## 执行流程

‍

‍

‍

‍

## 多级缓存

​​

MyBatis包含一个非常强大的查询缓存特性，它可以非常方便地定制和配置缓存. 缓存可以极大的提升查询效率. 

‍

默认定义了两级缓存:一级缓存和二级缓存

> 默认情况下只有一级缓存开启. (SqlSession级别的缓存，也称为本地缓存)  
> 二级缓存需要手动开启和配置，他是基于namespace级别的缓存.   
> 为了提高扩展性，MyBatis定义了缓存接口Cache. 我们可以通过实现Cache接口来自定义二级缓存

‍

‍

### 一级缓存

默认开启

也叫本地缓存, 就是一个Map  
与数据库同一次会话(SQLSession)期间查询到的数据会放在本地缓存中  
以后如果需要获取相同的数据，直接从缓存中拿，没必须再去查询数据库

> 一级缓存的作用域是SQLSession，同一个SqlSession中执行相同的SQL查询(相同的SQL和参数)，第一次会去查询数据库并写在缓存中，第二次会直接从缓存中取

基于PerpetualCache 的 HashMap本地缓存

‍

缓存失效

情况

* 查询不同的东西
* 增删改操作，可能会改变原来的数据，所以必定会刷新缓存!
* 查询不同的Mapper.xml
* 手动清理缓存 `sqlsession. clearCache()`​

‍

‍

### 二级缓存

‍

也叫全局缓存，一级缓存作用域太低了，所以诞生了二级缓存

基于namespace级别的缓存，一个名称空间，对应一个二级缓存

一级缓存和二级缓存使用顺序

> 优先查询二级缓存->查询一级缓存->数据库

‍

**工作机制**  

一个会话查询一条数据，这个数据就会被放在当前会话的一级缓存中;  

如果当前会话关闭了，这个会话对应的一级缓存就没; 之后**会话关闭了**,一级缓存数据**被保存**到二级缓存中;

新的会话查询信息，就可以从二级缓存中获取内容;这样就能不同会话了

不同的mapper查出的数据会放在自己对应的缓存(map）中

二级缓存是namespace级别的，多个SqlSession去操作同一个namespace下的Mapper的sql语句，多个SqlSession可以共用二级缓存,如果两个mapper的namespace相同，（即使是两个mapper，那么这两个mapper中执行sql查询到的数据也将存在相同的二级缓存区域中，但是最后是每个Mapper单独的名称空间）

‍

**注意**

开启二级缓存,在同一个Mapper下就有效

所有的数据都会先放在一级缓存中;  
只有当会话提交，或者关闭的时候，才会提交到二级缓冲中!

‍

失效策略：执行同个namespace下的mapepr映射文件中增删改sql，并执行了commit操作,会清空该二级缓存

注意：实现二级缓存的时候，MyBatis建议返回的POJO是可序列化的， 也就是建议实现Serializable接口

缓存淘汰策略：会使用默认的 LRU 算法来收回（最近最少使用的）

‍

如何开启某个二级缓存 mapper.xml里面配置

```
<!--开启mapper的namespace下的二级缓存-->
    <!--
        eviction:代表的是缓存回收策略，常见下面两种。
        (1) LRU,最近最少使用的，一处最长时间不用的对象
        (2) FIFO,先进先出，按对象进入缓存的顺序来移除他们

        flushInterval:刷新间隔时间，单位为毫秒，这里配置的是100秒刷新，如果不配置它，当SQL被执行的时候才会去刷新缓存。

        size:引用数目，代表缓存最多可以存储多少个对象，设置过大会导致内存溢出

        readOnly:只读，缓存数据只能读取而不能修改，默认值是false
    -->
<cache eviction="LRU" flushInterval="100000" readOnly="true" size="1024"/>
```

```
全局配置：
<settings>
<!--这个配置使全局的映射器(二级缓存)启用或禁用缓存，全局总开关，这里关闭，mapper中开启了也没用-->
        <setting name="cacheEnabled" value="true" />
</settings>
```

‍

如果需要控制全局mapper里面某个方法不使用缓存，可以配置 useCache="false"

```
<select id="selectById" parameterType="java.lang.Integer" resultType="Video" useCache="false">

select <include refid="base_video_field"/>  from video where id = #{video_id,jdbcType=INTEGER}

</select>
```

‍

## 懒加载

按需加载，先从单表查询，需要时再从关联表去关联查询，能大大提高数据库性能,并不是所有场景下使用懒加载都能提高效率

Mybatis懒加载： resultMap里面的association、collection有延迟加载功能

dubug模式测试懒加载不准确，可以直接run

‍

‍

```xml
<!--全局参数设置-->
<settings>
    <!--延迟加载总开关-->
    <setting name="lazyLoadingEnabled" value="true"/>
    <!--将aggressiveLazyLoading设置为false表示按需加载，默认为true-->
    <setting name="aggressiveLazyLoading" value="false"/>
</settings>
```

```xml
<resultMap id="VideoOrderResultMapLazy" type="VideoOrder">
        <id column="id" property="id"/>

        <result column="user_id" property="userId"/>
        <result column="out_trade_no" property="outTradeNo"/>
        <result column="create_time" property="createTime"/>
        <result column="state" property="state"/>
        <result column="total_fee" property="totalFee"/>
        <result column="video_id" property="videoId"/>
        <result column="video_title" property="videoTitle"/>
        <result column="video_img" property="videoImg"/>

<!-- 
select： 指定延迟加载需要执行的statement id 
column： 和select查询关联的字段
-->
        <association property="user" javaType="User" column="user_id" select="findUserByUserId"/>


</resultMap>

    <!--一对一管理查询订单， 订单内部包含用户属性  懒加载-->
<select id="queryVideoOrderListLazy" resultMap="VideoOrderResultMapLazy">

        select
         o.id id,
         o.user_id ,
         o.out_trade_no,
         o.create_time,
         o.state,
         o.total_fee,
         o.video_id,
         o.video_title,
         o.video_img
         from video_order o

</select>


<select id="findUserByUserId" resultType="User">

       select  * from user where id=#{id}

</select>
```

‍

## 事务

‍

‍

* 使用**JDBC**的事务管理

  * 使用 java.sql.Connection对象完成对事务的提交（commit()）、回滚（rollback()）、关闭（close()）
* 使用**MANAGED**的事务管理

  * MyBatis自身不会去实现事务管理，而让程序的容器如（Spring, JBOSS）来实现对事务的管理

‍

```xml
<environment id="development">
            <!-- mybatis使用jdbc事务管理方式 -->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.cj.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/xdclass?useUnicode=true&amp;characterEncoding=utf-8&amp;useSSL=false"/>
                <property name="username" value="root"/>
                <property name="password" value="xdclass.net"/>
            </dataSource>
</environment>
```

* 事务工厂TransactionFactory 的两个实现类

  * JdbcTransactionFactory->JdbcTransaction
  * ManagedTransactionFactory->ManagedTransaction
* 注意：如果不是web程序，然后使用的事务管理形式是MANAGED, 那么将没有事务管理功能

‍

## 核心配置

‍

核心配置文件包含了 MyBatis 最核心的设置和属性信息，如数据库的连接、事务、连接池信息等

命名：MyBatisConfig.xml

* 核心配置文件的文件头：

  ```xml-dtd
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
  ```
* 根标签：

  * ：核心根标签
* 引入连接配置文件：

  * ： 引入数据库连接配置文件标签

    * resource：属性，指定配置文件名

    ```xml-dtd
    <properties resource="jdbc.properties"/>
    ```
* 调整设置

  * ：可以改变 Mybatis 运行时行为
* 起别名：

  * ：为全类名起别名的父标签

    * ：为全类名起别名的子标签

      * type：指定全类名
      * alias：指定别名
    * ：为指定包下所有类起别名的子标签，别名就是类名，首字母小写

    ```xml-dtd
    <!--起别名-->
    <typeAliases>
    	<typeAlias type="bean.Student" alias="student"/>
    	<package name="com.seazean.bean"/>
    		<!--二选一-->
    </typeAliase>
    ```
  * |自带别名：||
    |别名|数据类型|
    | ------------| -------------------|
    |string|java.lang.String|
    |long|java.lang.Lang|
    |int|java.lang.Integer|
    |double|java.lang.Double|
    |boolean|java.lang.Boolean|
    |....|......|
* 配置环境，可以配置多个标签

  * ：配置数据库环境标签，default 属性指定哪个 environment
  * ：配置数据库环境子标签，id 属性是唯一标识，与 default 对应
  * ：事务管理标签，type 属性默认 JDBC 事务
  * ：数据源标签* type 属性：POOLED 使用连接池（MyBatis 内置），UNPOOLED 不使用连接池
  * ：数据库连接信息标签。* name 属性取值：driver，url，username，password

    * value 属性取值：与 name 对应
* 引入映射配置文件

  * ：引入映射配置文件标签
  * ：引入映射配置文件子标签* resource：属性指定映射配置文件的名称

    * url：引用网路路径或者磁盘路径下的 sql 映射文件
    * class：指定映射配置类
  * ：批量注册

参考官方文档：[https://mybatis.org/mybatis-3/zh/configuration.html](https://mybatis.org/mybatis-3/zh/configuration.html)

‍

‍

## 结果映射

‍

### 相关标签

：返回结果映射对象类型，和对应方法的返回值类型保持一致，但是如果返回值是 List 则和其泛型保持一致

：返回一条记录的 Map，key 是列名，value 是对应的值，用来配置**字段和对象属性**的映射关系标签，结果映射（和 resultType 二选一）

* id 属性：唯一标识
* type 属性：实体对象类型
* autoMapping 属性：结果自动映射

内的核心配置文件标签：

* ：配置主键映射关系标签
* ：配置非主键映射关系标签

  * column 属性：表中字段名称
  * property 属性： 实体对象变量名称
* ：配置被包含**单个对象**的映射关系标签，嵌套封装结果集（多对一、一对一）

  * property 属性：被包含对象的变量名，要进行映射的属性名
  * javaType 属性：被包含对象的数据类型，要进行映射的属性的类型（Java 中的 Bean 类）
  * select 属性：加载复杂类型属性的映射语句的 ID，会从 column 属性指定的列中检索数据，作为参数传递给目标 select 语句
* ：配置被包含**集合对象**的映射关系标签，嵌套封装结果集（一对多、多对多）

  * property 属性：被包含集合对象的变量名
  * ofType 属性：集合中保存的对象数据类型
* ：鉴别器，用来判断某列的值，根据得到某列的不同值做出不同自定义的封装行为

自定义封装规则可以将数据库中比较复杂的数据类型映射为 JavaBean 中的属性

‍

### 嵌套查询

子查询：

```java
public class Blog {
    private int id;
    private String msg;
    private Author author;
    // set + get
}
```

```xml
<resultMap id="blogResult" type="Blog" autoMapping = "true">
    <association property="author" column="author_id" javaType="Author" select="selectAuthor"/>
</resultMap>

<select id="selectBlog" resultMap="blogResult">
    SELECT * FROM BLOG WHERE ID = #{id}
</select>

<select id="selectAuthor" resultType="Author">
    SELECT * FROM AUTHOR WHERE ID = #{id}
</select>
```

循环引用：通过缓存解决

```xml
<resultMap id="blogResult" type="Blog" autoMapping = "true">
    <id column="id" property="id"/>
    <collection property="comment" ofType="Comment">
        <association property="blog" javaType="Blog" resultMap="blogResult"/><!--y-->
    </collection>
</resultMap
```

‍

### 多表查询

#### 一对一

一对一实现：

* 数据准备

  ```mysql
  CREATE TABLE person(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	name VARCHAR(20),
  	age INT
  );
  INSERT INTO person VALUES (NULL,'张三',23),(NULL,'李四',24),(NULL,'王五',25);

  CREATE TABLE card(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	number VARCHAR(30),
  	pid INT,
  	CONSTRAINT cp_fk FOREIGN KEY (pid) REFERENCES person(id)
  );
  INSERT INTO card VALUES (NULL,'12345',1),(NULL,'23456',2),(NULL,'34567',3);
  ```
* bean 类

  ```java
  public class Card {
      private Integer id;     //主键id
      private String number;  //身份证号
      private Person p;       //所属人的对象
      ......
  }

  public class Person {
      private Integer id;     //主键id
      private String name;    //人的姓名
      private Integer age;    //人的年龄
  }
  ```
* 配置文件 OneToOneMapper.xml，MyBatisConfig.xml 需要引入（可以把 bean 包下起别名）

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

  <mapper namespace="OneToOneMapper">

      <!--配置字段和实体对象属性的映射关系-->
      <resultMap id="oneToOne" type="card">
         	<!--column 表中字段名称，property 实体对象变量名称-->
          <id column="cid" property="id" />
          <result column="number" property="number" />
          <!--
              association：配置被包含对象的映射关系
              property：被包含对象的变量名
              javaType：被包含对象的数据类型
          -->
          <association property="p" javaType="bean.Person">
              <id column="pid" property="id" />
              <result column="name" property="name" />
              <result column="age" property="age" />
          </association>
      </resultMap>

      <select id="selectAll" resultMap="oneToOne"> <!--SQL-->
          SELECT c.id cid,number,pid,NAME,age FROM card c,person p WHERE c.pid=p.id
      </select>
  </mapper>
  ```
* 核心配置文件 MyBatisConfig.xml

  ```xml
  <!-- mappers引入映射配置文件 -->
  <mappers>
      <mapper resource="one_to_one/OneToOneMapper.xml"/>
      <mapper resource="one_to_many/OneToManyMapper.xml"/>
      <mapper resource="many_to_many/ManyToManyMapper.xml"/>
  </mappers>
  ```
* 测试类

  ```java
  public class Test01 {
      @Test
      public void selectAll() throws Exception{
          //1.加载核心配置文件
          InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

          //2.获取SqlSession工厂对象
          SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(is);

          //3.通过工厂对象获取SqlSession对象
          SqlSession sqlSession = ssf.openSession(true);

          //4.获取OneToOneMapper接口的实现类对象
          OneToOneMapper mapper = sqlSession.getMapper(OneToOneMapper.class);

          //5.调用实现类的方法，接收结果
          List<Card> list = mapper.selectAll();

          //6.处理结果
          for (Card c : list) {
              System.out.println(c);
          }

          //7.释放资源
          sqlSession.close();
          is.close();
      }
  }
  ```

---

#### 一对多

一对多实现：

* 数据准备

  ```mysql
  CREATE TABLE classes(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	name VARCHAR(20)
  );
  INSERT INTO classes VALUES (NULL,'程序一班'),(NULL,'程序二班')

  CREATE TABLE student(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	name VARCHAR(30),
  	age INT,
  	cid INT,
  	CONSTRAINT cs_fk FOREIGN KEY (cid) REFERENCES classes(id)
  );
  INSERT INTO student VALUES (NULL,'张三',23,1),(NULL,'李四',24,1),(NULL,'王五',25,2);
  ```
* bean 类

  ```java
  public class Classes {
      private Integer id;     //主键id
      private String name;    //班级名称
      private List<Student> students; //班级中所有学生对象
      ........
  }
  public class Student {
      private Integer id;     //主键id
      private String name;    //学生姓名
      private Integer age;    //学生年龄
  }
  ```
* 映射配置文件

  ```xml
  <mapper namespace="OneToManyMapper">
      <resultMap id="oneToMany" type="bean.Classes">
          <id column="cid" property="id"/>
          <result column="cname" property="name"/>

          <!--collection：配置被包含的集合对象映射关系-->
          <collection property="students" ofType="bean.Student">
              <id column="sid" property="id"/>
              <result column="sname" property="name"/>
              <result column="sage" property="age"/>
          </collection>
      </resultMap>
      <select id="selectAll" resultMap="oneToMany"> <!--SQL-->
          SELECT c.id cid,c.name cname,s.id sid,s.name sname,s.age sage FROM classes c,student s WHERE c.id=s.cid
      </select>
  </mapper>
  ```
* 代码实现片段

  ```java
  //4.获取OneToManyMapper接口的实现类对象
  OneToManyMapper mapper = sqlSession.getMapper(OneToManyMapper.class);

  //5.调用实现类的方法，接收结果
  List<Classes> classes = mapper.selectAll();

  //6.处理结果
  for (Classes cls : classes) {
      System.out.println(cls.getId() + "," + cls.getName());
      List<Student> students = cls.getStudents();
      for (Student student : students) {
          System.out.println("\t" + student);
      }
  }
  ```

---

#### 多对多

学生课程例子，中间表不需要 bean 实体类

* 数据准备

  ```mysql
  CREATE TABLE course(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	name VARCHAR(20)
  );
  INSERT INTO course VALUES (NULL,'语文'),(NULL,'数学');

  CREATE TABLE stu_cr(
  	id INT PRIMARY KEY AUTO_INCREMENT,
  	sid INT,
  	cid INT,
  	CONSTRAINT sc_fk1 FOREIGN KEY (sid) REFERENCES student(id),
  	CONSTRAINT sc_fk2 FOREIGN KEY (cid) REFERENCES course(id)
  );
  INSERT INTO stu_cr VALUES (NULL,1,1),(NULL,1,2),(NULL,2,1),(NULL,2,2);
  ```
* bean类

  ```java
  public class Student {
      private Integer id;     //主键id
      private String name;    //学生姓名
      private Integer age;    //学生年龄
      private List<Course> courses;   // 学生所选择的课程集合
  }
  public class Course {
      private Integer id;     //主键id
      private String name;    //课程名称
  }
  ```
* 配置文件

  ```xml
  <mapper namespace="ManyToManyMapper">
      <resultMap id="manyToMany" type="Bean.Student">
          <id column="sid" property="id"/>
          <result column="sname" property="name"/>
          <result column="sage" property="age"/>

          <collection property="courses" ofType="Bean.Course">
              <id column="cid" property="id"/>
              <result column="cname" property="name"/>
          </collection>
      </resultMap>
      <select id="selectAll" resultMap="manyToMany"> <!--SQL-->
          SELECT sc.sid,s.name sname,s.age sage,sc.cid,c.name cname FROM student s,course c,stu_cr sc WHERE sc.sid=s.id AND sc.cid=c.id
      </select>
  </mapper>
  ```

‍

### 鉴别器

需求：如果查询结果是女性，则把部门信息查询出来，否则不查询 ；如果是男性，把 last_name 这一列的值赋值

```xml
<!-- 
    column：指定要判断的列名 
    javaType：列值对应的java类型
   -->
<discriminator javaType="string" column="gender">
    <!-- 女生 -->
    <!-- resultType不可缺少，也可以使用resutlMap -->
    <case value="0" resultType="com.bean.Employee">
        <association property="dept"
                     select="com.dao.DepartmentMapper.getDeptById"
                     column="d_id">
        </association>
    </case>
    <!-- 男生 -->
    <case value="1" resultType="com.bean.Employee">
        <id column="id" property="id"/>
        <result column="last_name" property="lastName"/>
        <result column="gender" property="gender"/>
    </case>
</discriminator>
```

‍

### 延迟加载

#### 两种加载

立即加载：只要调用方法，马上发起查询

延迟加载：在需要用到数据时才进行加载，不需要用到数据时就不加载数据，延迟加载也称懒加载

优点： 先从单表查询，需要时再从关联表去关联查询，提高数据库性能，因为查询单表要比关联查询多张表速度要快，节省资源

坏处：只有当需要用到数据时，才会进行数据库查询，这样在大批量数据查询时，查询工作也要消耗时间，所以可能造成用户等待时间变长，造成用户体验下降

核心配置文件：

|标签名|描述|默认值|
| -----------------------| -----------------------------------------------------------------------------------------------------------------------| --------|
|lazyLoadingEnabled|延迟加载的全局开关。当开启时，所有关联对象都会延迟加载，特定关联关系中可通过设置 `fetchType`​ 属性来覆盖该项的开关状态。|false|
|aggressiveLazyLoading|开启时，任一方法的调用都会加载该对象的所有延迟加载属性。否则每个延迟加载属性会按需加载（参考 lazyLoadTriggerMethods）|false|

```xml
<settings> 
	<setting name="lazyLoadingEnabled" value="true"/> 
    <setting name="aggressiveLazyLoading" value="false"/> 
</settings>
```

---

#### assocation

分布查询：先按照身份 id 查询所属人的 id、然后根据所属人的 id 去查询人的全部信息，这就是分步查询

* 映射配置文件 OneToOneMapper.xml  
  一对一映射：

  * column 属性表示给要调用的其它的 select 标签传入的参数
  * select 属性表示调用其它的 select 标签
  * fetchType="lazy" 表示延迟加载（局部配置，只有配置了这个的地方才会延迟加载）

  ```xml
  <mapper namespace="OneToOneMapper">
      <!--配置字段和实体对象属性的映射关系-->
      <resultMap id="oneToOne" type="card">
          <id column="id" property="id" />
          <result column="number" property="number" />
          <association property="p" javaType="bean.Person"
                       column="pid" 
                       select="one_to_one.PersonMapper.findPersonByid"
                       fetchType="lazy">
              		<!--需要配置新的映射文件-->
          </association>
      </resultMap>

      <select id="selectAll" resultMap="oneToOne"> 
          SELECT * FROM card <!--查询全部，负责根据条件直接全部加载-->
      </select>
  </mapper>
  ```
* PersonMapper.xml

  ```xml
  <mapper namespace="one_to_one.PersonMapper">
      <select id="findPersonByid" parameterType="int" resultType="person">
          SELECT * FROM person WHERE id=#{pid}
      </select>
  </mapper>
  ```
* PersonMapper.java

  ```java
  public interface PersonMapper {
      User findPersonByid(int id);
  }
  ```
* 测试文件

  ```java
  public class Test01 {
      @Test
      public void selectAll() throws Exception{
          InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");
          SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(is);
          SqlSession sqlSession = ssf.openSession(true);
          OneToOneMapper mapper = sqlSession.getMapper(OneToOneMapper.class);
          // 调用实现类的方法，接收结果
          List<Card> list = mapper.selectAll();

        	// 不能遍历，遍历就是相当于使用了该数据，需要加载，不遍历就是没有使用。

          // 释放资源
          sqlSession.close();
          is.close();
      }
  }
  ```

---

#### collection

同样在一对多关系配置的 结点中配置延迟加载策略， 结点中也有 select 属性和 column 属性

* 映射配置文件 OneToManyMapper.xml  
  一对多映射：

  * column 是用于指定使用哪个字段的值作为条件查询
  * select 是用于指定查询账户的唯一标识（账户的 dao 全限定类名加上方法名称）

  ```xml
  <mapper namespace="OneToManyMapper">
      <resultMap id="oneToMany" type="bean.Classes">
          <id column="id" property="id"/>
          <result column="name" property="name"/>

          <!--collection：配置被包含的集合对象映射关系-->
          <collection property="students" ofType="bean.Student"
                      column="id" 
                      select="one_to_one.StudentMapper.findStudentByCid">
          </collection>
      </resultMap>
      <select id="selectAll" resultMap="oneToMany">
        SELECT * FROM classes
      </select>
  </mapper>
  ```
* StudentMapper.xml

  ```xml
  <mapper namespace="one_to_one.StudentMapper">
      <select id="findPersonByCid" parameterType="int" resultType="student">
          SELECT * FROM person WHERE cid=#{id}
      </select>
  </mapper>
  ```

‍

‍

## 注解开发

### 单表操作

注解可以简化开发操作，省略映射配置文件的编写

常用注解：

* @Select(“查询的 SQL 语句”)：执行查询操作注解
* @Insert(“插入的 SQL 语句”)：执行新增操作注解
* @Update(“修改的 SQL 语句”)：执行修改操作注解
* @Delete(“删除的 SQL 语句”)：执行删除操作注解

参数注解：

* @Param：当 SQL 语句需要**多个（大于1）参数**时，用来指定参数的对应规则

核心配置文件配置映射关系：

```xml
<mappers>
	<package name="使用了注解的Mapper接口所在包"/>
</mappers>
<!--或者-->
<mappers>
 	<mapper class="包名.Mapper名"></mapper>
</mappers>
```

基本增删改查：

* 创建 Mapper 接口

  ```java
  package mapper;
  public interface StudentMapper {
      //查询全部
      @Select("SELECT * FROM student")
      public abstract List<Student> selectAll();

      //新增数据
      @Insert("INSERT INTO student VALUES (#{id},#{name},#{age})")
      public abstract Integer insert(Student student);

      //修改操作
      @Update("UPDATE student SET name=#{name},age=#{age} WHERE id=#{id}")
      public abstract Integer update(Student student);

      //删除操作
      @Delete("DELETE FROM student WHERE id=#{id}")
      public abstract Integer delete(Integer id);

  }
  ```
* 修改 MyBatis 的核心配置文件

  ```xml
  <mappers>
  	<package name="mapper"/>
  </mappers>
  ```
* bean类

  ```java
  public class Student {
      private Integer id;
      private String name;
      private Integer age;
  }
  ```
* 测试类

  ```java
  @Test
  public void selectAll() throws Exception{
      //1.加载核心配置文件
      InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

      //2.获取SqlSession工厂对象
      SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(is);

      //3.通过工厂对象获取SqlSession对象
      SqlSession sqlSession = ssf.openSession(true);

      //4.获取StudentMapper接口的实现类对象
      StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

      //5.调用实现类对象中的方法，接收结果
      List<Student> list = mapper.selectAll();

      //6.处理结果
      for (Student student : list) {
          System.out.println(student);
      }

      //7.释放资源
      sqlSession.close();
      is.close();
  }
  ```

---

### 多表操作

#### 相关注解

实现复杂关系映射之前我们可以在映射文件中通过配置 来实现，使用注解开发后，可以使用 @Results 注解，@Result 注解，@One 注解，@Many 注解组合完成复杂关系的配置

|注解|说明|
| ---------------| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|@Results|代替 标签，注解中使用单个 @Result 注解或者 @Result 集合<br />使用格式：@Results({ @Result(), @Result() })或@Results({ @Result() })|
|@Result|代替< id> 和 标签，@Result 中属性介绍：<br />column：数据库的列名 property：封装类的变量名<br />one：需要使用 @One 注解（@Result(one = @One)）<br />Many：需要使用 @Many 注解（@Result(many= @Many)）|
|@One(一对一)|代替 标签，多表查询的关键，用来指定子查询返回单一对象<br />select：指定调用 Mapper 接口中的某个方法<br />使用格式：@Result(column="", property="", one=@One(select=""))|
|@Many(多对一)|代替 标签，多表查询的关键，用来指定子查询返回对象集合<br />select：指定调用 Mapper 接口中的某个方法<br />使用格式：@Result(column="", property="", many=@Many(select=""))|

---

#### 一对一

身份证对人

* PersonMapper 接口

  ```java
  public interface PersonMapper {
      //根据id查询
      @Select("SELECT * FROM person WHERE id=#{id}")
      public abstract Person selectById(Integer id);
  }
  ```
* CardMapper接口

  ```java
  public interface CardMapper {
      //查询全部
      @Select("SELECT * FROM card")
      @Results({
              @Result(column = "id",property = "id"),
              @Result(column = "number",property = "number"),
              @Result(
                      property = "p",             // 被包含对象的变量名
                      javaType = Person.class,    // 被包含对象的实际数据类型
                      column = "pid",  // 根据查询出的card表中的pid字段来查询person表
                       /* 
                       	one、@One 一对一固定写法
                          select属性：指定调用哪个接口中的哪个方法
                       */
                      one = @One(select = "one_to_one.PersonMapper.selectById")
              )
      })
      public abstract List<Card> selectAll();
  }
  ```
* 测试类（详细代码参考单表操作）

  ```java
  //1.加载核心配置文件
  //2.获取SqlSession工厂对象
  //3.通过工厂对象获取SqlSession对象

  //4.获取StudentMapper接口的实现类对象
  CardMapper mapper = sqlSession.getMapper(CardMapper.class);
  //5.调用实现类对象中的方法，接收结果
  List<Card> list = mapper.selectAll();
  ```

---

#### 一对多

班级和学生

* StudentMapper接口

  ```java
  public interface StudentMapper {
      //根据cid查询student表  cid是外键约束列
      @Select("SELECT * FROM student WHERE cid=#{cid}")
      public abstract List<Student> selectByCid(Integer cid);
  }
  ```
* ClassesMapper接口

  ```java
  public interface ClassesMapper {
      //查询全部
      @Select("SELECT * FROM classes")
      @Results({
              @Result(column = "id", property = "id"),
              @Result(column = "name", property = "name"),
              @Result(
                      property = "students",  //被包含对象的变量名
                      javaType = List.class,  //被包含对象的实际数据类型
                      column = "id",          //根据id字段查询student表
                      many = @Many(select = "one_to_many.StudentMapper.selectByCid")
              )
      })
      public abstract List<Classes> selectAll();
  }
  ```
* 测试类

  ```java
  //4.获取StudentMapper接口的实现类对象
  ClassesMapper mapper = sqlSession.getMapper(ClassesMapper.class);
  //5.调用实现类对象中的方法，接收结果
  List<Classes> classes = mapper.selectAll();
  ```

---

#### 多对多

学生和课程

* SQL 查询语句

  ```mysql
  SELECT DISTINCT s.id,s.name,s.age FROM student s,stu_cr sc WHERE sc.sid=s.id
  SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}
  ```
* CourseMapper 接口

  ```java
  public interface CourseMapper {
      //根据学生id查询所选课程
      @Select("SELECT c.id,c.name FROM stu_cr sc,course c WHERE sc.cid=c.id AND sc.sid=#{id}")
      public abstract List<Course> selectBySid(Integer id);
  }
  ```
* StudentMapper 接口

  ```java
  public interface StudentMapper {
      //查询全部
      @Select("SELECT DISTINCT s.id,s.name,s.age FROM student s,stu_cr sc WHERE sc.sid=s.id")
      @Results({
              @Result(column = "id",property = "id"),
              @Result(column = "name",property = "name"),
              @Result(column = "age",property = "age"),
              @Result(
                      property = "courses",    //被包含对象的变量名
                      javaType = List.class,  //被包含对象的实际数据类型
                      column = "id", //根据查询出的student表中的id字段查询中间表和课程表
                      many = @Many(select = "many_to_many.CourseMapper.selectBySid")
              )
      })
      public abstract List<Student> selectAll();
  }

  ```
* 测试类

  ```java
  //4.获取StudentMapper接口的实现类对象
  StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);
  //5.调用实现类对象中的方法，接收结果
  List<Student> students = mapper.selectAll();
  ```

‍

‍

## 缓存机制

‍

### 缓存概述

缓存：缓存就是一块内存空间，保存临时数据

作用：将数据源（数据库或者文件）中的数据读取出来存放到缓存中，再次获取时直接从缓存中获取，可以减少和数据库交互的次数，提升程序的性能

缓存适用：

* 适用于缓存的：经常查询但不经常修改的，数据的正确与否对最终结果影响不大的
* 不适用缓存的：经常改变的数据 , 敏感数据（例如：股市的牌价，银行的汇率，银行卡里面的钱）等等

缓存类别：

* 一级缓存：SqlSession 级别的缓存，又叫本地会话缓存，自带的（不需要配置），一级缓存的生命周期与 SqlSession 一致。在操作数据库时需要构造 SqlSession 对象，**在对象中有一个数据结构（HashMap）用于存储缓存数据**，不同的 SqlSession 之间的缓存数据区域是互相不影响的
* 二级缓存：mapper（namespace）级别的缓存，二级缓存的使用，需要手动开启（需要配置）。多个 SqlSession 去操作同一个 Mapper 的 SQL 可以共用二级缓存，二级缓存是跨 SqlSession 的

开启缓存：配置核心配置文件中 标签

* cacheEnabled：true 表示全局性地开启所有映射器配置文件中已配置的任何缓存，默认 true

‍

### 一级缓存

一级缓存是 SqlSession 级别的缓存

工作流程：第一次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，如果没有，从数据库查询用户信息，得到用户信息，将用户信息存储到一级缓存中；第二次发起查询用户 id 为 1 的用户信息，先去找缓存中是否有 id 为 1 的用户信息，缓存中有，直接从缓存中获取用户信息。

一级缓存的失效：

* SqlSession 不同
* SqlSession 相同，查询条件不同时（还未缓存该数据）
* SqlSession 相同，手动清除了一级缓存，调用 `sqlSession.clearCache()`​
* SqlSession 相同，执行 commit 操作或者执行插入、更新、删除，清空 SqlSession 中的一级缓存，这样做的目的为了让缓存中存储的是最新的信息，**避免脏读**

Spring 整合 MyBatis 后，一级缓存作用：

* 未开启事务的情况，每次查询 Spring 都会创建新的 SqlSession，因此一级缓存失效
* 开启事务的情况，Spring 使用 ThreadLocal 获取当前资源绑定同一个 SqlSession，因此此时一级缓存是有效的

‍

### 二级缓存

#### 基本介绍

二级缓存是 mapper 的缓存，只要是同一个命名空间（namespace）的 SqlSession 就共享二级缓存的内容，并且可以操作二级缓存

作用：作用范围是整个应用，可以跨线程使用，适合缓存一些修改较少的数据

工作流程：一个会话查询数据，这个数据就会被放在当前会话的一级缓存中，如果**会话关闭或提交**一级缓存中的数据会保存到二级缓存

二级缓存的基本使用：

1. 在 MyBatisConfig.xml 文件开启二级缓存，**cacheEnabled 默认值为 true**，所以这一步可以省略不配置

    ```xml
    <!--配置开启二级缓存-->
    <settings>
        <setting name="cacheEnabled" value="true"/>
    </settings>
    ```
2. 配置 Mapper 映射文件  
    ​`<cache>`​ 标签表示当前这个 mapper 映射将使用二级缓存，区分的标准就看 mapper 的 namespace 值

    ```xml
    <mapper namespace="dao.UserDao">
        <!--开启user支持二级缓存-->
       	<cache eviction="FIFO" flushInterval="6000" readOnly="" size="1024"/>
    	<cache></cache> <!--则表示所有属性使用默认值-->
    </mapper>
    ```

    eviction（清除策略）：

    * ​`LRU`​ – 最近最少使用：移除最长时间不被使用的对象，默认
    * ​`FIFO`​ – 先进先出：按对象进入缓存的顺序来移除它们
    * ​`SOFT`​ – 软引用：基于垃圾回收器状态和软引用规则移除对象
    * ​`WEAK`​ – 弱引用：更积极地基于垃圾收集器状态和弱引用规则移除对象

    flushInterval（刷新间隔）：可以设置为任意的正整数， 默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新  
    size（引用数目）：缓存存放多少元素，默认值是 1024  
    readOnly（只读）：可以被设置为 true 或 false

    * 只读的缓存会给所有调用者返回缓存对象的相同实例，因此这些对象不能被修改，促进了性能提升
    * 可读写的缓存会（通过序列化）返回缓存对象的拷贝， 速度上会慢一些，但是更安全，因此默认值是 false

    type：指定自定义缓存的全类名，实现 Cache 接口即可
3. 要进行二级缓存的类必须实现 java.io.Serializable 接口，可以使用序列化方式来保存对象。

    ```java
    public class User implements Serializable{}
    ```

---

#### 相关属性

1. select 标签的 useCache 属性  
    映射文件中的 `<select>`​ 标签中设置 `useCache="true"`​ 代表当前 statement 要使用二级缓存（默认）  
    注意：如果每次查询都需要最新的数据 sql，要设置成 useCache=false，禁用二级缓存

    ```xml
    <select id="findAll" resultType="user" useCache="true">
        select * from user
    </select>
    ```
2. 每个增删改标签都有 flushCache 属性，默认为 true，代表在**执行增删改之后就会清除一、二级缓存**，保证缓存的一致性；而查询标签默认值为 false，所以查询不会清空缓存
3. localCacheScope：本地缓存作用域， 中的配置项，默认值为 SESSION，当前会话的所有数据保存在会话缓存中，设置为 STATEMENT 禁用一级缓存

---

#### 源码解析

事务提交二级缓存才生效：DefaultSqlSession 调用 commit() 时会回调 `executor.commit()`​

* CachingExecutor#query()：执行查询方法，查询出的数据会先放入 entriesToAddOnCommit 集合暂存

  ```java
  // 从二缓存中获取数据，获取不到去一级缓存获取
  List<E> list = (List<E>) tcm.getObject(cache, key);
  if (list == null) {
      // 回调 BaseExecutor#query
      list = delegate.query(ms, parameterObject, rowBounds, resultHandler, key, boundSql);
      // 将数据放入 entriesToAddOnCommit 集合暂存，此时还没放入二级缓存
      tcm.putObject(cache, key, list);
  }
  ```
* commit()：事务提交，**清空一级缓存，放入二级缓存**，二级缓存使用 TransactionalCacheManager（tcm）管理

  ```java
  public void commit(boolean required) throws SQLException {
      // 首先调用 BaseExecutor#commit 方法，【清空一级缓存】
      delegate.commit(required);
      tcm.commit();
  }
  ```
* TransactionalCacheManager#commit：查询出的数据放入二级缓存

  ```java
  public void commit() {
      // 获取所有的缓存事务，挨着进行提交
      for (TransactionalCache txCache : transactionalCaches.values()) {
          txCache.commit();
      }
  }
  ```

  ```java
  public void commit() {
      if (clearOnCommit) {
          delegate.clear();
      }
      // 将 entriesToAddOnCommit 中的数据放入二级缓存
      flushPendingEntries();
      // 清空相关集合
      reset();
  }
  ```

  ```java
  private void flushPendingEntries() {
      for (Map.Entry<Object, Object> entry : entriesToAddOnCommit.entrySet()) {
          // 将数据放入二级缓存
          delegate.putObject(entry.getKey(), entry.getValue());
      }
  }
  ```

增删改操作会清空缓存：

* update()：CachingExecutor 的更新操作

  ```java
  public int update(MappedStatement ms, Object parameterObject) throws SQLException {
      flushCacheIfRequired(ms);
      // 回调 BaseExecutor#update 方法，也会清空一级缓存
      return delegate.update(ms, parameterObject);
  }
  ```
* flushCacheIfRequired()：判断是否需要清空二级缓存

  ```java
  private void flushCacheIfRequired(MappedStatement ms) {
      Cache cache = ms.getCache();
      // 判断二级缓存是否存在，然后判断标签的 flushCache 的值，增删改操作的 flushCache 属性默认为 true
      if (cache != null && ms.isFlushCacheRequired()) {
          // 清空二级缓存
          tcm.clear(cache);
      }
  }
  ```

‍

‍

### 自定义

自定义缓存

```xml
<cache type="com.domain.something.MyCustomCache"/>
```

type 属性指定的类必须实现 org.apache.ibatis.cache.Cache 接口，且提供一个接受 String 参数作为 id 的构造器

```java
public interface Cache {
  String getId();
  int getSize();
  void putObject(Object key, Object value);
  Object getObject(Object key);
  boolean hasKey(Object key);
  Object removeObject(Object key);
  void clear();
}
```

缓存的配置，只需要在缓存实现中添加公有的 JavaBean 属性，然后通过 cache 元素传递属性值，例如在缓存实现上调用一个名为 `setCacheFile(String file)`​ 的方法：

```xml
<cache type="com.domain.something.MyCustomCache">
  <property name="cacheFile" value="/tmp/my-custom-cache.tmp"/>
</cache>
```

* 可以使用所有简单类型作为 JavaBean 属性的类型，MyBatis 会进行转换。
* 可以使用占位符（如 `${cache.file}`​），以便替换成在配置文件属性中定义的值

MyBatis 支持在所有属性设置完毕之后，调用一个初始化方法， 如果想要使用这个特性，可以在自定义缓存类里实现 `org.apache.ibatis.builder.InitializingObject`​ 接口

```java
public interface InitializingObject {
  void initialize() throws Exception;
}
```

注意：对缓存的配置（如清除策略、可读或可读写等），不能应用于自定义缓存

对某一命名空间的语句，只会使用该命名空间的缓存进行缓存或刷新，在多个命名空间中共享相同的缓存配置和实例，可以使用 cache-ref 元素来引用另一个缓存

```xml
<cache-ref namespace="com.someone.application.data.SomeMapper"/>
```

‍

‍

## 构造语句

### 动态 SQL

#### 基本介绍

动态 SQL 是 MyBatis 强大特性之一，逻辑复杂时，MyBatis 映射配置文件中，SQL 是动态变化的，所以引入动态 SQL 简化拼装 SQL 的操作

DynamicSQL 包含的标签：

* if
* where
* set
* choose (when、otherwise)
* trim
* foreach

各个标签都可以进行灵活嵌套和组合

OGNL：Object Graphic Navigation Language（对象图导航语言），用于对数据进行访问

参考文章：[https://www.cnblogs.com/ysocean/p/7289529.html](https://www.cnblogs.com/ysocean/p/7289529.html)

---

#### where

：条件标签，有动态条件则使用该标签代替 WHERE 关键字，封装查询条件

作用：如果标签返回的内容是以 AND 或 OR 开头的，标签内会剔除掉

表结构：

‍

#### if

基本格式：

```xml
<if test=“条件判断”>
	查询条件拼接
</if>
```

我们根据实体类的不同取值，使用不同的 SQL 语句来进行查询。比如在 id 如果不为空时可以根据 id 查询，如果username 不同空时还要加入用户名作为条件，这种情况在我们的多条件组合查询中经常会碰到。

* UserMapper.xml

  ```xml
  <?xml version="1.0" encoding="UTF-8" ?>
  <!DOCTYPE mapper
          PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
          "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

  <mapper namespace="mapper.UserMapper">
      <select id="selectCondition" resultType="user" parameterType="user">
          SELECT * FROM user
          <where>
              <if test="id != null ">
                  id = #{id}
              </if>
              <if test="username != null ">
                  AND username = #{username}
              </if>
              <if test="sex != null ">
                  AND sex = #{sex}
              </if>
          </where>
      </select>

  </mapper>
  ```
* MyBatisConfig.xml，引入映射配置文件

  ```xml
  <mappers>
      <!--mapper引入指定的映射配置 resource属性执行的映射配置文件的名称-->
      <mapper resource="UserMapper.xml"/>
  </mappers>
  ```
* DAO 层 Mapper 接口

  ```java
  public interface UserMapper {
      //多条件查询
      public abstract List<User> selectCondition(Student stu);
  }
  ```
* 实现类

  ```java
  public class DynamicTest {
      @Test
      public void selectCondition() throws Exception{
          //1.加载核心配置文件
          InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

          //2.获取SqlSession工厂对象
          SqlSessionFactory ssf = new SqlSessionFactoryBuilder().build(is);

          //3.通过工厂对象获取SqlSession对象
          SqlSession sqlSession = ssf.openSession(true);

          //4.获取StudentMapper接口的实现类对象
          UserMapper mapper = sqlSession.getMapper(UserMapper.class);

          User user = new User();
          user.setId(2);
          user.setUsername("李四");
          //user.setSex(男); AND 后会自动剔除

          //5.调用实现类的方法，接收结果
          List<Student> list = mapper.selectCondition(user);

          //6.处理结果
          for (User user : list) {
              System.out.println(user);
          }

          //7.释放资源
          sqlSession.close();
          is.close();
      }
  }
  ```

---

#### set

：进行更新操作的时候，含有 set 关键词，使用该标签

```xml
<!-- 根据 id 更新 user 表的数据 -->
<update id="updateUserById" parameterType="com.ys.po.User">
    UPDATE user u
        <set>
            <if test="username != null and username != ''">
                u.username = #{username},
            </if>
            <if test="sex != null and sex != ''">
                u.sex = #{sex}
            </if>
        </set>
     WHERE id=#{id}
</update>
```

* 如果第一个条件 username 为空，那么 sql 语句为：update user u set u.sex=? where id=?
* 如果第一个条件不为空，那么 sql 语句为：update user u set u.username = ? ,u.sex = ? where id=?

---

#### choose

假如不想用到所有的查询条件，只要查询条件有一个满足即可，使用 choose 标签可以解决此类问题，类似于 Java 的 switch 语句

标签：，

```xml
<select id="selectUserByChoose" resultType="user" parameterType="user">
    SELECT * FROM user
    <where>
        <choose>
            <when test="id !='' and id != null">
                id=#{id}
            </when>
            <when test="username !='' and username != null">
                AND username=#{username}
            </when>
            <otherwise>
                AND sex=#{sex}
            </otherwise>
        </choose>
    </where>
</select>
```

有三个条件，id、username、sex，只能选择一个作为查询条件

* 如果 id 不为空，那么查询语句为：select * from user where id=?
* 如果 id 为空，那么看 username 是否为空

  * 如果不为空，那么语句为：select * from user where username=?
  * 如果 username 为空，那么查询语句为 select * from user where sex=?

---

#### trim

trim 标记是一个格式化的标记，可以完成 set 或者是 where 标记的功能，自定义字符串截取

* prefix：给拼串后的整个字符串加一个前缀，trim 标签体中是整个字符串拼串后的结果
* prefixOverrides：去掉整个字符串前面多余的字符
* suffix：给拼串后的整个字符串加一个后缀
* suffixOverrides：去掉整个字符串后面多余的字符

改写 if + where 语句：

```xml
<select id="selectUserByUsernameAndSex" resultType="user" parameterType="com.ys.po.User">
    SELECT * FROM user
    <trim prefix="where" prefixOverrides="and | or">
        <if test="username != null">
            AND username=#{username}
        </if>
        <if test="sex != null">
            AND sex=#{sex}
        </if>
    </trim>
</select>
```

改写 if + set 语句：

```xml
<!-- 根据 id 更新 user 表的数据 -->
<update id="updateUserById" parameterType="com.ys.po.User">
    UPDATE user u
    <trim prefix="set" suffixOverrides=",">
        <if test="username != null and username != ''">
            u.username = #{username},
        </if>
        <if test="sex != null and sex != ''">
            u.sex = #{sex},
        </if>
    </trim>
    WHERE id=#{id}
</update>
```

---

#### foreach

基本格式：

```xml
<foreach>：循环遍历标签。适用于多个参数或者的关系。
    <foreach collection=“”open=“”close=“”item=“”separator=“”>
		获取参数
</foreach>
```

属性：

* collection：参数容器类型， (list-集合， array-数组)
* open：开始的 SQL 语句
* close：结束的 SQL 语句
* item：参数变量名
* separator：分隔符

需求：循环执行 sql 的拼接操作，`SELECT * FROM user WHERE id IN (1,2,5)`​

* UserMapper.xml片段

  ```xml
  <select id="selectByIds" resultType="user" parameterType="list">
      SELECT * FROM student
      <where>
          <foreach collection="list" open="id IN(" close=")" item="id" separator=",">
              #{id}
          </foreach>
      </where>
  </select>
  ```
* 测试代码片段

  ```java
  //4.获取StudentMapper接口的实现类对象
  UserMapper mapper = sqlSession.getMapper(UserMapper.class);

  List<Integer> ids = new ArrayList<>();
  Collections.addAll(list, 1, 2);
  //5.调用实现类的方法，接收结果
  List<User> list = mapper.selectByIds(ids);

  for (User user : list) {
      System.out.println(user);
  }
  ```

---

#### SQL片段

将一些重复性的 SQL 语句进行抽取，以达到复用的效果

格式：

```xml
<sql id=“片段唯一标识”>抽取的SQL语句</sql>		<!--抽取标签-->
<include refid=“片段唯一标识”/>				<!--引入标签-->
```

使用：

```xml
<sql id="select">SELECT * FROM user</sql>

<select id="selectByIds" resultType="user" parameterType="list">
    <include refid="select"/>
    <where>
        <foreach collection="list" open="id IN(" close=")" item="id" separator=",">
            #{id}
        </foreach>
    </where>
 </select>
```

---

### 逆向工程

MyBatis 逆向工程，可以针对**单表**自动生成 MyBatis 执行所需要的代码（mapper.java、mapper.xml、pojo…）

generatorConfig.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
  PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
  "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
 
<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true" />
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
            connectionURL="jdbc:mysql://localhost:3306/mybatisrelation" userId="root"
            password="root">
        </jdbcConnection>
 
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL和NUMERIC类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false" />
        </javaTypeResolver>
 
        <!-- targetProject:生成PO类的位置！！ -->
        <javaModelGenerator targetPackage="com.ys.po"
            targetProject=".\src">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false" />
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true" />
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置！！ -->
        <sqlMapGenerator targetPackage="com.ys.mapper"
            targetProject=".\src">
            <property name="enableSubPackages" value="false" />
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置，重要！！ -->
        <javaClientGenerator type="XMLMAPPER"
            targetPackage="com.ys.mapper"
            targetProject=".\src">
            <property name="enableSubPackages" value="false" />
        </javaClientGenerator>
        <!-- 指定数据库表，要生成哪些表，就写哪些表，要和数据库中对应，不能写错！ -->
        <table tableName="items"></table>
        <table tableName="orders"></table>
        <table tableName="orderdetail"></table>
        <table tableName="user"></table>   
    </context>
</generatorConfiguration>
```

生成代码：

```java
public void testGenerator() throws Exception{
    List<String> warnings = new ArrayList<String>();
    boolean overwrite = true;
    //指向逆向工程配置文件
    File configFile = new File(GeneratorTest.class.
                               getResource("/generatorConfig.xml").getFile());
    ConfigurationParser cp = new ConfigurationParser(warnings);
    Configuration config = cp.parseConfiguration(configFile);
    DefaultShellCallback callback = new DefaultShellCallback(overwrite);
    MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                                                             callback, warnings);
    myBatisGenerator.generate(null);

}
```

参考文章：[https://www.cnblogs.com/ysocean/p/7360409.html](https://www.cnblogs.com/ysocean/p/7360409.html)

---

### 构建 SQL

#### 基础语法

MyBatis 提供了 org.apache.ibatis.jdbc.SQL 功能类，专门用于构建 SQL 语句

|方法|说明|
| -------------------------------| ----------------------|
|SELECT(String... columns)|根据字段拼接查询语句|
|FROM(String... tables)|根据表名拼接语句|
|WHERE(String... conditions)|根据条件拼接语句|
|INSERT_INTO(String tableName)|根据表名拼接新增语句|
|INTO_VALUES(String... values)|根据值拼接新增语句|
|UPDATE(String table)|根据表名拼接修改语句|
|DELETE_FROM(String table)|根据表名拼接删除语句|

增删改查注解：

* @SelectProvider：生成查询用的 SQL 语句
* @InsertProvider：生成新增用的 SQL 语句
* @UpdateProvider：生成修改用的 SQL 语句注解
* @DeleteProvider：生成删除用的 SQL 语句注解。

  * type 属性：生成 SQL 语句功能类对象
  * method 属性：指定调用方法

---

#### 基本操作

* MyBatisConfig.xml 配置

  ```xml
   <!-- mappers引入映射配置文件 -->
  <mappers>
      <package name="mapper"/>
  </mappers>
  ```
* Mapper 类

  ```java
  public interface StudentMapper {
      //查询全部
      @SelectProvider(type = ReturnSql.class, method = "getSelectAll")
      public abstract List<Student> selectAll();

      //新增数据
      @InsertProvider(type = ReturnSql.class, method = "getInsert")
      public abstract Integer insert(Student student);

      //修改操作
      @UpdateProvider(type = ReturnSql.class, method = "getUpdate")
      public abstract Integer update(Student student);

      //删除操作
      @DeleteProvider(type = ReturnSql.class, method = "getDelete")
      public abstract Integer delete(Integer id);

  }
  ```
* ReturnSQL 类

  ```java
  public class ReturnSql {
      //定义方法，返回查询的sql语句
      public String getSelectAll() {
          return new SQL() {
              {
                  SELECT("*");
                  FROM("student");
              }
          }.toString();
      }

      //定义方法，返回新增的sql语句
      public String getInsert(Student stu) {
          return new SQL() {
              {
                  INSERT_INTO("student");
                  INTO_VALUES("#{id},#{name},#{age}");
              }
          }.toString();
      }

      //定义方法，返回修改的sql语句
      public String getUpdate(Student stu) {
          return new SQL() {
              {
                  UPDATE("student");
                  SET("name=#{name}","age=#{age}");
                  WHERE("id=#{id}");
              }
          }.toString();
      }

      //定义方法，返回删除的sql语句
      public String getDelete(Integer id) {
          return new SQL() {
              {
                  DELETE_FROM("student");
                  WHERE("id=#{id}");
              }
          }.toString();
      }
  }
  ```
* 功能实现类

  ```java
  public class SqlTest {
  	@Test  //查询全部
      public void selectAll() throws Exception{
          //1.加载核心配置文件
          InputStream is = Resources.getResourceAsStream("MyBatisConfig.xml");

          //2.获取SqlSession工厂对象
          SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(is);

          //3.通过工厂对象获取SqlSession对象
          SqlSession sqlSession = sqlSessionFactory.openSession(true);

          //4.获取StudentMapper接口的实现类对象
          StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

          //5.调用实现类对象中的方法，接收结果
          List<Student> list = mapper.selectAll();

          //6.处理结果
          for (Student student : list) {
              System.out.println(student);
          }

          //7.释放资源
          sqlSession.close();
          is.close();
      }

      @Test  //新增
      public void insert() throws Exception{
          //1 2 3 4获取StudentMapper接口的实现类对象
          StudentMapper mapper = sqlSession.getMapper(StudentMapper.class);

          //5.调用实现类对象中的方法，接收结果 ->6 7
          Student stu = new Student(4,"赵六",26);
          Integer result = mapper.insert(stu);
      }

      @Test //修改
      public void update() throws Exception{
          //1 2 3 4 5调用实现类对象中的方法，接收结果 ->6 7 
  		Student stu = new Student(4,"赵六wq",36);
          Integer result = mapper.update(stu);
      }
      @Test //删除
      public void delete() throws Exception{
          //1 2 3 4 5 6 7
          Integer result = mapper.delete(4);
      }
  }
  ```

‍

‍

## 运行原理

‍

### 运行机制

MyBatis 运行过程：

1. 加载 MyBatis 全局配置文件，通过 XPath 方式解析 XML 配置文件，首先解析核心配置文件， 标签中配置属性项有 defaultExecutorType，用来配置指定 Executor 类型，将配置文件的信息填充到 Configuration对象。最后解析映射器配置的映射文件，并**构建 MappedStatement 对象填充至 Configuration**，将解析后的映射器添加到 mapperRegistry 中，用于获取代理
2. 创建一个 DefaultSqlSession 对象，**根据参数创建指定类型的 Executor**，二级缓存默认开启，把 Executor 包装成缓存执行器
3. DefaulSqlSession 调用 getMapper()，通过 JDK 动态代理获取 Mapper 接口的代理对象 MapperProxy
4. 执行 SQL 语句：

    * MapperProxy.invoke() 执行代理方法，通过 MapperMethod#execute 判断执行的是增删改查中的哪个方法
    * 查询方法调用 sqlSession.selectOne()，从 Configuration 中获取执行者对象 MappedStatement，然后 Executor 调用 executor.query 开始执行查询方法
    * 首先通过 CachingExecutor 去二级缓存查询，查询不到去一级缓存查询，**最后去数据库查询并放入一级缓存**
    * Configuration 对象根据  标签的 statementType 属性创建 StatementHandler 对象，在 StatementHandler 的构造方法中，创建了 ParameterHandler 和 ResultSetHandler 对象 最后获取 JDBC 原生的 Connection 数据库连接对象，创建 Statement 执行者对象，然后通过 ParameterHandler 设置预编译参数，底层是 TypeHandler#|setParameter 方法，然后通过 StatementHandler 回调执行者对象执行增删改查，最后调用 ResultsetHandler 处理查询结果    四大对象：  StatementHandler：执行 SQL 语句的对象 ParameterHandler：设置预编译参数用的 ResultHandler：处理结果集 Executor：执行器，真正进行 Java 与数据库交互的对象  参考视频：https://www.bilibili.com/video/BV1mW411M737?p=71  获取工厂  SqlSessionFactoryBuilder.build(InputStream, String,  Properties)：构建工厂 XMLConfigBuilder.parse()：解析核心配置文件每个标签的信息（XPath）   parseConfiguration(parser.evalNode(&quot;/configuration&quot;))：读取节点内数据， 是 MyBatis 配置文件中的顶层标签 settings = settingsAsProperties(root.evalNode(&quot;settings&quot;))：读取核心配置文件中的  标签 settingsElement(settings)：设置框架相关的属性  configuration.setCacheEnabled()：设置缓存属性，默认是 true configuration.setDefaultExecutorType()：设置 Executor 类型到 configuration，默认是 SIMPLE  mapperElement(root.evalNode(&quot;mappers&quot;))：解析 mappers 信息，分为 package 和 单个注册两种   if...else...：根据映射方法选择合适的读取方式   XMLMapperBuilder.parse()：解析 mapper 的标签的信息   configurationElement(parser.evalNode(&quot;/mapper&quot;))：解析 mapper 文件，顶层节点    buildStatementFromContext(context.evalNodes(&quot;select...&quot;))：解析每个操作标签 XMLStatementBuilder.parseStatementNode()：解析操作标签的所有的属性 builderAssistant.addMappedStatement(...)：封装成 MappedStatement 对象加入 Configuration 对象，代表一个增删改查的标签       Class&lt;?&gt; mapperInterface = Resources.classForName(mapperClass)：加载 Mapper 接口   Configuration.addMappers()：将核心配置文件配置的映射器添加到 mapperRegistry 中，用来获取代理对象   MapperAnnotationBuilder parser = new MapperAnnotationBuilder(config, type)：创建注解解析器   parser.parse()：解析 Mapper 接口   SqlSource sqlSource = getSqlSourceFromAnnotations()：获取 SQL 的资源对象    builderAssistant.addMappedStatement(...)：封装成 MappedStatement 对象加入 Configuration 对象         return configuration：返回配置完成的 configuration 对象   return new DefaultSqlSessionFactory(config)：返回工厂对象，包含 Configuration 对象  总结：解析 XML 是对 Configuration 中的属性进行填充，那么可以在一个类中创建 Configuration 对象，自定义其中属性的值来达到配置的效果  获取会话  DefaultSqlSessionFactory.openSession()：获取 Session 对象，并且创建 Executor 对象 DefaultSqlSessionFactory.openSessionFromDataSource(...)：ExecutorType 为 Executor 的类型，TransactionIsolationLevel 为事务隔离级别，autoCommit 是否开启事务   transactionFactory.newTransaction(DataSource, IsolationLevel, boolean：事务对象   configuration.newExecutor(tx, execType)：根据参数创建指定类型的 Executor  批量操作笔记的部分有讲解到  的属性 defaultExecutorType，根据配置创建对象 二级缓存默认开启，会包装 Executor 对象 new CachingExecutor(executor)    return new DefaultSqlSession(configuration, executor, autoCommit)：返回 DefaultSqlSession 对象   获取代理  Configuration.getMapper(Class, SqlSession)：获取代理的 mapper 对象 MapperRegistry.getMapper(Class, SqlSession)：MapperRegistry 是 Configuration 属性，在获取工厂对象时初始化  (MapperProxyFactory&lt;T&gt;) knownMappers.get(type)：获取接口信息封装为 MapperProxyFactory 对象 mapperProxyFactory.newInstance(sqlSession)：创建代理对象  new MapperProxy&lt;&gt;(sqlSession, mapperInterface, methodCache)：包装对象  methodCache 是并发安全的 ConcurrentHashMap 集合，存放要执行的方法 MapperProxy&lt;T&gt; implements InvocationHandler 说明 MapperProxy 默认是一个 InvocationHandler 对象   Proxy.newProxyInstance()：JDK 动态代理创建 MapperProxy 对象      执行SQL  MapperProxy.invoke()：执行 SQL 语句，Object 类的方法直接执行 public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {     try {         // 当前方法是否是属于 Object 类中的方法         if (Object.class.equals(method.getDeclaringClass())) {             return method.invoke(this, args);             // 当前方法是否是默认方法         } else if (isDefaultMethod(method)) {             return invokeDefaultMethod(proxy, method, args);         }     } catch (Throwable t) {         throw ExceptionUtil.unwrapThrowable(t);     }     // 包装成一个 MapperMethod 对象并初始化该对象     final MapperMethod mapperMethod = cachedMapperMethod(method);     // 【根据 switch-case 判断使用的什么类型的 SQL 进行逻辑处理】，此处分析查询语句的查询操作     return mapperMethod.execute(sqlSession, args); } sqlSession.selectOne(String, Object)：查询数据 public Object execute(SqlSession sqlSession, Object[] args) {     //.....     // 解析传入的参数     Object param = method.convertArgsToSqlCommandParam(args);     result = sqlSession.selectOne(command.getName(), param); } // DefaultSqlSession.selectList(String, Object) public &lt;E&gt; List&lt;E&gt; selectList(String statement, Object parameter, RowBounds rowBounds) {     // 获取执行者对象     MappedStatement ms = configuration.getMappedStatement(statement);     // 开始执行查询语句，参数通过 wrapCollection() 包装成集合类     return executor.query(ms, wrapCollection(parameter), rowBounds, Executor.NO_RESULT_HANDLER); } Executor#|​query()：   CachingExecutor.query()：先执行 CachingExecutor 去二级缓存获取数据 public class CachingExecutor implements Executor {   private final Executor delegate;		// 包装了 BaseExecutor，二级缓存不存在数据调用 BaseExecutor 查询 }   MappedStatement.getBoundSql(parameterObject)：把 parameterObject 封装成 BoundSql 构造函数中有：this.parameterObject = parameterObject    CachingExecutor.createCacheKey()：创建缓存对象   ms.getCache()：获取二级缓存   tcm.getObject(cache, key)：尝试从二级缓存中获取数据     BaseExecutor.query()：二级缓存不存在该数据，调用该方法  localCache.getObject(key) ：尝试从本地缓存（一级缓存）获取数据    BaseExecutor.queryFromDatabase()：缓存获取数据失败，开始从数据库获取数据，并放入本地缓存   SimpleExecutor.doQuery()：执行 query   configuration.newStatementHandler()：创建 StatementHandler 对象  根据  标签的 statementType 属性，根据属性选择创建哪种对象
    * 判断 BoundSql 是否被创建，没有创建会重新封装参数信息到 BoundSql
    * **StatementHandler 的构造方法中，创建了 ParameterHandler 和 ResultSetHandler 对象**
    * ​`interceptorChain.pluginAll(statementHandler)`​：拦截器链
5. ​`prepareStatement()`​：通过 StatementHandler 创建 JDBC 原生的 Statement 对象

    * ​`getConnection()`​：**获取 JDBC 的 Connection 对象**
    * ​`handler.prepare()`​：初始化 Statement 对象

      * ​`instantiateStatement(Connection connection)`​：Connection 中的方法实例化对象

        * 获取普通执行者对象：`Connection.createStatement()`​
        * **获取预编译执行者对象**：`Connection.prepareStatement()`​
    * ​`handler.parameterize()`​：进行参数的设置

      * ​`ParameterHandler.setParameters()`​：**通过 ParameterHandler 设置参数**

        * ​`typeHandler.setParameter()`​：底层通过 TypeHandler 实现，回调 JDBC 的接口进行设置
6. ​`StatementHandler.query()`​：**调用 JDBC 原生的 PreparedStatement 执行 SQL**

    ```java
    public <E> List<E> query(Statement statement, ResultHandler resultHandler) {
        // 获取 SQL 语句
        String sql = boundSql.getSql();
        statement.execute(sql);
        // 通过 ResultSetHandler 对象封装结果集，映射成 JavaBean
        return resultSetHandler.handleResultSets(statement);
      }
    ```

    ​`resultSetHandler.handleResultSets(statement)`​：处理结果集

    * ​`handleResultSet(rsw, resultMap, multipleResults, null)`​：底层回调

      * ​`handleRowValues()`​：逐行处理数据，根据是否配置了 属性选择是否使用简单结果集映射

        * 首先判断数据是否被限制行数，然后进行结果集的映射
        * 最后将数据存入 ResultHandler 对象，底层就是 List 集合

          ```java
          public class DefaultResultHandler implements ResultHandler<Object> {
          	private final List<Object> list;
            	public void handleResult(ResultContext<?> context) {
              	list.add(context.getResultObject());
            	}
          }
          ```
    * ​`return collapseSingleResultList(multipleResults)`​：可能存在多个结果集的情况
7. ​`localCache.putObject(key, list)`​：**放入一级（本地）缓存**  
    ​`return list.get(0)`​：返回结果集的第一个数据

‍

​​

‍

‍

‍

## 插件使用

### 插件原理

实现原理：插件是按照插件配置顺序创建层层包装对象，执行目标方法的之后，按照逆向顺序执行（栈）

‍

在四大对象创建时：* 每个创建出来的对象不是直接返回的，而是 `interceptorChain.pluginAll(parameterHandler)`​

* 获取到所有 Interceptor（插件需要实现的接口），调用 `interceptor.plugin(target)`​返回 target 包装后的对象
* 插件机制可以使用插件为目标对象创建一个代理对象，代理对象可以**拦截到四大对象的每一个执行**

```java
@Intercepts(
		{
		@Signature(type=StatementHandler.class,method="parameterize",args=java.sql.Statement.class)
		})
public class MyFirstPlugin implements Interceptor{

	//intercept：拦截目标对象的目标方法的执行
	@Override
	public Object intercept(Invocation invocation) throws Throwable {
		System.out.println("MyFirstPlugin...intercept:" + invocation.getMethod());
		//动态的改变一下sql运行的参数：以前1号员工，实际从数据库查询11号员工
		Object target = invocation.getTarget();
		System.out.println("当前拦截到的对象：" + target);
		//拿到：StatementHandler==>ParameterHandler===>parameterObject
		//拿到target的元数据
		MetaObject metaObject = SystemMetaObject.forObject(target);
		Object value = metaObject.getValue("parameterHandler.parameterObject");
		System.out.println("sql语句用的参数是：" + value);
		//修改完sql语句要用的参数
		metaObject.setValue("parameterHandler.parameterObject", 11);
		//执行目标方法
		Object proceed = invocation.proceed();
		//返回执行后的返回值
		return proceed;
	}

	// plugin：包装目标对象的，为目标对象创建一个代理对象
	@Override
	public Object plugin(Object target) {
		//可以借助 Plugin 的 wrap 方法来使用当前 Interceptor 包装我们目标对象
		System.out.println("MyFirstPlugin...plugin:mybatis将要包装的对象" + target);
		Object wrap = Plugin.wrap(target, this);
		//返回为当前target创建的动态代理
		return wrap;
	}

	// setProperties：将插件注册时的property属性设置进来
	@Override
	public void setProperties(Properties properties) {
		System.out.println("插件配置的信息：" + properties);
	}
}
```

核心配置文件：

```xml
<!--plugins：注册插件  -->
<plugins>
    <plugin interceptor="mybatis.dao.MyFirstPlugin">
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
    </plugin>
</plugins>
```

---

### 分页插件

‍

‍

‍

* 分页可以将很多条结果进行分页显示。如果当前在第一页，则没有上一页。如果当前在最后一页，则没有下一页，需要明确当前是第几页，这一页中显示多少条结果。
* MyBatis 是不带分页功能的，如果想实现分页功能，需要手动编写 LIMIT 语句，不同的数据库实现分页的 SQL 语句也是不同，手写分页 成本较高。
* PageHelper：第三方分页助手，将复杂的分页操作进行封装，从而让分页功能变得非常简单

---

### 分页操作

开发步骤：1. 导入 PageHelper 的 Maven 坐标

1. 在 MyBatis 核心配置文件中配置 PageHelper 插件  
    注意：分页助手的插件配置在通用 Mapper 之前

    ```xml
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor">
            <!-- 指定方言 -->
        	<property name="dialect" value="mysql"/>
        </plugin> 
    </plugins>
    <mappers>.........</mappers>
    ```
2. 与 MySQL 分页查询页数计算公式不同  
    ​`static <E> Page<E> startPage(int pageNum, int pageSize)`​：pageNum第几页，pageSize页面大小

    ```java
    @Test
    public void selectAll() {
        //第一页：显示2条数据
        PageHelper.startPage(1,2);
        List<Student> students = sqlSession.selectList("StudentMapper.selectAll");
        for (Student student : students) {
            System.out.println(student);
        }
    }
    ```

---

### 参数获取

PageInfo构造方法：* `PageInfo<Student> info = new PageInfo<>(list)`​ : list 是 SQL 执行返回的结果集合，参考上一节

PageInfo相关API：1. startPage()：设置分页参数

1. PageInfo：分页相关参数功能类。
2. getTotal()：获取总条数
3. getPages()：获取总页数
4. getPageNum()：获取当前页
5. getPageSize()：获取每页显示条数
6. getPrePage()：获取上一页
7. getNextPage()：获取下一页
8. isIsFirstPage()：获取是否是第一页
9. isIsLastPage()：获取是否是最后一页

‍

## configuration.xml

‍

|设置名|描述|有效值|默认值|
| --------------------------------------------------------------------------------------------------------------------------------------------------------------| ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------| -------------------------------------------------| -------------------------------------------------------|
|cacheEnabled|全局性地开启或关闭所有映射器配置文件中已配置的任何缓存|true|false|
|lazyLoadingEnabled|延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。特定关联关系中可通过设置fetchType属性来覆盖该项的开关状态|true|false|
|aggressiveLazyLoading|开启时，任一方法的调用都会加载该对象的所有延迟加载属性。否则，每个延迟加载属性会按需加载|true|false|
|multipleResultSetsEnabled|是否允许单个语句返回多结果集(需要数据库驱动支持)|true|false|
|useColumnLabel|使用列标签代替列明。实际表现依赖于数据库驱动，具体可参考数据库驱动的相关文档，或通过对比测试来观察|true|false|
|useGeneratedKeys|允许JDBC支持自动生成主键，需要数据库驱动支持。如果设置为true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作(如Derby)|true|false|
|autoMappingBehavior|指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示关闭自动映射；PARTIAL 只会自动映射没有定义嵌套结果映射的字段。 FULL 会自动映射任何复杂的结果集（无论是否嵌套）。|NONE, PARTIAL, FULL|PARTIAL|
|autoMappingUnknownColumnBehavior|指定发现自动映射目标未知列（或未知属性类型）的行为。 NONE: 不做任何反应|||
|WARNING: 输出警告日志（'org.apache.ibatis.session.AutoMappingUnknownColumnBehavior' 的日志等级必须设置为 WARN） FAILING: 映射失败 (抛出 SqlSessionException)|NONE, WARNING, FAILING|NONE||
|defaultExecutorType|配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（PreparedStatement）； BATCH 执行器不仅重用语句还会执行批量更新。|SIMPLE REUSE BATCH|SIMPLE|
|defaultStatementTimeout|设置超时时间，它决定数据库驱动等待数据库响应的秒数。|任意正整数|未设置 (null)|
|defaultFetchSize|为驱动的结果集获取数量（fetchSize）设置一个建议值。此参数只可以在查询设置中被覆盖。|任意正整数|未设置 (null)|
|defaultResultSetType|指定语句默认的滚动策略。（新增于 3.5.2）|FORWARD_ONLY|SCROLL_SENSITIVE|
|safeRowBoundsEnabled|是否允许在嵌套语句中使用分页（RowBounds）。如果允许使用则设置为 false。|true|false|
|safeResultHandlerEnabled|是否允许在嵌套语句中使用结果处理器（ResultHandler）。如果允许使用则设置为 false。|true|false|
|mapUnderscoreToCamelCase|是否开启驼峰命名自动映射，即从经典数据库列名 A_COLUMN 映射到经典 Java 属性名 aColumn。|true|false|
|localCacheScope|MyBatis 利用本地缓存机制（Local Cache）防止循环引用和加速重复的嵌套查询。 默认值为 SESSION，会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地缓存将仅用于执行语句，对相同 SqlSession 的不同查询将不会进行缓存。|SESSION|STATEMENT|
|jdbcTypeForNull|当没有为参数指定特定的 JDBC类型时，空值的默认 JDBC类型。 某些数据库驱动需要指定列的 JDBC类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR 或 OTHER。|JdbcType 常量，常用值：NULL、VARCHAR 或 OTHER。|OTHER|
|lazyLoadTriggerMethods|指定对象的哪些方法触发一次延迟加载。|用逗号分隔的方法列表。|equals,clone,hashCode,toString|
|defaultScriptingLanguage|指定动态 SQL 生成使用的默认脚本语言。|一个类型别名或全限定类名。|org.apache.ibatis.scripting.xmltags.XMLLanguageDriver|
|defaultEnumTypeHandler|指定Enum使用的默认TypeHandler。（新增于 3.4.5）|一个类型别名或全限定类名。|org.apache.ibatis.type.EnumTypeHandler|
|callSettersOnNulls|指定当结果集中值为 null 的时候是否调用映射对象的 setter（map 对象时为 put）方法，这在依赖于 Map.keySet() 或 null 值进行初始化时比较有用。注意基本类型（int、boolean 等）是不能设置成 null 的。|true|false|
|returnInstanceForEmptyRow|当返回行的所有列都是空时，MyBatis默认返回 null。 当开启这个设置时，MyBatis会返回一个空实例。 请注意，它也适用于嵌套的结果集（如集合或关联）。（新增于 3.4.2）|true|false|
|logPrefix|指定 MyBatis 增加到日志名称的前缀。|任何字符串|未设置|
|logImpl|指定 MyBatis 所用日志的具体实现，未指定时将自动查找。|SLF4J|LOG4J(deprecated since 3.5.9)|
|proxyFactory|指定 Mybatis 创建可延迟加载对象所用到的代理工具。|CGLIB|JAVASSIST|
|vfsImpl|指定 VFS 的实现|自定义 VFS 的实现的类全限定名，以逗号分隔。|未设置|
|useActualParamName|允许使用方法签名中的名称作为语句参数名称。 为了使用该特性，你的项目必须采用 Java 8 编译，并且加上 -parameters 选项。（新增于 3.4.1）|true|false|
|configurationFactory|指定一个提供 Configuration实例的类。 这个被返回的 Configuration 实例用来加载被反序列化对象的延迟加载属性值。 这个类必须包含一个签名为static Configuration getConfiguration() 的方法。（新增于 3.2.3）|一个类型别名或完全限定类名。|未设置|
|shrinkWhitespacesInSql|从SQL中删除多余的空格字符。请注意，这也会影响SQL中的文字字符串。 (新增于 3.5.5)|true|false|
|defaultSqlProviderType|指定一个包含提供程序方法的 sql 提供程序类（自 3.5.6 起）。 当省略这些属性时，此类适用于 sql 提供程序注释（例如 @SelectProvider）上的类型（或值）属性。|类型别名或完全限定的类名|未设置|
|nullableOnForEach|指定'foreach'标记上的'nullable'属性的默认值。（自 3.5.9 起）|true|false|

‍

‍

‍

# 实操

‍

​​

## BUGFIX

‍
