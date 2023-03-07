# 用 CSS 实现 7 种垂直居中的方法

如果父元素不设置高度，那么直接对设置上下内边距，即可实现子元素的垂直居中。

## 1 只有一个单元格的 table 元素

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
  background: #7a3e65;
}

td {
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/LYJjXXP)

## 2 用 div 模拟 table 元素

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
  padding: 8px;
  display: table;
  height: 400px;
  background: #7a3e65;
}

.tr {
  display: table-row;
}

.td {
  display: table-cell;
  vertical-align: middle;
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/KKxvEgq)

## 3 100% 高度的行内块级伪元素

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
  background: #7a3e65;
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
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/GRXveaN)

## 4 负二分之一子元素高度的顶部外边距与绝对定位

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
  background: #7a3e65;
}

.child {
  --height: 100px;
  position: absolute;
  top: 50%;
  margin-top: calc(var(--height) / -2);
  height: var(--height);
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/VwGzNdb)

## 5 自动顶部底部外边距与绝对定位

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
  background: #7a3e65;
}

.child {
  position: absolute;
  top: 0;
  bottom: 0;
  margin-top: auto;
  margin-bottom: auto;
  height: 100px;
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/oNPeOQW)

## 6 垂直平移与绝对定位

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
  background: #7a3e65;
}

.child {
  position: absolute;
  top: 50%;
  background: #a84448;
  transform: translateY(-50%);
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/yLxodyX)

## 7 flex 布局

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
  background: #7a3e65;
}

.child {
  background: #a84448;
}
```

[效果预览](https://codepen.io/Woozyzzz/pen/gOdxNaO)

## 8 参考文章

1. [table - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/table)
2. [tr - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/tr)
3. [td：表格数据单元格元素 - HTML（超文本标记语言） | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/td)
4. [display - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/display)
5. [::before (:before) - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::before)
6. [::after (:after) - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/::after)
7. [position - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)
8. [translateY() - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/transform-function/translateY)
9. [align-items - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/align-items)
