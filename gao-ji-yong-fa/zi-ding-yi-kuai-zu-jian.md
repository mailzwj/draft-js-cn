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

通过在上层组件上下文中定义该函数，该自定义组件的props可能需要绑定到上层组件，以便该自定义组件的props能正确调用上层组件的实例方法。

### 定义自定义块组件

`MediaComponent`最常见的使用场景是恢复实体元数据来渲染自定义块。在管理`EditorState`期间，你可以在`'atomic'`块中将整个实体key全部写进文本，然后在自定义组件的`render()`方法中恢复key对应的元数据。

```js
class MediaComponent extends React.Component {
  render() {
    const {block, contentState} = this.props;
    const {foo} = this.props.blockProps;
    const data = contentState.getEntity(block.getEntityAt(0)).getData();
    // Return a <figure> or some other content using this data.
  }
}
```

在自定义组件中可以使用`contentBlock`对象和`ContentState`记录，以及上层组件中定义的props。通过从`ContentBlock`和`Entity`映射表中提取实体信息，你可以得到渲染自定义组件需要的元数据。

_从块中恢复实体的API被很多人认为不太友好，这值得重新研究。_

### 建议与其他注意事项

如果你的自定义块渲染器需要鼠标交互，通常在交互的时候将`Editor`临时设置为只读（`readOnly={true}`）。这样，当用户触发自定义块上的交互时不会导致编辑器中的选区发生变化。

