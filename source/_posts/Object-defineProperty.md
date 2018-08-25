---
title: Object.defineProperty
date: 2018-08-25 17:52:44
tags:
  - javascript
  - javascript api
categories:
  - javascript
---

## Object.defineProperty

> [MDN Object.defineProperty()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/defineProperty)

### 语法

```
/**

obj: 要在其上定义属性的对象

prop: 要定义或修改的属性的名称

descriptor: 将被定义或修改的属性描述符

*/

Object.defineProperty(obj, prop, descriptor)
```

### 属性描述符

- 数据描述符和存取描述符。
- 数据描述符是一个具有值的属性，该值可能是可写的，也可能不是可写的。(对应`value`和`writable`)
- 存取描述符是由 getter-setter 函数对描述的属性。（对应`get`和`set`）
- 描述符必须是这两种形式之一；不能同时是两者，否则会异常。
- configurable 与 enumerable 则无限制。
- 有可能会改变继承来的属性，使用 Object.freeze(Object.prototype)或将**proto**指向 null，可防止。

1.  `configurable`(= false)

    当且仅当该属性的 configurable 为 true 时，该属性描述符才能够被改变，同时该属性也能从对应的对象上被删除。

    ```js
    var o = {}
    Object.defineProperty(o, 'a', {
    	get: function() {
    		return 1
    	},
    	configurable: false,
    })

    // 一旦设置了configurable: false，就无法改变描述符，无法改值，也无法delete这个属性了

    // throws a TypeError
    Object.defineProperty(o, 'a', { configurable: true })

    // throws a TypeError (set was undefined previously)
    Object.defineProperty(o, 'a', { set: function() {} })

    // throws a TypeError (even though the new get does exactly the same thing)
    Object.defineProperty(o, 'a', {
    	get: function() {
    		return 1
    	},
    })

    // throws a TypeError
    Object.defineProperty(o, 'a', { value: 12 })

    console.log(o.a) // logs 1
    delete o.a // Nothing happens
    console.log(o.a) // logs 1
    ```

1.  `enumerable`(= false)

    当且仅当该属性的 enumerable 为 true 时，该属性才能够出现在对象的枚举属性中。

1.  `value`(= undefined)

    该属性对应的值。可以是任何有效的 JavaScript 值（数值，对象，函数等）。

1.  `writable`(= false)

    当且仅当该属性的 writable 为 true 时，value 才能被赋值运算符改变

1.  `get`(= undefined)

    一个给属性提供 getter 的方法，如果没有 getter 则为 undefined。该方法返回值被用作属性值。

1.  `set`(= undefined)

    一个给属性提供 setter 的方法，如果没有 setter 则为 undefined。该方法将接受唯一参数，并将该参数的新值分配给该属性。
