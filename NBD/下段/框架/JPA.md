--Java持久化层标准规范

‍

JPA(Java Persistence API)是一个基于O/R映射(Object Relational Mapping) -- **对象关系映射**的标准规范

主要实现框架有Hibernate、EclipseLink 和OpenJPA 等

JPA通过JDK 5.0注解或XML描述对象－关系表的映射关系，并将运行期的实体对象持久化到数据库中

‍

### Header

‍

‍

# 知识

‍

‍

## 功能内容

* ORM映射元数据：JPA支持XML和JDK 5.0注解两种元数据的形式，元数据描述对象和表之间的映射关系，框架据此将实体对象持久化到数据库表中；
* JPA 的API：定义规范，以操作实体对象，执行CRUD操作，框架在后台替我们完成所有的事情，开发者从繁琐的JDBC和SQL代码中解脱出来.
* 查询语言：通过面向对象而非面向数据库的查询语言查询数据，避免程序的SQL语句紧密耦合. 定义JPQL和Criteria两种查询方式.

‍

‍

## 评价

Java Persistence API, 轻量级，部分中小项目适合

‍

# 基础

‍

## 搭建

‍

### 依赖

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

‍

### 配置

‍

hibernate

```yml
#jpa相关配置                                                                             
  jpa:                                                                             
    hibernate:                                                                     
      #DDL:用于定义数据库的三层结构，包括外模式、概念模式、内模式及其相互之间的映像，定义数据的完整性，安全控制等约束                     
      ddl-auto: none #什么也不做                                                          
      #其他可选值                                                                         
      #create: 每次运行应用程序时，都会重新创建表，所以，数据都会丢失                                           
      #create-drop:每次运行程序时会创建表结构，然后程序结束时清空数据                                         
      #update: 每次运行程序没有表时会创建表，如果对象改变会更新表结构，原有数据不会清除，只会更新                             
      #validate: 运行程序会校验数据与数据库的字段类型是否相同，字段不同会报错                                      
    #打印执行的sql及参数                                                                     
    show-sql: true                                                                 
    # 关闭懒加载配置，否则会报错                                                                  
    open-in-view: false                                                            
    properties:                                                                    
      hibernate:                                                                   
        #输出sql语句                                                                     
        show_sql: true                                                             
        #格式化输出的sql，否则会一行显示                                                           
        format_sql: true                                                           
```

‍

### **实体类映射**

‍

#### 基本注解

‍

##### @Entity

‍

标志为一个JPA的实体类

‍

##### @Getter、@Setter

‍

表示设置实体类属性的Getter、Setter方法

‍

##### @Id

标志数据库表的主键字段. 

‍

‍

##### @GeneratedValue(strategy = GenerationType.XXX)

指定主键的生成策略

‍

‍

**示例**

创建Banner对应的实体类映射数据库中的字段

```java
    @Entity
    @Getter
    @Setter
    public class Banner{
        @Id
        @GeneratedValue(strategy = GenerationType.IDENTITY)
        private Long id;
        private String name;
        private String description;
        @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
        private Date createTime;
        @JsonFormat(pattern = "yyyy-MM-dd HH:mm:ss")
        private Date modifyTime;
        private Integer deleted;
        private Integer enable;
        private Long creator;
    }
```

‍

### **Repository接口**

只需要定义一个接口就可以轻松地操作数据库，这个接口要继承JpaRepository接口

需要指定两个泛型: 第一个泛型是实体类的类型，第二个泛型是主键的类型

```java
public interface BannerRepository extends JpaRepository<Banner, Long> {
}
```

‍

JpaRepository继承关系

‍

> JpaRepository继承自PagingAndSortingRepository，PagingAndSortingRepository则继承自CrudRepository，CrudRepository继承自Repository接口，扩展了Repository的接口，并添加了一些其他的功能. 
>
> 查看Repository源码，可以看到Repository接口中没有任何的实现，但是这个接口是JPA的核心接口，实现了该接口的类，都会被Spring容器识别为一个Repository Bean，放到IOC容器中，进而可以定义一些满足一定规范的方法. 
>
> CrudRepository实现了一组CRUD的相关方法，PagingAndSortingRepository实现了排序和分页的相关功能，我们可以自定义一些自定义的Repository，作为通用的Repository，具体选择哪个父接口，要根据业务需求来进行选择，如：有时可能不想暴露一些接口，如可能不想对外暴露删除的方法，这时可以把想要的方法复制到Repository中，来避免暴露不必要的方法.

‍

‍

## CRUD

### 增

‍

### 删

‍

### 改

‍

‍

### 查

‍

#### **简单查询**

‍

```java
public interface BannerRepository extends JpaRepository<Banner, Long> {

    /**
     * 查询banner 通过bannerId
     *
     * @param bannerId bannerId
     * @return banner 详细信息
     */
    Optional<Banner> findBannerById(Long bannerId);

}
```

```java
    @Test
    public void testFindBannerById() {
        Optional<Banner> bannerInfo = bannerRepository.findBannerById(1L);

        log.info("banner exists: {}, banner info: {}", bannerInfo.isPresent(), JSON.toJSONString(bannerInfo.get()));
    }
```

‍

简单查询方法，只要**符合JPA规范就不需要写SQL语句**，简单条件查询：查询方法 find | read | get |query | stream开头，它们都是同义词没有任何区别. 

删除方法如delete | remove开头的方法也没有区别

‍

‍

#### **多条件查询**

如果通过多个条件进行查询，如通过名称和描述进行查询，JPA也支持这种查询方式，我们只需要将条件用and进行连接，这里的条件的属性名称与个数要与参数的位置和个数一一对应. 

```java
Banner findBannerByNameAndDescription(String name, String description);
```

```java
    @Test
    public void testFindBannerByNameAndDescription() {
        Banner bannerInfo = bannerRepository.findBannerByNameAndDescription("首页banner", "首页顶部展示banner");

        log.info("banner info: {}", JSON.toJSONString(bannerInfo));
    }
```

JPA几乎实现了MySQL所有的查询关键字，第一个查询是通过Equals关键字来查询名称相同和价格相等的商品，第二个查询是通过GreaterThanEqual关键字来查询名称相同价格大于等于给定价格的商品. 

‍

‍

#### **排序与分页**

查询方法的入参中加入Sort对象作为入参

将Pageable作为入参就可以实现分页功能

```java
List<Goods> findAll(Sort sort);
```

可以使用Sort.by静态方法来快速构建Sort对象，并且可以传入多个Sort对象

```java
@Test
public void testSortFindAll() {
  log.info("{}", JSON.toJSONString(goodsRepository.findAll(Sort.by(Sort.Order.desc("price"), (Sort.Order.asc("id"))))));
}
```

先按照价格降序排列，再按照id升序排列

‍

‍

#### **自定义JPQL**

如果JPA规范定义的查询关键字不能满足需求的话，就可以使用@Query自定义查询的JPQL

‍

查询最大id的商品信息，nativeQuery属性表示，是否使用原生SQL，目前使用的JPQL并不是原生的，该属性默认是false，默认可以不写就是使用JPQL查询，这里要注意的是查询使用的表名是实体类，而不是数据表. 

```java
@Query(value = "SELECT g from Goods g WHERE id = (SELECT max(id) FROM Goods)", nativeQuery = false)
Goods getMaxIdGoods();  
```

```java
@Test
public void testGetMaxIdGoods() {
  log.info("{}", JSON.toJSONString(goodsRepository.getMaxIdGoods()));
}
```

‍

#### **简化更新**

默认提供了一个save方法用来保存或更新数据，但是这个方法的缺点是会更新所有字段的值

可以通过JPQL的方式来自定义更新只更新我们想更新的字段即可，就能更加高效的执行更新操作

‍

首先使用@Query来编写更新的JPQL，使用@Modifying注解来标志是一个更新操作，JPA中的其他查询方法默认都是只读事务，我们需要使用@Transactional注解来标志当前方法不是只读事务，readOnly属性可以不写默认就是非只读事务. 

```java
@Query("UPDATE Goods g SET g.price = :price WHERE id = :goodsId")
@Modifying
@Transactional(readOnly = false)
int updateGoodsPrice(@Param("goodsId") Long goodsId, @Param("price") BigDecimal price);
```

```java
@Test
public void testUpdateGoodsPrice() {
        goodsRepository.updateGoodsPrice(1L, BigDecimal.valueOf(23000));
}
```

‍

‍

#### **原生查询**

使用原生SQL来进行查询: nativeQuery用来标志当前查询是一条原生SQL

```java
@Query(value = "SELECT * FROM goods", nativeQuery = true)
List<Goods> findAllNativeQuery();
```

如果想要拿到原生查询的部分结果就需要使用Map来进行接收

```java
@Query(value = "SELECT name, price FROM goods", nativeQuery = true)
List<Map<String, Object>> getGoodsNameAndPriceNativeQuery();
```

‍

‍

## 注解

‍

### \@Column

```java
@Column(name="banner_id")
```

实体和数据表列不同名的时候才有用，如数据库字段banner_id，实体类字段id，这时就要使用@Column注解来标志数据库字段名称

‍

属性

name：数据库表中的字段名称

nullable：该字段是否允许为null，默认是true. 

unique：字段值是否唯一，默认是false

length：字段的大小，仅仅对String类型的字段有效. 

以上的限制，都是在JPA层面进行拦截限制，而不是在数据库层面

‍

### @Basic

标志是数据库表的列，这个注解会默认加载字段上，如果数据表没有这个字段将会报错. 

‍

### @Transient

如果想忽略实体类的某一个字段时，可以使用这个注解，JPA就不会将这个字段作为数据表的列. 

‍

‍

## 实体生命周期

‍

### 实体状态

1. New，新创建的实体对象，没有主键(identity)值
2. Managed，对象处于Persistence Context(持久化上下文）中，被EntityManager管理
3. Detached，对象已经游离到Persistence Context之外，进入Application Domain
4. Removed, 实体对象被删除

  

EntityManager提供一系列的方法管理实体对象的生命周期，包括：

‍

1. persist, 将新创建的或已删除的实体转变为Managed状态，数据存入数据库.
2. remove，删除受控实体
3. merge，将游离实体转变为Managed状态，数据存入数据库.

‍

如果使用了事务管理，则事务的commit/rollback也会改变实体的状态. 

‍

‍

## 主键生成策略

‍

* IDENTITY    主键自增，由数据库进行生成
* TABLE    使用一个特定的数据库表去保存主键
* SEQUENCE    根据底层数据库序列生成主键
* AUTO    由程序控制，是GenerationType 的默认值

‍

‍

# 实操

‍

‍

‍

‍

‍

‍
