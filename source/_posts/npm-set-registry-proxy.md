---
title: npm set registry & proxy
categories: notes
date: 2020-07-06 15:39:12
tags:
	- npm
	- nodejs
photos:
---





# ~/.npmrc设置

正常在**git-bash**里输入

```bash
npm config edit
```

就会用记事本弹出`~/.npmrc`的设置内容，可以直接修改，保存即可。

# npm设置镜像源registry

以下全用安装express作为示例。

## 临时使用镜像源

```bash
npm --registry https://registry.npm.taobao.org install express
```

## 设定固定镜像源

这里我设置淘宝镜像，一般这个速度足够了。

```bash
npm config set registry https://registry.npm.taobao.org
```

设置完成后可以通过下面命令进行查看确认，或者打开`~/.npmrc`查看。

```bash
npm config get registry
```

## 使用nrm管理镜像源

### a.下载nrm

```bash
npm install -g nrm
```

### b.添加镜像源

```bash
nrm add npm http://registry.npmjs.org
nrm add taobao https://registry.npm.taobao.org
```

### c.切换镜像源

```bash
nrm use taobao
nrm use npm
```

### d.查看已存镜像源

```bash
nrm ls
```

切换后查看方式如之前写的那样。

nrm其他命令内容：[npm--nrm](https://www.npmjs.com/package/nrm)

# npm设置代理proxy

```bash
npm config set proxy http://proxy.company.com:8080
npm config set https-proxy http://proxy.company.com:8080
```

如果你开了**SSR**，可以设置你的本机地址和你对应端口号。

对着“小飞机”单击右键——选项设置

![image-20200706155858670](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/06/155931-99487.png)

设置本地代理内容

![image-20200706160024320](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202007/06/160057-35142.png)

所以你在设置**npm**的**proxy**时，这么写

```bash
npm config set proxy http://127.0.0.1:11112
npm config set https-proxy http://127.0.0.1:11112
```

如果你用迅雷下载国外的什么内容很慢或者是干脆下载失败，就需要设置一下代理了。参数设置就以上那些内容。设置完后，秒下载好。

### 代理用户名和密码

```bash
npm config set proxy http://username:password@server:port
npm confit set https-proxy http://username:password@server:port
```

### 取消代理

可以还是在git-bash命令行里直接输入命令设置。或者打开`~/.npmrc`，删除设置代理的那两行，保存即可。

```bash
npm config delete proxy
npm config delete https-proxy
```

# 查看Npm Doc

如果对于npm的一些参数设置，还有些不清楚的话，可以在**git-bash**输入，查看

```bash
npm help config
```



# 参考链接

[CSDN--设置npm的registry几种方法](https://blog.csdn.net/qq_15980201/article/details/78765009)

[简书--npm设置registry](https://www.jianshu.com/p/bbcbf9f0508a)

[如何在公司Web代理后面设置Node.js和Npm](https://jjasonclark.com/how-to-setup-node-behind-web-proxy/)

[npm设置和取消代理的方法](https://blog.csdn.net/yanzi1225627/article/details/80247758)

---

写文不易，如需转载，请注明出处。

如果某处写的有问题，欢迎留言，一起交流讨论，共同进步。