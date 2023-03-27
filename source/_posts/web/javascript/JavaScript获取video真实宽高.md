---
title: JavaScript获取video真实宽高
date: 2021-11-17 16:36:10
tags:
---


## 代码

```javascript
const video = document.querySelector('video');

video.addEventListener('canplay', function () {
  console.log(this.videoWidth, this.videoHeight);
});
```

## 相关

1、canplay 事件，视频达到可以播放时触发；

2、videoWidth 和 videoHeight 属性为视频真实宽高，这两个属性为只读属性，赋值不会生效；

3、width 和 height 属性为视频在页面上显示的尺寸，可以在元素设置或JS赋值；

4、width 和 height 属性优先级低于样式，同时使用样式和属性设置宽高，最后生效的是样式；