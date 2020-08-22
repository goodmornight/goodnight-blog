---
title: win10搭建coding.net+hexo博客
date: 2017-08-15 12:39:53
tags:
	- Win10
	- Hexo
	- Coding.net
categories: projects
icon: /images/projects.svg
---

距离我搭好博客也有两三天了，我现在才基本上全部配置好。在这期间，我的hexo装了卸，卸了装，可算是被我管教的服服帖帖的了。接下来，我和大家分享我的搭建博客过程。

我先说说我的配置：

Windows 10 x64

git 2.14.1

node.js LTS 6.11.2

hexo-cli 1.0.3


## 一、Windows 10上的部署

### 1.注册github账号

基本看英文照着注册就好。[github官网](https://github.com/)


### 2.安装git

Git是上传到Github的工具，如果在Github上有项目都会用到这个。

[下载git地址](https://git-scm.com/downloads)

注意你自己电脑的版本，至于git如何安装，你注意选好下面这几个选项，其他默认就好。选择几个功能后，操作更像是在Linux Shell里操作。

Use Git from Bash only,
Checkout Windows-style,
commit Unix-style line endings,
Use MinTTY(the default terminal of MSYS2)

### 3.配置git

安装好后，鼠标指向**git bash**单击右键，**以管理员身份运行**。输入这几条命令。自行替换自己github的用户名和邮箱。

```bash
git config --global user.name "Your Name"
git config --global user.email "email@example.com"
```



注意：为了避免出差错，以下在命令行操作的内容，都在**git bash**里进行,并且都是**以管理员身份运行**，不然容易出很多奇奇怪怪的问题。

### 4.创建coding.net账号

在coding.net注册一个账户，这个很简单，又是中国的网站，要求啥就照着做就好。

### 5.生成密钥文件

这个操作加密你的通信过程，同时后期上传到Github都会用到。邮箱填你自己coding.net注册邮箱。

```bash
ssh-keygen -t rsa -b 4096 -C "email@example.com"
```

按3次回车，不填任何内容。最后记住你的.ssh保存在了哪。一般证书文件会在C:\User\用户名\.ssh 生成两个文件。

### 6.安装node.js

去官网下载NodeJS Windows版本，建议选择LTS版本，[nodejs官网下载](https://nodejs.org/en/download/)

安装的时候务必选择Add to PATH选项。

### 7.npm更新源更改

装好了node.js就装好了npm。但是npm的源在国外，下载速度太慢，还容易下载失败，因此我们要把npm的更新源更改为淘宝镜像源。

还是在**git bash**里输入，注意**管理员身份**。

```bash
npm config set registry https://registry.npm.taobao.org
```

配置后可通过下面方式来验证是否成功 。

```bash
npm config get registry
```

你也可以用cnpm，具体的你可以在[淘宝NPM镜像](https://npm.taobao.org/)了解。

### 8.安装hexo

终于可以安装hexo了！还是在**git bash**里输入，注意**管理员身份**。

```bash
 npm install hexo-cli -g 
```

**中途有任何卡顿都别放弃，直到它自动结束，才算完成。**只是警告（WARN）就没关系。如果是错误（ERR）就有问题了，有问题再找到问题，百度解决。

至此在Win上的部署安装已经完成，接下来就是如何使用了。

## 二、Hexo的使用

### 1.新建站点

假设你要把你的博客代码放在G:\hexo\blog

```bash
cd /g
mkdir hexo
cd hexo
hexo init blog
```

还是那句话，**无论多慢都要等它进行完**。不然后面会出问题的。

完成后就可以在G:\hexo\blog看到所有文件了。


### 2.hexo配置

#### (1)替换主题

人气最高的主题是NexT主题，也是一切考虑的最周到的主题。但我用的是Cutie。看个人喜好吧。你可以去[hexo主题](https://hexo.io/themes/)看看，选择你自己喜欢的主题。然后去点进主题查看它的官方文档，它会提示你该如何使用这个主题。我这里还是以[NexT](https://github.com/iissnan/hexo-theme-next)为例。

进入你的blog代码文件夹，还是在**git bash**里输入，注意**管理员身份**。(我重复了n回了，为了避免出问题)，把主题安装到blog文件夹下的themes/next。并命名为next。

```bash
cd /g/hexo/blog
git clone https://github.com/iissnan/hexo-theme-next themes/next
```

回到根目录，找到_config.yml 文件，把theme字段里把默认主题名字换成next即可。

#### (2)插入图片

**Markdown**支持插入本地图片或外部链接图片，要保证本地和网络上都能访问图片，使用[**hexo-asset-image**](https://github.com/CodeFalling/hexo-asset-image)。**用git bash安装，并且所在目录一定要重视，安装hexo插件，一定要在根目录下，不然会出错**。

```bash
## 安装插件
npm install https://github.com/CodeFalling/hexo-asset-image --save
启用插件，确保主题配置文件D:\hexo\blog\_config.yml中
post_asset_folder: true
## 新增博文后，会在D:\hexo\blog\source\_posts目录下生成 xxx.md 和 xxx 目录，将图片存放该目录中，使用时 ![](***.jpg)，这样Typora预览和发布到网上时Hexo博客都能正常显示图片
```



#### (3)新建博文

还是在你自己的博客根目录。

```bash
hexo new "Hello World"
```

在目录`G:\hexo\blog\source\_posts`下会生成Markdown文件Hello World.md

手动添加md文件到G:\hexo\blog\source\_posts目录效果一样。

#### (4)新建页面（后面如有需要再做，刚建好博客还不需要这种操作）

我这里以新建一个个人简历页面为例。**注意，后面如有需要再做，刚建好博客还不需要这种操作**。

```bash
hexo new page "resume"
```

在目录`G:\hexo\blog\source\`下会生成一个名字为**resume**的文件夹。文件夹里一个index文件夹和index.md。内容自行修改。但这些都是后话了。我们继续往下看。

#### (5)本地浏览静态页面（就是debug）

```bash
cd /d/hexo/blog
## 生成静态文件
hexo g
## 启动本地服务器
hexo s
```

打开浏览器，输入http://localhost:4000 即可看到站点的预览了。

tips：如果不成功，有可能是你的4000端口被占用了，我们换个端口。

解决方案：使用Ctrl+C中断本地服务，使用命令

```bash
hexo s -p 端口号
```

例如**hexo s -p 5000**重新开启本地服务，访问**http://localhost:5000/**可以看到博客页面了。

好了，我们可以在本地看到自己的博客是怎么样的了，但别人还不能看到，我们要把代码托管到noding.net上。

小贴士：

**hexo g**是**hexo generate**的简写。

**hexo s**是**hexo server**的简写。

剩下的搭好博客后，你可以自己去了解。

## 三、配置到coding.net

### 1.创建项目

![](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/4eeed5J140.png?imageslim)

项目名称和简介随意

项目类型选私有

勾选“使用README.md初始化项目”

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/F8i2l441c5.png?imageslim)

创建好后，点击左边选项**“代码”——“WebIDE”**

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/3bc53951b9.png?imageslim)

配置默认。

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/e42FJkg3mK.png?imageslim)

注意的是webIDE是部分收费的，但是不用着急，官方提供了如下方式免费获得其网站虚拟币——码币，且Coding在注册后会送给用户一些码币，足以让IDE跑起来！

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/hh74l07Gle.png?imageslim)

找到node.js点击使用

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/m8lEID5bI8.png?imageslim)

### 2.添加SSH keys

回到自己的项目，点击“设置”——“部署公钥”——“新建部署公钥”

复制公钥文件`id_rsa.pub`中的内容进去就好。公钥就是我们最开始弄好的，一般在C:\User\用户名\.ssh。

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/2A6b88E52l.png?imageslim)



### 3.测试SSH连接

老样子，git bash里执行。

```bash
ssh -T git@git.coding.net
```

如果出现`Hello username! You have connected to Coding.net by SSH successfully!`提示，则表示连接成功。

## 四、部署你的博客

### 1.站点配置文件

注意**站点配置文件**`D:\hexo\blog\_config.yml`中`deploy`参数配置如下，

```bash
deploy:
  type: git
  repo: 
    coding: git@git.coding.net:****/****.git,master
```

coding后面的内容，在自己的项目里查看。一般这两个都是用户名。

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/fAhl9GGamh.png?imageslim)

### 2.部署

好了，到了最重要的最后一步了。我当时就在这里耽误了好长时间。

网上教程用`hexo-deployer-git`插件，部署博客，但我怎么试都是失败的。后来，在胡大佬的帮助下，才得以成功部署。方法如下：

把coding.net仓库的东西git clone到本地。

```bash
git clone git@git.coding.net:****/****.git
```

![](http://upload-images.jianshu.io/upload_images/7101374-f1f7fb5b5467720f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

好了，G:\hexo\下会有个你coding.net的仓库的内容。

接着，进入blog文件夹，

按照之前说的`hexo g`建立静态页面，然后把产生的**public**文件夹里的东西全部复制到**“G:\hexo\你的仓库文件夹”**里,

以后每次都这样，,把原来库里的内容删除，把新建好的静态页面**public**全部替换过去。

然后再进入本地仓库文件夹，把文件全部添加

```bash
cd SPYxxx/
git add ./
```



![](http://upload-images.jianshu.io/upload_images/7101374-c2aa47b08b531bdc.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

你也可以备注新更改的内容为“Firt commit”方便查看

```bash
git commit -m "备注内容"
```



![](http://upload-images.jianshu.io/upload_images/7101374-178aa1a4bdf39faa.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

以后你可以在这里看到你写的备注，方便你管理。

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/cCb8HcBGi3.png?imageslim)

最后上传到网络，master分支。进入到你的coding.net项目，你就可以看到你上传的内容了。然后在网址输入你的域名，所有人就都可以看到你的博客了~

```bash
git push -u origin master
```



![](http://upload-images.jianshu.io/upload_images/7101374-89c549c63554fd25.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![](http://upload-images.jianshu.io/upload_images/7101374-691b411e5cb8f3f9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

![mark](http://oupvuwiny.bkt.clouddn.com/spyxxx/170815/ba5dimJ50D.png?imageslim)

## 五、其他辅助工具

### Markdown书写工具

推荐[**Typora**](https://www.typora.io/)，用markdown书写完即时显示效果，不是分栏显示,我个人感觉特别好，但有的时候排版会出问题。

[Typora使用教程](http://jingyan.baidu.com/article/09ea3ede151af8c0afde397e.html?allowHTTP=1)

### 七牛

注册[**七牛**](https://portal.qiniu.com/signup?code=3layz9bl08bwy)，在`对象存储`中新建存储空间，要选择公开空间，不然上传图片后无法分享外部链接。

### MPic

下载[**MPic-图床神器**](http://mpic.lzhaofu.cn/)，设置账号，`空间名`为七牛对象存储空间名称，`AccessKey`和`SecretKey`在七牛`个人面板`下的`密钥管理`中，`域名`为七牛对象存储空间中`内容管理`页签下的`外链默认域名。`

## 六、TIPS

### 1.注意研读hexo官方文档

我这里有个大佬带上自己的理解翻译好的，可以学习。

传送门：[Hexo Docs](http://www.ituring.com.cn/article/199295)

### 2.注意研读你所使用的主题其作者写的使用说明

上面会介绍你可以如何使用这个主题达到作者主题里面内置的效果。

### 3.如果安装出问题了，尤其是hexo，你想卸了重装，一定要卸干净了，不可有残留。

比如，卸载node.js，别忘了把appdata里的nodejs的内容删除。

## 七、参考文章

[轻快装B低成本–Win10搭建Hexo博客](https://www.zrj96.com/post-471.html)

[hexo博客搭建时遇到的一些问题](https://segmentfault.com/a/1190000003710962)

[使用Coding.net来搭建基于Hexo的博客](http://blog.csdn.net/summer_zmc/article/details/55049906)

---

终于写完这篇文章了！！！我从中午写到下午，也写了有5个多小时了。

2017.8.12搭起的博客，直到今天2017.8.15我才完全调试好。然后就立即来记录下我搭博客的过程。

不得不说，坑很多，但还好我存活下来了，像我这么笨的人都能弄成，相信各位也一定能搭好。在此，我再次感谢胡大佬同学，感谢他孜孜不倦的教导。大佬的博客[BDZNH](https://bdznh.coding.me/)

最后希望大家都能搭好自己的博客，并且分享给我，一起学习分享。

**如需转载，请告知，并注明出处，谢谢。**