---
title: isNaN的诡异行为
date: 2018-08-25 17:57:38
tags:
  - javascript
  - javascript api
categories:
  - javascript
---

## isNaN 的诡异行为

### 什么是 NaN

在 javascript 中有一个神奇的变量叫做 NaN，意思是 Not a number。

什么时候会遇到这个变量呢，例如我们想把一个字符串`'10'`转换成数字`10`，会使用`Number('10')` 或者 `parseInt('10')`，那如果这个字符串并不是一个有效的数字呢？例如我们进行 parseInt(‘abc’)就会得到 NaN。

当我们想知道转换出来的东西是不是一个 NaN，就会涉及到如何去判断 NaN 这个问题，但是他的行为又非常的诡异，因为：

```js
NaN == NaN // false

NaN === NaN // false
```

所以我们没有办法通过双等和三等来判断出 NaN

当然反过来想，我们也可以利用他的这个特性 `a !== a` 来判断是不是 NaN，因为目前为止只有 NaN 会有这样的行为。

但是这样的代码非常的不直观，甚至于说不熟悉 js 或者 NaN 的人都会怀疑这个代码是不是写错了，哪有变量不等于自己呢？

### isNaN 应运而生

isNaN 就是为了解决这个问题而诞生的，我们希望有一个函数能帮我们判断一个变量是不是 NaN

```js
isNaN(NaN) // true
```

看似这个 api 非常完美，但是似乎事情并没有那么简单

```js
isNaN('abc') // true
```

曾经在这个上面踩过坑，为什么为了判断 NaN 的 api 会将字符串’abc’判断成 NaN 呢

这是因为 isNaN 在判断的时候会做一次隐式的转换，等同于说先对参数进行一次 Number()的包装，再用返回值作为判断是否为 NaN 的标准

```js
Number('abc') // NaN

isNaN('abc') // true

Number(true) // 1

isNaN(true) // false

Number('10') // 10

isNaN('10') // false
```

所以 isNaN 的行为是非常诡异的，甚至和他的语义并不相等价，所以 es6 增加了新的 API`Number.isNaN`

### Number.isNaN

Number.isNaN 不会对参数进行隐式转换，而是符合 api 语义地只判断参数是否是 NaN

```js
Number.isNaN(NaN) // true

Number.isNaN('abc') // false

Number.isNaN(true) // false

Number.isNaN('10') // false
```

### 结论

isNaN 有坑，能用就用 Number.isNaN，不能用最好自己 polyfill 或包装一个函数，如果判断 NaN 的地方都利用 `a !== a`，代码可读和理解上会有损失

```js
Number.isNaN =
	Number.isNaN ||
	function(value) {
		return value !== value
	}
```
