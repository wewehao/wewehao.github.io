---
title: flutter实现Container渐变Border
date: 2023-03-22 19:12:00
tags:
---

## 代码实现

让 Container 的边框渐变而不是整个背景，可以将 BoxDecoration 中的 gradient 属性设置为空，并使用 BoxDecoration 中的 border 属性来创建渐变边框。

具体实现方法如下：

```dart
import 'package:flutter/material.dart';

class GradientBorderPainter extends CustomPainter {
  final double strokeWidth;
  final BorderRadiusGeometry radius;
  final Gradient gradient;

  GradientBorderPainter({
    required this.strokeWidth,
    required this.radius,
    required this.gradient,
  });

  @override
  void paint(Canvas canvas, Size size) {
    final rect = Rect.fromLTWH(0, 0, size.width, size.height);
    final paint = Paint()
      ..shader = gradient.createShader(rect)
      ..strokeWidth = strokeWidth
      ..style = PaintingStyle.stroke;

    final path = Path.combine(
        PathOperation.difference,
        Path()..addRRect(radius.resolve(TextDirection.ltr).toRRect(rect)),
        Path()..addRRect(radius.resolve(TextDirection.ltr).toRRect(rect.deflate(strokeWidth / 2)))
    );

    canvas.drawPath(path, paint);
  }

  @override
  bool shouldRepaint(CustomPainter oldDelegate) => false;
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Center(
        child: ClipRRect(
          borderRadius: BorderRadius.circular(16),
          child: Container(
            width: 200,
            height: 200,
            decoration: BoxDecoration(
              border: Border.all(
                width: 4,
                color: Colors.transparent,
              ),
            ),
            child: SizedBox.expand(
              child: CustomPaint(
                painter: GradientBorderPainter(
                  strokeWidth: 4,
                  radius: BorderRadius.circular(16),
                  gradient: LinearGradient(
                    begin: Alignment.centerLeft,
                    end: Alignment.centerRight,
                    colors: [
                      Colors.red,
                      Colors.blue,
                    ],
                  ),
                ),
              ),
            ),
          ),
        ),
      ),
    );
  }
}
```