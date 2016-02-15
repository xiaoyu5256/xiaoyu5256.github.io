title: JavaScript模式
date: 2016-02-05 09:13:48
categories:
- 前端
- JavaScript
tags:
- 读书笔记
- JavaScript
- 前端
---


## 第二章 基本技巧
---

### 编写可维护的代码

> 什么是易维护的代码
> * 阅读性好
> * 具有一致性
> * 预见性好
> * 看起来如同一个人编写的
> * 有文档

### 尽量少用全局变量

链式赋值产生全局变量:
```javascript    
    function foo(){
        var a=b=0  //b为全局变量
    }
```
    
    
### delete 问题

    1. 通过var声明的全局变量，无法delete
    2. 非通过var声明的全局变量，可以delete

### 单一var模式

只使用一个var在函数顶部定义变量
好处：
    1. 提供一个单一的地址，查找所有的局部变量
    2. 防止出现变量在定义前就被使用的逻辑错误
    3. 帮助牢记要声明的变量，以尽少地使用全局变量
    4. 更少地编码
    
单一var模式：
```javascript
    function func(){
        var a = 1,
            b = 2,
            c =3;
        //函数体 
    }
```
   
### for 循环

缓存dom数组长度：
```javascript
    for(var i= 0,len = domarray.length; i < len,;i++){
        //对domarray[i] 进行处理
    } 
```


使用`i++`代替：

```javascript
    i=i+1;
    i+=1;
```
    
少变量的形式：
```javascript
    var i,arr=[];
    for(i=arr.length;i--;){
        //处理 arr[i]
    }
```
        
使用while：
```javascript
    var arr=[],i=arr.length;
    while(i--){
        //处理arr[i]
    }
```
    
### for in 循环

使用`hasOwnProperty`对`prototype`中的属性进行过滤
    
### 不要增加内置对象的prototype

### 避免使用隐式类型转换

### 避免使用eval
如果实在要使用`eval`,可以使用`Function`代替：
```javascript
    new Function("alert('dd')")()
```
Function类似一个沙盒，仅能看到全局作用域，不影响local作用域


### 使用`parseInt`时，指定进制,如
```javascript
    parseInt('010',10);
```
    
### 编写api文档
* JSDoc Toolkit
* YUIDoc

## 第三章 字面量和构造函数
---
### 优先选择字面量，而不是构造函数

### 自定义构造函数            

使用new 调用构造函数时，函数内部发生如下情况：
    1. 创建一个空对象，作为函数的上下文，同时继承了函数的原型
    2. 执行函数，返回刚创建的对象（如果构造函数显式返回除外）
    3. 如果显式返回对象的话，显式对象不会继承函数原型
    
如果构造函数中使用了this,但是没有使用new创建对象，将导致this指向全局变量。（严格模式中，this不指向全局变量），这样会引起错误。
解决方法一：
```javascript
    function MyClass(){
        var that = {};
        that.name = "that";
        return that;
    }
    MyClass.prototype.pname = "pname";
```
这种方法无法继承MyClass的prototype
    

解决方法二（自调用构造函数）：
```javascript
    function MyClass(){
       if(!(this instanceof MyClass)){
           return new MyClass();
       }
       this.name ="this";
    }
    MyClass.prototype.pname = "pname";
```
        
### 数组字面量和构造函数
Array构造函数会被误用，如：
```javascript
    new Array(3) //3个空元素的数组
    new Array(3.14) //会报错
    new Array(3,3) //[3,3] 
```
因此建议使用数组字面量，可以使用构造的函数的地方
```javascript
    new Array(10).join("=") //创建10个=的字符串
```

### 检查是否是数组
ECMAScript 5使用`Array.isArray`检查，对于老版本的，可以使用`Object.prototype.toString`来检查,如：
```javascript
    if(typeof Array.isArray === 'undefined' ){
        Array.isArray = function(arg){
            return Object.prototype.toString.call([]) === '[object Array]';
        }
    }
```
    
### Json
json和js字面量的区别：json的属性名必须带双引号，json不能使用函数和正则表达式字面量。

解析json字符串:
```javascript
    JSON.parse //(ECMASciprt 5) 
    json.org库 //(老版本浏览器)
    $.parseJSON() //jQuery
```

序列化对象：
```javascript
    $.stringify() //jQuery将对象序列化为json字符串
```
        
### 正则表达式字面量
字面量只有在第一次解析的时候才创建一个对象，后面再次调用，使用的是同一个对象，但是这个问题在新版本的浏览器中得到修正。
    
### 基本类型包装器
基本类型可以直接调用对应包装类型的方法，但是无法给基本类型添加属性。因为解析器会为基本类型创建一个临时包装类型，操作结束后，包装类型是转换成基本类型。因此给基本类型添加属性时不报错，但是获取不到属性。如果没有使用new，包装器将返回基本类型：
```javascript
    typeof Number(1)  //number
    typeof Number("1")  //number
```
    
### 错误对象
可以throw任意类型的对象，如：
```javascript
    throw{
        name : "Error",
        message : "出错了",
        func : function(){//函数}
    }
```
或者：
```javascript
    throw new Error("出错了");
```
或者：
```javascript
    throw new Error("出错了");     
```

## 第四章 函数
---
### 自定义函数
在一个函数里，将原函数改写，可以在原函数里完成一个初始化的工作，改写后的函数更简单，以提高性能，如：
```javascript
    var rename = function(){
        //做初始化的一些工作
        rename = function(){
            console.log("我更简单");
        }
    }
```

### 即时函数（自调用函数，自执行函数）
好处：避免全局作用域被临时变量污染，通过即时函数分模块。
```javascript
    (function())();
    (function(){});
```
   
### 即时对象初始化
好处：也可以避免全局作用域被临时变量污染。适合一次性的任务。
缺点：不能有效的压缩
```javascript
    ({
        width:500,
        height:500,
        init : function(){
            //做初始化工作
        }
    }).init();
```
或者：
```javascript
    ({}.init());
```
 

### 初始化分支

一旦分支确定了，后面再执行就不会改变了，比如ajax中，IE为ActiveX对象，Chrome为XMLHttpRequest,第一次确定以后，后面的ajax请求肯定是使用同样的对象，这样就可以通过初始化分支来解决，而不用每次都判断。
如事件的处理(也可用自定义函数实现)：
```javascript
    // the interface 
    var utils = {
        addListener: null,
        removeListener: null 
    };
    // the implementation
    if (typeof window.addEventListener === 'function'){
        utils.addListener = function (el, type, fn) {
            el.addEventListener(type, fn, false); 
        };
        utils.removeListener = function (el, type, fn) {
            el.removeEventListener(type, fn, false); 
        };
    } else if (typeof document.attachEvent === 'function') { 
        // IE 
        utils.addListener = function (el, type, fn) {
            el.attachEvent('on' + type, fn); 
        };
        utils.removeListener = function (el, type, fn) {
            el.detachEvent('on' + type, fn); 
        };
    } else { 
        // older browsers
        utils.addListener = function (el, type, fn) {
            el['on' + type] = fn; 
        };
        utils.removeListener = function (el, type, fn) {
            el['on' + type] = null; 
        };
    }
```

### 函数属性 --备忘模式
可以通过函数的length属性来获取参数的个数:
```javascript
    function func(a,b,c){}
    console.log(func.length) //3
```

备忘模式：可以在任何时候给函数添加自定义属性，比如可以添加cache属性到函数，将函数执行结果缓存cache中，下次不用计算，直接取结果。如：
```javascript
    var myFunc = function (param) {
        if (!myFunc.cache[param]) {
            var result = {};
            // ... expensive operation ... 
            myFunc.cache[param] = result;
        }
        return myFunc.cache[param]; 
    };
    // cache storage 
    myFunc.cache = {};
```
多个参数：
```javascript
    var myFunc = function () {
        var cachekey = JSON.stringify(Array.prototype.slice.call(arguments)),
             result;
        if (!myFunc.cache[cachekey]) { 
            result = {};
            // ... expensive operation ...
            myFunc.cache[cachekey] = result; 
        }
        return myFunc.cache[cachekey]; 
    };
    // cache storage 
    myFunc.cache = {};
```

### Curry
通用的curry化函数：
```javascript
    function schonfinkelize(fn) {
        var slice = Array.prototype.slice,
             stored_args = slice.call(arguments, 1);            return function () {
            var new_args = slice.call(arguments), 
            args = stored_args.concat(new_args);
            return fn.apply(null, args);
        };
    }
```

何时使用curry化函数：
    当调用一个函数的时候，发现大部分参数是相同的，这个时候curry化生成一个使用起来更简单的函数。
        
## 对象创建模式
---
### 命名空间
```javascript
    var MYAPP = MYAPP || {};
    MYAPP.namespace = function (ns_string) {
        var parts = ns_string.split('.'),
        parent = MYAPP, i;
        // strip redundant leading global 
        if (parts[0] === "MYAPP") {
            parts = parts.slice(1); 
        }
        for (i = 0; i < parts.length; i += 1) {
            // create a property if it doesn't exist
            if (typeof parent[parts[i]] === "undefined") {
                parent[parts[i]] = {}; 
            }       
            parent = parent[parts[i]]; 
        }
        return parent; 
    };
```
        
### 声明依赖关系
好处：容易发现依赖关系，解析更快，性能好，易压缩
```javascript
    var myFunction = function () { 
        // dependencies
        var event = YAHOO.util.Event, 
            dom = YAHOO.util.Dom;
        // use event and dom variables
        // for the rest of the function... 
    };     
```


### 私有成员 公有方法
可以通过闭包实现私有成员
    
### 常量的实现方法
```javascript
    var constant = (function () { 
        var constants = {},
        ownProp = Object.prototype.hasOwnProperty, 
        allowed = {
                            string: 1, 
                            number: 1, 
                            boolean: 1
                        },
        prefix = (Math.random() + "_").slice(2); 
        return {
            set: function (name, value) { 
                if (this.isDefined(name)) {
                    return false; 
                }
                if (!ownProp.call(allowed, typeof value)) {
                    return false; 
                }
                constants[prefix + name] = value;
                return true; 
            },
            isDefined: function (name) {
                return ownProp.call(constants, prefix + name); 
            },
            get: function (name) {
                if (this.isDefined(name)) {
                    return constants[prefix + name]; 
                }
                return null; 
                }
        }; 
    }());
```

### 链模式
在方法里返回this
优点：简洁
缺点：不容易调试
    
### method() Method
实现：
```javascript
    if (typeof Function.prototype.method !== "function") { 
        Function.prototype.method = function (name, implementation) {
            this.prototype[name] = implementation;
            return this; 
        };
    }
```

调用：
```javascript
    var Person = function (name) {
        this.name = name; }.
    method('getName', function () {
        return this.name;
    }).
    method('setName', function (name) { 
        this.name = name;
        return this; 
    }); 
```

## 第六章 代码复用模式
---

### 类式继承模式#1 ---默认模式
实现方法：
```javascript
    function inherit(C,P){
        C.prototype = new P();
    }
```
缺点：
    同时继承了2个对象的属性，this属性和原型中的属性
    不支持将子构造函数的参数，传递到父函数中，如：
```javascript    
    function Parent(name) {
        this.name = name || 'Adam';
    }
    Parent.prototype.say = function () {
        return this.name;
    };
    function Child(name) {}
    inherit(Child, Parent);
    var s = new Child('Seth');
    s.say(); // "Adam"

    var kid = new Child();
    kid.name = "Patrick";
    kid.say(); // "Patrick"

```
### 类式继承模式#2 ---借用构造函数
```javascript
    function Child(a, c, b, d) {
        Parent.apply(this, arguments);
    }
```
但是这样无法继承父对象中的原型：
```javascript
    // the parent constructor
    function Parent(name) {
        this.name = name || 'Adam';
    }
    // adding functionality to the prototype
    Parent.prototype.say = function () {
        return this.name;
    };
    // child constructor
    function Child(name) {
        Parent.apply(this, arguments);
    }
    var kid = new Child("Patrick");
    kid.name; // "Patrick"
    typeof kid.say; // "undefined"
```
        
借用构造函数实现多重继承:
```javascript
    function Child(a, c, b, d) {
        Parent1.apply(this, arguments);
        Parent2.apply(this, arguments);
    }
```

借用构造函数的优缺点：
    缺点：无法从原型中继承任何东西
    优点：获得父对象的成员的属性副本，不会覆盖父对象
        
    
### 类式继承模式#3 ---借用构造函数 和 设置原型
```javascript
    function Child(a, c, b, d) {
        Parent.apply(this, arguments);
    }
    Child.prototype = new Parent();
```

### 类式继承模式#4 ---共享原型
```javascript
        function inherit(C, P) {
            C.prototype = P.prototype;
        } 
```
缺点： 任何一个子对象修改了原型将导致父对象原型的修改
    

### 类式继承模式#5 ---临时构造函数
```
    function inherit(C, P) {
        var F = function () {};
        F.prototype = P.prototype;
        C.prototype = new F();
        C.uber = P.prototype;
        C.prototype.constructor = C;
    }
```
或者：
```javascript
    var inherit = (function () {
        var F = function () {};
        return function (C, P) {
            F.prototype = P.prototype;
            C.prototype = new F();
            C.uber = P.prototype;
            C.prototype.constructor = C;
        }
    }());
```

### 通过复制继承
浅复制： 
```javascript
    var inherit = (function () {
        var F = function () {};
        return function (C, P) {
            F.prototype = P.prototype;
            C.prototype = new F();
            C.uber = P.prototype;
            C.prototype.constructor = C;
        }
    }());
```

深复制：
```javascript
    function extendDeep(parent, child) {
        var i,
            toStr = Object.prototype.toString,
            astr = "[object Array]";
        child = child || {};
        for (i in parent) {
            if (parent.hasOwnProperty(i)) {
                if (typeof parent[i] === "object") {
                    child[i] = (toStr.call(parent[i]) === astr) ? [] : {};
                    extendDeep(parent[i], child[i]);
                } else {
                    child[i] = parent[i];
                }
            }
        }
        return child;
    }
```


### 绑定上下文的实现
```javascript
    function bind(o, m) {
        return function () {
            return m.apply(o, [].slice.call(arguments));
        };
    }  
```
ECMAScript5 中`Function.prototype.bind()`:
```javascript
    if (typeof Function.prototype.bind === "undefined") {
        Function.prototype.bind = function (thisArg) {
            var fn = this,
                slice = Array.prototype.slice,
                args = slice.call(arguments, 1);
            return function () {
                return fn.apply(thisArg, args.concat(slice.call(arguments)));
            };
        };
    }
```

    
## 第七章 设计模式
---
### 单例模式
字面量就是单例的。
通过闭包和构造函数静态变量，自定义函数均可实现单例。自定义函数实现单例后，新添入原型的属性将访问不到。

### 工厂模式
实现：将各个产品构造函数当作工厂构造函数的属性。根据传参选择产品的构造函数，new 产品构造函数返回
    
内置对象工厂：
```javascript
    var o = new Object(), 
        n = new Object(1), 
        s = Object('1'),
        b = Object(true);
    // test
    o.constructor === Object; // true 
    n.constructor === Number; // true 
    s.constructor === String; // true 
    b.constructor === Boolean; // true    
```

### 迭代器模式
```javascript
    function ListStep(array,step){
        if(!(this instanceof ListStep)){
            return new ListStep(array,step);
        }
        this.index = 0;
        this.step = step,
        this.len = array.length,
        this.data = array;
    }
    ListStep.prototype.iterator = function(){
        var list = this;
        return {
            hasnext : function(){
                return list.index < list.len;
            },
            next : function(){
                var ele = null;
                if (this.hasnext()) {
                    ele = list.data[list.index];
                    list.index += list.step;
                };
                return ele;
            },
            rewind : function(){
                list.index = 0;
            },
            current : function(){
                return list.data[index];
            }
        }
    }
    var num = new ListStep([1,2,3,4,5,6,7,8],2);
    var iter = num.iterator();  
```
### 装饰者模式
可以通过继承和列表来实现，列表更简单
继承实现：
```javascript
    function Sale(price) {
        this.price = price || 100; 
    }
    Sale.prototype.getPrice = function () {
        return this.price; 
    };
    Sale.decorators.quebec = { 
        getPrice: function () {
            var price = this.uber.getPrice(); 
            price += price * 7.5 / 100; 
            return price;
        } 
    };
    Sale.decorators.money = {
        getPrice: function () {
            return "$" + this.uber.getPrice().toFixed(2); 
        }
    };
    Sale.decorators.cdn = { 
        getPrice: function () {
            return "CDN$ " + this.uber.getPrice().toFixed(2); 
        }
    };
    
    Sale.prototype.decorate = function (decorator) { 
        var F = function () {},
            overrides = this.constructor.decorators[decorator],
            i, newobj;
        F.prototype = this;
        newobj = new F(); 
        newobj.uber = F.prototype; 
        for (i in overrides) {
            if (overrides.hasOwnProperty(i)) {
                newobj[i] = overrides[i]; 
            }
        }
        return newobj; 
    };  
    
    var sale = new Sale(100);
    sale = sale.decorate('fedtax'); 
    sale = sale.decorate('cdn'); 
    sale.getPrice();
```

列表实现：
```javascript
    function Sale(price) {
        this.price = (price > 0) || 100;
        this.decorators_list = []; 
    }
    
    Sale.prototype.decorate = function (decorator) {
        this.decorators_list.push(decorator); 
    };
    Sale.prototype.getPrice = function () { 
        var price = this.price,
            i,
            max = this.decorators_list.length, 
            name;
        for (i = 0; i < max; i += 1) {
            name = this.decorators_list[i];
            price = Sale.decorators[name].getPrice(price); 
        }
        return price; 
    };
    Sale.decorators = {};
    Sale.decorators.fedtax = { 
        getPrice: function (price) {
            return price + price * 5 / 100; 
        }
    };
    Sale.decorators.quebec = { 
        getPrice: function (price) {
            return price + price * 7.5 / 100; 
        }
    };
    Sale.decorators.money = {
        getPrice: function (price) {
            return "$" + price.toFixed(2); 
        }
    };
```

### 策略模式
作用：动态选择算法，比如验证器，根据验证类型选择不同的验证算法。
    
### 外观模式
作用：提供更简单的访问接口，比如为浏览器兼容性封装接口。
    
### 代理模式
作用： 提供一个中间层，间接访问被代理对象。比如可以通过代理合并请求。通过代理懒加载,缓存代理等。
    
### 中介者模式
作用： 减少对象互相之间的通信，对象通过和中介者通信，中介者将通知发到该发送的对象。
    
### 观察者模式
    
## 第八章 DOM和浏览器模式
---
### DOM操作
    只更新一次活动文档，并只促发一次重绘。
```javascript
    var p, t, frag;
    frag = document.createDocumentFragment();
    p = document.createElement('p');
    t = document.createTextNode('first paragraph'); 
    p.appendChild(t);
    frag.appendChild(p);
    p = document.createElement('p');
    t = document.createTextNode('second paragraph'); 
    p.appendChild(t);
    frag.appendChild(p);
    document.body.appendChild(frag);
```
