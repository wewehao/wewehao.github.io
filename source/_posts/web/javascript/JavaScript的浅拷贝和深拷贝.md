---
title: JavaScript的浅拷贝和深拷贝
date: 2021-12-19 12:51:46
tags:
---

## 对象浅拷贝

1、Object.assign()

```javascript
let obj1 = {a: 1, b: 2};
let obj2 = Object.assign({}, obj1);
```

2、解构赋值

```javascript
let obj1 = {a: 1, b: 2};
let obj2 = {...obj1};
```

## 对象深拷贝

1、JSON

```javascript
// 通过js的内置对象JSON来进行数组对象的深拷贝
function deepClone(obj) {
  let _obj = JSON.stringify(obj);
  let objClone = JSON.parse(_obj);
  return objClone;
}
```

这种方法有局限性，当值为undefined、function、symbol 会在转换过程中被忽略。

2、循环和递归实现数组或对象深拷贝

```javascript
function deepClone(obj, newObj) {
    newObj = newObj || {};
    for (let key in obj) {
        if (typeof obj[key] == 'object') {
            newObj[key] = (obj[key].constructor === Array) ? [] : {}
            deepClone(obj[key], newObj[key]);
        } else {
            newObj[key] = obj[key]
        }
    }
    return newObj;
}
```

3、数组浅拷贝

利用数组的 slice 方法或者 concat 方法

```javascript
let arr1 = ["1","2","3"];
let arr2 = arr1.slice(0);
arr2[0] = "4";
```
