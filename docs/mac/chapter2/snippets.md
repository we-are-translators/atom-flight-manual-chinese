# 代码片段

代码片段是从快捷方式快速生成常用代码语法的一种非常强大的方法。

这个想法是,你可以键入`habtm`之类的东西，然后按<kbd>Tab</kbd>键，它将扩展为`has_and_belongs_to_many`。

许多Core和Community包都捆绑了自己特定的代码段。例如，为HTML语法突出显示和语法提供支持的`language-html`包附带了许多片段，可以创建许多您可能想要使用的HTML标记。如果你在Atom中创建一个新的HTML文件，您可以键入html然后按<kbd>Tab</kbd>键它将扩展为：

```
<html>
  <head>
    <title></title>
  </head>
  <body>

  </body>
</html>
```

它还会将光标定位在标题标记的中间，以便你可以立即开始填写标记。许多片段都有多个焦点，你也可以通过<kbd>Tab</kbd>键移动它们 - 例如，在此HTML片段的情况下，一旦填写了标题标签，您可以按<kbd>Tab</kbd>键，光标将移动到中间身体标签。

要查看当前打开的文件类型的所有可用片段，请在命令选项板中选择“Snippets：Available”。

![https://flight-manual.atom.io/using-atom/images/snippets.png](https://flight-manual.atom.io/using-atom/images/snippets.png)

你也可以使用模糊搜索通过在选择框中键入来过滤此列表。选择其中一个将执行光标所在的片段（或多个光标）。

## 创建自己的代码片段

所以这很酷，但如果有什么语言包不包含或者你编写的代码是自定义的呢？幸运的是，添加自己的代码片段非常容易。

`％USERPROFILE％\.atom`目录中有一个名为`snippets.cson`的文本文件，其中包含启动Atom时加载的所有自定义代码段。你还可以通过选择*文件*>*代码段*菜单轻松打开该文件。

## 片段格式

那么让我们来看看如何编写代码片段。基本代码段格式如下所示：

```
'.source.js':
  'console.log':
    'prefix': 'log'
    'body': 'console.log(${1:"crash"});$2'
```

最左边的键是这些片段应该处于活动状态的选择器。确定这应该是什么的最简单方法是转到要为其添加代码段的语言的语言包，并查找“范围”字符串。

例如，如果我们想添加一个适用于Java文件的代码片段，我们将在Settings视图中查找`language-java`包，我们可以看到Scope是`source.java`。然后顶级片段密钥将是一个句点前面的（就像CSS类选择器一样）。

![https://flight-manual.atom.io/using-atom/images/snippet-scope.png](https://flight-manual.atom.io/using-atom/images/snippet-scope.png)

下一级别的密钥是代码段名称。这些用于在代码段菜单中以更易读的方式描述代码段。你可以随心所欲地命名它们。

在每个代码段名称下面都是一个`前缀`，它应该触发代码段，并在触发代码段时插入正文。

每个`$`后跟一个数字是制表位。一旦触发了片段，按<kbd>Tab</kbd>键即可循环切换制表位。

具有相同编号的制表位将创建多个光标。上面的示例将一个`日志`片段添加到JavaScript文件中，这些文件将扩展为：

```
console.log("crash");
```

最初选择字符串`“crash”`并再次按Tab键将光标放在`;`

>与CSS选择器不同，代码段密钥只能在每个级别重复一次。如果同一级别存在重复键，则只读取最后一个键。有关更多信息，请参阅使用CSON配置。

## 多行代码片段体

对于较大的模板，您还可以使用`"""`来使用[CoffeeScript多行语法](https://coffeescript.org/#strings)：
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

正如您所料，有一个片段可以创建片段。如果您打开一个片段文件并键入snip然后按<kbd>Tab</kbd>键，则会插入以下文本：

```
'.source.js':
  'Snippet Name':
    'prefix': 'hello'
    'body': 'Hello World!'
```

只是填补那个坏孩子，你自己有一个片段。一旦保存文件，Atom应该重新加载片段，您将立即尝试它。

## 多行代码片段的资源

你可以在下面看到在`snippets.cson`文件中为同一范围包含多个片段的格式。只需在作用域密钥中包含其他代码段的代码段名称，前缀和正文密钥：

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

同样，有关CSON密钥结构和不可重复性的更多信息，请参阅使用[CSON进行配置](https://flight-manual.atom.io/using-atom/sections/basic-customization/#configuring-with-cson)。

## 更多信息

代码片段功能在代码[片段](https://github.com/atom/snippets)包中实现。

有关更多示例，请参阅[language-html](https://github.com/atom/language-html/blob/master/snippets/language-html.cson)和[language-javascript](https://github.com/atom/language-javascript/blob/master/snippets/language-javascript.cson)包中的代码段。

