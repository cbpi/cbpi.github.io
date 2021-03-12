---
layout: post
title: Homebrew更新资源缓慢
date: 2021-03-12
category: 解决方案
comments: true
excerpt: 使用Homebrew安装或者更新资源下载缓慢
tags: [解决方案]
---


　　很久没有写Blog了最近想起来再搞搞记录下生活记录下工作中碰到的事情，时间过去很久发现pull仓库里的github-page竟然很多依赖都需要更新，能怎么办呢，先得更新一波啊不然一大堆报错，或者就是本地无法运行起来。首先Homebrew就得更新，发现超级慢，就算连了vpn也依旧很慢。尝试了几条解决方案都不能够提上下载速度，最后在简书上发现一个解决的方案，亲测有效：

#### 手动执行下面这句命令，更换为中科院的镜像：
```
git clone git://mirrors.ustc.edu.cn/homebrew-core.git/ /usr/local/Homebrew/Library/Taps/homebrew/homebrew-core --depth=1
```

#### 复制代码然后把homebrew-core的镜像地址也设为中科院的国内镜像
```
cd "$(brew --repo)"
```

```
git remote set-url origin https://mirrors.ustc.edu.cn/brew.git
```

```
cd "$(brew --repo)/Library/Taps/homebrew/homebrew-core"
```

```
git remote set-url origin https://mirrors.ustc.edu.cn/homebrew-core.git
```

#### 然后执行
```
brew update
```
