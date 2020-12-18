---
title: 将博客部署到Github Pages
categories: notes
date: 2020-11-01 17:05:19
tags:
	- Hexo
	- Github
	- 建站
---

# 创建github新仓库

命名为“账户名.github.io

# 配置Hexo

配置Hexo博客 **_config.yml** 文件：

```yaml
deploy:
- type: git
  repo: git@github.com:goodmornight/goodmornight.github.io.git
  branch: main
```

# 上传静态文件

```bash
hexo g && hexo d
```

# GitHub设置

**Settings**设置根目录为`main`分支，文件夹为`/`

![image-20201103130313263](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202011/03/130326-451168.png)

# 参考

[[1] Working with GitHub Pages | GitHub Docs](https://docs.github.com/en/free-pro-team@latest/github/working-with-github-pages)

[[2] Websites for you and your projects.| GitHub Pages](https://pages.github.com/)



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。