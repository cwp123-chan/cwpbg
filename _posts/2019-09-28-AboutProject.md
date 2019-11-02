---
title: 关于Bookkeep项目的总结
date: 2019-09-26 22:46:49
---
# 关于Bookkeep项目的总结

## 1. 流程

* 底部tar
* 首页等三张页面
* 各个页面的组成子页面
* 设计路由
* 设计数据逻辑
* 测试api
* 完成数据导入
* 完成图表
* 修改,以及去除页面console语句
* 测试是否可行

## 2. 总结

* 在完成登录页时,一开始,想利用页面的显示或者隐藏来控制页面的交替的,但是,最后产生的bug太多,所以到最后,使用路由传参进行,单个子页面的变换

* 在完成,图片上传的过程当中,需要给予config 一个 header头信息,以及,理由fromdata模块,进行对页面图片数据的格式转换

* 在for循环当中,可以通过 函数(参数) 的方式,向方法传参,这样就可以获取当前所需要的对应参数

* 在修改完数据,出现页面数不更新的问题,可以通过,直接改变data里面所绑定的值,或者,重新获取api的值也可以完成页面数据更新

* 在整个页面开始前,最好能够设计好项目的公共组件,这样就能避免许多组件class名一样,导致其他样式发生改变 也可以利用 class>class来进行当前页面下的组件样式设置

* 是否出现异步问题,并且,如果出现,那么 promise进行axios请求,获取的then后面的res内容,如果想继续使用,那就必须进行 return 在进行一次then,才能获得

* ```
  function(){
  	axios('...',{data})
  	.then((res)=>{
  		return res
  	})
  	.then((res)=>{
  		console.log(res)
  	})
  }
  ```

  

* v-if = "xxx === 'xxx'" 可以来判断收入/支出数据的交换,避免再重写一个页面