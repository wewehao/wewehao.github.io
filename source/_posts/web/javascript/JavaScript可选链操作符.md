---
title: JavaScript可选链操作符
date: 2021-05-06 13:23:11
tags:
---

## 为什么要用可选链

在深层次嵌套的`if`判断时，普通的写法会使代码变得很不美观，看下面这段代码。

```javascript
const doubt = {
  id: 1,
  content: 'doubt',
  userId: 2333,
  userInfo: {
    id: 2333,
    username: '昵称',
    mobile: 12312341234,
    createdAt: 1598600417347,
    role: ['ROLE_ADMIN', 'ROLE_USER'],
  },
};

if (doubt && doubt.userInfo && doubt.userInfo.role && doubt.userInfo.role.includes('ROLE_ADMIN')) {
  // ...
}
```

当然也可以使用三元表达式，但依然会使代码变得难以阅读。

## 可选链操作符

> 可选链操作符( ?. )允许读取位于连接对象链深处的属性的值，而不必明确验证链中的每个引用是否有效。?. 操作符的功能类似于 . 链式操作符，不同之处在于，在引用为空(nullish ) (null 或者 undefined) 的情况下不会引起错误，该表达式短路返回值是 undefined。与函数调用一起使用时，如果给定的函数不存在，则返回 undefined。 --- MDN

下面是可选链调用的语法

```text
obj?.prop
obj?.[expr]
arr?.[index]
func?.(args)
```

在文章最开始那段代码段中的判断则可以用下面这种方式来表达

```javascript
if (doubt?.userInfo?.role.includes('ROLE_ADMIN')) {
  // ...
}
```

这样代码看起来就简洁多了，这里只介绍其中一种用法，还有类似下面这些

```text
a?.b?.c
a?.b?.[1]
a?.b?.func()
```

实际用起来可是真香～

## 在Vue中使用可选链

1、安装依赖

`npm install --save-dev @babel/plugin-proposal-optional-chaining`

2、在`babel.config.js`中添加代码解析插件

```javascript
module.exports = {
  presets: ['@vue/app'],
  plugins: [
    ["@babel/plugin-proposal-optional-chaining"],
  ],
};
```

## 相关资料

MDN文档：[https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/可选链](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/可选链)