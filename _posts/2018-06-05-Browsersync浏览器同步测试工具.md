---
layout: post
title: Browsersync浏览器同步测试工具
date: 2018-06-05
category: 工具
comments: true
excerpt: 当你在写静态文件的时候，重复刷新浏览器(F5)会感觉非常的繁琐。Browsersync解决了这些不必要的繁琐，它能让浏览器实时响应你对文件的修改并且可以自动为你刷新页面。如果你是一名前端开发者，这个东西我觉得是你的必备品。
---

当你在写静态文件的时候，重复刷新浏览器(F5)会感觉非常的繁琐。Browsersync解决了这些不必要的繁琐，它能让浏览器实时响应你对文件的修改并且可以自动为你刷新页面。如果你是一名前端开发者，这个东西我觉得是你的必备品。

要使用Browsersync，首先确保你的PC上已经安装了node.js和npm。

第一步：全局安装 Browsersync

    npm install -g browser-sync

也可以结合gulpjs安装到项目

    npm install --save-dev browser-sync

第二步：启用 BrowserSync

进入到你的某个项目目录下，比如有个名为test的项目，我们需要监听该羡慕下的css文件里面的style.css文件

```
// --files 路径是相对于运行该命令的项目（目录）
browser-sync start --server --files "css/*.css"
```

这条命令会让Browsersync启动一个小型服务器，并且在终端里会显示一条URL来让你查看你运行的页面。

如果需要监听多个文件，我们只需要用英文状态下的逗号(“,”)隔开。比如我们还需要监听一个index.html文件可以这样来操作。

```
// --files 路径是相对于运行该命令的项目（目录）
browser-sync start --server --files "css/*.css, *.html"
// 如果你的文件层级比较深，您可以考虑使用 **（表示任意目录）匹配，任意目录下任意.css 或 .html文件。
browser-sync start --server --files "**/*.css, **/*.html"
```

[Browsersync Github](https://github.com/BrowserSync/browser-sync)
