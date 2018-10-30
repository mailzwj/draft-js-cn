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

```js
textDirectionality?: DraftTextDirectionality
```

可选的为编辑器设置覆盖文本的方向属性。这些属性有代表文本从右到左的'RTL'，如希伯来文或阿拉伯文，代表文本从左到右的'LTR'，如英语或西班牙语。文本的方向性会应用到整个内容中，无论默认的文本输入方向如何。

如果没有设置这个属性，文本的方向性将会基于编辑器的特性，以每个块为基准。

##### blockRendererFn

```js
blockRendererFn?: (block: ContentBlock) => ?Object
```

可选地设置一个函数来定义自定义块的呈现。有关使用的详细信息，请参阅[高级用法：自定义块组件](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/zi-ding-yi-kuai-zu-jian.html)。

##### blockStyleFn

```js
blockStyleFn?: (block: ContentBlock) => string
```

当给定块渲染时，可选地设置一个函数来定义它的类名。有关使用的详细信息，请参阅[高级用法：块级样式](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/kuai-ji-yang-shi.html)。

##### customStyleMap

```js
customStyleMap?: Object
```

可选地设置一个内联样式表，以应用到具有指定样式的文本范围。有关使用的详细信息，请参阅[高级用法：复杂内联样式](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/fu-za-nei-lian-yang-shi.html)。

#### 行为（可选）

#### autoCapitalize?: string

```js
autoCapitalize?: string
```

设置是否将自动大写打开及其如何表现。更多有关平台可用性及使用信息可参照[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input#attr-autocapitalize)。

#### autoComplete?: string

```js
autoComplete?: string
```

设置是否自动补全及其如何表现。更多有关平台可用性及使用信息可参照[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input#attr-autocomplete)。

#### autoCorrect?: string

```js
autoCorrect?: string
```

设置是否自动校正及其如何表现。更多有关平台可用性及使用信息可参照[MDN](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/Input#attr-autocorrect)。

##### readOnly

```js
readOnly?: boolean
```

设置是否将编辑器渲染为禁止所有编辑行为的静态DOM。

当[自定义块组件](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/zi-ding-yi-kuai-zu-jian.html)相互作用或你只想展示静态用例的内容时，这会很有用。

默认值为`false`。

##### spellCheck

```js
spellCheck?: boolean
```

设置你的编辑器是否启用拼写检查。

注意，在OSX Safari中，打开拼写检查也会打开自动校正功能，如果用户已将其打开。同时也要注意到，IE中始终禁止拼写检查，因为在IE中不会触发观察拼写检查事件所需的事件。

默认值为`false`。

##### stripPastedStyles

```js
stripPastedStyles?: boolean
```

设置是否删除除粘贴内容的纯文本外的所有信息。

如果你的编辑器不支持丰富的样式，则应该使用它。

默认值为`false`。

#### DOM和辅助功能（可选）

##### tabIndex

##### ARIA props

这些prop允许你在你的编辑器上设置辅助功能。在[DraftEditorProps](https://github.com/facebook/draft-js/blob/master/src/component/base/DraftEditorProps.js)上查看详尽的有关支持的属性。

##### editorKey

```js
editorKey?: string
```

你可能不会手动把`editorKey`设置到`<Editor />`上，除非你正在服务端渲染Draft组件。如果是这样，你必须设置这个prop来避免服务端和客户端之间不匹配。

如果没有设置这个key，当组件渲染和被分配作为编辑器的`<DraftEditorContents />`组件的一个prop的时候，这个key会自动生成。

如果你设置了这个prop，那么这里的key应该是每个编辑器唯一的，因为它将会确定在编辑器中粘贴文本时是否要保留样式。

#### 可取消的处理程序（可选）

这些prop函数为一小部分有用的事件提供了一些自定义事件处理程序。通过在处理程序中返回`'handled'`，你可以该事件已被处理，Draft核心将不会做任何事情。通过返回`'not-handled'`，你可以将事件推到Draft中去处理。

##### handleReturn

```js
handleReturn?: (e: SyntheticKeyboardEvent, editorState: EditorState) => DraftHandleValue
```

处理`RETURN` keydown事件。举例：从渲染的列表结果中选择一个提及标签，将提及实例应用到你的内容中。

##### handleKeyCommand

```js
handleKeyCommand?: (command: string, editorState: EditorState, eventTimeStamp: number) => DraftHandleValue
```

处理命名的编辑器命令。有关使用详情参阅[高级用法：案件绑定](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/an-jian-bang-ding.html)。

##### handleBeforeInput

```js
handleBeforeInput?: (chars: string, editorState: EditorState, eventTimeStamp: number) => DraftHandleValue
```

处理从`beforeInput`事件中插入的字符。返回'handled'会导致`beforeInput`事件的默认行为被阻止。（即与事件上调用preventDefault方法相同）。举个例子：当用户在一个块的开头输入`-`的时候，你可以将ContentBlock转换为`unordered-list-item`。

在Facebook中，我们还使用它将输入的ASCII引号转换为“智能”引号，也可以将表情符号转换为图片。

##### handlePastedText

```js
handlePastedText?: (text: string, html?: string, editorState: EditorState) => DraftHandleValue
```

处理直接被粘贴到编辑器里面的文本或html（富文本）。返回true会阻止默认粘贴的表现。

##### handlePastedFiles

```js
handlePastedFiles?: (files: Array<Blob>) => DraftHandleValue
```

处理直接被粘贴进编辑器的文件。

##### handleDroppedFiles

```js
handleDroppedFiles?: (selection: SelectionState, files: Array<Blob>) => DraftHandleValue
```

处理被拖放进编辑器中的文件。

##### handleDrop

```js
handleDrop?: (selection: SelectionState, dataTransfer: Object, isInternal: DraftDragType) => DraftHandleValue
```

处理其他拖拽操作。

#### 键盘处理（可选）

Draft为你提供了一套自定义`keyDown`处理程序来覆盖或重写默认的处理方式。

##### keyBindingFn

```js
keyBindingFn?: (e: SyntheticKeyboardEvent) => ?string
```

这个方法为你暴露了`keyDown`事件的处理程序。如果指定事件发生，你可以执行自定义的逻辑并（或）返回一个与`DraftEditorCommand`一致的字符串，或者执行你自己创建的自定义编辑器指令。举例来说：在Facebook中，它被用来提供提及（当输入一个朋友名字时出现）自动补全菜单的键盘交互。你可以在[这里](http://seejs.me/draft-js-cn/docs/gao-ji-yong-fa/an-jian-bang-ding.html)找到更多详细解释。

#### 鼠标事件

#### onFocus

```js
onFocus?: (e: SyntheticFocusEvent) => void
```

#### onBlur

```js
onBlur?: (e: SyntheticFocusEvent) => void
```

#### 方法

##### focus

```js
focus(): void
```

编辑器强制获取焦点。

##### blur

```js
blur(): void
```

从编辑器移除焦点。













