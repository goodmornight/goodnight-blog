---
title: Kali Linux 2.0英文原版安装谷歌拼音输入法（解决）
date: 2017-08-10 16:51:39
tags: Kali 2.0
categories: notes
icon: /images/notes.svg
---

由于我装了**Kali Linux 2.0**英文原版，安装谷歌拼音输入法就与网络上教的内容有点出入。

上篇文章我们已经成功更新了kali的更新源。我们这就不加以赘诉了。
打开终端，分别输入

````
apt-get install fcitx
apt-get install fcitx-googlepinyin
````

安装好后，重启。

桌面左下角，有个键盘，单击右键，点击“configure”

![1](http://upload-images.jianshu.io/upload_images/7101374-63f6eb152c802d70.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

进去后，点左下角的+

取消选中 **Only Show Current Language**

![2](http://upload-images.jianshu.io/upload_images/7101374-49749534643c3481.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

在框内输入**google**,查找谷歌拼音

![3](http://upload-images.jianshu.io/upload_images/7101374-dcd42cc37634a11c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

最后，按快捷键 **Ctrl + 空格**，即可切出谷歌拼音输入法。