# 给官方 Atom 包贡献

如果你觉得你知道哪一个包引起了你正在报告的问题，你可以在那个指定的仓库中随意打开问题。当有疑问时只需要在 [atom/atom](https://github.com/atom/atom) 仓库中打开问题，但要注意它可能会被关闭并在合适的包仓库重新打开。

## 包中的黑客技术

### 克隆

第一部是创建你自己的克隆。对于某些包，你也可能需要安装 [构建 Atom 必要的工具](https://flight-manual.atom.io/hacking-atom/sections/hacking-on-atom-core/#building) 来运行 `apm install`。

比如，如果你想要对 `tree-view` 包做更改，在你的 github 账户 fork 该仓库，然后克隆它：

```bash
git clone git@github.com:your-username/tree-view.git
```

然后安装所有依赖模块：

```bash
cd tree-view
apm install
Installing modules ✓
```

现在你可以把它连接到开发模式，所以当你用 `atom --dev` 运行一个 Atom 窗口时，你将使用你 fork 的包而不是内置的包：

```bash
apm link -d
```

### 在开发模式中运行

在 Atom 中编辑包有点循环的感觉：你正在使用 Atom 修改它自己。如果你暂时中断某事会发生什么？你不愿意你正在使用的 Atom 版本在这个过程中无法使用了。因此，当你正在做开发包的工作时，你只希望在**开发模式**中运行。你将在**稳定模式**做开发编辑工作，仅切换到开发来测试更改的内容。

要打开开发模式窗口，使用 "Application: Open Dev" 指令。你也可以在命令行用 `atom --dev` 来运行开发模式。

要在开发模式中加载你的包，需要创建一个符号链接到 `~/.atom/dev/packages`。当你用 `apm develop` 克隆包时会自动执行这个命令。你也可以从包目录运行 `apm link --dev` 和 `apm unlink --dev` 来创建和删除开发模式的符号链接。

### 安装依赖模块

在拉取了任何上游的更改后，你想保持最新的依赖模块，可以运行 `apm update` 来更新依赖模块。
