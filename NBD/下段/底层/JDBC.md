--Java数据库访问技术

‍

JDBC(Java DataBase Connectivity), Java数据库连接，是一套用于执行SQL语句的Java API，由一组用Java语言编写的类和接口组成，是Java程序访问数据库的标准规范. 

java语言操作数据库**从根本上来说**只能通过一种方式：使用sun公司提供的JDBC规范, 就是使用Java语言操作关系型数据库的一套API. JDBC是一套**接口规范**, 在Java的标准库`java.sql`​里. 大部分都是接口. 接口并不能直接实例化，而是必须实例化对应的实现类，然后通过接口引用这个实例

‍

‍

### Header

‍

# 知识

  

## 概念

‍

### 要素

‍

Connection连接

特定数据库的连接（会话），在连接上下文中执行sql语句并返回结果

‍

Statement 语句

创建执行SQL语句的statement, 有好几种实现类，用于执行对应的sql

‍

ResultSet结果集

SQL查询返回的结果信息

‍

‍

## 原理

JDBC驱动    意味着某个数据库实现了JDBC接口的jar包

‍

## 评价

**==问题==**

1. 数据库链接的四要素(驱动、链接、用户名、密码)全部**硬编码**在java代码中
2. 查询结果的解析及封装非常繁琐
3. 每一次查询都需要获取连接,操作完毕后释放连接, 资源浪费, 性能降低 ->连接池解决

‍

## 架构

在现实中常常推荐三层模型，方便开发和维护

‍

### 双层架构

​

**应用程序（客户端）直接与数据库服务器相连**

> ​客户端(Client) <- JDBC -> 服务器端(DBMS)​

‍

作用    此架构中，Java Applet 或应用直接访问数据源

条件    要求 Driver 能与访问的数据库交互

机制    用户命令传给数据库或其他数据源，随之结果被返回

部署    数据源可以在另一台机器上，用户通过网络连接，称为 C/S配置（可以是内联网或互联网）

‍

局限

（1）受数据库厂商的限制，更换数据库需要改写大量的客户端应用程序代码. 

（2）受数据库版本限制，厂商更新数据库时，使用该数据库的应用程序需要重新编译和发布. 

（3）与使用和操作数据库相关的所有操作都在客户端应用程序中实现，造成客户端的设计和修改复杂，增加了客户端的成本. 

‍

‍

### 三层架构

‍

**客户端和数据库服务器端之间增加了一个中间服务器，客户端与中间服务器进行通信，由中间服务器处理来自客户端的请求并管理一个或者多个数据库服务器的连接**

> ​​Client <- HTTP/RMI -> 中间服务器 <- JDBC -> DBMS​​

​​

侧架构特殊之处在于，引入中间层服务

‍

流程    命令和结构都会经过该层. 

‍

优势

（1）将客户端与数据库系统区分开，数据库的更换不会影响客户端程序.   

（2）将密集任务的处理和操作抽象到更高层，简化客户端的设计，防止客户端变得庞大而臃肿.   

（3）由专门的服务器来处理客户端的请求，与数据的通信，提高了数据库的访问效率.   

‍

## 实现方案

‍

> **市面上常见的DB工具类和数据连接池**

* 数据库工具类 : Apache commens-dbutils

  * Apache 组织提供的一个开源 JDBC工具类库，它是对JDBC的简单封装,能极大简化jdbc编码的工作量，同时也不会影响程序的性能
  * 导入

    * 可以添加到tomcat的lib包
    * 可以添加到web-inf的lib包

‍

* 数据库连接池：c3p0、druid、dbcp

  * dbcp: 全称 DataBase connection pool，数据库连接池是 apache 上的一个Java连接池项目

‍

‍

‍

# 基础

‍

## 数据类型

‍

使用JDBC需要在Java数据类型和SQL数据类型之间进行转换

JDBC在java.sql.Types定义了一组常量来表示如何映射SQL数据类型

‍

|SQL数据类型|Java数据类型|
| ---------------| --------------------------|
|BIT, BOOL|boolean|
|INTEGER|int|
|BIGINT|long|
|REAL|float|
|FLOAT, DOUBLE|double|
|CHAR, VARCHAR|String|
|DECIMAL|BigDecimal|
|DATE|java.sql.Date, LocalDate|
|TIME|java.sql.Time, LocalTime|

‍

‍

## 元素

‍

### DriverManager(类)：数据库驱动管理类

* 作用：

  1. 注册驱动
  2. 创建java代码和数据库之间的连接，即获取Connection对象

‍

### Connection(接口)：建立数据库连接的对象

* 作用：用于建立java程序和数据库之间的连接

‍

‍

### Statement(接口)： 数据库操作对象(执行SQL语句的对象)

* 作用：用于向数据库发送sql语句

‍

‍

### ResultSet(接口)：结果集对象（一张虚拟表）

* 作用：sql查询语句的执行结果会封装在ResultSet中

‍

‍

## 初始化

‍

### 依赖引入

mysql-connector

‍

Maven

```xml
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.47</version>
    <scope>runtime</scope>
</dependency>
```

‍

> 添加依赖的scope是runtime，因为编译Java程序并不需要MySQL的这个jar包，只有在运行期才需要使用. 如果把runtime改成compile，虽然也能正常编译，但是在IDE里写程序的时候，会多出来一大堆类似com.mysql.jdbc.Connection这样的类，非常容易与Java标准库的JDBC接口混淆，所以坚决不要设置为compile

‍

‍

### 连接**格式**

Connection代表一个JDBC连接，它相当于Java程序到数据库的连接（通常是TCP连接）

打开一个Connection时，需要准备URL、用户名和口令，才能成功连接到数据库. 

URL是由数据库厂商指定的格式

‍

MySQL的URL

```
jdbc:mysql://<hostname>:<port>/<db>?key1=value1&key2=value2
```

> 数据库运行在本机`localhost`​，端口使用标准的`3306`​，数据库名称是`learnjdbc`​，那么URL如下：
>
> 后面的两个参数表示不使用SSL加密，使用UTF-8作为字符编码（注意MySQL的UTF-8是`utf8`​）
>
> ```
> jdbc:mysql://localhost:3306/learnjdbc?useSSL=false&characterEncoding=utf8
> ```

‍

‍

### 获取连接

核心代码是DriverManager提供的静态方法getConnection()

DriverManager会自动扫描classpath，找到所有的JDBC驱动，然后根据传入的URL自动挑选一个合适的驱动

建议把各个变量拆出来

```java
String JDBC_URL = "jdbc:mysql://localhost:3306/test";
String JDBC_USER = "root";
String JDBC_PASSWORD = "password";

Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD);
```

‍

### 释放连接

```java
// 关闭连接:
conn.close();
```

‍

JDBC连接是一种昂贵的资源，使用后要及时释放

使用`try (resource)`​来自动释放JDBC连接是一个好方法：

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    ...
}
```

‍

### 示例

‍

> **快速使用jdbc连接Mysql**

‍

**步骤**

* 加载JDBC驱动程序
* 建立数据库连接Connection
* 创建执行SQL的语句Statement
* 处理执行结果ResultSet
* 释放连接资源

  * resultSet.close();
  * statement.close();
  * connection.close();

‍

jdbc程序

```java
public static void main(String [] args) throws Exception{

         //加载JDBC驱动程序
         Class.forName("com.mysql.jdbc.Driver");

         //建立数据库连接Connection
        String username = "root";
        String password = "xdclass.net";
        //协议:子协议://ip:端口/数据库名称?参数1=值1&参数2=值2
        String url = "jdbc:mysql://127.0.0.1:3306/xd_web?useUnicode=true&characterEncoding=utf-8&useSSL=false";

        Connection connection = DriverManager.getConnection(url,username,password);

         //创建执行SQL的语句Statement
        Statement statement  = connection.createStatement();

         //处理执行结果ResultSet
        ResultSet resultSet = statement.executeQuery("select * from user");

        while (resultSet.next()){

            System.out.println("用户名称 name="+ resultSet.getString("username") + "  联系方式 wechat="+ resultSet.getString("wechat"));
        }

        //释放连接资源
        resultSet.close();
        statement.close();
        connection.close();


    }
```

‍

‍

## 操作

‍

### 查询

‍

#### ==步骤==

1. 通过`Connection`​提供的`createStatement()`​方法创建一个`Statement`​对象，用于执行一个查询
2. 执行`Statement`​对象提供的`executeQuery("SELECT * FROM students")`​并传入SQL语句，执行查询并获得返回的结果集，使用`ResultSet`​来引用这个结果集
3. 反复调用`ResultSet`​的`next()`​方法并读取每一行结果

‍

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {

    try (Statement stmt = conn.createStatement()) {

        try (ResultSet rs = stmt.executeQuery("SELECT id, grade, name, gender FROM students WHERE gender=1")) {

            while (rs.next()) {
                long id = rs.getLong(1); 
                long grade = rs.getLong(2);
                String name = rs.getString(3);
                int gender = rs.getInt(4);
            }
        }
    }
}
```

‍

#### ==注意==

**Statment**和**ResultSet**都是需要关闭的资源，因此嵌套使用try (resource)确保及时关闭；

​`rs.next()`​用于判断是否有下一行记录，如果有，将自动把当前行移动到下一行（一开始获得`ResultSet`​时当前行不是第一行）；

​`ResultSet`​获取列时，索引从`1`​开始而不是`0`​；

必须根据`SELECT`​的列的对应位置来调用`getLong(1)`​，`getString(2)`​这些方法，否则对应位置的数据类型不对，将报错. 

‍

‍

### 插入

‍

#### ==步骤==

本质上是用`PreparedStatement`​执行一条SQL语句，不过最后执行的不是`executeQuery()`​，而是`executeUpdate()`​

‍

设置参数与查询是一样的，有几个`?`​占位符就必须设置对应的参数

虽然Statement也可以执行插入操作，但仍然要严格遵循绝不能手动拼SQL字符串的原则，以避免安全漏洞. 

当成功执行executeUpdate()后，返回值是int，表示插入的记录数量. 

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {

    try (PreparedStatement ps = conn.prepareStatement("INSERT INTO students (id, grade, name, gender) VALUES (?,?,?,?)")) {

        ps.setObject(1, 999); // 注意：索引从1开始
        ps.setObject(2, 1); // grade
        ps.setObject(3, "Bob"); // name
        ps.setObject(4, "M"); // gender
        int n = ps.executeUpdate(); // 1 .此处总是1，因为只插入了一条记录

    }
}
```

‍

‍

#### ==获取主键==

要获取自增主键，不能先插入，再查询. 因为两条SQL执行期间可能有别的程序也插入了同一个表. 

正确写法是在创建`PreparedStatement`​的时候，指定一个`RETURN_GENERATED_KEYS`​标志位(常量)，表示JDBC驱动必须返回插入的自增主键

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {
    try (PreparedStatement ps = conn.prepareStatement(
            "INSERT INTO students (grade, name, gender) VALUES (?,?,?)",
            Statement.RETURN_GENERATED_KEYS)) {

        ps.setObject(1, 1); // grade
        ps.setObject(2, "Bob"); // name
        ps.setObject(3, "M"); // gender

        int n = ps.executeUpdate(); // 1

        try (ResultSet rs = ps.getGeneratedKeys()) {
            if (rs.next()) {
                long id = rs.getLong(1); // 注意：索引从1开始
            }
        }
    }
}
```

‍

‍

#### ==注意==

执行`executeUpdate()`​方法后，必须调用`getGeneratedKeys()`​获取一个`ResultSet`​对象

它包含了数据库自动生成的主键的值，读取该对象的每一行来获取自增主键的值

如果一次插入多条记录，那么这个`ResultSet`​对象就会**有多行返回值**

如果插入时有多列自增，那么`ResultSet`​对象的**每一行都会对应多个自增值(** 自增列不一定必须是主键)

‍

‍

### 更新

‍

#### ==步骤==

​`UPDATE`​语句可以一次更新若干列的记录

​`executeUpdate()`​返回数据库实际更新的行数. 返回结果可能是正数，也可能是0（表示没有任何记录更新）. 

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {

    try (PreparedStatement ps = conn.prepareStatement("UPDATE students SET name=? WHERE id=?")) {
        ps.setObject(1, "Bob"); // 注意：索引从1开始
        ps.setObject(2, 999);
        int n = ps.executeUpdate(); // 返回更新的行数
    }
}
```

‍

‍

‍

### 删除

‍

#### ==步骤==

​`DELETE`​语句可以一次删除若干行

```java
try (Connection conn = DriverManager.getConnection(JDBC_URL, JDBC_USER, JDBC_PASSWORD)) {

    try (PreparedStatement ps = conn.prepareStatement("DELETE FROM students WHERE id=?")) {

        ps.setObject(1, 999); // 注意：索引从1开始
        int n = ps.executeUpdate(); // 删除的行数
    }
}
```

‍

‍

‍

‍

## 操作综合模板

‍

> 增删改都是executeUpdate(sql)
>
> 查是executeQuery(sql)
>
> 返回值>0修改成功

‍

==步骤==

1. 加载驱动
2. 连接数据库DriverManager
3. 获得执行sql的对象Statement
4. 获得返回的结果集
5. 释放连接

```java
        //1. 注册(加载)驱动(需要驱动包,导入至相关库)
        Class.forName("com.mysql.cj.jdbc.Driver");

        //2. 获取数据库连接, 设置信息
        String url="jdbc:mysql://127.0.0.1:3306/mybatis";
        String username = "root";
        String password = "2333";
        Connection connection = DriverManager.getConnection(url, username, password);

        //3. 执行SQL
        Statement statement = connection.createStatement(); //操作SQL的对象

        String sql="select id,name,age,gender,phone from user"; //封装SQL语句到字符串

        ResultSet rs = statement.executeQuery(sql);    //SQL查询结果会封装在ResultSet对象中

        //4. 处理SQL执行结果封装的结果集
        while (rs.next()){ //逐个判断取出

            int id = rs.getInt("id");//取出一行记录中id、name、age、gender、phone下的数据
            //为了不报错可以用getObject()克服类型问题

            ......

            User user = new User(id,name,age,gender,phone); //把一行记录中的数据，封装到User对象

            userList.add(user);//User对象添加到集合

        }

        //5. 释放资源
        statement.close();
        connection.close();
        rs.close();

```

‍

‍

‍

# 高级

‍

## 自动化工具类

‍

try-catch-finally完成资源收放

工具类抽取对应方法

‍

properties保存db参数

```java
public class TestSelect {
    public static void main( String[] args) {
        Connection conn =null;
        statement st = null;Resultset rs = null;

        try {
            conn = jdbcUtils.getConnection();st = conn. createStatement( );

            //SQL
            String sql = "select * from users where id = 1" ;
            rs = st.executeQuery(sql); //查询完毕会返回一个结果集
  
            while (rs.next()){
                system.out.println(rs.getString( columnLabel:"NAME"));
        }
        catch (SQLException e) {
            e.printStackTrace();
        finally {
            JdbcUtils.release(conn, st,rs) ;
        }
}
```

‍

‍

## SQL注入

‍

要避免SQL注入攻击，一个办法是针对所有字符串参数进行转义，但是转义很麻烦，而且需要在任何使用SQL的地方增加转义代码

使用**PreparedStatement**可以完全避免SQL注入的问题

‍

PreparedStatement始终使用?作为占位符，并且把数据连同SQL本身传给数据库，这样可以保证每次传给数据库的SQL语句是相同的，和仅仅是占位符的数据不同，还能高效利用数据库本身对查询的缓存

​`PreparedStatement`​比`Statement`​更安全，而且更快

‍

‍

### ==步骤==

1. PreparedStatement对象替代Statement
2. conn直接使用prepareStatement(),需要预编译SQL, 使用?占位符替代参数;
3. 手动给参数赋值

‍

使用PreparedStatement和Statement稍有不同，必须首先调用`setObject()`​设置每个占位符`?`​的值，最后获取的仍然是`ResultSet`​对象. 

另外注意到从结果集读取列时，使用`String`​类型的列名比索引要易读，而且不易出错. 

注意到JDBC查询的返回值总是`ResultSet`​，即使我们写这样的聚合查询`SELECT SUM(score) FROM ...`​，也需要按结果集读取：

```java
ResultSet rs = ...
if (rs.next()) {
    double sum = rs.getDouble(1);
}
```

‍

‍

### **示例**

```java
User login(String name, String pass) {
    ...
    String sql = "SELECT * FROM user WHERE login=? AND pass=?";
    PreparedStatement ps = conn.prepareStatement(sql);
    ps.setObject(1, name);
    ps.setObject(2, pass);
    ...
}
```

‍

‍

## 批量操作

Batch功能

‍

通过一个循环来执行每个`PreparedStatement`​​虽然可行，但是性能很低. SQL数据库对SQL语句相同，但只有参数不同的若干语句可以作为batch执行，即批量执行，这种操作有特别优化，**速度远远快于循环执行每个SQL**

利用SQL数据库的这一特性，可以把同一个SQL但参数不同的若干次操作合并为一个batch执行

执行batch和执行一个SQL不同点在于，需要对同一个`PreparedStatement`​反复设置参数并调用`addBatch()`​，这样就相当于给一个SQL加上了多组参数，相当于变成了“多行”SQL. 

第二个不同点是调用的不是`executeUpdate()`​，而是`executeBatch()`​，因为我们设置了多组参数，相应地，返回结果也是多个`int`​值，因此返回类型是`int[]`​，循环`int[]`​数组即可获取每组参数执行后影响的结果数量. 

‍

**示例**

```java
try (PreparedStatement ps = conn.prepareStatement("INSERT INTO students (name, gender, grade, score) VALUES (?, ?, ?, ?)")) {

    // 对同一个PreparedStatement反复设置参数并调用addBatch()
    for (Student s : students) {
        ps.setString(1, s.name);
        ps.setBoolean(2, s.gender);
        ps.setInt(3, s.grade);
        ps.setInt(4, s.score);
        ps.addBatch(); // 添加到batch
    }

    // 执行batch
    int[] ns = ps.executeBatch();
    for (int n : ns) {
        System.out.println(n + " inserted."); // batch中每个SQL执行的结果数量
    }

}
```

‍

‍

‍

## 事务

‍

‍

在JDBC中执行事务，本质上就是如何把多条SQL包裹在一个数据库事务中执行

‍

开启事务的关键代码是`conn.setAutoCommit(false)`​，表示关闭自动提交. 

提交事务的代码在执行完指定的若干条SQL语句后，调用`conn.commit()`​. 

提交失败，会抛出SQL异常（也可能在执行SQL语句的时候就抛出了），此时必须捕获并调用`conn.rollback()`​回滚事务. 最后，在`finally`​中通过`conn.setAutoCommit(true)`​把`Connection`​对象的状态恢复到初始值. 

‍

‍

### 模板

```java
try {
	// 设置是否自动提交
	connection.setAutoCommit(false)

	// 数据库操作 insert，update，delete

	connection.commit()
} catch(Exception ex) {
	// 回滚
	connection.rollback()
} finally {
	connection.setAutoCommit(true)
}
```

‍

模板1

```java
Connection conn = openConnection();
try {
    // 关闭自动提交:
    conn.setAutoCommit(false);
    // 执行多条SQL语句:
    insert(); update(); delete();
    // 提交事务:
    conn.commit();
} catch (SQLException e) {
    // 回滚事务:
    conn.rollback();
} finally {
    conn.setAutoCommit(true);
    conn.close();
}
```

‍

模板2

```java
    try{
    
        conn = getConnection 获取到conn
    
        conn.setAutoCommit(false) //关闭数据库自动提交,自动开启事务, 自动失败回滚
    
    
        ......自己代码段
    
    
        conn.commit()  //提交事务
  
    }catch(E e){
  
        try{ //显式声明回滚
            conn.rollback();//回滚(不写也会回滚,默认就是
        }
  
    }finally{
            JdbcUtils.releash(conn,st,rs);

```

‍

‍

### 其他

设定事务的隔离级别

```java
// 设定隔离级别为READ COMMITTED:
conn.setTransactionIsolation(Connection.TRANSACTION_READ_COMMITTED);
```

‍

‍

## 连接池

‍

JDBC连接池有一个标准的接口`javax.sql.DataSource`​​，注意这个类位于Java标准库中，但仅仅是接口. 

要使用JDBC连接池，必须选择一个JDBC连接池的实现

‍

### 配置

Mybatis中配套的是Hikari

‍

以追光者Hikari为例, 单独使用JDBC情况下添加连接池

‍

‍

依赖

‍

创建一个`DataSource`​实例，这个实例就是连接池

创建`DataSource`​也是一个非常昂贵的操作，所以通常`DataSource`​实例总是作为一个全局变量存储，并贯穿整个应用程序的生命周期

```java
HikariConfig config = new HikariConfig();
config.setJdbcUrl("jdbc:mysql://localhost:3306/test");
config.setUsername("root");
config.setPassword("password");
config.addDataSourceProperty("connectionTimeout", "1000"); // 连接超时：1秒
config.addDataSourceProperty("idleTimeout", "60000"); // 空闲超时：60秒
config.addDataSourceProperty("maximumPoolSize", "10"); // 最大连接数：10
DataSource ds = new HikariDataSource(config);
```

‍

‍

具体实现和前面的代码类似，只是获取`Connection`​时，把`DriverManage.getConnection()`​改为`ds.getConnection()`​：

```
try (Connection conn = ds.getConnection()) { // 在此获取连接
    ...
} // 在此“关闭”连接
```

‍

### 原理

‍

通过连接池获取连接时，并不需要指定JDBC的相关URL、用户名、口令等信息，因为这些信息已经存储在连接池内部了

一开始，连接池内部并没有连接，所以，第一次调用`ds.getConnection()`​，会迫使连接池内部先创建一个`Connection`​，再返回给客户端使用. 当我们调用`conn.close()`​方法时（`在try(resource){...}`​结束处），不是真正“关闭”连接，而是释放到连接池中，以便下次获取连接时能直接返回. 

因此，连接池内部维护了若干个`Connection`​实例，如果调用`ds.getConnection()`​，就选择一个空闲连接，并标记它为“正在使用”然后返回，如果对`Connection`​调用`close()`​，那么就把连接再次标记为“空闲”从而等待下次调用. 这样一来，我们就通过连接池维护了少量连接，但可以频繁地执行大量的SQL语句. 

‍

通常连接池提供了大量的参数可以配置，例如，维护的最小、最大活动连接数，指定一个连接在空闲一段时间后自动关闭等，需要根据应用程序的负载合理地配置这些参数. 此外，大多数连接池都提供了详细的实时状态以便进行监控. 

‍

‍

‍

# 实操

‍

## **面试题**

‍

XD

* 说下JDBC连接数据库的开发步骤  
  1.加载数据库连接驱动      
  2.获取数据连接对象  
  3.获取语句对象  
  会话对象有两种Statement和PreparedStatement      4.执行语句     
  5.处理结果集    6.关闭资源   rs.close()、st.close()、conn.close() 注意关闭顺序以及处理异常
* JDBC中的Statement 和PreparedStatement的区别

  * PreparedStatement在执行之前会进行预编译
  * 效率高于Statement,且能够有效防止SQL注入
  * PreparedStatement支持?占位符而不是直接拼接，提高可读性
* 数据库连接池工作原理和优点

  * 先创建一定数量的连接对象存放在连接池
  * 需要使用连接对象的时候，从连接池中请求一个空闲的连接
  * 使用完毕之后，并不会把连接关闭，而是还给连接池
  * 优点：

    * 系统响应速度加快
    * 资源利用率高

‍

‍

## Bugfix

‍

### 连接不上数据库

* 检查防火墙-云服务器的网络安全组
* mysql有没开启允许远程连接

‍
