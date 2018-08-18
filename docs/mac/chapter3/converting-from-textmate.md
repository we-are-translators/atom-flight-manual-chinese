# 从 TextMate 转换

你可能有来自你喜欢和使用的 [TextMate](https://macromates.com/) 主题或语法并且你想要转换到 Atom 上来。如果是这种情况，你是幸运的，因为有工具可以帮助你进行转换。

## 转换 TextMate 语法包

转换 TextMate 包允许你在 Atom 中使用它的编辑器偏好设置，代码片段，以及颜色显示。

让我们转换 [R](https://en.wikipedia.org/wiki/R_(programming_language)) 编程语言的 TextMate 包。你可以 [在 GitHub 上](https://github.com/textmate) 找到其它已有的 TextMate 包。

你可以用下面的命令转换 R 包：

```bash
apm init --package language-r --convert https://github.com/textmate/r.tmbundle
```

你现在可以切换目录到 `language-r` 来查看转换后的包。一旦你使用 `apm link` 命令链接你的包，那么你的新包裹就已经准备好使用了。启动 Atom 并在编辑器中打开一个 `.r` 文件来查看实际效果！

## 转换 TextMate 语法主题

本小结将复习如何转换 [TextMate](https://macromates.com/) 主题为 Atom 主题。

### 差异

在编辑器中，TextMate 主题使用 [plist](https://en.wikipedia.org/wiki/Property_list) 文件而 Atom 主题使用 [CSS](https://en.wikipedia.org/wiki/Cascading_Style_Sheets) 或 [Less](http://lesscss.org/) 来给 UI 和 语法设置样式。

改转换主题的功能首先解析主题的 plist 文件，然后将像给 Atom 设置样式一样，创建相匹配的 CSS 规则和属性。

### 转换主题

下载你希望转换的主题，你可以在 [TextMate website](https://wiki.macromates.com/Themes/UserSubmittedThemes) 浏览已存在的 TextMate 主题。

现在，比如说你已经下载主题到 `~/Downloads/MyTheme.tmTheme`，你可以用以下命令转换主题：

```bash
apm init --theme my-theme --convert ~/Downloads/MyTheme.tmTheme
```

然后你可以切换目录至 `my-theme` 来查看转换后的主题。

### 激活主题

一旦主题已经安装，你可以通过启动 Atom 并使用 *Edit > Preferences* 菜单项打开设置界面启用该主题。然后在左侧导航栏选择 "Themes" 标签。最后，从 "Syntax Theme" 下拉菜单选择 "My Theme" 来启用你的新主题。

你的主题现在已经启用了，打开一个编辑器来查看它的实际运行效果吧！
