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
* [getLastCreatedEntityKey\(\)](#getlastcreateentitykey)
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
* [selectionBefore](#selectbefore)
* [selectionAfter](#selectafter)

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



