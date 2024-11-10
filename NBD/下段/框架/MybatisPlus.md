--后端数据库操作框架Mybatis强化

‍

‍

‍

是基于MyBatis框架基础上开发的增强型工具，旨在简化开发、提供效率 [官网](https://baomidou.com/)

MP是MyBatis的一套增强工具，它是在MyBatis的基础上进行开发的，我们虽然使用MP但是底层依然是MyBatis的东西

‍

‍

## Header

‍

> Spring整合MybatisPlus 见SpringBoot
>
> 对于Mybatis整合MP有常常有三种用法，Mybatis+MP、Spring+Mybatis+MP、Spring Boot+Mybatis+MP.

‍

‍

### 插件

‍

使用的 MybatisX 提示 XML与DAO关联

‍

# 知识

‍

从这张图中我们可以看出MP旨在成为MyBatis的最好搭档，而不是替换MyBatis,所以可以理解为，也就是说我们也可以在MP中写MyBatis的内容. 

对于MP的学习，大家可以参考着官方文档来进行学习，里面都有详细的代码案例. 

‍

## 特性

‍

* **无侵入：** 只做增强不做改变，引入它不会对现有工程产生影响，如丝般顺滑
* **损耗小：** 启动即会自动注入基本 CURD，性能基本无损耗，直接面向对象操作
* **强大的 CRUD 操作：** 内置通用 Mapper、通用 Service，仅仅通过少量配置即可实现单表大部分 CRUD 操作， 更有强大的条件构造器，满足各类使用需求
* **支持 Lambda 形式调用：** 通过 Lambda 表达式，方便的编写各类查询条件，无需再担心字段写错
* **支持多种数据库：** 支持 MySQL、MariaDB、Oracle、DB2、H2、HSQL、SQLite、Postgre、
* SQLServer2005、SQLServer 等多种数据库
* **支持主键自动生成：** 支持多达 4 种主键策略（内含分布式唯一 ID 生成器 - Sequence），可自由配置，完美解 决主键问题
* **支持 XML 热加载：** Mapper 对应的 XML 支持热加载，对于简单的 CRUD 操作，甚至可以无 XML 启动
* **支持 ActiveRecord 模式：** 支持 ActiveRecord 形式调用，实体类只需继承 Model 类即可进行强大的 CRUD 操 作
* **支持自定义全局通用操作：** 支持全局通用方法注入（ Write once, use anywhere ）
* **支持关键词自动转义：** 支持数据库关键词（order、key......）自动转义，还可自定义关键词
* **内置代码生成器：** 采用代码或者 Maven 插件可快速生成 Mapper 、 Model 、 Service 、 Controller 层代码， 支持模板引擎，更有超多自定义配置等您来使用
* **内置分页插件：** 基于 MyBatis 物理分页，开发者无需关心具体操作，配置好插件之后，写分页等同于普通List 查询
* **内置性能分析插件：** 可输出 Sql 语句以及其执行时间，建议开发测试时启用该功能，能快速揪出慢查询
* **内置全局拦截插件：** 提供全表 delete 、 update 操作智能分析阻断，也可自定义拦截规则，预防误操作
* **内置 Sql 注入剥离器：** 支持 Sql 注入剥离，有效预防 Sql 注入攻击

‍

‍

‍

‍

# 基础

‍

‍

## 搭建

‍

‍

### 依赖

‍

* 引入MyBatis-Plus之后请不要再次引入MyBatis以及MyBatis-Spring，以避免因版本差异导致的问题
* 配不好可能会导致MBPP 找不到Mapper位置

‍

‍

### 基础配置

‍

yml

```yml
mybatis-plus:
  configuration:
    #控制台打印完整带参数SQL语句
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
    call-setters-on-nulls: true
  # 这里根据自己项目的包修改，扫描到自己的*xml文件
  mapper-locations: classpath:mapper/*.xml
```

‍

pom

‍

```xml
<build>
	<resources>
          <resource>
              <directory>src/main/java</directory>
              <includes>
                  <include>**/*.xml</include>
              </includes>
              <filtering>true</filtering>
          </resource>

          <resource>
              <directory>src/main/resources</directory>
              <includes>
                  <include>**.*</include>
              </includes>
          </resource>

      </resources>
 </build>

```

‍

‍

### 实体类的别名扫描包

type-aliases-package

```yml
mybatis-plus:
  type-aliases-package: com.itheima.mp.domain.po
```

配置以简化xml文件中resultType中指定路径配置, resultType就可以直接用对应"类名"

‍

‍

### 全局id类型

id-type

```YAML
mybatis-plus:

  global-config:
    db-config:
      id-type: auto # 全局id类型为自增长
```

‍

### mapper包

MyBatisPlus手写SQL - mapper文件的读取地址

```YAML
mybatis-plus:
  mapper-locations: "classpath*:/mapper/**/*.xml" 
```

默认值是`classpath*:/mapper/**/*.xml`​

‍

‍

‍

## 声明表信息

UserMapper在继承BaseMapper的时候指定了一个泛型, 泛型中的User就是与数据库对应的PO

MybatisPlus就是根据PO实体的信息来推断出表的信息，从而生成SQL的

‍

默认情况

* MybatisPlus会把PO实体的类名驼峰转下划线作为表名
* MybatisPlus会把PO实体的所有变量名驼峰转下划线作为表的字段名，并根据变量类型推断字段类型
* MybatisPlus会把名为id的字段作为主键

‍

但很多情况下，默认的实现与实际场景不符，因此MybatisPlus提供了一些注解便于我们声明表信息

‍

‍

###  **@TableName**

‍

表名注解，标识实体类对应的表

* 使用位置  实体类

‍

|名称|@TableName|
| ----------| -------------------------------|
|类型|类注解|
|位置|模型类定义上方|
|作用|设置当前类对应于数据库表关系|
|相关属性|value(默认)：设置数据库表名称|

‍

#### 属性

‍

|**属性**|**类型**|**必须指定**|**默认值**|**描述**|
| ------------------| ----------| ----| -------| -------------------------------------------------------------------------------------------|
|value|String|否|""|表名|
|schema|String|否|""|schema|
|keepGlobalPrefix|boolean|否|false|是否保持使用全局的 tablePrefix 的值（当全局 tablePrefix 生效时）|
|resultMap|String|否|""|xml 中 resultMap 的 id（用于满足特定类型的实体类对象绑定）|
|autoResultMap|boolean|否|false|是否自动构建 resultMap 并使用（如果设置 resultMap 则不会进行 resultMap 的自动构建与注入）|
|excludeProperty|String[]|否|{}|需要排除的属性名 @since 3.3.1|

‍

#### 示例

```Java
@TableName("user")
public class User {
    private Long id;
    private String name;
}
```

‍

‍

###  **@TableId**

‍

主键注解，标识实体类中的主键字段

* 使用位置  实体类的主键字段

‍

#### 属性

‍

|**属性**|**类型**|**必须指定**|**默认值**|**描述**|
| -------| --------| ----| -------------| --------------|
|value|String|否|""|表名|
|type|Enum|否|IdType.NONE|指定主键类型|

‍

##### `IdType`​选项

支持类型

|**值**|**描述**|
| -------------| -----------------------------------------------------------------------------------------------------------------------------------------------------------|
|<br />||
|AUTO|数据库 ID 自增|
|NONE|无状态，该类型为未设置主键类型（注解里等于跟随全局，全局里约等于 INPUT）|
|INPUT|insert 前自行 set 主键值|
|ASSIGN_ID|分配 ID(主键类型为 Number(Long 和 Integer)或 String)(since 3.3.0),使用接口IdentifierGenerator的方法nextId(默认实现类为DefaultIdentifierGenerator雪花算法)|
|ASSIGN_UUID|分配 UUID,主键类型为 String(since 3.3.0),使用接口IdentifierGenerator的方法nextUUID(默认 default 方法)|
|~~ID_WORKER~~|分布式全局唯一 ID 长整型类型(please use ASSIGN_ID)|
|UUID|32 位 UUID 字符串(please use ASSIGN_UUID)|
|~~ID_WORKER_STR~~|分布式全局唯一 ID 字符串类型(please use ASSIGN_ID)|

‍

常见

* ​`AUTO`​：利用数据库的id自增长
* ​`INPUT`​：手动生成id
* ​`ASSIGN_ID`​：雪花算法生成`Long`​类型的全局唯一id，这是默认的ID策略

‍

‍

#### 示例

```Java
@TableName("user")
public class User {
    @TableId
    private Long id;
    private String name;
}
```

‍

###  **@TableField**

‍

|名称|@TableField|
| ----------| -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|类型|==属性注解==|
|位置|模型类属性定义上方|
|作用|设置当前属性对应的数据库表中的字段关系|
|相关属性|value(默认)：设置数据库表字段名称<br />exist:设置属性在数据库表字段中是否存在，默认为true，此属性不能与value合并使用<br />select:设置属性是否参与查询，此属性与select()映射配置不冲突|

‍

普通字段注解

‍

一般情况下我们并不需要给字段添加`@TableField`​注解，一些特殊情况除外：

* 成员变量名与数据库字段名不一致
* 成员变量是以`isXXX`​命名，按照`JavaBean`​的规范，`MybatisPlus`​识别字段时会把`is`​去除，这就导致与数据库不符.
* 成员变量名与数据库一致，但是与数据库的关键字冲突. 使用`@TableField`​注解给字段名添加````转义

‍

‍

支持的其它属性如下：

‍

|**属性**|**类型**|**必填**|**默认值**|**描述**|
| ------------------| ------------| ----| -----------------------| --------------------------------------------------------------------------------------------------------------------------------------------------------|
|value|String|否|""|数据库字段名|
|exist|boolean|否|true|是否为数据库表字段|
|condition|String|否|""|字段 where 实体查询比较条件，有值设置则按设置的值为准，没有则为默认全局的 %s=#{%s}  [参考](https://github.com/baomidou/mybatis-plus/blob/3.0/mybatis-plus-annotation/src/main/java/com/baomidou/mybatisplus/annotation/SqlCondition.java)|
|update|String|否|""|字段 update set 部分注入，例如：当在version字段上注解update="%s+1" 表示更新时会 set version=version+1 （该属性优先级高于 el 属性）|
|insertStrategy|Enum|否|FieldStrategy.DEFAULT|举例：<br />NOT_NULLinsert into table_a(<if test="columnProperty != null">column</if>) values (<if test="columnProperty != null">#{columnProperty}</if>)<br />|
|updateStrategy|Enum|否|FieldStrategy.DEFAULT|举例：<br />IGNOREDupdate table_a set column=#{columnProperty}<br />|
|whereStrategy|Enum|否|FieldStrategy.DEFAULT|举例：<br />NOT_EMPTYwhere <if test="columnProperty != null and columnProperty!=''">column=#{columnProperty}</if><br />|
|fill|Enum|否|FieldFill.DEFAULT|字段自动填充策略|
|select|boolean|否|true|是否进行 select 查询|
|keepGlobalFormat|boolean|否|false|是否保持使用全局的 format 进行处理|
|jdbcType|JdbcType|否|JdbcType.UNDEFINED|JDBC 类型 (该默认值不代表会按照该值生效)|
|typeHandler|TypeHander|否|<br />|类型处理器 (该默认值不代表会按照该值生效)|
|numericScale|String|否|""|指定小数点后保留的位数|

‍

‍

## IService

MybatisPlus不仅提供了BaseMapper，还提供了通用的Service接口及默认实现，封装了一些常用的service模板方法.  通用接口为`IService`​，默认实现为`ServiceImpl`​，其中封装的方法可以分为以下几类

* ​`save  `​新增
* ​`remove`​  删除
* ​`update`​  更新
* ​`get`​  查询单个结果
* ​`list  `​查询集合结果
* ​`count`​  计数
* ​`page`​  分页查询

‍

### 简化

接口和对应的实现类

```java
public interface UserService{

}

@Service
public class UserServiceImpl implements UserService{

}
```

```java
public interface UserService{
	public List<User> findAll();
}

@Service
public class UserServiceImpl implements UserService{
    @Autowired
    private UserDao userDao;
  
	public List<User> findAll(){
        return userDao.selectList(null);
    }
}
```

‍

MP提供了一个Service接口和实现类，分别是:`IService`​和`ServiceImpl`​, 后者是对前者的一个具体实现

‍

#### 使用模板

```java
public interface xxxService extends IService<User>{

}

@Service
public class xxxServiceImpl extends ServiceImpl<UserDao, User> implements xxxService{

}
```

MP帮我们把业务层的一些基础的增删改查都已经实现了

‍

‍

#### 快速实现简单接口

简单功能能够快速实现接口(**直接在controller即可实现)**

**带有业务逻辑的接口则需要自定义实现**

```java
       private final IUserService xxxService; //注入
```

‍

Restful风格编写示例CRUD

```Java
@RequiredArgsConstructor
@RestController
@RequestMapping("users")
public class UserController {

    private final IUserService userService;


    @PostMapping
    public void saveUser(@RequestBody UserFormDTO userFormDTO){

        // 1.转换DTO为PO
        User user = BeanUtil.copyProperties(userFormDTO, User.class);

        // 2.新增
        userService.save(user);
    }


    @DeleteMapping("/{id}")
    public void removeUserById(@PathVariable("id") Long userId){

        userService.removeById(userId);
    }


    @GetMapping("/{id}")
    public UserVO queryUserById(@PathVariable("id") Long userId){

        // 1.查询用户
        User user = userService.getById(userId);

        // 2.处理vo
        return BeanUtil.copyProperties(user, UserVO.class);
    }


    @GetMapping
    public List<UserVO> queryUserByIds(@RequestParam("ids") List<Long> ids){

        // 1.查询用户
        List<User> users = userService.listByIds(ids);

        // 2.处理vo
        return BeanUtil.copyToList(users, UserVO.class);
    }
}
```

‍

‍

‍

‍

‍

‍

‍

### **CRUD**

‍

#### **新增**

‍

* ​`save`​  新增单个元素
* ​`saveBatch`​  批量新增
* ​`saveOrUpdate`​  根据id判断，如果数据存在就更新，不存在则新增
* ​`saveOrUpdateBatch`​  批量的新增或修改

‍

‍

#### **删除**

‍

* ​`removeById`​  根据id删除
* ​`removeByIds`​  根据id批量删除
* ​`removeByMap`​  根据Map中的键值对为条件删除
* ​`remove(Wrapper<T>)`​  根据Wrapper条件删除
* ​`~~removeBatchByIds~~`​  暂不支持

‍

‍

#### **修改**

‍

* ​`updateById`​  根据id修改
* ​`update(Wrapper<T>)`​  根据`UpdateWrapper`​修改，`Wrapper`​中包含`set`​和`where`​部分
* ​`update(T，Wrapper<T>)`​  按照`T`​内的数据修改与`Wrapper`​匹配到的数据
* ​`updateBatchById`​  根据id批量修改

‍

‍

#### **查**

‍

* ​`getById`​：根据id查询1条数据
* ​`getOne(Wrapper<T>)`​：根据`Wrapper`​查询1条数据
* ​`getBaseMapper`​：获取`Service`​内的`BaseMapper`​实现，某些时候需要直接调用`Mapper`​内的自定义`SQL`​时可以用这个方法获取到`Mapper`​

**查多**

* ​`listByIds`​：根据id批量查询
* ​`list(Wrapper<T>)`​：根据Wrapper条件查询多条数据
* ​`list()`​：查询所有

**统计**

* ​`count()`​：统计所有数量
* ​`count(Wrapper<T>)`​：统计符合`Wrapper`​条件的数据数量

‍

‍

‍

### **使用**

不能直接使用IService，而是自定义Service接口，然后继承IService以拓展方法. 同时，让自定义的Service实现类继承ServiceImpl，这样就不用自己实现IService中的接口了. 

‍

#### **getBaseMapper**

通过这个方法获取service对应的Mapper

‍

#### service结构

* IUserSercice 继承 IService
* impl文件夹  
  -UserServiceImpl 继承 ServiceImpl + 实现 UserService(implement)

‍

#### 流程

* IUserService
* UserServiceImpl

‍

定义`IUserService`​，继承`IService`​

```Java
public interface IUserService extends IService<User> {
    // 拓展自定义方法
}
```

编写`UserServiceImpl`​类，继承`ServiceImpl`​，实现`UserService`​

```Java
@Service
public class UserServiceImpl extends ServiceImpl<UserMapper, User> implements UserService {}
```

‍

‍

‍

### **Lambda优化**

‍

#### 一般方法

‍

##### **定义查询条件实体**

UserQuery实体

```Java
package com.itheima.mp.domain.query;


@Data
@ApiModel(description = "用户查询条件实体")
public class UserQuery {
    @ApiModelProperty("用户名关键字")
    private String name;
    @ApiModelProperty("用户状态：1-正常，2-冻结")
    private Integer status;
    @ApiModelProperty("余额最小值")
    private Integer minBalance;
    @ApiModelProperty("余额最大值")
    private Integer maxBalance;
}
```

‍

‍

##### **定义controller方法**

```Java
@GetMapping("/list")
@ApiOperation("根据id集合查询用户")
public List<UserVO> queryUsers(UserQuery query){
    // 1.组织条件
    String username = query.getName();
    Integer status = query.getStatus();
    Integer minBalance = query.getMinBalance();
    Integer maxBalance = query.getMaxBalance();
    LambdaQueryWrapper<User> wrapper = new QueryWrapper<User>().lambda()
            .like(username != null, User::getUsername, username)
            .eq(status != null, User::getStatus, status)
            .ge(minBalance != null, User::getBalance, minBalance)
            .le(maxBalance != null, User::getBalance, maxBalance);
    // 2.查询用户
    List<User> users = userService.list(wrapper);
    // 3.处理vo
    return BeanUtil.copyToList(users, UserVO.class);
}
```

‍

在组织查询条件的时候加入了 `username != null`​ 这样的参数，意思就是当条件成立时才会添加这个查询条件，类似Mybatis的mapper.xml文件中的`<if>`​标签. 这样就实现了动态查询条件效果了. 

‍

‍

#### 优化LQ

LambdaQuery

Service中对`LambdaQueryWrapper`​和`LambdaUpdateWrapper`​的用法进一步做了简化. 

无需自己通过`new`​的方式来创建`Wrapper`​，而是直接调用`lambdaQuery`​和`lambdaUpdate`​方法

MybatisPlus会根据链式编程的最后一个方法来判断最终的返回结果. 

‍

##### 示例

```Java
@GetMapping("/list")
@ApiOperation("根据id集合查询用户")
public List<UserVO> queryUsers(UserQuery query){

    // 1.组织条件
    String username = query.getName();
    Integer status = query.getStatus();
    Integer minBalance = query.getMinBalance();
    Integer maxBalance = query.getMaxBalance();

    // 2.查询用户
    List<User> users = userService.lambdaQuery()
            .like(username != null, User::getUsername, username)
            .eq(status != null, User::getStatus, status)
            .ge(minBalance != null, User::getBalance, minBalance)
            .le(maxBalance != null, User::getBalance, maxBalance)
            .list();

    // 3.处理vo
    return BeanUtil.copyToList(users, UserVO.class);
}
```

‍

lambdaQuery方法中除了可以构建条件，还需要在链式编程的最后添加一个`list()`​，这是在告诉MP我们的调用结果需要是一个list集合. 这里不仅可以用`list()`​，可选的方法有

* ​`.one()`​：最多1个结果
* ​`.list()`​：返回集合结果
* ​`.count()`​：返回计数结果

‍

‍

#### 优化LU

LambdaUpdate

‍

‍

##### 示例

动态的

```Java
@Override
@Transactional
public void deductBalance(Long id, Integer money) {
    // 1.查询用户
    User user = getById(id);
    // 2.校验用户状态
    if (user == null || user.getStatus() == 2) {
        throw new RuntimeException("用户状态异常！");
    }
    // 3.校验余额是否充足
    if (user.getBalance() < money) {
        throw new RuntimeException("用户余额不足！");
    }
    // 4.扣减余额 update tb_user set balance = balance - ?
    int remainBalance = user.getBalance() - money;
    lambdaUpdate()
            .set(User::getBalance, remainBalance) // 更新余额
            .set(remainBalance == 0, User::getStatus, 2) // 动态判断，是否更新status
            .eq(User::getId, id)
            .eq(User::getBalance, user.getBalance()) // 乐观锁
            .update();
}
```

‍

‍

‍

## CRUD

**标准数据层开发**

‍

数据层标准的CRUD(增删改查)的实现与分页功能

‍

|功能|自定义接口|MP接口|
| :----------: | :--------------------------------------: | :-----------------------------------------------------------: |
|新增|boolean save(T t)|int insert(T t)|
|删除|boolean delete(int id)|int deleteById(Serializable id)|
|修改|boolean update(T t)|int updateById(T t)|
|ID查询|T getById(int id)|T selectById(Serializable id)|
|查全部|List<T> getAll()|List<T> selectList(Wrapper<T> queryWrapper)|
|分页查|PageInfo<T> getAll(int page, int size)|IPage<T> selectPage(IPage<T> page, Wrapper<T> queryWrapper)|
|条件分页查|List<T> getAll(Condition condition)|IPage<T> selectPage(IPage<T> page, Wrapper<T> queryWrapper)|

‍

‍

### 增

‍

#### 原型

插入对象

```java
int insert (T t)
```

* T  泛型，用来保存新增数据
* int  返回值，新增成功后返回1，没有新增成功返回的是0

‍

‍

### 删

‍

#### 原型

按ID删

```java
int deleteById (Serializable id)
```

* int   返回值，数据删除成功返回1，未删除数据返回0
* Serializable 参数类型序列化

  > Serializable = String "+" Number
  >
  > * String和Number是Serializable的子类，
  > * Number又是Float,Double,Integer等类的父类，
  > * 能作为主键的数据类型都已经是Serializable的子类，
  > * MP使用Serializable作为参数类型，就好比我们可以用Object接收任何数据类型一样.
  >

‍

‍

### 改

‍

#### 原型

```java
int updateById(T t);
```

* T:泛型，需要修改的数据内容，注意因为是根据ID进行修改，所以传入的对象中需要有ID属性值
* int:返回值，修改成功后返回1，未修改数据返回0

修改的时候，只修改实体对象中有值的字段

‍

‍

‍

### 查

‍

#### 原型

‍

##### 单个

```java
T selectById (Serializable id)
```

* Serializable  参数类型,主键ID的值
* T  根据ID查询只会返回一条数据

‍

‍

‍

##### 所有

‍

```java
List<T> selectList(Wrapper<T> queryWrapper)
```

* Wrapper  用来构建条件查询的条件，没有可直接传为Null
* List  返回的数据是一个集合

‍

‍

‍

##### 分页

‍

```java
IPage<T> selectPage(IPage<T> page, Wrapper<T> queryWrapper)
```

* IPage  用来构建分页查询条件
* Wrapper  用来构建条件查询的条件, 没有可直接传为Null
* IPage  返回值，你会发现构建分页条件和方法的返回值都是IPage

IPage是一个接口，我们需要找到它的实现类来构建它

‍

‍

 **(1)调用方法传入参数获取返回值**

```java
  //分页查询
    @Test
    void testSelectPage(){
        //1 创建IPage分页对象,设置分页参数,1为当前页码，3为每页显示的记录数
        IPage<User> page=new Page<>(1,3);
        //2 执行分页查询
        userDao.selectPage(page,null);
        //3 获取分页结果
        System.out.println("当前页码值："+page.getCurrent());
        System.out.println("每页显示数："+page.getSize());
        System.out.println("一共多少页："+page.getPages());
        System.out.println("一共多少条数据："+page.getTotal());
        System.out.println("数据："+page.getRecords());
    }
```

‍

 **(2)设置分页拦截器**

MP已经为我们提供好了，我们只需要将其配置成Spring管理的bean对象即可

```java
@Configuration
public class MybatisPlusConfig {
  
    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor(){

        //1 创建MybatisPlusInterceptor拦截器对象
        MybatisPlusInterceptor mpInterceptor=new MybatisPlusInterceptor();

        //2 添加分页拦截器
        mpInterceptor.addInnerInterceptor(new PaginationInnerInterceptor());

        return mpInterceptor;
    }
}
```

‍

‍

‍

#### 多条件

‍

支持链式编程

‍

##### 示例

查询数据库表中，年龄在10岁到30岁之间的用户信息

```java
 @Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.lt(User::getAge, 30).gt(User::getAge, 10);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

```sql
SELECT id,name,password,age,tel FROM user WHERE (age < ? AND age > ?)
```

‍

需要OR关系，用or()链接就可以了,不加**默认是and**

```java
lqw.lt(User::getAge, 10).or().gt(User::getAge, 30);
```

```sql
SELECT id,name,password,age,tel FROM user WHERE (age < ? OR age > ?)
```

‍

‍

#### null判定

‍

> 一般会有很多条件可以供用户进行选择查询, 这些条件用户可以选择使用也可以选择不使用, 后台如果想接收前端的两个数据，该如何接收?

‍

可以使用两个简单数据类型，也可以使用一个模型类，但是User类中目前只有一个age属性,如:

```java
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}
```

‍

处理方法

‍

##### **添加属性**

添加属性age2,这种做法可以但是**会影响到原模型类的属性内容**

```java
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
    private Integer age2;
}
```

‍

##### 新**模型类**

新建一个**模型类**,让其**继承User类**

并在其中添加age2属性，UserQuery在拥有User属性后同时添加了age2属性

```java
@Data
public class User {
    private Long id;
    private String name;
    private String password;
    private Integer age;
    private String tel;
}

@Data
public class UserQuery extends User {
    private Integer age2;
}
```

‍

示例

```java
    @Test
    void testGetAll(){
        //模拟页面传递过来的查询数据
        UserQuery uq = new UserQuery();
        uq.setAge(10);
        uq.setAge2(30);
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        if(null != uq.getAge2()){
            lqw.lt(User::getAge, uq.getAge2());
        }
        if( null != uq.getAge()) {
            lqw.gt(User::getAge, uq.getAge());
        }
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

‍

> 问题很明显，如果条件多的话，每个条件都需要判断，代码量就比较大

‍

‍

##### 简化lt重载

‍

lt还有一个重载的方法，当condition为true时，添加条件，为false时，不添加条件

```java
public Children lt(boolean condition, R column, Object val) {
    return this.addCondition(condition, column, SqlKeyword.LT, val);
}
```

```java
        lqw.lt(null!=uq.getAge2(),User::getAge, uq.getAge2())
           .gt(null!=uq.getAge(),User::getAge, uq.getAge());
```

‍

‍

#### 投影

查询投影即不查询所有字段，只查询出指定内容的数据

‍

‍

指定字段 --select(…)方法用来设置查询的字段列，可以设置多个

```java
lqw.select(User::getId,User::getName,User::getAge);
```

‍

##### 示例

```java
@Test
    void testGetAll(){
        LambdaQueryWrapper<User> lqw = new LambdaQueryWrapper<User>();
        lqw.select(User::getId,User::getName,User::getAge);
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
    }
```

```sql
SELECT id,name,age FROM user
```

使用的不是lambda，就需要手动指定字段

```java
    @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("id","name","age","tel");
        List<User> userList = userDao.selectList(lqw);
        System.out.println(userList);
```

```sql
SELECT id,name,age,tel FROM user
```

‍

‍

#### 聚合

> 聚合与分组查询，无法使用lambda表达式来完成
>
> MP只是对MyBatis的增强，如果MP实现不了，就可以直接在DAO接口中使用MyBatis的方式实现

‍

聚合函数查询

count:总记录数

max:最大值

min:最小值

avg:平均值

sum:求和

‍

‍

##### 示例

为了在做结果封装的时候能够更简单，将上面的聚合函数都起了个名称，方便后期来获取这些数据

‍

```java
 @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        //lqw.select("count(*) as count");
        //SELECT count(*) as count FROM user
        //lqw.select("max(age) as maxAge");
        //SELECT max(age) as maxAge FROM user
        //lqw.select("min(age) as minAge");
        //SELECT min(age) as minAge FROM user
        //lqw.select("sum(age) as sumAge");
        //SELECT sum(age) as sumAge FROM user
        lqw.select("avg(age) as avgAge");
        //SELECT avg(age) as avgAge FROM user
        List<Map<String, Object>> userList = userDao.selectMaps(lqw);
        System.out.println(userList);
    }
```

‍

‍

#### 分组

> 聚合与分组查询，无法使用lambda表达式来完成
>
> MP只是对MyBatis的增强，如果MP实现不了，就可以直接在DAO接口中使用MyBatis的方式实现

‍

##### 示例

‍

```java
 @Test
    void testGetAll(){
        QueryWrapper<User> lqw = new QueryWrapper<User>();
        lqw.select("count(*) as count,tel");
        lqw.groupBy("tel");
        List<Map<String, Object>> list = userDao.selectMaps(lqw);
        System.out.println(list);
        //for (Map<String, Object> selectMap : userDao.selectMaps(qw)) {
        //   System.out.println(selectMap);
        //}
    }
```

```sql
SELECT count(*) as count,tel FROM user GROUP BY tel
```

‍

‍

‍

#### 特种

其他的方法，比如isNull,isNotNull,in,notIn等等方法可供选择，具体参考官方文档的条件构造器来学习使用

‍

‍

MP的查询条件

* 范围匹配（> 、 = 、between）
* 模糊匹配（like）
* 空判定（null）
* 包含性匹配（in）
* 分组（group）
* 排序（order）
* ……

‍

‍

##### 等值

‍

eq()： 相当于 \=

```sql
SELECT id,name,password,age,tel FROM user WHERE (name \= ? AND password \= ?)
```

‍

selectList：查询结果为多个或者单个

‍

selectOne: 查询结果为单个

‍

‍

## 查询条件对象

​​Wrapper​

用来构建查询条件; 是一个接口, 实现类很多, 意味着有多种构建查询条件对象的方式

‍

‍

### QW

QueryWrapper

无论是**修改、删除、查询**，都可以使用QueryWrapper来构建查询条件

> 会存在写错名称的情况

‍

‍

#### 示例

‍

##### **查询**

> 查询出名字中带`o`​的，存款大于等于1000元的人

```Java
@Test
void testQueryWrapper() {
    // 1.构建查询条件 where name like "%o%" AND balance >= 1000
    QueryWrapper<User> wrapper = new QueryWrapper<User>()
            .select("id", "username", "info", "balance")
            .like("username", "o")
            .ge("balance", 1000);
    // 2.查询数据
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}
```

‍

##### **更新**

> 更新用户名为jack的用户的余额为2000

```Java
@Test
void testUpdateByQueryWrapper() {
    // 1.构建查询条件 where name = "Jack"
    QueryWrapper<User> wrapper = new QueryWrapper<User>().eq("username", "Jack");
    // 2.更新数据，user中非null字段都会作为set语句
    User user = new User();
    user.setBalance(2000);
    userMapper.update(user, wrapper);
}
```

‍

‍

### LambdaQW

QueryWrapper + lambda

‍

User::getAget, 为lambda表达式中的 `类名::方法名`​

构建LambdaQueryWrapper的时候泛型不能省

> 此时再次编写条件的时候，就不会存在写错名称的情况，但是qw后面多了一层lambda()调用

‍

#### 示例

‍

##### 查询

查询年龄小于10的对象

```java
    @Test
    void testGetAll(){
        QueryWrapper<User> qw = new QueryWrapper<User>();
        qw.lambda().lt(User::getAge, 10);//添加条件
        List<User> userList = userDao.selectList(qw);
        System.out.println(userList);
    }
```

‍

最终的sql语句为

```sql
SELECT id,name,password,age,tel FROM user WHERE (age < ?)
```

‍

‍

### **UW**

UpdateWrapper

‍

基于BaseMapper中的update方法, 更新时只能直接赋值，对于一些复杂的需求就难以实现

‍

‍

#### 示例

更新id为`1,2,4`​的用户的余额，(原有基础上扣200

SET的赋值结果是基于字段现有值的，这个时候就要利用UpdateWrapper中的setSql功能了

```Java
@Test
void testUpdateWrapper() {
    List<Long> ids = List.of(1L, 2L, 4L);
    // 1.生成SQL
    UpdateWrapper<User> wrapper = new UpdateWrapper<User>()
            .setSql("balance = balance - 200") // SET balance = balance - 200
            .in("id", ids);     // WHERE id in (1, 2, 4)

    // 2.更新，注意第一个参数可以给null，也就是不填更新字段和数据，而是基于UpdateWrapper中的setSQL来更新
    userMapper.update(null, wrapper);
}
```

```Java
UPDATE user SET balance = balance - 200 WHERE id in (1, 2, 4)
```

‍

‍

### Lambda(Q/U)W

LambdaQueryWrapper / LambdaUpdateWrapper

‍

> 无论是QueryWrapper还是UpdateWrapper在构造条件的时候都需要写死字段名称，会出现字符串`魔法值`​. 这在编程规范中显然是不推荐的.  那怎么样才能不写字段名，又能知道字段名呢？

‍

其中一种办法是基于变量的`gettter`​方法结合反射技术. 因此我们只要将条件对应的字段的`getter`​方法传递给MybatisPlus，它就能计算出对应的变量名了. 而传递方法可以使用JDK8中的`方法引用`​和`Lambda`​表达式.  因此MybatisPlus又提供了一套基于Lambda的Wrapper，包含两个：

* LambdaQueryWrapper
* LambdaUpdateWrapper

分别对应QueryWrapper和UpdateWrapper

‍

#### 示例

```Java
@Test
void testLambdaQueryWrapper() {
    // 1.构建条件 WHERE username LIKE "%o%" AND balance >= 1000
    QueryWrapper<User> wrapper = new QueryWrapper<>();
    wrapper.lambda()
            .select(User::getId, User::getUsername, User::getInfo, User::getBalance)
            .like(User::getUsername, "o")
            .ge(User::getBalance, 1000);
    // 2.查询
    List<User> users = userMapper.selectList(wrapper);
    users.forEach(System.out::println);
}
```

‍

‍

‍

## ID生成策略

使用 **@TableId**

‍

‍

‍

其他的几个策略均已过时，都将被ASSIGN_ID和ASSIGN_UUID代替掉

* AUTO    自动
* NONE    不设置id生成策略
* INPUT    用户手工输入id

**分布式**

* ==ASSIGN_ID==    雪花算法生成id(可兼容数值型与字符串型)
* ==ASSIGN_UUID==    以UUID生成算法作为id生成策略

‍

分布式ID

> 当数据量足够大的时候，一台数据库服务器存储不下，这个时候就需要多台数据库服务器进行存储
>
> 比如订单表就有可能被存储在不同的服务器上
>
> 如果用数据库表的自增主键，因为在两台服务器上所以会出现冲突; 这个时候就需要一个全局唯一ID,这个ID就是分布式ID.

‍

‍

每一种主键策略都有自己的优缺点，根据自己项目业务的实际情况来选择使用才是最明智的选择. 

> 不同的表应用不同的id生成策略
>
> * 日志：自增（1,2,3,4，……）
> * 购物订单：特殊规则（FQ23948AK3843）
> * 外卖单：关联地区日期等信息（10 04 20200314 34 91）
> * 关系表：可省略id
> * ……

‍

‍

###  **@TableId**

|名称|@TableId|
| ----------| --------------------------------------------------------------------------------------|
|类型|==属性注解==|
|位置|模型类中用于表示主键的属性定义上方|
|作用|设置当前类中主键属性的生成策略|
|相关属性|value(默认)：设置数据库表主键名称<br />type:设置主键属性的生成策略，值查照IdType的枚举值|

‍

‍

### 类型

‍

#### NONE

‍

不设置id生成策略，MP不自动生成，约等于INPUT,所以这两种方式都需要用户手动设置，但是手动设置第一个问题是容易出现相同的ID造成主键冲突，为了保证主键不冲突就需要做很多判定，实现起来比较复杂

‍

#### AUTO

==使用数据库ID自增==

一定要确保对应的数据库表设置了ID主键自增，否则无效; 不可作为分布式ID使用

> 控制选项-自动递增修改或IDEA修改表-对应字段修改自动递增选项

```java
    @TableId(type = IdType.AUTO)
```

‍

‍

#### INPUT

==手动输入==

需要将表的自增策略删除掉 (不需要主键自增)

添加数据需要回归到和其他普通字段一样手动设置ID, 否则报错

```java
    @TableId(type = IdType.INPUT)
```

‍

‍

#### ASSIGN_ID

==雪花算法生成id==

不需要手动设置ID，但如果手动设置ID，则会使用自己设置的值

```java
    @TableId(type = IdType.ASSIGN_ID)
```

> 雪花算法(SnowFlake), 其生成的结果是一个64bit大小整数
>
> 可以在分布式的情况下使用，而且能够保证唯一，但是生成的主键是32位的字符串，长度过长占用空间而且还不能排序，查询性能也慢

‍

‍

‍

#### ASSIGN_UUID

==UUID==

**主键的类型不能是Long**，而应该改成String类型

```java
    @TableId(type = IdType.ASSIGN_UUID)
```

‍

> 可以在分布式的情况下使用，生成的是Long类型的数字，可以排序性能也高，但是生成的策略**和服务器时间有关**，如果修改了系统时间就有可能导致出现重复主键

‍

‍

### 简化

‍

#### 模型类主键策略设置

在项目中的每一个模型类上都需要使用相同的生成策略

‍

配置

```yaml
mybatis-plus:
  global-config:
    db-config:
    	id-type: assign_id
```

‍

‍

#### 数据库表与模型类的映射关系

MP会默认将模型类的类名名首字母小写作为表名使用，假如数据库表的名称都以`tbl_`​开头

‍

配置

```yaml
mybatis-plus:
  global-config:
    db-config:
    	table-prefix: tbl_
```

‍

## 逻辑删除

‍

比较重要的数据，往往会采用逻辑删除的方案

对于删除操作业务有两种

* 物理删除    业务数据从数据库中丢弃，执行的是delete操作
* 逻辑删除    为数据设置是否可用状态字段，删除时设置状态字段为不可用状态，**数据保留在数据库中，执行的是update操作**

‍

> 逻辑删除本身也有自己的问题，比如：
>
> * 会导致数据库表垃圾数据越来越多，从而影响查询效率
> * SQL中全都需要对逻辑删除字段做判断，影响查询效率
>
> 一旦采用了逻辑删除，所有的查询和删除逻辑都要跟着变化，非常麻烦. 
>
> 因此不太推荐采用逻辑删除功能，如果数据不能删除，可以采用把数据迁移到其它表的办法.

‍

**逻辑删除的本质其实是**​**==修改操作==**​ **. 如果加了逻辑删除字段，查询数据时也会自动带上逻辑删除字段. **

```yaml
UPDATE tbl_user SET deleted=1 where id = ? AND deleted=0
```

‍

###  **@TableLogic**

|名称|@TableLogic|
| ----------| ----------------------------------------|
|类型|==属性注解==|
|位置|模型类中用于表示删除字段的属性定义上方|
|作用|标识该字段为进行逻辑删除的字段|
|相关属性|value：逻辑未删除值<br />delval:逻辑删除值|

‍

### 流程

‍

综合

* 在表中添加一个字段标记数据是否被删除
* 当删除数据时把标记置为true
* 查询时过滤掉标记为true的数据

‍

‍

#### 数据库表添加​deleted​​​列

‍

字段名可以任意，内容也可以自定义

> 比如`0`​代表正常，`1`​代表删除，可以在添加列的同时设置其默认值为`0`​正常

‍

示例

给`address`​表添加一个逻辑删除字段

```SQL
alter table address add deleted bit default b'0' null comment '逻辑删除';
```

‍

#### 实体类添加属性

‍

给`Address`​实体添加`deleted`​字段

‍

(1)添加与数据库表的列对应的一个属性名，名称可以任意，如果和数据表列名对不上，可以使用@TableField进行关系映射，如果一致，则会自动对应. 

(2)标识新增的字段为逻辑删除字段，使用`@TableLogic`​

示例

```java
@Data
public class User {
...
    @TableLogic(value="0",delval="1")
    private Integer deleted;  //value为正常数据的值，delval为删除数据的值
...
}
```

‍

‍

#### 配置优化

每个表都要有逻辑删除

‍

YML配置

```YAML
mybatis-plus:
  global-config:
    db-config:
      logic-delete-field: deleted # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
      logic-delete-value: 1 # 逻辑已删除值(默认为 1)
      logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
```

‍

#### 其他

如果还是想把已经删除的数据都查询出来, 可以直接Select *

```java


@Mapper
public interface UserDao extends BaseMapper<User> {
    //查询所有数据包含已经被删除的数据
    @Select("select * from tbl_user")
    public List<User> selectAll();
}
```

‍

‍

‍

## 映射匹配兼容

‍

### @TableField 解决

‍

#### 表字段与编码属性设计不同步

> 表字段与编码属性设计不同步; 会导致数据封装不到模型对象，这个时候就需要其中一方做出修改，那如果前提是两边都不能改又该如何解决?

‍

‍

MP提供了一个注解`@TableField`​,使用该注解可以实现模型类属性名和表的列名之间的映射关系

```java
@TableName("tb_user")
@Data
public class User {
    @TableField("pwd")
    private String password;
```

‍

‍

#### 添加了数据库中未定义的属性

> 模型类中多了一个数据库表不存在的字段
>
> 生成的sql语句中在select的时候查询了数据库不存在的字段，，错误信息为: ` Unknown column '多出来的字段名称' in 'field list'`​

‍

​`@TableField`​属性`exist`​，设置该字段是否在数据库表中存在

设置为false则不存在，生成sql语句查询的时候就不会再查询该字段了

```java
  @TableField(exist = false)
    private Integer online;
```

‍

‍

‍

#### 采用默认查询开放了更多的字段查看权限

‍

> 查询表中所有的列的数据，就可能把一些敏感数据查询到返回给前端，这个时候需要限制哪些字段默认不要进行查询

‍

​`@TableField`​属性`select`​，该属性设置默认是否需要查询该字段的值，true(默认值)表示默认查询该字段，false表示默认不查询该字段. 

‍

例如像密码这种的敏感字段，不应该查询出来作为JSON返回给前端，不安全

```java
   @TableField(value = "pwd",select = false)
    private String password;
```

‍

### @TableName 解决

‍

#### 表名与编码开发设计不同步

问题主要是**表的名称**和**模型类的名称**不一致，导致查询失败

‍

​`@TableName`​来设置表与模型类之间的对应关系

实体类上加`@TableName("")`​对应对应的表名字

使用@TableField排除字段`@TableField(exist=false)`​

‍

‍

‍

# 高级

‍

## **日志**

‍

‍

### SQL查阅日志

如果想查看MP执行的SQL语句，可以修改application.yml配置文件，打印SQL日志到控制台

开启日志功能性能就会受到影响，调试完后记得关闭

```yaml
mybatis-plus:
  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl #打印SQL日志到控制台
```

‍

‍

### 日志简化方案

‍

测试的时候，控制台打印的日志比较多，速度有点慢而且不利于查看运行结果

‍

‍

‍

#### 初始化spring日志打印

resources目录下添加 **logback.xml**，名称固定

```xml
<?xml version="1.0" encoding="UTF-8"?>
<configuration>

</configuration>
```

‍

‍

#### MybatisPlus图标

application.yml>>

```yaml
# mybatis-plus日志控制台输出
mybatis-plus:

  configuration:
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl

  global-config:
    banner: off # 关闭mybatisplus启动图标
```

‍

‍

#### SpringBoot图标

application.yml>>

```yaml
spring:
  main:
    banner-mode: off # 关闭SpringBoot启动图标(banner)
```

‍

#### Parsed-mapper-file日志打印

‍

将`yml`​​配置里的`mybatis-plus`​​配置

```yml
log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```

改为Slf4j

```yml
log-impl: org.apache.ibatis.logging.slf4j.Slf4jImpl
```

‍

​`yml`​配置里新增一条

```yml
logging:
  level:
    root: DEBUG
    com.baomidou.mybatisplus.extension.spring.MybatisSqlSessionFactoryBean: INFO
```

‍

‍

## 批处理

‍

### 批量插入

MybatisPlus的批处理比逐条新增效率提高了10倍左右

‍

基于`PrepareStatement`​的预编译模式，然后批量提交，最终在数据库执行时还是会有多条insert语句，逐条插入数据. 而如果想要得到最佳性能，最好是**将多条SQL合并为一条**

MySQL的客户端连接参数中有`rewriteBatchedStatements`​ ->重写批处理的`statement`​语句，将其配置为true

‍

application.yml文件

jdbc的url后面添加参数`&rewriteBatchedStatements=true`​

```YAML
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/mp?useUnicode=true&characterEncoding=UTF-8&autoReconnect=true&serverTimezone=Asia/Shanghai&rewriteBatchedStatements=true
    driver-class-name: com.mysql.cj.jdbc.Driver
    username: root
    password: 2333
```

‍

‍

### **多记录删除**

deleteBatchIds

```java
int deleteBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

删除（根据ID 批量删除）, 参数是一个集合，可以存放多个id值

‍

‍

#### 示例

根据传入的id集合将数据库表中的数据删除掉

```java
 @Test
    void testDelete(){
        //删除指定多条数据
        List<Long> list = new ArrayList<>();
        list.add(1402551342481838081L);
        list.add(1402553134049501186L);
        list.add(1402553619611430913L);
        userDao.deleteBatchIds(list);
    }
```

‍

‍

### 批量查询

按照id集合进行批量查询

```java
List<T> selectBatchIds(@Param(Constants.COLLECTION) Collection<? extends Serializable> idList);
```

查询（根据ID 批量查询），参数是一个集合，可以存放多个id值

‍

#### 示例

根据传入的ID集合查询用户信息

```java
@Test
    void testGetByIds(){
        //查询指定多条数据
        List<Long> list = new ArrayList<>();
        list.add(1L);
        list.add(3L);
        list.add(4L);
        userDao.selectBatchIds(list);
    }
```

‍

‍

## **自定义SQL**

‍

之前是用代码编写的SQL语句, 在Service层, 不好

因为SQL语句最好都维护在持久层，而不是业务层。就当前案例来说，由于条件是in语句，只能将SQL写在Mapper.xml文件，利用foreach来生成动态SQL

这实在是太麻烦了。假如查询条件更复杂，动态SQL的编写也会更加复杂。

所以，MybatisPlus提供了自定义SQL功能，可以让我们利用Wrapper生成查询条件，再结合Mapper.xml编写SQL

‍

‍

### 示例

```Java
@Test
void testCustomWrapper() {
    // 1.准备自定义查询条件
    List<Long> ids = List.of(1L, 2L, 4L);
    QueryWrapper<User> wrapper = new QueryWrapper<User>().in("id", ids);

    // 2.调用mapper的自定义方法，直接传递Wrapper
    userMapper.deductBalanceByIds(200, wrapper);
}
```

UserMapper>>SQL

```Java
public interface UserMapper extends BaseMapper<User> {
    @Select("UPDATE user SET balance = balance - #{money} ${ew.customSqlSegment}")
    void deductBalanceByIds(@Param("money") int money, @Param("ew") QueryWrapper<User> wrapper);
}
```

这样就省去了编写复杂查询条件的烦恼了

‍

‍

## **Service接口**

‍

通用接口为`IService`​，默认实现为`ServiceImpl`​，其中封装的方法可以分为以下几类：

* ​`save`​：新增
* ​`remove`​：删除
* ​`update`​：更新
* ​`get`​：查询单个结果
* ​`list`​：查询集合结果
* ​`count`​：计数
* ​`page`​：分页查询

‍

### **CRUD**

‍

**新增**：

* ​`save`​    新增单个元素
* ​`saveBatch`​    批量新增
* ​`saveOrUpdate`​    根据id判断，如果数据存在就更新，不存在则新增
* ​`saveOrUpdateBatch`    批量的新增或修改

‍

**删除：**

* ​`removeById`​：根据id删除
* ​`removeByIds`​：根据id批量删除
* ​`removeByMap`​：根据Map中的键值对为条件删除
* ​`remove(Wrapper<T>)`​：根据Wrapper条件删除
* ​`~~removeBatchByIds~~`​：暂不支持?

‍

**修改：**

* ​`updateById`​：根据id修改
* ​`update(Wrapper<T>)`​：根据`UpdateWrapper`​修改，`Wrapper`​中包含`set`​和`where`​部分
* ​`update(T，Wrapper<T>)`​：按照`T`​内的数据修改与`Wrapper`​匹配到的数据
* ​`updateBatchById`​：根据id批量修改

‍

查

* ​`getById`​：根据id查询1条数据
* ​`getOne(Wrapper<T>)`​：根据`Wrapper`​查询1条数据
* ​`getBaseMapper`​：获取`Service`​内的`BaseMapper`​实现，某些时候需要直接调用`Mapper`​内的自定义`SQL`​时可以用这个方法获取到`Mapper`​

‍

* ​`listByIds`​：根据id批量查询
* ​`list(Wrapper<T>)`​：根据Wrapper条件查询多条数据
* ​`list()`​：查询所有

‍

* ​`count()`​：统计所有数量
* ​`count(Wrapper<T>)`​：统计符合`Wrapper`​条件的数据数量

‍

‍

**getBaseMapper**： 当我们在service中要调用Mapper中自定义SQL时，就必须获取service对应的Mapper，就可以通过这个方法：

‍

### Lambda

---

在UserController中定义一个controller

‍

```Java
@GetMapping("/list")
@ApiOperation("根据id集合查询用户")
public List<UserVO> queryUsers(UserQuery query){
    // 1.组织条件
    String username = query.getName();
    Integer status = query.getStatus();

    LambdaQueryWrapper<User> wrapper = new QueryWrapper<User>().lambda()
            .like(username != null, User::getUsername, username)
            .eq(status != null, User::getStatus, status)
            .ge(minBalance != null, User::getBalance, minBalance)
            .le(maxBalance != null, User::getBalance, maxBalance);
    // 2.查询用户
    List<User> users = userService.list(wrapper);
    // 3.处理vo
    return BeanUtil.copyToList(users, UserVO.class);
}
```

‍

在组织查询条件的时候，我们加入了 `username != null`​ 这样的参数，意思就是当条件成立时才会添加这个查询条件，类似Mybatis的mapper.xml文件中的`<if>`​标签。这样就实现了动态查询条件效果了。

不过，上述条件构建的代码太麻烦了。 因此Service中对`LambdaQueryWrapper`​和`LambdaUpdateWrapper`​的用法进一步做了简化。我们无需自己通过`new`​的方式来创建`Wrapper`​，而是直接调用`lambdaQuery`​和`lambdaUpdate`​方法：

‍

‍

基于Lambda查询：

直接用Service.出来lambdaQuery(), 直接链式编程

```Java
@GetMapping("/list")
@ApiOperation("根据id集合查询用户")
public List<UserVO> queryUsers(UserQuery query){
    // 1.组织条件

    // 2.查询用户
    List<User> users = userService.lambdaQuery()
            .like(username != null, User::getUsername, username)
            .eq(status != null, User::getStatus, status)
            .ge(minBalance != null, User::getBalance, minBalance)
            .le(maxBalance != null, User::getBalance, maxBalance)
            .list();
    // 3.处理vo
    return BeanUtil.copyToList(users, UserVO.class);
}
```

可以发现lambdaQuery方法中除了可以构建条件，还需要在链式编程的最后添加一个`list()`​，这是在告诉MP我们的调用结果需要是一个list集合。这里不仅可以用`list()`​，可选的方法有：

* ​`.one()`​：最多1个结果
* ​`.list()`​：返回集合结果
* ​`.count()`​：返回计数结果

MybatisPlus会根据链式编程的最后一个方法来判断最终的返回结果。

与lambdaQuery方法类似，IService中的lambdaUpdate方法可以非常方便的实现复杂更新业务。

‍

‍

‍

‍

## **多表关联**

‍

> 理论上来讲MyBatisPlus是不支持多表查询的; 但是可以用更强的MPJ 进行联表查询(见其他)

基于**自定义SQL**结合**Wrapper**的玩法，就可以利用Wrapper来构建查询条件，然后手写SELECT及FROM部分，实现多表查询

‍

### 示例

查询出所有收货地址在北京的并且用户id在1、2、4之中的用户 要是自己基于mybatis实现SQL

```XML
<select id="queryUserByIdAndAddr" resultType="com.itheima.mp.domain.po.User">
      SELECT *
      FROM user u
      INNER JOIN address a ON u.id = a.user_id
      WHERE u.id
      <foreach collection="ids" separator="," item="id" open="IN (" close=")">
          #{id}
      </foreach>
      AND a.city = #{city}
  </select>
```

可以看出其中最复杂的就是WHERE条件的编写，如果业务复杂一些，这里的SQL会更变态. 

‍

查询

```Java
@Test
void testCustomJoinWrapper() {
    // 1.准备自定义查询条件
    QueryWrapper<User> wrapper = new QueryWrapper<User>()
            .in("u.id", List.of(1L, 2L, 4L))
            .eq("a.city", "北京");

    // 2.调用mapper的自定义方法
    List<User> users = userMapper.queryUserByWrapper(wrapper);

    users.forEach(System.out::println);
}
```

然后在UserMapper中自定义方法

```Java
@Select("SELECT u.* FROM user u INNER JOIN address a ON u.id = a.user_id ${ew.customSqlSegment}")
List<User> queryUserByWrapper(@Param("ew")QueryWrapper<User> wrapper);
```

当然，也可以在`UserMapper.xml`​中写SQL

```XML
<select id="queryUserByIdAndAddr" resultType="com.itheima.mp.domain.po.User">
    SELECT * FROM user u INNER JOIN address a ON u.id = a.user_id ${ew.customSqlSegment}
</select>
```

‍

‍

‍

## 乐观锁

‍

乐观锁主要解决的问题是当要更新一条记录的时候，==希望这条记录没有被别人更新==

‍

> 业务并发现象带来的问题
>
> 假如有100个商品或者票在出售，为了能保证每个商品或者票只能被一个人购买，如何保证不会出现超买或者重复卖
>
> 对于这一类问题，其实有很多的解决方案可以使用
>
> 第一个最先想到的就是锁，锁在一台服务器中是可以解决的，但是如果在多台服务器下锁就没有办法控制，比如12306有两台服务器在进行卖票，在两台服务器上都添加锁的话，那也有可能会导致在同一时刻有两个线程在进行卖票，还是会出现并发问题
>
> 接下来介绍的这种方式是针对于小型企业的解决方案，因为数据库本身的性能就是个瓶颈，如果对其并发量超过2000以上的就需要考虑其他的解决方案了

‍

‍

### 线程分析

1. 数据库表中添加version列，比如默认值给1
2. 第一个线程要修改数据之前，取出记录时，获取当前数据库中的version=1
3. 第二个线程要修改数据之前，取出记录时，获取当前数据库中的version=1
4. 第一个线程执行更新时，set version = newVersion where version = oldVersion

    * newVersion = version+1 [2]
    * oldVersion = version [1]
5. 第二个线程执行更新时，set version = newVersion where version = oldVersion

    * newVersion = version+1 [2]
    * oldVersion = version [1]
6. 假如这两个线程都来更新数据，第一个和第二个线程都可能先执行

    * 假如第一个线程先执行更新，会把version改为2，
    * 第二个线程再更新的时候，set version = 2 where version = 1,此时数据库表的数据version已经为2，所以第二个线程会修改失败
    * 假如第二个线程先执行更新，会把version改为2，
    * 第一个线程再更新的时候，set version = 2 where version = 1,此时数据库表的数据version已经为2，所以第一个线程会修改失败
    * 不管谁先执行都会确保只能有一个线程更新数据，这就是MP提供的乐观锁的实现原理分析.

‍

### 实现流程

‍

#### 添加列

列名可以任意，比如使用`version`​, 给列设置默认值为​1​

‍

‍

#### 模型类添加对应属性

```java
    @Version
    private Integer version;
```

‍

‍

#### 添加拦截器

```java
@Configuration
public class MpConfig {
    @Bean
    public MybatisPlusInterceptor mpInterceptor() {
        //1.定义Mp拦截器
        MybatisPlusInterceptor mpInterceptor = new MybatisPlusInterceptor();
        //2.添加乐观锁拦截器
        mpInterceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
        return mpInterceptor;
    }
}
```

‍

#### 执行更新

‍

需要添加version数据

‍

第一步应该是拿到表中的version，然后拿version当条件在将version加1更新回到数据库表中

‍

#### 查询

在查询的时候，需要对其进行查询

```java
    @Test
    void testUpdate(){
        //1.先通过要修改的数据id将当前数据查询出来
        User user = userDao.selectById(3L);
        //2.将要修改的属性逐一设置进去
        user.setName("Jock888");
...
        userDao.updateById(user);
    }
```

‍

‍

## DB**静态工具**

解决循环依赖问题

> 考虑相互注入的问题, DAO需要Service, Service需要DAO

‍

MybatisPlus提供一个静态工具类`Db`​，其中的一些静态方法与`IService`​中方法签名基本一致，也可以帮助我们实现CRUD功能

‍

‍

### 示例

```Java
@Test
void testDbGet() {
    User user = Db.getById(1L, User.class);
    System.out.println(user);
}

@Test
void testDbList() {
    // 利用Db实现复杂条件查询
    List<User> list = Db.lambdaQuery(User.class)
            .like(User::getUsername, "o")
            .ge(User::getBalance, 1000)
            .list();
    list.forEach(System.out::println);
}

@Test
void testDbUpdate() {
    Db.lambdaUpdate(User.class)
            .set(User::getBalance, 2000)
            .eq(User::getUsername, "Rose");
}
```

‍

‍

#### 需求

改造根据id用户查询的接口，查询用户的同时返回用户收货地址列表

‍

#### VO修改

添加一个收货地址的VO对象

改造原来的UserVO，添加一个地址属性

‍

#### **UserServiceImpl**

Db.lambdaQuery(Address.class)

采用了Db的静态方法，因此避免了注入AddressService，减少了循环依赖的风险

```Java
@Override
public UserVO queryUserAndAddressById(Long userId) {

    // 1.查询用户
    User user = getById(userId);
    if (user == null) {
        return null;
    }

    // 2.查询收货地址
    List<Address> addresses = Db.lambdaQuery(Address.class)
            .eq(Address::getUserId, userId)
            .list();

    // 3.处理vo
    UserVO userVO = BeanUtil.copyProperties(user, UserVO.class);
    userVO.setAddresses(BeanUtil.copyToList(addresses, AddressVO.class));
    return userVO;
}
```

‍

‍

‍

## 转换**器**

‍

### **枚举处理器**

> 问题情境 -- User类中的一个用户状态字段
>
> 这种字段我们一般会定义一个枚举，做业务判断的时候就可以直接基于枚举做比较. 但是我们数据库采用的是`int`​类型，对应的PO也是`Integer`​. 因此业务操作时必须手动把`枚举`​与`Integer`​转换，非常麻烦.

MybatisPlus提供了一个处理枚举的类型转换器，可以帮我们把**枚举类型**与数据库类型自动转换. 

‍

#### 示例

‍

定义一个用户状态的枚举

enums->UserStatus

```Java
@Getter
public enum UserStatus {
    NORMAL(1, "正常"),
    FREEZE(2, "冻结")
    ;
    private final int value;
    private final String desc;

    UserStatus(int value, String desc) {
        this.value = value;
        this.desc = desc;
    }
}
```

然后把`User`​类中的`status`​字段改为`UserStatus`​ 类型

‍

要让`MybatisPlus`​处理枚举与数据库类型自动转换，我们必须告诉`MybatisPlus`​，枚举中的哪个字段的值作为数据库值.  `MybatisPlus`​提供了`@EnumValue`​注解来标记枚举属性

‍

​`@EnumValue`​

‍

**配置枚举处理器**

application.yaml

```YAML
mybatis-plus:
  configuration:
    default-enum-type-handler: com.baomidou.mybatisplus.core.handlers.MybatisEnumTypeHandler
```

‍

**测试**

```Java
@Test
void testService() {
    List<User> list = userService.list();
    list.forEach(System.out::println);
}
```

最终，查询出的`User`​类的`status`​字段会是枚举类型

‍

同时，为了使页面查询结果也是枚举格式，要修改UserVO中的status属性

‍

​`UserStatus status`​

‍

并且，在UserStatus枚举中通过`@JsonValue`​注解标记JSON序列化时展示的字段

‍

‍

### **JSON处理器**

> 数据库的user表中有一个`info`​字段，是JSON类型, 而目前`User`​实体类中却是`String`​类型
>
> 这样一来，我们要读取info中的属性时就非常不方便. 如果要方便获取，info的类型最好是一个`Map`​或者实体类. 
>
> 而一旦我们把`info`​改为`对象`​类型，就需要在写入数据库时手动转为`String`​，再读取数据库时，手动转换为`对象`​，这会非常麻烦

MybatisPlus提供了很多特殊类型字段的类型处理器，解决特殊字段类型与数据库类型转换的问题. 例如处理JSON就可以使用`JacksonTypeHandler`​处理器. 

‍

#### 示例

‍

**定义实体**

定义一个单独实体类来与info字段的属性匹配： domain.po -> UserInfo

```Java
package com.itheima.mp.domain.po;
@Data
public class UserInfo {
    private Integer age;
    private String intro;
    private String gender;
}
```

‍

**类型处理器**

将User类的info字段修改为UserInfo类型，并声明类型处理器

```java
//详细信息
@TableField(typeHandler = JacksonTypeHandler.class)
private UserInfo info;
```

记得把User上面TableName的自动映射打开, 实现相互转换

```java
@TableName (value= "user". autoResultMap = true )
```

同时，为了让页面返回的结果也以对象格式返回，我们要修改UserVO中的info字段

```java

@ApiModelProperty("详细信息")
private UserInfo info;
```

‍

‍

‍

## **分页**插件

​`MybatisPlus`​是不支持分页功能的，`IService`​和`BaseMapper`​中的分页方法都无法正常起效

> MP的分页看来非常漂亮; 使用多个分页插件的时候需要注意插件定义顺序，建议使用顺序

‍

‍

### **配置**

配置类

‍

```Java

@Configuration
public class MybatisConfig {

    @Bean
    public MybatisPlusInterceptor mybatisPlusInterceptor() {
        // 初始化核心插件
        MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();

        //设置参数(需要创建对象)
        pageInterceptor.setMaxLimit(1000L) //分页上限

        // 直接new添加分页插件
        interceptor.addInnerInterceptor(new PaginationInnerInterceptor(DbType.MYSQL));
  
        return interceptor;
    }
}
```

‍

‍

### **分页API**

‍

#### 示例

‍

这里用到了分页参数

```Java
    @Test
    void testPageQuery() {
        // 1.分页查询，new Page()的两个参数分别是：页码、每页大小
        Page<User> p = userService.page(new Page<>(2, 2));
  
        // 2.总条数
        System.out.println("total = " + p.getTotal());
  
        // 3.总页数
        System.out.println("pages = " + p.getPages());
  
        // 4.数据
        List<User> records = p.getRecords();
        records.forEach(System.out::println);
    }
```

‍

Page即可以支持分页参数，也可以支持排序参数

```Java
    int pageNo = 1, pageSize = 5;
  
    // 分页参数
    Page<User> page = Page.of(pageNo, pageSize);
  
    // 排序参数, 通过OrderItem来指定, 可以设置多条
    page.addOrder(new OrderItem("balance", false));
  
    //可以用p接
    Page<User> p = userService.page(page);
  
    //解析输出
    long total = p.getTotal();
    sout -> total

    long pages = p.getPages();
    sout -> pages

    List<User> users = p.getRecords();
    users.forEach(System.out::println);
```

‍

‍

### **通用分页实体**

‍

* PageQuery
* DTO
* VO

> 例如要实现一个用户分页查询的接口
>
> 需要定义3个实体
>
> * ​`UserQuery`​：分页查询条件的实体，包含分页、排序参数、过滤条件
> * ​`PageDTO`​：分页结果实体，包含总条数、总页数、当前页数据
> * ​`UserVO`​：用户页面视图实体

‍

‍

‍

#### **PageQuery**

分页条件不仅仅用户分页查询需要，以后其它业务也都有分页查询的需求, 因此建议将分页查询条件单独定义为一个`PageQuery`​实体, 然后让自己的UserQuery继承这个实体

​`PageQuery`​是前端提交的查询参数，一般包含四个属性

* ​`pageNo  `​页码
* ​`pageSize`​  每页数据条数
* ​`sortBy`​  排序字段
* ​`isAsc`​  是否升序

‍

#### **PageDTO**

分页实体

‍

#### UserVO

返回值的用户实体

‍

‍

#### 示例

‍

##### **UserQuery**

```Java
package com.itheima.mp.domain.query;

@Data
@ApiModel(description = "用户查询条件实体")
public class UserQuery {
    @ApiModelProperty("用户名关键字")
    private String name;
    @ApiModelProperty("用户状态：1-正常，2-冻结")
    private Integer status;
    @ApiModelProperty("余额最小值")
    private Integer minBalance;
    @ApiModelProperty("余额最大值")
    private Integer maxBalance;
}
```

‍

##### **PageQuery**

domain.query -> **PageQuery**

```Java
@Data
@ApiModel(description = "分页查询实体")
public class PageQuery {
    @ApiModelProperty("页码")
    private Integer pageNo;
    @ApiModelProperty("页码")
    private Integer pageSize;
    @ApiModelProperty("排序字段")
    private String sortBy;
    @ApiModelProperty("是否升序")
    private Boolean isAsc;
}
```

UserQuery

```Java
package com.itheima.mp.domain.query;

@EqualsAndHashCode(callSuper = true)
@Data
@ApiModel(description = "用户查询条件实体")
public class UserQuery extends PageQuery {
    @ApiModelProperty("用户名关键字")
    private String name;
    @ApiModelProperty("用户状态：1-正常，2-冻结")
    private Integer status;
    @ApiModelProperty("余额最小值")
    private Integer minBalance;
    @ApiModelProperty("余额最大值")
    private Integer maxBalance;
}
```

‍

##### UserVO

返回值的用户实体

domain.vo -> UserVO

‍

##### **PageDTO**

分页实体

domain.dto -> PageDTO

```Java
package com.itheima.mp.domain.dto;
@Data
@ApiModel(description = "分页结果")
public class PageDTO<T> {
    @ApiModelProperty("总条数")
    private Long total;
    @ApiModelProperty("总页数")
    private Long pages;
    @ApiModelProperty("集合")
    private List<T> list;
}
```

‍

‍

### **接口开发**

‍

#### ​​Controller​

​`UserController`​中定义分页查询用户的接口

```Java

@RestController
@RequestMapping("users")
@RequiredArgsConstructor
public class UserController {

    private final UserService userService;

    @GetMapping("/page")
    public PageDTO<UserVO> queryUsersPage(UserQuery query){
        return userService.queryUsersPage(query);
    }

    // 。。。 略
}
```

‍

#### Service

‍

​​IUserService​中创建​queryUsersPage​方法

```Java
PageDTO<UserVO> queryUsersPage(PageQuery query);
```

‍

UserServiceImpl中实现该方法

```Java
@Override
public PageDTO<UserVO> queryUsersPage(PageQuery query) {
    // 1.构建条件
    // 1.1.分页条件
    Page<User> page = Page.of(query.getPageNo(), query.getPageSize());
    // 1.2.排序条件
    if (query.getSortBy() != null) {
        page.addOrder(new OrderItem(query.getSortBy(), query.getIsAsc()));
    }else{
        // 默认按照更新时间排序
        page.addOrder(new OrderItem("update_time", false));
    }
    // 2.查询
    page(page);
    // 3.数据非空校验
    List<User> records = page.getRecords();
    if (records == null || records.size() <= 0) {
        // 无数据，返回空结果
        return new PageDTO<>(page.getTotal(), page.getPages(), Collections.emptyList());
    }
    // 4.有数据，转换
    List<UserVO> list = BeanUtil.copyToList(records, UserVO.class);
    // 5.封装返回
    return new PageDTO<UserVO>(page.getTotal(), page.getPages(), list);
}
```

‍

‍

### 封装工具优化

‍

#### **改造PageQuery实体**

在PageQuery这个实体中定义一个工具方法, 这样开发也时就省去对从​PageQuery​到​Page​的的转换

```Java
@Data
public class PageQuery {
.....

    public <T>  Page<T> toMpPage(OrderItem ... orders){
        // 1.分页条件
        Page<T> p = Page.of(pageNo, pageSize);
        // 2.排序条件
        // 2.1.先看前端有没有传排序字段
        //或是这样: StrUtil.isNotBlank(sortBy)

        if (sortBy != null) {
            p.addOrder(new OrderItem(sortBy, isAsc));
            return p;
        }
        // 2.2.再看有没有手动指定排序字段
        if(orders != null){
            p.addOrder(orders);
        }
        return p;
    }

    public <T> Page<T> toMpPage(String defaultSortBy, boolean isAsc){
        return this.toMpPage(new OrderItem(defaultSortBy, isAsc));
    }

    public <T> Page<T> toMpPageDefaultSortByCreateTimeDesc() {
        return toMpPage("create_time", false);
    }

    public <T> Page<T> toMpPageDefaultSortByUpdateTimeDesc() {
        return toMpPage("update_time", false);
    }
}
```

‍

```Java
// 1.构建条件
Page<User> page = query.toMpPageDefaultSortByCreateTimeDesc();
```

‍

‍

#### **改造PageDTO实体**

‍

在查询出分页结果后，数据的非空校验，数据的vo转换都是模板代码，编写起来很麻烦; 完全可以将其封装到PageDTO的工具方法中，简化整个过程

```Java
package com.itheima.mp.domain.dto;

@Data
@NoArgsConstructor
@AllArgsConstructor
public class PageDTO<V> {
    private Long total;
    private Long pages;
    private List<V> list;

    /**
     * 返回空分页结果
     * @param p MybatisPlus的分页结果
     * @param <V> 目标VO类型
     * @param <P> 原始PO类型
     * @return VO的分页对象
     */
    public static <V, P> PageDTO<V> empty(Page<P> p){
        return new PageDTO<>(p.getTotal(), p.getPages(), Collections.emptyList());
    }

    /**
     * 将MybatisPlus分页结果转为 VO分页结果
     * @param p MybatisPlus的分页结果
     * @param voClass 目标VO类型的字节码
     * @param <V> 目标VO类型
     * @param <P> 原始PO类型
     * @return VO的分页对象
     */
    public static <V, P> PageDTO<V> of(Page<P> p, Class<V> voClass) {
        // 1.非空校验
        List<P> records = p.getRecords();
        if (records == null || records.size() <= 0) {
            // 无数据，返回空结果
            return empty(p);
        }
        // 2.数据转换
        List<V> vos = BeanUtil.copyToList(records, voClass);
        // 3.封装返回
        return new PageDTO<>(p.getTotal(), p.getPages(), vos);
    }

    /**
     * 将MybatisPlus分页结果转为 VO分页结果，允许用户自定义PO到VO的转换方式
     * @param p MybatisPlus的分页结果
     * @param convertor PO到VO的转换函数
     * @param <V> 目标VO类型
     * @param <P> 原始PO类型
     * @return VO的分页对象
     */
    public static <V, P> PageDTO<V> of(Page<P> p, Function<P, V> convertor) {
        // 1.非空校验
        List<P> records = p.getRecords();
        if (records == null || records.size() <= 0) {
            // 无数据，返回空结果
            return empty(p);
        }
        // 2.数据转换
        List<V> vos = records.stream().map(convertor).collect(Collectors.toList());
        // 3.封装返回
        return new PageDTO<>(p.getTotal(), p.getPages(), vos);
    }
}
```

‍

#### 效果

‍

最终，业务层的代码可以简化为

```Java
@Override
public PageDTO<UserVO> queryUserByPage(PageQuery query) {

    // 1.构建条件
    Page<User> page = query.toMpPageDefaultSortByCreateTimeDesc();

    // 2.查询
    page(page);

    // 3.封装返回
    return PageDTO.of(page, UserVO.class);
}
```

‍

如果是希望自定义PO到VO的转换过程 ->

```Java
@Override
public PageDTO<UserVO> queryUserByPage(PageQuery query) {
    // 1.构建条件
    Page<User> page = query.toMpPageDefaultSortByCreateTimeDesc();
    // 2.查询
    page(page);
    // 3.封装返回
    return PageDTO.of(page, user -> {
        // 拷贝属性到VO
        UserVO vo = BeanUtil.copyProperties(user, UserVO.class);
        // 用户名脱敏
        String username = vo.getUsername();
        vo.setUsername(username.substring(0, username.length() - 2) + "**");
        return vo;
    });
}
```

‍

> 这里的反射和泛型可以说是教科书类型的封装定义方法了, 十分好

‍

‍

## **代码生成**插件

MP 插件(就是这个名字)

基于图形化界面完成​MybatisPlus​的代码生成

‍

**示例**

数据库中有一张address表尚未生成对应的实体和mapper等基础代码

‍

1. 配置数据库地址，在Idea顶部菜单中，找到`other`​，选择`Config Database`​; 填写数据库连接的基本信息
2. 再次点击Idea顶部菜单中的other，然后选择`Code Generator`​ 代码生成器, 在弹出的表单中填写信息
3. 指定需要生成代码的表, 指定实体类等包的名称, 自选额外选项

​ 

‍

## 插件市场

‍

MybatisPlus提供了很多的插件功能，进一步拓展其功能。目前已有的插件有：

* ​`PaginationInnerInterceptor`​：自动分页
* ​`TenantLineInnerInterceptor`​：多租户
* ​`DynamicTableNameInnerInterceptor`​：动态表名
* ​`OptimisticLockerInnerInterceptor`​：乐观锁
* ​`IllegalSQLInnerInterceptor`​：sql 性能规范
* ​`BlockAttackInnerInterceptor`​：防止全表更新与删除

使用多个插件的时候需要注意插件定义顺序，建议使用顺序如下：

* 多租户,动态表名
* 分页,乐观锁
* sql 性能规范,防止全表更新与删除

插件添加步骤类似 -分页插件

> MybatisConfig里面加MybatisPlusInterceptor, 里面在核心插件里面加上自定义插件

‍

‍

‍

### 性能分析拦截器插件

PerformanceInterceptor

‍

用于分析 SQL 性能的拦截器。它记录每个 SQL 语句的执行时间，这有助于识别和优化慢查询

‍

工作原理

1. Bean 定义：在 Spring 配置类 MybatisPlusConfig 中定义 PerformanceInterceptor Bean，使其可用于 Spring 上下文。
2. 拦截器注册：应用启动时，Spring Boot 自动将 PerformanceInterceptor 注册为 MyBatis 的拦截器。
3. SQL 执行：在执行 SQL 语句期间，PerformanceInterceptor 拦截调用。
4. 日志记录：拦截器记录每个 SQL 语句的执行时间。如果执行时间超过某个阈值，它可以记录警告或错误信息

```java
@Configuration
@MapperScan(basePackages = {"com.xkcoding.orm.mybatis.plus.mapper"})
@EnableTransactionManagement
public class MybatisPlusConfig {
    /**
     * 性能分析拦截器，不建议生产使用
     */
    @Bean
    public PerformanceInterceptor performanceInterceptor() {
        return new PerformanceInterceptor();
    }
```

‍

‍

‍

‍

# 实操

‍

## 热部署

‍

热部署配置

```java
mybatis-plus:
  mapper-locations: classpath:mappers/*.xml
  #实体扫描，多个package用逗号或者分号分隔
  typeAliasesPackage: com.xkcoding.orm.mybatis.plus.entity

  global-config:
    # 数据库相关配置
    db-config:
      #主键类型  AUTO:"数据库ID自增", INPUT:"用户输入ID",ID_WORKER:"全局唯一ID (数字类型唯一ID)", UUID:"全局唯一ID UUID";
      id-type: auto
      #字段策略 IGNORED:"忽略判断",NOT_NULL:"非 NULL 判断"),NOT_EMPTY:"非空判断"
      field-strategy: not_empty
      #驼峰下划线转换
      table-underline: true
      #是否开启大写命名，默认不开启
      #capital-mode: true
      #逻辑删除配置
      #logic-delete-value: 1
      #logic-not-delete-value: 0
      db-type: mysql
    #刷新mapper 调试神器
    refresh: true

  # 原生配置
  configuration:
    map-underscore-to-camel-case: true
    cache-enabled: true
```

‍

这里启用 MyBatis 映射器的自动刷新。允许在不重新启动应用程序的情况下获取映射 XML 文件的更改

```java
#刷新mapper 调试神器
    refresh: true
```

‍

‍

‍

‍

## BUGFIX

‍

‍

### 无法使字段私有化

CMD MSG

```java
Unable to make field private final java.lang.Class java.lang.invoke
```

‍

从java9开始就依赖模块化了，目前mybatis-plus有用到的几个lib是没有模块化的，mybatis-plus要完美支持是很难的. 但并不是没有办法规避. 对jdk来说，没有命名模块的都是unnamed模块. 错误提示已经很明确了，可以在启动参数添加 `--add-opens=java.base/java.lang.invoke=ALL-UNNAMED`​ 来规避

‍

1. 编辑运行配置(运行-运行配置)
2. 修改参数 Modify Options(**Alt+M**)
3. 选择 Add VM options(**Alt+V**)
4. 加入参数 --add-opens java.base/java.lang.invoke=ALL-UNNAMED

‍

‍

### 循环依赖问题

‍

> [Link](https://www.zhihu.com/question/314745062)
>
> 感觉mp挺好用的，不过mp有点屏蔽掉mapper层，代码架构设计不合适，经常出现循环依赖的问题，顺便请教一下，大家遇到过么，有没有什么合适的解决方式
>
>  --是的，有时候比如按条件查询一类数据，有了mp就会在service中用wrapper 实现，这样增加了好多service 间的调用，感觉有时会出现循环依赖问题，像你说的，可能就是前期模块规划的不合理吧
>
>  --循环依赖只会发生在service之间.mapper之间是不会发生的.所以底层CRUD只用mapper操作数据库,中间不要经过service，避免不必要的扩大范围.存在业务逻辑的service方法.需要模块化,明确业务范围.对于范围无法明确的需要提出来做关联聚合.  
> 另外，最重要的是不要过度设计.

‍

### MP or M? 建议

‍

> [Link](https://www.zhihu.com/question/314745062/answer/3282783134)
>
> 大部分接触到MP的人都有几个误区
>
> 1，用了MP就不应该用mybatis了
>
> 2，用了MP，service层就必须继承BaseService，造成sql操作侵入service层
>
> 3，用了MP，就必须写QueryWrapper或LambdaQueryWrapper
>
> 其实，
>
> 1，mybatis-plus是用来增强mybatis的，两者要同时使用
>
> 2，如果不希望mybatis-plus侵入service层，就在团队里约定好service层不要继承BaseService就行了
>
> 3，基础的单表操作用BaseMapper中封装好的方法，BaseMapper中没有的简单的单表操作可以写QueryWrapper或LambdaQueryWrapper等，而且可以在Mapper接口中写default方法，不用创建实现类。另外，复杂的单表操作和多表联查依然写到mybatis的xml里
>
> 这才是mybatis-plus的最佳实践

‍
