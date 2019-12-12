---
title: Javascript-decorators
date: 2019-12-12 10:42:04
tags: decorators
---

# Summary

> Decorators make it possible to annotate and modify classes and properties at design time.


A decorator is:

*   an expression
*   that evaluates to a function
*   that takes the target, name, and decorator descriptor as arguments
*   and optionally returns a decorator descriptor to install on the target object

A decorator precedes the syntax that defines a property:
```javascript
class Person {
  @readonly
  name() { return `${this.first} ${this.last}` }
}
```

Now, before installing the descriptor onto Person.prototype, the engine first invokes the decorator:

```javascript
let description = {
  type: 'method',
  initializer: () => specifiedFunction,
  enumerable: false,
  configurable: true,
  writable: true
};

description = readonly(Person.prototype, 'name', description) || description;
defineDecoratedProperty(Person.prototype, 'name', description);

function defineDecoratedProperty(target, { initializer, enumerable, configurable, writable }) {
  Object.defineProperty(target, { value: initializer(), enumerable, configurable, writable });
}
```

The has an opportunity to intercede before the relevant defineProperty actually occurs.