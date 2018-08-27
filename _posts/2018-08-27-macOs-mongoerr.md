---
title: macOs mongo err
date: 2018-08-27
category: 工具
tags: [tools]
excerpt: 使用Homebrew升级MongoDB后，启动发现报错："The data files need to be fully upgraded to version ... details."，这个报错有两种解决的办法。
---

macOS使用homebrew更新MongoDB到4.0后启动MongoDB会有一个报错 

```
The data files need to be fully upgraded to version 3.6 before attempting an upgrade to 4.0; see http://dochub.mongodb.org/core/4.0-upgrade-fcv for more details

```

#### 解决方案一：

打开终端，进入到`data/db`目录，使用命令`rm -rf *`（每当使用该命令时请谨慎小心，不可逆，不可逆，不可逆，重要的事情说三遍），删除`db`目录下的所有文件，然后重亲启动MongoDB。

#### 解决方案二：

重新下载一个3.6的MongoDB，降级！！！:smile: