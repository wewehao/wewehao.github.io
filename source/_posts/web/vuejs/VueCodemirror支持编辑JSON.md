---
title: VueCodemirror支持编辑JSON
date: 2022-11-28 18:51:17
tags:
---

## 介绍

vue-codemirror是一款优秀的编辑器。支持很多种格式的语言编辑，如vue、js、css等。

## 如何扩展到json

1、安装依赖 `npm install vue-codemirror --save`

2、在main.js中引入

```
import { codemirror } from 'vue-codemirror'
import 'codemirror/lib/codemirror.css'

Vue.use(VueCodemirror)
```

3、编辑公共组件 code-editor.vue

```vue
<template>
  <codemirror class="code-editor-wrapper" :options="cmOptions" v-bind="$attrs" v-on="$listeners" />
</template>

<script>
import { codemirror } from "vue-codemirror";

import "codemirror/theme/blackboard.css";
import "codemirror/theme/eclipse.css";
import "codemirror/mode/javascript/javascript.js";
import "codemirror/mode/xml/xml.js";
import "codemirror/mode/htmlmixed/htmlmixed.js";
import "codemirror/mode/css/css.js";
import "codemirror/mode/yaml/yaml.js";
import "codemirror/mode/sql/sql.js";
import "codemirror/mode/python/python.js";
import "codemirror/mode/markdown/markdown.js";
import "codemirror/addon/hint/show-hint.css";
import "codemirror/addon/hint/show-hint.js";
import "codemirror/addon/hint/javascript-hint.js";
import "codemirror/addon/hint/xml-hint.js";
import "codemirror/addon/hint/css-hint.js";
import "codemirror/addon/hint/html-hint.js";
import "codemirror/addon/hint/sql-hint.js";
import "codemirror/addon/hint/anyword-hint.js";
import "codemirror/addon/lint/lint.css";
import "codemirror/addon/lint/lint.js";
import "codemirror/addon/lint/json-lint";

require("script-loader!jsonlint");
import "codemirror/addon/lint/css-lint.js";
import "codemirror/addon/lint/javascript-lint.js";
import "codemirror/addon/fold/foldcode.js";
import "codemirror/addon/fold/foldgutter.js";
import "codemirror/addon/fold/foldgutter.css";
import "codemirror/addon/fold/brace-fold.js";
import "codemirror/addon/fold/xml-fold.js";
import "codemirror/addon/fold/comment-fold.js";
import "codemirror/addon/fold/markdown-fold.js";
import "codemirror/addon/fold/indent-fold.js";
import "codemirror/addon/edit/closebrackets.js";
import "codemirror/addon/edit/closetag.js";
import "codemirror/addon/edit/matchtags.js";
import "codemirror/addon/edit/matchbrackets.js";
import "codemirror/addon/selection/active-line.js";
import "codemirror/addon/search/jump-to-line.js";
import "codemirror/addon/dialog/dialog.js";
import "codemirror/addon/dialog/dialog.css";
import "codemirror/addon/search/searchcursor.js";
import "codemirror/addon/search/search.js";
import "codemirror/addon/display/autorefresh.js";
import "codemirror/addon/selection/mark-selection.js";
import "codemirror/addon/search/match-highlighter.js";
import "codemirror/mode/vue/vue.js";
import "codemirror/theme/monokai.css";
import "codemirror/addon/scroll/annotatescrollbar.js";
import "codemirror/addon/search/matchesonscrollbar.js";
import "codemirror/mode/clike/clike.js";
import "codemirror/addon/comment/comment.js";
import "codemirror/keymap/sublime.js";

export default {
  name: "CodeEditor",
  components: {
    codemirror,
  },
  inheritAttrs: false,
  props: {
    options: Object,
    mode: String,
  },
  computed: {
    cmOptions() {
      return {
        tabSize: 2,
        line: true,
        mode: this.mode,
        lineWrapping: false,
        lineNumbers: true,
        autofocus: false,
        smartIndent: false,
        autocorrect: true,
        spellcheck: true,
        lint: true,
        foldGutter: true,
        autoCloseBrackets: true,
        autoCloseTags: true,
        matchTags: { bothTags: true },
        matchBrackets: true,
        styleActiveLine: false,
        autoRefresh: true,
        highlightSelectionMatches: {
          minChars: 2,
          style: "matchhighlight",
          showToken: true,
        },
        styleSelectedText: true,
        enableAutoFormatJson: true,
        defaultJsonIndentation: 2,
        ...this.options,
      };
    },
  },
};
</script>

<style lang="scss" scoped>
.code-editor-wrapper {
  height: 100%;
  text-align: left;
  overflow: auto;
  border: 1px solid #ccc;

  ::v-deep .CodeMirror {
    height: 100%;
  }
}
</style>
```

4、在其他组件中使用

```
<code-editor v-model="jsonEg" mode="application/json" />

import CodeEditor from './code-editor.vue';
```

## 资源

文档：https://github.surmon.me/vue-codemirror/
