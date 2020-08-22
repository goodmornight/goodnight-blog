---
title: 重装win10系统出现故障解决
date: 2017-08-09 11:56:15
tags: Win10
categories: notes
icon: /images/notes.svg
---

一般出厂的正版电脑从正规的地方出售，安装的**windows**都是正版的。所以就算你被迫要重装系统，你只要还装的是原来版本的系统，登录原来的微软账号就不需要注册码，并且装好后在**“设置”——“更新和安全”——“激活”**可以看到，如图：


![windows设置](http://upload-images.jianshu.io/upload_images/7101374-4ad3b282b997eeb9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)



![激活](http://upload-images.jianshu.io/upload_images/7101374-ba28ff4df3163caf.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

如果你是维修店里的维修人员便宜价格给你装的，你就要注意了！！我昨天就是入的这个坑，折腾了半天。（PS：给我装了一个老的**win10 1507**，这个版本微软已经不提供安全保护了，自己升级到1703还不会成功，还花了我50RMB）我把我昨天用手机拍的照片做了记录。


![盗版激活](http://upload-images.jianshu.io/upload_images/7101374-5b6ce879b5b5aaad.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你也可以**win+R**,打开运行输入此命令自己看看。

![运行输入](http://upload-images.jianshu.io/upload_images/7101374-f13a996d99038e3c.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

正版的会显示永久激活！！

![正版的是永久激活](http://upload-images.jianshu.io/upload_images/7101374-7c4f84e8f6823405.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而我昨天盗版的是这样的。

![盗版的会过期](http://upload-images.jianshu.io/upload_images/7101374-17f272eae51e1c8f.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

然后你再输入这个：

![运行](http://upload-images.jianshu.io/upload_images/7101374-b251e311e1682ee5.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

正常显示这个：

![正版截图](http://upload-images.jianshu.io/upload_images/7101374-dd0c2680e876fed0.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

而我昨天显示的是这样的：

![盗版拍照](http://upload-images.jianshu.io/upload_images/7101374-28040b693cb13f9b.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

由此可以看出，他的所谓的注册码是批量激活的，我的系统只有180天的有效期，180天过后，这个系统就失效了！！所以我还不如在他的保修期内赶快换个系统。*（PS：装系统那个人说我这个系统有一个星期的保修期）*

想看看自己以前用的win10是不是正版的，可以去微软官网登录自己以前的账号查看，
[Microsoft – Home Page oficial](https://www.microsoft.com/zh-cn/)

![Microsoft – Home Page oficial](http://upload-images.jianshu.io/upload_images/7101374-3779797d85e2c50c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


![登录后点设备](http://upload-images.jianshu.io/upload_images/7101374-610b9c85e12de738.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

可以看到这个，说明你以前装的win10是正版，你在你原来的电脑上再次安装不需要注册码。

![查看你是否有正版windows](http://upload-images.jianshu.io/upload_images/7101374-c48ad1675f014bbc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
OK，我们这里不讲，如何制作U盘启动盘（用微软的**MediaCreateTool**，插入U盘傻瓜制作），也不讲如何安装**win10**系统（百度搜索到处都有，也是傻瓜式安装）。我只记录我遇到问题，我安装失败的情况。

我安装好后，重启，结果出现这样的情况：

![这个界面是未识别硬盘](http://upload-images.jianshu.io/upload_images/7101374-ea9ccd04924f2f23.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

那么重新回到U盘启动界面

![重新回到U盘安装界面](http://upload-images.jianshu.io/upload_images/7101374-8ad3813c6b699173.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

按下**shift+F10**,打开了命令行

![打开命令行](http://upload-images.jianshu.io/upload_images/7101374-d3e0b376c2b9552e.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们用了简单粗暴的做法，
在命令行中分别输入：

```
diskpart

list disk

sel disk 0

clean
```


注意,我的**disk 0**原来是系统盘，我只是把原来安装好的系统盘给清空，重新来。

重新用U盘安装系统，就成功了。

注意，安装完系统记得下一个驱动软件，给硬件安装驱动程序，这样你的网卡呀，无线网卡呀，键盘呀，才能正常使用。

以上内容是胡大佬神助攻，我才得以成功安装，不然的话，又要去找那个维修的了。作为一个硬件、系统的小白在此特别感谢胡大佬孜孜不倦的教导。
我的这篇文章就是想把我出现过的问题记下来，以免忘记，同时也希望能帮到同样遇到这个问题的人。