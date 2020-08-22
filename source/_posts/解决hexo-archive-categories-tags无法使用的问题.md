---
title: 解决hexo archive/categories/tags无法使用的问题
date: 2020-07-01 20:09:55
categories: notes
tags:
	- Hexo
---

（个人觉得如下内容，对于大部分Hexo搭建的博客都是通用的。但请注意文章记录时间）

# 问题

点击主页右边的`Archives`/`Categories`/`Tags`查看，发现会报类似于找不到这个链接的错误。（个人理解）

![image-20200701201335639](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/01/201415-904859.png)

页面无法显示，报错：

```
Cannot GET /tags/CSS/
```

# 解决问题

## 插件安装

查看你的`node_modules`文件夹内是否安装对应Hexo归档的插件，如下，按需安装

```
hexo-generator-archive
hexo-generator-category
hexo-generator-tag
```

这几个插件是可以让你在`hexo g`的时候生成public在对应文件夹`archives`、`categories`、`tags`内可以分类你文章中设置好的信息。

安装插件，可以Google搜索，然后进入对应`hexojs`的仓库下载，对应页面也会详细介绍相关参数设置，贴心的链接如下。

[hexo-generator-tag](https://github.com/hexojs/hexo-generator-tag)

[hexo-generator-archive](https://github.com/hexojs/hexo-generator-archive)

[hexo-generator-category](https://github.com/hexojs/hexo-generator-category)

注意，安装好插件后，别忘了在博客根目录内的`_config.yml`文件内进行相关配置。

## 创建页面

通过查看发现，生成的静态文件夹里没有对应的索引。比如在`/public`文件内没有tags

那么说明你没有创建这个页面。

可以执行如下代码，创建新的一个页面

```bash
hexo new page archives     # 创建archives页面
hexo new page categories   # 创建categories页面
hexo new page tags         # 创建tags页面
```

然后在`source`内就可以看到对应的文件夹

![image-20200701202019813](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/01/202025-943000.png)

接下来在你选择的主题文件里的`_config.yml`进行相关配置。

```
menu:
  home: /
  categories: /categories
  archives: /archives
  tags: /tags
```

## 文章配置

用hexo创建一篇文章。

```
hexo new "文章题目"
```

在文章头那块，进行相关配置，类似下面内容：

```
title: 解决hexo archive/categories/tags无法使用的问题
date: 2020-07-01 20:09:55
categories: Notes
tags: hexo
```

## 最后

```javascript
hexo clean # 删除原来的public静态文件
hexo g     # 生成新的public静态文件，可以进/public文件内看看对应文件夹有没有生成
hexo s     # 本地运行测试一下
```

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。

