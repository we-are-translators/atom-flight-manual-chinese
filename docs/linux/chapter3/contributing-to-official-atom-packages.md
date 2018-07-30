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

在 Atom 中编辑包有点循环的感觉：你正在使用 Atom 修改它自己。如果你暂时中断某事会发生什么？你不想你正在使用的 Atom 版本在这个过程中不能用了。因此，
