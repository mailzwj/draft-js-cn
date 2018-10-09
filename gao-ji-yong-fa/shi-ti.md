## 实体（处理内容中的富数据）

本文讨论了实体系统，该Draft用于使用元数据对文本的范围进行注释。 实体使工程师能够向编辑人员介绍超越样式文本的丰富程度。链接，提及和嵌入式内容都可以使用实体来实现。

\(文本中会有许多高级的元数据，实体就是用于表示这些元数据，使得我们可以在内容中添加链接，图片，等\)

在Draft知识库中，“[链接编辑器](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/link)”和“[实体演示](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/entity)”提供实时代码示例，以帮助澄清实体可以如何使用以及其内置行为。

“[实体API参考](https://draftjs.org/docs/api-reference-entity.html)”提供了创建，检索或更新实体对象时要使用的静态方法的详细信息。

有关实体API最近更改的信息以及如何更新应用程序的示例，请参阅我们的[v0.10 API迁移指南](https://draftjs.org/docs/v0-10-api-migration.html#content)。

## 介绍

实体是表示Draft编辑器中文本范围的元数据的对象。 它有三个属性：

* **type** : 一个字符串，表示它是什么样的实体，例如`'LINK'`，`'MENTION'`，`'PHOTO'`。
* **mutability** : 不要将不可变性与`immutable-js`混淆，此属性表示在编辑器中编辑文本范围时，使用此实体对象注释的一系列文本的行为。 这在下面更详细地讨论。
* **data** : 包含实体元数据的可选对象。 例如，`“LINK”`实体对象可能包含该链接的`href`的值。

所有实体都存储在ContentState记录中。所有的实体通过`ContentState` key 引用， 并且React组件用于描述替换范围。（我们目前不赞成使用以前的API访问实体;请参阅[问题＃839](https://github.com/facebook/draft-js/issues/839)。）

使用[装饰器](https://draftjs.org/docs/advanced-topics-decorators.html)或[自定义块组件](https://draftjs.org/docs/advanced-topics-block-components.html)，您可以根据实体元数据向编辑器添加丰富的渲染样式。

## 创建和检索实体

1. 通过 `contentState.createEntity`来创建实体。
2. 他接受上面3个属性作为参数。
3. 此方法返回一个`ContentState`记录。
4. 通过 `contentState.getLastCreatedEntityKey` 来获取创建的实体记录的key

此键是将实体应用于内容时应使用的值。 例如，`Modifier` 模块包含一个`applyEntity`方法：

```js
const contentState = editorState.getCurrentContent();
const contentStateWithEntity = contentState.createEntity(
  'LINK',
  'MUTABLE',
  {url: 'http://www.zombo.com'}
);
const entityKey = contentStateWithEntity.getLastCreatedEntityKey();
const contentStateWithLink = Modifier.applyEntity(
  contentStateWithEntity,
  selectionState,
  entityKey
);
const newEditorState = EditorState.push(editorState, { currentContent: contentStateWithLink });
```

对于给定的文本范围，则可以通过在`ContentBlock`对象上使用 `getEntityAt()`方法来提取其关联的实体密钥，从而传递目标偏移值。

```js
const blockWithLinkAtBeginning = contentState.getBlockForKey('...');
const linkKey = blockWithLinkAtBeginning.getEntityAt(0);
const contentState = editorState.getCurrentContent();
const linkInstance = contentState.getEntity(linkKey);
const {url} = linkInstance.getData();
```

## 可变性

实体可以是三个“可变性”值中的其中一个。 它们之间的差异是用户对其进行编辑时的行为方式。

请注意，`DraftEntityInstance`对象始终是不可变记录，此属性仅用于指示注释文本在编辑器中如何“突变”。  
（未来的变化可能会重命名此属性，以防止围绕命名的潜在混淆。）

## 不可变（Immutable）

如果不从文本中删除实体注释，则无法更改此文本。 具有这种可变性类型的实体是有效的原子。

例如，在Facebook的输入中，添加一个页面（即奥巴马）。然后，在所提到的文本中添加一个字符，或尝试删除一个字符。请注意，添加字符时，实体被删除，删除字符时，整个实体被删除。

在文本绝对必须匹配其相关元数据的情况下，此可变性值很有用，可能不会被更改。

（ 实体关联的文字修改后就直接全部删掉，文字也删掉，对应的实体也删掉 ）

## 可变（Mutable）

本文可能会自由更改。 例如，链接文本通常意图是“可变的”，因为href和链接文本没有紧密耦合。

\( 可以修改实体对应的内容，不会影响实体与内容的关联 \)

## 分段（Segmented）

“分段”的实体以与“不可变”实体非常相似的方式与其文本紧密耦合，但允许通过删除进行自定义。

例如，在Facebook输入中，添加一个朋友的提及。 然后，添加一个字符到文本。 请注意，实体从整个字符串中删除，因为您提到的朋友可能没有在文本中更改其名称。

接下来，尝试删除提及内的字符或单词。 请注意，只有您删除的部分已被删除。 这样，我们可以允许提及短名称。

（ 不能修改文字，当修改后不会删掉整个范围的文字，但是会取消与实体关联 ）

## 修改实体

由于`DraftEntityInstance`记录是不可变的，因此您不能直接更新实例上的`data`属性。

相反，两个`Entity`方法可用于修改实体：`mergeData`和`replaceData`。  
前者允许通过传入对象来合并来更新数据，而后者在新的数据对象中完全交换。

## 使用实体丰富的内容

本节中的下一篇文章将介绍装饰器对象的用法，可用于检索实体以进行渲染。

链接编辑器示例提供了使用中实体创建和装饰的工作示例。

