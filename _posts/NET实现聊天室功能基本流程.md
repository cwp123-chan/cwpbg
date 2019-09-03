# NET实现聊天室功能基本流程

## 1.步骤

* ### createServer 实现基本的服务器端的链接创建

* ### socket.on 获取客户端/服务端的响应

* ### socket.write 传输客户端/服务端响应代码

* ### socket.listen 监听ip与端口

* ### socket.on（‘data’,function（data）{}） 获取数据

* ### 遍历所有用户下的socket.write 群发

* ### 获取单个用户下的socket.write私聊

* ### process.stdin.setEncoding('utf8'); 设置接收代码格式

* ### process.stdin.on('readable', callback）接收命令行的数据

* ### const chunk = process.stdin.read(); 读取命令行数据

* ### process.stdout.write('输入的数据是：'+chunk); 打印命令行数据

* ### process.stdin.resume();旧模式 让命令行可以继续可写；

  ## 2. 注意事项

  * ### 每次发送数据时 if语句会重复判断 通过参数即可修复

  * ### 将用户与socket连接绑定才可以实现每次遍历群聊

  * ### 需要每次制定协议，已达到不一样的指令执行

  ## 3. 服务端代码；

  #### 注意：在实际中需要进行心跳，以防止运营商网络超时断开，具体方法见2019.8.1号net的代码，这里不做演示；

  ```js
  const net = require('net');
  const port = 8080;
  const hostname = '192.168.28.71';
  const server = net.createServer();
  server.listen(port,hostname);
  var exits = '退出命令【exit】';
  var userList = {};
  var userLists = [];
  var userArry = [];
  var stats = 0
  
  server.on('connection',function(socket){
  	// 设置状态码 避免重复出现操作循环添加数组
  	stats =1;
  	// 添加socket用户广播
  	userLists.push(socket);
  	// 获取当前链接人数
  	userArry.push('username');
  	console.log('链接成功! 已有'+userArry.length+'登陆成功！' + exits+'\n');
  	// 当服务器链接成功接收端口和127
  	const port = socket.remotePort;
  	const address = socket.remoteAddress;
  
  	var ip = socket.remoteAddress;
  	var userPort = socket.remotePort;
  	socket.write('user:'+'请输入您的用户名' + exits+'\n' + '-----------------INSRER INTO--------------------');
  
  	socket.on('data',function(data){
  		// 这里接收三类数据 分别判断其头文件协议指代的聊天模式
  		const datas = data.toString();
  		// 清屏
  		console.log('\033[2J');
  		// 拆分标识
  		const mark = datas.split('\/')[0];
  		const user = datas.split('\/')[1];
  		// console.log("接收的数据"+user);
  		// console.log("接收的全部数据"+datas);
  		// console.log("第二次："+user);
  		// return false;
  		// 当服务器断开时执行
  		socket.on('close',function(){
  			console.log('与客户端主动断开连接......');
  		})
  		// 设置命令行输入代码 输出代码获取行内容
  		 process.stdin.setEncoding('utf8');
  			process.stdin.on('readable', () => {
  			  const chunk = process.stdin.read();
  			  if(chunk == 'exit\n'){
  	 			 	socket.end();
  	 			}
  		 })
  
  			// 如果前台是user且为第一次登录 那么记录其用户名
  		if (mark == 'user' && stats == '1') {
  			// admin.push(ip+','+user);
  			username = user.split('\n')[0];
  			stats =0;
  			inuser(ip,user,socket);
  		}	
  			// 如果是执行断开操作，那么删除统计数量数组最后一个值
  		if(mark == 'end'){
  			userArry.pop();
  		}
  			// 如果前台是all/。。。 那么执行此操作
  		if(user == 'all'){
  			var msg = datas.split('\/')[2];
  			console.log('所有人:'+msg);
  			// 利用循环遍历广播
  			for(var i = 0;i<userLists.length;i++){
  				userLists[i].write(socket.name+'对所有人说:'+msg);
  			}
  
  		}
  		// 如果前台是@/用户名/。。。 那么执行此操作
  		if(user == '@'){
  			var msg = datas.split('\/')[2]+'\n';
  			var mass = datas.split('\/')[3];
  			console.log(user);
  			console.log(msg);
  			console.log(userList[msg])
  			// 如果有这个人
  			if(userList[msg]){
  				console.log(socket)
  				userList[msg].write(socket.name+'对你说:'+mass);
  
  			}else{
  				socket.write('sorry哦O(∩_∩)O 没这个人');
  			}
  		}
  	})
  
  });
  
  	// 执行方法
  function inuser(ip,user,socket){
  	// 如果是user 则将用户名与socket绑定成对象
  		//插入对象;
  		// 将用户输入的用户名储存在socket.name对象方法内，避免别的终端启动后覆盖原值
  		socket.name = user;
  		// console.log(socket.name);
  		userList[user] = socket;
  		// console.log(userList);
  		socket.write("talk:所有人广播【all/】\n 私聊【@/好友名/】");
  }
  
  
  
  
  ```

  

## 4. 客户端

```js
var net = require("net");
var usered = 'user/';
var talked = 'talk/';
var alled = 'all/';
//输入输出,这里没用到
var out = process.stdout;
var cin = process.stdin;
var socket = net.connect('8080','192.168.28.71');

socket.on('error',function(){
	console.log("链接失败，请检查网络......");
})

socket.on('connect',function(){
	// console.log("服务器链接成功")
	
	// 当服务器链接成功接收端口和127
	const port = socket.remotePort;
	const address = socket.remoteAddress;
	// 监听服务器传回数据
	socket.on('data',function(data){
		var result = data.toString();
		var user = result.split('\:')[0];
		// 打印服务器结果
		console.log(result);
		// 如果头信息为user着输入用户名
		if (user == 'user') {
			users(result);
			// console.log("接受的user数据是"+result);
		}
		
	})
	
})

socket.on('close',function(){
	console.log('与服务器断开连接......');
})

function users(result){
	// 接收数据以及命令行数据
	process.stdin.setEncoding('utf8');
		process.stdin.on('readable', () => {
	  const chunk = process.stdin.read();
	  // exit 退出操作;
	  if(chunk == 'exit\n'){
	  	socket.write('end/');
	  	socket.end();
	  }
	  if (chunk !== null) {
	    process.stdout.write('输入的数据是：'+chunk);
	    // 清屏
		console.log('\033[2J');
	    socket.write('user/'+chunk)
	  }

	   process.stdin.resume();
	});
}



			// console.log('\033[2J');

```

