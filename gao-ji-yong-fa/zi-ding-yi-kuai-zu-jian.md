## 自定义块组件

Draft被设计来解决简单的富文本编辑问题，比如编辑评论和聊天消息等。但是它也有能力提供更丰富的富文本编辑体验，比如[Facebook Notes](https://www.facebook.com/notes/)。

用户可以从已存在的Facebook相册中选择或者重新上传图片来嵌入到他编辑的内容中。为了达到该目的，DraftJS支持自定义块级渲染方式，以纯文本的方式来渲染富媒体内容。

Draft仓库中提供了一个自定义块渲染的示例[TeX editor](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/tex)，使用[KaTeX库](https://khan.github.io/KaTeX/)将Tex语法转换成公式进行渲染并嵌入编辑器。

Draft还提供了一个[在编辑器中嵌入媒体的示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/media)，演示如何自定义渲染`audio`、`image`和`video`。

通过使用自定义块渲染器，你可以在你的编辑器中加入许多复杂的交互。

### 自定义块组件

在`Editor`组件中，开发者可能会指定一个`blockRendererFn` prop。这个函数允许上层组件根据块类型、文本或其他规则为`ContentBlock`对象定义自定义渲染器。

例如，我们可以希望使用自定义组件`MediaComponent`来渲染`'atomic'`类型的`ContentBlock`对象：

```js
function myBlockRenderer(contentBlock) {
  const type = contentBlock.getType();
  if (type === 'atomic') {
    return {
      component: MediaComponent,
      editable: false,
      props: {
        foo: 'bar',
      },
    };
  }
}

// Then...
import {Editor} from 'draft-js';
class EditorWithMedia extends React.Component {
  ...
  render() {
    return <Editor ... blockRendererFn={myBlockRenderer} />;
  }
}
```

如果`blockRendererFn`函数没有返回一个自定义渲染器对象，`Editor`组件将默认使用`DraftEditorBlock`文本块组件来渲染。

`component`属性用来定义要使用的组件，当可选的`props`对象包含props时，它将通过props.blockProps子属性对象传入自定义组件渲染器。此外，可选属性`editable`用于控制自定义组件是否可被编辑。

如果你的组件不会包含文本，强烈推荐使用`editable: false`。

如果组件包含通过`ContentState`传入的文本，自定义组件应该封装成一个`DraftEditorBlock`组件。这将允许`Draft`准确控制内容中的光标。

