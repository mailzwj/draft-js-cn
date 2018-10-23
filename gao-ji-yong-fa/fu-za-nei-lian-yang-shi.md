## 复杂内联样式

在编辑器中，除了提供基础的加粗\(bold\)、斜体\(italic\)、下划线\(underline\)等行内样式，你可能还希望能提供更多种多样的行内样式。例如，你想要支持多种多样的颜色\(color\)、字体\(font-family\)、字号\(font-size\)等等。另外，你所想要的样式可能会叠加，也可能互相排斥。

[Rich Editor](http://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/rich)和[Colorful Editor](http://github.com/facebook/draft-js/tree/master/examples/draft-0-10-0/color)两个实例展示了如何使用复杂的内联样式。

## Model 模型

在Draft模型中，行内样式体现在字符级别上，使用一个不可变的`OrderedSet`对象来定义应用于每个字符的样式列表。这些样式通过字符串来识别（详情请参阅[CharacterMetadata](https://draftjs.org/docs/api-reference-character-metadata.html)）。

例如，编辑文本“Hello **world**”。字符串的前六个字符使用空集来表示，`OrderedSet()`。

