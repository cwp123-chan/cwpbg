---
title: php 接收 content-type为 application/json的格式

date: 2019-10-25 23:40:49
---

# php 接收 content-type为 application/json的格式

## 1. 前端

```js
// 通过 ajax 设置 协议头 
axios.post(url，qs.stringify(data),

{headers: {'Content-Type': 'application/json'}}

).then(result => {

// do something

})
```



## 2. 后端

```php
//以laravel为例
<?php

namespace App\Http\Controllers;

use App\cateModel;
use Illuminate\Http\Request;

class cateController extends Controller
{
    public function addCate(Request $request){
        $content = file_get_contents("php://input"); // 接收 application/json 格式的文本
        $content = json_decode($content,true);		//	转为 json
        $data = $content["catagoryName"];			// 获取参数
    }
}
```

