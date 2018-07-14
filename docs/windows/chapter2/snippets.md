# 代码片段

代码片段是一个能快速生成常用代码语法的一种强大的方法。

这个想法是你可以键入像 `habtm` 之类的东西，然后按下 `tab` 键，它将扩展为 `has_and_belongs_to_many`。

许多核心和社区软件包都捆绑了自己特定的代码段。例如，为HTML语法高亮和语法突出显示提供language-html包支持，可以创建许多你可能使用的HMTL标记，如果你在 `Atom` 中创建一个新的HMTL文件，你可以键入 `HMTL` 然后按 `Tab` 键，它将扩展为：
```
	
	<html>
	  <head>
	    <title></title>
	  </head>
	  <body>
	
	  </body>
	</html>

```

它还会将光标定位在 `title` 标记的中间，以便您可以立即开始填写标记。许多片段都有多个焦点，您也可以通过 `Tab` 键移动它们-例如，在此HTML片段的情况下，一旦填写了标题标签，您可以按 `Tab` 键，光标将移动到 `body` 身体标签中间。

![View all available snippets](https://flight-manual.atom.io/using-atom/images/snippets.png)

你也可以使用模糊搜索通过在选择框中键入来过滤此列表，选择其中一个你将执行的代码片段，然后将光标放在那里（或多个光标）。

** 创建你拥有的代码片段 **

这个很酷，如果有什么语言包不包含或者你编写的代码是自定义的呢？幸运的是，你添加自己的代码片段也非常容易。

在你的 `%USERPROFILE%\.atom` 目录中有一个名为 `snippets.cson` 的文本文件，当你启动Atom时，会加载包含所有自定义的代码段。你可以通过选择“文件” > “代码段”菜单轻松打开该文件。

** 代码段格式化 **

那么让我们来看看如何编写代码片段。基本代码段格式如下所示：
```

	'.source.js':
	  'console.log':
	    'prefix': 'log'
	    'body': 'console.log(${1:"crash"});$2'

```
最左边的`$`键是这个代码片段处于活动状态的选择器。这个简单的方法是确定为其添加代码语言的语言包，并且查找“范围”的字符串。

例如，如果我们想添加一个适用于Java文件的代码片段，我们将在Settings视图中查找language-java包，我们可以看到Scope是source.java。然后顶级代码片段键就是一个句点的前面（就像CSS类选择器一样）。


![Finding the selector scope for a snippet](https://flight-manual.atom.io/using-atom/images/snippet-scope.png)


下一级的键是代码片段的名称，这些用于在代码段菜单中以更易读的方式描述代码段，你可以随心所欲地命名它们。

在每一个代码名称下面都是一个前缀，它会触发代码段落，并在触发代码段落时插入正文。

每个$后跟一个数字是制表位。一旦触发了片段，按Tab键即可循环切换制表位。

具有相同编号的制表位将创建多个光标。

上面的示例将一个日志片段添加到javascript文件中，这些文件将扩展为：

```
console.log("crash");
```
最初选择这个字符串“crash”并再次按Tab键，光标将会放在`;`之后。

```
	与CSS选择器不同，代码键入只能在每一级重复一次。如果同一级别存在重复键入，则只读最后一个键。有关更多信息，请参阅使用[CSON配置](https://flight-manual.atom.io/using-atom/sections/basic-customization/#configuring-with-cson)。
```


** 多线代码段落主体 **

你还可以使用[CoffeeScript multi-line syntax](http://coffeescript.org/#strings)“"”作为大模板。

```
'.source.js':
  'if, else if, else':
    'prefix': 'ieie'
    'body': """
      if (${1:true}) {
        $2
      } else if (${3:false}) {
        $4
      } else {
        $5
      }
    """
```
正如你所期望的，这有一个段落用来创建代码段。如果打开一个段落文件并键入snip，然后按下Tab键，则会插入以下文本：
```
'.source.js':
  'Snippet Name':
    'prefix': 'hello'
    'body': 'Hello World!'
```
只是填充一个坏的家伙，并且是你自己的一个段落。一旦你保存文件，Atom就会重新加载段落，并且你会立即尝试它。

** 每一个来源的多个代码段落 **

你能够看到下面 `snippets.cson` 文件，为同一范围包含多个段落的格式，只需要在作用域中键入包含其他代码段的段落名称，前缀和正文键。

```
'.source.gfm':
  'Hello World':
    'prefix': 'hewo'
    'body': 'Hello World!'

  'Github Hello':
    'prefix': 'gihe'
    'body': 'Octocat says Hi!'

  'Octocat Image Link':
    'prefix': 'octopic'
    'body': '![GitHub Octocat](https://assets-cdn.github.com/images/modules/logos_page/Octocat.png)'
```

同样，[Configuring with CSON](https://flight-manual.atom.io/using-atom/sections/basic-customization/#configuring-with-cson)结构和不可重复性的更多信息，请参阅使用CSON进行配置。

** 更多信息 **

代码段落功能在[snippets](https://github.com/atom/snippets)包中实现。
有关更多示例，请参阅 `[language-html](https://github.com/atom/language-html/blob/master/snippets/language-html.cson)` 和 `[language-javascript](https://github.com/atom/language-javascript/blob/master/snippets/language-javascript.cson)`包中的代码段。