## 块级样式

为了限制工程师在配置运行自定义编辑器时的基本配置参数的数量，`Editor`中某些类型的代码块已经赋予了默认CSS样式。

通过在`Editor`组件上定义`blockStyleFn` prop函数，可以指定用于代码块的class\(css类选择器\)。

### DraftStyleDefault.css

`Draft`库在[DraftStyleDefault.css](https://github.com/facebook/draft-js/blob/master/src/component/utils/DraftStyleDefault.css)中提供了默认的块CSS样式。（注意：CSS类名前的注释是由Facebook内部的模块管理系统自动生成的）

这个css文件主要提供列出项的默认css样式，不需要使用者调用什么方法来管理他们的默认样式。

### blockStyleFn

渲染的时候，`Editor`上的`blockStyleFn` prop 允许你定义CSS类来渲染块的样式。例如，你可能希望`'blockquote'`块中的文本显示成花式斜体样式：

```js
function myBlockStyleFn(contentBlock) {
  const type = contentBlock.getType();
  if (type === 'blockquote') {
    return 'superFancyBlockquote';
  }
}

// Then...
import {Editor} from 'draft-js';
class EditorWithFancyBlockquotes extends React.Component {
  render() {
    return <Editor ... blockStyleFn={myBlockStyleFn} />;
  }
}
```

然后，编写你自己的CSS样式：

```css
.superFancyBlockquote {
  color: #999;
  font-family: 'Hoefler Text', Georgia, serif;
  font-style: italic;
  text-align: center;
}
```



