---
title: node服务器接收前端参数的坑
tags:
  - node.js
categories:
  - 后端
date: 2018-02-20 23:29:56
---


在写后台时发现`req.body`取不到前端传过来的参数，后来发现是参数类型的问题，
特此记录
<!-- more -->

### 原因
开始用__ POSTMAN __ 插件时测试接口发现参数都没问题
`req.body` 能够取到传来参数值
而实际用到自己的前端项目上时发现后台取不到传来的参数了
前端采用的是__ axios.js __ 插件来进行HTTP请求的，
通过浏览器debug可以发现请求里参数全都在__ Request Payload __ 之下
所以通过`req.body`取不到了

### 解决
解决方法可以通过以下类似代码解决：
```javascript
// 请求处理
var reqBody = null;
var str = "";
req.on('data', function(chunk) {
  str += chunk;
});
req.on('end', function() {
  reqBody = JSON.parse(str);
  //省略
});  
```
这是由于
>Request Payload方式是以“流“”的方式出入到后台，需要监听data事件来获取完整的数据。

借鉴[此处](http://www.cnblogs.com/hsp-blog/p/5919877.html)
