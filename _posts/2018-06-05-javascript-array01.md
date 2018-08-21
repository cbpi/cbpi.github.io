---
layout: post
title: 把数组对象相同的key值合并，并且把对应的id放到一个数组
date: 2018-06-05
category: JavaScript
comments: true
---

做项目的时候碰到一个问题，需要把返回数据拼接成不同的div列。碰上vpn近端时间被封，只能百度了。还不错找到了差不多的解决方法，下面贴一下js代码：

{% highlight javascript %}
    var old = [
	   {
	     id: 1,
	     name: 'css',
	     type: 'html11'
	   },
	   {
	     id: 2,
	     name: 'css',
	     type: 'html22'
	   },
	   {
	     id: 3,
	     name: 'javacript',
	     type: 'code33'
	   },
	   {
	     id: 4,
	     name: 'javacript',
	     type: 'code44'
	   },
	   {
	     id: 5,
	     name: 'php',
	     type: 'drake'
	   },
	   {
	     id: 6,
	     name: 'php',
	     type: 'brew'
	   },
	   {
	     id: 6,
	     name: 'php',
	     type: 'brewss'
	   }
          ];
	  var hash = {};
	  var i = 0;
	  var res = [];
	  old.forEach(function(item) {
		var name = item.name;
		if(hash[name]){
		    res[hash[name] - 1].id.push(item.id);
		    res[hash[name] - 1].type.push(item.type);
		}else{
		    hash[name] = ++i && res.push({
			    id: [item.id],
			    name: name,
			    type: [item.type]
			});
		    }
		});
	console.log(res);
{% endhighlight %}

结合项目要求自己稍微改造了一下，把不同的type放入数组。转自[segmentfault.com](https://segmentfault.com/)

原文链接：[如何把数组对象相同的key值合并，并且把对应的id放到一个数组](https://segmentfault.com/q/1010000009821632)
