---
layout: post
title: gulp-gulpfile文件
date: 2018-06-05
category: JavaScript
---

在项目里面新建一个gulpfile.js，编辑下这个文件添加一个任务。

```
/*
引入gulp
*/
var gulp = require('gulp');
/*
创建一个输出
"你好"的任务
*/
gulp.task('hello', function() {
    console.log('你好');
});
/*
创建一个输出
"世界"的任务
*/
gulp.task('world', function() {
    console.log('世界');
});
/*
默认执行任务的列表，
这里绑定了两个
*/
gulp.task('default', ['hello', 'world']);
```
![image](https://drakecb.me/wp-content/uploads/2017/01/254b6fda6a18b5e.png)

在终端里执行 gulp 后会看到执行了所有的任务，一个输出“你好”，一个输出“世界”。
