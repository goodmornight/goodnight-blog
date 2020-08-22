---
title: vue keycloak实现页面权限控制
categories: notes
date: 2020-07-18 11:30:55
tags:
	- VUE
	- keycloak
	- 寒武纪项目
photos:
---







刚开始使用keycloak的时候，很多接口参数不知道，就感觉你访问任何页面都需要登录才可访问。但项目需要某些页面不需要用户登录也可以查看。我通过[Protected urls #2 | github issue](https://github.com/dsb-norge/vue-keycloak-js/issues/2)受到了启发。

keycloak中的`init.onLoad`可选两个参数`login-required`、`check-sso`。其中，`login-required`会检查你是否登录到了keycloak以及判断你登录的客户端是否正确，如果没有登录就跳转到keycloak的登录页面；`check-sso`只会判断你登录的客户端是否正确，如果没有登录就会保持未登录的状态。

所以`check-sso`是我所需要的。

我是使用vue开发的项目，用的插件是[@dsb-norge/vue-keycloak-js | npm](https://www.npmjs.com/package/@dsb-norge/vue-keycloak-js)。以下内容均是基于这个插件的。

# 初始化设置

最开始的开始，当然是安装插件了，这块就不需要我多说了，建议用**cnpm**安装，这个比较快。

在**main.js**文件里，添加使用**vue-keycloak-js**代码。

用**token**不需要另外保存到**localStorage**了，只需要通过**this.$keycloak.token**获取即可。

```javascript
import VueKeyCloak from '@dsb-norge/vue-keycloak-js'
Vue.use(VueKeyCloak, {
  init: {
    onLoad: 'check-sso'
  },
  config: {
    url: process.env.VUE_APP_AUTH_URL, realm: process.env.VUE_APP_AUTH_REALM, clientId: process.env.VUE_APP_AUTH_CLIENT_ID,
  },
  onReady: kc => {

    // 判断用户是否登录，如果登录了就向数据库请求用户信息
    if(kc.authenticated){
      // 获取用户信息
      http.get('_users/' + kc.subject)
      .then((response) => {
        store.commit('auth/SET_CURRENT_USER', response.data.data)
      })
      .catch((error) => {
        console.log(error)
      })
        
    }
    
    new Vue({
      router,
      store,
      created () {
        // 监听用户信息，用户信息改变则重新从数据库里请求用户信息
        this.$watch('$keycloak.userInfo', function () {

          if(kc.authenticated){

            http.get('_users/' + kc.subject)
            .then((response) => {
              store.commit('auth/SET_CURRENT_USER', response.data.data)
            })
            .catch((error) => {
              console.log(error)
            })
            
          }

        }, { immediate: true })

      },
      render: h => h(App)
    }).$mount('#app')
  }
})
```

# 路由设置

routes.js

```javascript
export default [
  {
    path: '/',
    name: 'main',
    component: Main
  },
  {
    path: '/secured',
    name: 'secured',
    component: Secured,
    meta: {
      requiresAuth: true
    }
  }
]
```

router.js

```javascript
router.beforeEach((routeTo, routeFrom, next) => {

  // 页面是否需要用户验证
  const authRequired = routeTo.matched.some((route) => route.meta.authRequired)

  // 如果该页面不需要登陆验证，则直接进入
  if (!authRequired) return next()

  // 如果该页面需要认证，则判断该用户是否已登录
  if (Vue.prototype.$keycloak.authenticated) {

    // 判断用户是否有管理员权限，管理员进入页面不受限制
    if (Vue.prototype.$keycloak.hasRealmRole('C_ADMIN')) return next()

    // 用户为普通用户，判断是否有进入该页面的权限
    if (userRoutes.some(route => route.path === routeTo.path)) return next()

    // 普通用户，无进入该页面权限，重定向可进入的页面
    next({ path: userRoutes[0].path })

  } else {
    // 未登录则进入登录界面
    const loginUrl = Vue.prototype.$keycloak.createLoginUrl()
    window.location.replace(loginUrl)

  }

})
```

# 主页跳转问题

后来我发现这样设置，出现了一点点小问题。

如果你退出账户时，不在首页，则重新登录就直接跳转到你原来所在的页面。

这个问题看起来没有太大的毛病，但是如果是普通用户和管理员用户之间切换，那么管理员状态登录就无法直接进入主页。

这个问题我尝试了很多种方法：

1. 删除路由历史记录（其实这个不是问题的根本，所以这么做是无意义的，况且Vue Router不存在清空历史记录的函数和功能）
2. 尝试从刚进入页面时进行全局路由设置，会导致next()循环
3. 通过退出用户，设置跳转主页，可实现管理员下次系统进入主页，但无法实现普通用户切换管理员进入非主页问题

最后的解决方法是，单独对每一个路由进行设置，如果是初次进入系统，进入页面如果不是主页就跳转到主页。以我的某个页面为例：

```javascript
const containersRoutes = [
  {
    path: '/containerList',
    name: '容器列表',
    icon: 'box',
    component: () => lazyLoadView(import('@views/docker-hub/containerList')),
    meta: {

      authRequired: true,
      beforeResolve(routeTo, routeFrom, next) {

        // 非初次进入系统
        if (routeFrom.name !== null) return next()
        // 初次进入系统，管理员进入主机页面，普通用户进入镜像列表
        if(Vue.prototype.$keycloak.hasRealmRole('C_ADMIN')) return next('/')
        return next('/imageList')

      },

    },
    props: (route) => ({ user: store.state.auth.currentUser || {} })
  }
]

```





# 参考链接

[Vue集成Keycloak | 简书](https://www.jianshu.com/p/4ff7f0e491c3)

[Protected urls #2 | github issue](https://github.com/dsb-norge/vue-keycloak-js/issues/2)

[@dsb-norge/vue-keycloak-js | npm](https://www.npmjs.com/package/@dsb-norge/vue-keycloak-js)





---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。