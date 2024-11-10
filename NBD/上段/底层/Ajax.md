--前端通信框架

‍

‍

‍

AJAX不是新的编程语言，而是一种使用现有标准的新方法, 一种用于创建快速动态网页的技术, 基于现有的Internet标准，并且联合使用它们

异步的 JavaScript 和 XML- AXIOS

AJAX 最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容。

AJAX 不需要任何浏览器插件，但需要用户允许JavaScript在浏览器上执行。

‍

### Header

‍

> Ajax-Axios被合并到一起, 直接使用Axios即可
>
> 简单了解, 后续深入拆分

‍

# 知识

‍

## 特性

最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容. 

实现异步交互 --请求可以不导致整个页面的重新加载，并且只针对局部模块的数据进行数据的更新

原生的繁琐, Axios 对原生的Ajax进行了封装, 主要用这个

XMLHttpRequest 只是实现 Ajax 的一种方式

‍

‍

‍

# 基础

‍

## 搭建

‍

‍

### 示例

‍

1.准备数据地址模拟后台传的数据([黑马提供的网址](http://yapi.smart-xwork.cn/mock/169327/emp/list))

2.创建XMLHttpRequest对象, 用于和服务器交换数据

3.向服务器发送请求

4.获取服务器响应数据

‍

##### 创建页面原型

```html
<body>
    <input type="button" value="获取数据" onclick="getData()">

    <div id="div1"></div>
</body>
<script>
    function getData(){
   
    }
</script>
```

‍

#### XMLHttpRequest

在getData()方法中创建XMLHttpRequest对象用于和服务器交换数据  
调用对象的open()方法设置请求的参数信息, 然后调用send()方法向服务器发送请求

```html
var xmlHttpRequest  = new XMLHttpRequest();

      xmlHttpRequest.open(
        "GET",
        "http://yapi.smart-xwork.cn/mock/169327/emp/list"
      ); //设置请求方式和请求地址
      xmlHttpRequest.send(); //发送请求
```

‍

‍

#### 绑定事件

然后通过绑定事件的方式，来获取服务器响应的数据

```html
      xmlHttpRequest.onreadystatechange = function () {
        //监听状态变化
        if (xmlHttpRequest.readyState == 4 && xmlHttpRequest.status == 200) {
          //判断状态: 通过看状态码
          document.getElementById("div1").innerHTML =
            xmlHttpRequest.responseText; //获取响应数据
        }
      };
```

‍

‍

## XMLHttpRequest

原生Ajax请求的核心对象，提供了各种方法

是 AJAX 的基础, 用于在后台与服务器交换数据. 这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新. 所有现代浏览器均内建 XMLHttpRequest 对象

‍

```java
    variable=new XMLHttpRequest();
```

‍

‍

### 请求

‍

使用 XMLHttpRequest 对象的 **open()**  和 **send()**  方法

url 参数是服务器上文件的地址

‍

‍

#### open(method,url,async)

规定请求的类型、URL 以及是否异步处理请求

* method：请求的类型；GET 或 POST
* url：文件在服务器上的位置
* async：true（异步）或 false（同步）

‍

#### send(string)

将请求发送到服务器

* string：仅用于 POST 请求

‍

#### setRequestHeader(header,value)

向请求添加 HTTP 头. 

* header: 规定头的名称
* value: 规定头的值

‍

‍

#### 注意

XMLHttpRequest 对象如果要用于 AJAX 的话，其 open() 方法的 async 参数必须设置为 true

使用 async=true 时，请规定在响应处于 onreadystatechange 事件中的就绪状态时执行的函数

如需使用 async=false，请将 open() 方法中的第三个参数改为 false; 不推荐使用 async=false，但是对于一些小型的请求，也是可以的: JavaScript 会等到服务器响应就绪才继续执行. 如果服务器繁忙或缓慢，应用程序会挂起或停止

‍

实例

```xml
xmlhttp.onreadystatechange=function()
{
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
xmlhttp.open("GET","/try/ajax/ajax_info.txt",true);
xmlhttp.send();
```

‍

‍

‍

‍

‍

### 响应

‍

响应使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性

‍

‍

#### responseText

获得字符串形式的响应数据

‍

示例

```xml
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

‍

#### responseXML

获得 XML 形式的响应数据

‍

示例

```xml
xmlDoc=xmlhttp.responseXML;
txt="";
x=xmlDoc.getElementsByTagName("ARTIST");
for (i=0;i<x.length;i++)
{
    txt=txt + x[i].childNodes[0].nodeValue + "<br>";
}
document.getElementById("myDiv").innerHTML=txt;
```

‍

‍

‍

# 高级

‍

# 实操

‍

## Bugfix

‍
