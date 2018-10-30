## SelectionState

`SelectionState`是一个不可变\(Immutable\)记录\([Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)\)，表示编辑器中的选择范围。

`SelectionState`最常见的用法是通过`EditorState.getSelection()`来获得编辑器中当前的`SelectionState`对象。

### Keys and Offsets

一个选区内有两个重要的点：锚点和焦点。（更多详细信息，请参阅[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Selection#Glossary)）

DOM方法中将每个点表示为一对节点\(Node\)/偏移量\(offset\)，其中偏移量是一个与节点的`childNodesd`的位置相对应的数字。如果节点是文本节点，偏移量则为文本内容的字符偏移量。

由于Draft使用`ContentBlock`对象维护编辑器的内容，我们可以使用自己的模型来表示这些点。因此，这些点可以以key/offset的形式被追踪，其中`key`的值是`ContentBlock`的key所在的位置，`offset`的值是块中的字符偏移量。

### 开始\(Start\)/结束\(End\)与锚点\(Anchor\)/焦点\(Focus\)

当我们真正在浏览器中渲染选区的时候，锚点和焦点的概念非常有用，因为它允许我们根据需要选择向前或向后选择。但是，对于编辑操作，选择的方向无关紧要。这种情况，考虑起点和终点更合适。

因此，`SelectionState`会同时提供锚点/焦点和开始点/结束点的值。管理选择行为时，我们建议使用锚点和焦点来维护选择方向。但是，在管理内容操作时，我们更建议使用开始和结束值。

例如，当我们基于`SelectionState`从块中截取一段文本时，是否向后选择内容并不重要：

```js
var selectionState = editorState.getSelection();
var anchorKey = selectionState.getAnchorKey();
var currentContent = editorState.getCurrentContent();
var currentContentBlock = currentContent.getBlockForKey(anchorKey);
var start = selectionState.getStartOffset();
var end = selectionState.getEndOffset();
var selectedText = currentContentBlock.getText().slice(start, end);
```

注意，对于`SelectionState`本身只会记录锚点和焦点值，开始和结束值需要计算才能得到。

### 概览

_静态方法_

* [static createEmpty\(blockKey\)](#createempty)

_方法_

* [getStartKey\(\)](#getstartkey)
* [getStartOffset\(\)](#getstartoffset)
* [getEndKey\(\)](#getendkey)
* [getEndOffset\(\)](#getendoffset)
* [getAnchorKey\(\)](#getanchorkey)
* [getAnchorOffset\(\)](#getanchoroffset)
* [getFocusKey\(\)](#getfocuskey)
* [getFocusOffset\(\)](#getfocusoffset)
* [getIsBackward\(\)](#getisbackward)
* [getHasFocus\(\)](#gethasfocus)
* [isCollapsed\(\)](#iscollapsed)
* [hasEdgeWithin\(blockKey, start, end\)](#hasedgewithin)
* [serialize\(\)](#serialize)

_属性_

> 使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Record/Record)来设置属性。
>
> **例**
>
> ```js
> const selectionState = SelectionState.createEmpty();
> const selectionStateWithNewFocusOffset = selection.set('focusOffset', 1);
> ```

* [anchorKey](#anchorkey)
* [anchorOffset](#anchoroffset)
* [focusKey](#focuskey)
* [focusOffset](#focusoffset)
* [isBackward](#isbackward)
* [hasFocus](#hasfocus)

### 静态方法

#### createEmpty\(\) {#createempty}

```js
createEmpty(blockKey: string): SelectionState
```

在提供的块key的零偏移处创建一个`SelectionState`对象，并将`hasFocus`设置为`false`。

### 方法

#### getStartKey\(\) {#getstartkey}

```js
getStartKey(): string
```

返回包含选区起始位置的块的key。

#### getStartOffset\(\) {#getstartoffset}

```js
getStartOffset(): number
```

返回选择范围块级字符起始位置的偏移量。

#### getEndKey\(\) {#getendkey}

```js
getEndKey(): string
```

返回包含选区结束位置的块的key。

#### getEndOffset\(\) {#getendoffset}

```js
getEndOffset(): number
```

返回选择范围块级字符结束位置的偏移量。

#### getAnchorKey\(\) {#getanchorkey}

```js
getAnchorKey(): string
```

返回包含选区锚点位置的块的key。

#### getAnchorOffset\(\) {#getanchoroffset}

```js
getAnchorOffset(): number
```

返回选择范围块级字符锚点位置的偏移量。

#### getFocusKey\(\) {#getfocuskey}

```js
getFocusKey(): string
```

返回包含选区焦点位置的块的key。

#### getFocusOffset\(\) {#getfocusoffset}

```js
getFocusOffset(): number
```

返回选择范围块级字符焦点位置的偏移量。

#### getIsBackward\(\) {#getisbackward}

```js
getIsBackward(): boolean
```

返回文档内的焦点位置是否在锚点位置前面。

这必须根据ContentState的key的活动顺序来演算，如果选区范围完全在一个块范围内，则可以通过比较锚点和焦点的偏移值获得返回结果。

#### getHasFocus\(\) {#gethasfocus}

```js
getHasFocus(): boolean
```

返回编辑器是否包含焦点（个人理解，编辑器是否处于焦点状态）。

#### isCollapsed\(\) {#iscollapsed}

```js
isCollapsed(): boolean
```

返回选区是否处于折叠状态，例如，一个插入符号（光标）。当锚点和焦点的key相同，并且锚点和焦点的偏移量也相同的时候，返回值为true。

#### hasEdgeWithin\(\) {#hasedgewithin}

```js
hasEdgeWithin(blockKey: string, start: number, end: number): boolean
```

返回选区范围是否与指定的块内指定的开始/结束位置发生重叠。

当内容渲染后在块内操作DOM选择时，这个方法非常有用。

#### serialize\(\) {#serialize}

```js
serialize(): string
```

返回`SelectionState`的一个序列化版本，调试时非常有用。

### Properties

> 使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Record/Record)来设置属性。

```js
var selectionState = SelectionState.createEmpty('foo');
var updatedSelection = selectionState.merge({
  focusKey: 'bar',
  focusOffset: 0,
});
var anchorKey = updatedSelection.getAnchorKey(); // 'foo'
var focusKey = updatedSelection.getFocusKey(); // 'bar'
```

#### anchorKey {#anchorkey}

包含选择范围锚点的块的key。

#### anchorOffset {#anchoroffset}

选择范围锚点位置的偏移量。

#### focusKey {#focuskey}

包含选择范围焦点的块的key。

#### focusOffset {#focusoffset}

选择范围焦点位置的偏移量。

#### isBackward {#isbackward}

在文档中，如果锚点位置是否在焦点位置后面，则为向后选择。注意：`SelectionState`是一个并不了解`ContentState`结构的对象。因此，当你在更新`SelectionState`的值的时候，你需要同时更新`isBackward`。

#### hasFocus {#hasfocus}

当前编辑器是否包含焦点。

