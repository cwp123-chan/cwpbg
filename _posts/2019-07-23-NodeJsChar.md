---
title: Node.js的特点
date: 2019-07-23 22:46:49
---
# Node.js的特点

## 1.Node.js是让Javascript运行在服务端的开发平台

## 2.Node.js不是一种独立的语言，与PHP Ruby 等“既是语言又是平台“不同，他是使用Javascript编程，运行在Javascript引擎上；

## 与PHP Jsp 不同的是：他跳过了apache ngnix IIS等http服务器

## 他可以不同建立任何服务器软件，其设计理念等和lamp有很大不同，可以提供强大的伸缩功能；

## 3.Node.js特点开销

### 1. 单线程：

#### 		所有客户端都使用一个线程来处理；

#### 		Node.js 不是给每个连接去创建新的线程，而是仅仅使用一个线程来处理；

#### 		单线程带来的好处，减少内存损耗，提高并发量，操作系统完全不再有线程创建和

#### 		开销；

### 2.非阻塞I/O；

#### 		I/O操作不会阻塞程序的运行

#### 		I	Input 输入；

#### 		O	output 输出；

#### 		在阻塞模式下，一个线程只能处理一个任务，想要提高吞吐率，必须多线程；

#### 		非阻塞模式下，一个线程，永远在执行某种运算操作，这个线程的CPU核心利用永远

#### 		是满载的；

### 3.事件驱动；

#### 		客户端请求建立连接，提交数据等行为，就会触发相应的事件；

#### 		在Node中，在一个时刻只能执行 一个事件回调函数，但是在执行一个事件回调函数途中，（比如，又一个用户连接了） 可以转而处理其他事件，然后返回继续执行原事件的回调函数，这种事件机制，称为“事件环”机制；

```
Node.js底层是C++(V8引擎也是C++写的).底层代码中，近半数都用于事件队列、回调函数队列的构建.用事件驱动 来完成服务器的 任务调度,是Node.js中 真正底层核心逻辑.
```

```
单线程
    是为了减少内存的开销,操作系统的内存换页(创建/销毁)
    但是,如果某一个请求 有I/O操作,单线程就会被阻塞了!
非阻塞I/O
    程序不会傻等I/O语句的执行结束,才继续后续代码的运行,而会直接运行后续代码.
    但是,非阻塞就能完美的解决问题吗, 比如 小A的业务执行I/O过程中,有小C要新的请求,此时则么办?
事件驱动(事件环)
    不管是新用户的请求,还是老用户的I/O操作,都将以事件的方式加入到事件环之中,等待调度.
    在node.js中 所有的I/O都是异步的，都是通过回调函数 套 回调函数来使用的；
```



## 4. node.js的优缺点；

### 优点：

#### 适合I/O处理 但是不适合大量的计算操作；

#### 处理高并发

#### 服务器推送

### 缺点：

#### 单线程一旦崩溃整个线程都会崩溃；

#### 服务不是绝对可靠的；

### 适用场景；

```
不能完全替代 传统的后端语言,但在某些方面优于传统.
当应用程序需要处理大量并发的I/O操作，而在发出响应之前，应用程序内部 并不需要进行非常复杂的计算处理的时候，Node.js非常适合。
Node.js也非常适合与web socket配合，开发长连接的实时交互应用程序。

- 聊天室
- 图文直播
- 考试系统
- 收集用户数据的表单
- 提供JSON的API
```

```
搭建Node.js服务器 -- 首个nodejs页面页面

如果修改了程序代码,必须中断当前的node服务,重新在node一次

node.js是服务器的程序,写好的JS代码,都将运行在服务器上.返回给客户端的, 都是已经处理好的结果

node就是一个JS的运行环境

js文件是不能直接拖入浏览器取运行的,必须靠node去执行
```

# 5. 根目录 与 web容器

### 1). Node.js没有根目录的概念，因为它根本没有任何的web容器！

### 2). 静态页面的呈现

#### URL和真实的物理文件之间 是没有关系的。

#### URL是通过 Node 的路由设计之后，才呈现出 某一个静态文件的

```js
var http = require('http');					// 调用http模块
var fs = require（'fs'）					   // 调用fs文件模块
var port = 3000;							// 设置端口
var hostname = '127.0.0.1';					// 设置主机名；
var server = http.createServer(function(req,res){
    if(req.url == '/index.html'){
        fs.readFile('./index.html',function(err,data){
            res.writeHead(200,{'Content-Type':'text/html;charset=utf-8'});
            res.end(data);
        });
    }else if(req.url == '/1.jpg'){						// 需要啥文件就找啥文件
        fs.readFile('./1.jpg',function(err,data){
            res.writeHead(200,{'Content-Type':'image/jpg'});	// 字符集类型也要改变
            res.end(data);
        });
    };
});

server.listen(port,hostname);
```

### 3). HTTP运行原理

#### 由此,我们可以看出,要是使用nodejs 搭建服务器,

#### 是需要写 各种回调函数,定义 各种路由规则,来实现 页面的显示.

#### nodejs中就是 回调函数 套回调函数,每个回调函数 都是一个事件

#### 由此就用到了 nodejs的三个特性:

####     单线程 | 非阻塞I/O | 事件驱动(事件环)

####     URL路由规则,与实际的物理文件,不一定有直接的联系. 

## HTTP模块

### Node.js将各种功能，都划分为了一个个mudule(模块)

### 需要用什么模块,就可以使用require('')来引入使用



# 6. request 和 request (见官网手册)；

```js
// 我们看看require和request之间有啥
var http = require('http');
var port = 3000；
var hostname = '127.0.0.1';
var server = http.createServer(function(req,res){
    console.log(req.url);
    console.log(req.method);
    res.writeHead(200,{'Content-Type':'text/html'});
    res.end();
});
server.listen(port,hostname);
```



Node.js将各种功能，都划分为了一个个mudule(模块)
需要用什么模块,就可以使用require('')来引入使用