---
title: jQuery 事件	
date: 2019-08-23 22:46:49
---
# jQuery 事件	

## 1 . 事件绑定

```
1. on	
$("#idName").on("click",function(){})						自定义事件绑定
$("#idName").on("click",p,function(){
		$(this).slideToggle();								可以绑定未创建的元素 
															#idName与p 必须是父子级,this指的p
															当点击时未创建的元素有滑动效果
});
$("#idName").on("click",{msg:"你好啊"},function(event){
		alert(event.data.msg);								绑定时传递参数
})

$("#idName").on("mouseover click",function(){})				多个函数绑定同一个事件
$("#idName").on({
	mouseover:function(){...},
	mousemove:function(){...},
	click:function(){...}										多个函数绑定多个方法
});

也可以如下绑定多个方法

$("#idName").on({
	mouseover:function(){},
	mousemove:function(){},
	click:function(){}										多个函数绑定多个方法
},fun1,fun2,fun3);



2. 自定义绑定
$(".className").click(fun(){});								自定义事件绑定
$(".className").click(fun(){
	$("<p>neirong</p>").insertAfter(".className")			绑定未创建标签
})

3. 事件的移除
$(".className").off("click")				移除所有事件
$("./className").off("click",funcName)		移除指定方法事件;

```

## 2. 事件冒泡

```
$(".son").on("click",function(event){
	alert("das");
	event.proPagation();									阻止事件冒泡
	evevt.preventDefault();									阻止默认事件
})
```

## 3. 事件自动触发(trigger,triggerHandler)

```
$(".asd").trigger("click");									自动触发 会事件冒泡和默认行为
$("#className").triggerHandler("click");					自动触发 不会事件冒泡和默认行为


对于a标签时(具体需要默认行为时;)
需要再a标签内加span(或别的)标签同时对 span绑定Trigger事件
原理: tRigger会事件冒泡和默认行为
绑定a子级的span 触发事件冒泡后 就自动开始 默认行为 ,这样就可以达到自动执行a的点击事件后 也能触发a的默认行为
```

## 4 . 事件自动触发自定义事件

```
$('.idNmae').on("myClick",function(){
		alert('as');						可以触发 因为 on可以触发如何事件;
})
$(".idName").trigger("myClick");


$('.idNmae').myClick(funtion(){
		alert('as');					事件触发不了,因为 event里没有myClick自定义事件
})
$(".idName").trigger("myClick");
```

## 5. 事件的命名空间

```
无论自定义事件还是默认事件时 为了避免 重复事件可使用命名空间

$("#idName").on("click.zs",function(){})					点击事件后可以加命名 用于区别;对结果没有影响
$("#idName").on("click.ls",function(){})	

自动触发指定事件
$("#idName").trigger("click.zs");						只触发zs的事件
```

```
自动触发需要注意的地方
$("#father").on("click.zs",function(){});
$("#son").on("click.zs",function(){});	
$("#son").trigger("click.zs")


当相同命名空间事件被触发时,如果他的父级的命名空间相同,那么父级也会触发这个事件
因为Trigger会执行冒泡行为;父元素如果没有 就不会粗发

$("#son").trigger("click")
如果子元素触发不带命名空间的元素,那么同事件子元素和父元素,都会被触发
```

## 6 . 事件的委托

```
ul
	li-li
	li-li
ul

$("ul>li").click(function(){
	console.log($(this).html())						this指代被点击的li
													这样li会依次遍历加载好的li 但是如果之后新增													的话 那么就绑定不了了 所以就要事件委托
})


$("ul").delegate("li","click",function(){
	alert(this);									委托ul为li绑定所有的点击事件 包括页面加载	完														毕	之后新增的
})


```

## 7 .this	与 $(this的区别)

```
如果在绑定事件之后 使用this 那么 this 就是指的被绑定的事件所对应的元素 而 $(this)得到的是jQuery对象
因此需要$(this).context;获取到被绑定的事件所对应的元素

$("ul").delegate("li","click",function(){			事件委托下
			console.log(this);					返回的是点击的li
			console.log($(this));				返回的是点击的li对应的obj对象
})

$("ul").on("click",function(){
					console.log(this);				返回的是点击的ul
					console.log($(this));			返回的是点击的ul对应的obj对象
})


```

## 8 . jQuery事件 一入一出

```
mouseover
mouseout						鼠标移入移除会触发父元素事件
mouseenter
mouseleave						鼠标移入移除不会触发父元素事件


$(".ClassName").hover(fun1(){},fun2(){});	第一个参数 一移入事件,第二个参数 移除事件 并且不会触发父元素事件;
当只有一个参数时 也可以即触发移入,也可以触发移除 并且 执行同一个方法;
```

## 9  . 获取页面元素标签的集中发誓

```
$(".idName").eq();				会获取到 对应元素的jQuery对象
$(".idName").get();				直接获取当前元素;
```

## 10 .获取当前元素下标和其他元素的方法 index()与siblings();

````
$("#ccc").on("mouseenter",function(){
	$(this).index();							获取当前鼠标移入的元素的下标	
})

$("#ccc").on("mouseenter",function(){
	$(this).siblings();							获除自己外 当前元素同等级的其他兄弟元素
})
````

## 11 . 简单的轮播图

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script src="./jquery-1.7.2/jquery.min.js"></script>
    <style>
    .box{
        border:1px solid;
    }
        .container{
            /*display:none;*/
            margin-left:400px;
            list-style:none;
        }
        .container li{
            display:none;
        }
        
        .container>.show{
            display:block;          /*权重问题*/
        }
        .nav>li{
            border:1px solid;
            width:100px;
            float: left;
            list-style:none;
        }
        .current{
            background: yellow;
        }
    </style>
</head>
<body>
    <div class="box">
        <ul class="nav">
            <li class="current">1</li>
            <li>2</li>
            <li>3</li>
            <li>4</li>
        </ul>
        <ul class="container">
            <li class="show"><img src="./img/1(1).jpg" alt="" width="60%" ></li>
            <li ><img src="./img/1(2).jpg" alt="" width="60%"></li>
            <li ><img src="./img/1(3).jpg" alt="" width="60%"></li>
            <li ><img src="./img/1(4).jpg" alt="" width="60%"></li>
        </ul>
         

    </div>
</body>
    <script>
        $(".nav>li").mouseenter(function(){
            var $count = $(this).index();
            $(this).addClass("current");
            $(this).siblings().removeClass("current");
            var $index = $(".container>li").eq($count);   //      获取 鼠标所在位置下,所对应的的图片 需要eq来获取
            $index.addClass("show");
            $index.siblings().removeClass("show")
            console.log($index);
        })
    </script>
</html>
```

