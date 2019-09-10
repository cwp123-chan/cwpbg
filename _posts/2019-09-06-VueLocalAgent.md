---
title: Vue本地代理
date: 2019-09-06 23:40:49
---
# Vue本地代理

## 1. 为什么要本地代理

##### 如果在工作中,后台给你数据接口之后,会给你一个示例,示例就是当你请求成功之后后台应该给你返回什么;

##### 		是为了 方便前台进行测试

## 2. 本地代理和方向代理是冲突的

##### 		如果要同时使用本地代理和方向代理 需进一步配置,步骤繁琐

## 3. 配置本地代理

##### 		(1) 配置 webpack-dev-conf.js文件

##### 		(2) 在 const portfinder = require ('portfinder')下配置

```
const portfinder = require('portfinder')
// 开始设置本地代理
// 声明变量接收express组件
const express = require('express')
// 请求server,赋值express函数
var app = express()
// 加载本地数据(本地的api文件)
var appData = require('../src/data/homeData.json')
// 配置路由文件
var apiRoutes = express.Router()
// 通过路由请求数据
app.use('/api',apiRoutes)
```

##### 	(3) 配置服务,在devServer里面加上before()方法

```vue
 devServer: {
    clientLogLevel: 'warning',
    historyApiFallback: {
      rewrites: [
        { from: /.*/, to: path.posix.join(config.dev.assetsPublicPath, 'index.html') },
      ],
    },
    hot: true,
    contentBase: false, // since we use CopyWebpackPlugin.
    compress: true,
    host: HOST || config.dev.host,
    port: PORT || config.dev.port,
    open: config.dev.autoOpenBrowser,
    overlay: config.dev.errorOverlay
      ? { warnings: false, errors: true }
      : false,
    publicPath: config.dev.assetsPublicPath,
    proxy: config.dev.proxyTable,
    quiet: true, // necessary for FriendlyErrorsPlugin
    watchOptions: {
      poll: config.dev.poll,
    },
    before(app){
      // 不同版本不同(旧版)
      app.get('/api/homeData',function(req,res){
        res.json({
// 如果没有错误返回0 // 并且返回数据
          errno:0,
          data:appData
        })
      })
      // (新版)
      // app.get('/homeData',function(req,res){
      //   res.json({

      //     errno:0,
      //     data:appData
      //   })
      // })
    }
  },
```

```
如果出现:
xhr.js?363a:172 GET http://localhost:8080/api/homeData 500 (Internal Server Error)  500错误

1. 如果出现500 或者502 第一种情况就是前台参数错误,后台无法返回.
2. 第二种是 后台数据为空
```

```
如果出现:
xhr.js?363a:172 GET http://localhost:8080/api/homeData 404

那就是后台代码写错,或者 请求地址无效
```

