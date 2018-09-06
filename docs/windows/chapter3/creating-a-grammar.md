# 创建语法

Atom 的语法高亮和代码折叠功能由 [Tree-sitter] 提供支持。Tree-sitter 解析器创建和维护表示代码的完整 [语法树](https://en.wikipedia.org/wiki/Abstract_syntax_tree) 。

该语法树给 Atom 一个关于你代码的全面了解，这么做有几个好处：

1. 语法高亮不会因为格式更改而破坏。
2. 无论你如何缩进代码，代码折叠功能都能正常工作。
3. 编辑器特性可以在语法树上操作。举个例子，`Select Larger Syntax Node` 和 `Select Smaller Syntax Node` 允许你概念上选择更大和更小的代码块。
4. 社区包能够使用语法树智能地维护代码。

Tree-sitter 语法比较新。虽然 Atom 中的许多语言依旧由 [TextMate grammars](/windows/chapter3/creating-a-legacy-textmate-grammar) 提供支持，但是随着时间的推移，我们会逐步解决这些问题。

如果你正在添加新语法的支持，那么你来对地方了！

## 起航

在 Atom 中有两类组件需要使用 Tree-sitter：一种是 *parser* 文件，另一种是 *grammar* 文件。

## 解析器

Tree-sitter 基于通常用 JavaScript 写的 [context-free grammars](https://en.wikipedia.org/wiki/Context-free_grammar) 生成解析器。生成的解析器是既可用于其它应用也可用于 Atom 的 C 类库。

他们也能分别地从 Atom 命令行中开发和测试。关于如何创建这些解析器， Tree-sitter 有 [它自己的文档页面](http://tree-sitter.github.io/tree-sitter/creating-parsers)。[Tree-sitter GitHub 组织](https://github.com/tree-sitter) 也有许多你可以学习的示例解析器，它们都是自己的仓库。

一旦你创建了一个解析器，你需要把它发布到 [the NPM registry](https://www.npmjs.com/) ，这样你就可以在 Atom 中使用它了。要做这件事，确保在你的解析器的 `package.json` 中有 `name` 和  `version` 。

```json
{
  "name": "tree-sitter-mylanguage",
  "version": "0.0.1",
  // ...
}
```

然后运行命令 `npm publish` 。

## 包

一旦你有了一个在 npm 上可以找到的 Tree-sitter 解析器，那么你就可以在 Atom 中使用它了。根据惯例，语法包总是用 *language* 开头。你将需要一个带有 `package.json`，`grammars` 子目录的文件夹，以及在 `grammars` 目录中有单个 `json` 或者 `cson` 文件，它可以用任何名字命名。

```json
language-mylanguage
├── LICENSE
├── README.md
├── grammars
│   └── mylanguage.cson
└── package.json
```

## 语法文件

`mylanguage.cson` 文件表示 Atom 应该怎样使用创建的解析器。

## 基础字段

以一些必须的字段开始：

```
name: 'My Language'
scopeName: 'mylanguage'
type: 'tree-sitter'
parser: 'tree-sitter-mylanguage'
```

* `scopeName` - 该语言唯一稳定的标识符。如果他们想要基于该语言指定自定义配置，那么 Atom 用户将在配置文件中使用该配置。
* `name` - 人类可读的语言名称。
* `parser` - node module 将用于解析的解析器名称。该字符串将直接传给 `require()` 来加载解析器。
* `type` - 该字段应该有一个值 `tree-sitter`，这个值让 Atom 知道这是 Tree-sitter 语法而不是 [TextMate grammar](/windows/chapter3/creating-a-legacy-textmate-grammar) 。

## 语言识别

接着，该文件应该包含一些字段让 Atom 知道何时应该使用该语言。这些字段都是可选的。

* `fileTypes` - 文件名后缀数组。该语法将用于名称末尾带有这些后缀中其中一个的文件。注意那些后缀可以是整个文件名。
* `firstLineRegex` - 将对文件第一行进行测试的正则表达式。如果正则匹配，那么将使用该语法。
* `contentRegex` -
