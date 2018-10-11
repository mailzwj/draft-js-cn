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

## 配置块与标签的对应关系

`Draft`默认的块渲染映射表可以通过使用`blockRender` prop传入一个[Immutable Map](http://facebook.github.io/immutable-js/docs/#/Map)来重写。

一个覆盖默认渲染映射表的例子：

```js
// The example below deliberately only allows
// 'heading-two' as the only valid block type and
// updates the unstyled element to also become a h2.
const blockRenderMap = Immutable.Map({
  'header-two': {
    element: 'h2'
  },
  'unstyled': {
    element: 'h2'
  }
});

class RichEditor extends React.Component {
  render() {
    return (
      <Editor
        ...
        blockRenderMap={blockRenderMap}
      />
    );
  }
}
```

有些情况下，我们不想重写默认值，而只想添加新的块类型，可以使用`DefaultDraftBlockRenderMap`创建一个新的`blockRenderMap`来完成。

```js
const blockRenderMap = Immutable.Map({
  'section': {
    element: 'section'
  }
});

// Include 'paragraph' as a valid block and updated the unstyled element but
// keep support for other draft default block types
const extendedBlockRenderMap = Draft.DefaultDraftBlockRenderMap.merge(blockRenderMap);

class RichEditor extends React.Component {
  render() {
    return (
      <Editor
        ...
        blockRenderMap={extendedBlockRenderMap}
      />
    );
  }
}
```

当Draft分析粘贴的HTML时，它遍历HTML元素找到对应的块类型。如果你想指定映射到特定类型块的HTML元素，你可以在块配置中添加一个`aliasedElements`数组。

_unstyled块的别名用法示例：_

```js
'unstyled': {
  element: 'div',
  aliasedElements: ['p'],
}
```

## 自定义块包装器

一般情况下，html元素被用来包装块类型。但是`blockRenderMap`也可以使用React组件来包装`EditorBlock`。

当粘贴或使用[convertFromHTML](https://draftjs.org/docs/api-reference-data-conversion.html#convertfromhtml)的时候，html将被扫描为对应的标签元素。当在`blockRenderMap`上定义了特殊类型块时，该包装器将会被使用。例如：

Draft使用包装器包装`<ul/>`或`<ol/>`中的`<li/>`，但包装器也可以用于包装其他任意自定义类型。

扩展默认块映射表来支持使用React组件渲染自定义块的示例：

```js
class MyCustomBlock extends React.Component {
  constructor(props) {
    super(props);
  }

  render() {
    return (
      <div className='MyCustomBlock'>
        {/* here, this.props.children contains a <section> container, as that was the matching element */}
        {this.props.children}
      </div>
    );
  }
}

const blockRenderMap = Immutable.Map({
  'MyCustomBlock': {
    // element is used during paste or html conversion to auto match your component;
    // it is also retained as part of this.props.children and not stripped out
    element: 'section',
    wrapper: <MyCustomBlock />,
  }
});

// keep support for other draft default block types and add our myCustomBlock type
const extendedBlockRenderMap = Draft.DefaultDraftBlockRenderMap.merge(blockRenderMap);

class RichEditor extends React.Component {
  ...
  render() {
    return (
      <Editor
        ...
        blockRenderMap={extendedBlockRenderMap}
      />
    );
  }
}
```



