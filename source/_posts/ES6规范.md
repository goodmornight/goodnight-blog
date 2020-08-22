---
title: ES6规范
date: 2020-07-03 16:32:08
categories: notes
tags:
	- ES6
	- Javascript
---



以下内容，根据参考链接是参考Airbnb公司的JavaScript风格规范，我在此记录只为加深记忆。坚持ESLint代码风格。保持良好的代码风格，一方面是方便自己查看之前写过的代码，方便更新维护，另一方面是方便接手你代码的人。每个团队都有自己的代码规范，团队共同开发，保持一致的代码规范，是维持关系的基础。~~（以上都是我瞎说的）~~

# 编程风格

## 1.块级作用域

### (1) let替换var

`let`的特点是不会变量提升，而是被锁在当前块中。

唯一正确的使用方法：**先声明，再访问**。

### (2) 全局常量const

声明常量，一旦声明，不可更改，而且常量必须初始化赋值。

const虽然是常量，不允许修改默认赋值，但如果定义的是对象Object，那么可以修改对象内部的属性值。

`const`优于`let`有几个原因。一个是`const`可以提醒阅读程序的人，这个变量不应该改变；另一个是`const`比较符合函数式编程思想，运算不改变值，只是新建值，而且这样也有利于将来的分布式运算；最后一个原因是 JavaScript 编译器会对`const`进行优化，所以多使用`const`，有利于提高程序的运行效率，也就是说`let`和`const`的本质区别，其实是编译器内部的处理不同。

`const`声明常量还有两个好处，一是阅读代码的人立刻会意识到不应该修改这个值，二是防止了无意间修改变量值所导致的错误。

**所有的函数都应该设置为常量。**（对于这句话我还存在疑问）

长远来看，JavaScript 可能会有多线程的实现（比如 Intel 公司的 River Trail 那一类的项目），这时`let`表示的变量，只应出现在单线程运行的代码中，不能是多线程共享的，这样有利于保证线程安全。

```javascript
// bad
var a = 1, b = 2, c = 3;

// good
const a = 1;
const b = 2;
const c = 3;

// best
const [a, b, c] = [1, 2, 3];
```

### (3) const和let异同点


相同点：const和let都是在当前块内有效，执行到块外会被销毁，也不存在变量提升（TDZ），不能重复声明。
不同点：const不能再赋值，let声明的变量可以重复赋值。

const实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，const只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

## 2.字符串

静态字符串一律使用单引号或反引号，不使用双引号。动态字符串使用反引号。

```javascript
// bad
const a = "foobar";
const b = 'foo' + a + 'bar';

// acceptable
const c = `foobar`;

// good
const a = 'foobar';
const b = `foo${a}bar`;
```

ES6引入了模板字符串的特性，用反引号来表示，可以表示多行字符串以及做到文本插值（利用模板占位符）。(这块内容的实现还是有点懵)

```
# ES5
console.log('hello\n\
world')
# ES6
console.log(`hello
world`)
```

![image-20200703173037602](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/03/173249-179422.png)

可以用${}来表示模板占位符，可以将你已经定义好的变量传进括弧中。

```javascript
let name = 'night'
console.log('Hello ${name}')
```

## 3.解构赋值

使用**数组**成员对变量赋值时，优先使用解构赋值。

```javascript
const arr = [1, 2, 3, 4];

// bad
const first = arr[0];
const second = arr[1];

// good
const [first, second] = arr;
```

函数的参数如果是**对象**的成员，优先使用解构赋值。

```javascript
// bad
function getFullName(user) {
  const firstName = user.firstName;
  const lastName = user.lastName;
}

// good
function getFullName(obj) {
  const { firstName, lastName } = obj;
}

// best
function getFullName({ firstName, lastName }) {
}
```

如果函数返回多个值，**优先使用对象的解构赋值**，而不是数组的解构赋值。这样便于以后添加返回值，以及更改返回值的顺序。

```javascript
// bad
function processInput(input) {
  return [left, right, top, bottom];
}

// good
function processInput(input) {
  return { left, right, top, bottom };
}

const { left, right } = processInput(input);
```

### 扩展

[MDN--解构赋值(详细介绍)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)

## 4.对象

**单行定义的对象，最后一个成员不以逗号结尾。**

**多行定义的对象，最后一个成员以逗号结尾。**

```javascript
// bad
const a = { k1: v1, k2: v2, };
const b = {
  k1: v1,
  k2: v2
};

// good
const a = { k1: v1, k2: v2 };
const b = {
  k1: v1,
  k2: v2,
};
```

对象尽量静态化，一旦定义，就不得随意添加新的属性。如果添加属性不可避免，要使用`Object.assign`方法。

```javascript
// bad
const a = {};
a.x = 3;

// if reshape unavoidable
const a = {};
Object.assign(a, { x: 3 });

// good
const a = { x: null };
a.x = 3;
```

如果对象的属性名是动态的，可以在创造对象的时候，使用属性表达式定义。

```javascript
// bad
const obj = {
  id: 5,
  name: 'San Francisco',
};
obj[getKey('enabled')] = true;

// good
const obj = {
  id: 5,
  name: 'San Francisco',
  [getKey('enabled')]: true,
};
```

上面代码中，对象`obj`的最后一个属性名，需要计算得到。这时最好采用**属性表达式**，在新建`obj`的时候，将该属性与其他属性定义在一起。这样一来，所有属性就在一个地方定义了。

另外，对象的属性和方法，尽量采用简洁表达法，这样易于描述和书写。

```javascript
var ref = 'some value';

// bad
const atom = {
  ref: ref,

  value: 1,

  addValue: function (value) {
    return atom.value + value;
  },
};

// good
const atom = {
  ref,

  value: 1,

  addValue(value) {
    return atom.value + value;
  },
};
```

### 扩展：属性表达式

JavaScript语言定义对象的属性，有两种方法。

```javascript
let obj = {};
// 方法一
obj.foo = true;
// 方法二
obj['a'+'bc'] = 123;
document.write(obj);
```

上面代码的方法一是直接用*标识符*作为属性名，方法二是用*表达式*作为属性名，这时要将表达式放在方括号之内。

如果使用字面量方式定义对象（使用大括号），在ES5中只能使用方法一（*标识符*）定义属性。

```javascript
var obj = {
  foo: true,
  abc: 123
};
```

ES6允许字面量定义对象时，用方法二（*表达式*）作为对象的属性名，即把表达式放在方括号内。

```javascript
let propKey = 'foo';

let obj = {
   [propKey]: true,
   ['a'+'bc']: 123
};
```

表达式还可以用于定义方法名。

```javascript
let obj = {
  ['h'+'ello']() {
    return 'hi';
  }
};

document.write(obj.hello()); // hi
```

## 5.数组

使用**扩展运算符（...）**拷贝数组。

```javascript
// bad
const len = items.length;
const itemsCopy = [];
let i;

for (i = 0; i < len; i++) {
  itemsCopy[i] = items[i];
}

// good
const itemsCopy = [...items];
```

使用 Array.from 方法，将类似数组的对象转为数组。

```javascript
const foo = document.querySelectorAll('.foo');
const nodes = Array.from(foo);
```

## 6.函数

立即执行函数、匿名函数可写成箭头函数形式。**简单的、单行的、不会复用的函数，建议采用箭头函数。**如果函数体较为复杂，行数较多，还是应该采用传统的函数写法。

```javascript
(() => {
  console.log('Welcome to the Internet.');
})();

// bad
[1, 2, 3].map(function (x) {
  return x * x;
});

// good
[1, 2, 3].map((x) => {
  return x * x;
});

// best
[1, 2, 3].map(x => x * x);
```

箭头函数取代`Function.prototype.bind`，不应再用 self/_this/that 绑定 this。

```javascript
// bad
const self = this;
const boundMethod = function(...params) {
  return method.apply(self, params);
}

// acceptable
const boundMethod = method.bind(this);

// best
const boundMethod = (...params) => method.apply(this, params);
```

所有配置项都应该集中在一个对象，放在最后一个参数，布尔值不可以直接作为参数。(这个不是很理解)

```javascript
// bad
function divide(a, b, option = false ) {
}

// good
function divide(a, b, { option = false } = {}) {
}
```

不要在函数体内使用 arguments 变量，使用 rest 运算符（...）代替。因为 rest 运算符显式表明你想要获取参数，而且 arguments 是一个类似数组的对象，而 rest 运算符可以提供一个真正的数组。

```javascript
// bad
function concatenateAll() {
  const args = Array.prototype.slice.call(arguments);
  return args.join('');
}

// good
function concatenateAll(...args) {
  return args.join('');
}
```

使用默认值语法设置函数参数的默认值。

```javascript
// bad
function handleThings(opts) {
  opts = opts || {};
}

// good
function handleThings(opts = {}) {
  // ...
}
```

## 7.Map结构

注意区分 Object 和 Map，只有模拟现实世界的实体对象时，才使用 Object。如果只是需要`key: value`的数据结构，使用 Map 结构。因为 Map 有内建的遍历机制。（暂时不是很理解）

```javascript
let map = new Map(arr);

for (let key of map.keys()) {
  console.log(key);
}

for (let value of map.values()) {
  console.log(value);
}

for (let item of map.entries()) {
  console.log(item[0], item[1]);
}
```

## 8.Class

总是用 Class，取代需要 prototype 的操作。因为 Class 的写法更简洁，更易于理解。(暂时没用过)

```javascript
// bad
function Queue(contents = []) {
  this._queue = [...contents];
}
Queue.prototype.pop = function() {
  const value = this._queue[0];
  this._queue.splice(0, 1);
  return value;
}

// good
class Queue {
  constructor(contents = []) {
    this._queue = [...contents];
  }
  pop() {
    const value = this._queue[0];
    this._queue.splice(0, 1);
    return value;
  }
}
```

使用`extends`实现继承，因为这样更简单，不会有破坏`instanceof`运算的危险。

```javascript
// bad
const inherits = require('inherits');
function PeekableQueue(contents) {
  Queue.apply(this, contents);
}
inherits(PeekableQueue, Queue);
PeekableQueue.prototype.peek = function() {
  return this._queue[0];
}

// good
class PeekableQueue extends Queue {
  peek() {
    return this._queue[0];
  }
}
```

## 9.模块

首先，Module 语法是 JavaScript 模块的标准写法，坚持使用这种写法。使用`import`取代`require`。

```javascript
// bad
const moduleA = require('moduleA');
const func1 = moduleA.func1;
const func2 = moduleA.func2;

// good
import { func1, func2 } from 'moduleA';
```

使用`export`取代`module.exports`。

```javascript
// commonJS的写法
var React = require('react');

var Breadcrumbs = React.createClass({
  render() {
    return <nav />;
  }
});

module.exports = Breadcrumbs;

// ES6的写法
import React from 'react';

class Breadcrumbs extends React.Component {
  render() {
    return <nav />;
  }
};

export default Breadcrumbs;
```

如果模块只有一个输出值，就使用`export default`，如果模块有多个输出值，就不使用`export default`，`export default`与普通的`export`不要同时使用。

不要在模块输入中使用通配符。因为这样可以确保你的模块之中，有一个默认输出（export default）。

```javascript
// bad
import * as myObject from './importModule';

// good
import myObject from './importModule';
```

如果模块默认输出一个函数，函数名的首字母应该小写。

```javascript
function makeStyleGuide() {
}

export default makeStyleGuide;
```

如果模块默认输出一个对象，对象名的首字母应该大写。

```javascript
const StyleGuide = {
  es6: {
  }
};

export default StyleGuide;
```

# 参考链接

[ECMAScript6--编程风格](https://es6.ruanyifeng.com/?search=%26%26&x=15&y=10#docs/style)

[知乎--关于工作中常用到的ES6语法](https://zhuanlan.zhihu.com/p/43890979)

[汇智网--属性名表达式](http://cw.hubwiz.com/card/c/5594e91ac086935f4a6fb8ef/1/5/2/)

[MDN--解构赋值(详细介绍)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment)