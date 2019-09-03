---
title: jQuery 相关选择器以及属性节点
date: 2019-08-23 22:46:49
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

## 5. 类型选择

```
$("div[type="text"]").onclick(function(){});
```

