# Atom 中的移动

虽然用鼠标点击或使用箭头键在 Atom 中移动非常简单，不过有一些快捷键绑定能帮助你解放双手并更快定位到相关内容附近。

Atom 装载了许多基础的 Emacs 快捷键绑定来导航文档。要向上和向下单个字符，你可以用 <kbd>Ctrl+P</kbd> 和 <kbd>Ctrl+N</kbd>。要向左和向右单个字符，你可以用 <kbd>Ctrl+B</kbd> 和 <kbd>Ctrl+F</kbd>。这与使用箭头键是等价的，不过有些人不喜欢把他们的手放在键盘的方向键的位置。

除了单字符移动，还有许多其他的移动快捷键绑定：

* <kbd>Alt+Left</kbd> 或 <kbd>Alt+B</kbd> - 移动到单词的起始位置
* <kbd>Alt+Right</kbd> 或 <kbd>Alt+F</kbd> - 移动到单词的结束位置
* <kbd>Cmd+Left</kbd> 或 <kbd>Ctrl+A</kbd> - 移动到当前行的第一个字符位置
* <kbd>Cmd+Right</kbd> 或 <kbd>Ctrl+E</kbd> - 移动到当前行的结束位置
* <kbd>Cmd+Up</kbd> - 移动到文件的顶部位置
* <kbd>Cmd+Down</kbd> - 移动到文件的底部位置

你也可以直接按 <kbd>Ctrl+G</kbd> 移动到指定的行（和列）数。这会弹出一个对话框问你想要跳转到哪一行。你也可以用 `row:column` 语法跳转到该行中的字符位置。

![Go directly to a line](https://flight-manual.atom.io/using-atom/images/goto.png)

## 其它的移动命令和选择命令

Atom 也有一些移动命令和选择命令默认没有快捷键绑定。你可以在 [命令调色板](/linux/chapter1/atom-basics?id=命令调色板) 中访问这些命令，但是如果你发现你经常使用的命令没有快捷键绑定，不要担心！你可以很容易添加一个入口到你的 `keymap.cson` 来创建一个组合键。你可以从 * Atom > keymap * 菜单打开 `keymap.cson` 文件到编辑器中。

比如，命令 `editor:move-to-beginning-of-screen-line` 在命令调色板中是可用的，但是它没有绑定到任何组合键上。要创建一个组合键你需要在 `keymap.cson` 文件中添加一个入口。对于 `editor:select-to-previous-word-boundary`，你可以添加下面的命令到 `keymap.cson`:

```cson
'atom-text-editor':
  'ctrl-shift-e': 'editor:select-to-previous-word-boundary'
```

这将绑定命令 `editor:select-to-previous-word-boundary` 到 <kbd>Cmd+Shift+E</kbd>。获取更多关于自定义快捷键的信息，查看 [自定义快捷键绑定](linux/chapter2/basic-customization?id=自定义快捷键绑定) 。

这是一份默认没有快捷绑定的移动命令和选择命令列表：

```
editor:move-to-beginning-of-next-paragraph
editor:move-to-beginning-of-previous-paragraph
editor:move-to-beginning-of-screen-line
editor:move-to-beginning-of-line
editor:move-to-beginning-of-next-word
editor:move-to-previous-word-boundary
editor:move-to-next-word-boundary
editor:select-to-beginning-of-next-paragraph
editor:select-to-beginning-of-previous-paragraph
editor:select-to-beginning-of-line
editor:select-to-beginning-of-next-word
editor:select-to-next-word-boundary
editor:select-to-previous-word-boundary
```

## 标记定位

你也可以通过标记界面围绕更多信息地跳转。要跳转到一个方法定义的标记位置，按 <kbd>Cmd+R</kbd>。这会打开当前文件中所有标记的列表，你可以在其中进行类似 <kbd>Cmd+T</kbd>模糊查询。你也可以在项目中进行标记查询，不过这需要一个 `tags` 文件。

![Search by symbol across your project](https://flight-manual.atom.io/using-atom/images/symbol.png)

你可以用 [ctags utility](https://ctags.io/) 生成 `tags` 文件。一旦安装完毕，你可以用它通过运行命令来生成 `tags` 文件。查看 [ctags documentation](https://docs.ctags.io/en/latest/) 获取更多信息。

一旦你已经生成了 `tags` 文件，你可以按 <kbd>Cmd+Shift+R</kbd> 来进行跨项目的标记搜索。这也可以使你用 <kbd>Alt+Cmd+Down</kbd> 转到声明处以及用 <kbd>Alt+Cmd+Up</kbd> 从光标下的标记声明处返回。

你可以通过在你的 home 目录，`~/.ctags` 创建你自己的 `.ctags` 文件来自定义创建多少 tags。可以在 [这里](https://github.com/atom/symbols-view/blob/master/lib/ctags-config) 找到一个示例。

标记导航功能在 [symbols-view](https://github.com/atom/symbols-view) 包中实现。

## 书签

Atom 也有一个非常好的方式来给你项目中的指定行添加书签，所以你可以快速的跳回到它们。

如果你按 <kbd>Cmd+F2</kbd>，Atom 将在当前行切换 "bookmark"。你可以通过项目设置这些书签以及用它们来快速查找和跳转到你项目的重要行。在行的凹槽里会添加一个小书签标记，像 [下面图片](/linux/chapter2/moving-in-atom?id=bookmarks-image) 中的 22 行。

如果你按 <kbd>F2</kbd>, Atom 将转到在你当前聚焦的文件中的下一个书签。如果你使用 <kbd>Shift+F2</kbd> 将在它们中反向循环。

你也可以通过按 <kbd>Ctrl+F2</kbd> 查看当前项目的所有书签列表以及快速查找书签和跳转到任意书签。

<a id="bookmarks-image"></a>
![View and filter bookmarks](https://flight-manual.atom.io/using-atom/images/bookmarks.png)

书签功能在 [bookmarks package](https://github.com/atom/bookmarks) 中实现。
