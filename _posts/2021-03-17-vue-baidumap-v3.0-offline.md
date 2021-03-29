---
layout: post
title: Vue.js结合百度地图3.0离线开发
date: 2021-03-17
category: vue,solution
comments: true
excerpt: vue2结合百度地图v3.0在内网环境下做开发。需要修改百度地图js文件，需要下载地图离线瓦片。
tags: [vue,solution]
---

vue+百度地图api如何在内网环境中开发呢？大致思路就是把所有资源都下载到本地，通过加载本地资源来展示，前期准备：
- 申请百度地图ak
- 下载地图离线瓦片

下面介绍具体步骤

### 申请百度地图ak
[申请地址](http://lbsyun.baidu.com/index.php?title=%E9%A6%96%E9%A1%B5)
账号密码登陆后在这个页面点击控制台
![image](/assets/img/baidumap1.png)
然后在左侧菜单栏上点击我的应用并`创建应用`，创建完成后会生成一个**访问应用(ak)**
![image](/assets/img/baidumap2.png)
接下来修改百度地图的JS文件

### 修改百度地图v3.0 js文件
1. 第一步在浏览起立输入`http://api.map.baidu.com/api?v=3.0&ak=这里填写刚才你申请的ak码`。
2. 回车后会在浏览器里看见一段代码:![image](/assets/img/baidumap3.png),找到这串代码中`src=""`中的部分，复制进浏览器地址栏并回车，这时候会看到一个压缩后的js把它另存为一下取名为baidu-api.js。
3. 用编辑器打开baidu-api.js，首先代码格式化一下。
4. 这一步就是重点了:
    - control+f 搜索如下代码`var c = (1e5 * Math.random()).toFixed(0)`![image](/assets/img/replace1.png)，找到类似的代码块（因为ak码不一样所以下载的js文件里面的变量名也不一样。）在红框处添加:
    
    ```javascript
    if (/^http/.test(a)) return //修改  屏蔽ak验证，若调用外部资源直接返回
    ```
    这段代码屏蔽ak验证。
    - control+f 搜索如下代码 `url.domain.main_domain_cdn.other[0]`![image](/assets/img/replace2.png)，找到类似的代码块，注释掉原来的添加以下代码：
    ```javascript
    D.pa = bmapcfg.home
    ```
    `bmapcfg.home`是下面一个js里面的变量。
    注意代码不要照抄，要根据你的js文件来复制，`D.pa`是我js里面的变量名这个不是固定的，不同ak码会生成不同的js文件。
    - control+f 搜索`&mod=`，定位到如下代码![image](/assets/img/replace3.png)，如截图所示修改修改原来的代码
    ```javascript
    if (a.length > 0) {
      for (i = 0; i < a.length; i++) {
        mf = bmapcfg.home + 'modules/' + a[i] + '.js'
        qa(mf)
      }
    } else {
      f.WK()
    }
    ``` 
    - control+f 搜索`panoramaflash`![image](/assets/img/replace4.png)，找到如截图所示代码块，这些就是我们离线要用到的js模块，需要我们手动下载下来，下载方法就是在浏览器里输入`http://api.map.baidu.com/getmodules?v=3.0&mod=key_value`这个key加value的形式，比如截图中第一个就是`http://api.map.baidu.com/getmodules?v=3.0&mod=map_0zz35j`，把这些文件全部下载下来后，在根目录下新建一个modules文件夹，然后把这些js文件通通放进去。到这里所有修改baidu-api.js的操作已经结束了。
5. 接下来我们在根目录下新建一个map_load.js作为入口文件，里面代码是这样的
    ```javascript
    var bmapcfg = {
    imgext: '.png', //瓦片图的后缀  根据需要修改，一般是 .png .jpg
    tiles_dir: '' //普通瓦片图的地址，为空默认在tiles/ 目录
    }

    var scripts = document.getElementsByTagName('script')
    var JS__FILE__ = scripts[scripts.length - 1].getAttribute('src') //获得当前js文件路径
    bmapcfg.home = JS__FILE__.substr(0, JS__FILE__.lastIndexOf('/') + 1) //地图API主目录
    ;(function () {
    window.BMap_loadScriptTime = new Date().getTime()
    //加载地图API主文件
    document.write('<script type="text/javascript" src="' + bmapcfg.home + 'baidu-api.js"></script>')
    })()
    ```
这个js文件中的代码就是定义了文件读取路径。
1. 下载地图瓦片，这是一个很烦的过程，需要付费购买瓦片下载程序，这个我就不多介绍了，附上本人现在使用的瓦片文件，一共1-17级，如果离线地图出现无法正常显示的正方形区域，说明缺少当前瓦片。[瓦片链接](https://pan.baidu.com/s/1QsssyzZRKN1ufiDofj5WMw)，密码: alo0

### 根目录结构
![image](/assets/img/replace5.png)
如果有问题可以留言讨论
