---
title: 右键菜单栏不显示Git Bash Here问题解决
categories: notes
date: 2020-12-28 13:14:58
tags:
	- fix
photos:
---



# 问题

以前正常在 Windows 下安装 Git 工具时，在只要配置得当，在文件浏览器中，单击右键，就应该出现“ Git Bash Here ”选项，结果最近不知道为什么找不到了。~~（猜测：更新 Github Desktop 软件时有可能自动更新了 Git，或者什么东西覆盖了，未证实）~~

# 解决方案

1. **Win+R** 打开运行输入`regedit` 回车打开注册表；
2. 找到 **[HKEY_CLASSES_ROOT\Directory\Background]** ；
3. 在 **[Background]** 下如果没有 **[shell]** ，则**右键-新建项 [shell] **；
4. 在[shell]下右键-新建项[Git Bash Here]，其值为"`Git Bash Here`"（无双引号），此为右键菜单显示名称。
   此时在任意位置鼠标右击就能看到**Git Bash Here**但是没有关联程序，现在还没有实际作用；
5. 在 **[Git Bash Here]** 下**右键-新建-项 [command]**，其默认值为 `"git安装根目录\bin\bash.exe"`（需要加双引号）；
6. 再完善一下添加一个Git的小图标：选中Git Bash Here右击-新建-字符串值，名称为`Icon`,双击编辑，其值为`"git安装根目录\mingw64\share\git\git-for-windows.ico"`（需要加双引号）。

# 参考

[[1] Git------解决右键不显示Git Bash Here问题](https://blog.csdn.net/qq_37878579/article/details/107304736)



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。