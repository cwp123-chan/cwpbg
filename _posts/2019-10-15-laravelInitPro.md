---
title: laravel 初始化项目
date: 2019-09-05 23:40:49
---

# laravel 初始化项目 

## 1.配置要求 

php > 7.2.0

安装有conposer环境

```
composer global require "laravel/installer"
laravel new 项目名
```

上面会为我们创建 laravel 的所有包 

通过 conposer 安装

```
composer create-project --prefer-dist laravel/laravel 项目名
```



## 2.验证数据库是否连接成功

修改 env 数据库配置

```
php artisan migrate // 用于创建表 如遇报错 已存在的话 如果是新表 可以删除重建
```

## 3. 安装 debug 调试工具(win)

```
composer require barryvdh/laravel-debugbar
```

打开config/app.php,在 **providers** 项下添加下面代码

```
'Barryvdh\Debugbar\ServiceProvider',
```

保存,然后刷新页面,在页面的下方就能看到debug调试信息了.如果想使用 **facade** 模式,需执行下面的步骤

**在config/app.php,找到** **aliases**  **项下添加以下代码**

```
'Debugbar' => 'Barryvdh\Debugbar\Facade',
```

 然后命令行执行以下命令

```
php artisan vendor:publish
```



## 4.创建个人档案的model类

```
php artisan make:model Profile -m
加上-M 会为我们创建 migrations 表结构文件
```

在建好的profile文件中编辑以下代码 创建表结构

```
<?php

use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

class CreateProfilesTable extends Migration
{
    /**
     * Run the migrations.
     *
     * @return void
     */
    public function up()
    {
        Schema::create('profiles', function (Blueprint $table) {
            $table->bigIncrements('id');
//            为我们创建 变的字段 以及类型;
            $table->string('phone');
//            指定表与用户关系 指定uesr_id 关联 user表;
            $table->unsignedInteger('admin_id');
            一张表 的非主键的字段指向另一张表的主键 那么这个就叫做外键 一张表可以有多个外键
            这边的意思就是 profiles中的admin_id 指向 admin表中的id 那么 admin_id就是关联外键 用于联查
            (这边的admin 就相当于 profiles 的父级关系表)
//            指定外键 通过外键 通知数据库让数据库按照指定的关联外键进行查询 方便数据库索引,提高效率
            $table->foreign('admin_id')
                  ->references('id')->on('admin')
                  ->onDelete('cascade');
            $table->timestamps();
        });
    }

    /**
     * Reverse the migrations.
     *
     * @return void
     */
    public function down()
    {
        Schema::dropIfExists('profiles');
    }
}

```

error 常见问题

1. 遇到 isready exites 已存在 那说明表已经存在

2. 遇到 Syntax error or access violation: 1071 Specified key was too long; max key length is 1000 bytes" 说明 长度超出该版本mysql的默认范围 直接修改app/Providers/AppServiceProvider.php中  boot 添加如下代码

   ```
   <?php
   
   namespace App\Providers;
   
   引入 Schema 类
   use Illuminate\Support\Facades\Schema;
   use Illuminate\Support\ServiceProvider;
   
   class AppServiceProvider extends ServiceProvider
   {
       /**
        * Register any application services.
        *
        * @return void
        */
       public function register()
       {
           //
       }
   
       public function boot()
       {
           Schema::defaultStringLength(191);
       }
   }
   
   ```

   

## 5. 绑定 Profile 表与 user 表之间的关系

#### 1. 什么是关联外键;         

 	一张表 的非主键的字段指向另一张表的主键 那么这个就叫做外键 一张表可以有多个外键
        这边的意思就是 profiles中的admin_id 指向 admin表中的id 那么 admin_id就是关联外键 用于联查
        (这边的admin 就相当于 profiles 的父级关系表)指定外键 通过外键 通知数据库让数据库按照指定的关联外键进行查询 方便数据库索引,提高效率

#### 2. 实现互相关联

```php
在app/user.php下添加方法
    public function profile(){
//        用于绑定 profile 表 与user表之间的关系
        return $this->hasOne(Profile::class);
    }
 
```

然后再 app/profile.php写入

```php
class Profile extends Model
{
//        创建 fillable属性 使得两表关联
//        代表 是其包含字段名 phone admin_id
    protected $fillable = ['phone','admin_id'];   // 重要!!!

    public function user(){
        return $this->belongsTo(User::class);
    }
}
```

