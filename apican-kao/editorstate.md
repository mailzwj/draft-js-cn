## EditorState

`EditorState`是编辑器顶级的state对象。

它是一个不可变的[Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)代表了Draft编辑器的整体状态，包括了：

* The current text content state （当前文本内容状态）
* The current selection state （当前选择状态）
* The fully decorated representation of the contents （内容完整的表示形式）
* Undo/redo stacks （撤销/重做栈）
* The most recent type of change made to the contents （最近一次对内容进行更改的类型）

> 注意
>
> 当使用EditorState对象的时候，你不应该使用Immutable（不可变的） API，而是使用下面实例的getters和静态方法来代替它。

### 概览

_常用的实例方法      
_

下面这个列表包括了EditorState对象最常用的实例方法。

* [getCurrentContent\(\): ContentState](#getcurrentcontent)
* [getSelection\(\): SelectionState](#getselection)
* [getCurrentInlineStyle\(\): DraftInlineStyle](#getcurrentinlinestyle)
* [getBlockTree\(\): OrderedMap](#getblocktree)

_静态方法      
_

* [static createWithContent\(contentState, ?decorator\): EditorState](#createwithcontent)
* [static create\(config\): EditorState](#create)
* [static push\(editorState, contentState, changeType\): EditorState](#push)
* [static undo\(editorState\): EditorState](#撤销（undo）)
* [static redo\(editorState\): EditorState](#chongzuoredo)
* [static acceptSelection\(editorState, selectionState\): EditorState](#accepteselection)
* [static forceSelection\(editorState, selectionState\): EditorState](#forceselection)
* [static moveSelectionToEnd\(editorState\): EditorState](#moveselectiontoend)
* [static moveFocusToEnd\(editorState\): EditorState](#movefocustoend)
* [static setInlineStyleOverride\(editorState, inlineStyleOverride\): EditorState](#setinlinestyleoverride)
* [static set\(editorState, EditorStateRecordType\): EditorState](#setmethods)

_属性      
_

> 注意
>
> 通过`EditorState`静态方法设置属性，而不是直接使用Immutable（不可变的） API。这意味着使用`EditorState.set`去传递EditorState实例新的属性。
>
> **例子      
> **
>
> ```js
> const editorState = EditorState.createEmpty();
> const editorStateWithoutUndo = EditorState.set(editorState, {allowUndo: false});
> ```

* [allowUndo](#allowundo)
* [currentContent](#currentcontent)
* [decorator](#decorator)
* [directionMap](#directionmap)
* [forceSelection](#propertyfs)
* [inCompositionMode](#incompositionmode)
* [inlineStyleOverride](#inlinestyleoverride)
* [lastChangeType](#lastchangetype)
* [nativelyRenderedContent](#nativelyrenderedcontent)
* [redoStack](#redostack)
* [selection](#propertyselection)
* [treeMap](#treemap)
* [undoStack](#undostack)

### 常用实例方法

#### getCurrentContent {#getcurrentcontent}

```js
getCurrentContent(): ContentState
```

返回当前编辑器的内容。

#### getSelection {#getselection}

```js
getSelection(): SelectionState
```

返回当前编辑器光标/选择的状态。

#### getCurrentInlineStyle

```js
getCurrentInlineStyle(): DraftInlineStyle
```

返回一个代表着编辑器“当前”内联样式的`OrderedSet<string>`。

为当前`ContentState`和`SelectionState`插入一个字符，并且考虑任何需要被应用的内联样式重写的时候，这个内联样式的值将会被用到。

#### getBlockTree

```js
getBlockTree(blockKey: string): List;
```

返回一个关于装饰（decorated）和样式（styuled）范围的不可变的列表。这被用来渲染，并基于`currentContent`和`decorator`生成。

在渲染的时候，这个对象用于将内容分成适当的块、装饰器和样式（styled）范围组件。

### 静态方法

#### createEmpty

```js
static createEmpty(decorator?: DraftDecoratorType): EditorState
```

返回一个有空`ContentState`和默认配置的新的`EditorState`对象。

#### createWithContent

```js
static createWithContent(
  contentState: ContentState,
  decorator?: DraftDecoratorType
): EditorState
```

基于`ContentState`和提供的装饰器返回一个新的`EditorState`对象。

#### create

```js
static create(config: EditorStateCreationConfig): EditorState
```

基于配置对象返回一个新的`EditorState`对象。如果你需要自定义配置通过上述方法不可用，那你可以使用这个。

#### push

```js
static push(
  editorState: EditorState,
  contentState: ContentState,
  changeType: EditorChangeType
): EditorState
```

返回一个新的`EditorState`对象，该对象使用指定的`ContentState`作为新的`currentContent`。基于`changeType`，这个`ContentState`可能会被视为撤销/重做的边界状态。

必须使用此方法将所有的内容改变应用到EditorState中。

_需要重命名      
_

#### 撤销（undo）

```js
static undo(editorState: EditorState): EditorState
```

返回一个新的`EditorState`对象，该对象使用撤销栈顶部作为新的`currentContent`。

现有的`currentContent`被推到重做栈上。

#### 重做（redo） {#chongzuoredo}

```js
static redo(editorState: EditorState): EditorState
```

返回一个新的`EditorState`对象，该对象使用重做栈顶部作为新的`currentContent`。

现有的`currentContent`被推到撤销栈上。

#### acceptSelection {#accepteselection}

```js
static acceptSelection(
editorState: EditorState,
selectionState: SelectionState
): EditorState
```

返回一个新的`EditorState`对象，该对象使用指定的`SelectionState`，但是不需要进行选择。

举个例子，当DOM选择在我们控制之外改变时这会有用，并且不需要重新渲染。

#### forceSelection {#forceselection}

```js
static forceSelection(
editorState: EditorState,
selectionState: SelectionState
): EditorState
```

返回一个新的`EditorState`对象，该对象使用指定的`SelectionState`，并强制选择被渲染。

#### moveSelectionToEnd {#moveselectiontoend}

```js
static moveSelectionToEnd(editorState: EditorState): EditorState
```

返回一个新的`EditorState`对象，其选择（selection）在结尾处。

移动选择（selection）到编辑器末尾且不需要强制聚焦（focus）。

#### moveFocusToEnd {#movefocustoend}

```js
static moveFocusToEnd(editorState: EditorState): EditorState
```

返回一个新的`EditorState`对象，其选择（selection）在最后并且聚焦（focus）。

这适用于我们想以编程方式聚焦输入的场景中，且在允许用户连续无缝工作上也是有用的。

#### setInlineStyleOverride {#setinlinestyleoverride}

```js
static setInlineStyleOverride(editorState: EditorState, inlineStyleOverride: DraftInlineStyle): EditorState
```

返回一个新的`EditorState`对象，该对象中应用了指定的`DraftInlineStyle`，此DraftInlineStyle作为下一个插入字符应用的内联样式集。

#### set {#setmethods}

```js
static set(editorState: EditorState, options: EditorStateRecordType): EditorState
```

返回一个传入新选项的新`EditorState`对象。该方法继承自不可变的记录（Immutable `record`）API。

### Properties and Getters

在多数情况下，上述实例和静态方法应足以管理你Draft编辑器的状态。

以下是由`EditorState`追踪属性的完整列表，以及他们相应的getter方法，来更好地提供在该对象中跟踪状态范围的详细信息。

> 注意
>
> 当使用EditorState对象的时候，你不应该使用Immutable（不可变的） API，而是使用下面实例的getters和静态方法来代替它。

#### allowUndo {#allowundo}

```js
allowUndo: boolean;
getAllowUndo()
```

是否允许编辑器进行撤销/重做行为。默认值是`true`。

因为撤销/重做栈是内存保留的主要来源，如果你的编辑器交互不需要撤销/重做栈，你可能要考虑将这个值设置为`false`。

#### currentContent {#currentcontent}

```js
currentContent: ContentState;
getCurrentContent()
```

当前渲染的`ContentState`。请参阅[getCurrentContent\(\)](#getcurrentcontent)。

#### decorator {#decorator}

```js
decorator: ?DraftDecoratorType;
getDecorator()
```

当前的装饰器对象（如果有的话）。

注意`ContentState`是独立于你的装饰器的。如果提供了装饰器，它将被用于装饰渲染文本的范围。

#### directionMap {#directionmap}

```js
directionMap: BlockMap;
getDirectionMap()
```

每个块及其文本方向的映射，它由UnicodeBidiService确定。

你不应该手动管理它。

#### forceSelection {#propertyfs}

```js
forceSelection: boolean;
mustForceSelection()
```

是否强制渲染当前的`SelectionState`。

你应该手动设置这个属性。--请参阅[forceSelection\(\)](#forceselection)。

#### inCompositionMode {#incompositionmode}

```js
inCompositionMode: boolean;
isInCompositionMode()
```

用户是否处于IME组合模式。这对为IME用户渲染适当的UI是有用的，即使是在没有内容改变提交到编辑器的时候它也是有用的。你不应该手动设置这个属性。

#### inlineStyleOverride {#inlinestyleoverride}

```js
inlineStyleOverride: DraftInlineStyle;
getInlineStyleOverride()
```

下一个插入字符应用的内联样式值。当键盘命令或样式按钮应用到一个折叠选择范围的内联样式时，它会被用到。

`DraftInlineStyle`是一个不可变`OrderedSet`的字符串类型别名，其中每一个都对应一个内联样式。

#### lastChangeType {#lastchangetype}

```js
lastChangeType: EditorChangeType;
getLastChangeType()
```

为了给我们当前的`ContentState`发生内容变化的类型。当确定撤销/重做边界状态的时候，这会被用到。

#### nativelyRenderedContent {#nativelyrenderedcontent}

```js
nativelyRenderedContent: ?ContentState;
getNativelyRenderedContent()
```

在编辑行为过程中，编辑器可能会允许某些操作进行原生渲染。举个例子，在基于contentEditable组件中进行输入行为时，我们可以特别允许在DOM中打印字符时键事件失败。这样做我们可以避免额外的重绘并维持拼写检查的高亮。

当允许原生渲染行为时，使用`nativelyRenderedContent`属性来指示这个`EditorState`不需要重绘。

#### redoStack {#redostack}

```js
redoStack: Stack<ContentState>;
getRedoStack()
```

`ContentState`的不可变对象栈，在重做操作的时候它可以被恢复。当执行撤销操作时，当前的`ContentState`被推到`redoStack`中。

你不应该手动管理这个属性。如果你想要禁止撤销/重做行为，使用`allowUndo`属性。

请参阅[undoStack](#undostack)。

#### selection {#propertyselection}

```js
selection: SelectionState;
getSelection()
```

当前渲染的`SelctionState`。参阅[acceptSelection\(\)](#accepteselection)和[forceSelection\(\)](#forceselection)。

你不应该手动管理这个属性。

#### treeMap {#treemap}

```js
treeMap: OrderedMap<string, List>;
```

在编辑器组件中渲染的完整的装饰和样式树。`treeMap`对象由`CotentState`和一个可选的装饰器（`DraftDecoratorType`）生成。

在渲染的时候，组件应该遍历`treeMap`对象使用[getBlockTree\(\)](#getblocktree)来渲染装饰范围和样式范围。

你不应该手动管理这个属性。

#### undoStack {#undostack}

```js
undoStack: Stack<ContentState>;
getUndoStack()
```

`ContentState`对象的一个不可变的栈，该栈可以被还原以进行撤销操作。

当执行修改内容的操作时，我们确定是否当前的`ContentState`是否应被视为用户可以通过执行撤销操作来达到的“边界”状态。如果是这样，`ContentState`会被推进`undoStack`（撤销栈）。如果不是这样，则直接丢弃即将改变的`ContentState`。

你不应该手动管理这个属性。如果你想禁止撤销/重做行为，请使用`allowUndo`属性。

请参阅[redoStack](#redostack)。

