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


### 动态原型模式

```js
    function Person(name, age, job) {
        this.name = name;
        this.age = age;
        this.job = job;
        // 方法
        if(typeof this.sayName != 'function') {
            Person.prototype.sayName = function() {
                console.log('this.name');
            }
        }
    }
    let friend = new Person('Nicholas', 29, 'software Engineer');
    friend.sayName();
```

> 只有在sayName()方法不存在的情况下,才能将它添加到原型当中去.

### 寄生构造函数模式
```js
    function SpecialArray() {
        let values = new Array();
        values.push.apply(values, arguments);
        values.toPipedString = function() {
            return this.join('|');
        }
        return values;
    }
    let colors = new SpecialArray("red", "blue", "green");
    console.log(colors.toPipedString());  // "red|blue|green"
```

1. 返回的对象和构造函数或者与构造函数的原型属性之间没有关系。
2. 构造函数返回的对象与在构造函数外部创建的对象没有什么不同。
3. 不能依赖instanceof操作符确定对象类型。
4. 末尾的return 语句，可以重写调用构造函数时返回的值。

### 稳妥构造函数模式

```js 
    function Person(name, age, job) {
        let o = new Object();
        o.sayName = function() {
            console.log(name);
        }
        return o;
    }
    let friend = Person('Nicholas', 29, 'Software Engineer');
    friend.sayName();  // 'Nicholas'
```

> 所谓的稳妥对象，指的是没有公共属性，而且其方法也不引用this的对象，不使用new操作符号调用构造函数。在这种模式创建的对象中，除了使用sayName()方法之外，没有其他办法访问name的值。

### 使用原型链实现继承出现的问题
```js 
    function SuperType() {
        this.colors = ['red', 'blue', 'green'];
    }
    function SubType() {}
    subType.prototype = new SuperType();
    let instance1 = new SubType();
    instance1.colors.push('black');
    console.log(instance1.colors);  // ['red','blue','green','black']
    let instance2 = new SubType();
    console.log(instance2.colors);  // ['red','blue','green','black']
```

> 没有办法在不影响所有对象实例的情况下，给超类型的构造函数传递参数。

### 借用构造函数
> 通过使用apply()和call()方法在新创建的对象上执行构造函数

```js
    function SuperType() {
        this.colors = ['red', 'blue', 'green']; 
    }
    function SubType() {
        // 继承了SuperType
        SuperType.call(this);
    }
    let instance1 = new SubType();
    instance1.colors.push('black');
    console.log(instance1.colors); // ['red','blue','green','black']
    let instance2 = new SubType();
    console.log(instance2.colors); // ['red','blue','green']
```
> 借用构造函数的缺点　方法都在构造函数中定义,因此函数复用就无从谈起。

### 组合继承函数
```js
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green'];
    }
    SuperType.prototype.sayName = function() {
        console.log(this.name);
    };
    function SubType(name, age) {
        SuperType.call(this, name);
        this.age = age;
    }
    SubType.prototype = new SuperType();
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function() {
        console.log(this.age);
    }
    let instance1 = new SubType('Nicholas', 29);
    instance1.colors.push('black');
    console.log(instance1.colors); // ['red', 'blue', 'green', 'black']
    instance1.sayName();   // 'Nicholas';
    instance1.sayAge();  // 29
    let instance2 = new SubType('Greg', 27);
    console.log(instance2.colors); // ['red', 'blue', 'green']
    instance2.sayName();   // 'Greg';
    instance2.sayAge();  // 27
```

> instanceof 和　isPrototypeOf() 也能够识别基于组合继承创建的对象

### 原型式继承
```js
    let person = {
        name: 'Nicholas',
        friends: ['Shelby', 'Court', 'Van']
    }
    let anotherPerson = Object.create(person);
    anotherPerson.name = 'Greg';
    anotherPerson.friends.push('Rob');
    let yetAnotherPerson = Object.create(person);
    yetAnotherPerson.name = 'Linda';
    yetAnotherPerson.friends.push('Barbie');
    console.log(person.friends); // ['Shelby', 'Court', 'Van', 'Rob', 'Barbie']
```

> Object.create() 方法第二个参数与Object.defineProperties() 方法第二个参数格式相同

```js
    let person = {
        name: 'Nicholas',
        friends: ['Shelby', 'Court', 'Van']
    }
    let anotherPerson = Object.create(person, {
        name: {
            value: "Greg"
        }
    });
    console.log(anotherPerson.name); // 'Greg'
```

### 寄生式继承
```js
    function createAnother(original) {
        let clone = object(original);
        clone.sayHi = function() {
            console.log("hi");
        };
        return clone; 
    }
    let person = {
        name: 'Nicholas',
        friends: ['Shelby', 'Court', 'Van']
    };
    let anotherPerson = createAnother(person);
    anotherPerson.sayHi(); // 'hi'
```

### 寄生组合式继承
```js
    function SuperType(name) {
        this.name = name;
        this.colors = ['red', 'blue', 'green']; 
    }
    SuperType.prototype.sayName = function() {
        console.log(this.name);
    }
    function SubType(name, age) {
        SuperType.call(this, name);        // 第二次调用SuperType()
        this.age = age;
    }
    SubType.prototype = new SuperType();    // 第一次调用SuperType()　
    SubType.prototype.constructor = SubType;
    SubType.prototype.sayAge = function() {
        console.log(this.age);
    }
```

#### 寄生组合式继承基本模式如下：
```js
    function inheritPrototype(subType, superType) {
        let prototype = object(superType.prototype); // 创建对象
        prototype.constructor = subType;             // 增强对象
        subType.prototype = prototype;               // 指定对象
    }
```