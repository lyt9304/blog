---
title: Node.js的调试
date: 2018-08-25 17:34:10
tags:
  - node
  - debug
categories:
  - node
---

## Node.js 的调试

### 如何启动

> ref: [https://nodejs.org/en/docs/guides/debugging-getting-started/](https://nodejs.org/en/docs/guides/debugging-getting-started/)
>
> ref: [https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27](https://medium.com/@paul_irish/debugging-node-js-nightlies-with-chrome-devtools-7c4a1b95ae27)

```
node --inspect index.js

node --inspect-brk index.js # node v7+ 会在第一行直接断点
```

### 调试的载体

#### 1. chrome-dev-tool

- `about:inspect`（等价于`chrome://inspect`）
- 选择`Remote Target` -> `inspect`
- 弹出一个 dev tool 窗口，就可以调试了

#### 2. vscode

> ref: [https://code.visualstudio.com/docs/editor/debugging#\_variable-substitution](https://code.visualstudio.com/docs/editor/debugging#_variable-substitution)

- 菜单`调试` -> `启动调试`，或者 F5
- 切换到 debug 界面即可开始调试

Tips:

- debug 模式下 node 默认路径是`/usr/local/bin/node`。如果需要自定义的话，可以添加配置，vscode 会生成一个`lauch.json`文件，然后添加`runtimeExecutable`选项到自定义的 node 可执行文件即可。另外，必须要在项目文件夹下才能够创建`lauch.json`，所以我随便建的一个 test.js 打开，就无法创建。
- `lauch.json`还能配置很多选项，可以参考 ref 链接查看详细的文档

### 关于调试的基本概念

无论是在 chrome 也好，vscode 也好，在打完断点之后，主要会通过几个按钮来操作语句的进行状态

- 单步跳过(step over) 跳过该行语句，进入下一行进行调试
- 单步调试(step into) 进入语句中的子函数进行步进调试
- 单步跳出(step out) 跳出当前函数，进入调用源进行调试
- 继续（step）继续执行，直到下一个断点暂停

对于 `step over` 和 `step into` 一直有些搞不清楚，举个例子：

```
const fn = () => {

	const random = Math.random() // fn:1

	return random

}

const random = fn() // main:1 => breakpoint

if (random > 0.5) {

	// main:2

	console.log('You win')

} else {

	console.log('Maybe next time')

}
```

当我们在`main:1`的地方打上断点后：

- 如果采用`step over`，那么将会直接跳到`main:2`
- 如果采用`step into`则会进入到 fn 内部的`fn:1`；如果在此时使用`step out`则会跳出 fn 函数，回到原来调用源的下一句，也就是`main:2`
