# 包：活动编辑器信息

在 [字数统计](/linux/chapter3/package-word-count) 包中我们了解了如何在模态面板中展示信息。不过，面板并不是扩展 Atom UI 的唯一方式 — 你也可以添加项目到工作区。这些项目可以被拖到新的区域（比如，在窗口边上的一个停靠项），Atom 将在你下次打开项目时恢复它们。Atom 的树形界面以及第三方包像 [Nuclide](https://nuclide.io/) 的控制台，调试，轮廓界面，以及诊断（linter  结果）都使用了该系统。

对于此包，我们将定义一个工作区域项目，告诉我们一些关于我们的活动文本编辑器的信息。最终实现的包可以在 [https://github.com/atom/active-editor-info](https://github.com/atom/active-editor-info) 查看。


## 创建包

首先，按 <kbd>Ctrl+Shift+P</kbd> 打开 [指令面板](https://github.com/atom/command-palette)。输入 "generate package" 并选择 "Package Generator: Generator Package" 指令，就像我们在 [](/linux/chapter3/package-word-count?id=包生成器) 小节中做的那样。
