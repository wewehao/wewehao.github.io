---
title: Vue虚拟列表
date: 2020-04-11 19:04:54
tags:
---

## 背景

在页面中显示多行或需要循环大量数据时，页面渲染速度很快变慢。

## 代码实践

1、安装依赖

`npm install vue-virtual-scroller`

2、在main.js中引入

```vue
import Vue from 'vue'
import VueVirtualScroller from 'vue-virtual-scroller'

Vue.use(VueVirtualScroller)
```

3、在vue组件中使用

```vue
<template>
  <RecycleScroller
    class="scroller"
    :items="list"
    :item-size="32"
    key-field="id"
    v-slot="{ item }"
  >
    <div class="user">
      {{ item.name }}
    </div>
  </RecycleScroller>
</template>

<script>
export default {
  props: {
    list: Array,
  },
}
</script>

<style scoped>
.scroller {
  height: 100%;
}

.user {
  height: 32%;
  padding: 0 12px;
  display: flex;
  align-items: center;
}
</style>
```

