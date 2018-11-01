## ContentBlock

`ContentBlock`是一个不可变的[Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)对象，表示编辑器内容中单个块的完整状态，包括：

* 块中的纯文本内容
* 类型，比如段落、头部，列表项等
* 实体，内联样式和深度信息等

一个`ContentState`对象包含一个由这些`ContentBlock`对象组成的`OrderedMap`对象，它们共同构成了编辑器中全部的内容。

