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

