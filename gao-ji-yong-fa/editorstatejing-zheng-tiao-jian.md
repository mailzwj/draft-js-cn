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

根据经验，应避免在相同的事件中使用不同的事件处理程序来处理`EditorState`。使用`setTimeout`来执行`setState`同样可能出现类似状况。任何时候你觉得你遇到了“losing state（状态丢失）”，请先确认你没有在`Editor`重新执行`render()`之前覆盖state。

### 最佳实践

现在我们已经明白了这个问题，我们可以做些什么来避免这种问题呢？通常需要注意我们从那里得到`EditorState`。如果你使用一个暂存的state（存储在`this.state`中），那么它有可能不是最新的。为了最小化该问题，Draft在许多callback函数中提供了最新的`EditorState`实例。在我们的代码中，应该使用编辑器提供的`EditorState`来代替暂存的state对象，来确保我们基于最新的`EditorState`进行修改。`Editor`上支持callback的列表如下：

* `handleReturn(event, editorState)`
* `handleKeyCommand(command, editorState)`
* `handleBeforeInput(chars, editorState)`
* `handlePastedText(text, html, editorState)`

粘贴内容的示例可以使用这些方法进行重写，以达到更好的效果：

```js
this.handlePastedText = (text, styles, editorState) => {
  this.setState({
    editorState: removeEditorStyles(text, editorState)
  });
}
//...
<Editor
  editorState={this.state.editorState}
  onChange={this.onChange}
  handlePastedText={this.handlePastedText}
  placeholder="Enter some text..."
/>
```

在`handlePastedText`中，我们可以用自己的方式实现粘贴功能。

注意：如果你想让你的编辑器拥有这个功能，还有一个更加简便的方式可以达到目的。只需将`Editor`的`stripPastedStyles`属性设置为`true`。

