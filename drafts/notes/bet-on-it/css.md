# CSS

## 1 BFC 是什么？

**概念题**

1. 是什么：BFC 是块级格式化上下文（直接翻译，不要试图解释，没有人能确切描述）。
2. 怎么做：[BFC 的触发条件](https://developer.mozilla.org/zh-CN/docs/Web/Guide/CSS/Block_formatting_context)
   - 浮动元素（float 值不为 none）
   - 绝对定位元素（position 值为 absolute 或 fixed）
   - 行内块级元素（display 值为 inline-block）
   - overflow 值不为 visible、clip 的块级元素
   - 弹性元素（display 值为 flex 或 inline-flex 元素的直接子元素）
3. 解决了什么问题：
   1. 清除浮动（最好用 .clearfix）
   2. 防止 margin 合并
   3. 某些古老的布局方式会用到（已过时）

## 2 如何实现垂直居中？

**实践题**

- 博客总结。[用 CSS 实现 7 种垂直居中的方法](https://woozyzzz.github.io/blog/posts/css/7-ways-to-implement-vertical-centering-with-css)
- 如果父元素不设置高度，那么直接对设置上下内边距，即可实现子元素的垂直居中。

### 2.1 只有一个单元格的 table 元素

```html
<!-- HTML -->
<table>
  <tr>
    <td>垂直居中</td>
  </tr>
</table>
```

```css
/* CSS */
table {
  height: 400px;
  border: 1px solid #000;
}
```

### 2.2 用 div 模拟 table 元素

```html
<!-- HTML -->
<div class="table">
  <div class="tr">
    <div class="td">垂直居中</div>
  </div>
</div>
```

```css
/* CSS */
.table {
  display: table;
  height: 400px;
  border: 1px solid #000;
}

.tr {
  display: table-row;
}

.td {
  display: table-cell;
  vertical-align: middle;
}
```

### 2.3 100% 高度的行内块级伪元素

```html
<!-- HTML -->
<div class="parent">
  <div class="child">垂直居中</div>
</div>
```

```css
/* CSS */
.parent {
  height: 400px;
  border: 1px solid green;
}

.parent::before,
.parent::after {
  display: inline-block;
  height: 100%;
  vertical-align: middle;
  content: "";
}

.child {
  display: inline-block;
  vertical-align: middle;
  border: 1px solid red;
}
```

### 2.4 负二分之一子元素高度的顶部外边距与绝对定位

```html
<!-- HTML -->
<div class="parent">
  <div class="child">垂直居中</div>
</div>
```

```css
/* CSS */
.parent {
  position: relative;
  height: 400px;
  border: 1px solid green;
}

.child {
  --height: 100px;
  position: absolute;
  top: 50%;
  margin-top: calc(var(--height) / -2);
  height: var(--height);
  border: 1px solid red;
}
```

### 2.5 自动顶部底部外边距与绝对定位

```html
<!-- HTML -->
<div class="parent">
  <div class="child">垂直居中</div>
</div>
```

```css
/* CSS */
.parent {
  position: relative;
  height: 400px;
  border: 1px solid green;
}

.child {
  position: absolute;
  top: 0;
  bottom: 0;
  margin-top: auto;
  margin-bottom: auto;
  height: 100px;
  border: 1px solid red;
}
```

### 2.6 垂直平移与绝对定位

```html
<!-- HTML -->
<div class="parent">
  <div class="child">垂直居中</div>
</div>
```

```css
/* CSS */
.parent {
  position: relative;
  height: 400px;
  border: 1px solid green;
}

.child {
  position: absolute;
  top: 50%;
  border: 1px solid red;
  transform: translateY(-50%);
}
```

### 2.7 flex 布局

```html
<!-- HTML -->
<div class="parent">
  <div class="child">垂直居中</div>
</div>
```

```css
/* CSS */
.parent {
  display: flex;
  align-items: center;
  height: 400px;
  border: 1px solid green;
}

.child {
  border: 1px solid red;
}
```

## 3 CSS 选择器的优先级如何确定？

**实践题**

- 博客总结。[计算 CSS 选择器的优先级](https://woozyzzz.github.io/blog/posts/css/calculating-the-specificity-of-css-selectors)

1. 相同优先级，后面的样式声明覆盖前面的
1. 选择器越具体，优先级越高
1. `!important` 规则会覆盖任何其他声明，包括内联样式，尽量避免使用
1. 内联样式会覆盖外部样式表的样式
1. ID 选择器 > 类选择器、属性选择器、伪类 > 类型（元素）选择器、伪元素
1. 通配选择符、关系选择符和匹配伪类、关系伪类和否定伪类对优先级没有影响
1. 匹配伪类、关系伪类和否定伪类括号内部声明的选择器会影响优先级

## 4 如何清除浮动？

**实践题** **记忆题**

- 博客总结。
- 方法 1，给父元素加上 `overflow:hidden`
- 方法 2，给父元素加上 `.clearfix`

```css
/* CSS */
.clearfix::after {
  display: block;
  /* 或者 */
  /* display: table; */
  clear: both;
  content: "";
}

/* 兼容 IE */
.clearfix {
  zoom: 1;
}
```

## 5 两种盒模型（box-sizing）的区别？

**区分题**

- 先说一，再说二，接着说相同点，最后说不同点，最好有博客总结。

1. 第一种盒模型是内容盒模型（content-box），即 width 指定的是 content 区域的宽度，而不是实际宽度，`实际宽度 = width + padding + border`
2. 第二种盒模型是边框盒模型（border-box），即 width 指定的是左右边框外侧之间的距离，`实际宽度 = width`
3. 相同点：都是用来指定宽度的
4. 不同点：border-box 更直观更好用
5. border-box 最早由 IE 提出
