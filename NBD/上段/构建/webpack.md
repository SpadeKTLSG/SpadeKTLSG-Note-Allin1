前端模块化打包工具

webpack是一个JavaScript应用的静态模块打包工具

‍

‍

### Header

‍

‍

# 知识

‍

‍

## 评价

‍

> 模块化

webpack可以支持前端模块化的一些方案，例如AMD、CMD、CommonJS、ES6。可以处理模块之间的依赖关系。不仅仅是js文件可以模块化，图片、css、json文件等等都可以模块化。

> 打包

webpack可以将模块资源打包成一个或者多个包，并且在打包过程中可以处理资源，例如压缩图片，将scss转成css，ES6语法转成ES5语法，将TypeScript转成JavaScript等等操作。**grunt/gulp**也可以打包。

‍

‍

### **和grunt/glup的对比**

* grunt/glup的核心是Task

  * 我们可以配置一系列的task，并且定义task要处理的事务（例如ES6/TS转化，图片压缩，scss转css）
  * 之后可以让grunt/glup来依次执行这些任务，让整个流程自动化
  * 所以grunt/glup也被称为前端自动化任务管理工具
* 看一个gulp例子

  * task将src下的js文件转化为ES5语法
  * 并输入到dist文件夹中

    ```javascript
    const gulp = require('gulp')
        const babel = require('gulp-babel')
        gulp.task('js'()=>
          gulp.src('src/*.js')
            .pipe(babel({
              presets:['es2015']
            }))
            .pipe(gulp.dest('dist'))
        );
    ```
* 什么时候使用grunt/gulp呢？

  * 如果工程依赖简单，甚至没有模块化
  * 只需要进行简单的合并/压缩
  * 如果模块复杂，相互依赖性强，我们需要使用webpack
* grunt/glup和webpack区别

  * grunt/glup更加强调的是前端自动化流程，模块化不是其核心
  * webpack加强模块化开发管理，而文件压缩/合并/预处理等功能，是附带功能

‍

‍

# 基础

‍

## 搭建

‍

1. webpack依赖node环境。
2. node环境依赖众多包，所以需要npm，npm（node packages manager）node包管理工具
3. nvm是node管理工具可以自由切换node环境版本

**全局安装webpack**

```shell
npm install webpack -g
//指定版本安装
npm install webpack@3.6.0 -g
```

> 由于vue-cli2基于webpack3.6.0  
> 如果要用vue-cli2的可以使用`npm install webpack@3.6.0 -g`​

‍

**局部安装**

```shell
npm install webpack --save-dev
```

* 在终端执行webpack命令，使用的是全局安装。
* 当在package.json中定义了scripts时，其中包括了webpack命令，那么使用的是局部webpack

‍

一般整合入项目, 因此不演示基础打包过程

‍

## 配置

‍

‍

## loader

loader是webpack中一个非常核心的概念。

webpack可以将js、图片、css处理打包，但是对于webpack本身是不能处理css、图片、ES6转ES5等。

此时就需要webpack的扩展，使用对应的loader就可以。

**loader使用**

> 步骤一：通过npm安装需要使用的loader

> 步骤二：通过webpack.config.js中的modules关键字下进行配置

大部分loader可以在webpack的官网找到对应的配置。

‍

## plugin

plugin插件用于扩展webpack的功能的扩展，例如打包时候优化，文件压缩。

**loader和plugin的区别**

loader主要用于转化某些类型的模块，是一个转化器。

plugin主要是对webpack的本身的扩展，是一个扩展器。

**plugin的使用过程**

步骤一：通过npm安装需要使用的plugins(某些webpack已经内置的插件不需要在安装)

步骤二：在**webpack.config.js**中的plugins中配置插件。

‍

# 高级

‍

## 本地服务器

‍

webpack提供了一个可选的本地开发服务器，这个本地服务器基于node.js搭建，内部使用了express框架，可以实现热启动。

> 准备工作复制05-webpack的plugin文件夹到同级目录，并改名为06-webpack搭建本地服务器。

不过这是一个单独的模块，在webpack中使用之前需要先安装：

```shell
npm install --save-dev webpack-dev-server@2.9.1
```

devServe也是webpack中一个选项，选项本省可以设置一些属性：

* contentBase：为哪个文件夹提供本地服务，默认是根文件夹，这里我们需要改成./dist
* port：端口号
* inline：页面实时刷新
* historyApiFallback：在SPA（单页面富应用）页面中，依赖HTML5的history模式

修改webpack.config.js的文件配置

```javascript
//2.配置webpack的入口和出口
module.exports = {
 ...
  devServer: {
    contentBase: './dist',//服务的文件夹
    port: 4000,
    inline: true//是否实时刷新
  }

}

```

配置package.json的script：

```json
"dev": "webpack-dev-server --open"
```

> --open表示直接打开浏览器

启动服务器

```shell
npm run dev
```

启动成功，自动打开浏览器，发现在本地指定端口启动了，此时你修改src文件内容，**会热修改。**

‍

‍

# 实操

‍

## Bugfix
