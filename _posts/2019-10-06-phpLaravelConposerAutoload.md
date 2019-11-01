---
title: composer 自动加载原理 
date: 2019-10-06 22:46:49
---
# composer 自动加载原理 

## 1. 以 monolog为例

```
composer require monolog/monolog 加载 monolog框架 	cmd
```



## 2. 创建自动加载文件 test.php

```
<?php 
	require __DIR__ . "/vendor/autoload.php"
	echo "test";	测试文件 符合 prs_4标准
	
	// 使用 Foo.php中的 Foo类
	use Name\Foo;
	$d = new Foo;
	$d->test();
```

### 3. 进入 composer.json 加载 自定义的类文件

```
{
    "require": {
        "monolog/monolog": "^2.0"
    },
	"autoload":{
		"psr_4":{"Name\\":"src/"} // 用于绑定以Name为命名空间的名字与src目录名绑定 以便框架可以找到
	}
}

```

## 4. 创建自定义类文件Foo.php

```
<?php 
	namespace Name;
	class Foo
	{
		public function test(){
			echo "我是真实";
		}
	}
```



