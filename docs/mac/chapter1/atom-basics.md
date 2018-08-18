# Atom 基础知识

现在 Atom 已经在你的系统中安装好了，让我们乘热打铁，配置以及熟悉 Atom 编辑器。

当你第一次启动 Atom，你会看到像下面这样的界面：

![Atom's welcome screen](https://flight-manual.atom.io/getting-started/images/first-launch.png)

这是 Atom 的欢迎界面，并且对于如何开始使用这款编辑器给你提供了一个非常好的的起点。

## 术语

你可以在我们的[术语表](/mac/appendixa/glossary)找到我们使用遍及本手册的各种术语的定义

## 命令调色板

在欢迎界面中，我们介绍了也许是 Atom 中最重要的命令，命令调色板。如果当你聚焦在编辑器面板时按下 <kbd>Cmd+Shift+P</kbd>，命令调色板就会弹出来。

> 贯穿整书，我们将使用像 <kbd>Cmd+Shift+P</kbd> 这样的快捷键绑定来说明如何运行命令。这些是我们检测到你运行的平台的默认快捷键绑定。
>
> 如果你想在我们检测到你的平台后查看不同平台的信息，你可以通过使用页面顶部附近的平台选择器选择不同的平台：
>
> ![Platform Selector](https://flight-manual.atom.io/getting-started/images/platform-selector.png)
>
> 如果平台选择器不存在，那么当前页面没有任何特定平台的内容。
>
> 如果你已经自定义了 Atom 的键盘映射，你可以看到这些快捷键绑定已经映射到命令调色板中或 [Settings View](/mac/chapter1/atom-basics?id=设置与偏好) 的快捷键绑定标签中。

搜索驱动菜单在 Atom 中能够做任何可能的重要任务。不用点击周围所有的应用程序菜单来查找一些功能，你可以按下 <kbd>Cmd+Shift+p</kbd> 并搜索你需要的命令。

![Command Palette](https://flight-manual.atom.io/getting-started/images/command-palette.png)

你不仅可以查看并通过数千条可能的命令快速搜索，而且你也可以查看是否有快捷键绑定关联到这些命令。这是非常好的功能，因为它意味着你可以用猜的方式来做有趣的事情同时也可以学习用快捷键来做。

本书其余部分，除了不同命令的快捷键绑定，我们将试着弄清楚关于你可以在命令调色板中搜索的文本。

## 设置与偏好

Atom 有许多设置与偏好，你可以在设置界面里修改。

![Settings View](https://flight-manual.atom.io/getting-started/images/settings.png)

包括像更换主题，指定如何处理换行，字体设置，tab 尺寸，滚动速度等等。你也可以用此界面安装新的包和主题，我们将在 `Atom Packages` 中涉及。

要打开设置界面，你可以：

* 在菜单条中选择 *Atom > Preferences* 菜单项
* 在 [Command Palette](/mac/chapter1/atom-basics?id=命令调色板)
* 用 <kbd>Ctrl+</kbd>，快捷键绑定

### 更换主题

设置界面也能让你为 Atom 更换主题。Atom 有 4 种不同的 UI 主题可供选择，Atom 主题和 One 主题各有一款暗色系和一款亮色系列，此外还有 8 中不同的语法主题。你可以修改活动主题或者点击设置界面侧边栏的 Themes 标签安装新主题。

![Changing the theme from the Settings View](https://flight-manual.atom.io/getting-started/images/theme.png)

UI 主题控制 UI 元素的样式，像 tabs 和 tree view 之类的元素，而语法主题控制编辑器加载文本后的语法高亮。要更改语法或 UI 主题，只要在对应的下拉列表选择一个不同的主题即可。

在 [https://atom.io](https://atom.io) 上有很多主题，如果你想要一些不同的主题可以从上面选择安装。我们将在 `Style tweaks` 介绍自定义主题，在 `Creating a Theme` 里介绍创建属于你自己的主题。

### 自动换行

你可以在设置界面设置你的空白间隔和换行偏好。

![Whitespace and wrapping preferences settings](https://flight-manual.atom.io/getting-started/images/settings-wrap.png)

启用 "Soft Tabs" 功能后，当你按下 `Tab` 键时，系统将用空格代替实际的 tab 字符，"Tab Length" 表示插入多少个空格，或者表示当 "Soft Tabs" 禁用时用多少空格代替 tab。

"Soft Wrap" 选项会在一行文本太长不能在当前窗口显示完整的情况下进行换行。如果 禁用 soft wrapping，这一行很长的文本就会直接超出界面，你需要滚动窗口来查看其余内容。如果启动 "Soft Wrap At Preferred Line Length" 功能，那一行文本将在 80 个字符长度的位置换行而不是界面的最后。你也可以在这个界面修改默认的 line length 超过 80 个字符长度。

在 `Basic Customization` 章节里，我们将学会如何针对不同类型的文件设置不同的换行偏好（比如，你只想要 Markdown 文件自动换行，其他文件不自动换行）。

## 打开，修改和保存文件

既然你的编辑器已经是你想要的样子和状态了，那么让我们开始打开文件并编辑它们。这个毕竟一个文本编辑器，对吧？

### 打开文件

Atom 中有好几种方式可以打开文件。你可以从菜单条选择 *File > Open* 或按 <kbd>Cmd+O</kbd> 从标准对话框中选择文件

![Open file by dialog](https://flight-manual.atom.io/getting-started/images/open-file.png)

这对于打开你当前项目（接下来会详细介绍）外的文件,或者如果因为某些原因你需要从新窗口打开文件，是非常有用的。

Atom 中另一个打开文件的方式是在命令行使用 `atom` 命令。作为 Atom 安装过程中的一部分 `atom` 和 `apm` 命令是自动安装好了的。

在 Atom 中，你可以运行 `atom` 命令带一个或多个文件路径来打开那些文件

```bash
atom --help
Atom Editor v1.8.0
Usage: atom [options] [path ...]
One or more paths to files or folders may be specified. If there is an
existing Atom window that contains all of the given folders, the paths
will be opened in that window. Otherwise, they will be opened in a new
window.
...
```

如果你习惯于终端或你用终端工作多一点。你只需要输入 `atom [files]` 并准备好开始编辑文件。

### 编辑和保存文件

编辑文件是非常简单的。你可以在周围点击并用鼠标滚动然后输入更改的内容。没有特殊的编辑模式或快捷键命令。如果你更喜欢带有编辑模式或者带有更多灵活的快捷键命令的编辑器，你可以看一看 [Atom package list](https://atom.io/packages)。那儿有许多模仿流行风格的包。

要保存文件你可以从菜单条选择 *File > Save* 或者按 <kbd>Cmd+S</kbd> 组合键来保存文件。如果你选择 *File > Save As* 或者按 <kbd>Cmd+Shift+S</kbd> 那么你可以在不同的文件名下保存编辑器中的当前内容。最后，你可以选择 *File > Save All* 或按 <kbd>Alt+Cmd+S</kbd> 来保存所有在 Atom 中打开的文件。

## 打开目录

不过 Atom 不仅仅只能对但文件进行操作;你很有可能花大部分时间在多个文件的项目中工作。要打开一个目录，选择菜单项 *File > Open Folder* 并从对话框中选择一个目录。你也可以通过从菜单条中选择 *File > Add Project Folder* 或按 `Cmd+Shift+O` 添加多个目录到你当前的 Atom 窗口中。

你可以在命令行给 `atom` 命令行工具输入它们的路径打开任意数量的目录。比如，你可以运行命令 `atom ./hopes ./dreams` 来同时打开 `hopes` 和 `dreams` 目录。

当你打开带有一个或多个目录的 Atom 时，你将在窗口的侧面自动获取一个树形界面。

![Tree View in an open project](https://flight-manual.atom.io/getting-started/images/project-view.png)

这个树形界面允许你查看和修改文件以及项目的目录结构。你可以在这个视图打开，重命名，删除和创建文件。

你也可以按 <kbd>Ctrl+\\</kbd> 或者在命令调色板中输入 `tree-view:toggle` 隐藏和显示树形界面，并按 <kbd>Ctrl+0</kbd> 聚焦它。当树形界面处在聚焦状态是，你可以按 <kbd>A</kbd>，<kbd>M</kbd>，或 <kbd>Delete</kbd> 来添加，移动或删除文件以及文件夹。你也可以在树形界面的文件或文件夹上点击右键来查看许多不同的操作，包括所有这些加在本地文件系统中显示文件或复制文件路径到剪贴板。

> Atom Packages
>
> 像 Atom 许多部分一样，树形界面不是直接构建到编辑器中的，它是 Atom 自己独立的包，Atom 默认会加载它。与 Atom 绑定的包称为 Core packages。不与 Atom 绑定的包称为 Community packages。
>
> 你可以在 GitHub 的 [https://github.com/atom/tree-view](https://github.com/atom/tree-view) 仓库找到树形界面的源码。
>
> 这是关于 Atom 有趣的事情之一。它的许多核心特性实际上只是与你实现任何其他功能相同方式的包。这意味着如果你不喜欢示例中的树形界面，你可以自己实现一个树形界面并完全替换它。

### 在项目中打开一个文件

一旦你在 Atom 已经打开了一个项目，你可以简单的查找和打开任何在项目中的文件。

如果你按 <kbd>Cmd+T</kbd> 或者 <kbd>Cmd+P</kbd>，将弹出模糊查询器。你可以输入部分路径来快速查找项目中的任何文件。

![Opening files with the Fuzzy Finder](https://flight-manual.atom.io/getting-started/images/finder.png)

你也可以按 <kbd>Cmd+B</kbd> 只在当前打开的文件中搜索（而不是你项目中的所有文件）。搜索会在 "buffers" 或打开的文件中进行。你也可以用 `Cmd+Shift+B` 限制模糊搜索，它只会从你最后在 Git 提交的新文件或者已经修改了的文件中进行搜索。

模糊查询器使用 `core.ignoredNames`，`fuzzy-finder.ignoredNames` 和 `core.excludeVCSIgnoredPaths` 的中的配置设置来过滤不显示的文件和文件夹。如果你有一个项目，这个项目包含大量不想在其中被搜索的文件，你可以添加模式匹配或文件路径到这些配置设置或者[标准的 .gitignore 文件](https://git-scm.com/docs/gitignore)。我们将在 `Global Configuration Settings` 一节中学习更多关于配置设置的内容，不过现在你可以在 Core Settings 下的设置界面简单的设置这些。

`core.ignoredNames` 和 `fuzzy-finder.ignoredNames` 都是用 [minimatch Node module](https://github.com/isaacs/minimatch) 实现的 glob 模式。

> 配置设置标记法
>
> 有时你会看到我们用全拼配置设置，像 "Core Settings 中的 Ignored Names"。有时你会看到我们用速记名称，像 `core.ignoredNames`。这些都是指的相同的东西。速记法是包名，加一个点 `.`，后面跟 "camel-cased" 的设置名称。
> 如果你有一个短语想要小驼峰化，遵循这些步骤：
>  1. 第一个字母小写
>  2. 所有其它单词的第一个字母大写
>  3. 移除空格
>
> 所以 "Ignored Names" 变为 "ignoredNames"。
