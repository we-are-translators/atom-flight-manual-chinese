# 包：修改文本

现在我们已经写了我们的第一个包了，让我们看看我们能做的其它类型的包的示例。本章将指导你创建一个用 [ascii art](https://en.wikipedia.org/wiki/ASCII_art) 替换选中文本的简单指令。当你选中 "cool" 单词运行我们的新指令，它将被替换为：

```
                                   o888
  ooooooo     ooooooo     ooooooo   888
888     888 888     888 888     888 888
888         888     888 888     888 888
  88ooo888    88ooo88     88ooo88  o888o
```

这应该演示如何在当前的文本缓冲区中做基本的文本操作以及如何处理选中的文本。

最终完成的包可以在 [https://github.com/atom/ascii-art](https://github.com/atom/ascii-art) 查看。

## 基础文本插入

首先，按 <kbd>Cmd+Shift+P</kbd> 打开 [指令面板](https://github.com/atom/command-palette)。输入 "generate package" 并选择 "Package Generator: Generate Package" 指令，就像我们在 [包生成器一节](linux/chapter3/package-word-count?id=包生成器) 中做的那样。输入 `ascii-art` 作为包名。

现在让我们编辑包文件来让我们的 ASCII Art 包做一些有趣的事。由于该包不需要任何 UI，我们可以移除所有视图相关的代码，因此我们继续并删除 `lib/ascii-art-view.js` ，`spec/ascii-art-view-spec.js` ，以及 `styles/` 。

下一步，打开 `lib/ascii-art.js` 并移除所有视图相关的代码，所以它看起来像下面这样：

```js
const {CompositeDisposable} = require('atom')

module.exports = {
  subscriptions: null,

  activate () {
    this.subscriptions = new CompositeDisposable()
    this.subscriptions.add(atom.commands.add('atom-workspace',
      {'ascii-art:convert': () => this.convert()})
    )
  },

  deactivate () {
    this.subscriptions.dispose()
  },

  convert() {
    console.log('Convert text!')
  }
}
```

### 创建一个指令

现在让我们添加一个指令。你应该用包名跟 `:` 再跟指令名的格式来命名你的指令。正如在代码中你看到的，我们叫我们的指令为 `ascii-art:convert`，我们将定义当它执行时调用 `convert()` 方法。

至此，该方法将简单地在控制台输出日志。让我们开始让它插入一些内容到文本缓冲区中。

```js
convert() {
  const editor = atom.workspace.getActiveTextEditor()
  if (editor) {
    editor.insertText('Hello, World!')
  }
}
```

与在 [统计字数](/linux/chapter3/package-word-count?id=统计字数) 中一样，我们使用 `atom.workspace.getActiveTextEditor()` 来获取表示活动文本编辑器对象。如果有选中的文本，它将用 "Hello, World!" 文本替换所有选中的文本。

### 重新加载包

在我们能够触发 `ascii-art:convert` 指令之前，我们需要重新加载窗口来为我们的包加载最新的代码。从指令面板运行指令 "Window: Reload" 或者按 <kbd>Alt+Cmd+Ctrl+L</kbd> 。

### 触发命令

现在打开指令面板并搜索 "Ascii Art: Convert" 指令。但是它并不在那里！为了修复这个问题，打开 `package.json` 并找到叫做 `activationCommands` 的属性。激活指令通过允许 Atom 延迟包的激活直到它需要被激活的时候才激活，这样可以使得 Atom 更快地启动。所以移除已经存在的指令并在 `activationCommands` 中使用 `ascii-art:convert` ：

```json
"activationCommands": {
  "atom-workspace": "ascii-art:convert"
}
```

首先，从指令面板运行指令 "Window: Reload" 重新加载窗口。现在，当你运行 "Ascii Art: Convert" 指令时，它将插入 "Hello, World!" 到活动的编辑器中，如果有的话。

### 添加快捷键绑定

现在让我们添加一个快捷键绑定来触发 `ascii-art:convert` 指令。打开 `keymaps/ascii-art.json` 并添加快捷绑定链接 <kbd>Alt+Ctrl+A</kbd> 到 `ascii-art:convert` 指令。你可以删除已经存在的快捷键绑定，因为你不再需要它了。

```json
{
  "atom-text-editor": {
    "ctrl-alt-a": "ascii-art:convert"
  }
}
```

现在重新加载窗口并验证快捷绑定的工作是否正常。

> **警告**：Atom 快捷键映射系统是 *区分大小写的* 。这意味着创建快捷键绑定的时候 `a` 和 `A` 是有区别的。`a` 表示你想要触发当你按 <kbd>A</kbd> 时的快捷键绑定。但是 `A` 表示你想要触发当你按 <kbd>Shift+A</kbd> 时的快捷键绑定。当你想要触发在你按 <kbd>Shift+A</kbd> 时的快捷键绑定，你也可以写 `shift-a` 。

我们 **强烈** 推荐当你想包含 <kbd>Shift</kbd> 到你的快捷绑定时，总是使用小写并明确地拼写单词。

## 添加 ASCII Art

现在我们需要转换选中的文本为 ASCII art 。要做这件事情我们将从 [npm](https://www.npmjs.com/) 使用 [figlet](https://www.npmjs.com/package/figlet) Node 模块。打开 `package.json` 并添加 figlet 最新版本到依赖项 ：

```json
"dependencies": {
  "figlet": "1.0.8"
}
```

保存文件后，从指令面板运行指令 "Update Package Dependencies: Update" 。这将安装包的 node 模块依赖项，在这个案例中只有 figlet 。每当你在 `package.json` 文件中更新依赖字段，你需要运行 "Update Package Dependencies: Update" 。

如果因为某些原型导致其无法正常工作，你将看到一条消息说 "Failed to update package dependencies" 并且你将在你的目录中发现一个新的 `npm-debug.log` 文件。该文件应该提供你一些关于哪里出错的信息。

现在在 `lib/ascii-art.js` 中加载 figlet 的 node 模块并替换 "Hello, World!"，转换选中的文本为 ASCII art 。

```js
convert () {
  const editor = atom.workspace.getActiveTextEditor()
  if (editor) {
    const selection = editor.getSelectedText()

    const figlet = require('figlet')
    const font = 'o8'
    figlet(selection, {font}, function (error, art) {
      if (error) {
        console.error(error)
      } else {
        editor.insertText(`\n${art}\n`)
      }
    })
  }
}
```

现在重新加载编辑器，在编辑器窗口中选中一些文本并按 <kbd>Alt+Ctrl+A</kbd> 。那些文本应该被替换为荒谬的 ASCII art 版本。

这个示例中有几个新事物，我们应该快速地看一看。第一个是  `editor.getSelectedText()` ，你可能已经猜到了，该方法返回当前选中的文本。

然后，我们调用 Figlet 代码来转换选中的文本为其它文本并调用 `editor.insertText()` 方法替换选中的文本。

## 小结

在本章节中，我们已经编写了一个无 UI 包，这个包获取选中的文本并用一个处理过的版本替换选中的文本。这在为你的代码创建 linters 或 checkers 工具中可能是非常有用的。
