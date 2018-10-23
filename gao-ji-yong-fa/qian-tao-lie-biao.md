## 嵌套列表

Draft框架提供了嵌套列表支持，如Facebook Notes编辑器所示。在Facebook Notes编辑器中，你可以使用`Tab`和`Shift+Tab`来增加或移除列表项的缩进。

### 实现

[`RichUtils`](https://draftjs.org/docs/api-reference-rich-utils.html)模块提供了一个方便的`onTab`方法来管理这种行为，并且应该足以满足大部分列表嵌套的需求。你可以在你的`Editor`中使用`onTab` prop来使用该功能。

默认情况下，使用`DraftStyleDefault.css`中的样式来为列表项设置合适的间距和列表样式。

注意：目前只支持`有序列表('ordered-list-item')` 和`无序列表('unordered-list-item')`实现缩进，其余的内容块暂不支持。

