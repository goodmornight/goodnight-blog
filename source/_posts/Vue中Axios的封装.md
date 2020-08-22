---
title: Vue中Axios的封装
categories: notes
date: 2020-07-10 09:36:46
tags:
	- VUE
	- Axios
	- 寒武纪项目
	- 前端
photos:
---



# Axios简介

Axios 是一个基于 promise 的 HTTP 库，可以用在浏览器和 node.js 中。

## 特性

- 从浏览器中创建 [XMLHttpRequests](https://developer.mozilla.org/en-US/docs/Web/API/XMLHttpRequest)
- 从 node.js 创建 [http](http://nodejs.org/api/http.html) 请求
- 支持 [Promise](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise) API
- 拦截请求和响应
- 转换请求数据和响应数据
- 取消请求
- 自动转换 JSON 数据
- 客户端支持防御 [XSRF](http://en.wikipedia.org/wiki/Cross-site_request_forgery)

详细信息可以查看[axios文档](https://www.npmjs.com/package/axios)。

# 安装Axios

```bash
npm install axios; // 安装axios
```

# 封装Axios

我在项目的**src**目录下，新建了一个**request**文件夹，当中建了文件**http.js**

## 引入

这里肯定是需要引入**Axios**的，但**vue**的引入根据需求，因为我的**token**是根据`Vue.prototype.$keycloak`全局设定获取的。

```javascript
import axios from 'axios'
import Vue from 'vue'
```

## 创建示例+固定信息设置

```javascript
// 创建axios实例
let instance = axios.create({ timeout: 1000 * 12 })

// 设置默认请求地址
instance.defaults.baseURL = process.env.VUE_APP_API_URL
```

## 请求拦截器

因为该项目使用的是**keycloak**作为审核登陆系统。所以这边的**token**是通过我在项目中全局设置的**keycloak**获取的**token**。

请求头内容，根据各自需要设置。

```javascript
// 请求拦截器,每次请求前,如果存在token,ip则在请求头中携带token,ip
instance.interceptors.request.use(

    config => {        
        // 请求头加上token,ip       
        const ip = localStorage.getItem('dh-ip')
        const token = Vue.prototype.$keycloak.token

        ip && (config.headers['X-Forwarded-For'] = ip)
        token && (config.headers.Authorization = 'Bearer ' + token)

        return config
    },
    error => Promise.error(error)

)
```

这里的`&&`的使用，如果不清楚什么意思的话，可以看看我之前写的[逻辑运算符 && / || / !](http://www.goodnight.wiki/2020/07/07/逻辑运算符/)

## 响应拦截器

根据服务器返回状态码，判断请求是否报错。

这里写了一个根据状态码，采取不同操作的函数。

```javascript
/** 
 * 请求失败后的错误统一处理 
 * @param {Number} status 请求失败的状态码
 */
function errorHandle (status, other) {
    // 状态码判断
    switch (status) {
        // 401: token过期，删除缓存，并刷新页面
        case 401:
        	localStorage.removeItem('dh-token')
        	localStorage.removeItem('dh-refresh-token')
        	window.location.reload()
            break;
        // 403: ACCESS_DENIED 权限不够
        case 403:
        	console.log('权限不足')
            break;
        // 404请求不存在
        case 404:
            console.log('请求的资源不存在');
            break;
        default:
            console.log(other);   
    }
}
```



```javascript
// 响应拦截器,根据状态码进行一些统一的操作
instance.interceptors.response.use(    
    // 请求成功
    res => res.status === 200 ? Promise.resolve(res) : Promise.reject(res),    
    // 请求失败
    error => {
    	console.log(error)
        const { response } = error;
        if (response) {
            // 请求已发出，但是不在2xx的范围 
            errorHandle(response.status, response.data.message)
            return Promise.reject(response)

        } else {
            // TODO 处理断网的情况
            // eg:请求超时或断网时，更新state的network状态
            // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
            // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
            console.log('断网：' + error)
        }
    }
)
```

## 最后输出

```javascript
export default instance
```

# 代码

```javascript
import axios from 'axios'
import Vue from 'vue'

/** 
 * 请求失败后的错误统一处理 
 * @param {Number} status 请求失败的状态码
 */
function errorHandle (status, other) {
    // 状态码判断
    switch (status) {
        // 401: token过期，删除缓存，并刷新页面
        case 401:
        	localStorage.removeItem('dh-token')
        	localStorage.removeItem('dh-refresh-token')
        	window.location.reload()
            break;
        // 403: ACCESS_DENIED 权限不够
        case 403:
        	console.log('权限不足')
            break;
        // 404请求不存在
        case 404:
            console.log('请求的资源不存在');
            break;
        default:
            console.log(other);   
    }
}

// 创建axios实例
let instance = axios.create({ timeout: 1000 * 12 })

// 设置默认请求地址
instance.defaults.baseURL = process.env.VUE_APP_API_URL

// 请求拦截器,每次请求前,如果存在token,ip则在请求头中携带token,ip
instance.interceptors.request.use(

    config => {        
        // 请求头加上token,ip       
        const ip = localStorage.getItem('dh-ip')
        const token = Vue.prototype.$keycloak.token

        ip && (config.headers['X-Forwarded-For'] = ip)
        token && (config.headers.Authorization = 'Bearer ' + token)

        return config
    },
    error => Promise.error(error)

)

// 响应拦截器,根据状态码进行一些统一的操作
instance.interceptors.response.use(    
    // 请求成功
    res => res.status === 200 ? Promise.resolve(res) : Promise.reject(res),    
    // 请求失败
    error => {
    	console.log(error)
        const { response } = error;
        if (response) {
            // 请求已发出，但是不在2xx的范围 
            errorHandle(response.status, response.data.message)
            return Promise.reject(response)

        } else {
            // TODO 处理断网的情况
            // eg:请求超时或断网时，更新state的network状态
            // network状态在app.vue中控制着一个全局的断网提示组件的显示隐藏
            // 关于断网组件中的刷新重新获取数据，会在断网组件中说明
            console.log('断网：' + error)
        }
    }
)

export default instance
```



# 参考链接

[掘金--vue中Axios的封装和API接口的管理](https://juejin.im/post/5b55c118f265da0f6f1aa354#heading-10)

[axios中文文档](http://axios-js.com/zh-cn/docs/index.html)





---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。