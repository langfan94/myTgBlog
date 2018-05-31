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

## demo2
```
function Publish() {     // 主题对象
    this.subscribers = [];
}

// 定义订阅的函数
Publish.prototype = {
    theme: 'default',       // 默认主题
    sub: function(subPerson) {  // 订阅的函数
        let btn = true
        this.subscribers.forEach(item => {      // 不可重复订阅
            if(subPerson.name === item.name) {
                return btn = false
            }
        })
        if(btn) {
            this.subscribers.push(subPerson)
        }
    },
    deliver: function() {   // 分发消息函数
        if(this.subscribers.length > 0) {
            this.subscribers.forEach(item => {  // 触发所有订阅者的更新
                item.update(this.theme)
            });
        }else {
            console.log(this.theme + 'is no subscribers');
        }
    }
}

function Sub(name) {        // 订阅者名字
    this.name = name;
}

Sub.prototype = {
    subFn: function(pub) {         // 订阅者订阅方法
        if(pub && pub.sub && typeof pub.sub === 'function') {
            pub.sub(this);
        }
    },
    update: function(theme) {   // 订阅者更新方法
        console.log(this.name + '收到了'　+ theme);
    }
}


// 创建了三个主题
let publish1 = new Publish()
publish1.theme = '美剧'

let publish2 = new Publish()
publish2.theme = '日剧'

let publish3 = new Publish()
publish3.theme = '韩剧'

// 创建8个订阅者
let sub1 = new Sub('Joan')
let sub2 = new Sub('Joan1')
let sub3 = new Sub('Joan2')
let sub4 = new Sub('Joan3')
let sub5 = new Sub('Joan4')
let sub6 = new Sub('Joan5')
let sub7 = new Sub('Joan6')
let sub8 = new Sub('Joan7')

sub1.subFn(publish1);
sub1.subFn(publish2);
sub2.subFn(publish2);
sub3.subFn(publish2);

publish1.deliver();
publish2.deliver();
publish3.deliver();

```

> 运行结果 
> 
> Joan收到了美剧
> 
> Joan收到了日剧
> 
> Joan1收到了日剧
> 
> Joan2收到了日剧
> 
> 韩剧is no subscribers