---
title: SQLServer学习(2)创建数据库和数据表
date: 2019-04-23 22:51:18
tags: SQL Server
categories: notes
icon: /images/notes.svg
---

之前已经大概的介绍过SQLServer，也详细写过了该如何安装它。

[传送门：SQL Server学习(1)介绍与安装](http://spyxxx.xyz/2019/04/10/SQLServer%E5%AD%A6%E4%B9%A0-1-%E4%BB%8B%E7%BB%8D%E4%B8%8E%E5%AE%89%E8%A3%85/)

接下来这篇文章，将分别从**图形化界面**和**SQL语句**两个途径，来**建数据库和数据表**。

我的配置：

- 操作系统：Window10 x64

- SQL Server 2016

---

# 图形化界面

## 建数据库

首先，打开我们的SQLServer，即一个叫 “**Microsoft SQL Server Management Studio**”的软件。然后如图，**点击连接**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_1.png?raw=true)

在左边的**对象资源管理器**的**数据库**那**单击右键**，选择**新建数据库**，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_2.png?raw=true)

然后输入一个你想给取的**数据库名字**，数据库的其他属性根据实际情况修改，这里我保持默认，然后单击“**确定**”就新建一个数据库了。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_3.png?raw=true)

现在我新建了一个名为“test”的数据库，其他属性保持默认。

## 建数据表

依旧是在**对象资源管理器**里，找到你**新建的数据库**“**test**”**点开——找到**“**表**”——**单击右键——新建——表**。如图。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_4.png?raw=true)

然后就建成了表。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_5.png?raw=true)



接下来设置表里的列，以及列里存的数据类型等等。如图，正常一行一行的填入信息，会自动添加空白行。如图，过程很简单。

接下来给他们**设置主键**，一般来说，一个表只有一个主键。你只要选中要**做主键的列——单击右键——选择**“**设为主键**”即可。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_6.png?raw=true)

如果**一个表中建立两个列及以上的列为主键，可能会发生错误**。但如果一定要这么做的话，就**按住Ctrl选中（Ctrl+最前面三角型那部分）要做主键的列——再单击右键——选择**“**设为主键**”即可。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_7.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_8.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_9.png?raw=true)

# SQL语句操作

## 建库(如果照着上面一步步来的，这一步就可以忽略了)

如图，点击上面的**新建查询**，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_10.png?raw=true)

然后在里面输入T-SQL语句，即下面代码：

```sql
create database test;
use test;
```

输入完T-SQL后，点击**执行**。

## 建数据表

如图，点击上面的**新建查询**，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_10.png?raw=true)

然后在里面输入T-SQL语句，即下面代码：

```sql
Create table SE
(
	ID int not null,
	MONTH int not null,
	LON float not null,
	LAT float not null,
	WAVE float not null,
	E float not null,
	primary key(MONTH, LON, LAT, WAVE)
)
```

这个代码的内容就是，新建一个名字叫“SE”的表，然后定义表当中的列，分别有ID、MONTH、LON、LAT、WAVE、E，然后再将MONTH、LON、LAT、WAVE设为主键。

输入完T-SQL后，点击**执行或按F5**，执行成功后，下面的消息框会有提示。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_11.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_12.png?raw=true)

然后**在对象资源管理器内点击刷新按钮**，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_13.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_14.png?raw=true)

我们就**可以看到新建的表和里面的列**了，如果想要更直观的看到自己的新建的内容，可以在**新建的表上单击右键——设计**，即可通过图形界面设计表。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_15.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-2_16.png?raw=true)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。