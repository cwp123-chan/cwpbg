---
title:wamp与redis和php的安装
date: 2019-10-20 23:40:49
---
# wamp与redis和php的安装

## 1. 打印phpinfo找到相关配置

```
echo phpinfo()
// Loaded Configuration File 查看php.ini的配置路径 底下要用
// Compiler
// Architecture
// Zend Extension Build
// PHP Extension Build			查看配置
```

## 2.进入官方网站 下载对应文件 

```
php_redis-5.0.2-7.2-ts-vc15-x64 php redis扩展安装包
php_igbinary-3.0.0-7.2-ts-vc15-x64.zip igbinary 扩展安装包
```

## 3. 移动 文件

```
移动文件内 php_redis.dll 与 php_redis.pdb 和 php_igbinary.dll 与
php_igbinary.pdb 文件到 php对应 F:\wampNew64\bin\php\php7.2.18\ext目录下
并 在 phpinfo信息下查询到的Apache加载目录(Loaded Configuration File) 下的路径下的php.ini文件中添加

extension=redis
extension=igbinary
或
extension=php_redis.dll
extension=php_igbinary.dll
```

## 4. 测试(phpinfo中会有redis信息)

```
<?php
    //连接本地的 Redis 服务
   $redis = new Redis();
   $redis->connect('127.0.0.1', 6379);
   echo "Connection to server successfully";
         //查看服务是否运行
   echo "Server is running: " . $redis->ping();
?>


// 也可以用 cmder 在 php7.2.18 目录下运行 php.exe -m 查看apache正在运行的扩展
```

## 5.完毕!