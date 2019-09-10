---
title: webpack 安装与使用
date: 2019-09-05 23:40:49
---
# webpack 安装与使用

## 1. 前端的两种架构方式

### (1) 传统架构

​		传统架构方式就是同学们之前写页面的方式，有多个HTML文件，并且每个HTML文件都引入自己的js和css。传统方式更适合原生JS开发或者Jquery开发。

### (2) Spa架构（单页面应用）

​		前端页面最终只有一个HTML文件，并且只引入了一个js文件，其他的js，css，image，都被打包到一个js文件里进行引入。

​		Spa架构是前端模块化开发的体现。

​		Spa架构更适合React,Vue进行开发。

## 2. webpack是什么？

### (1) 概念

Webpack是当前最火的项目压缩打包工具，他的目标是把所有的JS，CSS，Image等都打包成一个js文件，并且最后引入该文件即可。

### (2) 安装

#### ① 安装Node

Webpack需要在node.js的express框架下进行运行，另外还需要node.js中的npm工具，安装webpack。

简单来说，如果要安装webpack，必须安装node.js。

安装完毕之后，需要验证node.js和npm是否安装成功。在终端输入:

Npm  -v

如果出现版本号，说明安装成功。

#### ②安装cnpm

因为npm下载东西需要翻墙，而且下载速度极慢，所以一般情况下，我们会使用cnpm进行安装。

进入淘宝cnpm镜像官方网站：https://npm.taobao.org/

在终端运行如下代码：

#### ③ 安装webpack

在终端输入：cnpm  install  webpack  -g

-g表示全局安装，如果不是全局安装，你只能在你安装的文件夹内使用webpack，如果是全局安装，你可以在任何地方运行webpack。

安装完毕之后，测试webpack  --version是否安装成功。出现以下信息，说明webpack安装成功

#### ④ 如果是4.xx.x以上版本的webpack需要额外安装webpack-cli

在终端输入指令：cnpm  install  -g  webpack-cli

### (2) 使用webpack

在需要打包的文件夹内点击右键，打开终端，输入webpack 需要打包的js文件

如果运行成功，那么在项目文件夹内会多出一个文件夹，叫做dist，里面有一个main.js文件，这个文件就是打包之后的文件。

### (3) 利用webpack配置文件进行打包

Webpack的配置文件的名字必须是webpack.config.js，其他名字webpack不能识别在配置文件内，需要填写入口属性(entry)，出口属性(output)。

#### 入口属性需要注意，如果js文件在当前文件夹下，必须写上'./'

```js
module.exports = {
    // 入口属性 ,entry,代表的是我要打包的那个js文件
        // 一定要写上./
    entry: './once.js',
    // 代表出口属性
    output:{
        //  这里表示出口文件的名称
        // 一定要写上./
        filename:'./selfmain.js'
    }
}
```

此时，在终端直接运行webpack命令，webpack会直接调用配置文件，进行打包。打包成功之后，会在dist文件夹内生成你规定的命名的文件。

#### 如果要打包多个js文件，需要进行模块的抛出和引入。

模块的抛出依赖于module.exports语法，该语法可以将模块里面的任何内容进行抛出

```js
var demo = function(){
    // 必须是抛出方法
    console.log("我是抛出文件")
    console.log("使用webpack --watch 进行监听哦")
}

// 通过 module.exports抛出 如需打包 则可以被打包入口文件接收
module.exports = {
    // 只能抛出方法名
    demo
}
```

模块的引入需要require(‘文件路径’)语法进行引入模块

```js
// 导入模块
var newdemo = require('./new')
console.log('我是nocejs文件')
// 使用导入文件
newdemo.demo();
```

Webpack --watch 让webpack自动监听我们的改动，不需要手动执行命令。原理是开启了一个node.js服务器，让webpack持续运行，并且监听所有引入模块的代码改动，每当有代码改动的时候都会重新构建。