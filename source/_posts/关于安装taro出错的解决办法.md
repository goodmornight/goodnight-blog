---
title: 关于安装taro出错的解决办法
date: 2019-01-29 13:45:38
tags:
	- taro
	- 微信小程序
	- UI
categories: notes
icon: /images/notes.svg
---

昨晚发现了看了一篇文章[推荐5个小程序开源 UI 组件库，个个都优秀，第一个很凹凸...](https://mp.weixin.qq.com/s/YrQyCFXeb9VISk4DmCScJQ)是有关UI库的，正好我最近准备写一个微信小程序，正愁UI怎么办，看到这篇文章，激动得我睡不着觉。 

我最后选择试试taro-ui库，这是由京东凹凸实验室推出的一套多端UI组件库（[github传送门](https://github.com/NervJS/taro-ui)，[使用手册](https://taro-ui.aotu.io/#/docs/introduction)）。

关于安装的详细过程，他们在github和使用手册里已经写的很清楚了，但前提你的电脑里要装有npm或yarn，以及node.js。

这篇文章主要是写我安装出现问题的解决过程。

**我的环境是win10 x64**

正常的安装过程：

**1.先安装Taro开发工具`tarojs/cli`，可以用npm或yarn进行全局安装（即安装路径为~即可）或者用npx**

```bash
npm install -g @tarojs/cli  #用npm安装
yarn global add @tarojs/cli  #用yarn安装 
#选其中一个即可
```

**2.初始化项目**

创建命令模版项目（后面这个"xidian_boy"是自己随便取的）

```bash
taro init xidian_boy
```

### 然而我在这里出现了问题！！

```bash
C:\****\****\AppData\Roaming\npm\node_modules\@tarojs\cli\src\util\index.js:22                                                      7
exports.replaceAsync = async function (str, regex, asyncFn) {
                             ^^^^^^^^

SyntaxError: Unexpected token function
    at createScript (vm.js:56:10)
    at Object.runInThisContext (vm.js:97:10)
    at Module._compile (module.js:542:28)
    at Object.Module._extensions..js (module.js:579:10)
    at Module.load (module.js:487:32)
    at tryModuleLoad (module.js:446:12)
    at Function.Module._load (module.js:438:3)
    at Module.require (module.js:497:17)
    at require (internal/module.js:20:19)
    at Object.<anonymous> (C:\Users\80581\AppData\Roaming\npm\node_modules\@taro                                                      js\cli\bin\taro:4:44)
```

后来我去查，了解到可能是我的node版本太低了，因此我需要把**node升级**了,然后我顺带把npm也升级了。

- (1)先查看我的npm，node版本

```bash
npm -v  #查看npm版本
node -v #查看node版本
```

- (2)升级npm

```bash
npm install npm -g
```

-  (3)升级node.js时，先查看自己的node装在哪，然后在node官网上下载新的版本安装到该路径下覆盖即可。

```bash
#打开cmd,输入
where node
```

然后再次运行刚刚的代码初始化代码，后面就没啥问题了。

参考链接：

  [windows10下升级node.js](https://www.jianshu.com/p/25c053b246a1)

  [新的小程序开发框架？-Taro的深度体验](https://segmentfault.com/a/1190000015432413)

**3.安装Taro UI**

```bash
cd xidian_boy
npm install taro-ui
#或者使用自定义定义主题版本，使用手册上说建议都升级到next版本
npm install taro-ui@next
```

**4.使用Taro UI**

引用部分，详情请看[使用手册](https://taro-ui.aotu.io/#/docs/quickstart)，一切以[官方使用手册](https://taro-ui.aotu.io/#/docs/quickstart)为主。