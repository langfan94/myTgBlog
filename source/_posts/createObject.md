---
title: javascript 创建对象
date: 2018-05-11 14:31:59
tags:
---

# 创建对象
## 工厂模式
> 用函数封装以特定接口创建对象

```js
    function createPerson(name, age, job) {
        let o = new Object();
        o.name = name;
        o.age = age;
        o.job = job;
        o.sayName = function() {
            console.log(this.name)
        }
        return o;
    }
    let person1 = createPerson('nick', 29, 'Software Engineer');
    let person2 = createPerson('Greg', 27, 'Doctor');
```

## 构造函数模式

```js
    function Person(name, age, job) {
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = sayName;
    // this.sayName = function() {
    //     console.log('name is:', this.name);
    // }
    }

    function sayName() {
        return this.name;
    }

    let person1 = new Person('Nicholas', 29, 'Software Engineer');
    let person2 = new Person('Greg', 27, 'Doctor');

    function readPerson(oPerson) {
        for(let i in oPerson) {
            if(typeof oPerson[i] === 'function') {
                console.log(i, oPerson[i]())
            }else {
                console.log(i, oPerson[i])
            }
        }
    }

    readPerson(person1);

    readPerson(person2);
```

## 原型模式
> 每个函数都有一个prototype属性,这个属性是一个指针，指向一个对象,而这个对象的用途是包含可以由特定类型的所有实例共享的属性和方法。
```js 
    function Person() {
        Person.prototype.name = 'Nicholas';
        Person.prototype.age = 29;
        Person.prototype.job = 'Software Engineer';
        Person.prototype.sayName = function() {
            console.log(this.name);
        }
    }
    let person1 = new Person();
    person1.sayName();      // 'Nicholas'
    let person2 = new Person();
    person2.sayName();      // 'Nicholas'
    console.log(person1.sayName == person2.sayName);    // true
```
> person1和person2 的内部属性_proto_(read virtual) 指向Person prototype; Person prototype.constructor 指向Person
> constructor 属性也是共享的,也可以通过实例共享。

```js
    person1.name = 'KKK';
    console.log(person1.name); // "KKK"  -----来自实例
    delete person1.name;
    console.log(person1.name); // "Nicholas"  -----来自原型
    console.log(person2.name); // "Nicholas"  -----来自原型
    console('name' in person1);// true
```

### 原型与in操作符　

> 有两种方式使用in操作符: 单独使用和在for-in循环中使用。在单独使用时,in操作符会在通过对象能够访问给定属性时返回true, 无论该属性存在于实例中还是原型中 

### 原型的动态性
```js
    let friend = new Person();
    Person.prototype.sayHi = function() {
        console.log('hi');
    }
    friend.sayHi();  // "hi"
```

> 在原型中查找值的过程是一次搜索

### 原生对象的原型

> 所有原生的引用类型(Object、Array、String,　等等) 都在其构造函数的原型上定义了方法

```js
    console.log(typeof Array.prototype.sort); // "function"
```


### 组合使用构造函数模式和原型模式
```js
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        this.friends = ["Shelby", "Count"];
    }
    Person.prototype = {
        constructor: Person,
        sayName: function() {
            console.log(this.name;)
        }
    }
    let person1 = new Person('Nicholas', 27, "Software Engineer");
    let person2 = new Person('Greg', 22, "Doctor");
    person1.friends.push("Van");
    console.log(person1.friends);  // "Shelby, Count, Van"
    console.log(person1.friends);  // "Shelby, Count"
    console.log(person1.friends === person2.friends);  // false;
    console.log(person1.sayName === person2.sayName);  // true;
```
