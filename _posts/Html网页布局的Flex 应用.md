# Html网页布局的Flex 应用

## 1.flex 容器属性

### 1. flex弹性盒子；

### 对于某个元素，只要声明diaplay:flex 就是弹性盒子　同样，弹性元素也可以被设为另一个弹性容器

### 2. 主轴;

```
flex-direction修改主轴方向
参数:
flex-direction:row (默认) 上边主轴 坐到右
flex-direction:column 左边主轴 上到下
flex-direction:row-reverse 上边主轴 右到左
flex-direction:column-reverse 左边主轴 上到下
```

### 3.沿主轴的排列处理

```
通过设置flex-wrap: nowrap | wrap | wrap-reverse可使得主轴上的元素不折行、折行、反向折行。

默认是nowrap不折行

wrap折行，顾名思义就是另起一行，那么折行之后行与行之间的间距（对齐）怎样调整？这里又涉及到交叉轴上的多行对齐。

wrap-reverse反向折行，是从容器底部开始的折行，但每行元素之间的排列仍保留正向。
```

```
复合型属性:
flex-flow = flex-direction  + flex-wrap;
flex-flow: row nowrap;
flex-flow: row wrap;....
```

### 4.元素如何弹性伸缩应对

```
flex-shrink: 1 ; 等比例收缩;  参数为其他 比例不等
flex-grow:1;		等比放大;
```

```
元素收缩算法
真的是等比缩小(每个元素各减去70/3的宽度)吗？这里稍微深究一下它的收缩计算方法。

弹性元素1：50px→37.03px
弹性元素2：100px→74.08px
弹性元素3：120px→88.89px
先抛结论：flex-shrink: 1并非严格等比缩小，它还会考虑弹性元素本身的大小。

容器剩余宽度：-70px
缩小因子的分母：1*50 + 1*100 + 1*120 = 270 (1为各元素flex-shrink的值)
元素1的缩小因子：1*50/270
元素1的缩小宽度为缩小因子乘于容器剩余宽度：1*50/270 * (-70)
元素1最后则缩小为：50px + (1*50/270 *(-70)) = 37.03px
加入弹性元素本身大小作为计算方法的考虑因素，主要是为了避免将一些本身宽度较小的元素在收缩之后宽度变为0的情况出现。
```

```
元素放大
元素放大的计算方法
放大的计算方法并没有与缩小一样，将元素大小纳入考虑。

仅仅按flex-grow声明的份数算出每个需分配多少，叠加到原来的尺寸上。

容器剩余宽度：50px
分成每份：50px / (3+2) = 10px
元素1放大为：50px + 3 * 10 = 80px
无多余宽度时，flex-grow无效
下图中，弹性容器的宽度正好等于元素宽度总和，无多余宽度，此时无论flex-grow是什么值都不会生效。
```

### 5. 弹性处理与刚性尺寸(重点)

```
flex-basis:10px;			在与width一起使时 basis优先级更高
(1) 两者为0
width: 0 —— 完全没显示
flex-basis: 0 —— 根据内容撑开宽度
(2) 两者非0
width: 非0;
flex-basis: 非0
—— 数值相同时两者等效
—— 同时设置，flex-basis优先级高
(3) 两者为auto
flex-basis为auto时，如设置了width则元素尺寸由width决定；没有设置则由内容决定;
(4) flex-basis == 主轴上的尺寸 != width
(5) 方向问题
将主轴方向改为：上→下
此时主轴上的尺寸是元素的height
flex-basis == height
```

### 6. 常用的复合属性 flex

```
flex = flex-grow + flex-shrink + flex-basis
一些简写
flex: 1 = flex: 1 1 0%
flex: 2 = flex: 2 1 0%
flex: auto = flex: 1 1 auto;
flex: none = flex: 0 0 auto; // 常用于固定尺寸 不伸缩
```

```
flex:1 和 flex:auto 的区别:
其实可以归结于flex-basis:0和flex-basis:auto的区别。

flex-basis是指定初始尺寸，当设置为0时（绝对弹性元素），此时相当于告诉flex-grow和flex-shrink在伸缩的时候不需要考虑我的尺寸；相反当设置为auto时（相对弹性元素），此时则需要在伸缩时将元素尺寸纳入考虑。

```

### 7. 容器内如何对齐

1. #### 主轴上的对齐方式

```
1. 主轴上的对齐方式
justify-content:
参数	
flex-start;			开始->结束
flex-end;			结束->开始对齐
center;				居中
space-between;		左中右对齐等比例
apsce-around;		左右相对位置按比例对齐
```

2. #### 交叉轴上的对齐方式

```
2. 交叉轴上的对齐方式	
交叉轴上的单行对齐
align-items
默认值是stretch，当元素没有设置具体尺寸时会将容器在交叉轴方向撑满。
当align-items不为stretch时，此时除了对齐方式会改变之外，元素在交叉轴方向上的尺寸将由内容或自身尺寸（宽高）决定。

align-items:stretch(默认) 如果元素未设置高度或设置为auto时 默认沾满整个容器的高度
align-items:baseline;  沿第一行文字对齐;
```

#### 3.交叉轴上的多行对齐

```
交叉轴上的多行对齐
还记得可以通过flex-wrap: wrap使得元素在一行放不下时进行换行。在这种场景下就会在交叉轴上出现多行，多行情况下，flex布局提供了align-content属性设置对齐。

align-content与align-items比较类似，同时也比较容易迷糊。下面会将两者对比着来看它们的异同。

首先明确一点：align-content只对多行元素有效，会以多行作为整体进行对齐，容器必须开启换行。
```

```
align-content: stretch | flex-start | flex-end | center | space-between | space-around

align-items: stretch | flex-start | flex-end | center | baseline
```

#### 4.align-content 与 align-items对比(重点!!!!)

```
在属性值上，align-content比align-items多了两个值：space-between和space-around。

align-content与align-items异同对比
与align-items一样，align-content:默认值也是stretch。两者同时都为stretch时，毫无悬念所有元素都是撑满交叉轴。
代码:
#container {
  align-items: stretch;
  align-content: stretch;
}
```

当我们将align-items改为`flex-start`或者给弹性元素设置一个具体高度，此时效果是行与行之间形成了间距。

```
#container {
  align-items: flex-start;
  align-content: stretch;
}

/*或者*/
#container {
  align-content: stretch;
}
#container > div {
  height: 30px;
}
```

为什么？因为`align-content`会以整行为单位，此时会将整行进行拉伸占满交叉轴；而`align-items`设置了高度或者顶对齐，在不能用高度进行拉伸的情况下，选择了用间距。

#### 尝试把`align-content`设置为顶对齐，此时以行为单位，整体高度通过内容撑开。

而`align-items`仅仅管一行，因此在只有第一个元素设置了高度的情况下，第一行的其他元素遵循`align-items: stretch`也被拉伸到了50px。而第二行则保持高度不变。

```
#container {
  align-items: stretch;
  align-content: flex-start;
}
#container > div:first-child {
    height: 50px;
}
```

#### 5.其他对其方式

#### order:更优雅地调整元素顺序

```
#container > div:first-child {
  order: 2;
}
#container > div:nth-child(2) {
  order: 4;
}
#container > div:nth-child(3) {
  order: 1;
}
#container > div:nth-child(4) {
  order: 3;
}
```

```
order：可设置元素之间的排列顺序

数值越小，越靠前，默认为0
值相同时，以dom中元素排列为准
```

参见文档https://www.cnblogs.com/qcloud1001/p/9848619.html