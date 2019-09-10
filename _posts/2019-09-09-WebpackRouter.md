---
title: webpack router
date: 2019-09-09 23:40:49
---
# webpack router

## 1. 代码示例

在初始化好的项目中 src/component/创建 Home.vue(简单定义需要加载的页面)

```vue
<template>
   <div>
       <h1>我是Home</h1>
       <p>{{message}}</p>
   </div>
</template>

<script type="text/ecmascript-6">
export default {
   data() {
       return {
           message : '我是Home页面'
       }
   }
}
</script>

<style lang="less">
</style>
```

在初始化好的项目中 src/component/创建 about.vue(简单定义需要加载的页面)

```vue
<template>
   <div>
       <h1>我是about</h1>
       <p>{{message}}</p>
   </div>
</template>

<script type="text/ecmascript-6">
export default {
   data() {
       return {
           message : '我是about页面'
       }
   }
}
</script>

<style lang="less">
</style>

```

接着在/src/app.vue中写入前台index.html需要显示的文件标签 

<router-link></router-link>			表示需要点击加载路由的地方

​    <router-view><router-view>			表示页面展示的地方

```vue
<template>
  <div id="app">
    <img src="./assets/logo.png">
    <router-link to="/Home">Home</router-link>			
    <router-link to="/about">about</router-link>
    <router-view/>

  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>

```

最后在/src/router/index.js中定义路由

这边需要使用模块 "vue-router"

```js
import Vue from 'vue'
import Router from 'vue-router'
import Index from '@/pages/Index'

Vue.use(Router)

// 导入组件
import Home from '../components/Home'
import about from '../components/about'
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Index',					//name 可写可不写
      component: Index
    },{
      path: '/Home',
      component:Home
    },{
      path: '/about',
      component:about
    }
  ]
})

// 上面一步相当于
// 先定义
// const routes = [{
//   {
//     path: '/',
//     name: 'Index',
//     component: Index
//   },{
//     path: '/Home',
//     component:Home
//   },{
//     path: '/about',
//     component:about
//   }
// }]
// 定义路由
// var router = new Router({routes})
// 然后 抛出路由
// export default router

```

重定向

**1.**这边的path就是 地址栏中 如果 是/home 就展示home内容 如果是/about 就展示 about内容, 如果要想一进入页面就想展示/home内容 那么 只需要将 index的path改掉 将 home的path改成'/'就行

**2** 如果输入不存在的页面 那么就让他跳转到指定页面 只需在最后的path加上 '*'

```
export default new Router({
  routes: [
    {
      path: '/',
      name: 'Index',					//name 可写可不写
      component: Index
    },{
      path: '/Home',
      component:Home
    },{
      path: '/about',
      component:about
    },{
      path: '*',
      component:404
  ]
})

```

3. 如果需要在路由传参时就必须在路由文件中加上id

   ```
   <router-link to = "/home/参数名>我是参数下的页面</router-link>
   ```

   ```
   routes : [{
   	path: '/',
       name: 'Index',					//name 可写可不写
       component: Index
   
   },{
       path: '/Home/:id',
       component:Home
     }
       
   ]
   ```

   

```vue
//在需要展示的文件中
<template>
   <div>
       <h1>我是about</h1>
       <p>{{msg}}</p>
   </div>
</template>

<script type="text/ecmascript-6">
export default {
   data() {
       return {
           message : '我是about页面'
       }
   },
   computed :{
       msg(){
           return this.$route.params.id               // 获取url中参数
       }
   }
}
</script>

<style lang="less">
</style>
```

3. ###### 路由的嵌套

   如果需要在 一个路由下在创建一个子跳转页面 那么就得创建子路由

   ```
   <router-link to = "/home/子路由路径>我是子路由的页面</router-link>
   ```

   ```vue
   routes : [{
   	path: '/',
       name: 'Index',					//name 可写可不写
       component: Index
   
   	},{
       path: '/Home/:id',
       component:Home
     	},{
           path: '/Home'
           children:[{
            	                               //构建Home页面下的子路由页面路径
   			path:'book'
   			component:book
           },{
   			path:'movie'
   			component:movie
           },{
   			path:''							//重定向 但不需要写'/'
   			component:默认页面
           }]
     	}
       
   ]
   ```

   ```
   另一种方法:
   {
   path:'/book',
   name:'book',
   component:Book,
   redirect:'/book/playing',					// 设置路由默认
   // 如果出现错误 改成 redirect :'/playing'		版本问题 意思就是 当打开 /book时默认 打开/playing
   children:[{
   path:'子路径地址',
   component:子路径名称,
   },{},{}]
   }
   ```
   
   
   
   ### 4. 编程式路由
   
   给定每个被点击的组件id值 方便在 点击事件方法中用 $route.id获取
   
   (组件 app.vue文件)
   
   ```vue
   <template>
   <div id="app">
     <router-view></router-view>
     <m-Tabbar>
         <m-TabbarItem id="index">
           <img slot="icon-normal" src="./assets/images/ic_tab_home_normal.png" alt />
           <img slot="icon-active" src="./assets/images/ic_tab_home_active.png" alt />
           首页
         </m-TabbarItem>
           <m-TabbarItem id="book">
             <img slot="icon-normal" src="./assets/images/ic_tab_subject_normal.png" alt />
             <img slot="icon-active" src="./assets/images/ic_tab_subject_active.png" alt />
             书影音
           </m-TabbarItem>
         <m-TabbarItem id="broad">
           <img slot="icon-normal" src="./assets/images/ic_tab_status_normal.png" alt />
           <img slot="icon-active" src="./assets/images/ic_tab_status_active.png" alt />
           广播
         </m-TabbarItem>
         <m-TabbarItem id="group">
           <img slot="icon-normal" src="./assets/images/ic_tab_group_normal.png" alt />
           <img slot="icon-active" src="./assets/images/ic_tab_group_active.png" alt />
           小组
         </m-TabbarItem>
         <m-TabbarItem id="me">
           <img slot="icon-normal" src="./assets/images/ic_tab_profile_normal.png" alt />
           <img slot="icon-active" src="./assets/images/ic_tab_profile_active.png" alt />
           我的
         </m-TabbarItem>
       </m-Tabbar>
   </div>
   </template>
   
   <script>
   import mTabbar from "./components/tabbar";
   import mTabbarItem from "./components/tabbar-item";
   export default {
     name: "index",
     // Html代码中必须使用m-Tabbar 在html中也会自动解析成mtabbar/mTabbar
     components: {
       // mTabbar,                  // 实际上就是 "m-Tabbar" : mTabbar
       "m-Tabbar":mTabbar,
       mTabbarItem                 //也可同上
     }
   };
   </script>
   
   <!-- Add "scoped" attribute to limit CSS to this component only -->
   <style lang="less">
   </style>
   
   
   ```
   
   
   
   给定每个路由的name值
   
   (路由 index.js文件)
   
   ```js
   import Vue from 'vue'
   import Router from 'vue-router'
   // 导入首页面
   import Index from '@/pages/Index'
   import Book from '@/pages/Book'
   import Group from '@/pages/Group'
   import Broad from '@/pages/Broad'
   import Me from '@/pages/Me'
   // 使用Vue的Router包
   Vue.use(Router)
   
   // 导入组件
   // import Home from '../components/Home'
   import about from '../components/about'
   import none from '../components/404'
   export default new Router({
     routes: [{
         path:'/book',
         name:'book',
         component : Book
       },
       {
         path:'/group',
         name:'group',
         component : Group
       },
       {
         path:'/broad',
         name:'broad',
         component : Broad
       },
       {
         path:'/me',
         name:'me',
         component : Me
       },{
         path: '/',
         name: 'Index',
         component: Index
       },{
         path : '/about/:id',
         component:about
       }
       ,{
         path:'*',
         name:'index',
         component:Index
       }
       // ,{
       //   path: '/Home',
       //   component:Home
       // },{
       //   path: '/about',
       //   component:about
       // }
     ]
   })
   
   // 上面一步相当于
   // 先定义
   // const routes = [{
   //   {
   //     path: '/',
   //     name: 'Index',
   //     component: Index
   //   },{
   //     path: '/Home',
   //     component:Home
   //   },{
   //     path: '/about',
   //     component:about
   //   }
   // }]
   // 定义路由
   // var router = new Router({routes})
   // 然后 抛出路由
   // export default router
   
   ```
   
   

最后通过$router.push(location)来添加路由

(mTabbar.vue 组件文件)

```vue
<template>
   <a class="m-tabbar-item" @click = "goToRouter" :class="{'is-Active':isActive}">
        <span class="m-tabbar-item-icon" v-show = "!isActive">
            <slot name="icon-normal"></slot>
        </span>
        <span class="m-tabbar-item-icon" v-show = "isActive">
            <slot name="icon-active"></slot>
        </span>
        <span class="m-tabbar-item-text">
            <slot></slot>
        </span>
    </a>
</template>

<script type="text/ecmascript-6">
    export default ({
        props :{           
            id:{
                type:String                     //跨域传值
            }                                   //上级像页面下级传值,需要props进行传值
        },
        methods:{
            goToRouter : function(){
                console.log(this.id)
                // 通过编程式导航进行跳转  Router.push(location)进行传值
                this.$router.push(this.id)
                // console.log(this.$route)
            }
        },
        computed :{
            isActive(){
                // 判断路径与id是否一致,如果一致 则图标变色
                if(this.id === this.$route.name){
                    return true
                }else{
                    return false
                }
            }
        }
    })
</script>

<style lang="less">
.m-tabbar-item{
            flex: 1;
            text-align: center;
            .m-tabbar-item-icon{
                display: block;
                padding-top: 2px;
                img{
                    width: 28px;
                    height: 28px;
                }
            }
            .m-tabbar-item-text{
                display: block;
                font-size: 10px;
                margin-top: -2px;
                color: #949494;
            }
        }
    .is-Active .m-tabbar-item-text{
        color:green;
    }
</style>


```

