# sass 基本

## 1. 安装

### 在安装ruby的基础上通过以下命令;

```
apt-get install update 
gem install sass
```

## 2. 格式转换

```
sass style.scss style.css
```

## 3. 监听转换变化

```
sass --watch scss:css <--style> <模式>		监听转换变化

sass -i 							在终端支持sass语法;
```

## 4. 基本语法

* #### 使用嵌套

```scss
div {
  color: black;
  ul {
    font-size: 10px;
    li {
      background: #747474;
      a {
        position: relative;
        &:hover {
          display: none;
        }
      }
      &:hover {
        color: yellow;
      }
    }
  }
}
```

* #### 样式属性嵌套

```scss
div {
  background: {
    color: yellow;
    image: url(./1.jpg);
  }
}

div {
  border: 1px solid {
    color: yellow;
    width: 19px;
  }
}
```



* #### 在sass变量中 也支持嵌套形式

```scss
// 在sass变量中 也支持嵌套形式;
// @mixin arguName (argument1,argument2){
// styleName:stylelist;
// }
@mixin aler() {
  color: blue;
  &:hover {
    display: none;
  }
}

body {
  @include aler;
}

```

* #### @mixin 用法

```
// @mixin arguName (argument1,argument2){
// styleName:stylelist;
// }
```

```scss
@mixin alert($spaceColor, $inlineColor) {
  background: {
    color: $inlineColor;
  }
  &:hover {
    font: {
      color: $inlineColor;
    }
  }
  border: 1px solid $spaceColor {
    size: 10px;
  }
}

div {
  @include alert(#747538, red);					// 输出时 div内属性内容就是mixin中的属性内容
}

span {
  @include alert($inlineColor: blue, $spaceColor: yellow);	// 这样参数顺序可以改变
}
```

* ####  sass 的继承(extend)

```scss
div {
  border: 1px solid $primary-color;
  width: 100px;
  height: 100px;
  ul {
    @extend div;
  }
}

.div2 {
  @extend div;
}
```

* #### 如何加载外部文件(import)

```
首先:// 加载外部文件 外部文件命名以_开头 就不会被scss监听解析
如:在外部创建 _base.scss 文件
用@import加载
```

```
@import "base";
```

## 5.函数

* ####  数学函数

```
percentage(30px / 60px);			// 求百分比
abs()								// 求绝对值
```

* #### sass类型

```
color 颜色; number 数字, booleran 布尔值......
```

* #### sass 字符串几种函数(sass中 都是以1 为起始键)

```scss
$var = "hello world"
to-upper-case($var)						//转换为大写
to-lower-case($var)						//转换为小写
str-length($var)						//统计字符串长度
str-index($var,"holle") 				//查看字符串出现的位置  下标都是以1为开始键
str-insert($var,"you",14)				//表示在第14个位置插入 you
```

* #### color

```scss
darken($color,30%)						//加深颜色 变暗
lighten($color,30%)						//减少颜色 变亮
```

* #### 颜色透明度

```scss
// opacity($color,0.3); 减少透明度
// transparentize($color,0.3);增加透明度
```

* #### 列表函数(list)

```scss
$list:1px solid;	
length($list)							//获取list内的长度
nth($list,1)							//获取list内 第一个组数据
index($list,1px)						//获取组数据出现的位置
append($list,white);					//在组末尾插入组数据
join($list1,$list2);					//合并两组
join($list1,$list2,comma)				//合并两组并且各组数据用','隔开

```

​	

* #### map

```scss
$color:(color:yellow,size:18px);
length($color);							//统计组项数
map-get($color,color)					//获取项属性中的属性值
map-keys($color)						//获取组中所有属性名
map-values($color)						//获取组中所有属性值
map-has-key($color,color)				//判断是否存在该属性名
map-merge($color,(font-size:100px))		//添加合并项属性
map-remove($color,font-size,...	)		//参2只要写属性名就行了 可以移除多项
```

​	

* #### boolean

```scss
and 表示 &&
or 表示 ||
not 表示 ^
// not(a>b) 表示 (!a>b)
```

* #### 变量的调用

```scss
// 变量的调用
// #{}   // 来调用变量 在变量名或者选择器名中的字符串合并配合使用
```

```scss

$version: "0.0.1";
/* 当前版本号是:#{$version}*/

$name: "items";
$keyName: "border";
.alert-#{$name} {
  #{$keyName}-size: 1px solid;
}
```

## 6. 逻辑结构

* #### @if 条件 {...}

```scss
// @if 条件 {...}
$display-stats: (
  display: noe
);

.div {
  @if map-get($display-stats, display) == block {
    display: none;
  } @else if map-get($display-stats, display) == none {
    display: block;
  } @else {
    #{$keyName}-size: 10px;
  }
}
```

* #### @for $var from <起始值> through <结束值> {...}	包含结束值

* #### @for $var from <起始值> to <结束值> {...}                不包含结束值

```scss
$cols: (
  color: #ccc
);
$name: "color";
$size: 1px;
.box {
  @if map-get($cols, color) == #cbc {
    background-#{$name}: map-get($cols, color);
  } @else {
    @for $size from 0 through 10 {
      border-size: #{$size}px;
    }
  }
}

// 列2

$coluns: 4;
@for $i from 1 through $coluns {
  .boxs-#{$i} {
    width: (100% / $coluns) * $i;
  }
}
// 列3
$colun: 15;
@for $k from 1 to $colun {
  .boxTo-#{$k} {
    width: (100% / $colun) * $k;
  }
}

// 3. @each $var in $list {...}

$strList: success error warning;

@each $Name in $strList {
  .items-#{$Name} {
    color: 1px;
  }
}
```

* #### @while 条件{...}

```scss
$p: 6;

@while $p > 0 {
  .itemess-#{$p} {
    width: 5px * $p;
  }
  $p: $p - 1;
}
```

* #### 自定义函数

```scss
// @function varName (var1,var2) {...}

$colors: (
  light: 1%,
  color: #ccc
);
$opct: (
  size: 100px,
  height: 30px
);
@function color($colors, $key) {
  @return map-get($colors, $key);
}

.nav {
  color: color($colors, color);
}
.nav2 {
  size: color($opct, size);
}
```



## 7. 错误与提示

```scss
# // 警告与提示(提示会在命令行出现)
# @function color($colors, $key) {
#   @if not map-has-key($colors, $key) {
#     @warn "对不起,没有找到#{$key}";
#     @error "对不起,没有找到#{$key}";
#   }
# }
# .nav3 {
#   color: color($colors, hehe);
# }
```



