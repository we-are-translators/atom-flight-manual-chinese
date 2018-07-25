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
