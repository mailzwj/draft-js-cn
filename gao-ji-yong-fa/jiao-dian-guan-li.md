## 焦点管理

管理文本输入焦点，在React组件中可能是一个比较棘手的问题。浏览器的focus/blur API是不可避免的，所以单纯的通过`render()`来处理往往比较困难，它需要尝试挑战控制focus的状态。

带着这种想法，在Facebook我们常常选择在包含文本输入的组件中暴露`focus()`方法。这打破了声明范式，但它也简化了工程师们在他们的App中成功管理焦点的工作。

`Editor`组件也遵循了这种模式，因此在组件中存在一个开放的`focus()`方法。这允许你必要的时候，在父级组件中通过ref直接调用组件的`focus()`方法。

组件中的事件监听器会观察焦点变化，并按照预期的将该变化传递到`onChange`中，以实现state和DOM保持同步。

### 将容器的点击事件转换为获得焦点

你的父组件很可能会以某种方式将`Editor`组建包裹在一个容器中，为了更加适配你的App布局，也许该容器还带有padding等样式。

一般情况下，当用户试图在该容器内的编辑器渲染区域以外的区域触发点击事件来让编辑器获得焦点，编辑器并不会响应点击事件。因此推荐你在父容器中添加点击事件监听器，并且使用前文提到的`focus()`方法来使编辑器获得焦点。

这个[文本编辑器示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/plaintext)，举例说明如何使用这种模式。

