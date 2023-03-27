---
title: flutter预加载图片
date: 2023-03-23 10:00:00
tags:
---

## 需求场景

在输入框发送按钮可用状态、禁用状态使用两张不同路径的ICON时，切换状态可能导致图片有一瞬间的加载状态，影响用户体验。解决办法就是预先加载后置状态的图片。

## 代码实现

```dart
@override
void didChangeDependencies() {
  precacheImage(
    const AssetImage('images/submit_active_icon.png'),
    context,
  );
  super.didChangeDependencies();
}
```
