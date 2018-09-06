# 创建主题

Atom 的界面是使用 HTML 渲染的，并用 CSS 的超集 Less 设置其样式。如果你以前没听过 Less 也没关系；它就像 CSS 一样，只不过多了一些便利的扩展。

Atom 支持两种类型的主题：*UI* 和 *Syntax* 。UI 主题给元素设置样式，比如树形视图，标签，下拉列表，以及状态栏。 Syntax 主题给代码，凹槽以及其它在编辑器视图中的元素设置样式。

![Theme boundary](https://flight-manual.atom.io/hacking-atom/images/theme-boundary.png)

主题可以从 Settings View 中安装和更换，Settings View 可以通过选择 Edit > Preferences 菜单，然后在左手边的导航栏上面点击 "Install" 标签 或者 "Themes" 标签。

## 起航

主题是非常简单的，不过在开始之前熟悉一些相关的知识还是很有用的：

* Less 是 CSS 的超集，不过它有一些像变量这样的而且真的非常有用的特性。如果你不熟悉它的语法，你可以自己花点时间去 [熟悉一下](https://speakerdeck.com/danmatthews/less-css) 。

* 你也可能想要复习一下 `package.json` 的概念（如 [Atom `package.json`](/linux/chapter3/package-word-count?id=packagejson) 中所述）。该文件用于帮助发布你的主题给 Atom 用户。

* 你的主题的 `package.json` 必须包含一个值为 `ui` 或者 `syntax` 的 `theme` 关键字，这样 Atom 才能识别并将其作为主题加载。

* 你可以查找已有主题来安装或者在 [the atom.io themes registry](https://atom.io/themes) 中 fork 。

## 创建 Syntax 主题

让我们一起来创建属于你的第一个主题吧。

首先，按 <kbd>Ctrl+Shift+P</kbd> 并输入 "Generate Syntax Theme" 来生成一个新的主题包。选择 "Generate Syntax Theme" ，然后你将被询问要创建的主题的路径。让我们叫我们的主题为 `motif-syntax` 吧。

> Syntax 主题应该以 *-syntax* 结尾，UI 主题应该以 *-ui* 结尾。

Atom 将打开一个新窗口，显示 motif-syntax 主题，它为我们创建了一套默认的文件夹和文件模版。如果你用 <kbd>Ctrl+,</kbd> 打开 Settings View 然后点击在左侧导航栏上的 "Themes" 标签，你将看到 "Motif" 主题在 "Syntax Theme" 下拉菜单中列出来了。从菜单中选择它来激活改主题，现在当你打开编辑器时，你应该看到你的新的 `motif-syntax` 主题已经应用了。

打开 `styles/colors.less` 来更改各种已经定义了的颜色变量。比如，把 `@red` 改为 `#f4c2c1` 。

然后打开 `styles/base.less` 并修改各种已经定义了的选择器。这些选择器给编辑器中的代码的不同部分设置样式，比如注释，字符串以及在凹槽中行号。

举个例子，让我们把 `.gutter` 的 `background-color` 设置为 `@red` 。

按 <kbd>Alt+Ctrl+R</kbd> 重启 Atom 来查看你在 Atom 窗口中所做的更改。非常优雅！

> 提示：你可以通过打开一个 Dev Mode 的 Atom 窗口，避免重启来查看你所做的更改。在终端运行 `atom --dev .` 来打开一个 Dev Mode 的 Atom 窗口，或者使用 *View > Developer > Open in Dev Mode* 菜单。当你编辑主题时，你所做的更改将会立即被应用！

> 注意：建议 *不要* 在你的 syntax 主题中指定 `font-family` ，因为它将覆盖在 Atom 设置中 Font Family 字段。如果你仍想推荐一个适合你主题的字体，我们建议你在你 README 中来做这件事情。

## 创建 UI 主题

要创建 UI 主题，需要做以下一些事情：

1. Fork [ui-theme-template](https://github.com/atom-community/ui-theme-template)
2. Clone 已经 fork 的仓库到本地文件系统
3. 在已经 fork 的主题目录中打开一个终端
4. 在终端运行 `atom --dev .` 在 Dev Mode 的 Atom 窗口中打开你的新主题或者使用 *View > Developer > Open in Dev Mode* 菜单
5. 在主题的 `package.json` 文件中更改主题名字
6. 用 `-ui` 结尾来给你的主题起名，比如 `super-white-ui`
7. 运行 `apm link --dev` 来 symlink 你的仓库到 `~/.atom/dev/packages`
8. 使用 <kbd>Alt+Ctrl+R</kbd> 重启 Atom
9. 通过 Settings View 中的 "Themes" 标签中的 "UI Theme" 下拉菜单启用主题
10. 更改吧！由于你打开的主题在 Dev Mode 窗口中，更改将在编辑器中立即应用，无需重启。

> 提示：因为我们在上面的指南中使用了 `apm link --dev` ，如果你改坏了任何东西，你可以关闭 Atom 并正常启动 Atom 来强制 Atom 为默认主题。即使发生了某些灾难性的错误，这也依旧允许你继续在你的主题上工作。

## 主题变量

UI 主题和 Syntax 主题 **必须** 提供一个 `ui-variables.less` 文件 和 `syntax-variables.less` 文件。该文件包含预定义的变量，这些变量用于确保界面外观相匹配。

这里是有默认值的变量：

* [ui-variables.less](https://github.com/atom/atom/blob/master/static/variables/ui-variables.less)
* [syntax-variables.less](https://github.com/atom/atom/blob/master/static/variables/syntax-variables.less)

这些默认值将用于主题没有定义它自己的变量时的回退处理。

### 在包中使用

在任何你的包的 `.less` 文件中，你可以通过从 Atom 导入 `ui-variables` 文件 或者 `syntax-variables` 文件访问主题变量。

你的包通常应该只指定结构样式，而且这些样式应该来自 [样式指南](https://github.com/atom/styleguide) 。你的包不应该指定颜色，padding 大小，或者用绝对像素的任何元素。你应该使用主题变量来代理它们。如果你遵循该指导方针，你的包将看起来很好，和其它任何主题一样开箱即用！

这有一个包可以定义使用主题变量的示例 `.less` 文件：

```less
@import "ui-variables";

.my-selector {
  background-color: @base-background-color;
  padding: @component-padding;
}
```

```less
@import "syntax-variables";

.my-selector {
  background-color: @syntax-background-color;
}
```

## 开发工作流

有一些工具可以帮助我们更快更容易地进行主题开发。

### 实时重载

当你更改主题后按 <kbd>Alt+Ctrl+R</kbd> 重载，这不是很理想。在 Dev Mode 中的 Atom 窗口上，Atom 支持样式的 [实时更新](https://github.com/atom/dev-live-reload) 。

要启动一个 Dev Mode 窗口：

* 通过在菜单项中选择 *View > Developer > Open in Dev Mode* 方式在 dev 窗口中打开你的主题目录
* 或者从终端输入 `atom --dev` 指令启动 Atom

如果你希望在任何时候重载所有样式，你可以使用快捷键 <kbd>Alt+Ctrl+R</kbd> 。

### 开发者工具

Atom 基于 Chrome 浏览器，它支持 Chrome 的开发者工具。你可以通过选择 *View > Developer > Toggle Developer Tools* 菜单打开它们，或者通过使用 <kbd>Ctrl+Shift+I</kbd> 快捷键。

开发工具允许你检查元素以及查看它们的 CSS 属性。

![Developer Tools](https://flight-manual.atom.io/hacking-atom/images/dev-tools.png)

查看 Google 的 [extensive tutorial](https://developer.chrome.com/devtools/docs/dom-and-styles) 获取简短介绍。

### Atom 风格指南

如果你正在创建一个 UI 主题，你将希望有一个方法可以查看你的主题更改如何影响系统中的所有组件。[Styleguide](https://github.com/atom/styleguide) 是一个渲染了 Atom 支持的所有组件的网页。

要打开 Styleguide，用 <kbd>Ctrl+Shift+P</kbd> 打开指令面板并搜索 "styleguide" ，或者使用快捷键 <kbd>Ctrl+Shift+G</kbd> 。

![Style Guide](https://flight-manual.atom.io/hacking-atom/images/styleguide.png)

### 并行

有时当创建主题（或包）时，事情可能会出错，而且编辑器变得不能使用。比如，如果文本和背景有相同的颜色，或者一些东西被推出视线。为了避免强制用 "normal" 模式打开 Atom 来修复改问题，打开 **两个** Atom 窗口是一个明智的选择。一个用于做修改，另一个在 Dev Mode 中查看更改是否应用。

![Side by side screenshot](https://flight-manual.atom.io/hacking-atom/images/theme-side-by-side.png)

> 在 **左侧** 做修改，在 **右侧** 的 "Dev Mode" 中查看更改是否应用。

现在如果你弄乱了一些东西，只有在 "Dev Mode" 中的窗口将受到影响，而且你可以很容易地在你的 "normal" 窗口中修复这个错误。

## 发布你的主题

一旦你对你的主题很满意，而且想要分享给其他 Atom 用户，是时候发布它了。:tada:

按照 [Publishing](/linux/chapter3/publishing) 页面上的步骤。虽然实例使用的是 Word Count 包，不过发布主题的工作其实是完全一样的。
