---
title: 响应式布局
date: 2019-08-20 22:46:49
---
# 响应式布局

## 1. 如何控制屏幕视口不缩放(在响应式布局中必须规定不缩放)

```
<meta name="viewport" content="initial-scale=1.0">					控制屏幕缩放比例为1;
							其他参数:	widtn=device-width			 规定视口宽度=设备宽度;
							其他参数见手册;
```

## 2. 响应式布局的几种导入方式

```
@import url('./screen.css');						1;
<link type="text/css" rel="stylesheet" href="style/demo.css media="screen" >
@media screen {
		...css code;
}
```

## 3. 网格系统

```
规定一个页面被封为12格

*{
padding:0;
margin:0;
}
body{
width:100%;

}
.col-1{
	width:8.33%;
}
.col-2{
	width:16.66%;
}
.col-3{
	width:25%;
}
.
.
.
.
.
.col-12{
	width:100%;
}

//限制盒子模型不允许你padding等
[class*="col-"]{
	box-sizing:border-box;
	padding:10px 10px;					默认padding
	float:left;
}
.row{
width:100%;
}
为了使行和行之间不受影响,所以要清除浮动
.row::after{
	content:"";
	display:block;
	clear:both;
}
```

## 4. 设备各大参数

```min
移动优先:<768
@media (min-width:768px){}	平板
@media (min-width:996px){}	中等屏幕
@media (min-width:1170px){}	大屏幕;

```

## 