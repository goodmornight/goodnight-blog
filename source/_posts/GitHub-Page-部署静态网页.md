---
title: GitHub Page 部署静态网页
categories: notes
date: 2020-12-19 10:58:47
tags:
	- GitHub
	- GitHub Action
	- 建站
photos:
---

# 基本流程

## GitHub 项目 token 配置

1. 右上角用户头像处，选择 **Settings**

![image-20201219151712451](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/151725-108976.png)

2. **Settings** 页面中，选择 **Developer settings**

![image-20201219152034924](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/152159-691984.png)

3. **Personal access tokens** => **Generate new token**

![image-20201219152255611](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/152319-214168.png)

![image-20201219152324116](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/152344-920096.png)

4. **Note** 可以自定义起一个名字；**Select scopes** 按需勾选。

   ![image-20201219152522492](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/152525-652879.png)

5. **token** 创建完成后，复制进入，项目配置（一个 **token** 只能配置在一个项目上）。

6. 以我的项目 [goodmornight / github-actions-demo](https://github.com/goodmornight/github-actions-demo)**为例。

   1. **Settings** => **Secrets** => **New repository secret**

   ![image-20201219152944385](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/152954-513835.png)

   2. **Name** 为 **ACCESS_TOKEN**，该命名与后面 **github workflow** 有关。**Value** 就将刚刚复制的 **token** 粘贴到该处。

   ![image-20201219153426662](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/153441-439445.png)

## 项目配置

### React 项目配置

1. 先在本地生成一个React 应用。

```bash
npx create-react-app github-actions-demo
cd github-actions-demo
```

2. 进入package.json文件中添加：

   1. 如果最后想生成以`https://${username}.github.io/${repo name}`访问，可以这么写：

   ```json
   "homepage": "https://[username].github.io/github-actions-demo"
   ```

   

   1. 如果想以自定义域名方式访问，则这么写：

   ```json
   "homepage": "."
   ```

3. 在项目根目录( src文件所在位置，就是项目根目录 )下，创建 `.github/workflows` 文件目录，在此目录下创建一个 `yaml` 文件，例如：**ci.yml**
   1. 此处的 `${{ secrets.ACCESS_TOKEN }}`就是刚刚设置的 `token`
   2. 在 push 操作时，操作`npm install && npm run build`，并提交至 `gh-pages` 分支

```yaml
name: GitHub Actions Build and Deploy Demo
on:
  push:
    branches:
      - master
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@master

    - name: Build and Deploy
      uses: JamesIves/github-pages-deploy-action@master
      env:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        BRANCH: gh-pages
        FOLDER: build
        BUILD_SCRIPT: npm install && npm run build
```

4. 然后将本地仓库提交至远程仓库，**github** 识别到 **workflow** 文件以后，就会自动运行。

### VuePress 项目配置

1. 在项目根目录 `docs` ( src文件所在位置，就是项目根目录 )下，创建 `.github/workflows` 文件目录，在此目录下创建一个 `yaml` 文件，比如：`vuepress-deploy.yml`
   1. 相应的 `script` 命令需要在 `package.json` 中确认一下
   2. 以及 `build` 后生成的静态文件是不是放在 `src/.vuepress/dist/`

```yaml
name: Build and Deploy
on: [push]
jobs:
  build-and-deploy:
    runs-on: ubuntu-latest
    steps:
    - name: Checkout
      uses: actions/checkout@master

    - name: vuepress-deploy
      uses: jenkey2011/vuepress-deploy@master
      env:
        ACCESS_TOKEN: ${{ secrets.ACCESS_TOKEN }}
        # TARGET_REPO: goodmornight/vue-learning-docs
        # TARGET_BRANCH: gh-pages
        BUILD_SCRIPT: yarn && yarn build
        BUILD_DIR: src/.vuepress/dist/
```

其中，`package.json` 是这样设置的，重点看`vuepress-deploy.yml`当中的 `script`设置的是否与 `package.json` 中的一致 。（此处，我是设置为自定义域名，所以`"homepage": "."`）

```json
{
  "name": "vue-learning-docs",
  "version": "0.0.1",
  "description": "",
  "main": "index.js",
  "authors": {
    "name": "goodmornight",
    "email": "goodnightlilanhui@gmail.com"
  },
  "homepage": ".",
  "repository": "https://github.com/goodmornight/vue-learning-docs.git/vue-learning-docs",
  "scripts": {
    "dev": "vuepress dev src",
    "build": "vuepress build src"
  },
  "license": "MIT",
  "devDependencies": {
    "vuepress": "^1.5.3"
  }
}
```
### 自定义域名访问
参考：[VuePress项目配置](https://vuepress.vuejs.org/zh/guide/deploy.html#github-pages)

## github.io域名访问配置

上传至远程仓库后，还需要配置一些内容。

1. Settings => Options => GitHub Pages
2. 设置对应的 **Branch** 和**文件目录**

![image-20201219154535029](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/154715-177924.png)

等待 **GtiHub Action** 运行完成后，即可以通过域名`https://goodmornight.github.io/github-actions-demo`访问

## 自定义域名配置

### DNS设置

1. 如需自定义域名，需要设置已备案的域名。
2. 比如你的域名是 `goodnight.wiki`，你希望通过 `react.goodnight.wiki` 访问。
   1. 主机记录：`react`
   2. 记录类型：`CNAME`
   3. 记录值：`goodmornight.github.io.`（此处是github page域名，`[username].github.io`。注意最后有个小数点，但腾讯云会自动给你加上）
   4. 其余默认

### CNAME

在项目的 `build` 出来的静态文件夹下创建一个文件 **CNAME**，里面写自定义域名。

例如，刚刚设置的 `react.goodnight.wiki`，你只需要在 **CNAME** 文件中写这个即可。

在此项目中，就是在 `public`文件夹下，创建 **CNAME**，写入：

```
react.goodnight.wiki
```

### package.json

此处设置，前面也提到过，刚刚的 `homepage` 配置需要修改：

```json
"homepage": "."
```



### GitHub自定义域名配置

最后，**提交远程代码**，在 **GitHub** 项目中再设置一下：

Settings => Options => GitHub Pages

![image-20201219155751730](https://mdpic-1258411264.cos.ap-shanghai.myqcloud.com/night/202012/19/155754-964281.png)

等待 **GitHub Action** 运行完成，即可通过该域名访问了~

# 参考链接

[[1] GitHub Actions 入门教程 | 阮一峰](http://www.ruanyifeng.com/blog/2019/09/getting-started-with-github-actions.html)

[[2] GitHub Pages自定义域名 | 掘金](https://juejin.cn/post/6844903558106578957)

[[3] React Github Pages Deploy ERR_ABORTED 404 (Not Found)](https://stackoverflow.com/questions/53797321/react-github-pages-deploy-err-aborted-404-not-found)

[[4] github pages 自定义域名失效 | 简书](https://www.jianshu.com/p/4e030da623ea)



---

写文不易，如需转载，请注明出处。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。