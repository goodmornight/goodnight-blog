---
title: SQLServer学习(3)导入导出数据表
date: 2019-05-06 20:36:07
tags: SQL Server
categories: notes
icon: /images/notes.svg
---

之前我的一些有关于SQLServer学习的文章：

[SQL Server学习(1)介绍与安装](http://spyxxx.xyz/2019/04/10/SQLServer%E5%AD%A6%E4%B9%A0-1-%E4%BB%8B%E7%BB%8D%E4%B8%8E%E5%AE%89%E8%A3%85/)

[SQL Server学习(2)创建数据库和数据表](http://spyxxx.xyz/2019/04/23/SQLServer%E5%AD%A6%E4%B9%A0-2-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%E6%95%B0%E6%8D%AE%E8%A1%A8/)

我的配置：

- 操作系统：Window10 x64

- SQL Server 2016


这篇文章将介绍如何Excel表格的数据导入数据表和数据表中的数据如何导出到Excel表格里。

---
# 导入数据表

SQL Server将外部数据导入数据表中有如下几种操作方式：

- **手动输入**:直接将数据输入到数据库即可；
- **使用复制/粘贴功能**:从其他来源的数据复制，然后粘贴到数据库中的表；
- **直接导入**:可以使用导入和导出向导从另一个源导入数据；
- **使用SQL脚本**:可以运行一个包含所有数据插入的SQL脚本；
- **应用/网站**: 通过应用程序或网站更新数据库。

**下面介绍直接导入方式和使用SQL脚本方式：**

## 直接导入：Excel表格导入到数据表

准备工作：

- 一个存有数据的Excel表格

- 建好的数据库

如图，Excel表格里的内容。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_1.png?raw=true)


在“对象资源管理器”部分，对需要导入Excel表格的数据库**单击右键——任务——导入数据**，如图。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_2.png?raw=true)

按照以下图片一步步走。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_3.png?raw=true)

第二步，**选择数据源界面。数据源选择“Microsoft Excel”，Excel文件路径选择要导入的Excel文件，Excel版本选择本机有的版本**，我选了“Microsoft Excel 97-2003”，因为我的Excel文件中首行存在列名称，所以勾选“首行包含列名称”，根据个人情况而定。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_4.png?raw=true)

第三步，**选择目标界面。目标选择“SQL Server Native Client”，服务器名称，身份验证根据个人情况而定，数据库的选择就是刚刚对其单击右键的数据库，也可以修改成别的数据库**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_5.png?raw=true)

第四步，**选择“复制一个或多个表或视图的数据”**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_6.png?raw=true)

第五步，**此处显示了SQL Server将会建一个新的表名为“Sheet1$”，继续下一步**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_7.png?raw=true)

第六步，**勾选“立即运行”，继续下一步**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_8.png?raw=true)

第七步，**点击完成，然后就开始导入操作**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_9.png?raw=true)

第八步，**操作信息会显示出来，操作成功后，关闭**即可。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_10.png?raw=true)

最后，回去**刷新**一下“对象资源管理器”，就会看见一个新建好的表“Sheet1$”，**单击右键——选择“编辑前200行”**，就可以在旁边看到刚刚导入进去的数据了。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_11.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_12.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_13.png?raw=true)

## 使用SQL脚本导入数据表

SQL脚本可以通过INSERT INTO语句将数据插入数据表，这里介绍的是**如何用SQL把数据表导入到txt文件**。

准备工作：

- 存有数据的文本文件
- 一个数据表

首先新建一个SQL查询脚本，以windows身份验证。如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_27.png?raw=true)

在脚本中写如下语句：

```sql
-- 启用xp_cmdshell
exec sp_configure 'xp_cmdshell',1
-- 重新配置 
reconfigure
EXEC master..xp_cmdshell 'BCP  test2.dbo.test in D:\sqlServerExp\test.txt -c -t -T'
```

引用网上介绍：

> **bcp** 实用工具可以在 Microsoft SQL Server 实例和用户指定格式的数据文件间大容量复制数据。  使用 **bcp** 实用工具可以将大量新行导入 SQL Server 表，或将表数据导出到数据文件。 除非与 **queryout** 选项一起使用，否则使用该实用工具不需要了解 Transact-SQL 知识。 若要将数据导入表中，必须使用为该表创建的格式文件，或者必须了解表的结构以及对于该表中的列有效的数据类型。

> BCP可以执行的4种操作:
>
> **(1) 导入** 这个动作使用in命令完成，后面跟需要导入的文件名。 
>
> **(2) 导出** 这个动作使用out命令完成，后面跟需要导出的文件名。 
>
> **(3) 使用SQL语句导出** 这个动作使用queryout命令完成，它跟out类似，只是数据源不是表或视图名，而是SQL语句。 
>
> **(4) 导出格式文件** 这个动作使用format命令完成，后而跟格式文件名。

常用参数：

> **-f format_file**
> format_file表示格式文件名。这个选项依赖于上述的动作，如果使用的是in或out，format_file表示已经存在的格式文件，如果使用的是format则表示是要生成的格式文件。
>
> **-x** 
> 这个选项要和-f format_file配合使用，以便生成xml格式的格式文件。
>
> **-F first_row**
> 指定从被导出表的哪一行导出，或从被导入文件的哪一行导入。
>
> **-L last_row**
> 指定被导出表要导到哪一行结束，或从被导入文件导数据时，导到哪一行结束。
>
> **-c**
> 使用char类型做为存储类型，没有前缀且以"\t"做为字段分割符，以"\n"做为行分割符。
>
> **-w**
> 和-c类似，只是当使用Unicode字符集拷贝数据时使用，且以nchar做为存储类型。
>
> **-t field_term**
> 指定字符分割符，默认是"\t"。
>
> **-r row_term**
> 指定行分割符，默认是"\n"。 
>
> **-S server_name[ \instance_name]**
> 指定要连接的SQL Server服务器的实例，如果未指定此选项，BCP连接本机的SQL Server默认实例。如果要连接某台机器上的默认实例，只需要指定机器名即可。 
>
> **-U login_id**
> 指定连接SQL Sever的用户名。
>
> **-P password**
> 指定连接SQL Server的用户名密码。
>
> **-T**
> 指定BCP使用信任连接登录SQL Server。如果未指定-T，必须指定-U和-P。
>
> **-k**
> 指定空列使用null值插入，而不是这列的默认值。



**其中“test2.dbo.test”是省略了服务器名的对象限定名，即“数据库名.数据库架构名.对象名”**，根据实际情况更改即可。

**“D:\sqlServerExp\test.txt”为需要导入的txt文本文件**。

如图，执行成功。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_29.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_30.png?raw=true)



# 导出数据表

## 数据表导出到Excel表格

准备工作：

- 存有数据的数据表

- 空的Excel表格（也不一定要空的）


基本操作和上文的“直接导入：Excel表格导入到数据表”内容相似。

在“对象资源管理器”部分，对需要导入Excel表格的数据库**单击右键——任务——导出数据**，如图。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_14.png?raw=true)

第二步，**选择数据源界面。数据源选择“SQL Server Native Client”，服务器名称，身份验证根据个人情况而定，数据库的选择就是刚刚对其单击右键的数据库，也可以修改成别的数据库**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_15.png?raw=true)

第三步，**选择目标界面。目标选择“Microsoft Excel”，Excel文件路径选择要导入的Excel文件，Excel版本选择本机有的版本**，我选了“Microsoft Excel 97-2003”，因为我的Excel文件中首行存在列名称，所以勾选“首行包含列名称”，根据个人情况而定。由图可见，我选择的“test.xls”是一个空的表。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_16.png?raw=true)

第四步，**选择“复制一个或多个表或视图的数据”**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_17.png?raw=true)

第五步，**此处显示了SQL Server将会建一个新的表名为“test”，继续下一步**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_18.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_19.png?raw=true)

第六步，**勾选“立即运行”，继续下一步**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_20.png?raw=true)

第七步，**点击完成，然后就开始导出操作**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_21.png?raw=true)

第八步，**操作信息会显示出来，操作成功后，关闭**即可。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_22.png?raw=true)

最后，返回去找test.xls文件，可以看到该文件新建了一个与数据表同名的工作表，里面导出的数据正确。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_23.png?raw=true)

## 使用SQL脚本导出数据表

这里介绍如何用SQL把数据表导出到txt文件。

准备工作：

- 存有数据的数据表

在脚本中写如下语句，**只要将上文中导入的命令“in”改为“out”即可**：

```sql
-- 启用xp_cmdshell
exec sp_configure 'xp_cmdshell',1
-- 重新配置 
reconfigure
EXEC master..xp_cmdshell 'BCP  test2.dbo.test out D:\sqlServerExp\test.txt -c -t -T'
```



**其中“test2.dbo.test”是省略了服务器名的对象限定名，即“数据库名.数据库架构名.对象名”，根据实际情况更改即可。

“D:\sqlServerExp\test.txt”为导出文件位置**。

如图，执行成功。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_28.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-3_31.png?raw=true)

---

参考连接：

[使用BCP导出导入数据](https://www.cnblogs.com/zerocc/p/3225723.html)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。