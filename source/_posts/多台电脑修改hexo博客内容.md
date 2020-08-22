---
title: 多台电脑修改hexo博客内容
categories: notes
date: 2020-08-22 21:43:21
tags:
	- Hexo
	- 建站
photos:
---







一直以来都只能在实验室这台电脑写我的博客，感觉很不方便，所以今天尝试着看看我能不能通过git将博客代码放到git远程仓库里。

之前部署博客的文章在这儿：

[[1]服务器nginx-git-hexo搭建个人博客]([http://www.goodnight.wiki/2020/06/26/%E6%9C%8D%E5%8A%A1%E5%99%A8nginx-git-hexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/](http://www.goodnight.wiki/2020/06/26/服务器nginx-git-hexo搭建个人博客/))

[[2]网站备案后其他设置]([http://www.goodnight.wiki/2020/07/07/%E7%BD%91%E7%AB%99%E5%A4%87%E6%A1%88%E5%90%8E%E5%85%B6%E4%BB%96%E8%AE%BE%E7%BD%AE/](http://www.goodnight.wiki/2020/07/07/网站备案后其他设置/))

# Hexo博客部署原理



hexo的部署命令：

```bash
hexo d
```

1. 生成站点有关文件到 `.deploy_git`
2. 把它初始化为git目录，并根据你的配置指定remote和branch(一般是master)
3. 进行`git commit`，并把修改push到指定的remote branch
4. 命令执行完成后，到你的github仓库，你会发现master分支上的内容和'.deploy_git'中一样

# 添加到github或其他远程仓库

1. 首先，需要新建一个新的git仓库

2. 本地仓库关联github远程仓库，名字取名为`github`

   ```bash
    git remote add github git@github.com:GoodNight-Git/goodnight.git
   ```

   查看你的仓库关联的远程仓库

   ```bash
   git remote -v
   ```

   

3. 在博客根目录下，新建一个分支，专门用于保存hexo博客的原始文件，而非静态文件。

   我新建一个本地分支`source`

   ```bash
   git checkout -b source
   git add .
   git commit -m '<提交内容>'
   git push github source   # 提交source分支到github远程仓库
   ```

   

4. 可以在github上设置source分支为主分支。其他电脑可以通过github远程仓库拉下来文件，并执行`npm install`，初始化hexo相关依赖，前提在电脑里装好hexo。

5. 完成以上步骤后合并source分支。切换到主分支下，合并source分支。

   ```bash
   git merge source
   ```

   

6. 每次提交还是先部署，后新建切换source分支提交source代码即可。

# 相关git操作

1. 查看分支：

```bash
git branch
```

2. 删除分支：

```bash
git branch -d source
```

3. 切换到source分支：

```bash
git checkout source
```





# 参考链接

[多台电脑使用Hexo](https://www.jianshu.com/p/4bcf2848b3fc)

[廖雪峰-使用Gitee](https://www.liaoxuefeng.com/wiki/896043488029600/1163625339727712)

---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。