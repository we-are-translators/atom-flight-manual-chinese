# 跨平台兼容性

Atom 运行在许多平台上面，虽然 Electron 以及 Node 帮你应付了许多细节问题，但是仍有一些需要考虑做兼容性的问题需要注意，以此来确保你的包可以在其它操作系统上工作。

## 符号链接

通过指定 'junction' 作为非管理员的类型（这个参数在 macOS & Linux 上被忽略）, 文件的符号链接可用于 Windows 。

也要注意：

* 在 Windows 中，符号链接提交到 Git 将不能正确检出 - 使用 `fs.symlink` 动态创建你所需要的符号链接的方式来代替
* 在 Windows 中，已经建立符号链接的 *目录* 只适用于管理员 - 避免依赖于它们

## 文件名

* 在 Windows 中，*保留的文件名* 是 `com1-com9`，`lpt1-lpt9`，`con`，`nul`，`aux` 和 `prn`（无论是不是扩展名，比如 `prn.txt` 是不可以使用的）
* 在 Windows 中，*保留字符* 是 ? \ / < > ? % | : " ，所以尽可能避免使用它们
* 在 *带有空格的名称* 传入命令行时;
  * Windows 需要你 *用双引号包裹*，比如 "c:\my test"
  * macOS 和 Linux 需要 *在每个空格前面加一个反斜杠符号* ，比如 "/my\ test"

## 文件路径

* Windows 使用 `\`，尽管一些工具以及 PowerShell 也允许 `/`
* macOS 和 Linux 使用 `/`

你可以使用 `path.sep` 动态找出你是什么平台，或者更好的方式是 *使用 node 的 path 库的功能* 比如 `join` 和 `normalize`，它们会自动解决这个问题。

对于路径，Windows 支持多大 *250 个字符* - *避免深度嵌套目录结构*

## 路径不是 URL

URL 解析程序不应该用于文件路径。在所有平台中，虽然它们起初看起来像一个相对路径，但是在许多场景中会解析失败。

* 许多字符是不被理解的，比如作为查询字符串的 `?`，作为片段标识符的 `#`
* **Windows** 驱动器标识符是不能作为协议被正确解析的

如果你需要使用路径作为 URL，使用 file: 协议带上绝对路径，以此确保驱动器字母和斜杠符号是合适的地址，比如 `file:///c|/test/pic.png`

## 目录上的 `fs.stat`

`fs.stat` 函数不会返回目录内容的大小，而是返回目录本身的分配大小。在 Windows 中这会返回 0，在 macOS 中返回 1024，所以不应该依赖这个函数。

## `path.relative` 不能跨驱动器

* 在 macOS 或者 Linux 系统 `path.relative` 可用于计算任意两条路径之间的相对路径
* 在 Windows 上这并非总是可行的，因为它可能包含多个绝对根路径，比如 `c:\` 和 `d:\`

## 快速文件操作

创建和删除操作可以用几毫秒完成。如果你需呀移除许多文件和文件夹可以考虑使用 [RimRAF](https://www.npmjs.com/package/rimraf) ，它对这些操作有内置的重试逻辑。

## 行尾

* Windows 使用 `CRLF`
* macOS 和 Linux 使用 `LF`
* Windows 上的 Git 通常有 `autocrlf` 设置，它可以自动在两者之间转换

如果你正在写文本文件固定装置的规格说明，注意这会干扰文件长度，哈希代码和直接文本的比较。它也将通过每行 1 个字符更改 Atom 选中区域的长度。

如果你有文本文件固定装置的规格说明，你可能想告诉 Git 强制 LF，CRLF 或在 `.gitattributes` 中指定路径来保留它们，比如

```
spec/fixtures/always-crlf.txt eol=crlf
spec/fixtures/always-lf.txt eol=lf
spec/fixtures/leave-as-is.txt -text
```
