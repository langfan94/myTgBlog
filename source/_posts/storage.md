---
title: 数据存储
date: 2018-06-27 10:44:09
tags:
---

# 数据存储

## Cookie

> HTTP Cookie（也叫Web Cookie或浏览器Cookie）是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。Cookie使基于无状态的HTTP协议记录稳定的状态信息成为了可能。

### Cookie主要用于以下三个方面：
* 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
* 个性化设置（如用户自定义设置、主题等）
* 浏览器行为跟踪（如跟踪分析用户行为等）

> JavaScript通过Document.cookies访问Cookie

```js 
    console.log(document.cookie); 
```

## sessionStorage 对象
> sessionStorage 属性允许你访问一个 session Storage 对象。它与 localStorage 相似，不同之处在于 localStorage 里面存储的数据没有过期时间设置，而存储在 sessionStorage 里面的数据在页面会话结束时会被清除。页面会话在浏览器打开期间一直保持，并且重新加载或恢复页面仍会保持原来的页面会话。在新标签或窗口打开一个页面会初始化一个新的会话，这点和 session cookies 的运行方式不同。

### 语法

```js
    // 保存数据到sessionStorage
    sessionStorage.setItem('key', 'value');

    // 从sessionStorage获取数据
    var data = sessionStorage.getItem('key');

    // 从sessionStorage删除保存的数据
    sessionStorage.removeItem('key');

    // 从sessionStorage删除所有保存的数据
    sessionStorage.clear();
```

## localStorage 对象
> localStorage 与 sessionStorage 一样，都遵循同源策略，但是它是持续存在的。localStorage 首次出现于 Firefox 3.5。

> localStorage 和 globalStorage[location.hostname] 是一样的, 除了作用域是在 HTML5 origin (结构 + 主机名 + 非标准的端口)， 并且 localStorage 是一个 Storage 实例 ，而globalStorage[location.hostname] 是一个 StorageObsolete 实例


## IndexedDB

> IndexedDB 是一个用于在浏览器中储存较大数据结构的 Web API, 并提供索引功能以实现高性能查找. 像其他基于 SQL 的 关系型数据库管理系统 (RDBMS) 一样, IndexedDB 是一个事务型的数据库系统. 然而, 它是使用 JavaScript 对象而非列数固定的表格来储存数据的.