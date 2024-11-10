--前端框架集大成者

‍

‍

‍

Vue是前端框架中的集大成者, 采用渐进式, 综合Angular模块化和React虚拟Dom的优点

基于MVVM思想实现数据的双向绑定, 核心库只关注视图层, 侧重于ViewModel部分开发的vue前端框架，用来替代JavaScript的DOM操作

‍

> 为凑数理解性文档, 下次直接采用覆盖式写入(进行全栈转移时)

‍

‍

### Header

‍

‍

# 知识

‍

#### 版本

‍

> 不保留Vue2内容, 直接学习Vue3, 本文中已于2024.4去除所有关于Vue2内容以免造成混淆

‍

‍

‍

‍

# 基础

‍

‍

## 目录

‍

‍

node_module    整个项目的依赖包

dist      打包后文件

public    静态文件

test	初始测试目录，可删除

src    项目源代码

    assets    静态资源

    components    可重用组件

    router    路由配置

    views    视图组件(页面)

    App.vue    入口页面/根组件

    main.js    入口js文件

package.json    模块基本信息(依赖)

vue.config.js    配置文件

‍

## 构建

‍

### 方法综述

‍

‍

* IDEA直接整合 Vue3 + vite 自动配置
* 命令行脱离IDE环境可以使用Vue + Cli, 通过 vue ui 命令来打开图形化界面创建和管理项目
* 直接命令行内操作

‍

‍

‍

‍

### **独立版本引入**Vue

‍

(手动构建时)在 Vue.js 的官网上直接下载最新版本, 并用  **&lt;script&gt;**  标签引入

‍

CDN引入

```java
<script src="https://cdn.staticfile.org/vue/3.0.5/vue.global.js"></script>
```

‍

1. 引入vue.js文件
2. 在body下面<script>引入vue对象,指定接管目标
3. <body>视图层的展示, 在HTML页面中head部分添加上`<script src="js/vue.js"></script>`​
4. 在body中定义视图, 对象id与要vue接管的对象一致, 使用v-model 调用对应指令

‍

示例

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <script src="js/vue.js"></script>
</head>
<body>
<div id="app">
    <input type="text" v-model="message">
    {{message}}
    <input type="text" v-model="news">
    {{news}}
</div>
</body>
<script>
    //定义Vue对象
    new Vue({
        el: "#app", //vue接管区域
        data: {
            message: "Hello Vue",
            news: "Hello World"
        }
    })
</script>
</html>
```

‍

### Vite直接构建

‍

‍

#### 创建App

‍

Vue3 中的应用是通过使用 createApp 函数来创建的

```vue
const app = Vue.createApp({ /* 选项 */ })
```

传递给 createApp 的选项用于配置根组件. 

‍

#### 挂载

在使用 **mount()**  挂载应用时，该组件被用作渲染的起点. 

```vue
<script>
  const HelloVueApp = {
    data() {
      return {
        message: 'Hello Vue!!'
      }
    }
  }

  Vue.createApp(HelloVueApp).mount('#hello-vue')
</script>
```

‍

一个应用需要被挂载到一个 DOM 元素中，以上代码使用 **mount('#hello-vue')**  将 Vue 应用 HelloVueApp 挂载到  **&lt;div id=&quot;hello-vue&quot;&gt;&lt;/div&gt;**  中. 

‍

目标区域:

```java
<div id="hello-vue" class="demo">
  {{ message }}
</div>
```

 **{{ }}**  用于输出对象属性和函数返回值

‍

‍

#### data选项

**data 选项**是一个函数. Vue 在创建新组件实例的过程中调用此函数. 它应该返回一个对象，然后 Vue 会通过响应性系统将其包裹起来，并以 $data 的形式存储在组件实例中. 

‍

‍

#### **methods**选项

在组件中添加方法，使用 **methods** 选项，该选项包含了所需方法的对象(this), 加在data(){}之后(,)

‍

逆转消息的示例, 利用按钮绑定

```java
<body>

<div id="hello-vue" class="demo">
  {{ message }}
    <button v-on:click="reverseMessage">逆转消息</button>
<!--    {{ message.split('').reverse().join('')}} -->
</div>

<script>
  const HelloVueApp = {
    data() {
      return {
        message: 'Hello Vue!!'
      }
    },
    methods: {
      reverseMessage() {
        this.message = this.message.split('').reverse().join('')
      }
    }
  }

  Vue.createApp(HelloVueApp).mount('#hello-vue')
</script>
</body>
```

‍

‍

‍

## 结构

‍

### 根节点

> 在 Vue3.x 中，组件现在支持有多个根节点

‍

‍

#### 挂载

Vue3通过createApp()函数创建出来的对象，是一个比较纯粹的Vue对象

```js
import { createApp } from 'vue'
import App from './App.vue'
import './index.css'

createApp(App).mount('#app')
```

‍

## 组件

‍

‍

### 生命周期

‍

### 钩子函数

‍

‍

## 指令

‍

|指令|用途|
| :-------------------: | -----------------------------------------------------|
|v-bind|为HTML标签绑定属性值，如设置href , css样式等|
|v-model|创建双向数据绑定|
|v-on|为HTML标签绑定事件|
|v-if|条件性的渲染某元素，判定为true时渲染,否则不渲染|
|v-else-if|-|
|v-else|-|
|v-show|根据条件展示某元素，区别在于切换的是display属性的值|
|v-for|列表渲染，遍历容器的元素或者对象的属性|

‍

‍

### Mustache语法

 mustache是胡须的意思，因为`{{}}`​像胡须，又叫大括号语法。

 在vue对象挂载的dom元素中，`{{}}`​不仅可以直接写变量，还可以写简单表达式。    

‍

#### 示例

```xml
<body>
  <div id="app">
    <h2>{{message}}</h2>
    <h2>{{message}},啧啧啧</h2>

    <!-- Mustache的语法不仅可以直接写变量，还可以写简单表达式 -->
    <h2>{{firstName + lastName}}</h2>
    <h2>{{firstName + " " + lastName}}</h2>
    <h2>{{firstName}}{{lastName}}</h2>
    <h2>{{count * 2}}</h2>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        message:"你好啊",
        firstName:"skt t1",
        lastName:"faker",
        count:100
      }
    })

  </script>
</body>
```

‍

### v-bind

‍

为HTML标签绑定属性值，如设置href , css样式等. 当vue对象中的数据模型发生变化时，标签的属性值会随之发生变化; 绑定的变量必须在数据模型中声明

‍

```html
<a v-bind:href="url">链接1</a>

或

<a :href="url">链接2</a>
```

‍

缩写

```
<a :href="url"></a>
```

‍

#### 样式绑定

-bind

‍

class 与 style 是 HTML 元素的属性，用于设置元素的样式，我们可以用 v-bind 来设置样式属性. 

v-bind 在处理 class 和 style 时， 表达式除了可以使用字符串之外，还可以是对象或数组. 

**v-bind:class** 可以简写为  **:class**

‍

‍

为 **v-bind:class** 设置一个对象，从而动态的切换 **class**:

```java
<div id="app">
    <div :class="{ 'active': isActive }"></div>
</div>
```

此外，我们也可以在这里绑定一个返回对象的计算属性

‍

‍

‍

##### 数组语法

把一个数组传给 **v-bind:class**

```java
<div class="static" :class="[activeClass, errorClass]"></div>
```

‍

可以使用三元表达式来切换列表中的 class

```java
<div id="app">
    <div class="static" :class="[isActive ? activeClass : '', errorClass]"></div>
</div>
```

‍

##### Vue.js style(内联样式)

在 **v-bind:style** 直接设置样式，可以简写为  **:style**

‍

```java
<div id="app">
    <div :style="{ color: activeColor, fontSize: fontSize + 'px' }">菜鸟教程</div>
</div>
```

‍

也可以直接绑定到一个样式**对象**，让模板更清晰

```java
<div id="app">
  <div :style="styleObject">菜鸟教程</div>
</div>
```

‍

可以使用数组将多个样式对象应用到一个元素上

```java
<div id="app">
  <div :style="[baseStyles, overridingStyles]">菜鸟教程</div>
</div>
```

‍

当 **v-bind:style** 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀. 

‍

‍

##### 多重值

可以为 style 绑定中的 property 提供一个包含多个值的数组，常用于提供多个带前缀的值

```java
<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```

‍

‍

##### 组件class属性

‍

当你在带有单个根元素的自定义组件上使用 class 属性时，这些 class 将被添加到该元素中. 此元素上的现有 class 将不会被覆盖, 对于带数据绑定 class 也同样适用

‍

如果你的组件有多个根元素，你需要定义哪些部分将接收这个类. 可以使用  **$attrs** 组件属性执行此操作：

‍

```html
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
<style>
.classA {
    color: red;
	font-size:30px;
}
	</style>
</head>
<body>
<div id="app">
    <runoob class="classA"></runoob>
</div>
 
<script>
const app = Vue.createApp({})
 
app.component('runoob', {
  template: `
    <p :class="$attrs.class">I like runoob!</p>
    <span>这是一个子组件</span>
  `
})
 
app.mount('#app')
</script>
</body>
</html>
```

‍

‍

### v-model

‍

    在表单元素上创建双向数据绑定, 绑定的变量必须在数据模型中声明

‍

```html
<input type="text" v-model="url">
```

‍

#### 数据绑定

-model

‍

##### .lazy

在默认情况下， v-model 在 input 事件中同步输入框的值与数据，但你可以添加一个修饰符 lazy ，从而转变为在 change 事件中同步

```
<!-- 在 "change" 而不是 "input" 事件中更新 -->
<input v-model.lazy="msg" >
```

‍

##### .number

如果想自动将用户的输入值转为 Number 类型（如果原值的转换结果为 NaN 则返回原值），可以添加一个修饰符 number 给 v-model 来处理输入值

```
<input v-model.number="age" type="number">
```

这通常很有用，因为在 type="number" 时 HTML 中输入的值也总是会返回字符串类型. 

‍

‍

##### .trim

如果要自动过滤用户输入的首尾空格，可以添加 trim 修饰符到 v-model 上过滤输入：

```
<input v-model.trim="msg">
```

‍

‍

‍

### v-on

‍

给html标签绑定事件

v-on语法给标签的事件绑定的函数，必须是vue对象声明的函数; 绑定事件时，事件名相比较js中的事件名，没有on

```html
<input type="button" value="点我一下" v-on:click="handle()">

简写方式，即v-on: 可以替换成@

<input type="button" value="点我一下" @click="handle()">
```

‍

缩写

```
<!-- 完整语法 -->
<a v-on:click="doSomething"></a>
<!-- 缩写 -->
<a @click="doSomething"></a>
```

‍

```java
 <template v-if="seen">


data() {
    return {
      seen: true /* 改为false，信息就无法显示 */
    }
  }
```

‍

#### 事件处理

v-on

‍

使用 v-on 指令来监听 DOM 事件，从而执行 JavaScript 代码. 

**v-on** 指令可以缩写为  **@**  符号

v-on 可以接收一个定义的方法来调用. 

除了直接绑定到一个方法，也可以用内联 JavaScript 语句：

```java
<button @click="say('hi')">Say hi</button>
  <button @click="say('what')">Say what</button>

methods: {
    say(message) {
      alert(message)
    }
  }
```

‍

事件处理程序中可以有多个方法，这些方法由逗号运算符分隔

```java
<!-- 这两个 one() 和 two() 将执行按钮点击事件 -->
  <button @click="one($event), two($event)">
  点我
  </button>


 methods: {
    one(event) {
      alert("第一个事件处理器逻辑...")
    },
    two(event) {
      alert("第二个事件处理器逻辑...")
    }
  }
```

‍

‍

##### 事件修饰符

‍

Vue.js 为 v-on 提供了事件修饰符来处理 DOM 事件细节，如：event.preventDefault() 或 event.stopPropagation(). 

Vue.js 通过由点  **.**  表示的指令后缀来调用修饰符. 

* ​`.stop`​ - 阻止冒泡
* ​`.prevent`​ - 阻止默认事件
* ​`.capture`​ - 阻止捕获
* ​`.self`​ - 只监听触发该元素的事件
* ​`.once`​ - 只触发一次
* ​`.left`​ - 左键事件
* ​`.right`​ - 右键事件
* ​`.middle`​ - 中间滚轮事件

```java
<!-- 阻止单击事件冒泡 -->
<a v-on:click.stop="doThis"></a>
<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>
<!-- 修饰符可以串联  -->
<a v-on:click.stop.prevent="doThat"></a>
<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>
<!-- 添加事件侦听器时使用事件捕获模式 -->
<div v-on:click.capture="doThis">...</div>
<!-- 只当事件在该元素本身（而不是子元素）触发时触发回调 -->
<div v-on:click.self="doThat">...</div>

<!-- click 事件只能点击一次，2.1.4版本新增 -->
<a v-on:click.once="doThis"></a>
```

‍

##### 按键修饰符

‍

v-on 在监听键盘事件时添加按键修饰符：

```
<!-- 只有在 keyCode 是 13 时调用 vm.submit() -->
<input v-on:keyup.13="submit">
```

‍

记住所有的 keyCode 比较困难，所以 Vue 为最常用的按键提供了别名：

```
<!-- 同上 -->
<input v-on:keyup.enter="submit">
<!-- 缩写语法 -->
<input @keyup.enter="submit">
```

全部的按键别名：

* ​`.enter`​
* ​`.tab`​
* ​`.delete`​ (捕获 "删除" 和 "退格" 键)
* ​`.esc`​
* ​`.space`​
* ​`.up`​
* ​`.down`​
* ​`.left`​
* ​`.right`​

系统修饰键：

* ​`.ctrl`​
* ​`.alt`​
* ​`.shift`​
* ​`.meta`​

鼠标按钮修饰符:

* ​`.left`​
* ​`.right`​
* ​`.middle`​

‍

‍

示例

```java
<p><!-- Alt + C -->
<input @keyup.alt.67="clear">
<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

‍

‍

##### .exact 修饰符

 **.exact** 修饰符允许你控制由精确的系统修饰符组合触发的事件

‍

```java
<!-- 即使 Alt 或 Shift 被一同按下时也会触发 -->
<button @click.ctrl="onClick">A</button>

<!-- 有且只有 Ctrl 被按下的时候才触发 -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 没有任何系统修饰符被按下的时候才触发 -->
<button @click.exact="onClick">A</button>
```

‍

‍

### v-if/v-show

‍

    v-show和v-if的作用效果一样，只是原理不同; v-if指令中不满足条件的标签代码直接没了，而v-show指令中，不满足条件的代码依然存在，只是添加了css样式来控制标签不去显示. 

‍

‍

### v-once

‍

 v-once表示该dom元素只渲染一次，之后数据改变，不会再次渲染。

```html
  <div id="app">
    <h2>{{message}}</h2>
    <!-- 只会渲染一次，数据改变不会再次渲染 -->
    <h2 v-once>{{message}}</h2>

  </div>
```

上述`{{message}}`​的message修改后，第一个h2标签数据会自动改变，第二个h2不会

‍

‍

### v-for

‍

v-for 指令就写在需要循环的那个标签上

```html
<div v-for="addr in addrs">{{addr}}</div>
<div v-for="(addr,index) in addrs">{{index + 1}} : {{addr}}</div>
```

```html
<标签 v-for="变量名 in 集合模型数据">
    {{变量名}}
</标签>
```

‍

遍历时需要使用索引时

```html
<标签 v-for="(变量名,索引变量) in 集合模型数据">
    <!--索引变量是从0开始，所以要表示序号的话，需要手动的加1-->
   {{索引变量 + 1}} {{变量名}}
</标签>
```

‍

### v-html

在某些时候我们不希望直接输出`<a href='http://www.baidu.com'>百度一下</a>`​这样的字符串，而输出被html自己转化的超链接。此时可以使用v-html。

‍

#### 示例

```xml
<body>
  <div id="app">
    <h2>不使用v-html</h2>
    <h2>{{url}}</h2>
    <h2>使用v-html，直接插入html</h2>
    <h2 v-html="url"></h2>

  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        message:"你好啊",
        url:"<a href='http://www.baidu.com'>百度一下</a>"
      }
    })
  </script>
</body>
```

‍

### v-text

v-text会覆盖dom元素中的数据，相当于js的innerHTML方法

```xml
<body>
  <div id="app">
    <h2>不使用v-text</h2>
    <h2>{{message}}，啧啧啧</h2>
    <h2>使用v-text，以文本形式显示,会覆盖</h2>
    <h2 v-text="message">，啧啧啧</h2>

  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        message:"你好啊"
      }
    })
  </script>
</body>
```

使用`{{message}}`​是拼接变量和字符串，而是用v-text是直接覆盖字符串内容。

‍

### v-pre

‍

时候我们期望直接输出`{{message}}`​这样的字符串，而不是被`{{}}`​语法转化的message的变量值，此时我们可以使用`v-pre`​标签。

‍

```xml
<body>
  <div id="app">
    <h2>不使用v-pre</h2>
    <h2>{{message}}</h2>
    <h2>使用v-pre,不会解析</h2>
    <h2 v-pre>{{message}}</h2>

  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        message:"你好啊"
      }
    })
  </script>
</body>
```

‍

### v-cloak

有时候因为加载延时问题，例如卡掉了，数据没有及时刷新，就造成了页面显示从`{{message}}`​到message变量“你好啊”的变化，这样闪动的变化，会造成用户体验不好。此时需要使用到`v-cloak`​的这个标签。在vue解析之前，div属性中有`v-cloak`​这个标签，在vue解析完成之后，v-cloak标签被移除。简单，类似div开始有一个css属性`display:none;`​，加载完成之后，css属性变成`display:block`​，元素显示出来。

‍

```xml
  <title>v-cloak指令的使用</title>
  <style>
    [v-cloak]{
      display: none;
    }
  </style>
</head>

<body>
  <div id="app" v-cloak>
    <h2>{{message}}</h2>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    //在vue解析前，div中有一个属性cloak
    //在vue解析之后，div中没有一个属性v-cloak
    setTimeout(() => {
      const app = new Vue({
        el: "#app",
        data: {
          message: "你好啊"
        }
      })
    }, 1000);
  </script>
</body>
```

这里通过延时1秒模拟加载卡住的状态，结果一开始不显示message的值，div元素中有v-cloak的属性，1秒后显示message变量的值，div中的v-cloak元素被移除。

‍

### 自定义指令

‍

‍

‍

## 展示

> 大部分为V2版本内容, 将就着看

‍

‍

‍

### 文本

数据绑定最常见的形式就是使用  **{{...}}** （双大括号）的文本插值

‍

```java
<div id="app">
  <p>{{ message }}</p>
</div>
```

 **{{...}}**  标签的内容将会被替代为对应组件实例中 **message** 属性的值，如果 **message** 属性的值发生了改变， **{{...}}**  标签内容也会更新. 

如果不想改变标签的内容，可以通过使用 **v-once** 指令执行一次性地插值，当数据改变时，插值处的内容不会更新

```java
<span v-once>这个将不会改变: {{ message }}</span>
```

‍

‍

### Html

使用 v-html 指令用于输出 html 代码

‍

div

```java
<p>使用 v-html 指令: <span v-html="rawHtml"></span></p>
```

```java
data() {
    return {
      rawHtml: '<span style="color: red">这里会显示红色！</span>'
    }
  }
```

‍

‍

### 属性

HTML 属性中的值应使用 v-bind 指令. 

```
<div v-bind:id="dynamicId"></div>
```

对于布尔属性，常规值为 true 或 false，如果属性值为 null 或 undefined，则该属性不会显示出来. 

```java
<button v-bind: disabled="isButtonDisabled">按钮</button>
```

以上代码中如果 isButtonDisabled 的值是 null 或 undefined，则 disabled 属性甚至不会被包含在渲染出来的 <button> 元素中. 

‍

‍

‍

### 表达式

Vue.js 都提供了完全的 JavaScript 表达式支持. 

‍

```java
<div id="app">
    {{5+5}}<br>
    {{ ok ? 'YES' : 'NO' }}<br>
    {{ message.split('').reverse().join('') }}
    <div v-bind:id="'list-' + id">菜鸟教程</div>
</div>
  
<script>
const app = {
  data() {
    return {
      ok: true,
      message: 'RUNOOB!!',
      id: 1
    }
  }
}
 
Vue.createApp(app).mount('#app')
</script>
```

‍

表达式会在当前活动实例的数据作用域下作为 JavaScript 被解析. 有个限制就是，每个绑定都只能包含单个表达式，所以下面的例子都不会生效:

```
<!--  这是语句，不是表达式：-->
{{ var a = 1 }}

<!-- 流控制也不会生效，请使用三元表达式 -->
{{ if (ok) { return message } }}
```

‍

### 修饰符

修饰符是以半角句号  **.**  指明的特殊后缀，用于指出一个指令应该以特殊方式绑定. 例如， **.prevent** 修饰符告诉 **v-on** 指令对于触发的事件调用 **event.preventDefault()** ：

```
<form v-on:submit.prevent="onSubmit"></form>
```

‍

‍

### 表单

‍

v-model 指令在表单 `<input>`​、`<textarea>`​ 及 `<select>`​ 等元素上创建双向数据绑定

v-model 会根据控件类型自动选取正确的方法来更新元素. 

v-model 会忽略所有表单元素的 value、checked、selected 属性的初始值，使用的是 data 选项中声明初始值. 

v-model 在内部为不同的输入元素使用不同的属性并抛出不同的事件：

‍

* text 和 textarea 元素使用 `value`​ 属性和 `input`​ 事件；
* checkbox 和 radio 使用 `checked`​ 属性和 `change`​ 事件；
* select 字段将 `value`​ 作为属性并将 `change`​ 作为事件.

‍

在文本区域 textarea 插值是不起作用，需要使用 v-model 来代替

```
<!-- 错误 -->
<textarea>{{ text }}</textarea>

<!-- 正确 -->
<textarea v-model="text"></textarea>
```

‍

#### 复选框

‍

复选框如果是一个为逻辑值，如果是多个则绑定到同一个数组

复选框的双向数据绑定

```java
<div id="app">
  <p>单个复选框：</p>
  <input type="checkbox" id="checkbox" v-model="checked">
  <label for="checkbox">{{ checked }}</label>
  
  <p>多个复选框：</p>
  <input type="checkbox" id="runoob" value="Runoob" v-model="checkedNames">
  <label for="runoob">Runoob</label>
  <input type="checkbox" id="google" value="Google" v-model="checkedNames">
  <label for="google">Google</label>
  <input type="checkbox" id="taobao" value="Taobao" v-model="checkedNames">
  <label for="taobao">taobao</label>
  <br>
  <span>选择的值为: {{ checkedNames }}</span>
</div>
 
<script>
const app = {
  data() {
    return {
      checked : false,
      checkedNames: []
    }
  }
}
 
Vue.createApp(app).mount('#app')
</script>
```

‍

‍

#### 单选按钮

‍

单选按钮的双向数据绑定

```java
<div id="app">
  <input type="radio" id="runoob" value="Runoob" v-model="picked">
  <label for="runoob">Runoob</label>

  <br>

  <input type="radio" id="google" value="Google" v-model="picked">
  <label for="google">Google</label>

  <br>

  <span>选中值为: {{ picked }}</span>

</div>
 
<script>
const app = {
  data() {
    return {
      picked : 'Runoob'
    }
  }
}
 
Vue.createApp(app).mount('#app')
</script>
```

‍

‍

#### select 列表

拉列表的双向数据绑定

```java
<div id="app">
  <select v-model="selected" name="site">
    <option value="">选择一个网站</option>
    <option value="www.runoob.com">Runoob</option>
    <option value="www.google.com">Google</option>
  </select>
 
  <div id="output">
      选择的网站是: {{selected}}
  </div>
</div>
 
<script>
const app = {
  data() {
    return {
      selected: '' 
    }
  }
}
 
Vue.createApp(app).mount('#app')
</script>
```

‍

多选时会绑定到一个数组： 加上`multiple`​

```java
<div id="app">
  <select v-model="selected" name="fruit" multiple>
    <option value="www.runoob.com">Runoob</option>
    <option value="www.google.com">Google</option>
    <option value="www.taobao.com">Taobao</option>
  </select>
 
  <div id="output">
      选择的网站是: {{selected}}
  </div>
</div>
 
<script>
const app = {
  data() {
    return {
      selected: '' 
    }
  }
}
 
Vue.createApp(app).mount('#app')
</script>
```

‍

#### 值绑定

对于单选按钮，复选框及选择框的选项，v-model 绑定的值通常是静态字符串 (对于复选框也可以是布尔值)：

```java
<!-- 当选中时，`picked` 为字符串 "a" -->
<input type="radio" v-model="picked" value="a" />

<!-- `toggle` 为 true 或 false -->
<input type="checkbox" v-model="toggle" />

<!-- 当选中第一个选项时，`selected` 为字符串 "abc" -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

但是有时我们可能想把值绑定到当前活动实例的一个动态属性上，这时可以用 v-bind 实现，此外，使用 v-bind 可以将输入值绑定到非字符串

‍

**复选框 (Checkbox)：**

```
<input type="checkbox" v-model="toggle" true-value="yes" false-value="no" />
...
// 选中时
vm.toggle === 'yes'
// 取消选中 
vm.toggle === 'no'
```

‍

**单选框 (Radio)：**

```
<input type="radio" v-model="pick" v-bind:value="a" />
// 当选中时
vm.pick === vm.a
```

‍

**选择框选项 (Select)：**

```
<select v-model="selected">
  <!-- 内联对象字面量 -->
  <option :value="{ number: 123 }">123</option>
</select>
// 当被选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

‍

‍

## 计算属性

‍

### 示例

 现在有变量姓氏和名字，要得到完整的名字。

```html

</head>
<body>
  <div id="app">
    <!-- Mastache语法 -->
    <h2>{{firstName+ " " + lastName}}</h2>
    <!-- 方法 -->
    <h2>{{getFullName()}}</h2>
    <!-- 计算属性 -->
    <h2>{{fullName}}</h2>
  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        firstName:"skt t1",
        lastName:"faker"
      },
      computed: {
        fullName:function(){
          return this.firstName + " " + this.lastName
        }
      },
      methods: {
        getFullName(){
          return this.firstName + " " + this.lastName
        }
      },
    })
  </script>
</body>
</html>
```

1. 使用Mastache语法拼接`<h2>{{firstName+ " " + lastName}}</h2>`​
2. 使用方法methods`<h2>{{getFullName()}}</h2>`​
3. 使用计算属性computed`<h2>{{fullName}}</h2>`​

> 例子中计算属性computed看起来和方法似乎一样，只是方法调用需要使用()，而计算属性不用，方法取名字一般是动词见名知义，而计算属性是属性是名词，但这只是基本使用。

‍

‍

**复杂使用**

现在有一个数组数据books，里面包含许多book对象，数据结构如下：

```javascript
books:[
          {id:110,name:"JavaScript从入门到入土",price:119}, 
          {id:111,name:"Java从入门到放弃",price:80},
          {id:112,name:"编码艺术",price:99},
          {id:113,name:"代码大全",price:150},
        ]
```

要求计算出所有book的总价格`totalPrice`​

```xml
<body>
  <div id="app">


    <h2>总价格：{{totalPrice}}</h2>

  </div>
  <script src="https://cdn.jsdelivr.net/npm/vue@2.6.10/dist/vue.js"></script>
  <script>
    const app = new Vue({
      el:"#app",
      data:{
        books:[
          {id:110,name:"JavaScript从入门到入土",price:119},
          {id:111,name:"Java从入门到放弃",price:80},
          {id:112,name:"编码艺术",price:99},
          {id:113,name:"代码大全",price:150},
        ]
      },
      computed: {
        totalPrice(){
          let result= 0;
          for (let i = 0; i < this.books.length; i++) {
            result += this.books[i].price;
          }
          return result
        }
      }
    })
  </script>
</body>
```

获取每一个book对象的price累加，当其中一个book的价格发生改变时候，总价会随之变化

‍

‍

# 高级

‍

## 特殊属性

‍

### 计算属性

computed

计算属性在处理一些复杂逻辑时是很有用的

```java
data():{
}, 
computed: {
    // 计算属性的 getter
    reversedMessage: function () {
      // `this` 指向 vm 实例
      return this.message.split('').reverse().join('')
    }
  }
```

声明了一个计算属性 reversedMessage

提供的函数将用作属性 vm.reversedMessage 的 getter

vm.reversedMessage 依赖于 vm.message，在 vm.message 发生改变时，vm.reversedMessage 也会更新

‍

#### 对比methods

我们可以使用 methods 来替代 computed (声明时候的关键字)，效果上两个都是一样的，但是 computed 是基于它的依赖缓存，只有相关依赖发生改变时才会重新取值. 而使用 methods ，在重新渲染的时候，函数总会重新调用执行. 

可以说使用 computed 性能会更好，但是如果你不希望缓存，你可以使用 methods 属性. 

‍

#### setter

computed 属性默认只有 getter ，不过在需要时你也可以提供一个 setter ：

示例

```java
computed: {
    site: {
      // getter
      get: function () {
        return this.name + ' ' + this.url
      },
      // setter
      set: function (newValue) {
        var names = newValue.split(' ')
        this.name = names[0]
        this.url = names[names.length - 1]
      }
    }
  }
```

‍

‍

### 监听属性

watch

响应数据的变化

```java
<script>
const app = {
  data() {
    return {
      counter: 1
    }
  }
}
vm = Vue.createApp(app).mount('#app')
vm.$watch('counter', function(nval, oval) {
    alert('计数器值的变化 :' + oval + ' 变为 ' + nval + '!');
});
</script>
```

示例

进行千米与米之间的换算

```java
<div id = "app">
    千米 : <input type = "text" v-model = "kilometers"  @focus="currentlyActiveField = 'kilometers'">
    米 : <input type = "text" v-model = "meters" @focus="currentlyActiveField = 'meters'">
</div>
<p id="info"></p>  
<script>
const app = {
  data() {
    return {
      kilometers : 0,
      meters:0
    }
  },
  watch : {
    kilometers:function(newValue, oldValue) {
      // 判断是否是当前输入框
      if (this.currentlyActiveField === 'kilometers') {
        this.kilometers = newValue;
        this.meters = newValue * 1000
      }
    },
    meters : function (newValue, oldValue) {
      // 判断是否是当前输入框
      if (this.currentlyActiveField === 'meters') {
        this.kilometers = newValue/ 1000;
        this.meters = newValue;
      }
    }
  }
}
vm = Vue.createApp(app).mount('#app')
vm.$watch('kilometers', function (newValue, oldValue) {
  // 这个回调将在 vm.kilometers 改变后调用
  document.getElementById ("info").innerHTML = "修改前值为: " + oldValue + "，修改后值为: " + newValue;
})
</script>
```

‍

#### 异步加载优化

Vue 通过 watch 选项提供了一个更通用的方法，来响应异步加载数据的变化

使用 axios 库

```java
<!-- 因为 AJAX 库和通用工具的生态已经相当丰富，Vue 核心代码没有重复 -->
<!-- 提供这些功能以保持精简。这也可以让你自由选择自己更熟悉的工具。 -->
<script src="https://cdn.staticfile.org/axios/0.27.2/axios.min.js"></script>
<script src="https://cdn.staticfile.org/vue/3.2.37/vue.global.min.js"></script>
<script>
  const watchExampleVM = Vue.createApp({
    data() {
      return {
        question: '',
        answer: '每个问题结尾需要输入 ? 号。'
      }
    },
    watch: {
      // 每当问题改变时，此功能将运行，以 ? 号结尾，兼容中英文 ?
      question(newQuestion, oldQuestion) {
        if (newQuestion.indexOf('?') > -1 || newQuestion.indexOf('？') > -1) {
          this.getAnswer()
        }
      }
    },
    methods: {
      getAnswer() {
        this.answer = '加载中...'
        axios
          .get('/try/ajax/json_vuetest.php')
          .then(response => {
            this.answer = response.data.answer
          })
          .catch(error => {
            this.answer = '错误! 无法访问 API。 ' + error
          })
      }
    }
  }).mount('#watch-example')
</script>
```

‍

‍

‍

## 模板语法

Vue 使用了基于 HTML 的模板语法，允许开发者声明式地将 DOM 绑定至底层 Vue 实例的数据. 

Vue 的核心是一个允许你采用简洁的模板语法来声明式的将数据渲染进 DOM 的系统. 

结合响应系统，在应用状态改变时， Vue 能够智能地计算出重新渲染组件的最小代价并应用到 DOM 操作上

‍

‍

## 组合式 API

Composition API 主要用于在大型组件中提高代码逻辑的可复用性. 

传统的组件随着业务复杂度越来越高，代码量会不断的加大，整个代码逻辑都不易阅读和理解. 

Vue3 使用组合式 API 的地方为 **setup**. 

在 setup 中，我们可以按逻辑关注点对部分代码进行分组，然后提取逻辑片段并与其他组件共享代码. 因此，组合式 API（Composition API） 允许我们编写更有条理的代码. 

‍

‍

## 单文件组件

‍

就是IDE里面那一套,使用  **.vue** 文件来创建单文件组件(Single File Components, SFCs)，这个文件包含了组件的模板、JavaScript 代码以及 CSS 样式. 

‍

删除 <template>、<script> 和 <style> 标签内的所有内容，以及任何像 setup 或 scoped 这样的属性

App.vue 文件现在应该如下所示

```java
<script></script>

<template></template>

<style></style>
```

main.js 文件代码修改为如下代码：

‍

```java
import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

‍

现在我们就创建了一个简单的项目

‍

组件的 JavaScript 部分，我们使用了新的 export default 语法，这个语法可以让我们将组件定义导出为一个默认的对象. 在 Vue3 中，我们可以使用这个语法来定义组件，而不必像 Vue2 那样使用 Vue.component 函数. 

‍

当我们定义好了一个组件之后，我们可以在其他组件中使用这个组件. 

使用组建，我们需要先创建组建，比如以下实例在  **./src/components/**  目录下创建 **HelloRunoob.vue** 组件文件，代码如下：

‍

```java
<template>
  <div>
    <h1>{{ message }}</h1>
  </div>
</template>

<script>
export default {
  data() {
    return {
      message: 'Hello, Runoob!'
    }
  }
}
</script>

<style>
h1 {
  color: red;
}
</style>
```

‍

然后我们在 ./src/main.js 文件中引入并定义该组件：

```java
import { createApp } from 'vue'

import App from './App.vue'
import HelloRunoob from './components/HelloRunoob.vue'

const app = createApp(App)
app.component('hello-runoob', HelloRunoob) // 自定义标签
app.mount('#app')
```

在父组件的模板中，我们可以使用自定义标签的方式来引入子组件，就像以下 App.vue 文件代码：

```java
<template>
  <div>
    <hello-runoob></hello-runoob>
  </div>
</template>
```

‍

## Ajax(axios)

Vue版本推荐使用 axios 来完成 ajax 请求. 

‍

**使用 cdn:**

```
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

或

```
<script src="https://cdn.staticfile.org/axios/0.18.0/axios.min.js"></script>
```

**使用 npm:**

```
$ npm install axios
```

**使用 bower:**

```
$ bower install axios
```

**使用 yarn:**

```
$ yarn add axios
```

‍

使用方法：

```
Vue.axios.get(api).then((response) => {
  console.log(response.data)
})

this.axios.get(api).then((response) => {
  console.log(response.data)
})

this.$http.get(api).then((response) => {
  console.log(response.data)
})
```

‍

## 混入

mixins

定义了一部分可复用的方法或者计算属性. 混入对象可以包含任意组件选项. 当组件使用混入对象时，所有混入对象的选项将被混入该组件本身的选项. 

```java
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
</head>
<body>
<div id = "app"></div>
<script type = "text/javascript">
// 定义混入对象
const myMixin = {
  created() {
    this.hello()
  },
  methods: {
    hello() {
      document.write('欢迎来到混入实例-RUNOOB!')
    }
  }
}
 
// 定义一个应用，使用混入
const app = Vue.createApp({
  mixins: [myMixin]
})
 
app.mount('#app') // => "欢迎来到混入实例-RUNOOB!"
</script>
</body>
</html>
```

‍

### 选项合并

‍

当组件和混入对象含有同名选项时，这些选项将以恰当的方式混合. 

比如，数据对象在内部会进行浅合并 (一层属性深度)，在和组件的数据发生冲突时以组件数据优先. 

同名钩子函数将合并为一个数组，因此都将被调用. 另外，mixin 对象的钩子将在组件自身钩子之前调用

值为对象的选项，例如 methods、components 和 directives，将被合并为同一个对象. 两个对象键名冲突时，取组件对象的键值对. 

从输出结果 methods 选项中如果碰到相同的函数名则 Vue 实例有更高的优先级会执行输出

‍

### 全局混入

‍

也可以全局注册混入对象. 注意使用！ 一旦使用全局混入对象，将会影响到 所有 之后创建的 Vue 实例. 使用恰当时，可以为自定义对象注入处理逻辑. 

‍

```java
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Vue 测试实例 - 菜鸟教程(runoob.com)</title>
<script src="https://unpkg.com/vue@next"></script>
</head>
<body>
<div id = "app"></div>
<script type = "text/javascript">
const app = Vue.createApp({
  myOption: 'hello!'
})
 
// 为自定义的选项 'myOption' 注入一个处理器。
app.mixin({
  created() {
    const myOption = this.$options.myOption
    if (myOption) {
      document.write(myOption)
    }
  }
})
 
app.mount('#app') // => "hello!"
</script>
</body>
</html>
```

‍

‍

‍

‍

‍

‍

‍

# 实操

‍

## Bugfix

‍
