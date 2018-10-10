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



