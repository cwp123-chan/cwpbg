---
title: Es6Let 与Const 的用法
date: 2019-08-28 22:46:49
---

# Let 与Const 的用法;

## 1. let 关键字的用法

```
let a = 100;
var b = 200;

let a = 1000;					不可重复声明
var a = 2000; 					也不可用var重复声明let 定义的变量
```

## 2. 变量提升

```
function demo(){
	console.log(usernmae);
	var username = "是";								存在变量提升		underfind
	let username = "阿萨";							不存在变量提升	   报错
}

如果是let 那么 他就在内部就是临时性死区 
var username = "as";						//同样是报错 因为 在局部变量中以及存在 但是 let会导致临时性死区 所以就会 即产生定义变量 但是 却不存在变量提升 所以 获取不到;
function demo(){
	console.log(usernmae);
	let username = "阿萨";
}
```

## 3. 作用域

```
1. 块状作用域
2. 可以代替闭包的一些功能
for(let i = 0;i<10;i++){
	click[i].click = function(){
		console.log(i)
	}
}

3. if for while else 都会产生块级作用域,并且 var 会向上提升一级作用域,let 不会提升作用域, 不加var 会提升到全局作用域 const不会提升作用域;

if(){	
	var a = 100;
	let b = 200;
}
console.log(a);						可以获取到
console.log(b)						报错 因为 产生了会计作用域
```

## 4 .const 常量

```
1.  常量存在作用域
if(){	
	const a = 100;
}
console.log(a)						报错 因为 产生了会计作用域


2.  常量的值一经声明就不能改变;


3. 常量作用域有 全局,局部,和块状
	

```

## 5 .let 声明的顶层对象

```
let co = 200;
console.log(window.co);					underfined;
										// let和const,声明的变量不属于window的顶层变量
										但,var 等声明的就是window 的顶层变量;
```

## 6. es6 的顶层对象

```
在Es6中 已经去掉了顶层对象的概念
为了向下兼容,在全局作用域中,用var声明的变量和直接声明的函数 同样都属于window顶层对象的属性和方法;
而使用const 和 let 声明的变量 不再属于顶层对象;
```



## 7 . Es6 的解构赋值

### 1. 数组的结构赋值

```
let [a,b,c] = [10,12,100]
console.log(a,b,c);

复杂数组的模式匹配;
let [aa,[[bb],[cc,dd]]] = [11,[[22],[33,44]]];				只要保证两边模式一样

let [a,,b] = [1,2,3];console.log(a,b)						// 1 3;
let [a,d,b] = [1,2];console.log(a,d,b)		// 	1 2 undefind

let [a,b="嗷嗷啊"] = [0] console.log(a,b)		// 0 嗷嗷啊

let [a="111",b] = [0] console.log(a,b)	// 111 underfind			会按照顺序进行赋值;
```

## 2. 对象的解构赋值

```
let {} = {};
完整:
let {a:a,b:b} = {a:100,b:200};
let {a:nub1,b:nub2} = {a:100,b:200};		console.log(nub1,nub2)		// 100 200
简写
let {a,b} = {a:100,b:200}



复杂的对象解构赋值
let obj = {p:[
	"Hello",					
	{x:"World"}
]}								//只要保证两边模式相同就ok


let {p:[a,{x:b}]} = obj;
console.log(a,b);
```

## 3. 特殊对象的解构赋值

```
1 字符串可以当做字符组成的数组;
let[a,b,c,d,e] = "hello";
console.log(a,b,c,d,e)					//h e l l o;
```

## 4. 对象中特殊的解构函数

```
let {length:len} = "world"		
console.log(len);  // 5					
let {PI:p} = Math;   							
Math对象里面有方法,把Math里的方法提取出来变成一个变量 然后 解析这个方法 把这个方法的值给p						
console.log(p)		// 3.1415926...				 
因为 定义变量中的length和PI都是特殊方法 所以 定义的变量会将 右边的值当做特殊方法的值,然后得出 执行后的值 ;

总结; 判断右边的值是什么对象 如果 那个对象有左边的变量中的方法 那么就将右边的值作为左边的方法的参数 执行方法 然后再将执行完毕后的值赋给自定义变量

let {length}='lxy';
       console.log(length);//3						用解构的方式获取到字符串的长度
```

## 5 .解构赋值的实际应用

```
1.
let a = 200;
let b = 300;
[a,b] = [b,a]			数值交换
2. 提取JSon数据中的值

let jsonData = {
	id:42,
	status:"OK",
	data:[12,3,12,1,2,3]
}

let {id,status,data} = jsonData;		这样可以一部全部提取 不需要进行一个一个提取了;

3. 在ajax中的默认值
	function ajax({
		url,
		type = "get",
		data
	}){
		console.log(url,type,data)
	}
	
	ajax({
		url:"./1,php",
		data:"hello world"
	})
	
// ./1.php get hello world				可以设置ajax默认值
4. 在 ES6 中 导入模板必须用 解构赋值
model.js文件:
	export var filename = "file";
	export var user = "liwei";
	export var age = "18";
导入文件
<script type="module">
	import {filename,user,age} from "./model.js"
	console.log(filename,user,age);
<script>
```

## 6. for of 

```
var arr = ["helle","my","friend"];

for (var val of arr){
	console.log(val);					 // 遍历出 数组值
	
}
for (var i in arr){
	console.log(i)						// 遍历出数组的键
}
```

```
2. 可以遍历那些类型
1. array
2. string
for (var val of "asdasdasdasda"){
	console.log(val);					 // 遍历出 字符串
	
}
3. 页面的袁术列表
<body>
	ul>
		li>
		li>
		li>
		li>
	ul>
</body>
<script>
	var lis = documennt.querySelectorAll("ul li");
	for (var list of lis){
	console.log(list);
	}
</script>

4. arguments
5. Map
6. Set
7. 实现遍历器接口的数据类型 都可以使用 forof
```

## 7. for of 与 for in以及forEach的比较

```
forof 可以遍历的类型更多;
forof 可以得到遍历的值 而 forin 得到的只是 遍历的键
foreach 可以遍历更多的类型 并且 可以使用 break; continue等结束语句

	var arr = ["da","asd","as"];
	arr.forEach(function(){
		break;/continue;
	})
	
```

## 8. 模板字符串

```
我们使用一段`` 反引号来加载 html标签 并且可以具有可读性以及 复杂的字符串
var html = `<ul>
				<li></li>
				<li></li>				
			</ul>`;
console.log(html);
也可在 模板字符串中插入变量
var html = `<ul>
				<li>${number}</li>
				<li></li>				
			</ul>`;
console.log(html);
```

## 9. es6 新增 repeat()方法 代表重复

```
var a = "Hello";
a.repeat(4);
console.log(a.repeat(4));					// HelloHelloHelloHello;
```

## 10. 字符串补全长度方法

```
var a = "Hello";
a.padStart(10)					//	在字符串最前补充空格 使得总长为10
a.padStart(10,"*")				//  在字符最前补充*


a.padEnd(10)					//	在字符串最后补充空格 使得总长为10
a.padEnd(10,"*")				//  在字符最后补充*
```

## 11 .字符串包含验证

```
var str = "Hello World";
一般方法
console.log(str.indexOf("Hello"))				//返回"Hello"的位置  =>0
console.log(str.indexOf("0"))					//返回"0"的位置  =>-1


Es6
console.log(str.includes("Hello"))				//true
console.log(str.includes("88"))					//false


// 判断字符串是否以指定字符串开头
console.log(str.startsWith("Hello"))			// true 是以 Hello 开头
console.log(str.startsWith("lo"))				// false 不是以 lo 开头

// 判断字符串是否以指定字符串结尾 
console.log(str.endsWith("World"))				// true 是以 World 结尾
console.log(str.endsWidth("lo"))				// false 不是以 lo 结尾
```

## 12. 函数的默认值(Es6)

```
function (a,b="默认值"){

}
如果只赋值一个值 那么从第一个参数到最后一个形参开始赋值 遵循从前往后赋值;
```

