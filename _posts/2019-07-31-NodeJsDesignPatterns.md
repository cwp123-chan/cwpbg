---
title: Nodejs的几大设计模式
date: 2019-07-31 22:46:49
---
# Nodejs的几大设计模式

## 1. 工厂模式

### 特点：

### 能接受三个参数，可以无数次调用这个函数，每次返回都会包含三个属性和一个方法的对象。

```js
function CreatePerson(name,age,sex) {
    var obj = new Object();
    obj.name = name;
    obj.age = age;
    obj.sex = sex;
    obj.sayName = function(){
        return this.name;
    }
    return obj;
}
var p1 = new CreatePerson("longen",'28','男');
var p2 = new CreatePerson("tugenhua",'27','女');
console.log(p1.name); // longen
console.log(p1.age);  // 28
console.log(p1.sex);  // 男
console.log(p1.sayName()); // longen
 
console.log(p2.name);  // tugenhua
console.log(p2.age);   // 27
console.log(p2.sex);   // 女
console.log(p2.sayName()); // tugenhua
 
// 返回都是object 无法识别对象的类型 不知道他们是哪个对象的实列
console.log(typeof p1);  // object
console.log(typeof p2);  // object
console.log(p1 instanceof Object); // true

```

### **优点：**能解决多个相似的问题。

### **缺点：**不能知道对象识别的问题(对象的类型不知道)。

### 复杂的工厂模式

```js

// 定义自行车的构造函数
var BicycleShop = function(){};
BicycleShop.prototype = {
    constructor: BicycleShop,
    /*
    * 买自行车这个方法
    * @param {model} 自行车型号
    */
    sellBicycle: function(model){
        var bicycle = this.createBicycle(mode);
        // 执行A业务逻辑
        bicycle.A();
 
        // 执行B业务逻辑
        bicycle.B();
 
        return bicycle;
    },
    createBicycle: function(model){
        throw new Error("父类是抽象类不能直接调用，需要子类重写该方法");
    }
上面是定义一个自行车抽象类来编写工厂模式的实列，定义了createBicycle这个方法，但是如果直接实例化父类，调用父类中的这个createBicycle 方法,会抛出一个error，因为父类是一个抽象类，他不能被实列化，只能通过子类来实现这个方法，实现自己的业务逻辑，下面我们来定义子类，我们学会如何使用工厂模式重新编写这个方法，首先我们需要继承父类中的成员，然后编写子类 ;如下代码：

// 定义自行车的构造函数
var BicycleShop = function(name){
    this.name = name;
    this.method = function(){
        return this.name;
    }
};
BicycleShop.prototype = {
    constructor: BicycleShop,
    /*
     * 买自行车这个方法
     * @param {model} 自行车型号
    */
    sellBicycle: function(model){
            var bicycle = this.createBicycle(model);
            // 执行A业务逻辑
            bicycle.A();
 
            // 执行B业务逻辑
            bicycle.B();
 
            return bicycle;
        },
        createBicycle: function(model){
            throw new Error("父类是抽象类不能直接调用，需要子类重写该方法");
        }
    };
    // 实现原型继承
    function extend(Sub,Sup) {
        //Sub表示子类，Sup表示超类
        // 首先定义一个空函数
        var F = function(){};
 
        // 设置空函数的原型为超类的原型
        F.prototype = Sup.prototype; 
 
        // 实例化空函数，并把超类原型引用传递给子类
        Sub.prototype = new F();
                    
        // 重置子类原型的构造器为子类自身
        Sub.prototype.constructor = Sub;
                    
        // 在子类中保存超类的原型,避免子类与超类耦合
        Sub.sup = Sup.prototype;
 
        if(Sup.prototype.constructor === Object.prototype.constructor) {
            // 检测超类原型的构造器是否为原型自身
            Sup.prototype.constructor = Sup;
        }
    }
    var BicycleChild = function(name){
        this.name = name;
// 继承构造函数父类中的属性和方法
        BicycleShop.call(this,name);
    };
    // 子类继承父类原型方法
    extend(BicycleChild,BicycleShop);
// BicycleChild 子类重写父类的方法
BicycleChild.prototype.createBicycle = function(){
    var A = function(){
        console.log("执行A业务操作");    
    };
    var B = function(){
        console.log("执行B业务操作");
    };
    return {
        A: A,
        B: B
    }
}
var childClass = new BicycleChild("龙恩");
console.log(childClass);
实例化子类，然后打印出该实例, 如下截图所示：



console.log(childClass.name);  // 龙恩

// 下面是实例化后 执行父类中的sellBicycle这个方法后会依次调用父类中的A

// 和B方法；A方法和 B方法依次在子类中去编写具体的业务逻辑。

childClass.sellBicycle("mode"); // 打印出  执行A业务操作和执行 B业务操作

上面只是"龙恩"自行车这么一个型号的，如果需要生成其他型号的自行车的话，可以编写其他子类，工厂模式最重要的优点是：可以实现一些相同的方法，这些相同的方法我们可以放在父类中编写代码，那么需要实现具体的业务逻辑，那么可以放在子类中重写该父类的方法，去实现自己的业务逻辑；使用专业术语来讲的话有 2点：第一：弱化对象间的耦合，防止代码的重复。在一个方法中进行类的实例化，可以消除重复性的代码。第二：重复性的代码可以放在父类去编写，子类继承于父类的所有成员属性和方法，子类只专注于实现自己的业务逻辑。



```





## 2.单体模式；

```
单体模式的基本结构：

// 单体模式
var Singleton = function(name){
    this.name = name;
    this.instance = null;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
function getInstance(name) {
    if(!this.instance) {
        this.instance = new Singleton(name);
    }
    return this.instance;
}
// 测试单体模式的实例
var a = getInstance("aa");
var b = getInstance("bb");
// 因为单体模式是只实例化一次，所以下面的实例是相等的

console.log(a === b); // true

由于单体模式只实例化一次，因此第一次调用，返回的是a实例对象，当我们继续调用的时候，b的实例就是a 的实例，因此下面都是打印的是aa；

console.log(a.getName());// aa

console.log(b.getName());// aa

上面的封装单体模式也可以改成如下结构写法：

// 单体模式
var Singleton = function(name){
    this.name = name;
};
Singleton.prototype.getName = function(){
    return this.name;
}
// 获取实例对象
var getInstance = (function() {
    var instance = null;
    return function(name) {
        if(!instance) {
            instance = new Singleton(name);
        }
        return instance;
    }
})();
// 测试单体模式的实例
var a = getInstance("aa");
var b = getInstance("bb");
// 因为单体模式是只实例化一次，所以下面的实例是相等的

console.log(a === b); // true

console.log(a.getName());// aa

console.log(b.getName());// aa

理解使用代理实现单列模式的好处
    比如我现在页面上需要创建一个div的元素，那么我们肯定需要有一个创建 div的函数，而现在我只需要这个函数只负责创建div元素，其他的它不想管，也就是想实现单一职责原则，就好比淘宝的kissy 一样，一开始的时候他们定义kissy只做一件事，并且把这件事做好，具体的单体模式中的实例化类的事情交给代理函数去处理，这样做的好处是具体的业务逻辑分开了，代理只管代理的业务逻辑，在这里代理的作用是实例化对象，并且只实例化一次;  创建div代码只管创建div，其他的不管；如下代码：

// 单体模式
var CreateDiv = function(html) {
    this.html = html;
    this.init();
}
CreateDiv.prototype.init = function(){
    var div = document.createElement("div");
    div.innerHTML = this.html;
    document.body.appendChild(div);
};
// 代理实现单体模式
var ProxyMode = (function(){
    var instance;
    return function(html) {
        if(!instance) {
            instance = new CreateDiv("我来测试下");
        }
        return instance;
    } 
})();
var a = new ProxyMode("aaa");
var b = new ProxyMode("bbb");
console.log(a===b);// true
理解使用单体模式来实现弹窗的基本原理

下面我们继续来使用单体模式来实现一个弹窗的demo；我们先不讨论使用单体模式来实现，我们想下我们平时是怎么编写代码来实现弹窗效果的; 比如我们有一个弹窗，默认的情况下肯定是隐藏的，当我点击的时候，它需要显示出来；如下编写代码：

// 实现弹窗
var createWindow = function(){
    var div = document.createElement("div");
    div.innerHTML = "我是弹窗内容";
    div.style.display = 'none';
    document.body.appendChild('div');
    return div;
};
document.getElementById("Id").onclick = function(){
    // 点击后先创建一个div元素
    var win = createWindow();
    win.style.display = "block";
}
如上的代码；大家可以看看，有明显的缺点，比如我点击一个元素需要创建一个div，我点击第二个元素又会创建一次div，我们频繁的点击某某元素，他们会频繁的创建div的元素，虽然当我们点击关闭的时候可以移除弹出代码，但是呢我们频繁的创建和删除并不好，特别对于性能会有很大的影响，对DOM频繁的操作会引起重绘等，从而影响性能；因此这是非常不好的习惯；我们现在可以使用单体模式来实现弹窗效果，我们只实例化一次就可以了；如下代码：

// 实现单体模式弹窗
var createWindow = (function(){
    var div;
    return function(){
        if(!div) {
            div = document.createElement("div");
            div.innerHTML = "我是弹窗内容";
            div.style.display = 'none';
            document.body.appendChild(div);
        }
        return div;
    }
})();
document.getElementById("Id").onclick = function(){
    // 点击后先创建一个div元素
    var win = createWindow();
    win.style.display = "block";
}
理解编写通用的单体模式

上面的弹窗的代码虽然完成了使用单体模式创建弹窗效果，但是代码并不通用，比如上面是完成弹窗的代码，假如我们以后需要在页面中一个iframe呢？我们是不是需要重新写一套创建iframe的代码呢？比如如下创建iframe：

var createIframe = (function(){
    var iframe;
    return function(){
        if(!iframe) {
            iframe = document.createElement("iframe");
            iframe.style.display = 'none';
            document.body.appendChild(iframe);
        }
        return iframe;
    };
})();
我们看到如上代码，创建div的代码和创建iframe代码很类似，我们现在可以考虑把通用的代码分离出来，使代码变成完全抽象，我们现在可以编写一套代码封装在getInstance函数内，如下代码：

var getInstance = function(fn) {
    var result;
    return function(){
        return result || (result = fn.call(this,arguments));
    }
};
如上代码：我们使用一个参数fn传递进去，如果有result这个实例的话，直接返回，否则的话，当前的getInstance函数调用fn这个函数，是this指针指向与这个fn这个函数；之后返回被保存在result里面；现在我们可以传递一个函数进去，不管他是创建div也好，还是创建iframe也好，总之如果是这种的话，都可以使用getInstance来获取他们的实例对象；

如下测试创建iframe和创建div的代码如下：

// 创建div
var createWindow = function(){
    var div = document.createElement("div");
    div.innerHTML = "我是弹窗内容";
    div.style.display = 'none';
    document.body.appendChild(div);
    return div;
};
// 创建iframe
var createIframe = function(){
    var iframe = document.createElement("iframe");
    document.body.appendChild(iframe);
    return iframe;
};
// 获取实例的封装代码
var getInstance = function(fn) {
    var result;
    return function(){
        return result || (result = fn.call(this,arguments));
    }
};
// 测试创建div
var createSingleDiv = getInstance(createWindow);
document.getElementById("Id").onclick = function(){
    var win = createSingleDiv();
    win.style.display = "block";
};
// 测试创建iframe
var createSingleIframe = getInstance(createIframe);
document.getElementById("Id").onclick = function(){
    var win = createSingleIframe();
    win.src = "http://cnblogs.com";
};

```

