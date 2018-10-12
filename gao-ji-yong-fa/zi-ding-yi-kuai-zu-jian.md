## 自定义块组件

Draft被设计来解决简单的富文本编辑问题，比如编辑评论和聊天消息等。但是它也有能力提供更丰富的富文本编辑体验，比如[Facebook Notes](https://www.facebook.com/notes/)。

用户可以从已存在的Facebook相册中选择或者重新上传图片来嵌入到他编辑的内容中。为了达到该目的，DraftJS支持自定义块级渲染方式，以纯文本的方式来渲染富媒体内容。

Draft仓库中提供了一个自定义块渲染的示例[TeX editor](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/tex)，使用[KaTeX库](https://khan.github.io/KaTeX/)将Tex语法转换成公式进行渲染并嵌入编辑器。

Draft还提供了一个[在编辑器中嵌入媒体的示例](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/media)，演示如何自定义渲染`audio`、`image`和`video`。

通过使用自定义块渲染器，你可以在你的编辑器中加入许多复杂的交互。

### 自定义块组件



