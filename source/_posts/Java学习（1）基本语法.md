---
title: 我的Java学习笔记（1）基本语法
date: 2019-05-17 22:49:40
tags:
	- Java
categories: notes
icon: /images/notes.svg
---

# Java学习需要注意的地方

- **大小写敏感**；
- **类名：类名首字母应大写**，如几个单词组成则每个单词开头第一个字母都大写；
- **方法名：方法名首字母应小写**，但几个单词组成的方法名除首字母外，其余单词的第一个字母都应大写；
- **源文件名：源文件名必须和类名一样，注意大小写**；
- **主方法入口**：所有的 Java 程序由 **public static void main(String []args)** 方法开始执行。
- **标识符：所有的标识符都应该以字母（A-Z 或者 a-z）,美元符（$）、或者下划线（_）开始，注意不可以数字开头**。

# Java中的基本概念

Java作为面向对象的语言，支持以下概念：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-2-1.png?raw=true)

# Java修饰符

修饰符用于修饰类和方法的属性，主要分为两大类：

- **访问控制修饰符 : default, public , protected, private**
- **非访问控制修饰符 : final, abstract, static, synchronized**

#Java变量

- 局部变量
- 类变量（静态变量）
- 成员变量（非静态变量）

# Java关键字

这里引用了菜鸟教程里的表格：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-1-2.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-1-3.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-1-4.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-1-5.png?raw=true)

# Java注释

```java
//这是一种单行注释
/*这是第二种单行注释*/
/*
*这是一种多行注释
*/
```

# Java语法思维导图

网上有人将菜鸟教程里的Java基本语法里的内容做成了思维导图，再此就借用一下：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/JavaStudy/Java-1-1.png?raw=true)

**参考连接**：

[菜鸟教程-Java基本语法](https://www.runoob.com/java/java-basic-syntax.html)

[掘金-Java 基础思维导图，让 Java 不再难懂](https://juejin.im/entry/58e467a961ff4b006b2fb7f9)

---

写这篇Java笔记的原因主要是记一些我学习Java过程中的笔记，我想要写一个更适合我后期回顾的Java笔记。其中引用了不少菜鸟教程里的内容，如有侵权，请联系我，谢谢~