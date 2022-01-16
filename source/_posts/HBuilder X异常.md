---
title: HBuilder异常
date: 2020-10-09 11:06:37
tags: 软件
categories: 踩坑
---


## HBuilder X出现404 Page Not Found

![](https://s1.ax1x.com/2020/10/09/0DiCes.png)

#### 分析原因：

主要问题就是打开浏览器跳转时出现异常，显示404包括内置浏览器都是如此！

#### 解决方法：

1.找到你安装HBuilder X的文件夹\plugins\nodeserver

2.将里面的server.js更改成其它名字（这里不直接删除是为了防止以后有用，保险的做法）

3.新建server.js文件并且将下面的代码复制粘贴过去即可

4.保存让后重启HBuilder X就可以完美解决（这里可能需要等待一会，我的好像不是立马起效）

```js
var args = process.argv.splice(2)[0];
var express = require('express');
var app = express();
var argsjson =JSON.parse(args); 
var projects = argsjson.projects;
var port = argsjson.port;

projects.forEach(function (value,index,array) {
	app.use('/'+encodeURI(value.name),express.static(value.path+''));
});

app.get('*', function(req, res){
    res.sendFile( __dirname + "/" + "404.html" );
});

var server = app.listen(port, function () {
	console.log('server start at '+port);
})

```

