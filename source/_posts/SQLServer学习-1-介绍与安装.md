---
title: SQL Server学习(1)介绍与安装
date: 2019-04-10 09:53:02
tags: SQL Server
categories: notes
icon: /images/notes.svg
---

我的配置：

- 操作系统：Window10 x64

- SQL Server 2017

以下内容均为我学习SQL Server的笔记，可能有摘抄的（我会标明出处），可能也有自己写的。

最后安装的是2016版本的，未来有机会再补上2017版本的过程。  ——2019.4.12

---
# 介绍

以下介绍出自百度百科：

> SQL是英文Structured Query Language的缩写，意思为[结构化查询语言](https://baike.baidu.com/item/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F)。
>
> SQL语言的主要功能就是同各种数据库建立联系，进行沟通。
>
> 按照ANSI([美国国家标准协会](https://baike.baidu.com/item/%E7%BE%8E%E5%9B%BD%E5%9B%BD%E5%AE%B6%E6%A0%87%E5%87%86%E5%AD%A6%E4%BC%9A/1351184?fromtitle=%E7%BE%8E%E5%9B%BD%E5%9B%BD%E5%AE%B6%E6%A0%87%E5%87%86%E5%8D%8F%E4%BC%9A&fromid=5709537))的规定，SQL被作为[关系型数据库管理系统](https://baike.baidu.com/item/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F/11032386?fromtitle=%E5%85%B3%E7%B3%BB%E5%9E%8B%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F&fromid=696511)的标准语言。
>
> SQL Server是由Microsoft开发和推广的[关系数据库管理系统](https://baike.baidu.com/item/%E5%85%B3%E7%B3%BB%E6%95%B0%E6%8D%AE%E5%BA%93%E7%AE%A1%E7%90%86%E7%B3%BB%E7%BB%9F/11032386)（RDBMS）

所以说，SQL Server是一个关系数据库管理系统。

## SQL Server可以用来做什么？

- 创建和维护数据库

- 创建和维护表

- 创建和维护其他数据库对象，如存储过程，视图等

- 创建和维护和调度数据备份

- 复制（例如，创建数据库的副本）

- 创建和维护用户，角色等

- 优化任务

  (*出自W3CSchool*)

## SQL Server2017的版本介绍

- **SQL Server 2017 on-premises**
  借助可扩展的混合数据平台，针对要求严苛的工作负载构建智能任务关键型应用程序。为期 180 天的 Windows SQL Server 2017 免费试用。

- **SQL Server in the cloud**

  充分利用 SQL 数据库内置的高可用性、安全性和智能以及熟悉的 Azure SQL 引擎，而无需再进行复杂的基础架构管理。在 Azure 中免费使用 SQL 数据库。

- **SQL Server 2017 Developer**

  SQL Server 2017 Developer 是一个全功能免费版本，许可在非生产环境下用作开发和测试数据库。

- **SQL Server 2017 Express**

  SQL Server 2017 Express 是 SQL Server 的一个免费版本，非常适合用于桌面、Web 和小型服务器应用程序的开发和生产。

## 适用平台

- **Windows**

- **Linux**

- **Docker**

# 安装

 ## 下载

**[官网下载SQL Server](https://www.microsoft.com/en-us/sql-server/sql-server-downloads#)**

根据上面的版本介绍（官网下载页面也有介绍），选择适合自己的版本，下载。（下载过程较为简单，所以此处略过）

因为我是Windows，根据我目前的需求，我选择下载SQL Server 2017 Developer。

## SQL Server2017安装

安装如下，我选择了**自定义安装**，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_1.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_2.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_3.png?raw=true)

当然你也可以选别的，然后**选择安装路径**，开始**安装**，

结果一开始就报错，我们看看报错原因“**无法连接到远程服务器**”，有可能是国外的服务器不好连上，

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_4.png?raw=true)

那我只好开启我的**fan-qiang软件**，然后就**正常下载**了。我的网速比较慢，估计要下载好一会了。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_5.png?raw=true)



————2019.4.12时间分割线————

我没想到下载失败了。

如果未来，把2017的给下下来了，如果有时间，我会再来补上。

---

————2019.4.12时间分割线————

## SQL Server2016安装

emm。。。没想到最后下载失败了。从星期二下到星期五，每天都开机挂着下。没办法，只好跑去西电睿思上下载别人发的种子，虽然没有最新的SQL Server2017的种子，但2016的也是够用了。因为时间比较赶安装过程我也忘了截图了，只好把我当时用的**参考链接**记下来：

[**SQL server 2016 安装步骤**](https://www.cnblogs.com/ksguai/p/5869558.html)

[**SQLServer 2016安装时的错误：Polybase要求安装Oracle JRE 7更新51或更高版本**](https://zhidao.baidu.com/question/1758182120612261868.html)

因为我中间报了有关于Polybase的错误，从他的报错来看是要装JRE 7，然而这个版本不存在了，我为了省事就直接不装需要JRE 7的部件，具体不装什么直接看上面的链接。

## SQL Server2016安装成功

安装完后，就如截图那样，好多个，作为一个初学者，我都不是很清楚哪个是哪个。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_6.png?raw=true)

但你只要从当中找一个名字叫“**Microsoft SQL Server Management Studio**”，如图就是这样，你只要登录上去就好了。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-1_7.png?raw=true)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。