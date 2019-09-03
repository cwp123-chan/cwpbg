---
title: jQuery 静态方法
date: 2019-08-14 21:46:49
---

# jQuery 静态方法

## 1. forEach();each();

```
使用:
var arr = [1,2,3,5,6,7,8,3];				数组;
var obj = {0:1,1:2,2:3,3:5,4:6,length=5}	伪数组
arr.forEach(function(value,index){})		原生js
value--------遍历到的元素
index--------当前元素的索引;
注意点:
原生的forEach 只能遍历数组 不能遍历伪数组

// jQuery中 each 方法遍历伪数组和原生数组
$.each(arr,function(index,value){});		arr为数组
第一个元素 :当前遍历到的索引;
第二个元素 :当前遍历到的元素						//与forEach 顺序相反;

遍历伪数组
$.each(obj,function(indedx,value){})		// 可以遍历伪数组

```

## 2. map 静态方法

```
使用:
var arr = [1,2,3,5,6,7,8,3];				数组;
var obj = {0:1,1:2,2:3,3:5,4:6,length=5}	伪数组
arr.map(value,index,array){};
第一个参数: 当前数组的元素
第二个参数: 当前数组的索引;
第三个参数; 被遍历到的数组;
obj.map(value,index,array){};				// 不能遍历伪数组

// jQuery方法
$.map(arr,function(index,value){})	// 可以遍历数组
第一个参数: 要遍历的数组
后有一个回调函数;
第二个参数: 遍历到的元素;
第三个参宿: 遍历到的索引						// 顺序和原生相同 不同于each参数;
$.map(obj,function(index,value){});
```

## 3. jQuery中的each和map方法区别

```
1.
each 静态方法的返回值是 遍历谁 返回谁
map 静态方法的返回值是一个空数组
2. 
each 静态方法 不支持回调的函数中对相应的数组做加工处理 也不能加工返回
map 静态方法 支持回到函数中对相应数据做加工处理并且可以return回来;
```

## 4. trim();去空白函数

### jQery中 函数处理过得数据都必须接收其返回值 才能使用!!!

```
///首先 先声明一个字符串
var str = "  你好  ";
var strS = $.trim(str);					jQery中 函数处理过得数据都必须接收其返回值 才能使用
console.log('----'+str+'----'')							// ----   你好  ----
console.log('----'+strS+'----'')						// ----你好----

// $.trim()作用
去除字符串两端空格
参数: 需要去除的字符串
返回值: 去除完毕之后的字符串;
```

## 5. iswindow

```
作用: 判断传入的对象是否是window对象
返回值 boolearn;
var arr = [1,2,3,4,4]	真数组							false
var arrL = {0:1,1:2,2:3,3:4,4:4,length=5}	伪数组		false
var obj = {"demo":"hellow","name"="ke"}		对象		false
var demo = function(){}						函数		false
var w = window;								window对象	true;
$.isWindow(W) 								判断是否为window对象
```

## 6. isArray();

```
作用:
判断传入数据是否为真数组;会明确判断真伪数组
$.isArray(arr)		true;

```

## 7. isFunction();

```
作用:
判断传入数据是否为一个函数
$.isFunction(demo)				true
$.isFunction(jQuery)			true
注意点:
jQuery 框架本质上是一个函数;
```

## 8. holdReady 暂停和恢复ready事件

#### 在许多开发情况下 我们刚进入页面 需要等其他js加载完后才执行自动加载onload函数内的方法,因此我们使用holdReady(true)来暂停自动加载

````
<head>
<script>
$.holdReady(true)					head标签中我们在自动加载的方法上面暂停自动加载
$(document).raedy(function(){
	alert('自动加载')
})

</script>
</head>


<script>
等加载完毕下面js后
$.holdReady(flase);					开始执行上面ready中的方法;
</script>
````

