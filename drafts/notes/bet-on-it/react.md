# React

## 1 虚拟 DOM 的原理是什么？

**论述题**

1. 是什么
   1. 虚拟 DOM 就是虚拟节点。React 用 JS 对象来模拟 DOM 节点，然后将其渲染成真实的 DOM 节点。
2. 怎么做
   1. 第一步是模拟，用对象模拟 DOM
      1. 用 JSX 语法写出来的 div 其实就是一个虚拟节点，得到一个包含 tag、props、children 等属性的对象
      2. 能做到这点是因为 JSX 语法会被转译为 `React.createElement` 函数调用（也叫 h 函数）
   2. 第二步是将虚拟节点渲染为真实节点
      1. 使用 render 方法做渲染，接收一个虚拟节点，返回一个真实的 DOM 节点
         1. 如果入参是字符串或者数字，就创建一个文本节点
         2. 否则，就根据 tag 创建一个真实 DOM 标签
            1. 设置标签属性
            2. 遍历子节点，并获取创建真实 DOM，插入到当前节点
            3. 在虚拟 DOM 中缓存真实的 DOM 节点
            4. 返回该节点
      2. 注意：如果节点发生变化，并不是直接把新的虚拟节点渲染到真实节点，而是先经过 diff 算法得到第一个 patch 在更新到真实节点上
3. 解决了什么问题
   1. DOM 操作性能问题。通过虚拟 DOM 和 diff 算法减少不必要的 DOM 操作，保证性能下限不太低
   2. DOM 操作不方便。以前需要记各种 DOM API，现在只有 setState
4. 优点
   1. 为 React 带来了跨平台能力，因为虚拟节点除了渲染为真实 DOM 节点，还可以渲染为其他节点
   2. 让 DOM 操作的整体性能更好，能（通过 diff）减少不必要的 DOM 操作
5. 缺点
   1. 在性能要求极高的地方，还是得用真实的 DOM 操作（目前没遇到）
   2. React 为虚拟 DOM 创造了合成事件，跟原生 DOM 事件不太一样，工作中需要额外注意
      1. 所有 React 时间都绑定到根元素，自动实现事件委托
      2. 如果混用合成事件和原生 DOM 事件，有可能会出 bug
6. 怎么解决缺点
   1. 用 Vue 3

## 2 React 或 Vue 的 DOM diff 算法是怎样的？

**记忆题**

1. 是什么
   1. DOM diff 就是对比两棵虚拟 DOM 树的算法。
   2. 当组件变化时，会 render 出一个新的虚拟 DOM，diff 算法对比新旧虚拟 DOM 之后，得到一个 patch，然后 React 用 patch 来更新真实 DOM。
2. 怎么做
   1. 首先对比两棵的根节点
      1. 如果根节点的类型变化了，比如 div 变成了 p，那么直接认为整颗树都变了，不再对比子节点。此时直接删除对应的真实 DOM 树，创建新的真实 DOM 树
      2. 如果根节点的类型没变，就看看属性是否改变
         1. 如果没变，就保留对应的真实节点
         2. 如果变了，就只更新该节点的属性，不重新创建节点
            1. 更新 style 时，如果多个 css 属性只有一个改变了，那么 React 只更新改变的
      3. 然后同时遍历两棵树的子节点，每个节点的对比过程同上。
         1. 子节点个数发生变化，从上至下遍历，可能更新多次
         2. 面试官可能想听《源码分析之双端交叉对比》
            1. 四个指针，分别指向旧数组的头尾和新数组的头尾
            2. 头头对比：对比两个数组的头部，如果找到，把新节点 patch 到旧节点，头指针后移
            3. 尾尾对比：对比两个数组的尾部，如果找到，把新节点 patch 到旧节点，尾指针前移
            4. 旧尾新头对比：交叉对比，旧尾新头，如果找到，把新节点 patch 到旧节点，旧尾指针前移，新头指针后移
            5. 旧头新尾对比：交叉对比，旧头新尾，如果找到，把新节点 patch 到旧节点，旧头指针后移，新尾指针前移
            6. 利用 key 对比：用新指针对应节点的 key 去旧数组寻找对应的节点
               1. 当没有对应 key 时，那么创建新的节点
               2. 当有对应 key 并且是相同节点，把新节点 patch 到旧节点
               3. 当有对应 key 但不是相同节点，则创建新节点

## 3 React 有哪些生命周期钩子函数？数据请求放在哪个钩子里？

**记忆题**

- hooks 函数组件没有生命周期，class 组件才有
- 挂载时调用 constructor，更新时不调用
- 更新时调用 shouldComponentUpdate 和 getSnapshotBeforeUpdate，挂载时不调用
- shouldComponentUpdate 在 render 前调用， getSnapshotBeforeUpdate 在 render 后调用
- 请求放在 componentDidMount 里，博客总结
  - 放在 constructor 和 getDerivedStateFromProps 里不行，如果 SSR 会在后台运行
  - 放在 getDerivedStateFromProps 、 render 、 getSnapshotBeforeUpdate 和 componentDidUpdate 里都不行，会调用很多次

1. 挂载时
   1. constructor
   2. **getDerivedStateFromProps**
   3. **render**
   4. 更新 DOM 和 refs
   5. **componentsDidMount**
2. 更新时（三种触发）
   1. New props
      1. **getDerivedStateFromProps**
      2. shouldComponentUpdate
      3. **render**
      4. getSnapshotBeforeUpdate
      5. 更新 DOM 和 refs
      6. **componentDidUpdate**
   2. setState()
      1. **getDerivedStateFromProps**
      2. shouldComponentUpdate
      3. **render**
      4. getSnapshotBeforeUpdate
      5. 更新 DOM 和 refs
      6. **componentDidUpdate**
   3. forceUpdate()
      1. **getDerivedStateFromProps**
      2. **render**
      3. getSnapshotBeforeUpdate
      4. 更新 DOM 和 refs
      5. **componentDidUpdate**
3. 卸载时
   1. **componentWillUnmount**

## 4 React 如何实现组件间通信

**记忆题**

1. 父子组件通信：props + 调用父组件传进来的函数
2. 爷孙组件通信：两层父子通信或使用 Context.Provider 填充 value 和 Context.Consumer 获取 value
3. 任意组件通信：其实就是状态管理
   1. Redux
   2. Mobx
   3. Recoil

## 5 你如何理解 Redux？

**记忆题**

1. 文档第一句话：Redux 是一个状态管理库/状态容器。
2. 说一下 Redux 核心概念：
   1. State：存放状态
   2. Action = type 动作类型 + payload 动作载荷：改变数据
   3. Reducer：传一个旧的 State 返回一个新的 State
   4. Dispatch 派发：后面加上 Action
   5. Middleware：举例 redux-thunk redux-promise 中间件
3. 还需要说一下 ReactRedux 核心概念：
   1. connect()(Component) 接受两次参数，第二个参数 Component 将 State 关联起来
   2. mapStateToProps
   3. mapDispatchToProps
   4. 参阅 手写 Redux 写博客

## 6 什么是高阶组件 HOC？

**记忆题**

- 高阶组件：参数是组件，返回值也是组件的函数。什么都能做，举例回答。
- React.forwardRef
- ReactRedux 的 connect
- ReactRouter 的 withRouter

## 7 React Hooks 如何模拟组件生命周期？

1. componentDidMount
2. componentDidUpdate
3. componentWillUnmount
