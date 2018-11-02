## ContentState

`ContentState`是一个不可变的[Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)对象，表示一下条目的完整状态：

* 一个编辑器的完整contents：文本，块，内联样式和实体范围
* 一个编辑器的selection states：在渲染这些内容之前和之后

`ContentState`对象最常见的用法是通过`EditorState.getCurrentContent()`，该方法提供当前编辑器渲染的`ContentState`对象。

使用`EditorState`对象来维护由`ContentState`对象组成的撤销与重做堆栈。

### 概览

_静态方法_

* [static createFromText\(text: string\): ContentState](#createfromtext)
* [static createFromBlockArray\(blocks: Array\): ContentState](#createfromblockarray)

_方法_

* [getEntityMap\(\)](#getentitymap)
* [getBlockMap\(\)](#getblockmap)
* [getSelectionBefore\(\)](#getselectionbefore)
* [getSelectionAfter\(\)](#getselectionafter)
* [getBlockForKey\(key\)](#getblockforkey)
* [getKeyBefore\(key\)](#getkeybefore)
* [getKeyAfter\(key\)](#getkeyafter)
* [getBlockBefore\(key\)](#getblockbefore)
* [getBlockAfter\(key\)](#getblockafter)
* [getBlocksAsArray\(\)](#getblocksasarray)
* [getFirstBlock\(\)](#getfirstblock)
* [getLastBlock\(\)](#getlastblock)
* [getPlainText\(delimiter\)](#getplaintext)
* [getLastCreatedEntityKey\(\)](#getlastcreatedentitykey)
* [hasText\(\)](#hastext)
* [createEntity\(...\)](#createentity)
* [getEntity\(...\)](#getentity)
* [mergeEntityData\(...\)](#mergeentitydata)
* [replaceEntityData\(...\)](#replaceentitydata)
* [addEntity\(...\)](#addentity)

_属性_

> 使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Map)来设置属性值。
>
> **举个例子**
>
> ```js
> const editorState = EditorState.createEmpty();
> const contentState = editorState.getCurrentContent();
> const contentStateWithSelectionBefore = contentState.set('selectionBefore', SelectionState.createEmpty(contentState.getBlockForKey('1pu4d')));
> ```

* [blockMap](#blockmap)
* [selectionBefore](#selectionbefore)
* [selectionAfter](#selectionafter)

### 静态方法

#### createFromText {#createfromtext}

```js
static createFromText(
  text: string,
  delimiter?: string
): ContentState
```

基于字符串创建`ContentState`对象，并使用分隔符将字符串拆分为`ContentBlock`对象。如果没有显性提供分隔符，则使用`'\n'`分割。

#### createFromBlockArray {#createfromblockarray}

```js
static createFromBlockArray(
  blocks: Array<ContentBlock>,
  entityMap: ?OrderedMap
): ContentState
```

基于一个由`ContentBlock`对象组成的数组来创建`ContentState`对象。默认的`selectionBefore`和`selectionAfter`状态将光标放在内容的起始位置。

### 方法介绍

#### getEntityMap {#getentitymap}

```js
getEntityMap(): EntityMap
```

返回一个包含所有已创建的`DraftEntity`记录的存储对象。在v0.11.0版本中，返回的Map对象被修改为不可变的有序Map对象。

多数情况下，你应该能够使用下面的快捷方法来定位特定的`DraftEntity`记录或获取有关内容状态的信息。

#### getBlockMap {#getblockmap}

```js
getBlockMap(): BlockMap
```

返回表示整个文档状态的`ContentBlock`对象组成的完整有序映射。

多数情况下，你应该能够使用下面的快捷方法来定位特定的`ContentBlock`对象或获取有关内容状态的信息。

#### getSelectionBefore {#getselectionbefore}

```js
getSelectionBefore(): SelectionState
```

返回渲染`blockMap`之前编辑器中显示的`SelectionState`。

当你在编辑器中执行`undo`操作时，当前`ContentState`中的`selectionBefore`将用来在合适的位置设置选区。

#### getSelectionAfter {#getselectionafter}

```js
getSelectionAfter(): SelectionState
```

返回`blockMap`渲染完成后编辑器中显示的`SelectionState`。

当执行任何导致`blockMap`渲染的操作时，选区位置将被设置在`selectionAfter`的位置。

#### getBlockForKey {#getblockforkey}

```js
getBlockForKey(key: string): ContentBlock
```

返回与给定key对应的`ContentBlock`对象。

#### 举个例子

```js
var {editorState} = this.state;
var startKey = editorState.getSelection().getStartKey();
var selectedBlockType = editorState
  .getCurrentContent()
  .getBlockForKey(startKey)
  .getType();
```

#### getKeyBefore {#getkeybefore}

```js
getKeyBefore(key: string): ?string
```

返回`blockMap`中指定key的前一个key，如果该key已经是第一个则返回`null`。

#### getKeyAfter {#getkeyafter}

```js
getKeyAfter(key: string): ?string
```

返回`blockMap`中指定key的后一个key，如果该key已经是最后一个则返回`null`。

#### getBlockBefore {#getblockbefore}

返回`blockMap`中指定key的前一个`ContentBlock`对象，如果该key已经是第一个则返回`null`。

#### getBlockAfter {#getblockafter}

```js
getBlockAfter(key: string): ?ContentBlock
```

返回`blockMap`中指定key的后一个`ContentBlock`对象，如果该key已经是最后一个则返回`null`。

#### getBlocksAsArray {#getblocksasarray}

```js
getBlocksAsArray(): Array<ContentBlock>
```

以数组形式返回`blockMap`中的所有`ContentBlock`。

因为`getBlockMap`可以得到一个可用于迭代的`OrderedMap`对象，因此通常你不需要使用该方法。

#### getFirstBlock {#getfirstblock}

```js
getFirstBlock(): ContentBlock
```

返回第一个`ContentBlock`对象。

#### getLastBlock {#getlastblock}

```js
getLastBlock(): ContentBlock
```

返回最后一个`ContentBlock`对象。

#### getPlainText {#getplaintext}

```js
getPlainText(delimiter?: string): string
```

返回使用分隔符\(delimiter\)连接的完整的纯文本内容。如果没有指定分隔符，默认使用换行符\(`\u00A`\)连接。

#### getLastCreatedEntityKey {#getlastcreatedentitykey}

```js
getLastCreatedEntityKey(): string
```

返回最近一次创建的`DraftEntity`Record对象的应用key。因为在`ContentState`对象中可以通过字符串key来引用对应的实体对象。应在`CharacterMetadata`对象中使用字符串key来标记对应的字符实体。

#### hasText {#hastext}

```js
hasText(): boolean
```

返回编辑内容是否包含文本。

#### createEntity {#createentity}

```js
createEntity(
  type: DraftEntityType,
  mutability: DraftEntityMutability,
  data?: Object
): ContentState
```

返回`EntityMap`中包含了新建的`DraftEntity`对象的`ContentState`对象。调用`getLastCreatedEntityKey`可以获得新建的`DraftEntity`对象的字符串key。

#### getEntity {#getentity}

```js
getEntity(key: string): DraftEntityInstance
```

返回指定key对应的`DraftEntityInstance`对象。如果找不到与指定key对应的对象则抛出异常。

#### mergeEntityData {#mergeentitydata}

```js
mergeEntityData(
  key: string,
  toMerge: {[key: string]: any}
): ContentState
```

由于`DraftEntityInstance`对象是不可变的，你不能使用传统的修改方式来修改它们的元数据。

mergeData方法允许你在指定的实体上应用更新。

#### replaceEntityData {#replaceentitydata}

```js
replaceEntityData(
  key: string,
  newData: {[key: string]: any}
): ContentState
```

replaceData方法与mergeData方法类似，除了replaceData方法会使用新数据替换全部旧数据。

#### addEntity {#addentity}

```js
addEntity(instance: DraftEntityInstance): ContentState
```

大多数情况下你会使用`contentState.createEntity()`。这是一个你可能在Draft传统用法中不需要快捷方法。

当实例已经被创建，并且需要添加到实体存储对象上时，add方法非常有用。这种情况常常在我们使用原生JS表示的`ContengState`对象正在被恢复编辑的时候发生。

### 属性

> 使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Map)来设置属性值。

#### blockMap {#blockmap}

详见`getBlockMap()`。

#### selectionBefore {#selectionbefore}

详见`getSelectionBefore()`。

#### selectionAfter {#selectionafter}

详见`getSelectionAfter()`。

