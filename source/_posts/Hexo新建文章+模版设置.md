---
title: Hexo新建文章+模版设置
categories: notes
date: 2020-07-03 20:59:09
tags:
	- Hexo
photos:
---



注意写文时间，以官方文档为主。

# 新建文章

```bash
hexo new [layout] <title>
```

新建一篇文章。如果没有设置 `layout` 的话，默认使用 [_config.yml](https://hexo.io/zh-cn/docs/configuration) 中的 `default_layout` 参数代替。如果标题包含空格的话，请使用引号括起来。

Hexo 有三种默认布局`layout`：`post`、`page` 和 `draft`。在创建者三种不同类型的文件时，它们将会被保存到不同的路径；而您自定义的其他布局和 `post` 相同，都将储存到 `source/_posts` 文件夹。

## 创建普通文章

```bash
hexo new 'hexo文章新建+模版设置'
```

## 创建草稿

```bash
hexo new draft 'hexo文章新建+模版设置'
```

这种草稿布局在建立时会被保存到 `source/_drafts` 文件夹，可通过 `publish` 命令将草稿移动到 `source/_posts` 文件夹。也可在命令中指定 `layout` 来指定布局。

```bash
hexo publish [layout] <title>
```

草稿默认不会显示在页面中，您可在执行时加上 `--draft`(这里是英文的两个短横线`-`) 参数，或是把 `render_drafts` 参数设为 `true` 来预览草稿。

```bash
hexo s --draft
```

## 创建页面

```bash
hexo new page 'categories'
```

# 设置文章模版

在你的hexo博客的根目录下有个`scaffold`文件夹，里面有三个文件`post.md`、`draft.md`、`page.md`。

其中，

`post.md`对应于`hexo new 'xxxxx'`生成的md文章的模版。

`draft.md`对应于`hexo new draft 'xxxxx'`生成的草稿。

`page.md`对应于`hexo new page 'xxxx'`生成的页面。

按需修改。我就修改`post.md`即可。

```
---
title: {{ title }}
tags:
categories:
description:
date: {{ date }}
---

点击阅读前文前, 首页能看到的文章的简短描述

<!-- more -->
```

# 参考链接

[hexo-doc--指令](https://hexo.io/zh-cn/docs/commands)

[hexo-doc--写作](https://hexo.io/zh-cn/docs/writing.html)

[hexo文章模板设置]([https://shmilybaozi.github.io/2018/11/05/hexo%E6%96%87%E7%AB%A0%E6%A8%A1%E6%9D%BF%E8%AE%BE%E7%BD%AE/](https://shmilybaozi.github.io/2018/11/05/hexo文章模板设置/))

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。