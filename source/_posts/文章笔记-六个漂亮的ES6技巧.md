---
title: 文章笔记 - 六个漂亮的ES6技巧
date: 2018-08-25 17:35:15
tags:
  - javascript
  - javascript语法
categories:
  - 文章笔记
  - javascript
---

## 六个漂亮的 ES6 技巧

### 引用

> [英文原文：Six nifty ES6 tricks](http://2ality.com/2016/05/six-nifty-es6-tricks.html)  
> [中文译文：六个漂亮的 ES6 技巧](https://www.h5jun.com/post/six-nifty-es6-tricks.html)

### 概要

1.  通过参数默认值强制要求传参

    ```js
    function paramRequired() {
    	throw new Error('Missing parameter')
    }

    function foo(arg = paramRequired()) {
    	return arg
    }
    ```

    这里可以发现，默认参数是运行时，参数缺失时，才会被求值的

    查看 babel 的转码也可以看出：

    ```js
    // babel

    function mandatory() {
    	throw new Error('Missing parameter')
    }

    function foo() {
    	var mustBeProvided =
    		arguments.length > 0 && arguments[0] !== undefined
    			? arguments[0]
    			: mandatory()
    	return mustBeProvided
    }
    ```

2.  通过 for-of 循环来遍历数组元素和索引

    可以通过 Array.prototype.entries 来弥补 for-of 无法获得数组索引的缺点

    ```js
    for (let [index, elem] of arr.entries()) {
    	console.log(`index = ${index}, elem = ${elem}`)
    }
    ```

3.  遍历 Unicode 表示的字符串 🚀 占两个字符的长度

    ```js
    const str = 'x🚀y' //x\uD83D\uDE80y
    // for-of遍历每个字符，不用担心长度不一

    for (ch of str) {
    	console.log(ch.length)
    }

    // Output:
    // 1
    // 2
    // 1
    // spread也可以很好将各个字符展开，容易算出实际的字数

    ;[...'x\uD83D\uDE80y'].length // 3
    ```

4.  通过变量解构交换两个变量的值

    ```js
    ;[a, b] = [b, a]
    ```

    可以想象，JavaScript 引擎在未来将会针对这个模式进行特别优化，去掉构造数组的开销

5.  通过模板字面量（template literals）进行简单的模板解析

6.  通过子类工厂实现简单的合成器

    javascript 通过继承来达到 mixin 的作用，类似于 php 中的 trait

    ```js
    const Storage = Sup => class extends Sup {
        save(database) { ··· }
    };

    const Validation = Sup => class extends Sup {
        validate(schema) { ··· }
    };

    class Person { ··· }

    class Employee extends Storage(Validation(Person)) { ··· }
    ```
