---
category: Stuff
path: '/stuff'
title: 'express 基本'
type: 'GET'

layout: nil
---



# express 基本

## 1. 创建express的HTTP服务

```js
1. 引入框架
var express = require('express');
// 创建express的htt服务器
var app = express();
//指定框架的模板引擎,无需导入;
app.set('View engine','ejs');
//指定静态化目录 参一:前台输入url(可以用于作url保密) 参二:加载静态文件目录;
app.use('/static',express.static('./static'));

// 设置路由;
app.get('/',function(req,res){
	//响应输出；
	res.send('hellow');			// 只要url路径为/ 他就会在/下目录寻找
});

app.get('/test',function(){
	res.send('我是test页面')；	// 他会在/tset下目录寻找文件
})


app.get('/user', function(req, res){
    // 绑定并输入数据	user指user页面模块
    res.render('user', {			ejs匹配绑定
        'userlist' : [
            '钢铁侠 屎大颗',
            '绿巨人 浩克',
            '美队 史蒂文',
            '雷神 托尔',
            '邪神 洛基',
            '蜘蛛侠 彼得帕克'
        ]
    });

```



## 2.路由的使用

```js
//支持所有请求方式，实现中间件功能 next 起到过滤作用 匹配到他之后 就跳过他；
app.all('/t',function(req,res,next){
// 响应输出
    console.log(new Date().toString()); //时间
	res.send('Hello');
	next();		如果匹配到他 就next 匹配下一条；
})；

// 设置路由规则
app.get('/', function(req, res){
    // 响应输出
    res.send('Hello Express~~~');
});
// GET
app.get('/t', function(req, res){
    // 响应输出
    res.send('GET 请求');
});
// POST
app.post('/t', function(req, res){
    // 响应输出
    res.send('POST 请求');
});

// DELETE
app.delete('/t', function(req, res){
    // 响应输出
    res.send('DELETE 请求');
});


// 路由方法默认匹配 pathname部分 忽略get参数
// 对大小写不敏感
app.get('/aaa',function(req,res){
	console.log(req.query); // 获取url参数
	res.send('3A页面')；
})；


// 路由路径  默认express path-to-regexp 	匹配路由路径

// 正则路由  /stu/121311232/tom
app.get(/^\/stu\/(\d{10})\/(\w+)$/,function(req.res){
console.log(req.params);		params 匹配参数
    res.send('学员的学号是: '+ req.params[0]);	
})；

// 路由参数
// ：xxx 表示参数占位，使用req.params读取参数
app.get('/tch/:tid',function(req.res){
    console.log(req.params);
    res.send('老师的工号是: '+req.params.tid);
})

// 多个路由参数
app.get('/goods/:name/:num', function(req,res){
    // 参数限制
    var name = req.params.name;
    var num = req.params.num;
    if (/\d+/.test(num)) {
        res.send('商品名: ' + name + ', 入库: ' + num + '件');
    } else {
        res.send('请填写正确的数量');
    }
});



// 设置请求监听
app.listen(3000);

```

## 3. nodejs 回调特性

```js
var http = require('http');
var a = 100;

var server = http.createServer(function (req,res){
    a++;

    res.end(a.toString());

});

server.listen(3000, '127.0.0.1');


// 每次刷新都会有新的值生成
```

## 4. express的路由特性

```js
var express = require('express');
var app = express();

var a = 100;

app.get('/',function(req,res){
    a++;
    res.send(a.toString());
});


app.get('/t',function(req,res){
    a++;
    res.send(a.toString());
});

app.listen(3000);

//无论访问/或/t 都会有新的值生成
```



## 5. 路由具有中间件

```js

var http = require('http');
var fs = require('fs');

// 路由重名；
// 路由中间件
app.get('/kk',function(req,res,next){
    console.log(1);
    next();				# 指 如果访问到我 就执行函数 然后跳过我 访问下一个；
})；

app.get('/k',function(req,res){
   	console.log(2);
    res.send('响应完成')；	# 指 上面跳过中间件之后就可以访问我了；
});



// 匹配冲突的情况
app.get('/:goods/:num',function(req,res，next){
    	// 先做调查/匹配
    if(符合情况){
        console.log(1);
        res.send('商品' + req.params.goods + '数量' + req.params.num);
        
    }else{
        //如果匹配不成功 那么就执行下面路由；
        next();
    }
});

app.get('/admin/login',function(req,res){
    console.log(2);
    res.send('登录界面');
});


app.listen(3000);

```



## 6. app.use 的使用

```js
var express = require('express');
var app = express();

// 全局中间件
app.use(function(req, res, next){			// next 必须写；
    console.log(new Date().toString());
    next();
});

// 中间件
app.use('/admin',function(req,res){
    res.write(req.originUrl + '\n');				//完整的url地址
     res.write(req.baseUrl + '\n');					// 基础URL
     res.write(req.path + '\n');					// 除去基础以外的URL
    res.end('后台')；
})；
app.listen(3000);
```

## 7. 中间件.静态服务.404

```js

var express = require('express');
var fs = require('fs');
var app = express();


// app.use(xx00);
// 体共静态资源服务
// app.use('/static', express.static('./static'));
 app.use('/jingtai',express.static('./static'));			//当请求url有jingtai时 服务器会从static目录下找之后的文件；

add.get('admin',function(req.res){
    res.send('后台。。')；
    
})
// 404

app.use(function(req,res,next){				//第一个参数如果不写 默认从更目录开始寻找
    res.status(404).send('404 not found')
})；

app.listen(3000);

// 第三方模块，处理use页面
// 之前定义的xxoo方法
function xxoo(req,res,next){
        // res.send('XX and OO');
    var filePath = req.originalUrl;
    fs.readFile('./static/'+ filePath, function(err, data){
        if (err) {
            next();
        }
        res.end(data); // 存在即输出
    });
}








```

## 8. 模板 -响应输出；

```js
var express = require('express');
var app = express();

 // 指定框架的模板引擎 无需导入
app.set('View engine','ejs');
// 提供静态服务
// app.use(express.static('./static'));
// 默认使用 .ejs 为模版文件,目录views
// app.set('views', './pages');


app.get('/', function (req,res){
    // 绑定数据并渲染视图
    res.render('user', {
        'userlist' : [
            '钢铁侠 屎大颗',
            '绿巨人 浩克',
            '美队 史蒂文',
            '雷神 托尔',
            '邪神 洛基',
            '蜘蛛侠 彼得帕克'
        ]
    });
});


// send的输出类型
app.get('/hh', function (req,res){
    // node.js end()
    // express send()
    // 二进制
    // res.send(new Buffer('HOOH~'));

    // str
    // res.send('HOOH~');

    // JSON
    // res.send({name:"静静", sec:0});
    // ARRAY
    res.send([15,168,19681,9681,98,986884,true]);
    
});

app.listen(3000);

```



## 9. get和post

```js
var express = require('express');
var bodyParser = require('body-parser');
var app = express();

// body模块读取post参数
// 制定框架的模板无需导入
app.set('view engine', 'ejs');

// parse 	application/x-www-form-urlencoded
// 解析 post 数据
app.use(bodyParser,urlencoded({extended:false}));
//get参数

app.get('/',function(req,res){
    console.log(req,query);
    res.send('GET完成')；
    
})；

app.get('/form',function(req,res){
        res.render('form');
})

// POST
app.post('/', function (req,res){
    console.log(req.body); 					//post 参数需要body方法解析
    res.send('POST 完成');
});


app.listen(3000);


```



## 10. socket连接机制

#### 用原生Node搭建 WebSocket协议的服务 非常麻烦,我们使用写好的模块: Socket.IO

#### 它屏蔽了所有底层细节，让顶层调用非常简单。

#### 并且还为不支持WebSocket协议的浏览器(IE)，提供了长轮询的透明模拟机制。

#### Node的单线程、非阻塞I/O、事件驱动机制，使它非常适合Socket服务器。

```html
<!DOCTYPE html>
<html lang="cn">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <style type="text/css"></style>
</head>
<body>
<h1>我是index页面,我引用的一个script文件</h1>
<hr>
    <button id="btn">提问</button>
    <script src="/socket.io/socket.io.js"></script> !--src地址是socket文件明确给出的 是实现交互的关键    方便下面调用io()--
    <script>
    	var socket = io();		//调用加载文集中io方法
        // 点击发送 发出问题
        document.getElementById('btn').onckick = function(){
            // 发送
           	// 全局发送
            socket.emit('tiwen','你多大')；
            // 监听服务器回答
            socket.on('huida',function(msg){
                alert('服务器说：' + msg)；		// msg指后台传过来的数据；
            })
        }
        
        
        
    </script>	
</body>
</html>
```

```js
// 后台
var http = require('http');
var fs = require('fs');
var server = http.cerateServer(function(req,res){
    if(req.url == '/'){
        fs.readFile('./index.html',function(err,data)){
       		res.end(data);             
       }
    }
});

var io = require('socket.io')(server);			//调用这个方法
	// http://127.0.0.1:3000/socket.io/socket.io.js

// io 监听  接受 服务端发来的消息
io.on('connection',function(socket){			// socket 就是服务器发送的响应数据
    // console.log('有一个用户可连接')；
    // 接受浏览器发来的数据
    socket.on('tiwen',fuunction(msg){
              console.log(msg);				// msg 就是前台发送的数据
    		// socket.emit('huida','你猜！')；		// 非全局响应，也就是不同浏览器都会有同样的效果 不符合聊天功能；
    		io.emit('huida','18~~~~');
       });
});
// 页面监听
server.listen(3000, '127.0.0.1');


// 提一下：
// io 指后台发送或请求
// socket 指前台发送或请求
```

