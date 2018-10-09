## 基础API

本文档概述了`Draft` API的基础知识。 一个工作的例子也可以跟随。

## 受控输入

React组件 `Editor` 构建为受控的ContentEditable组件，  
其目标是提供一个基于熟悉的React控制输入API建模的顶级API。

作为一个简短的回顾，控制输入涉及两个关键因素：

1. 用于表示输入状态的值
2. 一个onChange支持功能来接收输入的更新

这种方法允许构成输入的组件对输入的状态进行严格的控制，  
同时仍然允许对DOM的更新来提供关于用户所写的文本的信息。

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

顶级组件可以通过该值state属性来保持对输入状态的控制。

## 控制富文本

在“富文本”案例中，有两个明确的问题：

1. 一串纯文本不足以表示丰富的编辑器的复杂状态。
2. 一个 onChange prop function 用于接收输入的更新

State因此被表示为一个不可变的 [EditorState](https://draftjs.org/docs/api-reference-editor-state.html) 对象，并且`onChange`在`Editor`内部实现，  
以将该状态值提供给顶层。  
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

对于在编辑器DOM中发生的任何编辑或选择更改，您的onChange处理程序将根据这些更改执行最新的EditorState对象。

