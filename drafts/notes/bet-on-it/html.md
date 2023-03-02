# HTML

## 1 讲讲 HTML 中的语义化标签

**概念题**

1. 是什么：语义化标签是一种写 HTML 的方法论。
2. 怎么做：开发中遇到标题用 h1 到 h6 ，段落用 p，文章用 article，主要内容用 main，边栏用 aside，导航用 nav 等。
3. 解决了什么问题：明确了 HTML 的书写规范。
4. 优点：
   1. 适合搜索引擎检索；
   2. 适合人类阅读，利于团队维护。
5. 缺点：无。
6. 怎么解决缺点：无需解决。

## 2 HTML 5 有哪些新标签？

**记忆题**

- 一定要回答自己熟悉的标签，回答不熟悉的标签大概率会变成下一题。
- 文章相关：header main footer nav article section figure mark
- 多媒体相关：video audio svg canvas
- 表单相关：input type="email" input type="tel"

## 3 SVG 和 Canvas 有什么区别？

**区分题**

1. 先说一，再说二，接着说相同点，最后说不同点，最好有博客总结。
   1. SVG 主要是用标签来绘制不规则矢量图形的。
   2. Canvas 主要是用笔刷来绘制 2D 图形的。
2. 相同点：都是主要用来画 2D 图形的。
3. 不同点：
   1. SVG 绘制的是矢量图，Canvas 绘制的是位图。
   2. SVG 节点过多时渲染慢，Canvas 性能更好一点，但写起来相对复杂。
   3. SVG 支持分层和事件，Canvas 不支持，但是可以用库实现。
