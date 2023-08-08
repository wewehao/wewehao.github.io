---
title: 解决JS小数计算精度丢失问题
date: 2023-08-07 20:00:26
tags:
---

## 背景

通常在计算金额的时候容易出现精度丢失的问题，如下 `0.1 + 0.2` 得到的结果不等于 `0.3`

<div align=left>
<img src="/images/js_float_calc.png" width="33%" />
</div>

## 解决方案

为了解决这个问题，直接到npm仓库找到了计算精度丢失的三方库，对比了 `mathjs`、`number-precision`、`bignumber.js` 这几个库以后选择了 `number-precision`，因为它的体积最小，仅仅只有 `44.9K`。

具体使用方式

```js
NP.plus(num1, num2, num3, ...)   // 加法，num + num2 + num3，至少需要两个数字。
NP.minus(num1, num2, num3, ...)  // 减法，num1 - num2 - num3
NP.times(num1, num2, num3, ...)  // 乘法，num1 * num2 * num3
NP.divide(num1, num2, num3, ...) // 除法，num1 / num2 / num3
NP.round(num, ratio)  // 根据比率舍入数字
```

上面 `0.1 + 0.2` 的问题直接使用 `NP.plus(0.2, 0.3)` 就可以得到精确的结果了。

## 仓库地址

NPM: [number-precision](https://www.npmjs.com/package/number-precision)