---
title: React创建一个应用
categories: notes
date: 2020-07-20 10:31:34
tags:
	- React
photos:
---







使用React有好多种方法。

1. 通过`<script>`标记添加的html页面中(不适用工具链，也是引入React最简单的方法)
2. 通过**Create React App**创建单页应用
3. 官方文档还推荐了其他的方式，我没尝试过，在此引入

>- 如果你是在**学习 React** 或**创建一个新的[单页](https://zh-hans.reactjs.org/docs/glossary.html#single-page-application)应用**，请使用 [Create React App](https://zh-hans.reactjs.org/docs/create-a-new-react-app.html#create-react-app)。
>- 如果你是在**用 Node.js 构建服务端渲染的网站**，试试 [Next.js](https://zh-hans.reactjs.org/docs/create-a-new-react-app.html#nextjs)。
>- 如果你是在构建**面向内容的静态网站**，试试 [Gatsby](https://zh-hans.reactjs.org/docs/create-a-new-react-app.html#gatsby)。
>- 如果你是在打造**组件库**或**将 React 集成到现有代码仓库**，尝试[更灵活的工具链](https://zh-hans.reactjs.org/docs/create-a-new-react-app.html#more-flexible-toolchains)。

# Create React App

```bash
npx create-react-app my-app # my-app是项目名称
cd my-app  # 进入my-app项目
npm start  # 或者是yarn start
```

**npx 是npm5.2+附带的package运行工具。**

在我理解是，在项目中安装的包，你无法通过igt-bash命令行工具直接运行，但可以通过`npx webpack`(这里以项目中安装webpack为例)运行。

# Next.js

[Next.js](https://nextjs.org/) 是一个流行的、轻量级的框架，用于配合 React 打造**静态化和服务端渲染应用**。它包括开箱即用的**样式和路由方案**，并且假定你使用 [Node.js](https://nodejs.org/) 作为服务器环境。

从 [Next.js 的官方指南](https://nextjs.org/learn/)了解更多。

# Gatsby

[Gatsby](https://www.gatsbyjs.org/) 是用 React 创建**静态网站**的最佳方式。它让你能使用 React 组件，但输出预渲染的 HTML 和 CSS 以保证最快的加载速度。

从 Gatsby 的[官方指南](https://www.gatsbyjs.org/docs/)和[入门示例集](https://www.gatsbyjs.org/docs/gatsby-starters/)了解更多。

# 更灵活的工具链

以下工具链为 React 提供更多更具灵活性的方案。推荐给更有经验的使用者：

- **[Neutrino](https://neutrinojs.org/)** 把 [webpack](https://webpack.js.org/) 的强大功能和简单预设结合在一起。并且包括了 [React 应用](https://neutrinojs.org/packages/react/)和 [React 组件](https://neutrinojs.org/packages/react-components/)的预设。
- **[Nx](https://nx.dev/react)** 是针对全栈 monorepo 的开发工具包，其内置了 React，Next.js，[Express](https://expressjs.com/) 等。
- **[Parcel](https://parceljs.org/)** 是一个快速的、零配置的网页应用打包器，并且可以[搭配 React 一起工作](https://parceljs.org/recipes.html#react)。
- **[Razzle](https://github.com/jaredpalmer/razzle)** 是一个无需配置的服务端渲染框架，但它提供了比 Next.js 更多的灵活性。

# 从头开始打造工具链

一组 JavaScript 构建工具链通常由这些组成：

- 一个 **package 管理器**，比如 [Yarn](https://yarnpkg.com/) 或 [npm](https://www.npmjs.com/)。它能让你充分利用庞大的第三方 package 的生态系统，并且轻松地安装或更新它们。
- 一个**打包器**，比如 [webpack](https://webpack.js.org/) 或 [Parcel](https://parceljs.org/)。它能让你编写模块化代码，并将它们组合在一起成为小的 package，以优化加载时间。
- 一个**编译器**，例如 [Babel](https://babeljs.io/)。它能让你编写的新版本 JavaScript 代码，在旧版浏览器中依然能够工作。

如果你倾向于从头开始打造你自己的 JavaScript 工具链，可以[查看这个指南](https://blog.usejournal.com/creating-a-react-app-from-scratch-f3c693b84658)，它重新创建了一些 Create React App 的功能。

别忘了确保你自定义的工具链[针对生产环境进行了正确配置](https://zh-hans.reactjs.org/docs/optimizing-performance.html#use-the-production-build)。

# 参考链接

[React官方文档](https://zh-hans.reactjs.org/docs/create-a-new-react-app.html)







---

如有侵权，请联系删除。

本文主要是整理记录，自己写的内容相对不多。

注意文章编写时间，一切以官方文档为主。

如果某处写的有问题，欢迎发邮件，一起交流讨论，共同进步。