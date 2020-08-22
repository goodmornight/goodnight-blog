---
title: import/export/export default in ES6
categories: notes
date: 2020-07-11 10:49:05
tags:
	- ES6
photos:
---



# 简言之

## export

1. 输出
2. **Module**中可以有多个**export**
3. 后面接的内容是一个等式或者一个函数
4. **import**时，需要与**Module**中的对应的函数名一致

## export default

首先是**export default为什么存在？**

别人在使用已经写好的**Module**的时候，不需要知道内部有什么函数，函数名是什么，可以直接**import**进来，并且可以自定义引入模块，更加方便。

1. 输出
2. **Module**中只能有一个**export default**
3. 后面接的是一个变量名或函数都可
4. **import**时名字可以自定义

## import

1. 输入

## 代码示例

```javascript
// 在模块test_export.js中

export function f1 () {
    // ...
}

export function f2 () {
    // ...
}

// main.js引用
import { f1, f2 } from './test_export'
```

```javascript
// 在模块test_export_default.js中

function f () {
    // ...
}

default export f

// main.js引用
import fn from './test_export'
```



# 细说之

## 前生

在 ES6 之前，社区制定了一些模块加载方案，最主要的有 CommonJS 和 AMD 两种。前者用于服务器，后者用于浏览器。CommonJS 和 AMD 模块，都只能在运行时确定这些东西（这种加载称为“运行时加载”）。比如，CommonJS 模块就是对象，输入时必须查找对象属性。

## export

变量写法：

```javascript
// 写法一
export var m = 1;

// 写法二
var m = 1;
export {m};

// 写法三
var n = 1;
export {n as m};
```



函数写法：

```javascript
// 报错
function f1() {}
export f1;

// 正确
export function f2() {};

// 正确
function f3() {}
export {f3};
```



# 参考链接：

[ECMAScript6入门 | 阮一峰](https://es6.ruanyifeng.com/?search=global&x=4&y=5#docs/module)

[exports、module.exports和export、export default到底是咋回事 | segmentfault](https://segmentfault.com/a/1190000010426778)

[ES6 export && export default 差异总结 | 掘金](https://juejin.im/post/5abe3ecf6fb9a028e25dab77)

---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。