---
title: Vue创建实例
date: 2019-09-03 23:40:49
---
# Vue创建实例

## 	1. 每个Vue应用都是通过Vue函数创建一个新的Vue实例开始的

演示:one:

````html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="./package/vue.js"></script>
    <title>Document</title>
</head>
<body>

    <!-- 在页面中显示数据 -->
    <div id = "app">
        {{message}}
    </div>
    <!-- 与Dom内 元素绑定 -->
    <input type="text" v-bind:value = "value" id="app-2">
    <!-- 实时监听页面内的数据 -->
    <div id="app-3">   
    <input type="password" v-model="password" >
    <p>{{password}}</p>
    </div>

    <!-- 条件与循环 -->
    <!-- v-if -->
    <div id="app-4">
    <div style="width: 100px;height: 100px;background-color: red;" v-if = "seen"></div>
    <div style="width: 100px;height: 100px;background-color: red;" v-if = "noseen"></div>
    </div>  

    <!-- v-for -->
    <ul id = "app-5">
        <li v-for = "td in ts">
            {{td.text}}
        </li>
    </ul>

    <!-- v-on 添加监听事件 -->
    <div id = "app-6">
        <p>{{clickDate}}</p>
        <button v-on:click = "clickMethod">点击切换内容</button>
    </div>
    <!-- v-text 绑定元素 -->
    <div id = "vm-1">
        <p v-text="message">
            {{message}}
        </p>
    </div>
</body>
<script>
    // 在页面绑定内容中 显示 数据
    var app = new Vue({
        el : "#app",
        data : {
            message : "HELLO world"
        }
    });
    console.log(app.message);

    // 与Dom内 元素绑定
    var app2 = new Vue({
        el : "#app-2",
        data : {
            value : "你好啊"
        }
    })

    // 实时监听页面内的数据;
    var app3 = new Vue({
        el : "#app-3",
        data : {
            password : "输入默认值"           //  这边只是默认值;并且表单内容可以根据用户输入的来查看
        }
    })

    // 条件与循环
    // v-if
    var app4 = new Vue({
        el : "#app-4",
        data : {
            seen : true,
            noseen : false                    //  意思就是 只有页面元素中只显示seen为true元素显示 当为false时 则隐藏
        }
    })

    // v-for
    var app5 = new Vue({
        el : "#app-5",
        data : {
            ts : [
                { text : "我是第一"},
                { text : "我是第二"},
                { text : "我是第三"},
                { text : "我是第四"}          // 循环数组,前提是ts是一个数组
             ]
        }
    })

    // v-on 添加监听事件
    var app6 = new Vue({
        el : "#app-6",
        data : {
            clickDate : "hello cwp"
        },
        methods : {
            clickMethod : function(){
                this.clickDate = "你好,xiaoj"
            }
        }
    })
    
    // 使用 v-text 绑定元素 使页面加载更加流畅
    var vm = new Vue({
            el : "#vm-1",
            data : {
                message : "hello world"
            }
        })
</script>
</html>
````

演示:two:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="./package/vue.js"></script>
    <title>Document</title>
</head>
<body>
    <div id="app-ul">
        <ol>
            <todo-item
            v-for = "item in groceryList"
            v-bind:todo = "item"                   
            v-bind:key = "item.id"></todo-item> 
            <!-- 让自定义的组件样式与所需变化的值进行绑定,这样就能让数据可以传入进来 -->
        </ol>
    </div>

    <div id="app-2">
        <diy 
        v-for = "msg in msgs"
        v-bind:value = "msg"
        v-bind:key = "msg.id">

        </diy>
    </div>
</body>
<script>
    // 构建一个组件
    Vue.component('todo-item', {
        props : ['todo'],                              // 相当于给自己自定义的一个组件设置一个自定义属性/特性
        template: '<li>{{todo.text}}</li>'              // 设置组件的样式
    })

    var app1 = new Vue({
        el : "#app-ul",
        data : {
            groceryList : [
                { id : 1 , text : '我是li1'},
                { id : 2 , text : '我是li2'},
                { id : 3 , text : '我是li3'}
        ]
      }
    })


    // 构建一个组件
    Vue.component("diy",{
        template : "<h1>{{value.text}}</h1>",
        props : ['value']
    })

    var app2 = new Vue({
        el : "#app-2",
        data : {
            msgs : [
                {id : 1 , text : "第1"},
                {id : 2 , text : "第2"},
                {id : 3 , text : "第3"},
            ]
        }
    })
</script>
</html>
```

演示 :three:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="./package/vue.js"></script>
    <title>Document</title>
</head>
<body>
    <div id = "vm-1">
        <p>{{text}}</p>
        <p>{{meth}}</p> 
        
        <!-- 值得注意的时 页面加载完毕之后,不能改变其vm中设置的值 -->
        <!-- 如果需要改变 那就得先声明然后添加默认值, 这样才能在页面加载之后 -->
    </div>
    <div id="vm-2">       
        <h1>{{foo}}</h1>                       
        <button v-on:click="foo = 'baz'">Change it</button>        
        <!-- 加了 Object.freeze(obj) 属性,所以点击不会更新 -->
        </div>      
        <!-- 注意!  如果嵌套vue时,标签内数据会默认指向父级vue内的数据  -->
</body>
<script>
  
    // console.log(obj.foo);
    var vm = new Vue({
        el : "#vm-1",
        data : {
            text : "hello",
            meth : "" ,
                                       //  只有设定默认值,才会在之后修改数据时起作用;
        }
    })  
    var obj = {
        foo : "小米"                    
    }                   
    Object.freeze(obj);                 //加上之后就限定了data中的数据不会被改变
    var vm = new Vue({
        el : "#vm-2",
        data : obj
    }) 


    function demo(){
        console.log(this);
    }
    (()=>{
        console.log(this);
    })();
    demo();
</script>
</html>
```



## 	2. 一个Vue应用由一个通过new Vue创建的根Vue实例,以及可选的嵌套的可复用的组件树组成

```html
<body>
    <div id = "vm-1">
      	<p>
            {{message}}
        </p>  
    </div>
</body>
<script>
	var vm1 = new Vue({
        el : "#vm-1",
        data : {
            message : "HELLO WORLD"
        }
        // 如果 Vue实例中存在 template 属性,那么 对应的id = vm-1就无效了 此时 该vm1 实例指向的是 h1 标签
        template : "<h1>你好 世界!</h1>"
    })
</script>
```

## 3. Vue中的数据与方法

### 	1. vm.$el

```js
var vm = new Vue({
	el : "#id",
	data : {
		message : "hellow world"
	}
})
console.log(vm.$el)			// 获取到的是 id为 "id" 所对应的Dom标签					相当于=> document.getElementById("id")
			
```

### 	2. vm.$data

```js
var vm = new Vue({
	el : "#id",
	data : {
		message : "hellow world"
	}
})
console.log(vm.$data); 		// 获取到的是一个Vue 对象
console.log(vm.$data.message) === vm.message
```

### 	3. vm.$watch("属性名",function(){})

```js
var vm = new Vue({
	el : "#id",
	data : {
		message : "hellow world"
	}
})

// vm.$watch 只有在页面元素中 message 变化时才会触发 所以可以试着让页面message元素改变
vm.$watch("message",function(){
    console.log(this)  // 返回的是Vue 对象
    console.log(this.message)       // 返回的是 message
})								//	监听message的变化
```

