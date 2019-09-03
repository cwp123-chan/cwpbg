---
title: Es6 Set和map数据结构
date: 2019-08-29 22:46:49
---

# Set和map数据结构

## 1. Set:没有下标只有值

```
类似于数组 但是 其中成员不可重复 是唯一的

创建方法 同数组一样
let myset = new Set([1,2,4,3,9]);

set的构造函数
1. 可以接收一个数组
2. 可以接收所有构造interior的数据类型
let myset2 = new Set(myset);				也可接收一个set数据结构
3. 可以进行遍历 
```

### Set 的去重

```
let arr = [10,9,10,2,1,3]
let newarr = [...(new Set(arr))]
```

### Set 属性

```
size:长度
add:添加
Set对象.delete(值)
Set对象.clear()			删除所有值
```

## 2. Weakset(弱Set)

```
与Set相似 但其成员必须是对象;
不能遍历 ,没有实现遍历器接口
```

## 3. Map

```
是一个obj类型
通过键值对组成的集合,key 可以是任意类型;
let mymap = new Map([["name","丽丽"],["age":"18"]],[window,"hahaha"]]);

可以创建一个Map类型;

构造函数:
构造函数的类型必须是一个数组类型,并且,每一个键值对必须通过[key,value]来构造;


函数:
可通过 set修改或添加map属性
map.set("name","aaa")
可通过get 获取1里面的属性值
map.get(key);
可通过clear 清空map
遍历:
map也可遍历
for of..
map.forEach(function(k,v){
...
})

keys();
values();
entries();
```

## 4. Weakmap

```
1. 键必须是对象
2. 不可遍历
3. 是弱引用
```



