## RichUtils

`RichUtils`是为实现富文本编辑器准备的一组实用的静态函数集合。

在使用中，这些方法接收带有相关参数的`EditorState`对象，并且返回`EditorState`对象。

### 静态方法

#### currentBlockContainsLink

```js
currentBlockContainsLink(
  editorState: EditorState
): boolean
```

#### getCurrentBlockType

```js
getCurrentBlockType(
  editorState: EditorState
): string
```

#### handleKeyCommand

```js
handleKeyCommand(
  editorState: EditorState,
  command: string
): EditorState?
```

#### insertSoftNewline

```js
insertSoftNewline(
  editorState: EditorState
): EditorState
```

#### onBackspace

```js
onBackspace(
  editorState: EditorState
): EditorState?
```

#### onDelete

```js
onDelete(
  editorState: EditorState
): EditorState?
```

#### onTab

```js
onTab(
  event: SyntheticEvent,
  editorState: EditorState,
  maxDepth: integer
): EditorState
```

#### toggleBlockType

```js
toggleBlockType(
  editorState: EditorState,
  blockType: string
): EditorState
```

#### toggleCode

```js
toggleCode(
  editorState: EditorState
): EditorState
```

#### toggleInlineStyle

```js
toggleInlineStyle(
  editorState: EditorState,
  inlineStyle: string
): EditorState
```

在选中区域上切换指定的内联样式。如果用户的选区是折叠的，则应用或移除内部状态的样式。如果选区没有折叠，则直接将更改应用于文档state。

#### toggleLink

```js
toggleLink(
  editorState: EditorState,
  targetSelection: SelectionState,
  entityKey: string
): EditorState
```

#### tryToRemoveBlockStyle

```js
tryToRemoveBlockStyle(
  editorState: EditorState
): ContentState?
```



