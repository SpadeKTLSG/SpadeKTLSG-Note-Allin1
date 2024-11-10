‍

‍

npm（“Node 包管理器” Node Packaged Modules）是 JavaScript 运行时 Node.js 的默认程序包管理器; Node本身提供了一些基本API模块，但是这些基本模块难以满足开发者需求. Node需要通过使用NPM来管理开发者自我研发的一些模块，并使其能够共用与其他开发者

NPM是随同NodeJS一起安装的包管理工具，能解决NodeJS代码部署上的很多问题

npm: nodejs的包管理工具，可以下载使用公共仓库的包，类似maven 包安装分为本地安装（local）、全局安装（global）两种

```
npm install express          # 本地安装express
npm install express -g       # 全局安装express
npm list -g                 #查看所有全局安装的模块
```

‍

‍

### Header

nodejs 的包管理工具

Node 成功的主要因素之一是它广受欢迎的软件包管理器——npm，因为 npm 使 JavaScript 开发人员可以快速方便地共享软件包

‍

#### 环境

使用终端命令 npm list vue即可查看vue版本

‍

‍

# 知识

‍

## 结构

npm 由两个主要部分组成

* 用于发布和下载程序包的 CLI（命令行界面）工具
* 托管 JavaScript 程序包的 在线存储库

‍

# 基础

‍

## 项目

‍

### 运行

‍

‍

#### 终端启动

File - open -选择源码文件夹下的文件夹(根目录) - CMD

npm run serve

‍

‍

#### IDEA自启动

给idea设置自启动便捷方式，方便启动项目

选择Edit Configurations编辑运行配置

选择npm

根据项目而来有些事dev有些是serve ，这里选择serve，然后先Apply再OK

‍

‍

### 停止

‍

## 管理

‍

### 初始化项目

‍

**删除package-lock.json和node_modules 文件**

‍

package-lock.json记录了整个node_moudles文件夹的树状结构，还记录了模块的下载地址，但是它是基于项目作者的npm版本库生成的，若不删掉这个依赖文件，容易出现npm版本差异导致的报错. 

‍

**进入项目的终端清除npm缓存**

‍

npm有缓存时，常常出现安装依赖不成功的现象，且一旦出现这个问题，报错信息很完善，但根据报错信息一项一项去解决，却死活解决不了，还找不出原因. 控制台输入下面命令清除npm缓存，npm有缓存时，常常出现安装依赖不成功的现象

目录CMD

```html
npm cache clean -force
```

‍

**重新安装依赖运行**

```cmd
npm install
```

```cmd
npm run service
```

‍

‍

## 镜像

‍

### 管理

‍

#### 淘宝镜像

淘宝 NPM 镜像是一个完整 npmjs.org 镜像，可以用此代替官方版本.

使用淘宝定制的 cnpm 命令行工具代替默认的 npm:(需要管理员权限)

2024新版

```
npm config set registry https://registry.npmmirror.com
```

查看镜像使用状态

```xml
npm config get registry
```

如果返回[https://registry.npmmirror.com](https://registry.npmmirror.com/ "https://registry.npmmirror.com")，说明配置的是淘宝镜像。

‍

‍

‍

‍

# 实操

‍

## Bugfix

‍

### 新版本问题 digital envelope routines BUG

[Link](https://blog.csdn.net/fengyuyeguirenenen/article/details/128319228)

‍

修改package.json，在相关构建命令之前加入SET NODE_OPTIONS=--openssl-legacy-provider

```
"scripts": {
   "serve": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service serve",
   "build": "SET NODE_OPTIONS=--openssl-legacy-provider && vue-cli-service build"
},
```

‍

## pnmp组件安装位置

```xml
pnpm config set store-dir <new path> //将<new path> 替换为目标存储路径(非中文)
```

验证

```xml
pnpm store path
```
