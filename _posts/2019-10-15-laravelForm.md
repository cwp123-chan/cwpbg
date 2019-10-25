---
title: lavrvel form
date: 2019-10-05 23:40:49
---

# lavrvel form

## 1. laravel 分页时 

利用模板

```
public function index()
{	
	// 具体参数见 文档  分页 
	$student = Student :: paginate(2);
	return view ("stuedent",[
		"student"=>$student
	])
}

只需在html 文件 中加入模板引擎
{{ $student->render() }}
```

