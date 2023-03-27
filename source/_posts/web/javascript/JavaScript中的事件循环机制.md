---
title: JavaScript中的事件循环机制
date: 2021-05-19 13:27:15
tags:
---

## Event Loop
> Event Loop的事件分两种，宏任务(macro-task)和微任务(micro-task)

- 宏任务：整体代码script，setTimeout，setInterval，setImmediate，I/O ，UI rendering
- 微任务：Promise.then，process.nextTick，MutationObserver

- 先执行宏任务，然后执行微任务。

> 任务有同步任务和异步任务，同步任务进入主线程，异步任务进入事件表并注册函数，异步事件完成后，会将回调函数放入事件队列中(宏任务和微任务是不同的队列)，同步任务执行完成后，会从事件队列中读取事件放入主线程执行，回调函数中可能还会包含不同的任务，因此会循环执行上述操作。

```js
console.log('1');

setTimeout(function () {
  console.log('2');
  process.nextTick(function () {
    console.log('3');
  })
  new Promise(function (resolve) {
    console.log('4');
    resolve();
  }).then(function () {
    console.log('5')
  })
})
process.nextTick(function () {
  console.log('6');
})
new Promise(function (resolve) {
  console.log('7');
  resolve();
}).then(function () {
  console.log('8')
})

setTimeout(function () {
  console.log('9');
  process.nextTick(function () {
    console.log('10');
  })
  new Promise(function (resolve) {
    console.log('11');
    resolve();
  }).then(function () {
    console.log('12')
  })
})
```

- JS 事件执行机制 什么是单线程->同一个时间只能做一件事
- 多线程要考虑线程之间的资源抢占，死锁，冲突之类一系列问题
- 单线程就意味着，所有任务需要排队，前一个任务结束，才会执行后一个任务。如果前一个任务耗时很长，后一个任务就不得不一直等着。
- 所有任务可以分成两种，同步任务和异步任务
- 同步任务指的是，在主线程上排队执行的任务，只有前一个任务执行完毕，才能执行后一个任务；
- 异步任务指的是，不进入主线程、而进入”任务队列”（task queue）的任务，只有”任务队列”通知主线程，某个异步任务可以执行了，该任务才会进入主线程执行。

- 异步执行的运行机制
  （1）所有同步任务都在主线程上执行，形成一个执行栈（execution context stack）。
  （2）主线程之外，还存在一个"任务队列"（task queue）。只要异步任务有了运行结果，就在"任务队列"之中放置一个事件。
  （3）一旦"执行栈"中的所有同步任务执行完毕，系统就会读取"任务队列"，看看里面有哪些事件。那些对应的异步任务，于是结束等待状态，进入执行栈，开始执行。
  （4）主线程不断重复上面的第三步。

- 上面代码的执行顺序：
  1. 整体script作为第一个宏任务进入主线程。遇到console，输出1。
  2. 碰到setTimeout作为宏任务，放到宏任务队列中。记为 s1。
  3. 碰到process作为微任务，进入微任务队列中。记为 p1。
  4. 碰到new Promise立即执行，输出7。then被分发到微任务队列中。记为 t1。
  5. 碰到setTimeout作为宏任务，放到宏任务队列中。记为 s2。

- 至此，第一轮事件循环宏任务结束时。任务队列的情况。
  1. 宏任务：s1， s2。
  2. 微任务：p1, t1。

- 发现了两个微任务，依次执行p1, t1。输出 6，8。
- 此时第一轮事件循环结束。剩下两个宏任务 s1，s2。

- 开始执行第二轮任务。宏任务。
  1. 执行s1。
  2. 碰到console立即执行，输出 2。
  3. 碰到process作为微任务，放到微任务队列中。记为 p2。
  4. 碰到new Promise立即执行，输出 4。then被分发到微任务队列中。记为 t2。

- 第二轮宏任务执行结束。发现微任务 p2, t2。依次输出 3，5。
- 第二轮事件循环结束。剩下宏任务 s2。

- 开始执行第三轮任务。宏任务。
  1. 执行s2。
  2. 碰到console立即执行，输出 9。
  3. 碰到process作为微任务，放到微任务队列中。记为 p3。
  4. 碰到new Promise立即执行，输出 11。then被分发到微任务队列中。记为 t3。

- 第三轮宏任务执行结束。发现微任务 p3，t3。依次输出 10, 12。
- 主线程任务队列已经执行完毕。事件循环结束。

- 输出依次为：1，7，6，8，2，4，3，5，9，11，10，12。
