# 安装 Atom

要开始使用 Atom，我们需要在你的系统上获取一个安装包。本章节将重温在你的系统上安装 Atom 以及如何从源文件构建 Atom 的基础知识。

安装 Atom 非常简单。通常情况下，你可以到 [https://atom.io](https://atom.io) ，在页面顶部你会看到一个想下面这样的下载按钮：

![Download buttons on https://atom.io](https://flight-manual.atom.io/getting-started/images/linux-downloads.png)

这个按钮或这些按钮对应下载的安装包是针对你的平台的，下载安装包也是易于安装的。不过，还是让我们在这里详细的回顾一下它们吧。

## Linux 上安装 Atom

在 Linux 系统上安装 Atom，你可以设置你的分布式包管理器，让它使用我们的其中一个官方安装包仓库。这也可以使你当新版本发布时能更新 Atom。

### Debian 和 Ubuntu (deb/apt)

要在 Debian，Ubuntu，或相关的发行版上安装 Atom，运行下面的命令添加我们的官方安装包仓库到你的系统：

```bash
curl -sL https://packagecloud.io/AtomEditor/atom/gpgkey | sudo apt-key add -
sudo sh -c 'echo "deb [arch=amd64] https://packagecloud.io/AtomEditor/atom/any/ any main" > /etc/apt/sources.list.d/atom.list'
sudo apt-get update
```

你现在可以用 apt-get（或在 Ubuntu 上用 apt）安装 Atom：

```bash
# Install Atom
sudo apt-get install atom
# Install Atom Beta
sudo apt-get install atom-beta
```

或者，你可以直接下载 [`Atom.deb package`](https://atom.io/download/deb) 安装：

```bash
# Install Atom
sudo dpkg -i atom-amd64.deb
# Install Atom's dependencies if they are missing
sudo apt-get -f install
```

### Red Hat 和 CentOS (YUM), 或 Fedora (DNF)

要在 CentOS，Oracle Linux，Red Hat Enterprise Linux，Scientific Linux，Fedora，或者使用 YUM 或 DNF 包管理器的相关发行版上安装 Atom，运行下面的命令添加我们的官方安装包仓库到你的系统：

```bash
sudo rpm --import https://packagecloud.io/AtomEditor/atom/gpgkey
sudo sh -c 'echo -e "[Atom]\nname=Atom Editor\nbaseurl=https://packagecloud.io/AtomEditor/atom/el/7/\$basearch\nenabled=1\ngpgcheck=0\nrepo_gpgcheck=1\ngpgkey=https://packagecloud.io/AtomEditor/atom/gpgkey" > /etc/yum.repos.d/atom.repo'
```

你现在可以用 dnf 安装 Atom（或根据你的发行版用 yum）

```bash
# Install Atom
sudo dnf install atom
# Install Atom Beta
sudo dnf install atom-beta
```

或者你可以直接下载 [`Atom.rpm package`](https://atom.io/download/rpm) 安装 Atom：

```bash
# On YUM-based distributions
sudo yum install -y atom.x86_64.rpm
# On DNF-based distributions
sudo dnf install -y atom.x86_64.rpm
```

### SUSE (zypp)

要在 openSUSE 或使用 Zypp 包管理器的相关发行版上安装 Atom，运行下面的命令添加我们的官方安装包仓库到你的系统：

```bash
sudo sh -c 'echo -e "[Atom]\nname=Atom Editor\nbaseurl=https://packagecloud.io/AtomEditor/atom/el/7/\$basearch\nenabled=1\ntype=rpm-md\ngpgcheck=0\nrepo_gpgcheck=1\ngpgkey=https://packagecloud.io/AtomEditor/atom/gpgkey" > /etc/zypp/repos.d/atom.repo'
sudo zypper --gpg-auto-import-keys refresh
```

你现在可以用 `zypper` 安装 Atom：

```bash
# Install Atom
sudo zypper install atom
# Install Atom Beta
sudo zypper install atom-beta
```

或者你可以直接下载 [`Atom.rpm package`](https://atom.io/download/rpm) 安装 Atom：

```bash
sudo zypper in -y atom.x86_64.rpm
```

## 便携模式

Atom 存储配置和状态在 `.atom` 目录中，这个目录通常位于你的 `home` 目录下（Windows 中是 `%userprofile%` 目录）。但是你可以在便携模式中运行 Atom , app 和 配置文件存储在一起，例如在在移动存储设备上。

要用便携模式安装 Atom ，下载 [zip/tar.gz package for your system](https://github.com/atom/atom/releases/tag/v1.28.0) 并加压到你的移动存储设备中。

然后创建一个与含有 Atom 二进制文件的目录同级的 `.atom` 目录，举个例子：

```
/media/myusb/atom-1.14/atom
/media/myusb/.atom
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

你可以运行 `apm config get https-proxy` 来验证它是否设置正确。
