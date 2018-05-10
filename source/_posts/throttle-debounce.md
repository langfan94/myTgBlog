---
title: throttle && debounce
date: 2018-05-10 11:00:50
tags:
---

# javaScript 函数节流和去抖

## OverView
***
### throttle
> throttle 降低触发回调的频率
### debounce
> debounce 对于一定时间段的连续的函数调用，只让其执行一次

***

## throttle

### throttle应用场景
* DOM 元素的拖拽功能实现（mousemove）
* 射击游戏的 mousedown/keydown 事件（单位时间只能发射一颗子弹）
* 计算鼠标移动的距离（mousemove）
* Canvas 模拟画板功能（mousemove）
* 搜索联想（keyup）
* 监听滚动事件判断是否到页面底部自动加载更多：给 scroll 加了 debounce 后，只有用户停止滚动后，才会判断是否到了页面底部；如果是 throttle 的话，只要页面滚动就会间隔一段时间判断一次

### throttle Function
```js
    function throttle(fn, interval = 300) {
        let canRun = true;
        return function () {
            if (!canRun) return;
            canRun = false;
            setTimeout(() => {
                fn.apply(this, arguments);
                canRun = true;
            }, interval);
        };
    }
```

## debounce
### debounce 应用场景
* 每次 resize/scroll 触发统计事件
* 文本输入的验证（连续输入文字后发送 AJAX 请求进行验证，验证一次就好）

###　debounce Function
```js 
    function debounce(fn, interval = 300) {
        let timeout = null;
        return function () {
            clearTimeout(timeout);
            timeout = setTimeout(() => {
                fn.apply(this, arguments);
            }, interval);
        };
    }
```