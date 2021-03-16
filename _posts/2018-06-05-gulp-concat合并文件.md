---
layout: post
title: gulp-concat合并文件
date: 2018-06-05
category: Gulp
---

当我们开发结束时，需要将项目中的文件做个合并，这样可以减少文件的数量。首先安装下插件：

    npm install --save-dev gulp-concat

然后引入下这个插件：

```
/*
引入gulp
*/
var gulp = require('gulp');
var sass = require('gulp-sass');
var concat = require('gulp-concat');

gulp.task('scripts', function() {
return gulp.src('javascript/*.js')
.pipe(concat('all.js'))
.pipe(gulp.dest('./dist/js/'));
});

```
执行上面的代码gulp会把javascript这个目录下的所有js文件合并，并且把合并后的文件命名为all.js保存在dist的js目录下。

![image](https://drakecb.me/wp-content/uploads/2017/02/4d9826960d27a4b.png)
