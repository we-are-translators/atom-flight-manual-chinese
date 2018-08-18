# 发布

Atom 捆绑了一个叫做 `apm` 的命令行功能，我们在 [命令行](linux/chapter2/atom-packages?id=命令行) 一节中，通过命令行首次使用它来搜索和安装包。`apm` 命令也可以用于发布 Atom 包到公共登记处并更新它们的信息。

## 准备好你的包

有些许事情你应该在发布之前仔细检查：

* 你的 `package.json` 文件有 `name`，`description`，以及 `repository` 字段。
* 你的 `package.json` 文件有带有一个值为 `0.0.0` 的 `version` 字段。
* 你的 `package.json` 文件有一个 `engines` 字段，它包含一个 Atom 的入口比如：`"engines": {"atom": ">=1.0.0 <2.0.0"}`。
* 你的包在根目录有一个 `README.md` 文件。
* 你的 `package.json` 文件中的 `repository` URL 与你的仓库 URL 是相同的。
* 你的包是在 Git 仓库中并且它已经推送到了 [GitHub](https://github.com/)。如果你的包还没有在 GitHub，你可以参考 [这篇指南](https://help.github.com/articles/importing-a-git-repository-using-the-command-line/)。

## 发布你的包

在你发布包之前，提前检查包是否与已经发布在 [the atom.io package registry](https://atom.io/packages) 上的包重名，这不妨是一个好的主意。要做这件事情，你可以通过访问 `https://atom.io/packages/your-package-name` 来查看包是否已经存在。如果已经存在，你需要在发布前更新包名为可以发布的有效名称。

现在让我们回顾一下 `apm publish` 命令做了什么：

1. 如果是首次发布，在 atom.io 注册包名。
2. 更新 `package.json` 中的 `version` 字段并提交。
3. 创建一个新的 [Git tag](https://git-scm.com/book/en/v2/Git-Basics-Tagging) 给正在发布的版本。
4. 推送该 tag 和当前分支到 GitHub。
5. 更新 atom.io 为正在发布的新版本。

现在运行下面的指令来发布你的包：

```bash
cd path-to-your-package
apm publish minor
```

如果这是你正在发布的第一个包，`apm publish` 指令可能会提示你输入 GitHub 用户名和密码。如果你开启了双重身份验证，使用 [personal access token](https://help.github.com/articles/creating-a-personal-access-token-for-the-command-line/) 代替密码。这些认证操作是发布包必须要做的，而且你只需要在你第一次发布包时输入这些信息。一旦你登录了，证书就会安全地存储到你的 [keychain](https://en.wikipedia.org/wiki/Keychain_(software)) 中。

你的包现在已经发布了并且可以在 atom.io 使用了。直接到 `https://atom.io/packages/your-package-name` 查看你的包的主页。

关于 `apm publish`，要迭代版本和发布版本，你可以用

```bash
apm publish version-type
```

这里的 `version-type` 可以是 `major`, `minor` 以及 `patch`。

给发布指令设置 `major` 参数，这就告诉 apm 在发布之前增加版本的第一个数字，所以发布的版本将是 `1.0.0` 并且创建的 Git 标签将是 `v1.0.0`。

给发布指令设置 `minor` 参数，这就告诉 apm 在发布之前增加版本的第二个数字，所以发布的版本将是 `0.1.0` 并且创建的 Git 标签将是 `v0.1.0`。

给发布指令设置 `patch` 参数，这就告诉 apm 在发布之前增加版本的第三个数字，所以发布的版本将是 `0.0.1` 并且创建的 Git 标签将是 `v0.0.1`。

当你做了破坏向后兼容性的更改时使用 `major`，像更改默认值或者移除特性。当添加新功能或选项时使用 `minor`，不过没有破坏向后兼容性。当你更改了已有特性的实现方式使用 `patch`，不过没有更改你的包的行为或选项。查看 [semantic versioning](https://semver.org/) 来学习更多关于包发布版本的最佳实践。

你也可以运行 `apm help publish` 来查看所有可用的选项以及运行 `apm help` 来查看所有其它可用的指令。
