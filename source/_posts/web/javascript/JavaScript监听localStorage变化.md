---
title: JavaScript监听localStorage变化
date: 2022-09-20 20:00:21
tags:
---

> 当同源页面的某个页面修改了localStorage，其余的同源页面只要注册了storage事件，就会触发 - HTML5移动Web开发指南

## Storage事件

在某些复杂情况下，如果多个页面都需要访问本地存储的数据，就需要在存储区域的内容发生改变时，能够通知相关的页面。

Web Storage API内建了一套事件通知机制，当存储区域的内容发生改变（包括增加、修改、删除数据）时，就会自动触发`storage`事件。

- 同一浏览器打开了两个同源页面
- 其中一个网页修改了`localStorage`
- 另一网页注册了`storage`事件

满足以上条件，就可以在A页面监听到B页面的修改了。

```js
window.addEventListener("storage", function (e) {
 conosle.log(e.newValue);
});
```