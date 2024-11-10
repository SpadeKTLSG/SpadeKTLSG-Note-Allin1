--泛用Android开发语言

‍

‍

## Header

​`.kt`​ 作为扩展名

### 快速方法

块注释允许嵌套

### 我的简写

‍

### 版本控制

‍

### 环境相关

使用IDEA的插件即可,不用下载(需要17高版本Java)

‍

‍

# 知识

‍

Kotlin 是运行在 Java 虚拟机上的静态语言，被称之为 Android 世界的 Swift，由 JetBrains 设计开发并开源. Kotlin 可以编译成 JAVA 字节码，也可以编译成 JavaScript，方便在没有 JVM 的设备上运行

‍

## 数学特性

‍

## 底层特性

‍

1. **简洁 :**  大大减少样板代码的数量。
2. **安全 :**  避免空指针异常等整个类的错误。
3. **互操作性 :**  充分利用 JVM、Android 和浏览器的现有库
4. **工具友好 :**  可用任何 Java IDE 或者使用命令行构建

‍

# 基础

‍

‍

## 基本数型

‍

‍

基本数值类型包括 Byte、Short、Int、Long、Float、Double 等

字符不属于数值类型，是一个独立的数据类型

‍

Kotlin只有封装的数字类型

每定义的一个变量，其实 Kotlin 帮我们封装了一个对象，这样可以保证不会出现空指针

数字类型也一样，所有在比较两个数字的时候，就有比较数据大小和比较两个对象是否相同的区别了: 在 Kotlin 中，三个等号 === 表示比较对象地址，两个 == 表示比较两个值大小

‍

‍

### 枚举

可以声明一个枚举类**enum class, ** 最基本的用法是实现一个类型安全的枚举

枚举常量用逗号分隔,每个枚举常量都是一个对象

```
enum class Color{
    RED,BLACK,BLUE,GREEN,WHITE
}
```

‍

#### 枚举常量

Kotlin 中的枚举类具有合成方法，允许遍历定义的枚举常量，并通过其名称获取枚举常数。

```
EnumClass.valueOf(value: String): EnumClass  // 转换指定 name 为枚举值，若未匹配成功，会抛出IllegalArgumentException
EnumClass.values(): Array<EnumClass>        // 以数组的形式，返回枚举值
```

获取枚举相关信息

```
val name: String //获取枚举名称
val ordinal: Int //获取枚举值在所有枚举数组中定义的顺序
```

‍

### 字符

Kotlin 中的 Char 不能直接和数字操作，Char 必需是 `单引号''`​ 包含起来的，比如普通字符 '0'，'a'

```
fun check(c: Char) {
    if (c == 1) { // 错误：类型不兼容
        // ……
    }
}
```

字符字面值用单引号括起来: '1'。 特殊字符可以用反斜杠转义。 支持这几个转义序列：\t、 \b、\n、\r、\'、\"、\ 和 \$。 编码其他字符要用 Unicode 转义序列语法：'\uFF00'。

我们可以显式把字符转换为 Int 数字：

```
fun decimalDigitValue(c: Char): Int {
    if (c !in '0'..'9')
        throw IllegalArgumentException("Out of range")
    return c.toInt() - '0'.toInt() // 显式转换为数字
}
```

当需要可空引用时，像数字、字符会被装箱。装箱操作不会保留同一性。

‍

### 数组

‍

创建

1. 使用函数 arrayOf()
2. 使用工厂函数

下面我们分别用两种方式创建了两个数组

```
fun main(args: Array<String>){
    val a = arrayOf(1, 2, 3)
    val b = Array(3, { i -> (i * 2) })
}
```

如上所述，[] 运算符代表调用成员函数 get() 和 set()

除了类Array，还有ByteArray, ShortArray, IntArray，用来表示各个类型的数组，省去了装箱操作，因此效率更高，其用法同Array一样

‍

‍

### 字符串

‍

String 是不可变的, 使用 `方括号( [] )`​获取字符串中的某个字符

for循遍历

```kotlin
for (c in str) {
    println(c)
}
```

‍

支持三个引号 """ 扩起来的字符串，支持多行字符串

```
fun main(args: Array<String>) {
    val text = """
    多行字符串
    多行字符串
    """
    println(text)   // 输出有一些前置空格
}
```

‍

String通过 trimMargin() 方法来删除多余的空白

```
fun main(args: Array<String>) {
    val text = """
    |多行字符串
    |简单编程
    |多行字符串
    |www.twle.cn
    """.trimMargin()
    println(text)    // 前置空格删除了
}
```

默认 | 用作边界前缀，但你可以选择其他字符并作为参数传入，比如 trimMargin(">")

‍

‍

‍

#### 字符串模板

‍

Kotlin 语言使用 `$`​ 符号在字符串中引用一个变量或者常量

$varName 表示变量值

${varName.fun()} 表示变量的方法返回值

‍

```
fun main(args: Array<String>){
    val i = 10
    val s = "i = $i" // 求值结果为 "i = 10"
    println(s)
}
```

‍

或者用花括号扩起来的任意表达式

```
fun main(args: Array<String>) {
    val s = "简单教程"
    val str = "$s.length is ${s.length}" 
    println(str)
}
```

‍

原生字符串和转义字符串内部都支持模板

‍

在原生字符串中表示字面值 $ 字符（它不支持反斜杠转义）

```
fun main(args: Array<String>){
    val price = """
    ${'$'}9.99
    """
    println(price)  // 求值结果为 $9.99
}
```

‍

‍

### 范围

范围/区间(range)

Kotlin 支持区间表达式

区间表达式由具有操作符形式 `..`​ 的 rangeTo 函数辅以 in 和 !in 形成

区间是为任何可比较类型定义的，但对于整型原生类型，它有一个优化的实现

以下是使用区间的一些范例

```
for (i in 1..4) print(i) // 输出“1234”

for (i in 4..1) print(i) // 什么都不输出

if (i in 1..10) { // 等同于 1 <= i && i <= 10
    println(i)
}

// 使用 step 指定步长
for (i in 1..4 step 2) print(i) // 输出“13”

for (i in 4 downTo 1 step 2) print(i) // 输出“42”


// 使用 until 函数排除结束元素
for (i in 1 until 10) {   // i in [1, 10) 排除了 10
     println(i)
}
```

‍

‍

‍

## 基本量

‍

### 变量

**var** 关键字定义可变变量

‍

```
var <标识符> : <类型> = <初始化值>
```

‍

### 不可变变量

**val** 关键字定义不可变变量, 允许常量与变量定义时可以不用初始化，但在引用前必须初始化

```
val <标识符> : <类型> = <初始化值>
```

‍

‍

### 型变

Kotlin 中没有通配符类型，它有两个其他的东西：声明处型变（declaration-site variance）与类型投影（type projections)

‍

#### 声明处型变

声明处的类型变异使用协变注解修饰符：in、out，消费者 in, 生产者 out

使用 out 使得一个类型参数协变，协变类型参数只能用作输出，可以作为返回值类型但是无法作为入参的类型：

```
// 定义一个支持协变的类
class Twle<out A>(val a: A) {
    fun foo(): A {
        return a
    }
}

fun main(args: Array<String>) {
    var strCo: Twle<String> = Twle("a")
    var anyCo: Twle<Any> = Twle<Any>("b")
    anyCo = strCo
    println(anyCo.foo())   // 输出 a
}
```

in 使得一个类型参数逆变，逆变类型参数只能用作输入，可以作为入参的类型但是无法作为返回值的类型：

```
class Twle<in A>(a: A) {
    fun foo(a: A) {
    }
}

fun main(args: Array<String>) {
    var strDCo = Twle("a")
    var anyDCo = Twle<Any>("b")
    strDCo = anyDCo
}
```

‍

‍

### 星号投射

有些时候, 你可能想表示你并不知道类型参数的任何信息, 但是仍然希望能够安全地使用它.

这里所谓"安全地使用"是指, 对泛型类型定义一个类型投射, 要求这个泛型类型的所有的实体实例, 都是这个投射的子类型

对于这个问题, Kotlin 提供了一种语法, 称为 星号投射(star-projection):

* 假如类型定义为 Foo<out T> , 其中 T 是一个协变的类型参数, 上界(upper bound)为 TUpper ,Foo<  **&gt; 等价于 Foo&lt;out TUpper&gt; . 它表示, 当 T 未知时, 你可以安全地从 Foo&lt;**  > 中 读取TUpper 类型的值.
* 假如类型定义为 Foo<in T> , 其中 T 是一个反向协变的类型参数, Foo<  **&gt; 等价于 Foo&lt;inNothing&gt; . 它表示, 当 T 未知时, 你不能安全地向 Foo&lt;**  > 写入 任何东西.
* 假如类型定义为 Foo<T> , 其中 T 是一个协变的类型参数, 上界(upper bound)为 TUpper , 对于读取值的场合, Foo<*> 等价于 Foo<out TUpper> , 对于写入值的场合, 等价于 Foo<in Nothing> .

如果一个泛型类型中存在多个类型参数, 那么每个类型参数都可以单独的投射. 比如, 如果类型定义为interface Function<in T, out U> , 那么可以出现以下几种星号投射:

* Function<*, String> , 代表 Function<in Nothing, String> ;
* Function<Int, *> , 代表 Function<Int, out Any?> ;
* Function<  **,**  > , 代表 Function<in Nothing, out Any?> .

注意: 星号投射与 Java 的原生类型(raw type)非常类似, 但可以安全使用

‍

‍

## 基础语法

‍

‍

### 函数

使用 `fun`​ 关键字定义函数

‍

```
fun [function_name] ( [parameter list]): [return_type]{
   [statement]
}
```

*  **[function_name]**  是函数名，比如 hello, hello_world 等
*  **[parameter list]**  是参数列表
*  **[return_type]**  是函数的返回值类型
*  **[statement]**  函数体，是具体的实现函数功能的代码语句

‍

当使用表达式作为函数体的时候，因为返回类型自动推断，所以可以省略

```
fun sum(a: Int, b: Int) = a + b
```

‍

当然也有例外，就是 fun 前加上了访问范围修饰符的时候，就要明确写出返回类型

```
public fun sum(a: Int, b: Int): Int = a + b   // public 方法则必须明确写出返回类型
```

‍

如果一个函数没有返回值，可以将返回类型声明为 Unit ( 类似 Java 中的 void )

```
fun printSum(a: Int, b: Int): Unit { 
    print(a + b)
}
```

‍

当然了，如果是返回 Unit 类型，则可以省略( 对于 public 方法也是这样)

```
public fun printSum(a: Int, b: Int) { 
    print(a + b)
}
```

‍

‍

#### 可变长参数

可以用 **vararg** 关键字来修饰函数的参数，那么这个函数就可以接收可变数量的参数

‍

‍

### lambda

Kotlin 支持匿名函数，也就是支持 lambda 格式的函数

‍

```
// 测试
fun main(args: Array<String>) {
    val sumLambda: (Int, Int) -> Int = {x,y -> x+y}
    println(sumLambda(1,2))  // 输出 3
}
```

‍

### 类型

‍

支持自动类型判断,即声明时可以不指定类型,由编译器判断

```
val a: Int = 1
val b = 1       // 系统自动推断变量类型为Int
val c: Int      // 如果不在声明时初始化则必须提供变量类型
c = 1           // 明确赋值


var x = 5        // 系统自动推断变量类型为Int
x += 1           // 变量可修改
```

‍

#### 类型检测 + 自动类型转换

‍

Kotlin 使用 `is`​ 运算符检测一个表达式是否某类型的一个实例，这类似于 Java 中的 instanceof 关键字)

```
fun getStringLength(obj: Any): Int? {
  if (obj is String) {
    // 做过类型判断以后，obj会被系统自动转换为String类型
    return obj.length 
  }

  //在这里还有一种方法，与Java中instanceof不同，使用!is
  // if (obj !is String){
  //   // XXX
  // }

  // 这里的obj仍然是Any类型的引用
  return null
}
```

或者

```
fun getStringLength(obj: Any): Int? {
  if (obj !is String)
    return null
  // 在这个分支中, `obj` 的类型会被自动转换为 `String`
  return obj.length
}
```

甚至还可以

```
fun getStringLength(obj: Any): Int? {
  // 在 `&&` 运算符的右侧, `obj` 的类型会被自动转换为 `String`
  if (obj is String && obj.length > 0)
    return obj.length
  return null
}
```

‍

### 标签

‍

Kotlin 语言支持任何表达式都可以用标签（label）来标记

标签的格式为标识符后跟 `@`​符号，例如：abc@、fooBar@都是有效的标签

要为一个表达式加标签，我们只要在其前加标签即可

```
loop@ for (i in 1..100) {
    // ……
}
```

现在，我们可以用标签限制 break

```
fun main(args: Array<String>) {
    loop@ for (i in 1..5) {
        for (j in 1..3) {
            if ( i == 2 && j == 2 ) break@loop
            println("$i $j")
        }
    }
}
```

‍

### when

‍

将它的参数和所有的分支条件顺序比较，直到某个分支满足条件

when 既可以被当做表达式使用也可以被当做语句使用

1. 如果它被当做表达式，符合条件的分支的值就是整个表达式的值
2. 如果当做语句使用， 则忽略个别分支的值

扩展为when..else 语句

Kotlin when 语句中如果其他分支都不满足条件将会求值 **else** 分支

‍

```Kotlin
fun main(args: Array<String>) {
    var x = 3
    when (x) {
        1 -> print("x == 1")
        2 -> print("x == 2")
        else -> print("x > 2")
    }
}
```

‍

如果很多分支需要用相同的方式处理，则可以把多个分支条件放在一起，用逗号分隔

```kotlin
fun main(args: Array<String>) {
    var x = 3
    when (x) {
        0, 1 -> print("x == 0 or x == 1")
        else -> print("otherwise")
    }
}
```

‍

#### elif

when 也可以用来取代 if-elif 链

如果不提供参数，所有的分支条件都是简单的布尔表达式，而当一个分支的条件为真时则执行该分支：

```
fun main(args: Array<String>) {
    var x = 3
    when {
        x.isOdd() -> print("x is odd")
        x.isEven() -> print("x is even")
        else -> print("x is funny")
    }
```

‍

#### in

可以使用 **in 运算符** 来检测一个值在（in）或者不在（!in）一个区间或者集合中

```
fun main(args: Array<String>) {
    var x = 9
    var validNumbers = arrayOf(1, 2, 3)
    when (x) {
        in 1..10 -> print("x is in the range")
        in validNumbers -> print("x is valid")
        !in 10..20 -> print("x is outside the range")
        else -> print("none of the above")
    }
}
```

‍

#### is

可以使用 **is** 或者  **!is** 运算符来检测一个特定类型的值

> is 运算符因为会智能转换，所以我们可以直接访问该类型的方法和属性而无需任何额外的检测

```
// filename: main.kt
// author: 简单教程(www.twle.cn)
// Copyright © 2015-2065 www.twle.cn. All rights reserved.

fun hasPrefix(x: Any) = when(x) {
    is String -> x.startsWith("hello")
    else -> false
}

fun main(args: Array<String>) {
    print(hasPrefix("hello world"))
    print(hasprefix(1))
}
```

‍

‍

### for

‍

对任何提供迭代器进行遍历

```
for (item in collection) print(item)
```

```
for (item: Int in ints) {
    // ……
}
```

‍

通过索引遍历一个数组或者一个 list

```
for (i in array.indices) {
    print(array[i])
}
```

> 这种"在区间上遍历"会编译成优化的实现而不会创建额外对象

或者你可以用库函数 withIndex

```
for ((index, value) in array.withIndex()) {
    println("the element at $index is $value")
}
```

‍

对集合进行迭代

```
fun main(args: Array<String>) {
    val items = listOf("百度", "腾讯", "阿里巴巴")
    for (item in items) {
        println(item)
    }

    for (index in items.indices) {
        println("item at $index is ${items[index]}")
    }
}
```

‍

### if

if 表达式的结果可以赋值给一个变量

```
val max = if (a > b) {
    print("Choose a")
    a
} else {
    print("Choose b")
    b
}
```

if 的这种功能可以代替其它语言的 **三元操作符**

```
val c = if (condition) a else b
```

‍

### return

‍

用于从函数中返回或者跳出循环

‍

三种结构化跳转表达式：

1. **return :**  默认从最直接包围它的函数或者匿名函数返回
2. **break :**  终止最直接包围它的循环
3. **continue :**  继续下一次最直接包围它的循环

‍

‍

‍

## 特殊操作

‍

‍

‍

## 类

‍

### 构造器

‍

构造函数是 public

如果你不想你的类有公共的构造函数，你就得声明一个空的主构造函数

```
class DontCreateMe private constructor () {
}
```

在 JVM 虚拟机中，如果主构造函数的所有参数都有默认值，编译器会生成一个附加的无参的构造函数，这个构造函数会直接使用默认值

这使得 Kotlin 可以更简单的使用像 Jackson 或者 JPA 这样使用无参构造函数来创建类实例的库

```
class Customer(val customerName: String = "")
```

‍

‍

#### 主构造器

‍

类可以有一个主构造器，以及一个或多个次构造器，主构造器是类头部的一部分，位于类名称之后. 主构造器中不能包含任何代码，初始化代码可以放在初始化代码段中

```
class Person constructor(firstName: String) {}
```

‍

如果主构造器没有任何注解，也没有任何可见度修饰符，那么 constructor 关键字可以省略

‍

初始化代码段使用 init 关键字作为前缀

```
class Person constructor(firstName: String) {
    init {
        System.out.print("FirstName is $firstName")
    }
}
```

‍

主构造器的参数可以在初始化代码段中使用，也可以在类主体定义的属性初始化代码中使用

‍

一种简洁语法，可以通过主构造器来定义属性并初始化属性值（ 可以是 var或 val ）

```
class People(val firstName: String, val lastName: String) 
{
    //...
}
```

如果构造器有注解，或者有可见度修饰符，这时 constructor 关键字是必须的，注解和修饰符要放在它之前

‍

#### 次构造器

使用 constructor 关键词定义二级构造函数

```
class Person { 
    constructor(parent: Person)
    {
        parent.children.add(this) 
    }
}
```

如果类有主构造函数，每个次构造函数都要，或直接或间接通过另一个次构造函数代理主构造函数

在同一个类中代理另一个构造函数使用 this 关键字

```
class Person(val name: String) {
    constructor (name: String, age:Int) : this(name) {
        // 初始化...
    }
}
```

如果一个非抽象类没有声明构造函数(主构造函数或次构造函数)，它会产生一个没有参数的构造函数

‍

### 字段

‍

类不能有字段

提供了 Backing Fields(后端变量) 机制,备用字段使用 field 关键字声明,

field 关键词只能用于属性的访问器

```
var no: Int = 100
        get() = field                // 后端变量
        set(value) {
            if (value < 10) {       // 如果传入的值小于 10 返回该值
                field = value
            } else {
                field = -1         // 如果传入的值大于等于 10 返回 -1
            }
        }
```

非空属性必须在定义的时候初始化

‍

Kotlin 提供了一种可以延迟初始化的方案,使用 lateinit 关键字描述属性

```
class LazyProperty(val initializer: () -> Int) {
    var value: Int? = null
    val lazy: Int
        get() {
            if (value == null) {
                value = initializer()
            }
            return value!!
        }
}
```

‍

### 修饰符

‍

Kotlin 类的修饰符包括 classModifier 和_accessModifier_

1. classModifier: 类属性修饰符，标示类本身特性

    ```
    abstract    // 抽象类  
    final       // 类不可继承，默认属性
    enum        // 枚举类
    open        // 类可继承，类默认是final的
    annotation  // 注解类
    ```
2. accessModifier: 访问权限修饰符

    ```
    private    // 仅在同一个文件中可见
    protected  // 同一个文件中或子类可见
    public     // 所有调用的地方都可见
    internal   // 同一个模块中可见
    ```

‍

### 抽象类

类本身或类中的部分成员，都可以声明为abstract, 抽象成员在类中不存在具体的实现

使用 `abstract`​ 关键字来定义抽象类, 无需对抽象类或抽象成员标注 open 注解

```
open class Base {
    open fun f() {}
}

abstract class Derived : Base() {
    override abstract fun f()
}
```

‍

### 内部类

‍

支持一个类内部在定义一个类, 使用 `inner class`​ 关键字定义内部类

Kotlin 内部类与嵌套类的区别是：

1. 内部类会带有一个外部类的对象的引用，嵌套类则没有
2. 内部类需要使用 `inner class`​ 定义，而嵌套类则使用 `class`​ 定义

Kotlin 内部类会带有一个对外部类的对象的引用，所以内部类可以访问外部类成员属性和成员函数

Kotlin 内部类使用 `this@[外部类名]`​ 持有外部类对象的引用

```
// filename: main.kt
// author: 简单教程(www.twle.cn)
// Copyright © 2015-2065 www.twle.cn. All rights reserved.


class Outer {
    private val bar: Int = 1
    var v = "成员属性"
    /**嵌套内部类**/
    inner class Inner {
        fun foo() = bar  // 访问外部类成员
        fun innerTest() {
            var o = this@Outer //获取外部类的成员变量
            println("内部类可以引用外部类的成员，例如：" + o.v)
        }
    }
}

fun main(args: Array<String>) {
    val demo = Outer().Inner().foo()
    println(demo) //   1
    val demo2 = Outer().Inner().innerTest()   
    println(demo2)   // 内部类可以引用外部类的成员，例如：成员属性
}
```

‍

### 密封类

使用 **sealed class** 关键字创建一个密封类, 用来表示受限的类继承结构：当一个值为有限集中的类型, 而不能有任何其他类型时

在某种意义上，它们是枚举类的扩展：枚举类型的值集合 也是受限的，但每个枚举常量只存在一个实例，而密封类 的一个子类可以有可包含状态的多个实例

声明一个密封类，使用 **sealed** 修饰类，密封类可以有子类，但是所有的子类都必须要内嵌在密封类中

sealed 关键字不能修饰 interface ,abstract class(会报 warning,但是不会出现编译错误)

```
sealed class Expr
data class Const(val number: Double) : Expr()
data class Sum(val e1: Expr, val e2: Expr) : Expr()
object NotANumber : Expr()

fun eval(expr: Expr): Double = when (expr) {
    is Const -> expr.number
    is Sum -> eval(expr.e1) + eval(expr.e2)
    NotANumber -> Double.NaN
}
```

使用密封类的关键好处在于使用 when 表达式的时候，如果能够 验证语句覆盖了所有情况，就不需要为该语句再添加一个 else 子句了

```
fun eval(expr: Expr): Double = when(expr) {
    is Expr.Const -> expr.number
    is Expr.Sum -> eval(expr.e1) + eval(expr.e2)
    Expr.NotANumber -> Double.NaN
    // 不再需要 `else` 子句，因为我们已经覆盖了所有的情况
}
```

‍

### 数据类

使用 **data class** 创建一个只包含数据的类，这种类又称数据类, 是只包含属性布不包含方法的特殊类

```
data class User(val name: String, val age: Int)
```

编译器会自动的从主构造函数中根据所有声明的属性提取以下函数：

* ​`equals()`​/`hashCode()`​
* ​`toString()`​格式如`"User(name=John, age=42)"`​
* ​`componentN() functions`​对应于属性，按声明顺序排列
* ​`copy()`​函数

如果这些函数在类中已经被明确定义了，或者从超类中继承而来，就不再会生成。

为了保证生成代码的一致性以及有意义，数据类需要满足以下条件：

* 主构造函数至少包含一个参数
* 所有的主构造函数的参数必须标识为`val`​或者`var`​;
* 数据类不可以声明为`abstract`​,`open`​,`sealed`​或者`inner`​;
* 数据类不能继承其他类 (但是可以实现接口)

‍

‍

组件函数允许数据类在解构声明中使用

```
val jane = User("Jane", 35)
val (name, age) = jane
println("$name, $age years of age") // prints "Jane, 35 years of age"
```

‍

#### 标准数据类

标准库提供了 `Pair`​和 `Triple`​ 两个标准的数据类

在大多数情形中，命名数据类是更好的设计选择，因为这样代码可读性更强而且提供了有意义的名字和属性

‍

‍

### 泛型

Kotlin 语言支持泛型，为为类型安全提供保证，消除类型强转的烦恼

泛型，即 "参数化类型"，将类型参数化，可以用在类，接口，方法上

```
class Box<T>(t: T) {
    var value = t
}
```

创建泛型类的实例时我们需要指定类型参数

```
val box: Box<Int> = Box<Int>(1)
```

或者

```
val box = Box(1) // 编译器会进行类型推断，1 类型 Int，所以编译器知道我们说的是 Box<Int>
```

‍

#### 泛型约束

泛型约束可以用来设定一个给定参数允许使用的类型

Kotlin 中使用 **冒号(:)**  对泛型的的类型上限进行约束

最常见的约束是上界(upper bound)：

```
fun <T : Comparable<T>> sort(list: List<T>) {
    // ……
}
```

‍

Kotlin 泛型约束默认的上界是 `Any?`​

对于多个上界约束条件，可以用 where 子句：

```
fun <T> cloneWhenGreater(list: List<T>, threshold: T): List<T>
    where T : Comparable, Cloneable {
      return list.filter(it > threshold).map(it.clone())
    }
```

‍

### 继承

‍

Any 类是所有类的超类，对于没有超类型声明的类是默认超类

Kotlin 规定如果一个类可以给继承，必须使用 **open** 关键字修饰

```
class Example // 从 Any 隐式继承
```

Any 默认提供了三个函数：

```
equals()

hashCode()

toString()
```

如果一个类要被继承，可以使用 open 关键字进行修饰

```
open class Base(p: Int)           // 定义基类

class Derived(p: Int) : Base(p)
```

‍

#### 子类构造问题

如果子类有主构造函数， 则基类必须在主构造函数中立即初始化。

如果子类没有主构造函数，则必须在每一个二级构造函数中用 super 关键字初始化基类，或者在代理另一个构造函数

```
open class Person(var name : String, var age : Int){// 基类

}

class Student(name : String, age : Int, var no : String, var score : Int) : Person(name, age) {

}

// 测试
fun main(args: Array<String>)
{
    val s =  Student("Runoob", 18, "S12346", 89)
    println("学生名： ${s.name}")
    println("年龄： ${s.age}")
    println("学生号： ${s.no}")
    println("成绩： ${s.score}")
}
```

‍

初始化基类时，可以调用基类的不同构造方法

```
calss Student : Person {

    constructor(ctx: Context) : super(ctx) {
    } 

    constructor(ctx: Context, attrs: AttributeSet) : super(ctx,attrs) {
    }
}
```

‍

‍

### 重写

在基类中，使用 fun 声明函数时，此函数默认为 final 修饰，不能被子类重写

如果允许子类重写该函数，那么就要手动添加 open 修饰它, 子类重写方法使用 override 关键词

‍

```kotlin
open class Person{
    open fun study(){       // 允许子类重写
        println("我毕业了")
    }
}

/**子类继承 Person 类**/
class Student : Person() {

    override fun study(){    // 重写方法
        println("我在读大学")
    }
}

fun main(args: Array<String>) {
    val s =  Student()
    s.study();

}
```

‍

#### 属性重写

属性重写使用 override 关键字，属性必须具有兼容类型，每一个声明的属性都可以通过初始化程序或者 getter 方法被重写

```
open class Foo {
    open val x: Int get { …… }
}

class Bar1 : Foo() {
    override val x: Int = ……
}
```

我们可以用一个 var 属性重写一个 val 属性，但是反过来不行

因为 val 属性本身定义了 getter 方法，重写为 var 属性会在衍生类中额外声明一个setter方法

我们可以在主构造函数中使用 override 关键字作为属性声明的一部分

```
interface Foo {
    val count: Int
}

class Bar1(override val count: Int) : Foo

class Bar2 : Foo {
    override var count: Int = 0
}
```

‍

### 复制

复制使用 copy() 函数，我们可以使用该函数复制对象并修改部分属性, 对于上文的 User 类，其实现会类似下面这样：

```
fun copy(name: String = this.name, age: Int = this.age) = User(name, age)
```

‍

## 接口

‍

在类名或者对象名后面使用 **冒号(:)+接口名**

必须实现接口中所有没有默认实现的方法

```
class MyClass : MyInterface {
    override fun bar() {
    }
}
```

‍

## 异常

‍

### NULL检查

Kotlin 使用一种叫 **空安全** 的机制来处理声明可为空的参数

在使用声明可为空的参数时要进行空判断处理，有两种处理方式:

1. 字段后加 `!!`​，会像 Java 一样抛出空异常
2. 字段后加 `?`​，可不做处理返回值为 null 或配合 ?: 做空判断处理

```
//类型后面加?表示可为空
var age: String? = "23" 
//抛出空指针异常
val ages = age!!.toInt()
//不做处理返回 null
val ages1 = age?.toInt()
//age为空返回-1
val ages2 = age?.toInt() ?: -1
```

当一个引用可能为 null 值时, 对应的类型声明必须明确地标记为可为 null

‍

# 高级

‍

## IO

‍

‍

## 扩展

Kotlin 允许对一个类的属性和方法进行扩展，且不需要继承或使用 Decorator 模式

Kotlin 扩展是一种静态行为，对被扩展的类代码本身不会造成任何影响

‍

### 扩展函数

扩展函数可以在已有类中添加新的方法，不会对原类做修改，扩展函数定义形式：

```
fun receiverType.functionName(params){
    body
}
```

* receiverType：表示函数的接收者，也就是函数扩展的对象
* functionName：扩展函数的名称
* params：扩展函数的参数，可以为NULL

‍

下面范例我们扩展了 User 类

```
/**扩展函数**/
fun User.Print(){
    print("用户名 $name")
}

fun main(arg:Array<String>){
    var user = User("www.twle.cn")
    user.Print()
}
```

‍

下面代码为 MutableList添加一个swap 函数：

```
// 扩展函数 swap,调换不同位置的值
fun MutableList<Int>.swap(index1: Int, index2: Int){
    val tmp = this[index1]     //  this 对应该列表
    this[index1] = this[index2]
    this[index2] = tmp
}

fun main(args: Array<String>) {

    val l = mutableListOf(1, 2, 3)
    // 位置 0 和 2 的值做了互换
    l.swap(0, 2) // 'swap()' 函数内的 'this' 将指向 'l' 的值

    println(l.toString())
}
```

‍

#### 静态解析

扩展函数是静态解析的，并不是接收者类型的虚拟成员，在调用扩展函数时，具体被调用的的是哪一个函数，由调用函数的的对象表达式来决定的，而不是动态的类型决定的

```
open class C

class D: C()

fun C.foo() = "c"   // 扩展函数 foo

fun D.foo() = "d"   // 扩展函数 foo

fun printFoo(c: C) {
    println(c.foo())  // 类型是 C 类
}

fun main(arg:Array<String>){
    printFoo(D())
}
```

‍

若扩展函数和成员函数一致，则使用该函数时，会优先使用成员函数。

‍

#### 扩展空对象

在扩展函数内， 可以通过 this 来判断接收者是否为 null

这样，即使接收者为 null，也可以调用扩展函数

```
fun Any?.toString(): String
{
    if (this == null) return "null"
    // 空检测之后，“this”会自动转换为非空类型，所以下面的 toString()
    // 解析为 Any 类的成员函数
    return toString()
}

fun main(arg:Array<String>)
{
    var t = null
    println(t.toString())
}
```

‍

### 扩展属性

除了函数，Kotlin 也支持属性对属性进行扩展

```
val <T> List<T>.lastIndex: Int
    get() = size - 1
```

扩展属性允许定义在类或者包内，不允许定义在函数中

初始化属性因为属性没有后端字段（backing field），所以不允许被初始化，只能由显式提供的 getter/setter 定义。

```
val Foo.bar = 1 // 错误：扩展属性不能有初始化器
```

扩展属性只能被声明为 val

‍

‍

### 扩展伴生对象

如果一个类定义有一个伴生对象 ，Kotlin 允许为伴生对象定义扩展函数和属性

伴生对象通过 `类名.`​ 形式调用伴生对象，伴生对象声明的扩展函数，通过用类名限定符来调用

```
class MyClass
{
    companion object { }  // 将被称为 "Companion"
}

fun MyClass.Companion.foo()
{
    println("伴随对象的扩展函数")
}

val MyClass.Companion.no: Int
    get() = 10

fun main(args: Array<String>)
{
    println("no:${MyClass.no}")
    MyClass.foo()
}
```

‍

‍

### 扩展作用域

通常扩展函数或属性定义在顶级包下

```
package foo.bar

fun Baz.goo() { …… }
```

要使用所定义包之外的一个扩展, 通过 import 导入扩展的函数名进行使用

```
package com.example.usage

import foo.bar.goo // 导入所有名为 goo 的扩展
                   // 或者
import foo.bar.*   // 从 foo.bar 导入一切

fun usage(baz: Baz) {
    baz.goo()
}
```

‍

### 扩展声明为成员

在一个类内部为另一个类声明扩展方法

在这样的扩展中，有个多个隐含的接受者，其中扩展方法定义所在类的实例称为分发接受者

而扩展方法的目标类型的实例称为扩展接受者

```
class D 
{
    fun bar() { println("D bar") }
}

class C 
{
    fun baz() { println("C baz") }

    fun D.foo() {
        bar()   // 调用 D.bar
        baz()   // 调用 C.baz
    }

    fun caller(d: D) {
        d.foo()   // 调用扩展函数
    }
}

fun main(args: Array<String>)
{
    val c: C = C()
    val d: D = D()
    c.caller(d)

}
```

编译运行以上 Kotlin 范例，输出结果如下

```
$ kotlinc main.kt -include-runtime -d main.jar 
$ java -jar main.jar
D bar
C baz
```

在 C 类内，创建了 D 类的扩展。此时，C 被成为分发接受者，而 D 为扩展接受者

从上例中可以看到，在扩展函数中，可以调用派发接收者的成员函数

假如在调用某一个函数，而该函数在分发接受者和扩展接受者均存在，则以扩展接收者优先，要引用分发接收者的成员你可以使用限定的 this 语法

```
// filename: main.kt
// author: 简单教程(www.twle.cn)
// Copyright © 2015-2065 www.twle.cn. All rights reserved.

class D
{
    fun bar() { println("D bar") }
}

class C
{
    fun bar() { println("C bar") }  // 与 D 类 的 bar 同名

    fun D.foo() {
        bar()         // 调用 D.bar()，扩展接收者优先
        this@C.bar()  // 调用 C.bar()
    }

    fun caller(d: D) {
        d.foo()   // 调用扩展函数
    }
}

fun main(args: Array<String>) {
    val c: C = C()
    val d: D = D()
    c.caller(d)

}
```

编译运行以上 Kotlin 范例，输出结果如下

```
$ kotlinc main.kt -include-runtime -d main.jar 
$ java -jar main.jar
D bar
C bar
```

以成员的形式定义的扩展函数, 可以声明为 open , 而且可以在子类中覆盖

也就是说, 在这类扩展函数的派发过程中, 针对分发接受者是虚拟的(virtual), 但针对扩展接受者仍然是静态的

```
// filename: main.kt
// author: 简单教程(www.twle.cn)
// Copyright © 2015-2065 www.twle.cn. All rights reserved.

open class D 
{
}

class D1 : D() 
{
}

open class C 
{
    open fun D.foo() {
        println("D.foo in C")
    }

    open fun D1.foo() {
        println("D1.foo in C")
    }

    fun caller(d: D) {
        d.foo()   // 调用扩展函数
    }
}

class C1 : C()
{
    override fun D.foo() {
        println("D.foo in C1")
    }

    override fun D1.foo() {
        println("D1.foo in C1")
    }
}


fun main(args: Array<String>)
{
    C().caller(D())   // 输出 "D.foo in C"
    C1().caller(D())  // 输出 "D.foo in C1" —— 分发接收者虚拟解析
    C().caller(D1())  // 输出 "D.foo in C" —— 扩展接收者静态解析

}
```

编译运行以上 Kotlin 范例，输出结果如下

```
$ kotlinc main.kt -include-runtime -d main.jar 
$ java -jar main.jar
D.foo in C
D.foo in C1
D.foo in C
```

‍

‍

‍
