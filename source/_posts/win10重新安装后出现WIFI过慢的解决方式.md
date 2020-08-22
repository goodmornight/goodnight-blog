---
title: win10重新安装后出现WIFI过慢的解决方式
date: 2017-08-09 10:42:39
tags: Win10
categories: notes
icon: /images/notes.svg
---

自从我卸360装卡巴斯基后，我的电脑全部崩盘，开不了机，进不去BIOS界面，最后确认为硬盘坏了。
昨天把硬盘换上以后，就开始重装我以前的win10系统64位1703，装好后，又继续用“驱动人生”把我电脑内部的驱动装上。第一次驱动用“鲁大师”没装好，不能重启，系统又崩盘，我又重装系统。第二次装驱动，驱动都安装成功。注意，驱动程序不求新，只求稳，尽量用原配，或者“驱动人生”推荐的，但无线网卡的驱动就算装上，我发现WIFI速度还是过慢，甚至慢到没网。起先我以为是我的网慢，后来连接上有线的后发现，网速正常。
所以后来我开始在网上查找资料，按照网上什么百度经验之类的，我的默认设置都是对的。所以网络上的教程对我都没有用。最后判断我的无线网卡没有坏，只是驱动不兼容问题，但我的现在的驱动程序是戴尔官网2016年版本，基本是很稳定，估计就差一些设置问题。

后来我自己解决了。步骤如下：*（PS：如果你正在使用的是无线网络，在802.bgn格式下经常会掉线，那么可以勾选这一选项，用来加强连接兼容性，以防止网络在使用中掉线。）*

![对这个无线标志单击右键，选择打开网络和共享中心](http://upload-images.jianshu.io/upload_images/7101374-300d783d3cf92655.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

**单击右键——“打开网络和共享中心”——“更改适配器设置”——找到“WLAN”双击左键——“无线属性”——“安全”——“高级设置”——打勾选择启用FIPS兼容**


![双击WLAN，选择无线属性](http://upload-images.jianshu.io/upload_images/7101374-0a582430c9c186e0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![安全选项，高级设置](http://upload-images.jianshu.io/upload_images/7101374-2b78d94242ab7650.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![设置为FIPS兼容](http://upload-images.jianshu.io/upload_images/7101374-3d5d87aea2742b5c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后就一切正常了！！开心O(∩_∩)O~~