‍

‍

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

异步的 JavaScript 和 XML- AXIOS

‍

‍

### Header

‍

‍

# 知识

‍

‍

## 特性

最大的优点是在不重新加载整个页面的情况下，可以与服务器交换数据并更新部分网页内容.

实现异步交互 --请求可以不导致整个页面的重新加载，并且只针对局部模块的数据进行数据的更新

原生的繁琐, Axios 对原生的Ajax进行了封装, 主要用这个

‍

‍

* 浏览器端发起XMLHttpRequests请求
* node端发起http请求
* 支持Promise API
* 监听请求和返回
* 转化请求和返回
* 取消请求
* 自动转化json数据
* 客户端支持抵御

‍

‍

# 基础

‍

## 搭建

‍

==vue使用axios==

```shell
   npm install axios --save
```

或者使用cdn

```javascript
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

‍

‍

==普通使用axios==

导入    `<script src="js/axios-0.18.0.js"></script>`​

‍

‍

### 示例

```html
<body>
    <input type="button" value="获取数据GET" onclick="get()" />

    <input type="button" value="删除数据POST" onclick="post()" />
</body>

<script>

    function get() {
      axios({
        method: "get",
        url: "http://yapi.smart-xwork.cn/mock/169327/emp/list",
      }).then((result) => {
        console.log(result.data);
      });
    }


    function post() {
      axios({
        method: "post",
        url: "http://yapi.smart-xwork.cn/mock/169327/emp/deleteById",
        data: "id=1",
      }).then((result) => {
        console.log(result.data);
      });
    }

</script>
```

‍

‍

‍

## 组成

‍

1. method属性：用来设置请求方式, 取值为 get 或者 post.
2. url属性：用来书写请求的资源路径. 如果是 get 请求，需要将请求参数拼接到路径的后面，格式为： url?参数名=参数值&参数名2=参数值2.
3. data属性：作为请求体被发送的数据. 也就是说如果是 post 请求的话，数据需要作为 data 属性的值.

‍

then() 需要传递一个匿名函数(回调函数). 回调函数中的 resp 参数是对响应的数据进行封装的对象，通过 resp.data 可以获取到响应的数据.

匿名函数在发送请求时不会被调用，而是在成功响应后调用的

‍

‍

## 操作

‍

请求方式 (加上 axios.)

get(url [, config])

delete(url [, config])

post(url [, data[, config]])

put(url [, data[, config]])

‍

‍

### get

```javascript
axios({
    method:"get",
    url:"http://localhost:8080/ajax-demo1/aJAXDemo1?username=zhangsan"
}).then(function (resp){
    alert(resp.data);
})
```

简写

```html
axios
    .get("http://yapi.smart-xwork.cn/mock/169327/emp/list")
    .then((result) => {
      console.log(result.data);
    });
```

```javascript
axios.get('/user', {
  params: {
    name: 'krislin'
  }
}).then(function (response) {
  console.log(response);
}).catch(function (error) {
  console.log(error)
}
```

‍

### post

```javascript
axios({
    method:"post",
    url:"http://localhost:8080/ajax-demo1/aJAXDemo1",
    data:"username=zhangsan"
}).then(function (resp){
    alert(resp.data);
});
```

简写

```javascript
axios
    .post("http://yapi.smart-xwork.cn/mock/169327/emp/deleteById", "id=1")
    .then((result) => {
      console.log(result.data);
    });
```

‍

```javascript
axios.post('/user',{
    name:'krislin',
    address:'china'
})
.then(function(response){
    console.log(response);
})
.catch(function(error){
    console.log(error);
});
```

‍

### 并发

```javascript
function getUserAccount(){
    return axios.get('/user/12345');
}

function getUserPermissions(){
    return axios.get('/user/12345/permissions');
}

axios.all([getUerAccount(),getUserPermissions()])
    .then(axios.spread(function(acc,pers){
        //两个请求现在都完成
}));
```

‍

## 请求API配置

axios 能够在进行请求时进行一些设置

‍

示例

```javascript
axios({
    method:'post',
    url:'/user/12345',
    data:{
        name:'krislin',
        address:'china'
    }
});
```

‍

## 请求设置

请求配置中，只有url是必须的，如果没有指明的话，默认是Get请求

```javascript
{
    //`url`是服务器链接，用来请求用
    url:'/user',
 
    //`method`是发起请求时的请求方法
    method:`get`,
 
    //`baseURL`如果`url`不是绝对地址，那么将会加在其前面。
    //当axios使用相对地址时这个设置非常方便
    //在其实例中的方法
    baseURL:'http://some-domain.com/api/',
 
    //`transformRequest`允许请求的数据在传到服务器之前进行转化。
    //这个也支持`PUT`,`GET`,`PATCH`方法。
    //数组中的最后一个函数必须返回一个字符串，一个`ArrayBuffer`,或者`Stream`
    transformRequest:[function(data){
        //依自己的需求对请求数据进行处理
        return data;
    }],
 
    //`transformResponse`允许返回的数据传入then/catch之前进行处理
    transformResponse:[function(data){
        //依需要对数据进行处理
        return data;
    }],
 
    //`headers`是自定义的要被发送的头信息
    headers:{'X-Requested-with':'XMLHttpRequest'},
 
    //`params`是请求连接中的请求参数，必须是一个纯对象，或者URLSearchParams对象
    params:{
        ID:12345
    },
  
    //`paramsSerializer`是一个可选的函数，是用来序列化参数
    //例如：（https://ww.npmjs.com/package/qs,http://api.jquery.com/jquery.param/)
    paramsSerializer: function(params){
        return Qs.stringify(params,{arrayFormat:'brackets'})
    },
 
    //`data`是请求提需要设置的数据
    //只适用于应用的'PUT','POST','PATCH'，请求方法
    //当没有设置`transformRequest`时，必须是以下其中之一的类型（不可重复？）：
    //-string,plain object,ArrayBuffer,ArrayBufferView,URLSearchParams
    //-仅浏览器：FormData,File,Blob
    //-仅Node：Stream
    data:{
        firstName:'fred'
    },
    //`timeout`定义请求的时间，单位是毫秒。
    //如果请求的时间超过这个设定时间，请求将会停止。
    timeout:1000,
  
    //`withCredentials`表明是否跨域请求，
    //应该是用证书
    withCredentials:false //默认值
 
    //`adapter`适配器，允许自定义处理请求，这会使测试更简单。
    //返回一个promise，并且提供验证返回（查看[response docs](#response-api)）
    adapter:function(config){
        /*...*/
    },
 
    //`auth`表明HTTP基础的认证应该被使用，并且提供证书。
    //这个会设置一个`authorization` 头（header），并且覆盖你在header设置的Authorization头信息。
    auth:{
        username:'janedoe',
        password:'s00pers3cret'
    },
 
    //`responsetype`表明服务器返回的数据类型，这些类型的设置应该是
    //'arraybuffer','blob','document','json','text',stream'
    responsetype:'json',
 
    //`xsrfHeaderName` 是http头（header）的名字，并且该头携带xsrf的值
    xrsfHeadername:'X-XSRF-TOKEN'，//默认值
 
    //`onUploadProgress`允许处理上传过程的事件
    onUploadProgress: function(progressEvent){
        //本地过程事件发生时想做的事
    },
 
    //`onDownloadProgress`允许处理下载过程的事件
    onDownloadProgress: function(progressEvent){
        //下载过程中想做的事
    },
 
    //`maxContentLength` 定义http返回内容的最大容量
    maxContentLength: 2000,
 
    //`validateStatus` 定义promise的resolve和reject。
    //http返回状态码，如果`validateStatus`返回true（或者设置成null/undefined），promise将会接受；其他的promise将会拒绝。
    validateStatus: function(status){
        return status >= 200 && stauts < 300;//默认
    },
 
    //`httpAgent` 和 `httpsAgent`当产生一个http或者https请求时分别定义一个自定义的代理，在nodejs中。
    //这个允许设置一些选选个，像是`keepAlive`--这个在默认中是没有开启的。
    httpAgent: new http.Agent({keepAlive:treu}),
    httpsAgent: new https.Agent({keepAlive:true}),
 
    //`proxy`定义服务器的主机名字和端口号。
    //`auth`表明HTTP基本认证应该跟`proxy`相连接，并且提供证书。
    //这个将设置一个'Proxy-Authorization'头(header)，覆盖原先自定义的。
    proxy:{
        host:127.0.0.1,
        port:9000,
        auth:{
            username:'cdd',
            password:'123456'
        }
    },
 
    //`cancelTaken` 定义一个取消，能够用来取消请求
    //（查看 下面的Cancellation 的详细部分）
    cancelToke: new CancelToken(function(cancel){
    })
}
```

‍

## 响应数据

Response

‍

一个请求的返回包含以下信息

```javascript
{
    //`data`是服务器的提供的回复（相对于请求）
    data{},
 
    //`status`是服务器返回的http状态码
    status:200,
 
 
    //`statusText`是服务器返回的http状态信息
    statusText: 'ok',
 
    //`headers`是服务器返回中携带的headers
    headers:{},
 
    //`config`是对axios进行的设置，目的是为了请求（request）
    config:{}
}
```

‍

## Vue动态加载

‍

Vue及Axios完成数据的动态加载展示, 需要使用Ajax请求获取后台的数据. 页面加载完成，自动发送请求，展示数据，需要借助vue的mounted钩子函数. 拿到数据后借助v-for指令来遍历数据

‍

‍

‍

### 示例

‍

主体是一个table表格

```html
<div id="app">
        <table border="1" cellspacing="0" width="60%">
          <tr>
          <th>编号</th>
          <th>姓名</th>
              <th>图像</th>
          <th>性别</th>
          <th>职位</th>
          <th>入职日期</th>
          <th>最后操作时间</th>
          </tr>
        <tr align="center" v-for="(emp,index) in emps"> //遍历
          <td>{{index + 1}}</td> //插值表达式
          <td>{{emp.name}}</td>
          <td>
            <img :src="emp.image" width="70px" height="50px" />
          </td>
          <td>
            <span v-if="emp.gender == 1">男</span>
            <span v-if="emp.gender == 2">女</span>
          </td>
          <td>{{emp.job}}</td>
          <td>{{emp.entrydate}}</td>
          <td>{{emp.updatetime}}</td>
        </tr>
<div>
```

‍

核心: 引入Vue 以及 钩子函数

```html
<script>
    new Vue({
       el: "#app",
       data: { 
         emps:[] //data对象
       },
       mounted () {// 挂载完成后触发此钩子函数
          //发送异步请求,加载数据
          axios.get("http://yapi.smart-xwork.cn/mock/169327/emp/list").then(result => 
           {
            this.emps = result.data.data;//将数据保存到thisVue对象的emps中
          })
       }
    });
</script>
```

‍

‍

‍

# 高级

‍

## 拦截器

Interceptors

你可以在 请求 或者 返回 被 then 或者 catch 处理之前对他们进行拦截。

添加拦截器:

```javascript
//添加一个请求拦截器
axios.interceptors.request.use(function(config){
    //在请求发送之前做一些事
    return config;
},function(error){
    //当出现请求错误是做一些事
    return Promise.reject(error);
});

//添加一个返回拦截器
axios.interceptors.response.use(function(response){
    //对返回的数据进行一些处理
    return response;
},function(error){
    //对返回的错误进行一些处理
    return Promise.reject(error);
});
```

移除拦截器：

```javascript
var myInterceptor = axios.interceptors.request.use(function(){/*...*/});
axios.interceptors.rquest.eject(myInterceptor);
```

‍

## 封装

在日常项目开发过程中，在和后台交互获取数据的时候，我们都需要使用到网络库，通常在vue的项目中 ，使用的是 **axios** 库 ，在此基于自身项目业务，做一个二次封装。

‍

‍

### 条件准备

在UI轻提示组件上，选定的是 **vant** 库中的 **Toast** 组件([Vant文档](https://youzan.github.io/vant/#/zh-CN/intro)),可按实际需要选定具体要使用的UI框架

安装:

```shell
npm install vant --save
```

数据序列化，如果有实际需要的项目，可以使用`qs`​,在这里做一个简单的介绍

安装:

```shell
npm install qs --save
```

**qs.stringify和JSON.stringify的使用和区别**

qs.stringify()将对象 序列化成URL的形式，以&进行拼接

JSON.stringify 是将对象转化成一个json字符串的形式

**用法:**

```javascript
  var a = {name:'xiaoming',age:10}
  
  qs.stringify(a); //log: 'name=xiaoming&age=10'

  JSON.stringify(a) //log: '{"name":"hehe","age":10}'
```

基于底层配置和业务接口分离，在src目录中会新建文件夹 **httpServer**,同时新建立 **ajax.js** 和 **api.js** 文件

```text
ajax.js: axios的二次封装，作为基础网络库，添加基础的配置
```

```text
api.js: 管理项目实际业务基础接口的输出，以及返回响应数据的处理
```

在日常项目模块中，基于多人开发，当然可以在api.js的基础上，可以根据功能模块实现业务拓展延伸，比如

```javascript
小明负责list模块业务

新建api-list.js，并导入api.js .... 

//api-list.js文件中:
import api from './api'

export default {
    getList(url,params){
        api.get(url,params)
    }
}
```

对于个别项目，可能存在多个域名配置的情况下, 可以重新建立 **base.js** , 来管理多个接口域名

**base.js:**

```javascript
/**
 * 接口域名的管理
 */
const base = {
  sq: 'https://xxxx111111.com/api/v1',
  bd: 'http://xxxxx22222.com/api'
}

export default base;
```

‍

### axios封装

(单域名)

‍

‍

**src/main.js文件:**

```javascript
import Vue from 'vue'
import App from './App'
import router from './router'
import Api from './httpServer/api'

//挂载到vue的全局属性上
Vue.prototype.$https = Api

Vue.config.productionTip = false

new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})
```

‍

**src/httpServer/ajax.js文件:**

```javascript
import axios from 'axios'
import {Toast} from 'vant'


const ajax = axios.create({
     timeout:60000,
     baseURL:process.env.BASE_URL   //基础域名
})


/**
 * 请求拦截器
 * 每次请求前，如果存在token则在请求头中携带token
 */
ajax.interceptors.request.use(
  config => {
    //判断token(根据实际情况拦截)
   
    return config;
  },
  error => Promise.error(error)
)

/**
 * 响应拦截器
 */
ajax.interceptors.response.use(
  // 请求成功
  res => res.status === 200 ? Promise.resolve(res) : Promise.reject(res),
  error => {
    const {response} = error;
    if (response) { // 请求已发出，但是不在2xx的范围
      Toast({message: response.message});
      return Promise.reject(response);
    } else {
      // 处理断网的情况
      // eg:请求超时或断网时，更新state的network状态
      // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
      // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
      Toast({message: '网络开小差，请稍后重试'});
    }
  }
)

export default ajax;
```

‍

对于**process.env.BASE_URL**的配置，在开发环境中，需要以代理的方式进行访问:

```javascript
//config/dev.env.js

'use strict'
const merge = require('webpack-merge')
const prodEnv = require('./prod.env')

module.exports = merge(prodEnv, {
  NODE_ENV: '"development"',
  BASE_URL:'"/api"'  //对api进行处理
})
```

```javascript
//config/prod.env.js

'use strict'
module.exports = {
  NODE_ENV: '"production"',
  BASE_URL:'"https://www.xxx.com"' //生产环境不需要处理
}
```

```javascript
//config/index.js
...

   proxyTable: {
       '/api': {
        target: 'https://tasst.sinoxk.cn',//后端接口地址
        changeOrigin: true,//是否允许跨越
        pathRewrite: {
          '^/api': '',//重写(接口地址带api会被替换)
        },
      }
    },
...
```

‍

**src/httpServer/api.js文件:**

```javascript
import ajax from './ajax'
import {Toast} from 'vant'

/**
 *  业务接口成功或者失败的情况处理
 *
 */
const handleResponse = (res, success, failure) => {
  switch (res.code) {
    case 200:   //成功
      success && success(res.data);
      break;
    case 401:   //登录token失效

      break;
    default:
      if (failure) {
        failure(res);
      } else {
        Toast({message:res.msg || '请求失败，请稍后重试!'});
      }
      break;
  }
}

export default {
  get: function (url, params, success, failure) {
    ajax.get(url, {
      params: params
    }).then(res => {
      if (res.status == 200) {
        handleResponse(res.data.data, success, failure);
      }
    });
  },

  post: function (url, params, success, failure) {
    ajax.post(url, params).then(res => {
      if (res.status == 200) {
        handleResponse(res.data.data, success, failure);
      }
    })
  }
}
```

‍

在**src/components/HelloWorld.vue**文件中使用:

```javascript
<script>
  export default {
    name: 'HelloWorld',
    data() {
      return {
        msg: 'Welcome to Your Vue.js App'
      }
    },
    created() {
     //请求接口数据
      this.$https.get('/xkzx/member/service', {
        pageNum: 1,
        pageSize: 10
      }, function (data) { //成功
         console.log(data);

      }, function (res) {  //失败

      })

    }
  }
</script>
```

‍

# 实操

‍

## Bugfix

‍

‍

‍

‍
