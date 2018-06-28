# 安装 Atom

要开始使用 Atom，我们需要在你的系统上获取一个安装包。本章节将重温在你的系统上安装 Atom 以及如何从源文件构建 Atom 的基础知识。

安装 Atom 非常简单。通常情况下，你可以到 [https://atom.io](https://atom.io) ，在页面顶部你会看到一个想下面这样的下载按钮：

![https://flight-manual.atom.io/getting-started/images/windows-downloads.png](https://flight-manual.atom.io/getting-started/images/windows-downloads.png)

这个按钮或这些按钮对应下载的安装包是针对你的平台的，下载安装包也是易于安装的。不过，还是让我们在这里详细的回顾一下它们吧。

## Windows 上安装 Atom

Atom 支持 Windows installer 安装，可以从 [https://atom.io](https://atom.io) 下载或者从 [Atom releases page](https://github.com/atom/atom/releases/tag/v1.28.0) 下载名叫 `AtomSetup.exe` 的包。安装程序将安装 Atom，并添加 `atom` 和 `apm` 命令到 `PATH`，然后在桌面创建和开始菜单里添加快捷方式。

![https://flight-manual.atom.io/getting-started/images/windows-system-settings.png](https://flight-manual.atom.io/getting-started/images/windows-system-settings.png)

在 File Explorer 中上下文菜单 `Open with Atom` ，使 Atom 能用 `Open with...` 做文件关联，通过上面所示的 System Settings 面板控制。

Atom 启动后，点击 `File > Settings`，然后在左侧你会看到 `System` 标签栏，勾选 `Show in file context menus` 以及 `Show in folder context menus` 前面的复选框，这样你就完成了所有设置。

## 便携模式

Atom 存储配置和状态在 `.atom` 目录中，这个目录通常位于你的 `home` 目录下（Windows 中是 `%userprofile%` 目录）。但是你可以在便携模式中运行 Atom , app 和 配置文件存储在一起，例如在在移动存储设备上。

要用便携模式安装 Atom ，下载 [zip/tar.gz package for your system](https://github.com/atom/atom/releases/tag/v1.28.0) 并加压到你的移动存储设备中。

然后创建一个与含有 atom.exe 目录同级的 `.atom` 目录，举个例子：

```
e:\atom-1.14\atom.exe
e:\.atom
```

### 便携模式注意点

* `.atom` 目录必须是可写的
* 你可以赋值一个现有的 `.atom` 目录到你的便携设备
* Atom 也可以在 `.atom` 目录中存储它的 Electron 用户数据 - 只要在 `.atom` 里创建一个叫 `electronUserData` 子目录
* 或者你可以设置 `ATOM_HOME` 环境变量来指定你想要的位置（你可以写一个 `.sh` 或 `.cmd` 脚本临时设置并从中启动它）
* 便携模式安装不支持自动更新

## 从源文件构建 Atom

如果你对这部分内容感兴趣可以阅读飞行手册的 [Hacking on Atom Core](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/) 章节，里面详细介绍如何克隆和构建源代码。

## 代理和防火墙设置

### 在防火墙后面？

如果你处在防火墙后面而且在安装包文件时遇到 SSL 错误，你可以禁用 strict SSL，运行如下代码：

```
C:\> apm config set strict-ssl false
```

### 使用代理？

如果你使用 HTTP(S) 代理你可以通过运行下面的代码设置 apm：

```
C:\> apm config set https-proxy YOUR_PROXY_ADDRESS
```
