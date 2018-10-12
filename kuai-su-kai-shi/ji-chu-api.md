## 基础API

本节概述了`Draft` API的基础知识。 一个使用[基础API的演示示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/plaintext)。

### 受控输入

React `Editor` 组件被设计为一个受控的`ContentEditable`组件，其目的是提供一个可以通过React控制输入的模块化的顶级API。

简单回顾一下，受控输入主要涉及两个关键因素：

1. 用于表示输入状态的值
2. 一个onChange prop函数用来接收输入的更新

这种方式允许组件t通过输入的状态严格控制输入内容，同时仍然允许更新DOM来提供用户所写的文本信息。

```js
class MyInput extends React.Component {
  constructor(props) {
    super(props);
    this.state = {value: ''};
    this.onChange = (evt) => this.setState({value: evt.target.value});
  }
  render() {
    return <input value={this.state.value} onChange={this.onChange} />;
  }
}
```

顶级组件可以通过该state属性来保持对输入状态的控制。

### 控制富文本

在“富文本”案例中，有两个明确的问题：

1. 一串纯文本不足以表示富文本编辑器的复杂状态。
2. 没有一个`ContentEditable`元素可用的`onChange` prop。

因此状态被表示为一个唯一的不可变的 [EditorState](https://draftjs.org/docs/api-reference-editor-state.html) 对象，并且`onChange`在`Editor`内部实现，用来将该状态值提供给顶层使用。  
（state存储的富文本内容是以对象的形式组织的。 ）

```js
import {Editor, EditorState} from 'draft-js';

class MyEditor extends React.Component {
  constructor(props) {
    super(props);
    this.state = {editorState: EditorState.createEmpty()};
    this.onChange = (editorState) => this.setState({editorState});
  }
  render() {
    return <Editor editorState={this.state.editorState} onChange={this.onChange} />;
  }
}
```

当编辑器DOM中发生任何编辑或选择更改，onChange处理程序将被执行并传入基于这些变化的最新的`EditorState`对象。

