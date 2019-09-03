---
title: jQuery 尺寸设置以及事件绑定
date: 2019-08-15 22:46:49
---
# jQuery 尺寸设置以及事件绑定

## 1. 尺寸的操作;

```
offset();
$(".div").offset().left;							获取左窗口偏移量
$(".div").offset().left("100px");					设置偏移量


position();
$(".div").offset().position;						获取父级相对位置
													注意.position不能设置相对位置
													
连续操作;
$(".div").css({
width:"100px",
height:"100px"					
})
```

## 2. 获取和设置滚动的偏移量

```
scrollTop();
$(function(){
	$(".div").scrollTop();							获取div滚动条的偏移量
	$(".div").scrollTop(300);						设置div滚动条的偏移量 不要加px
	$("html").scrollTop();							获取网页滚动条的偏移量
	$("html").scrollTop(300);						设置网页滚动条的偏移量 不要加px
})


注意点 为了兼容浏览器 我们通常这样获取网页偏移量
$("html").scrollTop()+$("body").scrollTop;							获取网页滚动条的偏移量
$("html,body").scrollTop(400)										设置网页滚动条的偏移量
```

## 3. 事件绑定

```
jQuery事件绑定的两种方法
1. $("div").click(function(){});					效率高 但部分事件不支持 不会重复覆盖事件
2. $("div").on("click",function(){})				效率低 但js事件全部支持 不会覆盖事件
```

## 4. 事件移除

```
off();
1. $("div").off();									不传参// 移除所事件
2. $("div").off("click");							如果传递一个参数 那么所有该事件都被移除
3. $("div").off("事件Name",funcName)				   如果传递两个参数,那么会移除对应的事件;(必须使用构造函数的方法 给予方法名)

```

