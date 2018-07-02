# 进入 Atom

虽然通过单击鼠标或使用箭头键轻松移动Atom，但仍有一些键绑定可帮助您将手放在键盘上并更快地导航。

Atom支持所有标准的Windows光标移动键组合。要向上，向下，向左或向右移动单个字符，您可以使用箭头键。

除了单个角色移动之外，还有其他一些移动键绑定：
- `Ctrl + Left` - 移至文字的开头
- `Ctrl + Right` - 移至文字的结尾
- `Home` - 移至当前行的第一个字符
- `End` - 移动到行的末尾
- `Ctrl + Home` - 移到文件顶部
- `Ctrl + End` - 移到文件底部

您也可以使用`Ctrl + G`直接移动到特定的行（和列）编号。这将打开一个对话框，询问您要跳转到哪一行。您还可以使用**row：column**语法跳转到该行中的某个字符。

![https://flight-manual.atom.io/using-atom/images/goto.png](https://flight-manual.atom.io/using-atom/images/goto.png)

## 附加移动和选择命令

Atom也有一些移动和选择命令，默认情况下没有键绑定。您可以从[命令面板](https://flight-manual.atom.io/getting-started/sections/atom-basics/#command-palette)访问这些命令，但是如果您发现自己使用的命令经常没有键绑定，请不要担心！您可以轻松地在`keymap.cson`中添加条目以创建组合键。您可以在*File> Keymap*菜单中的编辑器中打开`keymap.cson`文件。

例如，命令编辑器：移动到屏幕开始行可在命令选项板中使用，但不绑定到任何组合键。要创建组合键，您需要在`keymap.cson`文件中添加一个条目。对于编辑器：选择到前一个单词边界，可以将以下内容添加到您的`keymap.cson`中：

```
'atom-text-editor':
  'ctrl-shift-e': 'editor:select-to-previous-word-boundary'
```

这将绑定命令`editor：select-to-previous-word-boundary` 和 `Ctrl + Shift + E.有关自定义键绑定的详细信息，请参阅[自定义键绑定](https://flight-manual.atom.io/using-atom/sections/basic-customization/#customizing-keybindings)。

以下是默认情况下没有键盘快捷键的移动和选择命令列表：

```
editor:move-to-beginning-of-next-paragraph
editor:move-to-beginning-of-previous-paragraph
editor:move-to-beginning-of-screen-line
editor:move-to-beginning-of-line
editor:move-to-end-of-line
editor:move-to-first-character-of-line
editor:move-to-beginning-of-next-word
editor:move-to-previous-word-boundary
editor:move-to-next-word-boundary
editor:select-to-beginning-of-next-paragraph
editor:select-to-beginning-of-previous-paragraph
editor:select-to-end-of-line
editor:select-to-beginning-of-line
editor:select-to-beginning-of-next-word
editor:select-to-next-word-boundary
editor:select-to-previous-word-boundary
```

## 按符号导航

您还可以使用符号视图进行更多信息。要跳转到诸如方法定义之类的符号，请按`Ctrl + R`。这会打开当前文件中所有符号的列表，您可以使用与`Ctrl + T`类似的模糊过滤器。您也可以在整个项目中搜索符号，但它需要一个`标签`文件。

![https://flight-manual.atom.io/using-atom/images/symbol.png](https://flight-manual.atom.io/using-atom/images/symbol.png)

您可以使用[ctags实用程序](https://ctags.io/)生成`标签`文件。安装完成后，可以通过运行命令生成标签文件来生成`标签`文件。有关详细信息，请参阅[ctags文档](https://docs.ctags.io/en/latest/)。

您可以通过在主目录`％USERPROFILE％\ .ctags`中创建自己的`.ctags`文件来定制如何生成标签。一个例子可以在[这里](https://github.com/atom/symbols-view/blob/master/lib/ctags-config)找到。

符号导航功能在[符号视图](https://github.com/atom/symbols-view)包中实现。

## 书签

Atom还有一种很好的方法可以为项目中的特定行添加书签，这样您就可以快速跳回到它们。

如果按`Alt + Ctrl + F2`，Atom将在当前行上切换“书签”。您可以在整个项目中对其进行设置，并使用它们快速查找并跳转到项目的重要部分。一个小书签符号被添加到线槽，就像下面的[图像](https://flight-manual.atom.io/using-atom/sections/moving-in-atom/#bookmarks-image)的第22行。

如果您按`F2`，则Atom将跳转到您当前所关注的文件中的下一个书签。如果您使用`Shift + F2`，它将向后循环。

您还可以看到所有项目当前书签的列表，并快速过滤它们并通过按`Ctrl + F2`跳转到它们中的任何一个。

![https://flight-manual.atom.io/using-atom/images/bookmarks.png](https://flight-manual.atom.io/using-atom/images/bookmarks.png)

书签功能在[书签](https://github.com/atom/bookmarks)包中实现。
