---
title: node.Js中的自定义模块
date: 2019-07-25 22:38:34
---
# node.Js中的自定义模块

## 1.实例

```js
// 封装函数模块
function demo (a){
	console.log(a);
}
module.exports = fuc名;
```

```js
//封装变量
var str = 'string‘；
exports.str = str;
```

## 2.事件委托,事件捕获,事件冒泡

```js
//使用事件委托可以自动绑定动态添加的元素,即新增的节点不需要主动添加也可以一样具有和其他元素一样的事件。
<script>
    window.onload = function(){
        let div = document.getElementById('div');
        
        div.addEventListener('click',function(e){		// 创建监听事件 给div创建 然后会自动为其子节点创建相同的方法;
            console.log(e.target)					// 返回目标节点
        })
        
        let div3 = document.createElement('div'); 	// 创建一个元素
        div3.setAttribute('class','div3')			// 设置属性;
        div3.innerHTML = 'div3';				
        div.appendChild(div3)					// 为div下面最后一位添加一个节点,名称为div3
    }
</script>


<body>
    <div id="div">
        <div class="div1">div1</div>
        <div class="div2">div2</div>
    </div>
</body>
//虽然没有给div1和div2添加点击事件,但是无论是点击div1还是div2,都会打印当前节点。因为其父级绑定了点击事件,点击div1后冒泡上去的时候,执行父级的事件。
```

```
当为事件捕获(useCapture:true)时,先执行body的事件,再执行div的事件
当为事件冒泡(useCapture:false)时,先执行div的事件,再执行body的事件
```

