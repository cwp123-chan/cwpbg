---
title: 前端的跨域问题
date: 2019-09-05 23:40:49
---
# 前端的跨域问题

### 1.什么是跨域

#### 指的是从一个域名去请求另外一个域名的资源。即跨域名请求！跨域时，浏览器不能执行其他域名网站的脚本

### 2.为什么要跨域?

#### 实工作开发中,因为公司会有很多项目,各个项目或者网站之间需要相互调用对方的资源

#### 举个栗子:我做了一个电影信息的网站,但是我们网站的电影信息量不够,所以我们要去请求豆瓣网站的数据库,把它的电影数据获取到,然后展示在自己的网站上.

### 3.同源策略

所谓同源是指，域名，协议，端口相同。

当一个浏览器的两个tab页中分别打开来 百度和谷歌的页面

当浏览器的百度tab页执行一个脚本的时候会检查这个脚本是属于哪个页面的，

即检查是否同源，只有和百度同源的脚本才会被执行。 

如果非同源，那么在请求数据时，浏览器会在控制台中报一个异常，提示拒绝访问。

**同源策略是浏览器的行为，是为了保护本地数据不被JavaScript代码获取回来的数据污染，因此拦截的是客户端发出的请求回来的数据接收，即请求发送了，服务器响应了，但是无法被浏览器接收。**

### 4.怎么实现跨域呢?

1. 把数据类型改为jsonp的方式

### 5.JSONP优缺点

JSONP优点是兼容性好，可用于解决主流浏览器的跨域数据访问的问题。**缺点是仅支持get方法具有局限**

https://www.juhe.cn/myData // api数据中心

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="./node_modules/jquery/dist/jquery.js"></script>
    <title>Document</title>
    <ul id="ul">
        <li></li>
    </ul>
</head>
<body>
    <script>
        $(function(){
            $.ajax({
                type : 'get',
                url : "http://v.juhe.cn/movie/movies.today?key=4a4a2a620739ab77322e2c7aeaffa797&cityid=3",
                data : {
                    cityid : "3"
                },
                dataType:'jsonp',
                success :function(msg){
                    console.log(msg.result.length)
                    for (var i = 0 ; i < msg.result.length ; i++){
                        $("#ul").append("<li>"+"ID为"+msg.result[i].movieId + "&nbsp;:&nbsp电影名"+msg.result[i].movieName+"</li>")
                        console.log(i)
                    }


                },
                error : function(err){
                    console.log(err)
                }
            })
        })

    </script>
</body>
</html>
```

