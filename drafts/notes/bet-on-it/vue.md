# Vue

## 1 Vue 2 的生命周期钩子有哪些？数据请求放在哪个钩子？

**记忆题**

- Vue 2 文档中红色空心框中文字皆为生命周期钩子
  - beforeCreate、created
  - beforeMount、mounted
  - beforeUpdate、updated
  - beforeDestroy、destroyed
- 还有三个写在文档钩子列表里
  - activated：被 keep-alive 缓存的组件激活时被调用
  - deactivated：被 keep-alive 缓存的组件失活时被调用
  - errorCaptured：在捕获一个组件或其后代组件的错误时被调用（组件封装时用，上报问题）
- 数据请求通常放在 mounted 里
  - created 如果用 SSR 会在后端、前端各执行一次，SSR 会执行 created
  - updated 触发太频繁了
  - destroyed 组件都销毁了没用

## 2 Vue 2 组件间通信方式有哪些？

**记忆题**

1. 父子组件：父组件传 props 向子组件通信，子组件调用 $emit() 触发事件向父组件通信
2. 爷孙组件：
   1. 使用两次父子组件间通信实现
   2. 使用 provide + inject 来通信，在爷组件提供在孙组件注入
3. 任意组件：
   1. 使用 eventBus = new Vue() 来通信
      1. 主要 API 是 `eventBus.$on()` 和 `eventBus.$emit()`
      2. 但是事件多了就很乱，难以维护
   2. 使用 Vuex 通信(Vue 3 可用 Pinia 代替 Vuex)

## 3 Vuex 用过吗？怎么理解？

**论述题**

1. 背出文档第一句：Vuex 是一个专为 Vue.js 应用程序开发的状态管理模式+库
2. 说出核心概念的名字和作用：store/State/Getter/Mutation/Action/Module
   1. store 是个大容器，包含以下所有内容
   2. State 用来读取状态，带有一个 mapState 辅助函数
   3. Getter 用来读取派生（计算）状态，带有一个 mapGetter 辅助函数
   4. Mutation 用于同步提交状态变更，带有一个 mapMutation 辅助函数
   5. Action 用于异步变更状态，但它提交的是 Mutation，而不是直接变更状态
   6. Module 用来给 store 划分模块，方便维护代码
3. 常见追问：Mutation 和 Action 为什么要分开？
   1. 为了让代码更容易维护（万能答案，实际上就是作者喜好，Pinia 将 Mutation 和 Action 合并了）

## 4 Vue Router 用过吗？怎么理解？

**论述题**

1.  背出文档第一句：Vue Router 是 Vue.js 的官方路由，它与 Vue.js 核心深度集成，让用 Vue.js 构建单页应用变得轻而易举
2.  说出核心概念的名字和作用：
    1.  router-link 点击跳转
    2.  router-view 容纳路由视图
    3.  嵌套路由通过加上 children 属性让子路由渲染在 router-view 中
    4.  Hash 模式和 HisTory 模式
    5.  导航守卫
    6.  懒加载 import()
3.  常见追问：
    1.  Hash 模式和 History 模式的区别
        1.  Hash 模式用的 URL 里面的 hash，Hash 模式是纯前端路由
        2.  History 模式用的 HTML5 的 API，History 模式需要后端 Nginx 配合（把所有的 HTML 请求都重定向到 index）
    2.  导航守卫如何实现登录控制？
        1. 每一个路由都可以设置一个钩子，设置进入、解析、离开时做什么、是否放行
        2. 判断路由是否为登录页或受控页面，决定是否放行或重定向到登录页

## 5 Vue 2 是如何实现双向绑定的？

**记忆题**

1. 双向绑定一般使用 `v-model` / `.sync` 实现，`v-model` 是绑定 value `v-bind:value` 和监听 input 事件 `v-on:input` 的语法糖
   1. `v-bind:value` 实现了 data 到 UI 的单向绑定
   2. `v-on:input` 实现了 UI 到 data 的单向绑定
   3. 两个单向绑定合起来就是双向绑定
2. 这两个单向绑定是如何实现的呢？
   1. 前者通过 `Object.defineProperty` API 递归遍历 data 的每个属性创建 getter 和 setter ，用于监听 data 的改变，当 data 变化就会改变 UI
   2. 后者通过 temple compiler 给 DOM 根元素添加事件委托监听，当 DOM input 值变化了就会修改 data 中对应属性

## 6 Vue 3 为什么使用 Proxy？

**论述题**

1. 为了弥补 `Object.defineProperty` 的两个不足
   1. 动态创建的 data 属性需要使用 `Vue.set` 来赋值，Vue3 使用了 Proxy 就不需要了
   2. 出于性能考虑，Vue 2 篡改了 7 个数组 API，Vue3 使用了 Proxy 就不需要了
      1. `push()`、`pop()`、`shift()`、`unshift()`、`splice()`、`sort()`、`reverse()`
2. `Object.defineProperty` 需要提前递归地遍历 data 以做到响应式，而 Proxy 可以在真正用到深层数据的时候再做（惰性）响应式

## 7 Vue3 为什么使用 Composition API？

**论述题**

- 尤雨溪博客 [Code, Design & Things in between - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/evanyou)

1. Composition API 比 mixins、高阶组件、extends、Renderless Components 等更好，原因有三点：
   1. 模板中的数据来源不清晰
   2. 命名空间冲突
   3. 性能
2. 更适合 TypeScript

- Vue 3 - template + tsx = 升级版 React
- Svelte = 升级版的 Vue 2

## 8 Vue3 对比 Vue2 做了哪些改动？

**记忆题**

- 显著的变化

1. `createApp()` 代替 `new Vue()`
2. `v-model` 代替以前的 `v-model` 和 `.sync`
3. 单文件组件中的根元素可以有不止一个元素
4. 新增 `Teleport` 传送门
5. `destroyed` 改名为 `unmounted`，`beforeDestroy` 也改名为 `beforeUnmount`
6. `ref` 属性支持函数了
