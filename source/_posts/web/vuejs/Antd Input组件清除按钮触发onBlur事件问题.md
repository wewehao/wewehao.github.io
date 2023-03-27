---
title: Antd Input组件清除按钮触发onBlur事件问题
date: 2022-12-21 15:41:53
tags:
---

## 问题

![image1](/images/ant_design_input_blur.png)

在使用ant-design组件库开发时，需求场景如下：

1、要在输入框中输入文字描述
2、输入框失去焦点自动调用接口更新文案
3、点击输入框后面的叉叉清除按钮清空当前输入框文案

在实际开发中表现如下：
点击叉叉按钮无法清空文案，并且调用了接口。

## 解决过程

1、经过排查，是因为点击叉叉触发了组件的onBlur事件。
2、然后在点击叉叉按钮的点击事件添加阻止冒泡。验证无效

## 解决方案

1、最后无奈去看了ant-design的源码，发现源码里有这么一行代码.

```javascript
<CloseCircleFilled
  onClick={handleReset}
  // Do not trigger onBlur when clear input
  // https://github.com/ant-design/ant-design/issues/31200
  onMouseDown={e => e.preventDefault()}
  className={classNames(
    {
      [`${className}-hidden`]: !needClear,
      [`${className}-has-suffix`]: !!suffix,
    },
    className,
  )}
  role="button"
/>
```

```javascript
onMouseDown={e => e.preventDefault()}
```

这个方法可真是太机智了。利用事件的执行顺序来解决这个问题。

```js
onMouseDown(最早) -> onBlur（onFocus） -> onClick -> onMouseUp
```
