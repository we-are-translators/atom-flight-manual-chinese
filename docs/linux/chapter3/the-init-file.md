# 初始化文件

当 Atom 完成加载，它将在 `~/.atom` 目录读取 `init.coffee` 文件，这给你一个运行 CoffeeScript 代码的机会来做自定义功能。这个文件中的代码可以完全访问 [Atom's API](https://atom.io/docs/api/v1.28.2/AtomEnvironment)。如果自定义功能变得广泛使用，可以考虑创建一个插件包，我们将在 [包：字数统计](/linux/chapter3/package-word-count) 一节中详细讨论。

你可以从 *Edit > Init Script* 菜单在编辑器中打开 `init.coffee` 文件。这个文件也可以命名为 `init.js` 并且包含 JavaScript 代码。

比如，你启用了 Audio Beep 设置项，你可以添加下面的代码到你的 `init.coffee` 文件，这样一来，Atom 每次加载就会用音频蜂鸣向你问好。

```js
atom.beep()
```

因为 `init.coffee` 提供了访问 Atom's API 的权限，你可以不用创建新的插件包或者扩展已有的插件包来实现有用的命令。这是一个用 [Selection API](https://atom.io/docs/api/v1.28.2/Selection) 和 [Clipboard API](https://atom.io/docs/api/v1.28.2/Clipboard) 从选中的文本构造 Markdown 超链接并且用剪贴板内容作为该超链接的 URL 的命令：

```coffeescript
atom.commands.add 'atom-text-editor', 'markdown:paste-as-link', ->
  return unless editor = atom.workspace.getActiveTextEditor()

  selection = editor.getLastSelection()
  clipboardText = atom.clipboard.read()

  selection.insertText("[#{selection.getText()}](#{clipboardText})")
```

现在，重启 Atom 并使用 [命令调色板](/windows/chapter1/atom-basics?id=命令调色板) 来执行名叫 "Markdown:Paste As Link" 的新命令。如果你喜欢用键盘快捷键来使用命令，你可以定义一个 [快捷键绑定给这个命令](https://flight-manual.atom.io/using-atom/sections/basic-customization/#customizing-keybindings)。
