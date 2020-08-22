---
title: SQLServer学习(7)C语言txt文本插入数据库
date: 2019-06-07 15:59:50
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

[SQLServer学习(6)C语言连接数据库](http://www.spyxxx.xyz/2019/05/29/SQLServer%E5%AD%A6%E4%B9%A0-6-C%E8%AF%AD%E8%A8%80%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/)

我的配置：

- 操作系统：Window10 x64
- SQL Server 2016
- IDE：Visual Studio 2017

这篇文章将介绍如何用C语言将TXT文本数据插入SQLServer数据库。

---

# 读取TXT文件内容

首先，抛开SQLServer数据库，单纯地用C语言实现读取TXT格式文本内容。

## 代码

```c
#include <stdio.h>
#include <stdlib.h>
#include <string.h>
#define MAX_LINE 1024
struct data {
	int id;
	char name[10];
}person[1000];

int main()
{
	char buf[MAX_LINE];  /*缓冲区*/
	FILE *fp;            /*文件指针*/
	int len;             /*行字符个数*/
	int i=0;
	if ((fp = fopen("D:\\vs_project\\WriteTXT\\WriteTXT\\WriteTXT\\test.txt", "r")) == NULL)
	{ //如果打开文件错误，报错，退出
		perror("fail to read"); 
		exit(1);
	}
	printf("id\tname\n");
	while(fgets(buf, MAX_LINE, fp) != NULL)
	{
		len = strlen(buf);
		buf[len - 1] = '\0';  /*去掉换行符*/
		
		sscanf(buf,"%d,%s", &person[i].id, &person[i].name);
		printf("%d\t%s\n", person[i].id, person[i].name);
		i++;	
	}
	printf("num:%d\n", i);
	getchar(); 
	//关闭文件 
	fclose(fp);
	return 0;
}
```

## 相关函数

### perror()函数

`void perror(const char *str)` 把一个描述性错误消息输出到标准错误 stderr。首先输出字符串 **str**，后跟一个冒号，然后是一个空格。

### exit()函数

`void exit(int status)` 立即终止调用进程。任何属于该进程的打开的文件描述符都会被关闭，该进程的子进程由进程 1 继承，初始化，且会向父进程发送一个 SIGCHLD 信号。

### fopen()函数

 `FILE *fopen(const char *filename, const char *mode)` 使用给定的模式 mode 打开 filename 所指向的文件。

- **filename** -- 这是 C 字符串，包含了要打开的文件名称。

- **mode** -- 这是 C 字符串，包含了文件访问模式，模式有：`r`、`w`、`a`、`r+`、`w+`、`a+`。


该函数返回一个 FILE 指针。否则返回 NULL，且设置全局变量 errno 来标识错误。

### fgets()函数

`char *fgets(char *str, int n, FILE *stream)` 从指定的流 stream 读取一行，并把它存储在 str 所指向的字符串内。当读取 (n-1) 个字符时，或者读取到换行符时，或者到达文件末尾时，它会停止，具体视情况而定。

- **str** -- 这是指向一个字符数组的指针，该数组存储了要读取的字符串。
- **n** -- 这是要读取的最大字符数（包括最后的空字符）。通常是使用以 str 传递的数组长度。
- **stream** -- 这是指向 FILE 对象的指针，该 FILE 对象标识了要从中读取字符的流。

### sscanf()函数

`int sscanf(const char *str, const char *format, ...)` 从字符串读取格式化输入。

### fscanf()函数

与`sscanf()`函数类似，`int fscanf(FILE *stream, const char *format, ...)` 从流 stream 读取格式化输入。fscanf能正确操作的txt文件编码方式为ANSI，以下编码方式均不能使函数正常执行：~~UTF-8，Unicode，Unicode big endian~~。

## 报错

VS使用fopen()时可能会报如图错误“error C4996: 'fopen': This function or variable may be unsafe. Consider using fopen_s instead. To disable deprecation, use _CRT_SECURE_NO_WARNINGS. See online help for details.”：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_1.png?raw=true)

### 解决方法

（1）在VS中，**在“解决方案资源管理器”中项目名上单击右键——选择“属性”。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_2.png?raw=true)

（2）**在属性页中的“预处理器”中添加预处理定义。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_3.png?raw=true)

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_4.png?raw=true)

（3）**把 _CRT_SECURE_NO_WARNINGS添加进去，确定保存**即可，再次运行就没有问题了。

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_5.png?raw=true)

## 结果

TXT文本文件里的内容，如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_6.png?raw=true)

程序运行效果如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_7.png?raw=true)

# 插入数据库

结合上一篇文章[SQLServer学习(6)C语言连接数据库](http://www.spyxxx.xyz/2019/05/29/SQLServer%E5%AD%A6%E4%B9%A0-6-C%E8%AF%AD%E8%A8%80%E8%BF%9E%E6%8E%A5%E6%95%B0%E6%8D%AE%E5%BA%93/)将部分代码修改写成`void insertData(int i)`函数，然后和上文读取TXT文本的代码写成`int readTxt(char path[1024])`函数相结合即可。

## 代码

```c
#include <stdio.h>
#include <string.h>     
#include <windows.h>     
#include <sql.h>     
#include <sqlext.h>     
#include <sqltypes.h>     
#include <odbcss.h>  
#include <stdlib.h>
#define MAX_LINE 1024

SQLINTEGER id;
SQLCHAR name[20];
SQLINTEGER len_id, len_name;

struct data {
	int id;
	char name[10];
}person[1000];

/*读取txt文件*/
int readTxt(char path[1024])
{
	char buf[MAX_LINE];  //缓冲区
	FILE *fp;            //文件指针
	int len;             //行字符个数
	int i = 0;
	if ((fp = fopen(path, "r")) == NULL)
	{
		perror("fail to read");
		exit(1);
	}
	printf("id\tname\n");
	while (fgets(buf, MAX_LINE, fp) != NULL)
	{
		len = strlen(buf);
		buf[len - 1] = '\0';  //去掉换行符
		sscanf(buf, "%d,%s", &person[i].id, &person[i].name);
		printf("%d\t%s\n", person[i].id, person[i].name);
		i++;
	}
	printf("num:%d\n", i);
	//关闭文件 
	fclose(fp);
	return i;
}
/*插入数据*/
void insertData(int i) {
	SQLRETURN retcode;
	SQLHENV henv;
	SQLHDBC hdbc;
	SQLHSTMT hstmt;

	retcode = SQLAllocHandle(SQL_HANDLE_ENV, NULL, &henv); //申请环境句柄
	retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION,
		(SQLPOINTER)SQL_OV_ODBC3,
		SQL_IS_INTEGER); //设置环境属性
	retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc); //申请数据库连接句柄
	retcode = SQLConnect(hdbc, (SQLCHAR*)"SQLC", SQL_NTS, (SQLCHAR*)"SQLC", SQL_NTS, (SQLCHAR*)"SQLC123", SQL_NTS);
	if ((retcode == SQL_SUCCESS) || (retcode == SQL_SUCCESS_WITH_INFO)) { //判断是否成功执行
		printf("insertDataBase success!\n");
		retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);//申请SQL语句句柄 
		SQLCHAR sql[] = "USE test2";
		retcode = SQLExecDirect(hstmt, sql, SQL_NTS);//直接执行SQL语句
		SQLCHAR sql1[] = "INSERT INTO test VALUES (?,?);";
		SQLINTEGER P = SQL_NTS;
		retcode = SQLPrepare(hstmt, sql1, SQL_NTS);
		retcode = SQLBindParameter(hstmt, 1, SQL_PARAM_INPUT, SQL_C_LONG, SQL_INTEGER, 10, 0, &person[i].id, 0, &P);
		retcode = SQLBindParameter(hstmt, 2, SQL_PARAM_INPUT, SQL_C_CHAR, SQL_CHAR, 20, 0, &person[i].name, 20, &P);
		retcode = SQLExecute(hstmt);//执行插入SQL语句，前面可以用结构体加入问号内容
		if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {
			printf("insertData%d success!\n",i);
		}
		else {
			printf("insertData wrong!\n");
			SQLDisconnect(hdbc);//断开与数据库的连接 
		}

	}
	else {
		printf("insertDatabase wrong!\n");
		SQLDisconnect(hdbc);//断开与数据库的连接 
	}
	SQLFreeHandle(SQL_HANDLE_DBC, hdbc);//释放连接句柄 
	SQLFreeHandle(SQL_HANDLE_ENV, henv);//释放环境句柄
}
int main() {
	int num;
	int i;
	char path[]="D:\\vs_project\\WriteTXT\\WriteTXT\\WriteTXT\\test.txt";
	num = readTxt(path);
	for (i = 0; i < num; i++) {
		insertData(i);
	}
	getchar();
	return 0;
}
```

## 相关函数

SQL Server Native Client ODBC 驱动程序基于准备好的 SQL 语句创建临时存储过程。存储过程是多次执行某个语句的有效方式，不过创建存储过程的开销要比执行简单语句高。一般规则是，如果应用程序将要提交 SQL 语句超过三次，需要考虑使用 **SQLPrepare** 和 **SQLExecute**。在调用 **SQLPrepare** 之前使用 **SQLBindParameter** 绑定参数变量。

```c
SQLRETURN SQLBindParameter(
      SQLHSTMT        StatementHandle,     // statement句柄
      SQLUSMALLINT    ParameterNumber,    // 参数位于语句中的序号，最小为1
      SQLSMALLINT     InputOutputType,    // 入参/出参类型标识
      SQLSMALLINT     ValueType,          // 对应的C数据类型标识
      SQLSMALLINT     ParameterType,     // 对应的SQL数据类型标识
      SQLULEN         ColumnSize,          // 对应字段长度
      SQLSMALLINT     DecimalDigits,       // 如果是浮点数，则对应字段精度
      SQLPOINTER      ParameterValuePtr,   // 参数缓存
      SQLLEN          BufferLength,        // 参数缓存字节数
      SQLLEN *        StrLen_or_IndPtr);   // 用于表示字符串长度或NULL值的标识
```

## 结果

TXT文本内容为：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_9.png?raw=true)

原数据库test表格中的内容为：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_8.png?raw=true)

程序效果图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_10.png?raw=true)

程序执行后的test表格中的内容为：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/SQLServer/SQLfigure-7_11.png?raw=true)

---

参考连接：

[菜鸟教程 - C 库函数 - perror()](https://www.runoob.com/cprogramming/c-function-perror.html)

[菜鸟教程 - C 库函数 - exit()](https://www.runoob.com/cprogramming/c-function-exit.html)

[菜鸟教程 - C 库函数 - fopen()](https://www.runoob.com/cprogramming/c-function-fopen.html)

[菜鸟教程 - C 库函数 - fgets()](https://www.runoob.com/cprogramming/c-function-fgets.html)

[百度经验 - VS2013中如何解决error C4996: 'fopen'问题](https://jingyan.baidu.com/article/ce436649fd61543773afd32e.html)

[Microsoft Docs - SQLBindParameter](https://docs.microsoft.com/zh-cn/previous-versions/sql/sql-server-2008-r2/ms131462(v=sql.105))

[CSDN博客 - SQLBindParameter 函数的参数解析及使用方法](https://blog.csdn.net/u013187074/article/details/52895801)

---

初学，可能写的一些地方不对，欢迎指出。

如有问题，可以在评论区里讨论。如需转载，请注明出处，谢谢。

