---
title: Js 大纲
date: 2019-07-05 22:46:49
---
# 回顾

1. 获取页面中的元素

2. 改变元素的属性以及 css 属性

3. 定时

4. js 的函数

- 函数的声明
- js 函数特点
- js 的参数
- js 函数调用
- 作用域 及 作用域链
- 自执行 与 闭包

----

# JS 对象

## 创建对象

- Object 构造函数
    var obj = new Obejct();
- JSON 方式  (推荐)
    var obj = {p:v, p:v, funName:function (){}}
- 自定义构造函数方式
    function 函数名(){ this... }
    var obj = new 函数名();
- 匿名构造函数方式
    var obj = new function (){ this ... }


## 测试属性

所有的对象都有构造函数，
可以通过 `constructor` 属性，获取某个对象的构造函数。


## 原型 prototype

一般用于给一个构造函数，添加属性或方法.

作用：
1. 模拟对象的继承性,扩展功能
2. 给系统内置对象 添加或修改方法


## 删除对象中的成员 delete

delete obj.name; // 删除属性
delete obj.funName; // 删除方法

----

# JS 数组

## 声明数组

- 1 构造函数
    - 空数组 var list = new Array();
    - 指定数组长度 var list = new Array(length);
    - 指定数组初始值 var list = new Array(v1,v2,v....);
- 2 JSON 方式
    - 空数组 var list = [];
    - 指定数组初始值 var list = [v1, v2, v3...]


## 数组特点

- 不能使用 [] 追加数组元素, 使用 push()
- JS数组连续的, 跳跃着赋值,中间的元素,都会赋值为undefined
- JS 只有索引数组,没有关联数组

## 遍历

- for
- for in



## 数组 属性


## 数组 方法

- 不改变原数组的方法:
- 改变原数组的方法:

----

# JS 内置对象

1. Boolean
2. Number
3. String
4. RegExp
5. Date
6. Math
7. Array
