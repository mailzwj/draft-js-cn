## KeyBindingUtil

`KeyBindingUtil`模块是用于定义快捷键的一组实用静态方法的集合。

### 静态方法

#### isCtrlKeyCommand

```js
isCtrlKeyCommand: function(
  e: SyntheticKeyboardEvent
): boolean
```

检查`ctrlKey`是否与`altKey`一起使用。如果将它们组合在一起，结果将是`altGraph`，它不会被该快捷键处理。

#### isOptionKeyCommand

```js
isOptionKeyCommand: function(
  e: SyntheticKeyboardEvent
): boolean
```

#### usesMacOSHeuristics

```js
usesMacOSHeuristics: function(): boolean
```

检查是否在内部使用仅适用于macOS的触发方式，例如在确定组合键时使用Command键。

#### hasCommandModifier

```js
hasCommandModifier: function(
  e: SyntheticKeyboardEvent
): boolean
```



