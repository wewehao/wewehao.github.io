---
title: CSS盒模型
date: 2020-07-04 10:50:42
tags:
---

## 盒模型

> 当对一个文档进行布局（lay out）的时候，浏览器的渲染引擎会根据标准之一的 CSS 基础框盒模型（CSS basic box model），将所有元素表示为一个个矩形的盒子（box）。CSS 决定这些盒子的大小、位置以及属性（例如颜色、背景、边框尺寸…）。

> 每个盒子由四个部分（或称区域）组成，其效用由它们各自的边界（Edge）所定义（原文：defined by their respective edges，可能意指容纳、包含、限制等）。如图，与盒子的四个组成区域相对应，每个盒子有四个边界：内容边界 Content edge、内边距边界 Padding Edge、边框边界 Border Edge、外边框边界 Margin Edge。

目前CSS盒模型有两种，标准盒模型和IE盒模型。

1、标准盒模型包含 内容（content） 内边距（padding） 边框（border） 外边框（margin）。

2、IE盒模型content包含了padding 和  border。

## 宽度计算

1、标准盒子实际宽 width = content+margin+padding+border

2、IE盒子的实际宽 width = content+margin

## CSS3中的box-sizing

> box-sizing ： content-box || border-box || inherit;

1、当设置为box-sizing:content-box时，将采用标准盒模型计算

2、当设置为box-sizing:border-box时，将采用IE盒模型计算
