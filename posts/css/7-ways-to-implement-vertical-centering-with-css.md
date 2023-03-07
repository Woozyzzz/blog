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
  height: 200px;
  background: #7a3e65;
}

td {
  background: #a84448;
}
```

<iframe id="example-1"
    title="example-1"
    width="240"
    height="320"
    src="https://codepen.io/Woozyzzz/pen/LYJjXXP">
</iframe>

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
  border: 1px solid green;
}

.child {
  position: absolute;
  top: 50%;
  border: 1px solid red;
  transform: translateY(-50%);
}
```

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
  border: 1px solid green;
}

.child {
  border: 1px solid red;
}
```
