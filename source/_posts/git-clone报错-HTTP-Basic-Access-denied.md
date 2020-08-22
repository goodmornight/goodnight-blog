---
title: 'git clone报错:HTTP Basic: Access denied'
categories: notes
date: 2020-08-14 10:59:47
tags:
	- Git
photos:
---





# 问题

在git仓库下，`clone` 我的仓库，由于输错账号密码，无法`clone`。报错：

```
$ git clone http://xxxx.git
Cloning into 'xxxx'...
remote: HTTP Basic: Access denied
fatal: Authentication failed for ...
```



# 解决方法

## 解决方法1：凭据管理器修改

1. **设置** 中搜索“**凭据管理器**”。

2. 进入**凭据管理器**。

3. 删除对应凭据即可。

![image-20200814110640727](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202008/14/110644-877516.png)

## 解决方法2：git命令

**git bash**，在该git仓库目录下，输入

```bash
git config --system --unset credential.helper
```

然后，再次`clone`，输入正确的账号密码即可。

**但是该操作后，每次你对你的仓库有个什么操作都需要输入账号密码。**

解决方法：

```bash
git config --global credential.helper store
```

设置git记录你的账号密码。再次操作，输入正确账号密码后，就不需要你再次输入了。



# 参考链接

[CSDN-使用git报错 - remote: HTTP Basic: Access denied](https://blog.csdn.net/qq_25835645/article/details/84390221)



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。