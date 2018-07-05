# 编辑和删除文本

到目前为止，我们已经了解了许多在文件中移动的方式和文件中选择区域的方式，不过现在让我们实际更改一些它们的文本。很明显，你可以通过输入来插入字符，不过也有许多可能会用到的方式来删除和修改文本。

## 基础操作

对于基本的文本操作可能会用到一些酷炫的快捷绑定。这些范围从围绕文本行移动和复制行来更改文件内容。

* <kbd>Ctrl+J</kbd> - 连接下一行到当前行的末尾
* <kbd>Ctrl+Up/Down</kbd> - 向上或向下移动当前行
* <kbd>Ctrl+Shift+D</kbd> - 复制当前行
* <kbd>Ctrl+K</kbd> <kbd>Ctrl+U</kbd> 使当前单词大写
* <kbd>Ctrl+K</kbd> <kbd>Ctrl+L</kbd> 使当前单词小写

Atom also has built in functionality to re-flow a paragraph to hard-wrap at a given maximum line length. You can format the current selection to have lines no longer than 80 (or whatever number editor.preferredLineLength is set to) characters using Alt+Ctrl+Q. If nothing is selected, the current paragraph will be reflowed.

在给定的最大行字符数的位置，Atom 也有内置方法来强制换行重开一个段落。
