---
title: aravel 开发api 启动测试 与解决 跨域问题
date: 2019-10-15 23:40:49
---
# laravel 开发api 启动测试 与解决 跨域问题

## 开发环境 laravel5.5



### 1. laravel5.5 已经引入了独立的无状态路由文件 api.php 作为 api 的开发，我们可以将接口需要的路由定义在该文件中

routes/api.php

### 2.定义路由并测试

利用 postman 软甲 做测试

在 api.php 中 测试 连接是否成功

```
Route::get('/show',function(){
    return "测试数据...";
}) 
```

3. ### 做 中间件测试 

   ```
   php arisan middleware AdminApi 			 命令行 创建 AdminApi 中间件
   ```

   * 中间件 内容 

   * ```
     <?php
     
     namespace App\Http\Middleware;
     
     use Closure;
     use Illuminate\Support\Facades\Response;
     
     class AdminApi
     {
         /**
          * Handle an incoming request.
          *
          * @param  \Illuminate\Http\Request  $request
          * @param  \Closure  $next
          * @return mixed
          */
         public function handle($request, Closure $next)
         {
             return Response::json(["status"=>"0","msg"=>"不安全访问"],401);
         }
     }
     ```

   * 

   * 注册中间件  app/http/kernel.php

   * ```
       */
         protected $routeMiddleware = [
             'auth' => \App\Http\Middleware\Authenticate::class,
             'auth.basic' => \Illuminate\Auth\Middleware\AuthenticateWithBasicAuth::class,
             'bindings' => \Illuminate\Routing\Middleware\SubstituteBindings::class,
             'cache.headers' => \Illuminate\Http\Middleware\SetCacheHeaders::class,
             'can' => \Illuminate\Auth\Middleware\Authorize::class,
             'guest' => \App\Http\Middleware\RedirectIfAuthenticated::class,
             'password.confirm' => \Illuminate\Auth\Middleware\RequirePassword::class,
             'signed' => \Illuminate\Routing\Middleware\ValidateSignature::class,
             'throttle' => \Illuminate\Routing\Middleware\ThrottleRequests::class,
             'verified' => \Illuminate\Auth\Middleware\EnsureEmailIsVerified::class,
             'AdminApi'=>\App\Http\Middleware\AdminApi::class,
         ];
     ```

   * 路由中使用

   * ```
     
     Route::middleware('AdminApi')->match(['get','post'],'/show',function(){
         return "hahaha";
     });
     ```

   * 

## 针对 laraver 5.7 以上版本解决 跨域问题 

说明: 出现跨域问题 是因为 测试阶段 请求和 服务端 端口不一样 所以 导致出现了 跨域 问题  所以我们需要 加上 一段 规定的header头信息 才允许 跨端口请求

方法:

1. 在laravel 中 创建 中间件 

2. 中间件内容 

   ```php
   <?php
   
   namespace App\Http\Middleware;
   
   use Closure;
   use Illuminate\Support\Facades\Response;
   
   class AdminApi
   {
       /**
        * Handle an incoming request.
        *
        * @param  \Illuminate\Http\Request  $request
        * @param  \Closure  $next
        * @return mixed
        */
       public function handle($request, Closure $next)
       {
   
           // 利用中间件 给 header 加上 Access-Control-Allow-Origin 等头信息 解决跨域问题
           $response = $next($request);
           $origin = $request->server('HTTP_ORIGIN') ? $request->server('HTTP_ORIGIN') : '';
           // 这里设置 允许 访问 的内容
           $allow_origin = [
               'http://localhost:8080',
           ];
           if (in_array($origin, $allow_origin)) {
               $response->header('Access-Control-Allow-Origin', $origin);
               $response->header('Access-Control-Allow-Headers', 'Origin, Content-Type, Cookie, X-CSRF-TOKEN, Accept, Authorization, X-XSRF-TOKEN');
               $response->header('Access-Control-Expose-Headers', 'Authorization, authenticated');
               $response->header('Access-Control-Allow-Methods', 'GET, POST, PATCH, PUT, OPTIONS');
               $response->header('Access-Control-Allow-Credentials', 'true');
           }
           return $response;
       }
   }
   ```

   3. 注册中间件 

   4.  $allow_origin 中 是允许请求的前端 域名

   5. 会多出一次 `method` 为 `options` 的请求是正常的，因为浏览器要先判断该服务器是否允许该跨域请求。

   6. 有时候返回的不是 laravel 的 response 对象而是 Symfony 的 response，所以会报 $response->header 方法找不到，所以添加 header 的方法要简单改一下，可以拼好一个数组直接调用一次，我这里是懒得改了。

      ```php
      $response->headers->add(['Access-Control-Allow-Origin' => $origin]);
      $response->headers->add(['Access-Control-Allow-Headers' => 'Origin, Content-Type, Cookie,X-CSRF-TOKEN, Accept,Authorization']);
      $response->headers->add(['Access-Control-Expose-Headers' => 'Authorization,authenticated']);
      $response->headers->add(['Access-Control-Allow-Methods' => 'GET, POST, PATCH, PUT, OPTIONS']);
      $response->headers->add(['Access-Control-Allow-Credentials' => 'true']);
      ```

   7. 另外需要注意的是，lumen 框架直接添加这个 中间件是不行的，妥妥的报 `options` 路由找不到，因为 `lumen` 用的是 `fast-route` 路由组件，跟 `laravel` 的不是同一个，`laravel` 可以是因为它帮你做了这件事，所以我们要自己注册一个 `options路由` , 大致代码如下:(针对于 lumen框架)

      ```
      路径  bootstrap/app.php
      ```

      

   ```php
   $app->router->group([
       'prefix'     => 'api',
       'middleware' => ['cross','api'],
   ], function ($router) {
       $router->options('/{path:.*}', function ($path) {});
       require __DIR__ . '/../routes/api.php';
   });
   ```

   

