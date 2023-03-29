---
title: flutter实现滚动一定高度后出现AppBar
date: 2023-03-29 17:44:00
tags:
---

要实现这个效果，可以将 `SingleChildScrollView` 包装在一个 `Stack` 中，并将 `AppBar` 放在 `Stack` 的顶部。然后，使用 `ScrollController` 来监听滚动位置，并根据需要显示或隐藏 `AppBar`。

```dart
class MyHomePage extends StatefulWidget {
  const MyHomePage({Key? key}) : super(key: key);

  @override
  _MyHomePageState createState() => _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  final ScrollController _scrollController = ScrollController();
  bool _showAppBar = false;

  @override
  void initState() {
    super.initState();
    _scrollController.addListener(() {
      if (_scrollController.offset > 100 && !_showAppBar) {
        setState(() {
          _showAppBar = true;
        });
      } else if (_scrollController.offset <= 100 && _showAppBar) {
        setState(() {
          _showAppBar = false;
        });
      }
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      body: Stack(
        children: [
          SingleChildScrollView(
            controller: _scrollController,
            child: Column(
              children: [
                // 在此添加内容
              ],
            ),
          ),
          Positioned(
            top: 0,
            left: 0,
            right: 0,
            child: AnimatedOpacity(
              duration: Duration(milliseconds: 200),
              opacity: _showAppBar ? 1.0 : 0.0,
              child: AppBar(
                title: Text("My App"),
              ),
            ),
          ),
        ],
      ),
    );
  }

  @override
  void dispose() {
    _scrollController.dispose();
    super.dispose();
  }
}
```

在这个例子中，使用 `ScrollController` 来监听滚动位置，并根据需要显示或隐藏 `AppBar`。将 `SingleChildScrollView` 包装在一个 `Stack` 中，并将 `AppBar` 放在 `Stack` 的顶部。在 `AppBar` 上使用了 `AnimatedOpacity`，以便根据需要平滑地显示或隐藏 `AppBar`。
