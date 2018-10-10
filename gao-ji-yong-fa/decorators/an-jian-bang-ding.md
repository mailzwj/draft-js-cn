## 按键绑定

`Editor`组件为编辑器提供了灵活的绑定自定义按键的方法，可通过`keyBindingFn` prop来实现绑定。这允许您将自定义按键命令与编辑器中的组件行为进行绑定。

## 默认

默认的按键绑定函数是：`getDefaultKeyBinding`。

由于Draft框架保持了对DOM渲染和行为的严格控制，因此必须使用按键绑定系统来捕获和路由基础的编辑命令。

`getDefaultKeyBinding`将已知的操作系统级编辑器命令映射到`DraftEditorCommand`字符串，然后对应到组件处理程序中的行为。

比如，`Ctrl+Z`\(Win\)和`Cmd+Z`\(OSX\) 映射到`'undo'`命令，它控制处理程序执行`EditorState.undo()`。

## 自定义

你可能会提供自己的事件绑定函数，来扩展自定义编辑器命令。

将`getDefaultKeyBinding`作为自定义函数处理失败的备选方案是一种被推荐的做法，这样可以让你的编辑器在默认命令中受益。

使用自定义命令字符串，你可以实现`handleKeyCommand` prop函数，它允许你映射命令字符串到你期望的行为上。如果`handleKeyCommand`返回`'handled'`，则说明命令已经被处理。反之，如果返回`'not-handled'`，则说明处理失败。

## 举例

假设我们有一个编辑器，它应该有一个“保存”机制来定期将您的内容保存到服务器上，作为Draft的备份。

首先，我们来定义我们的键绑定功能：

```js
import {getDefaultKeyBinding, KeyBindingUtil} from 'draft-js';
const {hasCommandModifier} = KeyBindingUtil;

function myKeyBindingFn(e: SyntheticKeyboardEvent): string {
  if (e.keyCode === 83 /* `S` key */ && hasCommandModifier(e)) {
    return 'myeditor-save';
  }
  return getDefaultKeyBinding(e);
}
```

我们的函数接收一个键盘事件参数，然后我们检查它是否与我们的标准匹配：它必须是`'S'`键，并且必须有一个命令修饰器。例如，OSX系统的`'Cmd'`键，或Windows系统的`'Ctrl'`键等。

