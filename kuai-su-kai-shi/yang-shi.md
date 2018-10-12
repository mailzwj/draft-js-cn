## 装饰富文本

现在我们已经建立了顶级API的基础知识，我们可以进一步了解如何将基本的丰富样式添加到`Draft`编辑器。

一个可以继续使用的[富文本编辑器示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/rich)。

### EditorState: Yours to Command

之前的文章介绍了`EditorState`对象。作为由编辑器内部通过`onChange` prop提供的完整状态的简单介绍。

但是，由于您的顶级React组件负责维护状态，您也可以以任何您认为合适的方式修改该`EditorState`对象。

对于内联和块样式行为，例如，[`RichUtils`](https://draftjs.org/docs/api-reference-rich-utils.html)模块提供了许多有用的函数来帮助处理状态。

类似地，[修饰器\(Modifier\)](https://draftjs.org/docs/api-reference-modifier.html) 模块还提供了许多常见操作，允许您应用编辑，包括对文本，样式等的更改。该模块是一组编辑功能，它们组成更简单，更小的编辑功能，以返回所需的`EditorState`对象。

对于这个例子，我们将坚持使用`RichUtils`来演示如何在顶级组件中应用基本的丰富样式。

### RuchUtils和键盘命令

`RichUtils`提供有关Web编辑器可用的核心键盘命令的信息，如Cmd + B（粗体），Cmd + I（斜体）等）。

我们可以通过`handleKeyCommand` prop来观察和处理键盘命令，并将它们连接到`RichUtils`中以应用或删除所需的样式。

```js
import {Editor, EditorState, RichUtils} from 'draft-js';

class MyEditor extends React.Component {
  constructor(props) {
    super(props);
    this.state = {editorState: EditorState.createEmpty()};
    this.onChange = (editorState) => this.setState({editorState});
    this.handleKeyCommand = this.handleKeyCommand.bind(this);
  }
  handleKeyCommand(command, editorState) {
    const newState = RichUtils.handleKeyCommand(editorState, command);
    if (newState) {
      this.onChange(newState);
      return 'handled';
    }
    return 'not-handled';
  }
  render() {
    return (
      <Editor
        editorState={this.state.editorState}
        handleKeyCommand={this.handleKeyCommand}
        onChange={this.onChange}
      />
    );
  }
}
```

> `handleKeyCommand`
>
> 提供给`handleKeyCommand`的命令参数是一个字符串值，即要执行的命令\(command\)的名称。  
> 这是从DOM键盘事件映射的。 有关更多信息，请参阅高级主题 - [按键绑定](https://draftjs.org/docs/advanced-topics-key-bindings.html)，以及为什么函数返回处理或未被处理的详细信息。

### 控制UI样式

在React组件中，您可以添加按钮或其他控件，以允许用户在编辑器中修改样式。在上面的例子中，我们使用已知的按键命令，但是我们可以添加更复杂的UI来提供这些丰富的功能。

这是一个超级基本示例，“Bold”按钮可以切换`BOLD`样式。

```js
class MyEditor extends React.Component {
  // …

  _onBoldClick() {
    this.onChange(RichUtils.toggleInlineStyle(this.state.editorState, 'BOLD'));
  }

  render() {
    return (
      <div>
        <button onClick={this._onBoldClick.bind(this)}>Bold</button>
        <Editor
          editorState={this.state.editorState}
          handleKeyCommand={this.handleKeyCommand}
          onChange={this.onChange}
        />
      </div>
    );
  }
}
```



