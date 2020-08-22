---
title: SQLServer学习(4)给数据库、表生成SQL脚本
date: 2019-05-14 14:13:16
tags: SQL Server
categories: notes
icon: /images/notes.svg
---

之前我的一些有关于SQLServer学习的文章：

[SQL Server学习(1)介绍与安装](http://spyxxx.xyz/2019/04/10/SQLServer%E5%AD%A6%E4%B9%A0-1-%E4%BB%8B%E7%BB%8D%E4%B8%8E%E5%AE%89%E8%A3%85/)

[SQL Server学习(2)创建数据库和数据表](http://spyxxx.xyz/2019/04/23/SQLServer%E5%AD%A6%E4%B9%A0-2-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%E6%95%B0%E6%8D%AE%E8%A1%A8/)

[SQLServer学习(3)导入导出数据表](http://www.spyxxx.xyz/2019/05/06/SQLServer%E5%AD%A6%E4%B9%A0-3-%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E6%95%B0%E6%8D%AE%E8%A1%A8/)

我的配置：

- 操作系统：Window10 x64
- SQL Server 2016

这篇文章将以数据库里的表为例，介绍如何将数据库里的表生成SQL脚本并导入。

---

# 生成SQL脚本

如图，打开我们需要生成SQL脚本的表，查看里面的内容，即“test”数据库里的“info”表。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_1.png?raw=true)

然后**在对象资源管理器里，对“test”数据库单击右键——选择“任务”——生成脚本**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_2.png?raw=true)

就会**打开如下“生成和发布脚本”对话框——选择next** 。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_3.png?raw=true)

接下来的内容需要**根据实际情况选择**，由于只需要将“info”表生成SQL脚本，所以**选择“选择具体的数据对象”——表——勾选“dbo.info”——next** 。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_4.png?raw=true)

**保存位置与格式也需要根据实际情况来选择**，如图。然后**检查自己的操作没有问题后，再选择“next”，就开始执行了，执行完毕后，点击“finish”**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_5.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_6.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_7.png?raw=true)

# 通过SQL脚本导入表

为了达到效果，先将原来已有的“info”数据表**删除**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_8.png?raw=true)

然后菜单栏里的**文件——打开——文件，选择刚刚保存的sql脚本文件——点击执行**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_9.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_10.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_11.png?raw=true)

执行完毕，**刷新一下“test”数据库，即可看到刚刚的“info”数据表**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_12.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_13.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3-2_14.png?raw=true)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。