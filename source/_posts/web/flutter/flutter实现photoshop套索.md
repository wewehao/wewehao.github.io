---
title: flutter实现photoshop套索
date: 2023-08-23 20:00:00
tags:
---

## 背景

APP有编辑图片的需求，目前已经实现了基本的涂抹模式，需要进一步实现套索模式。就像下图中手指移动，松开手指的时候自动连接首尾，再对选中的区域进行操作。

<div align=left>
<img src="/images/flutter_photoshop_lasso.gif" />
</div>

## 思路

- 尝试寻找三方库
  - Google
  - Github
  - flutter三方库 [https://pub.dev/](https://pub.dev/)
  - ChatGPT

- Google搜索“实现photoshop套索” - 有一些实现思路

经过以上步骤，基本没有找到能用的三方库，只能看一下photoshop的实现思路，然后再APP中尝试实现该功能。

由于已经实现了画板，直接看了一下画板的插件文档，然后在画板添加一个功能按钮，表示套索模式。

然后看美图秀秀的UI，直观的看是从起点到终点有一条虚线

