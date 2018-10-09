## 实体（处理内容中的富数据）

本文讨论了实体系统，该Draft用于使用元数据对文本的范围进行注释。 实体使工程师能够向编辑人员介绍超越样式文本的丰富程度。链接，提及和嵌入式内容都可以使用实体来实现。



\(文本中会有许多高级的元数据，实体就是用于表示这些元数据，使得我们可以在内容中添加链接，图片，等\)

在Draft知识库中，“[链接编辑器](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/link)”和“[实体演示](https://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/entity)”提供实时代码示例，以帮助澄清实体可以如何使用以及其内置行为。



“[实体API参考](https://draftjs.org/docs/api-reference-entity.html)”提供了创建，检索或更新实体对象时要使用的静态方法的详细信息。



有关实体API最近更改的信息以及如何更新应用程序的示例，请参阅我们的[v0.10 API迁移指南](https://draftjs.org/docs/v0-10-api-migration.html#content)。

## 介绍

实体是表示Draft编辑器中文本范围的元数据的对象。 它有三个属性：

* **type** : 一个字符串，表示它是什么样的实体，例如`'LINK'`，`'MENTION'`，`'PHOTO'`。
* **mutability** : 不要将不可变性与`immutable-js`混淆，此属性表示在编辑器中编辑文本范围时，使用此实体对象注释的一系列文本的行为。 这在下面更详细地讨论。
* **data** : 包含实体元数据的可选对象。 例如，`“LINK”`实体可能包含包含该链接的`href`值的数据对象。类型，替换， 元数据的对象

所有实体都存储在ContentState记录中。所有的实体通过ContentState key 引用， 并且React组件用于描述替换范围。（我们目前不赞成使用以前的API访问实体;请参阅问题＃839。）

使用装饰器或自定义块组件，您可以根据实体元数据向编辑器添加丰富的渲染。



