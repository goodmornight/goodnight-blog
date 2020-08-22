---
title: Request Method-HTTP请求方法
categories: notes
date: 2020-07-03 22:43:10
tags:
	- HTTP
photos:
---





学到一点，补一点。

# Request Method

## GET

`GET`方法请求一个指定资源的表示形式. 使用GET的请求应该只被用于获取数据.

## HEAD

`HEAD`方法请求一个与GET请求的响应相同的响应，但没有响应体.

## POST

`POST`方法用于将实体提交到指定的资源，通常导致在服务器上的状态变化或副作用. 

## PUT

`PUT`方法用请求有效载荷替换目标资源的所有当前表示。

## DELETE

`DELETE`方法删除指定的资源。

## CONNECT

`CONNECT`方法建立一个到由目标资源标识的服务器的隧道。

## OPTIONS

`OPTIONS`方法用于描述目标资源的通信选项。

经常可以在**chrome**浏览器的控制台里可以看到，在发送**GET**或**POST**请求前，都会有个**OPTIONS**请求。

>**浏览器在某些请求中，在正式通信前会增加一次HTTP查询请求，称为"预检"请求（preflight）。** **OPTIONS方法是用于请求获得由Request-URI标识的资源在请求/响应的通信过程中可以使用的功能选项。通过这个方法，客户端可以在采取具体资源请求之前，决定对该资源采取何种必要措施，或者了解服务器的性能。** **该请求方法的响应不能缓存。** **浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的XMLHttpRequest请求，否则就报错。**

简单地说，

1. `OPTIONS`是在某种请求前，浏览器获取服务器支持请求的方法。
2. 可用来检测服务器性能。

通过**OPTIONS**，再简单了解一下跨域问题**COR**。

### CORS

**CORS**是一个W3C标准，全称是"跨域资源共享"（Cross-origin resource sharing）。

它允许浏览器向跨源服务器，发出**XMLHttpRequest**请求，从而克服了**AJAX**只能同源使用的限制。

>CORS需要浏览器和服务器同时支持。目前，所有浏览器都支持该功能，IE浏览器不能低于IE10。
>整个CORS通信过程，都是浏览器自动完成，不需要用户参与。对于开发者来说，CORS通信与同源的AJAX通信没有差别，代码完全一样。浏览器一旦发现AJAX请求跨源，就会自动添加一些附加的头信息，有时还会多出一次附加的请求，但用户不会有感觉。
>因此，实现CORS通信的关键是服务器。只要服务器实现了CORS接口，就可以跨源通信。

**CORS**分两种请求：

1. 简单请求（simple request）
2. 非简单请求（not-so-simple request）

#### 1.简单请求

需同时满足以下条件：

  (1) 请求方法

```
  - HEAD
  - GET
  - POST
```

   (2) HTTP请求头不超出以下字段

```
 - Accept
 - Accept-Language
 - Content-Language
 - Last-Event-ID
 - Content-Type:只限于三个值application/x-www-form-urlencoded、multipart/form-data、text/plain
```

对于简单请求，浏览器发送**CORS**请求，就是头部加个**Origin**字段。

**Origin**字段用来说明，本次请求来自哪个源（协议 + 域名 + 端口）。服务器根据这个值，决定是否同意这次请求。

如果Origin指定的源，不在许可范围内，服务器会返回一个正常的HTTP回应。浏览器发现，这个回应的头信息没有包含`Access-Control-Allow-Origin`字段（详见下文），就知道出错了，从而抛出一个错误，被**XMLHttpRequest**的`onerror`回调函数捕获。注意，这种错误无法通过状态码识别，因为**HTTP**回应的状态码有可能是200。

如果Origin指定的域名在许可范围内，服务器返回的响应，会多出几个头信息字段。都以**Access-Control-** 开头：

**（1）Access-Control-Allow-Origin**

该字段是必须的。它的值要么是请求时**Origin**字段的值，要么是一个*，表示接受任意域名的请求。

> 需要注意的是，如果要发送Cookie，Access-Control-Allow-Origin就不能设为星号，必须指定明确的、与请求网页一致的域名。同时，Cookie依然遵循同源政策，只有用服务器域名设置的Cookie才会上传，其他域名的Cookie并不会上传，且（跨源）原网页代码中的document.cookie也无法读取服务器域名下的Cookie。

**（2）Access-Control-Allow-Credentials**

该字段可选。它的值是一个布尔值，表示是否允许发送**Cookie**。默认情况下，**Cookie**不包括在CORS请求之中。设为`true`，即表示服务器明确许可，**Cookie**可以包含在请求中，一起发给服务器。这个值也只能设为`true`，如果服务器不要浏览器发送**Cookie**，删除该字段即可。

**（3）Access-Control-Expose-Headers**

该字段可选。**CORS**请求时，**XMLHttpRequest**对象的`getResponseHeader()`方法只能拿到6个基本字段：`Cache-Control`、`Content-Language`、`Content-Type`、`Expires`、`Last-Modified`、`Pragma`。如果想拿到其他字段，就必须在Access-Control-Expose-Headers里面指定。

#### 2.非简单请求

**非简单请求是那种对服务器有特殊要求的请求，比如请求方法是PUT或DELETE，或者Content-Type字段的类型是application/json。**

非简单请求的**CORS**请求，会在正式通信之前，增加一次HTTP查询请求，称为“预检”请求（preflight）。"预检"请求用的请求方法就是`OPTIONS`，表示这个请求是用来询问的。

浏览器先询问服务器，当前网页所在的域名是否在服务器的许可名单之中，以及可以使用哪些HTTP动词和头信息字段。只有得到肯定答复，浏览器才会发出正式的**XMLHttpRequest**请求，否则就报错。

在页面域名与接口域名不一致的情况下，就出现了每次请求前先发送一个options请求的问题。

**OPTIONS**请求头信息中，除了**Origin**字段，还至少会多两个特殊字段：

**（1）Access-Control-Request-Method**

该字段是必须的，用来列出浏览器的**CORS**请求会用到哪些HTTP方法。

**（2）Access-Control-Request-Headers**

该字段是一个逗号分隔的字符串，指定浏览器**CORS**请求会额外发送的头信息字段

服务器收到预检请求后，检查了`Origin`、`Access-Control-Request-Method`和`Access-Control-Request-Headers`字段以后，确认允许跨源请求，就可以做出回应。

如果浏览器否定了"预检"请求，会返回一个正常的**HTTP**回应，但是没有任何**CORS**相关的头信息字段。这时，浏览器就会认定，服务器不同意预检请求，因此触发一个错误，被**XMLHttpRequest**对象的`onerror`回调函数捕获。控制台会打印出如下的报错信息。

其他字段中**Access-Control-Max-Age** 用来指定本次预检请求的有效期，单位为秒。该字段可选。



## TRACE

`TRACE`方法沿着到目标资源的路径执行一个消息环回测试。

## PATCH

`PATCH`方法用于对资源应用部分修改。



# 参考链接

[MDN--HTTP请求方法](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Methods)

[掘金--由Request Method:OPTIONS初窥CORS](https://juejin.im/entry/5a36176051882574d23c6bad)

[阮一峰--跨域资源共享 CORS 详解](https://www.ruanyifeng.com/blog/2016/04/cors.html)

---

该文主要是多方资料的汇总和个人理解。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。