## Modifier

`Modifier`模块是一组实用的静态函数，主要封装`ContentState`对象上的各种常用编辑操作。强烈推荐你使用这些方法来开发编辑操作。

考虑到受影响的实体的可变类型，这些方法还负责适当的删除和修改实体范围。

任何情况下，这些方法都接收具有相关参数的`ContentState`对象，并返回一个新的`ContentState`对象。如果实际并未发生任何编辑行为，将原样返回输入的`ContentState`对象。

### 概览

方法列表

* [replaceText\(...\): ContentState](#replacetext)
* [insertText\(...\): ContentState](#inserttext)
* [moveText\(...\): ContentState](#movetext)
* [replaceWithFragment\(...\): ContentState](#replacewithfragment)
* [removeRange\(...\): ContentState](#removerange)
* [splitBlock\(...\): ContentState](#splitblock)
* [applyInlineStyle\(...\): ContentState](#applyinlinestyle)
* [removeInlineStyle\(...\): ContentState](#removeinlinestyle)
* [setBlockType\(...\): ContentState](#setblocktype)
* [setBlockData\(...\): ContentState](#setblockdata)
* [mergeBlockData\(...\): ContentState](#mergeblockdata)
* [applyEntity\(...\): ContentState](#applyentity)

### 静态方法

#### replaceText {#replacetext}

```js
replaceText(
  contentState: ContentState,
  rangeToReplace: SelectionState,
  text: string,
  inlineStyle?: DraftInlineStyle,
  entityKey?: ?string
): ContentState
```

使用提供的字符串替换该`ContentState`的指定范围的内容，并将内联样式和实体key应用于整合插入的字符串。

例如：在Facebook，当使用提到Abraham Lincoln替换`@abraham lincoln`时，替换的目标是整个旧的实体范围，提到的实体应该应用于新插入的字符串。

#### insertText {#inserttext}

```js
insertText(
  contentState: ContentState,
  targetRange: SelectionState,
  text: string,
  inlineStyle?: DraftInlineStyle,
  entityKey?: ?string
): ContentState
```

与`replaceText`相同，只是强制折叠目标范围，以便不替换任何内容。这只是为了方便，因为文本编辑通常是插入而不是替换。

#### moveText {#movetext}

```js
moveText(
  contentState: ContentState,
  removalRange: SelectionState,
  targetRange: SelectionState
): ContentState
```

将指定范围\(removal\)移动到目标范围\(target\)，并替换目标范围的文本。

#### replaceWithFragment {#replacewithfragment}

```js
replaceWithFragment(
  contentState: ContentState,
  targetRange: SelectionState,
  fragment: BlockMap
): ContentState
```

"fragment"是块映射的一部分，实际上只是一个`OrderedMap<string, ContentBlock>`，与`ContentState`对象的完整映射非常相似。

该方法将使用fragment替换目标范围\(target\)。

例如：当我们粘贴内容的时候，我们将粘贴的内容转换为要插入到编辑器的片段，然后使用该方法来添加。

#### removeRange {#removerange}

```js
removeRange(
  contentState: ContentState,
  rangeToRemove: SelectionState,
  removalDirection: DraftRemovalDirection
): ContentState
```

从编辑器中移除整个文本范围。删除方向\(removalDirection\)对于正确的实体删除行为非常重要。

#### splitBlock {#splitblock}

```js
splitBlock(
  contentState: ContentState,
  selectionState: SelectionState
): ContentState
```

将选中的块拆分为两个块。仅供选区折叠时使用。

#### applyInlineStyle {#applyinlinestyle}

```js
applyInlineStyle(
  contentState: ContentState,
  selectionState: SelectionState,
  inlineStyle: string
): ContentState
```

将指定的内联样式应用于整个选中的范围。

#### removeInlineStyle {#removeinlinestyle}

```js
removeInlineStyle(
  contentState: ContentState,
  selectionState: SelectionState,
  inlineStyle: string
): ContentState
```

从整个选中范围中移除指定的内联样式。

#### setBlockType {#setblocktype}

```js
setBlockType(
  contentState: ContentState,
  selectionState: SelectionState,
  blockType: DraftBlockType
): ContentState
```

设置所有选中块的块类型。

#### setBlockData {#setblockdata}

```js
setBlockData(
  contentState: ContentState,
  selectionState: SelectionState,
  blockData: Map<any, any>
): ContentState
```

为所有选中的块设置数据。

#### mergeBlockData {#mergeblockdata}

```js
mergeBlockData(
  contentState: ContentState,
  selectionState: SelectionState,
  blockData: Map<any, any>
): ContentState
```

更新所有选中块的数据。

#### applyEntity {#applyentity}

```js
applyEntity(
  contentState: ContentState,
  selectionState: SelectionState,
  entityKey: ?string
): ContentState
```

将实体应用于整个选中的范围，如果`entityKey`为`null`，则移除整个范围中的所有实体。

