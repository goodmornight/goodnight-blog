---
title: SQLServer学习(5)为SQLServer创建登录账户
date: 2019-05-29 10:00:14
tags: SQL Server
categories: notes
icon: /images/notes.svg
---

之前我的一些有关于SQLServer学习的文章：

[SQL Server学习(1)介绍与安装](http://spyxxx.xyz/2019/04/10/SQLServer%E5%AD%A6%E4%B9%A0-1-%E4%BB%8B%E7%BB%8D%E4%B8%8E%E5%AE%89%E8%A3%85/)

[SQL Server学习(2)创建数据库和数据表](http://spyxxx.xyz/2019/04/23/SQLServer%E5%AD%A6%E4%B9%A0-2-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%E6%95%B0%E6%8D%AE%E8%A1%A8/)

[SQLServer学习(3)导入导出数据表](http://www.spyxxx.xyz/2019/05/06/SQLServer%E5%AD%A6%E4%B9%A0-3-%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E6%95%B0%E6%8D%AE%E8%A1%A8/)

[SQLServer学习(4)给数据库、表生成SQL脚本](http://www.spyxxx.xyz/2019/05/14/SQLServer%E5%AD%A6%E4%B9%A0-4-%E7%BB%99%E6%95%B0%E6%8D%AE%E5%BA%93%E3%80%81%E8%A1%A8%E7%94%9F%E6%88%90SQL%E8%84%9A%E6%9C%AC/)

我的配置：

- 操作系统：Window10 x64
- SQL Server 2016

这篇文章将介绍如何为SQLServer创建登录账户。

---

# 新建账户

1.先**以Windows身份验证登录账户**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_1.png?raw=true)

2.在对象资源管理器窗口中，如图，**安全性——登录名——单击右键“新建登录名”。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_2.png?raw=true)

窗口内设置如下，**“常规”选项卡中输入登录名，选择以“SQLServer身份验证”，输入并确认密码，取消选择“强制实施密码策略”，选择默认数据库。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_3.png?raw=true)

3.此时该用户还不能访问数据库里的对象，需要进行如下操作。

# 数据表添加数据库用户

1.依然还是以Windows身份验证登录的账户。

2.然后找到你想要**映射的数据库，比如我的数据库名字为“test2”——安全性——用户——单击右键——新建用户——如图添加用户名和登录名。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_4.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_5.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_6.png?raw=true)

3.如果是**新建用户则别关闭那个窗口，则就在“安全对象”选项卡里继续进行如下操作。**

如果是原来**已经存在的用户名，那么就在对应的用户名处单击右键，点击“属性”，直接就进入了“安全对象”选项卡里了，接下来也是如下操作。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_14.png?raw=true)

操作中，我选用了test2数据库里的test数据表。具体授予的权限，**需要根据实际决定。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_7.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_8.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_9.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_10.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_11.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_12.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_13.png?raw=true)

4.**设置完后，切换账户登录，查看**是否可以查看并编辑刚刚添加的表。

（1）点击如下按钮，可以**断开当前连接**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_15.png?raw=true)

（2）点击如下按钮，就可以**连接对象管理资源器**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_16.png?raw=true)

如图成功。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-5_17.png?raw=true)

---

参考链接：

[百度经验-如何为SQL server 创建登录账户](https://jingyan.baidu.com/article/f71d6037b2233e1ab641d19e.html)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。