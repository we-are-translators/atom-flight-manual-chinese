# Atom 包

首先我们Atom包系统开始。正如我们之前提到的，Atom 本身是一个非常基本的功能核心并附带了许多有用的包来添加新的功能，比如树视图([tree View](https://github.com/atom/tree-view))和设置视图([Setting View](https://github.com/atom/settings-view))。

事实上，默认情况下，有超过90个软件包包含Atom中可用的所有功能。比如，当你第一次启动Atom时你会看到这个[欢迎界面](https://github.com/atom/welcome),[拼写检查](https://github.com/atom/spell-check)、[主题](https://github.com/atom/one-dark-ui)、[模糊查找器](https://github.com/atom/fuzzy-finder)都是单独维护的包，并且都使用您可以访问的相同API，我们将在后面[黑客攻击(Hacking Atom)](https://flight-manual.atom.io/hacking-atom/)详细介绍。

这意味着软件包可以非常强大，并且可以改变从整个界面的外观和感觉到甚至是核心功能的基本操作。

为了安装新的软件包，您可以使用现在熟悉的设置视图中的安装选项卡。
使用`Cmd +`打开设置视图，点击“安装”选项卡，然后在安装包下的框中键入搜索查询。

此处列出的软件包已发布到[https://atom.io/packages](https://atom.io/packages)，它是Atom软件包的官方注册表。在设置视图上搜索将转到Atom包注册表并提取与您的搜索条件相匹配的任何内容。

![https://flight-manual.atom.io/using-atom/images/packages-install.png](https://flight-manual.atom.io/using-atom/images/packages-install.png)

所有的软件包都会提供一个"Install"按钮。点击它将下载并安装它。您的编辑器现在具有该软件包提供的功能

## 包的设置

一旦在Atom中安装了一个包，它将显示在"Packages"选项卡下的设置视图中，以及Atom附带的所有预安装包中。
要过滤列表以找到它，您可以直接在"Installed Packages"标题下键入搜索框。
![https://flight-manual.atom.io/using-atom/images/package-specific-settings.png](https://flight-manual.atom.io/using-atom/images/package-specific-settings.png)

点击软件包的"Setting"按钮将会为您提供该软件包的设置屏幕。在这里，您可以选择更改软件包的某些默认变量，查看所有命令键绑定是什么，暂时禁用软件包，查看源代码，查看软件包的当前版本，报告问题以及卸载软件包。

如果任何软件包的新版本发布，Atom会自动检测到它，并且您可以从此屏幕或从"Updates"选项卡升级软件包。这有助于您轻松地保持所有安装的软件包保持最新状态。

## Atom 主题

您还可以从设置视图中找到并安装Atom的新主题。这些可以是UI主题或语法主题，您可以从"Install"选项卡搜索它们，就像搜索新软件包一样。确保按下搜索框旁边的"Themes"切换。

![https://flight-manual.atom.io/using-atom/images/themes.png](https://flight-manual.atom.io/using-atom/images/themes.png)

点击主题标题将带您进入atom.io主题的配置文件页面，该页面通常有主题截图。这样你可以在安装之前看到它的样子。

点击"Install"将安装主题，并在主题下拉菜单中显示，如我们在[更改主题](https://flight-manual.atom.io/getting-started/sections/atom-basics/#changing-the-theme)中看到的。

![https://flight-manual.atom.io/using-atom/images/unity-theme.png](https://flight-manual.atom.io/using-atom/images/unity-theme.png)

## 命令行

您也可以使用`apm`ss从命令行安装包或主题。

> 通过在终端中运行以下命令来检查是否安装了`apm`：
```
$ apm help install
```
您应该看到一条消息，打印出有关`apm install`命令的详细信息。
如果你不这样做，请参阅[安装atom部分](https://flight-manual.atom.io/getting-started/sections/installing-atom/)以获取有关如何为系统安装`atom`和`apm`命令的说明。

您还可以使用`apm install`命令安装软件包：

- `apm install <package_name>` 安装最新版本。
- `apm install <package_name>@<package_version>` 安装最新版本。

例如`apm install emmet@0.1.5`安装[Emmet](https://github.com/atom/emmet)包的`0.1.5`版本。

您也可以使用`apm`来查找要安装的新软件包。如果您运行`apm search`，您可以在包注册表中搜索搜索词。

```
$apm search coffee
Search Results For 'coffee' (29)
├── build-coffee Atom Build provider for coffee, compiles CoffeeScript (1160 downloads, 2 stars)
├── scallahan-coffee-syntax A coffee inspired theme from the guys over at S.CALLAHAN (183 downloads, 0 stars)
├── coffee-paste Copy/Paste As : Js ➤ Coffee / Coffee ➤ Js (902 downloads, 4 stars)
├── atom-coffee-repl Coffee REPL for Atom Editor (894 downloads, 2 stars)
├── coffee-navigator Code navigation panel for Coffee Script (3493 downloads, 22 stars)
...
├── language-iced-coffeescript Iced coffeescript for atom (202 downloads, 1 star)
└── slontech-syntax Dark theme for web developers ( HTML, CSS/LESS, PHP, MYSQL, javascript, AJAX, coffee, JSON ) (2018 downloads, 3 stars)
```

您可以使用`apm`视图查看有关特定软件包的更多信息

```
$apm view build-coffee
build-coffee
├── 0.6.4
├── https://github.com/idleberg/atom-build-coffee
├── Atom Build provider for coffee, compiles CoffeeScript
├── 1152 downloads
└── 2 stars
> 
Run `apm install build-coffee` to install this package.s
```

