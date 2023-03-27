---
title: JavaScript的new和箭头函数
date: 2021-07-04 12:02:09
tags:
---

## new运算符

new运算符会创建一个用户定义的对象类型的实例。

```javascript
function objectFactory(){
    let obj = new Object();
    let Constructor = [].shift.call(arguments);
    obj.__proto__ = Constructor.prototype;
    let ret = Constructor.apply(obj,arguments);
    return typeof ret === 'object' ? ret : obj;
}

function Child (name,age){
    this.name = name;
    this.age = age;
    this.habit = 'Games';
}

Child.prototype.strength = 60;
Child.prototype.sayYourName = function(){
    console.log('I am' + this.name);
};

let person = objectFactory(Child, 'Kevin', '10');
console.log(person.name);
console.log(person.habit);
console.log(person.strength);
console.log(person.age);
console.log(person.sayYourName())
```

new操作实质上是定义一个具有构造函数内置对象的实例，运行过程：

- 创建一个javascript空对象 {};
- 将要实例化对象的原形链指向该对象原形。
- 绑定该对象为this的指向
- 返回该对象。

## 箭头函数

> 箭头函数表达式的语法比函数表达式更简洁，并且没有自己的this，arguments，super或new.target。箭头函数表达式更适用于那些本来需要匿名函数的地方，并且它不能用作构造函数。

1、因为箭头函数没有function关键字所以箭头函数没有prototype。

> 箭头函数不会创建自己的this,它只会从自己的作用域链的上一层继承this。

2、通过call或apply调用，只能传递参数（不能绑定this---译者注），他们的第一个参数会被忽略。

3、箭头函数不能作为构造器，和new一起使用会抛出错误。

箭头函数不可以使用new实例化，因为箭头函数没有prototype也没有自己的this指向并且不可以使用arguments。
