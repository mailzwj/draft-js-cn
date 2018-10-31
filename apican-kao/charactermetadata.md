## CharacterMetadata

`CharacterMetadata`是一个包含单一字符行内样式和实体信息的不可更改的[Record](http://facebook.github.io/immutable-js/docs/#/Record/Record)对象。

`CharacterMetadata`对象被即时的汇总和共享。如果两个字符拥有相同的行内样式和实体，它们会被表示为相同的`CharacterMetadata`对象。因此，我们只需要尽可能多的组合带有实体key的内联样式集合，以达到即便内容的大小和复杂性增加，我们的内存占用也很小的目的。

为此，你需要用过提供的静态方法来为`CharacterMetadata`对象创建或应用变更，这能确保最大限度的复用。

由于大多数常用的编辑操作已经通过一些实用模块封装实现，因此大多数的Draft使用场景中不会（直接）用到这些静态方法。但是，在渲染的时候，可能会使用到getter方法。

有关如何在`ContentBlock`中使用`CharacterMetadata`对象，请参阅[ContentBlock API介绍](/./contentblock.html)。

### 概览

_静态方法_

* [static create\(...\): CharacterMetadata](#create)
* [static applyStyle\(...\): CharacterMetadata](#applystyle)
* [static removeStyle\(...\): CharacterMetadata](#removestyle)
* [static applyEntity\(...\): CharacterMetadata](#applyentity)

_方法_

* [getStyle\(\): DraftInlineStyle](#getstyle)
* [hasStyle\(style: string\): boolean](#hasstyle)
* [getEntity\(\): ?string](#getentity)

### 静态方法

在底层，这些方法将利用对象池返回匹配到的对象，如果对象不存在则放回一个新的对象。

#### create {#create}

```js
static create(config?: CharacterMetadataConfig): CharacterMetadata
```

根据提供的配置信息生成一个`CharacterMetadata`对象。应该使用该方法来替代使用构造函数。

这些配置会被用来在对象池中检查是否已经存在与该配置相匹配的对象。如果存在，池中的对象将被返回。否则，将创建一个新的`CharacterMetadata`对象放入池中，并返回该新对象。

#### applyStyle {#applystyle}

```js
static applyStyle(
  record: CharacterMetadata,
  style: string
): CharacterMetadata
```

在`CharacterMetadata`对象上应用一个\(组\)内联样式。

#### removeStyle {#removestyle}

```js
static removeStyle(
  record: CharacterMetadata,
  style: string
): CharacterMetadata
```

从`CharacterMetadata`对象中移除一个\(组\)内联样式。

#### applyEntity {#applyentity}

```js
static applyEntity(
  record: CharacterMetadata,
  entityKey: ?string
): CharacterMetadata
```

在`CharacterMetadata`对象上应用一个实体key，如果`entityKey`为`null`，则为移除实体key。

### 方法介绍

#### getStyle {#getstyle}

```js
getStyle(): DraftInlineStyle
```

返回用于该字符的`DraftInlineStyle`对象，一个字符串组成的`OrderedSet`对象，代表在渲染该字符时使用的内联样式。

#### hasStyle {#hasstyle}

```js
hasStyle(style: string): boolean
```

返回该字符串是否包含指定的样式。

#### getEntity {#getentity}

```js
getEntity(): ?string
```

返回应用到该字符的实体key\(如果有\)，该key映射到`Entity`模块记录的全局实体集合中的实体。

通过此处记录字符串key，我们可以将具体的元数据对象与相应的字符串key分开。

如果返回`null`，则说明没有实体应用到该字符。

