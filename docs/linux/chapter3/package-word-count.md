# 包：字数统计

让我们从编写一个非常简单的包开始，看看一些高效开发包所需要的工具。我们将开始编写一个包，它会告诉你在当前缓冲区中有多少字并显示在一个小的模态窗口中。

## 包生成器

创建包最简单的方式是使用 Atom 内置的包生成器。如你所料，这个生成器自身是一个独立实现于 [package-generator](https://github.com/atom/package-generator) 的包。

你可以调用指令面板并搜索 "Generate Package" 来运行包生成器。然后将会弹出一个对话框要求你给新项目命名。我们命名它为 `your-name-word-count`。Atom 然后将创建那个目录并生成一个基础的项目框架，它会连接到你的 `~/.atom/packages` 目录，所以当你下次打开编辑器时便会加载这个新创建的包。

> 注意：你可能会遇到包无法加载的问题。这是因为新的包使用与实际托管在 [atom.io](https://atom.io/packages) 的包一样的名字（比如 "wordcount" 和 "word-count"）是无法如你预期加载的。如果你按照我们的建议，使用上面的 `your-name-word-count` 包名字，那就没有问题 :grinning:

![Basic generated Atom package](https://flight-manual.atom.io/hacking-atom/images/package.png)

你可以看到 Atom 已经创建了一打构成包的文件。让我们看一看每一个文件来了解包是怎样构成的，然后我们可以修改它们来实现我们的字数统计功能。

基本的包布局如下：

```
my-package/
├─ grammars/
├─ keymaps/
├─ lib/
├─ menus/
├─ spec/
├─ snippets/
├─ styles/
├─ index.js
└─ package.json
```

不是每个包都有（或需要）所有这些目录，而且包生成器不会创建 `snippets` 或 `grammars`。为了愉快地与它们玩耍，让我们看看这些是什么吧。

### package.json

与 [Node modules](https://en.wikipedia.org/wiki/Npm_(software)) 一样，Atom 包在它们的顶级目录中包含一个 `package.json` 文件。这个文件包含关于包的元数据，比如 "main" 模块的路径，类库依赖，以及指定其应加载资源顺序的清单。

除了一些常规的 [Node `package.json` keys] 可以用之外，Atom 的 `package.json` 文件有自己的扩展。

* `main`：包入口点的 JavaScript 文件路径。如果没有指定，Atom 默认会查找 `index.coffee` 或 `index.js`。
* `styles`：一个表示包需要加载的样式表的顺序的字符串数组。如果不指定，`styles` 目录中的样式表按字母表顺序加载。
* `keymaps`：一个表示包需要加载的快捷键映射的顺序的字符串数组。如果不指定，`keymaps` 目录中的映射按字母表顺序加载。
* `menus`：一个表示包需要加载的菜单映射的顺序的字符串数组。如果不指定，`menus` 目录中的映射按字母表顺序加载。
* `snippets`：一个表示需要加载的代码片段的顺序的字符串数组。如果不指定，`snippets` 目录中的代码按字母表顺序加载。
* `activationCommands`：一个表示触发包激活的命令的对象。关键字是 CSS 选择器，值是一个表示命令的字符串数组。包的加载会延迟到 CSS 选择器定义的相关事件被触发。如果不指定，当包加载后将调用主文件中导出的 `activate()` 方法。
* `activationHooks`：一个表示触发包激活的钩子的字符串数组。包的加载会延迟到其中一个钩子被触发。目前，有两个激活钩子：`language-package-name:grammar-used`（比如，`language-javascript:grammar-used`）和（`core:loaded-shell-environment`）。

目前我们在包中刚生成的 `package.json` 文件看起来像这样：

```json
{
  "name": "wordcount",
  "main": "./lib/wordcount",
  "version": "0.0.0",
  "description": "A short description of your package",
  "activationCommands": {
    "atom-workspace": "wordcount:toggle"
  },
  "repository": "https://github.com/atom/your-name-word-count",
  "license": "MIT",
  "engines": {
    "atom": ">=1.0.0 <2.0.0"
  },
  "dependencies": {
  }
}
```

如果你想使用 activationHooks，你可能有：

```json
{
  "name": "wordcount",
  "main": "./lib/wordcount",
  "version": "0.0.0",
  "description": "A short description of your package",
  "activationHooks": ["language-javascript:grammar-used", "language-coffee-script:grammar-used"],
  "repository": "https://github.com/atom/your-name-word-count",
  "license": "MIT",
  "engines": {
    "atom": ">=1.0.0 <2.0.0"
  },
  "dependencies": {
  }
}
```

首先你应该确保这些信息已经填写了。名称，描述，项目所在的仓库 URL，以及许可协议都可以立即填写。其他信息我们随后会详细讨论。

> 警告：不要忘记更新仓库 URL。通过设计生成的默认仓库 URL 对你是无效的，直到更新了新的地址你才能发布你的包。

### 源代码

如果你想要扩展 Atom 的行为，你的包应该包含一个顶级模块，它可以从 `package.json` 中 `main` 关键字指定的任何一个文件中导出。在我们刚刚生成的包中，包的主文件是 `lib/wordcount.js`。其余的代码应该放在 `lib` 目录中，并且从你的顶级文件中加载。如果你的 `package.json` 文件中没有 `main` 关键字，它将寻找 `index.js` 或 `index.coffee` 作为主入口点。

你的包的顶级模块是一个单对象，它管理你的 Atom 扩展的生命周期。即使你的包创建十个不同的视图并把他们添加到 DOM 的不同部分，也都是从你的顶级对象管理的。

你的包的顶级模块可以实现下面的基本方法：

* `activate(state)`：当你的包激活后就会调用这个**可选的**方法。
