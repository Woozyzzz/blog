# 计算 CSS 选择器的优先级

- 优先级是分配给指定 CSS 声明的权重
- 当同一个元素有多个声明时，优先级高的声明会覆盖优先级低的，作用于元素样式上
- 当多个声明的优先级相等时，最后的声明会覆盖前面的，作用于元素样式上
- 选择器越具体，其优先级越高
- 元素的内联样式（如： `style="color: red"` ）会覆盖外部样式表的样式

## 1 选择器类型对优先级的影响

1. ID 选择器（如： `#example` ）的优先级最高
2. 其次为类选择器（如： `.example` ）、属性选择器（如： `[type="radio"]` ）和伪类（如： `:hover` ）
3. 再次为类型（元素）选择器（如： `div` ）和伪元素（如： `::before` ）
4. 通配选择符（ `*` ）、关系选择符与匹配伪类（ `:is()` ）、关系伪类（ `:has()` ）和否定伪类（ `:not()` ）对优先级没有影响，**但是在 `:is()` 、 `:has()` 和 `:not()` 其括号内部声明的选择器会影响优先级**

## 2 `!important` 规则

- `!important` 与优先级无关，而与最终样式结果直接相关
- 当一个样式声明中使用 `!important` 规则时，此声明将覆盖任何其他声明
- 当两条相互冲突的带有 `!important` 规则的声明被应用到相同元素上时，优先级高的声明会覆盖优先级低的，作用于元素样式上

### 2.1 应用场景

- 覆盖内联样式
- 覆盖优先级高的选择器

### 2.2 经验法则

- 除非万不得已，否则不要使用 `!important` ，优先使用样式规则的优先级解决问题
- 只有在需要覆盖全站或外部 CSS 的特定页面中使用 `!important`
- 永远不要在你的插件中使用 `!important`
- 永远不要在全站范围的 CSS 代码中使用 `!important`

## 3 `:is()` 、 `:not()` 和 `:has()` 规则

- 在优先级权重计算中，匹配伪类（ `:is()` ）、关系伪类（ `:has()` ）和否定伪类（ `:not()` ）不会被认为是伪类且没有权重
- 然而，其括号内传入的选择器参数会参与优先级权重的计算

## 4 总结

1. 相同优先级，后面的样式声明覆盖前面的
2. 选择器越具体，优先级越高
3. `!important` 规则会覆盖任何其他声明，包括内联样式，尽量避免使用
4. 内联样式会覆盖外部样式表的样式
5. ID 选择器 > 类选择器、属性选择器、伪类 > 类型（元素）选择器、伪元素
6. 通配选择符、关系选择符和匹配伪类、关系伪类和否定伪类对优先级没有影响
7. 匹配伪类、关系伪类和否定伪类括号内部声明的选择器会影响优先级

## 5 参考文章

1. [优先级 - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/Specificity)
2. [:is() (:matches(), :any()) - CSS：层叠样式表 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Web/CSS/:is)
3. [层叠与继承 - 学习 Web 开发 | MDN (mozilla.org)](https://developer.mozilla.org/zh-CN/docs/Learn/CSS/Building_blocks/Cascade_and_inheritance#specificity_2)
4. [属性赋值，层叠（Cascading）和继承 (ayqy.net)](http://www.ayqy.net/doc/css2-1/cascade.html#specificity)
