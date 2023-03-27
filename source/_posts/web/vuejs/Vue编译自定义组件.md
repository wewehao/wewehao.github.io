---
title: Vue编译自定义组件
date: 2020-05-06 20:57:24
tags:
---

## 需求背景

最近在开发可配置平台时碰到了一些难以解决的问题。在从组件库拖拽组件到可视区时，怎么实现form表单表头筛选条件和表格的交互。这里涉及到了事件交互，在表头中需要传递参数到表格组件，然后表格开始调用接口查询数据，然后渲染到页面上。

这里的交互如果在组件代码中约定会很复杂，目前使用的技术方案是使用自定义组件实现表格，在表格注册事件到Vue事件总线中，然后当点击搜索按钮时获取form表单参数然后调用bus事件总线中的事件，达到和表格交互的目的。

## 代码实现

`vm.js`

```js
/**
 * 将字符串转换成代码对象
 * @param code 代码
 * @param value 默认值
 * @param params scoped变量，上下文变量，类似全局变量
 */
export function stringToCode(code, value, params) {
  const result = { value, error: null };
  try {
    result.value = new Function("context", `return ${code}`)(params) || value;
  } catch (e) {
    console.error("js脚本错误：", e);
    result.error = e;
  }
  return result;
}

/**
 * 执行一段字符串格式的函数
 */
export function runFnInVm(code, params, globalParams) {
  const NOOP = (args) => args;
  const result = stringToCode(code, NOOP, globalParams);
  const fn = result.value;
  result.value = params;
  if (result.error) {
    return result;
  }
  if (typeof fn !== "function") {
    console.error("非法的js脚本函数", fn);
    result.error = new Error("非法的js脚本函数");
    return result;
  }
  try {
    result.value = fn.call(fn, params);
  } catch (e) {
    console.error("js脚本执行错误：", e);
    result.error = e;
  }
  return result;
}

```

`mount-component.vue`

```vue
<script>
import { runFnInVm } from "./vm";

export default {
  name: "MountComponent",
  props: {
    template: {
      type: String,
      default: "<div></div>",
    },
    js: {
      type: String,
      default: `function code() {
  return {
  };
}`,
    },
    css: {
      type: String,
      default: "",
    },
    isEdit: {
      type: Boolean,
      default: false,
    },
  },
  data() {
    return {
      subCompErr: null,
    };
  },
  computed: {
    className() {
      // 生成唯一class，主要用于做scoped的样式
      const uid = Math.random().toString(36).slice(2);
      return `component-${uid}`;
    },
    scopedStyle() {
      if (this.css) {
        const scope = `.${this.className}`;
        const regex = /(^|\})\s*([^{]+)/g;
        // 为class加前缀，做类似scope的效果
        return this.css.trim().replace(regex, (m, g1, g2) => {
          return g1 ? `${g1} ${scope} ${g2}` : `${scope} ${g2}`;
        });
      }
      return "";
    },
    component() {
      const js = (this.js || "").trim();
      const result = runFnInVm(js, {});
      const component = result.value;
      if (result.error) {
        result.error = {
          msg: result.error.toString(),
          type: "js脚本错误",
        };
        result.value = { hasError: true };
        return result;
      }

      const template = (this.template || "").replace(/^ *< *template *>|<\/ *template *> *$/g, "").trim();

      // 注入template或render，设定template优先级高于render
      if (template) {
        component.template = template;
        component.render = undefined;
      } else if (!component.render) {
        component.template = "<p style='text-align:center'>未提供模板或render函数</p>";
      }

      // 注入mixins
      component.mixins = [
        {
          // 注入 beforeUpdate 钩子，用于子组件重绘时，清理父组件捕获的异常
          beforeUpdate: () => {
            this.subCompErr = null;
          },
        },
      ];

      // 通过computed注入一些变量，比如store中的一些字段，
      // 当store中的变量发生变化时，也会引发组件重绘
      const computed = component.computed || {};
      computed.isEdit = () => {
        return this.isEdit;
      };
      component.computed = computed;

      return result;
    },
  },
  watch: {
    js() {
      // 当代码变化时，清空error，重绘
      this.subCompErr = null;
    },
    template() {
      // 当代码变化时，清空error，重绘
      this.subCompErr = null;
    },
    css() {
      // 当代码变化时，清空error，重绘
      this.subCompErr = null;
    },
  },
  render() {
    const { error: compileErr, value: component } = this.component;
    const error = compileErr || this.subCompErr;
    let errorDom;
    if (error) {
      errorDom = (
        <div class="error-msg-wrapper" style={{ position: !component.hasError ? "absolute" : "" }}>
          <div>{error.type}</div>
          <div>{error.msg}</div>
        </div>
      );
    }
    return (
      <div class="code-preview-wrapper">
        <div class={this.className}>
          <style>{this.scopedStyle}</style>
          <component />
        </div>
        {errorDom}
      </div>
    );
  },
  errorCaptured(err, vm, info) {
    this.subCompErr = {
      msg: err && err.toString && err.toString(),
      type: "自定义组件运行时错误：",
    };
    console.error("自定义组件运行时错误：", err, vm, info);
  },
};
</script>

<style lang="scss" scoped>
.code-preview-wrapper {
  position: relative;

  .error-msg-wrapper {
    top: 0;
    left: 0;
    right: 0;
    bottom: 0;
    z-index: 1;
    background: #fff;
    color: red;
  }
}
</style>
```

## 实现效果

![](/images/vue_custom_component_1.png)

![](/images/vue_custom_component_2.png)

![](/images/vue_custom_component_3.png)

![](/images/vue_custom_component_4.png)

## 总结

这种动态挂在组件的方式很好用，并且可以基于这个去实现任何复杂组件，我们可以在页面通过拖拽实现简单的UI，而复杂的业务逻辑、事件交互等则通过自定义代码实现。
