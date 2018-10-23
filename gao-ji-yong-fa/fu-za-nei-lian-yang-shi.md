## 复杂内联样式

在编辑器中，除了提供基础的加粗\(bold\)、斜体\(italic\)、下划线\(underline\)等行内样式，你可能还希望能提供更多种多样的行内样式。例如，你想要支持多种多样的颜色\(color\)、字体\(font-family\)、字号\(font-size\)等等。另外，你所想要的样式可能会叠加，也可能互相排斥。

[Rich Editor](http://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/rich)和[Colorful Editor](http://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/color)两个实例展示了如何使用复杂的内联样式。

### Model 模型

在Draft模型中，行内样式体现在字符级别上，使用一个不可变的`OrderedSet`对象来定义应用于每个字符的样式列表。这些样式通过字符串来识别（详情请参阅[CharacterMetadata](https://draftjs.org/docs/api-reference-character-metadata.html)）。

例如，编辑文本“Hello **world**”。字符串的前六个字符使用空集`OrderedSet()`来表示。最后五个字符使用`OrderedSet.of('BOLD')`来表示。为了方便理解，我们将这一系列`OrderedSet`对象想象成数组，然而实际使用时我们尽可能的复用相同的不可变对象。

从本质上讲，我们的样式如下：

```js
[
  [], // H
  [], // e
  ...
  ['BOLD'], // w
  ['BOLD'], // o
  // etc.
]
```

### 样式叠加

现在，我们希望将中间的字符串显示为斜体：He_llo **wo**_**rld**。这个操作可以使用 [Modifier](https://draftjs.org/docs/api-reference-modifier.html) API来执行。

最终结果将通过在现有的`OrderedSet`对象中添加`'ITALIC'`来实现样式叠加。

```js
[
  [], // H
  [], // e
  ['ITALIC'], // l
  ...
  ['BOLD', 'ITALIC'], // w
  ['BOLD', 'ITALIC'], // o
  ['BOLD'], // r
  // etc.
]
```

当编辑器确定如何渲染内联样式文本时，Draft将标记具备相同样式的连续字符，并将它们渲染到一个具备样式的span标签中。

### 将样式字符串映射到CSS

`Editor`默认提供一个基础内联样式清单：`'BOLD'`、`'ITALIC'`、`'UNDERLINE'`和`'CODE'`。这些命令已经被映射到了简单的CSS样式，用于相关内容区域的样式渲染。

在你的编辑器中，你可能会定义一些自定义样式来包含这些默认样式，或者重写这些基本的默认样式。

当你在使用`Editor`时，你可能会提供一个`customStyleMap` prop来定义你的样式对象。（一个活生生的示例[Colorful Editor](http://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/color)）

例如，你可能想要增加一个`'STRIKETHROUGH'`样式。为此，定义一个自定义样式映射：

```js
import {Editor} from 'draft-js';

const styleMap = {
  'STRIKETHROUGH': {
    textDecoration: 'line-through',
  },
};

class MyEditor extends React.Component {
  // ...
  render() {
    return (
      <Editor
        customStyleMap={styleMap}
        editorState={this.state.editorState}
        ...
      />
    );
  }
}
```

渲染时，`textDecoration: line-through`样式将被应用于所有使用了`STRIKETHROUGH`的字符。

