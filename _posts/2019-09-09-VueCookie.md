---
title: 跨域和缓存
date: 2019-09-09 23:40:49
---
# 跨域和缓存

## 1. 跨域问题:

### 	在 开发环境中 我们可以使用 反向代理 但是在生产环境中 不允许使用

开发环境: 开发人员开发软件测试时使用

生产环境: 给客户端使用

### 解决方法:

可以使用jsonp方式请求api,也可以下载 vue jsonp插件

```
cnpm install jsonp
```

## 2. keep-alive

#### 是我们vue中的缓存机制,所有的内容如果添加了keep-alive标签,都不会被第三次渲染,除非ctrl+f5;

## 3. 3px的bug问题

#### 如果是图片直接添加到页面中 ,他的display属性就是行内块状元素(inline-block)所以 所占空间比实际高度大,

#### 所以 一般情况下 图片宽度为定值,图片高度需要自动扩展的情况,可以考虑给图片转换为块状元素



## 4. 设置子路径默认

```
{
path:'/book',
name:'book',
component:Book,
redirect:'/book/playing',
// 如果出现错误 改成 redirect :'/playing'		版本问题 意思就是 当打开 /book时默认 打开/playing
children:[{
path:'子路径地址',
component:子路径名称,
},{},{}]
}
```

