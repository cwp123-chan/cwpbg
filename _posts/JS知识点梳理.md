# JS知识点梳理

## 1. 什么是函数优先原则

```js
foo();//1
var foo;
function foo() {
    console.log( 1 );
}
foo = function() {
    console.log( 2 );
}
```

#### 以上代码 我们的js引擎会按照以下方法进行解析

```js
function foo() {
    console.log( 1 );
}
foo();//1
foo = function() {
    console.log( 2 );
}
// 我们的js引擎会优先找到声明函数的地方 然后再变量声明,等到执行函数的时候 我们的js引擎才会给函数进行赋值
// 函数声明>变量声明;在执行时才会给变量赋值
```

```
我们可以看一下这个例子
函数声明:
解析器会优先读取函数声明,并使得在执行前能够被访问
function sum1(n1,n2){
    return n1+n2;
  };
  
函数表达式，又叫函数字面量:
而函数表达式则必须等到解析器执行到它所在的代码行才会真正被解释执行。
var sum2=function(n1,n2){
    return n1+n2;
};

自执行函数
严格来说 他也是函数表达式
它主要用于创建一个新的作用域，在此作用域内声明的变量，不会和其它作用域内的变量冲突或混淆，大多是以匿名函数方式存在，且立即自动执行。
(function(n1,n2){
    console.log (n1+n2)
})(1,3);//4
```

#### 另外几种自执行函数

```js
//可用来传参
(function(x,y){
  console.log(x+y);
})(2,3);
 
//带返回值
var sum=(function(x,y){
  return x+y;
})(2,3);
console.log(sum);
 
~function(){
  var name='~'
  console.log(name);
}();
 
!function(){
  var name='!'
  console.log(name);
}();
 
;(function(){
  var name=';'
  console.log(name);
})();
 
-function(){
  var name='-'
  console.log(name);
}();
 
//逗号运算符
1,function(){
  var name=',';
  console.log(name);
}();
 
//异或
1^function(){
  var name='^';
  console.log(name);
}();
 
//比较运算符
1>function(){
  var name='>';
  console.log(name);
}();
 
~+-!(function(){
  var name='~+-!';
  console.log(name);
})();
 
~!(function(){
  var name='~!';
  console.log(name);
})();
 
(function(){
  var name='call';
  console.log(name);
}).call();
 
(function(){
  var name='apply';
  console.log(name);
}).apply();
```

```
函数构造法，参数必须加引号:
var sum3=new Function('n1','n2','return n1+n2');
console.log(sum3(2,3));//5
从技术角度讲，这是一个函数表达式。一般不推荐用这种方法定义函数，因为这种语法会导致解析两次代码（第一次是解析常规ECMAScript代码，第二次是解析传入构造函数中的字符串），从而影响性能。
var name='haoxl';
  function fun(){
    var name='lili';
    return new Function('return name');//不能获取局部变量
  }
 console.log(fun()());//haoxl
 Function()构造函数每次执行时都会解析函数主体，并创建一个新的函数对象，所以当在一个循环或频繁执行的函数中调用Function()构造函数效率是非常低的。而函数字面量却不是每次遇到都会重新编译的，用Function()构造函数创建一个函数时并不遵循典型的作用域，它一直把它当作是顶级函数来执行。
```

## 2.什么是基于原型

```
1.什么事obj和Func
Object和Function都作为JS的自带函数，Object继承自己，Funtion继承自己，Object和Function互相是继承对方，也就是说Object和Function都既是函数也是对象。
console.log(Function instanceof Object); // true
console.log(Object instanceof Function); // true

```

```
2.Object 是 Function的实例，而Function是它自己的实例。
console.log(Function.prototype); // ƒ () { [native code] }
console.log(Object.prototype);  // Object
```

#### 2.**普通对象和函数对象**

```
JavaScript中万物皆对象，但对象之间也是有区别的。分为函数对象和普通对象。

函数对象可以创建普通对象，普通对象没法创建函数对象，普通对象JS世界中最低级的小喽啰，啥特权也没有。

凡是通过new Function创建的对象都是函数对象，其他都是普通对象（通常通过Object创建），可以通过typeof来判断。
```

```js
function f1(){};
typeof f1 //"function"
 
 
var o1 = new f1();
typeof o1 //"object"
 
var o2 = {};
typeof o2 //"object"
```

```
以下写法是等价的
function f1(){};  ==  var f1 = new Function();


function f2(a,b){
  alert(a+b);
} 
等价于
 
var f2 = new Function(a,b,"alert(a+b)");
```

```
prototype、_proto_和construetor（构造函数）
```

```js
//举个例子:
var o = {};
  console.log(o.prototype); //undefined
  console.log(o instanceof Object); //true
  console.log(o.__proto__ === Object.prototype) //true
  console.log(Object === Object.prototype.constructor) //true 
  console.log(Object.prototype.constructor) //function Object()
　　console.log(Object.prototype.__proto__); //null
```

```
以上?
1、每一个函数对象都有一个prototype属性，但是普通对象是没有的；

　 prototype下面又有个construetor，指向这个函数。

2、每个对象都有一个名为_proto_的内部属性，指向它所对应的构造函数的原型对象，原型链基于_proto_;

好了,开始上代码和例子,建一个普通对象，我们可以看到

　　1、o的确没有prototype属性

　　2、o是Object的实例

　　3、o的__proto__指向Object的prototype

　　4、Object.prototype.constructor指向Object本身
```

```js
//实例?
function Demo(){};
  var f1 = new Demo();
  console.log(f1.prototype); //undefined
  console.log(f1 instanceof Demo); //true
  console.log(f1.__proto__ === Demo.prototype); //true
  console.log(Demo === Demo.prototype.constructor) ;//true
  console.log(Demo.prototype.__proto__ === Object.prototype) ;//true
  console.log(Object.prototype.__proto__); //null
```

```
1、demo是函数对象，f1还是普通对象

2、f1是Demo的实例

3、demo的原型prototype的__proto__指向Object的原型prototype,而Object的原型prototyped的__proto__指向null;
```

### 原型链

```
原型链

javascript中，每个对象都会在内部生成一个proto 属性，当我们访问一个对象属性时，如果这个对象不存在就回去proto 指向的对象里面找，一层一层找下去，这就是javascript原型链的概念。

f1.__proto__ ==> Demo.prototype ==> Demo.prototype.__proto__ ==> Object.prototype ==> Object.prototype.__proto__ ==> null

JS中所有的东西都是对象,所有的东西都由Object衍生而来, 即所有东西原型链的终点指向null
```

## 3. JS多范式编程

####  和C++语言一样，JavaScript语言也支持多范式编程。我们常常混合使用这些范式来完成一些大型的Web项目的开发。JavaScript支持的编程范式有：命令式函数式面向对象命令式（Imperative JavaScript）命令式可以理解成面 ...

```

和C++语言一样，JavaScript语言也支持多范式编程。我们常常混合使用这些范式来完成一些大型的Web项目的开发。JavaScript支持的编程范式有：

命令式

函数式

面向对象

命令式（Imperative JavaScript）

命令式可以理解成面向过程，是简单的从上至下完成任务，流水账式的编程风格。如下图所示代码：

这样编写代码比较简单，但存在一些明显的问题，比喻可能存在函数名描述性差、命名冲突、变量冗余、复用性差等。

函数式（Functional JavaScript）

示例代码如下：

可以看到，通过数组对象的内置原型方法map和reduce，大大简化了代码，并且如果熟悉了这种风格，其代码的自描述性也很棒。

map和reduce都是高阶函数（能接受函数作为参数，也能返回函数对象）。map函数用来把一组数据依次变换成另外一组数据，而reduce用来汇总数据，下图是对这两个关键函数有趣而直白的描述：

面向对象（Object-Oriented JavaScript）

面向对象的核心思想是把任务抽象成一个（或一组）对象的数据和操作，在C++语言中，这些抽象出来的对象可以便于接口或实现的复用（继承）以及访问控制，在JS中由于没有接口的概念，只能是函数实现的复用。

示例代码如下：

上面的代码创建了一个Task对象，包含公开属性nums，并通过原型定义了2个方法：square和sum，可以在多个对象之间共享，这样将节约内存。另外代码被包含在一个IIAF（immediately-invoked function expression）中，而不再是全局的。除了更符合OO的思维方式外，似乎代码并没有变得更为简洁。

小结

命令式编程是最基础的方式，面向对象的方式适合对现实世界进行抽象封装以及实现访问控制，函数式能最大程度的精简代码且适用于并行计算，而纯净函数（pure function）具有很好的移植性。
```

## 4. LET问题

```
举个例子
var foo = 1;
function demo (){
var foo2 = 1;							//	var 的作用域在function之内
let foo3 = 5;							// let 作用域在{}内 也就是说 其在函数内为自己开辟了一个作用空间 且 作用范围比var 更小
}

// 以上可以知道 单独的一个{} 里面可以写let 进行声明 如{let a = 0;}	
// 在函数外部能够访问var 的时候 却不能够访问let 就可以让变量定义更加简单 不重复

```

## 5. JS支持的变量名

```
大致的不说 这里说一些特殊的
中文可以作为变量名
var 陈伟平 = "帅哥";	
var $asd = "66";
甚至在ios中 表情包中符号允许作为变量名存在如
var O(∩_∩)O~~ = '哈哈';
```

## 6. mysql中真正的编码使用

```
在以后mysql编码 实际上 utf8mb4   才是真正意义上的 utf8;
```

## 7. js中 变量声明提前问题

```
var a = 10;
function demo (){
	console.log(a);		// underfied
	var a = 20;
}
demo();						
```

```
在执行中 js引擎会先声明函数demo 然后 定义全局变量a 和局部变量a 
但是 没有对其赋值 (只有在执行过程才会赋值)
接着开始执行  当执行到 console中是 因为 已经定义了局部变量 所以 不会向上继续找 但是因为没有赋值 所以就输出 为underfined;
```

## 8. 宿主问题

#### 在js中 像console / alert pareseInt 等等 都不是js对象函数,只要不是什么 a.b()的基本上都不是js标准规定的     

#### 这些函数其实都是浏览器的也就是以浏览器为宿主进行定义的 不同的浏览器宿主就不一样;

## 9. 顶层对象

```
举个例子
var myvar = "foo";

console.log(myvar)
console.log(window.myvar)
console.log(globalThis.myvar)
C. foo foo foo

因为在顶层对象中 存放的都是一些没有宿主对象的函数 那么 这些函数就被当成孤儿 留在顶层对象中 不同浏览器都不同
chrome 是window 所以
但是最新标准就是 globalThis是所有浏览器统一的顶层对象;
```

## 10.八种数据类型

```
boolean、number
null、undefined
bigint、string
symbol、object

bigint // 代表最大的整数 如:4n  , 3n等等 他不能与number相加 4n不能和4相加 但是 4n+4n = 8n
symbol // es6 新加
例子:
var obj = {};
const U = symbol();
obj(U) = {}
规定一个常量为U  然后放入obj对象中 如果两次调用这个对象下的U方法 主要是symbol定义的u 是不可能取出一抹一样的对象的
也就是说 symbol生成的是一个不可能重复的值;
```

## 11.函数的深拷贝

```
JavaScript这门语言，有两种数据类型，一种是基本数据类型，包括（number，string，null，undefined，Boolean，symbol），另一种是复杂数据类型包括Object；

而对于基本数据类型来说，复制一个变量值，本质上就是copy了这个变量。一个变量值的修改，不会影响到另外一个变量。看一个简单的例子。

let val = 123;
let copy = val;
console.log(copy);  //123
val = 456;          //修改val的值对copy的值不产生影响
console.log(copy);  //123

而复杂数据类型的复制与基本数据类型的复制不同，当我们将复杂数据类型的变量复制给另外一个变量时，实际是对指针的复制，两个变量的指向地址是相同的，所以当修改一个对象变量名的值时，会对另外一个对象的变量名的值产生影响。所以这里就要谈到深拷贝，深拷贝不会对原对象产生影响，深拷贝的方法：JSON.parse(JSON.stringify(obj));即先把json对象转换为json字符串，然后在把json字符串转换为json对象。另外一种方法是用Object.assign()这种方法，我还没有测试是否可行。
```

## 12.JS中的switch不同于PHP要求 参数类型也要一抹一样

```js
var n = "2"
switch(n){
    case 1:
        console.log("星期一")
    case 2:
        console.log("星期二")
}

//什么也不输出
```

## 13.for in / for of

```js
var data = ['a','b','c','d']
for(var i in [1,2,3]){
    console.log(data[i])
}
// abc



let arr = [3, 5, 7];
arr.foo = "hello";

for (let i of arr) {
   console.log(i)
}
//3 5 7 hello


//相当于 php中 foreach 
//for in 取出数组的下标keys
//for of 取出数组的值 values

```

## 14 . let 与 list 

#### set 要求对象集合里面的每一个值都必须唯一

#### list 要求可以重复值 但是顺序不能变

```
[a=1,b=3,a=4] 这里面就相当于一个set集合 不允许key值发生重复
set 可以用于高并发 也可以用于加锁
比如:
当两个同样的请求同时进行 我们放入set 集合里 那么set 不允许两个同时的值出现 所以就让高并发不至于出错
也可以用加锁 在每次请求进入时 给其带把锁 如果 请求结束就给下一个请求加锁...

var s = new Set()
s.add(1)
s.add(1)
console.log(s.size)
// 1

```

## 15 . ES6几种简写

```

var name = 'mary'
var foo = {
    name:'jack',
    say1:function(){console.log(this.name)},   	// jack
    say2(){console.log(this.name)},				//jack
    say3:()=>{console.log(this.name)},			// mary
}
```

```
say:function(){}		======  	say(){}
在这里 所有的函数都是有this的 如果 this没有特殊指代 那么就指代这个对象本身
但是:
say3:()=>{};	箭头函数是没有this的 因此 他会向上一级查找,知道找到全局中的name

经常使用箭头函数 会方便我们因为局部定义变量而找不到变量位置的麻烦

```

## 16 . ES6新曾

```
var a = "b"
var user = {
    a,
    [a]:"c",
    "c":"d"
}
console.log(user.a)
console.log(user.b)
console.log(user.c)
console.log(user[a])
//bcdc
```

```
var a = "b";
var user = {
	a,				// 这边的a在ES6新加 a等同于a:a;后因为 a的值为b所以就是b
}

[a] 相当于php中$的意思 解析变量
[a] 等价与 先解析a的值为"b" 然后 为"b":"c"
["a"]	如果这样子就不解析 也就"a" : "c";
```

## 其他新加标准:

ES6:

```
1.
function test (a,b,...c){};
test(1,2,3,4,5,6,7);
其中 参数a为1,参数b为2 [3,4,5,6,7] 为c

2.
var [a,,b] = [1,2,3]
其中 a=1 ;  b=3 ; '' 为2;		这边空字符串为2;

```

```
对象
1.  
var obj = {color:red , size: 19px}
var [color,size] = obj; 
那么 console.log(color)  // red
那么 console.log(size)   // 19px				可以用数组一起赋值

2. 
var obj2 = {...obj,height:13px}				
也可以一个对象放入另一个对象中 增加对象属性或方法等...
```



## 附上题:

1. 以下说法正确的是________。abcd

   ```
   A. js是跨平台、面向对象的动态编程语言
   B. js是一种具有函数优先的解释型编程语言
   C. js是基于原型、多范式的编程语言
   D. js的核心语言叫ECMAScript
   ```

2. 以下说法正确的是________。abcd

   ```
   A. js能运行在浏览器中
   B. js能运行在服务端
   C. js能绘制出动态图像
   D. js具有基于事件循环的并发模型
   ```

3. 下面代码运行结果________。c

   ```
   var userName = "jack"
   console.log(username)
   
   A. 输出:jack
   B. 报错:缺少分号
   C. 报错: username is not defined
   D. 报错: console.log is not a function
   ```

4. 下面代码运行结果________。a

   ```
   if(true){
       var foo = 1
       let bar = 2
   }
   
   console.log(foo)
   console.log(bar)
   
   A. 输出 1 然后报错 bar is not defined
   B. 输出 1 2
   C. 报错 foo is not defined 然后输出 2
   D. 没有输出，只报错 bar is not defined
   ```

5. 下面代码运行结果________。c

   ```
   var a = 1
   var a = 2
   {
       let a = 3
   }
   console.log(a)
   
   A. Identifier 'a' has already been declared
   B. Uncaught SyntaxError: Unexpected identifier
   C. 输出2
   D. 输出3
   ```

6. 下面代码运行结果________。a

   ```
   const PI = 3.14
   function test(){
       var PI = 3.14159
   }
   test()
   console.log(PI)
   
   A. 输出 3.14
   B. 输出 3.14159
   C. Identifier 'PI' has already been declared
   ```

7. 下面代码运行结果________。d

   ```
   let foo = 1
   const bar = 2
   foo = 3
   bar = 4
   console.log(foo)
   console.log(bar)
   
   A. 输出 3 4
   B. 输出 3 2
   C. 输出 1 2
   D. 报错 Assignment to constant variable
   ```

8. 下面代码能正确运行的有________。ab

   ```
   A. var 姓名 = "张三";       console.log(姓名)
   B. var $name = "李四";     console.log($name)
   C. var 7niu = "王五";      console.log(7niu)
   D. var user-name = "赵六"; console.log(user-name)
   ```

9. 下面代码运行结果________。b

   ```
   var foo
   console.log(foo)
   
   A. null
   B. undefined
   C. false
   D. None
   ```

10. 下面代码运行结果________。a

    ```
    var myvar = "foo"
    
    function test(){
      console.log(myvar)
      var myvar = "bar"
    }
    test()
    
    A. undefined
    B. null
    C. foo
    D. bar
    ```

11. 下面代码在Chrome中运行结果________。c

    ```
    var myvar = "foo";
    
    console.log(myvar)
    console.log(window.myvar)
    console.log(globalThis.myvar)
    
    A. foo undefined undefined
    B. foo null null
    C. foo foo foo
    D. foo 
        window.myvar is not defined 
        globalThis.myvar is not defined 
    ```

12. 最新的 ECMAScript 标准定义的数据类型，选出正确的________。abd

    ```
    A. boolean、number
    B. null、undefined
    C. int、string
    D. symbol、object
    ```

13. 下面代码运行结果________。d

    ```
    var a = 1
    var b = 2
    var c = "3"
    console.log(a+b+c)
    console.log(a+c-b)
    
    A. 6   2
    B. 123 2
    C. 15  11
    D. 33  11 
    ```

14. 下面代码运行结果________。d

    ```
    var n = "2"
    switch(n){
        case 1:
            console.log("星期一")
        case 2:
            console.log("星期二")
    }
    
    A. 星期一
    B. 星期二
    C. 星期一 星期二
    D. 什么也不输出
    ```

15. 下面代码运行结果________。a

    ```
    function test(n){
        try{
            throw new Error("foo")
            console.log(n)
        }catch(e){
            console.log(e.message)
            return
        }finally{
            console.log("{$n}")
        }
    }
    
    test("bar");
    
    A. foo {$n}
    B. foo bar
    C. bar foo
    D. bar foo {$n}
    ```

16. 下面代码运行结果________。c

    ```
    for(var i=0;i<=3;i++){
        if(i==2){
            continue
        }
        console.log(i)
    }
    
    A. 0 1
    B. 0 1 2
    C. 0 1 3
    D. 0 2 3
    ```

17. 下面代码运行结果________。a

    ```
    var data = ['a','b','c','d']
    for(var i in [1,2,3]){
        console.log(data[i])
    }
    
    A. abc
    B. bcd
    C. 123
    C. 012
    ```

18. 下面代码运行结果________。d

    ```
    let arr = [3, 5, 7];
    arr.foo = "hello";
    
    for (let i of arr) {
       console.log(i)
    }
    
    A. 0 1 2 foo
    B. 3 5 7
    C. 0 1 2 hello
    D. 3 5 7 hello
    ```

19. 下面代码能正确输出3的是________。b

    ```
    let arr = [3, 5, 7];
    A. console.log(arr.count)
    B. console.log(arr.length)
    C. console.log(len(arr))
    D. console.log(size(arr))
    ```

20. 下面代码运行结果________。b

    ```
    var s = new Set()
    s.add(1)
    s.add(1)
    console.log(s.size)
    
    A. 0
    B. 1
    C. 2
    D. 报错
    ```

21. 下面代码运行结果________。c

    ```
    var name = 'mary'
    var foo = {
        name:'jack',
        say1:function(){console.log(this.name)},
        say2(){console.log(this.name)},
        say3:()=>{console.log(this.name)},
    }
    
    foo.say1()
    foo.say2()
    foo.say3()
    
    A. jack mary undefined
    B. jack jack jack
    C. jack jack mary
    D. jack jack undefined
    ```

22. 下面代码运行结果________。a

    ```
    var a = "b"
    var user = {
        a,
        [a]:"c",
        "c":"d"
    }
    console.log(user.a)
    console.log(user.b)
    console.log(user.c)
    console.log(user[a])
    
    A. b c d c
    B. b undefined d b
    C. a undefined d undefined
    D. b c d undefined
    ```

23. 下面代码运行结果________。d

    ```
    const p1 = new Promise(function (resolve, reject) {
        setTimeout(() => resolve("data1"), 2000)
    })
    
    const p2 = new Promise(function (resolve, reject) {
        setTimeout(() => reject(new Error('fail')), 1000)
    })
    
    Promise.all([p1, p2]).then(result => {
        console.log(result[0])
        console.log(result[1])
    }).catch(error => console.log(error.message))
    
    A. data1 fail
    B. fail data1
    C. fail fail
    D. fail
    ```

24. 下面代码能正确将该div的类名改为active的是________。b

    ```
    <div id="foo" class="invalid">
    
    A. document.getElementById('foo').class="active"
    B. document.getElementById('foo').className="active"
    C. document.getElementById('#foo').className="active"
    D. document.getElementById('#foo').class="active"
    ```

25. 下面代码运行结果________。b

    ```
    var f = (function(i){
        console.log(i)
        i++
        return function(i){
            console.log(i)
        }
    })
    
    f(2)(3)
    
    A. 2 2
    B. 2 3
    C. 2 4
    D. 3 4
    ```