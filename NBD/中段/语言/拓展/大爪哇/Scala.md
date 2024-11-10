--能屈能伸

‍

## Header

通过IDEA安装插件加自动下载

‍

‍

‍

### 速查

‍

‍

### 环境

‍

‍

# 知识

‍

Scala是Scalable Language 的简写，是一门多范式的编程语言; 基于java之上，大量使用java的类库和变量，使用 Scala 之前必须先安装 Java

Scala 与 Java 的最大区别是：Scala 语句末尾的分号 ; 是可选的(强制区分语句)

我个人感觉就类似JavaPython化, 脚本语言化

‍

## 底层

‍

### 特性

‍

**面向对象特性**

Scala是一种纯面向对象的语言，每个值都是对象。对象的数据类型以及行为由类和特质描述。

类抽象机制的扩展有两种途径：一种途径是子类继承，另一种途径是灵活的混入机制。这两种途径能避免多重继承的种种问题。

‍

**函数式编程**

Scala也是一种函数式语言，其函数也能当成值来使用。Scala提供了轻量级的语法用以定义匿名函数，支持高阶函数，允许嵌套多层函数，并支持柯里化。Scala的case class及其内置的模式匹配相当于函数式编程语言中常用的代数类型。

更进一步，程序员可以利用Scala的模式匹配，编写类似正则表达式的代码处理XML数据。

‍

**静态类型**

Scala具备类型系统，通过编译时检查，保证代码的安全性和一致性。

‍

**扩展性**

Scala的设计秉承一项事实，即在实践中，某个领域特定的应用程序开发往往需要特定于该领域的语言扩展。Scala提供了许多独特的语言机制，可以以库的形式轻易无缝添加新的语言结构：

* 任何方法可用作前缀或后缀操作符
* 可以根据预期类型自动构造闭包。

‍

**并发性**

Scala使用Actor作为其并发模型，Actor是类似线程的实体

‍

## 命名

‍

**类名** - 对于所有的类名的第一个字母要大写。  
如果需要使用几个单词来构成一个类的名称，每个单词的第一个字母要大写。

示例：*class MyFirstScalaClass*

‍

**方法名称** - 所有的方法名称的第一个字母用小写。  
如果若干单词被用于构成方法的名称，则每个单词的第一个字母应大写。

示例：*def myMethodName()*

‍

**程序文件名** - 程序文件的名称应该与对象名称完全匹配

‍

**main()**  - Scala程序从main()方法开始处理，这是每一个Scala程序的强制程序入口部分

​`def main(args: Array[String])`​

‍

‍

## 工程

编程有脚本和交互式两种

‍

### 类型

‍

#### 脚本形式

创建一个 HelloWorld.scala 的文件来执行代码

使用 scalac 命令编译它

```java
$ scalac HelloWorld.scala 
$ ls
```

编译后我们可以看到目录下生成了 HelloWorld.class 文件，该文件可以在Java Virtual Machine (JVM)上运行。

编译后，我们可以使用以下命令来执行程序：

```
$ scala HelloWorld
```

‍

#### IDEA整合

‍

需要选择构建工具, 并且需要代理来加速项目结构下载.

运行速度较慢...

‍

### 包管理

‍

#### 定义

使用 package 关键字定义包，在Scala将代码定义到某个包中有两种方式：

第一种方法和 Java 一样，在文件的头定义包名，这种方法就后续所有代码都放在该包中

```scala
package com.runoob
class HelloWorld
```

第二种方法有些类似 C#，如：

```scala
package com.runoob {
  class HelloWorld 
}
```

第二种方法，可以在一个文件中定义多个包

‍

#### 引用

‍

Scala 使用 import 关键字引用包

‍

```scala
import java.awt.Color  // 引入Color
 
import java.awt._  // 引入包内所有成员
 
def handler(evt: event.ActionEvent) { // java.awt.event.ActionEvent
  ...  // 因为引入了java.awt，所以可以省去前面的部分
}
```

‍

import语句可以出现在任何地方，而不是只能在文件顶部。import的效果从开始延伸到语句块的结束。这可以大幅减少名称冲突的可能性。

如果想要引入包中的几个成员，可以使用selector（选取器）：

```scala
import java.awt.{Color, Font}
 
// 重命名成员
import java.util.{HashMap => JavaHashMap}
 
// 隐藏成员
import java.util.{HashMap => _, _} // 引入了util包的所有成员，但是HashMap被隐藏了
```

‍

‍

# 基础

‍

‍

## 数型

‍

数据类型都是对象，也就是说scala没有java中的原生类型。在scala是可以对数字等基础类型调用方法的。

‍

|数据类型|描述|
| ----------| --------------------------------------------------------------------------------------------------|
|Byte|8位有符号补码整数。数值区间为 -128 到 127|
|Short|16位有符号补码整数。数值区间为 -32768 到 32767|
|Int|32位有符号补码整数。数值区间为 -2147483648 到 2147483647|
|Long|64位有符号补码整数。数值区间为 -9223372036854775808 到 9223372036854775807|
|Float|32 位, IEEE 754 标准的单精度浮点数|
|Double|64 位 IEEE 754 标准的双精度浮点数|
|Char|16位无符号Unicode字符, 区间值为 U+0000 到 U+FFFF|
|String|字符序列|
|Boolean|true或false|
|**Unit**|表示无值，和其他语言中void等同。用作不返回任何结果的方法的结果类型。Unit只有一个实例值，写成()。|
|Null|null 或空引用|
|**Nothing**|Nothing类型在Scala的类层级的最底端；它是任何其他类型的子类型。|
|**Any**|Any是所有其他类的超类|
|AnyRef|AnyRef类是Scala里所有引用类(reference class)的基类|

‍

‍

### 数值类型

```scala
val b: Byte = 1
val s: Short = 1
val i: Int = 1
val l: Long = 1
val f: Float = 3.0
val d: Double = 2.0

val x = 1_000L // val x: Long=1000
val y = 2.2D // val y: Double=2.2
val z = 3.3F //val z: Float = 3.3
```

如果需要准确的大数值，可以使用`BigInt`​,`BigDecimal`​类型

```scala
var a = BigInt(1_234_567_890_987_654_321L)
var b = BigDecimal(123_456.789)
```

‍

‍

### 字符类型

Scala 也有`String`​ 和`Char`​类型

```scala
val name = "Bill"   // String
val c = 'a'         // Char
```

Scala中的Strings和Java中的类似，但是提供了2个强大的额外功能

* 支持字符串插值
* 可以创建多行字符串

> 字符串插值

符串插值提供了一种非常易读的方式来使用字符串中的变量。例如，给定这三个变量：

```scala
val firstName = "John"
val mi = 'C'
val lastName = "Doe"
```

您可以将这些变量组合在一个字符串中，如下所示：

```
println(s"Name: $firstName $mi $lastName")   
// "Name: John C Doe"
```

只需在字符串前面加上字母`s`​，然后在变量名之前添加`$`​符号

如果字符串中有表达式，需要加`{}`​

```
println(s"2 + 2 = ${2 + 2}")   // prints "2 + 2 = 4"

val x = -1
println(s"x.abs = ${x.abs}")   // prints "x.abs = 1"
```

‍

‍

### 整型

整型字面量用于 Int 类型，如果表示 Long，可以在数字后面添加 L 或者小写 l 作为后缀

‍

### 浮点

如果浮点数后面有f或者F后缀时，表示这是一个Float类型，否则就是一个Double类型的

‍

### 布尔

布尔型字面量有 true 和 false。

‍

### 符号

符号字面量被写成：  **'&lt;标识符&gt;**  ，这里  **&lt;标识符&gt;**  可以是任何字母或数字的标识（注意：不能以数字开头）。这种字面量被映射成预定义类scala.Symbol的实例。

如： 符号字面量  **'x** 是表达式 **scala.Symbol(&quot;x&quot;)**  的简写，符号字面量定义如下：

```
package scala
final case class Symbol private (name: String) {
   override def toString: String = "'" + name
}
```

‍

‍

### 字符串

双引号  **&quot;**

字符    单引号  **'**

多行字符串     **&quot;&quot;&quot; ... &quot;&quot;&quot;**

‍

在 Scala 中，字符串的类型实际上是 Java String，它本身没有 String 类。

在 Scala 中，String 是一个不可变的对象，所以该对象不可被修改。这就意味着你如果修改字符串就会产生一个新的字符串对象。

‍

#### 方法

‍

使用 length() 方法来获取字符串长度

```scala
var len = palindrome.length();
```

使用 concat() 方法来连接两个字符串, 也可以使用加号(+)来连接：

```
string1.concat(string2);
```

‍

#### 格式化字符串

‍

使用 printf() 方法来格式化字符串并输出，String format() 方法可以返回 String 对象而不是 PrintStream 对象

‍

```scala
 var floatVar = 12.456
      var intVar = 2000
      var stringVar = "菜鸟教程!"
      var fs = printf("浮点型变量为 " +
                   "%f, 整型变量为 %d, 字符串为 " +
                   " %s", floatVar, intVar, stringVar)
      println(fs)
```

‍

‍

### 空值

空值是 scala.Null 类型

Scala.Null和scala.Nothing是用统一的方式处理Scala面向对象类型系统的某些"边界情况"的特殊类型。

Null类是null引用对象的类型，它是每个引用类（继承自AnyRef的类）的子类。Null不兼容值类型。

‍

### 转义字符

|\b|\u0008|退格(BS) ，将当前位置移到前一列|
| ------------| --------| -------------------------------------|
|\t|\u0009|水平制表(HT) （跳到下一个TAB位置）|
|\n|\u000a|换行(LF) ，将当前位置移到下一行开头|
|\f|\u000c|换页(FF)，将当前位置移到下页开头|
|\r|\u000d|回车(CR) ，将当前位置移到本行开头|
|\"|\u0022|代表一个双引号(")字符|
|\'|\u0027|代表一个单引号（'）字符|
|\\\\|\u005c|代表一个反斜线字符 '\'|

‍

### 数组

‍

```scala
var z:Array[String] = new Array[String](3)

或

var z = new Array[String](3)

二维数组的实例：

val myMatrix = Array.ofDim[Int](3, 3)
```

‍

z 声明一个字符串类型的数组，数组长度为 3 ，可存储 3 个元素。我们可以为每个元素设置值，并通过索引来访问每个元素，如下所示：

```
z(0) = "Runoob"; z(1) = "Baidu"; z(4/2) = "Google"
```

最后一个元素的索引使用了表达式 **4/2** 作为索引，类似于 **z(2) = &quot;Google&quot;** 。

我们也可以使用以下方式来定义一个数组：

```
var z = Array("Runoob", "Baidu", "Google")
```

‍

```scala
object Test {
   def main(args: Array[String]) {
      var myList = Array(1.9, 2.9, 3.4, 3.5)
   
      // 输出所有数组元素
      for ( x <- myList ) {
         println( x )
      }

      // 计算数组所有元素的总和
      var total = 0.0;
      for ( i <- 0 to (myList.length - 1)) {
         total += myList(i);
      }
      println("总和为 " + total);

      // 查找数组中的最大元素
      var max = myList(0);
      for ( i <- 1 to (myList.length - 1) ) {
         if (myList(i) > max) max = myList(i);
      }
      println("最大值为 " + max);
   
   }
}
```

‍

‍

## 基础语法

‍

### Input

​`val line = StdIn.readLine()`​

‍

### Output

‍

### 变量

使用关键词  **&quot;var&quot;**  声明变量，使用关键词  **&quot;val&quot;**  声明常量

‍

==类型声明==

类型在变量名之后等号之前声明

```scala
var/val VariableName : DataType [=  Initial Value]
```

‍

==类型引用==

没有指明数据类型的情况下，其数据类型是通过变量或常量的初始值推断出来的

‍

‍

==多个变量声明==

‍

支持多个变量的声明：

```
val xmax, ymax = 100  // xmax, ymax都声明为100
```

如果方法返回值是元组，我们可以使用 val 来声明一个元组：

```
scala> val pa = (40,"Foo")
pa: (Int, String) = (40,Foo)
```

‍

### 访问修饰符

‍

基本和Java的一样，分别有：private，protected，public

默认情况下，Scala 对象的访问级别都是 public, 在任何地方都可以被访问

private 限定符，比 Java 更严格，在嵌套类情况下，外层类甚至不能访问被嵌套类的私有成员。

对保护（Protected）成员的访问比 java 更严格一些。因为它只允许保护成员在定义了该成员的的类的子类中被访问

‍

访问修饰符可以通过使用限定词强调。格式为:

```
private[x] 

或 

protected[x]
```

这里的x指代某个所属的包、类或单例对象。如果写成private[x],读作"这个成员除了对[…]中的类或[…]中的包中的类及它们的伴生对像可见外，对其它所有类都是private。

‍

### 流程控制

‍

### 区间

range() 方法来生成一个区间范围内的数组。range() 方法最后一个参数为步长，默认为 1

‍

```scala
 var myList1 = range(10, 20, 2)
      var myList2 = range(10,20)

      // 输出所有数组元素
      for ( x <- myList1 ) {
         print( " " + x )
      }
      println()
      for ( x <- myList2 ) {
         print( " " + x )
      }
```

‍

‍

### 模式匹配

‍

一个模式匹配包含了一系列备选项，每个都开始于关键字 **case**。每个备选项都包含了一个模式及一到多个表达式。箭头符号  **=&gt;**  隔开了模式和表达式。

match 对应 Java 里的 switch，但是写在选择器表达式之后。即： **选择器 match {备选项}。**

match 表达式通过以代码编写的先后次序尝试每个模式来完成计算，只要发现有一个匹配的case，剩下的case不会继续匹配。

```scala
   def matchTest(x: Int): String = x match {
      case 1 => "one"
      case 2 => "two"
      case _ => "many"
   }
```

‍

不同数据类型的模式匹配：

```scala
   def matchTest(x: Any): Any = x match {
      case 1 => "one"
      case "two" => 2
      case y: Int => "scala.Int"
      case _ => "many"
   }
```

‍

‍

## 进阶语法

‍

‍

## 数学

‍

‍

## 方法

‍

Scala 有方法与函数，二者在语义上的区别很小。Scala 方法是类的一部分，而函数是一个对象可以赋值给一个变量。换句话来说在类中定义的函数即是方法。

Scala 中的方法跟 Java 的类似，方法是组成类的一部分。

Scala 中的函数则是一个完整的对象，Scala 中的函数其实就是继承了 Trait 的类的对象。

Scala 中使用 **val** 语句可以定义函数，**def** 语句定义方法。

**注意：** 有些翻译上函数(function)与方法(method)是没有区别的。

```scala
class Test{
  def m(x: Int) = x + 3
  val f = (x: Int) => x + 3
}
```

‍

#### 声明

‍

Scala 方法声明格式如下：

```
def functionName ([参数列表]) : [return type]
```

如果你不写等于号和方法主体，那么方法会被隐式声明为**抽象(abstract)** ，包含它的类型于是也是一个抽象类型。

‍

#### 定义

‍

定义由一个 **def** 关键字开始，紧接着是可选的参数列表，一个冒号  **:**  和方法的返回类型，一个等于号  **=**  ，最后是方法的主体。

Scala 方法定义格式如下：

```
def functionName ([参数列表]) : [return type] = {
   function body
   return [expr]
}
```

以上代码中 **return type** 可以是任意合法的 Scala 数据类型。参数列表中的参数可以使用逗号分隔。

‍

以下方法的功能是将两个传入的参数相加并求和：

```scala
object add{
   def addInt( a:Int, b:Int ) : Int = {
      var sum:Int = 0
      sum = a + b

      return sum
   }
}
```

如果方法没有返回值，可以返回为 **Unit**，这个类似于 Java 的 **void**

```scala
object Hello{
   def printMe( ) : Unit = {
      println("Hello, Scala!")
   }
}
```

‍

#### 调用

‍

‍

标准格式：

```
functionName( 参数列表 )
```

如果方法使用了实例的对象来调用，我们可以使用类似java的格式 (使用  **.**  号)：

```
[instance.]functionName( 参数列表 )
```

‍

#### 闭包

‍

闭包是一个函数，返回值依赖于声明在函数外部的一个或多个变量。

闭包通常来讲可以简单的认为是可以访问一个函数里面局部变量的另外一个函数。

如下面这段匿名的函数：

```scala
val multiplier = (i:Int) => i * 10  
```

函数体内有一个变量 i，它作为函数的一个参数。如下面的另一段代码：

```scala
val multiplier = (i:Int) => i * factor
```

在 multiplier 中有两个变量：i 和 factor。其中的一个 i 是函数的形式参数，在 multiplier 函数被调用时，i 被赋予一个新的值。然而，factor不是形式参数，而是自由变量，考虑下面代码：

```
var factor = 3  
val multiplier = (i:Int) => i * factor  
```

这里我们引入一个自由变量 factor，这个变量定义在函数外面。

这样定义的函数变量 multiplier 成为一个"闭包"，因为它引用到函数外面定义的变量，定义这个函数的过程是将这个自由变量捕获而构成一个封闭的函数。

‍

‍

## OO

‍

继承一个基类跟Java很相似, 但我们需要注意以下几点：

* 1、重写一个非抽象方法必须使用override修饰符。
* 2、只有主构造函数才可以往基类的构造函数里写参数。
* 3、在子类中重写超类的抽象方法时，你不需要使用override关键字。

‍

### 单例

在 Scala 中，是没有 static 这个东西的，但是它也为我们提供了单例模式的实现方法，那就是使用关键字 object。

Scala 中使用单例模式时，除了定义的类之外，还要定义一个同名的 object 对象，它和类的区别是，object对象不能带参数。

当单例对象与某个类共享同一个名称时，他被称作是这个类的伴生对象：companion object。你必须在同一个源文件里定义类和它的伴生对象。类被称为是这个单例对象的伴生类：companion class。类和它的伴生对象可以互相访问其私有成员。

‍

## 特征

‍

Scala Trait(特征) 相当于 Java 的接口，实际上它比接口还功能强大。

与接口不同的是，它还可以定义属性和方法的实现。

一般情况下Scala的类只能够继承单一父类，但是如果是 Trait(特征) 的话就可以继承多个，从结果来看就是实现了多重继承

子类继承特征可以实现未被实现的方法。所以其实 Scala Trait(特征)更像 Java 的抽象类

```scala
trait Equal {
  def isEqual(x: Any): Boolean
  def isNotEqual(x: Any): Boolean = !isEqual(x)
}
```

‍

‍

‍

‍

‍

‍

‍

‍

‍

‍

## 异常

‍

捕捉异常的 catch 子句，语法与其他语言中不太一样。在 Scala 里，借用了模式匹配的思想来做异常的匹配，因此，在 catch 的代码里，是一系列 case 字句，如下例所示：

‍

catch字句里的内容跟match里的case是完全一样的。由于异常捕捉是按次序，如果最普遍的异常，Throwable，写在最前面，则在它后面的case都捕捉不到，因此需要将它写在最后面。

```scala
object Test {
   def main(args: Array[String]) {
      try {
         val f = new FileReader("input.txt")
      } catch {
         case ex: FileNotFoundException =>{
            println("Missing file exception")
         }
         case ex: IOException => {
            println("IO Exception")
         }
      }
   }
}
```

‍

‍

# 高级

‍

‍

## 提取器

提取器是从传递给它的对象中提取出构造该对象的参数。

Scala 标准库包含了一些预定义的提取器，我们会大致的了解一下它们。

Scala 提取器是一个带有unapply方法的对象。unapply方法算是apply方法的反向操作：unapply接受一个对象，然后从对象中提取值，提取的值通常是用来构造该对象的值。

‍

## IO

Scala 进行文件写操作，直接用的都是 java中 的 I/O 类

从文件读取内容非常简单。我们可以使用 Scala 的 **Source** 类及伴生对象来读取文件

‍

```scala
Source.fromFile("test.txt" ).foreach{
         print
      }
```

‍

## 数构

‍

Scala 集合分为可变的和不可变的集合。

可变集合可以在适当的地方被更新或扩展。这意味着你可以修改，添加，移除一个集合的元素。

而不可变集合类，相比之下，永远不会改变。不过，你仍然可以模拟添加，移除或更新操作。但是这些操作将在每一种情况下都返回一个新的集合，同时使原来的集合不发生改变。

‍

### List

多种类型的列表

```scala
// 字符串列表
val site: List[String] = List("Runoob", "Google", "Baidu")

// 整型列表
val nums: List[Int] = List(1, 2, 3, 4)

// 空列表
val empty: List[Nothing] = List()

// 二维列表
val dim: List[List[Int]] =
   List(
      List(1, 0, 0),
      List(0, 1, 0),
      List(0, 0, 1)
   )
```

‍

构造列表的两个基本单位是 **Nil** 和  **::**

**Nil** 也可以表示为一个空列表。

‍

以上实例我们可以写成如下所示：

```scala
// 字符串列表
val site = "Runoob" :: ("Google" :: ("Baidu" :: Nil))

// 整型列表
val nums = 1 :: (2 :: (3 :: (4 :: Nil)))

// 空列表
val empty = Nil

// 二维列表
val dim = (1 :: (0 :: (0 :: Nil))) ::
          (0 :: (1 :: (0 :: Nil))) ::
          (0 :: (0 :: (1 :: Nil))) :: Nil
```

‍

‍

#### 操作

‍

​`head`​​ 返回列表第一个元素

​`tail`​​ 返回一个列表，包含除了第一元素之外的其他元素

​`isEmpty`​​ 在列表为空时返回true

‍

‍

==连接==

使用  **:::**  运算符 或  **List.:::()**  方法  或 **List.concat()**

```scala
 // 使用 ::: 运算符
      var fruit = site1 ::: site2
      println( "site1 ::: site2 : " + fruit )
   
      // 使用 List.:::() 方法
      fruit = site1.:::(site2)
      println( "site1.:::(site2) : " + fruit )

      // 使用 concat 方法
      fruit = List.concat(site1, site2)
      println( "List.concat(site1, site2) : " + fruit  )
```

‍

==填充==

fill()

创建一个指定重复数量的元素列表

```scala
      val site = List.fill(3)("Runoob") // 重复 Runoob 3次
      println( "site : " + site  )

      val num = List.fill(10)(2)         // 重复元素 2, 10 次
      println( "num : " + num  )
```

‍

==反转==

reverse

‍

### Map

‍

Map 有两种类型，可变与不可变，区别在于可变对象可以修改它，而不可变对象不可以。

默认情况下 Scala 使用不可变 Map。如果你需要使用可变集合，你需要显式的引入 **import scala.collection.mutable.Map** 类

在 Scala 中 你可以同时使用可变与不可变 Map，不可变的直接使用 Map，可变的使用 mutable.Map。以下实例演示了不可变 Map 的应用：

```
// 空哈希表，键为字符串，值为整型
var A:Map[Char,Int] = Map()

// Map 键值对演示
val colors = Map("red" -> "#FF0000", "azure" -> "#F0FFFF")
```

定义 Map 时，需要为键值对定义类型。如果需要添加 key-value 对，可以使用 + 号，如下所示：

```
A += ('I' -> 1)
A += ('J' -> 5)
A += ('K' -> 10)
A += ('L' -> 100)
```

‍

‍

#### 操作

‍

keys	    返回 Map 所有的键(key)  

values	    返回 Map 所有的值(value)  

isEmpty	    在 Map 为空时返回true

‍

基本应用

```scala
val colors = Map("red" -> "#FF0000",
                       "azure" -> "#F0FFFF",
                       "peru" -> "#CD853F")

      val nums: Map[Int, Int] = Map()

      println( "colors 中的键为 : " + colors.keys )
      println( "colors 中的值为 : " + colors.values )
      println( "检测 colors 是否为空 : " + colors.isEmpty )
      println( "检测 nums 是否为空 : " + nums.isEmpty )
```

‍

==合并==

使用  **++**  运算符或 **Map.++()**  方法来连接两个 Map，Map 合并时会移除重复的 key

‍

‍

### Tuple

与列表一样，元组也是不可变的，但与列表不同的是元组可以包含不同类型的元素。

元组的值是通过将单个的值包含在圆括号中构成的。

```scala
val t = (1, 3.14, "Fred")  
或
val t = new Tuple3(1, 3.14, "Fred")
```

‍

可以使用 t._1 访问第一个元素， t._2 访问第二个元素

```scala
      val t = (4,3,2,1)

      val sum = t._1 + t._2 + t._3 + t._4
```

‍

‍

#### 操作

‍

==迭代==

‍

使用 **Tuple.productIterator()**  方法来迭代输出元组的所有元素

```scala
  t.productIterator.foreach{ i =>println("Value = " + i )}
```

‍

‍

‍

### Set

‍

默认情况下，Scala 使用的是不可变集合，如果你想使用可变集合，需要引用 **scala.collection.mutable.Set** 包。

默认引用 scala.collection.immutable.Set，不可变集合实例如下：

```scala
val set = Set(1,2,3)
println(set.getClass.getName) //

println(set.exists(_ % 2 == 0)) //true
println(set.drop(1)) //Set(2,3)
```

‍

#### 操作

‍

​`head`​​ 返回集合第一个元素

​`tail`​​ 返回一个集合，包含除了第一元素之外的其他元素

​`isEmpty`​​ 在集合为空时返回true

使用  **++**  运算符或 **Set.++()**  方法来连接两个集合

‍

‍

‍

### Option

表示一个值是可选的

‍

Option[T] 是一个类型为 T 的可选值的容器： 如果值存在， Option[T] 就是一个 Some[T] ，如果不存在， Option[T] 就是对象 None

‍

‍

### Iterator

不是一个集合，它是一种用于访问集合的方法, 两个基本操作是 **next** 和 **hasNext**
