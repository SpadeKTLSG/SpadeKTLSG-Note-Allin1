--懒狗福音

‍

‍

一个静态表格，基本上在不修改的现有代码的情况下，只要增加 `class="layui-table"`​，就能立刻展现出优美的界面. [网址](https://layui.dev/docs/2/)

更具体的说，当你使用 ASP.NET Gridview 控件时，基本上只要增加 `class="layui-table"`​ 就能达到 UI 上专业的美观效果. 

这和 Ant Design 这种 UI 设计理念完成不同，在 Ant Design 里，你需要 import/export 各种 JS 包. 

‍

### Header

‍

# 知识

‍

‍

# 基础

‍

## 组成

‍

layui框架的主要内容是[页面元素],[内置模块]

‍

‍

### 页面元素

见官网

‍

‍

### 内置模块

> layui 定义了一套更轻量的模块规范, 将一些特殊功能,比如一些动态效果, 封装成了一个一个的函数, 一个功能就是一个模块, 每个模块有对应的名字

需要加载对应功能模块

‍

‍

## 使用

‍

use 方法加载模块, 再获得对应的模块对象

然后在use方法的回调中完成业务

‍

示例

```html
<script>
    // 1 加载模块,可以一次加载多个模块,此时使用数组
    layui.use(['form','layer','table'],function(){
       // 2获得对应的模块对象
       var form = layui.form;
       var layer = layui.layer;
       var table = layui.table;
  
       // 3 完成一些业务
       form.on('submit(formDemo)',function(){
           return false;
       })
   
       layer.msg();
       layer.confirm();
       layer.alert();
  
       table.render()
    });
</script>
```

‍

‍

# 高级

‍

‍

# 实操

‍

## Bugfix
