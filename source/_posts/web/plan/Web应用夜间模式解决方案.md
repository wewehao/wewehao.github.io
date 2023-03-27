---
title: Web应用夜间模式解决方案
date: 2020-06-20 20:11:13
tags:
---

## 安装依赖

`npm i postcss-nested postcss-theme-colors precss --save-dev`

## 根目录添加 `postcss.config.js`

```js
'use strict';
const base64 = require('base64-img');

const colors = {
  363636: '#363636!important',
  CCCCCC: '#CCCCCC!important',
  '5A5A5A': '#5A5A5A!important',
  '8E8E93': '#8E8E93!important',
  181818: '#181818!important',
  DFDFDF: '#DFDFDF!important',
  BBBBBB: '#BBBBBB!important',
  222222: '#222222!important',
  D4D4D4: '#D4D4D4!important',
  '01A0BA': '#01A0BA!important',
  EB9B01: '#EB9B01!important',
  '00F8FF': '#00F8FF!important',
  D8BF97: '#D8BF97!important',
  837153: '#837153!important',
  FFFFFF: '#FFFFFF!important',
  '1D1E1F': '#1D1E1F!important',
  FF6262: '#FF6262!important',
  DCDCDC: '#DCDCDC!important',
  '86B7FF': '#86B7FF!important',
  E2AA78: '#E2AA78!important',
  '01A0EB': '#01A0EB!important',
  F8B83C: '#F8B83C!important',
  A0A0A0: '#A0A0A0!important',
  '386AF6': '#386AF6!important',
  '01A0EA': '#01A0EA!important',
  '00EFF5': '#00EFF5!important',
  F7F8F9: '#F7F8F9!important',
  EFEFEF: '#EFEFEF!important',
  979797: '#979797!important',
  F1F1F1: '#F1F1F1!important',
  333333: '#333333!important',
  '3E3E3E': '#3E3E3E!important',

  // 图片
  'LIGHT-BI-RECENT-LIVE-ICON': `url(${base64.base64Sync('./src/assets/images/light/recent_live_icon.png')})`,
  'DARK-BI-RECENT-LIVE-ICON': `url(${base64.base64Sync('./src/assets/images/dark/recent_live_icon.png')})`
};

const groups = {
  'FC-BOOK-MANAGE-86B7FF': [ '86B7FF', 'E2AA78' ],
  'FC-363636': [ '363636', 'CCCCCC' ],
  'BC-BOOK-CONFIRM-01A0BA': [ '01A0BA', 'E2AA78' ],
  'BC-386AF6': [ '386AF6', 'F8B83C' ],
  'BG-BODY': [ 'FFFFFF', '1D1E1F' ],
};

module.exports = {
  plugins: {
    autoprefixer: {},
    'postcss-theme-colors': {
      colors,
      groups,
      function: 'theme',
      darkThemeSelector: '#app.dark-theme', // 给id为app的节点
      nestingPlugin: 'nested',
    },
    'postcss-nested': {},
    precss: {},
  },
};

```

## 使用postcss
```css
.app {
    color: theme(FC-BBBBBB);
}
```
