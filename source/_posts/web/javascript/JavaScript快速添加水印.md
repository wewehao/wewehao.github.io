---
title: JavaScript快速添加水印
date: 2021-06-03 20:43:39
tags:
---

## 需求背景

在浏览器中预览文件的时候，需要在页面上添加一层水印。

## 代码实现

在调研多个npm水印库以后，选择了watermark-dom。使用方法简单，文件依赖较小（18.5KB）。

```
npm i watermark-dom;

import watermark from "watermark-dom";

watermark.load({ watermark_txt: '文件预览中' });
```
