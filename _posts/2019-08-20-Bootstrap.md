---
title: bootstrap
date: 2019-08-20 22:46:49
---
# bootstrap

## 1. 文件导入

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <!--兼容模式-->
    <meta http-equiv="X-UA-Compatible" content="IE=Edge">
    <title>Document</title>
    <link rel="stylesheet" href="./bootstrap-3.3.7-dist/css/bootstrap.min.css">
    <!-- [if lte IE 8]>
            <script src="./html5shiv/dist/html5shiv.min.js">
            <script src="./Respond-master/src/respond.js"></script>
    <![endif]-->
    <style>
        [class*="col-"]{
            border:1px solid #ccc;
            /*padding:20px;*/
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="page-header">
            <h1>bootstrap <small>一个框架</small></h1>         
        </div>
        <div class="row">
            <div class="col-md-3">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores eligendi </div>
             
             <div class="col-md-3">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores eligendi </div>
             
             <div class="col-md-3">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores eligendi </div>
            
             <div class="col-md-3">Lorem ipsum dolor sit amet, consectetur adipisicing elit. Dolores eligendi </div>
             
        </div>
        <div class="row">
            <!-- <div class="col-xs-2 col-sm-3 col-md-4 col-lg-6 col-md-offset-2">Lorem ipsum dolor sit amet</div> -->
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
            <div class="col-xs-2  col-sm-3 col-sm-offset-2 col-md-4 col-lg-6">Lorem ipsum dolor sit amet</div>
        </div>
    </div>

    <script src="./jquery/dist/jquery.min.js"></script>
    <script src="./bootstrap-3.3.7-dist/js/bootstrap.min.js"></script>
</body>
</html>
```

## 2. css样式 class名

```
page-header					标题
container					进行删个系统布局
row							进行删格排版
col-xs-1					小屏幕
col-sm-1					中等屏幕
col-md-1					电脑屏幕
col-lg-1					大屏幕
col-md-offset-1				列整体偏移
col-md-push-1				单个偏移(右)
col-md-pull-1				单个偏移(左)


```

## 3. 表格

```
在div后加table	就为 table样式
table-bordered				有边框
table-hover					鼠标悬浮有颜色
table-striped				奇偶条纹;
table-condensed				紧凑型表格 上下内编剧减小;
响应式表格
table-responsive			当视图小于768时,自动参生横线滚动条不影响显示数据
							生效条件:数据必须有一定的量
							
表格状态类
.active						访问时状态
.danger						
.info						
.warning					错误状态
.success					成功状态

```

## 4. 表单(具体见官方手册)

```
1. div后加 form-group
form-control				表单样式	适合于输入框
btn btn-default btn-lg		大按钮 提交框
2. div后面加checkbox				多选框
3. 内联式表单
form-inline					表单水平排列;
4. form表单的删格系统
form-horizontal				将删格运用到表单内部(结构一样)

```

```html
<form action="#" method="get">
	<div class="form-group">
        <label for="input">邮箱:</label><input type="text" class="form-controls" id="input">
    </div>
    <div class="form-group">
        <label for="in">username:</label><input type="text" class="form-controls" id="in">
    </div>
</form>
```

