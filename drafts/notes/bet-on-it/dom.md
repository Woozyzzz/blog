# DOM

## 1 请简述 DOM 事件模型

**记忆题**

- 博客总结
- 要点：
  - 先经历从上到下的捕获阶段，再经历从下到上的冒泡阶段
  - `addEventListener("click",fn,true/false)` 第三个参数默认为 `false` 选择冒泡阶段触发监听
  - 可以使用 `event.stopPropagation()` 来阻止传播冒泡或捕获

## 2 手写事件委托

**记忆题**

- 博客总结
- `e.currentTarget` 是监听对象，`e.target` 是触发对象
- 实际开发中框架会帮你事件委托到根元素上
- 优点：
  - 节省监听器
  - 动态监听（元素还没创建时就可以监听）
- 缺点：
  - 调试时复杂，不容易确定监听者
- 怎么解决缺点：
  - 无

### 2.1 可能通过面试的错误版

```javascript
// JavaScript
// 如果点击 li 内的 span 就会失效
ul.addEventListener("click", function (e) {
  if (e.target.tagName.toLowerCase() === "li") {
    fn();
  }
});
```

### 2.2 高级版

```javascript
// JavaScript
// 点击 span 后，递归遍历 span 的祖先元素看其中有没有 ul 中的 li
const delegate = (element, eventType, selector, fn) => {
  element.addEventListener(eventType, (event) => {
    let { target } = event;

    while (!target.matches(selector)) {
      if (target === element) {
        target = null;
        break;
      }
      target = target.parentNode;
    }

    target && fn.call(target, event);
  });
  return element;
};
```

## 3 手写可拖曳 div

**记忆题**

- 博客总结
- 要点：
  - mousedown 时，记录位置、标记拖曳
  - mousemove 时，鼠标移动多少将记录位置移动多少，记录位置
  - mouseup 时，标记取消拖曳
  - 监听 document 否则鼠标移动快，就掉了
  - 改变 top、left，性能优化用 transform
  - 不要用 drag 事件，很难用
