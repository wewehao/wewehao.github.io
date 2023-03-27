---
title: JavaScript的基本类型和引用类型
date: 2021-11-21 11:28:29
tags:
---

## JS数据类型

一个变量可以存放两种类型的值，基本类型的值和引用类型的值。

## 基本类型

> 基本数据类型的值是按值访问

6种基本数据类型：Undefined、Null、Boolean、Number、String、Symbol(ES6 唯一值)。

1、栈内存中包括了变量的标识符和变量的值。

## 引用类型

> 引用类型的值是按引用访问

引用类型统称为对象类型。细分有 Object类型、Array类型、Date类型、RegExp类型、Function类型等。

1、栈内存中保存了变量标识符和指向堆内存中该对象的指针。

2、堆内存中保存了对象的内容。

## 判断数据类型

1、typeof

常被用来检测一个变量是不是最基本的数据类型。

返回一个表示数据类型的字符串，不能判断array和null，除function以外的对象都会被识别成object，这时就需要instanceof来进行判断。

```javascript
let a;
typeof a;    // undefined

a = null;
typeof a;    // object

a = true;
typeof a;    // boolean

a = 666;
typeof a;    // number

a = "hello";
typeof a;    // string

a = Symbol();
typeof a;    // symbol

a = function(){}
typeof a;    // function

a = [];
typeof a;    // object
a = {};
typeof a;    // object
a = /aaa/g;
typeof a;    // object
```

2、instanceof

instanceof运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。简单来说就是 instanceof 是用来判断 A 是否为 B 的实例，表达式为

> A (object) instanceof B (constructor)

如果A是B的实例，则返回true,否则返回false。

```javascript
({}) instanceof Object; // true
([]) instanceof Array; // true
(/aa/g) instanceof RegExp; // true
(function(){}) instanceof Function; // true
```

这种是否处于原型链上的判断方法不严谨。

- instanceof 方法判断的是是否处于原型链上，而不是是不是处于原型链最后一位，所以会出现下面这种情况：

```javascript
let arr = [1, 2, 3];
console.log(arr instanceof Array); // true
console.log(arr instanceof Object); // true

function fn(){}
console.log(fn instanceof Function); // true
console.log(fn instanceof Object); // true
```

- 无法判断字面量方式创建的基本数据类型。

对于基本数据类型来说，字面量方式创建出来的结果和实例方式创建的是有一定区别的：

```javascript
console.log(1 instanceof Number)//false
console.log(new Number(1) instanceof Number)//true
```

- 无法检测 null 和 undefined。

对于特殊的数据类型 null 和 undefined，他们的所属类是 Null 和 Undefined，但是浏览器把这两个类保护起来了，不允许我们在外面访问使用。

3、constructor

与instanceof非常相似，但是可以处理基本数据类型的检测。

```javascript
let aa=[1];
console.log(aa.constructor === Array); // true
console.log(aa.constructor === RegExp); // false
console.log((1).constructor === Number); // true

let reg=/^$/;
console.log(reg.constructor === RegExp); // true
console.log(reg.constructor === Object); // false
```

- 无法检测 null 和 undefined

null 和 undefined 是无效的对象，不存在constructor。

- 检测出来的结果不准确

把类的原型进行重写，在重写的过程中很有可能出现把之前的constructor给覆盖了。

```javascript
function Fn(){}
Fn.prototype = new Array();

let f = new Fn;
console.log(f.constructor); // Array
```

4、Object.prototype.toString.call()

> 每个对象都有一个 toString() 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，toString() 方法被每个 Object 对象继承。如果此方法在自定义对象中未被覆盖，toString() 返回 "[object type]"，其中 type 是对象的类型。

这种判断方法最准确最常用。

```javascript
Object.prototype.toString.call('') ;   // [object String]
Object.prototype.toString.call(1) ;    // [object Number]
Object.prototype.toString.call(true) ; // [object Boolean]
Object.prototype.toString.call(undefined) ; // [object Undefined]
Object.prototype.toString.call(null) ; // [object Null]
Object.prototype.toString.call(new Function()) ; // [object Function]
Object.prototype.toString.call(new Date()) ; // [object Date]
Object.prototype.toString.call([]) ; // [object Array]
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call(new Error()) ; // [object Error]
Object.prototype.toString.call(document) ; // [object HTMLDocument]
Object.prototype.toString.call(window) ; //[object global] window是全局对象global的引用
```

---

> 原文链接：[JavaScript 数据类型与类型判断详解](https://mp.weixin.qq.com/s/4BwD64yx2bMng5nd9aDafw)
