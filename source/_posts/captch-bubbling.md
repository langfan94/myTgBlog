---
title: js 冒泡和捕获事件
date: 2018-06-06 11:32:22
tags:
---

# 冒泡和捕获事件

> 事件流描述的是从页面中接收事件的顺序。IE和Netscape开发团队提出了完全相反的事件流的概念。

> IE的事件流是事件冒泡流

> Netscape Communicator 的事件流是事件捕获流。

## 事件冒泡
> IE事件流叫做事件冒泡,即事件开始时由最具体的元素接收,然后逐级向上传播到较为不具体的节点.

```html
    <!Doctype html>
    <html>
    <head>
        <title>Event Bubbling Example</title>
    </head>
    <body>
        <div id="myDiv">Click Me</div>       
    </body>
    </html>
```

> 冒泡顺序是: Element div > Element body > Element html > Document

## 事件捕获
> Netscape Communicator 团队提出的另一种事件流叫做事件捕获。事件捕获的思想是不太具体的节点应该更早的接收到事件。

> 捕获的顺序是:  Document > Element html > Element body > Element div 


## DOM事件流
> 三个阶段: 事件捕获阶段、处于目标阶段 和 事件冒泡阶段


## DOM2级事件处理程序
> 用于处理指定和删除事件处理程序的操作: addEventListener() 和 removeEventListener().
> 所有DOM节点中都包含这两个方法,并且它们都接受三个参数: 要处理的事假名, 作为事件处理程序的函数 和 一个布尔值. 布尔值参数如果是 true ,表示在捕获阶段调用事件处理程序; 如果是false, 表示在冒泡阶段调用事件处理程序。

> 通过addEventListenter() 添加的事件处理程序只能使用removeEventListener() 来移除; 移除时传入的参数与添加处理程序时使用的参数相同。通过addEventListenter()添加的匿名函数将无法移除。

```js
    var btn = document.getElementById("myBtn");
    btn.addEventListener("click", function() {
        alert(this.id);
    }, false);
    btn.removeEventListener("click", function() {       // 没有用!
        alert(this.id);
    }, false)   
```

```js
    var btn = document.getElementById("myBtn");
    var handler = function() {
        alert(this.id);
    }
    btn.addEventListener("click", handler, false);
    btn.removeEventListener("click", handler, false);   // 有效 
```