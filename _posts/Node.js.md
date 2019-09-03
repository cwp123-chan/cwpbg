# Node.js 

## 1. Javascript中将a.js中的函数模块暴露出去让别的文件进行调用的方法：

```js
‘use strict‘；										#使用严格模式
var a = 'hello';
function greet(name){
    alert(a + ',' + name + '!');
}

module.exports = greet;								#代表将greet方法暴露出去为下面main.js提供f方法；
//// 保存文件名：hello.js
```

```js
'use strict';										#使用严格模式；
// 引入hello模板
var model = require(./hello.js);
greet('张三')；
console.log(model);

```

