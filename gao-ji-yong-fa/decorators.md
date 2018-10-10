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



