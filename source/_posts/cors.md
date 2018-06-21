---
title: 跨域技术
date: 2018-06-21 14:45:29
tags:
---

# 其他跨域技术

## 图ping
> 使用\<img\>标签实现跨域，使用它们的onload和onerror事件处理程序来确定是否收到了相应，通过图像Ping,浏览器得不到任何具体的数据,但通过监听load和error事件,它能知道响应是什么时候接收道的。

### demo
```js
    var img = new Image();
    img.onload = img.onerror = function() {
        alert('Done!');
    }
    img.src = "http://www.example.com/test?name=Nicholas";
```

> 这里创建了一个Image的实例,然后将onload 和　onerror事件处理程序指定为同一个函数。这样无论是什么响应,只要请求完成,就能得到通知.请求从设置src属性的那一刻开始,而这个例子在请求中发送了一个name参数。
> 图像ping最常用于跟踪用户点击页面或动态广告曝光次数。图像Ping有两个主要的缺点,一是只能发送Get请求,二是无法访问服务器的响应文本。

## JSONP
> JSONP 是JSON with padding(填充式JSON或参数式JSON)的简写，是应用JSON的一种新方法

### demo
```js 
    function handleResponse(response) {
        alert('You` are at IP' + response.ip + ", which is in" + response.city + ", " + response.response.regin_name);        
    }
    var script = document.createElement("script");
    script.src = "http://freegeoip.net/json/?callback=handleResponse"
    document.body.insertBefore(script, document.body.firstChild);
```
> 优点是能够直接访问响应文本,支持在浏览器与服务器之间双向通信。但是缺点有可能在响应中夹带一些恶意代码。H5给\<script\>元素新增了一个onerror事件处理程序，但是目前还没有得到任何浏览器的支持。

# Comet
> 指的是一种更高级的Ajax技术,Ajax是一种从页面向服务器请求数据的技术,而Comet则是一种服务器向页面推送数据的技术。Comet能够让信息近乎实时的被推送到页面上,非常适合处理体育比赛的分数和股票的报价。

>短轮询: 浏览器定时向服务器发送请求
>长轮询: 页面发起一个请求,然后服务器一直保持连接打开,直到有数据可发送。发送完数据之后,浏览器关闭连接,随即又发起一个到服务器的新请求。
>Comet 实现的是HTTP流。流不同于上述两种轮询,因为它在页面的整个生命周期内只使用一个HTTP连接.具体来说就是浏览器向服务器发送一个请求,而服务器保持连接打开,然后周期性的向浏览器发送数据。