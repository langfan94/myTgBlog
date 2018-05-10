---
title: Object.defineProperty
date: 2018-05-10 10:57:23
tags:
---

# Object.defineProperty()
> 方法会直接在一个对象上定义一个新属性，或者修改一个已经存在的属性， 并返回这个对象。

## 参数
* obj 需要定义属性的对象
* prop 需要定义或修改的属性的名字
* descriptor 将被定义或修改的属性的描述符
* 返回值 返回传入函数的对象,即第一个参数obj
* configurable 当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，也能够被删除。默认为 false。
* enumerable 当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。默认为 false。
* value 该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。默认为 undefined。
* writable 当且仅当该属性的 writable 为 true 时，该属性才能被赋值运算符改变。默认为 false。
* get 一个给属性提供 getter 的方法，如果没有 getter 则为undefined。当我们读取某个属性的时候，其实是在对象内部调用了该方法，此方法必须要有return语句。该方法返回值被用作属性值。默认为 undefined。
* set 一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。默认为 undefined。也就是说，当我们设置某个属性的时候，实际上是在对象的内部调用了该方法。
## 实例
```js
var a = {}
Object.defineProperty(a, "b", {
    set: function(newValue) {
        console.log('新值: ' + newValue)
    },
    get: function() {
        console.log('需要返回的值')
        return 2
    }
})
a.b = 1 // 新值: 1
a.b  // 需要返回的值 2
```