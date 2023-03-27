---
title: Chrome扩展开发
date: 2023-02-15 12:00:00
tags:
---

## Chrome扩展简介

Chrome扩展（`Chrome Extension`）是一种用Web技术开发、用来增强Chrome浏览器功能的软件技术，其本质是由HTML、CSS、JS和图片等资源组成的一个`.crx`后缀的压缩包。

## 项目结构

项目根目录需要创建一个`manifest.json`文件，是Chrome扩展不可缺少的文件，用于编写所有扩展相关的配置。

其中描述了扩展的名称，版本，需要用到的资源文件等。这里简介其中最重要的两项配置，`popup`、`content-scripts`和`background`。

### Popup

```json
{
    "browser_action":
    {
        "default_icon": "img/icon.png",
        "default_popup": "popup.html"
    }
}
```

popup是点击扩展图标时打开的一个小窗口页面，当焦点离开时该页面就立即关闭，一般用来做登录蕾丝的临时性交互。

### Content Scripts


```json
{
  "name": "My extension",
  "content_scripts": [
    {
      "matches": ["http://www.google.com/*"],
      "css": ["mystyles.css"],
      "js": ["jquery.js", "myscript.js"]
    }
  ],
}
```

content-scripts是扩展向页面注入脚本的一种形式，通过使用标准DOM，可以获取浏览器所访问页面的详细信息，并且可以`修改`这些信息。包括且不限于以下操作：

从页面中找到文本形式的url，并将其转为可以点击跳转的超链接。
修改页面使用的字体。
修改页面文字颜色，大小，状态等等。

content-scripts也有一些限制：

不能访问Web页面中定义的函数和变量。
不能做跨域请求。
不能使用除了chrome.extension之外的所有chrome接口。

当然，这些限制是为了浏览器安全考虑。跨域请求在代码中可以通过扩展的`message`机制与所在的扩展通信，达到间接使用chrome接口的目的。

### Background

```json
{
  "name": "My extension",
  "background": {
    "scripts": ["background.js"]
  },
}
```

`background`是一个常驻在浏览器扩展进程中的HTML页面，在扩展的整个生命周期中都存在。通常把需要一直运行的代码放到background里面。并且可以跨域访问任何网站。


## 插件实现

接触到插件是因为公司需要做一个录屏的软件，开始研究这个Chrome扩展。下面是一个知乎自动点赞的简单实现，代码放在Github上了。

[点击跳转到知乎自动点赞代码仓库](https://github.com/wewehao/zhihu-auto-like)