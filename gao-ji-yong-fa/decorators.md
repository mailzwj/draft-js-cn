## 修饰器

内联和块级样式并不是我们想要添加到编辑器的唯一富文本样式。例如，Facebook评论输入给提及和标签提供了蓝色背景高亮。

为了支持自定义富文本的灵活性，Draft提供了一个“修饰器”系统。[Tweet示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/tweet)提供了一个活生生的修饰器使用示例。

（“装饰器是用来添加高级富内容的一种方法”）

## 组合修饰器

1. 扫描给定的[ContentBlock](https://draftjs.org/docs/api-reference-content-block.html)。
2. 满足定义的匹配策略的文本范围。
3. 使用指定的React组件去渲染它们。

可以使用`CompositeDecorator`类定义所需的修饰器行为。该类允许您提供多个`DraftDecorator`对象，并根据策略依次搜索每一组文本。

修饰器存储在`EditorState`记录中。当创建一个新的`EditorState`对象（例如：`EditorState.createEmpty()`）的时候，可以选择提供修饰器。

> 基于钩子
>
> 当Draft编辑器中的内容更改时，生成的`EditorState`对象将使用其装饰器评估新的`ContentState`，并标识要进行装饰的范围。 此时形成了一个完整的块，修饰器和内联样式，作为我们渲染输出的基础。
>
> 通过这种方式，我们始终保证随着内容的改变，渲染的修饰与我们的`EditorState`同步。

例如，在“Tweet”编辑器示例中，我们使用一个`CompositeDecorator`来搜索@-handle字符串以及主题标签字符串：

```js
const compositeDecorator = new CompositeDecorator([
  {
    strategy: handleStrategy,
    component: HandleSpan,
  },
  {
    strategy: hashtagStrategy,
    component: HashtagSpan,
  },
]);
```

该复合修饰器将首先扫描给定的文本块以获取@-handle匹配，然后对于hashtag匹配进行扫描。

```js
// Note: these aren't very good regexes, don't use them!
const HANDLE_REGEX = /\@[\w]+/g;
const HASHTAG_REGEX = /\#[\w\u0590-\u05ff]+/g;

function handleStrategy(contentBlock, callback, contentState) {
  findWithRegex(HANDLE_REGEX, contentBlock, callback);
}

function hashtagStrategy(contentBlock, callback, contentState) {
  findWithRegex(HASHTAG_REGEX, contentBlock, callback);
}

function findWithRegex(regex, contentBlock, callback) {
  const text = contentBlock.getText();
  let matchArr, start;
  while ((matchArr = regex.exec(text)) !== null) {
    start = matchArr.index;
    callback(start, start + matchArr[0].length);
  }
}
```

策略函数执行提供的回调并传入匹配的文本范围的起始和结束值。

## 修饰器组件

对于您修饰的文本范围，您必须定义一个用于渲染它们的React组件。 这些往往是一个使用 CSS 或 style 的 span 元素。

在我们当前的示例中，名为`HandleSpan`和`HashtagSpan`的`CompositeDecorator`对象作为修饰器使用的组件。

这些只是基本的无状态组件：

```js
const HandleSpan = (props) => {
  return <span {...props} style={styles.handle}>{props.children}</span>;
};

const HashtagSpan = (props) => {
  return <span {...props} style={styles.hashtag}>{props.children}</span>;
};
```

修饰器将从`props`中接收各种各样的元数据片段。包括一个`contentState`、`entityKey`（如果有）、`blockKey`的拷贝。修饰器组件可使用的完整props列表，请查阅[DraftDecoratorComponentProps type](https://github.com/facebook/draft-js/blob/master/src/model/decorators/DraftDecorator.js)。

注意，`props.children`被传递到渲染中输出。 这样做是为了确保文本在装饰的范围内呈现。

您可以使用相同的方法进行链接，如我们的[链接示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/link)所示。

## 超越组合修饰器

提供给`EditorState`使用的修饰器，只需要满足符合类型定义的[DraftDecoratorType](https://github.com/facebook/draft-js/blob/master/src/model/decorators/DraftDecoratorType.js)。这意味着您可以创建任何您想要的装饰器类，只要它们与预定类型相匹配，则不受`CompositeDecorator`的限制。

## 设置新的修饰器

当然，在正常状态传播期间，通过不可变的方式，可以在`EditorState`上设置新的修饰器值。

这意味着在您的应用程序工作流程中，如果您的修饰器无效或需要修改，您可以创建一个新的修饰器对象（或使用`null`删除所有修饰）和`EditorState.set()`来设置新的装饰器。

例如，如果由于某种原因，我们希望在用户与编辑器交互时禁用@-handle修饰的创建，那么执行以下操作会很好：

```js
function turnOffHandleDecorations(editorState) {
  const onlyHashtags = new CompositeDecorator([{
    strategy: hashtagStrategy,
    component: HashtagSpan,
  }]);
  return EditorState.set(editorState, {decorator: onlyHashtags});
}
```

将使用新修饰器重新评估此`editorState`的`ContentState`，并且@-handle修饰将不再存在于下一个渲染过程中。

同样，由于不可变对象上数据的持久性，这在内存中仍然是有效的。

