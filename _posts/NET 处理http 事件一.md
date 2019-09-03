# NET 处理http 事件一

## server端

```js
var net  = require("net");  //引入net模块
 
var server = net.createServer((sock)=>{
	var address = sock.remoteAddress
	var port = sock.remotePort;		// 建立一个链接 记住他的端口和地址
	sock.setEncoding("utf8");  		// 设置接收的编码
	// sock.setEncoding("hex");		// 将接收的格式转为二进制

});  //创建一个Server对象
 
server.listen('8080','192.168.28.71'); //监听已有的连接
 
//一些事件
server.on('listening',function () {  //服务绑定后触发
    console.log('服务器已启动');
});
 
server.on('connection',function (socket) { //当一个新的连接建立时触发，可接收一个socket对象
    console.log('有新的连接！');
 
    socket.on('data',function (data) {	/// data 默认是Buffer对象，如果你强制设置为utf8,那么底层会先转换成utf8的字符串，传给你  
  										  // hex 底层会把这个Buffer对象转成二进制字符串传给你  
    										// 如果你没有设置任何编码 <Buffer 48 65 6c 6c 6f 57 6f 72 6c 64 21>  
  											  // utf8 --> HelloWorld!!!   hex--> "48656c6c6f576f726c6421"  
        console.log(data.toString());
        socket.write('<h1>你好</h1>');
        socket.write('客户端,请关闭连接~');
    })
 
    // server.close(); //关闭服务
})
 
// server.on('close',function () {  //关闭连接时触发
//     console.log('连接已关闭');

// });
```



## 2.server

```js

var net  = require("net");  //引入net模块
 


var server = net.createServer((sock)=>{
	var address = sock.remoteAddress
	var port = sock.remotePort;		// 建立一个链接 记住他的端口和地址
	sock.setEncoding("utf8");  		// 设置接收的编码
	// sock.setEncoding("hex");		// 将接收的格式转为二进制

	sock.on('data',function (data) {	/// data 默认是Buffer对象，如果你强制设置为utf8,那么底层会先转换成utf8的字符串，传给你  
  										  // hex 底层会把这个Buffer对象转成二进制字符串传给你  
    										// 如果你没有设置任何编码 <Buffer 48 65 6c 6c 6f 57 6f 72 6c 64 21>  
  											  // utf8 --> HelloWorld!!!   hex--> "48656c6c6f576f726c6421"  
        var cs = data.toString().split("\n")[0];
        var csarr = cs.split(" ")[1].split("?")[1].split("&");
        var csList={};
        for(var i=0;i<csarr.length;i++)
        {
        	csList[csarr[i].split("=")[0]] = csarr[i].split("=")[1];
        }

        console.log(csList);
        sock.write('<h1>你好</h1>');
        sock.write('客户端,请关闭连接~');
    })


});  //创建一个Server对象
 
server.listen('8080','192.168.28.71'); //监听已有的连接
 
//一些事件
server.on('listening',function () {  //服务绑定后触发
    console.log('服务器已启动');
});
 
// server.on('connection',function (socket) { //当一个新的连接建立时触发，可接收一个socket对象
//     console.log('有新的连接！');
 
//     socket.on('data',function (data) {	/// data 默认是Buffer对象，如果你强制设置为utf8,那么底层会先转换成utf8的字符串，传给你  
//   										  // hex 底层会把这个Buffer对象转成二进制字符串传给你  
//     										// 如果你没有设置任何编码 <Buffer 48 65 6c 6c 6f 57 6f 72 6c 64 21>  
//   											  // utf8 --> HelloWorld!!!   hex--> "48656c6c6f576f726c6421"  
//         console.log(data.toString());
//         socket.write('<h1>你好</h1>');
//         socket.write('客户端,请关闭连接~');
//     })
 
//     // server.close(); //关闭服务
// })
 
// server.on('close',function () {  //关闭连接时触发
//     console.log('连接已关闭');

// });

```



## client 端

```js

var net  = require("net");  //引入net模块
 
var socket = net.connect('8080','192.168.28.71'); //客户端创建一个连接
 
//失败事件
socket.on('error',function () {
    console.log("连接失败");
})
 
socket.on('connect',function () {   //连接成功时触发
    console.log("连接服务器成功！");
    socket.write("我已接收数据");
 
    socket.on('data',function (data) { //接收到数据的时候触发
        console.log(data.toString());
        socket.end();
    })
})
// socket.on('end',function () {
//     console.log('我已主动关闭连接成功');
// })

```

