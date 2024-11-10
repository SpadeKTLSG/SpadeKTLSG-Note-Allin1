--微软的JS超级

‍

‍

### Header

此文档作为JS的超集, 主要作为JS的补充

‍

#### 环境

‍

‍

‍

# 知识

‍

‍

## 概念

‍

‍

‍

## 底层

‍

‍

‍

## 命名

‍

‍

## 管理

‍

‍

## 版本

‍

* TypeScript 是 JavaScript 的超集，扩展了 JavaScript 的语法，因此现有的 JavaScript 代码可与 TypeScript 一起工作无需任何修改
* TypeScript 通过类型注解提供编译时的静态类型检查
* TypeScript 可处理已有的 JavaScript 代码，并只对其中的 TypeScript 代码进行编译

‍

增加的功能包括：

* 类型批注和编译时类型检查
* 类型推断
* 类型擦除
* 接口
* 枚举
* Mixin
* 泛型编程
* 名字空间
* 元组
* Await

‍

以下功能是从 ECMA 2015 反向移植而来：

* 类
* 模块
* lambda 函数的箭头语法
* 可选参数以及默认参数

‍

## 特性

‍

‍

‍

# 基础

‍

## 引入

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

‍

### 操作

‍

‍

#### 类型批注

TypeScript 通过类型批注提供静态类型以在编译时启动类型检查

这是可选的，而且可以被忽略而使用 JavaScript 常规的动态类型

```typescript
function Add(left: number, right: number): number {
    return left + right;
}
```

1. 对于基本类型的批注是 number, bool 和string
2. 而弱或动态类型的结构则是 any 类型

‍

类型批注可以被导出到一个单独的声明文件以让使用类型的已被编译为JavaScript的TypeScript脚本的类型信息可用

批注可以为一个现有的 JavaScript 库声明，就像已经为 Node.js 和 jQuery 所做的那样

当类型没有给出时，TypeScript 编译器利用类型推断以推断类型

如果由于缺乏声明，没有类型可以被推断出，那么它就会默认为是动态的 any 类型

‍

‍

‍

### 基本类型

TypeScript 和 JavaScript 没有整数类型

‍

#### Any

‍

变量的值会动态改变时，比如来自用户的输入，任意值类型可以让这些变量跳过编译阶段的类型检查，示例代码如下：

```typescript
let x: any = 1;    // 数字类型
x = 'I am who I am';    // 字符串类型
x = false;    // 布尔类型
```

‍

改写现有代码时，任意值允许在编译时可选择地包含或移除类型检查，示例代码如下：

```typescript
let x: any = 4;
x.ifItExists();    // 正确，ifItExists方法在运行时可能存在，但这里并不会检查
x.toFixed();    // 正确
```

‍

定义存储各种类型数据的数组时，示例代码如下：

```typescript
let arrayList: any[] = [1, false, 'fine'];
arrayList[1] = 100;
```

‍

‍

#### undefined

在 JavaScript 中, undefined 是一个没有设置值的变量。

typeof 一个没有值的变量会返回 undefined。

Null 和 Undefined 是其他任何类型（包括 void）的子类型，可以赋值给其它类型，如数字类型，此时，赋值后的类型会变成 null 或 undefined

‍

在TypeScript中启用严格的空校验（--strictNullChecks）特性，就可以使得null 和 undefined 只能被赋值给 void 或本身对应的类型，示例代码如下：

```
// 启用 --strictNullChecks
let x: number;
x = 1; // 编译正确
x = undefined;    // 编译错误
x = null;    // 编译错误
```

上面的例子中变量 x 只能是数字类型。如果一个类型可能出现 null 或 undefined， 可以用 | 来支持多种类型，示例代码如下：

```
// 启用 --strictNullChecks
let x: number | null | undefined;
x = 1; // 编译正确
x = undefined;    // 编译正确
x = null;    // 编译正确
```

‍

‍

‍

### 引用类型

引用类型的对象存储于堆中，占用的内存较大，不能直接操作它们的值，而是需要通过引用来访问它们的属性和方法，它们的赋值和比较也是按引用进行的，即一个变量的值是一个指向实际对象的引用.

对象(Object)、数组(Array)、函数(Function); 还有两个特殊的对象：正则（RegExp）和日期（Date）

‍

‍

### 字面量

‍

‍

### 变量

‍

```typescript
var [变量名] : [类型] = 值;
```

声明变量的类型，但没有初始值，变量值会设置为 undefined

声明变量并初始值，但不设置类型，该变量可以是任意类型

声明变量没有设置类型和初始值，类型可以是任意类型，默认初始值为 undefined

TypeScript 遵循强类型，如果将不同的类型赋值给变量会编译错误

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

‍

### 类型转换

‍

‍

#### 自动

‍

‍

#### 强制

‍

‍

‍

## 基础语法

‍

‍

### 快速方法

‍

‍

### 语法糖

‍

‍

‍

### 数算

‍

‍

‍

### 拷贝

‍

‍

### Output

‍

‍

### Input

‍

‍

‍

### 流程控制

‍

forEach、every 和 some , JavaScript 的循环语法默认也是支持的

‍

#### for/in

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

#### for...of

创建一个循环来迭代可迭代的对象。在 ES6 中引入的 for...of 循环，以替代 for...in 和 forEach() ，并支持新的迭代协议。for...of 允许你遍历 Arrays（数组）, Strings（字符串）, Maps（映射）, Sets（集合）等可迭代的数据结构等

```typescript
let someArray = [1, "string", false];
 
for (let entry of someArray) {
    console.log(entry); // 1, "string", false
}
```

‍

‍

#### forEach

```typescript
let list = [4, 5, 6];
list.forEach((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
});
```

‍

‍

#### every

```typescript
let list = [4, 5, 6];
list.every((val, idx, array) => {
    // val: 当前值
    // idx：当前index
    // array: Array
    return true; // Continues
    // Return false will quit the iteration
});
```

因为 forEach 在 iteration 中是无法返回的，所以可以使用 every 和 some 来取代 forEach

‍

‍

‍

‍

‍

### Format

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

‍

## OO

‍

### 接口

接口（interface）指定某个类必须实现哪些方法，但不需要定义这些方法的具体内容

可以作为一个类型批注

‍

‍

## 异常

‍

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
