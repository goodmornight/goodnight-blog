---
title: Vue中返回上一页面函数实现
categories: notes
date: 2020-07-14 16:08:51
tags:
	- VUE
	- 寒武纪项目
photos:
---





只是做一个简单的记录。省的自己以后还要查。我觉得这是目前我认为比较周全的返回写法。

在dashboard页面中，有一个需要返回上一页面的按钮，这个按钮我给它写了一个方法。

```vue
export default {
	methods: {
		// 返回按钮
        back() {
          window.history.length > 1 ? this.$router.go(-1) : this.$router.push('/')
        },
	}
}
<template>
<i class="uil uil-corner-up-left-alt" @click="back" style="font-size: 1.5rem;cursor: pointer;color:#ccc"></i>
</template>
```

判断该页面之前有没有其他页面，有的话就回退到上一页面，反之，返回主页。





---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。