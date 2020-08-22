---
title: docker部署mysql并提供客户端访问
date: 2019-05-12 22:18:25
tags:
	- MySQL
	- Docker
categories: notes
icon: /images/notes.svg
---

我们以Docker部署Mysql为例，介绍docker如何获取镜像、创建容器，从而部署mysql并提供客户端访问。

配置：

- Windows 10

- Docker 18.03.0

  - Mysql 5.7

---

# 获取Mysql镜像

首先**打开“Docker Quickstart Terminal”。**在获取Mysql镜像前，我们需要**查看我们已有的镜像**，即在Terminal里输入：

```dockerfile
docker images
```

如果没有这个镜像，则**在Docker Hub里查找**有没有“mysql”这个镜像（当然是有的）：

```
docker search mysql
```

如下图，命令行里打印出来了搜索内容，第一行就是我们想要的，从图片中，我们可以看到**第一列是“镜像名字”，第二列是对该镜像的描述，第三列是该镜像所获得的星星数，第四列如果有个[OK]字样则代表它是由官方项目组创建和维护的，第五列“automated”表示资源是否允许用户验证镜像的来源和内容**：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/Docker/20190512-1.png?raw=true)

然后获取mysql镜像，如果不表明获取哪个版本的镜像，则默认是最新版本，**建议获取确定的版本，以方便后期别的连接**。这里我获取5.7版本的mysql。

```
docker pull mysql:5.7
```

# 基于mysql镜像创建并运行容器

```
docker run --name study_mysql --privileged=true -p 3306:3306  -v /data/mysql/datadir:/var/lib/mysql -v /data/mysql/conf.d:/etc/mysql/conf.d -e  MYSQL_ROOT_PASSWORD=123456 -d  mysql:5.7
```

- **`docker run ***`是创建并运行容器的意思，等同于`docker create *** `+`docker start *** `**

- **`--name study_mysql` ，即指定容器名为“study_mysql”**

- **`--privileged=true` ，即防止挂载数据卷出现权限问题**

- **`-p 3306:3306` ，即设置映射宿主主机端口，前一个3306是对外端口，后一个3306是对内端口**

- **`-v` ，即挂载宿主目录到容器目录**

- **`-e` ，设置环境变量，此处指定root密码，即`MYSQL_ROOT_PASSWORD=123456`**

- **`-d` ,即以守护进程运行（后台运行）**

- **最后一个即是镜像名称**

这一条运行完后，**查看后台运行的镜像**：

```
docker ps
```

如图：

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/Docker/20190512-2.png?raw=true)

# 进入容器设置客户端访问权限

## 进入容器

```
docker exec -it study_mysql /bin/bash
```

其中，`-it` 为`-i` 和`-t` ：

- **`docker exec` ，即在运行的容器中执行命令**
- **`-i` 选项让容器的标准输入保持打开**
- **`-t` 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上**
- **这段命令的含义就是“在容器 study_mysql 中开启一个交互模式的终端”**

## 进入mysql数据库的客户端

```
mysql -u root -p
```

**输入完，上面的代码，回车，再输入密码（不可见），在敲一次回车，如账号密码没有错误的话，就进入了mysql客户端里了。**

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/Docker/20190512-3.png?raw=true)

## 修改root 可以通过任何客户端连接

```
ALTER USER 'root'@'%' IDENTIFIED WITH mysql_native_password BY '123456';
```

在mysql里输入命令的时候记得在最后加个分号“；”。

## 退出mysql

```
exit
```

这个语句末尾不用分号，也能退出mysql客户端。

## 宿主机或远程客户端工具访问

```
mysql -h 127.0.0.1 -P 3306 -u root -p
```

- **`-h 127.0.0.1` ，后面的ip为mysql服务器主机地址，但由于还是本机测试，所以仍是127.0.0.1**

- **`-P 3306` ，说明指定端口3306**

- 其他的同上

![](https://github.com/SPY-xxx/MyImagesOnline/blob/master/Docker/20190512-4.png?raw=true)

最后，提示，**退出容器，也是用`exit`。**

---

参考文章：

[CSDN-Docker部署mysql并提供客户端访问](https://blog.csdn.net/skh2015java/article/details/82659688)

[CSDN-docker部署mysql 实现远程连接](https://blog.csdn.net/weixin_42459563/article/details/80924634)

[Gitbooks-Docker Hub-拉取镜像](https://yeasy.gitbooks.io/docker_practice/repository/dockerhub.html#%E6%8B%89%E5%8F%96%E9%95%9C%E5%83%8F)

[菜鸟教程-Docker exec 命令](http://www.runoob.com/docker/docker-exec-command.html)

---

如需转载，请注明出处，谢谢~