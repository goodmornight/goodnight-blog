---
title: Translation:Hexo Theme--Cutie(Official Document)
date: 2017-08-14 10:07:53
tags: 
	- Translation
	- Hexo
categories: projects
icon: /images/projects.svg
abstract: 
	现在早已有新的关于这个主题的文档，这篇文章也算是基本没什么用了，等我有空，再翻译一下新的文档。——2019.4.3
---


# Hexo Theme:Cutie

​									**原作者 by Qu Tang**

​									**翻译 by Spyxxx**

*cutie*是一个反应敏捷的*hexo*主题，受简洁友好用户设计的[www.linpx.com](http://www.linpx.com/)影响而制作。

## 简介

### 特征

- 反应敏捷的设计
  - 单栏和双栏布局
  - 底部导航菜单
- 可配置结构
  - 导航菜单（名字，链接，图标）
  - 由Disqus实现的用户评论部分
  - 谷歌分析
  - 标语
  - 签名
- 额外的内容
  - *Mathjax*(一款相当强悍的在网页显示数学公式的插件)
  - *Instant click*(一个很小的JavaScript库，可以大大提高网站响应速度)
  - 搜索网页模版
  - *Lightbox*支持（一种jQuery插件）

### Demo(演示)

访问我的[个人网页](https://qutang.github.io/)演示。 

## 毫无疑问第一次使用想要有个Demo网页的用户都已经准备好去做了

1.从github库里拷贝[https://github.com/qutang/theme-cutie-demo](https://github.com/qutang/theme-cutie-demo)（fork是github里的一种操作）

2.拷贝到你的本地机器上

3.运行`hexo server`  

## 安装和使用

1.拷贝到你的hexo网站的`themes`(主题)文件夹里，并重命名为'`cutie`' 

```bash
git clone https://github.com/qutang/hexo-theme-cutie.git
```

1.在网站的`_config.yml`文件里关于主题的示例片段

```yaml
# 注意缩进
theme: cutie

google-analytics: UA-xxxxxxxx-1

cutie:
  slogan: This is a slogan.
  signature: SIGNATURE

# 配置导航菜单，提供一个相关链接和图标路径（图标最好是正方形）
# 如果菜单未配置，只有“存档”和“搜索”菜单项将被保留。
# 配置菜单项不应超过4个，否则后面的项将被忽略

  menu:
    Resume: 
      link: /resume/
      icon: /images/resume.svg
    Projects: 
      link: /categories/projects/
      icon: /images/projects.svg
    Notes: 
      link: /categories/notes/
      icon: /images/notes.svg
    Fun: 
      link: /categories/fun/
      icon: /images/fun.svg

# 只要名字匹配图标，你可以添加更多的社交链接
# 提示：社交链接可以是一个QR码链接
  social:
    medium: 
    skype: 
    slack: 
    steam: 
    tumblr: 
    whatsapp: 
    youtube: 
    snapchat: 
    instagram: 
    qq: 
    google-plus: 
    twitter: 
    facebook: 
    weibo: 
    weixin: 
    github: https://your/github/profile/link
    linkedin: https://your/linkedin/profile/link
```

1.一套默认图标，写好使用路径，就像`/images/icon_name.svg`:

- [archive](https://qutang.github.io/images/archive.svg)
- [fun](https://qutang.github.io/images/fun.svg)
- [home](https://qutang.github.io/images/home.svg)
- [notes](https://qutang.github.io/images/notes.svg)
- [projects](https://qutang.github.io/images/projects.svg)
- [resume](https://qutang.github.io/images/resume.svg)
- [search](https://qutang.github.io/images/search.svg)
- [uncategorized](https://qutang.github.io/images/uncategorized.svg)

2.添加`search`搜索页面

​	1.创建一个新的命名为'`search`'页面

​	2.在`search`页面前面，使用`search`布局

```yaml
---
layout: search
---
```

3.使用hexo的js插件[hexo-prism-plugin](https://github.com/ele828/hexo-prism-plugin)使代码高亮

4.自定义post图标

除了默认分类图标以外，当你想要将自定义的图标显示在主页，把`icon: path/to/your/icon`写在页面前面。 

## 投稿

发送功能请求和bugs到[这里](https://github.com/qutang/hexo-theme-cutie/issues)，或者发给我pull请求。 

## 鸣谢

1.[www.linpx.com](http://www.linpx.com/)

2.[Flaticon](http://www.flaticon.com/)

3.[InstantClick](http://instantclick.io/)



--END--

---



PS：我只是想通过翻译官方文档，来练习英语，锻炼耐心，最重要的是让自己完全读懂官方文档的内容方便使用，如翻译有误，欢迎指出。