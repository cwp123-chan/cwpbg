---
title: laravel各类报错
date: 2019-09-05 23:40:49
---

# laravel各类报错

## 1: 用ajax请求时 后台数据返回200 数据库 数据更新正常 但是 ajax返回的数据只走error 

原因: laravel 后台 使用var_dump等输出时 ajax 会误以为错误数据 

解决: 取出 打印数据