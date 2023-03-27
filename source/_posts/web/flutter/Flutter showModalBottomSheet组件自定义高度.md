---
title: showModalBottomSheet组件自定义高度
date: 2023-03-23 19:12:00
tags:
---

showModalBottomSheet组件是从底部弹出的UI组件。其高度默认为屏幕高度的一半。

如果需要自定义高度，要设置`isScrollControlled`属性为true可滚动（默认是false，不可滚动）

```dart
showModalBottomSheet(
  backgroundColor: const Color.fromRGBO(244, 244, 244, 1),
  shape: const RoundedRectangleBorder(
    borderRadius: BorderRadius.vertical(top: Radius.circular(30)),
  ),
  clipBehavior: Clip.hardEdge,
  context: context,
  isScrollControlled: true,
  builder: (BuildContext context) {
    return const SheetTemplate();
  },
);
```

在SheetTemplate组件中设置高度

```dart
double screenHeight = MediaQuery.of(context).size.height;

Container(
  height: screenHeight * 0.8,
  color: Colors.black,
);
```