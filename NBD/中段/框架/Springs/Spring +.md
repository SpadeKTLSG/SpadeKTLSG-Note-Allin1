--最流行的Java框架, 下

‍

‍

### Header

Spring下篇, 高级篇

‍

# 高级

‍

## AOP

SpringAOP

不惊动原始设计的基础上为方法进行功能增强

**AOP(Aspect Oriented Programming)面向切面编程，一种编程范式**

‍

‍

‍

‍

‍

### 概念

‍

‍

> 具体情境**介绍**
>
> (1)Spring的AOP是对一个类的方法在不进行任何修改的前提下实现增强. 对于上面的案例中BookServiceImpl中有`save`​,`update`​,`delete`​和`select`​方法,这些方法我们给起了一个名字叫==连接点==
>
> (2)在BookServiceImpl的四个方法中，`update`​和`delete`​只有打印没有计算万次执行消耗时间，但是在运行的时候已经有该功能，那也就是说`update`​和`delete`​方法都已经被增强，所以对于需要增强的方法我们给起了一个名字叫==切入点==
>
> (3)执行BookServiceImpl的update和delete方法的时候都被添加了一个计算万次执行消耗时间的功能，将这个功能抽取到一个方法中，换句话说就是存放共性功能的方法，我们给起了个名字叫==通知==
>
> (4)通知是要增强的内容，会有多个，切入点是需要被增强的方法，也会有多个，那哪个切入点需要添加哪个通知，就需要提前将它们之间的关系描述清楚，那么对于通知和切入点之间的关系描述，我们给起了个名字叫==切面==
>
> (5)通知是一个方法，方法不能独立存在需要被写在一个类中，这个类我们也给起了个名字叫==通知类==

‍

‍

* 连接点(JoinPoint)：程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等

  * 在SpringAOP中，理解为方法的执行
* 切入点(Pointcut):匹配连接点的式子

  * 在SpringAOP中，一个切入点可以描述一个具体方法，也可也匹配多个方法

    * 一个具体的方法:如com.itheima.dao包下的BookDao接口中的无形参无返回值的save方法
    * 匹配多个方法:所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所有带有一个参数的方法
  * 连接点范围要比切入点范围大，是切入点的方法也一定是连接点，但是是连接点的方法就不一定要被增强，所以可能不是切入点.
* 通知(Advice):在切入点处执行的操作，也就是共性功能

  * 在SpringAOP中，功能最终以方法的形式呈现
* 通知类：定义通知的类
* 切面(Aspect):描述通知与切入点的对应关系.
* 织入(Weaving)：把通知应用到具体的类，进而创建新的代理类的过程.

‍

‍

#### 核心概念

‍

在工作流程中提到了两个核心概念

* 目标对象(Target)：原始功能**去掉共性功能**对应的类产生的对象，这种对象是无法直接完成最终工作的
* 代理(Proxy)：目标对象无法直接完成工作，需要对其进行功能回填，通过原始对象的代理对象实现

‍

目标对象就是要增强的类对应的对象，也叫原始对象; 在不改变原有设计(代码)的前提下对其进行增强，它的底层采用的是代理模式实现的，所以要对原始对象进行增强就需要对原始对象创建代理对象，在代理对象中的方法把通知[如:MyAdvice中的method方法]内容加进去，就实现了增强,这就是我们所说的代理(Proxy)

‍

‍

### 结构

‍

‍

**核心**

* 代理（Proxy）：SpringAOP的核心本质是采用代理模式实现的
* 连接点（JoinPoint）：在SpringAOP中，理解为任意方法的执行
* 切入点（Pointcut）：匹配连接点的式子，也是具有共性功能的方法描述
* 通知（Advice）：若干个方法的共性功能，在切入点处执行，最终体现为一个方法
* 切面（Aspect）：描述通知与切入点的对应关系
* 目标对象（Target）：被代理的原始对象成为目标对象

‍

‍

==基础结构==

‍

```java
public class MyAdvice{   //通知类
  
    //通知

}

public void save(){    //连接点

    doSth()    //(要增强的方法 -> 切入点
}

public void update(){    //连接点

通知  -> 切面  <- 切入点
```

‍

### 代理模式 - 插入

‍

二十三种设计模式中的一种，属于结构型模式. 它的作用就是通过提供一个代理类，让我们在调用目标方法的时候，不再是直接对目标方法进行调用，而是通过代理类**间接**调用. 让不属于目标方法核心逻辑的代码从目标方法中剥离出来——**解耦**. 调用目标方法时先调用代理对象的方法，减少对目标方法的调用和打扰，同时让附加功能能够集中在一起也有利于统一维护.

* 代理：将非核心逻辑剥离出来以后，封装这些非核心逻辑的类、对象、方法.
* 目标：被代理“套用”了非核心逻辑代码的类、对象、方法.

‍

#### 静态代理

```java
public class CalculatorStaticProxy implements Calculator {

    // 将被代理的目标对象声明为成员变量
    private Calculator target;

    public CalculatorStaticProxy(Calculator target) {
        this.target = target;
    }

    @Override
    public int add(int i, int j) {

        // 附加功能由代理类中的代理方法来实现
        System.out.println("[日志] add 方法开始了，参数是：" + i + ", " + j);

        // 通过目标对象来实现核心业务逻辑
        int addResult = target.add(i, j);

        System.out.println("[日志] add 方法结束了，结果是：" + addResult);

        return addResult;
    }
}
```

> 静态代理确实实现了解耦，但是由于代码都写死了，完全不具备任何的灵活性. 就拿日志功能来说，将来其他地方也需要附加日志，那还得再声明更多个静态代理类，那就产生了大量重复的代码，日志功能还是分散的，没有统一管理.
>
> 提出进一步的需求：将日志功能集中到一个代理类中，将来有任何日志需求，都通过这一个代理类来实现. 这就需要使用动态代理技术了.

‍

‍

#### 动态代理

‍

生产代理对象的工厂类：

```java
public class ProxyFactory {

    private Object target;

    public ProxyFactory(Object target) {
        this.target = target;
    }

    public Object getProxy() {

        /**
         * newProxyInstance()：创建一个代理实例
         * 其中有三个参数：
         * 1、classLoader：加载动态生成的代理类的类加载器
         * 2、interfaces：目标对象实现的所有接口的class对象所组成的数组
         * 3、invocationHandler：设置代理对象实现目标对象方法的过程，即代理类中如何重写接口中的抽象方法
         */
        ClassLoader classLoader = target.getClass().getClassLoader();
        Class<?>[] interfaces = target.getClass().getInterfaces();
        InvocationHandler invocationHandler = new InvocationHandler() {
            @Override
            public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
                /**
                 * proxy：代理对象
                 * method：代理对象需要实现的方法，即其中需要重写的方法
                 * args：method所对应方法的参数
                 */
                Object result = null;
                try {
                    System.out.println("[动态代理][日志] "+method.getName()+"，参数："+ Arrays.toString(args));
                    result = method.invoke(target, args);
                    System.out.println("[动态代理][日志] "+method.getName()+"，结果："+ result);
                } catch (Exception e) {
                    e.printStackTrace();
                    System.out.println("[动态代理][日志] "+method.getName()+"，异常："+e.getMessage());
                } finally {
                    System.out.println("[动态代理][日志] "+method.getName()+"，方法执行完毕");
                }
                return result;
            }
        };

        return Proxy.newProxyInstance(classLoader, interfaces, invocationHandler);
    }
}
```

‍

#### 测试

```java
@Test
public void testDynamicProxy() {
    ProxyFactory factory = new ProxyFactory(new CalculatorLogImpl());
    Calculator proxy = (Calculator) factory.getProxy();
    proxy.div(1, 0);
    //proxy.div(1, 1);
}
```

‍

‍

### 基础实现

采用注解完成

‍

@EnableAspectJAutoProxy

|名称|@EnableAspectJAutoProxy|
| ------| -------------------------|
|类型|配置类注解|
|位置|配置类定义上方|
|作用|开启注解格式AOP功能|

‍

@Aspect

|名称|@Aspect|
| ------| -----------------------|
|类型|类注解|
|位置|切面类定义上方|
|作用|设置当前类为AOP切面类|

‍

@Pointcut

|名称|@Pointcut|
| ------| -----------------------------|
|类型|方法注解|
|位置|切入点方法定义上方|
|作用|设置切入点方法|
|属性|value（默认）：切入点表达式|

‍

@Before

|名称|@Before|
| ------| ----------------------------------------------------------------------------|
|类型|方法注解|
|位置|通知方法定义上方|
|作用|设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前运行|

‍

‍

#### **注解配置**

1.导入坐标(pom.xml)

2.制作连接点(原始操作，Dao接口与实现类)

3.制作共性功能(通知类与通知)

4.定义切入点

5.绑定切入点与通知关系(切面)

‍

‍

##### 导入AspectJ

​`spring-context`​中已经导入了`spring-aop`​

导入AspectJ的jar包, AspectJ是AOP思想的一个具体实现，**Spring有自己的AOP实现**，但是相比于AspectJ来说比较麻烦，所以直接采用**Spring整合ApsectJ**的方式进行AOP开发

‍

pom.xml

AspectJ导入

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

‍

‍

##### 定义通知类

通知, 切入点, 切面

‍

###### **通知**

通知就是将共性功能抽取出来后形成的方法

```java
public class MyAdvice {

    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

‍

###### **切入点**

要增强的是update方法, 在MyAdvice中添加无用的方法pt, 写@Pointcut注解

切入点定义依托一个不具有实际意义的方法进行，即无参数、无返回值、方法体无实际逻辑

```java
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
```

‍

###### **切面**

通过注解进行关系绑定: 绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行位置

```java
    @Before("pt()")
    public void method(){
```

‍

###### 配置容器

‍

**将通知类配给容器并标识其为切面类**

使用Component表示为bean, 同时记录为Aspect对象应用切面

```java
@Component
@Aspect
class...
```

‍

###### 示例

‍

完成的通知类示例

```java
@Component
@Aspect
public class MyAdvice {
        @Pointcut("execution(void com.itheima.dao.BookDao.save())")
        private void ptx(){}
  
        @Pointcut("execution(void com.itheima.dao.BookDao.update())")
        private void pt(){}
  
        @Before("pt()")
        public void method(){
            System.out.println( System.currentTimeMillis());
        }
}
```

‍

‍

##### 开启AOP配置

Config开启注解格式AOP功能 @EnableAspectJAutoProxy

```java
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

‍

#### XML配置

```html
    <!-- 目标类,UserService -->
    <bean id="userService" class="com.qf.service.impl.UserServiceImpl"/>

    <!-- 切面类 -->
    <bean id="myAspect" class="com.qf.aspect.MyAspect"/>

    <!-- 织入 -->
    <aop:config>
        <!-- 确定切面-->
        <aop:aspect ref="myAspect">
            <!-- 1确定通知/增强类型 -->
            <!-- 2确定切面中增强的方法 -->
            <!-- 3确定目标类以及目标方法
               execution(返回值类型 包路径.类名.方法名(参数))
               返回值类型任意,写 *
               包路径,写com.qf
               类名,UserServiceImpl,如果是所有类,写 *
               方法名,findUserById,如果是所有方法,写 *
               () 参数列表
               (..) 代表任意参数
            -->
            <aop:around method="myAround"
                        pointcut="execution(* com.qf.service.impl.*.*(..))"/>
        </aop:aspect>
    </aop:config>
```

‍

‍

‍

### 工作流程

‍

‍

#### 容器启动

从Spring加载bean说起, 容器启动就需要去加载bean

被加载的类包含需要被增强的类 和 通知类

注意此时bean对象还没有创建成功

‍

#### 读取切入点

读取所有切面配置中的**使用到的**切入点

‍

#### 初始化bean

判定bean对应的类中的方法**是否匹配到任意切入点**

* 注意第1步在容器启动的时候，bean对象还没有被创建成功.
* 要被实例化bean对象的类中的方法和切入点进行匹配

‍

**判断**

* 匹配失败，创建原始对象,如`UserDao`​

  匹配失败说明不需要增强，直接调用原始对象的方法即可.
* 匹配成功，创建原始对象（==目标对象==）的==代理==对象,如:`BookDao`​

  * 匹配成功说明需要对其进行增强
  * 对哪个类做增强，这个类对应的对象就叫做**目标对象**
  * 因为要对目标对象进行功能增强，而采用的技术是**动态代理**，所以会为其创建一个**代理对象**
  * 最终运行的是代理对象的方法，在该方法中会对原始方法进行功能增强

‍

‍

#### 获取bean执行方法

* 获取的bean是原始对象时，调用方法并执行，完成操作
* 获取的bean是代理对象时，根据代理对象的运行模式**运行原始方法与增强的内容**，完成操作

‍

容器中是否为代理对象:

* 如果目标对象中的方法会被增强，那么容器中将存入的是目标对象的代理对象
* 如果目标对象中的方法不被增强，那么容器中将存入的是目标对象本身.

‍

‍

### 配置管理

‍

#### 切入点表达式

‍

概念

* 切入点:要进行增强的方法
* 切入点表达式:要进行增强的方法的描述方式

‍

##### 语法

‍

> 动作关键字(访问修饰符 返回值 包名.类/接口名.方法名(参数) 异常名）

```
execution(public User com.itheima.service.UserService.findById(int))
```

* execution：动作关键字，描述切入点的行为动作，例如execution表示执行到指定切入点
* public:访问修饰符,还可以是public，private等，可以省略
* User：返回值，写返回值类型
* com.itheima.service：包名，多级包使用点连接
* UserService:类/接口名称
* findById：方法名
* int:参数，直接写参数的类型，多个类型用逗号隔开
* 异常名：方法定义中抛出指定异常，可以省略

‍

‍

> 因为调用接口方法的时候最终运行的还是其实现类的方法，两种描述方式都是可以的
>
> 描述方式一：执行包下的BookDao接口中的无参数update方法
>
> ```java
> execution(void com.itheima.dao.BookDao.update())
> ```
>
> 描述方式二：执行包下的BookDaoImpl类中的无参数update方法
>
> ```java
> execution(void com.itheima.dao.impl.BookDaoImpl.update())
> ```

‍

‍

##### 通配符

* ​`*`​：匹配任意符号（常用）
* ​`..`​ ：匹配多个连续的任意符号（常用）
* ​`+`​：匹配子类类型

‍

​`*`​   **单个独立的任意符号**

可以独立出现，也可以作为前缀或者后缀的匹配符出现

```
execution（public * com.itheima.*.UserService.find*(*))
```

匹配com.itheima包下的任意包中的UserService类或接口中所有find开头的带有一个参数的方法

‍

‍

​`..`​   **多个连续的任意符号**

可以独立出现，常用于简化包名与参数的书写

```
execution（public User com..UserService.findById(..))
```

匹配com包下的任意包中的UserService类或接口中所有名称为findById的方法

‍

‍

​`+`​   专用于**匹配子类类型**

```
execution(* *..*Service+.*(..))
```

\*Service+，表示所有以Service结尾的接口的子类

‍

‍

##### **示例**

```java
execution(void com.itheima.dao.BookDao.update())
匹配接口，能匹配到
execution(void com.itheima.dao.impl.BookDaoImpl.update())
匹配实现类，能匹配到
execution(* com.itheima.dao.impl.BookDaoImpl.update())
返回值任意，能匹配到
execution(* com.itheima.dao.impl.BookDaoImpl.update(*))
返回值任意，但是update方法必须要有一个参数，无法匹配，要想匹配需要在update接口和实现类添加参数
```

```java
execution(void com.*.*.*.*.update())
返回值为void,com包下的任意包三层包下的任意类的update方法，匹配到的是实现类，能匹配

execution(void com.*.*.*.update())
返回值为void,com包下的任意两层包下的任意类的update方法，匹配到的是接口，能匹配

execution(void *..update())
返回值为void，方法名是update的任意包下的任意类，能匹配
```

```java
execution(* *..*(..))
匹配项目中任意类的任意方法，能匹配，但是不建议使用这种方式，影响范围广(太抽象了)

execution(* *..u*(..))
匹配项目中任意包任意类下只要以u开头的方法，update方法能满足，能匹配
execution(* *..*e(..))
匹配项目中任意包任意类下只要以e结尾的方法，update和save方法能满足，能匹配
execution(void com..*())
返回值为void，com包下的任意包任意类任意方法，能匹配，*代表的是方法
```

‍

更规范的示例

execution(* com.itheima. *.* Service.find*(..))  
将项目中所有业务层方法的以find开头的方法匹配

execution(* com.itheima. *.* Service.save*(..))  
将项目中所有业务层方法的以save开头的方法匹配

‍

‍

##### 书写技巧

‍

> 所有代码都要按照**标准规范开发**，否则以下技巧全部失效

‍

* 描述切入点通常**==描述接口==**​ **，而不描述实现类, 如果描述到实现类，就出现紧耦合了**
* 访问控制修饰符针对接口开发均**采用public描述（** 访问控制修饰符针对接口开发均采用public描述（**==可省略访问控制修饰符描述==**​ **）** ）
* 返回值类型对于**增删改**类使用精准类型加速匹配，对于查询类使用*通配快速描述
* 包名书写**尽量不使用..匹配，效率过低**，常用*做单个包描述匹配，或精准匹配
* 接口名/类名书写名称**与模块相关的**采用匹配，例如UserService书写成Service，绑定业务层接口名
* 方法名书写以书写以动词进行进行精准匹配，例如getById书写成getBy
* 参数规则较为复杂，根据业务方法灵活调整
* 通常通常不使用**异常**作为匹配规则规则

‍

‍

### 通知

‍

#### 概念

‍

* 前置通知：使用@Before注解标识，在被代理的目标方法**前**执行
* 返回通知：使用@AfterReturning注解标识，在被代理的目标方法**成功结束**后执行（**寿终正寝**）
* 异常通知：使用@AfterThrowing注解标识，在被代理的目标方法**异常结束**后执行（**死于非命**）
* 后置通知：使用@After注解标识，在被代理的目标方法**最终结束**后执行（**盖棺定论**）
* 环绕通知：使用@Around注解标识，使用try...catch...finally结构围绕**整个**被代理的目标方法，包括上面四种通知对应的所有位置

‍

##### 应用场合

‍

* 环绕通知: 目标方法执行前,后都有增强的方法(已经演示过)

  * 常用于事务管理
* 前置通知

  * 常用于权限检验
* 后置通知

  * 常用于日志记录,或者释放资源
* 后置返回通知

  * 常用于获得目标方法的返回值,处理返回值
* 异常通知

  * 常用于代码有异常后去执行一些功能,处理异常

‍

##### 执行顺序

各种通知的执行顺序：

* Spring版本5.3.x以前：

  * 前置通知
  * 目标操作
  * 后置通知
  * 返回通知或异常通知
* Spring版本5.3.x以后：

  * 前置通知
  * 目标操作
  * 返回通知或异常通知
  * 后置通知

‍

##### 注解综合

‍

@After

|名称|@After|
| ------| ----------------------------------------------------------------------------|
|类型|方法注解|
|位置|通知方法定义上方|
|作用|设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法后运行|

‍

@AfterReturning

|名称|@AfterReturning|
| ------| --------------------------------------------------------------------------------------|
|类型|方法注解|
|位置|通知方法定义上方|
|作用|设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法正常执行完毕后执行|

‍

@AfterThrowing

|名称|@AfterThrowing|
| ------| --------------------------------------------------------------------------------------|
|类型|方法注解|
|位置|通知方法定义上方|
|作用|设置当前通知方法与切入点之间绑定关系，当前通知方法在原始切入点方法运行抛出异常后执行|

‍

@Around

|名称|@Around|
| ------| ------------------------------------------------------------------------------|
|类型|方法注解|
|位置|通知方法定义上方|
|作用|设置当前通知方法与切入点之间的绑定关系，当前通知方法在原始切入点方法前后运行|

‍

#### 示例

‍

##### **位置示例**

```java
public class ClassName(){
    public void methodName(){
        1    前置
        try{
            2    前置
            DoSth()    增强方法
            3    返回后通知
        }catch(Exception e){
            4    异常后通知
        }
        5    后置通知
    }
}
```

(1)前置通知，追加功能到方法执行前,类似于在代码1或者代码2添加内容

(2)后置通知,追加功能到方法执行后,不管方法执行的过程中有没有抛出异常都会执行，类似于在代码5添加内容

(3)返回后通知,追加功能到方法执行后，只有方法正常执行结束后才进行,类似于在代码3添加内容，如果方法执行抛出异常，返回后通知将不会被添加

(4)抛出异常后通知,追加功能到方法抛出异常后，只有方法执行出异常才进行,类似于在代码4添加内容，只有方法抛出异常后才会被添加

(5)环绕通知,环绕通知功能比较强大，它可以追加功能到方法执行的前后, 可以实现其他四种通知类型的功能

‍

‍

##### **通知类示例**

```java
@Component
@Aspect
public class MyAdvice {
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
  
    @Before("pt()")
    public void before() {
        System.out.println("before advice ...");
    }
}
```

‍

‍

#### 前置

before方法上添加`@Before注解`​

```java
    @Before("pt()")
    //此处也可以写成 @Before("MyAdvice.pt()"),不建议没必要
    public void before() 
```

‍

#### 后置

```java
    @After("pt()")
    public void after()
```

‍

#### 环绕

必须要能对原始操作进行调用

> 类似夹心饼干, 还要帮里面的小兄弟处理异常和返回值; 环绕通知中是可以对原始方法返回值就地修改的

```java
    @Around("pt()")
    public void around(ProceedingJoinPoint pjp) throws Throwable{ //proceed()为什么要抛出异常? 不能帮人擦屁股
        System.out.println("around before advice ...");

        Object ret=pjp.proceed(); //表示对原始操作的调用

        System.out.println("around after advice ...");
        return ret;
    }
```

返回更通用的Object避免类型问题

‍

==**注意事项**==

1. 环绕通知必须依赖**形参ProceedingJoinPoint**才能实现对原始方法的调用，进而实现原始方法调用前后同时添加通知, 最好马上处理这个东西
2. 通知中如果未使用ProceedingJoinPoint对原始方法进行调用, 将跳过原始方法的执行
3. 对原始方法的调用可以不接收返回值，通知方法设置成void即可，如果接收返回值，最好设定为Object类型
4. 原始方法的返回值如果是void类型，通知方法的返回值类型**也可以设置成Object**
5. 由于无法预知原始方法运行后是否会抛出异常，因此环绕通知方法**必须要处理Throwable异常**

‍

* 环绕通知依赖形参ProceedingJoinPoint才能实现对原始方法的调用
* 环绕通知可以隔离原始方法的调用执行
* 环绕通知返回值设置为Object类型
* 环绕通知中可以对原始方法调用过程中出现的异常进行处理

‍

‍

#### 返回后

需要在原始方法`select`​正常执行后才会被执行，如果`select()`​方法执行的过程中出现了异常，那么返回后通知是不会被执行

对比: 后置通知是不管原始方法有没有抛出异常都会被执行.

```java
@AfterReturning("pt()")
    public void afterReturning()
```

‍

#### 异常后

抛出异常被执行

```java
@AfterThrowing("pt()")
    public void afterThrowing()
```

‍

‍

### 通知获取数据

‍

**要点分析**

* 获取切入点方法的参数，所有的通知类型都可以获取参数

  * JoinPoint：适用于前置、后置、返回后、抛出异常后通知
  * ProceedingJoinPoint：适用于环绕通知
* 获取切入点方法返回值，前置和抛出异常后通知是没有返回值，后置通知可有可无，所以不做研究

  * 返回后通知
  * 环绕通知
* 获取切入点方法运行异常信息，前置和返回后通知是不会有，后置通知可有可无，所以不做研究

  * 抛出异常后通知
  * 环绕通知

‍

‍

#### 参数

‍

##### 环绕

专门使用的是ProceedingJoinPoint，是JoinPoint的子类, 可以实现对原方法的调用

‍

pjp.proceed()方法有有参数和无参数的两个构造方法, 任意一个都可以完成功能

调用无参的proceed，当原始方法有参数会在调用的过程中自动传入参数

但是当需要修改原始方法的参数时，就**只能采用带有参数的方法**

这样便可对原始方法的参数进行拦截过滤，避免由于参数的问题导致程序无法正确运行，保证代码的健壮性

```java
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) throws Throwable{
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        args[0] = 666; //偷偷改一下参数
        Object ret = pjp.proceed(args);
        return ret;
    }
```

‍

##### 非环绕

在方法上添加JoinPoint来**获取参数数组**, 适用于`前置`​、`后置`​、`返回后`​、`抛出异常后`​通知

因为参数的个数不固定的, 使用数组更通配

使用getArgs()方法获取存到args对象数组中, 通过Arrays.toString()打印

```java
    //JoinPoint：用于描述切入点的对象，必须配置成通知方法中的第一个参数，可用于获取原始方法调用的参数
    //@Before("pt()")
    public void before(JoinPoint jp) {
        Object[] args = jp.getArgs();
        System.out.println(Arrays.toString(args));
        System.out.println("before advice ..." );
    }
```

‍

‍

#### 返回值

只有返回后`AfterReturing`​和环绕`Around`​两个通知类型可以获取

‍

##### 环绕

上面参数获取的`ret`​就是方法的返回值，可以直接获取进行修改

```java
//ProceedingJoinPoint：专用于环绕通知，是JoinPoint子类，可以实现对原始方法的调用
    @Around("pt()")
    public Object around(ProceedingJoinPoint pjp) {
        Object[] args = pjp.getArgs();
        System.out.println(Arrays.toString(args));
        args[0] = 666; //!偷偷改一下...
        Object ret = null;
        try {

            ret = pjp.proceed(args);

        } catch (Throwable t) {
            t.printStackTrace();
        }
        return ret;
    }
```

‍

##### 返回后

‍

方法形参和注解内(,returning = "XX")的形参一致

参数的顺序问题 如果有JoinPoint参数，参数必须要放第一位

```java
    //设置返回后通知获取原始方法的返回值，要求returning属性值必须与方法形参名相同
    @AfterReturning(value = "pt()",returning = "ret")
    public void afterReturning(JoinPoint jp,String ret) {
        System.out.println("afterReturning advice ..."+ret);
    }
```

‍

‍

#### 异常

只有抛出异常后`AfterThrowing`​和环绕`Around`​这两个通知类型可以获取

‍

##### 环绕

只需要将异常捕获就可以获取到原始方法的异常信息

```java
        Object ret = null;
        try{
            ret = pjp.proceed(args);
        }catch(Throwable throwable){
            t.printStackTrace();
        }
        return ret;
    }
```

‍

##### 抛异后

同样和返回值一样注意名称一致问题

```java
@AfterThrowing(value = "pt()",throwing = "t")
    public void afterThrowing(Throwable t) {
        System.out.println("afterThrowing advice ..."+t);
    }
```

‍

‍

### 注开AOP/ApsectJ实例

‍

‍

使用SpringAOP的注解方式完成在方法执行的前打印出当前系统时间

‍

**流程**

1.导入坐标(pom.xml)

2.制作连接点(原始操作，Dao接口与实现类)

3.制作共性功能(通知类与通知)

4.定义切入点

5.绑定切入点与通知关系(切面)

‍

‍

#### 导入坐标

​`spring-context`​中已经导入了`spring-aop`​

导入AspectJ的jar包,AspectJ是AOP思想的一个具体实现，**Spring有自己的AOP实现**，但是相比于AspectJ来说比较麻烦，所以我们直接采用**Spring整合ApsectJ**的方式进行AOP开发

‍

‍

pom.xml AspectJ导入

```xml
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.4</version>
</dependency>
```

‍

‍

#### 定义通知类和通知

通知就是将共性功能抽取出来后形成的方法

```java
public class MyAdvice {
    public void method(){
        System.out.println(System.currentTimeMillis());
    }
}
```

‍

‍

#### 定义切入点

要增强的是update方法, 在MyAdvice中添加无用的方法pt, 写@Pointcut注解

```java
    @Pointcut("execution(void com.itheima.dao.BookDao.update())")
    private void pt(){}
```

* 切入点定义依托一个不具有实际意义的方法进行，即无参数、无返回值、方法体无实际逻辑

‍

‍

#### 制作切面

通过注解进行关系绑定: 绑定切入点与通知关系，并指定通知添加到原始连接点的具体执行==位置==

```java
    @Before("pt()")
    public void method(){
```

通知会在切入点方法执行之前执行

‍

‍

#### 配置容器

**将通知类配给容器并标识其为切面类**

使用Component表示为bean, 同时记录为Aspect对象应用切面

```java
@Component
@Aspect
class...
```

‍

#### 开启AOP

开启注解格式AOP功能

@EnableAspectJAutoProxy

```java
@EnableAspectJAutoProxy
public class SpringConfig {
}
```

‍

#### 运行

基础配置

```java
        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);
        BookDao bookDao = ctx.getBean(BookDao.class);
        bookDao.update();
```

‍

‍

#### 环绕通知HW

测定业务层接口执行效率

‍

**需求分析**

任意业务层接口执行均可显示其执行效率（执行时长）

‍

查看每个业务层执行的时间，这样就可以监控出哪个业务比较耗时，将其查找出来方便优化.

‍

具体实现的思路:

(1) 开始执行方法之前记录一个时间

(2) 执行方法

(3) 执行完方法之后记录一个时间

(4) 用后一个时间减去前一个时间的差值，就是我们需要的结果.

所以要在方法执行的前后添加业务，经过分析我们将采用`环绕通知`​.

**说明:** 原始方法如果只执行一次，时间太快，两个时间差可能为0，所以我们要执行万次来计算时间差.

‍

‍

##### 开启注解

主配置文件SpringConfig类中添加注解

```java
@EnableAspectJAutoProxy
```

‍

##### 创建通知类

* 该类要被Spring管理，需要添加@Component
* 要标识该类是一个AOP的切面类，需要添加@Aspect
* 配置切入点表达式，需要添加一个方法，并添加@Pointcut

‍

‍

##### 添加环绕通知

runSpeed()方法上添加@Around

```java
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public Object runSpeed(ProceedingJoinPoint pjp){
        Object ret = pjp.proceed();
        return ret;
    } 
}
```

‍

‍

##### 完成核心业务

记录万次执行的时间, 区分到底是哪个接口的哪个方法执行的具体时间

‍

==补充说明==

当前测试的接口执行效率仅仅是一个理论值，并不是一次完整的执行过程.

这块只是通过该案例把AOP的使用进行了学习，具体的实际值是有很多因素共同决定的.

‍

完整通知类

```java
@Component
@Aspect
public class ProjectAdvice {
    //配置业务层的所有方法
    @Pointcut("execution(* com.itheima.service.*Service.*(..))")
    private void servicePt(){}
    //@Around("ProjectAdvice.servicePt()") 可以简写为下面的方式
    @Around("servicePt()")
    public void runSpeed(ProceedingJoinPoint pjp){
        //获取执行签名信息
        Signature signature = pjp.getSignature();
        //通过签名获取执行操作名称(接口名)
        String className = signature.getDeclaringTypeName();
        //通过签名获取执行操作名称(方法名)
        String methodName = signature.getName();
  
        long start = System.currentTimeMillis();
        for (int i = 0; i < 10000; i++) {
           pjp.proceed();
        }
        long end = System.currentTimeMillis();
        System.out.println("万次执行："+ className+"."+methodName+"---->" +(end-start) + "ms");
    } 
}
```

‍

‍

‍

‍

## 事务

‍

### 概念

‍

SQL事务作用：在数据层保障一系列的数据库操作同成功同失败

Spring事务作用：在数据层或**==业务层==**​**保障一系列的数据库操作同成功同失败**保障一系列的数据库操作同成功同失败

‍

Spring为了管理事务，提供了一个平台事务管理器`PlatformTransactionManager`​

```java
public interface PlatformTransactionManager{
    void commit( Transactionstatus status) throws TransactionException;
    void rollback(Transactionstatus status) throws TransactionException;
}
```

commit是用来提交事务，rollback是用来回滚事务.

‍

PlatformTransactionManager只是一个接口，Spring还为其提供了一个具体的实现:

```java
public class DataSourceTransactionManager {
}
```

从名称上可以看出，我们只需要给它一个DataSource对象，它就可以帮你去在业务层管理事务. 其内部采用的是JDBC的事务. 所以说如果你持久层采用的是JDBC相关的技术，就可以采用这个事务管理器来管理你的事务. 而Mybatis内部采用的就是JDBC的事务，所以后期我们Spring整合Mybatis就采用的这个DataSourceTransactionManager事务管理器.

‍

### 类型

‍

#### 编程式事务

事务功能的相关操作全部通过自己编写代码来实现：

```java
Connection conn = ...;

try {

    // 开启事务：关闭事务的自动提交
    conn.setAutoCommit(false);

    // 核心操作

    // 提交事务
    conn.commit();

}catch(Exception e) {

    // 回滚事务
    conn.rollBack();

}finally{

    // 释放数据库连接
    conn.close();

}
```

编程式的实现方式存在缺陷：

* 细节没有被屏蔽：具体操作过程中，所有细节都需要程序员自己来完成，比较繁琐.
* 代码复用性不高：如果没有有效抽取出来，每次实现功能都需要自己编写代码，代码就没有得到复用.

‍

#### 声明式事务

既然事务控制的代码有规律可循，代码的结构基本是确定的，所以框架就可以将固定模式的代码抽取出来，进行相关的封装.

封装起来后，我们只需要在配置文件中进行简单的配置即可完成操作.

* 好处1：提高开发效率
* 好处2：消除了冗余的代码
* 好处3：框架会综合考虑相关领域中在实际开发环境下有可能遇到的各种问题，进行了健壮性、性能等各个方面的优化

所以，我们可以总结下面两个概念：

* **编程式**：**自己写代码**实现功能
* **声明式**：通过**配置**让**框架**实现功能

‍

‍

### 注解配置

使用事务注解@Transactional

@Transactional建议写在实现类或实现类的方法上(虽然都可以写)

‍

@Transactional

|名称|@Transactional|
| ------| ----------------------------------------------------------------------------------|
|类型|接口注解 类注解 方法注解|
|位置|业务层接口上方 业务层实现类上方 业务方法上方|
|作用|为当前业务层方法添加事务（如果设置在类或接口上方则类或接口中所有方法均添加事务）|

‍

‍

@EnableTransactionManagement

|名称|@EnableTransactionManagement|
| ------| ----------------------------------------|
|类型|配置类注解|
|位置|配置类定义上方|
|作用|设置当前Spring环境中开启注解式事务支持|

‍

#### 配置管理器

在JdbcConfig类配置事务管理器

根据使用技术进行选择，Mybatis框架使用的是JDBC事务，可以直接使用`DataSourceTransactionManager`​

这里传参传入了dataSource(数据源)对象进行管理

```java
  //配置事务管理器，mybatis使用的是jdbc事务
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
```

‍

‍

#### 配置类开启事务

SpringConfig的配置类中开启事务 `@EnableTransactionManagement`​

‍

```java
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class


@EnableTransactionManagement  //开启注解式事务驱动
 
public class SpringConfig {
...
}
```

‍

### XML配置

‍

#### 配置文件

将Spring配置文件中去掉tx:annotation-driven 标签，并添加配置：

```xml
<aop:config>
    <!-- 配置事务通知和切入点表达式 -->
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.atguigu.spring.tx.xml.service.impl.*.*(..))"></aop:advisor>
</aop:config>
<!-- tx:advice标签：配置事务通知 -->
<!-- id属性：给事务通知标签设置唯一标识，便于引用 -->
<!-- transaction-manager属性：关联事务管理器 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!-- tx:method标签：配置具体的事务方法 -->
        <!-- name属性：指定方法名，可以使用星号代表多个字符 -->
        <tx:method name="get*" read-only="true"/>
        <tx:method name="query*" read-only="true"/>
        <tx:method name="find*" read-only="true"/>

        <!-- read-only属性：设置只读属性 -->
        <!-- rollback-for属性：设置回滚的异常 -->
        <!-- no-rollback-for属性：设置不回滚的异常 -->
        <!-- isolation属性：设置事务的隔离级别 -->
        <!-- timeout属性：设置事务的超时属性 -->
        <!-- propagation属性：设置事务的传播行为 -->
        <tx:method name="save*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
        <tx:method name="update*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
        <tx:method name="delete*" read-only="false" rollback-for="java.lang.Exception" propagation="REQUIRES_NEW"/>
    </tx:attributes>
</tx:advice>
```

‍

基于xml实现的声明式事务，必须引入aspectJ的依赖

‍

‍

### 角色

‍

* 事务管理员：发起事务方，在Spring中通常指代业务层开启事务的方法
* 事务协调员：加入事务方，在Spring中通常指代数据层方法，也可以是业务层方法

> 可以参考一下原子操作和之前学的数据库事务, 了解的挺好了就不在这里赘述了

‍

**原理**

* transfer上添加了@Transactional注解，在该方法上就会有一个事务T
* AccountDao的outMoney方法的事务T1加入到transfer的事务T中
* AccountDao的inMoney方法的事务T2加入到transfer的事务T中
* 这样就保证他们在同一个事务中，当业务层中出现异常，整个事务就会回滚，保证数据的准确性.

‍

‍

‍

==注意==

这里介绍示例的事务管理是基于`DataSourceTransactionManager`​和`SqlSessionFactoryBean`​使用的是同一个数据源

‍

‍

‍

### 配置属性

​`@Transactional`​注解的参数

‍

* readOnly    true只读事务，false读写事务    增删改要设为false,查询设为true.
* timeout    设置超时时间单位秒，在多长时间之内事务没有提交成功就自动回滚，-1表示不设置超时时间.
* rollbackFor    当出现指定异常进行事务回滚 (rollbackFor=NPE.class)
* noRollbackFor    当出现指定异常不进行事务回滚
* rollbackForClassName    等同于rollbackFor,只不过属性为异常的类全名字符串
* noRollbackForClassName    等同于noRollbackFor，只不过属性为异常的类全名字符串
* isolation    设置事务的隔离级别

  * DEFAULT  默认隔离级别, 会采用数据库的隔离级别
  * READ\_UNCOMMITTED  读未提交
  * READ\_COMMITTED  读已提交
  * REPEATABLE\_READ  重复读取
  * SERIALIZABLE  串行化

‍

‍

#### **rollbackFor**

rollbackFor是指定回滚异常，对于异常事务不应该都回滚么? 并不是所有的异常都会回滚事务，比如下面的

```java
public interface AccountService {
    //配置当前接口方法具有事务
    public void transfer(String out,String in ,Double money) throws IOException;
}

@Service
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;
	@Transactional
    public void transfer(String out,String in ,Double money) throws IOException{
        accountDao.outMoney(out,money);
        //int i = 1/0; //这个异常事务会回滚
        if(true){
            throw new IOException(); //这个异常事务就不会回滚
        }
        accountDao.inMoney(in,money);
    }
}
```

‍

出现这个问题的原因是，Spring的事务只会对`Error异常`​和`RuntimeException异常`​及其子类进行事务回顾，**其他的异常类型**是不会回滚的，对应IOException不符合上述条件所以不回滚

‍

测试: 使用rollbackFor属性来设置**出现IOException异常不回滚**

```java
@Service
public class AccountServiceImpl implements AccountService {

    @Autowired
    private AccountDao accountDao;
	 @Transactional(rollbackFor = {IOException.class})
    public void transfer(String out,String in ,Double money) throws IOException{
        accountDao.outMoney(out,money);
        //int i = 1/0; //这个异常事务会回滚
        if(true){
            throw new IOException(); //这个异常事务就不会回滚
        }
        accountDao.inMoney(in,money);
    }

}
```

‍

**rollbackFor**

默认情况下，只有出现RuntimeException(运行时异常)才会回滚事务,配置@Transactional注解当中的rollbackFor属性指定出现何种异常类型回滚事务

​`@Transactional(rollbackFor=Exception.class)`​

‍

‍

### 传播行为

‍

概念

当一个事务方法被另一个事务方法调用时，这个事务方法应该如何进行事务控制(我的理解: 管理事务是否加入当前存在事务的行为)

‍

‍

**修改示例**

Service实现中方法 + `@Transactional(propagation = Propagation.REQUIRES_NEW)`​

```java
@Service
public class LogServiceImpl implements LogService {

    @Autowired
    private LogDao logDao;

	//propagation设置事务属性：传播行为设置为当前操作需要新事务
    @Transactional(propagation = Propagation.REQUIRES_NEW)
    public void log(String out,String in,Double money ) {
        logDao.log("转账操作由"+out+"到"+in+",金额："+money);
    }
}
```

‍

‍

#### 类型

‍

常见的传播行为

* REQUIRED ：大部分情况下都是用该传播行为即可, 默认是全部加入到一个事务之中,一个寄了,各个都寄了.
* REQUIRES_NEW ：当我们不希望事务之间相互影响时，可以使用该传播行为. 比如：下订单前需要记录日志，不论订单保存成功与否，都需要保证日志记录能够记录成功.

|**属性值**|**含义**|
| ---------------| --------------------------------------------------------------------|
|**REQUIRED**|【默认值】需要事务，有则加入，无则创建新事务|
|**REQUIRES_NEW**|需要新事务，无论有无，总是创建新事务|
|SUPPORTS|支持事务，有则加入，无则在无事务状态中运行|
|NOT_SUPPORTED|不支持事务，在无事务状态下运行,如果当前存在已有事务,则挂起当前事务|
|MANDATORY|必须有事务，否则抛异常|
|NEVER|必须没事务，否则抛异常|

‍

一共有七种传播行为

* ​`REQUIRED`​：支持当前事务，如果不存在就新建一个(默认)  **【没有就新建，有就加入】**
* SUPPORTS：支持当前事务，如果当前没有事务，就以非事务方式执行  **【有就加入，没有就不管了】**
* MANDATORY：必须运行在一个事务中，如果当前没有事务正在发生，将抛出一个异常  **【有就加入，没有就抛异常】**
* ​`REQUIRES_NEW`​：开启一个新的事务，如果一个事务已经存在，则将这个存在的事务挂起  **【不管有没有，直接开启一个新事务，开启的新事务和之前的事务不存在嵌套关系，之前事务被挂起】**
* NOT_SUPPORTED：以非事务方式运行，如果有事务存在，挂起当前事务  **【不支持事务，存在就挂起】**
* NEVER：以非事务方式运行，如果有事务存在，抛出异常  **【不支持事务，存在就抛异常】**
* NESTED：如果当前正有一个事务在进行中，则该方法应当运行在一个嵌套式事务中. 被嵌套的事务可以独立于外层事务进行提交或回滚. 如果外层事务不存在，行为就像REQUIRED一样.  **【有事务的话，就在这个事务里再嵌套一个完全独立的事务，嵌套的事务可以独立的提交和回滚. 没有事务就和REQUIRED一样. 】**

‍

‍

* Propagation.REQUIRED（required）：支持当前事务.   如果当前有事务那么**加入**. 如果当前没有事务则新建一个(默认情况)
* Propagation.NOT_SUPPORTED（not_supported) ： 以非事务方式执行操作   如果当前存在事务就把当前事务挂起，执行完后恢复事务（忽略当前事务）
* Propagation.SUPPORTS (supports) ：如果当前有事务则加入，如果没有则不用事务
* Propagation.MANDATORY (mandatory) ：支持当前事务，如果当前没有事务，则抛出异常. （当前必须有事务）
* PROPAGATION_NEVER (never) ：以非事务方式执行，如果当前存在事务，则抛出异常. （当前必须不能有事务）
* Propagation.REQUIRES_NEW (requires_new) ：支持当前事务，如果当前有事务，则**挂起当前事务**，然后新创建一个事务，如果当前没有事务，则自己创建一个事务
* Propagation.NESTED (nested 嵌套事务) ：如果当前存在事务，则嵌套在当前事务中.  如果当前没有事务，则新建一个事务自己执行（和required一样）. 嵌套的事务使用保存点作为回滚点，当内部事务回滚时不会影响外部事物的提交；但是外部回滚会把内部事务一起回滚回去. （这个和新建一个事务的区别）

‍

**支持当前事务**的情况：* TransactionDefinition.PROPAGATION_REQUIRED： 如果当前存在事务则**加入该事务**；如果当前没有事务则创建一个新的事务

* 内外层是相同的事务，在 aMethod 或者在 bMethod 内的任何地方出现异常，事务都会被回滚
* 工作流程：

  * 线程执行到 serviceA.aMethod() 时，其实是执行的代理 serviceA 对象的 aMethod
  * 首先执行事务增强器逻辑（环绕增强），提取事务标签属性，检查当前线程是否绑定 connection 数据库连接资源，没有就调用 datasource.getConnection()，设置事务提交为手动提交 autocommit(false)
  * 执行其他增强器的逻辑，然后调用 target 的目标方法 aMethod() 方法，进入 serviceB 的逻辑
  * serviceB 也是先执行事务增强器的逻辑，提取事务标签属性，但此时会检查到线程绑定了 connection，检查注解的传播属性，所以调用 DataSourceUtils.getConnection(datasource) 共享该连接资源，执行完相关的增强和 SQL 后，发现事务并不是当前方法开启的，可以直接返回上层
  * serviceA.aMethod() 继续执行，执行完增强后进行提交事务或回滚事务
* TransactionDefinition.PROPAGATION_SUPPORTS： 如果当前存在事务，则**加入该事务**；如果当前没有事务，则以非事务的方式继续运行
* TransactionDefinition.PROPAGATION_MANDATORY： 如果当前存在事务，则**加入该事务**；如果当前没有事务，则抛出异常

**不支持当前事务**的情况：* TransactionDefinition.PROPAGATION_REQUIRES_NEW： 创建一个新的事务，如果当前存在事务，则把当前事务挂起

* 内外层是不同的事务，如果 bMethod 已经提交，如果 aMethod 失败回滚 ，bMethod 不会回滚
* 如果 bMethod 失败回滚，ServiceB 抛出的异常被 ServiceA 捕获，如果 B 抛出的异常是 A 会回滚的异常，aMethod 事务需要回滚，否则仍然可以提交
* TransactionDefinition.PROPAGATION_NOT_SUPPORTED： **以非事务方式运行**，如果当前存在事务，则把当前事务挂起
* TransactionDefinition.PROPAGATION_NEVER： **以非事务方式运行**，如果当前存在事务，则抛出异常

其他情况：* TransactionDefinition.PROPAGATION_NESTED： 如果当前存在事务，则创建一个事务作为当前事务的嵌套事务（两个事务没有关系）来运行

* 如果 ServiceB 异常回滚，可以通过 try-catch 机制执行 ServiceC
* 如果 ServiceB 提交， ServiceA 可以根据具体的配置决定是 commit 还是 rollback
* **应用场景**：在查询数据的时候要向数据库中存储一些日志，系统不希望存日志的行为影响到主逻辑，可以使用该传播

requied：必须的、supports：支持的、mandatory：强制的、nested：嵌套的

‍

‍

### 只读属性

‍

对一个查询操作来说，如果我们把它设置成只读，就能够明确告诉数据库，这个操作不涉及写操作. 这样数据库就能够针对查询操作来进行优化.

‍

对于只有读取数据查询的事务，可以指定事务类型为 readonly，即只读事务；只读事务不涉及数据的修改，数据库会提供一些优化手段，适合用在有多条数据库查询操作的方法中

读操作为什么需要启用事务支持：* MySQL 默认对每一个新建立的连接都启用了 `autocommit`​ 模式，在该模式下，每一个发送到 MySQL 服务器的 SQL 语句都会在一个**单独**的事务中进行处理，执行结束后会自动提交事务，并开启一个新的事务

* 执行多条查询语句，如果方法加上了 `@Transactional`​ 注解，这个方法执行的所有 SQL 会被放在一个事务中，如果声明了只读事务的话，数据库就会去优化它的执行，并不会带来其他的收益。如果不加 `@Transactional`​，每条 SQL 会开启一个单独的事务，中间被其它事务修改了数据，比如在前条 SQL 查询之后，后条 SQL 查询之前，数据被其他用户改变，则这次整体的统计查询将会出**现读数据不一致的状态**

‍

### 超时

‍

事务在执行过程中，有可能因为遇到某些问题，导致程序卡住，从而长时间占用数据库资源. 而长时间占用资源，大概率是因为程序运行出现了问题（可能是Java程序或MySQL数据库或网络连接等等）. 此时这个很可能出问题的程序应该被回滚，撤销它已做的操作，事务结束，把资源让出来，让其他正常程序可以执行.

概括来说就是一句话：超时回滚，释放资源.

‍

‍

### 回滚策略

‍

声明式事务默认只针对运行时异常回滚，编译时异常不回滚.

‍

可以通过@Transactional中相关属性设置回滚策略

* rollbackFor 属性：需要设置一个Class类型的对象
* rollbackForClassName 属性：需要设置一个字符串类型的全类名
* noRollbackFor 属性：需要设置一个Class类型的对象
* noRollbackForClassName 属性：需要设置一个字符串类型的全类名

‍

‍

### 隔离级别

‍

数据库系统必须具有隔离并发运行各个事务的能力，使它们不会相互影响，避免各种并发问题. 一个事务与其他事务隔离的程度称为隔离级别. SQL标准中规定了多种事务隔离级别，不同隔离级别对应不同的干扰程度，隔离级别越高，数据一致性就越好，但并发性越弱.

‍

隔离级别一共有四种

* 读未提交：READ UNCOMMITTED  
  允许Transaction01读取Transaction02未提交的修改.
* 读已提交：READ COMMITTED、  
  要求Transaction01只能读取Transaction02已提交的修改.
* 可重复读：REPEATABLE READ  
  确保Transaction01可以多次从一个字段中读取到相同的值，即Transaction01执行期间禁止其它事务对这个字段进行更新.
* 串行化：SERIALIZABLE  
  确保Transaction01可以多次从一个表中读取到相同的行，在Transaction01执行期间，禁止其它事务对这个表进行添加、更新、删除操作. 可以避免任何并发问题，但性能十分低下.

‍

各个隔离级别解决并发问题的能力

|隔离级别|脏读|不可重复读|幻读|
| ------------------| ------| ------------| ------|
|READ UNCOMMITTED|有|有|有|
|READ COMMITTED|无|有|有|
|REPEATABLE READ|无|无|有|
|SERIALIZABLE|无|无|无|

各种数据库产品对事务隔离级别的支持程度

|隔离级别|Oracle|MySQL|
| ------------------| ----------| ----------|
|READ UNCOMMITTED|×|√|
|READ COMMITTED|√(默认)|√|
|REPEATABLE READ|×|√(默认)|
|SERIALIZABLE|√|√|

‍

#### 使用

```java
@Transactional(isolation = Isolation.DEFAULT)//使用数据库默认的隔离级别
@Transactional(isolation = Isolation.READ_UNCOMMITTED)//读未提交
@Transactional(isolation = Isolation.READ_COMMITTED)//读已提交
@Transactional(isolation = Isolation.REPEATABLE_READ)//可重复读
@Transactional(isolation = Isolation.SERIALIZABLE)//串行化
```

‍

‍

### 事务示例

需求

实现任意两个账户间转账操作

‍

需求微缩: A账户减钱，B账户加钱

为了实现上述的业务需求，我们可以按照下面步骤来实现下:  
①：数据层提供基础操作，指定账户减钱（outMoney），指定账户加钱（inMoney）

②：业务层提供转账操作（transfer），调用减钱与加钱的操作

③：提供2个账号和操作金额执行转账操作

④：基于Spring整合MyBatis环境搭建上述操作

‍

数据库表

```java
create database spring_db character set utf8;
use spring_db;
create table tbl_account(
    id int primary key auto_increment,
    name varchar(35),
    money double
);
insert into tbl_account values(1,'Tom',1000);
insert into tbl_account values(2,'Jerry',1000);
```

‍

#### **注解**

被事务管理的方法上添加注解

```java
    @Transactional
    public void transfer(String out,String in ,Double money) {
        accountDao.outMoney(out,money);
        int i = 1/0;
        accountDao.inMoney(in,money);
    }
```

‍

#### 配置事务管理器

这里在JdbcConfig类配置事务管理器

根据使用技术进行选择，Mybatis框架使用的是JDBC事务，可以直接使用`DataSourceTransactionManager`​

这里传参传入了dataSource(数据源)对象进行管理

```java
  //配置事务管理器，mybatis使用的是jdbc事务
    @Bean
    public PlatformTransactionManager transactionManager(DataSource dataSource){
        DataSourceTransactionManager transactionManager = new DataSourceTransactionManager();
        transactionManager.setDataSource(dataSource);
        return transactionManager;
    }
```

‍

‍

#### 开启事务注解

SpringConfig的配置类中开启

​`@EnableTransactionManagement`​

```java
@Configuration
@ComponentScan("com.itheima")
@PropertySource("classpath:jdbc.properties")
@Import({JdbcConfig.class,MybatisConfig.class


@EnableTransactionManagement  //开启注解式事务驱动
 
public class SpringConfig {
...
}
```

‍

‍

### 编程式实现

‍

#### 控制方式

编程式、声明式（XML）、声明式（注解）

‍

#### 环境准备

银行转账业务

* 包装类

  ```java
  public class Account implements Serializable {
      private Integer id;
      private String name;
      private Double money;
      .....
  }
  ```
* DAO层接口：AccountDao

  ```java
  public interface AccountDao {
      //入账操作	name:入账用户名	money:入账金额
      void inMoney(@Param("name") String name, @Param("money") Double money);

      //出账操作	name:出账用户名	money:出账金额
      void outMoney(@Param("name") String name, @Param("money") Double money);
  }
  ```
* 业务层接口提供转账操作：AccountService

  ```java
  public interface AccountService {
  	//转账操作	outName:出账用户名	inName:入账用户名	money:转账金额
  	public void transfer(String outName,String inName,Double money);
  }
  ```
* 业务层实现提供转账操作：AccountServiceImpl

  ```java
  public class AccountServiceImpl implements AccountService {
      private AccountDao accountDao;
      public void setAccountDao(AccountDao accountDao) {
          this.accountDao = accountDao;
      }
      @Override
      public void transfer(String outName,String inName,Double money){
  		accountDao.inMoney(outName,money);
          accountDao.outMoney(inName,money);
  	}
  }
  ```
* 映射配置文件：dao / AccountDao.xml

  ```xml
  <mapper namespace="dao.AccountDao">
      <update id="inMoney">
          UPDATE account SET money = money + #{money} WHERE name = #{name}
      </update>

      <update id="outMoney">
          UPDATE account SET money = money - #{money} WHERE name = #{name}
      </update>
  </mapper>
  ```
* jdbc.properties

  ```properties
  jdbc.driver=com.mysql.jdbc.Driver
  jdbc.url=jdbc:mysql://192.168.2.185:3306/spring_db
  jdbc.username=root
  jdbc.password=1234
  ```
* 核心配置文件：applicationContext.xml

  ```xml
  <context:property-placeholder location="classpath:*.properties"/>

  <bean id="dataSource" class="com.alibaba.druid.pool.DruidDataSource">
      <property name="driverClassName" value="${jdbc.driver}"/>
      <property name="url" value="${jdbc.url}"/>
      <property name="username" value="${jdbc.username}"/>
      <property name="password" value="${jdbc.password}"/>
  </bean>

  <bean id="accountService" class="service.impl.AccountServiceImpl">
      <property name="accountDao" ref="accountDao"/>
  </bean>

  <bean class="org.mybatis.spring.SqlSessionFactoryBean">
      <property name="dataSource" ref="dataSource"/>
      <property name="typeAliasesPackage" value="domain"/>
  </bean>
  <!--扫描映射配置和Dao-->
  <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
      <property name="basePackage" value="dao"/>
  </bean>
  ```
* 测试类

  ```java
  ApplicationContext ctx = new ClassPathXmlApplicationContext("ap...xml");
  AccountService accountService = (AccountService) ctx.getBean("accountService");
  accountService.transfer("Jock1", "Jock2", 100d);
  ```

‍

#### 编程式

编程式事务就是代码显式的给出事务的开启和提交* 修改业务层实现提供转账操作：AccountServiceImpl

```java
  public void transfer(String outName,String inName,Double money){
      //1.创建事务管理器，
      DataSourceTransactionManager dstm = new DataSourceTransactionManager();
      //2.为事务管理器设置与数据层相同的数据源
      dstm.setDataSource(dataSource);
      //3.创建事务定义对象
      TransactionDefinition td = new DefaultTransactionDefinition();
      //4.创建事务状态对象，用于控制事务执行，【开启事务】
      TransactionStatus ts = dstm.getTransaction(td);
      accountDao.inMoney(inName,money);
      int i = 1/0;    //模拟业务层事务过程中出现错误
      accountDao.outMoney(outName,money);
      //5.提交事务
      dstm.commit(ts);
  }
```

* 配置 applicationContext.xml

  ```xml
  <!--添加属性注入-->
  <bean id="accountService" class="service.impl.AccountServiceImpl">
      <property name="accountDao" ref="accountDao"/>
      <property name="dataSource" ref="dataSource"/>
  </bean>
  ```

---

#### AOP改造

* 将业务层的事务处理功能抽取出来制作成 AOP 通知，利用环绕通知运行期动态织入

```java
  public class TxAdvice {
      private DataSource dataSource;
      public void setDataSource(DataSource dataSource) {
          this.dataSource = dataSource;
      }

      public Object tx(ProceedingJoinPoint pjp) throws Throwable {
          //开启事务
          PlatformTransactionManager ptm = new DataSourceTransactionManager(dataSource);
          //事务定义
          TransactionDefinition td = new DefaultTransactionDefinition();
          //事务状态
          TransactionStatus ts =  ptm.getTransaction(td);
          //pjp.getArgs()标准写法，也可以不加，同样可以传递参数
          Object ret = pjp.proceed(pjp.getArgs());
  
          //提交事务
          ptm.commit(ts);

          return ret;
      }
  }
```

* 配置 applicationContext.xml，要开启 AOP 空间

  ```xml
  <!--修改bean的属性注入-->
  <bean id="accountService" class="service.impl.AccountServiceImpl">
      <property name="accountDao" ref="accountDao"/>
  </bean>

  <!--配置AOP通知类，并注入dataSource-->
  <bean id="txAdvice" class="aop.TxAdvice">
      <property name="dataSource" ref="dataSource"/>
  </bean>

  <!--使用环绕通知将通知类织入到原始业务对象执行过程中-->
  <aop:config>
      <aop:pointcut id="pt" expression="execution(* *..transfer(..))"/>
      <aop:aspect ref="txAdvice">
          <aop:around method="tx" pointcut-ref="pt"/>
      </aop:aspect>
  </aop:config>
  ```
* 修改业务层实现提供转账操作：AccountServiceImpl

  ```java
  public class AccountServiceImpl implements AccountService {
      private AccountDao accountDao;
      public void setAccountDao(AccountDao accountDao) {
          this.accountDao = accountDao;
      }
      @Override
      public void transfer(String outName,String inName,Double money){
  		accountDao.inMoney(outName,money);
          //int i = 1 / 0;
          accountDao.outMoney(inName,money);
  	}
  }
  ```

‍

‍

### 声明式实现

‍

#### XML

##### tx使用

删除 TxAdvice 通知类，开启 tx 命名空间，配置 applicationContext.xml

```xml
<!--配置平台事务管理器-->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>

<!--定义事务管理的通知类-->
<tx:advice id="txAdvice" transaction-manager="txManager">
    <!--定义控制的事务-->
    <tx:attributes>
        <tx:method name="transfer" read-only="false"/>
    </tx:attributes>
</tx:advice>

<!--使用aop:advisor在AOP配置中引用事务专属通知类，底层invoke调用-->
<aop:config>
    <aop:pointcut id="pt" expression="execution(* service.*Service.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="pt"/>
</aop:config>
```

pom.xml 文件引入依赖：

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.1.9.RELEASE</version>
</dependency>
```

---

##### tx配置

‍

###### advice

标签：tx:advice，beans 的子标签

作用：专用于声明事务通知

格式：

```xml
<beans>
    <tx:advice id="txAdvice" transaction-manager="txManager">
    </tx:advice>
</beans>
```

基本属性：

* id：用于配置 aop 时指定通知器的 id
* transaction-manager：指定事务管理器 bean

‍

###### attributes

类型：tx:attributes，tx:advice 的子标签

作用：定义通知属性

格式：

```xml
<tx:advice id="txAdvice" transaction-manager="txManager">
    <tx:attributes>
    </tx:attributes>
</tx:advice>
```

###### method

标签：tx:method，tx:attribute 的子标签

作用：设置具体的事务属性

格式：

```xml
<tx:attributes>
    <!--标准格式-->
    <tx:method name="*" read-only="false"/>
    <tx:method name="get*" read-only="true"/>
    <tx:method name="find*" read-only="true"/>
</tx:attributes>
<aop:pointcut id="pt" expression="execution(* service.*Service.*(..))"/><!--标准-->
```

说明：通常事务属性会配置多个，包含 1 个读写的全事务属性，1 个只读的查询类事务属性

属性：

* name：待添加事务的方法名表达式（支持 * 通配符）
* read-only：设置事务的读写属性，true 为只读，false 为读写
* timeout：设置事务的超时时长，单位秒，-1 为无限长
* isolation：设置事务的隔离界别，该隔离级设定是基于 Spring 的设定，非数据库端
* no-rollback-for：设置事务中不回滚的异常，多个异常使用 `,`​ 分隔
* rollback-for：设置事务中必回滚的异常，多个异常使用 `,`​ 分隔
* propagation：设置事务的传播行为

‍

#### 注解

##### 开启注解

###### XML

标签：tx:annotation-driven

归属：beans 标签

作用：开启事务注解驱动，并指定对应的事务管理器

范例：

```xml
<tx:annotation-driven transaction-manager="txManager"/>
```

---

###### 纯注解

名称：@EnableTransactionManagement

类型：类注解，Spring 注解配置类上方

作用：开启注解驱动，等同 XML 格式中的注解驱动

范例：

```java
@Configuration
@ComponentScan("com.seazean")
@PropertySource("classpath:jdbc.properties")
@Import({JDBCConfig.class,MyBatisConfig.class,TransactionManagerConfig.class})
@EnableTransactionManagement
public class SpringConfig {
}
```

```java
public class TransactionManagerConfig {
    @Bean												//自动装配
    public PlatformTransactionManager getTransactionManager(@Autowired DataSource dataSource){
        return new DataSourceTransactionManager(dataSource);
    }
}
```

---

##### 配置注解

名称：@Transactional

类型：方法注解，类注解，接口注解

作用：设置当前类/接口中所有方法或具体方法开启事务，并指定相关事务属性

范例：

```java
@Transactional(
    readOnly = false,
    timeout = -1,
    isolation = Isolation.DEFAULT,
    rollbackFor = {ArithmeticException.class, IOException.class},
    noRollbackFor = {},
    propagation = Propagation.REQUIRES_NEW
)
public void addAccount{} 
```

说明：

* ​`@Transactional`​ 注解只有作用到 public 方法上事务才生效
* 不推荐在接口上使用 `@Transactional`​ 注解  
  原因：在接口上使用注解，**只有在使用基于接口的代理（JDK）时才会生效，因为注解是不能继承的**，这就意味着如果正在使用基于类的代理（CGLIB）时，那么事务的设置将不能被基于类的代理所识别
* 正确的设置 `@Transactional`​ 的 rollbackFor 和 propagation 属性，否则事务可能会回滚失败
* 默认情况下，事务只有遇到运行期异常 和 Error 会导致事务回滚，但是在遇到检查型（Checked）异常时不会回滚

  * 继承自 RuntimeException 或 error 的是非检查型异常，比如空指针和索引越界，而继承自 Exception 的则是检查型异常，比如 IOException、ClassNotFoundException，RuntimeException 本身继承 Exception
  * 非检查型类异常可以不用捕获，而检查型异常则必须用 try 语句块把异常交给上级方法，这样事务才能有效

‍

‍

**事务不生效的问题**

* 情况 1：确认创建的 MySQL 数据库表引擎是 InnoDB，MyISAM 不支持事务
* 情况 2：注解到 protected，private 方法上事务不生效，但不会报错  
  原因：理论上而言，不用 public 修饰，也可以用 aop 实现事务的功能，但是方法私有化让其他业务无法调用  
  AopUtils.canApply：`methodMatcher.matches(method, targetClass) --true--> return true`​  
  ​`TransactionAttributeSourcePointcut.matches()`​ ，AbstractFallbackTransactionAttributeSource 中 getTransactionAttribute 方法调用了其本身的 computeTransactionAttribute 方法，当加了事务注解的方法不是 public 时，该方法直接返回 null，所以造成增强不匹配

  ```java
  private TransactionAttribute computeTransactionAttribute(Method method, Class<?> targetClass) {
      // Don't allow no-public methods as required.
      if (allowPublicMethodsOnly() && !Modifier.isPublic(method.getModifiers())) {
          return null;
      }
  }
  ```
* 情况 3：注解所在的类没有被加载成 Bean
* 情况 4：在业务层捕捉异常后未向上抛出，事务不生效  
  原因：在业务层捕捉并处理了异常（try..catch）等于把异常处理掉了，Spring 就不知道这里有错，也不会主动去回滚数据，推荐做法是在业务层统一抛出异常，然后在控制层统一处理
* 情况 5：遇到检测异常时，也无法回滚  
  原因：Spring 的默认的事务规则是遇到运行异常（RuntimeException）和程序错误（Error）才会回滚。想针对检测异常进行事务回滚，可以在 @Transactional 注解里使用 rollbackFor 属性明确指定异常
* 情况 6：Spring 的事务传播策略在**内部方法**调用时将不起作用，在一个 Service 内部，事务方法之间的嵌套调用，普通方法和事务方法之间的嵌套调用，都不会开启新的事务，事务注解要加到调用方法上才生效  
  原因：Spring 的事务都是使用 AOP 代理的模式，动态代理 invoke 后会调用原始对象，而原始对象在去调用方法时是不会触发拦截器，就是**一个方法调用本对象的另一个方法**，所以事务也就无法生效

  ```java
  @Transactional
  public int add(){
      update();
  }
  //注解添加在update方法上无效，需要添加到add()方法上
  public int update(){}
  ```
* 情况 7：注解在接口上，代理对象是 CGLIB

---

‍

##### 使用注解

* Dao 层

```java
  public interface AccountDao {
      @Update("update account set money = money + #{money} where name = #{name}")
      void inMoney(@Param("name") String name, @Param("money") Double money);

      @Update("update account set money = money - #{money} where name = #{name}")
      void outMoney(@Param("name") String name, @Param("money") Double money);
  }
```

* 业务层

  ```java
  public interface AccountService {
      //对当前方法添加事务，该配置将替换接口的配置
      @Transactional(
          readOnly = false,
          timeout = -1,
          isolation = Isolation.DEFAULT,
          rollbackFor = {},//java.lang.ArithmeticException.class, IOException.class
          noRollbackFor = {},
          propagation = Propagation.REQUIRED
          )
      public void transfer(String outName, String inName, Double money);
  }
  ```

  ```java
  public class AccountServiceImpl implements AccountService {
      @Autowired
      private AccountDao accountDao;
      public void transfer(String outName, String inName, Double money) {
          accountDao.inMoney(outName,money);
          //int i = 1/0;
          accountDao.outMoney(inName,money);
      }
  }
  ```
* 添加文件 Spring.config、Mybatis.config、JDBCConfig (参考ioc_Mybatis)、TransactionManagerConfig

  ```java
  @Configuration
  @ComponentScan({"","",""})
  @PropertySource("classpath:jdbc.properties")
  @Import({JDBCConfig.class,MyBatisConfig.class})
  @EnableTransactionManagement
  public class SpringConfig {
  }
  ```

‍

‍

## 模板对象

Spring 模板对象：TransactionTemplate、JdbcTemplate、RedisTemplate、RabbitTemplate、JmsTemplate、HibernateTemplate、RestTemplate* JdbcTemplate：提供标准的 sql 语句操作API

* NamedParameterJdbcTemplate：提供标准的具名 sql 语句操作API
* RedisTemplate

  * ```java
      public void changeMoney(Integer id, Double money) {
          redisTemplate.opsForValue().set("account:id:"+id,money);
      }
      public Double findMondyById(Integer id) {
          Object money = redisTemplate.opsForValue().get("account:id:" + id);
          return new Double(money.toString());
      }
    ```

‍

‍

## 整合

‍

‍

‍

### 整合环境配置

‍

**MySQL数据表**

```sql
create database spring_db character set utf8;
use spring_db;
create table tbl_account(
    id int primary key auto_increment,
    name varchar(35),
    money double
);
```

‍

**依赖管理**

添加Druid, Mybatis, Mysql坐标

```xml
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>druid</artifactId>
        <version>1.1.16</version>
    </dependency>

    <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>3.5.6</version>
    </dependency>

    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
        <version>5.1.47</version>
    </dependency>
```

‍

**模型类**

(生成POJO)

```java
public class Account implements Serializable {
    private Integer id;
    private String name;
    private Double money;
	//setter...getter...toString...方法略  
}
```

‍

**Dao接口**

注解调用Mybatis操作

增删改查(全,单)

```java
public interface AccountDao {

    @Insert("insert into tbl_account(name,money)values(#{name},#{money})")
    void save(Account account);

    @Delete("delete from tbl_account where id = #{id} ")
    void delete(Integer id);

    @Update("update tbl_account set name = #{name} , money = #{money} where id = #{id} ")
    void update(Account account);

    @Select("select * from tbl_account")
    List<Account> findAll();

    @Select("select * from tbl_account where id = #{id} ")
    Account findById(Integer id);
}
```

‍

**Service接口**

**Service实现类**

直接调用DAO的增删改查

```java
public interface AccountService {
    void save(Account account);
    void delete(Integer id);
    void update(Account account);
    List<Account> findAll();
    Account findById(Integer id);
}
```

```java
@Service
public class AccountServiceImpl implements AccountService {
    @Autowired
    private AccountDao accountDao;

    public void save(Account account) {
        accountDao.save(account);
    }
......
}
```

‍

**jdbc.properties**

resources目录, 配置数据库连接四要素

useSSL=false   关闭MySQL的SSL连接

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/spring_db?useSSL=false
jdbc.username=root
jdbc.password=2333
```

‍

‍

### Mybatis

‍

‍

#### **核心**配置

‍

整合Mybatis，就是将Mybatis用到的内容交给Spring管理

‍

配置文件层次

1. 初始化属性数据
2. 初始化类型别名
3. 初始化DataSource
4. 初始化映射配置

```xml
<configuration>

    <!--1读取外部properties配置文件,初始化属性数据-->
    <properties resource="jdbc.properties"></properties>

    <!--2别名扫描的包路径;初始化类型别名-->
    <typeAliases>
        <package name="com.itheima.domain"/>
    </typeAliases>

    <!--3数据源;初始化DataSource-->
    <environments default="mysql">
        <environment id="mysql">
            <transactionManager type="JDBC"></transactionManager>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"></property>
                <property name="url" value="${jdbc.url}"></property>
                <property name="username" value="${jdbc.username}"></property>
                <property name="password" value="${jdbc.password}"></property>
            </dataSource>
        </environment>
    </environments>


    <!--4映射文件扫描包路径-->
    <mappers>
        <package name="com.itheima.dao"></package>
    </mappers>

</configuration>
```

> 1. 第一注释 读取外部properties配置文件，Spring有提供具体的解决方案`@PropertySource`​,需要交给Spring
> 2. 第二行起别名包扫描，为SqlSessionFactory服务的，需要交给Spring
> 3. 第三行主要用于做连接池，已经整合了Druid连接池，需要交给Spring
> 4. 前面三行一起都是为了创建SqlSession对象用的，而SqlSession是由SqlSessionFactory创建出来的，只需要将Factory交给Spring管理即可
> 5. 第四行是Mapper接口和映射文件[如果使用注解就没有该映射文件]，这个是在获取到SqlSession以后执行具体操作的时候用，所以它和SqlSessionFactory创建的时机都不在同一个时间，可能需要单独管理.

‍

‍

#### 初步实现

编写应用(也就是启动类)

Mybatis程序核心对象分析: 真正需要交给Spring管理的是==SqlSessionFactory==

‍

App这里有几个层次

1. 初始化
2. 获取连接, 获取实现
3. 获取数据层接口
4. 关闭连接

‍

手动实现, 非常繁琐

```java
public class App {
    public static void main(String[] args) throws IOException {

        // 1. 创建SqlSessionFactoryBuilder对象        初始化!
        SqlSessionFactoryBuilder sqlSessionFactoryBuilder = new SqlSessionFactoryBuilder();

        // 2. 加载SqlMapConfig.xml配置文件到输入流对象
        InputStream inputStream = Resources.getResourceAsStream("SqlMapConfig.xml");

        // 3. 利用inputStream输入流和工厂建造者对象创建工厂SqlSessionFactory对象(核心对象)
        SqlSessionFactory sqlSessionFactory = sqlSessionFactoryBuilder.build(inputStream);

        // 4. 通过工厂openSession方法获取SqlSession高级接口对象
        SqlSession sqlSession = sqlSessionFactory.openSession();

        // 5. 执行sqlSession对象的查询方法，传入Dao类对象, 调用对应Dao的方法获取结果POJO(Account类)
        AccountDao accountDao = sqlSession.getMapper(AccountDao.class);
        Account ac = accountDao.findById(1);        //获取数据层接口, 调用方法生成POJO
  
        ...打印...

        // 6. 手动释放资源, 关闭连接
        sqlSession.close();
    }
}
```

‍

‍

#### 注解优化

Spring要管理两个东西:

* MyBatis中的SqlSessionFactory
* Mapper接口的扫描

‍

主要用到的两个类分别是

* ==SqlSessionFactoryBean==
* ==MapperScannerConfigurer==

‍

##### **导入坐标**

由于M/MP并未被收录到idea的系统内置配置，无法直接选择加入，需要手动在pom.xml中配置添加

spring-jdbc mybatis-spring

```xml
<dependency>
    <!--Spring操作数据库需要该jar包-->
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.10.RELEASE</version>
</dependency>

<dependency>
    <!--
		Spring与Mybatis整合的jar包
		这个jar包mybatis在前面，是Mybatis提供的
	-->
    <groupId>org.mybatis</groupId>
    <artifactId>mybatis-spring</artifactId>
    <version>1.3.0</version>
</dependency>
```

‍

‍

##### 数据源配置类

‍

配置类包 >>数据源配置类JdbcConfig

采用@Value(" ${ } ")注解注入对应成员变量, 利用成员变量完成DataSource中四要素的注入

```java
public class JdbcConfig {
    @Value("${jdbc.driver}")
    private String driver;
    @Value("${jdbc.url}")
    private String url;
    @Value("${jdbc.username}")
    private String userName;
    @Value("${jdbc.password}")
    private String password;

    @Bean
    public DataSource dataSource(){
        DruidDataSource ds = new DruidDataSource();

        ds.setDriverClassName(driver);
        ds.setUrl(url);
        ds.setUsername(userName);
        ds.setPassword(password);

        return ds;
    }
}
```

‍

##### 主配置类

读properties

引入数据源配置类 + Mybatis配置类

```java
@Configuration //配置标识
@ComponentScan("com.itheima") //扫描

@PropertySource("classpath:jdbc.properties") //读配置
@Import({JdbcConfig.class,MybatisConfig.class}) //引入其他配置类

public class SpringConfig {
}
```

‍

‍

##### Mb配置类

​`public class MybatisConfig {}`​中, 配置两个Bean

‍

使用SqlSessionFactoryBean封装SqlSessionFactory需要的环境信息

```java
    //定义bean，SqlSessionFactoryBean，用于产生SqlSessionFactory对象
    @Bean
    public SqlSessionFactoryBean sqlSessionFactory(DataSource dataSource){
        SqlSessionFactoryBean ssfb = new SqlSessionFactoryBean();

        //设置模型类的别名扫描
        ssfb.setTypeAliasesPackage("com.itheima.domain");

        //设置数据源
        ssfb.setDataSource(dataSource);

        return ssfb;
    }
```

* SqlSessionFactoryBean是FactoryBean的子类

  在该类中将SqlSessionFactory的创建进行了封装，简化对象的创建，只需要将其需要的内容设置即可
* 方法参数为dataSource,当前Spring容器中已经创建了Druid数据源，类型刚好是DataSource类型

  在初始化SqlSessionFactoryBean这个对象的时候，发现需要使用DataSource对象而容器中刚好有，就自动加载导入了(上面的特性)

‍

‍

使用MapperScannerConfigurer加载Dao接口，创建代理对象保存到IOC容器中

```java
    //定义bean，返回MapperScannerConfigurer对象
    @Bean
    public MapperScannerConfigurer mapperScannerConfigurer(){
        MapperScannerConfigurer msc = new MapperScannerConfigurer();

        msc.setBasePackage("com.itheima.dao");

        return msc;
    }
```

* MapperScannerConfigurer对象也是MyBatis提供的专用于整合的jar包中的类

  用来处理原始配置文件中的mappers相关配置，加载数据层的Mapper接口类
* MapperScannerConfigurer核心属性basePackage

  用来设置所扫描的包路径

‍

‍

##### 运行类

从IOC容器中获取Service对象调用方法获取结果

```java
public class App{
    public static void main(String[] args) {

        ApplicationContext ctx = new AnnotationConfigApplicationContext(SpringConfig.class);

        AccountService accountService = ctx.getBean(AccountService.class);

        Account ac = accountService.findById(1);

        ......打印POJO对象
    }
}
```

‍

‍

‍

### Junit

‍

‍

引入坐标

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

‍

Junit运行后是基于Spring环境运行的，所以Spring提供了一个专用的类运行器，这个务必要设置，这个类运行器就在Spring的测试专用包中提供的，导入的坐标就是这个东西`SpringJUnit4ClassRunner`​

‍

#### 测试类

抄模板即可

‍

在test\java下创建一个测试类AccountServiceTest

```java
//设置类运行器
@RunWith(SpringJUnit4ClassRunner.class)
//设置Spring环境对应的配置类
@ContextConfiguration(classes = {SpringConfiguration.class}) //加载配置类
//@ContextConfiguration(locations={"classpath:applicationContext.xml"})//加载配置文件
public class AccountServiceTest {
    //支持自动装配注入bean
    @Autowired
    private AccountService accountService;

    @Test
    public void testFindById(){
        System.out.println(accountService.findById(1));
...
}
```

‍

#### 注解选择

‍

测试的是注解配置类，则使用`@ContextConfiguration(classes = 配置类.class)`​

测试的是配置文件，则使用`@ContextConfiguration(locations={配置文件名,...})`​

‍

@RunWith

|名称|@RunWith|
| ------| -----------------------------------|
|类型|测试类注解|
|位置|测试类定义上方|
|作用|设置JUnit运行器|
|属性|value（默认）：运行所使用的运行期|

‍

@ContextConfiguration

|名称|@ContextConfiguration|
| ------| ---------------------------------------------------------------------------------------------------------------------------|
|类型|测试类注解|
|位置|测试类定义上方|
|作用|设置JUnit加载的Spring核心配置|
|属性|classes：核心配置类，可以使用数组的格式设定加载多个配置类<br />locations:配置文件，可以使用数组的格式设定加载多个配置文件名称|

‍

## 生产功能

‍

### 热部署

‍

#### 概念

* 应用正在运行的时候升级功能, 不需要重新启动应用
* 对于Java应用程序来说, 热部署就是在运行时更新Java类文件

‍

#### 评价

不需要重新手工启动应用，提高本地开发效率

注意尽量不要线上热部署, 否则有被黑客攻击的风险

‍

常见实现热部署的方式

* Jrebel
* Spring Loaded
* spring-boot-devtools

‍

‍

#### 依赖

**在创建SpringBoot项目时选择添加模块 : devtools**

或者手动导入

```xml
<!-- 引入DevTools热部署 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <scope>runtime</scope>
    <optional>true</optional>
</dependency>
```

‍

#### 配置

```yml
spring:
  devtools:
    restart:
      enabled: true  #设置开启热部署
      additional-paths: src/main/java #重启目录
      exclude: WEB-INF/**
  thymeleaf:
    cache: false #使用Thymeleaf模板引擎，关闭缓存
```

‍

#### 选项

File → Settings → Build, Execution, Deployment → Compiler

在右侧的选项中找到 <kbd>Build project automatically </kbd>​ 自动构建项目 选项并勾选

并行编译独立模块(不要紧)勾选; 开启自动热部署后，修改业务代码，当IDEA失去焦点5秒钟，就会自动进行构建

‍

‍

#### 维护面板配置

‍

快捷键 Ctrl + Shift + Alt + / ，点击Registry注册表项

compiler.automake.allow.when.app.running 选项并勾选(旧版本)

‍

打开高级设置里面- 搜索 自动 关键字 ->即使当前应用正在运行,也允许自动make启动

‍

‍

#### 范围控制

配置中默认不参与热部署的目录信息如下

```java
/META-INF/maven
/META-INF/resources
/resources
/static
/public
/templates
```

以上目录中的文件如果发生变化，是不参与热部署的. 如果想修改配置，可以通过application.yml文件进行设定哪些文件不参与热部署操作

‍

```java
spring:
  devtools:
    restart:
      # 设置不参与热部署的文件或文件夹
      exclude: static/**,public/**,config/application.yml

```

‍

‍

#### 关闭热部署

线上环境运行时是不可能使用热部署功能的，所以需要强制关闭此功能，通过配置可以关闭此功能.

```yaml
spring:
  devtools:
    restart:
      enabled: false
```

‍

如果当心配置文件层级过多导致相符覆盖最终引起配置失效，可以提高配置的层级，在更高层级中配置关闭热部署. 例如在启动容器前通过系统属性设置关闭热部署功能.

```yml
System.setProperty("spring.devtools.restart.enabled","false");
```

‍

```java
@SpringBootApplication
public class SSMPApplication {
    public static void main(String[] args) {
        System.setProperty("spring.devtools.restart.enabled","false");
        SpringApplication.run(SSMPApplication.class);
    }
}
```

‍
