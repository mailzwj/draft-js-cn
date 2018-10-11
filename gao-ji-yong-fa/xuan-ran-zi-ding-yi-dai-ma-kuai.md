## 渲染自定义代码块

本节讨论如何自定义渲染`Draft`默认块。块渲染用于定义支持的块类型及其各自的渲染器，以及将被粘贴的内容转换为已知的`Draft`块类型。

当你粘贴内容或者使用[convertFromHTML](https://draftjs.org/docs/api-reference-data-conversion.html#convertfromhtml)的时候，`Draft`通过将`Draft`渲染块映射为相应的标签，把粘贴的内容转换为相应的渲染块进行展示。

## Draft块类型与标签对应关系

| HTML标签 | Draft块类型 |
| :--- | :--- |
| `<h1/>` | header-one |
| `<h2/>` | header-two |
| `<h3/>` | header-three |
| `<h4/>` | header-four |
| `<h5/>` | header-five |
| `<h6/>` | header-six |
| `<blockquote/>` | blockquote |
| `<pre/>` | code-block |
| `<figure/>` | atomic |
| `<li/>` | unordered-list-item, ordered-list-item\*\* |
| `<div/>` | unstyled\*\*\* |

\*\*：有序/无序基于父元素`<ul/>`或`<ol/>`而定。

\*\*\*：任何未被块渲染器所识别的块都将被处理为unstyled。

