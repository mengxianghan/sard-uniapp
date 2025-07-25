---
nav: 组件
title: Dialog
subtitle: 对话框
group: 反馈组件
---

## 介绍

提示或接收用户的确认。

## 引入

```ts
import Dialog from 'sard-uniapp/components/dialog/dialog.vue'
import DialogAgent from 'sard-uniapp/components/dialog-agent/dialog-agent.vue'
import { dialog } from 'sard-uniapp'
```

## 代码演示

### 基础使用

先在页面放置代理组件。

```tsx
<sar-dialog-agent />
```

接着便可以使用 `dialog` 等方法显示对话框。

@code('${DEMO_PATH}/dialog/demo/Basic.vue')

### 异步关闭

如果 `beforeClose` 返回 `false` 或 `rejected` 状态的 `Promise` 可阻止关闭。

@code('${DEMO_PATH}/dialog/demo/AsyncClose.vue')

### 圆角按钮

`buttonType` 属性值为 `round` 可以将底部按钮显示为圆角的形式。

@code('${DEMO_PATH}/dialog/demo/Round.vue')

### 有头部的

配置 `headed` 属性让对话框显示头部，此时的对话框更像一个模态框。

@code('${DEMO_PATH}/dialog/demo/Headed.vue')

### 自定义内容

对话框里面可以放置任何内容，例如以模态的方式展示一个表单，通常要配合 `headed` 属性一起使用。

@code('${DEMO_PATH}/dialog/demo/Content.vue')

### 内容中包含弹出框

`v1.4.1` 起，对话框底层的 `Popup` 组件包裹在了 `root-portal` (小程序)和 `teleport` (H5)中，因此基于 `Popup` 的可弹出组件都可以相互嵌套使用，也可以放置在 `scroll-view` 里面。

@code('${DEMO_PATH}/dialog/demo/PopupContent.vue')

### 自定义按钮属性

使用 `cancelProps` 和 `confirmProps` 属性可以自定义取消和确定按钮组件的属性。

@code('${DEMO_PATH}/dialog/demo/ButtonProps.vue')

## API

### DialogProps

| 属性                           | 描述                                                                 | 类型              | 默认值  |
| ------------------------------ | -------------------------------------------------------------------- | ----------------- | ------- |
| root-class                     | 对话框根元素类名                                                     | string            | -       |
| root-style                     | 对话框根元素样式                                                     | StyleValue        | -       |
| popup-class <sup>1.14.1+</sup> | 弹窗框根元素类名                                                     | string            | -       |
| popup-style <sup>1.14.1+</sup> | 弹窗框根元素样式                                                     | StyleValue        | -       |
| visible (v-model)              | 是否可见                                                             | boolean           | false   |
| title                          | 标题                                                                 | string            | -       |
| message                        | 文本内容                                                             | string            | -       |
| headed                         | 是否显示带头部类型                                                   | boolean           | true    |
| button-type                    | 按钮类型                                                             | 'round' \| 'text' | 'round' |
| show-cancel                    | 是否显示取消按钮                                                     | boolean           | true    |
| cancel-text                    | 取消按钮文案                                                         | string            | '取消'  |
| show-confirm                   | 是否显示确定按钮                                                     | boolean           | true    |
| confirm-text                   | 确定按钮文案                                                         | string            | '确定'  |
| overlay-closable               | 点击遮罩是否关闭                                                     | boolean           | false   |
| before-close                   | 关闭前的回调，返回 `false` 或 `rejected` 状态的 `Promise` 可阻止关闭 | DialogBeforeClose | -       |
| duration                       | 显隐动画时长，单位 ms                                                | number            | 300     |
| confirm-props <sup>1.10+</sup> | 设置确定按钮 props                                                   | ButtonProps       | -       |
| cancel-props <sup>1.10+</sup>  | 设置取消按钮 props                                                   | ButtonProps       | -       |

#### DialogBeforeClose

- 当点击确定按钮时，`type` 为 `confirm`；
- 当点击取消按钮时，`type` 为 `cancel`；
- 当点击关闭按钮或遮罩时，`type` 为 `close`。

`loading` 表示当前哪个按钮处于异步关闭状态。

```ts
type DialogBeforeClose = (
  type: 'close' | 'cancel' | 'confirm',
  loading: {
    readonly cancel: boolean
    readonly confirm: boolean
    readonly close: boolean
  },
) => any | Promise<any>
```

### DialogSlots

| 插槽    | 描述           | 属性 |
| ------- | -------------- | ---- |
| default | 自定义默认内容 | -    |

### DialogEmits

| 事件                             | 描述                        | 类型                               |
| -------------------------------- | --------------------------- | ---------------------------------- |
| update:visible                   | 对话框显隐时触发            | (visible: boolean) => void         |
| close                            | 点击关闭按钮或遮罩时触发    | () => void                         |
| cancel                           | 点击取消按钮时触发          | () => void                         |
| confirm                          | 点击确定按钮时触发          | () => void                         |
| visible-hook                     | 入场/退场动画状态改变时触发 | (name: TransitionHookName) => void |
| before-enter <sup>1.12+</sup>    | 入场动画开始前触发          | () => void                         |
| enter <sup>1.12+</sup>           | 入场动画开始时触发          | () => void                         |
| after-enter <sup>1.12+</sup>     | 入场动画结束时触发          | () => void                         |
| enter-cancelled <sup>1.12+</sup> | 入场动画取消时触发          | () => void                         |
| before-leave <sup>1.12+</sup>    | 退场动画开始前触发          | () => void                         |
| leave <sup>1.12+</sup>           | 退场动画开始时触发          | () => void                         |
| after-leave <sup>1.12+</sup>     | 退场动画结束时触发          | () => void                         |
| leave-cancelled <sup>1.12+</sup> | 退场动画取消时触发          | () => void                         |

### DialogAgentProps / DialogOptions

继承 [`DialogProps`](#DialogProps) 并有以下额外属性。

| 属性                                | 描述                        | 类型                               | 默认值   |
| ----------------------------------- | --------------------------- | ---------------------------------- | -------- |
| id                                  | 对话框组件的 id             | string                             | 'dialog' |
| onClose <sup>1.20.2+</sup>          | 点击关闭按钮或遮罩时调用    | () => void                         |
| onCancel <sup>1.20.2+</sup>         | 点击取消按钮时调用          | () => void                         |
| onConfirm <sup>1.20.2+</sup>        | 点击确定按钮时调用          | () => void                         |
| onVisibleHook <sup>1.20.2+</sup>    | 入场/退场动画状态改变时调用 | (name: TransitionHookName) => void |
| onBeforeEnter <sup>1.20.2+</sup>    | 入场动画开始前调用          | () => void                         |
| onEnter <sup>1.20.2+</sup>          | 入场动画开始时调用          | () => void                         |
| onAfterEnter <sup>1.20.2+</sup>     | 入场动画结束时调用          | () => void                         |
| onEnterCancelled <sup>1.20.2+</sup> | 入场动画取消时调用          | () => void                         |
| onBeforeLeave <sup>1.20.2+</sup>    | 退场动画开始前调用          | () => void                         |
| onLeave <sup>1.20.2+</sup>          | 退场动画开始时调用          | () => void                         |
| onAfterLeave <sup>1.20.2+</sup>     | 退场动画结束时调用          | () => void                         |
| onLeaveCancelled <sup>1.20.2+</sup> | 退场动画取消时调用          | () => void                         |

### DialogAgentEmits <sup>1.20.2+</sup>

继承 [`DialogEmits`](#DialogEmits)。

### 命令式方法

| 名称    | 描述                         | 类型                     |
| ------- | ---------------------------- | ------------------------ |
| dialog  | 显示对话框                   | DialogFunction           |
| alert   | 显示警告框                   | DialogSimpleShowFunction |
| confirm | 显示确认框                   | DialogSimpleShowFunction |
| hide    | 隐藏指定 `id` 的命令式对话框 | (id = 'dialog') => void  |
| hideAll | 隐藏所有命令式对话框         | () => void               |

### DialogFunction

```ts
type DialogFunction = DialogSimpleShowFunction & {
  alert: DialogSimpleShowFunction
  confirm: DialogSimpleShowFunction
  hide: (id?: string) => void
  hideAll: () => void
}
```

### DialogSimpleShowFunction

```ts
interface DialogSimpleShowFunction {
  (options: DialogOptions): void
  (title: string, options?: DialogOptions): void
}
```

### defaultDialogOptions

命令式默认值和声明式有所区别。

```ts
const defaultDialogOptions = {
  headed: false,
  buttonType: 'text',
  showCancel: false,
}
```

## 主题定制

### CSS 变量

@code('./variables.scss#variables')
