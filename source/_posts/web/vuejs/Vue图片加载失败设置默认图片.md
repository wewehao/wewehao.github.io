---
title: Vue图片加载失败设置默认图片
date: 2020-04-10 22:24:33
tags:
---

## 直接上代码

- parameters.js

```js
export default {
  avatar: require("@/assets/images/default/avatar.png")
};
```

- defaultImg.ts

```js
import Vue from "vue";
import parameters from "parameters";

Vue.directive("default-img", {
  bind: function(el, binding) {
    el.onerror = function() {
      let arg = parameters[binding.arg];
      if (arg) {
        el.onerror = null;
        el.src = arg;
      }
    };
  }
});
```

## 使用方式

`<img v-default-img:avatar :src="Avatar" />`
