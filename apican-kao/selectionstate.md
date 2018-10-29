## SelectionState

`SelectionState`是一个不可变\(Immutable\)记录\([Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)\)，表示编辑器中的选择范围。

`SelectionState`最常见的用法是通过`EditorState.getSelection()`来获得编辑器中当前的`SelectionState`对象。

### Keys and Offsets

一个选区内有两个重要的点：锚点和焦点。（更多详细信息，请参阅[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection#Glossary)）

DOM方法中将每个点表示为一对节点\(Node\)/偏移量\(offset\)，其中偏移量是一个与节点的`childNodesd`的位置相对应的数字。如果节点是文本节点，偏移量则为文本内容的字符偏移量。

由于Draft使用`ContentBlock`对象维护编辑器的内容，我们可以使用自己的模型来表示这些点。因此，这些点可以以key/offset的形式被追踪，其中`key`的值是`ContentBlock`的key所在的位置，`offset`的值是块中的字符偏移量。

### 开始\(Start\)/结束\(End\)与锚点\(Anchor\)/焦点\(Focus\)

当我们真正在浏览器中渲染选区的时候，锚点和焦点的概念非常有用，因为它允许我们根据需要选择向前或向后选择。但是，对于编辑操作，选择的方向无关紧要。这种情况，考虑起点和终点更合适。



