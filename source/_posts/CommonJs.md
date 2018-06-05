---
title: CommonJs规范
date: 2018-06-05 11:43:06
tags:
---

# CommonJs规范

## 概述
> Node 应用由模块组成，采用 CommonJS 模块规范。

> 每个文件就是一个模块，有自己的作用域。在一个文件里面定义的变量、函数、类，都是私有的，对其他文件不可见。

## CommonJS模块的特点如下:
* 所有代码都运行在模块作用域，不会污染全局作用域。
* 模块可以多次加载，但是只会在第一次加载时运行一次，然后运行结果就被缓存了，以后再加载，就直接读取缓存结果。要想让模块再次运行，必须清除缓存。
* 模块加载的顺序，按照其在代码中出现的顺序。

## module对象
> Node内部提供一个Module构建函数。所有模块都是Module的实例。

### function
```js
    function Module(id, parent) {
        this.id = id;
        this.exports = {};
        this.parent = parent;
  // ...
```

> 每个模块内部，都有一个module对象，代表当前模块。它有以下属性。

* module.id 模块的识别符，通常是带有绝对路径的模块文件名。
* module.filename 模块的文件名，带有绝对路径。
* module.loaded 返回一个布尔值，表示模块是否已经完成加载。
* module.parent 返回一个对象，表示调用该模块的模块。
* module.children 返回一个数组，表示该模块要用到的其他模块。
* module.exports 表示模块对外输出的值。

## module.exports属性
> module.exports属性表示当前模块对外输出的接口，其他文件加载该模块，实际上就是读取module.exports变量。

### demo 
```js
    var EventEmitter = require('events').EventEmitter;
    module.exports = new EventEmitter();

    setTimeout(function() {
    module.exports.emit('ready');
    }, 1000);
```

> 上面模块会在加载后1秒后，发出ready事件。其他文件监听该事件，可以写成下面这样。

```js
    var a = require('./a');
    a.on('ready', function() {
    console.log('module a is ready');
    });
```

## exports变量
> 为了方便，Node为每个模块提供一个exports变量，指向module.exports。这等同在每个模块头部，有一行这样的命令。

## AMD规范与CommonJS规范的兼容性

> CommonJS规范加载模块是同步的，也就是说，只有加载完成，才能执行后面的操作。AMD规范则是非同步加载模块，允许指定回调函数。由于Node.js主要用于服务器编程，模块文件一般都已经存在于本地硬盘，所以加载起来比较快，不用考虑非同步加载的方式，所以CommonJS规范比较适用。但是，如果是浏览器环境，要从服务器端加载模块，这时就必须采用非同步模式，因此浏览器端一般采用AMD规范。

```js
    define(['package/lib'], function(lib){
    function foo(){
        lib.log('hello world!');
    }

    return {
        foo: foo
    };
    });
```

> AMD规范允许输出的模块兼容CommonJS规范，这时define方法需要写成下面这样：

```js 
    define(function (require, exports, module){
    var someModule = require("someModule");
    var anotherModule = require("anotherModule");

    someModule.doTehAwesome();
    anotherModule.doMoarAwesome();

    exports.asplode = function (){
        someModule.doTehAwesome();
        anotherModule.doMoarAwesome();
    };
    });
```

## require命令

### 基本用法
> Node使用CommonJS模块规范，内置的require命令用于加载模块文件。

> require命令的基本功能是，读入并执行一个JavaScript文件，然后返回该模块的exports对象。如果没有发现指定模块，会报错。

## 模块的缓存
> 第一次加载某个模块时，Node会缓存该模块。以后再加载该模块，就直接从缓存取出该模块的module.exports属性。

```js 
    require('./example.js');
    require('./example.js').message = "hello";
    require('./example.js').message
// "hello"
```

> 所有缓存的模块保存在require.cache之中，如果想删除模块的缓存，可以像下面这样写。

```js
    // 删除指定模块的缓存
    delete require.cache[moduleName];

    // 删除所有模块的缓存
    Object.keys(require.cache).forEach(function(key) {
    delete require.cache[key];
    })
```
## require.main

> require方法有一个main属性，可以用来判断模块是直接执行，还是被调用执行。

> 直接执行的时候（node module.js），require.main属性指向模块本身。

```js 
    require.main === module
    // true
```

## 模块的加载机制

> CommonJS模块的加载机制是，输入的是被输出的值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。请看下面这个例子。

> 下面是一个模块文件lib.js。

```js 
    // lib.js
    var counter = 3;
    function incCounter() {
    counter++;
    }
    module.exports = {
    counter: counter,
    incCounter: incCounter,
    };
```

> 然后，加载上面的模块。

```js
    // main.js
    var counter = require('./lib').counter;
    var incCounter = require('./lib').incCounter;

    console.log(counter);  // 3
    incCounter();
    console.log(counter); // 3
```