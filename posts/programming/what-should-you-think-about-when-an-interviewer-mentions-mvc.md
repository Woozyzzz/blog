# 当面试官提到 MVC 时，你该想些什么？

## 1 什么是 MVC？

MVC 是上世纪七十年代末提出的一种著名的软件架构模式，曾经被应用于设计桌面图形界面，现在被用于设计 Web 应用和移动应用。它将软件系统分为三个基本部分：模型、视图和控制器，以此解耦程序结构、提高代码复用性，实现关注点的干净分离。其中：

- M（Model）是模型，负责管理应用数据与业务规则。
- V（View）是视图，负责用户界面的输出展示。
- C（Controller）是控制器，负责接收用户输入并将其转换为模型或视图。

## 2 什么是架构模式？

对于软件设计中普遍存在的各种问题的解决策略，根据不同的抽象层次可以分为架构模式、设计模式和代码模式。

架构模式（architectural pattern）是高层次的策略，描述了软件系统中的基本结构组织或核心内容。它将预先定义一些子系统，制定各个子系统的职责，并给出子系统之间的组织规则。

设计模式（design pattern）是中层次的策略，描述了普遍存在的相互通信组件内重复出现的结构。它提供子系统或组件之间的结构关系，用于解决一些场景下的一般性设计问题。

代码模式（coding pattern）是低层次的策略，描述了在特定语言下的代码范例和编程技巧。

虽然 MVC 经常被称为设计模式，但是它其实是一种架构模式。因为设计模式用于解决特定的技术问题，而架构模式用于解决系统架构问题，影响着整个应用程序的结构。

## 3 什么是关注点分离？

关注点分离是一种将计算机程序分隔为不同部分的设计原则。程序中每个部分都有其各自关注的焦点，开发者通过代码封装将解决特定领域问题的代码从业务逻辑中抽离出来，使得各部分能够独立开发、维护，便于程序的管理。这种能够展现关注点分离设计的程序被称为模块化程序。

## 4 参考文章

1. [MVC - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/MVC)
2. [架构模式 - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/%E6%9E%B6%E6%9E%84%E6%A8%A1%E5%BC%8F)
3. [设计模式 (计算机) - 维基百科，自由的百科全书 (wikipedia.org)](<https://zh.wikipedia.org/wiki/%E8%AE%BE%E8%AE%A1%E6%A8%A1%E5%BC%8F_(%E8%AE%A1%E7%AE%97%E6%9C%BA)>)
4. [关注点分离 - 维基百科，自由的百科全书 (wikipedia.org)](https://zh.wikipedia.org/wiki/%E5%85%B3%E6%B3%A8%E7%82%B9%E5%88%86%E7%A6%BB)
5. [Why is Model View Controller so popular? — Accion Labs](https://www.accionlabs.com/why-is-model-view-controller-so-popular)
6. [Everything you need to know about MVC architecture | by Zanfina Svirca | Towards Data Science](https://towardsdatascience.com/everything-you-need-to-know-about-mvc-architecture-3c827930b4c1)
