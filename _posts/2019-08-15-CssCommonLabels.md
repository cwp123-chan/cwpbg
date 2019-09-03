---
title: CSS基本标签
date: 2019-08-15 22:46:49
---
# CSS基本标签

## 1.背景标签

```
background-image:url();							添加背景图片
background-repeat:repeat/norepeat			重复/不重复
background-attachment:fixed/scroll			背景是否随滚动条滚动 不滚动/滚动 固定对body有效
```



## 2. 文字

```
text-index: 2em;							首航缩进2汉字
text-alignt:right/center/left;				水平对齐
line-height:;								行高与总高长度相同 那么就会水平居中;
letter-spacing:1px;							字间像素;
```

## 3. 浮动

```
float:left;									左浮动
在其下一个元素中写 
clear:both|left|right						使其下一个元素不受浮动影响;
```

## 4. 隐藏hiddle

```
display:none								不占位隐藏
visibility:hidden							占位隐藏;
overfolw :hidden							超出部分隐藏
overflow:auto								超出加滚动条 不超出不加 自动判断
overflow:scroll								加滚动条;

```

## 5. textarea

```
resize:none									禁止用户拖动文本框
```

## 6. :after :before

```
表示在标签前插入 content 和标签后插入 content
p :after{
content:"是";
color:white										并且可以设置属性;

}

p :before{
content:attr(title)								可以将某属性的值作为content内容显示到p标签后面;
content:url('./1.jpg')							还可以引入一张图片;
}
```

## 7. counter

```
见手册;
```

