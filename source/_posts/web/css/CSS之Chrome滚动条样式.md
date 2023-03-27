---
title: CSS之Chrome滚动条样式
date: 2020-09-17 15:36:22
tags:
---

```css
::-webkit-scrollbar{
    width: 10px;
    height: 10px;
    background-color: #ccc;
}

::-webkit-scrollbar-track{
    box-shadow: inset 0 0 5px rgba(0,0,0,.3);
    background-color: #ccc;
}

::-webkit-scrollbar-thumb{
    border-radius: 10px;
    background-color: #bbb;
    box-shadow: inset 0 0 5px #000;
}

::-webkit-scrollbar-button{
    height: 10px;
    background-color: #333;
}
```