---
title: JavaScript的事件冒泡和事件捕获
date: 2021-10-09 12:10:53
tags:
---

## 事件类型

鼠标事件

键盘事件

框架事件 onerror onresize onscroll等

表单事件事件 onblur onfocus等

剪贴板事件 oncopy oncut onpaste

打印事件 onafterprint onbeforeprint

拖动事件 ondrag ondragenter等

media事件 onplay onpause

动画事件 animationend

UI事件 load 、unload、error、select、resize、scroll

## 事件的三种模型

1、原始事件模型

原始事件模型中，事件发生后没有传播的概念，没有事件流。事件发生，立即处理。

监听函数只是元素的一个属性值，通过指定元素的属性值来绑定监听器。

```
HTML: <input id=”btn” onclick=”func()” />
js : document.getElementsById(‘btn’).onclick = func
```

优点是兼容性好。

缺点：

- 相同事件的监听函数只能绑定一个，后绑定的会覆盖掉前面的。

- 无法通过事件的冒泡、委托等机制等。

2、IE事件模型

IE是将event对象在处理函数中设为window的属性，一旦函数执行结束，便被置为null
了。

IE的事件模型只有两步，先执行元素的监听函数，然后事件沿着父节点一直冒泡到document。

- IE中不支持事件捕获，只有事件冒泡。

- 使用attachEvent()方法时候，事件处理程序会在全局作用域中运行，因此this等于window。

- 添加多个事件的时候，触发的顺序是：后添加的先执行，这点和DOM方法不同。

3、DOM2事件模型

W3C制定的事件模型中，一次事件的发生包含三个过程：

- 事件捕获阶段。事件被从document一直向下传播到目标元素,在这过程中依次检查经过的节点是否注册了该事件的监听函数，若有则执行。
- 事件处理阶段。事件到达目标元素,执行目标元素的事件处理函数.
- 事件冒泡阶段。事件从目标元素上升一直到达document，同样依次检查经过的节点是否注册
了该事件的监听函数，有则执行。

注意：所有的事件类型都会经历事件捕获阶段，但是只有部分事件会经历事件冒泡阶段,例如
submit事件就不会被冒泡。为了最大程度兼容各种浏览器，一般都是将事件处理函程序添加到事件流的冒泡阶段。

绑定解除监听函数的方法：

addEventListener("eventType","handler","true|false");其中eventType指事件类型，注意
不要加‘on’前缀，与IE下不同。

第二个参数是处理函数，第三个参数是可选参数。

监听器的解除也类似：removeEventListener("eventType","handler", {});

兼容IE和现代浏览器的事件注册监听写法：

```
let a = document.getElementById('XXX');

if(a.attachEvent) {
    a.attachEvent('onclick', func);
} else { // IE9以上和主流浏览器
    a.addEventListener('click', func, {});
}
```

## 事件的捕获-冒泡机制

DOM2标准中，一次事件的完整过程包括三步：捕获→执行目标元素的监听函数→冒泡，在捕获和冒泡阶段，会依次检查途径的每个节点，如果该节点注册了相应的监听函数，则执行监听函数。

如果不想让事件向上冒泡，可以在监听函数中调用event.stopPropagation()来完成。

preventDefault可以用来取消事件的默认行为，比如点击a标签默认跳转链接，点击button默认提交。

stopPropagation用来取消事件进一步捕获或者冒泡。

options 可选:

- capture:  Boolean，表示 listener 会在该类型的事件捕获阶段传播到该 EventTarget 时触发。
- once:  Boolean，表示 listener 在添加之后最多只调用一次。如果是 true， listener 会在其被调用之后自动移除。
- passive: Boolean，设置为true时，表示 listener 永远不会调用 preventDefault()。如果 listener 仍然调用了这个函数，客户端将会忽略它并抛出一个控制台警告。查看 使用 passive 改善的滚屏性能 了解更多.
- signal：AbortSignal，该 AbortSignal 的 abort() 方法被调用时，监听器会被移除。
- mozSystemGroup: 只能在 XBL 或者是 Firefox' chrome 使用，这是个 Boolean，表示 listener 被添加到 system group。
