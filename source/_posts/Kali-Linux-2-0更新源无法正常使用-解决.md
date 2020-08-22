---
title: Kali Linux 2.0更新源无法正常使用(解决)
date: 2017-08-10 10:06:02
tags: Kali 2.0
categories: notes
icon: /images/notes.svg
---

**Kali2.0**的更新源，千篇一律，还没有没有实际效果，这里是我的解决方案。

更改kali的更新源：
打开终端输入：(如果你不会用**vim**的话)

```
leafpad /etc/apt/sources.list
```

删除里面的内容，换为国内更新源：
````
#中科大kali源
deb http://mirrors.ustc.edu.cn/kali kali-rolling main non-free contrib
#阿里云kali源
deb http://mirrors.aliyun.com/kali kali-rolling main non-free contrib
#清华大学更新源 
deb http://mirrors.tuna.tsinghua.edu.cn/kali kali-rolling main contrib non-free
````

![](http://upload-images.jianshu.io/upload_images/7101374-76494ffe3d80b5e5.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
然后在终端里分别输入
````
apt-get update
````
````
apt-get upgrade
````
````
apt-get dist-upgrade
````
重复输入,直到没有更新为止。
**update** 是同步 /etc/apt/sources.list 和 /etc/apt/sources.list.d 中列出的源的索引，这样才能获取到最新的软件包。
**upgrade** 是升级已安装的所有软件包，升级之后的版本就是本地索引里的，因此，在执行 upgrade 之前一定要执行 update, 这样才能是最新的。
由于包与包之间存在各种依赖关系。upgrade只是简单的更新包，不管这些依赖，它不和添加包，或是删除包。而dist-upgrade可以根据依赖关系的变化，添加包，删除包。

最后输入
````
apt-get clean
````
这行是将已安装的软件包的安装包也删除掉，当然多数情况下这些包没什么用了，可以腾出一些空间。
然后重启
````
reboot
````
好了，结束。
参考：
[Kali Linux2.0更新源问题](http://blog.csdn.net/coding_or_dead/article/details/53448059)