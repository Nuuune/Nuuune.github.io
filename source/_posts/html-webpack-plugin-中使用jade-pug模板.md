---
title: html-webpack-plugin 中使用jade/pug模板
tags:
  - webpack插件
  - jade
  - pug
categories:
  - 构建工具
date: 2018-02-09 15:46:03
---


关于 _ html-webpack-plugin _ 使用自定义模板的传参问题
<!-- more -->

### 起因
由于此插件的默认使用是_ .ejs _ 的模板引擎
所以它编译时针对此模板已将此插件的_ options _ 传入了模板
于是便可以向如下直接使用
```
<!-- default_index.ejs -->
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title><%= htmlWebpackPlugin.options.title %></title>
  </head>
  <body>
  </body>
</html>
```
其中_ htmlWebpackPlugin.options.title _ 就是传入的变量

而当使用其他模板时是获取不到这个变量的，那么应该如何实现？

### 解决
对于_ jade/pug _ 模板
由于网上搜索很少这方面解决方案
通过了解webpack文档
可以通过loader加载器对其进行传参
如下
``` javascript
// webpack.config.js

const HtmlWebpackPlugin_option = {
  // ··· 省略一些其他设置
  title = "标题"
}

// ···省略 ···
{
  test: /\.pug$/,
  use: [
        { loader: 'html-loader' },
        {
          loader: 'pug-html-loader',
          options: {
            data: {
              user: 'sss',
              htmlWebpackPlugin: {
                options: HtmlWebpackPlugin_option
              }
            }
          }
        }
      ]
}
```
上面是wepack的module部分配置
需要传入的参数通过_ options.data _ 来传入
如上传入了一个_ user _ 、_ htmlWebpackPlugin _ 两个变量
其中_ HtmlWebpackPlugin_option _ 是html-webpack-plugin的options对象

通过这样的配置就可以在_ jade/pug _ 模板中使用传入的参数了
如下

``` 
doctype html
html(lang="en")
  head
    title #{htmlWebpackPlugin.options.title}
    meta(charset="utf-8")
  body
    h1 #{user}
```
编译后
``` html
<!DOCTYPE html>
<html lang="en">
  <head>
    <title>标题</title>
    <meta charset="utf-8" />
  </head>
  <body>
    <h1>sss</h1>
  </body>
</html>
```
