---
title: VueRouter微信中的跳转问题
date: 2020-03-20 22:23:04
tags:
---

## 背景

站点页面在IOS微信浏览器中存在一个问题：从 `/page`页面通过`route.push() `跳转到`/page/2`页面以后，页面显示正常，但是在分享以及复制浏览器地址时发现取到的地址仍然是`/page`。

## Vue-Router

> `Vue-Router`有`hash`、`history`两种模式，官网使用了`history`模式，其利用了H5 History API中`pushState()` 和 `replaceState()`方法，而IOS微信暂时没有支持这些新特性。这样会使IOS微信浏览器无法检测到HTML History PushState变化，就会导致在单页面应用中IOS微信浏览器只会记住第一次进来的地址。

## 解决方案

直接上代码。

```javascript
router.afterEach((to, from) => {
  const u = navigator.userAgent.toLowerCase();
  if (u.indexOf("like mac os x") < 0 || u.match(/MicroMessenger/i) != "micromessenger") {
    return;
  }
  if (to.path !== global.location.pathname) {
    location.assign(to.fullPath);
  }
});
```

上面的代码很简单，可以放置在Vue入口文件`main.js`的位置，简单的说：

1. 注册Vue-Router全局后置钩子。

2. 通过 User Agent 判断当前的环境是否是IOS微信。
3. 通过 `location.assign` 方法载入要加载的页面。

## 总结

`location.assign`重新加载了页面资源文件，显得不是很优雅，如果需要更优雅的实现方式，可能需要等微信支持H5相关的API了。
