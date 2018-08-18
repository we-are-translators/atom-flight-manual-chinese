# 查找和替换

在Atom中快速轻松地查找和替换文件或项目中的文本。

* <kbd>Ctrl+F</kbd> - 在缓存区内搜索
* <kbd>Ctrl+Shift+F</kbd> - 搜索整个项目

如果你启动其中任何一个命令，你将会看到屏幕底部的“查找和替换”面板。

![https://flight-manual.atom.io/using-atom/images/find-replace-file.png](https://flight-manual.atom.io/using-atom/images/find-replace-file.png)

要在当前文件中搜索，可以按<kbd>Ctrl + F</kbd>.输入搜索字符串并按<kbd>Enter</kbd>(或<kbd>F3</kbd>或者"Find Next"按钮)多次循环遍历该文件中的所有匹配项。“查找和替换”面板还包含用于切换区分大小写，执行正则表达式匹配以及将搜索范围限定为选择的按钮。

如果在替换文本框中键入字符串，则可以使用其他字符串替换匹配项。例如，如果要将字符串“Scott”的每个实例替换为字符串“Dragon”，则应在两个文本框中输入这些值，然后按“全部替换”按钮执行替换。

> 注意：Atom使用JavaScript正则表达式来执行正则表达式搜索。

> 在进行正则表达式搜索时，引用回搜索组的替换语法是$ 1，$ 2，... $＆。请参阅JavaScript的正则表达式指南，以了解有关可在Atom中使用的[正则表达式语法](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Regular_Expressions)的更多信息。

如果你调用面板，也可以在整个项目中查找和替换
<kbd>Ctrl+Shift+F</kbd>

![https://flight-manual.atom.io/using-atom/images/find-replace-project.png](https://flight-manual.atom.io/using-atom/images/find-replace-project.png)

这是一个很好的方法，可以找出项目中调用函数的位置，链接到锚点还是找到特定的拼写错误。单击匹配的行以跳转到该文件中的该位置。

通过在“文件/目录模式”文本框中输入[glob模式](https://en.wikipedia.org/wiki/Glob_%28programming%29)，可以将搜索限制为项目中文件的子集。例如，模式`src / * .js`会将搜索限制为`src`目录中的javascript文件。“globstar”模式（`**`）可用于匹配任意多个子目录。例如，`docs / ** / *.md`将匹配`docs / a / foo.md`，`docs / a / b / foo.md`等。您可以输入多个用逗号分隔的glob模式，这对于多个搜索很有用文件类型或子目录。

打开多个项目文件夹时，此功能也可用于仅搜索其中一个文件夹。例如，如果打开文件夹`/ p​​ath1 / folder1`和`/ path2 / folder2`，则可以输入以`folder1`开头的模式，仅在第一个文件夹中搜索。

在聚焦“查找和替换”面板的同时按<kbd>Esc</kbd>以清除工作区中的窗格。

查找和替换功能在[查找和替换](https://github.com/atom/find-and-replace)程序包中实现，并使用[scandal](https://github.com/atom/scandal)节点模块进行实际搜索。
