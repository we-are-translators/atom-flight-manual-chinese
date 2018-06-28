# 安装 Atom

要开始使用 Atom，我们需要在你的系统上获取一个安装包。本章节将重温在你的系统上安装 Atom 以及如何从源文件构建 Atom 的基础知识。

安装 Atom 非常简单。通常情况下，你可以到 [https://atom.io](https://atom.io) ，在页面顶部你会看到一个想下面这样的下载按钮：

![Download buttons on https://atom.io](https://flight-manual.atom.io/getting-started/images/mac-downloads.png)

这个按钮或这些按钮对应下载的安装包是针对你的平台的，下载安装包也是易于安装的。不过，还是让我们在这里详细的回顾一下它们吧。

## Mac 上安装 Atom

Atom 遵循标准的 Mac zip 安装过程。你可以在 [https://atom.io](https://atom.io) 站点点击下载按钮或者你可以到 [Atom releases page](https://github.com/atom/atom/releases/latest) 下载 `atom-mac.zip` 文件。一旦你有了安装文件，你可以点击它解压应用程序，然后把新安装的 Atom 应用程序拖到 "Applications" 文件夹。

当你首次打开 Atom，它将试着安装 `atom` 和 `apm` 命令以便在终端使用。在一些情况下，Atom 可能无法安装这些命令，因为它需要管理员密码。要检查 Atom 是否能够安装 `atom` 命令，比如你可以打开一个终端窗口并输入 `which atom`，如果 `atom` 命令已经安装，你将看到如下的信息：

```bash
which atom
/usr/local/bin/atom
```

如果 `atom` 命令没有安装，`which` 命令不会返回任何信息：

```bash
which atom
```
要安装 `atom` 和 `apm` 命令，从 [命令调色板](/mac/chapter1/atom-basics?id=命令调色板) 运行 "Window: Install Shell Commands"，它将提示你输入管理员密码。

## 便携模式

Atom 存储配置和状态在 `.atom` 目录中，这个目录通常位于你的 `home` 目录下（Windows 中是 `%userprofile%` 目录）。但是你可以在便携模式中运行 Atom , app 和 配置文件存储在一起，例如在在移动存储设备上。

要用便携模式安装 Atom ，下载 [zip/tar.gz package for your system](https://github.com/atom/atom/releases/tag/v1.28.0) 并加压到你的移动存储设备中。

然后创建一个与含有 `Atom.app` 目录同级的 `.atom` 目录，举个例子：

```
/MyUSB/Atom.app
/MyUSB/.atom
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
