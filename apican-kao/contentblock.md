## ContentBlock

`ContentBlock`是一个不可变的[Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)对象，表示编辑器内容中单个块的完整状态，包括：

* 块中的纯文本内容
* 类型，比如段落、头部，列表项等
* 实体，内联样式和深度信息等

一个`ContentState`对象包含一个由这些`ContentBlock`对象组成的`OrderedMap`对象，它们共同构成了编辑器中全部的内容。

`ContentBlock`对象与块级HTML元素如段落和列表等非常类似。可用的类型包括：

* unstyled（无样式）
* paragraph（段落）
* header-one（一级标题）
* header-two（二级标题）
* header-three（三级标题）
* header-four（四级标题）
* header-five（五级标题）
* header-six（六级标题）
* unordered-list-item（无序列表）
* ordered-list-item（有序列表）
* blockquote（引用块）
* code-block（代码块）
* atomic

可以使用构造函数直接创建新的`ContentBlock`对象。预期的Record值详情如下所述。

### 表示样式和实体

`characterList`字段是一个不可变的`List`对象，包含块中所有字符的`CharacterMetadata`对象。我们通过这种方式用代码来构建块的样式和实体。

通过在这些列表和CharacterMetadata对象上大量使用不可变和数据持久化特性，做到了在编辑器中编辑内容对编辑器的内存占用几乎没有影响。

通过这种方式将内联样式和实体一起编码，编辑`ContentBlock`的函数可以对单个`List`对象执行截取、拼接以及其他`List`支持的操作。

当我们创建一个包含`text`但不包含`characterList`的`ContentBlock`对象时，会默认为提供的文本添加一个带有空`CharacterMetadata`对象的`characterList`对象。

### 概览

_方法_

* [getKey\(\): string](#getkey)
* [getType\(\): DraftBlockType](#gettype)
* [getText\(\): string](#gettext)
* [getCharacterList\(\): List](#getcharacterlist)
* [getLength\(\): number](#getlength)
* [getDepth\(\): number](#getdepth)
* [getInlineStyleAt\(offset: number\): DraftInlineStyle](#getinlinestyleat)
* [getEntityAt\(offset: number\): ?string](#getentityat)
* [getData\(\): Map](#getdata)
* [findStyleRanges\(filterFn: Function, callback: Function\): void](#findstyleranges)
* [findEntityRanges\(filterFn: Function, callback: Function\): void](#findentityranges)

_属性_

> 注意
>
> 使用`ContentBlock`构造函数或设置属性值时，请使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Map)。

* [key: string](#key)
* [type: DraftBlockType](#type)
* [text: string](#text)
* [characterList: List](#characterlist)
* [depth: number](#depth)
* [data: Map](#data)

### 方法介绍

#### getKey\(\) {#getkey}

```js
getKey(): string
```

返回该`ContentBlock`的字符串类型的key。块的key是由字母和数字组成的字符串。建议使用`generateRandomKey`方法来创建块的key。

#### getType\(\) {#gettype}

```js
getType(): DraftBlockType
```

返回该`ContentBlock`的类型。大多数类型值与HTML块级元素一致。

#### getText\(\) {#gettext}

```js
getText(): string
```

返回该`ContentBlock`中的全部纯文本内容。该返回值不包括任何样式、修饰或HTML信息。

#### getCharacterList\(\) {#getcharacterlist}

```js
getCharacterList(): List<CharacterMetadata>
```

返回一个不可变的`List`对象，该List由`ContentBlock`中的每个字符的`CharacterMetadata`对象组成。（详见[CharacterMetadata](/./charactermetadata.html)）

该`List`包含该块的所有样式和实体信息。

#### getLength\(\) {#getlength}

```js
getLength(): number
```

返回该`ContentBlock`中纯文本类容的长度。

这个值使用标准的JavaScript字符串的`length`属性，因此不支持Unicode——Unicode字符将被计算为两个字符长度。

#### getDepth\(\) {#getdepth}

```js
getDepth(): number
```

如果有，则返回当前块的深度值。当前只适用于列表项。

#### getInlineStyleAt\(\) {#getinlinestyleat}

```js
getInlineStyleAt(offset: number): DraftInlineStyle
```

返回该`ContentBlock`中给定位置的`DraftInlineStyle`值（一个`OrderedSet<string>`对象）。

#### getEntityAt\(\) {#getentityat}

```js
getEntityAt(offset: number): ?string
```

返回该`ContentBlock`中给定位置的实体key的值（如果没有则返回`null`）。

#### getData\(\) {#getdata}

```js
getData(): Map<any, any>
```

返回块级元数据。

#### findStyleRanges\(\) {#findstyleranges}

```js
findStyleRanges(
  filterFn: (value: CharacterMetadata) => boolean,
  callback: (start: number, end: number) => void
): void
```

为该`ContentBlock`中每个连续的样式范围执行回调函数。

#### findEntityRanges\(\) {#findentityranges}

```js
findEntityRanges(
  filterFn: (value: CharacterMetadata) => boolean,
  callback: (start: number, end: number) => void
): void
```

为该`ContentBlock`中每个连续的实体范围执行回调函数。

### 属性

> 注意
>
> 使用`ContentBlock`构造函数或设置属性值时，请使用[Immutable Map API](http://facebook.github.io/immutable-js/docs/#/Map)。

#### key {#key}

参见`getKey()`。

#### text {#text}

参见`getText()`。

#### type {#type}

参见`getType()`。

#### characterList {#characterlist}

参见`getCharacterList()`。

#### depth {#depth}

参见`getDepth()`。

#### data {#data}

参见`getData()`。

