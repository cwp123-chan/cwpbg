---
title:laravel 对request等的操作
date: 2019-10-20 23:40:49
---

# laravel 对request等的操作  

## 1. $request

* 输出 请求内容 

* ```
  $request->input("请求参数name");
  $request->input("请求参数name","默认参数")
  ```

* 判断是否含有某个参数

* ```
  $request->has("参数name")
  ```

* 所有参数

* ```
  $request->all()
  ```

* 获取请求方式

* ```
  $requesty->method()
  判断是否是某种请求方式
  $request->isMethod("get")
  
  ```

* 判断是否是ajax请求

* ```
  $request->ajax()
  ```

* 判断是否通过某个路径请求

* ```
  $request->is("xxx/*")			// 是否是xxx目录下的路径请求
  ```

* 获取当前 url

*  ```
  $request->url()
  ```

## 2. controller 之Sessoion

### 1. 使用middleWare启用 session    在 web中间件启用 

```
// 路由文件 
Route::group(['middleware'=>'web'],function(){
	Route::any("session1",xxxController@sessionmethod);
})

1. 通过 $request 中使用sesssion
function demo (Request $request){
	$request->session()->put("key","value")
	$request->session()->get("key")
}
2. 通过 session ()
function demo (Request $request){
	session()->put("key","value")
	session()->get("key")
}

3. 通过 Session类 
function demo (Request $request){
	Session::put("key","value");
	// 存储数组 
	Session::put(["key"=>"value"]);
	// 将数据放到session数组中 以数组形式存储
	Session::push("key","value")
	// 在session中只存储一次数据 使用完之后就删除
	Session::pull("key","value")
	Session:get("key");
	Session:get("key","可以加上默认值");
	// 取出所有的值
	Session::all()
	// 判断是否存在
	Session::has("key")
	
	// 删除某个数据
	Session::forget("key");
	// 删除所有数据
	Session:flush();
	
	// 暂存 第一次访问存在,第二次就不存在
	Session:falsh("key","value")
}
```

## 3.controller之 response

```
1. 返回json格式
function res(){
	$data = [
		"key1"=>"v1",
		"key2"=>"v2"
	]
	// 返回 json格式 的数组
	return response->json($data)
}

2. 重定向
return redirect("目标地址")
// 携带 快闪数据
return redirect("目标地址") -> with("key","value")

// 利用 action 重定向
return redirect()->action("xxxController@method")
				->with("key","快闪数据")
// 通过 route()别名跳转
// 首先要定义目标地址的别名
// 路由文件 
// Route::("xxx",["as"=>"response"]) .........这边就是取response为 路径 xxx 的别名
// 重定向文件
	return redirect()->route("response")->with("key","快闪数据")
	
// 返回上一级
	return redirect()->back();

```

## 3.controller之 middleware

```
过程 
1. 创建中间件 
2. 注册中间件
3. 定义路由 并且定义路由方法
4. 操作完成后的后置中间件
```

