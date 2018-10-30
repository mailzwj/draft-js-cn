## EditorChangeType

[EditorChangeType](https://github.com/facebook/draft-js/blob/master/src/model/immutable/EditorChangeType.js)是一个枚举，它列出了可以被Draft模型处理的改变操作集。它被展示成流类型，作为字符串的并集。

它作为一个参数传递给EditorState.push，并且表明了正在执行的转换到新的ContentState这一改变操作的类型。

在背后，这个值被用来确定适当的撤销/重做、拼写检查行为等。因此，虽然可以在这里提供任意的字符串类型值作为changeType参数，但是你应该避免这么做。

我们强烈建议你安装[Flow](https://flow.org/)来对你的项目进行静态类型检查。Flow会强制运用适当的EditorChangeType值。

### 值（Values）

#### `adjust-depth`

一个或者多个`ContentBlock`对象的`depth`值正在被改变。

#### `apply-entity`

一个实例正在被添加（或者移除为`null`）到一个或者多个字符。

#### `backspace-character`

正在向后删除单个字符。

#### `change-block-data`

一个或多个`ContentBlock`对象的`data`值正在被改变。

#### `change-block-type`

一个或多个ContentBlock对象的type值正在被改变。

#### `change-inline-style`

一个或多个字符的内联样式正在被添加或者移除。

#### `move-block`

[BlockMap](https://github.com/facebook/draft-js/blob/master/src/model/immutable/BlockMap.js)正在移动一个块。

#### `delete-character`

正在向前删除单个字符。

#### `insert-characters`

正在向slection state中插入一个或多个字符。

#### `insert-fragment`

正在向slection state中插入内容的一个“片段”（例如[BlockMap](https://github.com/facebook/draft-js/blob/master/src/model/immutable/BlockMap.js)）。

#### `redo`

正在执行重做操作。由于重做表现是由Draft核心处理的，你可能不太需要明白地使用它。

#### `remove-range`

正在移除多个字符或者块。

#### `spellcheck-change`

正在执行拼写检查或自动校正改变。用于通知核心编辑器是否尝试去允许原生的撤销行为。

#### `split-block`

正在将一个单独的`ContentBlock`分割成两个，例如当用户按下回车的时候。

#### `undo`

正在执行撤销操作。由于撤销操作是由Draft核心处理的，你可能不太需要明白地使用它。

