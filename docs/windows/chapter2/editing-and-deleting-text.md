# 编辑和删除文本

到目前为止，我们已经了解了许多在文件中移动的方式和文件中选择区域的方式，不过现在让我们实际更改一些它们的文本。很明显，你可以通过输入来插入字符，不过也有许多可能会用到的方式来删除和修改文本。

## 基础操作

对于基本的文本操作可能会用到一些酷炫的快捷绑定。这些包括整行文本移动和复制行以及改变大小写。

* <kbd>Ctrl+J</kbd> - 连接下一行到当前行的末尾
* <kbd>Ctrl+Up/Down</kbd> - 向上或向下移动当前行
* <kbd>Ctrl+Shift+D</kbd> - 复制当前行
* <kbd>Ctrl+K</kbd> <kbd>Ctrl+U</kbd> 使当前单词大写
* <kbd>Ctrl+K</kbd> <kbd>Ctrl+L</kbd> 使当前单词小写

Atom 也有内置方法可以在给定的最大行字符数的位置，强制换行并重新排版。你可以用 <kbd>Alt+Ctrl+Q</kbd> 格式化当前选中行，使它每行不超过 80 个字符（或者 `editor.preferredLineLength` 中设置的任意数字）。如果没有选中的内容，那么将重新排版当前段落。

## 删除和剪切

你也可以用一些快捷键在缓冲区外删除或剪切文本。无须手下留情。

* <kbd>Ctrl+Shift+K</kbd> - 删除当前行
* <kbd>Ctrl+Backspace</kbd> - 删除到单词的起始位置
* <kbd>Ctrl+Delete</kbd> - 删除到单词的结束位置

## 多重光标和多重选取

Atom 可以做一件不拘一格而且非常酷炫的事，那就是支持多重光标。这在操作长文本列表中是极其有用的。

* <kbd>Ctrl+Click</kbd> - 在点击的位置添加一个新光标
* <kbd>Alt+Shift+Up/Down</kbd> - 在当前光标上面/下面添加另一个光标
* <kbd>Ctrl+D</kbd> - 选中文档中与当前选中单词一样的下一个单词
* <kbd>Alt+F3</kbd> - 选中文档中与当前选中单词一样的所有单词

使用这些命令，你可以在文档中的多个位置放置光标并一次性在多个地方有效地执行相同的命令。

![Using multiple cursors](https://flight-manual.atom.io/using-atom/images/multiple-cursors.gif)

这在做许多重复类型的任务中，例如重命名变量或更改一些文本的格式，是非常有用的。你可以结合几乎任何插件或命令来使用这些快捷键 - 比如，更改大小写以及移动或者复制行。

你也可以用鼠标选取文本结合按住 <kbd>Ctrl</kbd> 键来同时选取文本的多个区域。

## 空格

Atom 自带几个命令来帮助你管理文档中的空格。有一对非常有用的命令，它们可以转换前置空格为制表符以及转换前置制表符为空格。如果你正在一个有混合空格的文档中工作，这些命对帮助你格式化文件是非常好用的。空格命令没有快捷键绑定，所以你得在命令调色板搜索 "Convert Spaces to Tabs"（反之亦然）来运行其中一个命令。

空格命令在 [atom/whitespace](https://github.com/atom/whitespace) 包中实现。空格命令的设置在 `whitespace` 包的主页中进行管理。

![Managing your whitespace settings](https://flight-manual.atom.io/using-atom/images/whitespace.png)

> "Remove Trailing whitespace" 设置项默认是启用的。这表示在 Atom 中每次你保存文件时，它会过滤文件中所有末尾的空格。如果你想要禁用这个功能，在设置面板转到 `whitespace` 包并取消选中那个设置项。

Atom 也会默认确保你的文件末尾有一个新行。你也可以在那个界面禁用这个设置。

## 括号

Atom 有智能并且简单的括号处理方式。

当光标在它们上面时，它默认会高亮 `[]`，`()` 和 `{}` 样式的括号。它也会高亮匹配到的 XML 和 HTML 标签。

当你输入 `[]`，`()`，和 `{}`，`""`，`''`，`“”`，`‘’`，`«»`，`‹›`，和反引号开头的一个括号时，Atom 也会自动补完它们。如果你有一个选中的区域，你在这个区域上输入任何这些括号或引号的起始部分，Atom 会对这个区域用相应的括号或引号的结束部分将其包围起来。

还有一些其它关于括号的有趣命令可供你使用。

* <kbd>Ctrl+M</kbd> - 把光标跳转到相邻的配对括号位置。当没有相邻的括号时，则跳转到最近的闭合括号位置
* <kbd>Alt+Ctrl+,</kbd> - 选中当前括号中的所有文本
* <kbd>Alt+Ctrl+.</kbd> - 关闭当前 XML/HTML 标签

括号功能在 [bracket-matcher](https://github.com/atom/bracket-matcher) 包中实现。类似所有这些包，要更改默认相关的括号处理方式，或者要整个禁用它，你可以在设置界面里定位到这个包。

## 编码

Atom 也自带一些基本的文件编码，可能你发现你在非 UTF-8 编码的文件里工作，或者你希望创建一个 UTF-8 编码的文件。

* <kbd>Ctrl+Shift+U</kbd> - 切换菜单来更改文件编码

如果你停留在文件编码对话框，你可以选择一个其它的文件编码格式来保存你的文件。

当你打卡一个文件，Atom 会尝试自动检测文件编码。如果 Atom 不能识别编码，那么文件编码将默认为 UTF-8，这也是新文件的默认编码。

![Changing your file encoding](https://flight-manual.atom.io/using-atom/images/encodings.png)

如果你停留在编码菜单并更改了当前有效编码为其它编码格式，那么该文件将在下次保存时用那个编码格式保存文件。

编码选择器在 [encoding-selector](https://github.com/atom/encoding-selector) 包中实现。
