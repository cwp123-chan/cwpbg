---
title: PHP 多进程概述
date: 2019-09-14 23:40:49
---
# PHP 多进程概述

## 前言

在服务器跑脚本时，避免不了一些耗时任务，使用多进程是必不可少的。而在 PHP5.5 之后，PHP 开始加入了多进程元素，以满足开发需求。

## 注意

1. 实现多进程需要开启的扩展：**pcntl**、 **posix**。
2. Windows 环境下不支持 PHP 的多进程编程，本文主要在 Linux 环境下开发测试

## 一张简单结构图

![img](https:////upload-images.jianshu.io/upload_images/3188339-aa22e91e83bedea9.PNG?imageMogr2/auto-orient/strip|imageView2/2/w/571/format/webp)

多进程示意图

## 主要功能

> 主要参考官方文档 [进程控制](http://php.net/manual/en/book.pcntl.php#book.pcntl)

1. pcntl_fork：创建多进程，调用后会返回两条进程的pid，0 为子进程，大于 0 为父进程**（父进程得到子进程的 id，所以大于 0）**，-1为创建失败

```php
$pid = $pcntlInstall ? pcntl_fork() : 0;

if ($pid == -1) {
    //fork失败
 } elseif ($pid > 0) {
    //父进程
    ......
 } elseif ($pid == 0) {
    //子进程
    ......
 }
```

1. pcntl_signal: 注册一个信号处理回调函数，可以捕获子进程结束时发出的信号

```php
/配合pcntl_signal使用
declare (ticks = 1);

//当子进程退出时，会触发该函数,当前子进程数-1
pcntl_signal(SIGCHLD, function ($signo) {
    switch ($signo) {
        case SIGCHLD:
            echo $curChildPro . 'SIGCHLD', PHP_EOL;
            $curChildPro--;
            break;
    }
});
```

1. pcntl_wait: 用来暂停父进程，等待子进程退出

## 一个多进程的例子

> 该例子主要介绍了如何控制 **同一时刻** 进程 **并发** 的数量

```php
$curChildPro = 0;
$maxChildPro = 5;  // 同一时刻最多 5 个进程

 //配合pcntl_signal使用
declare (ticks = 1);

//当子进程退出时，会触发该函数,当前子进程数-1
pcntl_signal(SIGCHLD, function ($signo) {
    global $curChildPro;
    switch ($signo) {
        case SIGCHLD:
            echo $curChildPro . 'SIGCHLD', PHP_EOL;
             $curChildPro--;
            break;
     }
});

$index  = 0;
while ($index < 10) {
    $index ++;
    $curChildPro++;
    echo "-------- current process" . $curChildPro . "--------\r\n";
    $pid = $pcntlInstall ? pcntl_fork() : 0;
    
    if ($pid == -1) {
        //fork失败
    } elseif ($pid > 0) {
        //达到上限时父进程阻塞等待任一子进程退出后while循环继续
        if ($curChildPro >= $maxChildPro) {
            pcntl_wait($status);
        }
    } elseif ($pid == 0) {
        //子进程   执行一些操作
        ......
        exit(); // 需要退出，避免产生僵尸进程
    }
}
```