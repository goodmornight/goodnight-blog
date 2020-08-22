---
title: '解决vue-perfect-scrollbar插件:data刷新，视图不刷新问题'
date: 2020-07-01 22:44:55
categories: notes
tags: 
	- VUE
	- 寒武纪项目
---



# 问题

在做大屏页面时，向大屏页面发送数据时，发现`data`对应列表的数据更新了，但屏幕上视图并没有渲染出来。

# 解决问题

后来发现是插件调用的问题。`vue-perfect-srollbar`给的demo中，加了一个`v-once`<sup>[[1]](https://github.com/lecion/vue-perfect-scrollbar/issues/26)</sup>。把`v-once`去掉即可。

然而`v-once`的作用是什么呢？

## v-once

只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。<sup>[[2]](https://cn.vuejs.org/v2/api/#v-once)</sup>

# 小结

以后若是出现，数据更新，但视图未更新的问题，可以考虑看看是不是用了`v-once`，或者是插件某些参数设置导致。

# 参考链接

[[1] github-issue-使用了本组件后 导致 组件内绑定的数据不响应](https://github.com/lecion/vue-perfect-scrollbar/issues/26)

[[2] vue-doc v-once](https://cn.vuejs.org/v2/api/#v-once)

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。