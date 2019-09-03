---
title: JQuery基础
date: 2019-08-14 22:46:49
---
# Jquery基础

## 1. 推荐使用的版本

```
jquery版本区别：

         1.3一般功能够

             1.4.2一般功能够而且稳定

            1.7+比较新特性

            2不支持老IE

            兼容的话最好选 1.x。稳定性就用1.7或者1.4，其中1.4的体积相对小

            另外更加情况选用1系列还是2系列，2系列不支持ie6，7，8
```

## 2. 原生和Jquery如何获取页面元素

```
原生:
window.onload = function(ev){
	console.log(ev.target)						// 原生获取页面元素
	var dic = document.getElementById('');		//只有在这个函数{}内写才能获取到页面内容(浏览器后期版本有做调整);
}

$(document).ready(function(ev){
	console.log(ev.target)						//Jquery获取页面元素;
	var $img = $("#img")
	console.log($img);							// 同原生一样
})
```

## 3 . 原生JS 与 Jquery 加载模式的区别

```
原生js 会等到DOM元素加载完毕,且图片等文件加载完毕之后 才会加载js
Jquery 会等到DOM元素加载完毕,但在图片等文件加载完毕之前就 加载 js
```

```
原生js如果多写了几个入口函数 那么后面的则会覆盖原函数
window.onload = function(){
alert('hellow1');
}
window.onload = function(){			// 覆盖hellow1 打印hellow2;
alert('hellow2');
}
Juery如果多写了几个入口函数 那么后面的不会覆盖原函数
$(document).ready({
alert('hellow1')
})
$(document).ready({					// 打印hellow1->helow2
alert('hellow2')
})
```

## 4. jquery 入口函数的四种写法

```
1. 
$(document).ready(function{});
2.
jQuery(document).ready(function(){});
3. 
$(function(){});
4. 
jQuery(function(){})
```

## 5. jQuery 的冲突问题

````
我们1在加载不同的js文件中 在html头文件引入的文件顺序不同 那么他们的冲突优先权也不一样 先引入的优先权更高
如何解决?
1. 释放$的使用权
jQuery.noConflict();
通过noConflict函数 释放$ 那么接下来的函数都不能使用$;改为使用jQuery
注意点:
在释放的时候必须在其他的jQuery代码编写之前释放;
2. 自定义访问符号
var kk = jQuery.noConflict();
kk(function(){
	alert("哈哈");
})
// 这样我们就可以通过自定义的符号代替$以及代替jQuery

````

## 6. 调用jQuery的核心元素

```
$();				// 该用法就是用来调用jQuery核心函数;这边的$ 就代表jQuery
```

## 7. $();可以接收什么类型的参数

```
1. 可以接收一个函数;
$(function(){});
2. 接收字符串
2.1 接收一个字符选择器;
返回一个jquery对象 对象中保留了找到的dom元素
$('.div'){};
var $div = $('.div');
var $div1 = $('#div1');
2.2 接收一个字符串代码片段 他会根据接收到的字符串代码片段判断其属于什么类型的代码 如果没有会创建代码片段
返回一个jquery对象 对象中保留了创建的dom元素
var $p = $("<p>我是</p>");
2.3 接收一个DOM元素
会把接收到的数据包装成一个jquery对象给我们
var $span = $(span)				注意这边不需要接"";
类似于
var span = document.getElementByTagname("span");
```

## 8. jQuery数组的性质

```
实质上输一个伪数组;因为可以通过length获取长度 并且有0-length-1;
```

## 9.静态方法和实例方法

```
//首先我们定义一个类
function Aclass(){

};
// 然后我们在类里添加一个静态方法
Aclass.staticMethod = function(){
	alert("this is staticMethod");
}

// 调用该类下的静态方法
Aclass.staticMethod();

2. 我们添加一个实例方法
Aclass.prototype.instanceMethod = function(){
	alert("instanceMethod");
}

实力方法的调用
// 首先 我们需要 创建一个实例(创建一个对象);
var obj = new Aclass();
obj.instanceMethod();


静态方法和实力方法的区别
静态方法:
是直接添加给类的 并且可以通过类直接调用
实力方法:
是添加给类原型的 必须实例化之后才能够使用;

```

