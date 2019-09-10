---
title:  webpack -- 利用前端脚手架
date: 2019-09-05 23:40:49
---
# webpack -- 利用前端脚手架

## 1. 初始化项目，安装webpack

   一般情况下，利用自动化构建工具构建项目的方式叫做初始化项目。

   利用npm构建工具，可以使用**cnpm  init** 命令创建一个项目

   暂时来说，项目名称，版本号，项目入口文件比较关键，其他项比较少用。



##### 创建成功之后，在project中多了一个package.json文件，里面就是该项目的配置文件。

```js
{
  "name": "project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "css-loader": "^3.2.0",
    "jquery": "^3.4.1",
    "node-loader": "^0.6.0",
    "node-sass": "^4.12.0",
    "sass-loader": "^8.0.0",
    "style-loader": "^1.0.0",
    "webpack": "^4.39.3"
  }
}

```

**一般情况下，项目需求的所有工具，模块，插件都会在package.json中被列举出来。**

**如果需要将某个插件或者某个工具引入项目中，需要用到安装命令+--save-dev命令。**

## 2. 创建页面文件，以及js入口文件

在系统中有一些内置的模块，可以被js文件直接拿来调用，比如代表路径的path模块，可以直接在webpack的配置文件中引入使用。(webpack.config.js)

```js
// 我们导入path模块来设定 出口文件的地址
const path = require('path')
module.exports = {
    // 入口文件
    entry : './src/index.js',
    output : {
        filename : 'main.js',
        // 路径应该指向 出口文件的地址
        // 第一个参数: 当前文件夹
        // 第二个参数:当前文件夹中的某个文件夹
        path : path.resolve(__dirname,'dist')
    }
  }
```

## 3.编译并且打包sass文件

首先，在入口文件处引入sass文件，需要用到import语法。(index.js)

```js
var test = require('./text')
document.write("入口文件测试")
// 使用 import 引入 scss文件
import "./index.scss"
// 使用 test.js文件
test.test()
```

**尝试运行打包，发现出现错误，webpack说，可能需要loader支持**

**经过查阅可知，打包sass文件需要四个loader，style-loader sass-loader node-sass css-loader，这四个插件需要用npm+--save-dev的方式进行引入。**

```c
cnpm install style-loader sass-loader node-sass css-loader --save-dev
```

使用loader,需要在webpack的配置文件中添加加载器

```js
// 我们导入path模块来设定 出口文件的地址
const path = require('path')
module.exports = {
    // 入口文件
    entry : './src/index.js',
    output : {
        filename : 'main.js',
        // 路径应该指向 出口文件的地址
        // 第一个参数: 当前文件夹
        // 第二个参数:当前文件夹中的某个文件夹
        path : path.resolve(__dirname,'dist')
    },
    module : {
        // 如果要解析less 需要添加 less规则,如果要解析sass需要添加sass规则
        // 添加规则是为了解析 sass/less 当然 前提必须安装
        // cnpm install style-loader sass-loader node-sass css-loader --save-dev
        //四个包
            rules : [{
                // 解析的文件后缀,需要用到正则表达式
                test:/\.scss$/,
                // 要用到什么loader
                use:[{
                    loader :'style-loader'
                },{
                    loader : 'css-loader'
                },{
                    loader : 'sass-loader'
                }]
            }]
    }
}
```

### 4. 如何接手半成品项目

当接手前端项目的时候，不需要整个将项目拷贝过来，只需要拷贝人写出的代码，以及所有文件的配置文件，包括package.json在内。

直接在项目目录执行，cnpm  install，cnpm会自动下载所有package.json里面的插件，并且导入路径。