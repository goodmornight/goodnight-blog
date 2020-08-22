---
title: 修改网页字体「@font-face」
date: 2020-06-26 20:24:56
tags:
	- CSS
	- 前端
categories: notes
photos:
	- https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/203221-105075.png
---

![image-20200626203219345](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/203221-105075.png)

# 前言

一直以来对衬线字体都有一种迷之喜爱。

这也就是为什么我这次博客主题选用了这个主题[paper](https://randomyang.top/)，但当我使用这个主题的时候发现，这个默认的字体就算他写了“Noto Serif SC”，在`font-family`里可是显示还是衬线字体。令我很苦恼，只好手动修改了。

![image-20200626204522250](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/204540-276024.png)

# 什么是@font-face

首先，CSS3中出现了`var`变量，而**`@font-face`本质上就是一个变量，一个定义字体或字体集的变量**。<sup>[[1]](https://www.zhangxinxu.com/wordpress/2017/03/css3-font-face-src-local/)</sup> 它允许开发者为其网页指定在线字体。 通过自备字体的方式，`@font-face` 可以消除对用户电脑字体的依赖。<sup>[[2]](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)</sup>

## 语法<sup>[[2]](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)</sup>

```css
@font-face {
  [ font-family: <family-name>; ] ||
  [ src: <src>; ] ||
  [ unicode-range: <unicode-range>; ] ||
  [ font-variant: <font-variant>; ] ||
  [ font-feature-settings: <font-feature-settings>; ] ||
  [ font-variation-settings: <font-variation-settings>; ] ||
  [ font-stretch: <font-stretch>; ] ||
  [ font-weight: <font-weight>; ] ||
  [ font-style: <font-style>; ]
}
```

### sample<sup>[[2]](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)</sup>

```css
@font-face {
  font-family: MyHelvetica;
  src: local("Helvetica Neue Bold"),
  local("HelveticaNeue-Bold"),
  url(MgOpenModernaBold.ttf);
  font-weight: bold;
}
@font-face {
  font-family: "Bitstream Vera Serif Bold";
  src: url("http://developer.mozilla.org/@api/deki/files/2934/=VeraSeBd.ttf");
}
```

以下是**常用参数**介绍。

### 1.font-family

**自定义font名字**，可用引号，也可不用引号，但如果是以符号命名，建议加引号。

### 2.src

**调用字体文件位置**，可是本地文件，也可是线上地址。

### 3.font-style

**设置字体类型**，可选`normal`,`italic`,`oblique`。<sup>[[3]](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)</sup>

### 4.font-weight<sup>[[4]](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/font-weight)</sup>

**字重**，可用数字也可用字符。

如果，你下载的字体有一套的话，可以相同的`font-family`名称，设置不同的`font-weight`。

| Value | Common weight name        |
| ----- | ------------------------- |
| 100   | Thin (Hairline)           |
| 200   | Extra Light (Ultra Light) |
| 300   | Light                     |
| 400   | Normal                    |
| 500   | Medium                    |
| 600   | Semi Bold (Demi Bold)     |
| 700   | Bold                      |
| 800   | Extra Bold (Ultra Bold)   |
| 900   | Black (Heavy)             |

### 5.unicode-range<sup>[[1]](https://www.zhangxinxu.com/wordpress/2017/03/css3-font-face-src-local/)</sup> 

**可以让特定的字符或者字符片段使用特定的字体。**

```css
@font-face {
  font-family: BASE;
  src: local("Microsoft Yahei");
}
@font-face {
  font-family: quote;
  src: local('SimSun');    
  unicode-range: U+201c, U+201d;
}
.font {
    font-family: quote, BASE;
}
```

如图，微软雅黑的引号：

![微软雅黑的引号](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/214843-416869.png)

修改后：

![修改后效果图](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/214918-932091.png)

# 实际操作

(此处是我的实际操作，每个人情况不一样，可跳过不看)

## 1.下载字体

我将下载好的Noto Serif SC字体挑选后，在`./themes/paper/source`文件夹下新建了一个font文件夹，放在里面了。

## 2.配置

我在`./themes/paper`文件夹里，找到了对应设置字体变量的位置`./themes/paper/source/css/var.styl`，可以看出原主题作者是用**stylus**写的。因为我之前没写过stylus，找了对应stylus文档<sup>[[5]](https://stylus-lang.com/docs/font-face.html)</sup> ，模仿写了一下。

```stylus
@font-face
	font-family Noto Serif SC
	font-style normal
	src url(../font/NotoSerifSC-Black.otf)
	font-weight bold
@font-face
	font-family Noto Serif SC
	font-style normal
	src url(../font/NotoSerifSC-SemiBold.otf)
	font-weight normal
$font-family = "Noto Serif SC","SF Pro Text","SF Pro Icons","Helvetica Neue","Helvetica","Arial",sans-serif
```

**Happy ending.**

![修改后页面结果图](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202006/26/220213-97641.png)

# 参考链接

[1] [真正了解CSS3背景下的@font face规则](https://www.zhangxinxu.com/wordpress/2017/03/css3-font-face-src-local/)

[2] [MDN--@font-face](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face)

[3] [MDN--font-style](https://developer.mozilla.org/en-US/docs/Web/CSS/font-style)

[4] [MDN--font-weight](https://developer.mozilla.org/zh-CN/docs/Web/CSS/@font-face/font-weight)

[5] [stylus--@font-face](https://stylus-lang.com/docs/font-face.html)

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。