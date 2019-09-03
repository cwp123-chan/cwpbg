---
category: Stuff
path: '/stuff'
title: 'js随机数'
type: 'GET'

layout: nil
---

# JavaScript的随机数

## Js中的随机数的产生

Math.random() 函数返回 0和1之间的伪随机数

可能为0 但总是小于1;

```js
 // 生成 min-max, 包含min 但不包含 max 的整数
 parseInt(Math.random() * (max-min) + min ,10 );
 
 function RandomNum(Min, Max){
 		var Range = Max-Min;
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





