--对象关系映射框架

‍

全自动的ORM框架，能够自动生成的SQL语句并自动执行，在Java对象与关系数据库之间建立某种映射，以实现直接存取Java对象; 是实现了JPA规范的一种ORM框架，但是，JPA规范的实现仅仅是Hibernate的一部分

‍

如果代码使用纯JPA标准编写，那么不修改java代码代码，而只需修改一下配置文件即可将HIbernate的代码移植到其他的JPA框架（OpenJPA、EclipseTOP）

Hibernate是这么多JPA框架里面，性能最好的一个

Hibernate是实现了JPA规范的一种ORM框架，但是，JPA规范的实现仅仅是Hibernate的一部分

‍

‍

‍

‍

## Header

‍

# 知识

‍

## 概念

> Q: Hibernate使用JPA规范解决了什么问题
>
> A: 使用注解来替代配置文件, 将程序的元数据(程序必须依赖的数据)写在代码上  
>     配置文件不是编程语言的语法，无法断点调试  
>     注解是Java语法，报错的时候可以快速的定位问题

‍

> ​Hibernate兼容性支持JPA规范就是操作的接口是Hibernate原来指定的，只有映射注解使用JPA标准接口提供的

‍

> Hibernate完全性地支持JPA标准就是操作框架的API和映射注解都全部使用JPA的标准来实现.   
> 意思就是不使用Hibernate原来的接口和类，而使用由JPA标准规范制定的类和接口来实现. 那么入门Hibernate-JPA，只需知道JPA规范使用了哪些类名和接口名来替代原来Hibernate的操作类和接口即可

‍

## 评价

比较笨重，sql调优麻烦

‍

# 基础

## **注解开发**

‍

### 原型

‍

将一个原来使用xml配置的入门示例修改使用注解实现，仅仅将映射文件修改为注解，其他不变

**删除掉XML映射文件**

‍

#### 映射注解

将映射的注解写在实体类里面

```java

@Entity     // @Entity注解，指定该实体类是一个基于JPA规范的实体类
@Table(name = "tb_student")    // @Table注解，指定当前实体类关联的表
public class Student {

    // @Id注解，声明属性为一个ID属性
    @Id

    // @GeneratedValue注解，指定主键生成策略
    @GeneratedValue(strategy = GenerationType.IDENTITY)

    // @Column注解，设置属性关联的数据库表字段
    // 注意：如果属性名和表字段名相同，可以不设置
    @Column(name = "student_id")
    private Integer studentId;
    @Column(name = "student_name")
    private String studentName;
    @Column(name = "student_pwd")
    private String studentPwd;
    @Column(name = "student_status")
    private String studentStatus;
    @Column(name = "create_date")
    private Date createDate;

    // get、set方法
}
```

‍

#### **修改配置**

修改总配置文件hibernate.cfg.xml

**加载的是映射的实体类**

```xml
<?xml version='1.0' encoding='utf-8'?>
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>

        <!-- 配置DB连接4要素 -->
        <property name="hibernate.connection.driver_class">com.mysql.cj.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://localhost:3306/hibernate?serverTimezone=UTC</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">2333</property>

        <!-- 配置映射实体类 -->
        <mapping class="org.brick.pojo.Student" />

    </session-factory>
</hibernate-configuration>
```

‍

#### **测试**

```java
@Test
public void testSave() {
    // [1] 获取操作对象
    Session session = HibernateUtil.openSession();
    // [2] 获取事务对象
    Transaction transaction = session.getTransaction();
    try {
        // [2.1] 启动事务
        transaction.begin();

        // [3] 插入
        Student student = new Student();
        student.setStudentName("张三");
        student.setStudentPwd("zhangsan");
        student.setStudentStatus(1);
        student.setCreateDate(new Date());
        session.save(student);

        // [4] 提交事务
        transaction.commit();
    } catch (Exception e) {
        e.printStackTrace();
        // [2.2] 回滚事务
        transaction.rollback();
    } finally {
        // [5] 关闭资源
        session.close();
    }
}
```

‍

‍

‍

‍

‍

‍

# 高级

‍

‍

# 实操

‍
