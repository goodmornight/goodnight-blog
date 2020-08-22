---
title: vuejs-logger使用
categories: notes
date: 2020-07-09 21:32:06
tags:
	- VUE
	- Translation
photos:
---



（逼着自己看一遍英文的）

# 介绍

**vuejs-logger**是用于Vue应用开发的日志记录工具。特征如下：

- 基于所选的日志级别输出限制
- 自动将JSON字符串化（`JSON.stringify()`）传给记录器
- 可选配置日志自定义输出内容
- 针对于`$log.warning`、`$log.error`、`$log.fatal`等级的不同颜色的控制台信息输出

日志输出等级有：

- debug/调试
- info/信息
- warn/警告
- error/错误
- fatal/致命

# 安装

该项目使用[node](http://nodejs.org/)和[npm](https://npmjs.com/)。

```bash
npm install vuejs-logger --save-exact
```



# 使用

## 参数

| 名称               | 是否必填 | 内容类型 | 默认内容 | 描述介绍                                                     |
| ------------------ | -------- | -------- | -------- | ------------------------------------------------------------ |
| isEnabled          | false    | Boolean  | true     | 是否启用vuejs-logger插件，可针对于生产模式/开发模式切换。    |
| logLevel           | false    | String   | 'debug'  | 可选项有['debug', 'info', 'warn', 'error', 'fatal']。        |
| stringifyArguments | false    | Boolean  | false    | 如果为true，则将所有输入都JSON字符化，在打印时有效。         |
| showLogLevel       | false    | Boolean  | false    | 如果为true，日志等级logLevel将会显示。                       |
| showMethodName     | false    | Boolean  | false    | 如果为true，则控制台将显示方法的父函数名称                   |
| separator          | false    | String   | '\|'     | 输出内容的分割线                                             |
| showConsoleColors  | false    | Boolean  | false    | 如果为true，则为相应的日志级别启用console.warn，console.fatal，console.error。 |

## 例子

**main.js**文件里，

```javascript
import VueLogger from 'vuejs-logger';
const isProduction = process.env.NODE_ENV === 'production';
 
const options = {
    isEnabled: true,
    logLevel : isProduction ? 'error' : 'debug',
    stringifyArguments : false,
    showLogLevel : true,
    showMethodName : true,
    separator: '|',
    showConsoleColors: true
};
 
Vue.use(VueLogger, options);
```

vue文件里，

```javascript
new Vue({
    data() {
        return {
            a : 'a',
            b : 'b'
        }
    },
    created() {
        this.$log.debug('test', this.a, 123)
        this.$log.info('test', this.b)
        this.$log.warn('test')
        this.$log.error('test')
        this.$log.fatal('test')
        externalFunction()
    }
});
 
function externalFunction() {
   // log from external function
   Vue.$log.debug('log from function outside component.');
}
```

![31655570-910fcbbe-b329-11e7-9738-bece4be4d1a8](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/09/215328-18956.png)

# production（生产模式）小贴士

在生产模式插件可以被禁止，或者设置较低日志级别logLevel最小化输出，如下。如果日志级别被设置为‘fatal’(致命)，插件将会忽略代码中不太重要的日志级别的所有调用。

```
function foo() {
        // 如果logLevel设置为“fatal”，则这些语句将不显示任何内容。但是它们会编译。
        this.$log.debug('test', 'bar')
        this.$log.info('test')
        this.$log.warn('test')
        this.$log.error('test', 'foo')
        // 如果logLevel设置为'fatal'，则将显示此语句
        this.$log.fatal('test', 'bar', 123)
    }
```



# 参考链接

[npm--vuejs-logger](https://www.npmjs.com/package/vuejs-logger)



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。