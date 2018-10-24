## 问题和陷阱

本节主要记录一些我们在Facebook使用Draft编辑器组件时已经遇到过的一些已知问题和常见陷阱。

### 常见陷阱

#### 状态更新被延迟

单项数据传输的一种常见模式是使用`setTimeout`或其他机制对存储的数据进行批量处理或以其他方式延迟更新。存储的数据被更新后，将变更发送给相关的React组件触发重新渲染。

然而，当在React应用中使用Draft编辑器出现延迟问题时，可能会造成严重的体验问题。这也是编辑器内容的更新和渲染希望与用户的输入行为保持同步的原因。延迟可以阻止通过编辑器组件树传播更新，这可能导致按键点击与内容更新不连续。

为了避免这类问题，但仍然使用延迟或批量处理机制，你应该将延迟行为从`Editor`的状态传递中分离出来。也就是说，你必须让你的`EditorState`立即传递到`Editor`组件，并独立处理不影响`Editor`组件状态的批量更新。

#### Draft.css遗漏

Draft框架包含少量与编辑器组件一起使用的CSS样式资源，这些CSS样式被编译为一个独立的`Draft.css`文件。

在渲染编辑器组件的时候，这个css文件应该被引入。这些样式设置了默认的文本对其方式，间距和其他关键特征。没有这些样式，你可能会遇到一些问题，包括块位置、对其方式以及光标行为等。

如果你选择编写独立于`Draft.css`的CSS样式，你可能需要复制很多默认样式。

### 已知问题

#### 自定义OSX快捷键

由于浏览器没有自定义系统级别快捷键的权限，因此想要拦截系统默认快捷键触发的编辑行为是不可能的。

其结果就是在Draft编辑器中使用自定义快捷键的用户可能会遇到问题，因为自定义按键命令的处理结果可能会与他们预期的不一样。

#### 浏览器插件/扩展

跟任何React应用一样，具备修改页面DOM结构能力的浏览器插件和扩展，可能导致Draft编辑器被破坏。

例如，语法检查器可能会修改contentEditable元素内的DOM结构，添加下划线或背景样式等。因为如果浏览器没有匹配到预期的变更，React就无法更新DOM，因此编辑器的状态可能无法与DOM更新保持同步。

一些老旧的广告代码块也是已知的会破坏原生DOM选择API的因素——无论如何，这都是一个不好的做法！由于Draft依赖于该API来控制选择状态，因此这也可能给编辑器交互带来问题。

#### 输入法编辑器与IE（IME and Internet Explorer）

截止IE11，IE浏览器暴露出了一些使用国际输入法是的显著问题，其中最为显著的就是韩语输入。

#### Polyfills

Draft和它的依赖模块中使用了一些ES2015的语言特性。在构建Draft时，像`class`这样的语法特性是通过Babel来编译的，但它并不包括一些在现在高级浏览器中包含的API（例如：`String.prototype.startsWith`）的兼容实现。我们希望您的浏览器本身支持这些API，或者在polyfill的帮助下支持这些API。我们在很多示例中使用了[es6-shim](https://github.com/es-shims/es6-shim)这个polyfill，但如果你有更多的使用场景，请酌情使用[babel-polyfill](https://babeljs.io/docs/usage/polyfill/)。

当使用polyfill/shim时，应该尽早将其进入到应用程序中（至少要在引入Draft之前）。例如，使用[create-react-app](https://github.com/facebookincubator/create-react-app)来创建运行在IE11中的应用，`src/index.js`可能是导入polyfill最合适的地方。

**src/index.js**

```js
import 'babel-polyfill';
// or
import 'es6-shim';

import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import './index.css';

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

#### 暂不支持移动设备

Draft.js正在往支持移动浏览方向发展，但截至目前还没有正式支持移动浏览器。

