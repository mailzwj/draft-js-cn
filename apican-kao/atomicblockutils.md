## AtomicBlockUtils

`AtomicBlockUtils`模块是用于原子块\(atomic block\)编辑的一组实用静态方法的集合。

在使用中，这些方法接收具有关联参数的`EditorState`对象，并返回`EditorState`对象。

### 静态方法

#### insertAtomicBlock

```js
insertAtomicBlock: function(
  editorState: EditorState,
  entityKey: string,
  character: string
): EditorState
```

#### moveAtomicBlock

```js
moveAtomicBlock: function(
  editorState: EditorState,
  atomicBlock: ContentBlock,
  targetRange: SelectionState,
  insertionMode?: DraftInsertionType
): EditorState
```



