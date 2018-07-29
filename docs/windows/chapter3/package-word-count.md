# 包：字数统计

让我们从编写一个非常简单的包开始，看看一些高效开发包所需要的工具。我们将开始编写一个包，它会告诉你在当前缓冲区中有多少字并显示在一个小的模态窗口中。

## 包生成器

创建包最简单的方式是使用 Atom 内置的包生成器。如你所料，这个生成器自身是一个独立实现于 [package-generator](https://github.com/atom/package-generator) 的包。

你可以调用指令面板并搜索 "Generate Package" 来运行包生成器。然后将会弹出一个对话框要求你给新项目命名。我们命名它为 `your-name-word-count`。Atom 然后将创建那个目录并生成一个基础的项目框架，它会连接到你的 `%USERPROFILE%\.atom\packages` 目录，所以当你下次打开编辑器时便会加载这个新创建的包。

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

与 [Node modules](https://en.wikipedia.org/wiki/Npm_(software) 一样，Atom 包在它们的根目录中包含一个 `package.json` 文件。这个文件包含关于包的元数据，比如 "main" 模块的路径，类库依赖，以及指定其应加载资源顺序的清单。

除了一些常规的 [Node `package.json` keys](https://docs.npmjs.com/files/package.json) 可用之外，Atom 的 `package.json` 文件还有自己的扩展。

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

* `activate(state)`：当你的包激活后就会调用这个**可选的**方法。如果你的模块实现了 `serialize()` 方法，它会传入最后一次窗口序列化的状态数据。当你的包启动了，可以用这个方法来做初始化工作（像设置 DOM 元素或绑定事件）。如果该方法返回一个 promise，那么包会等到 promise 为 resolves（或 rejects）时才加载。
* `initialize(state)`：（Atom 1.14 及以上可用）这个**可选的**方法与 `activate()` 类似，但是它更早被调用。然而激活发生在工作区被反序列化之后发生（因此它可以在[你的包反序列化](todu)后发生），，`initialize()` 方法确保会在任何事情之前被调用。如果你要确保工作区准备就绪，可以使用 `activate()` 方法；如果你需要在反序列化之前做一些设置或调用视图提供者，可以使用 `initialize()` 方法。
* `serialize()`：当窗口关闭时会调用这个**可选的**方法，允许你返回表示你组件状态的 JSON。 当之后恢复窗口时，数据会返回并传给你的模块的 `activate` 方法，所以你可以恢复视图到用户离开的地方。
* `deactivate()`：当窗口关闭时会调用这个**可选的**方法。如果你的包正在监视任意文件或者以任何其它方式持有外部资源，可以在这里释放它们。如果你只是在窗口订阅事物，你无需担心，因为无论如何它们都会被释放。

### 样式表

你的包的样式表应该放在 `styles` 目录。当你的包激活后，将加载任何在这个目录中的样式表并添加到 DOM 上。样式表可以用 CSS 或 [Less](http://lesscss.org/) 编写，不过推荐使用 Less 来编写。

理论上来说，你无需太多关于样式方面的知识。Atom 提供了与之无缝结合的标准套件来给任何包定义颜色和 UI 元素。你可以打开风格指南查看所有 Atom 的 UI 组件：按下 <kbd>Ctrl+Shift+P</kbd> 打开命令面板并搜索 `styleguide`，或者输入 <kbd>Ctrl+Shift+G</kbd>

如果你需要特殊的样式，可以尝试只在包样式表中保留结构样式。如果你必须指定颜色和大小，这些应该从活动主题的 [ui-variables.less](https://github.com/atom/atom-dark-ui/blob/master/styles/ui-variables.less) 中获取。

在你的 `package.json` 中有一个可选的 `styleSheets` 数组，它可以用名称列出样式表来指定加载顺序；否则，样式表按字母表顺序加载。

### 键映射

你可以给你的扩展的常用功能提供快捷键绑定，特别是你还添加了一个新指令。在我们的包中，我们已经在 `keymaps/wordcount.json` 中为我们填写了一个键映射：

```json
{
  "atom-workspace": {
    "ctrl-alt-o": "your-name-word-count:toggle"
  }
}
```

这意味着如果你按 <kbd>Alt+Ctrl+O</kbd>，我们的包将运行 `your-name-word-count:toggle` 指令。我们后面来看那部分代码，不过如果你想更改默认的键映射，你可以在这个文件中修改。

键映射放在 `keymaps` 子目录。默认情况，所有键映射是按字母表顺序加载。在你的 `package.json` 文件中有一个可选的 `keymaps` 数组可以指定加载哪一个键映射以及用什么顺序加载。

通过确定在哪个元素上触发按键事件来执行快捷键绑定。在上面的示例中，当在 `atom-workspace` 元素中按 <kdb>Alt+Ctrl+O</kbd> 时，会执行 `your-name-word-count:toggle` 指令。因为 `atom-workspace` 元素是整个 Atom UI 的父级元素，这意味着在应用程序中组合键可以在任何地方工作。

我们稍后将在 [Keymaps in Depth](#) 一节中详细介绍更多高级的快捷键绑定的资料。

### 菜单

菜单放在 `menus` 子目录中。在你的插件中，这里定义了像当你右击时弹出的上下文菜单或进入应用程序菜单来触发功能之类的菜单元素。

默认情况，所有菜单按字母表顺序加载。在 `package.json` 文件中有一个可选的 `menus` 数组可以指定读取哪一个菜单以及按什么顺序加载。

#### 应用程序菜单

对于未关联到特定元素的包的常见操作，建议你在 **包** 菜单下面创建一个应用程序菜单项。如果我们看一下我们生成的 `menus/your-name-word-count.json` 文件，我们将看到想这样的一部分内容：

```json
"menu": [
  {
    "label": "Packages",
    "submenu": [
      {
        "label": "Word Count",
        "submenu": [
          {
            "label": "Toggle",
            "command": "your-name-word-count:toggle"
          }
        ]
      }
    ]
  }
]
```

这部分内容在 "Packages" 菜单中名叫 "Your Name Word Count" 的菜单组下面放了一个 "Toggle" 菜单项

![Application Menu Item](https://flight-manual.atom.io/hacking-atom/images/menu.png)

我们稍微来看一下，当你选择那个菜单项，它将运行 "your-name-word-count:toggle" 指令。

你指定的菜单模版与所有其他包提供的模版按他们的加载顺序进行合并。

#### 上下文菜单

推荐给链接到界面指定部分的指令指定一个上下文菜单项。在我们的 `menus/your-name-word-count.json` 文件中，我们可以看到一个自动生成的部分，就像这样：

```json
"context-menu": {
  "atom-text-editor": [
    {
      "label": "Toggle your-name-word-count",
      "command": "your-name-word-count:toggle"
    }
  ]
}
```

这里添加一个 "Toggle Word Count" 菜单选项到在 Atom 文本编辑器面板右击时弹出的菜单中。

![Context Menu Entry](https://flight-manual.atom.io/hacking-atom/images/context-menu.png)

当你点击时它将再次在你的代码中运行 `your-name-word-count:toggle` 方法。

上下文菜单的创建是通过确定选择了哪一个元素，然后添加所有选择器匹配到该元素（按它们的加载顺序）的菜单项。然后对这些元素重复该过程，直到抵达 DOM 树的顶部。

你也可以添加分隔符和子菜单到你的上下文菜单中。要添加一个子菜单，需要提供一个 `submenu` 关键字而不是一个指令。要添加一个分隔符，添加一个 `type:'separator'` 的键/值对的单一项。例如，你可以像下面这样做：

```json
{
  "context-menu": {
    "atom-workspace": [
      {
        "label": "Text",
        "submenu": [
          {
            "label": "Inspect Element",
            "command": "core:inspect"
          },
          {
            "type": "separator"
          },
          {
            "label": "Selector All",
            "command": "core:select-all"
          },
          {
            "type": "separator"
          },
          {
            "label": "Deleted Selected Text",
            "command": "core:delete"
          }
        ]
      }
    ]
  }
}
```

## 开发我们包

目前我们已经生成了包，如果我们通过菜单或者指令面板运行 `your-name-word-count:toggle` 指令，我们将得到一个说 "The Wordcount package is Alive! It's ALIVE!" 的对话框

![Wordcount Package is Alive Dialog](https://flight-manual.atom.io/hacking-atom/images/toggle.png)

### 理解生成的代码

让我们看一看在我们的 `lib` 目录中的代码以及看看发生了什么。

在我们的 `lib` 目录中有两个文件。一个是主文件（lib/your-name-word-count.js），它是指在 `package.json` 文件中作为这个包的主文件来执行。这个文件处理整个插件的逻辑。

第二个文件是一个视图类，`lib/your-name-word-count-view.js`，它处理包的 UI 元素。让我们先看看这个文件，因为它非常简单。

```js
export default class YourNameWordCountView {

  constructor(serializedState) {
    // Create root element
    this.element = document.createElement('div');
    this.element.classList.add('your-name-word-count');

    // Create message element
    const message = document.createElement('div');
    message.textContent = 'The YourNameWordCount package is Alive! It\'s ALIVE!';
    message.classList.add('message');
    this.element.appendChild(message);
  }

  // Returns an object that can be retrieved when package is activated
  serialize() {}

  // Tear down any state and detach
  destroy() {
    this.element.remove();
  }

  getElement() {
    return this.element;
  }

}
```

基本上这里唯一发生的事情是当视图类创建后，它创建一个简单的 `div` 元素并给它添加 `your-name-word-count` 类（所以我们可以在后面查找或者给它设置样式），然后给它添加 `Your Name Word Count package is Alive!` 文本。这里也有一个返回该 `div` 的 `getElement` 方法。`serialize` 和 `destroy` 方法不做任何事情，在看另一个例子之前，我们不用太担心这个问题。

注意我们简单的使用基本的浏览器 DOM 方法：`createElement()` 和 `appendChild()`。

我们拥有的第二个文件是包的主入口点。再说一次，因为它引用于 `package.json` 文件。让我们看一看该文件。

```js
import YourNameWordCountView from './your-name-word-count-view';
import { CompositeDisposable } from 'atom';

export default {

  yourNameWordCountView: null,
  modalPanel: null,
  subscriptions: null,

  activate(state) {
    this.yourNameWordCountView = new YourNameWordCountView(state.yourNameWordCountViewState);
    this.modalPanel = atom.workspace.addModalPanel({
      item: this.yourNameWordCountView.getElement(),
      visible: false
    });

    // Events subscribed to in atom's system can be easily cleaned up with a CompositeDisposable
    this.subscriptions = new CompositeDisposable();

    // Register command that toggles this view
    this.subscriptions.add(atom.commands.add('atom-workspace', {
      'your-name-word-count:toggle': () => this.toggle()
    }));
  },

  deactivate() {
    this.modalPanel.destroy();
    this.subscriptions.dispose();
    this.yourNameWordCountView.destroy();
  },

  serialize() {
    return {
      yourNameWordCountViewState: this.yourNameWordCountView.serialize()
    };
  },

  toggle() {
    console.log('YourNameWordCount was toggled!');
    return (
      this.modalPanel.isVisible() ?
      this.modalPanel.hide() :
      this.modalPanel.show()
    );
  }

};
```

这里发生了很多事情。首先我们可以看到我们定义了四个方法。唯一需要的一个是 `activate`。`deactivate` 和 `serialize` 方法是 Atom 期望的，不过是可选的。 `toggle` 方法是 Atom 不查找的一个方法，所以我们必须在某个地方调用它，你可以回想一下我们在 `package.json` 文件的 `activationCommands` 部分以及我们在菜单文件中的操作。

`deactivate` 方法简单地销毁各种我们已经创建的类实例，`serialize` 方法简单的传递序列化到视图类。这里没什么太令人兴奋的东西。

`activate` 指令做了许多事情。比如，当 Atom 启动时，它不会自动调用，它是在 `package.json` 文件中的其中一个 `activationCommands` 被调用时首次调用。在这种情况中，`activate` 只在 `toggle` 命令首次调用时调用。如果没有人曾经调用菜单项或者热键，这的代码是从未调用的。

该方法做两件事情。第一件事情是它创建一个我们已经拥有的视图类实例并添加其创建的元素到 Atom 工作区的一个隐藏的模态面板中。

```js
this.yourNameWordCountView = new YourNameWordCountView(state.yourNameWordCountViewState);
this.modalPanel = atom.workspace.addModalPanel({
  item: this.yourNameWordCountView.getElement(),
  visible: false
});
```

目前我们忽略关于状态内容，由于对于这个简单的插件它不是很重要的。剩下的应该是非常直接的。

该方法下一个做的事情是创建一个 CompositeDisposable 类实例，它可以注册所有能从插件调用的指令，所以其它插件能订阅这些事件。

```js
// Events subscribed to in atom's system can be easily cleaned up with a CompositeDisposable
this.subscriptions = new CompositeDisposable();

// Register command that toggles this view
this.subscriptions.add(atom.commands.add('atom-workspace', {
  'your-name-word-count:toggle': () => this.toggle()
}));
```

下一个我们有 `toggle` 方法。该方法简单地切换我们在 `activate` 方法中创建的模态面板的显示。

```js
toggle() {
  console.log('YourNameWordCount was toggled!');
  return (
    this.modalPanel.isVisible() ?
    this.modalPanel.hide() :
    this.modalPanel.show()
  );
}
```

这应该是非常简单易懂的。我们期望看到该模态面板根据它当前的状态来决定是否可见以及隐藏或显示。

### 流程

好，让我们回顾一下在这个包中的实际流程。

1. Atom 启动
2. Atom 开始加载包
3. Atom 读取你的 `package.json`
4. Atom 加载 键映射，菜单，样式以及主模块
5. Atom 结束加载包
6. 在某个时间点，用户执行你的包指令 `your-name-word-count:toggle`
7. Atom 在你的主模块执行 `activate` 方法，它会创建隐藏的模态面板界面来设置 UI
8. Atom 执行显示该隐藏的模态视图的包指令 `your-name-word-count:toggle`
9. 在某个时间点，用户再次执行 `your-name-word-count:toggle` 指令
10. Atom 执行该指令隐藏该模态面板界面
11. 最后，Atom 关闭，它可以触发任何你的包已经定义的序列化。

> 提示：记住如果在你包中你选择不使用 `activationCommands`，那么该流程将稍微有些不同。


### 统计字数

现在我们理解放生了什么，让我们修改代码使我们的小模态框显示我们当前的字数而不是静态文本。

我们用一个非常简单的方式来做这件事情。当切换对话框时，我们将在显示模态框之前统计正确的字数。所以让我们在 `toggle` 指令中做这件事情。如果我们添加一些代码来统计字数并要求改界面自己更新，我们添加一些像这样的代码：

```js
toggle() {
  if (this.modalPanel.isVisible()) {
    this.modalPanel.hide();
  } else {
    const editor = atom.workspace.getActiveTextEditor();
    const words = editor.getText().split(/\s+/).length;
    this.yourNameWordCountView.setCount(words);
    this.modalPanel.show();
  }
}
```

让我们看一看我们已经添加的 3 行代码。首先我们调用 [`atom.workspace.getActiveTextEditor()`](https://atom.io/docs/api/v1.28.2/Workspace#instance-getActiveTextEditor) 获取一个当前编辑器对象的实例（我们统计文本的地方）

下一步我们调用 [`getText()`](https://atom.io/docs/api/v1.28.2/TextEditor#instance-getText) 在我们新的编辑器对象中获取字数，然后用正则表达式的空白字符分离文本并获取改数组的长度。

最后，我们在我们的界面调用 `setCount()` 方法告诉我们的界面更新显示的字数并再次显示模态框。由于该方法还不存在，让我们现在创建它。

我们可以添加这段代码到我们的 `your-name-word-count-view.js` 文件的末尾：

```js
setCount(count) {
  const displayText = `There are ${count} words.`;
  this.element.children[0].textContent = displayText;
}
```

非常简单！我们获取传入的统计数并把它放到一个字符串中，然后我们把这个字符串插入到我们的界面正在控制的元素中。

> 注意：要查看更改，你需要重新加载代码。你可以重启窗口（在指令面板中输入 `window:reload` 指令）来做这件事情。常见的做法是打开两个 Atom 窗口，一个用于开发你的包，另一个用于测试以及重新加载。

![Word Count Working](https://flight-manual.atom.io/hacking-atom/images/wordcount.png)


## 基础调试

你将在代码中注意到少许 `console.log` 语句。关于 Atom 基于 Chromium 上构建的一件非常酷的事情是你在网页开发中能使用的调试工具同样也可以在 Atom 中使用。

要打开开发控制台，按 <kbd>Ctrl+Shift+I</kbd>，或者选择菜单选项 **View > Developer > Toggle Developer Tools**。

![Developer Tools Debugging](https://flight-manual.atom.io/hacking-atom/images/dev-tools.png)

从这里你可以检查对象，运行代码以及查看控制台输出就像你正在调试网站一样。


## 测试

你的包应该有测试，如果他们放在 `spec` 目录，他们可以用过 Atom 来运行。

在内部，[Jasmine v1.3](https://jasmine.github.io/1.3/introduction.html) 执行你的测试，所以你可以认为任何可用的 DSL 也可以用于你的包。

### 运行测试

一旦你写好的测试套件，你可以按 <kbd>Alt+Ctrl+P</kbd> 或通过 **View > Developer > Run Package Specs** 菜单来运行。我们生成的包带有一个示例测试套件，所以你可以现在运行看看发生了什么。

![Spec Suite Results](https://flight-manual.atom.io/hacking-atom/images/spec-suite.png)

你也可以从命令行使用 `atom --test spec` 指令来运行它们。它打印测试输出以及结果到控制台并根据测试是否通过或失败返回相应的状态码。

## 小结

我们现在已经生成，定制以及测试了我们的收个 Atom 插件。恭喜你！现在让我们继续并发布它给全世界用。
