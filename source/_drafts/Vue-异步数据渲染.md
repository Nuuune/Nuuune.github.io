---
title: Vue 异步数据渲染
tags:
 - Vue
 - 异步数据
categories:
 - 框架
---

使用 * Vue * 的时候遇到了异步数据渲染错误的问题。
__ 原因： __ 渲染前没有传入渲染所需参数所导致。

<!--more-->
---

### 1.例子

#### 父组件
```html
//用getData自身的数据模拟异步数据
<template>
  <div>
    <child :dataList="dataList"></child>
    <getData @upData="getChildData"></getData>
  </div>
</template>

//部分script
data () {
  return {
    dataList: null
  }
},

methods: {
  getChildData(val){
    this.dataList = val;
  }
}


```

#### child.vue
```html
<template>
  <h1>{{dataList[0].msg}}</h1>
</template>

//script
props: ['dataList']

```

#### getData.vue
```html
<template>
  <button @click="updata">获取数据</button>
</template>

//script
data () {
      return {
          asynData: [{msg:"1"},{msg:"2"}]
      }
},
methods: {
  updata(){
    console.log(this.asynData);
    this.$emit('upData', this.asynData)
  }
}

```

运行时，报错
>TypeError: Cannot read property '0' of null

### 2.解决

自己作demo时出现这类似的错误，当时用了最“笨”的方法，初始化参数(但是 ** 参数为对象 ** 时不是太理想)

后来查阅网上资料发现只需要对用异步数据进行一个 * v-if * 判断来加载组件即可

对于上面例子，只要进行父组件的修改

```html
<template>
  <div>
-    <child :dataList="dataList"></child>
+    <child v-if="dataList" :dataList="dataList"></child>
    <getData @upData="getChildData"></getData>
  </div>
</template>
```
