---
layout: post
title: Github删除所有提交历史记录
date: 2018-06-11
category: Git
comments: true
excerpt: 几条命令删除所有提交历史记录并强制更新存储库
tags: [Git]
---

1. 尝试 运行 git checkout --orphan latest_branch

2. 添加所有文件git add -A

3. 提交更改git commit -am "commit message"

4. 删除分支git branch -D master

5. 将当前分支重命名git branch -m master

6. 最后，强制更新存储库。git push -f origin master