## **概要**

Draft.js是在React中构建丰富的文本编辑器的框架，由不可变模型提供支持，并且提取跨浏览器的差异。

Draft.js可以轻松构建任何类型的富文本输入，无论您只是想支持一些内联文本样式，还是构建一个复杂的文本编辑器来构成长篇文章。

### 安装

Darft.js 通过npm安装。它依赖于 React React-dom

```bash
npm install --save draft-js react react-dom
```

Darft.js是使用新版本的ECMAScript编写的，原生不支持IE11,你可以通过 babel-polyfill 来进行转码

```bash
npm install --save draft-js react react-dom babel-polyfill
```

### API变更通知

开始之前，请注意，我们最近在 Draft 中更改了实体存储的API。

最新版本`v0.10.0`支持旧的和新的API。 接下来将是`v0.11.0`，这将删除旧的API。

如果您有兴趣帮助或跟踪进度，请按照[问题839](https://github.com/facebook/draft-js/issues/839)。

### 用法

```js
import React from 'react';
import ReactDOM from 'react-dom';
import {Editor, EditorState} from 'draft-js';

class MyEditor extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            editorState: EditorState.createEmpty()
        };
        this.onChange = (editorState) => this.setState({editorState});
    }
    render() {
        return (
            <Editor editorState={this.state.editorState} onChange={this.onChange} />
        );
    }
}

ReactDOM.render( <MyEditor />, document.getElementById('container') );
```

由于Draft.js支持unicode，您必须在HTML文件的`<head></head>`块中包含以下元标记：

```xml
<meta charset="utf-8" />
```

渲染编辑器时应包含`Draft.css`。 了解更多关于为什么。

接下来，我们来介绍API的基础知识，并了解您可以用Draft.js做些什么。

