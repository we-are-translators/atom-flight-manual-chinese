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

首先，按 <kbd>Ctrl+Shift+P</kbd> 打开 [指令面板](https://github.com/atom/command-palette)。输入 "generate package" 并选择 "Package Generator: Generate Package" 指令，就像我们在 [包生成器一节](linux/chapter3/package-word-count?id=包生成器) 中做的那样。输入 `ascii-art` 作为包名。
