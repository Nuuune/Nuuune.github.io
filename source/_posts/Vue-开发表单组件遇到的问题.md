---
title: Vue-开发表单组件遇到的问题
tags:
  - Vue
  - computed属性
  - data属性
categories:
  - 框架
date: 2018-01-24 00:48:50
---


在写表单组件时，想写一个从父元素传来表单所需的组件类型，
通过computed属性来初始化表单各子输入框的基本类型及数据

写的过程中出现了computed初始化表单数据并没有双向绑定的
问题，对此进行分析记录
<!--more-->


### 例子
```html
//简单demo,'co'是父组件传来的参数并计算得到的computed属性
<template>
  <h3 v-text="co.text"></h3>
  <input v-model="co.text" type="text"/>
  <button @click="show">show co</button>
</template>

//部分script
computed: {
  co () {
    return {
      text: "co"
    }
  }
},

methods: {
  show () {
    console.log(this.co.text)
  }
}
```

效果如下：
![效果图 1.1](/imgs/0123.gif)

__ 分析 __
从上图可以看出，初始渲染是没有问题的，但是当改变输入框的值时， ` this.co.text ` 是跟着改变了，但是却没有在` <h3> `标签上渲染出来

查询官网，推荐使用data对象，因为在Vue实例化时，会对data对象进行setter/getter的转换，而computed属性不会，只有转换为setter/getter才能被watch观察，更新，重新渲染，所以使用computed属性进行绑定是不会达到响应式的
具体可以参考官方文档 [Vue 深入响应式原理](https://cn.vuejs.org/v2/guide/reactivity.html#%E5%A6%82%E4%BD%95%E8%BF%BD%E8%B8%AA%E5%8F%98%E5%8C%96)

__ 解决方案 __
将computed属性的对象地址，赋给data对象的属性中

```html
//改为直接从data中拿值渲染，到达响应
<template>
  <h3 v-text="dataCo.text"></h3>
  <input v-model="dataCo.text" type="text"/>
  <button @click="show">show co</button>
</template>

//部分script

+data () {
+  return {
+    dataCo: null
+  }
+},

computed: {
  co () {
    return {
      text: "co"
    }
  }
},

methods: {
  show () {
    console.log(this.co.text)
  }
},
//需要在数据加载完后赋值
+created () {
+  this.dataCo = this.co;
+}
```
_ 注意: _ 通过此操作之后，`co` 与 `dataCo`是同步一起了，因为它们都是指向了同一个对象的地址 
