## **概要**

Draft.js是在React中构建富文本编辑器的框架，基于不可变模式和封装跨浏览器差异来实现。

无论您只是想支持一些内联文本样式，还是构建一个复杂的文本编辑器来编写长篇文章，Draft.js使得构建任何类型的富文本输入变得容易。

### 安装

Darft.js 通过npm安装。它依赖于`React`和`React-dom`

```bash
npm install --save draft-js react react-dom
```

Darft.js基于最新版本的ECMAScript编写，原生不支持IE11，你可以通过 `babel-polyfill` 来进行兼容处理

```bash
npm install --save draft-js react react-dom babel-polyfill
```

了解更多关于[在Draft中使用shim](https://draftjs.org/docs/advanced-topics-issues-and-pitfalls.html#polyfills)的知识。

### API变更通知

开始之前，请注意，我们最近在 Draft 中更改了实体存储的API。

最新版本`v0.10.0`兼容新旧API。 接下来的`v0.11.0`版本将移除旧的API。

如果您有兴趣帮助完善DraftJS或想持续跟踪DraftJS更新进度，请关注[问题839](https://github.com/facebook/draft-js/issues/839)。

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

由于Draft.js支持unicode，您必须在HTML文件的`<head></head>`块中包含以下标签：

```xml
<meta charset="utf-8" />
```

渲染编辑器时应包含`Draft.css`。 前往了解[更多原因](https://draftjs.org/docs/advanced-topics-issues-and-pitfalls.html#missing-draft-css)。

接下来，我们来介绍API的基础知识，并了解您可以用Draft.js做些什么。

