---
title: MySQL学习(1)安装
date: 2019-04-13 16:43:30
tags: MySQL
categories: notes
icon: /images/notes.svg
---

因为学习需要，刚安装完SQL Server，现在又跑来安装MySQL。我记得我很久以前安装，貌似是5.7版本的，可能之前重装过系统，被我弄没了，重新装一个吧。

我的配置：

- 操作系统：Window10 x64
- MySQL8.0.15（解压版）

以下内容均为我学习MySQL的笔记，可能有摘抄的（我会标明出处），可能也有自己写的。

---

# 下载

[MySQL Community下载](https://dev.mysql.com/downloads/)

如图选择：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_1.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_2.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_3.png?raw=true)

正常网速下载会比较慢，还容易出错，因此我开了VPN。

# 配置

## 添加环境变量（非必须）

下载好后，你可以把它解压到你想要放的位置。然后该mysql的bin的路径到环境变量（但这个是非必须的，只是方便你未来用cmd打开mysql）。

## 编写my.ini

然后返回到mysql的根目录，新建一个txt文档，将其改名为my.ini（后缀自然也是改了）。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_4.png?raw=true)

再用notepad++或记事本打开my.ini，往里面添加如下代码：

```sql
[mysqld]

# 这是你的mysql的路径
basedir=D:\mysql-8.0.15-winx64

# 确定data的位置，一会配置 data 这个文件夹 
datadir=D:\mysql-8.0.15-winx64\data

#默认端口，无需更改
port = 3306
```

## 配置data文件

进入D:\mysql-8.0.15-winx64\bin（根据你的实际情况而定），按住**shift+单击右键**，选择“**在此处打开PowerShell窗口**”（你也可以正常的以管理员身份打开cmd.exe，进入该目录D:\mysql-8.0.15-winx64\bin），

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_5.png?raw=true)

然后输入

```powershell
# 配置 mysql 服务，默认用户名为：root ，默认密码为空
#如果你是用cmd打开的话
mysqld --initialize-insecure
#如果你是用PowerShell打开的话，前面加上.\
.\mysqld --initialize-insecure
```

稍等片刻就好。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_6.png?raw=true)

## 最后，在服务中加入MySQL服务

输入：

```powershell
#如果你是用cmd打开的话
mysqld --install MySQL
#如果你是用PowerShell打开的话，前面加上.\
.\mysqld --install MySQL
```

成功如图：（图一是在cmd成功图，图二是在powershell成功图）

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_8.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_7.png?raw=true)

# 启动MySQL

如果你已经把D:\mysql-8.0.15-winx64\bin加入环境变量里了，你可以**以管理员身份打开cmd**，输入

```shell
net start mysql
```

启动成功，如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_9.png?raw=true)

如果你出现如下**报错**：

**发生系统错误5**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_10.png?raw=true)

原因就是**你没有以管理员身份打开cmd**。

然后登录，输入

```
mysql -u root -p
```

root用户默认密码为空。

如图，**成功**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_1_11.png?raw=true)

# if 你想装两个版本的MySQL

没错，就像我一样，突然发现学长要求的mysql的版本是5.7，我只好把我之前下载好的mysql重新配置一下。

还是同上把你下载好的zip文件，**解压到你想要放的位置**。因为我以前就配过，但换了一个地方，也忘了我之前的数据库密码，所以在调整一下。

**删掉之前的data文件** ，然后，**更改my.ini里的内容**，

- **port //port端口因为之前，3306用过了，所以我改成了3307**

- **basedir //根据实际**

- **datadir //根据实际**

然后用**管理员身份**打开cmd，**进入mysql5.7的bin文件夹**，

**安装mysql服务**，为了防止重复，**指定该mysql5.7服务名为mysql2**，

服务安装成功后，就开始**初始化数据库**。

```shell
mysqld --initialize
```

成功后不会有任何提示，但**根目录会增加新的data文件夹**。

然后，**打开注册表**，找到**HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\mysql2,修改ImagePath参数，更正mysql2服务相关路径**。（至于打开注册表的方式有很多，**打开计算机，进入系统所在盘，进入Windows文件夹，进入System32文件夹，找到regedt.exe程序**，双击打开，就是其中的一个方法），如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_2_1.png?raw=true)

接下来启动mysql服务器：

```shell
net start mysql2
```

**mysql2服务启动后，去data/xxx.err文件中找到临时密码**，进行登录。如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/mysql/mysql_figure_2_2.png?raw=true)

**使用临时密码进行登录(tip:-P是端口，-p是密码,建议-p后直接敲回车，这样输入的密码就不会显示出来)**

```
mysql -P3307 -uroot -p
```

修改密码,双引号里的密码你可以为空，也可以自定。

```mysql
mysql> set password for root@localhost=password('');
```

**使用quit或exit或\q退出，再用新密码登录mysql客户端。**

顺便加一句，要退出mysql服务器，就写

``` shell
net stop mysql2
```

参考链接：

[windows上同时安装两个版本的mysql数据库](https://blog.csdn.net/wudinaniya/article/details/82455431#commentBox)

——更新于2019.4.19

---

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。