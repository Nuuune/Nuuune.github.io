---
layout: js学习-前言
title: JS学习(数据类型)
tags:
  - JS
categories:
  - 前端
date: 2016-12-15 22:45:14
---


初入前端，JS作为前端三剑客中的一员，是必须要了解且精通的。
学习参考书：[《JavaScript 标准参考教程（alpha）》](http://javascript.ruanyifeng.com/)，by 阮一峰

此《JS学习》系列为对参考书进行学习的日志，作为本人的*学习笔记*
<!--more-->

---

## 引导
* [六种数据类型](#ar1)
* [判断数据类型的方法](#ar2)
* [数值类型(Number)](#ar3)

<a name="ar1" />
### 六种数据类型 
* 数值（number）：整数和小数（比如1和3.14）
* 字符串（string）：文本（比如Hello World）。
* 布尔值（boolean）：表示真伪的两个特殊值，即true（真）和false（假）
* undefined：表示“未定义”或不存在，即由于目前没有定义，所以此处暂时没有任何值
* null：表示空值，即此处的值为空。
* 对象（object）：各种值组成的集合。

*对象又分为`object`(狭义对象) `array`(数组) `function`(方法)*

<a name="ar2" />
### 判断数据类型的方法 
#### (1) typeof 运算符
```javascript
typeof 123 // "number"
typeof '123' // "string"
typeof false // "boolean"

function f() {}
typeof f
// "function"

typeof undefined // "undefined"

typeof null // "object" ---> 对于null来说是历史原因造成的，它并不是对象类型

typeof window // "object"
typeof {} // "object"
typeof [] // "object"

```
从上面代码可以发现，`typeof`对于基本数据类型判断还是没问题的
但是对于对象(狭义)与数组还是不能很好区分所以可以采用接下来的方法

#### (2) instanceof 运算符
```javascript
var o = {};
var a = [];

o instanceof Array // false
a instanceof Array // true

```

<a name="ar3" />
### 数值(Number)
> JavaScript 语言的底层根本没有整数，所有数字都是小数（64位浮点数）。
> 由于浮点数不是精确的值，所以涉及小数的比较和运算要特别小心。

```javascript
0.1 + 0.2 === 0.3
// false

0.3 / 0.1
// 2.9999999999999996

(0.3 - 0.2) === (0.2 - 0.1)
// false

```

#### 特殊数值
* 正零和负零
* NaN (非数字)
* Infinity (无穷)

__正零__ 和 __负零__ 在绝大多数场合和零是没有区别的
只有当其作为分母时会分别产生`+Infinity` `-Infinity`

__NaN__ 主要出现在将字符串解析成数字出错的场合，还有一些函数运算上
`0 / 0` 也会出现
它不是一种独立数据类型，只是 __Number__ 数据类型中的特殊值
判断一个变量是否为此值可以采取 `isNaN()` 方法 返回一个 `boolean` 值

__Infinity__ 用来表示正的数值太大或负的数值太小 还一种方式 *非0数除以0*	


最后对于判断是否为特殊值可以采用 `isFinite()` 方法 为特殊值会返回 `false` 

