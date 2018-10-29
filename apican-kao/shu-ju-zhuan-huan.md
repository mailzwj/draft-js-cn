## 数据转换

因为富文本编辑器不可能凭空存在，因此对内容进行保存和传输非常重要，你可能希望将`ContentState`对象转换为原生JS，或者反过来将原生JS转换为`ContentState`对象。

为此，我们提供了一些实用的函数，允许你实现这些转换。

注意，Draft库目前暂不支持markdown或markup数据的转换，因为不同的客户端可能对这些格式有不同的要求。取而代之，我们提供JS对象，可以根据需要转换为其他格式。

[`RawDraftContentState`](https://github.com/facebook/draft-js/blob/master/src/model/encoding/RawDraftContentState.js)封装了内容的原生格式所期望的数据结构。原始状态包含内容块列表，以及所有相关实体对象的映射。

### 方法

#### convertFromRaw

```js
convertFromRaw(rawState: RawDraftContentState): ContentState
```

将一个原始`state`转换为`ContentState`对象。在Draft编辑器中显示数据的时候非常有用。

#### convertToRaw

```js
convertToRaw(contentState: ContentState): RawDraftContentState
```

将一个`ContentState`对象转换为原生JS结构。当需要保存编辑器状态、将编辑数据转换为其他格式，或在应用中开发其他功能时，非常有用。

#### convertFromHTML

```js
const sampleMarkup =
  '<b>Bold text</b>, <i>Italic text</i><br/ ><br />' +
  '<a href="http://www.facebook.com">Example link</a>';

const blocksFromHTML = convertFromHTML(sampleMarkup);
const state = ContentState.createFromBlockArray(
  blocksFromHTML.contentBlocks,
  blocksFromHTML.entityMap
);

this.state = {
  editorState: EditorState.createWithContent(state),
};
```

将一段HTML片段转换为一个包含两个key的对象。其中一个保存`ContentBlock`对象数组，另一个保存对entityMap的引用。再从块元素数组和entityMap构造contentState，然后使用该contentState更新editorState。查看[完整示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/convertFromHTML)。

