---
title: '有作用域的CSS:解决VUE路由跳转样式错乱问题'
date: 2020-06-27 20:59:38
tags:
	- CSS
	- VUE
categories: notes
photos:
	- https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/27/210228-266840.png
---

![vue-newsletter](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/27/210228-266840.png)

# 问题描述

今天在优化VUE项目中的返回按钮，结果发现直接使用`this.$router.replace('/')`跳转到主页面，发现跳转A页面的背景色出现在了主页面里，样式出现了问题。

# 解决方法

在A页面中，`<style>`里添加`scoped`属性。

而这个属性的作用是**让该部分的css样式只在当前页面加载**。

>scoped属性（可选属性）：会自动添加一个唯一的属性 (比如 `data-v-21e5b78`) 为组件内 CSS 指定作用域，即css样式只在当前页面加载<sup>[[1]](https://blog.csdn.net/qq_44333271/article/details/88175219)</sup>

修改如下：

在`<style>`加了一个`scoped`

```css
<style scoped>
  body {
    background: linear-gradient(180deg, #2a1b3d, #0f0f0f);
    color: #fff;
    padding:0;
  }
</style>
```

从控制台里可以看到给这部分css加了类似于id的名称。

![image-20200627211346255](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/27/211347-40647.png)

# 涉及内容

`<style>`里添加`scoped`属性，属于**vue-loader**的内容了。<sup>[[2]](https://vue-loader-v14.vuejs.org/zh-cn/features/scoped-css.html)</sup>

| TODO(2020/06/27) | Content                             |
| ---------------- | ----------------------------------- |
| 1                | 阅读vue-loader文档                  |
| 2                | 补充`<style>`标签的`scoped`属性内容 |

# 参考链接

[1] [CSDN--vue 路由跳转，导致页面样式错乱，刷新又好了的情况](https://blog.csdn.net/qq_44333271/article/details/88175219)

[2] [vue-loader--有作用域的CSS](https://vue-loader-v14.vuejs.org/zh-cn/features/scoped-css.html)

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。