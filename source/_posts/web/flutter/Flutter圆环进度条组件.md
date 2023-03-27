---
title: Flutter圆环进度条组件
date: 2023-03-25 11:12:00
tags:
---

记录一下最近开发中用到的第三方库，可以实现圆环进度条，完全满足需求。

![image1](/images/flutter_progress.png)

```dart
import 'package:percent_indicator/circular_percent_indicator.dart';

CircularPercentIndicator(
  radius: 258 / 2,
  lineWidth: 16,
  percent: 0.3,
  backgroundColor: const Color.fromRGBO(234, 234, 234, 1),
  linearGradient: const LinearGradient(
    colors: [
      Color.fromRGBO(137, 13, 237, 1),
      Color.fromRGBO(252, 0, 183, 1.0),
      Color.fromRGBO(219, 24, 166, 1),
    ],
    stops: [0.0, 0.5, 1.0],
    begin: Alignment.topLeft,
    end: Alignment.bottomRight,
  ),
  circularStrokeCap: CircularStrokeCap.round,
  center: Column(
    crossAxisAlignment: CrossAxisAlignment.center,
    mainAxisAlignment: MainAxisAlignment.center,
    children: [
      const Text(
        'progress',
        textAlign: TextAlign.center,
        softWrap: true,
        style: TextStyle(
          fontSize: 14,
          height: 16 / 14,
          color: Color.fromRGBO(0, 0, 0, 0.6),
        ),
      ),
      const SizedBox(height: 16),
      const Text(
        '25%',
        textAlign: TextAlign.center,
        softWrap: true,
        style: TextStyle(
          fontSize: 20,
          height: 28 / 20,
          color: Color.fromRGBO(0, 0, 0, 1),
          fontWeight: FontWeight.bold,
        ),
      ),
    ],
  ),
),
```