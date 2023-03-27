---
title: JavaScript字符串截取指定字节
date: 2021-11-09 12:00:00
tags:
---

## 函数实现

```javascript
function substringBytes(string, len) {
    if ((!string && typeof(string) !== 'undefined')) {
      return '';
    }
    let num = 0;
    let value = '';
    for (var i = 0,lens = string.length; i < lens; i++) {
        num += ((string.charCodeAt(i) > 255) ? 2 : 1);
        if (num > len) {
            break;
        } else {
            value = string.substring(0, i + 1);
        }
    }
    return value;
}

substringBytes('测试字符串截取', 5); // 测试
```
