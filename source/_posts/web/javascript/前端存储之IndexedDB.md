---
title: 前端存储之IndexedDB
date: 2021-09-10 20:10:27
tags:
---

## 背景

> IndexedDB是一种低级API，用于客户端存储大量结构化数据(包括, 文件/ blobs)。该API使用索引来实现对该数据的高性能搜索。虽然 Web Storage 对于存储较少量的数据很有用，但对于存储更大量的结构化数据来说，这种方法不太有用。IndexedDB提供了一个解决方案。 -- MDN

## 应用场景
> 针对客户端需要存储大量结构化数据，可能导致 Storage 超出5M存储限制的场景。

掘金文章：[新一代的前端存储方案--indexedDB](https://juejin.im/post/5b09a641f265da0dcd0b674f)
相关资料：[MDN-IndexedDB](https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)

由于原生API比较复杂，在对比过多个第三方库以后，这里强烈推荐使用 [localForage](https://github.com/xmoyking/localForage-cn)。

localForage 在不支持 IndexedDB 的浏览器中还会降级使用 WebSQL 或 localStorage，不必担心浏览器兼容性问题。
