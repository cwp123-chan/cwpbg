---
title: Js 大纲
date: 2019-07-05 22:46:49
---
# 前情提要

JS 对象

JS 数组

----

# JavaScript 的内置对象

## Boolean

### Boolean 对象的声明
- 直接赋值  t / f
- 转换函数  
- 构造函数  

----

## Number

### Number 对象的声明
- 直接赋值
- 转换函数
- 构造函数

### Number的属性
### Number的方法

----

## String

### String 对象的声明
- 直接赋值
- 转换函数
- 构造函数

### String 的属性
### String 的方法

----

## RegExp

### RegExp 对象的声明
- 直接赋值
- RegExp 构造函数

### RegExp的方法

### RegExp 示例

----

## Date

### 获取 date 对象
### 获取 date 中的信息
### 设置 date

----

## Math

### Math 的属性
### Math 的方法

----

## Array

已学习

----

# PS.补充

> JS 中的随机数的产生.
> > Math.random() 函数 返回 0 和 1 之间的伪随机数
> > 可能为 0, 但总是小于 1

```javascript
// 生成 min-max,包含 min 但不包含 max 的整数:
parseInt(Math.random() * (max-min) + min, 10);

function RandomNum(Min, Max) {
      var Range = Max - Min;
      var Rand = Math.random();
      var num = Min + Math.floor(Rand * Range); //舍去
      return num;
}


// 生成min-max,不包含 min 但包含 max 的整数:
Math.floor(Math.random() * (max-min) + min) + 1;

function RandomNum(Min, Max) {
      var Range = Max - Min;
      var Rand = Math.random();
      if(Math.round(Rand * Range)==0){       
        return Min + 1;
      }
      var num = Min + Math.round(Rand * Range);
      return num;
}


// 生成 min-max,不包含 min 和 max 的整数:
Math.round(Math.random() * (max-min) + min + 1);

function RandomNum(Min, Max) {
      var Range = Max - Min;
      var Rand = Math.random();
      if(Math.round(Rand * Range)==0){
        return Min + 1;
      }else if(Math.round(Rand * Max)==Max)
      {
        index++;
        return Max - 1;
      }else{
        var num = Min + Math.round(Rand * Range) - 1;
        return num;
      }
 }


// 生成min-max ,包含 min 和 max 的随机数:
Math.round(Math.random() * (max-min)+min);
Math.floor(Math.random() * ((max-min)+1) + min);


function RandomNumBoth(Min,Max){
      var Range = Max - Min;
      var Rand = Math.random();
      var num = Min + Math.round(Rand * Range); //四舍五入
      return num;
}
```
