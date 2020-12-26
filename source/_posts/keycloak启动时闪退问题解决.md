---
title: keycloak启动时闪退问题解决
categories: notes
date: 2020-12-26 16:42:15
tags:
	- Keycloak
	- fix
photos:
---

# 问题

在 Windows 系统下启动 Keycloak ，即鼠标双击打开 [bin] 文件夹下的 standalone.bat 启动 Keycloak 服务发现，闪退，根本看不到命令行界面，也看不到命令行报错信息。

后来通过 PowerShell 运行程序发现，原来是本机系统没有装 Java。

# 解决

因此，解决该问题就是**安装 Java 并配置 Java 环境变量**。

具体如何安装 Java ，如何配置 Java 环境变量，可以看[Java 开发环境配置 | 菜鸟教程](https://www.runoob.com/java/java-environment-setup.html)







---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。