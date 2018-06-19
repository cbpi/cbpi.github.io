---
layout: post
title: node+express+vue+mongodb实现前后端交互
date: 2018-05-25
category: JavaScript
---

# 准备
用vue-cli脚手架生成生
{% highlight javascript %}

npm install -g @vue/cli
init <template> <app-name> 从一个远程模板生成一个项目 (遗留 API, 依赖 `@vue/cli-init`)

{% endhighlight %}
关于vue-cli的文档可以看[这里](https://github.com/vuejs/vue-docs-zh-cn/blob/master/vue-cli/cli.md)

安装脚手架完成后就可以看到一个完整的项目结构了

## 后端配置

接下来我们需要生辰成一个本地服务器，在根目录下新建一个server的文件夹,文件夹里面新建

api.js(配置项目所需api)

db.js(配置数据库的链接)

index.js(服务器入口文件)

这个三个文件

![image](/images/1.png)

安装express，mongoose模块

{% highlight javascript %}npm install express mongoose --save{% endhighlight %}

在db.js中配置配置mongodb
{% highlight javascript %}
	'server/db.js'

	// 引入mongoose模块
	const mongoose = require('mongoose')
	// 连接数据库 如果不自己创建 默认生成端口号后面的名字，如这里会生成一个peopleinfo的库
	mongoose.connect('mongodb://localhost:27017/peopleinfo')

	const db = mongoose.connection
	db.once('error', () => console.log('Mongo connection error'))
	db.once('open', () => console.log('Mongo connection successed'))
   // 在根目录下新建models文件夹，在该文件加下新建peopleinfo.js

	'models/peopleinfo.js'

	// 引入mongoose模块
	const mongoose = require('mongoose');

	/************** 定义模式 peopleinfoSchema **************/
	const peopleinfoSchema = mongoose.Schema({
	  name: String,
	  sex: String,
	  hobby: String
	})

	/************** 定义模型 Model **************/
	const People = mongoose.model('Peopleinfo', peopleinfoSchema)

	module.exports = People
{% endhighlight %}
直接用node来操作数据库比较繁琐，一般推荐使用'mongoose'这个第三方模块来对数据库进行增删改查，关于mongoose中Schemas，Models的概念可以在官方网站上阅读

英文：[https://mongoosedoc.top/docs/cnhome.html](https://mongoosedoc.top/docs/cnhome.html)

中文：[http://mongoosejs.com/](http://mongoosejs.com/)

在peopleinfo.js中定义了一个peopleinfoSchema的Model，项目中对人物信息的增删改查我们可以基于这个People的Model来进行操作。

接下来编写增删改查的API，进入api.js

{% highlight javascript %}
server/api.js

"use strict";
const db = require('./db');
const peopleinfomodels = require('../models/peopleinfo');
const express = require('express');
const router = express.Router();

/************** 创建(create) 读取(get) 更新(update) 删除(delete) **************/

// 创建create 人物信息接口
router.post('/api/createinfo', (req, res) => {
  let newPeopleinfo = new peopleinfomodels({
    name: req.body.name,
    sex: req.body.sex,
    hobby: req.body.hobby,
  })
  // 保存人物信息的方法
  newPeopleinfo.save((err, data) => {
    if (err) {
      res.send(err)
    } else {
      res.send('created successed')
    }
  })
})

// 读取get 获取所有人物信息列表
router.get('/api/getallinfo', (req, res) => {
  // 查找所有人物信息的方法
  peopleinfomodels.find((err, data) => {
    if (err) {
      res.send(err);
    } else {
      res.send(data);
    }
  })
})

// 删除delete 根据id删除对应人物信息
router.delete('/api/deleteByid/:id', (req, res) => {
  // 查找所有人物信息的方法
  peopleinfomodels.findOneAndRemove({_id: req.params.id}, function(err, res) {
    if (!err) {

    }
  })
  res.sendStatus(200)

})

module.exports = router;
{% endhighlight %}
在这个文件中，首先引入了三个模块，引入express，使用它的路由功能([express 文档](http://www.expressjs.com.cn))，还用到了mongoose中基于模型操作的一些方法，最后导出路由，在入口文件index.js中引入。

{% highlight javascript %}

server/index.js

const api = require('./api')

const fs = require('fs')

const path = require('path')

const bodyParser = require('body-parser')

const express = require('express')
const app = express()

app.use(bodyParser.json())
app.use(bodyParser.urlencoded({
  extended: false
}));
app.use(api)
app.use(express.static(path.resolve(__dirname, '../dist')))
app.get('*', function (req, res) {
  const html = fs.readFileSync(path.resolve(__dirname, '../dist/index.html'), 'utf-8')
  res.send(html)
})
app.listen(8088)
console.log('success listen…………')

{% endhighlight %}

打开更目录下的package.json文件，找到"script"这个选项，添加一条命令
{% highlight ruby %}
"server": "node server/index.js"
{% endhighlight %}

在终端中执行 'npm run server'来启动本地后台。在这之前确保本地已经安装了MongoDB，并且已经启动。
链接数据库成功后终端会有这样的提示：

![image](/images/2.png)   

到这一步，后台的配置算是结束了。

---

## 前端配置

为了方便页面的架构，推荐使用[Element Ui](http://element-cn.eleme.io/#/zh-CN)，Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库。在vue cli脚手架里面使用Element，首先要在main.js里面引入，  
![image](/images/v1.png) 
 
接下来我们就可以在项目中愉快的使用了，在项目中我们会用一个表格来实现对数据库的增删改查功能，界面可以这样简单的来安排：
![image](/images/v2.png)

1. 添加按钮：实现增
2. 更新按钮：实现改
3. 删除按钮：实现删
4. 详情按钮：实现查

前端的代码可以在项目的/src/pages/home.vue里查看，表格的属性设置也可以在element官网上的[组件](http://element-cn.eleme.io/#/zh-CN/component/table)里查看。有几个注意注意项：
1. 删除单个item的时候要给按钮绑定一个id属性，根据id来删除数据库中的对应数据，id的值可以用scope.row('_id')来取
2. 更新单项数据的时候需要重新复制下原先的数据，使用[Object.assign()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/assign)这个方法。
3. 项目里我使用了axios，它是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端。他有一下特点：

---
* 从浏览器中创建 XMLHttpRequest

* 从 node.js 发出 http 请求

* 支持 Promise API

* 拦截请求和响应

* 转换请求和响应数据

* 取消请求

* 自动转换JSON数据

* 客户端支持防止 CSRF/XSRF



