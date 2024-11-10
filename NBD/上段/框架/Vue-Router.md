‍

‍

使用官方路由器vue-router

vue**属于单页面应用**，所谓的路由，就是根据浏览器路径不同，用**不同的视图组件替换这个页面内容**

Vue 路由允许通过不同的 URL 访问不同的内容

通过 Vue 可以实现多视图的单页 Web 应用（single page web application，SPA）

> 高阶篇日后

‍

# 知识

‍

## 评价

* 路由就是通过互联的网络把信息从源地址传送到目的地的活动
* 路由提供了两种机制：路由和传送

  * 路由是决定数据包从来源到目的地的路径
  * 转送就是将数据转移
* 路由表

  * 路由表本质就是一个映射表，决定了数据包的指向

‍

# 基础

‍

## 前端/后端路由

‍

1. 后端渲染（服务端渲染）  
    jsp技术  
    后端路由，后端处理URL和页面映射关系，例如springmvc中的@requestMapping注解配置的URL地址，映射前端页面
2. 前后端分离（ajax请求数据）  
    后端只负责提供数据  
    静态资源服务器（html+css+js）  
    ajax发送网络请求后端服务器，服务器回传数据  
    js代码渲染dom
3. 单页面富应用（SPA页面）  
    前后端分离加上前端路由，前端路由的url映射表不会向服务器请求，是单独url的的页面自己的ajax请求后端，后端只提供api负责响应数据请求。改变url，页面不进行整体的刷新。  
    整个网站只有一个html页面。

‍

‍

## 搭建

‍

使用`npm install vue-router --save`​​来安装vue-router插件模块

在模块化工程中使用他(因为是一个插件，所以可以通过Vue.user来安装路由功能)

* 在src下创建一个router文件夹（一般安装vue-router时候会自动创建）用来存放vue-router的路由信息导入路由对象，并且调用**Vue.use(VueRouter)**
* 创建路由实例，并且传入路由**映射配置**
* 在vue实例中挂载创建的**路由实例对象**

> router文件夹中的index.js

‍

‍

```javascript
/**
 * 配置路由相关信息
 * 1.先导入vue实例和vue-router实例
 */
import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

// 2. 通过Vue.use(插件)，安装插件
Vue.use(Router)
//3. 创建 router路由对象
const routes = [
  //配置路由和组件之间的对应关系
  {
    path: '/',//url
    name: 'HelloWorld',
    component: HelloWorld //组件名
  }
]
const router = new Router({
  //配置路由和组件之间的应用关系
  routes
})
//4.导出router实例
export default router
```

> main.js中挂载router对象

```javascript
/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,//使用路由对象，简写对象增强写法
  render: h => h(App)
})
```

‍

## 使用

### 创建路由组件

在components文件夹下创建2个组件。

> Home组件

```vue
<template>
  <div class="page-contianer">
    <h2>这是首页</h2>
    <p>我是首页的内容,123456.</p>
  </div>
</template>
<script type="text/ecmascript-6">
export default {
  name: 'Home'
}
</script>
<style scoped>
</style>
```

> About组件

```vue
<template>
  <div class="page-contianer">
    <h2>这是关于页面</h2>
    <p>我是关于页面的内容，about。</p>
  </div>
</template>
<script type="text/ecmascript-6">
export default {
  name: 'About'
}
</script>
<style scoped>
</style>
```

‍

### 配置路由映射

路由映射: 组件和路径映射关系

在路由与组件对应关系配置在`routes`​中。

> 修改index.js

```javascript
import Vue from 'vue'
import Router from 'vue-router'
import Home from '@/components/Home'

// 2. 通过Vue.use(插件)，安装插件
Vue.use(Router)
//3. 创建 router路由对象
const routes = [
  //配置路由和组件之间的对应关系
  {
    path: '/home',//home  前端路由地址
    name: 'Home',
    component: Home //组件名
  },
  {
    path: '/about',//about 前端路由地址
    name: 'About',
    component: () => import('@/components/About') //懒加载组件
  }
]
const router = new Router({
  //配置路由和组件之间的应用关系
  routes
})
//4.导出router实例
export default router
```

‍

### 使用

通过`<router-link>`​和`<router-view>`​

在app.vue中使用`<router-link>`​和`<router-view>`​ 两个全局组件显示路由。

> ​`<router-link>`​是全局组件，最终被渲染成a标签，但是`<router-link>`​只是标记路由指向类似一个a标签或者按钮一样，但是我们点击a标签要跳转页面或者要显示页面，所以就要用上`<router-view>`​。
>
> ​`<router-view>`​ 是用来占位的，就是路由对应的组件展示的地方，该标签会根据当前的路径，动态渲染出不同的组件。
>
> 路由切换的时候切换的是`<router-view>`​挂载的组件，其他不会发生改变。
>
> ​`<router-view>`​默认使用hash模式，可以在index.js中配置修改为history模式。

> app.vue修改template

```vue
<template>
  <div id="app">
    <router-link to="/home">首页</router-link> |
    <router-link to="/about">关于</router-link>
    <router-view/>
  </div>
</template>
```

使用`npm run dev`​启动项目，此时`<router-view>`​在`<router-link>`​下面，那渲染页面就在下面，此时未配置路由的默认值，所以第一次进入网页的时候`<router-view>`​占位的地方是没有内容的。

‍

### 路由的默认值和history模式

‍

> 路由的默认值，修改index.js的routes

```javascript
const routes = [
  {
    path: '',
    redirect: '/home'//缺省时候重定向到/home
  },
  //配置路由和组件之间的对应关系
  {
    path: '/home',//home  前端路由地址
    name: 'Home',
    component: Home //组件名
  },
  {
    path: '/about',//about 前端路由地址
    name: 'About',
    component: () => import('@/components/About') //懒加载组件
  }
]
```

添加缺省值，并重定向到`/home`​路径，此时打开[http://localhost:8080](http://localhost:8080/) ，直接显示home组件内容。

> 修改hash模式为history模式，修改index.js的router对象

```javascript
const router = new Router({
  //配置路由和组件之间的应用关系
  routes,
  mode: 'history'//修改模式为history
})
```

‍
