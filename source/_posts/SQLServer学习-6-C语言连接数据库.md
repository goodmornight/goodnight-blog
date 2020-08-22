---
title: SQLServer学习(6)C语言连接数据库
date: 2019-05-29 12:41:24
tags: 
	- SQL Server
	- C
categories: notes
icon: /images/notes.svg
---

之前我的一些有关于SQLServer学习的文章：

[SQL Server学习(1)介绍与安装](http://spyxxx.xyz/2019/04/10/SQLServer%E5%AD%A6%E4%B9%A0-1-%E4%BB%8B%E7%BB%8D%E4%B8%8E%E5%AE%89%E8%A3%85/)

[SQL Server学习(2)创建数据库和数据表](http://spyxxx.xyz/2019/04/23/SQLServer%E5%AD%A6%E4%B9%A0-2-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%E6%95%B0%E6%8D%AE%E8%A1%A8/)

[SQLServer学习(3)导入导出数据表](http://www.spyxxx.xyz/2019/05/06/SQLServer%E5%AD%A6%E4%B9%A0-3-%E5%AF%BC%E5%85%A5%E5%AF%BC%E5%87%BA%E6%95%B0%E6%8D%AE%E8%A1%A8/)

[SQLServer学习(4)给数据库、表生成SQL脚本](http://www.spyxxx.xyz/2019/05/14/SQLServer%E5%AD%A6%E4%B9%A0-4-%E7%BB%99%E6%95%B0%E6%8D%AE%E5%BA%93%E3%80%81%E8%A1%A8%E7%94%9F%E6%88%90SQL%E8%84%9A%E6%9C%AC/)

[SQLServer学习(5)为SQLServer创建登录账户](http://www.spyxxx.xyz/2019/05/29/SQLServer%E5%AD%A6%E4%B9%A0-5-%E4%B8%BASQLServer%E5%88%9B%E5%BB%BA%E7%99%BB%E5%BD%95%E8%B4%A6%E6%88%B7/)

我的配置：

- 操作系统：Window10 x64
- SQL Server 2016
- IDE：Visual Studio 2017

这篇文章将介绍如何用C语言连接SQLServer并显示数据表中的数据。

---

# 配置环境

## 启动SQLServer服务

1.先**查看SQLServer在本机对应的服务名称。**

**“Shift+Ctrl+Esc”打开任务管理器->服务->找到SQLServer对应的服务名称**，如图。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_1.png?raw=true)

可以看到MSSQLSERVER服务正在启动状态，但**如果在停止状态，可以以管理员身份打开cmd，输入**

```
net start [服务名]
```

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_2.png?raw=true)

## 配置ODBC环境

1.首先要有一个**以SQL Server身份验证登陆的账户**。

如果没有SQLServer身份的用户，可以看我之前写的一篇文章[SQLServer学习(5)为SQLServer创建登录账户](http://www.spyxxx.xyz/2019/05/29/SQLServer%E5%AD%A6%E4%B9%A0-5-%E4%B8%BASQLServer%E5%88%9B%E5%BB%BA%E7%99%BB%E5%BD%95%E8%B4%A6%E6%88%B7/)。

2.然后**打开“运行”，可以按“Win+R”键，调出运行框，然后搜索“odbcad32”，打开“ODBC数据源管理程序”对话框**。然后如图依次操作。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_3.png?raw=true)

3.**添加用户数据源（SQLServer）**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_4.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_5.png?raw=true)

4.**命名数据源（自定义），确定服务器（这里不要选择local，选择本机的名字）。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_6.png?raw=true)

5.**选择用SQLServer验证，输入账号密码**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_7.png?raw=true)

6.**默认数据库为master**。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_8.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_9.png?raw=true)

7.**测试数据源，看到测试结果“测试成功”即可。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_10.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_11.png?raw=true)

8.如图，添加成功。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_12.png?raw=true)

## 选择一个数据表

具体怎么建库建表，可以看我之前写的一篇文章：[SQL Server学习(2)创建数据库和数据表](http://spyxxx.xyz/2019/04/23/SQLServer%E5%AD%A6%E4%B9%A0-2-%E5%88%9B%E5%BB%BA%E6%95%B0%E6%8D%AE%E5%BA%93%E5%92%8C%E6%95%B0%E6%8D%AE%E8%A1%A8/)

如图，选择“test2”数据库里的“test”数据表。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_13.png?raw=true)

# C语言编写

## 新建项目

**使用Microsoft Visual Studio，新建一个空项目**。如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_14.png?raw=true)

## C语言关键函数

### SQLAllocHandle()函数

```c
SQLRETURN SQLAllocHandle(
     SQLSMALLINT     HandleType,
     SQLHANDLE       InputHandle,
     SQLHANDLE *     OutputHandlePtr);
```

**(1)参数：**

**`HandleType` ：[输入]**

该变量只能从下列四个中的一个：

`SQL_HANDLE_ENV`：用于申请环境句柄
`SQL_HANDLE_DBC` ：用于申请连接句柄

`SQL_HANDLE_DESC`：用于申请描述符句柄
`SQL_HANDLE_STMT`：用于申请语句句柄

**`InputHandle` ：[输入]**

该变量放入已经被分配好的前提句柄，如果第一个变量为环境句柄，则放入`SQL_NULL_HANDLE`即可，若果第一个变量为`SQL_HANDLE_DBC`，则第二个变量必须为已分配的环境句柄，如第一个变量为`SQL_HANDLE_DESC`，`SQL_HANDLE_STMT`则，第二个变量必须为已分配好的连接句柄。

**`OutputHandlePtr` ：[输出]**

该变量为一个指针变量，用于保存申请来的句柄，申请句柄类型为第一个变量，在定义该指针的时候需注意类型一致。

**(2)函数返回值(4种)：**

`SQL_SUCCESS`、`SQL_SUCCESS_WITH_INFO`、`SQL_INVALID_HANDLE`或`SQL_ERROR`。

### SQLSetEnvAttr()函数

```c
SQLRETURN SQLSetEnvAttr(  
     SQLHENV      EnvironmentHandle,  
     SQLINTEGER   Attribute,  
     SQLPOINTER   ValuePtr,  
     SQLINTEGER   StringLength);  
```

(1)参数

**`EnvironmentHandle`：[输入]环境句柄。**

**`Attribute`：[输入]要设置，属性列在"注释"。**

**`ValuePtr` ：[输入]指向要与之关联的值*属性*。 具体取决于值*特性*， *ValuePtr*将 32 位整数值或指向以 null 结尾的字符串。**

**`StringLength`：[输入]如果ValuePtr指向的字符串或二进制缓冲区中，此参数应为的长度 ValuePtr。 对于字符串数据，此参数应包含在字符串中的字节数。**如果*ValuePtr*是一个整数*StringLength*将被忽略。

(2)函数返回值

`SQL_SUCCESS`、 `SQL_SUCCESS_WITH_INFO`、 `SQL_ERROR` 或` SQL_INVALID_HANDLE`。

### SQLConnect()函数

```c
SQLRETURN SQLConnect(  
     SQLHDBC        ConnectionHandle,  
     SQLCHAR *      ServerName,  
     SQLSMALLINT    NameLength1,  
     SQLCHAR *      UserName,  
     SQLSMALLINT    NameLength2,  
     SQLCHAR *      Authentication,  
     SQLSMALLINT    NameLength3);  
```

(1)参数

**`ConnectionHandle`：[输入]连接句柄。**

**`ServerName`：[输入]数据源名称。** 数据可能不位于该程序，在同一台计算机上，也可以在网络上的某个位置的另一台计算机上。

**`NameLength1`：[输入]长度 `ServerName`以字符为单位。**

**`UserName`：[输入]用户标识符。**

**`NameLength2`：[输入]长度 用户名以字符为单位。**

**`Authentication`：[输入]身份验证的字符串 （通常是密码）。**

**`NameLength3`：[输入]长度 身份验证以字符为单位。**

(2)函数返回值

`SQL_SUCCESS`、 `SQL_SUCCESS_WITH_INFO`、 `SQL_ERROR`、 `SQL_INVALID_HANDLE` 或` SQL_STILL_EXECUTING`。

### SQLBindCol()函数

```c
SQLRETURN SQLBindCol(
	SQLHSTMT          StatementHandle,
	SQLUSMALLINT      ColumnNumber,
	SQLSMALLINT       TargetType,
	SQLPOINTER        TargetValuePtr,
	SQLINTEGER        BufferLength,
	SQLLEN *          StrLen_or_Ind);
```

(1)参数

**`StatementHandle`：[输入]语句句柄。**

**`ColumnNumber`：[输入]要绑定的列集的结果数**。列中从 0 开始，其中第 0 列书签列的列顺序递增编号。 如果不使用书签-也就是说，`SQL_ATTR_USE_BOOKMARKS` 语句属性设置为 `SQL_UB_OFF`，然后列号从 1 开始。

**`TargetType`：[输入]C 数据类型的标识符 TargetValuePtr缓冲区。** 当它从数据源检索数据`SQLFetch`， `SQLFetchScroll`， `SQLBulkOperations`，或者`SQLSetPos`、驱动程序将数据转换为此类型;当其将数据发送到数据源`SQLBulkOperations`或`SQLSetPos`，驱动程序将数据从这种类型转换。

如果`TargetType`参数中的 `SQL_DESC_DATETIME_INTERVAL_PRECISION` 和 `SQL_DESC_PRECISION` 字段设置为间隔数据类型，默认时间间隔的前导精度 (2) 和默认时间间隔的秒精度 (6)，ARD，分别用于数据。 如果`TargetType`参数是 `SQL_C_NUMERIC`，默认的精度 （驱动程序定义） 和 ARD 的` SQL_DESC_PRECISION` 和 `SQL_DESC_SCALE` 字段中设置默认小数位数 (0)，用于数据。 如果任何默认精度或小数位数不适当，应用程序显式应通过调用设置适当的描述符字段`SQLSetDescField`或`SQLSetDescRec`。

**`TargetValuePtr`：延迟的输入/输出指向要绑定到的列的数据缓冲区的指针。**`SQLFetch`并`SQLFetchScroll`在此缓冲区中返回数据。`SQLBulkOperations`在此返回数据缓冲区时操作是 `SQL_FETCH_BY_BOOKMARK`; 它检索的数据从该缓冲区时操作`SQL_ADD `或` SQL_UPDATE_BY_BOOKMARK`。 `SQLSetPos`在此返回数据缓冲区时操作是 `SQL_REFRESH`; 它检索的数据从该缓冲区时操作是 `SQL_UPDATE`。

如果`TargetValuePtr`是 null 指针，该驱动程序取消绑定列的数据缓冲区。 应用程序可以通过调用取消绑定所有列`SQLFreeStmt`使用 `SQL_UNBIND` 选项。 应用程序可以取消绑定列的数据缓冲区，但如果仍有长度/指示器缓冲区列中，发往`TargetValuePtr调用中的参数`SQLBindCol`是 null 指针，但`StrLen_or_IndPtr`参数是有效的值。

**`BufferLength`：[输入]长度 TargetValuePtr以字节为单位的缓冲区。**

驱动程序使用*BufferLength*以免超出末尾的编写`TargetValuePtr`缓冲时它将返回长度可变的数据，如字符或二进制数据。 请注意，该驱动程序，它返回字符数据时都计的 null 终止字符`TargetValuePtr`。 `TargetValuePtr`因此必须包含空间的 null 终止字符或驱动程序将截断数据。

当驱动程序返回固定长度的数据，如整数或日期结构时，驱动程序会忽略*BufferLength*并假定缓冲区足够大以保存数据。 因此，务必要为固定长度的数据分配缓冲区足够大的应用程序或驱动程序将写入缓冲区的结束。

`SQLBindCol`返回 `SQLSTATE HY090` （字符串或缓冲区长度无效） 时`BufferLength`是小于 0，但不是在时`BufferLength`为 0。 但是，如果`TargetType`指定字符的类型，不应设置应用程序`BufferLengt`为 0，因为符合 ISO CLI 的驱动程序返回 `SQLSTATE HY090` （字符串或缓冲区长度无效），用例。

**`StrLen_or_IndPtr` ：延迟的输入/输出要绑定到的列的长度/指示器缓冲区的指针**。`SQLFetch`并`SQLFetchScroll`此缓冲区中返回值。 `SQLBulkOperations`检索一个值，从该缓冲区何时操作是 SQL_ADD、 `SQL_UPDATE_BY_BOOKMARK`，还是` SQL_DELETE_BY_BOOKMARK`。 `SQLBulkOperations`返回一个值，在此缓冲区何时操作是 `SQL_FETCH_BY_BOOKMARK`。 `SQLSetPos`返回一个值，在此缓冲区何时操作是 `SQL_REFRESH`; 它检索一个值，从该缓冲区时操作是` SQL_UPDATE`。

`SQLFetch`，`SQLFetchScroll`，`SQLBulkOperations``，并且`SQLSetPos`可以返回的长度/指示器缓冲区中的以下值：

- 可用于返回的数据的长度
- `SQL_NO_TOTAL`
- `SQL_NULL_DATA`

应用程序可以将以下值放在与一起使用的长度/指示器缓冲区`SQLBulkOperations`或`SQLSetPos`:

- 正在发送的数据的长度
- `SQL_NTS`
- `SQL_NULL_DATA`
- `SQL_DATA_AT_EXEC`
- `SQL_LEN_DATA_AT_EXEC` 宏的结果
- `SQL_COLUMN_IGNORE`

如果指示器缓冲区和长度的缓冲区是单独的缓冲区，指示器缓冲区可返回仅` SQL_NULL_DATA`，而长度缓冲区可以返回所有其他值。

如果`StrLen_or_IndPtr`是使用空指针、 没有长度或指示器值。 当提取数据和数据为 NULL 时，这是一个错误。

(2)函数返回值

`SQL_SUCCESS`、` SQL_SUCCESS_WITH_INFO`、 `SQL_ERROR `或 `SQL_INVALID_HANDLE`。

### SQLExecDirect()函数

```c
SQLRETURN SQLExecDirect(
     SQLHSTMT     StatementHandle,
     SQLCHAR *    StatementText,
     SQLINTEGER   TextLength);
```

(1)参数

**`StatementHandle`：[输入]语句句柄。**

**`StatementText`：[输入]若要执行的 SQL 语句。**

**`TextLength：·[输入]长度 `StatementText`以字符为单位。**

(2)函数返回值

`SQL_SUCCESS`、 `SQL_SUCCESS_WITH_INFO`、 `SQL_NEED_DATA`、` SQL_STILL_EXECUTING`、 `SQL_ERROR`、 SQL_NO_DATA、 `SQL_INVALID_HANDLE` 或 `SQL_PARAM_DATA_AVAILABLE`。

### SQLFetch()函数

```C
SQLRETURN SQLFetch (SQLHSTMT hstmt);
```

`hstmt`：句柄

`SQLFetch()` 使游标前进到结果集的下一行，并检索任何已绑定的列。

`SQLFetch()` 可用来将数据直接接收到 `SQLBindCol()` 指定的变量中，也可以在取装之后通过调用 `SQLGetData()` 来分别接收列。如果在绑定列时指示了转换，则调用 `SQLFetch()` 时也将执行数据转换。

仅当最近对 *hstmt* 执行的语句是 SELECT 时，才可以调用 `SQLFetch()`。

使用 `SQLBindCol()` 绑定的应用程序变量的数目一定不能超出结果集中的列数，否则 `SQLFetch()` 将失败。

如果尚未调用 `SQLBindCol()` 来绑定任何列，则 `SQLFetch()` 不返回数据给应用程序，而仅仅使游标前进。在这种情况下，可接着调用 `SQLGetData()` 来个别地获取所有的列。当 `SQLFetch()` 使游标前进到下一行时，将废弃未绑定的列中的数据。

### SQLGetData()函数

```c
SQLRETURN SQLGetData(  
      SQLHSTMT       StatementHandle,  
      SQLUSMALLINT   Col_or_Param_Num,  
      SQLSMALLINT    TargetType,  
      SQLPOINTER     TargetValuePtr,  
      SQLLEN         BufferLength,  
      SQLLEN *       StrLen_or_IndPtr);  
```

**(1)参数：**

**`StatementHandle` ：[输入]语句句柄。**

**`Col_or_Param_Num` ：[输入]对于检索列数据**，它是列的要为其返回数据数。 结果集列在不断增加的列顺序从 1 开始编号。书签列是列号为 0;这可以是仅在指定是否启用书签。而用于检索参数数据时，它是从 1 开始的参数的序号。

**`TargetType` ：[输入]C 数据类型的类型标识符`TargetValuePtr`缓冲区**。

如果`TargetType`是` SQL_ARD_TYPE`，驱动程序使用 `ARD SQL_DESC_CONCISE_TYPE` 字段中指定的类型标识符。 如`TargetType`是` SQL_APD_TYPE`， **SQLGetData**将使用相同的 C 数据类型中指定**SQLBindParameter**。 C 数据类型中的指定，否则**SQLGetData**重写中指定的 C 数据类型**SQLBindParameter**。 如果它为` SQL_C_DEFAULT`，驱动程序将选择基于源的 SQL 数据类型的默认 C 数据类型。此外可以指定扩展的 C 数据类型。

**`TargetValuePtr`：[输出]指向用于返回数据缓冲区的指针。**`TargetValuePtr`不能为 NULL。

**`BufferLength`：[输入]长度 `TargetValuePtr`以字节为单位的缓冲区。**

驱动程序使用`BufferLength`以免超出末尾的编写`TargetValuePtr`缓冲时返回可变长度数据，如字符或二进制数据。 请注意，驱动程序将 null 终止字符，返回字符数据时记`TargetValuePtr`。`TargetValuePtr`因此必须包含 null 终止字符的空间或驱动程序将截断数据。

当驱动程序返回固定长度的数据，如整数或日期结构时，驱动程序会忽略*BufferLength*并假定缓冲区足够大以保存数据。 因此对于应用程序为固定长度的数据分配足够大的缓冲区或驱动程序将写入缓冲区的结束。

`SQLGetData`返回 `SQLSTATE HY090` （字符串或缓冲区长度无效） 时`BufferLength`是小于 0，但不是在时`BufferLength`为 0。

**`StrLen_or_IndPtr`：[输出]要返回的长度或指示器值在其中的缓冲区的指针**。如果这是 null 指针，则返回没有长度或指示器的值。 这将返回错误时要提取的数据为 NULL。

`SQLGetData`可以返回的长度/指示器缓冲区中的以下值：

- 可用于返回的数据的长度
- `SQL_NO_TOTAL`
- `SQL_NULL_DATA`

**(2)函数返回值**

`SQL_SUCCESS`、` SQL_SUCCESS_WITH_INFO`、` SQL_NO_DATA`、` SQL_STILL_EXECUTING`、` SQL_ERROR` 或 `SQL_INVALID_HANDLE`。

这部分主要参考了[Microsoft SQL文档](https://docs.microsoft.com/zh-cn/sql/connect/odbc/microsoft-odbc-driver-for-sql-server?view=sql-server-2017)，详细内容可以在官方文档查看。

## C语言代码

完整代码：

```c
#include <stdio.h>
#include <string.h>     
#include <windows.h>     
#include <sql.h>     
#include <sqlext.h>     
#include <sqltypes.h>     
#include <odbcss.h>  

SQLHENV henv = SQL_NULL_HENV;
SQLHDBC hdbc = SQL_NULL_HDBC;
SQLHSTMT hstmt = SQL_NULL_HSTMT;

SQLINTEGER id;
SQLCHAR name[20];
SQLINTEGER len_id, len_name;

int main() {
	SQLRETURN retcode;

	retcode = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv); //申请环境句柄
	retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,
		(SQLPOINTER)SQL_OV_ODBC3,
		SQL_IS_INTEGER); //设置环境属性
	retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc); //申请数据库连接句柄
	retcode = SQLConnect(hdbc, (SQLCHAR*)"SQLC", SQL_NTS, (SQLCHAR*)"SQLC", SQL_NTS, (SQLCHAR*)"SQLC123", SQL_NTS);
	if ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) { //判断是否成功执行
		printf("success!\n");
		retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);//申请SQL语句句柄 
		SQLCHAR sql[] = "USE test2";
		retcode = SQLExecDirect(hstmt, sql, SQL_NTS);//直接执行SQL语句
		SQLCHAR sql1[] = "SELECT * FROM test";
		retcode = SQLExecDirect(hstmt, sql1, SQL_NTS);//直接执行SQL语句
		if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {
			while (SQLFetch(hstmt) != SQL_NO_DATA) {
				SQLGetData(hstmt, 1, SQL_C_ULONG, &id, 20, &len_id);
				SQLGetData(hstmt, 2, SQL_C_CHAR, &name, 20, &len_name);
				printf("%d %s\n", id, name);
			}
			printf("success2!");
		}
		else {
			printf("false2!");
		}
		getchar();
	}
	else {
		printf("false!");
	}
	return 0;
}
```

运行结果：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-6_15.png?raw=true)

---

参考链接：

[C语言+ODBC+SQL 连接](https://www.cnblogs.com/beta-data/p/4457898.html)

[ODBC学习笔记—SQLAllocHandle](https://blog.csdn.net/tanshoudong0049/article/details/74538967)

[Microsoft-Using SQLBindCol](https://docs.microsoft.com/en-us/sql/odbc/reference/develop-app/using-sqlbindcol?view=sql-server-2017)

[VS用C语言连接SQL Server解决办法](https://blog.csdn.net/u013007900/article/details/51174128)

[学步园-SQLBindCol](https://www.xuebuyuan.com/573634.html)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。