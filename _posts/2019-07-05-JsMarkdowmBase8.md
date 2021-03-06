---
title: Js 大纲
date: 2013-12-26 22:46:49
---
# 回顾

1. 在 HTML 中使用 JS

2. JS 基本语法

3. JS 的变量

4. JS 的数据类型

```
基础部分: String   Number     Boolean
复合类型: Object   Array
特殊类型: null     undefined  function
```

5. Number 类型

6. String 类型

5. 数据类型的转换

    1. 显式转换(强制)

    2. 隐式转换(自动)

6. 正/负 无穷大，精度问题

----

# 扩展


1. 获取页面中的一个元素

    document.getElementById()
    一定要写在元素生成之后,或者可以写在函数中,使用事件...

2. 获取或设置元素的 css 属性

    element.style.color
    element.style.color = 'red';


3. 获取或设置元素 标签的属性
    style
    innerHTML  双标签读写文本
    HTMLt元素标签具有什么属性,当它变为JS标签对象后,改属性就自动会变成JS对象的属性
    (img: src id width height alt title class)
    (a: href id title class name target)
    标签的属性有很多: id  class title name src href....


4. 定时函数

4.1 单次定时
    setTimeout()
    clearTimeout()

4.2 多次定时
    setInterval()
    clearInterval()

----

# JS 函数

## 1. 函数的声明

1. function 关键字方式
    function 函数名([形参]) {
        JS CODE...
    }
2. 表达式方式
    var 函数名 = function([形参]) {
        JS CODE...
    }
3. Function 构造函数方式
    var 函数名 = new Function('p1','p2', 'JS code');

## 2. 调用函数

- 加括号是调用
- 不加括号是引用


## 3. JS 函数特点

- 函数如果没有返回值 默认返回undefined
- JS函数可以重复定义的(变量)

## 4. JS 函数中的参数

hjkl;'

'

- 形参 与 实参
    - 实参个数 > 形参个数  多余的实参会被忽略
    - 实参个数 < 形参个数  未被赋值的参数 自动赋值为undefined
- 参数的默认值
    - 在函数内部 判断指定的参数是否为 undefined...
- 可变参数个数的函数
    arguments 数组对象,内涵所有的实参


## 5. JS 中的变量作用域  全局和局部变量

- 在函数内部 使用var 定义的是 局部变量
- 在函数外部 使用var 定义的是 全局变量
- 在函数内/外部 不使用var 定义的是 全局变量


## 6. JS 的作用域链

函数的执行 依赖于变量的作用域
这个作用域是在 函数定义声明时决定的，而不是 函数调用时决定的！

- 如果当前作用域里 没有声明变量，则向上一层作用域里面找.
- 如果一直到找到全局里 还都未找到，则在执行函数时 会报错.



## 7. 自执行函数 与 闭包

### 7.1 自执行

```javascript
( function(){console.log(1)} )()
( function(){console.log(2)} () )
```

这种写法的含义是将函数声明转换成函数表达式，
消除了 JS 引擎识别函数表达式和函数声明的歧义.

它告诉 JS 引擎这是一个函数表达式，不是函数声明。
并且可以在后面加括号，立即执行函数的内的代码.


### 7.2 闭包

简单说，闭包就是 能够读取 其他函数内部变量的 函数。

由于在 JS 中，只有函数内部的 子函数 才能读取 局部变量，
因此可以把闭包 简单理解成 “定义在一个 函数内部的 函数”。

所以，在本质上，闭包就是将 函数内部 和 函数外部 连接起来的一座桥梁。

闭包的最大用处有两个
1. 一个是可以读取 函数内部的变量，
2. 另一个就是 让这些变量的值 始终保持在内存中。


