## 按键绑定

`Editor`组件为编辑器提供了灵活的绑定自定义按键的方法，可通过`keyBindingFn` prop来实现绑定。这允许您将自定义按键命令与编辑器中的组件行为进行绑定。

## 默认

默认的按键绑定函数是：`getDefaultKeyBinding`。

由于Draft框架保持了对DOM渲染和行为的严格控制，因此必须使用按键绑定系统来捕获和路由基础的编辑命令。

`getDefaultKeyBinding`将已知的操作系统级编辑器命令映射到`DraftEditorCommand`字符串，然后对应到组件处理程序中的行为。

比如，`Ctrl+Z`\(Win\)和`Cmd+Z`\(OSX\) 映射到`'undo'`命令，它控制处理程序执行`EditorState.undo()`。



