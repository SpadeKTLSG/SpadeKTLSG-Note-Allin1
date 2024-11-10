--JS库

‍

jQuery 是一个轻量级的JavaScript, 使用 CSS 选择器来访问HTML 元素（DOM 对象）

‍

jquery是js的一个框架。目的是对js的编程方式进行封装，提供了新的编程方法。简化过程。

‍

主要功能包括以下五个部分

1. HTML元素选取
2. HTML元素操作：HTML和CSS、HTML DOM 遍历和修改
3. HTML事件处理：
4. JS特效动画
5. AJAX异步请求

‍

### Header

‍

‍

# 知识

‍

‍

# 基础

‍

## 搭建

‍

### 使用本地jquery

```html
<head>
<script src="https://cdn.staticfile.org/jquery/1.10.2/jquery.min.js">
</script>
</head>
```

‍

### 使用CDN

```html
<head>
<script src="https://lib.sinaapp.com/js/jquery/2.0.2/jquery-2.0.2.min.js">
</script>
</head>
```

‍

‍

## 基础选择器

‍

### 元素选择器

```text
$("p")
```

‍

### #id选择器

```text
$("#test")
```

‍

### .class 选择器

```text
$(".test")
```

‍

### 属性选择器

```text
$("[href]")
```

‍

### 特殊选择器

```text
$("*")	选取所有元素
$(this)	选取当前 HTML 元素
```

‍

### 组合选择器

```text
$("selector1,selector2,...")选取多个选择器指定的元素
```

‍

## 层次选择器

‍

### 后代选择器

* 可能是子代也可能是子代的子代

```text
$("ancestor descendant")
```

‍

‍

### 子代选择器

* 直接后代

```text
$("parent>child")
```

‍

‍

### 相邻选择器

* 同级元素的下一个

```text
$("prev+next")
```

‍

‍

### 同级选择器

* 所有同级元素

```text
$("prev-siblings")
```

‍

‍

## 表单选择器

表单

```text
:input
:text
:password
:radio
:checkbox
:submit
:image
:reset
:button
:file
```

‍

## 事件

> jquery对事件进行了重新封装。采取了与原生JS完全不同的事件处理方法。JS是在HTMLDOM元素中个，添加事件属性，将事件属性与事件响应函数绑定的方法，完成事件响应机制。

> jquery，大多数DOM事件都有一个等效的jQuery方法对应。调用jQuery对象的事件函数，传递高阶函数作为参数，用于回调。实现事件响应与主进程的异步通信。

‍

### 常见事件

‍

#### 鼠标事件

* click
* dblclick
* mouseenter
* mouseleave
* hover

‍

#### 键盘事件

* keypress
* keydown
* keyup

‍

#### 表单事件

* submit
* change
* focus
* blur

‍

#### 文档/窗口事件

* load
* resize
* scroll
* unload

‍

### 处理

‍

#### 直接绑定事件处理方法

回调函数作为参数进行传递。

```text
$("p").click(function(){$(this).hide()});
```

‍

#### $(document).ready()

在加载完成文档后需要执行的函数。

‍

#### 通过bind绑定事件处理方法

可以直接在jQuery对事件进行绑定。而不用在html代码中书写js代码了。

* bind绑定

```text
$(selector).bind(eventtype,eventdata,handler(eventobject))
```

‍

## 效果

‍

### 显示和隐藏

```text
$(selector).hide(speed,callback);
$(selector).show(speed,callback);
$(selector).toggle(speed,callback);

$("#hide").click(function(){
  $("p").hide();
});
 
$("#show").click(function(){
  $("p").show();
});

$("button").click(function(){
  $("p").toggle();
});
```

‍

### 淡入淡出

```text
$(selector).fadeIn(speed,callback);
$(selector).fadeOut(speed,callback);
$(selector).fadeToggle(speed,callback);
$(selector).fadeTo(speed,opacity,callback);

$("button").click(function(){
  $("#div1").fadeIn();
  $("#div2").fadeIn("slow");
  $("#div3").fadeIn(3000);
});

$("button").click(function(){
  $("#div1").fadeOut();
  $("#div2").fadeOut("slow");
  $("#div3").fadeOut(3000);
});

$("button").click(function(){
  $("#div1").fadeToggle();
  $("#div2").fadeToggle("slow");
  $("#div3").fadeToggle(3000);
});
$("button").click(function(){
  $("#div1").fadeTo("slow",0.15);
  $("#div2").fadeTo("slow",0.4);
  $("#div3").fadeTo("slow",0.7);
});
```

‍

### 滑动

```text
$(selector).slideDown(speed,callback);
$(selector).slideUp(speed,callback);
$(selector).slideToggle(speed,callback);

$("#flip").click(function(){
  $("#panel").slideDown();
});
$("#flip").click(function(){
  $("#panel").slideUp();
});

$("#flip").click(function(){
  $("#panel").slideToggle();
});
```

‍

### 效果动画

‍

#### 定义

animate()方法

* 必需的 params 参数定义形成动画的 CSS 属性。
* 可选的 speed 参数规定效果的时长。它可以取以下值："slow"、"fast" 或毫秒。
* 可选的 callback 参数是动画完成后所执行的函数名称。

```text
$(selector).animate({params},speed,callback);
```

‍

#### 使用

```text
$("button").click(function(){
  $("div").animate({
    height:'toggle'
  });
});
```

‍

### 停止动画

```text
$(selector).stop(stopAll,goToEnd);

$("#stop").click(function(){
  $("#panel").stop();
});
```

‍

### 动画链

```text
$("#p1").css("color","red")
.slideUp(2000)
.slideDown(2000);

$("#p1").css("color","red")
  .slideUp(2000)
  .slideDown(2000);
```

‍

‍

## 获取设置内容的方法

* text() - 设置或返回所选元素的文本内容

* html() - 设置或返回所选元素的内容（包括 HTML 标记）

* val() - 设置或返回表单字段的值

```text
$("#btn1").click(function(){
  alert("Text: " + $("#test").text());
});
$("#btn2").click(function(){
  alert("HTML: " + $("#test").html());
});
$("#btn1").click(function(){
  alert("值为: " + $("#test").val());
});

$("#btn1").click(function(){
    $("#test1").text("Hello world!");
});
$("#btn2").click(function(){
    $("#test2").html("<b>Hello world!</b>");
});
$("#btn3").click(function(){
    $("#test3").val("RUNOOB");
});
```

‍

## 获取设置属性的方法

attr() 方法用于获取属性值。

```text
$("button").click(function(){
  alert($("#runoob").attr("href"));
});

$("button").click(function(){
  $("#runoob").attr("href","http://www.runoob.com/jquery");
});
```

‍

jQuery 方法 attr()，也提供回调函数。回调函数有两个参数：被选元素列表中当前元素的下标，以及原始（旧的）值。然后以函数新值返回您希望使用的字符串。

```text
$("button").click(function(){
  $("#runoob").attr("href", function(i,origValue){
    return origValue + "/jquery"; 
  });
});
```

‍

## 添加元素

* append() - 在被选元素的结尾插入内容
* prepend() - 在被选元素的开头插入内容
* after() - 在被选元素之后插入内容
* before() - 在被选元素之前插入内容

```text
$("p").append("追加文本");
$("p").prepend("在开头追加文本");
$("img").after("在后面添加文本");
$("img").before("在前面添加文本");
```

‍

## 删除元素

* remove() - 删除被选元素（及其子元素）
* empty() - 从被选元素中删除子元素

```text
$("#div1").remove();
$("#div1").empty();
```

‍

‍

## CSS设置

* addClass() - 向被选元素添加一个或多个类
* removeClass() - 从被选元素删除一个或多个类
* toggleClass() - 对被选元素进行添加/删除类的切换操作
* css() - 设置或返回样式属性

```text
$("button").click(function(){
  $("h1,h2,p").addClass("blue");
  $("div").addClass("important");
});

$("button").click(function(){
  $("h1,h2,p").removeClass("blue");
});

$("button").click(function(){
  $("h1,h2,p").toggleClass("blue");
});

$("p").css("background-color","yellow");
```

‍

## 遍历

‍

### 定义

与Xpath选择器十分详细。在简介中通过css选择器，能够锁定目标元素。

‍

### 向上遍历

* parent()
* parents()
* parentsUntil()

```text
$(document).ready(function(){
  $("span").parents("ul");
});

$(document).ready(function(){
  $("span").parentsUntil("div");
});
```

‍

### 向下遍历

* children()
* find()

```text
$(document).ready(function(){
  $("div").children();
});

$(document).ready(function(){
  $("div").find("span");
});
```

‍

### 水平遍历

* siblings()
* next()
* nextAll()
* nextUntil()
* prev()
* prevAll()
* prevUntil()

```text
$(document).ready(function(){
  $("h2").siblings();
});

$(document).ready(function(){
  $("h2").next();
});

$(document).ready(function(){
  $("h2").nextUntil("h6");
});
```

‍

### 遍历过滤

* first(), last() 和 eq()，它们允许您基于其在一组元素中的位置来选择一个特定的元素。
* filter() 和 not() 允许您选取匹配或不匹配某项指定标准的元素。

```text
$(document).ready(function(){
  $("div p").first();
});
$(document).ready(function(){
  $("div p").last();
});
$(document).ready(function(){
  $("p").eq(1);
});
$(document).ready(function(){
  $("p").filter(".url");
});
$(document).ready(function(){
  $("p").not(".url");
});
```

‍

### 遍历each

遍历所有的子元素。回调函数的两个参数分别是下标和dom对象

```text
$('selector').each(function(index,element){
  //可以将dom对象转换为jQuery对象
  $(element)
})
```

‍

‍

# 高级

‍

## AJAX

‍

### load

‍

#### load方法

jQuery load() 方法是简单但强大的 AJAX 方法。

load() 方法从服务器加载数据，并把返回的数据放入被选元素中

```text
$(selector).load(URL,data,callback);

$("#div1").load("demo_test.txt");
```

‍

#### load回调参数

可选的 callback 参数规定当 load() 方法完成后所要允许的回调函数。回调函数可以设置不同的参数：

responseTxt - 包含调用成功时的结果内容  
statusTXT - 包含调用的状态  
xhr - 包含 XMLHttpRequest 对象

```text
$("button").click(function(){
  $("#div1").load("demo_test.txt",function(responseTxt,statusTxt,xhr){
    if(statusTxt=="success")
      alert("外部内容加载成功!");
    if(statusTxt=="error")
      alert("Error: "+xhr.status+": "+xhr.statusText);
  });
});
```

‍

‍

### GET方法

GET - 从指定的资源请求数据.GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。

```text
$.get(URL,callback);

$("button").click(function(){
  $.get("demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
  });
});
```

‍

‍

### POST方法

POST - 向指定的资源提交要处理的数据.POST 也可用于从服务器获取数据。不过，POST 方法不会缓存数据，并且常用于连同请求一起发送数据

```text
$.post(URL,data,callback);

$("button").click(function(){
    $.post("/try/ajax/demo_test_post.php",
    {
        name:"菜鸟教程",
        url:"http://www.runoob.com"
    },
    function(data,status){
        alert("数据: \n" + data + "\n状态: " + status);
    });
});
```

‍

‍

### GET JSON方法

GET - 从指定的资源请求数据.GET 基本上用于从服务器获得（取回）数据。注释：GET 方法可能返回缓存数据。

```text
$.getJSON(URL,callback);

$("button").click(function(){
  $.get("demo_test.php",function(data,status){
    alert("数据: " + data + "\n状态: " + status);
  });
});
```

‍

‍

# 实操

‍

‍
