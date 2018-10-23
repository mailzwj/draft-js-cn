## EditorState竞争条件

Draft`Editor`是一个可控的输入组件（详情见[API Basics](/../kuai-su-kai-shi/ji-chu-api.html)）。这意味着`Editor`中任何状态的改变，都将通过`onChange`向上传递，直到应用程序将其反馈到`Editor`组件。

这个周期通常看起来像：

```js
...
this.onChange = function(editorState) {
  this.setState({editorState: editorState});
}
...
<Editor
  editorState={this.state.editorState}
  onChange={this.onChange}
  placeholder="Enter some text..."
/>
```

不同的浏览器事件可以触发`Editor`组件创建一个新的状态，并调用`onChange`。例如，当用户粘贴文本到编辑器中，Draft会解析新的内容，并创建必要的数据结构来表示这些内容。

这个周期没有问题，但是由于`setState`的调用，它将是一个异步操作。这使得设置状态到使用新的状态渲染`Editor`中的内容期间产生了延迟。在此期间，可以执行其他的JS代码。

![](/assets/editorstate-race-condition-1-handler.png)

想这种非完整过程的操作，有可能引起优先级问题。举个例子：假设你想移除粘贴内容中的全部样式。这可通过监听`onPaste`事件来实现移除`EditorState`中的全部样式：

```js
this.onPaste = function() {
  this.setState({
    editorState: removeEditorStyles(this.state.editorState)
  });
}
```

然而，它和我们期望的并不一样。现在有两个事件处理器可以在完全相同的浏览器事件中设置一个新的`EditorState`。由于事件处理器将一个接一个的执行，因此只有最后一个`setState`的结果才会有效。JS执行顺序大致如下：![](/assets/editorstate-race-condition-2-handlers.png)

如你所见，由于`setState`是一个异步操作，第二个`setState`将覆盖第一个`setState`中做的任何设置，导致编辑器丢失所有被粘贴的内容。

你可以通过[该示例](https://jsfiddle.net/qecccw3r/)观察和探索优先级问题。该示例还通过日志来突出JS执行顺序，因此你可以打开开发者工具查看详细信息。

