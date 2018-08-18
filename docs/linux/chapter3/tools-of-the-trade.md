# 开发工具

首先，我们假设你至少在一定程度上知道了解一些相关的技术知识。由于 Atom 是用 web 技术实现的，我们只能假设你了解诸如 JavaScript 和 CSS 之类的 web 技术。具体来说，我们将用 CoffeeScript 和 Less 来实现所有东西，它们分别是 JavaScript 和 CSS 预处理器。

如果你不了解 CoffeeScript，但是你熟悉 JavaScript，你也无需太过担心。这里是一些简单的 CoffeeScript 代码示例：

```coffeescript
MyPackageView = require './my-package-view'

module.exports =
  myPackageView: null

  activate: (state) ->
    @myPackageView = new MyPackageView(state.myPackageViewState)

  deactivate: ->
    @myPackageView.destroy()

  serialize: ->
    myPackageViewState: @myPackageView.serialize()
```

我们仔细看一下这个例子，这就是这门语言的样子。在 Atom 中几乎任何你能用 CoffeeScript 做的事情也都能用 JavaScript 来做。你可以在 [coffeescript.org](https://coffeescript.org/) 复习 CoffeeScript。

Less 是从 CSS 更简单转变。它添加了一些像变量和函数之类的有用的东西给 CSS。你可以在 [lesscss.org](http://lesscss.org/) 学习 Less。不过我们在本书中对于 Less 的使用不会过于复杂，你只要了解基本的 CSS 只是就可以了。
