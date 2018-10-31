## Entity

`Entity`是一个包含创建、检索和更新实体对象API的静态模块，用于使用元数据批注文本范围。该模块还包含一个唯一的存储对象，用于维护实体数据。

本节详细介绍这些API。更多有关如何使用实体的介绍，请参见[实体的高级用法](https://draftjs.org/docs/advanced-topics-entities.html)一文。

请注意，实体存储和管理相关的API，最近做了一些调整。请参阅[v0.10 API迁移](/../gao-ji-yong-fa/v010-apiqian-yi.html)升级你的应用。

`Entity`的方法返回的实体对象被表示为不可变的[DraftEntityInstance](https://github.com/facebook/draft-js/blob/master/src/model/entity/DraftEntityInstance.js)记录。这些记录拥有一组简单的getter方法，只能用于检索数据。

### 概览

方法

* [create\(...\): DraftEntityInstance](#create)
* [add\(instance: DraftEntityInstance\): string](#add)
* [get\(key: string\): DraftEntityInstance](#get)
* [mergeData\(...\): DraftEntityInstance](#mergedata)
* [replaceData\(...\): DraftEntityInstance](#replacedata)

### 方法介绍

#### create_\(该方法已弃用，推荐_[_contentState.createEntity_](/../apican-kao/contentstate.html#createentity)_\)_ {#create}

```js
create(
  type: DraftEntityType,
  mutability: DraftEntityMutability,
  data?: Object
): string
```

`create`方法用于创建带有提供的属性的实体对象。

注意，该方法返回一个字符串。因为实体在`ContentState`中通过字符串key来引用。这个字符值应该在`CharacterMetadata`对象中用来引用带注释字符的实体。

#### add\(该方法已弃用，推荐[contentState.addEntity](/../apican-kao/contentstate.html#addentity)\) {#add}

```js
add(instance: DraftEntityInstance): string
```

大多数情况下，你会使用`Entity.create()`。这是一个传统Draft用法中可能不需要的简便方法。

`add`方法在实例已经被创建好，并且需要被添加到`Entity`存储区的情况下非常有用。这种情况常常在我们使用原生JS表示的`ContengState`对象正在被恢复编辑的时候发生。

#### get\(该方法已弃用，推荐[contentState.getEntity](/../apican-kao/contentstate.html#getentity)\) {#get}

```js
get(key: string): DraftEntityInstance
```

返回指定key对应的`DraftEntityInstance`。如果指定key对应的实例不存在则抛出异常。

#### mergeData\(该方法已弃用，推荐[contentState.mergeEntityData](/../apican-kao/contentstate.html#mergeentitydata)\) {#mergedata}

```js
mergeData(
  key: string,
  toMerge: {[key: string]: any}
): DraftEntityInstance
```

由于`DraftEntityInstance`对象是不可变的，你不能通过传统的修改方法来更新一个实体的元数据。

`mergeData`方法允许你再指定的实体上应用更新。

#### replaceData\(该方法已弃用，推荐[contentState.replaceEntityData](/../apican-kao/contentstate.html#replaceentitydata)\) {#replacedata}

```js
replaceData(
  key: string,
  newData: {[key: string]: any}
): DraftEntityInstance
```

`replaceData`方法与`mergeData`方法类似，只是`replaceData`方法会丢弃实例中存在的所有`data`，替换为指定的`newData`。

