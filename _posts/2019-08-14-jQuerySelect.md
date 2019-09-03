---
title: jQuery 相关选择器以及属性节点
date: 2019-08-14 22:16:49
---
# jQuery 相关选择器以及属性节点

## 1. :empty

```
var $div = $("div:empty")				他会获取到既没有文本内容,也没有子元素的指定元素
```

## 2. :parent

```
var $div = $("div:parent");				找到有文本内容或有子元素的指定元素
```

## 3. :contains

```
var $div = $("div:contains("我是特例")")	找到有文本内容并且包含参数中的内容的元素;也就是相当于模糊查询;
```

## 4. :has

```
var $div = $("div:has(span)")			找到元素中包含span标签的相关元素
```

## 6. 什么是属性

#### 对象身上保存的变量就是属性;

## 7. 如何操作

#### 对象.属性()	调用

#### 对象.属性 = "";	赋值

#### 对象["属性名"] = "属性值"; 赋值

## 8. 什么事属性节点

```
在编写HTML中 在我们标签中添加的属性 就是属性节点  如: name="" type=""...
在浏览器器中 我们找到span这个 DOM元素之后 展开 看到的都是属性
在attributes中保存的都是属性节点;
```

## 9. 如何设置属性节点

```
获取节点
var span = document.getElementByTagnem("span")[0];
span.setAttribute("name","力"); 第一个参数 属性名
								第二个参数 属性值;
span.getAttribute("name")		获取属性值;
```

## 10.jQuery操作属性节点

### 标签中属性和属性节点值的区别  前者是设置标签属性的 后者是设置标签里面属性节点的属性的;

```
1.  attr(name|pro|key,val|fn);
作用: 
获取或设置属性节点的值;
传递一个参数: 获取属性节点的值
传递两个参数: 设置属性节点的值
注意点:
无论找到多少个属性节点 都只会返回第一个属性节点的值
无论找到多少个属性节点 都会设置每一个属性值
如果 设置不存在的属性节点的值 那么系统会新增属性节点与属性值;

// 获取属性值
$("div").attr("span")				返回 div下span属性节点的值
$("div").attr("span","ggg")			设置属性节点的值
```

```
2. removeAttr(name)
作用:
删除所有找到的属性节点
$("div").remove("class name")		如果需要删除多个属性节点 只要在每个属性节点之间用空格隔开就行了

```

```
3. prop();
作用 同attr一样 但既可以操作属性节点(浏览器器中 我们找到span这个 DOM元素之后 展开 看到的都是属性) 也可以操作属性节点(在attributes中保存的都是属性节点) 但是用法与attr一样

removeprop();同removeAttr()一样
关键点:
官方推荐 在标签中 需要判断属性值是否为true/flase时 如 disable/slected...的我们推荐使用prop 因为
如果prop可以获取到属性值 那么就返回true 但attr返回的是值的名称 其他需要名称的地方 使用attr();
```

## 11. 添加class 删除class 切换class

```
addclass(classname);			如果添加多个类中间空格隔开
$('div').addclass("box1 box2 box3")

removeclass(classname);			同上

toggleclass(classname)		切换类 如果有就删除 如果没有 接添加
```

## 12. html/text/val

```
添加html标签内容
$("div").html("<p>我是p</p>")		添加html	就相当于innerHTML
$("div").html()				获取html内容  	就相当于innerHTML
$("div").text("我是p")		添加text	就相当于innerText
$("div").text()				获取text内容  	就相当于innerText
val();
同上 都是设置value值和 获取value值的

```

