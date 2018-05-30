---
title: 订阅者模式
date: 2018-05-30 17:49:58
tags:
---

# 订阅者模式

> 订阅者模式涉及三个对象：发布者、主题对象、订阅者，三个对象间的是一对多的关系，每当主题对象状态发生改变时，其相关依赖对象都会得到通知，并被自动更新

## demo
```js 
    function Dep() {//主题对象
    this.subs = []; //订阅者列表
    }
    Dep.prototype.notify = function() { //主题对象通知订阅者
    this.subs.forEach(function(sub){ //遍历所有的订阅者，执行订阅者提供的更新方法
        sub.update();
    });
    }
    function Sub(x) { //订阅者
    this.x = x;
    }
    Sub.prototype.update = function() { //订阅者更新
    this.x = this.x + 1;
    console.log(this.x);
    }
    var pub = { //发布者
    publish: function() {
        dep.notify();
    }
    };
    var dep = new Dep(); //主题对象实例
    Array.prototype.push.call(dep.subs, new Sub(1), new Sub(2), new Sub(4)); //新增 3 个订阅者
    pub.publish(); //发布者发布更新
// 2
// 3
// 5
```
