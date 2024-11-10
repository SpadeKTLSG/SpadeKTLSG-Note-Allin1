--Web编程语言

‍

web前端的脚本语言，主要为HTML页面布局进行提供动态支持

现在主要转移到TS学习

‍

‍

### Header

‍

#### 环境

‍

一律以HTML5为准, 将合并新特性

‍

‍

‍

‍

# 知识

‍

‍

‍

‍

## 底层

‍

‍

‍

‍

## 命名

‍

### 命名格式

‍

### 注释

‍

注释 **//**   
多行注释 **/**/**

‍

‍

## 管理

‍

‍

## 版本

> 版本变化特性记录

‍

‍

## 特性

‍

结尾的分号可有可无, 当然还是建议加上

是弱类型语言, 拥有动态类型(变量可以存储不同类型的语言)

很多时候，你需要避免使用 **new** 关键字

具有声明提升

+运算符能把文本值或字符串变量加起来

‍

‍

### 严格模式

(strict mode)

'use strict' 指令只允许出现在脚本或函数的开头,推荐使用

在严格的条件下运行,不能使用未声明的变量,不允许删除变量或对象和函数,不允许变量重名,不允许使用转义字符,不允许对只读属性赋值

‍

‍

# 基础

‍

## 引入

在HTML中必须位于  **&lt;script&gt;** 之间, 一般放在body的底部,可以改善显示速度

‍

### 格式

内部标签

​`<></>`​

‍

外部引入

​`< src=""></>`​

‍

‍

​`<head>`​标签中

```html
<head>
    <script>
        /*打印语句*/
        alert(111);
    </script>
</head>
```

‍

外部文件引用

```html
<script src="js01.js"></script>
```

‍

页面的底部

```html
<html>
<body>
</body>
    <!--书写方式三：js代码写在页面的底部-->
    <script>
        alert(333);
    </script>
</html>
```

‍

直接写在HTML标签中（不常用）

```javascript
<a href="javascript:alert(123)">测试链接</a>
```

‍

‍

‍

## 数型

‍

* Number类型：数值类型
* String类型：字符串类型
* Boolean类型：布尔类型
* Null类型：空
* Undefined类型：未定义
* Symbol类型：唯一（不与其他的值相等）

‍

‍

### 操作

‍

#### 查看类型

**typeof**     查看变量的数据类型

‍

### 基本类型

基本数据类型存储在栈中（栈区指内存里的栈内存），占用的内存较小，可以直接操作它们的值，而且是按值传递的，即一个变量的值是直接存储在变量中的，它们的比较也是按值进行的

‍

‍

#### Boolean

布尔型

```javascript
var b1 = true;
var b2 = false;
console.log(b1 && b2);//false
console.log(b1 || b2);//true
console.log(b1 | b2);//1  1:表示true
console.log(b1 & b2);//0  0:表示false
```

‍

‍

#### Number

整数或浮点数

‍

##### 基础使用

```javascript
console.log(Number.MAX_VALUE);//最大值 1.7976931348623157e+308
console.log(Number.MIN_VALUE);//最小值 5e-324

console.log(Number.POSITIVE_INFINITY);//超出最大值是 无穷大 Infinity
console.log(Number.NEGATIVE_INFINITY);//无穷小 -Infinity

console.log(Number.NaN);//非数字
```

‍

‍

##### 精度问题

```java
var a = 0.9;
var b = 0.999999999999999999999;//1 精度丢失
console.log(a);
console.log(b);

var a1 = 0.1;
var a2 = 0.2;
console.log(a1 + a2);//0.30000000000000004
```

‍

‍

##### 底层默认浮点型

‍

```javascript
var a3 = 10;//底层为：10.0
var a4 = 10.0;
console.log(typeof a3);//Number
console.log(typeof a4);//Number
```

‍

‍

##### 八进制和十六进制

‍

```javascript
var n1 = 0o377;//八进制，前缀是：0或者0o
var n2 = 0xAEB;//十六进制，前缀是：0x
```

‍

‍

##### 判断一个数值是否为数字

isNaN()    全局方法，直接使用isNaN()

‍

‍

```javascript
console.log(isNaN("abad"));//false:数字    true：非数字
```

‍

‍

#### **BigInt**

任意精度的整数

安全地存储和操作大整数，甚至可以超过数字的安全整数限制

‍

‍

#### null

空，什么都没有

一个表明 null 值的特殊关键字. JavaScript 是大小写敏感的，因此 null 与 Null、NULL或变体完全不同

```javascript
/*4.Null:空，什么都没有*/
var x1 = null;
/*这里的方法IsNull() 是官方提供的。如果为null，返回true；否则返回false*/
console.log(IsNull(x1));
```

‍

‍

#### **undefined**

未定义. 声明变量时没有直接初始化, 表示变量未赋值时的属性

```javascript
var x2;
console.log(x2);
```

‍

‍

#### Symbol

一种实例是唯一且不可改变的数据类型

> ECMAScript 6 中新添加的类型

```javascript
var m1 = Symbol(1);
var m2 = Symbol(1);
console.log(m1 === m2);//false
```

‍

#### String

‍

##### 属性

```javascript
var s1 = "这是一个字符串";
console.log(s1.length);
```

‍

##### CRUD

‍

###### 增

‍

###### 删

‍

‍

‍

###### 改

‍

split("XX")    按XX拆分 返回数组

大小写转换

toLowerCase() 转为小写

toUpperCase() 转为大写

‍

‍

###### 查

‍

indexOf()

match()    字符串的匹配 -> 匹配上，返回结果对象; 没有就返回null

‍

‍

#### enum

枚举

```java
enum Color {Red, Green, Blue};
let c: Color = Color.Blue;
console.log(c);    // 输出 2
```

‍

‍

### 引用类型

引用类型的对象存储于堆中，占用的内存较大，不能直接操作它们的值，而是需要通过引用来访问它们的属性和方法，它们的赋值和比较也是按引用进行的，即一个变量的值是一个指向实际对象的引用. 

对象(Object)、数组(Array)、函数(Function); 还有两个特殊的对象：正则（RegExp）和日期（Date）

‍

‍

#### Object

对象

Object Array、Function 等

‍

‍

#### Array

属于对象类型

‍

##### 特点

* 可以存储不同数据类型的数据
* 长度可变

JS中的数组元素不必相同, 同时尽量使用[]表示数组, 取数组越界会Undefine异常

‍

##### CRUD

‍

###### 增

‍

定义​

​`new Array()`​    new数组

​`[1, 2, true, "qwer"]`​    直接量定义数组

```javascript
/*数组的定义：4种*/
var a1 = new Array();//定义一个空的数组
var a2 = new Array(3);//长度为3的数组
var a3 = new Array("123", true, 3.14);//初始化赋值
var a4 = [1, 2, true, "qwer"];//直接量定义数组
```

‍

​​push()​    数组最后添加元素

‍

‍

###### 删

​​pop()​    删除并返回数组最后一个元素

​​shift()​    删除并返回数组的第一个元素

‍

‍

###### 改

排序<sup>（默认按照字符的字典顺序从小到大排列）</sup>    ​​sort()​

‍

在`sort()`​参数中重写`function`​，`a-b`​为冒泡升序

```javascript
var a5 = [1, 2, 3, 10, 100, 33, 22];
console.log(a5.sort());//默认按照字符的字典顺序从小到大排列
var a6 = a5.sort(function (a, b) {//内部为冒泡算法
    return a - b;//a-b结果，如果是负数，a,b不交换
    //a-b结果，如果是整数，a,b交换
});
console.log(a6);
```

‍

###### 查

‍

##### 自动扩容

数组自动扩容，扩容时不会出现数组越界，但是返回值是undefined

‍

‍

### 字面量

字面量是一个**值**

一般固定值称为字面量(数字字面量,字符串字面量)

‍

‍

#### 分类

‍

**表达式字面量**

用于计算

‍

**数组（Array）字面量**

​`[40, 100, 1, 5, 25, 10]`​

‍

**对象（Object）字面量**

定义一个对象：`{firstName:"John", lastName:"Doe", age:50, eyeColor:"blue"}`​

‍

**函数（Function）字面量**

定义一个函数：`function myFunction(a, b) { return a * b;}`​

‍

‍

### 变量

‍

**var** 来定义变量, 普通定义为全局,可以重复定义

可以在一条语句中声明很多变量: 以 var 开头，并使用逗号分隔变量即可,声明也可横跨多行

变量声明时如不使用 var 就是一个全局变量, 即便在函数内定义

‍

const    定义一个常量,不能再改变

let    定义局部变量

‍

‍

‍

### 类型转换

‍

‍

#### 自动

js内部的函数在执行时，会根据函数的参数以及函数的定义，自动尝试转换为所期望的类型

尝试操作一个 "错误" 的数据类型时，会自动转换为 "正确" 的数据类型(会自动调用变量的 toString() 方法; 如果变量不能转换，它仍然会是一个数字，但值为 NaN)

‍

示例

```javascript
<script>
    /*问题：（）中的参数如何自动转换的*/
    /*答案：js内部的函数在执行时，会根据函数的参数以及函数的定义，自动尝试
     		转换为所期望的类型
     */
    console.log(isNaN(1));//false：数字  true：NaN
    console.log(isNaN("1"));//false --- 发生自动转换
    console.log(isNaN("123abc"));//true --- 自动转换失败
    console.log(isNaN(""));//false：""转为0，再判断
    console.log(isNaN(null));//false：null转为0
    console.log(isNaN(true));//false：true转为1

</script>
```

‍

‍

#### 强制

‍

用js函数进行强制类型转换

‍

数字转为非数字​String()​

非数字转为数字主要使用Number()

‍

|原始数据|转换后|
|“ ”|0|
| :----------: | :------------: |
|“123”|123|
|“123abc”|NaN：非数字|
|true|1|
|false|0|
|new Date()|日期的毫秒数|

‍

‍

‍

##### ==数字-&gt;字符串==

1. 全局方法 **String()**
2. Number 方法 **toString()**

‍

‍

##### ==字符串-&gt;数字==

1. 全局方法 Number()
2. parseInt()
3. Operator +

‍

‍

##### 示例

```javascript
/*1.数字转为非数字*/
var a1 = 10;
console.log(String(a1));
console.log(a1.toString());

/*2.非数字转为数字: 主要使用Number()*/
var a2 = " ";//多个空格转为数字时,都成了0
var a3 = "123";//如果时非数字的字符串（123qwe），会转为NaN,表示非数字
var a4 = true;//true:1   false:0
var a5 = new Date();
console.log(Number(a2));//0
console.log(Number(a3));//123
console.log(Number(a4));//1
console.log(Number(a5));//1575339378151 ：日期的毫秒数
```

‍

‍

#### isNaN()

对应表

|原有类型|转换后|
| -------------------------| ---------------|
|“123”：数字字符串|123：对应数字|
|“123abc”:非数字字符串|NaN|
|“ ”|0|
|null|0|
|true|1|

‍

‍

## 基础语法

‍

### 快速方法

‍

‍

### 语法糖

‍

> 注意, 这里内容与IDE本身的补全应该区别开来, 这里是Java语言自带的简写方法. IDE见对应模板
>
> 当然, 鄙人才疏学浅, 可能大多数情况下做不到刻意区分(能用就行, 能用就行...)

‍

### 数算

‍

#### 三元运算

```javascript
(bool)? 1 : 2
```

bool==true, 1; else 2

‍

‍

#### 相等比较

‍

=== 绝对等于（值和类型均相等）

!== 不绝对等于（值和类型有一个不相等，或两个都不相等）

‍

‍

### 运算符

‍

算术运算符：+ - * / % -- ++

逻辑运算符：|| && ！

位运算符：| &

关系运算符：> < == === <= >= !=

赋值运算符：+= -+ *= /= %=

三目运算符：同java

‍

‍

#### 关系运算符

‍

##### 特殊

if括弧中一个`=`​表示==先进行赋值操作，再用赋值后的值判断==

‍

```javascript
var a = 10;
//if(a=20):先进行赋值操作，a变成了20；
if (a = 20) {//js中，如果判断条件是0/null：false  非零/非null：true
    console.log(a);
} else {
    console.log("不相等");
}
```

‍

‍

### 拷贝

‍

### Output

|api|描述|
| :----------------: | :--------------------------------------------: |
|window.alert()|警告框|
|document.write()|在HTML输出内容 - 完成加载后执行,页面将被覆盖|
|console.log()|写入浏览器控制台|
|innerHTML()|写入到 HTML 元素|

‍

没有自带的控制台输出

```javascript
function print(){
  document.write("这一句话是由一个自定义函数打印");
}
print();
```

‍

### Input

‍

‍

‍

### 流程控制

‍

‍

#### for/in

js中没有foreach，有for/in循环结构(类Python)

​`for (var i in arr){}`​：==i是 数组中的index下标==

‍

‍

**示例**

```javascript
    /*1.for循环*/
    for (var i = 0; i < 10; i++) {
        console.log(i);
    }

    /*2.while循环*/
    var a = 1;
    while (a < 10) {
        a++;
    }

    /*3.do-while循环*/
    do {
        a++;
    } while (a<20);

    /*4.for/in 循环：与Java中的foreach完全不同*/
    /*因为js是弱类型的，所以数组的定义时也是弱类型，其中的元素也是弱类型的*/
    var arr = [1, "2", true, false, 3.4, null];//js数组使用[]
    for (var i in arr) {
        /*特别注意：输出的i是  数组中的index下标*/
        console.log(i);
    }
```

‍

‍

### Format

‍

格式化输出'${变量名}'

‍

‍

‍

## 进阶语法

‍

‍

‍

‍

## 数学

浮点数运算不精确,尽量避免

‍

‍

‍

## 方法

> ==函数

‍

==JS中的函数是一堆可执行代码的合集. ==在需要的时候可以通过函数的名字调用其中的代码. 函数可以理解为一种特殊的对象，其实==本质上就是一段可执行的字符串==

JS中的函数可以认为是一种特殊的对象，可以任意的赋值给不同的引用甚至通过方法来当做参数传递，唯一特殊的是，通过跟一对小括号可以执行函数中的代码. 其实js是一门解释运行的语言，函数的本质就是一段js代码的字符串，来回赋值、来回传递都可以，一但跟一对小括号，就执行这段js代码字符串. 

‍

‍

### 特点

一般而言，this指向函数执行时的当前对象

‍

#### 嵌套函数

所有函数都能访问**它们上一层的作用域**

‍

#### 提升

（Hoisting）默认将当前作用域提升到前面去, 应用在变量的声明与函数的声明

函数可以在声明之前调用

‍

‍

### 参数

‍

‍

#### 数量对应

参数列表可以随意定义长度，参数的个数不确定, ==实参的数量可以和形参的数量不一致. ==

* ==参数列表如果参数较少时，其他参数是undefined未定义的==
* ==参数列表传入的参数较多时，其他多余参数不做计算==
* 如果实参少于形参，则形参依次赋值，没有被赋值到的形参取值为undefined.
* 如果实参多余形参，则依次赋值，对于没有被赋值到的实参也不会丢失，可以在方法中通过arguments来获取

‍

‍

#### 参数传递

在函数中调用的参数是函数的隐式参数, 只是传值  
如果引用对象的值就会修改其原始值

‍

‍

### 创建

‍

‍

#### 普通

```javascript
function f1() {
    console.log("无参函数");
}
function f2(a,b,x) {
    console.log(x);
    return a - b;
}
```

‍

#### 动态

()中可以有N个参数，前N-1个是形参，最后一个是方法体

```javascript
var f3 = new Function("a", "b", "return a-b");
console.log(f3);
```

‍

‍

#### 静态方法

使用 static 关键字修饰的方法，又叫类方法

‍

‍

#### 匿名函数

```javascript
var f4 = function (a, b) {
    return a - b;
}
```

‍

#### 自调用函数

‍

函数表达式可以 "自调用" , 自调用表达式会自动调用, 如果表达式后面紧跟 ()

通过添加括号来说明它是一个函数表达式

不能自调用声明的函数

```javascript
(function () {
    var x = "Hello!!";      // 我将调用自己
})();
```

‍

#### 箭头函数

> 箭头函数是不能提升的，所以需要在使用之前定义

箭头函数表达式的语法简洁, 不适合定义一个对象的方法(有的箭头函数都没有自己的 this)

会默认绑定外层 this 的值 (在箭头函数中 this 的值和外层的 this 是一样的)  
如果函数部分只是一条语句，则可以省略<sup>（比较好的习惯）</sup> return 关键字和大括号 {}

‍

##### 语法

(参数1,参数N) => { 函数声明 }

(参数1,参数N) => 表达式(单一)

相当于：(参数1, 参数2, …, 参数N) =>{ return 表达式; }

‍

当只有一个参数时，圆括号是可选的：  
(单一参数) => {函数声明}  
单一参数 => {函数声明}

‍

没有参数的函数应该写成一对圆括号  
() => {函数声明}

‍

##### 函数表达式

在函数表达式存储在变量后，变量也可作为一个函数使用, 实际上是一个匿名函数

```javascript
var x = function (a, b) {return a * b};
var z = x(4, 3);
```

‍

##### 函数构造器

函数同样可以通过内置的函数构造器Function()定义

```javascript
var myFunction = new Function("a", "b", "return a * b");
var x = myFunction(4, 3);

实际上，你不必使用构造函数;很多时候，你需要避免使用 new 关键字。
var myFunction = function (a, b) {return a * b};
var x = myFunction(4, 3);
```

‍

‍

### 调用

‍

* ==函数调用==
* ==全局对象==

  函数始终是默认的全局对象

  > HTML中默认的全局对象<sup>（实例返回this 的值是 window 对象）</sup>是页面本身，所以函数属于 HTML 页面<sup>（在浏览器中的页面对象是浏览器窗口(window 对象)）</sup>; 函数会自动变为 window<sup>(使用 window 对象作为一个变量容易造成程序崩溃)</sup> 对象的函数<sup>（myFunction() 和 window.myFunction() 是一样的）</sup>
  >
* ==方法调用==
* ==构造函数调用==

  使用**new**调用构造函数, 创建一个新的对象
* ==函数方法调用==

  第一个参数必须是对象本身

  严格模式下调用函数时第一个参数会成为 **this** 的值， 即使该参数不是一个对象

  非严格模式下, 如果第一个参数的值是 null 或 undefined, 它将使用全局对象替代

‍

‍

‍

‍

## OO

> 参考Java

‍

‍

### 类要素

‍

#### 类表达式

类表达式可以命名或不命名. 命名类表达式的名称是该类体的局部名称

‍

例子

```javascript
// 未命名/匿名类
let Runoob = class {
  constructor(name, url) {
    this.name = name;
    this.url = url;
  }
};

// 命名类
let Runoob = class Runoob2 {
  constructor(name, url) {
    this.name = name;
    this.url = url;
  }
};
```

‍

```javascript
class ClassName {
  constructor() { ... }   //构造器
}
class Runoob {
  constructor(name, url) {
    this.name = name;
    this.url = url;
  }
} 
let site = new Runoob("菜鸟教程",  "https://www.runoob.com");
```

‍

#### 对象

‍

##### Object

几乎所有的对象都是 Object 类型的实例，它们都会从 Object.prototype 继承属性和方法. 

Object 构造函数创建一个对象包装器. 

Object 构造函数，会根据给定的参数创建对象：

* 如果给定值是 null 或 undefined，将会创建并返回一个空对象.
* 如果传进去的是一个基本类型的值，则会构造其包装类型的对象.
* 如果传进去的是引用类型的值，仍然会返回这个值，经他们复制的变量保有和源对象相同的引用地址.
* 当以非构造函数形式被调用时，Object 的行为等同于 new Object().

‍

##### 基础

‍

对象由花括号分隔. 内部，对象的属性以名称和值对的形式 (name : value) 来定义. 属性由逗号分隔, 空格和折行无关紧要. 声明可横跨多行; JavaScript 对象是变量的容器. 

```javascript
var person={firstname:"John", lastname:"Doe", id:5566};
```

对象的方法定义了一个函数，并作为对象的属性存储,对象方法通过添加 () 调用 (作为一个函数). 

**constructor** 属性返回所有 JavaScript 变量的构造函数. 

‍

###### 创建

‍

示例

创建对象的一个新实例，并向其添加了四个属性

```javascript
person=new Object();
person.firstname="John";
person.lastname="Doe";
person.age=50;
person.eyecolor="blue";  
```

‍

也可以使用对象字面量来创建对象

```javascript
{ name1 : value1, name2 : value2,...nameN : valueN }
```

‍

###### 访问对象属性

```javascript
person.lastName;
```

```javascript
person["lastName"];
```

‍

‍

###### 对象构造器

可以通过为对象赋值，向已有对象添加新属性

```javascript
function person(firstname,lastname,age,eyecolor){
    this.firstname=firstname;
    this.lastname=lastname;
    this.age=age;
    this.eyecolor=eyecolor;
}  
```

```javascript
var myFather=new person("John","Doe",50,"blue");
var myMother=new person("Sally","Rally",48,"green");
```

```javascript
person.firstname="John";
person.lastname="Doe";
person.age=30;
person.eyecolor="blue";

```

‍

‍

##### 内置对象

‍

js已经提供好的对象

String，Number，Boolean，Array，Function、Math(数学类)，Global(全局对象)，Date(日期对象)，RegExp(正则对象）

‍

‍

###### Math

```javascript
/*1.    Math对象：常用属性和方法;其他方法参考JS的API文档*/
console.log(Math.PI);
console.log(Math.E);
console.log(Math.round(3.14));//四舍五入
console.log(Math.pow(2, 4));//2^4
console.log(Math.random());//[0,1)
```

‍

###### Global

```javascript
/*2.    Global全局对象：主要是提供全局方法，直接使用即可*/
console.log(isNaN(123));
console.log(parseFloat("3.14"));//把字符串转为浮点数
console.log(parseInt("123"));//把字符串转为整数，默认转换为十进制
```

‍

###### Date

```javascript
/*3.    Date日期对象：*/
var d1 = new Date();//日期的创建
var d2 = new Date("2019-01-01");//把字符串转为日期。注意：字符串的类型不能随意定义
//一般为：2019-01-01  或者 2019/01/01
console.log(d1.getDate());//返回当前月份的第几天
console.log(d1.getDay());//返回星期的第几天 0-星期天，1-星期一
console.log(d1.getFullYear());//返回本地时间的年份
```

‍

###### RegExp

正则

​`^`​：正则表达式开始

​`$`​：正则表达式结束

​`\w`​：[0-9a-zA-Z_]

```javascript
/*4.    RegExp：正则表达式：匹配特定格式的字符串*/
//比如：邮箱地址：test@qq.com
var r1 = /^\w+@\w+(\.\w+)+$/;
console.log(r1.test("123AFD@qq.com.cn"));
```

‍

‍

##### 自定义对象

‍

步骤

> * 创建一个构造函数
> * 添加属性和方法
> * 使用new关键字创建对象

‍

###### 无参构造

```javascript
function Person() {
}

//创建对象
var p = new Person();
console.log(p);

//添加属性
p.name = "小明";
p.age = 19;

//添加方法
p.say = function () {
    alert("您好");
}

//调用属性和方法
console.log(p.age);
console.log(p["name"]);
p.say();

```

‍

###### 有参构造

```javascript
//2.    有参构造
function Person(name,age) {
    this.name = name;
    this.age = age;
    this.say = function () {
        alert("有参构造");
    }
}

var p = new Person("张三",18);
console.log(p.name);
console.log(p["age"]);
p.say();
```

‍

###### 直接量

```javascript
{key1:value1,key2:value2,key3:value3,......}
```

```javascript
//3.    直接量定义对象
//语法格式：{key1:value1,key2:value2,key3:value3,......}
//最常用
var p = {
    name:"🐱 ",
    age:28,
    say:function () {
        alert("我是🐱 ");
    }
};
console.log(p);
```

‍

‍

‍

### 技巧

‍

‍

#### 继承

‍

**super()**  方法用于调用父类的构造函数

```javascript
//派生类
class Dog extends Animal {
    // bark() 函数
};
```

‍

‍

‍

‍

## 异常

‍

### 操作

‍

**try** 语句测试代码块的错误. 

**catch** 语句处理错误. 

**throw** 语句创建自定义错误. 

**finally** 语句在 try 和 catch 语句之后, 无论是否有触发异常都会执行

‍

示例

```javascript
<script>
 
function f1(){
  //函数f1是存在的
}
try{
   document.write("试图调用不存在的函数f2()<br>");
    f2();  //调用不存在的函数f2();
}
catch(err){
   document.write("捕捉到错误产生:");
    document.write(err.message);
}
 
document.write("<p>因为错误被捕捉了，所以后续的代码能够继续执行</p>");
 
</script>
```

‍

# 快速

> 存储一些封装好的优秀代码块

‍

‍

‍

‍

# 高级

‍

## 正则

> need?nonono

‍

‍

## JSON

轻量级的数据交换格式<sup>（JavaScript Object Notation）</sup>, 通常用于传递数据

‍

### JSON对象

```javascript
var data = {
    name: "李白",
    age: 10,
    wife: [//数组中每一个元素都是一个对象
        {name:"张飞1",age:10, job: ["打架1", "喝酒1"]},
        {name:"张飞2",age:11, job: ["打架2", "喝酒2"]},
        {name:"张飞3",age:12, job: ["打架3", "喝酒3"]},
    ]
};

//调用json对象，输出其中的某个字段
console.log(data.wife[0]);
console.log(data["wife"][1]["job"][0]);
```

‍

#### 语法

* 数据为 ==键:值 (要双引号)==, 由逗号分隔
* 大括号保存对象, 方括号保存数组
* 布尔直接TF, 字符串加""

‍

#### JSON和JS转化

JSON字符串转JS对象

```javascript
var jsObj=JSON.parse(suerStr);
```

‍

JS对象转JSON字符串

```javascript
var jsonStr=JSON.stringify(jsObj);
```

‍

‍

## BOM

**浏览器对象模型(Browser Object Model)**

允许JS和浏览器对话,JS将浏览器各个部分封装为对象,操作不同对象的方法实现相关功能

‍

‍

### 组成

* Window  浏览器窗口对象
* Navigator  浏览器对象
* Screen  屏幕对象
* History  历史记录对象
* Location  地址栏对象

‍

‍

## DOM

**文档对象模型(Document Object Model)**

被构造为**对象**的树,将标记语言的各部分封装为对应对象,可访问 JavaScript HTML 文档的所有元素, 使JS有能力对 HTML 事件做出反应

‍

‍

### 组成

* Document  整个文档对象
* Element  元素对象
* Attribute  属性对象
* Text  文本对象
* Comment  注释对象

‍

‍

‍

## 数据验证

‍

数据验证用于确保用户输入的数据是有效的

**服务端数据验证**是在数据提交到服务器上后再验证

**客户端数据验证**是在数据发送到服务器前，在浏览器上完成验证

**约束验证**是表单被提交时浏览器用来实现验证的一种算法

‍

### 示例

‍

判断表单字段(fname)值是否存在， 如果不存在，就弹出信息，阻止表单提交

```javascript
function validateForm() {
    var x = document.forms["myForm"]["fname"].value;
    if (x == null || x == "") {
        alert("需要输入名字。");
        return false;
    }
}
```

```html
<script>
function validateForm() {
    var x = document.forms["myForm"]["fname"].value;
    if (x == null || x == "") {
        alert("需要输入名字。");
        return false;
    }
}
</script>
</head>

<body>

    <form name="myForm" action="demo_form.php" 
    onsubmit="return validateForm()" method="post">
        名字: <input type="text" name="fname">
            <input type="submit" value="提交">
    </form>

</body>
</html>
```

‍

‍

==表单自动验证==

点击提交按钮，如果输入框是空的，浏览器会提示错误信息

```html
<body>
    <form action="demo_form.php" method="post">
          <input type="text" name="fname" required="required">
          <input type="submit" value="提交">
    </form>
</body>
```

‍

‍

‍

‍

‍

## 事件

‍

### 实例

> 可以是浏览器行为，也可以是用户行为

* HTML 页面完成加载
* HTML input 字段改变时
* HTML 按钮被点击

‍

常见的HTML事件列表

|事件|描述|
| :-----------: | ----------------------------------|
|onchange|HTML 元素改变|
|onclick|用户点击 HTML 元素|
|onmouseover|鼠标指针移动到指定的元素上时发生|
|onmouseout|从一个 HTML 元素上移开鼠标时|
|onkeydown|用户按下键盘按键|
|onload|浏览器已完成页面的加载|
|onfocus|获得焦点|
|onblur|失去焦点|

‍

‍

### 声明

> 单双引号皆可

```javascript
<some-HTML-element some-event='JavaScript 代码'>
```

‍

‍

‍

### 监听

‍

HTML中标签的事件属性绑定

```javascript
onclick=""
```

‍

DOM元素属性绑定

```javascript
.getElementById().onclick=function(){}
```

‍

‍

## 多线程

‍

### Promise    

ECMAScript 6 提供的类，更加优雅地书写复杂的异步任务

‍

‍

‍

‍

# 数构

‍

‍

‍

‍

## Map

```javascript
= new Map([[:],[:],,,]) 键值对
```

set()    添加

delete()    删除

get()    用key查找value

‍

‍

## Set

```javascript
= new Set([])
```

add()    添加

delete()    删除

has()    判存

‍

‍

‍

# 实操

‍

## Bugfix

‍
