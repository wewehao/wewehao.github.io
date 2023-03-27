---
title: ES6中的Map和Set
date: 2021-10-04 13:04:44
tags:
---

## Map

JavaScript的对象本质上是键值对的集合，但是传统上只能用字符串当作键。为了解决这个问题，ES6提供了Map数据结构。它类似于对象，也是键值对的集合，但是键的范围不限于字符串，各种类型的值（包括对象）都可以当作键。

```javascript
const m = new Map();
const o = {p: 'Hello World'};

m.set(o, '1');
m.get(o); // "1"

m.has(o); // true
m.delete(o); // true
m.has(o); // false
```

实例的属性和操作方法:

1、size

size返回Map结构的成员总数。

2、Map.prototype.set(key, value)

set方法返回的是当前的Map对象，因此可以采用链式写法。

3、Map.prototype.get(key)

get方法读取key对应的键值，如果找不到key，返回undefined。

4、Map.prototype.has(key)

has方法返回一个布尔值，表示某个键是否在当前 Map 对象之中。

5、Map.prototype.delete(key)

delete方法删除某个键，返回true。如果删除失败，返回false。

6、Map.prototype.clear()

clear方法清除所有成员，没有返回值。

7、Map.prototype.keys()

返回键名的遍历器。

8、Map.prototype.values()

返回键值的遍历器。

9、Map.prototype.entries()

返回所有成员的遍历器。

10、Map.prototype.forEach()

遍历 Map 的所有成员。

## Set

ES6提供了新的数据结构Set。它类似于数组，但是成员的值都是唯一的，没有重复的值。

```javascript
const s = new Set();

[2, 3, 2].forEach(x => s.add(x));

for (let i of s) {
  console.log(i);
}
// 2 3
```

实例的属性和操作方法:

1、Set.prototype.constructor

构造函数，默认就是Set函数。

2、Set.prototype.size

返回Set实例的成员总数。

3、Set.prototype.add(value)

添加某个值，返回 Set 结构本身。

4、Set.prototype.delete(value)

删除某个值，返回一个布尔值，表示删除是否成功。

5、Set.prototype.has(value)

返回一个布尔值，表示该值是否为Set的成员。

6、Set.prototype.clear()

清除所有成员，没有返回值。

7、Map.prototype.keys()

返回键名的遍历器。

8、Map.prototype.values()

返回键值的遍历器。

9、Map.prototype.entries()

返回所有成员的遍历器。

10、Map.prototype.forEach()

## 应用

1、Map转为数组

使用扩展运算符

```javascript
const myMap = new Map()
  .set(true, 7)
  .set({foo: 3}, ['abc']);
[...myMap];
// [ [ true, 7 ], [ { foo: 3 }, [ 'abc' ] ] ]
```

2、数组转为Map

将数组传入Map构造函数，就可以转为Map。

```javascript
new Map([
  [true, 7],
  [{foo: 3}, ['abc']]
])
// Map {
//   true => 7,
//   Object {foo: 3} => ['abc']
// }
```

3、Map转为对象

如果所有Map的键都是字符串，它可以无损地转为对象。

```javascript
function strMapToObj(strMap) {
  let obj = Object.create(null);
  for (let [k,v] of strMap) {
    obj[k] = v;
  }
  return obj;
}

const myMap = new Map()
  .set('yes', true)
  .set('no', false);
strMapToObj(myMap)
// { yes: true, no: false }
```

如果有非字符串的键名，那么这个键名会被转成字符串，再作为对象的键名。

4、对象转为Map

对象转为Map可以通过Object.entries()。

```javascript
let obj = {"a":1, "b":2};
let map = new Map(Object.entries(obj));
```

5、Map转为JSON

Map转为JSON要区分两种情况。一种情况是，Map的键名都是字符串，这时可以选择转为对象JSON。

```javascript
function strMapToJson(strMap) {
  return JSON.stringify(strMapToObj(strMap));
}

let myMap = new Map().set('yes', true).set('no', false);
strMapToJson(myMap)
// '{"yes":true,"no":false}'
```

另一种情况是，Map的键名有非字符串，这时可以选择转为数组JSON。

```javascript
function mapToArrayJson(map) {
  return JSON.stringify([...map]);
}

let myMap = new Map().set(true, 7).set({foo: 3}, ['abc']);
mapToArrayJson(myMap)
// '[[true,7],[{"foo":3},["abc"]]]'
```

6、JSON转为Map

JSON 转为 Map，正常情况下，所有键名都是字符串。

```javascript
function jsonToStrMap(jsonStr) {
  return objToStrMap(JSON.parse(jsonStr));
}

jsonToStrMap('{"yes": true, "no": false}')
// Map {'yes' => true, 'no' => false}
```

但是，有一种特殊情况，整个 JSON 就是一个数组，且每个数组成员本身，又是一个有两个成员的数组。这时，它可以一一对应地转为 Map。这往往是 Map 转为数组 JSON 的逆操作。

```javascript
function jsonToMap(jsonStr) {
  return new Map(JSON.parse(jsonStr));
}

jsonToMap('[[true,7],[{"foo":3},["abc"]]]')
// Map {true => 7, Object {foo: 3} => ['abc']}
```

---

> 原文链接：[Set和Map数据结构](https://es6.ruanyifeng.com/#docs/set-map)
