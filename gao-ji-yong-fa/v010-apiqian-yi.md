## v0.10 API迁移

Draft.js v0.10发行版包含对用于管理`DraftEntity`数据的API的更改; 全局“`DraftEntity`”模块已被弃用，`DraftEntity`实例将作为`ContentState`的一部分进行管理。这意味着以前在`DraftEntity`上访问的方法现在被移动到`ContentState`记录。

该API改进解决了v0.11中可用的许多优点的路径：

* DraftEntity实例和存储将是不可变的。
* DraftEntity将不再可全局访问。
* 对实体数据的任何更改将触发重新渲染。

## 概要

以下简要列出已更改的内容以及如何更新应用程序：

## 创建一个实体

### 旧语法

```js
const entityKey = Entity.create(
  urlType,
  'IMMUTABLE',
  {src: urlValue},
);
```

### 新语法

```js
const contentStateWithEntity = contentState.createEntity(
  urlType,
  'IMMUTABLE',
  {src: urlValue},
);
const entityKey = contentStateWithEntity.getLastCreatedEntityKey();
```

## 获取一个实体

### 旧语法

```js
const entityInstance = Entity.get(entityKey);
// entityKey is a string key associated with that entity when it was created
```

### 新语法

```js
const entityInstance = contentState.getEntity(entityKey);
// entityKey is a string key associated with that entity when it was created
```

## 修饰器策略参数变化

### 旧语法

```js
const compositeDecorator = new CompositeDecorator([
  {
    strategy: (contentBlock, callback) => exampleFindTextRange(contentBlock, callback),
    component: ExampleTokenComponent,
  },
]);
```

### 新语法

```js
const compositeDecorator = new CompositeDecorator([
  {
    strategy: (
      contentBlock,
      callback,
      contentState
    ) => exampleFindTextRange(contentBlock, callback, contentState),
    component: ExampleTokenComponent,
  },
]);
```

注意ExampleTokenComponent将接收一个contentState prop。

