---
title: 闭包
date: 2018-05-21 11:50:48
tags:
---

# 闭包
>　闭包：当某个函数被调用时,会创建一个执行环境及相应的作用域链。然后,使用arguments和其他命名参数的值来初始化函数的活动对象。但是在作用域链中,外部函数的活动对象始终处于第二位,外部函数的活动对象处于第三位，...直至作为作用域链终点的全局执行环境。

## 闭包与变量
> 作用域链的这种配置机制引出了一个副作用,即闭包只能去地包含函数中任何变量的最后一个值。

### demo
```js
    function createFunctions() {
        var result = new Array();
        for(var i=0; i<10; i++) {
            result[i] = function () {
                return i;
            }
        }
        return result;
    }
    var re = createFunctions();
    re[0]();   // 10 
```

### demo2
```js 

    function createFunctions() {
        var result = new Array();
        for(var i=0; i<10; i++) {
            result[i] = function(num) {
                return function() {
                    return num;
                }
            }(i)
        }
    }
```

>　在调用每个匿名函数时, 我们传入了变量i。由于函数参数是按值传递的,所以会将变量i的当前值复制给参数num.而在这个匿名函数内部,又创建返回了一个访问num的闭包.这样一来,result 数组中的每个函数都有自己num变量的一个副本,因此就可以返回各自不同的数值了.