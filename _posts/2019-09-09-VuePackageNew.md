---
title: Vue-cli脚手架
date: 2019-09-09 23:40:49
---
# **Vue-cli脚手架**

## **1.** **准备工作**

(1) Vue和vue-cli的全局安装，如果cnpm安装失败，需要用npm 翻墙进行安装

```
npm install -g vue vue-cli
```



(2) Webpack全局安装

## **2.** **创建项目工程**

1.创建项目工程通过官方的vue-cli初始化项目，并且采用webpack示例

在终端输入: vue  init  webpack  (项目名称，不能是中文)

2.初始化完毕之后，进入项目目录，进入douban通过npm install进行依赖的安装。

依赖安装完毕之后，需要在index.html中添加meta标签，获取设备的真是宽度。

##### 注意：meta标签必须填写在页面最上方位置，尤其不能写在flexible.js的下面。

验证项目是否可以运行，在项目目录执行npm  run  dev。如果在浏览器能够访问成功，则说明整个项目初始化没有任何问题。

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>douban</title>
  </head>
  <body>
    <div id="app"></div>
    <!-- built files will be auto injected -->
  </body>
</html>
```



## **3.** **项目目录分析**

| 关键文件夹   | 说明                                          |
| ------------ | --------------------------------------------- |
| Index.html   | 唯一入口html文件                              |
| Package.json | 项目配置文件，里面列举了项目所需的依赖等等    |
| Src          | 所有主要程序文件夹                            |
| Src/App.vue  | 项目入口vue文件                               |
| Router文件夹 | 规定了所有的router路由                        |
| asssets      | 存放所有的静态文件，包括图片，js文件，css文件 |
| Components   | 存放所有的vue组件                             |

## **4.** **豆瓣APP的tabbar部分**

#### (1) 修改显示的vue文件

在src文件夹内创建一个文件夹，名为pages，并且在pages里面创建Index.vue文件。

修改router里面的Index.js文件。

```html
import Vue from 'vue'
import Router from 'vue-router'
import Index from '@/pages/Index'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'Index',
      component: Index
    }
  ]
})
```



每一个组件的css都要用less来进行编写，所以需要通过npm安装less插件

在命令行输入cnpm  install  less  less-loader  --save

使用less预处理器需要在页面添加lang=”less”

#### (2) 创建Tabbar和TabbarItem模块

#### (3) 将Tabbar和TabbarItem模块导入到Index.vue文件中并且注册组件

#### (4) 完成代码替换，将所有的无关标签替换成组件标签

## **5.** **Slot组件**

可以将父组件放在子组件的内容，放到他想要显示的地方。

如果父级有多个标签需要调整位置显示，那么需要给slot具名。

在上方的元素中，利用slot属性，给该标签标记，在slot标签中用name属性进行对应的标记。

```html
<template>
   <a class="m-tabbar-item">
        <span class="m-tabbar-item-icon">
            <slot name="icon-normal"></slot>
        </span>
        <span class="m-tabbar-item-text">
            <slot></slot>
        </span>
    </a>
</template>
```



## **6.** **export default导出模块**

#### (1) Export和export default都可以导出常量，函数，文件，模块等等

#### (2) 在一个文件中，export可以有多个，但是export default 只能有一个

Export default里面的属性和方法:

Name

name:’index’ 相当于声明了一个全局ID

该属性可以不写

写了可以提供更好的调试信息，具体调试信息在初期开发阶段很少碰到

Components 

该属性是用于注册组件时使用的，例如如果在该文件中引入了a组件，那么需要注册之后才能够使用。

完整写法为:

```html
<script>
import mTabbar from "../components/tabbar";
import mTabbarItem from "../components/tabbar-item";
export default {
  name: "index",
  // Html代码中必须使用m-Tabbar 在html中也会自动解析成mtabbar/mTabbar
  components: {
    // mTabbar,                  // 实际上就是 "m-Tabbar" : mTabbar
    "m-Tabbar":mTabbar,
    mTabbarItem                 //也可同上
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="less">
</style>

```

## **7.** **如果出现spaces（空格）错误的处理方式**

那么需要关闭vue的代码审查机制，Vue代码审查机制过于严格，会导致出现异常错误。

如果出现上图中的情况，可以在Vue文件夹的config文件夹中的index.js中关闭代码审查。

```
useEslint:false   将此处True改为 false
```

## 8 :src 图片问题

```
 <!-- 如果使用:src绑定图片 则必须require或者import 引入图片 -->
 <swiper-slide><swiper-index><img :src="swiperImg1" alt=""></swiper-index></swiper-slide>
 import swiperImg from '../assets/images/swiperimg.jpg'
 export default{
 	data(){
 		return:{
 		 // 轮播图图片(需要引入图片)
          swiperImg1 : swiperImg
          }
 	}
 }
```

