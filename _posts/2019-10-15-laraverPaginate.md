---
title:laravel 接收参数进行分页
date: 2019-10-20 23:40:49
---

# laravel 接收参数进行分页

## Eloquent如何进行分页？

Eloquent如何进行分页的办法在官方文档中有详细的解析，http://laravelacademy.org/post/6160.html 大家也可以参考这篇文章。主要是利用两个方法：其一使用`simplePaginate`可以进行简单的分页，我们可以获得前一页和后一页的链接。其二是使用`paginate`方法进行分页，这种方法可以传递参数进行分页，并且返回json格式的数据，本篇文章将详细介绍`paginate`的方法。

## paginate如何运作

假设我们对用户列表进行分页。

```php
Users::paginate();
```

paginate当中可以接收一个参数，这个参数决定每一页显示多少条目数据，比如`paginate(5);`就是每五条数据一页。
如果返回json数据看起来是下图的样子。

```json
{
  "total": 2,
  "per_page": "1",
  "current_page": 1,
  "last_page": 2,
  "next_page_url": "http://localhost/learn_slim/v1/users/page/1/2",
  "prev_page_url": "",
  "from": 1,
  "to": 1,
  "data": [
    {
      "ID": 1,
      "user_login": "admin",
      "user_pass": "123123",
    }
  ]
}
```

total:表示数据的总页数
per_page:表示每一页显示的数据条目
current_page:表示当前页面的页码
last_page:表示最后一页的页码
next_page_url:表示下一页的链接
prev_page_url:表示上一页的链接
from:获取切片中第一个条目
to:获取切片中最后一个条目

## 通过传值方法进行分页查询

上面的让`paginate()`只接收一个参数的办法虽然可以进行简单的分页，但是无法返回指定页面。`paginate()`还可以接收其他参数。

```php
paginate($perPage = null, $columns = ['*'], $pageName = '', $page = null)
```

上面的参数解释
perPage:表示每页显示的条目数量
columns:接收数组，可以向数组里传输字段，可以添加多个字段用来查询显示每一个条目的结果
pageName:表示在返回链接的时候的参数的前缀名称，在使用控制器模式接收参数的时候会用到
page:表示查询第几页及查询页码
其他详细内容可以去查看paginate类里面的内容



![img](https://upload-images.jianshu.io/upload_images/677473-435919867a8c612a.png?imageMogr2/auto-orient/strip|imageView2/2/w/250/format/webp)

paginate类

## 关于返回链接的问题

实际上返回链接是`？=page`的形式，但是我们在编写restful api的时候不会去接收这种形式的参数，而是使用pretty url。这时我们可以修改paginate()输出的数组内容。
我们可以利用`toArray()`获得paginate的数组。`paginate()->toArray()`。
然后利用 `str_replace()`替换掉？=，再重新赋值到数组当中就可以了。