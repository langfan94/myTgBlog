---
title: How to write your own Virtual DOM
date: 2018-06-13 14:26:50
tags:
---

# How to write your own Virtual DOM

[引用文章链接](https://medium.com/@deathmood/how-to-write-your-own-virtual-dom-ee74acc13060)

* Virtual DOM is any kind of representation of a real DOM
* When we change something in our Virtual DOM Tree, we get a new Virtual Tree. Algorithm compares these two trees (old and new), finds differences and makes only necessary small changes to real DOM so it reflects virtual

## Representing our DOM Tree
```html
    <ul class=”list”>
        <li>item 1</li>
        <li>item 2</li>
    </ul>
```

turn to Js Object

```js
    { type: ‘ul’, props: { ‘class’: ‘list’ }, children: [
        { type: ‘li’, props: {}, children: [‘item 1’] },
        { type: ‘li’, props: {}, children: [‘item 2’] }
    ] }
```

* We represent DOM elements with objects like

```js
    { type: ‘…’, props: { … }, children: [ … ] }
```

