## Editor组件

本文讨论了核心受控的contentEditable组件（`Editor`组件）自身的API和props，Props是在`DraftEditorProps`中定义的。

### Props

#### 基本要素（Basics）

有关介绍请参阅[基础API](http://seejs.me/draft-js-cn/docs/kuai-su-kai-shi/ji-chu-api.html)。

##### editorState

```js
  editorState: EditorState
```

`EditorState`对象由`Editor`创建。

##### onChange

```js
  onChange: (editorState: EditorState) => void
```

`onChange`方法在`Editor`编辑或文本选区（selection）变化的时候触发。

#### Presentation \(可选\)

##### placeholder

```js
placeholder?: string
```

当编辑器输入为空时展示可选的placeholder字段。

注意：你可以根据需要用CSS给placeholder添加style样式或隐藏placeholder。以富文本编辑器举例，当用户在空编辑器中更改块样式时placeholder被隐藏。这是因为当样式更改时，placeholder可能不会与光标对齐。

##### textAlignment

```js
textAlignment?: DraftTextAlignment
```

可选地为编辑器设置覆盖文本的对齐方式。无论输入文本的默认文字方向如何，该对齐方式会应用到整个文本。

如果你想在你的UI设计里将文本居中或者整合到某个方向可以使用textAlignment。

如果这个属性的值未被设置，文本对齐方式将会基于编辑器的特性，以每个块为基准对齐。

##### textDirectionality

```
textDirectionality?: DraftTextDirectionality
```

可选的为编辑器设置覆盖文本的方向属性。

