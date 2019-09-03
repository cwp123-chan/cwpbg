# Nodejs 文件系统

## 1. 新建文件夹

```
1. var fs = require('fs');							#加载文件
2. fs.mkdir（<address>,function(err){}）
```

## 2. 删除文件夹

```
1. var fs = require（'fs'）;
2. fs.rmdir(<address>,function(err){})
```

## 3. 查看文件状态；

```
1. var fs = require('fs');
2. fs.stat(<address>,function(err,stats){
	if(stats.isDirectory){
		#如果是文件夹
	}
	if(stats.isFile){
		#如果是文件
	}
})			#参数：报错和文件状态
```

## 4. 读取文件夹

```
1. var fs = require('fs');
2.  fs.readdir(<address>,function(err,files){
		# files 以数组形式储存文件名；
})
```

## 5. 读取文件

```
1. var fs = require('fs');
2. fs.readFile(<address>,function(err,data){
		#data 储存的是文件内容
})
```

## 7.迭代器的使用

### 作用：将异步强变同步；

```js
var http = require('http');
var fs = require('fs');
var port = 3000；
var hostname = '127.0.0.1'
var server = createServer(function(req,res){
    // 读取文件夹
    fs.readdir('./img',function(err,files)){
    	console.log(files);
    //定义一个数组用于存储files
     	var getFile = [];
    //带函数名的自执行
    	(function demo(i){
            // 添加宗旨条件
            if(i == files.length)return;
            // 查看文件状态
            fs.stat('./img/' +files[i],function(err,stats){
                // 判断是否为文件夹
                if(stats.isDirectory(files[i]){
                	   getFile.push(files[i]);
                });
            });
    		// 递归；
    		demo(i++);
        })(0);
     };
});
server.listen();
```



## 8. web 容器；

```js
// 1.加载相关模块
// 2. 判断用户有无输入 默认加载index文件；
// 		用path模块获取path文件名
// 3. 读取文件
// 		
// 4.设定mime类型 可以通过加载模块文家获取
var http = require('http');
var fs = require('fs');
var path = require('path');
var port = 3000；
var hostname = '127.0.0.1'
var server = createServer(function(req,res){
        // 跳过了 chrome 的收藏夹图标的请求
            if (req.url == '/favicon.ico') return;

            // 得到用户的路径
            var pathname = url.parse(req.url).pathname;
            // console.log(pathname);
            // 没有指定访问的文件,则默认访问 index
            // 没有指定访问的文件,则默认访问 index
            if (pathname.indexOf('.') == -1) {
                pathname += 'index.html';
            }
	 //  读取访问的文件
    fs.readFile('./static/'+pathname, function(err, data){
        if (err) {
            // 如果文件不存在,就必须显示404
            fs.readFile('./static/404.html', function (err, data){
                res.writeHead(404, {'content-type': 'text/html;charset=utf-8'});
                res.end(data);
            });
            return; // 停止运行输出
        }
    var extname = path.extname();
    
 // 因为文件的MIME类型(扩展名)不同,所以头信息的设置 要动态设置才可以
        // 获取到 实际访问文件的 后缀, 要根据这个后缀,得到对应的MIME类型
        // .html  => text/html  | .png  =>  image/png  | .css
        
        // 响应得到文件的后缀,就需要使用 path 模块
        var extname = path.extname(pathname);
        // console.log(extname);

        /*var mime = getMime(extname);
        res.writeHead(200, {'content-type': mime});
        res.end(data);*/

        // 异步执行
        /*getMime(extname, function (mime){
            res.writeHead(200, {'content-type': mime});
            res.end(data);
        });*/

        // 使用nodejs提供的同步接口实现读取mime 文件
        var mimedata = fs.readFileSync('./mime.json');
        var mime = JSON.parse(mimedata);
        res.writeHead(200, {'content-type': mime[extname]});
        res.end(data);
    });
});

// 运行服务器
server.listen(port, hostname);

/*// 传入后缀,返回对应的MIME类型
function getMime(extname, callback){
    // 读取mime.json文件,用作于后缀与mime类型的匹配
    fs.readFile('./mime.json', function (err, data){
        // console.log(data);
        var mimeJSON = JSON.parse(data);
        // console.log(mimeJSON);
        // console.log(mimeJSON[extname]); //  输出mime类型
        // return mimeJSON[extname];
        callback(mimeJSON[extname]);
    });
}*/

/*// 传入后缀,返回对应的MIME类型
function getMime(extname){
    // 读取mime.json文件,用作于后缀与mime类型的匹配
    fs.readFile('./mime.json', function (err, data){
        // console.log(data);
        var mimeJSON = JSON.parse(data);
        // console.log(mimeJSON);
        // console.log(mimeJSON[extname]); //  输出mime类型
        return mimeJSON[extname];
    });
}*/

/*// 传入后缀,返回对应的MIME类型
function getMime(extname){
    switch (extname) {
        case '.html': return 'text/html'; break;
        case '.jpg': return 'image/jpg'; break;
        case '.png': return 'image/png'; break;
        case '.css': return 'text/css'; break;
        case '.js': return 'application/javascript'; break;
    }
}
*/

```

## 

```js
      // 异步执行
        getMime(extname, function (mime){ 			#function相当于回调函数也相当于匿名函数定义，只有被调用才会i执行
            res.writeHead(200, {'content-type': mime});	#-------------------01
            res.end(data);
        });
        
        /*// 传入后缀,返回对应的MIME类型
    function getMime(extname, callback){				#-------------------02
    // 读取mime.json文件,用作于后缀与mime类型的匹配		# ####这里的callback参数其实就是为了接调用时传入的回调函数而准备的。
    fs.readFile('./mime.json', function (err, data){
        // console.log(data);
        var mimeJSON = JSON.parse(data);
        // console.log(mimeJSON);
        // console.log(mimeJSON[extname]); //  输出mime类型 # ---------------03
        // return mimeJSON[extname];
        callback(mimeJSON[extname]);					#-------------------04
        			#这里的callback（）；相当于 调用时 func回调函数；三
    });
}*/


//异步执行的思路：
/*
安正常的思路来说，我们的程序在加载完后会出现，异步还没加载完就已经程序执行完的操作了 这时我们就得用以上方法，将异步的函数换过来；
思路：
首先我们调用01函数； 遇到function跳过（没有被调用所以不执行）
在执行getMime时，遇到callback回调函数时，我们会跳过他执行下面的fs.readFile函数，我们的参数extname在上面就已经获取到了 因此我们可以执行fs.readFile();函数
接着，只有执行完03时（回调函数的特点），我们才开始执行04；并且将获取到的参数传入callback中；
其实callback()函数就是我们调用getMime时function（）函数；因此；我们调用他
我们把mimeJSON【extname】 参数传到callback中，执行01的function（）；然后就显示出页面；

01（调用）-> 02(只有执行完02才允许执行01的回调函数func) -> 03 ->04 ->01;

*/

```



## 9. 获取IP地址；

```js

var http = require('http');
var fs = require('fs');

var hostname = '127.0.0.1';
// var hostname = '192.168.14.254';
var port = 3000;

console.log(1);

// 创建服务器
var server = http.createServer(function(req, res){
    // 跳过了 chrome 的收藏夹图标的请求
    if (req.url == '/favicon.ico') return;

    // 获取用户的IP
    var userIp = getIp(req);
    // 随机数 1-9
    var num = Math.ceil(Math.random() * 10000 % 9);
    // console.log('欢迎IP: ' + userIp + ' 的用户读取第:[ ' + num + ' ]张图片');

    // 读取图片输出
    fs.readFile('./imgs/'+num+'.jpg', function (err, data){
        if (err) throw err;

        res.writeHead(200, {'content-type': 'image/jpeg'});
        // console.log(userIp + ' 的第[ ' + num + ' ]张图片,已读取完成!');

        res.end(data);
        console.log(2);
    });

    console.log(3);
});

// 运行服务器
server.listen(port, hostname);

console.log(4);


// 获取url请求客户端ip
var getIp = function(req) {
    var ip = req.headers['x-forwarded-for'] ||
        req.ip ||
        req.connection.remoteAddress ||
        req.socket.remoteAddress ||
        req.connection.socket.remoteAddress || '';
    if(ip.split(',').length>0){
        ip = ip.split(',')[0]
    }
    return ip;
};
```








