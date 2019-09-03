---
category: Stuff
path: '/stuff'
title: 'nodejs中ejs的运用'
type: 'GET'

layout: nil
---

# nodejs中ejs的运用

## 1.ejs 基本用法

```
<%= var %>			在ejs文件中 用于填写后台数据
获取模板；
fs.readfile 读取ejs文档；
获取数据；
var massage = {
var1:'内容'，
var2:'内容'
}
实现模板连接
var datas = ejs.render(模板data，数据massage）；
res.writeHead(200, {'content-type': 'text/html'});
res.end(datas);

```

## 2.nodejs 数据库连接：

```
sudo cnpm install mysql
var mysql = require('mysql')
// 创建连接
var connection = mysql.createConnection({
         		host: '127.0.0.1',       //主机
                user: 'root',            //MySQL认证用户名
                password: 'cwp199811',
                port: '3306',
                database: 'info'
});

// 建立连接
connection.connect(function(err){
	if(err){
	console.log('错误：'+err)；
	return；
	}	
})；
 var massage = `'${fields.massage}' `;
 var pwd = `'${fields.pwd}'`;
 var msql = "select * from user where name= " + massage + "and pwd=" + pwd;
 connection.query(sql,function(err,result){
   str = JSON.stringify(result);    // 将数据处理；
    })；
    
    connection.end();//断开；
    
    
    //如果需要读到数据；
    var data = str[0].字段名；				//他是一个对象数组
```

