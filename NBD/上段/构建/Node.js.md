--前端构建工具

‍

Node.js 就是运行在服务端的 JavaScript

自行执行JavaScript代码需要Node.js解析器才能运行

JavaScript是运行在浏览器的脚本。为了能让JavaScript在服务器上运行，执行服务器上的业务逻辑，产生了NodeJS

‍

‍

‍

### Header

‍

#### 版本

新版的nodejs已经集成了npm，所以npm也一并安装好

‍

#### 附属

‍

nvm 管理 nodejs 和 npm 的版本

npm 可以管理 nodejs 的第三方插件

nodejs 是在项目开发时的所需要的代码库

‍

总之

* Node是做事工具
* npm则是安装工具
* nvm是版本管理

‍

‍

# 知识

‍

## 评价

Node.js 是一个基于Chrome JavaScript 运行时建立的一个平台。  
Node.js是一个事件驱动I/O服务端JavaScript环境。

‍

### 优势

* 单线程高并发。通过JS的回调方法，实现了并发。
* 自带web服务器模块。

‍

# 基础

‍

‍

# 高级

‍

‍

# 实操

‍

## Bugfix

‍

### 关于 NodeJS 环境的常见坑点

* 中文开发者：如果你使用 cnpm 来安装依赖，可能会导致某些包不一致，导致应用起不来，目前原因不明，需要 cnpm 官方来解决。（cnpm 不是单纯的加速节点，它做了很多自己的处理，包括对一些 C++ 编写的 Node 模块做了预编译缓存，因此用它安装的包可能和官方源不一致。这不是 Angular 框架的问题，所有前端框架都存在这个问题。）
* 如果之前装过@angular/cli 需要先卸载：npm uninstall -g @angular/cli
* 如果之前装过老版本的 angular-cli 需要先卸载：npm uninstall -g angular-cli
* 如果你之前已经尝试用 npm install 安装过 node 模块，请手动把 NiceFish 根目录下的 node_moduels 目录删掉重新 npm install
* 命令行删除 node_modules 速度更快，Windows 平台使用： rmdir /s/q node_modules ，*nix 平台使用：sudo rm -rf node_modules
* 如果你遇到其它任何看起来比较玄幻的问题，请手动删掉 node_modules 目录，然后切换到 npm 官方源，重新安装所有 node 模块
* 如果你需要把项目发布到其它类型的 Server 上，例如 Tomcat，需要对 Server 进行一些简单的配置才能支持 HTML5 下的 PushState 路由模式，请从以下链接里面查找对应的配置方式：[https://github.com/angular-ui/ui-router/wiki/Frequently-Asked-Questions](https://gitee.com/link?target=https%3A%2F%2Fgithub.com%2Fangular-ui%2Fui-router%2Fwiki%2FFrequently-Asked-Questions) ，在 How to: Configure your server to work with html5Mode 这个小节里面把常见的 WEB 容器的配置方式都列举出来了，包括：IIS、Apache、nginx、NodeJS、Tomcat 全部都有。（请注意，这个配置不是 Angular 所特有的，当前主流的 SPA 型前端框架都需要做这个配置。）

‍
