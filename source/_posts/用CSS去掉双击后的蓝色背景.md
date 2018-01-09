---
title: 用CSS去掉双击后的蓝色背景
tags:
  - CSS
categories:
  - 前端
date: 2018-01-06 20:51:39
---

```css
.elements {
  -moz-user-select: none;  /* firefox */
  -webkit-user-select: none;  /* webkit */
  -ms-user-select: none;  /* IE10+ */
  -khtml-user-select: none;  /* 早期浏览器 */
  user-select: none;
}
```
