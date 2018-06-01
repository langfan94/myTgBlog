---
title: Symbol.iterator
date: 2018-06-01 16:15:16
tags:
---

# Symbol.iterator

> Symbol.iterator 为每一个对象定义了默认的迭代器。该迭代器可以被 for...of 循环使用。

## 描述
> 当需要对一个对象进行迭代时（比如开始用于一个for..of循环中），它的@@iterator方法都会在不传参情况下被调用，返回的迭代器用于获取要迭代的值。

> 一些内置类型拥有默认的迭代器行为，其他类型（如 Object）则没有。下表中的内置类型拥有默认的@@iterator方法：

### demo

```js
    const iterable1 = new Object();

    iterable1[Symbol.iterator] = function* () {
    yield 1;
    yield 2;
    yield 3;
    };

    console.log([...iterable1]);
    // expected output: Array [1, 2, 3]
```