---
title: 动态组件
date: 2019-09-05 23:40:49
---
# 动态组件

### 4.1 实现动态组件

在不同组件之间进行动态切换

```html
<component is="组件名" class="tab"></component>
```

实现选项卡案例:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <script src="./package/vue.js"></script>
    <title>动态组件</title>
</head>
<style>
    button {
        float: left;
    }
    .tab1{
        background-color: red;
    }
    .tab2{
        background-color: blue;
    }
    .tab3{
        background-color: green;
    }
</style>
<body>
    <div id = "vm">
        <button v-for = "tab in tabs" :key = "tab" @click = "changetab(tab)">{{tab}}</button><br>
        <component :is = "tabnum"></component>
    </div>
</body>
<script>
    Vue.component("tab-1",{
        template : `
            <div>
                <h1>我是选项卡1</h1>
            </div>
        `
    })
    Vue.component("tab-2",{
        template : `
            <div>
                <h1>我是选项卡2</h1>
            </div>
        `
    })
    Vue.component("tab-3",{
        template : `
            <div>
                <h1>我是选项卡3</h1>
            </div>
        `
    })
    var vm = new Vue({
        el : "#vm",
        data : {
            tabs : ["tab1","tab2","tab3"],
            currentTab : "tab1",
            tabnum : "tab-1"
        },
        methods : {
            changetab : function(val){
                console.log(val)
                this.tabnum = "tab-" + val.split("tab")[1]
            }
        }
    })
</script>
</html>
```

下面是实际案例

```css
.tab-button {
  	padding: 6px 10px;
	border-radius: 3px 3px 0px 0px;
	border: 1px solid #ccc;
	cursor: pointer;
	background: #f0f0f0;
	margin-bottom: -1px;
	margin-right: -1px;
	outline: none;
}
.tab-button:hover {
  	background: #ddd;
}
.tab-button.active {
  	background: #ddd;
}
.tab {
  	border: 1px solid #ccc;
  	padding: 10px;
}

.posts-tab {
	display: flex;
}
.posts-sidebar {
	max-width: 40vw;
	margin: 0;
	padding: 0 10px 0 0;
	list-style-type: none;
	border-right: 1px solid #ccc;
}
.posts-sidebar li {
	white-space: nowrap;
	text-overflow: ellipsis;
	overflow: hidden;
	cursor: pointer;
}
.posts-sidebar li:hover {
 	background: #eee;
}
.posts-sidebar li.selected {
 	background: lightblue;
}
.selected-post-container {
 	padding-left: 10px;
}
.selected-post > :first-child {
 	margin-top: 0;
 	padding-top: 0;
}
```



```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>动态组件</title>
	<link rel="stylesheet" href="./tab-style.css">
</head>
<body>
	<div id="app">
		<h1>动态组件</h1>
		<hr>
											<!-- :class 意思是: 当tab名与currentTab一致时 触发active -->
		<button v-for="tab in tabs" :key="tab" :class="['tab-button', {active: tab === currentTab}]" @click="setTab(tab)"> {{tab}} </button>

		<component :is="currentTabComponent"></component>

	</div>

	<script src="./package/vue.js"></script>
	<script>
		//创建组件
		Vue.component('tab-home', {
			template: `
			<div class="tab">Home 组件 </div>
			`
		});

		Vue.component('tab-posts', {
			template: `
			<div class="tab">posts 组件 </div>
			`
		});

		Vue.component('tab-article', {
			template: `
			<div class="tab">article 组件 </div>
			`
		});


		//创建Vue实例
		var vm = new Vue({
			el: '#app',
			data: {
				tabs: ['Home', 'Posts', 'Article'],
				currentTab: 'Home'
			},
			methods: {
				setTab: function(val){
					this.currentTab = val;
				}
			},
			computed: {
				currentTabComponent: function(){
					return 'tab-'+this.currentTab.toLowerCase();
				}
			}
		})
	</script>
</body>
</html>
```



### 4.2 在动态组件上使用 `keep-alive`

包裹动态组件时，会缓存不活动的组件实例，而不是销毁它们

主要用于保留组件状态或避免重新渲染

```html
<!-- 基本 -->
<keep-alive>
  <component :is="view"></component>
</keep-alive>

<!-- 多个条件判断的子组件 -->
<keep-alive>
  <comp-a v-if="a > 1"></comp-a>
  <comp-b v-else></comp-b>
</keep-alive>

```

实例:

```
css同上!
```

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>动态组件</title>
	<link rel="stylesheet" href="./tab-style.css">
</head>
<body>
	<div id="app">
		<h1>动态组件 keep-alive</h1>
		<hr>
				
		<button v-for="tab in tabs" :key="tab" :class="['tab-button', {active: tab === currentTab}]" @click="setTab(tab)"> {{tab}} </button>
		<keep-alive>
			<component :is="currentTabComponent"></component>
		</keep-alive>

	</div>

	<script src="./package/vue.js"></script>
	<script>
		//创建组件
		Vue.component('tab-home', {
			template: `
			<div class="tab">Home 组件 </div>
			`
		});

		Vue.component('tab-posts', {
			data: function(){
				return {
					posts: [
						{id:1, title:"静夜思", content:'床前明月光，疑是地上霜。'},
						{id:2, title:"春晓", content:'春眠不觉晓，处处闻啼鸟。'},
						{id:3, title:"登黄鹤楼", content:'白日依山尽，黄河入海流。'}
					],
					selectedPost: null,
				}
			},
			created: function(){
				console.log('tab-posts被创建了');
			},
			beforeMount: function(){
				this.selectedPost = this.posts[0];
			},
			activated: function(){
				console.log('tab-posts activated');
			},
			deactivated: function(){
				console.log('tab-posts deactivated ')
			},
			destroyed: function(){
				console.log('tab-posts被销毁了');
			},

			template: `
			<div class="posts-tab tab">
				<ul class="posts-sidebar">
					<li 
						v-for="post in posts" 
						:key="post.id" 
						:class="{selected: post === selectedPost}" 
						@click="selectedPost = post" 
					> 
						{{post.title}} 
					</li>
				</ul>
				<div class="selected-post-container">
					<div class="selected-post">
						<h2> {{ selectedPost.title }} </h2>
						<div>
							<p> {{ selectedPost.content }} </p>
						</div>
					</div>
				</div>
			</div>
			`
		});

		Vue.component('tab-article', {
			template: `
			<div class="tab">article 组件 </div>
			`
		});


		//创建Vue实例
		var vm = new Vue({
			el: '#app',
			data: {
				tabs: ['Home', 'Posts', 'Article'],
				currentTab: 'Home'
			},
			methods: {
				setTab: function(val){
					this.currentTab = val;
				}
			},
			computed: {
				currentTabComponent: function(){
					return 'tab-'+this.currentTab.toLowerCase();
				}
			}
		})
	</script>
</body>
</html>
```

### 4.3 绑定组件选项对象

动态组件可以绑定 组件选项对象（有component属性的对象），而不是已注册组件名的示例

```javascript
var tabs = [
  {
    name: 'Home', 
    component: { 
      template: '<div>Home component</div>' 
    }
  },
  {
    name: 'Posts',
    component: {
      template: '<div>Posts component</div>'
    }
  },
  {
    name: 'Archive',
    component: {
      template: '<div>Archive component</div>',
    }
  }
]

new Vue({
  el: '#dynamic-component-demo',
  data: {
  	tabs: tabs,
    currentTab: tabs[0]
  }
})
```

```javascript	
<component
    v-bind:is="currentTab.component"
    class="tab"
 ></component>
```

实例:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>动态组件</title>
	<link rel="stylesheet" href="./tab-style.css">
</head>
<body>
	<div id="app">
		<h1>动态组件-绑定动态组件对象</h1>
		<hr>

		<button v-for="tab in tabs" :key="tab.id" :class="['tab-button', {active: tab === currentTab}]" @click="setTab(tab)"> {{tab.name}} </button>
	
		<keep-alive>
			<component :is="currentTab.component" class="tab"></component>
		</keep-alive>
	</div>

	<script src="../dist/vue.js"></script>
	<script>

		var tabs = [
			{
				id: 1,
				name:'Home',
				component: {
					template: `
					<div>Home 组件</div>
					`
				},
			},
			{
				id: 2,
				name:'Posts',
				component: {
					template: `
					<div>Posts 组件</div>
					`
				},
			},
			{
				id: 3,
				name:'Article',
				component: {
					template: `
					<div>Article 组件</div>
					`
				},
			}
		]
		

		//创建Vue实例
		var vm = new Vue({
			el: '#app',
			data: {
				tabs: tabs,
				currentTab: tabs[0]
			},
			methods: {
				setTab: function(val) {
					this.currentTab = val;
				}
			}
		})
	</script>
</body>
</html>
```

## 5 组件的其他特性

### 5.1 解析 DOM 模板时的注意事项

有些 HTML 元素，诸如 <ul>、<ol>、<table> 和 <select>，对于哪些元素可以出现在其内部是有严格限制的。而有些元素，诸如 <li>、<tr> 和 <option>，只能出现在其它某些特定的元素内部。

```html
<table>
  <blog-post-row></blog-post-row>
</table>
```

上面的写法，渲染效果会不甚理想，可以采用以下写法

```html
<table>
  <tr is="blog-post-row"></tr>
</table>
```

需要注意的是如果我们从以下来源使用模板的话，这条限制是不存在的：

- 字符串 (例如：template: '...')
- 单文件组件 (.vue)
- `<script type="text/x-template">`

```html
表格实例:
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>Vue 组件 表格 DOM模板特性</title>
	<style>
		.table-container {
			width: 800px;
			border-collapse: collapse;
		}
		.table-container td {
			padding: 10px;
			border: 1px solid #ccc;
		}
		.table-container th {
			padding: 10px;
			border: 1px solid #ccc;
			background-color: #f0f0f0;
		}
	</style>
</head>
<body>
	<div id="app">
		<h1>Vue 组件 表格 </h1>
		<hr>
		

		<table class="table-container">
			<tr>
				<th>ID</th>
				<th>姓名</th>
				<th>年龄</th>
				<th>地址</th>
				<th>操作</th>
			</tr>

			<tr is="row-item" v-for="item in items" :key="item.id" :item="item" @remove="execRemove"></tr>
		</table>

		<br>
		<br>
		<br>

		<table>
			<tr>
				<td>姓名</td>
				<td><input type="text" v-model="itemName"></td>
			</tr>
			<tr>
				<td>年龄</td>
				<td><input type="text" v-model="itemAge"></td>
			</tr>
			<tr>
				<td>住址</td>
				<td><input type="text" v-model="itemAddress"></td>
			</tr>
			<tr>
				<td></td>
				<td>
					<button @click="addItem">添 加</button>
				</td>
			</tr>
		</table>
		

	</div>
	<script type="text/x-template" id="row-item-template">
		
		
	</script>


	<script src="../dist/vue.js"></script>
	<script>
		//定义对象 组件的选项
		var rowItemComponent = {
			props: {
				item: Object
			},
			methods: {
				removeItem: function(item){
					this.$emit('remove', item);
				}
			},
			template: `
			<tr>
				<td> {{ item.id }} </td>
				<td> {{ item.name }} </td>
				<td> {{ item.age }} </td>
				<td> {{ item.address }} </td>
				<td> <button @click="removeItem(item)">删 除</button> </td>
			</tr>
			`
		};

		//创建Vue实例
		var vm = new Vue({
			el: '#app',
			data: {
				items: [
					{id: 1, name: '赵子龙', age: 29, address:'石家庄'},
					{id: 2, name: '吕布', age: 59, address:'包头'},
					{id: 3, name: '孙悟空', age: 1059, address:'连云港'},
					{id: 4, name: '贾宝玉', age: 19, address:'南京'}
				],
				itemName: '',
				itemAge: '',
				itemAddress: '',
				newItemId: 5
			},
			methods: {
				addItem: function(){
					this.items.push({
						id: this.newItemId ++,
						name: this.itemName,
						age: this.itemAge,
						address: this.itemAddress
					});

					this.itemName = this.itemAge = this.itemAddress = '';
				},
				execRemove: function(val) {
					var index = this.items.indexOf(val); //计算要删除的项目的索引
					this.items.splice(index, 1);
				}
			},
			//template: '#row-item-template',
			components: {
				'row-item': rowItemComponent
			}
		});

	</script>
</body>
</html>
```

### 5.2 Prop的一些问题

#### Prop的属性名问题

HTML 中的特性名是大小写不敏感的，所以浏览器会把所有大写字符解释为小写字符。这意味着当你使用 DOM 中的模板时，camelCase (驼峰命名法) 的 prop 名需要使用其等价的 kebab-case (短横线分隔命名) 命名

如果你使用字符串模板，那么这个限制就不存在了。

#### 非Prop属性

组件上定义的非Prop属性 会传递到 组件模板的根元素上

class 和 style 特性会非常智能，即两边的值会被合并起来

#### 对prop重新赋值 

子组件中，对prop重新赋值，会报警告

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>prop的一些问题</title>
	<style></style>
</head>
<body>
	<div id="app">
		<h1>prop的一些问题</h1>
		<hr>

		<my-component item-name="阿波罗计划" item-address="中国北京" title="你好" class="active"> </my-component>
	</div>

	<script src="../dist/vue.js"></script>
	<script>
		//创建组件实例
		Vue.component('my-component', {
			props: ['itemName', 'item-address'],
			data: function(){
				return {
					myItemName: this.itemName
				}
			},
			methods: {
				changeItemName: function(){
					this.itemName = '我是新的值';
				}
			},
			template: `
				<div title="hello" class="item">
					<p>{{ myItemName }}</p>
					<p>{{ itemAddress }}</p>
					<input type="text" v-model="myItemName"/>
					<button @click="changeItemName">change</button>
				</div>
			`
		})	

		//创建Vue实例
		var vm = new Vue({
			el: '#app'
		})
	</script>
</body>
</html>
```

## 5.3 Vue组件的相关特性

#### 1. 监听原生组件

当我们创建组件时,只有创建组件实例内部的方法才能在被调用时使用,但我们想在原生组件,也就是 html组件中监听事件  我们可以在 @click.native = "";监听



#### .sync 修饰符

在有些情况下，我们可能需要对一个 prop 进行“双向绑定”

推荐以 update:my-prop-name 的模式触发事件

```javascript
//子组件中
this.$emit('update:title', newTitle)
```

```html
<!-- 上级组件 模板中 -->
<text-document
  v-bind:title="doc.title"
  v-on:update:title="doc.title = $event"
></text-document>
```

以上写法可以换成下列写法

```html
<text-document v-bind:title.sync="doc.title"></text-document>
```

以上实例:

```html
<!DOCTYPE html>
<html lang="en">
<head>
	<meta charset="UTF-8">
	<title>组件事件特性</title>
	<style>
		.blog-post{
			padding: 20px;
			width: 600px;
			border: 1px solid #ccc;
		}
	</style>
</head>
<body>
	<div id="app">
		<h1>组件 事件的特性</h1>
		<hr>
		<h2>上层组件</h2>
		<p>{{ message }}</p>
		
		<hr>
        <!--使用 @click.native 可以监听原生组件-->
		<blog-post @click.native="clickFn" v-bind:message="message" @update:message="message = $event"></blog-post>
		<!-- 上面的@update 是监听子组件方法传来的参数 -->
        <!-- 意思是 监听名字updata:message 且将message变成传来的$event的值 从而实现改变-->
        
        <!-- 这就是上面方法的简写 -->
		<blog-post :message.sync="message"></blog-post>
	</div>

	<script src="../dist/vue.js"></script>
	<script>
		//定义组件实例
		Vue.component('blog-post', {
			props: ['message'],
			data: function(){
                //要想 在子组件中将值反馈给父组件,不能直接修改 message值,所以 只能用newMessage替代
                // 间接修改message 值
				return {
					newMessage: this.message
				}
			},
            // 因为只能用newMessage值,所以,这边就得监听newMessage 只要一有变化,就直接传递数据给父组件,通过父组件改变父组件中message值,这边介绍了一种简写
			watch: {
				newMessage: function(){
                    // 参一 : 需要改变的父组件值
                    // 参二 : 参入父组件的值;
					this.$emit('update:message', this.newMessage);
				}
			},
			template: `
				<div class="blog-post">
					<h2>blog-post</h2>
					<p>{{ newMessage }}</p>
					<!-- 这边的v-model不能直接监听message 而是需要简介监听newMessage 因为是子组件所以不能-->
					<input type="text" v-model="newMessage" />
				</div>
			`
		})

		//创建Vue实例
		new Vue({
			el: '#app',
			data: {
				message:'哈哈哈，同志们好'
			},
			methods: {
				clickFn: function(){
					console.log('我被click了');
				}
			}
		})
	</script>
</body>
</html>
```





### 5.4 官方文档-组件

深入了解组件

网址： https://cn.vuejs.org/v2/guide/components-registration.html



