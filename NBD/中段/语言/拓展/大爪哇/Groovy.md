--无缝兼容Java的语言

‍

‍

一种基于Java平台的既可以面向对象又可以脚本化的语言

‍

## Header

与Java

很难用 "编译型还是解释型" 来区分 Groovy 和 Java，因为两者都需要 `javac`​ or `groovyc`​ 将源码翻译成二进制码，然后交给 JVM 解释执行。这样看的话，Java 和 Groovy 应该都算 "编译兼解释型" 语言。两者的主要区别是：Java 是典型的静态语言 ( 所有的数据都在编译期间就被确定 )，而 Groovy 可以做到 "动态分发"，同时也支持静态编译。

‍

Groovy是一种基于Java平台的面向对象语言，它与Java语法非常相似，但是也有一些不同的特点

* Groovy支持**动态类型**和**静态类型**，可以用def关键字来定义变量，而不需要写明具体的类型。
* Groovy支持**运算符重载**，可以自定义运算符的行为。
* Groovy可以很轻松地定义和操作**列表**和**关联数组**，并提供了一些方便的方法。
* Groovy对**正则表达式**有原生支持，可以直接使用`~`​符号来创建正则表达式对象。
* Groovy对各种标记语言，如XML和HTML有原生支持，可以使用类似于HTML的语法来创建和解析XML文档²。
* Groovy可以调用Java的所有库，并且可以与Java代码无缝集成。
* Groovy扩展了java.lang.Object类，为所有对象添加了一些有用的方法。

‍

在某种程度上，Groovy 可以被视为Java 的一种脚本化改良版,Groovy 也是运行在 JVM 上，它可以很好地与 Java 代码及其相关库进行交互操作。它是一种成熟的面向对象编程语言，既可以面向对象编程，又可以用作纯粹的脚本语言。大多数有效的 Java 代码也可以转换为有效的 Groovy 代码，Groovy 和 Java 语言的主要区别是：完成同样的任务所需的Groovy 代码比 Java 代码更少。其特点为：

1. 功能强大，例如提供了动态类型转换、闭包和元编程（metaprogramming）支持
2. 支持函数式编程，不需要main 函数
3. 默认导入常用的包
4. 类不支持 default 作用域,且默认作用域为public。
5. Groovy 中基本类型也是对象，可以直接调用对象的方法。
6. 支持DSL（Domain Specific Languages 领域特定语言）和其它简洁的语法，让代码变得易于阅读和维护。
7. Groovy 是基于Java 语言的，所以完全兼容Java 语法,所以对于java 程序员学习成本较低

### 快速方法

大多数情况下按照Java的想法来做应该就可以了, 末尾分号可以省略

‍

### 我的简写

‍

### 版本控制

‍

### 环境相关

‍

**特别提示 1：** 使得在Terminal 中执行以gradlew 开头命令和操作图形化的IDEA 使用Gradle 版本**不一定是同一个版本**哦。

**1.Terminal中以gradlew开头指令用的是Wrapper规定的gradle版本,wrapper中规定版本默认和idea插件中规定的版本一致。**

**2.而图形化的IDEA使用Gradle是本地安装的哦。**

在IDEA中调整使用的Gradle位置，可以加速Gradle构建(有魔法全局代理另说)

‍

‍

# 知识

‍

## 数学特性

‍

## 底层特性

‍

### 构建

使用面向对象时, 生成字节码文件中可以看到继承了GroovyObject类, 如果是直接用脚本式编程时, 字节码看到继承了Script类

对于脚本之后便可以使用脚本的文件名调用对应脚本方法

‍

### 令牌概念

令牌可以是一个关键字，一个标识符，常量，字符串文字或符号。

```
println(“Hello World”);
```

在上面的代码行中，有两个令牌，首先是关键词的 println 而接下来就是字符串的“Hello World”。

‍

### 一切即闭包

‍

一切 `{}`​ 代码块在 Groovy 会被视作一个**闭包, ** 一段 `{}`​ 扩起来的闭包不能单独声明出现，除非是写成赋值的形式：

‍

### Bool求值

‍

Groovy 的处理则更加优雅一些，当传入的值不是纯粹的布尔值时，Groovy 会基于传入的类型进行一些合理的推断，而不是直接报错

‍

### 可选类型

‍

"定义不建议使用具体值类型"

‍

Groovy是一个“可选”类型的语言，当理解语言的基本原理时，这种区别是一个重要的语言。与Java相比，Java是一种“强”类型的语言，由此编译器知道每个变量的所有类型，并且可以在编译时理解和尊重合同。这意味着方法调用能够在编译时确定。

当在Groovy中编写代码时，开发人员可以灵活地提供类型或不是类型。这可以提供一些简单的实现，并且当正确利用时，可以以强大和动态的方式为您的应用程序提供服务。

‍

在Groovy中，可选的键入是通过'def'关键字完成的

要了解如何使用Groovy中的可选输入，而不让代码库陷入无法维护的混乱，最好在应用程序中采用“鸭式输入”的理念。

鸭子类型是一个与动态类型相关的概念，其中对象的类型或类不如它定义的方法重要。当您使用鸭子类型时，您根本不检查类型。相反，您检查给定方法或属性是否存在。计算机编程中的鸭子打字是鸭子测试的一个应用程序——“如果它像鸭子一样走路并且它像鸭子一样嘎嘎叫，那么它一定是鸭子”——以确定一个对象是否可以用于特定目的。对于普通类型，适用性由对象的类型决定。. Duck Typing 是一种编程方式，其中传递给函数或方法的对象支持该对象在运行时预期的所有方法签名和属性。对象的类型本身并不重要。相反，该对象应支持对其调用的所有方法/属性。

‍

### 多重赋值

(类似解包)

如果方法 ( 函数 ) 返回的是一个数组，那么 Groovy 支持使用多个变量接收数组内的元素内容

```groovy
def swap(x, y) { return [y, x] }

Integer a, b
a = 10
b = 50

// 通过多重赋值实现了两数交换
(a, b) = swap(a, b)
print("a=$a,b=$b")
```

‍

Groovy 的方法 (函数) 可以返回多个值。其它支持这么做的编程语言还有 Go，Scala ( 通过包装成元组来实现 ) 等。当接收变量的个数和实际的返回值个数不匹配时，Groovy 会这样做：

1. 如果接收的变量更多，那么会将没有赋值的变量赋为 null 。
2. 如果返回值更多，那么多余的返回值会被丢弃。

当然，Groovy 也的确提供了元组，这个写法对于一些 Scala 程序员绝对不陌生：

```groovy
Tuple2<Integer,Integer> swap(Integer a,Integer b){
    return new Tuple2<Integer,Integer>(b,a)
}

a = 10
b = 20

(a, b) = swap(a, b)
print("a=${a},b=${b}")

```

‍

‍

### 简洁\"非空则调用"

```groovy
简洁的 "非空则调用" 语法
为了避免调用某个空指针的方法，在 Java 代码中，我们通常要包裹一层 if 语句块：
    String maybeNull = "I'm Java";
    if(maybeNull != null){System.out.println(nullString.length());}

这一长串逻辑在 Groovy 当中可以直接使用一个 ?. 操作符解决：
    String maybeNull = 'I\'m groovy'
    print(maybeNull?.length())

```

‍

### 可见度

‍

默认为public 与Java中的default可见度不同

‍

‍

### 对象属性操作

‍

#### 赋值

对象.setter

具名构造器(自带)

‍

#### 读取

对象.属性

对象["属性"]

对象.getter

‍

‍

‍

# 基础

‍

‍

‍

## 基本数型

‍

### 列表

列表是用于存储数据项集合的结构。在 Groovy 中，List 保存了一系列对象引用。

List 中的对象引用占据序列中的位置，并通过整数索引来区分。

列表文字表示为一系列用逗号分隔并用方括号括起来的对象。

groovy 列表使用索引操作符 [] 索引。

groovy 中的一个列表中的数据可以是任意类型。这 java 下集合列表有些不同，java 下的列表是同种类型的数据集合。

groovy 列表可以嵌套列表

‍

‍

### 字典

映射（也称为关联数组，字典，表和散列）是对象引用的无序集合。Map集合中的元素由键值访问。 Map中使用的键可以是任何类。

‍

‍

### String

‍

在 Groovy 中，短字符串可以使用 `''`​ 或者 `""`​ 表示，而需要跨行的长字符串则通常使用 `''' '''`​ 或者 `""" """`​ 。被双引号包括的字符串又被称为 GString，支持在字符串内部使用 `${}`​ 做占位符 ( 类似 `printf `​)(双单引号的各自特性不能互通, 技能只有一个)

‍

**字符串索引**

Groovy中的字符串是字符的有序序列。字符串中的单个字符可以通过其位置访问。这由索引位置给出。

‍

‍

‍

‍

## 基本量

‍

‍

## 基础语法

‍

### ==

‍

 Groovy 当中，这两者的混乱程度有所加剧：Groovy 的 `==`​**相当于**是 Java 的 `.equals()`​ 方法或者是 `compareTo()`​ 方法 (见运算符重载的那个表格)，而 Java 原始的 `==`​ 语义在 Groovy 中变成了 `is()`​ 方法。

‍

‍

### 比较运算符

`<=>`​

‍

### 范围

示例

```groovy
class Example { 
   static void main(String[] args) { 
      def range = 5..10; 
      println(range); 
      println(range.get(2)); 
   } 
}
```

​`5..10`​​等价于`[5, 6, 7, 8, 9, 10] `​​，这是Groovy中的一个整数范围（Range）对象，包含从5到10的所有整数。这种写法在Groovy中是非常常见的，它用于表示一个连续的整数序列，通常用于循环或遍历操作中

‍

‍

### 方法

‍

    使用返回类型或使用 def 关键字定义的, 方法可以接收任意数量的参数。定义参数时，不必显式定义类型。()在不引起歧义时候可以省略(听IDE说), 可以添加修饰符，如 public，private 和 protected。默认情况下，如果未提供可见性修饰符，则该方法为 public

‍

```groovy
class Example {
   static def DisplayName() {
      println("This is how methods work in groovy");
      println("This is an example of a simple method");
   } 

   static void main(String[] args) {
      DisplayName();
   } 
}
```

‍

‍

#### 可选形参

‍

在方法 ( 或函数 ) 参数列表内，可以提前为**最后一个**参数设定默认值，那么在调用该方法时，最后一个参数可以被省略

```groovy
def add(Integer arg,Integer implicit = 10){arg + implicit}

// 11
print(add(1))
// 3 
print(add(1,2))
```

‍

Groovy 的方法 ( 函数 ) 不要求显示地添加 `return`​ 关键字，它总是默认返回函数体内最后一个调用的结果值。然后，这个函数的返回值类型是显然可以推断的，因此这里也可使用 `def`​ 关键字替换掉函数的返回值声明。

‍

‍

## 注解

‍

使用注释(注解)时，需要至少设置所有没有默认值的成员。下面给出一个例子。当定义后使用注释示例时，需要为其分配一个值。

```
@interface Example {
   int status() 
}

@Example(status = 1)
```

‍

字符串注释

```
@interface Simple { 
   String str1() default "HelloWorld"; 
}
```

枚举类型注释

```
enum DayOfWeek { mon, tue, wed, thu, fri, sat, sun } 
@interface Scheduled {
   DayOfWeek dayOfWeek() 
} 
```

类类型注释

```
@interface Simple {} 
@Simple 
class User {
   String username
   int age
}
 
def user = new User(username: "Joe",age:1); 
println(user.age); 
println(user.username);
```

‍

### 关闭注释参数

Groovy中注释的一个很好的特性是，你也可以使用闭包作为注释值。因此，注释可以与各种各样的表达式一起使用。

下面给出一个例子。注释Onlyif是基于类值创建的。然后注释应用于两个方法，它们基于数字变量的值向结果变量发布不同的消息。

```
@interface OnlyIf {
   Class value() 
}  

@OnlyIf({ number<=6 }) 
void Version6() {
   result << 'Number greater than 6' 
} 

@OnlyIf({ number>=6 }) 
void Version7() {
   result << 'Number greater than 6' 
}
```

‍

‍

### 元注释

这是groovy中注释的一个非常有用的功能。有时可能有一个方法的多个注释，如下所示。有时这可能变得麻烦有多个注释。

```
@Procedure 
@Master class 
MyMasterProcedure {} 
```

在这种情况下，您可以定义一个元注释，它将多个注释集中在一起，并将元注释应用于该方法。所以对于上面的例子，你可以使用AnnotationCollector来定义注释的集合。

```
import groovy.transform.AnnotationCollector
  
@Procedure 
@Master 
@AnnotationCollector
```

一旦完成，您可以应用以下元注释器到该方法 -

```
import groovy.transform.AnnotationCollector
  
@Procedure 
@Master 
@AnnotationCollector
  
@MasterProcedure 
class MyMasterProcedure {}
```

‍

### 强力注解

‍

这里或许有一些官方提供的注解帮助快速开发，它们绝大部分都是来自于 `groovy.lang`​ 包，这意味着不需要通过 `import`​ 关键字额外地导入外部依赖：

‍

#### @Canonical 替代 toString

假如希望打印一个类信息，又不想自己生成 `toString()`​ 方法，则可以使用 `@Canonical`​ 注解。该注解有额外的 `excludes`​ 选项：允许我们忽略一些属性。

‍

#### @Delegate 实现委托

使用 `@Delegate`​ 注解，在 Groovy 中实现方法委托非常容易。委托是继承以外的另一种代码复用的思路。在下面的代码块中，Manager 通过注解将 `work()`​ 方法委托给了内部的 worker 属性：

‍

```groovy
@Singleton 单例模式
在 Groovy 中，仅凭 @Singleton 注解就可以实现一个线程安全，并且简洁的单例模式。
groovy复制代码// 懒加载的单例模式，lazy 项是可选的。
@Singleton(lazy = true)
class TheUnique{
    {
        println "created only once"
    }
}

// 通过 .instance 调用这个单例对象。
TheUnique.instance

单例模式可以选择懒汉式加载，仅需在注解的 lazy 选项中设置为 true 即可

```

‍

‍

## 类

### 构造器

对于上述的 Student_ 类而言，它可能需要有 4 个构造器：无参构造器，仅附带 name 属性的构造器，仅附带 age 属性的构造器，完整的构造器。Groovy 可以让我们仅通过一个 Map 实现灵活的对象创建，并且不需要再手动地补充构造器写法：

```groovy
class Student_{
    String name
    Integer age
}

// 没有实现 Student_(name,age) 构造器，但是可以直接使用
stu1 = new Student_(name: "Wang Fang",age: 12)

// 同样，我们也没有手动实现 Student_(name) 构造器。
stu2 = new Student_(name:"Wang Fang")

```

‍

在些参数列表中，我们传入的其实是一整个 Map。里面的每一个 `k:v`​ 都表示了一个键值对。`k`​ 对应了这个类当中每个属性名，而 `v`​ 则为这些属性赋值。但是，我们不能这样做：除非手动地补充上对应的构造函数。

```groovy
stu1 = new Student_("Wang Fang",12)
stu2 = new Student_("Wang Fang")
```

‍

### 精简JavaBean

‍

在 Groovy 当中，编译器总是自动在底层为属性生成对应的 Set 和 Get 方法

如果希望某个属性在对象被构造之后就不可变，则需使用 `final`​ 关键字，编译器将不会主动地为其生成 Set 方法 ( 意味着该属性是只读的 ) 。另外，属性可以不主动声明类型，此时原本的类型被 `def`​ 关键字替代。

对于未主动声明类型的属性，其本质上属于 Object 对象，这不利于对该属性的后续操作。想要解决这个问题，不妨在构造器中留下一些线索，以便于编译器能够 "推导" 出目标类型 ( Groovy 总是通过变量的赋值来推断这个变量的实际类型 )。

‍

‍

## 异常

Groovy中的异常层次结构都基于Java中定义的层次结构

Groovy 对异常处理的写法更为宽松：如果没有在该代码块内通过 try-catch 处理异常，那么该异常就会自动地向上级抛出，且无需在函数声明中使用 `throws`​ 主动定义它们

‍

‍

‍

# 高级

‍

## IO

‍

Groovy在使用I / O时提供了许多辅助方法，Groovy提供了更简单的类来为文件提供以下功能。

* 读取文件
* 写入文件
* 遍历文件树
* 读取和写入数据对象到文件

‍

除此之外，您始终可以使用下面列出的用于文件I / O操作的标准Java类。

* java.io.File
* java.io.InputStream
* java.io.OutputStream
* java.io.Reader
* java.io.Writer

‍

## 特征

trait

特征是语言的结构构造，允许 -

* 行为的组成。
* 接口的运行时实现。
* 与静态类型检查/编译的兼容性

它们可以被看作是承载默认实现和状态的接口。使用trait关键字定义 trait。

下面给出了一个特征的例子：

```groovy
trait Marks {
   void DisplayMarks() {
      println("Display Marks");
   } 
}
```

然后可以使用 implement 关键字以类似于接口的方式实现 trait。

```groovy
class Example {
   static void main(String[] args) {
      Student st = new Student();
      st.StudentID = 1;
      st.Marks1 = 10; 
      println(st.DisplayMarks());
   } 
} 

trait Marks { 
   void DisplayMarks() {
      println("Display Marks");
   } 
} 

class Student implements Marks { 
   int StudentID
   int Marks1;
}
```

‍

### 实现接口

Traits 可以实现接口，在这种情况下，使用 interface 关键字声明接口。

下面给出了实现接口的特征的示例。在以下示例中，可以注意以下要点。

* 接口 Total 使用方法 DisplayTotal 定义。
* 特征 Marks 实现了 Total 接口，因此需要为 DisplayTotal 方法提供一个实现。

‍

```groovy
class Example {
   static void main(String[] args) {
      Student st = new Student();
      st.StudentID = 1;
      st.Marks1 = 10;
	
      println(st.DisplayMarks());
      println(st.DisplayTotal());
   } 
} 

interface Total {
   void DisplayTotal() 
} 

trait Marks implements Total {
   void DisplayMarks() {
      println("Display Marks");
   }

   void DisplayTotal() {
      println("Display Total"); 
   } 
} 

class Student implements Marks { 
   int StudentID
   int Marks1;  
} 
```

‍

### 属性

特征可以定义属性。下面给出了具有属性的trait的示例。

在以下示例中，integer 类型的 Marks1 是一个属性。

```
class Example {
   static void main(String[] args) {
      Student st = new Student();
      st.StudentID = 1;

      println(st.DisplayMarks());
      println(st.DisplayTotal());
   } 

   interface Total {
      void DisplayTotal() 
   } 

   trait Marks implements Total {
      int Marks1;

      void DisplayMarks() {
         this.Marks1 = 10;
         println(this.Marks1);
      }

      void DisplayTotal() {
         println("Display Total");
      } 
   } 

   class Student implements Marks {
      int StudentID 
   }
} 
```

‍

### 扩展特征

特征可能扩展另一个特征，在这种情况下，必须使用extends关键字。在下面的代码示例中，我们使用 Marks trait 扩展了 Total trait。

```
class Example {
   static void main(String[] args) {
      Student st = new Student();
      st.StudentID = 1;
      println(st.DisplayMarks());
   } 
} 

trait Marks {
   void DisplayMarks() {
      println("Marks1");
   } 
} 

trait Total extends Marks {
   void DisplayMarks() {
      println("Total");
   } 
}  

class Student implements Total {
   int StudentID 
}
```

‍

‍

## 闭包

‍

闭包是一个开放的匿名代码块。它通常跨越几行代码。一个方法甚至可以将代码块作为参数。它们是匿名的。

闭包中的变量默认是it

闭包调用方式： 闭包是 groovy.lang.Closure 的实例。它可以像任何其他变量一样分配给一个变量或字段。  

闭包对象(参数)  
闭包对象.call(参数)

作为方法最后一个参数的时候可以放在方法的外面

```
class Example {
   static void main(String[] args) {
      def clos = {println "Hello World"};
      clos.call();
   } 
}
```

在上面的例子中，代码行 - {println“Hello World”}被称为闭包。此标识符引用的代码块可以使用call语句执行

‍

```groovy
//闭包体完成变量自增操作
{ item++ }
//闭包使用 空参数列表 明确规定这是无参的
{ -> item++ }
//闭包中有一个默认的参数[it]，写不写无所谓
{ println it }
{ it -> println it }
//如果不想使用默认的闭包参数it,那需要显示自定义参数的名称
{ name -> println name }
//闭包也可以接受多个参数
{ String x, int y ->
    println "hey ${x} the value is ${y}"
}
//闭包参数也可是一个对象
{ reader ->
    def line = reader.readLine() 
    line.trim()
}
```

‍

### 形式参数

闭包也可以包含形式参数，以使它们更有用，就像Groovy中的方法一样。

```
class Example {
   static void main(String[] args) {
      def clos = {param->println "Hello ${param}"};
      clos.call("World");
   } 
}
```

在上面的代码示例中，注意使用$ {param}，这导致closure接受一个参数。当通过clos.call语句调用闭包时，我们现在可以选择将一个参数传递给闭包。

‍

下一个图重复了前面的例子并产生相同的结果，但显示可以使用被称为它的隐式单个参数。这里的'it'是Groovy中的关键字。

```
class Example {
   static void main(String[] args) {
      def clos = {println "Hello ${it}"};
      clos.call("World");
   } 
}
```

‍

### 变量

更正式地，闭包可以在定义闭包时引用变量。以下是如何实现这一点的示例。

```
class Example {   
   static void main(String[] args) {
      def str1 = "Hello";
      def clos = {param -> println "${str1} ${param}"}
      clos.call("World");

      // We are now changing the value of the String str1 which is referenced in the closure
      str1 = "Welcome";
      clos.call("World");
   } 
}
```

‍

### 方法中

闭包也可以用作方法的参数。在Groovy中，很多用于数据类型（例如列表和集合）的内置方法都有闭包作为参数类型。

‍

‍

### 数据结构中

几个List，Map和String方法接受一个闭包作为参数

‍

在下面的例子中，我们首先定义一个简单的值列表。列表集合类型然后定义一个名为.each的函数。此函数将闭包作为参数，并将闭包应用于列表的每个元素

```
class Example {
   static void main(String[] args) {
      def lst = [11, 12, 13, 14];
      lst.each {println it}
   } 
}
```

‍

定义一个简单的关键值项Map。然后，映射集合类型定义一个名为.each的函数。此函数将闭包作为参数，并将闭包应用于映射的每个键值对。

```
class Example {
   static void main(String[] args) {
      def mp = ["TopicName" : "Maps", "TopicDescription" : "Methods in Maps"]         
      mp.each {println it}
      mp.each {println "${it.key} maps to: ${it.value}"}
   } 
}
```

‍

遍历集合的成员，并且仅当元素满足一些标准时应用一些逻辑。这很容易用闭包中的条件语句来处理。

```
class Example {
   static void main(String[] args) {
      def lst = [1,2,3,4];
      lst.each {println it}
      println("The list will only display those numbers which are divisible by 2")
      lst.each{num -> if(num % 2 == 0) println num}
   } 
}
```

‍

‍

## 系统交互

‍

得益于 Groovy 的简练语法（其实几乎只要是个新颖的编程语言就要比 Java 简洁得多，因此 "简洁" 其实不应该再算是 Groovy 的 Feature）和动态特性，使得 Groovy 可以轻松地和系统进程进行交互：

‍

```groovy
// 用 groovy 去执行 "groovy -v".
// Windows 环境下需要带上 cmd /C 前缀。
println("cmd /C groovy -v".execute().text)
```

‍

​`execute()`​ 方法可以将这个字符串视作是一个**命令**交给系统去执行，`.text`​ 可以获取该命令在系统下的执行结果。下面演示了在 Linux 和 Windows 系统当中，如何通过 `.groovy`​ 脚本实现 "浏览当前目录（即执行者 `.groovy`​ 所在的那个目录下）的内容"：

```groovy
// Linux 系统
println("ls -l")

// Windows 系统
println("cmd /C dir")
```

注意，`ls`​ 是 Linux 系统中可以直接运行的程序，但是 `dir`​ 在 Widows 系统中仅仅是 `cmd`​ 命令行解释器当中定义的一条命令，所以在这里补充了额外的前缀 `cmd /C`​。一段 Groovy 代码还可以即时调用另一个文件存储的 Groovy 代码。比如：

```groovy
evaluate(new File("C:\\Users\\i\\IdeaProjects\\GroovyHello\\src\\HelloWorld.groovy"))
// 在另一个 ./HelloWorld.groovy 脚本中，我们在那个文件里仅使用了一句 print 'hello World! -by groovy' 令其输出一段话。

```

‍

‍

‍

# 实操

‍

包含groovy二进制文件作为maven或gradle构建的一部分，你可以添加以下行

‍

### Gradle

```
'org.codehaus.groovy:groovy:2.4.5'
```

### Maven

```
<groupId>org.codehaus.groovy</groupId> 
<artifactId>groovy</artifactId>  
<version>2.4.5</version>
```

‍
